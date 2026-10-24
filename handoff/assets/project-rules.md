# <项目名> 项目规则（CodeBuddy / WorkBuddy 共用）

## 分工（固定）

- **CodeBuddy（VSCode / Visual Studio）**：写代码、debug、构建、跑测试。
- **WorkBuddy**：定义需求与方案、写文档/手册、规划任务、维护 `CODEBUDDY.md`。
- 两个工具没有实时通道；一切上下文通过项目文件夹内的文件流通，「告知对方」一律靠写文件。

## 上下文维护（最重要）

- 每次结构性改动后回写 `CODEBUDDY.md`：新增/删除/移动文件、改架构、改用户可见文案。
- debug 或实测发现的坑点写进「已知坑点」；影响用户行为的同步权威文档。
- 多步任务用 `.codebuddy/plans/<任务名>.md` 记录计划，frontmatter 格式 `name` / `overview` / `todos[].status`。
- 不要把对话记录粘贴进 `CODEBUDDY.md`；只放可复用的结论、约定与指针。

## 修改代码前必读（新增）
- 在修改任何源码文件之前，**必须先读 `handoff.md`（若存在）**，确认当前目标、待办、约束与已知问题。
- 若 `handoff.md` 不存在，先从 `assets/handoff.md` 复制一份到项目根（仅一次），再读取。
- 修改完成后，按「文档同步」回写 `CODEBUDDY.md` 对应小节，并更新 `handoff.md` 的「已完成/待办」。

<TODO：项目特定红线——钉死的依赖版本、禁止删除的兼容代码、不可外泄的私钥文件等>

## 文档同步

<TODO：用户可见改动需同步的文档清单，以及是否需要重新生成（如 User-Manual.*）>

## 验证要求

<TODO：build/test 命令与通过标准，如「dotnet build 必须 0 warning / 0 error」>
