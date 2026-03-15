# 宇树 Go2 机器狗超级开发手册

> **版本**: v1.0 | **日期**: 2026-03-13 | **适用机型**: Unitree Go2 AIR / PRO / X / EDU  
> **官方文档**: https://support.unitree.com/home/en/developer  
> **GitHub**: https://github.com/unitreerobotics  
> **官方网站**: https://www.unitree.com/go2

---

## 目录

- [第一章 Go2 产品概述](#第一章-go2-产品概述)
- [第二章 硬件规格与参数](#第二章-硬件规格与参数)
- [第三章 系统架构与通信协议](#第三章-系统架构与通信协议)
- [第四章 开发环境搭建](#第四章-开发环境搭建)
- [第五章 unitree_sdk2 C++ 开发](#第五章-unitree_sdk2-c-开发)
- [第六章 Python SDK 开发](#第六章-python-sdk-开发)
- [第七章 低层电机控制](#第七章-低层电机控制)
- [第八章 高层运动控制](#第八章-高层运动控制)
- [第九章 传感器数据获取](#第九章-传感器数据获取)
- [第十章 运动服务切换接口](#第十章-运动服务切换接口)
- [第十一章 ROS2 开发](#第十一章-ros2-开发)
- [第十二章 MuJoCo 仿真](#第十二章-mujoco-仿真)
- [第十三章 强化学习训练 (unitree_rl_gym)](#第十三章-强化学习训练-unitree_rl_gym)
- [第十四章 导航与 SLAM](#第十四章-导航与-slam)
- [第十五章 扩展硬件开发](#第十五章-扩展硬件开发)
- [第十六章 App 与图形化编程](#第十六章-app-与图形化编程)
- [第十七章 开源项目索引](#第十七章-开源项目索引)
- [第十八章 安全须知与最佳实践](#第十八章-安全须知与最佳实践)
- [第十九章 常见问题与故障排查](#第十九章-常见问题与故障排查)
- [附录 A: DDS Topic 速查表](#附录-a-dds-topic-速查表)
- [附录 B: 关节编号与限位速查表](#附录-b-关节编号与限位速查表)
- [附录 C: 高层运动控制接口速查表](#附录-c-高层运动控制接口速查表)

---

## 第一章 Go2 产品概述

### 1.1 产品定位

宇树 Go2 是宇树科技（Unitree Robotics）推出的**新一代交互仿生四足机器人**。作为 Go1 的全面升级版，Go2 在感知系统、运动能力、AI 智能和二次开发能力上都有显著提升。Go2 系列提供 AIR、PRO、X、EDU 四个版本，覆盖消费级到科研级需求。

### 1.2 产品线对比

| 特性 | AIR | PRO | X | EDU |
|------|-----|-----|---|-----|
| **定价** | ¥9,997 | ¥18,600 | ¥29,999 | 联系销售 |
| **最大速度** | 2.5 m/s | 3.5 m/s | 3.7 m/s（极限~5m/s） | 3.7 m/s（极限~5m/s） |
| **载荷** | ≈7kg（极限~10kg） | ≈8kg（极限~10kg） | ≈8kg（极限~12kg） | ≈8kg（极限~12kg） |
| **基础算力** | - | 8核高性能 CPU | 8核高性能 CPU | 8核高性能 CPU |
| **最大关节扭矩** | - | 约 45N.m | 约 45N.m | 约 45N.m |
| **足端力传感器** | ✗ | ✗ | ✗ | ✓ |
| **4G 模组（带 GPS）** | ✗ | ✓ | ✓ | ✓ |
| **语音功能** | ✗ | ✓ | ✓ | ✓ |
| **ISS 2.0 智能伴随** | ✗ | ✓ | ✓ | ✓ |
| **充电桩支持** | ✗ | ✗ | ✓ | ✓ |
| **二次开发** | ✗ | ✗ | 基础支持 | **完整支持** |
| **高算力模组** | ✗ | ✗ | ✗ | 选配 NVIDIA Jetson Orin |
| **深度相机** | ✗ | ✗ | ✗ | ✓ |
| **电池** | 标准 8000mAh | 标准 8000mAh | 标准 8000mAh | 长续航 15000mAh |
| **续航** | 约 1-2h | 约 1-2h | 约 1-2h | 约 2-4h |
| **保修** | 半年 | 1年 | 1年 | 1年 |

> ⚠️ **重要**: 仅 **Go2 EDU** 版本完整支持二次开发。本手册主要针对 EDU 版本编写。

### 1.3 核心亮点

- **4D 超广角激光雷达 L2**: 360° × 96° 半球形感知，最小探测距离 0.05m
- **AI 模式**: 通过大规模 AI 仿真训练，掌握倒立行走、自适应翻身起立、越障攀爬
- **ISS 2.0 智能伴随**: 无线矢量定位，定位精度提升 50%，遥控距离 >30m
- **3D 激光建图**: 使用自研 L2 雷达 + APP 实现点云地图构建与自主导航
- **暴走模式**: 充分释放关节电机潜能，复杂地形运动更快更灵活
- **OTA 升级**: 持续云端更新，不断进化

---

## 第二章 硬件规格与参数

### 2.1 机械参数

| 参数 | 数值 |
|------|------|
| **站立尺寸** | 70cm × 31cm × 40cm |
| **趴下尺寸** | 76cm × 31cm × 20cm |
| **净重（含电池）** | 约 15kg |
| **自由度** | 12（每条腿 3 个关节） |
| **材质** | 铝合金 + 高强度工程塑料 |
| **关节电机数量** | 12 个铝合金精密关节电机 |
| **膝关节内走线** | ✓ |
| **关节热管辅助散热** | ✓ |

### 2.2 运动性能

| 参数 | 数值 |
|------|------|
| **最大速度** | 3.7 m/s（极限约 5m/s，实验室环境） |
| **最大攀爬落差** | 约 16cm |
| **最大爬坡角度** | 40° |
| **工作温度** | 5℃ ~ 35℃ |

### 2.3 电气参数

| 参数 | 数值 |
|------|------|
| **供电电压** | 28V ~ 33.6V |
| **工作最大功率** | 约 3000W |
| **输出电源** | DC 28.8V（电池电压） |
| **连接接口** | RJ45 以太网口 |

### 2.4 关节参数

每条腿包含 3 个关节：

| 关节 | 编号 | 运动范围 |
|------|------|----------|
| **Hip（机身关节）** | Joint 0 | -48° ~ 48° |
| **Thigh（大腿关节）** | Joint 1 | 前腿: -200° ~ 90°，后腿: -260° ~ 30° |
| **Calf（小腿关节）** | Joint 2 | -156° ~ -48° |

**最大关节扭矩**: 约 45N.m（最大关节电机，12 个电机扭矩有差异）

### 2.5 腿部编号

| 编号 | 名称 | 位置 |
|------|------|------|
| **Leg 0** | FR | 右前腿（Front Right） |
| **Leg 1** | FL | 左前腿（Front Left） |
| **Leg 2** | RR | 右后腿（Rear Right） |
| **Leg 3** | RL | 左后腿（Rear Left） |

每条腿的关节命名规则：`{Leg}_{Joint}`，例如 `FR_thigh` 表示右前腿大腿关节。

### 2.6 坐标系与关节旋转

- **机身关节**旋转轴为 **x 轴**
- **大腿关节和小腿关节**旋转轴为 **y 轴**
- 旋转正方向遵循**右手定则**
- 坐标系颜色：红色 = x 轴，绿色 = y 轴，蓝色 = z 轴

### 2.7 感知系统

| 传感器 | 规格 |
|--------|------|
| **4D LiDAR L1**（头部） | 自研 4D 雷达（3D位置+1D灰度），360° × 90° FOV，21600次/秒激光采样，905nm |
| **头部摄像头** | 720p/15fps + 1080p/15fps 双流，F2.2 光圈，120° FOV |
| **IMU** | 加速度计 + 陀螺仪 + 姿态四元数 |
| **足端力传感器** | 4 路（仅 EDU 版本） |
| **超声波** | 障碍检测 |

### 2.8 通信

| 参数 | 规格 |
|------|------|
| **Wi-Fi** | Wi-Fi 6 双频 802.11x |
| **蓝牙** | 5.2 / 4.2 / 2.1 |
| **4G** | 内置 SIM 卡（带 GPS，PRO/X/EDU） |
| **存储** | 64GB |
| **前置照明** | 3W 探照灯 |

### 2.9 电池

| 参数 | 标准电池 BT2-05 | 长续航电池 BT2-06 |
|------|-----------------|-------------------|
| **容量** | 8000mAh / 236.8Wh | 15000mAh / 432Wh |
| **额定电压** | DC 29.6V | DC 28.8V |
| **充电限压** | DC 33.6V | DC 33.6V |
| **重量** | 2.5kg | 2.5kg |
| **运行时间** | 1-2h | 3-5h |
| **充电电流** | 3.5A（慢充） | 9A（快充） |
| **充电时间** | 约 2h | 约 1h15min |

### 2.10 指示灯含义

| 颜色 | 状态 | 含义 |
|------|------|------|
| 绿色闪烁 | 开机中 | 正在启动 |
| 绿色常亮 | 已开机 | 默认避障开启 |
| 蓝色常亮 | - | 避障关闭 |
| 黄色常亮 | - | 续航模式 |
| 紫色常亮 | - | 伴随模式开启 |
| 蓝色慢闪 | - | 电机 & IMU 校准中 |
| 黄色慢闪 | - | 低电量警告，10分钟内自动蹲下 |
| 红色慢闪 | - | 系统异常，需联系售后 |
| 红色快闪 | - | 电机 & IMU 校准失败 |
| 白色常亮 | - | 头部指示灯 |

### 2.11 电气接口

| 接口 | 说明 |
|------|------|
| **电源接口** | DC 28.8V 输出，连接 Orin NX/Nano 高算力模组 |
| **以太网接口** | 标准 RJ45，连接 User PC / Orin NX/Nano |
| **SBUS 接口** | 遥控器通信，不提供供电，引脚定义：NC/GND/SBUS |

### 2.12 背部安装孔位

背部提供标准安装孔位，用于安装扩展坞、LiDAR、机械臂等外设。具体尺寸请参考官方文档中的孔位图。

---

## 第三章 系统架构与通信协议

### 3.1 Go2 EDU 硬件架构

```
┌─────────────────────────────────────────────────────────────┐
│                    Go2 EDU 系统架构                          │
│                                                             │
│  ┌──────────────┐      ┌──────────────┐                    │
│  │  User PC     │◄────►│  机载主板     │                    │
│  │ 192.168.123  │ RJ45 │ 192.168.123  │                    │
│  │    .222      │      │    .161      │                    │
│  └──────────────┘      └──────┬───────┘                    │
│                               │                             │
│                    ┌──────────┼──────────┐                  │
│                    │          │          │                  │
│              ┌─────▼────┐ ┌──▼───┐ ┌───▼────┐            │
│              │ 12个关节  │ │ IMU  │ │ LiDAR  │            │
│              │  电机     │ │      │ │  L1    │            │
│              └──────────┘ └──────┘ └────────┘            │
│                                                             │
│  ┌──────────────┐  可选扩展                                │
│  │ Orin Nano 8G │  ← 通过扩展坞连接                       │
│  │ Orin NX  16G │  ← DC 28.8V 供电                        │
│  └──────────────┘                                          │
└─────────────────────────────────────────────────────────────┘
```

### 3.2 通信协议：DDS（CycloneDDS）

Go2 使用 **CycloneDDS**（版本 0.10.2）作为底层通信协议。DDS 同时也是 ROS2 的通信机制，因此 Go2 天然兼容 ROS2。

**关键概念**:

| 概念 | 说明 |
|------|------|
| **Topic** | 数据通道名称，如 `rt/sportmodestate` |
| **Publisher** | 数据发布者 |
| **Subscriber** | 数据订阅者 |
| **Domain ID** | DDS 域 ID，实机默认为 0，仿真建议为 1 |
| **IDL 类型** | Go2 使用 `unitree_go` IDL 类型 |

### 3.3 网络架构

| 设备 | IP 地址 | 说明 |
|------|---------|------|
| Go2 机载主板 | `192.168.123.161` | **固定不可修改** |
| 用户 PC | `192.168.123.222` | 用户自定义（推荐，"222"可改） |
| 扩展坞 Orin | 由 DHCP 或手动配置 | 通过背部 RJ45 连接 |

> ⚠️ 不允许将用户 PC 的 IP 设为 `192.168.123.161`，这是机器人内部地址。

### 3.4 运动服务架构

```
┌────────────────────────────────────────────┐
│            Go2 运动控制服务层               │
│                                            │
│  ┌─────────────┐    ┌─────────────────┐   │
│  │ MCF 服务     │    │ 用户控制程序     │   │
│  │ (主运动控制) │    │ (SDK/ROS2/自研) │   │
│  └──────┬──────┘    └──────┬──────────┘   │
│         │                   │              │
│         ▼                   ▼              │
│     ┌───────────────────────────┐         │
│     │    12 个关节电机控制器     │         │
│     └───────────────────────────┘         │
│                                            │
│  ⚠️ MCF 和用户程序不能同时控制！          │
│     运行低层控制前必须先关闭 MCF          │
└────────────────────────────────────────────┘
```

**运动控制服务名称**:

| 服务名 | 适用版本 |
|--------|----------|
| `mcf` | 软件版本 ≥ 1.1.6 |
| `sport_mode` | 软件版本 < 1.1.6 |
| `wheeled_sport` | Go2-W 轮足版 |

---

## 第四章 开发环境搭建

### 4.1 系统要求

| 项目 | 要求 |
|------|------|
| **操作系统** | Ubuntu 20.04 或 Ubuntu 22.04 |
| **CPU 架构** | x86_64 或 aarch64 |
| **编译器** | GCC 9.4.0+ |
| **CMake** | 3.10+ |

> ⚠️ 不支持 Mac、Windows 系统开发。不允许在 Go2 内置电脑上开发。

### 4.2 安装基础依赖

```bash
sudo apt update
sudo apt install -y cmake gcc g++ build-essential \
    libeigen3-dev libyaml-cpp-dev libboost-all-dev \
    libspdlog-dev libfmt-dev
```

### 4.3 网络配置

#### 步骤 1: 物理连接

用网线连接 Go2 和你的电脑。

#### 步骤 2: 配置 IP

打开网络设置，找到连接机器人的网卡，将 IPv4 设为手动模式：

- **IP 地址**: `192.168.123.222`（"222"可自定义，但不能是 161）
- **子网掩码**: `255.255.255.0`

#### 步骤 3: 验证连接

```bash
ping 192.168.123.161
```

看到正常回应即表示连接成功。

#### 步骤 4: 查看网卡名

```bash
ifconfig
```

找到 IP 为 `192.168.123.222` 的网卡名（如 `enxf8e43b808e06`、`enp3s0` 等），**记住这个名字**，后续运行程序时需要。

### 4.4 安装 unitree_sdk2（C++）

```bash
cd ~
git clone https://github.com/unitreerobotics/unitree_sdk2.git
cd unitree_sdk2
mkdir build && cd build

# 安装到默认路径
cmake ..
sudo make install

# 或安装到指定路径
cmake .. -DCMAKE_INSTALL_PREFIX=/opt/unitree_robotics
sudo make install
```

### 4.5 安装 unitree_sdk2_python（Python）

```bash
cd ~
sudo apt install python3-pip
git clone https://github.com/unitreerobotics/unitree_sdk2_python.git
cd unitree_sdk2_python
pip3 install -e .
```

如果遇到 `Could not locate cyclonedds` 错误，需要设置 `CYCLONEDDS_HOME` 或 `CMAKE_PREFIX_PATH` 环境变量。参考: https://github.com/unitreerobotics/unitree_sdk2_python

### 4.6 编译 SDK 示例

```bash
cd ~/unitree_sdk2
mkdir build && cd build
cmake ..
make
```

`make` 完成后，生成的可执行文件位于 `build/bin` 目录。

---

## 第五章 unitree_sdk2 C++ 开发

### 5.1 SDK 概述

`unitree_sdk2` 是基于 CycloneDDS 实现的 C++ SDK，提供：

- **低层控制**: 直接控制 12 个关节电机的位置、速度、力矩
- **高层控制**: 通过 `SportClient` 发送运动指令（行走、翻转、跳跃等）
- **状态订阅**: 订阅 DDS Topic 获取机器人实时状态
- **服务调用**: 通过 RPC 调用各种服务接口

### 5.2 项目结构

```
unitree_sdk2/
├── include/                     # 头文件
│   └── unitree/
│       ├── robot/
│       │   ├── channel/         # DDS Channel 通信
│       │   └── go2/
│       │       └── sport/       # Go2 SportClient
│       └── idl/
│           ├── go2/             # Go2 IDL 消息定义
│           └── ...
├── example/                     # 示例代码
│   └── go2/
│       ├── go2_low_level.cpp    # 低层控制示例
│       ├── go2_stand_example.cpp # 站立示例
│       └── ...
├── thirdparty/                  # 第三方库
│   └── cyclonedds/              # CycloneDDS
├── CMakeLists.txt
└── README.md
```

### 5.3 初始化 SDK

```cpp
#include <unitree/robot/channel/channel_factory.hpp>

int main(int argc, char **argv) {
    if (argc < 2) {
        std::cout << "Usage: " << argv[0] << " networkInterface" << std::endl;
        exit(-1);
    }
    
    // 初始化 SDK，参数: domain_id, 网卡名
    // 连接实机: domain_id=0, 网卡=实际网卡名
    // 连接仿真: domain_id=1, 网卡="lo"
    unitree::robot::ChannelFactory::Instance()->Init(0, argv[1]);
    
    // ... 你的代码
    return 0;
}
```

### 5.4 CMake 集成

创建你自己的项目时，`CMakeLists.txt` 参考：

```cmake
cmake_minimum_required(VERSION 3.10)
project(my_go2_project)

find_package(unitree_sdk2 REQUIRED)

add_executable(my_app main.cpp)
target_link_libraries(my_app unitree_sdk2::unitree_sdk2)
```

> 如果 `unitree_sdk2` 安装在非默认路径，需要将安装路径加入 `CMAKE_PREFIX_PATH`。

### 5.5 运行示例

```bash
# 低层控制示例（会导致腿摆动，请确保四腿悬空！）
cd ~/unitree_sdk2/build/bin
./go2_low_level enxf8e43b808e06

# 站立示例（机器人应静止趴在地上再运行）
./go2_stand_example enxf8e43b808e06
```

> ⚠️ 运行低层控制前，**必须先关闭 MCF 主运动控制服务**。可以通过 App → 设备 → 服务状态 关闭，或通过 MotionSwitcherClient 接口关闭。

---

## 第六章 Python SDK 开发

### 6.1 概述

`unitree_sdk2_python` 是 `unitree_sdk2` 的 Python 封装，提供与 C++ SDK 相同的功能，适合快速原型开发和 AI 应用集成。

### 6.2 安装

```bash
cd ~
git clone https://github.com/unitreerobotics/unitree_sdk2_python.git
cd unitree_sdk2_python
pip3 install -e .
```

### 6.3 基础用法

```python
import sys
from unitree_sdk2py.core.channel import ChannelFactory

# 初始化
if len(sys.argv) < 2:
    # 仿真模式
    ChannelFactory.Instance().Init(1, "lo")
else:
    # 实机模式
    ChannelFactory.Instance().Init(0, sys.argv[1])
```

### 6.4 Sim2Real 切换模式

Python SDK 的 Sim2Real 切换非常简单，只需修改初始化参数：

```python
import sys
from unitree_sdk2py.core.channel import ChannelFactoryInitialize

if len(sys.argv) < 2:
    # 无网卡参数 → 仿真模式
    ChannelFactoryInitialize(1, "lo")     # domain_id=1, 网卡=lo
else:
    # 有网卡参数 → 实机模式
    ChannelFactoryInitialize(0, sys.argv[1])  # domain_id=0, 网卡=实际名
```

运行：
```bash
# 仿真（确保 MuJoCo 仿真器已启动）
python3 my_app.py

# 实机
python3 my_app.py enp3s0
```

---

## 第七章 低层电机控制

### 7.1 概述

低层控制直接操控 12 个关节电机，适用于：
- 自定义步态算法
- 强化学习策略部署
- 电机特性测试
- 研究型开发

> ⚠️ **运行低层控制前必须关闭 MCF 服务！** 否则两个控制源同时发送指令会导致机器人失控。

### 7.2 电机命令 (LowCmd)

通过发布 `unitree_go::msg::LowCmd` 消息到 `/lowcmd` Topic 来控制电机。

```cpp
// LowCmd 结构
struct LowCmd {
    uint8_t head[2];
    uint8_t level_flag;
    uint8_t frame_reserve;
    uint32_t sn[2];
    uint32_t version[2];
    uint16_t bandwidth;
    MotorCmd motor_cmd[20];  // 电机命令（使用前12个）
    BmsCmd bms_cmd;
    uint8_t wireless_remote[40];
    uint8_t led[12];
    uint8_t fan[2];
    uint8_t gpio;
    uint32_t reserve;
    uint32_t crc;
};
```

### 7.3 电机命令结构 (MotorCmd)

```cpp
struct MotorCmd {
    uint8_t mode;   // 模式: 0x01=FOC模式, 0x00=停止模式
    float q;        // 目标位置 (rad)
    float dq;       // 目标速度 (rad/s)
    float tau;      // 目标力矩 (N.m)
    float kp;       // 位置环增益
    float kd;       // 速度环增益
    unsigned long reserve[3];
};
```

**电机控制公式**:

$$\tau_{output} = kp \times (q_{target} - q_{current}) + kd \times (dq_{target} - dq_{current}) + \tau_{feedforward}$$

### 7.4 电机编号映射

| 电机编号 | 关节名称 | 说明 |
|----------|----------|------|
| 0 | FR_hip | 右前腿-机身 |
| 1 | FR_thigh | 右前腿-大腿 |
| 2 | FR_calf | 右前腿-小腿 |
| 3 | FL_hip | 左前腿-机身 |
| 4 | FL_thigh | 左前腿-大腿 |
| 5 | FL_calf | 左前腿-小腿 |
| 6 | RR_hip | 右后腿-机身 |
| 7 | RR_thigh | 右后腿-大腿 |
| 8 | RR_calf | 右后腿-小腿 |
| 9 | RL_hip | 左后腿-机身 |
| 10 | RL_thigh | 左后腿-大腿 |
| 11 | RL_calf | 左后腿-小腿 |

### 7.5 C++ 低层控制示例

```cpp
#include <unitree/idl/go2/LowCmd_.hpp>
#include <unitree/idl/go2/LowState_.hpp>
#include <unitree/robot/channel/channel_publisher.hpp>
#include <unitree/robot/channel/channel_subscriber.hpp>

#define TOPIC_LOWCMD "rt/lowcmd"
#define TOPIC_LOWSTATE "rt/lowstate"

using namespace unitree::robot;

// 状态回调
void LowStateHandler(const void* message) {
    auto state = *(unitree_go::msg::dds_::LowState_*)message;
    // 获取第0号电机（FR_hip）的状态
    float q = state.motor_state()[0].q();          // 角度
    float dq = state.motor_state()[0].dq();         // 角速度
    float tau = state.motor_state()[0].tau_est();    // 估计力矩
    int8_t temp = state.motor_state()[0].temperature(); // 温度
}

int main(int argc, char** argv) {
    ChannelFactory::Instance()->Init(0, argv[1]);
    
    // 创建发布者和订阅者
    ChannelPublisher<unitree_go::msg::dds_::LowCmd_> pub(TOPIC_LOWCMD);
    pub.InitChannel();
    
    ChannelSubscriber<unitree_go::msg::dds_::LowState_> sub(TOPIC_LOWSTATE);
    sub.InitChannel(LowStateHandler);
    
    // 构造控制命令
    unitree_go::msg::dds_::LowCmd_ cmd;
    cmd.motor_cmd()[0].mode() = 0x01;  // FOC 模式
    cmd.motor_cmd()[0].q() = 0.0;      // 目标角度
    cmd.motor_cmd()[0].dq() = 0.0;     // 目标角速度
    cmd.motor_cmd()[0].tau() = 1.0;    // 前馈力矩 1 N.m
    cmd.motor_cmd()[0].kp() = 0.0;
    cmd.motor_cmd()[0].kd() = 0.0;
    
    pub.Write(cmd);
    return 0;
}
```

### 7.6 安全提示

1. **首次测试时务必将机器人四腿悬空**
2. 先用小力矩测试，确认方向和范围正确
3. 设置合理的 kp/kd 增益，避免关节振荡
4. 随时准备好按急停或切断电源

---

## 第八章 高层运动控制

### 8.1 概述

高层运动控制通过 `SportClient` 实现，提供丰富的运动指令，包括：行走、站立、舞蹈、翻转、跳跃等。

### 8.2 SportClient 初始化

```cpp
#include <unitree/robot/go2/sport/sport_client.hpp>

int main(int argc, char** argv) {
    unitree::robot::ChannelFactory::Instance()->Init(0, argv[1]);
    
    unitree::robot::go2::SportClient sport_client;
    sport_client.SetTimeout(10.0f);  // 设置超时时间
    sport_client.Init();             // 初始化
    
    // 使用各种运动指令...
    return 0;
}
```

### 8.3 基础运动指令

#### 站立与蹲下

```cpp
sport_client.StandUp();       // 站高（关节锁定，默认高度 0.33m）
sleep(3);
sport_client.StandDown();     // 蹲下（关节锁定）
sleep(3);
sport_client.RecoveryStand(); // 从任意姿态恢复站立
sport_client.BalanceStand();  // 平衡站立（姿态随地形自适应）
```

#### 速度控制

```cpp
// Move(vx, vy, vyaw) - 速度控制
// vx: 前进速度 [-2.5 ~ 3.8] m/s
// vy: 横向速度 [-1.0 ~ 1.0] m/s
// vyaw: 偏航角速度 [-4 ~ 4] rad/s
sport_client.Move(0.5, 0.0, 0.0);  // 向前走 0.5 m/s
sport_client.Move(0.0, 0.3, 0.0);  // 向左横移 0.3 m/s
sport_client.Move(0.0, 0.0, 0.5);  // 原地左转
sport_client.StopMove();            // 停止运动
```

#### 姿态控制

```cpp
// Euler(roll, pitch, yaw) - 姿态控制
// roll:  [-0.75 ~ 0.75] rad
// pitch: [-0.75 ~ 0.75] rad
// yaw:   [-0.6 ~ 0.6] rad
sport_client.Euler(0.2, 0.0, 0.0);   // 右侧倾斜
sport_client.BalanceStand();          // 需要在平衡站立模式下
```

#### 高度与步高

```cpp
// BodyHeight(height) - 相对默认体高的偏移
// height: [-0.18 ~ 0.03] m
// 默认体高 = 0.33m，例如 BodyHeight(-0.1) → 实际高度 0.23m
sport_client.BodyHeight(-0.1);

// FootRaiseHeight(height) - 相对默认抬脚高的偏移
// height: [-0.06 ~ 0.03] m
// 默认抬脚高 = 0.09m，例如 FootRaiseHeight(0.01) → 实际 0.1m
sport_client.FootRaiseHeight(0.01);
```

### 8.4 步态切换

```cpp
// SwitchGain(d) - 切换步态
// 0: idle 空闲
// 1: trot 快步
// 2: trot running 快跑
// 3: forward climbing 正面爬梯
// 4: reverse climbing 背面爬梯
sport_client.SwitchGain(1);  // 切换到 trot 步态
```

### 8.5 速度档位

```cpp
// SpeedLevel(level)
// -1: 慢速
//  0: 正常速度
//  1: 快速
sport_client.SpeedLevel(1);  // 切换到快速模式
```

### 8.6 特殊动作

```cpp
sport_client.Hello();        // 打招呼（挥手）
sport_client.Stretch();      // 伸懒腰
sport_client.Sit();          // 坐下
sport_client.RiseSit();      // 从坐姿恢复站立

sport_client.Wallow();       // 打滚
sport_client.Pose(true);     // 摆姿势（true 开始，false 恢复）
sport_client.Scrape();       // 拜年

sport_client.FrontFlip();    // 前空翻
sport_client.FrontJump();    // 前跳
sport_client.FrontPounce();  // 前扑

sport_client.Dance1();       // 舞蹈第 1 段
sport_client.Dance2();       // 舞蹈第 2 段
```

> ⚠️ 特殊动作需要在前一个动作完成后再执行下一个，否则可能导致动作异常。

### 8.7 轨迹跟踪

```cpp
// trajectoryFollow(path) - 轨迹跟踪
// path: 30 个 PathPoint 组成的轨迹
std::vector<PathPoint> path(30);
for (int i = 0; i < 30; i++) {
    path[i].tFromStart = i * 0.1;   // 时间
    path[i].x = i * 0.05;           // x 位置
    path[i].y = 0.0;                // y 位置
    path[i].yaw = 0.0;              // 偏航角
    path[i].vx = 0.5;               // x 速度
    path[i].vy = 0.0;               // y 速度
    path[i].vyaw = 0.0;             // 偏航角速度
}
sport_client.TrajectoryFollow(path);
```

### 8.8 遥控器控制

```cpp
// SwitchJoystick(flag) - 原生遥控器响应开关
sport_client.SwitchJoystick(false);  // 关闭遥控器响应
sport_client.SwitchJoystick(true);   // 恢复遥控器响应
```

### 8.9 连续步态

```cpp
// ContinuousGait(flag) - 连续步态模式
sport_client.ContinuousGait(true);   // 开启：即使速度为0也保持抬腿
sport_client.ContinuousGait(false);  // 关闭
```

### 8.10 紧急停止

```cpp
sport_client.Damp();  // 进入阻尼模式（最高优先级急停！）
```

> ⚠️ `Damp()` 是最高优先级命令，所有电机立即停止并进入阻尼状态。**用于紧急情况！**

### 8.11 姿态控制完整示例

```cpp
#include <cmath>
#include <signal.h>
#include <unistd.h>
#include <unitree/robot/go2/sport/sport_client.hpp>

bool stopped = false;

void sigint_handler(int sig) {
    if (sig == SIGINT) stopped = true;
}

int main(int argc, char** argv) {
    if (argc < 2) {
        std::cout << "Usage: " << argv[0] << " networkInterface" << std::endl;
        return -1;
    }
    
    unitree::robot::ChannelFactory::Instance()->Init(0, argv[1]);
    
    unitree::robot::go2::SportClient sport_client;
    sport_client.SetTimeout(10.0f);
    sport_client.Init();
    
    double t = 0, dt = 0.01;
    signal(SIGINT, sigint_handler);
    
    while (!stopped) {
        t += dt;
        // 姿态跟踪三角函数轨迹
        sport_client.Euler(0.2 * sin(2 * t), 0.2 * cos(2 * t) - 0.2, 0.0);
        sport_client.BalanceStand();
        usleep(int(dt * 1000000));
    }
    
    // 退出时复位姿态
    sport_client.Euler(0, 0, 0);
    return 0;
}
```

---

## 第九章 传感器数据获取

### 9.1 运动状态（SportModeState）

通过订阅 `rt/sportmodestate`（实时）或 `lf/sportmodestate`（低频）Topic：

```cpp
#include <unitree/idl/go2/SportModeState_.hpp>
#include <unitree/robot/channel/channel_subscriber.hpp>

#define TOPIC_HIGHSTATE "rt/sportmodestate"

void HighStateHandler(const void* message) {
    auto state = *(unitree_go::msg::dds_::SportModeState_*)message;
    
    // 位置 (x, y, z)
    float x = state.position()[0];
    float y = state.position()[1];
    float z = state.position()[2];
    
    // 姿态四元数 (w, x, y, z)
    float qw = state.imu_state().quaternion()[0];
    float qx = state.imu_state().quaternion()[1];
    float qy = state.imu_state().quaternion()[2];
    float qz = state.imu_state().quaternion()[3];
    
    // 速度 (vx, vy, vz)
    float vx = state.velocity()[0];
    float vy = state.velocity()[1];
    float vz = state.velocity()[2];
    
    // 偏航角速度
    float yawSpeed = state.yaw_speed();
    
    // 体高
    float bodyHeight = state.body_height();
    
    // 足力
    int16_t footForce[4];
    for (int i = 0; i < 4; i++)
        footForce[i] = state.foot_force()[i];
    
    // 足端相对身体的位置 (12个值: 4腿 × 3坐标)
    for (int i = 0; i < 12; i++)
        float fp = state.foot_position_body()[i];
    
    // 足端相对身体的速度 (12个值)
    for (int i = 0; i < 12; i++)
        float fs = state.foot_speed_body()[i];
    
    // 障碍物距离 (4方向)
    for (int i = 0; i < 4; i++)
        float obs = state.range_obstacle()[i];
}

int main() {
    unitree::robot::ChannelFactory::Instance()->Init(0, "enp3s0");
    
    ChannelSubscriber<unitree_go::msg::dds_::SportModeState_> sub(TOPIC_HIGHSTATE);
    sub.InitChannel(HighStateHandler);
    
    while (true) usleep(20000);
    return 0;
}
```

### 9.2 SportModeState 运动模式值

| 值 | 模式 | 说明 |
|----|------|------|
| 0 | idle | 默认站立 |
| 1 | balanceStand | 平衡站立 |
| 2 | pose | 摆姿势 |
| 3 | locomotion | 运动 |
| 5 | lieDown | 趴下 |
| 6 | jointLock | 关节锁定 |
| 7 | damping | 阻尼状态 |
| 8 | recoveryStand | 恢复站立 |
| 10 | sit | 坐下 |
| 11 | frontFlip | 前空翻 |
| 12 | frontJump | 前跳 |
| 13 | frontPounce | 前扑 |

### 9.3 步态类型值

| 值 | 步态 |
|----|------|
| 0 | idle 空闲 |
| 1 | trot 快步 |
| 2 | run 奔跑 |
| 3 | climb stair 爬梯 |
| 4 | forwardDownStair 下梯 |
| 9 | adjust 调整 |

### 9.4 低层状态（LowState）

通过订阅 `rt/lowstate` 或 `lf/lowstate` Topic：

```cpp
// LowState 包含:
struct LowState {
    IMUState imu_state;          // IMU 状态
    MotorState motor_state[20];  // 电机状态（前12个有效）
    BmsState bms_state;          // 电池管理系统
    int16_t foot_force[4];       // 足力传感器
    int16_t foot_force_est[4];   // 估计足力
    float power_v;               // 供电电压
    float power_a;               // 供电电流
    int8_t temperature_ntc1;     // 温度传感器1
    int8_t temperature_ntc2;     // 温度传感器2
    // ...
};

// MotorState 结构
struct MotorState {
    uint8_t mode;       // 模式，0x01 为控制模式
    float q;            // 关节角度 (rad)
    float dq;           // 关节角速度 (rad/s)
    float ddq;          // 关节角加速度
    float tau_est;      // 估计力矩 (N.m)
    float q_raw;        // 原始角度
    float dq_raw;       // 原始角速度
    float ddq_raw;      // 原始角加速度
    int8_t temperature; // 电机温度
    uint32_t lost;      // 丢包计数
};
```

### 9.5 IMU 数据

```cpp
// 从 imu_state 获取
std::array<float, 4> quaternion;     // 四元数 (w, x, y, z)
std::array<float, 3> gyroscope;      // 角速度 (rad/s)
std::array<float, 3> accelerometer;  // 加速度 (m/s²)
std::array<float, 3> rpy;            // 欧拉角 Roll/Pitch/Yaw (rad)
int8_t temperature;                  // 温度
```

### 9.6 无线遥控器状态

通过订阅 `/wirelesscontroller` Topic：

```cpp
struct WirelessController {
    float lx;      // 左摇杆 X [-1.0 ~ 1.0]
    float ly;      // 左摇杆 Y [-1.0 ~ 1.0]
    float rx;      // 右摇杆 X [-1.0 ~ 1.0]
    float ry;      // 右摇杆 Y [-1.0 ~ 1.0]
    uint16_t keys; // 按键值（位掩码）
};
```

### 9.7 获取当前运动状态

```cpp
// 通过 SportClient 获取状态
std::vector<std::string> names = {"state", "bodyHeight", "footRaiseHeight", 
                                   "speedLevel", "gaitType", "joystick",
                                   "dance", "continuousGait", "economicGait"};
std::map<std::string, std::string> status;

sport_client.GetState(names, status);
// 例如当机器人在阻尼状态时：status["state"] = "damping"
```

---

## 第十章 运动服务切换接口

### 10.1 概述

`MotionSwitcherClient` 用于在不同运动控制模式之间切换。这是运行低层控制程序前关闭默认运动服务的关键接口。

### 10.2 运动控制模式名称

| 名称 | 适用版本 |
|------|----------|
| `mcf` | 软件版本 ≥ 1.1.6 |
| `ai` (AI模式)、`normal` (普通)、`advanced` (前沿) | 软件版本 < 1.1.6 |

### 10.3 接口说明

```cpp
#include <unitree/robot/go2/motion_switcher/motion_switcher_client.hpp>

unitree::robot::go2::MotionSwitcherClient switcher;
switcher.SetTimeout(10.0f);
switcher.Init();

// 检查当前模式
std::string form, name;
switcher.CheckMode(form, name);
// form: "0"=标准形态, "1"=轮足形态
// name: 当前运动控制模式名

// 选择运动控制模式
switcher.SelectMode("mcf");  // 启用 MCF 服务

// 关闭运动控制模式（运行低层控制前调用）
switcher.ReleaseMode();

// 设置静默状态（Go2 启动时不自动启动默认运动控制）
switcher.SetSilent(true);   // 设置静默
switcher.SetSilent(false);  // 取消静默

// 获取静默状态
bool silent;
switcher.GetSilent(silent);
```

### 10.4 错误码

| 错误码 | 说明 |
|--------|------|
| 7001 | 请求参数错误 |
| 7002 | 切换服务忙，请稍后重试 |
| 7004 | 不支持的运动控制模式名 |
| 7005 | 内部命令执行错误 |
| 7006 | 检测命令执行错误 |
| 7007 | 切换命令执行错误 |
| 7008 | 释放命令执行错误 |
| 7009 | 自定义配置错误 |

### 10.5 go2_stand_example 工作流程

`go2_stand_example` 示例展示了完整的运动切换流程：

1. 初始化 SDK
2. 通过 `MotionSwitcherClient` 关闭 `sport_mode`/`mcf`
3. 发送低层电机命令让机器人站立
4. 等待一段时间
5. 恢复趴下

```bash
# 运行（机器人应已趴在地上）
./go2_stand_example enp3s0
```

---

## 第十一章 ROS2 开发

### 11.1 概述

由于 Go2 底层使用 CycloneDDS，与 ROS2 天然兼容。可以直接使用 ROS2 msg 与 Go2 通信，无需额外封装。

### 11.2 系统要求

| 系统 | ROS2 版本 |
|------|-----------|
| Ubuntu 20.04 | Foxy |
| Ubuntu 22.04 | Humble |

### 11.3 安装 ROS2 Foxy

参考官方文档: https://docs.ros.org/en/foxy/Installation/Ubuntu-Install-Debians.html

### 11.4 安装 Unitree ROS2 包

#### 步骤 1: 克隆仓库

```bash
cd ~
git clone https://github.com/unitreerobotics/unitree_ros2
```

目录结构：
- `cyclonedds_ws/`: Unitree ROS2 包工作空间
  - `unitree_go`: Go2 消息定义
  - `unitree_api`: API 消息定义
- `Go2_ROS2_example/`: 示例工作空间

#### 步骤 2: 安装依赖

```bash
sudo apt install ros-foxy-rmw-cyclonedds-cpp
sudo apt install ros-foxy-rosidl-generator-dds-idl
```

#### 步骤 3: 编译 CycloneDDS

> ⚠️ **关键**: 编译 CycloneDDS 时**不要** source ROS2 环境！如果 `~/.bashrc` 中有 `source /opt/ros/foxy/setup.bash`，需要先注释掉。

```bash
# 注释掉 bashrc 中的 ros2 source（如果有的话）
sudo gedit ~/.bashrc
# 注释: # source /opt/ros/foxy/setup.bash

# 编译 CycloneDDS（版本必须是 0.10.x）
cd ~/unitree_ros2/cyclonedds_ws/src
git clone https://github.com/ros2/rmw_cyclonedds -b foxy
git clone https://github.com/eclipse-cyclonedds/cyclonedds -b releases/0.10.x
cd ..
colcon build --packages-select cyclonedds
```

#### 步骤 4: 编译消息包

```bash
source /opt/ros/foxy/setup.bash
cd ~/unitree_ros2/cyclonedds_ws
colcon build
```

### 11.5 网络配置

创建/编辑 `~/unitree_ros2/setup.sh`:

```bash
#!/bin/bash
echo "Setup unitree ros2 environment"
source /opt/ros/foxy/setup.bash
source $HOME/unitree_ros2/cyclonedds_ws/install/setup.bash
export RMW_IMPLEMENTATION=rmw_cyclonedds_cpp
export CYCLONEDDS_URI='<CycloneDDS><Domain><General><Interfaces>
    <NetworkInterface name="enp3s0" priority="default" multicast="default" />
</Interfaces></General></Domain></CycloneDDS>'
```

> 将 `enp3s0` 替换为你连接机器人的实际网卡名。

```bash
source ~/unitree_ros2/setup.sh
```

**仿真模式**（不连接机器人）:
```bash
source ~/unitree_ros2/setup_local.sh    # 使用 "lo" 本地回环
# 或
source ~/unitree_ros2/setup_default.sh  # 不指定网卡
```

### 11.6 验证连接

```bash
source ~/unitree_ros2/setup.sh

# 列出所有 Topic
ros2 topic list

# 查看运动状态数据
ros2 topic echo /sportmodestate
```

### 11.7 编译示例

```bash
source ~/unitree_ros2/setup.sh
cd ~/unitree_ros2/example
colcon build
```

### 11.8 ROS2 读取运动状态示例

```bash
./install/unitree_ros2_example/bin/read_motion_state
```

输出：
```
[INFO] Position -- x: 0.567; y: 0.214; z: 0.052; body height: 0.320
[INFO] Velocity -- vx: -0.009; vy: -0.001; vz: -0.019; yaw: -0.002
[INFO] Foot position and velocity relative to body -- num: 0; x: 0.204; ...
[INFO] Gait state -- gait type: 1; raise height: 0.090
```

### 11.9 ROS2 运动控制示例

```cpp
// 创建发布者
rclcpp::Publisher<unitree_api::msg::Request>::SharedPtr req_puber = 
    this->create_publisher<unitree_api::msg::Request>("/api/sport/request", 10);

SportClient sport_req;
unitree_api::msg::Request req;

// 获取运动请求消息
sport_req.Euler(req, roll, pitch, yaw);

// 发布
req_puber->publish(req);
```

运行：
```bash
./install/unitree_ros2_example/bin/sport_mode_ctrl
# 程序启动 1 秒后，机器人会在 x 方向来回走动
```

### 11.10 ROS2 低层电机控制

```bash
./install/unitree_ros2_example/bin/low_level_ctrl
# RL 腿的 hip 和 calf 电机将旋转到指定角度
```

### 11.11 Rviz2 可视化

```bash
# 列出 Topic
ros2 topic list
# 找到激光雷达 Topic: utlidar/cloud

# 查看 frame_id
ros2 topic echo --no-arr /utlidar/cloud
# frame_id: utlidar_lidar

# 启动 Rviz2
ros2 run rviz2 rviz2
```

在 Rviz2 中：
1. 添加 PointCloud2 Topic: `utlidar/cloud`
2. 修改 Fixed Frame 为 `utlidar_lidar`
3. 即可看到激光雷达点云数据

---

## 第十二章 MuJoCo 仿真

### 12.1 概述

`unitree_mujoco` 是基于 MuJoCo 物理引擎和 Unitree SDK2 的仿真器。它支持 C++ 和 Python 两个版本，可以无缝对接 `unitree_sdk2`、`unitree_ros2` 和 `unitree_sdk2_python` 开发的程序。

**核心优势**: **仿真和实机使用完全相同的代码**，只需修改初始化参数（domain_id 和网卡名）。

**GitHub**: https://github.com/unitreerobotics/unitree_mujoco (⭐866)

### 12.2 支持的消息

当前版本主要支持低层开发：

| 消息类型 | 说明 |
|----------|------|
| `LowCmd` | 电机控制命令 |
| `LowState` | 电机状态 |
| `SportModeState` | 位置和速度（仿真中保留） |

> Go2 使用 `unitree_go` IDL 类型。

### 12.3 目录结构

```
unitree_mujoco/
├── simulate/           # C++ 仿真器（推荐）
├── simulate_python/    # Python 仿真器
├── unitree_robots/     # MJCF 机器人描述文件
│   └── go2/
│       └── scene.xml   # Go2 场景文件
├── terrain_tool/       # 地形生成工具
└── example/            # Sim2Real 示例
    ├── cpp/            # C++ 示例（unitree_sdk2）
    ├── python/         # Python 示例（unitree_sdk2_python）
    └── ros2/           # ROS2 示例
```

### 12.4 C++ 仿真器安装

#### 安装依赖

```bash
# 系统依赖
sudo apt install libyaml-cpp-dev libspdlog-dev libboost-all-dev libglfw3-dev

# 安装 unitree_sdk2（建议安装到 /opt/unitree_robotics）
git clone https://github.com/unitreerobotics/unitree_sdk2.git
cd unitree_sdk2 && mkdir build && cd build
cmake .. -DCMAKE_INSTALL_PREFIX=/opt/unitree_robotics
sudo make install
cd ../..

# 下载 MuJoCo
mkdir -p ~/.mujoco
cd ~/.mujoco
# 从 https://github.com/google-deepmind/mujoco/releases 下载
# 例如 mujoco-3.3.6
wget https://github.com/google-deepmind/mujoco/releases/download/3.3.6/mujoco-3.3.6-linux-x86_64.tar.gz
tar xzf mujoco-3.3.6-linux-x86_64.tar.gz
```

#### 编译

```bash
cd unitree_mujoco/simulate
ln -s ~/.mujoco/mujoco-3.3.6 mujoco
mkdir build && cd build
cmake ..
make -j4
```

#### 运行

```bash
# 启动 Go2 仿真
./unitree_mujoco -r go2 -s scene_terrain.xml

# 在另一个终端运行测试
./test
# 输出位姿信息，每个电机持续输出 1Nm 力矩
```

### 12.5 Python 仿真器安装

```bash
# 安装依赖
pip3 install mujoco pygame

# 安装 unitree_sdk2_python
cd ~
git clone https://github.com/unitreerobotics/unitree_sdk2_python.git
cd unitree_sdk2_python && pip3 install -e .

# 运行
cd unitree_mujoco/simulate_python
python3 unitree_mujoco.py

# 另一个终端
python3 test/test_unitree_sdk2.py
```

### 12.6 仿真器配置

#### C++ 配置 (`simulate/config.yaml`)

```yaml
robot: "go2"                    # 机器人名称
robot_scene: "scene.xml"        # 场景文件
domain_id: 1                    # DDS 域 ID（仿真建议为 1）
interface: "lo"                 # 网卡（仿真用 lo）
use_joystick: 1                 # 使用手柄模拟遥控器
joystick_type: "xbox"           # xbox 或 switch
joystick_device: "/dev/input/js0"
print_scene_information: 1      # 输出场景信息
enable_elastic_band: 0          # 弹性绑带（人形机器人用）
```

#### Python 配置 (`simulate_python/config.py`)

```python
ROBOT = "go2"
ROBOT_SCENE = "../unitree_robots/go2/scene.xml"
DOMAIN_ID = 1
INTERFACE = "lo"
PRINT_SCENE_INFORMATION = True
USE_JOYSTICK = 1
JOYSTICK_TYPE = "xbox"
SIMULATE_DT = 0.003    # 仿真时间步 (s)
VIEWER_DT = 0.02       # 可视化步长 (50fps)
ENABLE_ELASTIC_BAND = False
```

### 12.7 手柄模拟遥控器

仿真器支持 Xbox / Switch 手柄模拟无线遥控器，按钮信息通过 `rt/wireless_controller` Topic 发布。

如果没有手柄，将 `use_joystick`/`USE_JOYSTICK` 设为 0。

### 12.8 地形生成工具

`terrain_tool` 文件夹提供参数化地形生成工具，支持：
- **楼梯**
- **粗糙地面**
- **高度图**

### 12.9 Sim2Real 示例

`example` 文件夹中的示例展示了如何用同一份代码控制仿真和实机：

#### C++ (unitree_sdk2)

```bash
cd example/cpp && mkdir build && cd build && cmake .. && make -j4

# 仿真
./stand_go2

# 实机
./stand_go2 enp3s0
```

关键代码：
```cpp
if (argc < 2) {
    ChannelFactory::Instance()->Init(1, "lo");     // 仿真
} else {
    ChannelFactory::Instance()->Init(0, argv[1]);  // 实机
}
```

#### Python

```bash
python3 ./stand_go2.py         # 仿真
python3 ./stand_go2.py enp3s0  # 实机
```

#### ROS2

```bash
# 仿真
source ~/unitree_ros2/setup_local.sh
export ROS_DOMAIN_ID=1
./install/stand_go2/bin/stand_go2

# 实机
source ~/unitree_ros2/setup.sh
export ROS_DOMAIN_ID=0
./install/stand_go2/bin/stand_go2
```

---

## 第十三章 强化学习训练 (unitree_rl_gym)

### 13.1 概述

`unitree_rl_gym` 是宇树官方的强化学习训练框架，支持 Go2、G1、H1、H1-2 机器人。基于 Isaac Gym 和 legged_gym，提供完整的 **Train → Play → Sim2Sim → Sim2Real** 工作流。

**GitHub**: https://github.com/unitreerobotics/unitree_rl_gym (⭐3k)

### 13.2 工作流程

```
Train（Isaac Gym）→ Play（验证）→ Sim2Sim（MuJoCo）→ Sim2Real（实机部署）
```

1. **Train**: 在 Isaac Gym 仿真环境中训练，通过奖励函数学习运动策略
2. **Play**: 可视化验证策略是否符合预期
3. **Sim2Sim**: 将策略部署到 MuJoCo 仿真器，确保不过拟合
4. **Sim2Real**: 部署到物理机器人

### 13.3 安装

详细步骤参考: https://github.com/unitreerobotics/unitree_rl_gym/blob/main/doc/setup_en.md

主要依赖：
- **Isaac Gym**: https://developer.nvidia.com/isaac-gym
- **legged_gym**: 基础训练框架
- **rsl_rl**: 强化学习算法实现
- **MuJoCo**: Sim2Sim 验证
- **unitree_sdk2_python**: Sim2Real 部署

### 13.4 训练

```bash
# 训练 Go2
python legged_gym/scripts/train.py --task=go2

# 常用参数
python legged_gym/scripts/train.py \
    --task=go2 \              # 机器人类型: go2, g1, h1, h1_2
    --headless \              # 无界面模式（更高效）
    --num_envs=4096 \         # 并行环境数量
    --max_iterations=10000 \  # 最大迭代次数
    --sim_device=cuda:0 \     # 仿真计算设备
    --rl_device=cuda:0 \      # RL 计算设备
    --seed=42                 # 随机种子
```

**参数说明**:

| 参数 | 说明 |
|------|------|
| `--task` | **必选**，机器人类型: go2, g1, h1, h1_2 |
| `--headless` | 默认有图形界面，设为 true 则无界面（效率更高） |
| `--resume` | 从检查点恢复训练 |
| `--experiment_name` | 实验名称 |
| `--run_name` | 运行名称 |
| `--load_run` | 加载指定运行 |
| `--checkpoint` | 加载指定检查点 |
| `--num_envs` | 并行环境数 |
| `--max_iterations` | 最大迭代数 |
| `--sim_device` | 仿真设备，CPU: `--sim_device=cpu` |
| `--rl_device` | RL 设备，CPU: `--rl_device=cpu` |

**训练输出目录**: `logs/<experiment_name>/<date_time>_<run_name>/model_<iteration>.pt`

### 13.5 验证 (Play)

```bash
python legged_gym/scripts/play.py --task=go2
```

Play 会自动导出 Actor 网络到 `logs/{experiment_name}/exported/policies/`:
- 标准 MLP 网络: `policy_1.pt`
- RNN 网络: `policy_lstm_1.pt`

### 13.6 Sim2Sim (MuJoCo)

```bash
# 运行 Sim2Sim
python deploy/deploy_mujoco/deploy_mujoco.py go2.yaml

# go2.yaml 位于 deploy/deploy_mujoco/configs/
```

**配置文件重点**:
- `policy_path`: 策略模型路径，默认在 `deploy/pre_train/{robot}/motion.pt`
- 自训练模型保存在 `logs/go2/exported/policies/policy_lstm_1.pt`

### 13.7 Sim2Real (实机部署)

```bash
# 确保机器人在 Debug 模式
python deploy/deploy_real/deploy_real.py enp3s0 go2.yaml
```

| 参数 | 说明 |
|------|------|
| `net_interface` | 连接机器人的网卡名，如 `enp3s0` |
| `config_name` | 配置文件，位于 `deploy/deploy_real/configs/` |

#### C++ 部署（可选）

```bash
cd deploy/deploy_real/cpp_g1

# 下载 LibTorch
wget https://download.pytorch.org/libtorch/cpu/libtorch-cxx11-abi-shared-with-deps-2.7.1%2Bcpu.zip
unzip libtorch-cxx11-abi-shared-with-deps-2.7.1+cpu.zip

# 编译
mkdir build && cd build
cmake .. && make -j4

# 运行
./g1_deploy_run enp3s0
```

### 13.8 训练自定义步态

修改 `legged_gym/envs/go2/` 目录下的配置文件来自定义：
- **奖励函数**: 定义什么行为得到奖励/惩罚
- **观测空间**: 策略能看到什么信息
- **动作空间**: 策略能控制什么
- **域随机化**: 仿真到实机的迁移鲁棒性

---

## 第十四章 导航与 SLAM

### 14.1 概述

Go2 EDU 支持通过外挂 3D 导航 LiDAR（Livox MID-360 或禾赛 HESAI XT16）结合 SLAM 算法实现自主建图与导航。本章将详细介绍：

1. **建图**：如何使用 FAST-LIO2 构建高精度 3D/2D 地图
2. **导航**：如何使用 Nav2 实现自主路径规划与避障
3. **API 服务**：如何将导航能力封装为 RESTful API，供网页前端调用

**整体架构**：

```
┌───────────────────────────────────────────────────────────────────┐
│                        系统整体架构                                │
│                                                                   │
│  ┌──────────┐   ROS2    ┌──────────┐  WebSocket   ┌──────────┐  │
│  │  Go2 EDU │◄────────►│ Orin NX  │◄───────────►│ 网页前端  │  │
│  │  机载主板 │  DDS/    │ 导航主机  │  REST API   │ (Vue/React)│ │
│  │          │  CycloneDDS│          │             │           │  │
│  └──────────┘          └──────────┘             └──────────┘  │
│       │                     │                                    │
│   12个电机              ┌───┴───┐                               │
│   IMU/足力             │       │                               │
│                   MID-360   D435i                               │
│                   3D LiDAR  深度相机                             │
└───────────────────────────────────────────────────────────────────┘
```

### 14.2 硬件准备

#### 14.2.1 导航 LiDAR 选型

| 型号 | Livox MID-360 | HESAI XTi6R |
|------|--------------|-------------|
| 尺寸 | 65mm×65mm×60mm | Φ100mm×76mm |
| 供电 | 9-27V DC | 9-36V DC |
| 激光波长 | 905nm | 905nm |
| FOV | 水平 360°，垂直 -7°~52° | 水平 360°，垂直 30°（-15°~15°） |
| 测距范围 | 70m（10%反射率） | 30m（10%反射率） |
| 点频 | 200,000 点/秒 | 320,000 点/秒 |
| **推荐场景** | **通用首选，性价比高** | 多线雷达，均匀扫描 |

> 💡 推荐 **Livox MID-360**，宇树官方适配好、社区资源丰富。

#### 14.2.2 内置 LiDAR L1（避障用）

Go2 头部自带的 4D LiDAR L1 主要用于避障，不适合建图导航：

| 参数 | 数值 |
|------|------|
| 尺寸 | 75mm × 75mm × 65mm |
| 供电 | 12V DC |
| 波长 | 905nm |
| 采样率 | 21,600 次/秒 |
| FOV | 360° × 90° |

#### 14.2.3 推荐硬件配置

| 组件 | 推荐 | 用途 |
|------|------|------|
| 导航主机 | Orin NX 16GB (100 TOPS) | 运行 SLAM + Nav2 + API 服务 |
| 3D LiDAR | Livox MID-360 | 建图与定位 |
| 深度相机 | Intel D435i | 近距离避障补充 |
| 网络 | 外接路由器 / Wi-Fi AP | 网页端远程访问 |

### 14.3 SLAM 建图详解

#### 14.3.1 SLAM 方案对比

| 方案 | 传感器需求 | 精度 | 实时性 | 安装难度 | 推荐指数 |
|------|-----------|------|--------|---------|---------|
| **FAST-LIO2** | LiDAR + IMU | ★★★★★ | ★★★★★ | ★★★☆☆ | ⭐⭐⭐⭐⭐ |
| **Point-LIO** | LiDAR + IMU | ★★★★★ | ★★★★★ | ★★★☆☆ | ⭐⭐⭐⭐ |
| **LIO-SAM** | LiDAR + IMU + GPS(可选) | ★★★★☆ | ★★★★☆ | ★★☆☆☆ | ⭐⭐⭐☆ |
| **Cartographer** | LiDAR (+ IMU可选) | ★★★★☆ | ★★★☆☆ | ★★★☆☆ | ⭐⭐⭐☆ |

> 本手册以 **FAST-LIO2** 为主要建图方案进行讲解。

#### 14.3.2 安装 FAST-LIO2

**前提**：已完成第十一章 ROS2 环境搭建。

```bash
# 1. 安装依赖
sudo apt install -y libeigen3-dev libpcl-dev ros-$ROS_DISTRO-pcl-ros \
    ros-$ROS_DISTRO-tf2-geometry-msgs

# 2. 安装 Livox SDK2 (MID-360 驱动)
cd ~
git clone https://github.com/Livox-SDK/Livox-SDK2.git
cd Livox-SDK2 && mkdir build && cd build
cmake .. && make -j$(nproc) && sudo make install

# 3. 安装 livox_ros_driver2
cd ~/ros2_ws/src
git clone https://github.com/Livox-SDK/livox_ros_driver2.git
cd livox_ros_driver2
# 修改 MID360_config.json 中的 IP 配置
cd ~/ros2_ws
colcon build --packages-select livox_ros_driver2

# 4. 安装 FAST-LIO2
cd ~/ros2_ws/src
git clone https://github.com/hku-mars/FAST_LIO.git
cd FAST_LIO
git submodule update --init
cd ~/ros2_ws
colcon build --packages-select fast_lio
```

#### 14.3.3 配置 MID-360

编辑 `livox_ros_driver2/config/MID360_config.json`：

```json
{
  "lidar_summary_info": {
    "lidar_type": 8
  },
  "MID360": {
    "lidar_net_info": {
      "cmd_data_port": 56100,
      "push_msg_port": 56200,
      "point_data_port": 56300,
      "imu_data_port": 56400,
      "log_data_port": 56500
    },
    "host_net_info": {
      "cmd_data_ip": "192.168.1.50",
      "cmd_data_port": 56101,
      "push_msg_ip": "0.0.0.0",
      "push_msg_port": 56201,
      "point_data_ip": "0.0.0.0",
      "point_data_port": 56301,
      "imu_data_ip": "0.0.0.0",
      "imu_data_port": 56401,
      "log_data_ip": "",
      "log_data_port": 56501
    }
  }
}
```

> 将 `cmd_data_ip` 改为 Orin NX 连接 MID-360 的网卡 IP。

#### 14.3.4 配置 FAST-LIO2

编辑 `FAST_LIO/config/mid360.yaml`：

```yaml
common:
    lid_topic: "/livox/lidar"         # LiDAR Topic
    imu_topic: "/livox/imu"           # IMU Topic (MID-360自带)
    time_sync_en: false
    time_offset_lidar_to_imu: 0.0

preprocess:
    lidar_type: 1                      # 1=Livox
    scan_line: 4                       # MID-360 扫描线数
    blind: 0.5                         # 最小探测距离
    point_filter_num: 3                # 降采样

mapping:
    acc_cov: 0.1                       # 加速度协方差
    gyr_cov: 0.1                       # 陀螺仪协方差
    b_acc_cov: 0.0001                  # 加速度偏置协方差
    b_gyr_cov: 0.0001                  # 陀螺仪偏置协方差
    fov_degree: 360                    # FOV 角度
    det_range: 100.0                   # 检测范围 (m)
    extrinsic_est_en: true             # 在线标定外参
    extrinsic_T: [0.0, 0.0, 0.0]      # LiDAR→IMU 平移 (需标定)
    extrinsic_R: [1, 0, 0,
                  0, 1, 0,
                  0, 0, 1]             # LiDAR→IMU 旋转 (需标定)

publish:
    path_en: true                      # 发布轨迹
    scan_publish_en: true              # 发布扫描
    dense_publish_en: true             # 发布稠密点云
    scan_bodyframe_pub_en: true        # 发布体坐标系点云

pcd_save:
    pcd_save_en: true                  # 保存 PCD 地图
    interval: -1                       # -1 = 只在结束时保存
```

#### 14.3.5 建图流程

```bash
# 终端 1: 启动 MID-360 驱动
source ~/ros2_ws/install/setup.bash
ros2 launch livox_ros_driver2 msg_MID360_launch.py

# 终端 2: 启动 FAST-LIO2
source ~/ros2_ws/install/setup.bash
ros2 launch fast_lio mapping.launch.py config_file:=mid360.yaml

# 终端 3: Rviz2 可视化
ros2 run rviz2 rviz2
# 添加: PointCloud2 → /cloud_registered
# 添加: Path → /path
# Fixed Frame: camera_init

# 终端 4: 遥控机器人走动建图
# 使用 Go2 App 或遥控器控制机器人缓慢行走，覆盖目标区域
```

**建图要点**：
1. 移动速度不宜过快（≤0.5 m/s），避免运动模糊
2. 避免长时间面对无特征墙面
3. 走完一圈后尽量**回环闭合**（回到起点附近）
4. 建图完成后按 Ctrl+C 停止 FAST-LIO，PCD 地图自动保存

#### 14.3.6 地图后处理

FAST-LIO2 输出的是 3D 点云地图（`.pcd`），需要转换为 2D 占用栅格地图才能供 Nav2 使用。

```python
#!/usr/bin/env python3
"""pcd_to_2d_map.py — 将 3D PCD 点云转换为 2D 占用栅格地图"""

import numpy as np
import open3d as o3d
import yaml
from PIL import Image

def pcd_to_gridmap(pcd_file, output_prefix, resolution=0.05,
                   z_min=0.1, z_max=1.5):
    """
    Args:
        pcd_file: PCD 文件路径
        output_prefix: 输出文件前缀 (会生成 .pgm 和 .yaml)
        resolution: 栅格分辨率 (m/pixel)，默认 5cm
        z_min: 最低高度截取 (m)，过滤地面
        z_max: 最高高度截取 (m)，过滤天花板/噪声
    """
    # 1. 读取点云
    pcd = o3d.io.read_point_cloud(pcd_file)
    points = np.asarray(pcd.points)
    print(f"原始点数: {len(points)}")

    # 2. 高度过滤 — 只保留特定高度范围的点（障碍物层）
    mask = (points[:, 2] >= z_min) & (points[:, 2] <= z_max)
    filtered = points[mask]
    print(f"过滤后点数: {len(filtered)}")

    # 3. 计算地图边界
    x_min, y_min = filtered[:, 0].min(), filtered[:, 1].min()
    x_max, y_max = filtered[:, 0].max(), filtered[:, 1].max()

    # 外扩 1m 边距
    margin = 1.0
    x_min -= margin; y_min -= margin
    x_max += margin; y_max += margin

    width = int(np.ceil((x_max - x_min) / resolution))
    height = int(np.ceil((y_max - y_min) / resolution))
    print(f"地图尺寸: {width} x {height} pixels")

    # 4. 生成栅格地图 (初始全部为未知=205)
    grid = np.full((height, width), 205, dtype=np.uint8)

    # 5. 将点云投影到 2D 栅格（障碍物=0/黑色）
    for p in filtered:
        col = int((p[0] - x_min) / resolution)
        row = int((p[1] - y_min) / resolution)
        row = height - 1 - row  # 翻转 y 轴
        if 0 <= row < height and 0 <= col < width:
            grid[row, col] = 0  # 占用

    # 6. 膨胀处理 — 对障碍物周围小范围标记为自由空间
    from scipy.ndimage import binary_dilation
    occupied = grid == 0
    # 将障碍物附近的未知区域标记为自由
    dilated = binary_dilation(occupied, iterations=3)
    free_mask = dilated & (~occupied) & (grid == 205)
    grid[free_mask] = 254  # 自由空间

    # 7. 保存地图图片 (.pgm)
    img = Image.fromarray(grid)
    pgm_file = f"{output_prefix}.pgm"
    img.save(pgm_file)

    # 8. 保存地图元数据 (.yaml)
    map_yaml = {
        'image': pgm_file,
        'resolution': resolution,
        'origin': [x_min, y_min, 0.0],
        'negate': 0,
        'occupied_thresh': 0.65,
        'free_thresh': 0.196
    }
    yaml_file = f"{output_prefix}.yaml"
    with open(yaml_file, 'w') as f:
        yaml.dump(map_yaml, f, default_flow_style=False)

    print(f"地图已保存: {pgm_file}, {yaml_file}")

if __name__ == "__main__":
    import sys
    if len(sys.argv) < 2:
        print("用法: python3 pcd_to_2d_map.py <input.pcd> [output_prefix]")
        sys.exit(1)
    pcd_file = sys.argv[1]
    output = sys.argv[2] if len(sys.argv) > 2 else "map"
    pcd_to_gridmap(pcd_file, output)
```

安装依赖并运行：

```bash
pip3 install open3d numpy pillow pyyaml scipy

# 将 3D 点云转为 2D 栅格地图
python3 pcd_to_2d_map.py scans.pcd my_map
# 输出: my_map.pgm + my_map.yaml
```

生成的 `my_map.yaml` 内容：

```yaml
image: my_map.pgm
resolution: 0.05          # 每像素 5cm
origin: [-12.5, -8.3, 0.0] # 地图左下角的世界坐标
negate: 0
occupied_thresh: 0.65
free_thresh: 0.196
```

### 14.4 Nav2 自主导航详解

#### 14.4.1 Nav2 架构

```
┌───────────────────────────────────────────────────────┐
│                    Nav2 导航栈                         │
│                                                       │
│  ┌─────────────┐  ┌─────────────┐  ┌──────────────┐ │
│  │  BT Navigator│  │ Planner     │  │ Controller   │ │
│  │  (行为树)    │  │ (路径规划)  │  │ (局部控制)   │ │
│  └──────┬──────┘  └──────┬──────┘  └──────┬───────┘ │
│         │                │                 │          │
│         ▼                ▼                 ▼          │
│  ┌─────────────┐  ┌─────────────┐  ┌──────────────┐ │
│  │ Recovery    │  │ Costmap 2D  │  │ Velocity     │ │
│  │ (恢复行为)  │  │ (代价地图)  │  │ Smoother     │ │
│  └─────────────┘  └─────────────┘  └──────────────┘ │
│                          │                            │
│                          ▼                            │
│                   ┌─────────────┐                    │
│                   │ AMCL 定位    │                    │
│                   │ (蒙特卡洛)  │                    │
│                   └─────────────┘                    │
└───────────────────────────────────────────────────────┘
           │                          ▲
           ▼ /cmd_vel                 │ /scan, /odom
    ┌──────────────┐          ┌──────────────┐
    │  Go2 运动    │          │  Go2 传感器  │
    │  SportClient │          │  LiDAR/IMU   │
    └──────────────┘          └──────────────┘
```

#### 14.4.2 安装 Nav2

```bash
# ROS2 Foxy
sudo apt install -y ros-foxy-navigation2 ros-foxy-nav2-bringup \
    ros-foxy-slam-toolbox ros-foxy-robot-localization

# ROS2 Humble
sudo apt install -y ros-humble-navigation2 ros-humble-nav2-bringup \
    ros-humble-slam-toolbox ros-humble-robot-localization
```

#### 14.4.3 创建导航 Launch 文件

创建 `go2_nav2_ws/src/go2_navigation/launch/go2_nav2.launch.py`：

```python
import os
from ament_index_python.packages import get_package_share_directory
from launch import LaunchDescription
from launch.actions import IncludeLaunchDescription
from launch.launch_description_sources import PythonLaunchDescriptionSource
from launch_ros.actions import Node

def generate_launch_description():
    nav2_bringup_dir = get_package_share_directory('nav2_bringup')
    
    # 地图文件路径
    map_yaml = os.path.expanduser('~/maps/my_map.yaml')
    
    # Nav2 参数文件
    nav2_params = os.path.expanduser(
        '~/go2_nav2_ws/src/go2_navigation/config/nav2_params.yaml'
    )
    
    return LaunchDescription([
        # AMCL 定位 + Nav2 导航
        IncludeLaunchDescription(
            PythonLaunchDescriptionSource(
                os.path.join(nav2_bringup_dir, 'launch', 
                             'bringup_launch.py')
            ),
            launch_arguments={
                'map': map_yaml,
                'params_file': nav2_params,
                'use_sim_time': 'false',
            }.items(),
        ),
        
        # cmd_vel → Go2 运动桥接节点
        Node(
            package='go2_navigation',
            executable='cmd_vel_bridge',
            name='cmd_vel_bridge',
            output='screen',
        ),
    ])
```

#### 14.4.4 Nav2 参数配置

创建 `go2_navigation/config/nav2_params.yaml`：

```yaml
amcl:
  ros__parameters:
    use_sim_time: false
    alpha1: 0.2        # 旋转→旋转噪声
    alpha2: 0.2        # 旋转→平移噪声
    alpha3: 0.2        # 平移→平移噪声
    alpha4: 0.2        # 平移→旋转噪声
    base_frame_id: "base_link"
    global_frame_id: "map"
    odom_frame_id: "odom"
    max_particles: 2000
    min_particles: 500
    scan_topic: /scan   # 2D 扫描 Topic

bt_navigator:
  ros__parameters:
    use_sim_time: false
    global_frame: map
    robot_base_frame: base_link
    odom_topic: /odom
    # 行为树默认即可

controller_server:
  ros__parameters:
    use_sim_time: false
    controller_frequency: 20.0
    # Go2 运动学参数
    FollowPath:
      plugin: "dwb_core::DWBLocalPlanner"
      min_vel_x: -0.5
      max_vel_x: 1.5          # Go2 正常行走速度上限
      min_vel_y: -0.5          # Go2 支持横移
      max_vel_y: 0.5
      max_vel_theta: 1.5
      min_speed_xy: 0.0
      max_speed_xy: 1.5
      acc_lim_x: 2.0
      acc_lim_y: 2.0
      acc_lim_theta: 3.2
      decel_lim_x: -3.0
      decel_lim_y: -3.0
      decel_lim_theta: -3.2

planner_server:
  ros__parameters:
    use_sim_time: false
    GridBased:
      plugin: "nav2_navfn_planner/NavfnPlanner"
      tolerance: 0.5
      use_astar: true
      allow_unknown: true

local_costmap:
  local_costmap:
    ros__parameters:
      update_frequency: 5.0
      publish_frequency: 2.0
      global_frame: odom
      robot_base_frame: base_link
      rolling_window: true
      width: 5          # 5m × 5m 局部窗口
      height: 5
      resolution: 0.05
      robot_radius: 0.25  # Go2 半径约 25cm
      plugins: ["obstacle_layer", "inflation_layer"]
      obstacle_layer:
        plugin: "nav2_costmap_2d::ObstacleLayer"
        observation_sources: scan
        scan:
          topic: /scan
          max_obstacle_height: 2.0
          clearing: true
          marking: true
      inflation_layer:
        plugin: "nav2_costmap_2d::InflationLayer"
        cost_scaling_factor: 3.0
        inflation_radius: 0.55  # 膨胀半径

global_costmap:
  global_costmap:
    ros__parameters:
      update_frequency: 1.0
      publish_frequency: 1.0
      global_frame: map
      robot_base_frame: base_link
      robot_radius: 0.25
      resolution: 0.05
      track_unknown_space: true
      plugins: ["static_layer", "obstacle_layer", "inflation_layer"]
      static_layer:
        plugin: "nav2_costmap_2d::StaticLayer"
        map_subscribe_transient_local: true
      obstacle_layer:
        plugin: "nav2_costmap_2d::ObstacleLayer"
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
    use_sim_time: false
    costmap_topic: local_costmap/costmap_raw
    footprint_topic: local_costmap/published_footprint
    recovery_plugins: ["spin", "backup", "wait"]
    spin:
      plugin: "nav2_recoveries/Spin"
    backup:
      plugin: "nav2_recoveries/BackUp"
    wait:
      plugin: "nav2_recoveries/Wait"
```

#### 14.4.5 cmd_vel 桥接节点

Nav2 输出 `/cmd_vel`（geometry_msgs/Twist），需要桥接到 Go2 的 `SportClient`：

```python
#!/usr/bin/env python3
"""cmd_vel_bridge.py — 将 Nav2 的 /cmd_vel 转换为 Go2 运动指令"""

import rclpy
from rclpy.node import Node
from geometry_msgs.msg import Twist

from unitree_sdk2py.core.channel import ChannelFactoryInitialize
from unitree_sdk2py.go2.sport.sport_client import SportClient


class CmdVelBridge(Node):
    def __init__(self):
        super().__init__('cmd_vel_bridge')

        # 初始化 Go2 SDK
        ChannelFactoryInitialize(0, "eth0")  # 替换为实际网卡名
        self.sport_client = SportClient()
        self.sport_client.SetTimeout(5.0)
        self.sport_client.Init()

        # 订阅 Nav2 的 /cmd_vel
        self.sub = self.create_subscription(
            Twist, '/cmd_vel', self.cmd_vel_callback, 10)

        # 安全超时：如果 0.5s 没收到新指令，停止运动
        self.timer = self.create_timer(0.5, self.safety_timeout)
        self.last_cmd_time = self.get_clock().now()

        self.get_logger().info('Go2 cmd_vel bridge started')

    def cmd_vel_callback(self, msg: Twist):
        vx = msg.linear.x       # 前进速度 m/s
        vy = msg.linear.y       # 横移速度 m/s
        vyaw = msg.angular.z    # 偏航角速度 rad/s

        # 限幅保护
        vx = max(-1.0, min(vx, 2.0))
        vy = max(-0.5, min(vy, 0.5))
        vyaw = max(-2.0, min(vyaw, 2.0))

        self.sport_client.Move(vx, vy, vyaw)
        self.last_cmd_time = self.get_clock().now()

    def safety_timeout(self):
        elapsed = (self.get_clock().now() - self.last_cmd_time).nanoseconds / 1e9
        if elapsed > 0.5:
            self.sport_client.StopMove()


def main():
    rclpy.init()
    node = CmdVelBridge()
    rclpy.spin(node)
    node.destroy_node()
    rclpy.shutdown()

if __name__ == '__main__':
    main()
```

#### 14.4.6 TF 树配置

Nav2 需要完整的 TF 树。创建静态 TF 发布：

```python
#!/usr/bin/env python3
"""go2_tf_publisher.py — 发布 Go2 的 TF 树"""

import rclpy
from rclpy.node import Node
from tf2_ros import StaticTransformBroadcaster, TransformBroadcaster
from geometry_msgs.msg import TransformStamped
import math

from unitree_sdk2py.core.channel import ChannelFactoryInitialize
# 订阅 Go2 里程计


class Go2TFPublisher(Node):
    def __init__(self):
        super().__init__('go2_tf_publisher')
        
        # 静态 TF: base_link → lidar_link
        self.static_br = StaticTransformBroadcaster(self)
        t = TransformStamped()
        t.header.stamp = self.get_clock().now().to_msg()
        t.header.frame_id = 'base_link'
        t.child_frame_id = 'lidar_link'
        # MID-360 安装在 Go2 背部，根据实际安装位置调整
        t.transform.translation.x = 0.0
        t.transform.translation.y = 0.0
        t.transform.translation.z = 0.45  # 高度约 45cm
        t.transform.rotation.w = 1.0
        self.static_br.sendTransform(t)
        
        # 动态 TF: odom → base_link (由里程计更新)
        self.br = TransformBroadcaster(self)
        self.timer = self.create_timer(0.02, self.publish_odom_tf)  # 50Hz
        
    def publish_odom_tf(self):
        # 从 Go2 SportModeState 获取位姿，发布 odom→base_link
        # 这里需要订阅 Go2 的 sportmodestate Topic
        pass  # 具体实现参见第九章


def main():
    rclpy.init()
    node = Go2TFPublisher()
    rclpy.spin(node)
    rclpy.shutdown()
```

#### 14.4.7 启动导航

```bash
# 终端 1: MID-360 驱动
ros2 launch livox_ros_driver2 msg_MID360_launch.py

# 终端 2: 定位（使用 FAST-LIO2 在线定位模式 或 AMCL）
ros2 launch fast_lio mapping.launch.py

# 终端 3: Nav2 导航
ros2 launch go2_navigation go2_nav2.launch.py

# 终端 4: Rviz2 可视化
ros2 launch nav2_bringup rviz_launch.py
# 在 Rviz2 中:
#   1. 点击 "2D Pose Estimate" 设置初始位置
#   2. 点击 "Nav2 Goal" 设置目标点
#   3. 机器人自动规划路径并行走
```

#### 14.4.8 编程导航（Action Client）

```python
#!/usr/bin/env python3
"""go2_nav_to_pose.py — 编程方式发送导航目标"""

import rclpy
from rclpy.node import Node
from rclpy.action import ActionClient
from nav2_msgs.action import NavigateToPose
from geometry_msgs.msg import PoseStamped
import math


class Go2Navigator(Node):
    def __init__(self):
        super().__init__('go2_navigator')
        self.nav_client = ActionClient(
            self, NavigateToPose, 'navigate_to_pose')

    def go_to(self, x, y, yaw=0.0):
        """导航到指定坐标点"""
        self.nav_client.wait_for_server()

        goal = NavigateToPose.Goal()
        goal.pose = PoseStamped()
        goal.pose.header.frame_id = 'map'
        goal.pose.header.stamp = self.get_clock().now().to_msg()
        goal.pose.pose.position.x = x
        goal.pose.pose.position.y = y
        goal.pose.pose.position.z = 0.0
        # yaw → quaternion
        goal.pose.pose.orientation.z = math.sin(yaw / 2)
        goal.pose.pose.orientation.w = math.cos(yaw / 2)

        self.get_logger().info(f'导航目标: ({x}, {y}, yaw={yaw:.2f})')
        future = self.nav_client.send_goal_async(
            goal, feedback_callback=self.feedback_cb)
        future.add_done_callback(self.goal_response_cb)

    def goal_response_cb(self, future):
        goal_handle = future.result()
        if not goal_handle.accepted:
            self.get_logger().warn('目标被拒绝')
            return
        self.get_logger().info('目标已接受，正在导航...')
        result_future = goal_handle.get_result_async()
        result_future.add_done_callback(self.result_cb)

    def feedback_cb(self, feedback_msg):
        pos = feedback_msg.feedback.current_pose.pose.position
        self.get_logger().info(f'当前位置: ({pos.x:.2f}, {pos.y:.2f})')

    def result_cb(self, future):
        self.get_logger().info('导航完成!')


def main():
    rclpy.init()
    nav = Go2Navigator()
    nav.go_to(3.0, 2.0, yaw=1.57)  # 导航到 (3, 2)，面朝北
    rclpy.spin(nav)
    rclpy.shutdown()

if __name__ == '__main__':
    main()
```

### 14.5 RESTful API 服务（供网页前端调用）

#### 14.5.1 架构设计

```
┌────────────────────────────────────────────────────────────┐
│                   Web 控制架构                              │
│                                                            │
│  ┌─────────┐  HTTP/WS  ┌──────────┐  ROS2   ┌─────────┐ │
│  │ 网页前端 │◄────────►│ FastAPI  │◄──────►│  Nav2   │ │
│  │ Vue.js  │ REST+WS  │ 后端服务  │ Action  │  SLAM   │ │
│  │ Leaflet │          │ Port:8080│ Topic   │  Go2    │ │
│  └─────────┘          └──────────┘         └─────────┘ │
│                            │                             │
│                       功能模块:                          │
│                       • POST /nav/goal     发送导航目标  │
│                       • GET  /nav/status   导航状态      │
│                       • POST /nav/cancel   取消导航      │
│                       • GET  /robot/state  机器人状态    │
│                       • POST /robot/move   手动控制      │
│                       • POST /robot/action 特殊动作      │
│                       • GET  /map/image    获取地图      │
│                       • WS   /ws/status    实时状态推送  │
└────────────────────────────────────────────────────────────┘
```

#### 14.5.2 安装依赖

```bash
pip3 install fastapi uvicorn[standard] websockets pydantic
```

#### 14.5.3 FastAPI 后端服务

```python
#!/usr/bin/env python3
"""go2_api_server.py — Go2 导航 + 控制 RESTful API 服务"""

import asyncio
import math
import os
import threading
import base64
from typing import Optional
from contextlib import asynccontextmanager

import rclpy
from rclpy.node import Node
from rclpy.action import ActionClient
from rclpy.executors import MultiThreadedExecutor
from nav2_msgs.action import NavigateToPose
from geometry_msgs.msg import PoseStamped, Twist

from fastapi import FastAPI, WebSocket, WebSocketDisconnect
from fastapi.middleware.cors import CORSMiddleware
from fastapi.responses import FileResponse
from pydantic import BaseModel

# ───── 数据模型 ─────

class NavGoalRequest(BaseModel):
    x: float
    y: float
    yaw: float = 0.0

class MoveRequest(BaseModel):
    vx: float = 0.0     # 前进速度 m/s
    vy: float = 0.0     # 横移速度 m/s
    vyaw: float = 0.0   # 偏航角速度 rad/s

class ActionRequest(BaseModel):
    action: str          # hello, sit, stand, dance1, dance2, stretch 等

# ───── ROS2 节点 ─────

class Go2APINode(Node):
    def __init__(self):
        super().__init__('go2_api_node')

        # Nav2 Action Client
        self.nav_client = ActionClient(
            self, NavigateToPose, 'navigate_to_pose')

        # cmd_vel 发布者（手动控制时使用）
        self.cmd_pub = self.create_publisher(Twist, '/cmd_vel', 10)

        # 状态订阅
        self.robot_state = {
            'position': {'x': 0.0, 'y': 0.0, 'z': 0.0},
            'velocity': {'vx': 0.0, 'vy': 0.0, 'vz': 0.0},
            'battery': 100.0,
            'mode': 'idle',
        }
        self.nav_status = {
            'active': False,
            'goal': None,
            'progress': 0.0,
            'feedback_pose': None,
        }
        self._goal_handle = None

        # 订阅 Go2 相关 Topic (根据实际环境适配)
        # self.state_sub = self.create_subscription(...)

    # ── 导航 ──
    async def navigate_to(self, x: float, y: float, yaw: float):
        if not self.nav_client.wait_for_server(timeout_sec=5.0):
            return {'success': False, 'message': 'Nav2 服务未就绪'}

        goal = NavigateToPose.Goal()
        goal.pose = PoseStamped()
        goal.pose.header.frame_id = 'map'
        goal.pose.header.stamp = self.get_clock().now().to_msg()
        goal.pose.pose.position.x = x
        goal.pose.pose.position.y = y
        goal.pose.pose.orientation.z = math.sin(yaw / 2)
        goal.pose.pose.orientation.w = math.cos(yaw / 2)

        future = self.nav_client.send_goal_async(
            goal, feedback_callback=self._nav_feedback_cb)
        goal_handle = await asyncio.wrap_future(
            asyncio.ensure_future(self._await_goal(future)))

        if not goal_handle or not goal_handle.accepted:
            self.nav_status['active'] = False
            return {'success': False, 'message': '导航目标被拒绝'}

        self._goal_handle = goal_handle
        self.nav_status.update({
            'active': True,
            'goal': {'x': x, 'y': y, 'yaw': yaw},
            'progress': 0.0,
        })
        return {'success': True, 'message': f'导航至 ({x:.2f}, {y:.2f})'}

    async def _await_goal(self, future):
        """在 asyncio 中等待 ROS2 future"""
        loop = asyncio.get_event_loop()
        result = await loop.run_in_executor(None, lambda: future.result())
        return result

    def _nav_feedback_cb(self, feedback_msg):
        pos = feedback_msg.feedback.current_pose.pose.position
        self.nav_status['feedback_pose'] = {
            'x': pos.x, 'y': pos.y
        }

    async def cancel_navigation(self):
        if self._goal_handle:
            self._goal_handle.cancel_goal_async()
            self._goal_handle = None
        self.nav_status['active'] = False
        return {'success': True, 'message': '导航已取消'}

    # ── 手动控制 ──
    def send_move(self, vx, vy, vyaw):
        # 限幅保护
        vx = max(-1.0, min(vx, 2.0))
        vy = max(-0.5, min(vy, 0.5))
        vyaw = max(-2.0, min(vyaw, 2.0))

        msg = Twist()
        msg.linear.x = vx
        msg.linear.y = vy
        msg.angular.z = vyaw
        self.cmd_pub.publish(msg)

    def stop_move(self):
        msg = Twist()  # 全零速度
        self.cmd_pub.publish(msg)


# ───── 全局实例 ─────

ros_node: Optional[Go2APINode] = None
ws_clients: list[WebSocket] = []


@asynccontextmanager
async def lifespan(app: FastAPI):
    """启动和关闭 ROS2 节点"""
    global ros_node
    rclpy.init()
    ros_node = Go2APINode()

    # 在后台线程运行 ROS2 spin
    executor = MultiThreadedExecutor()
    executor.add_node(ros_node)
    ros_thread = threading.Thread(
        target=executor.spin, daemon=True)
    ros_thread.start()

    yield

    ros_node.destroy_node()
    rclpy.shutdown()


# ───── FastAPI 应用 ─────

app = FastAPI(
    title="Go2 Navigation API",
    description="宇树 Go2 机器狗导航与控制 API",
    version="1.0.0",
    lifespan=lifespan,
)

# CORS 配置（允许网页前端跨域访问）
app.add_middleware(
    CORSMiddleware,
    allow_origins=["*"],       # 生产环境应限制为具体域名
    allow_credentials=True,
    allow_methods=["*"],
    allow_headers=["*"],
)

# ── 导航 API ──

@app.post("/nav/goal", tags=["导航"])
async def set_nav_goal(req: NavGoalRequest):
    """发送导航目标点"""
    result = await ros_node.navigate_to(req.x, req.y, req.yaw)
    return result

@app.get("/nav/status", tags=["导航"])
async def get_nav_status():
    """获取当前导航状态"""
    return ros_node.nav_status

@app.post("/nav/cancel", tags=["导航"])
async def cancel_navigation():
    """取消当前导航"""
    return await ros_node.cancel_navigation()

# ── 机器人控制 API ──

@app.get("/robot/state", tags=["机器人"])
async def get_robot_state():
    """获取机器人状态（位置、速度、电量等）"""
    return ros_node.robot_state

@app.post("/robot/move", tags=["机器人"])
async def manual_move(req: MoveRequest):
    """手动速度控制"""
    ros_node.send_move(req.vx, req.vy, req.vyaw)
    return {"success": True, "message": f"Move({req.vx}, {req.vy}, {req.vyaw})"}

@app.post("/robot/stop", tags=["机器人"])
async def stop_robot():
    """停止运动"""
    ros_node.stop_move()
    return {"success": True, "message": "已停止"}

@app.post("/robot/action", tags=["机器人"])
async def execute_action(req: ActionRequest):
    """执行特殊动作（hello, sit, stand, dance1, dance2, stretch 等）"""
    # 这里需要通过 SportClient 调用具体动作
    # 实际实现中通过 DDS 发送对应的 sport 请求
    allowed_actions = [
        "hello", "sit", "stand_up", "stand_down",
        "recovery_stand", "balance_stand",
        "stretch", "dance1", "dance2",
        "front_flip", "front_jump", "wallow"
    ]
    if req.action not in allowed_actions:
        return {"success": False,
                "message": f"不支持的动作: {req.action}，"
                           f"可用: {allowed_actions}"}
    # TODO: 调用 SportClient 对应方法
    return {"success": True, "message": f"执行动作: {req.action}"}

# ── 地图 API ──

@app.get("/map/image", tags=["地图"])
async def get_map_image():
    """获取当前地图图片"""
    map_path = os.path.expanduser("~/maps/my_map.pgm")
    if os.path.exists(map_path):
        return FileResponse(map_path, media_type="image/x-portable-graymap")
    return {"error": "地图文件不存在"}

@app.get("/map/info", tags=["地图"])
async def get_map_info():
    """获取地图元数据（分辨率、原点等）"""
    import yaml
    yaml_path = os.path.expanduser("~/maps/my_map.yaml")
    if os.path.exists(yaml_path):
        with open(yaml_path, 'r') as f:
            return yaml.safe_load(f)
    return {"error": "地图元数据不存在"}

# ── WebSocket 实时推送 ──

@app.websocket("/ws/status")
async def websocket_status(ws: WebSocket):
    """通过 WebSocket 实时推送机器人状态"""
    await ws.accept()
    ws_clients.append(ws)
    try:
        while True:
            # 每 100ms 推送一次状态
            data = {
                "robot": ros_node.robot_state,
                "nav": ros_node.nav_status,
            }
            await ws.send_json(data)
            await asyncio.sleep(0.1)
    except WebSocketDisconnect:
        ws_clients.remove(ws)


# ── 启动 ──

if __name__ == "__main__":
    import uvicorn
    # 监听所有网卡，端口 8080
    uvicorn.run(app, host="0.0.0.0", port=8080)
```

#### 14.5.4 启动 API 服务

```bash
# 确保 ROS2 环境已 source
source ~/unitree_ros2/setup.sh
source ~/go2_nav2_ws/install/setup.bash

# 启动 API 服务
python3 go2_api_server.py
# 输出: Uvicorn running on http://0.0.0.0:8080

# 自动生成的 API 文档
# Swagger UI:  http://<ORIN_IP>:8080/docs
# ReDoc:       http://<ORIN_IP>:8080/redoc
```

#### 14.5.5 API 接口速查

| 方法 | 路径 | 说明 | 请求体 |
|------|------|------|--------|
| `POST` | `/nav/goal` | 发送导航目标 | `{"x": 3.0, "y": 2.0, "yaw": 1.57}` |
| `GET` | `/nav/status` | 获取导航状态 | — |
| `POST` | `/nav/cancel` | 取消导航 | — |
| `GET` | `/robot/state` | 获取机器人状态 | — |
| `POST` | `/robot/move` | 手动控制 | `{"vx": 0.5, "vy": 0.0, "vyaw": 0.0}` |
| `POST` | `/robot/stop` | 停止运动 | — |
| `POST` | `/robot/action` | 特殊动作 | `{"action": "dance1"}` |
| `GET` | `/map/image` | 获取地图图片 | — |
| `GET` | `/map/info` | 获取地图元数据 | — |
| `WS` | `/ws/status` | 实时状态推送 | — |

#### 14.5.6 前端调用示例

**JavaScript (Fetch API)**：

```javascript
const API_BASE = 'http://192.168.123.100:8080';

// 1. 发送导航目标
async function navigateTo(x, y, yaw = 0) {
    const res = await fetch(`${API_BASE}/nav/goal`, {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({ x, y, yaw }),
    });
    return await res.json();
}

// 2. 获取导航状态
async function getNavStatus() {
    const res = await fetch(`${API_BASE}/nav/status`);
    return await res.json();
}

// 3. 取消导航
async function cancelNav() {
    const res = await fetch(`${API_BASE}/nav/cancel`, { method: 'POST' });
    return await res.json();
}

// 4. 手动控制
async function move(vx, vy, vyaw) {
    const res = await fetch(`${API_BASE}/robot/move`, {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({ vx, vy, vyaw }),
    });
    return await res.json();
}

// 5. 执行动作
async function doAction(action) {
    const res = await fetch(`${API_BASE}/robot/action`, {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({ action }),
    });
    return await res.json();
}

// 6. WebSocket 实时状态
const ws = new WebSocket('ws://192.168.123.100:8080/ws/status');
ws.onmessage = (event) => {
    const data = JSON.parse(event.data);
    console.log('机器人位置:', data.robot.position);
    console.log('导航状态:', data.nav);
    // 更新前端 UI...
};
```

**Vue 3 组合式 API 示例**：

```vue
<template>
  <div class="go2-control">
    <!-- 地图显示区域 -->
    <div id="map-container" ref="mapRef"></div>

    <!-- 控制面板 -->
    <div class="control-panel">
      <h3>Go2 导航控制</h3>

      <!-- 状态显示 -->
      <div class="status">
        <p>位置: ({{ state.position.x.toFixed(2) }},
                  {{ state.position.y.toFixed(2) }})</p>
        <p>电量: {{ state.battery.toFixed(0) }}%</p>
        <p>导航: {{ navStatus.active ? '进行中' : '空闲' }}</p>
      </div>

      <!-- 导航目标 -->
      <div class="nav-input">
        <input v-model.number="goalX" placeholder="X" />
        <input v-model.number="goalY" placeholder="Y" />
        <button @click="sendGoal">导航</button>
        <button @click="cancelNav" class="danger">取消</button>
      </div>

      <!-- 方向控制 -->
      <div class="joystick">
        <button @mousedown="move(0.5, 0, 0)"
                @mouseup="stop">前进</button>
        <button @mousedown="move(-0.3, 0, 0)"
                @mouseup="stop">后退</button>
        <button @mousedown="move(0, 0.3, 0)"
                @mouseup="stop">左移</button>
        <button @mousedown="move(0, -0.3, 0)"
                @mouseup="stop">右移</button>
        <button @mousedown="move(0, 0, 0.8)"
                @mouseup="stop">左转</button>
        <button @mousedown="move(0, 0, -0.8)"
                @mouseup="stop">右转</button>
      </div>

      <!-- 特殊动作 -->
      <div class="actions">
        <button @click="doAction('hello')">打招呼</button>
        <button @click="doAction('sit')">坐下</button>
        <button @click="doAction('dance1')">跳舞</button>
        <button @click="doAction('stretch')">伸懒腰</button>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, reactive, onMounted, onUnmounted } from 'vue';

const API = 'http://192.168.123.100:8080';

const state = reactive({
  position: { x: 0, y: 0, z: 0 },
  velocity: { vx: 0, vy: 0, vz: 0 },
  battery: 100,
  mode: 'idle',
});
const navStatus = reactive({ active: false, goal: null, progress: 0 });
const goalX = ref(0);
const goalY = ref(0);

let ws = null;

onMounted(() => {
  // WebSocket 实时连接
  ws = new WebSocket(`ws://192.168.123.100:8080/ws/status`);
  ws.onmessage = (e) => {
    const data = JSON.parse(e.data);
    Object.assign(state, data.robot);
    Object.assign(navStatus, data.nav);
  };
});

onUnmounted(() => { ws?.close(); });

async function sendGoal() {
  await fetch(`${API}/nav/goal`, {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({ x: goalX.value, y: goalY.value, yaw: 0 }),
  });
}

async function cancelNav() {
  await fetch(`${API}/nav/cancel`, { method: 'POST' });
}

async function move(vx, vy, vyaw) {
  await fetch(`${API}/robot/move`, {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({ vx, vy, vyaw }),
  });
}

async function stop() {
  await fetch(`${API}/robot/stop`, { method: 'POST' });
}

async function doAction(action) {
  await fetch(`${API}/robot/action`, {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({ action }),
  });
}
</script>
```

#### 14.5.7 地图可视化（Leaflet.js）

在网页中用 Leaflet.js 显示占用栅格地图，并支持点击设置导航目标：

```javascript
import L from 'leaflet';

const API = 'http://192.168.123.100:8080';

async function initMap(containerId) {
    // 1. 获取地图元信息
    const info = await (await fetch(`${API}/map/info`)).json();
    const resolution = info.resolution;       // m/pixel
    const origin = info.origin;               // [x_min, y_min, 0]

    // 2. 创建 Leaflet 地图 (CRS.Simple 用于非地球坐标)
    const map = L.map(containerId, {
        crs: L.CRS.Simple,
        minZoom: -3,
        maxZoom: 3,
    });

    // 3. 加载地图图片
    const mapImg = new Image();
    mapImg.src = `${API}/map/image`;
    mapImg.onload = () => {
        const w = mapImg.width * resolution;   // 地图实际宽度 (m)
        const h = mapImg.height * resolution;  // 地图实际高度 (m)

        // 地图边界 (世界坐标)
        const southWest = L.latLng(origin[1], origin[0]);
        const northEast = L.latLng(origin[1] + h, origin[0] + w);
        const bounds = L.latLngBounds(southWest, northEast);

        L.imageOverlay(mapImg.src, bounds).addTo(map);
        map.fitBounds(bounds);
    };

    // 4. 机器人位置标记
    const robotMarker = L.circleMarker([0, 0], {
        radius: 8, color: '#00ff00', fillOpacity: 0.8,
    }).addTo(map);

    // 5. 点击地图设置导航目标
    map.on('click', async (e) => {
        const { lat: y, lng: x } = e.latlng;

        // 在地图上显示目标点
        L.marker([y, x]).addTo(map)
            .bindPopup(`目标: (${x.toFixed(2)}, ${y.toFixed(2)})`)
            .openPopup();

        // 发送导航请求
        await fetch(`${API}/nav/goal`, {
            method: 'POST',
            headers: { 'Content-Type': 'application/json' },
            body: JSON.stringify({ x, y, yaw: 0 }),
        });
    });

    // 6. 实时更新机器人位置
    const ws = new WebSocket('ws://192.168.123.100:8080/ws/status');
    ws.onmessage = (e) => {
        const data = JSON.parse(e.data);
        const pos = data.robot.position;
        robotMarker.setLatLng([pos.y, pos.x]);
    };

    return map;
}
```

### 14.6 多点巡航

#### 14.6.1 巡航 API

在 `go2_api_server.py` 中添加多点巡航端点：

```python
class PatrolRequest(BaseModel):
    waypoints: list[NavGoalRequest]   # 巡航路径点列表
    loop: bool = False                # 是否循环巡航

patrol_task = None  # 巡航协程句柄

@app.post("/nav/patrol", tags=["导航"])
async def start_patrol(req: PatrolRequest):
    """多点巡航 — 按顺序导航到各个路径点"""
    global patrol_task

    async def _run_patrol():
        while True:
            for i, wp in enumerate(req.waypoints):
                ros_node.get_logger().info(
                    f'巡航点 {i+1}/{len(req.waypoints)}: '
                    f'({wp.x}, {wp.y})')
                await ros_node.navigate_to(wp.x, wp.y, wp.yaw)

                # 等待导航完成
                while ros_node.nav_status['active']:
                    await asyncio.sleep(0.5)

            if not req.loop:
                break

    patrol_task = asyncio.create_task(_run_patrol())
    return {
        "success": True,
        "message": f"开始巡航，共 {len(req.waypoints)} 个路径点，"
                   f"循环={'是' if req.loop else '否'}"
    }

@app.post("/nav/patrol/stop", tags=["导航"])
async def stop_patrol():
    """停止巡航"""
    global patrol_task
    if patrol_task:
        patrol_task.cancel()
        patrol_task = None
    await ros_node.cancel_navigation()
    return {"success": True, "message": "巡航已停止"}
```

#### 14.6.2 前端调用巡航

```javascript
// 多点巡航
async function startPatrol(waypoints, loop = false) {
    const res = await fetch(`${API}/nav/patrol`, {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({
            waypoints: waypoints,   // [{x:1,y:2,yaw:0}, {x:3,y:4,yaw:1.57}, ...]
            loop: loop,
        }),
    });
    return await res.json();
}

// 示例：设定 3 个巡航点并循环
startPatrol([
    { x: 1.0, y: 0.0, yaw: 0 },
    { x: 3.0, y: 2.0, yaw: 1.57 },
    { x: 0.0, y: 3.0, yaw: 3.14 },
], true);
```

### 14.7 完整启动流程

```bash
# ── 步骤 1: 建图（首次或环境变化时执行） ──

# 终端 1: MID-360 驱动
ros2 launch livox_ros_driver2 msg_MID360_launch.py

# 终端 2: FAST-LIO2 建图
ros2 launch fast_lio mapping.launch.py config_file:=mid360.yaml

# 控制机器人走遍目标区域...完成后 Ctrl+C 保存 PCD

# 转换 3D → 2D 地图
python3 pcd_to_2d_map.py ~/PCD/scans.pcd ~/maps/my_map

# ── 步骤 2: 导航 ──

# 终端 1: MID-360 驱动
ros2 launch livox_ros_driver2 msg_MID360_launch.py

# 终端 2: FAST-LIO2 定位（或 AMCL）
ros2 launch fast_lio mapping.launch.py config_file:=mid360.yaml

# 终端 3: Nav2 导航
ros2 launch go2_navigation go2_nav2.launch.py

# 终端 4: API 服务
python3 go2_api_server.py

# ── 步骤 3: 打开网页前端 ──
# 浏览器访问: http://<ORIN_IP>:8080/docs  — API 文档
# 或自行部署 Vue/React 前端项目
```

### 14.8 导航开发最佳实践

1. **建图**：移动速度 ≤0.5m/s，尽量回环闭合，避免纯旋转
2. **定位**：AMCL 适合 2D 场景，FAST-LIO2 适合 3D 复杂场景
3. **Nav2 调参**：先调 costmap 膨胀半径（`robot_radius` + 余量），再调 controller 速度限制
4. **安全**：`cmd_vel_bridge` 中务必加限幅保护和超时停止
5. **前端**：使用 WebSocket 实时推送状态，避免高频 HTTP 轮询
6. **部署**：API 服务运行在 Orin NX 上，前端可部署在任意 Web 服务器或直接通过 Nginx 代理
7. **多机**：若需控制多台 Go2，可通过不同的 DDS Domain ID 或 ROS2 Namespace 隔离

---

## 第十五章 扩展硬件开发

### 15.1 扩展坞

Go2 EDU 可选配 NVIDIA Jetson 扩展坞：

| 型号 | Orin Nano 8GB | Orin NX 16GB |
|------|--------------|--------------|
| 算力 | 40 TOPS | 100 TOPS |
| 供电范围 | 16-60V DC | 16-60V DC |
| 接口 | USB 3.0 A×1, USB 3.0 C×1, USB 2.0 C×1, 千兆网口×2, 百兆网口×1, M8 航空插头×1 | 同左 |

### 15.2 深度相机 D435i

Go2 EDU 标配 Intel RealSense D435i：

| 参数 | 数值 |
|------|------|
| 尺寸 | 124mm × 29mm × 26mm |
| 最小深度 | 0.105m |
| 深度分辨率 | 1280×720@30fps / 848×480@90fps |
| 深度视场角 | 86° × 57°（±3°） |

### 15.3 小型伺服机械臂 D1 / D1-550

| 参数 | D1 | D1-550 |
|------|-----|--------|
| 重量 | 约 2.37kg | 约 3.15kg |
| 自由度 | 6+1（夹爪） | 6+1（夹爪） |
| 负载 | 500g | 500g |
| 最大臂展 | 670mm（含夹爪） | 670mm（含夹爪） |
| 通信方式 | RJ45 (ETH) | RJ45 (ETH) + Type-C |
| 控制方式 | DDS 订阅 | DDS 订阅 |
| 控制周期 | - | 10Hz |

### 15.4 双目摄像头

| 参数 | 数值 |
|------|------|
| 尺寸 | 106mm × 25mm × 41mm |
| 重量 | 108g |
| 功耗 | 3W |
| 视场角 | FOV 120° |
| 探照灯 | 8W |

### 15.5 声光三合一模组

| 参数 | 数值 |
|------|------|
| 尺寸 | 270mm × 143mm × 133mm |
| 重量 | 690g |
| 亮度 | 3000lm |
| 最大音压 | 130dB |
| 有效传声距离 | 500m |
| 最大传声距离 | 800m |
| 喇叭功率 | 30W |
| 功能 | 实时喊话、录音、音频播放、文字转语音、照明、红蓝闪灯 |

### 15.6 带屏遥控器

| 参数 | 数值 |
|------|------|
| 屏幕 | 5.5 寸 1080P LCD 触屏 |
| 系统 | Android 9.0，2GB RAM，16GB 存储 |
| 电池 | 10200mAh 7.4V |
| 快充 | PD 20W |
| 续航 | 约 13 小时 |
| 接口 | HDMI×1, USB-A, Type-C, SIM 卡, TF 卡 |

---

## 第十六章 App 与图形化编程

### 16.1 Go2 App

Go2 APP 下载地址: https://www.unitree.com/app/go2

功能：
- **实时图传**: RTT 2.0 高清图像传输
- **遥控操作**: 通过手机/平板控制机器人
- **服务管理**: 开启/关闭运动控制服务
- **OTA 升级**: 云端自动更新固件
- **3D 建图**: 使用 LiDAR 建图并设置路径
- **智能伴随**: ISS 2.0 跟随模式
- **语音交互**: 离线语音指令、对讲、音乐播放

### 16.2 图形化编程

Go2 支持图形化编程（全系标配），适合教育场景和快速原型开发。

### 16.3 服务状态管理

通过 App → 设备 → 服务状态 可以：
- 查看当前运行的服务
- 开启/关闭 MCF 运动控制服务
- 切换 AI/Normal/Advanced 模式

---

## 第十七章 开源项目索引

### 17.1 官方仓库

| 仓库 | Stars | 说明 |
|------|-------|------|
| [unitree_sdk2](https://github.com/unitreerobotics/unitree_sdk2) | - | C++ SDK（CycloneDDS） |
| [unitree_sdk2_python](https://github.com/unitreerobotics/unitree_sdk2_python) | - | Python SDK |
| [unitree_ros2](https://github.com/unitreerobotics/unitree_ros2) | - | ROS2 支持 |
| [unitree_mujoco](https://github.com/unitreerobotics/unitree_mujoco) | 866 | MuJoCo 仿真器 |
| [unitree_rl_gym](https://github.com/unitreerobotics/unitree_rl_gym) | 3k | 强化学习训练框架 |

### 17.2 社区热门项目

| 项目 | 说明 |
|------|------|
| [go2_ros2_sdk](https://github.com/abizovnuralem/go2_ros2_sdk) | Go2 ROS2 高层 SDK 封装 |
| [unitree_legged_sdk](https://github.com/unitreerobotics/unitree_legged_sdk) | 老版 Legged SDK（Go1 兼容） |
| [walk-these-ways](https://github.com/Improbable-AI/walk-these-ways) | MIT 四足步态学习 |
| [legged_gym](https://github.com/leggedrobotics/legged_gym) | ETH / legged_gym 基础框架 |
| [rsl_rl](https://github.com/leggedrobotics/rsl_rl) | ETH 强化学习算法 |
| [Isaac Lab](https://github.com/isaac-sim/IsaacLab) | NVIDIA Isaac 仿真平台 |

### 17.3 URDF / MJCF 模型

- **MuJoCo**: `unitree_mujoco/unitree_robots/go2/`
- **Isaac Lab**: NVIDIA Isaac 资源中心
- **ROS**: `unitree_ros2/cyclonedds_ws/unitree/unitree_go/`

### 17.4 文档与教程

| 资源 | 链接 |
|------|------|
| **官方文档中心** | https://support.unitree.com/home/en/developer |
| **Go2 产品页** | https://www.unitree.com/go2 |
| **开源页面** | https://www.unitree.com/opensource |
| **GitHub 组织** | https://github.com/unitreerobotics |
| **具身智能社群** | https://www.unifolm.com/ |
| **客服中心** | https://global-serviceconsole.unitree.com/ |

---

## 第十八章 安全须知与最佳实践

### 18.1 首次使用注意事项

1. **充分阅读官方文档**，了解各版本功能差异
2. **首次开机在开阔空地进行**，周围无贵重物品
3. **电池充满电**后再开始测试
4. **保持安全距离**，特别是运行特殊动作（前空翻等）时

### 18.2 低层控制安全

1. ⚠️ **必须先关闭 MCF 服务**再运行低层控制
2. **首次测试务必悬空四腿**
3. 从小力矩开始测试，逐步加大
4. 准备好 `Damp()` 急停命令
5. 不要同时运行多个控制程序

### 18.3 高层控制安全

1. 特殊动作（翻转、跳跃）需要在**平坦宽敞区域**执行
2. 前一个特殊动作**完成后**再执行下一个
3. 使用 `SpeedLevel(-1)` 慢速模式调试
4. 运行前确认机器人状态正常（指示灯绿色常亮）

### 18.4 网络安全

1. 不要在公共网络上暴露 DDS 通信
2. 注意 DDS Domain ID 隔离
3. 仿真和实机使用不同的 Domain ID

### 18.5 电池安全

1. 使用官方充电器充电
2. 不在高温/低温环境下充电
3. 低电量警告（黄色慢闪）后及时充电
4. 长期存放时保持 50-70% 电量

### 18.6 机械安全

1. 避免在光滑/湿滑地面运行
2. 注意载荷不超过限制（EDU: 8kg 常规，12kg 极限）
3. 定期检查关节有无异响
4. 避免关节长时间满负载运行

---

## 第十九章 常见问题与故障排查

### 19.1 连接问题

**Q: ping 192.168.123.161 不通？**

A: 检查：
1. 网线是否连接牢固
2. PC 网卡 IP 是否设置为 `192.168.123.222`（子网 `255.255.255.0`）
3. 机器人是否已开机（绿灯亮起）
4. 网卡是否启用

**Q: SDK 连接超时？**

A: 检查：
1. 网卡名是否正确（`ifconfig` 查看）
2. Domain ID 是否正确（实机=0，仿真=1）
3. CycloneDDS 版本是否为 0.10.x

### 19.2 控制问题

**Q: 低层控制发送命令无反应？**

A:
1. 确认已关闭 MCF 服务（App 或 MotionSwitcherClient）
2. 确认 MotorCmd.mode = 0x01
3. 确认 CRC 校验正确
4. 检查电机编号是否正确

**Q: 机器人运行特殊动作时摔倒？**

A:
1. 确保在平坦地面执行
2. 确保电池电量充足
3. 等待前一个动作完成再执行下一个
4. 检查机器人软件版本是否最新（OTA 更新）

### 19.3 仿真问题

**Q: unitree_mujoco 编译失败？**

A:
1. 检查 MuJoCo 版本和链接路径
2. 确认 `unitree_sdk2` 已安装到 `/opt/unitree_robotics`
3. 安装所有依赖: `libyaml-cpp-dev libspdlog-dev libboost-all-dev libglfw3-dev`

**Q: 仿真中机器人行为和实机不同？**

A:
1. 检查仿真时间步设置（`SIMULATE_DT`）
2. 确保使用正确的 MJCF 模型
3. 强化学习策略需要通过 Sim2Sim 验证
4. 关注域随机化参数配置

### 19.4 ROS2 问题

**Q: `ros2 topic list` 看不到机器人 Topic？**

A:
1. 确认已 `source ~/unitree_ros2/setup.sh`
2. 确认 CycloneDDS 版本编译正确（0.10.x）
3. 确认 `CYCLONEDDS_URI` 中网卡名正确
4. 确认 `RMW_IMPLEMENTATION=rmw_cyclonedds_cpp`
5. 重启电脑后重试

**Q: ROS2 编译 CycloneDDS 报错？**

A: 确保编译时**没有** source ROS2 环境。注释掉 `~/.bashrc` 中的 `source /opt/ros/foxy/setup.bash`，重新开一个终端编译。

### 19.5 强化学习问题

**Q: Isaac Gym 安装失败？**

A: Isaac Gym 需要：
- Python 3.8
- CUDA 11.7+
- Ubuntu 20.04/22.04
- NVIDIA 开发者账号下载

**Q: 训练不收敛？**

A:
1. 检查奖励函数设计
2. 增大 `num_envs` 提高样本效率
3. 调整学习率和训练步数
4. 使用 `--headless` 提高训练速度
5. 参考预训练模型了解基线性能

### 19.6 硬件问题

**Q: 指示灯红色慢闪？**

A: 系统异常或硬件故障，联系宇树售后：
- 客服中心: https://global-serviceconsole.unitree.com/

**Q: 关节发出异响？**

A:
1. 检查是否有外物卡住
2. 检查关节温度是否过高
3. 降低负载
4. 联系售后检修

---

## 附录 A: DDS Topic 速查表

### 高频 Topic（rt/ 前缀）

| Topic | 消息类型 | 方向 | 说明 |
|-------|----------|------|------|
| `rt/sportmodestate` | SportModeState | 订阅 | 运动状态（实时） |
| `rt/lowstate` | LowState | 订阅 | 低层状态（实时） |
| `rt/lowcmd` | LowCmd | 发布 | 低层控制命令 |
| `rt/wireless_controller` | WirelessController | 订阅 | 遥控器状态 |

### 低频 Topic（lf/ 前缀）

| Topic | 消息类型 | 方向 | 说明 |
|-------|----------|------|------|
| `lf/sportmodestate` | SportModeState | 订阅 | 运动状态（低频） |
| `lf/lowstate` | LowState | 订阅 | 低层状态（低频） |

### 服务 Topic

| Topic | 说明 |
|-------|------|
| `/api/sport/request` | 运动控制请求（ROS2 用） |
| `/wirelesscontroller` | 遥控器状态（ROS2 用） |

### LiDAR Topic

| Topic | 说明 |
|-------|------|
| `utlidar/cloud` | LiDAR 点云数据 |

---

## 附录 B: 关节编号与限位速查表

### 关节编号

| ID | 名称 | 腿 | 关节 |
|----|------|----|------|
| 0 | FR_hip | 右前 | 机身 |
| 1 | FR_thigh | 右前 | 大腿 |
| 2 | FR_calf | 右前 | 小腿 |
| 3 | FL_hip | 左前 | 机身 |
| 4 | FL_thigh | 左前 | 大腿 |
| 5 | FL_calf | 左前 | 小腿 |
| 6 | RR_hip | 右后 | 机身 |
| 7 | RR_thigh | 右后 | 大腿 |
| 8 | RR_calf | 右后 | 小腿 |
| 9 | RL_hip | 左后 | 机身 |
| 10 | RL_thigh | 左后 | 大腿 |
| 11 | RL_calf | 左后 | 小腿 |

### 关节限位

| 关节类型 | 限位角度 |
|----------|----------|
| Hip（机身） | -48° ~ 48° |
| Thigh（前腿大腿） | -200° ~ 90° |
| Thigh（后腿大腿） | -260° ~ 30° |
| Calf（小腿） | -156° ~ -48° |

### 坐标系规则

- Hip 关节旋转轴: **x 轴**
- Thigh/Calf 关节旋转轴: **y 轴**
- 正方向: **右手定则**

---

## 附录 C: 高层运动控制接口速查表

| 函数 | 参数 | 说明 |
|------|------|------|
| `Damp()` | 无 | **紧急停止**，进入阻尼状态 |
| `BalanceStand()` | 无 | 平衡站立（姿态自适应） |
| `StopMove()` | 无 | 停止运动，恢复默认参数 |
| `StandUp()` | 无 | 站高（0.33m，关节锁定） |
| `StandDown()` | 无 | 趴下（关节锁定） |
| `RecoveryStand()` | 无 | 从任意姿态恢复站立 |
| `Euler(r, p, y)` | roll, pitch, yaw (rad) | 姿态控制 |
| `Move(vx, vy, vyaw)` | m/s, m/s, rad/s | 速度控制 |
| `Sit()` | 无 | 坐下 |
| `RiseSit()` | 无 | 从坐姿站起 |
| `SwitchGain(d)` | 0-4 | 切换步态 |
| `BodyHeight(h)` | m | 体高偏移 [-0.18~0.03] |
| `FootRaiseHeight(h)` | m | 抬脚高偏移 [-0.06~0.03] |
| `SpeedLevel(l)` | -1/0/1 | 速度档位 |
| `Hello()` | 无 | 打招呼 |
| `Stretch()` | 无 | 伸懒腰 |
| `Wallow()` | 无 | 打滚 |
| `Pose(flag)` | bool | 摆姿势 |
| `Scrape()` | 无 | 拜年 |
| `FrontFlip()` | 无 | 前空翻 |
| `FrontJump()` | 无 | 前跳 |
| `FrontPounce()` | 无 | 前扑 |
| `Dance1()` | 无 | 舞蹈第一段 |
| `Dance2()` | 无 | 舞蹈第二段 |
| `TrajectoryFollow(path)` | 30个PathPoint | 轨迹跟踪 |
| `SwitchJoystick(flag)` | bool | 遥控器响应开关 |
| `ContinuousGait(flag)` | bool | 连续步态开关 |
| `GetState(names, map)` | vectors | 获取当前状态 |

### 错误码

| 错误码 | 说明 |
|--------|------|
| 0 | 成功 |
| 3104 | DDS 超时 |
| 4101 | 轨迹点数量错误 |
| 4201 | 动作超时 |
| 4205 | 状态机未初始化 |
| 4206 | 执行动作前姿态不正确 |

---

> **联系宇树科技**: https://www.unitree.com/contact  
> **技术支持**: https://global-serviceconsole.unitree.com/  
> **官方论坛**: https://www.unifolm.com/
