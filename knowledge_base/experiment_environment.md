# 推荐系统实验环境与命令

## 一、三层结构

一次推荐系统实验通常包含三层：

```text
操作系统与硬件
  → Conda/Python/依赖库环境
  → 项目代码、数据和实验参数
```

排查问题时要先判断是哪一层出了问题，不要一开始就修改模型代码。

## 二、Conda 环境

Conda 用于创建相互隔离的 Python 环境，避免不同项目的 Python 版本和依赖互相影响。

```powershell
conda create -n lesson03 python=3.10 -y
conda activate lesson03
conda deactivate
python --version
where.exe python
```

看到终端前面有 `(lesson03)`，表示当前已经进入环境。`where.exe python` 的第一条路径应该指向：

```text
Anaconda3\envs\lesson03\python.exe
```

## 三、PowerShell 与 VS Code

VS Code 是编辑器和终端工具，真正运行代码的是当前选择的 Python 解释器。使用 VS Code 时应确认：

1. 终端前面显示 `(lesson03)`；
2. `python --version` 是目标版本；
3. `where.exe python` 的第一条路径属于目标 Conda 环境；
4. `Python: Select Interpreter` 选择了 lesson03。

如果出现 `profile.ps1` 禁止执行脚本，通常是 PowerShell 没有加载 Conda 初始化脚本。当前用户可以使用：

```powershell
Set-ExecutionPolicy -Scope CurrentUser -ExecutionPolicy RemoteSigned
```

不要随意设置为 `Unrestricted`。

## 四、PyTorch、CUDA 与 GPU

PyTorch 版本、CUDA runtime 和 GPU 架构需要匹配。能够显示 GPU 名称不代表训练一定兼容，还要关注是否出现架构警告。

本机 RTX 5070 Ti 的 capability 为 `(12, 0)`，使用 PyTorch 2.12.1 的 cu126 版本时出现不兼容警告，改用 cu130 后正常：

```text
PyTorch: 2.12.1+cu130
CUDA runtime: 13.0
CUDA available: True
GPU: NVIDIA GeForce RTX 5070 Ti
```

检查 GPU：

```powershell
nvidia-smi
python -c "import torch; print(torch.__version__)"
python -c "import torch; print(torch.version.cuda)"
python -c "import torch; print(torch.cuda.is_available())"
python -c "import torch; print(torch.cuda.get_device_name(0))"
python -c "import torch; print(torch.cuda.get_device_capability(0))"
```

`torch.cuda.is_available()` 为 True 只是第一步；如果出现 `sm_120 is not compatible` 等警告，应更换支持当前 GPU 架构的 PyTorch/CUDA wheel。

## 五、NumPy 警告

如果出现：

```text
Failed to initialize NumPy: No module named 'numpy'
```

说明当前 Conda 环境缺少 NumPy，不一定表示 PyTorch 安装失败。安装：

```powershell
python -m pip install numpy
```

## 六、项目运行前的检查顺序

```text
确认当前目录
  → 确认 Conda 环境
  → 确认 Python 和 PyTorch
  → 确认 CUDA/GPU
  → 确认数据文件
  → 运行单元测试
  → 再运行完整实验
```

常用命令：

```powershell
pwd
Get-Content data\smoke_interactions.csv -TotalCount 6
python -m unittest discover -s tests -v
```

## 七、实验可复现记录

每次实验至少记录：

- Python、PyTorch、CUDA 和 GPU 版本；
- Conda 环境名称；
- 数据集和数据切分方式；
- 模型参数；
- 训练轮数、batch size 和随机种子；
- Loss、HR、NDCG 和资源占用；
- 日志、指标文件和模型保存位置。

不要把密码、Token、API Key 或个人隐私信息写入实验记录。
