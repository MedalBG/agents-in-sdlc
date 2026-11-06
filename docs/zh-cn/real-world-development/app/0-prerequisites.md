---
title: "第 0 课 - 先决条件"
description: "为 GitHub Copilot app 课程做好准备：为 Tailspin Toys 项目安装 Node.js，并通过模板创建自己的存储库副本。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

GitHub Copilot app 是一款桌面应用，也是你使用 Copilot 和 GitHub 的中心入口。它可以快速访问议题和拉取请求，并使用 GitHub Copilot 辅助开发。本研讨会将在本地使用基于 Astro 构建的 Tailspin Toys 应用和 GitHub Copilot app。开始前，先确认本机已安装 Node.js，然后再安装 Copilot app。

本课将介绍如何：

- 安装 Node.js，以便在本机运行项目测试。
- 根据模板创建自己的 Tailspin Toys 项目副本。

## 安装 Node.js

多节课程会要求智能体构建功能，并在本地运行 Tailspin Toys 测试套件。项目只需要一个运行时：[**Node.js**][nodejs]。请安装当前的 **LTS** 版本。

所有平台上最简单的方式都是使用官方安装程序：

1. 在操作系统中使用 Windows Terminal、macOS 终端或常用工具打开终端窗口。
2. 运行以下命令，检查已安装的 Node.js 和 npm 版本：

    ```shell
    node --version
    npm --version
    ```

3. 如果两个命令都成功运行，且 Node.js 满足项目 README 和 `package.json` 中的要求，可以跳到下一节。

> [!TIP]
> 仅当尚未安装 Node.js 或需要更新时，才需要完成以下步骤。

4. 打开 [Node.js 下载页面][node-download]。
5. 下载适用于当前操作系统的 **LTS** 版本。
6. 运行安装程序并接受默认设置。在 Windows 上，保留选中的 **Add to PATH** 选项。
7. 安装完成后，打开新的终端窗口。
8. 在新终端窗口中运行以下命令确认安装：

    ```bash
    node --version
    npm --version
    ```

9. 应显示已安装的 Node.js 和 npm 版本。

> [!IMPORTANT]
> Copilot app 智能体也必须能够运行 `node --version` 和 `npm --version`。常用终端中运行成功并不保证应用也能访问这些命令。安装 Node.js 或更改 PATH 后，请完全退出并重新打开应用。如果会话中仍有任一命令失败，请先参阅[会话环境故障排除步骤][session-environment]。

> [!IMPORTANT]
> 每个工作树还需要项目依赖项和用于 E2E 检查的 Playwright Chromium。准备工作树时，请遵循 Tailspin Toys 存储库的 README，并在批准前审查所有安装请求。

## 设置实验室存储库

你将在自己的 Tailspin Toys 项目副本中工作。现在根据[模板存储库][template-repository]创建副本。新存储库包含实验所需的全部文件，安装应用时会将它连接到应用。

1. 在新的浏览器窗口中打开本实验的 GitHub 存储库：`https://github.com/github-samples/tailspin-toys`。
2. 在实验室存储库页面选择 **Use this template**，再选择 **Create a new repository**，创建自己的存储库副本。

    ![展开 Use this template 菜单并选中 Create a new repository](../../../_images/app-0-use-template.png)

3. 如果你正在参加由 GitHub 或 Microsoft 举办的活动，请遵循导师的说明。否则，可以在你拥有 GitHub Copilot 使用权限的组织中创建新存储库。

    ![Create a new repository 表单，其中 github-samples/tailspin-toys 被设为模板，且已填写存储库名称](../../../_images/app-0-create-repository.png)

4. 记下创建的存储库路径（**organization-or-user-name/repository-name**），稍后会在实验中用到。

> [!NOTE]
> 从模板创建存储库时，系统会自动创建一组 GitHub 议题作为待办事项。整个研讨会都会围绕这些议题开展，无需自行创建。

请使用研讨会模板的新副本。其中包含存储库指令、应用代码、测试、quality-checks 技能和现有画布扩展。你将在研讨会中自定义该技能，并创建 QA 智能体。如果使用旧副本，请向导师确认其中包含所需文件。

## 总结和后续步骤

环境已准备就绪。本课中，你：

- 安装了 Node.js，以便在本机构建和测试项目。
- 根据模板创建了自己的 Tailspin Toys 存储库副本。

接下来，[安装 GitHub Copilot app][next-lesson]、连接刚创建的存储库并熟悉工作区。

## 资源

- [下载 Node.js][node-download]
- [从模板创建存储库][template-repository]
- [关于 GitHub Copilot app][about-copilot-app]

[next-lesson]: ../1-install-copilot-app/
[nodejs]: https://nodejs.org/
[node-download]: https://nodejs.org/en/download
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[session-environment]: ../3-agent-modes/#从议题启动会话
