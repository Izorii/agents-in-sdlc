---
title: "演習 2 - Copilot エージェントモードで機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

アプリケーションの簡単な変更でも、通常は複数のファイルを更新し、テストなどを実行する必要があります。開発者の作業では、必要なファイルを探し、変更し、テストを実行し、デバッグし、変更漏れのファイルを見つけて、さらに更新する、といった手順が続きます。

こうした作業に役立つのが Copilot エージェントモードです。

Copilot エージェントモードは、IDE 内でより自律的に作業できるように設計されています。開発者と同じように、既存のプロジェクト構成を調査し、必要な変更を加え、テストなどを実行し、見つかった問題を自動的に修正します。エージェントモードでサイトに機能を追加する方法を確認しましょう。

> [!NOTE]
> 名前は似ていますが、エージェントモードと cloud agent は異なる利用体験を提供します。エージェントモードは IDE 内で作業し、素早いフィードバックと対話を可能にします。cloud agent はチームメンバーのように非同期で作業し、issue とプルリクエストを通じてやり取りします。

この演習では、次の方法を学びます。

- Copilot エージェントモードでプロジェクトを調査し、関連ファイルを特定して、連携した変更を加えます。
- GitHub Copilot エージェントモードで UI とデータレイヤーをまたぐ新機能を実装します。
- コードベースにマージする前に、Copilot エージェントモードが生成した変更とテストをレビューします。

## シナリオ

ゲーム一覧が拡大しているため、ユーザーがカテゴリや発行元で絞り込めるようにします。前の演習では発行元ヘルパーを追加しました。ここでは、Copilot エージェントモードで残りのデータレイヤー、UI、テストを完成させます。

## Tailspin Toys の Web サイトを実行する

変更を加える前に、Tailspin Toys の現在の機能を確認しましょう。

この Web サイトは、開発者向けのテーマを持つボードゲームのクラウドファンディングプラットフォームです。ゲーム一覧とゲームの詳細を表示します。サイトは単一の Astro アプリで、ビルド時にページを静的 HTML としてレンダリングします。ページは frontmatter 内で Drizzle ORM を使ってローカルの SQLite データベースを直接クエリします。別のバックエンド API やクライアント側 UI フレームワークはありません。再利用可能なデータアクセスヘルパーは `src/lib/` に配置し、インタラクティブ機能は標準 DOM API を使った小さな Astro `<script>` で追加します。

### Web サイトを起動する

Web サイトを起動するために、単一の Astro 開発サーバーを開始する `npm run dev` タスクが用意されています。GitHub Codespace では、次の手順で実行できます。

1. Codespace に戻ります。前の演習の後、`main` にいることを確認します。
2. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、Codespace 内に新しいターミナルを開きます。
3. 新しいブランチを作成して切り替えます。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 次のコマンドを実行して Web サイトを起動します。

   ```bash
   npm run dev
   ```

   Astro 開発サーバーが起動すると、次のような URL のバナーが表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> `http://localhost:4321` をブラウザーで開くようダイアログが表示された場合は、**x** を選択して閉じます。

5. ターミナルに表示された Astro サーバーのアドレス `http://localhost:4321` を、<kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら選択して、Web サイトを開きます。

> [!NOTE]
> Codespace を使っている場合、ターミナルで localhost URL のリンクを選択すると、自動的に `https://<your-codespace-name>-4321.app.github.dev/` にリダイレクトされます。これは、Web サーバーをホストしている Codespace へのプライベートトンネルです。

### Web サイトを確認する

Web サイトを起動したら、機能を確認できます。主な機能は次のとおりです。

- **ホームページ**: ボードゲームの一覧と、タイトル、画像、説明を表示します。
- **ゲーム詳細ページ**: ゲームを選択すると、タイトル、説明、発行元、カテゴリなどの詳細を表示します。

## Copilot でバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> 次の演習を始める前に、Codespace に戻って Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を開始します。モードとモデルは演習ごとに異なるため、必要な手順で説明します。

Web サイトの初期実装は動作しますが、機能を追加して改善します。まずバックログを確認しましょう。テンプレートからプロジェクトをセットアップしたとき、issue のバックログも自動作成されています。GitHub Copilot に一覧を表示してもらいます。

1. Chat ビューのエージェントのドロップダウンから **Agent** を選択します。**Agent** は自律的に計画を立て、ファイルを変更し、ターミナルコマンドやツールを実行します。

   ![Chat ビューでエージェントを選ぶ画面](../../../_images/shared-chat-mode-selector.png)

2. 利用可能なモデルの一覧から **Claude Sonnet 4.5** を選択します。

> [!CAUTION]
> ワークショップの作成者が特定のモデルを推奨しているわけではありません。このワークショップの作成時に Claude Sonnet 4.5 を使ったため、手順に含めています。提案コードをできるだけ一貫させ、学習しやすくすることが目的です。ただし、LLM は確率的に動作するため、結果が説明と異なる場合があります。これは正常な動作です。

> [!NOTE]
> LLM の確率的な性質により、Copilot が別の MCP コマンドを使う場合がありますが、タスクは同じように完了できるはずです。

