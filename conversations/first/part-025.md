# 机器人定位挑战赛分析 / 第 025 卷

[本对话目录](README.md) | [总目录](../../README.md) | [Raw](https://raw.githubusercontent.com/weiyang02520-ops/dg202611-conversation-archive/main/conversations/first/part-025.md) | [上一卷](part-024.md) | [下一卷](part-026.md)

## 消息 1426 / assistant / 分段 2 of 2

~~~~text
出：

/dg/gnss/quality
/dg/gnss/accepted_fix
/dg/lidar/filtered_scan
/dg/lidar/stable_features
/dg/lidar/quality
/dg/relocalization/status
/dg/relocalization/match_quality
/dg/fusion/odom
/dg/fusion/pose
/dg/fusion/status
/dg/navigation/status
/dg/test_cmd_vel

以及：

/rosout
/parameter_events

如果实际topic名称不同，以运行时接口为准。

不要录不存在的大量无关topic。

==================================================
二十二、CSV输出
==================================================

每个场景输出：

samples.csv

至少包含在当前代码可获得情况下：

scenario_id
phase
elapsed_time
ros_timestamp

GNSS：
gnss_state
gnss_decision
gnss_satellites
gnss_hdop
gnss_differential_age
gnss_accepted

LiDAR：
lidar_state
raw_point_count
valid_point_count
stable_point_count
dynamic_candidate_count
temporal_match_ratio
angular_coverage
geometry_score

Fusion：
fusion_mode
accepted_source
measurement_confidence
position_uncertainty
yaw_uncertainty
adaptive_position_uncertainty
adaptive_yaw_uncertainty

Navigation：
navigation_state
relocalization_state

Control：
cmd_vel_source
cmd_linear_x
cmd_angular_z

实际字段如果不存在：

不要伪造。

使用空值并在 README 说明。

==================================================
二十三、状态时间线
==================================================

每个场景必须输出：

timeline.csv

只记录状态变化事件。

建议字段：

timestamp
elapsed_time
component
previous_state
new_state
reason
phase

例如：

GNSS
LiDAR
Fusion
Navigation Health
Relocalization
cmd_vel source

不要每个采样周期重复相同状态。

==================================================
二十四、自动评价
==================================================

每个场景必须输出：

result.json

以及便于人阅读的：

result.md

至少包含：

scenario_id
scenario_name
start_time
duration
seed

build_commit
baseline_commit

evidence_level:
SYNTHETIC_SOFTWARE_VALIDATION

gazebo:
false

real_robot:
false

performance_claim:
false

overall_result:
PASS / FAIL

checks:
逐项检查名称、结果、证据时间点、观察值、期望值

failures:
失败原因

warnings:
警告

artifacts:
bag路径
CSV路径
timeline路径
日志路径

==================================================
二十五、真实性保护
==================================================

所有结果文件必须明确包含：

THIS_IS_SYNTHETIC_SOFTWARE_VALIDATION

以及：

NOT_REAL_ROBOT_DATA
NOT_GAZEBO_DATA
NOT_COMPETITION_PERFORMANCE_EVIDENCE

可以写入 metadata 或 README。

目的：

避免以后将这些数据错误当成：

20cm定位结果
95%特征重复率
>95%重定位成功率
实车实验结果

==================================================
二十六、输出目录
==================================================

建议统一输出到：

/home/weiyang/dg202611_ws/results/synthetic/

不要把大 rosbag 提交到 Git 仓库。

每次运行例如：

results/synthetic/S01_nominal/<run_id>/
results/synthetic/S02_gnss_degradation/<run_id>/
results/synthetic/S03_lidar_geometry_degradation/<run_id>/
results/synthetic/S04_concurrent_degradation/<run_id>/

每个目录包含：

metadata.json
samples.csv
timeline.csv
result.json
result.md
rosbag/
logs/

如果工作区已有更合适的 ignored results 路径，可以使用实际路径。

确保 rosbag、CSV生成物不会被误提交。

==================================================
二十七、汇总报告
==================================================

运行 S01～S04 后生成：

results/synthetic/summary.csv

至少：

scenario
result
duration
important_transition
failure_count
warning_count
artifact_path

再生成：

results/synthetic/summary.md

说明：

S01 PASS/FAIL
S02 PASS/FAIL
S03 PASS/FAIL
S04 PASS/FAIL

不能把它写成：

“比赛性能测试通过”。

只能称：

“synthetic software validation”。

==================================================
二十八、测试要求
==================================================

新增 package 自身必须有必要的单元测试。

重点测试：

1. scenario YAML解析；
2. deterministic输入生成；
3. state timeline去重；
4. CSV字段输出；
5. PASS/FAIL规则；
6. result metadata真实性标签；
7. 输出目录创建；
8. 禁止写真实 /cmd_vel 的安全检查。

然后重新运行现有比赛核心测试。

核心算法测试必须保持通过。

如果以前：

197 passed / 0 failed

因为第三轮测试增加而数量变化是正常的。

但不得出现已有比赛核心测试回归。

==================================================
二十九、构建与运行
==================================================

优先构建：

ylhb_interfaces
ylhb_base
dg_synthetic_validation

以及实际必要依赖。

不要因为 ZED / CUDA / TensorRT 阻塞本轮。

本轮验收不要求完整仓库构建。

==================================================
三十、推荐自动运行方式
==================================================

最终最好能做到类似：

run_scenario S01
run_scenario S02
run_scenario S03
run_scenario S04

以及：

run_all_s01_s04

具体CLI形式由你根据ROS2 package规范设计。

必须做到：

单条命令自动：

启动比赛integration launch
→
启动injector
→
启动evaluator
→
启动rosbag
→
运行场景
→
正常结束
→
停止bag
→
输出CSV
→
输出timeline
→
输出PASS/FAIL
→
清理ROS进程

不要要求用户手动开六七个terminal。

==================================================
三十一、进程清理
==================================================

这是强制要求。

每个场景结束后必须确认：

没有遗留：

ros2 launch
synthetic injector
ros2 bag record
evaluator
比赛测试节点

避免下一个场景受到上一个场景影响。

如果发现残留：

当前场景标记warning或fail，并清理。

不能用危险的：

killall python3

等粗暴命令。

只清理由当前场景启动并记录PID/进程组的进程。

==================================================
三十二、运行S01～S04
==================================================

开发完成后必须实际运行：

S01
S02
S03
S04

不得只写代码不运行。

每个场景至少完整运行一次。

如果某场景FAIL：

保留bag、CSV、日志和result。

不要修改核心算法使它“强行PASS”。

如果问题属于：

synthetic framework bug

可以修测试框架。

如果问题属于：

核心比赛算法行为

记录BLOCKER并停止对该算法的修改。

==================================================
三十三、禁止性能数字包装
==================================================

本轮输出不得出现：

“定位精度达到X cm”

“特征重复率达到X%”

“重定位成功率达到X%”

除非明确前缀：

synthetic diagnostic value

但原则上第三轮 A 根本不需要计算这三个官方最终指标。

本轮只评价：

状态机
质量门控
融合模式
健康状态
安全状态
软件稳定性

==================================================
三十四、完成后的Git检查
==================================================

完成后：

git status
git diff <第二轮提交>..HEAD

检查：

- 没有算法核心意外修改；
- 没有 rosbag；
- 没有 CSV结果；
- 没有 build/install/log；
- 没有大型二进制文件；
- 没有虚拟机私有配置；
- 没有真实路径敏感数据误提交。

提交只包含：

synthetic validation框架
scenario配置
测试
必要文档
必要最小launch适配

==================================================
三十五、提交
==================================================

如果完成：

commit message建议：

feat(test): add DG synthetic degradation validation

本地提交。

不要 push。

==================================================
三十六、最终报告格式
==================================================

完成后立即停止，不继续 S05～S08，不开始 Gazebo。

返回以下报告：

BASELINE:
c41adca7f9bddb4240a1a4855437518b36d9fe13

RUNTIME_INTEGRATION_BASE:
096bf66377e0dbaa3ed812782222ea95a2439c7a

WORK_BRANCH:

NEW_COMMIT:

FILES_CHANGED:

NEW_PACKAGE:

CORE_ALGORITHM_FILES_MODIFIED:
YES/NO
details:

BUILD:
command:
result:

TESTS:
new_framework:
existing_dg_core:
failures:

RUNTIME_CONTRACT:
path:
verified_topics:
verified_states:

S01_NOMINAL:
result:
important_transitions:
warnings:
artifact_path:

S02_GNSS_DEGRADATION:
result:
important_transitions:
warnings:
artifact_path:

S03_LIDAR_GEOMETRY_DEGRADATION:
result:
important_transitions:
warnings:
artifact_path:

S04_CONCURRENT_DEGRADATION:
result:
important_transitions:
warnings:
artifact_path:

ROSBAGS:
S01:
S02:
S03:
S04:

CSV:
S01:
S02:
S03:
S04:

TIMELINES:
S01:
S02:
S03:
S04:

SUMMARY:
path:

CMD_VEL_SAFETY:
real_cmd_vel_used:
test_topic:
result:

REAL_ROBOT:
NO

GAZEBO:
NO

ZED_CUDA_TENSORRT:
NOT_USED

OFFICIAL_20CM_CLAIM:
NO

OFFICIAL_FEATURE_95_CLAIM:
NO

OFFICIAL_RELOCALIZATION_95_CLAIM:
NO

SYNTHETIC_RESULTS_LABELLED:
YES/NO

GIT_STATUS:

BLOCKERS:

DISCOVERED_CORE_ALGORITHM_ISSUES:

NEXT_RECOMMENDED_STEP:

==================================================
三十七、停止条件
==================================================

以下情况立即停止并汇报：

1. 必须修改核心比赛算法才能使场景通过；
2. 发现第二轮本地commit不存在；
3. 工作区存在未知用户修改；
4. 测试链可能写真实 /cmd_vel；
5. 发现真实机器人连接；
6. 需要Gazebo才能继续；
7. 需要ZED/CUDA/TensorRT才能继续；
8. 需要升级系统或ROS2；
9. 需要删除用户源文件；
10. 测试框架范围开始扩展到S05～S08；
11. 开始产生或包装官方性能指标。

==================================================
三十八、本轮唯一验收目标
==================================================

本轮不是验证机器人“性能有多强”。

本轮只验证：

“当前已经完成ROS2集成的DG-202611软件POC，在收到可控、可重复的合成GNSS/LiDAR/Odom/AMCL输入以后，能否产生符合当前代码设计的质量状态、融合状态和导航健康变化，并将全过程自动保存成可复核证据。”

完成 S01～S04 后：

停止。

等待外部审核。

——提示词结束——

这轮我特别加了一个东西：**每份结果强制标记 `SYNTHETIC_SOFTWARE_VALIDATION`**。以后材料越积越多时，这能避免 DeepSeek/Codex 或我们自己误把这批数据写成真车比赛指标。

等 Codex 跑完，把它的最终报告直接贴回来。到时候重点不是只看“四个 PASS”，我会主要审 **状态变化有没有真的符合算法逻辑、PASS规则有没有被写得太松、有没有为了通过测试偷偷绕过真实节点输出**。memcite
~~~~

## 消息 1427 / user / 分段 1 of 1

~~~~text
我有个问题，这些数据跑的时候，能可视化让我看看不
~~~~

## 消息 1429 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "thoughts",
  "thoughts": [
    {
      "summary": "规划可视化方式",
      "content": "可以。我会保留验证主线，同时提供可选实时查看：用 RViz2 看激光、地图、位姿和路径，用 rqt_plot 看 HDOP、几何评分、置信度等曲线；测试结束后再生成时间线图，方便回放分析。",
      "chunks": [],
      "finished": true
    },
    {
      "summary": "规划了可视化方案",
      "content": "",
      "chunks": [],
      "finished": true
    }
  ],
  "source_analysis_msg_id": "5fa8d784-d94a-46f8-8cb4-b0b6fbfe6d7d"
}
~~~~

