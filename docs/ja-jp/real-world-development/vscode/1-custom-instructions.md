---
title: "演習 1 - カスタムインストラクション (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: エージェントモード →][next-lesson]

生成 AI を使ううえで、コンテキストは重要です。タスクを特定の方法で実行する必要がある場合や、Copilot に把握しておいてほしい背景情報がある場合は、そのコンテキストを参照できるようにします。Copilot に特定のコンテキストを共有する方法はいくつかあります。特に重要なのが[インストラクションファイル][instruction-files]です。コード生成のガイダンスを提供できます。

この演習では、次のことを行います。

- リポジトリのカスタムインストラクションとパスを対象とするインストラクションファイルを使って、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント標準が Copilot に伝わる方法を確認します。
- 現在のインストラクションで、フィルタリングのデータ処理の最初の部分 (発行元ヘルパー) を生成します。
- `.github/copilot-instructions.md` にリポジトリ全体で適用する新しい標準を追加します。
- 同じプロンプトを再実行し、生成コードが新しい標準に従うことを確認します。
- 次の演習で Copilot が更新済みガイダンスを使えるよう、インストラクションの更新とフィルタリング機能の最初の部分を `main` にコミットします。

> [!CAUTION]
> 生成コードが設定した標準から外れる場合があります。Copilot は確率的に動作します。この演習では、出力を一文字ずつ一致させるのではなく、インストラクションの更新後に動作がどのように変わるかを確認します。

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

- `.github/copilot-instructions.md` は、リポジトリに対する**すべての**リクエストで Copilot に送信される単一のインストラクションファイルです。技術スタック、開発内容の概要、ベストプラクティス、全体的なガイダンスなど、ほとんどのリクエストに関係するプロジェクトレベルの情報を記載します。
- `.github/instructions/*.instructions.md` ファイルは、特定のタスクやファイルの種類を対象に作成できます。TypeScript のような言語や、UI コンポーネントの作成、ユニットテストの追加といったタスクにガイドラインを指定できます。

> [!NOTE]
> IDE で作業するとき、インストラクションファイルは Copilot Chat でのコード生成にのみ使われます。コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、リポジトリレベルのインストラクションと、`applyTo` フロントマターで対象を指定した `*.instructions.md` ファイルの両方を使います。
>
> GitHub Copilot は、AGENTS.md や CLAUDE.md など、ほかの標準に準拠したインストラクションファイルも[サポートしています][custom-instructions-support]。

### インストラクションファイルを管理するためのベストプラクティス

インストラクションファイルの作成方法を詳しく説明することは、このワークショップの範囲外です。ただし、サンプルプロジェクトには代表的な方法が示されています。基本的な考え方は次のとおりです。

- `copilot-instructions.md` には、作成するもの、プロジェクトの構成、全体的なコーディング標準など、プロジェクトレベルのガイダンスを記載します。
- `*.instructions.md` ファイルは、ユニットテスト、Astro コンポーネント、データレイヤーなど、特定のファイルの種類やタスクを対象に使います。
- 自然な言葉を使って、明確なガイダンスを記載します。コードの良い例と悪い例を示してください。

AI の使い方に唯一の正解がないように、インストラクションファイルにも唯一の作成方法はありません。試しながら、プロジェクトに合った方法を見つけてください。

