# 机器人定位挑战赛分析 / 第 052 卷

[本对话目录](README.md) | [总目录](../../README.md) | [Raw](https://raw.githubusercontent.com/weiyang02520-ops/dg202611-conversation-archive/main/conversations/first/part-052.md) | [上一卷](part-051.md) | [下一卷](part-053.md)

## 消息 1993 / assistant / 分段 1 of 1

~~~~text
那更简单：**这 3 亿额度就把它当成 ZCode 里的“重体力工人”**，不要想着跨平台调度。

而且你现在正好有很适合它干的活。

我建议 **不要让 ZCode 的 GLM 和 Claude 同时修改当前 Round B 的同一份代码**，容易再次出现版本/工作树混乱。让 GLM 干“独立、可并行、可复核”的任务最合适。

现在最值得开的几类：

1. **Gazebo P0 前置审计**
   Claude 继续收尾 Round B，ZCode+GLM 先把仓库全部扫一遍：
   - URDF/Xacro
   - CAD/3MF/SLDASM
   - 轮径、轮距
   - LiDAR/IMU/ZED/RTK 安装位置
   - TF
   - controller 参数
   - 哪些参数可信、哪些只是占位
   - Gazebo 还缺什么
   
   最后输出一个：
   `REAL / REPO_CONFIRMED / CAD_DERIVED / PROVISIONAL / MISSING`
   参数表。

   **这个现在就能和 Claude 并行做，而且几乎没有冲突。**

2. **全仓库“事实对账”**
   让 GLM 疯狂扫：
   ```text
   README声称了什么
   ↓
   代码有没有
   ↓
   launch有没有接
   ↓
   config有没有配置
   ↓
   test有没有验证
   ↓
   哪些只是文档写了但实际上没实现
   ```
   这种任务 Flash 很合适，而且非常吃 token。

3. **测试审查/测试生成**
   每次 Claude 做完一轮，把 commit 丢给 GLM：
   - 找漏测
   - 找假 PASS
   - 找死代码
   - 找 schema 不一致
   - 找 topic/type 不匹配
   - 找“文件生成了但内容是假的”
   - 给出建议测试
   
   刚才的 `InputState`、空 plot、CJK `\b`、RViz 类型错误，本质上都特别适合这种模型长期扫。

4. **代码阅读员**
   大型 ROS2 包让 GLM 一份一份读，产出：
   ```text
   节点
   topic
   service
   parameter
   TF
   数据流
   启动关系
   失败模式
   ```
   以后我们就不用反复烧强模型上下文读一遍。

5. **给 Tech-Knowledge-Base 批量加工**
   只要资料已经在本地，ZCode 就可以让 GLM：
   - 提取知识
   - 去重
   - 分类
   - 打标签
   - 写 HOW_TO_USE
   - 项目对比
   - API 摘要
   
   最后再由强模型抽查。

### 现在我最推荐先让它干这个

**别碰 Round B 主代码。**

直接在 ZCode 里开 GLM-5.3-Flash，对当前机器人仓库做：

> **DG-202611 Gazebo P0 Simulation Asset & Parameter Audit**

因为 Claude 现在还在收尾 S05–S08，而 **Gazebo 就是下一阶段**。GLM 可以提前把物理模型需要的信息全部挖出来，等 Claude Round B 一结束，直接拿审计结果开 Gazebo，不用再花一天重新读仓库。

理想分工就变成：

```text
Claude Code
└── Round B 收尾 / 正式开发

ZCode + GLM-5.3-Flash
└── Gazebo资产审计
    ├── URDF
    ├── CAD
    ├── 参数
    ├── TF
    ├── 传感器
    ├── 缺失数据
    └── 仿真实施建议

我
└── 审核两边结果 + 决定下一步
```

这样 **ZCode 限定反而不影响价值**，3 亿额度照样能用得很爽。

而且第一刀我就会拿它去做 **Gazebo 前置审计**，这是现在收益最高、风险最低的用法。memcite
~~~~

## 消息 1994 / user / 分段 1 of 1

~~~~text
就是，我目前打算让他去找论文啥的
~~~~

## 消息 1995 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "reasoning_recap",
  "content": "思考了 14s"
}
~~~~

