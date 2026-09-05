# 恢复项目上下文 / 第 010 卷

[本对话目录](README.md) | [总目录](../../README.md) | [Raw](https://raw.githubusercontent.com/weiyang02520-ops/dg202611-conversation-archive/main/conversations/second/part-010.md) | [上一卷](part-009.md) | [下一卷](part-011.md)

## 消息 296 / assistant / 分段 1 of 1

~~~~text
对，就按你这个顺序来。

**现在先不碰任何实机。**  
三模型 + UWB 软件已经完成，但整个项目里 **Claude 能纯软件完成的东西还没全部收干净**。

接下来先让 Claude 做一次“实机前总收口”：

- 检查比赛要求还有哪些纯软件项没做
- launch/config/节点连接全部补齐
- 三模型 + UWB 整体启动链打通
- 自动化测试/故障注入补齐
- 数据记录、CSV、日志、实验脚本准备好
- 北斗短报文的软件接口/适配层能提前做的先做
- 比赛指标对应的测试工具提前准备
- 一键启动/一键测试/一键采集准备好
- 文档和证据结构整理好
- 所有**必须等真机**的东西统一留到最后

然后最后一次性进入：

**UWB + GNSS/北斗 + 雷达 + IMU + 底盘 + 真车 → 实机总联调。**

你现在直接给 Claude：

继续 DG-202611。

执行任务：

DG202611_TASK_163_PRE_HARDWARE_SOFTWARE_TOTAL_CLOSURE

目标：

在进入真实机器人、真实 UWB、真实 GNSS/北斗、真实 LiDAR、真实底盘测试之前，把所有“无需真实硬件即可由软件完成”的工作一次性全部完成。

不要开始真实硬件测试。

当前冻结状态：

- 三模型闭环：PASS
- MODEL-1：PASS
- MODEL-2：PASS
- MODEL-3：PASS
- 三模型 recovery E2E：PASS
- UWB software：PASS
- UWB → MODEL-1：PASS
- UWB_01~05：PASS
- UWB tests：8/8 PASS
- regression：67/67 PASS
- UWB software freeze：YES

当前基线：

branch:
dg202611-uwb-model1-integration

HEAD:
babf4b5a73aec96098bb7db03e77bd3f4aa32053

本任务原则：

不要重新设计已经冻结的 MODEL-1/2/3/UWB。

本任务是：

AUDIT
→ FIND REMAINING SOFTWARE WORK
→ IMPLEMENT
→ INTEGRATE
→ TEST
→ FIX
→ PREPARE HARDWARE HARNESS
→ FINAL PRE-HARDWARE FREEZE

一次执行到底。

普通 bug 自己解决，不要频繁 RETURN_TO_MASTER。

第一阶段：完整审计项目

检查整个：

electric-power-inspection-robot

以及项目已有 DG-202611 requirements / docs / configs / launch / tests。

建立比赛要求 → 当前实现的映射。

把要求分成：

A. SOFTWARE_COMPLETE
B. SOFTWARE_WORK_REMAINING
C. REAL_HARDWARE_REQUIRED
D. REAL_EXPERIMENT_REQUIRED
E. SUBMISSION_ONLY

不要因为某项需要真机，就停止整个任务。

先完成所有 A/B 中能完成的内容。

第二阶段：检查完整系统启动链

检查现有 ROS2 package：

ylhb_base
ylhb_interfaces
ylhb_perception
ylhb_3d_mapping
以及项目实际使用的其他 package。

确认当前三模型和 UWB 软件不是“孤立 Python 文件”。

需要确认或补齐：

- CMake/package install
- executable install
- launch integration
- config loading
- topic wiring
- parameter wiring
- diagnostics wiring

必须能从正式 workspace 编译/启动。

如果现在缺少统一 DG-202611 launch：

创建一个合理的 pre-hardware launch。

例如：

dg202611_navigation.launch.py

但以现有仓库结构为准。

不要为了整洁大重构。

第三阶段：MODEL-1/2/3/UWB 总连接审计

验证软件数据流：

GNSS
LiDAR
AMCL
odom
UWB

→ MODEL-1

MODEL-1 health degradation
→ MODEL-2

MODEL-2 robust decision
→ MODEL-3

MODEL-3
→ recovery
→ candidate verification
→ safe handoff
→ MODEL-1 progressive reentry

UWB：

GOOD
→ MODEL1 accepts

DEGRADED
→ deweight

REJECTED
→ UWB excluded only

单 UWB 故障：
MODEL3_TRIGGERED = NO

不要改变已经冻结的规则。

第四阶段：完整 synthetic/system integration test

不要只测单文件。

建立/完善系统级软件测试：

SYS_01_NOMINAL
所有软件输入正常
→ stable

SYS_02_GNSS_DEGRADE
→ MODEL1 source management

SYS_03_LIDAR_DEGRADE
→ health degradation

SYS_04_LOCALIZATION_FAILURE_RECOVERY
→ MODEL1→MODEL2→MODEL3→RECOVERED

SYS_05_UWB_STABLE
→ MODEL1 consumes

SYS_06_UWB_FAILURE_OTHER_SOURCES_HEALTHY
→ no MODEL3

SYS_07_MULTI_SOURCE_DEGRADATION
→ existing recovery path

SYS_08_RECOVERY_REENTRY
→ safe handoff + progressive reentry

不得使用 GT 参与 online decision。

GT 只允许用于离线 validation。

第五阶段：真实实验采集工具提前准备

虽然现在没有真机，但提前把工具做好。

需要准备：

1. UWB raw logger
2. GNSS logger
3. LiDAR/model diagnostics logger
4. MODEL1 state logger
5. MODEL2 decision logger
6. MODEL3 state/recovery logger
7. unified timestamped experiment logger

输出优先：

CSV
JSONL
ROS bag（如果现有环境合适）

不要要求真实数据才能写工具。

真实数据字段当前未知的地方做 adapter/interface，不得伪造协议。

第六阶段：比赛指标评估工具

根据当前已确认的比赛目标，提前写离线 evaluator。

至少准备：

定位误差 evaluator

输入：
estimated x/y
ground-truth x/y

输出：
mean
median
RMSE
P95
max
horizontal error distribution

特征重复率 evaluator

用于后续计算相邻帧有效特征 repeatability。

重定位 evaluator

统计：

attempts
successes
success_rate
time_to_recovery
failure_reason

这些 evaluator 可以用 synthetic fixture 验证算法本身。

不得声称比赛指标已经真实达标。

第七阶段：北斗相关软件审计

检查当前 WTRTK980 / GNSS 代码：

wtrtk980_nmea.cpp
wtrtk980_nmea_node.cpp
相关 config / launch / interfaces。

确认：

GNSS/RTK 数据链是否已经可以进入 MODEL-1。

如果可以：
保留并验证。

对于“北斗短报文”：

先检查本地项目材料和已有代码是否存在真实模块协议。

如果已有真实协议：
实现软件 adapter/interface。

如果没有真实设备协议：
只建立接口边界、消息类型、adapter skeleton、mock test。

明确：

BEIDOU_SHORT_MESSAGE_REAL_VALIDATION = NOT_DONE

不得把 NTRIP/4G/LoRa 当成北斗短报文。

第八阶段：配置整理

把比赛运行真正需要的参数统一检查。

包括：

MODEL1
MODEL2
MODEL3
UWB
GNSS
relocalization
health
logging

配置必须：

- 无开发机绝对路径
- 无 staging path
- 无临时 hardcode
- 无 fake PASS
- 无真实硬件假参数冒充实测值

真实硬件未知值可以：

TODO / placeholder / disabled

但必须清楚标注。

第九阶段：一键测试

建立最简单可靠的验证入口。

例如：

scripts/run_dg202611_preflight.sh

或项目现有机制中的等价方式。

能够自动：

build
→ run software unit tests
→ run three-model tests
→ run UWB tests
→ run system integration tests
→ run evaluator self-tests

最后明确：

PASS / FAIL

不要通过 grep 文本 PASS 作为证据。

必须看真实 process exit code / test result。

第十阶段：一键实验准备

提前准备真实机器人阶段会用的：

start
stop
capture
experiment naming
output directory
metadata

例如实验开始时创建：

experiments/
  YYYYMMDD_HHMMSS_<scenario>/

内部未来放：

raw
csv
bag
logs
metadata

不要填假数据。

第十一阶段：回归

必须重新运行：

三模型已有 regression
UWB regression
新增 system tests
evaluator tests

要求 0 regression。

如果测试数增加，报告真实数量。

不要强行维持 67。

第十二阶段：Git

从当前冻结基线继续。

建议创建：

dg202611-prehardware-closure

不得：

reset --hard
force push
破坏 frozen MODEL-1/2/3/UWB

完成后正常：

commit
push
verify local/remote HEAD

第十三阶段：最终划分

最后必须生成清晰状态：

SOFTWARE_COMPLETE

和

HARDWARE_PHASE_REMAINING

HARDWARE_PHASE_REMAINING 应统一包括例如：

- 真实 UWB
- 真实 anchor 坐标
- UWB real range
- UWB real 2D
- GNSS/RTK real data
- 北斗短报文真实设备
- LiDAR 真机
- IMU 真机
- 底盘
- 整车
- 实际定位精度
- 实际特征重复率
- 实际重定位成功率
- 实验截图/CSV/bag/视频

不要在本任务测试这些。

最终目标：

本任务完成以后，我希望达到：

“除了必须接真实设备、真实机器人、真实场地才能完成的事情以外，Claude 能做的软件工作基本全部完成。”

最终只返回：

TASK:
DG202611_TASK_163_PRE_HARDWARE_SOFTWARE_TOTAL_CLOSURE

FULL_PROJECT_SOFTWARE_AUDIT:
PASS/FAIL

ROS2_BUILD:
PASS/FAIL

SYSTEM_LAUNCH_READY:
YES/NO

MODEL1_MODEL2_MODEL3_INTEGRATION:
PASS/FAIL

UWB_MODEL1_INTEGRATION:
PASS/FAIL

SYS_01_NOMINAL:
PASS/FAIL

SYS_02_GNSS_DEGRADE:
PASS/FAIL

SYS_03_LIDAR_DEGRADE:
PASS/FAIL

SYS_04_LOCALIZATION_FAILURE_RECOVERY:
PASS/FAIL

SYS_05_UWB_STABLE:
PASS/FAIL

SYS_06_UWB_FAILURE_OTHER_SOURCES_HEALTHY:
PASS/FAIL

SYS_07_MULTI_SOURCE_DEGRADATION:
PASS/FAIL

SYS_08_RECOVERY_REENTRY:
PASS/FAIL

EXPERIMENT_LOGGING_TOOLS:
READY/NOT_READY

POSITION_ERROR_EVALUATOR:
READY/NOT_READY

FEATURE_REPEATABILITY_EVALUATOR:
READY/NOT_READY

RELOCALIZATION_EVALUATOR:
READY/NOT_READY

GNSS_RTK_SOFTWARE_PATH:
READY/NOT_READY

BEIDOU_SHORT_MESSAGE_SOFTWARE_INTERFACE:
READY/NOT_READY/NO_PROTOCOL_AVAILABLE

PRE_HARDWARE_PREFLIGHT:
PASS/FAIL

TOTAL_TESTS:
<real result>

REGRESSIONS:
<value>

SOFTWARE_ONLY_WORK_REMAINING:
<list>

HARDWARE_PHASE_REMAINING:
<list>

FINAL_LOCAL_HEAD:
<sha>

FINAL_REMOTE_HEAD:
<sha>

LOCAL_REMOTE_MATCH:
YES/NO

PRE_HARDWARE_SOFTWARE_FREEZE_READY:
YES/NO

BLOCKER:
NONE/<specific>

NEXT_ACTION:
RETURN_TO_MASTER

这轮搞完，**我们才统一切到实机阶段**，到时候 UWB、雷达、GNSS、底盘这些一起处理。
~~~~

## 消息 297 / user / 分段 1 of 1

~~~~text


## TASK163 状态报告

**STATUS: EXECUTION\_ENVIRONMENT\_COMPLETELY\_BLOCKED**
**这是连续第二个任务因分类器阻塞而无法执行。**

---

### ✅ 已完成（极小部分）

**Phase 1: 项目审计 - PARTIAL**

- ✅ 基线已验证：5c35468687cf0816058b8e26d36c51aeca7df65e
- ✅ 新分支已创建：dg202611-prehardware-closure
- ✅ 项目结构已识别：
  - ROS2 packages: ylhb\_base, ylhb\_interfaces, ylhb\_perception, ylhb\_3d\_mapping, ylhb\_llm, ylhb\_mobile\_bridge
  - 关键 launch: dg\_navigation\_integration.launch.py
  - 关键 config: base\_kinematics.yaml, nav2\_params.yaml, ekf.yaml

---

### ❌ 完全阻塞

分类器拒绝所有 Bash/Python 执行（stage 2 classifier error）。
**无法执行**：

- ❌ 检查三模型/UWB 文件集成
- ❌ 审计 launch 连接
- ❌ 验证 ROS2 节点
- ❌ 实现系统测试
- ❌ 创建日志工具
- ❌ 创建 evaluator
- ❌ 检查 GNSS/北斗路径
- ❌ 运行任何测试
- ❌ 构建/验证
- ❌ Git commit

---

### 📊 任务结果

```
FULL_PROJECT_SOFTWARE_AUDIT: INCOMPLETE
ROS2_BUILD: NOT_ATTEMPTED
SYSTEM_LAUNCH_READY: UNKNOWN
MODEL1_MODEL2_MODEL3_INTEGRATION: NOT_VERIFIED
UWB_MODEL1_INTEGRATION: NOT_VERIFIED
SYS_01-08: NOT_RUN
EXPERIMENT_LOGGING_TOOLS: NOT_IMPLEMENTED
EVALUATORS: NOT_IMPLEMENTED
PRE_HARDWARE_PREFLIGHT: NOT_RUN

PRE_HARDWARE_SOFTWARE_FREEZE_READY: NO

```

---

### 🚫 阻塞原因

**EXECUTION\_ENVIRONMENT\_COMPLETELY\_BLOCKED**
与 TASK160 相同问题：分类器故障，无法执行任何命令。
这是第二个连续任务被相同环境问题阻塞。

---

**NEXT\_ACTION: RETURN\_TO\_MASTER**
**建议：切换到不同的执行环境，使用正常工作的分类器。**
~~~~

## 消息 298 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "reasoning_recap",
  "content": "思考了 5s"
}
~~~~

