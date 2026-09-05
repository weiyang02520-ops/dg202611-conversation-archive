# 机器人定位挑战赛分析 / 第 003 卷

[本对话目录](README.md) | [总目录](../../README.md) | [Raw](https://raw.githubusercontent.com/weiyang02520-ops/dg202611-conversation-archive/main/conversations/first/part-003.md) | [上一卷](part-002.md) | [下一卷](part-004.md)

## 消息 279 / assistant / 分段 1 of 1

~~~~text
有，**而且仓库里明显已经整理过一套“答辩用素材”**。不过目前我在 `main` 分支里**没有确认到现成的 `.ppt/.pptx` 成品 PPT**，更像是“做 PPT 和答辩稿需要的材料已经准备得比较全”。

最明显的是这个目录：

`docs/architecture/`

里面直接有：

- `01-答辩系统总架构图.svg`
- `01-答辩系统总架构图.mmd`
- `02-ROS2节点通信拓扑.svg`
- `02-ROS2节点通信拓扑.mmd`
- `03-AI-Agent任务闭环时序.svg`
- `03-AI-Agent任务闭环时序.mmd`
- `04-地图路线平台闭环.svg`
- `04-地图路线平台闭环.mmd`
- `05-通信接口矩阵.md`
- `README.md`

也就是说，**总架构、ROS2架构、AI闭环、导航闭环、通信接口这几页 PPT 的核心图都已经做好了**。甚至第一张文件名就直接叫“答辩系统总架构图”。fileciteturn34file0L2-L2

另外仓库里还有一大堆能直接拿来做答辩内容的文档，比如：

- `电力巡检机器人使用与调试手册.md` —— 整个项目功能和使用情况
- `AI智能体工程日志.md` —— AI Agent 部分
- `ZED三维建模集成说明.md`
- `三维建图工作流程.md`
- `本体QML操控台.md`
- `移动端桥接调试接口.md`
- `云平台连接运维.md`
- `二值禁行区安全地图操作.md`
- `路线JSON字段参考.md`
- `superpowers/plans/` —— 开发计划
- `superpowers/specs/` —— 各模块设计说明
- `UI_sc/` —— UI 截图
- `记录照片/` —— 实机照片

所以这个仓库实际上可以理解成：

**代码 + 实机照片 + UI截图 + 架构图 + 技术文档 + 开发设计文档都有，只是没有看到最终打包好的答辩 PPT。**

我建议你现在别只让 Codex 下载照片，**顺手把所有“答辩相关资料”一起抓到材料文件夹里**。后面我们甚至可以直接从这些东西里整理出一套新的比赛 PPT。

你要的话，我下一条直接给你一份 **Codex 提示词：自动把“答辩相关文档 + 架构图 + 实机照片 + UI截图”全部筛出来、重命名、分类下载**。这样一次收齐。
~~~~

## 消息 280 / user / 分段 1 of 1

~~~~text
2. 作品提交方式
请各参赛团队于 2026 年 9 月 15 日前将材料文档、源代码
及使用说明一并提交至邮箱 zbbtwt@163.com，邮件主题统一按
“榜单编号+大学名称+团队名称”格式命名，提交材料文件命名
8
为“方案文档/代码_大学名称_团队名称.pdf/zip”；所有参赛材料
须确保原创性与真实性，严禁抄袭、剽窃及数据造假等学术不
端行为，一经查实违规，将一律取消参赛资格，通报所在高校
及主管部门，并依法依规追究相关责任
~~~~

## 消息 282 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "model_editable_context",
  "model_set_context": ""
}
~~~~

## 消息 304 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "thoughts",
  "thoughts": [
    {
      "summary": "判断了材料、源码及提交要求",
      "content": "",
      "chunks": [],
      "finished": true
    }
  ],
  "source_analysis_msg_id": "598d5f4a-e8dd-4ecc-9e63-b9f8aa84d961"
}
~~~~

