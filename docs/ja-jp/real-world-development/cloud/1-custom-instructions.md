---
title: "演習 1 - カスタム指示 (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot cloud agent →][next-lesson]

生成 AI を使う際は、コンテキストが重要です。作業の方法が決まっている場合や、Copilot に知っておいてほしい背景情報がある場合は、そのコンテキストを参照できるようにします。[指示ファイル][instruction-files]を使うと、Copilot に「何を」するかだけでなく、「どのように」行うかも伝えられます。

この演習では、次の作業を行います。

- リポジトリのカスタム指示とパス指定の指示ファイルを使い、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント標準を Copilot に伝える方法を確認します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加します。

> [!NOTE]
> VS Code や CLI のハーネスとは異なり、この演習ではプロンプトの実行前後を比較しません。Copilot cloud agent は GitHub Issue に対して非同期で動作するため、リアルタイムで影響を並べて示すのは困難です。後の演習で cloud agent が作成した pull request をレビューする際に、指示ファイルの影響を確認します。

## 指示ファイル

### シナリオ

Tailspin Toys には、開発のガイドラインと要件があります。具体的には次のとおりです。

- データレイヤーには、必ず単体テストを用意します。
- UI はダークモードにして、モダンな見た目にします。
- コードには TSDoc のドキュメントコメントを追加します。
- 各ファイルの先頭に、ファイルの役割を説明するコメントブロックを追加します。

指示ファイルを使い、これらのプラクティスに沿って作業できるよう Copilot に必要な情報を提供します。

### カスタム指示

カスタム指示を使うと、コーディングスタイルや要件を Copilot に伝え、より適切な提案やコードスニペットを得られます。好みのコーディング規約、ライブラリ、コメントの書き方などを指定できます。リポジトリ全体、または特定の種類のファイルに対する指示を作成できます。

指示ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリへの**すべての**要求に対して Copilot に送信される単一の指示ファイルです。多くのチャット要求や CLI 要求に関係するプロジェクトレベルのコンテキストを記載します。使用する技術スタック、開発中の内容、ベストプラクティス、全体に適用するガイダンスなどが該当します。
- `.github/instructions/*.instructions.md` は、特定のタスクやファイルの種類に対するガイドラインを記載できます。TypeScript や Astro などの言語、UI コンポーネントの作成、単体テストの追加などに利用できます。

> [!NOTE]
> IDE で作業する場合、指示ファイルは Copilot Chat のコード生成にのみ使用されます。コード補完や次の編集候補には適用されません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、コード生成時にリポジトリレベルの指示と `applyTo` フロントマター付きの `*.instructions.md` ファイルを使用します。
>
> Copilot は AGENTS.md や CLAUDE.md など、[ほかの標準の指示ファイル][custom-instructions-support]にも対応しています。

### 指示ファイルを管理するためのベストプラクティス

指示ファイルの作成について詳しく説明することは、このワークショップの範囲外です。ただし、サンプルプロジェクトに含まれるファイルは、一般的な管理方法を示しています。

- `copilot-instructions.md` には、開発中の内容、プロジェクトの構造、全体に適用するコーディング標準など、プロジェクトレベルのガイダンスを記載します。
- `*.instructions.md` ファイルには、単体テスト、Astro コンポーネント、データレイヤーなどのファイル種類や、特定のタスクに関する指示を記載します。
- 自然な言葉を使い、明確なガイダンスを記述します。コードの良い例と悪い例を示します。

指示ファイルの作成方法は 1 つではありません。AI の使い方と同じように、試行を重ねながらプロジェクトに合う方法を見つけてください。

