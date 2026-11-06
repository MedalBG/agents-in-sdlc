---
title: "レッスン 10 - 振り返りと次のステップ"
description: "Copilot CLI のワークフロー、2 件の pull request、再利用可能なカスタマイズ、追加リソースを振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

GitHub Copilot CLI を使い、Tailspin Toys の一連のワークフローに取り組みました。次の作業を行いました。

- Codespace を準備し、Copilot CLI をインストールし、プロジェクトを確認して、登録済みのフィルター機能の Issue を見つけました。
- 星評価を追加し、転送されたブラウザーで結果を確認して、最初の pull request (PR) を手動でマージしました。
- フィルター機能の Issue から開始し、Plan モードでアプローチを決め、Autopilot モードで構築して、Interactive モードでレビューしました。
- カスタム指示でエージェントをガイドし、既存の `quality-checks` スキルをカスタマイズして、単体テスト、lint、型チェックを実行しました。
- Playwright Model Context Protocol (MCP) server を追加し、実際のブラウザーでフィルター機能を確認しました。
- quality assurance (QA) カスタムエージェントを作成して選択し、要件、カバレッジ、スキルの結果、ブラウザーで得た証拠を評価しました。
- フィルター機能の変更全体をレビューし、フィルター機能の PR に Agent Merge を許可しました。
- コンテキスト、モデル、共有、cloud agent への任意の委任に使うスラッシュコマンドを確認しました。

## リリースしたもの

このワークショップでは、更新済みの `main` からそれぞれ独自のブランチを作成し、2 件の PR を行います。

1. **星評価:** ゲームカードに既存の `starRating` と未評価の場合の状態を明示して表示します。
2. **フィルター機能と品質改善:** フィルター機能を実装し、指示を更新して機能に適用します。`quality-checks` の報告形式をカスタマイズし、QA プロファイルを作成して関連するテストを追加します。

フィルター機能の計画から PR の作成まで、同じ会話とブランチを使用しました。ワークショップを簡潔にするため、これらの作業を 1 件の PR にまとめています。

## さまざまな検証方法

自動テスト、自分で行うブラウザー確認、MCP を介した Copilot によるブラウザー確認など、複数の方法でコードを検証しました。`quality-checks` スキルは単体テスト、lint、型チェックを実行し、新しい形式で結果を報告しました。QA はこれらの結果に要件とテストカバレッジのレビューを加え、PR の前にまとめて確認しました。

追加したテストで実際の不足を埋める必要があります。QA の実行時にテストの追加が不要な場合もあります。マージを許可する前にコードと検証の証拠を確認し、変更後は影響を受ける検証結果を更新してください。

## ベストプラクティス

Copilot に与えるコンテキストとツールが、作業の結果を左右します。このワークショップでは、指示を更新し、スキルをカスタマイズし、QA プロファイルを作成し、MCP サーバーを設定しました。会話をまたいでカスタマイズを再利用し、チームのニーズに合わせて調整できます。指示は標準を定め、スキルは繰り返し実行するタスクを説明し、カスタムエージェントは専門家の役割を定義し、MCP サーバーは外部ツールに接続します。エージェントの要約だけでなく、実際の変更とツールの結果を確認してください。

タスクに合った**モードとモデル**を選びます。作成前にアプローチを考えるには **Plan**、範囲の明確な変更を一緒に進めるには **Interactive**、十分に範囲を絞ったタスクを任せるには **Autopilot** を使います。定型的な編集には高速なモデルを、複雑な作業にはより高性能なモデルを選びます。

インフラストラクチャと同じくらいコンテキストも重要です。何を、なぜ、どのように作りたいかを明確に伝えることで、結果は大きく変わります。

## さらに試す

主要なワークフローを学びました。Copilot CLI には、ほかにも確認する価値のある機能があります。

- `/review` で code review エージェントに変更を分析させます。
- `/rubber-duck` で問題を整理し、別の視点を得ます。
- `/fleet` で独立したサブタスクを並行して調整します。
- `/worktree` で別のタスクを分離します。
- `/delegate` で Copilot cloud agent にタスクを送ります。

## 次のステップ

どのツールでも、上達する最善の方法は使い続けることです。本番コード、趣味のコード、何年も考えていたけれどまだ作っていない小さなアプリなどで活用してください。学んだことをチームと共有し、チームの経験からも学びましょう。いつものように、ドキュメントも確認してください。

ターミナルから Tailspin Toys をさらに拡張したい場合は、オプションの[Foundry Backer Concierge シリーズ][foundry]に進みます。ほかの環境と比較するには、[VS Code ワークショップ][vscode]、[GitHub Copilot app ワークショップ][app]、[Copilot cloud agent ワークショップ][cloud]を確認してください。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンドリファレンス][cli-reference]
- [Copilot CLI のカスタマイズ][customize-cli]
- [Copilot CLI で pull request を管理する][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