## 消息 305 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "reasoning_recap",
  "content": "思考了 15s"
}
~~~~

## 消息 306 / assistant / 分段 1 of 1

~~~~text
对，你贴的这一段其实把 **9 月 15 日初审材料怎么交** 说得很清楚了。

截至 **2026 年 9 月 15 日**，明确要求邮件提交的是：

1. **方案文档 PDF**
2. **源代码 ZIP**
3. **使用说明**（可以放方案文档里，也可以随代码附 README/说明文档）

邮件发到 `zbbtwt@163.com`，主题格式是：

`DG-202611 + 大学名称 + 团队名称`

文件名要求类似：

`方案文档_大学名称_团队名称.pdf`

`代码_大学名称_团队名称.zip`

这是比赛方案原文明确写的。fileciteturn41file0L22-L29

而且我专门又查了一遍整份比赛方案：**9 月 15 日这一轮没有写要求提交 PPT，也没有写必须提交答辩视频。**

真正要求的作品组成是“材料文档 + 源代码及使用说明 + 样机”。其中材料文档必须覆盖：

- 国内外发展调研
- 技术路线
- BDS 中断 + 激光雷达退化并发模型
- 动态目标干扰下激光雷达特征提取模型
- 具身智能重定位模型
- 工程应用可行性分析
- 技术性能分析结果报告

fileciteturn41file7L295-L306

所以你前面问的“答辩 PPT 要不要现在找”，答案现在更明确了：

**可以找旧 PPT 当素材，但它不是你们 9 月 15 日必须提交的东西。现在真正最重要的是把“方案文档 PDF”搞出来。**

时间流程实际上是：

**9 月 15 日：交方案文档 + 代码 + 使用说明 → 9 月 30 日前初审 → 入围后 10 月企业指导完善 → 11 月终审擂台赛。** fileciteturn41file1L73-L85

所以 PPT 更像是 **入围终审以后为了 11 月擂台赛准备的东西**。至少你现在给我的这份官方文件里，**没有规定初审阶段必须交 PPT**。

你现在收集旧项目资料，最重要的目标其实应该变成：**拿这些旧照片、架构图、旧文档、代码和实验数据，拼出这份“方案文档 PDF”需要的内容。** 你刚上传的《电力机器人巡检创新项目立项报告》其实很有用，它本身已经有 **23 页**，而且里面项目背景、系统架构、ROS2/Nav2、RTK/GNSS、ZED、技术风险等内容都有，可以大量继承重写。比如它已经明确写了现有系统面向电力巡检，采用 ROS2/Nav2/AMCL/SLAM，并预留了 RTK/GNSS 和 ZED 三维能力。fileciteturn40file0L47-L68

也就是说，**我们现在不是从零写比赛文档。**

下一步应该是：

**旧《立项说明书》 + GitHub 项目 + 比赛 DG-202611 要求 → 改造成新的《揭榜挂帅方案文档》**。

这才是你现在最该干的。
~~~~

## 消息 307 / user / 分段 1 of 1

~~~~text

~~~~

## 消息 311 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "reasoning_recap",
  "content": "思考了 4s"
}
~~~~

## 消息 312 / assistant / 分段 1 of 1

~~~~text
收到，这份 `AI_LONG_TERM_WORKFLOW(3).md` 我已经读到了。

从现在开始，这个项目我会按它定义的长期协作方式执行：**我负责架构、计划、独立审核和连续性管理；Codex / Claude Code 等 Workspace Agent 负责实际修改、测试、提交和推送；GitHub 作为最终可验证事实来源；每个重要阶段到 Gate 后先停下来由我复核，再进入下一阶段。** fileciteturn42file0L78-L133

另外我会遵守它的几个关键原则：仓库事实优先于聊天记忆、重要结论要有证据、不会仅凭 Agent 自报“PASS”就认可、长期上下文要沉淀到项目 Memory / Handoff / Git 历史里，而不是依赖当前对话一直记着。fileciteturn42file0L298-L355

