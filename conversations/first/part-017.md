# 机器人定位挑战赛分析 / 第 017 卷

[本对话目录](README.md) | [总目录](../../README.md) | [Raw](https://raw.githubusercontent.com/weiyang02520-ops/dg202611-conversation-archive/main/conversations/first/part-017.md) | [上一卷](part-016.md) | [下一卷](part-018.md)

## 消息 1084 / assistant / 分段 1 of 1

~~~~text
这轮核心逻辑基本已经到位了，但我核代码后还发现 **2 个值得现在就堵掉的小坑**。

远端 `main` 已确认是 `3922a3296575f58b2fe25a8121d4fb180727c3b3`，父提交是 `c2ea9b...`，提交信息正确。fileciteturn191file0L1-L2

**Gate：CHANGES_REQUESTED（最后一次很小的 runtime 收尾）。**

第一，launch 里的未配置 GNSS 参数现在是：

`default_value="nan"`

然后把 `LaunchConfiguration` 直接塞给 `multisource_fusion_node` 的 double 参数。fileciteturn194file0L1-L2

而 node 自己声明的是 `float("nan")` 类型。当前又没有 ROS2 环境真正 launch 过，所以这里存在真实的参数类型风险：launch 字符串 `"nan"` 可能不会按 double 传进去，导致默认未配置状态下节点启动就报参数类型不匹配。

不要赌 ROS 的隐式转换，直接显式类型化。

第二，core 末尾还有一个状态覆盖问题：

只要当前 tick 有 GNSS、alignment 没配置，就会强制：

`mode = WAITING_ALIGNMENT`

哪怕这一 tick 已经成功接受了 AMCL / Scan-to-Map，或者系统之前已经建立 global anchor。fileciteturn187file0L1-L2

于是可能出现：

`AMCL 已正常工作`
→ GNSS alignment 没配
→ 每来一个 GNSS fix
→ fusion status 突然跳成 WAITING_ALIGNMENT

位置没坏，但状态语义会乱。

其余上一轮的问题确实已经修掉了：duplicate 不再当倒退、首次 global anchor 能跳过普通 residual gate、GNSS 不再降低 yaw uncertainty，而且 mixed-frequency 测试也覆盖到了。fileciteturn187file0L1-L2 fileciteturn193file0L1-L2

直接给 Codex：

Starting HEAD：

3922a3296575f58b2fe25a8121d4fb180727c3b3

本轮只做 DG Multi-source Fusion 最后的 runtime/config semantics 修复。

不要增加新算法。

一、强制 launch 数值参数类型

在：

dg_navigation_integration.launch.py

对以下参数传给 multisource_fusion_node 时显式声明 float 类型：

gnss_origin_latitude
gnss_origin_longitude
gnss_origin_altitude
map_enu_yaw
map_enu_offset_x
map_enu_offset_y

优先使用：

launch_ros.parameter_descriptions.ParameterValue

例如等价形式：

ParameterValue(gnss_origin_latitude, value_type=float)

所有六个参数都处理。

目标：

默认未配置：

gnss_origin_latitude = NaN
gnss_origin_longitude = NaN
map_enu_yaw = NaN

仍能作为合法 double 参数进入 ROS node。

不要依赖字符串 "nan" 的隐式类型推断。

允许 launch argument 本身继续是字符串，因为 ROS launch argument 本质如此，但传 Node parameter 时必须显式转换为 float。

增加静态测试确认：

ParameterValue 被使用
六个 alignment 参数都有 value_type=float

如果当前环境有 launch_ros，可以加运行级参数解析测试；
没有就 NOT_RUN，不安装ROS。

二、修 WAITING_ALIGNMENT 状态覆盖

当前错误语义：

只要 event.gnss != None 且 alignment None，
最终会强制 mode=WAITING_ALIGNMENT。

改成：

WAITING_ALIGNMENT 只表示：

系统尚未建立 global anchor，
当前希望依赖 GNSS，
但 GNSS alignment 尚未准备好，
并且本周期没有其它可信 global source 完成 anchor。

以下情况不得被 WAITING_ALIGNMENT 覆盖：

accepted_source == SCAN_MATCH
accepted_source == AMCL
global_anchored == true
当前已经处于 LIDAR_AIDED

如果系统已经通过 AMCL / Scan-to-Map 建立 global anchor，
而 GNSS alignment 尚未配置：

fusion仍正常工作。

只加入 reason：

GNSS_ALIGNMENT_UNAVAILABLE

或等价明确 reason。

不能把整体 mode 强制改成 WAITING_ALIGNMENT。

例如：

global anchored by AMCL
+
GNSS alignment absent
+
new accepted GNSS fix

结果应：

GNSS不融合
map pose不变
accepted_source不是GNSS
fusion mode保持 NOMINAL / LIDAR_AIDED / DEAD_RECKONING 等真实状态
reasons包含 GNSS_ALIGNMENT_UNAVAILABLE

三、补测试

至少：

1. unanchored + only GNSS + no alignment -> WAITING_ALIGNMENT
2. AMCL anchor + no GNSS alignment -> LIDAR_AIDED
3. 已AMCL anchor，之后新GNSS但无alignment -> NOT WAITING_ALIGNMENT
4. Scan-to-Map anchor + GNSS alignment missing -> NOT WAITING_ALIGNMENT
5. missing alignment GNSS never updates map pose
6. missing alignment reason明确存在
7. alignment配置完成后GNSS仍能正常GNSS_AIDED
8. 六个launch float参数使用显式ParameterValue

保留现有：

Fusion Core 32
Geodetic/ENU 3
Alignment 2
Concurrent Degradation 5
Mixed Frequency 1
Existing regressions 170

四、不做

不要修改：

默认EKF
默认bringup
Navigation Integration已收口逻辑
Active Relocalization
LiDAR算法
GNSS quality gate
TF ownership

不要加入视觉。

五、提交

git diff --check

commit：

fix(fusion): finalize alignment runtime semantics

push main。

最后输出：

Starting HEAD
Final HEAD

Launch Runtime:
typed GNSS origin params: YES/NO
typed ENU alignment params: YES/NO
unconfigured NaN supported: YES/NO

Alignment Semantics:
unanchored GNSS-only -> WAITING_ALIGNMENT: YES/NO
AMCL anchor not overridden: YES/NO
Scan-to-Map anchor not overridden: YES/NO
anchored system with missing GNSS alignment keeps operating: YES/NO
missing alignment GNSS update blocked: YES/NO
explicit alignment reason: YES/NO

Tests:
Fusion Core: X/0
Geodetic/ENU: X/0
Alignment: X/0
Concurrent Degradation: X/0
Mixed Frequency: X/0
Protocols: X/0
Existing regressions: X/0

ROS2 Build:
PASS/FAIL/NOT_RUN + reason

Original EKF Modified:
NO

Publishes map→odom TF:
NO

Real Robot Tested:
NO

20 cm Proven:
NO

Capability:
PARTIAL

Remote Verified:
YES/NO

然后 STOP。

**这两点过了，我会直接给 Multi-source Fusion POC 收口，不再继续抠这一层。** 后续只把“本地 odom 停更时 fusion 自身仍会持续发布新时间戳”记作 ROS 实机阶段的观察项，因为目前 fusion 仍是 side-channel、没有接管 TF，所以它不阻塞当前 POC。memcite
~~~~

## 消息 1085 / user / 分段 1 of 1

~~~~text
Starting HEAD: `3922a3296575f58b2fe25a8121d4fb180727c3b3`
Final HEAD: `ff421e25520913d13637c815a0fc0ead4ce0b051`

Launch Runtime:

- Typed GNSS origin params: YES
- Typed ENU alignment params: YES
- Unconfigured NaN supported: YES

Alignment Semantics:

- Unanchored GNSS-only → `WAITING_ALIGNMENT`: YES
- AMCL anchor not overridden: YES
- Scan-to-Map anchor not overridden: YES
- Anchored system with missing GNSS alignment keeps operating: YES
- Missing alignment blocks GNSS update: YES
- Explicit alignment reason: YES

Tests:

- Fusion Core: 41/0
- Geodetic/ENU: 3/0
- Alignment: 2/0
- Concurrent Degradation: 5/0
- Mixed Frequency: 1/0
- Protocols: 10/0
- Existing regressions: 170/0

ROS2 Build: `NOT_RUN` — 缺少 `rclpy`、`ament_index_python`、`colcon`
Original EKF Modified: NO
Publishes `map→odom` TF: NO
Real Robot Tested: NO
20 cm Proven: NO
Capability: PARTIAL
Remote Verified: YES
~~~~

## 消息 1086 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "model_editable_context",
  "model_set_context": ""
}
~~~~

