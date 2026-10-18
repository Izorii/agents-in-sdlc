---
slug: ja-jp/real-world-development/cloud
title: "GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

**[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent)** を使うと、GitHub Copilot がクラウド上で非同期に作業できます。GitHub で作業を割り当てると、cloud agent はバックグラウンドでリポジトリを調べ、変更を加えてプルリクエストを作成します。その間、ほかの作業を進められます。

この演習では、まず cloud agent が従うカスタムインストラクションを追加し、GitHub issue を割り当てて実装を任せます。カスタムエージェントをレビューして使い、進め方を調整します。また、agents ダッシュボードからセッションを監視して指示を出し、最後にエージェントが作成したプルリクエストをレビューし、結果を改善します。

## 演習

| 演習 | トピック | 説明 |
|----------|-------|-------------|
| [0. 前提条件][ex0] | セットアップ | リポジトリと Codespace を作成します |
| [1. カスタムインストラクション][ex1] | コンテキスト | cloud agent が従うカスタムインストラクションを追加します |
| [2. Cloud Agent][ex2] | 非同期エージェント | GitHub issue を Copilot cloud agent に割り当てます |
| [3. カスタムエージェント][ex3] | 特化型エージェント | カスタムエージェントをレビューして使います |
| [4. エージェントの管理][ex4] | 監視 | エージェントセッションを監視し、作業に指示を出します |
| [5. 反復][ex5] | レビュー | プルリクエストをレビューし、Copilot の作業を改善して、次のステップを選びます |

## 前提条件

このワークショップに参加する前に、次の準備をしてください。

- [ ] 有効な **Copilot Student、Pro、Pro+、Business、Enterprise** プランを利用できる GitHub アカウント
- [ ] アカウントまたは組織で Copilot cloud agent が有効になっていること

> [!TIP]
> 有料プランを利用していませんか？認証済みの学生は、[GitHub Education][callout-student-plan-education] から GitHub Copilot を無料で利用できます。**Copilot Student** プランには、このワークショップで使うエージェント、MCP、コードレビュー、Copilot CLI の機能が含まれているため、どのハーネスも最後まで実施できます。

[callout-student-plan-education]: https://github.com/education/students

> [!NOTE]
> Cloud agent は **Copilot Student、Pro、Pro+、Business、Enterprise** で利用できます。Copilot Free を除くすべてのプランが対象です。Copilot Business または Enterprise では、管理者が組織で有効にする必要があります。

> [!NOTE]
> このハーネスでは MCP を扱いません。Copilot で MCP サーバーを使う方法については、[CLI ハーネス](../cli/)または [VS Code ハーネス](../vscode/)を参照してください。

## 始める

**[演習 0: 前提条件から始める →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-custom-instructions/
[ex2]: 2-cloud-agent/
[ex3]: 3-custom-agents/
[ex4]: 4-managing-agents/
[ex5]: 5-iterating/
