# 恢复项目上下文 / 第 012 卷

[本对话目录](README.md) | [总目录](../../README.md) | [Raw](https://raw.githubusercontent.com/weiyang02520-ops/dg202611-conversation-archive/main/conversations/second/part-012.md) | [上一卷](part-011.md) | [下一卷](part-013.md)

## 消息 462 / assistant / 分段 1 of 1

~~~~text
能，我已经把你之前那套规则重新找回来了。刚才我确实差点走错方向，**我现在先不改任何方案书文件**。

你之前定的核心规则我已经重新对上了：

- **正式方案书 ≠ 开发报告。** 正式稿是给评委看的最终技术方案，不能塞 `TASK159`、`PASS/FAIL`、`133/133 tests`、Claude/Codex、内部 Gate 这些开发管理语言。fileciteturn38file2L216-L238
- **最终系统已经确定要采用的技术，可以直接按“最终方案”来写。** 不需要每句话都写“待实现”“POC”。比如 UWB 已经确定属于 MODEL-1，就可以作为完整系统组成正式描述。只有“已经部署、已经实测、已经达到 XX cm”这种事实，才必须有真实证据。fileciteturn38file1L103-L115
- **没有真实实验结果时，不造数据，也不要在正式稿里塞一大片“暂无数据/待验证”。** 实验结果位置可以先留空，后面 Gazebo / Jetson / 真机数据出来再正式回填。fileciteturn38file0L34-L43
- **20 cm、特征重复率 ≥95%、重定位成功率 >95%、北斗短报文样机验证**是比赛最终指标，当前不能写成已经实测达成。Synthetic PASS 不能变成实机指标。fileciteturn38file2L236-L244
- **UWB 可以正式写入最终方案。** 而且固定规则就是：UWB 作为局部绝对定位源进入 MODEL-1，质量好就参与、异常就降权/拒绝；不能单独触发 MODEL-3。正式方案主线里本来就已经冻结了这一点。fileciteturn38file1L119-L129
- **3 个真实物理 UWB 基站不能包装成 6 个真实独立基站。** 六逻辑通道只能作为系统接口/架构能力，不能虚增几何信息或置信度。
- **北斗定位和北斗短报文必须分开。** WTRTK980 / RTK / NTRIP / 4G / LoRa 不能拿来证明短报文已经完成。
- **正式稿里不出现内部研发词。** 包括 Claude、Codex、DSH、Package A、任务编号、内部 PASS/FAIL 等。图表规范也明确禁止。fileciteturn38file6L542-L562

还有一个特别重要的点，我刚才也搞错了：

**`DG-202611_方案书当前整稿预览_vCurrent.docx` 不是最高规则。**

真正已经冻结的是：

`07_DG-202611_正式提交方案书总体结构与写作蓝图_v1.0_FROZEN.md`

状态明确是：

`FROZEN_FOR_DSH_WRITING`

而且它的定位就是**正式投稿方案书**。fileciteturn38file1L95-L115

所以正确做法不是：

> 拿 vCurrent → 塞进去 133/133 PASS → 改成最新版

而是：

> **按照冻结的正式方案书蓝图，整理“当前能形成的正式方案书”；开发结果只用来校正技术事实和让方案写得更扎实，不把开发报告直接搬进正文。**

而且 MASTER 总章程甚至明确说过：**不默认沿用旧 12 章结构。**fileciteturn38file0L20-L30

所以你刚才拦我拦得对。

**我现在没有修改你任何原方案书。** 下一步如果继续，我会先重新读取 `MASTER_CHARTER + FROZEN正式蓝图 + 当前正式写作源文件`，严格按你之前定的规则整理，而不是自己重新发明一套。
~~~~

## 消息 463 / user / 分段 1 of 1

~~~~text
你tm不准脑补，必须先看好之前的再继续啊，你重复一下之前方案书规则
~~~~

## 消息 480 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "thoughts",
  "thoughts": [
    {
      "summary": "检索正式方案规则及相关文件",
      "content": "",
      "chunks": [],
      "finished": true
    }
  ],
  "source_analysis_msg_id": "5addab88-29ac-485f-b11e-b1ba13903fa1"
}
~~~~

