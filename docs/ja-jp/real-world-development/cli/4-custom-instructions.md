---
title: "レッスン 4 - カスタム指示で Copilot をガイドする"
description: "リポジトリの指示を確認し、ドキュメント標準を追加して、フィルター機能のコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を使う際、コンテキストは重要です。タスクを特定の方法で実行する必要がある場合は、そのガイダンスを Copilot が参照できるようにします。[指示ファイル][instruction-files]には、求めるコードだけでなく、その構造や作成方法も記載できます。フィルター機能を実装したので、Copilot が参照した指示を確認し、ドキュメント標準を追加して、コードに適用します。

このレッスンでは、次の作業を行います。

- リポジトリの指示ファイルと、パスを指定した指示ファイルがエージェントに読み込まれる方法を確認します。
- コーディング標準に従うよう指示ファイルを更新します。
- 指示ファイルがコードに与える影響を確認します。

## シナリオ

Tailspin Toys には、開発プラクティスに関するガイドラインと要件があります。具体的には、次のとおりです。

- コメントでは、コードの内容を繰り返すのではなく、目的や自明でない判断を説明します。
- `db/` と `src/lib/` のエクスポートされた関数では、目的、パラメーター、戻り値を TSDoc/JSDoc で記述します。`db` 引数を注入できる場合は、その点も説明します。
- 再利用可能な Astro コンポーネントでは `Props` コントラクトを説明し、関連するコードを変更した場合はコメントも更新します。
- 既存の書式設定と lint のガイダンスを維持します。

指示ファイルを使い、Copilot がこれらのプラクティスに沿って作業できるようにします。

## 指示ファイル

カスタム指示は、コーディングスタイルや要件を Copilot に伝え、より適切な提案やコードスニペットを得るために使います。コーディング規約、ライブラリ、コメントの書き方などを指定できます。リポジトリ全体、または特定の種類のファイルを対象にできます。

指示ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリへの**すべての**要求に対して Copilot に送信される単一の指示ファイルです。多くの要求に関係するプロジェクトレベルの情報を記載します。
- `.github/instructions/*.instructions.md` は、特定の言語、ファイルの種類、タスクに関するガイドラインを記載します。

> [!NOTE]
> ほかの指示ファイルの形式や、各環境でのサポート状況は異なります。特定の形式を使う前に、[カスタム指示のサポート状況][custom-instructions-support]を確認してください。

## このプロジェクトのカスタム指示ファイルを確認する

スターター プロジェクトには、すでに指示ファイルが用意されています。変更を加える前に内容を確認し、影響を把握しましょう。

1. Codespace に戻ります。
2. Codespaces のエディターで `.github/copilot-instructions.md` を開きます。
3. プロジェクトの概要とコーディングガイダンスを確認します。この指示は、このリポジトリでの Copilot とのすべてのやり取りに適用されます。
4. `.github/instructions` フォルダーを開き、Astro、Drizzle データレイヤー、テストなどの指示を確認します。
5. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドが、適用対象を決める glob です。
6. `.github/instructions/drizzle.instructions.md` を開き、ほかの指示ファイルやプロジェクト内の既存ファイルへのリンクを確認します。大きな指示を小さく再利用可能なファイルに分け、Copilot が参考にする例を示せます。

## チームのガイダンスに合わせて指示ファイルを更新する

指示ファイルは出発点として適切ですが、TypeScript にドキュメントコメントを追加する標準がありません。`copilot-instructions.md` を編集し、新しく生成する TypeScript の TSDoc コメントを必須にします。

1. `.github/copilot-instructions.md` で、35 行目付近にある **Code formatting guidance** セクションを探します。
2. 最後の箇条書きに、次の行を追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

ファイルは自動的に保存されます。

## 更新したガイダンスを使う

Copilot CLI は会話の開始時にリポジトリの指示を読み込みます。フィルター機能のコンテキストを保ったまま新しい指示を読み込むため、変更を保存した後で会話を終了し、再開します。

1. フィルター機能の会話が開いている場合は、`/exit` を入力して終了します。Copilot CLI は会話を自動的に保存します。
2. `game-filters-cli` ブランチのリポジトリルートで、保存した会話を再開します。

   ```bash
   copilot --resume
   ```

   選択を求められた場合は、フィルター機能の会話を選び、`game-filters-cli` ブランチがチェックアウトされていることを確認します。
3. Copilot に次のように依頼し、更新したガイダンスを適用します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. `/diff` を入力し、変更された TypeScript ファイルを確認します。生成された TSDoc コメントがコードを正確に説明していることを確認します。

## まとめと次のステップ

Copilot CLI が指示ファイルからコンテキストを取得する方法を確認し、新しい標準を機能に適用しました。具体的には、次の作業を行いました。

- リポジトリの指示ファイルと、パスを指定した指示ファイルがエージェントに読み込まれる方法を確認した。
- コーディング標準に従うよう指示ファイルを更新した。
- 指示ファイルがコードに与える影響を確認した。

次は、[再利用可能な quality-checks スキルをカスタマイズして実行し][next-lesson]、lint とテストを一貫して実行する方法を確認します。

## リソース

- [Copilot CLI のカスタム指示の追加][instruction-files]
- [カスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Awesome Copilot — 指示ファイルなどを集めたリソース集][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
