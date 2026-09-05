# 机器人定位挑战赛分析 / 第 016 卷

[本对话目录](README.md) | [总目录](../../README.md) | [Raw](https://raw.githubusercontent.com/weiyang02520-ops/dg202611-conversation-archive/main/conversations/first/part-016.md) | [上一卷](part-015.md) | [下一卷](part-017.md)

## 消息 1030 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "thoughts",
  "thoughts": [
    {
      "summary": "定位初始化检查",
      "content": "我发现绝对定位更新会同时降低航向不确定性，尽管GNSS并不校正航向；更严重的是，初始地图位姿直接沿用里程计，若与AMCL相差超过门限，绝对测量可能持续被拒绝。",
      "chunks": [],
      "finished": true
    },
    {
      "summary": "完成定位初始化检查",
      "content": "",
      "chunks": [],
      "finished": true
    }
  ],
  "source_analysis_msg_id": "db892a1b-169c-4d5c-a931-92635ae4f9a1"
}
~~~~

## 消息 1041 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "thoughts",
  "thoughts": [
    {
      "summary": "检查重复测量",
      "content": "我还发现周期处理一直重复提交缓存数据：相同时间戳可能被误判为过期或时间倒退，使状态反复显示拒绝。建议区分“重复数据”和真正过期数据，并在没有新里程计时保持稳定状态。",
      "chunks": [],
      "finished": true
    },
    {
      "summary": "检查了重复测量",
      "content": "",
      "chunks": [],
      "finished": true
    }
  ],
  "source_analysis_msg_id": "f32c3181-56b8-459d-85e6-7afe301fd52a"
}
~~~~

## 消息 1042 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "thoughts",
  "thoughts": [
    {
      "summary": "审查提交差异并完善多源融合功能测试",
      "content": "",
      "chunks": [],
      "finished": true
    }
  ],
  "source_analysis_msg_id": "ce9ab598-bdcf-4242-b870-1bd405857a99"
}
~~~~

## 消息 1043 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "reasoning_recap",
  "content": "思考了 1m 20s"
}
~~~~

## 消息 1044 / assistant / 分段 1 of 1

~~~~text
这轮**方向是对的，但还不能收口**。

我已经核了远端：`main` 当前确实是 `c2ea9b1460e1d460f5fb298c5a43e03b4af0ddcc`，父提交就是 `ce67efb...`，提交信息也正确。fileciteturn184file0L1-L2

**Gate：CHANGES_REQUESTED。**

不是架构推倒重来，而是现在这个 Fusion POC 有几个会在真实 ROS 定时运行时暴露出来的问题。

### 1. 最大的问题：缓存的同一帧数据会被当成“旧数据错误”

wrapper 是定时 `_tick()`，每次都把当前缓存的 `_local_odom`、GNSS、AMCL、Scan-to-Map 再传给 core。fileciteturn180file0L1-L2

但是 core 对 odom 是：

`odom.timestamp <= last_timestamp → LOCAL_ODOM_TIME_REVERSED`

对绝对定位也是：

`measurement.timestamp <= last_timestamp → *_OLD_MEASUREMENT`。fileciteturn179file0L1-L2

这意味着真实运行时非常容易出现：

`10Hz fusion timer`
→ 这一 tick 收到 odom
→ 下一 tick 还没来新 odom
→ 又传同一个 odom
→ 被判 TIME_REVERSED

GNSS 如果 1Hz：

`GNSS成功融合一次`
→ 后面 9 个 fusion tick 还拿同一个 fix
→ 全被判 `GNSS_OLD_MEASUREMENT`
→ fusion mode 甚至可能变 `REJECTED_UPDATE`

这不是异常，是**正常的消息频率差异**。

所以这必须修。

正确语义应该是：

- timestamp `< last_timestamp`：真正时间倒退，REJECT
- timestamp `== last_timestamp`：DUPLICATE / NO_NEW_DATA，静默忽略，不是异常
- timestamp `> last_timestamp`：新数据，正常处理

wrapper 最好再加 sequence/consumed 标记，只把**新到达的绝对测量**交给 core。

---

### 2. GNSS 明明不修 yaw，却会降低 yaw uncertainty

你现在 GNSS 生成的 measurement yaw 是当前 `_map_pose.yaw`，所以位置更新本身确实不会改变航向。fileciteturn179file0L1-L2

但 `_apply_absolute_update()` 无论 GNSS 还是 LiDAR，都会同时执行：

- position uncertainty × factor
- yaw uncertainty × factor

也就是说：

**GNSS虽然没提供航向信息，却会让系统越来越“相信自己的yaw”。**

这在物理意义上不对。

应该改成：

