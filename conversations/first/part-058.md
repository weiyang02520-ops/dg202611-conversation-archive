# 机器人定位挑战赛分析 / 第 058 卷

[本对话目录](README.md) | [总目录](../../README.md) | [Raw](https://raw.githubusercontent.com/weiyang02520-ops/dg202611-conversation-archive/main/conversations/first/part-058.md) | [上一卷](part-057.md) | [下一卷](part-059.md)

## 消息 2043 / assistant / 分段 2 of 2

~~~~text
on_Metric

Official_Requirement

Closest_Literature_Metric

Exact_Same_Definition
YES/NO

Paper_ID

Scenario

Ground_Truth

Reported_Value

Directly_Comparable
YES/PARTIAL/NO

Reason

Current_Project_Evidence

Safe_Current_Wording

Experiment_Needed

重点：

Horizontal <20 cm

Vertical <20 cm

Feature Repeatability >=95%

Relocalization Success >95%

BeiDou Short Message Integration

==================================================
二十一、性能数字第三轮审计
==================================================

Stage D 已有：

PERFORMANCE_VALUES_EXTRACTED = 16

SECONDARY_VERIFIED = 15

Stage E 对这16个数字做：

COMPARABILITY AUDIT

不是单纯再核数字是否抄对。

每条新增：

METRIC_DEFINITION_MATCH

SCENARIO_MATCH

GROUND_TRUTH_QUALITY

PLATFORM_SIMILARITY

SENSOR_SIMILARITY

BEST_CASE_BIAS

DIRECTLY_COMPARABLE_TO_DG

SAFE_AS_BENCHMARK

结论：

DIRECT BENCHMARK

INDIRECT REFERENCE

BACKGROUND ONLY

==================================================
二十二、不要拿论文最好结果给项目定目标
==================================================

必须建立：

BEST_REPORTED

TYPICAL_REPORTED

DEGRADED_REPORTED

FAILURE_CASE

如果只有 BEST_REPORTED：

不能直接拿来作为项目目标。

必须注明：

INSUFFICIENT_FOR_TARGET_SETTING

==================================================
二十三、正式第2章 Ready Gate
==================================================

Stage E 最终必须判断：

READY_TO_WRITE_FORMAL_CHAPTER_2:
YES / NO

只有满足以下条件才建议 YES：

1. >=15篇全文实验核验核心论文，或有合理替代证据；
2. 国内外核心方向都有可靠来源；
3. UWB / Radar / Event / Adaptive Fusion 主线证据足够；
4. 不依赖AI生成材料作为正式引用；
5. 核心性能数字具有实验条件；
6. 研究现状能够形成真正的比较，而不是论文罗列；
7. 关键争议与限制已有记录；
8. 引用元数据完整。

如果不满足：

继续 NO。

不得为了推进写作强行 PASS。

==================================================
二十四、第6—8章 Ready Gate
==================================================

本轮同时重新判断：

READY_TO_SUPPORT_CHAPTER_6_8_METHODS

可分别给：

Chapter 6
Chapter 7
Chapter 8

状态：

READY
READY_WITH_LIMITATIONS
NOT_READY

只评价：

方法论和引用依据。

不要评价项目结果。

==================================================
二十五、北斗短报文轻量补证
==================================================

不作为本轮主力。

只确认：

- 官方 BDS/RDSS/短报文服务说明
- 北斗三号相关官方资料
- 典型接口/服务边界

不要浪费大量时间搜索不相关应用文章。

继续保持：

PROJECT_SHORT_MESSAGE_INTEGRATION = NOT_VERIFIED

==================================================
二十六、文献真实性继续强制
==================================================

每篇新增正式引用必须：

TITLE VERIFIED

AUTHORS VERIFIED

YEAR VERIFIED

VENUE VERIFIED

DOI/OFFICIAL URL VERIFIED

若全文：

EXPERIMENT SECTION LOCATED

任何疑似：

fake paper
fake DOI
wrong title
secondary citation only

进入：

REJECTED_OR_UNVERIFIED

==================================================
二十七、来源层级
==================================================

优先：

IEEE
Springer
Elsevier
ACM
Wiley
Nature/Science
IJRR
T-RO
RA-L
ICRA
IROS
RSS
CVPR/ICCV/ECCV
官方arXiv
作者机构仓库
官方BDS资料

普通网页只能用于找到原始来源。

==================================================
二十八、不要把老师提供的AI材料作为正式论文证据
==================================================

老师此前提供的两份定位技术材料：

可以：

作为技术关键词来源
作为方向启发

不能：

直接作为正式引用
直接采用其性能数字
直接把其中AI生成结论视为事实

最终必须追溯原始论文或官方资料。

==================================================
二十九、输出目录
==================================================

在：

C:\Users\peng\Desktop\材料\DG-202611比赛材料文档\

创建：

STAGE_E_高优先级证据缺口闭合_v1.0

至少包含：

01_STAGE_E_ABSTRACT_ONLY_FULLTEXT_RECOVERY_v1.0.md

02_STAGE_E_VERTICAL_20CM_EVIDENCE_AUDIT_v1.0.md

03_STAGE_E_FEATURE_REPEATABILITY_95_AUDIT_v1.0.md

04_STAGE_E_RELOCALIZATION_95_SUCCESS_AUDIT_v1.0.md

05_STAGE_E_UWB_DEEP_EVIDENCE_v1.0.md

06_STAGE_E_MMWAVE_DEEP_EVIDENCE_v1.0.md

07_STAGE_E_EVENT_DEEP_EVIDENCE_v1.0.md

08_STAGE_E_ADAPTIVE_FUSION_DEEP_EVIDENCE_v1.0.md

09_STAGE_E_COMPETITION_METRIC_COMPARABILITY_MATRIX_v1.0.csv

10_STAGE_E_PERFORMANCE_VALUE_COMPARABILITY_AUDIT_v1.0.csv

11_STAGE_E_CHAPTER_READINESS_GATE_v1.0.md

12_STAGE_E_UNRESOLVED_CRITICAL_GAPS_v1.0.md

13_STAGE_E_SEARCH_AND_VERIFICATION_LOG_v1.0.md

14_STAGE_E_STAGE_C_D_CORRECTION_LOG_v1.0.md

STAGE_E_references.bib

继续优先：

Markdown
CSV
BibTeX

不要生成大型渲染文件。

==================================================
三十、允许并行研究
==================================================

如当前环境支持并行：

Research Agent A：
13篇全文获取

Research Agent B：
高程20cm

Research Agent C：
特征重复率95%

Research Agent D：
重定位95%

Research Agent E：
UWB

Research Agent F：
毫米波

Research Agent G：
Event

Research Agent H：
自适应融合

Verification Agent：
性能数字与论文真实性二次检查

Synthesis Agent：
最终比较矩阵和Ready Gate

但所有结论必须统一审计，避免不同 Agent 使用不同指标定义。

==================================================
三十一、本轮扩量限制
==================================================

新增高价值核心论文：

建议 10–25 篇以内。

如果确实需要更多，可以增加，但必须说明原因。

禁止：

为了达到某个论文数量而降低质量。

==================================================
三十二、Stage E 结束时必须回答的问题
==================================================

必须明确回答：

Q1：
高程20 cm 有无直接文献支持？

Q2：
高程20 cm 最接近的成熟技术路线是什么？

Q3：
95%特征重复率是否存在标准等价指标？

Q4：
如果没有，项目应该如何定义和验证？

Q5：
>95%重定位成功率是否有直接可比闭环文献？

Q6：
UWB质量判断最可靠的证据是什么？

Q7：
RSSI能否单独做UWB可靠性判断？

Q8：
毫米波真正适合提供什么定位能力？

Q9：
Event与ZED/普通帧相机是否要求同FPS？

Q10：
自适应融合中降权与拒绝的理论边界是什么？

Q11：
当前第2章能否正式开写？

Q12：
第6—8章哪些部分已经有足够方法论依据？

==================================================
三十三、最终报告格式
==================================================

最终第一行必须：

TASK_DIRECTION: DOCUMENT_LINE

然后：

STAGE_E_STATUS:
PASS / PASS_WITH_LIMITATIONS / PARTIAL / BLOCKED

FORMAL_BODY_MODIFIED:
NO

DEVELOPMENT_LINE_CONTACTED:
NO

DSH_USED:
NO

STAGE_D_FROZEN:
YES

ABSTRACT_ONLY_INITIAL:
13

FULLTEXT_RECOVERED_FROM_13:

TOTAL_FULLTEXT_EXPERIMENT_VERIFIED:

NEW_HIGH_VALUE_PAPERS_ADDED:

TOTAL_CORE_EVIDENCE_PAPERS:

PERFORMANCE_VALUES_REAUDITED:

DIRECTLY_COMPARABLE_PERFORMANCE_VALUES:

INDIRECT_REFERENCE_VALUES:

VERTICAL_20CM_STATUS:
DIRECT_SUPPORT / INDIRECT_ONLY / NO_SUPPORT

VERTICAL_20CM_SAFE_CONCLUSION:

FEATURE_REPEATABILITY_95_STATUS:
DIRECT_EQUIVALENT / PARTIAL_EQUIVALENT / NO_DIRECT_EQUIVALENT

FEATURE_REPEATABILITY_RECOMMENDED_DEFINITION:

RELOCALIZATION_95_STATUS:
DIRECT_SUPPORT / INDIRECT_ONLY / NO_DIRECT_SUPPORT

UWB_EVIDENCE_STATUS:

RSSI_ALONE_SUFFICIENT:
YES / NO / CONDITIONAL

MMWAVE_EVIDENCE_STATUS:

EVENT_EVIDENCE_STATUS:

EVENT_AND_FRAME_CAMERA_REQUIRE_SAME_FPS:
YES / NO / NOT_SUPPORTED

ADAPTIVE_FUSION_EVIDENCE_STATUS:

READY_TO_WRITE_FORMAL_CHAPTER_2:
YES / NO

CHAPTER_6_METHOD_SUPPORT:
READY / READY_WITH_LIMITATIONS / NOT_READY

CHAPTER_7_METHOD_SUPPORT:
READY / READY_WITH_LIMITATIONS / NOT_READY

CHAPTER_8_METHOD_SUPPORT:
READY / READY_WITH_LIMITATIONS / NOT_READY

SAFE_CLAIMS_READY:

CLAIMS_STILL_FORBIDDEN:

UNRESOLVED_CRITICAL_GAPS:

FILES_CREATED:

==================================================
三十四、停止条件
==================================================

完成 Stage E 后停止。

不要自动：

写正式第2章
改第6—8章正文
调用 DSH
生成整稿
联系开发线
修改开发仓库

即使：

READY_TO_WRITE_FORMAL_CHAPTER_2 = YES

也必须先停止，等待外部审核。

现在开始执行 Stage E。
~~~~

## 消息 2044 / user / 分段 1 of 1

~~~~text
梳理一下目前工作流
~~~~

## 消息 2045 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "reasoning_recap",
  "content": "思考了 7s"
}
~~~~