## 消息 1996 / assistant / 分段 1 of 1

~~~~text
对，这个用途最合适。**3 亿 GLM-5.3-Flash额度拿去做论文挖掘，比拿来写主代码划算得多。**

但别只给它一句“帮我找论文”，那样很容易得到一堆标题、摘要，最后没法直接服务方案书。建议把 ZCode 变成一条“文献流水线”。

第一轮让它**广撒网**，围绕你们比赛真正需要的方向搜：
- GNSS/北斗退化、遮挡、拒止与恢复
- UWB 定位、NLOS 检测、质量评估、GNSS/UWB 融合
- 毫米波雷达定位、Radar Odometry、Radar-LiDAR/IMU fusion
- 事件相机定位、Event-VIO、Event + Stereo/IMU
- LiDAR 退化检测、可观测性/几何质量评估
- 动态环境 LiDAR 特征过滤
- 自适应多传感器融合、fault-aware / uncertainty-aware fusion
- 主动重定位 / active localization
- 多传感器时间同步、外参标定
- Gazebo/仿真验证相关方法

先搜 **100～200 篇都没关系**，Flash额度多。

然后第二轮让另外一个 GLM Agent 做筛选，砍掉水文和重复文献，留下大约 **30～50 篇真正有用的**。优先级建议是：

`近5年高质量论文 > 经典基础论文 > 官方技术资料 > 普通会议/预印本`

第三轮才是最重要的：**逐篇抽性能数据**。每篇统一输出：

```text
论文标题
作者
年份
期刊/会议
DOI / arXiv / 官方链接

研究技术：
实验场景：
真实实验 / 仿真 / 公共数据集：
传感器配置：
硬件平台：
Ground Truth：
数据集：

指标名称：
指标定义：

定位误差：
RMSE：
ATE：
RPE：
成功率：
重定位时间：
其它指标：

测试条件：
退化条件：
实验限制：

论文原文支持的结论：

哪些数字可以用于我们的方案书：
哪些数字不能直接比较：

如果作为我们项目目标：
保守目标建议：
依据：
```

**尤其一定要求“数字 + 实验条件”绑定。**

比如一篇论文说：

> 定位误差 6.8 cm

绝对不能只记 `6.8 cm`。

必须一起记录：

> 室内 / UWB基站数量 / 场地大小 / 是否LOS / Ground Truth是什么 / 平均误差还是RMSE。

不然老师一问“别人在哪个条件下做到的”，就麻烦了。

第四轮让 GLM 专门做一张：

### PERFORMANCE BENCHMARK MATRIX

类似：

| 技术 | 论文 | 场景 | 指标 | 文献结果 | 条件 | 我们安全写到什么程度 |
|---|---|---|---|---|---|---|
| UWB | Paper A | 室内LOS | RMSE | xx cm | 4基站 | ≤xx cm目标 |
| UWB | Paper B | NLOS | RMSE | xx cm | 遮挡 | 不应宣称厘米级 |
| Radar | Paper C | 室外 | ATE | xx | 77GHz | 待实验 |
| Event-VIO | Paper D | HDR | ATE | xx | Dataset X | 可论证抗光照退化 |

这个东西对你们现在**特别重要**。

因为老师已经明确提醒你：

> **不要我们的性能数字写得比文献还离谱。**

有了这个表，以后我们定：
- `<20 cm`
- `>95%`
- 重定位成功率
- 退化条件下的性能

就有文献底线，而不是拍脑袋。

还有一个非常重要的规则，可以直接告诉 ZCode：

> **禁止凭记忆生成论文。每篇进入正式文献库的论文必须验证标题、作者、年份、venue 和 DOI/arXiv/出版社链接至少一项；无法验证的进入 REJECTED，不得进入正式引用库。**

尤其 GLM Flash 这种模型拿来大规模搜索很好，但**核心论文最终还是要二次核验**。

我的建议最后形成 5 个成果：

