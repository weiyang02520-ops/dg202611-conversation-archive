# 机器人定位挑战赛分析 / 第 014 卷

[本对话目录](README.md) | [总目录](../../README.md) | [Raw](https://raw.githubusercontent.com/weiyang02520-ops/dg202611-conversation-archive/main/conversations/first/part-014.md) | [上一卷](part-013.md) | [下一卷](part-015.md)

## 消息 910 / assistant / 分段 1 of 1

~~~~text
这轮可以正式收口。

**Gate：PASS_WITH_NOTES。**

我核了远端，`main` 当前确实是 `67e8358eb3789e16d2959a8531f403579c76fe84`，父提交是 `b134f9da...`，提交信息也正确。fileciteturn144file0L1-L2

这次几个关键问题都真修了：

- optional 的 LiDAR / GNSS / Scan-to-Map stale 不再把整体健康状态强行变成 degraded。fileciteturn142file0L7-L7
- `required_signals` 已经暴露成 ROS 参数并进入 core 配置。fileciteturn142file0L7-L7
- Scan-to-Map 失败跨节点协议不再发送 `inf`，而是有限哨兵值。fileciteturn142file0L7-L7
- supervisor 现在即使收到 rejected candidate 的异常/缺失质量字段，也不会直接把失败消息扔掉。fileciteturn142file0L7-L7
- 测试也覆盖了 `NO_MAP / NO_SCAN / TF_FAILED / NO_VALID_POINTS` 的立即失败处理，以及非有限 accepted candidate 的拒绝。fileciteturn145file0L1-L2

所以现在可以认为：

**Local Active Relocalization Closed-loop POC = COMPLETE_POC / Capability PARTIAL**

这里 `PARTIAL` 只是因为还缺 ROS2 实机、真实 rosbag 和 >95% 实测，不再是代码闭环没做完。

有一个很小的 note：如果未来某个外部节点发送 `accepted=true + 非有限数值 + reason=ACCEPTED` 这种自相矛盾的数据，解析器虽然会把它改成 rejected，但 reason 可能仍保留外部的 `ACCEPTED`。当前自己的 Scan-to-Map 发布端已经会先把数据有限化，所以**不阻塞这一轮**，以后协议统一时顺手收掉即可。

现在按照我们定好的原则，**不换方向**。

下一步开始把目前已经做出来的：

`LiDAR鲁棒处理`
+
`GNSS质量门`
+
`AMCL`
+
`Scan-to-Map`
+
`主动重定位`

真正整理成**统一导航定位运行链**。

先做“集成层”，暂时不急着把 GNSS 硬塞进现有 EKF。因为现在最大的实际缺口是这些 POC 虽然都有了，但还是一个个散开的旁路节点。

给 Codex：

仓库：

weiyang02520-ops/electric-power-inspection-robot

Starting HEAD：

67e8358eb3789e16d2959a8531f403579c76fe84

本轮继续 DG-202611 导航定位主线。

不要切换到：

短报文
方案书
UI
3D LiDAR
比赛材料

当前已有能力已经包括：

1. 原有轮速 + IMU EKF
2. AMCL / Nav2
3. 2D LiDAR robust POC
4. GNSS quality gate POC
5. Scan-to-Map local relocalization
6. Active Relocalization Closed-loop POC

现在的问题不是继续堆独立 POC。

本轮目标：

把这些节点整理成一个可启动、可观察、可安全关闭、协议一致的：

DG-202611 Navigation Integration POC

注意：

本轮先做集成运行层。

不要直接修改现有默认机器人 bringup。

不要把未经验证的 GNSS 直接塞进现有 EKF。

==================================================
一、建立独立 competition navigation launch
==================================================

新增独立 launch。

建议：

src/ylhb_base/launch/dg_navigation_integration.launch.py

或者根据当前仓库结构使用等价位置。

这个 launch 默认不被原机器人 bringup include。

只有手动启动时才运行比赛 POC。

至少能组合：

原 `/scan`
→ lidar_robust_node

原 `/gps/fix + /gps/rtk_status`
→ gnss_quality_node

AMCL / odom / scan
+
LiDAR quality
+
GNSS quality
+
Scan-to-Map
→ active_relocalization_node

以及：

scan_map_relocalization_node

要求：

所有 topic 参数清晰。

不要硬编码隐藏依赖。

==================================================
二、统一 Navigation Health 输出
==================================================

现在各节点分别输出自己的 diagnostic。

新增一个 ROS-independent：

navigation_health_core.py

以及：

navigation_health_node.py

作用不是重新实现 active relocalization。

而是提供整个导航系统统一健康状态。

输入至少：

GNSS quality
LiDAR quality
AMCL covariance / freshness
odom freshness
scan freshness
Scan-to-Map quality
Active Relocalization state

输出：

/dg/navigation/status

推荐：

diagnostic_msgs/DiagnosticArray

统一状态至少：

NOMINAL
DEGRADED
LOCALIZATION_SUSPECT
RECOVERING
RECOVERED
FAILED
MANUAL_REQUIRED

并输出：

gnss_state
lidar_state
amcl_state
scan_match_state
relocalization_state
overall_state
reasons
timestamp

==================================================
三、不要重复做主动重定位判断
==================================================

navigation health 的职责：

汇总
观测
输出统一状态

Active Relocalization 仍然负责：

触发
主动动作
candidate
验证
retry

不要出现两个状态机同时决定是否旋转机器人。

==================================================
四、统一 freshness 规则
==================================================

建立统一输入适配层。

每一个 side-channel 都明确：

received_at
fresh/stale
current state

不能让：

GNSS node用一套 freshness
Navigation health又误解一套含义。

允许各节点内部阈值不同，但接口语义必须统一。

建议统一约定：

GOOD
DEGRADED
REJECTED
STALE
UNKNOWN

或对不同模块进行明确映射。

写成代码中的 mapping，不要靠字符串猜测散落在多个函数里。

==================================================
五、解决 `/cmd_vel` 所有权
==================================================

这是本轮重要安全项。

当前 active_relocalization_node 直接发布 `/cmd_vel`。

未来 Nav2 运行时也可能发布 `/cmd_vel`。

禁止把 competition launch 做成两个节点直接抢 `/cmd_vel`。

本轮实现一个最小安全 arbitration POC。

建议：

正常导航命令：
`/cmd_vel_nav`

主动重定位命令：
`/cmd_vel_recovery`

输出：
`/cmd_vel`

新增纯核心：

cmd_vel_arbiter_core.py

ROS wrapper：

cmd_vel_arbiter_node.py

原则：

NORMAL / NOMINAL：
允许 navigation command

RECOVERING：
只允许 recovery command

FAILED / MANUAL_REQUIRED：
输出零速度

manual takeover：
自动 recovery 不得占控制权

source stale：
输出零速度

source switch：
至少一个短暂 zero/guard window，避免切换瞬间速度跳变

注意：

不要修改真实 Nav2 默认 remap。

competition launch 中做 remap 即可。

原默认 bringup保持不变。

==================================================
六、修改 Active Relocalization 输出 topic
==================================================

不要让 competition integration 模式下 active relocalization 直接发 `/cmd_vel`。

通过 launch 参数设置：

active_relocalization_node
→ `/cmd_vel_recovery`

Arbiter：

`/cmd_vel_recovery`
→ 最终 `/cmd_vel`

但 active_relocalization_node 原来的默认参数可以保持 `/cmd_vel`，保证向后兼容。

==================================================
七、Nav2输入边界
==================================================

本轮如果无法安全完整启动 Nav2：

不要假装。

competition launch 可以支持参数：

enable_nav2:=false

默认建议：

false

或者如果当前 bringup结构已经非常清楚，可通过 include existing Nav2 launch。

但不要复制一套 Nav2配置。

不要破坏现有导航。

==================================================
八、统一事件日志
==================================================

navigation health 需要能看到一次完整状态变化：

NOMINAL
→ DEGRADED
→ LOCALIZATION_SUSPECT
→ RECOVERING
→ RECOVERED

或者：

→ FAILED
→ MANUAL_REQUIRED

状态变化只在 transition 时记录 INFO/WARN/ERROR。

不要每10Hz刷屏。

==================================================
九、Synthetic integration harness
==================================================

因为当前没 ROS2 环境，本轮必须增加纯 Python integration simulation。

建议：

test_navigation_integration_core.py

模拟：

LiDAR quality
GNSS quality
AMCL
scan-map
relocalization
cmd arbiter

至少覆盖：

1. 全健康 → NOMINAL
2. GNSS DEGRADED单独 → DEGRADED但继续导航
3. GNSS REJECTED单独不能等于 AMCL lost
4. LiDAR轻度退化 → DEGRADED
5. AMCL连续异常 → LOCALIZATION_SUSPECT
6. relocalization TRIGGERED/ACTIVE_SCAN → RECOVERING
7. RECOVERING期间 nav cmd被禁止
8. RECOVERING只放行 recovery cmd
9. RECOVERED → 恢复导航控制
10. FAILED → zero
11. MANUAL_REQUIRED → zero
12. stale navigation cmd → zero
13. stale recovery cmd → zero
14. source切换包含zero guard
15. manual takeover禁止自动恢复命令
16. optional GNSS不存在仍可工作
17. optional LiDAR quality不存在仍可工作
18. stale side-channel不使用旧GOOD
19. 多次状态更新不重复刷 transition
20. 完整：
NOMINAL
→ SUSPECT
→ RECOVERING
→ candidate
→ RECOVERED
流程

==================================================
十、协议一致性测试
==================================================

增加静态/纯Python检查：

LiDAR quality实际字段
GNSS quality实际字段
Scan-to-Map match quality字段
Active Relocalization status字段
Navigation Health parser

确保字段名和当前真实 publisher 一致。

不要写一个理论接口但真实节点根本不发布。

==================================================
十一、Launch静态测试
==================================================

即使没有 ROS2：

至少检查：

launch文件存在
脚本被 CMake install
topic名称一致
competition launch不被default bringup包含
active relocalization在integration launch中remap到recovery cmd
arbiter是唯一最终 `/cmd_vel` publisher（本competition launch设计范围内）

如果可以通过纯文本/static Python测试完成，就做。

==================================================
十二、当前不要做 GNSS 正式融合
==================================================

再次强调：

本轮 GNSS仍然是：

quality / degradation signal

还不是位置融合源。

不要改：

ekf.yaml
map->odom
NavSatTransform
现有AMCL定位链

GNSS真正进入多源定位放下一阶段专门做。

==================================================
十三、回归
==================================================

必须重新运行：

Active Relocalization 50 tests
LiDAR 11 tests
GNSS 13 tests
新 Navigation Health tests
新 Cmd Arbiter tests
Integration tests
其它当前能运行的纯Python tests

ROS2无法运行：

NOT_RUN + reason

==================================================
十四、真实性
==================================================

完成后可以写：

DG Navigation Integration POC = PARTIAL

可以说：

已建立统一运行/健康/恢复/控制仲裁框架。

不能说：

BDS+LiDAR融合定位完成
20cm完成
>95%完成
实机已联调
GNSS已进入EKF
全局重定位完成

==================================================
十五、提交
==================================================

git diff --check

commit：

feat(navigation): integrate DG-202611 localization recovery stack

push：

electric-power-inspection-robot/main

最后输出：

Starting HEAD
Final HEAD

Integration:
competition launch: YES/NO
LiDAR quality wired: YES/NO
GNSS quality wired: YES/NO
AMCL health wired: YES/NO
Scan-to-Map wired: YES/NO
Active Relocalization wired: YES/NO
Unified navigation status: YES/NO

Command Arbitration:
nav source: YES/NO
recovery source: YES/NO
single final cmd_vel output: YES/NO
stale source zero: YES/NO
switch guard: YES/NO
manual safety: YES/NO

Tests:
Navigation Health: X passed / X failed
Cmd Arbiter: X passed / X failed
Integration: X passed / X failed
Active Relocalization Regression: X passed / X failed
LiDAR Regression: X passed / X failed
GNSS Regression: X passed / X failed
Other Runnable: X passed / X failed

ROS2 Build:
PASS/FAIL/NOT_RUN + reason

Default Bringup Modified:
NO

GNSS Integrated Into EKF:
NO

Real Robot Tested:
NO

20 cm Proven:
NO

Competition >95% Proven:
NO

Capability:
PARTIAL

Remote Verified:
YES/NO

然后 STOP。

这一轮做完，我们再继续沿这条线往下，不跑去做别的。**后面真正的大块就是把 GNSS 从“质量旁路”升级成实际的多源定位输入**，但也会用独立 competition 配置先做，不去直接破坏原机器人。memcite
~~~~

## 消息 911 / user / 分段 1 of 1

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

## 消息 914 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "model_editable_context",
  "model_set_context": ""
}
~~~~