- GNSS：只降低 position uncertainty
- AMCL / Scan-to-Map：可降低 position + yaw uncertainty
- wheel/IMU dead reckoning：继续传播 yaw uncertainty

这个必须有测试。

---

### 3. 第一次 map-frame 绝对定位很可能永远进不来

目前启动后第一帧 local odom 会直接：

`self._map_pose = odom`

也就是暂时把 map 和 odom 当成重合。fileciteturn179file0L1-L2

之后第一次 AMCL / Scan-to-Map / GNSS 绝对位置仍然要经过 residual gate，比如 LiDAR 默认 5 m、GNSS 默认 8 m。fileciteturn179file0L1-L2

但现实里：

`map` 原点和 `odom` 原点完全可能差十几米甚至几十米。

例如：

- odom `(0,0)`
- AMCL 在 map 里 `(20,15)`

这本来是合法的 `map→odom` 初始化。

现在却会被当成 `AMCL_OUTLIER`，之后永远无法建立正确的全局关系。

所以要区分：

**首次全局锚定** 和 **已经锚定后的异常值修正**。

第一次可信 map measurement 应该允许初始化 `map→odom`，之后才执行 residual gate / bounded correction。

---

### 4. competition launch 默认开启 Fusion，但实际上 GNSS 默认根本不能融合

launch 默认：

`enable_multisource_fusion=true`

但没有给 fusion node 提供：

- `gnss_origin_latitude`
- `gnss_origin_longitude`
- `map_enu_yaw`
- map ENU offset

而 wrapper 的这些参数默认是 NaN，因此 core 默认没有 GNSS reference/alignment。fileciteturn180file0L1-L2

core 确实正确地拒绝未对齐 GNSS，并进入 `WAITING_ALIGNMENT`。fileciteturn179file0L1-L2

所以算法能力有了，但**competition launch 还没有实际配置入口**。

至少应该把这些做成 launch arguments：

`gnss_origin_latitude`
`gnss_origin_longitude`
`gnss_origin_altitude`
`map_enu_yaw`
`map_enu_offset_x`
`map_enu_offset_y`

没配置时明确：

`GNSS fusion unavailable / WAITING_ALIGNMENT`

不能让用户以为开了 fusion 就自动有 GNSS。

---

所以这一轮我认可：

**Multi-source Fusion architecture = YES**
**真正可运行的 Fusion POC = 还差最后一次 hardening**

直接给 Codex：

仓库：
weiyang02520-ops/electric-power-inspection-robot

Starting HEAD：
c2ea9b1460e1d460f5fb298c5a43e03b4af0ddcc

本轮只修 DG Multi-source Fusion POC 的运行时语义。

不要扩功能，不加视觉，不改默认 EKF，不改已收口 Navigation Integration 架构。

一、修 duplicate measurement 语义

当前 periodic wrapper 会重复把缓存消息传给 core。

要求：

timestamp < last_timestamp：
REJECT，reason `*_TIME_REVERSED` / `*_OLD_MEASUREMENT`

timestamp == last_timestamp：
视为 DUPLICATE / NO_NEW_DATA。

不能：
- 进入 REJECTED_UPDATE
- 增加 uncertainty
- 降低 uncertainty
- 重复修正位置
- 产生错误告警

timestamp > last_timestamp：
才是真正的新 measurement。

Local odom 同样处理。

建议 wrapper 同时维护：

odom sequence
gnss sequence
amcl sequence
scan_match sequence

以及 consumed sequence。

只有新绝对测量才传给 core。

但 core 本身也必须具备 duplicate 防护，不能完全依赖 wrapper。

增加测试：

1. same odom timestamp twice → no TIME_REVERSED
2. older odom timestamp → TIME_REVERSED
3. same GNSS measurement 10 ticks → only first updates
4. duplicate GNSS does not enter REJECTED_UPDATE
5. duplicate AMCL does not enter REJECTED_UPDATE
6. duplicate Scan-to-Map does not enter REJECTED_UPDATE
7. 10Hz fusion + 1Hz GNSS synthetic loop remains stable

二、GNSS不得降低yaw uncertainty

把 absolute update 分离：

GNSS:
correct_position = true
correct_yaw = false

AMCL / Scan-to-Map:
correct_position = true
correct_yaw = true

GNSS update后：

map_pose.yaw unchanged
yaw_uncertainty unchanged

position_uncertainty 可以降低。

增加测试：

GNSS correction reduces position uncertainty
GNSS correction does NOT reduce yaw uncertainty
GNSS correction does NOT change yaw
LiDAR correction reduces yaw uncertainty

三、实现首次global anchor

增加明确状态：

GLOBAL_UNANCHORED
或等价内部标志。

local odom到来：
仍然可以连续传播局部运动。

但是第一次可信绝对 map measurement：