```text
01_RAW_LITERATURE_POOL.md
        100~200篇候选

02_FILTERED_CORE_PAPERS.md
        30~50篇

03_PERFORMANCE_BENCHMARK_MATRIX.csv
        性能数据核心表

04_CLAIM_EVIDENCE_MATRIX.md
        我们每一句技术结论对应哪篇论文

05_REJECTED_OR_UNVERIFIED.md
        假引用/查不到/质量差/不适用
```

然后真正写比赛方案时，只从 **02 + 03 + 04** 里拿东西。

这样你这 **3 亿 token 就不是“花掉了”**，而是能给 DG 项目做出一个以后写方案、答辩、实验设计都能反复用的**文献证据库**。

而且这活儿可以让 ZCode 的 GLM 同时开很多 Agent：**一个专攻 UWB、一个毫米波、一个事件相机、一个 GNSS退化、一个 LiDAR、一个融合、一个重定位，再安排一个专门查假引用和数字。** 这才真正能把 Flash 的额度优势发挥出来。memcite
~~~~

## 消息 1997 / user / 分段 1 of 1

~~~~text
给提示词吧
~~~~

## 消息 1998 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "reasoning_recap",
  "content": "思考了 10s"
}
~~~~

## 消息 1999 / assistant / 分段 1 of 3

~~~~text
执行位置：当前 ZCode 研究线程/工作区，请在本线程直接执行。  
本任务只负责公开文献检索、真实性核验、性能数据抽取与证据库建设，不修改 DG-202611 开发代码，不替代 Claude Code 开发线程。

任务名称：
DG-202611 REAL LITERATURE MINING + PERFORMANCE BENCHMARK DATABASE

项目背景：

我们正在参加 DG-202611“基于具身智能的无人平台多源导航定位技术”比赛。

当前项目主线是：

轮式里程计 + IMU
+
北斗/GNSS/RTK
+
LiDAR
+
ZED 2i 双目
+
UWB
+
毫米波雷达
+
事件相机
↓
时间同步 / 空间标定
↓
传感器质量评估
↓
异常检测
↓
动态降权 / 拒绝不可靠观测
↓
多源自适应融合
↓
定位健康评估
↓
主动重定位 / 恢复

当前事实口径：

1. 北斗/GNSS/RTK：已有。
2. IMU：已有。
3. LiDAR：已有。
4. ZED 2i：已有。
5. UWB：老师已确认有现成硬件，后续拿到实物继续验证。
6. 毫米波雷达：准备采购，目前不能视为已实机集成。
7. 事件相机：准备采购，目前不能视为已实机集成。
8. 偏振光导航 + 天基光学导航：只放未来展望。
9. ToF：不作为当前方案主线。
10. 光流 / 地面纹理定位：不作为当前方案主线。

特别注意：

老师明确要求：
“项目性能数字不能随便写，尤其不能在没有证据时写得比已有论文结果还激进。”

因此本任务最重要的目的不是“搜很多论文”，而是建立一个可以直接约束方案书性能声明和后续实验设计的真实文献证据库。

==================================================
一、核心原则
==================================================

必须遵守：

NO_FAKE_PAPER

NO_FAKE_DOI

NO_FAKE_METRIC

NO_UNVERIFIED_NUMBER

NO_NUMBER_WITHOUT_EXPERIMENT_CONDITION

NO_AI_MEMORY_AS_CITATION

每一篇进入正式文献库的论文，都必须至少验证：

标题
作者
年份
venue
以及以下至少一个：

DOI
出版社正式页面
IEEE / Springer / Elsevier / ACM 等正式页面
arXiv正式页面
作者/实验室正式页面

如果无法验证：

放入 REJECTED_OR_UNVERIFIED

绝不能混入正式参考文献。

不要因为模型“好像记得有这篇论文”就认为它存在。

==================================================
二、优先使用的来源
==================================================

优先：