## 消息 517 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "thoughts",
  "thoughts": [
    {
      "summary": "梳理并核对正式方案规则及冻结版本",
      "content": "",
      "chunks": [],
      "finished": true
    }
  ],
  "source_analysis_msg_id": "b778d781-3544-4010-8eb8-01f2bb8387b9"
}
~~~~

## 消息 518 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "reasoning_recap",
  "content": "思考了 1m 2s"
}
~~~~

## 消息 519 / assistant / 分段 1 of 1

~~~~text
对，我现在按你之前**已经冻结的原文件**复述，不加我自己的东西。

之前方案书规则是：

1. **正式方案书不是开发进度汇报。**
   它是提交给发榜单位的正式答卷，写完整最终技术系统。不能把 `TASKxxx`、PASS/FAIL、TODO、成熟度、Claude/Codex/DSH、Git 状态、内部审计这些东西塞进去。fileciteturn42file0L20-L37 fileciteturn42file0L93-L108

2. **“最终方案设计”和“当前已经实现”必须分开。**
   已经确定进入最终系统的技术，哪怕还没实机完成，也可以直接按：
   “本系统采用……”
   “系统通过……”
   来写。
   
   但“已经部署、已经实测、已经达到 XX cm、成功率 XX%”必须有一手项目证据。fileciteturn42file0L41-L65 fileciteturn42file0L69-L108

3. **不能根据当前代码反推正式方案书只能写什么。**
   正式方案书从官方比赛问题出发设计最终系统，而不是“仓库现在有什么就写什么”。fileciteturn42file0L131-L179

4. **旧 v0.2、vCurrent 都不是正式稿母版。**
   之前已经明确：
   - v0.2 = HISTORICAL
   - vCurrent = HISTORICAL
   - 旧 12 章结构不继续沿用
   
   正式方案重新建立独立路线。fileciteturn42file0L222-L270 fileciteturn43file4L1066-L1077

5. **正式目录已经冻结成 9 章，不是旧 12 章。**
   结构是：
   - 摘要 / 项目概览
   - 第1章 赛题理解与总体研究思路
   - 第2章 国内外发展现状与关键技术边界
   - 第3章 系统总体方案与技术路线
   - 第4章 BDS中断 + LiDAR退化并发多源定位模型
   - 第5章 动态目标/异常反射 LiDAR稳定特征模型
   - 第6章 具身主动重定位模型
   - 第7章 样机与北斗短报文集成
   - 第8章 工程应用可行性
   - 第9章 技术性能与实验验证
   - 结论、参考文献、附录
   
   `FORMAL_BLUEPRINT = FROZEN`，`CHAPTER_COUNT = 9`。fileciteturn42file1L319-L386 fileciteturn42file1L619-L633

6. **全书只有一条主线。**
   
   复杂环境导致 BDS、LiDAR 等观测退化  
   → 先判断观测质量  
   → 多源自适应参与/降权/拒绝  
   → LiDAR 提升稳定特征质量  
   → 被动融合不够时主动改变机器人运动/视角  
   → 生成重定位候选  
   → 多帧、多源、置信度验证  
   → 平滑恢复并安全交权  
   → UWB 按同样质量规则接入  
   → 北斗短报文负责通信受限场景的信息传输。fileciteturn42file1L305-L315

7. **UWB 是最终正式系统组成，不是候选。**
   正式方案可以正常写：
   UWB 提供局部绝对定位观测，根据覆盖、NLOS、质量决定参与、降权或拒绝。
   
   但不能无证据写成“已经真实部署并达到某厘米精度”。fileciteturn42file0L51-L65

