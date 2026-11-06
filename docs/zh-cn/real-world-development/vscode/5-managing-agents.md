---
title: "练习 5 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义智能体][previous-lesson] |
|:--|

Copilot 在智能体模式下会自主探索代码库、提出变更、编辑文件并运行命令。由于这些工作会在本地实时进行，VS Code 中的 Copilot Chat 会显示每项工具调用和文件编辑。你可以查看内联差异、接受或拒绝单项变更，并通过后续提示引导会话，继续完善或扩展工作，而无需离开编辑器。

本练习将介绍如何：

- 实时监控无障碍智能体的工作。
- 审查并接受建议的文件编辑。
- 在会话进行中指导智能体，添加浅色模式切换。

## 场景

[练习 4][previous-lesson]中使用的无障碍自定义智能体正在实现高对比度模式。开发过程中，设计团队又提出新要求：为觉得默认主题太暗的用户添加浅色模式切换。与其从头开始新会话，不如指导现有会话扩展工作。

## 监控 Copilot 的进度

Copilot Chat 视图会显示智能体执行的每个步骤，包括探索的文件、运行的命令和提出的编辑。每项文件编辑都会以内联差异和 **Keep** / **Undo** 控件显示。

1. 返回练习 4 中的 Copilot Chat 视图。
2. 智能体工作期间，滚动聊天内容查看工具调用，包括文件读取、文件编辑和请求运行的终端命令。
3. 智能体提出文件编辑时，差异会以内联方式显示：
   - 选择 **Keep** 接受变更。
   - 选择 **Undo** 撤销变更，让智能体重新尝试。
4. 也可以打开 Chat 顶部的 **Files changed** 指示器，查看该会话修改的文件。

> [!NOTE]
> 如果工具调用请求权限（例如运行终端命令），智能体会暂停并显示 **Allow** 提示。只批准你认可的命令。

5. 等待智能体完成高对比度模式，然后查看它总结所做变更的最终消息。

## 指导智能体添加浅色模式

高对比度模式已就绪，现在扩展同一对话，添加浅色模式切换。由于继续使用同一会话，智能体会保留先前工作中的上下文，包括修改的组件、样式方法和本地存储模式。

1. 在 Copilot Chat 输入框中发送以下后续提示：

   ```plaintext
   Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
   ```

2. 智能体会继续工作，探索已做的变更并提出浅色模式所需的修改。
3. 按照高对比度功能的方式，逐项审查新编辑。使用 **Keep** 或 **Undo** 处理每个差异。
4. 如果智能体的方向不符合预期，例如试图以不喜欢的方式重构切换 UI，可以选择 Chat 输入框旁的 **Stop** 按钮停止，再发送澄清提示。

> [!TIP]
> 指导正在进行的会话比从头开始更快。可以通过后续提示要求调整（“简化 CSS，改用 CSS 变量，避免重复选择器”）、纠正方向（“不要添加第三个开关，将浅色模式整合到现有设置面板”）或扩展范围（“同时为两个开关添加键盘快捷键”）。

## 审查工作集

提交工作前，快速检查智能体修改的所有内容。

1. 在 VS Code 中打开 **Source Control** 视图。
2. 审查所有已更改文件。应该会看到 Astro 组件、样式和相关测试的更新。
3. 打开一些已更改文件并查看差异。确认实现体现了自定义智能体的无障碍模式，包括 ARIA 属性、键盘导航、语义化 HTML 和本地存储。
4. 确认满意后，在 Source Control 面板中暂存并提交变更。下一课会发布这些变更。

## 总结和后续步骤

你使用 Copilot Chat 智能体模式监控并指导正在进行的会话，审查每项文件编辑、接受或拒绝变更，并在保留上下文的情况下发送后续提示扩展工作。

本课介绍了：

- 实时监控智能体工作。
- 接受或拒绝单项文件编辑。
- 使用后续提示指导进行中的会话。

在[下一课][next-lesson]中，你将以拉取请求的形式发布无障碍功能，并审查本实验中本地自定义智能体生成的变更。

## 资源

- [VS Code 中的 Copilot Chat][copilot-chat-vscode]
- [使用智能体模式][agent-mode]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代改进 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
