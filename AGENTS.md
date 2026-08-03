# Repository Instructions — 推荐系统科研训练笔记库

本仓库是 Obsidian 笔记库（`C:\research-training\recsys-research-training-xushengqin`），
记录 2026 暑期推荐系统科研训练的学习计划、每日日志、作业与知识沉淀。

## 维护约定（所有 agent 必须遵守）

1. **提交风格**：commit message 用 `docs: <简述>` 前缀（如 `docs: add July 12 research log`）。
2. **提交时机**：完成一次有意义的改动后立即 `git add -A && git commit && git push`，
   不要留下未提交的变更；推送遇到 TLS 抖动就重试。
3. **Obsidian 兼容**：这是实时打开的 Obsidian 库——
   - 不要删除或重命名 `.obsidian/` 下的文件；
   - 笔记间的链接用 `[[Wikilink]]` 语法；
   - `.obsidian/workspace.json` 是 Obsidian 界面状态，会频繁变动，提交时如实带上即可，
     不要花精力"整理"它。
4. **目录职责**（见 README.md）：
   - `learning_plan.md`：阶段目标、每周计划与复盘（核心文档，改前先读）；
   - `daily_logs/`：每日学习记录，文件名 `YYYY-MM-DD.md`；
   - `assignment/`：课程作业；
   - `knowledge_base/`：长期复用的知识笔记（recsys / python / math / tools 等）；
   - `course_materials/`：课程资料；
   - `research_landscape/`：学术地图与阅读清单。
5. **隐私**：仓库是 PUBLIC —— 不得提交任何密钥、密码、个人信息明文。
6. **并行协作**：用户可能同时用 Codex 和 Hermes 维护本仓库，提交前先 `git pull`，
   避免冲突；同一时间只由一方提交。

## 维护人

- 当前由 **Hermes Agent**（用户助手）负责日常维护；
- 用户本人通过 Obsidian 直接编辑，agent 负责整理、提交与推送。
