---
title: "演習 3 - Playwright MCP サーバーで機能をテストする"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: エージェントモード][previous-lesson] · [次のレッスン: カスタムエージェント →][next-lesson]

エージェントモードでフィルター機能を作成しました。pull request を開く前に、ブラウザーで正しく動作することを確認します。自分でアプリを操作する代わりに、**Playwright MCP サーバー**を接続し、Copilot に実際のブラウザーを操作させて機能をテストします。その後、ブランチを公開して PR を作成します。

この演習では、次の作業を行います。

- Model Context Protocol (MCP) と、MCP サーバーが Copilot にツールを追加する仕組みを学びます。
- Playwright MCP サーバーをワークスペースに追加します。
- ブラウザーでフィルター機能を手動テストするよう Copilot に依頼します。
- ブランチを公開し、フィルター機能の pull request を作成します。

## Model Context Protocol (MCP) とは

エージェントモードは、エディターの外部にあるツールを利用できると、さらに便利になります。Copilot がそのために使う仕組みが Model Context Protocol (MCP) です。エージェントが外部のツールやサービスと通信するための標準です。

![エージェントモードの内部構造と、MCP サーバーや VS Code 拡張機能が提供するツールなどのコンテキスト、LLM、ツールとの連携を示す図](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) は、AI エージェントが外部ツールやサービスと通信する仕組みです。MCP を使うと、AI エージェントは外部ツールやサービスとリアルタイムにやり取りし、最新情報 (リソース) にアクセスしたり、ユーザーに代わって操作 (ツール) を実行したりできます。

ツールやリソースには、AI エージェントと外部サービスの間をつなぐ MCP サーバーを通じてアクセスします。MCP サーバーは、既存の API や NPM パッケージのようなローカルツールなど、AI エージェントと外部ツール間の通信を管理します。各 MCP サーバーは、エージェントがアクセスできるツールとリソースのセットを提供します。

広く使われている MCP サーバーの例を紹介します。

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: GitHub リポジトリを管理する API にアクセスし、新しいリポジトリの作成、既存リポジトリの更新、Issue や pull request の管理などを実行できます。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: Playwright によるブラウザー自動化機能を提供します。Web ページへの移動、フォームへの入力、ボタンの選択などを実行できます。