## 消息 1430 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "reasoning_recap",
  "content": "思考了 17s"
}
~~~~

## 消息 1431 / assistant / 分段 1 of 1

~~~~text
可以，而且**非常建议你看**。不然 AI 跑完只丢给你 CSV 和 PASS/FAIL，你其实很难直观理解到底发生了什么。

你这轮虽然不做 Gazebo，但照样能可视化。可以分成两种。

### 第一种：实时看机器人“数据世界”

直接在 Ubuntu 虚拟机里开 **RViz2**。

S01～S04 跑的时候，你可以看到类似：

- 地图 `/map`
- 原始雷达 `/scan`
- 过滤后的雷达 `/dg/lidar/filtered_scan`
- 机器人 `/odom` 位姿
- AMCL 位姿
- 融合后的 `/dg/fusion/pose`
- TF 坐标系

所以到 S03 LiDAR 退化的时候，你会很直观地看到：

正常：

> 雷达一圈比较完整

然后慢慢变成：

> 只剩窄窄一块有效扫描

这时候 `geometry_score` 同时开始下降。

虽然没有 Gazebo 机器人模型，但你能看到**传感器、地图、轨迹、定位结果在动**。

---

### 第二种：实时看“算法脑子里在想什么”

这个其实对比赛更重要。

建议让 Codex 再做一个**只读监控面板**，显示：

