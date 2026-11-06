---
title: "练习 1 - 自定义指令（云智能体）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：Copilot 云智能体 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，或 Copilot 需要了解背景信息，应确保它能访问这些上下文。[指令文件][instruction-files]用于提供此类指导，让 Copilot 不仅了解要做什么，也了解如何完成。

本练习将介绍如何：

- 通过存储库自定义指令和路径范围指令文件，向 Copilot 提供项目上下文、编码指南和文档标准。
- 在 `.github/copilot-instructions.md` 中添加新的存储库级标准。

> [!NOTE]
> 与 VS Code 和 CLI 工作流不同，本练习不会运行“更新前后”对比提示。Copilot 云智能体会异步处理 GitHub 议题，因此较难实时并排展示指令的影响。稍后审查云智能体创建的拉取请求时，你会看到这些指令的作用。

## 指令文件

### 场景

Tailspin Toys 制定了一套开发实践指南和要求，包括：

- 数据层始终需要单元测试。
- 用户界面应采用深色模式并具有现代感。
- 应通过 TSDoc 文档注释为代码添加文档。
- 每个文件开头都应添加说明该文件用途的注释块。

通过使用指令文件，确保 Copilot 获取正确的信息，并遵循这些实践完成任务。

### 自定义指令

自定义指令可为 Copilot 提供上下文和偏好，帮助它理解你的编码风格和要求。你可以通过指令引导它生成更相关的建议和代码片段，包括指定编码约定、库以及希望采用的注释类型。可以创建适用于整个存储库的指令，也可以创建适用于特定文件类型或任务的指令。

指令文件有两种：

- `.github/copilot-instructions.md`：每次请求都会发送给 Copilot 的单个文件。此文件应包含适用于大多数 Copilot 聊天或 CLI 请求的项目级信息，例如技术栈、项目概况、最佳实践和全局指南。
- `.github/instructions/*.instructions.md`：可针对特定任务或文件类型创建的文件，例如 TypeScript、Astro、UI 组件或单元测试。

> [!NOTE]
> 在 IDE 中，指令文件只用于 Copilot Chat 的代码生成，不用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot 云智能体都会在生成代码时使用存储库级指令文件以及带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> Copilot 还支持其他标准的指令文件，包括 AGENTS.md 和 CLAUDE.md。

### 管理指令文件的最佳实践

创建指令文件的完整讨论不在本研讨会范围内，但示例项目展示了常见方法：

- 在 `copilot-instructions.md` 中只放置项目级指南，例如项目说明、结构和全局编码标准。
- 使用 `*.instructions.md` 为文件类型（单元测试、Astro 组件、数据层）或特定任务提供专门指令。
- 使用自然语言，保持指南清晰，并提供代码示例。

创建指令文件或使用 AI 都没有唯一正确的方法。可以通过实践找到最适合项目的方法。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应准备一组完善的指令文件。浏览项目文件时，你可能会看到针对 [UI 更新][ui-instructions]和 [Astro][astro-instructions] 等任务的指令。
>
> Copilot 也可以帮助生成指令文件。不同产品界面的操作方式各异，例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`。对应产品的课程会说明具体方法。
>
> 需要模板或起点？可以浏览 [awesome-copilot][awesome-copilot]，其中包含大量指令文件、自定义智能体和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 探索项目中的自定义指令文件

查看存储库自带的指令文件：包括一个核心 `copilot-instructions.md` 文件，以及用于不同任务的 `*.instructions.md` 文件。可以在编辑器或 GitHub 网页界面中打开这些文件。

1. 打开 `.github/copilot-instructions.md`。
2. 查看项目简介，以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。注意 **Code standards** 下的 **GitHub Actions Workflows** 指南。这些内容适用于你与 Copilot 的各种交互。
3. 打开 `.github/instructions` 文件夹，查看 Astro、Drizzle 数据层、测试等相关指令。
4. 打开 `.github/instructions/unit-tests.instructions.md`。顶部的 `applyTo` 字段指定一个相对于存储库根目录的 glob，用于确定指令适用的文件。例如匹配 `**/*.test.ts` 的 TypeScript 测试文件。
5. 查看创建单元测试的专门指令。
6. 打开 `.github/instructions/drizzle.instructions.md` 并查看文件末尾。留意它指向其他指令文件（如 `unit-tests.instructions.md`）和项目现有文件的链接。这样可以将较大的指令集拆分成较小、可复用的部分，并为 Copilot 提供示例。（这些路径相对于指令文件所在位置，而非存储库根目录。）

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分记录了项目编码标准，但尚未要求代码内文档。接下来会添加 TSDoc 注释和文件注释头的要求。

## 添加新的存储库标准

接下来，你只需进行一项编辑：添加项目级规则，要求使用 TSDoc 文档注释和文件级注释头。稍后云智能体处理你分配的议题时会遵循此标准。

先创建分支（云智能体会读取议题目标分支中的指令，但仍要提交自己的编辑）：

1. 在 Codespace 终端中创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。添加编码标准，以改善代码建议：

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分（通常接近第 27 行）。此部分记录了项目编码标准，但尚未包含代码文档要求。
3. 在现有标准下方添加以下 Markdown 内容，要求 Copilot 添加文件注释头和 TSDoc 注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如上一课所示，可以在存储库级 `.github/copilot-instructions.md` 中添加全局指南，也可以使用 `*.instructions.md` 为特定语言、文件类型或任务提供指南。项目级标准（例如文档注释规则）适合放在存储库级文件中。

## 提交、推送并合并指令更新

云智能体会读取议题目标分支中的指令。下一练习中，分配给 Copilot 的议题会从 `main` 分支创建新分支，因此必须先将指令更新合并到 `main`。

1. 暂存并提交：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上打开从 `custom-instructions-cloud` 合并到 `main` 的拉取请求并将其合并。之后，云智能体在本工作流中处理议题时就会读取这些指令。

> [!TIP]
> 如果希望在本研讨会中直接使用 `main`，可以跳过分支步骤并直接提交到 `main`。此处采用分支，是为了模拟真实项目的处理方式。

## 总结和后续步骤

你探索了 Copilot 如何获取此项目的指令文件上下文，并在 `.github/copilot-instructions.md` 中添加了存储库级标准。后续工作流中，你会看到云智能体创建的拉取请求如何体现该标准。

接下来，[将第一个议题分配给 Copilot 云智能体][next-lesson]。

## 资源

- [GitHub Copilot 自定义指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义指令的 5 个技巧][copilot-instructions-five-tips]
- [Awesome Copilot — 指令文件和其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
