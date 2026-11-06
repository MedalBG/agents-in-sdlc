---
title: "练习 3 - 使用 Playwright MCP 服务器测试功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：智能体模式][previous-lesson] · [下一课：自定义智能体 →][next-lesson]

你刚刚使用智能体模式构建了筛选功能。打开拉取请求之前，先在浏览器中确认功能确实正常。无需自己逐一操作应用，可以连接 **Playwright MCP 服务器**并让 Copilot 驱动真实浏览器测试，然后发布分支并打开 PR。

本练习将介绍如何：

- 了解 Model Context Protocol (MCP) 以及 MCP 服务器如何扩展 Copilot 工具。
- 将 Playwright MCP 服务器添加到工作区。
- 让 Copilot 使用该服务器在浏览器中手动测试筛选功能。
- 发布分支并为筛选工作创建拉取请求。

## 什么是 Model Context Protocol (MCP)？

智能体模式接入编辑器以外的工具后，功能会大幅增强。Copilot 使用 Model Context Protocol (MCP) 与外部工具和服务通信。

![智能体模式内部工作原理示意图，显示它如何与上下文、LLM 和工具（包括 MCP 服务器及 VS Code 扩展提供的工具）交互](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) 为 AI 智能体提供了与外部工具和服务通信的方式。借助 MCP，AI 智能体可以实时访问最新信息（使用资源），并代表你执行操作（使用工具）。

MCP 服务器负责连接 AI 智能体与外部工具和服务，并管理智能体与外部工具（例如现有 API 或 NPM 包等本地工具）之间的通信。每个 MCP 服务器都提供一组智能体可访问的工具和资源。常用服务器包括：

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**：提供管理 GitHub 存储库的 API，让 AI 智能体可以创建新存储库、更新现有存储库、管理议题和拉取请求。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**：使用 Playwright 提供浏览器自动化能力，让 AI 智能体可以浏览网页、填写表单和选择按钮。

还有许多提供不同工具和资源的 MCP 服务器。GitHub 提供 [MCP 注册表](https://github.com/mcp)，帮助用户发现并参与生态建设。

> [!CAUTION]
> 从安全角度看，应像对待项目中的其他依赖项一样对待 MCP 服务器。使用前请仔细审查其源代码、验证发布者并考虑安全影响。只使用可信的 MCP 服务器，并谨慎授予敏感资源或操作的访问权限。

## 查看 MCP 配置

`.vscode/mcp.json` 文件用于配置 VS Code 工作区可用的 MCP 服务器。

1. 在 Codespace 中打开 `.vscode/mcp.json`。
2. 其中应该已配置 `github` 服务器：

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       }
     }
   }
   ```

项目模板预先配置了 `github` 项，因此 Copilot 能在上一练习中读取议题待办列表。它使用[远程 GitHub MCP 服务器][remote-github-mcp-server]，无需本地安装；VS Code 通过 HTTP 连接，并在 Copilot 首次使用其工具时通过 GitHub 进行身份验证。

## 添加 Playwright MCP 服务器

现在添加第二个服务器。[Playwright MCP 服务器][playwright-mcp-server]为 Copilot 提供可控制的浏览器，用于测试功能。

1. 在 `.vscode/mcp.json` 中添加 `playwright` 项，与 `github` 并列：

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       },
       "playwright": {
         "command": "npx",
         "args": ["@playwright/mcp@latest", "--headless"]
       }
     }
   }
   ```

2. 保存文件。

与远程 `github` 服务器不同，`playwright` 是**本地**服务器：VS Code 会在你的计算机上运行 `npx @playwright/mcp@latest` 来启动它。`--headless` 参数会让 Playwright 在没有可见窗口的情况下运行浏览器，这适用于没有桌面环境的 Codespace。

> [!NOTE]
> Tailspin Toys 项目已在端到端测试中使用 Playwright，因此通常已经安装了 Playwright 浏览器。如果 Copilot 报告缺少浏览器，请让它运行 `npx playwright install chromium`，然后重试。

