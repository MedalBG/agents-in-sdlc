---
title: "演習 5 - GitHub Copilot の作業を反復して改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

## 作業をレビューする

このラボでは、ユーザー体験の改善や機能追加を目的に、GitHub Copilot と複数のタスクに取り組みました。コードにドキュメントを追加し、デザインチーム向けの関連ゲーム機能を実装し、ハイコントラストモードとライトモードを含むアクセシビリティ機能を作成するよう依頼しました。コードの変更を確認し、必要に応じて Copilot にフィードバックを送って作業を改善します。

### シナリオ

何度も説明したように、生成 AI を導入してもソフトウェア設計と DevOps の基本は変わりません。生成されたコードをレビューし、通常の DevOps プロセスに沿って進めます。チーム全体にレビューを有効にする前に、Copilot が作成したドキュメント、関連ゲーム機能、アクセシビリティ機能を確認しましょう。

## GitHub Copilot cloud agent のセキュリティ

Copilot cloud agent は非同期に、監視されない状態でタスクを実行するため、安全性を保つためのセキュリティ制約があります。

- Copilot はリポジトリへの読み取りアクセスのみを持ち、書き込みできるのは作業に使うブランチだけです。
- cloud agent は GitHub Actions 内で動作し、作業用の独立した一時環境を作成します。
- GitHub Actions ワークフローを実行するには、人間の承認が必要です。
- [MCP サーバーを含む外部リソースへのアクセスは、既定で制限されています][agent-firewall]。

## 生成されたドキュメントをレビューする

最初に、GitHub Copilot cloud agent が作成したコードドキュメントの pull request (PR) を確認します。github.com の標準的な PR インターフェイスを使います。

> [!NOTE]
> PR に、GitHub Copilot がファイアウォールでブロックされているという警告が表示されることがあります。Copilot は MCP サーバーなどの外部リソースに既定で制限付きでアクセスするため、これは想定された動作です。必要であれば、Copilot cloud agent のファイアウォールを[設定または無効化][agent-firewall]できます。

1. github.com のリポジトリに戻ります。
2. **Pull Requests** を選択して PR の一覧を開きます。
3. **Add missing documentation** など、同様のタイトルが付いた PR を開きます。

> [!NOTE]
> Copilot がまだ作業中の場合、PR のタイトルに **[WIP]** が付いています。作業が完了するまで待ちます。数分かかることがあります。その間に休憩したり、学んだことを振り返ったりしてください。

4. 準備ができたら **Files changed** タブを選び、変更をレビューします。

    ![Files changed タブ](../../../_images/shared-pr-files-changed.png)

5. TSDoc のドキュメントコメントなど、更新されたコードを確認します。実際の変更内容は状況によって異なります。Exercise 2 の最初に確認したカスタム指示ファイルが反映されているか、TSDoc コメント、TypeScript の規約、コメントヘッダーに注目してください。
6. 更新内容に問題がなければ **Conversation** タブに戻り、下にスクロールします。
7. ワークフローが承認待ちになっていることを示すインジケーターを探します。
8. 承認待ちの場合は **Approve and run workflows** を選択します。

    ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. ワークフローが PR のチェックに追加されたことを確認します。単一の Astro アプリのプロジェクトチェックが成功するまで数分かかる場合があります。

## GitHub Copilot に変更を依頼する

Copilot との PR 作業は一方通行ではありません。チームメンバーと同じように、PR のコメントやコードのインラインコメントで Copilot に言及できます。コメントを確認した Copilot は、修正するための新しいセッションを開始します。結果は確率的に変わるため、入力する文章を指定することはできません。たとえば、次のような変更を依頼できます。

- 各コードファイルの先頭に、内容を簡単に説明するコメントヘッダーを追加する。
- TypeScript と Astro ファイルに TSDoc のドキュメントコメントを追加する。
- Astro アプリの構造を説明する README を作成する。

1. 上記の例などを参考に、生成されたドキュメントの変更を依頼するコメントを追加し、ほかのユーザーと同じように **@copilot** をメンションします。
2. **View Session** を選択して Copilot の作業を確認します。Copilot が変更用の新しいセッションを開始します。
3. **Back to pull request** を選択すると PR に戻れます。

    ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Copilot の作業が完了したら、PR に新しいコミットが追加されたことを確認します。
5. **Files changed** タブを選択し、変更をレビューします。

納得できるまで改善を繰り返せます。問題がなければ、ドラフト PR をレビュー可能な状態にして `main` ブランチにマージします。