## 消息 1092 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "thoughts",
  "thoughts": [
    {
      "summary": "审视实现细节",
      "content": "我正在检查主分支和提交上下文，确认改动是否完整。整体逻辑看起来合理，包括浮点参数解析与原因去重；不过我注意到 GNSS 不可用时，模式可能立即切换为 DEAD_RECKONING，需要确认这是否符合预期。",
      "chunks": [],
      "finished": true
    },
    {
      "summary": "审视了实现细节",
      "content": "",
      "chunks": [],
      "finished": true
    }
  ],
  "source_analysis_msg_id": "f79c62f3-1850-45f5-a70c-f637f632fb52"
}
~~~~

## 消息 1102 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "thoughts",
  "thoughts": [
    {
      "summary": "规划后续验证",
      "content": "我正在把多源融合 POC 视为已闭环，但还不会宣称视觉里程计已经接入。下一步更适合先审计现有相机链路，再明确可用输入、接口边界与真实降级行为。",
      "chunks": [],
      "finished": true
    },
    {
      "summary": "规划后续验证方案",
      "content": "",
      "chunks": [],
      "finished": true
    }
  ],
  "source_analysis_msg_id": "680fc5f9-532a-4e18-a063-997fa1cd178d"
}
~~~~

## 消息 1103 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "thoughts",
  "thoughts": [
    {
      "summary": "审查远程提交并搜索ZED视觉里程计配置",
      "content": "",
      "chunks": [],
      "finished": true
    }
  ],
  "source_analysis_msg_id": "99dff86e-3c09-443d-bf01-4f0b03abf1c1"
}
~~~~

