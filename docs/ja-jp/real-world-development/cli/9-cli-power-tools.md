---
title: "レッスン 9 - GitHub Copilot CLI のスラッシュコマンド"
description: "コンテキストの管理、モデルの選択、セッションの共有、任意のクラウド委任に使えるスラッシュコマンドを確認します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

優れた CLI ツールと同じように、GitHub Copilot CLI には多くのスラッシュコマンドがあり、Copilot を操作できます。これらのコマンドを使うと、高度な機能、内部情報、追加の構成オプションにアクセスできます。すでに `/diff`、`/mcp`、`/skills`、`/agent`、`/pr` などのコマンドを使いました。ここでは、ほかの便利なコマンドを確認します。

このレッスンでは、次のことを行います。

- `/context` と `/compact` で Copilot の会話コンテキスト管理を確認します。
- `/model` で利用できるモデルを確認します。
- `/share` でセッションをエクスポートまたは共有する方法を学びます。
- 並列作業、worktree、cloud agent への委任に使える任意のコマンドを確認します。

## シナリオ

CLI の中心的なワークフローを終えました。次は、コンテキストの管理、モデルの切り替え、セッションの共有、[Copilot cloud agent][about-cloud-agent]への任意の委任など、追加機能を確認します。

## Copilot CLI のコンテキストを確認する

大規模または複雑なタスクでは、モデルの最大コンテキストウィンドウに達する場合があります。Copilot CLI は必要に応じて会話を自動的に圧縮します。また、スラッシュコマンドでコンテキストを確認したり、手動で圧縮したりできます。

1. Codespace に戻ります。Copilot CLI が起動していない場合は、リポジトリのルートから起動します。
2. 次のコマンドを入力します。

   ```plaintext
   /context
   ```

3. モデル、現在のトークン使用量、システムインストラクション、ツール、メッセージ、空き領域にコンテキストがどのように分割されているかを確認します。
4. 会話を圧縮します。

   ```plaintext
   /compact
   ```

5. もう一度 `/context` を入力して、結果を比較します。会話がすでに短い場合は、大きな変化がないこともあります。

> [!NOTE]
> コンテキストウィンドウが埋まると、Copilot CLI は自動的にコンテキストを圧縮します。圧縮するタイミングを自分で選ぶには `/compact` を使います。関係のないタスクに切り替えて新しい会話を開始するには、`/clear` または `/new` を使います。

## モデルを選択する

モデルごとに得意分野が異なり、開発者によって好みも異なります。Copilot CLI では、モデルを一覧表示して、使用するモデルを選択できます。

1. 次のコマンドを入力します。

   ```plaintext
   /model
   ```

2. 利用可能なモデルと使用量に関する情報を確認します。
3. 現在のモデルを使い続けるか、別のモデルを選択するか、<kbd>Esc</kbd> を押して一覧を閉じます。

## セッションを共有する

チームで協力して学んだことを共有すると、AI ツールの活用方法を改善できます。`/share` コマンドでは、セッションを Markdown または HTML ファイルにエクスポートしたり、共有リンクを作成したり、GitHub gist として公開したりできます。

1. `/help` を入力し、インストール済みバージョンの `/share` オプションを確認します。
2. セッションを共有する場合は、たとえば `/share file` でローカルの Markdown ファイルにエクスポートするなど、適切な保存先を選びます。
3. 誰かに送信したり公開したりする前に、エクスポートした内容を確認します。セッションのエクスポートには、プロンプト、応答、プロジェクトの詳細が含まれることがあります。

リンクや gist の公開は任意です。チームが共有する予定のないリポジトリや会話の内容は公開しないでください。

## 任意: 作業を拡大または委任する

ワークショップの中心的な内容は完了しました。Copilot CLI には、大きなタスクに使えるコマンドもあります。

- `/fleet` は、独立したサブタスクをサブエージェントに分割して並列実行できます。
- `/worktree` は、別のタスク用に分離された Git worktree を作成できます。
- `/delegate` は、タスクを Copilot cloud agent に送り、非同期で作業させてプルリクエストを作成できます。

これらのコマンドは、追加の worktree やリモート作業が発生する場合があるため、任意の操作です。試す場合は、新しく始める対象範囲の明確なタスクから始め、通常のワークフローで結果をレビューしてください。非同期エージェントの作業を詳しく知りたい場合は、[cloud agent ワークショップ][cloud-workshop]に進みます。

## まとめと次のステップ

Copilot CLI のスラッシュコマンドを使うと、構成の変更、セッションの共有、内部の動作の確認ができます。このレッスンでは、次のことを行いました。

- `/context` と `/compact` で Copilot の会話コンテキスト管理を確認しました。
- `/model` で利用できるモデルを確認しました。
- `/share` でセッションをエクスポートまたは共有する方法を学びました。
- 並列作業、worktree、cloud agent への委任に使える任意のコマンドを確認しました。

Copilot CLI には、ほかにも多くのスラッシュコマンドや機能があります。最後に、[学んだことを振り返り][next-lesson]、今後の学習に役立つ情報を確認しましょう。

## 参考資料

- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI のコンテキスト管理][context-management]
- [Copilot cloud agent について][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
