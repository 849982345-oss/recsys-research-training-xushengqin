# 第三次课：远程实验环境与 SASRec Smoke Demo

> 本节课的核心不是学习一个全新的推荐模型，而是把“远程 Linux + Python 环境 + GPU + SASRec 小实验”完整跑通。
>
> 命令中的服务器地址、用户名、端口和 Token 以老师最新通知为准。不要把真实密码或 Token 写进代码、Git 仓库、截图或实验日志。

## 1. 本节课要完成的实验链路

~~~text
本地电脑 → SSH 登录远程 Linux
        → VS Code Remote SSH 打开远程目录
        → Conda 创建环境
        → 安装并验证 PyTorch / CUDA
        → 下载课程代码
        → 运行 SASRec Smoke Demo
        → 查看 HR、NDCG、日志、Checkpoint 和显存
~~~

Smoke Demo 是小规模流程测试，不代表正式推荐效果。它的目标是验证代码、数据、环境、GPU 和实验输出都正常。

## 2. 名词速查

| 名词         | 通俗解释                          |
| ---------- | ----------------------------- |
| SSH        | 通过命令行远程登录另一台电脑                |
| Remote SSH | 让 VS Code 直接编辑和运行远程服务器代码      |
| Linux      | 服务器上常用的操作系统                   |
| Conda      | 创建相互隔离的 Python 环境             |
| PyTorch    | 深度学习训练框架                      |
| CUDA       | 让 PyTorch 调用 NVIDIA GPU 的软件平台 |
| AutoDL     | GPU 实验机器租用/管理平台               |
| NAS        | 课程组共享代码和数据的存储设备               |
| Checkpoint | 保存下来的模型参数                     |

## 3. SSH 和 VS Code Remote SSH

### 3.1 测试 SSH 端口

在 Windows PowerShell 中执行：

~~~powershell
Test-NetConnection <服务器地址> -Port 22
~~~

看到 TcpTestSucceeded : True，说明网络可以访问 SSH 端口。

### 3.2 登录并确认位置

~~~powershell
ssh <用户名>@<服务器地址>
~~~

第一次连接时，确认地址正确后输入 yes。密码输入时终端通常不会显示字符，这是正常的。

登录后执行：

~~~bash
whoami
hostname
pwd
ls -lah
mkdir -p ~/research-training/lesson03
cd ~/research-training/lesson03
pwd
~~~

这一步确认当前用户、服务器、工作目录和目录内容，避免把代码跑在本地或别人的目录中。

### 3.3 VS Code 连接步骤

1. 安装扩展 Remote - SSH。
2. 执行 Remote-SSH: Open SSH Configuration File。
3. 添加配置：

~~~sshconfig
Host lab-server
    HostName <服务器地址>
    User <你的用户名>
    Port 22
~~~

4. 执行 Remote-SSH: Connect to Host，选择 lab-server。
5. 打开 ~/research-training/lesson03。
6. 在 VS Code 终端运行 pwd 和 hostname。

如果路径或主机名不是远程服务器，说明当前窗口仍然是本地环境。

## 4. Linux 最小命令集

~~~bash
mkdir -p linux_practice
cd linux_practice
pwd
ls -lah
touch notes.txt
printf "Linux command practice\nGPU environment check\nSASRec smoke demo\n" > notes.txt
cat notes.txt
head -n 2 notes.txt
tail -n 2 notes.txt
cp notes.txt notes_copy.txt
mv notes_copy.txt notes_backup.txt
grep -n "GPU" notes.txt
find . -maxdepth 1 -type f -print
du -sh .
df -h ~
which python
~~~

练习 Shell 脚本：

~~~bash
printf '#!/usr/bin/env bash\necho "Hello from Linux"\n' > hello.sh
chmod u+x hello.sh
./hello.sh
~~~

删除文件时优先使用交互确认：

~~~bash
rm -i notes_backup.txt
~~~

## 5. Miniconda、PyTorch 和 CUDA

### 5.1 创建 Conda 环境

~~~bash
uname -m
conda create -n lesson03 python=3.10 -y
conda activate lesson03
which python
python --version
python -m pip --version
~~~

which python 应指向类似 ~/miniconda3/envs/lesson03/ 的路径，而不是系统 Python。

课程材料建议配置可用的 Conda/PyPI 镜像。配置前先备份 ~/.condarc，网络异常时以老师当前通知为准，不要随意混用不同来源的 CUDA/PyTorch 包。

### 5.2 GPU 版本