## 消息 1104 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "reasoning_recap",
  "content": "思考了 1m 23s"
}
~~~~

## 消息 1105 / assistant / 分段 1 of 1

~~~~text
这轮可以正式收口了。

**Gate：PASS_WITH_NOTES。**

我核了远端，`main` 当前确实是 `ff421e25520913d13637c815a0fc0ead4ce0b051`，父提交是 `3922a329...`，提交信息 `fix(fusion): finalize alignment runtime semantics` 正确。fileciteturn197file0L1-L2

这次两个最后的问题都解决了：

- 6 个 GNSS/ENU 参数现在通过 `ParameterValue(..., value_type=float)` 显式传入，不再赌 `"nan"` 的隐式类型转换。fileciteturn196file0L7-L11
- 未配置 GNSS alignment 时，只在“尚未建立全局锚点、又只有 GNSS 可用”的情况下进入 `WAITING_ALIGNMENT`；已经由 AMCL / Scan-to-Map 建立的定位不会被覆盖。fileciteturn196file0L13-L19
- `GNSS_ALIGNMENT_UNAVAILABLE` 也已经成为明确状态原因，而不是偷偷失败。fileciteturn196file0L13-L19

所以现在正式记为：

**DG Multi-source Fusion Closed-loop POC = COMPLETE_POC**  
**Capability = PARTIAL**

这里的 `PARTIAL` 只剩现实验证层面：ROS2 build/launch、rosbag、真车、20 cm 指标。不是目前代码架构没闭环。

到这里，我们导航定位主线已经形成了：

`轮速+IMU连续传播`
→ `GNSS质量判定`
→ `LiDAR退化感知`
→ `AMCL / Scan-to-Map全局修正`
→ `多源融合`
→ `并发退化时Dead Reckoning`
→ `主动重定位`
→ `恢复`
→ `cmd_vel安全仲裁`

下一步我建议**先不要急着加视觉，也不要回头重写这些已经收口的模块**。

现在最值得继续做的是赛题第1项真正的算法强化：

**Fault-aware Adaptive Fusion（故障感知自适应融合）**

也就是把现在比较固定的：

`GNSS GOOD → 固定权重`
`GNSS DEGRADED → 固定权重`
`LiDAR GOOD → 固定权重`

升级成真正根据：

`HDOP / satellites / differential age / GNSS state`
+
`geometry_score / temporal_match_ratio / LiDAR state`
+
`AMCL covariance / Scan-to-Map score`