8. **UWB 不自动解决高程。**
   没确认真实 3D 锚点几何和高程可观性前，不能把 UWB 写成“高程 20 cm 的主要保证”。2D LiDAR 也绝对不能写成直接提供高程。fileciteturn42file3L902-L949

9. **高程必须单独建模。**
   高程不是平面定位附赠的结果。
   
   正式路线是：
   - GNSS/RTK：绝对高程
   - IMU：高频垂向动态
   - ZED 2i：三维相对约束
   - 地面/结构运动约束
   - BDS恢复后渐进重接入
   
   目标可以写“面向 <20 cm”，但实测前不能写“已经 <20 cm”。fileciteturn42file3L872-L933

10. **三项高风险指标的定义已经冻结，不能事后为了过线改。**
    原则是：
    **官方原文 + 高水平文献 + 预先冻结的操作定义。**
    
    如果官方以后给更明确测试协议，官方协议覆盖内部定义。fileciteturn42file3L824-L836

11. **特征重复率不能拿 temporal_match_ratio 冒充。**
    主指标冻结为：
    
    `R_min = M / min(N1,N2)`
    
    同时报告：
    
    `R_avg = 2M/(N1+N2)`
    
    还必须报告特征数量、保留率、无效帧对，防止“只剩几个点然后刷到100%”。正式总判据采用 `R_pool ≥95%`。fileciteturn42file3L965-L1037 fileciteturn43file1L324-L358

12. **重定位成功率不能只看 RECOVERED。**
    一个正式 trial 可以包含多个内部 attempt，但**整个 trial 只能算一次样本**。
    
    冻结的主判据是：
    - `e_xy ≤ 0.30 m`
    - `e_yaw ≤ 10°`
    - `T_reloc ≤ 30 s`
    - 恢复后稳定 `≥10 s`
    - 无人工接管
    - 无假恢复
    
    另外报告更严格的 20 cm 伴随指标。
    
    这些是 **DG 项目预注册判据，不是论文标准**，不能写成“文献规定”。fileciteturn43file1L420-L502 fileciteturn43file2L714-L736

13. **实验结果没有时，宁可留空，不写内部状态废话。**
    不需要在正式方案里到处写：
    - 暂无数据
    - 待验证
    - 当前阶段
    - Package A
    - 后续开发
   
    更不能编实验表、曲线、截图、指标。fileciteturn43file0L14-L24 fileciteturn43file4L1122-L1142

14. **Synthetic / Gazebo 可以证明方法和工程可行性，但绝对不能冒充最终比赛实机指标。**
    第9章实验没完成时可以留结果位；不能拿 synthetic/Gazebo 代替 20 cm、95%、>95% 的正式实机结果。fileciteturn42file1L510-L532

15. **北斗短报文和 GNSS/RTK 是两套东西。**
    GNSS、RTK、NTRIP、4G、LoRa 都不能当成“北斗短报文已经完成”。
    
    北斗短报文要作为独立通信链，最后必须有真实终端、协议、收发日志、端到端验证。fileciteturn42file2L694-L704

16. **文献只证明技术可行性，不能变成项目成绩。**
    写法是：
    “相关研究表明……”
    “说明该路线具备达到目标的技术基础。”
    
    不能写：
    “本项目已经达到论文中的 XX cm / XX%。”fileciteturn42file0L139-L179

17. **每个重要正文事实来源只能来自这些类型：**
    - 官方要求
    - 已核验文献
    - 已确认最终系统设计
    - 已核验项目真实事实
    - 正式项目实验结果
    
    不能因为“旧稿这么写过”就继续继承。fileciteturn43file4L1159-L1170

