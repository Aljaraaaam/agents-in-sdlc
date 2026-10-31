---
title: "演習 1 - カスタム指示 (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前の演習: 前提条件][previous-lesson] · [次の演習: Copilot cloud agent →][next-lesson]

生成型人工知能 (AI) を使うときは、コンテキストが重要です。タスクの進め方に決まりがある場合や、Copilot に知っておいてほしい背景情報がある場合は、その情報を参照できるようにします。[指示ファイル][instruction-files]を使うと、Copilot に実行内容だけでなく方法も伝えられます。

この演習では、次の作業を行います。

- リポジトリのカスタム指示とパス別の指示ファイルを通じて、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント規約が Copilot に伝わる方法を確認する。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する規約を追加する。

> [!NOTE]
> VS Code やコマンドラインインターフェイス (CLI) のハーネスとは異なり、この演習では変更前後のプロンプトを比較しません。Copilot cloud agent は GitHub Issue に対して非同期に動作するため、リアルタイムの比較が難しいためです。このハーネスの後半で cloud agent が作成したプルリクエストをレビューするときに、指示ファイルの影響を確認します。

## 指示ファイル

### シナリオ

Tailspin Toys には、開発に関するガイドラインと要件があります。たとえば、次の規約です。

- データ層には必ず単体テストを用意する。
- ユーザーインターフェイス (UI) はダークモードにし、現代的な印象にする。
- TypeScript Documentation (TSDoc) のドキュメントコメントをコードに追加する。
- 各ファイルの先頭に、ファイルの役割を説明するコメントブロックを追加する。

指示ファイルを使い、Copilot がこれらの開発慣行に沿ってタスクを実行できるようにします。

### カスタム指示

カスタム指示は、コーディングスタイルや要件に関するコンテキストと希望を Copilot に伝えます。コーディング規約、ライブラリ、コードに含めるコメントの形式などを指定でき、より適切な提案やコード生成に役立ちます。リポジトリ全体または特定のファイル種別に指示を設定できます。

指示ファイルには次の2種類があります。

- `.github/copilot-instructions.md` は、リポジトリへのすべてのリクエストで Copilot に送られる単一の指示ファイルです。技術スタック、開発中の内容、ベストプラクティスなど、ほとんどのチャットや CLI リクエストに関係するプロジェクト全体の情報を記載します。
- `.github/instructions/*.instructions.md` は、特定のタスクやファイル種別に指針を与えるファイルです。TypeScript や Astro などの言語、UI コンポーネントの作成、単体テストの作成といったタスクに利用できます。

> [!NOTE]
> 統合開発環境 (IDE) で指示ファイルがコード生成に使われるのは Copilot Chat の場合のみです。コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、リポジトリレベルの指示と `applyTo` フロントマター付きの `*.instructions.md` ファイルを使用します。
>
> Copilot は、AGENTS.md や CLAUDE.md など、ほかの規格の指示ファイルにも対応しています。[対応する指示ファイルの詳細][custom-instructions-support]を確認してください。

### 指示ファイルを管理するためのベストプラクティス

指示ファイルの作成方法をすべて扱うことは、このワークショップの範囲外です。サンプルプロジェクトの例から、代表的な方法を確認できます。基本的には、次の点に注意します。

- `copilot-instructions.md` には、プロジェクトの説明、構成、全体に適用するコーディング規約など、プロジェクトレベルの指針を記載します。
- `*.instructions.md` は、単体テスト、Astro コンポーネント、データ層などのファイル種別や、特定のタスク向けの指針に使用します。
- 自然な言葉で、明確な指示とコード例を記載します。

指示ファイルや AI の使い方に唯一の正解はありません。プロジェクトや環境に合わせて試しながら、適切な方法を見つけてください。

