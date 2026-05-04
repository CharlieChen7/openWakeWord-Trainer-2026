# 🚀 openWakeWord-Trainer-WSL-Fix

[![English](https://img.shields.io/badge/Language-English-blue)](#english-version) [![中文](https://img.shields.io/badge/Language-中文-red)](#中文版)
![OS](https://img.shields.io/badge/OS-WSL2%20(Ubuntu)-orange)
![Python](https://img.shields.io/badge/Python-3.10-blue)
![PyTorch](https://img.shields.io/badge/PyTorch-2.6%2B%20(CUDA%2011.8)-red)
![License](https://img.shields.io/badge/License-MIT-green)

---

<a id="english-version"></a>
## 🇬🇧 English Version

**Cross-Platform Local Training Fix Guide (Perfectly adapted for WSL2 / PyTorch 2.6+)**

The official auto-training script for [openWakeWord](https://github.com/dscripka/openWakeWord) is a great tool for custom wake words. However, in the latest 2026 hardware/software environments (especially WSL2 + PyTorch 2.6+), the original script suffers from severe technical debt, leading to endless crashes during environment setup and model generation.

This repository provides an **out-of-the-box, foolproof guide** and a fully patched Jupyter Notebook to help you bypass these bugs and train high-sensitivity, offline wake word models effortlessly.

### ✨ Core Fixes (Why this repo?)

1. **Path Isolation (`ModuleNotFoundError`):** Fixed the issue where Jupyter's `!` command swallows the `PYTHONPATH`. We hardcoded the absolute path at the top of the source code.
2. **PyTorch 2.6+ Security Lock (`_pickle.UnpicklingError`):** Overcame the new `weights_only=True` restriction in PyTorch 2.6+ by injecting a global Monkey Patch to bypass the security lock for legacy model loading.
3. **Broken API & 404 Links:** Replaced the dead AudioSet `.tar` download link with a working historical snapshot, and reverted the `piper-sample-generator` to a stable commit compatible with the original script.
4. **Dependency Hell:** Locked `numpy<2` and `pyarrow==14.0.1` to prevent memory crashes and attribute errors.

### 🛠️ Prerequisites
- **OS:** Windows 10/11 with WSL2 (Ubuntu 22.04 / 24.04 recommended).
- **GPU:** NVIDIA GPU (Host Windows driver is sufficient; no need to install drivers inside WSL2).
- **Network:** Global proxy recommended for downloading datasets (TUN mode preferred).

### 🚀 Quick Start
**Step 1: Setup the Environment (WSL2 Terminal)**
```bash
# 1. Accept Anaconda TOS (Required for new Miniconda)
conda tos accept --override-channels --channel [https://repo.anaconda.com/pkgs/main](https://repo.anaconda.com/pkgs/main)
conda tos accept --override-channels --channel [https://repo.anaconda.com/pkgs/r](https://repo.anaconda.com/pkgs/r)

# 2. Create and activate Sandbox
conda create -n oww_env python=3.10 -y
conda activate oww_env

# 3. Install PyTorch with CUDA support
pip uninstall torch torchvision torchaudio -y
pip install torch torchvision torchaudio --index-url [https://download.pytorch.org/whl/cu118](https://download.pytorch.org/whl/cu118)

# 4. Install Jupyter Lab inside the environment (CRITICAL)
pip install jupyterlab
```

**Step 2: Launch**
Run 
```
jupyter lab
```
in your terminal. Copy the URL with the `token=` and open it in an **Incognito/Private window** in your Windows browser to avoid conflicts with existing background instances.

**Step 3: Train**
Open `automatic_model_training_fixed.ipynb`, change the target wake word in the config section, and click `Run -> Run All Cells`.

---

<a id="中文版"></a>
## 🇨🇳 中文版

**跨平台本地炼丹避坑与修复指南（完美适配 WSL2 / PyTorch 2.6+）**

官方的 [openWakeWord](https://github.com/dscripka/openWakeWord) 自动训练脚本为自定义唤醒词提供了极大的便利。然而，在最新的软硬件环境下（尤其是 WSL2 + PyTorch 2.6+ 环境），原版脚本存在大量严重的“技术债”，会导致新手在环境搭建和模型生成时遭遇反复崩溃。

本项目提供了一套**开箱即用**的环境搭建指南，并提供了一个打满“物理外挂补丁”的全新 Jupyter Notebook，让你彻底告别无穷无尽的 Bug，实现一键生成高灵敏度专属离线唤醒模型。

### ✨ 核心修复清单 (Why this repo?)

1. **底层路径隔离阻断 (`ModuleNotFoundError`)：** 修复了 Jupyter `!` 命令行环境吞噬 `PYTHONPATH` 的问题，通过在源码绝对顶端硬编码路径，确保发音引擎绝对不迷路。
2. **PyTorch 2.6+ 安全锁背刺 (`_pickle.UnpicklingError`)：** 针对新版 PyTorch 强制开启 `weights_only=True` 拦截老版模型的问题，通过底层猴子补丁（Monkey Patch）注入全局抗体，彻底熔断安全锁限制。
3. **第三方 API 破坏与 404 坏链：** 替换了已失效的 AudioSet `.tar` 数据集下载链接，并自动将 `piper-sample-generator` 发音引擎回滚至与原脚本完美兼容的历史稳定快照。
4. **依赖项“神仙打架”：** 明确并锁定了 `numpy<2` 和 `pyarrow==14.0.1` 的降级策略，避免内存崩溃与属性报错。

### 🛠️ 准备工作 (Prerequisites)
- **系统：** Windows 10/11 并在系统中安装了 WSL2（推荐 Ubuntu 22.04 / 24.04）。
- **显卡：** NVIDIA GPU（**注意：** 仅需在 Windows 宿主机安装好显卡驱动即可，WSL2 内部无需重新安装驱动）。
- **网络：** 建议配置稳定的全局代理下载数据集（推荐使用 TUN 虚拟网卡模式接管底层流量）。

### 🚀 极速上手指南 (Quick Start)

**第一步：搭建纯净的环境 (WSL2 终端)**
```bash
# 1. 接受 Anaconda 协议
conda tos accept --override-channels --channel [https://repo.anaconda.com/pkgs/main](https://repo.anaconda.com/pkgs/main)
conda tos accept --override-channels --channel [https://repo.anaconda.com/pkgs/r](https://repo.anaconda.com/pkgs/r)

# 2. 创建专属沙盒并激活
conda create -n oww_env python=3.10 -y
conda activate oww_env

# 3. 安装带 CUDA 加速引擎的 PyTorch
pip uninstall torch torchvision torchaudio -y
pip install torch torchvision torchaudio --index-url [https://download.pytorch.org/whl/cu118](https://download.pytorch.org/whl/cu118)

# 4. 在沙盒内部安装真正的 Jupyter Lab (极其重要)
pip install jupyterlab
```

**第二步：点火启动**
在终端运行
```
jupyter lab
```
。必须复制输出的带有 `token=` 的长链接，并在 Windows 浏览器的**无痕模式**下打开，以防连接到旧的 Windows Jupyter 进程。

**第三步：一键炼丹**
打开 `automatic_model_training_fixed.ipynb`，在配置区域修改你的目标唤醒词，点击顶部的 `Run -> Run All Cells` 即可。

---

## 🤝 Acknowledgments / 鸣谢与贡献
Based on / 基于 [openWakeWord](https://github.com/dscripka/openWakeWord) & [piper-sample-generator](https://github.com/rhasspy/piper-sample-generator).
If this guide saved you hours of debugging, please consider giving it a ⭐ Star!
如果这个修复指南帮你省下了几个小时的 Debug 时间，欢迎给个 ⭐ Star！