动态调整绝对修正可信度。

这样我们的技术路线才会从“几个源接起来了”，进一步变成：

**“根据卫星退化程度和LiDAR几何退化程度自适应改变多源定位权重”**

这会非常贴赛题。

给 Codex 的下一轮核心目标可以直接定为：

Starting HEAD：

`ff421e25520913d13637c815a0fc0ead4ce0b051`

任务：

`feat(fusion): add fault-aware adaptive measurement weighting`

要求暂时仍保持 side-channel，不改原 EKF、不发 `map→odom TF`，基于现有真实 GNSS/LiDAR/AMCL/Scan-to-Map diagnostic 字段计算 `measurement confidence / adaptive uncertainty`，并做并发退化 synthetic tests。

这一步做完以后，我们再判断是否值得把 **视觉里程计**作为第四路冗余源加进来，而不是现在为了“多一个传感器”就硬塞。memcite
~~~~

## 消息 1106 / user / 分段 1 of 1

~~~~text
你确定这些都是原来就做到了的，不是还没开始的那几个？
~~~~

## 消息 1109 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "model_editable_context",
  "model_set_context": ""
}
~~~~

## 消息 1110 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "thoughts",
  "thoughts": [
    {
      "summary": "核对实现状态",
      "content": "我先区分“原有基线”和“本轮新增”。并不是全部都原来完成：EKF、AMCL、GNSS驱动、雷达基础链路和候选匹配原本已有；LiDAR重定位闭环、GNSS融合、质量门控与参数调试仍属于新增或未完成部分。",
      "chunks": [],
      "finished": true
    },
    {
      "summary": "核对实现状态",
      "content": "",
      "chunks": [],
      "finished": true
    }
  ],
  "source_analysis_msg_id": "43738007-5897-4307-962d-42d572f77d97"
}
~~~~

## 消息 1120 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "thoughts",
  "thoughts": [
    {
      "summary": "核实现状功能并梳理导航地图扫描新增边界",
      "content": "",
      "chunks": [],
      "finished": true
    }
  ],
  "source_analysis_msg_id": "4f5b2040-8ed1-472a-b3aa-e3defff3f908"
}
~~~~

## 消息 1121 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "reasoning_recap",
  "content": "思考了 40s"
}
~~~~

## 消息 1122 / assistant / 分段 1 of 1

~~~~text
你这个质疑是对的。**我上一条把“原来就有的能力”和“这几轮刚做出来的能力”混在一起说了。**

准确分法应该是：

- **比赛开发开始前就已经有的**
  - 轮速 + IMU 的 EKF。旧基线 `e83d...` 里 `ekf.yaml` 就已经明确融合 `/odom + /imu/data`。fileciteturn198file0L1-L2
  - WTRTK980 GNSS 驱动，已经能发布 `/gps/fix` 和 `/gps/rtk_status`，但**当时没有参与定位融合**。fileciteturn199file0L1-L2
  - AMCL / Nav2 基础导航。
  - Scan-to-Map 局部匹配也已经有，但当时主要靠外部 `/initialpose` 触发，只是“给一个粗位置后帮你附近精修”，不是自动主动重定位。旧代码已经有 `refine_pose_near_seed()` 和 `/scan_match_pose`。fileciteturn200file0L1-L2

- **这几轮我们才刚刚做出来，现在已经进入仓库的**
  - LiDAR robust / 动态候选点 / geometry quality
  - GNSS quality gate
  - 自动 Active Relocalization
  - 自动 Scan-to-Map seed 闭环
  - Navigation Health
  - `/cmd_vel` 安全仲裁
  - competition navigation integration launch
  - **GNSS 真正参与多源定位的 side-channel Fusion POC**
  - GNSS outage + LiDAR degradation 的 synthetic dead-reckoning / recovery 流程

- **还没开始，或者至少还没有完成的**
  - 我刚刚提的 **Fault-aware Adaptive Fusion**：这就是**下一步候选任务，尚未开始**
  - ZED2i 视觉里程计加入融合：**没开始**
  - Fusion 正式接管 `map→odom TF`：**没做**
  - 原机器人 EKF 加入 GNSS：**没做，而且目前故意不做**
  - ROS2 真环境 build / launch 联调：**没验证**
  - rosbag / 真车实验：**没做**
  - 并发退化下真实 `<20 cm`：**没证明**
  - LiDAR 特征重复率 `≥95%`：**没证明**
  - 主动重定位成功率 `>95%`：**没证明**
  - 北斗短报文：**基本还没开始**

