# 机器人定位挑战赛分析 / 第 027 卷

[本对话目录](README.md) | [总目录](../../README.md) | [Raw](https://raw.githubusercontent.com/weiyang02520-ops/dg202611-conversation-archive/main/conversations/first/part-027.md) | [上一卷](part-026.md) | [下一卷](part-028.md)

## 消息 1444 / assistant / 分段 2 of 2

~~~~text
=======
二十四、禁止测试框架绕过真实节点
==================================================

重点自查：

synthetic injector只能伪造“传感器输入”。

禁止直接伪造这些算法输出：

/dg/gnss/quality
/dg/lidar/quality
/dg/fusion/status
/dg/navigation/status
/dg/relocalization/status

Evaluator只能订阅这些输出。

如果发现测试框架直接发布上述状态：

立即修正测试框架。

==================================================
二十五、CMD_VEL安全
==================================================

继续使用：

/dg/test_cmd_vel

不得出现比赛链publisher写：

/cmd_vel

每个场景启动时检查一次。

每个场景结束时再检查一次。

真实机器人：

NO。

==================================================
二十六、测试
==================================================

新增兼容性测试和可视化工具测试以后：

运行 validation package 测试。

运行现有DG核心测试。

必须报告：

compatibility tests
validation framework tests
existing DG core tests

已有核心测试不得回归。

==================================================
二十七、提交规则
==================================================

如果所有工作完成：

查看：

git diff f7676ee3d9f53bae33d384f5bb506eaa144df095..HEAD

确认：

没有：

rosbag
CSV运行结果
PNG运行结果
build
install
log
大型文件
用户私有配置

提交代码只包含：

DiagnosticStatus兼容性修复
相关回归测试
可视化配置/工具
synthetic framework必要修正

建议commit：

fix(test): continue DG synthetic runtime validation

如修改内容较合理，也可以拆成：

fix(ros2): normalize diagnostic status levels

feat(test): add synthetic validation visualization

最多两次清晰commit。

不要 push。

==================================================
二十八、最终报告
==================================================

完成后停止并返回：

PREVIOUS_HEAD:
f7676ee3d9f53bae33d384f5bb506eaa144df095

WORK_BRANCH:

NEW_COMMITS:

DIAGNOSTIC_LEVEL_ROOT_CAUSE:

DIAGNOSTIC_LEVEL_RUNTIME_TYPE:

FILES_CHANGED:

CORE_ALGORITHM_LOGIC_MODIFIED:
YES/NO

COMPATIBILITY_FIX:
files:
functions:
behavior:

COMPATIBILITY_TESTS:
passed:
failed:

BUILD:
result:

DG_CORE_TESTS:
passed:
failed:

VALIDATION_FRAMEWORK_TESTS:
passed:
failed:

S01:
result:
expected:
observed:
important_transitions:
artifact:
plots:

S02:
result:
expected:
observed:
important_transitions:
artifact:
plots:

S03:
result:
expected:
observed:
important_transitions:
artifact:
plots:

S04:
result:
expected:
observed:
important_transitions:
artifact:
plots:

PASS_CRITERIA_REVIEW:
S01:
S02:
S03:
S04:

DIRECT_DG_OUTPUTS_FAKED_BY_INJECTOR:
YES/NO

ROSBAGS:
S01:
S02:
S03:
S04:

CSV:
S01:
S02:
S03:
S04:

TIMELINE:
S01:
S02:
S03:
S04:

SUMMARY:

VISUALIZATION:

RVIZ_CONFIG:
path:
tested:
result:

LIVE_MONITOR:
implemented:
command:
fields:

STATIC_PLOTS:
S01:
S02:
S03:
S04:

HISTORICAL_FAILED_S01_PLOTS:
result:

HEADLESS_SUPPORTED:
YES/NO

CMD_VEL_SAFETY:
real_cmd_vel_used:
test_topic:
result:

REAL_ROBOT:
NO

GAZEBO:
NO

S05_S08:
NOT_RUN

OFFICIAL_20CM_CLAIM:
NO

OFFICIAL_FEATURE_95_CLAIM:
NO

OFFICIAL_RELOCALIZATION_95_CLAIM:
NO

SYNTHETIC_RESULTS_LABELLED:
YES/NO

GIT_STATUS:

BLOCKERS:

DISCOVERED_CORE_ALGORITHM_ISSUES:

NEXT_RECOMMENDED_STEP:

==================================================
二十九、停止条件
==================================================

以下任一情况立即停止：

- 兼容性修复需要改变核心算法语义；
- S01出现新的核心算法阻塞；
- 发现测试框架必须绕过真实节点才能PASS；
- 出现真实 /cmd_vel 输出风险；
- 发现真实机器人连接；
- 需要Gazebo；
- 需要ZED/CUDA/TensorRT；
- 需要修改S05～S08；
- 需要伪造算法状态；
- 开始包装20cm/95%/>95%指标；
- 工作范围开始明显扩大。

==================================================
三十、本轮最终目标
==================================================

完成：

DiagnosticStatus.level ROS2 Humble兼容修复

↓

重新完整运行 S01

↓

运行 S02

↓

运行 S03

↓

运行 S04

↓

得到：

真实ROS2合成注入
+
真实算法节点输出
+
rosbag
+
samples.csv
+
timeline.csv
+
PASS/FAIL
+
RViz配置
+
实时状态监控
+
静态曲线图

然后停止。

不进入 Gazebo。

不进入 S05～S08。

等待外部审核。

提示词结束。

这次你直接整段丢给 Codex 就行。

这轮如果顺利，结束以后你第一次应该能真正“看到”整个系统：一边 RViz 看雷达和位姿，一边终端看 GNSS / LiDAR / Fusion 状态切换，跑完再直接打开 PNG 看曲线。memcite
~~~~

## 消息 1445 / user / 分段 1 of 1

~~~~text
{
  "content_type": "image_asset_pointer",
  "asset_pointer": "sediment://file_000000005c4c8209ae89f400636f14ea?shared_conversation_id=6a9b88ba-085c-83e9-b27b-7cbb97a8c954",
  "size_bytes": 128027,
  "width": 2048,
  "height": 694,
  "metadata": {
    "sanitized": true
  }
}
这个是老师给的，不得不说，老师还是知识面广很多啊
~~~~

## 消息 1447 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "model_editable_context",
  "model_set_context": ""
}
~~~~