所以接下来这个 **DG-202611 揭榜项目 / 电力巡检机器人改造**，也可以正式按这套流程来管理。你之后哪怕换新对话，只要仓库里的项目状态和 Memory 做好，就不需要重新跟我讲一遍。
~~~~

## 消息 313 / user / 分段 1 of 1

~~~~text
GitHub仓库搞一个公开的，方便ai访问
~~~~

## 消息 314 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "model_editable_context",
  "model_set_context": ""
}
~~~~

## 消息 318 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "thoughts",
  "thoughts": [
    {
      "summary": "准备建立仓库",
      "content": "我正在确认可行方式：当前无法直接替用户创建公开仓库，因此我会改为提供一套可复制的创建方案，包括仓库命名、目录结构、初始化文件，以及让智能编程助手自动生成项目骨架的提示词。",
      "chunks": [],
      "finished": true
    },
    {
      "summary": "准备建立仓库",
      "content": "",
      "chunks": [],
      "finished": true
    }
  ],
  "source_analysis_msg_id": "21ac2211-efcc-40fb-9328-cbd10bddd871"
}
~~~~

## 消息 321 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "thoughts",
  "thoughts": [
    {
      "summary": "准备公开 AI 协作仓库",
      "content": "",
      "chunks": [],
      "finished": true
    }
  ],
  "source_analysis_msg_id": "a661083a-9f59-4156-91c5-6d7108afb731"
}
~~~~

## 消息 322 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "reasoning_recap",
  "content": "思考了 20s"
}
~~~~

## 消息 323 / assistant / 分段 1 of 1

~~~~text
可以。这个比赛我建议单独建一个公开仓库，不要直接拿原来的电力巡检机器人仓库硬改。

仓库名我建议用：

`DG-202611-embodied-navigation`

最终就是：

`weiyang02520-ops/DG-202611-embodied-navigation`

我刚查了，这个名字目前没有占用。

不过我现在连着的 GitHub 接口**能读写已有仓库，但没有“新建仓库”这个动作**。所以你让 Codex 执行一下最快，直接把下面整段给它：

```text
请为 DG-202611 揭榜挂帅比赛创建一个新的 GitHub Public 仓库。

GitHub 用户：
weiyang02520-ops

仓库名：
DG-202611-embodied-navigation

仓库描述：
DG-202611 基于具身智能的无人平台多源导航定位技术｜BDS/GNSS + LiDAR + IMU + ROS2

要求：

1. 创建 Public 仓库，方便 ChatGPT、Codex、Claude Code 等 AI 后续直接读取。

2. 默认分支：
main

3. 初始化 README.md。

4. 本地工作目录建议：
C:\Users\peng\Documents\DG-202611-embodied-navigation

5. 使用 GitHub CLI 优先创建，例如：

gh repo create weiyang02520-ops/DG-202611-embodied-navigation ^
  --public ^
  --description "DG-202611 基于具身智能的无人平台多源导航定位技术｜BDS/GNSS + LiDAR + IMU + ROS2" ^
  --clone

如果 Windows PowerShell 不接受 ^，使用单行命令：

gh repo create weiyang02520-ops/DG-202611-embodied-navigation --public --description "DG-202611 基于具身智能的无人平台多源导航定位技术｜BDS/GNSS + LiDAR + IMU + ROS2" --clone

6. 创建初始目录：

docs/
docs/context/
docs/competition/
docs/design/
docs/experiments/
docs/materials/

.ai-handoff/

src/

scripts/

config/

7. 将 AI_LONG_TERM_WORKFLOW.md 放到仓库根目录。

并初始化：

docs/context/CHATGPT_MEMORY.md
docs/context/AGENT_MEMORY.md

.ai-handoff/PROJECT_STATE.md
.ai-handoff/HANDOFF.md
.ai-handoff/DECISIONS.md
.ai-handoff/REVIEW_REQUEST.md
.ai-handoff/NEXT_TASKS.md

8. README.md 第一版只写项目身份，不要虚构尚未实现的能力。

至少说明：

- Competition ID: DG-202611
- 项目：基于具身智能的无人平台多源导航定位技术
- 当前基础来自现有电力巡检机器人项目
- 当前目标包括：
  BDS/GNSS + LiDAR + IMU 多源融合定位
  BDS 中断与 LiDAR 退化并发处理
  动态目标干扰下 LiDAR 特征处理
  主动重定位
  北斗短报文通信
  实体无人平台验证

明确区分：
CURRENT / TARGET

不能把规划中的功能写成已经完成。

9. 添加 .gitignore。

严禁上传：
- API Key
- Token
- 密码
- .env
- 私钥
- Cookie
- 个人账号信息
- 学号
- 手机号
- 身份证信息
- 私人聊天记录
- 未脱敏报名表

尤其不要直接上传现有《团队创建报告》中包含的成员学号、手机号等个人信息。

10. 完成后：

git status
git diff --check

提交：

git add .
git commit -m "chore: bootstrap DG-202611 project repository"
git push origin main

最后确认：

- 仓库为 Public
- 默认分支为 main
- Remote HEAD 与 Local HEAD 一致
- 输出仓库 URL
- 输出最终 commit SHA
- STOP，不继续开发比赛功能。
```

