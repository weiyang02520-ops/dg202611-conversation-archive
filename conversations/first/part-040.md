# 机器人定位挑战赛分析 / 第 040 卷

[本对话目录](README.md) | [总目录](../../README.md) | [Raw](https://raw.githubusercontent.com/weiyang02520-ops/dg202611-conversation-archive/main/conversations/first/part-040.md) | [上一卷](part-039.md) | [下一卷](part-041.md)

## 消息 1693 / assistant / 分段 2 of 2

~~~~text
====================================
十八、建立“冲突证据”
==================================================

如果不同论文结论不一致：

必须记录。

例如：

某论文认为毫米波雷达在恶劣环境优势明显；

另一篇指出点云稀疏或角分辨率不足。

不能只保留支持我们方案的论文。

生成：

STAGE_C_CONFLICTING_EVIDENCE_v1.0.md

记录：

争议点

支持方

限制方

条件差异

对DG-202611的影响。

==================================================
十九、建立“不可支撑论点”清单
==================================================

如果当前第1—8章存在某些论述：

目前找不到可靠来源；

或者来源不足；

或者表述过强；

单独生成：

STAGE_C_UNSUPPORTED_CLAIMS_v1.0.md

格式：

CLAIM:

CURRENT_LOCATION:

WHY_NOT_SUPPORTED:

WHAT_SOURCE_IS_NEEDED:

RECOMMENDED_ACTION:

不要本轮直接修改正文。

先报告。

==================================================
二十、参考文献格式
==================================================

生成：

STAGE_C_REFERENCE_LIBRARY_v1.0.md

以及如果条件允许：

STAGE_C_REFERENCE_LIBRARY_v1.0.bib

每个条目保存：

完整作者

完整题目

期刊/会议

年份

卷期页码

DOI

URL

访问时间。

后续方案书最终引用格式再统一。

本轮重点是：

数据完整。

==================================================
二十一、检索日志必须保存
==================================================

建立：

STAGE_C_SEARCH_LOG_v1.0.md

记录：

检索日期

检索平台

关键词

找到多少

筛掉多少

为什么筛掉

重要补充关键词

检索过程中发现的新方向。

因为以后老师问：

“这些论文怎么找的？”

我们要有过程记录。

==================================================
二十二、记录失败和无效来源
==================================================

建立：

STAGE_C_REJECTED_SOURCES_v1.0.md

记录典型被剔除资料：

论文不存在

DOI不匹配

只有二手转载

实验条件不透明

作者/年份错误

AI生成引用

厂商宣传稿

无法证明性能数字

重复论文

质量过低。

防止 DSH 后面重新把这些资料引用回来。

==================================================
二十三、禁止马上改正文
==================================================

非常重要：

本轮不要直接修改：

01_第1-2章调研框架_老师意见同步_v1.1.md

05_第3-5章初稿_v2.2.1_老师意见同步.md

06_第6-8章模型初稿_v1.2.md

DG-202611_方案书当前整稿预览_vCurrent.md

DG-202611_方案书当前整稿预览_vCurrent.docx

本轮只建立：

研究证据。

等我审核阶段 C 研究成果以后，再单独批准：

“第2章正式成文化”。

==================================================
二十四、不要让DSH独立搜论文
==================================================

本轮真实文献检索和真实性核验由当前文档主管线程负责。

DSH不能成为文献真实性裁判。

如果使用 DSH：

最多用于：

整理

分类

语言归纳。

但：

论文是否真实

DOI是否正确

论文到底写了什么

实验数据到底是什么

必须由当前线程根据原始来源核验。

DSH输出不得直接进入证据库而不复核。

==================================================
二十五、开发线边界
==================================================

本轮：

不SSH

不改ROS2

不联系Claude Code

不执行S01—S08

不做Gazebo

不做Jetson

不做真车

不修改算法。

如果文献研究发现：

某项技术很有价值，

只记录：

DEVELOPMENT_RECOMMENDATION_CANDIDATE

不得启动开发。

==================================================
二十六、与当前开发事实保持隔离
==================================================

文献中别人做到：

5cm

99%

厘米级RTK

高成功率重定位

不代表 DG-202611 做到了。

所有外部数据必须明确标识：

LITERATURE_REPORTED_RESULT

不得写成：

DG-202611_RESULT。

==================================================
二十七、本轮建议创建文件
==================================================

在当前 DG-202611 文档工作区创建研究目录或沿用现有研究目录。

至少形成：

STAGE_C_MASTER_SOURCE_TABLE_v1.0.md

STAGE_C_CORE_PAPER_CARDS_v1.0.md

STAGE_C_CITATION_MATRIX_v1.0.md

STAGE_C_CLAIM_EVIDENCE_MATRIX_v1.0.md

STAGE_C_RESEARCH_SYNTHESIS_v1.0.md

STAGE_C_TECHNOLOGY_CANDIDATE_REVIEW_v1.0.md

STAGE_C_CONFLICTING_EVIDENCE_v1.0.md

STAGE_C_UNSUPPORTED_CLAIMS_v1.0.md

STAGE_C_REFERENCE_LIBRARY_v1.0.md

STAGE_C_SEARCH_LOG_v1.0.md

STAGE_C_REJECTED_SOURCES_v1.0.md

如可行：

STAGE_C_REFERENCE_LIBRARY_v1.0.bib

STAGE_C_MASTER_SOURCE_TABLE_v1.0.csv

不要覆盖历史研究文件。

==================================================
二十八、MASTER SOURCE TABLE
==================================================

总表至少包含：

ID

Topic

Title

Authors

Year

Venue

DOI

URL

Source Type

Peer Reviewed

Official Source

Classic/Recent

Domestic/International

Sensors

Method

Scenario

Dataset

Real Robot / Simulation / Dataset

Metrics

Key Result

Limitations

Chapter Support

Claim Support

Evidence Strength

Verification Status

Notes。

Verification Status 只允许：

VERIFIED

PARTIALLY_VERIFIED

UNVERIFIED

REJECTED。

正式引用候选只允许：

VERIFIED。

==================================================
二十九、证据强度
==================================================

统一使用：

LEVEL_A

同行评议论文原文 / 官方标准 / 官方系统文档。

LEVEL_B

正式厂商技术手册 / 官方开源项目技术文档。

LEVEL_C

预印本 / 高质量技术报告。

LEVEL_D

二手资料。

LEVEL_D 不进入正式引用库。

==================================================
三十、阶段完成门槛
==================================================

本轮不能因为“找到几十篇搜索结果”就宣布完成。

必须同时满足：

真实来源数量达到合理规模；

核心五方向都有可靠论文；

至少15篇核心论文精读；

DOI/出版信息核验；

引用矩阵完成；

论点证据矩阵完成；

研究综合完成；

技术候选评价完成；

冲突证据完成；

不可支撑论点完成；

检索日志完成；

参考文献候选库完成。

如果任何关键方向资料明显不足：

状态：

PARTIAL

并继续检索。

==================================================
三十一、最终输出
==================================================

完成后返回：

STAGE_C_STATUS:

TOTAL_SOURCES_FOUND:

TOTAL_VERIFIED_SOURCES:

TOTAL_REJECTED_SOURCES:

CORE_PAPERS_READ:

CLASSIC_SOURCES:

RECENT_2019_2026_SOURCES:

DOMESTIC_SOURCES:

INTERNATIONAL_SOURCES:

OFFICIAL_SOURCES:

TOPIC_A_BDS_GNSS:
count:
assessment:

TOPIC_B_ADAPTIVE_FUSION:
count:
assessment:

TOPIC_C_LIDAR_DEGENERATION:
count:
assessment:

TOPIC_D_DYNAMIC_LIDAR:
count:
assessment:

TOPIC_E_ACTIVE_RELOCALIZATION:
count:
assessment:

TOPIC_F_VIO_ALTITUDE:
count:
assessment:

TOPIC_G_MMWAVE_RADAR:
count:
assessment:

TOPIC_H_BDS_SHORT_MESSAGE:
count:
assessment:

TOP_15_CORE_REFERENCES:

MOST_IMPORTANT_FINDINGS:

MOST_IMPORTANT_LIMITATIONS:

CONFLICTING_EVIDENCE_FOUND:

UNSUPPORTED_CURRENT_DOCUMENT_CLAIMS:

RESEARCH_GAPS:

TECHNOLOGY_CANDIDATE_FINDINGS:

ZED_VIO_EVIDENCE_STATUS:

MMWAVE_RADAR_EVIDENCE_STATUS:

ALTITUDE_CHANNEL_EVIDENCE_STATUS:

ACTIVE_RELOCALIZATION_EVIDENCE_STATUS:

BDS_SHORT_MESSAGE_EVIDENCE_STATUS:

READY_TO_WRITE_FORMAL_CHAPTER_2:
YES/NO

READY_TO_SUPPORT_CHAPTER_6:
YES/NO

READY_TO_SUPPORT_CHAPTER_7:
YES/NO

READY_TO_SUPPORT_CHAPTER_8:
YES/NO

FILES_CREATED:

FILES_MODIFIED:

CURRENT_DOCUMENT_SOURCE_FILES_MODIFIED:
NO

DSH_USED:
YES/NO

DSH_ROLE:

DEVELOPMENT_LINE_CONTACTED:
NO

RECOMMENDED_NEXT_DOCUMENT_ACTION:

==================================================
三十二、完成后停止
==================================================

本轮完成：

真实文献检索

真实性核验

核心论文精读

引用矩阵

论点证据矩阵

研究综合

候选技术评价

冲突证据

不可支撑论点

正式参考文献候选库

之后停止。

不要直接开始重写第2章。

不要自行修改第6—8章。

不要启动新技术开发。

我要先审核这批真实文献和研究结论，再决定是否批准“第2章正式成文化”。

——DG-202611 STAGE C REAL LITERATURE RESEARCH END——
~~~~

## 消息 1694 / user / 分段 1 of 1

~~~~text
## Verified so far

Pre-flight matched expectations exactly: branch `dg202611-synthetic-validation`, HEAD `57ec8ec`, parent `fd2c60d`, clean tree, `c41adca` confirmed as ancestor.

The state machine analysis is done and is the load-bearing result of this round. The real state names are `NORMAL / SUSPECTED / TRIGGERED / STOPPING / ACTIVE_SCAN / WAITING_CANDIDATE / VERIFYING / RECOVERED / FAILED / MANUAL_REQUIRED` — `LOST` and `SEARCHING` do not exist, so I dropped them. The finding that reshapes the scenario design: `assess_health` splits causes into `trigger_reasons` and `degraded_reasons`, and LiDAR-quality-low plus GNSS DEGRADED/REJECTED land in **degraded-only**. S02–S04-style degradation therefore *cannot* trigger active relocalization by construction. That is the concrete reason the old `RELOCALIZATION_ACTIVITY_OBSERVED_OUT_OF_SCOPE` warning must not be read as closed-loop success. Real trigger paths are AMCL covariance, AMCL/scan staleness, and scan-match quality.

The decisive check: I ran the real `refine_pose_near_seed` from [scan\_map\_relocalization\_node.py](vscode-webview://0f4m7pn0sninoa4t6baiuusea0qhmsg1nfkqf92pri999t1ojc4h/src/ylhb_base/scripts/scan_map_relocalization_node.py) offline against ray-cast geometry. It accepted with score 0.975, inlier\_ratio 1.000, mean\_distance 0.009 against thresholds 0.45/0.45/0.30, and recovered true +10° and −10° yaw from a 0° seed in 0.15 s per match. **No threshold or state-machine change is needed** — that was the main risk of hitting your stop condition 1 or 2, and it's cleared.

## Two design decisions you should weigh in on

Both are within `dg_synthetic_validation`, neither touches core algorithm code, but both widen what the injector does, so I don't want them buried in a final report.

The closed loop needs a **synthetic base plant**: something must consume `/dg/test_cmd_vel` and integrate it into `/odom` and `/amcl_pose`, or the robot never turns and `ACTIVE_SCAN` dies on `SEGMENT_TIMEOUT`. Currently `linear.x` is pinned at 0.05 > `stop_velocity` 0.03, so `STOPPING` can never even confirm. The plant models the *body*, publishes no `/dg/*`, and never touches `/cmd_vel` — but it is a genuine feedback path from an algorithm output back into synthetic input.

Second, S07 needs TF. There is no TF publisher anywhere in the integration launch, so `_scan_to_base_points` always returns `TF_FAILED` today. I opted to have the plant publish `map→odom→base_footprint→laser`, which is what a real base driver plus `robot_state_publisher` provide, and which RViz needs anyway. That takes the injector from 7 topics to 7 + `/tf` + `/tf_static`, so it is a documented widening of the injector boundary in your section 27 list.

Neither is faking `/dg/*` output and neither lowers a threshold, so I read them as allowed under section 9 rather than stop conditions — but the boundary change is yours to ratify, and I'd rather hear that now than at the evidence checkpoint.

## Remaining work

Still to do: `result_writer` columns and real S05–S08 assertions, S05–S08 scenario YAMLs, the enhanced monitor, the new plots, RViz config, the `ffmpeg -f x11grab` capture helper plus manifests, and the three docs. `AUTOMATIC_GUI_CAPTURE` looks **available** with no new software — `gnome-screenshot`/`scrot`/`import`/`maim` are all absent but `xwd` and `ffmpeg` are present, and a GUI session exists on tty2. `tmux` is confirmed absent and I will not install it.

One expectation to set now, so it isn't a surprise in the S08 report: after `RECOVERED`, the arbiter will select NAV and then emit zero with `NAV_SOURCE_STALE`, because Nav2 is intentionally disabled and nothing publishes `/cmd_vel_nav`. That is correct, safe control release, but it is *not* navigation resuming motion, and I won't write it up as such. Publishing synthetic `/cmd_vel_nav` to make it look otherwise would be fabricating a control input, so I'm not doing it.

I'll continue with `result_writer` and the scenario YAMLs unless you want the plant or TF decision changed first.
~~~~

## 消息 1695 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "model_editable_context",
  "model_set_context": ""
}
~~~~

## 消息 1696 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "thoughts",
  "thoughts": [
    {
      "summary": "评估仿真方案",
      "content": "我认可报告中的两项决定，但会把边界写清：动力学节点负责闭环运动、里程计和TF；定位输出必须包含可控噪声、延迟与失效，不能直接等同真值。随后继续分层验证各故障场景与日志证据。",
      "chunks": [],
      "finished": true
    },
    {
      "summary": "评估了仿真方案",
      "content": "",
      "chunks": [],
      "finished": true
    }
  ],
  "source_analysis_msg_id": "edf87394-60cc-4b40-852b-7950ad1a1a15"
}
~~~~