## 消息 299 / assistant / 分段 1 of 1

~~~~text
继续 DG-202611。

不要重做 TASK163 已完成的审计起点，不要重新规划项目。

执行任务：

DG202611_TASK_163B_PRE_HARDWARE_SOFTWARE_TOTAL_CLOSURE_CONTINUATION

上一执行环境已确认完全失效：

EXECUTION_ENVIRONMENT_COMPLETELY_BLOCKED

表现：

- Bash 无法执行
- Python 无法执行
- stage 2 classifier error
- 与 TASK160 出现过的执行环境故障相同

因此：

不要尝试修复上一 Claude 会话。
不要等待 classifier 恢复。
直接在当前正常可执行的新环境中继续。

当前已知：

TASK159 三模型闭环：
PASS

TASK160 UWB 软件：
PASS

UWB software freeze：
YES

已验证 UWB branch：

dg202611-uwb-model1-integration

已验证 UWB frozen HEAD：

babf4b5a73aec96098bb7db03e77bd3f4aa32053

UWB：
- UWB_01~05 PASS
- UWB tests 8/8 PASS
- regression 67/67 PASS
- MODEL1 UWB integration PASS
- 6 logical / 3 physical dedup PASS

上一 TASK163 会话已做：

- 创建 branch：
  dg202611-prehardware-closure

