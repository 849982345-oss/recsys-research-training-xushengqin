# 第七次课：实验复现与代码精读

> 这节课把第六课的“论文理解”接到代码和实验：先确认环境能运行，再追踪程序入口、数据、模型、loss、参数更新和评价指标。

## 1. 本节课的总链路

```text
环境核验 -> 启动实验 -> 找程序入口 -> 跟数据流
    -> 对应模型和公式 -> 观察训练更新 -> 核对评价指标 -> 判断证据边界
```

重点不是记住所有代码行，而是回答：数据如何进入模型、loss 如何计算、参数如何更新、指标如何得到、运行结果能证明什么。

## 2. 环境配置：先复用，再补缺失

优先检查已有环境，不要因为环境名字不同就重新下载整套 PyTorch。只有缺少依赖、版本冲突或确实需要隔离时，才创建或调整环境。

```powershell
python --version
python -m pip --version
nvidia-smi
python -c "import torch; print(torch.__version__); print(torch.version.cuda); print(torch.cuda.is_available()); print(torch.cuda.get_device_name(0))"
```

要分清：`nvidia-smi` 的 CUDA Version 是驱动支持上限；`torch.version.cuda` 是当前 PyTorch 使用的 CUDA Runtime；`torch.cuda.is_available()` 是当前解释器是否真正能使用 GPU。

你本次环境已经验证过 RTX 5070 Ti 和 CUDA 13.0 可用；这说明当前解释器具备运行 GPU 实验的条件，但仍要看具体项目的依赖和显存需求。

## 3. 两种启动实验方式

### 命令行

```powershell
python main.py --dataset=ml-1m --train_dir=default --maxlen=200 --dropout_rate=0.2
```

优点是命令完整、参数清楚、容易记录和复现。运行前必须确认当前目录真的包含 `main.py` 和 `data/`。

### VS Code

- 直接运行：依赖 CLI 默认参数；
- Run and Debug：用 `.vscode/launch.json` 保存入口、参数、工作目录和环境变量。

两种方式最终都会进入同一个程序入口。启动前检查 VS Code 选择的 Python 解释器和终端中的解释器是不是同一个。

## 4. 通用排错顺序

```text
Traceback 最后一行
-> 第一个项目代码栈帧
-> 当前工作目录、文件和解释器
-> 版本和设备
-> 每次只改一个条件
-> 用原命令重试并记录
```

常见问题：

| 现象 | 先检查 |
|---|---|
| 找不到文件 | 工作目录、文件名、相对路径、`data/` |
| ModuleNotFoundError | `python` 和 `pip` 是否属于同一环境 |
| ImportError / AttributeError | Python、PyTorch 和关键依赖版本 |
| CUDA 不可用或显存不足 | `nvidia-smi`、PyTorch Runtime、batch size |
| 没有有效用户 | 数据格式、数据集参数、用户交互数是否大于 1 |
| 无法写输出 | 输出路径、权限、同名目录和磁盘空间 |

不要一次重装所有东西，也不要同时改路径、版本、参数和代码。

## 5. 从入口建立主流程

以 `sasrec_pytorch` 代码为例，可以先看：

```text
main.py::build_parser
    -> main.py::run
    -> data_partition
    -> SASRecDataset / DataLoader / sasrec_collate
    -> Model
    -> Adam
    -> epoch 循环
    -> next(train_iterator)
    -> model(seq, pos, neg)
    -> loss.backward()
    -> optimizer.step()
    -> evaluate / evaluate_valid
    -> log.txt
```

第一遍读代码只回答：程序按什么顺序执行、每个调用负责什么、参数和输出从哪里进出。看到调用时先记职责，需要解释时再跳入函数定义，回答后返回入口。

## 6. 数据流和张量形状

课堂代码中的训练 batch 大致包含：

| 变量 | 含义 | 典型形状 |
|---|---|---|
| `u` | 用户编号 | `[B]` |
| `seq` | 输入行为序列 | `[B, T]` |
| `pos` | 每个位置的正样本 | `[B, T]` |
| `neg` | 每个位置的负样本 | `[B, T]` |

其中 `B` 是 batch size，`T` 是最大序列长度。代码里补齐的位置通常用 0 表示；只对有效目标计算 loss。

调试时先看 `shape`、`dtype`、`device` 和少量切片，不要展开完整 GPU 张量。

## 7. 论文公式如何对应代码

### 7.1 表示层

```text
[B, T] 的物品 ID
    -> item_embedding
    -> 加 position_embedding
    -> [B, T, H]
```

`H` 是隐藏维度。代码中序列侧物品 Embedding 可以带缩放，目标物品查表使用共享物品表但不使用同样的序列侧缩放；因此不能只看变量名相似就断定两个张量完全相同。

### 7.2 Self-Attention

```text
Q = projection(queries)
K = projection(keys)
V = projection(values)
Attention = softmax(QK^T / sqrt(head_dim)) V
```

要核对三件事：缩放使用哪个维度、Softmax 沿哪个维度、Q 与 K/V 分别来自哪个张量。

### 7.3 Mask