## 消息 1451 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "reasoning_recap",
  "content": "思考了 17s"
}
~~~~

## 消息 1452 / assistant / 分段 1 of 1

~~~~text
对，老师这句话其实一下把“项目层次”说清楚了。

你前面一直在想的是：**怎么把比赛要求的几个难点解决掉**，所以自然会盯着 GNSS 失效、LiDAR 退化、主动重定位这些“局部问题”。

老师现在给你的视角更像是：

**先有一套完整、常规、能跑的定位主干，再把创新定位手段作为增强层叠上去。**

也就是：

> 常规定位体系  
> ＋  
> 创新定位手段  
> ＋  
> 故障感知与切换机制  
> ＝  
> 完整的“复杂环境多源导航定位系统”

这比单纯堆几个创新算法更像一个比赛项目。

你老师给的那份材料里，思路确实铺得很广：从事件相机、毫米波雷达、IMU、双目，到偏振光、光学导航、磁场导航、仿生感知等，核心一直是在找“北斗拒止 + 视觉/反光退化时还能提供约束的独立信息源”。例如文档后面已经明确提出要从“被动抗干扰”转向“主动寻求多重绝对基准”。fileciteturn233file0L280-L290

而且里面进一步把这些创新传感器分成“高频相对运动估计”和“绝对位置/航向基准”两类，再通过分层融合连接起来。fileciteturn233file0L421-L427

### 这对我们现在的项目特别有用

因为你其实**已经有“常规定位主干”了**：

轮速  
＋ IMU  
→ 二维 EKF

LiDAR  
＋ 地图  
→ AMCL / Scan-to-Map

GNSS/RTK  
→ 外部绝对位置源

Nav2  
→ 导航执行

所以我们不是从零开始。

现在比赛新增的：

GNSS质量门控  
LiDAR退化检测  
动态候选处理  
多源自适应融合  
Navigation Health  
主动重定位  
安全控制权仲裁

其实就是第一层创新增强。

老师现在又提供了一堆**更高一级的创新定位候选**：

事件相机、毫米波雷达、ToF、偏振光、光学导航……

这就相当于把我们的技术池一下扩大了。

---

但这里我反而要帮你**踩一下刹车**。

