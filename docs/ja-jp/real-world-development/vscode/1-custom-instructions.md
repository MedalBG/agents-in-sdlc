---
title: "演習 1 - カスタム指示 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: エージェントモード →][next-lesson]

生成 AI を使う際、コンテキストは重要です。タスクを特定の方法で実行する必要がある場合や、Copilot に知っておいてほしい背景情報がある場合は、そのコンテキストを参照できるようにします。[指示ファイル][instruction-files]を使うと、コード生成に必要なガイダンスを Copilot に伝えられます。

この演習では、次の作業を行います。

- リポジトリのカスタム指示とパスを指定した指示ファイルを使い、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント標準を Copilot に伝える方法を確認します。
- 現在の指示を使って、フィルター機能の最初のデータ部分 (publishers ヘルパー) を作成します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加します。
- 同じプロンプトを再実行し、生成されたコードに新しい標準が反映されることを確認します。
- 指示の更新とフィルター機能の一部を `main` にコミットし、次の演習で新しいガイダンスを使えるようにします。

> [!CAUTION]
> 生成コードが設定した標準と異なる場合があります。Copilot の動作は確率的です。この演習では、指示の更新による傾向の変化を確認します。生成結果を文字単位で一致させることが目的ではありません。

## 指示ファイル

### シナリオ

Tailspin Toys には、開発プラクティスに関するガイドラインと要件があります。具体的には次のとおりです。

- データレイヤーには、必ず単体テストを用意します。
- UI はダークモードにして、モダンな見た目にします。
- コードには TSDoc のドキュメントコメントを追加します。
- 各ファイルの先頭に、ファイルの役割を説明するコメントブロックを追加します。

指示ファイルを使い、これらのプラクティスに沿って Copilot が作業できるよう、必要な情報を提供します。

### カスタム指示

カスタム指示を使うと、コーディングスタイルや要件を Copilot に伝え、より適切な提案やコードスニペットを得られます。コーディング規約、ライブラリ、コメントの書き方などを指定できます。リポジトリ全体、または特定の種類のファイルを対象にできます。

