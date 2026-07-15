# 推荐系统研究全景：从表示学习竞赛走向可信、生成式与可验证系统

## 摘要

本文面向推荐系统零基础科研新生，系统梳理从协同过滤、矩阵分解、神经协同过滤、图推荐和序列推荐，到多行为、多模态、因果去偏、可信推荐、Foundation Model、生成式推荐与 Agentic Recommendation 的研究演进。检索覆盖经典奠基工作、2024—2026 年综述与代表论文，并结合重庆大学大数据与软件学院文俊浩、周魏课题组公开成果进行方向校准。综合证据表明，推荐系统的主线并非简单地由“更深模型”替代“浅层模型”，而是从单一交互上的相关性拟合，转向对多源行为、时空意图和内容语义的联合建模；与此同时，离线指标、数据偏差、可复现性和真实用户价值已经成为与模型结构同等重要的研究对象。LLM 与生成式模型扩展了语义理解、统一任务和交互能力，但现有证据尚不足以说明它们在传统 Top-K 准确率、推理成本和稳定性上全面取代专用推荐模型。对本课题组的新生而言，最有连续性的进入路径是：先掌握 LightGCN、SASRec 和标准评测，再进入多行为/多兴趣、序列/POI、多模态及因果可信方向，最后探索 LLM 增强和多智能体可信推荐。

## 1. 引言

推荐系统研究的对象不是一个孤立模型，而是一个由数据记录、候选召回、排序、重排、交互反馈和评测构成的闭环。早期研究主要回答“如何从用户—物品交互中预测偏好”，近年的问题则变成“如何在多行为、动态环境和多利益相关者约束下，提供准确、可靠、可解释且可持续的推荐”。这种变化意味着，只会复现一个更复杂的网络结构已不足以形成有说服力的研究。

本文回答三个研究问题：

- **RQ1：** 推荐系统从经典协同过滤到生成式/智能体范式的稳定知识骨架是什么？
- **RQ2：** 截至 2026 年，哪些方向已形成较可靠的方法共识，哪些仍受评测、偏差、成本或可复现性限制？
- **RQ3：** 文俊浩、周魏课题组的公开研究重点位于学术地图何处，零基础新生应怎样进入？

课题组定位来自学院公开页面：文俊浩教授的研究领域包括机器学习与推荐系统、服务计算和软件服务工程；周魏教授的公开方向包括推荐系统、用户行为分析、异常检测、机器学习与数据挖掘，并明确列出多行为、序列、POI、因果去偏、LLM 增强和可信服务推荐相关项目与论文。因而本文不是平均分配篇幅，而是对这些相邻分支给予更高权重。

## 2. 方法

检索截至 2026 年 7 月 15 日，采用五个视角：

1. 主流模型演进：CF、MF、神经、图、序列与多模态；
2. 课题组相关：多行为、多兴趣、Session、POI、因果去偏、可信推荐；
3. 新范式：Foundation Model、LLM、生成式检索和智能体；
4. 批判视角：可复现性、基线强度、离线—在线偏差与评价协议；
5. 应用与社会价值：公平、隐私、鲁棒性、可控性和多利益相关者目标。

优先纳入 RecSys、SIGIR、KDD、WWW、WSDM、CIKM、NeurIPS、ACL/EMNLP 以及 TOIS、TKDE、TSC 等同行评审成果；2025—2026 年尚未完成同行评审但直接代表新范式的综述以 arXiv 预印本身份使用。具体方法结论只在论文标题和摘要能够支持时写入，不从二手博客推断实验细节。

本文使用二维分类轴：第一轴是**系统要学习什么信号**，第二轴是**系统必须满足什么约束**。前者包括协同、序列、多行为、多模态和语言/生成信号；后者包括准确性、因果有效性、可信性、效率与可验证性。这样可避免把论文仅按模型名串联，也能解释跨分支工作为何越来越多。

## 3. 学术分类体系