先检查显卡和其他进程：

~~~bash
nvidia-smi
~~~

课程材料示例组合为 Python 3.10、PyTorch 2.12.1、CUDA 12.6：

~~~bash
python -m pip install torch==2.12.1 --index-url https://download.pytorch.org/whl/cu126
~~~

验证：

~~~bash
python -c "import torch; print('PyTorch:', torch.__version__)"
python -c "import torch; print('PyTorch CUDA runtime:', torch.version.cuda)"
python -c "import torch; print('CUDA available:', torch.cuda.is_available())"
python -c "import torch; print('GPU:', torch.cuda.get_device_name(0) if torch.cuda.is_available() else 'CPU only')"
~~~

### 5.3 没有 GPU 时使用 CPU

~~~powershell
conda create -n lesson03 python=3.10 -y
conda activate lesson03
python -m pip install torch==2.12.1 --index-url https://download.pytorch.org/whl/cpu
python -c "import torch; print(torch.__version__)"
python -c "import torch; print(torch.cuda.is_available())"
~~~

CPU 版本预期输出 False，但可以完成 Smoke Demo 的功能测试。

## 6. AutoDL、NAS 和 Token

### AutoDL

选择实例时关注 GPU 型号、显存、价格、磁盘、镜像和可用状态。启动后检查：

~~~bash
nvidia-smi
python -c "import torch; print(torch.__version__, torch.cuda.is_available())"
pwd
df -h
~~~

保存代码、日志和模型后及时关机或释放实例，避免产生额外费用。

### NAS

NAS 只用于获取课程代码和数据。下载到自己的实验目录后再解压和修改，不要直接修改共享原文件。

### Token

Token 只在当前终端临时加载，并且只验证是否存在：

~~~bash
read -s LAB_AGENT_TOKEN
export LAB_AGENT_TOKEN
python -c "import os; print('Token loaded:', bool(os.getenv('LAB_AGENT_TOKEN')))"
unset LAB_AGENT_TOKEN
python -c "import os; print('Token loaded:', bool(os.getenv('LAB_AGENT_TOKEN')))"
~~~

不要打印 Token 本身，也不要放进 .py、.md、.json、Git 提交或截图。

## 7. SASRec Smoke Demo 的目录

解压课程代码后，目录应类似：

~~~text
sasrec_smoke_demo/
├── sasrec_demo.py
├── generate_smoke_data.py
├── README.md
├── data/
│   └── smoke_interactions.csv
└── tests/
    └── test_sasrec_demo.py
~~~

检查目录：

~~~bash
cd ~/research-training/lesson03/sasrec_smoke_demo
pwd
find . -maxdepth 2 -type f -print
~~~

## 8. 数据和模型的核心逻辑

### 8.1 Smoke 数据

- 128 个用户；
- 120 个物品；
- 每个用户 24 次有序交互；
- 总计 3072 条交互；
- CSV 字段为 user_id,item_id,timestamp。

这是人工生成的小数据集，只用于快速检查代码，不代表真实推荐数据集。

### 8.2 时间切分

每个用户的交互按时间排序：

~~~text
前 22 条 → 训练集
倒数第 2 条 → 验证集
最后 1 条 → 测试集
~~~

这是“用过去预测未来”的序列推荐切分，可以避免未来行为泄漏到训练集。

### 8.3 SASRec 数据流

~~~text
物品 ID
  → 物品 Embedding + 位置 Embedding
  → 因果 Self-Attention
  → 前馈网络
  → 序列表示
  → 与候选物品向量点积
  → 正负样本损失
~~~

训练时，正样本是用户真实发生的下一个物品，负样本是用户没有交互过的物品。序列过长从左侧截断，不足 max_len 时左侧补 0；0 是 padding，不是有效物品。因果 mask 保证预测某个位置时不能看到未来位置。

### 8.4 指标

- HR@K：真实目标是否进入 Top-K；
- NDCG@K：目标越靠前，得分越高；
- valid：选择最佳训练轮次；
- test：最后只评估一次，不能反复用测试集挑模型。

## 9. 按顺序运行实验

### 9.1 检查数据和可重复生成

~~~bash
head -n 6 data/smoke_interactions.csv
wc -l data/smoke_interactions.csv
python generate_smoke_data.py --output /tmp/smoke_interactions_regenerated.csv
cmp data/smoke_interactions.csv /tmp/smoke_interactions_regenerated.csv
~~~

