# 学习工作流文档

本文件描述该项目的完整学习流程，供 Claude Code 作为上下文参考。

---

## 项目信息

| 项目 | 详情 |
|------|------|
| 课程 | MIT 14.01 Principles of Microeconomics, Fall 2018 |
| 开始时间 | 2026-03-28 |
| 目标完成 | 2026-06-25（约13周，每周二周四各2小时） |
| 工作区路径 | `/Users/yaohaizhou/Documents/code/Principles_of_Microeconomics_MIT/` |
| GitHub Repo | `git@github.com:yaohaizhou/Principles_of_Microeconomics_MIT_Learning_Notes.git` |
| NotebookLM Notebook | MIT 14.01 微观经济学原理 (Fall 2018) |
| NotebookLM ID | `78c7f9dc-c30e-4f03-a2e8-60df4a626b21` |
| YouTube 播放列表 | https://www.youtube.com/playlist?list=PLUl4u3cNGP62oJSoqb4Rf-vZMGUBe59G- |

---

## 每讲学习流程（标准步骤）

### 第一步：看视频
- 打开对应讲座的 YouTube 链接（见 `notes/L##.md` 顶部）
- 边看边在 `notes/L##.md` 里填写：
  - 学习日期
  - 直觉理解（用自己的话解释）
  - 现实应用（联系真实经济现象）
  - 遗留问题

### 第二步：用 NotebookLM 深化理解
- 打开 NotebookLM，选择 notebook `78c7f9dc-c30e-4f03-a2e8-60df4a626b21`
- 推荐提问模板：
  - "这节课（L##: 标题）最重要的3个概念是什么，请结合视频内容解释"
  - "给我出2道关于[概念]的练习题，附上解答"
  - "用一个现实案例解释[公式/模型]"
  - "对比[概念A]和[概念B]的区别"
- 把有价值的问答记录到笔记的"NotebookLM 问答记录"区

也可以让 Claude Code 直接调用 NotebookLM：
```bash
python3.11 -m notebooklm ask "问题内容" --notebook 78c7f9dc-c30e-4f03-a2e8-60df4a626b21
```

### 第三步：生成可视化学习资料（每讲完成后）

每学完一讲，用 NotebookLM 生成两种可视化资料，存入 `notes/assets/L##/`：

**Infographic（卡通插画风，易于记忆）：**
```bash
# 获取该讲的 source ID（从 source list 中找）
python3.11 -m notebooklm source list --notebook 78c7f9dc-c30e-4f03-a2e8-60df4a626b21 --json

# 生成 kawaii 风格 infographic（异步，需等待）
python3.11 -m notebooklm generate infographic \
  -s <source_id> --style kawaii \
  --notebook 78c7f9dc-c30e-4f03-a2e8-60df4a626b21 --json

# 等待完成后下载（用 artifact_id）
python3.11 -m notebooklm artifact wait <artifact_id> -n 78c7f9dc-c30e-4f03-a2e8-60df4a626b21
python3.11 -m notebooklm download infographic notes/assets/L##/infographic_kawaii.png \
  -a <artifact_id> -n 78c7f9dc-c30e-4f03-a2e8-60df4a626b21
```

**Mind Map（思维导图，结构化梳理）：**
```bash
# 生成（同步，即时完成）
python3.11 -m notebooklm generate mind-map \
  -s <source_id> --notebook 78c7f9dc-c30e-4f03-a2e8-60df4a626b21 --json

# 下载（用 artifact_id，注意多讲时指定 ID 避免下错）
python3.11 -m notebooklm download mind-map notes/assets/L##/mindmap.json \
  -a <artifact_id> -n 78c7f9dc-c30e-4f03-a2e8-60df4a626b21
```

生成后在 `notes/L##.md` 的"关键图形"区块嵌入：
```markdown
## 关键图形

### NotebookLM Infographic
![L## Infographic - Kawaii](assets/L##/infographic_kawaii.png)

### 视频截图
[▶ 视频跳转（MM:SS）](https://youtu.be/VIDEO_ID?t=SECONDS)
![概念名称](assets/L##/screenshot_name.png)
```

