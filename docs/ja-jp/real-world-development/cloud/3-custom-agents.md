---
title: "演習 3 - カスタムエージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタムエージェントとは

GitHub Copilot の[カスタムエージェント][custom-agents-concept]を使うと、開発ワークフローにおける特定のタスクや分野に合わせた AI アシスタントを作成できます。プロジェクトの `.github/agents` フォルダーに Markdown ファイルを置き、エージェントを定義します。指示、ベストプラクティス、コーディングパターン、分野固有の知識を指定すると、Copilot は特定の作業をより効果的に行えます。チームの専門知識を再利用できるエージェントとしてまとめることもできます。たとえば、[WCAG][wcag] に準拠したアクセシビリティエージェント、安全なコーディングプラクティスに従うセキュリティエージェント、一貫したテストパターンを維持するテストエージェントなどです。

カスタムエージェントは、プロジェクトの `.github/agents` フォルダー、またはグローバルな `~/.copilot/agents` フォルダーにある Markdown ファイルで定義します。各ファイルには、少なくとも `name` と `description` を含む YAML フロントマターと、エージェントの動作、専門知識、指示を記載する Markdown プロンプトが必要です。

### カスタムエージェントとエージェントスキルの違い

カスタムエージェントと[エージェントスキル][agent-skills-concept]には、役割の重なる部分があります。どちらも主に Markdown ファイルで定義され、AI に作業の方法を伝えます。最も簡単な区別は、**カスタムエージェントが作業者で、スキルがツール**だということです。

カスタムエージェントは独自のコンテキストウィンドウを持ち、スキルやほかのエージェントを組み合わせて作業を進めます。このラボのアクセシビリティカスタムエージェントは、アクセシビリティガイドラインに沿ってサイトをレビューし、更新します。その作業の中で、pull request ワークフロー用スキルや、テストを実行・管理するスキルを呼び出すこともできます。

> [!NOTE]
> カスタムエージェントの作成方法は 1 つではありません。AI のほかの用途と同じように、使う環境やシナリオに合わせて試し、改善してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

カスタムエージェントでは、次の内容を確認します。

- カスタムエージェントの定義方法
- カスタムエージェントへのタスクの割り当て

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、すべてのユーザーがクラウドファンディングプラットフォームを利用できるように取り組んでいます。最近、現在のダークテーマはテキストと背景色のコントラストが不十分で、読みにくいというフィードバックがありました。このアクセシビリティ上の課題に対応するため、デザインチームから、ユーザーが切り替えられるハイコントラストモードの実装を依頼されました。

アクセシビリティは重要なため、できるだけ早く実装する必要があります。カスタムエージェントに機能を生成させます。

## アクセシビリティカスタムエージェントを確認する

アクセシビリティ用のカスタムエージェントはすでに用意されています。内容を確認し、Copilot への指示を把握しましょう。

codespace に戻り、アクセシビリティカスタムエージェントのファイルを確認します。

1. `.github/agents/accessibility.md` を開きます。
2. `name` と `description` を含む YAML フロントマターを確認します。

> [!CAUTION]
> `name` と `description` のフロントマターは、カスタムエージェントに必須です。

3. 続くセクションを読み、次の内容を確認します。
   - アクセシブルな Web サイトのコード生成における主要な役割
   - アクセシビリティのベストプラクティス
   - HTML、CSS、JavaScript のコード例
   - よくある問題と避けるべきミス

## Issue を作成して割り当てる

Mission Control は、環境内のすべてのエージェントを管理する場所です。Copilot cloud agent にタスクを割り当て、進行状況を監視し、方向を変えたり追加のガイダンスを提供したりできます。まず、ハイコントラストモードの作成を Copilot に割り当てます。

1. リポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue のダイアログを開きます。
4. **Blank issue** を選択して、新しい Issue を作成します。
5. **Title** に `Add high contrast mode to website` と入力します。
6. **Description** に次のテキストを入力します。

    ```plaintext
    We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
    ```

7. **Create** を選択して Issue を作成します。
8. 右側にある **Assign to Copilot** を選択して、割り当てダイアログを開きます。
9. 一覧から **Accessibility agent** を選択します。

    ![カスタムエージェントの割り当て画面で、Accessibility agent が選択されているスクリーンショット](../../../_images/ex5-select-custom-agent.png)

10. **Assign** を選択します。
11. Copilot がバックグラウンドで作業を開始します。

## まとめと次のステップ

このレッスンでは、特定のタスクや分野向けの AI アシスタントである、GitHub Copilot の[カスタムエージェント][custom-agents]について学びました。カスタムエージェントを使うと、チームの専門知識や標準を再利用可能な形にまとめ、特定の作業をより効果的に行えるよう Copilot を導けます。

次の内容を確認しました。

- カスタムエージェントの定義方法
- カスタムエージェントへのタスクの割り当て

Copilot がハイコントラストモードを実装する間に、[Mission Control からエージェントセッションを監視して指示する][next-lesson]方法を確認しましょう。

## リソース

- [カスタムエージェントについて][custom-agents]
- [Organization でカスタムエージェントを使うための準備][org-custom-agents]
- [Enterprise でカスタムエージェントを使うための準備][enterprise-custom-agents]

---

| [← 前のレッスン: GitHub Copilot cloud agent][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
