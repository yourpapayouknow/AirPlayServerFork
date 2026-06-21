# AirPlayServer：基于Windows10/11的AirPlay接收端

1. AirPlayServer目标接收iPhone、iPad和macOS的屏幕镜像、视频流和音频播放。
2. 项目低延迟、稳定播放，且配备GUI界面。
3. 英文说明请看[README_en_US.md](README_en_US.md)。

>本项目基于[fingergit/airplay2-win](https://github.com/fingergit/airplay2-win)继续维护和改进。

## 快速安装

1. 前往[Release页面](https://github.com/yourpapayouknow/AirPlayServerFork/releases/latest)下载最新版压缩包。
2. 解压到任意文件夹。
3. 如果电脑没有安装Bonjour，请先安装[BonjourforWindows](https://support.apple.com/kb/DL999)。安装iTunes也会附带安装Bonjour。
4. 运行`AirPlayServer.exe`。
5. 程序启动后会自动在局域网里广播AirPlay服务。如果Bonjour没有安装或服务没有运行，程序启动时会提示。

## 系统要求

- Windows10或Windows11，64位系统
- AppleBonjourforWindows，用于局域网设备发现
- iPhone、iPad、Mac和电脑需要处在同一个局域网内

## 主要特性

- 支持iOS/iPadOS/macOS的AirPlay镜像、视频和音频
- 默认60FPS，界面中可切换30/60FPS
- 可切换画质模式：GOOD、BALANCED、FAST
- 窗口可自由缩放，也支持双击全屏
- 低延迟渲染，YUV视频帧通过SDL2纹理上传显示
- 内置性能面板，方便观察帧率、码率和延迟
- 界面会按窗口大小和Windows缩放比例调整，避免字糊

## 使用方法

1. 运行`AirPlayServer.exe`。
2. 在iPhone或iPad上下拉打开控制中心。
3. 点击“屏幕镜像”。
4. 选择你的Windows电脑名称。
5. 连接成功后，镜像画面会显示在电脑窗口中。

- macOS上可以从菜单栏或系统设置里的AirPlay/屏幕镜像入口连接。

## 界面和快捷键

- 启动后的首页可以设置设备名称、画质和目标帧率。连接后也可以在叠加面板里继续调整这些选项。

|操作|功能|
|---|---|
|`H`|显示或隐藏叠加面板|
|`F`|切换全屏|
|双击窗口|切换全屏|
|`F1`|显示或隐藏性能图表|
|移动鼠标|显示鼠标指针，闲置后自动隐藏|

## 画质和帧率

- 画质和帧率是两个独立选项，可以按实际网络和电脑性能搭配。

|选项|说明|适合场景|
|---|---|---|
|Good|Lanczos缩放，画面更细腻|画质优先|
|Balanced|双线性缩放|日常使用，默认推荐|
|Fast|最近邻缩放|延迟优先或低性能机器|
|30FPS|降低刷新压力|网络不稳或机器负载高|
|60FPS|更流畅|默认|

## 常见问题

## iPhone/iPad找不到电脑

1. 确认Bonjour已安装，并且“BonjourService”正在运行。可以在`services.msc`里查看。
2. 确认手机和电脑在同一个Wi-Fi或同一个局域网网段。
3. 检查Windows防火墙，允许`AirPlayServer.exe`在专用网络中通信。
4. 如果电脑开了VPN、代理或虚拟网卡，先临时关闭再试。

## 能连接，但没有画面或声音

1. 如果Windows运行在虚拟机里，网络模式建议使用桥接，不要使用NAT。
2. 确认没有其他AirPlay接收软件占用相关端口。
3. 尝试重新连接，或者在手机上关闭再打开屏幕镜像。

## 画面卡顿

- 先把帧率切到30FPS试试。
- 画质可以从Balanced切到Fast。
- 尽量用5GWi-Fi，或者让电脑用有线网络，或者把宿舍里在恶意占用网络的舍敌狠狠压力一顿。

## 从源码构建

- 需要VisualStudio2022、v143工具集和Windows10SDK。

```powershell
msbuildAirPlay.sln/p:Configuration=Release/p:Platform=x64
```

- 或直接用VisualStudio打开`AirPlay.sln`：

1. 右键`AirPlayServer`，设为启动项目。
2. 选择`Release|x64`。
3. 生成解决方案。

- Release输出位置：

```text
x64\Release\AirPlayServer.exe
```

- 运行时需要同目录下的DLL。正常使用VisualStudio/MSBuild构建时，项目的post-buildstep会自动复制这些依赖。

## 项目结构

```text
AirPlayServer/
├──AirPlayServer/#WindowsGUI主程序，基于SDL2和ImGui
├──AirPlayServerLib/#AirPlay/RAOP协议、配对、加密等核心逻辑
├──airplay2dll/#DLL封装，以及FFmpegH.264解码
├──dnssd/#Bonjour/mDNS服务发现DLL
├──external/#SDL2、FFmpeg、ImGui等第三方依赖
└──AirPlay.sln#VisualStudio解决方案
```

## 致谢

感谢[fingergit/airplay2-win](https://github.com/fingergit/airplay2-win)以及AirPlay逆向社区的长期工作。

## 说明

这是一个非官方实现。Apple、AirPlay以及相关商标归AppleInc.所有。