> [!TIP]
> GitHub Copilot を使うプロジェクトには、十分な指示ファイルを用意してください。このプロジェクトには、[UI 更新][ui-instructions]や [Astro][astro-instructions]など、さまざまなタスク向けのファイルがあります。
>
> Copilot に指示ファイルを作成してもらうこともできます。各環境で方法は異なります。たとえば、VS Code では **Configure Chat → Generate Agent Instructions**、Copilot CLI では `/init` を使います。環境に応じたレッスンで詳しく説明します。
>
> テンプレートや出発点を探すには、指示ファイル、カスタムエージェント、その他のリソースを集めた [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム指示ファイルを確認する

このリポジトリに含まれる、共通の `copilot-instructions.md` と各タスク向けの `*.instructions.md` を確認します。エディターまたは GitHub の Web UI で開いてください。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの概要に加え、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。これらは Copilot とのあらゆるやりとりに適用されます。
3. `.github/instructions` フォルダーを開いて内容を確認します。Astro ファイル、Drizzle データ層、テストなどの指示ファイルがあります。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドが、リポジトリルートを基準とした glob を定めています。この例では、`**/*.test.ts` に一致する TypeScript のテストファイルなどが対象です。
5. このプロジェクトの単体テスト作成に関する指示を確認します。
6. `.github/instructions/drizzle.instructions.md` を開き、末尾まで確認します。`unit-tests.instructions.md` などの指示ファイルや既存ファイルへのリンクがあります。大きな指示を再利用可能な小さなファイルに分け、コード生成の参考例を Copilot に示せます。ここにあるパスはリポジトリルートではなく、指示ファイルからの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング規約が記載されていますが、コード内のドキュメント作成はまだ必須ではありません。次の手順で、TSDoc のドキュメントコメントとファイルコメントのヘッダーに関する規則を追加します。

## リポジトリの規約を追加する

ここでは、TSDoc のドキュメントコメントとファイルコメントのヘッダーをコードに記載するという、プロジェクト全体の規則を追加します。後の演習で Issue を割り当てるとき、cloud agent がこの規則に従います。

編集する前に、作業用のブランチを作成します。cloud agent は Issue の対象ブランチにある指示を読み取りますが、ここでは変更を個別にコミットします。

1. Codespace のターミナルで、新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

`.github/copilot-instructions.md` はプロジェクト全体の情報を Copilot に提供します。コーディング規約を記載し、提案の質を高めます。

1. `.github/copilot-instructions.md` を再度開きます。
2. 27 行目付近にある **Code formatting requirements** セクションを見つけます。既存の規約を確認します。生成されたヘルパーにドキュメントコメントがなかったのは、この規則がまだないためです。
3. 既存の規約の直後に、次の Markdown 行を追加し、ファイルコメントのヘッダーと TSDoc のドキュメントコメントを Copilot に指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前の演習で確認したように、指示ファイルはリポジトリレベル (`.github/copilot-instructions.md`) に置いて全体に適用することも、特定の言語、ファイル種別、タスク向けに `*.instructions.md` として作成することもできます。今回追加するドキュメントコメント規則のような、プロジェクト全体の標準にはリポジトリレベルのファイルが適しています。

## 指示の変更をコミット、プッシュ、マージする

cloud agent は、Issue の対象ブランチにある指示ファイルを読み取ります。次の演習で Issue を割り当てると、Copilot は `main` からブランチを作成します。そのため、cloud agent が指示を読み取れるよう、変更を `main` にマージする必要があります。

1. 変更をステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチをプッシュします。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` に向けたプルリクエストを作成してマージします。次の演習で Issue を割り当てる際、cloud agent はこの指示を読み取ります。

> [!TIP]
> このワークショップでは `main` に直接作業してもかまいません。その場合、ブランチ作成を省略して `main` に直接コミットできます。ここでは実際のプロジェクトに近い手順を体験できるよう、ブランチを使っています。

## まとめと次のステップ

このプロジェクトの指示ファイルから Copilot がコンテキストを取得する方法を確認し、`.github/copilot-instructions.md` にリポジトリ全体へ適用される規則を追加しました。このハーネスの後半で cloud agent が作成するプルリクエストをレビューし、規則が適用されているか確認します。

- リポジトリのカスタム指示とパス別の指示ファイルを通じて、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント規約が Copilot に伝わる方法を確認した。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する規約を追加した。

次は、[最初の Issue を Copilot cloud agent に割り当てます][next-lesson]。

## リソース

- [GitHub Copilot をカスタマイズする指示ファイル][instruction-files]
- [カスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Copilot のカスタム指示を改善する5つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
