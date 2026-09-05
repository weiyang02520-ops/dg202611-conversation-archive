# 机器人定位挑战赛分析 / 第 085 卷

[本对话目录](README.md) | [总目录](../../README.md) | [Raw](https://raw.githubusercontent.com/weiyang02520-ops/dg202611-conversation-archive/main/conversations/first/part-085.md) | [上一卷](part-084.md) | [下一卷](part-086.md)

## 消息 3434 / assistant / 分段 1 of 1

~~~~text
可以，按**“距离最终比赛交付完成”**来估，我给你一个更直观的百分比：

| 方向 | 当前完成度 | 说明 |
|---|---:|---|
| **正式方案书** | **≈ 65%** | 第1–5章已过，第6章在写，后面还有第7–9章、总装、参考文献、图表、排版 |
| **文献与证据库** | **≈ 90%** | ZCode P0 已完成，主要错误已修，后面只需终稿 DOI/原文总核验 |
| **Synthetic / Package A** | **≈ 95%** | 已冻结，主要只差远端 push/少量非阻塞缺口 |
| **Gazebo 仿真** | **≈ 70%** | P0 PASS，P1 默认配置明显改善，但 step-size 鲁棒性还没过 |
| **核心算法设计** | **≈ 75%** | 模型1、模型2已经写透，模型3正在写；但“设计完成”≠代码实装完成 |
| **核心算法实际开发** | **≈ 40%** | supervisor/synthetic 有基础，但真正融合、稳定特征、真实 matcher/active relocalization 还没全部落地 |
| **UWB** | **≈ 20%** | 设计已明确，真实硬件接入/融合/实验基本还没做 |
| **北斗短报文** | **≈ 30%** | 官方证据和架构准备较好，但真实终端、协议、卫星链路验证还没做 |
| **Jetson / 实车集成** | **≈ 25%** | 已有硬件和驱动基础，但还有 wheel_track 配置风险，系统级实车验证尚未展开 |
| **正式三项指标实验** | **≈ 15%** | 20 cm、95% repeatability、>95% relocalization 目前都还没有正式项目级证据 |
| **比赛最终交付包** | **≈ 40%** | 文档、代码、说明、样机、正式实验、最终PDF/ZIP都还需要整合 |

### 总体我会给：

**当前整个项目完成度：≈ 55%～60%**

但这里有个很重要的区别：

**“方案和技术路线成熟度”已经大约 75%～80%，  
“真正能拿去比赛验收的工程/实验完成度”只有大约 40%～45%。**

也就是说，我们现在**不是“不知道怎么做”了**，而是已经进入后半程：  
**把设计真正做出来 → 上实车 → 跑正式指标 → 填回方案书 → 最终交付。**

如果按阶段看，大概就是：

**前期调研/方案：90%**  
**系统设计：80%**  
**仿真验证：70%**  
**代码与算法实现：40%**  
**实车集成：25%**  
**正式指标实验：15%**  
**最终提交：40%**。memcite
~~~~

## 消息 3435 / user / 分段 1 of 1

~~~~text
已完成 **第6章《剧烈运动、跨域切换与感知退化下的具身主动重定位模型》正式写作**，按任务要求输出到：

`C:\Users\peng\Desktop\材料\DG-202611_FORMAL_SUBMISSION_REBUILD\DSH_ROUND6\`

## 生成文件

| 文件 | 说明 |
|---|---|
| `01_CHAPTER_6_DRAFT_v0.1.md` | 第6章正式初稿，约 2073 个汉字 |
| `02_CHAPTER_6_MODEL_EQUATION_AND_METRIC_PLAN.md` | 模型公式与指标定义规划 |
| `03_CHAPTER_6_FIGURE_TABLE_PLAN.md` | 图6-1～6-4、表6-1～6-4 规划 |
| `04_CHAPTER_6_CLAIM_REFERENCE_MAP.md` | 第6章 Claim—Reference Map |
| `05_DSH_CH6_SELF_CHECK.md` | 第6章自检 |

## 本章核心内容

- **具身主动重定位定义**：明确不是调用大模型，而是“感知—判断—动作—再感知—验证”闭环；ActLoc 仅作为主动视角/主动定位思想依据，不写“本项目采用 ActLoc 网络”。
- **触发逻辑**：由健康/恢复层形成，输入来自模型1的 covariance、quality、innovation、degradation flags、continuity，以及模型2的 stable feature count、geometry coverage、feature uncertainty、masks；阈值不虚构，由第9章冻结。
- **恢复控制模式**：`normal navigation → stop original navigation control → enter constrained recovery control → active search motion`；明确“停止”不等于机器人绝对静止，动作受线速度/角速度/碰撞/最大搜索时间限制。
- **主动动作集合**：原地/小半径旋转、左右扇扫、短距离前后移动、朝向高几何信息方向调整、小范围组合搜索；禁止无约束大范围探索和高速随机运动。
- **动作价值**：`J(a)=w_g G(a)+w_v V(a)+w_u U(a)-w_c C(a)-w_t T(a)`，权重不冻结。
- **候选生成与接受分离**：明确 candidate generation ≠ candidate acceptance；单次 matcher success 不等于重定位成功。
- **候选置信度**：`C_cand=f(s_match,u_pose,g_coverage,q_lidar,q_vision,q_absolute)`，高不确定候选直接拒绝。
- **多周期/多帧验证 + 多源一致性**：候选经临时接受、多周期稳定性、多源一致性、不确定度检查后才进入恢复确认；与 IMU/轮速/GNSS/UWB/ZED/LiDAR 稳定特征交叉验证。
- **渐进重接入**：恢复确认 → 渐进重接入 → 健康保持 → 安全交还导航；hold 期间再次失效则该 trial 失败。
- **SAFE_FAIL**：超时、候选耗尽、安全中止、反复拒绝、验证失败均进入安全失效；可形成北斗短报文事件（第7章展开）。
- **正式 trial 定义**：一次 trial 从首次进入需主动恢复到稳定恢复或最终失败；多个内部 attempt 不拆成多个 trial。
- **成功判据**：主判据 `e_xy≤0.30m、e_yaw≤10°、T≤30s、hold≥10s、无人工接管、无错误恢复`；严格伴随 `e_xy≤0.20m、e_yaw≤5°`。
- **统计要求**：`SR=N_success/N_trials`，官方目标 `>95%` 仅作设计/正式验证目标；计划报告 Wilson 95% CI、N_trials/N_success/N_fail、恢复时间 mean/median/P95、FALSE_RECOVERY_COUNT/RATE、场景分层。
- **真值边界**：正式实验 truth 独立于被测 matcher；Gazebo 用 simulator ground truth 做仿真评价，实车用独立高可信定位/测量体系。
- **当前证据边界**：不把 synthetic surrogate、仿真匹配结果或内部软件流程验证作为真实重定位性能证据。

## 自检结果

```text
CHAPTER_6_COMPLETED: YES
EMBODIED_INTELLIGENCE_NOT_LLM: PASS
PASSIVE_TO_ACTIVE_TRIGGER_CLEAR: PASS
SAFE_RECOVERY_CONTROL_CLEAR: PASS
ACTIVE_ACTION_OBJECTIVE_CLEAR: PASS
CANDIDATE_AND_ACCEPTANCE_SEPARATED: PASS
MULTI_CYCLE_VERIFICATION_CLEAR: PASS
MULTI_SOURCE_CONSISTENCY_CLEAR: PASS
PROGRESSIVE_REENTRY_CLEAR: PASS
SAFE_FAIL_PRESENT: YES
TRIAL_DEFINITION_CLEAR: PASS
MULTIPLE_ATTEMPTS_NOT_COUNTED_AS_MULTIPLE_TRIALS: PASS
SUCCESS_CRITERION_30CM_10DEG_30S_10S: DEFINED
STRICT_20CM_5DEG_COMPANION: DEFINED
FALSE_RECOVERY_COUNTED_AS_FAILURE: YES
WILSON_CI_PLANNED: YES
PACKAGE_A_USED_AS_REAL_MATCHER_EVIDENCE: NO
GROUND_TRUTH_SURROGATE_MISREPRESENTED: NO
95_PERCENT_WRITTEN_AS_CURRENT_RESULT: NO
GOReloc_OLD_ERROR_REUSED: NO
PROJECT_RESULTS_FABRICATED: NO
CHAPTER_7_WRITTEN: NO
RETURN_TO_MASTER: YES
```

已停止，等待 MASTER 审查并放行第7章。
~~~~

## 消息 3443 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "thoughts",
  "thoughts": [
    {
      "summary": "检查技术一致性",
      "content": "我正在核对文件与总结是否完全一致，并重点检查指标定义。一个关键问题是恢复时间应从触发到确认恢复计算，稳定保持时间应单独计时，否则会把两项要求混在一起。",
      "chunks": [],
      "finished": true
    },
    {
      "summary": "检查了技术一致性",
      "content": "",
      "chunks": [],
      "finished": true
    }
  ],
  "source_analysis_msg_id": "017f2a17-9c42-4822-a966-2797840e0e16"
}
~~~~

## 消息 3444 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "thoughts",
  "thoughts": [
    {
      "summary": "完善验收定义",
      "content": "我正在区分正式验收标准与补充统计信息，避免用置信区间下限擅自改变目标。除此之外，我还需要补足剧烈运动后的稳定条件，以及跨域切换时的候选生成与置信度校准。",
      "chunks": [],
      "finished": true
    },
    {
      "summary": "完善了验收定义",
      "content": "",
      "chunks": [],
      "finished": true
    }
  ],
  "source_analysis_msg_id": "1feedfbe-3a22-469c-8d06-9c667a734be9"
}
~~~~

## 消息 3445 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "reasoning_recap",
  "content": "思考了 33s"
}
~~~~

## 消息 3448 / assistant / 分段 1 of 1

~~~~text
第6章 v0.1 我实审完了，结论是：

**`PASS_WITH_TECHNICAL_REVISION`，不用重写，但第7章先不放。**

整体路线是对的：具身主动恢复、候选生成/接受分离、multi-cycle 验证、多源一致性、渐进重接入、SAFE_FAIL、trial 定义和 >95% 统计框架都已经成立。fileciteturn336file0L62-L120

现在主要有 7 个技术收口点，其中最关键的是两个：

第一，**`T_reloc` 和 10s hold 现在有语义重叠风险**。应该改成 `trigger → recovery confirm` 算恢复时间，之后再单独计 10s hold，否则可能把 hold 重复塞进 30s 里。现有公式确实把恢复时间写成“到恢复确认并稳定保持”。fileciteturn336file1L101-L111

第二，正式方案正文里不该出现 Package A / synthetic surrogate 这种内部证据治理语言。第6章正式稿应该只保留设计级边界，内部 Package A 的证明范围继续放 MASTER 证据台账，不放正式比赛正文。

另外我要求把**剧烈运动**和**跨域切换**各自专门写透，现在它们更多还是出现在场景列表里，没有完全进入恢复模型本体。fileciteturn336file0L134-L147

直接把这个发给 **DSH**：

[88｜DSH 第6章 v0.1 → v0.2 技术收口提示词](sandbox:/mnt/data/88_DG-202611_DSH第6章v0.1到v0.2技术收口提示词.md)

完整审查报告：

[87｜MASTER 第6章 v0.1 审查报告](sandbox:/mnt/data/87_DG-202611_MASTER_DSH第6章v0.1审查报告_v1.0.md)

这轮修完，如果 `T_reloc/hold`、GT在线隔离、violent motion、domain switch 这几个点都收掉，第6章就可以冻结，然后正式进入第7章样机与北斗短报文。
~~~~

## 消息 3449 / user / 分段 1 of 1

~~~~text
已完成 **第6章技术收口：v0.1 → v0.2**，七项技术收口全部完成，未扩展到第7章。

## 输出文件

`C:\Users\peng\Desktop\材料\DG-202611_FORMAL_SUBMISSION_REBUILD\DSH_ROUND6\`

| 文件 | 说明 |
|---|---|
| `01_CHAPTER_6_DRAFT_v0.2.md` | 第6章 v0.2 技术收口稿，约 2748 个汉字 |
| `02_CHAPTER_6_MODEL_EQUATION_AND_METRIC_PLAN_v0.2.md` | 公式与指标定义规划 v0.2 |
| `03_CHAPTER_6_FIGURE_TABLE_PLAN_v0.2.md` | 图/表规划 v0.2 |
| `04_CHAPTER_6_CLAIM_REFERENCE_MAP_v0.2.md` | Claim—Reference Map v0.2 |
| `06_DSH_CH6_TECHNICAL_REVISION_CHECK.md` | 技术收口自检 |

## 七项技术收口完成情况

- **T6-01 恢复时间与 hold 拆分**：冻结 `t_trigger`、`t_confirm`、`t_hold_end`；定义 `T_reloc = t_confirm - t_trigger`、`T_hold = t_hold_end - t_confirm`；明确禁止把 10s hold 塞进 T_reloc。
- **T6-02 成功指示补充**：主/伴随成功指示均加入 `recoveryConfirmed=1` 和 `safetyAbort=0`；candidate exhaustion、timeout、final failed 因 recoveryConfirmed=0 自动失败。
- **T6-03 Ground Truth 彻底分离**：冻结 `GROUND_TRUTH_IS_EVALUATION_ONLY`；GT 只用于 e_xy/e_yaw/T_reloc/success 统计和 false recovery 判定，不参与在线 candidate acceptance、hold 或 handoff 决策。
- **T6-04 剧烈运动恢复入口**：新增 violent motion 处理链：停止原导航控制 → 有界减速/运动稳定 → 观测传感器可用性 → 必要运动补偿 → 进入受约束主动搜索；IMU 维持高动态连续性、LiDAR 需畸变抑制、视觉模糊时降权、不适合时不大动作。
- **T6-05 跨域切换策略**：新增专门段落，覆盖描述子分布偏移、几何外观变化、候选置信度失准；单一模态置信度下降不直接接受、提高候选不确定度、使用模型2稳定几何与多源一致性、跨域场景单独统计 SR 与 false recovery。
- **T6-06 内部 Package A 语言删除**：正式正文未出现 Package A / S07/S08 / synthetic surrogate 内部审计语言；改为正式设计句“仿真与代理候选可用于流程级验证，但正式重定位性能必须由真实候选生成链和独立真值评价确定”。
- **T6-07 连续事件 = 一个 trial**：明确同一次连续定位失效事件内 candidate rejection、重新搜索、内部 attempt、恢复动作重启均属同一 trial，不增加分母；只有 trial 成功或失败结束并恢复正常后，新的独立失效事件才建立新 trial。

同时完成：false recovery 定义收紧（内部宣布恢复确认但 truth 超差 → FALSE_RECOVERY=1 → TRIAL=FAIL）；Wilson CI 仍计划报告，但不把 CI 下限>95% 写成官方硬门槛；图6-2 增加剧烈运动稳定入口；图6-4 明确 `t_confirm` 与 `t_hold_end` 不同；表6-3 增加 recoveryConfirmed、safetyAbort、连续事件一 trial、GT evaluation-only。

## 自检结果

```text
T6_01_RELOC_TIME_AND_HOLD_SEPARATED: PASS
T_RELOC_ENDS_AT_CONFIRM: YES
T_HOLD_SEPARATE: YES
T6_02_RECOVERY_CONFIRMED_IN_SUCCESS_INDICATOR: PASS
SAFETY_ABORT_COUNTED_AS_FAILURE: YES
T6_03_GROUND_TRUTH_EVALUATION_ONLY: PASS
GROUND_TRUTH_USED_IN_ONLINE_ACCEPTANCE: NO
T6_04_VIOLENT_MOTION_ENTRY_HANDLING: PASS
T6_05_DOMAIN_SWITCH_STRATEGY: PASS
T6_06_INTERNAL_PACKAGE_A_LANGUAGE_REMOVED: PASS
PACKAGE_A_MENTIONED_IN_FORMAL_CHAPTER: NO
T6_07_CONTINUOUS_INCIDENT_ONE_TRIAL: PASS
MULTIPLE_INTERNAL_TRIGGERS_COUNTED_AS_NEW_TRIALS: NO
FALSE_RECOVERY_DEFINITION_CLEAR: PASS
WILSON_CI_PLANNED: YES
CI_LOWER_BOUND_MADE_OFFICIAL_GATE: NO
95_PERCENT_WRITTEN_AS_CURRENT_RESULT: NO
PROJECT_RESULTS_FABRICATED: NO
CHAPTER_7_WRITTEN: NO
RETURN_TO_MASTER: YES
```

已停止，等待 MASTER 审查。
~~~~

## 消息 3450 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "model_editable_context",
  "model_set_context": ""
}
~~~~

## 消息 3458 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "reasoning_recap",
  "content": "思考了 13s"
}
~~~~

## 消息 3461 / assistant / 分段 1 of 1

~~~~text
第6章 v0.2 这次可以正式过了。

结论：

**`CHAPTER_6_STATUS = PASS`**  
**`CHAPTER_7_AUTHORIZED = YES`**

关键点都收干净了：`T_reloc` 和 `T_hold` 已经真正拆开，成功指示也加入了 `recoveryConfirmed=1` 与 `safetyAbort=0`；GT 被明确冻结成只用于评价，不参与在线 candidate acceptance / hold / handoff。fileciteturn337file3L69-L83 fileciteturn337file3L113-L132

剧烈运动和跨域切换也不再只是“实验场景名”，现在已经真正进入模型本体：violent motion 有稳定入口，domain switch 也有独立的不确定度和多源候选策略。fileciteturn337file4L40-L57 连续失效事件只算一个 trial 的边界也已经冻结。fileciteturn337file4L153-L175

直接把这个发给 **DSH**：

[90｜DSH 正式方案书第7章写作任务（已授权）](sandbox:/mnt/data/90_DG-202611_DSH正式方案书第7章写作任务_AUTHORIZED.md)

最终审查记录：

[89｜MASTER 第6章 v0.2 最终审查与第7章放行记录](sandbox:/mnt/data/89_DG-202611_MASTER_DSH第6章v0.2最终审查与第7章放行记录_v1.0.md)

第7章这次就是**样机 + 北斗短报文集成设计**，我已经把最容易出事的边界写死：`WTRTK980 ≠ 短报文终端`、定位链和通信链必须分开、真实终端未确认时不能编厂商/型号/协议、官方 RSMC/GSMC 能力不能写成项目实测、真正比赛证据必须最终来自真实终端 + 真实卫星服务链路。
~~~~

## 消息 3462 / user / 分段 1 of 1

~~~~text
已完成 **第7章《无人平台样机与北斗短报文通信集成设计》正式写作**，按任务要求输出到：

`C:\Users\peng\Desktop\材料\DG-202611_FORMAL_SUBMISSION_REBUILD\DSH_ROUND7\`

## 生成文件

| 文件 | 说明 |
|---|---|
| `01_CHAPTER_7_DRAFT_v0.1.md` | 第7章正式初稿，约 2611 个汉字 |
| `02_CHAPTER_7_SYSTEM_AND_INTERFACE_PLAN.md` | 系统接口与消息/状态/验证路径规划 |
| `03_CHAPTER_7_FIGURE_TABLE_PLAN.md` | 图7-1～7-5、表7-1～7-6 规划 |
| `04_CHAPTER_7_CLAIM_REFERENCE_MAP.md` | 第7章 Claim—Reference Map |
| `05_DSH_CH7_SELF_CHECK.md` | 第7章自检 |

## 本章核心内容

- **样机总体架构**：计算与控制、定位感知、定位与恢复、应急通信四部分；定位链与通信链架构上明确分开。
- **当前平台工程基础**：Jetson Orin Nano Super、ROS 2 Humble、差速底盘、ZLAC8015D、PEAK PCAN-USB、2D RPLidar、HiPNUC IMU、WTRTK980 GNSS/RTK、ZED 2i；明确 WTRTK980 属于绝对定位观测链，不是 UWB，也不是北斗短报文终端。
- **面向赛题的扩展设计**：UWB、北斗短报文独立通信链、质量感知、三核心模型协同、定位健康/主动恢复、关键事件通信，均按“系统扩展设计”表述，未写成已全部完成实机集成。
- **两条北斗链解耦**：BDS/GNSS/RTK 定位链 vs 北斗短报文通信链；北斗短报文不直接作为模型1的定位观测源。
- **厂商无关终端适配层**：当前未确认终端型号时不写厂商、型号、串口协议、SDK、波特率、Linux驱动、服务授权、发送频率或 ACK 机制；采用“应用层消息 → ROS2逻辑接口 → 终端适配层 → 厂商/标准驱动”分层。
- **关键事件与优先级**：一级应急/求救、二级定位退化/重定位结果、三级任务状态、四级周期心跳；不使用 P0/P1/P2/P3。
- **Rate/Retry/ACK 设计**：服务频度感知、发送间隔约束、有限重试、退避、重复抑制、序列号、ACK/状态跟踪、队列超时；不因上层连续事件违反终端/服务频度。
- **RSMC/GSMC 能力边界**：RSMC 一般频度1次/30秒、最高1次/1秒、成功率≥95%、响应≤1秒、最大14000 bit（约1000汉字）；GSMC 最大560 bit（约40汉字）、分钟级响应；明确为官方系统级能力边界，不是本项目实测。
- **国家标准**：GB/T 44087-2024、GB/T 44086.1-2024、GB/T 44086.2-2024，只使用已核验名称/元数据，不杜撰条文内容。
- **UWB 与短报文关系**：UWB 属于定位感知链扩展，按质量门控接入模型1；与短报文通信链功能独立。
- **工程集成边界**：天线杆臂未正式确认时不编具体安装坐标；无真实电气数据时不虚构电流/功耗/续航/EMC结果。
- **三级验证路径**：软件接口与消息流程验证 → 真实终端硬件在环 → 真实卫星服务端到端；只有第三级才能作为官方“样机集成与验证”的核心项目证据。
- **第9章验证接口**：预留 terminal detected/online、TX accepted、send success/fail、ACK/status、RX correctness、端到端时延、优先级行为、重试行为、消息丢失/失败记录和弱网/应急场景；真实服务可用后再统计项目自身成功率与时延分布，禁止提前填结果。

## 自检结果

```text
CHAPTER_7_COMPLETED: YES
CURRENT_PLATFORM_FACTS_SEPARATED_FROM_FINAL_DESIGN: PASS
WTRTK980_USED_AS_SHORT_MESSAGE_TERMINAL: NO
BDS_POSITIONING_AND_SHORT_MESSAGE_SEPARATED: PASS
SHORT_MESSAGE_IS_POSITIONING_OBSERVATION: NO
VENDOR_MODEL_FABRICATED: NO
UNCONFIRMED_PROTOCOL_FABRICATED: NO
RSMC_GSMC_SEPARATED: PASS
OFFICIAL_SERVICE_LIMIT_WRITTEN_AS_PROJECT_RESULT: NO
EVENT_PRIORITY_CLEAR: PASS
RATE_RETRY_ACK_DESIGN_CLEAR: PASS
TERMINAL_ADAPTER_VENDOR_INDEPENDENT: PASS
UWB_AND_SHORT_MESSAGE_CONFUSED: NO
REAL_TERMINAL_INTEGRATION_CLAIMED_COMPLETE: NO
REAL_SATELLITE_SERVICE_VALIDATION_CLAIMED_COMPLETE: NO
THREE_LEVEL_VALIDATION_PATH_CLEAR: PASS
FINAL_REAL_SERVICE_REQUIRED_FOR_COMPETITION_EVIDENCE: YES
PROJECT_RESULTS_FABRICATED: NO
CHAPTER_8_WRITTEN: NO
RETURN_TO_MASTER: YES
```

已停止，等待 MASTER 审查并放行第8章。
~~~~

## 消息 3471 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "thoughts",
  "thoughts": [
    {
      "summary": "审查章节内容",
      "content": "我正在核对章节与既有资料的一致性，重点检查北斗短报文能力、确认机制、频度指标和终端遥测字段。当前较需要澄清的是公网不可用表述，以及 ACK、心跳字段和服务频度是否属于已验证能力。",
      "chunks": [],
      "finished": true
    },
    {
      "summary": "审查了章节内容",
      "content": "",
      "chunks": [],
      "finished": true
    }
  ],
  "source_analysis_msg_id": "e2a43d47-5291-419b-8456-f6b4b8ed06bb"
}
~~~~

## 消息 3472 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "reasoning_recap",
  "content": "思考了 27s"
}
~~~~

## 消息 3475 / assistant / 分段 1 of 1

~~~~text
第7章 v0.1 我审完了，结论：

**`PASS_WITH_TECHNICAL_REVISION`**，不用重写，但第8章先不放。

整体其实已经很好：WTRTK980 定位链和短报文通信链已经分得很清楚，真实终端没确认前也没有乱编厂商/协议；三级验证路径也对，明确只有真实终端 + 真实卫星服务链才算核心比赛证据。fileciteturn338file0L18-L40 fileciteturn338file0L93-L103

这次主要收 6 个工程细节，最关键的是：**不能默认所有短报文终端都有 ACK**，以及**一级应急消息也不能因为优先级高就绕过服务频度限制**。现在接口规划里把 `ACKED` 当成通用状态，这个需要改成“具体终端支持才有”。fileciteturn338file1L43-L63

另外，RSMC/GSMC 现在虽然分开了，但还要明确：**这只是能力边界对比，不代表最终样机一定同时支持两种服务**。最终用哪一种，要看实际终端和服务授权。fileciteturn338file0L69-L75

直接把这个发给 DSH：

[92｜DSH 第7章 v0.1 → v0.2 技术收口提示词](sandbox:/mnt/data/92_DG-202611_DSH第7章v0.1到v0.2技术收口提示词.md)

完整审查记录：

[91｜MASTER 第7章 v0.1 审查报告](sandbox:/mnt/data/91_DG-202611_MASTER_DSH第7章v0.1审查报告_v1.0.md)

这轮修完，ACK、rate limit、队列抢占、RSMC/GSMC service profile、位置可信性/时效性这几项收掉后，第7章就可以冻结，然后放第8章。
~~~~

[本对话目录](README.md) | [总目录](../../README.md) | [Raw](https://raw.githubusercontent.com/weiyang02520-ops/dg202611-conversation-archive/main/conversations/first/part-085.md) | [上一卷](part-084.md) | [下一卷](part-086.md)
