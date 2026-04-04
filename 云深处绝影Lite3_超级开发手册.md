# 🐾 云深处 绝影Lite3 专业版 — 超级二次开发手册

> **版本**: v1.0 | **更新日期**: 2026-04-04  
> **适用机型**: 绝影Lite3 专业版（Lite3 Pro）/ 探索版（Lite3 Venture）  
> **厂商**: 杭州云深处科技股份有限公司（DEEP Robotics / DeepRobotics）  
> **官方网站**: https://www.deeprobotics.cn/  
> **官方 GitHub**: https://github.com/DeepRoboticsLab  
> **技术支持**: support@deeprobotics.cn  
> **Discord 社区**: https://discord.gg/gdM9mQutC8

---

## 📑 目录

- [第一章 绝影Lite3 产品概述](#第一章-绝影lite3-产品概述)
- [第二章 硬件规格与参数](#第二章-硬件规格与参数)
- [第三章 系统架构与网络拓扑](#第三章-系统架构与网络拓扑)
- [第四章 开发环境搭建](#第四章-开发环境搭建)
- [第五章 MotionSDK 关节控制开发（C++）](#第五章-motionsdk-关节控制开发c)
- [第六章 新版 SDK Deploy（ROS2 版本）](#第六章-新版-sdk-deployers2-版本)
- [第七章 ROS2 感知开发（Lite3_ROS）](#第七章-ros2-感知开发lite3_ros)
- [第八章 高层运动控制](#第八章-高层运动控制)
- [第九章 手柄遥控开发](#第九章-手柄遥控开发)
- [第十章 仿真环境搭建（MuJoCo / PyBullet）](#第十章-仿真环境搭建mujoco--pybullet)
- [第十一章 强化学习训练（rl_training / Isaac Lab）](#第十一章-强化学习训练rl_training--isaac-lab)
- [第十二章 强化学习部署（Lite3_rl_deploy）](#第十二章-强化学习部署lite3_rl_deploy)
- [第十三章 3D 模型资源（URDF / MJCF / USD）](#第十三章-3d-模型资源urdf--mjcf--usd)
- [第十四章 硬件扩展与 FAST-LIVO2 SLAM](#第十四章-硬件扩展与-fast-livo2-slam)
- [**第十五章 专业版 vs 探索版完整开发流程对比**](#第十五章-专业版-vs-探索版完整开发流程对比) ★ 新增
- [**第十六章 建图与自主导航完全指南**](#第十六章-建图与自主导航完全指南) ★ 新增
- [**第十七章 高难度动作强化学习训练实战**](#第十七章-高难度动作强化学习训练实战) ★ 新增
- [**第十八章 大语言模型（LLM）语音控制机器狗**](#第十八章-大语言模型llm语音控制机器狗) ★ 新增
- [**第十九章 VLA 视觉-语言-动作模型接入指南**](#第十九章-vla-视觉-语言-动作模型接入指南) ★ 新增
- [第二十章 开源项目完整索引](#第二十章-开源项目完整索引)
- [第二十一章 安全须知与最佳实践](#第二十一章-安全须知与最佳实践)
- [第二十二章 常见问题与故障排查](#第二十二章-常见问题与故障排查)
- [附录 A 关节编号与数据结构速查表](#附录-a-关节编号与数据结构速查表)
- [附录 B 网络配置速查表](#附录-b-网络配置速查表)
- [附录 C 常用命令速查表](#附录-c-常用命令速查表)
- [附录 D 术语表（小白词典）](#附录-d-术语表小白词典)

---

## 🌟 给初学者的话（零基础必读）

**欢迎来到四足机器人的世界！** 如果你是第一次接触编程或机器人开发，不用担心——这本手册会手把手教你。

### 我需要什么基础？

**完全零基础** 也可以开始！但建议你先了解以下内容（不懂也没关系，后面会教）：

| 你可能不懂的东西 | 简单解释 | 在哪里会用到 |
|-----------------|---------|------------|
| **Linux / Ubuntu** | 一种电脑操作系统，机器人开发都用它 | 所有章节 |
| **终端/命令行** | 在电脑上输入文字命令来操控电脑（不用鼠标点） | 所有章节 |
| **Python** | 一种编程语言，像写英语作文一样写代码 | 第十章、第十一章 |
| **C++** | 另一种编程语言，比 Python 快但更难写 | 第五章、第六章 |
| **ROS2** | 机器人操作系统，帮多个程序协同工作来控制机器人 | 第六~八章 |
| **UDP** | 一种网络通信协议，程序之间快速传递消息的"快递" | 第三章、第五章 |
| **SDK** | 软件开发工具包，别人写好的工具让你更方便控制机器人 | 第五~六章 |
| **SLAM** | 一边走一边画地图的技术，像你边走迷宫边画地图 | 第十四章 |
| **强化学习（RL）** | 让机器人像训练小狗一样，通过"奖惩"学会走路 | 第十一~十二章 |
| **URDF** | 机器人模型描述文件，告诉仿真器机器人长什么样 | 第十三章 |

### 推荐学习路线

```
🔰 完全零基础:
   第一章（了解产品）
   → 第四章（搭建环境）
   → 第八章（高层控制，让机器狗走路）
   → 第九章（手柄遥控）
   → 第十章（仿真，不怕摔坏）

🔧 有编程基础:
   第一~三章（了解产品和架构）
   → 第四章（搭建环境）
   → 第五章（MotionSDK 关节控制）
   → 第七章（ROS2 感知开发）
   → 第十章（仿真环境）
   → 第十一章（强化学习训练）
   → 第十二章（策略部署）

🚀 有 ROS2 / RL 基础:
   第六章（新版 SDK Deploy）
   → 第十一章（Isaac Lab 强化学习）
   → 第十二章（Sim-to-Real 部署）
   → 第十四章（FAST-LIVO2 SLAM 扩展）
```

### 遇到问题怎么办？

1. **看报错信息** — 电脑给出的错误提示往往就是答案
2. **搜索引擎** — 把报错信息复制粘贴到百度/Google 搜索
3. **Discord 社区** — https://discord.gg/gdM9mQutC8
4. **GitHub Issues** — 在对应仓库提问
5. **官方技术支持** — support@deeprobotics.cn
6. **重启试试** — 很多时候真的管用 😄

> 💡 **小贴士**: 本手册中看到 `这种灰色框里的文字` 表示需要在终端中输入的命令，或者代码中的名称。每段代码都有详细的中文注释，一行一行看就能理解。

---

## 第一章 绝影Lite3 产品概述

### 1.1 公司简介

**杭州云深处科技股份有限公司（DEEP Robotics）** 成立于 2017 年，是一家专注于具身智能技术创新与行业应用的国际高新技术企业。公司核心产品包括四足机器人、人形机器人以及高性能关节模组等。产品广泛应用于电力巡检、应急救援、管廊隧道、金属冶炼、建筑测绘、教育科研等领域。

- **总部**: 杭州
- **官方网站**: https://www.deeprobotics.cn/
- **联系电话**: 400-0559-095
- **教育合作**: education@deeprobotics.cn
- **售后服务**: 0571-85073618 / service@deeprobotics.cn

### 1.2 产品定位

绝影Lite3 是云深处推出的**新一代轻量级智能四足机器人（机械狗）**，定位为先进的仿生机器人平台，具有强劲的驱动力、先进精确的运动控制算法、智能的环境交互能力以及丰富的模块化扩展选项。

### 1.3 版本区别

绝影Lite3 目前有以下版本：

| 特性 | Lite3 基础版 | Lite3 专业版（Pro） | Lite3 探索版（Venture） |
|------|-------------|-------------------|----------------------|
| **主要定位** | 入门体验 | 科研开发 | 深度开发与扩展 |
| **运动主机** | ✅ | ✅ | ✅ |
| **感知主机（Orin NX）** | ❌ | ✅（自带） | ❌（需自行扩展） |
| **二次开发** | 有限 | ✅ 完整支持 | ✅ 完整支持 |
| **硬件扩展接口** | ❌ | ✅ | ✅（更灵活） |
| **RL 部署支持** | ❌ | ✅ | ✅ |
| **ROS2 感知开发** | ❌ | ✅ | ✅（需扩展） |

> ⚠️ **重要**: 专业版自带 NVIDIA Jetson Orin NX 感知计算模组，可直接进行 ROS2 感知开发和 SLAM；探索版需要自行外挂算力平台（如 AGX Jetson Orin）。

### 1.4 核心能力

- **高性能关节**: 关节扭矩较上一代提升 50%，具有极高的转矩密度、响应带宽和反向传动效率
- **先进运动算法**: 运动更加灵巧敏捷，实现更强越障能力和高难度动作
- **人机交互**: 增强第一视角实时图传性能，降低延迟
- **模块化扩展**: 加装模块化设计，支持无界拓展
- **多种开发方式**: 支持 MotionSDK（C++）、ROS2、强化学习等多种二次开发方式

### 1.5 开发方式总览

| 开发方式 | 适用场景 | 仓库 | 通信方式 |
|---------|---------|------|---------|
| **MotionSDK** | 底层关节力控/位控 | `Lite3_MotionSDK` | UDP |
| **SDK Deploy（新版）** | ROS2 关节控制 + 部署 | `sdk_deploy` | ROS2 DDS |
| **ROS2 感知开发** | 速度指令 / 传感器数据 | `Lite3_ROS` | UDP↔ROS2 |
| **RL 训练** | 强化学习策略训练 | `rl_training` | Isaac Lab |
| **RL 部署** | 训练策略 Sim-to-Real | `Lite3_rl_deploy` | UDP |
| **手柄遥控** | 实时遥控操作 | `gamepad` | UDP |

---

## 第二章 硬件规格与参数

### 2.1 机械参数

| 参数 | 数值 |
|------|------|
| **站立尺寸（长×宽×高）** | 610mm × 370mm × 406mm |
| **重量（带电池）** | 约 12kg |
| **持续行走负载** | 5kg |
| **最大斜坡坡度** | 40° |
| **连续楼梯高度** | 18cm |
| **运动续航时间** | 1.5h ~ 2h |
| **续航里程** | 约 5km |

### 2.2 自由度分布

Lite3 共有 **12 个主动关节**（每条腿 3 个关节）：

| 部位 | 关节名称 | 自由度数 |
|------|---------|---------|
| **左前腿（FL）** | FL_HipX, FL_HipY, FL_Knee | 3 |
| **右前腿（FR）** | FR_HipX, FR_HipY, FR_Knee | 3 |
| **左后腿（HL）** | HL_HipX, HL_HipY, HL_Knee | 3 |
| **右后腿（HR）** | HR_HipX, HR_HipY, HR_Knee | 3 |
| **总计** | | **12** |

> **关节命名规则**:
> - `Hip` = 髋关节（大腿根部）
> - `Knee` = 膝关节
> - `X` = 侧摆方向（Roll，左右方向）
> - `Y` = 俯仰方向（Pitch，前后方向）

### 2.3 关节电机特性

- **类型**: 云深处自研高性能电机
- **驱动精度**: 电机电流闭环控制频率 20kHz
- **控制方式**: 支持位置控制、速度控制、力矩控制、阻尼控制及混合控制
- **关节安全**: 当 SDK 超过 1 秒未发送指令，底层自动进入阻尼保护模式

### 2.4 传感器

| 传感器 | 描述 |
|--------|------|
| **IMU（惯性测量单元）** | 包含三轴加速度计、三轴陀螺仪，提供 Roll/Pitch/Yaw 角度 |
| **关节编码器** | 每个关节提供位置、速度、力矩、温度反馈 |
| **足端力传感器** | 四足各提供 X/Y/Z 三轴接触力数据 |
| **深度相机**（专业版） | 前向感知，避障与跟随 |

### 2.5 感知功能

- 前后停障
- 识别跟随

### 2.6 电气接口（专业版）

| 接口类型 | 描述 |
|---------|------|
| **以太网口** | 有线连接开发主机 |
| **WiFi** | 无线连接开发主机 |
| **24V XT30** | 可为外部设备供电（如 LiDAR） |
| **USB** | 外接传感器/设备 |

---

## 第三章 系统架构与网络拓扑

### 3.1 整体架构

绝影Lite3 内部采用 **运动主机 + 感知主机（专业版）** 的双主机架构：

```
┌──────────────────────────────────────────────────┐
│                    Lite3 机器狗                     │
│                                                    │
│  ┌──────────────────┐   ┌──────────────────────┐  │
│  │   运动主机（ARM）   │   │  感知主机（Orin NX）  │  │
│  │                    │   │   （仅专业版自带）     │  │
│  │  - 关节控制         │   │                      │  │
│  │  - 运动算法         │   │  - ROS2 节点          │  │
│  │  - 底层安全保护     │   │  - 相机/雷达驱动      │  │
│  │  - jy_exe 进程     │   │  - SLAM/导航           │  │
│  │                    │   │  - 二次开发             │  │
│  └────────┬───────────┘   └──────────┬───────────┘  │
│           │        UDP               │              │
│           └──────────────────────────┘              │
└──────────────────────────────────────────────────────┘
         │ WiFi / Ethernet
         ▼
┌──────────────────────┐     ┌──────────────────────┐
│   开发者电脑           │     │   手柄 App            │
│   (Ubuntu x86)        │     │   (Retroid 遥控手柄)  │
│                       │     │                      │
│  - SDK 开发            │     │  - 遥控控制           │
│  - RL 训练             │     │  - 模式切换           │
│  - 仿真               │     │  - 图传显示           │
└───────────────────────┘     └──────────────────────┘
```

### 3.2 通信协议

整个系统的通信核心是 **UDP 协议**：

| 通信链路 | 协议 | 端口 | 说明 |
|---------|------|------|------|
| 运动主机 → SDK | UDP | 43897 | 运动主机向 SDK 发送关节数据/IMU 数据 |
| SDK → 运动主机 | UDP | 43893 | SDK 向运动主机发送关节控制指令 |
| 手柄 → 开发主机 | UDP | 12121 | 手柄按键事件传输 |
| 感知主机 ↔ 运动主机 | UDP | — | 内部数据交换 |

### 3.3 网络拓扑

**WiFi 连接模式**:
- WiFi 名称: `Lite*******`（出厂设置）
- WiFi 密码: `12345678`（默认，如有问题联系技术支持）
- 网段 1: 运动主机 IP = `192.168.1.120`
- 网段 2: 运动主机 IP = `192.168.2.1`

**以太网连接模式**:
- 运动主机 IP: `192.168.1.120`

### 3.4 运动主机登录信息

运动主机为 ARM 架构，运行 Linux 系统。默认登录账户如下（按优先级尝试）：

| 用户名 | 密码 | 说明 |
|--------|------|------|
| `ysc` | `'`（一个英文单引号） | 默认主账户 |
| `user` | `123456` | 备用账户 |
| `firefly` | `firefly` | 旧版本账户 |

> 💡 通过 SSH 登录: `ssh ysc@192.168.2.1`，输入密码即可进入远程开发模式。

### 3.5 运动主机核心进程

运动主机上运行着名为 `jy_exe` 的核心运动程序：

```
~/jy_exe/
├── conf/
│   ├── network.toml         # 网络配置（SDK 目标 IP、端口）
│   └── name.toml            # 机器人型号配置
├── scripts/
│   ├── run.sh               # 启动运动程序
│   ├── stop.sh              # 停止运动程序
│   ├── restart.sh           # 重启运动程序
│   └── show_log.sh          # 查看运行日志
```

---

## 第四章 开发环境搭建

### 4.1 硬件准备

| 项目 | 要求 |
|------|------|
| **开发电脑** | Ubuntu 20.04 / 22.04（推荐使用原生 Linux 或双系统，不推荐虚拟机） |
| **网络** | 能连接到机器狗 WiFi 或通过以太网直连 |
| **GPU**（RL 训练） | NVIDIA GPU（GTX 3060 以上推荐），安装 CUDA |
| **机器狗** | 绝影Lite3 专业版 / 探索版 |

> ⚠️ 如果使用虚拟机开发，需要配置**桥接网络模式**，手动设置 IP 地址，并在设置后重启。

### 4.2 基础软件安装

#### 4.2.1 Ubuntu 操作系统

如果你使用 Windows，推荐安装双系统或使用 WSL2：
- Ubuntu 20.04 LTS（Focal）— 适配 ROS2 Foxy
- Ubuntu 22.04 LTS（Jammy）— 适配 ROS2 Humble

#### 4.2.2 基础工具

```bash
# 更新系统
sudo apt update && sudo apt upgrade -y

# 安装基础开发工具
sudo apt install -y build-essential cmake git vim ssh net-tools

# 安装 Python 开发工具
sudo apt install -y python3 python3-pip python3-dev

# 安装网络工具
sudo apt install -y tcpdump curl wget
```

#### 4.2.3 安装 ROS2

**Ubuntu 20.04 安装 ROS2 Foxy**:
```bash
# 设置 locale
sudo apt update && sudo apt install locales
sudo locale-gen en_US en_US.UTF-8
sudo update-locale LC_ALL=en_US.UTF-8 LANG=en_US.UTF-8
export LANG=en_US.UTF-8

# 添加 ROS2 源
sudo apt install software-properties-common
sudo add-apt-repository universe
sudo apt update && sudo apt install curl -y
sudo curl -sSL https://raw.githubusercontent.com/ros/rosdistro/master/ros.key -o /usr/share/keyrings/ros-archive-keyring.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/ros-archive-keyring.gpg] http://packages.ros.org/ros2/ubuntu $(. /etc/os-release && echo $UBUNTU_CODENAME) main" | sudo tee /etc/apt/sources.list.d/ros2.list > /dev/null

# 安装 ROS2 Foxy
sudo apt update
sudo apt install ros-foxy-desktop -y

# 配置环境变量
echo "source /opt/ros/foxy/setup.bash" >> ~/.bashrc
source ~/.bashrc
```

**Ubuntu 22.04 安装 ROS2 Humble**:
```bash
# 添加源并安装（步骤类似，将 foxy 替换为 humble）
sudo apt install ros-humble-desktop -y
echo "source /opt/ros/humble/setup.bash" >> ~/.bashrc
source ~/.bashrc
```

### 4.3 连接机器狗

#### 4.3.1 WiFi 连接

1. 打开机器狗电源
2. 在电脑上搜索 WiFi 名称 `Lite*******`（以 Lite 开头）
3. 输入密码: `12345678`
4. 连接成功后，检查自己电脑的 IP 地址：

```bash
ifconfig
# 或
ip addr
```

- 如果网段是 `192.168.1.xxx`，运动主机 IP 为 `192.168.1.120`
- 如果网段是 `192.168.2.xxx`，运动主机 IP 为 `192.168.2.1`

#### 4.3.2 以太网连接

1. 用网线将电脑连接到机器狗以太网口
2. 手动设置电脑 IP 为 `192.168.1.xxx`（避开 120）
3. 运动主机 IP 为 `192.168.1.120`

#### 4.3.3 验证连接

```bash
# 测试网络连通性
ping 192.168.2.1        # 或 192.168.1.120

# SSH 连接到运动主机
ssh ysc@192.168.2.1     # 密码是 ' (一个英文单引号)
```

如果连接成功，你会看到机器狗运动主机的终端提示符。

### 4.4 配置运动主机

首次使用 SDK 前，需要配置运动主机的数据发送目标地址：

```bash
# SSH 连接运动主机
ssh ysc@192.168.2.1

# 打开网络配置文件
cd ~/jy_exe/conf
vim network.toml
```

配置文件内容：
```toml
ip = '192.168.1.102'      # ← 改为你的开发电脑的 IP 地址！
target_port = 43897
local_port = 43893

ips = ['192.168.1.103']
ports = [43897]
```

> ⚠️ **关键**: 将 `ip` 字段改为你的开发电脑的实际 IP 地址。如果 SDK 运行在运动主机本身上，则设为 `192.168.1.120` 或 `192.168.2.1`。

修改完成后重启运动程序：
```bash
cd ~/jy_exe/scripts
sudo ./stop.sh
sudo ./restart.sh
```

### 4.5 检查运动主机版本

```bash
cd ~/jy_exe/scripts
./stop.sh
./run.sh
```

查看终端输出的 Deeprcs Version 信息。根据版本号可能需要进一步配置（参见第五章 5.2 节）。

---

## 第五章 MotionSDK 关节控制开发（C++）

> **仓库**: https://github.com/DeepRoboticsLab/Lite3_MotionSDK  
> **语言**: C++（76%），支持 ARM / x86 交叉编译  
> **许可**: MIT  
> **注意**: 该仓库为经典 MPC 实现，官方推荐新项目使用新版 `rl_training` + `sdk_deploy`

### 5.1 SDK 简介

MotionSDK 是绝影Lite3 最基础的底层控制接口，提供 **5 个控制参数** 来控制每个关节的运动：

- `pos_goal` — 目标位置（弧度）
- `vel_goal` — 目标速度（弧度/秒）
- `kp` — 位置比例增益
- `kd` — 速度微分增益
- `tff` — 前馈力矩

最终关节控制信号的计算公式：

$$T = k_p \times (pos_{goal} - pos_{real}) + k_d \times (vel_{goal} - vel_{real}) + t_{ff}$$

底层驱动会将计算出的关节控制信号转换为电流，并以 **20kHz** 的频率进行闭环控制。

### 5.2 常见控制模式示例

| 控制模式 | pos_goal | vel_goal | kp | kd | tff |
|---------|----------|----------|----|----|-----|
| **位置控制** | 3.14 | 0 | 30 | 0 | 0 |
| **速度控制** | 0 | 5 | 0 | 1 | 0 |
| **阻尼控制** | 0 | 0 | 0 | 1 | 0 |
| **力矩控制** | 0 | 0 | 0 | 0 | 3.0 |

### 5.3 下载与编译

```bash
# 克隆仓库
cd ~/robot_dev     # 你的开发目录
git clone --recurse-submodules https://github.com/DeepRoboticsLab/Lite3_MotionSDK.git
cd Lite3_MotionSDK

# 创建构建目录
mkdir build && cd build

# --- 在开发电脑（x86 Ubuntu）上编译 ---
cmake .. -DBUILD_PLATFORM=x86
make -j

# --- 在机器狗运动主机（ARM）上编译 ---
cmake .. -DBUILD_PLATFORM=arm
make -j
```

编译成功后，会在 `build` 目录下生成 `Lite_motion` 可执行文件。

### 5.4 配置 SDK

修改 `main.cpp` 第 39 行中的 IP 地址为运动主机的 IP：

```cpp
Sender* send_cmd = new Sender("192.168.1.120", 43893);  ///< 创建发送线程
// 将IP改为你的运动主机地址（192.168.1.120 或 192.168.2.1）
```

### 5.5 检查通信

为了安全，原始代码中 `main.cpp` 第 73 行的发送命令是被**注释掉的**：

```cpp
//send_cmd->SendCmd(robot_joint_cmd);  // 默认注释，确保安全
```

**首先验证通信是否正常**：

1. 取消注释 `main.cpp` 第 75 行的 IMU 数据打印：
```cpp
cout << robot_data->imu.acc_x << endl;  // 打印 IMU 加速度
```

2. 重新编译并运行：
```bash
cd build
cmake .. -DBUILD_PLATFORM=x86
make -j
./Lite_motion
```

3. 观察终端是否打印 IMU 数据。如果看到数据输出，说明通信正常。

4. 如果看到 `No data from the robot was received!!!!!!`，说明通信异常，参见 5.7 节排查。

### 5.6 运行关节控制

确认通信正常后：

1. 取消 `main.cpp` 第 73 行的注释：
```cpp
send_cmd->SendCmd(robot_joint_cmd);  // 取消注释，启用关节控制
```

2. 重新编译并运行

> ⚠️ **安全警告**: 运行关节控制前，确保：
> - 所有人员距离机器狗至少 **5 米**
> - 机器狗应悬挂在悬吊装置上
> - 调整机器狗至准备姿态（趴下）
> - 如需接近机器人，必须先按急停或执行 `sudo ./stop.sh`

### 5.7 通信故障排查

如果 SDK 收不到机器人数据：

1. **检查网络连通性**:
```bash
ping 192.168.2.1    # ping 运动主机
```

2. **检查 IP 配置**: 确保 `network.toml` 中的 IP 是你开发电脑的实际 IP

3. **抓包检查**:
```bash
# 在运动主机上执行（根据连接方式选择）

# SDK 运行在运动主机上
sudo tcpdump -x port 43897 -i lo

# 用户名 user + WiFi 连接
sudo tcpdump -x port 43897 -i p2p0

# 用户名 user + 以太网连接
sudo tcpdump -x port 43897 -i eth1

# 用户名 firefly + 任意连接
sudo tcpdump -x port 43897 -i eth0
```

4. **检查运动进程**:
```bash
top    # 查看 jy_exe 是否在运行
# 如果没在运行，重启：
cd ~/jy_exe/scripts
sudo ./stop.sh
sudo ./restart.sh
```

5. **虚拟机用户**: 确保使用桥接网络模式，手动设置 IP 后重启虚拟机

### 5.8 完整代码示例解析

```cpp
#include "receiver.h"
#include "sender.h"
#include "dr_timer.h"
#include "motion_example.h"

// === 1. 定时器初始化 ===
DRTimer set_timer;
set_timer.TimeInit(int);              // 定时器初始化，输入：周期（毫秒）
set_timer.GetCurrentTime();           // 获取当前时间
set_timer.TimerInterrupt();           // 定时器中断标志
set_timer.GetIntervalTime(double);    // 获取间隔时间

// === 2. 创建发送线程（连接运动主机）===
Sender* send_cmd = new Sender("192.168.1.120", 43893);
send_cmd->RobotStateInit();           // 所有关节归零，获取控制权
send_cmd->SetSend(RobotCmd);          // 发送关节控制指令
send_cmd->ControlGet(int);            // 归还控制权

// === 3. 创建接收线程（接收关节数据）===
Receiver* robot_data_recv = new Receiver();
robot_data_recv->GetState();          // 接收 12 个关节的数据
robot_data_recv->RegisterCallBack(CallBack);  // 注册回调函数

// === 4. 读取机器人状态数据 ===
RobotData *robot_data = &robot_data_recv->GetState();

// --- IMU 数据 ---
robot_data->imu.acc_x;               // X 轴加速度
robot_data->imu.acc_y;               // Y 轴加速度
robot_data->imu.acc_z;               // Z 轴加速度
robot_data->imu.angle_pitch;         // 俯仰角
robot_data->imu.angle_roll;          // 横滚角
robot_data->imu.angle_yaw;           // 偏航角
robot_data->imu.angular_velocity_pitch;   // 俯仰角速度
robot_data->imu.angular_velocity_roll;    // 横滚角速度
robot_data->imu.angular_velocity_yaw;     // 偏航角速度
robot_data->imu.timestamp;           // 时间戳

// --- 关节数据 ---
robot_data->joint_data.fl_leg[0].position;     // 左前腿 HipX 位置
robot_data->joint_data.fl_leg[0].velocity;     // 左前腿 HipX 速度
robot_data->joint_data.fl_leg[0].torque;       // 左前腿 HipX 力矩
robot_data->joint_data.fl_leg[0].temperature;  // 左前腿 HipX 温度

// --- 足端力 ---
robot_data->contact_force.fl_leg[0];  // 左前足 X 轴力
robot_data->contact_force.fl_leg[1];  // 左前足 Y 轴力
robot_data->contact_force.fl_leg[2];  // 左前足 Z 轴力
robot_data->contact_force.leg_force[]; // 所有足端力（12 个值）

// === 5. 发送关节控制指令 ===
RobotCmd robot_joint_cmd;
robot_joint_cmd.fl_leg[0]->kp;        // 左前腿 HipX 的 Kp
robot_joint_cmd.fl_leg[0]->kd;        // 左前腿 HipX 的 Kd
robot_joint_cmd.fl_leg[0]->position;   // 左前腿 HipX 目标位置
robot_joint_cmd.fl_leg[0]->velocity;   // 左前腿 HipX 目标速度
robot_joint_cmd.fl_leg[0]->torque;     // 左前腿 HipX 前馈力矩

// === 6. 站立 Demo 流程 ===
MotionExample robot_set_up_demo;

// Step 1: 花 1 秒收腿准备站立
robot_set_up_demo.PreStandUp(robot_joint_cmd, now_time, *robot_data);

// Step 2: 记录当前时间和角度
robot_set_up_demo.GetInitData(robot_data->motor_state, now_time);

// Step 3: 花 1.5 秒站起来
robot_set_up_demo.StandUp(robot_joint_cmd, now_time, *robot_data);
```

### 5.9 版本检查与配置

连接运动主机后，检查 Deeprcs 版本：

```bash
cd ~/jy_exe/scripts
./stop.sh
./run.sh
```

- 如果用户名是 `user` 且版本早于 **1.4.24(97)**，或用户名是 `firefly` 且版本早于 **1.4.21(94)**：
  ```bash
  cd ~/jy_exe/conf
  vim name.toml
  # 将 name = standard_1 改为 name = standard_2
  ```

- 修改后重启运动程序：
  ```bash
  cd ~/jy_exe/scripts
  sudo ./stop.sh
  sudo ./restart.sh
  ```

### 5.10 注意事项

1. Lite3 运动主机是 **ARM 架构**，如果要在运动主机上运行程序需要使用 ARM 编译
2. WiFi 环境干扰可能导致通信延迟波动，对 **500Hz 以上控制频率** 的控制器有影响
3. 当底层控制器超过 **1 秒** 未收到 SDK 指令，会自动进入阻尼保护模式

---

## 第六章 新版 SDK Deploy（ROS2 版本）

> **仓库**: https://github.com/DeepRoboticsLab/sdk_deploy  
> **版本**: v1.4.1（最新）  
> **语言**: C++（98.3%）  
> **许可**: BSD-3-Clause  
> **支持机型**: Lite3 / M20

### 6.1 概述

`sdk_deploy` 是云深处最新发布的 ROS2 版 SDK 部署方案，是官方推荐的新一代开发方式。相比旧版 `Lite3_MotionSDK`（纯 UDP），新版使用 ROS2 DDS 通信，具有更好的扩展性和生态兼容性。

> ⚠️ **注意**: 因使用 SDK 造成的设备损坏不在保修范围内！

### 6.2 仓库结构

```
sdk_deploy/
├── src/
│   ├── drdds/                   # drdds 通信格式定义（SDK 通用）
│   ├── Lite3_sdk_deploy/        # Lite3 SDK 部署代码
│   ├── lite3_sdk_service/       # Lite3 SDK 模式切换、频率修改服务
│   └── lite3_transfer/          # Lite3 UDP↔ROS2 消息转换
├── README.md
└── README_lite3_sdk_service.md  # SDK 服务说明文档
```

### 6.3 ROS2 Topic 与 Service

#### 6.3.1 lite3_transfer 节点

发布的 Topic：

| Topic 名称 | 消息类型 | 说明 |
|-----------|---------|------|
| `/IMU_DATA` | `drdds/msg/ImuData` | IMU 数据 |
| `/JOINTS_DATA` | `drdds/msg/JointsData` | 关节状态数据 |

订阅的 Topic：

| Topic 名称 | 消息类型 | 说明 |
|-----------|---------|------|
| `/JOINTS_CMD` | `drdds/msg/JointsDataCmd` | 关节控制指令 |

服务：

| Service 名称 | 类型 | 说明 |
|-------------|------|------|
| `/SDK_MODE` | `drdds/srv/StdSrvInt32` | SDK 模式切换 |

#### 6.3.2 retroid_gamepad 节点

| Topic 名称 | 消息类型 | 说明 |
|-----------|---------|------|
| `/GAMEPAD_DATA` | `drdds/msg/GamepadData` | 手柄按键数据 |

### 6.4 部署步骤（Lite3）

#### 6.4.1 前置准备

1. 配置运动主机网络（参见第四章 4.4 节）
2. 运动主机上安装 ROS2 Foxy（Ubuntu 20.04）

#### 6.4.2 传输与编译

```bash
# 在你的电脑上执行：传输文件到 Lite3 运动主机
# 密码是 ' (单引号)
ssh ysc@192.168.2.1 'mkdir -p ~/lite3_sdk_service/src' && \
scp -r ~/sdk_deploy/src/drdds ~/sdk_deploy/src/lite3_sdk_service ~/sdk_deploy/src/lite3_transfer ysc@192.168.2.1:~/lite3_sdk_service/src

# SSH 连接到运动主机
ssh ysc@192.168.2.1

# 编译
cd lite3_sdk_service
source /opt/ros/foxy/setup.bash
colcon build --packages-up-to lite3_sdk_service
```

#### 6.4.3 运行服务

```bash
# 运行 SDK 服务
cd lite3_sdk_service
source install/setup.bash
export ROS_DOMAIN_ID=0
ros2 run lite3_sdk_service sdk_service
```

### 6.5 设置开机自启（Systemd）

```bash
# 创建 systemd 服务文件
sudo vim /etc/systemd/system/lite3_sdk_service.service
```

写入以下内容：
```ini
[Unit]
Description=Lite3 SDK Control UDP Service
After=network.target

[Service]
Type=simple
User=ysc
Environment=ROS_DOMAIN_ID=0
ExecStart=/bin/bash -lc 'source /opt/ros/foxy/setup.bash && \
                         source /home/ysc/transfer/install/setup.bash && \
                         exec /home/ysc/transfer/install/lite3_sdk_service/lib/lite3_sdk_service/sdk_service'
Restart=on-failure

[Install]
WantedBy=multi-user.target
```

启用与管理：
```bash
sudo systemctl daemon-reload                 # 重新加载配置
sudo systemctl enable lite3_sdk_service      # 开机自启
sudo systemctl start lite3_sdk_service       # 立即启动
sudo systemctl status lite3_sdk_service      # 查看状态
sudo systemctl stop lite3_sdk_service        # 停止服务
sudo systemctl restart lite3_sdk_service     # 重启服务
sudo journalctl -u lite3_sdk_service -f      # 查看日志
```

---

## 第七章 ROS2 感知开发（Lite3_ROS）

> **仓库**: https://github.com/DeepRoboticsLab/Lite3_ROS  
> **分支**: `ros2-foxy`（ROS2 Foxy / Ubuntu 20.04）  
> **语言**: C++（93.5%）  
> **许可**: MIT

### 7.1 概述

`Lite3_ROS` 提供了 UDP ↔ ROS2 消息转换功能。它将运动主机通过 UDP 发送的数据（关节状态、IMU 等）转换为标准 ROS2 Topic 消息，同时将 ROS2 速度指令转换为 UDP 发送给运动主机。

### 7.2 发布的 ROS2 Topic

| Topic 名称 | 消息类型 | 说明 |
|-----------|---------|------|
| `/leg_odom` | `geometry_msgs::msg::PoseWithCovarianceStamped` | 腿部里程计（仅位姿） |
| `/leg_odom2` | `nav_msgs::msg::Odometry` | 腿部里程计（位姿 + 速度） |
| `/imu/data` | `sensor_msgs::msg::Imu` | IMU 数据 |
| `/joint_states` | `sensor_msgs::msg::JointState` | 关节状态 |

### 7.3 订阅的 ROS2 Topic

| Topic 名称 | 消息类型 | 说明 |
|-----------|---------|------|
| `/cmd_vel` | `geometry_msgs::msg::Twist` | 速度控制指令 |

### 7.4 速度指令格式

```
geometry_msgs/msg/Vector3 linear      # 线速度 (m/s)
    float64 x                         # 纵向速度：正值前进
    float64 y                         # 横向速度：正值左移
    float64 z                         # 无效参数

geometry_msgs/msg/Vector3 angular     # 角速度 (rad/s)
    float64 x                         # 无效参数
    float64 y                         # 无效参数
    float64 z                         # 偏航角速度：正值左转
```

### 7.5 使用方法

```bash
# 终端 1: 启动转换节点
source ~/lite_cog_ros2/transfer/install/setup.bash
ros2 launch transfer transfer.launch

# 终端 2: 查看机器人状态
ros2 topic info /imu/data
ros2 topic echo /imu/data

ros2 topic info /joint_states
ros2 topic echo /joint_states

ros2 topic info /leg_odom2
ros2 topic echo /leg_odom2
```

### 7.6 发送速度指令控制机器狗行走

```bash
# 通过命令行发送（10Hz 频率）
ros2 topic pub -r 10 /cmd_vel geometry_msgs/msg/Twist \
  "{linear:{x: 0.2, y: 0.1, z: 0.0}, angular:{x: 0.0, y: 0.0, z: 0.3}}"
```

Python 代码示例：
```python
import rclpy
from rclpy.node import Node
from geometry_msgs.msg import Twist

class VelocityPublisher(Node):
    def __init__(self):
        super().__init__('velocity_publisher')
        self.publisher_ = self.create_publisher(Twist, '/cmd_vel', 10)
        self.timer = self.create_timer(0.1, self.timer_callback)  # 10Hz

    def timer_callback(self):
        msg = Twist()
        msg.linear.x = 0.3    # 前进 0.3 m/s
        msg.linear.y = 0.0    # 不横移
        msg.angular.z = 0.1   # 缓慢左转
        self.publisher_.publish(msg)
        self.get_logger().info(f'发送速度: vx={msg.linear.x}, wz={msg.angular.z}')

def main():
    rclpy.init()
    node = VelocityPublisher()
    rclpy.spin(node)
    node.destroy_node()
    rclpy.shutdown()

if __name__ == '__main__':
    main()
```

> ⚠️ **安全提醒**: 发送速度指令前，先通过 App 切换到**自动模式**。在开阔场地调试，紧急情况切回手动模式或按急停。

### 7.7 包结构

```
~/lite_cog_ros2/transfer/
├── LICENSE
├── README.md
├── README_ZH.md
└── src/
    ├── transfer/
    │   ├── CMakeLists.txt
    │   ├── include/
    │   │   └── protocol.hpp         # 通信协议定义
    │   ├── launch/
    │   │   └── transfer_launch.py   # 启动文件
    │   ├── package.xml
    │   └── src/
    │       ├── Jetson2App.cpp       # App 命令接收
    │       ├── Jetson2Motion.cpp    # 运动主机数据转换
    │       ├── SensorChecker.cpp    # 传感器状态检查
    │       └── SensorsLogger.hpp    # 传感器日志
    └── transfer_interfaces/
        ├── CMakeLists.txt
        ├── msg/
        │   ├── MotionComplexCMD.msg # 复杂运动指令
        │   └── MotionSimpleCMD.msg  # 简单运动指令
        └── package.xml
```

---

## 第八章 高层运动控制

### 8.1 概述

高层运动控制是最简单的控制方式——你只需要告诉机器狗"往哪走、走多快"，不需要关心每个关节怎么动。

### 8.2 通过 ROS2 Topic 控制

使用 `/cmd_vel` Topic 发送 `geometry_msgs/msg/Twist` 消息（参见第七章 7.6 节）。

### 8.3 通过 App 控制

1. 手柄上安装 `controlapp.apk`（从 `gamepad` 仓库下载）
2. 连接机器狗 WiFi
3. 打开 App，配置 IP 和端口
4. 使用摇杆控制机器狗运动

### 8.4 运动模式切换

通过 App 或服务调用切换运动模式：
- **手动模式**: 手柄直接控制
- **自动模式**: 接受 `/cmd_vel` 速度指令
- **SDK 模式**: 接受底层关节指令

---

## 第九章 手柄遥控开发

> **仓库**: https://github.com/DeepRoboticsLab/gamepad  
> **语言**: C++（97%）  
> **许可**: BSD-3-Clause

### 9.1 概述

手柄开发库通过 UDP 通信在远程主机上监听手柄物理按键触发事件。开发者可以实时获取手柄按键信息，并自行开发机器人遥控程序。

### 9.2 支持的手柄

| 手柄类型 | 对应机型 | 示例程序 |
|---------|---------|---------|
| **Retroid**（绝影Lite3 官方手柄） | Lite3 | `example_retroid.cpp` |
| **Skydroid**（绝影X30 手柄） | X30 | `example_skydroid.cpp` |

### 9.3 下载与编译

```bash
git clone --recurse-submodules https://github.com/DeepRoboticsLab/gamepad.git
cd gamepad
mkdir build && cd build
cmake .. -DBUILD_EXAMPLE=ON
make -j4
```

### 9.4 安装 App

在手柄上安装仓库中的 `controlapp.apk` 文件。安装后：

1. 打开 App
2. 点击左上角按钮
3. 配置开发主机的 IP 地址和端口号（默认 12121）

### 9.5 运行示例

```bash
# Lite3 手柄
cd build/
./example/example_retroid

# X30 手柄
cd build/
./example/example_skydroid
```

操作手柄物理按键，终端中会显示手柄按键的触发信息。

### 9.6 修改端口号

如果默认端口 12121 被占用，可修改 `example/example_retroid.cpp`：

```cpp
int main(int argc, char* argv[]) {
    RetroidGamepad rc(12121);  // ← 修改这里的端口号
    InitialRetroidKeys(rc);
    // ...
}
```

修改后重新编译即可。

---

## 第十章 仿真环境搭建（MuJoCo / PyBullet）

### 10.1 概述

在实际部署到真实机器狗之前，强烈建议先在仿真环境中测试。仿真环境提供了零风险的测试场景——即使程序写错了，仿真中的机器狗也不会摔坏。

### 10.2 支持的仿真器

| 仿真器 | 说明 | 用途 |
|--------|------|------|
| **MuJoCo** | 高精度物理仿真器 | RL 训练、策略验证 |
| **PyBullet** | 轻量级物理仿真器 | 快速原型验证 |
| **Isaac Sim / Isaac Lab** | NVIDIA GPU 加速仿真 | 大规模 RL 训练 |

### 10.3 使用 Lite3_rl_deploy 进行仿真

#### 10.3.1 环境安装

```bash
# 安装调试工具
sudo apt-get install libdw-dev
wget https://raw.githubusercontent.com/bombela/backward-cpp/master/backward.hpp
sudo mv backward.hpp /usr/include

# 安装 Python 依赖（需要 Python 3.10）
pip install pybullet "numpy < 2.0" mujoco

# 克隆仓库
git clone --recurse-submodule https://github.com/DeepRoboticsLab/Lite3_rl_deploy.git
cd Lite3_rl_deploy
```

#### 10.3.2 编译（仿真模式）

```bash
mkdir build && cd build
cmake .. -DBUILD_PLATFORM=x86 -DBUILD_SIM=ON -DSEND_REMOTE=OFF
make -j
```

参数说明：
- `-DBUILD_PLATFORM=x86` — 电脑平台（x86 或 arm）
- `-DBUILD_SIM=ON` — 启用仿真模式
- `-DSEND_REMOTE=OFF` — 不发送数据到远程

#### 10.3.3 运行仿真

需要打开 **两个终端**：

**终端 1（启动仿真器）**：
```bash
# 方式 A: 使用 PyBullet
cd interface/robot/simulation
python3 pybullet_simulation.py

# 方式 B: 使用 MuJoCo（推荐）
cd interface/robot/simulation
python3 mujoco_simulation.py
```

**终端 2（启动控制程序）**：
```bash
cd build
./rl_deploy
```

#### 10.3.4 键盘操控

在终端 2 中使用键盘控制：

| 按键 | 功能 |
|------|------|
| `z` | 机器狗站立 → 进入默认状态 |
| `c` | 机器狗站立 → 进入 RL 控制状态 |
| `w` | 前进 |
| `s` | 后退 |
| `a` | 左移 |
| `d` | 右移 |
| `q` | 逆时针旋转 |
| `e` | 顺时针旋转 |

> 💡 **技巧**: 可以将仿真器窗口设为"始终位于最上层"，方便实时观察。

---

## 第十一章 强化学习训练（rl_training / Isaac Lab）

> **仓库**: https://github.com/DeepRoboticsLab/rl_training  
> **语言**: Python（100%）  
> **许可**: BSD-3-Clause / Apache-2.0  
> **⭐ Stars**: 184+  
> **教程视频**: [Bilibili](https://b23.tv/UoIqsFn) | [YouTube](https://youtube.com/playlist?list=PLy9YHJvMnjO0X4tx_NTWugTUMJXUrOgFH&si=pjUGF5PbFf3tGLFz)

### 11.1 概述

`rl_training` 是云深处官方基于 **NVIDIA Isaac Lab** 的强化学习训练库。支持 Lite3 和 M20 机器人。

### 11.2 支持的环境

| 机器人 | 环境名称 | 模型名称 |
|--------|---------|---------|
| **Lite3** | `Rough-Deeprobotics-Lite3-v0` | `Lite3` |
| M20 | `Rough-Deeprobotics-M20-v0` | `deeprobotics_m20` |

### 11.3 安装

#### 11.3.1 安装 Isaac Lab

按照 [Isaac Lab 官方安装指南](https://isaac-sim.github.io/IsaacLab/main/source/setup/installation/index.html) 安装。推荐使用 **conda** 方式安装。

#### 11.3.2 安装 rl_training

```bash
# 在 IsaacLab 目录外克隆仓库
git clone --recurse-submodules https://github.com/DeepRoboticsLab/rl_training.git
cd rl_training

# 使用安装了 Isaac Lab 的 Python 环境
python -m pip install -e source/rl_training

# 验证安装
python scripts/tools/list_envs.py
```

### 11.4 训练策略

```bash
# 训练 Lite3 粗糙地面行走策略（无头模式，无GUI）
python scripts/reinforcement_learning/rsl_rl/train.py \
  --task=Rough-Deeprobotics-Lite3-v0 --headless

# 观看训练好的策略
python scripts/reinforcement_learning/rsl_rl/play.py \
  --task=Rough-Deeprobotics-Lite3-v0 --num_envs=10
```

### 11.5 键盘控制回放

在回放时，添加 `--keyboard` 可以用键盘控制单个机器人：

```bash
python scripts/reinforcement_learning/rsl_rl/play.py \
  --task=Rough-Deeprobotics-Lite3-v0 --keyboard
```

键盘映射：

| 功能 | 正方向按键 | 负方向按键 |
|------|----------|----------|
| 沿 X 轴移动 | 小键盘 8 / 上箭头 | 小键盘 2 / 下箭头 |
| 沿 Y 轴移动 | 小键盘 4 / 右箭头 | 小键盘 6 / 左箭头 |
| 绕 Z 轴旋转 | 小键盘 7 / Z 键 | 小键盘 9 / X 键 |

### 11.6 多 GPU 加速训练

```bash
# 2 块 GPU 训练
python -m torch.distributed.run --nnodes=1 --nproc_per_node=2 \
  scripts/reinforcement_learning/rsl_rl/train.py \
  --task=Rough-Deeprobotics-Lite3-v0 --headless --distributed --num_envs=2048
```

> 💡 每个 GPU 默认使用配置中指定的全部环境数。多 GPU 时，总环境数会翻倍。如果要保持总环境数不变，需要将每 GPU 环境数除以 GPU 数量。

### 11.7 多节点训练

```bash
# 主节点
python -m torch.distributed.run --nproc_per_node=2 --nnodes=2 --node_rank=0 \
  --rdzv_id=123 --rdzv_backend=c10d --rdzv_endpoint=localhost:5555 \
  scripts/reinforcement_learning/rsl_rl/train.py \
  --task=Rough-Deeprobotics-Lite3-v0 --headless --distributed

# 工作节点
python -m torch.distributed.run --nproc_per_node=2 --nnodes=2 --node_rank=1 \
  --rdzv_id=123 --rdzv_backend=c10d --rdzv_endpoint=<主节点IP>:5555 \
  scripts/reinforcement_learning/rsl_rl/train.py \
  --task=Rough-Deeprobotics-Lite3-v0 --headless --distributed
```

### 11.8 查看训练过程

```bash
tensorboard --logdir=logs
```

### 11.9 导出模型为 ONNX

训练完成后，导出策略为 ONNX 格式用于部署：

```bash
# 快速导出（不需要 Isaac Sim 环境）
python scripts/tools/export_onnx_fast.py \
  --checkpoint_path logs/rsl_rl/deeprobotics_lite3_rough/<run>/model_5000.pt \
  --robot lite3 \
  --output_path exported/lite3_policy.onnx
```

> 机器人元数据（关节名称、刚度/阻尼、默认位置、动作比例）会自动嵌入 ONNX 文件。添加 `--no_metadata` 可跳过。

### 11.10 比较训练实验

```bash
python scripts/tools/compare_runs.py \
  logs/rsl_rl/deeprobotics_lite3_rough/<run1> \
  logs/rsl_rl/deeprobotics_lite3_rough/<run2>
```

### 11.11 常用命令速查

| 功能 | 命令 |
|------|------|
| 训练 | `python scripts/.../train.py --task=Rough-Deeprobotics-Lite3-v0 --headless` |
| 回放 | `python scripts/.../play.py --task=Rough-Deeprobotics-Lite3-v0 --num_envs=10` |
| 录制视频 | 添加 `--video --video_length 200` |
| 指定环境数 | 添加 `--num_envs 32` |
| 恢复训练 | 添加 `--resume --load_run <folder> --checkpoint model.pt` |
| 指定 checkpoint | 添加 `--load_run <folder> --checkpoint model.pt` |
| 清理 USD 缓存 | `rm -rf /tmp/IsaacLab/usd_*` |

---

## 第十二章 强化学习部署（Lite3_rl_deploy）

> **仓库**: https://github.com/DeepRoboticsLab/Lite3_rl_deploy  
> **语言**: C++（96.7%）  
> **许可**: BSD-3-Clause  
> **⭐ Stars**: 114+  
> **教程视频**: [Bilibili](https://b23.tv/UoIqsFn) | [YouTube](https://youtube.com/playlist?list=PLy9YHJvMnjO0X4tx_NTWugTUMJXUrOgFH&si=pjUGF5PbFf3tGLFz)

### 12.1 概述

`Lite3_rl_deploy` 是将训练好的强化学习策略部署到仿真或真实 Lite3 机器狗的工具。支持 **仿真-仿真（Sim-to-Sim）** 和 **仿真-实际（Sim-to-Real）** 两种模式。

### 12.2 系统模块

```
Lite3_rl_deploy/
├── interface/          # 数据收发接口（仿真/实物 + 键盘/手柄）
├── policy/             # 策略文件（.onnx 格式）和模型转换脚本
├── run_policy/         # 策略执行模块（可继承扩展）
├── state_machine/      # 状态机模块
├── scripts/            # 辅助脚本
├── third_party/        # 第三方依赖
├── types/              # 数据类型定义
├── utils/              # 工具函数
├── Lite3_description/  # MuJoCo 模型描述
└── main.cpp            # 主程序入口
```

### 12.3 状态机

机器狗在不同状态之间切换：

| 状态 | 说明 |
|------|------|
| **Idle** | 空闲状态，关节不发力 |
| **StandUp** | 站起状态，从趴下到站立 |
| **RL** | RL 控制状态，执行策略输出的 action |
| **JointDamping** | 关节阻尼状态，关节处于阻尼控制 |

### 12.4 Sim-to-Sim 仿真部署

（参见第十章 10.3 节，完整流程相同。）

### 12.5 Sim-to-Real 实机部署

#### 12.5.1 连接机器狗 WiFi

```bash
# WiFi 名称: Lite*******
# WiFi 密码: 12345678
```

#### 12.5.2 修改 IP 配置

在运动主机上修改网络配置：

```bash
# SSH 连接运动主机
ssh ysc@192.168.2.1

# 修改配置
cd ~/jy_exe/conf
vim network.toml
```

改为：
```toml
ip = '192.168.2.1'
target_port = 43897
local_port = 43893

ips = ['192.168.1.103']
ports = [43897]
```

#### 12.5.3 传输代码到机器狗

```bash
# 在电脑上执行
scp -r ~/Lite3_rl_deploy ysc@192.168.2.1:~/
```

#### 12.5.4 在机器狗上编译

```bash
# SSH 连接
ssh ysc@192.168.2.1
# 密码: ' (单引号)

# 编译
cd Lite3_rl_deploy
mkdir build && cd build
cmake .. -DBUILD_PLATFORM=arm -DBUILD_SIM=OFF -DSEND_REMOTE=OFF
make -j

# 运行
./rl_deploy
```

#### 12.5.5 切换控制方式

默认实机使用 **Retroid 手柄模式**。如需使用键盘模式，修改 `state_machine/state_machine.hpp` 第 121 行：

```cpp
uc_ptr_ = std::make_shared<KeyboardInterface>();
```

### 12.6 模型转换（.pt → .onnx）

RL 训练输出 `.pt` 格式模型，部署需要 `.onnx` 格式：

```bash
# 安装依赖
pip install torch numpy onnx onnxruntime

# 验证安装
python3 -c 'import torch, numpy, onnx, onnxruntime; print(" All modules OK")'

# 执行转换
cd policy/
python pt2onnx.py
```

转换完成后会在当前目录生成 `.onnx` 文件。程序会自动比较两个模型的一致性。

### 12.7 手柄操控

实机部署后使用 Retroid 手柄操控，参考 gamepad 仓库：https://github.com/DeepRoboticsLab/gamepad

---

## 第十三章 3D 模型资源（URDF / MJCF / USD）

> **仓库**: https://github.com/DeepRoboticsLab/deep_robotics_model  
> **许可**: BSD-3-Clause

### 13.1 概述

该仓库提供 Lite3、M20、X30 三种机器人的 3D 模型文件，支持 URDF、MJCF 和 USD 格式。

### 13.2 Lite3 模型文件

| 类型 | 路径 | 说明 |
|------|------|------|
| **URDF（高精度）** | `Lite3/Lite3_urdf/urdf/Lite3_high_res.urdf` | 适用于 Isaac Lab/Isaac Sim |
| **URDF（低精度）** | `Lite3/Lite3_urdf/urdf/Lite3.urdf` | 适用于 PyBullet 等轻量仿真器 |
| **MJCF** | `Lite3/Lite3_mjcf/` | 适用于 MuJoCo |

> ⚠️ 高精度模型文件较大，旧版仿真器（如 PyBullet）可能无法打开。低精度模型仓库中直接提供；高精度模型需要从 [Google Drive](https://drive.google.com/drive/folders/1EOELXUYSBPEJeD0rUIkJxnlLMDr6IHBV?usp=sharing) 下载。

### 13.3 使用方法

```bash
git clone https://github.com/DeepRoboticsLab/deep_robotics_model.git

# 在线预览模型
# 推荐使用: https://viewer.robotsfan.com/
# 直接将模型文件夹拖拽到网页即可预览
```

### 13.4 模型用途

| 用途 | 格式 | 仿真器 |
|------|------|--------|
| RL 训练 | URDF | Isaac Lab / Isaac Sim |
| 仿真部署 | MJCF | MuJoCo |
| 快速验证 | URDF（低精度） | PyBullet |
| 可视化 | URDF/MJCF | RViz / 在线查看器 |

---

## 第十四章 硬件扩展与 FAST-LIVO2 SLAM

> **仓库**: https://github.com/DeepRoboticsLab/fast-livo2-deep-robotics  
> **许可**: GPL-2.0  
> **适用版本**: 仅 Lite3 Venture（探索版）

### 14.1 概述

本章介绍如何在 Lite3 上扩展硬件（LiDAR + 相机）并复现 FAST-LIVO2 同步定位与建图（SLAM）算法。

> 💡 如果你使用的是 Lite3 Pro（专业版），自带 Orin NX，可以直接作为算力平台使用，无需额外扩展。

### 14.2 系统要求

- **操作系统**: Ubuntu 22.04
- **ROS2**: Humble
- **算力扩展**: AGX Jetson Orin（推荐 Jetpack 6.1）
- **LiDAR**: Livox Mid-360
- **相机**: Intel RealSense

### 14.3 硬件安装

#### 14.3.1 算力平台

1. 设置好 AGX Jetson Orin 或其他计算平台
2. 安装 ROS2 Humble

#### 14.3.2 与 Lite3 集成

- **Lite3 Pro/LiDAR 版**: 自带 Orin NX，可直接作为额外算力
- **Lite3 Venture 版**: 需要外挂计算平台，参考教程视频安装
- 3D 打印结构件可从 [Google Drive](https://drive.google.com/drive/folders/1KmWNuOF0Qg5XM6EQKRiWrkJJhoBknjIZ?usp=drive_link) 下载

#### 14.3.3 LiDAR 连接

1. 按照 [Livox Mid-360 用户手册](https://terra-1-g.djicdn.com/851d20f7b9f64838a34cd02351370894/Livox/Livox_Mid-360_User_Manual_EN.pdf) 连接
2. 将 XT30 端插入 Lite3 的 24V XT30 接口
3. 将以太网端连接到计算平台

### 14.4 安装 FAST-LIVO2

```bash
# 克隆仓库
git clone https://github.com/DeepRoboticsLab/fast-livo2-deep-robotics.git
cd fast-livo2-deep-robotics

# 编译 Livox-SDK2
cd src/Livox-SDK2
mkdir build && cd build
cmake .. && make -j
sudo make install

# 回到项目根目录，编译 livox_ros_driver2
cd fast-livo2-deep-robotics/src/livox_ros_driver2/
source /opt/ros/humble/setup.bash
./build.sh humble

# 安装 ROS2 依赖
sudo apt update
sudo apt -y install ros-humble-pcl-ros ros-humble-compressed-image-transport ros-humble-sophus

# 安装 RealSense
sudo apt -y install ros-humble-realsense2-*

# 验证
cd fast-livo2-deep-robotics
source install/setup.bash
ros2 pkg list | grep livo
# 应看到 fast_livo 和 livox_ros_driver2
```

### 14.5 运行 FAST-LIVO2

#### 14.5.1 使用 ROS2 bag 离线测试

```bash
# 终端 1: 启动 FAST-LIVO2
cd fast-livo2-deep-robotics
source install/setup.bash
ros2 launch fast_livo mapping_avia.launch.py use_rviz:=True

# 终端 2: 回放数据集
source install/setup.bash
ros2 bag play Retail_Street   # 从 Google Drive 下载数据集
```

#### 14.5.2 在真实 Lite3 上运行

需要打开 **3 个终端**：

```bash
# 终端 1: 启动 LiDAR 驱动
cd fast-livo2-deep-robotics
source install/setup.bash
ros2 launch livox_ros_driver2 msg_MID360_launch.py

# 终端 2: 启动相机
cd fast-livo2-deep-robotics
source install/setup.bash
ros2 launch realsense2_camera rs_launch.py \
  enable_rgbd:=false enable_sync:=false align_depth.enable:=false \
  enable_color:=true enable_depth:=false

# 终端 3: 启动 SLAM
cd fast-livo2-deep-robotics
source install/setup.bash
ros2 launch fast_livo mapping_avia.launch.py use_rviz:=True
```

### 14.6 RealSense 安装注意事项

如果在 Jetson 上遇到设备权限问题，可能是 librealsense 版本冲突：

```bash
# 检查库版本
ldd $(which rs-enumerate-devices) | grep realsense

# 如果存在版本冲突，强制优先使用本地编译版
echo "/usr/local/lib" | sudo tee /etc/ld.so.conf.d/99-realsense-local.conf
sudo ldconfig
echo 'export LD_LIBRARY_PATH=/usr/local/lib:$LD_LIBRARY_PATH' >> ~/.bashrc
source ~/.bashrc
```

---

## 第十五章 专业版 vs 探索版完整开发流程对比

### 15.1 版本差异全景对比

绝影 Lite3 有两个版本：**专业版（Pro）** 和 **探索版（Venture）**，两者在硬件配置和开发能力上存在显著差异，选择不同版本将直接影响你的开发路线。

| 对比维度 | 专业版（Pro） | 探索版（Venture） |
|----------|--------------|-------------------|
| **运动主机** | ARM 主机（运动控制） | ARM 主机（运动控制） |
| **感知主机** | ✅ NVIDIA Jetson Orin NX 16GB | ❌ 无（需自行外接） |
| **LiDAR** | ✅ Livox Mid-360（出厂预装） | ❌ 无 |
| **深度相机** | ✅ Intel RealSense D435i（出厂预装） | ❌ 无 |
| **机载算力** | Orin NX（100 TOPS INT8） | 仅 ARM 运动主机 |
| **传感反馈** | IMU + 关节编码器 + LiDAR + 视觉 | IMU + 关节编码器 |
| **出厂可用功能** | SLAM 建图、3D 避障、自主导航 | 遥控行走、基础运动 |
| **SDK 支持** | MotionSDK + sdk_deploy + Lite3_ROS + FAST-LIVO2 | MotionSDK + sdk_deploy |
| **适合人群** | 全栈开发者、实验室、企业 | 底层运动控制研究者、DIY 爱好者 |
| **价格定位** | 较高 | 较低 |

### 15.2 专业版（Pro）开发流程

专业版拥有完整的感知-决策-执行链路，开发流程如下：

```
┌─────────────────── 专业版开发架构 ───────────────────┐
│                                                       │
│  ┌─────────────┐    ┌──────────────┐    ┌──────────┐ │
│  │ Livox Mid-360│    │ RealSense    │    │ IMU      │ │
│  │ (LiDAR)     │    │ D435i (视觉) │    │ 关节编码器│ │
│  └──────┬──────┘    └──────┬───────┘    └────┬─────┘ │
│         │                  │                  │       │
│         ▼                  ▼                  ▼       │
│  ┌─────────────────────────────────────────────────┐ │
│  │        Jetson Orin NX 16GB (感知主机)            │ │
│  │  Ubuntu 22.04 + ROS2 Humble + CUDA 11.4+        │ │
│  │  ┌──────────┐ ┌────────┐ ┌─────────────────┐    │ │
│  │  │FAST-LIVO2│ │ Nav2   │ │ RL推理 / LLM    │    │ │
│  │  │ SLAM建图  │ │自主导航 │ │ 决策(可选)      │    │ │
│  │  └──────────┘ └────────┘ └─────────────────┘    │ │
│  └────────────────────┬────────────────────────────┘ │
│                       │ UDP / DDS                     │
│                       ▼                               │
│  ┌─────────────────────────────────────────────────┐ │
│  │            ARM 运动主机                          │ │
│  │         关节伺服控制 + 步态生成                   │ │
│  └─────────────────────────────────────────────────┘ │
└───────────────────────────────────────────────────────┘
```

#### 15.2.1 第一阶段：基础运动控制（1-2 周）

```bash
# 1. 连接机器人 WiFi
# SSID: Lite*******  密码: 12345678

# 2. SSH 到 ARM 运动主机
ssh ysc@192.168.2.1  # 密码: '

# 3. 克隆 MotionSDK
git clone https://github.com/DeepRoboticsLab/Lite3_MotionSDK.git
cd Lite3_MotionSDK
mkdir build && cd build
cmake .. && make -j4

# 4. 测试关节控制
./lite3_motion_example
```

**目标**：熟悉 12 个关节的编号、控制模式、通信协议（UDP 端口 43893/43897）。

#### 15.2.2 第二阶段：感知开发（2-3 周）

```bash
# SSH 到 Orin NX 感知主机
ssh ysc@192.168.2.2  # 默认用户名和密码

# 1. 安装 ROS2 Humble（如未预装）
sudo apt update
sudo apt install ros-humble-desktop

# 2. 克隆 Lite3_ROS
git clone https://github.com/DeepRoboticsLab/Lite3_ROS.git
cd Lite3_ROS
colcon build --symlink-install
source install/setup.bash

# 3. 启动感知桥接
ros2 launch lite3_bringup lite3_bringup.launch.py

# 4. 查看话题
ros2 topic list
# 应看到: /imu/data, /joint_states, /leg_odom, /cmd_vel 等

# 5. 查看 LiDAR 点云
ros2 topic echo /livox/lidar  # Livox Mid-360

# 6. 查看深度图像
ros2 topic echo /camera/depth/image_rect_raw  # RealSense D435i
```

#### 15.2.3 第三阶段：SLAM 建图与导航（3-4 周）

```bash
# 在 Orin NX 上
# 1. 克隆 FAST-LIVO2
git clone https://github.com/DeepRoboticsLab/fast-livo2-deep-robotics.git
cd fast-livo2-deep-robotics
colcon build --symlink-install

# 2. 实时建图
ros2 launch fast_livo2 mapping.launch.py

# 3. 用手柄遥控机器人在场景中走一圈建图

# 4. 接入 Nav2 实现自主导航
sudo apt install ros-humble-navigation2 ros-humble-nav2-bringup
ros2 launch nav2_bringup navigation_launch.py
```

> 详细步骤见 **第十六章 建图与自主导航完全指南**。

#### 15.2.4 第四阶段：算法部署（长期）

| 可部署算法 | 运行位置 | 用途 |
|-----------|---------|------|
| FAST-LIVO2 | Orin NX | 3D SLAM 建图 |
| Nav2 | Orin NX | 自主导航与避障 |
| RL 推理 (ONNX/TRT) | Orin NX 或 ARM | 运动策略推理 |
| YOLO 目标检测 | Orin NX (GPU) | 物体/人识别 |
| LLM 推理 | Orin NX / 云端 | 语义决策 |
| VLA 模型 | 云端 (推荐) | 视觉-语言-动作 |

### 15.3 探索版（Venture）开发流程

探索版没有 Orin NX 和传感器，开发路线侧重**底层运动控制和算法研究**。

```
┌─────────────────── 探索版开发架构 ───────────────────┐
│                                                       │
│  ┌──────────────────────────┐                        │
│  │ 外接开发电脑 / NUC       │ ← 可选，通过 WiFi/USB  │
│  │ Ubuntu 22.04 + ROS2      │                        │
│  │ 算法开发 & 数据处理       │                        │
│  └────────────┬─────────────┘                        │
│               │ WiFi / Ethernet                       │
│               ▼                                       │
│  ┌──────────────────────────────────────────────────┐│
│  │            ARM 运动主机                          ││
│  │  MotionSDK / sdk_deploy                         ││
│  │  关节伺服控制 + 步态生成                         ││
│  └──────────────────────────────────────────────────┘│
│                                                       │
│  可选外接传感器:                                      │
│  - USB 摄像头 → 接开发电脑                           │
│  - 外接 LiDAR → 接开发电脑                           │
│  - Intel RealSense → 接开发电脑                       │
└───────────────────────────────────────────────────────┘
```

#### 15.3.1 第一阶段：基础运动控制（同专业版）

```bash
# 与专业版完全相同
ssh ysc@192.168.2.1  # 密码: '
git clone https://github.com/DeepRoboticsLab/Lite3_MotionSDK.git
cd Lite3_MotionSDK && mkdir build && cd build && cmake .. && make -j4
```

#### 15.3.2 第二阶段：仿真训练（重点投入）

由于没有机载感知算力，探索版的核心开发在 **开发电脑上的仿真环境** 中完成：

```bash
# 在你的开发电脑上（推荐 RTX 3060 及以上）

# 1. 安装 Isaac Sim + Isaac Lab
# 参考第十一章

# 2. 克隆 rl_training
git clone https://github.com/DeepRoboticsLab/rl_training.git
cd rl_training
pip install -e .

# 3. 训练步态策略
python scripts/train.py --task Rough-Deeprobotics-Lite3-v0 --num_envs 4096

# 4. 在仿真中验证
# - MuJoCo Sim-to-Sim 验证
git clone https://github.com/DeepRoboticsLab/Lite3_rl_deploy.git
cd Lite3_rl_deploy
mkdir build && cd build && cmake .. && make -j4
./sim2sim_mujoco --model_path /path/to/policy.onnx

# 5. 部署到真机
./sim2real --model_path /path/to/policy.onnx
```

#### 15.3.3 第三阶段：外接感知方案（可选扩展）

如果探索版用户需要感知能力，有以下方案：

**方案 A：外接 Jetson 开发板**

```bash
# 购买 Jetson Orin Nano / NX 开发套件
# 通过 Ethernet 或 USB 连接到机器人

# 1. 在 Jetson 上安装 ROS2 + Lite3_ROS
# 2. USB 连接 RealSense D435i
sudo apt install ros-humble-realsense2-camera
ros2 launch realsense2_camera rs_launch.py

# 3. USB 连接 Livox Mid-360（如有）
# 4. /cmd_vel → WiFi → ARM 运动主机
```

**方案 B：笔记本 + USB 摄像头远程控制**

```bash
# 在笔记本上运行 ROS2
# 通过 WiFi 与机器人通信

# 开发机 → /cmd_vel → WiFi → ARM 运动主机
ros2 topic pub /cmd_vel geometry_msgs/msg/Twist \
  "{linear: {x: 0.3, y: 0.0, z: 0.0}, angular: {x: 0.0, y: 0.0, z: 0.2}}"
```

### 15.4 版本选择建议矩阵

| 你的目标 | 推荐版本 | 理由 |
|---------|---------|------|
| SLAM 建图 + 自主导航 | **专业版** | 需要 LiDAR + GPU 算力 |
| RL 步态研究 | **探索版即可** | 训练在 PC 上，部署只需 ARM |
| LLM / VLA 控制 | **专业版** | 需要相机 + 机载 GPU |
| 目标跟踪 / 人跟随 | **专业版** | 需要视觉传感器 |
| 教学 / 学习底层控制 | **探索版即可** | MotionSDK 不需要感知 |
| 比赛 / 商用开发 | **专业版** | 完整传感器 + 算力 |

### 15.5 两版本 SDK 支持矩阵

| SDK / 工具 | 专业版 Pro | 探索版 Venture | 说明 |
|-----------|:---------:|:-------------:|------|
| MotionSDK（C++ 关节控制） | ✅ | ✅ | 两版均支持 |
| sdk_deploy（ROS2 DDS） | ✅ | ✅ | 两版均支持 |
| Lite3_ROS（感知桥接） | ✅ | ⚠️ 部分 | Venture 无传感器话题 |
| FAST-LIVO2 | ✅ | ❌ | 需 Livox + RealSense |
| rl_training | ✅ | ✅ | PC 端训练，不依赖机器人版本 |
| Lite3_rl_deploy | ✅ | ✅ | 部署到 ARM 主机 |
| gamepad | ✅ | ✅ | 两版均支持手柄 |
| deep_robotics_model | ✅ | ✅ | URDF/MJCF 仿真模型 |

---

## 第十六章 建图与自主导航完全指南

> **前置要求**：专业版 Lite3 Pro（含 Livox Mid-360 + RealSense D435i + Orin NX），或探索版外接等效传感器 + 计算平台。

### 16.1 建图与导航技术栈总览

```
┌─────────────────── SLAM 建图导航全栈架构 ─────────────────────┐
│                                                                │
│  传感器层:                                                      │
│  ┌──────────┐  ┌──────────┐  ┌──────────┐  ┌──────────┐      │
│  │Livox     │  │RealSense │  │ IMU      │  │腿部里程计 │      │
│  │Mid-360   │  │D435i     │  │          │  │(leg_odom)│      │
│  └────┬─────┘  └────┬─────┘  └────┬─────┘  └────┬─────┘      │
│       │              │             │              │             │
│  ┌────▼──────────────▼─────────────▼──────────────▼─────┐      │
│  │                SLAM 建图层                            │      │
│  │  FAST-LIVO2 (LiDAR-Inertial-Visual Odometry)        │      │
│  │  输出: 3D 点云地图 + 位姿 (Odometry)                  │      │
│  └────┬─────────────────────────────────────────────────┘      │
│       │                                                        │
│  ┌────▼─────────────────────────────────────────────────┐      │
│  │                地图处理层                              │      │
│  │  3D 点云 → 2D 栅格 (Occupancy Grid)                   │      │
│  │  或 3D 八叉树 (Octomap) / 高度图 (Elevation Map)      │      │
│  └────┬─────────────────────────────────────────────────┘      │
│       │                                                        │
│  ┌────▼─────────────────────────────────────────────────┐      │
│  │                Nav2 导航层                             │      │
│  │  ┌──────────┐ ┌──────────┐ ┌──────────────┐          │      │
│  │  │路径规划   │ │行为树    │ │代价地图      │          │      │
│  │  │(Planner) │ │(BT)     │ │(Costmap)     │          │      │
│  │  └──────────┘ └──────────┘ └──────────────┘          │      │
│  └────┬─────────────────────────────────────────────────┘      │
│       │ /cmd_vel                                               │
│  ┌────▼─────────────────────────────────────────────────┐      │
│  │                运动执行层                              │      │
│  │  Lite3_ROS → ARM 运动主机 → 关节控制                   │      │
│  └──────────────────────────────────────────────────────┘      │
└────────────────────────────────────────────────────────────────┘
```

### 16.2 环境准备

#### 16.2.1 Orin NX 基础环境配置

```bash
# SSH 到 Orin NX
ssh ysc@192.168.2.2

# 确认 ROS2 Humble 已安装
ros2 --version

# 安装必要依赖
sudo apt update
sudo apt install -y \
  ros-humble-pcl-ros \
  ros-humble-tf2-ros \
  ros-humble-tf2-geometry-msgs \
  ros-humble-nav2-bringup \
  ros-humble-navigation2 \
  ros-humble-slam-toolbox \
  ros-humble-robot-localization \
  ros-humble-octomap-server \
  python3-colcon-common-extensions

# 安装 Livox SDK2（如未预装）
git clone https://github.com/Livox-SDK/Livox-SDK2.git
cd Livox-SDK2 && mkdir build && cd build
cmake .. && make -j$(nproc) && sudo make install

# 安装 RealSense SDK（如未预装）
sudo apt install -y ros-humble-realsense2-camera ros-humble-realsense2-description
```

#### 16.2.2 工作空间创建

```bash
mkdir -p ~/lite3_nav_ws/src
cd ~/lite3_nav_ws/src

# 克隆所需包
git clone https://github.com/DeepRoboticsLab/Lite3_ROS.git
git clone https://github.com/DeepRoboticsLab/fast-livo2-deep-robotics.git

cd ~/lite3_nav_ws
colcon build --symlink-install
source install/setup.bash

# 加入 .bashrc
echo "source ~/lite3_nav_ws/install/setup.bash" >> ~/.bashrc
```

### 16.3 FAST-LIVO2 建图详解

FAST-LIVO2 是一个高性能的 **LiDAR-惯性-视觉融合里程计**（紧耦合），可以实时生成高精度 3D 点云地图。

#### 16.3.1 FAST-LIVO2 核心原理

```
传感器数据流:

Livox Mid-360 ─(点云)──┐
                         │
IMU ──────────(加速度)──┼──→ FAST-LIVO2 ──→ 位姿 T_w_b
                         │                    ──→ 3D 稠密点云地图
RealSense D435i─(图像)──┘                    ──→ 着色点云

融合方式:
1. LiDAR 点云注册到全局坐标系
2. IMU 预积分提供运动先验
3. 视觉特征辅助退化场景（长走廊、空旷场地）
4. 自适应融合权重，根据场景选择传感器信任度
```

#### 16.3.2 修改配置文件

```yaml
# ~/lite3_nav_ws/src/fast-livo2-deep-robotics/config/lite3_config.yaml
# 根据实际场景调整以下参数:

common:
  lidar_topic: "/livox/lidar"          # Livox Mid-360 话题
  imu_topic: "/livox/imu"              # IMU 话题（Livox 内置）
  camera_topic: "/camera/color/image_raw"  # RealSense 彩色图像
  
  # 点云滤波参数
  point_filter_num: 3                   # 每 N 个点取 1 个（降采样）
  
  # 建图参数
  scan_line: 6                          # Livox Mid-360 扫描线数
  mapping_max_iteration: 3              # ICP 最大迭代次数
  filter_size_surf: 0.3                 # 平面特征降采样（米）
  filter_size_map: 0.3                  # 地图体素滤波（米）
  
  # 退化检测
  degeneration_threshold: 100.0         # 退化阈值
```

#### 16.3.3 实时建图操作流程

```bash
# 终端1: 启动 Lite3_ROS 桥接
ros2 launch lite3_bringup lite3_bringup.launch.py

# 终端2: 启动传感器
ros2 launch livox_ros2_driver livox_lidar_msg.launch.py
ros2 launch realsense2_camera rs_launch.py depth_module.profile:=640x480x30

# 终端3: 启动 FAST-LIVO2 建图
ros2 launch fast_livo2 mapping.launch.py \
  config_file:=$HOME/lite3_nav_ws/src/fast-livo2-deep-robotics/config/lite3_config.yaml

# 终端4: 可视化（在开发机远程查看）
# 在开发机上：
rviz2 -d ~/lite3_nav_ws/src/fast-livo2-deep-robotics/rviz/mapping.rviz
```

**建图操作步骤**：

1. **启动后静止 3~5 秒**：让 IMU 完成初始化
2. **用手柄遥控机器人缓慢行走**：推荐速度 0.3~0.5 m/s
3. **覆盖全部目标区域**：确保无死角，回环闭合效果更好
4. **建图完成后保存**：
   ```bash
   # 保存 PCD 点云地图
   ros2 service call /fast_livo2/save_map std_srvs/srv/Trigger
   # 地图将保存在 ~/.ros/fast_livo2_map.pcd
   ```

#### 16.3.4 建图质量优化技巧

| 问题 | 原因 | 解决方案 |
|------|------|---------|
| 点云重影/拖影 | 帧率不足或运动过快 | 降低行走速度 ≤ 0.3 m/s，增大 mapping_max_iteration |
| 长走廊漂移 | LiDAR 几何退化 | 确保融合视觉；走廊中尽量 S 形路线 |
| 室外光照变化 | 视觉特征不稳定 | 调低视觉权重 或仅用 LiDAR+IMU 模式 |
| 地图稀疏 | 降采样过多 | 减小 point_filter_num (如 1 或 2) |
| 建图中断 | Orin NX 过热 | 加装散热风扇，降低帧率 |

### 16.4 3D 点云地图转 2D 栅格地图

Nav2 需要 **2D 占据栅格地图（Occupancy Grid Map）**。需将 FAST-LIVO2 输出的 3D 点云转换为 2D 地图。

#### 16.4.1 使用 pcl_tools 转换

```python
#!/usr/bin/env python3
"""
pcd_to_2d_map.py
将 3D PCD 点云地图转换为 2D 栅格地图（.pgm + .yaml）
"""

import numpy as np
import open3d as o3d
from PIL import Image
import yaml
import sys

def pcd_to_occupancy_grid(pcd_file, output_prefix, resolution=0.05, 
                           height_min=0.1, height_max=1.5):
    """
    参数:
        pcd_file: 输入 PCD 文件路径
        output_prefix: 输出文件前缀（生成 .pgm 和 .yaml）
        resolution: 栅格分辨率（米/像素），默认 5cm
        height_min: 最低高度过滤（米），过滤地面
        height_max: 最高高度过滤（米），过滤天花板
    """
    # 1. 读取点云
    print(f"读取点云: {pcd_file}")
    pcd = o3d.io.read_point_cloud(pcd_file)
    points = np.asarray(pcd.points)
    print(f"总点数: {len(points)}")
    
    # 2. 高度过滤（保留 height_min < z < height_max 的点作为障碍物）
    mask = (points[:, 2] > height_min) & (points[:, 2] < height_max)
    obstacle_points = points[mask]
    print(f"障碍物点数: {len(obstacle_points)}")
    
    # 3. 计算地图边界
    x_min, y_min = obstacle_points[:, 0].min(), obstacle_points[:, 1].min()
    x_max, y_max = obstacle_points[:, 0].max(), obstacle_points[:, 1].max()
    
    # 增加边距
    margin = 2.0  # 米
    x_min -= margin
    y_min -= margin
    x_max += margin
    y_max += margin
    
    # 4. 创建栅格
    width = int((x_max - x_min) / resolution)
    height = int((y_max - y_min) / resolution)
    grid = np.ones((height, width), dtype=np.uint8) * 205  # 205 = 未知区域
    
    # 5. 标记自由空间（所有原始点的 XY 投影区域为自由空间）
    all_x = ((points[:, 0] - x_min) / resolution).astype(int)
    all_y = ((points[:, 1] - y_min) / resolution).astype(int)
    valid = (all_x >= 0) & (all_x < width) & (all_y >= 0) & (all_y < height)
    grid[all_y[valid], all_x[valid]] = 254  # 254 = 自由空间
    
    # 6. 标记障碍物
    obs_x = ((obstacle_points[:, 0] - x_min) / resolution).astype(int)
    obs_y = ((obstacle_points[:, 1] - y_min) / resolution).astype(int)
    valid_obs = (obs_x >= 0) & (obs_x < width) & (obs_y >= 0) & (obs_y < height)
    grid[obs_y[valid_obs], obs_x[valid_obs]] = 0  # 0 = 障碍物
    
    # 7. 上下翻转（PGM 原点在左下角）
    grid = np.flipud(grid)
    
    # 8. 保存 PGM 图像
    img = Image.fromarray(grid)
    pgm_file = f"{output_prefix}.pgm"
    img.save(pgm_file)
    print(f"保存栅格地图: {pgm_file} ({width}x{height} 像素)")
    
    # 9. 生成 YAML 配置
    yaml_data = {
        'image': pgm_file,
        'resolution': resolution,
        'origin': [x_min, y_min, 0.0],
        'negate': 0,
        'occupied_thresh': 0.65,
        'free_thresh': 0.196,
    }
    yaml_file = f"{output_prefix}.yaml"
    with open(yaml_file, 'w') as f:
        yaml.dump(yaml_data, f, default_flow_style=False)
    print(f"保存地图配置: {yaml_file}")
    
    return pgm_file, yaml_file

if __name__ == "__main__":
    if len(sys.argv) < 2:
        print("用法: python3 pcd_to_2d_map.py <input.pcd> [output_prefix] [resolution]")
        sys.exit(1)
    
    pcd_file = sys.argv[1]
    output_prefix = sys.argv[2] if len(sys.argv) > 2 else "map"
    resolution = float(sys.argv[3]) if len(sys.argv) > 3 else 0.05
    
    pcd_to_occupancy_grid(pcd_file, output_prefix, resolution)
```

```bash
# 安装依赖
pip3 install open3d numpy pillow pyyaml

# 运行转换
python3 pcd_to_2d_map.py ~/.ros/fast_livo2_map.pcd my_map 0.05
# 输出: my_map.pgm + my_map.yaml
```

### 16.5 Nav2 自主导航配置

#### 16.5.1 Nav2 核心组件

```
┌─────────────── Nav2 导航栈架构 ───────────────┐
│                                                │
│  ┌──────────────┐      ┌──────────────┐       │
│  │ BT Navigator │ ←──→ │ 目标服务器   │       │
│  │ (行为树)      │      │ (Goal Pose)  │       │
│  └──────┬───────┘      └──────────────┘       │
│         │                                      │
│  ┌──────▼───────┐      ┌──────────────┐       │
│  │ Planner      │      │ Controller   │       │
│  │ (全局路径规划) │ ──→ │ (局部轨迹跟踪)│       │
│  │ NavFn/Theta* │      │ DWB/MPPI     │       │
│  └──────┬───────┘      └──────┬───────┘       │
│         │                      │               │
│  ┌──────▼──────────────────────▼───────┐       │
│  │        Costmap 2D (代价地图)         │       │
│  │  ┌──────────┐  ┌──────────────────┐ │       │
│  │  │静态层     │  │ 障碍物层          │ │       │
│  │  │(Static)  │  │ (Obstacle/Voxel) │ │       │
│  │  └──────────┘  └──────────────────┘ │       │
│  │  ┌──────────┐  ┌──────────────────┐ │       │
│  │  │ 膨胀层    │  │ 速度调整层       │ │       │
│  │  │(Inflate) │  │ (Speed Filter)   │ │       │
│  │  └──────────┘  └──────────────────┘ │       │
│  └─────────────────────────────────────┘       │
│                     │                          │
│              ┌──────▼───────┐                  │
│              │  /cmd_vel    │                  │
│              │  速度指令输出  │                  │
│              └──────────────┘                  │
└────────────────────────────────────────────────┘
```

#### 16.5.2 Nav2 参数配置文件

```yaml
# ~/lite3_nav_ws/src/lite3_nav/config/nav2_params.yaml

amcl:
  ros__parameters:
    use_sim_time: false
    alpha1: 0.2          # 旋转→旋转 噪声
    alpha2: 0.2          # 旋转→平移 噪声
    alpha3: 0.2          # 平移→平移 噪声
    alpha4: 0.2          # 平移→旋转 噪声
    base_frame_id: "base_link"
    global_frame_id: "map"
    odom_frame_id: "odom"
    scan_topic: "/scan"   # 2D激光扫描话题
    max_particles: 2000
    min_particles: 500
    robot_model_type: "nav2_amcl::DifferentialMotionModel"
    tf_broadcast: true

bt_navigator:
  ros__parameters:
    use_sim_time: false
    global_frame: map
    robot_base_frame: base_link
    odom_topic: /leg_odom    # 使用 Lite3 腿部里程计
    default_bt_xml_filename: "navigate_w_replanning_and_recovery.xml"
    plugin_lib_names:
      - nav2_compute_path_to_pose_action_bt_node
      - nav2_follow_path_action_bt_node
      - nav2_back_up_action_bt_node
      - nav2_spin_action_bt_node
      - nav2_wait_action_bt_node
      - nav2_clear_costmap_service_bt_node
      - nav2_is_stuck_condition_bt_node
      - nav2_goal_reached_condition_bt_node
      - nav2_rate_controller_bt_node
      - nav2_recovery_node_bt_node
      - nav2_pipeline_sequence_bt_node

controller_server:
  ros__parameters:
    use_sim_time: false
    controller_frequency: 20.0     # 控制频率 20Hz
    min_x_velocity_threshold: 0.001
    min_y_velocity_threshold: 0.001
    min_theta_velocity_threshold: 0.001
    progress_checker_plugin: "progress_checker"
    goal_checker_plugins: ["goal_checker"]
    controller_plugins: ["FollowPath"]
    
    progress_checker:
      plugin: "nav2_controller::SimpleProgressChecker"
      required_movement_radius: 0.5
      movement_time_allowance: 10.0
    
    goal_checker:
      plugin: "nav2_controller::SimpleGoalChecker"
      xy_goal_tolerance: 0.25       # 位置容差 25cm
      yaw_goal_tolerance: 0.25      # 角度容差
      stateful: true
    
    FollowPath:
      plugin: "dwb_core::DWBLocalPlanner"
      min_vel_x: 0.0
      max_vel_x: 0.5                # 最大前进速度 0.5 m/s
      min_vel_y: 0.0
      max_vel_y: 0.3                # 最大侧移速度 0.3 m/s（Lite3 支持全向）
      max_vel_theta: 0.8            # 最大旋转速度 0.8 rad/s
      min_speed_xy: 0.0
      max_speed_xy: 0.5
      min_speed_theta: 0.0
      acc_lim_x: 1.0
      acc_lim_y: 0.5
      acc_lim_theta: 1.5
      decel_lim_x: -1.0
      decel_lim_y: -0.5
      decel_lim_theta: -1.5
      vx_samples: 20
      vy_samples: 10
      vtheta_samples: 20
      sim_time: 1.5
      transform_tolerance: 0.2
      xy_goal_tolerance: 0.25
      critics:
        - RotateToGoal
        - Oscillation
        - ObstacleFootprint
        - GoalAlign
        - PathAlign
        - PathDist
        - GoalDist

planner_server:
  ros__parameters:
    use_sim_time: false
    planner_plugins: ["GridBased"]
    GridBased:
      plugin: "nav2_navfn_planner/NavfnPlanner"
      tolerance: 0.5
      use_astar: true              # A* 搜索
      allow_unknown: true

local_costmap:
  local_costmap:
    ros__parameters:
      update_frequency: 5.0
      publish_frequency: 2.0
      global_frame: odom
      robot_base_frame: base_link
      use_sim_time: false
      rolling_window: true
      width: 3                       # 局部地图 3m x 3m
      height: 3
      resolution: 0.05              # 5cm 分辨率
      robot_radius: 0.3             # Lite3 机器人半径约 30cm
      plugins: ["obstacle_layer", "inflation_layer"]
      obstacle_layer:
        plugin: "nav2_costmap_2d::ObstacleLayer"
        enabled: true
        observation_sources: scan
        scan:
          topic: /scan
          max_obstacle_height: 2.0
          clearing: true
          marking: true
      inflation_layer:
        plugin: "nav2_costmap_2d::InflationLayer"
        cost_scaling_factor: 3.0
        inflation_radius: 0.55       # 膨胀半径 = 机器人半径 + 安全余量

global_costmap:
  global_costmap:
    ros__parameters:
      update_frequency: 1.0
      publish_frequency: 0.5
      global_frame: map
      robot_base_frame: base_link
      use_sim_time: false
      robot_radius: 0.3
      resolution: 0.05
      track_unknown_space: true
      plugins: ["static_layer", "obstacle_layer", "inflation_layer"]
      static_layer:
        plugin: "nav2_costmap_2d::StaticLayer"
        map_subscribe_transient_local: true
      obstacle_layer:
        plugin: "nav2_costmap_2d::ObstacleLayer"
        enabled: true
        observation_sources: scan
        scan:
          topic: /scan
          max_obstacle_height: 2.0
          clearing: true
          marking: true
      inflation_layer:
        plugin: "nav2_costmap_2d::InflationLayer"
        cost_scaling_factor: 3.0
        inflation_radius: 0.55

recoveries_server:
  ros__parameters:
    costmap_topic: local_costmap/costmap_raw
    footprint_topic: local_costmap/published_footprint
    cycle_frequency: 10.0
    recovery_plugins: ["spin", "backup", "wait"]
    spin:
      plugin: "nav2_recoveries/Spin"
    backup:
      plugin: "nav2_recoveries/BackUp"
    wait:
      plugin: "nav2_recoveries/Wait"

map_server:
  ros__parameters:
    use_sim_time: false
    yaml_filename: "/home/ysc/maps/my_map.yaml"  # 你的地图文件
```

#### 16.5.3 3D LiDAR 转 2D LaserScan

Nav2 的障碍物层默认使用 2D LaserScan，需要将 Livox Mid-360 的 3D 点云转为 2D：

```bash
# 安装 pointcloud_to_laserscan
sudo apt install ros-humble-pointcloud-to-laserscan
```

```python
# ~/lite3_nav_ws/src/lite3_nav/launch/pointcloud_to_scan.launch.py
from launch import LaunchDescription
from launch_ros.actions import Node

def generate_launch_description():
    return LaunchDescription([
        Node(
            package='pointcloud_to_laserscan',
            executable='pointcloud_to_laserscan_node',
            name='pointcloud_to_laserscan',
            parameters=[{
                'target_frame': 'base_link',
                'transform_tolerance': 0.01,
                'min_height': 0.1,          # 最低高度（过滤地面）
                'max_height': 1.0,          # 最高高度
                'angle_min': -3.14159,      # -180度
                'angle_max': 3.14159,       # +180度
                'angle_increment': 0.00872, # 0.5度分辨率
                'scan_time': 0.1,
                'range_min': 0.3,           # 最近距离 0.3m
                'range_max': 50.0,          # 最远距离 50m
                'use_inf': True,
                'inf_epsilon': 1.0,
            }],
            remappings=[
                ('cloud_in', '/livox/lidar'),  # 输入: Livox 点云
                ('scan', '/scan'),              # 输出: 2D 扫描
            ],
        ),
    ])
```

#### 16.5.4 完整导航启动流程

```bash
# ========== 在 Orin NX 上 ==========

# 终端 1: Lite3 桥接
ros2 launch lite3_bringup lite3_bringup.launch.py

# 终端 2: 传感器启动
ros2 launch livox_ros2_driver livox_lidar_msg.launch.py

# 终端 3: 点云转 2D 扫描
ros2 launch lite3_nav pointcloud_to_scan.launch.py

# 终端 4: 地图服务器
ros2 launch nav2_map_server map_server.launch.py \
  map:=/home/ysc/maps/my_map.yaml

# 终端 5: AMCL 定位
ros2 launch nav2_amcl amcl.launch.py \
  params_file:=/home/ysc/lite3_nav_ws/src/lite3_nav/config/nav2_params.yaml

# 终端 6: Nav2 导航
ros2 launch nav2_bringup navigation_launch.py \
  params_file:=/home/ysc/lite3_nav_ws/src/lite3_nav/config/nav2_params.yaml

# ========== 在开发机上（远程可视化） ==========
# 终端 7: RViz2 可视化 + 目标点发布
rviz2 -d ~/lite3_nav_ws/src/lite3_nav/rviz/navigation.rviz
# 在 RViz2 中使用 "2D Pose Estimate" 设置初始位姿
# 使用 "Nav2 Goal" 发送目标点
```

#### 16.5.5 编程发送导航目标

```python
#!/usr/bin/env python3
"""
nav_to_goal.py
通过代码发送导航目标点
"""
import rclpy
from rclpy.node import Node
from geometry_msgs.msg import PoseStamped
from nav2_simple_commander.robot_navigator import BasicNavigator, TaskResult
import math

def main():
    rclpy.init()
    navigator = BasicNavigator()
    
    # 等待 Nav2 就绪
    navigator.waitUntilNav2Active()
    
    # === 1. 设置初始位姿（如果 AMCL 需要） ===
    initial_pose = PoseStamped()
    initial_pose.header.frame_id = 'map'
    initial_pose.header.stamp = navigator.get_clock().now().to_msg()
    initial_pose.pose.position.x = 0.0
    initial_pose.pose.position.y = 0.0
    initial_pose.pose.orientation.w = 1.0
    navigator.setInitialPose(initial_pose)
    
    # === 2. 发送单个目标 ===
    goal_pose = PoseStamped()
    goal_pose.header.frame_id = 'map'
    goal_pose.header.stamp = navigator.get_clock().now().to_msg()
    goal_pose.pose.position.x = 3.0   # 目标 X = 3 米
    goal_pose.pose.position.y = 2.0   # 目标 Y = 2 米
    # 朝向角 90 度
    goal_pose.pose.orientation.z = math.sin(math.pi / 4)
    goal_pose.pose.orientation.w = math.cos(math.pi / 4)
    
    navigator.goToPose(goal_pose)
    
    # 等待导航完成
    while not navigator.isTaskComplete():
        feedback = navigator.getFeedback()
        if feedback:
            print(f"剩余距离: {feedback.distance_remaining:.2f} m")
    
    result = navigator.getResult()
    if result == TaskResult.SUCCEEDED:
        print("✅ 导航成功!")
    elif result == TaskResult.CANCELED:
        print("⚠️ 导航被取消")
    elif result == TaskResult.FAILED:
        print("❌ 导航失败")
    
    # === 3. 多点巡航 ===
    waypoints = []
    for (x, y, yaw) in [(1.0, 0.0, 0.0), (2.0, 1.0, 1.57), (0.0, 2.0, 3.14), (0.0, 0.0, 0.0)]:
        wp = PoseStamped()
        wp.header.frame_id = 'map'
        wp.header.stamp = navigator.get_clock().now().to_msg()
        wp.pose.position.x = x
        wp.pose.position.y = y
        wp.pose.orientation.z = math.sin(yaw / 2)
        wp.pose.orientation.w = math.cos(yaw / 2)
        waypoints.append(wp)
    
    navigator.followWaypoints(waypoints)
    
    while not navigator.isTaskComplete():
        feedback = navigator.getFeedback()
        if feedback:
            print(f"当前航点: {feedback.current_waypoint + 1}/{len(waypoints)}")
    
    print("✅ 巡航完成!")
    navigator.lifecycleShutdown()
    rclpy.shutdown()

if __name__ == '__main__':
    main()
```

### 16.6 实时避障与动态环境应对

#### 16.6.1 使用 3D 障碍物层（Voxel Layer）

对于动态障碍物（行人、移动物体），推荐使用 Voxel Layer 替代 Obstacle Layer：

```yaml
# 局部代价地图中替换 obstacle_layer 为:
local_costmap:
  local_costmap:
    ros__parameters:
      plugins: ["voxel_layer", "inflation_layer"]
      voxel_layer:
        plugin: "nav2_costmap_2d::VoxelLayer"
        enabled: true
        publish_voxel_map: true
        origin_z: 0.0
        z_resolution: 0.05
        z_voxels: 16
        max_obstacle_height: 2.0
        mark_threshold: 0
        observation_sources: pointcloud
        pointcloud:
          topic: /livox/lidar       # 直接使用 3D 点云
          max_obstacle_height: 2.0
          min_obstacle_height: 0.1
          obstacle_max_range: 5.0
          obstacle_min_range: 0.3
          clearing: true
          marking: true
          data_type: "PointCloud2"
```

#### 16.6.2 速度限制区域

```yaml
# 在特定区域限速（如窄通道、楼梯附近）
local_costmap:
  local_costmap:
    ros__parameters:
      plugins: ["static_layer", "obstacle_layer", "speed_filter", "inflation_layer"]
      speed_filter:
        plugin: "nav2_costmap_2d::SpeedFilter"
        enabled: true
        filter_info_topic: "/speed_filter_info"
        speed_limit_topic: "/speed_limit"
```

### 16.7 官方训练案例参考

> **务必参考**：`Robot_Training_Cases` 仓库中的 **Case8** 和 **Case9** 是官方 SLAM 建图与导航教程。
>
> - **Case8: SLAM 建图导航（上篇）** — 传感器配置、FAST-LIVO2 建图流程、地图保存
> - **Case9: SLAM 建图导航（下篇）** — Nav2 导航配置、路径规划、动态避障
>
> 获取方式：
> ```bash
> git clone https://github.com/DeepRoboticsLab/Robot_Training_Cases.git
> cd Robot_Training_Cases/Case8_SLAM_Mapping
> cd Robot_Training_Cases/Case9_SLAM_Navigation
> ```

---

## 第十七章 高难度动作强化学习训练实战

> 本章在第十一章基础上深入，专注于训练**高难度特技动作**：后空翻、跳跃、上楼梯、倒地恢复等。

### 17.1 高难度动作训练理论基础

#### 17.1.1 为什么普通 RL 训练不了高难度动作？

| 挑战 | 原因 | 解决方案 |
|------|------|---------|
| **稀疏奖励** | 后空翻要完成整个动作才有正奖励，训练初期几乎收不到 | 课程学习（Curriculum）、奖励塑形（Reward Shaping） |
| **动作空间爆炸** | 12 关节同时高频运动，组合极多 | 分层控制、动作先验、参考运动 |
| **安全约束** | 真机测试危险（摔倒、损坏） | 仿真中充分验证、Domain Randomization |
| **Sim-to-Real Gap** | 仿真转真机效果大幅下降 | 域随机化、系统辨识、对抗训练 |

#### 17.1.2 训练方法学图谱

```
高难度动作训练方法:
│
├── 📚 参考运动模仿 (Reference Motion Imitation)
│   ├── AMP (Adversarial Motion Priors) ← 推荐
│   ├── DeepMimic
│   └── Motion Tracking Reward
│
├── 📈 课程学习 (Curriculum Learning)
│   ├── 难度递增 (Progressive Difficulty)
│   ├── 领域递增 (Progressive Domain)
│   └── 奖励递增 (Progressive Reward)
│
├── 🎯 奖励工程 (Reward Engineering)
│   ├── 稠密奖励设计
│   ├── 阶段性奖励
│   └── 构造性奖励 (shaped reward)
│
└── 🔄 域随机化 (Domain Randomization)
    ├── 物理参数随机化（质量、摩擦、关节阻尼）
    ├── 视觉随机化（纹理、光照）
    └── 外部扰动随机化（推力、不平地形）
```

### 17.2 Isaac Lab 环境定制

#### 17.2.1 自定义 Lite3 训练环境

```python
# ~/rl_training/envs/lite3_acrobatic_env.py
"""
Lite3 高难度动作训练环境
基于 Isaac Lab (Orbit) 框架
"""
import torch
from omni.isaac.lab.envs import ManagerBasedRLEnv
from omni.isaac.lab.managers import (
    ObservationManager,
    RewardManager,
    TerminationManager,
    CurriculumManager,
)
from omni.isaac.lab.assets import Articulation
import omni.isaac.lab.utils.math as math_utils


class Lite3AcrobaticEnvCfg:
    """高难度动作环境配置"""
    
    # ===== 仿真参数 =====
    sim_dt = 0.002          # 仿真步长 2ms (500Hz)
    control_dt = 0.02       # 控制步长 20ms (50Hz)
    num_envs = 4096         # 并行环境数
    episode_length = 500    # 一回合最大步数 (= 10秒)
    
    # ===== 机器人参数 =====
    robot_cfg = {
        "asset_path": "deep_robotics_model/lite3/urdf/lite3.urdf",
        "num_joints": 12,
        "default_joint_pos": {  # 站立姿态关节角度 (rad)
            "FL_HipX": 0.0, "FL_HipY": 0.67, "FL_Knee": -1.3,
            "FR_HipX": 0.0, "FR_HipY": 0.67, "FR_Knee": -1.3,
            "HL_HipX": 0.0, "HL_HipY": 0.67, "HL_Knee": -1.3,
            "HR_HipX": 0.0, "HR_HipY": 0.67, "HR_Knee": -1.3,
        },
    }
    
    # ===== 域随机化 =====
    domain_randomization = {
        "mass_range": [0.8, 1.2],           # 质量 ±20%
        "friction_range": [0.5, 1.5],        # 摩擦系数
        "joint_damping_range": [0.8, 1.2],   # 关节阻尼
        "Kp_range": [0.9, 1.1],             # 关节刚度
        "Kd_range": [0.9, 1.1],             # 关节阻尼系数
        "push_force_range": [-30.0, 30.0],   # 随机推力 (N)
        "push_interval": [5.0, 15.0],        # 推力间隔 (秒)
        "motor_strength_range": [0.9, 1.1],  # 电机力矩 ±10%
        "gravity_range": [-0.2, 0.2],        # 重力偏差 (m/s^2)
    }


# ===== 后空翻 (Backflip) 奖励函数 =====
class BackflipReward:
    """
    后空翻奖励分为 4 个阶段:
    1. 蓄力阶段 (crouch)：降低重心
    2. 起跳阶段 (launch)：向上加速
    3. 翻转阶段 (flip)：绕 Y 轴旋转 360°
    4. 着陆阶段 (land)：四脚着地，速度接近 0
    """
    
    def __init__(self, env):
        self.env = env
        self.phase_timer = torch.zeros(env.num_envs, device=env.device)
        
    def compute(self, obs, actions):
        base_pos = obs["base_position"]       # (N, 3)
        base_quat = obs["base_orientation"]   # (N, 4)
        base_vel = obs["base_linear_vel"]     # (N, 3)
        base_ang_vel = obs["base_angular_vel"] # (N, 3)
        joint_pos = obs["joint_positions"]    # (N, 12)
        foot_contact = obs["foot_contact"]    # (N, 4)
        
        # 获取 pitch 角（绕 Y 轴旋转）
        roll, pitch, yaw = math_utils.euler_xyz_from_quat(base_quat)
        
        reward = torch.zeros(self.env.num_envs, device=self.env.device)
        
        # ---------- 阶段 1: 蓄力 (0 ~ 0.3s) ----------
        crouch_mask = self.phase_timer < 15  # 15 steps * 20ms = 0.3s
        # 奖励降低重心
        target_height = 0.15  # 蓄力时重心降至 0.15m
        height_reward = -torch.abs(base_pos[:, 2] - target_height)
        reward += crouch_mask.float() * height_reward * 2.0
        
        # ---------- 阶段 2: 起跳 (0.3 ~ 0.6s) ----------
        launch_mask = (self.phase_timer >= 15) & (self.phase_timer < 30)
        # 奖励向上速度
        upward_vel_reward = torch.clamp(base_vel[:, 2], 0.0, 5.0)
        reward += launch_mask.float() * upward_vel_reward * 3.0
        
        # ---------- 阶段 3: 翻转 (0.6 ~ 1.5s) ----------
        flip_mask = (self.phase_timer >= 30) & (self.phase_timer < 75)
        # 奖励向后翻转角速度（绕 Y 轴负方向）
        backflip_vel = -base_ang_vel[:, 1]  # 负 Y 轴角速度
        flip_reward = torch.clamp(backflip_vel, 0.0, 15.0)
        reward += flip_mask.float() * flip_reward * 2.0
        # 奖励翻转角度进度
        flip_progress = torch.clamp(-pitch / (2 * 3.14159), 0.0, 1.0)
        reward += flip_mask.float() * flip_progress * 5.0
        
        # ---------- 阶段 4: 着陆 (1.5s+) ----------
        land_mask = self.phase_timer >= 75
        # 奖励四脚着地
        all_feet_contact = (foot_contact.sum(dim=1) >= 3).float()
        reward += land_mask.float() * all_feet_contact * 10.0
        # 奖励站稳（低速度 + 水平姿态）
        stable_reward = -torch.abs(base_vel[:, :2]).sum(dim=1) - torch.abs(roll) - torch.abs(pitch)
        reward += land_mask.float() * stable_reward * 2.0
        
        # ---------- 通用惩罚 ----------
        # 关节力矩惩罚（鼓励用更少力矩完成动作）
        torque_penalty = -torch.sum(actions ** 2, dim=1) * 0.001
        reward += torque_penalty
        
        # 关节速度惩罚
        joint_vel = obs.get("joint_velocities", torch.zeros_like(joint_pos))
        joint_vel_penalty = -torch.sum(joint_vel ** 2, dim=1) * 0.0001
        reward += joint_vel_penalty
        
        self.phase_timer += 1
        return reward
    
    def reset(self, env_ids):
        self.phase_timer[env_ids] = 0


# ===== 跳跃 (Jump) 奖励函数 =====
class JumpReward:
    """原地跳跃奖励"""
    
    def compute(self, obs, actions):
        base_pos = obs["base_position"]
        base_vel = obs["base_linear_vel"]
        foot_contact = obs["foot_contact"]
        roll, pitch, yaw = math_utils.euler_xyz_from_quat(obs["base_orientation"])
        
        reward = torch.zeros(obs["base_position"].shape[0], device=obs["base_position"].device)
        
        # 奖励跳跃高度
        jump_height = torch.clamp(base_pos[:, 2] - 0.32, 0.0, 1.0)  # 0.32m 是站立高度
        reward += jump_height * 20.0
        
        # 奖励水平姿态（在空中保持平衡）
        reward -= torch.abs(roll) * 2.0
        reward -= torch.abs(pitch) * 2.0
        
        # 奖励垂直跳跃（抑制水平漂移）
        reward -= torch.abs(base_vel[:, 0]) * 1.0
        reward -= torch.abs(base_vel[:, 1]) * 1.0
        
        # 着陆奖励
        landing = (foot_contact.sum(dim=1) >= 4).float()
        low_vel = (torch.norm(base_vel, dim=1) < 0.5).float()
        reward += landing * low_vel * 5.0
        
        return reward


# ===== 上楼梯 (Stair Climbing) 奖励函数 =====
class StairClimbReward:
    """楼梯攀爬奖励"""
    
    def compute(self, obs, actions, target_vel_x=0.3):
        base_pos = obs["base_position"]
        base_vel = obs["base_linear_vel"]
        roll, pitch, yaw = math_utils.euler_xyz_from_quat(obs["base_orientation"])
        foot_contact = obs["foot_contact"]
        
        reward = torch.zeros(base_pos.shape[0], device=base_pos.device)
        
        # 奖励前进（X 方向）
        forward_reward = torch.clamp(base_vel[:, 0], -0.5, target_vel_x)
        reward += forward_reward * 5.0
        
        # 奖励爬升（Z 方向）
        climb_reward = torch.clamp(base_vel[:, 2], 0.0, 0.5)
        reward += climb_reward * 10.0
        
        # 姿态稳定惩罚
        reward -= torch.abs(roll) * 3.0
        reward -= (torch.abs(pitch) - 0.2).clamp(min=0) * 3.0  # 允许 ±0.2 rad 前倾
        
        # 侧移惩罚
        reward -= torch.abs(base_vel[:, 1]) * 2.0
        
        # 足端接触奖励（至少 2 只脚着地）
        enough_contact = (foot_contact.sum(dim=1) >= 2).float()
        reward += enough_contact * 1.0
        
        # 存活奖励
        reward += 0.5
        
        return reward
```

#### 17.2.2 课程学习配置

课程学习（Curriculum Learning）是训练高难度动作的核心策略——从简单任务逐步升级到困难任务。

```python
# ~/rl_training/curriculum/lite3_curriculum.py
"""
课程学习调度器
"""

class AcrobaticCurriculum:
    """高难度动作课程学习"""
    
    def __init__(self, num_envs, device):
        self.num_envs = num_envs
        self.device = device
        self.difficulty = 0.0  # 0 ~ 1
        self.success_buffer = []
        self.update_interval = 100  # 每 100 episodes 更新一次
        
    def get_curriculum_params(self):
        """根据当前难度等级返回环境参数"""
        d = self.difficulty
        
        return {
            # ===== 后空翻课程 =====
            "backflip": {
                # Level 0: 辅助力 (像蹦床一样给额外向上力)
                "assist_force_z": max(0, 200.0 * (1 - d * 2)),  # 前半段有辅助力，后半段取消
                # Level 0.3+: 从半空中开始（降低翻转难度）
                "initial_height": 0.32 + max(0, 0.3 * (1 - d * 3)),  # 逐渐降到正常站立高度
                # Level 0.5+: 加入域随机化
                "mass_range": [1.0 - 0.2 * d, 1.0 + 0.2 * d],
                "friction_range": [1.0 - 0.5 * d, 1.0 + 0.5 * d],
                # Level 0.7+: 加入外部扰动
                "push_force": 30.0 * max(0, d - 0.7) / 0.3,
            },
            
            # ===== 跳跃课程 =====
            "jump": {
                # 目标跳跃高度逐步提高
                "target_height": 0.05 + 0.35 * d,  # 5cm → 40cm
                # 低重力辅助（逐步恢复正常重力）
                "gravity_scale": 0.7 + 0.3 * d,    # 70% → 100% 正常重力
            },
            
            # ===== 楼梯课程 =====
            "stair_climb": {
                # 台阶高度逐步增加
                "stair_height": 0.03 + 0.17 * d,   # 3cm → 20cm
                # 台阶宽度逐步减小
                "stair_depth": 0.4 - 0.15 * d,     # 40cm → 25cm
                # 坡度
                "stair_count": int(3 + 7 * d),      # 3 → 10 阶
            },
        }
    
    def update(self, episode_rewards, success_flags):
        """根据训练成功率更新难度"""
        self.success_buffer.extend(success_flags.cpu().tolist())
        
        if len(self.success_buffer) >= self.update_interval:
            success_rate = sum(self.success_buffer[-self.update_interval:]) / self.update_interval
            
            # 成功率 > 80% → 提升难度
            if success_rate > 0.8:
                self.difficulty = min(1.0, self.difficulty + 0.05)
                print(f"📈 难度提升: {self.difficulty:.2f} (成功率: {success_rate:.1%})")
            # 成功率 < 30% → 降低难度
            elif success_rate < 0.3:
                self.difficulty = max(0.0, self.difficulty - 0.02)
                print(f"📉 难度降低: {self.difficulty:.2f} (成功率: {success_rate:.1%})")
            else:
                print(f"📊 难度保持: {self.difficulty:.2f} (成功率: {success_rate:.1%})")
            
            self.success_buffer = self.success_buffer[-self.update_interval:]
```

### 17.3 训练高难度动作实战

#### 17.3.1 后空翻训练

```bash
# Step 1: 基础后空翻训练（带辅助力）
cd ~/rl_training
python scripts/train.py \
  --task Backflip-Deeprobotics-Lite3-v0 \
  --num_envs 4096 \
  --max_iterations 10000 \
  --curriculum \
  --seed 42

# Step 2: 去除辅助力，Fine-tuning
python scripts/train.py \
  --task Backflip-Deeprobotics-Lite3-v0 \
  --num_envs 4096 \
  --max_iterations 5000 \
  --resume /path/to/backflip_checkpoint.pt \
  --curriculum_start 0.5 \
  --seed 42

# Step 3: 在仿真中验证
python scripts/play.py \
  --task Backflip-Deeprobotics-Lite3-v0 \
  --checkpoint /path/to/best_model.pt \
  --num_envs 16 \
  --render
```

#### 17.3.2 参考运动模仿 (AMP) 训练

AMP（Adversarial Motion Priors）是训练自然动作的最佳方法之一：用判别器区分"真实参考动作"和"策略生成动作"，策略通过对抗训练学会模仿参考运动。

```python
# ~/rl_training/configs/amp_backflip_config.py
"""
AMP (Adversarial Motion Priors) 配置
需要参考运动数据（可从动画或其他机器人录制获取）
"""

amp_config = {
    # 参考运动数据
    "motion_file": "motions/lite3_backflip_ref.json",
    
    # AMP 判别器网络
    "discriminator": {
        "hidden_dims": [512, 256],
        "learning_rate": 1e-4,
        "weight_decay": 1e-5,
        "gradient_penalty_weight": 5.0,  # GP 正则化
        "replay_buffer_size": 100000,
    },
    
    # AMP 奖励权重
    "amp_reward_weight": 0.5,    # AMP style reward
    "task_reward_weight": 0.5,   # Task completion reward
    
    # 观测空间（用于判别器）
    "amp_obs_keys": [
        "base_position",
        "base_orientation",
        "base_linear_vel",
        "base_angular_vel",
        "joint_positions",
        "joint_velocities",
    ],
}

# 参考运动数据格式示例 (JSON)
# {
#     "fps": 50,
#     "frames": [
#         {
#             "base_pos": [0, 0, 0.32],
#             "base_quat": [1, 0, 0, 0],
#             "joint_pos": [0, 0.67, -1.3, 0, 0.67, -1.3, ...]
#         },
#         ... // 每帧一个关键状态
#     ]
# }
```

**获取参考运动数据的方法**：

1. **从其他机器人/动画重定向**：使用动作重定向工具将 Boston Dynamics Spot 等大型四足的后空翻动作缩放到 Lite3 尺寸
2. **手动关键帧编辑**：在 MuJoCo 仿真器中手动摆拍关键帧，插值生成连续动作
3. **轨迹优化生成**：使用 CasADi / Drake 等工具做轨迹优化生成可行参考轨迹

#### 17.3.3 跳跃与上楼梯训练

```bash
# 跳跃训练
python scripts/train.py \
  --task Jump-Deeprobotics-Lite3-v0 \
  --num_envs 4096 \
  --max_iterations 5000 \
  --curriculum

# 上楼梯训练（需要楼梯地形）
python scripts/train.py \
  --task StairClimb-Deeprobotics-Lite3-v0 \
  --num_envs 4096 \
  --max_iterations 8000 \
  --curriculum \
  --terrain stairs
```

### 17.4 地形配置

```python
# ~/rl_training/terrain/terrain_config.py
"""
Isaac Lab 地形配置
"""
from omni.isaac.lab.terrains import TerrainGeneratorCfg, HfTerrainBaseCfg

terrain_config = TerrainGeneratorCfg(
    size=(8.0, 8.0),         # 每块地形 8m x 8m
    border_width=20.0,
    num_rows=10,
    num_cols=10,
    horizontal_scale=0.1,
    vertical_scale=0.005,
    slope_threshold=0.75,
    
    sub_terrains={
        # 平地 (10%)
        "flat": {
            "proportion": 0.1,
            "type": "flat",
        },
        # 随机粗糙地面 (20%)
        "rough": {
            "proportion": 0.2,
            "type": "random_uniform",
            "min_height": -0.05,
            "max_height": 0.05,
        },
        # 楼梯 (25%)
        "stairs_up": {
            "proportion": 0.15,
            "type": "pyramid_stairs",
            "step_height": 0.1,       # 10cm 台阶
            "step_width": 0.3,
            "platform_size": 1.5,
        },
        "stairs_down": {
            "proportion": 0.10,
            "type": "inverted_pyramid_stairs",
            "step_height": 0.1,
            "step_width": 0.3,
            "platform_size": 1.5,
        },
        # 离散障碍物 (15%)
        "stepping_stones": {
            "proportion": 0.15,
            "type": "discrete_obstacles",
            "max_height": 0.15,
            "min_size": 0.1,
            "max_size": 0.5,
            "platform_size": 2.0,
        },
        # 斜坡 (20%)
        "slopes": {
            "proportion": 0.2,
            "type": "pyramid_sloped",
            "slope_angle": 0.4,       # ~23度
            "platform_size": 1.5,
        },
        # 间隙 (10%)
        "gaps": {
            "proportion": 0.1,
            "type": "gap",
            "gap_size": [0.1, 0.3],   # 10-30cm 间隙
            "platform_size": 2.0,
        },
    },
)
```

### 17.5 域随机化深度配置

```python
# ~/rl_training/configs/domain_rand_config.py
"""
域随机化配置 - 提升 Sim-to-Real 迁移效果
"""

domain_rand_config = {
    # ===== 物理参数随机化 =====
    "randomize_physics": True,
    
    "body_mass": {
        "range": [0.8, 1.2],        # 整体质量 ±20%
        "operation": "scaling",
    },
    "body_com": {
        "range": [-0.05, 0.05],     # 质心偏移 ±5cm
        "operation": "additive",
    },
    "friction": {
        "range": [0.3, 2.0],        # 摩擦系数大范围
    },
    "restitution": {
        "range": [0.0, 0.5],        # 恢复系数
    },
    
    # ===== 关节参数随机化 =====
    "joint_stiffness": {
        "range": [0.8, 1.2],        # 关节刚度 Kp ±20%
    },
    "joint_damping": {
        "range": [0.8, 1.2],        # 关节阻尼 Kd ±20%
    },
    "joint_friction": {
        "range": [0.0, 0.5],        # 关节摩擦
    },
    "motor_strength": {
        "range": [0.85, 1.15],      # 电机力矩 ±15%
    },
    
    # ===== 外部扰动随机化 =====
    "push_robots": True,
    "push_interval_s": 8,            # 每 8 秒推一次
    "max_push_vel_xy": 1.0,         # 最大推力速度 1 m/s
    "max_push_ang_vel": 0.5,        # 最大旋转扰动 0.5 rad/s
    
    # ===== 传感器噪声随机化 =====
    "observation_noise": {
        "joint_pos_noise": 0.01,     # 关节位置噪声 (rad)
        "joint_vel_noise": 1.5,      # 关节速度噪声 (rad/s)
        "imu_noise": 0.1,            # IMU 噪声 (m/s^2)
        "gyro_noise": 0.05,          # 陀螺仪噪声 (rad/s)
    },
    
    # ===== 延迟随机化 =====
    "action_delay": {
        "range": [0, 3],             # 动作延迟 0-3 步 (0-60ms)
    },
}
```

### 17.6 策略导出与真机部署

```bash
# 1. 训练完成后导出 ONNX
python scripts/export_onnx.py \
  --checkpoint /path/to/best_model.pt \
  --output_dir ./exported_models/

# 2. 在 MuJoCo 中做 Sim-to-Sim 验证
cd ~/Lite3_rl_deploy
./sim2sim_mujoco --model_path ./exported_models/backflip_policy.onnx

# 3. 确认仿真中动作合理后，部署到真机
# ⚠️ 安全须知：
# - 首次测试使用吊绳/安全带保护
# - 确保周围无人、无易碎物品
# - 准备随时按下急停按钮（遥控器 L2+R2）
# - 建议先用低增益（Kp/Kd 降为训练值的 50%）试运行

./sim2real --model_path ./exported_models/backflip_policy.onnx --kp_scale 0.5 --kd_scale 0.5
```

### 17.7 官方训练案例参考

> **务必参考**：`Robot_Training_Cases` 仓库中的 **Case7** 是官方强化学习训练与部署教程。
>
> ```bash
> git clone https://github.com/DeepRoboticsLab/Robot_Training_Cases.git
> cd Robot_Training_Cases/Case7_RL_Training_Deploy
> ```
> 包含从环境搭建、奖励设计、训练到部署的完整流程。

---

## 第十八章 大语言模型（LLM）语音控制机器狗

> 实现全链路：**人类语音 → 语音识别 → LLM 推理 → 运动指令 → 机器狗执行**

### 18.1 系统架构总览

```
┌────────────────── LLM 语音控制全链路 ──────────────────┐
│                                                         │
│  ┌──────────┐    ┌──────────┐    ┌──────────────────┐  │
│  │ 麦克风    │ →  │ ASR      │ →  │ 文本（用户指令） │  │
│  │ (Mic)    │    │ 语音识别  │    │ "往前走3米，    │  │
│  │          │    │ Whisper   │    │  然后转左"      │  │
│  └──────────┘    └──────────┘    └────────┬─────────┘  │
│                                           │             │
│                                    ┌──────▼─────────┐  │
│                                    │ LLM 大模型      │  │
│                                    │ GPT-4 / Qwen   │  │
│                                    │ + 系统提示词    │  │
│                                    │ + 机器人 API    │  │
│                                    └──────┬─────────┘  │
│                                           │             │
│                                    ┌──────▼─────────┐  │
│                                    │ 代码/指令解析   │  │
│                                    │ JSON / Python   │  │
│                                    └──────┬─────────┘  │
│                                           │             │
│                                    ┌──────▼─────────┐  │
│                                    │ ROS2 执行器     │  │
│                                    │ /cmd_vel 发布   │  │
│                                    └──────┬─────────┘  │
│                                           │             │
│                                    ┌──────▼─────────┐  │
│                                    │ Lite3 机器狗    │  │
│                                    │ 执行运动        │  │
│                                    └────────────────┘  │
└─────────────────────────────────────────────────────────┘
```

### 18.2 环境准备

```bash
# 在 Orin NX 或 开发电脑上

# 1. 安装语音识别（OpenAI Whisper）
pip3 install openai-whisper
pip3 install sounddevice numpy  # 麦克风录音

# 2. 安装 LLM 客户端
pip3 install openai  # OpenAI API (GPT-4/GPT-3.5)
# 或
pip3 install dashscope  # 阿里通义千问 API
# 或安装本地 LLM
pip3 install ollama  # 本地运行 Qwen/LLaMA

# 3. 安装 ROS2 Python 绑定
pip3 install rclpy

# 4. 安装 TTS（文本转语音，可选）
pip3 install pyttsx3
# 或
pip3 install edge-tts  # 微软 Edge TTS（效果更好）
```

### 18.3 语音识别模块

```python
#!/usr/bin/env python3
"""
voice_listener.py
实时语音录制 + Whisper 识别
"""
import whisper
import sounddevice as sd
import numpy as np
import tempfile
import wave
import os

class VoiceListener:
    def __init__(self, model_size="base", language="zh"):
        """
        model_size: tiny, base, small, medium, large
        - tiny: 最快，精度一般（推荐 Orin NX 本地运行）
        - base: 平衡选择
        - small: 较好精度
        - large: 最高精度（需云端或高性能 GPU）
        """
        print(f"加载 Whisper {model_size} 模型...")
        self.model = whisper.load_model(model_size)
        self.language = language
        self.sample_rate = 16000
        
    def record_audio(self, duration=5, silence_threshold=0.01, max_duration=15):
        """
        录制音频，支持自动检测静音停止
        duration: 最短录制时长（秒）
        max_duration: 最长录制时长（秒）
        """
        print("🎤 开始录音...（说完后会自动停止）")
        
        audio_chunks = []
        silence_count = 0
        
        def callback(indata, frames, time, status):
            nonlocal silence_count
            volume = np.abs(indata).mean()
            audio_chunks.append(indata.copy())
            if volume < silence_threshold:
                silence_count += 1
            else:
                silence_count = 0
        
        with sd.InputStream(samplerate=self.sample_rate, channels=1, 
                          dtype='float32', callback=callback, 
                          blocksize=self.sample_rate // 10):
            # 至少录 duration 秒
            import time
            start = time.time()
            while True:
                elapsed = time.time() - start
                if elapsed >= max_duration:
                    break
                if elapsed >= duration and silence_count > 10:  # 1秒静音
                    break
                sd.sleep(100)
        
        if not audio_chunks:
            return None
            
        audio = np.concatenate(audio_chunks, axis=0)
        print(f"📝 录音完成 ({len(audio)/self.sample_rate:.1f}秒)")
        return audio
    
    def transcribe(self, audio):
        """将音频转为文本"""
        # 写入临时 WAV 文件
        with tempfile.NamedTemporaryFile(suffix='.wav', delete=False) as f:
            temp_path = f.name
            with wave.open(f.name, 'wb') as wf:
                wf.setnchannels(1)
                wf.setsampwidth(2)
                wf.setframerate(self.sample_rate)
                wf.writeframes((audio * 32767).astype(np.int16).tobytes())
        
        try:
            result = self.model.transcribe(temp_path, language=self.language)
            text = result["text"].strip()
            print(f"🗣️ 识别结果: {text}")
            return text
        finally:
            os.unlink(temp_path)
    
    def listen(self):
        """一次完整的录音+识别"""
        audio = self.record_audio()
        if audio is None:
            return ""
        return self.transcribe(audio)


if __name__ == "__main__":
    listener = VoiceListener(model_size="base", language="zh")
    while True:
        text = listener.listen()
        if text:
            print(f"你说了: {text}")
        if "退出" in text or "停止" in text:
            break
```

### 18.4 LLM 推理与指令生成

#### 18.4.1 系统提示词设计（核心）

```python
# ~/lite3_llm/prompts.py
"""
LLM 系统提示词 - 让大模型理解机器狗的能力和 API
"""

LITE3_SYSTEM_PROMPT = """你是一个控制绝影 Lite3 四足机器狗的 AI 助手。

## 你的能力
你可以理解用户的自然语言指令，并将其转化为机器狗的运动命令。

## 机器狗参数
- 名称: 绝影 Lite3
- 类型: 12 自由度四足机器人
- 最大前进速度: 0.5 m/s
- 最大侧移速度: 0.3 m/s
- 最大旋转速度: 0.8 rad/s
- 步态: trot（小跑）
- 安全限速: 建议不超过 0.4 m/s

## 可用 API 函数
你必须用以下 JSON 格式返回命令：

### 1. move(vx, vy, vyaw, duration)
移动机器狗。
- vx: 前进速度 (m/s)，正=前进，负=后退，范围 [-0.5, 0.5]
- vy: 侧移速度 (m/s)，正=左移，负=右移，范围 [-0.3, 0.3]
- vyaw: 旋转速度 (rad/s)，正=左转，负=右转，范围 [-0.8, 0.8]
- duration: 持续时间 (秒)

### 2. stop()
立即停止所有运动。

### 3. stand()
站立不动。

### 4. sit()
坐下。

### 5. navigate_to(x, y, yaw)
导航到目标点（需要已建好地图并运行 Nav2）。
- x, y: 目标坐标 (米)
- yaw: 目标朝向 (弧度)

### 6. speak(text)
让机器狗"说话"（通过 TTS 播放语音）。

### 7. sequence(commands)
按顺序执行多个命令。

## 输出格式
你必须返回严格的 JSON 格式（不要 markdown 代码块），例如:

{"action": "move", "params": {"vx": 0.3, "vy": 0.0, "vyaw": 0.0, "duration": 3.0}}

多步指令:
{"action": "sequence", "params": {"commands": [
  {"action": "speak", "params": {"text": "好的，我往前走"}},
  {"action": "move", "params": {"vx": 0.3, "vy": 0.0, "vyaw": 0.0, "duration": 3.0}},
  {"action": "stop", "params": {}}
]}}

## 安全规则
1. 速度不要超过安全限制
2. 如果用户的指令可能导致危险（如跳下悬崖），拒绝执行并解释原因
3. 不确定的指令，先确认再执行
4. 遇到"停"、"别动"、"紧急停止"等指令，立即执行 stop()

## 距离/角度换算
- "往前走一步" ≈ move(0.3, 0, 0, 1.0) （约 0.3 米）
- "往前走3米" ≈ move(0.4, 0, 0, 7.5) （0.4m/s × 7.5s = 3m）
- "左转90度" ≈ move(0, 0, 0.5, 3.14) （0.5rad/s × π秒 ≈ π/2 弧度）
- "左转" ≈ move(0, 0, 0.4, 2.0) （约转 45 度）
- "后退" ≈ move(-0.2, 0, 0, 2.0)
"""
```

#### 18.4.2 LLM 调用模块

```python
#!/usr/bin/env python3
"""
llm_controller.py
调用 LLM 将自然语言转为机器狗指令
支持 OpenAI / 通义千问 / 本地 Ollama
"""
import json
import os

class LLMController:
    def __init__(self, provider="openai", model=None):
        """
        provider: "openai" | "dashscope" | "ollama"
        model: 模型名称
        """
        self.provider = provider
        self.conversation_history = []
        
        if provider == "openai":
            from openai import OpenAI
            self.client = OpenAI(api_key=os.environ.get("OPENAI_API_KEY"))
            self.model = model or "gpt-4o"
            
        elif provider == "dashscope":
            import dashscope
            dashscope.api_key = os.environ.get("DASHSCOPE_API_KEY")
            self.model = model or "qwen-max"
            
        elif provider == "ollama":
            import ollama
            self.client = ollama
            self.model = model or "qwen2.5:7b"  # 本地 7B 模型
        
        from prompts import LITE3_SYSTEM_PROMPT
        self.system_prompt = LITE3_SYSTEM_PROMPT
    
    def query(self, user_input: str) -> dict:
        """
        发送用户输入给 LLM，返回解析后的动作字典
        """
        self.conversation_history.append({
            "role": "user",
            "content": user_input
        })
        
        messages = [
            {"role": "system", "content": self.system_prompt}
        ] + self.conversation_history[-10:]  # 保留最近 10 轮对话
        
        try:
            if self.provider == "openai":
                response = self.client.chat.completions.create(
                    model=self.model,
                    messages=messages,
                    temperature=0.1,  # 低温度 = 更确定性的输出
                    max_tokens=500,
                )
                reply = response.choices[0].message.content
                
            elif self.provider == "dashscope":
                from dashscope import Generation
                response = Generation.call(
                    model=self.model,
                    messages=messages,
                    result_format='message',
                    temperature=0.1,
                )
                reply = response.output.choices[0].message.content
                
            elif self.provider == "ollama":
                response = self.client.chat(
                    model=self.model,
                    messages=messages,
                )
                reply = response['message']['content']
            
            # 存入对话历史
            self.conversation_history.append({
                "role": "assistant",
                "content": reply
            })
            
            # 解析 JSON
            action = self._parse_action(reply)
            return action
            
        except Exception as e:
            print(f"❌ LLM 调用失败: {e}")
            return {"action": "error", "params": {"message": str(e)}}
    
    def _parse_action(self, reply: str) -> dict:
        """从 LLM 回复中提取 JSON 指令"""
        # 尝试直接解析
        try:
            return json.loads(reply)
        except json.JSONDecodeError:
            pass
        
        # 尝试从 markdown 代码块提取
        import re
        json_match = re.search(r'```(?:json)?\s*([\s\S]*?)\s*```', reply)
        if json_match:
            try:
                return json.loads(json_match.group(1))
            except json.JSONDecodeError:
                pass
        
        # 尝试找到第一个 { 和最后一个 }
        start = reply.find('{')
        end = reply.rfind('}')
        if start != -1 and end != -1:
            try:
                return json.loads(reply[start:end+1])
            except json.JSONDecodeError:
                pass
        
        # 解析失败
        print(f"⚠️ 无法解析 LLM 回复: {reply[:200]}")
        return {"action": "speak", "params": {"text": reply}}


if __name__ == "__main__":
    # 测试
    controller = LLMController(provider="ollama", model="qwen2.5:7b")
    
    test_inputs = [
        "往前走三步",
        "左转90度然后往前走2米",
        "绕圈走",
        "停下来",
        "去客厅",
    ]
    
    for user_input in test_inputs:
        print(f"\n用户: {user_input}")
        action = controller.query(user_input)
        print(f"指令: {json.dumps(action, ensure_ascii=False, indent=2)}")
```

### 18.5 ROS2 运动执行器

```python
#!/usr/bin/env python3
"""
motion_executor.py
将 LLM 生成的 JSON 指令转为 ROS2 /cmd_vel 命令执行
"""
import rclpy
from rclpy.node import Node
from geometry_msgs.msg import Twist
from std_msgs.msg import String
import json
import time
import threading

class MotionExecutor(Node):
    def __init__(self):
        super().__init__('llm_motion_executor')
        
        # /cmd_vel 发布者
        self.cmd_vel_pub = self.create_publisher(Twist, '/cmd_vel', 10)
        
        # 状态
        self.is_executing = False
        self.should_stop = False
        
        self.get_logger().info("MotionExecutor 初始化完成")
    
    def execute_action(self, action: dict):
        """执行一个动作指令"""
        action_type = action.get("action", "")
        params = action.get("params", {})
        
        self.get_logger().info(f"执行动作: {action_type} {params}")
        
        if action_type == "move":
            self._execute_move(params)
        elif action_type == "stop":
            self._execute_stop()
        elif action_type == "stand":
            self._execute_stop()  # stand 和 stop 相同
        elif action_type == "navigate_to":
            self._execute_navigate(params)
        elif action_type == "speak":
            self._execute_speak(params)
        elif action_type == "sequence":
            self._execute_sequence(params)
        elif action_type == "error":
            self.get_logger().error(f"LLM 错误: {params.get('message')}")
        else:
            self.get_logger().warn(f"未知动作: {action_type}")
    
    def _execute_move(self, params):
        """执行移动命令"""
        vx = float(params.get("vx", 0.0))
        vy = float(params.get("vy", 0.0))
        vyaw = float(params.get("vyaw", 0.0))
        duration = float(params.get("duration", 1.0))
        
        # 安全限速
        vx = max(-0.5, min(0.5, vx))
        vy = max(-0.3, min(0.3, vy))
        vyaw = max(-0.8, min(0.8, vyaw))
        duration = max(0.1, min(30.0, duration))  # 最多30秒
        
        twist = Twist()
        twist.linear.x = vx
        twist.linear.y = vy
        twist.angular.z = vyaw
        
        self.is_executing = True
        self.should_stop = False
        
        start_time = time.time()
        rate = self.create_rate(20)  # 20Hz
        
        while time.time() - start_time < duration and not self.should_stop:
            self.cmd_vel_pub.publish(twist)
            rate.sleep()
        
        # 发送零速停止
        self._execute_stop()
        self.is_executing = False
    
    def _execute_stop(self):
        """立即停止"""
        twist = Twist()  # 全零
        for _ in range(5):  # 连发 5 次确保停止
            self.cmd_vel_pub.publish(twist)
            time.sleep(0.05)
        self.should_stop = True
        self.is_executing = False
    
    def _execute_navigate(self, params):
        """调用 Nav2 导航"""
        x = float(params.get("x", 0.0))
        y = float(params.get("y", 0.0))
        yaw = float(params.get("yaw", 0.0))
        
        # 使用 nav2_simple_commander
        from nav2_simple_commander.robot_navigator import BasicNavigator
        from geometry_msgs.msg import PoseStamped
        import math
        
        navigator = BasicNavigator()
        
        goal = PoseStamped()
        goal.header.frame_id = 'map'
        goal.header.stamp = self.get_clock().now().to_msg()
        goal.pose.position.x = x
        goal.pose.position.y = y
        goal.pose.orientation.z = math.sin(yaw / 2)
        goal.pose.orientation.w = math.cos(yaw / 2)
        
        navigator.goToPose(goal)
        
        while not navigator.isTaskComplete():
            if self.should_stop:
                navigator.cancelTask()
                break
            time.sleep(0.5)
    
    def _execute_speak(self, params):
        """文本转语音"""
        text = params.get("text", "")
        try:
            import pyttsx3
            engine = pyttsx3.init()
            engine.setProperty('rate', 150)
            engine.say(text)
            engine.runAndWait()
        except ImportError:
            self.get_logger().info(f"🔊 机器狗说: {text}")
    
    def _execute_sequence(self, params):
        """顺序执行多个命令"""
        commands = params.get("commands", [])
        for cmd in commands:
            if self.should_stop:
                break
            self.execute_action(cmd)
            time.sleep(0.5)  # 命令之间间隔 0.5 秒
    
    def emergency_stop(self):
        """紧急停止"""
        self.should_stop = True
        self._execute_stop()
        self.get_logger().warn("⚠️ 紧急停止!")
```

### 18.6 完整集成：语音→LLM→机器狗

```python
#!/usr/bin/env python3
"""
main_voice_control.py
完整的语音控制主程序

使用方式:
  python3 main_voice_control.py --llm ollama --model qwen2.5:7b
  python3 main_voice_control.py --llm openai --model gpt-4o
  python3 main_voice_control.py --llm dashscope --model qwen-max
"""
import argparse
import threading
import signal
import sys

import rclpy

from voice_listener import VoiceListener
from llm_controller import LLMController
from motion_executor import MotionExecutor


class VoiceControlSystem:
    def __init__(self, llm_provider, llm_model, whisper_model="base"):
        # 初始化 ROS2
        rclpy.init()
        
        # 初始化各模块
        print("=" * 50)
        print("🤖 绝影 Lite3 语音控制系统 v1.0")
        print("=" * 50)
        
        print("[1/3] 初始化语音识别...")
        self.voice = VoiceListener(model_size=whisper_model, language="zh")
        
        print("[2/3] 初始化 LLM 控制器...")
        self.llm = LLMController(provider=llm_provider, model=llm_model)
        
        print("[3/3] 初始化运动执行器...")
        self.executor = MotionExecutor()
        
        # ROS2 spin 线程
        self.spin_thread = threading.Thread(target=self._ros_spin, daemon=True)
        self.spin_thread.start()
        
        self.running = True
        print("\n✅ 系统就绪！对我说话或按 Ctrl+C 退出\n")
    
    def _ros_spin(self):
        rclpy.spin(self.executor)
    
    def run(self):
        """主循环"""
        while self.running:
            try:
                # 1. 语音识别
                text = self.voice.listen()
                if not text:
                    continue
                
                # 2. 紧急停止检测（不经过 LLM，直接执行）
                stop_keywords = ["停", "别动", "急停", "紧急停止", "stop", "halt"]
                if any(kw in text for kw in stop_keywords):
                    print("🛑 检测到停止指令，紧急停止！")
                    self.executor.emergency_stop()
                    continue
                
                # 3. 退出检测
                if any(kw in text for kw in ["退出", "关闭", "再见"]):
                    print("👋 再见！")
                    self.shutdown()
                    break
                
                # 4. LLM 推理
                print("🧠 LLM 思考中...")
                action = self.llm.query(text)
                print(f"📋 生成指令: {action}")
                
                # 5. 执行
                self.executor.execute_action(action)
                
            except KeyboardInterrupt:
                self.shutdown()
                break
            except Exception as e:
                print(f"❌ 错误: {e}")
    
    def shutdown(self):
        self.running = False
        self.executor.emergency_stop()
        rclpy.shutdown()
        print("系统已关闭")


def main():
    parser = argparse.ArgumentParser(description="Lite3 语音控制系统")
    parser.add_argument("--llm", type=str, default="ollama",
                       choices=["openai", "dashscope", "ollama"],
                       help="LLM 提供商")
    parser.add_argument("--model", type=str, default=None,
                       help="LLM 模型名称")
    parser.add_argument("--whisper", type=str, default="base",
                       choices=["tiny", "base", "small", "medium", "large"],
                       help="Whisper 模型大小")
    args = parser.parse_args()
    
    system = VoiceControlSystem(
        llm_provider=args.llm,
        llm_model=args.model,
        whisper_model=args.whisper,
    )
    system.run()


if __name__ == "__main__":
    main()
```

### 18.7 本地 LLM 部署（Ollama）

如果不想依赖云端 API，可以在 Orin NX 上运行本地 LLM：

```bash
# 1. 安装 Ollama
curl -fsSL https://ollama.com/install.sh | sh

# 2. 拉取轻量模型（Orin NX 16GB 推荐 7B 模型）
ollama pull qwen2.5:7b       # 通义千问 7B（中文最佳）
# 或
ollama pull llama3.2:3b      # Llama 3.2 3B（英文，更快）
# 或
ollama pull phi3:mini        # Microsoft Phi-3 Mini（均衡）

# 3. 测试
ollama run qwen2.5:7b "你好，请用JSON格式回复一个让四足机器人往前走的命令"

# 4. Ollama 以服务模式运行
ollama serve &  # 后台运行，端口 11434
```

**Orin NX 本地 LLM 性能参考**：

| 模型 | 大小 | 推理速度 | 中文能力 | 推荐场景 |
|------|------|---------|---------|---------|
| qwen2.5:3b | 2GB | ~30 tok/s | ★★★★ | 简单指令，低延迟 |
| qwen2.5:7b | 4.5GB | ~15 tok/s | ★★★★★ | **推荐**，中文最佳 |
| llama3.2:3b | 2GB | ~35 tok/s | ★★ | 英文场景 |
| phi3:mini | 2.3GB | ~28 tok/s | ★★★ | 均衡选择 |

### 18.8 多模态扩展：视觉+语音

```python
#!/usr/bin/env python3
"""
multimodal_controller.py
结合相机图像 + 语音指令的多模态控制

例如: "去拿那个红色的杯子" → 相机识别杯子位置 → 规划路径 → 移动
"""
import cv2
import base64
import rclpy
from sensor_msgs.msg import Image
from cv_bridge import CvBridge

class MultimodalController:
    def __init__(self, llm_controller):
        self.llm = llm_controller
        self.bridge = CvBridge()
        self.latest_image = None
        
    def process_with_vision(self, text_command, image=None):
        """
        多模态指令处理
        将相机图像 + 文本指令一起发送给 VLM (Vision Language Model)
        """
        if image is not None:
            # 将图像编码为 base64
            _, buffer = cv2.imencode('.jpg', image)
            img_base64 = base64.b64encode(buffer).decode('utf-8')
            
            # 使用 GPT-4o / Qwen-VL 等多模态模型
            if self.llm.provider == "openai":
                messages = [
                    {"role": "system", "content": self.llm.system_prompt},
                    {"role": "user", "content": [
                        {"type": "text", "text": text_command},
                        {"type": "image_url", "image_url": {
                            "url": f"data:image/jpeg;base64,{img_base64}"
                        }}
                    ]}
                ]
                response = self.llm.client.chat.completions.create(
                    model="gpt-4o",
                    messages=messages,
                    temperature=0.1,
                )
                reply = response.choices[0].message.content
                return self.llm._parse_action(reply)
        
        # 无图像时回退到纯文本
        return self.llm.query(text_command)
```

### 18.9 官方训练案例参考

> **务必参考**：`Robot_Training_Cases` 仓库中的 **Case11_New** 是官方大模型部署与应用教程。
>
> ```bash
> git clone https://github.com/DeepRoboticsLab/Robot_Training_Cases.git
> cd Robot_Training_Cases/Case11_New_LLM_Deploy
> ```
> 包含 LLM 与机器狗集成的官方方案和示例代码。
>
> 另外参考 Microsoft 的 **PromptCraft-Robotics** 项目：
> - GitHub: https://github.com/microsoft/PromptCraft-Robotics
> - 核心思想: 使用 LLM 生成机器人控制代码，通过系统提示词定义机器人 API

---

## 第十九章 VLA 视觉-语言-动作模型接入指南

> **VLA (Vision-Language-Action)** 是最新的机器人基础模型范式，直接将视觉观测和语言指令映射为机器人动作，无需人工设计中间表征。

### 19.1 什么是 VLA？

#### 19.1.1 LLM vs VLA 对比

```
传统 LLM 控制流程:
  图像 → 视觉模型(YOLO等) → 场景描述（文字） 
                                       ↘
  语音 → ASR → 文字指令 ─────────────→ LLM → JSON 指令 → 运动控制器 → 关节
  
VLA 端到端控制流程:
  图像 ──┐
         ├──→ VLA 模型 ──→ 直接输出关节动作 → 关节
  语言 ──┘
```

| 对比 | LLM 管线式 | VLA 端到端 |
|------|-----------|-----------|
| **优点** | 可解释、模块化、容易调试 | 自动学习视觉-动作映射，泛化能力强 |
| **缺点** | 需要手工定义 API、处理中间表征 | 需要大量训练数据、黑盒、延迟可能较高 |
| **适合场景** | 语义级别指令（"去客厅"） | 操作级别指令（"拿起桌上的杯子"） |
| **数据需求** | 少量提示词设计 | 大量机器人遥操作数据 |
| **部署算力** | 中等（7B LLM 即可） | 高（需要 GPU 推理） |

#### 19.1.2 主流 VLA 模型

| 模型 | 来源 | 架构 | 参数量 | 开源 | 特点 |
|------|------|------|--------|------|------|
| **RT-2** | Google DeepMind | PaLM-E + ViT | 55B | ❌ | 先驱性工作，将动作视为语言 token |
| **OpenVLA** | Stanford / UC Berkeley | Prismatic VLM + 动作头 | 7B | ✅ | 完全开源，基于 Open X-Embodiment 数据集 |
| **π0 (pi-zero)** | Physical Intelligence | Flow Matching + VLM | ~3B | ✅ | 扩散策略，灵巧操作强 |
| **Octo** | UC Berkeley | Transformer | 93M | ✅ | 轻量级，适合 fine-tuning |
| **RT-1** | Google | EfficientNet + Transformer | 35M | ✅ | 经典基线 |

### 19.2 OpenVLA 接入方案

OpenVLA 是目前最易于接入的开源 VLA 模型。

#### 19.2.1 架构原理

```
OpenVLA 架构:
┌───────────────────────────────────────────────────────┐
│                    OpenVLA (7B)                       │
│                                                       │
│  ┌──────────┐    ┌──────────────────────────────┐    │
│  │ 视觉编码器 │    │ 语言模型 (Llama 2 基础)      │    │
│  │ (ViT)    │───→│                              │    │
│  │ 224×224  │    │  视觉 tokens + 语言 tokens    │    │
│  └──────────┘    │         ↓                     │    │
│                  │  Transformer Decoder           │    │
│  ┌──────────┐    │         ↓                     │    │
│  │ 语言指令   │───→│  动作 tokens (离散化)         │    │
│  │ 文本      │    │  → 反离散化 → 连续动作        │    │
│  └──────────┘    └──────────────────────────────┘    │
│                              │                        │
│                    ┌─────────▼──────────┐             │
│                    │ 输出: 7-DoF 动作    │             │
│                    │ (dx,dy,dz,drx,dry, │             │
│                    │  drz, gripper)     │             │
│                    └────────────────────┘             │
└───────────────────────────────────────────────────────┘
```

#### 19.2.2 安装 OpenVLA

```bash
# 推荐在带 GPU 的服务器上运行（RTX 3090+ / A100）
# Orin NX 16GB 也可尝试运行 quantized 版本

# 1. 创建环境
conda create -n openvla python=3.10
conda activate openvla

# 2. 安装 PyTorch
pip install torch torchvision --index-url https://download.pytorch.org/whl/cu118

# 3. 克隆 OpenVLA
git clone https://github.com/openvla/openvla.git
cd openvla
pip install -e .

# 4. 下载预训练权重
# OpenVLA 7B (约 14GB)
python -c "
from transformers import AutoModelForVision2Seq, AutoProcessor
model = AutoModelForVision2Seq.from_pretrained('openvla/openvla-7b', torch_dtype='auto')
processor = AutoProcessor.from_pretrained('openvla/openvla-7b')
print('模型下载完成')
"
```

#### 19.2.3 OpenVLA 推理代码

```python
#!/usr/bin/env python3
"""
openvla_inference.py
使用 OpenVLA 将 [图像 + 语言指令] 直接映射为机器人动作
"""
import torch
import numpy as np
from PIL import Image
from transformers import AutoModelForVision2Seq, AutoProcessor

class OpenVLAInference:
    def __init__(self, model_name="openvla/openvla-7b", device="cuda"):
        print(f"加载 OpenVLA 模型: {model_name}")
        self.device = device
        self.processor = AutoProcessor.from_pretrained(model_name, trust_remote_code=True)
        self.model = AutoModelForVision2Seq.from_pretrained(
            model_name,
            torch_dtype=torch.bfloat16,  # 使用 bf16 节省显存
            trust_remote_code=True,
        ).to(device)
        self.model.eval()
        print("OpenVLA 模型加载完成!")
    
    def predict_action(self, image: np.ndarray, instruction: str) -> np.ndarray:
        """
        输入:
            image: RGB 图像 (H, W, 3)，numpy 数组
            instruction: 自然语言指令字符串
        输出:
            action: 7D 动作向量 [dx, dy, dz, drx, dry, drz, gripper]
                    前 6 维是末端执行器的位姿增量
                    最后 1 维是 gripper 开合 (0=关, 1=开)
        """
        # 预处理
        pil_image = Image.fromarray(image)
        inputs = self.processor(
            text=instruction,
            images=pil_image,
            return_tensors="pt",
        ).to(self.device, dtype=torch.bfloat16)
        
        # 推理
        with torch.no_grad():
            action = self.model.predict_action(**inputs)
        
        # 转为 numpy
        action = action.cpu().numpy().flatten()
        return action  # shape: (7,)


if __name__ == "__main__":
    # 测试
    vla = OpenVLAInference()
    
    # 用随机图像测试（实际使用时替换为 RealSense 图像）
    dummy_image = np.random.randint(0, 255, (480, 640, 3), dtype=np.uint8)
    
    action = vla.predict_action(
        image=dummy_image,
        instruction="move forward slowly"
    )
    print(f"预测动作: {action}")
```

### 19.3 VLA → Lite3 动作映射

OpenVLA 标准输出是 **7-DoF 末端执行器动作**（适用于机械臂），需要映射到 Lite3 的 **12 关节 + 全身速度**。

#### 19.3.1 映射策略

```python
#!/usr/bin/env python3
"""
vla_to_lite3_mapper.py
将 VLA 模型的 7-DoF 输出映射到 Lite3 运动指令
"""
import numpy as np

class VLAToLite3Mapper:
    """
    映射策略:
    VLA 输出 [dx, dy, dz, drx, dry, drz, gripper] 含义:
    - dx: 前后位移增量 → 映射为 cmd_vel.linear.x
    - dy: 左右位移增量 → 映射为 cmd_vel.linear.y
    - dz: 上下位移增量 → 映射为体高变化 (可选)
    - drz: 偏航增量 → 映射为 cmd_vel.angular.z
    - drx, dry: 横滚/俯仰 → 通常忽略
    - gripper: 忽略（Lite3 无夹爪）
    """
    
    def __init__(self):
        # 缩放参数（VLA 输出通常是小增量，需要放大到实际速度）
        self.vx_scale = 2.0      # dx → vx 的缩放系数
        self.vy_scale = 1.5      # dy → vy 的缩放系数
        self.vyaw_scale = 3.0    # drz → vyaw 的缩放系数
        
        # 安全限速
        self.max_vx = 0.5        # m/s
        self.max_vy = 0.3        # m/s
        self.max_vyaw = 0.8      # rad/s
        
        # 低通滤波（平滑输出）
        self.alpha = 0.3         # 滤波系数 (0~1, 越小越平滑)
        self.prev_cmd = np.zeros(3)
    
    def map_action(self, vla_action: np.ndarray) -> dict:
        """
        将 7-DoF VLA 动作映射为 Lite3 /cmd_vel
        
        输入: vla_action shape (7,)
        输出: {"vx": float, "vy": float, "vyaw": float}
        """
        dx, dy, dz, drx, dry, drz, gripper = vla_action
        
        # 映射
        vx = dx * self.vx_scale
        vy = dy * self.vy_scale
        vyaw = drz * self.vyaw_scale
        
        # 安全裁剪
        vx = np.clip(vx, -self.max_vx, self.max_vx)
        vy = np.clip(vy, -self.max_vy, self.max_vy)
        vyaw = np.clip(vyaw, -self.max_vyaw, self.max_vyaw)
        
        # 低通滤波
        cmd = np.array([vx, vy, vyaw])
        cmd = self.alpha * cmd + (1 - self.alpha) * self.prev_cmd
        self.prev_cmd = cmd
        
        # 死区处理（小于阈值视为零）
        if abs(cmd[0]) < 0.02:
            cmd[0] = 0.0
        if abs(cmd[1]) < 0.02:
            cmd[1] = 0.0
        if abs(cmd[2]) < 0.05:
            cmd[2] = 0.0
        
        return {
            "vx": float(cmd[0]),
            "vy": float(cmd[1]),
            "vyaw": float(cmd[2]),
        }
```

#### 19.3.2 VLA 闭环控制节点

```python
#!/usr/bin/env python3
"""
vla_control_node.py
VLA 闭环控制: 相机图像 → VLA 推理 → /cmd_vel 控制
以 5Hz 频率运行（受限于 VLA 推理速度）
"""
import rclpy
from rclpy.node import Node
from geometry_msgs.msg import Twist
from sensor_msgs.msg import Image
from std_msgs.msg import String
from cv_bridge import CvBridge
import numpy as np
import threading

from openvla_inference import OpenVLAInference
from vla_to_lite3_mapper import VLAToLite3Mapper


class VLAControlNode(Node):
    def __init__(self):
        super().__init__('vla_control_node')
        
        # 参数
        self.declare_parameter('model_name', 'openvla/openvla-7b')
        self.declare_parameter('control_frequency', 5.0)  # Hz
        self.declare_parameter('instruction', 'walk forward')
        
        model_name = self.get_parameter('model_name').value
        freq = self.get_parameter('control_frequency').value
        self.instruction = self.get_parameter('instruction').value
        
        # 初始化 VLA
        self.vla = OpenVLAInference(model_name=model_name)
        self.mapper = VLAToLite3Mapper()
        self.bridge = CvBridge()
        
        # 最新图像
        self.latest_image = None
        self.image_lock = threading.Lock()
        
        # 订阅相机图像
        self.image_sub = self.create_subscription(
            Image, '/camera/color/image_raw', self.image_callback, 1)
        
        # 订阅语言指令（可运行时更改）
        self.instruction_sub = self.create_subscription(
            String, '/vla/instruction', self.instruction_callback, 1)
        
        # 发布 /cmd_vel
        self.cmd_vel_pub = self.create_publisher(Twist, '/cmd_vel', 10)
        
        # 控制循环定时器
        self.timer = self.create_timer(1.0 / freq, self.control_loop)
        
        self.get_logger().info(f"VLA 控制节点已启动 (指令: '{self.instruction}')")
    
    def image_callback(self, msg):
        with self.image_lock:
            self.latest_image = self.bridge.imgmsg_to_cv2(msg, 'rgb8')
    
    def instruction_callback(self, msg):
        self.instruction = msg.data
        self.get_logger().info(f"更新指令: '{self.instruction}'")
    
    def control_loop(self):
        with self.image_lock:
            image = self.latest_image
        
        if image is None:
            return
        
        # VLA 推理
        action = self.vla.predict_action(image, self.instruction)
        
        # 映射为 Lite3 指令
        cmd = self.mapper.map_action(action)
        
        # 发布 /cmd_vel
        twist = Twist()
        twist.linear.x = cmd["vx"]
        twist.linear.y = cmd["vy"]
        twist.angular.z = cmd["vyaw"]
        self.cmd_vel_pub.publish(twist)
        
        self.get_logger().debug(
            f"VLA 动作: {action[:3]} → cmd_vel: "
            f"vx={cmd['vx']:.2f}, vy={cmd['vy']:.2f}, vyaw={cmd['vyaw']:.2f}")


def main():
    rclpy.init()
    node = VLAControlNode()
    rclpy.spin(node)
    rclpy.shutdown()

if __name__ == '__main__':
    main()
```

```bash
# 启动 VLA 控制
ros2 run lite3_vla vla_control_node \
  --ros-args -p model_name:="openvla/openvla-7b" \
             -p control_frequency:=5.0 \
             -p instruction:="walk to the red chair"

# 运行时更改指令
ros2 topic pub /vla/instruction std_msgs/msg/String "data: 'turn left and stop'"
```

### 19.4 Fine-tuning VLA for Lite3

预训练的 OpenVLA 主要针对机械臂，要用于四足机器人需要微调。

#### 19.4.1 数据采集

```python
#!/usr/bin/env python3
"""
data_collector.py
遥操作数据采集工具 - 用手柄遥控机器人的同时记录 [图像, 指令, 动作] 三元组
"""
import rclpy
from rclpy.node import Node
from sensor_msgs.msg import Image, Joy
from geometry_msgs.msg import Twist
from cv_bridge import CvBridge
import json
import os
import time
import numpy as np
from PIL import Image as PILImage

class DataCollector(Node):
    def __init__(self, save_dir="~/lite3_vla_data"):
        super().__init__('data_collector')
        
        self.save_dir = os.path.expanduser(save_dir)
        os.makedirs(self.save_dir, exist_ok=True)
        
        self.bridge = CvBridge()
        self.episode_count = 0
        self.step_count = 0
        self.recording = False
        self.current_episode = []
        self.current_instruction = ""
        
        # 订阅
        self.image_sub = self.create_subscription(Image, '/camera/color/image_raw', self.image_cb, 1)
        self.cmd_vel_sub = self.create_subscription(Twist, '/cmd_vel', self.cmd_vel_cb, 1)
        self.joy_sub = self.create_subscription(Joy, '/joy', self.joy_cb, 1)
        
        self.latest_image = None
        self.latest_cmd = None
        
        # 10Hz 采集
        self.timer = self.create_timer(0.1, self.collect_step)
        
        self.get_logger().info(f"数据采集器就绪，保存目录: {self.save_dir}")
        self.get_logger().info("按手柄 A 键开始录制，B 键停止")
    
    def image_cb(self, msg):
        self.latest_image = self.bridge.imgmsg_to_cv2(msg, 'rgb8')
    
    def cmd_vel_cb(self, msg):
        self.latest_cmd = [
            msg.linear.x, msg.linear.y, msg.linear.z,
            msg.angular.x, msg.angular.y, msg.angular.z,
            0.0  # 无 gripper
        ]
    
    def joy_cb(self, msg):
        # A 键开始录制
        if msg.buttons[0] == 1 and not self.recording:
            self.start_recording()
        # B 键停止录制
        if msg.buttons[1] == 1 and self.recording:
            self.stop_recording()
    
    def start_recording(self):
        self.recording = True
        self.current_episode = []
        self.step_count = 0
        # 语言指令（可通过终端输入或预设）
        self.current_instruction = input("请输入本次任务的语言指令: ") or "walk forward"
        self.get_logger().info(f"🔴 开始录制 Episode {self.episode_count} (指令: '{self.current_instruction}')")
    
    def stop_recording(self):
        self.recording = False
        self.save_episode()
        self.episode_count += 1
        self.get_logger().info(f"⏹️ 录制结束，共 {self.step_count} 步")
    
    def collect_step(self):
        if not self.recording or self.latest_image is None or self.latest_cmd is None:
            return
        
        # 保存图像
        img_filename = f"ep{self.episode_count:04d}_step{self.step_count:05d}.jpg"
        img_path = os.path.join(self.save_dir, "images", img_filename)
        os.makedirs(os.path.dirname(img_path), exist_ok=True)
        PILImage.fromarray(self.latest_image).save(img_path)
        
        # 记录数据
        self.current_episode.append({
            "step": self.step_count,
            "image": img_filename,
            "instruction": self.current_instruction,
            "action": self.latest_cmd,
            "timestamp": time.time(),
        })
        
        self.step_count += 1
    
    def save_episode(self):
        ep_file = os.path.join(self.save_dir, f"episode_{self.episode_count:04d}.json")
        with open(ep_file, 'w') as f:
            json.dump(self.current_episode, f, indent=2)
        self.get_logger().info(f"💾 保存: {ep_file}")


def main():
    rclpy.init()
    collector = DataCollector()
    rclpy.spin(collector)
    rclpy.shutdown()

if __name__ == '__main__':
    main()
```

#### 19.4.2 Fine-tuning 配置

```python
# ~/openvla/finetune_lite3.py
"""
OpenVLA Fine-tuning for Lite3
需要约 50-200 个遥操作 episode（每个 50-200 步）
"""
from openvla.training import VLAFineTuner

config = {
    # 数据
    "data_dir": "~/lite3_vla_data",
    "image_size": 224,
    "action_dim": 7,
    
    # 模型
    "base_model": "openvla/openvla-7b",
    "use_lora": True,              # LoRA 微调（节省显存）
    "lora_rank": 32,
    "lora_alpha": 64,
    
    # 训练
    "batch_size": 8,
    "learning_rate": 2e-5,
    "num_epochs": 50,
    "warmup_ratio": 0.1,
    "weight_decay": 0.01,
    
    # 数据增强
    "augmentation": {
        "random_crop": True,
        "color_jitter": True,
        "random_erasing": 0.1,
    },
    
    # 保存
    "output_dir": "./lite3_vla_finetuned",
    "save_every_n_epochs": 10,
}

# 训练命令:
# python finetune_lite3.py
# 或使用 Hugging Face Trainer:
# accelerate launch --num_processes 4 finetune_lite3.py
```

### 19.5 π0 (pi-zero) 接入方案

π0 使用**扩散策略（Diffusion Policy）**，在灵巧操作任务上表现更好。

```python
#!/usr/bin/env python3
"""
pi0_inference.py
π0 模型推理（扩散策略）

π0 的特殊之处: 
- 使用 Flow Matching 生成动作序列
- 一次预测未来 N 步动作（action chunking）
- 更适合需要精细控制的任务
"""
import torch
import numpy as np

class Pi0Inference:
    def __init__(self, checkpoint_path, device="cuda"):
        """
        注意: π0 的具体 API 可能随版本变化
        以下为示意代码，请参考官方文档
        """
        self.device = device
        # 加载模型（具体方式取决于 π0 发布形式）
        # self.model = load_pi0_model(checkpoint_path).to(device)
        
        self.action_horizon = 16  # 一次预测 16 步
        self.action_buffer = []
        
    def predict_action_chunk(self, image, instruction):
        """
        预测未来 N 步动作
        输出: (N, action_dim) 的动作序列
        """
        # 预处理
        # ...
        
        # Flow Matching 去噪过程
        # 从噪声逐步去噪得到动作序列
        # action_chunk = self.model.sample(image, instruction, num_steps=10)
        
        # 返回动作序列
        # return action_chunk  # shape: (16, 7)
        pass
    
    def get_action(self, image, instruction):
        """
        获取当前步的动作
        使用 action chunking 策略: 每 N 步重新预测
        """
        if len(self.action_buffer) == 0:
            chunk = self.predict_action_chunk(image, instruction)
            self.action_buffer = list(chunk)
        
        return self.action_buffer.pop(0)
```

### 19.6 Octo 轻量级 VLA 方案

Octo 只有 93M 参数，适合在 Orin NX 上本地运行。

```python
#!/usr/bin/env python3
"""
octo_inference.py
Octo 轻量级 VLA 推理
93M 参数，可在 Orin NX 上实时运行 (~10Hz)
"""
import jax
import numpy as np

class OctoInference:
    def __init__(self):
        from octo.model.octo_model import OctoModel
        
        print("加载 Octo 模型...")
        self.model = OctoModel.load_pretrained("hf://rail-berkeley/octo-small")
        print("Octo 加载完成!")
    
    def predict(self, image, instruction, task=None):
        """
        输入:
            image: (H, W, 3) RGB
            instruction: 语言指令
        输出:
            action: (7,) 连续动作
        """
        if task is None:
            task = self.model.create_tasks(texts=[instruction])
        
        observation = {
            "image_primary": image[None],  # 添加 batch 维度
            "pad_mask": np.array([[True]]),
        }
        
        action = self.model.sample_actions(
            observation,
            task,
            rng=jax.random.PRNGKey(0),
        )
        
        return np.array(action[0, 0])  # (7,)
```

```bash
# 安装 Octo
pip install octo-model
# 或从源码:
git clone https://github.com/octo-models/octo.git
cd octo && pip install -e .
```

### 19.7 VLA 部署架构建议

```
┌────────────── 推荐部署架构 ──────────────────┐
│                                               │
│  方案 A: 云端推理（推荐初期开发）              │
│  ┌──────┐  WiFi/5G  ┌────────────────┐       │
│  │Lite3 │ ←──────→  │ 云端 GPU 服务器  │       │
│  │Orin NX│ 图像上传   │ OpenVLA / π0   │       │
│  │      │ 动作下发   │ RTX 4090 / A100 │       │
│  └──────┘           └────────────────┘       │
│  延迟: 50-200ms，适合低速导航任务              │
│                                               │
│  方案 B: 边缘推理（推荐成熟部署）              │
│  ┌──────────────────────┐                    │
│  │ Lite3 Orin NX        │                    │
│  │ Octo (93M) 本地推理   │ ← 推荐 Lite3      │
│  │ 或 OpenVLA-3B 量化版  │                    │
│  └──────────────────────┘                    │
│  延迟: 20-100ms，适合实时控制                  │
│                                               │
│  方案 C: 混合架构（最佳平衡）                  │
│  ┌──────┐           ┌────────────────┐       │
│  │Lite3 │           │ 边缘计算盒      │       │
│  │Orin NX│──有线──→ │ Jetson AGX Orin│       │
│  │低级控制│          │ VLA 推理        │       │
│  └──────┘           └────────────────┘       │
│  延迟: 10-50ms                                │
└───────────────────────────────────────────────┘
```

### 19.8 VLA 与 LLM 混合架构（推荐）

在实际应用中，推荐将 LLM（高层决策）和 VLA（底层执行）结合：

```
┌─────────────── LLM + VLA 混合架构 ───────────────────┐
│                                                       │
│  用户语音: "去厨房，拿起桌上的红杯子"                   │
│                                                       │
│  ┌──────────────────────────────┐                    │
│  │ LLM（高层任务分解）           │                    │
│  │ 输入: 用户指令 + 场景描述     │                    │
│  │ 输出:                         │                    │
│  │  1. navigate_to("kitchen")   │                    │
│  │  2. find("red cup on table") │                    │
│  │  3. approach("red cup")      │                    │
│  │  4. grasp("red cup")         │ ← 高层规划         │
│  └──────────┬───────────────────┘                    │
│             │                                        │
│  ┌──────────▼───────────────────┐                    │
│  │ 任务 1: navigate_to          │                    │
│  │ → 使用 Nav2 导航             │ ← 经典方法         │
│  └──────────┬───────────────────┘                    │
│             │                                        │
│  ┌──────────▼───────────────────┐                    │
│  │ 任务 2-4: 视觉引导操作        │                    │
│  │ → 使用 VLA 模型               │ ← 端到端学习       │
│  │ 输入: 相机图像 + "approach    │                    │
│  │        the red cup"          │                    │
│  │ 输出: 运动动作                │                    │
│  └──────────────────────────────┘                    │
└───────────────────────────────────────────────────────┘
```

```python
#!/usr/bin/env python3
"""
hybrid_controller.py
LLM + VLA 混合控制器
"""

class HybridController:
    def __init__(self, llm_controller, vla_controller, nav_executor, motion_executor):
        self.llm = llm_controller
        self.vla = vla_controller
        self.nav = nav_executor
        self.motion = motion_executor
    
    def execute_user_command(self, user_text, current_image=None):
        """
        高层: LLM 分解任务
        底层: 根据任务类型选择 Nav2 / VLA / 直接控制
        """
        # Step 1: LLM 任务分解
        plan = self.llm.query(f"""
请将以下用户指令分解为子任务序列，每个子任务标注类型:
- "navigate": 导航到某地（用 Nav2）
- "vla_control": 需要视觉引导的精细控制（用 VLA）
- "simple_move": 简单移动（直接 cmd_vel）
- "speak": 语音回复

用户指令: {user_text}

返回 JSON 格式: {{"tasks": [{{"type": "...", "instruction": "...", "params": {{}}}}]}}
""")
        
        # Step 2: 按类型执行
        tasks = plan.get("tasks", [])
        for task in tasks:
            task_type = task.get("type")
            instruction = task.get("instruction", "")
            params = task.get("params", {})
            
            if task_type == "navigate":
                self.nav.navigate_to(params.get("x", 0), params.get("y", 0), params.get("yaw", 0))
            
            elif task_type == "vla_control":
                # VLA 闭环控制直到任务完成
                for _ in range(100):  # 最多 100 步
                    image = self.get_current_image()
                    action = self.vla.predict_action(image, instruction)
                    cmd = self.mapper.map_action(action)
                    self.motion.publish_cmd_vel(cmd)
                    if self.is_task_done(action):
                        break
            
            elif task_type == "simple_move":
                self.motion.execute_action({"action": "move", "params": params})
            
            elif task_type == "speak":
                self.motion.execute_action({"action": "speak", "params": {"text": instruction}})
```

---

## 第二十章 开源项目完整索引

以下是 DeepRoboticsLab GitHub 组织下所有与 Lite3 相关的开源项目：

| 仓库名称 | 描述 | 语言 | Stars | 用途 |
|---------|------|------|-------|------|
| [**rl_training**](https://github.com/DeepRoboticsLab/rl_training) | 基于 Isaac Lab 的 RL 训练库 | Python | 184+ | 策略训练 |
| [**Lite3_rl_deploy**](https://github.com/DeepRoboticsLab/Lite3_rl_deploy) | Lite3 RL Sim-to-Sim / Sim-to-Real 部署 | C++ | 114+ | 策略部署 |
| [**Lite3_MotionSDK**](https://github.com/DeepRoboticsLab/Lite3_MotionSDK) | Lite3 底层关节控制 SDK | C++ | 77+ | 底层控制 |
| [**deep_robotics_model**](https://github.com/DeepRoboticsLab/deep_robotics_model) | 机器人 URDF/MJCF/USD 模型 | — | 39+ | 仿真模型 |
| [**Robot_Training_Cases**](https://github.com/DeepRoboticsLab/Robot_Training_Cases) | 机器人训练案例 | — | 34+ | 学习参考 |
| [**Lite3_ROS**](https://github.com/DeepRoboticsLab/Lite3_ROS) | Lite3 ROS/ROS2 感知开发 | C++ | 31+ | 感知开发 |
| [**sdk_deploy**](https://github.com/DeepRoboticsLab/sdk_deploy) | 新版 ROS2 SDK 部署（Lite3 + M20） | C++ | 20+ | SDK 部署 |
| [**fast-livo2-deep-robotics**](https://github.com/DeepRoboticsLab/fast-livo2-deep-robotics) | Lite3 硬件扩展 + FAST-LIVO2 | C++ | 16+ | SLAM |
| [**gamepad**](https://github.com/DeepRoboticsLab/gamepad) | 手柄遥控开发库 | C++ | 10+ | 遥控操作 |
| [**Lite3_rl_training**](https://github.com/DeepRoboticsLab/Lite3_rl_training) | Lite3 旧版 RL 训练 | Python | 6+ | 旧版训练 |
| [**lightning-lm-deep-robotics**](https://github.com/DeepRoboticsLab/lightning-lm-deep-robotics) | LiDAR 定位与建图 | C++ | 5+ | 定位建图 |

### 教程视频

- **Bilibili**: https://b23.tv/UoIqsFn
- **YouTube**: https://youtube.com/playlist?list=PLy9YHJvMnjO0X4tx_NTWugTUMJXUrOgFH

### 社区

- **Discord**: https://discord.gg/gdM9mQutC8
- **GitHub**: https://github.com/DeepRoboticsLab

---

## 第二十一章 安全须知与最佳实践

### 21.1 安全距离

> ⚠️ **最高优先级安全规则**: 当机器狗在 SDK 控制下运行时，所有在场人员必须保持至少 **5 米** 的安全距离！

### 21.2 操作安全清单

#### 运行前检查
- [ ] 电池电量充足
- [ ] 机器狗放置在开阔平坦地面
- [ ] 周围无易碎物品或障碍物
- [ ] 所有人员已撤至安全距离
- [ ] 急停按钮随时可触及
- [ ] 首次测试建议使用悬吊装置

#### 开发安全规范
- [ ] **永远先在仿真中测试**，确认无误再部署到实机
- [ ] SDK 控制代码中**不要直接取消注释发送指令**，先验证通信
- [ ] 控制参数（kp, kd, tff）需要仔细计算，过大的参数可能导致剧烈运动
- [ ] 确保控制频率足够高（建议 ≥ 100Hz），否则关节会进入阻尼保护

### 21.3 紧急停止方法

| 方法 | 操作 | 响应时间 |
|------|------|---------|
| **App 急停** | App 界面按急停按钮 | 即时 |
| **手柄急停** | 手柄急停按钮 | 即时 |
| **远程停止** | `ssh` 后执行 `sudo ./stop.sh` | 2-3秒 |
| **断电** | 拔掉电池 | 即时 |

### 21.4 保修注意事项

以下情况 **不在免费保修范围** 内：

1. 人为损坏（如摔落、碰撞）
2. 私自改造、拆装、开壳
3. 未按说明书使用导致损坏
4. 超过安全承重使用
5. 自行加装第三方产品导致的损坏
6. 进水、进杂物或化学物质
7. 不可抗力（台风、地震、火灾、雷击等）

> ⚠️ **特别注意**: **因使用 SDK 造成的设备损坏不在保修范围内！**

### 21.5 最佳实践

1. **渐进式开发**: 仿真 → 悬挂测试 → 地面低速测试 → 正常速度测试
2. **参数调优**: 从小参数开始，逐步增大
3. **日志记录**: 记录每次实验的参数和结果
4. **版本控制**: 使用 Git 管理你的控制代码
5. **定期备份**: 备份运动主机上的配置文件

---

## 第二十二章 常见问题与故障排查

### 22.1 连接问题

#### Q: WiFi 连不上机器狗

**A**: 
1. 确认机器狗已开机
2. WiFi 名称以 `Lite` 开头，密码 `12345678`
3. 如果仍然连不上，尝试重启机器狗
4. 检查是否有多台机器狗 WiFi 冲突

#### Q: SSH 连接被拒绝

**A**: 
1. 确认 IP 地址正确（`192.168.2.1` 或 `192.168.1.120`）
2. 尝试不同的用户名/密码组合：
   - `ysc` / `'`（单引号）
   - `user` / `123456`
   - `firefly` / `firefly`
3. 确认在同一网段

#### Q: SDK 收不到机器人数据

**A**: 参见第五章 5.7 节通信故障排查流程。

### 22.2 编译问题

#### Q: CMake 编译报错找不到库

**A**: 
```bash
# 安装基础编译工具
sudo apt install build-essential cmake

# 如果缺少特定库，根据报错安装
sudo apt install libdw-dev   # backward-cpp 需要
```

#### Q: ARM 编译不通过

**A**: 
1. 确认在运动主机上使用 `-DBUILD_PLATFORM=arm`
2. 运动主机的 GCC 版本可能较旧，避免使用过新的 C++ 特性

### 22.3 运动问题

#### Q: 机器狗自己停止运动

**A**: 这是**正常现象**。底层控制器在超过 1 秒未收到 SDK 指令时会进入阻尼保护模式。确保你的控制循环频率足够高。

#### Q: 运行 SDK 后机器狗身体歪斜

**A**: 
1. 在运行前调整机器狗至标准准备姿态
2. 检查关节控制参数是否正确
3. 检查是否有关节电机异常（温度过高等）

#### Q: 手柄操作没有反应

**A**: 
1. 确认手柄已安装 `controlapp.apk`
2. 确认手柄连接的 WiFi 和 IP 配置正确
3. 确认端口号（默认 12121）未被占用
4. 检查手柄电池电量

### 22.4 仿真问题

#### Q: MuJoCo 仿真器打不开

**A**: 
```bash
pip install mujoco    # 确认已安装
python3 -c "import mujoco; print(mujoco.__version__)"  # 验证
```

#### Q: RL 训练很慢

**A**: 
1. 确认使用了 GPU（`nvidia-smi` 查看 GPU 状态）
2. 尝试增加 `--num_envs` 数量
3. 使用 `--headless` 无头模式训练
4. 考虑多 GPU 训练

### 22.5 ROS2 问题

#### Q: ROS2 Topic 没有数据

**A**: 
1. 确认 `transfer` 节点已启动
2. 检查 `ROS_DOMAIN_ID` 是否一致（默认 0）
3. 使用 `ros2 topic list` 查看可用 Topic
4. 使用 `ros2 node list` 查看活跃节点

#### Q: Isaac Lab 安装后 Pylance 无法索引

**A**: 在 VS Code 的 `.vscode/settings.json` 中添加：
```json
{
    "python.languageServer": "Pylance",
    "python.analysis.extraPaths": [
        "${workspaceFolder}/source/rl_training",
        "/<path-to-isaac-lab>/source/isaaclab",
        "/<path-to-isaac-lab>/source/isaaclab_assets",
        "/<path-to-isaac-lab>/source/isaaclab_mimic",
        "/<path-to-isaac-lab>/source/isaaclab_rl",
        "/<path-to-isaac-lab>/source/isaaclab_tasks"
    ]
}
```

---

## 附录 A 关节编号与数据结构速查表

### A.1 关节顺序（12 个关节）

| 索引 | 关节名称 | 位置 |
|------|---------|------|
| 0 | FL_HipX | 左前腿-髋关节侧摆 |
| 1 | FL_HipY | 左前腿-髋关节俯仰 |
| 2 | FL_Knee | 左前腿-膝关节 |
| 3 | FR_HipX | 右前腿-髋关节侧摆 |
| 4 | FR_HipY | 右前腿-髋关节俯仰 |
| 5 | FR_Knee | 右前腿-膝关节 |
| 6 | HL_HipX | 左后腿-髋关节侧摆 |
| 7 | HL_HipY | 左后腿-髋关节俯仰 |
| 8 | HL_Knee | 左后腿-膝关节 |
| 9 | HR_HipX | 右后腿-髋关节侧摆 |
| 10 | HR_HipY | 右后腿-髋关节俯仰 |
| 11 | HR_Knee | 右后腿-膝关节 |

### A.2 数据结构一览

#### RobotData（接收数据）

```
robot_data->
├── imu
│   ├── acc_x / acc_y / acc_z              # 三轴加速度
│   ├── angle_roll / angle_pitch / angle_yaw  # 三轴角度
│   ├── angular_velocity_roll / pitch / yaw   # 三轴角速度
│   ├── buffer_byte / buffer_float            # 缓冲数据
│   └── timestamp                              # 时间戳
├── joint_data
│   ├── fl_leg[3]  →  .position / .velocity / .torque / .temperature
│   ├── fr_leg[3]
│   ├── hl_leg[3]
│   ├── hr_leg[3]
│   └── joint_data[12]                        # 全部关节数据
├── contact_force
│   ├── fl_leg[3]  →  X / Y / Z 轴力
│   ├── fr_leg[3]
│   ├── hl_leg[3]
│   ├── hr_leg[3]
│   └── leg_force[12]                         # 全部足端力
└── tick                                       # 运行周期
```

#### RobotCmd（发送指令）

```
robot_joint_cmd->
├── fl_leg[3]  →  .kp / .kd / .position / .velocity / .torque
├── fr_leg[3]
├── hl_leg[3]
└── hr_leg[3]
```

### A.3 ROS2 Topic 消息类型（Lite3_ROS）

| Topic | 消息类型 | 方向 |
|-------|---------|------|
| `/leg_odom` | `geometry_msgs::msg::PoseWithCovarianceStamped` | 发布 |
| `/leg_odom2` | `nav_msgs::msg::Odometry` | 发布 |
| `/imu/data` | `sensor_msgs::msg::Imu` | 发布 |
| `/joint_states` | `sensor_msgs::msg::JointState` | 发布 |
| `/cmd_vel` | `geometry_msgs::msg::Twist` | 订阅 |

### A.4 ROS2 Topic 消息类型（sdk_deploy）

| Topic | 消息类型 | 方向 |
|-------|---------|------|
| `/IMU_DATA` | `drdds/msg/ImuData` | 发布 |
| `/JOINTS_DATA` | `drdds/msg/JointsData` | 发布 |
| `/JOINTS_CMD` | `drdds/msg/JointsDataCmd` | 订阅 |
| `/GAMEPAD_DATA` | `drdds/msg/GamepadData` | 发布 |
| `/SDK_MODE` | `drdds/srv/StdSrvInt32` | 服务 |

---

## 附录 B 网络配置速查表

### B.1 IP 地址

| 设备 | WiFi（网段 1） | WiFi（网段 2） | 以太网 |
|------|--------------|--------------|--------|
| 运动主机 | `192.168.1.120` | `192.168.2.1` | `192.168.1.120` |
| 开发电脑 | `192.168.1.xxx` | `192.168.2.xxx` | `192.168.1.xxx` |

### B.2 端口号

| 端口 | 用途 |
|------|------|
| `43897` | 运动主机 → SDK 数据端口 |
| `43893` | SDK → 运动主机 控制端口 |
| `12121` | 手柄 → 开发主机 遥控端口 |

### B.3 WiFi 信息

| 项目 | 值 |
|------|------|
| WiFi 名称 | `Lite*******`（以 Lite 开头） |
| WiFi 密码 | `12345678` |

### B.4 SSH 登录

| 用户名 | 密码 | 说明 |
|--------|------|------|
| `ysc` | `'`（英文单引号） | 默认，优先尝试 |
| `user` | `123456` | 备用 |
| `firefly` | `firefly` | 旧版 |

### B.5 关键配置文件

| 文件路径 | 作用 |
|---------|------|
| `~/jy_exe/conf/network.toml` | SDK 通信目标 IP 和端口 |
| `~/jy_exe/conf/name.toml` | 机器人型号配置 |

---

## 附录 C 常用命令速查表

### C.1 运动主机管理

```bash
# SSH 连接
ssh ysc@192.168.2.1

# 启动/停止/重启运动程序
cd ~/jy_exe/scripts
sudo ./run.sh
sudo ./stop.sh
sudo ./restart.sh
sudo ./show_log.sh

# 查看运动进程
top | grep jy_exe
```

### C.2 MotionSDK 编译与运行

```bash
# x86 编译
mkdir build && cd build
cmake .. -DBUILD_PLATFORM=x86
make -j

# ARM 编译
cmake .. -DBUILD_PLATFORM=arm
make -j

# 运行
./Lite_motion
```

### C.3 RL Deploy 编译与运行

```bash
# 仿真模式（x86）
cmake .. -DBUILD_PLATFORM=x86 -DBUILD_SIM=ON -DSEND_REMOTE=OFF
make -j
./rl_deploy

# 实机模式（ARM）
cmake .. -DBUILD_PLATFORM=arm -DBUILD_SIM=OFF -DSEND_REMOTE=OFF
make -j
./rl_deploy
```

### C.4 RL 训练

```bash
# 训练
python scripts/reinforcement_learning/rsl_rl/train.py \
  --task=Rough-Deeprobotics-Lite3-v0 --headless

# 回放
python scripts/reinforcement_learning/rsl_rl/play.py \
  --task=Rough-Deeprobotics-Lite3-v0 --num_envs=10

# 导出 ONNX
python scripts/tools/export_onnx_fast.py \
  --checkpoint_path <path>/model_5000.pt \
  --robot lite3 --output_path lite3_policy.onnx

# TensorBoard
tensorboard --logdir=logs
```

### C.5 ROS2 常用命令

```bash
# 查看话题
ros2 topic list
ros2 topic info /cmd_vel
ros2 topic echo /imu/data

# 查看节点
ros2 node list
ros2 node info /lite3

# 发送速度指令
ros2 topic pub -r 10 /cmd_vel geometry_msgs/msg/Twist \
  "{linear:{x: 0.2, y: 0.0, z: 0.0}, angular:{x: 0.0, y: 0.0, z: 0.0}}"
```

### C.6 文件传输

```bash
# 电脑 → 机器狗
scp -r ~/my_project ysc@192.168.2.1:~/

# 机器狗 → 电脑
scp -r ysc@192.168.2.1:~/data ~/local_data/
```

### C.7 网络诊断

```bash
# 测试连通性
ping 192.168.2.1

# 抓包分析
sudo tcpdump -x port 43897 -i lo

# 查看网络接口
ifconfig
```

---

## 附录 D 术语表（小白词典）

| 术语 | 英文 | 解释 |
|------|------|------|
| **四足机器人** | Quadruped Robot | 四条腿的机器人，像一只机器狗 |
| **关节** | Joint | 机器人腿上可以转动的地方，像人的膝盖 |
| **自由度（DOF）** | Degree of Freedom | 关节可以转动的方向数量 |
| **力矩/扭矩** | Torque | 让关节转动的力，单位 N·m |
| **位置控制** | Position Control | 让关节转到指定角度 |
| **速度控制** | Velocity Control | 让关节以指定速度转动 |
| **力矩控制** | Torque Control | 给关节施加指定的力 |
| **阻尼控制** | Damping Control | 让关节像弹簧一样有阻力 |
| **PD 控制** | PD Control | 一种常用的控制方法，P 是比例，D 是微分 |
| **kp** | Proportional Gain | 位置误差的放大系数 |
| **kd** | Derivative Gain | 速度误差的放大系数 |
| **前馈力矩（tff）** | Feed-forward Torque | 预先给定的补偿力矩 |
| **IMU** | Inertial Measurement Unit | 惯性测量单元，测量加速度和角速度 |
| **里程计** | Odometry | 通过传感器估计机器人走了多远 |
| **URDF** | Unified Robot Description Format | 机器人模型描述格式 |
| **MJCF** | MuJoCo XML Format | MuJoCo 仿真器使用的模型格式 |
| **USD** | Universal Scene Description | 通用场景描述格式 |
| **SDK** | Software Development Kit | 软件开发工具包 |
| **UDP** | User Datagram Protocol | 一种快速的网络通信协议 |
| **DDS** | Data Distribution Service | 数据分发服务 |
| **ROS2** | Robot Operating System 2 | 机器人操作系统第 2 版 |
| **Topic** | Topic | ROS2 中的消息通道 |
| **Node** | Node | ROS2 中的程序单元 |
| **SSH** | Secure Shell | 安全远程登录协议 |
| **SCP** | Secure Copy Protocol | 安全文件传输协议 |
| **RL** | Reinforcement Learning | 强化学习，通过奖惩训练 AI |
| **Sim-to-Sim** | Simulation to Simulation | 仿真到仿真的迁移 |
| **Sim-to-Real** | Simulation to Real | 仿真到实际的迁移 |
| **ONNX** | Open Neural Network Exchange | 神经网络模型交换格式 |
| **Isaac Lab** | NVIDIA Isaac Lab | NVIDIA 的机器人学习仿真框架 |
| **MuJoCo** | Multi-Joint dynamics with Contact | 多关节动力学仿真器 |
| **PyBullet** | PyBullet | Python 物理仿真引擎 |
| **SLAM** | Simultaneous Localization and Mapping | 同步定位与建图 |
| **LiDAR** | Light Detection and Ranging | 激光雷达 |
| **FAST-LIVO2** | Fast LiDAR-Inertial-Visual Odometry | 快速激光-惯性-视觉里程计 |
| **ARM** | ARM Architecture | 一种 CPU 架构，机器狗运动主机使用 |
| **x86** | x86 Architecture | 一种 CPU 架构，普通电脑使用 |
| **Orin NX** | NVIDIA Jetson Orin NX | NVIDIA 的嵌入式 AI 计算平台 |
| **AGX Orin** | NVIDIA Jetson AGX Orin | NVIDIA 的高性能 AI 计算平台 |

---

## 版权与免责声明

本手册为第三方编写的二次开发参考文档，非云深处科技官方文档。所有技术信息汇编自云深处科技官方网站（https://www.deeprobotics.cn/）、官方 GitHub 仓库（https://github.com/DeepRoboticsLab）及公开技术资料。

- 本手册中所有的产品参数均为官方测试数据，实际运行环境下或有偏差
- 产品及软件以实际发货版本为准
- 使用 SDK 进行二次开发造成的设备损坏不在保修范围内
- 开发过程中请务必注意人身安全和设备安全

如有任何疑问，请联系云深处科技官方：
- **技术支持**: support@deeprobotics.cn
- **售后服务**: 0571-85073618
- **购买咨询**: partner@deeprobotics.cn
- **教育合作**: education@deeprobotics.cn

---

> 📝 **最后更新**: 2026-04-04 | 如发现内容有误或需要补充，欢迎反馈