## 消息 1697 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "thoughts",
  "thoughts": [
    {
      "summary": "收紧仿真边界",
      "content": "我会批准基础运动学闭环，但把定位模拟单独标注为合成定位源。TF只保留必要链路，避免发布未经说明的真值变换；里程计、定位噪声、延迟和重定位响应都必须可追溯、可注入、可审计。",
      "chunks": [],
      "finished": true
    },
    {
      "summary": "收紧仿真边界",
      "content": "",
      "chunks": [],
      "finished": true
    }
  ],
  "source_analysis_msg_id": "71586110-8c62-49b7-ad9d-e0145112cd24"
}
~~~~

## 消息 1698 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "reasoning_recap",
  "content": "思考了 26s"
}
~~~~

## 消息 1699 / assistant / 分段 1 of 1

~~~~text
这份分析我认可，而且它把 S05～S08 最关键的逻辑弄清楚了。**可以继续，但我会对 plant 和 TF 加两个边界，防止我们又把“合成闭环”做得太像真实机器人而产生证据污染。**

第一，状态机结论直接接受。真实状态就是 `NORMAL / SUSPECTED / TRIGGERED / STOPPING / ACTIVE_SCAN / WAITING_CANDIDATE / VERIFYING / RECOVERED / FAILED / MANUAL_REQUIRED`，以后文档、图、Monitor 都只准用这些真实状态，`LOST / SEARCHING` 删除。更重要的是，GNSS DEGRADED/REJECTED + LiDAR quality low 本身只是 degraded-only，**不能触发 active relocalization**。所以 S05 必须使用真实 trigger path，例如 AMCL covariance、AMCL/scan stale 或 scan-match quality，不能为了“让流程跑起来”改状态机。

