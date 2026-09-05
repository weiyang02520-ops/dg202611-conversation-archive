# 恢复项目上下文 / 第 013 卷

[本对话目录](README.md) | [总目录](../../README.md) | [Raw](https://raw.githubusercontent.com/weiyang02520-ops/dg202611-conversation-archive/main/conversations/second/part-013.md) | [上一卷](part-012.md) | [下一卷](part-014.md)

## 消息 562 / assistant / 分段 1 of 1

~~~~text
你之前对这个“计划书/方案书”的要求，核心不是“把开发过程写进去”，而是把它做成一份**正式、能交给评委看的技术方案书**。我按你之前冻结的规则给你重新归纳一遍：

1. **不能写成开发日志。**  
   正式稿不能出现 `TASKxxx`、PASS/FAIL、TODO、当前完成度、Claude/Codex/DSH、Git、内部 Gate、审计 issue 这些东西。正式稿回答的是“我们最终系统怎么解决比赛问题”，不是“今天代码做到哪一步”。fileciteturn42file0L20-L37

2. **最终已经确定要用的技术，可以直接按正式系统写。**  
   比如 UWB、MODEL-1/2/3、ZED 2i、GNSS/RTK、北斗短报文，只要已经确定属于最终方案，就可以写“本系统采用……”“系统通过……”。不能因为现在还没实机，就写成“未来可能考虑”。fileciteturn42file0L41-L65

3. **但是“方案设计”和“已经实测”必须严格分开。**  
   可以写“设计目标小于20 cm”“目标重复率≥95%”“目标重定位成功率>95%”；没有真实证据前，不能写“本项目实测18 cm”“已经达到97%”。Synthetic、Gazebo、论文成绩都不能冒充自己的实机成绩。fileciteturn42file0L131-L179

4. **不是根据当前仓库有什么来决定方案写什么。**  
   方案书要从官方要求反推最终系统。代码没做完，不代表方案书里不能写；反过来，方案书里写了，也不代表可以冒充已经实现。fileciteturn42file0L131-L143

5. **旧 v0.2、vCurrent 都不再当正式母版。**  
   它们只是历史参考材料。正式稿已经重新按官方评分逻辑建立，旧12章结构不继续沿用。fileciteturn42file0L222-L270

6. **正式结构冻结为9章。**  
   主线是：赛题理解 → 国内外技术边界 → 总体方案 → MODEL-1 → MODEL-2 → MODEL-3 → 样机与北斗短报文 → 工程可行性 → 技术性能与实验验证。fileciteturn42file1L319-L386

7. **全文必须只有一条清晰故事线。**  
   观测退化 → 质量判断 → 自适应融合 → LiDAR稳定特征增强 → 被动融合不够时主动改变运动/视角 → 重定位候选 → 多帧/多源验证 → 安全恢复 → UWB质量感知接入 → 北斗短报文负责受限通信。fileciteturn42file1L305-L315

8. **三个核心模型必须是全书技术核心。**  
   MODEL-1：多源质量感知与自适应融合；MODEL-2：动态/异常环境下 LiDAR 鲁棒稳定特征；MODEL-3：具身主动重定位。三个模型要讲输入、判据、算法、输出、失败边界和验证方法，而不是堆代码。fileciteturn42file2L678-L690

9. **UWB 必须正式进入 MODEL-1。**  
   有效时参与融合，异常时降权或拒绝；UWB 单源故障不能直接触发 MODEL-3。3 个物理基站和 6 个 logical slot 不能写成6个独立实物测量。

10. **高程20 cm必须独立认真写。**  
    不能拿2D LiDAR直接说能测高程，也不能默认UWB解决高程。高程主要依靠 GNSS/RTK、IMU、ZED 2i 三维相对约束、地面运动约束和渐进恢复。fileciteturn42file3L902-L949

11. **三项高风险指标的定义已经提前冻结，不能以后为了过线再改。**  
    特征重复率主指标是 `R_min = M/min(N1,N2)`，还要报告 `R_avg`、特征数量和保留率；不能只剩几个特征刷出100%。重定位也必须按冻结 trial 和成功条件统计。fileciteturn42file3L1004-L1069

12. **重定位>95%不是“进 RECOVERED 就算成功”。**  
    主判据包括位置误差≤0.30 m、航向误差≤10°、30 s内完成、恢复后稳定≥10 s、无人工接管、无假恢复；一次试验里的多个内部 attempt 仍然只能算一个 trial。fileciteturn43file1L420-L502

13. **北斗短报文必须和GNSS/RTK严格分开。**  
    WTRTK980、RTK、NTRIP、4G、LoRa 都不能冒充北斗短报文。短报文是独立的通信链，最终要靠真实终端和真实收发链路验证。fileciteturn42file2L694-L704

14. **没有真实实验结果时，宁可留结果位，也不要写一堆“待验证/暂无数据”。**  
    更不能造表、造曲线、造截图。内部开发状态不要污染评委版。fileciteturn43file0L14-L24

15. **文献用于证明“路线有技术基础”，不是替我们拿成绩。**  
    正确写法是“相关研究表明……因此本方案具备达到目标的技术基础”，而不是把论文里的厘米级、成功率直接写成本项目成绩。fileciteturn42file0L139-L169

16. **正文的重要内容必须有明确来源。**  
    只能来自：官方要求、已核验文献、已确认最终系统设计、已核验真实项目事实、真实正式实验结果。不能因为“旧稿以前这么写”就继续沿用。fileciteturn43file4L1159-L1170

17. **写作风格要像正式科研/工程技术方案，不像宣传册。**  
    要正规、科研、工程、简洁、可信、技术感强；图表是为了帮助评委理解评分点，不是为了好看。fileciteturn43file3L800-L818

18. **评委应该能很快看懂。**  
    重点图就是总系统架构、具身闭环、三个模型、样机硬件、ROS2数据流、北斗短报文、正式实验结果。每张图只回答一个问题。fileciteturn43file3L919-L947

19. **评分导向非常明确。**  
    篇幅和精力优先级：**技术路线 + 三个核心模型 > 国内外调研 > 工程实现 > 其他内容**。因为主观60分里技术路线20分、模型合理性20分，是最大头。fileciteturn42file2L678-L690

20. **现在这份正式稿已经不是“重新写”的阶段了。**  
    `00_FORMAL_SUBMISSION_INTEGRATED_DRAFT_v0.3.1.md` 的文本和引用体系已经 Gate PASS。现在原则是**不无理由重写正文**，接下来主要是核心图、视觉总装、Word版式，以及最后真实实验结果回填。fileciteturn45file0L114-L179

一句话概括你之前定的要求就是：

> **方案书要像一份已经把最终技术路线想清楚的正式竞赛答卷，而不是一份“我们代码现在做到哪里”的工作汇报；可以把最终方案写完整、写强，但任何“已经实测达到”的东西必须有真证据。**
~~~~

## 消息 563 / user / 分段 1 of 1

~~~~text
我想整合一下目前的方案书，先看看效果
~~~~

## 消息 570 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "thoughts",
  "thoughts": [
    {
      "summary": "读取文档技能规范",
      "content": "",
      "chunks": [],
      "finished": true
    }
  ],
  "source_analysis_msg_id": "d48ded2f-9403-4055-82db-e273f250fbe3"
}
~~~~

