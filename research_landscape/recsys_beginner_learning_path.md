# 推荐系统零基础科研学习路径（24 周）

## 总目标

24 周后达到以下最低科研能力：

1. 能读懂推荐模型的数据流、Loss 和评测代码；
2. 能用统一协议复现 LightGCN 与 SASRec；
3. 能读懂一篇课题组论文并复现核心结果；
4. 能提出一个带强基线、消融和可信评测的初步问题。

## 阶段 1：Python 与数据处理（第 1—4 周）

使用本地 `Python+AI从入门到大神一套通关`：

- 第 1 章全部；
- 第 2 章全部；
- 第 5 章的 Jupyter、Pandas、数据读取、清洗和分组；
- 暂时跳过 AI 应用、爬虫与 Web。

必须掌握：变量、列表/字典、条件循环、函数、类、模块、NumPy array、Pandas DataFrame、CSV。

阶段项目：读取 `interactions.csv`，统计用户数、物品数、交互数、稀疏度和每个用户的行为序列。

通过标准：不给参考代码，也能完成 CSV 读取、筛选、分组和简单绘图。

## 阶段 2：数学、机器学习与 PyTorch（第 5—8 周）

只补推荐系统必要数学：

- 向量、矩阵、点积、矩阵乘法；
- 均值、方差、概率与条件概率；
- 导数、梯度和链式法则的直觉；
- 交叉熵、Sigmoid、Softmax；
- 训练/验证/测试、过拟合与正则化。

PyTorch 必学：Tensor、shape、dtype、Dataset、DataLoader、`nn.Module`、Embedding、Loss、Optimizer、`train/eval`、GPU。

阶段项目：手写一个二分类训练循环，再实现一个用户—物品 Embedding 点积模型。

通过标准：能解释 `zero_grad → forward → loss → backward → step`，能定位 shape/device 错误。

## 阶段 3：推荐系统基础（第 9—12 周）

知识顺序：

```text
用户/物品/交互
→ 显式与隐式反馈
→ Top-K 推荐
→ UserCF / ItemCF
→ Matrix Factorization
→ BPR
→ Recall@K / NDCG@K / AUC
→ 数据切分与负采样
```

阶段项目：在 MovieLens-1M 上实现 Popular、ItemCF、MF/BPR 三类 baseline，保持同一数据切分和评测协议。

通过标准：能解释为什么不能直接比较使用不同切分、负采样和候选集的论文结果。

## 阶段 4：图推荐与序列推荐（第 13—16 周）

主线论文：NGCF → LightGCN；GRU4Rec（了解）→ SASRec → BERT4Rec。

实践顺序：

1. 使用 RecBole 跑通 BPR、LightGCN、SASRec；
2. 固定数据与评测协议比较三者；
3. 修改一个超参数并记录均值、方差和运行时间；
4. 阅读模型源码，画出 Tensor shape 流程。

通过标准：独立复现 LightGCN 和 SASRec，并能说明它们分别利用了什么结构信息。

## 阶段 5：进入课题组主线（第 17—20 周）

从以下四条路线中选一条，不要同时全做：

### A. 多行为/多兴趣

前置：LightGCN、SASRec、多任务学习、对比学习。

阅读：Multi-Behavior Recommender Systems Survey → PPN-ARE。

最小研究问题：辅助行为在什么条件下帮助或伤害目标行为？

### B. 序列/Session/POI

前置：SASRec、BERT4Rec、图学习、时间切分。

阅读：Sequential Recommendation Survey → STIRec / LLM4SBR / CAMSHG。

最小研究问题：长短期意图、空间约束或语义信息是否在不同稀疏度下稳定有效？

### C. 因果与可信推荐

前置：LightGCN、概率、曝光偏差、因果图基础。

阅读：Bias and Debias Survey → Causal Recommendation Survey → ESCI。

最小研究问题：模型改善的是偏差、准确率，还是评测协议造成的表面变化？

### D. 多模态与 LLM 增强

前置：LightGCN/SASRec、文本与图像 Embedding、对比学习。

阅读：Multimodal Survey → LLM/FM Survey → LLM4SBR / MSCF-Net。

最小研究问题：语义增强在哪些用户/物品冷启动条件下有效，代价和幻觉风险如何？

通过标准：完成一篇课题组论文的“问题—方法—实验—局限”四页报告，并跑通其代码或最接近的开源 baseline。

## 阶段 6：第一次科研闭环（第 21—24 周）

选择一个小而可证伪的问题：

1. 写出假设和失败条件；
2. 确定 2 个简单强基线、1 个近期强基线；
3. 固定数据切分、随机种子和超参数预算；
4. 至少运行 3 次并报告均值/方差；
5. 做消融、效率和分用户/分物品分析；
6. 检查是否存在泄漏、曝光偏差或不公平比较；
7. 写 6—8 页技术报告并准备 10 分钟汇报。

## 每周固定节奏

| 时间 | 内容 | 产出 |
|---|---|---|
| 周一—周二 | 视频/教材与基础代码 | 概念笔记、可运行示例 |
| 周三—周四 | 论文与源码 | 一页论文卡、模块图 |
| 周五—周六 | 实验 | 配置、日志、结果表 |
| 周日 | 复盘 | 本周结论、问题、下周计划 |

## 新手必须避免的坑

- 一开始就做 LLM Agent，而没有 LightGCN/SASRec 基线；
- 只看论文摘要，不运行代码；
- 只追求更高 Recall，不检查切分和负采样；
- baseline 没有公平调参；
- 单次运行就声称显著提升；
- 把 Attention 权重直接当作因果解释；
- 在测试集上反复选模型；
- 只堆模块，不写清楚假设和失败条件。