| 主分支 | 核心输入 | 典型任务 | 关键模型/范式 | 当前主要瓶颈 |
|---|---|---|---|---|
| 经典与深度 CF | 用户—物品交互 | Top-K、评分预测 | MF、BPR、NCF | 稀疏、冷启动、负样本与评测协议 |
| 图推荐与自监督 | 二部图、高阶关系 | Top-K、图增强推荐 | NGCF、LightGCN、SSL/CL | 增益来源解释、噪声传播、复杂度 |
| 序列与 Session | 有序行为序列 | Next-item、Session 推荐 | GRU4Rec、SASRec、BERT4Rec | 长短期兴趣、时间漂移、泄漏与切分 |
| 多行为/多兴趣 | 点击、收藏、购买等 | 目标行为预测 | 级联/并行、多任务、兴趣解耦 | 行为依赖、负迁移、辅助行为质量 |
| POI 与上下文 | 时空轨迹、地理与社交 | Next-POI | 时空模型、图/超图、多视图 | 极端稀疏、时空耦合、隐私 |
| 多模态与内容增强 | 文本、图像、音频、知识 | 冷启动、内容推荐 | 模态图、对比学习、VLM | 模态噪声、对齐、缺失模态、成本 |
| 因果与可信推荐 | 观测日志、曝光/策略信息 | 去偏、公平、鲁棒、解释 | IPS、SCM、反事实、联邦/隐私 | 因果假设不可检验、缺少无偏数据 |
| Foundation/生成式/Agent | 文本、语义 ID、工具与记忆 | 统一推荐、对话、生成、模拟 | P5、TIGER、LLM agent | 准确率、幻觉、时延、闭环评测与安全 |

该分类中的空缺很重要：例如，“多行为 + 因果可信 + Agent”仍缺乏统一公开基准；“多模态 + 真实在线长期效用”也远少于离线准确率实验。这些空缺比再叠加一个模块更可能形成有价值的研究问题。

## 4. 从矩阵分解到图与序列：稳定的技术骨架

矩阵分解把用户和物品映射到共同潜在空间，是现代 Embedding 推荐的基础；BPR 则把隐式反馈任务直接表述为成对排序优化 [1,2]。NCF 用 MLP 替代内积以学习非线性交互 [3]，但后续复核表明，在充分调参时，简单点积模型可优于 NCF 的学习相似度，且更利于大规模检索 [4]。这组相互矛盾的结果形成了一个重要科研原则：**模型表达能力不是性能证据，强基线与公平调参才是。**

图推荐把用户—物品交互显式建模为二部图。NGCF 强调高阶连接传播 [5]，而 LightGCN 通过消融发现特征变换与非线性对协同过滤贡献有限，只保留邻域聚合后反而更简洁有效 [6]。这与 NCF 的复核共同说明，推荐系统经常从“移除通用深度学习组件”而不是“增加组件”中获益。自监督推荐随后通过数据增强、对比任务和生成任务缓解稀疏监督，但覆盖 170 余篇工作的综述也显示，视图生成是否保持推荐语义、负样本是否引入伪监督，仍是效果解释的核心 [7]。

序列推荐进一步把静态偏好改写为“给定历史预测下一行为”。SASRec 用因果 Self-Attention 在长程依赖与稀疏条件之间折中 [8]；BERT4Rec 使用双向 Transformer 和 Cloze 目标增强序列表征 [9]。二者的差异不仅是架构，还包括训练目标和信息可见范围；后续工作显示，损失函数、实现和数据处理就可能改变模型排序 [10]。因此，新生复现序列推荐时应首先固定时间切分、负采样、最大序列长度和评测候选集，而不是先关注注意力层数。

## 5. 课题组主轴：多行为、多兴趣、序列与 POI

多行为推荐利用浏览、点击、收藏、加购、购买等异质反馈缓解目标行为稀疏。最新综述把方法拆分为数据建模、编码和训练三个阶段 [11]，而多行为序列综述进一步强调行为异质性与时间顺序的联合建模 [12]。相比单行为模型，多行为方法的真正难点不是“数据更多”，而是辅助行为可能与目标行为存在级联关系、噪声和负迁移。