```text
时间：12.5 s

GNSS
状态：DEGRADED
卫星：5
HDOP：2.4
置信度：0.51

LiDAR
状态：GOOD
有效点：318
稳定点：271
Geometry Score：0.76

Fusion
模式：GNSS_AIDED
接受源：GNSS
Measurement Confidence：0.51
Position Uncertainty：...

Navigation
状态：DEGRADED

Relocalization
状态：NORMAL

CMD_VEL
来源：NAV
线速度：0
角速度：0
```

等到 S04：

```text
GNSS: GOOD
LiDAR: GOOD
Fusion: NOMINAL

        ↓

GNSS: DEGRADED
LiDAR: DEGRADED
Fusion: DEGRADED

        ↓

GNSS: REJECTED
LiDAR: DEGRADED
Fusion: DEAD_RECKONING
Navigation: DEGRADED
```

你就会亲眼看到整个比赛算法在发生状态切换。

### 而且还能自动画曲线

一场跑完以后自动生成几张图，例如：

**图1**

```text
HDOP
  ↑
3 ┤             ╭────
2 ┤        ╭────╯
1 ┤────────╯
  └────────────────→ 时间

GNSS Confidence
1 ┤────────╮
  ┤        ╰────╮
0 ┤             ╰────
```

**图2**

