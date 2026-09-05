# 机器人定位挑战赛分析 / 第 015 卷

[本对话目录](README.md) | [总目录](../../README.md) | [Raw](https://raw.githubusercontent.com/weiyang02520-ops/dg202611-conversation-archive/main/conversations/first/part-015.md) | [上一卷](part-014.md) | [下一卷](part-016.md)

## 消息 972 / assistant / 分段 1 of 1

~~~~text
这轮主体确实已经搭起来了，但我核完代码后，**还不能直接 PASS**。

远端 `main` 现在就是 `2782b3efa48d7e7960351a27adc106c63e23f6d5`，父提交为 `67e8358e...`，提交信息也正确，所以这次不是旧结果。fileciteturn148file0L1-L2

**Gate：CHANGES_REQUESTED——属于集成层最后一轮安全收尾，不需要推倒重来。**

主要有 4 个真实问题。

第一，统一 Navigation Health 目前会把**实际已经退化的 LiDAR 错判成 GOOD**。真实 LiDAR POC 发布了 `geometry_score`、`valid_ratio`、`temporal_status`，其中 `temporal_status` 实际只有 `NO_PREVIOUS_FRAME / COMPARED` 这类状态。fileciteturn164file0L1-L2 但 Navigation Health parser 不认识 `COMPARED`，于是只要 `valid_ratio > 0` 就直接判 GOOD，完全没有使用 `geometry_score`。fileciteturn152file0L1-L2 所以比如 `valid_ratio=0.9, geometry_score=0.05`，真实主动重定位会认为 LiDAR 很差，而统一导航状态却可能显示 GOOD。这必须修。

第二，**cmd_vel 仲裁器没有检查 `/dg/navigation/status` 是否过期。** Wrapper 只保存了最后一个 `_navigation_state`，没有 `navigation_status_received_at`。fileciteturn154file0L1-L2 这意味着 Health Node 如果在 `NOMINAL` 时崩了，只要 Nav2 还不断发送新速度，arbiter 就可能永远拿着旧 `NOMINAL` 继续放行导航。这个属于安全问题。

第三，`LOCALIZATION_SUSPECT` 现在也会走 Nav 命令。arbiter 逻辑只有 `RECOVERING` 切 recovery，`FAILED/MANUAL_REQUIRED` 输出零，其余状态全部走 NAV。fileciteturn146file0L7-L7 所以 AMCL 已经进入 `LOCALIZATION_SUSPECT` 时，机器人仍可能继续按照 Nav2 跑。我建议明确成：

`NOMINAL / DEGRADED / RECOVERED → NAV`

`LOCALIZATION_SUSPECT → ZERO`

`RECOVERING → RECOVERY`

`FAILED / MANUAL_REQUIRED → ZERO`

第四，Navigation Health 已经记录了 `_relocalization_received_at`，但实际 `_tick()` 根本没有用它做 freshness。fileciteturn153file0L1-L2 所以主动重定位节点死掉后，旧的 `NORMAL / RECOVERING / RECOVERED` 状态会一直保留。

另外还有一个 launch 小坑：集成 launch 的 `map` 默认是空字符串，但启用 Nav2 时又总是把这个空值传给原 `navigation.launch.py`；原导航 launch 本来是有自己的地图默认路径的。fileciteturn149file0L1-L2 fileciteturn157file0L1-L2 顺手一起修掉。

给 Codex 跑这一轮：

1. 仓库仍然是 `weiyang02520-ops/electric-power-inspection-robot`。Starting HEAD 必须是 `2782b3efa48d7e7960351a27adc106c63e23f6d5`。本轮只做 Navigation Integration hardening，不做 GNSS EKF、不做短报文、不做方案书、不做 3D LiDAR。

2. 修 LiDAR 真实协议映射。`navigation_health_core.py` 必须真正读取当前 `lidar_robust_node` 发布的 `geometry_score`。增加可配置 `min_lidar_quality`，默认与 Active Relocalization 当前阈值保持一致，建议 `0.2`。规则建议：无有效点或 diagnostic ERROR → REJECTED；`geometry_score < min_lidar_quality` → DEGRADED；达到阈值 → GOOD；stale → STALE。不能再用“只要 valid_ratio > 0 就 GOOD”。必须增加真实字段测试：`temporal_status=COMPARED, valid_ratio=0.9, geometry_score=0.05` 应得到 DEGRADED；`geometry_score=0.8` 才是 GOOD。

3. 给 cmd_vel arbiter 增加 Navigation Health freshness。`ArbiterInput` 增加 `navigation_status_received_at`，`ArbiterConfig` 增加 `status_timeout`。`cmd_vel_arbiter_node.py` 收到 `/dg/navigation/status` 时记录本地接收时间。只要 health status 从未收到或者已经 stale，无论 Nav/Recovery 命令多新，都必须输出零速度，reason 类似 `NAVIGATION_STATUS_STALE`。不要继续使用旧状态。

4. 明确状态到控制源的唯一映射：`NOMINAL / DEGRADED / RECOVERED` 允许 NAV；`LOCALIZATION_SUSPECT` 输出 ZERO；`RECOVERING` 只允许 RECOVERY；`FAILED / MANUAL_REQUIRED` 输出 ZERO；manual takeover 永远 ZERO。未知状态也必须 fail-safe ZERO，不能默认当 NAV。

5. 修 Active Relocalization status freshness。`navigation_health_node.py` 已有 `_relocalization_received_at`，真正使用它。给 `NavigationHealthInput` 增加 `relocalization_fresh` 或等价字段。主动重定位状态一旦曾经收到、随后超过 freshness timeout，就不能继续把旧 NORMAL/RECOVERING/RECOVERED 当当前事实。输出 `RELOCALIZATION_STALE`，整体至少进入 `LOCALIZATION_SUSPECT`。结合第4条，此时最终 `/cmd_vel` 应为零。启动阶段从未收到时可以设置合理 grace/UNKNOWN 语义，避免节点刚启动0.1秒就误报失败。

6. 修 competition launch 的 Nav2 边界。`enable_nav2:=false` 继续保持默认。启用 Nav2 时必须确保它在 competition launch 内启动并接受 `/cmd_vel → /cmd_vel_nav` remap。不要在注释或日志里暗示可以另外启动一个未 remap 的 Nav2；单独启动原 `navigation.launch.py` 会绕过 arbiter。地图参数也要修：不能 `enable_nav2:=true` 时默认把空字符串强制覆盖原 navigation launch 的正常默认地图。可以提供一个真实默认值，或者只有用户显式传 map 时才覆盖。

7. 增加纯 Python 测试，至少覆盖：真实 LiDAR `geometry_score` 低 → DEGRADED；高 → GOOD；零有效点 → REJECTED；Navigation Health status missing → cmd zero；status stale → cmd zero；fresh NOMINAL + fresh nav → nav；DEGRADED → nav；LOCALIZATION_SUSPECT → zero；RECOVERING → recovery；RECOVERED → nav；unknown state → zero；relocalization status stale → LOCALIZATION_SUSPECT；relocalization stale 最终导致 arbiter zero；health node死亡后的旧 NOMINAL 不能继续放行 nav；health node死亡后的旧 RECOVERING 不能继续放行 recovery；Nav2 enable 路径不会用空 map；competition launch 内最终仍只有 arbiter 负责 `/cmd_vel`。

8. 保留现有所有回归：Navigation Health 17、Cmd Arbiter 10、Integration 20、Active Relocalization 50、LiDAR 11、GNSS 13、其它 runnable。数字可以增加，不能减少已有覆盖。如果当前环境没有 ROS2/colcon，继续 `NOT_RUN`，不要安装整套 ROS2。

9. 不修改默认 bringup，不修改 `ekf.yaml`，不把 GNSS 塞进 EKF，不声明 20cm、95%、实机联调完成。

10. 完成后运行 `git diff --check`，commit 使用 `fix(navigation): harden integration health and command safety`，push main，然后 STOP。最终报告 Starting HEAD、Final HEAD、LiDAR geometry quality mapping、navigation status freshness guard、LOCALIZATION_SUSPECT zero、unknown state zero、relocalization freshness、Nav2 safe remap/default map、各测试数量、ROS2 Build、Default Bringup Modified、GNSS Integrated Into EKF、Real Robot Tested、20cm Proven、>95% Proven、Capability、Remote Verified。

这一轮过了之后，**统一导航定位集成层我就会给它正式收口**。然后下一大块才是赛题真正核心的那一步：让 GNSS 从现在的“质量旁路”变成实际参与定位估计的多源输入。memcite
~~~~

## 消息 973 / user / 分段 1 of 1

~~~~text
Starting HEAD: `67e8358eb3789e16d2959a8531f403579c76fe84`
Final HEAD: `2782b3efa48d7e7960351a27adc106c63e23f6d5`

Integration:

- competition launch: YES
- LiDAR quality wired: YES
- GNSS quality wired: YES
- AMCL health wired: YES
- Scan-to-Map wired: YES
- Active Relocalization wired: YES
- Unified navigation status: YES

Command Arbitration:

- nav source: YES
- recovery source: YES
- single final `/cmd_vel` output: YES
- stale source zero: YES
- switch guard: YES
- manual safety: YES

Tests:

- Navigation Health: 17 passed / 0 failed
- Cmd Arbiter: 10 passed / 0 failed
- Integration: 20 passed / 0 failed
- Active Relocalization Regression: 50 passed / 0 failed
- LiDAR Regression: 11 passed / 0 failed
- GNSS Regression: 13 passed / 0 failed
- Other Runnable: 23 passed / 0 failed

ROS2 Build: NOT\_RUN — ROS2/colcon unavailable
Default Bringup Modified: NO
GNSS Integrated Into EKF: NO
Real Robot Tested: NO
20 cm Proven: NO
Competition >95% Proven: NO
Capability: PARTIAL
Remote Verified: YES
~~~~

## 消息 974 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "model_editable_context",
  "model_set_context": ""
}
~~~~