所以我上一条那句：

> “导航定位主线已经形成……”

正确理解应该是：

**“截至现在，通过这几轮开发，代码层的 POC 链已经形成。”**

不是：

**“这些本来机器人就已经全部实现了。”**

尤其是 **多源 Fusion、主动重定位闭环、GNSS quality gate、LiDAR robust、统一健康和控制仲裁**，全是我们刚才这一串 commit 一步一步补出来的。

而我最后提的 **Fault-aware Adaptive Fusion**，你要特别区分：**它还没开始，我是在提下一步。**

后面我会严格用三种标签说，避免再混：

**原有基线 / 本轮已实现 / 下一步未开始。** memcite
~~~~

## 消息 1123 / user / 分段 1 of 1

~~~~text
我和你说一遍目前的情况，你等会重述，我看你懂了吗，目前这个比赛是基于已有框架继续开发，我打算先把已有的都先写完计划书，之后再继续进行没有完成的部分，并把那部分通过ai进行完成，中间会需要数据，同样需要你帮忙进行，待大部分完成之后，再进行下一步的计划书填写
~~~~

## 消息 1124 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "reasoning_recap",
  "content": "思考了 7s"
}
~~~~

## 消息 1125 / assistant / 分段 1 of 1

~~~~text
我理解的是这样：

你现在不是要“先把整个比赛项目全部做完，再写计划书”，而是要分阶段推进。

第一阶段：**只基于现有框架和目前已经真实完成的东西，把计划书里现在能写的部分先全部写完。**  
已经做到了什么，就写什么；还没做到的部分只保留框架、目标或待补内容，不能提前写成已经完成。

第二阶段：等这一版计划书把“已有内容”吃干净以后，**再回到技术开发**。针对目前还没完成的赛题要求，一项一项继续做，主要让 AI/Codex 去实现，我负责帮你规划、审核、判断下一步。

第三阶段：开发过程中如果某些功能需要**实验数据、测试数据、参数、rosbag、真车结果等**，我们再专门解决数据问题。我可以帮你设计需要什么数据、怎么采、怎么测、怎么分析；可以用模拟数据做开发验证，但必须明确是模拟数据，**比赛最终指标不能伪造**。

第四阶段：等后面这些未完成部分已经完成了大部分，并且积累了新的代码、实验结果和真实数据以后，**再回到计划书进行第二轮补充**，把之前留空的技术模型、实验结果、性能分析、创新点等补进去。

所以整体节奏是：

**已有内容 → 先写计划书第一版 → 暂停写计划书 → 集中补技术和数据 → 大部分技术完成 → 再回来继续完善计划书。**

而不是我们刚才那样一直顺着代码往后开发，结果把“现在该先写的已有成果”跳过去。

这次我懂你的节奏了。
~~~~

## 消息 1126 / user / 分段 1 of 1

~~~~text
懂了吗，所以目前是什么情况
~~~~

## 消息 1127 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "model_editable_context",
  "model_set_context": ""
}
~~~~

## 消息 1128 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "reasoning_recap",
  "content": "思考了 6s"
}
~~~~

## 消息 1129 / assistant / 分段 1 of 1

~~~~text
懂了。**目前现在应该停在“第一阶段：把已有内容先写进计划书”这里。**

也就是说，**现在先不要继续让 Codex 往后做 Fault-aware Fusion、视觉融合、短报文之类的新功能。**

当前实际情况可以分成三块：

- **原来机器人就有的基础**：轮速+IMU EKF、AMCL/Nav2、WTRTK980 GNSS 驱动、2D LiDAR、基础 Scan-to-Map、ZED2i 等。
- **这几轮已经真实补完到代码里的比赛 POC**：LiDAR robust、GNSS quality gate、主动重定位闭环、统一 Navigation Health、cmd_vel 仲裁、Navigation Integration、GNSS 真正参与的 Multi-source Fusion POC。
- **还没做/没验证的**：自适应权重融合、视觉加入定位、北斗短报文、ROS2 真环境完整联调、真车/rosbag、20cm、95%特征重复率、>95%重定位成功率等。