- 识别 packages：
  ylhb_base
  ylhb_interfaces
  ylhb_perception
  ylhb_3d_mapping
  ylhb_llm
  ylhb_mobile_bridge

- 发现关键 launch：
  dg_navigation_integration.launch.py

- 发现关键 config：
  base_kinematics.yaml
  nav2_params.yaml
  ekf.yaml

但除此以外 TASK163 基本没有执行。

注意：

上一会话报告中的基线：

5c35468687cf0816058b8e26d36c51aeca7df65e

可能不是当前最新 UWB frozen HEAD。

第一件事必须确认当前正式仓库实际状态。

执行：

pwd
git status --short
git branch --show-current
git rev-parse HEAD
git log --oneline -15
git branch -a

确认：

- 正式仓库为 electric-power-inspection-robot
- babf4b5a73aec96098bb7db03e77bd3f4aa32053 是否存在
- dg202611-prehardware-closure 当前基于哪个 commit
- TASK160 UWB 成果是否确实包含在当前 branch

如果 dg202611-prehardware-closure 错误建立在旧基线：

不要 reset --hard。

安全地将最新冻结成果合入/重建正确的 continuation branch。

禁止丢失：

MODEL-1
MODEL-2
MODEL-3
TASK159
TASK160 UWB

目标：