课题组的 PPN-ARE 工作构造多层正负兴趣视图链，并通过表示增强过滤低质量辅助行为，体现了“行为依赖 + 兴趣层级 + 多任务优化”的路线 [13]；与只做统一行为融合的方法相比，它把辅助行为质量和正负偏好纳入模型。2025 年课题组还公开了趋势与多样性解耦、多簇异构图对比学习等工作，说明研究重点正从简单融合转向**兴趣解耦、行为关系和表示质量控制**。

序列与 Session 方向中，STIRec 同时建模空间意图图与多跨度时间依赖 [14]，LLM4SBR 则用 LLM 从多视图推断 Session 语义意图，再与传统行为表示对齐，并通过意图定位抑制幻觉 [15]。二者共同指向一个清晰问题：用户下一次交互既取决于行为结构，也取决于行为背后的潜在意图；但后者无法被直接观测，因此模型提升是否来自真实意图还是额外语义特征，需要更严谨的可解释性和反事实验证。

POI 推荐把序列问题置于时空约束下。课题组 CAMSHG 通过多语义超图、通道注意力、对比对齐和多尺度融合建模高阶时空关系 [16]。与一般序列推荐相比，POI 还受到距离、时间周期、生活圈和地理曝光影响；因此它天然连接序列推荐、图学习、因果曝光和隐私保护，是课题组内较有延展性的应用方向。

## 6. 多模态、因果去偏与可信推荐

多模态推荐把文本、图像、音频等内容信号与协同行为融合，常见设计可分为特征提取、编码、融合和损失四个阶段 [17]。其优势主要出现在内容理解、冷启动和长尾物品，但模态特征并非天然可靠：背景、冗余描述和预训练偏差会引入噪声。课题组 MSCF-Net 在频域抑制高频模态噪声，并通过用户—用户和物品—物品共频结构增强协同信号 [18]；这与“简单拼接更多模态即可提升”的假设形成对照。

可信推荐不是一个单独模型，而是公平、可解释、隐私、鲁棒、可控等目标的集合 [19]。传统日志来自已部署系统，曝光、位置、流行度和选择偏差会共同影响观测交互；直接拟合日志可能放大既有策略。因果推荐综述把方法归纳为去偏、解释和泛化，并提醒所有因果方法都依赖识别假设 [20,21]。课题组 ESCI 同时建模物品曝光和用户满意度，并在推断时移除曝光影响 [22]，与仅根据流行度重加权的方法相比，它更明确地描述了交互生成机制。

但因果图不是事实本身。缺少随机实验或无偏日志时，不同图结构都可能解释同一观测分布。因而可信推荐的高质量工作应报告：因果假设、可观测变量、干预目标、敏感性分析以及与普通相关性基线的公平比较。RecSys 2025 最佳论文用 Conformal Risk Control 对“不想要的推荐”给出分布无关风险界限 [23]，代表可信推荐从平均准确率转向可控风险的趋势。

## 7. Foundation Model、生成式与智能体推荐

Foundation Model 与推荐系统的结合可以分为三种范式：Feature-based 增强、Generative Recommendation 和 Agentic Recommendation [24]。P5 把多种推荐任务统一为 text-to-text 问题 [25]；TIGER 用量化后的 Semantic ID 自回归生成目标物品，改变了“用户向量检索物品向量”的传统召回形式 [26]；TALLRec 则用参数高效微调把通用 LLM 对齐到推荐任务 [27]。

现有证据并不支持“LLM 已全面替代专用推荐模型”。LLMRec 的多任务基准显示，LLM 在解释和文本生成任务上有优势，但在序列与直接推荐等准确率任务上仅表现中等 [28]。课题组 LLM4SBR 选择“两阶段增强”而非端到端替代：LLM 提供语义意图，传统推荐器负责稳定的候选评分 [15]。这一设计与 Foundation Model 综述中的 Feature-based 路线一致，也更符合当前算力和时延约束。

