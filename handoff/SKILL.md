---
name: handoff
description: HandOff — 在项目中建立 WorkBuddy ↔ CodeBuddy/OpenCode 双（多）工具协作的「共享项目大脑」（.codebuddy/CODEBUDDY.md + handoff.md 交接快照 + AGENTS.md 项目规则），让在 VSCode/VS 用 CodeBuddy 写代码 debug、OpenCode 接手、WorkBuddy 定义功能/文档共享同一份上下文，跨工具/跨 session 接力不重复描述需求，减少 token 耗用。当用户要求"让 workbuddy 和 codebuddy/opencode 协作/互协"、"新建项目并让两个工具配合"、"给已有项目建立 CODEBUDDY.md 项目记忆"、"HandOff/handoff/任务交接/打包上下文/交给下一个agent/生成交接文档"、或提到新/旧项目要双工具协办时使用本技能。需要用户提供项目位置（新或已有）；新项目还必须提供项目描述。
agent_created: true
---

# HandOff — WorkBuddy ↔ CodeBuddy/OpenCode 双工具协作落地

## Overview

在用户的单机工作流中：CodeBuddy 在 VSCode/Visual Studio 里写代码、debug、构建；OpenCode 可接手代码/debug；WorkBuddy 负责定义需求、写文档、规划、维护与交接。两个工具**没有实时对话通道**，上下文只能通过项目文件夹内的 `.codebuddy/` 文件与 `handoff.md` 流通。本技能把「建立并维护共享项目大脑」+「生成结构化交接快照」的完整流程固化下来：收集项目信息 → 按新/已有项目两条路径落地 `.codebuddy/` → 验证源码零改动 → 交付交接说明 → 按需生成 handoff 快照。执行一次，该项目长期受益。

## Step 0 — 收集必填输入（缺一项就问，用 AskUserQuestion）

1. **项目位置**（必填）：新项目要建的目录，或已有项目的根目录。用户没给就问。
2. **新 or 已有**（自动判定 + 确认）：检查该路径是否已存在且有内容。
3. **项目描述**（仅新项目必填）：这是什么、给谁用、技术栈（若已知）。**新项目缺少描述不得开工**——CODEBUDDY.md 的简介和技术栈要靠它填充。

## Workflow Decision Tree

```
项目路径已存在且有内容？
├─ 是 → Path A：Onboard Existing Project（读仓库，生成 .codebuddy/）
└─ 否 → Path B：Bootstrap New Project（建目录，生成 .codebuddy/ + 可选 PRD）
```

两条路径的共同产物：`.codebuddy/CODEBUDDY.md`（项目大脑）+ `.codebuddy/rules/project-rules.md`（维护规则）+ `.codebuddy/plans/`（任务计划目录）。

## Path B — Bootstrap New Project（新项目）

1. 创建项目根目录（不存在时）。
2. 从 `assets/CODEBUDDY.md` 生成 `.codebuddy/CODEBUDDY.md`：用用户描述填充「项目简介」「技术栈」，其余小节保留 TODO 占位，不要留空章节标题以外的废话。
3. 从 `assets/project-rules.md` 生成 `.codebuddy/rules/project-rules.md`。
4. 创建空目录 `.codebuddy/plans/`。
5. 用户需要需求文档时，从 `assets/PRD-template.md` 生成 `docs/PRD.md`（或用户指定位置）并引导填写。
6. **handoff.md 只生成一次**：若项目根不存在 `handoff.md`，从 `assets/handoff.md` 复制一份到项目根；**若已存在，绝不覆盖**。
7. **AGENTS.md 只生成一次**：若项目根不存在 `AGENTS.md`，从 `assets/AGENTS.md` 生成；**若已存在，绝不覆盖**。其中已包含「修改代码前必读 handoff.md」规则。
8. 若目录不是 git 仓库，执行 `git init` 并建议把 `.codebuddy/`、`handoff.md`、`AGENTS.md` 纳入首次提交。
9. 交接说明：告诉用户下一步在 VSCode/VS 用 CodeBuddy 打开同一文件夹写代码；WorkBuddy 之后做文档与维护。

## Path A — Onboard Existing Project（已有项目）

