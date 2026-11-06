---
title: "レッスン 2 - 星評価を追加する: 小さな成果"
description: "Copilot CLI でゲームカードに小さな変更を加え、転送されたブラウザーで確認して、最初の pull request としてマージします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Copilot CLI をインストールして会話を試したので、プロジェクトに最初の変更を加えましょう。変更は小さくします。ゲームのデータにはすでに星評価がありますが、ホームページのゲームカードにはまだ表示されていません。エージェントに表示を依頼し、変更をレビューして、最初の pull request としてマージします。

このレッスンでは、次の作業を行います。

- 機能ブランチで対象を絞った Copilot の会話を開始します。
- エージェントに小さな変更を依頼します。
- `/diff` で変更をレビューします。
- アプリを起動し、転送されたブラウザーで変更を確認します。
- 最初の pull request を作成してマージします。

## シナリオ

Tailspin Toys の各ゲームには星評価があり、ゲーム詳細ページにはすでに表示されています。一方、ホームページのゲームカードには、タイトル、カテゴリー、パブリッシャー、説明だけが表示されています。最初のセッションの準備運動として、各カードに既存の評価を表示するようエージェントに依頼します。小規模で自己完結した、最初のセッションに最適な変更です。

## 会話の構造

**会話**とは、タスクについて Copilot CLI と作業する場所です。Copilot app とは異なり、通常の CLI 会話では専用の worktree を作成せず、ターミナルで現在チェックアウトされているリポジトリと Git ブランチを使います。保存した会話に戻って作業を続けられますが、ファイルとブランチはディスク上の通常の Git 状態です。

会話には、プロンプトとエージェントの応答、ファイルを探索および編集するエージェントのツール操作、`/diff` で確認できる変更の 3 つが表示されます。

## 会話を開始して変更を依頼する

新しい会話を開始し、機能の実装に取りかかります。

1. Codespace に戻ります。
2. まだターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押します。
3. Copilot CLI が起動していない場合は、次のコマンドを実行します。

   ```bash
   copilot --yolo
   ```

4. スラッシュコマンド `/new` を入力し、<kbd>Enter</kbd> を押して新しいセッションを開始します。
5. 次のプロンプトで変更を依頼します。

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

Copilot はプロジェクトを調べ、ゲーム詳細の表示に使われているファイルを見つけ、必要なコードを作成します。Copilot CLI を使って新しい機能を追加できました。

## 差分をレビューする

AI が生成したすべての変更は、どれほど小さくてもマージ前にレビューする必要があります。Copilot CLI 内で変更を確認します。

1. `/diff` を入力し、変更されたすべてのファイルを確認します。
2. ゲームカードに数値評価が表示され、`starRating` が `null` の場合は `No rating yet` と表示されることを確認します。
3. 両方の状態がテストされていることを確認します。
4. Copilot が実行したチェックの結果を確認し、失敗があれば修正を依頼します。
5. レビューが完了したら、<kbd>Esc</kbd> を押して差分画面を閉じます。

> [!NOTE]
> すべての生成 AI ツールと同様に、Copilot は決定論的ではなく確率的に動作するため、実際のコードは異なる場合があります。特定の実装ではなく、動作を確認してください。

## 変更を確認する

コードを読むだけで動くと判断するのではなく、Copilot に Web サイトを起動させ、Codespaces で転送されたブラウザーから更新されたユーザーインターフェイス (UI) を確認します。

1. Copilot に次のプロンプトを送り、アプリを起動します。

   ```plaintext
   Start the app so I can inspect the star-rating change in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces でポート `4321` が利用可能になったら、**Open in Browser** を選択します。
3. ゲームカードに 5 点満点の評価が表示されることを確認します。
4. Copilot に戻り、次のプロンプトで起動したサーバーを停止します。

   ```plaintext
   Stop the development server you started.
   ```

## 最初の pull request を開いてマージする

機能を作成できました。次は、新しいコードをプロジェクトにマージするための pull request (PR) を作成します。

1. 既定のエージェントに変更をコミットするよう、次のプロンプトを送ります。

   ```plaintext
   Commit the reviewed star-rating changes with an appropriate commit message.
   ```

2. `/pr create` を入力します。Copilot CLI は PR の作成時に既存のコミットをプッシュし、PR の URL を表示します。
3. Copilot CLI が表示した URL を選択し、PR を開きます。Mac では <kbd>Command</kbd>、Windows/Linux では <kbd>Ctrl</kbd> を押しながら URL を選択します。
4. 変更されたファイルとチェックをレビューします。
5. 準備ができたら **Merge pull request** を選択し、マージを確認します。
6. Codespace に戻り、`/exit` を入力して Copilot CLI を終了します。
7. ローカルの `main` を更新します。

   ```bash
   git checkout main
   git pull
   ```

## まとめと次のステップ

GitHub Copilot CLI を使って最初の変更をリリースしました。具体的には、次の作業を行いました。

- 機能ブランチで対象を絞った Copilot の会話を開始した。
- ゲームカードに小さな変更を加えるようエージェントに依頼した。
- `/diff` で変更をレビューした。
- 転送されたブラウザーで星評価を確認した。
- 最初の pull request を開いてマージした。

次は、[フィルター機能の Issue から開始し、Plan モードと Autopilot モードを使って][next-lesson]、より大きな機能を構築します。

## リソース

- [GitHub Copilot CLI について][about-copilot-cli]
- [Copilot CLI コマンドリファレンス][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
