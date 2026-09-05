# 机器人定位挑战赛分析 / 第 055 卷

[本对话目录](README.md) | [总目录](../../README.md) | [Raw](https://raw.githubusercontent.com/weiyang02520-ops/dg202611-conversation-archive/main/conversations/first/part-055.md) | [上一卷](part-054.md) | [下一卷](part-056.md)

## 消息 2012 / assistant / 分段 2 of 2

~~~~text
==============================

Stage D 不是新一轮“数量竞赛”。

原则：

已有 20 篇核心论文优先全文精读。

只有以下情况才补充：

1. 现有核心论文没有全文。
2. 缺某关键性能指标。
3. 缺 UWB / Radar / Event / Fusion 关键证据。
4. 找到明显更高质量、更直接相关论文。
5. 需要验证冲突结论。

新增核心论文原则上控制在：

10–20 篇以内。

不要再无目的扩大到数百篇。

==================================================
十九、输出目录
==================================================

在：

C:\Users\peng\Desktop\材料\DG-202611比赛材料文档\

下创建：

STAGE_D_核心论文全文与性能证据_v1.0

建议至少包含：

01_STAGE_D_FULLTEXT_CORE_PAPER_CARDS_v1.0.md

02_STAGE_D_PERFORMANCE_BENCHMARK_MATRIX_v1.0.csv

03_STAGE_D_COMPETITION_METRIC_SAFE_BOUNDARIES_v1.0.md

04_STAGE_D_UWB_QUALITY_REJECTION_EVIDENCE_v1.0.md

05_STAGE_D_MMWAVE_RADAR_POSITIONING_EVIDENCE_v1.0.md

06_STAGE_D_EVENT_ZED_TEMPORAL_SYNC_EVIDENCE_v1.0.md

07_STAGE_D_ADAPTIVE_FAULT_AWARE_FUSION_EVIDENCE_v1.0.md

08_STAGE_D_RELOCALIZATION_EVIDENCE_v1.0.md

09_STAGE_D_SAFE_CLAIM_MATRIX_v1.0.md

10_STAGE_D_PROJECT_TECHNOLOGY_STATUS_MATRIX_v1.0.md

11_STAGE_D_UNRESOLVED_EVIDENCE_GAPS_v1.0.md

12_STAGE_D_SEARCH_AND_VERIFICATION_LOG_v1.0.md

STAGE_D_references.bib

全部优先 Markdown / CSV / BibTeX。

不要渲染大型 DOCX/PDF。

==================================================
二十、Stage D Gate
==================================================

Stage D 不要求所有20篇都必须成功取得全文。

但最终必须真实报告：

CORE_PAPERS_TARGETED

FULL_TEXT_OBTAINED

FULL_TEXT_EXPERIMENT_VERIFIED

ABSTRACT_ONLY

KEY_PERFORMANCE_VALUES_EXTRACTED

SECONDARY_VERIFIED_VALUES

UWB_FULLTEXT_PAPERS

RADAR_FULLTEXT_PAPERS

EVENT_FULLTEXT_PAPERS

FUSION_FULLTEXT_PAPERS

RELOCALIZATION_FULLTEXT_PAPERS

COMPETITION_METRICS_WITH_DIRECT_LITERATURE_SUPPORT

COMPETITION_METRICS_WITH_ONLY_INDIRECT_SUPPORT

UNRESOLVED_METRICS

==================================================
二十一、最终报告格式
==================================================

最终回复第一行必须明确：

TASK_DIRECTION: DOCUMENT_LINE

然后报告：

STAGE_D_STATUS:
PASS / PASS_WITH_LIMITATIONS / PARTIAL / BLOCKED

FORMAL_BODY_MODIFIED:
NO

DEVELOPMENT_LINE_CONTACTED:
NO

DSH_USED:
NO

CORE_PAPERS_TARGETED:

FULL_TEXT_OBTAINED:

FULL_TEXT_EXPERIMENT_VERIFIED:

ABSTRACT_ONLY:

NEW_HIGH_VALUE_PAPERS_ADDED:

PERFORMANCE_VALUES_EXTRACTED:

PERFORMANCE_VALUES_SECONDARY_VERIFIED:

UWB_CONCLUSION:

MMWAVE_CONCLUSION:

EVENT_ZED_SYNC_CONCLUSION:

ADAPTIVE_FUSION_CONCLUSION:

RELOCALIZATION_CONCLUSION:

20CM_HORIZONTAL_EVIDENCE:

20CM_VERTICAL_EVIDENCE:

95_PERCENT_FEATURE_REPEATABILITY_EVIDENCE:

95_PERCENT_RELOCALIZATION_EVIDENCE:

BDS_SHORT_MESSAGE_EVIDENCE:

PROJECT_FACT_STATUS_CORRECTED:
YES / NO

UNRESOLVED_HIGH_PRIORITY_GAPS:

SAFE_CLAIMS_READY_FOR_DOCUMENT:

CLAIMS_STILL_FORBIDDEN:

FILES_CREATED:

==================================================
二十二、停止条件
==================================================

完成 Stage D 研究资产后停止。

不要自动进入：

正式第2章撰写

第6—8章改写

整稿重写

DSH成稿

开发线联动

等我审核 Stage D 后再决定是否进入正文。

现在直接开始执行。
~~~~

## 消息 2013 / user / 分段 1 of 1

~~~~text
# VISIBLE\_EVIDENCE\_CHECKPOINT

All four prechecks **PASS**, verified against the underlying CSV rather than the verdict line.

**IMPLEMENTATION\_STATUS:** complete for precheck purposes.

**TARGET\_WORKTREE\_VERIFICATION**
branch `dg202611-synthetic-validation` · head `57ec8ec` · parent `fd2c60d` · dirty (28 files: 9 modified, 19 new) · diff \~2128 insertions across 8 tracked files

**RESULT\_WRITER:** TARGET\_WORKTREE\_VERIFIED
**FIELD\_ROLE\_VALIDATION:** PASS — 75 columns, 0 duplicates, 0 unclassified (`SUT_OUTPUT` 54 / `INPUT` 15 / `METADATA` 4 / `DERIVED_ASSERTION` 2)

**SCENARIO\_YAMLS:** S05 S06 S07 S08 all load via real `load_scenario()`, all installed to share

**PERFECT\_MAP\_ODOM\_PRESENT: NO**
**STATIC\_TF:** `base_footprint→laser` only · **DYNAMIC\_TF:** `odom→base_footprint`
**SURROGATE\_SEPARATED: YES** · **INPUTSTATE\_SEPARATED: YES**
**GROUND\_TRUTH\_LEAKAGE\_CHECK: PASS** (`GROUND_TRUTH_USED_BY_SUT: NO`)

**SETUP\_ENTRY\_POINTS: PASS** — 8 discoverable via `ros2 pkg executables`
**PY\_COMPILE: PASS** · **TARGETED\_BUILD: PASS** (3 packages)
**TARGETED\_FUNCTIONAL\_TESTS:** ylhb\_base **253 passed** — `PREVIOUS_253_PASS_REPRODUCED: YES`, ROS sourced this time
**SYNTHETIC\_PACKAGE\_TESTS:** 33 passed · **FULL\_COLCON\_TEST: NOT\_RUN**

**PRECHECK\_ROOT:** `results/synthetic/precheck/`

| resultkey evidence |      |                                                                                 |
| ------------------ | ---- | ------------------------------------------------------------------------------- |
| S05                | PASS | `trigger_reason=AMCL_COVARIANCE_HIGH`, 239 samples                              |
| S06                | PASS | `odom_yaw` moved 9.28°, 9 supervisor commands matched 1:1 on `/dg/test_cmd_vel` |
| S07                | PASS | seed 6.510 s → accepted match 6.610 s                                           |
| S08                | PASS | `verification_count` reached 3, nav → `RECOVERED`                               |

**S07\_MATCH\_RESULT:** score 0.9796 · inlier 1.000 · mean\_distance 0.00722 · 180 points · classification `SYNTHETIC_MATCH_QUALITY_RESULT`
**S07\_ACCURACY\_CLAIM\_ALLOWED: NO** — 0.00722 is a scan-to-map residual against synthetic grid geometry, not a 7 mm positioning error

**CORE\_STATE\_MACHINE\_UNCHANGED: YES** · **CORE\_THRESHOLDS\_UNCHANGED: YES** (`ylhb_base` git-clean) · **REAL\_CMD\_VEL\_PUBLISHERS: 0** at start and end, all four runs

**STAGING\_TARGET\_DIVERGENCE\_OCCURRENCES: 2** · **FALSE\_S06\_FAIL\_RISK: RESOLVED** (15/15 INPUT columns verified through a real `SampleRecorder` round-trip)
**EMPTY\_PLOT\_DEFECT: FIXED** (14 tests; artist-gid series counting, not column proxies) · **CJK\_CLAIM\_SCANNER\_DEFECT: FIXED** (POSITIVE 11/11, NEGATIVE 10/10)

**MONITOR\_READY: YES** · **RVIZ\_READY:** config valid, MarkerArray display unverified until RViz opens · **MARKER\_READY:** installed, never run
**GUI\_CAPTURE\_READY: MANUAL\_ONLY** — `Print` / `Shift+Print` / `Alt+Print`; screenshot directory `CONFIRM_AFTER_FIRST_CAPTURE`
**DEVELOPMENT\_RECORDS\_UPDATED: PARTIAL** — docs agent was lost mid-task; needs re-dispatch
**NO\_FINAL\_EVIDENCE\_SCENARIO\_EXECUTED: YES**

## Three things worth your attention

**S06 and S07 both ran past their charter to** **`RECOVERED`****.** That's real, not a bug — `VERIFYING` needs only candidate quality and pose stability, not healthy covariance. It means S06/S07 pass their own criteria *and* incidentally demonstrate more.

**The surrogate-mode column proves S08's convergence was real.** S07 shows only `BIASED_NO_CONVERGENCE_CONFIGURED` and its nav ends at `LOCALIZATION_SUSPECT`; S08 shows `BIASED → CONVERGED` and reaches `RECOVERED`. The difference is caused by a real accepted `/scan_match_pose`, not a timer.

**Both S07 and S08 report the identical match quality** (0.9795765221706485). Deterministic and expected given identical geometry and seed — but flagging it so it isn't mistaken for independent corroboration.

## USER\_VMWARE\_COMMANDS

Terminal A — monitor:

svg

```bash
source /opt/ros/humble/setup.bash && source /home/weiyang/dg202611_ws/install/setup.bash
ros2 run dg_synthetic_validation monitor_scenario

```

Terminal B — RViz:

svg

```bash
source /opt/ros/humble/setup.bash && source /home/weiyang/dg202611_ws/install/setup.bash
rviz2 -d /home/weiyang/dg202611_ws/install/dg_synthetic_validation/share/dg_synthetic_validation/rviz/dg_synthetic_validation.rviz

```

Terminal C — markers (state text in RViz) plus log tail:

svg

```bash
source /opt/ros/humble/setup.bash && source /home/weiyang/dg202611_ws/install/setup.bash
ros2 run dg_synthetic_validation visualization_markers_node

```

**WAITING\_FOR: VISIBLE\_READY**

Stopping here. Nothing committed. Before the final batch I'd suggest opening RViz once so we catch the unverified MarkerArray display and confirm the screenshot directory, rather than discovering either mid-evidence-run.
~~~~

## 消息 2014 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "reasoning_recap",
  "content": "思考了 4s"
}
~~~~

