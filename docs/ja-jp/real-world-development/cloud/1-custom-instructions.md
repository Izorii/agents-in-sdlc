---
title: "演習 1 - カスタムインストラクション (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot cloud agent →][next-lesson]

生成 AI を使ううえで、コンテキストは重要です。タスクを特定の方法で実行する必要がある場合や、Copilot に把握しておいてほしい背景情報がある場合は、そのコンテキストを参照できるようにします。[インストラクションファイル][instruction-files]を使うと、Copilot に何をしてほしいかだけでなく、どのように作業してほしいかも伝えられます。

この演習では、次のことを行います。

- リポジトリのカスタムインストラクションとパスを対象とするインストラクションファイルを使って、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント標準が Copilot に伝わる方法を確認します。
- `.github/copilot-instructions.md` にリポジトリ全体で適用する新しい標準を追加します。

> [!NOTE]
> VS Code や CLI のハーネスと異なり、この演習ではプロンプトの変更前と変更後を比較しません。Copilot cloud agent は GitHub issue に対して非同期で作業するため、リアルタイムに並べて効果を確認するのが難しいからです。このハーネスの後半で cloud agent が作成したプルリクエストをレビューするときに、インストラクションファイルの影響を確認できます。

## インストラクションファイル

### シナリオ

優れた開発会社である Tailspin Toys には、開発方法に関するガイドラインと要件があります。たとえば、次のようなものです。

- データレイヤーには常にユニットテストが必要です。
- UI はダークモードにし、モダンな印象にします。
- TSDoc のドキュメントコメントを使ってコードを文書化します。
- 各ファイルの先頭に、そのファイルの役割を説明するコメントブロックを追加します。

インストラクションファイルを使ってこれらのガイドラインを Copilot に伝え、作業がプロジェクトの標準に沿うようにします。

### カスタムインストラクション

カスタムインストラクションを使うと、コンテキストや設定を Copilot に伝え、コーディングスタイルや要件を理解させることができます。優先するコーディング規約やライブラリ、コードに含めるコメントの種類などを指定して、より適切な提案やコードスニペットを得られます。リポジトリ全体、または特定のファイルやタスクを対象にインストラクションを作成できます。

インストラクションファイルには、次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリに対する**すべての**リクエストで Copilot に送信される単一のインストラクションファイルです。Copilot Chat や CLI へのリクエストのほとんどに関係する、技術スタック、開発内容の概要、ベストプラクティス、全体的なガイダンスなどのプロジェクトレベルの情報を記載します。
- `.github/instructions/*.instructions.md` ファイルは、特定のタスクやファイルの種類を対象に作成できます。TypeScript のような言語や、UI コンポーネントの作成、ユニットテストの追加といったタスクに合わせてガイドラインを指定できます。

> [!NOTE]
> IDE で作業するとき、インストラクションファイルは Copilot Chat でのコード生成にのみ使われます。コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、リポジトリレベルのインストラクションファイルと、`applyTo` フロントマターで対象を指定した `*.instructions.md` ファイルの両方を使います。
>
> さらに、GitHub Copilot は AGENTS.md や CLAUDE.md など、ほかの標準に準拠したインストラクションファイルも[サポートしています][custom-instructions-support]。

### インストラクションファイルを管理するためのベストプラクティス

インストラクションファイルの作成方法について詳しく説明することは、このワークショップの範囲外です。ただし、サンプルプロジェクトには代表的な方法が示されています。基本的な考え方は次のとおりです。

- `copilot-instructions.md` には、作成するもの、プロジェクトの構成、全体的なコーディング標準など、プロジェクトレベルのガイダンスを記載します。
- `*.instructions.md` ファイルは、ユニットテスト、Astro コンポーネント、データレイヤーなど、特定のファイルの種類やタスクを対象に使います。
- 自然な言葉を使って、明確なガイダンスを記載します。コードの良い例と悪い例を示してください。

AI の使い方に唯一の正解がないように、インストラクションファイルにも唯一の作成方法はありません。プロジェクトに適した方法は、試しながら見つけていきます。

> [!TIP]
> GitHub Copilot を使うプロジェクトには、充実したインストラクションファイルを用意することをお勧めします。このプロジェクトのファイルを確認すると、[UI の更新][ui-instructions]や [Astro][astro-instructions] など、さまざまなタスクを対象にしたファイルがあります。
>
> Copilot にインストラクションファイルを生成してもらうこともできます。利用する環境によって操作は異なります。たとえば、VS Code では **Configure Chat → Generate Agent Instructions**、Copilot CLI では `/init` を使います。該当する環境のレッスンで、必要に応じて手順を紹介します。
>
> テンプレートや出発点を探していますか？インストラクションファイル、カスタムエージェント、その他のリソースを集めたリポジトリ、[awesome-copilot][awesome-copilot]を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタムインストラクションファイルを確認する