智能体推荐进一步增加 Profile、Memory、Planning 和 Action，并形成三类用途：增强推荐、自然语言交互和用户/系统模拟 [29]。课题组“LLM 多智能体增强的可信服务推荐”项目正位于这一前沿交叉点。但 Agent 的开放问题比普通模型更多：多个代理之间会传播错误，模拟用户不能代替真人，工具调用引入安全边界，且离线自动评委可能继承 LLM 偏差。2026 RecSys 教程已将多智能体推荐的组合风险和评测列为核心议题，说明该方向尚处于“系统定义与风险框架先行”的阶段。

## 8. 评测与可复现性：贯穿所有分支的硬约束

推荐系统论文的最大共同风险不是模型不够复杂，而是评测不可比。Dacrema 等复现 18 个神经推荐方法时，仅 7 个能在合理成本下复现；其中多数没有稳定超过简单强基线 [30]。DaisyRec 进一步梳理 141 篇论文，指出数据过滤、切分、负采样、候选集、超参数与指标实现等“超因素”都会显著改变结论 [31]。这两项工作与 NCF 复核 [4] 共同否定了“论文表格中的小幅提升可以直接横向比较”的朴素理解。

RecBole 和 Elliot 通过统一数据、模型、超参数和指标管线改善可复现性 [32,33]，但框架不能自动保证研究有效。研究者仍需明确：

- 是全排序还是采样排序；
- 是随机切分还是时间切分；
- 验证集是否参与调参；
- 每个 baseline 是否得到同等搜索预算；
- 是否报告均值、方差和显著性；
- 指标提升是否伴随覆盖、多样性、公平、成本或风险退化。

截至 2025 年，RecSys 会议仍把离线、在线与真实世界评测单列为专题，并出现对候选召回的 Off-Policy Evaluation、采样可靠性和反事实解释指标一致性的研究。这表明评测不是已经解决的“实验细节”，而是可以独立形成论文贡献的核心方向。

## 9. 跨分支综合判断与开放问题

第一，**推荐系统的稳健进步来自问题建模、数据和评测共同改进，而不是网络深度本身。** LightGCN、NCF 复核和可复现性研究从不同角度支持这一判断 [4,6,30,31]。

第二，**课题组的连续主线是“异质行为—动态意图—多视图表示—可信约束”。** 多行为、Session/序列、POI、多模态和因果去偏并非互不相关：它们都在处理观测信号不完整、异质、有噪声且受环境影响的问题。LLM 的合理角色目前更像语义增强器和交互器，而非默认替代整个推荐管线。

第三，**2026 年最值得关注的前沿不是单纯 LLM 化，而是可验证的生成式/智能体推荐。** 生成式模型需要与候选约束、事实性、长尾覆盖、推理成本和真实用户效用联合评测；Agent 还需处理记忆污染、工具安全、多代理错误放大和模拟有效性。

基于检索结果，以下问题具有较好的课题组契合度：

1. 多行为辅助信号何时产生负迁移，能否用因果图或不确定性估计选择辅助行为？
2. 多行为序列中的“意图”如何通过可干预或可证伪的实验验证，而不只依靠注意力可视化？
3. POI 推荐如何联合建模时空曝光、用户满意度和隐私风险？
4. 多模态推荐如何区分有用语义与模态噪声，并处理缺失/冲突模态？
5. LLM 语义增强是否在冷启动、长尾和跨域上稳定有效，其收益是否抵得过成本与幻觉风险？
6. LLM 多智能体推荐如何建立从组件到系统的风险传播与评测协议？
7. 能否为多行为、POI 或 Agent 推荐建立同时覆盖准确率、校准、风险、成本和复现性的 benchmark？

## 10. 对研究问题的回答