## 消息 2015 / assistant / 分段 1 of 1

~~~~text
这次可以过。

**当前开发线结论：VISIBLE_EVIDENCE_CHECKPOINT = PASS。**

几个关键点我认可：

- S05–S08 四个 precheck 都不是只看 verdict，而是回查 CSV，可信度够。
- 253 passed 这次是在 ROS 环境正确 source 后真实复现，可以正式记。
- `InputState` 那个第二次 staging/target 偏差已经闭环，15/15 INPUT 真正 round-trip 验证。
- 空图、CJK scanner 两个“假证据风险”都已经补了测试。
- `ylhb_base` 未改，核心状态机/阈值未为了过测试而动。
- S07 的 `0.00722` 被正确限制成 **synthetic scan-to-map residual**，不能包装成毫米级定位精度。
- S07/S08 相同 match quality 是同一确定性几何条件产生的结果，**不能当成两次独立实验相互佐证**。
- 目前没有任何 final evidence run，这点保持得很好。

现在**先不要提交，也不要直接跑最终 S05–S08**。

你先在 VMware 做一次纯 GUI 验证就行。

按报告里的三个终端分别打开：

**A：Monitor**

```bash
source /opt/ros/humble/setup.bash && source /home/weiyang/dg202611_ws/install/setup.bash
ros2 run dg_synthetic_validation monitor_scenario
```

