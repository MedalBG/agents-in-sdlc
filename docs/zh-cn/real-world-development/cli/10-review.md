---
title: "第 10 课 - 总结与后续步骤"
description: "回顾 Copilot CLI 工作流、两个拉取请求、可复用的自定义项和更多资源。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

你已在 Tailspin Toys 的连续工作流中使用 GitHub Copilot CLI。你：

- 准备了 Codespace、安装了 Copilot CLI、探索了项目，并找到了预置的筛选议题。
- 添加了星级评分，在转发的浏览器中检查结果，并手动合并了第一个拉取请求（PR）。
- 从筛选议题开始，使用 Plan 模式确定方案、使用 Autopilot 模式构建功能，并在 Interactive 模式下审查。
- 使用自定义指令引导智能体，自定义现有的 `quality-checks` 技能，并使用它运行单元测试、lint 和类型检查。
- 添加了 Playwright Model Context Protocol (MCP) 服务器，并用它在真实浏览器中探索筛选功能。
- 创建并选择质量保证（QA）自定义智能体，评估需求、覆盖率、技能结果和浏览器证据。
- 审查完整的筛选变更，并授权 Agent Merge 处理筛选 PR。
- 探索了用于上下文、模型、共享和可选云委派的斜杠命令。

## 交付的内容

本研讨会包含两个 PR 阶段，每个阶段都基于更新后的 `main` 创建独立分支：

1. **星级评分**：在游戏卡片上显示现有的 `starRating` 和明确的未评分状态。
2. **筛选和质量工作流**：实现筛选、更新指令并将其应用于功能、自定义 `quality-checks` 报告、创建 QA 配置，并加入相关测试。

从规划筛选功能到打开 PR，你一直使用同一对话和分支。为了简化研讨会流程，我们将这些工作合并为一个 PR。

## 不同类型的验证

你通过自动化测试、浏览器手动检查和 Copilot 经 MCP 进行的浏览器探索检查了代码。`quality-checks` 技能运行单元测试、lint 和类型检查，并按你新设定的格式报告结果。QA 在 PR 创建前综合检查了这些结果、需求和测试覆盖率。

新增测试应解决实际覆盖缺口；QA 运行后无需添加测试也可能是正确结果。授权合并前，请审查代码和证据；变更后应更新受影响的证据。

## 最佳实践

提供给 Copilot 的上下文和工具会影响它的工作。在本研讨会中，你更新了指令、自定义了技能、创建了 QA 配置并配置了 MCP 服务器。可以在多个对话中复用这些自定义项，并根据团队需求调整。指令用于设定标准，技能用于描述可重复任务，自定义智能体定义专用角色，MCP 服务器则连接外部工具。请审查实际变更和工具结果，而不只是智能体的总结。

根据任务选择**模式和模型**。使用 **Plan** 在构建前考虑实施方案；使用 **Interactive** 处理需要密切参与的重点变更；使用 **Autopilot** 执行范围明确的任务。常规编辑可选择速度较快的模型，复杂工作则可选择能力更强的模型。

上下文与基础设施同样重要。清晰说明要构建什么、为什么构建以及如何构建，会显著影响输出。

## 更多可探索内容

你已完成核心工作流，还可以了解其他 CLI 功能：

- `/review`：让代码审查智能体分析变更。
- `/rubber-duck`：讨论问题并获取另一种看法。
- `/fleet`：并行协调彼此独立的子任务。
- `/worktree`：隔离单独任务。
- `/delegate`：将任务发送给 Copilot 云智能体。

## 后续步骤

要熟练使用工具，最好的办法就是持续使用。可以将它用于生产代码、兴趣项目，或你多年来一直想构建却尚未动手的小应用。与团队分享经验，也向团队成员学习。最后，继续探索文档。

如果要从终端扩展 Tailspin Toys，可以继续学习可选的 [Foundry Backer Concierge 系列][foundry]。若想比较不同环境，可以探索 [VS Code 研讨会][vscode]、[GitHub Copilot app 研讨会][app]或 [Copilot 云智能体研讨会][cloud]。

## 资源

- [关于 GitHub Copilot CLI][about-cli]
- [Copilot CLI 命令参考][cli-reference]
- [自定义 Copilot CLI][customize-cli]
- [使用 Copilot CLI 管理拉取请求][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
