# 第二次课：Python、Tensor 与推荐模型训练

> 课程主题：Python 基础、Tensor 基础、机器学习任务、推荐模型概览，以及一个从数据到验证/测试的 PyTorch 二分类实验。

## 1. 本节课知识主线

```text
Python 数据结构
    → Tensor 与 shape
    → Dataset / DataLoader
    → 模型前向计算得到 Logit
    → Loss
    → 梯度与反向传播
    → Optimizer 更新参数
    → Validation / Test
    → 实验记录与分析
```

## 2. Python 基础

### 2.1 变量、类型与运算

Python 变量不需要预先声明类型，常见类型包括：

```python
count = 3
learning_rate = 0.01
name = "model"
is_training = True
result = None
```

常用操作：

| 写法 | 作用 |
|---|---|
| `=` | 赋值，将对象绑定到变量 |
| `==`、`!=` | 判断是否相等或不等 |
| `>`、`<`、`>=`、`<=` | 数值比较 |
| `and`、`or`、`not` | 逻辑运算 |
| `type(x)` | 查看对象类型 |

### 2.2 常用数据结构

| 类型 | 可变 | 允许重复 | 典型用途 |
|---|---:|---:|---|
| `list` | 是 | 是 | 有顺序、需要修改的数据集合 |
| `tuple` | 否 | 是 | 固定结构，例如 Tensor 的 shape |
| `set` | 是 | 否 | 去重、成员判断、集合运算 |
| `dict` | 是 | 键不可重复 | 通过字段名组织结构化数据 |
| `str` | 否 | — | 文本、名称和路径 |

```python
values = [1.2, -0.7, 0.5]
values[0]
values[0:2]
values.append(0.8)
values[1] = -0.6

shape = (240, 2)
shape[0]

user_ids = {101, 102, 103, 101}
101 in user_ids

sample = {"features": [1.2, -0.7], "label": 1}
sample["label"]
sample.get("user_id")
```

注意：空集合要写成 `set()`；`{}` 表示空字典。列表和字符串使用从 `0` 开始的索引，并支持切片；字符串本身不能通过索引直接修改。

### 2.3 条件、循环与函数

```python
for index, sample in enumerate(samples):
    if sample["label"] == 1:
        category = "正类"
    else:
        category = "负类"
```

- `if / elif / else`：根据条件选择执行路径。
- `for`：遍历列表、字典或其他可迭代对象。
- `range(n)`：产生 `0` 到 `n - 1`。
- `enumerate(sequence)`：同时取得位置和元素。
- 缩进决定代码块层级。

```python
def function_name(parameter):
    result = parameter
    return result
```

阅读陌生函数时，先看输入参数和返回值，再看内部实现。复杂项目通常还会用 `class` 把数据、参数和行为组织在一起；`__init__` 用于初始化，`self` 表示当前对象。

## 3. Tensor 与 shape

### 3.1 为什么需要 Tensor

Tensor 可以统一表示标量、向量、矩阵和更高维数据，并支持：

- 批量计算；
- GPU/CPU 设备迁移；
- 自动微分；
- 与神经网络层和 Loss 函数直接连接。

### 3.2 维度与形状

| 数据 | 示例 shape | `ndim` |
|---|---:|---:|
| 标量 | `[]` | 0 |
| 向量 | `[2]` | 1 |
| 二维数据 | `[B, 2]` | 2 |
| 序列/批量嵌入 | `[B, L, D]` | 3 |

- `ndim`：Tensor 有多少个维度。
- `shape`：每个维度包含多少个元素。
- `numel()`：Tensor 中元素总数。
- `dtype`：元素类型，如 `float32`、`long`。
- `device`：所在设备，如 `cpu`、`cuda`。
- `requires_grad`：是否记录梯度，模型参数通常为 `True`。

推荐系统中常见的形状变化：

```text
单条特征       [2]
一个 Batch     [B, 2]
模型输出       [B, 1]
Embedding ID  [B]
Embedding 输出 [B, embedding_dim]
```

### 3.3 dtype 与 device

- 连续特征、模型权重和大多数 Loss 计算通常使用 `torch.float32`。
- 分类标签的类型取决于 Loss；二分类标签通常是 0/1 浮点值。
- Embedding 的索引通常使用 `torch.long`。
- 参与同一次计算的 Tensor 通常需要位于同一设备。

## 4. 机器学习任务与推荐系统

机器学习的核心是：给定数据和目标，学习一个模型，使其在指定指标上表现良好。

| 任务 | 预测内容 | 推荐系统例子 |
|---|---|---|
| 回归 | 连续数值 | 预测用户评分 |
| 分类 | 离散类别 | 预测用户是否点击 |
| 排序 | 候选项顺序 | 对候选商品排序 |
| 序列预测 | 下一个事件 | 预测用户下一次交互 |
| 生成 | 新内容 | 生成或改写推荐理由 |

推荐系统实验首先要明确：输入是什么、标签是什么、模型要预测什么、用什么指标判断好坏。

## 5. 模型与训练概念

### 5.1 参数、超参数与激活函数

- **参数**：训练过程中由数据学习得到，如权重、偏置和 Embedding。
- **超参数**：训练前设定，如学习率、Batch Size、Epoch 数和 Embedding 维度。
- **激活函数**：为模型引入非线性，使多层网络能够表达更复杂的关系。ReLU 常用于隐藏层，Sigmoid 可将 Logit 映射为概率。