老师的“思路层次”我非常赞同，**但那份 Word 里面很多具体技术结论本身是 AI 生成的，不能直接全部当真**。

比如里面有些表述非常绝对：

> “毫米波雷达提供绝对可信的距离约束”  
> “信息几何是目前最优解”  
> “这套方案可以提升一个量级”

这种话拿来启发方案可以，正式比赛方案里不能直接照搬。

文件自己其实也反复标了：

> “本回答由 AI 生成，内容仅供参考，请仔细甄别。” fileciteturn233file0L322-L332

所以老师真正给你的财富，我觉得不是“这37页里的每一句技术结论”，而是：

**他告诉你要把视野从‘三个比赛算法’扩大成‘完整多源定位体系’。**

这点非常关键。

### 我现在对项目的理解也会随之调整

以前我们大致想的是：

> GNSS门控  
> → LiDAR退化  
> → 多源融合  
> → 主动重定位

现在更合理的是：

> **基础定位层**  
> 轮速 + IMU + LiDAR + AMCL + GNSS/RTK
>
> ↓
>
> **创新感知层**  
> 根据最终选型加入毫米波 / 事件视觉 / ToF / 其他异构定位源
>
> ↓
>
> **质量感知层**  
> 判断每个传感器现在到底可不可信
>
> ↓
>
> **自适应融合层**
>
> ↓
>
> **定位健康监测**
>
> ↓
>
> **主动重定位**
>
> ↓
>
> **安全导航**

这样，“创新定位”就不是孤零零地摆在那里，而是成为整个定位体系的一部分。

而且这和官方赛题要求的“**系统化定位方法与整体解决方案**”会更贴。

---

现在最重要的是：**别看到老师列了十几个方向，就全部往机器人上装。**

那样项目肯定炸。

我们后面真正需要做的是从老师这份材料里筛：

**哪些值得真的加到 DG-202611，哪些只适合写调研，哪些纯粹作为未来方向。**

比如毫米波雷达可能工程价值很高；事件相机很前沿但开发成本明显更高；偏振光导航可能很有创新性，但环境限制和硬件获取都要验证；某些所谓“光学导航”更要先查真实论文、设备可获得性和比赛周期。

所以这份材料我觉得非常值得认真拆。

**不是“全部实现”，而是从里面挑出最适合你这台参赛机器人的 1～2 个增强方向。**

老师确实给你打开了一个比我们之前更大的技术视角。这个对后面开发和方案书都会产生影响。memcite
~~~~

## 消息 1453 / user / 分段 1 of 1

~~~~text
{
  "content_type": "image_asset_pointer",
  "asset_pointer": "sediment://file_00000000102c82099b24f5a22a056ba0?shared_conversation_id=6a9b88ba-085c-83e9-b27b-7cbb97a8c954",
  "size_bytes": 52240,
  "width": 2038,
  "height": 300,
  "metadata": {
    "sanitized": true
  }
}
还有这些
~~~~

## 消息 1457 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "reasoning_recap",
  "content": "思考了 15s"
}
~~~~

## 消息 1458 / assistant / 分段 1 of 1

~~~~text
对，这份第二个 Word 我看了。它和老师前面给你的那份“创新定位方案”其实正好是一对：

**这份 = 常规定位方法库**  
**上一份 = 创新定位方法库**

而老师那句话“把我这里面的方案结合一些常规的定位，就是整体的定位了”，现在就非常清楚了。

这份常规方案主要把轮式机器人定位分成几类：轮速编码器、IMU、光流等相对定位；GNSS/RTK、UWB、Wi-Fi、RFID 等绝对/信标定位；LiDAR/视觉 SLAM 等环境感知定位；以及二维码、磁钉等人工地标，最后通过 EKF、因子图等做多传感器融合。fileciteturn234file0L91-L120

而且这里有一个对我们挺有用的点：**光流/地面纹理定位**。它相当于在机器人底部装“光电鼠标”，直接看地面相对运动，所以轮子打滑时，它还能提供一条独立的相对位移信息。fileciteturn234file0L248-L260 但它自己也会受地面纹理、光照、高度变化影响，而且依然会累积漂移，所以更适合当辅助里程计，而不是主定位。fileciteturn234file0L262-L277

所以老师现在给你的整体思路，我觉得可以理解成三层：

