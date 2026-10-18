---
title: "実践的な開発"
slug: ja-jp/real-world-development
authors:
  - geektrainer
lastUpdated: 2026-09-16
---

現実的なシナリオ、完全なアプリケーション、共有バックログを使って、ソフトウェア開発ライフサイクル全体で GitHub Copilot を活用します。変更の計画、コードの作成、動作のテスト、プルリクエストのレビュー、エージェントを使った作業管理を反復しながら進めます。

## シナリオ

あなたは、ボードゲームのクラウドファンディングを提供する架空の企業、Tailspin Toys に入社したばかりの開発者です。チームのバックログはすでに GitHub issue として登録されており、すぐに着手できます。フィルタリングやページネーションなどの機能開発のほか、アクセシビリティやコーディング標準などの品質改善も含まれています。

## 環境を選択する

GitHub Copilot は、どこで作業していても利用できます。希望する開発環境を選択してください。どのワークショップも、それぞれのセットアップ手順から始まり、共通の Tailspin Toys シナリオを使います。

### [GitHub Copilot CLI][cli]

ターミナルでエージェント型アシスタントを使います。Plan モードと Autopilot モード、インストラクション、スキル、カスタムエージェント、Playwright MCP、Agent Merge、実用的なスラッシュコマンドを扱います。

### [GitHub Copilot app][app]

複数のエージェントセッションを並行して実行し、セッションモードを切り替え、キャンバスで共同作業し、デスクトップアプリで GitHub issue とプルリクエストを管理します。

### [GitHub Copilot cloud agent][cloud]

GitHub issue を非同期エージェントに割り当て、作業を案内し、進行状況を監視して、エージェントが作成したプルリクエストをレビューします。

### [Visual Studio Code][vscode]

Visual Studio Code と GitHub Codespaces で GitHub Copilot を使います。エディターを離れずに、Copilot Chat のエージェントモード、MCP サーバー、カスタムエージェントを利用します。

[cli]: cli/
[app]: app/
[cloud]: cloud/
[vscode]: vscode/