**B：RViz**

```bash
source /opt/ros/humble/setup.bash && source /home/weiyang/dg202611_ws/install/setup.bash
rviz2 -d /home/weiyang/dg202611_ws/install/dg_synthetic_validation/share/dg_synthetic_validation/rviz/dg_synthetic_validation.rviz
```

**C：Markers**

```bash
source /opt/ros/humble/setup.bash && source /home/weiyang/dg202611_ws/install/setup.bash
ros2 run dg_synthetic_validation visualization_markers_node
```

这一步你只检查四件事：

1. **RViz 能正常打开，没有红色报错 display。**
2. **MarkerArray 确实能显示**，尤其状态文字，不是“配置里有但画面没有”。
3. Monitor 界面正常，不是一片空白/疯狂报错。
4. 随便按一次 `Print` 截图，确认截图到底存到哪个目录。

先**不用追求画面好不好看，也不用现在正式截图比赛证据**。

如果这四个都正常，直接回来跟我说：

**VISIBLE_READY**

如果哪里不正常，给我截图/报错，我们先修 GUI，避免正式证据跑到一半翻车。

另外 docs agent 中途掉了这件事先不用卡住 GUI。等 GUI 验证完，我会让开发线在 final evidence 前把 development records 补齐，再开始真正的 **S05 → S06 → S07 → S08 最终证据批次**。memcite
~~~~