**第一层：成熟常规定位底座**

你们现在实际上已经有不少：

轮速编码器  
+ IMU  
+ 2D EKF  
+ LiDAR  
+ AMCL / Scan-to-Map  
+ GNSS/RTK

这层负责“正常情况下稳定工作”。

**第二层：常规冗余定位补充**

候选包括：

光流/地面纹理  
UWB  
视觉里程计  
人工地标等。

这层解决类似：

轮子打滑、GNSS不可用、某个传感器暂时异常。

**第三层：比赛创新定位**

就是老师前面那份文件里的：

事件相机  
毫米波雷达  
ToF  
偏振光  
以及我们已经做的质量感知、自适应融合、退化检测、主动重定位等。

这层才负责比赛要求的：

**BDS中断 + LiDAR退化 + 动态/反光干扰 + 定位失效恢复。**

这样架构就完整了。

---

不过我现在更确定一件事：

**我们不能看到一个定位方法就往车上加一个。**

比如这份里面：

UWB确实好用，但需要布基站；  
二维码/磁钉很稳定，但场景依赖严重；  
Wi-Fi成本低，但定位精度和稳定性一般；  
光流很便宜、容易做，但主要解决打滑，不解决全局定位；  
视觉SLAM很好，但反光/弱纹理恰恰是本赛题的困难之一。

所以真正需要的是：

> **把“所有可选定位方法”做成候选池，然后挑最值得加的，而不是全装。**

我目前对你们项目的初步判断是：

**现有的轮速 + IMU + LiDAR + GNSS/RTK 继续作为基础；**

常规方案里，**光流值得认真评估**，因为成本低、代码开发难度相对小，而且能够新增一条与轮速独立的相对运动观测；

创新方案里，**毫米波雷达非常值得优先研究**，因为它和比赛的“LiDAR/视觉退化”场景互补性比较强；

事件相机、偏振光这些可以作为更高创新度候选，但需要先查论文、硬件价格、SDK、ROS2支持以及比赛周期，不能直接拍脑袋上。

所以老师这两份东西放在一起之后，我们现在其实已经可以做一个非常有价值的东西：

**《DG-202611定位技术候选矩阵》**

把：

轮速、IMU、GNSS/RTK、2D/3D LiDAR、视觉、光流、UWB、毫米波、ToF、事件相机、偏振光……

全部按：

“解决什么问题 / 是否已有硬件 / 开发难度 / 成本 / 对本赛题价值 / 是否值得实现 / 放基础层还是创新层”

筛一遍。

这样我们下一步新增开发就不会乱。

如果你要，我建议下一步就先做这个筛选，**先不改代码**；把老师两份方案 + 你现在机器人已有能力放在一起，最后只选出最值得真正开发的 1～2 个新定位方向。memcite
~~~~

## 消息 1459 / user / 分段 1 of 1

~~~~text
可以
~~~~

## 消息 1460 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "model_editable_context",
  "model_set_context": ""
}
~~~~

## 消息 1476 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "thoughts",
  "thoughts": [
    {
      "summary": "整理筛选矩阵",
      "content": "我正在依据两份教师资料与现有机器人配置，比较传感器的可获得性、研究价值、融合难度和成本，重点筛选能形成明确创新点、又适合当前平台验证的方向。",
      "chunks": [],
      "finished": true
    },
    {
      "summary": "整理筛选矩阵",
      "content": "",
      "chunks": [],
      "finished": true
    }
  ],
  "source_analysis_msg_id": "28b1d627-2792-4f70-9966-0f52e25a7ef9"
}
~~~~

## 消息 1477 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "thoughts",
  "thoughts": [
    {
      "summary": "比较三维定位方向",
      "content": "我正在补充高度约束的分析：现有双目相机可提供视觉惯性三维里程计，毫米波雷达则能避开部分光学反射问题。前者适合快速落地，后者更具创新性，但硬件与融合开发成本更高。",
      "chunks": [],
      "finished": true
    },
    {
      "summary": "比较了三维定位方向",
      "content": "",
      "chunks": [],
      "finished": true
    }
  ],
  "source_analysis_msg_id": "b813313c-6e2a-4ebb-8c28-d6837741e504"
}
~~~~