**注意事项：**
- Infographic 生成时间约 5-10 分钟，建议用后台 agent 等待
- 同时生成多个 mind map 时，download 须用 `-a <artifact_id>` 指定，避免下到错误的版本
- 视频截图由用户手动截取（`Shift+Cmd+4`），保存到 `notes/assets/L##/` 后告知 Claude 嵌入

### 第四步：整理笔记并同步 GitHub
每学完一讲（或每周末），将笔记推送到 GitHub：

```bash
cd /Users/yaohaizhou/Documents/code/Principles_of_Microeconomics_MIT
git add notes/L##.md
git commit -m "完成 L##: [讲座标题] 学习笔记"
git push
```

或者让 Claude Code 帮助执行这些命令。

### 第四步：更新进度总览
学完每讲后，在 `学习规划.md` 的进度表里把 `[ ]` 改为 `[x]`。

---

## NotebookLM 操作说明

**CLI 工具**：`python3.11 -m notebooklm`（需要 Python 3.11+，Python 3.9 不兼容）

**常用命令**：
```bash
# 检查认证状态
python3.11 -m notebooklm auth check

# 向 notebook 提问
python3.11 -m notebooklm ask "问题" --notebook 78c7f9dc-c30e-4f03-a2e8-60df4a626b21

# 查看 notebook 中的所有 source
python3.11 -m notebooklm source list --notebook 78c7f9dc-c30e-4f03-a2e8-60df4a626b21

# 生成学习指南（study guide）
python3.11 -m notebooklm generate report --format study-guide --notebook 78c7f9dc-c30e-4f03-a2e8-60df4a626b21

# 生成测验题
python3.11 -m notebooklm generate quiz --notebook 78c7f9dc-c30e-4f03-a2e8-60df4a626b21
```

**NotebookLM 中已有的 Source**：26 个 MIT 14.01 YouTube 讲座视频（L01-L25）

---

## GitHub 同步说明

**Remote**: `git@github.com:yaohaizhou/Principles_of_Microeconomics_MIT_Learning_Notes.git`

**建议的 commit message 规范**：
```
完成 L##: [讲座标题] 学习笔记
更新学习规划，完成第 X 阶段
添加 L## 练习题解答
```

**建议同步频率**：每学完 1-2 讲推送一次，至少每周推送一次。

---

## 文件结构

```
Principles_of_Microeconomics_MIT/
├── .claude/
│   └── workflow.md           ← 本文件，学习工作流说明
├── notes/
│   ├── _模板.md              ← 笔记模板（新建讲座时参考）
│   ├── L01.md                ← 各讲座笔记（L01-L25）
│   ├── ...
│   └── assets/
│       └── L##/
│           ├── infographic_kawaii.png  ← NotebookLM 生成的 kawaii 插画
│           ├── mindmap.json            ← NotebookLM 生成的思维导图
│           └── *.png                   ← 视频截图（手动截取）
└── 学习规划.md               ← 13周进度总览 + 公式速查表
```

---

## 给 Claude Code 的操作指引

在这个项目目录下，Claude Code 应当：

1. **协助填写笔记**：用户说"帮我整理 L## 的笔记"时，读取对应的 `notes/L##.md`，结合 NotebookLM 查询内容填充。
2. **调用 NotebookLM**：使用 `python3.11 -m notebooklm` 而非 `notebooklm`（避免 Python 3.9 兼容问题）。
3. **生成可视化资料**：每讲完成后，按第三步流程生成 infographic + mind map，嵌入笔记；infographic 用后台 agent 等待，避免阻塞主对话。
4. **同步 GitHub**：帮用户 commit 和 push 时，遵循上述 commit message 规范。
5. **更新进度**：学完一讲后，同步修改 `学习规划.md` 中的进度表。
6. **不要修改模板**：`notes/_模板.md` 只作参考，不直接修改。
