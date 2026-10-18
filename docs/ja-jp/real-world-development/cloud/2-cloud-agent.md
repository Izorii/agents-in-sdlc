---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

技術的負債に悩んでいない組織はほとんどありません。解決されていないセキュリティの問題、更新が必要なレガシーコード、実装する時間がなくバックログに残ったままの機能リクエストなどがあります。GitHub Copilot の cloud agent は、コードの更新や機能追加などを自律的に実行します。作業が完了すると、人間の開発者がレビューできるドラフト PR を作成します。面倒なタスクを任せ、開発を加速させることで、開発者はより大きな課題に集中できます。

このレッスンでは、Copilot cloud agent を使って次のことを行います。

- コード生成の環境をカスタマイズします。
- 操作が安全に実行されるようにします。
- スコープを明確にした issue の重要性を確認します。
- issue を Copilot に割り当てます。

## シナリオ

Tailspin Toys には対応したい技術的負債があります。最初のサイトを作成した請負業者が残したドキュメントは、望ましい状態ではありません。実際には、ドキュメントがまったくありません。最初の対応として、アプリケーションのすべてのエクスポート関数に TSDoc のドキュメントコメントを追加したいと考えています。

さらに、デザインチームはゲームを見つけやすくしたいと考えています。各ゲームの詳細ページに、同じカテゴリに属するほかのタイトルを関連ゲームとして表示し、閲覧を続けられるようにしたいと考えています。完成度の高い実装ではなく、ユーザー体験 (UX) の受け入れテストに使えるものを求めています。現在は作業の妨げになっていますが、より優先度の高い issue もあります。

このようなタスクは優先度が下がりがちですが、Copilot cloud agent に割り当てるのに適しています。cloud agent が非同期で作業する間、開発者は別のタスクに取り組み、後で戻って Copilot の作業をレビューできます。

## GitHub Copilot cloud agent とは

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) は、人間の開発者と同様にバックグラウンドでタスクを実行できます。人間の開発者に作業を依頼するのと同じように、[GitHub issue を Copilot に割り当てる](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)方法などがあります。割り当て後、Copilot は進行状況を追跡するドラフトのプルリクエストを作成し、環境をセットアップしてタスクに取りかかります。作業中でも完了後でも Copilot のセッションを確認できます。提案された解決策のレビュー準備ができると、プルリクエストで通知されます。

## 適切なスコープのインストラクションが重要

GitHub Copilot に魔法はありません。考える作業を省略できるわけではありません。一見簡単な操作も、詳しく調べると複雑な場合があります。[Copilot cloud agent に依頼するタスクのスコープ][cloud-agent-best-practices]を慎重に検討してください。AI ペアプログラマーのように扱い、段階的に作業し、学び、試して、必要に応じて調整します。生成 AI を取り入れても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタムインストラクション

前の演習で、カスタムインストラクションと、それが Copilot をガイドする方法を確認しました。カスタムインストラクションの演習を済ませている場合は、`.github/copilot-instructions.md` がどのように使われるかを確認しています。まだの場合も、ここで簡単に確認できます。Copilot cloud agent に作業を割り当てる前に、このリポジトリに含まれるインストラクションファイルを読み取り専用で確認し、後でその影響を把握できるようにします。

GitHub Web UI または起動済みの Codespace で、次のファイルを開きます。

- `.github/copilot-instructions.md` - **Code standards** セクション、特に TypeScript 規約と TSDoc のドキュメントコメントに関する要件を確認します。
- `.github/instructions/unit-tests.instructions.md` - `applyTo` フロントマターを確認します。このインストラクションは `**/*.test.ts` ファイルを対象とします。

次のセクションで `Code lacks documentation` issue を cloud agent に割り当てる際、生成されたプルリクエストに TSDoc のドキュメントコメント、TypeScript 規約、コメントヘッダーが含まれるか確認します。これらはインストラクションファイルの設定によるもので、後の演習で PR をレビューするときにもう一度確認します。

## Copilot cloud agent の開発環境を設定する

誰がコードを作成する場合でも、通常は特定の環境とセットアップスクリプトが必要です。Copilot にタスクを割り当てる場合も同様です。cloud agent はソフトウェアエンジニアと同様の方法で作業します。

cloud agent は [GitHub Actions][github-actions] を実行環境として使います。`.github/workflows/copilot-setup-steps.yml` に構成する[専用のセットアップワークフロー][setup-workflow]を作成すると、作業開始前に必要な開発ツールや依存関係を利用できるよう環境をカスタマイズできます。ラボを円滑に進め、この学習機会を提供するため、必要な設定はあらかじめ用意されています。単一の Astro アプリに対して、Node.js、プロジェクトの依存関係、エンドツーエンドテスト用のブラウザーバイナリ、移行とシードが済んだ SQLite データベースを Copilot が利用できます。

```yaml
name: "Copilot Setup Steps"

# リポジトリの "Actions" タブからセットアップ手順をテストできます
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # *手順*に必要な最小限のアクセス許可を設定します。Copilot は操作用に独自のトークンを使用します。
    permissions:
      # 依存関係のインストールなど、セットアップ手順の一部としてリポジトリをクローンする場合は、`contents: read` のアクセス許可が必要です。セットアップ手順でクローンしない場合、手順の完了後に Copilot が自動的にクローンします。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # フロントエンド / アプリのセットアップ - Node.js (現在、アプリ全体が Astro + Drizzle/libSQL です)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # ビルドとテストに使うデータを用意するため、ローカル SQLite データベースをマイグレーションしてシードします。
      - name: Set up the database
        run: npm run db:setup
```