## 消息 916 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "reasoning_recap",
  "content": "思考了 5s"
}
~~~~

## 消息 917 / assistant / 分段 1 of 1

~~~~text
这份还是刚才那一轮已经审核过的结果，不是新的集成结果。

关键是：

- Starting HEAD：`b134f9da...`
- Final HEAD：`67e8358e...`

这一轮我已经给了 **PASS_WITH_NOTES**，主动重定位部分已经正式收口，不需要再修了。

现在应该执行下一轮“统一导航定位集成”，正确的 Starting HEAD 必须是：

`67e8358eb3789e16d2959a8531f403579c76fe84`

下一轮目标是把：

`LiDAR robust + GNSS quality + AMCL + Scan-to-Map + Active Relocalization + cmd_vel 仲裁`

真正接成统一的 **DG Navigation Integration POC**。

你前面如果没保存我那份集成提示词，我可以直接再完整发你一次。memcite
~~~~

## 消息 918 / user / 分段 1 of 1

~~~~text
Starting HEAD: `67e8358eb3789e16d2959a8531f403579c76fe84`
Final HEAD: `2782b3efa48d7e7960351a27adc106c63e23f6d5`

Integration:

- competition launch: YES
- LiDAR quality wired: YES
- GNSS quality wired: YES
- AMCL health wired: YES
- Scan-to-Map wired: YES
- Active Relocalization wired: YES
- Unified navigation status: YES