这个仓库以后就作为**本次比赛的主仓库**；原来的：

`electric-power-inspection-robot`

继续作为**现有机器人基础/参考实现**。新仓库需要哪些东西，再有选择地迁移过去，不要一开始整个复制。

还有一点很重要：你刚上传的《团队创建报告》里有**学号、手机号等个人信息**，公开仓库里千万别原样上传。Public 对 AI 访问确实方便，但也意味着任何人都能看到。fileciteturn40file1L50-L66

仓库建好之后把 Codex 最后的 URL 和 commit SHA 发给我，我就按你刚给我的长期工作流开始接管这个比赛项目。fileciteturn42file0L78-L133
~~~~

## 消息 324 / user / 分段 1 of 1

~~~~text
等等，我的意思是你单独搞一个仓库专门搞这个事，就叫做plan吧，然后有关于个人隐私和可公开的我等会会自己去创建2个文件夹分开放，所以应该还行
~~~~

## 消息 325 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "model_editable_context",
  "model_set_context": ""
}
~~~~

## 消息 326 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "thoughts",
  "thoughts": [
    {
      "summary": "规划仓库结构",
      "content": "我建议把 `plan` 设为公开仓库，仅提交可公开资料；个人隐私不能只靠文件夹隔离，否则仍会公开。隐私内容应放进私有仓库，或在本地用忽略规则确保永不提交。",
      "chunks": [],
      "finished": true
    },
    {
      "summary": "规划了仓库结构",
      "content": "",
      "chunks": [],
      "finished": true
    }
  ],
  "source_analysis_msg_id": "3cdfbea1-6292-4847-b3ff-34bf14c8237b"
}
~~~~

## 消息 337 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "thoughts",
  "thoughts": [
    {
      "summary": "确认 plan 仓库可用性及隐私文件夹限制",
      "content": "",
      "chunks": [],
      "finished": true
    }
  ],
  "source_analysis_msg_id": "a93e2ecd-f702-449c-9b59-a907180748c6"
}
~~~~

## 消息 338 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "reasoning_recap",
  "content": "思考了 21s"
}
~~~~

## 消息 339 / assistant / 分段 1 of 1

~~~~text
对，我明白了。不是建“比赛代码仓库”，而是**单独建一个专门管理这次比赛计划、材料、文档、进度的仓库**，就叫：

`weiyang02520-ops/plan`

这个名字目前看没有被你占用。

