# 机器人定位挑战赛分析 / 第 053 卷

[本对话目录](README.md) | [总目录](../../README.md) | [Raw](https://raw.githubusercontent.com/weiyang02520-ops/dg202611-conversation-archive/main/conversations/first/part-053.md) | [上一卷](part-052.md) | [下一卷](part-054.md)

## 消息 1999 / assistant / 分段 2 of 3

~~~~text
ON:
YES / NO

SAFE_PROJECT_CLAIM:

PERFORMANCE_UPPER_BOUND_FOR_OUR_DOCUMENT:

NOTES:

==================================================
八、性能数字必须绑定测试条件
==================================================

禁止：

Paper A = 6.8 cm accuracy

这种记录。

必须写成类似：

Paper A
技术：UWB/IMU
环境：室内 LOS
区域：xx m × xx m
UWB基站：4
Ground Truth：Vicon
指标：2D RMSE
结果：6.8 cm
NLOS：未测试
限制：固定基站布局

只有这样才算有效证据。

==================================================
九、建立 PERFORMANCE BENCHMARK MATRIX
==================================================

创建：

03_PERFORMANCE_BENCHMARK_MATRIX.csv

至少字段：

Technology
Paper_ID
Year
Scenario
Real_Sim_Dataset
Sensors
Hardware
Ground_Truth
Metric
Metric_Definition
Value
Unit
LOS_NLOS
Degradation
Test_Distance
Test_Duration
Limitations
Safe_Project_Claim
Can_Compare_To_Our_Project
Reason

重点技术分别汇总：

GNSS/RTK
UWB
LiDAR
ZED/VIO
Radar Odometry
Event VIO
Adaptive Fusion
Relocalization

==================================================
十、特别建立“我们最多安全写多少”矩阵
==================================================

创建：

04_SAFE_CLAIM_BOUNDARIES.md

针对比赛关键指标，不能自己编数字。

重点分析：

1. 平面定位误差 <20 cm
2. 高程定位误差 <20 cm
3. 相邻帧有效特征重复率 ≥95%
4. 重定位成功率 >95%
5. 重定位时间
6. GNSS拒止情况下定位误差
7. UWB辅助定位精度
8. 雷达退化环境辅助效果
9. 事件相机抗强光/暗光效果

对每一项输出：

COMPETITION_REQUIREMENT:

LITERATURE_BEST_REPORTED:

LITERATURE_TYPICAL_RANGE:

TEST_CONDITIONS:

DIRECTLY_COMPARABLE:
YES / NO

WHY:

OUR_CURRENT_EVIDENCE:
NONE / SIMULATION / SYNTHETIC / REAL

SAFE_CURRENT_WORDING:

SAFE_TARGET_WORDING:

UNSAFE_WORDING:

NEEDED_EXPERIMENT:

绝不能因为论文最优结果很好，就直接把论文最优值写成我们的项目能力。

==================================================
十一、Claim-Evidence Matrix
==================================================

创建：

05_CLAIM_EVIDENCE_MATRIX.md

将我们未来可能写入方案书的技术结论逐条和文献绑定。

例如：

CLAIM:
UWB在NLOS环境下定位误差可能显著增加，因此需进行质量判断与异常观测拒绝。

EVIDENCE:
Paper X
Paper Y
Paper Z

STRENGTH:
STRONG / MODERATE / WEAK

CONDITIONS:

SAFE_WORDING:

UNSAFE_EXAGGERATION:

--------------------------------------------------

CLAIM:
事件相机在高动态范围和高速运动条件下具有传统帧相机不具备的异步视觉优势。

同样给出正式证据。

--------------------------------------------------

CLAIM:
毫米波雷达可在低照度以及部分视觉退化环境下提供几何和Doppler信息。

同样核验。

==================================================
十二、专门审计 Competition Metrics
==================================================

建立：

06_COMPETITION_METRIC_LITERATURE_AUDIT.md

分别审查：

<20 cm 平面定位

<20 cm 高程定位

≥95% 相邻帧有效特征重复率

>95% 重定位成功率

北斗短报文集成验证

对于：

“相邻帧有效特征重复率 ≥95%”

特别调查：

学术界是否存在完全相同定义。

如果没有：

明确写：

NO_DIRECT_STANDARD_EQUIVALENT_FOUND

并寻找最接近的：

feature repeatability
feature tracking rate
feature survival rate
correspondence repeatability

但禁止偷偷把不同指标当成同一个指标。

==================================================
十三、建立术语表
==================================================

创建：

07_TERMINOLOGY_AND_METRIC_DEFINITIONS.md

必须至少解释：

ATE
RPE
RMSE
MAE
CEP
2DRMS
95th percentile
success rate
relocalization success
relocalization time
feature repeatability
inlier ratio
matching score
HDOP
GDOP
NLOS
LOS
CIR
RSSI
innovation
residual
Mahalanobis distance
covariance
observability
degeneracy
Radar Odometry
VIO
Event-VIO

每个定义注明：

是否不同论文可能定义不一致。

==================================================
十四、建立公开数据集清单
==================================================

创建：

08_DATASET_CATALOG.md

针对：

GNSS
LiDAR
Radar
Event Camera
VIO
UWB
multisensor

收集可用于后续验证的公开数据集。

例如候选可以调查但必须核验：

KITTI
EuRoC
TUM-VI
MulRan
Oxford Radar RobotCar
DSEC
MVSEC
M3ED

以及真正适合 UWB / GNSS degradation 的数据集。

记录：

Dataset
Sensors
Environment
Ground Truth
Size
License
Official Link
Useful For DG
Limitations

==================================================
十五、查找可复现开源实现
==================================================

创建：

09_OPEN_SOURCE_IMPLEMENTATION_MAP.md

针对核心论文，寻找：

Official GitHub
Author GitHub
ROS/ROS2 implementation
Dataset tools

记录：

Paper
Repository
Official / Third-party
License
Language
ROS1/ROS2
Last Update
Reproducibility
Relevant Module
Risk

不要下载或执行不可信代码。

这里只做调研和链接验证。

==================================================
十六、Rejected / Unverified
==================================================

创建：

10_REJECTED_OR_UNVERIFIED.md

任何：

找不到
标题对不上
DOI错误
作者错误
页面不存在
二手文章无法追原文
疑似AI杜撰
实验条件不清
无法确认指标

都记录在这里。

不要悄悄删除。

记录：

Claimed Paper
Reason Found
Verification Attempt
Final Status
Reason Rejected

==================================================
十七、Search Log
==================================================

创建：

11_SEARCH_LOG.md

记录：

Date
Research Area
Search Query
Search Engine / Database
Number Reviewed
Number Accepted
Number Rejected
Notes

方便未来复查。

==================================================
十八、重点形成 UWB 专题
==================================================

额外创建：

12_UWB_QUALITY_AND_REJECTION_REVIEW.md

这是目前特别重要的专题。

最终必须回答：

UWB localization 的主要误差来源是什么？

LOS / NLOS 差异是什么？

coverage edge 的表现是什么？

哪些指标最适合判断可靠性？

RSSI 是否足够？

CIR 能提供什么？

first path power 有什么意义？

range residual 有什么意义？

如何结合几何/GDOP判断？

有哪些论文使用：

adaptive covariance

robust weighting

outlier rejection

NLOS rejection

machine learning NLOS classification

系统应：

何时正常使用

何时降权

何时拒绝

给出一个“文献支持的决策框架”。

不要把它写成我们已经实测完成。

==================================================
十九、重点形成毫米波雷达专题
==================================================

创建：

13_MMWAVE_RADAR_LOCALIZATION_REVIEW.md

最终必须回答：

我们这个轮式机器人如果为了：

GNSS拒止
视觉退化
LiDAR局部退化

加入毫米波雷达，

需要的是哪一类 radar capability。

重点比较：

simple ranging radar
point-cloud FMCW radar
4D imaging radar

不要推荐只有存在检测功能、却无法支撑定位研究的硬件。

输出：

REQUIRED_CAPABILITIES
NICE_TO_HAVE
UNNECESSARY_FEATURES
COMMON_INTERFACES
SYNC_REQUIREMENTS
RESEARCH_DATASETS
KNOWN_ALGORITHMS

采购具体型号仍待后续结合预算决定。

==================================================
二十、重点形成事件相机专题
==================================================

创建：

14_EVENT_CAMERA_ZED_SYNC_REVIEW.md

重点研究：

Event Camera
+
ZED 2i
+
IMU

的组合。

必须回答：

event stream 和 stereo frame 如何时间对齐？

软件同步有什么误差来源？

硬件 trigger 的意义是什么？

是否要求相同 FPS？

正确答案必须由文献支撑，不能凭直觉。

调查：

event accumulation window
nearest timestamp
interpolation
time surface
voxel representation
hardware trigger
shared clock
timestamp offset calibration

最终形成：

SENSOR_TIME_MODEL

SOFTWARE_SYNC_OPTION

HARDWARE_SYNC_OPTION

RISKS

RECOMMENDED_RESEARCH_DIRECTION

==================================================
二十一、重点形成自适应融合专题
==================================================

创建：

15_ADAPTIVE_FUSION_EVIDENCE_REVIEW.md

围绕：

GNSS
UWB
LiDAR
visual
radar

研究：

质量指标
confidence
uncertainty
covariance adaptation
innovation gating
outlier rejection
sensor failure detection

最终建立：

SENSOR
QUALITY_INDICATORS
NORMAL
DEGRADED
REJECT
POSSIBLE_WEIGHTING_METHOD
LITERATURE_SUPPORT

不要提前替我们确定阈值。

阈值必须后续通过：

论文
数据
实测

共同确定。

==================================================
二十二、性能数据双重核验
==================================================

任何准备进入：

PERFORMANCE_BENCHMARK_MATRIX

的关键数字，尽量进行两层核验。

第一层：

Research Agent 抽取。

第二层：

Verification Agent 独立打开论文重新检查：

table
figure
experiment section
supplementary material

然后记录：

EXTRACTION_VERIFIED = YES/NO

尤其检查：

cm vs m

mean vs RMSE

2D vs 3D

translation vs rotation

simulation vs real

best case vs average

LOS vs NLOS

dataset sequence

是否只取了一条最好的 sequence。

==================================================
二十三、禁止 Cherry Picking
==================================================

不能只找最漂亮的结果。

对于每个核心技术，至少记录：

BEST_CASE

TYPICAL_CASE

DEGRADED_CASE

FAILURE_CASE

这样才能知道我们的项目目标是否现实。

==================================================
二十四、冲突文献
==================================================

如果不同论文结论冲突：

不要强行合并成一个答案。

创建：

16_CONFLICTING_EVIDENCE.md

记录：

Claim
Paper A
Paper B
Different Conditions
Likely Explanation
What We Can Safely Say

==================================================
二十五、最终研究综合
==================================================

创建：

17_RESEARCH_SYNTHESIS_FOR_DG202611.md

这个文件不是方案书正文。

它只回答：

1. 当前多源定位领域成熟路线是什么？
2. 我们项目路线是否合理？
3. UWB 最适合扮演什么角色？
4. 毫米波雷达最适合扮演什么角色？
5. 事件相机最适合扮演什么角色？
6. 哪些场景是三者真正有价值的场景？
7. 时间同步/空间标定的重要性如何？
8. 自适应质量融合有哪些文献基础？
9. 主动重定位有哪些可借鉴方法？
10. 哪些比赛指标文献上比较激进？
11. 哪些指标属于合理目标？
12. 哪些现在完全不能声称已经达到？
13. 后续 Gazebo 应优先验证什么？
14. 后续实车实验必须采什么数据？
15. 哪些实验最值得写进最终方案书？

==================================================
二十六、输出目录
==================================================

在当前项目合适位置创建一个独立研究目录，例如：

DG-202611_literature_research/

包含：

01_RAW_LITERATURE_POOL.csv
02_FILTERED_CORE_PAPERS.md
03_PERFORMANCE_BENCHMARK_MATRIX.csv
04_SAFE_CLAIM_BOUNDARIES.md
05_CLAIM_EVIDENCE_MATRIX.md
06_COMPETITION_METRIC_LITERATURE_AUDIT.md
07_TERMINOLOGY_AND_METRIC_DEFINITIONS.md
08_DATASET_CATALOG.md
09_OPEN_SOURCE_IMPLEMENTATION_MAP.md
10_REJECTED_OR_UNVERIFIED.md
11_SEARCH_LOG.md
12_UWB_QUALITY_AND_REJECTION_REVIEW.md
13_MMWAVE_RADAR_LOCALIZATION_REVIEW.md
14_EVENT_CAMERA_ZED_SYNC_REVIEW.md
15_ADAPTIVE_FUSION_EVIDENCE_REVIEW.md
16_CONFLICTING_EVIDENCE.md
17_RESEARCH_SYNTHESIS_FOR_DG202611.md
references.bib

如果环境适合，再附：

papers/
metadata/

但不要未经许可批量下载受版权限制的论文全文。

==================================================
二十七、多 Agent 分工建议
==================================================

如果支持多个 Agent：

Agent 1:
GNSS / BeiDou / RTK

Agent 2:
UWB

Agent 3:
mmWave Radar

Agent 4:
Event Camera

Agent 5:
LiDAR Degeneracy + Dynamic Environment

Agent 6:
Adaptive / Fault-aware Fusion

Agent 7:
Active Relocalization

Agent 8:
Synchronization + Calibration

Agent 9:
Gazebo / Simulation + datasets

Agent 10:
BeiDou Short Message

Agent 11:
Performance Number Verification

Agent 12:
Fake Citation / DOI / Metadata Verification

最后由一个 synthesis agent 统一合并。

允许消耗大量 GLM-5.3-Flash token。

本任务优先：

广泛检索
+
真实性核验
+
逐篇抽取
+
交叉审计

不要为了省 token 简化成十几篇论文摘要。

==================================================
二十八、阶段性 Gate
==================================================

不要一开始就写最终综合。

Stage A：
RAW POOL ≥120

Stage B：
metadata verification

Stage C：
40–60 high-value papers

Stage D：
15–30 core evidence papers

Stage E：
performance extraction

Stage F：
independent metric verification

Stage G：
claim-evidence matrix

Stage H：
synthesis

每阶段保留真实统计。

==================================================
二十九、最终报告必须给出统计
==================================================

结束时报告：

TOTA
~~~~

## 消息 1999 / assistant / 分段 3 of 3

~~~~text
L_SEARCHED:

TOTAL_CANDIDATES:

VERIFIED_PAPERS:

PARTIALLY_VERIFIED:

REJECTED:

CORE_PAPERS:

PAPERS_WITH_REAL_EXPERIMENTS:

PAPERS_WITH_SIMULATION_ONLY:

PAPERS_WITH_PUBLIC_DATASETS:

PERFORMANCE_VALUES_EXTRACTED:

PERFORMANCE_VALUES_SECONDARY_VERIFIED:

UWB_CORE_PAPERS:

RADAR_CORE_PAPERS:

EVENT_CORE_PAPERS:

FUSION_CORE_PAPERS:

RELOCALIZATION_CORE_PAPERS:

GNSS_CORE_PAPERS:

BDS_SHORT_MESSAGE_REFERENCES:

UNRESOLVED_CLAIMS:

POTENTIALLY_UNREALISTIC_PROJECT_METRICS:

SAFE_CLAIM_RECOMMENDATIONS:

==================================================
三十、最重要的边界
==================================================

本研究只能回答：

“文献报道了什么”

“在什么条件下做到”

“我们的目标是否合理”

“后续应该如何实验”

不能回答：

“我们项目已经达到这个性能”

除非未来有我们自己的真实实验数据。

所有项目性能声明必须区分：

LITERATURE_REPORTED

COMPETITION_REQUIREMENT

PROJECT_DESIGN_TARGET

SIMULATION_MEASURED

REAL_ROBOT_MEASURED

五者严禁混淆。

现在开始执行。

优先把 UWB、毫米波雷达、事件相机、自适应融合四个方向同时开工，然后扩展到其余方向。

不要等待人工逐步确认；在不违反真实性和证据边界的情况下持续推进，直到完整研究资产落盘。
~~~~

## 消息 2000 / user / 分段 1 of 1

~~~~text
阶段C已完成，现已停止，不改正文、不联系开发线，也未使用DSH。详细材料均为本地 Markdown/CSV/BibTeX，没有渲染大文件。

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

## 消息 2001 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "thoughts",
  "thoughts": [
    {
      "summary": "评估研究进展",
      "content": "我认为这份报告只能算阶段性通过，不能直接视为完整交付。它完成了初筛和卡片整理，但全文证据、定量指标、工程映射仍不充分；还需要核对 UWB、事件相机与毫米波的真实集成状态，并补齐可复核材料。",
      "chunks": [],
      "finished": true
    },
    {
      "summary": "评估了研究进展",
      "content": "",
      "chunks": [],
      "finished": true
    }
  ],
  "source_analysis_msg_id": "7c1b88ae-e63a-4684-806d-451226a407a8"
}
~~~~