## 消息 2046 / assistant / 分段 1 of 1

~~~~text
可以。现在整个 DG-202611 项目已经逐渐形成一套比较清晰的 **“我管方向 → 文档线管证据 → 开发线做事实 → 硬件线补实物 → 最后统一收口”** 的工作流。

### 总体结构

```text
你
↓
我（总控 / PM / 外部审核）
├─ 决定下一阶段做什么
├─ 审核各条线结果
├─ 防止夸大、跑偏、重复造轮子
├─ 给 Claude / 文档线程 / ZCode 下任务
└─ 决定什么时候允许进入下一 Gate
        │
        ├───────────────┬────────────────┬───────────────┐
        ↓               ↓                ↓               ↓
   DEVELOPMENT_LINE   DOCUMENT_LINE   HARDWARE_LINE   ZCODE_RESEARCH
   Claude Code        文档线程          实物/老师         GLM Flash
```

### 1. 开发线 DEVELOPMENT_LINE

负责：

```text
真正写代码
→ 测试
→ ROS2运行
→ synthetic验证
→ Gazebo
→ Jetson
→ 实车
→ 产生真实实验数据
```

核心原则：

> **开发线负责“做出事实”，不能负责把没有做过的东西写成成果。**

当前开发进度：

```text
原始代码 / 单测
      ↓
ROS2 Runtime
      ↓
S01–S04 synthetic validation ✅
      ↓
S05–S08 precheck ✅
      ↓
RViz / Monitor 可视化验证  ← 当前这里
      ↓
S05 → S06 → S07 → S08 FINAL EVIDENCE
      ↓
独立审计
      ↓
commit + push
      ↓
Package A 冻结
      ↓
Gazebo P0
      ↓
物理仿真
      ↓
Jetson
      ↓
实车
```