## 消息 1478 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "thoughts",
  "thoughts": [
    {
      "summary": "已搜索当前文档内容",
      "content": "",
      "chunks": [],
      "finished": true
    }
  ],
  "source_analysis_msg_id": "29a3f301-7fa2-4c04-9af7-42efda1c49e4"
}
~~~~

## 消息 1479 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "reasoning_recap",
  "content": "思考了 58s"
}
~~~~

## 消息 1480 / assistant / 分段 1 of 1

~~~~text
可以。把老师两份材料和我们现在这台参赛样机放到一起筛以后，我的结论反而比刚才更明确了：

> **不要把老师列的东西全做。真正值得你们继续开发的，优先只有两条：**
>
> **① 把现有 ZED 2i 真正接入定位，形成 3D/VIO 定位通道；**
>
> **② 重点评估并考虑增加毫米波雷达，作为 LiDAR/视觉退化时的异构兜底。**

光流可以作为第三优先级的小增强。其他很多方法更适合写进方案书调研，而不是现在真做。

### DG-202611 定位技术候选矩阵

| 技术 | 你们当前情况 | 主要解决什么 | 对本赛题价值 | 实现难度 | 我的判断 |
|---|---|---|---:|---:|---|
| 轮速编码器 | ✅ 已有 | 高频相对位移 | ★★★ | 低 | 保留基础层 |
| IMU | ✅ 已有 | 姿态、短时运动 | ★★★★ | 低 | 保留基础层 |
| 轮速+IMU EKF | ✅ 已有 | 连续局部里程计 | ★★★★ | 已完成 | 保留 |
| GNSS/RTK | ✅ 已有 | 室外绝对位置 | ★★★★★ | 已有 | 必须保留 |
| 2D LiDAR | ✅ 已有 | SLAM/AMCL/地图匹配 | ★★★★★ | 已有 | 核心 |
| GNSS质量门控 | ✅ 软件POC | BDS异常识别 | ★★★★★ | 已完成POC | 核心创新 |
| LiDAR退化感知 | ✅ 软件POC | 识别几何退化 | ★★★★★ | 已完成POC | 核心创新 |
| 自适应多源融合 | ✅ 软件POC | 坏数据降权 | ★★★★★ | 已完成POC | 核心创新 |
| 主动重定位 | ✅ 软件POC | 定位失效后自救 | ★★★★★ | 已完成POC | 核心创新 |
| **ZED 2i VIO/3D里程计** | **🟡 硬件已有，未进定位链** | **补充XYZ、视觉惯性运动信息** | **★★★★★** | **中** | **优先开发①** |
| **毫米波雷达** | ❌ 暂无 | **LiDAR/视觉退化时独立距离、速度信息** | **★★★★★** | **中高** | **优先研究②** |
| 下视光流/地面纹理 | ❌ | 轮胎打滑时独立平面里程计 | ★★★ | 低~中 | 第三优先级 |
| 3D LiDAR | ❌ | 3D几何、Z方向定位 | ★★★★ | 中高+成本 | 有预算再考虑 |
| UWB | ❌ | 室内绝对位置 | ★★★ | 中 | 更适合实验真值/辅助 |
| ToF | ❌ | 近距离深度 | ★★☆ | 中 | 暂不优先 |
| 事件相机 | ❌ | HDR、强明暗变化、快速运动 | ★★★★ | **很高** | 调研亮点，暂不实装 |
| 偏振光罗盘 | ❌ | GNSS拒止时绝对航向 | ★★★ | 高 | 调研/后续研究 |
| 光学导航 | ❌ | 外部绝对位置 | ? | 很高 | 先核验真实性/可获得性 |
| 磁场指纹 | ❌ | 无光环境定位 | ★★ | 中高 | 不匹配当前主线 |
| 超声波 | ❌ | 无光近距感知 | ★★ | 中 | 不值得主攻 |
| PINN纯惯导 | ❌ | 极端情况下辅助惯导 | ★★☆ | 高 | 研究方向，不适合当前周期 |

老师第二份材料其实也明确把轮速、IMU、光流归入相对定位，把GNSS/UWB等作为绝对定位，把LiDAR/视觉SLAM归入环境感知定位，最后再通过EKF或因子图进行多源融合。fileciteturn234file0L102-L120

其中光流最大的价值确实是**绕开车轮打滑**：它直接观察地面纹理移动，但会受到纹理、光照、离地高度和累计漂移限制，所以更适合当辅助里程计。fileciteturn234file0L248-L277

