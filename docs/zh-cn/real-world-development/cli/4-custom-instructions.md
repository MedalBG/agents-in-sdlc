---
title: "第 4 课 - 使用自定义指令引导 Copilot"
description: "探索存储库指令、添加文档标准，并将其应用于筛选代码。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，应确保 Copilot 能获取相应指导。[指令文件][instruction-files]不仅说明你想要什么代码，也说明代码应如何组织。现在你已实现筛选功能，接下来将探索 Copilot 使用的指令、添加文档标准，并将其应用于代码。

本课将介绍如何：

- 探索存储库指令和路径范围指令文件如何传递给智能体。
- 更新指令文件，确保遵循编码标准。
- 了解指令文件对代码的影响。

## 场景

Tailspin Toys 有一套开发实践指南和要求，包括：

- 注释应解释意图和不明显的决策，而不是复述代码。
- `db/` 和 `src/lib/` 中导出的函数应使用 TSDoc/JSDoc 说明用途、参数和返回值；如果函数接受可注入的 `db` 参数，也应予以说明。
- 可复用 Astro 组件应说明其 `Props` 契约；相关代码更改时，注释也应保持同步。
- 应保留现有格式设置和 lint 指南。

通过使用指令文件，你可以确保 Copilot 获取正确的信息，并按这些实践完成任务。

## 指令文件

自定义指令可以向 Copilot 提供上下文和偏好，帮助它理解你的编码风格和要求。你可以通过指令引导它生成更相关的建议和代码片段。指令可以指定编码约定、库，以及你希望添加的注释类型。可以创建适用于整个存储库的指令，也可以创建适用于特定文件类型或任务的指令。

指令文件有两种：

- `.github/copilot-instructions.md`：每次请求都会发送给 Copilot 的单个指令文件。此文件应包含适用于该存储库大多数请求的项目信息。
- `.github/instructions/*.instructions.md`：为特定语言、文件类型或任务提供指南。

> [!NOTE]
> 其他指令格式及其支持情况因环境而异。依赖特定格式前，请查看[自定义指令支持参考][custom-instructions-support]。

## 探索项目中的自定义指令文件

初始项目已包含一组指令文件。先探索现有文件，再进行更改，以了解其影响。

1. 返回 Codespace。
2. 在 Codespaces 编辑器（而非终端）中打开 `.github/copilot-instructions.md`。
3. 查看项目简介和编码指南。此文件适用于该存储库中与 Copilot 的所有交互。
4. 打开 `.github/instructions` 文件夹并查看其中的文件，包括 Astro、Drizzle 数据层、测试等相关指令。
5. 打开 `.github/instructions/unit-tests.instructions.md`。留意顶部的 `applyTo` 字段；它指定指令适用的文件 glob。
6. 打开 `.github/instructions/drizzle.instructions.md`，查看它链接到的其他指令文件和项目文件。这种方式可以将大型指令集拆分成较小、可复用的文件，并提供可供 Copilot 参考的示例。

## 更新指令以符合团队指南

现有文件已经很有帮助，但还可以补充一点：修改核心 `copilot-instructions.md`，要求新生成的 TypeScript 添加 TSDoc 注释。保存指令更新后，退出并恢复筛选会话，使 Copilot 在保留功能上下文的同时加载新指南。

1. 在 `.github/copilot-instructions.md` 中找到 **Code formatting guidance** 部分，通常位于第 35 行附近。
2. 在该部分末尾添加以下项目：

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

文件会自动保存。

## 使用更新后的指南

Copilot CLI 会在对话开始时加载存储库指令。保存更新后，退出并恢复筛选对话，让 Copilot 加载新指南，同时保留功能上下文。

1. 如果筛选对话仍打开，请输入 `/exit` 退出。Copilot CLI 会自动保存对话。
2. 在存储库根目录的 `game-filters-cli` 分支中恢复已保存的对话：

   ```bash
   copilot --resume
   ```

   如果出现提示，选择筛选对话，并确认检出的是 `game-filters-cli` 分支。
3. 让 Copilot 应用更新后的指南：

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. 输入 `/diff`，查看已更改的 TypeScript 文件。检查新生成的 TSDoc 注释，并确认其准确解释了代码。

## 总结和后续步骤

你已了解 Copilot CLI 如何获取指令文件中的上下文，并应用新的功能标准。具体来说，你：

- 探索了存储库指令和路径范围指令文件如何传递给智能体。
- 更新了指令文件以确保遵循编码标准。
- 观察了指令文件对代码的影响。

接下来，[自定义并运行 `quality-checks` 可复用技能][next-lesson]，确保以一致的方式运行 lint 和测试。

## 资源

- [为 Copilot CLI 添加自定义指令][instruction-files]
- [自定义指令支持][custom-instructions-support]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [Awesome Copilot — 指令文件和其他资源集合][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
