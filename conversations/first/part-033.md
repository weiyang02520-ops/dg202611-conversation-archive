# 机器人定位挑战赛分析 / 第 033 卷

[本对话目录](README.md) | [总目录](../../README.md) | [Raw](https://raw.githubusercontent.com/weiyang02520-ops/dg202611-conversation-archive/main/conversations/first/part-033.md) | [上一卷](part-032.md) | [下一卷](part-034.md)

## 消息 1551 / assistant / 分段 2 of 2

~~~~text
inal C：

RViz2

如果后续进入 Gazebo：

未来再增加 Gazebo GUI。

当前不要安装或启动 Gazebo。

==================================================
二十三、cmd_vel安全规则
==================================================

必须非常醒目地写进 handoff：

TEST_OUTPUT:

/dg/test_cmd_vel

REAL_ROBOT_CMD:

/cmd_vel

本阶段禁止向：

/cmd_vel

发送测试控制。

最终 S01～S04 验证：

/cmd_vel

起始 publisher：

0

结束 publisher：

0。

未来 Claude Code 每轮涉及控制时：

必须检查：

ros2 topic info /cmd_vel -v

以及：

/dg/test_cmd_vel

实际 publisher。

如果测试链开始控制：

/cmd_vel

立即停止。

==================================================
二十四、不能伪造 /dg 输出
==================================================

当前 synthetic injector：

只能发布传感器和基础输入。

DIRECT_DG_OUTPUTS_FAKED_BY_INJECTOR:

NO

禁止未来为了PASS直接发布：

/dg/gnss/quality

/dg/lidar/quality

/dg/fusion/status

/dg/navigation/status

/dg/relocalization/status

这些必须由真实比赛节点产生。

==================================================
二十五、当前能力成熟度
==================================================

Claude Code必须知道真实性边界。

请按以下格式整理：

ENGINEERING_FOUNDATION

SOFTWARE_POC

ROS2_RUNTIME_VALIDATED

SYNTHETIC_VALIDATED

NOT_YET_GAZEBO_VALIDATED

NOT_YET_TARGET_HARDWARE_VALIDATED

NOT_YET_REAL_ROBOT_VALIDATED

NOT_YET_COMPETITION_METRIC_VALIDATED

不要使用模糊的：

“已经完成定位系统”。

==================================================
二十六、当前已有工程基础
==================================================

核实并记录：

Jetson Orin Nano Super

Ubuntu 22.04 / ROS2 Humble

差速底盘

ZLAC8015D V4

RPLidar

HiPNUC IMU

WTRTK980 RTK/GNSS

ZED 2i

SLAM Toolbox

AMCL

Nav2

wheel + IMU 2D EKF

注意：

当前 EKF：

wheel + IMU

2D

world frame为当前代码实际配置。

不得写：

已经融合GNSS。

不得写：

3D EKF。

==================================================
二十七、当前比赛软件POC
==================================================

记录当前实际代码存在的：

GNSS quality gate

LiDAR robust processing

stable feature extraction

dynamic candidate handling

LiDAR geometry quality

Scan-to-Map

Active Relocalization state machine

Scan-to-Map recovery logic

Multisource Fusion side channel

Adaptive observation weighting

Navigation Health

cmd_vel Arbiter

Synthetic Validation Framework

但区分：

代码存在

和

实验已验证。

==================================================
二十八、明确没有完成的东西
==================================================

至少写明：

S05～S08：

NOT_RUN

Gazebo：

NOT_RUN

Real Robot：

NOT_RUN

Jetson target integration：

NOT_DONE

ZED SDK / VIO integration：

NOT_DONE

mmWave Radar：

NOT_DONE

Optical Flow：

NOT_DONE

Event Camera：

NOT_DONE

BeiDou short-message：

NOT_IMPLEMENTED

Official XY <20cm：

NOT_PROVEN

Official Z <20cm：

NOT_PROVEN

Feature repeatability >=95%：

NOT_PROVEN

Relocalization success >95%：

NOT_PROVEN

不得因为 S01～S04 PASS 提升这些成熟度。

==================================================
二十九、高程/Z问题
==================================================

Claude Code必须知道：

当前主定位体系仍以二维为主。

Wheel + IMU EKF：

2D。

2D LiDAR / AMCL：

不能单独证明 Z / 高程性能。

官方高程要求仍是：

OPEN TECHNICAL GAP。

后续候选包括：

ZED 2i VIO

RTK altitude

3D state estimator

3D LiDAR

其它高度观测

但尚未选定。

==================================================
三十、老师新定位方案
==================================================

只登记为：

NEXT_STAGE_TECHNOLOGY_SELECTION_CONTEXT。

不要现在实施。

老师提出：

常规定位

+

创新定位

+

北斗

共同形成整体定位体系。

当前下一阶段重点候选：

1. ZED 2i VIO / 3D localization
2. mmWave Radar
3. Optical Flow / Ground Texture辅助
4. Event / ToF / Polarization 等研究储备

Claude Code不得在第一次接管时自行安装这些组件。

==================================================
三十一、推荐下一阶段顺序
==================================================

请根据当前实际状态给出推荐，但不得执行。

当前默认候选顺序：

PHASE A：

S05～S08 Active Relocalization synthetic closed-loop validation

↓

PHASE B：

ZED 2i / VIO technical verification and interface design

↓

PHASE C：

3D / Z-axis localization POC

↓

PHASE D：

mmWave Radar technology selection

↓

PHASE E：

Gazebo / sensor simulation

↓

PHASE F：

Jetson target integration

↓

PHASE G：

real robot

↓

PHASE H：

official metric validation。

如果你认为顺序需要调整：

可以提出。

但不要执行。

==================================================
三十二、不要让Claude Code第一次就直接开发
==================================================

CLAUDE_CODE_TAKEOVER_PROMPT 必须要求 Claude Code：

第一步只做：

READ

VERIFY

REPORT。

Claude Code首次接管后必须：

1. SSH连接；
2. 核验系统环境；
3. 核验 repo；
4. 核验 branch；
5. 核验 HEAD；
6. 核验 clean status；
7. 阅读 handoff；
8. 阅读 runtime_contract；
9. 找到 summary；
10. 找到最新 S01～S04；
11. 检查 build/test入口；
12. 检查安全规则；
13. 返回接管确认。

第一次接管：

禁止改代码。

禁止 commit。

禁止重跑实验。

禁止安装软件。

禁止 push。

等外部审核确认接管成功以后：

才会发送下一轮开发任务。

==================================================
三十三、创建正式交接文件
==================================================

请在仓库中创建：

docs/dg202611/CLAUDE_CODE_HANDOFF.md

如果：

docs/dg202611/

不存在：

允许创建。

这个文件只记录：

非敏感开发交接信息。

禁止写：

密码

API key

token

私钥内容。

同时可以创建：

docs/dg202611/CLAUDE_CODE_FIRST_CONNECT_CHECKLIST.md

如果你认为有帮助。

==================================================
三十四、Git处理
==================================================

本轮允许提交的内容仅限：

handoff documentation。

不得修改其它开发代码。

建议 commit：

docs(dg): add Claude Code development handoff

不要 push。

如果不希望新增 commit：

可以解释原因并保持工作树 clean。

但优先建议：

形成一个独立 handoff documentation commit。

最终必须报告：

HANDOFF_COMMIT

以及：

git status。

==================================================
三十五、最后生成可直接复制的Claude Code提示词
==================================================

最终报告最后必须有：

CLAUDE_CODE_TAKEOVER_PROMPT:

其内容必须完全独立、自包含。

不能写：

“参考上面的内容”

“和之前一样”

“沿用上一轮”

“你应该已经知道”

Claude Code看不到当前 Codex 对话。

因此必须把所有首次接管所需：

连接

路径

branch

commit

环境

安全规则

事实边界

需要核验的内容

第一次任务

全部写进去。

这段提示词以后会被整体复制给 Claude Code。

==================================================
三十六、最终输出格式
==================================================

最终返回：

HANDOFF_STATUS:

ENVIRONMENT:

SSH_CONNECTION:
user:
host:
port:
alias:
auth_method:
safe_command:
unknown_fields:

VMWARE_CONTEXT:

WORKSPACE:

REPOSITORY:

REMOTE:

BRANCH:

BASELINE:

RUNTIME_INTEGRATION_BASE:

SYNTHETIC_BASE:

FINAL_HEAD:

GIT_STATUS:

LOCAL_ONLY_COMMITS:

PUSH_STATUS:

ROS_SETUP_COMMANDS:

BUILD_COMMANDS:

TEST_COMMANDS:

NODE_EXECUTABLES:

RUNTIME_CONTRACT:

SCENARIO_COMMANDS:
S01:
S02:
S03:
S04:

LATEST_RESULTS:
S01:
S02:
S03:
S04:

SUMMARY_FILES:

HISTORICAL_FAILED_S01:

RVIZ:
config:
desktop_command:
ssh_gui_status:

LIVE_MONITOR:
command:
behavior:

TMUX:
installed:
version:
recommended_session:
commands:

USER_VISIBLE_WORKFLOW:

CMD_VEL_SAFETY:

SYNTHETIC_INJECTOR_RULES:

DIAGNOSTIC_COMPATIBILITY_FIX:

CURRENT_CAPABILITIES:

VALIDATED_CAPABILITIES:

UNVALIDATED_CAPABILITIES:

OFFICIAL_METRIC_STATUS:

Z_AXIS_GAP:

NEXT_TECHNOLOGY_CONTEXT:

RECOMMENDED_NEXT_PHASE:

HANDOFF_FILES_CREATED:

HANDOFF_COMMIT:

GIT_FINAL_STATUS:

BLOCKERS:

OPEN_QUESTIONS:

CLAUDE_CODE_TAKEOVER_PROMPT:

==================================================
三十七、完成后停止
==================================================

完成：

环境核验

+

连接信息整理

+

运行/测试命令整理

+

结果路径整理

+

事实边界整理

+

安全规则整理

+

CLAUDE_CODE_HANDOFF.md

+

首次接管 checklist

+

最终 CLAUDE_CODE_TAKEOVER_PROMPT

之后立即停止。

不要自行启动下一阶段开发。

等待外部审核。

——CODEX → CLAUDE CODE HANDOFF TASK END——
~~~~

## 消息 1552 / user / 分段 1 of 1

~~~~text
你现在接管 DG-202611 的 ROS2 开发线。本次是首次接管，只允许 READ、VERIFY、REPORT，禁止开发。

连接信息：

SSH user: weiyang
SSH host: 192.168.3.130
SSH port: 22
安全连接命令: ssh weiyang@192.168.3.130
认证方式: 此前使用交互式密码认证；凭据由项目所有者另行提供
SSH alias: 没有已确认的命名 alias；Windows SSH config 中有 192.168.3.130 的 IP Host 配置
Key authentication: UNKNOWN_NOT_VERIFIED

禁止将密码、私钥、Token 或 API key 写入仓库、日志或报告。

目标环境：

Ubuntu hostname: ubuntu103
Ubuntu: 22.04.5 LTS
ROS_DISTRO: humble
Python: 3.10.12
colcon: /usr/bin/colcon
Git: 2.34.1

Workspace:
/home/weiyang/dg202611_ws

Repository:
/home/weiyang/dg202611_ws/src/electric-power-inspection-robot

Remote:
origin https://github.com/liaojingwu20041031/electric-power-inspection-robot.git 
 
Expected branch: 
dg202611-synthetic-validation 
 
Expected current HEAD: 
57ec8ecff90d4e383e8c5197105bc8d182474245 
 
该提交是 docs-only handoff commit，其父提交和已验证功能代码 HEAD 是： 
fd2c60d54401480fd40590857be55b9443c43d49 
 
固定比赛事实源： 
c41adca7f9bddb4240a1a4855437518b36d9fe13 
 
ROS2 runtime-integration base： 
096bf66377e0dbaa3ed812782222ea95a2439c7a 
 
Synthetic-validation base： 
f7676ee3d9f53bae33d384f5bb506eaa144df095 
 
预期工作树状态：clean 
当前开发提交尚未 push，分支没有已配置 upstream。 
 
登录后显式执行： 
 
source /opt/ros/humble/setup.bash 
source /home/weiyang/dg202611_ws/install/setup.bash 
cd /home/weiyang/dg202611_ws 
 
不要依赖 .bashrc 自动加载 DG workspace。当前已知： 
 
ROS_DOMAIN_ID=NOT_SET 
RMW_IMPLEMENTATION=NOT_SET 
DISPLAY=NOT_SET（SSH） 
ROS_LOCALHOST_ONLY=0 
 
Ubuntu103 运行在 VMware Workstation。后台 SSH 输出不会自动显示在 Ubuntu GUI。Git、build、test、headless scenario 和日志适合 SSH；RViz2 和用户可视观察适合 VMware Ubuntu 桌面终端。SSH 无 DISPLAY 时无法启动 RViz，不代表配置失败。 
 
首次接管按以下顺序执行，只做只读核验： 
 
1. SSH 连接。 
2. 执行 hostname、whoami、hostname -I，核验系统和 ROS 环境。 
3. 执行： 
 
cd /home/weiyang/dg202611_ws/src/electric-power-inspection-robot 
git status --short --branch 
git branch --show-current 
git rev-parse HEAD 
git log --oneline --decorate -15 
git remote -v 
 
4. 如果 branch、HEAD 或 clean status 与预期不一致，停止并报告；禁止 reset、clean、checkout 覆盖、merge 或 rebase。 
5. 完整阅读： 
 
/home/weiyang/dg202611_ws/src/electric-power-inspection-robot/docs/dg202611/CLAUDE_CODE_HANDOFF.md 
 
/home/weiyang/dg202611_ws/src/electric-power-inspection-robot/docs/dg202611/CLAUDE_CODE_FIRST_CONNECT_CHECKLIST.md 
 
/home/weiyang/dg202611_ws/src/electric-power-inspection-robot/src/dg_synthetic_validation/docs/runtime_contract.md 
 
6. 找到并只读检查： 
 
/home/weiyang/dg202611_ws/results/synthetic/summary.csv 
/home/weiyang/dg202611_ws/results/synthetic/summary.md 
 
最新结果目录： 
 
S01: 
/home/weiyang/dg202611_ws/results/synthetic/S01_nominal_20260828-145833 
 
S02: 
/home/weiyang/dg202611_ws/results/synthetic/S02_gnss_gradual_degradation_and_outage_20260828-145858 
 
S03: 
/home/weiyang/dg202611_ws/results/synthetic/S03_lidar_geometry_degradation_20260828-145928 
 
S04: 
/home/weiyang/dg202611_ws/results/synthetic/S04_gnss_and_lidar_concurrent_degradation_20260828-145956 
 
预期：S01～S04 PASS。S02～S04 有 RELOCALIZATION_ACTIVITY_OBSERVED_OUT_OF_SCOPE warning；这不表示主动重定位闭环成功。S05～S08 没有运行。 
 
7. 确认历史失败证据仍存在，不得修改、覆盖或删除： 
 
/home/weiyang/dg202611_ws/results/synthetic/S01_nominal_20260827-205016 
 
8. 阅读 handoff 中已经验证的定向 build/test 命令，但首次接管不得执行 build、test 或 scenario。 
 
已记录的结果边界： 
 
ylhb_base targeted pytest: 253 passed 
dg_synthetic_validation pytest: 4 passed 
py_compile: PASS 
完整选定包 colcon test: 666 tests, 0 errors, 335 failures, 10 skipped 
完整测试不是 PASS；主要失败是已有 flake8/pep257/风格问题，也不能将其误判为 DG 核心行为测试失败。 
 
9. 核验安全规则： 
 
TEST_OUTPUT=/dg/test_cmd_vel 
REAL_ROBOT_CMD=/cmd_vel 
 
synthetic injector 只能发布： 
/gps/fix 
/gps/rtk_status 
/scan 
/odom 
/amcl_pose 
/map 
/initialpose 
 
禁止 injector 直接发布任何 /dg/* 算法输出。禁止测试链向 /cmd_vel 发布。未来涉及控制前必须运行： 
 
ros2 topic info /cmd_vel -v 
ros2 topic info /dg/test_cmd_vel -v 
 
如果测试链向 /cmd_vel 发布，立即停止。 
 
10. 当前成熟度只能表述为： 
 
ENGINEERING_FOUNDATION 
SOFTWARE_POC 
ROS2_RUNTIME_VALIDATED 
SYNTHETIC_VALIDATED 
NOT_YET_GAZEBO_VALIDATED 
NOT_YET_TARGET_HARDWARE_VALIDATED 
NOT_YET_REAL_ROBOT_VALIDATED 
NOT_YET_COMPETITION_METRIC_VALIDATED 
 
不得声称 XY/Z <20 cm、特征重复率 >=95%、重定位成功率 >95%，不得把合成数据包装成实车或竞赛性能。 
 
11. 当前 tmux 未安装，首次接管不得安装。Monitor 命令为： 
 
ros2 run dg_synthetic_validation monitor_scenario 
 
它是只读订阅器。RViz 配置为： 
 
/home/weiyang/dg202611_ws/install/dg_synthetic_validation/share/dg_synthetic_validation/rviz/dg_synthetic_validation.rviz 
 
首次接管不要启动 Monitor、RViz 或实验。 
 
12. 返回结构化接管确认，至少包含： 
 
实际 host/user/IP 
Ubuntu、ROS_DISTRO、Python、colcon、Git 
workspace 和 repository 
branch、HEAD、HEAD parent、git status、remote 
三个交接/runtime contract 文件是否可读 
summary 和最新 S01～S04 是否存在 
历史失败 S01 是否保留 
对 build/test 入口的理解 
对 /cmd_vel 安全边界和 injector 边界的理解 
所有与预期不一致之处 
TAKEOVER_READY: YES 或 NO 
 
首次接管严格禁止： 
 
修改任何代码或文档 
commit 
build 
test 
重跑 S01～S04 
运行 S05～S08 
安装软件 
push 
merge 
rebase 
删除历史结果 
启动 Gazebo 
连接或控制真实机器人 
开发 ZED/VIO、毫米波、光流、事件相机或北斗短报文 
 
完成 READ、VERIFY、REPORT 后立即停止，等待外部审核和下一条明确开发指令。，，，，，，你看看这个行不
~~~~

## 消息 1556 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "model_editable_context",
  "model_set_context": ""
}
~~~~

## 消息 1559 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "thoughts",
  "thoughts": [
    {
      "summary": "评估了接管提示词的完整性",
      "content": "",
      "chunks": [],
      "finished": true
    }
  ],
  "source_analysis_msg_id": "dc8f3831-900e-4079-b473-25cba95fdba8"
}
~~~~

## 消息 1560 / assistant / 分段 1 of 1

~~~~text
{
  "content_type": "reasoning_recap",
  "content": "思考了 22s"
}
~~~~

## 消息 1561 / assistant / 分段 1 of 1

~~~~text
可以，这份已经能直接给 Claude Code 了，结构很稳。

我刚顺手核了一下 GitHub，`liaojingwu20041031/electric-power-inspection-robot` 仓库和你写的 `origin` 是对应的，默认分支是 `main`。而你这里的 `57ec8ec...` 是明确写成 **local-only handoff commit**，所以 Claude Code 应该以 Ubuntu 本地 Git 为准核验它，不能拿 GitHub 查不到就说有问题。

我只建议再补 **4 个很小的点**，会让第一次接管更稳：

1. **明确核验 HEAD 父提交**。现在虽然写了父提交是 `fd2c60...`，但执行命令里没单独要求验证。加一句：
   `git rev-parse HEAD^`
   预期必须是：
   `fd2c60d54401480fd40590857be55b9443c43d49`

2. **SSH 密码交互失败时不要乱试**。加一句：
   > 如果 SSH 因交互式密码认证无法由当前 Claude Code 会话完成，立即停止并请求项目所有者完成认证；禁止修改 SSH 配置、创建密钥、保存密码或尝试绕过认证。

3. **禁止它因为本地提交没 push 而“顺手同步 GitHub”**。你虽然已经写了禁止 push，但可以再明确：
   > `57ec8ec...` 和此前开发提交未出现在 remote 属于预期状态，不是 blocker，不得因此 push。

4. **首次接管最后最好有一个非常短的判定标准**：
   `TAKEOVER_READY: YES` 只有同时满足：
   - host/user 正确
   - branch 正确
   - HEAD=`57ec8ec...`
   - HEAD^=`fd2c60...`
   - working tree clean
   - 3 个交接文件可读
   - summary 与 S01～S04 存在
   - 历史失败 S01 存在
   - 安全边界理解无误

除此之外，我觉得**不用再加内容了**。现在已经够完整，再继续塞会让 Claude 第一次接管花太多 token。

还有一点我很赞成：你这次没有直接让 Claude Code 接着写代码，而是先让它 **READ → VERIFY → REPORT**。这样我们能先确认 Claude Code 确实进到了正确的 Ubuntu、正确的仓库和正确的 HEAD，再把正式开发权交过去。

所以流程现在就是：

**把这份给 Claude Code → 它只核验 → 把接管报告给我 → 我审核 TAKEOVER_READY → 再发 Claude Code 第一轮真正开发任务。**

这一步通过以后，Codex 开发线就可以正式退休到“历史开发记录/备用审查”角色了。
~~~~

[本对话目录](README.md) | [总目录](../../README.md) | [Raw](https://raw.githubusercontent.com/weiyang02520-ops/dg202611-conversation-archive/main/conversations/first/part-033.md) | [上一卷](part-032.md) | [下一卷](part-034.md)