3. 次のプロンプトを Copilot に送信して、issue のバックログを確認します。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. **Continue** を選択して、すべての issue を一覧表示するコマンドを実行します。
5. 生成された issue の一覧を確認します。

Copilot がユーザーにとって有用だと考える順に issue を優先付けしていることも確認できます。

## インストラクションファイルを確認する

エージェントにコードを生成させる前に、作業のコンテキストを Copilot に提供するインストラクションファイルを確認しましょう。Web サイトへの機能追加方法を説明する [ユーザーインターフェイス (UI) インストラクション][ui-instructions]を使います。

1. Codespace で `.github/instructions/ui.instructions.md` に移動します。
2. 機能追加に関する全体的なガイダンスを確認します。次の内容が含まれています。
   - アーキテクチャの概要
   - コンポーネント設計、テスト容易性、アクセシビリティの原則
   - 次のようなファイル形式別インストラクションへのリンク:
     - Astro
     - Tailwind CSS
     - Drizzle

> [!TIP]
> インストラクションファイルから、ほかのインストラクションファイルやプロジェクト内のファイルを参照できます。パスはインストラクションファイルからの相対パスです。ガイダンスを再利用したり、複雑な内容を小さなファイルに分割したり、コード生成の例やテンプレートを提供したりできます。

## フィルタリング機能を実装する

フィルタリングを完成させるには、少なくとも 3 つの変更が必要です。

- データレイヤー (`src/lib/`) に残りのフィルタリングロジックを追加または調整します。
- フィルタリングの動作を確認するテストを追加または更新します。
- ゲーム一覧ページを更新し、フィルタリング UI を追加します。

さらに、コードベースにマージする前にテストを実行して成功させる必要があります。Copilot エージェントモードにこれらの作業を依頼できます。

1. Copilot の現在の会話を続けるか、**New Chat** を選択して新しい会話を開始します。
2. **Add Context**、**Instructions**、**ui** の順に選択し、インストラクションファイルを追加します。

   ![UI インストラクションファイルを選択する例](../../../_images/ex3-select-instructions-file.png)

3. Chat ビューのエージェントのドロップダウンで **Agent** が選択されていることを確認します。

   ![Chat ビューでエージェントを選ぶ画面](../../../_images/shared-chat-mode-selector.png)

4. モデルとして **Claude Sonnet 4.5** が選択されていることを確認します。
5. バックログ内の関連 issue に基づいて機能を実装するよう、次のプロンプトで Copilot に依頼します。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot がプロジェクトを調べ、機能に関連するファイルを探します。データレイヤーのヘルパー、UI、テストを見つけて変更し、テストを実行する様子を確認します。

   ![Copilot がプロジェクトのファイルを調査している画面](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot はプロジェクトの調査、ファイルの変更、テストの実行などを行います。タスクやコードベースの複雑さによっては数分かかります。作業中にエディターに **Keep** と **Undo** が表示される場合があります。完了後にすべての変更について **Keep** または **Undo** を選べるため、作業中に選択する必要はありません。

7. Copilot から求められたら、**Continue** を選択してテストを実行します。

   ![Copilot Chat ペインに表示されたテスト実行の確認ダイアログ](../../../_images/ex3-agent-mode-run-tests.png)

8. 作業中に一時停止したり、テストが失敗したりすることがあります。問題ありません。Copilot はエラーが検出されなくなるまで、コード生成とテストを繰り返します。

   ![Copilot エージェントモードの完了したチャットセッション](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 生成されたコードに問題がないか確認します。

> [!CAUTION]
> Copilot などの AI ツールが生成したコードは、必ずレビューしてください。

10. Web サイトを実行しているブラウザーに戻り、新機能を試します。
11. すべてが動作することを確認し、コードをレビューしたら、Copilot Chat ウィンドウで **Keep** を選択します。

## まとめと次のステップ

おめでとうございます。この演習では、GitHub Copilot エージェントモードで Tailspin Toys に機能を追加しました。次のことを学びました。

- GitHub Copilot エージェントモードで UI とデータレイヤーをまたぐ新機能を実装する方法。
- Copilot エージェントモードでプロジェクトを調査し、関連ファイルを特定して、連携した変更を加える方法。
- コードベースにマージする前に、Copilot エージェントモードが生成した変更とテストをレビューすること。

次は、[Playwright MCP サーバーで機能をテスト][next-lesson]し、プルリクエストを作成します。

### ボーナス演習 - ページネーションを実装する

ゲーム一覧が拡大すると、ページネーションが必要になります。この演習で学んだ方法を使って、Copilot にページネーションの実装を依頼してみましょう。次の点を考慮してください。

- インストラクションファイルを使うなど、既存のベストプラクティスに従います。
- ページサイズをユーザーに選択させるか、固定するかなど、ページネーションの方法を検討します。
- 希望どおりの実装を生成できるよう、プロンプトに必要なガイダンスを含めます。
- Copilot に変更を依頼したり、コンテキストを追加したりして、何度か反復する必要があるかもしれません。Copilot を使うときの通常の流れです。

## 参考資料

- [Copilot の ask、edit、agent モード: それぞれの用途][choose-mode]
- [VS Code のエージェントモード][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