现在开发线只差：

**RViz文字布局收尾 → 你再看一眼 → 正式跑 S05–S08。**

然后 synthetic 这部分就不再无限打磨，马上进入 **Gazebo**。

---

### 2. 文档线 DOCUMENT_LINE

负责：

```text
比赛要求
→ 技术事实
→ 文献
→ 指标边界
→ 方法论证据
→ 正式方案书
→ 最终材料
```

但它不能：

```text
改代码
联系开发线自己指挥开发
把文献结果写成我们的结果
把仿真写成实车
```

目前文档线流程：

```text
Stage A/B
初步框架、技术路线
      ↓
Stage C
60条真实来源 + 20篇核心论文
      ↓
Stage D
全文精读 + 性能数字抽取
      ↓
Stage E  ← 当前正在做
高优先级证据缺口闭合
      ↓
正式 Chapter 2 Gate
      ↓
第2章国内外研究现状
+
第6–8章方法论引用补强
      ↓
等待开发实验数据
      ↓
第9–10章实验与指标
      ↓
最终整稿
```

Stage E 现在重点查：

```text
高程 <20 cm
特征重复率 ≥95%
重定位成功率 >95%
UWB
毫米波雷达
事件相机
自适应融合
```

也就是说：

**文档线现在还不是“疯狂写正文”，而是在把以后能写什么、不能写什么彻底查清楚。**