## 消息 577 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "thoughts",
  "thoughts": [
    {
      "summary": "汇总编排并物化v0.3.1文档及参考文献",
      "content": "",
      "chunks": [],
      "finished": true
    }
  ],
  "source_analysis_msg_id": "1bed2e08-6c89-4679-adee-8e18f46817ad"
}
~~~~

## 消息 582 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "thoughts",
  "thoughts": [
    {
      "summary": "检查文档内容并核对最新参考文献列表",
      "content": "",
      "chunks": [],
      "finished": true
    }
  ],
  "source_analysis_msg_id": "b59fd1c5-90ad-4909-bb0f-90c8749a2142"
}
~~~~

## 消息 598 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "thoughts",
  "thoughts": [
    {
      "summary": "设计封面样式",
      "content": "我正在把现有文档整理成正式方案格式：重排封面信息、补充指导教师与日期字段、统一标题层级，并在封面后加入目录区域，同时尽量保留原有内容。",
      "chunks": [],
      "finished": true
    },
    {
      "summary": "设计了封面样式",
      "content": "",
      "chunks": [],
      "finished": true
    }
  ],
  "source_analysis_msg_id": "affa457a-ef1a-4e2d-8d69-7a925c23eddf"
}
~~~~

## 消息 609 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "thoughts",
  "thoughts": [
    {
      "summary": "完善文档排版",
      "content": "我正在编写样式脚本：设置封面、页眉页脚、标题层级、目录占位、表格边框、正文缩进与分页规则，确保文档结构清晰，同时不擅自补写未提供的实质内容。",
      "chunks": [],
      "finished": true
    },
    {
      "summary": "完善了文档排版",
      "content": "",
      "chunks": [],
      "finished": true
    }
  ],
  "source_analysis_msg_id": "fc25b270-ecf3-4cd3-9efa-057de6516a58"
}
~~~~

