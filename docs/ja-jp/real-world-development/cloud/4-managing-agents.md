---
title: "演習 4 - エージェントを監視・管理する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前の 2 つの演習では、ユーザー体験の改善や機能追加に関する 3 つのタスクを Copilot cloud agent に依頼しました。cloud agent は非同期かつ自律的に動作しますが、タスクを監視することも重要です。

cloud agent に割り当てたタスクを管理するツールは複数あります。たとえば、GitHub.com の[エージェントページ][agents-page]では、オープン中のプルリクエスト (PR) を持つすべてのエージェントタスクを確認できます。実行された操作を調べたり、進行中のセッションに指示を追加してガイドしたりできます。

このレッスンでは、次のことを行います。

- エージェントページで cloud agent のタスクを監視します。
- 実行中のセッションに指示を追加し、機能をリクエストします。

## シナリオ

高コントラストモードの作成をエージェントに依頼した後、チームはライトモードも追加するのがよいと考えました。サイトのスタイルを更新し、切り替え機能を追加する作業にすでに取り組んでいるため、同じ作業に含めるのが合理的です。高コントラストに加えてライトモードも実装するよう、エージェントに指示します。

## Copilot cloud agent のタスクを確認する

Copilot cloud agent に割り当てられたタスクの現在の状態を確認します。

1. [Copilot agents ページ](https://github.com/copilot/agents)に移動します。
2. メインペインと左ペインの両方に表示されるタスク一覧を確認します。次のような、Copilot に割り当てたタスクが表示されます。
   - コードベースのドキュメントを更新する
   - ゲームを変更するための API を生成する
   - Web サイトに高コントラストモードを追加する
3. 実行中のタスクを選択し、Copilot が行った作業を確認します。次のような操作が含まれます。
   - リポジトリからコードをチェックアウトする
   - Copilot が作業する環境を作成する
   - MCP サーバーをセットアップする
   - 割り当てられたタスクを完了するための各種手順を実行する

> [!NOTE]
> 表示される手順は、Copilot の作業状態や進め方によって異なります。

4. 右側に表示されるプルリクエスト (PR) ペインも確認します。PR と変更されたファイルを表示し、追加の監視を行えます。

## cloud agent に指示を追加する

実行中のタスクを確認したので、Copilot が高コントラストモードに取り組んでいる間に、ライトモードの切り替えも含めるよう依頼します。

1. 高コントラストモードの追加に関するセッションを選択します。タイトルは Copilot が付けた名前や作業状態によって異なります。

    ![ミッションコントロールのアクセシビリティセッション](../../../_images/ex6-accessibility-session.png)

2. セットアップが完了して作業を始めたことを示すメッセージが表示されるまで、数分間セッションを確認します。
3. **Steer active session while Copilot is working** ダイアログに、次のプロンプトを入力します。

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![エージェントページで作業中の cloud agent タスク。セッション指示ダイアログが強調表示されている](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot がプロンプトを認識し、作業に含めることを確認します。

## Copilot に作業を任せる

先ほどと同じように、Copilot は更新したタスクに取り組みます。メッセージを送信した時点で進めていた手順を完了してから、新しいリクエストを作業に取り入れます。

この処理にも数分かかるため、これまでに学んだ内容を振り返ってみましょう。

## まとめと次のステップ

このレッスンでは、Copilot cloud agent のタスクを監視し、ガイドする中心的な場所である Copilot agents ページを確認しました。このミッションコントロールで進行中および完了済みのタスクを追跡し、実行中の作業をレビューして、途中で方向を変えたり追加のガイダンスを与えたりできます。

次の内容を確認しました。

- ミッションコントロールとエージェントページで cloud agent のタスクを監視しました。
- 実行中のセッションに指示を追加して機能をリクエストしました。

Copilot がアクセシビリティ機能の作業を続ける間に、次は[Copilot が作成したプルリクエストを反復して改善][next-lesson]します。

## 参考資料

- [Copilot agents ページ][agents-page]
- [カスタムエージェントについて][custom-agents]

---

| [← 前のレッスン: カスタムエージェント][previous-lesson] | [次のレッスン: Copilot の作業を反復する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
