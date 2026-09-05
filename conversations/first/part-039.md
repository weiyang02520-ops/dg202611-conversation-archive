# 机器人定位挑战赛分析 / 第 039 卷

[本对话目录](README.md) | [总目录](../../README.md) | [Raw](https://raw.githubusercontent.com/weiyang02520-ops/dg202611-conversation-archive/main/conversations/first/part-039.md) | [上一卷](part-038.md) | [下一卷](part-040.md)

## 消息 1679 / user / 分段 1 of 1

~~~~text
CURRENT\_DOCUMENT\_STATUS: 已生成当前整稿预览；正文实际写到第8章，不是最终投稿版。

CURRENT\_VALID\_CHAPTER\_FILES:

- 第1—2章：`v1.1`调研框架
- 第3—5章：`v2.2.1`
- 第6—8章：`v1.2`

HISTORICAL\_FILES\_PRESERVED: YES，包括第3—5章v2.1冻结基线、v2.2纠偏前版本及其他历史稿。

CURRENT\_DOCUMENT\_STRUCTURE: 采用12章结构；第6—8章沿用文档线已批准标题。第9—12章、参考文献和附录为明确占位。

FULL\_PREVIEW\_FILE:

- [Markdown整稿预览]\(C:/Users/peng/Desktop/材料/DG-202611比赛材料文档/DG-202611\_方案书当前整稿预览\_vCurrent.md)
- 格式：Markdown
- 状态：可连续阅读，完整保留第1—8章内容

DOCX\_PREVIEW:

- generated: YES
- [Word整稿预览]\(C:/Users/peng/Desktop/材料/DG-202611比赛材料文档/DG-202611\_方案书当前整稿预览\_vCurrent.docx)

CHAPTER\_STATUS:

- CHAPTER\_1：NEEDS\_FORMAL\_WRITING；当前是调研框架
- CHAPTER\_2：WAITING\_FOR\_RESEARCH；缺真实文献检索
- CHAPTER\_3：READY\_FOR\_POLISH
- CHAPTER\_4：READY\_FOR\_POLISH
- CHAPTER\_5：READY\_FOR\_POLISH
- CHAPTER\_6：WAITING\_FOR\_DEVELOPMENT\_EVIDENCE
- CHAPTER\_7：WAITING\_FOR\_DEVELOPMENT\_EVIDENCE
- CHAPTER\_8：WAITING\_FOR\_DEVELOPMENT\_EVIDENCE
- CHAPTER\_9：NOT\_STARTED
- CHAPTER\_10：WAITING\_FOR\_DEVELOPMENT\_EVIDENCE
- CHAPTER\_11：WAITING\_FOR\_DEVELOPMENT\_EVIDENCE
- CHAPTER\_12：NOT\_STARTED
- REFERENCES：WAITING\_FOR\_RESEARCH
- APPENDIX：NOT\_STARTED

CONSISTENCY\_CHECK: PASS。硬件、二维EKF、POC、BDS/短报文及候选技术边界保持一致。

TRUTHFULNESS\_CHECK: PASS。未写入S01—S08通过、实车完成或20cm/95%/95%指标达成结论。

TEACHER\_FEEDBACK\_INTEGRATION: 已保留“常规定位＋创新定位＋北斗/BDS”的整体体系；候选技术未升级为已实现。

CURRENT\_DOCUMENT\_GAPS:

- [逐章缺口报告]\(C:/Users/peng/Desktop/材料/DG-202611比赛材料文档/CURRENT\_DOCUMENT\_GAP\_REPORT.md)
- 当前文档工作量约完成55%—60%，不代表技术完成度。

WAITING\_FOR\_DEVELOPMENT\_PACKAGES: DEVELOPMENT PACKAGE A（S01—S08）、Gazebo、Jetson、真车、比赛指标、GNSS→map、北斗短报文证据。

WAITING\_FOR\_RESEARCH: 阶段C真实论文、官方资料、引用矩阵和正式参考文献。

DSH\_USED: NO
DSH\_ACTIONS: 无
FILES\_CREATED: 3
FILES\_MODIFIED: 0个既有源文件
EXISTING\_SOURCE\_FILES\_OVERWRITTEN: NO
READY\_FOR\_USER\_REVIEW: YES，，，，，，，，，，目前看起来效果不是特别好，还是得等后面啊
~~~~

## 消息 1680 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "model_editable_context",
  "model_set_context": ""
}
~~~~