IEEE Xplore
SpringerLink
Elsevier / ScienceDirect
ACM Digital Library
Wiley
MDPI（仅质量合适时）
Nature / Science 系列
Science Robotics
IJRR
T-RO
RA-L
T-IV
T-ITS
Sensors
IROS
ICRA
RSS
CVPR / ICCV / ECCV
NeurIPS 等确有相关工作的会议
arXiv 原始页面
Google Scholar
Semantic Scholar
Crossref
DBLP
大学/实验室官网
官方技术文档

对于硬件：

厂商官方 datasheet
官方开发文档
官方 SDK 文档

不要把：

CSDN
知乎
营销文章
二手转载
AI生成文章

作为正式性能依据。

可以用它们发现关键词，但最终必须追溯到原始论文或官方资料。

==================================================
三、并行研究方向
==================================================

如果 ZCode 支持并行 Agent / 子任务，请尽量并行执行。

至少分成以下研究组。

--------------------------------------------------
A. 北斗 / GNSS / RTK 退化、遮挡、拒止
--------------------------------------------------

重点：

GNSS denied
GNSS degraded
urban canyon
multipath
NLOS
RTK degradation
GNSS outage
GNSS recovery
BeiDou positioning
GNSS/INS integration
GNSS/LiDAR fusion
GNSS/UWB fusion

重点寻找：

定位误差
RTK FIX/FLOAT
遮挡情况下性能
恢复时间
异常检测
多路径/NLOS处理
质量指标

尤其关注：

HDOP
satellite count
fix type
differential age
carrier-to-noise ratio
innovation/residual

与动态降权/拒绝机制的关系。

--------------------------------------------------
B. UWB 定位
--------------------------------------------------

这是重点方向。

关键词：

UWB localization
UWB positioning
UWB ranging
UWB NLOS detection
UWB LOS/NLOS classification
UWB quality assessment
UWB/IMU fusion
UWB/GNSS fusion
UWB/LiDAR fusion
adaptive UWB fusion
robust UWB localization

重点回答：

1. UWB 在 LOS 环境下通常能达到什么精度？
2. NLOS 时误差会恶化到什么程度？
3. 常见的 UWB 质量判断指标是什么？
4. RSSI 是否足够？
5. CIR / first-path power / receive power / range residual 等有什么作用？
6. 如何检测覆盖边缘？
7. 什么情况下应该降权？
8. 什么情况下应该直接拒绝？
9. 基站数量、几何布局对定位精度有什么影响？
10. UWB 与 IMU/GNSS/LiDAR 融合的典型方法是什么？

尤其为老师提出的：

“UWB信号稳定时使用，不稳定或覆盖边缘时弃用”

寻找理论和实验依据。

--------------------------------------------------
C. 毫米波雷达定位
--------------------------------------------------

这是重点方向。

关键词：

mmWave radar localization
millimeter-wave radar odometry
radar inertial odometry
radar SLAM
4D radar odometry
77GHz radar localization
60GHz radar localization
radar LiDAR fusion
radar IMU fusion
radar visual fusion
radar localization adverse weather
FMCW radar odometry

重点区分：

普通测距雷达
毫米波点云雷达
4D imaging radar
automotive radar

必须说明不同设备能力差异。

重点回答：

1. 毫米波雷达能提供：
   range?
   velocity?
   angle?
   point cloud?
   Doppler?
2. 哪种雷达适合机器人定位？
3. 60 GHz 与 77/79 GHz 常见区别是什么？
4. 毫米波雷达在：
   黑暗
   雾
   烟尘
   视觉退化
   LiDAR退化
   中的优势是什么？
5. Radar Odometry 通常达到什么性能？
6. Radar + IMU / LiDAR 融合有哪些成熟路线？
7. 有哪些公开数据集？

不要直接给我们推荐采购型号，除非具有明确论文/官方依据。
本阶段主要解决技术路线。

--------------------------------------------------
D. 事件相机
--------------------------------------------------

这是重点方向。

关键词：

event camera localization
event-based visual odometry
event camera VIO
event inertial odometry
event stereo
event frame fusion
event camera HDR localization
event camera high-speed localization
event camera SLAM
event camera ZED fusion
event asynchronous sensor fusion

重点回答：

