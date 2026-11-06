---
title: "レッスン 5 - quality-checks スキルをカスタマイズして使う"
description: "既存の quality-checks スキルを確認し、結果のレポート形式をカスタマイズして実行します。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

コードを書くこと以外にも、必要な作業はあります。これまではコードが手動で動作することを確認し、指示ファイルで規約に従うようにしてきました。テストや lint、継続的インテグレーション (CI) のほかの作業はどうでしょうか。

このような作業には **エージェントスキル**が適しています。スキルを使うと、こうした操作を適切に実行する方法を Copilot に伝えられます。

このレッスンでは、次の作業を行います。

- 既存の `quality-checks` スキルを確認します。
- 結果のレポート形式をカスタマイズします。
- スキルを再読み込みして実行します。

## シナリオ

Tailspin Toys では、単体テスト、lint、型チェックに `quality-checks` スキルを使っています。チームは、結果を読みやすくするためにレポートを改善したいと考えています。

## 指示、スクリプト、リソース

エージェントスキルには、再利用可能なタスクの手順、実行可能なスクリプト、補助リソースをまとめられます。基本となるのは、スキル名のフォルダーと、その中の `SKILL.md` です。Markdown ファイルには、スキル名と説明を定義する frontmatter、スキルの概要、呼び出すタイミングのガイダンスを記載します。スキルフォルダーには、スクリプトやその他のリソースを含むサブフォルダーも置けます。

> [!NOTE]
> スキルに追加のフォルダーやファイルは必須ではありません。Tailspin Toys の `quality-checks` スキルには、プロジェクトにある既存のコマンドを使うため、`SKILL.md` だけが含まれています。

スキルは、チームで共有して再利用できるようプロジェクトの `.github/skills` フォルダーに配置するか、ユーザーの `~/.copilot/skills` フォルダーに配置できます。

## スキルを確認する

Tailspin Toys のチームが単体テスト、lint、型チェックに使う `quality-checks` スキルを確認しましょう。

1. Codespace に戻り、Codespaces エディターで `.github/skills/quality-checks/SKILL.md` を開きます。
2. ファイル上部の `name` と `description` を読みます。説明は、Copilot がスキルを呼び出すタイミングを判断する助けになります。
3. 指示を読み、テストと lint の手順を Copilot にどう案内するか確認します。
4. まだ **Results output formatting** セクションがないことを確認します。

## 変更前にスキルを実行する

スキルは Copilot CLI から直接呼び出すことも、自然言語で依頼することもできます。3 つのチェックを実行するよう Copilot に依頼します。

1. Interactive モードでフィルター機能の会話に戻ります。
2. 次のプロンプトを使います。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 完了時に表示されるレポートを確認します。

## レポートをカスタマイズする

実行した内容、成功したかどうか、ツールが報告した内容がわかる、より良いレポートを作成しましょう。スキルを更新してその形式で出力します。

1. `.github/skills/quality-checks/SKILL.md` に戻ります。
2. ファイルの末尾に次のセクションを追加します。

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. ファイルは自動的に保存されます。

## 更新したスキルを実行する

変更したスキルを実行して、レポートを確認しましょう。Copilot CLI では、会話を再起動せずに編集済みのスキルを再読み込みできます。

1. 次のコマンドを入力します。

   ```plaintext
   /skills reload
   ```

2. 前と同じプロンプトを使います。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 完了時のレポートを確認し、最初のレポートと比較します。

## まとめと次のステップ

既存のエージェントスキルをカスタマイズして使いました。このレッスンでは、単体テスト、lint、型チェック用の `quality-checks` スキルを確認し、結果の形式をカスタマイズして、再読み込み後に実行しました。

この変更はフィルター機能の PR に含めます。次は [Playwright MCP サーバー][next-lesson]を通じて、Copilot がサイトと直接やり取りできるようにします。

## スキルの例

コミュニティの次の例は参考資料であり、追加の演習ではありません。

- [Agent Skills 仕様][skill-spec]
- [コントリビューションのワークフロー: `make-repo-contribution`][contribution-example]
- [要件ドキュメント: `prd`][prd-example]
- [図とエクスポートスクリプト: `drawio`][drawio-example]
- [ブラウザーテスト: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