ほかの GitHub ワークフローファイルと同様に見えますが、いくつか重要な点があります。

- `copilot-setup-steps` という単一のジョブが含まれています。このジョブは Copilot がプルリクエストの作業を開始する前に、GitHub Actions で実行されます。
- `workflow_dispatch` トリガーを使うと、リポジトリの **Actions** タブからワークフローを手動で実行できます。Copilot の実行を待たずに、ワークフローが成功することをテストできます。

## ドキュメントを追加する

ドキュメントの重要性は広く認識されていますが、多くのプロジェクトでは内容が古いか、ドキュメント自体が不足しています。この種の技術的負債は対応されないままになりやすく、生産性を低下させ、コードベースの保守や新しい開発者の参加を難しくします。Copilot はドキュメントの作成を得意とするため、cloud agent に割り当てるのに適した issue です。バックグラウンドで必要なドキュメントを作成します。後の演習で成果をレビューします。

1. 新しいブラウザータブで github.com の自分のリポジトリに移動します。
2. **Issues** タブを選択します。
3. **New issue** を選択し、新しい issue のダイアログを開きます。
4. **Blank issue** を選択して issue を作成します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次の内容を入力します。

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. **Create** を選択して issue を作成します。
8. 右側の **Assign to Copilot** を選択し、割り当てダイアログを開きます。

  ![Copilot を issue に割り当てる](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

  ![Copilot の割り当て詳細](../../../_images/ex4-assign-copilot-details.png)

10. **Pull Requests** タブを選択します。
11. 新しく生成されたプルリクエスト (PR) を開きます。タイトルは `[WIP]: Code lacks documentation` のようなものです。新しい PR が一覧に表示されない場合は、少し待ってからブラウザーを更新してください。
12. 数分後、Copilot が todo リストを作成したことを確認できます。

> [!NOTE]
> Copilot の todo リストが PR に表示されるまで、数分かかる場合があります。Copilot は環境を作成し (前述のワークフローを実行し)、プロジェクトを分析して、問題に取り組む方法を決定しています。

13. リストと、Copilot が実行するタスクを確認します。
14. プルリクエストのタイムラインを下にスクロールし、Copilot が issue の作業を開始したことを示す更新を確認します。
15. **View session** ボタンを選択します。

  ![Copilot のセッション表示](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 更新されたインジケーターを確認するために、ウィンドウの更新が必要な場合があります。

16. ライブセッションをスクロールして、Copilot が問題を解決する様子を確認します。コードを調べて状態を理解し、考えながら適切な計画を決め、コードを作成する過程が表示されます。

この作業には数分かかる場合があります。Copilot cloud agent の主な目的の 1 つは、非同期にタスクを実行し、ほかの作業に集中できるようにすることです。この機能を活用して、別のタスクも Copilot cloud agent に割り当ててから、アプリケーションへの機能追加に取り組みます。

## ゲーム詳細ページに関連ゲームのセクションを追加する

GitHub Copilot cloud agent の大きな利点の 1 つは、作業を分担できることです。エージェントが作業している間、別の作業に集中できます。デザインチーム向けの関連ゲーム表示に時間がかからないとしても、その時間を別の作業に使えます。Copilot cloud agent に依頼しましょう。

1. github.com の自分のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択し、新しい issue のダイアログを開きます。
4. **Blank issue** を選択して、空のテンプレートを使います。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次の内容を入力します。

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. **Create** を選択して issue を作成します。
8. 右側の **Assign to Copilot** を選択し、割り当てダイアログを開きます。

  ![Copilot を issue に割り当てる](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

まもなく、issue の最初のコメントに 👀 が表示されます。Copilot が作業を始めた合図です。

![目の絵文字で issue に取り組んでいることを示す Copilot](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は 2 つ目のリクエストにも取り組んでいます。Copilot cloud agent はソフトウェアエンジニアと同じように作業するため、完了まで積極的に監視する必要はありません。完了後にレビューします。次に、カスタムエージェントを作成して使う方法を確認します。

## まとめと次のステップ

このレッスンでは、[GitHub Copilot cloud agent][copilot-agents]を確認しました。cloud agent に issue を割り当て、非同期で作業させることができます。技術的負債への対応、新機能の作成、別のフレームワークへのコード移行などを依頼できます。

次の内容を学びました。

- コード生成の環境をカスタマイズする方法
- 操作が安全に実行されるようにする方法
- スコープを明確にした issue の重要性
- issue を Copilot に割り当てる方法

cloud agent がバックグラウンドで作業している間に、カスタムエージェントの作成と利用に進みます。[Copilot cloud agent は MCP サーバーも利用できます][cloud-agent-mcp]。前のモジュールで確認したカスタムインストラクションも利用できます。

## 参考資料

- [Copilot cloud agent について][copilot-agents]
- [GitHub issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent セットアップワークフローのベストプラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタムエージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
