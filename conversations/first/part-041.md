# 机器人定位挑战赛分析 / 第 041 卷

[本对话目录](README.md) | [总目录](../../README.md) | [Raw](https://raw.githubusercontent.com/weiyang02520-ops/dg202611-conversation-archive/main/conversations/first/part-041.md) | [上一卷](part-040.md) | [下一卷](part-042.md)

## 消息 1702 / assistant / 分段 1 of 2

~~~~text
执行位置：当前 Claude Code 开发线程，请在本线程直接继续执行，不得转发到其他 Agent。

DG-202611 ROUND B CONTINUATION APPROVAL
S05—S08 合成闭环验证继续执行指令

你上一轮完成的状态机分析和离线 Scan-to-Map 检查已审核。

本轮允许继续开发，但以下决策、边界和证据规则必须严格执行。

==================================================
一、状态机分析正式批准
==================================================

STATE_MACHINE_ANALYSIS: APPROVED

真实状态名固定为：

NORMAL
SUSPECTED
TRIGGERED
STOPPING
ACTIVE_SCAN
WAITING_CANDIDATE
VERIFYING
RECOVERED
FAILED
MANUAL_REQUIRED

不存在：

LOST
SEARCHING

后续：

scenario YAML
result_writer
monitor
plots
RViz
evidence manifest
development log
最终报告

全部只能使用真实状态名。

禁止为了可视化方便创造虚假状态。

已确认的重要事实：

assess_health 将异常分为：

trigger_reasons

和

degraded_reasons

GNSS DEGRADED / REJECTED
+
LiDAR quality low

属于 degraded-only。

因此 S02—S04 类似的 GNSS/LiDAR 退化本身不能触发 active relocalization。

这意味着此前：

RELOCALIZATION_ACTIVITY_OBSERVED_OUT_OF_SCOPE

不得解释为主动重定位闭环成功。

S05—S08 必须使用真实代码中存在的 trigger path，例如：

AMCL covariance

AMCL stale

scan stale

scan-match quality

等真实触发条件。

禁止修改状态机，让 GNSS/LiDAR degraded-only 强行触发重定位。

==================================================
二、核心算法与阈值冻结
==================================================

你已经实际调用真实：

refine_pose_near_seed

并得到：

score = 0.975
inlier_ratio = 1.000
mean_distance = 0.009

当前阈值：

score >= 0.45
inlier_ratio >= 0.45
mean_distance <= 0.30

并已验证：

true yaw +10°
true yaw -10°

均能从 0° seed 恢复。

因此：

CORE_THRESHOLD_CHANGE: NOT ALLOWED
CORE_STATE_MACHINE_CHANGE: NOT ALLOWED
CORE_ALGORITHM_SEMANTIC_CHANGE: NOT ALLOWED

如果后续 S05—S08 只有通过修改：

核心阈值
状态机
算法语义
真实质量判据

才能 PASS：

立即停止。

返回 BLOCKER。

不得为了实验通过调低门槛。

==================================================
三、Synthetic Base Plant 批准
==================================================

SYNTHETIC_BASE_PLANT: APPROVED_WITH_BOUNDARIES

允许在：

dg_synthetic_validation

内部增加：

SYNTHETIC_KINEMATIC_PLANT

目的：

消费：

/dg/test_cmd_vel

并根据明确的平面运动模型生成 synthetic body motion。

这是 S05—S08 闭环必要组成：

算法输出恢复运动指令
→ synthetic body motion
→ 新 odom / sensor pose
→ 新观测
→ 算法继续判断

该 plant：

不得发布任何 /dg/* 算法输出。

不得发布：

/cmd_vel

不得伪造核心节点内部状态。

不得替代核心算法。

不得修改 ylhb_base 算法代码。

所有 plant 参数必须明确记录：

运动学模型
积分方法
更新频率
初始状态
速度输入
姿态更新
轮式/平面运动假设
是否加入噪声
是否加入延迟

当前阶段若无必要：

优先保持模型简单、确定性、可解释。

禁止为了“看起来真实”加入未经验证的机器人物理参数。

当前阶段仍属于：

SYNTHETIC SOFTWARE VALIDATION

不是：

GAZEBO PHYSICAL SIMULATION

不是：

REAL ROBOT MODEL

不是：

REAL ROBOT DATA

==================================================
四、Synthetic AMCL surrogate 必须单独定义
==================================================

如果 plant 或 validation framework 需要发布：

/amcl_pose

不得把它悄悄归类到 synthetic base plant。

必须单独命名和记录：

SYNTHETIC_AMCL_SURROGATE

因为：

body motion

和

localization observation

不是一回事。

如果 surrogate：

接收 /initialpose

根据 synthetic truth 更新 /amcl_pose

模拟恢复后的 AMCL 行为

必须明确记录。

报告必须写明：

这是 synthetic localization surrogate。

不得写成：

真实 AMCL 成功恢复。

不得写成：

AMCL 在真实地图完成重定位。

必须记录：

输入是什么
输出是什么
是否直接访问 synthetic ground truth
何时进入恢复
恢复延迟
是否有噪声
是否有故障模式

==================================================
五、TF 批准边界
==================================================

以下 TF 允许：

ODOM_TO_BASE:
APPROVED

BASE_TO_LASER:
APPROVED

例如：

odom → base_footprint

base_footprint → laser

其中：

base_footprint → laser

可通过：

/tf_static

提供。

这用于：

真实 ROS2 坐标转换逻辑
_scan_to_base_points
RViz显示
sensor frame转换

是允许的。

==================================================
六、map → odom 不允许默认注入 perfect truth
==================================================

PERFECT_MAP_TO_ODOM_FROM_PLANT:
NOT APPROVED BY DEFAULT

不要为了让 RViz 或 Scan-to-Map 工作，直接让 synthetic plant 发布一个完美：

map → odom

ground-truth transform。

原因：

map → odom 本身可能属于定位链的重要输出。

如果 plant 把真值直接送入：

可能形成：

GROUND_TRUTH_LEAKAGE

让算法间接获得正确答案。

首先确认：

_scan_to_base_points

真实需要哪些 TF。

如果仅需要：

laser → base

则不得增加：

map → odom。

如果后续某真实代码路径确实必须依赖：

map → odom

在实现之前先完成一份：

MAP_ODOM_TF_JUSTIFICATION

至少回答：

1. 哪个真实代码文件需要 map → odom？
2. 哪个函数读取？
3. 用于什么计算？
4. 是否进入被测算法？
5. 是否影响 candidate pose？
6. 是否影响 score？
7. 是否可能产生 ground-truth leakage？
8. 能否改用独立 ground_truth frame？
9. 能否通过已有算法真实输出获得？
10. 如果只能 synthetic 提供，为什么仍然不会把答案泄漏给被测算法？

如果存在无法排除的 ground-truth leakage：

停止。

不要实现。

返回 BLOCKER。

==================================================
七、Injector Boundary 更新规则
==================================================

原 synthetic injector 边界是 7 个输入 topic。

新增：

/tf
/tf_static

属于本轮经过批准的受控扩展。

但是必须在：

runtime_contract
development log
test protocol
evidence documentation

中明确记录：

OLD_INJECTOR_BOUNDARY

NEW_INJECTOR_BOUNDARY

WHY_CHANGED

TF_SOURCE

TF_SEMANTICS

GROUND_TRUTH_ACCESS

是否存在：

algorithm output fabrication

必须明确：

NO。

如果新增任何其他 synthetic input topic：

必须记录。

不能悄悄增加。

==================================================
八、S05—S08 目标
==================================================

继续完成：

S05
S06
S07
S08

最终目标不是“让 scenario PASS”。

而是验证真实软件状态链：

真实 trigger
→ SUSPECTED
→ TRIGGERED
→ STOPPING
→ ACTIVE_SCAN
→ WAITING_CANDIDATE
→ VERIFYING
→ RECOVERED

或在失败条件下进入：

FAILED
MANUAL_REQUIRED

具体场景必须根据实际代码行为设计。

不要按我们脑中的理想状态强行设计。

==================================================
九、STOPPING 必须真实成立
==================================================

当前发现：

linear.x = 0.05

而：

stop_velocity = 0.03

所以之前 synthetic 输入会导致 STOPPING 无法确认。

本轮必须让 synthetic body state 与真实控制输出形成合理闭环：

触发失败
→ arbiter输出停止/恢复控制
→ plant响应
→ odom速度下降
→ STOPPING真实确认

不得直接在 scenario 脚本中：

伪造 state=STOPPED

不得直接修改：

stop_velocity

不得跳过：

STOPPING。

==================================================
十、ACTIVE_SCAN 必须由真实 cmd_vel 驱动
==================================================

ACTIVE_SCAN 中机器人姿态变化必须来自：

真实 active_relocalization node

产生恢复 cmd_vel

经过：

cmd_vel arbiter

输出到：

/dg/test_cmd_vel

再由：

SYNTHETIC_KINEMATIC_PLANT

积分。

禁止 scenario injector 直接根据预设时间修改 yaw，让它看起来像机器人主动旋转。

也就是说：

ACTIVE_SCAN motion must be closed-loop driven.

必须可以证明：

algorithm command
→ arbiter
→ /dg/test_cmd_vel
→ plant
→ odom yaw change
→ new scan geometry

这一链路真实存在。

==================================================
十一、S07 Scan-to-Map
==================================================

S07 必须实际经过：

真实 scan_map_relocalization_node

真实：

refine_pose_near_seed

真实：

candidate quality

真实：

score

真实：

inlier_ratio

真实：

mean_distance

真实接受/拒绝逻辑。

Synthetic map / scan 可以由 validation framework 构造。

但必须：

几何关系一致
来源明确
可重复
无算法输出伪造

candidate 不得由 injector 直接发布成“正确答案”。

==================================================
十二、S08 多帧验证
==================================================

S08 必须实际验证：

candidate
→ VERIFYING
→ 多帧一致性/验证
→ RECOVERED

不能：

第一帧 candidate 一出现就直接写 RECOVERED。

必须记录：

candidate count
verification sample count
candidate consistency
verification duration
accepted/rejected candidates

以及：

恢复发生在哪一个真实状态转换。

==================================================
十三、S08 恢复后的 NAV_SOURCE_STALE 是允许结果
==================================================

已确认：

Nav2 本轮故意不运行。

也没有：

/cmd_vel_nav

publisher。

因此：

RECOVERED
→ arbiter切换回 NAV
→ NAV_SOURCE_STALE
→ final command = zero

属于：

EXPECTED_AND_ACCEPTABLE

不要为了让车“继续走”而 synthetic 发布：

/cmd_vel_nav

SYNTHETIC_CMD_VEL_NAV:
NOT ALLOWED

不要伪造导航恢复运动。

S08 最终可以证明：

safe control handoff verified in synthetic software validation

但不得写：

navigation resumed successfully

不得写：

Nav2 resumed navigation

不得写：

robot continued mission

除非以后真实 Nav2 接入后另外验证。

==================================================
十四、安全约束保持
==================================================

正式 S05—S08 中：

恢复输出继续只能进入：

/dg/test_cmd_vel

真实：

/cmd_vel

必须保持无 publisher。

每个 scenario：

开始前检查。

结束后检查。

记录：

real_cmd_vel_publishers_start
real_cmd_vel_publishers_end

两者必须：

0

如果不为0：

该场景 FAIL。

==================================================
十五、result_writer 扩展
==================================================

继续完善 result_writer。

至少加入与 S05—S08 有关的真实字段：

scenario
elapsed
phase

navigation health
navigation health reason

active relocalization state
active relocalization reason

trigger reason

candidate request state

candidate available

candidate accepted

candidate score
candidate inlier_ratio
candidate mean_distance

verification count
verification state

cmd_vel source
cmd_vel reason
cmd_vel linear
cmd_vel angular

odom x
odom y
odom yaw
odom linear
odom angular

amcl x
amcl y
amcl yaw
amcl covariance

synthetic plant state

synthetic amcl surrogate state

TF availability

real /cmd_vel publisher count

所有无法获得的字段：

明确留空或NA。

不得捏造。

==================================================
十六、场景 PASS 条件必须具体
==================================================

每个 S05—S08 都必须有：

PRECONDITIONS

INPUTS

EXPECTED_STATE_TRANSITIONS

REQUIRED_OBSERVATIONS

PASS_CRITERIA

FAIL_CRITERIA

OUT_OF_SCOPE

不得只写：

“看到重定位活动即PASS”。

必须验证：

真实状态顺序
真实触发原因
真实输出
真实候选
真实验证
真实安全行为。

==================================================
十七、Monitor 增强
==================================================

继续增强：

monitor_scenario

必须保持：

READ_ONLY

只订阅真实运行输出。

不得改变系统行为。

实时展示至少：

Scenario
Elapsed
Phase

GNSS
LiDAR
Fusion
Navigation Health

Active Relocalization State

Trigger Reason

Candidate State

Candidate Score

Candidate Inlier Ratio

Candidate Mean Distance

Verification Progress

cmd_vel source

cmd_vel reason

/dg/test_cmd_vel

odom yaw

AMCL state

Safety status

如果显示：

SYNTHETIC

必须明确标识：

SYNTHETIC SOFTWARE VALIDATION

避免用户截图以后误认为真车。

==================================================
十八、RViz
==================================================

继续完善 RViz evidence configuration。

允许增加：

只读 visualization marker

但 marker 必须完全基于：

真实算法输出
synthetic plant state
synthetic scenario phase

不得造：

算法没有输出的“成功状态”。

RViz建议展示：

RobotModel

TF

LaserScan

Map

Odometry

AMCL pose

candidate pose

trajectory/path

relocalization event marker

health/status text marker

如果当前 RobotModel 只是简化 URDF：

保持原样。

本轮不要进入 Gazebo。

本轮不要修改机器人真实模型。

==================================================
十九、GUI与自动证据采集
==================================================

已确认：

ffmpeg
xwd

存在。

无需安装：

tmux
gnome-screenshot
scrot
maim

禁止因为截图需求安装大型软件。

允许开发：

ffmpeg -f x11grab

或：

xwd

辅助证据采集工具。

但是不得依赖无法确认的 DISPLAY。

正式证据运行前必须：

检测当前 GUI session
DISPLAY
XAUTHORITY
XDG_RUNTIME_DIR

如果自动 GUI capture 无法可靠运行：

不要阻塞核心实验。
~~~~

## 消息 1702 / assistant / 分段 2 of 2

~~~~text


改为：

MANUAL_SCREENSHOT_CHECKLIST

==================================================
二十、VISIBLE EXECUTION 仍然是强制要求
==================================================

VISIBLE_EXECUTION_REQUIREMENT = MANDATORY

你可以：

后台开发
headless调试
headless预运行

但在最终 S05—S08 evidence batch 之前：

必须停止。

输出：

VISIBLE_EVIDENCE_CHECKPOINT

给用户提供：

VMware Ubuntu103 桌面上需要打开的终端和精确命令。

至少包括：

Terminal A:
monitor_scenario

Terminal B:
RViz

Terminal C:
可选实时日志

然后等待用户回复：

VISIBLE_READY

收到：

VISIBLE_READY

之前：

不得执行最终正式 S05—S08 evidence run。

==================================================
二十一、截图目标
==================================================

最终 evidence run 尽量保存：

01_nominal_before_failure

02_failure_onset

03_health_suspected_or_triggered

04_relocalization_triggered

05_stopping

06_active_scan_motion

07_waiting_candidate

08_candidate_quality

09_verifying

10_recovered

11_safe_nav_handoff

12_post_recovery_stable

但：

只有真实观察到才保存并标注。

如果某事件不存在：

不得为了凑12张图制造。

==================================================
二十二、Evidence Manifest
==================================================

继续建立：

CSV
+
Markdown

至少字段：

filename

scenario

run_id

timestamp

elapsed

phase

event

source

real_state

caption

evidence_level

synthetic_or_real

notes

当前全部必须标：

SYNTHETIC SOFTWARE VALIDATION

NOT REAL ROBOT DATA

NOT COMPETITION PERFORMANCE EVIDENCE

==================================================
二十三、Document Evidence Index
==================================================

生成：

docs/dg202611/DOCUMENT_EVIDENCE_INDEX.md

为后续方案书/PPT记录：

推荐图片

图片路径

对应场景

事件

建议标题

建议图注

可以支撑什么结论

不能支撑什么结论

例如：

可以：

“主动重定位状态机在ROS2合成闭环中完成触发、主动扫描、候选匹配和多帧恢复。”

不可以：

“机器人在真实复杂环境中实现高成功率重定位。”

==================================================
二十四、开发过程记录永久规则
==================================================

DEVELOPMENT_PROCESS_RECORDING = MANDATORY

这是永久要求。

本轮必须同步记录整个开发过程。

至少建立/更新：

01_goal_and_scope.md

02_architecture_and_design.md

03_implementation_log.md

04_debug_and_failures.md

05_test_protocol.md

06_results_and_evidence.md

07_open_issues.md

08_handoff_summary.md

如果已有对应结构：

沿用，不要重复创建。

必须记录：

为什么需要 synthetic plant

为什么原 STOPPING 无法确认

为什么需要 TF

哪些 TF 被批准

为什么禁止 perfect map→odom

状态机真实状态

degraded_reasons vs trigger_reasons

S02—S04 为什么不能证明主动重定位

refine_pose_near_seed 离线验证过程

所有失败实验

报错

根因

修复方式

无效尝试

最终命令

最终结果路径

证据路径

Git commit

尚未验证内容

下一步建议。

失败过程不能删除。

==================================================
二十五、特别记录边界决策
==================================================

本轮至少形成明确记录：

DECISION_001:
synthetic base plant approved

DECISION_002:
synthetic AMCL surrogate must be separated

DECISION_003:
odom→base approved

DECISION_004:
base→laser approved

DECISION_005:
perfect map→odom not approved by default

DECISION_006:
no synthetic /cmd_vel_nav

DECISION_007:
NAV_SOURCE_STALE after RECOVERED is acceptable

DECISION_008:
no core threshold/state-machine modifications

以后文档线需要理解为什么这样设计时：

可以直接使用这些过程记录。

==================================================
二十六、测试要求
==================================================

继续：

build
targeted tests
synthetic package tests
py_compile

如有新增测试：

必须运行。

不要把现有仓库 lint/pep257 大量历史失败写成：

core behavior failure。

但也不得把 full colcon test 写成 PASS。

报告必须严格区分：

TARGETED_FUNCTIONAL_TESTS

SYNTHETIC_VALIDATION_TESTS

FULL_COLCON_TEST_STATUS

==================================================
二十七、禁止进入其他阶段
==================================================

本轮禁止：

Gazebo

URDF物理建模

CAD修改

ZED VIO开发

毫米波Radar

高程模型开发

Jetson

真实机器人

真实GNSS

真实LiDAR

比赛性能指标

北斗短报文

新技术选型。

这些全部属于后续技术包。

本轮只完成：

S05—S08 ROS2合成场景软件闭环验证。

==================================================
二十八、Git规则
==================================================

继续使用当前：

branch:
dg202611-synthetic-validation

不得 push。

运行产物：

results
rosbag
plots
screenshots
video

不得提交 Git，除非现有仓库规则明确要求某些小型示例证据。

代码和文档可提交。

正式 commit 前：

git status

确认没有意外大文件。

不要修改历史 commit。

==================================================
二十九、Stop Conditions
==================================================

以下任一情况立即停止并报告：

1. 必须修改核心状态机才能让 S05—S08 工作；

2. 必须降低核心算法阈值才能 PASS；

3. 必须伪造 /dg/* 输出；

4. 必须 synthetic 发布 /cmd_vel_nav 才能看起来恢复导航；

5. 必须让 perfect map→odom ground truth 进入被测算法才能完成 Scan-to-Map；

6. 必须修改核心 ylhb_base 算法语义；

7. /cmd_vel 出现真实 publisher；

8. synthetic plant 无法与真实 cmd_vel 闭环；

9. candidate 只能通过直接注入正确答案产生；

10. 无法区分 synthetic AMCL surrogate 和真实 AMCL 行为；

11. 发现当前 state machine 实际行为与本轮分析冲突。

返回：

BLOCKED

并说明：

exact blocker

affected file

affected state

why

minimum proposed change

是否会改变技术结论。

不得擅自越过。

==================================================
三十、接下来直接执行
==================================================

现在继续完成：

1. result_writer 扩展

2. S05—S08 scenario YAML

3. SYNTHETIC_KINEMATIC_PLANT

4. 如必要，SYNTHETIC_AMCL_SURROGATE

5. 批准范围内 TF

6. S05—S08 assertions

7. monitor增强

8. plots

9. RViz config

10. evidence helper

11. evidence manifest

12. DOCUMENT_EVIDENCE_INDEX

13. development process records

14. targeted build/tests

15. headless预验证

然后：

停止在

VISIBLE_EVIDENCE_CHECKPOINT

不要直接跑最终正式证据批次。

==================================================
三十一、到 Checkpoint 时返回
==================================================

VISIBLE_EVIDENCE_CHECKPOINT:

IMPLEMENTATION_STATUS:

S05_HEADLESS_PRECHECK:
PASS/FAIL

S06_HEADLESS_PRECHECK:
PASS/FAIL

S07_HEADLESS_PRECHECK:
PASS/FAIL

S08_HEADLESS_PRECHECK:
PASS/FAIL

STATE_MACHINE_UNCHANGED:
YES/NO

CORE_THRESHOLDS_UNCHANGED:
YES/NO

SYNTHETIC_PLANT_IMPLEMENTED:
YES/NO

SYNTHETIC_AMCL_SURROGATE_USED:
YES/NO

TF_PUBLISHED:

MAP_ODOM_TF_USED:
YES/NO

GROUND_TRUTH_LEAKAGE_CHECK:
PASS/FAIL

REAL_CMD_VEL_PUBLISHERS:
0/OTHER

MONITOR_READY:
YES/NO

RVIZ_READY:
YES/NO

GUI_CAPTURE_READY:
YES/NO/MANUAL_ONLY

DEVELOPMENT_RECORDS_UPDATED:
YES/NO

USER_VMWARE_COMMANDS:

Terminal A:

Terminal B:

Terminal C:

WAITING_FOR:
VISIBLE_READY

到这里停止。

等待用户明确回复：

VISIBLE_READY

之后才允许执行最终 S05—S08 evidence batch。

——DG-202611 ROUND B CONTINUATION APPROVAL END——
~~~~

[本对话目录](README.md) | [总目录](../../README.md) | [Raw](https://raw.githubusercontent.com/weiyang02520-ops/dg202611-conversation-archive/main/conversations/first/part-041.md) | [上一卷](part-040.md) | [下一卷](part-042.md)
