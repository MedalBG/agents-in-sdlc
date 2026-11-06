---
title: "演習 4 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前の 2 つの演習では、Copilot cloud agent にユーザー体験の改善や機能追加を目的とする 3 つのタスクを依頼しました。cloud agent は非同期かつ自律的に作業しますが、タスクを監視することも重要です。

cloud agent に割り当てたタスクを管理する方法はいくつかあります。[GitHub.com のエージェントページ][agents-page]では、オープンな pull request (PR) があるすべてのエージェントタスクを確認できます。実行された操作を調べ、進行中のセッションに指示を出すこともできます。

このレッスンでは、次の作業を行います。

- エージェントページで cloud agent のタスクを監視します。
- 進行中のセッションに指示を出し、機能を追加で依頼します。

## シナリオ

ハイコントラストモードの作成をエージェントに依頼した後、チームはライトモードも追加するよい機会だと気づきました。サイトのスタイルと切り替え機能をすでに更新しているため、ライトモードも含めるのが自然です。エージェントの作業に指示を出し、ハイコントラストモードに加えてライトモードも実装してもらいます。

## Copilot cloud agent のタスクを確認する

Copilot cloud agent に割り当てたタスクの現在の状況を確認します。

1. [Copilot agents ページ](https://github.com/copilot/agents)を開きます。
2. メインペインと左ペインの両方に表示されるタスク一覧を確認します。次のようなタスクが表示されます。
   - コードベースのドキュメントを更新する
   - ゲームを変更するための API を生成する
   - Web サイトにハイコントラストモードを追加する
3. 実行中のタスクを 1 つ選択し、Copilot が実行した作業を確認します。次のような操作が含まれます。
   - リポジトリからコードをチェックアウトする
   - Copilot の作業環境を作成する
   - MCP サーバーを設定する
   - タスクを完了するための各種手順を実行する

> [!NOTE]
> 表示される手順は、Copilot の作業状況や採用した方法によって異なります。

4. 右側に表示される pull request (PR) ペインも確認します。ここから PR と変更されたファイルを確認し、追加で監視できます。

## cloud agent に指示を出す

実行中のタスクを確認したので、ハイコントラストモードに取り組んでいる Copilot にライトモードの切り替えも追加するよう依頼します。

1. ハイコントラストモードを追加しているセッションを選択します。正確なタイトルは Copilot が付けた名前や作業状況によって異なります。

    ![Mission Control のアクセシビリティセッション](../../../_images/ex6-accessibility-session.png)

2. Copilot がセットアップを完了して作業を開始するまで、数分間セッションを確認します。作業開始後は、次の例に似たメッセージが表示されます。
3. **Steer active session while Copilot is working** ダイアログに、次のプロンプトを入力します。

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Copilot agents ページで Steer active session while Copilot is working ダイアログが強調表示されたスクリーンショット](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot がプロンプトを受け取り、作業に含めたことを確認します。

## Copilot の作業を待つ

前の作業と同じように、Copilot は更新されたタスクに取り組みます。メッセージを送信した時点で進行中だった手順を終えた後、新しい要求を作業に取り込みます。

今回も完了まで数分かかります。これまでに学習した内容を振り返りましょう。

## まとめと次のステップ

このレッスンでは、GitHub Copilot cloud agent のタスクを監視してガイドする中心的な場所である Copilot agents ページを確認しました。Mission Control から、進行中と完了済みのタスクを追跡し、実行中の作業をレビューし、進行中のタスクの範囲を変更したり追加のガイダンスを提供したりできます。

次の内容を確認しました。

- Mission Control とエージェントページで cloud agent のタスクを監視する
- 進行中のセッションに指示を出し、機能を追加で依頼する

Copilot がアクセシビリティ機能に取り組む間に、[Copilot が作成した pull request を改善][next-lesson]しましょう。

## リソース

- [Copilot agents ページ][agents-page]
- [カスタムエージェントについて][custom-agents]

---

| [← 前のレッスン: カスタムエージェント][previous-lesson] | [次のレッスン: Copilot の作業を改善する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