## 消息 2016 / user / 分段 1 of 1

~~~~text
{
  "content_type": "image_asset_pointer",
  "asset_pointer": "sediment://file_0000000060148209ab227557bbbd04bd?shared_conversation_id=6a9b88ba-085c-83e9-b27b-7cbb97a8c954",
  "size_bytes": 160793,
  "width": 1189,
  "height": 930,
  "metadata": {
    "sanitized": true
  }
}
{
  "content_type": "image_asset_pointer",
  "asset_pointer": "sediment://file_000000005d2881f8b9bb0d0479589215?shared_conversation_id=6a9b88ba-085c-83e9-b27b-7cbb97a8c954",
  "size_bytes": 557382,
  "width": 2048,
  "height": 1247,
  "metadata": {
    "sanitized": true
  }
}
----------------------------------------------------------------------------
SCENARIO      : N/A  (not published to the monitor)
PHASE         : N/A  (not published to the monitor)
ELAPSED       : 63.0s
------------------------------------------------------------------------------
GNSS          : NO_DATA
LIDAR         : NO_DATA
FUSION        : NO_DATA
NAV HEALTH    : NO_DATA
------------------------------------------------------------------------------
RELOCALIZATION: NO_DATA
TRIGGER REASON: NO_DATA
FAILURE REASON: NO_DATA
CANDIDATE     : NO_DATA
CANDIDATE Q   : NO_DATA
VERIFICATION  : NO_DATA
SEED SOURCE   : NO_DATA
AMCL HEALTH   : NO_DATA
------------------------------------------------------------------------------
ODOM YAW      : NO_DATA
TEST CMD_VEL  : NO_DATA
RECOVERY CMD  : NO_DATA
CMD SOURCE    : NO_DATA
SAFETY        : /cmd_vel publishers: 0  (no chassis command path active)
------------------------------------------------------------------------------
monitor: subscriptions only; this node creates no publisher of its own
(rclpy itself adds /parameter_events to every node; /rosout is disabled)
Twist carries no source field, so CMD SOURCE is an inference

