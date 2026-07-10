# Obsidian 仓库与 GitHub 同步演示

## 一、准备

演示前准备：

- 已安装 Obsidian。
- 已安装 Git。
- 已登录 GitHub。
- 可以打开 PowerShell。
- 准备一个本地存放笔记仓库的位置，例如：

```text
D:\research-notes
```

学生需要准备：

- GitHub 账号。
- Obsidian。
- Git。
- 自己的姓名拼音，用于仓库命名。

第一次使用 Git 时，先配置用户名和邮箱：

```powershell
git config --global user.name "你的姓名或GitHub用户名"
git config --global user.email "你的邮箱"
```

检查配置：

```powershell
git config --global user.name
git config --global user.email
```

## 二、内容

本次演示新建一个 Obsidian 仓库，用于保存课程作业和科研训练笔记。

仓库名称：

```text
recsys-research-training-姓名拼音
```

示例：

```text
recsys-research-training-zhangsan
```

目录结构：

```text
recsys-research-training-姓名拼音/
├── README.md
├── learning_plan.md
├── daily_logs/
│   ├── 2026-07-10.md
│   └── 2026-07-11.md
├── assignment/
│   ├── 第一次课作业.md
│   └── ...
└── knowledge_base/
    ├── README.md
    ├── python.md
    ├── math.md
    ├── recsys.md
    ├── paper_reading.md
    └── tools.md
```

各文件和目录用途：

| 文件或目录 | 用途 |
| --- | --- |
| `README.md` | 仓库说明。 |
| `learning_plan.md` | 个人学习计划。 |
| `daily_logs/` | 每日学习记录。 |
| `assignment/` | 课程作业提交。 |
| `knowledge_base/` | 长期知识整理。 |
| `knowledge_base/README.md` | 知识库索引。 |
| `python.md` | Python 学习笔记。 |
| `math.md` | 数学基础笔记。 |
| `recsys.md` | 推荐系统笔记。 |
| `paper_reading.md` | 论文阅读笔记。 |
| `tools.md` | 工具使用笔记。 |

建议新建 `.gitignore`：

```gitignore
.obsidian/workspace.json
.obsidian/workspace-mobile.json
.trash/
.DS_Store
Thumbs.db
*.tmp
```

## 三、步骤

### 1. 新建 Obsidian 仓库

打开 Obsidian，选择 `Create new vault`。

Vault name 填写：

```text
recsys-research-training-姓名拼音
```

Location 选择本地目录，例如：

```text
D:\research-notes
```

创建完成后，进入 Obsidian 仓库。

### 2. 新建目录和文件

在 Obsidian 或文件管理器中新建以下内容：

```text
README.md
learning_plan.md
daily_logs/
daily_logs/2026-07-10.md
daily_logs/2026-07-11.md
assignment/
assignment/第一次课作业.md
knowledge_base/
knowledge_base/README.md
knowledge_base/python.md
knowledge_base/math.md
knowledge_base/recsys.md
knowledge_base/paper_reading.md
knowledge_base/tools.md
```

### 3. 写入最少内容

`README.md`：

```markdown
# 推荐系统科研训练笔记

本仓库用于保存课程作业、每日记录和科研训练笔记。
```

`learning_plan.md`：

```markdown
# 个人学习计划

## 当前目标

- 学会使用 Obsidian 管理笔记。
- 学会使用 GitHub 提交作业。
- 建立每日科研记录习惯。
```

`daily_logs/2026-07-10.md`：

```markdown
# 2026-07-10 学习记录

## 今天完成了什么

## 遇到的问题

## 明天准备做什么
```

`assignment/第一次课作业.md`：

```markdown
# 第一次课作业

## GitHub 仓库地址

## 已完成内容

- [ ] 新建 Obsidian 仓库。
- [ ] 建立目录结构。
- [ ] 初始化 Git 仓库。
- [ ] 同步到 GitHub。

## 遇到的问题
```

`knowledge_base/README.md`：

```markdown
# 知识库索引

- `python.md`：Python 笔记。
- `math.md`：数学基础笔记。
- `recsys.md`：推荐系统笔记。
- `paper_reading.md`：论文阅读笔记。
- `tools.md`：工具使用笔记。
```

### 4. 打开 PowerShell

进入 Obsidian 仓库根目录：

```powershell
cd D:\research-notes\recsys-research-training-姓名拼音
```

确认当前位置：

```powershell
pwd
```

### 5. 初始化 Git 仓库

```powershell
git init
git status
git add .
git commit -m "init obsidian research training notes"
```

### 6. 在 GitHub 新建仓库

打开 GitHub，点击 `New repository`。

Repository name 填写：

```text
recsys-research-training-姓名拼音
```

设置：

- 选择 `Public`。
- 不勾选 `Add a README file`。
- 不添加 `.gitignore`。
- 不选择 license。

创建仓库后，复制远程仓库地址。

### 7. 连接 GitHub 并同步

回到 PowerShell，执行：

```powershell
git branch -M main
git remote add origin https://github.com/用户名/recsys-research-training-姓名拼音.git
git push -u origin main
```

把 `用户名` 和 `姓名拼音` 替换成自己的真实信息。

示例：

```powershell
git remote add origin https://github.com/zhangsan/recsys-research-training-zhangsan.git
```

同步完成后，刷新 GitHub 仓库页面，检查文件是否已经上传。

### 8. 后续日常更新

每次写完笔记或作业后，执行：

```powershell
git status
git add .
git commit -m "update notes"
git push
```

作业提交时，确认 GitHub 上能看到：

- `assignment/第一次课作业.md`
- `daily_logs/`
- `knowledge_base/`
- `README.md`
- `learning_plan.md`
