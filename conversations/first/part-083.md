# 机器人定位挑战赛分析 / 第 083 卷

[本对话目录](README.md) | [总目录](../../README.md) | [Raw](https://raw.githubusercontent.com/weiyang02520-ops/dg202611-conversation-archive/main/conversations/first/part-083.md) | [上一卷](part-082.md) | [下一卷](part-084.md)

## 消息 3283 / user / 分段 1 of 1

~~~~text
已完成 **第5章《动态目标与异常反射条件下的LiDAR稳定特征提取模型》正式写作**，按任务要求输出到：

`C:\Users\peng\Desktop\材料\DG-202611_FORMAL_SUBMISSION_REBUILD\DSH_ROUND5\`

## 生成文件

| 文件 | 说明 |
|---|---|
| `01_CHAPTER_5_DRAFT_v0.1.md` | 第5章正式初稿，约 2181 个汉字 |
| `02_CHAPTER_5_MODEL_EQUATION_AND_METRIC_PLAN.md` | 模型公式与指标定义规划 |
| `03_CHAPTER_5_FIGURE_TABLE_PLAN.md` | 图5-1～5-4、表5-1～5-4 规划 |
| `04_CHAPTER_5_CLAIM_REFERENCE_MAP.md` | 第5章 Claim—Reference Map |
| `05_DSH_CH5_SELF_CHECK.md` | 第5章自检 |

## 本章核心内容

- **硬件边界**：以 2D RPLidar / 2D LaserScan 为主体；不声称当前平台具备 FMCW LiDAR Doppler 速度、3D point cloud plane feature 或完整 3D semantic LiDAR；intensity/quality 仅作为辅助判据。
- **问题定义**：覆盖动态目标、遮挡变化、异常反射/瞬态错误量测、几何退化/重复结构四类干扰；目标不是“删到最少点”，而是保留有效静态信息并提高稳定特征可重复性。
- **观测表示与预处理**：给出 `S_k = {(ρ_j,θ_j)}` 与激光系坐标转换，预处理含无效range、离群、局部突变和运动补偿；不写未冻结阈值。
- **在线运动补偿与正式评价分离**：在线可用 IMU/Wheel/模型1位姿；正式 repeatability 评价必须使用独立参考变换，避免循环证明。
- **动态目标检测**：几何聚类 + 时序一致性，给出残差 `e_j` 和动态置信度 `D_j=f_D(...)`；明确“未知”不默认静态可信。
- **异常反射抑制**：多维度判断孤立跳点、局部突变、邻域不一致、无时间持续性；`A_j=f_A(...)`，`I_j` 仅设备支持时使用。
- **遮挡/新可见区域区分**：明确“当前帧没匹配到上一帧”不等于动态。
- **稳定特征提取**：采用“点特征 + 线结构特征”轻量组合，适合 2D LiDAR 与 Jetson；不把 3D plane/voxel 作为当前核心。
- **稳定评分与不确定度**：给出 `s_f=f_s(G,T,R,D,A,U)` 和 `Σ_f=f_Σ(...)`，输出稳定/疑似/拒绝与特征协方差。
- **模型2输出**：stable features、type、uncertainty、dynamic mask、anomalous mask、stable count、retention ratio、geometric coverage、repeatability quality indicators。
- **官方 repeatability 定义**：正式冻结 `R_min=M/min(N1,N2)`，伴随 `R_avg=2M/(N1+N2)`，池化 `R_pool=ΣM/Σmin(N1,N2)`；共同可见区、同类型特征、一对一匹配、预冻结 `d_rep`；分母为 0 记 `INVALID_PAIR`，禁止按 100% 计算。
- **防作弊**：必须同时报告 RAW/FILTERED/STABLE/RETENTION/VALID/INVALID，并明确“高重复率但低保留率”不能直接认为模型优秀。
- **算法5-1**：给出模型2完整处理链高层流程。
- **与模型1/模型3接口**：模型2输出用于模型1 的 LiDAR quality / directional information，用于模型3 的候选几何与置信度。
- **失败边界**：动态占比过高、静态几何过少、遮挡严重、保留率低时降低 LiDAR quality，必要时触发模型3。

## 自检结果

```text
CHAPTER_5_COMPLETED: YES
2D_LIDAR_HARDWARE_BOUNDARY: PASS
DYNAMIC_OCCLUSION_NEW_VISIBILITY_SEPARATED: PASS
ANOMALOUS_REFLECTION_MODEL_CLEAR: PASS
STABLE_FEATURE_MODEL_CLEAR: PASS
FEATURE_UNCERTAINTY_PRESENT: YES
ONLINE_AND_EVALUATION_TRANSFORM_SEPARATED: PASS
R_MIN_DEFINED: YES
R_AVG_DEFINED: YES
R_POOL_DEFINED: YES
ZERO_DENOMINATOR_MARKED_INVALID: YES
ONE_TO_ONE_MATCHING_REQUIRED: YES
COMMON_VISIBLE_REGION_REQUIRED: YES
D_REP_PRE_FROZEN: YES
RETENTION_RATIO_REPORTED: YES
TEMPORAL_MATCH_RATIO_USED_AS_OFFICIAL_REPEATABILITY: NO
FMCW_DOPPLER_CLAIMED_FOR_CURRENT_RPLIDAR: NO
95_PERCENT_WRITTEN_AS_CURRENT_RESULT: NO
MODEL1_INTERFACE_CLEAR: PASS
MODEL3_INTERFACE_CLEAR: PASS
FAILURE_BOUNDARY_PRESENT: YES
PROJECT_RESULTS_FABRICATED: NO
CHAPTER_6_WRITTEN: NO
RETURN_TO_MASTER: YES
```

已停止，等待 MASTER 审查并放行第6章。
~~~~

## 消息 3311 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "thoughts",
  "thoughts": [
    {
      "summary": "核对审查材料",
      "content": "我现在只看到完成报告，没有看到第5章的五个实际文件，因此还不能进行正式MASTER审查或给出Gate结论。请把文件上传到当前对话，我会直接按既定框架继续，不要求重复说明背景。",
      "chunks": [],
      "finished": true
    },
    {
      "summary": "核对了审查材料",
      "content": "",
      "chunks": [],
      "finished": true
    }
  ],
  "source_analysis_msg_id": "c880df40-00e1-4d96-a9c7-530f188198b9"
}
~~~~

## 消息 3344 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "thoughts",
  "thoughts": [
    {
      "summary": "无法进行正式审查：第5章文件尚未上传",
      "content": "",
      "chunks": [],
      "finished": true
    }
  ],
  "source_analysis_msg_id": "e15ad950-896f-4ace-af9a-5c87c7547624"
}
~~~~

## 消息 3345 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "reasoning_recap",
  "content": "思考了 1m 10s"
}
~~~~

## 消息 3346 / assistant / 分段 1 of 1

~~~~text
这次我**先不放第6章**，不是因为第5章内容看起来有问题，而是因为我现在只收到这份“完成报告”，**第5章那 5 个实际文件没有上传到当前对话**。我检查了当前会话文件，最新能读取到的还是 ZCode 材料和第4章 v0.3。fileciteturn333file0L1-L6 fileciteturn333file3L20-L24 fileciteturn333file7L44-L48

所以我不能只根据 DSH 自己的 `PASS` 自检就给：

`CHAPTER_5_STATUS = PASS`

`CHAPTER_6_AUTHORIZED = YES`

把这 5 个文件直接拖进来就行，不用再发说明：

`01_CHAPTER_5_DRAFT_v0.1.md`  
`02_CHAPTER_5_MODEL_EQUATION_AND_METRIC_PLAN.md`  
`03_CHAPTER_5_FIGURE_TABLE_PLAN.md`  
`04_CHAPTER_5_CLAIM_REFERENCE_MAP.md`  
`05_DSH_CH5_SELF_CHECK.md`

上传后我直接做 MASTER 实审，重点查 **repeatability 定义有没有数学漏洞、共同可见区有没有循环证明、动态/遮挡有没有误分类风险、retention 防作弊是否真的写进正文、以及 2D RPLidar 边界有没有被偷偷突破**。通过我就直接给第6章提示词。
~~~~

## 消息 3347 / user / 分段 1 of 1

~~~~text

~~~~

## 消息 3355 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "thoughts",
  "thoughts": [
    {
      "summary": "审查技术指标",
      "content": "我正在检查文件中的数学定义、评测口径和动态剔除流程。主要疑点包括公共可见区域、因果时间窗口，以及重复率统计是否会因视场变化或候选数量差异而失真。",
      "chunks": [],
      "finished": true
    },
    {
      "summary": "审查技术指标",
      "content": "",
      "chunks": [],
      "finished": true
    }
  ],
  "source_analysis_msg_id": "3fe1dc17-996e-4341-8129-b07f6d7b8900"
}
~~~~

## 消息 3356 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "thoughts",
  "thoughts": [
    {
      "summary": "排除评测循环",
      "content": "我发现一个更关键的问题：若特征稳定性判定直接使用被评测相邻帧的匹配结果，重复率可能被循环性地抬高。需要将特征选择与正式评测匹配过程分离，或采用评测帧之前的历史信息。",
      "chunks": [],
      "finished": true
    },
    {
      "summary": "排除了评测循环",
      "content": "",
      "chunks": [],
      "finished": true
    }
  ],
  "source_analysis_msg_id": "a04a133c-6bb2-49a8-9f8b-83d98a86ede0"
}
~~~~

## 消息 3357 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "reasoning_recap",
  "content": "思考了 30s"
}
~~~~

## 消息 3360 / assistant / 分段 1 of 1

~~~~text
这次我实审完了，第5章结论是：

**`PASS_WITH_TECHNICAL_REVISION`，不用重写，但第6章先不放。**

主体其实已经很好了。2D RPLidar 边界守住了，动态/遮挡/新可见区域也分开了，正式 repeatability 已经有 `R_min / R_avg / R_pool`、共同可见区、一对一匹配、`INVALID_PAIR` 和 retention 防作弊。fileciteturn334file0L168-L237 图表和接口设计也基本完整。fileciteturn334file2L76-L95

现在最关键的问题只有一个核心：**不能让“稳定特征是因为它匹配成功”然后再拿这些特征去算“匹配成功率”**。当前稳定评分里用了时间持续性和跨帧残差，而正式 repeatability 又基于相邻帧匹配，所以必须把：

**特征选择 → 冻结 F1/F2 → 独立评价**

这三步彻底分开，否则有循环证明风险。fileciteturn334file0L112-L129

另外我要求把“共同可见区域”正式定义出来，不能看完哪些点匹配成功后再反推共同区域；retention 也要和 repeatability 使用同一个评价域；在线多周期判断必须保持因果，不能偷偷用未来帧提高当前结果。

直接把这个发给 **DSH**：

[79｜DSH 第5章 v0.1 → v0.2 技术收口提示词](sandbox:/mnt/data/dg202611_formal_rebuild_next/79_DG-202611_DSH第5章v0.1到v0.2技术收口提示词.md)

完整审查报告：

[78｜MASTER 第5章 v0.1 审查报告](sandbox:/mnt/data/dg202611_formal_rebuild_next/78_DG-202611_MASTER_DSH第5章v0.1审查报告_v1.0.md)

这轮修完，如果“特征筛选和正式评价彻底独立”这一点过了，第5章就可以直接冻结，然后进入第6章主动重定位模型。
~~~~

## 消息 3361 / user / 分段 1 of 1

~~~~text
TASK\_DIRECTION: DEVELOPMENT\_LINE

svg

```
TASK:
DG202611_PACKAGE_A_BOOKKEEPING_AND_FREEZE_FIX

STATUS:
COMPLETE — stopped at MASTER gate

RERUN_PERFORMED:
NO

FREEZE_ANCHOR_COMMIT:
02af370d99ece040605ec1bee585866e5760f9c6
(follow-up cc19289 records the SHA in the freeze report)

PUSHED:
NO — see reason below

FILES_COMMITTED:
35 in 02af370 (10 modified, 25 added), all under src/dg_synthetic_validation/
and docs/dg202611/. Run artifacts are outside the repository and were not
committed.

S06_CANONICAL:
S06_closed_loop_active_scan_recovery_motion_20260829-165043  (399 samples)

S06_SUPERSEDED_FINALS:
S06_..._20260829-161050  (392 samples, PASS, retained on the record)

TOTAL_RUNS:
40 result.json / 41 dirs

PASS_RUNS: 31        FAIL_RUNS: 9

FAILURE_HISTORY_CREATED:            YES  (330 lines)
CRITERIA_EVOLUTION_DISCLOSED:       YES
NON_DISCRIMINATING_CHECKS_FIXED:    YES
MULTIFRAME_WORDING_FIXED:           YES
GROUND_TRUTH_SURROGATE_BOUNDARY_FIXED: YES
SAFE_HANDOFF_BOUNDARY_FIXED:        YES
NAVIGATION_RESUMED_CLAIM_PRESENT:   NO
QOS_MISDIAGNOSIS_CLOSED:            YES
REAL_S05_TF_ISSUE_RECORDED:         YES

PACKAGE_A_RUNTIME_COMPLETE:  YES
COMPETITION_METRIC_EVIDENCE: NO
PACKAGE_A_FREEZE_READY:      YES

GIT_COMMITTED: YES        GIT_PUSHED: NO
REAL_HARDWARE_ACCESSED: NO

```

## Why not pushed

The remote is readable anonymously (`refs/heads/main` lists fine) but there is **no credential helper configured** and `dg202611-synthetic-validation` does **not exist on the remote**. A push would create a new remote branch and needs write credentials I don't hold, in a non-interactive session. Not attempted rather than left half-done. No force push was considered.

## Corrections where I was wrong

Three of my own earlier claims were reversed this round, and the record now reflects the verified versions:

The **QoS issue was my misdiagnosis twice over**. I had the direction backwards — `initial_pose_qos_profile()` (TRANSIENT\_LOCAL) is on the *publisher*, the subscription is default VOLATILE, which is compatible. I also misquoted the warning text, and the emitter turned out to be `rosbag2_recorder`, a recording-side subscription, so it was never evidence of loss on a supervisor data path. Closed `CLOSED_AS_MISDIAGNOSIS`, with the wrong diagnosis kept visible as superseded.

**Check totals were inflated** and I had published them. `12/12` and `15/15` each contained one check hardcoded `True`. Fixed in code, and recomputed against real `samples.csv`: S05 11+1, S06 14+1, S07 13+1, S08 12+1, and **S01 0 non-discriminating** because there the check is genuine.

I also wrote a wrong appendix table into the inventory (attributing 12/12 to S01 from a synthetic test row) and corrected it after recomputing from the canonical run — S01 actually recorded 9 checks.

## The criteria-evolution finding is stronger than the audit stated

S01 **passed at** **`618122f`****, then regressed to FAIL at** **`1a3fb4b`** — a criteria commit made it worse. It first passed at `fd2c60d`, and the two commits immediately before it touch only `scenario_runner.py`, i.e. test-rig start-up rather than criteria. So the loosened criteria were real and consequential but **not sufficient**; the last step to green removed a start-up transient at source. Neither "pre-registered" nor "loosened until green" describes what happened.

## KNOWN\_GAPS

`REAL_BASE_KINEMATICS_CONFIG_MISMATCH` (severity HIGH) needs a separate real-vehicle audit — `base_controller` never receives `base_kinematics.yaml`, runs on `wheel_track = 0.25` against the measured `0.4008`, and declares no `wheel_radius`. `FAILED`/`MANUAL_REQUIRED` branches have no synthetic evidence at all. S05 GUI capture and S06 transient states are `NOT_CAPTURED`, explicitly not `NOT_OBSERVED`. `/dg/fusion/pose` has no RViz path. GNSS antenna offset is still a `0 0 0` placeholder.

Final state verified: tree clean, 33 tests pass, 41 run dirs and 158M evidence untouched, **0 files changed under** **`src/ylhb_base`**.

svg

```
NEXT_ACTION:
RETURN_TO_MASTER
```
~~~~

## 消息 3362 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "thoughts",
  "thoughts": [
    {
      "summary": "评估交付状态",
      "content": "我正在把这份报告视为 Package A 的冻结申请：完成项、验证结果和回滚边界都较清楚。当前唯一缺口是提交尚未推送，因此我会先要求完成远端同步，再进入 MASTER Gate。",
      "chunks": [],
      "finished": true
    },
    {
      "summary": "评估了交付状态",
      "content": "",
      "chunks": [],
      "finished": true
    }
  ],
  "source_analysis_msg_id": "315828cc-7086-4174-965b-4a0b3d0b8bd5"
}
~~~~

## 消息 3365 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "thoughts",
  "thoughts": [
    {
      "summary": "检查 GitHub 连接器与发布授权",
      "content": "",
      "chunks": [],
      "finished": true
    }
  ],
  "source_analysis_msg_id": "1b0f4f72-e896-4322-ae91-da6e349f0ebf"
}
~~~~

## 消息 3368 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "thoughts",
  "thoughts": [
    {
      "summary": "确认冻结闸门",
      "content": "我判定 Package A 本地冻结通过：核心运行验证完成，工作区干净，回滚锚点明确。远端尚未同步属于交付备注，不阻塞本地 Gate。下一步授权 Gazebo P1；真实底盘参数审计另行进行。",
      "chunks": [],
      "finished": true
    },
    {
      "summary": "确认冻结闸门",
      "content": "",
      "chunks": [],
      "finished": true
    }
  ],
  "source_analysis_msg_id": "4adf0165-3caf-4cbc-b286-529d6324adf9"
}
~~~~

## 消息 3369 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "thoughts",
  "thoughts": [
    {
      "summary": "读取并预览提示词文件内容",
      "content": "",
      "chunks": [],
      "finished": true
    }
  ],
  "source_analysis_msg_id": "cb53dd25-3217-4a7e-99f1-ee41e21bac0d"
}
~~~~

## 消息 3370 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "reasoning_recap",
  "content": "思考了 1m 35s"
}
~~~~

## 消息 3375 / assistant / 分段 1 of 1

~~~~text
Package A 这次我正式判：

**`PASS_WITH_REMOTE_BACKUP_NOTE`，可以冻结，不用重跑。**

最关键的都已经收掉了：40 个 `result.json` / 41 个目录 / 31 PASS / 9 FAIL 已统一，S06 canonical 也定了；失败历史、判据演进、hardcoded true 膨胀计数、S07/S08“不是3次独立 matcher”、ground-truth surrogate、safe handoff、QoS误诊这些全部已经回到正确口径。

没 push **不阻塞 freeze**，因为之前任务本来就允许“无法安全 push 时只 commit 并报告原因”。现在冻结锚点是：

`02af370d99ece040605ec1bee585866e5760f9c6`

`cc19289` 只是后续把这个 SHA 写进 freeze report 的记录提交。以后有 GitHub 凭据时补推即可，**不要 force push**。

正式 Gate：

[80｜MASTER Package A 最终冻结 Gate](sandbox:/mnt/data/dg202611_formal_rebuild_next/80_DG-202611_MASTER_PackageA最终冻结Gate_v1.0.md)

现在 DEVELOPMENT_LINE 可以继续 **Gazebo P1**。直接把这份发给 Claude：

[81｜Claude Gazebo P1 脚轮与稳定性验证提示词](sandbox:/mnt/data/dg202611_formal_rebuild_next/81_DG-202611_Claude_Gazebo_P1脚轮与稳定性验证提示词.md)

P1 这次只解决 P0 已经抓到的 **rolling caster / idle drift 仿真结构问题**，不会碰实车 `base_controller`，也不会碰已经冻结的 Package A。`wheel_track 0.25 vs 0.4008` 那个 HIGH 风险继续留到单独的实车配置核验任务。memcite
~~~~

## 消息 3376 / user / 分段 1 of 1

~~~~text
TASK\_DIRECTION: DEVELOPMENT\_LINE

漂移降了 **6 倍**,两次复现差 0.8%。step-size 敏感性和球体对照组在跑。

## 已测数据

| 变体idle drift30 s 位移 |                  |           |
| ------------------- | ---------------- | --------- |
| P0 sphere mu=0.3    | 0.0036 m/s       | \~0.108 m |
| **P1-A rolling**    | **0.000597 m/s** | 0.0179 m  |
| **P1-B rolling**    | **0.000602 m/s** | 0.0181 m  |

两次独立 fresh launch 差 0.8%,复现性成立。yaw 漂移 4.2–4.3e-05 rad/s,同样一致。

## 正在跑的四个变体,以及为什么需要它们

按第 6 节要求,不调参凑绿,每个变体都记录。这四个不是重复测量,各有判定目的:

**C1 (rolling, step 0.0005)** 和 **C2 (rolling, step 0.002)** —— 决定性实验。P0 的漂移有个明确签名:步长减半、累计漂移翻倍,这是"每步固定泄漏"的特征。如果 P1 的漂移**不随步长这样缩放**,就说明剩余的 0.0006 m/s 不再由 contact leak 主导,而是别的东西(数值噪声、接触柔度的稳态沉降)。这是本轮技术结论的核心。

**D (sphere, step 0.001)** 和 **E (sphere, step 0.0005)** —— 对照组。我保留了球体变体正是为此:在**同一套 rig、同一世界、同一测量脚本**下重测 P0 模型。这样"6 倍改善"才是同条件对比,而不是拿今天的 P1 去比昨天不同代码路径下的 P0 数字。如果 D 复现出 \~0.0036,说明改善确实来自结构改动;如果 D 明显偏离,那说明其他改动(joint state publisher、`publish_wheel_tf` 关闭)也有影响,必须如实拆开。

## 一件我暂时无法断言的事

`odom_drift_mps` 是 6.9e-05,而 ground truth 是 0.000597 —— 相差约 8.7 倍。也就是说轮式编码器几乎没察觉这个漂移,车身却在动。这符合"车身被接触力推着滑移而驱动轮没转"的图像,但我还没有独立证据排除别的解释,所以先记为观测,不写成结论。

这个差值同样**不能读成里程计精度** —— 它衡量的是我自己假设的接触模型,`ODOM_GT_DIFFERENCE_ROLE: SIMULATION_MODEL_DIAGNOSTIC`。

P0 证据目录未触碰,Package A 仍冻结在 `cc19289`,未修改 `src/ylhb_base`。
~~~~

## 消息 3378 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "model_editable_context",
  "model_set_context": ""
}
~~~~

## 消息 3379 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "reasoning_recap",
  "content": "思考了 8s"
}
~~~~

## 消息 3380 / assistant / 分段 1 of 1

~~~~text
这组数据很有价值，但**现在还不能判 P1 PASS**，因为最关键的因果验证 C1/C2/D/E 还没回来。

目前可以先冻结两个中间结论：rolling caster 两次 fresh launch 分别 `0.000597 / 0.000602 m/s`，相对 P0 的 `0.0036 m/s` 已经约 **6 倍下降**，而且两次仅差约 **0.8%**，所以“改善可复现”这一点已经很强。你对 `odom_drift_mps` 和 ground truth 漂移差异的处理也对——**先记观测，不下原因结论，更不能叫里程计精度。**

现在继续把 C1/C2/D/E 全跑完，不要改任何参数，也不要提前挑最好结果。MASTER 最终看四件事：**同 rig 的 sphere 对照能否复现 P0；rolling 是否持续显著优于 sphere；step-size 改变后 rolling 是否摆脱 P0 那种“步长减半、漂移翻倍”的 per-step leak 签名；forward / rotate / LiDAR / IMU / GNSS / TF 是否没有被 rolling caster 修坏。**

尤其 D/E 很重要：如果 sphere 对照重新得到接近 `0.0036 m/s`，那基本可以把这次改善主要归因到 caster 结构改动；如果 D 明显不同，就必须继续拆分 `publish_wheel_tf`、joint-state 路径等伴随变化，不能直接写“rolling caster 带来6倍改善”。

所以当前状态我判：

`P1_INTERIM_STATUS = PROMISING / NOT_YET_GATED`

**Claude继续跑，不需要新提示词。** 等 C1/C2/D/E + 最终功能回归一起回来，我直接做 Gazebo P1 Gate。
~~~~

## 消息 3381 / user / 分段 1 of 1

~~~~text
TASK\_DIRECTION: DEVELOPMENT\_LINE

在等确认结果,先把已经确定的结论和那个坏消息讲清楚,因为它决定 P1 该不该被判 PASS。

## 对照组让比较站得住

`P1-D`（sphere,step 0.001,同 rig 同世界同脚本）测出 **0.003622 m/s**,精确复现 P0 的 0.0036。这一步很重要 —— 没有它,"改善 6 倍"就只是拿今天的代码比昨天不同代码路径下的旧数字。现在它是同条件对照。

`P1-E`（sphere,step 0.0005）测出 0.011173,是 0.001 下的 **3.08 倍**。P0 当时观察到的"步长减半漂移翻倍"签名不仅复现,而且比记录的更强。所以球体模型的诊断是对的:每步固定泄漏。

保留球体变体这个决定在这里回本了。如果我按常规做法直接改掉脚轮,这两个对照都做不出来。

## 坏消息\:rolling 在小步长下不稳定

rolling 的漂移随步长**单调递减**:0.0498（0.0005）→ 0.000597（0.001）→ 0.000227（0.002)。

这个方向是反的。per-step leak 的签名是步数越多累计越多,所以减小步长应该变差 —— 球体正是如此。而 rolling 减小步长变差了 83 倍,增大步长反而变好,这说明剩余漂移**不再是接触泄漏主导**,而是某种在小步长下被激发的数值不稳定。

我的怀疑是那两个被动关节:阻尼给到 0.0015,非常轻,轻阻尼自由关节在小步长下更容易被求解器泵入能量而抖振。但这只是假设,我还没有独立证据,不写成结论。

## 这对 P1 判定的意义

按第 8 节,Gate 要看"step-size sensitivity 是否明显改善"。诚实的回答是:**没有改善,是性质变了** —— 从"可预测的每步泄漏"变成"默认步长下好得多,但小步长下灾难性恶化"。

这不能被包装成 PASS。默认步长 0.001 下 P1 确实比 P0 好 6 倍且复现性 0.8%,但一个在步长减半时恶化 83 倍的模型是脆的,而"减小步长以提高精度"是仿真里最常见的操作。下一个人这么做就会踩坑。

按第 6 节我不会反复调阻尼直到 0.0005 也变绿再只留好结果。正确做法是把这个脆性作为 P1 的主要发现之一交给 MASTER,附完整变体表,由 MASTER 决定判 PASS / PARTIAL / FIX\_REQUIRED。

如果要诊断机制,我会把阻尼变体明确标为**诊断实验**并记录全部结果,而不是当作"选定配置"。

确认跑完后我做功能回归（spawn/forward/rotate/LiDAR/IMU/GNSS/TF/odom/GT)、LiDAR 双重 guard、证据归档和四份文档。

**PACKAGE\_A\_WORKSPACE\_MODIFIED: NO** · P0 证据未触碰 · **GIT\_COMMITTED: NO**
~~~~

## 消息 3383 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "thoughts",
  "thoughts": [
    {
      "summary": "评估当前证据",
      "content": "这份中期报告已经足以把候选定为 `FIX_REQUIRED`，但我先让既定回归和文档收尾，不立即调参。当前证据支持步长敏感性显著下降却仍存在关节相关失稳；之后再单独诊断阻尼、求解器或约束配置。",
      "chunks": [],
      "finished": true
    }
  ],
  "source_analysis_msg_id": "cea315db-7b1d-4193-b28e-e773597cacb2"
}
~~~~

[本对话目录](README.md) | [总目录](../../README.md) | [Raw](https://raw.githubusercontent.com/weiyang02520-ops/dg202611-conversation-archive/main/conversations/first/part-083.md) | [上一卷](part-082.md) | [下一卷](part-084.md)
