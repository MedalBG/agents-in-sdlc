---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタム指示][previous-lesson] |
|:--|

技術的負債に悩んでいない Organization はほとんどありません。未解決のセキュリティ問題、更新が必要なレガシーコード、実装する時間がなくバックログに残った機能リクエストなど、さまざまな形があります。GitHub Copilot の cloud agent は、コードの更新や機能の追加といった作業を自律的に実行できます。作業が完了すると、人間の開発者がレビューできるドラフト PR を作成します。これにより、時間のかかる作業を任せて開発を加速し、開発者はより大きな課題に集中できます。

Copilot cloud agent を使って、次の内容を確認します。

- コード生成に使う環境のカスタマイズ
- 操作を安全に行う方法
- Issue の範囲を明確に定めることの重要性
- Copilot への Issue の割り当て

## シナリオ

Tailspin Toys には解決したい技術的負債があります。最初の Web サイトを作成した委託先が、ドキュメントを適切に整備しませんでした。実際、ドキュメントはほとんどありません。まず、アプリケーションのエクスポートされたすべての関数に TSDoc のドキュメントコメントを追加します。

デザインチームはゲームを見つけやすくしたいと考えています。各ゲームの詳細ページに、同じカテゴリに属するほかのタイトル、つまり関連ゲームを表示します。完成度の高い実装はまだ必要なく、UX の受け入れテストに使える状態を求めています。この作業は現在ブロッカーですが、優先度の高い Issue がほかにあるため、後回しになっています。

どちらも優先度が下がりやすい作業であり、Copilot cloud agent に割り当てるのに適しています。Copilot cloud agent に非同期で作業させれば、開発者は別の作業に集中し、後から結果をレビューできます。

## GitHub Copilot cloud agent について

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) は、人間の開発者と同じようにバックグラウンドで作業できます。[GitHub Issue を Copilot に割り当てる][assign-issue]方法など、複数の方法で作業を依頼できます。割り当てると、Copilot は進行状況を追跡するドラフト pull request を作成し、環境を準備して、タスクに取り組み始めます。作業中でも完了後でも、Copilot のセッションを確認できます。提案された解決策のレビュー準備ができると、pull request で通知されます。

## 適切な範囲の指示が重要

GitHub Copilot に魔法はありません。考える作業を省略できるわけではありません。一見簡単な作業でも、詳しく調べると複雑さが見えてきます。[Copilot cloud agent に依頼するタスクの範囲][cloud-agent-best-practices]を慎重に決めてください。AI ペアプログラマーとして扱い、段階を踏んで学び、試し、必要に応じて調整します。生成 AI を導入しても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム指示

前の演習では、カスタム指示と Copilot のガイドに使う方法を確認しました。カスタム指示を扱うハンズオンを完了していれば、`.github/copilot-instructions.md` が機能することを確認しています。まだの場合は、この機会に簡単に確認しましょう。Copilot cloud agent に作業を割り当てる前に、このリポジトリに用意された指示ファイルを読み取り専用で確認し、後でその影響を探せるようにします。

稼働中の codespace がある場合はそこで、またはリポジトリの GitHub Web UI で、次のファイルを開きます。

- `.github/copilot-instructions.md` — **Code standards** セクション、特に TypeScript の規約と TSDoc ドキュメントコメントに関する要件を確認します。
- `.github/instructions/unit-tests.instructions.md` — `applyTo` フロントマターを確認します。この指示は `**/*.test.ts` ファイルに適用されます。

次のセクションで `Code lacks documentation` Issue を cloud agent に割り当てる際、生成された pull request に TSDoc コメント、TypeScript の規約、コメントヘッダーが反映されていることを確認します。これらの指示ファイルの影響は、後の演習で PR をレビューするときにもう一度取り上げます。

## Copilot cloud agent の開発環境を設定する

誰が作業する場合でも、コードを作成するには通常、適切な状態を整えるための環境設定やスクリプトが必要です。ソフトウェアエンジニアと同じように作業する Copilot にタスクを割り当てる場合も同様です。

cloud agent は作業環境として [GitHub Actions][github-actions] を使います。`.github/workflows/copilot-setup-steps.yml` ファイルに設定する[専用のセットアップワークフロー][setup-workflow]を作成すると、作業を始める前に必要な開発ツールと依存関係を準備できます。このラボでは、学習をスムーズに進められるよう、あらかじめ設定してあります。このワークフローによって、Copilot は Node.js、プロジェクトの依存関係、E2E テスト用のブラウザーバイナリ、単一の Astro アプリ用に移行およびシードされた SQLite データベースを利用できます。

