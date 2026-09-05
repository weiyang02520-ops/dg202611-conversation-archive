# 机器人定位挑战赛分析 / 第 045 卷

[本对话目录](README.md) | [总目录](../../README.md) | [Raw](https://raw.githubusercontent.com/weiyang02520-ops/dg202611-conversation-archive/main/conversations/first/part-045.md) | [上一卷](part-044.md) | [下一卷](part-046.md)

## 消息 1725 / assistant / 分段 1 of 2

~~~~text
执行位置：当前 Claude Code 开发线程，请在本线程直接继续执行，不得转发。

DG-202611 ROUND B PRECHECK INTEGRITY APPROVAL
完成 S05—S08 Precheck 并停在 VISIBLE_EVIDENCE_CHECKPOINT

你最新状态报告已审核。

结论：

APPROVED_TO_CONTINUE = YES

本轮继续完成剩余实现、验证和 S05—S08 headless precheck。

两个新增判断正式批准：

1. PRECHECK 可以真实 FAIL，不要求四个场景必须 PASS。
2. S07 只能证明 synthetic closed-loop software behavior，不能证明真实定位精度。

不得为了得到漂亮结果调整冻结的核心算法。

==================================================
一、当前状态继续保持
==================================================

TARGET_WORKTREE_VERIFIED:

scenario_schema.py
synthetic_injector_node.py
evaluator_node.py

LOCAL_STAGING_ONLY:

result_writer.py

WRITTEN_TO_TARGET_WORKTREE / NOT_YET_INDEPENDENTLY_VERIFIED:

monitor_node.py
plot_results.py
RViz config
marker node
evidence helpers
MANUAL_CAPTURE_CHECKLIST.md

GIT_COMMITTED = NO

GIT_PUSHED = NO

NO_FINAL_EVIDENCE_SCENARIO_EXECUTED = YES

以上状态只有经过实际 target read-back / syntax / build / test 后才能升级。

==================================================
二、Precheck 不以 PASS 为目标
==================================================

PRECHECK_PURPOSE:

发现真实问题，而不是制造成功结果。

因此 S05—S08 headless precheck 可以：

PASS
或
FAIL

如果出现：

STOPPING 无法确认

ACTIVE_SCAN 超时

8 s segment_timeout 内转角未收敛

candidate 未产生

candidate 被真实阈值拒绝

VERIFYING 未完成

RECOVERED 未出现

NAV handoff 与预期不符

TF 不可用

消息时序不稳定

任何 assertion 失败

都必须：

保留该 run

记录 FAIL

分析真实根因

不得删除失败 run

不得重跑到成功后只保留成功结果。

==================================================
三、禁止为了 Precheck PASS 调参
==================================================

如果 precheck FAIL：

首先判断失败属于哪一类：

TEST_RIG_BUG

SCENARIO_DESIGN_BUG

ROS2_INTEGRATION_BUG

TIMING_ASSUMPTION_ERROR

SYNTHETIC_PLANT_LIMITATION

SURROGATE_LIMITATION

REAL_ALGORITHM_BEHAVIOR

UNKNOWN

只有：

测试框架自身错误

场景 YAML 明显违反真实接口

消息没有正确送到 SUT

字段/类型错误

entry point错误

测试基础设施 bug

这类问题允许修复。

以下禁止：

降低 Scan-to-Map threshold

扩大核心 timeout

修改 active relocalization state machine

改变真实触发条件

缩短真实验证帧数

绕过 STOPPING

直接修改 odom yaw

直接注入 candidate

直接注入 RECOVERED

伪造 /dg/* 输出

增加 synthetic /cmd_vel_nav

加入 perfect map→odom

修改 ylhb_base 核心语义。

如果只有修改上述核心行为才能 PASS：

STOP。

返回：

BLOCKER_REQUIRES_CORE_CHANGE

==================================================
四、segment_timeout 特别规则
==================================================

已知真实：

segment_timeout = 8 s

如果 ACTIVE_SCAN 在 synthetic plant 下无法在真实 8 s 内完成：

不得第一反应修改 timeout。

必须先检查：

实际恢复 angular cmd

arbiter 输出

/dg/test_cmd_vel

plant integration dt

实际 odom yaw

目标 segment angle

角度 wrap

到达判定

消息频率

simulation wall-time / ROS-time关系

是否存在 plant bug。

如果 plant 按正确输入和正确运动学积分，但真实恢复控制确实无法满足状态机要求：

如实 FAIL。

不要把算法改到适配测试台。

==================================================
五、S07 证据边界正式冻结
==================================================

S07_SYNTHETIC_TRUTH_LIMITATION = ACKNOWLEDGED

当前 surrogate 使用 synthetic plant truth 构造带偏差 localization pose。

因此测试 rig 知道：

true pose

然后人为构造：

biased pose / seed

真实 Scan-to-Map matcher 再执行：

refine_pose_near_seed

根据 synthetic map + scan 恢复 candidate。

这属于合法 synthetic algorithm test。

但必须明确：

TRUE POSE IS AUTHORED BY TEST RIG

因此 S07 可以证明：

真实 Scan-to-Map 代码被调用；

真实 refine_pose_near_seed 被调用；

candidate 不是 injector直接发布；

真实 score / inlier_ratio / mean_distance 被计算；

真实接受/拒绝阈值被执行；

candidate 可以进入后续状态机；

synthetic closed-loop state chain 可以工作。

S07 不可以证明：

真实机器人定位精度；

真实 LiDAR 定位精度；

实际环境 Scan-to-Map 性能；

20 cm定位指标；

真实重定位成功率；

Gazebo性能；

实车性能。

==================================================
六、0.975 等 Matcher 数字的表述规则
==================================================

类似：

score = 0.975
inlier_ratio = 1.000
mean_distance = 0.009

只能标：

SYNTHETIC_MATCH_QUALITY_RESULT

不得标：

LOCALIZATION_ACCURACY

不得因为：

mean_distance = 0.009

写成：

定位误差 9 mm。

两者不是同一指标。

必须在：

06_results_and_evidence.md

明确写出该区别。

==================================================
七、Ground Truth 使用必须显式记录
==================================================

在 S07 / S08 artifact 中增加或确认：

GROUND_TRUTH_AVAILABLE = YES

GROUND_TRUTH_OWNER = SYNTHETIC_TEST_RIG

GROUND_TRUTH_USED_BY_SUT = NO

GROUND_TRUTH_USED_BY_SURROGATE = YES/按真实情况

GROUND_TRUTH_USED_FOR_ASSERTION = 仅允许描述测试输入/误差构造，不得替代 SUT 输出成功条件

GROUND_TRUTH_LEAKAGE_TO_MATCHER = NO

如果 matcher、state machine、candidate acceptance 或其他 SUT 路径可以直接读取 true pose：

STOP。

返回：

GROUND_TRUTH_LEAKAGE_BLOCKER

==================================================
八、完成 result_writer
==================================================

继续完成：

result_writer.py

包括：

FIELD_ROLES

field_roles.csv

S05—S08 assertions

precheck metadata

ground-truth disclosure fields

run classification。

完成后：

写入目标 VM

read-back

py_compile

grep

再升级状态为：

TARGET_WORKTREE_VERIFIED

==================================================
九、完成四个 Scenario YAML
==================================================

完成：

S05
S06
S07
S08

每个必须有：

PRECONDITIONS

INPUT_CONDITIONS

REAL_TRIGGER_PATH

EXPECTED_TRANSITIONS

REQUIRED_OUTPUTS

PASS_CRITERIA

FAIL_CRITERIA

SAFETY_ASSERTIONS

OUT_OF_SCOPE

KNOWN_SYNTHETIC_LIMITATIONS

特别：

不得在 YAML 中直接写出 SUT 应输出的状态作为注入数据。

==================================================
十、继续保持真实 Closed Loop
==================================================

ACTIVE_SCAN：

active relocalization
→ recovery cmd
→ arbiter
→ /dg/test_cmd_vel
→ synthetic plant
→ odom yaw
→ scan变化

S07：

真实 scan_map_relocalization_node
→ refine_pose_near_seed
→ candidate quality
→ candidate accept/reject

S08：

candidate
→ VERIFYING
→ real multi-frame verification
→ RECOVERED

不允许 shortcut。

==================================================
十一、完成 setup.py 与 Entry Point 验证
==================================================

注册所有新增 entry point。

之后必须实际执行：

ros2 pkg executables dg_synthetic_validation

确认新增 executable 能被 ROS2发现。

只看 setup.py 文本不算验证。

==================================================
十二、FINAL_SUBAGENT_MERGE_REVIEW
==================================================

主 Claude 现在必须亲自验证：

monitor_node.py

plot_results.py

RViz config

marker node

evidence helpers

MANUAL_CAPTURE_CHECKLIST.md

以及所有其他 subagent 修改。

至少执行：

git status

git diff --stat

git diff

关键文件 read-back

py_compile

imports检查

entry point检查

RViz config检查

marker订阅/发布检查

确认没有：

重复实现

旧接口

死代码

perfect map→odom

字段名不匹配

Input/SUT role混淆

subagent互相覆盖。

返回：

FINAL_SUBAGENT_MERGE_REVIEW = PASS/FAIL

==================================================
十三、TARGET_WORKTREE_VERIFICATION
==================================================

在任何 precheck 前执行。

必须实际确认：

CURRENT_BRANCH

HEAD

PARENT

git status

git diff --stat

InputState

AmclSurrogate

PlantConfig

result_writer

FIELD_ROLES

scenario YAML

setup.py

monitor

plots

marker

RViz

evidence helper

docs

TF publication。

必须确认：

PERFECT_MAP_ODOM_PRESENT = NO

STATIC_TF = base_footprint→laser only

DYNAMIC_PLANT_TF = odom→base_footprint

SURROGATE_SEPARATED = YES

INPUTSTATE_SEPARATED = YES

UNEXPECTED_TF = NONE

NO_SYNTHETIC_DG_OUTPUT = YES

NO_SYNTHETIC_CMD_VEL_NAV = YES

==================================================
十四、Build / Test
==================================================

然后执行：

py_compile

targeted build

synthetic package tests

relevant core functional tests

entry-point verification

如果执行 full colcon test：

如实报告。

历史 lint / pep257 failure 不得隐瞒，也不得误写成核心算法失败。

==================================================
十五、Precheck Root
==================================================

固定：

`/home/weiyang/dg202611_ws/results/synthetic/precheck/`

每个 run 必须带：

RUN_CLASS = PRECHECK

NOT_FINAL_EVIDENCE = TRUE

NOT_FOR_DOCUMENT_CLAIM = TRUE

EVIDENCE_LEVEL = SYNTHETIC SOFTWARE VALIDATION PRECHECK

失败 run 也必须保留。

不得覆盖。

==================================================
十六、执行四个 Headless Precheck
==================================================

按顺序：

S05

S06

S07

S08

每个都要输出真实：

PASS / FAIL

不得默认预期四个必须 PASS。

如果某一场景 FAIL：

如果不会污染后续场景，可以继续跑剩余场景以获得诊断信息。

如果失败揭示：

core blocker

ground-truth leakage

安全问题

状态机前提错误

则停止后续测试并报告。

==================================================
十七、失败记录要求
==================================================

任何 precheck FAIL 都必须进入：

04_debug_and_failures.md

包括：

RUN_ID

SCENARIO

FAILED_ASSERTION

LAST_VALID_STATE

EXPECTED_STATE

ACTUAL_STATE

INPUT_STATE

SUT_OUTPUT

ROOT_CAUSE_STATUS

IS_TEST_RIG_BUG

IS_CORE_ALGORITHM_ISSUE

PROPOSED_FIX

CORE_CHANGE_REQUIRED

RETEST_NEEDED

不得只写：

“测试失败”。

==================================================
十八、S08 NAV handoff 规则不变
==================================================

没有 Nav2。

没有 /cmd_vel_nav。

因此：

RECOVERED
→ NAV selected
→ NAV_SOURCE_STALE
→ zero output

仍然属于合法 safe handoff。

不得为了好看 synthetic 生成导航输入。

==================================================
十九、GUI规则不变
==================================================

GUI_CAPTURE_READY = MANUAL_ONLY

不要继续尝试自动抓 Wayland桌面。

最终正式证据运行由用户手动截图。

本轮只保证：

Monitor ready

RViz ready

Marker ready

Manual capture checklist ready。

==================================================
二十、Docs 第二次检查
==================================================

docs agent 完成后：

主 Claude 必须亲自 read-back。

确认过程文档真实覆盖：

错误的 pushed/verified 报告

target worktree read-back纠正

map→odom移除

surrogate拆分

InputState

dead plant method删除

FIELD_ROLES

capture black-frame false positive

brightness floor

Wayland investigation

MANUAL_ONLY

subagent merge review

S05—S08设计

precheck failures/successes

S07 synthetic truth limitation

0.975不得解释为定位精度

ground truth disclosure

最终 target verification。

==================================================
二十一、本轮仍禁止
==================================================

禁止：

Gazebo

真实机器人建模

CAD审计

ZED VIO

Radar

高程开发

Jetson

真车

比赛指标

北斗短报文

Git push

正式 final evidence batch。

==================================================
二十二、完成后停在 Checkpoint
==================================================

全部完成后必须停止。

输出：

VISIBLE_EVIDENCE_CHECKPOINT

不得直接执行 FINAL S05—S08。

==================================================
二十三、Checkpoint 返回格式
==================================================

VISIBLE_EVIDENCE_CHECKPOINT:

IMPLEMENTATION_STATUS:

TARGET_WORKTREE_VERIFICATION:
branch:
head:
parent:
git_status:
diff_summary:

R
~~~~

## 消息 1725 / assistant / 分段 2 of 2

~~~~text
ESULT_WRITER:
status:

FIELD_ROLE_VALIDATION:
PASS/FAIL

SCENARIO_YAMLS:
S05:
S06:
S07:
S08:

PERFECT_MAP_ODOM_PRESENT:
YES/NO

STATIC_TF:

DYNAMIC_TF:

SURROGATE_SEPARATED:
YES/NO

INPUTSTATE_SEPARATED:
YES/NO

GROUND_TRUTH_DISCLOSURE:
status:

GROUND_TRUTH_LEAKAGE_CHECK:
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

SYNTHETIC_PACKAGE_TESTS:

FULL_COLCON_TEST:
NOT_RUN / actual result

PRECHECK_ROOT:

S05_HEADLESS_PRECHECK:
PASS/FAIL
run_path:
root_cause_if_fail:

S06_HEADLESS_PRECHECK:
PASS/FAIL
run_path:
root_cause_if_fail:

S07_HEADLESS_PRECHECK:
PASS/FAIL
run_path:
root_cause_if_fail:

S07_MATCH_RESULT:
score:
inlier_ratio:
mean_distance:

S07_ACCURACY_CLAIM_ALLOWED:
NO

S08_HEADLESS_PRECHECK:
PASS/FAIL
run_path:
root_cause_if_fail:

ALL_PRECHECKS_MARKED_NOT_FINAL:
YES/NO

FAILED_PRECHECKS_PRESERVED:
YES/NO

CORE_STATE_MACHINE_UNCHANGED:
YES/NO

CORE_THRESHOLDS_UNCHANGED:
YES/NO

REAL_CMD_VEL_PUBLISHERS:
0/OTHER

MONITOR_READY:
YES/NO

RVIZ_READY:
YES/NO

MARKER_READY:
YES/NO

GUI_CAPTURE_READY:
MANUAL_ONLY

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

无论 precheck 是：

4 PASS

3 PASS / 1 FAIL

或其他真实结果，

都不得为了得到“全绿”擅自调整核心算法。

先把真实结果交回来审核。

只有收到用户明确：

VISIBLE_READY

之后，才允许进入最终正式 evidence batch。

——DG-202611 ROUND B PRECHECK INTEGRITY APPROVAL END——
~~~~

## 消息 1726 / user / 分段 1 of 1

~~~~text
I've been executing Round B in this thread. Your two new rulings match constraints I'd already flagged, so they're reflected in the design rather than needing rework.

## Status (your terminology)

**TARGET\_WORKTREE\_VERIFIED:** `scenario_schema.py`, `synthetic_injector_node.py`, `evaluator_node.py` — written to the VM, `py_compile` passed, grep-confirmed that `amcl_follows_plant` / `covariance_after_match` / `effective_covariance` are gone, remaining `map_frame_id` uses are message-header semantics only, and the static-transform function emits `base_footprint→laser` alone.

**LOCAL\_STAGING\_ONLY:** `result_writer.py` — field roles, helpers, and all four scenario criteria now written; not yet on the VM.

**WRITTEN\_TO\_TARGET\_WORKTREE / NOT\_YET\_INDEPENDENTLY\_VERIFIED:** `monitor_node.py`, `plot_results.py`, `.rviz`, marker node, evidence helpers, `MANUAL_CAPTURE_CHECKLIST.md`.

**GIT\_COMMITTED: NO. GIT\_PUSHED: NO. NO\_FINAL\_EVIDENCE\_SCENARIO\_EXECUTED: YES.**

## How the criteria enforce the closed loop

The checks are written so a shortcut cannot pass them. S06 requires the *supervisor's own* `reloc_command_angular_z` to be non-zero, the arbiter to have forwarded it (`cmd_vel_source_inferred == RECOVERY`), the test topic to carry that rotation, and — critically — `odom_yaw` to have actually moved more than 1°. A scripted yaw change with no command upstream fails the first three; a command with a dead plant fails the last. It also asserts `cmd_linear_x` stays zero throughout, which matches the real code where `command_linear_x` is hardcoded to 0.0.

S07 requires a real seed request to precede the first accepted match in wall-clock order, so an unattributed candidate can't satisfy it, and it records that the accepted candidate met the **unmodified** 0.45/0.45/0.30 thresholds rather than any threshold I chose. S08 requires `WAITING_CANDIDATE → VERIFYING → RECOVERED` in order plus `max(verification_count) >= 3`, so a single-frame candidate cannot produce a pass.

The S08 handoff check asserts only that recovery *released* the command. It deliberately does not assert navigation resumed, and the docstring says why.

Ground-truth disclosure is emitted per run with `GROUND_TRUTH_USED_BY_SUT: NO` and `GROUND_TRUTH_LEAKAGE_TO_MATCHER: NO`, both of which rest on the verified fact that the only TF consumer in the chain reads `base_footprint ← laser` and no `map→odom` exists.

## Remaining before checkpoint

Write `result_writer` to target and verify it; the four YAMLs; `setup.py` plus `ros2 pkg executables` verification; precheck run-classification in `scenario_runner`; `FINAL_SUBAGENT_MERGE_REVIEW`; `TARGET_WORKTREE_VERIFICATION`; targeted build/tests; the four prechecks into `results/synthetic/precheck/`; docs second pass.

On the `0.975` framing — I'll record it as `SYNTHETIC_MATCH_QUALITY_RESULT` and state explicitly that `mean_distance = 0.009` is a scan-to-map residual against a synthetic grid, not a 9 mm positioning error. Those are different quantities and conflating them is precisely how a synthetic number turns into a false accuracy claim.

I'll stop at `VISIBLE_EVIDENCE_CHECKPOINT` and report whatever the prechecks actually produce, including failures, without tuning toward green.
~~~~

## 消息 1728 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "reasoning_recap",
  "content": "思考了 5s"
}
~~~~

[本对话目录](README.md) | [总目录](../../README.md) | [Raw](https://raw.githubusercontent.com/weiyang02520-ops/dg202611-conversation-archive/main/conversations/first/part-045.md) | [上一卷](part-044.md) | [下一卷](part-046.md)