## 消息 1685 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "reasoning_recap",
  "content": "思考了 13s"
}
~~~~

## 消息 1686 / assistant / 分段 1 of 1

~~~~text
对，现在这个“看起来不够好”其实很正常。你这份整稿目前更像是**把技术骨架拼起来给我们检查方向**，还不是一份真正拿去比赛看的成品。

我看了你上传的 Word，问题主要不是“写错了”，而是**成品感还没形成**：

- 第1—2章明确还是“调研框架”，文档自己也标了“待正式成文化”。fileciteturn253file0L34-L38
- 第3—5章虽然已经比较完整，但里面还有大量“待绘制图3-1、待完善表3-1、待完善表5-1、待绘制图5-1”等占位。
- 第6—8章现在更偏“模型设计稿”，技术味很重，但还缺真正实验结果把它撑起来。
- 第9—12章、参考文献、附录目前基本都是占位，所以目录看起来完整，实际上后半本还没长出来。fileciteturn253file0L42-L55
- 当前版本自己也明确说明：它只是“当前文档工作量预览”，不是最终投稿版。fileciteturn253file0L2-L9

所以我建议**现在不要花很多时间去精修这份 Word**。

目前值得做的只有少量“不会被后续开发推翻”的工作：

**可以现在做：**
- 第1章从框架改成正式文字
- 第2章开始真实论文检索
- 把总体架构图、技术路线图、ROS2数据流图先画出来
- 把已有硬件表、软件表、成熟度表整理漂亮
- 统一术语和排版

**最好等后面再做：**
- 第6—8章最终润色
- 第9章工程实现
- 第10章实验评价
- 第11章样机/北斗短报文
- 大量实验图、曲线、截图、Gazebo图、真车图
- 最终创新点提炼
- 最终摘要/结论
- 整体比赛版美化

真正会让方案书“突然变好看”的，其实是后面这些东西进来：

```text
S01～S08完整证据
↓
真实机器人参数审计
↓
Gazebo机器人仿真图
↓
轨迹 / ground truth / 误差曲线
↓
ZED / 高程 / 新技术实验
↓
Jetson运行证据
↓
真车照片与实验
↓
指标统计表
```

到了那个阶段，第6—10章就不再是“我们准备怎么做”，而会变成：

> **为什么这样设计 → 怎么实现 → 实验怎么做 → 结果是什么 → 图在哪里 → 有什么改进。**

那时候整本方案书的观感会完全不一样。

所以现在这份 **55%～60% 更准确地说是“文档骨架和技术叙述完成度”**，不是“成品观感已经有60%”。

我更倾向于现在把它当成我们的**内部母稿**，先别追求漂亮。

等 **技术包 A + 第一轮真实文献研究** 完成后，我们再做一次明显的大升级；等 Gazebo 和后续开发证据进来，再做第二次升级。最后真车和指标出来，再做最终投稿版。

现在看到它“不够像比赛成品”，反而说明我们没有用大量空话和假实验把52页硬塞满。这个阶段先这样是合理的。memcite
~~~~

## 消息 1687 / user / 分段 1 of 1

~~~~text
我们是不是还要叫codex多去找点文献啥的啊
~~~~

## 消息 1688 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "reasoning_recap",
  "content": "思考了 6s"
}
~~~~

## 消息 1689 / assistant / 分段 1 of 1

~~~~text
要，而且我觉得这件事**现在就可以并行做**，不用等开发线。

因为你当前文档里，第2章自己就明确写着“尚未开展阶段 C 真实文献检索”，目前只是检索框架；参考文献也还没正式形成。fileciteturn253file0L80-L93

我建议文档 Codex 现在单独开一条“文献研究线”，但不要急着改正文，先做这几件事：

- 找真实论文：BDS/GNSS拒止、LiDAR退化、多源融合、VIO、毫米波雷达、主动重定位、动态目标LiDAR、机器人高程定位等。
- 找一手官方资料：北斗白皮书/标准、GNSS/RTK官方资料、ROS2/Nav2/SLAM Toolbox、传感器厂商手册。
- 每篇文献都记录：作者、题目、年份、期刊/会议、DOI/链接、核心方法、实验条件、能支撑我们哪一章、能不能支撑具体数字。
- 做“引用矩阵”：哪篇支撑第2章现状，哪篇支撑第6章方法，哪篇支撑第7章动态目标，哪篇支撑第8章主动重定位。
- 特别找**近5年高质量论文 + 几篇经典奠基论文**，避免全是老文献或者全是综述。
- 老师给的AI材料只能当“搜索线索”，不能直接当引用来源。你现在文档里也已经把这个规则写死了。fileciteturn253file0L178-L183

