# 机器人定位挑战赛分析 / 第 073 卷

[本对话目录](README.md) | [总目录](../../README.md) | [Raw](https://raw.githubusercontent.com/weiyang02520-ops/dg202611-conversation-archive/main/conversations/first/part-073.md) | [上一卷](part-072.md) | [下一卷](part-074.md)

## 消息 2456 / user / 分段 1 of 1

~~~~text
TASK\_DIRECTION: EXTERNAL\_AUDIT\_LINE

第一轮独立外部审计完成。13 份报告已落盘 [DG-202611\_EXTERNAL\_AUDIT/](vscode-webview://0f4m7pn0sninoa4t6baiuusea0qhmsg1nfkqf92pri999t1ojc4h/%E6%9D%90%E6%96%99/DG-202611_EXTERNAL_AUDIT/)，未修改任何被审源。

**OVERALL\_VERDICT: CHANGES\_REQUIRED** — 29 个 issue（BLOCKER 2 / CRITICAL 5 / HIGH 7 / MEDIUM 11 / LOW 2 / INFO 2）

## 两个 BLOCKER

**EA-005 — UWB 断言无一手证据。** v0.2 审阅版 docx 有 7 个段落、CH2 canonical v1.1.1（状态 `FROZEN` / `FORMAL_MERGE_AUTHORIZED`）第 29/63/81 行以现在时写"UWB 已实现并纳入多源定位体系，系统通过软件质量评估机制动态降权或拒绝"。我在 ROS2 仓库实测：`grep -rIil "uwb" src/` → **0 个文本文件命中**（`-ril` 的 16 个命中全是 jpg/png/pdf/CAD 二进制字节巧合），`docs/` → 0，DW1000/Decawave/LinkTrack 等 8 个硬件关键词全 0，融合核心 `accepted_source` 取值全集只有 `{lidar, GNSS}`。该断言的依据链逐字是「本轮任务'当前冻结技术事实'」，并据此作废了 Stage D 的「硬件存在、未融合」旧记录；而同一文档包内 3 处（`03_STRUCTURE_REVIEW_NOTES:19`、`05_MISSING_CONTENT:7`、`19_CH2_REMAINING_GAPS:G-01`）同时记载该证据缺失，项目自订的两道禁令（`06_CH2_FORBIDDEN_CLAIMS:19`、`10_CHECKLIST:31`）被反向覆盖。判定：`PROJECT_STATUS_ASSERTION_REQUIRES_ADDITIONAL_PRIMARY_EVIDENCE`。

**EA-001 — 代码不在任何已发布分支。** GitHub `main` = e83d308（190 commits）是巡检智能体/3D 建图那条谱系；DG-202611 的 13 个提交、`dg_synthetic_validation` 包、`docs/dg202611/` 10 份文档在 main 上 **0 命中**。基线 `c41adca` 能按 SHA fetch 到，但不被任何 ref 引用。本地是 shallow clone，两条分支都无远程跟踪，另有 32 项未提交改动。

## 五个 CRITICAL

- **EA-002** S05–S08 的 `build_commit: 57ec8ec` 不含产出该产物的代码 —— 57ec8ec 版 `result_writer.py` 中 `run_class`/`ground_truth_disclosure`/`field_roles`/`match_quality_note` 命中数全为 0，而这些字段出现在每个 result.json 里；工作树 +2501/−211 未提交，`scenario_runner.py:108` 只取 HEAD 无 dirty 检测，且 install 是 symlink 模式（跑的就是脏工作树）。
- **EA-003** S01–S04 共 29 次运行 **9 次 FAIL**，summary 只登记最后一次 PASS 且 `failure_count=0`，无排除记录、无 canonical 规则、S01–S04 连 `run_class` 字段都没有。
- **EA-004** 判据在观测到失败后被放宽：14:50 `ignore bounded startup relocalization noise` → 14:53 `scope relocalization checks by scenario` → 14:55 `apply startup grace to nominal health`，与 14:53:55/14:55:27/14:56:57 三次 FAIL 交错，14:58:33 那次 PASS 被 summary 采用。
- **EA-022 / EA-023** 两处文献汇总跨数据集串位，我亲自抓原文核对：GOReloc 的 17.43% 属 ICL-Data "Head" 序列，fr2\_person @2m 实为 **64.87%**（偏差 47 个百分点）；"低成本 RTK 高程 0.25–0.30 m"把动态**平面** 0.30 m 当高程用，真实动态高程是 **0.45 m**。两处方向都是低估差距，且分别支撑"重定位 >95%"和"高程 <20 cm"的可达性判定。

## 需要说清的正面结论

不是所有东西都有问题，这部分同样是核验结果：

- **四项竞赛指标无一被声称达成。** 20cm/95%/95% 在文档里一律是"比赛要求/无数据/尚未证明"，反冒充规则齐备（北斗短报文 4 处、厂商规格 3 处）。特征重复率明确区分了点级 `temporal_match_ratio` 与官方特征级口径。
- **ZCode 语料级统计 15 项全部可复算**（246/239/217/22/189/137/129-3-5/18/25 等），0 例虚构论文、0 例非法 DOI。问题在汇总层，不在原始层。
- **合成证据纪律在文件层真实存在**：`performance_claim: false`、`match_quality_note` 明令禁止把 residual 当定位精度、`field_roles` 区分 INPUT/SUT\_OUTPUT/DERIVED\_ASSERTION、precheck 在目录与字段两层隔离。S06 的 evidence\_manifest 主动区分 `NOT_CAPTURED` 与 `NOT_OBSERVED` 并禁止事后重建 —— 这条做得比多数项目好。
- 任务书 §19 担心的"两组相同确定性输出互证"没发生（S07/S08 matcher 数值不同）。

## 两处需要你知道的修正

任务书 §16 转述的开发线判断 **不成立**：`/initialpose` 的 `TRANSIENT_LOCAL` 在 scan\_map 节点的 *publisher* 上，它对 `/initialpose` 的 *subscription* 用的是默认 QoS（VOLATILE），与 injector 的 VOLATILE publisher 兼容。"S06 不受影响"这个结论恰好对，但正确理由是 S06.yaml `publish_initialpose: false`。

审计过程中我自己有一次误判并已推翻：一次 `find -maxdepth 4` 的输出漏了 `final/S08`，据此曾判"S08 无 final 运行"；重跑同一命令返回 11 个目录含该 S08，该判断未进入任何结论。这条写在报告里了。

## 需要披露的两处流程偏差

被审内容完整性我校验过：18 份文献资产与 git 镜像逐一哈希相同，`DG-202611比赛材料文档` 无文件 mtime 晚于审计开始。但受托的取证子过程有两处违反我下发的 READ\_ONLY 约束：

1. 审计输出目录里多了一个 `_extract_v0.2_text.txt`（76 KB，v0.2 docx 的纯文本抽取）。不是我写的，内容无害且正好可复核 EA-005 的 docx 引文，我保留了它 —— 要删我就删。
2. `DG-202611_literature_research\` 的**目录** mtime 变成 17:30，说明期间在该目录内建过又删了一个临时文件。所有文件内容哈希未变、文件数未变，无内容影响，但"往被审目录里落盘"本身是偏差，已记入报告。

按 §47 停在这里，不修代码、不改方案书、不派活给其他线。等你和 MASTER 定 triage。，，，，，，，，，，，这个是Claude的审查，你先记着，下面是zcode的审查收到。我以 DG-202611 独立外部审计员身份开始第一轮审计：先做全源可访问性检查，建立审计范围文件，再按优先级逐项核验，全程只读，输出到独立审计目录。 
 
主要数据源基本可访问。继续检查镜像仓库的 remote、文档子目录结构和 CH2 canonical 文件。 
 
镜像与远端一致，SHA256 对比仅 1 个文件差异（`staging/xicp_tmp2.html`）。继续核验：排除清单是否声明该文件、CH2 全文、文献统计数字。 
 
镜像完整性核验通过（排除项已声明）。文献池 239 行、bib 239 条、性能矩阵 189 行均与自报一致。继续：CSV 状态列精确统计、PLAN_REVIEW 文件、GitHub 主仓库克隆。 
 
文献统计基本全部吻合（217/22、87 篇含数字、189 条、137/52 复核分布）。文献仓库确认 PUBLIC。现在检查主仓库代码事实与文献库细分统计。 
 
重要发现：主仓库中 **没有任何 UWB 相关代码**（grep 无命中），而方案书声称 UWB“已实现并纳入体系”。继续核验主仓库 GNSS 驱动、数据集/仓库计数与历史基线 commit。 
 
GNSS 驱动、硬件清单与主仓库事实一致；c41adca 基线存在（内容为 fusion 自适应权重）。现在读写作计划与证据矩阵、检查方案书中 synthetic 证据引用情况。 
 
证据矩阵声明了大量 c41adca 提交下的 POC 文件，必须逐一核验其真实存在。同时检查 HEAD 是否仍保留这些文件。 
 
HEAD (e83d308) 已删除大部分比赛 POC 脚本——这是重大发现。重新用正确路径核验 c41adca 的文件树。 
 
c41adca 树核验通过：证据矩阵声称的 17 个文件与全部测试均存在。HEAD 已删除大部分 POC 脚本，记为重大风险。 
 
范围文件已建立。继续 Priority 1：提取 v0.2 审阅版 docx 全文并审阅，同时检查方案书中 synthetic 引用与文献库细分结构。 
 
v0.2 审阅版文本已提取。检查其中的 UWB 口径、指标表述与潜在过度声明。 
 
v0.2 审阅版整体纪律性好：指标均标注为比赛要求、短报文边界清晰、无明显过度表述。核心遗留风险集中在 UWB“已实现”断言。继续查 POC 从 main 消失的提交记录、文献库细分统计与引文数值溯源。 
 
需要修正 git 考古结论（上一条 0 提交结果不可靠）。同时把 CH2 引用的具体数值在性能矩阵中定位到论文。 
 
c41adca 确实不在 main 历史中（main 停在 2026-07-22），POC 在另一条未合并历史线上。确认远端分支；同时读 CH2 数字白名单核对引文数值出处。 
 
数字白名单结构完整，CH2 引用数字与白名单一一对应。现在对其中最关键的 3 组数字做独立 web 复核（VINS-Mono、LIO-SAM、UWB NLOS）。 
 
VINS-Mono 与 LIO-SAM 两组数字均与原论文精确一致。继续核验 UWB 与 FAST-LIO2 数字。 
 
FAST-LIO2 数字与原文一致。UWB 检索超时，改查项目内二级核验记录并重试一次检索；同时确认 c41adca 是否在某条 PR 引用上。 
 
UWB 98.8%/99.4% 数字与 Glasgow Yang 2025 论文精确一致。c41adca 不在任何分支/PR 上——是悬空提交，风险升级。最后补齐 ID 映射与竞争指标文献审计，然后写全部审计文件。 
 
CH2 引用的关键论文在 189 行性能矩阵中缺席，需要确认 CH2 引用编号到实际论文的映射链。 
 
双文献体系结构已厘清（Stage C/D 的 S/N 体系 + ZCode 库）。最后核查 UWB 在文档线内部的证据轨迹，然后开始写审计文件。 
 
核验完毕：c41adca 基线树中同样无任何 UWB 文件。所有证据链已闭合，开始写审计输出文件。 
 
第一轮独立外部审计完成。全部 13 份审计文件已输出至 `C:\Users\peng\Desktop\材料\DG-202611_EXTERNAL_AUDIT\`，未修改任何被审计源（仅系统临时目录留有一个只读克隆，可随时删除；我曾误存一个临时 JSON 进文献库目录，已删除复原）。 
 
```text 
TASK_DIRECTION: EXTERNAL_AUDIT_LINE 
TASK: DG202611_INDEPENDENT_EXTERNAL_AUDIT 
AUDIT_MODE: READ_ONLY | AUDITOR_PRIOR_PROJECT_CONTEXT: NONE | ROUND 1 
 
OVERALL_VERDICT: CHANGES_REQUIRED 
PROJECT_PROCESS_VERDICT:        CHANGES_REQUIRED 
DOCUMENT_VERDICT:               PASS_WITH_NOTES 
LITERATURE_EVIDENCE_VERDICT:    PASS_WITH_NOTES 
SOFTWARE_AND_EXPERIMENT_VERDICT: CHANGES_REQUIRED 
COMPETITION_METRIC_CLAIMS_VERDICT: PASS_WITH_NOTES 
 
ISSUES: 10 = BLOCKER 0 / CRITICAL 0 / HIGH 2 / MEDIUM 3 / LOW 3 / INFO 2 
``` 
 
**两个 HIGH 问题（需要 MASTER 分诊）：** 
 
1. **AUD-001（HIGH）POC 基线悬空**。文档线指定的软件事实源 `c41adca`（2026-08-27，含全部 7 个比赛 POC 模块、launch、测试——文件树逐项核验 17/17 属实）**不在 main 上，也不在任何分支或 PR 上**，是一条悬空提交线；main 停在 2026-07-22，`scripts/` 只剩 4 个脚本，比赛 POC 已不在。若按 main 提交源代码或评审直读 main，“已落库”不成立。→ 建议开发线立即在 c41adca 上建分支/tag 并定提交形态。 
 
2. **AUD-002（HIGH）UWB“已实现并纳入体系”缺原始证据**。冻结 CH2 与 v0.2 审阅版多处把 UWB 写成当前事实，但：main 仓库和 c41adca 树 grep `uwb` 均零命中；`00_证据矩阵` 成熟度表无 UWB 行；文档线自己的 Stage D 状态矩阵记录“当前融合证据 NO”；该口径来自 08-29 MASTER 口头冻结事实，无归档证据。属规范 §23 的 `PROJECT_STATUS_ASSERTION_REQUIRES_ADDITIONAL_PRIMARY_EVIDENCE`，同时是典型的多 AI 层层引用同一口头口径结构。→ 补证据包（硬件照片/型号、代码位置、一次融合运行记录）或降级表述；否则提交前升级 BLOCKER。 
 
**其余核验结果（均健康）：** ZCode 文献库自报统计 9 项中 8 项独立重算精确一致（239/217/22/60/30/87/189/137+52/25 仓库），仅“原始候选约246”无留存依据（LOW）；镜像 SHA256 逐文件一致、远端 PUBLIC 且与本地同 HEAD，唯一排除文件已合规留痕；CH2 数字白名单 13 组中抽样 4 组（VINS-Mono 漂移、LIO-SAM 表 III、FAST-LIO2 手持实验、UWB CIR 98.8%/99.4%）全部与原始论文精确一致；四项比赛指标在 v0.2 中全部正确保持“比赛要求/目标”，短报文、毫米波、事件相机、高程、二维 EKF 边界处理正确，未发现伪造文件、伪造统计或论文数字幻觉。Synthetic S01–S08 与 ROS2 工作区因环境不可达记 `NOT_YET_AUDITABLE`（文档侧反向检查未发现 precheck 数字泄入正文）。 
 
**建议处理线：** AUD-001→DEVELOPMENT_LINE；AUD-002→MASTER（给证据或改口径）→DOCUMENT_LINE；AUD-003（双文献体系整合）/AUD-005（00 号状态文件已过期）/AUD-006（vCurrent 加废弃标记）→DOCUMENT_LINE；AUD-004→ZCODE_RESEARCH_LINE。审计至此停止，等待您与 MASTER 决定下一步（完整依据见 `11_OPEN_ISSUES_REGISTER.md`，分诊摘要见 `12_FINAL_INDEPENDENT_AUDIT_REPORT.md`）。，，，，，，，，，，，，，，，，，，，，，等会，怎么好像他们存到一个地方搞冲突了
~~~~

## 消息 2474 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "thoughts",
  "thoughts": [
    {
      "summary": "核对审计文件",
      "content": "我正在比较两份审计的输出数量与文件命名。当前上传内容显示为29项问题的版本，和Claude报告一致；如果ZCode随后使用相同文件名写入，确实可能发生覆盖。最好先分别保存两套结果，再继续整合。",
      "chunks": [],
      "finished": true
    },
    {
      "summary": "核对了审计文件",
      "content": "",
      "chunks": [],
      "finished": true
    }
  ],
  "source_analysis_msg_id": "4c87160c-483e-4890-90da-5827458d5450"
}
~~~~

## 消息 2475 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "reasoning_recap",
  "content": "思考了 12s"
}
~~~~

## 消息 2476 / assistant / 分段 1 of 1

~~~~text
对，**这里确实有问题**，而且是我之前那份审计提示词设计得不够严。

两个审计 AI 都被安排写到了同一个目录：

`C:\Users\peng\Desktop\材料\DG-202611_EXTERNAL_AUDIT\`

而且文件名还完全一样：

`00_AUDIT_SCOPE_AND_SOURCES.md`  
`01_PROJECT_PROCESS_AUDIT.md`  
……  
`12_FINAL_INDEPENDENT_AUDIT_REPORT.md`

所以**存在覆盖/互相污染风险**。

你现在上传给我的这 13 份文件，我确认是 **Claude 那一套 29 个 issue 的审计**，例如当前 `11_OPEN_ISSUES_REGISTER.md` 明确写的是 `TOTAL_AUDIT_ISSUES: 29`、`BLOCKER 2 / CRITICAL 5 ...`。fileciteturn294file12L1-L5 最终报告也是同样的 29 个问题。fileciteturn294file13L54-L66

所以目前情况大概率是：

> **Claude 审计文件还在；ZCode 后面也试图往同一个目录写。**

至于 ZCode 是覆盖失败、跳过已有文件、部分覆盖，还是它的 10-issue 报告现在藏在别处，**目前不能猜**。

### 现在先别再让任何一个 AI 写这个目录

把结构改成这样最安全：

```text
C:\Users\peng\Desktop\材料\DG-202611_EXTERNAL_AUDIT\
│
├─ ROUND1_CLAUDE\
│   ├─ 00_AUDIT_SCOPE_AND_SOURCES.md
│   ├─ ...
│   └─ 12_FINAL_INDEPENDENT_AUDIT_REPORT.md
│
├─ ROUND1_ZCODE\
│   ├─ 00_AUDIT_SCOPE_AND_SOURCES.md
│   ├─ ...
│   └─ 12_FINAL_INDEPENDENT_AUDIT_REPORT.md
│
└─ MASTER_TRIAGE\
    └─ 后面我合并两边审计结果
```

**两份绝对不能先互相合并。**

因为我们正需要比较：

- Claude 发现 29 个；
- ZCode 发现 10 个；
- 哪些两边都发现；
- 哪些只有 Claude 发现；
- 哪些只有 ZCode 发现；
- 哪些是误报；
- 两边对同一个事实有没有互相矛盾。

这才是请两个独立审计 AI 的意义。

### 现在最重要的是保住 ZCode 那份

Claude 这套已经保住了——你已经把 13 份文件上传给我了。

现在去 ZCode 那边，**先不要让它重新审计**，只让它做一件事：

> 把它刚刚第一轮产生的原始 13 份审计文件，复制/恢复到独立的 `ROUND1_ZCODE` 目录，不修改任何审计内容。

如果它那套已经被覆盖，也要让它检查自己的会话产物/工作记录，尽可能原样恢复，**不要重新跑后“凭记忆重写”**。

之后你把 ZCode 的那套也发给我。

然后我来做：

**Claude 29 issues + ZCode 10 issues → 独立交叉比对 → MASTER 统一裁决。**

顺带说一下，这次其实还暴露了一个挺典型的问题：**连负责审 AI 的 AI，都可能因为输出路径设计不严谨互相踩文件。** 所以后面审计线的目录隔离我会写死，不再共用目录。
~~~~

## 消息 2477 / user / 分段 1 of 1

~~~~text
我靠，给提示词吧
~~~~

## 消息 2478 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "model_editable_context",
  "model_set_context": ""
}
~~~~

## 消息 2479 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "thoughts",
  "thoughts": [
    {
      "summary": "整理恢复提示",
      "content": "我正在把任务拆成两步：先恢复当前智能体遗漏的审计文件，再确保结果独立保存到指定目录。请仅处理自身产出，保留原文件名、内容和证据链，不覆盖共享目录，并在完成后报告未恢复项目及原因。",
      "chunks": [],
      "finished": true
    },
    {
      "summary": "整理恢复提示",
      "content": "",
      "chunks": [],
      "finished": true
    }
  ],
  "source_analysis_msg_id": "3cfd4ab1-b9d7-4f39-b4d9-804063433e8f"
}
~~~~

## 消息 2480 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "reasoning_recap",
  "content": "思考了 17s"
}
~~~~

[本对话目录](README.md) | [总目录](../../README.md) | [Raw](https://raw.githubusercontent.com/weiyang02520-ops/dg202611-conversation-archive/main/conversations/first/part-073.md) | [上一卷](part-072.md) | [下一卷](part-074.md)