---

### 3. 硬件线 HARDWARE_LINE

这个现在也越来越重要。

当前事实：

```text
已有：
轮式里程计
IMU
北斗 / GNSS / RTK
LiDAR
ZED 2i
UWB

准备采购：
毫米波雷达
事件相机

未来展望：
偏振光 / 天基光学导航

不进入当前主线：
ToF
光流
地面纹理
```

硬件线以后流程：

```text
拿到实物
↓
拍照
↓
确认型号
↓
说明书 / SDK
↓
接口
↓
Ubuntu可读
↓
ROS2 topic
↓
时间同步 / 外参标定
↓
数据质量指标
↓
融合
↓
实测
```

例如 UWB：

```text
老师确认有硬件 ✅
↓
拿到实物
↓
型号？
↓
基站/标签？
↓
接口？
↓
能否读取？
↓
信号质量怎么判断？
↓
正常 → 使用
退化 → 降权
不可用 → 拒绝
```

---

### 4. ZCode + GLM-5.3-Flash

这条线**不是正式文档线，也不是开发线。**

它更像：

> 大规模廉价研究/审计劳动力。

适合干：

```text
论文广搜
论文初筛
论文结构化提取
仓库扫描
代码审计
测试漏项检查
技术知识库加工
```

不适合让它最终决定：

```text
项目架构
核心算法修改
关键性能结论
正式成果口径
```

