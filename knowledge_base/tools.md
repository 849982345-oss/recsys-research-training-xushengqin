# 科研工具使用笔记

## 一、VS Code

### 1. 主要用途

- 编写 Python 代码；
- 编辑和预览 Markdown 笔记；
- 管理、搜索项目文件；
- 使用集成终端；
- 查看 Git 文件变化；
- 配合 Codex 阅读和修改项目。

### 2. 基本操作

在 VS Code 中选择“文件 → 打开文件夹”，打开仓库根目录：

```text
recsys-research-training-xushengqin
```

应打开整个仓库，而不是单独打开某一个文件。

常用操作：

- 在资源管理器中创建文件和文件夹；
- Markdown 文件使用 `.md` 后缀；
- Python 文件使用 `.py` 后缀；
- 使用 `Ctrl + Shift + V` 预览 Markdown；
- 选择“终端 → 新建终端”打开集成终端。

## 二、Obsidian

Obsidian 可以直接打开 Markdown 笔记目录，适合整理知识之间的关系。

主要功能：

- 使用双向链接关联笔记；
- 使用标签归类内容；
- 通过知识图谱查看笔记关系；
- 与 VS Code 共同编辑同一套 Markdown 文件。

课程正式笔记保存在当前 GitHub 仓库中，Obsidian 主要作为辅助阅读和组织工具，避免在不同位置维护重复内容。

## 三、Markdown

### 1. 常用语法

```markdown
# 一级标题
## 二级标题
### 三级标题

- 无序列表
1. 有序列表

- [ ] 未完成任务
- [x] 已完成任务

**加粗内容**
`行内代码`
[链接名称](链接地址)
```

代码块示例：

````markdown
```powershell
git status
```
````

### 2. 使用特点

Markdown 是普通文本格式，便于使用 VS Code 或 Obsidian 编辑，也适合在 GitHub 中保存、搜索和进行版本管理。

## 四、Git

### 1. 基本概念

Git 用于记录文件变化和管理版本。

| 概念 | 含义 |
| --- | --- |
| 工作区 | 当前正在编辑的文件 |
| 暂存区 | 准备放入下一次提交的修改 |
| 本地仓库 | 已经通过 `commit` 保存的版本 |
| 远程仓库 | GitHub 上的仓库 |

基本流程：

> 修改文件 → 查看差异 → 添加到暂存区 → 创建提交 → 推送到 GitHub

### 2. 安装与配置

```powershell
# 检查 Git 是否安装
git --version

# 配置用户信息
git config --global user.name "Xu Shengqin"
git config --global user.email "你的GitHub邮箱"

# 检查配置
git config --global user.name
git config --global user.email
```

不要将密码、验证码、Cookie、访问令牌或 API Key 写入笔记或提交到公开仓库。

### 3. 查看状态与差异

```powershell
# 查看文件状态和当前分支
git status

# 查看尚未暂存的修改
git diff

# 查看已经暂存的修改
git diff --staged

# 查看简化的提交历史
git log --oneline
```

### 4. 添加与提交

```powershell
# 只添加指定文件
git add daily_logs/2026-07-10.md knowledge_base/tools.md

# 检查待提交内容
git diff --staged

# 创建提交
git commit -m "update first lesson daily log and tools notes"
```

重要修改应优先添加指定文件，避免把无关内容一起提交。提交说明应简洁、具体地描述本次修改。

## 五、GitHub

### 1. 主要用途

- 在线保存代码和笔记；
- 展示个人项目和课程作业；
- 保存 Git 提交历史；
- 支持多设备同步和团队协作；
- 使用 Issue 和 Pull Request 跟踪问题与修改。

### 2. 创建并连接仓库

本地已有 README 等文件时，在 GitHub 创建同名空仓库，不要再次初始化 README、`.gitignore` 或 License。

```powershell
# 初始化本地仓库并首次提交
git init
git status
git add .
git commit -m "init research training repository"

# 连接远程仓库
git branch -M main
git remote add origin GitHub仓库地址
git remote -v

# 第一次推送并建立跟踪关系
git push -u origin main
```

`origin` 是远程仓库的默认名称，`main` 是要推送的分支，`-u` 用于建立本地分支与远程分支的跟踪关系。

### 3. 日常更新流程

```powershell
git status
git diff
git add 文件名
git diff --staged
git commit -m "提交说明"
git push
git status
```

推荐顺序：

> 查看变化 → 检查内容 → 暂存相关文件 → 检查待提交内容 → 创建提交 → 推送 → 再次确认状态

推送后还应刷新 GitHub 仓库页面，确认文件内容、提交说明和提交时间正确，并检查是否误传隐私文件。

## 六、Codex

### 1. 适合处理的任务

- 阅读仓库和解释代码；
- 创建、修改和整理文件；
- 运行终端命令；
- 检查错误、测试和 Git 差异；
- 在明确授权后执行 Git 提交或推送。

示例任务：

> 请更新 `daily_logs/2026-07-10.md` 和 `knowledge_base/tools.md`，保留已有有效内容并合并重复内容。修改后展示 `git diff`，不要提交。

### 2. 使用流程

> 明确任务 → Codex 读取仓库 → 修改文件 → 查看 `git diff` → 人工检查 → 暂存和提交 → 推送 → 检查 GitHub

### 3. 使用原则

- 明确允许修改的文件和操作范围；
- 修改后检查 `git diff`；
- 提交前检查暂存内容；
- 推送前确认分支和远程地址；
- 不向 Codex 提供密码、验证码或访问令牌；
- 不能用命令执行成功代替自己对知识的理解。

## 七、命令行

PowerShell 常用路径操作：

```powershell
# 显示当前路径
pwd

# 查看当前目录内容
dir

# 切换目录
cd 仓库路径
```

执行 Git 命令前，应确认终端当前位于仓库根目录。

## 八、`.gitignore`

`.gitignore` 用于指定不需要提交到 GitHub 的文件。

当前课程仓库根目录尚未创建 `.gitignore`，以下内容仅作为后续建议：

```gitignore
.vscode/
.trash/
.DS_Store
Thumbs.db
*.tmp
__pycache__/
*.pyc
.env
```

注意：

- `.env` 可能包含密钥或个人配置；
- `.gitignore` 只能忽略尚未被 Git 跟踪的文件；
- 已经提交的敏感文件需要另外移除，并及时更换泄露的凭据。

## 九、实验环境记录

运行实验时应记录：

- 操作系统和硬件环境；
- Python 版本；
- 虚拟环境名称与创建方式；
- 关键依赖及其版本；
- 安装、运行和复现实验的命令。

## 十、常见问题

### 1. 当前目录不是 Git 仓库

```powershell
pwd
git status
cd 仓库路径
```

### 2. 远程仓库已经存在

```powershell
# 查看远程地址
git remote -v

# 修改 origin 地址
git remote set-url origin 新仓库地址
```

如果已经存在 `origin`，不要重复执行 `git remote add origin ...`。

### 3. 没有可提交内容

```text
nothing to commit, working tree clean
```

这表示当前没有新的文件修改需要提交。

### 4. 修改后无法推送最新内容

`git push` 只能上传已经提交的版本。修改文件后应先执行：

```powershell
git add 文件名
git commit -m "提交说明"
git push
```

### 5. 远程仓库有本地没有的提交

先检查差异，不要直接强制推送：

```powershell
git status
git log --oneline --all
git fetch origin
```

当前阶段先熟练掌握单人仓库的创建、提交和推送流程，再学习复杂的分支、合并、变基和冲突处理。
