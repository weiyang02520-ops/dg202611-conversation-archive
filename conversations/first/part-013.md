# 机器人定位挑战赛分析 / 第 013 卷

[本对话目录](README.md) | [总目录](../../README.md) | [Raw](https://raw.githubusercontent.com/weiyang02520-ops/dg202611-conversation-archive/main/conversations/first/part-013.md) | [上一卷](part-012.md) | [下一卷](part-014.md)

## 消息 889 / assistant / 分段 1 of 1

~~~~text
这轮已经很接近了，我核了远端：`main` 现在确实是 `b134f9da5d94adeba161b86f62ee291d2d9d9377`，父提交就是 `24ae211...`，提交信息也对。fileciteturn134file0L1-L2

但按照我们现在的原则——**这一块既然做了，就把它收干净再进统一集成**——我这里给：

**Gate：CHANGES_REQUESTED（小修，不是推倒重来）**

主要只剩 3 个问题。

第一，**部分 Scan-to-Map 失败反馈实际上会被 supervisor 丢掉。**

比如 `NO_MAP / NO_SCAN / TF_FAILED` 时，Scan-to-Map 发布的是：

`mean_distance = inf`。fileciteturn133file0L1-L2

但 supervisor 的 `_float_value()` 明确只接受有限浮点数，`inf` 会变成 `None`。fileciteturn135file0L1-L2

后面 `_on_match_quality()` 又要求：

`score / inlier / mean_distance / x / y / yaw`

全部非 None，否则直接 `return`。fileciteturn130file0L1-L2

结果就是：

`NO_MAP`
→ Scan-to-Map 明明发了失败
→ supervisor 没收到有效 candidate
→ 还是只能等 timeout。

所以“failed match feedback = YES”目前还没有完全成立。

第二，**optional freshness 的语义还有点问题。**

现在 optional LiDAR/GNSS/Scan-to-Map stale 虽然不会直接 `triggerable=True`，但仍然会被加入 `degraded_reasons`。这样整体 `assessment.degraded=True`。fileciteturn127file0L7-L7

而状态机里 `degraded` 会阻止 `SUSPECTED → NORMAL` 的健康恢复；同时 `last_trusted_pose` 也只在 `NORMAL && not degraded` 时更新。fileciteturn138file0L1-L2

所以可能出现：

`Scan-to-Map以前成功过`
→ 一会儿旧结果 stale
→ 它本来是 optional
→ 却长期让整体处于 degraded
→ 影响 last_trusted_pose 更新和 SUSPECTED 恢复。

这个不太合理。

第三，core 里虽然有 `required_signals`，但是 **ROS wrapper 现在没暴露这个参数**。

ROS 参数列表和 `_read_config()` 里目前都没有 `required_signals`。fileciteturn141file0L1-L2

所以代码层支持 required/optional，运行时实际上还不能配置。

这三个都是**收尾问题**。自动 seed、专用 `/dg/relocalization/seed`、last trusted pose、candidate freshness、成功 candidate 验证这些主体已经接起来了。fileciteturn129file0L1-L2

直接给 Codex 跑下面这份。

仓库：

weiyang02520-ops/electric-power-inspection-robot

Starting HEAD：

b134f9da5d94adeba161b86f62ee291d2d9d9377

本轮不要增加新功能。

目标：

关闭 Active Relocalization 自动闭环最后的健壮性缺口。

只处理以下三类问题。

一、修复 failed match feedback 被丢弃

当前问题：

scan_map_relocalization_node 在：

NO_MAP
NO_SCAN
TF_FAILED
NO_VALID_POINTS

等失败路径中可能发布：

mean_distance = inf

而 active_relocalization_node 的 `_float_value()` 会拒绝非有限数值，导致整个 failed candidate 被丢弃。

要求：

每一次自动 `/dg/relocalization/seed` 请求，不论成功失败，都必须让 supervisor 收到机器可读结果。

优先同时做两层保护：

1. Scan-to-Map 失败输出使用有限值。

例如：

score = 0.0
inlier_ratio = 0.0
mean_distance = 一个有限的大值

不要发布 NaN/inf 作为跨节点协议字段。

2. active_relocalization_node 解析 match_quality 时：

先解析：

accepted
reason

如果 accepted=false：

不要因为某个质量数值缺失或异常就直接丢弃整条失败消息。

必须生成 rejected CandidateQuality / failed result 交给 core。

失败 candidate 不需要满足正常 candidate 的质量要求。

accepted=true 时仍然执行严格有限值校验。

要求覆盖：

NO_MAP
NO_SCAN
TF_FAILED
NO_VALID_POINTS
SCORE_LOW
INLIER_LOW
MEAN_DISTANCE_HIGH

收到后 supervisor 必须立刻 retry / next segment，而不是等 timeout。

二、修正 optional stale 语义

当前原则：

required signal stale：
允许触发或作为强异常。

optional signal stale：
只表示 UNKNOWN/STALE。

不能仅仅因为一个 optional signal 很久没有更新，就把整个 LocalizationHealth 永久标记 degraded。

因此：

LiDAR optional stale：
lidar_health = STALE
但不能单独设置 overall degraded。

GNSS optional stale：
gnss_health = STALE
但不能单独设置 overall degraded。

Scan-to-Map optional stale：
scan_match_health = STALE
但不能单独设置 overall degraded。

注意区别：

当前有效的：

GNSS REJECTED
LiDAR quality low
Scan-to-Map 当前低质量结果

仍然可以作为 degraded / trigger evidence。

这里只修：

“旧数据过期”

的语义。

必须验证：

曾经进入 SUSPECTED，
随后 AMCL恢复正常，
只有 optional GNSS/LiDAR/Scan-to-Map stale，

仍然可以经过 healthy_recovery_samples 回到 NORMAL。

同时 NORMAL 状态下：

optional stale 不能阻止 last_trusted_pose 正常更新。

三、ROS wrapper 暴露 required_signals

新增参数：

required_signals

默认：

[]

ROS2 string array。

允许例如：

["lidar"]
["gnss"]
["scan_match"]
["amcl"]

或者当前 core 已支持的标准名称。

读取后传给：

ActiveRelocalizationConfig(required_signals=...)

默认空数组时：

LiDAR
GNSS
Scan-to-Map

均保持 optional。

不要改变默认行为。

四、增加测试

保留现有39个 Active Relocalization测试。

新增至少：

1. failed match accepted=false + mean_distance=inf 不被静默丢弃
2. NO_MAP反馈立即进入失败处理
3. NO_SCAN反馈立即进入失败处理
4. TF_FAILED反馈立即进入失败处理
5. SCORE_LOW反馈立即进入下一segment
6. accepted=true + 非有限score必须拒绝
7. optional LiDAR stale 不造成 overall degraded
8. optional GNSS stale 不造成 overall degraded
9. optional Scan-to-Map stale 不造成 overall degraded
10. optional stale 时 SUSPECTED 可以恢复 NORMAL
11. optional stale 时 NORMAL 继续更新 last_trusted_pose
12. required LiDAR stale 仍可 trigger
13. required GNSS stale 仍可 trigger
14. required Scan-to-Map stale 仍可 trigger
15. required_signals 参数正确进入 config

尽量把 match_quality 解析抽成 ROS-independent helper，这样当前没有 rclpy 的环境也可以测试。

不要为了测试安装 ROS2。

五、回归

必须重新运行：

Active Relocalization
LiDAR
GNSS
其它当前可运行纯 Python tests

Scan-to-Map ROS测试如果仍缺 rclpy：

NOT_RUN + reason

六、本轮禁止

不要做：

GNSS正式融合
Nav2接管
短报文
方案书
UI
3D LiDAR
真实指标
新的导航架构

这一轮只是 Active Relocalization closure hardening。

七、提交

git diff --check

commit：

fix(relocalization): harden recovery failure and freshness semantics

push：

electric-power-inspection-robot/main

最终只输出：

Starting HEAD
Final HEAD

Failure Feedback:
NO_MAP immediate: YES/NO
NO_SCAN immediate: YES/NO
TF_FAILED immediate: YES/NO
NO_VALID_POINTS immediate: YES/NO
quality rejection immediate: YES/NO
non-finite accepted candidate rejected: YES/NO

Freshness Semantics:
optional lidar stale non-degrading: YES/NO
optional gnss stale non-degrading: YES/NO
optional scan-map stale non-degrading: YES/NO
required stale triggerable: YES/NO
SUSPECTED recovery with optional stale: YES/NO
last trusted pose updates with optional stale: YES/NO

Runtime Configuration:
required_signals ROS parameter: YES/NO

Tests:
Active Relocalization: X passed / X failed
LiDAR Regression: X passed / X failed
GNSS Regression: X passed / X failed
Other Runnable: X passed / X failed

ROS2 Build:
PASS/FAIL/NOT_RUN + reason

Automatic Recovery Loop:
COMPLETE_POC / INCOMPLETE

Real Robot Tested:
NO

Competition >95% Proven:
NO

Capability Status:
PARTIAL

Remote Verified:
YES/NO

然后 STOP。

**这轮如果过，我认为“主动重定位这一部分”在当前无 ROS2 实机环境下就可以正式收口了。**

下一轮就不再抠这一块，而是按我们定好的方向，开始真正做：

**LiDAR + GNSS + AMCL + Scan-to-Map + Active Relocalization 的统一导航定位集成闭环。** memcite
~~~~

## 消息 890 / user / 分段 1 of 1

~~~~text
Starting HEAD: `b134f9da5d94adeba161b86f62ee291d2d9d9377`
Final HEAD: `67e8358eb3789e16d2959a8531f403579c76fe84`

Failure Feedback:

- NO\_MAP immediate: YES
- NO\_SCAN immediate: YES
- TF\_FAILED immediate: YES
- NO\_VALID\_POINTS immediate: YES
- quality rejection immediate: YES
- non-finite accepted candidate rejected: YES

Freshness Semantics:

- optional lidar stale non-degrading: YES
- optional gnss stale non-degrading: YES
- optional scan-map stale non-degrading: YES
- required stale triggerable: YES
- SUSPECTED recovery with optional stale: YES
- last trusted pose updates with optional stale: YES

Runtime Configuration:

- required\_signals ROS parameter: YES

Tests:

- Active Relocalization: 50 passed / 0 failed
- LiDAR Regression: 11 passed / 0 failed
- GNSS Regression: 13 passed / 0 failed
- Other Runnable: 23 passed / 0 failed

ROS2 Build: NOT\_RUN — ROS2/colcon unavailable

Automatic Recovery Loop: COMPLETE\_POC
Real Robot Tested: NO
Competition >95% Proven: NO
Capability Status: PARTIAL
Remote Verified: YES
~~~~

## 消息 892 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "model_editable_context",
  "model_set_context": ""
}
~~~~

