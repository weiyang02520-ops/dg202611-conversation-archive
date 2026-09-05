# 机器人定位挑战赛分析 / 第 054 卷

[本对话目录](README.md) | [总目录](../../README.md) | [Raw](https://raw.githubusercontent.com/weiyang02520-ops/dg202611-conversation-archive/main/conversations/first/part-054.md) | [上一卷](part-053.md) | [下一卷](part-055.md)

## 消息 2007 / user / 分段 1 of 1

~~~~text
所以阶段C已完成，现已停止，不改正文、不联系开发线，也未使用DSH。详细材料均为本地 Markdown/CSV/BibTeX，没有渲染大文件。

状态：PARTIAL
原因：60条来源和证据矩阵已完成，但核心论文多数为出版社/摘要级核验，尚未完成20篇全文实验细节精读；真实机器人指标仍无数据。

统计：

- 唯一来源：60
- VERIFIED：60
- 拒收记录：12类
- 核心论文卡片：20
- 经典文献：22
- 2019—2026文献：38
- 国内来源：19
- 国际来源：41
- 主题数量：A9、B6、C9、D6、E7、F7、G6、H3，候选I5，实验J2

主要判断：

- A GNSS/多源融合：强
- B 鲁棒融合与故障处理：充分，但参数标定仍缺
- C LiDAR退化：强
- D 动态目标处理：充分，但当前方案仍是几何/时序方法，不等同于完整语义识别
- E 主动重定位：方法依据充分，实车成功率未验证
- F VIO/视觉：可支撑ZED候选，不支撑高程20cm结论
- G 毫米波雷达：有研究依据，但必须设备、驱动和实测验证
- H 北斗：可支撑BDS定位与短报文边界、接口说明，不支撑短报文已完成

候选结论：

- ZED 2i VIO：强候选
- 毫米波/4D雷达：有潜力，待选型验证
- 光流/地面纹理：次级候选
- 3D LiDAR、UWB 3D：待工程评估
- Event、ToF、偏振、磁场、超声、PINN：研究储备或暂缓

当前不能写入正式成果的结论：

- 平面/高程优于20 cm
- 特征重复率≥95%
- 重定位成功率>95%
- ZED已解决高程
- 毫米波/Event/UWB已集成
- 北斗短报文已完成
- 二维EKF已成为三维GNSS融合
- 当前代码已完成目标机或实车闭环

文件位置：

- [阶段C目录]\(C:\\\Users\\\peng\\\Desktop\\\材料\\\DG-202611比赛材料文档\\\STAGE\_C\_真实文献研究\_v1.0)
- [来源总表]\(C:\\\Users\\\peng\\\Desktop\\\材料\\\DG-202611比赛材料文档\\\STAGE\_C\_真实文献研究\_v1.0\\\STAGE\_C\_MASTER\_SOURCE\_TABLE\_v1.0.md)
- [核心论文卡片]\(C:\\\Users\\\peng\\\Desktop\\\材料\\\DG-202611比赛材料文档\\\STAGE\_C\_真实文献研究\_v1.0\\\STAGE\_C\_CORE\_PAPER\_CARDS\_v1.0.md)
- [研究综合]\(C:\\\Users\\\peng\\\Desktop\\\材料\\\DG-202611比赛材料文档\\\STAGE\_C\_真实文献研究\_v1.0\\\STAGE\_C\_RESEARCH\_SYNTHESIS\_v1.0.md)
- [候选技术评价]\(C:\\\Users\\\peng\\\Desktop\\\材料\\\DG-202611比赛材料文档\\\STAGE\_C\_真实文献研究\_v1.0\\\STAGE\_C\_TECHNOLOGY\_CANDIDATE\_REVIEW\_v1.0.md)
- [不可支撑论点]\(C:\\\Users\\\peng\\\Desktop\\\材料\\\DG-202611比赛材料文档\\\STAGE\_C\_真实文献研究\_v1.0\\\STAGE\_C\_UNSUPPORTED\_CLAIMS\_v1.0.md)

当前状态：

- 正式第2章：暂不写，等待你审核证据库
- 第6—8章：可以用文献补强方法论，但不能补写实验结果
- 源章节文件：未修改
- 开发线：未联系
- DSH：未使用
~~~~

## 消息 2008 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "reasoning_recap",
  "content": "思考了 8s"
}
~~~~

