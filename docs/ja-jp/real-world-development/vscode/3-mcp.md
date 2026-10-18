---
title: "演習 3 - Playwright MCP サーバーで機能をテストする"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: エージェントモード][previous-lesson] · [次のレッスン: カスタムエージェント →][next-lesson]

エージェントモードでフィルタリング機能を作成しました。プルリクエストを開く前に、ブラウザーで実際に動作することを確認します。自分でアプリを操作する代わりに、**Playwright MCP サーバー**を接続して Copilot に実際のブラウザーを操作させ、機能をテストします。その後、ブランチを公開して PR を作成します。

この演習では、次のことを行います。

- Model Context Protocol (MCP) とは何か、MCP サーバーで Copilot にツールを追加する方法を学びます。
- Playwright MCP サーバーをワークスペースに追加します。
- Copilot に Playwright MCP サーバーを使わせ、ブラウザーでフィルタリング機能を手動テストします。
- ブランチを公開して、フィルタリング機能のプルリクエストを作成します。

## Model Context Protocol (MCP) とは

エージェントモードは、エディターの外部にもアクセスできると、さらに強力になります。Model Context Protocol (MCP) を使うと、Copilot は外部ツールやサービスを利用できます。これは、エージェントが外部ツールやサービスと通信するための標準的な方法です。

![エージェントモードの内部動作とコンテキスト、LLM、MCP サーバーや VS Code 拡張機能が提供するツールとの連携を示す図](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) は、AI エージェントが外部ツールやサービスと通信するためのプロトコルです。MCP を使うと、AI エージェントはリアルタイムに外部ツールやサービスと通信できます。リソースを使って最新情報にアクセスしたり、ツールを使ってユーザーに代わって操作したりできます。

これらのツールやリソースには、AI エージェントと外部ツールやサービスの橋渡しをする MCP サーバーを介してアクセスします。MCP サーバーは、AI エージェントと外部ツール (既存の API や npm パッケージなどのローカルツール) との通信を管理します。各 MCP サーバーは、AI エージェントが利用できる異なるツールやリソースを提供します。

よく使われる MCP サーバーには、次のものがあります。

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: GitHub リポジトリを管理する API を提供します。AI エージェントは、新しいリポジトリの作成、既存リポジトリの更新、issue やプルリクエストの管理などを実行できます。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: Playwright によるブラウザー自動化機能を提供します。Web ページへの移動、フォームへの入力、ボタンの選択などを AI エージェントが実行できます。