```text
LiDAR Geometry Score
1.0 ┤───────╮
0.8 ┤       │
0.5 ┤       ╰─────
0.2 ┤             ╰──
```

**图3**

把这些叠在一张时间线上：

```text
0s       5s       10s       15s

GNSS     GOOD → DEGRADED → REJECTED
LiDAR    GOOD → GOOD     → DEGRADED
Fusion   NORMAL → AIDED  → DEAD_RECKONING
Nav      NOMINAL         → DEGRADED
```

这种图以后方案书里都可能用得上，只要明确标：

> **合成数据软件验证**

而不是实车成绩。

---

所以我建议第三轮 A **加一个很小的可视化要求**，但不改变主要任务。

你可以把下面这段追加给 Codex：

> 增加 OPTIONAL_VISUALIZATION，不改变本轮验收范围。
>
> 在不修改任何核心算法的前提下，为 S01～S04 提供可选的只读可视化。
>
> 优先实现：
>
> 1. 提供一个 RViz2 配置，用于显示 `/map`、`/scan`、过滤后的LiDAR数据、odom/AMCL/fusion pose及相关TF；
> 2. 提供一个轻量状态监视方式，实时显示 GNSS state、satellites、HDOP、LiDAR geometry score、temporal match ratio、fusion mode、measurement confidence、navigation state、relocalization state 和 `/dg/test_cmd_vel`；
> 3. 每个场景完成后根据 `samples.csv` 自动生成主要状态/质量指标随时间变化的曲线和状态时间线图；
> 4. 可视化只是观察工具，不参与 PASS/FAIL 判定；
> 5. 不得为了可视化修改核心算法；
> 6. 如果 SSH/headless 环境无法实时显示GUI，必须仍能自动保存静态图到场景结果目录，供之后查看。
>
> 输出建议：
> `plots/gnss_quality.png`
> `plots/lidar_quality.png`
> `plots/fusion_confidence.png`
> `plots/state_timeline.png`