18. **视觉风格也冻结了：**
    正规、科研、工程、简洁、可信、技术感强。
    
    不做商业宣传册，不搞花里胡哨渐变；图表是为了回答评分问题，不是装饰。fileciteturn43file3L800-L818
    
    核心图优先：
    - 总系统架构
    - 具身闭环
    - MODEL-1
    - MODEL-2
    - MODEL-3
    - 样机硬件
    - ROS2数据流
    - 北斗短报文
    - 实验结果
    
    每张图只回答一个问题。fileciteturn43file3L919-L947

19. **正式引用体系已经冻结。**
    内部 Source ID、REF-LIT、ZCode ID 等不能出现在评委版。
    
    正式参考文献已经统一到 `[1]–[29]`。fileciteturn43file3L986-L999 fileciteturn45file0L63-L88

20. **最重要的当前状态：方案书正文其实已经不是“还要重写”。**
    
    你后面已经有：
    
    `00_FORMAL_SUBMISSION_INTEGRATED_DRAFT_v0.3.1.md`
    
    而且 `121_DG-202611_MASTER_正式方案书文本与引用冻结Gate_v1.0.md` 已明确：
    
    `STATUS = PASS`
    
    当前已经冻结：
    - 摘要
    - 第1～9章正文
    - 结论
    - 三模型逻辑
    - UWB边界
    - WTRTK980边界
    - 北斗短报文边界
    - EXP-01～05协议框架
    - 官方指标口径
    - `[1]–[29]` 引用体系
    
    **除非官方测试规则变化、真实工程证明方案必须调整、真实实验结果需要回填，或者发现新的事实/引用错误，否则不再重写。** fileciteturn45file0L10-L32 fileciteturn45file0L114-L135

所以现在正确的基准不是 `vCurrent`，也不是 `v0.2`。

**当前正式文本基准就是 `00_FORMAL_SUBMISSION_INTEGRATED_DRAFT_v0.3.1.md`，它已经通过文本+引用冻结 Gate。**

现在应该做的是：**在不破坏被冻结正文的前提下，做图表/Word视觉装配，以及后面真实实验结果的回填。** 这也是 Gate 121 明确授权的下一阶段。fileciteturn45file0L137-L182
~~~~

## 消息 520 / user / 分段 1 of 1

~~~~text
可以，新对话你直接发下面这段“对接词”就行：

DG-202611 方案书续接。请先恢复本项目上下文，不要重新从零分析。优先读取资料库 `/DG-202611_MASTER_CONTINUITY/` 中的 MASTER 总上下文和最新状态，然后定位当前最新正式方案书 `00_FORMAL_SUBMISSION_INTEGRATED_DRAFT_v0.3.1.md`，同时读取 `02_FINAL_REFERENCE_LIST_v0.2.md`。方案书此前已经进入阶段性冻结，禁止无理由大改或重新推翻架构。

当前系统核心为 MODEL-1 多源自适应融合、MODEL-2 LiDAR 鲁棒特征、MODEL-3 主动重定位。最新新增工作是把 HR-RTLS1 / DW1000 UWB 正式融入 MODEL-1；UWB 原始资料仓为 `weiyang02520-ops/dg202611-uwb-materials`，目前已完成协议、ROS1 驱动、4/8 基站数据格式、RSSI 输出等关键核验。实际现有 3 个独立 UWB 基站，系统接口按 6 个 logical anchor 设计，但必须按 physical\_source\_id / correlation\_group 去重，禁止把重复通道当 6 个独立测量。

先告诉我：你恢复到的最新方案书版本、当前 MASTER 状态、UWB 在方案书中的现有表述分别是什么。未完成恢复前不要直接修改方案书。

这个对接词基本就是专门给**最新方案书续接**用的。
~~~~

## 消息 521 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "model_editable_context",
  "model_set_context": ""
}
~~~~

## 消息 522 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "reasoning_recap",
  "content": "思考了 7s"
}
~~~~

## 消息 523 / assistant / 分段 1 of 1

~~~~text
可以，这段对接词可以直接作为**新对话的方案书恢复入口**。

而且我现在已经重新确认了关键点：