以后所有提示词第一行都会要求它明确：

```text
TASK_DIRECTION:
ZCODE_RESEARCH_LINE
```

避免再和 DOCUMENT_LINE 混起来。

---

## 最重要的证据分层

整个项目以后统一按这个：

```text
老师确认
    ↓
TEACHER_CONFIRMED

实物存在
    ↓
HARDWARE_EXISTS

仓库有实现
    ↓
REPOSITORY_PROVEN

接口能读
    ↓
INTERFACE_VERIFIED

ROS2数据通
    ↓
ROS2_DATA_VERIFIED

算法完成
    ↓
ALGORITHM_IMPLEMENTED

synthetic验证
    ↓
SYNTHETIC_VERIFIED

Gazebo验证
    ↓
SIMULATION_VERIFIED

实车验证
    ↓
REAL_ROBOT_VERIFIED

达到具体指标
    ↓
PERFORMANCE_VERIFIED
```

不能跨级。

例如：

```text
UWB有硬件
≠
UWB已经完成融合
≠
UWB已经达到20cm
```

---

## 性能数字也统一分五类

这个以后是铁规则：

```text
LITERATURE_REPORTED
文献报道

COMPETITION_REQUIREMENT
比赛要求

PROJECT_DESIGN_TARGET
项目设计目标

SIMULATION_MEASURED
仿真测量

REAL_ROBOT_MEASURED
实车测量
```

**五类绝不能混。**

比如：

> `<20 cm`

现在只能属于：

`COMPETITION_REQUIREMENT`

还不能属于：

`REAL_ROBOT_MEASURED`

---

## 每个技术包的标准工作流

以后一个功能统一走：

```text
1. 需求/比赛要求
        ↓
2. 文献依据
        ↓
3. 技术方案
        ↓
4. 代码实现
        ↓
5. 单元测试
        ↓
6. ROS2运行验证
        ↓
7. Synthetic
        ↓
8. Gazebo
        ↓
9. 实物接入
        ↓
10. 实车实验
        ↓
11. 指标统计
        ↓
12. 独立审核
        ↓
13. 文档正式写入
```

而不是以前那种：

```text
想到一个技术
↓
先写进方案
↓
以后再想办法证明
```

现在彻底反过来了：

> **先产生证据，再升级文档口径。**

---

# 你现在各条线的位置

```text
开发线
████████████████░░░
Round B基本结束
→ RViz收尾
→ Final Evidence
→ Gazebo

文档线
██████████████░░░░░
Stage D完成
→ Stage E证据缺口闭合
→ 再决定正式第2章

硬件线
████████░░░░░░░░░░░
现有机器人基础清楚
→ UWB拿实物
→ 毫米波/Event采购

最终比赛指标
███░░░░░░░░░░░░░░░░
还早
→ Gazebo
→ 实车
→ 才能证明20cm / 95%
```

所以**现在真正的主节奏**就是：

```text
开发：
Final Evidence → Package A → Gazebo

文档：
Stage E → Chapter 2 Gate

硬件：
UWB确认 → 毫米波/Event选型采购

我：
持续审两边 → 防止口径超过证据
```

这套流程基本已经可以作为我们后面整个比赛项目的固定 SOP 了。memcite
~~~~

## 消息 2047 / user / 分段 1 of 1

~~~~text
记住哈，文档线那边还有控制dsh写文档的部分（我复制粘贴给dsh）
~~~~

## 消息 2048 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "reasoning_recap",
  "content": "思考了几秒"
}
~~~~

## 消息 2049 / assistant / 分段 1 of 1

~~~~text
记住了。

以后我会把**文档线**明确理解成两层职责，而不是只有“查论文/管证据”：

