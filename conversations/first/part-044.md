# 机器人定位挑战赛分析 / 第 044 卷

[本对话目录](README.md) | [总目录](../../README.md) | [Raw](https://raw.githubusercontent.com/weiyang02520-ops/dg202611-conversation-archive/main/conversations/first/part-044.md) | [上一卷](part-043.md) | [下一卷](part-045.md)

## 消息 1722 / assistant / 分段 1 of 2

~~~~text
执行位置：当前 Claude Code 开发线程，请在本线程直接继续执行，不得转发。

DG-202611 ROUND B CONTINUE TO VISIBLE EVIDENCE CHECKPOINT

你最新状态报告已审核。

结论：

APPROVED_TO_CONTINUE: YES

PRECHECK_ROOT:
`/home/weiyang/dg202611_ws/results/synthetic/precheck/`

该目录正式批准用于 S05—S08 headless precheck。

不要更换位置。

不要把 precheck 与最终 evidence batch 混在一起。

==================================================
一、Precheck 目录规则
==================================================

所有 S05—S08 headless precheck 必须写入：

`results/synthetic/precheck/`

禁止写入未来最终正式 evidence run 所使用的目录。

每个 precheck run 的：

result.json
result.md
samples.csv
timeline.csv
plots
summary

必须明确记录：

RUN_CLASS = PRECHECK

NOT_FINAL_EVIDENCE = TRUE

NOT_FOR_DOCUMENT_CLAIM = TRUE

EVIDENCE_LEVEL = SYNTHETIC SOFTWARE VALIDATION PRECHECK

禁止文档线以后误把 precheck 当正式实验结果。

Precheck 结果必须永久保留，不覆盖。

==================================================
二、最终正式 Evidence Batch 必须与 Precheck 隔离
==================================================

最终正式 S05—S08 evidence batch：

不得写入：

`results/synthetic/precheck/`

不得覆盖任何 precheck。

正式运行建议使用：

`results/synthetic/final/`

或当前框架中等价、明确标识为 FINAL 的独立目录。

但正式目录只能在：

VISIBLE_READY

之后执行最终 evidence batch 时使用。

当前禁止提前运行。

==================================================
三、继续保持术语准确
==================================================

当前状态术语保持：

scenario_schema.py
synthetic_injector_node.py
evaluator_node.py

= TARGET_WORKTREE_VERIFIED

result_writer.py

= LOCAL_STAGING_ONLY

直到实际写入目标 VM 并完成 read-back / py_compile / grep 后，才能升级为：

TARGET_WORKTREE_VERIFIED

monitor_node.py
plot_results.py
RViz config
marker node
evidence helpers
MANUAL_CAPTURE_CHECKLIST.md

当前状态只能写：

WRITTEN_TO_TARGET_WORKTREE
NOT_YET_INDEPENDENTLY_VERIFIED

不能提前写 VERIFIED。

GIT_COMMITTED = NO

GIT_PUSHED = NO

==================================================
四、继续完成 result_writer
==================================================

完成：

result_writer.py

并写入目标 VM。

字段角色继续保持：

INPUT
SUT_OUTPUT
DERIVED_ASSERTION
METADATA

通过：

FIELD_ROLES

和：

field_roles.csv

表达字段边界。

批准保留现有字段名。

不要求为了增加 `in_` 前缀而破坏已完成 plotting compatibility。

但必须保证：

一个字段只能有一个 role。

不得同一字段同时作为输入和算法输出。

INPUT 至少包括：

scenario phase
scheduled GNSS condition
scheduled LiDAR condition
scheduled AMCL condition
plant mode
surrogate mode
scheduled covariance
TF availability
odom stimulus/state
amcl stimulus/state

SUT_OUTPUT 至少包括：

navigation health
health reason
active relocalization state
trigger reason
candidate state
candidate score
candidate inlier_ratio
candidate mean_distance
verification state/count
arbiter source
arbiter reason
/dg/test_cmd_vel

DERIVED_ASSERTION 至少包括：

required transition observed
candidate accepted
verification completed
recovered observed
safe handoff observed
real /cmd_vel publisher check
scenario PASS/FAIL

==================================================
五、完成 S05—S08 场景判据
==================================================

分别为：

S05
S06
S07
S08

定义真实：

PRECONDITIONS

INPUT_CONDITIONS

REAL_TRIGGER_PATH

EXPECTED_STATE_TRANSITIONS

REQUIRED_SUT_OUTPUTS

PASS_CRITERIA

FAIL_CRITERIA

SAFETY_ASSERTIONS

OUT_OF_SCOPE

必须依据真实 state machine。

真实状态固定：

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

禁止：

LOST
SEARCHING

不得用：

“观察到 relocalization activity”

作为 PASS。

==================================================
六、完成四个 Scenario YAML
==================================================

完成：

S05
S06
S07
S08

scenario YAML。

场景必须保证：

S05—S08 的状态转换由真实算法输出驱动。

不得通过 scenario YAML 直接指定：

active relocalization state

candidate success

verification success

RECOVERED

等 SUT 输出。

==================================================
七、Synthetic closed loop 继续保持边界
==================================================

真实闭环必须保持：

active relocalization node
→ recovery cmd
→ cmd_vel arbiter
→ /dg/test_cmd_vel
→ SYNTHETIC_KINEMATIC_PLANT
→ odom/yaw变化
→ scan geometry变化
→ algorithm继续判断

禁止 injector 根据时间直接修改 yaw 伪造主动旋转。

==================================================
八、AMCL Surrogate 继续独立
==================================================

保持：

SyntheticKinematicPlant
≠
AmclSurrogate

不得重新合并。

如果 AmclSurrogate 使用 synthetic truth：

必须记录：

GROUND_TRUTH_ACCESS = YES

但必须明确：

只属于 synthetic localization surrogate。

不得将其结果解释为：

真实 AMCL recovery。

==================================================
九、TF 继续保持冻结状态
==================================================

允许：

odom → base_footprint

base_footprint → laser

禁止 synthetic plant 发布：

perfect map → odom

再次验证：

static transform function 只能发：

base_footprint → laser

剩余 map_frame_id 只能用于：

message header/frame semantics

不得用于 perfect ground-truth TF injection。

==================================================
十、完成 setup.py
==================================================

注册本轮新增 executable entry points。

完成后必须实际检查：

`ros2 pkg executables dg_synthetic_validation`

确认所有需要的 entry points 已出现。

不得仅检查 setup.py 文本。

==================================================
十一、主 Claude 必须执行 FINAL_SUBAGENT_MERGE_REVIEW
==================================================

在 build/test 前：

主 Claude 必须亲自统一检查所有 subagent 修改。

至少：

git status

git diff --stat

git diff

关键文件 read-back

imports

syntax

setup.py

entry points

monitor_node.py

plot_results.py

RViz config

marker node

evidence helper

manual capture checklist

禁止直接相信 subagent completion summary。

输出：

FINAL_SUBAGENT_MERGE_REVIEW:
PASS/FAIL

==================================================
十二、完成 TARGET_WORKTREE_VERIFICATION
==================================================

在任何 headless precheck 前：

必须在 Ubuntu103 目标 VM 实际验证。

记录：

branch

HEAD

parent

git status

git diff --stat

关键文件 read-back

grep InputState

grep AmclSurrogate

grep PlantConfig

grep map→odom相关发布

grep static TF

grep setup entry points

py_compile

确认：

PERFECT_MAP_ODOM_PRESENT = NO

SURROGATE_SEPARATED = YES

INPUTSTATE_SEPARATED = YES

BASE_TO_LASER_STATIC_TF = YES

ODOM_TO_BASE_DYNAMIC_TF = YES

UNEXPECTED_TF = NONE

如果任何一项与报告不一致：

STOP。

先修正。

不要运行 precheck。

==================================================
十三、测试
==================================================

完成：

targeted build

py_compile

synthetic package tests

relevant core functional tests

entry-point verification

不要将历史 flake8/pep257 问题误写为核心功能失败。

如果运行 full colcon test：

必须如实报告。

不得写：

FULL_COLCON_TEST = PASS

除非真的零失败。

==================================================
十四、执行 S05—S08 Headless Precheck
==================================================

仅在前述验证全部通过后运行：

S05 headless precheck

S06 headless precheck

S07 headless precheck

S08 headless precheck

所有结果必须写入：

`results/synthetic/precheck/`

每次 run 必须标识：

PRECHECK

不是 final evidence。

重点检查：

真实 trigger path

STOPPING 是否成立

ACTIVE_SCAN 是否由 /dg/test_cmd_vel 驱动

candidate 是否来自真实 Scan-to-Map

refine_pose_near_seed 是否真实执行

VERIFYING 是否真实进入

multi-frame verification 是否发生

RECOVERED 是否真实观察到

NAV_SOURCE_STALE 是否符合预期

real /cmd_vel publisher count 是否始终为 0

==================================================
十五、S08 恢复后不要伪造导航
==================================================

继续保持：

不发布 synthetic `/cmd_vel_nav`

RECOVERED 后：

arbiter releases NAV
→ NAV_SOURCE_STALE
→ zero command

是允许结果。

只能表述：

SAFE_CONTROL_HANDOFF_VERIFIED_IN_SYNTHETIC_SOFTWARE_VALIDATION

不能表述：

NAVIGATION_RESUMED

NAV2_RESUMED

ROBOT_CONTINUED_MISSION

==================================================
十六、GUI Capture 决策冻结
==================================================

GUI_CAPTURE_READY = MANUAL_ONLY

不要继续研究 x11grab / xwd / D-Bus 自动抓 GNOME Wayland 桌面。

保留 capture helper 的：

brightness floor
black-frame rejection
known-positive / known-negative validation

作为防止无效截图入证据库的保护。

正式 GUI screenshot：

用户手动执行。

GNOME keys：

Print

Shift+Print

Alt+Print

截图目录：

第一次截图后确认。

不得假设：

`~/Pictures/Screenshots`

已经存在。

==================================================
十七、开发过程记录必须第二次更新
==================================================

docs agent 完成后，主 Claude 必须再统一检查一次过程记录。

必须包含：

LOCAL_STAGING_ONLY 与 TARGET_WORKTREE_VERIFIED 的区别

之前错误报告“pushed/verified”的过程

docs agent 如何通过实际 target read-back 发现问题

map→odom 实际移除

AMCL surrogate 实际拆分

dead PlantState method 移除

InputState field-role separation

FIELD_ROLES 设计

为什么保留现有列名

capture helper black-frame false positive

brightness floor 修复

Wayland/XWayland调查

MANUAL_ONLY决策

subagent并发修改

FINAL_SUBAGENT_MERGE_REVIEW

TARGET_WORKTREE_VERIFICATION

precheck目录隔离设计

S05—S08 precheck结果。

==================================================
十八、本轮仍禁止
==================================================

禁止：

Gazebo

真实机器人建模

CAD审计

ZED VIO

毫米波雷达

高程算法

Jetson

真车

官方性能指标

北斗短报文

Git push

最终 evidence batch。

==================================================
十九、到 VISIBLE_EVIDENCE_CHECKPOINT 必须停止
==================================================

完成：

implementation
+
subagent merge review
+
target worktree verification
+
build/tests
+
S05—S08 precheck
+
development docs update

后：

立即停止。

输出：

VISIBLE_EVIDENCE_CHECKPOINT

不得执行最终 S05—S08 evidence batch。

==================================================
二十、Checkpoint 返回格式
==================================================

VISIBLE_EVIDENCE_CHECKPOINT:

IMPLEMENTATION_STATUS:

TARGET_WORKTREE_VERIFICATION:
branch:
head:
parent:
git_status:
diff_summary:

PERFECT_MAP_ODOM_PRESENT:
YES/NO

STATIC_TF:
list:

DYNAMIC_TF:
list:

SYNTHETIC_PLANT:
status:

SYNTHETIC_AMCL_SURROGATE:
status:
ground_truth_access:

INPUTSTATE:
status:

FIELD_ROLE_VALIDATION:
PASS/FAIL

FINAL_SUBAGENT_MERGE_REVIEW:
PASS/FAIL

SETUP_ENTRY_POINTS:
PASS/FAIL
list:

PY_COMPILE:
PASS/FAIL

TARGETED_BUILD:
PASS/FAIL

TARGETED_FUNCTIONAL_TESTS:
result:

SYNTHETIC_PACKAGE_TESTS:
result:

FULL_COLCON_TEST:
NOT_RUN / result

PRECHECK_ROOT:
path:

S05_HEADLESS_PRECHECK:
PASS/FAIL
run_path:

S06_HEADLESS_PRECHECK:
PASS/FAIL
run_path:

S07_HEADLESS_PRECHECK:
PASS/FAIL
run_path:

S08_HEADLESS_PRECHECK:
PASS/FAIL
run_path:

ALL_PRECHECKS_MARKED_NOT_FINAL:
YES/NO

CORE_STATE_MACHINE_UNCHANGED:
YES/NO

CORE_THRESHOLDS_UNCHANGED:
YES/NO

GROUND_TRUTH_LEAKAGE_CHECK:
PASS/FAIL

REAL_CMD_VEL_PUBLISHERS:
0/OTHER
~~~~

## 消息 1722 / assistant / 分段 2 of 2

~~~~text


MONITOR_READY:
YES/NO

RVIZ_READY:
YES/NO

GUI_CAPTURE_READY:
MANUAL_ONLY

MANUAL_SCREENSHOT_KEYS:
Print
Shift+Print
Alt+Print

SCREENSHOT_DIRECTORY:
CONFIRM_AFTER_FIRST_CAPTURE

DEVELOPMENT_RECORDS_UPDATED:
YES/NO

NO_FINAL_EVIDENCE_SCENARIO_EXECUTED:
YES/NO

USER_VMWARE_COMMANDS:

Terminal A:

Terminal B:

Terminal C:

WAITING_FOR:
VISIBLE_READY

到这里必须停止。

只有用户明确回复：

VISIBLE_READY

才允许进入最终 S05—S08 正式 evidence batch。

——DG-202611 ROUND B CONTINUE TO VISIBLE EVIDENCE CHECKPOINT END—— memcite
~~~~

## 消息 1723 / user / 分段 1 of 1

~~~~text
I've been executing Round B in this thread. Accurate status, using your terminology:
**TARGET\_WORKTREE\_VERIFIED:** `scenario_schema.py`, `synthetic_injector_node.py`, `evaluator_node.py` — written to the VM, `py_compile` passed, grep-confirmed that `amcl_follows_plant` / `covariance_after_match` / `effective_covariance` are gone, that remaining `map_frame_id` uses are message-header semantics only, and that the static-transform function emits `base_footprint→laser` alone.
**LOCAL\_STAGING\_ONLY:** `result_writer.py` — field-role separation and helpers written, per-scenario criteria still in progress.
**WRITTEN\_TO\_TARGET\_WORKTREE / NOT\_YET\_INDEPENDENTLY\_VERIFIED:** `monitor_node.py`, `plot_results.py`, `.rviz`, marker node, evidence helpers, `MANUAL_CAPTURE_CHECKLIST.md`.
**GIT\_COMMITTED: NO. GIT\_PUSHED: NO. NO\_FINAL\_EVIDENCE\_SCENARIO\_EXECUTED: YES.**
`PRECHECK_ROOT` accepted as `results/synthetic/precheck/`, with `RUN_CLASS = PRECHECK` and the not-final markers written into each run's artifacts.
Field roles are now explicit: `FIELD_ROLES` plus a `field_roles.csv` written beside `samples.csv`, one role per column, no column doing double duty. Input-side values are recorded as stimulus evidence and never consulted to assert algorithm success. I kept existing column names per your approval rather than adding an `in_` prefix, since renaming would have broken the already-tested plotting code for no evidential gain.
I also removed a dead `on_match_correction` method that was still sitting on `PlantState` — it implied the body handled match events, contradicting the plant/surrogate separation.
Remaining before the checkpoint: finish the S05–S08 criteria and the four YAMLs, register entry points and verify with `ros2 pkg executables`, then `FINAL_SUBAGENT_MERGE_REVIEW`, `TARGET_WORKTREE_VERIFICATION`, targeted build/tests, the four prechecks, and the docs second pass.
Two things worth raising now rather than at the checkpoint, since both could change what the prechecks mean.
The prechecks are genuine closed-loop runs, so a precheck can legitimately **FAIL** — for instance if `STOPPING` never confirms or the plant's rotation doesn't converge inside the real 8 s `segment_timeout`. If that happens I'll report the failure and its root cause rather than tuning anything toward a pass, since the alternatives all run into your frozen constraints.
Second, the honest limit on what S07 can show: the surrogate reads synthetic plant truth to construct its biased pose, so the matcher recovers a pose whose true value was authored by the test rig. The bias makes the recovery non-trivial and the thresholds stay untouched, but this remains synthetic software validation of the state chain, not evidence about localization accuracy. I'll state it that way in `06_results_and_evidence.md` rather than let the 0.975 score imply more.
~~~~

## 消息 1724 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "reasoning_recap",
  "content": "思考了 8s"
}
~~~~

[本对话目录](README.md) | [总目录](../../README.md) | [Raw](https://raw.githubusercontent.com/weiyang02520-ops/dg202611-conversation-archive/main/conversations/first/part-044.md) | [上一卷](part-043.md) | [下一卷](part-045.md)
