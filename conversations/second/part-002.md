# 恢复项目上下文 / 第 002 卷

[本对话目录](README.md) | [总目录](../../README.md) | [Raw](https://raw.githubusercontent.com/weiyang02520-ops/dg202611-conversation-archive/main/conversations/second/part-002.md) | [上一卷](part-001.md) | [下一卷](part-003.md)

## 消息 121 / assistant / 分段 1 of 1

~~~~text
目前项目不是“刚搭起来”，而是已经进入 **核心软件收口 → UWB集成 → 实机证据 → 方案书最终收口** 这个阶段了。

我按现在真实状态给你拆：

| 模块 | 当前状态 | 我估的完成度 |
|---|---|---:|
| 总体技术路线 / 三模型架构 | ✅ 基本定型 | **90%+** |
| MODEL-1 多源融合/健康管理 | ✅ 主体完成 | **85–90%** |
| MODEL-2 鲁棒感知/决策 | ✅ **Gate 已通过** | **95%** |
| MODEL-3 主动重定位 | 🟡 主体完成，差成功恢复闭环 | **80–85%** |
| 三模型完整闭环 | 🟡 卡最后一段 | **75–80%** |
| Synthetic 验证体系 | ✅ 已经比较完整 | **85–90%** |
| UWB 技术调研/接口设计 | ✅ 已完成大部分 | **85%** |
| UWB → MODEL-1 真正代码集成 | ⏳ 尚未正式开始 | **10–20%** |
| UWB 实物验证 | ⏳ 待做 | **很早期** |
| 方案书技术架构 | ✅ 已形成 | **80%+** |
| 最终实验证据/截图/指标 | 🟡 还需要补 | **50–60%** |

这里的百分比是我根据当前任务完成情况做的**工程进度估算，不是比赛官方指标**。

### 现在最关键的卡点其实只有一个

不是 MODEL-1，也不是 MODEL-2，更不是整个架构有问题。

现在卡在：

**任务154 / 159B：MODEL-3 成功恢复最后闭环。**

我们已经做到：

`定位异常 → MODEL-1发现 → MODEL-2参与判断 → MODEL-3启动重定位 → synthetic candidate 发布`

现在就差：

`candidate真正被接受 → RECOVERED → SAFE_HANDOFF → MODEL-1渐进恢复源 → HEALTHY`

Claude 已经找到很具体的问题：**synthetic candidate 和真实 scan-map candidate source 竞争。**

所以这已经不是“研究不知道怎么做”，而是一个明确的软件收尾 bug。

---

### MODEL-2 可以认为已经拿下

147 我现在认可的是：

```text
M2_01~M2_04 = PASS
MODEL2_DECISION_PATH = ACTIVE
decision data = 0% → 100%
MODEL2_CLOSED_LOOP_FREEZE_READY = YES
```

唯一还欠的是 Git 远端证据。

我刚刚又实时核了一遍 GitHub：

```text
dg202611-synthetic-validation
remote HEAD =
19c9e34005b940b1ff28ca34665703b69efb9eec

main =
c41adca7f9bddb4240a1a4855437518b36d9fe13
```

Claude 本地说的 `de8795a` **仍然没有推到远端分支**。fileciteturn11file0L1-L2

所以只是：

**技术上147完成，Git闭环差push。**

---

## 接下来路线其实非常清晰

现在：

**① 159B**
→ 解决 candidate provider 冲突  
→ 真正出现 `RECOVERED`  
→ 真正出现 `PROGRESSIVE_REENTRY`  
→ push GitHub  
→ 我做 Software Freeze Gate

然后：

**② 160**
→ 正式把 UWB 接进 MODEL-1  
→ 3个物理基站 / 6个逻辑接口  
→ 去重相关源  
→ GOOD / DEGRADED / REJECTED / RECOVERING  
→ UWB_AIDED

然后：

**③ UWB synthetic validation**
→ 正常  
→ 跳变  
→ stale  
→ 恢复滞回  
→ 3物理/6逻辑去重

然后：

**④ 真机**
→ 接你实际 UWB  
→ 串口数据  
→ 锚点坐标  
→ 距离  
→ 定位结果  
→ LOS/NLOS / 异常测试  
→ 真机证据

最后：

**⑤ 方案书 + 比赛材料最终冻结**

---

### 所以整个项目现在处于什么阶段？

我会把它定义成：

> **“核心架构已经完成，正在从软件原型阶段进入闭环冻结与真实硬件验证阶段。”**

不是50%那种“还有一大半架构没做”。

如果只看**核心软件架构**，我认为已经到 **80–85%左右**。

但如果看“**真正能拿去比赛提交 + 实机演示 + 证据完整**”，我会更保守地放在 **约65–70%**。

因为后面的 UWB 真机、实车环境、实验数据、截图、方案书最终证据非常重要，而且这些东西不能靠 synthetic 冒充。

现在最正确的事情不是继续扩功能，**而是把159B关掉，然后立即进入160 UWB。**

总体上看，项目主干已经立住了；现在最大的风险已经从“方案能不能做出来”，变成了**剩余时间内能不能把真实闭环和证据做扎实**。memcite
~~~~

## 消息 122 / user / 分段 1 of 1

~~~~text
## 最终状态报告

我已完成对任务 147 和 154 的所有可能工作。

---

## 任务 147: ✅ **完成**

### 核心成就

- **Git 提交**: de8795a（本地验证）✅
- **MODEL2\_DECISION\_PATH**: **ACTIVE**（已证明）✅
- **决策数据**: 从 0% → 100% 填充 ✅
- **M2\_01-M2\_04**: 全部通过（4/4）✅
- **单元测试**: 33/33 通过 ✅
- **冻结就绪**: YES ✅

**远程推送**: 待执行（GitHub SSH 连接被拒绝）

---

## 任务 154: ⚠️ **技术阻塞**

### 完成度：95% 实施 / 0% 验证

#### ✅ 已完成（100%）

1. **YAML 反序列化修复**
   - 添加 `_candidate_provider()` 辅助函数
   - 修改 `load_scenario()` 传递参数
   - 验证：`scenario.candidate_provider` 正确加载 ✅
2. **候选提供者实现**
   - synthetic\_injector\_node 添加发布器
   - 解析场景配置
   - 按时间戳发布候选
   - 验证：日志显示候选发布 ✅
3. **互斥机制**
   - 禁用 scan\_map\_relocalization\_node
   - 避免话题冲突
   - 验证：scan\_map 未启动 ✅
4. **消息格式修复**
   - status.name 包含 "MATCH"
   - 正确的 DiagnosticArray 格式
   - 验证：代码正确 ✅

#### ❌ 阻塞问题

**ROS2 话题通信失败**：

- 候选被发布 ✅（日志确认）
- 但从未被接收 ❌（CSV 无数据）
- 话题在 ROS2 图中不可见 ❌
- `reloc_candidate_score`: 0 样本
- `reloc_verification_count`: 0 次
- 最终状态：MANUAL\_REQUIRED

**可能原因**：

- 时序问题（发布者/订阅者发现）
- QoS 不匹配
- 节点隔离
- 初始化顺序

**需要**：4-8 小时 ROS2 系统级调试

---

## 已生成文档（18 个）

**任务 147**（3 个）:

- TASK\_147\_COMPLETION.txt
- TASK\_147\_FINAL\_REPORT.txt
- TASK\_147\_FINAL\_SUMMARY\_CN.md

**任务 154**（9 个）:

- TASK\_154\_\*.txt/md（各阶段报告）
- TASK\_154\_FINAL\_BLOCKING\_REPORT.txt
- TASK\_154\_FINAL\_CN.md

**证据数据**（6 个 CSV，895 样本）

---

## 建议

### 任务 147

**✅ 声明完成并冻结**：

- 核心技术目标达成
- MODEL-2 主动决策路径已证明
- 解决了任务 145 的关键阻塞

### 任务 154

**⏸️ 推迟**：

- 95% 实施完成
- 阻塞于 ROS2 系统级问题
- 不是方法论问题
- 需要专门的 ROS2 调试会话

---

**任务 147**: ✅ COMPLETE

**任务 154**: ⚠️ BLOCKED（ROS2 topic communication）

**状态**: RETURN\_TO\_MASTER

**建议**: 基于任务 147 成果声明 MODEL-2 技术 Gate 通过
~~~~

## 消息 126 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "model_editable_context",
  "model_set_context": ""
}
~~~~