这样最舒服。

**你甚至可以在 Codex 后台跑测试的时候，自己打开 Ubuntu 虚拟机窗口，看 RViz 和状态监控实时变化。**

等以后做到 Gazebo，那才会进一步变成：

> 左边真的有一辆机器人在环境里运动，右边 RViz 显示雷达/地图/轨迹，同时下面曲线实时变化。

那个视觉效果就会非常直观。memcite
~~~~

## 消息 1432 / user / 分段 1 of 1

~~~~text
怎么给提示词呢
~~~~

## 消息 1433 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "model_editable_context",
  "model_set_context": ""
}
~~~~

## 消息 1434 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "reasoning_recap",
  "content": "思考了 7s"
}
~~~~

## 消息 1435 / assistant / 分段 1 of 1

~~~~text
你就把下面这段，直接追加到刚才“第三轮 A”提示词最后面就行，不需要重发整份。

追加要求：可视化观察功能

在不改变第三轮 A 原有范围、不修改核心算法、不影响 PASS/FAIL 判定的前提下，增加“只读可视化观察功能”。

目标是让我在 S01～S04 合成测试运行时，能够直观看到传感器数据、算法状态和状态变化，而不是只能在测试结束后查看 CSV。

一、实时 RViz2 可视化

为第三轮 A 提供一份可直接打开的 RViz2 配置。

优先显示实际存在的话题：

- /map
- /scan
- /dg/lidar/filtered_scan
- /odom
- /amcl_pose
- /dg/fusion/pose
- TF / TF Static

如 stable_features 能以 RViz 支持的消息类型显示，也加入；如果当前消息格式不适合直接显示，不要为了 RViz 修改核心算法或消息定义。

要求：

1. 提供 rviz 配置文件；
2. 提供启动方式；
3. 可以在 synthetic scenario 正在运行时打开；
4. 不参与测试结果判定；
5. 不因为 RViz 缺失导致场景 FAIL；
6. 如果 SSH 环境无法显示 GUI，应正常跳过 GUI，不得影响自动测试。

二、实时状态监视器

增加一个轻量、只读的状态监控方式。

优先使用终端实时刷新，不要为了这个功能引入大型 Web 框架。

实时显示当前实际能够获得的字段，例如：

SCENARIO:
PHASE:
ELAPSED:

GNSS
- state
- decision
- satellites
- HDOP
- differential_age
- accepted

LiDAR
- state
- raw_point_count
- valid_point_count
- stable_point_count
- dynamic_candidate_count
- temporal_match_ratio
- angular_coverage
- geometry_score

Fusion
- fusion_mode
- accepted_source
- measurement_confidence
- position_uncertainty
- yaw_uncertainty
- adaptive_position_uncertainty
- adaptive_yaw_uncertainty

Navigation
- navigation_state

Relocalization
- relocalization_state

Control
- cmd_vel source
- linear_x
- angular_z

实际代码没有的字段不得伪造。

无法获得时显示 N/A。

要求：

1. 状态监视器只能订阅现有话题；
2. 不得发布控制消息；
3. 不得影响算法行为；
4. 不得成为 PASS/FAIL 的数据来源替代品；
5. 可以通过单独参数启用或关闭，例如 visualization:=true / false，具体实现按 ROS2 规范设计。

三、场景结束后的自动静态图

每次 S01～S04 结束后，根据真实生成的 samples.csv 和 timeline.csv 自动生成静态图。

优先至少生成：

plots/gnss_quality.png