## 消息 2009 / assistant / 分段 1 of 1

~~~~text
按**文档方向**重新看，这个 Stage C 我可以先给出明确审核结论：

**STAGE C = PASS WITH LIMITATIONS**  
不是失败，也不是最终完成；它已经完成了“真实文献池 + 第一轮证据筛选”，但还没完成“全文实验指标核验”。

不过在进入下一阶段前，有几处必须根据老师刚刚的新口径修正，**只修研究资产，不改正式正文**：

- `光流/地面纹理`：从“次级候选”移出当前方案，标为 `NOT_IN_CURRENT_SCOPE`。
- `ToF`：从研究储备里移出当前主线，标为 `NOT_IN_CURRENT_SCOPE`。
- `偏振光`：和天基光学导航一起归入 `FUTURE_OUTLOOK`。
- `UWB`：不能再简单写“待工程评估”。老师已明确确认**有现成硬件**，但是否完成机器人接入、ROS2读取和融合仍待验证。正确状态应是：  
  `HARDWARE_EXISTS / INTEGRATION_NOT_YET_VERIFIED`
- `毫米波雷达`：老师准备采购，当前状态是：  
  `PLANNED_PROCUREMENT / NOT_YET_INTEGRATED`
- `事件相机`：老师准备采购，当前状态也是：  
  `PLANNED_PROCUREMENT / NOT_YET_INTEGRATED`
- `Event` 不应该再和 ToF、磁场、超声一起笼统标“暂缓”，它现在已经是**确定准备实施的技术方向**。

因此，“当前不能写入正式成果”的这句：

> 毫米波/Event/UWB已集成

原则仍然正确，但最好拆开写成：

> UWB：已有项目硬件，但尚无充分证据证明已经完成当前机器人软硬件融合闭环。  
> 毫米波雷达：尚未采购/集成。  
> 事件相机：尚未采购/集成。

这样事实更准确。

然后下一阶段不要急着写第2章，也不要继续盲目扩大到几百篇论文。**现在最值钱的是把20篇核心论文往深里挖。**

下一阶段应该是：

**DOCUMENT_LINE — Stage D：Core Paper Full-Text & Performance Evidence Extraction**

核心目标只有三个：

1. 精读20篇核心论文的实验部分；
2. 把每一个关键性能数字和实验条件绑定；
3. 建立“我们的指标最多安全写到什么程度”的性能基准矩阵。

重点先压在：

**UWB → 毫米波雷达 → 事件相机/ZED时间同步 → 自适应融合 → GNSS退化 → LiDAR退化 → 主动重定位。**

尤其老师刚强调了“别把自己的指标写得比文献还离谱”，所以 Stage D 最重要的产物应该是这种表：

| 技术 | 文献 | 场景 | Ground Truth | 指标 | 文献结果 | 条件 | 可否与我们比较 | 我们当前能安全怎么写 |
|---|---|---|---|---|---|---|---|---|

并且继续严格区分：

`LITERATURE_REPORTED`  
`COMPETITION_REQUIREMENT`  
`PROJECT_DESIGN_TARGET`  
`SIMULATION_MEASURED`  
`REAL_ROBOT_MEASURED`

五种东西绝不能串。

还有一点：**仅凭这份汇报，我审核的是“Stage C 的工作状态和结论结构”，不是60个来源本身的真实性。** 真正要逐条审核文献是否引用正确，需要看到你列出来的那些 Markdown/CSV/BibTeX 内容。

所以目前可以正式冻结为：

**TASK_DIRECTION: DOCUMENT_LINE**  
**STAGE_C_STATUS: PASS_WITH_LIMITATIONS**  
**FORMAL_BODY_MODIFICATION: NO**  
**DEVELOPMENT_LINE_CONTACT: NO**  
**DSH_USAGE: NO**  
**NEXT_STAGE: FULL_TEXT_PERFORMANCE_EVIDENCE_EXTRACTION**

这条文档线目前走得是对的。下一步就该进入 Stage D，而不是开始写正文。memcite
~~~~

