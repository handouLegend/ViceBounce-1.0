# ViceBounce 1.0

将 [tek-95/ViceBounce](https://github.com/tek-95/ViceBounce) 移植到 GTA Vice City 1.0 US 零售版。

原版只支持 Steam 版，这个版本支持 1.0 的 `gta-vc.exe`（3,088,896 字节，2003-05-09）。

## 效果

车辆模型跟着音乐节拍缩放，看起来像在"弹跳"。支持汽车、摩托、船、直升机、飞机，人物也会跟着变形。

## 安装

1. GTA Vice City 1.0 US 零售版
2. 已装 ASI Loader（支持 `scripts` 子目录的那种）
3. 把 `ViceBounce.asi` 和 `ViceBounce.ini` 放进 `scripts\`
4. 启动游戏

## 热键

| 键 | 功能 |
|---|---|
| F6 | 开关汽车 |
| F7 | 重载配置 |
| F8 | 开关其他载具 |
| F9 | 对齐节拍 |
| F10 | 开关人物 |
| F11 | 选音乐 |
| Page Up / Down | BPM ±5 |
| Home / End | 幅度 ±0.025 |

## 音乐

支持 WAV / MP3，用 MCI 播放。

在 `ViceBounce.ini` 里设置：

```ini
MusicEnabled=1
MusicFile=你的音乐路径.wav
```

建议用 WAV，路径别带中文（MCI 对中文路径支持差）。

## 已知问题

- 车灯类的部分亮光不会跟着变形
- 如果装了 SilentPatch，需要把 `EnableVehicleCoronaFixes` 改成 0
- F7 重载配置有概率崩溃，改配置建议直接重启游戏

## 构建

```bash
export PLUGIN_SDK_DIR=/path/to/plugin-sdk
premake5 vs2022
msbuild build/ViceBounce.vcxproj /p:Configuration=Release /p:Platform=Win32
```

## 许可

MIT，基于 [tek-95/ViceBounce](https://github.com/tek-95/ViceBounce)。