```yaml
name: "Copilot Setup Steps"

# リポジトリの "Actions" タブからセットアップ手順をテストできます
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 手順に必要な最小限の権限を設定します。Copilot には操作用の独自トークンが付与されます。
    permissions:
      # 依存関係のインストールなど、セットアップ手順でリポジトリをクローンする場合は `contents: read` 権限が必要です。セットアップ手順でクローンしない場合は、完了後に Copilot が自動的にクローンします。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # フロントエンド / アプリのセットアップ - Node.js (アプリ全体は Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # ローカルの SQLite データベースをマイグレーションしてシードし、ビルドとテストで使うデータを用意します。
      - name: Set up the database
        run: npm run db:setup
```

ほかの GitHub ワークフローと同じ形式ですが、次の点を確認してください。

- `copilot-setup-steps` という 1 つのジョブがあります。このジョブは Copilot が pull request の作業を始める前に GitHub Actions で実行されます。
- `workflow_dispatch` トリガーが設定されています。リポジトリの **Actions** タブからワークフローを手動実行できるため、Copilot の実行を待たずに正常に動くかテストできます。

## ドキュメントを追加する

ドキュメントの重要性は広く理解されていますが、古い情報しかないプロジェクトや、ドキュメントがまったくないプロジェクトは少なくありません。こうした技術的負債は放置されやすく、生産性を下げ、コードベースの保守や新しい開発者の受け入れを難しくします。Copilot はドキュメントの作成が得意なため、cloud agent に依頼するのに適した Issue です。バックグラウンドでドキュメントを作成している間、別の作業に取り組み、後の演習で成果をレビューします。

1. 新しいブラウザータブで github.com のリポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue のダイアログを開きます。
4. **Blank issue** を選択して Issue を作成します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次のテキストを入力します。

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選択して、割り当てダイアログを開きます。

    ![Issue への Copilot の割り当て](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。
10. **Pull Requests** タブを選択します。
11. 新しく作成された pull request (PR) を開きます。タイトルは `[WIP]: Code lacks documentation` のような名前になります。PR が一覧に表示されない場合は、少し待ってからブラウザーを更新してください。
12. 数分後、Copilot が作成した todo リストが表示されます。

> [!NOTE]
> Copilot の todo リストが PR に表示されるまで、数分かかることがあります。Copilot は前述のワークフローを実行して環境を準備し、プロジェクトを分析して、問題に取り組む方法を検討しています。

13. todo リストと、Copilot が実行する作業を確認します。
14. pull request のタイムラインを下にスクロールし、Copilot が Issue に取り組み始めたことを示す更新を確認します。
15. **View session** ボタンを選択します。

    ![Copilot のセッションを表示する画面](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 更新されたインジケーターが表示されない場合は、ブラウザーを更新してください。

16. ライブセッションをスクロールして、Copilot が問題を解決する過程を確認します。コードを調査して状態を把握し、計画を考えて、コードを作成する様子を確認できます。

この作業には数分かかる場合があります。Copilot cloud agent の主な目的は、非同期でタスクを実行し、その間にほかの作業を進められるようにすることです。別のタスクも Copilot cloud agent に割り当て、アプリケーションの機能追加に取り組みながら、この利点を活用しましょう。

## ゲームの詳細ページに関連ゲームを追加する

Copilot cloud agent の大きな利点は、作業を分担できることです。デザインチーム向けの関連ゲームの表示は時間のかからない作業かもしれませんが、その時間をほかの作業に使えます。Copilot cloud agent に割り当てましょう。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue のダイアログを開きます。
4. **Blank issue** を選択して、空のテンプレートを使います。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次のテキストを入力します。

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選択して、割り当てダイアログを開きます。

    ![Issue への Copilot の割り当て](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

まもなく、Issue の最初のコメントに 👀 が表示されます。Copilot が作業を開始したことを示します。

![目の絵文字で Copilot が Issue に取り組んでいることを示す画面](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は 2 件目の依頼にも取り組んでいます。Copilot cloud agent はソフトウェアエンジニアと同じように作業するため、常に監視する必要はありません。完了後に結果をレビューできます。次はカスタムエージェントの作成と使用を確認します。

## まとめと次のステップ

このレッスンでは、[GitHub Copilot cloud agent][copilot-agents]について学びました。cloud agent に Issue を割り当てると、非同期で作業を実行します。技術的負債への対処、新機能の作成、フレームワーク間のコード移行などを依頼できます。

次の内容を確認しました。

- コード生成に使う環境のカスタマイズ
- 操作を安全に行う方法
- Issue の範囲を明確に定めることの重要性
- Copilot への Issue の割り当て

cloud agent がバックグラウンドで作業している間に、カスタムエージェントを作成して使う方法を確認します。[Copilot cloud agent は MCP サーバーも使用でき][cloud-agent-mcp]、前のモジュールで説明したカスタム指示にも対応しています。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップワークフローのベストプラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタムエージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