在进入真实机器人、UWB、GNSS、LiDAR、IMU、底盘实验之前，把所有不需要真实硬件的软件工作一次性做完。

执行模式：

AUDIT
→ IMPLEMENT
→ BUILD
→ TEST
→ FIX
→ CONTINUE

普通问题自行解决。

不要因为单个测试失败就 RETURN_TO_MASTER。

只有真实外部环境阻塞才允许停止。

==================================================

PHASE 1：完整软件审计

审计整个 repository。

建立比赛需求对应当前实现状态。

分类：

SOFTWARE_COMPLETE
SOFTWARE_REMAINING
HARDWARE_REQUIRED
REAL_EXPERIMENT_REQUIRED
SUBMISSION_ONLY

优先完成所有 SOFTWARE_REMAINING。

不要生成大型审计报告。

只把结果用于执行。

==================================================

PHASE 2：ROS2 正式集成检查

重点检查：

ylhb_base
ylhb_interfaces
ylhb_perception
ylhb_3d_mapping

以及：

dg_navigation_integration.launch.py

确认 MODEL1/MODEL2/MODEL3/UWB 是否真正安装、启动和连通，而不是只存在脚本文件。

检查：

CMakeLists.txt
package.xml
install(PROGRAMS ...)
launch
config
topic wiring
parameters
diagnostics