1. 事件相机真正优势是什么？
2. 哪些场景普通相机会退化但事件相机仍然有效？
3. HDR / 强反光 / 暗光 / 高速运动的论文证据。
4. Event-VIO 当前达到什么水平？
5. 事件相机和 frame camera 如何融合？
6. 是否通常需要 IMU？
7. 异步 event stream 如何与普通帧相机进行时间对齐？
8. 软件时间同步 vs 硬件 trigger 各有什么问题？
9. event accumulation / time surface / voxel grid 等常见表示是什么？
10. 哪些公开数据集最适合我们未来验证？

特别关注：

ZED 2i 为 frame-based stereo camera，
事件相机为 asynchronous event stream。

寻找：

multi-rate synchronization
timestamp alignment
hardware synchronization
trigger
latency compensation

相关文献。

不要把“同频”简单理解成两个传感器必须相同 FPS。

--------------------------------------------------
E. LiDAR 退化检测 / 几何质量
--------------------------------------------------

关键词：

LiDAR degeneracy detection
LiDAR localization degeneracy
LiDAR SLAM degeneracy
observability
geometric degeneracy
scan matching confidence
ICP degeneracy
LiDAR localization uncertainty
point cloud geometry quality

寻找：

走廊
平面
重复结构
稀疏特征
开放区域

等场景下退化问题。

重点：

eigenvalue
Hessian
condition number
observability
inlier ratio
matching score
residual
geometry score

寻找可以支撑：

“根据 LiDAR 几何质量动态调整融合权重”

的研究。

--------------------------------------------------
F. 动态环境 LiDAR
--------------------------------------------------

关键词：

dynamic object removal LiDAR SLAM
dynamic LiDAR localization
dynamic point cloud filtering
dynamic object detection SLAM
robust localization dynamic environment

重点：

行人
车辆
移动障碍物

如何影响 scan matching / localization。

寻找：

temporal consistency
semantic filtering
motion consistency
robust kernel

相关方法和性能。

--------------------------------------------------
G. 自适应 / 故障感知多传感器融合
--------------------------------------------------

这是核心方向。

关键词：

adaptive sensor fusion
fault-aware sensor fusion
quality-aware sensor fusion
uncertainty-aware fusion
robust multi-sensor fusion
dynamic covariance estimation
adaptive Kalman filter
innovation-based fault detection
measurement rejection
sensor fault detection localization
confidence-aware localization

重点回答：

1. 如何根据质量动态改变 covariance？
2. 如何进行 measurement rejection？
3. residual / innovation gating 如何工作？
4. Mahalanobis gating 的常见方式。
5. sensor health 如何量化？
6. fault detection + fusion 如何结合？
7. 权重动态变化是否有理论依据？
8. 什么时候应该“降权”，什么时候应该“彻底拒绝”？

重点为我们的：

GNSS
UWB
LiDAR
visual
radar

质量感知融合提供正式文献基础。

--------------------------------------------------
H. 主动重定位 / 恢复
--------------------------------------------------

关键词：

active relocalization
active localization
robot relocalization recovery
kidnapped robot recovery
active perception localization
localization recovery behavior
robot localization failure recovery
viewpoint selection localization

重点：

定位健康下降
主动停止
主动旋转扫描
寻找高质量候选
验证候选
恢复定位
恢复控制权

寻找与这种闭环恢复逻辑最接近的方法。

不要只找 image place recognition。

我们重点关注：

机器人“做动作”主动改善定位状态。

--------------------------------------------------
I. 多传感器时间同步与空间标定
--------------------------------------------------

关键词：

multi sensor time synchronization
camera lidar imu synchronization
event camera synchronization
hardware timestamp
software timestamp alignment
sensor latency calibration
temporal calibration
extrinsic calibration
LiDAR camera IMU calibration
radar camera calibration

重点：

GNSS
UWB
LiDAR
ZED
event camera
radar
IMU

异步、多频率传感器如何统一时间基准。

必须特别寻找：

timestamp offset
clock drift
latency
hardware trigger
PTP
PPS

以及：

extrinsic calibration

对融合误差的影响。