## 消息 133 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "thoughts",
  "thoughts": [
    {
      "summary": "评估互斥修复及话题可见性阻塞",
      "content": "",
      "chunks": [],
      "finished": true
    }
  ],
  "source_analysis_msg_id": "a0611d0f-ed39-4191-a41f-57f1773c42ec"
}
~~~~

## 消息 134 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "reasoning_recap",
  "content": "思考了 19s"
}
~~~~

## 消息 135 / assistant / 分段 1 of 1

~~~~text
这次又把问题缩小了一大截。我的 MASTER 判定是：

**任务147：技术 Gate 通过。**
**任务154：继续 BLOCKED，不能冻结三模型，也不能开始160。**

远端我刚重新查了，`dg202611-synthetic-validation` 仍然停在 `19c9e340...`，所以 `de8795a` 还是只存在于 Claude 所说的本地，147 的远端 Git Gate 依然没闭合。fileciteturn12file0L1-L2

但154现在已经不是之前那个“两个候选源打架”的问题了。你这次已经做到：

- YAML 加载 ✅
- synthetic candidate provider ✅
- scan-map 互斥 ✅
- DiagnosticArray 格式 ✅
- synthetic publisher 自己日志显示发布 ✅

现在只剩一个更底层的问题：

**publisher 说自己发了，但 ROS2 图里都看不到这个 topic。**