优先级：
accepted fresh Scan-to-Map
否则 healthy AMCL
否则 aligned accepted GNSS position

必须允许建立初始 `map→odom`。

首次 anchor 不受普通 residual_gate 限制。

但仍要求：

finite
fresh
accepted
合法质量状态

完成首次 anchor 后：

后续所有 absolute update 才执行：
residual gate
bounded correction
timestamp guard

GNSS首次anchor只提供x/y：
yaw继续使用local/current yaw。

AMCL或Scan-to-Map首次anchor：
可以建立x/y/yaw。

增加测试：

odom=(0,0)
AMCL map=(20,15)
→ first global anchor succeeds

anchor完成后：
第二个跳到(100,100)
→ outlier rejected

GNSS alignment offset远离odom origin：
首次GNSS anchor允许建立global translation。

四、明确 GNSS alignment runtime configuration

在：

dg_navigation_integration.launch.py

新增launch参数：

gnss_origin_latitude
gnss_origin_longitude
gnss_origin_altitude
map_enu_yaw
map_enu_offset_x
map_enu_offset_y

传给 multisource_fusion_node。

默认可以保持 NaN/未配置。

但 status 必须明确：

alignment_ready=false
GNSS fusion disabled until alignment configured

不要自动伪造origin。

不要默认把第一帧GNSS经纬度当map原点，除非以后专门设计校准流程。

五、状态语义

没有新absolute measurement只是正常运行。

不要因为：
GNSS本周期没新帧
AMCL本周期没新帧
Scan-to-Map本周期没新帧

就进入 REJECTED_UPDATE。

DEAD_RECKONING 应表示：

在一段时间内没有可用absolute correction，
但local odom仍连续传播。

REJECTED_UPDATE 只用于：
真正收到一个新的、但被异常检查拒绝的 measurement。

六、uncertainty时间传播

当前 uncertainty 主要按运动距离增加。

保留即可，但检查机器人静止、absolute sources全部丢失时。

建议增加最小 time-based drift：

odom_position_noise_per_s
odom_yaw_noise_per_s

可以默认很小。

这样：
GNSS+LiDAR都中断，即使机器人暂时静止，
uncertainty也不会永远完全不增长。

如果本轮实现会明显复杂化，可以先不做；
但必须在代码注释和status里明确当前 uncertainty 是 POC heuristic，不是统计意义真实 covariance。

禁止包装成正式EKF covariance。

七、测试

保留原：

Fusion Core 21
Geodetic/ENU 3
Alignment 2
Concurrent Degradation 5
Existing regressions 170

新增上述 duplicate / initial anchor / GNSS yaw uncertainty tests。

重点增加真实频率simulation：

local odom 20Hz
fusion tick 10Hz
GNSS 1Hz
AMCL 5Hz

运行至少模拟10秒。

验证：

no false TIME_REVERSED
no false REJECTED_UPDATE
no NaN/inf
GNSS only updates on new sample
AMCL only updates on new sample
continuous output

八、真实性

完成后仍然：

Real Robot Tested = NO
20 cm Proven = NO
Concurrent Degradation Metric Proven = NO
Original EKF Modified = NO
Publishes map→odom TF = NO

允许：

DG Multi-source Fusion Closed-loop POC = COMPLETE_POC
Capability = PARTIAL

九、提交

git diff --check

commit：

fix(fusion): harden multisource runtime and anchoring semantics

push main。

最后报告：

Starting HEAD
Final HEAD

Runtime Semantics:
duplicate odom ignored: YES/NO
duplicate GNSS ignored: YES/NO
duplicate AMCL ignored: YES/NO
duplicate Scan-to-Map ignored: YES/NO
older measurement rejected: YES/NO
false REJECTED_UPDATE removed: YES/NO

Global Anchor:
first AMCL anchor: YES/NO
first Scan-to-Map anchor: YES/NO
first aligned GNSS anchor: YES/NO
post-anchor residual gate: YES/NO

Uncertainty:
GNSS reduces position uncertainty: YES/NO
GNSS changes yaw uncertainty: NO
LiDAR can reduce yaw uncertainty: YES/NO

Alignment Runtime:
launch alignment args: YES/NO
unconfigured alignment explicit: YES/NO

Frequency Simulation:
10s mixed-rate simulation: PASS/FAIL
false time reversal: 0 expected
false rejected update: 0 expected

Tests:
Fusion Core: X/X
Geodetic/ENU: X/X
Alignment: X/X
Concurrent Degradation: X/X
Mixed Frequency: X/X
Existing regressions: X/X

ROS2 Build:
PASS/FAIL/NOT_RUN + reason

Original EKF Modified:
NO

Publishes map→odom TF:
NO