> [!TIP]
> GitHub Copilot を使うプロジェクトには、充実したインストラクションファイルを用意することをお勧めします。このプロジェクトのファイルを確認すると、[UI の更新][ui-instructions]や [Astro][astro-instructions] など、さまざまなタスクを対象にしたファイルがあります。
>
> Copilot にインストラクションファイルを生成してもらうこともできます。利用する環境によって操作は異なります。たとえば、VS Code では **Configure Chat → Generate Agent Instructions**、Copilot CLI では `/init` を使います。該当する環境のレッスンで、必要に応じて手順を説明します。
>
> テンプレートや出発点を探していますか？インストラクションファイル、カスタムエージェント、その他のリソースを集めた [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタムインストラクションファイルを確認する

このリポジトリに含まれるインストラクションファイルを確認します。コアとなる `copilot-instructions.md` と、さまざまなタスクを対象にした `*.instructions.md` ファイルがあります。エディターまたは GitHub の Web UI で開いてください。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの簡単な説明と、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** の下には、**GitHub Actions Workflows** のガイダンスもあります。これらは Copilot とのすべてのやり取りに適用されます。
3. `.github/instructions` フォルダーを開き、Astro ファイル、Drizzle データレイヤー、テストなどのインストラクションを確認します。
4. `.github/instructions/unit-tests.instructions.md` を開き、先頭にある `applyTo` フィールドを確認します。このフィールドでは、リポジトリルートからの glob で対象ファイルを指定します。この例では、`**/*.test.ts` に一致する TypeScript テストファイルが対象になります。
5. このプロジェクトのユニットテスト作成に関するインストラクションを確認します。
6. `.github/instructions/drizzle.instructions.md` を開き、末尾まで確認します。`unit-tests.instructions.md` などのインストラクションファイルや、プロジェクト内の既存ファイルへのリンクがあります。このように大きなインストラクションを小さく再利用可能なファイルに分割し、コード生成時に参照する例を示せます。記載されているパスはリポジトリルートではなく、インストラクションファイルからの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング標準が記載されていますが、コード内のドキュメント化はまだ求めていません。次の手順では、TSDoc のドキュメントコメントとファイルコメントヘッダーを追加します。

## 変更用のブランチを作成する

ベストプラクティスに従い、変更用のブランチを作成します。

1. 前の演習で使った Codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. 新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## インストラクションを更新する前に Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 次の演習を始める前に、Codespace に戻って Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を開始します。モードとモデルは演習ごとに異なるため、必要な手順で説明します。

カスタムインストラクションの影響を確認するため、まず現在のインストラクションファイルを使ってプロンプトを送信します。後でインストラクションを更新し、同じプロンプトを再送信して違いを確認します。

1. 前の演習で開いたエディターのタブを閉じ、Copilot が必要なコンテキストだけを取得するようにします。
2. `src/lib/publishers.ts` を開き、ヘルパーの配置場所を Copilot に示します。
3. Chat ビューのエージェントのドロップダウンから **Agent** を選択し、Copilot がファイルを変更できるようにします。

   ![Chat ビューでエージェントを選ぶ画面](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot はプロジェクトを調査し、多くの場合、ヘルパーファイルとテストにコード変更を適用します。
6. 提案されたヘルパーは、最初の引数に `db` クライアントを受け取り、型付きの発行元配列を返す関数です。これは、`src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` のデータレイヤー規約に沿ったものです。
7. 提案されたコードには、TSDoc のドキュメントコメントとファイルレベルのコメントヘッダーが**含まれていない**ことを確認します。

> [!CAUTION]
> Copilot は確率的に動作するため、指示がなくてもドキュメントコメントを追加する場合があります。その場合も問題ありません。インストラクションの更新後に動作の一貫性が向上することが、この手順のポイントです。

## リポジトリ全体に適用する標準を追加する

`.github/copilot-instructions.md` はプロジェクトレベルの情報を Copilot に提供するファイルです。コードの提案を改善するため、リポジトリのコーディング標準を記載します。

1. `.github/copilot-instructions.md` をもう一度開きます。
2. 27 行目付近にある **Code formatting requirements** セクションを探します。コーディング標準は記載されていますが、コード内のドキュメントに関するルールはありません。そのため、生成されたヘルパーにドキュメントコメントがありませんでした。
3. 既存の標準の直後に次の Markdown 行を追加し、ファイルコメントヘッダーと TSDoc のドキュメントコメントを追加するよう Copilot に指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したように、リポジトリレベルの `.github/copilot-instructions.md` は全体的なガイダンスに使い、`*.instructions.md` ファイルは特定の言語、ファイルの種類、タスクを対象にできます。今回追加したドキュメントコメントのルールのようなプロジェクト全体の標準は、リポジトリレベルのファイルに記載するのが適切です。

## プロンプトを再送信して変化を確認する

1. Copilot Chat に戻り、**New Chat** を選択して会話を新しくします。
2. `src/lib/publishers.ts` をもう一度開き、Copilot が正しいファイルに注目するようにします。
3. 先ほどと**同じプロンプト**を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 提案されたファイルの冒頭に、次のようなコメントブロックがあることを確認します。

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 提案された関数に、次のような TSDoc コメントがあることを確認します。

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

Copilot を新しいプロジェクト標準に従うようガイドし、次の演習で使う実際のコードに適用しました。

## インストラクションをコミットしてブランチをプッシュする

インストラクションファイルも、ほかの資産と同じようにソース管理で扱います。インストラクションの変更をコミットし、ブランチをリポジトリにプッシュします。

1. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
2. ターミナルで次のコマンドを実行し、インストラクションとヘルパーの変更が含まれていることを確認します。

   ```bash
   git status
   ```

3. ターミナルで、インストラクションとヘルパーの基盤として Copilot が変更したすべてのファイルをステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # git status にテストなどの追加変更が表示された場合は、それらのファイルも追加します。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. ターミナルからブランチをリポジトリにプッシュします。

   ```bash
   git push -u origin custom-instructions
   ```

## プルリクエストを作成してマージする

ブランチをプッシュしたら、バックログのドキュメント標準に関する issue と関連付けたプルリクエストを作成します。手動で行うこともできますが、接続済みの GitHub ツールを使って Copilot に依頼します。issue を探し、それをクローズする PR を作成して、マージします。

> [!NOTE]
> Copilot に GitHub 上の操作を依頼するのは、この演習が初めてです。プロジェクトのテンプレートでは GitHub ツールがワークスペースに接続済みなので、追加のセットアップは不要です。Copilot がツールを初めて使う際に、VS Code から GitHub へのサインインを求められる場合があります。画面の指示に従ってください。この接続のしくみである Model Context Protocol は後の演習で学びます。

1. Codespace で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、エージェントの選択欄で **Agent** が選択されていることを確認します。
3. インストラクションファイルの更新に関する issue を探し、現在のブランチから PR を作成するよう Copilot に依頼します。PR にはインストラクションの更新と発行元ヘルパーの基盤の両方を記載します。

   ```plaintext
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot が情報を取得して操作を行うために **Allow** を求めたら、コマンドを確認して許可します。
5. PR が作成されたら、PR をマージし、ローカルのブランチを `main` に戻すよう Copilot に依頼します。

   ```plaintext
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

6. GitHub 操作とシェルコマンドの実行について Copilot から **Allow** を求められたら、内容を確認して許可します。

GitHub Copilot の支援で PR を作成し、マージできました。

## まとめと次のステップ

このプロジェクトのインストラクションファイルから Copilot がコンテキストを取得する方法を確認し、VS Code の Copilot Chat を使って次のことを行いました。

- 既存のインストラクションでコード生成プロンプトを送り、Copilot の出力を確認しました。
- `.github/copilot-instructions.md` にリポジトリ全体で適用する新しい標準を追加しました。
- 同じプロンプトを再実行し、提案コードが新しい標準に従うことを確認しました。
- 次の演習の基盤になるインストラクションとヘルパーをコミットし、`main` に取り込みました。

次は、Copilot がコードベース全体に機能を追加する際に、[エージェントモード][next-lesson]でインストラクションを活用します。

## 参考資料

- [GitHub Copilot のカスタマイズに使うインストラクションファイル][instruction-files]
- [カスタムインストラクション作成のベストプラクティス][instructions-best-practices]
- [Copilot 向けのカスタムインストラクションを改善する 5 つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用カスタムインストラクション][personal-instructions]
- [Awesome Copilot — インストラクションファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
[awesome-copilot]: https://github.com/github/awesome-copilot