最重要的是：

> **先建立“文献证据库”，再写第2章。**

不要让 Codex 一边搜两篇，一边就开始编“国内外研究现状”，这样很容易又变成AI作文。

所以现在两条线完全可以并行：

**Claude Code：继续开发技术包 A。**  
**文档 Codex：开始真实文献检索 + 引用矩阵。**

等两边都有完整成果后，再一起进下一轮方案书升级。

如果你要，我下一条可以直接给你一份**文档 Codex 的“阶段C真实文献检索”完整提示词**，让它开始干这个。memcite
~~~~

## 消息 1690 / user / 分段 1 of 1

~~~~text
提示词吧
~~~~

## 消息 1691 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "thoughts",
  "thoughts": [
    {
      "summary": "规划文献检索",
      "content": "我将围绕GNSS/BDS退化、激光雷达退化与动态场景、多传感器自适应融合、主动重定位、VIO高度约束、毫米波雷达和北斗短报文，系统检索约40—60篇高质量文献及官方资料，优先保证真实性与章节对应关系。",
      "chunks": [],
      "finished": true
    },
    {
      "summary": "规划了文献检索方案",
      "content": "",
      "chunks": [],
      "finished": true
    }
  ],
  "source_analysis_msg_id": "b5062513-a455-4c51-b2bd-bc9671ed6ffe"
}
~~~~

## 消息 1692 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "reasoning_recap",
  "content": "思考了 22s"
}
~~~~

## 消息 1693 / assistant / 分段 1 of 2

~~~~text
执行位置：当前线程《查看比赛提交要求》，请在本线程直接执行；本线程是 DG-202611 文档线与文献研究线。本轮不要转发开发线程，不要要求 Claude Code 做任何开发任务。

DG-202611 STAGE C REAL LITERATURE RESEARCH
阶段 C：真实文献检索、核验与引用证据库建设

本轮任务不是继续扩写方案书正文，而是正式启动此前一直缺失的“阶段 C 真实文献研究”。

当前文档状态：

第1—2章：
`01_第1-2章调研框架_老师意见同步_v1.1.md`

第3—5章：
`05_第3-5章初稿_v2.2.1_老师意见同步.md`

第6—8章：
`06_第6-8章模型初稿_v1.2.md`

当前整稿预览：
`DG-202611_方案书当前整稿预览_vCurrent.md`
`DG-202611_方案书当前整稿预览_vCurrent.docx`

当前第2章明确仍属于“调研框架”，尚未形成经过真实论文支撑的国内外研究现状。

本轮必须先建立可靠文献证据库，再考虑正式写第2章。

==================================================
一、本轮总目标
==================================================

完成：

真实论文检索
+
官方资料检索
+
论文真实性核验
+
核心论文精读
+
引用矩阵
+
章节支撑关系
+
研究缺口分析
+
候选技术证据整理
+
正式参考文献候选库

本轮优先追求：

“来源真实、可追溯、能支撑方案书具体论述”

而不是单纯追求文献数量。

禁止：

AI凭记忆生成论文

杜撰 DOI

杜撰作者

杜撰期刊

杜撰实验数据

杜撰结论

看到二手文章后直接当论文引用

用老师提供的 AI Word 作为正式引用来源。

==================================================
二、首先读取当前文档
==================================================

开始检索前，必须实际阅读当前有效文档，至少理解：

第1章提出的问题；

第2章现有调研框架；

第3章参赛样机与技术基础；

第4章总体研究思路；

第5章系统架构；

第6章 BDS 中断 + LiDAR 退化并发多源定位模型；

第7章动态目标干扰下 LiDAR 特征；

第8章具身主动重定位。

不要脱离当前方案另起一套研究方向。

文献研究的目的，是：

支撑当前 DG-202611 技术体系

以及帮助判断尚未确定的候选技术。

==================================================
三、文献检索主题
==================================================

至少覆盖以下八个研究簇。

A. BDS / GNSS / RTK 退化、拒止与恢复

重点：

BDS/GNSS denied environment

GNSS outage

GNSS degradation

multipath

NLOS

GNSS integrity

RAIM

fault detection

GNSS/INS integrated navigation

GNSS reacquisition

GNSS gradual recovery

RTK degradation

BDS positioning

BDS/GNSS 与轮速、IMU、LiDAR 的融合。