![Convert PR to ready](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

ゲーム詳細ページに関連ゲームを表示する Issue について、Copilot が作成した PR を確認します。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Show related games on the game details page** などのタイトルが付いた PR を選択します。
4. **Files changed** タブを選択し、生成されたコードをレビューします。
5. 更新に問題がなければ **Conversation** タブに戻り、下にスクロールします。
6. ワークフローが承認待ちになっていることを示すインジケーターを探します。
7. 承認待ちの場合は **Approve and run workflows** を選択します。

    ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. ワークフローが PR のチェックに追加され、単一の Astro アプリのプロジェクトチェックが成功することを確認します。完了まで数分かかる場合があります。
9. **任意:** Codespace でこのブランチに切り替えて関連ゲーム機能を手動テストできます。Codespace でターミナルを開き、Copilot が作成したブランチ名 (例: **copilot/fix-8**) に置き換えて、次のコマンドを実行します。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot が関連ゲーム機能を作成しました。cloud agent には反復して変更を依頼できます。たとえば、表示する関連ゲームの数を調整したり、この機能を割り当てた時点ではまだ追加していなかったコメントヘッダーや TSDoc コメントを追加したりできます。前と同じように、**Conversation** タブでコメントを追加すると、Copilot が内容を確認して新しいセッションを開始します。

## アクセシビリティ機能をレビューする

最後に、アクセシビリティカスタムエージェントで実装した機能を確認します。この PR には、Exercise 3 で依頼したハイコントラストモードと、Exercise 4 の Mission Control から追加を依頼したライトモードの両方が含まれているはずです。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Add high contrast mode to website** などのタイトルが付いた PR を選択します。

> [!NOTE]
> Copilot がまだ作業中の場合、PR に **[WIP]** が表示されます。作業が完了するまで待ちます。数分かかることがあります。

4. **Files changed** タブを選択して、生成されたコードをレビューします。
5. 実装を確認するときは、次の点に注目します。
   - 表示モードを切り替える UI コンポーネント
   - ユーザー設定を保存する local storage の使用
   - ハイコントラストモードとライトモードの CSS またはスタイルの変更
   - アクセシビリティ属性 (ARIA ラベル、キーボード操作など)
   - モード切り替えを管理する JavaScript/TypeScript コード
6. 更新内容に問題がなければ **Conversation** タブに戻り、下にスクロールします。
7. ワークフローが承認待ちになっていることを示すインジケーターを探します。
8. 承認待ちの場合は **Approve and run workflows** を選択します。

    ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. ワークフローが PR のチェックに追加され、単一の Astro アプリのプロジェクトチェックが成功することを確認します。完了まで数分かかる場合があります。
10. **任意:** Codespace でこのブランチに切り替えて、アクセシビリティ機能を手動テストできます。ターミナルを開き、Copilot が作成したブランチ名に置き換えて、次のコマンドを実行します。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    アプリを起動し、ブラウザーでハイコントラストモードとライトモードを切り替えます。ページを再読み込みしても設定が維持されることを確認します。

アクセシビリティカスタムエージェントは、アクセシビリティのベストプラクティスに沿って機能を実装するよう Copilot をガイドしました。アクセシビリティ上の問題や改善点があれば、前の PR と同様にコメントで **@copilot** に修正を依頼できます。

## 任意の演習 — 委任を続ける

cloud agent は、実際のバックログ項目を任せて別の作業に集中するときに役立ちます。使い方に慣れるため、リポジトリにいくつか Issue を登録し、Copilot に割り当ててみてください。

- ゲーム詳細ページに支援者向けの関心フォームを作成する。
- ゲーム一覧ページにページネーションを実装する。
- データアクセスヘルパーに入力検証とエラー処理を追加する。

## まとめ

Cloud agent ハーネスを完了しました。各レッスンで、次のことを行いました。

- **このリポジトリに用意されたカスタム指示ファイルを確認し、cloud agent の出力に反映されることを後から確認できるようにした。**
- **Copilot cloud agent に Issue を割り当て、環境の準備、計画、非同期の実行を確認した。**
- **カスタムエージェントを作成して使い、ハイコントラストモードとライトモードを追加した。**
- **Copilot agents ページを Mission Control として使い、進行中のアクセシビリティセッションを監視してガイドした。**
- **Copilot の pull request をレビューして改善し、`@copilot` に変更を依頼して、ワークフローを承認した。**

## 振り返りと次のステップ

Cloud agent ハーネスを完了しました。さらに学びたい場合は、ほかのハーネスで今回の内容を発展させられます。

- 🖥️ **[VS Code ハーネス](../../vscode/)** — IDE から Copilot Chat のエージェントモードと MCP を使用します。
- 💻 **[CLI ハーネス](../../cli/)** — Plan と Autopilot、エージェントスキル、QA カスタムエージェント、Playwright MCP、Agent Merge、cloud agent への任意の `/delegate` を使い、ターミナルからレビュー済みの変更を 2 件リリースします。

自分のリポジトリで、次のアイデアも試せます。

- リファクタリングやテストカバレッジの Issue を cloud agent に割り当てる。
- 別の分野向けのカスタムエージェントを作成する。
- 別の技術スタック向けに `copilot-setup-steps.yml` を設定する。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot agents について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップワークフローのベストプラクティス][cloud-agent-best-practices]
- [Copilot cloud agent のファイアウォールを設定する][agent-firewall]

---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
