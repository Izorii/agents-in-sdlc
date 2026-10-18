---
title: "レッスン 4 - カスタムインストラクションで Copilot をガイドする"
description: "リポジトリのインストラクションを確認し、ドキュメント標準を追加して、フィルタリングのコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を使ううえで、コンテキストは重要です。タスクを特定の方法で実行する必要がある場合は、そのガイダンスを Copilot が参照できるようにします。[インストラクションファイル][instruction-files]では、必要なコードの内容だけでなく、コードの構成方法も指定できます。フィルタリングを実装したので、Copilot が使ったインストラクションを確認し、ドキュメント標準を追加して、コードに適用します。

このレッスンでは、次のことを行います。

- リポジトリのインストラクションとパスを対象とするインストラクションファイルがエージェントに適用される方法を確認します。
- コーディング標準を守るためにインストラクションファイルを更新します。
- インストラクションファイルがコードに与える影響を確認します。

## シナリオ

優れた開発会社である Tailspin Toys には、開発方法に関するガイドラインと要件があります。たとえば、次のようなものです。

- コメントにはコードの言い換えではなく、意図や自明でない判断を記載します。
- `db/` と `src/lib/` のエクスポート関数には、`db` 引数を注入できる場合はその内容も含めて、目的、パラメーター、戻り値を説明する TSDoc/JSDoc を記載します。
- 再利用可能な Astro コンポーネントでは `Props` の仕様を文書化し、関連するコードが変更された場合はコメントも最新に保ちます。
- 既存のフォーマットと lint のガイダンスを維持します。

インストラクションファイルを使ってこれらのガイドラインを Copilot に伝え、作業がプロジェクトの標準に沿うようにします。

## インストラクションファイル

カスタムインストラクションを使うと、コンテキストや設定を Copilot に伝え、コーディングスタイルや要件を理解させることができます。優先するコーディング規約やライブラリ、コードに含めるコメントの種類などを指定して、より適切な提案やコードスニペットを得られます。リポジトリ全体、または特定のファイルやタスクを対象にインストラクションを作成できます。

インストラクションファイルには、次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリに対する**すべての**リクエストで Copilot に送信される単一のインストラクションファイルです。ほとんどのリクエストに関係するプロジェクトレベルの情報を記載します。
- `.github/instructions/*.instructions.md` ファイルは、特定の言語、ファイルの種類、タスクに関するガイドラインを記載します。

> [!NOTE]
> インストラクションのほかの形式やサポート状況は、利用する環境によって異なります。特定の形式を前提にする前に、[カスタムインストラクションのサポート情報][custom-instructions-support]を確認してください。

## このプロジェクトのカスタムインストラクションファイルを確認する

スタータープロジェクトには、インストラクションファイルがあらかじめ含まれています。変更の影響を確認するため、まず既存のファイルを確認します。

1. Codespace に戻ります。
2. ターミナルではなく Codespaces エディターで `.github/copilot-instructions.md` を開きます。
3. プロジェクトの簡単な説明とコーディングガイダンスを確認します。このインストラクションは、このリポジトリでの Copilot とのすべてのやり取りに適用されます。
4. `.github/instructions` フォルダーを開き、Astro ファイル、Drizzle データレイヤー、テストなどのインストラクションを確認します。
5. `.github/instructions/unit-tests.instructions.md` を開き、対象ファイルを指定する `applyTo` フィールドを確認します。
6. `.github/instructions/drizzle.instructions.md` を開き、ほかのインストラクションファイルや既存のプロジェクトファイルへのリンクを確認します。大きなインストラクションを小さく再利用可能なファイルに分割し、参照例を示せます。

## チームのガイダンスに合わせてインストラクションファイルを更新する

既存のファイルは良い出発点ですが、改善の余地があります。コアの `copilot-instructions.md` を変更して、新しく生成する TypeScript に TSDoc コメントを追加するようにします。

1. `.github/copilot-instructions.md` で、35 行目付近にある **Code formatting guidance** セクションを探します。
2. 最後の箇条書きとして、次の行を追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

ファイルは自動的に保存されます。

## 更新したガイダンスを使う

Copilot CLI は会話の開始時にリポジトリのインストラクションを読み込みます。インストラクションを保存したら、フィルタリングの会話を終了して再開します。機能のコンテキストを保ったまま、新しいガイダンスを読み込めます。

1. フィルタリングの会話が開いている場合は、`/exit` を入力して終了します。Copilot CLI は会話を自動的に保存します。
2. リポジトリのルートで `game-filters-cli` ブランチから保存した会話を再開します。

   ```bash
   copilot --resume
   ```

   プロンプトが表示されたら、フィルタリングの会話を選択し、`game-filters-cli` ブランチがチェックアウトされていることを確認します。
3. 更新したガイダンスを適用するよう Copilot に依頼します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. `/diff` を入力し、変更された TypeScript ファイルを確認します。追加された TSDoc コメントがコードを正確に説明していることを確認します。

## まとめと次のステップ

Copilot がインストラクションファイルからコンテキストを取得する方法を確認し、新しい標準を機能に適用しました。具体的には、次のことを行いました。

- リポジトリのインストラクションとパスを対象とするインストラクションファイルがエージェントに適用される方法を確認しました。
- コーディング標準を守るためにインストラクションファイルを更新しました。
- インストラクションファイルがコードに与える影響を確認しました。

次は、再利用可能な `quality-checks` スキルを[カスタマイズして実行][next-lesson]し、lint とテストを一貫して実施します。

## 参考資料

- [Copilot CLI にカスタムインストラクションを追加する][instruction-files]
- [カスタムインストラクションのサポート][custom-instructions-support]
- [カスタムインストラクション作成のベストプラクティス][instructions-best-practices]
- [Awesome Copilot — インストラクションファイルなどのリソース集][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