ほかにも多くの MCP サーバーがあり、さまざまなツールやリソースを提供しています。GitHub は、エコシステム内での発見やコントリビューションを促すために [MCP registry](https://github.com/mcp) を公開しています。

> [!CAUTION]
> MCP サーバーは、プロジェクト内のほかの依存関係と同じように扱ってください。利用する前に、ソースコードを慎重に確認し、公開元を検証して、セキュリティへの影響を検討します。信頼できる MCP サーバーのみを使い、機密リソースや操作へのアクセスを許可する際は慎重に判断してください。

## MCP の設定を確認する

`.vscode/mcp.json` ファイルで、この VS Code ワークスペースで利用する MCP サーバーを設定します。

1. codespace で `.vscode/mcp.json` を開きます。
2. すでに `github` サーバーが設定されていることを確認します。

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

`github` の設定はプロジェクトテンプレートに含まれています。そのため、前の演習で Copilot はバックログの Issue を読み取れました。これは[リモート GitHub MCP サーバー][remote-github-mcp-server]を使用しているため、ローカルへのインストールは不要です。VS Code は HTTP で接続し、Copilot が最初にツールを使うときに GitHub で認証します。

## Playwright MCP サーバーを追加する

2 つ目のサーバーを追加します。[Playwright MCP サーバー][playwright-mcp-server]を使うと、Copilot がブラウザーを操作できます。これは機能のテストに必要です。

1. `.vscode/mcp.json` を編集し、`github` と並べて `playwright` エントリを追加します。

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

2. ファイルを保存します。

リモートの `github` サーバーとは異なり、`playwright` は **ローカル**サーバーです。VS Code は `npx @playwright/mcp@latest` を実行して起動します。`--headless` フラグは、表示ウィンドウなしでブラウザーを実行します。デスクトップ表示がない codespace では、この設定が必要です。

> [!NOTE]
> Tailspin Toys では E2E テストにすでに Playwright を使っているため、必要なブラウザーもインストール済みの場合がほとんどです。Copilot がブラウザーの不足を報告した場合は、`npx playwright install chromium` を実行して再試行します。

## サーバーを起動して信頼する

VS Code は MCP サーバーを**必要に応じて**起動します。Copilot が初めてサーバーのツールを使うときにサーバーを起動し、信頼するか確認します。

1. `.vscode/mcp.json` で `playwright` エントリの上に表示される **Start** 操作を選択し、サーバーを起動して設定が正しいことを確認します。
2. VS Code からサーバーを信頼するか確認された場合は、設定を確認して信頼するよう選択し、サーバーを起動します。

手動で起動する必要はありません。省略した場合は、次の手順で Copilot がツールを初めて使用するときに VS Code が起動し、信頼を確認します。

## フィルター機能をテストする

[Playwright MCP サーバー][playwright-mcp-server]を使うと、Copilot が実際のブラウザーを操作できます。自分でアプリを操作する代わりに、エージェントがページを開き、移動し、フィルターを適用して結果を読み取り、確認した内容を要約できます。会話を離れずに機能を確認する最も速い方法です。

Playwright MCP サーバーはスクリーンショットではなく、ページの[アクセシビリティツリー][playwright-mcp-server]を使って動作します。エージェントは、支援技術と同じように、ボタン、リンク、リスト項目などの構造化されたラベル付き要素を扱います。簡単な機能確認が、アクセシビリティの簡易チェックも兼ねます。

サーバーを接続してアプリを起動したら、次のプロンプトを Copilot に送信してフィルター機能を確認します。

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot は Playwright MCP サーバー経由でブラウザーを起動し、各手順を実行して結果を報告します。その要約を Issue の受け入れ条件に照らし合わせます。問題がある場合は、追加で質問するか、pull request を開く前にコードの修正を依頼します。

> [!NOTE]
> このテストには、`http://localhost:4321` で起動中のアプリが必要です。開発サーバーを停止した場合は、プロンプトを送る前に再起動します。Copilot が Playwright MCP サーバーを初めて使うときに、ブラウザーのダウンロードが必要になる場合があります。ブラウザーが見つからないと報告された場合は、`npx playwright install chromium` を実行して再試行します。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## ブランチを公開して pull request を作成する

機能が動作することを確認したので、チームでレビューする pull request (PR) を作成します。まず `filtering-vscode` ブランチを公開します。

1. codespace の **Source Control** パネルに移動し、Copilot が行った変更を確認します。
2. **+** アイコンを選択して変更をステージします。
3. **Sparkle** ボタンを使ってコミットメッセージを生成します。

   ![Copilot が行った変更を表示する Source Control パネルのスクリーンショット](../../../_images/ex3-source-control-changes.png)

4. **Publish** を選択して、ブランチをリポジトリにプッシュします。

## pull request を作成する

pull request の作成には、github.com や GitHub コマンドラインインターフェイス (CLI) など複数の方法があります。今回は GitHub Copilot に作成を依頼します。Copilot は関連する Issue を見つけ、PR と関連付けられます。

1. Copilot Chat パネルに移動し、**New Chat** を選択して新しいセッションを開始します。
2. エージェントドロップダウンで **Agent** が選択され、Copilot が GitHub ツールを使用できることを確認します。
3. 次のプロンプトで PR の作成を依頼します。

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. Copilot が情報を収集して操作を行う際に **Continue** を選択し、必要な操作を許可します。GitHub ツールを初めて使うときに GitHub への認証を求められた場合は、画面の指示に従います。
5. Copilot が Issue を検索し、適切な Issue を見つけて PR を作成する様子を確認します。
6. Copilot が生成したリンクを選択して PR をレビューします。ただし、**まだマージしないでください**。

## まとめと次のステップ

この演習では、次の作業を行いました。

- Model Context Protocol (MCP) と、MCP サーバーが Copilot にツールを追加する仕組みを学んだ。
- Playwright MCP サーバーをワークスペースに追加した。
- 出荷前に、ブラウザーでフィルター機能を手動テストするために使用した。
- ブランチを公開し、フィルター機能の pull request を作成した。

MCP サーバーを使って機能をテストしましたが、MCP は Copilot を拡張する方法の 1 つです。次は、アクセシビリティレビューなど、対象を絞ったタスクを効率化する[カスタムエージェントを作成します][next-lesson]。

## リソース

- [MCP とは何か、なぜいま注目されているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP Registry][mcp-registry]
- [VS Code の MCP サーバー][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
