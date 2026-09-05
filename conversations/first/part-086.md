# 机器人定位挑战赛分析 / 第 086 卷

[本对话目录](README.md) | [总目录](../../README.md) | [Raw](https://raw.githubusercontent.com/weiyang02520-ops/dg202611-conversation-archive/main/conversations/first/part-086.md) | [上一卷](part-085.md) | [下一卷](part-087.md)

## 消息 3476 / user / 分段 1 of 1

~~~~text
已完成 **第7章技术收口：v0.1 → v0.2**，六项工程语义收口全部完成，未扩展到第8章。

## 输出文件

`C:\Users\peng\Desktop\材料\DG-202611_FORMAL_SUBMISSION_REBUILD\DSH_ROUND7\`

| 文件 | 说明 |
|---|---|
| `01_CHAPTER_7_DRAFT_v0.2.md` | 第7章 v0.2 技术收口稿，约 3076 个汉字 |
| `02_CHAPTER_7_SYSTEM_AND_INTERFACE_PLAN_v0.2.md` | 系统与接口规划 v0.2 |
| `03_CHAPTER_7_FIGURE_TABLE_PLAN_v0.2.md` | 图/表规划 v0.2 |
| `04_CHAPTER_7_CLAIM_REFERENCE_MAP_v0.2.md` | Claim—Reference Map v0.2 |
| `06_DSH_CH7_TECHNICAL_REVISION_CHECK.md` | 技术收口自检 |

## 六项工程语义收口完成情况

- **T7-01 ACK / delivery confirmation 改为能力相关**：通用发送状态改为 `QUEUED / RATE_LIMITED / SENDING / SENT / CONFIRMED_IF_SUPPORTED / FAILED / EXPIRED`；明确 `SENT` 表示本地/终端侧已执行发送动作，`CONFIRMED_IF_SUPPORTED` 仅在具体终端/协议提供 ACK、delivery report 或等价确认能力时存在；终端不支持 ACK 时不因缺少 ACK 自动判失败；适配层将 `ACKED/DELIVERED/TERMINAL_CONFIRMED` 等厂商状态映射到统一上层语义；冻结 `TERMINAL_CONFIRMATION_CAPABILITY: CAPABILITY_DEPENDENT`。
- **T7-02 优先级不能绕过 rate limit**：改为“一级应急事件拥有最高调度优先级；发送失败后，在终端能力、服务授权与允许发送频度约束下，于下一合法发送时机尽快重试”；明确优先级不能绕过服务频度限制；冻结 `PRIORITY_DOES_NOT_BYPASS_RATE_LIMIT: YES`。
- **T7-03 队列抢占与 in-flight 发送分离**：允许高优先级消息调到未发送队列前部、低优先级 heartbeat 过期/丢弃、低优先级排队项延后；已进入终端发送流程的消息不能默认被上层中断，是否支持中止取决于具体终端和协议；冻结 `QUEUE_PREEMPTION: SUPPORTED`、`IN_FLIGHT_ABORT: TERMINAL_CAPABILITY_DEPENDENT`。
- **T7-04 RSMC/GSMC service profile 边界**：明确 RSMC/GSMC 用于展示不同北斗短报文服务的系统能力边界；最终样机实际使用哪一种服务由比赛指定/最终选定终端能力与服务授权确定，不假设同一终端或同一链路同时具备两类服务；终端确认后冻结 `ACTIVE_SERVICE_PROFILE`，并据此冻结 payload、interval、retry、receive/confirmation 能力。
- **T7-05 位置可信性与时效性**：消息设计增加 `position_valid / position_source / position_timestamp / position_age / last_trusted_position`；当前可信时发当前可信位置，当前不可信时禁止把 current estimate 冒充可信位置并优先发 last_trusted_position，无可信位置时明确 `POSITION_UNAVAILABLE`，不填 0,0 或陈旧位姿。
- **T7-06 官方能力来源层级备注**：Claim Map 中 RSMC/GSMC 条目增加 `SOURCE_LEVEL_NOTE: OFFICIAL_SYSTEM_CAPABILITY_BOUNDARY_VERIFIED; FINAL_EXACT_TABLE_CITATION_PREFER_OFFICIAL_ORIGINAL_IF_AVAILABLE`；保留 BDS-ASA-1.0、官方白皮书和三项国标，不写内部审计过程。

同时将 heartbeat 中的 battery 字段改为“battery/power state（仅当平台实际提供相应遥测时）”，未写成当前已确认工程接口。

## 自检结果

```text
T7_01_TERMINAL_CONFIRMATION_CAPABILITY_DEPENDENT: PASS
ACK_ASSUMED_ALWAYS_AVAILABLE: NO
T7_02_PRIORITY_DOES_NOT_BYPASS_RATE_LIMIT: PASS
T7_03_QUEUE_PREEMPTION_INFLIGHT_ABORT_SEPARATED: PASS
IN_FLIGHT_ABORT_ASSUMED_SUPPORTED: NO
T7_04_RSMC_GSMC_SERVICE_PROFILE_BOUNDARY: PASS
SIMULTANEOUS_RSMC_GSMC_SUPPORT_ASSUMED: NO
T7_05_POSITION_VALIDITY_AND_AGE_DEFINED: PASS
UNTRUSTED_CURRENT_POSITION_SENT_AS_TRUSTED: NO
POSITION_UNAVAILABLE_STATE_DEFINED: YES
T7_06_OFFICIAL_CAPABILITY_SOURCE_LEVEL_NOTE: PASS
BATTERY_TELEMETRY_WRITTEN_AS_CONFIRMED_CURRENT_FACT: NO
WTRTK980_USED_AS_SHORT_MESSAGE_TERMINAL: NO
SHORT_MESSAGE_IS_POSITIONING_OBSERVATION: NO
OFFICIAL_SERVICE_LIMIT_WRITTEN_AS_PROJECT_RESULT: NO
REAL_TERMINAL_INTEGRATION_CLAIMED_COMPLETE: NO
REAL_SATELLITE_VALIDATION_CLAIMED_COMPLETE: NO
PROJECT_RESULTS_FABRICATED: NO
CHAPTER_8_WRITTEN: NO
RETURN_TO_MASTER: YES
```

已停止，等待 MASTER 审查。
~~~~

## 消息 3484 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "reasoning_recap",
  "content": "思考了 20s"
}
~~~~

## 消息 3486 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "reasoning_recap",
  "content": "已停止思考"
}
~~~~

## 消息 3487 / user / 分段 1 of 1

~~~~text
继续
~~~~

## 消息 3488 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "reasoning_recap",
  "content": "思考了 8s"
}
~~~~

## 消息 3491 / assistant / 分段 1 of 1

~~~~text
继续完成了。第7章 v0.2 可以正式冻结：

**`CHAPTER_7_STATUS = PASS`**  
**`CHAPTER_8_AUTHORIZED = YES`**

ACK 能力相关、优先级不能绕过 rate limit、queue preemption/in-flight abort 分离、RSMC/GSMC service profile、位置可信性/时效性，这几个关键工程边界都已经收干净了。fileciteturn339file0L63-L95 fileciteturn339file4L49-L83

直接把这份发给 **DSH**：

[94｜DSH 正式方案书第8章写作任务（已授权）](sandbox:/mnt/data/94_DG-202611_DSH正式方案书第8章写作任务_AUTHORIZED.md)

最终审查记录：

[93｜MASTER 第7章 v0.2 最终审查与第8章放行记录](sandbox:/mnt/data/93_DG-202611_MASTER_DSH第7章v0.2最终审查与第8章放行记录_v1.0.md)

第8章这次重点是**工程可行性**：把前三个核心模型真正映射到 Jetson/ROS2/传感器/底盘/UWB/短报文上，同时把时间同步、TF/标定、参数一致性、command ownership、故障降级、仿真到实车迁移这些工程问题写透，但**不提前写任何正式性能结果**。
~~~~

## 消息 3492 / user / 分段 1 of 1

~~~~text
TASK\_DIRECTION: DEVELOPMENT\_LINE

svg

```
TASK:
DG202611_GAZEBO_P1_VISUAL_EVIDENCE_FINAL_CLOSEOUT

STATUS:
COMPLETE

P1_GAZEBO_SCREENSHOTS_VALID:            YES  (6, serial capture)
P1_RVIZ_SCREENSHOTS_VALID:              YES  (1, human-verified)
P1_ALL_KEY_SCREENSHOTS_HUMAN_VERIFIED:  YES

CASTER_GEOMETRY_VISUAL_CONFIRMATION:    NOT_CAPTURED
CASTER_VISUAL_NOT_CAPTURED_REASON:      BODY_ENVELOPE_OCCLUSION
MODEL_MODIFIED_FOR_SCREENSHOT:          NO

CASTER_URDF_TOPOLOGY_EVIDENCE:          PASS  (11 links / 10 joints / 6 continuous vs 9/8/2)
CASTER_JOINT_STATE_EVIDENCE:            PASS  (all 6 non-fixed joints on /joint_states)
CASTER_TF_EVIDENCE:                     PASS  (3 caster frames resolve from base_link)
CASTER_ABLATION_EVIDENCE:               PASS  (sphere 0.003622 vs rolling 0.000597/0.000602)

P1_LIDAR_NUMERICAL_GUARD:               PASS  (360 beams, 48 finite, 0 inside body radius)
P1_LIDAR_VISUAL_GUARD:                  PASS  (human-verified: odom fixed frame, all Status Ok, returns clear of body)

P0_CANONICAL_GAZEBO_SCREENSHOT:         NOT_CAPTURED
P0_FUNCTIONAL_GATE_CHANGED:             NO
P0_FUNCTIONAL_STATUS:                   PASS

P1_FUNCTIONAL_REGRESSION:               PASS  (T1-T5, GUARD_CMD_VEL, OVERALL)
P1_STEP_SIZE_ROBUSTNESS:                FAIL

ROOT_CAUSE_STATUS:
CONTACT_SOLVER_OR_CONSTRAINT_CONVERGENCE_STRONGLY_INDICATED_NOT_UNIQUELY_PROVEN

01_CASTER_DESIGN_CREATED:               YES (130 lines)
02_RUNTIME_VALIDATION_CREATED:          YES (211 lines)
03_P0_VS_P1_COMPARISON_CREATED:         YES (85 lines)
04_OPEN_ISSUES_UPDATED:                 YES (90 lines)
EVIDENCE_INVENTORY_COMPLETE:            YES

P0_EVIDENCE_MODIFIED:                   DOCUMENTATION_CORRECTION_ONLY
NEW_P0_RUN:                             NO
PACKAGE_A_WORKSPACE_MODIFIED:           NO   (HEAD cc19289, dirty 0)
SYNTHETIC_RERUN:                        NO   (158M, 41 dirs)
REAL_BASE_CONTROLLER_MODIFIED:          NO
GIT_COMMITTED:                          NO
GIT_PUSHED:                             NO
RESIDUAL_P1_PROCESSES:                  0

EVIDENCE_DIR:
/home/weiyang/results/gazebo/p1/20260830-170019/

RECOMMENDED_GATE:
PARTIAL

NEXT_ACTION:
RETURN_TO_MASTER

```

## KNOWN\_GAPS

**Step-size robustness is the failing dimension.** Both models worsen as the step shrinks — the direction is the same — but halving costs the sphere 3.08x and the rolling caster 71–83x, with onset already at 5.1x by step 0.00075. At 0.0005 the robot slides 1.3–1.5 m in 30 s under zero command, and repeatability degrades from 0.8% to 15%. Reducing step size to gain accuracy is routine, so the next person to try it will hit this.

**My damping hypothesis was refuted by my own diagnostic** — 33x more damping changed drift by 0.07%. Solver iterations 50→150 cut it 4.3x but left it \~19x above the default-step figure. Recorded as strongly indicated, not uniquely proven; no further sweep was run.

**Caster geometry is unphotographable** by construction: a 553.5 mm chassis cylinder sitting on the ground fully encloses 30 mm wheels at −0.0565 m. Three camera attempts failed for the same geometric reason. I did not hide the chassis or switch to wireframe — that would modify the object under validation, so the image would no longer show the model that ran.

**Ablation evidence has a boundary I want stated.** Strictly it shows that replacing sphere-on-fixed-joint reduced drift. It becomes evidence *for rolling contact* only together with the P0 friction ablation, which showed no `mu` can improve drift and yaw fidelity simultaneously — whereas rolling improved drift 6x and odom−GT 27x at once. `STRUCTURAL_CHANGE_EFFECT_EVIDENCED`, not `REAL_WORLD_PHYSICS_PROVEN`.

**The screenshot defect was the fourth of its family** — black frame, empty plot, LiDAR self-occlusion with a green test, wrong-window capture. All 14 P0/P1 frames were RViz at left-panel luma 221.233 against 128.702 for verified Gazebo; four were labelled Gazebo. `xwd` exited 0, sizes were normal, md5s were all distinct. `md5 distinct ⇒ subject correct` is revoked; I had used that inference earlier as grounds for authenticity and it was invalid. Every one of the four was caught only by looking at content.

Carried forward untouched: GNSS antenna offset still `0 0 0` provisional, mass/COM/inertia absent repo-wide, Gazebo Classic 11 EOL, `URDF_PHYSICAL_FIDELITY = SIMPLIFIED`, and `REAL_BASE_KINEMATICS_CONFIG_MISMATCH` at HIGH awaiting a separate real-vehicle audit.
~~~~

## 消息 3493 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "reasoning_recap",
  "content": "思考了 12s"
}
~~~~

## 消息 3496 / assistant / 分段 1 of 1

~~~~text
这次 P1 可以正式关掉了，结论就是：

**`GAZEBO_P1 = PARTIAL`**  
不是失败，但也不能给 PASS。

核心很明确：**rolling caster 方向已经验证有效，默认 step=0.001 下稳定、功能全绿；但 step-size robustness 明确 FAIL。** 所以我不建议现在继续追 solver 了，这个问题先作为已知仿真数值稳定性限制冻结。

下一步开发优先级我改成：**先审实车底盘运动学参数链**。因为那个 `wheel_track 0.25 vs 0.4008` 风险是真正会污染实车控制/里程计/融合的 HIGH 风险，比继续深挖 Gazebo solver 更重要。

MASTER 最终 Gate：

[95｜Gazebo P1 最终 Gate](sandbox:/mnt/data/95_DG-202611_MASTER_Gazebo_P1最终Gate_v1.0.md)

直接发给 Claude 的下一任务：

[96｜Claude 实车底盘运动学配置链专项审计提示词](sandbox:/mnt/data/96_DG-202611_Claude_实车底盘运动学配置链专项审计提示词.md)

这轮只审计，不动实车、不发 `/cmd_vel`、不改 controller、不 commit。先把**到底哪个 backend 在跑、实际 wheel_track/wheel_radius 从哪来、配置有没有真正生效**查清楚，再决定最小修复。
~~~~

## 消息 3497 / user / 分段 1 of 1

~~~~text
已完成 **第8章《工程可行性与样机实现保障》正式写作**，按任务要求输出到：

`C:\Users\peng\Desktop\材料\DG-202611_FORMAL_SUBMISSION_REBUILD\DSH_ROUND8\`

## 生成文件

| 文件 | 说明 |
|---|---|
| `01_CHAPTER_8_DRAFT_v0.1.md` | 第8章正式初稿，约 2487 个汉字 |
| `02_CHAPTER_8_ENGINEERING_FEASIBILITY_PLAN.md` | 工程可行性规划：实时性预算、标定链、风险框架、验证路径 |
| `03_CHAPTER_8_FIGURE_TABLE_PLAN.md` | 图8-1～8-5、表8-1～8-5 规划 |
| `04_CHAPTER_8_CLAIM_REFERENCE_MAP.md` | 第8章 Claim—Reference Map |
| `05_DSH_CH8_SELF_CHECK.md` | 第8章自检 |

## 本章核心内容

- **工程可行性总体分析**：硬件、软件与ROS 2、算力与实时部署、时空一致性、可靠性/降级/安全、集成/维护/验证/扩展六维覆盖。
- **现有平台基础**：Jetson Orin Nano Super、ROS 2 Humble、差速底盘、ZLAC8015D、PEAK PCAN-USB、2D RPLidar、HiPNUC IMU、WTRTK980 GNSS/RTK、ZED 2i；明确“现有平台已具备……工程基础”，未写最终系统已全部集成完成。
- **三核心模型工程映射**：模型1多源连续定位、模型2稳定LiDAR特征、模型3具身主动重定位分别映射到实际工程输入/输出和硬件边界。
- **Jetson部署与实时性**：模块化ROS 2节点、异步处理、callback group/executor/queue/timestamp 管理；按快速状态链、中速环境感知链、事件级恢复/通信链分层；不虚构CPU/GPU/内存/延迟/FPS。
- **时间同步、TF与标定**：保留原始时间戳 + ROS 2 层时序对齐；硬同步/PPS/PTP 仅在有证据时声明；GNSS 天线杆臂明确“仍需实物测量/标定闭环，最终正式实验前必须冻结”。
- **底盘参数一致性**：提出轮径、轮距、坐标约定、速度方向、编码器尺度单一权威来源，最终通过实车标定冻结；未写入内部 0.25/0.4008 bug 细节。
- **UWB/短报文工程可行性**：UWB 按质量门控接入，不写已完成实机融合；北斗短报文承接第7章，强调真实终端+真实服务链路。
- **软件模块化/QoS/数据可追溯**：模块层、消息边界、时间戳、质量状态、失败原因显式；QoS 按设计原则，不复用被证伪的误诊结论；rosbag/配置快照/版本/场景ID等可追溯设计。
- **故障隔离与安全机制**：stale detection、quality gate、降级、safe fail、恢复触发；加入 command ownership：正常导航与主动恢复不能同时争抢 `/cmd_vel`，任何时刻只允许一个控制所有者。
- **仿真→实车迁移**：软件/单元 → 仿真 → 实车三级闭环；明确仿真证据 ≠ 真实性能证据。
- **电气/机械/环境边界**：只写工程要求，不虚构功耗、续航、EMC、温升数值。
- **风险与保障**：风险表覆盖 GNSS、LiDAR、动态/反射、视觉、UWB、标定、时间戳、控制器参数、算力、短报文、通信时延、恢复安全等，每项含检测/缓解/降级/验证。
- **第9章接口**：明确第9章将验证精度、重复率、重定位成功率、部署资源、标定/TF、退化处理、UWB、短报文真实服务、失败案例与可重复性；不提前填结果。

## 自检结果

```text
CHAPTER_8_COMPLETED: YES
CURRENT_BASIS_AND_FINAL_DESIGN_SEPARATED: PASS
THREE_CORE_MODELS_ENGINEERING_MAPPING_CLEAR: PASS
JETSON_RESOURCE_NUMBERS_FABRICATED: NO
TIME_SYNC_DESIGN_CLEAR: PASS
HARD_SYNC_CLAIMED_WITHOUT_EVIDENCE: NO
TF_AND_CALIBRATION_CHAIN_CLEAR: PASS
GNSS_LEVER_ARM_WRITTEN_AS_CONFIRMED: NO
CONTROLLER_PARAMETER_SINGLE_SOURCE_PRINCIPLE: PASS
UWB_WRITTEN_AS_CURRENT_COMPLETED_INTEGRATION: NO
SHORT_MESSAGE_WRITTEN_AS_CURRENT_COMPLETED_INTEGRATION: NO
ROS2_QOS_FALSE_DIAGNOSIS_REUSED: NO
COMMAND_OWNERSHIP_CLEAR: PASS
SIMULATION_WRITTEN_AS_REAL_PERFORMANCE: NO
POWER_OR_EMC_RESULTS_FABRICATED: NO
FORMAL_METRIC_RESULTS_FABRICATED: NO
RISK_MITIGATION_TABLE_COMPLETE: YES
SIM_TO_REAL_PATH_CLEAR: PASS
CHAPTER_9_WRITTEN: NO
RETURN_TO_MASTER: YES
```

已停止，等待 MASTER 审查并放行第9章。
~~~~

## 消息 3505 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "thoughts",
  "thoughts": [
    {
      "summary": "审查第九章",
      "content": "我正在核对章节任务、证据边界和插图逻辑。初步看，需特别收紧“现有系统已覆盖导航与视觉感知”等表述，并避免让 Model3 直接绕过健康检查回灌 Model1。",
      "chunks": [],
      "finished": true
    },
    {
      "summary": "审查了第九章",
      "content": "",
      "chunks": [],
      "finished": true
    }
  ],
  "source_analysis_msg_id": "5b51c82f-0d8f-433b-943d-6c44a8b22e81"
}
~~~~

## 消息 3506 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "reasoning_recap",
  "content": "思考了 29s"
}
~~~~

## 消息 3509 / assistant / 分段 1 of 1

~~~~text
第8章 v0.1 我审完了，结论是：

**`PASS_WITH_TECHNICAL_REVISION`**，不用重写，但第9章先不放。

整体其实已经成型：工程可行性六个维度、三核心模型落地、Jetson部署边界、时间同步/TF/标定、command ownership、仿真→实车、风险矩阵这些都写对了，而且没有把仿真结果冒充实车性能。fileciteturn340file0L31-L49 fileciteturn340file0L69-L101

这次主要收 5 个点，其中最重要的是两个：一是**时钟域**要写清楚，设备时间戳、ROS/主机时间、接收时间不能未经映射直接比较；二是图8-2 里不能让 `Model3 → Model1 recovered pose` 看起来像未经验证就直接灌回融合，必须经过 Health/Recovery gate 和渐进重接入。当前图表规划确实还有这个直连箭头。fileciteturn340file2L30-L45

另外我也让它把“现有导航/视觉功能”的当前事实收紧，避免从“有硬件/有接口”扩大成“已经完整验证”；Jetson 的能力来源也改成明确的 NVIDIA 官方来源，不再只写“厂商公开能力”。fileciteturn340file3L17-L24

直接发给 DSH：

[98｜DSH 第8章 v0.1 → v0.2 技术收口提示词](sandbox:/mnt/data/98_DG-202611_DSH第8章v0.1到v0.2技术收口提示词.md)

完整审查记录：

[97｜MASTER 第8章 v0.1 审查报告](sandbox:/mnt/data/97_DG-202611_MASTER_DSH第8章v0.1审查报告_v1.0.md)

这轮修完，如果这 5 项都收掉，第8章就可以冻结，然后正式进入**第9章——整个方案最关键的“技术性能分析 + 正式实验验证设计”**。
~~~~

## 消息 3510 / user / 分段 1 of 1

~~~~text
已完成 **第8章技术收口：v0.1 → v0.2**，五项技术收口全部完成，未扩展到第9章。

## 输出文件

`C:\Users\peng\Desktop\材料\DG-202611_FORMAL_SUBMISSION_REBUILD\DSH_ROUND8\`

| 文件 | 说明 |
|---|---|
| `01_CHAPTER_8_DRAFT_v0.2.md` | 第8章 v0.2 技术收口稿，约 2703 个汉字 |
| `02_CHAPTER_8_ENGINEERING_FEASIBILITY_PLAN_v0.2.md` | 工程可行性规划 v0.2 |
| `03_CHAPTER_8_FIGURE_TABLE_PLAN_v0.2.md` | 图/表规划 v0.2 |
| `04_CHAPTER_8_CLAIM_REFERENCE_MAP_v0.2.md` | Claim—Reference Map v0.2 |
| `06_DSH_CH8_TECHNICAL_REVISION_CHECK.md` | 技术收口自检 |

## 五项技术收口完成情况

- **T8-01 当前工程基础表述收紧**：删除“已有真实移动、导航、传感、视觉和计算平台”“现有工程链已覆盖……导航与视觉感知等基础功能”等过强表述；改为“现有平台已具备移动底盘、里程计、计算平台及多类传感器接入基础，并具备导航与视觉模块继续集成的软硬件条件”，不把硬件接入推断为功能已验证。
- **T8-02 时钟域语义**：新增 `SOURCE_TIMESTAMP + CLOCK_DOMAIN + HOST_RECEIVE_TIME + CLOCK_OFFSET/LATENCY → COMMON_FUSION_TIMEBASE` 时间同步链；明确不同 clock domain 不能未经映射直接比较、只有 host receipt time 时要记录不确定性/延迟边界、未来 PPS/PTP/hardware sync 可增强；仍不写“已完成硬同步”。
- **T8-03 恢复重接入必须经 Health/Recovery 门控**：图8-2 和正文均改为 `Model3 → verified recovery result / handoff request → Health & Recovery gate → controlled reinitialization / progressive re-entry → Model1`；明确候选/未确认位姿不得直接覆盖模型1。
- **T8-04 参数权威来源细分**：共享物理运动学参数（wheel radius、wheel track、frame convention、velocity sign、encoder scale）采用统一权威来源；仿真专用参数（friction、mass/inertia、contact solver、caster simulation assumptions、numerical physics settings）单独版本化，不与真实运动学参数混为一个权威源。
- **T8-05 Jetson 来源明确为官方厂商来源**：Claim Map 中 Jetson 条目改为 `NVIDIA official Jetson Orin Nano Super product/documentation`，并增加 `SOURCE_LEVEL_NOTE: FINAL_ASSEMBLY_BIND_EXACT_NVIDIA_OFFICIAL_PAGE_OR_DOCUMENT`；正文未新增 TOPS、功耗、GPU 核数、CPU/GPU 占用、FPS 或延迟等未实测数字。

同时完成：`/cmd_vel` 改为“底盘速度命令接口（如 `/cmd_vel`）”；风险表 EXP 编号统一改为“第9章统一实验协议”；ZED 2i 表述改为“视觉/深度观测能力与视觉相对运动信息来源”。

## 自检结果

```text
T8_01_CURRENT_ENGINEERING_BASIS_OVERCLAIM_CLOSED: PASS
UNVERIFIED_NAVIGATION_OR_VISION_WRITTEN_AS_CURRENT_VERIFIED_FUNCTION: NO
T8_02_TIMESTAMP_CLOCK_DOMAIN_EXPLICIT: PASS
DIFFERENT_CLOCK_DOMAINS_COMPARED_WITHOUT_MAPPING: NO
HARD_SYNC_CLAIMED_COMPLETE: NO
T8_03_RECOVERY_REENTRY_HEALTH_GATE: PASS
RECOVERY_REENTRY_BYPASSES_HEALTH_GATE: NO
UNVERIFIED_CANDIDATE_DIRECTLY_OVERWRITES_MODEL1: NO
T8_04_PHYSICAL_KINEMATICS_SINGLE_SOURCE: PASS
SIMULATION_ONLY_PARAMETERS_SEPARATED: YES
T8_05_JETSON_OFFICIAL_VENDOR_SOURCE_DEFINED: PASS
JETSON_UNVERIFIED_PERFORMANCE_NUMBERS_ADDED: NO
CMD_VEL_WRITTEN_AS_ONLY_POSSIBLE_CONTROL_INTERFACE: NO
EXP_IDS_DEFERRED_TO_CHAPTER_9_PROTOCOL: YES
ZED_CURRENT_VO_PERFORMANCE_FABRICATED: NO
UWB_WRITTEN_AS_CURRENT_COMPLETED_INTEGRATION: NO
SHORT_MESSAGE_WRITTEN_AS_CURRENT_COMPLETED_INTEGRATION: NO
SIMULATION_WRITTEN_AS_REAL_PERFORMANCE: NO
FORMAL_METRIC_RESULTS_FABRICATED: NO
CHAPTER_9_WRITTEN: NO
RETURN_TO_MASTER: YES
```

已停止，等待 MASTER 审查。
~~~~

## 消息 3518 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "thoughts",
  "thoughts": [
    {
      "summary": "评估文档质量",
      "content": "我正在核对当前版本的证据链、章节结构和验收逻辑。整体已经具备提交基础；我会重点检查仍可能被评审追问的表述，并据此设计第九章写作任务。",
      "chunks": [],
      "finished": true
    },
    {
      "summary": "评估了文档质量",
      "content": "",
      "chunks": [],
      "finished": true
    }
  ],
  "source_analysis_msg_id": "28e8f9b8-c668-4121-8885-8364075b9989"
}
~~~~

[本对话目录](README.md) | [总目录](../../README.md) | [Raw](https://raw.githubusercontent.com/weiyang02520-ops/dg202611-conversation-archive/main/conversations/first/part-086.md) | [上一卷](part-085.md) | [下一卷](part-087.md)
