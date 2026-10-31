---
title: "演習 1 - カスタム指示 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前の演習: 前提条件][previous-lesson] · [次の演習: エージェントモード →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。タスクの進め方に決まりがある場合や、Copilot に知っておいてほしい情報がある場合は、参照できる状態にします。[指示ファイル][instruction-files]を使うと、コード生成の指針を伝えられます。

この演習では、次の作業を行います。

- リポジトリのカスタム指示とパス別の指示ファイルを通じて、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント規約が Copilot に伝わる方法を確認する。
- 現在の指示を使って、フィルター機能の最初のデータ部分 (パブリッシャーヘルパー) を生成する。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する規約を追加する。
- 同じプロンプトを再実行し、生成コードに新しい規約が反映されることを確認する。
- 指示の変更とフィルター機能の一部を `main` にコミットし、次の演習で新しい指針を使えるようにする。

> [!CAUTION]
> 生成コードが設定した規約の一部に従わない場合があります。Copilot は確率的に動作します。この演習では、指示の更新後に出力の傾向が変わることを確認します。文字単位で同じ出力を得ることが目的ではありません。

## 指示ファイル

### シナリオ

Tailspin Toys には開発慣行に関するガイドラインがあります。

- データ層には必ず単体テストを用意する。
- UI はダークモードにし、現代的な印象にする。
- TSDoc のドキュメントコメントをコードに追加する。
- 各ファイルの先頭に、ファイルの役割を説明するコメントブロックを追加する。

指示ファイルを使い、Copilot がこれらの慣行に沿って作業できるようにします。

### カスタム指示

カスタム指示は、コーディングスタイルや要件に関するコンテキストと希望を Copilot に伝えます。規約、ライブラリ、コードコメントの形式などを指定でき、提案やコード生成を導きます。リポジトリ全体または特定のファイル種別に指示を設定できます。

- `.github/copilot-instructions.md` は、リポジトリへのすべてのリクエストで Copilot に送られる単一の指示ファイルです。技術スタック、開発中の内容、ベストプラクティスなど、プロジェクト全体の情報を記載します。
- `.github/instructions/*.instructions.md` は、特定のタスクやファイル種別に指針を与えるファイルです。TypeScript や Astro などの言語、UI コンポーネントの作成、単体テストの作成に利用できます。

> [!NOTE]
> IDE で指示ファイルがコード生成に使われるのは Copilot Chat の場合のみです。コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、リポジトリレベルの指示と `applyTo` フロントマター付きの `*.instructions.md` ファイルを使用します。
>
> Copilot は AGENTS.md や CLAUDE.md など、ほかの規格の指示ファイルにも対応しています。[対応する指示ファイルの詳細][custom-instructions-support]を確認してください。

指示ファイルを管理する際は、次の点を参考にしてください。

- `copilot-instructions.md` には、プロジェクトの説明、構成、全体に適用するコーディング規約など、プロジェクトレベルの情報を記載します。
- `*.instructions.md` は、単体テスト、Astro コンポーネント、データ層などのファイル種別や、特定のタスク向けに使用します。
- 自然な言葉で、明確な指示とコード例を記載します。

指示ファイルや AI の使い方に唯一の正解はありません。プロジェクトに合わせて試しながら、適切な方法を見つけてください。

> [!TIP]
> GitHub Copilot を使うプロジェクトには、十分な指示ファイルを用意します。このプロジェクトには、[UI 更新][ui-instructions]や [Astro][astro-instructions]など、さまざまなタスク向けのファイルがあります。
>
> Copilot に指示ファイルを作成してもらうこともできます。VS Code では **Configure Chat → Generate Agent Instructions**、Copilot CLI では `/init` を使います。環境に応じたレッスンで詳しく説明します。
>
> テンプレートや出発点には、指示ファイル、カスタムエージェントなどを集めた [awesome-copilot][awesome-copilot] を利用できます。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム指示ファイルを確認する

スターターには、共通の `copilot-instructions.md` と各タスク向けの `*.instructions.md` が含まれています。エディターまたは GitHub の Web UI で開きます。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの概要と、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** の下にある **GitHub Actions Workflows** の指針も確認します。これらは Copilot とのすべてのやりとりに適用されます。
3. `.github/instructions` フォルダーを開き、Astro、Drizzle データ層、テストなどの指示ファイルを確認します。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドが、リポジトリルートを基準とした glob を定義しています。この例では `**/*.test.ts` に一致する TypeScript のテストファイルなどが対象です。
5. このプロジェクトの単体テスト作成に関する指示を確認します。
6. `.github/instructions/drizzle.instructions.md` を開き、末尾にある他の指示ファイル (`unit-tests.instructions.md` など) やプロジェクトファイルへのリンクを確認します。大きな指示を再利用できる小さなファイルに分け、コード生成の参考例を Copilot に示せます。ここにあるパスはリポジトリルートではなく、指示ファイルからの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング規約が記載されていますが、コード内のドキュメント作成はまだ必須ではありません。次の手順で TSDoc コメントとファイルコメントのヘッダーに関する規則を追加します。

## 変更用のブランチを作成する

変更用のブランチを作成し、作業を分けます。

1. 前の演習で使った Codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して新しいターミナルを開きます。
3. 新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## 指示を更新する前に Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 以降の演習を始める前に Codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は各手順で説明します。

カスタム指示の影響を確認するため、まず現在の指示ファイルのままでプロンプトを送信します。後で指示を更新して、同じプロンプトを再度送信し、違いを確認します。

