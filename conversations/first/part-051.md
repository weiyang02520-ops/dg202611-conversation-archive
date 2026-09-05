# 机器人定位挑战赛分析 / 第 051 卷

[本对话目录](README.md) | [总目录](../../README.md) | [Raw](https://raw.githubusercontent.com/weiyang02520-ops/dg202611-conversation-archive/main/conversations/first/part-051.md) | [上一卷](part-050.md) | [下一卷](part-052.md)

## 消息 1987 / assistant / 分段 1 of 2

~~~~text
执行位置：当前 Claude Code 开发线程，请在本线程直接继续执行，不得转发。

DG-202611 ROUND B INTEGRITY FIX + ACCELERATED PRECHECK GATE

你最新报告已审核。

结论：

CONTINUE = YES

但当前仍然：

PRECHECK_NOT_ALLOWED = YES

在以下问题全部完成 TARGET_WORKTREE_VERIFIED 之前，不得运行 S05—S08 precheck。

本轮重点不是增加设计，而是修正证据链、消除 staging/target 偏差，然后尽快进入 precheck。

==================================================
一、InputState 问题定性
==================================================

这是本 Round B 第二次出现：

LOCAL_STAGING_UPDATED
≠
TARGET_WORKTREE_UPDATED

但此前被错误理解为目标机已具备该能力。

当前事实冻结为：

InputState on target = MISSING

`grep 'InputState\|_inputs' evaluator_node.py`
无结果。

15 个 INPUT columns 当前实际不会被 `_sample` 填充。

因此当前如果运行 S06：

`s06_plant_yaw_responded_to_command`

可能因：

odom_yaw input evidence empty

而产生：

FALSE_FAIL

这不是 plant failure。

这是：

EVIDENCE_PIPELINE_BUG

所以：

DO_NOT_RUN_PRECHECK

直到 InputState 真正进入目标 VM。

==================================================
二、立即修复 InputState，并以目标机为事实来源
==================================================

将已经完成的 InputState 实现写入目标 VM。

然后必须实际执行：

target read-back

grep InputState

grep `_inputs`

grep 15 INPUT columns

py_compile

并通过一个最小静态/运行级 sanity check 确认：

`_sample` 实际能够给 15 个 INPUT columns 提供值。

至少确认：

odom_x
odom_y
odom_yaw
odom_linear
odom_angular

amcl_x
amcl_y
amcl_yaw
amcl_covariance

plant state/mode

surrogate state/mode

TF availability

scheduled covariance / scenario input fields

按现有 FIELD_ROLES 的真实列名核对。

不得仅看到 InputState class 存在就宣布完成。

必须确认：

INPUTSTATE_DEFINED = YES
INPUTSTATE_CONNECTED_TO_SAMPLE = YES
INPUT_COLUMNS_POPULATED_BY_INPUTSTATE = YES

完成后才允许：

TARGET_WORKTREE_VERIFIED

==================================================
三、建立永久 Anti-Staging Verification Gate
==================================================

这已经是第二次同类问题。

从现在开始，增加一个永久开发规则：

NO_TARGET_VERIFICATION_FROM_LOCAL_STATE

任何文件从：

LOCAL_STAGING_ONLY

升级为：

TARGET_WORKTREE_VERIFIED

必须同时具备：

1. target file read-back
2. target grep / structural check
3. py_compile 或对应 syntax check
4. 若功能涉及 runtime data path，则最小 runtime/sanity check

Subagent summary、local staging diff、transfer command success 都不能作为 VERIFIED 证据。

在开发记录中记录：

REPEATED_FAILURE_CLASS:
STAGING_TARGET_DIVERGENCE

OCCURRENCE_COUNT:
2

ROOT_CAUSE:
implementation completed locally after an earlier target sync, but target was not re-written before status reporting.

PREVENTION:
mandatory target read-back gate before VERIFIED status.

==================================================
四、Empty Plot Defect 必须修
==================================================

当前问题：

plot `save()` 无论有没有真正绘制 series 都：

savefig
+
append path to files

因此可能生成：

EMPTY_PLOT

但 artifact manifest 把它当：

VALID_GENERATED_PLOT

这是证据完整性 bug。

原则：

FILE_EXISTS != VALID_EVIDENCE

和之前 black screenshot trap 属于同一类别。

修复要求：

只有至少存在一个有效 plotted data series 时：

才允许 savefig
才允许登记到 generated files / evidence manifest。

如果没有有效 series：

不得创建“看似正常”的 evidence plot。

建议返回明确状态：

PLOT_GENERATED = YES/NO

SKIP_REASON = NO_VALID_SERIES

如已有 API 不适合增加返回值，可采用等价但明确的实现。

必须增加测试，至少覆盖：

KNOWN_VALID_SERIES
→ file generated + listed

NO_SERIES
→ no evidence file listed

ALL_REQUIRED_COLUMNS_EMPTY
→ no evidence file listed

不得单纯判断 PNG 是否存在。

==================================================
五、CJK Forbidden-Claim Scanner Bug 必须修
==================================================

`\b` 对中文词组边界不可靠。

已证明漏掉至少：

定位精度20厘米

使用Gazebo仿真采集

等 CJK forbidden claims。

因此：

FORBIDDEN_CLAIM_SCANNER_CURRENT = UNSAFE

修复原则：

不要再依赖：

`\b...`

包裹中文短语。

根据当前 scanner 架构选择：

明确中文 phrase matching

或

适用于 Unicode/CJK 的边界逻辑。

目标：

中文连续文本也必须被识别。

必须建立 regression cases。

至少包含已知漏检的 7 个短语。

必须有：

POSITIVE_CASES
→ 全部命中

以及合理的：

NEGATIVE_CASES
→ 不误报

特别检查：

20厘米
20 cm
95%
Gazebo仿真
实车验证
定位精度
成功率

等正式文档高风险词。

不要扩大成会把正常免责声明也全部误杀的规则。

==================================================
六、RViz 三个 Dead Display 必须修
==================================================

已确认：

/dg/fusion/pose

和：

/amcl_pose

消息类型为：

PoseWithCovarianceStamped

但 RViz 配成：

rviz_default_plugins/Pose

这是类型不匹配。

不得保留“配置存在但实际看不到”的 display。

修复成与真实 ROS message type 匹配的 RViz display。

同时检查第三个被 agent 标记的 dead display。

必须逐个建立：

DISPLAY_NAME

TOPIC

ACTUAL_MESSAGE_TYPE

RVIZ_DISPLAY_TYPE

COMPATIBLE:
YES/NO

最后全部必须：

YES

不要靠“RViz 文件能加载”判断成功。

==================================================
七、Manual Capture Checklist 必须修
==================================================

当前 checklist 要求用户从 RViz screenshot 描述 relocalization state，

但 marker node 尚未安装/注册。

这是：

EVIDENCE_INSTRUCTION_MISMATCH

修复原则：

用户只能被要求截图画面里真实存在的信息。

两种合法方式：

A.
marker node 完成注册、安装、运行，并真实显示 state text；

或

B.
checklist 不再要求从 RViz 中读取不存在的 relocalization state，改为从 Monitor 读取。

优先方案：

Monitor = 状态文本主要来源
RViz = 空间/轨迹/scan/candidate 可视化来源

不要为了截图塞大量无必要 marker。

Marker 只有在已经实现且不会伪造 SUT 状态时保留。

==================================================
八、scenario_runner.py 是 Precheck 硬门
==================================================

在 scenario_runner 完成前：

NO PRECHECK

必须实现：

PRECHECK_ROOT =
`/home/weiyang/dg202611_ws/results/synthetic/precheck/`

每个 precheck artifact 内部必须显式记录：

RUN_CLASS = PRECHECK

NOT_FINAL_EVIDENCE = TRUE

NOT_FOR_DOCUMENT_CLAIM = TRUE

EVIDENCE_LEVEL = SYNTHETIC SOFTWARE VALIDATION PRECHECK

不能只依靠：

目录名称

来区分 precheck。

必须验证这些字段真实进入：

result.json
result.md
summary / metadata

按现有 artifact schema 实际支持情况写入。

==================================================
九、setup.py 是第二个 Precheck 硬门
==================================================

完成所有新增 entry points 注册。

然后必须：

build

source workspace

执行：

ros2 pkg executables dg_synthetic_validation

实际确认 executable。

至少核对当前需要使用的：

scenario runner
monitor
marker（如果保留）
以及其它本 Round B 新增 entry points。

SETUP_TEXT_LOOKS_CORRECT

不等于：

ENTRY_POINT_VERIFIED。

==================================================
十、ylhb_base Regression 必须重新实际运行
==================================================

此前 bare：

python3

因为：

rclpy unavailable

而失败。

因此当前不能引用：

253 passed

作为本轮已经复现的结果。

重新运行前必须正确 source：

ROS2 Humble

+
当前 workspace install（如测试依赖）

例如按目标环境实际情况：

source /opt/ros/humble/setup.bash
source /home/weiyang/dg202611_ws/install/setup.bash

然后执行既定 relevant ylhb_base functional regression。

必须报告实际：

PASSED
FAILED
SKIPPED
ERROR

如果无法复现 253：

如实写当前结果。

不得继承历史数字当本轮测试事实。

==================================================
十一、多 Subagent 并行加速
==================================================

老师要求尽快进入真正仿真。

因此这一批完整性修复允许并鼓励继续多 Subagent 并行。

建议立即分配：

SUBAGENT A — INPUT PIPELINE

负责只读审核：

InputState
evaluator `_sample`
15 INPUT columns
FIELD_ROLES compatibility

主 Claude负责最终 target sync。

--------------------------------------------------
SUBAGENT B — PLOT INTEGRITY

文件所有权：

plot_results.py
相关 plotting tests

只解决：

empty plot false evidence

不得碰 evaluator/result_writer。

--------------------------------------------------
SUBAGENT C — CLAIM SCANNER

文件所有权：

forbidden-claim scanner
对应 tests

解决：

CJK `\b` bug

运行 positive/negative regression。

--------------------------------------------------
SUBAGENT D — RVIZ + MANUAL EVIDENCE

文件所有权：

RViz config
marker node（如必要）
MANUAL_CAPTURE_CHECKLIST.md

解决：

message/display mismatch
checklist visual mismatch。

--------------------------------------------------
SUBAGENT E — RUNNER + ENTRY POINTS

文件所有权：

scenario_runner.py
setup.py

完成：

precheck classification
entry points。

--------------------------------------------------
SUBAGENT F — DOCS

记录：

第二次 staging-target divergence
false S06 failure risk
empty plot defect
CJK scanner bug
dead RViz displays
manual checklist mismatch
bare-python ROS regression mistake

不得写：

PRECHECK PASS

因为尚未运行。

==================================================
十二、并行修改后必须主 Claude统一复核
==================================================

所有 Subagent 完成后：

MAIN CLAUDE 必须执行：

FINAL_SUBAGENT_MERGE_REVIEW

检查：

git status
git diff --stat
git diff

并亲自 read-back：

scenario_schema.py
synthetic_injector_node.py
evaluator_node.py
result_writer.py
scenario_runner.py
setup.py
monitor_node.py
plot_results.py
marker node
RViz
evidence helpers
claim scanner
manual checklist
S05–S08 YAML
development docs

不得根据 Subagent report 自动 PASS。

==================================================
十三、目标树最后必须达到的状态
==================================================

进入 precheck 前必须达到：

LOCAL_STAGING_ONLY_CRITICAL_FILES = NONE

所有 precheck 必需代码必须：

WRITTEN_TO_TARGET_WORKTREE
+
TARGET_WORKTREE_VERIFIED

特别确认：

INPUTSTATE_PRESENT = YES

INPUTSTATE_CONNECTED = YES

15_INPUT_COLUMNS_POPULATABLE = YES

RESULT_WRITER_ON_TARGET = YES

S05_YAML_ON_TARGET = YES

S06_YAML_ON_TARGET = YES

S07_YAML_ON_TARGET = YES

S08_YAML_ON_TARGET = YES

SCENARIO_RUNNER_PRECHECK_CLASSIFICATION = YES

SETUP_ENTRY_POINTS = VERIFIED

EMPTY_PLOT_FALSE_EVIDENCE_FIXED = YES

CJK_FORBIDDEN_SCANNER_FIXED = YES

RVIZ_DISPLAY_TYPES_VALID = YES

MANUAL_CHECKLIST_MATCHES_VISIBLE_UI = YES

==================================================
十四、既有冻结约束继续保持
==================================================

ylhb_base：

UNMODIFIED

继续保持。

禁止：

core state machine changes

core threshold changes

perfect map→odom

synthetic /cmd_vel_nav

synthetic /dg/* outputs

direct candidate injection

direct RECOVERED injection

scripted yaw shortcut。

==================================================
十五、TARGET_WORKTREE_VERIFICATION
==================================================

修复完成后统一执行。

至少返回：

branch
HEAD
parent
git status
git diff --stat

以及：

INPUTSTATE_PRESENT
INPUTSTATE_CONNECTED
INPUT_COLUMN_COUNT
FIELD_ROLE_COUNTS

PERFECT_MAP_ODOM_PRESENT

SURROGATE_SEPARATED

NO_SYNTHETIC_DG_OUTPUT

NO_SYNTHETIC_CMD_VEL_NAV

EMPTY_PLOT_GUARD

CJK_SCANNER_REGRESSION

RVIZ_TYPE_AUDIT

MANUAL_CHECKLIST_AUDIT

SETUP_ENTRY_POINTS

PY_COMPILE_ALL

==================================================
十六、Build/Test Gate
==================================================

然后执行：

py_compile

targeted colcon build

synthetic package tests

正确 ROS source 后的 ylhb_base relevant regression

entry-point verification

必要 runtime smoke tests。

只有这些通过到足够运行 precheck 的程度，才进入下一步。

==================================================
十七、Precheck 必须仍然串行
================
~~~~

## 消息 1987 / assistant / 分段 2 of 2

~~~~text
==================================

S05
→
S06
→
S07
→
S08

禁止并行。

Precheck 可以 FAIL。

不得为了绿色：

改核心 timeout
改 threshold
改 state machine
改 verification count
直接 fake 状态。

失败 run 保留。

==================================================
十八、S06 特别增加 Evidence Completeness Gate
==================================================

在执行 S06 assertion 前确认：

odom_yaw INPUT series：

NON_EMPTY = YES

NUMERIC = YES

HAS_MULTIPLE_SAMPLES = YES

否则：

不得把：

s06_plant_yaw_responded_to_command = FAIL

解释为 plant 不响应。

应该归类为：

INPUT_EVIDENCE_INCOMPLETE

如果这是 test-rig bug：

修复 test rig 后重新 precheck。

==================================================
十九、Plot Artifact Integrity Gate
==================================================

每张正式登记的 plot 必须满足：

SOURCE_DATA_PRESENT = YES

PLOTTED_SERIES_COUNT >= 1

FILE_CREATED = YES

只有同时满足：

才允许加入：

generated_files
evidence manifest
DOCUMENT_EVIDENCE_INDEX

空图不能进入证据库。

==================================================
二十、当前仍禁止 final evidence
==================================================

NO_FINAL_EVIDENCE_SCENARIO_EXECUTED 必须继续：

YES

完成所有 precheck 后：

停在：

VISIBLE_EVIDENCE_CHECKPOINT

等待：

VISIBLE_READY

==================================================
二十一、Checkpoint 额外增加本轮 Integrity 项
==================================================

到 checkpoint 时除原格式外增加：

STAGING_TARGET_DIVERGENCE_OCCURRENCES:
2

INPUTSTATE_TARGET_STATUS:

INPUTSTATE_SAMPLE_CONNECTION:
PASS/FAIL

INPUT_COLUMNS:
expected:
actual_nonempty_capability:

FALSE_S06_FAIL_RISK:
RESOLVED/UNRESOLVED

EMPTY_PLOT_DEFECT:
FIXED/OPEN

EMPTY_PLOT_TESTS:

CJK_CLAIM_SCANNER_DEFECT:
FIXED/OPEN

CJK_POSITIVE_CASES:
passed/total

CJK_NEGATIVE_CASES:
passed/total

RVIZ_DEAD_DISPLAYS:
RESOLVED/OPEN

RVIZ_TYPE_AUDIT:

MANUAL_CAPTURE_CHECKLIST:
PASS/FAIL

ROS_ENV_SOURCED_FOR_YLHB_REGRESSION:
YES/NO

YLHB_BASE_REGRESSION:
actual result

PREVIOUS_253_PASS_REPRODUCED:
YES/NO

如果 NO：
说明真实结果，不得伪造。

==================================================
二十二、执行优先级
==================================================

P0:
InputState target sync + connection

P0:
scenario_runner precheck classification

P0:
setup entry points

P1:
empty plot integrity

P1:
CJK scanner

P1:
RViz/checklist

P1:
merge review + target verification

P1:
build/tests

P1:
S05–S08 precheck

然后：

VISIBLE_EVIDENCE_CHECKPOINT

不要再扩大 Round B scope。

尽快把 Package A 收尾。

Package A 一冻结，马上进入下一阶段 Gazebo 物理仿真。

现在直接执行，多 Subagent 并行处理独立工作，不再等待确认。

——DG-202611 ROUND B INTEGRITY FIX + ACCELERATED PRECHECK GATE END——
~~~~

## 消息 1988 / user / 分段 1 of 1

~~~~text
刚刚zcode送了3亿glm5.3flash的额度，我想着拿来干点啥呗
~~~~

## 消息 1989 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "reasoning_recap",
  "content": "思考了 8s"
}
~~~~

## 消息 1990 / assistant / 分段 1 of 1

~~~~text
能干，而且 **3 亿 token 最适合拿来当“廉价劳动力池”**，别拿 GLM-5.3-Flash 去承担最终架构决策，拿它做大量重复、并行、可验证的工作最划算。

我最推荐你现在这么用：

1. **给 DG 比赛项目开一群 GLM 子 Agent**
   - 批量读论文、提取性能指标、实验条件、硬件型号。
   - UWB / 毫米波雷达 / 事件相机分别开 Agent 做技术调研。
   - 扫整个 GitHub 仓库，做“文档声称 ↔ 代码事实 ↔ 硬件事实”对账。
   - 每次 Claude 改完代码，让 GLM 做独立 code review、测试路径检查、证据完整性检查。
   - Gazebo 后面每个场景让 GLM 帮忙检查 bag、CSV、日志、结果表有没有自相矛盾。

2. **专门做“大规模文献矿工”**
   
   这个特别适合你现在老师要求的：
   
   > 项目指标不能比论文吹得还高。
   
   可以让 GLM 一篇篇输出统一结构：
   `论文 → 场景 → 传感器 → Ground Truth → 指标定义 → 数值 → 限制 → 我们最多安全写到什么程度`
   
   然后真正重要的 20～30 篇再交给我/Claude 二次审核。
   
   这种工作很吃 token，但不太需要顶级推理，**正适合 Flash**。

3. **拿来当测试生成器**
   
   每增加一个模块就让 GLM：
   - 找边界条件；
   - 生成单元测试；
   - 生成异常输入；
   - 检查测试是否真的覆盖需求；
   - 查“假 PASS”。
   
   像刚刚 Claude 连续出现两次 `local staging != target VM`，就很适合长期放一个便宜 Agent 专门盯：
   
   `代码说完成了 → 目标机到底有没有 → runtime 到底有没有 → 证据到底是不是真的`

4. **Tech-Knowledge-Base 直接升级成自动知识工厂**
   
   你之前那个技术知识库其实特别适合消耗这批额度：
   
   ```text
   GitHub/论文/文档
          ↓
   GLM Flash 批量阅读
          ↓
   摘要 / API / 陷阱 / 适用场景 / 示例
          ↓
   去重、分类、打标签
          ↓
   强模型最后审核
          ↓
   Tech-Knowledge-Base
   ```
   
   以后 Claude/Codex 需要某技术的时候再检索相关片段，而不是每轮全部塞上下文。

5. **做 Agent 自动开发流水线的“工人模型”**
   
   你一直想要的：
   
   `产品目标 → 自动拆任务 → 开发 → 测试 → 审核 → 修复 → 再审核`
   
   这里其实没必要每个角色都用贵模型。
   
   可以变成：
   
   ```text
   我 / Claude
      ↓
   Master / Planner
      ↓
   ┌───────────────┐
   GLM Flash × N
   ├─ repo audit
   ├─ test generation
   ├─ docs audit
   ├─ log analysis
   ├─ literature extraction
   ├─ regression review
   └─ consistency checking
   └───────────────┘
      ↓
   Claude/Codex 做最终实现
      ↓
   强模型 Final Gate
   ```
   
   **这个可能才是 3 亿 token 最值钱的用法。**

而且 3 亿真的很多。

哪怕平均一个子任务吃 **5 万 token**：

**300,000,000 ÷ 50,000 = 6000 个大型 Agent 任务。**

平均 2 万 token，就是 **15000 次任务**。

所以完全没必要省着聊天用。

### 我现在最建议

直接给 GLM-5.3-Flash 定位成：

> **Mass Worker / Researcher / Auditor**

Claude Code 继续负责真正代码修改，我负责架构和 Gate，GLM 专门承担海量阅读、检查、测试、资料整理。

尤其你现在 DG 项目正处于 **Round B → Gazebo → UWB/毫米波/事件相机 → 文献指标验证** 这个阶段，正好能把这 3 亿额度狠狠干掉，而且是真产生项目资产，不是刷 token 玩。

如果你愿意，我下一步可以直接给你设计一套 **“3 亿 GLM Token 榨干计划”**，包括到底开哪些 Agent、每个 Agent 的固定 Prompt、怎么避免它们重复劳动、哪些任务绝对不能交给 Flash。memcite
~~~~

## 消息 1991 / user / 分段 1 of 1

~~~~text
只能在zcode里面用
~~~~

## 消息 1992 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "reasoning_recap",
  "content": "思考了 8s"
}
~~~~

[本对话目录](README.md) | [总目录](../../README.md) | [Raw](https://raw.githubusercontent.com/weiyang02520-ops/dg202611-conversation-archive/main/conversations/first/part-051.md) | [上一卷](part-050.md) | [下一卷](part-052.md)