## 消息 2002 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "reasoning_recap",
  "content": "思考了 11s"
}
~~~~

## 消息 2003 / assistant / 分段 1 of 1

~~~~text
这版我看下来，**阶段 C 可以收，但只能算“文献地图和第一轮筛选完成”，绝对不能在这里停掉整个文献工作。** ZCode 自己标 `PARTIAL` 是对的。

现在最大的问题不是“60 篇够不够”，而是**老师最关心的那一层还没有真正做出来：论文在什么条件下做到了什么数字，我们最多能安全写到哪里。**

你这份报告目前已经解决了：

- 哪些方向有文献基础；
- 哪些技术路线基本靠谱；
- 哪些话现在不能写；
- 20 篇核心论文候选已经选出来；
- 60 个来源真实性至少做了元数据级核验。

但还没有真正解决我们最需要的：

> **“别人到底做到了多少？”**
>
> **“是在什么硬件、什么场景、什么 Ground Truth、什么指标定义下做到的？”**
>
> **“我们的 `<20 cm`、`>95%` 到底现实不现实？”**

所以我建议下一步不要扩到 100 多篇继续“搜标题”，而是**收缩战线，狠狠干 20 篇核心论文全文**。

尤其优先级现在应该调整一下，因为老师的新口径已经比这个 Stage C 报告更新了：

