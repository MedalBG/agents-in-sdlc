---
title: "练习 2 - GitHub Copilot 云智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

你可能很少遇到完全没有技术债务的组织。技术债务可能是尚未解决的安全问题、需要更新的旧代码，或因缺少时间而长期搁置的功能请求。GitHub Copilot 云智能体可自主执行更新代码、添加功能等任务。完成后，它会创建草稿 PR，供开发者审查。这样可以将繁琐任务交给智能体处理，加快开发进度，让开发者专注于更重要的工作。

本练习将介绍如何使用 Copilot 云智能体：

- 自定义代码生成环境。
- 确保安全地执行操作。
- 了解明确界定任务范围的重要性。
- 将议题分配给 Copilot。

## 场景

Tailspin Toys 有一些技术债务需要处理。最初负责创建网站的承包商没有为项目编写文档。团队希望先为应用中所有导出的函数添加 TSDoc 文档注释。

此外，设计团队希望改进游戏发现功能。他们想在每款游戏的详情页上展示同类别的其他游戏，以便用户继续浏览。团队目前不要求完善的实现，只需要能用于用户体验验收测试的版本。此需求目前会阻塞工作，但优先级低于其他议题。

这类任务很容易被搁置，适合交给 Copilot 云智能体异步处理。开发者可以先处理其他工作，之后再回来审查 Copilot 的成果。

## 介绍 GitHub Copilot 云智能体

[GitHub Copilot 云智能体](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent)可以像开发者一样在后台执行任务。可以通过多种方式启动任务，例如[将 GitHub 议题分配给 Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)。分配任务后，Copilot 会创建草稿拉取请求来跟踪进度、设置工作环境并开始处理。你可以在会话进行中或完成后查看详情。解决方案准备好等待审查时，Copilot 会在拉取请求中提及你。

## 明确界定任务范围的重要性

GitHub Copilot 并非魔法，使用它并不意味着可以跳过思考。即使看似简单的操作，也可能包含复杂情况，因此请谨慎界定云智能体任务范围，并参阅[云智能体最佳实践][cloud-agent-best-practices]。将它视为 AI 结对程序员：分阶段工作、学习、尝试并不断调整。加入生成式 AI 并不会改变软件开发的基本原则。

## 此存储库中的自定义指令

前面的练习介绍了自定义指令及其作用。若已完成指令练习，你已看到 `.github/copilot-instructions.md` 的效果；否则，现在可以先快速浏览。分配任务前，先只读查看存储库中的指令文件，稍后便能观察其影响。

在 GitHub 网页界面或已启动的 Codespace 中打开以下文件：

- `.github/copilot-instructions.md`：查看 **Code standards** 部分，尤其是 TypeScript 约定和 TSDoc 文档注释要求。
- `.github/instructions/unit-tests.instructions.md`：查看 `applyTo` frontmatter，了解此指令适用于 `**/*.test.ts` 文件。

下一节将 `Code lacks documentation` 议题分配给云智能体时，留意生成的拉取请求是否包含 TSDoc 文档注释、TypeScript 约定和文件注释头。稍后的练习会再次检查这些要求。

## 为 Copilot 云智能体设置开发环境

无论由谁编写代码，通常都需要配置环境并运行设置脚本。将任务分配给 Copilot 时也一样，它会像软件工程师一样完成类似的工作。

云智能体使用 [GitHub Actions][github-actions] 环境执行任务。可以在 `.github/workflows/copilot-setup-steps.yml` 中创建[专用设置工作流][setup-workflow]，在它开始工作前运行，以安装所需开发工具和依赖项。项目已预先配置该工作流，方便实验流程并帮助学习。它会为单体 Astro 应用安装 Node.js、项目依赖、端到端测试所需的浏览器二进制文件，以及已迁移并填充数据的 SQLite 数据库：