DG-202611 READ-ONLY MONITOR   elapsed=   64.0s
SYNTHETIC SOFTWARE VALIDATION | NOT_REAL_ROBOT_DATA | NOT_COMPETITION_PERFORMANCE_EVIDENCE
weiyang@ubuntu103:~$ source /opt/ros/humble/setup.bash && source /home/weiyang/dg202611_ws/install/setup.bash
rviz2 -d /home/weiyang/dg202611_ws/install/dg_synthetic_validation/share/dg_synthetic_validation/rviz/dg_synthetic_validation.rviz
Warning: Ignoring XDG_SESSION_TYPE=wayland on Gnome. Use QT_QPA_PLATFORM=wayland to run on Wayland anyway.
[INFO] [1787977556.589201224] [rviz2]: Stereo is NOT SUPPORTED
[INFO] [1787977556.589337945] [rviz2]: OpenGl version: 4.3 (GLSL 4.3)
[INFO] [1787977556.697069982] [rviz2]: Stereo is NOT SUPPORTEDweiyang@ubuntu103:~$ source /opt/ros/humble/setup.bash && source /home/weiyang/dg202611_ws/install/setup.bash
ros2 run dg_synthetic_validation visualization_markers_node
[INFO] [1787977567.779939202] [dg_validation_visualization_only_node]: visualization-only node: publishes /dg_validation_viz/markers and nothing else; no TF is published and no map->odom transform is created










~~~~

## 消息 2018 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "thoughts",
  "thoughts": [
    {
      "summary": "评估可见状态",
      "content": "我正在确认当前是否达到可见就绪状态：RViz 已显示轨迹、栅格和标记，节点也在运行。不过 Global Status 仍为 Warn，扫描相关状态为红色，说明还需要验证数据流或启动场景；目前还不能算最终通过。",
      "chunks": [],
      "finished": true
    }
  ],
  "source_analysis_msg_id": "bac7b89f-0790-4c8c-af6e-fa94bee52aea"
}
~~~~

## 消息 2019 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "reasoning_recap",
  "content": "思考了 11s"
}
~~~~

## 消息 2020 / assistant / 分段 1 of 1

~~~~text
看到了，**GUI 链路本身已经打通，但我现在不会给 `VISIBLE_READY`，因为 RViz 的证据画面还需要最后一次纯显示层修正。**

目前判断：

- **RViz 能正常启动：PASS**
- **MarkerArray 确实显示出来了：PASS**
- **Monitor 正常运行：PASS**
- Marker 节点确实只发布 `/dg_validation_viz/markers`，没有 TF/map→odom：**PASS**
- Monitor 当前全部 `NO_DATA`：**正常**，因为现在没有场景在运行，不是故障。
- RViz 左侧 `Raw Scan / Filtered Scan` 等红色/Global Status Warn：在当前无场景、无 topic 数据时暂时不说明配置有错，正式运行场景时再判断。
- **真正的问题：Marker 文本严重重叠，不适合作为正式证据截图。**

你截图里最明显的是：

> `SYNTHETIC SOFTWARE VALIDATION - NOT REAL ROBOT DATA`

这一行很好，必须保留。

但是下面状态、topic、candidate 等文字全挤在一起了。正式截图如果这样，评审根本读不清。

所以开发线现在只需要做一个很小的 **VISUALIZATION-ONLY FIX**：

1. 调整 Marker 文本的 `x/y/z` 布局；
2. 缩小/统一字体尺寸；
3. 把信息分区：
   - 顶部：证据性质声明；
   - 左上/中上：场景与状态；
   - 右上：融合/重定位；
   - 下方：candidate / match quality；
4. 默认 RViz 视角和 Scale 调好，打开就能看清；
5. 不修改任何 SUT、状态机、阈值、topic 语义；
6. 再打开一次 RViz 验证；
7. 正式场景运行时再确认 Raw Scan / Filtered Scan / Odom 不再是错误状态。

**不要现在就跑 final S05–S08。**

另外还有一个小 Gate 没确认：

> GNOME 截图实际保存目录。

你现在显然已经能截到图了，但最好确认一下文件到底落在哪里，避免正式 S05–S08 跑完之后找不到原图。

所以现在状态是：

`GUI_RUNTIME = PASS`  
`MARKERARRAY_RUNTIME = PASS`  
`MONITOR_RUNTIME = PASS`  
`EVIDENCE_LAYOUT = NEEDS_FIX`  
`SCREENSHOT_DIRECTORY = NEEDS_CONFIRMATION`  
`VISIBLE_READY = NOT_YET`