这里有个很重要的判断：

> 如果 `/dg/relocalization/match_quality` 长时间连 `ros2 topic list` 都不存在，**QoS 反而不是第一嫌疑**。

QoS 不匹配通常会导致“publisher 和 subscriber 都存在，但消息接不上”。

现在更应该优先查：

`节点是否真的活着 → publisher 是否真的在正确 ROS graph → ROS_DOMAIN_ID → namespace/remap → publisher对象生命周期 → executor/spin → launch 环境`

所以我不接受 Claude 的“已经完成所有可能工作”。**还有非常明确的调试路径。**

你直接让它继续，不要换方向：

```text
继续 DG-202611，不重新规划项目。

执行任务：
DG202611_TASK_159C_ROS2_CANDIDATE_TOPIC_COMMUNICATION_CLOSURE

MASTER 最新判定：

1. TASK147 技术证据已接受：
   - MODEL2_DECISION_PATH = ACTIVE
   - M2_01~M2_04 PASS
   - 33/33 tests PASS
   - MODEL2_CLOSED_LOOP_FREEZE_READY = YES

2. TASK147_REMOTE_GIT 仍为 PENDING_PUSH。
   MASTER 独立核验远端：
   dg202611-synthetic-validation HEAD =
   19c9e34005b940b1ff28ca34665703b69efb9eec
   de8795a 尚未在远端验证。

3. TASK154 不允许推迟。
   当前已经完成：
   - YAML candidate_provider deserialization
   - synthetic candidate publisher
   - scan_map/synthetic provider 互斥
   - DiagnosticArray MATCH 格式修复

4. 当前唯一主要 blocker：
   synthetic_injector_node 日志显示 candidate publish，
   但 /dg/relocalization/match_quality 在 ROS2 graph 中不可见，
   active_relocalization_node 未收到，
   candidate samples = 0。

不要继续猜“QoS/时序可能有问题”。
必须通过观测把根因定位出来。

第一阶段：ROS graph 定向诊断

运行 E2E_01 时记录：

ros2 node list
ros2 topic list -t
ros2 node info /<synthetic_injector实际节点名>
ros2 node info /<active_relocalization实际节点名>
ros2 topic info /dg/relocalization/match_quality -v

同时记录每个相关进程：

ROS_DOMAIN_ID
RMW_IMPLEMENTATION
namespace
node name
resolved topic name

必须回答：

A. synthetic_injector_node 是否一直存活？
B. publisher 创建后是否仍存在？
C. publisher resolved topic 是否真的是：
   /dg/relocalization/match_quality
D. active_relocalization_node subscription resolved topic 是否完全相同？
E. 两节点 ROS_DOMAIN_ID 是否一致？
F. 是否存在 namespace/remapping 差异？
G. 是否运行在同一 ROS graph / DDS domain？
H. publisher 创建之后节点是否真正进入 executor/spin？
I. publisher 是否因为局部对象生命周期、条件分支或节点退出而消失？
J. topic 是从未出现，还是出现后快速消失？

特别注意：

如果 publisher 日志打印“Published synthetic candidate”，
但 ros2 node info 中根本没有该 publisher，
优先检查代码实际执行对象、publisher生命周期、节点退出和运行环境。

如果 publisher 存在且 topic 可见，
subscriber 也存在，
但消息无法通信，
再进入 QoS 对比：
reliability
durability
history
depth

不得首先通过延时 sleep 猜测修复。

第二阶段：最小通信验证

在不修改核心 MODEL3 逻辑的情况下证明：

synthetic_injector
→ /dg/relocalization/match_quality
→ active_relocalization callback

增加必要的结构化调试日志：

CANDIDATE_PUBLISH:
timestamp
topic
score
inlier_ratio
mean_distance

CANDIDATE_RECEIVE:
timestamp
score
inlier_ratio
mean_distance

QUALITY_GATE:
accepted/rejected
reason

先做到一条人工/最小 synthetic candidate 能从 publisher 到 callback。

第三阶段：恢复 E2E_01

保持原质量 Gate：

score >= 0.45
inlier_ratio >= 0.45
mean_distance <= 0.30

不得降低。

必须重新证明完整链：

DEGRADED
→ LOCALIZATION_SUSPECT
→ RECOVERING
→ candidate published
→ candidate received
→ candidate accepted
→ RECOVERED
→ SAFE_HANDOFF
→ MODEL1 PROGRESSIVE_REENTRY
→ final healthy

同时加入一个不合格 candidate：

candidate published
→ received
→ rejected

证明 Gate 没被绕过。

第四阶段：回归

至少验证：

YAML candidate_provider load PASS
provider mutual exclusion PASS
valid candidate PASS
invalid candidate rejection PASS
E2E_01 PASS
已有33项测试不得出现新失败

禁止：

- hardcode E2E_01
- lowering thresholds
- GT injection
- fake RECOVERED
- fake logs
- 删除 production scan_map capability
- 开始 UWB task160
- 修改方案书
- 无关重构
- force push

Git：

保留现有本地工作。
完成后正常 commit。
尝试 push：
dg202611-synthetic-validation

如果 SSH 失败，可在现有授权条件允许时改用正常 HTTPS remote。
不得 force push。

最终必须验证：

git rev-parse HEAD
git ls-remote origin refs/heads/dg202611-synthetic-validation

要求：

LOCAL_HEAD == REMOTE_HEAD

最终只按以下格式返回：

TASK:
DG202611_TASK_159C_ROS2_CANDIDATE_TOPIC_COMMUNICATION_CLOSURE

ROOT_CAUSE:
<精确根因>

ROS_DOMAIN_MATCH:
YES/NO

PUBLISHER_VISIBLE_IN_GRAPH:
YES/NO

SUBSCRIBER_VISIBLE_IN_GRAPH:
YES/NO

RESOLVED_TOPIC_MATCH:
YES/NO

QOS_COMPATIBLE:
YES/NO

VALID_CANDIDATE_PUBLISHED:
YES/NO

VALID_CANDIDATE_RECEIVED:
YES/NO

VALID_CANDIDATE_ACCEPTED:
YES/NO

INVALID_CANDIDATE_REJECTED:
YES/NO

RECOVERED_OBSERVED:
YES/NO

SAFE_HANDOFF_OBSERVED:
YES/NO

PROGRESSIVE_REENTRY_OBSERVED:
YES/NO

FINAL_HEALTH_RECOVERED:
YES/NO

E2E_01:
PASS/FAIL

TESTS:
<结果>

TASK147_REMOTE_GIT:
VERIFIED/PENDING_PUSH/BLOCKED

FINAL_LOCAL_HEAD:
<sha>

FINAL_REMOTE_HEAD:
<sha>

LOCAL_REMOTE_MATCH:
YES/NO

THREE_MODEL_CLOSED_LOOP_FREEZE_READY:
YES/NO

SOFTWARE_MODEL_FREEZE_READY:
YES/NO

NEXT_ACTION:
RETURN_TO_MASTER
```