老师第一份创新方案里真正值得我们重视的，也是“让不同物理原理的传感器互相兜底”，尤其反复提到了事件视觉、毫米波、IMU等异构观测，再做质量判断和融合。fileciteturn235file3L113-L128

## 为什么我把 ZED 2i 提到第一优先级

这个其实是我们之前忽略得比较严重的一块。

**因为你们车上已经有它。**

不用重新买硬件。

而现在官方指标里最危险的问题之一就是：

> **平面和高程定位精度都要优于20 cm。**

但我们目前主定位链基本是二维：

`轮速 + IMU → 2D EKF`

`2D LiDAR → AMCL / Scan-to-Map`

这对 XY 没问题，**Z方向天然很弱。**

ZED 2i 本身有：

双目
+
IMU

所以最自然的开发方向就是：

```text
ZED 2i
   ↓
Stereo + IMU
   ↓
VIO / Visual Odometry
   ↓
x y z + roll pitch yaw
   ↓
质量评价
   ↓
进入我们的多源融合
```

这样你不是重新发明一个系统，而是给现在的：

```text
GNSS
LiDAR
Wheel
IMU
```

增加：

```text
Visual-Inertial
```

这非常合理。

而且将来材料可以形成：

> **GNSS绝对定位 + LiDAR地图定位 + VIO三维相对定位 + 轮速/IMU高频状态传播**

这就真的开始有“整体定位体系”的味道了。

---

# 第二优先：毫米波雷达

这个我觉得很可能是老师给你最大的启发。

为什么？

因为比赛专门针对：

**BDS中断**
+
**LiDAR退化**
+
**动态目标/异常反射**

这时候最怕的是：

> 我们所有定位信息其实来自差不多一种物理机制。

毫米波雷达和LiDAR/视觉的物理机理差别非常大。

老师材料提出的也是利用毫米波在光学感知受扰时继续提供距离/速度约束，再与IMU等融合。fileciteturn235file4L151-L165

所以未来可以形成：

```text
                    GNSS/RTK
                       │
                       ▼
Wheel ──┐
IMU ────┤
LiDAR ──┤
ZED VIO ├──→ Quality Assessment
Radar ──┘             │
                       ▼
              Adaptive Fusion
                       │
                       ▼
              Localization Health
                 ↙             ↘
             正常导航        主动重定位
```

这就比：

> “GNSS坏了以后主要靠LiDAR”

强太多了。

---

## 光流为什么只排第三

老师这份材料里的光流其实挺实用。

尤其是：

```text
编码器说：
机器人前进了1米

光流说：
地面实际只移动0.3米
```

那很可能：

> **轮子打滑了。**

这种能力对电力巡检机器人其实挺现实。老师材料也把它定位成对轮式编码器的补偿手段，而不是全局定位替代品。fileciteturn234file0L20-L40

而且它便宜。

但是它没直接打到比赛最核心的：

**BDS中断 + LiDAR退化**

所以我不会现在让它抢毫米波和VIO的开发时间。

---

# 有几个看起来特别高级的，我反而不建议现在搞

### 事件相机

创新性：

**★★★★★**

工程风险：

**★★★★★**

老师材料自己也承认这类方案目前工程难度非常高，部分方向仍偏实验室研究。fileciteturn235file7L247-L257

它不是装上摄像头然后调用一个SDK就完事。

还有：

事件流处理  
事件特征  
时间同步  
标定  
VIO  
融合  
训练/模型

你们现在去开这条线，很可能比赛结束了还在调相机。

所以：

> **方案书调研可以重点写，实机暂缓。**

---

### 偏振光

思路很漂亮：

> GNSS没了还能提供绝对航向。

但问题是它依赖天空偏振模式，对建筑遮挡、树荫、室内都会受到限制，老师材料自己也提到了环境依赖。fileciteturn235file6L225-L232

对你们电力巡检场景不是没价值，但现在不值得优先。

---

### “清华光学导航”这一类

**先不要碰。**

这里甚至不是开发难不难的问题。

我要先把老师文档里关于：

设备是否公开可获得  
具体论文是什么  
有没有可购买终端  
是否真的适合地面机器人

逐项查清楚。

因为老师文件本身也是AI调研产物，而且明确带有“内容仅供参考”的提示。fileciteturn235file1L62-L70