这已经非常接近最终证据批次了，**别再扩大范围，只修这个显示问题。** memcite
~~~~

## 消息 2021 / user / 分段 1 of 1

~~~~text
为什么rviz2里面只有这些字啊，这是对的吗
~~~~

## 消息 2022 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "model_editable_context",
  "model_set_context": ""
}
~~~~

## 消息 2023 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "reasoning_recap",
  "content": "思考了 7s"
}
~~~~

## 消息 2024 / assistant / 分段 1 of 1

~~~~text
对，**你现在这个状态下“只有这些字”基本是正常的**，因为你目前只是把 RViz 和 marker 节点打开了，**还没有真正运行 S05/S06/S07/S08 场景**。

你现在 RViz 里真正有数据的，只有这个节点：

`visualization_markers_node`

所以它会显示：

- `SYNTHETIC SOFTWARE VALIDATION - NOT REAL ROBOT DATA`
- 当前状态文字
- Scan-to-map / candidate 等文字框

而左边这些：

- Raw Scan
- Filtered Scan
- Odometry
- TF

现在没有对应场景数据在发布，所以 RViz 没东西可以画。你 Monitor 也明确显示了：

`SCENARIO: N/A`
`GNSS: NO_DATA`
`LIDAR: NO_DATA`
`ODOM YAW: NO_DATA`

这和 RViz 现象是完全一致的。

正常流程应该是：

```text
现在
RViz + Monitor + Marker
        ↓
只有说明文字 / NO_DATA
        ↓
启动 S05/S06/S07/S08
        ↓
synthetic injector 开始发数据
        ↓
/scan、odom、TF、定位状态等开始变化
        ↓
RViz 才应该出现扫描点/轨迹/TF/状态变化
```

所以分两种情况判断：

**现在没跑场景时只有字：正常。**

**等正式跑 S05～S08 时还是只有字：那就不正常，说明 Scan/Odom/TF 的 RViz 显示链路还有问题。**

另外你现在左边 `Raw Scan`、`Filtered Scan` 是红色，`Global Status: Warn`，目前也不必急着当 bug，因为没有数据源时 RViz 很容易这样。真正要看的，是场景启动以后它们能不能恢复并显示内容。

还有一点：**正式证据截图绝对不应该就是你现在这张。** 这张只能证明“RViz + Marker 能启动”。最终截图应该是在 S05/S06/S07/S08 跑起来以后，能看到实际 synthetic scan、odom/TF、状态变化，再配合 Monitor 截图。

所以现在你看到的不是“仿真画面”，它只是**证据可视化界面的空闲状态**。真正会动、会有扫描和轨迹的是下一步跑场景之后。
~~~~

## 消息 2025 / user / 分段 1 of 1

~~~~text
行，意思是目前没问题是吧
~~~~

## 消息 2026 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "reasoning_recap",
  "content": "思考了 9s"
}
~~~~

## 消息 2027 / assistant / 分段 1 of 1

~~~~text
对，**目前没有实质性问题**。

你现在看到“RViz 里主要只有文字、Scan/里程计没显示”，是因为**还没启动 S05–S08 场景数据**，所以属于正常空闲状态。

当前已经确认：
- RViz 能正常打开；
- Marker 节点能正常发布并显示；
- Monitor 正常；
- 没有误发 `/cmd_vel`；
- 没有非法 TF；
- `NO_DATA` 与“当前没跑场景”一致。

只剩两个小事：
1. Marker 文字有点重叠，正式截图前最好调一下布局；
2. 确认截图实际保存目录。

**等正式启动场景以后，再看 Scan、Odom、TF 是否真正出现。** 如果那时候还是只有文字，才算有问题。

所以现在可以继续往下走。
~~~~

## 消息 2028 / user / 分段 1 of 1

~~~~text
接下来搞什么
~~~~

## 消息 2029 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "reasoning_recap",
  "content": "思考了 4s"
}
~~~~

[本对话目录](README.md) | [总目录](../../README.md) | [Raw](https://raw.githubusercontent.com/weiyang02520-ops/dg202611-conversation-archive/main/conversations/first/part-055.md) | [上一卷](part-054.md) | [下一卷](part-056.md)