## 消息 2010 / user / 分段 1 of 1

~~~~text
提示词呢
~~~~

## 消息 2011 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "reasoning_recap",
  "content": "思考了 6s"
}
~~~~

## 消息 2012 / assistant / 分段 1 of 2

~~~~text
【任务方向】
DOCUMENT_LINE / 文档与证据研究方向

【执行位置】
当前线程《查看比赛提交要求》，请在本线程直接执行；本线程负责文档、文献、证据与 DSH 管理，不执行开发代码任务，不联系开发线。

【任务名称】
DG-202611 DOCUMENT STAGE D — 核心论文全文精读 + 性能证据抽取 + 安全指标边界

【当前状态】

Stage C 已完成并冻结：

STAGE_C_STATUS = PASS_WITH_LIMITATIONS

现有成果：

- 唯一来源：60
- VERIFIED：60
- 核心论文卡片：20
- 已建立来源表、证据矩阵、技术候选评价、不可支撑论点等研究资产
- 尚未完成 20 篇核心论文的全文实验细节精读
- 尚无真实机器人性能数据
- 正式正文未修改
- 开发线未联系
- DSH 未使用

本轮进入 Stage D。

本轮不是继续大规模搜标题，而是：

“少扩量，深挖核心论文，把性能数字和实验条件真正查清楚。”

==================================================
一、任务边界
==================================================

本轮允许：

1. 继续使用公开互联网检索论文。
2. 打开论文正式页面、PDF、Supplementary、作者页面。
3. 核验 Stage C 的 20 篇核心论文。
4. 必要时补充少量高价值论文。
5. 更新 Stage C 研究资产。
6. 新建 Stage D Markdown / CSV / BibTeX 文件。

本轮禁止：

1. 修改正式第1—12章正文。
2. 联系或指挥开发线。
3. 修改开发仓库。
4. 编造项目实验数据。
5. 把文献结果写成项目结果。
6. 使用 DSH。
7. 为了凑数字伪造论文、DOI、实验条件。
8. 将摘要级信息伪装成全文级核验。

FORMAL_BODY_MODIFICATION = NO  
DEVELOPMENT_LINE_CONTACT = NO  
DSH_USAGE = NO

==================================================
二、先修正 Stage C 中的最新项目事实口径
==================================================

仅修改研究资产中的技术状态，不改正式正文。

最新老师确认口径：

### 当前已有

- 北斗 / GNSS / RTK
- IMU
- LiDAR
- ZED 2i
- UWB 有现成硬件

UWB 当前正确状态：

HARDWARE_EXISTS = YES  
ROBOT_INTEGRATION_VERIFIED = NO  
ROS2_DATA_PATH_VERIFIED = NO  
FUSION_CLOSED_LOOP_VERIFIED = NO

因此不能再简单写：

“UWB待工程评估”

也不能写：

“UWB已完成集成”

正确表述：

“项目已有UWB硬件基础，后续需完成型号、接口、数据读取、质量指标和融合链路验证。”

### 准备实施

毫米波雷达：

PLANNED_PROCUREMENT = YES  
HARDWARE_EXISTS = NO / NOT_YET_CONFIRMED  
INTEGRATED = NO

事件相机：

PLANNED_PROCUREMENT = YES  
HARDWARE_EXISTS = NO / NOT_YET_CONFIRMED  
INTEGRATED = NO

Event Camera 不再与 ToF、磁场、超声等一起笼统归入“暂缓”。

### 不进入当前主线

ToF：

NOT_IN_CURRENT_SCOPE

光流：

NOT_IN_CURRENT_SCOPE

地面纹理定位：

NOT_IN_CURRENT_SCOPE

### 未来展望

偏振光导航  
天基光学导航

统一：

FUTURE_OUTLOOK

不要在本轮继续对这些非主线技术投入大量研究资源。

==================================================
三、Stage D 核心目标
==================================================

本轮最重要的不是“论文数量”，而是回答：

