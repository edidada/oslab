# 哈工大操作系统实验环境总结
生成了 Image 文件 Image 文件文件在哪儿
ls -la /mnt/d/develops/git/github/cpp/oslab/linux-0.11/Image
-rwxrwxrwx 1 wdidada wdidada 128161 Mar 28 16:26 /mnt/d/develops/git/github/cpp/oslab/linux-0.11/Image

简单来说，整个流程是：编译生成 Image 文件 → 构建工具将其写入 hdc-0.11.img → Bochs 从 hdc-0.11.img 加载并启动内核。

## 项目概述

本项目是哈工大操作系统实验环境的配置脚本，主要包含 Bochs 虚拟机和 Linux-0.11 的源码。该环境用于学习和实验操作系统原理，特别是 Linux 0.11 的内核实现。

## 项目结构

- `linux-0.11/`：Linux 0.11 内核源码
- `bochs/`：Bochs 虚拟机相关文件
- `lab2/`：实验二相关测试文件
- `setup`：环境安装脚本
- `run`：启动 Bochs 虚拟机脚本
- `init`：重置 Linux-0.11 目录脚本
- `mount-hdc`：挂载虚拟硬盘脚本
- `hdc-0.11.img`：虚拟硬盘镜像

## 环境配置

### 支持的系统
- Debian 系发行版（Ubuntu、Debian、Deepin 等）
- Arch 系发行版（Arch Linux、Manjaro 等）
- WSL2（Windows Subsystem for Linux 2）

### 安装步骤
1. 克隆仓库：`git clone https://gitee.com/cn-guoziyang/oslab.git ~/oslab`
2. 进入目录：`cd ~/oslab`
3. 执行安装脚本：
   - Debian 系：`./setup debian`
   - Arch 系：`./setup archlinux`

## 编译过程

### 编译 Linux 0.11
1. 进入 linux-0.11 目录：`cd linux-0.11`
2. 执行编译命令：`make all`
3. 多核编译（可选）：`make -j 2`
4. 清理编译结果：`make clean`

### 遇到的问题及解决方案
1. **依赖问题**：需要安装 32 位库和 gcc-3.4 编译器
2. **键盘映射表重复定义问题**：修改 keyboard.S 文件，移除条件编译指令，只保留 KBD_US 定义
3. **Windows 换行符问题**：使用 dos2unix 工具修复脚本文件

## 运行方式

1. 启动 Bochs 虚拟机：`./run`
2. 汇编级调试：`./dbg-asm`
3. C 语言级调试：
   - 启动调试服务器：`./dbg-c`
   - 启动 GDB：`./rungdb`
   - 启动 CGDB：`./runcgdb`

## 文件交换

虚拟机与宿主机之间的文件交换通过 hdc-0.11.img 镜像文件进行：
1. 挂载镜像：`sudo ./mount-hdc`
2. 进行文件操作
3. 卸载镜像：`sudo umount hdc`
4. 在虚拟机中同步修改：`sync`

## 在 WSL 中的使用

1. 安装 X11 Server：`sudo apt install xorg`
2. 配置 X11 转发：
   - WSL 1：`export DISPLAY=localhost:0`
   - WSL 2：`export DISPLAY=$(cat /etc/resolv.conf | grep nameserver | awk '{print $2}'):0`
3. Windows 宿主机安装 X11 Client（如 VcXsrv）
4. 启动 VcXsrv，配置为 Multiple Window 模式，勾选 Disable access control
5. 在 WSL 中执行 `./run` 启动 Bochs

## 项目特点

1. 支持多种 Linux 发行版
2. 支持 WSL2 环境
3. 包含最新版本的 Bochs 模拟器（2.6.10）
4. 提供完整的调试支持（汇编级和 C 语言级）
5. 方便的文件交换机制
6. 一键配置脚本，简化环境搭建过程

## 总结

本项目为操作系统实验提供了一个完整的环境，包括 Linux 0.11 内核源码、Bochs 虚拟机以及相关工具。通过该环境，学生可以学习操作系统的基本原理，进行内核实验和调试，深入理解 Linux 0.11 的实现细节。

在 WSL2 Ubuntu 24.04 环境下，我们成功编译了 Linux 0.11 内核，并启动了 Bochs 虚拟机，验证了环境的可用性。虽然在编译过程中遇到了一些问题，但通过修改相关文件和配置，最终成功完成了编译和运行。