重点关注：

GNSS异常如何识别；

中断期间如何传播；

恢复后为什么不能直接硬跳回；

渐进重新接入；

质量指标如何参与融合。

--------------------------------------------------

B. 多源定位、自适应融合与故障感知

重点：

adaptive sensor fusion

fault-aware fusion

robust sensor fusion

multi-sensor localization

EKF / UKF / ESKF

factor graph

sliding window

adaptive covariance

innovation gating

residual gating

confidence-based fusion

quality-aware fusion

sensor fault detection and isolation

uncertainty-aware localization。

特别寻找：

“观测质量 → 置信度/协方差 → 动态权重 → 异常拒绝”

这一类能够支撑第6章方法设计的论文。

--------------------------------------------------

C. LiDAR退化、几何可观测性与鲁棒特征

重点：

LiDAR degeneracy

geometric degeneracy

LiDAR localization degradation

LiDAR SLAM degeneracy

feature repeatability

geometric observability

scan matching quality

ICP degeneracy

point cloud registration confidence

corridor degeneracy

sparse geometry

dynamic scene LiDAR。

重点寻找：

如何量化 LiDAR 几何退化；

角度覆盖、特征数量、几何约束与匹配可靠性的关系；

Scan-to-Map 匹配质量评价；

退化检测。

--------------------------------------------------

D. 动态目标环境下 LiDAR 特征

重点：

dynamic object removal

dynamic SLAM

dynamic point cloud filtering

temporal consistency

LiDAR motion segmentation

static feature extraction

dynamic environment localization

semantic / geometric dynamic filtering。

重点支撑第7章：

相邻帧稳定性；

动态候选；

静态结构保留；

特征复现；

动态目标对定位的影响。

注意：

不要为了匹配当前代码而硬找论文。

要真实判断：

当前第7章方法与现有研究之间到底是什么关系。

--------------------------------------------------

E. 主动重定位 / Active Localization / Active Perception

重点：

active localization

active relocalization

active perception

belief-space planning

localization recovery

kidnapped robot problem

relocalization recovery

information gain navigation

active sensing

viewpoint selection for localization

robot motion for localization recovery。

重点寻找：

机器人为什么可以通过主动运动改善定位；

旋转扫描；

候选位姿；

多帧验证；

恢复状态机；

信息增益；

失败后安全停止/人工接管。

这是第8章最重要的文献方向之一。

--------------------------------------------------

F. VIO / ZED 2i / 三维定位 / 高程

重点：

visual inertial odometry

VIO

stereo visual inertial odometry

3D robot localization

vertical position estimation

altitude estimation

GNSS/VIO fusion

LiDAR/VIO fusion

wheel/IMU/VIO fusion

robot elevation estimation。

优先关注：

VINS-Mono / VINS-Fusion

ORB-SLAM3

OpenVINS

Kimera

ZED positional tracking

以及其他高质量系统。

但必须区分：

论文算法

和

ZED 2i 产品能力。

ZED 产品功能只能由 Stereolabs 官方资料支撑。

特别研究：

VIO能否补充 Z 方向；

尺度问题；

漂移；

算力；

坐标系；

室内外稳定性；

是否适合作为 DG-202611 下一阶段候选观测源。

本轮只研究。

不得写成项目已经采用。

--------------------------------------------------

G. 毫米波雷达定位

重点：

mmWave radar odometry

radar inertial odometry

radar SLAM

4D radar localization

FMCW radar odometry

radar-LiDAR fusion

radar-camera fusion

radar robust localization

Doppler velocity estimation。

关注：

雷达在：

尘雾

弱光

强光

LiDAR退化

视觉退化

条件下的互补性。

同时分析：

ROS2支持

数据类型

Doppler能力

点云稀疏性

硬件成本

算法复杂度

Jetson算力需求。

本轮仍属于技术候选研究。

不得直接宣布采购或接入。

--------------------------------------------------

H. 北斗短报文

北斗短报文必须独立于定位算法研究。

重点：

北斗三号短报文

Regional Short Message Communication

Global Short Message Communication

北斗短报文终端

短报文协议

应急通信

机器人 / 无人系统短报文应用。

优先：

北斗卫星导航系统官方网站

官方 ICD

官方白皮书

国家标准

设备厂商正式规格书。

严格区分：

BDS定位

RTK差分

4G

LoRa

NTRIP

和

北斗短报文通信。

==================================================
四、时间范围
==================================================

文献组合采用：