```yaml
name: "Copilot Setup Steps"

# Allows you to test the setup steps from your repository's "Actions" tab
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Set the permissions to the lowest permissions possible needed for *your steps*. Copilot will be given its own token for its operations.
    permissions:
      # If you want to clone the repository as part of your setup steps, for example to install dependencies, you'll need the `contents: read` permission. If you don't clone the repository in your setup steps, Copilot will do this for you automatically after the steps complete.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Frontend / app setup - Node.js (the whole app is now Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migrate + seed the local SQLite database so builds and tests have data.
      - name: Set up the database
        run: npm run db:setup
```

该文件与其他 GitHub 工作流类似，但有几点需要注意：

- 其中只有一个名为 `copilot-setup-steps` 的作业。Copilot 开始处理拉取请求之前，GitHub Actions 会运行此作业。
- `workflow_dispatch` 触发器可让你从存储库的 **Actions** 选项卡手动运行工作流。在等待 Copilot 启动工作流之前，可以先测试它是否成功运行。

## 添加文档

大多数项目要么文档过时，要么完全缺少文档。技术债务常常因此长期无人处理，降低工作效率，也让维护代码库或引入新团队成员变得更加困难。Copilot 擅长编写文档，因此很适合将这类议题分配给云智能体。它会在后台生成所需文档，稍后的练习会审查结果。

1. 在新的浏览器选项卡中打开 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue**。
5. 将 **Title** 设置为 `Code lacks documentation`。
6. 将 **Description** 设置为：

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

   ![将议题分配给 Copilot](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。
10. 选择 **Pull Requests** 选项卡。
11. 打开刚创建的拉取请求（PR），其标题类似 `[WIP]: Code lacks documentation`。如果列表中尚未出现新 PR，请稍等片刻并刷新浏览器。
12. 几分钟后，Copilot 应会创建待办列表。

> [!NOTE]
> Copilot 的待办列表可能需要几分钟才会显示在 PR 中。Copilot 正在创建环境（运行前面介绍的工作流）、分析项目并确定解决问题的方案。

13. 查看 Copilot 将要执行的任务。
14. 向下滚动 PR 时间线，查看 Copilot 已开始处理议题的更新。
15. 选择 **View session** 按钮。

   ![Copilot 会话视图](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 可能需要刷新页面才能看到更新后的状态。

16. 查看实时会话，了解 Copilot 如何探索代码、理解当前状态、暂停思考并决定计划，以及创建代码。

整个过程可能需要几分钟。云智能体的主要目标之一是异步执行任务，让你能专注于其他工作。接下来利用这一点：再分配一项任务，然后转而为应用编写新功能。

## 在游戏详情页中添加相关游戏

GitHub Copilot 云智能体的一大优势是可以拆分工作：你处理一组任务时，它可以并行处理另一组。为设计团队添加相关游戏功能可能不会花太久，但这段时间仍可用于其他工作。现在将任务分配给云智能体。

1. 返回 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue**。
5. 将 **Title** 设置为 `Show related games on the game details page`。
6. 将 **Description** 设置为：

   ```markdown
   We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

   ![将议题分配给 Copilot](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

稍后你会在议题的第一条评论旁看到 👀，表示 Copilot 正在处理任务。

![Copilot 使用眼睛表情符号表示正在处理议题](../../../_images/ex4-issue-eyes-emoji.png)

Copilot 正在处理第二项请求。云智能体的工作方式类似软件工程师，因此你不需要一直监控，可以等任务完成后再审查。现在转而了解如何创建和使用自定义智能体。

## 总结和后续步骤

本课介绍了 [GitHub Copilot 云智能体][copilot-agents]。可以将议题分配给云智能体，让它异步工作，以处理技术债务、创建新功能，或协助将代码迁移到其他框架。

你了解了以下概念：

- 自定义代码生成环境。
- 确保安全地执行操作。
- 明确界定任务范围的重要性。
- 将议题分配给 Copilot。

云智能体正在后台工作，现在可以转而创建和使用自定义智能体。[Copilot 云智能体也支持 MCP 服务器][cloud-agent-mcp]，并可使用自定义指令；你已在前面的模块中了解这些内容。

## 资源

- [关于 Copilot 云智能体][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot 云智能体设置工作流最佳实践][cloud-agent-best-practices]

| [下一课：自定义智能体 →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