预期总行数为 3073（表头 1 行 + 数据 3072 行）；cmp 无输出且返回成功，说明生成结果逐字节一致。

### 9.2 单元测试和 CPU 最小运行

~~~bash
python -m unittest discover -s tests -v
python sasrec_demo.py --device cpu --epochs 1
~~~

测试覆盖数据切分、负样本、模型输出形状、因果 mask、一次 CPU 更新和端到端输出文件。先通过 CPU，再排查 GPU，定位问题会更容易。

### 9.3 GPU 检查和训练

~~~bash
CUDA_VISIBLE_DEVICES=0 python sasrec_demo.py --device cuda --check-only
~~~

确认环境后运行：

~~~bash
CUDA_VISIBLE_DEVICES=0 python sasrec_demo.py \
  --device cuda \
  --epochs 10 \
  --batch-size 32 \
  --max-len 20 \
  --hidden-size 64 \
  --num-heads 2 \
  --num-blocks 2 \
  --allocator-limit-mib 700 \
  --gpu-budget-mib 1024
~~~

hidden-size 必须能被 num-heads 整除；本例为 64 ÷ 2 = 32。

### 9.4 查看输出

~~~bash
RUN_DIR=$(ls -dt outputs/run_* | head -n 1)
echo "$RUN_DIR"
ls -lh "$RUN_DIR"
tail -n 20 "$RUN_DIR/train.log"
cat "$RUN_DIR/summary.json"
~~~

正常输出包含：

~~~text
config.json       # 实验参数
train.log         # 环境、训练和显存日志
metrics.jsonl     # 每轮 loss、HR、NDCG
summary.json      # 最终结果和资源摘要
best_model.pt     # 验证集最优模型
~~~

## 10. 显存监控

另开终端观察 GPU：

~~~bash
watch -n 1 nvidia-smi
~~~

本实验有两层显存限制：

- allocator-limit-mib 700：限制 PyTorch 缓存分配器；
- gpu-budget-mib 1024：监控进程实际显存，超过预算时终止实验。

summary.json 中的 gpu_memory_within_budget 是最终检查结果。如果系统无法返回进程级显存，该字段可能为 null，结合 nvidia-smi 判断即可。

## 11. 常见问题排查

| 现象 | 优先检查 |
|---|---|
| SSH 连不上 | 地址、端口、校园网/VPN、Test-NetConnection |
| VS Code 实际跑在本地 | 左下角 Remote SSH 状态、pwd、hostname |
| conda 找不到 | conda init、source ~/.bashrc、是否打开新终端 |
| CUDA 为 False | 是否装了 CPU 版、GPU 是否可见、PyTorch/CUDA 是否匹配 |
| 找不到数据文件 | pwd、find、data-path |
| 显存超限 | 降低 batch size、hidden size、block 数，检查其他 GPU 进程 |
| 测试失败 | 先看第一条失败断言，不要直接重装全部依赖 |
| 每次结果不同 | 检查 seed、数据切分和是否修改代码 |
| Token 可能泄露 | 立即撤销/更换 Token，并检查日志和 Git 历史 |

## 12. 本节课验收清单

- [ ] 能解释 SSH、Remote SSH、Conda、PyTorch、CUDA、NAS、AutoDL。
- [ ] 能使用 pwd、ls、cd、cat、grep、find。
- [ ] 能创建并激活 lesson03 环境。
- [ ] 能判断当前是 CPU 版还是 GPU 版 PyTorch。
- [ ] 能通过 python -m unittest discover -s tests -v。
- [ ] 能完成一次 CPU Smoke 运行。
- [ ] 有 GPU 时能完成环境检查和受限显存训练。
- [ ] 能解释训练集、验证集、测试集及 HR@K/NDCG@K。
- [ ] 能找到日志、指标、摘要和 Checkpoint。
- [ ] 没有把密码、Token 或私密服务器信息提交到 Git。

## 13. 和前两节课的衔接

前两节课解决“Python、张量和推荐系统代码是什么”；第三节课解决“如何让代码在可控的远程实验环境中运行”。下一步按这个顺序推进：

~~~text
读懂 SASRec 数据流
  → 修改一个小参数
  → 比较 loss / HR / NDCG / 显存 / 时间
  → 记录实验配置
  → 再进入 LightGCN、SASRec 和课题组论文复现
~~~

Smoke Demo 不是正式论文实验，但它训练了以后做推荐系统研究必须具备的基本功：环境可复现、数据切分清楚、实验有日志、资源可监控、结果能重跑。

