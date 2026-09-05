# 机器人定位挑战赛分析 / 第 059 卷

[本对话目录](README.md) | [总目录](../../README.md) | [Raw](https://raw.githubusercontent.com/weiyang02520-ops/dg202611-conversation-archive/main/conversations/first/part-059.md) | [上一卷](part-058.md) | [下一卷](part-060.md)

## 消息 2062 / assistant / 分段 1 of 2

~~~~text
【任务方向】
DOCUMENT_LINE / 文档方向

【执行位置】
当前线程《查看比赛提交要求》，请在本线程直接执行；不得转发。本线程负责 DG-202611 的文档、文献、证据、章节管理，以及生成给 DSH 的写作/修订提示词；不执行开发代码任务。

【任务名称】
DG-202611 DOCUMENT WRITING GATE — 第2章正式写作准备 + DSH写作提示词生成

TASK_DIRECTION: DOCUMENT_LINE

当前已完成并冻结：

STAGE_C = FROZEN
STAGE_D = FROZEN
STAGE_E = FROZEN / PASS_WITH_LIMITATIONS

当前批准状态：

LITERATURE_EVIDENCE_BASE = READY

FORMAL_CHAPTER_2 = AUTHORIZED

CHAPTER_6_METHOD_SUPPORT = READY
CHAPTER_7_METHOD_SUPPORT = READY
CHAPTER_8_METHOD_SUPPORT = READY_WITH_LIMITATIONS

PROJECT_PERFORMANCE_CLAIMS = NOT_AUTHORIZED

本轮只处理：

“第2章正式写作准备，并生成一份可以直接复制给 DSH 的高质量写作提示词。”

不要直接进入第6—8章。
不要直接调用 DSH。
不要联系开发线。

==================================================
一、工作流必须保持
==================================================

当前固定工作流：

Master / 外部审核
→
Codex DOCUMENT_LINE
→
读取本地文档、Stage C/D/E证据
→
确定章节结构、引用与事实边界
→
Codex生成DSH写作提示词
→
用户手动复制给DSH
→
DSH负责长篇正式写作
→
DSH产物返回Codex DOCUMENT_LINE
→
Codex审核
→
必要时再次生成DSH修订提示词

因此本轮：

DSH_USED = NO

DSH_PROMPT_GENERATION = YES

FORMAL_CHAPTER_SOURCE_MODIFICATION = NO

本轮不要替 DSH 直接把整章写完。

==================================================
二、必须优先读取的本地材料
==================================================

请实际读取，不得仅依赖旧聊天记忆。

项目文档根目录：

C:\Users\peng\Desktop\材料\DG-202611比赛材料文档\

优先读取现有正式/母稿相关文件，以及以下研究资产。

Stage C：

C:\Users\peng\Desktop\材料\DG-202611比赛材料文档\STAGE_C_真实文献研究_v1.0\

重点：

STAGE_C_MASTER_SOURCE_TABLE_v1.0.md

STAGE_C_CORE_PAPER_CARDS_v1.0.md

STAGE_C_RESEARCH_SYNTHESIS_v1.0.md

STAGE_C_TECHNOLOGY_CANDIDATE_REVIEW_v1.0.md

STAGE_C_UNSUPPORTED_CLAIMS_v1.0.md

Stage D：

C:\Users\peng\Desktop\材料\DG-202611比赛材料文档\STAGE_D_核心论文全文与性能证据_v1.0\

重点：

01_STAGE_D_FULLTEXT_CORE_PAPER_CARDS_v1.0.md

02_STAGE_D_PERFORMANCE_BENCHMARK_MATRIX_v1.0.csv

03_STAGE_D_COMPETITION_METRIC_SAFE_BOUNDARIES_v1.0.md

09_STAGE_D_SAFE_CLAIM_MATRIX_v1.0.md

10_STAGE_D_PROJECT_TECHNOLOGY_STATUS_MATRIX_v1.0.md

Stage E：

C:\Users\peng\Desktop\材料\DG-202611比赛材料文档\STAGE_E_高优先级证据缺口闭合_v1.0\

重点：

README_STAGE_E_v1.0.md

11_STAGE_E_CHAPTER_READINESS_GATE_v1.0.md

以及：

高程20cm审计

特征重复率95%审计

重定位95%审计

UWB深度证据

毫米波深度证据

Event深度证据

自适应融合深度证据

比赛指标可比性矩阵

性能值可比性审计

Stage E unresolved gaps

references.bib

同时读取当前：

第1—2章已有框架/旧稿

整稿当前预览

已有章节结构

已有参考文献格式

不得自己另起一套完全不同的章节体系。

==================================================
三、最新事实口径必须纳入
==================================================

当前项目技术状态：

1. 北斗 / GNSS / RTK
已有明确项目基础。

2. IMU
已有明确项目基础。

3. LiDAR
已有明确项目基础。

4. ZED 2i
已有明确项目基础。

5. UWB
老师确认已有硬件。

但：

INTERFACE_VERIFIED = NO

ROS2_DATA_VERIFIED = NO

FUSION_VERIFIED = NO

REAL_ROBOT_PERFORMANCE_VERIFIED = NO

因此第2章可以讨论：

UWB研究现状
LOS/NLOS
质量评估
动态降权
异常拒绝
多源融合

但不能写：

“本项目已经完成UWB融合验证”。

6. 毫米波雷达

PLANNED_PROCUREMENT = YES

当前尚未完成采购/集成验证。

可以作为当前确定引入的研究方向和候选异构冗余感知源讨论。

不能写：

“项目已完成毫米波雷达集成”。

7. 事件相机

PLANNED_PROCUREMENT = YES

当前尚未完成采购/集成验证。

可以讨论：

HDR
高速运动
异步视觉
Event-VIO
Event + frame camera
时间同步
时间偏移
延迟补偿

不能写：

“事件相机已安装/已验证”。

8. Event + ZED 2i

当前文献支持的是：

共同时间轴
时间戳对齐
时间偏移估计
时延补偿
必要时硬件同步

不得写成：

“事件相机必须和ZED 2i完全相同FPS”。

9. ToF

NOT_IN_CURRENT_SCOPE

第2章不得再把它写成当前核心路线。

10. 光流 / 地面纹理

NOT_IN_CURRENT_SCOPE

不得继续作为当前主线扩写。

11. 偏振光 + 天基光学导航

FUTURE_OUTLOOK

正文当前研究现状可以极简提及或不展开。

未来展望再处理。

==================================================
四、Stage E重要结论必须继承
==================================================

当前累计：

FULL_TEXT_EXPERIMENT_VERIFIED = 15

与比赛指标 DIRECTLY_COMPARABLE 的文献：

0

这不是研究失败。

第2章可以正常写。

但必须明确：

文献研究现状
≠
比赛指标直接证明
≠
项目已经达到指标

高程 <20 cm：

INDIRECT_EVIDENCE_ONLY

特征重复率 ≥95%：

NO_DIRECT_STANDARD_EQUIVALENT_FOUND

重定位成功率 >95%：

NO_DIRECT_SUPPORT

北斗短报文：

只有官方服务/接口/能力依据。

PROJECT_SHORT_MESSAGE_INTEGRATION = NOT_VERIFIED

==================================================
五、第2章的真正目标
==================================================

第2章不是论文堆砌。

必须回答：

1. 国内外多源导航定位领域发展到哪里了？
2. GNSS/北斗在遮挡、拒止、NLOS环境中的主要问题是什么？
3. LiDAR/视觉/VIO在复杂环境中的优势和退化问题是什么？
4. UWB解决什么问题，又有什么LOS/NLOS和覆盖限制？
5. 毫米波雷达为什么值得作为退化环境中的异构冗余感知源？
6. 事件相机为什么在HDR、高速、复杂光照下有价值？
7. 多传感器融合为什么不能只是固定权重？
8. 为什么需要质量评估、动态降权、异常拒绝、故障感知？
9. 为什么需要主动重定位，而不只是被动等待定位恢复？
10. 当前研究还存在哪些不足？
11. DG-202611项目的研究切入点因此是什么？

最终应该自然引向：

多源异构感知
+
质量感知
+
自适应融合
+
退化检测
+
主动恢复

而不是机械列举传感器。

==================================================
六、先进行第2章结构审计
==================================================

读取现有第2章框架。

优先保留现有母稿章节体系。

不要未经必要性证明大改全书结构。

输出：

CURRENT_CH2_STRUCTURE

PROPOSED_CH2_STRUCTURE

CHANGES_NEEDED

CHANGES_NOT_NEEDED

推荐逻辑可参考，但必须结合现有章节：

2.1 国内外多源导航定位研究总体进展

2.2 北斗/GNSS与惯性组合导航研究现状

2.3 LiDAR / SLAM / 几何退化研究现状

2.4 视觉/VIO及事件视觉研究现状

2.5 UWB及局部无线定位研究现状

2.6 毫米波雷达辅助定位研究现状

2.7 鲁棒、自适应和故障感知多源融合研究现状

2.8 定位失效恢复与主动重定位研究现状

2.9 现有研究不足与本项目切入点

但如果当前正式框架已有合理划分：

优先在现有结构下完成内容组织。

不要为了匹配以上示例强制重构。

==================================================
七、建立 Chapter 2 Evidence Map
==================================================

在让 DSH 写之前，Codex必须先建立一份：

CHAPTER_2_EVIDENCE_MAP

至少包含：

SECTION

CLAIM_ID

CLAIM

SOURCE_ID

PAPER_TITLE

YEAR

DOMESTIC/INTERNATIONAL

FULLTEXT_VERIFIED

EXPERIMENT_VERIFIED

EVIDENCE_STRENGTH

SAFE_WORDING

UNSAFE_WORDING

NEEDS_CITATION

NOTES

必须做到：

每个重要技术判断都有来源。

尤其：

性能数字
优劣比较
退化结论
方法局限

不得无来源写成事实。

==================================================
八、国内外研究现状必须真正有“国内外”
==================================================

Stage C已有：

国内来源 19
国际来源 41

写作时不得变成：

国外论文写很多
国内只一句“国内也开展了研究”。

需要真实体现：

国内团队/论文/技术方向
国际团队/论文/技术方向

但也不要强制50:50。

按实际文献质量组织。

==================================================
九、正文引用优先级
==================================================

引用优先级：

1. FULL_TEXT_EXPERIMENT_VERIFIED

2. 高质量官方/出版社核验来源

3. Stage C verified background sources

4. 官方技术资料

摘要级论文：

可用于非常有限的背景事实。

不要用来支撑复杂性能比较。

老师提供的AI生成材料：

只可作为关键词和方向来源。

禁止直接作为正式参考文献。

==================================================
十、性能数字写作规则
==================================================

任何具体数字必须尽量同时说明：

技术
场景
平台
Ground Truth
指标定义
结果
限制

不要写：

“某方法定位精度达到6cm。”

应写成类似：

“在某公开数据集/某真实实验条件下，某研究以XX作为真值，报告XX指标为XX；但其环境、传感器配置和评价标准与本项目比赛场景并不完全一致。”

具体句式由 DSH 组织。

不要把全部论文都写成实验报告。

只在关键数字值得说明时加入测试条件。

==================================================
十一、禁止从文献数字推导项目成果
==================================================

必须在 DSH 提示词中强制：

LITERATURE_REPORTED

COMPETITION_REQUIREMENT

PROJECT_DESIGN_TARGET

SIMULATION_MEASURED

REAL_ROBOT_MEASURED

严格分离。

第2章以：

LITERATURE_REPORTED

为主。

可以提：

COMPETITION_REQUIREMENT

但不得写：

PROJECT HAS ACHIEVED

除非已有真实项目证据。

==================================================
十二、比赛指标在第2章如何处理
==================================================

对于：

平面 <20 cm

高程 <20 cm

特征重复率 ≥95%

重定位成功率 >95%

不得为了“对上比赛指标”硬找论文。

可以写：

已有研究展示了相关技术达到不同水平，但测试条件和评价指标差异明显。

尤其：

高程20 cm = 只有间接证据

特征重复率95% = 无直接等价标准指标

重定位95% = 无直接可比闭环文献

这些缺口反而可以成为：

后续项目实验设计和评价体系的重要依据。

==================================================
十三、第2章必须避免的写法
==================================================

禁止：

“目前国内外研究已经非常成熟”

这种空泛判断。

禁止：

“一篇论文一句话”连续几十篇。

禁止：

纯按年份罗列。

禁止：

纯按传感器百科介绍。

禁止：

没有比较和总结。

禁止：

把所有最新技术都强行写成项目路线。

禁止：

把毫米波、Event写成已经装车。

禁止：

把UWB写成已经完成融合闭环。

禁止：

把ZED写成已经解决高程20cm。

禁止：

把文献结果写成项目结果。

禁止：

为了显得先进堆砌：

PINN
磁场
超声
ToF
光流
地纹

这些非当前主线技术。

==================================================
十四、第2章希望呈现的研究逻辑
==================================================

建议形成：

传统单一定位源存在场景局限
↓
GNSS/北斗在遮挡/拒止环境退化
↓
LiDAR和视觉可以提供环境约束
↓
LiDAR/视觉自身也存在几何、动态、光照退化
↓
UWB提供局部绝对定位补充，但存在覆盖和NLOS问题
↓
毫米波/Event提供异构退化冗余
↓
仅仅“把传感器都融合”仍不够
↓
需要质量感知、故障检测、自适应权重和异常拒绝
↓
定位严重失效后还需要主动重定位
↓
形成DG项目的技术切入点

这一逻辑是本章主线。

==================================================
十五、与第6—8章避免重复
==================================================

第2章只负责：

研究现状
已有方法
代表性工作
不足
项目切入点

不要提前详细展开：

我们自己的具体公式
状态机
阈值
算法细节
软件实现
场景实验

这些留给第6—8章。

第2章最后只需要把问题自然引到后续章节。

==================================================
十六、篇幅和写作风格
==================================================

首先读取当前整稿和比赛材料整体篇幅。

不要凭空规定固定页数。

第2章应：

足够扎实
但不要变成硕士论文式超长综述。

写作要求：

正式
自然
工程技术型
比赛方案书风格
避免AI腔
避免大量“首先、其次、再次、最后”
避免套话
避免重复总结
避免每段结尾都“因此具有重要意义”

应有：

比较
判断
递进
问题导向

==================================================
十七、引用格式
==================================================

读取当前项目现有引用格式和参考文献格式。

DSH必须沿用现有格式。

不得自己创建另一套引用体系。

如果当前使用：

[1]
[2]

则保持。

如果存在 BibTeX / 引用映射：

保持可追踪关系。

DSH不得自己发明不存在的参考文献编号。

==================================================
十八、Codex本轮需要生成的工作资产
==================================================

在合适的文档工作目录中创建：

CH2_FORMAL_WRITING_PREP_v1.0/

建议包含：

01_CH2_CURRENT_STRUCTURE_AUDIT.md

02_CH2_PROPOSED_STRUCTURE.md

03_CH2_EVIDENCE_MAP.csv

04_CH2_CLAIM_SOURCE_MAP.md

05_CH2_PERFORMANCE_NUMBER_ALLOWLIST.md

06_CH2_FORBIDDEN_CLAIMS.md

07_CH2_CITATION_PLAN.md

08_CH2_DSH_WRITING_REQUIREMENTS.md

09_CH2_DSH_PROMPT_v1.0.txt

10_CH2_POST_DSH_REVIEW_CHECKLIST.md

不要生成大型渲染文件。

==================================================
十九、Performance Number Allowlist
==================================================

这是本轮重要资产。

建立：

05_CH2_PERFORMANCE_NUMBER_ALLOWLIST.md

只有已经满足：

来源已验证
指标定义基本清楚
条件基本明确
不会被误认为项目结果

的性能数字才能进入。

每条：

NUMBER_ID

SOURCE

VALUE

METRIC

CONDITION

FULLTEXT_VERIFIED

ALLOWED_IN_CH2
YES/NO

REQUIRED_CONTEXT

FORBIDDEN_SIMPLIFICATION

DSH只能使用：

ALLOWED_IN_CH2 = YES

的具体性能数字。

其他数字不要写。

==================================================
二十、Forbidden Claims
==================================================

至少明确禁止：

项目平面定位已优于20cm

项目高程定位已优于20cm

项目特征重复率≥95%

项目重定位成功率>95%

ZED 2i已解决高程20cm

UWB已完成机器人融合闭环

毫米波雷达已完成集成

事件相机已完成集成

北斗短报文已完成项目集成

二维EKF已经成为三维GNSS融合

Synthetic结果属于实车结果

Gazebo结果属于实车结果

文献最优结果就是项目设计指标

==================================================
二十一、DSH提示词必须由Codex生成
==================================================

本轮最重要输出：

09_CH2_DSH_PROMPT_v1.0.txt

该文件必须是一份：

用户可以直接完整复制给 DSH 的纯文本提示词。

提示词需要包含：

1. 任务身份
2. 项目背景
3. 第2章目标
4. 当前章节结构
5. Evidence Map
6. 允许引用的论文
7. 允许使用的性能数字
8. 禁止声明
9. 国内外文献要求
10. 引用格式
11. 写作风格
12. 章节长度要求
13. 与第6—8章的边界
14. 最新项目事实
15. Stage C/D/E的重要结论
16. 输出文件要求
17. 自检要求

提示词必须尽量做到：

DSH不需要自己猜项目事实。

==================================================
二十二、DSH不得自行联网补事实
==================================================

在生成给 DSH 的提示词时明确：

优先只使用 Codex 提供的：

Evidence Map
Source Map
Allowlist
Stage C/D/E成果
现有正文

如果 DSH 
~~~~

## 消息 2062 / assistant / 分段 2 of 2

~~~~text
自己具备联网能力：

不得自行加入未经 Codex审核的新论文、新数字、新项目事实。

若认为证据不足：

标记：

[EVIDENCE_GAP]

而不是自行补一个来源。

==================================================
二十三、DSH输出要求
==================================================

Codex在提示词中要求 DSH 输出：

1. 第2章完整正式正文草稿
2. 使用的引用编号/来源映射
3. 使用过的性能数字列表
4. 仍存在的证据缺口
5. 自检结果

正文不要使用：

“据提供材料显示”

“根据Stage E”

等内部工作流语言。

最终正文必须像正常比赛方案书。

==================================================
二十四、DSH写完之后的审核Checklist
==================================================

Codex同时建立：

10_CH2_POST_DSH_REVIEW_CHECKLIST.md

以后收到DSH正文后至少检查：

SOURCE_INVENTION

FAKE_CITATION

NUMBER_MISQUOTE

PROJECT_LITERATURE_CONFUSION

COMPETITION_PROJECT_CONFUSION

UWB_STATUS_ERROR

MMWAVE_STATUS_ERROR

EVENT_STATUS_ERROR

20CM_OVERCLAIM

95_PERCENT_OVERCLAIM

CH2_CH6_8_SCOPE_OVERLAP

DOMESTIC_INTERNATIONAL_BALANCE

AI_STYLE

REPETITION

REFERENCE_NUMBER_CONSISTENCY

==================================================
二十五、本轮不允许做的事
==================================================

不要：

修改正式第2章源文件

直接调用DSH

自己替DSH写完整正式第2章

修改第6—8章

修改整稿

联系开发线

修改开发仓库

生成最终DOCX

生成PPT

开始性能实验

==================================================
二十六、本轮Gate
==================================================

完成准备资产后判断：

CH2_STRUCTURE_READY:
YES/NO

CH2_EVIDENCE_MAP_READY:
YES/NO

CH2_CITATION_PLAN_READY:
YES/NO

CH2_NUMBER_ALLOWLIST_READY:
YES/NO

CH2_FORBIDDEN_CLAIMS_READY:
YES/NO

CH2_DSH_PROMPT_READY:
YES/NO

POST_DSH_REVIEW_CHECKLIST_READY:
YES/NO

只有全部 YES：

DSH_WRITING_AUTHORIZED = YES

但仍然：

DSH_USED = NO

因为实际复制给DSH由用户执行。

==================================================
二十七、最终回复格式
==================================================

最终回复第一行必须：

TASK_DIRECTION: DOCUMENT_LINE

然后：

TASK:
CHAPTER_2_FORMAL_WRITING_PREPARATION

STATUS:
PASS / PASS_WITH_LIMITATIONS / PARTIAL / BLOCKED

STAGE_C_FROZEN:
YES

STAGE_D_FROZEN:
YES

STAGE_E_FROZEN:
YES

FORMAL_BODY_MODIFIED:
NO

DEVELOPMENT_LINE_CONTACTED:
NO

DSH_USED:
NO

CH2_STRUCTURE_READY:

CH2_EVIDENCE_MAP_READY:

CH2_CITATION_PLAN_READY:

CH2_NUMBER_ALLOWLIST_READY:

CH2_FORBIDDEN_CLAIMS_READY:

CH2_DSH_PROMPT_READY:

POST_DSH_REVIEW_CHECKLIST_READY:

DSH_WRITING_AUTHORIZED:

CURRENT_CH2_STRUCTURE:

PROPOSED_CH2_STRUCTURE:

CORE_SOURCES_SELECTED:

FULLTEXT_VERIFIED_SOURCES_SELECTED:

PERFORMANCE_NUMBERS_ALLOWED:

PERFORMANCE_NUMBERS_BLOCKED:

UNRESOLVED_EVIDENCE_GAPS:

FILES_CREATED:

DSH_PROMPT_PATH:

NEXT_ACTION:
USER_COPY_CH2_DSH_PROMPT_TO_DSH / NEED_MORE_EVIDENCE / OTHER

==================================================
二十八、停止条件
==================================================

生成完整：

第2章写作准备资产
+
DSH写作提示词
+
DSH成稿后审核Checklist

之后立即停止。

不要自动调用 DSH。

不要开始第6—8章。

不要修改正式正文。

等待用户把：

09_CH2_DSH_PROMPT_v1.0.txt

复制到 DSH。

现在开始执行。
~~~~

## 消息 2063 / user / 分段 1 of 1

~~~~text
TASK_DIRECTION: DOCUMENT_LINE

TASK: CHAPTER_2_FORMAL_WRITING_PREPARATION
STATUS: PASS_WITH_LIMITATIONS
STAGE_C_FROZEN: YES
STAGE_D_FROZEN: YES
STAGE_E_FROZEN: YES
FORMAL_BODY_MODIFIED: NO
DEVELOPMENT_LINE_CONTACTED: NO
DSH_USED: NO
CH2_STRUCTURE_READY: YES
CH2_EVIDENCE_MAP_READY: YES
CH2_CITATION_PLAN_READY: YES
CH2_NUMBER_ALLOWLIST_READY: YES
CH2_FORBIDDEN_CLAIMS_READY: YES
CH2_DSH_PROMPT_READY: YES
POST_DSH_REVIEW_CHECKLIST_READY: YES
DSH_WRITING_AUTHORIZED: YES_FOR_SEPARATE_DRAFT_ONLY  

CURRENT_CH2_STRUCTURE: 2.1—2.10框架；2.9、2.10属于内部证据管理内容
PROPOSED_CH2_STRUCTURE: 保留2.1—2.8主干，补充完整定位体系、BDS恢复链、异构候选和高程缺口
CORE_SOURCES_SELECTED: Stage C/D/E已核验来源，国内外均有覆盖
FULLTEXT_VERIFIED_SOURCES_SELECTED: S001、S017、S018、S019、S021、S036、S045、N01、N03、N07、N09
PERFORMANCE_NUMBERS_ALLOWED: 仅允许带完整条件的文献报告值
PERFORMANCE_NUMBERS_BLOCKED: 所有DG项目实测结论、摘要级数字、高程达标数字、20cm/95%/95%已达成表述
UNRESOLVED_EVIDENCE_GAPS: 高程、特征重复率、重定位成功率、目标机实测、短报文集成、候选传感器选型
FILES_CREATED: 10个准备文件
DSH_PROMPT_PATH: [09_CH2_DSH_PROMPT_v1.0.txt](C:\\Users\\peng\\Desktop\\材料\\DG-202611比赛材料文档\\CH2_FORMAL_WRITING_PREP_v1.0\\09_CH2_DSH_PROMPT_v1.0.txt)
NEXT_ACTION: USER_COPY_CH2_DSH_PROMPT_TO_DSH  

文件目录：

[CH2_FORMAL_WRITING_PREP_v1.0](C:\\Users\\peng\\Desktop\\材料\\DG-202611比赛材料文档\\CH2_FORMAL_WRITING_PREP_v1.0)

已特别核对：S003、S004当前仍按未全文核验处理，其数字没有放入可用白名单。
~~~~

## 消息 2065 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "model_editable_context",
  "model_set_context": ""
}
~~~~