不过有一点必须注意：**如果 `plan` 是 Public 仓库，那么仓库里的任何文件夹都是公开的。** 你不能靠建一个“隐私文件夹”来保密；只要提交到 GitHub，别人和 AI 都能看到。

所以最适合你现在这个想法的是：

```text
plan/
├─ public/          # 可以提交 GitHub，给 AI 读取
│  ├─ competition/
│  ├─ references/
│  ├─ plans/
│  ├─ reports/
│  └─ materials/
│
├─ private/         # 只留本地，不上传 GitHub
│  ├─ team-info/
│  ├─ registration/
│  └─ personal/
│
├─ AI_LONG_TERM_WORKFLOW.md
├─ README.md
└─ .gitignore
```

然后 `.gitignore` 里直接写：

```gitignore
private/
```

这样就非常合适：**一个 Public `plan` 仓库负责长期 AI 协作，敏感材料仍然可以和它放在同一个本地项目目录里，但 Git 永远不追踪 `private/`。**

而且这个仓库的定位我建议固定成：

> **DG-202611 比赛的项目管理与知识仓库，不是机器人源代码仓库。**

里面主要放比赛原文、我们整理后的要求、方案计划、调研、实验记录、任务拆分、素材索引、方案文档草稿、AI Memory/Handoff 等。真正机器人代码仍然在原来的 `electric-power-inspection-robot`，以后如果确实需要比赛专用代码，再决定怎么处理。

你自己把 `plan` 创建成 Public 后，把链接丢给我就行。**仓库里面第一版结构和长期 AI 管理文件，我再给 Codex 一次性初始化。** memcite
~~~~

## 消息 340 / user / 分段 1 of 1

~~~~text
可以的
~~~~

## 消息 341 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "model_editable_context",
  "model_set_context": ""
}
~~~~

## 消息 342 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "reasoning_recap",
  "content": "思考了 5s"
}
~~~~

## 消息 343 / assistant / 分段 1 of 1

~~~~text
好，就按这个方案来。

你创建一个 **Public 仓库 `plan`**，然后把仓库链接发给我。之后我会按这个顺序帮你整理：

- 建好 `public/` 和本地 `private/` 的边界
- 配置 `.gitignore`，确保隐私目录不会上传
- 放入 `AI_LONG_TERM_WORKFLOW.md`
- 初始化 `CHATGPT_MEMORY.md`、`AGENT_MEMORY.md`
- 建比赛要求、计划、材料索引、实验记录、Handoff 等目录
- 把 **DG-202611** 官方要求整理进去
- 把原来的电力巡检机器人仓库作为“现有基础/参考项目”关联起来
- 最后交给 Codex 持续维护

仓库创建好后，直接把 `https://github.com/.../plan` 发我就行。
~~~~

## 消息 344 / user / 分段 1 of 1

~~~~text
ai创建的吧
~~~~

## 消息 348 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "thoughts",
  "thoughts": [
    {
      "summary": "确认创建方式",
      "content": "我理解是在问“让 AI 来创建吧”。可以：我会按方案生成仓库结构、README 和代码；如果当前连接无法直接新建仓库，我会改为提供可交给 Codex 或 Claude 执行的完整指令。",
      "chunks": [],
      "finished": true
    },
    {
      "summary": "确认了创建方式",
      "content": "",
      "chunks": [],
      "finished": true
    }
  ],
  "source_analysis_msg_id": "bcfd682a-3089-47c4-ae65-f73ef849cad6"
}
~~~~

## 消息 349 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "reasoning_recap",
  "content": "思考了 10s"
}
~~~~

## 消息 350 / assistant / 分段 1 of 1

~~~~text
对，仓库也直接让 Codex 创建，不用你手动点 GitHub。

把这段直接给 Codex：