このリポジトリに含まれるインストラクションファイルを確認します。コアとなる `copilot-instructions.md` と、さまざまなタスクを対象にした `*.instructions.md` ファイルがあります。エディターまたは GitHub の Web UI で開いてください。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの簡単な説明と、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** の下には、**GitHub Actions Workflows** のガイダンスもあります。これらは Copilot を使うすべてのやり取りに適用されます。
3. `.github/instructions` フォルダーを開いて内容を確認します。Astro ファイル、Drizzle データレイヤー、テストなどのインストラクションがあります。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドを確認してください。このフィールドでは、インストラクションを適用するファイルをリポジトリルートからの glob で指定します。この例では、`**/*.test.ts` に一致する TypeScript テストファイルが対象になります。
5. このプロジェクトのユニットテスト作成に関するインストラクションを確認します。
6. 最後に `.github/instructions/drizzle.instructions.md` を開き、末尾までスクロールします。`unit-tests.instructions.md` などのほかのインストラクションファイルや、プロジェクト内の既存ファイルへのリンクを確認します。このように大きなインストラクションを小さく再利用できるファイルに分割し、コード生成時に参照できる例を示します。ここに記載されているパスはリポジトリルートではなく、インストラクションファイルからの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションには、プロジェクトのコーディング標準が記載されていますが、コード内のドキュメント化はまだ求めていません。次の手順では、TSDoc のドキュメントコメントとファイルコメントヘッダーを追加します。

## リポジトリ全体に適用する標準を追加する

ここで行う編集は、コード内に TSDoc のドキュメントコメントとファイルレベルのコメントヘッダーを追加するルールを、プロジェクト全体に設定することです。cloud agent は、このハーネスの後半で割り当てる issue に取り組むときに、このルールを参照します。

編集する前に作業用のブランチを作成します。cloud agent は issue の対象ブランチからインストラクションを読み取りますが、ここでは変更を適切にコミットする手順も確認します。

1. Codespace のターミナルで、新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

前のセクションで説明したように、`.github/copilot-instructions.md` はプロジェクトレベルの情報を Copilot に提供するファイルです。リポジトリのコーディング標準にドキュメントのルールを追加して、コードの提案を改善しましょう。

1. `.github/copilot-instructions.md` をもう一度開きます。
2. 27 行目付近にある **Code formatting requirements** セクションを探します。プロジェクトのコーディング標準が記載されていますが、コード内のドキュメントに関するルールはありません。そのため、生成されたヘルパー関数にドキュメントコメントがありませんでした。
3. 次の Markdown 行を既存の標準の直後に追加し、Copilot にファイルコメントヘッダーと TSDoc のドキュメントコメントを追加するよう指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したように、リポジトリレベルの `.github/copilot-instructions.md` は全体的なガイダンスに使い、`*.instructions.md` ファイルは特定の言語、ファイルの種類、タスクを対象にできます。今回追加したドキュメントコメントのルールのようなプロジェクト全体の標準は、リポジトリレベルのファイルに記載するのが適切です。

## インストラクションの更新をコミット、プッシュ、マージする

cloud agent は、issue の対象ブランチにあるインストラクションファイルを読み取ります。次の演習で issue を Copilot に割り当てると、Copilot は `main` からブランチを作成します。そのため、cloud agent がインストラクションを使えるように、変更を `main` に取り込む必要があります。

1. 変更をステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチをプッシュします。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` に向けたプルリクエストを作成し、マージします。次の演習で issue を割り当てると、cloud agent はこのインストラクションを参照できます。

> [!TIP]
> このワークショップで `main` に直接作業したい場合は、ブランチの作成を省略して `main` に直接コミットできます。ここでは、実際のプロジェクトで行う方法に近づけるため、ブランチを作成しています。

## まとめと次のステップ

Copilot がこのプロジェクトのインストラクションファイルからコンテキストを取得する方法を確認し、リポジトリ全体に適用する標準を `.github/copilot-instructions.md` に追加しました。このハーネスの後半で cloud agent が作成するプルリクエストを確認すると、追加した標準が適用されていることが分かります。

次に、[初めての issue を Copilot cloud agent に割り当てます][next-lesson]。

## 参考資料

- [GitHub Copilot のカスタマイズに使うインストラクションファイル][instruction-files]
- [カスタムインストラクション作成のベストプラクティス][instructions-best-practices]
- [Copilot 向けのカスタムインストラクションを改善する 5 つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — インストラクションファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
