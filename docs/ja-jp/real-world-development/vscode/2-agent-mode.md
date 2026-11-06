---
title: "演習 2 - Copilot エージェントモードで機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

アプリケーションの小さな更新でも、通常は複数のファイルを変更し、テストを実行する必要があります。開発者は、必要なファイルを探し、変更を加え、テストを実行し、問題をデバッグして、変更漏れがないか確認します。場合によっては、さらにファイルを更新する必要があります。

このようなときに Copilot エージェントモードが役立ちます。

Copilot エージェントモードは、IDE 内でより自律的に作業するよう設計されています。開発者のように既存のプロジェクト構造を調べ、必要なファイルを更新し、テストなどのタスクを実行して、見つかった問題を自動的に修正します。エージェントモードを使って、Web サイトに新機能を追加する方法を確認しましょう。

> [!NOTE]
> 名前は似ていますが、エージェントモードと cloud agent は異なる用途向けです。エージェントモードは IDE 内で動作し、短いフィードバックサイクルで対話しながら作業できます。cloud agent はチームメンバーのように非同期で作業し、Issue や pull request を通じてやり取りします。

この演習では、次の内容を学びます。

- Copilot エージェントモードでプロジェクトを調べ、関連ファイルを特定し、複数ファイルの変更を調整する方法。
- GitHub Copilot エージェントモードで UI とデータレイヤーにまたがる機能を実装する方法。
- コードベースにマージする前に、Copilot エージェントモードが生成した変更とテストをレビューする方法。

## シナリオ

ゲームの一覧が増えてきたため、カテゴリやパブリッシャーでゲームを絞り込めるようにします。前の演習では publishers ヘルパーを追加しました。ここでは、Copilot エージェントモードで残りのデータレイヤー、UI、テストを完成させます。

## Tailspin Toys Web サイトを実行する

変更を加える前に、Tailspin Toys Web サイトの現在の機能を確認します。

この Web サイトは、開発者をテーマにしたボードゲームのクラウドファンディングプラットフォームです。ゲームの一覧と詳細を表示します。Astro アプリはビルド時に静的 HTML を生成します。ページは Drizzle ORM を通じて frontmatter からローカルの SQLite データベースを直接参照するため、独立したバックエンド API やクライアントサイドの UI フレームワークはありません。再利用可能なデータアクセスヘルパーは `src/lib/` に置き、インタラクティブな動作には標準の DOM API を使う範囲の狭い Astro `<script>` を使用します。

### Web サイトを起動する

単一の Astro 開発サーバーを起動する `npm run dev` タスクが用意されています。GitHub Codespace で次の手順を実行します。

1. 前の演習の後に `main` に戻っているはずなので、codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押して、Codespace 内に新しいターミナルを開きます。
3. フィルター機能用の新しいブランチを作成して切り替えます。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 次のコマンドを実行して Web サイトを起動します。

   ```bash
   npm run dev
   ```

   Astro 開発サーバーの準備ができると、次のような URL のバナーが表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> `http://localhost:4321` を開くよう促すダイアログが表示された場合は、**x** を選択して閉じます。

5. ターミナルで <kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら、Astro サーバーのアドレス `http://localhost:4321` を選択して Web サイトを開きます。

> [!NOTE]
> codespace のターミナルで localhost の URL を選択すると、自動的に `https://<your-codespace-name>-4321.app.github.dev/` に転送されます。これは Web サーバーをホストしている codespace へのプライベートトンネルです。

### Web サイトを確認する

Web サイトが起動したら、機能を確認します。主なページは次のとおりです。

- **Home Page**: タイトル、画像、説明を含むボードゲームの一覧を表示します。
- **Game Details Page**: ゲームを選択すると、タイトル、説明、パブリッシャー、カテゴリなどの情報を掲載した詳細ページを表示します。

## Copilot でバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に codespace に戻り、Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なるため、必要な場合は各手順で説明します。

Web サイトの初期実装は動作しますが、新しい機能で強化します。まずバックログを確認しましょう。テンプレートからプロジェクトを設定すると GitHub Issue のバックログも自動的に作成されるため、GitHub Copilot に一覧を表示するよう依頼します。

1. Chat ビューのエージェントドロップダウンから **Agent** を選択します。**Agent** はファイルを調べ、変更を計画して実装し、ターミナルコマンドやツールを実行します。

   ![Chat ビューでエージェントピッカーを表示したスクリーンショット](../../../_images/shared-chat-mode-selector.png)

2. 利用可能なモデル一覧から **Claude Sonnet 4.5** を選択します。

> [!CAUTION]
> このワークショップの作成者が特定のモデルを推奨しているわけではありません。ワークショップの作成には Claude Sonnet 4.5 を使ったため、手順ではそのモデルを指定しています。比較的一貫した体験を提供することが目的ですが、LLM は確率的に動作するため、提案は記載された例と異なる場合があります。これは正常な動作です。

> [!NOTE]
> LLM の確率的な動作により、Copilot が異なる MCP コマンドを使うことがありますが、タスクは実行できるはずです。