1. 别人在什么条件下做到了什么性能？
2. 用了什么硬件？
3. 什么 Ground Truth？
4. 指标到底怎么定义？
5. 是真实机器人、仿真还是公共数据集？
6. 是平均值、RMSE、95分位还是最好单次结果？
7. 在退化条件下性能恶化多少？
8. 这些结果能否与我们比赛指标直接比较？
9. 我们现在最多能安全写到什么程度？
10. 后续必须设计什么实验才能证明项目性能？

==================================================
四、核心论文全文精读
==================================================

优先处理 Stage C 已选出的 20 篇核心论文。

每篇必须尽量获得：

- 正式全文
- 作者公开版本
- arXiv
- Supplementary
- 官方实验表格/图

如果全文确实无法获得：

不要猜。

标记：

FULL_TEXT_AVAILABLE = NO  
VERIFICATION_LEVEL = ABSTRACT_OR_METADATA_ONLY

并降低证据等级。

只有真正读到 Methods / Experiments / Results 的论文，才允许：

FULL_TEXT_EXPERIMENT_VERIFIED = YES

==================================================
五、优先级调整
==================================================

P0：

1. UWB 质量评估 / NLOS / 降权 / 拒绝
2. 毫米波雷达定位 / Radar Odometry
3. 事件相机 + ZED / Frame Camera + IMU 时间同步
4. 自适应 / 故障感知多传感器融合

P1：

5. GNSS / 北斗退化
6. LiDAR 几何退化
7. 主动重定位
8. VIO / ZED 2i

P2：

9. 北斗短报文
10. 动态环境 LiDAR

非当前主线方向不得抢占主要时间。

==================================================
六、每篇核心论文统一抽取
==================================================

对每篇全文精读论文建立记录：

PAPER_ID:

TITLE:

AUTHORS:

YEAR:

VENUE:

DOI:

OFFICIAL_URL:

FULL_TEXT_SOURCE:

VERIFICATION_LEVEL:

FULL_TEXT_EXPERIMENT_VERIFIED:
YES / NO

TECHNOLOGY:

PROBLEM:

SENSORS:

EXACT_SENSOR_MODELS:

PLATFORM:

ROBOT_OR_VEHICLE:

REAL_SIM_DATASET:
REAL / SIMULATION / PUBLIC_DATASET

DATASET:

SCENARIO:

ENVIRONMENT:

LOS_NLOS:

DEGRADATION_CONDITION:

GROUND_TRUTH:

GROUND_TRUTH_ACCURACY:

EXPERIMENT_AREA:

TEST_DISTANCE:

TEST_DURATION:

NUMBER_OF_RUNS:

METRIC_NAME:

METRIC_DEFINITION:

REPORTED_VALUE:

UNIT:

STATISTIC_TYPE:
MEAN / MEDIAN / RMSE / MAX / MIN / PERCENTILE / SUCCESS_RATE / OTHER

BEST_CASE:

TYPICAL_CASE:

DEGRADED_CASE:

FAILURE_CASE:

BASELINES:

LIMITATIONS:

AUTHOR_STATED_LIMITATIONS:

WHAT_THIS_PAPER_SUPPORTS:

WHAT_THIS_PAPER_DOES_NOT_SUPPORT:

DIRECTLY_COMPARABLE_TO_DG202611:
YES / PARTIAL / NO

REASON:

SAFE_PROJECT_CLAIM:

UNSAFE_PROJECT_CLAIM:

==================================================
七、性能数字必须二次核验
==================================================

任何准备进入正式性能矩阵的数字必须至少重新核对一次原论文。

重点防止：

cm / m 混淆  
2D / 3D 混淆  
ATE / RPE 混淆  
RMSE / mean 混淆  
best sequence / overall average 混淆  
simulation / real 混淆  
LOS / NLOS 混淆  
translation / rotation 混淆  
单轴误差 / 总体误差混淆

每条关键数字新增：

VALUE_PRIMARY_CHECK = PASS / FAIL

VALUE_SECONDARY_CHECK = PASS / FAIL / NOT_DONE

SOURCE_LOCATION:
Table / Figure / Section / Page

如果无法定位到论文中的具体位置：

不得进入高置信性能矩阵。

==================================================
八、建立真正的性能基准矩阵
==================================================

创建：

STAGE_D_PERFORMANCE_BENCHMARK_MATRIX_v1.0.csv

至少字段：

