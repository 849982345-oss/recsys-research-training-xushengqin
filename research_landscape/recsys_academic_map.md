# 推荐系统学术地图（2026）

## 1. 全领域地图

```mermaid
flowchart LR
    A["推荐系统问题"] --> B["数据与反馈"]
    A --> C["表示与模型"]
    A --> D["任务与场景"]
    A --> E["约束与评测"]

    B --> B1["显式反馈：评分"]
    B --> B2["隐式反馈：点击/购买"]
    B --> B3["多行为反馈"]
    B --> B4["序列与 Session"]
    B --> B5["文本/图像/音频"]
    B --> B6["曝光/策略/因果变量"]

    C --> C1["邻域 CF / MF / BPR"]
    C --> C2["MLP / CTR 模型"]
    C --> C3["GNN / Hypergraph"]
    C --> C4["RNN / Transformer"]
    C --> C5["SSL / Contrastive"]
    C --> C6["Foundation Model / LLM"]
    C --> C7["Generative Retrieval / Agent"]

    D --> D1["召回与 Top-K"]
    D --> D2["排序与重排"]
    D --> D3["Next-item / Session"]
    D --> D4["Next-POI"]
    D --> D5["多模态/跨域/冷启动"]
    D --> D6["会话式推荐"]

    E --> E1["准确率：Recall/NDCG/AUC"]
    E --> E2["多样性/新颖性/覆盖"]
    E --> E3["公平/隐私/鲁棒/解释"]
    E --> E4["因果去偏/离线-在线一致"]
    E --> E5["效率/成本/可复现性"]
    E --> E6["风险/安全/多利益相关者"]
```

## 2. 技术演进主线

```mermaid
timeline
    title 推荐系统研究主线
    1990s : 邻域协同过滤 : 内容推荐
    2009 : Matrix Factorization : BPR 隐式排序
    2016-2018 : Wide & Deep / NCF : GRU4Rec / SASRec
    2019-2021 : NGCF / LightGCN : BERT4Rec : 自监督与对比学习
    2022-2024 : 多行为/多兴趣 : 因果与可信 : 多模态 : P5 / TIGER
    2025-2026 : Foundation Model : 生成式推荐 : Agentic Recommendation : 系统风险与可验证评测
```

## 3. 课题组方向定位

```mermaid
flowchart TB
    G["文俊浩—周魏课题组推荐系统研究"]

    G --> M["多行为与多兴趣"]
    G --> S["序列 / Session"]
    G --> P["POI 与时空推荐"]
    G --> V["多模态与多视图"]
    G --> T["因果与可信推荐"]
    G --> L["LLM / 多智能体推荐"]

    M --> M1["行为级联与依赖"]
    M --> M2["正负/长短期兴趣"]
    M --> M3["解耦、对比、多任务"]

    S --> S1["时空意图建模"]
    S --> S2["LLM 语义意图增强"]
    S --> S3["长短期偏好演化"]

    P --> P1["地理—时间耦合"]
    P --> P2["图/超图高阶关系"]
    P --> P3["稀疏、曝光与隐私"]

    V --> V1["文本/图像特征"]
    V --> V2["模态噪声与对齐"]
    V --> V3["频域与多尺度融合"]

    T --> T1["曝光与流行度偏差"]
    T --> T2["满意度与因果图"]
    T --> T3["攻击、异常、风险控制"]

    L --> L1["大模型语义增强"]
    L --> L2["大小模型协同"]
    L --> L3["多智能体可信服务推荐"]
```

## 4. 新手选方向决策树

```mermaid
flowchart TD
    A["已能复现 LightGCN 和 SASRec？"] -->|否| B["先补 PyTorch、Top-K 评测和 RecBole"]
    A -->|是| C{"更喜欢哪类问题？"}
    C -->|用户行为结构| D["多行为 / 多兴趣 / 序列"]
    C -->|地理与场景| E["POI / 时空推荐"]
    C -->|文本图像内容| F["多模态 / LLM 增强"]
    C -->|为什么会偏/不可信| G["因果去偏 / 公平 / 鲁棒"]
    C -->|交互和系统智能| H["会话式 / 生成式 / Agent"]

    D --> I["首个复现：SASRec + 多行为模型"]
    E --> J["首个复现：SASRec + POI 基线"]
    F --> K["首个复现：LightGCN + 内容特征"]
    G --> L["首个复现：LightGCN + 曝光偏差基线"]
    H --> M["先做 LLM 增强传统模型，再做 Agent"]
```

## 5. 论文阅读依赖图

```mermaid
flowchart LR
    MF["MF"] --> BPR["BPR"]
    BPR --> NCF["NCF"]
    BPR --> LGCN["LightGCN"]
    LGCN --> SSL["图自监督/对比学习"]
    BPR --> SAS["SASRec"]
    SAS --> BERT["BERT4Rec"]
    SAS --> MBR["多行为序列"]
    SAS --> POI["时空/POI"]
    LGCN --> MBR
    LGCN --> MM["多模态推荐"]
    MBR --> PPN["PPN-ARE"]
    SAS --> STI["STIRec"]
    SAS --> L4S["LLM4SBR"]
    MM --> MSCF["MSCF-Net"]
    POI --> CAM["CAMSHG"]
    LGCN --> CAUSAL["因果去偏"]
    CAUSAL --> ESCI["ESCI"]
    BERT --> GEN["P5 / TIGER"]
    GEN --> AGENT["LLM Agent Recommendation"]
```

## 6. 地图的核心判断

- 最底层共同基础是隐式反馈、Embedding、排序损失和 Top-K 评测。
- 课题组当前最密集的区域是多行为/多兴趣、序列/POI、多视图融合和可信推荐。
- LLM 并不是一条独立替代路线，它与语义增强、生成式检索、会话交互和系统模拟四类任务交叉。
- 因果、可信和可复现评测应作为每个分支的横向约束，而不是最后附加的一节。

