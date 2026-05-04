# 🚀 openWakeWord-Trainer-WSL-Fix
**跨平台本地炼丹避坑与修复指南（完美适配 WSL2 / PyTorch 2.6+）**

![OS](https://img.shields.io/badge/OS-WSL2%20(Ubuntu)-orange)
![Python](https://img.shields.io/badge/Python-3.10-blue)
![PyTorch](https://img.shields.io/badge/PyTorch-2.6%2B%20(CUDA%2011.8)-red)
![License](https://img.shields.io/badge/License-MIT-green)

官方的 [openWakeWord](https://github.com/dscripka/openWakeWord) 自动训练脚本为自定义唤醒词提供了极大的便利。然而，在 2026 年的最新软硬件环境下（尤其是 WSL2 + PyTorch 2.6+ 环境），原版脚本存在大量严重的“技术债”，会导致新手在环境搭建和模型生成时遭遇反复崩溃。

本项目提供了一套**开箱即用**的“保姆级”环境搭建指南，并提供了一个打满“物理外挂补丁”的全新 Jupyter Notebook，让你彻底告别无穷无尽的 Bug，实现一键生成高灵敏度专属离线唤醒模型。

---

## ✨ 核心修复清单 (Why this repo?)

本仓库彻底修复了原版代码中的以下致命问题：

1. **底层路径隔离阻断 (`ModuleNotFoundError`)：** 修复了 Jupyter `!` 命令行环境吞噬 `PYTHONPATH` 的问题，通过在源码绝对顶端硬编码路径，确保发音引擎绝对不迷路。
2. **PyTorch 2.6+ 安全锁背刺 (`_pickle.UnpicklingError`)：** 针对新版 PyTorch 强制开启 `weights_only=True` 拦截老版模型的问题，通过底层猴子补丁（Monkey Patch）注入全局抗体，彻底熔断安全锁限制。
3. **第三方 API 破坏与 404 坏链：** 替换了已失效的 AudioSet `.tar` 数据集下载链接，并自动将 `piper-sample-generator` 发音引擎回滚至与原脚本完美兼容的历史稳定快照。
4. **依赖项“神仙打架”：** 明确并锁定了 `numpy<2` 和 `pyarrow==14.0.1` 的降级策略，避免内存崩溃与属性报错。

---

## 🛠️ 准备工作 (Prerequisites)

- **系统：** Windows 10/11 并在系统中安装了 WSL2（推荐 Ubuntu 22.04 / 24.04）。
- **显卡：** NVIDIA GPU（**注意：** 仅需在 Windows 宿主机安装好显卡驱动即可，WSL2 内部无需重新安装驱动，微软已做好底层穿透）。
- **网络：** 由于需要下载大量海外数据集和模型，建议配置稳定的全局代理（推荐使用 TUN 虚拟网卡模式接管底层流量）。

---

## 🚀 极速上手指南 (Quick Start)

### 第一步：搭建纯净的“核动力炼丹炉”

在 WSL2 终端中，依次运行以下命令，创建一个隔离的虚拟环境并注入 GPU 算力：

```bash
# 1. (最新版 Miniconda 必须) 接受 Anaconda 协议
conda tos accept --override-channels --channel [https://repo.anaconda.com/pkgs/main](https://repo.anaconda.com/pkgs/main)
conda tos accept --override-channels --channel [https://repo.anaconda.com/pkgs/r](https://repo.anaconda.com/pkgs/r)

# 2. 创建专属 Python 3.10 沙盒并激活
conda create -n oww_env python=3.10 -y
conda activate oww_env

# 3. 安装带 CUDA 加速引擎的 PyTorch (替换纯 CPU 版本)
pip uninstall torch torchvision torchaudio -y
pip install torch torchvision torchaudio --index-url [https://download.pytorch.org/whl/cu118](https://download.pytorch.org/whl/cu118)

# 4. 在沙盒内部安装真正的 Jupyter Lab (极其重要，不要用 apt 全局安装)
pip install jupyterlab
```

### 第二步：点火启动

确保你仍在 `oww_env` 环境下，进入你的项目目录，启动中控大脑：

```bash
jupyter lab
```
*⚠️ **关键：** 必须复制 WSL 终端输出的带有 `token=` 的长链接，并在 Windows 浏览器的**无痕模式**下打开，以防连接到旧的 Windows Jupyter 进程。*

### 第三步：一键炼丹

1. 在 Jupyter Lab 中打开本项目提供的 `automatic_model_training_fixed.ipynb`。
2. 找到配置参数部分，修改你的目标唤醒词（例如 `hey cg lab`）。
3. 点击顶部的 `Run -> Run All Cells`。
4. 去喝杯咖啡，享受显卡狂飙的声音，等待专属 `.onnx` 模型在 `my_custom_model` 文件夹中出炉！

---

## 💡 常见问题排雷 (FAQ)

- **Q: 提示 `FileExistsError: [Errno 17] File exists...`**
  - **A:** 虚惊一场。说明你之前已经运行过创建文件夹的代码了，我已经将代码修改为 `os.makedirs(..., exist_ok=True)` 解决此问题。
- **Q: 终端出现红色的 `TqdmWarning: IProgress not found...`**
  - **A:** 纯视觉样式的警告，因为没有安装花哨的前端进度条组件，完全不影响训练速度和模型质量，请放心无视。
- **Q: 第一步生成音频时卡在 `119/120` 不动了？**
  - **A:** 数据集流式下载遭遇网络超时。修复版的 Notebook 中已加入了 `try-except` 容错机制，遇到卡死会自动跳出并继续执行后续步骤，不影响大局。

---

## 🤝 鸣谢与贡献

本项目基于 openWakeWord 和 piper-sample-generator 进行深度修复与二次封装。
如果这个修复指南帮你省下了几个小时的 Debug 时间，欢迎给个 ⭐ Star！
