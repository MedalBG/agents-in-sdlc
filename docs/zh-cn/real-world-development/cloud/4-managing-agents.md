---
title: "练习 4 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前两项练习中，你让 Copilot 云智能体执行了三项改善用户体验和添加功能的任务。云智能体虽然可以异步自主工作，但监控任务仍然很重要。

你可以使用多种工具管理分配给云智能体的任务，包括 GitHub.com 上的[智能体页面][agents-page]。在这个 mission control 中，你可以查看所有拥有未关闭拉取请求（PR）的智能体任务，查看它们执行的操作，还可以指导正在进行的会话。

本课将介绍如何：

- 使用智能体页面监控云智能体任务。
- 指导正在进行的会话并请求添加功能。

## 场景

分配高对比度模式任务后，团队认为也应添加浅色模式。由于智能体已经在更新网站样式并添加切换功能，因此将此功能一并纳入较为合理。你将指导智能体同时添加浅色模式和高对比度模式。

## 查看 Copilot 云智能体任务

查看当前分配给 Copilot 云智能体的所有任务状态。

1. 打开 [Copilot 智能体页面](https://github.com/copilot/agents)。
2. 查看主窗格和左侧窗格中的任务列表。列表中应包含你分配给 Copilot 的任务，例如：
   - 为代码库补充文档。
   - 生成用于修改游戏的 API。
   - 为网站添加高对比度模式。
3. 选择一个正在运行的任务，查看 Copilot 已执行的操作，例如：
   - 从存储库检出代码。
   - 创建 Copilot 工作环境。
   - 设置 MCP 服务器。
   - 执行完成任务所需的各种步骤。

> [!NOTE]
> 显示的具体步骤会根据 Copilot 当前的工作状态和处理方式而有所不同。

4. 同时查看右侧显示的 PR 窗格，其中可以查看 PR 和已更改的文件。

## 指导云智能体

现在已经了解当前任务，请求 Copilot 在实现高对比度模式的同时添加浅色模式。

1. 选择与添加高对比度模式有关的会话。具体标题会因 Copilot 使用的名称和当前工作状态而异。

   ![Mission control 中的无障碍访问会话](../../../_images/ex6-accessibility-session.png)

2. 等待几分钟，直到会话显示已完成设置并开始工作。此时应能看到类似下方的消息。
3. 在 **Steer active session while Copilot is working** 对话框中，输入以下提示：

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![智能体页面中的云智能体任务，突出显示 Steer active session while Copilot is working 对话框](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示。
5. 注意 Copilot 如何确认提示并将其纳入工作流程。

## 让 Copilot 继续工作

与之前一样，Copilot 会继续处理更新后的任务。收到消息时，它会先完成当前步骤，再将新请求加入工作流程。

这一过程也需要几分钟。等待期间，可以回顾并思考目前学到的内容。

## 总结和后续步骤

本课介绍了 Copilot 智能体页面，它是监控和指导 GitHub Copilot 云智能体任务的中心位置。通过 mission control，你可以跟踪所有活动和已完成的任务、审查正在进行的工作，还可以重定向任务以调整范围或提供更多指导。

你了解了以下概念：

- 使用 mission control 和智能体页面监控云智能体任务。
- 重定向正在进行的会话以请求添加功能。

Copilot 正在完成功能开发。接下来，[迭代 Copilot 创建的拉取请求][next-lesson]。

## 资源

- [Copilot 智能体页面][agents-page]
- [关于自定义智能体][custom-agents]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
