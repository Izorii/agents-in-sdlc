---
title: "レッスン 8 - 機能の PR を作成してマージする"
description: "フィルタリング、インストラクション、スキルの更新、QA プロファイル、テストをまとめてレビューし、PR を作成して Agent Merge を使います。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

フィルタリングの実装、インストラクションの更新、スキルの更新、品質保証 (QA) プロファイル、テストは、1 つのブランチに保存されています。まとめてレビューして、プルリクエストを作成しましょう。星評価のプルリクエスト (PR) は自分でマージしましたが、今回は **Agent Merge** にプロセスを任せます。

> [!NOTE]
> 通常は、機能、インストラクションの更新、スキルの更新、QA エージェントをいくつかの PR に分けます。このワークショップでは手順を簡潔にするため、フィルタリングと品質ワークフロー全体を 1 つの会話とブランチで行い、この PR にまとめます。

このレッスンでは、次のことを行います。

- Agent Merge が PR のマージライフサイクルを自動化する方法を学びます。
- 機能全体の変更と検証の根拠を確認します。
- フィルタリング機能の PR を作成します。
- レビュー後にのみ Agent Merge を有効にし、PR がマージされたことを確認します。

## シナリオ

フィルタリング機能の開発を通じて、Copilot で機能を計画、実装、検証しました。Tailspin Toys では、開発者がマージの承認権限を保持しながら、残りの PR 作業を自動化したいと考えています。

## Agent Merge とは

**Agent Merge** は、プルリクエストをマージするために必要な残りの作業を自動化します。有効にすると、Copilot は PR をブロックしている項目 (失敗した継続的インテグレーション (CI) チェック、レビューコメントへの対応、必要な場合のリベースなど) に取り組みます。その後、リポジトリで許可されていれば GitHub の自動マージを有効にします。

これまでは自分で **Merge pull request** を選択していました。Agent Merge にその作業を任せることができます。有効にする前に作業をレビューし、マージ可能であると判断してください。

## Agent Merge で PR を管理する

すべてのコードを作成できたので、まとめてレビューし、PR を作成して、残りのプロセスを Agent Merge に任せます。

1. Codespace に戻ります。
2. `/agent` を入力してエージェントのダイアログを開きます。
3. 一覧から **Default** を選択し、<kbd>Enter</kbd> を押します。
4. `/pr create` コマンドで PR を作成します。
5. `/pr agentmerge` で Agent Merge を有効にします。
6. Copilot が PR の継続的インテグレーションを監視します。すべてのチェックが成功すると、マージが実行されます。
7. Copilot から「PR #14 was squash-merged successfully.」などのメッセージが届いたことを確認します。

> [!IMPORTANT]
> Agent Merge は、必須の承認、ブランチ保護、マージキュー、リポジトリ設定、不足しているアクセス許可を回避しません。ブロックされた場合は、報告された理由を確認し、リポジトリで許可されている場合はレビュー済みの変更を手動でマージしてください。

## まとめと次のステップ

コード生成、テストと検証、プルリクエストの処理など、開発プロセスの一部を自動化しました。このレッスンでは、次のことを行いました。

- Agent Merge が PR のマージライフサイクルを自動化する方法を学びました。
- 機能全体の変更と検証の根拠を確認しました。
- フィルタリング機能の PR を作成しました。
- レビュー後に Agent Merge を有効にし、PR がマージされたことを確認しました。

次は、コンテキスト、モデル、共有、任意のクラウド委任に使える[便利な Copilot CLI スラッシュコマンド][next-lesson]を確認します。

## 参考資料

- [Copilot CLI でプルリクエストを管理する][manage-prs]
- [Copilot CLI コマンド リファレンス][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