## 消息 975 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "reasoning_recap",
  "content": "思考了 7s"
}
~~~~

## 消息 976 / assistant / 分段 1 of 1

~~~~text
这份还是上一轮“统一导航定位集成”的旧结果，已经审核过了。

关键是：

- Starting HEAD：`67e8358e...`
- Final HEAD：`2782b3ef...`

而我上一轮已经给了 **CHANGES_REQUESTED**，因为还剩 4 个安全收尾问题：

- LiDAR 的 `geometry_score` 还没真正进入统一 Health 判定
- `/dg/navigation/status` 过期后，arbiter 还可能继续放行旧状态
- `LOCALIZATION_SUSPECT` 目前还会放行 Nav 命令
- Active Relocalization 状态 freshness 还没真正纳入统一 Health

所以你下一轮正确的 Starting HEAD 必须是：

`2782b3efa48d7e7960351a27adc106c63e23f6d5`

提交信息应该是：

`fix(navigation): harden integration health and command safety`

如果你刚才没保存那份修复提示词，我可以直接再完整发你一遍。memcite
~~~~

## 消息 977 / user / 分段 1 of 1

~~~~text
Starting HEAD: `2782b3efa48d7e7960351a27adc106c63e23f6d5`
Final HEAD: `ce67efb95fec49a3dcece6c53f5356a9f615808a`