--------------------------------------------------
J. Gazebo / ROS2 多传感器仿真验证
--------------------------------------------------

这个方向优先级低于上述核心算法，但也要研究。

关键词：

ROS2 Gazebo sensor simulation
Gazebo GNSS simulation
Gazebo LiDAR simulation
Gazebo IMU simulation
Gazebo UWB simulation
radar simulation Gazebo
event camera simulator
ESIM event camera
ROS2 fault injection localization

重点：

如何构造：

GNSS outage
GNSS degraded
LiDAR degradation
UWB dropout
sensor noise
bias
latency
packet loss

只研究“仿真方法和可信边界”。

绝不能把仿真性能当实车性能。

--------------------------------------------------
K. 北斗短报文
--------------------------------------------------

比赛要求涉及北斗短报文原型集成/验证。

搜索：

BeiDou short message communication
BDS short message
BeiDou RDSS
BeiDou-3 short message
BDS emergency communication

重点：

工作机制
典型终端
消息长度
通信方式
定位与短报文如何协同
移动机器人应用案例

性能数字仍必须来自可靠来源。

==================================================
四、非核心方向
==================================================

偏振光导航
天基光学导航

只做一个很小的 FUTURE_OUTLOOK_RESEARCH。

最多收集：

5–10 篇高质量论文/官方资料。

不要让它抢占当前主线资源。

ToF
光流
地面纹理定位

本阶段不做主线文献挖掘。

==================================================
五、第一阶段：RAW LITERATURE POOL
==================================================

目标：

总候选文献不少于 120 篇。

建议：

150–250 篇。

不要为了凑数量降低真实性。

每一条至少记录：

ID
Title
Authors
Year
Venue
DOI
URL
Research Area
Keywords
Real / Simulation / Dataset
Potential Relevance
Verification Status

Verification Status 只能是：

VERIFIED
PARTIALLY_VERIFIED
UNVERIFIED
REJECTED

==================================================
六、第二阶段：核心论文筛选
==================================================

从 raw pool 中筛选：

40–60 篇高价值论文。

其中最终：

15–30 篇

作为 DG-202611 CORE EVIDENCE PAPERS。

筛选优先级：

1. 直接与项目方法相关。
2. 有真实实验。
3. 实验条件清楚。
4. Ground Truth 清楚。
5. 指标定义清楚。
6. 有具体数值。
7. 近 5 年工作。
8. 经典论文可保留。
9. 高质量 venue 优先。
10. 方法具有可复现性优先。

不要只按 citation count 排名。

==================================================
七、第三阶段：逐篇性能抽取
==================================================

对于进入核心库的论文，逐篇创建统一记录。

格式：

PAPER_ID:

TITLE:

AUTHORS:

YEAR:

VENUE:

DOI:

OFFICIAL_URL:

VERIFICATION:
VERIFIED / PARTIAL / REJECTED

TECHNOLOGY:

PROBLEM:

SENSORS:

PLATFORM:

ROBOT/VEHICLE:

REAL_OR_SIM:
REAL / SIMULATION / PUBLIC_DATASET

DATASET:

SCENARIO:

ENVIRONMENT:

DEGRADATION_CONDITION:

GROUND_TRUTH:

GROUND_TRUTH_ACCURACY:

METRIC_NAME:

METRIC_DEFINITION:

REPORTED_VALUE:

UNIT:

MEAN / MEDIAN / RMSE / MAX / PERCENTILE:

BASELINE:

IMPROVEMENT:

TEST_DURATION:

TEST_DISTANCE:

SAMPLE_COUNT:

LIMITATIONS:

FAILURE_CASES:

WHAT_THE_PAPER_ACTUALLY_SUPPORTS:

WHAT_THE_PAPER_DOES_NOT_SUPPORT:

SAFE_FOR_PROJECT_CITATI
~~~~

[本对话目录](README.md) | [总目录](../../README.md) | [Raw](https://raw.githubusercontent.com/weiyang02520-ops/dg202611-conversation-archive/main/conversations/first/part-052.md) | [上一卷](part-051.md) | [下一卷](part-053.md)