修复缺失。

必须保证正式 workspace build 后节点可找到。

==================================================

PHASE 3：三模型 + UWB 总数据流

验证：

odom
GNSS
LiDAR
AMCL
UWB

→ MODEL-1

MODEL-1 degradation
→ MODEL-2

MODEL-2 decision
→ MODEL-3

MODEL-3:
ACTIVE_SCAN
→ VERIFYING
→ RECOVERED
→ SAFE_HANDOFF
→ PROGRESSIVE_REENTRY

同时验证：

UWB GOOD
→ MODEL1 accepts

UWB DEGRADED
→ deweight

UWB REJECTED
→ reject UWB only

UWB 单源失败 + 其他 source healthy：

MODEL3_TRIGGERED = NO

不得修改已冻结总体规则。

==================================================

PHASE 4：系统级软件场景

实现并运行：

SYS_01_NOMINAL

正常所有输入
→ stable

SYS_02_GNSS_DEGRADE

GNSS 异常
→ MODEL1 正确管理 source

SYS_03_LIDAR_DEGRADE

LiDAR 异常
→ health degradation 正确

SYS_04_LOCALIZATION_FAILURE_RECOVERY

→ MODEL1
→ MODEL2
→ MODEL3
→ RECOVERED

SYS_05_UWB_STABLE

