# 机器人定位挑战赛分析 / 第 024 卷

[本对话目录](README.md) | [总目录](../../README.md) | [Raw](https://raw.githubusercontent.com/weiyang02520-ops/dg202611-conversation-archive/main/conversations/first/part-024.md) | [上一卷](part-023.md) | [下一卷](part-025.md)

## 消息 1426 / assistant / 分段 1 of 2

~~~~text
下面这段直接给 Codex。第三轮 A 的范围已经锁死：只做 synthetic injector + recorder/evaluator + S01～S04，完成后必须停。

DG-202611 THIRD ROUND A — SYNTHETIC ROS2 VALIDATION FRAMEWORK

任务目标：

在已经通过 ROS2 Humble 运行集成验证的 DG-202611 比赛导航软件链上，建立第一版“合成 ROS2 数据注入 + 自动记录 + 自动评价”框架。

本轮目标不是证明比赛性能，而是证明：

1. 给现有比赛节点输入确定性的合成传感器数据后；
2. GNSS、LiDAR、多源融合、Navigation Health 等模块能够按照当前代码设计产生合理状态变化；
3. 能够自动记录 rosbag；
4. 能够自动导出 CSV；
5. 能够自动生成状态时间线；
6. 能够根据预先定义的规则输出 PASS / FAIL；
7. 结果可以重复运行和复核。

本轮禁止 Gazebo、禁止真实机器人、禁止真实运动控制、禁止声称达到比赛性能指标。

==================================================
一、当前工作基础
==================================================

虚拟机：

Ubuntu 22.04.5 LTS
ROS2 Humble
Python 3.10.12

工作区：

/home/weiyang/dg202611_ws

仓库：

/home/weiyang/dg202611_ws/src/electric-power-inspection-robot

远程仓库：

https://github.com/liaojingwu20041031/electric-power-inspection-robot

固定原始比赛软件事实源：

c41adca7f9bddb4240a1a4855437518b36d9fe13

第二轮 ROS2 运行集成修复分支：

dg202611-runtime-integration

第二轮本地提交：

096bf66377e0dbaa3ed812782222ea95a2439c7a

提交信息：

fix(ros2): repair DG navigation runtime integration

注意：

该 commit 当前只存在虚拟机本地，未 push。

==================================================
二、开始前必须核验
==================================================

SSH 进入虚拟机以后：

cd /home/weiyang/dg202611_ws/src/electric-power-inspection-robot

执行：

git status
git branch --show-current
git rev-parse HEAD
git log -3 --oneline

必须确认：

1. 当前能够找到：
   096bf66377e0dbaa3ed812782222ea95a2439c7a

2. 该提交父链包含：
   c41adca7f9bddb4240a1a4855437518b36d9fe13

3. 工作区 clean。

如果任意一点不成立：
立即停止并报告。

禁止自行猜测或重新制作第二轮修改。

==================================================
三、新建第三轮开发分支
==================================================

从：

096bf66377e0dbaa3ed812782222ea95a2439c7a

创建：

dg202611-synthetic-validation

后续所有第三轮 A 修改只能发生在该分支。

不得修改：

dg202611-runtime-integration

不得改写固定事实源 c41adca。

本轮完成后本地 commit，但不要 push。

==================================================
四、本轮允许开发的内容
==================================================

本轮允许：

1. 新建合成验证 ROS2 package；
2. 编写 synthetic injector；
3. 编写 scenario runner；
4. 编写 recorder/evaluator；
5. 编写 S01～S04 场景 YAML；
6. 编写自动运行脚本；
7. 编写 CSV、JSON、Markdown 结果输出；
8. 调用 rosbag2 自动记录；
9. 增加仅针对合成验证框架的测试；
10. 如确有必要，可做最小 launch 参数调整，但不得改算法。

建议新 package：

src/dg_synthetic_validation

可以根据实际仓库结构调整名称，但必须是独立验证包，不要把大量测试逻辑塞进 ylhb_base 核心算法代码。

==================================================
五、本轮禁止修改
==================================================

禁止修改以下核心算法逻辑：

- GNSS质量门控算法；
- LiDAR鲁棒处理算法；
- dynamic candidate算法；
- stable feature算法；
- geometry score算法；
- Scan-to-Map匹配算法；
- Active Relocalization状态机；
- Multisource Fusion数学逻辑；
- 自适应观测权重算法；
- Navigation Health决策算法；
- cmd_vel Arbiter决策算法。

如果合成测试发现核心算法行为不符合预期：

记录为 FAIL / BLOCKER。

不要为了让测试通过而修改算法。

这是强制要求。

==================================================
六、不要处理的内容
==================================================

本轮禁止：

- Gazebo；
- Ignition / Gazebo Sim；
- RViz展示优化；
- 真车；
- Jetson；
- ZED SDK；
- CUDA；
- TensorRT；
- ylhb_perception；
- 北斗短报文；
- 视觉融合；
- 新导航算法；
- 20cm性能实验；
- 95%特征重复率性能实验；
- >95%重定位成功率实验；
- S05～S08主动重定位完整场景。

第三轮 A 只做 S01～S04。

==================================================
七、安全要求
==================================================

不得连接真实机器人。

不得连接真实底盘。

不得向真实：

/cmd_vel

发送测试运动控制。

集成 launch 必须继续使用第二轮已经加入的安全输出参数，将最终控制输出定向到：

/dg/test_cmd_vel

运行期间必须检查：

ros2 topic info /cmd_vel

若真实 /cmd_vel 出现比赛测试链 publisher：

立即停止。

正确测试目标为：

/dg/test_cmd_vel

即使 Active Relocalization 产生旋转恢复命令，也只能进入测试话题。

==================================================
八、先读取实际接口，不得凭提示词硬编码
==================================================

开始写 injector 之前，必须重新读取当前分支代码并建立实际接口表。

重点读取：

GNSS quality node
LiDAR robust node
active relocalization node
scan-map relocalization node
multisource fusion node
navigation health node
cmd_vel arbiter node
dg_navigation_integration.launch.py

确认：

- 订阅 topic；
- 发布 topic；
- message type；
- QoS；
- 参数；
- 默认阈值；
- DiagnosticArray 字段；
- 状态字符串；
- freshness语义；
- Fusion mode；
- Navigation Health状态；
- cmd_vel来源状态。

尤其禁止根据本提示词推测确切状态名。

代码是唯一软件行为事实源。

==================================================
九、建立接口证据文件
==================================================

在合成验证 package 或 docs 中生成：

docs/runtime_contract.md

至少记录：

模块
节点
输入topic
输入message
输出topic
输出message
关键字段
关键状态
关键阈值来源
对应源代码文件

后续场景 YAML 和 evaluator 都应基于这份实际接口表。

==================================================
十、合成验证包建议结构
==================================================

建议结构如下，可根据实际 ROS2 Python package 规范合理调整：

src/dg_synthetic_validation/

package.xml
setup.py
setup.cfg

resource/
dg_synthetic_validation/

dg_synthetic_validation/
__init__.py
synthetic_injector_node.py
scenario_runner.py
evaluator_node.py
result_writer.py
scenario_schema.py

config/
s01_nominal.yaml
s02_gnss_degradation.yaml
s03_lidar_geometry_degradation.yaml
s04_concurrent_degradation.yaml

launch/
synthetic_validation.launch.py

scripts/
run_scenario.sh
run_all_s01_s04.sh

docs/
runtime_contract.md
README.md

test/
必要的非ROS核心测试

不要为了完全照抄这个结构而制造无意义文件。

以简单、可维护、可运行优先。

==================================================
十一、Synthetic Injector职责
==================================================

Injector负责生成确定性的 ROS2 输入。

至少根据实际节点需求支持：

/gps/fix
/gps/rtk_status
/scan
/odom
/map
/amcl_pose

如当前实际链路还必须使用：

/initialpose
/tf
/tf_static
/scan_match_pose

则按真实接口补充。

不要为了“看起来完整”发布不需要的话题。

==================================================
十二、GNSS合成数据
==================================================

GNSS输入必须符合当前 WTRTK980 / GNSS quality gate 实际数据格式。

/gps/fix：

使用 sensor_msgs/msg/NavSatFix。

/gps/rtk_status：

根据真实代码发布对应 DiagnosticArray / DiagnosticStatus / KeyValue。

至少能够控制：

- fix availability；
- quality；
- satellites；
- HDOP；
- differential_age；
- timestamp / freshness。

如果当前质量门控还检查：

- 位置跳变；
- implied speed；
- stale；
- status freshness；

则 synthetic injector 必须能够构造对应输入。

具体数值不得凭空另定阈值。

必须读取当前代码中的：

GOOD threshold
DEGRADED threshold
REJECT threshold

场景输入值应使用：

“阈值附近的明确上下界”

例如：

正常阶段 = 明确优于 GOOD 阈值；
退化阶段 = 落入 DEGRADED 区间；
拒绝阶段 = 明确越过 REJECT 条件。

在 YAML 中记录这些值。

==================================================
十三、LiDAR合成数据
==================================================

使用：

sensor_msgs/msg/LaserScan

构造确定性的二维扫描。

必须至少能生成两类：

A. 正常几何：

- 足够有效点；
- 较宽角覆盖；
- 相邻帧稳定；
- geometry score应明显高于当前健康阈值。

B. 几何退化：

例如通过：

- 只保留窄角度范围有效点；
- 大部分beam设置为inf；
- 降低稳定有效覆盖；

使当前算法计算出的：

geometry_score

明显低于健康阈值。

注意：

必须使用现有 LiDAR 算法真实计算 geometry_score。

禁止 injector 直接伪造：

/dg/lidar/quality

因为本轮目的就是验证真实 LiDAR POC 能否从输入 LaserScan 得到正确状态。

同理：

不得直接伪造 GNSS gate 输出。

==================================================
十四、Odom / AMCL / Map
==================================================

为了隔离 S01～S04 的目标：

/odom：

保持稳定、连续、有限的二维运动输入。

可以设置简单匀速或静止场景。

/amcl_pose：

S01～S04保持健康。

不要在第三轮 A 人为制造 AMCL 定位失败。

因为 Active Relocalization 完整触发属于后续 S05～S08。

/map：

如 Scan-to-Map节点运行需要，则生成最小有效 OccupancyGrid。

目标只是确保节点正常运行，不是做真实地图匹配性能实验。

==================================================
十五、时间和随机性
==================================================

所有场景必须确定性。

要求：

- 场景持续时间固定；
- 阶段持续时间固定；
- 输入频率固定；
- 如使用随机数，固定 random seed；
- seed写入结果metadata。

优先保持实现简单。

如果使用系统 wall clock 已足以获得确定状态序列，可以使用 wall clock。

不要仅为了“更像仿真”强行引入复杂 /clock 机制。

如果选择 use_sim_time：

必须确保全部相关节点统一使用模拟时间，并证明 timer / freshness逻辑正常。

==================================================
十六、S01 — NOMINAL
==================================================

场景目的：

建立健康基线。

输入：

- GNSS正常；
- satellites明显满足GOOD；
- HDOP明显满足GOOD；
- differential_age健康；
- fix持续；
- LiDAR宽角覆盖；
- 相邻帧稳定；
- odom正常；
- AMCL健康；
- map有效。

需要自动检查：

1. GNSS质量没有进入REJECTED/STALE；
2. LiDAR geometry score为健康范围；
3. 融合节点产生有限输出；
4. Fusion没有错误拒绝全部健康观测；
5. Navigation Health没有进入FAILED类状态；
6. Active Relocalization不得无故进入恢复过程；
7. /dg/test_cmd_vel保持安全；
8. 不得出现真实 /cmd_vel 测试publisher。

PASS条件必须从实际代码状态语义生成，不要凭提示词猜状态名。

==================================================
十七、S02 — GNSS DEGRADATION
==================================================

场景至少分为：

Phase A：GOOD

Phase B：DEGRADED

Phase C：REJECTED 或 STALE

Phase D：保持GNSS失效一段时间

使用真实代码阈值控制：

- satellites；
- HDOP；
- differential_age；
- fix；
- freshness。

自动检查：

1. GNSS gate能够识别健康阶段；
2. 输入跨越退化阈值后出现退化状态；
3. 输入跨越拒绝条件后不再当作健康观测；
4. accepted_fix行为符合当前代码定义；
5. fusion在GNSS失效后不继续错误采纳坏GNSS修正；
6. 有健康odom时融合输出仍保持有限；
7. 整个过程无节点崩溃。

记录状态转换延迟。

不要测试恢复后的统计性能。

==================================================
十八、S03 — LIDAR GEOMETRY DEGRADATION
==================================================

场景至少分：

Phase A：正常LiDAR几何

Phase B：逐渐减少有效角覆盖或稳定点

Phase C：明显几何退化

GNSS保持健康。

AMCL保持健康。

自动检查：

1. 正常阶段 geometry score为健康；
2. 退化输入确实导致 geometry score下降；
3. geometry score跨越当前代码健康阈值；
4. LiDAR quality / Navigation Health能够体现退化；
5. fusion不得将明显退化LiDAR当成满置信度健康绝对修正；
6. 无节点崩溃。

记录：

raw point count
valid point count
stable point count
dynamic candidate count
temporal match ratio
angular coverage
geometry score

实际字段以代码为准。

==================================================
十九、S04 — CONCURRENT DEGRADATION
==================================================

这是第三轮 A 最关键场景。

场景至少包括：

Phase A：

GNSS健康
+
LiDAR健康

Phase B：

GNSS开始退化
+
LiDAR开始几何退化

Phase C：

GNSS REJECTED/STALE
+
LiDAR geometry score低于健康阈值

同时保持：

odom连续有效；
AMCL不人为设置成失败。

自动检查：

1. GNSS质量状态正确恶化；
2. LiDAR质量状态正确恶化；
3. 两个观测不能继续同时被当成健康输入；
4. adaptive confidence / measurement confidence按代码逻辑下降；
5. fusion mode进入当前代码定义的退化、dead-reckoning或等价状态；
6. fusion输出不得出现NaN/inf；
7. Navigation Health必须体现双源退化；
8. 不因仅GNSS+LiDAR退化而制造不符合当前设计的虚假“恢复成功”；
9. 全部节点保持运行；
10. /dg/test_cmd_vel安全。

具体 Fusion mode 和 Navigation state 以实际代码为准。

==================================================
二十、明确不测试S05～S08
==================================================

本轮禁止实现：

S05 定位失效
S06 主动旋转搜索
S07 Scan-to-Map自动候选恢复
S08 多帧验证与RECOVERED

Active Relocalization节点可以保持运行。

但第三轮 A 不以它的完整闭环作为验收目标。

如果 S01～S04 意外触发 Active Relocalization：

必须记录原因。

如果是不合理触发：

场景FAIL。

不要修改算法掩盖问题。

==================================================
二十一、Recorder
==================================================

每个场景必须自动启动 rosbag2。

至少记录实际存在的相关话题：

传感器输入：

/gps/fix
/gps/rtk_status
/scan
/odom
/map
/amcl_pose

算法输
~~~~

[本对话目录](README.md) | [总目录](../../README.md) | [Raw](https://raw.githubusercontent.com/weiyang02520-ops/dg202611-conversation-archive/main/conversations/first/part-024.md) | [上一卷](part-023.md) | [下一卷](part-025.md)