| 方向 | 下一阶段优先级 |
|---|---:|
| UWB 质量判断 / NLOS / 降权与拒绝 | **最高** |
| 毫米波雷达定位 / Radar Odometry | **最高** |
| 事件相机 + ZED 2i + IMU 时间同步 | **最高** |
| 自适应/故障感知融合 | **最高** |
| GNSS/北斗退化 | 高 |
| LiDAR 退化 | 高 |
| 主动重定位 | 高 |
| VIO/ZED 2i | 中高 |
| 北斗短报文 | 中 |
| ToF / 光流 / 地纹 | **停止** |
| 偏振/天基光学 | 只保留展望 |

有一个地方也要特别纠正口径：

**“毫米波/Event/UWB已集成不能写”这句话不能把三者完全并列。**

现在真实状态应该是：

- **UWB：已有实物，这是老师已经确认的；但是否完成机器人接入/ROS2数据读取/融合，还没验证，所以不能写“已完成集成验证”。**
- **毫米波：尚未购买。**
- **事件相机：尚未购买。**

所以后面文献库也要把：

`PROJECT_HARDWARE_EXISTS`

和

`PROJECT_INTEGRATION_VERIFIED`

分开，不能再只有一个“已集成/未集成”。

另外，我不建议现在让它碰正式第 2 章。它自己停下来这一点是对的。**现在应该继续 Stage D/E，而不是开始写正文。**