- 最新正式方案书基准：`00_FORMAL_SUBMISSION_INTEGRATED_DRAFT_v0.3.1.md`
- 正式参考文献：`02_FINAL_REFERENCE_LIST_v0.2.md`
- `121_DG-202611_MASTER_正式方案书文本与引用冻结Gate_v1.0.md`：**PASS**
- 当前正文、引用体系、三模型逻辑、UWB/WTRTK980/北斗短报文边界均已阶段冻结，**禁止新对话一上来重新设计方案书**
- UWB 属于 MODEL-1，3 物理 / 6 logical 必须去重
- 新对话第一步只能**恢复上下文 + 汇报恢复结果**，不能直接改稿

后面我也会把你这段视为“方案书新对话标准启动词”。最关键的是：**以后不能再拿 vCurrent、v0.2 这种历史稿当最新版。** fileciteturn45file0L18-L32
~~~~

## 消息 524 / user / 分段 1 of 1

~~~~text
我找之前的对话要了一份对接词，查漏补缺吧你可以，已经给你把**整个 DG-202611 的跨对话续接重新对齐了一遍**，不是只存 UWB。

现在资料库 `/DG-202611_MASTER_CONTINUITY/` 里已经新增并验证存在：

- `DG-202611_MASTER_CONTEXT_SNAPSHOT_2026-09-03.md`：最新 MASTER 总上下文，三模型、147/154/159/160、freeze、仓库、方案书边界、Agent 分工、Git 真相规则全部接上了。 
- `DG-202611_UWB_HANDOFF_2026-09-03.md`：专门把刚才趁 UWB 仓库还是公开时抓到的关键东西锁死，包括 **ROS1 驱动、4/8 基站** **`mc`** **协议、115200、trilateration、****`rx_power`** **字段及编码、三实物六逻辑、MODEL-1 接法、质量门、synthetic 场景**。 
- `DG-202611_SOURCE_AND_TASK_INDEX_2026-09-03.md`：所有主要仓库、任务链和恢复顺序的索引。 
- `DG-202611_NEW_CHAT_BOOTSTRAP_2026-09-03.md`：新对话直接拿来恢复，不需要你重新给我讲一遍项目。 

原来的 **159、160、9 月 1 日 MASTER 快照、旧 UWB 快照**也都还在，没有覆盖掉。   

所以现在你可以**把** **`dg202611-uwb-materials`** **调成 Private 了**。对目前的 **UWB 架构设计、ROS2 驱动迁移、MODEL-1 融合、异常切除、3→6 逻辑映射、synthetic 验证**，需要的关键资料已经保留下来了。

真正以后做**实机标定**时，还需要重新从你手里三台真实 UWB 采一次串口数据，因为诸如实际 `rx_power` 数值语义、真实 anchor 坐标、LOS/NLOS 阈值这些本来就只能靠实机确定，不是今天多翻说明书就能最终确定的。

本地也给你留了一份：

svgMASTER 最新总快照

svgUWB 完整交接快照

svg新对话启动文件

svg资料与任务总索引

**新对话以后不靠“模型记得多少”，直接靠这套资料库 + GitHub 最新状态恢复。** 这样稳得多。
~~~~

## 消息 554 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "thoughts",
  "thoughts": [
    {
      "summary": "查漏补缺",
      "content": "我发现几处需要补上：明确“对接词”只是工作记忆，不等于事实；区分已确认与待核验信息；补充资料缺失时的处理顺序；说明禁止擅自扩展世界观；增加每轮输出前的自检清单。",
      "chunks": [],
      "finished": true
    },
    {
      "summary": "完成查漏补缺",
      "content": "",
      "chunks": [],
      "finished": true
    }
  ],
  "source_analysis_msg_id": "db411dc6-ec4d-413c-a1b6-e7a678887ecc"
}
~~~~

