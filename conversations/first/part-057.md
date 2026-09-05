# 机器人定位挑战赛分析 / 第 057 卷

[本对话目录](README.md) | [总目录](../../README.md) | [Raw](https://raw.githubusercontent.com/weiyang02520-ops/dg202611-conversation-archive/main/conversations/first/part-057.md) | [上一卷](part-056.md) | [下一卷](part-058.md)

## 消息 2043 / assistant / 分段 1 of 2

~~~~text
【任务方向】
DOCUMENT_LINE / 文档与证据研究方向

【执行位置】
当前线程《查看比赛提交要求》，请在本线程直接执行；本线程负责文档、文献、证据与 DSH 管理，不执行开发代码任务，不联系开发线。

【任务名称】
DG-202611 DOCUMENT STAGE E — 高优先级证据缺口闭合 + 比赛指标可比性审计

【当前状态】

Stage D 已完成并冻结：

STAGE_D_STATUS = PASS_WITH_LIMITATIONS

已完成：

- CORE_PAPERS_TARGETED = 20
- FULL_TEXT_OBTAINED = 7
- FULL_TEXT_EXPERIMENT_VERIFIED = 7
- ABSTRACT_ONLY = 13
- NEW_HIGH_VALUE_PAPERS_ADDED = 5
- PERFORMANCE_VALUES_EXTRACTED = 16
- PERFORMANCE_VALUES_SECONDARY_VERIFIED = 15
- UWB全文论文 = 1
- 毫米波全文论文 = 1
- Event全文论文 = 1

当前已经形成：

- 全文核心论文卡片
- 性能基准矩阵
- 比赛指标安全边界
- UWB质量/拒绝证据
- 毫米波定位证据
- Event+ZED时间同步证据
- 自适应融合证据
- 主动重定位证据
- 安全声明矩阵
- 项目技术状态矩阵

当前仍然：

READY_TO_WRITE_FORMAL_CHAPTER_2 = NO

READY_TO_SUPPORT_CHAPTER_6_8 = YES
但仅限方法依据，不含项目性能结论。

==================================================
一、本轮定位
==================================================

Stage E 不是继续做“大规模论文搜集”。

本轮目标是：

CLOSE HIGH-PRIORITY EVIDENCE GAPS

只攻那些会直接影响：

1. 正式第2章能不能写；
2. 比赛指标能不能合理表述；
3. 第6—8章方法论是否有足够依据；
4. 后续实验应该怎么设计；
5. 哪些性能目标合理；
6. 哪些性能声明仍然必须禁止。

不要为了数量继续无目的扩张论文池。

==================================================
二、本轮边界
==================================================

允许：

- 搜索公开论文
- 获取作者公开全文 / arXiv / institutional repository
- 阅读 Methods / Experiment / Results / Supplement
- 补充少量高价值核心论文
- 更新 Stage C / D 的研究资产
- 新建 Stage E Markdown / CSV / BibTeX

禁止：

- 修改正式第1—12章正文
- 联系开发线
- 指挥 Claude Code
- 修改开发仓库
- 使用 DSH
- 编造项目实验数据
- 把文献结果写成项目结果
- 把比赛要求写成项目已达到结果
- 将摘要级数据伪装成全文级核验

必须保持：

FORMAL_BODY_MODIFIED = NO
DEVELOPMENT_LINE_CONTACTED = NO
DSH_USED = NO

==================================================
三、当前项目事实继续冻结
==================================================

当前项目技术事实：

### 已有明确硬件/基础

- 北斗 / GNSS / RTK
- IMU
- LiDAR
- ZED 2i
- UWB 硬件

UWB：

TEACHER_CONFIRMED = YES
HARDWARE_EXISTS = YES
INTERFACE_VERIFIED = NO
ROS2_DATA_VERIFIED = NO
FUSION_VERIFIED = NO
REAL_ROBOT_PERFORMANCE_VERIFIED = NO

### 近期计划采购

毫米波雷达：

PLANNED_PROCUREMENT = YES
HARDWARE_EXISTS = NO / NOT_YET_VERIFIED
INTEGRATION_VERIFIED = NO

事件相机：

PLANNED_PROCUREMENT = YES
HARDWARE_EXISTS = NO / NOT_YET_VERIFIED
INTEGRATION_VERIFIED = NO

### 当前不进入主线

ToF
光流
地面纹理定位

统一：

NOT_IN_CURRENT_SCOPE

### 未来展望

偏振光导航
天基光学导航

统一：

FUTURE_OUTLOOK

不要在 Stage E 重新扩大这些非主线方向。

==================================================
四、Stage E 五个核心问题
==================================================

本轮必须优先闭合以下五个问题：

P0-1：
高程定位误差 <20 cm 是否有直接或接近直接的文献依据？

P0-2：
“相邻帧有效特征重复率 ≥95%”到底对应什么学术指标？

P0-3：
“重定位成功率 >95%”在真正闭环机器人重定位中有没有直接可比证据？

P0-4：
13篇 ABSTRACT_ONLY 核心论文中，哪些能够升级成 FULL_TEXT_EXPERIMENT_VERIFIED？

P0-5：
UWB / 毫米波 / Event / 自适应融合当前全文证据是否足够支撑正式第2章研究现状？

==================================================
五、优先任务 A：补齐13篇核心论文全文
==================================================

逐一处理 Stage D 的：

ABSTRACT_ONLY = 13

对每篇执行：

1. 查出版社页面
2. 查 arXiv
3. 查作者主页
4. 查大学机构仓库
5. 查实验室公开版本
6. 查 accepted manuscript
7. 查 supplementary

不得绕过付费墙进行非法下载。

每篇最终标记：

FULL_TEXT_FOUND = YES / NO

FULL_TEXT_SOURCE =

METHODS_READ = YES / NO

EXPERIMENTS_READ = YES / NO

RESULTS_READ = YES / NO

FULL_TEXT_EXPERIMENT_VERIFIED = YES / NO

如果依然拿不到：

ABSTRACT_ONLY_REMAINS = YES

不要猜实验细节。

==================================================
六、全文目标
==================================================

目标：

FULL_TEXT_EXPERIMENT_VERIFIED >= 15

如果无法达到：

如实报告。

不要通过降低“全文验证”标准凑数。

FULL_TEXT_EXPERIMENT_VERIFIED 必须意味着：

论文实验方法和结果部分确实被阅读，
关键指标能够定位到正文、表格或图。

==================================================
七、优先任务 B：高程 <20 cm 专项闭合
==================================================

创建专项文件：

STAGE_E_VERTICAL_20CM_EVIDENCE_AUDIT_v1.0.md

重点搜索：

vertical localization accuracy
altitude estimation
height estimation
stereo visual odometry vertical accuracy
VIO vertical drift
GNSS RTK vertical accuracy
3D LiDAR localization vertical error
multisensor altitude estimation
LiDAR visual inertial altitude
stereo depth elevation localization
robot vertical positioning accuracy

重点关注：

ZED / stereo
VIO
GNSS RTK
3D LiDAR
LiDAR-inertial
visual-inertial
GNSS-inertial
multisensor fusion

每篇必须抽取：

SENSOR_CONFIG

REAL_OR_DATASET

SCENARIO

GROUND_TRUTH

GROUND_TRUTH_ACCURACY

VERTICAL_METRIC

VERTICAL_VALUE

UNIT

STATISTIC_TYPE

TEST_DISTANCE

TEST_DURATION

CONDITIONS

LIMITATIONS

==================================================
八、高程20cm必须区分三种情况
==================================================

最终只允许以下三种结论之一：

A：
DIRECT_SUPPORT_FOUND

存在可直接比较的真实实验，明确报告 vertical / z-axis error，并且指标定义接近比赛要求。

B：
INDIRECT_SUPPORT_ONLY

有高度/垂向误差结果，但场景、指标定义或平台不完全可比。

C：
NO_RELIABLE_SUPPORT_FOUND

没有足够可靠的直接证据。

禁止因为：

RTK一般很准
ZED有深度
3D LiDAR有z坐标

就自动推出：

“高程20cm容易达到”。

==================================================
九、优先任务 C：95% 特征重复率定义审计
==================================================

创建：

STAGE_E_FEATURE_REPEATABILITY_95_AUDIT_v1.0.md

比赛要求：

相邻帧有效特征重复率 ≥95%

必须调查它到底可能对应：

feature repeatability
feature tracking rate
feature survival rate
feature retention rate
feature matching rate
correspondence ratio
inlier correspondence rate
repeatability score
tracking success rate

重点查：

传统视觉特征

LiDAR features

event features

multi-frame feature tracking

==================================================
十、禁止指标偷换
==================================================

对每个候选指标建立：

CANDIDATE_METRIC:

FORMAL_DEFINITION:

NUMERATOR:

DENOMINATOR:

FRAME_RELATION:

REQUIRES_GROUND_TRUTH:

COMMON_USAGE:

CLOSE_TO_COMPETITION_METRIC:
HIGH / MEDIUM / LOW

CAN_BE_USED_AS_EQUIVALENT:
YES / NO

REASON:

如果学术界找不到完全等价定义：

必须明确：

NO_DIRECT_STANDARD_EQUIVALENT_FOUND

然后建议后续项目自己定义一个：

DG_FEATURE_REPEATABILITY_METRIC

但当前只提出定义框架，不得编实验结果。

==================================================
十一、尝试建立比赛指标的可执行定义
==================================================

如果官方比赛材料没有更详细公式，而文献没有完全等价标准，可提出候选定义供后续审核：

例如只作为研究建议：

Repeatability =
N_effective_corresponding_features_next_frame
/
N_valid_features_reference_frame

但必须注明：

PROPOSED_PROJECT_METRIC_DEFINITION
NOT_OFFICIAL_CONFIRMED

并说明：

- 有效特征如何定义
- correspondence 如何定义
- 空间阈值
- descriptor threshold
- occlusion处理
- 新出现/消失特征处理
- 动态目标处理
- frame interval

不要擅自宣称这是比赛官方公式。

==================================================
十二、优先任务 D：重定位 >95% 专项闭合
==================================================

创建：

STAGE_E_RELOCALIZATION_95_SUCCESS_AUDIT_v1.0.md

重点搜索真正的：

robot relocalization success rate
kidnapped robot recovery
active relocalization
localization recovery
global localization success
AMCL recovery
active localization
LiDAR relocalization
multi-modal relocalization
robot pose recovery

必须区分：

place recognition accuracy

loop closure recall

image retrieval Recall@1

global localization accuracy

robot relocalization success

closed-loop active recovery success

这些不是同一个指标。

==================================================
十三、定义“真正可比的闭环重定位”
==================================================

若论文要被认为可与 DG 的：

>95% 重定位成功率

直接比较，至少需要尽量具备：

1. 明确 initial localization failure / pose uncertainty
2. 明确 recovery trigger
3. 明确 candidate / pose recovery
4. 明确 success criterion
5. 多次 trial
6. success / fail count
7. 环境条件说明
8. ground truth 或可靠验证
9. 最好包含机器人闭环行为

分类：

DIRECTLY_COMPARABLE

PARTIALLY_COMPARABLE

NOT_COMPARABLE

==================================================
十四、如果找不到>95%的直接证据
==================================================

不要硬找数字。

必须输出：

DIRECT_LITERATURE_SUPPORT_FOR_95_PERCENT = NO

并明确：

比赛要求是：

COMPETITION_REQUIREMENT

不是：

LITERATURE_TYPICAL_VALUE

更不是：

PROJECT_CURRENT_RESULT

然后给出未来实验建议：

例如：

N >= 20 / 50 / 100 trials

不同初始误差

不同场景

成功判据

超时判据

统计置信区间

但不要替开发线实施。

==================================================
十五、优先任务 E：UWB全文证据扩充
==================================================

Stage D 当前：

UWB_FULLTEXT_PAPERS = 1

Stage E 目标：

至少再补 2–4 篇高质量全文实验论文。

重点：

UWB LOS/NLOS
UWB quality assessment
adaptive covariance
outlier rejection
NLOS identification
CIR
first path
RSSI
range residual
anchor geometry
GDOP

必须特别研究：

RSSI 是否可以单独作为质量判据。

如果多数文献认为不够：

总结原因。

==================================================
十六、UWB最终应形成质量指标层级
==================================================

输出：

LEVEL 1 — DIRECT RADIO QUALITY

RSSI
FP power
CIR
packet loss
etc.

LEVEL 2 — GEOMETRIC QUALITY

anchor count
anchor geometry
GDOP

LEVEL 3 — CONSISTENCY QUALITY

range residual
innovation
temporal consistency

LEVEL 4 — FUSION DECISION

NORMAL
DOWNWEIGHT
REJECT

必须全部由文献支撑。

不得提前替项目确定具体数值阈值。

==================================================
十七、优先任务 F：毫米波雷达全文扩充
==================================================

Stage D 当前：

RADAR_FULLTEXT_PAPERS = 1

目标：

再补 2–4 篇全文实验论文。

重点：

4D radar odometry
radar inertial odometry
FMCW radar localization
radar SLAM
radar-LiDAR fusion

优先真实机器人/车辆实验。

必须记录：

radar model/type
frequency band
range
angular resolution
Doppler
elevation
point density
ground truth
ATE/RPE/drift

重点研究：

毫米波作为“冗余定位源”到底能做到什么，
而不是只证明毫米波能检测目标。

==================================================
十八、优先任务 G：Event全文扩充
==================================================

Stage D 当前：

EVENT_FULLTEXT_PAPERS = 1

目标：

再补 2–4 篇。

重点：

event-frame fusion
event-inertial odometry
event stereo
temporal calibration
hardware synchronization
event camera HDR localization
high-speed localization

尤其补证：

共同时间轴
timestamp offset
latency compensation
hardware trigger
clock synchronization

如果没有 ZED 2i 专门论文：

不要强求。

可以使用：

frame camera + event camera

的通用异构视觉同步文献。

但必须注明：

GENERAL_FRAME_EVENT_EVIDENCE
NOT_ZED2I_SPECIFIC_VALIDATION

==================================================
十九、优先任务 H：自适应融合证据增强
==================================================

重点补：

adaptive Kalman filter

measurement covariance adaptation

innovation-based fault detection

Mahalanobis gating

robust estimation

switchable constraints

M-estimator

fault-tolerant multisensor fusion

目标不是堆算法名。

而是回答：

什么时候使用 soft downweight？

什么时候 hard reject？

什么时候允许 measurement 恢复？

怎么避免频繁切换？

有没有 hysteresis / temporal confirmation 方法？

==================================================
二十、比赛指标可比性矩阵
==================================================

创建：

STAGE_E_COMPETITION_METRIC_COMPARABILITY_MATRIX_v1.0.csv

字段至少包含：

Competiti
~~~~

[本对话目录](README.md) | [总目录](../../README.md) | [Raw](https://raw.githubusercontent.com/weiyang02520-ops/dg202611-conversation-archive/main/conversations/first/part-057.md) | [上一卷](part-056.md) | [下一卷](part-058.md)
