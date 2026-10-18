---
title: "レッスン 6 - Playwright MCP で機能を検証する"
description: "Copilot CLI で Playwright MCP を確認または構成し、ブラウザーでフィルタリング機能を試します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

これまでに説明したように、コードを書くこと以外にも、データや外部サービスを利用したり、Copilot が追加の自動化機能を使えるようにしたりする必要があります。ここで役立つのが MCP サーバーです。MCP サーバーを使うと、Copilot は CLI に組み込まれた機能を超えて、さらに多くのツールやサービスを利用できます。

このレッスンでは、次のことを行います。

- Model Context Protocol (MCP) とは何か、Copilot CLI がどう使うかを理解します。
- まだ利用できない場合は Playwright MCP サーバーを追加します。
- エージェントにブラウザーを操作させ、フィルタリング機能を試します。

## Model Context Protocol (MCP) とは

[Model Context Protocol (MCP)][mcp-blog-post] は、AI エージェントが外部のツールやサービスと通信するためのプロトコルです。MCP を使うと、AI エージェントは外部ツールやサービスとリアルタイムに通信し、最新情報にアクセスしたり、ユーザーに代わって操作したりできます。

これらのツールやリソースには、AI エージェントと外部ツールやサービスの橋渡しをする MCP サーバーを介してアクセスします。各 MCP サーバーは、AI エージェントが利用できる異なるツールやリソースを提供します。

よく使われる MCP サーバーには、次のものがあります。

- **[GitHub MCP Server][github-mcp]**: GitHub リポジトリ、issue、プルリクエストを管理する API を利用できます。
- **[Playwright MCP Server][playwright-mcp-server]**: Playwright によるブラウザー自動化機能を利用できます。

ほかにも多くの MCP サーバーがあります。GitHub はエコシステム内での発見や貢献を促進するために、[MCP レジストリ][mcp-registry]をホストしています。

> [!CAUTION]
> MCP サーバーは、ほかの依存関係と同じように扱ってください。利用する前にソースコードを確認し、発行元を検証して、セキュリティへの影響を検討してください。

## Playwright MCP サーバーを追加する

Playwright MCP サーバーがまだ利用できない場合は、Copilot がユーザーと同じように Web サイトを操作できるように追加します。

> [!NOTE]
> Copilot CLI ですでに Playwright MCP が構成されている場合は、このセットアップ手順を省略して次のセクションに進んでください。

1. Codespace に戻ります。
2. Copilot CLI で次のコマンドを入力し、MCP サーバー追加ダイアログを開きます。

   ```plaintext
   /mcp add
   ```

3. 名前に `playwright` と入力し、<kbd>Tab</kbd> を押します。
4. <kbd>Enter</kbd> を押してサーバーの種類が **STDIO** であることを確認し、<kbd>Tab</kbd> を押します。
5. **Command** ダイアログに次のコマンドを貼り付けます。

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. <kbd>Control</kbd>+<kbd>S</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) を押して、新しい MCP サーバーを保存します。
7. <kbd>Esc</kbd> を押して MCP ダイアログを終了します。

## Playwright で機能を試すよう Copilot に依頼する

先ほどは機能が想定どおりに動作することを手動で確認しました。今度は Copilot が利用できる Playwright MCP サーバーを使って、同じ確認を行います。

1. 次のプロンプトを使って、Playwright MCP サーバーで機能を検証するよう Copilot に指示します。

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> 通常、Copilot は自分で MCP サーバーを判断して使うため、明示的に指示する必要はありません。ただし、使用するサーバーが分かっている場合は、明示するとよいでしょう。より一貫した結果につながり、トークンも節約できます。

2. ブラウザーで機能を確認するために Copilot が実行する手順を確認します。
3. 報告を読み、すべてが期待どおりに動作することを確かめます。

Copilot はサーバーを起動し、Playwright で Web サイトを操作し、サーバーを停止して、結果を報告します。

## まとめと次のステップ

おめでとうございます。Copilot CLI から実際のブラウザーで機能を試すために、Playwright MCP サーバーを使いました。ここまでに、次のことを行いました。

- Model Context Protocol (MCP) とは何か、Copilot CLI がどう使うかを学びました。
- まだ利用できない場合は Playwright MCP サーバーを追加しました。
- エージェントにブラウザーを操作させ、フィルタリング機能を試しました。

次は、スキルとブラウザーツールを組み合わせた[カスタム QA エージェント][next-lesson]を作成します。

## 参考資料

- [MCP とは何か、なぜ話題になっているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [Copilot CLI に MCP サーバーを追加する][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
