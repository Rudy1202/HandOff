# HandOff

**WorkBuddy ↔ CodeBuddy / OpenCode 单机双工具协作技能** —— 用「共享项目大脑」（`.codebuddy/CODEBUDDY.md`）+「结构化交接快照」（`handoff.md`）+「项目级 agent 规则」（`AGENTS.md`），让两个工具接力时不重复描述需求，并大幅降低 token 耗用。

> A single-machine collaboration skill for WorkBuddy and CodeBuddy/OpenCode: a shared project brain, a structured handoff snapshot, and project-level agent rules — context flows between tools through files, not repeated prompts.

## 解决什么问题

- 你在 VSCode/VS 用 **CodeBuddy** 写代码、debug，在 **WorkBuddy** 定义功能、写文档 —— 两个工具**没有实时对话通道**，切工具就要把需求重讲一遍。
- 对话越来越长，新 session 加载全量历史，token 浪费严重。

HandOff 把协作固化成一键技能：**建新项目、接旧项目、切工具接力，三种场景一句话搞定。**

## 工作原理

```
        ┌──────────────────────────────────────────────┐
        │  同一个本地项目文件夹（唯一真相源）              │
        │                                              │
        │  .codebuddy/CODEBUDDY.md   ← 项目大脑          │
        │  handoff.md                ← 交接快照（9 模块）│
        │  AGENTS.md                 ← 项目级规则        │
        │  .codebuddy/rules/         ← 维护规则          │
        │  .codebuddy/plans/         ← 任务计划          │
        └──────────────────────────────────────────────┘
              ↑ 读写                          ↑ 读写
     ┌────────┴─────────┐            ┌────────┴────────┐
     │  WorkBuddy       │            │ CodeBuddy /     │
     │  定义功能/文档    │ ─────────► │ OpenCode        │
     │  维护项目大脑     │  handoff.md │  写代码/debug    │
     └──────────────────┘            └─────────────────┘
        工具间无实时通道 → 一切上下文靠文件流通
```

## 文件清单

```
HandOff/
├── README.md                  ← 本文件
└── handoff/                   ← 技能本体（安装时整份复制）
    ├── SKILL.md               ← 主工作流（收集输入 → 新/旧项目两条路径 → 验证 → 交接）
    ├── assets/                ← 可复制进目标项目的模板
    │   ├── CODEBUDDY.md       → .codebuddy/CODEBUDDY.md（项目大脑）
    │   ├── project-rules.md   → .codebuddy/rules/project-rules.md（维护规则）
    │   ├── PRD-template.md    → docs/PRD.md（新项目可选）
    │   ├── handoff.md         → 项目根 handoff.md（交接快照，已存在则不覆盖）
    │   └── AGENTS.md          → 项目根 AGENTS.md（含「改代码前必读 handoff.md」）
    └── references/
        └── existing-project-scan.md ← 旧项目只读扫描指引（Path A 必读）
```

## 安装

**用户级（推荐，所有项目可用）**：

```bash
# Windows（CMD）
xcopy /E /I <本仓库>\handoff %USERPROFILE%\.workbuddy\skills\handoff

# macOS / Linux
cp -r ./handoff ~/.workbuddy/skills/handoff
```

**项目级（随仓库共享给协作者）**：

```bash
cp -r ./handoff <项目目录>/.workbuddy/skills/handoff
```

> ⚠️ 目录名必须是小写 `handoff`（技能命名规范，需与 frontmatter `name` 一致）。安装后**重启 WorkBuddy 会话**生效。

### 命名说明（品牌名 ≠ 技能名）

- **HandOff** 是本仓库/分享品牌名（可读性好）；技能的注册名是 `handoff`（小写，目录名须与 frontmatter `name` 一致）。
- 如果你本地已装有旧名技能（如 `coding-with-workbuddy-and-codebuddy`），安装本仓库时**改目录名 + frontmatter `name`** 即可共存或替换，二者内容等价，只是叫法不同。

## 使用（激活方式）

| 方式 | 示例 |
|---|---|
| **点名调用（最可靠）** | 「用 handoff 给 `D:\...\项目路径` 建立协作，这是一个 xxx 项目」 |
| **关键词自动触发** | 「让 workbuddy 和 codebuddy 协作」「给已有项目建立 CODEBUDDY.md」「新建项目并让两个工具配合」 |
| **交接快照** | 对话输入 `/handoff`，或说「任务交接 / 打包上下文 / 交给下一个 agent / 生成交接文档」 |

### 在目标项目里会生成什么

- **新项目**：项目目录 + `.codebuddy/` 三件套（CODEBUDDY.md / rules / plans）+ `handoff.md` + `AGENTS.md`（+ 可选 PRD）；非 git 仓库会 `git init` 并建议首次提交。
- **已有项目**：只读扫描（README、清单文件、目录树、注释坑点）后生成/补写同样文件，**源码零改动**，用 `git status` 验证。

### 交接快照的 9 个模块

🎯 任务目标 · ✅ 已完成工作清单 · ⏳ 当前进度 · 📌 待办清单（按优先级）· ⚠️ 约束与硬性规则 · 📁 资产文件清单 · ❗ 已知问题/风险 · 📝 给下一个 Agent 的额外指令 ·（生成后标准提示语）

切换工具或开新 session 前跑一次 `/handoff`；接手方第一句：「请先读 handoff.md，按里面的约束和目标继续。」

## 环境要求

- **WorkBuddy**（本技能为 WorkBuddy skill 格式）
- 可选配合：**CodeBuddy**（VSCode/VS）、**OpenCode** —— 它们原生读取 `AGENTS.md` / `.codebuddy/`
- 可选：git（把 `.codebuddy/`、`handoff.md`、`AGENTS.md` 提交入库，跟着项目走）

## 许可

默认保留所有权利（All rights reserved）。按需自行修改或更换 LICENSE。
