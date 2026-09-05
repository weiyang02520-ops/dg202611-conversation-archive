# 机器人定位挑战赛分析 / 第 036 卷

[本对话目录](README.md) | [总目录](../../README.md) | [Raw](https://raw.githubusercontent.com/weiyang02520-ops/dg202611-conversation-archive/main/conversations/first/part-036.md) | [上一卷](part-035.md) | [下一卷](part-037.md)

## 消息 1588 / assistant / 分段 1 of 2

~~~~text
执行位置：当前 Claude Code 开发线程。请在当前线程直接执行，不得转发。

DG-202611 DEVELOPMENT ROUND B  
S05–S08 ACTIVE RELOCALIZATION CLOSED-LOOP VALIDATION  
+ VISIBLE EXECUTION  
+ DOCUMENT EVIDENCE CAPTURE

本轮是 Claude Code 正式接管后的第一轮开发任务。

核心要求：

不仅要把实验跑通，还必须让项目所有者能够在 VMware Ubuntu 桌面中实时看到实验过程，并为后续比赛方案书、答辩PPT、实验章节保存足够的真实图片证据。

注意：

用户口语中可能称为“训练”，但当前 S05～S08 本质仍是：

SYNTHETIC SOFTWARE VALIDATION

不是机器学习模型训练。

所有图片、数据和报告必须正确标注证据等级。

==================================================
一、开始前固定核验
==================================================

当前预期：

Workspace：

/home/weiyang/dg202611_ws

Repository：

/home/weiyang/dg202611_ws/src/electric-power-inspection-robot

Branch：

dg202611-synthetic-validation

Expected HEAD：

57ec8ecff90d4e383e8c5197105bc8d182474245

HEAD parent / 已验证功能代码：

fd2c60d54401480fd40590857be55b9443c43d49

固定比赛事实源：

c41adca7f9bddb4240a1a4855437518b36d9fe13

开始前核验：

git status --short --branch
git branch --show-current
git rev-parse HEAD
git rev-parse HEAD^
git log --oneline --decorate -15

要求：

branch正确；
HEAD正确；
working tree clean。

不一致立即停止。

禁止自动 reset / clean / rebase / merge / checkout 覆盖。

==================================================
二、已有成果不得破坏
==================================================

S01～S04 已完成最终 synthetic validation：

S01 PASS
S02 PASS
S03 PASS
S04 PASS

最新结果根目录：

/home/weiyang/dg202611_ws/results/synthetic/

历史失败证据：

/home/weiyang/dg202611_ws/results/synthetic/S01_nominal_20260827-205016/

不得：

删除
覆盖
重写
修改原始数据。

S01～S04 最新 PASS 结果也不得覆盖。

所有新实验必须产生新的 run_id。

==================================================
三、本轮唯一功能目标
==================================================

正式验证 Active Relocalization 闭环。

本轮只做：

S05
S06
S07
S08

不做：

ZED/VIO
毫米波
光流
事件相机
ToF
偏振光
Gazebo
Jetson实机部署
真实机器人
北斗短报文
官方性能指标测试。

==================================================
四、先读取真实 Active Relocalization 实现
==================================================

修改任何代码前，必须完整检查当前：

Active Relocalization

Scan-to-Map

Navigation Health

Multisource Fusion

cmd_vel Arbiter

之间的真实状态机和接口。

必须根据代码实际状态名称工作。

禁止为了符合提示词自行创造：

LOST
SEARCHING
VERIFYING
RECOVERED

等状态。

如果代码实际名称不同：

完全以代码为准。

首先输出内部状态机关系，包括：

触发条件

输入

输出

状态转移

恢复动作

Scan-to-Map candidate

多帧验证

恢复成功条件

恢复失败条件

cmd_vel控制权变化。

==================================================
五、S05 — LOCALIZATION FAILURE
==================================================

目标：

验证定位健康真正进入失效/严重退化条件。

通过 synthetic sensor input 制造真实算法可检测的定位失效条件。

Injector 仍然只能发布：

/gps/fix
/gps/rtk_status
/scan
/odom
/amcl_pose
/map
/initialpose

禁止直接发布任何：

/dg/*

算法输出。

S05至少验证：

定位输入由正常状态

↓

逐渐退化

↓

达到当前代码定义的严重失效条件

↓

Navigation Health产生实际状态响应

↓

Active Relocalization收到真实触发条件。

要求保存：

触发前状态

失效开始时间

Health状态变化

Fusion状态变化

Relocalization状态变化

cmd_vel来源。

S05不得因为“节点没崩”就PASS。

==================================================
六、S06 — ACTIVE RECOVERY MOTION
==================================================

目标：

验证 Active Relocalization 被真实触发以后，能够进入主动运动阶段。

重点观察：

是否安全停止原导航控制

↓

是否由 Recovery获得允许的测试控制权

↓

是否生成当前状态机设计的主动运动

例如：

旋转
观察
扫描

实际动作以代码为准。

测试输出必须继续固定：

/dg/test_cmd_vel

绝对禁止：

/cmd_vel

必须记录：

linear_x

angular_z

cmd_vel source

relocalization state

持续时间

进入条件

退出条件。

必须证明：

主动运动来自真实 Active Relocalization / Arbiter 链路。

禁止 injector 伪造 recovery cmd_vel。

==================================================
七、S07 — SCAN-TO-MAP CANDIDATE
==================================================

目标：

验证主动获取新观测以后：

Scan-to-Map能够产生真实候选恢复信息。

必须观察当前代码实际输出，例如：

candidate pose

match quality

geometry quality

inlier ratio

mean distance

score

covariance

实际字段完全以代码为准。

测试必须验证：

候选来自真实 Scan-to-Map 算法。

禁止 synthetic injector 直接生成：

/dg/relocalization/*

结果。

必须记录：

候选首次出现时间

候选质量

候选是否被接受

候选是否被拒绝

拒绝原因。

==================================================
八、S08 — MULTI-FRAME RECOVERY VERIFICATION
==================================================

目标：

验证恢复候选不是单帧偶然结果。

完整流程应根据真实状态机验证：

candidate

↓

连续观测

↓

多帧一致性/恢复确认

↓

恢复成功或失败

↓

Localization Health重新进入可导航状态

↓

Recovery控制释放

↓

Navigation重新获得允许的控制路径。

实际状态名称和阈值必须以当前代码为准。

禁止为了PASS降低阈值。

禁止为了PASS直接修改状态机语义。

==================================================
九、核心算法修改规则
==================================================

如果发现：

synthetic scenario输入设计问题

evaluator bug

startup race

记录工具问题

可视化问题

允许在 dg_synthetic_validation 范围修复。

如果发现：

Active Relocalization核心算法逻辑本身存在错误

或必须修改：

算法阈值
状态机条件
恢复成功定义
融合语义

才能让 S05～S08 PASS：

立即停止。

保存 FAIL 证据。

报告 blocker。

不要偷偷修改核心算法迎合测试。

==================================================
十、用户必须能实时看到
==================================================

这是从本轮开始的永久开发规则：

VISIBLE_EXECUTION_REQUIREMENT = MANDATORY

后续重要实验不能只在后台 SSH 静默运行。

必须同时支持：

1. HEADLESS AUTOMATION
2. VMWARE LIVE OBSERVATION

后台 SSH：

负责：

build
test
scenario
rosbag
CSV
日志
自动评价

VMware Ubuntu GUI：

负责让用户实时观察：

RViz2
Monitor
实验状态
恢复过程。

==================================================
十一、实时 Monitor 必须增强到 S05～S08
==================================================

现有命令：

ros2 run dg_synthetic_validation monitor_scenario

继续保持只读。

不得发布消息。

本轮根据实际字段扩展显示：

SCENARIO

PHASE

ELAPSED TIME

GNSS

LiDAR

Fusion

Navigation Health

Relocalization

Scan-to-Map

Candidate

Candidate quality

Verification count

Recovery status

cmd_vel source

linear_x

angular_z

实际不存在字段：

N/A。

终端中应能明显看到状态变化。

例如不要只连续打印原始JSON。

应尽可能形成容易人眼阅读的动态状态界面。

==================================================
十二、RViz2 必须成为正式证据观察界面
==================================================

扩展或复用当前：

dg_synthetic_validation.rviz

尽可能显示真实存在的：

/map

/scan

/dg/lidar/filtered_scan

/odom

/amcl_pose

/dg/fusion/pose

/tf

/tf_static

以及能够正常显示的 relocalization / candidate 信息。

如果 candidate 当前没有 RViz 可显示消息：

不要改变核心消息协议只为了显示。

可以使用 validation-only Marker / MarkerArray bridge：

前提是：

只订阅真实算法输出

只用于显示

绝不能反向影响算法。

如果实现 validation-only marker：

必须明显命名为 visualization-only。

==================================================
十三、用户观看工作流
==================================================

本轮必须提供：

USER_VISIBLE_WORKFLOW.md

或者已有 handoff 文档中的明确章节。

要求用户未来只需在 Ubuntu VMware 桌面打开少量终端。

推荐：

Terminal A：

实时 Monitor

Terminal B：

RViz2

Terminal C：

实验日志 / 状态

后台 Claude Code：

负责真正 scenario runner。

给出所有准确命令。

不能写伪命令。

==================================================
十四、最终证据实验前必须暂停一次
==================================================

允许 Claude Code 先在后台：

开发
调试
运行必要测试
运行 headless 场景

但是：

当 S05～S08 已经具备稳定运行条件以后，

不要直接静默完成最终证据批次。

必须输出：

VISIBLE_EVIDENCE_CHECKPOINT

告诉用户：

现在已经准备好进行正式可视证据运行。

给出 Ubuntu VMware 桌面应该打开的：

Monitor命令

RViz命令

必要的观察终端命令。

然后等待用户确认：

VISIBLE_READY

用户确认后：

再执行最终 S05～S08 evidence run。

这样用户可以亲眼看到最终实验。

==================================================
十五、图片证据是强制要求
==================================================

这是永久规则：

IMPORTANT_EXPERIMENT_EVIDENCE_IMAGES = REQUIRED

后续每一个重要实验都要尽可能保存大量真实图片。

目的包括：

比赛方案书

技术路线说明

实验章节

答辩PPT

阶段汇报

开发记录。

本轮 S05～S08 必须建立正式：

evidence/

或：

screenshots/

目录。

每个最终场景单独保存。

==================================================
十六、建议截图关键时刻
==================================================

根据实际状态尽可能保存：

01_nominal_before_failure.png

02_failure_onset.png

03_health_degraded_or_lost.png

04_relocalization_triggered.png

05_active_recovery_motion.png

06_scan_to_map_candidate.png

07_candidate_quality.png

08_multi_frame_verification.png

09_recovered_state.png

10_post_recovery_stable.png

注意：

只有真实发生对应状态时才能保存对应图片。

如果某状态没有发生：

不能伪造图片。

应该记录：

NOT_OBSERVED

而不是制作假截图。

目标：

最终 S05～S08 每个重要阶段尽量有图片。

不要求机械凑数量。

真实性优先。

==================================================
十七、截图来源
==================================================

图片证据分三类。

A. RViz screenshots

用于展示：

地图

LiDAR

pose

robot state

relocalization candidate

恢复前后状态。

B. Terminal / Monitor screenshots

用于展示：

实时状态

GNSS/LiDAR/Fusion/Health

Relocalization state

cmd_vel。

C. 自动分析图

继续生成：

state timeline

confidence

quality

cmd_vel

candidate quality

recovery verification

等PNG。

自动分析图不等于GUI截图。

三类证据应尽量同时存在。

==================================================
十八、自动截图能力
==================================================

先只读检查 Ubuntu GUI 环境已有截图工具，例如：

gnome-screenshot

scrot

ImageMagick import

其它已安装工具。

本轮禁止为了截图自行安装大型软件。

如果存在可安全使用的截图工具：

可以实现：

desktop evidence capture helper。

要求：

只能在用户的 Ubuntu GUI / DISPLAY 有效时运行。

不得在 SSH 无 DISPLAY 情况伪造GUI截图。

不得修改系统设置。

如果自动 GUI 截图无法安全实现：

明确报告：

AUTOMATIC_GUI_CAPTURE_UNAVAILABLE

然后提供：

MANUAL_CAPTURE_COMMAND / MANUAL_CAPTURE_CHECKLIST

用户手动截图。

但是：

自动生成的数据图仍必须完成。

==================================================
十九、图片必须带元数据
==================================================

每个最终 evidence 目录必须生成：

evidence_manifest.csv

和：

evidence_manifest.md

至少记录：

filename

scenario

run_id

timestamp

elapsed_time

phase

event

source

RVIZ / MONITOR / PLOT / MANUAL

真实算法状态

caption

evidence_level。

所有截图说明必须明确：

SYNTHETIC SOFTWARE VALIDATION

NOT REAL ROBOT DATA

NOT COMPETITION PERFORMANCE EVIDENCE

==================================================
二十、图片必须方便以后直接写文档
==================================================

为每张重要图片自动生成简短中文 caption。

例如：

“图 X S06 主动重定位触发后的恢复控制状态，可见导航健康度下降后恢复控制链开始输出测试旋转指令。”

但 caption：

只能描述图里真实存在的信息。

不得写：

“定位精度达到20cm”

“重定位成功率超过95%”

等未验证结论。

最终额外生成：

DOCUMENT_EVIDENCE_INDEX.md

按：

S05
S06
S07
S08

列出：

推荐用于方案书的图片

图片路径

建议图题

说明了什么

不能说明什么。

==================================================
二十一、以后所有实验都遵守图片规则
==================================================

创建：

docs/dg202611/EXPERIMENT_EVIDENCE_POLICY.md

记录以后永久规则：

任何：

synthetic validation

Gazebo

Jetson

real robot

competition metric test

以及未来真正的：

ML training

都必须考虑：

日志

原始数据

rosbag

CSV

plot

关键截图

实验环境照片

结果照片

异常照片。

真实机器人阶段尤其必须保留：

机器人整体照片

传感器安装照片

实验环境照片

操作过程照片

异常状态照片

恢复状态照片

测试标定照片。

当前本轮没有真车：

不得生成或伪造真车照片。

==================================================
二十二、如果未来是真正神经网络训练
==================================================

这里只建立规则，不在本轮训练任何模型。

未来若出现：

Event模型

PINN

学习式定位

视觉网络

其它 ML / DL training

证据必须额外保存：

训练配置

dataset版本

train/val划分

epoch

loss

validation loss

learning rate

GPU utilization

checkpoint

best checkpoint

训练曲线

验证曲线

典型成功样本

典型失败样本
~~~~

## 消息 1588 / assistant / 分段 2 of 2

~~~~text


可视化预测结果。

不得只保存最终 accuracy。

==================================================
二十三、静态图升级
==================================================

S05～S08 每个最终 run 至少根据实际数据生成适用的：

plots/state_timeline.png

plots/relocalization_state.png

plots/navigation_health.png

plots/cmd_vel.png

plots/scan_map_quality.png

plots/recovery_verification.png

具体字段不存在则不生成空图。

禁止用假的数据填图。

每张图明显标记：

SYNTHETIC SOFTWARE VALIDATION

NOT REAL ROBOT DATA

==================================================
二十四、结果目录
==================================================

推荐最终结构：

results/synthetic/<scenario>_<run_id>/

metadata.json

scenario.yaml

samples.csv

timeline.csv

result.json

result.md

rosbag/

logs/

plots/

evidence/

evidence_manifest.csv

evidence_manifest.md

不得提交运行结果进 Git。

==================================================
二十五、正式 PASS 规则
==================================================

重新审查 S05～S08 evaluator。

禁止：

节点没崩 = PASS

状态出现一次 = PASS

Recovery动了一下 = PASS

Candidate出现 = 重定位成功

S05 PASS 必须证明：

真实输入退化

→

Health真实进入目标失效条件

→

Relocalization真实得到触发。

S06 PASS 必须证明：

Recovery真实接管允许的测试控制

→

产生预期主动运动

→

/cmd_vel仍然绝对安全。

S07 PASS 必须证明：

真实 Scan-to-Map节点产生可评价candidate。

S08 PASS 必须证明：

真实candidate

→

当前代码定义的连续/多帧验证

→

真实状态恢复

→

Health恢复

→

控制权正确释放。

任何不满足：

FAIL。

==================================================
二十六、cmd_vel安全
==================================================

永久安全规则：

TEST_OUTPUT：

/dg/test_cmd_vel

REAL ROBOT CMD：

/cmd_vel

每个场景运行：

开始前检查：

ros2 topic info /cmd_vel -v
ros2 topic info /dg/test_cmd_vel -v

结束后再次检查。

如果测试链向：

/cmd_vel

发布：

立即终止整个实验。

保存证据。

报告：

SAFETY_BLOCKER。

不得继续。

==================================================
二十七、禁止伪造算法输出
==================================================

最终必须报告：

DIRECT_DG_OUTPUTS_FAKED_BY_INJECTOR:

必须为：

NO

禁止直接发布：

/dg/gnss/quality

/dg/lidar/quality

/dg/fusion/status

/dg/navigation/status

/dg/relocalization/status

以及其它核心 `/dg/*` 算法结果。

==================================================
二十八、Build/Test
==================================================

使用 handoff 中已经确认的定向入口。

禁止把：

full repository lint failures

误判为 DG功能失败。

但也禁止声称：

整个仓库所有测试PASS。

记录：

实际build命令

targeted tests

validation tests

py_compile

以及本轮新增测试。

==================================================
二十九、Git规则
==================================================

禁止 push。

禁止 merge。

禁止 rebase。

运行数据：

rosbag
CSV
PNG
screenshots
logs

不得 commit。

代码和文档可根据实际修改提交。

建议拆为：

feat(test): add active relocalization validation scenarios

feat(test): add visible experiment evidence workflow

如果只需要一个提交：

可以合理合并。

必须最终：

git status clean。

==================================================
三十、本轮不允许扩展新技术
==================================================

不要因为老师新方案而提前做：

ZED VIO

Radar

Optical Flow

Event Camera

ToF

Polarization

3D estimator。

S05～S08完成以后：

再进行下一阶段技术选型。

==================================================
三十一、正式证据运行后生成汇总
==================================================

生成：

results/synthetic/active_relocalization_summary.csv

results/synthetic/active_relocalization_summary.md

至少包括：

S05

S06

S07

S08

PASS/FAIL

关键状态变化

warning

error

run_id

artifact path

evidence image count

recommended document images。

==================================================
三十二、完成后最终返回
==================================================

最终报告格式：

PREVIOUS_HEAD:

WORK_BRANCH:

NEW_COMMITS:

CORE_ALGORITHM_LOGIC_MODIFIED:
YES/NO

VALIDATION_FRAMEWORK_MODIFIED:

ACTIVE_RELOCALIZATION_STATE_MACHINE_FOUND:

ACTUAL_STATE_NAMES:

BUILD:

TESTS:

S05:
result:
input_condition:
expected:
observed:
important_transitions:
artifact_path:
plots:
evidence_images:
warnings:

S06:
result:
input_condition:
expected:
observed:
important_transitions:
cmd_vel_behavior:
artifact_path:
plots:
evidence_images:
warnings:

S07:
result:
candidate_source:
candidate_fields:
expected:
observed:
artifact_path:
plots:
evidence_images:
warnings:

S08:
result:
verification_logic:
expected:
observed:
recovery_transition:
navigation_health_after_recovery:
cmd_vel_after_recovery:
artifact_path:
plots:
evidence_images:
warnings:

DIRECT_DG_OUTPUTS_FAKED_BY_INJECTOR:
YES/NO

CMD_VEL_SAFETY:

VISIBLE_EXECUTION:

VISIBLE_EVIDENCE_CHECKPOINT_USED:
YES/NO

USER_WATCH_COMMANDS:

RVIZ:
config:
tested_on_gui:
result:

LIVE_MONITOR:
command:
fields:
result:

AUTOMATIC_GUI_CAPTURE:
available:
method:
installed_new_software:
NO

EVIDENCE_CAPTURE:
S05_image_count:
S06_image_count:
S07_image_count:
S08_image_count:

EVIDENCE_MANIFESTS:

DOCUMENT_EVIDENCE_INDEX:

EXPERIMENT_EVIDENCE_POLICY:

STATIC_PLOTS:

ROSBAGS:

CSV:

TIMELINES:

ACTIVE_RELOCALIZATION_SUMMARY:

REAL_ROBOT:
NO

GAZEBO:
NO

ZED_VIO:
NOT_STARTED

MMWAVE_RADAR:
NOT_STARTED

OFFICIAL_20CM_CLAIM:
NO

OFFICIAL_FEATURE_95_CLAIM:
NO

OFFICIAL_RELOCALIZATION_95_CLAIM:
NO

EVIDENCE_LEVEL:
SYNTHETIC_SOFTWARE_VALIDATION

GIT_STATUS:

BLOCKERS:

DISCOVERED_CORE_ALGORITHM_ISSUES:

NEXT_RECOMMENDED_STEP:

==================================================
三十三、停止条件
==================================================

以下任一情况出现立即停止：

1. 需要改变 Active Relocalization 核心语义才能PASS；
2. 需要降低算法阈值；
3. 需要 injector伪造 `/dg/*` 输出；
4. 测试链向真实 `/cmd_vel` 发布；
5. 发现真实机器人连接；
6. 需要Gazebo才能继续；
7. 需要ZED/CUDA/TensorRT；
8. 需要开始毫米波/VIO等新技术；
9. 需要伪造图片；
10. 需要伪造状态；
11. 需要删除历史FAIL；
12. 开始声称官方20cm/95%/>95%已经达到。

==================================================
三十四、本轮最终目标
==================================================

完成真实的：

S05 定位失效

↓

S06 主动恢复运动

↓

S07 Scan-to-Map candidate

↓

S08 多帧恢复验证

并且不仅得到：

PASS/FAIL

rosbag

CSV

timeline

plots

还必须让用户：

在 VMware Ubuntu 中实时看到实验过程

并保存足够的：

RViz截图

Monitor截图

关键状态截图

自动数据图

证据说明

供后续比赛方案书和答辩直接使用。

先进行开发和 headless 调试。

当最终 evidence run 准备好以后：

输出：

VISIBLE_EVIDENCE_CHECKPOINT

给出用户需要在 VMware 中打开的准确命令。

然后等待用户回复：

VISIBLE_READY

再执行最终正式证据批次。

完成后停止。

等待外部审核。

——DG-202611 ROUND B END——
~~~~

[本对话目录](README.md) | [总目录](../../README.md) | [Raw](https://raw.githubusercontent.com/weiyang02520-ops/dg202611-conversation-archive/main/conversations/first/part-036.md) | [上一卷](part-035.md) | [下一卷](part-037.md)