第二，**synthetic base plant 批准**。它确实是 S05～S08 做闭环所需要的，否则 `/dg/test_cmd_vel` 发出来之后机器人状态完全不动，`ACTIVE_SCAN` 永远无法完成。但必须把它明确定义成：

> `SYNTHETIC_KINEMATIC_PLANT`

而不是机器人模拟器，更不能叫真实底盘。它只负责把 `/dg/test_cmd_vel` 按明确的差速/平面运动模型积分成 synthetic body motion。所有运动学假设、积分方法、更新频率、初始状态都要写进过程记录。

不过 `/amcl_pose` 要特别谨慎。**如果 plant 也生成 `/amcl_pose`，必须把这一部分单独定义成 synthetic localization surrogate，而不是“body plant”。**尤其如果它还会响应 `/initialpose`，那就是在模拟 AMCL 的恢复行为。可以做，但报告必须明确：

`SYNTHETIC_AMCL_SURROGATE`

不能写成真实 AMCL 恢复成功。

第三，TF 我只**部分批准**。

允许：

```text
odom -> base_footprint
base_footprint -> laser
```

其中 `base_footprint -> laser` 可以是 `/tf_static`。

但是：

> **不要默认让 plant 发布一个完美的 `map -> odom` 真值 TF。**

原因很简单：`map -> odom` 本来就是定位链的重要产物。如果 synthetic plant 直接把真实 map pose 塞进去，很容易形成 ground-truth leakage，最后 RViz 看起来特别漂亮，但算法实际上借到了答案。