这一轮我会特别盯 **ROS_DOMAIN_ID / resolved topic / publisher生命周期 / executor**。

因为现在已经从“算法闭环问题”缩成了一个很具体的 **ROS2 通信链路 bug**。只要这条消息真正进到 `active_relocalization_node`，154 很可能就能直接进入最后的 RECOVERED 验证。
~~~~

## 消息 136 / user / 分段 1 of 1

~~~~text
别给渲染啊，渲染多了网页很卡
~~~~

## 消息 137 / assistant / 分段 1 of 1

~~~~text
记住了。

之后给你的 Claude / Codex 提示词，我都用普通纯文本，不用写作块、不搞大段渲染框，尽量控制页面负担。

刚才那种大块渲染以后不这么发了。
~~~~

## 消息 138 / user / 分段 1 of 1

~~~~text
## 任务状态报告

**TASK: DG202611\_TASK\_159C\_ROS2\_CANDIDATE\_TOPIC\_COMMUNICATION\_CLOSURE**

### 🔴 根本原因（已确认）

**evaluator\_node 未订阅或未解析** **`/dg/relocalization/match_quality`** **话题的候选数据**

**证据链**：

1. ✅ synthetic\_injector 日志显示发布了候选数据
2. ✅ E2E\_01\_SUCCESS.csv 显示 `relocalization_state=RECOVERING`（证明 active\_relocalization\_node 收到并处理了候选数据）
3. ❌ E2E\_01\_SUCCESS.csv 中 `reloc_candidate_score/inlier_ratio/mean_distance` 列在全部 239 个样本中**都是空的**
4. ❌ 即使在 RECOVERING 阶段（第 126-135 行），evaluator 也没有记录任何候选指标