## 启动并信任服务器

VS Code 会**按需**启动 MCP 服务器，即在 Copilot 首次需要服务器工具时启动，并询问你是否信任服务器。

1. `.vscode/mcp.json` 中的 `playwright` 项上方会出现 **Start** 操作。选择它以立即启动服务器并确认配置正确。
2. 如果 VS Code 询问是否信任服务器，请检查配置并选择信任，以启动服务器。

手动启动是可选步骤。如果跳过，VS Code 会在下一步 Copilot 首次需要工具时启动服务器，并询问是否信任。

## 测试筛选功能

[Playwright MCP 服务器][playwright-mcp-server]为 Copilot 提供了一个可操作的真实浏览器。智能体可以打开页面、导航、应用筛选并读取结果，然后向你总结观察结果，无需离开对话即可快速验证功能行为。

Playwright MCP 服务器通过页面的[无障碍树][playwright-mcp-server]而非屏幕截图工作。智能体会根据结构化、带标签的元素（按钮、链接、列表项等）进行推理，与辅助技术的工作方式类似，因此简单的功能检查也能顺便进行基础的无障碍检查。

连接服务器并启动应用后，要求 Copilot 测试刚完成的筛选功能：

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot 会通过 Playwright MCP 服务器启动浏览器并逐步执行检查，然后报告结果。根据议题中的验收标准审查报告；如果发现问题，可以追问或让 Copilot 修复代码，再创建 PR。

> [!NOTE]
> 测试时应用必须运行在 `http://localhost:4321`。如果已停止开发服务器，请重新启动后再发送提示。Copilot 首次使用 Playwright MCP 服务器时可能需要下载浏览器；如果报告缺少浏览器，请让它运行 `npx playwright install chromium` 后重试。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## 发布分支并创建拉取请求

确认功能正常后，可以创建拉取请求（PR）供团队审查。首先发布 `filtering-vscode` 分支。

1. 打开 Codespace 中的 **Source Control** 面板，审查 Copilot 所做的变更。
2. 选择 **+** 图标暂存变更。
3. 使用 **Sparkle** 按钮生成提交消息。

   ![显示 Copilot 变更的 Source Control 面板](../../../_images/ex3-source-control-changes.png)

4. 选择 **Publish** 将分支推送到存储库。

## 创建拉取请求

可以通过 github.com 或 GitHub 命令行界面（CLI）等方式创建拉取请求。也可以让 Copilot 查找相关议题并创建关联 PR。

1. 打开 Copilot Chat 面板，选择 **New Chat** 开始新会话。
2. 确认智能体下拉列表中选中 **Agent**，以便 Copilot 使用 GitHub 工具。
3. 让 Copilot 创建 PR：

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. 根据需要选择 **Continue**，允许 Copilot 执行获取信息和操作所需的任务。首次使用 GitHub 工具时，系统可能会要求你进行 GitHub 身份验证；按提示授权即可。
5. 观察 Copilot 如何搜索议题、找到正确议题并创建 PR。
6. 选择 Copilot 生成的链接审查 PR，但**暂时不要合并**。

## 总结和后续步骤

恭喜！本练习中，你：

- 了解了 Model Context Protocol (MCP) 以及 MCP 服务器如何扩展 Copilot 工具。
- 将 Playwright MCP 服务器添加到工作区。
- 在发布前让 Copilot 使用服务器在浏览器中手动测试筛选功能。
- 发布分支并为筛选工作创建拉取请求。

你已使用 MCP 服务器测试功能，但 MCP 只是扩展 Copilot 的一种方式。接下来，[创建自定义智能体][next-lesson]，简化无障碍审查等专门任务。

## 资源

- [MCP 是什么，为什么大家都在讨论？][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP 注册表][mcp-registry]
- [VS Code 中的 MCP 服务器][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
