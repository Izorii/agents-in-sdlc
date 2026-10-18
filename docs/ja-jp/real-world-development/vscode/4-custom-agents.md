---
title: "演習 4 - カスタムエージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタムエージェントとは

GitHub Copilot の[カスタムエージェント][custom-agents-concept]を使うと、開発ワークフロー内の特定のタスクや分野に合わせた AI アシスタントを作成できます。リポジトリ内の `.github/agents` フォルダーにある Markdown ファイルでエージェントを定義すると、特定の作業をより効果的に実行するための専用インストラクション、ベストプラクティス、コーディングパターン、分野固有の知識を Copilot に提供できます。チームは専門知識を再利用可能なエージェントにまとめられます。たとえば、[WCAG][wcag] 準拠を確認するアクセシビリティエージェント、安全なコーディング手法に従うセキュリティエージェント、一貫したテストパターンを保つテストエージェントなどを作成できます。

カスタムエージェントは、プロジェクトの `.github/agents` フォルダー内にある Markdown ファイル、またはグローバルな `~/.copilot/agents` に定義します。各ファイルには、少なくとも `name` と `description` を含む YAML フロントマターと、エージェントの動作、専門知識、インストラクションを定義する Markdown プロンプトが含まれます。

### カスタムエージェントとエージェントスキルの違い

カスタムエージェントと[エージェントスキル][agent-skills-concept]には、ある程度の共通点があります。どちらも主に Markdown ファイルで定義し、操作方法を AI に伝えます。最も簡単な区別は、**カスタムエージェントが作業者で、スキルがツール**であるということです。

カスタムエージェントは独自のコンテキストウィンドウを持ち、作業の一環としてスキルやほかのエージェントを調整します。このラボでは、アクセシビリティのカスタムエージェントがガイドラインに照らしてサイトをレビューし、更新します。その一環として、プルリクエストのワークフローを扱うスキルや、テストを実行・管理するスキルなどを呼び出せます。

> [!NOTE]
> カスタムエージェントの作成方法に唯一の正解はありません。AI のほかの活用方法と同様に、利用する環境やシナリオで試し、改善してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、すべてのユーザーがクラウドファンディングプラットフォームを利用できるように取り組んでいます。最近、現在のダークテーマはテキストと背景色のコントラストが不十分で、読みにくいというフィードバックがありました。このアクセシビリティの課題に対応するため、デザインチームはユーザーが切り替えられる高コントラストモードを追加するよう依頼しました。

アクセシビリティは重要なので、できるだけ早く実装したいと考えています。カスタムエージェントを使って機能を生成します。

この演習では、次のことを行います。

- 既存のアクセシビリティカスタムエージェントを確認します。
- Copilot Chat でアクセシビリティエージェントを使い、高コントラストモードを実装します。

## アクセシビリティカスタムエージェントを確認する

アクセシビリティ用のカスタムエージェントがすでに用意されています。Copilot をどのようにガイドするかを理解するため、内容を確認します。

Codespace に戻り、ターミナルを開いて、アクセシビリティ作業用に `main` から新しいブランチを作成します。演習 3 のフィルタリング PR とは分けて作業します。

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. `.github/agents/accessibility.md` を開きます。
2. `name` と `description` フィールドを含む YAML フロントマターを確認します。

> [!CAUTION]
> `name` と `description` を含むフロントマターは、カスタムエージェントに必須です。

3. 続くセクションを確認します。次の内容が記載されています。
   - アクセシブルな Web サイトのコードを生成する際の主な責任
   - アクセシビリティのベストプラクティス
   - HTML、CSS、JavaScript のコード例
   - よくある落とし穴と間違い

## Copilot Chat でカスタムエージェントを使う

VS Code では、`.github/agents` に定義されたすべてのカスタムエージェントが、Copilot Chat ビュー下部のエージェントのドロップダウンに表示されます。カスタムエージェントを選択すると、そのエージェントのインストラクションとツールに合わせてチャットセッションを設定できます。

> [!TIP]
> **Copilot Chat を開く**
>
> 次の演習を始める前に、Codespace に戻って Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を開始します。モードとモデルは演習ごとに異なるため、必要な手順で説明します。

1. Chat ビューのエージェントのドロップダウンで **Agent** を選択します。すでに選択されている場合は、そのまま進みます。

   ![Chat ビューでエージェントを選ぶ画面](../../../_images/shared-chat-mode-selector.png)

2. Chat ビュー下部にあるエージェントのドロップダウンを選択します。現在のエージェントが表示されています。既定では **default** です。
3. 利用可能なエージェントから **Accessibility agent** を選択します。
4. アクセシビリティエージェントに、次のプロンプトを送信します。

   ```plaintext
   Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
   ```

5. Copilot Chat がコードベースを調査し、変更を提案して、プロジェクトファイルを編集します。各編集は、ファイルパスとレビュー可能な差分とともにチャット内に表示されます。
6. 編集が適用されると、**Files changed** の表示が更新され、このセッションでエージェントが変更した作業ファイルが示されます。

> [!NOTE]
> この処理には数分かかる場合があります。Copilot はリポジトリに実際の変更を加えます。作業中に Astro コンポーネント、CSS、関連テストなどの既存ファイルを編集します。

次の演習では、作業中のエージェントをレビューし、指示を出します。

## まとめと次のステップ

このレッスンでは、特定のタスクや分野に合わせた AI アシスタントである [GitHub Copilot のカスタムエージェント][custom-agents]を確認しました。カスタムエージェントを使うと、チームの専門知識や標準を再利用可能な形でまとめ、Copilot の作業をガイドできます。

次の内容を確認しました。

- カスタムエージェントの定義方法
- Copilot Chat のエージェントモードでカスタムエージェントを使う方法

Copilot は高コントラストモードの実装を続けています。次は、ミッションコントロールから[エージェントの作業を監視して指示を出し][next-lesson]、同じセッションでライトモードの切り替えも追加します。

## 参考資料

- [カスタムエージェントについて][custom-agents]
- [IDE でカスタムエージェントを作成する][creating-custom-agents-ide]
- [VS Code のカスタムエージェント][custom-agents-vscode]
- [カスタムエージェントの構成][custom-agents-config]
- [awesome-copilot のカスタムエージェント][awesome-copilot-agents]

---

| [← 前のレッスン: Playwright MCP サーバーで機能をテストする][previous-lesson] | [次のレッスン: エージェントを監視・管理する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