**RQ1：** 稳定骨架是“隐式反馈排序—Embedding 表示—图/序列结构—多源信号融合—偏差与评测闭环”。MF、BPR、LightGCN 和 SASRec 仍是理解现代模型的关键，不会因 LLM 出现而失效。

**RQ2：** 协同过滤、图传播、序列建模和标准 Top-K 评测已有成熟基础，但多行为关系、因果识别、多模态噪声、离线—在线一致性以及 Agent 系统评测仍未形成统一结论。LLM 在语义和交互方面前景明确，在准确率、成本和可靠性方面则需要条件化判断。

**RQ3：** 课题组位于多行为/多兴趣、序列/Session/POI、多模态、因果可信和 LLM 增强推荐的交叉区域。零基础新生最稳妥的入口是先复现 LightGCN 和 SASRec，再选择一个课题组模型做对照复现；直接从多智能体或复杂生成式模型开始，会因缺乏基线与评测能力而难以判断结果。

## 局限

本文以公开英文论文和课题组网页为主，无法覆盖未公开项目、内部数据和导师当前尚未发表的选题。2026 年 RecSys 主会尚未召开，因此“最新”主要覆盖已发表的 2025 年会议成果、2026 年期刊论文和截至检索日可验证的预印本。对于只提供摘要的论文，本文不陈述摘要以外的消融或具体数值结论。

## References

[1] Yehuda Koren, Robert Bell, Chris Volinsky, "Matrix Factorization Techniques for Recommender Systems," Computer, 2009.

[2] Steffen Rendle, Christoph Freudenthaler, Zeno Gantner, Lars Schmidt-Thieme, "BPR: Bayesian Personalized Ranking from Implicit Feedback," UAI, 2009.

[3] Xiangnan He, Lizi Liao, Hanwang Zhang, et al., "Neural Collaborative Filtering," WWW, 2017.

[4] Steffen Rendle, Walid Krichene, Li Zhang, John Anderson, "Neural Collaborative Filtering vs. Matrix Factorization Revisited," RecSys, 2020.

[5] Xiang Wang, Xiangnan He, Meng Wang, et al., "Neural Graph Collaborative Filtering," SIGIR, 2019.

[6] Xiangnan He, Kuan Deng, Xiang Wang, et al., "LightGCN: Simplifying and Powering Graph Convolution Network for Recommendation," SIGIR, 2020.

[7] Xubin Ren, Wei Wei, Lianghao Xia, Chao Huang, "A Comprehensive Survey on Self-Supervised Learning for Recommendation," arXiv:2404.03354, 2024.

[8] Wang-Cheng Kang, Julian McAuley, "Self-Attentive Sequential Recommendation," ICDM, 2018.

[9] Fei Sun, Jun Liu, Jian Wu, et al., "BERT4Rec: Sequential Recommendation with Bidirectional Encoder Representations from Transformer," CIKM, 2019.

[10] Fangyu Li, Shenbao Yu, Feng Zeng, Fang Yang, "Improving Sequential Recommendation Models with an Enhanced Loss Function," arXiv:2301.00979, 2023.

[11] Kyungho Kim, Sunwoo Kim, Geon Lee, et al., "A Comprehensive Survey on Multi-Behavior Recommender Systems: Extended Taxonomy and Recent Advances," International Journal of Data Science and Analytics, 2026.

[12] Xiaoqing Chen, Zhitao Li, Weike Pan, Zhong Ming, "A Survey on Multi-Behavior Sequential Recommendation," arXiv:2308.15701, 2023.

[13] Hu Liu, Xiao Huang, Wei Zhou, et al., "Representation-Enhanced Cascading Multi-Level Interest Learning for Multi-Behavior Recommendation," ACM Transactions on Information Systems, 2026.

[14] Huayi Shen, Wei Zhou, Fengji Luo, et al., "Spatio-Temporal Intent Modeling for Sequential Recommendation," IEEE Transactions on Services Computing, 2025.