LiDAR geometry quality mapping: YES
Navigation status freshness guard: YES
`LOCALIZATION_SUSPECT` zero: YES
Unknown state zero: YES
Relocalization freshness: YES
Nav2 safe remap/default map: YES  

Tests:

- Navigation Health: 22 passed / 0 failed
- Cmd Arbiter: 17 passed / 0 failed
- Integration: 24 passed / 0 failed
- Protocols: 10 passed / 0 failed
- Active Relocalization: 50 passed / 0 failed
- LiDAR: 11 passed / 0 failed
- GNSS: 13 passed / 0 failed
- Other runnable: 23 passed / 0 failed

ROS2 Build: `NOT_RUN` — `colcon`、`rclpy` 不可用
Default Bringup Modified: `NO`
GNSS Integrated Into EKF: `NO`
Real Robot Tested: `NO`
20 cm Proven: `NO`  

> 95% Proven: `NO`
> Capability: `PARTIAL`
> Remote Verified: `YES`
~~~~

## 消息 990 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "thoughts",
  "thoughts": [
    {
      "summary": "检查集成细节",
      "content": "我正在核对相关文件、测试和启动配置，重点确认导航健康状态、重定位状态新鲜度、速度指令仲裁，以及地图路径重映射是否还存在隐蔽问题。启动阶段未收到状态时的语义尤其需要再确认。",
      "chunks": [],
      "finished": true
    },
    {
      "summary": "检查了集成细节",
      "content": "",
      "chunks": [],
      "finished": true
    }
  ],
  "source_analysis_msg_id": "225614ac-a2dc-4c4d-b8d5-67860d1a5139"
}
~~~~

## 消息 1009 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "thoughts",
  "thoughts": [
    {
      "summary": "审查导航健康重定位与EKF配置变更及测试",
      "content": "",
      "chunks": [],
      "finished": true
    }
  ],
  "source_analysis_msg_id": "5894fc46-d02a-4494-aea3-c42b6f68a5b4"
}
~~~~