- `Causal Mask`：屏蔽未来位置；
- `Key Mask`：不让模型把 Padding 当成可关注内容；
- `Query Mask`：不让 Padding 位置产生有效查询结果。

Mask 通常改变参与聚合的位置，不改变输出 shape。

### 7.4 FFN、残差和归一化

一个 Block 可以理解为：

```text
归一化 -> Self-Attention -> 残差
归一化 -> FFN -> 残差
```

不要用自己熟悉的 Transformer 顺序替代当前代码的实际顺序；要回到 `modules.py` 和 `model.py` 核对。

### 7.5 训练目标

模型对正、负候选分别打分：

```text
正样本分数 = 序列表示 · 正物品 Embedding
负样本分数 = 序列表示 · 负物品 Embedding
```

再用二元交叉熵式目标让正样本分数更高、负样本分数更低。Padding 位置不进入 loss，分母应使用有效目标数量，而不是简单使用 `B*T`。

## 8. 训练循环到底做了什么

每个 batch 的顺序是：

```text
取一批数据
-> zero_grad 清空旧梯度
-> forward 计算 loss
-> backward 计算梯度
-> step 更新参数
```

课堂移植代码中使用 Adam；训练轮次、batch size、学习率、隐藏维度、Block 数、head 数、dropout 和 L2 正则等由参数控制。

一个重要细节是：SASRec 的 `IterableDataset` 持续生成随机训练样本，epoch 的更新次数由代码中的 `num_batch` 决定，不是普通有限数据集的 `len(dataset)`。

## 9. 评价指标和候选集

课堂介绍的评价方式是：

```text
1 个真实目标 + 100 个随机负物品
    -> 对 101 个候选打分
    -> 找真实目标的 rank
    -> 计算 HR@10 和 NDCG@10
```

- `HR@10`：真实目标是否进入前 10；
- `NDCG@10`：不仅看是否进入前 10，还看进入后排在第几名。

如果 `rank` 从 0 开始，真实目标排第 1 名时 `rank=0`，所以课堂代码使用 `log2(rank + 2)`。同名指标在不同候选集、切分和负采样协议下不能直接比较。

## 10. 结果证据边界

比较实验前要对齐：数据和版本、过滤与切分、模型和超参数、训练策略和预算、候选集和负采样、指标定义、随机种子和环境。

结论最好分三层：

1. 实验事实：日志或表格直接显示了什么；
2. 作者解释：作者认为为什么出现；
3. 个人判断：证据支持到哪一步，还缺什么。

例如“某模型在当前数据集、当前切分、当前候选集和当前指标下 NDCG@10 更高”是稳妥结论；不能仅凭一次运行就说它在所有数据集都更好。

## 11. `sasrec_pytorch.zip` 如何使用

这个压缩包的定位是“代码精读和复现入口”：

| 文件 | 先看什么 |
|---|---|
| `README.md` | 来源、运行命令、环境和实现边界 |
| `main.py` | 参数、训练循环、日志和评价调用 |
| `model.py` | 模型结构、loss、AUC、predict |
| `modules.py` | Embedding、Attention、FFN、LayerNorm |
| `sampler.py` | 训练样本生成和 batch 组织 |
| `util.py` | 数据划分、HR@10、NDCG@10 |
| `.vscode/launch.json` | VS Code 启动和调试配置 |
| `tests/` | 移植后的静态检查和行为验证 |
| `TF_TO_PYTORCH_MAPPING.md` | TensorFlow 官方实现与 PyTorch 文件对应关系 |

推荐精读顺序：

```text
README.md
-> main.py
-> sampler.py / util.py
-> model.py
-> modules.py
-> tests/
```

不要一开始按文件从头读到尾；围绕“数据如何进模型、loss 如何算、指标如何得出”跳转。

## 12. 远程长期运行

课堂介绍了 Linux 远程环境中的两种会话保持方式：

```bash
nohup python main.py ... > train.log 2>&1 &
tail -f train.log
```

以及：

```bash
tmux new -s sasrec
# 启动实验后按 Ctrl+B，再按 D 分离
tmux attach -t sasrec
```

本地 Windows + VS Code 学习阶段不必强行使用这两个命令；理解它们是为以后远程 GPU 训练做准备。

## 13. 第七课后实践顺序

1. 用 `README.md` 确认项目来源、数据集和启动方式。
2. 在 VS Code 选择正确解释器，运行环境核验命令。
3. 先运行项目自带测试或最小配置。
4. 从 `main.py` 追踪到 `DataLoader`、`Model`、loss 和 evaluation。
5. 在 `next(train_iterator)`、模型输入、loss 和评价 rank 处打断点。
6. 记录每个位置的调用者、输入输出、shape、dtype 和 device。
7. 把论文公式、代码变量和张量形状写成对应表。
8. 只改变一个主要参数，重新运行并记录结果。
9. 把命令、环境、参数、日志和结论保存到实验记录。

## 14. 最后记住一句话

```text
环境能跑，不等于实验可信；代码能跑，不等于论文复现；
指标变高，不等于机制被证明。
```