Technology
Paper_ID
Year
Venue
Real_Sim_Dataset
Platform
Sensor_Models
Scenario
Degradation
LOS_NLOS
Ground_Truth
Metric
Metric_Definition
Value
Unit
Statistic_Type
Test_Distance
Test_Duration
Number_Of_Runs
Best_Typical_Degraded
Full_Text_Verified
Secondary_Verified
Directly_Comparable
Limitations
Safe_Project_Claim

重点覆盖：

GNSS/RTK  
UWB  
LiDAR  
VIO  
Radar Odometry  
Event-VIO  
Adaptive Fusion  
Relocalization

==================================================
九、专项：比赛指标安全边界
==================================================

创建：

STAGE_D_COMPETITION_METRIC_SAFE_BOUNDARIES_v1.0.md

逐项研究：

### 1. 平面定位误差 <20 cm

调查：

- 文献中 2D positioning / localization accuracy
- RMSE / MAE / CEP / 95th percentile
- GNSS/RTK
- UWB
- LiDAR fusion
- VIO
- multisensor fusion

回答：

比赛指标是否合理？

在哪些场景合理？

哪些场景很困难？

必须做什么实验才能证明？

### 2. 高程定位误差 <20 cm

重点调查：

- ZED / stereo
- VIO
- GNSS RTK vertical
- 3D LiDAR
- multisensor altitude estimation

不得因为存在 ZED 2i 就推导：

“项目已经解决高程20 cm”。

### 3. 相邻帧有效特征重复率 ≥95%

必须调查这个指标在学术文献中的对应关系。

可能相关：

feature repeatability  
feature tracking rate  
correspondence survival  
inlier repeatability

如果找不到完全相同定义：

明确：

NO_DIRECT_STANDARD_EQUIVALENT_FOUND

不得偷换指标。

### 4. 重定位成功率 >95%

调查：

success definition  
number of trials  
initial pose error  
environment  
recovery time  
failure criteria

尤其防止拿视觉 place recognition accuracy 当机器人闭环重定位成功率。

### 5. 北斗短报文

区分：

接口实现  
通信成功  
定位系统融合  
机器人平台集成

不得把“北斗支持短报文”写成：

“项目已经完成短报文集成”。

==================================================
十、UWB 专项深挖
==================================================

创建：

STAGE_D_UWB_QUALITY_REJECTION_EVIDENCE_v1.0.md

目标是直接支撑老师提出的：

“UWB信号稳定时使用，覆盖边缘或不稳定时降低权重或弃用。”

重点查：

- LOS / NLOS
- coverage edge
- RSSI
- CIR
- first path power
- received power
- range residual
- anchor geometry
- GDOP
- packet loss
- timestamp quality

建立：

QUALITY_INDICATOR
WHAT_IT_MEANS
NORMAL_BEHAVIOR
DEGRADED_BEHAVIOR
REJECTION_EVIDENCE
PAPER_SUPPORT

特别回答：

RSSI alone 是否足够？

哪些论文明确进行了：

adaptive weighting  
covariance adaptation  
NLOS rejection  
range rejection  
robust loss  
fault detection

不得自己制定最终阈值。

==================================================
十一、毫米波雷达专项
==================================================

创建：

STAGE_D_MMWAVE_RADAR_POSITIONING_EVIDENCE_v1.0.md

重点区分：

simple ranging radar

FMCW radar

point-cloud radar

4D imaging radar

Radar Odometry

回答：

为了轮式机器人在：

GNSS拒止  
视觉退化  
LiDAR局部退化

场景中辅助定位，需要哪些能力？

重点抽：

Range  
Angle  
Doppler  
Point Cloud  
Elevation  
Frame Rate  
Interface

以及真实论文中的定位性能。

不要直接给采购最终型号结论。

本阶段先形成：

MINIMUM_REQUIRED_CAPABILITIES

RECOMMENDED_CAPABILITIES

UNSUITABLE_RADAR_TYPES

==================================================
十二、事件相机 + ZED 2i 时间同步专项
==================================================

创建：

STAGE_D_EVENT_ZED_TEMPORAL_SYNC_EVIDENCE_v1.0.md

重点回答：