ほかにも、さまざまなツールやリソースを利用できる MCP サーバーがあります。GitHub は、エコシステム内での発見や貢献を促進するために [MCP レジストリ](https://github.com/mcp)をホストしています。

> [!CAUTION]
> セキュリティの観点から、MCP サーバーはほかの依存関係と同じように扱ってください。利用する前にソースコードをよく確認し、発行元を検証して、セキュリティへの影響を検討してください。信頼できる MCP サーバーだけを使い、機密リソースや操作へのアクセスを許可するときは注意してください。

## MCP の構成を確認する

`.vscode/mcp.json` ファイルでは、この VS Code ワークスペースで利用できる MCP サーバーを構成します。

1. Codespace で `.vscode/mcp.json` を開きます。
2. `github` サーバーがすでに構成されていることを確認します。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

この `github` エントリはプロジェクトのテンプレートに含まれています。そのため、前の演習で Copilot がバックログの issue を読み取れました。[リモート GitHub MCP サーバー][remote-github-mcp-server]を使っているため、ローカルにインストールする必要はありません。VS Code は HTTP 経由で接続し、Copilot がツールを初めて使うときに GitHub で認証します。

## Playwright MCP サーバーを追加する

次に 2 つ目のサーバーを追加します。[Playwright MCP サーバー][playwright-mcp-server]を使うと Copilot がブラウザーを操作できるため、機能のテストに役立ちます。

1. `.vscode/mcp.json` で `github` と並べて `playwright` エントリを追加し、ファイルを次のようにします。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. ファイルを保存します。

リモートの `github` サーバーと異なり、`playwright` は **ローカル**サーバーです。VS Code が `npx @playwright/mcp@latest` を実行して起動します。`--headless` フラグを使うと、ブラウザーウィンドウを表示せずに Playwright を実行できます。Codespace には表示用のデスクトップがないため、この設定が必要です。

> [!NOTE]
> Tailspin Toys プロジェクトはエンドツーエンドテストにすでに Playwright を使っているため、通常は Playwright が必要とするブラウザーもインストール済みです。後で Copilot がブラウザーの不足を報告した場合は、`npx playwright install chromium` を実行するよう依頼してから、もう一度試してください。

## サーバーを起動して信頼する

VS Code は **必要になったときに** MCP サーバーを起動します。Copilot がサーバーのツールを初めて使うときにサーバーが起動し、信頼するか確認されます。

1. `.vscode/mcp.json` で `playwright` エントリの上に **Start** アクションが表示されます。選択してサーバーを起動し、構成が正しいことを確認します。
2. サーバーを信頼するか確認されたら、構成をレビューして信頼を選択し、起動を許可します。

手動での起動は任意です。省略すると、次の手順で Copilot がツールを必要としたときに VS Code がサーバーを起動し、信頼を確認します。

## フィルタリング機能をテストする

[Playwright MCP サーバー][playwright-mcp-server]を使うと、Copilot が実際のブラウザーを操作できます。自分でアプリを操作して変更を確認する代わりに、エージェントにページを開き、移動し、フィルターを適用させて、結果を読み上げてもらえます。会話を離れずに機能が期待どおりに動くかをすばやく確認できます。

Playwright MCP サーバーは、スクリーンショットではなくページの[アクセシビリティツリー][playwright-mcp-server]を使います。ボタン、リンク、リスト項目などのラベル付き要素を基にエージェントが判断します。支援技術と同様の構造情報を使うため、簡単な機能確認をアクセシビリティの基本的なチェックにもできます。

サーバーを接続し、アプリを実行した状態で、次のプロンプトを Copilot に送ってフィルタリング機能を試します。

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot は Playwright MCP サーバーを通じてブラウザーを起動し、各手順を実行して結果を報告します。報告内容を issue の受け入れ条件と比較してください。問題があれば、追加の質問をしたり、コードを修正するよう再度依頼したりしてから PR を作成します。

> [!NOTE]
> このテストを実施するには、アプリを `http://localhost:4321` で実行しておく必要があります。開発サーバーを停止した場合は、プロンプトを送信する前にもう一度起動してください。Copilot が Playwright MCP サーバーを初めて使うときに、ブラウザーをダウンロードする場合があります。ブラウザーがないと報告されたら、`npx playwright install chromium` を実行するよう依頼して、もう一度試してください。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## ブランチを公開してプルリクエストを作成する

機能が動作することを確認できたら、チームがレビューできるようにプルリクエスト (PR) を作成します。まず `filtering-vscode` ブランチを公開します。

1. Codespace で **Source Control** パネルを開き、Copilot の変更を確認します。
2. **+** アイコンを選択して変更をステージします。
3. **Sparkle** ボタンを使ってコミットメッセージを生成します。

   ![変更が表示された Source Control パネル](../../../_images/ex3-source-control-changes.png)

4. **Publish** を選択してブランチをリポジトリにプッシュします。

## プルリクエストを作成する

github.com や GitHub コマンドラインインターフェイス (CLI) など、PR を作成する方法はいくつかあります。ここでは GitHub Copilot に依頼します。Copilot は関連する issue を探し、それに関連付けた PR を作成できます。

1. Copilot Chat パネルに移動し、**New Chat** を選択して新しいセッションを開始します。
2. エージェントのドロップダウンで **Agent** が選択されていることを確認し、Copilot が GitHub ツールを使えるようにします。
3. 次のプロンプトで PR の作成を依頼します。

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. Copilot が情報を集めて操作を行うために **Continue** を求めたら、必要に応じて選択します。GitHub ツールを初めて使うときに GitHub での認証が必要な場合があります。画面の指示に従って許可します。
5. Copilot が issue を検索し、該当するものを見つけて PR を作成する様子を確認します。
6. Copilot が生成したリンクを選択してプルリクエストをレビューします。ただし、**まだマージしないでください**。

## まとめと次のステップ

おめでとうございます。この演習では、次のことを行いました。

- Model Context Protocol (MCP) とは何か、MCP サーバーで Copilot にツールを追加する方法を学びました。
- Playwright MCP サーバーをワークスペースに追加しました。
- 出荷前に Playwright MCP サーバーでブラウザーを操作し、フィルタリング機能を手動テストしました。
- ブランチを公開して、フィルタリング機能のプルリクエストを作成しました。

今回は MCP サーバーを機能の**テスト**に使いました。Copilot を拡張する方法は MCP だけではありません。次は、アクセシビリティレビューのような対象を絞ったタスクを効率化する[カスタムエージェント][next-lesson]を作成します。

## 参考資料

- [MCP とは何か、なぜ話題になっているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP Registry][mcp-registry]
- [VS Code の MCP サーバー][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
