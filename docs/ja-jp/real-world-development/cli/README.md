---
slug: ja-jp/real-world-development/cli
title: "GitHub Copilot CLI"
description: "Copilot CLI のモード、カスタマイズ、MCP ツール、pull request の自動化を学びながら、Tailspin Toys の変更を 2 件構築、検証、リリースします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

**[GitHub Copilot CLI][about-copilot-cli]** を使うと、ターミナルで GitHub Copilot をエージェント型コーディングアシスタントとして利用できます。コードベースを探索し、コードを生成し、コマンドを実行し、外部ツールに接続できます。すべてコマンドラインから行えるため、グラフィカルエディターに切り替えずに作業を続けられます。

このワークショップでは、Tailspin Toys の一連のワークフローに取り組みます。

1. GitHub Codespaces でプロジェクトを準備し、Copilot CLI をインストールして、作業環境を確認します。
2. 星評価の変更に取り組み、ブラウザーでレビューして、最初の pull request (PR) を手動でマージします。
3. フィルター機能の Issue から開始し、Plan モードでアプローチを決め、Autopilot モードで実装して、Interactive モードでレビューします。
4. リポジトリの指示を更新し、フィルター機能に適用します。
5. 既存の `quality-checks` スキルをカスタマイズし、プロジェクトのチェックを実行します。
6. Playwright Model Context Protocol (MCP) server を追加し、ブラウザーでフィルター機能を確認します。
7. quality assurance (QA) カスタムエージェントを作成し、要件、カバレッジ、検証の証拠をレビューします。
8. フィルター機能の変更全体をレビューし、その PR に Agent Merge を使用します。
9. コンテキスト、モデル、共有、任意のクラウドへの委任に役立つスラッシュコマンドを確認します。

ワークショップの焦点を絞るため、作成する PR は 2 件です。星評価の PR の後に、指示の更新、スキルの更新、QA プロファイル、テストを含むフィルター機能の PR を作成します。フィルター機能と品質改善のワークフローでは 1 つの会話とブランチを共有するため、各ツールを確認しながら、それまでの作業を活用できます。

## レッスン

| レッスン | トピック | 説明 |
|----------|-------|-------------|
| [0. 前提条件][ex0] | セットアップ | リポジトリと Codespace を作成します |
| [1. Copilot CLI のインストール][ex1] | インストール | Copilot CLI をインストールして認証し、作業環境を確認します |
| [2. 星評価を追加する: 小さな成果][ex2] | 最初の変更 | 既存の評価と null の場合の表示を追加し、最初の PR をマージします |
| [3. エージェントモード: Plan と Autopilot][ex3] | エージェントモード | Issue から機能を計画し、Autopilot で構築して、Interactive モードでレビューします |
| [4. カスタム指示で Copilot をガイドする][ex4] | コンテキスト | 指示を確認して更新し、絞り込み機能に適用します |
| [5. quality-checks スキルのカスタマイズと使用][ex5] | 繰り返し実行できるチェック | 既存のスキルを確認し、報告形式を変更して実行します |
| [6. Playwright MCP で機能を検証する][ex6] | ブラウザーでの確認 | CLI で MCP を設定し、絞り込みの動作を確認します |
| [7. QA エージェントの作成と使用][ex7] | 要件とカバレッジ | 専門家のプロファイルを作成して選択し、最終検証の証拠を収集します |
| [8. 機能の PR を作成してマージする][ex8] | レビューとマージ | 変更全体をレビューし、PR を作成して Agent Merge を使用します |
| [9. GitHub Copilot CLI のスラッシュコマンド][ex9] | CLI 機能 | コンテキスト、モデル、共有、cloud agent への任意の委任を確認します |
| [10. 振り返りと次のステップ][ex10] | まとめ | ワークフロー、再利用できるカスタマイズ、追加リソースを確認します |
| [オプション: Foundry を組み込む][foundry] | ホスト型エージェント | モデルを準備し、カタログの情報に基づくエージェントをデプロイして、Web サイトに接続します |

## 前提条件

このワークショップに参加する前に、次のものを用意してください。

- [ ] 有効な **Copilot Student、Pro、Pro+、Business、Enterprise** のいずれかのプランが設定された GitHub アカウント
- [ ] リポジトリと Codespace を作成する権限
- [ ] ターミナルまたはコマンドライン操作の基本的な知識

> [!TIP]
> 有料プランを利用していない場合、認証済みの学生は [GitHub Education][student-plan] を通じて GitHub Copilot を無料で利用できます。**Copilot Student** プランには、このワークショップで使用するエージェント、MCP、コードレビュー、Copilot CLI の機能が含まれています。

> [!NOTE]
> Copilot Business または Copilot Enterprise を使用している場合、管理者が Copilot CLI を有効にしていることを確認してください。

## はじめる

**[演習 0「前提条件」から始める →][ex0]**

[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[student-plan]: https://github.com/education/students
[ex0]: 0-prerequisites/
[ex1]: 1-install-copilot-cli/
[ex2]: 2-add-star-rating/
[ex3]: 3-agent-modes/
[ex4]: 4-custom-instructions/
[ex5]: 5-agent-skills/
[ex6]: 6-mcp-playwright/
[ex7]: 7-qa-agent/
[ex8]: 8-create-pull-request/
[ex9]: 9-cli-power-tools/
[ex10]: 10-review/
[foundry]: 8-foundry-agent/
