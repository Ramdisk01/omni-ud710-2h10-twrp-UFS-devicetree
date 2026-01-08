# 展讯平台 T710 TWRP 恢复镜像


[![TWRP](https://img.shields.io/badge/TWRP-3.7.0_+-orange.svg)](https://twrp.me)
[![License](https://img.shields.io/badge/License-GPL--3.0-blue.svg)](LICENSE)

有关Unisoc UD710定制的 TWRP (Team Win Recovery Project) 恢复镜像。特别解决了 FBE (File-Based Encryption) 解密问题。

## 📋 树来源の设备信息

- **设备型号**: 科大讯飞学习机 X2Pro
- **芯片平台**: Spreadtrum/Unisoc
- **Android 版本**: 9
- **TWRP 版本**: 3.7.0

## ✨ 特性

- ✅ **FBE 解密支持**：解决展讯平台 FBE 解密问题
- ✅ **SELinux 宽容模式**：强制在恢复环境中使用宽容模式
- ✅ **ADB 支持**：完整的 ADB 调试功能
- ✅ **备份/恢复**：完整的系统备份和恢复
- ✅ **刷机支持**：支持刷入 ZIP 格式的刷机包
- ✅ **触摸屏支持**：完整的触摸屏驱动
- ✅ **中文界面**：完整的中文语言支持
- ⚠️ **（暂未合并） MTP 支持**：USB 文件传输

## ⚠️ 重要说明

如果你的设备不是科大讯飞学习机X2Pro，请先进行移植。勿直接使用此树或镜像。

如果编译时patch没有生效（即selinux启动初期enforcing，这将导致FBE解密失败），
请手动修改 AOSP 源码中的 SELinux 策略，以强制在恢复模式启动初期即为宽容模式。缘故：

1. 展讯平台使用 DTB 分区中的 `bootargs` 而非标准内核命令行参数
2. 厂商固件在启动早期强制设置 SELinux 为强制模式
3. 需要在源码层面修改才能确保 FBE 解密正常工作

**修改位置**: `system/core/init/selinux.cpp`
```cpp
bool IsEnforcing() {
    return false; // 强制返回 false 以确保 FBE 解密
}