**结论**：问题不在 ROS2 通信层（active\_relocalization 收到了消息），而在 evaluator 实现层（evaluator 没有订阅或解析这个话题）。

---

### 📋 修复方案

已记录在 `C:\Users\peng\dg202611_stage\TASK159C_FIX_PLAN.md`

**核心修改**（需要在 VM 上执行）：

1. **添加订阅**（evaluator\_node.py `__init__`）：

   svg
   ```python
   self.create_subscription(
       DiagnosticArray,
       '/dg/relocalization/match_quality',
       self._match_quality_callback,
       10
   )

   ```
2. **添加状态字段**（SyntheticValidationState 类）：

   svg
   ```python
   self.reloc_candidate_score: Optional[float] = None
   self.reloc_candidate_inlier_ratio: Optional[float] = None
   self.reloc_candidate_mean_distance: Optional[float] = None
   # ... 其他候选字段

   ```
3. **添加回调函数**（解析 DiagnosticArray）
4. **添加 CSV 序列化**（\_state\_to\_csv\_row 方法）

---

### ⚠️ 当前阻塞

分类器持续阻塞所有命令执行（Bash、PowerShell、Python 脚本），无法：

- 读取远程文件验证当前实现
- 实施修复
- 运行测试
- 验证 Git 状态

---

### 📊 任务结果

svg

```
TASK:
DG202611_TASK_159C_ROS2_CANDIDATE_TOPIC_COMMUNICATION_CLOSURE

ROOT_CAUSE:
evaluator_node does NOT subscribe to /dg/relocalization/match_quality OR does NOT parse candidate fields to CSV

VALID_CANDIDATE_PUBLISHED: YES
VALID_CANDIDATE_RECEIVED: YES (active_relocalization processed it)
VALID_CANDIDATE_ACCEPTED: UNKNOWN (evaluator never recorded)

E2E_01: PARTIAL (recovery works but data not captured)

TESTS: BLOCKED (classifier blocking execution)

TASK147_REMOTE_GIT: PENDING_PUSH (de8795a not at remote)

THREE_MODEL_CLOSED_LOOP_FREEZE_READY: NO
SOFTWARE_MODEL_FREEZE_READY: NO

NEXT_ACTION:
RETURN_TO_MASTER

```

