---
title: "演習 5: GitHub Copilot の作業を反復して改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

## 作業をレビューする

このラボでは、ユーザー体験の改善や機能追加に関する複数のタスクを GitHub Copilot と進めてきました。Copilot にコードのドキュメントを追加させ、デザインチームが改善案を検討するための関連ゲーム機能を作成させ、高コントラストとライトモードの切り替えを含むアクセシビリティ機能を実装させました。コードの変更を確認し、必要に応じて Copilot にフィードバックを送り、作業を改善しましょう。

### シナリオ

これまで何度も説明したように、生成 AI を追加しても、ソフトウェア設計や DevOps の基本は変わりません。生成コードを必ずレビューし、通常の DevOps プロセスに沿って作業します。チーム全体のレビューを有効にする前に、ドキュメント、関連ゲーム機能、アクセシビリティ機能の作成に関する GitHub Copilot の提案を確認します。

## セキュリティと GitHub Copilot cloud agent

Copilot cloud agent は非同期で、監視されずにタスクを実行するため、安全性を保つための制約が設けられています。

- Copilot はリポジトリへの読み取りアクセスのみを持ち、書き込みアクセスはコードを作成するためのブランチに**限定**されます。
- cloud agent は GitHub Actions 内で実行され、作業用に独立した一時的な環境を作成します。
- GitHub Actions ワークフローは、人間が承認するまで実行できません。
- MCP サーバーを含め、[外部リソースへのアクセスは既定で制限されています](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)。

## 生成されたドキュメントをレビューする

まず、GitHub Copilot cloud agent が最初に作成したプルリクエスト (PR)、つまりコードへのドキュメント追加を確認します。GitHub.com の標準の PR インターフェイスを使います。

> [!NOTE]
> PR を確認する際に、GitHub Copilot がファイアウォールによってブロックされたという警告が表示される場合があります。外部 MCP サーバーへの呼び出しなど、外部リソースへのアクセスが既定で制限されているためで、これは**想定された動作**です。必要に応じて、[Copilot cloud agent のファイアウォールをカスタマイズまたは無効化][agent-firewall]できます。

1. github.com の自分のリポジトリに戻ります。
2. **Pull Requests** を選択して、プルリクエストの一覧を開きます。
3. **Add missing documentation** など、ドキュメント追加に関するタイトルのプルリクエストを開きます。

> [!NOTE]
> Copilot がまだ作業中の場合、PR のタイトルには **[WIP]** が含まれています。その場合は、Copilot の作業が完了するまで待ちます。数分かかる場合があります。その間に、これまで学んだ内容を振り返りましょう。

4. PR の準備ができたら、**Files changed** タブを選択して変更をレビューします。

    ![Files changed タブ](../../../_images/shared-pr-files-changed.png)

5. TSDoc のドキュメントコメントなど、更新されたコードを確認します。実際の変更内容は異なる場合があります。

   TypeScript の規約、TSDoc のドキュメントコメント、コメントヘッダーを確認してください。これらは演習 2 の最初に確認したカスタムインストラクションファイルによるものです。

6. 更新をレビューして問題がなければ、**Conversation** タブに戻って下にスクロールします。
7. ワークフローが承認待ちであることを示すインジケーターを確認します。
8. 承認待ちのワークフローがある場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する](../../../_images/shared-approve-workflows.png)

9. PR のチェック欄にワークフローが登録されます。問題がなければ、単一の Astro アプリに対するプロジェクトのチェックが成功します。完了まで数分かかる場合があります。

## GitHub Copilot に変更を依頼する

PR で Copilot と作業する方法は、変更を一方的に受け取るだけではありません。チームのほかのメンバーと同じように、PR のコメントやコードのインラインコメントで Copilot にメンションできます。Copilot はコメントを読み、新しいセッションを開始して対応します。結果は確率的に決まるため、依頼する文章を一つに指定することはできません。たとえば、次のような変更を依頼できます。

- 各コードファイルの先頭に、役割を簡潔に説明するコメントヘッダーを追加する。
- TypeScript ファイルと Astro ファイルに TSDoc のドキュメントコメントを追加する。
- Astro アプリの構成を説明する README を作成する。

1. 上記の案、またはコードベースに追加したい別のドキュメント改善を使って、**@copilot** にメンションするコメントを追加し、生成されたドキュメントの変更を依頼します。
2. **View Session** を選択して、Copilot が作業する様子を確認します。変更に新しいセッションが開始されます。
3. **Back to pull request** を選択して PR に戻れます。

    ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Copilot の作業が終わると、プルリクエストに新しいコミットが表示されます。
5. **Files changed** タブを選択して変更をレビューします。

納得できるまで Copilot に変更を依頼できます。準備ができたら、ドラフト PR をレビュー可能な状態に変更して `main` ブランチにマージします。

![プルリクエストをレビュー可能な状態に変更する](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

ゲーム詳細ページに関連ゲームを表示する issue に対して Copilot が作成した PR を確認します。

1. GitHub.com の自分のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Show related games on the game details page** など、関連ゲームの PR を開きます。
4. **Files changed** タブを選択して、生成されたコードをレビューします。
5. 更新をレビューして問題がなければ、**Conversation** タブに戻って下にスクロールします。
6. ワークフローが承認待ちであることを示すインジケーターを確認します。
7. 承認待ちのワークフローがある場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する](../../../_images/shared-approve-workflows.png)