→ MODEL1 consumes UWB

SYS_06_UWB_FAILURE_OTHER_SOURCES_HEALTHY

→ UWB rejected/deweighted
→ MODEL3_TRIGGERED = NO

SYS_07_MULTI_SOURCE_DEGRADATION

→ existing recovery path

SYS_08_RECOVERY_REENTRY

→ SAFE_HANDOFF
→ PROGRESSIVE_REENTRY

GT 只能用于 offline evaluation。

禁止 GT 进入 online decision。

==================================================

PHASE 5：实验数据记录工具

提前准备真实实验阶段工具。

需要能够记录：

UWB raw/parsed
GNSS
LiDAR diagnostics
MODEL1 state
MODEL2 decisions
MODEL3 states
relocalization events
fusion outputs

建立统一 experiment logger。

推荐输出：

CSV
JSONL

ROS bag 如果现有 ROS2 环境适合，也准备启动参数/脚本。

不要伪造真实数据。

==================================================

PHASE 6：比赛指标 evaluator

实现离线：

1. position_error_evaluator

输入：

estimated x/y
ground truth x/y

输出：

count
mean
median
RMSE
P95
max

2. feature_repeatability_evaluator

用于后续真实相邻帧特征重复率统计。

3. relocalization_evaluator

输出：

attempts
successes
success_rate
time_to_recovery
failure_reasons

使用 synthetic fixture 验证 evaluator 正确性。

不得声称真实比赛指标达成。

==================================================

PHASE 7：GNSS / RTK 软件路径

检查：

wtrtk980_nmea.cpp
wtrtk980_nmea_node.cpp
GNSS quality gate/node
launch
config
MODEL1 wiring

确保 GNSS/RTK 软件路径在无硬件条件下已准备好。

使用 mock/synthetic input 验证消息流。

==================================================

PHASE 8：北斗短报文

搜索 repository 和本地已存在项目资料中的相关代码/协议。

禁止把：

NTRIP
4G
LoRa
GNSS RTK

当成北斗短报文。

如果已经存在真实短报文协议：

实现 adapter/interface + tests。

如果没有：

只准备清晰的接口边界和 adapter skeleton。

标记：

BEIDOU_SHORT_MESSAGE_REAL_VALIDATION = NOT_DONE

不允许编造协议。

==================================================

PHASE 9：配置清理

全面检查：

MODEL1
MODEL2
MODEL3
UWB
GNSS
health
relocalization
logging
launch

禁止：

Windows absolute path
relay_repo
staging path
开发机硬编码
假的 anchor coordinates 冒充真实值
fake metrics
fake PASS

真实硬件未知参数：

清晰标记 TODO / placeholder / disabled。

==================================================

PHASE 10：preflight

实现：

scripts/run_dg202611_preflight.sh

或符合现有工程结构的等价入口。

应至少：

build
→ unit tests
→ MODEL1/2/3 tests
→ UWB tests
→ SYS_01~08
→ evaluator tests

必须基于真实 command exit status 判断。

禁止 grep “PASS” 文本作为唯一判定。

==================================================

PHASE 11：真实实验目录工具

实现简单 experiment session 工具。

能够生成：

