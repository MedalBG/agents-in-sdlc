---
title: "练习 1 - 自定义指令（VS Code）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：智能体模式 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，或 Copilot 需要了解背景信息，应确保它能访问这些上下文。提供代码生成背景信息的主要方式之一是使用[指令文件][instruction-files]。

本练习将介绍如何：

- 通过存储库自定义指令和路径范围指令文件，向 Copilot 提供项目上下文、编码指南和文档标准。
- 在现有指令下生成筛选功能的第一个数据切片（发行商辅助函数）。
- 在 `.github/copilot-instructions.md` 中添加新的存储库级标准。
- 重复相同提示，观察生成代码如何遵循新标准。
- 提交指令更新和筛选功能切片到 `main`，供下一练习使用。

> [!CAUTION]
> 生成的代码可能不完全符合设定的标准。Copilot 的输出具有非确定性。本练习的目标是观察更新指令前后行为变化的趋势，而不是逐字匹配结果。

## 指令文件

### 场景

Tailspin Toys 有一套开发实践指南和要求，包括：

- 数据层始终需要单元测试。
- 用户界面应采用深色模式并具有现代感。
- 应通过 TSDoc 文档注释为代码添加文档。
- 每个文件开头都应添加说明该文件用途的注释块。

通过使用指令文件，确保 Copilot 获取正确的信息，并遵循这些实践完成任务。

### 自定义指令

自定义指令可为 Copilot 提供上下文和偏好，帮助它理解你的编码风格和要求。你可以通过指令指定编码约定、库以及希望采用的注释类型。可以创建适用于整个存储库的指令，也可以创建适用于特定文件类型或任务的指令。

指令文件有两种：

- `.github/copilot-instructions.md`：每次请求都会发送给 Copilot 的单个文件。此文件应包含适用于大多数请求的项目级信息，例如技术栈、项目概况、最佳实践和全局指南。
- `.github/instructions/*.instructions.md`：可针对特定任务或文件类型创建的文件，例如 TypeScript、Astro、UI 组件或单元测试。

> [!NOTE]
> 在 IDE 中，指令文件只用于 Copilot Chat 的代码生成，不用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot 云智能体都会在生成代码时使用存储库级指令文件以及带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> Copilot 还支持其他标准的指令文件，包括 AGENTS.md 和 CLAUDE.md。

### 管理指令文件的最佳实践

示例项目展示了常见方法：

- 在 `copilot-instructions.md` 中只放置项目级指南，例如项目说明、结构和全局编码标准。
- 使用 `*.instructions.md` 为文件类型（单元测试、Astro 组件、数据层）或特定任务提供专门指令。
- 使用自然语言，保持指南清晰，并提供代码示例。

