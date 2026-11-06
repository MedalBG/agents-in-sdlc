---
title: "第 2 课 - 添加星级评分：快速上手"
description: "在 GitHub Copilot app 中启动首个智能体会话，对游戏卡片进行小幅修改，并通过第一个拉取请求合并变更。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

你已熟悉工作区并尝试了快速聊天，现在开始一个**智能体会话**，对项目进行第一次修改。这项修改很小：游戏数据中已有星级评分，但主页的游戏卡片尚未显示。让智能体显示评分、审查变更，然后通过第一个拉取请求合并。

本课将介绍如何：

- 启动智能体会话并了解会话结构。
- 让智能体对项目进行小幅、范围明确的修改。
- 在工作区差异视图中审查变更。
- 在本地运行应用，并在浏览器中确认变更。
- 创建并合并第一个拉取请求。

## 场景

Tailspin Toys 中的每款游戏都有星级评分，详情页已经显示该评分。但主页的游戏卡片只显示标题、类别、发行商和描述。作为热身，让智能体在每张卡片上显示现有评分。这是一个范围明确的小改动，适合首次会话。

## 会话的组成

**会话**是与智能体的对话，在独立工作区中运行。每个会话都有**专用的 Git 工作树和分支**，因此可以同时运行多个会话，例如一个添加功能，另一个修复 bug，而不会发生变更冲突。侧边栏按存储库分组显示会话；选择任一会话即可切换。

会话中包含三类内容：与智能体的**对话**、智能体探索和编辑文件时的**工具活动**，以及显示差异的**已更改文件**列表。

## 启动会话并请求变更

现在启动新会话，探索项目并实现功能。[上一课][prior-lesson]中，你已从 GitHub 存储库添加项目。接下来为该存储库创建新会话并请求变更。

1. 返回（或打开）GitHub Copilot app。
2. 选择侧边栏顶部的 **+** 开始会话。
3. 选择存储库 `tailspin-toys`。
4. 在提示框下方选择 **new working tree** 和 **Interactive** 模式。使用以下提示请求变更：

    ```plaintext
    Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
    ```

5. 按 <kbd>Enter</kbd> 将提示发送给 Copilot。

Copilot app 会先创建新的工作树，也就是项目的隔离副本。随后它会探索项目、定位需要更新的文件，并创建实现功能所需的代码。现在你已使用 Copilot app 添加了新功能。

## 审查差异

所有 AI 生成的变更都应在合并前审查，即使变更很小也不例外。现在直接在 Copilot app 中检查变更。

1. 在应用右上角选择 **Toggle review panel**。在打开的面板中选择 **Changes**，查看已更改文件及其差异。

    ![GitHub Copilot app 顶部工具栏，箭头指向 Create PR 右侧的 Toggle review panel 按钮](../../../_images/app-2-review-panel.png)

2. `GameCard.astro` 是显示游戏详情的核心文件，其中应该有新增代码。代码大致如下：评分存在时显示评分，`starRating` 为 `null` 时显示 `No rating yet`：

   ```astro
   {game.starRating !== null ? (
       <span class="text-xs font-medium px-2.5 py-0.5 rounded bg-amber-900/60 text-amber-300" data-testid="game-rating">
           ★ {game.starRating} / 5
       </span>
   ) : (
       <span class="text-xs font-medium text-slate-500" data-testid="game-rating-empty">
           No rating yet
       </span>
   )}
   ```

> [!NOTE]
> 与所有生成式 AI 工具一样，Copilot 的输出具有概率性，并非确定性的，因此实际代码可能与示例不同，但大致相似。

## 检查变更

打开浏览器前，先审查智能体自动检查的结果。确认测试覆盖数值类型的 `starRating` 和 `null` 回退状态。缺少先决条件或跳过检查不算通过；批准任何安装请求前先审查。

不要只看代码就假定它能正常工作。让 Copilot 启动网站，并在浏览器画布中打开，以便检查更新后的 UI。

> [!TIP]
> 画布是 Copilot app 内的交互式小组件。稍后会探索自定义画布，甚至创建自己的画布；现在先使用内置的浏览器画布。

1. 使用以下提示，让 Copilot 启动应用并在浏览器画布中打开页面：

    ```plaintext
    Start the app and open it in the browser canvas.
    ```

2. 稍等片刻，应用将启动，Copilot app 内会打开浏览器窗口。
3. 确认已评分的游戏卡片显示满分为五分的评分值。
4. 完成后，使用以下提示让 Copilot 停止为此会话启动的开发服务器，并关闭浏览器画布：

    ```plaintext
    Stop the dev server and close the browser canvas.
    ```

## 打开并合并第一个拉取请求

功能已完成，现在可以创建拉取请求 (PR)，将新代码合并到现有代码库。

1. 选择右上角的 **Create PR**。
2. 如果系统提示，请选择 **Sign in with your browser**，并按提示完成身份验证。
3. 如果 Copilot 说明存储库策略要求创建 PR 前必须关联议题，请确认它应遵循这一流程。
4. 等待 Copilot 创建 PR。
5. 选择聊天上方的 **PR** 气泡，在审查面板中打开 PR。可根据需要在这里审查 PR。
6. 准备就绪后，选择 **Ready to merge**。
7. 在新对话框中选择 **Merge pull request**，合并 PR。

## 总结和后续步骤

恭喜！你已使用 GitHub Copilot app 发布了第一个变更。具体来说，你：

- 启动了智能体会话并了解会话结构。
- 指示智能体对游戏卡片进行小幅、范围明确的修改。
- 在工作区差异视图中审查了变更。
- 在本地运行应用，并在浏览器中确认星级评分。
- 创建了 PR 1、审查其检查结果并明确执行合并。

接下来，从筛选议题开始，使用 [Plan 和 Autopilot 模式][next-lesson]构建更大的功能。

## 资源

- [在 GitHub Copilot app 中使用智能体会话][agent-sessions]
- [关于 GitHub Copilot app][about-copilot-app]
- [使用 GitHub Copilot app 管理议题和拉取请求][managing-issues-prs]

[prior-lesson]: ../1-install-copilot-app/#安装并配置-github-copilot-app
[next-lesson]: ../3-agent-modes/
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
