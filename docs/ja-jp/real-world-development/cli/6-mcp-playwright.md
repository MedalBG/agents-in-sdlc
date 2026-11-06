---
title: "レッスン 6 - Playwright MCP で機能を検証する"
description: "Copilot CLI で Playwright MCP を確認または設定し、ブラウザーからフィルター機能を確認します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

すでに説明したように、コードを書くことだけが開発ではありません。データや外部サービスを扱い、Copilot で追加の自動化を利用できるようにする必要もあります。そこで MCP サーバーを使います。MCP サーバーを使うと、Copilot CLI に組み込まれている機能に加えて、さらに多くのツールやサービスを利用できます。

このレッスンでは、次の作業を行います。

- Model Context Protocol (MCP) とは何か、Copilot CLI がどのように利用するかを確認します。
- まだ利用できない場合は、Playwright MCP サーバーを追加します。
- エージェントにブラウザーを操作させ、フィルター機能を確認します。

## シナリオ

単体テストや E2E テストは重要ですが、UI の変更を検証するには、実際に UI を操作する必要があります。Copilot が作業中の Web サイトをユーザーと同じように使えるようにし、変更が期待どおりに動作する確信を高めます。

## Model Context Protocol (MCP) とは

[Model Context Protocol (MCP)][mcp-blog-post] は、AI エージェントが外部のツールやサービスと通信するための仕組みです。MCP を使うと、AI エージェントは外部のツールやサービスとリアルタイムにやり取りし、最新情報にアクセスしたり、ユーザーに代わって操作を実行したりできます。

ツールやリソースには、AI エージェントと外部サービスの間をつなぐ MCP サーバーを通じてアクセスします。各 MCP サーバーは、エージェントが利用できるツールとリソースのセットを提供します。

広く使われている MCP サーバーの例を紹介します。

- **[GitHub MCP Server][github-mcp]**: GitHub リポジトリ、Issue、pull request を管理する API にアクセスできます。
- **[Playwright MCP Server][playwright-mcp-server]**: Playwright によるブラウザー自動化機能を提供します。

ほかにも多くの MCP サーバーがあります。GitHub は、エコシステム内での発見やコントリビューションを促すために [MCP registry][mcp-registry] を公開しています。

> [!CAUTION]
> MCP サーバーは、プロジェクト内のほかの依存関係と同じように扱ってください。利用する前に、ソースコードを確認し、公開元を検証して、セキュリティへの影響を検討してください。

## Playwright MCP サーバーを追加する

Playwright MCP サーバーがまだ利用できない場合は追加し、Copilot がユーザーと同じように Web サイトを操作できるようにします。

> [!NOTE]
> Copilot CLI ですでに Playwright MCP を設定している場合は、セットアップ手順を省略して次のセクションに進みます。

1. Codespace に戻ります。
2. Copilot CLI で次のコマンドを入力し、MCP サーバーの追加ダイアログを開きます。

   ```plaintext
   /mcp add
   ```

3. 名前に `playwright` と入力し、<kbd>Tab</kbd> を押します。
4. サーバーの種類が **STDIO** であることを確認し、<kbd>Enter</kbd> を押してから <kbd>Tab</kbd> を押します。
5. **Command** ダイアログに次のコマンドを貼り付けます。

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. <kbd>Control</kbd>+<kbd>S</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) を押して、新しい MCP サーバーを保存します。
7. <kbd>Esc</kbd> を押して MCP ダイアログを閉じます。

## Playwright で Copilot に機能を確認させる

前のレッスンでは、機能が期待どおりに動くことを手動で確認しました。今度は、Copilot が利用できる Playwright MCP サーバーを使って同じ確認を行います。

1. 次のプロンプトを使い、Playwright MCP サーバーで機能を検証するよう Copilot に依頼します。

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> 通常は Copilot が MCP サーバーを自分で選ぶため、使用するよう指定する必要はありません。ただし、使用するツールが分かっている場合は明示すると、より一貫した結果を得られ、トークンも節約できます。

2. Copilot がブラウザーで機能を確認する手順を表示する様子を見ます。
3. 結果を読み、すべて期待どおりに動作することを確認します。

Copilot はサーバーを起動し、Playwright で Web サイトを操作してからサーバーを停止し、結果を報告します。

## まとめと次のステップ

Copilot CLI から Playwright MCP サーバーを使い、実際のブラウザーで機能を確認しました。次の作業を行いました。

- Model Context Protocol (MCP) とは何か、Copilot CLI がどのように利用するかを確認した。
- まだ利用できない場合は、Playwright MCP サーバーを追加した。
- エージェントにブラウザーを操作させ、フィルター機能を確認した。

次は、スキルとブラウザーツールを組み合わせる専門的な役割として、[QA カスタムエージェントを作成します][next-lesson]。

## リソース

- [MCP とは何か、なぜいま注目されているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [Copilot CLI に MCP サーバーを追加する][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
