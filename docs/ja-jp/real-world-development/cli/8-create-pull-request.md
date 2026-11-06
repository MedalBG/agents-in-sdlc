---
title: "レッスン 8 - 機能の PR を作成してマージする"
description: "フィルター機能、指示、スキル、QA プロファイル、テストをまとめてレビューし、PR を作成して Agent Merge を使用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

フィルター機能の実装、指示の更新、スキルの更新、品質保証 (QA) プロファイル、テストは、すべて 1 つのブランチに保存されています。まとめてレビューし、pull request を作成しましょう。星評価の pull request (PR) は自分でマージしました。今回は **Agent Merge** にマージプロセスを管理させます。

> [!NOTE]
> 通常は、機能、指示の更新、スキルの更新、QA エージェントを複数の PR に分けます。ワークショップを簡潔にするため、フィルター機能と品質改善の作業全体を 1 つの会話とブランチで進め、この PR にまとめました。

このレッスンでは、次の作業を行います。

- Agent Merge と、マージのライフサイクルを自動化する仕組みを確認します。
- 機能全体と検証の証拠を調べます。
- フィルター機能の PR を作成します。
- レビュー後に Agent Merge を有効にし、PR がマージされたことを確認します。

## シナリオ

フィルター機能の一連の作業で、Copilot を使って機能を計画、実装、検証しました。Tailspin Toys は、マージの承認を開発者が管理しながら、残りの PR 作業を自動化したいと考えています。

## Agent Merge について

**Agent Merge** は、pull request をマージするために必要な残りの作業を自動化します。有効にすると、Copilot は PR を妨げている問題に対応します。たとえば、失敗した継続的インテグレーション (CI) チェックの修正、レビューコメントへの対応、必要な場合のリベースなどを行い、リポジトリで許可されていれば GitHub の自動マージを有効にします。

これまでは自分で **Merge pull request** を選択していました。Agent Merge にその作業を任せることもできます。Agent Merge を有効にする前に、変更をレビューして準備ができていることを確認してください。

## Agent Merge で PR を管理する

コードの作成とレビューが終わったら PR を作成し、残りのプロセスを Agent Merge に任せます。

1. Codespace に戻ります。
2. `/agent` を入力してエージェントダイアログを開きます。
3. 一覧から **Default** を選択し、<kbd>Enter</kbd> を押します。
4. `/pr create` コマンドで新しい PR を作成します。
5. `/pr agentmerge` コマンドで Agent Merge を有効にします。
6. Copilot は PR の継続的インテグレーションを監視し、すべて成功したらマージします。
7. Copilot から `PR #14 was squash-merged successfully.` のようなメッセージが届いたことを確認します。

> [!IMPORTANT]
> Agent Merge は、必須の承認、ブランチ保護、マージキュー、リポジトリ設定、権限の不足を回避しません。ブロックされた場合は報告された理由を確認し、リポジトリで許可されるときに、レビュー済みのマージを手動で完了します。

## まとめと次のステップ

コードの生成、テストと検証、pull request のプロセスなど、開発プロセスの複数の段階を自動化しました。具体的には、次の作業を行いました。

- Agent Merge と、マージのライフサイクルを自動化する仕組みを確認した。
- 機能全体と検証の証拠を調べた。
- フィルター機能の PR を作成した。
- レビュー後に Agent Merge を有効にし、PR がマージされたことを確認した。

次は、コンテキスト、モデル、共有、cloud agent への任意の委任に役立つ[スラッシュコマンド][next-lesson]を確認します。

## リソース

- [Copilot CLI で pull request を管理する][manage-prs]
- [Copilot CLI コマンドリファレンス][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