3. 次のプロンプトを Copilot に送信して、バックログの Issue を確認します。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. **Continue** を選択して、すべての Issue を一覧表示するコマンドを実行します。
5. 生成された Issue の一覧を確認します。

Copilot はユーザーに最も役立つと思われる項目を優先順位付けしています。

## 指示ファイルを確認する

コードの生成をエージェントに依頼する前に、Copilot にコンテキストを与える指示ファイルを確認します。この演習では、Web サイトに機能を追加するときの方法を説明する[ユーザーインターフェイス (UI) 指示ファイル](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)を使います。

1. codespace で `.github/instructions/ui.instructions.md` を開きます。
2. 機能を追加するときの全体的なガイダンスを確認します。次の内容が含まれます。
   - アーキテクチャの概要
   - コンポーネント設計、テスト容易性、アクセシビリティの原則
   - Astro、Tailwind CSS、Drizzle など、ファイルの種類ごとの指示へのリンク

> [!TIP]
> 指示ファイルから、ほかの指示ファイルやプロジェクト内のファイルを参照できます。パスは指示ファイルからの相対パスです。指示を再利用し、複雑な内容を管理しやすい単位に分け、Copilot に例やテンプレートを示せます。

## エージェントモードでフィルター機能を実装する

フィルター機能を実装するには、少なくとも次の 3 つの更新が必要です。

- データレイヤー (`src/lib/`) に残りのフィルターロジックを追加または調整する。
- フィルターの動作を確認するテストを追加または更新する。
- ゲーム一覧ページにフィルター UI を追加する。

コードベースにマージする前にテストを実行して成功させる必要があります。Copilot エージェントモードにこれらの作業を依頼します。

1. 現在の Copilot 会話を続けるか、**New Chat** を選択して新しい会話を開始します。
2. **Add Context**、**Instructions**、**ui** の順に選択して、UI 指示ファイルを追加します。

   ![UI 指示ファイルを選択する例を示したスクリーンショット](../../../_images/ex3-select-instructions-file.png)

3. Chat ビューのエージェントドロップダウンで、引き続き **Agent** が選択されていることを確認します。

   ![Chat ビューでエージェントピッカーを表示したスクリーンショット](../../../_images/shared-chat-mode-selector.png)

4. 引き続き **Claude Sonnet 4.5** がモデルとして選択されていることを確認します。
5. バックログの関連 Issue に基づいて機能を実装するよう、次のプロンプトを Copilot に送信します。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot がプロジェクトを調べ、データレイヤーのヘルパーと UI、テストなど、機能に関連するファイルを見つける様子を確認します。その後、ファイルを変更してテストを実行します。

   ![Copilot がプロジェクトファイルを調べているスクリーンショット](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot はプロジェクトの調査、ファイルの変更、テストの実行などを行うため、タスクやコードベースの複雑さによって数分かかることがあります。作業中、コードエディターに **Keep** と **Undo** が表示されることがあります。Copilot が完了するとすべての変更に対する **Keep** または **Undo** が表示されるため、作業中に選択する必要はありません。

7. Copilot から求められたら、**Continue** を選択してテストを実行します。

   ![Copilot Chat ペインでテスト実行の確認を求めるダイアログのスクリーンショット](../../../_images/ex3-agent-mode-run-tests.png)

8. 作業中に処理が一時停止したり、テストが失敗したりする場合があります。Copilot はコード生成とテストを繰り返し、タスクを完了してエラーがないと判断するまで作業します。

   ![Copilot エージェントモードの完了した Chat セッションのスクリーンショット](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 生成されたコードに問題がないか確認します。

> [!CAUTION]
> Copilot などの AI ツールが生成したコードは、必ずレビューしてください。

10. Web サイトを開いているブラウザーに戻り、新機能を確認します。
11. すべて動作し、コードのレビューも完了したら、Copilot Chat ウィンドウで **Keep** を選択します。

## まとめと次のステップ

GitHub Copilot エージェントモードを使って Tailspin Toys Web サイトに機能を追加しました。次の内容を学びました。

- GitHub Copilot エージェントモードで UI とデータレイヤーにまたがる機能を実装する方法。
- Copilot エージェントモードでプロジェクトを調べ、関連ファイルを特定し、複数ファイルの変更を調整する方法。
- コードベースにマージする前に、Copilot エージェントモードが生成した変更とテストをレビューする方法。

次は[Playwright MCP サーバーで機能をテストし][next-lesson]、pull request を作成します。

### ボーナス演習 — ページ分割を実装する

ゲーム一覧が増えると、ページ分割が必要になります。この演習で学んだスキルを使い、Copilot にページ分割機能の実装を依頼します。

- 既存の指示ファイルを使うなど、現在のベストプラクティスに従います。
- ページサイズをユーザーが選べるようにするか、固定にするかを決めます。
- 希望する実装に必要なガイダンスをプロンプトに含めます。
- Copilot に変更を依頼したりコンテキストを提供したりして、反復が必要な場合があります。これは Copilot を使う通常のワークフローです。

## リソース

- [Copilot の Ask、Edit、Agent モード — それぞれの役割と使い分け][choose-mode]
- [VS Code のエージェントモード][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
