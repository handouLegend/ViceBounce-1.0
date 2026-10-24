# ViceBounce 1.0 适配版

基于 tek-95/ViceBounce 修改，MIT 许可。
原始项目: https://github.com/tek-95/ViceBounce

## 要求
- GTA Vice City 1.0 US 零售版 (gta-vc.exe, 3,088,896 字节)
- 已安装 ASI Loader (支持 scripts 子目录)

## 安装
1. 把 ViceBounce.asi 和 ViceBounce.ini 放进游戏的 scripts 文件夹
2. 启动游戏
3. 按 F6 开关汽车效果，F8 开关其他载具，F10 开关人物
4. 按 F11 选音乐（或用 MusicFile 指定路径）

## 改动摘要（相对原版）
- 载具 vtable: +0x1000
- 光照 callsite: +0x1D0 / +0x3E0
- 光照函数: +0x110
- RenderWare 函数: +0x1050
- getHierarchy: +0x1D0
- cutsceneVtable: 0x690594 -> 0x69158C
- 数据段地址: +0xFF8
- 修复: patch 前校验 E8, 避免破坏非调用点

## 已知限制
- 跳过 RegisterBrightLight 的 11 个调用点（签名不兼容）
- 跳过部分 CallMatches 校验（1.0 地址错位）
- 需关闭 SilentPatch 的 EnableVehicleCoronaFixes

## 音乐
WAV/MP3 都可以，MCI 对中文路径支持差，建议放短英文路径。
