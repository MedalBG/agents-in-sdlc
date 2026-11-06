---
title: "レッスン 3 - エージェントモード: Plan と Autopilot"
description: "Plan でアプローチを決め、Autopilot で Issue からフィルター機能を構築して、Interactive モードで結果をレビューします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

最初にプロジェクトに追加したのは小さな機能でした。大きな変更には、より堅牢なプロセスが必要です。GitHub Copilot CLI は、組織の既存の開発フローに沿って適切な方法で作業できるよう設計されています。このレッスンから数回にわたり、Issue から新機能を作り、コードの妥当性と機能の動作を確認して、最終的にプロジェクトにマージする、一般的なエージェント主導の開発プロセスを実践します。

> [!NOTE]
> 機能の作業を続ける間、同じ会話とブランチを使用します。通常はファイルの種類ごとに異なるブランチや PR を使いますが、ここでは中心的な概念に集中できるよう手順を簡略化します。

このレッスンでは、次の作業を行います。

- GitHub Issue から新しい Copilot の会話を開始します。
- Plan モードで要件を定義します。
- Autopilot モードで新機能を実装します。
- コードをレビューします。
- 転送されたブラウザーで機能を手動検証します。

この機能の作業を続けながら、リポジトリの指示を更新し、既存の `quality-checks` スキルをカスタマイズし、MCP による検証を追加して QA エージェントを作成し、機能の PR を開きます。

## シナリオ

Tailspin Toys のカタログが充実し、訪問者がゲームをカテゴリとパブリッシャーで絞り込める機能が必要になりました。バックログの Issue には機能が記載されていますが、カテゴリを組み合わせる方法などはコーディング前に合意する必要があります。Plan モードでそれらの決定を行い、範囲を限定した実装を Autopilot に承認します。

## 背景

AI コーディングエージェントを開発フローに導入しても、基本は変わりません。むしろ、基本はさらに重要になります。多くの開発者は、次のようなフローに従います。

1. 必要な作業が記載された Issue から始めます。
2. 実装する内容の計画を立てます。
3. コードを作成してレビューします。
4. テストを実行し、コードを検証します。
5. 新機能を手動で検証します。
6. pull request (PR) を作成します。
7. コードのレビューと継続的インテグレーションが成功したら、コードをマージします。

> [!NOTE]
> 正確な手順はチームや Organization によって異なりますが、多くの場合は上記の流れを変形したものです。

この標準的なアプローチを守ることで、AI が生成したコードが要件を満たし、人間が作成したコードと同じレビューのプロセスを通るようにできます。

## 会話モード

**会話モード**は、エージェントにどの程度の自律性を与えるかを制御します。<kbd>Shift</kbd>+<kbd>Tab</kbd> を押してモードを切り替えます。

- **Interactive**: エージェントと一緒に作業します。エージェントは変更を提案し、続行する前にユーザーの入力を待ちます。
- **Plan**: エージェントが最初に計画を作成します。プロジェクトファイルの編集はできません。
- **Autopilot**: エージェントが自律的にコードを書き、テストを実行し、タスクが完了するまで反復します。

Plan モードで開始して計画をレビューし、その後 Autopilot で実装します。

## Issue から作業を開始する

フィルター機能に取り組む前に、Codespace に戻り、リポジトリとターミナルを準備します。

1. Codespace に戻ります。停止している場合は、再起動してから続行します。
2. 星評価の PR がマージされていることを確認します。
3. ターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押します。
4. `main` を更新してから、フィルター機能用のブランチを作成します。

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Copilot CLI を起動します。

   ```bash
   copilot --yolo
   ```

6. <kbd>Tab</kbd> を 2 回押して **Issues** タブを開きます。
7. <kbd>A</kbd> を押してすべての Issue を表示します。
8. 矢印キーを使って、**Allow users to filter games by category and publisher** というタイトルの Issue を選びます。
9. <kbd>C</kbd> を押して Issue をプロンプトに追加し、**Session** タブに戻ります。

プロンプトが `#7` (または同様の番号) で始まることに注目してください。`#` を使うと、GitHub 上の Issue や pull request (PR) をコンテキストに追加できます。

## フィルター機能を計画する

計画を立てることで、Copilot にタスクを任せる前に、機能や作業の実装方法を決められます。複雑な作業では、時間をかけて計画することをお勧めします。Plan モードに切り替え、Copilot に計画を作成するよう依頼します。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Plan モードに切り替えます。プロンプトの下にあるモード表示が **Plan** であることを確認します。
2. 前の手順で追加した Issue への参照に続けて、次のプロンプトを入力します。

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot はプロジェクトを調べて、最適な方法を検討しながら計画を作成します。

3. フィルター機能の動作について質問された場合は、希望に応じて回答します。回答に唯一の正解はありません。
4. 計画が完成したら、<kbd>Control</kbd>+<kbd>E</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) を押して計画を展開します。
5. 計画を上下にスクロールしてレビューします。
6. 決めた内容と異なる部分があれば、Copilot に計画を修正するよう依頼します。

## Autopilot を承認する

計画を作成してレビューしたら、実装に進みます。Autopilot を使うと、Copilot が作業を反復して完了を目指します。

1. **Accept plan and build on autopilot (recommended)**、またはインストール済みのバージョンに表示される同様のオプションを選択します。
2. プロンプトの下にあるモード表示が **Autopilot** であることを確認します。
3. Copilot が計画に沿って作業し、コードを生成してテストを実行する様子を確認します。

> [!NOTE]
> 承認すると実装がすぐに始まる場合があるため、先に計画をレビューしてください。依存関係が不足している場合やポートが競合している場合は、チェック完了と判断する前にセットアップの問題を解決します。

## 実装をレビューして検証する

生成されたコードも、ほかのコードと同じようにマージ前にレビューします。コードを確認し、サイトを実行して問題がないことを確認しましょう。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Interactive モードに切り替えます。モード表示が **Plan** または **Autopilot** ではないことを確認します。
2. `/diff` を入力し、フィルター機能の実装とテストを確認します。
3. 結果を Issue と計画中に決めた内容に照らし合わせます。
4. レビューが終わったら、<kbd>Esc</kbd> を押して差分画面を閉じます。
5. プロジェクトのチェック結果を確認し、失敗があれば Copilot に修正を依頼します。

## 新機能を試す

コードに問題がないように見えても、実際に動くか確認しましょう。前と同じようにアプリを起動し、Codespaces で転送されたポートからサイトを開きます。

1. Copilot にアプリの起動を依頼します。

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces でポート `4321` が利用可能になったら、**Open in Browser** を選択します。
3. カテゴリ、パブリッシャー、計画で合意した組み合わせによるフィルターを試します。
4. リセットと結果が空の場合の動作が、Issue と決定した内容に合っていることを確認します。
5. Codespace に戻り、Copilot に開発サーバーを停止するよう依頼します。

## まとめと次のステップ

会話モードを切り替えて機能を構築し、レビューしました。このレッスンでは、次の作業を行いました。

- GitHub Issue から新しい Copilot の会話を開始した。
- Plan モードで要件を定義した。
- Autopilot モードで新機能を実装した。
- コードをレビューした。
- 転送されたブラウザーで機能を手動検証した。

次は、[カスタム指示を使って][next-lesson]、文書化されたプラクティスにコードを従わせる方法を確認します。

## リソース

- [GitHub Copilot CLI の Autopilot][autopilot]
- [Copilot CLI コマンドリファレンス][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