经典文献
+
近年高质量研究。

近年研究重点：

2019—2026。

但不要为了年份强行排除：

AMCL

经典 EKF

经典 SLAM

经典 active localization

经典 VIO

等奠基工作。

目标结构：

约 20% 经典/基础文献

约 80% 近年文献/官方资料。

==================================================
五、文献数量目标
==================================================

本轮目标建立：

40—60 条经过真实性核验的高价值资料。

不是要求每个方向机械凑数量。

优先保证：

A/B/C/D/E

五个与当前核心模型直接相关的方向。

建议最低：

BDS/GNSS退化与融合：8篇左右

多源自适应融合：8篇左右

LiDAR退化/可观测性：8篇左右

动态环境LiDAR：6篇左右

主动重定位/主动感知：8篇左右

VIO/高程：6篇左右

毫米波雷达定位：6篇左右

北斗短报文：官方/标准资料若干。

允许一篇论文同时支撑多个方向。

最终按“唯一文献”计数，不重复凑数。

如果高质量资料不足：

宁可少于60。

禁止灌水。

==================================================
六、来源优先级
==================================================

优先级 1：

论文原始页面

出版社

IEEE Xplore

Springer

Elsevier

Wiley

ACM

Nature / Science 系列

期刊官网

会议官网

作者正式公开版本。

优先级 2：

arXiv

仅在可以确认：

标题

作者

版本

论文内容

时使用。

如果已有正式发表版本：

优先引用正式版本。

优先级 3：

官方技术资料：

北斗官网

ROS2官方文档

Nav2官方文档

SLAM Toolbox官方资料

Stereolabs官方资料

传感器厂家正式 datasheet/manual。

优先级 4：

GitHub原始项目

只用于：

软件实现

接口

开源能力

版本

工程事实。

不能单独作为学术性能结论依据。

禁止作为最终引用来源：

知乎

CSDN

微信公众号

博客搬运

百度百科

AI回答

AI生成Word

论文营销页面

二手转载。

这些最多用于找到原始来源。

==================================================
七、每条文献必须真实性核验
==================================================

每一条正式进入候选库的论文至少核验：

TITLE

AUTHORS

YEAR

VENUE

DOI

PUBLISHER_URL

ARXIV_URL（如果有）

OPEN_SOURCE_URL（如果有）

SOURCE_TYPE

SEARCH_DATE。

若 DOI 不存在：

写：

DOI_NOT_FOUND

不能自己生成。

若无法找到正式发表页面：

写：

PUBLICATION_NOT_VERIFIED

不得标为正式期刊论文。

==================================================
八、每篇核心论文做研究卡片
==================================================

不是40—60篇全部长篇精读。

从中筛选：

约15—25篇 CORE PAPER。

每篇生成研究卡片：

PAPER_ID:

TITLE:

AUTHORS:

YEAR:

VENUE:

DOI:

URL:

RESEARCH_PROBLEM:

SENSORS:

METHOD:

DATASET / HARDWARE:

SCENARIO:

FAILURE_MODE:

QUALITY / CONFIDENCE MODEL:

FUSION METHOD:

EXPERIMENT DESIGN:

KEY METRICS:

KEY RESULTS:

LIMITATIONS:

WHAT_IT_SUPPORTS_IN_DG202611:

CHAPTER_SUPPORT:

CAN_SUPPORT_NUMERIC_CLAIM:
YES/NO

CAN_SUPPORT_METHOD_DESIGN:
YES/NO

CAN_SUPPORT_RESEARCH_STATUS:
YES/NO

DIRECTLY_RELEVANT_TO_CURRENT_IMPLEMENTATION:
HIGH/MEDIUM/LOW

NOTES:

不得大段复制论文原文。

以自己的话准确概括。

==================================================
九、特别研究“实验条件”
==================================================

对于任何带性能数字的论文：

不能只抄：

“精度提高30%”

“成功率99%”

“误差5cm”。

必须同时记录：

数据集

机器人平台

场景

传感器

真值来源

距离

速度

样本数

是否仿真

是否实车

是否公开数据集

指标定义。

如果测试条件无法确认：

数字不得进入方案书正文。

==================================================
十、建立引用矩阵
==================================================

生成：

STAGE_C_CITATION_MATRIX_v1.0.md

至少包含：

SOURCE_ID

主题

论文/资料

第1章

第2章

第3章

第4章

第5章

第6章

第7章

第8章

第9章

第10章

第11章

可支撑论点