Command Arbitration:

- nav source: YES
- recovery source: YES
- single final `/cmd_vel` output: YES
- stale source zero: YES
- switch guard: YES
- manual safety: YES

Tests:

- Navigation Health: 17 passed / 0 failed
- Cmd Arbiter: 10 passed / 0 failed
- Integration: 20 passed / 0 failed
- Active Relocalization Regression: 50 passed / 0 failed
- LiDAR Regression: 11 passed / 0 failed
- GNSS Regression: 13 passed / 0 failed
- Other Runnable: 23 passed / 0 failed

ROS2 Build: NOT\_RUN — ROS2/colcon unavailable
Default Bringup Modified: NO
GNSS Integrated Into EKF: NO
Real Robot Tested: NO
20 cm Proven: NO
Competition >95% Proven: NO
Capability: PARTIAL
Remote Verified: YES
~~~~

## 消息 919 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "model_editable_context",
  "model_set_context": ""
}
~~~~

## 消息 944 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "thoughts",
  "thoughts": [
    {
      "summary": "识别状态新鲜度问题",
      "content": "我发现仲裁器没有跟踪健康状态的新鲜度：节点停在 NOMINAL 或 RECOVERING 后失联时，仲裁器仍可能选择导航源，造成恢复命令被忽略或意图冲突。应加入接收时间超时，并核查手动接管锁存行为。",
      "chunks": [],
      "finished": true
    },
    {
      "summary": "识别状态新鲜度问题",
      "content": "",
      "chunks": [],
      "finished": true
    }
  ],
  "source_analysis_msg_id": "1e2244d6-322d-42d4-8334-d64edd40c46c"
}
~~~~

