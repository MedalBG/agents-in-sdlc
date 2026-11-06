---
title: "レッスン 7 - QA エージェントの作成と使用"
description: "Issue の要件、quality-checks スキル、Playwright MCP を組み合わせる QA カスタムエージェントを作成します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

`quality-checks` スキルを使って自動チェックを実行し、Playwright MCP を使ってブラウザーでフィルター機能を確認しました。ここでは、明確な QA プロセスを持つカスタムエージェントに、これらの機能をまとめます。

このレッスンでは、次の作業を行います。

- カスタムエージェントが指示、スキル、MCP ツールと連携する方法を確認します。
- 再利用可能な品質保証 (QA) プロファイルを作成して調べます。
- QA エージェントを選択し、フィルター機能の Issue に照らして結果をレビューします。

## シナリオ

Tailspin Toys は、pull request (PR) を作成する前に、要件、コード品質、自動チェック、テストカバレッジ、ブラウザーの動作を一貫してレビューしたいと考えています。カスタムエージェントを使うと、QA プロセスを調整し、再利用できるレポートを作成できます。

## カスタムエージェントとは

カスタムエージェントは、Markdown プロファイルで定義する Copilot の特化バージョンです。プロファイルには、エージェントの目的、指示、利用できるツールを記述します。このワークショップでは、`.github/agents/qa.agent.md` に QA の役割を定義し、Copilot CLI で選択します。

これまでに使ったカスタマイズは、それぞれ異なる役割を持ちます。リポジトリの指示はチームの標準を説明し、`quality-checks` スキルは繰り返し実行できるチェックをまとめます。Playwright MCP はブラウザーツールを提供します。QA プロファイルは、それらを使って要件を評価し、結果を報告するよう Copilot に指示します。それらを置き換えたり、別の会話を必要としたりするものではありません。

## QA プロファイルを作成する

機能の PR を開く前に、再利用可能な QA プロファイルを作成するよう Copilot に依頼します。QA が行うチェックと、従う必要がある制約の両方をプロファイルに定義します。

1. Codespace に戻り、フィルター機能の会話が Interactive モードであることを確認します。
2. 既定のエージェントに次のプロンプトを送り、カスタムエージェントを作成します。

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## プロファイルを確認する

新しいエージェントを使う前に、意図した QA ワークフローと制約がプロファイルに反映されていることを確認します。検証だけを依頼したのに、不完全なエージェントや範囲の広すぎるエージェントが機能を変更するのを防げます。

1. `/diff` を入力し、`.github/agents/qa.agent.md` を開きます。
2. フロントマターを読みます。`description` は必須です。`name` は省略可能ですが、設定するとエージェントに分かりやすい表示名が付きます。
3. プロファイルの指示を読み、QA が要件から始めてリポジトリの指示に従い、`quality-checks` スキルと Playwright MCP を使うことを確認します。
4. QA が根拠を示してレポートし、実装コードを変更する前に確認を求め、変更をコミットしたり pull request を開いたりしないことを確認します。
5. これらの役割や制約が欠けている場合は、続行する前に既定のエージェントにプロファイルを修正させます。

## Issue に対して QA を実行する

Copilot CLI は会話の開始時にプロジェクトのエージェントを読み込みます。プロファイルを作成した後、同じフィルター機能の会話を再開し、QA を選択します。そうすると、コンテキストにすでにある Issue と計画上の決定事項を使えます。

1. 次のコマンドで QA エージェントを有効にします。

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> エージェントを作成した直後は、利用可能なエージェント一覧に表示されない場合があります。上記のコマンドで有効にできます。

2. 次のプロンプトで、QA エージェントに機能をレビューするよう依頼します。

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. QA エージェントが作業を開始します。
4. 作業が完了したら、レポートを確認します。

## まとめと次のステップ

再利用可能な専門家の役割をワークフローに加え、その作業をレビューしました。このレッスンでは、次の作業を行いました。

- カスタムエージェントが指示、スキル、MCP ツールと連携する方法を確認した。
- 再利用可能な QA プロファイルを作成して調べた。
- QA エージェントを選択し、フィルター機能の Issue に照らして結果をレビューした。

実装、スキルの更新、QA プロファイル、テスト、検証レポートがそろいました。次は[機能の PR にまとめ、Agent Merge を使用します][next-lesson]。

## リソース

- [Copilot CLI 用のカスタムエージェントを作成する][create-agents]
- [カスタムエージェントの設定][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