```text
DOCUMENT_LINE
├─ 证据与研究管理
│  ├─ 文献检索
│  ├─ 论文核验
│  ├─ 指标边界
│  ├─ 技术事实对账
│  └─ Claim-Evidence 管理
│
└─ DSH 文档生产管理
   ├─ 我/文档线程制定写作要求
   ├─ 生成给 DSH 的提示词
   ├─ 你复制粘贴给 DSH
   ├─ DSH 负责长篇正式写作
   ├─ 结果再回到文档线审核
   └─ 不合格则继续给 DSH 修订指令
```

也就是说以后流程是：

**文档线先把“事实和证据”查清楚 → 再由文档线生成 DSH 提示词 → 你复制给 DSH → DSH 写正文 → 文档线审稿 → 我做最终 Gate。**

所以今后我写提示词时，会明确区分：

`DSH_USED = NO`  
表示本阶段还只是在研究/审计；

和：

`DSH_WRITING_STAGE = YES`  
表示已经允许文档线组织 DSH 正式写作。

并且 **DSH 不是另一条独立项目线，它属于 DOCUMENT_LINE 管辖的“长文写作执行器”**。这个以后不再漏掉。
~~~~

## 消息 2050 / user / 分段 1 of 1

~~~~text
不不不，dsh提示词我是找codex的文档线要的啊，之前记忆你开始忘记了吗
~~~~

## 消息 2051 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "reasoning_recap",
  "content": "思考了 6s"
}
~~~~

## 消息 2052 / assistant / 分段 1 of 1

~~~~text
对，这里是我刚刚说错了，**不是你直接找我拿 DSH 提示词**。之前已经定好的工作流我应该保持住。

正确关系是：

```text
你
↓
我（总控 / 审核 / 决定阶段目标）
↓
Codex 文档线
《查看比赛提交要求》
├─ 管文献与证据
├─ 管技术事实口径
├─ 管章节结构和写作要求
├─ 审核已有正文
├─ 决定什么时候可以写
└─ 生成 DSH 写作提示词
          ↓
       你复制粘贴
          ↓
         DSH
       长篇正式写作
          ↓
       结果给回 Codex 文档线
          ↓
       Codex 审核 / 再给修订提示词
```

所以 **DSH 是文档线控制的写作执行器**，但实际操作方式是：

> **Codex 文档线给你 DSH 提示词 → 你复制粘贴到 DSH。**

不是我绕过 Codex 文档线直接给 DSH 下任务。

而我这里主要负责：

```text
决定文档线下一阶段干什么
→ 给 Codex 文档线任务
→ 审核 Codex 返回的阶段成果
→ 决定是否允许进入 DSH 写作阶段
```

比如现在：

```text
我
↓
让 Codex 文档线做 Stage E
↓
Codex 完成证据缺口闭合
↓
你把结果发给我
↓
我审核 PASS
↓
我再让 Codex 文档线进入“正式第2章写作准备”
↓
Codex 根据它掌握的全部证据
生成专门给 DSH 的提示词
↓
你复制给 DSH
```

这样最好，因为 **Codex 文档线掌握着本地 Stage C/D/E 文件、章节文件、证据矩阵和版本状态**，它比我直接凭聊天上下文生成一份 DSH 提示词更适合做“最后一公里”的写作指令。

以后我会固定区分四个角色：

- **我：MASTER / PM / 外部审核**
- **Codex：DOCUMENT_LINE，且负责生成和管理 DSH 提示词**
- **DSH：DOCUMENT_WRITER，只负责长文成稿**
- **Claude Code：DEVELOPMENT_LINE**

你刚才纠正得对，我前一条确实把这层关系说偏了。后面按原来的工作流走，不改。memcite
~~~~

## 消息 2053 / user / 分段 1 of 1

~~~~text
好，梳理好，后续不要搞错了
~~~~

## 消息 2054 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "reasoning_recap",
  "content": "思考了 5s"
}
~~~~

## 消息 2055 / assistant / 分段 1 of 1

~~~~text
好，后续我就按这套固定关系走，不再混线：

