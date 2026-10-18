---
title: "レッスン 10 - まとめと次のステップ"
description: "Copilot CLI のワークフロー、2 つのプルリクエスト、再利用可能なカスタマイズ、参考資料を振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

GitHub Copilot CLI を使って、Tailspin Toys の一連のワークフローに取り組みました。次のことを行いました。

- Codespace を準備し、Copilot CLI をインストールしてプロジェクトを調べ、用意されたフィルタリングの issue を見つけました。
- 星評価を追加し、転送されたブラウザーで結果を確認して、最初のプルリクエスト (PR) を手動でマージしました。
- フィルタリングの issue から始め、Plan モードで方針を決め、Autopilot モードで実装し、Interactive モードでレビューしました。
- カスタムインストラクションでエージェントをガイドし、既存の `quality-checks` スキルをカスタマイズして、ユニットテスト、lint、型チェックを実行しました。
- Playwright Model Context Protocol (MCP) サーバーを追加し、実際のブラウザーでフィルタリングを試しました。
- 要件、カバレッジ、スキルの結果、ブラウザー上の根拠を評価する QA カスタムエージェントを作成して選択しました。
- フィルタリングの変更全体をレビューし、フィルタリング PR に対して Agent Merge を承認しました。
- コンテキスト、モデル、共有、任意のクラウド委任に使うスラッシュコマンドを確認しました。

## リリースした機能

このワークショップでは、更新した `main` から作成した各ブランチで、2 つの PR を作成しました。

1. **星評価:** 既存の `starRating` と評価がない場合の状態をゲームカードに表示します。
2. **フィルタリングと品質ワークフロー:** フィルタリングを実装し、インストラクションを更新して機能に適用し、`quality-checks` のレポートをカスタマイズし、QA プロファイルと関連するテストを追加します。

フィルタリングの計画から PR の作成まで、同じ会話とブランチを使いました。ワークショップを簡潔にするため、これらの作業を 1 つの PR にまとめました。

## さまざまな検証方法

自動テスト、自分で行うブラウザーの確認、MCP 経由で Copilot が行うブラウザー操作など、複数の方法でコードを確認しました。`quality-checks` スキルはユニットテスト、lint、型チェックを実行し、新しい形式で結果を報告しました。QA エージェントは PR の前に、それらの結果と要件、テストカバレッジのレビューをまとめました。

追加するテストは、実際の不足を補うものである必要があります。テストを追加する必要がないという QA の結果が正しい場合もあります。マージを承認する前に、コードと検証の根拠を確認してください。変更があった場合は、影響を受ける検証をやり直してください。

## ベストプラクティス

Copilot に提供するコンテキストとツールが、その作業を方向付けます。このワークショップでは、インストラクションを更新し、スキルをカスタマイズし、QA プロファイルを作成し、MCP サーバーを構成しました。これらのカスタマイズは会話をまたいで再利用し、チームのニーズに合わせて調整できます。インストラクションは標準を定め、スキルは繰り返し行うタスクを説明し、カスタムエージェントは専門家の役割を定義し、MCP サーバーは外部ツールに接続します。エージェントの要約だけでなく、実際の変更とツールの結果を確認してください。

タスクに合った**モードとモデル**を選びます。**Plan** は実装前の検討に、**Interactive** は対象を絞った変更を確認しながら進めるのに、**Autopilot** は範囲が明確なタスクに使います。定型的な編集には高速なモデルを、複雑な作業には高性能なモデルを選びます。

ツールやインフラストラクチャと同様に、コンテキストも重要です。*何を*作りたいか、*なぜ*作るか、*どのように*作るかを明確に伝えると、生成結果が大きく変わります。

## さらに試す

中心的なワークフローを学びました。Copilot CLI で試せる機能をいくつか紹介します。

- `/review` でコードレビューエージェントに変更の分析を依頼します。
- `/rubber-duck` で問題について話し合い、別の視点を得ます。
- `/fleet` で独立したサブタスクを並列に調整します。
- `/worktree` で別のタスクを分離します。
- `/delegate` で Copilot cloud agent にタスクを送ります。

## 次のステップ

どのツールも、使い続けることが上達への近道です。本番コード、個人プロジェクト、長年作りたいと思っていた小さなアプリなどで活用してください。チームで学びを共有し、お互いの経験から学びましょう。ドキュメントも引き続き確認してください。

ターミナルから Tailspin Toys をさらに拡張したい場合は、任意の [Foundry Backer Concierge シリーズ][foundry]に進みます。ほかの環境と比較するには、[VS Code ワークショップ][vscode]、[GitHub Copilot app ワークショップ][app]、[Copilot cloud agent ワークショップ][cloud]を確認してください。

## 参考資料

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI をカスタマイズする][customize-cli]
- [Copilot CLI でプルリクエストを管理する][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