## 消息 2066 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "reasoning_recap",
  "content": "思考了 6s"
}
~~~~

## 消息 2067 / assistant / 分段 1 of 1

~~~~text
对，这一轮文档线可以正式过 Gate。

现在状态已经很清楚：

**CH2 写作准备 = 完成**  
**DSH 写作 = 已授权，但只能先生成独立草稿**  
**正式第2章源文件 = 仍然不允许直接覆盖**

尤其这几个点做得对：

- 2.1—2.8 作为正式主干，2.9/2.10 内部证据管理内容不混进正式正文；
- S003、S004 没有因为“看起来有用”就强行升级，摘要级数字继续禁止；
- 性能数字只允许使用带完整实验条件的文献报告值；
- `20 cm / 95% / 95%` 仍然没有被偷换成项目成果；
- 高程、短报文、候选传感器选型等缺口继续保留，没有硬填。

### 现在就做这一件事

打开：

`C:\Users\peng\Desktop\材料\DG-202611比赛材料文档\CH2_FORMAL_WRITING_PREP_v1.0\09_CH2_DSH_PROMPT_v1.0.txt`

把**整个提示词原样复制给 DSH**。

不要再自己额外补一句“写得先进一点”“尽量写我们已经实现”之类的话，避免破坏 Codex 刚建立好的证据边界。

### DSH 写完以后

也不要直接把 DSH 成稿当正式第2章。

正确流程继续是：

```text
Codex 文档线
生成 DSH Prompt
        ↓
你复制给 DSH
        ↓
DSH 生成 Chapter 2 独立草稿
        ↓
把 DSH 草稿交回 Codex 文档线
        ↓
Codex 用
10_CH2_POST_DSH_REVIEW_CHECKLIST.md
审核
        ↓
PASS
或
生成 DSH 修订提示词
        ↓
再次修订
```

只有 Codex 文档线审核通过后，我们才决定是否把它合入正式第2章源文件。

所以现在不用继续给 Codex 新任务。

**直接把 `09_CH2_DSH_PROMPT_v1.0.txt` 喂给 DSH。**

等 DSH 写完，再让 Codex 文档线审稿。memcite
~~~~

[本对话目录](README.md) | [总目录](../../README.md) | [Raw](https://raw.githubusercontent.com/weiyang02520-ops/dg202611-conversation-archive/main/conversations/first/part-059.md) | [上一卷](part-058.md) | [下一卷](part-060.md)