所以**现在的下一步不是继续开发第三块**。

现在应该做的是：

**把第一块 + 第二块这些“已经有真实依据”的内容，尽可能完整地写进计划书第一版。**

比如现在已经可以认真写：
- 项目背景与赛题理解
- 现有研究基础与样机
- 总体技术路线
- 多源定位现阶段方案
- LiDAR退化处理方案
- 主动重定位方案
- GNSS质量判定与多源融合方案
- 系统集成架构
- 工程可行性
- 当前实验方法设计
- 当前已经能成立的创新点

而像：

`20cm实验结果`
`95%特征重复率`
`>95%重定位成功率`
`短报文实测`
`真车并发退化结果`

这些现在**不能硬写结果**，先留“待后续实验验证/待补充”。

等这一轮把“现在能写的计划书”基本写干净以后，才进入第二阶段：

**暂停计划书 → 继续补没做完的技术 → 需要数据时专门采集/设计/分析 → 大部分完成以后再回来补计划书。**

所以现在的准确状态一句话就是：

> **技术先暂时冻结在 `ff421e...`，接下来进入“基于当前真实成果完成计划书第一阶段”的工作。** memcite
~~~~

## 消息 1130 / user / 分段 1 of 1

~~~~text
已完成并推送。

Starting HEAD: `ff421e25520913d13637c815a0fc0ead4ce0b051`
Final HEAD: `c41adca7f9bddb4240a1a4855437518b36d9fe13`

Adaptive Weighting：

- GNSS 置信度：YES
- LiDAR 置信度：YES
- 自适应不确定度：YES
- GNSS/LiDAR/AMCL/Scan-to-Map 字段接线：YES
- 低质量测量降权：YES
- 异常值与残差门保留：YES
- 并发退化测试：YES
- 输出仍为 side-channel：YES

Tests：

- Fusion 相关测试：55 passed / 0 failed
- 既有可运行回归：170 passed / 0 failed
- ROS2 Build：NOT\_RUN（当前环境无 `colcon`）
- Real Robot Tested：NO
- 20 cm Proven：NO
-

> 95% Proven：NO

约束确认：

- Default EKF Modified：NO
- Default Bringup Modified：NO
- `map→odom` TF：NO
- 视觉/短报文/3D LiDAR：未加入
- `plan` 仓库：未修改

Capability：`PARTIAL`
Remote Verified：`YES`，，，，，，，看一下最新情况
~~~~

## 消息 1149 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "thoughts",
  "thoughts": [
    {
      "summary": "评估最新进展",
      "content": "我正在按“能否正常运行、是否符合验收目标、是否存在隐患”检查当前实现。整体方向已经覆盖GNSS、LiDAR质量评估、动态权重和诊断输出，但我还需要重点核对低质量数据是否被正确降权，以及节点集成是否完整。",
      "chunks": [],
      "finished": true
    },
    {
      "summary": "评估了最新进展",
      "content": "",
      "chunks": [],
      "finished": true
    }
  ],
  "source_analysis_msg_id": "bd935c7d-bb8d-4a10-9294-e5e251330292"
}
~~~~

## 消息 1152 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "thoughts",
  "thoughts": [
    {
      "summary": "评估感知融合提交差异与最新状态",
      "content": "",
      "chunks": [],
      "finished": true
    }
  ],
  "source_analysis_msg_id": "618cef09-508c-4990-a061-098d7c706ce5"
}
~~~~

## 消息 1153 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "reasoning_recap",
  "content": "思考了 1m 16s"
}
~~~~

## 消息 1154 / assistant / 分段 1 of 1

~~~~text
我看了最新仓库，这次情况很明确。

**Gate：PASS_WITH_NOTES。**

远端 `main` 已经是 `c41adca7f9bddb4240a1a4855437518b36d9fe13`，父提交就是 `ff421e...`，提交信息是 `feat(fusion): add fault-aware adaptive measurement weighting`。fileciteturn203file0L1-L2

而且这里要特别纠正我们前面那个分类问题：

**Fault-aware Adaptive Fusion 原先确实属于“还没开始的部分”。**
但现在这次 commit 已经把它实际做进仓库了，所以**从现在开始，它已经从“未开始”移动到了“本轮已实现”**，不能再把它当未来计划。