证据强度

是否可正式引用。

特别重点映射：

第2章 → 国内外发展现状

第6章 → 自适应融合/BDS+LiDAR并发退化

第7章 → 动态环境LiDAR

第8章 → 主动重定位

第10章 → 后续实验设计依据。

==================================================
十一、建立“论点—来源”矩阵
==================================================

不要只做“章节—论文”。

再生成：

STAGE_C_CLAIM_EVIDENCE_MATRIX_v1.0.md

例如：

CLAIM:

“单一GNSS在遮挡、多路径和中断条件下存在定位连续性风险”

SUPPORTING_SOURCES:

...

EVIDENCE_LEVEL:

...

CLAIM:

“LiDAR在长走廊或低几何结构环境中可能出现可观测性退化”

SUPPORTING_SOURCES:

...

CLAIM:

“主动运动能够通过获取新的环境观测改善定位可观测性”

SUPPORTING_SOURCES:

...

这样后面 DSH 写正文时：

每一个重要论断都有真实来源。

==================================================
十二、国内外研究现状不能变成论文堆砌
==================================================

在研究完成后，需要额外形成：

STAGE_C_RESEARCH_SYNTHESIS_v1.0.md

不是正式第2章正文。

而是研究综合。

按主题总结：

国外主要路线是什么

国内主要路线是什么

各路线解决了什么

还有什么没解决

对 DG-202611 有什么启发

DG-202611 与这些路线的关系是什么。

重点分析：

不是“别人都有啥”。

而是：

现有方法在多种退化并发时还剩什么问题。

最终研究缺口应帮助回答：

为什么需要：

质量感知

故障感知融合

动态LiDAR鲁棒特征

主动重定位

异构观测扩展

高程通道。

==================================================
十三、国内研究必须真实
==================================================

第2章不能最后变成：

国外论文20篇

国内论文0篇。

请主动寻找：

中国高校

科研院所

机器人

无人车

组合导航

北斗

LiDAR

多传感器融合

毫米波雷达

主动定位

相关真实论文。

但禁止为了“国内外平衡”降低质量。

如果某个细分方向国内公开研究确实较少：

如实说明。

==================================================
十四、候选技术评价
==================================================

本轮同时给以下候选技术建立研究证据：

ZED 2i VIO

毫米波 Radar

Optical Flow / Ground Texture

3D LiDAR

UWB 3D

Event Camera

ToF

以及其他老师材料中提出的方向。

但是候选评价分为：

STRONG_CANDIDATE

PROMISING_BUT_NEEDS_VALIDATION

RESEARCH_RESERVE

LOW_PRIORITY_FOR_CURRENT_PROJECT

INSUFFICIENT_EVIDENCE

不得在本轮直接决定采用。

重点不是“技术先进不先进”。

而是：

与 DG-202611 当前问题是否匹配。

==================================================
十五、候选评价维度
==================================================

每项至少评价：

PHYSICAL_PRINCIPLE

OBSERVATION_TYPE

XYZ/YAW能力

退化互补性

是否帮助高程

动态环境表现

GNSS拒止表现

LiDAR退化表现

算力需求

ROS2生态

Jetson可部署性

硬件是否已存在

标定难度

时间同步难度

成本

实验可实现性

比赛相关性

研究价值

工程风险。

==================================================
十六、特别关注实验设计文献
==================================================

除了算法论文，还要寻找：

怎么测试

怎么统计

怎么定义指标

的资料。

重点帮助后续第10章设计：

定位平面误差

高程误差

ATE

RPE

重定位误差

恢复时间

重定位成功率

特征重复率

退化检测时延

错误接受率

错误拒绝率

定位可用率

导航恢复时间。

不要现在宣称 DG-202611 达到这些指标。

只是寻找：

合理实验方法。

==================================================
十七、官方比赛指标不得倒推论文结果
==================================================

当前官方要求包括：

平面与高程定位精度优于20cm

相邻帧有效特征重复率≥95%

重定位成功率>95%

北斗短报文原型集成/验证。

本轮可以研究：

别人如何定义和测量相关指标。

但不能为了让我们的指标看起来可达到，就筛掉负面论文或者只挑特别漂亮的数据。

==============
~~~~

[本对话目录](README.md) | [总目录](../../README.md) | [Raw](https://raw.githubusercontent.com/weiyang02520-ops/dg202611-conversation-archive/main/conversations/first/part-039.md) | [上一卷](part-038.md) | [下一卷](part-040.md)
