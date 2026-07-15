# 推荐系统

## 基本框架

- 召回：
- 排序：
- 重排：

## 经典方法

- 协同过滤：
- 矩阵分解：
- Learning to Rank：

## 深度推荐

- Embedding：
- Wide & Deep：
- DIN / DIEN：
- 序列推荐：
- SASRec：使用因果 Self-Attention 根据用户历史行为预测下一个物品。
- BERT4Rec：使用双向 Transformer 和遮盖目标建模序列。

## SASRec 最小数据流

```text
用户历史物品序列
→ 物品 Embedding + 位置 Embedding
→ 因果 Self-Attention
→ 序列表示
→ 候选物品打分
→ 正负样本损失
```

序列过长时从左侧截断，不足最大长度时补 padding。因果 mask 保证预测当前位置时不能看到未来行为。

## 数据集

- MovieLens：
- Amazon Reviews：

## 评估指标

- Precision / Recall：
- NDCG：
- AUC：
- Hit Rate：
- HR@K：真实目标物品是否进入前 K 名。
- NDCG@K：真实目标物品进入前 K 名后，根据排名位置给分。

## 序列推荐数据切分

推荐使用时间顺序切分：

```text
过去行为 → 训练集
较近行为 → 验证集
最后行为 → 测试集
```

随机切分可能把未来行为泄漏到训练集，因此不适合直接模拟“根据过去预测未来”的场景。

## Smoke Test

Smoke Test 是小规模流程验证，不是正式效果实验。它主要检查：数据、模型、训练、评测、日志和硬件环境能否完整运行。合成 Smoke 数据上的高指标不能直接代表真实推荐系统效果。