1. Event stream 与 frame camera 的本质区别。
2. 是否要求相同 FPS。
3. 软件时间同步如何实现。
4. timestamp offset 如何估计。
5. clock drift 如何处理。
6. hardware trigger 的优势。
7. shared clock / PPS / trigger 是否常用。
8. IMU 在其中如何对齐。
9. event accumulation window 如何选。
10. latency 对融合结果有什么影响。

特别核验：

老师说的“同频”是否更接近：

异构传感器时间同步 / 时间戳对齐 / 时延补偿

而不是：

“事件相机必须和 ZED 2i 完全相同帧率”。

没有文献支持时不得下最终结论。

==================================================
十三、自适应融合专项
==================================================

创建：

STAGE_D_ADAPTIVE_FAULT_AWARE_FUSION_EVIDENCE_v1.0.md

针对：

GNSS  
UWB  
LiDAR  
Visual  
Radar

整理：

Sensor
Quality Metric
Fault Indicator
Covariance Adaptation
Weighting Method
Innovation Gate
Mahalanobis Gate
Outlier Rejection
Hard Rejection
Recovery Logic
Literature Support

重点区分：

降权

与

拒绝

不是同一个动作。

寻找什么情况下文献选择：

soft weighting

什么情况下选择：

hard reject

不要替项目直接确定阈值。

==================================================
十四、主动重定位专项
==================================================

重点确认是否真正存在文献支持以下闭环：

LOCALIZATION_HEALTH_DEGRADED
→
STOP / SAFE CONTROL
→
ACTIVE MOTION
→
COLLECT BETTER OBSERVATION
→
CANDIDATE
→
VERIFY
→
RECOVER

不要只找：

image retrieval
place recognition
loop closure

必须区分：

“识别到了一个地方”

和

“机器人主动执行恢复动作并完成定位恢复”。

==================================================
十五、建立安全声明矩阵
==================================================

创建：

STAGE_D_SAFE_CLAIM_MATRIX_v1.0.md

每个重要项目声明给出：

CLAIM_ID

PROPOSED_CLAIM

LITERATURE_SUPPORT

PROJECT_EVIDENCE

CURRENT_STATUS

SAFE_NOW:
YES / NO

SAFE_WORDING

UNSAFE_WORDING

REQUIRED_PROJECT_EVIDENCE

必须严格区分：

LITERATURE_REPORTED

COMPETITION_REQUIREMENT

PROJECT_DESIGN_TARGET

SIMULATION_MEASURED

REAL_ROBOT_MEASURED

五种证据类型。

==================================================
十六、项目技术状态也必须分层
==================================================

以后不要只有：

“已实现 / 未实现”

采用：

TEACHER_CONFIRMED

HARDWARE_EXISTS

REPOSITORY_PROVEN

INTERFACE_VERIFIED

ROS2_DATA_VERIFIED

ALGORITHM_IMPLEMENTED

SIMULATION_VERIFIED

REAL_ROBOT_VERIFIED

PERFORMANCE_VERIFIED

FUTURE_OUTLOOK

例如 UWB：

TEACHER_CONFIRMED = YES  
HARDWARE_EXISTS = YES  
REPOSITORY_PROVEN = NO  
ROS2_DATA_VERIFIED = NO  
FUSION_VERIFIED = NO  
PERFORMANCE_VERIFIED = NO

毫米波：

PLANNED_PROCUREMENT = YES  
HARDWARE_EXISTS = NO  
REAL_ROBOT_VERIFIED = NO

事件相机同理。

==================================================
十七、不要 Cherry Pick
==================================================

对于核心技术尽量同时记录：

BEST_CASE

TYPICAL_CASE

DEGRADED_CASE

FAILURE_CASE

不能只挑论文最漂亮的一个 sequence。

如果论文只给最好结果：

明确注明：

POTENTIAL_CHERRY_PICKING_RISK

==================================================
十八、允许补论文，但限制扩量
====================
~~~~

[本对话目录](README.md) | [总目录](../../README.md) | [Raw](https://raw.githubusercontent.com/weiyang02520-ops/dg202611-conversation-archive/main/conversations/first/part-054.md) | [上一卷](part-053.md) | [下一卷](part-055.md)