```text
创建一个新的 GitHub 仓库，作为 DG-202611 揭榜项目的长期计划、材料、文档和 AI 协作仓库。

GitHub 账号：
weiyang02520-ops

仓库名：
plan

仓库可见性：
Public

默认分支：
main

仓库定位：
这个仓库不是机器人源代码主仓库。
它专门用于：
- DG-202611 比赛计划
- 比赛官方资料
- 技术调研
- 项目方案
- 工作量规划
- 实验记录
- 比赛材料
- AI 长期 Memory / Handoff
- 与现有 electric-power-inspection-robot 项目的关联和改造计划

执行要求：

1. 优先使用 GitHub CLI：
   gh repo create weiyang02520-ops/plan --public --clone

2. 如果本地已有同名目录，不覆盖任何已有文件，选择安全位置或停止说明。

3. 初始化 README.md，简单写明：

# plan

DG-202611 揭榜挂帅项目长期计划、材料与 AI 协作仓库。

Competition:
DG-202611 — 基于具身智能的无人平台多源导航定位技术

Reference implementation:
https://github.com/weiyang02520-ops/electric-power-inspection-robot

注意：
本仓库主要管理方案、计划、研究资料和项目状态。
机器人实际源代码目前仍以 electric-power-inspection-robot 为基础。

4. 创建 .gitignore，至少加入：

private/
.env
.env.*
*.key
*.pem
credentials*
secrets*

其中 private/ 永远不得提交到 Public GitHub。

5. 暂时不要替我建立 public/private 材料目录。
我之后会自己整理这两个目录。

6. 不要开始编写比赛方案，不要复制原机器人仓库代码，不要进行其他开发。

7. 完成后：

git status
git log -1 --oneline
git remote -v
git rev-parse HEAD
git ls-remote origin refs/heads/main

确认：
- Repository = weiyang02520-ops/plan
- Visibility = Public
- Branch = main
- Local HEAD = Remote main HEAD

最后输出：
Repository URL
Commit SHA
Remote Verified: YES/NO

然后 STOP。
```

这样最合适。**Codex 只负责把 `plan` 仓库建出来并初始化，材料怎么分 `公开 / 隐私` 你后面自己放。**

等 Codex 建完，把它最后输出的 **仓库链接 + Commit SHA** 给我，我再开始给这个 `plan` 仓库正式建立 DG-202611 的长期项目结构。memcite
~~~~

## 消息 351 / user / 分段 1 of 1

~~~~text
GitHub上有什么比较好些的计划书书写的skill吗，我看到这个比赛需要计划书
~~~~

## 消息 352 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "model_editable_context",
  "model_set_context": ""
}
~~~~

## 消息 360 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "thoughts",
  "thoughts": [
    {
      "summary": "判断最佳技能",
      "content": "我正在把比赛要求拆成研究背景、技术路线、三类模型、实验报告与可行性论证。相比商业计划书，研究型提案写作更匹配；我会优先推荐能生成完整科研方案的技能，同时说明各自适用场景。",
      "chunks": [],
      "finished": true
    },
    {
      "summary": "判断最佳技能",
      "content": "",
      "chunks": [],
      "finished": true
    }
  ],
  "source_analysis_msg_id": "2f746128-dc82-4c4c-8773-b74efc1387aa"
}
~~~~

## 消息 361 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "thoughts",
  "thoughts": [
    {
      "summary": "已搜索 1 个网站",
      "content": "",
      "chunks": [],
      "finished": true
    }
  ],
  "source_analysis_msg_id": "b42eb66b-202d-4787-adc8-eedb77fe52fe"
}
~~~~

## 消息 362 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "reasoning_recap",
  "content": "思考了 29s"
}
~~~~

## 消息 363 / assistant / 分段 1 of 1

~~~~text
有，而且我刚专门按你这个比赛的类型搜了一圈。你这个 **DG-202611 不适合用普通“商业计划书/business plan”Skill**，更接近：

**科研项目方案 + 技术路线 + 研究计划 + 工程验证报告。**

官方要求的“材料文档”本身就包含：国内外调研、技术路线、三个关键模型设计、工程可行性、技术性能分析；评分又重点看研究思路、技术路线和模型合理性。fileciteturn41file7L295-L318

