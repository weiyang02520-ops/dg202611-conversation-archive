# 机器人定位挑战赛分析 / 第 043 卷

[本对话目录](README.md) | [总目录](../../README.md) | [Raw](https://raw.githubusercontent.com/weiyang02520-ops/dg202611-conversation-archive/main/conversations/first/part-043.md) | [上一卷](part-042.md) | [下一卷](part-044.md)

## 消息 1714 / assistant / 分段 1 of 1

~~~~text
执行位置：当前 Claude Code 开发线程，请在本线程直接继续执行，不得转发。

DG-202611 ROUND B FINAL IMPLEMENTATION BEFORE VISIBLE CHECKPOINT

你最新报告已审核，可以继续。

本轮目标只有一个：

完成 S05—S08 正式证据运行之前的全部实现、测试和 headless 预检查，然后严格停在 VISIBLE_EVIDENCE_CHECKPOINT。

不得提前执行最终 evidence batch。

==================================================
一、本轮已批准事实
==================================================

1. InputState 分离设计：APPROVED

必须继续保持：

INPUT STATE
≠
SUT OUTPUT
≠
ASSERTION RESULT
≠
EVIDENCE

InputState 只记录实际喂给被测系统的 synthetic 输入。

不得把：

输入状态
预期状态
算法实际输出

混在一起。

最终 CSV / result.json / result.md 必须能够明确判断每个字段属于：

INPUT
SUT_OUTPUT
DERIVED_ASSERTION
METADATA

2. map→odom refactor：APPROVED AFTER TARGET VERIFICATION

当前目标状态必须保持：

synthetic framework 不发布 perfect map→odom ground truth。

剩余 map_frame_id 只能用于正常 ROS message header/frame 语义。

static transform 只允许：

base_footprint → laser

动态 plant TF 允许：

odom → base_footprint

不得重新引入 perfect：

map → odom

如果后续发现没有它 S07 无法运行：

STOP

返回 BLOCKER。

不得偷偷恢复。

3. Synthetic AMCL surrogate 必须独立

必须继续保持：

PlantConfig
≠
SyntheticAmclSurrogateConfig

body plant
≠
localization surrogate

如果 synthetic AMCL surrogate 使用 synthetic truth：

必须明确记录。

不得写成真实 AMCL recovery。

4. GUI Capture：

GUI_CAPTURE_READY = MANUAL_ONLY

冻结该结论。

当前实测：

Wayland session
seat0
tty2
Xwayland :0 -rootless
1280x800

x11grab 黑帧
xwd root BadMatch
D-Bus AccessDenied

不要再花时间尝试自动抓 GNOME 桌面。

capture helper 可以保留：

有效截图检测
brightness floor
black-frame rejection

但正式 GUI evidence 采用：

MANUAL SCREENSHOT

==================================================
二、截图假阳性必须进入失败记录
==================================================

必须在：

04_debug_and_failures.md

记录：

首次 capture helper 只做 flat-colour/channel spread 判断；

Mutter guard-window 黑帧仍有 channel spread≈11；

导致错误：

capture_available=true

但截图实际上无有效内容；

后增加：

brightness floor

并使用 known positive / known negative inputs 验证。

明确结论：

FILE_EXISTS != VALID_SCREENSHOT

CAPTURE_AVAILABLE != VALID_EVIDENCE

这一失败过程不得删除。

==================================================
三、开发报告真实性规则升级
==================================================

此前出现：

LOCAL STAGING UPDATED
但
TARGET VM WORKTREE NOT UPDATED

却被错误报告为：

pushed / verified

以后永久禁止这种状态混淆。

以后只允许以下术语：

LOCAL_STAGING_ONLY

WRITTEN_TO_TARGET_WORKTREE

TARGET_WORKTREE_VERIFIED

GIT_COMMITTED

GIT_PUSHED

其中：

GIT_PUSHED

只能用于真的执行：

git push

到 Git remote。

不要再用 “pushed” 表示：

从 Claude 本地临时目录写进虚拟机。

==================================================
四、任何 IMPLEMENTED / VERIFIED 必须 target read-back
==================================================

以后报告：

IMPLEMENTED

DEPLOYED

VERIFIED

之前必须至少满足与任务相应的：

target file read-back

grep

git diff

git status

py_compile

build

tests

entry-point inspection

之一或多个。

不能根据：

自己的 patch
subagent summary
local staging directory

直接宣布完成。

==================================================
五、Subagent 并发修改规则
==================================================

已确认：

monitor_node.py

plot_results.py

RViz config

等并发修改来自本线程自己的 subagents。

不是未知第三方。

允许继续。

但在 build/test 之前：

主 Claude 必须执行一次统一：

FINAL_SUBAGENT_MERGE_REVIEW

至少检查：

git status

git diff --stat

git diff

关键文件 read-back

冲突逻辑

重复逻辑

entry points

imports

syntax

不得只相信各 subagent 自己说完成。

==================================================
六、当前剩余任务
==================================================

现在继续完成：

1. result_writer

补齐 S05—S08 所需字段。

至少明确记录：

INPUT:

scenario phase
input GNSS state
input scan condition
input AMCL state
input plant state
input surrogate state

SUT OUTPUT:

navigation health
health reason
active relocalization state
trigger reason
candidate request
candidate score
candidate inlier_ratio
candidate mean_distance
verification progress
arbiter source
arbiter reason
/dg/test_cmd_vel
odom state

ASSERTION:

required transition seen
candidate accepted
verification completed
recovered
safety condition
real /cmd_vel publisher count
scenario PASS/FAIL

禁止一个字段同时承担 input 和 output 两种语义。

2. S05—S08 PASS / FAIL criteria

每个 scenario 必须单独写：

PRECONDITIONS

INPUT CONDITIONS

REAL TRIGGER PATH

EXPECTED REAL STATE TRANSITIONS

REQUIRED SUT OUTPUTS

SAFETY ASSERTIONS

PASS CRITERIA

FAIL CRITERIA

OUT_OF_SCOPE

不得用：

“看到重定位活动”

作为 PASS。

3. 四个 scenario YAML

完成：

S05
S06
S07
S08

场景设计必须来源于真实状态机。

禁止重新使用不存在的：

LOST
SEARCHING

4. setup.py

注册所有本轮新增 executable entry points。

注册完成后必须实际验证：

ros2 pkg executables dg_synthetic_validation

或等价目标机检查。

5. Monitor

完成增强，但保持 READ_ONLY。

6. plots

完成新状态和恢复链图表。

7. RViz

完成 evidence config。

8. evidence helper

保留 manual screenshot workflow。

==================================================
七、S05—S08 核心闭环仍然必须真实
==================================================

ACTIVE_SCAN motion 必须来自：

active relocalization node
→ recovery cmd
→ arbiter
→ /dg/test_cmd_vel
→ synthetic kinematic plant
→ odom/yaw变化
→ 新 scan geometry

禁止 injector 直接按照 scenario 时间修改机器人 yaw 来伪造主动扫描。

S07 candidate 必须来自真实：

scan_map_relocalization_node

和真实：

refine_pose_near_seed

禁止 injector 直接发布正确 candidate。

S08 RECOVERED 必须经过真实：

candidate
→ VERIFYING
→ multi-frame verification
→ RECOVERED

不得直接注入 RECOVERED。

==================================================
八、NAV_SOURCE_STALE 保持原结论
==================================================

Nav2 本轮没有运行。

不存在真实：

/cmd_vel_nav

因此：

RECOVERED
→ arbiter releases NAV
→ NAV_SOURCE_STALE
→ zero output

属于：

EXPECTED SAFE HANDOFF

不得 synthetic 发布 /cmd_vel_nav。

不得写：

navigation resumed

只能写：

safe control handoff verified in synthetic software validation

==================================================
九、核心冻结规则
==================================================

以下仍然禁止：

修改核心 state machine

降低核心 Scan-to-Map threshold

修改核心 algorithm semantics

伪造 /dg/* 输出

perfect map→odom ground truth injection

synthetic /cmd_vel_nav

修改 ylhb_base 只为让测试通过

如果必须做其中任何一个：

STOP

返回 BLOCKER。

==================================================
十、正式测试前必须完成 Target Worktree Verification
==================================================

在 headless pre-check 前增加：

TARGET_WORKTREE_VERIFICATION

必须在目标 Ubuntu103 worktree 实际执行和记录。

至少包含：

CURRENT_BRANCH

HEAD

PARENT

git status

git diff --stat

关键文件 read-back

grep map→odom / map transform publication

grep PlantConfig / SyntheticAmclSurrogate

grep InputState

static TF read-back

setup.py entry-point read-back

py_compile

确认：

PERFECT_MAP_ODOM_PRESENT:
NO

SURROGATE_SEPARATED:
YES

INPUTSTATE_SEPARATED:
YES

BASE_TO_LASER_STATIC_TF:
YES

ODOM_TO_BASE_PLANT_TF:
YES

UNEXPECTED_TF:
NONE

如果目标树与报告不一致：

不得继续测试。

先修正并重新验证。

==================================================
十一、Build / Test / Headless Precheck
==================================================

完成后执行：

targeted build

synthetic package tests

core relevant functional tests

py_compile

entry point check

S05 headless precheck

S06 headless precheck

S07 headless precheck

S08 headless precheck

Headless precheck 可以运行。

但它们必须清楚标记：

PRECHECK

不得作为最终 evidence batch。

不要覆盖未来正式结果。

==================================================
十二、过程记录必须同步更新
==================================================

DEVELOPMENT_PROCESS_RECORDING = MANDATORY

继续维护：

01_goal_and_scope.md
02_architecture_and_design.md
03_implementation_log.md
04_debug_and_failures.md
05_test_protocol.md
06_results_and_evidence.md
07_open_issues.md
08_handoff_summary.md

特别加入：

local staging / target worktree reporting mistake

docs agent 如何通过实际 read-back 发现问题

map→odom 实际移除验证

AMCL surrogate 实际拆分验证

InputState separation

capture black-frame false positive

brightness gate 修复

Wayland / XWayland 调查

MANUAL_ONLY 决策

subagent merge review

最终 target worktree verification。

==================================================
十三、本轮不进入 Gazebo
==================================================

仍然禁止：

Gazebo

真实机器人 URDF 重建

CAD参数审计

ZED VIO

毫米波 Radar

高程开发

Jetson

真车

比赛指标

北斗短报文

这些属于后续技术包。

==================================================
十四、最终 evidence batch 前必须停止
==================================================

完成：

implementation
+
target verification
+
build/tests
+
S05—S08 headless prechecks

之后：

必须停止。

不要执行正式 evidence batch。

输出：

VISIBLE_EVIDENCE_CHECKPOINT

==================================================
十五、Checkpoint 返回格式
==================================================

VISIBLE_EVIDENCE_CHECKPOINT:

IMPLEMENTATION_STATUS:

TARGET_WORKTREE_VERIFICATION:
branch:
head:
clean_or_dirty:
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
不得误写PASS

S05_HEADLESS_PRECHECK:
PASS/FAIL

S06_HEADLESS_PRECHECK:
PASS/FAIL

S07_HEADLESS_PRECHECK:
PASS/FAIL

S08_HEADLESS_PRECHECK:
PASS/FAIL

CORE_STATE_MACHINE_UNCHANGED:
YES/NO

CORE_THRESHOLDS_UNCHANGED:
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

只有收到用户明确回复：

VISIBLE_READY

之后，才允许执行最终 S05—S08 evidence batch。

——DG-202611 ROUND B FINAL IMPLEMENTATION BEFORE VISIBLE CHECKPOINT END——
~~~~

## 消息 1715 / user / 分段 1 of 1

~~~~text
I've been running Round B in this thread and have completed the implementation refactor plus delegated components; here's the accurate state, including a correction to my own earlier reporting.

## Terminology correction (your section 3)

My earlier "pushed / verified" claims were wrong and I'm adopting your vocabulary. Accurate status right now:

- `scenario_schema.py`, `synthetic_injector_node.py`, `evaluator_node.py` — **TARGET\_WORKTREE\_VERIFIED** (written, `py_compile` passed, grep-confirmed)
- `result_writer.py` — **LOCAL\_STAGING\_ONLY** (in progress, not yet written to the VM)
- `monitor_node.py`, `plot_results.py`, `.rviz`, marker node, evidence helpers, `MANUAL_CAPTURE_CHECKLIST.md` — written to target by subagents, **not yet independently verified by me**
- **GIT\_COMMITTED: NO. GIT\_PUSHED: NO.**

The docs agent caught the original discrepancy by reading the remote tree instead of trusting my summary — for most of this round the VM still emitted `map→odom` and still had the surrogate folded into `PlantConfig`, while I reported it as done.

## What's verified on target

Grep-confirmed after re-writing: `amcl_follows_plant`, `covariance_after_match`, and `effective_covariance` are gone from source; remaining `map_frame_id` uses are message-header semantics only; the static-transform function emits `base_footprint→laser` alone. `AmclSurrogate` appears in both schema and injector. `py_compile` passes on all three.

I also removed a dead `on_match_correction` method left on `PlantState` — it implied the body handles match events, contradicting the separation your section 1 requires.

## Field-role separation

`result_writer` now classifies every column as `INPUT` / `SUT_OUTPUT` / `DERIVED_ASSERTION` / `METADATA`, emitted both in `result.json` and as a `field_roles.csv` beside `samples.csv`. No column carries two roles. Input-side values (`odom_*`, `amcl_*`, plant/surrogate mode, scheduled covariance, TF availability) are recorded as stimulus evidence and never used to assert algorithm success.

One deliberate call: I kept the column *names* the plots agent already coded and tested against rather than renaming to an `in_` prefix, and expressed the boundary through `FIELD_ROLES` instead. Renaming would have broken already-verified plotting code for no evidential gain.

## Still outstanding before checkpoint

S05–S08 PASS/FAIL criteria in `result_writer`, the four scenario YAMLs, `setup.py` registration plus `ros2 pkg executables` verification, `FINAL_SUBAGENT_MERGE_REVIEW`, `TARGET_WORKTREE_VERIFICATION`, targeted build/tests, and the four headless prechecks. The docs agent is still running and will need a second pass for the items in your section 12.

`NO_FINAL_EVIDENCE_SCENARIO_EXECUTED: YES` — nothing has been run, and I'll stop at `VISIBLE_EVIDENCE_CHECKPOINT` for `VISIBLE_READY`.

One thing I want to flag now rather than at the checkpoint: the headless prechecks will write real run directories under `results/synthetic/`. To keep them from being mistaken for the final evidence batch, I'll point them at a separate `results/synthetic/precheck/` root so the evidence tree stays clean. Say so if you'd prefer a different location.
~~~~

## 消息 1717 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "reasoning_recap",
  "content": "思考了 13s"
}
~~~~

## 消息 1718 / assistant / 分段 1 of 1

~~~~text
这个位置我赞成，`results/synthetic/precheck/` 很合适，不用改。

这样证据树会很清楚：

```text
results/synthetic/
├── precheck/          # 只放 S05–S08 headless 预检查
├── S01_...
├── S02_...
├── S03_...
├── S04_...
└── final/ 或后续正式 S05–S08 evidence run
```

我再加两个要求，避免以后混淆：

1. 每个 precheck 的 `result.json / result.md / summary` 里必须明确写：
   `RUN_CLASS = PRECHECK`
   `NOT_FINAL_EVIDENCE = TRUE`
   `NOT_FOR_DOCUMENT_CLAIM = TRUE`

2. 最终正式证据运行不要复用 `precheck/`，也不要覆盖 precheck。正式批次单独进 `final/` 或明确的正式 run 目录。

你这次对状态的报告方式也对了：哪些是 `TARGET_WORKTREE_VERIFIED`，哪些还是 `LOCAL_STAGING_ONLY`，哪些只是 subagent 写入但主 Claude 还没独立验证，区分得很清楚。

现在继续按这个顺序走即可：

`result_writer PASS/FAIL`
→ `S05–S08 YAML`
→ `setup.py`
→ `FINAL_SUBAGENT_MERGE_REVIEW`
→ `TARGET_WORKTREE_VERIFICATION`
→ build/tests
→ `precheck/` 下跑四个 headless precheck
→ 更新 docs 第二遍
→ 停在 `VISIBLE_EVIDENCE_CHECKPOINT`

特别注意一点：`monitor_node.py / plot_results.py / .rviz / marker / evidence helpers` 目前虽然已经写到目标机，但还不能叫 VERIFIED，必须主 Claude 自己 read-back + diff + syntax/entry-point 检查后再升级状态。

所以当前结论：

**APPROVED TO CONTINUE**
**PRECHECK_ROOT = `results/synthetic/precheck/`**
**NO CHANGE NEEDED**
~~~~

## 消息 1719 / user / 分段 1 of 1

~~~~text
你给我提示词就行
~~~~

## 消息 1720 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "model_editable_context",
  "model_set_context": ""
}
~~~~

## 消息 1721 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "reasoning_recap",
  "content": "思考了 8s"
}
~~~~

[本对话目录](README.md) | [总目录](../../README.md) | [Raw](https://raw.githubusercontent.com/weiyang02520-ops/dg202611-conversation-archive/main/conversations/first/part-043.md) | [上一卷](part-042.md) | [下一卷](part-044.md)
