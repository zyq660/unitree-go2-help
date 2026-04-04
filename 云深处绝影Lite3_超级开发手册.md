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
- [第十五章 开源项目完整索引](#第十五章-开源项目完整索引)
- [第十六章 安全须知与最佳实践](#第十六章-安全须知与最佳实践)
- [第十七章 常见问题与故障排查](#第十七章-常见问题与故障排查)
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

## 第十五章 开源项目完整索引

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

## 第十六章 安全须知与最佳实践

### 16.1 安全距离

> ⚠️ **最高优先级安全规则**: 当机器狗在 SDK 控制下运行时，所有在场人员必须保持至少 **5 米** 的安全距离！

### 16.2 操作安全清单

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

### 16.3 紧急停止方法

| 方法 | 操作 | 响应时间 |
|------|------|---------|
| **App 急停** | App 界面按急停按钮 | 即时 |
| **手柄急停** | 手柄急停按钮 | 即时 |
| **远程停止** | `ssh` 后执行 `sudo ./stop.sh` | 2-3秒 |
| **断电** | 拔掉电池 | 即时 |

### 16.4 保修注意事项

以下情况 **不在免费保修范围** 内：

1. 人为损坏（如摔落、碰撞）
2. 私自改造、拆装、开壳
3. 未按说明书使用导致损坏
4. 超过安全承重使用
5. 自行加装第三方产品导致的损坏
6. 进水、进杂物或化学物质
7. 不可抗力（台风、地震、火灾、雷击等）

> ⚠️ **特别注意**: **因使用 SDK 造成的设备损坏不在保修范围内！**

### 16.5 最佳实践

1. **渐进式开发**: 仿真 → 悬挂测试 → 地面低速测试 → 正常速度测试
2. **参数调优**: 从小参数开始，逐步增大
3. **日志记录**: 记录每次实验的参数和结果
4. **版本控制**: 使用 Git 管理你的控制代码
5. **定期备份**: 备份运动主机上的配置文件

---

## 第十七章 常见问题与故障排查

### 17.1 连接问题

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

### 17.2 编译问题

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

### 17.3 运动问题

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

### 17.4 仿真问题

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

### 17.5 ROS2 问题

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