指示ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリへの**すべての**要求に対して Copilot に送信される単一の指示ファイルです。多くの Copilot Chat や CLI の要求に関係するプロジェクトレベルのコンテキストを記載します。技術スタック、開発中の内容、ベストプラクティス、全体に適用するガイダンスなどが該当します。
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
> GitHub Copilot を使うすべてのプロジェクトに、充実した指示ファイルを用意しましょう。このプロジェクトには、[UI 更新][ui-instructions]や [Astro][astro-instructions]など、さまざまなタスク用のファイルがあります。
>
> Copilot に指示ファイルの生成を依頼することもできます。VS Code の **Configure Chat → Generate Agent Instructions** や Copilot CLI の `/init` など、環境によって方法が異なります。この演習では VS Code を使うため、後ほど該当する手順を説明します。
>
> テンプレートや出発点を探している場合は、指示ファイル、カスタムエージェントなどを集めた [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトの指示ファイルを確認する

このリポジトリに用意された指示ファイルを読みます。コアの `copilot-instructions.md` と、タスクごとの `*.instructions.md` ファイルがあります。エディターまたは GitHub Web UI で開いてください。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの簡単な説明と、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** の下には **GitHub Actions Workflows** に関するガイダンスもあります。これらの指示は、Copilot とのあらゆるやり取りに適用されます。
3. `.github/instructions` フォルダーを開き、Astro ファイル、Drizzle データレイヤー、テストなどの指示を確認します。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドが、適用対象を決める glob です。リポジトリルートから `**/*.test.ts` に一致する TypeScript のテストファイルなどが対象になります。
5. このプロジェクトの単体テスト作成に関する指示を確認します。
6. `.github/instructions/drizzle.instructions.md` を開き、末尾を確認します。`unit-tests.instructions.md` などの指示ファイルや、プロジェクト内の既存ファイルへのリンクがあります。大きな指示を小さく再利用可能なファイルに分け、Copilot がコードを生成するときに参考にする例を示せます。ここに記載されたパスはリポジトリルートではなく、指示ファイルからの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング標準が記載されていますが、コード内のドキュメントはまだ必須ではありません。次の手順で、TSDoc コメントとファイルヘッダーに関するルールを追加します。

## 変更用のブランチを作成する

ベストプラクティスに沿って、変更用のブランチを作成します。

1. 前の演習で使った codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. 新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## 指示を更新する前に Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に codespace に戻り、Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なるため、必要な場合は各手順で説明します。

カスタム指示の影響を確認するため、まず現在の指示ファイルを使ってプロンプトを送信します。その後、指示を更新して同じプロンプトをもう一度送り、結果の違いを確認します。

1. 前の演習で開いたエディタータブを閉じ、Copilot が必要なコンテキストだけを取得するようにします。
2. `src/lib/publishers.ts` を開き、ヘルパーを置く場所を Copilot に示します。
3. Chat ビューのエージェントドロップダウンから **Agent** を選択し、Copilot がファイルを変更できるようにします。

   ![Chat ビューでエージェントピッカーを表示したスクリーンショット](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot はプロジェクトを調べてコードを更新します。ヘルパーファイルとテストが変更されることがあります。
6. 提案されたヘルパーが、型付けされた関数で `db` クライアントを最初の引数として受け取り、型付けされた publishers の配列を返すことを確認します。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` のデータレイヤー規約に基づいています。
7. 提案されたコードに、TSDoc ドキュメントコメントとファイルレベルのコメントヘッダーが**ない**ことを確認します。

> [!CAUTION]
> Copilot は確率的に動作するため、指示がなくてもドキュメントコメントを追加する可能性があります。その場合も問題ありません。この演習では、指示の更新後に一貫性が改善されることを確認します。

## リポジトリ標準を追加する

`.github/copilot-instructions.md` は、Copilot にプロジェクトレベルの情報を提供するファイルです。コードの提案を改善するために、リポジトリのコーディング標準を記載します。

1. `.github/copilot-instructions.md` を開き直します。
2. 27 行目付近にある **Code formatting requirements** セクションを探します。ここにはプロジェクトのコーディング標準が記載されていますが、コード内のドキュメントに関するルールはありません。そのため、生成されたヘルパーにドキュメントコメントがありませんでした。
3. 既存の標準の直後に、ファイルヘッダーと TSDoc のドキュメントコメントを追加する次の行を挿入します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したように、指示ファイルはリポジトリレベル (`.github/copilot-instructions.md`) に作成して全体に適用することも、特定の言語、ファイルの種類、タスク向けに `*.instructions.md` として作成することもできます。今回追加したドキュメントコメントのルールのように、プロジェクト全体の標準はリポジトリレベルのファイルに記載します。

## プロンプトを再実行して変化を確認する

1. Copilot Chat に戻り、**New Chat** を選択してコンテキストをクリアします。
2. `src/lib/publishers.ts` を再度選択し、Copilot の対象ファイルを設定します。
3. 前と**同じプロンプト**を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 提案されたファイルの先頭に、次の例に似たコメントブロックがあることを確認します。

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 提案された関数に、次の例に似た TSDoc コメントがあることを確認します。

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

新しいプロジェクト標準を Copilot に伝え、次の演習で利用する実際のコードに適用しました。

## 指示とブランチをコミットする

指示ファイルもリポジトリのほかのファイルと同じように、ソース管理で管理します。変更をコミットしてブランチをリポジトリにプッシュします。

1. <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
2. ターミナルで `git status` を実行し、指示の更新とヘルパーの変更があることを確認します。

   ```bash
   git status
   ```

3. 指示の更新と、ヘルパーの基礎を作成するため Copilot が変更したファイルをステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # git status にテストなどの追加変更が表示された場合は、それらのファイルも追加します。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. ブランチをリポジトリにプッシュします。

   ```bash
   git push -u origin custom-instructions
   ```

## pull request を作成してマージする

ブランチをプッシュしたら、バックログにあるドキュメント標準の Issue に関連付けて pull request を作成します。GitHub ツールはプロジェクトに接続済みのため、Copilot に Issue を探し、Issue をクローズする PR を作成してマージするよう依頼できます。

> [!NOTE]
> この演習では初めて Copilot に GitHub 上の操作を任せます。プロジェクトのテンプレートには GitHub ツールがあらかじめ設定されています。Copilot が初めて使うときに、VS Code で GitHub へのサインインを求められることがあります。画面の指示に従って許可してください。この接続方法である Model Context Protocol については、後の演習で学びます。

1. codespace で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、エージェントピッカーで **Agent** が選択されていることを確認します。
3. 指示ファイルを更新する Issue を探し、現在のブランチから PR を作成して、指示の更新と新しい publishers ヘルパーの基礎の両方を説明するよう Copilot に依頼します。

   ```plaintext
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot が Issue を探して PR を作成します。
5. Copilot に GitHub 操作を許可するよう求められたら、コマンドを確認してから **Allow** を選択します。
6. PR が作成されたら、次のプロンプトでマージしてローカルブランチを `main` に戻すよう依頼します。

   ```plaintext
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Copilot に GitHub 操作とシェルコマンドの実行を許可するよう求められたら、コマンドを確認してから **Allow** を選択します。

GitHub Copilot の支援で pull request を作成してマージできました。

## まとめと次のステップ

このプロジェクトで Copilot が指示ファイルからコンテキストを取得する方法を確認し、VS Code の Copilot Chat を使って、次の作業を行いました。

- 既存の指示でコード生成のプロンプトを送り、Copilot の出力を確認した。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加した。
- 同じプロンプトを再実行し、提案されたコードに新しい標準が反映されることを確認した。
- 指示とヘルパーの基礎を `main` にコミットし、次の演習で使えるようにした。

次は、Copilot がコードベース全体に新機能を追加するときに、[エージェントモードで指示を適用します][next-lesson]。

## リソース

- [GitHub Copilot のカスタマイズに使う指示ファイル][instruction-files]
- [カスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Copilot 用のカスタム指示を改善する 5 つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用カスタム指示][personal-instructions]
- [Awesome Copilot — 指示ファイルなどを集めたリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
