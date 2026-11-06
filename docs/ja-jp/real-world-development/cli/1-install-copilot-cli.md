---
title: "レッスン 1 - GitHub Copilot CLI のインストール"
description: "Codespace に Copilot CLI をインストールして認証し、基本操作を確認して、用意されたフィルター機能の Issue を見つけます。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

[GitHub Copilot CLI][about-copilot-cli] は、ターミナルで動作する強力なエージェント型コーディングアシスタントです。コードベースの調査、コードの生成、コマンドの実行、外部ツールとの連携をコマンドラインから行えます。タスクを任せたり、変更を依頼したりしながら、作業に集中できます。まずはツールをインストールしましょう。使い慣れたツールでインストールできます。

このレッスンでは、次の作業を行います。

- npm を使って GitHub Copilot CLI をインストールします。
- GitHub アカウントで認証します。
- ワークショップのリポジトリを信頼し、簡単な会話を試します。
- 組み込みの GitHub MCP サーバーを使ってフィルター機能の Issue を見つけます。

## シナリオ

チームでは、増え続けるバックログに対応するために AI エージェントを使い始めています。Copilot CLI は、多くの開発者が日常的に使うターミナルでその機能を利用できるようにします。このレッスンでは、インストールと認証を済ませ、ワークショップの残りで使える状態にします。

## Copilot CLI をインストールする

Copilot CLI は [npm][install-cli]、WinGet、Homebrew でインストールできます。GitHub Codespaces には Node.js がプリインストールされているため、ここでは npm を使います。

1. Codespace に戻り、ターミナルを開きます。
2. Node.js がインストールされ、必要なバージョンを満たしていることを確認します。

   ```bash
   node --version
   ```

   バージョン 24 以上が表示されます。

3. Copilot CLI をグローバルにインストールします。

   ```bash
   npm install -g @github/copilot
   ```

4. インストールを確認します。

   ```bash
   copilot --version
   ```

   バージョン番号が表示されます。

## GitHub で認証する

初めて起動すると、Copilot CLI から GitHub アカウントでの認証を求められます。

1. Copilot CLI を起動します。

   ```bash
   copilot
   ```

2. 認証を求められたら、デバイスコードの手順に従って認証し、Copilot CLI を許可します。
3. Copilot CLI に次のプロンプトが表示されます。

   ```plaintext
   Copilot can read files in this folder and, with your permission, edit them or run code and shell commands. It will remember your permissions for the rest of this session.

   Do you trust the files in this folder?
   ```

4. 表示されたパスが Tailspin Toys リポジトリであることを確認し、**Yes, and remember this folder for future sessions** を選択します。

> [!NOTE]
> Codespace では、GitHub セッションを通じてすでに認証されている場合があります。Copilot CLI が認証を求めずに起動した場合は、そのまま進めてください。

## 基本操作を確認する

通常のシェルプロンプトでは、コマンドは Codespace で直接実行されます。Copilot CLI の起動後は、自然言語の入力がエージェントに送られ、slash command で会話を操作します。

1. `/model` と入力し、矢印キーで **Auto** を選択して <kbd>Enter</kbd> を押します。もう一度 <kbd>Enter</kbd> を押して確定します。
2. `/help` と入力して、インストールされているバージョンで利用できるコマンドを確認し、<kbd>Esc</kbd> でヘルプ画面を閉じます。
3. 次の質問をして、正しく動作することを確認します。

   ```plaintext
   What are the key files in this project?
   ```

4. 応答を読み、Copilot が回答前にリポジトリを調べることを確認します。
5. `/mcp list` と入力し、組み込みの GitHub MCP サーバーが利用可能であることを確認します。
6. Copilot にフィルター機能の Issue を探すよう依頼します。

   ```plaintext
   Using GitHub MCP, find the issue in this repository titled "Allow users to filter games by category and publisher." Give me its URL and a short summary. Don't change anything.
   ```

7. URL を開いて Issue を読みます。短い変更を完了した後、この Issue を使います。

> [!TIP]
> 通常の Copilot CLI セッションは、ターミナルで現在チェックアウトされているブランチ上で動作します。worktree を自動的には作成しません。各変更の前に機能ブランチを作成します。

## ワークショップ用のショートカットを使う

通常、Copilot CLI は既存の許可範囲外でツールを使う前に確認を求めます。このワークショップでは、Codespace 内で `--yolo` を付けて再起動します。ユーザーが許可したこのショートカットを使うと、演習に集中できるよう、確認プロンプトを省略できます。

> [!CAUTION]
> `--yolo` は、すべての自動権限 (`--allow-all-tools`、`--allow-all-paths`、`--allow-all-urls`) を有効にします。Codespace や VM など隔離された環境でのみ使い、日常の開発で既定値として alias に設定しないでください。詳しくは[ツールの使用を許可または禁止する][allow-all-warning]を参照してください。

`--enable-all-github-mcp-tools` を指定すると、後続のレッスンで Issue や pull request を操作するための、読み取りと書き込みの GitHub MCP ツールが有効になります。Codespace によりローカルコンピューターへのアクセスは制限されますが、認証済みの GitHub リソースは引き続き実際のものです。公開やマージの前に変更をレビューしてください。

1. `/exit` で Copilot CLI を終了します。
2. リポジトリのルートから再起動します。

   ```bash
   copilot --yolo --enable-all-github-mcp-tools
   ```

3. プロジェクトについて簡単な質問をして会話が動作することを確認し、`/exit` で終了します。

Copilot は会話を自動的に保存します。後のレッスンでは、指示を変更したりエージェントを追加したりした後に `copilot --resume` を使い、同じ機能の会話とブランチに戻ります。

## まとめと次のステップ

このレッスンでは、npm を使って GitHub Copilot CLI をインストールし、GitHub アカウントで認証しました。また、ワークショップのリポジトリを信頼し、簡単な会話を試して、組み込みの GitHub MCP サーバーからフィルター機能の Issue を見つけました。

次は[最初の対象を絞った変更][next-lesson]を始め、Copilot CLI を使ってゲームカードに星評価を表示します。

## リソース

- [GitHub Copilot CLI のインストール][install-cli]
- [GitHub Copilot CLI について][about-copilot-cli]
- [Copilot CLI コマンドリファレンス][cli-reference]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-add-star-rating/
[install-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/set-up-copilot-cli/install-copilot-cli
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[allow-all-warning]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli#allowing-and-denying-tool-use