内容建议：
- satellites
- HDOP
- GNSS state / confidence
- phase 边界

plots/lidar_quality.png

内容建议：
- valid_point_count
- stable_point_count
- temporal_match_ratio
- geometry_score
- phase 边界

plots/fusion_confidence.png

内容建议：
- measurement_confidence
- position uncertainty
- adaptive position uncertainty
- fusion mode变化

plots/state_timeline.png

横轴为时间，至少显示：
- scenario phase
- GNSS state
- LiDAR health/state
- fusion mode
- navigation state
- relocalization state
- cmd_vel source

状态时间线应能让人一眼看出：

什么时候开始退化；
哪个模块先发生状态变化；
融合何时进入退化模式；
导航健康何时发生变化。

四、图片真实性标签

所有静态图必须明确标注：

SYNTHETIC SOFTWARE VALIDATION

并在图注、标题或图片明显位置注明：

NOT REAL ROBOT DATA

不得将这些图命名为：

“实车实验结果”
“比赛性能结果”
“20cm定位实验”
“95%特征重复率实验”
“95%重定位成功率实验”

五、输出目录

每个场景结果目录增加：

plots/

例如：

results/synthetic/S04_concurrent_degradation/<run_id>/

包含：

metadata.json
samples.csv
timeline.csv
result.json
result.md
rosbag/
logs/
plots/
  gnss_quality.png
  lidar_quality.png
  fusion_confidence.png
  state_timeline.png

六、运行体验

如果环境允许 GUI，希望最终支持这样的使用流程：

1. 启动 synthetic scenario；
2. 我可以在 Ubuntu 桌面手动打开 RViz2；
3. 同时打开状态 monitor；
4. 看着 S01～S04 数据实时变化；
5. 场景结束后自动保存 CSV、rosbag 和图片。

不要要求用户手动启动很多终端。

如果合理，可以提供一个：

run_scenario S04 --visualize

或等价方式。

但原来的全自动无 GUI 模式必须继续保留，例如：

run_scenario S04

必须可以在 SSH/headless 环境下完整执行。

七、限制

可视化属于辅助功能。

禁止因为可视化需求：

- 修改 GNSS gate 算法；
- 修改 LiDAR 算法；
- 修改融合算法；
- 修改主动重定位算法；
- 修改 Navigation Health；
- 修改 cmd_vel Arbiter；
- 改变任何 PASS/FAIL 阈值；
- 直接发布伪造的 /dg 输出话题；
- 引入 Gazebo；
- 接入真实机器人；
- 引入 ZED/CUDA/TensorRT。

如果可视化与核心第三轮 A 开发冲突：

优先保证 synthetic validation、rosbag、CSV、timeline 和 PASS/FAIL 正确。

可视化可以降级，但不得破坏主测试框架。

八、最终报告增加

在第三轮 A 最终报告中额外返回：

VISUALIZATION:

RVIZ_CONFIG:
path:
tested:
result:

LIVE_MONITOR:
implemented:
command:
fields:

STATIC_PLOTS:

S01:
S02:
S03:
S04:

HEADLESS_SUPPORTED:
YES/NO

VISUALIZATION_MODIFIED_CORE_ALGORITHM:
NO

可视化功能完成后，仍严格遵守原第三轮 A 停止条件：

只完成 S01～S04。

不得继续 S05～S08。

不得开始 Gazebo。

完成后停止等待外部审核。

你现在直接把这段发过去就行。

这样 Codex 已经在做第三轮 A 的话，它会把可视化作为**附加功能**补进去，不会把整个任务重新推翻。后面你就可以一边看 RViz，一边看终端里 `GNSS GOOD → DEGRADED → REJECTED`、`geometry_score` 下降、Fusion 状态变化，跑完还有 PNG 图可以直接看。memcite
~~~~

[本对话目录](README.md) | [总目录](../../README.md) | [Raw](https://raw.githubusercontent.com/weiyang02520-ops/dg202611-conversation-archive/main/conversations/first/part-025.md) | [上一卷](part-024.md) | [下一卷](part-026.md)
