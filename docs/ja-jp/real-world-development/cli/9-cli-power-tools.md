---
title: "レッスン 9 - GitHub Copilot CLI のスラッシュコマンド"
description: "コンテキストの管理、モデルの選択、セッションの共有、cloud agent への任意の委任に使うスラッシュコマンドを確認します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

多くの CLI ツールと同様に、GitHub Copilot CLI には対話に使うスラッシュコマンドが数多く用意されています。高度な機能、内部情報、追加の設定オプションを利用できます。これまでに `/diff`、`/mcp`、`/skills`、`/agent`、`/pr` などを使いました。ここでは、さらに便利なコマンドを確認します。

このレッスンでは、次の作業を行います。

- `/context` と `/compact` で、Copilot が会話のコンテキストを管理する方法を確認します。
- `/model` で利用可能なモデルを確認します。
- `/share` でセッションをエクスポートまたは共有する方法を学びます。
- 並行作業、worktree、cloud agent への委任に使える任意のコマンドを確認します。

## シナリオ

CLI の主要なワークフローが完了しました。次に、コンテキストの管理、モデルの切り替え、セッションの共有、[Copilot cloud agent][about-cloud-agent] への任意の委任など、追加機能を確認します。

## Copilot CLI のコンテキストを確認する

大規模または複雑なタスクに取り組むと、モデルの最大コンテキストウィンドウに達することがあります。Copilot CLI は必要に応じて会話を自動的に圧縮しますが、スラッシュコマンドを使って確認したり、自分で圧縮したりすることもできます。

1. Codespace に戻り、Copilot CLI が起動していない場合は、リポジトリのルートから起動します。
2. 次のコマンドを入力します。

   ```plaintext
   /context
   ```

3. モデル、現在のトークン使用量、システム指示、ツール、メッセージ、空き領域に分割されたコンテキストを確認します。
4. 会話を圧縮します。

   ```plaintext
   /compact
   ```

5. もう一度 `/context` を入力して結果を比較します。会話が短い場合は、大きな違いがないこともあります。

> [!NOTE]
> コンテキストウィンドウの空きが少なくなると、Copilot CLI は自動的にコンテキストを圧縮します。自分でタイミングを決めたいときは `/compact` を使います。関係のない別タスクに切り替える場合は、`/clear` または `/new` で新しい会話を始めます。

## モデルを選ぶ

モデルごとに得意分野が異なり、開発者によって好みも異なります。Copilot CLI では、利用可能なモデルを一覧表示して、使用するモデルを選べます。

1. 次のコマンドを入力します。

   ```plaintext
   /model
   ```

2. 利用可能なモデルと使用状況の情報を確認します。
3. 現在のモデルを使い続けるか、別のモデルを選びます。一覧を閉じるには <kbd>Esc</kbd> を押します。

## セッションを共有する

チームで協力して学びを共有すると、全員が AI ツールの使い方を改善できます。`/share` コマンドを使うと、セッションを Markdown ファイルまたは HTML ファイルにエクスポートしたり、共有リンクを作成したり、GitHub gist を公開したりできます。

1. `/help` を入力し、インストール済みのバージョンで利用できる `/share` オプションを確認します。
2. セッションを共有する場合は、ローカルの Markdown ファイルにエクスポートする `/share file` など、必要に合った方法を選びます。
3. エクスポートした内容を、誰かに送ったり公開したりする前に確認します。セッションのエクスポートには、プロンプト、応答、プロジェクトの詳細が含まれる場合があります。

共有リンクや gist の公開は任意です。チームが共有するつもりのないリポジトリや会話の内容を公開しないでください。

## オプション: 作業の並列化と委任

ワークショップの主要な内容は完了です。Copilot CLI には、大規模なタスクに使えるコマンドもあります。

- `/fleet` は、独立したサブタスクをサブエージェントに分割し、並行して実行します。
- `/worktree` は、別のタスク用に分離された Git worktree を作成します。
- `/delegate` は、Copilot cloud agent にタスクを送信します。cloud agent は非同期に作業し、pull request を開く場合があります。

これらのコマンドは worktree やリモート作業を追加する可能性があるため、任意の手順です。試す場合は、新しく範囲の明確なタスクから始め、通常のワークフローで結果をレビューしてください。非同期エージェントの作業について詳しく知りたい場合は、[cloud agent ワークショップ][cloud-workshop]に進みます。

## まとめと次のステップ

Copilot CLI のスラッシュコマンドを使うと、設定を変更したり、セッションを共有したり、内部で起きていることを確認したりできます。このレッスンでは、次の作業を行いました。

- `/context` と `/compact` で、Copilot が会話のコンテキストを管理する方法を確認した。
- `/model` で利用可能なモデルを確認した。
- `/share` でセッションをエクスポートまたは共有できることを学んだ。
- 並行作業、worktree、cloud agent への委任に使える任意のコマンドを確認した。

Copilot CLI にはほかにも多くのスラッシュコマンドがあります。最後に[学んだ内容を振り返り][next-lesson]、学習を続けるための次のステップを確認します。

## リソース

- [Copilot CLI コマンドリファレンス][cli-reference]
- [Copilot CLI のコンテキスト管理][context-management]
- [Copilot cloud agent について][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
