# AGENTS.md — <项目名>（WorkBuddy ↔ CodeBuddy/OpenCode 共用）

> 本文件是项目级 agent 规则，供 WorkBuddy / CodeBuddy / OpenCode 在同一项目文件夹协作时共享。
> **修改任何代码前，必须先读 `handoff.md`（若存在）与 `CODEBUDDY.md`。**

## 启动检查（每次新 session 第一步）
1. 检查项目根是否存在 `handoff.md`。
   - 若不存在：从 `assets/handoff.md`（或 skill 本地模板）复制一份到项目根，**仅此一次，不覆盖已有**。
2. 检查项目根是否存在 `CODEBUDDY.md`。
   - 若不存在：从 `assets/CODEBUDDY.md` 生成。
3. 读取 `handoff.md`（若存在）与 `CODEBUDDY.md`，按里面的约束与目标继续。

## 修改代码前必读
- **在修改任何源码文件之前，必须先读 `handoff.md`（若存在）**，确认当前目标、待办、约束与已知问题。
- 若 `handoff.md` 不存在，先执行「启动检查」生成它，再读取。
- 修改完成后，按 `CODEBUDDY.md` 的「文档同步规矩」回写对应小节，并更新 `handoff.md` 的「已完成/待办」。

## 分工（固定）
- CodeBuddy（VSCode / Visual Studio）：写代码、debug、构建、跑测试。
- OpenCode：接手代码/debug（可选）。
- WorkBuddy：定义需求与方案、写文档/手册、规划任务、维护 `CODEBUDDY.md` 与 `handoff.md`。

## 上下文维护
- 每次结构性改动后回写 `CODEBUDDY.md`：新增/删除/移动文件、改架构、改用户可见文案。
- debug 或实测发现的坑点写进「已知坑点」；影响用户行为的同步权威文档。
- 多步任务用 `.codebuddy/plans/<任务名>.md` 记录，frontmatter 格式 `name` / `overview` / `todos[].status`。
- 不要把对话记录粘贴进 `CODEBUDDY.md` / `handoff.md`；只放可执行的结论、约定与指针。

## 无实时通道
- 两个工具没有实时通道；一切上下文通过项目文件夹内的文件流通，「告知对方」一律靠写文件。