- **我**：总控 / PM / 外部审核，决定阶段目标、审核结果、开下一 Gate。
- **Codex 文档线《查看比赛提交要求》**：负责文献、证据、技术事实、章节管理，以及**生成给 DSH 的写作/修订提示词**。
- **DSH**：文档线下属的长文写作执行器；**Codex 给提示词 → 你复制粘贴给 DSH → 结果回 Codex 审核**。
- **Claude Code 开发线**：负责代码、ROS2、测试、Synthetic、Gazebo、Jetson、实车和实验事实。
- **ZCode / GLM**：单独的辅助研究/审计劳动力池，不自动等于文档线，也不负责最终口径。

以后每个长提示词我都会要求第一行明确：

`TASK_DIRECTION: DOCUMENT_LINE / DEVELOPMENT_LINE / ZCODE_RESEARCH_LINE`

其中 **DOCUMENT_LINE 天然包含 DSH 管理职责**，但不会未经 Codex 文档线直接跳过去指挥 DSH。

后面就以这个为固定 SOP。memcite
~~~~

## 消息 2056 / user / 分段 1 of 1

~~~~text
Stage E 已完成，未修改正式章节、未联系开发线、未使用 DSH。

已创建 16 个文件，目录：

[STAGE\_E\_高优先级证据缺口闭合\_v1.0]\(C:\\\Users\\\peng\\\Desktop\\\材料\\\DG-202611比赛材料文档\\\STAGE\_E\_高优先级证据缺口闭合\_v1.0)

核心结论：

- 13 篇摘要级论文中恢复 9 篇；
- 核心实验全文核验累计 15 篇；
- 与比赛指标直接可比的文献数：0；
- 高程 20 cm：仅间接证据；
- 特征重复率 95%：无直接等价指标；
- 重定位成功率 95%：无直接支持；
- UWB、毫米波、事件相机：可支撑候选方法研究，不能写成已集成；
- `FORMAL_BODY_MODIFIED=NO`
- `DEVELOPMENT_LINE_CONTACTED=NO`
- `DSH_USED=NO`

重点查看：

[README\_STAGE\_E\_v1.0.md]\(C:\\\Users\\\peng\\\Desktop\\\材料\\\DG-202611比赛材料文档\\\STAGE\_E\_高优先级证据缺口闭合\_v1.0\\\README\_STAGE\_E\_v1.0.md)

[11\_STAGE\_E\_CHAPTER\_READINESS\_GATE\_v1.0.md]\(C:\\\Users\\\peng\\\Desktop\\\材料\\\DG-202611比赛材料文档\\\STAGE\_E\_高优先级证据缺口闭合\_v1.0\\\11\_STAGE\_E\_CHAPTER\_READINESS\_GATE\_v1.0.md)
~~~~

## 消息 2057 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "thoughts",
  "thoughts": [
    {
      "summary": "评估阶段 E",
      "content": "我正在根据报告核对推进条件。现有文献数量和方法覆盖度已基本满足第二章写作，但直接可比实验仍不足，因此正式写作时必须明确区分文献事实、方法启发与尚未验证的研究设想。",
      "chunks": [],
      "finished": true
    },
    {
      "summary": "评估了阶段E",
      "content": "",
      "chunks": [],
      "finished": true
    }
  ],
  "source_analysis_msg_id": "ef4ce6e0-8897-4668-a4ee-b903f0e2e6b7"
}
~~~~

## 消息 2058 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "reasoning_recap",
  "content": "思考了 9s"
}
~~~~

## 消息 2059 / assistant / 分段 1 of 1

~~~~text
这次可以正式过审。

**TASK_DIRECTION: DOCUMENT_LINE**  
**STAGE_E_STATUS: PASS_WITH_LIMITATIONS**

这次的 `LIMITATIONS` 不是因为工作没做完，而是因为研究得到的真实结论本身就是：**比赛几个核心指标目前没有可以直接一一对应的论文证据。** 这个结果反而很重要，不能为了追求 PASS 去硬找“看起来差不多”的数字。

现在可以冻结几个结论：