[15] Shutong Qiao, Wei Zhou, Junhao Wen, et al., "Multi-view Intent Learning and Alignment with Large Language Models for Session-based Recommendation," ACM Transactions on Information Systems, 2025.

[16] Qilin Zhou, Jinkang Ye, Wei Zhou, Junhao Wen, "Multi-scale Hypergraph Representation Learning with Channel Attention for Next POI Recommendation," Neurocomputing, 2026.

[17] Jinfeng Xu, Zheyu Chen, Shuo Yang, et al., "A Survey on Multimodal Recommender Systems: Recent Advances and Future Directions," arXiv:2502.15711, 2025.

[18] Chenghao Li, Wei Zhou, Yihao Zhang, et al., "MSCF-net: Multi-scale Frequency Denoising and Co-frequency Enhancement Network for Multimodal Recommendation," Expert Systems with Applications, 2026.

[19] Yingqiang Ge, Shuchang Liu, Zuohui Fu, et al., "A Survey on Trustworthy Recommender Systems," arXiv:2207.12515, 2022.

[20] Chen Gao, Yu Zheng, Wenjie Wang, et al., "Causal Inference in Recommender Systems: A Survey and Future Directions," ACM Transactions on Information Systems, 2024.

[21] Huishi Luo, Fuzhen Zhuang, Ruobing Xie, et al., "A Survey on Causal Inference for Recommendation," The Innovation, 2024.

[22] Jie Liao, Min Yang, Wei Zhou, Hongyu Zhang, Junhao Wen, "Modeling Item Exposure and User Satisfaction for Debiased Recommendation with Causal Inference," Information Sciences, 2024.

[23] Giovanni De Toni, Erasmo Purificato, Emilia Gómez, et al., "You Don't Bring Me Flowers: Mitigating Unwanted Recommendations Through Conformal Risk Control," RecSys, 2025.

[24] Chengkai Huang, Hongtao Huang, Tong Yu, et al., "A Survey of Foundation Model-Powered Recommender Systems: From Feature-Based, Generative to Agentic Paradigms," arXiv:2504.16420, 2025.

[25] Shijie Geng, Shuchang Liu, Zuohui Fu, et al., "Recommendation as Language Processing: A Unified Pretrain, Personalized Prompt & Predict Paradigm," RecSys, 2022.

[26] Shashank Rajput, Nikhil Mehta, Anima Singh, et al., "Recommender Systems with Generative Retrieval," NeurIPS, 2023.

[27] Keqin Bao, Jizhi Zhang, Yang Zhang, et al., "TALLRec: An Effective and Efficient Tuning Framework to Align Large Language Model with Recommendation," RecSys, 2023.

[28] Junling Liu, Chao Liu, Peilin Zhou, et al., "LLMRec: Benchmarking Large Language Models on Recommendation Task," arXiv:2308.12241, 2023.

[29] Qiyao Peng, Hongtao Liu, Hua Huang, et al., "A Survey on LLM-powered Agents for Recommender Systems," Findings of EMNLP, 2025.

[30] Maurizio Ferrari Dacrema, Paolo Cremonesi, Dietmar Jannach, "Are We Really Making Much Progress? A Worrying Analysis of Recent Neural Recommendation Approaches," RecSys, 2019.

[31] Zhu Sun, Hui Fang, Jie Yang, et al., "DaisyRec 2.0: Benchmarking Recommendation for Rigorous Evaluation," IEEE Transactions on Pattern Analysis and Machine Intelligence, 2023.

[32] Wayne Xin Zhao, Shanlei Mu, Yupeng Hou, et al., "RecBole: Towards a Unified, Comprehensive and Efficient Framework for Recommendation Algorithms," CIKM, 2021.

[33] Vito Walter Anelli, Alejandro Bellogín, Antonio Ferrara, et al., "Elliot: A Comprehensive and Rigorous Framework for Reproducible Recommender Systems Evaluation," SIGIR, 2021.