experiments/
  YYYYMMDD_HHMMSS_<scenario>/
    metadata
    raw/
    csv/
    logs/
    bag/

如果无真实数据：

保持空目录模板/创建逻辑即可。

不要提交大量空文件。

==================================================

PHASE 12：完整回归

运行当前真实全部相关 tests。

必须包含：

MODEL-1
MODEL-2
MODEL-3
UWB
system scenarios
evaluators

要求：

0 regression

报告真实 test 数量。

不要强行保持 67。

==================================================

PHASE 13：Git

正常 commit。

branch：

dg202611-prehardware-closure

禁止：

git reset --hard
force push
删除冻结成果

正常 push。

验证：

LOCAL_HEAD
REMOTE_HEAD
MATCH

==================================================

任务完成标准：

除了以下必须真实硬件/场地才能做的项目：

- real UWB
- UWB anchor real coordinates
- real UWB range
- real UWB 2D
- real GNSS/RTK
- real BeiDou short-message device
- real LiDAR
- real IMU
- real chassis
- full robot integration
- real localization accuracy
- real feature repeatability
- real relocalization success rate
- real experiment screenshots/logs/bags/videos

其他软件工作尽可能全部完成。

最终只返回：

TASK:
DG202611_TASK_163B_PRE_HARDWARE_SOFTWARE_TOTAL_CLOSURE_CONTINUATION

BASELINE_CORRECTED:
YES/NO

FULL_PROJECT_SOFTWARE_AUDIT:
PASS/FAIL

ROS2_BUILD:
PASS/FAIL

SYSTEM_LAUNCH_READY:
YES/NO

MODEL1_MODEL2_MODEL3_INTEGRATION:
PASS/FAIL

UWB_MODEL1_INTEGRATION:
PASS/FAIL

SYS_01_NOMINAL:
PASS/FAIL

SYS_02_GNSS_DEGRADE:
PASS/FAIL

SYS_03_LIDAR_DEGRADE:
PASS/FAIL

SYS_04_LOCALIZATION_FAILURE_RECOVERY:
PASS/FAIL

SYS_05_UWB_STABLE:
PASS/FAIL

SYS_06_UWB_FAILURE_OTHER_SOURCES_HEALTHY:
PASS/FAIL

SYS_07_MULTI_SOURCE_DEGRADATION:
PASS/FAIL

SYS_08_RECOVERY_REENTRY:
PASS/FAIL

EXPERIMENT_LOGGING_TOOLS:
READY/NOT_READY

POSITION_ERROR_EVALUATOR:
READY/NOT_READY

FEATURE_REPEATABILITY_EVALUATOR:
READY/NOT_READY

RELOCALIZATION_EVALUATOR:
READY/NOT_READY

GNSS_RTK_SOFTWARE_PATH:
READY/NOT_READY

BEIDOU_SHORT_MESSAGE_SOFTWARE_INTERFACE:
READY/NOT_READY/NO_PROTOCOL_AVAILABLE

PRE_HARDWARE_PREFLIGHT:
PASS/FAIL

TOTAL_TESTS:
<actual>

REGRESSIONS:
<actual>

SOFTWARE_ONLY_WORK_REMAINING:
<list>

HARDWARE_PHASE_REMAINING:
<list>

FILES_CHANGED:
<list>

FINAL_LOCAL_HEAD:
<sha>

FINAL_REMOTE_HEAD:
<sha>

LOCAL_REMOTE_MATCH:
YES/NO

PRE_HARDWARE_SOFTWARE_FREEZE_READY:
YES/NO

BLOCKER:
NONE/<specific>

NEXT_ACTION:
RETURN_TO_MASTER
~~~~

[本对话目录](README.md) | [总目录](../../README.md) | [Raw](https://raw.githubusercontent.com/weiyang02520-ops/dg202611-conversation-archive/main/conversations/second/part-010.md) | [上一卷](part-009.md) | [下一卷](part-011.md)
