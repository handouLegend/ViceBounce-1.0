# ViceBounce for GTA VC 1.0

将 [tek-95/ViceBounce](https://github.com/tek-95/ViceBounce) 移植到 **GTA Vice City 1.0 US 零售版**（3,088,896 字节的 `gta-vc.exe`）。

原版只支持 Steam 版（`PLUGIN_SGV_STEAM`），因为所有地址都是硬编码的。这个分支把它改成了 1.0。

## 功能

- 车辆模型跟随音乐节拍缩放（"弹跳"）
- 支持汽车/摩托/船/直升机/飞机
- 人物骨骼变形（含车内司机）
- 支持 WAV/MP3 音乐，MCI 播放

## 安装

1. 需要 GTA VC 1.0 US 零售版
2. 需要 ASI Loader（支持 `scripts` 子目录，如 Ultimate ASI Loader）
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
| Page Up/Down | BPM ±5 |
| Home/End | 幅度 ±0.025 |

## 移植笔记

### 地址偏移规律

| 区域 | 偏移 | 验证方式 |
|---|---|---|
| 光照函数（0x54–0x57） | +0x110 | Plugin-SDK 三版对照 |
| callsite（0x58C 段） | +0x1D0 | `CallMatches` 逐条验证 |
| callsite（0x60B 段） | +0x3E0 | 同上 |
| 载具 vtable（0x69） | +0x1000 | 构造函数 `mov [this], imm32` |
| RenderWare（0x64） | +0x1050 | 签名搜索 |
| 数据段（0xA0） | **+0xFF8** | 从 `mov byte [0xA10AB2],1` 反推 |

**注意数据段是 +0xFF8 不是 +0x1000**——签名里的绝对地址也要跟着改。

### cutsceneVtable

Steam 是 `0x690594`，但不是 +0x1000。

从 `CCutsceneObject::CCutsceneObject`（`0x4E04D0`）里读：

```asm
C7 03 8C 15 69 00   mov [ebx], 0x69158C
```

真实地址 = `0x69158C`。

### 崩溃根因

patch callsite 时**不能无条件写**：

```cpp
// 错：不管这里是不是 call
std::memcpy(code+1, &rel, 4);

// 对：先校验
if(code[0]!=0xE8 || CallTarget(site.address)!=site.target) continue;
```

1.0 里有 11 条 callsite 不是 `E8`，写进去会把指令写坏，崩在 `0x58D954`。

### 已知限制

- `RegisterBrightLight` 的 11 个调用点跳过（1.0 里读不到 `E8`）
- `drawing` / 3 个 `CallMatches` 校验跳过（地址错位，无法定位）
- 需关闭 SilentPatch 的 `EnableVehicleCoronaFixes`

## 构建

```bash
export PLUGIN_SDK_DIR=/path/to/plugin-sdk
premake5 vs2022
msbuild build/ViceBounce.vcxproj /p:Configuration=Release /p:Platform=Win32
```

## 许可

MIT，基于 [tek-95/ViceBounce](https://github.com/tek-95/ViceBounce)。
