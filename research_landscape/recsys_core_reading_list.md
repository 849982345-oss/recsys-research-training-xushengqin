# 推荐系统核心论文阅读清单

## A 组：入门必读与必复现

| 顺序 | 论文 | 为什么读 | 要做到什么 |
|---:|---|---|---|
| 1 | Koren et al., *Matrix Factorization Techniques for Recommender Systems* (2009) | 建立用户/物品潜在向量直觉 | 手写 MF 预测公式 |
| 2 | Rendle et al., *BPR* (2009) | 理解隐式反馈和成对排序 | 写出正负样本与 BPR Loss |
| 3 | He et al., *NCF* (2017) | 理解 MLP 交互建模 | 比较点积与 MLP |
| 4 | Rendle et al., *NCF vs. MF Revisited* (2020) | 建立强基线意识 | 解释为什么复杂不等于更好 |
| 5 | Wang et al., *NGCF* (2019) | 理解用户—物品图传播 | 画出一层消息传递 |
| 6 | He et al., *LightGCN* (2020) | 图推荐最重要基线之一 | 用 RecBole 复现 |
| 7 | Kang & McAuley, *SASRec* (2018) | 序列推荐核心基线 | 画 causal attention 数据流 |
| 8 | Sun et al., *BERT4Rec* (2019) | 理解 masked 序列目标 | 比较 SASRec 与 BERT4Rec |
| 9 | Ferrari Dacrema et al., *Are We Really Making Much Progress?* (2019) | 学会审查实验可信度 | 写一页评测反思 |
| 10 | Zhao et al., *RecBole* (2021) | 建立统一实验管线 | 跑通 BPR/LightGCN/SASRec |

## B 组：课题组方向核心

| 方向 | 先读综述 | 再读课题组论文 |
|---|---|---|
| 多行为 | Kim et al., *Multi-Behavior Recommender Systems: A Survey* | Liu et al., *PPN-ARE* |
| 序列 | Pan et al., *A Survey on Sequential Recommendation* | Shen et al., *STIRec* |
| Session + LLM | Foundation Model / LLM 推荐综述 | Qiao et al., *LLM4SBR* |
| POI | 序列与图推荐基础 | Zhou et al., *CAMSHG* |
| 因果去偏 | Gao et al., *Causal Inference in Recommender Systems* | Liao et al., *ESCI* |
| 多模态 | Xu et al., *A Survey on Multimodal Recommender Systems* | Li et al., *MSCF-Net* |

## C 组：2025—2026 前沿

| 论文 | 研究信号 | 阅读时重点质疑 |
|---|---|---|
| Huang et al., *Foundation Model-Powered Recommender Systems* (2025) | Feature/Generative/Agentic 三范式 | 分类是否掩盖评测差异？ |
| Rajput et al., *TIGER* (2023) | Semantic ID 与生成式召回 | 推理成本、碰撞、目录扩展 |
| Peng et al., *LLM-powered Agents for Recommender Systems* (2025) | 推荐、交互、模拟三类 Agent | 模拟用户能否代表真人？ |
| De Toni et al., *You Don't Bring Me Flowers* (2025) | 从平均准确率转向风险界限 | 风险定义与反馈成本 |
| Kim et al., *Multi-Behavior Recommender Systems* (2026) | 多行为统一流程 | 不同评测协议是否可比？ |

## 每篇论文的一页卡片模板

```text
论文：
问题：
为什么旧方法不够：
核心假设：
输入/输出：
模型数据流：
Loss：
数据集与切分：
Baseline：
主要指标：
最有说服力的证据：
最薄弱的证据：
代码与复现状态：
与课题组方向的关系：
我能提出的一个可证伪问题：
```