这一类可以写“前沿研究方向”，但绝不能现在就画成我们的工程方案。

---

# 最终我建议的参赛定位体系

这样一整理，我现在比较喜欢这个版本：

```text
第一层：基础连续定位

轮速编码器
       +
IMU
       ↓
Local Odometry
```

↓

```text
第二层：多种定位观测

GNSS/RTK ─── 绝对位置

2D LiDAR ─── 地图定位

ZED 2i ───── 3D视觉惯性里程计

毫米波 ───── 异构距离/速度约束（候选）
```

↓

```text
第三层：质量感知

GNSS Quality
LiDAR Quality
VIO Quality
Radar Quality

        ↓

统一 Confidence
```

↓

```text
第四层：

Fault-aware
Adaptive Fusion
```

↓

```text
第五层：

Localization Health
```

↓

如果健康：

```text
Nav2继续导航
```

如果失效：

```text
主动停机
   ↓
主动旋转
   ↓
获取新观测
   ↓
Scan-to-Map / Visual / Radar candidate
   ↓
多帧验证
   ↓
恢复定位
   ↓
继续导航
```

### 这才是老师说的：

> **“整体的定位。”**

---

## 所以我建议现在不要改变 Codex 正在跑的第三轮A

现在那套：

**GNSS + LiDAR + Fusion + Health**

继续把 S01～S04 跑通。

它是我们的基础验证链。

之后，我建议不要直接冲 Gazebo，而是加一个很短的：

### “第四轮前技术选型阶段”

我去真正查：

**ZED 2i VIO**
和
**适合ROS2机器人的毫米波雷达**

包括：

有没有成熟ROS2驱动、GitHub项目、数据格式、Jetson能不能跑、能不能进现在的融合框架、硬件大概什么等级、开发周期。

然后我们再决定：

> **要不要真正增加毫米波硬件。**

但 **ZED 2i 定位通道，我现在已经比较倾向于做**，因为硬件都在车上了，不把它利用起来确实有点浪费，而且它正好有机会补我们目前最难受的 **Z方向定位链**。memcite
~~~~

## 消息 1481 / user / 分段 1 of 1

~~~~text
{
  "content_type": "image_asset_pointer",
  "asset_pointer": "sediment://file_00000000f54c820b8d8b4d022029bdae?shared_conversation_id=6a9b88ba-085c-83e9-b27b-7cbb97a8c954",
  "size_bytes": 60974,
  "width": 2048,
  "height": 385,
  "metadata": {
    "sanitized": true
  }
}
把刚刚说的那些整理一下，我交给codex
~~~~

## 消息 1482 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "thoughts",
  "thoughts": [
    {
      "summary": "整理交接内容",
      "content": "我正在把需求整合成一段可直接交给 Codex 的说明：保留当前第三轮综合验证，不改变现有流程；同时补充传统定位、创新定位，以及之前北斗资料，统一纳入后续技术方案和验证规划。",
      "chunks": [],
      "finished": true
    },
    {
      "summary": "整理了交接内容",
      "content": "",
      "chunks": [],
      "finished": true
    }
  ],
  "source_analysis_msg_id": "dda5cd6b-84b2-4409-95d2-2c4b72bd236a"
}
~~~~

## 消息 1483 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "thoughts",
  "thoughts": [
    {
      "summary": "整合技术路线",
      "content": "我正在整理给 Codex 的交接说明：保持当前第三轮 A 阶段验证不变，补充 P3 光流/视觉里程计与多类传感器研究，并将传统定位、创新定位、北斗资料、质量评估、故障融合和短报文通信统一纳入后续方案。",
      "chunks": [],
      "finished": true
    },
    {
      "summary": "整合了技术路线",
      "content": "",
      "chunks": [],
      "finished": true
    }
  ],
  "source_analysis_msg_id": "fca2bdf6-635a-4b15-8ce3-1edb300a7389"
}
~~~~

## 消息 1484 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "reasoning_recap",
  "content": "思考了 27s"
}
~~~~

[本对话目录](README.md) | [总目录](../../README.md) | [Raw](https://raw.githubusercontent.com/weiyang02520-ops/dg202611-conversation-archive/main/conversations/first/part-027.md) | [上一卷](part-026.md) | [下一卷](part-028.md)
