---
title: "レッスン 7 - QA エージェントを作成して使う"
description: "issue の要件、quality-checks スキル、Playwright MCP を組み合わせた QA カスタムエージェントを作成します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

これまでに `quality-checks` スキルで自動チェックを実行し、Playwright MCP でブラウザーからフィルタリング機能を確認しました。次は、明確な QA プロセスを備えたカスタムエージェントにこれらの機能をまとめます。

このレッスンでは、次のことを行います。

- カスタムエージェントがインストラクション、スキル、MCP ツールと連携する方法を確認します。
- 再利用可能な品質保証プロファイルを作成して確認します。
- QA エージェントを選択し、フィルタリングの issue に照らした評価結果を確認します。

## シナリオ

Tailspin Toys では、プルリクエスト (PR) を作成する前に、要件、コード品質、自動チェック、テストカバレッジ、ブラウザー上の動作を一貫して確認したいと考えています。カスタムエージェントを使うと、QA プロセスをまとめ、再利用可能なレポートを作成できます。

## カスタムエージェントとは

カスタムエージェントは、Markdown のプロファイルで定義する Copilot の特化版です。プロファイルには、エージェントの目的、インストラクション、利用できるツールを記述します。このワークショップでは、`.github/agents/qa.agent.md` に QA の役割を定義し、Copilot CLI で選択します。

これまでに使ったカスタマイズには、それぞれ異なる役割があります。リポジトリのインストラクションはチーム標準を説明します。`quality-checks` スキルは繰り返し実行できるチェックをまとめます。Playwright MCP はブラウザーツールを提供します。QA プロファイルは、要件を評価して結果を報告する際に、これらの機能をどのように使うかを Copilot に伝えます。既存の機能を置き換えたり、別の会話を要求したりするものではありません。

## QA プロファイルを作成する

機能の PR を開く前に、再利用できる QA プロファイルを作成するよう Copilot に依頼します。プロファイルには、QA が行うチェックと守るべき境界の両方を定義します。

1. Codespace に戻り、フィルタリングの会話が Interactive モードであることを確認します。
2. 既定のエージェントに、次のカスタムエージェントの作成を依頼します。

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## プロファイルを確認する

新しいエージェントを使う前に、Copilot が意図した QA の流れと制約を反映したことを確認します。検証だけを依頼したのに、不完全または範囲が広すぎるエージェントが機能を変更してしまうのを防げます。

1. `/diff` を入力し、`.github/agents/qa.agent.md` を開きます。
2. フロントマターを読みます。`description` は必須で、`name` は任意ですが、記載するとエージェントの表示名が明確になります。
3. プロファイルのインストラクションを読み、QA が要件を確認し、リポジトリのインストラクションに従い、`quality-checks` スキルを実行して、Playwright MCP を使うことを確認します。
4. QA が根拠を示して報告し、実装コードの変更前に確認を求め、変更をコミットしたり PR を作成したりしないことを確認します。
5. これらの役割や制約が抜けている場合は、続行する前に既定のエージェントに修正を依頼します。

## issue に対して QA を実行する

Copilot CLI は会話の開始時にプロジェクトのエージェントを読み込みます。プロファイルを作成したら、同じフィルタリングの会話を再開し、QA を選択します。issue と計画時の決定事項を利用できます。

1. 次のプロンプトを使ってエージェントを有効にします。

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> 作成した直後のエージェントは、利用可能なエージェントの一覧に表示されない場合があります。次のコマンドで有効にできます。

2. 次のプロンプトで QA エージェントに機能のレビューを依頼します。

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. QA エージェントが作業を始めます。
4. 作業が終わったら、レポートを確認します。

## まとめと次のステップ

ワークフローに再利用可能な専門家の役割を追加し、その結果を確認しました。このレッスンでは、次のことを行いました。

- カスタムエージェントがインストラクション、スキル、MCP ツールと連携する方法を確認しました。
- 再利用可能な品質保証プロファイルを作成して確認しました。
- QA エージェントを選択し、フィルタリングの issue に照らした評価結果を確認しました。

実装、スキルの更新、QA プロファイル、テスト、検証レポートがそろいました。次は、[機能の PR にまとめて Agent Merge を使います][next-lesson]。

## 参考資料

- [Copilot CLI 用のカスタムエージェントを作成する][create-agents]
- [カスタムエージェントの構成][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