## 消息 555 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "thoughts",
  "thoughts": [
    {
      "summary": "核对最新方案并追踪Task159至163进展",
      "content": "",
      "chunks": [],
      "finished": true
    }
  ],
  "source_analysis_msg_id": "6b471141-2b65-4098-a651-6213db97e5e2"
}
~~~~

## 消息 556 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "reasoning_recap",
  "content": "思考了 59s"
}
~~~~

## 消息 557 / assistant / 分段 1 of 1

~~~~text
有，而且我刚对完资料库后，发现这份对接词**方向对，但确实还漏了几块后续状态**。最关键的是：**9 月 3 日 continuity 文件本身已经有部分过时，不能把它当最终真相。**

目前应补这 6 点：

- **方案书基准已经更新。** `vCurrent` 已经是历史材料；现在正式正文基准是 `00_FORMAL_SUBMISSION_INTEGRATED_DRAFT_v0.3.1.md`，参考文献是 `02_FINAL_REFERENCE_LIST_v0.2.md`，而且 Gate 121 已经 `PASS`，摘要、第1～9章、结论、三模型逻辑、UWB/WTRTK980/短报文边界、EXP-01～05、[1]–[29] 引用体系都已冻结。fileciteturn47file7L507-L529 fileciteturn47file7L611-L625
- **正式方案书现在禁止因为“软件又做了一些”就重写。** Gate 121 规定只有官方测试规则变化、真实工程迫使方案调整、真实实验结果需要回填、发现事实/引用错误，才允许动冻结正文。当前授权方向是图表/视觉装配、Word版式准备，真实结果后续回填。fileciteturn45file0L114-L179
- **9 月 3 日任务索引已经落后。** 它还写 Task160 “部分完成”、HEAD `5c354686...`，并且任务链只列到160。fileciteturn46file0L32-L42 但我们这条对话后续已经推进到：Task160D UWB 软件清理/远端分支闭合 → `babf4b5a...`；Task161 真 UWB 因硬件没连接而停止；Task163 软件收口继续推进。因此新对话不能把旧索引里的 `5c354...` 当最新 HEAD。
- **UWB 需要补上“软件实现状态”和“实机状态”的明确分界。** Vendor资料已经确认 HR-RTLS1/DW1000、4/8 anchor 风格、115200、`mc`、receive-power 字段等，但 handoff 自己就明确说了：这不是实机证据，`rx_power` 的真实值、符号、单位、有效性仍需实机确认，CIR/NLOS flag 也没有确认。fileciteturn46file6L236-L266
- **正式方案书里的 UWB 表述其实已经写好了，不需要重新想。** 当前正文明确：UWB 是最终系统确定的局部绝对定位观测，根据覆盖、NLOS风险、测距一致性、锚点几何、时间稳定性决定接入/降权/拒绝；未确认真实三维部署前，不作为高程主要保证。fileciteturn47file6L490-L496
- **恢复时必须加“真相优先级”。** 9月3日 MASTER 快照明确规定：当前 Git/本地 HEAD > 实测数据 > MASTER Gate > 任务报告 > Library 快照 > 聊天记忆；文字 `PASS/COMPLETE` 不能替代证据。fileciteturn46file4L158-L179

所以我建议以后新对话的对接逻辑改成：