8. PR のチェック欄にワークフローが登録されます。問題がなければ、単一の Astro アプリに対するプロジェクトのチェックが成功します。完了まで数分かかる場合があります。
9. **任意:** Codespace でこのブランチに切り替えて、関連ゲーム機能を手動でテストできます。Codespace に移動し、ターミナルを開いて次のコマンドを実行します。`<branch-name>` は Copilot が作成したブランチ名 (例: **copilot/fix-8**) に置き換えてください。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot が関連ゲーム機能を作成しました。ほかの作業と同じように、Copilot cloud agent に変更を依頼して反復できます。たとえば、表示する関連ゲームの数を変更したり、コメントヘッダーや TSDoc のドキュメントコメントを追加したりできます (この issue を割り当てたのは、カスタムインストラクションを更新する前だったことを思い出してください)。前の PR と同じように、**Conversation** タブでコメントを追加すると Copilot が確認し、新しいセッションで作業を開始します。

## アクセシビリティ機能をレビューする

最後に、アクセシビリティカスタムエージェントで実装した機能をレビューします。この PR には、演習 3 で依頼した高コントラストモードと、演習 4 でミッションコントロールから追加依頼したライトモードの両方が含まれるはずです。

1. GitHub.com の自分のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Add high contrast mode to website** など、アクセシビリティ機能に関する PR を開きます。

> [!NOTE]
> Copilot がまだ作業中の場合、プルリクエストには **[WIP]** が含まれています。その場合は、作業が完了するまで待ちます。数分かかる場合があります。

4. **Files changed** タブを選択して、生成されたコードをレビューします。
5. 実装を確認し、特に次の点に注意してください。
   - モードを切り替える UI コンポーネント
   - ユーザー設定を保持するためのローカルストレージの使用
   - 高コントラストモードとライトモードの CSS またはスタイル変更
   - アクセシビリティ属性 (ARIA ラベル、キーボード操作など)
   - モード切り替えを管理する JavaScript/TypeScript コード

6. 更新をレビューして問題がなければ、**Conversation** タブに戻って下にスクロールします。
7. ワークフローが承認待ちであることを示すインジケーターを確認します。
8. 承認待ちのワークフローがある場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する](../../../_images/shared-approve-workflows.png)

9. PR のチェック欄にワークフローが登録されます。問題がなければ、単一の Astro アプリに対するプロジェクトのチェックが成功します。完了まで数分かかる場合があります。
10. **任意:** Codespace でこのブランチに切り替え、アクセシビリティ機能を手動でテストできます。Codespace に移動し、ターミナルで次のコマンドを実行します。`<branch-name>` は Copilot が作成したブランチ名に置き換えてください。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    アプリケーションを起動し、ブラウザーで高コントラストとライトモードの切り替えを試します。ページを再読み込みした後も設定が保持されることを確認してください。

アクセシビリティカスタムエージェントが、アクセシビリティのベストプラクティスに沿って機能を実装するよう Copilot をガイドしたことを確認できます。アクセシビリティについて懸念や改善案がある場合は、前の PR と同じようにコメントで **@copilot** に変更を依頼できます。

## 任意の演習 — 委任を続ける

cloud agent は、バックログにある実際の作業を任せて、その間に別の作業に取り組むのに適しています。この方法に慣れるため、自分のリポジトリで issue をいくつか作成し、Copilot に割り当てます。たとえば次のようなタスクを試せます。

- ゲーム詳細ページに支援者向けの問い合わせフォームを作成する。
- ゲーム一覧ページにページネーションを実装する。
- データアクセスヘルパーに入力検証とエラー処理を追加する。

## まとめ

Cloud agent ハーネスを完了しました。各レッスンを通じて、次のことを行いました。

- **このリポジトリに含まれるカスタムインストラクションファイルを確認し、** cloud agent の出力に与える影響を確認しました。
- **Copilot cloud agent に issue を割り当て、** 環境のセットアップ、計画、非同期の作業を観察しました。
- **アクセシビリティ用のカスタムエージェントを作成して使い、** 高コントラストとライトモードの切り替えを追加しました。
- **Copilot agents ページをミッションコントロールとして使い、** 進行中のアクセシビリティセッションを監視して指示を追加しました。
- **cloud agent のプルリクエストをレビューして改善し、** `@copilot` に変更を依頼してワークフローを承認しました。

## レビューと次のステップ

Cloud agent ハーネスは完了です。さらに Copilot エージェントの機能を学ぶ場合は、ほかのハーネスで異なる開発環境のシナリオを確認できます。

- 🖥️ **[VS Code ハーネス](../../vscode/)** — IDE で Copilot Chat のエージェントモードと MCP 統合を確認します。
- 💻 **[CLI ハーネス](../../cli/)** — Plan と Autopilot、エージェントスキル、カスタム QA エージェント、Playwright MCP、Agent Merge、cloud agent に戻る任意の `/delegate` 連携を使って、ターミナルからレビュー済みの変更を 2 つリリースします。

自分のリポジトリでは、次のようなタスクも試せます。

- リファクタリングまたはテストカバレッジの issue を cloud agent に割り当てる。
- 別の分野を担当するカスタムエージェントを作成する。
- 異なる技術スタック用の `copilot-setup-steps.yml` を設定する。

## 参考資料

- [GitHub Copilot][github-copilot]
- [Copilot agents について][copilot-agents]
- [GitHub issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent セットアップワークフローのベストプラクティス][cloud-agent-best-practices]
- [Copilot cloud agent のファイアウォールを構成する][agent-firewall]

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
