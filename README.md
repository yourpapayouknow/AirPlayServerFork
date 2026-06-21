# AirPlayServer：Windows 上的 AirPlay 接收器

AirPlayServer 是一个运行在 Windows 上的 AirPlay 接收器，可以接收 iPhone、iPad 和 macOS 的屏幕镜像、视频流和音频播放。项目重点是低延迟、稳定播放，以及适合桌面使用的窗口化界面。

英文说明请看 [README_en_US.md](README_en_US.md)。

> 本项目基于 [fingergit/airplay2-win](https://github.com/fingergit/airplay2-win) 继续维护和改进。

## 快速安装

1. 到 [Release 页面](https://github.com/yourpapayouknow/AirPlayServerFork/releases/latest) 下载最新版压缩包。
2. 解压到任意文件夹。
3. 如果电脑没有安装 Bonjour，请先安装 [Bonjour for Windows](https://support.apple.com/kb/DL999)。安装 iTunes 也会附带安装 Bonjour。
4. 运行 `AirPlayServer.exe`。

程序启动后会自动在局域网里广播 AirPlay 服务。如果 Bonjour 没有安装或服务没有运行，程序启动时会提示。

## 系统要求

- Windows 10 或 Windows 11，64 位系统
- Apple Bonjour for Windows，用于局域网设备发现
- iPhone、iPad、Mac 和电脑需要处在同一个局域网内

## 主要特性

- 支持 iOS / iPadOS / macOS 的 AirPlay 镜像、视频和音频
- 默认 60 FPS，界面中可切换 30 / 60 FPS
- 可切换画质模式：高质量、均衡、极速
- 窗口可自由缩放，也支持全屏
- 低延迟渲染，YUV 视频帧通过 SDL2 纹理上传显示
- 内置性能面板，方便观察帧率、码率和延迟
- 界面会按窗口大小和 Windows 缩放比例调整，避免字太小

## 使用方法

1. 运行 `AirPlayServer.exe`。
2. 在 iPhone 或 iPad 上打开控制中心。
3. 点击“屏幕镜像”。
4. 选择你的 Windows 电脑名称。
5. 连接成功后，镜像画面会显示在电脑窗口中。

macOS 上可以从菜单栏或系统设置里的 AirPlay / 屏幕镜像入口连接。

## 界面和快捷键

启动后的首页可以设置设备名称、画质和目标帧率。连接后也可以在叠加面板里继续调整这些选项。

| 操作 | 功能 |
| --- | --- |
| `H` | 显示或隐藏叠加面板 |
| `F` | 切换全屏 |
| 双击窗口 | 切换全屏 |
| `F1` | 显示或隐藏性能图表 |
| 移动鼠标 | 显示鼠标指针，闲置后自动隐藏 |

## 画质和帧率

画质和帧率是两个独立选项，可以按实际网络和电脑性能搭配。

| 选项 | 说明 | 适合场景 |
| --- | --- | --- |
| Good | Lanczos 缩放，画面更细腻 | 画质优先 |
| Balanced | 双线性缩放 | 日常使用，默认推荐 |
| Fast | 最近邻缩放 | 延迟优先或低性能机器 |
| 30 FPS | 降低刷新压力 | 网络不稳或机器负载高 |
| 60 FPS | 更流畅 | 默认推荐 |

## 常见问题

### iPhone / iPad 找不到电脑

- 确认 Bonjour 已安装，并且“Bonjour Service”正在运行。可以在 `services.msc` 里查看。
- 确认手机和电脑在同一个 Wi-Fi 或同一个局域网网段。
- 检查 Windows 防火墙，允许 `AirPlayServer.exe` 在专用网络中通信。
- 如果电脑开了 VPN、代理或虚拟网卡，先临时关闭再试。

### 能连接，但没有画面或声音

- 如果 Windows 运行在虚拟机里，网络模式建议使用桥接，不要使用 NAT。
- 确认没有其他 AirPlay 接收软件占用相关端口。
- 尝试重新连接，或者在手机上关闭再打开屏幕镜像。

### 画面卡顿

- 先把帧率切到 30 FPS 试试。
- 画质可以从 Balanced 切到 Fast。
- 尽量使用 5GHz Wi-Fi，或者让电脑使用有线网络。

## 从源码构建

需要 Visual Studio 2022、v143 工具集和 Windows 10 SDK。

```powershell
msbuild AirPlay.sln /p:Configuration=Release /p:Platform=x64
```

也可以直接用 Visual Studio 打开 `AirPlay.sln`：

1. 右键 `AirPlayServer`，设为启动项目。
2. 选择 `Release | x64`。
3. 生成解决方案。

Release 输出位置：

```text
x64\Release\AirPlayServer.exe
```

运行时需要同目录下的 DLL。正常使用 Visual Studio / MSBuild 构建时，项目的 post-build step 会自动复制这些依赖。

## 项目结构

```text
AirPlayServer/
├── AirPlayServer/           # Windows GUI 主程序，基于 SDL2 和 ImGui
├── AirPlayServerLib/        # AirPlay / RAOP 协议、配对、加密等核心逻辑
├── airplay2dll/             # DLL 封装，以及 FFmpeg H.264 解码
├── dnssd/                   # Bonjour / mDNS 服务发现 DLL
├── external/                # SDL2、FFmpeg、ImGui 等第三方依赖
└── AirPlay.sln              # Visual Studio 解决方案
```

## 致谢

感谢 [fingergit/airplay2-win](https://github.com/fingergit/airplay2-win) 以及 AirPlay 逆向社区的长期工作。

## 说明

这是一个非官方实现。Apple、AirPlay 以及相关商标归 Apple Inc. 所有。