1. 前の演習で開いたエディタータブを閉じ、Copilot が必要なコンテキストだけを取得するようにします。
2. `src/lib/publishers.ts` を開き、ヘルパーの配置場所を Copilot に示します。
3. Chat ビューのエージェントドロップダウンから **Agent** を選択し、ファイルの変更を適用できるようにします。

   ![Chat ビューのエージェント選択を示す画面](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot はプロジェクトを調査してコードを更新します。ヘルパーファイルとテストなど、複数のファイルが変更されることがあります。
6. 提案されたヘルパーは、最初の引数として `db` クライアントを受け取る型付き関数で、型付きのパブリッシャー配列を返します。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` のデータ層規約に基づいています。
7. 提案されたコードに TSDoc コメントとファイルレベルのコメントヘッダーがないことを確認します。

> [!CAUTION]
> Copilot は確率的に動作するため、指示がなくてもドキュメントコメントを追加する場合があります。その場合も問題ありません。指示を更新した後に一貫性が高まることがこの演習の目的です。

## リポジトリの規約を追加する

`.github/copilot-instructions.md` は、プロジェクト全体の情報を Copilot に提供します。コーディング規約を追加してコード提案を改善します。

1. `.github/copilot-instructions.md` を再度開きます。
2. 27 行目付近にある **Code formatting requirements** セクションを見つけます。このセクションには既存の規約が記載されていますが、コード内ドキュメントの規則がまだありません。
3. 既存の規約の直後に、次の Markdown 行を追加します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 指示ファイルはリポジトリレベル (`.github/copilot-instructions.md`) に置いて全体に適用することも、特定の言語、ファイル種別、タスク向けに `*.instructions.md` として作成することもできます。今回追加するドキュメントコメント規則のような、プロジェクト全体の規約にはリポジトリレベルのファイルが適しています。

## プロンプトを再実行して変化を確認する

1. Copilot Chat に戻り、**New Chat** を選択して会話を新しくします。
2. `src/lib/publishers.ts` を選択し、Copilot が適切なファイルに注目するようにします。
3. 前と**同じプロンプト**を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 提案されたファイルの先頭に、次のようなコメントブロックが追加されていることを確認します。

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 関数に次のような TSDoc コメントが追加されていることを確認します。

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

新しい規約を適用するよう Copilot を導き、次の演習でも使う実際のコードに適用できました。

## 指示をコミットしてブランチをプッシュする

指示ファイルも、ほかのリポジトリアセットと同じようにソース管理します。変更をコミットしてブランチをプッシュします。

1. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して新しいターミナルを開きます。
2. 次のコマンドを実行し、指示とヘルパーの変更を確認します。

   ```bash
   git status
   ```

3. 指示の更新とヘルパーの基礎部分をステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # git status に追加の変更 (テストなど) が表示された場合は、それらのファイルも追加します。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. ブランチをリポジトリにプッシュします。

   ```bash
   git push -u origin custom-instructions
   ```

## プルリクエストを作成してマージする

ブランチをプッシュしたら、バックログのドキュメント規約に関する Issue に関連付けてプルリクエストを作成します。GitHub ツールを使用できる Copilot に、Issue の検索、Issue を完了する PR の作成、マージを依頼します。

> [!NOTE]
> この演習では、Copilot が初めて GitHub 上の操作を行います。プロジェクトのテンプレートでは GitHub ツールがワークスペースに接続済みのため、追加設定は不要です。Copilot が初めてツールを使うとき、VS Code から GitHub へのサインインを求められる場合があります。画面の指示に従ってください。接続方法である Model Context Protocol は、後の演習で説明します。

1. Codespace 内で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、エージェント選択で **Agent** が選択されていることを確認します。
3. 指示ファイルの更新と、今後のフィルター機能に向けたパブリッシャーヘルパーを説明する PR を現在のブランチから作成するよう、Copilot に依頼します。

   ```plaintext
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot が Issue を見つけて PR を作成します。
5. Copilot に GitHub 操作を許可するよう求められた場合は、コマンドを確認して、適切な場合に **Allow** を選択します。
6. PR が作成されたら、次のプロンプトでマージしてローカルブランチを `main` に戻すよう依頼します。

   ```plaintext
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Copilot に GitHub 操作とシェルコマンドの実行を許可するよう求められた場合は、コマンドを確認して、適切な場合に **Allow** を選択します。

GitHub Copilot の支援でプルリクエストを作成し、マージできました。

## まとめと次のステップ

このプロジェクトで Copilot が指示ファイルからコンテキストを取得する方法を確認し、VS Code の Copilot Chat で次の作業を行いました。

- リポジトリのカスタム指示とパス別の指示ファイルを確認した。
- 現在の指示を使ってコード生成プロンプトを送り、Copilot の出力を確認した。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する規約を追加した。
- 同じプロンプトを再実行し、提案されたコードに新しい規約が反映されることを確認した。
- 指示の更新とヘルパーの基礎部分を `main` にコミットし、次の演習で利用できるようにした。

次は、[エージェントモード][next-lesson]で指示を使い、Copilot にコードベース全体の新機能を追加させます。

## リソース

- [GitHub Copilot のカスタマイズに使う指示ファイル][instruction-files]
- [カスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Copilot のカスタム指示を改善する5つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用カスタム指示][personal-instructions]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
[awesome-copilot]: https://github.com/github/awesome-copilot