现在代码里真实有了：

- GNSS 根据 `HDOP / satellites / differential_age / GNSS state` 算置信度；
- LiDAR/地图定位根据 `geometry_score / temporal_match_ratio / AMCL covariance / Scan-to-Map score / inlier_ratio / mean_distance` 算置信度；
- 低置信度测量的修正量会变小；
- 自适应 position/yaw uncertainty 会随测量质量变化；
- 原来的 residual gate、outlier rejection、bounded correction 还保留着。fileciteturn205file0L7-L7

GNSS 上游也确实真实提供了 `satellites / hdop / differential_age / accepted / state` 等字段，所以不是凭空造了一套输入。fileciteturn208file0L1-L2

不过这个阶段仍然只能叫：

**Fault-aware Adaptive Fusion POC = COMPLETE_POC**
**实际比赛能力 = PARTIAL**

因为这些权重和 uncertainty 目前还是**启发式模型**，不是经过真实数据标定出来的正式统计协方差；同时 ROS2 build、真车和比赛指标也都没验证。

---

### 所以截至现在，真实状态应该重新整理成这样

**A. 原机器人本来就有**

轮速+IMU EKF、AMCL/Nav2、WTRTK980 GNSS 驱动、2D LiDAR、基础 Scan-to-Map、ZED2i 等。

**B. 为这个比赛已经实际开发完成到 POC 的**

LiDAR robust  
→ GNSS quality gate  
→ Active Relocalization  
→ Scan-to-Map 自动闭环  
→ Navigation Health  
→ cmd_vel 仲裁  
→ Navigation Integration  
→ GNSS Multi-source Fusion  
→ 并发退化 Dead Reckoning / Recovery  
→ **Fault-aware Adaptive Fusion**

最后这一项就是刚刚才完成的，不是原来机器人就有的。fileciteturn201file0L3-L7

**C. 现在还没有完成的**

主要还是：

- ROS2 真环境 build / launch
- rosbag 和真实数据
- 真车联调
- GNSS/LiDAR退化真实实验
- 20 cm 定位指标
- LiDAR ≥95% 特征重复率
- >95% 重定位成功率
- 北斗短报文
- 是否加入 ZED 视觉里程计作为进一步冗余源
- 后续真正的数据标定和参数优化

---

而按照你刚才重新明确的比赛推进方式，**现在不应该继续顺着 C 往下开发。**

现在最正确的动作是：

> **技术先冻结在 `c41adca...`，把 A+B 这些已经真实存在的东西，全部吃进计划书第一阶段。**

也就是说，现在我们终于回到你真正想做的节奏：

**已有内容已经比刚才又多了一项 Adaptive Fusion → 现在先集中写计划书 → 把当前能写的全部写完 → 然后才继续做 C 里的未完成部分。**

这次我不会再直接给你下一个开发任务了。memcite
~~~~

## 消息 1155 / user / 分段 1 of 1

~~~~text
说人话
~~~~

## 消息 1156 / assistant / 分段 1 of 1

~~~~text
现在就一句话：

**能做的软件部分，我们已经先做了一大批；现在别继续开发了，先把这些已经做出来的东西写进计划书。**

目前已经做好的包括：
- GNSS质量判断
- LiDAR退化处理
- 主动重定位
- 导航健康监控
- 速度仲裁
- 多源融合
- GNSS真正参与定位
- GNSS/LiDAR质量变差时自动降权
- 并发退化时继续靠里程计顶住

这些现在都是真实代码里有的。

还没做完的主要是：
- 真车测试
- ROS2真实环境联调
- 20cm指标
- 95%特征重复率
- >95%重定位成功率
- 北斗短报文
- 真实数据标定

所以现在正确顺序是：

**先暂停开发 → 把目前已有成果全部写进计划书 → 写到没东西可写了 → 再继续补剩下没做的部分。**

就这么简单。memcite
~~~~

## 消息 1157 / user / 分段 1 of 1

~~~~text
等会哈，你说的是刚刚在开发？
~~~~

[本对话目录](README.md) | [总目录](../../README.md) | [Raw](https://raw.githubusercontent.com/weiyang02520-ops/dg202611-conversation-archive/main/conversations/first/part-017.md) | [上一卷](part-016.md) | [下一卷](part-018.md)