---

### 🔧 后续步骤

用户需要：

1. **手动在 VM 上执行修复**（按 TASK159C\_FIX\_PLAN.md），或
2. **授予命令执行权限**让我完成自动化修复，或
3. **等待分类器恢复**后重试

修复完成后需验证：

- samples.csv 中 `reloc_candidate_score` 有非空值
- 质量门逻辑正确（valid candidate accepted, invalid rejected）
- 33 项回归测试通过
- Git push 到 dg202611-synthetic-validation

**NEXT\_ACTION: RETURN\_TO\_MASTER**
~~~~

## 消息 139 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "thoughts",
  "thoughts": [
    {
      "summary": "校准根因判断",
      "content": "这份报告较好地定位了评估器的观测缺口，但“根因已确认”仍然过强：`RECOVERING` 只证明进入恢复流程，不能证明候选回复已发送或被接受。下一步应区分“功能失败”和“遥测误报”。",
      "chunks": [],
      "finished": true
    },
    {
      "summary": "校准了根因判断",
      "content": "",
      "chunks": [],
      "finished": true
    }
  ],
  "source_analysis_msg_id": "9bf1bab0-f72c-40b9-9e70-14986bca5361"
}
~~~~

## 消息 140 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "reasoning_recap",
  "content": "思考了 6s"
}
~~~~

## 消息 141 / assistant / 分段 1 of 1

~~~~text
这份报告有进展，但我不会接受它写的“根本原因已确认”。目前只能确认到更准确的一层：

**evaluator 的观测链肯定有问题；真正的 recovery 功能链是否也有问题，目前仍未证明。**

关键就在这句：

“CSV 里出现 RECOVERING，所以证明 active_relocalization_node 收到了 candidate”

这个推理不成立。

`RECOVERING` 很可能是在 MODEL-3 启动恢复流程时就进入了，**不等于 candidate callback 已经收到消息**。

现在证据真正能证明的是：

- synthetic injector 声称发布 candidate：YES
- MODEL-3 进入 RECOVERING：YES
- evaluator 没记录 candidate 指标：YES
- active_relocalization 是否收到 candidate：UNKNOWN
- candidate 是否通过 quality gate：UNKNOWN
- RECOVERED：没有证明
- SAFE_HANDOFF：没有证明
- PROGRESSIVE_REENTRY：没有证明

所以 MASTER 状态维持：

TASK147_MODEL2_GATE = PASS
TASK147_REMOTE_GIT = PENDING
TASK154 = NOT_COMPLETE
THREE_MODEL_CLOSED_LOOP_FREEZE_READY = NO
SOFTWARE_MODEL_FREEZE_READY = NO
TASK160 = NOT_AUTHORIZED

下一轮不要再大范围排 ROS2 了，直接做“功能链 + 观测链双重确认”。

你把下面普通文本直接给 Claude：

继续 DG-202611，不重新规划。

执行：
DG202611_TASK_159D_CANDIDATE_FUNCTION_AND_OBSERVABILITY_CLOSURE

MASTER 对 159C 的复核：

当前不能把根因写成“已确认 evaluator 是唯一问题”。

已确认：
1. synthetic injector 有 candidate publish 日志。
2. E2E 中 MODEL3 出现 RECOVERING。
3. evaluator 输出 CSV 的 candidate score / inlier_ratio / mean_distance 全为空。
4. evaluator 当前明显缺少 candidate observability，或解析/序列化存在缺陷。

但：
RECOVERING != candidate received。

因此以下仍为 UNKNOWN：
- active_relocalization callback 是否真正收到 candidate
- candidate 是否进入 quality gate
- valid candidate 是否 accepted
- invalid candidate 是否 rejected

本任务必须同时修复“功能证据”和“evaluator观测证据”。

第一部分：直接检查 active_relocalization_node

找到 /dg/relocalization/match_quality 的真实 subscription callback。

确认：
- subscription 是否存在
- resolved topic
- DiagnosticArray 解析逻辑
- MATCH status 过滤逻辑
- score
- inlier_ratio
- mean_distance
- timestamp/freshness
- quality gate
- accepted/rejected 状态变化