下一阶段我会要求 ZCode 专门做四件硬活：

1. 把 20 篇核心论文全文实验部分精读完；
2. 把所有关键性能数字二次核验；
3. 做真正的 `PERFORMANCE_BENCHMARK_MATRIX`；
4. 针对 UWB、毫米波、事件相机、自适应融合做专项深挖。

等这一步做完，我们才有资格回答老师最关心的：

> “这个指标写多少不会吹过头？”

所以目前我的审核结论是：

**STAGE C = PASS WITH LIMITATIONS**  
**LITERATURE RESEARCH = NOT FINISHED**  
**FORMAL BODY REWRITE = NOT ALLOWED YET**  
**NEXT = FULL-TEXT PERFORMANCE EXTRACTION + SPECIAL TOPIC DEEP DIVE**

而且这正好适合继续烧 ZCode 那 3 亿 GLM-5.3-Flash 额度——**别让它 60 篇摘要级核验就下班，最值钱的活才刚开始。** memcite
~~~~

## 消息 2004 / user / 分段 1 of 1

~~~~text
这个是文档方向的，不是zcode的，你下次提示词叫他们自己说明是哪一方向的
~~~~

## 消息 2005 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "reasoning_recap",
  "content": "思考了两秒"
}
~~~~

## 消息 2006 / assistant / 分段 1 of 1