- 13 篇摘要级核心论文追回 9 篇，累计 **15 篇完成全文实验核验**，已经达到我们之前给正式研究现状写作设的最低 Gate。
- **高程 <20 cm**：只能作为比赛要求/设计目标，不能包装成已有文献直接证明，更不能写成项目已达到。
- **特征重复率 ≥95%**：没有完全等价的标准学术指标，后面必须在实验设计里明确我们自己的测量定义，不能拿 tracking rate / inlier ratio 偷换。
- **重定位成功率 >95%**：同样没有直接闭环可比文献，后续必须靠自己的多次实验统计。
- **UWB**：项目已有硬件这一事实保留，但接口、ROS2、融合、性能仍未验证。
- **毫米波 / Event**：文献已经足够说明为什么值得引入，但仍属于计划采购/集成，不能写成项目既有成果。
- “直接可比文献 = 0” **不妨碍写第2章**。第2章的任务是说明国内外研究现状、技术路线、局限和我们的研究切入点，不是证明我们已经达到比赛指标。

所以这里我会把之前的 Gate 升级为：

**READY_TO_WRITE_FORMAL_CHAPTER_2 = YES**

但带一个硬限制：

> 第2章可以正式写“研究现状、代表性方法、已有性能及其测试条件、现有不足”；  
> 不允许把文献性能数字转换成我们的项目性能，也不能为了跟比赛 20 cm / 95% 对齐而硬凑论文。

同时：

**CHAPTER_6_METHOD_SUPPORT = READY**  
**CHAPTER_7_METHOD_SUPPORT = READY**  
**CHAPTER_8_METHOD_SUPPORT = READY_WITH_LIMITATIONS**

其中第8章主动重定位的方法论可以补强，但 `>95%` 仍然只能留给后续实验。

### 文档线下一步

现在不需要 Stage F 再继续漫无目的搜论文了。

应该正式进入：

**DOCUMENT FORMAL WRITING GATE — Chapter 2 + Chapter 6–8 Citation Reinforcement**

但按照我们已经定好的工作流：

```text
我批准进入正式写作
        ↓
Codex 文档线
读取 Stage C + D + E
以及现有章节版本
        ↓
先确定 Chapter 2 的最终写作结构和证据映射
        ↓
Codex 生成给 DSH 的正式写作提示词
        ↓
你复制给 DSH
        ↓
DSH 长篇成稿
        ↓
成稿回 Codex 文档线审核
        ↓
必要时 Codex 再生成 DSH 修订提示词
```

**不是我现在直接跳过 Codex 给 DSH 提示词。**

另外第6–8章也不要和第2章一次性全扔给 DSH。建议先把 **第2章单独完成并审核**，确定文献引用和写作风格没问题以后，再让文档线组织第6–8章补强。

目前文档方向可以正式冻结为：

```text
STAGE_C = FROZEN
STAGE_D = FROZEN
STAGE_E = FROZEN / PASS_WITH_LIMITATIONS

LITERATURE_EVIDENCE_BASE = READY
FORMAL_CHAPTER_2 = AUTHORIZED
CH6_7_METHOD_CITATIONS = AUTHORIZED
CH8_METHOD_CITATIONS = AUTHORIZED_WITH_LIMITATIONS

PROJECT_PERFORMANCE_CLAIMS = NOT_AUTHORIZED
```

下一条就可以给 **Codex 文档线**发任务，让它开始“第2章正式写作准备 + 生成 DSH 提示词”。memcite
~~~~

## 消息 2060 / user / 分段 1 of 1

~~~~text
给我
~~~~

## 消息 2061 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "reasoning_recap",
  "content": "思考了 6s"
}
~~~~

[本对话目录](README.md) | [总目录](../../README.md) | [Raw](https://raw.githubusercontent.com/weiyang02520-ops/dg202611-conversation-archive/main/conversations/first/part-058.md) | [上一卷](part-057.md) | [下一卷](part-059.md)