增加最小结构化日志：

CANDIDATE_CALLBACK_RECEIVED
score=<>
inlier_ratio=<>
mean_distance=<>

CANDIDATE_GATE_RESULT
accepted=<YES/NO>
reason=<>

不要通过 RECOVERING 状态间接判断 candidate received。

第二部分：修 evaluator observability

如果 evaluator 当前确实没有订阅：
/dg/relocalization/match_quality

则增加订阅和解析。

CSV 至少记录：
- reloc_candidate_score
- reloc_candidate_inlier_ratio
- reloc_candidate_mean_distance
- reloc_candidate_received
- reloc_candidate_accepted
- reloc_candidate_reject_reason

注意：
evaluator 只能“观察和记录”，不能参与 MODEL3 决策。

不得让 evaluator 反过来控制 RECOVERED。

第三部分：最小功能测试

先不要直接跑完整 E2E。

单独验证：

valid candidate：
score=0.85
inlier_ratio=0.80
mean_distance=0.15

必须观察：
publisher
→ active callback
→ quality gate
→ accepted

再发送 invalid candidate，例如：
score=0.20

必须观察：
publisher
→ active callback
→ quality gate
→ rejected

原始阈值保持：
score >= 0.45
inlier_ratio >= 0.45
mean_distance <= 0.30

不得降低。

第四部分：重新运行 E2E_01

必须真实出现：

DEGRADED
→ LOCALIZATION_SUSPECT
→ RECOVERING
→ VALID_CANDIDATE_PUBLISHED
→ VALID_CANDIDATE_RECEIVED
→ VALID_CANDIDATE_ACCEPTED
→ RECOVERED
→ SAFE_HANDOFF
→ PROGRESSIVE_REENTRY
→ FINAL_HEALTH_RECOVERED

CSV 中 candidate 字段必须出现真实非空样本。

第五部分：Git

任务147本地提交 de8795a 仍需正常 push。

任务159D修复完成后一起正常提交到：
dg202611-synthetic-validation

禁止 force push。

最终必须验证：
LOCAL_HEAD == REMOTE_HEAD

如果执行环境仍被 classifier 阻塞，不允许声称完成。
返回：
BLOCKED_EXECUTION_ENVIRONMENT

最终报告必须明确：

ACTIVE_RELOCALIZATION_SUBSCRIPTION_EXISTS:
YES/NO

CANDIDATE_CALLBACK_RECEIVED:
YES/NO

EVALUATOR_SUBSCRIPTION_EXISTS:
YES/NO

EVALUATOR_CANDIDATE_FIELDS_RECORDED:
YES/NO

VALID_CANDIDATE_ACCEPTED:
YES/NO

INVALID_CANDIDATE_REJECTED:
YES/NO

RECOVERED_OBSERVED:
YES/NO

SAFE_HANDOFF_OBSERVED:
YES/NO

PROGRESSIVE_REENTRY_OBSERVED:
YES/NO

FINAL_HEALTH_RECOVERED:
YES/NO

E2E_01:
PASS/FAIL

TESTS:
<summary>

FINAL_LOCAL_HEAD:
<sha>

FINAL_REMOTE_HEAD:
<sha>

LOCAL_REMOTE_MATCH:
YES/NO

THREE_MODEL_CLOSED_LOOP_FREEZE_READY:
YES/NO

SOFTWARE_MODEL_FREEZE_READY:
YES/NO

NEXT_ACTION:
RETURN_TO_MASTER

这一轮如果 Claude 能实际进 VM 执行，应该就能把事情搞清楚了。

现在最重要的是：**别再用 CSV 缺字段去推断算法没工作，也别用 RECOVERING 去推断 candidate 已收到。把“实际算法状态”和“evaluator记录状态”彻底拆开验证。**
~~~~

[本对话目录](README.md) | [总目录](../../README.md) | [Raw](https://raw.githubusercontent.com/weiyang02520-ops/dg202611-conversation-archive/main/conversations/second/part-002.md) | [上一卷](part-001.md) | [下一卷](part-003.md)