## 消息 945 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "thoughts",
  "thoughts": [
    {
      "summary": "审查状态映射",
      "content": "我正在核对参数转换与状态协议的一致性。除了新鲜度问题，我还注意到扫描匹配结果可能因过期长期停留在 DEGRADED，而重定位状态可能永久保持 RECOVERED；这些状态转换需要进一步确认。",
      "chunks": [],
      "finished": true
    },
    {
      "summary": "审查状态映射",
      "content": "",
      "chunks": [],
      "finished": true
    }
  ],
  "source_analysis_msg_id": "7b7e2e2b-e31d-42a7-9d0e-23ed2c6e18d8"
}
~~~~

## 消息 970 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "thoughts",
  "thoughts": [
    {
      "summary": "审查导航集成、健康检查及速度仲裁器变更",
      "content": "",
      "chunks": [],
      "finished": true
    }
  ],
  "source_analysis_msg_id": "46558730-b5cd-4168-a9c1-34608947cebf"
}
~~~~

## 消息 971 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "reasoning_recap",
  "content": "思考了 3m 20s"
}
~~~~

[本对话目录](README.md) | [总目录](../../README.md) | [Raw](https://raw.githubusercontent.com/weiyang02520-ops/dg202611-conversation-archive/main/conversations/first/part-014.md) | [上一卷](part-013.md) | [下一卷](part-015.md)