### 5.2 常见模型演进

| 模型 | 主要能力 | 推荐系统中的联系 |
|---|---|---|
| 线性/逻辑回归 | 学习特征的加权组合 | 点击率、转化率等基础预测 |
| 决策树 | 通过规则划分数据 | 用户/商品分群与可解释分析 |
| MLP | 通过非线性层学习特征组合 | 融合用户、商品及上下文特征 |
| CNN | 捕捉局部模式 | 用户行为中的局部兴趣模式 |
| RNN/LSTM | 维护顺序状态 | 建模兴趣随时间的变化 |
| Transformer | 通过 Attention 建模位置间关系 | 序列行为表示和下一物品预测 |

模型选择应由数据结构、任务目标、计算资源和实验结果共同决定，而不是只看模型复杂度。

## 6. 数据划分与指标

### 6.1 Train / Validation / Test

- **训练集**：用于学习模型参数。
- **验证集**：用于选择模型、超参数和训练轮数。
- **测试集**：最后只使用一次，用于估计泛化性能。

测试集信息不能反过来参与训练或调参，否则会产生数据泄漏。

### 6.2 Loss 与评价指标

- **Loss**：训练阶段用于产生优化信号，例如二分类交叉熵。
- **评价指标**：从任务角度评估效果，例如 Accuracy、Precision、Recall、AUC。
- **泛化能力**：模型在未见过的数据上保持效果的能力。

如果训练 Loss 持续下降而验证 Loss 开始上升，通常说明模型可能过拟合。此时应结合验证指标、模型复杂度、正则化和训练轮数进一步判断。

## 7. PyTorch 二分类实验闭环

### 7.1 数据组织

`Dataset` 负责提供单条样本，`DataLoader` 负责把样本组织成可迭代的 Batch：

```python
train_set = TensorDataset(train_x, train_y)
train_loader = DataLoader(
    train_set,
    batch_size=16,
    shuffle=True,
)
```

重要参数：

- `batch_size`：每个 Batch 的样本数。
- `shuffle=True`：每个 Epoch 开始前打乱训练样本顺序。
- `drop_last=True`：丢弃不足一个完整 Batch 的尾部数据。
- `num_workers`：并行读取数据的进程数。

DataLoader 只负责组织和提供数据，不负责计算 Logit、Loss 或更新模型参数。

### 7.2 模型、Logit 与 Loss

一个最小二分类模型可以把 `[B, 2]` 的输入映射为 `[B, 1]` 的输出：

```python
model = nn.Linear(2, 1)
logits = model(batch_features)
loss_fn = nn.BCEWithLogitsLoss()
loss = loss_fn(logits.squeeze(-1), batch_labels.float())
```

`BCEWithLogitsLoss` 已经把 Sigmoid 和二分类交叉熵结合起来，训练时通常直接传入 Logit，不需要先手动做 Sigmoid。

### 7.3 一次参数更新

```python
optimizer.zero_grad()
logits = model(batch_features)
loss = loss_fn(logits.squeeze(-1), batch_labels.float())
loss.backward()
optimizer.step()
```

执行顺序：

1. 清空上一次迭代留下的梯度。
2. 前向传播，得到预测 Logit。
3. 根据标签计算 Loss。
4. `backward()` 计算参数梯度。
5. `step()` 根据梯度更新参数。

### 7.4 Batch、Epoch 与训练循环

- 一个 **Batch** 是一次参数更新处理的数据。
- 一个 **Epoch** 是完整遍历一次训练集。
- 如果一个 Epoch 有 10 个 Batch，通常会执行 10 次前向、反向和优化器更新。

训练模式和评估模式要区分：

```python
model.train()
# 训练循环

model.eval()
with torch.no_grad():
    # 验证或测试
    pass
```

验证/测试阶段不应更新参数，并应关闭梯度计算，以减少内存和计算开销。

## 8. 实验记录与排错清单

每次实验至少记录：

- 数据集划分方式；
- 特征和标签的含义；
- 输入、标签和输出的 shape；
- dtype 与 device；
- 模型结构和参数量；
- 学习率、Batch Size、Epoch 数；
- 训练 Loss、验证 Loss 和主要评价指标；
- 最佳验证结果对应的模型状态。

出现错误时，优先检查：

1. `shape` 是否与模型层的输入输出要求一致；
2. 标签是否为正确的 dtype 和取值范围；
3. 输入与模型是否在同一 device；
4. Logit、概率和标签是否被混用；
5. 训练、验证、测试数据是否发生泄漏；
6. 是否忘记 `zero_grad()`、`backward()` 或 `step()`；
7. 验证/测试时是否调用了 `eval()` 和 `torch.no_grad()`。

## 9. 课后行动

- 独立运行 `lesson02_slides.ipynb`，不要只看输出。
- 练习用 `list`、`tuple`、`set`、`dict` 表示样本、标签和用户/商品集合。
- 看到任何 Tensor 先检查 `shape`、`dtype` 和 `device`。
- 手写一次最小训练循环，并解释每一行的作用。
- 将实验结果和自己的理解记录到 GitHub 仓库中。

## 10. 配套材料

- 课程文档：`暑期科研培训：第二次课程.docx`
- 课件 Notebook：`lesson02_slides.ipynb`
- 配套图示：`assets/`
- 配套数据：`data/interactions.csv`