~~~~text
对，这次是我把**“文档方向的 Stage C 文献研究”**和 **ZCode 那 3 亿额度的研究任务**混到一起了。以后必须分开。

从下一条提示词开始，我会强制在最开头加一个身份头，而且要求执行者在最终汇报里也原样声明自己是哪条线，例如：

```text
【任务方向】
DOCUMENT_LINE / 文档方向

【执行者身份】
你是 DG-202611 文档与证据研究线程。

【本方向职责】
负责：文献、证据、方案书、技术事实核对、指标边界。
不负责：开发代码、Gazebo实现、ROS2修改、硬件控制。

【禁止跨线】
不得联系或指挥开发线。
不得修改开发仓库。
不得把其他线程结果冒充本方向结果。
```

开发任务则写：

```text
【任务方向】
DEVELOPMENT_LINE / 开发方向
```

ZCode 单独做论文矿工时写：

```text
【任务方向】
ZCODE_RESEARCH_LINE / ZCode专项研究方向

【模型/环境】
ZCode + GLM-5.3-Flash

【职责】
只做指定的大规模检索、论文精读、数据抽取或审计。
```

而且**最终报告第一行也必须返回**：

```text
TASK_DIRECTION: DOCUMENT_LINE
```

或者：

```text
TASK_DIRECTION: DEVELOPMENT_LINE
```

这样以后你把某个 Agent 的结果直接扔给我，我一眼就知道它是哪条线，不再靠上下文猜。

刚才那个 Stage C 我现在重新归档为：

**TASK_DIRECTION = DOCUMENT_LINE**  
**内容 = 文档线的真实文献研究 Stage C**  
**与 ZCode 3 亿额度任务无关。**

以后我给你的所有长提示词都按这个规则来。
~~~~

[本对话目录](README.md) | [总目录](../../README.md) | [Raw](https://raw.githubusercontent.com/weiyang02520-ops/dg202611-conversation-archive/main/conversations/first/part-053.md) | [上一卷](part-052.md) | [下一卷](part-054.md)