如果 S07 真正只需要 `_scan_to_base_points`，那它应该只需要激光雷达到 base 的 TF，根本没必要靠完美 `map -> odom`。

如果实现过程中确认**确实必须有 `map -> odom`**，Claude 必须先证明：

1. 哪个真实代码路径需要它；
2. 它是否会进入被测算法；
3. 它是否会影响候选匹配结果；
4. 是否可以用独立 `ground_truth` frame 代替；
5. 如果不能代替，为什么不会造成答案泄漏。

没有这份说明，不准把 perfect `map→odom` 直接加进去。

第四，`refine_pose_near_seed` 这次离线验证很好。`score=0.975 / inlier=1.000 / mean_distance=0.009`，并且 ±10° 能恢复，说明**当前核心算法阈值暂时没有必要碰**。继续保持原规则：

> 核心阈值、核心状态机、核心算法语义一旦需要修改，就停下来报告，不得为了让 S05～S08 PASS 而调参。

第五，S08 后面的 `NAV_SOURCE_STALE` 我同意 Claude 的判断。**不要造 `/cmd_vel_nav`。**

S08 可以证明：

```text
恢复判定完成
→ arbiter 控制权释放回 NAV
→ 由于没有真实 Nav2 cmd source
→ 安全输出仍为 0
→ NAV_SOURCE_STALE
```