Real Robot Tested:
NO

20 cm Proven:
NO

Capability:
PARTIAL

Remote Verified:
YES/NO

然后 STOP。

**这一轮过了，我才会把“GNSS真正参与多源定位 POC”正式收口。** 现在已经不是缺大架构了，就是把运行时这些坑堵掉。memcite
~~~~

## 消息 1045 / user / 分段 1 of 1

~~~~text
Starting HEAD: `c2ea9b1460e1d460f5fb298c5a43e03b4af0ddcc`
Final HEAD: `3922a3296575f58b2fe25a8121d4fb180727c3b3`

Runtime Semantics:

- Duplicate odom ignored: YES
- Duplicate GNSS ignored: YES
- Duplicate AMCL ignored: YES
- Duplicate Scan-to-Map ignored: YES
- Older measurement rejected: YES
- False `REJECTED_UPDATE` removed: YES

Global Anchor:

- First AMCL anchor: YES
- First Scan-to-Map anchor: YES
- First aligned GNSS anchor: YES
- Post-anchor residual gate: YES

Uncertainty:

- GNSS reduces position uncertainty: YES
- GNSS changes yaw uncertainty: NO
- LiDAR can reduce yaw uncertainty: YES

Alignment Runtime:

- Launch alignment args: YES
- Unconfigured alignment explicit: YES

Frequency Simulation:

- 10s mixed-rate simulation: PASS
- False time reversal: 0
- False rejected update: 0

Tests:

- Fusion Core: 32/0
- Geodetic/ENU: 3/0
- Alignment: 2/0
- Concurrent Degradation: 5/0
- Mixed Frequency: 1/0
- Existing regressions: 170/0

ROS2 Build: `NOT_RUN` — 缺少 `rclpy`、`ament_index_python`、`colcon`
Original EKF Modified: NO
Publishes `map→odom` TF: NO
Real Robot Tested: NO
20 cm Proven: NO
Capability: PARTIAL
Remote Verified: YES
~~~~

## 消息 1060 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "thoughts",
  "thoughts": [
    {
      "summary": "检查同步问题",
      "content": "我正在审查 GNSS 修复与质量消息的时序。质量和位置分别递增序列，跨话题到达顺序并不保证；因此，新位置可能暂时沿用旧质量。虽然已接受位置本身代表通过门控，但状态滞后仍可能造成融合风险，需继续检查扫描匹配流程。",
      "chunks": [],
      "finished": true
    },
    {
      "summary": "检查了同步问题",
      "content": "",
      "chunks": [],
      "finished": true
    }
  ],
  "source_analysis_msg_id": "b728e1d5-668a-4ca6-a0af-a0fff2650f51"
}
~~~~

## 消息 1065 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "thoughts",
  "thoughts": [
    {
      "summary": "完善消息配对",
      "content": "我发现发布顺序确实可能让融合线程先收到新位姿、后收到对应质量，从而短暂使用旧状态。扫描匹配和 GNSS 都应按共同时间戳配对，或直接使用包含完整候选信息的质量消息，避免错误加权。",
      "chunks": [],
      "finished": true
    },
    {
      "summary": "完善了消息配对",
      "content": "",
      "chunks": [],
      "finished": true
    }
  ],
  "source_analysis_msg_id": "9814d23f-571c-4179-8de8-f6c36ae0b336"
}
~~~~

## 消息 1072 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "thoughts",
  "thoughts": [
    {
      "summary": "审查多源融合变更并验证测试实现状态",
      "content": "",
      "chunks": [],
      "finished": true
    }
  ],
  "source_analysis_msg_id": "9f3f4dbf-f6a9-404a-9343-f66da79e643b"
}
~~~~

## 消息 1077 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "thoughts",
  "thoughts": [
    {
      "summary": "验证了 YAML 数值及特殊浮点值解析",
      "content": "",
      "chunks": [],
      "finished": true
    }
  ],
  "source_analysis_msg_id": "da773f89-7d6f-48a3-ba93-e033c3889259"
}
~~~~

## 消息 1082 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "thoughts",
  "thoughts": [
    {
      "summary": "检查导航融合参数类型并审查协议测试",
      "content": "",
      "chunks": [],
      "finished": true
    }
  ],
  "source_analysis_msg_id": "f6746ac6-f4c5-40f5-8d7a-8696e2e3e6ba"
}
~~~~

## 消息 1083 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "reasoning_recap",
  "content": "思考了 3m 11s"
}
~~~~

[本对话目录](README.md) | [总目录](../../README.md) | [Raw](https://raw.githubusercontent.com/weiyang02520-ops/dg202611-conversation-archive/main/conversations/first/part-016.md) | [上一卷](part-015.md) | [下一卷](part-017.md)