所以我给你按适配度排：

| 推荐 | Skill | 适合我们什么 |
|---|---|---|
| ⭐⭐⭐⭐⭐ | **research-proposal-builder-skill** | 最适合，直接做研究方案、创新点、技术路线、实验方案、可行性、风险、时间表 |
| ⭐⭐⭐⭐⭐ | **technical-writing** | 把方案写得专业、具体、不像 AI 废话 |
| ⭐⭐⭐⭐½ | **research-proposal-writer** | 强化研究问题、里程碑、风险和备用路线 |
| ⭐⭐⭐⭐ | **research-skills / research-proposal** | 中文科研计划、文献调研、研究计划整体成稿 |
| ⭐⭐⭐⭐ | **Academic Research Agent Skill** | 前期查文献、创新性检查、实验设计特别好 |

### ① 我最推荐：research-proposal-builder-skill

urlwupa12/research-proposal-builder-skillhttps://github.com/wupa12/research-proposal-builder-skill

这个跟你这次比赛简直对口。

它本身就是面向 **计算机科学、AI、软件工程** 的科研方案 Skill，覆盖：

> 研究缺口 → 研究问题 → 创新点审计 → 技术路线 → 实验方案 → 可行性 → Pilot → 风险 → 时间规划 → 开题报告 → 答辩准备

citeturn895107search12

拿到 DG-202611 这里就可以直接映射成：

```text
比赛要求
        ↓
研究问题拆解
        ↓
RQ1：BDS短时中断 + LiDAR退化
RQ2：动态物体/异常反射
RQ3：具身主动重定位
RQ4：北斗短报文 + 样机
        ↓
创新点
        ↓
技术路线
        ↓
实验设计
        ↓
指标验证
        ↓
风险与Fallback
        ↓
方案文档
```

这比所谓 `business-plan` 强太多。

---

### ② 强烈建议搭配：technical-writing

urlgwagjiug/technical-writinghttps://github.com/gwagjiug/technical-writing

这个我觉得也应该装。

它不是帮你“想研究内容”，而是负责把已经研究出来的东西写成**真正像技术方案的文字**。

它强调：

**Reader goal → structure → concrete statements → factual safety**

例如不会写这种 AI 味很重的话：

> 本系统采用先进的多源融合技术，大幅提升系统鲁棒性和智能化水平。

而会逼 AI 写成：

> 当 BDS 定位进入失锁状态时，系统停止使用对应 GNSS 观测量，并由 LiDAR/IMU/轮速里程计维持短时位姿估计；GNSS 恢复后，经异常跳变检测后重新参与融合。

它还自带：

- 结构规划
- 技术文档撰写
- 文档 Review
- 事实保护
- 模板
- Codex Skill
- Claude Code Skill

citeturn196666search1

**这对比赛计划书尤其重要，因为最怕一整篇全是“大幅提升、显著增强、有效解决”。**

---

### ③ 很强的研究方案 Skill：research-proposal-writer

这个在：

urljurgendn/agent-skillshttps://github.com/jurgendn/agent-skills

里面有专门的：

`research-proposal-writer`

它有几个设计我特别喜欢：

**不允许把未来结果写成已经实现；必须把 methodology 写成“为什么这种方法能回答这个问题”；必须做可检查 Milestone；必须做 risk → fallback，而且 fallback 也应该还能产生有效研究成果。** citeturn895107search2

这跟我们比赛非常契合。

比如：

```text
目标：
重定位成功率 >95%

风险：
原设计的主动探索策略在真实机器人上计算量过大。

Fallback：
采用退化检测 + 候选视点评分 + 局部主动运动，
仍然验证“主动动作是否提高重定位成功率”这一核心研究问题。
```

这就比“失败了再说”强很多。

---

### ④ 中文支持很好：luwill/research-skills