## 消息 1010 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "reasoning_recap",
  "content": "思考了 3m 34s"
}
~~~~

## 消息 1011 / assistant / 分段 1 of 1

~~~~text
这轮可以正式收口。

**Gate：PASS_WITH_NOTES。**

我已经核了远端：`main` 当前就是 `ce67efb95fec49a3dcece6c53f5356a9f615808a`，父提交是 `2782b3ef...`，提交信息也正确。fileciteturn169file0L1-L2

上轮卡住的几个问题都真正修掉了：

- LiDAR 现在真实使用 `geometry_score`，低于阈值进入 `DEGRADED`，不再“有点就算 GOOD”。fileciteturn171file0L1-L2
- arbiter 增加 Navigation Health freshness，状态过期直接零速度。fileciteturn170file0L1-L2
- `LOCALIZATION_SUSPECT` → ZERO，未知状态 → ZERO；只有 `NOMINAL / DEGRADED / RECOVERED` 才允许 Nav，`RECOVERING` 只允许 recovery。fileciteturn170file0L1-L2
- Active Relocalization 状态现在真的做 freshness，旧状态过期会变 `RELOCALIZATION_STALE → LOCALIZATION_SUSPECT`。fileciteturn172file0L1-L2
- Nav2 地图默认值也不再是空字符串。fileciteturn167file0L7-L7
- 对应的 stale health、suspect zero、unknown zero、relocalization stale 等集成测试都有覆盖。fileciteturn174file0L1-L2

所以现在可以把这一阶段正式标记成：

**DG Navigation Integration POC = COMPLETE_POC**
**实际能力状态 = PARTIAL**

`PARTIAL` 不是代码集成没完成，而是还没有 ROS2 实际 build/launch、rosbag、真车和比赛指标验证。

有一个不阻塞的 note：启动后如果 Active Relocalization **从来一条状态都没发出来**，目前 Health 会暂时按“未收到，不宣称 stale”处理；代码甚至专门测试了这种语义。fileciteturn173file0L1-L2 真 ROS2 联调时要确认节点启动失败能否被 launch/runtime 监测。这不值得现在继续抠，留到 ROS 环境验证。

---

### 下一块：GNSS 真正进入多源定位

现在终于到了赛题核心的一步。

当前真实状态仍然是：

`轮速 + IMU → 原 EKF → 连续局部里程计`

GNSS目前只是 quality gate，**还没进入位置估计**。现有 `ekf.yaml` 也明确只融合轮速和 IMU，`world_frame=odom`。fileciteturn176file0L1-L2

下一步不要粗暴改现有 EKF。先做一个独立的 **DG Multi-source Fusion POC**。

给 Codex：

仓库：
weiyang02520-ops/electric-power-inspection-robot

Starting HEAD：
ce67efb95fec49a3dcece6c53f5356a9f615808a

继续 DG-202611 导航定位主线。

本轮目标：

让 GNSS 第一次真正参与“位置估计”，建立 competition-side Multi-source Fusion POC。

不要修改默认机器人 EKF，不破坏现有 odom->base_footprint 链。

一、总体架构

保留现有：

轮速 + IMU
→ 原 robot_localization EKF
→ local odometry

新融合层不要重新发明轮速/IMU EKF。

建议估计：

map -> odom 的全局修正

最终全局位姿：

map->odom
+
现有 odom->base

这样：

GNSS短时中断、LiDAR退化时
→ 仍靠现有 wheel+IMU odometry 连续传播

GNSS恢复或可靠LiDAR定位恢复时
→ 修正 map->odom

暂时不要发布正式 TF，先作为 side-channel POC。

二、新增纯 Python core

建议：

multisource_fusion_core.py

至少支持：

LOCAL_ODOM
GNSS_POSITION
LIDAR/MAP_POSE_CORRECTION

核心状态至少：

map_to_odom_x
map_to_odom_y
map_to_odom_yaw
position_uncertainty
yaw_uncertainty
fusion_mode

fusion_mode 至少：

INITIALIZING
NOMINAL
GNSS_AIDED
LIDAR_AIDED
DEAD_RECKONING
DEGRADED
REJECTED_UPDATE

三、GNSS坐标转换

不能把 latitude/longitude 直接当 x/y。

实现纯 Python WGS84：

geodetic
→ ECEF
→ local ENU

不要依赖联网安装第三方包。

必须有单元测试。

四、GNSS到地图坐标的对齐

这是重点。

GNSS ENU 和现有 `map` 不是天然同一个坐标系。