1. **只读扫描**（先读 `references/existing-project-scan.md` 拿扫描清单）：README*、清单文件（*.sln/*.csproj/package.json/pyproject.toml/go.mod 等）、顶层目录树、.gitignore、源码注释里的坑点线索。
2. 生成 `.codebuddy/CODEBUDDY.md`：填**真实**信息——技术栈（含钉死版本）、关键路径表（10–20 行，勿穷举）、核心架构要点（指向权威文档章节，**不要大抄 README**）、已知坑点（编号列表，从 README/注释提取）。
3. 生成 `.codebuddy/rules/project-rules.md`（分工 + 维护规矩通用；项目特定红线填进「改源码红线」）。
4. 确保 `.codebuddy/plans/` 存在。
5. **若已存在 `.codebuddy/CODEBUDDY.md`：先读旧文件，在保留原有内容基础上补充，绝不覆盖。**
6. **handoff.md 只生成一次**：若项目根不存在 `handoff.md`，从 `assets/handoff.md` 复制一份；**若已存在，绝不覆盖**。
7. **AGENTS.md 只生成一次**：若项目根不存在 `AGENTS.md`，从 `assets/AGENTS.md` 生成；**若已存在，绝不覆盖**。其中已包含「修改代码前必读 handoff.md」规则。

## Verification（每次必做）

- [ ] 列出 `.codebuddy/` 目录树，确认三个产物就位
- [ ] 确认 `handoff.md` 与 `AGENTS.md` 在项目根就位（若不存在则已从 assets 复制一次）
- [ ] 若是 git 仓库：运行 `git status --short`，确认**只有 `.codebuddy/`、`handoff.md`、`AGENTS.md` 为新增**；若工作区已有预存在的 `M`/`D` 条目，原样告知用户那不是本次改动
- [ ] 已有项目：再次确认没有修改/删除/移动任何源码文件

## Handoff Snapshot（交接快照，新增能力）

当用户在任一工具中完成一段工作、要切换到另一工具或新 session 接力时，生成结构化交接快照，**不放对话原文，只放可执行信息**，减少 token 耗用。

### 触发条件
- 用户输入 `/handoff`，或自然语言提到：任务交接、打包上下文、交给下一个 agent、生成交接文档、切换到 OpenCode/CodeBuddy/WorkBuddy 接手。

### 执行逻辑
1. 从当前对话/项目状态提取信息，**过滤闲聊、作废草稿、失败尝试**，只保留可执行结论。
2. 从 `assets/handoff.md` 生成目标项目的 `handoff.md`（建议放在项目根或 `.codebuddy/handoff.md`），填入 9 个模块：
   - 🎯 任务目标（原始需求、最终交付物、硬性要求）
   - ✅ 已完成工作清单（含文件路径）
   - ⏳ 当前进度（停在哪一步、最后一次成功验证）
   - 📌 待办清单（按优先级）
   - ⚠️ 约束与硬性规则（不可修改、格式规范、禁止事项）
   - 📁 资产文件清单
   - ❗ 已知问题/风险
   - 📝 给下一个 Agent 的额外指令
3. 未知内容标注【待确认】，**不编造**不存在的信息。
4. **不自动执行任务**，仅生成交接文档。
5. 生成完成后提示：「交接快照生成完毕。新 session 第一句：请先读 handoff.md，按里面的约束和目标继续。」

### 输出约束
- 只输出交接文档，不要多余闲聊解释。
- 文档尽量精简，保留可执行信息，不要冗余对话原文。

### 跨工具交接流程（文件流通）
- **WorkBuddy → OpenCode/CodeBuddy**：WorkBuddy 先把最新状态写回 `CODEBUDDY.md`（进行中的工作/决策记录）+ `handoff.md`，再切工具；接手方第一句读这两份文件，不载入完整对话历史。
- **OpenCode/CodeBuddy → WorkBuddy**：在源工具生成同等 `handoff.md`，WorkBuddy 读 `CODEBUDDY.md` + `handoff.md` 接手。
- 交接后让接手方 agent 复述「目标 / 关键失败尝试 / 为何选此方案 / 哪些检查通过 / 未解问题」以验证。

### Token 优化
- 交接只读 `handoff.md` + `CODEBUDDY.md`（+ 可选 `00_项目总览.md`），**不载入全量对话**。
- 对话变长时先 `/compact` 一次再生成 handoff。
- 长期约定写入 `.workbuddy/memory/MEMORY.md`，新窗口自动带上。

## Standing Collaboration Rules（写入每个项目，向用户复述）

- **分工**：CodeBuddy = 写代码/debug/构建；OpenCode = 接手代码/debug（可选）；WorkBuddy = 定义功能/文档/规划/维护 CODEBUDDY.md 与 handoff。
- **回写**：任一边做完结构性改动（增删移文件、改架构、改用户可见文案），更新 `CODEBUDDY.md` 对应小节；debug 发现的坑点进「已知坑点」。
- **交接快照**：切换工具/新 session 前生成 `handoff.md`（9 模块结构化），只放可执行信息；接手方第一句读它，不载入完整对话。
- **多步任务**：用 `.codebuddy/plans/<任务名>.md` 记录，frontmatter 格式 `name` / `overview` / `todos[].status`。
- **入库**：把 `.codebuddy/` 与 `handoff.md` 提交进 git，跟着项目走（先确认 .gitignore 没排除它）。
- **无实时通道**：「告知对方」一律靠写文件，不靠口头传递。

## Resources

### assets/（复制进目标项目的模板）

- `assets/CODEBUDDY.md` → 目标项目 `.codebuddy/CODEBUDDY.md`
- `assets/project-rules.md` → 目标项目 `.codebuddy/rules/project-rules.md`
- `assets/PRD-template.md` → 新项目可选，`docs/PRD.md`
- `assets/handoff.md` → 项目根 `handoff.md`（仅当不存在时复制一次，不覆盖）
- `assets/AGENTS.md` → 项目根 `AGENTS.md`（仅当不存在时生成一次，不覆盖；含「修改代码前必读 handoff.md」规则）

### references/（按需加载）

- `references/existing-project-scan.md` — **Path A 开始前必读**：已有项目的只读扫描清单、输出结构与红线