> [!TIP]
> GitHub Copilot を使うプロジェクトには、充実した指示ファイルを用意しましょう。このプロジェクトには、[UI 更新][ui-instructions]や [Astro][astro-instructions]など、さまざまなタスク用のファイルがあります。
>
> Copilot に指示ファイルの生成を依頼することもできます。各環境で手順は異なります (例: VS Code の **Configure Chat → Generate Agent Instructions**、Copilot CLI の `/init`)。その環境に関連するレッスンで詳しく説明します。
>
> テンプレートや出発点を探している場合は、指示ファイル、カスタムエージェントなどを集めた [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム指示ファイルを確認する

このリポジトリに用意されている指示ファイルを確認します。コアの `copilot-instructions.md` と、さまざまなタスクに対応する `*.instructions.md` ファイルがあります。エディターまたは GitHub Web UI で開いてください。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの簡単な説明と、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。これらの指示は、Copilot を使うあらゆる作業に適用されます。
3. `.github/instructions` フォルダーを開き、Astro ファイル、Drizzle データレイヤー、テストなどの指示を確認します。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドが、適用対象のファイルを指定する glob です。この例では `**/*.test.ts` に一致するファイルなど、TypeScript のテストファイルに適用されます。
5. このプロジェクトの単体テスト作成に関する指示を確認します。
6. `.github/instructions/drizzle.instructions.md` を開き、ファイルの末尾を確認します。`unit-tests.instructions.md` などの指示ファイルや、プロジェクト内の既存ファイルへのリンクがあります。大きな指示を再利用できる小さなファイルに分け、Copilot がコードを生成するときに参考にする例を示せます。ここに記載されたパスはリポジトリのルートではなく、指示ファイルからの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング標準が記載されていますが、コード内のドキュメントはまだ必須ではありません。次の手順で、TSDoc のコメントとファイルヘッダーに関するルールを追加します。

## リポジトリ標準を追加する

この演習で編集するのは、プロジェクト全体に適用する標準を 1 つ追加する作業です。コード内のドキュメントとして TSDoc コメントとファイルレベルのコメントヘッダーを追加するよう指示します。後の演習で Issue を割り当てると、cloud agent がこの標準を参照します。

編集の前に作業用ブランチを作成します。cloud agent は Issue の対象ブランチから指示を読み込みますが、ここでは変更をきちんとコミットします。

1. codespace のターミナルで、新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

`.github/copilot-instructions.md` は、Copilot にプロジェクトレベルの情報を提供するファイルです。コーディング標準を記載して、コードの提案を改善しましょう。

1. `.github/copilot-instructions.md` を開き直します。
2. 27 行目付近にある **Code formatting requirements** セクションを探します。プロジェクトのコーディング標準が記載されていますが、コード内のドキュメントに関するルールはありません。そのため、生成されたヘルパーにドキュメントコメントがありませんでした。
3. 既存の標準の直後に、ファイルヘッダーと TSDoc のドキュメントコメントを追加するため、次の行を追加します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したように、指示ファイルはリポジトリレベル (`.github/copilot-instructions.md`) に作成して全体に適用することも、特定の言語、ファイルの種類、タスク向けに `*.instructions.md` として作成することもできます。追加したドキュメントコメントのルールのように、プロジェクト全体の標準はリポジトリレベルのファイルに記載します。

## 指示の変更をコミット、プッシュ、マージする

cloud agent は、Issue の対象となるブランチの指示ファイルを読み込みます。次の演習で Issue を Copilot に割り当てると、Copilot は `main` からブランチを作成します。cloud agent が指示を参照できるように、変更を `main` にマージしてください。

1. 変更をステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチをプッシュします。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` に向けて pull request を作成し、マージします。次の演習で Issue に割り当てた作業を行う際に、cloud agent はこれらの指示を読み込みます。

> [!TIP]
> このワークショップでは、`main` に直接コミットしてもかまいません。その場合はブランチの手順を省略してください。実際のプロジェクトと同じ流れを体験できるよう、ここではブランチを使っています。

## まとめと次のステップ

このプロジェクトで Copilot が指示ファイルからコンテキストを取得する方法を確認し、`.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加しました。残りのハーネスで cloud agent が作成する pull request に、この標準が反映されます。

次は、[最初の Issue を Copilot cloud agent に割り当てます][next-lesson]。

## リソース

- [GitHub Copilot のカスタマイズに使う指示ファイル][instruction-files]
- [カスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Copilot 用のカスタム指示を改善する 5 つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — 指示ファイルなどを集めたリポジトリ][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