## 消息 618 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "thoughts",
  "thoughts": [
    {
      "summary": "检查并完善正式方案书Word文档及其样式导航",
      "content": "",
      "chunks": [],
      "finished": true
    }
  ],
  "source_analysis_msg_id": "1e523bda-0387-4aa7-960e-4d00bc98ccbb"
}
~~~~

## 消息 627 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "thoughts",
  "thoughts": [
    {
      "summary": "已渲染DOCX并检查生成的PDF页面",
      "content": "",
      "chunks": [],
      "finished": true
    }
  ],
  "source_analysis_msg_id": "123c4c3d-910c-41a6-b4ac-f077e02902bc"
}
~~~~

## 消息 657 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "thoughts",
  "thoughts": [
    {
      "summary": "排查环境并更新目录后生成预览统计页面",
      "content": "",
      "chunks": [],
      "finished": true
    }
  ],
  "source_analysis_msg_id": "90a1c431-aa53-4c0b-b17a-80360a91fa69"
}
~~~~

## 消息 673 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "thoughts",
  "thoughts": [
    {
      "summary": "检查并修复文档目录页码及视觉内容",
      "content": "",
      "chunks": [],
      "finished": true
    }
  ],
  "source_analysis_msg_id": "6471dd60-5c13-425f-86cb-9da1f03db7fb"
}
~~~~

## 消息 725 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "thoughts",
  "thoughts": [
    {
      "summary": "修复文档分页更新目录并生成最终预览",
      "content": "",
      "chunks": [],
      "finished": true
    }
  ],
  "source_analysis_msg_id": "87a258b9-f2fd-4d1d-a546-64745a59b742"
}
~~~~

## 消息 726 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "reasoning_recap",
  "content": "思考了 14m 17s"
}
~~~~

## 消息 727 / assistant / 分段 1 of 1

~~~~text
可以，已经给你整合出一版**可直接看的 Word 预览稿**了。

这版严格基于当前冻结的 `00_FORMAL_SUBMISSION_INTEGRATED_DRAFT_v0.3.1.md`，参考文献与 `02_FINAL_REFERENCE_LIST_v0.2.md` 保持一致，没有重新改技术路线。fileciteturn48file1 fileciteturn48file0

我主要做了：封面、3页目录、章节分页、标题层级、正文排版、公式、表格、页眉页脚、页码、参考文献排版，以及现有“待正式绘制”图表的版式占位。当前约 **63页**。

正文里那个 Stage B 内部状态提示也按照之前 Gate 的要求没有放进展示版；真实图、实机照片和实验结果现在没有乱填。

[下载：DG-202611 正式方案书整合预览 v0.3.1](sandbox:/mnt/data/DG-202611_正式方案书整合预览_v0.3.1.docx)

你先打开看看整体感觉。**这一版主要就是让你判断“像不像最终交上去的方案书”**，先别急着继续改技术内容。
~~~~

## 消息 728 / user / 分段 1 of 1

~~~~text
你给提示词，我给codex进行吧，我感觉你这些排版和很多地方都有问题，给个详细的提示词吧
~~~~

## 消息 730 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "thoughts",
  "thoughts": [
    {
      "summary": "编写重建提示词",
      "content": "我会为 Codex 组织一份可直接执行的中文任务说明，明确源文件、冻结规则、页面结构、插图占位、参考文献处理、输出路径，以及禁止事项，避免继续修改错误预览版。",
      "chunks": [],
      "finished": true
    },
    {
      "summary": "编写了重建提示词",
      "content": "",
      "chunks": [],
      "finished": true
    }
  ],
  "source_analysis_msg_id": "5c7e0625-1c38-43d4-bb1c-e28e6932d9ef"
}
~~~~

## 消息 731 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "reasoning_recap",
  "content": "思考了 9s"
}
~~~~

[本对话目录](README.md) | [总目录](../../README.md) | [Raw](https://raw.githubusercontent.com/weiyang02520-ops/dg202611-conversation-archive/main/conversations/second/part-013.md) | [上一卷](part-012.md) | [下一卷](part-014.md)