> 先读 `/DG-202611_MASTER_CONTINUITY/` 恢复历史上下文，但**不能默认其中 9 月 3 日的 task status / HEAD 仍是最新**；恢复后必须继续读取最新 MASTER Gate，并以当前 Git 和本对话后续任务状态校正。  
>   
> 正式方案书当前最高基准为 `00_FORMAL_SUBMISSION_INTEGRATED_DRAFT_v0.3.1.md` + `02_FINAL_REFERENCE_LIST_v0.2.md` + `121_DG-202611_MASTER_正式方案书文本与引用冻结Gate_v1.0.md`。旧 `vCurrent`、v0.2 仅为历史材料。  
>   
> 正式方案书“文本+引用”已经冻结，禁止因为开发线新增软件测试、commit 或 PASS 数字就重写正文；软件结果属于开发/证据层。只有真实工程迫使设计变化、真实实验结果回填、官方规则变化或发现事实/引用错误时才修改冻结正文。  
>   
> 开发线需继续恢复 Task159→160→160D→161→163 的最新状态。其中 UWB 已归属 MODEL-1，三实物/六逻辑按 physical_source_id / correlation_group 去重；UWB 单源异常不得直接触发 MODEL-3。真实 UWB 串口、真实 `rx_power` 语义、真实 anchor 坐标、LOS/NLOS阈值、真实定位精度仍必须通过实机取得。  
>   
> 恢复完成后先报告：①最新正式方案书版本和冻结 Gate；②当前 Git branch/HEAD；③三模型 freeze 状态；④UWB 软件状态与实机状态；⑤当前真正未完成任务。没有完成这些核验前，不允许重新规划架构、不允许修改方案书。

**这版比你拿来的对接词更完整。**

特别记住两个坑：**`vCurrent` 不是最新版；9月3日 continuity 里的 `5c354... / Task160部分完成` 也已经不是最新开发断点。** fileciteturn46file0L32-L42

后面我自己也会按这个规则执行，不再看到旧稿就直接动方案书。 memcite
~~~~

## 消息 558 / user / 分段 1 of 1

~~~~text
目前了解了没
~~~~

## 消息 559 / assistant / 分段 1 of 1

~~~~text
了解了，而且这次已经对齐到**正确版本和正确规则**了。

我现在认定的当前状态是：

- 正式方案书基准：`00_FORMAL_SUBMISSION_INTEGRATED_DRAFT_v0.3.1.md`
- 正式参考文献：`02_FINAL_REFERENCE_LIST_v0.2.md`
- 文本+引用冻结 Gate：**PASS**
- 旧 `vCurrent`、v0.2：**历史材料，不再当母版**
- 正式方案书正文：**不因为开发线又多了测试/commit 就重写**
- 当前允许继续做的主要是：**核心图、视觉总装、Word版式、后续真实实验结果回填**
- 三模型：MODEL-1 / MODEL-2 / MODEL-3 架构保持冻结
- UWB：属于 MODEL-1，不是 MODEL-4
- UWB：3 个真实物理基站、6 logical slot，必须按 physical/correlation group 去重
- UWB 单源异常：只能降权/拒绝，**不能直接触发 MODEL-3**
- UWB 协议资料：HR-RTLS1/DW1000、4/8 anchor、115200、`mc`、receive-power 等已经核验
- 但真实 `rx_power` 语义、真实 anchor 坐标、LOS/NLOS 阈值、真实精度：**必须等实机**
- synthetic / Gazebo / 软件 PASS：**绝不能冒充实机比赛指标**
- 20 cm / ≥95% / >95% / 北斗短报文：都必须按之前冻结的正式口径来
- 北斗短报文和 GNSS/RTK：**绝对不能混**
- 真相优先级：**当前 Git/真实证据 > Gate > 任务报告 > Library 快照 > 聊天记忆**
- 9 月 3 日 continuity 里的旧 HEAD / Task160“部分完成”状态已经过时，不能照抄

所以后面你叫我继续方案书、开发、审计，我都会先按这套状态走，**不再自己脑补，不再拿旧稿当最新版。**
~~~~

## 消息 560 / user / 分段 1 of 1

~~~~text
说一下我之前说过的计划书的要求吧
~~~~

## 消息 561 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "reasoning_recap",
  "content": "思考了 6s"
}
~~~~

[本对话目录](README.md) | [总目录](../../README.md) | [Raw](https://raw.githubusercontent.com/weiyang02520-ops/dg202611-conversation-archive/main/conversations/second/part-012.md) | [上一卷](part-011.md) | [下一卷](part-013.md)