urlluwill/research-skillshttps://github.com/luwill/research-skills

这个仓库目前 GitHub 搜索结果里大约 **643 stars**，其中就有：

`research-proposal`

支持中文，而且有完整的：

**Requirements → Literature → Outline → Content → Output**

还有：

- STEM 模板
- 中文 Proposal scaffold
- 文献工作流
- Writing Style Guide
- Quality Checklist
- 图表建议

citeturn895107search4

但它原本偏 **PhD Research Proposal**，默认要求 2000–4000 字、至少 40 篇参考文献。

所以我不会原样照搬。

**可以借它的文献调研和结构，不要套它的 PhD 格式。**

---

### ⑤ 前期研究辅助：Academic Research Agent Skill

urlngtiendong/Academic-Research-Agent-Skillhttps://github.com/ngtiendong/Academic-Research-Agent-Skill

这个不是专门“写计划书”的。

但它对我们后面这些特别有价值：

```text
文献检索
↓
研究缺口判断
↓
创新点 Reality Gate
↓
数学/算法形式化
↓
实验计划
↓
Reviewer 模拟
↓
Claim Verification
```

也就是说，**防止 AI 自己编一个“创新算法”，实际上论文里五年前就有人做了。** citeturn895107search5

---

## 我建议我们别装一个，而是组合 3 个

你这个 `plan` 仓库，我建议最后形成：

```text
plan/

.skills/
或
.agents/skills/

├── dg-202611-proposal/
│   └── 我们自己的比赛专用 Skill
│
├── research-proposal-builder/
│   └── 科研方案设计
│
└── technical-writing/
    └── 技术写作与 Review
```

然后不要让 Codex简单地：

> “使用 research proposal skill 写一个计划书。”

而应该搞成一条流水线：

```text
比赛官方 PDF
        ↓
【要求提取】
每个评分点、硬指标、提交要求
        ↓
research-proposal-builder
        ↓
研究问题 / 创新点 / 技术路线 / 实验
        ↓
文献研究 Skill
        ↓
验证方案是否有依据、有创新性
        ↓
technical-writing
        ↓
把内容写成正式比赛方案
        ↓
Reviewer Skill
        ↓
按照比赛 100 分评分表反向审查
        ↓
DOCX
        ↓
最终 PDF
```

### 最关键的一步

我其实更建议：

**借这些 GitHub Skill 的好东西，然后给 `plan` 仓库专门做一个我们自己的 `dg-202611-proposal` Skill。**

因为别人写的 Skill 不知道这个比赛明确要求：

- 平面和高程定位精度优于 **20 cm**
- 有效特征重复率 ≥ **95%**
- 重定位成功率 > **95%**
- 北斗短报文样机
- 技术路线 20 分
- 模型合理性 20 分

fileciteturn41file1L55-L72

我们的 Skill 可以直接把**官方评分表当作写作规则**，甚至规定：

> 每写完一章，都必须指出该章对应哪个官方评分项；没有证据不能声称“已实现”；CURRENT 和 TARGET 必须严格区分。

这会比网上任何通用 Skill 都更适合这个比赛。

**所以我的选择是：`research-proposal-builder` + `technical-writing` 作为底层参考，然后我们自己做一个 `DG-202611` 专用 Skill。**

这样后面 Codex 写计划书时就不是泛泛地“写得像计划书”，而是**奔着这个比赛评分标准写**。
~~~~

## 消息 364 / user / 分段 1 of 1

~~~~text
先全面分析得到目前已经做到，后续需要做到，需要提交，先搞一个任务计划书出来
~~~~

## 消息 365 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "model_editable_context",
  "model_set_context": ""
}
~~~~

[本对话目录](README.md) | [总目录](../../README.md) | [Raw](https://raw.githubusercontent.com/weiyang02520-ops/dg202611-conversation-archive/main/conversations/first/part-003.md) | [上一卷](part-002.md) | [下一卷](part-004.md)