创建指令文件或使用 AI 都没有唯一正确的方法。可以通过实践找到适合项目的方法。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应准备一组完善的指令文件。项目中包含针对 [UI 更新][ui-instructions]和 [Astro][astro-instructions] 等任务的指令。
>
> Copilot 也可以帮助生成指令文件。不同产品界面的操作方式各异，例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`。对应产品的课程会说明具体方法。
>
> 需要模板或起点？可以浏览 [awesome-copilot][awesome-copilot]，其中包含大量指令文件、自定义智能体和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 探索项目中的自定义指令文件

查看存储库自带的指令文件：包括核心 `copilot-instructions.md` 文件，以及用于不同任务的 `*.instructions.md` 文件。可以在编辑器或 GitHub 网页界面中打开这些文件。

1. 打开 `.github/copilot-instructions.md`。
2. 查看项目简介和 **Agent notes**、**Code standards**、**Scripts**、**Repository Structure** 等部分。注意 **Code standards** 下的 **GitHub Actions Workflows** 指南。
3. 打开 `.github/instructions` 文件夹，查看 Astro、Drizzle 数据层、测试等相关指令。
4. 打开 `.github/instructions/unit-tests.instructions.md`。顶部的 `applyTo` 字段指定一个相对于存储库根目录的 glob；例如匹配 `**/*.test.ts` 的 TypeScript 测试文件。
5. 查看创建单元测试的专门指令。
6. 打开 `.github/instructions/drizzle.instructions.md` 并查看文件末尾。留意它指向其他指令文件（如 `unit-tests.instructions.md`）和项目现有文件的链接。这可以拆分较大的指令集并提供示例（路径相对于指令文件，而非存储库根目录）。

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分记录了项目编码标准，但尚未要求代码内文档。接下来会添加 TSDoc 注释和文件注释头的要求。

## 为变更创建分支

遵循最佳实践，为变更创建分支。

1. 返回上一练习中打开的 Codespace。
2. 按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）打开终端。
3. 创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions
   ```

## 更新指令前先使用 Copilot Chat

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 Codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始全新对话。模式和模型的选择因练习而异；每个步骤会在需要时说明。

先在现有指令下发送提示，然后更新指令并重复同一提示，以观察自定义指令的影响。

1. 关闭之前练习留下的编辑器选项卡，让 Copilot 只获取所需上下文。
2. 打开 `src/lib/publishers.ts`，让 Copilot 知道辅助函数应放在哪里。
3. 在 Chat 视图的智能体下拉列表中选择 **Agent**，以便 Copilot 修改文件。

   ![Chat 视图中的智能体选择器](../../../_images/shared-chat-mode-selector.png)

4. 发送以下提示：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot 会探索项目并更新代码，通常会同时修改辅助函数文件和测试。
6. 注意生成的辅助函数是类型化函数，以 `db` 客户端为第一个参数，并返回类型化发行商数组。这来自适用于 `src/lib/*.ts` 的 `.github/instructions/drizzle.instructions.md` 数据层约定。
7. 注意生成的代码**缺少** TSDoc 文档注释和文件级注释头。

> [!CAUTION]
> Copilot 的输出具有非确定性，因此即使没有指令，它也可能添加文档注释。若发生这种情况也没关系，本练习仍要观察更新指令后的一致性改进。

## 添加新的存储库标准

`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。添加编码标准，以改善代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分，通常接近第 27 行。注意它记录了项目编码标准，但没有代码文档要求。
3. 在现有标准下方添加以下 Markdown 内容，要求 Copilot 添加文件注释头和 TSDoc 注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如上一课所示，可以在存储库级 `.github/copilot-instructions.md` 中添加全局指南，也可以使用 `*.instructions.md` 为特定语言、文件类型或任务提供指南。项目级标准适合放在存储库级文件中。

## 重复提示并观察变化

1. 返回 Copilot Chat，选择 **New Chat** 清除上下文。
2. 再次打开 `src/lib/publishers.ts`，让 Copilot 关注正确文件。
3. 发送之前的**相同提示**：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 注意生成的文件现在以类似以下内容的注释块开头：

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 注意生成的函数现在包含类似以下内容的 TSDoc 注释：

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

你已引导 Copilot 遵循新的项目标准，并将其应用于后续练习会继续使用的代码。

## 提交并推送指令更新

指令文件与其他项目资产一样，通过源代码管理。提交并推送分支：

1. 按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）打开新终端。
2. 在终端中运行以下命令，确认指令和辅助函数变更都已存在：

   ```bash
   git status
   ```

3. 暂存并提交指令更新，以及 Copilot 为辅助函数基础所做的所有文件变更：

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # If git status shows additional supporting updates (for example tests), add those files too.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. 将分支推送到存储库：

   ```bash
   git push -u origin custom-instructions
   ```

## 创建并合并拉取请求

分支已推送，现在创建拉取请求，并关联待办列表中的文档标准议题。Copilot 可以查找议题、创建关联 PR 并合并。

> [!NOTE]
> 这是 Copilot 首次代表你操作 GitHub。项目模板已经配置好 GitHub 工具，无需额外设置。Copilot 首次使用工具时，VS Code 可能会提示你登录 GitHub；按提示授权即可。后续练习会介绍这种连接如何工作（Model Context Protocol）。

1. 在 Codespace 中打开 Copilot Chat。
2. 按 <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd>（Windows/Linux）打开 Copilot Chat 视图，并确保智能体选择器中选中 **Agent**。
3. 让 Copilot 查找与更新指令文件有关的议题，并从当前分支创建 PR，说明指令更新和新的发行商辅助函数基础：

   ```text
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot 会查找议题并创建 PR。
5. Copilot 请求代表你执行 GitHub 操作时，请先审查命令，再根据需要选择 **Allow**。
6. PR 创建后，使用以下提示请求 Copilot 合并 PR 并让本地分支返回 main：

   ```text
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Copilot 请求代表你执行 GitHub 操作和 shell 命令时，请先审查命令，再根据需要选择 **Allow**。

你已在 GitHub Copilot 的帮助下创建并合并拉取请求。

## 总结和后续步骤

你探索了 Copilot 如何获取指令文件中的上下文，并使用 VS Code 中的 Copilot Chat：

- 发送代码生成提示，观察现有指令下 Copilot 的输出。
- 在 `.github/copilot-instructions.md` 中添加新的存储库级标准。
- 重复相同提示，观察生成代码如何遵循新标准。
- 将指令更新和辅助函数基础提交到 `main`，供下一练习使用。

接下来，在[智能体模式][next-lesson]中运用这些指令，让 Copilot 跨代码库添加新功能。

## 资源

- [GitHub Copilot 自定义指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义指令的 5 个技巧][copilot-instructions-five-tips]
- [GitHub Copilot 个人自定义指令][personal-instructions]
- [Awesome Copilot — 指令文件和其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