## 消息 907 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "thoughts",
  "thoughts": [
    {
      "summary": "推进统一集成",
      "content": "我会先核对现有节点之间的接口与控制权，重点检查定位来源、重定位状态、速度指令以及降级逻辑是否闭环，再设计统一导航监督与融合入口，避免多个节点重复发布或互相抢占。",
      "chunks": [],
      "finished": true
    },
    {
      "summary": "推进统一集成",
      "content": "",
      "chunks": [],
      "finished": true
    }
  ],
  "source_analysis_msg_id": "797633ba-7320-47df-945f-1c4db2cefd1b"
}
~~~~

## 消息 908 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "thoughts",
  "thoughts": [
    {
      "summary": "审查提交差异并检查主分支及候选解析",
      "content": "",
      "chunks": [],
      "finished": true
    }
  ],
  "source_analysis_msg_id": "a10bf888-d08b-4fcb-ae65-d8727ef99da6"
}
~~~~

## 消息 909 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "reasoning_recap",
  "content": "思考了 37s"
}
~~~~

[本对话目录](README.md) | [总目录](../../README.md) | [Raw](https://raw.githubusercontent.com/weiyang02520-ops/dg202611-conversation-archive/main/conversations/first/part-013.md) | [上一卷](part-012.md) | [下一卷](part-014.md)
