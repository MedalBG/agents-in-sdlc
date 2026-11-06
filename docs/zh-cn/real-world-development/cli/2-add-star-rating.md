---
title: "第 2 课 - 添加星级评分：快速上手"
description: "使用 Copilot CLI 修改游戏卡片，在转发的浏览器中检查结果，并将变更作为第一个拉取请求合并。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

安装 Copilot CLI 并尝试过对话后，现在来为项目做第一个小改动。游戏数据中已有星级评分，但主页的游戏卡片尚未显示。让智能体显示已有评分、审查变更，然后将其作为第一个拉取请求合并。

本课将介绍如何：

- 在功能分支上开始专注于单一任务的 Copilot 对话。
- 让智能体对项目进行小幅修改。
- 使用 `/diff` 审查变更。
- 运行应用，并在转发的浏览器中确认变更。
- 创建并合并第一个拉取请求。

## 场景

Tailspin Toys 中的每款游戏都有星级评分，详情页已经显示该评分。但主页的游戏卡片只显示标题、类别、发行商和描述。作为热身练习，让智能体在每张卡片上显示已有评分。这是一个范围明确的小改动，适合首次使用会话。

## 对话的组成

**对话**是你在 Copilot CLI 中处理任务的地方。与 Copilot app 不同，普通 CLI 对话使用终端当前检出的存储库和 Git 分支，而不是创建专用工作树。保存的对话可以让你稍后返回同一讨论，而文件和分支仍是磁盘上的普通 Git 状态。

对话中有三类内容：你的提示和智能体回复、智能体探索和编辑文件时的工具活动，以及可通过 `/diff` 检查的变更。

## 开始对话并请求变更

开始实现功能前，先启动新对话。

1. 返回 Codespace。
2. 如果终端尚未打开，按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）。
3. 如果 Copilot 尚未运行，执行：

   ```bash
   copilot --yolo
   ```

4. 输入斜杠命令 `/new` 并按 <kbd>Enter</kbd>，确保开始新会话。
5. 使用以下提示请求变更：

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

Copilot 会探索项目、定位显示游戏详情的文件并生成所需代码。现在你已使用 Copilot CLI 添加了一项功能。

## 审查差异

所有 AI 生成的变更都应在合并前审查，即使变更很小也不例外。现在可以直接在 Copilot CLI 中检查变更。

1. 输入 `/diff` 并检查每个已更改文件。
2. 确认游戏卡片在有评分时显示数字评分，在 `starRating` 为 `null` 时显示 `No rating yet`。
3. 确认测试覆盖这两种状态。
4. 查看 Copilot 运行的检查结果，并要求它修复失败项。
5. 审查完毕后，按 <kbd>Esc</kbd> 退出差异界面。

> [!NOTE]
> 与所有生成式 AI 工具一样，Copilot 的输出具有概率性，并非确定性的，因此你的代码可能与示例不同。请检查行为，不必追求完全相同的实现。

## 检查变更

不要只看代码就假定它能正常工作。让 Copilot 启动网站，然后在 Codespaces 转发的浏览器中查看更新后的界面。

1. 让 Copilot 启动应用：

   ```plaintext
   Start the app so I can inspect the star-rating change in my browser. Tell me the URL and leave the server running.
   ```

2. 当 Codespaces 报告端口 `4321` 可用时，选择 **Open in Browser**。
3. 确认游戏卡片显示了满分为五分的评分。
4. 返回 Copilot，并让它停止所启动的服务器：

   ```plaintext
   Stop the development server you started.
   ```

## 打开并合并第一个拉取请求

功能已完成，现在可以创建拉取请求（PR），将新代码合并到项目中。

1. 让默认智能体提交变更：

   ```plaintext
   Commit the reviewed star-rating changes with an appropriate commit message.
   ```

2. 输入 `/pr create`。Copilot CLI 创建 PR 时会推送现有提交，并显示 PR URL。
3. 按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），然后选择 Copilot CLI 显示的 URL，以打开 PR。
4. 审查变更文件和检查结果。
5. 准备就绪后，选择 **Merge pull request**，然后确认合并。
6. 返回 Codespace，并使用 `/exit` 退出 Copilot CLI。
7. 更新本地 `main`：

   ```bash
   git checkout main
   git pull
   ```

## 总结和后续步骤

恭喜！你已使用 GitHub Copilot CLI 发布了第一个变更。具体来说，你：

- 在功能分支上开始了专注于单一任务的 Copilot 对话。
- 指示智能体修改游戏卡片。
- 使用 `/diff` 审查变更。
- 运行应用，并在转发的浏览器中确认星级评分。
- 创建并合并了第一个拉取请求。

接下来，从筛选议题开始，使用 [Plan 和 Autopilot 模式][next-lesson]构建更大的功能。

## 资源

- [关于 GitHub Copilot CLI][about-copilot-cli]
- [Copilot CLI 命令参考][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
