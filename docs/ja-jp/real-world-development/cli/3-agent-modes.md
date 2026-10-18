---
title: "レッスン 3 - エージェントモード: Plan と Autopilot"
description: "Plan で実装方針に合意し、Autopilot で issue に基づくフィルタリング機能を作り、Interactive モードで結果をレビューします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

最初に、小さな機能をプロジェクトに追加しました。より大きな変更には、さらにしっかりしたプロセスが必要です。GitHub Copilot CLI は組織の既存の開発フローに沿って作業できるため、適切な方法で必要な機能を作成できます。このレッスンから、エージェント主導の一般的な開発プロセスに沿って進めます。issue から機能を作成し、コードの妥当性と機能の動作を確認して、最後にプロジェクトへマージします。

> [!NOTE]
> 機能開発の一連の作業では、同じ会話とブランチを使います。通常は、作業するファイルの種類ごとに異なるブランチや PR を使いますが、ここでは中心的な概念に集中できるよう簡略化しています。

このレッスンでは、次のことを行います。

- GitHub issue から Copilot の新しい会話を開始します。
- Plan モードで要件を定義します。
- Autopilot モードで新機能を実装します。
- コードをレビューします。
- 転送されたブラウザーで機能を手動検証します。

この機能開発を続けながら、リポジトリのインストラクションを更新し、既存の `quality-checks` スキルをカスタマイズし、MCP による検証と QA エージェントを追加して、機能の PR を作成します。

## シナリオ

Tailspin Toys のカタログが拡大し、訪問者がカテゴリや発行元でゲームを絞り込めるようにする必要があります。バックログの issue には機能が記載されていますが、カテゴリを組み合わせる方法など、コーディング前に合意すべき詳細があります。Plan モードで決定事項を整理し、範囲を定めた実装を Autopilot に任せます。

## 背景

AI コーディングエージェントを開発フローに取り入れても、基本は変わりません。むしろ、基本がより重要になります。多くの開発者は、次のような流れで作業します。

1. 必要な作業を説明する issue から始めます。
2. 実装内容を計画します。
3. コードを作成してレビューします。
4. テストを実行してコードを検証します。
5. 新機能を手動で検証します。
6. プルリクエスト (PR) を作成します。
7. コードレビューと継続的インテグレーションが成功したら、コードをマージします。

> [!NOTE]
> 実際の手順はチームや組織によって異なりますが、多くの場合、上記の流れを少し変えたものになります。

この標準的な方法に従うことで、AI が生成したコードが要件を満たし、手作業のコードと同じ検証プロセスを受けるようにできます。

## 会話モード

**会話モード**は、エージェントにどの程度の自律性を与えるかを制御します。<kbd>Shift</kbd>+<kbd>Tab</kbd> を押すと、モードを切り替えられます。

- **Interactive**: エージェントが変更を提案し、次に進む前にあなたの入力を待ちます。
- **Plan**: エージェントが先に計画を作成します。プロジェクトファイルの編集はできません。
- **Autopilot**: エージェントが自律的にコードを書き、テストを実行し、タスクが完了するまで反復します。

まず Plan モードで計画を確認し、その後 Autopilot で実装します。

## issue から始める

フィルタリングの作業を始める前に、Codespace に戻り、リポジトリとターミナルを準備します。

1. Codespace に戻ります。停止している場合は再起動します。
2. 星評価の PR がマージ済みであることを確認します。
3. ターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押します。
4. `main` を更新し、フィルタリング用ブランチを作成します。

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Copilot CLI を起動します。

   ```bash
   copilot --yolo
   ```

6. <kbd>Tab</kbd> を 2 回押して **Issues** タブを開きます。
7. <kbd>A</kbd> を押してすべての issue を表示します。
8. 矢印キーで **Allow users to filter games by category and publisher** というタイトルの issue を選択します。
9. <kbd>C</kbd> を押して issue をプロンプトに追加し、**Session** タブに戻ります。

プロンプトの先頭に `#7` (または同様の番号) が追加されます。`#` を使うと、GitHub の issue やプルリクエスト (PR) をコンテキストに追加できます。

## フィルタリング機能を計画する

計画を立てると、Copilot に任せる前に、機能やタスクの実装方法を定義できます。複雑な作業では、少し時間をかけて計画するのが効果的です。Plan モードに切り替えて、計画を作成するよう Copilot に依頼します。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Plan モードに切り替えます。プロンプトの下に **Plan** と表示されていることを確認します。
2. 直前の手順で追加した issue の参照に続けて、次のプロンプトを入力します。

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot はプロジェクトを調査し、最適な方法を検討して計画を作成します。

3. フィルタリングの動作について質問されたら、希望に沿って回答します。どの選択肢にも間違いはありません。
4. 計画が完成したら、<kbd>Control</kbd>+<kbd>E</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) を押して計画を展開します。
5. 計画の内容を確認します。
6. 決定事項と異なる部分があれば、修正を依頼します。

## Autopilot を承認する

計画を作成してレビューできたら、実装に進みます。Autopilot を使って Copilot に作業を任せましょう。

Autopilot では、タスクが完了したと Copilot が判断するまで、問題に対して反復的に取り組みます。

1. **Accept plan and build on autopilot (recommended)**、またはインストール済みのバージョンで同様のラベルの選択肢を選びます。
2. プロンプトの下に **Autopilot** と表示されていることを確認します。
3. Copilot が確定した計画に沿って反復し、コードを生成してテストを実行する様子を確認します。

> [!NOTE]
> 承認するとすぐに実装が始まる場合があります。先に計画をレビューしてください。依存関係が不足している、またはポートが競合していると Copilot が報告した場合は、チェック完了と見なす前にセットアップの問題を解決します。

## 実装をレビューして検証する

コードが生成されたら、ほかのコードと同じようにマージ前にレビューします。コードを確認し、サイトを実行して問題がないことを確かめます。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Interactive モードに切り替えます。モード表示に **Plan** や **Autopilot** がないことを確認します。
2. `/diff` を入力してフィルタリングの実装とテストを確認します。
3. issue と計画時に決めた内容に合っていることを確認します。
4. レビューが完了したら、<kbd>Esc</kbd> を押して差分表示を終了します。
5. プロジェクトのチェック結果を確認し、失敗があれば修正を依頼します。

## 新機能を試す

コードは良さそうですが、動作するでしょうか。以前と同じようにアプリを起動し、Codespaces の転送ポートを通してサイトを開きます。

1. Copilot にアプリの起動を依頼します。

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces でポート `4321` が使用可能になったら、**Open in Browser** を選択します。
3. 計画で合意した方法に沿って、カテゴリ、発行元、組み合わせを使ったフィルタリングを試します。
4. リセットと結果がない場合の動作が、issue と決定事項に合っていることを確認します。
5. Codespace に戻り、起動した開発サーバーを停止するよう Copilot に依頼します。

## まとめと次のステップ

異なる会話モードを使って機能を作成し、レビューしました。このレッスンでは、次のことを行いました。

- GitHub issue から Copilot の新しい会話を開始しました。
- Plan モードで要件を定義しました。
- Autopilot モードで新機能を実装しました。
- コードをレビューしました。
- 転送されたブラウザーで機能を手動検証しました。

次は、[カスタムインストラクション][next-lesson]を使って、コードが文書化された方法に沿って生成されるようにします。

## 参考資料

- [GitHub Copilot CLI の Autopilot][autopilot]
- [Copilot CLI コマンド リファレンス][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