禁止直接假设：

ENU x = map x
ENU y = map y

实现明确的：

Map/GNSS Alignment

至少支持：

1. 显式配置 map_enu_yaw / offset
或
2. 使用多组“可信 map pose + accepted GNSS”配对样本估计2D刚体对齐

如果没有完成 alignment：

GNSS不得进入融合。

状态必须明确：

WAITING_ALIGNMENT

不要静默使用错误坐标。

五、融合更新

Local odometry负责连续运动传播。

GNSS GOOD：
允许修正 x/y。

GNSS DEGRADED：
提高测量不确定度或根据配置拒绝。

GNSS REJECTED / STALE：
禁止进入位置更新。

GNSS不得直接修正 yaw，除非有真实可靠航向来源。

LiDAR/AMCL/Scan-to-Map可靠：
允许修正 x/y/yaw。

LiDAR geometry degraded / stale：
禁止使用旧GOOD。

Scan-to-Map accepted result可以作为较强全局修正。

注意：

AMCL与Scan-to-Map都依赖LiDAR。

同一时刻不能把两者当两个独立传感器重复融合。

做 source arbitration：
优先新鲜 accepted Scan-to-Map；
否则使用健康 AMCL；
不能双算。

六、异常值保护

所有绝对修正必须有：

finite check
timestamp freshness
position residual gate
yaw residual gate
max correction step

大跳变不能一帧把机器人全局位置拉飞。

必要时：

REJECTED_UPDATE
+
reason

七、不确定度

不要求做复杂论文级EKF。

但必须有明确 uncertainty propagation：

有绝对可靠修正
→ uncertainty下降

仅 dead reckoning
→ uncertainty随时间/运动增加

GNSS和LiDAR同时不可用
→ DEAD_RECKONING / DEGRADED

不要伪造“20cm”。

八、ROS wrapper

新增：

multisource_fusion_node.py

输入使用仓库当前真实topic。

首先检查现有 robot_localization 实际输出topic，不能凭空猜。

GNSS使用：

/dg/gnss/accepted_fix
+
/dg/gnss/quality

地图定位候选使用：

/amcl_pose
以及/或
/scan_match_pose + /dg/relocalization/match_quality

输出至少：

/dg/fusion/odom
/dg/fusion/pose
/dg/fusion/status

暂时：

publish_tf=false

不要抢现有 map->odom 或 odom->base TF。

九、接入 competition integration launch

只修改：

dg_navigation_integration.launch.py

新增参数：

enable_multisource_fusion:=true

默认机器人 bringup完全不变。

GNSS这一次可以明确写：

“参与 DG fusion POC”

但仍不能写：

“GNSS已进入原机器人EKF”。

十、并发退化 synthetic test

这是本轮最重要的测试。

纯Python模拟至少：

1. 正常 local odom连续传播
2. GNSS GOOD修正x/y
3. GNSS REJECTED完全不更新
4. GNSS stale不更新
5. LiDAR可靠修正x/y/yaw
6. LiDAR degraded不更新
7. Scan-to-Map与AMCL不重复融合
8. GNSS突然跳点被拒绝
9. LiDAR pose大跳被拒绝
10. correction step有限
11. GNSS outage时继续dead reckoning
12. LiDAR degradation时GNSS仍可辅助
13. GNSS outage + LiDAR degradation同时发生
14. 两者同时失效时仍连续输出local odometry传播结果
15. 两者同时失效时uncertainty增长
16. GNSS恢复后uncertainty下降
17. LiDAR恢复后uncertainty下降
18. 未alignment时GNSS绝不能融合
19. alignment完成后GNSS才生效
20. 多源恢复过程中无NaN/inf
21. 时间倒退/旧measurement拒绝
22.完整场景：
NOMINAL
→ GNSS_OUTAGE
→ LIDAR_DEGRADED
→ CONCURRENT_DEGRADATION
→ DEAD_RECKONING
→ GNSS_RECOVERY
→ NOMINAL

十一、真实性

完成后允许写：

GNSS participates in DG multi-source fusion POC = YES

Continuous estimate during synthetic concurrent degradation = YES

但必须继续：

Real Robot Tested = NO
20 cm Proven = NO
Competition concurrent degradation metric Proven = NO

不要声称：

BeiDou 20cm完成
真实断星实验完成
真实LiDAR退化实验完成

十二、回归

保留：

Navigation Health 22
Cmd Arbiter 17
Integration 24
Protocols 10
Active Relocalization 50
LiDAR 11
GNSS 13
其它可运行测试