这叫：

> **safe control handoff verified in synthetic software validation**

不能叫：

> “恢复正常导航并继续行驶”。

这反而是更可信的结果。

所以我的批准结论是：

```text
STATE_MACHINE_ANALYSIS: APPROVED

SYNTHETIC_BASE_PLANT: APPROVED_WITH_BOUNDARIES

SYNTHETIC_AMCL_SURROGATE:
ALLOWED_IF_EXPLICITLY_SEPARATED_AND_DOCUMENTED

TF_STATIC_BASE_TO_LASER: APPROVED

TF_ODOM_TO_BASE: APPROVED

PERFECT_MAP_TO_ODOM_FROM_PLANT:
NOT_APPROVED_BY_DEFAULT
REQUIRES_JUSTIFICATION

CORE_THRESHOLD_CHANGE:
NOT_ALLOWED

CORE_STATE_MACHINE_CHANGE:
NOT_ALLOWED

SYNTHETIC_CMD_VEL_NAV:
NOT_ALLOWED

S08_NAV_SOURCE_STALE:
EXPECTED_AND_ACCEPTABLE
```

让 Claude 继续做 `result_writer + S05～S08 YAML + monitor + plots + RViz + evidence`。

并且别忘了我们刚刚定下的永久规则：**这一轮所有设计决定、失败过程、为什么需要 plant、TF 为什么这样处理、每次实验命令和结果，都必须进入 development process record。**

到最终正式证据运行之前，还是必须停在：

`VISIBLE_EVIDENCE_CHECKPOINT`

你在 VMware 把 Monitor / RViz 打开之后，再给它 `VISIBLE_READY`，然后才跑最终 S05～S08。memcite
~~~~

## 消息 1700 / user / 分段 1 of 1

~~~~text
啥呀，提示词我看看
~~~~

## 消息 1701 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "reasoning_recap",
  "content": "思考了 8s"
}
~~~~

[本对话目录](README.md) | [总目录](../../README.md) | [Raw](https://raw.githubusercontent.com/weiyang02520-ops/dg202611-conversation-archive/main/conversations/first/part-040.md) | [上一卷](part-039.md) | [下一卷](part-041.md)
