---
title: "演習 3 - カスタムエージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタムエージェントとは

GitHub Copilot の[カスタムエージェント][custom-agents-concept]を使うと、開発ワークフロー内の特定のタスクや分野に合わせた AI アシスタントを作成できます。リポジトリ内の `.github/agents` フォルダーにある Markdown ファイルでエージェントを定義すると、特定の作業をより効果的に実行するための専用インストラクション、ベストプラクティス、コーディングパターン、分野固有の知識を Copilot に提供できます。チームは専門知識を再利用可能なエージェントにまとめられます。たとえば、[WCAG][wcag] 準拠を確認するアクセシビリティエージェント、安全なコーディング手法に従うセキュリティエージェント、一貫したテストパターンを保つテストエージェントなどを作成できます。

カスタムエージェントは、プロジェクトの `.github/agents` フォルダー内の Markdown ファイル、またはグローバルな `~/.copilot/agents` に定義します。各ファイルには、少なくとも `name` と `description` を含む YAML フロントマターと、エージェントの動作、専門知識、インストラクションを定義する Markdown プロンプトが含まれます。

### カスタムエージェントとエージェントスキルの違い

カスタムエージェントと[エージェントスキル][agent-skills-concept]には、ある程度の共通点があります。どちらも主に Markdown ファイルで定義し、操作方法を AI に伝えます。最も簡単な区別は、**カスタムエージェントが作業者で、スキルがツール**であるということです。

カスタムエージェントは独自のコンテキストウィンドウを持ち、作業の一環としてスキルやほかのエージェントを調整します。このラボでは、アクセシビリティのカスタムエージェントがガイドラインに照らしてサイトをレビューし、更新します。その作業の一環として、プルリクエストのワークフロー用スキルや、テストを実行・管理するスキルなどを呼び出せます。

> [!NOTE]
> カスタムエージェントの作成方法に唯一の正解はありません。AI のほかの活用方法と同様に、利用する環境やシナリオで試し、改善してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

このレッスンでは、カスタムエージェントを使って次のことを行います。

- カスタムエージェントの定義方法を学びます。
- カスタムエージェントにタスクを割り当てます。

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、すべてのユーザーがクラウドファンディングプラットフォームを利用できるように取り組んでいます。最近、現在のダークテーマはテキストと背景色のコントラストが不十分で、読みにくいというフィードバックがありました。このアクセシビリティの課題に対応するため、デザインチームはユーザーが切り替えられる高コントラストモードの実装を依頼しました。

アクセシビリティは重要なので、できるだけ早く実装したいと考えています。カスタムエージェントを使って機能を生成します。

## アクセシビリティカスタムエージェントを確認する

アクセシビリティ用のカスタムエージェントがすでに用意されています。Copilot をどのようにガイドするかを理解するため、内容を確認します。

Codespace に戻り、アクセシビリティカスタムエージェントのファイルを確認します。

1. `.github/agents/accessibility.md` を開きます。
2. `name` と `description` フィールドを含む YAML フロントマターを確認します。

> [!CAUTION]
> `name` と `description` を含むフロントマターは、カスタムエージェントに必須です。

3. 続くセクションを確認します。次の内容が記載されています。
   - アクセシブルな Web サイトのコードを生成する際の主な責任
   - アクセシビリティのベストプラクティス
   - HTML、CSS、JavaScript のコード例
   - よくある落とし穴と間違い

## issue を作成して割り当てる

ミッションコントロールは、環境内のすべてのエージェントを操作する中心的な場所です。Copilot cloud agent にタスクを割り当て、進捗を監視し、追加のガイダンスを与えたり、作業の方向を変えたりできます。まず、高コントラストモードの作成を Copilot に依頼します。

1. リポジトリに移動します。
2. **Issues** タブを選択します。
3. **New issue** を選択し、新しい issue のダイアログを開きます。
4. **Blank issue** を選択して issue を作成します。
5. **Title** に `Add high contrast mode to website` と入力します。
6. **Description** に次の内容を入力します。

    ```plaintext
    We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
    ```

7. **Create** を選択して issue を作成します。
8. 右側の **Assign to Copilot** を選択し、割り当てダイアログを開きます。
9. カスタムエージェントの一覧から **Accessibility agent** を選択します。

    ![クラウドエージェントの割り当て画面。カスタムエージェントと Accessibility が強調表示されている](../../../_images/ex5-select-custom-agent.png)

10. **Assign** を選択します。
11. Copilot がバックグラウンドでタスクを開始します。

## まとめと次のステップ

このレッスンでは、特定のタスクや分野に合わせた AI アシスタントである [GitHub Copilot のカスタムエージェント][custom-agents]を確認しました。チームの専門知識や標準を再利用可能なエージェントにまとめ、Copilot の作業をガイドできます。

次の内容を学びました。

- カスタムエージェントの定義方法
- カスタムエージェントにタスクを割り当てる方法

Copilot が高コントラストモードの実装を進めている間に、ミッションコントロールから[エージェントセッションを監視して指示を出し][next-lesson]ます。

## 参考資料

- [カスタムエージェントについて][custom-agents]
- [組織でカスタムエージェントを使う準備][org-custom-agents]
- [エンタープライズでカスタムエージェントを使う準備][enterprise-custom-agents]

---

| [← 前のレッスン: GitHub Copilot cloud agent][previous-lesson] | [次のレッスン: エージェントを監視・管理する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