新增：

Fusion Core
Geodetic/ENU
Alignment
Concurrent degradation simulation
Protocol/static tests

ROS2缺失继续 NOT_RUN。

十三、提交

git diff --check

commit：

feat(fusion): add DG-202611 multisource localization poc

push main。

最后报告：

Starting HEAD
Final HEAD

Fusion:
local odometry propagation: YES/NO
GNSS position correction: YES/NO
LiDAR/map pose correction: YES/NO
source arbitration: YES/NO
GNSS alignment required: YES/NO
outlier rejection: YES/NO
bounded correction: YES/NO
uncertainty propagation: YES/NO

Concurrent Degradation:
GNSS outage continuity: YES/NO
LiDAR degradation continuity: YES/NO
concurrent degradation continuity: YES/NO
dead reckoning mode: YES/NO
recovery correction: YES/NO

ROS:
side-channel fusion outputs: YES/NO
publishes map->odom TF: NO
default EKF modified: NO
default bringup modified: NO

Tests:
Fusion Core: X/X
Geodetic/ENU: X/X
Alignment: X/X
Concurrent Degradation: X/X
Existing regressions: ...

Real Robot Tested: NO
20 cm Proven: NO
Capability: PARTIAL
Remote Verified: YES/NO

然后 STOP。

这一步完成之后，我们才算从“几个传感器都各自存在”，真正跨到“**多源定位系统**”这一层。后面再考虑 ZED 视觉里程计是否要加入，来增强官方要求的“BDS中断 + LiDAR退化并发”场景，而不是现在就乱加传感器。memcite
~~~~

## 消息 1012 / user / 分段 1 of 1

~~~~text
收到，`Navigation Integration hardening` 已正式收口，远端 HEAD 为：

`ce67efb95fec49a3dcece6c53f5356a9f615808a`

下一步按既定顺序进入 GNSS 多源输入，不再修改已收口的集成层。
~~~~

## 消息 1013 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "model_editable_context",
  "model_set_context": ""
}
~~~~

## 消息 1014 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "reasoning_recap",
  "content": "思考了几秒"
}
~~~~

## 消息 1015 / assistant / 分段 1 of 1

~~~~text
对，状态就按这个锁定。

当前基线：
`ce67efb95fec49a3dcece6c53f5356a9f615808a`

已收口：
`Navigation Integration hardening`

下一步唯一主线：
**GNSS 从“质量旁路”升级为“实际参与 DG 多源定位估计的输入”。**

也就是说，接下来不再回头改已经收口的 integration 层，除非后面的融合实现真的暴露出必须修的接口问题。

你现在可以直接把我上一条那份 **`feat(fusion): add DG-202611 multisource localization poc`** 提示词交给 Codex 跑。跑完把 Starting HEAD / Final HEAD 和结果贴给我，我继续按同样流程审核。memcite
~~~~

## 消息 1016 / user / 分段 1 of 1

~~~~text
Starting HEAD: `ce67efb95fec49a3dcece6c53f5356a9f615808a`
Final HEAD: `c2ea9b1460e1d460f5fb298c5a43e03b4af0ddcc`

Fusion:

- Local odometry propagation: YES
- GNSS position correction: YES
- LiDAR/map pose correction: YES
- Source arbitration: YES
- GNSS alignment required: YES
- Outlier rejection: YES
- Bounded correction: YES
- Uncertainty propagation: YES

Concurrent Degradation:

- GNSS outage continuity: YES
- LiDAR degradation continuity: YES
- Concurrent degradation continuity: YES
- Dead reckoning mode: YES
- Recovery correction: YES

ROS:

- Side-channel fusion outputs: YES
- Publishes `map→odom` TF: NO
- Default EKF modified: NO
- Default bringup modified: NO

Tests:

- Fusion Core: 21/0
- Geodetic/ENU: 3/0
- Alignment: 2/0
- Concurrent Degradation: 5/0
- Existing regressions: 170 passed / 0 failed
- ROS2-only tests: `NOT_RUN`，环境缺少 `rclpy`、`ament_index_python`、`colcon`

Real Robot Tested: NO
20 cm Proven: NO
Capability: PARTIAL
Remote Verified: YES
~~~~

[本对话目录](README.md) | [总目录](../../README.md) | [Raw](https://raw.githubusercontent.com/weiyang02520-ops/dg202611-conversation-archive/main/conversations/first/part-015.md) | [上一卷](part-014.md) | [下一卷](part-016.md)
