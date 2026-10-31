---
title: "演習 2 - Copilot Agent Mode で新機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

アプリケーションの小さな更新でも、通常は複数のファイル変更やテスト実行が必要です。開発者は必要なファイルを探し、変更し、テストを実行し、問題を修正して、見落としたファイルを更新します。作業は何度も続くことがあります。

Copilot Agent Mode は、統合開発環境 (IDE) 内でより自律的に作業するための機能です。既存のプロジェクト構成を調査し、必要な変更を行い、テストなどのタスクを実行して、見つかった問題を自動的に修正します。ここでは、Agent Mode を使ってサイトに機能を追加します。

> [!NOTE]
> 名前は似ていますが、エージェントモードと cloud agent は異なる用途向けです。エージェントモードは IDE 内で動作し、すばやいフィードバックと対話を可能にします。Cloud agent は Issue とプルリクエストを通じてチームメンバーのように非同期で作業します。

この演習では、次のことを学びます。

- Copilot Agent Mode がプロジェクトを調査し、関連ファイルを特定して、一貫した変更を加える方法。
- GitHub Copilot Agent Mode がユーザーインターフェイス (UI) とデータ層にまたがる新機能を実装する方法。
- Copilot Agent Mode が生成した変更とテストを、コードベースにマージする前にレビューする方法。

## シナリオ

ゲーム一覧が増えてきたため、カテゴリーとパブリッシャーで絞り込めるようにします。前の演習ではパブリッシャーヘルパーを追加しました。残りのデータ層、UI、テストの作業を Copilot Agent Mode で完了します。

## Tailspin Toys の Web サイトを実行する

変更を加える前に、Tailspin Toys の現在の機能を確認します。

この Web サイトは、開発者をテーマにしたボードゲームのクラウドファンディングプラットフォームです。ゲーム一覧と詳細を表示します。サイトは Astro アプリ1つで構成され、ビルド時に静的 HTML を生成します。ページは Drizzle ORM を通じてフロントマターからローカル SQLite データベースを直接参照します。独立したバックエンドのアプリケーションプログラミングインターフェイス (API) やクライアント側ユーザーインターフェイス (UI) フレームワークはありません。再利用可能なデータアクセスヘルパーは `src/lib/` にあり、インタラクティブな動作には標準 DOM API を使う小さな Astro `<script>` を使用します。

### Web サイトを起動する

`npm run dev` タスクを使うと、Astro の開発サーバーを起動できます。GitHub Codespace で次の手順を実行します。

1. 前の演習を終えた Codespace に戻ります。ブランチは `main` になっているはずです。
2. Codespace 内で <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. フィルター機能用の新しいブランチを作成して切り替えます。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 次のコマンドを実行して Web サイトを起動します。

   ```bash
   npm run dev
   ```

   Astro の開発サーバーの準備ができると、次のような URL を示すメッセージが表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> `http://localhost:4321` を開くかどうか尋ねるダイアログが表示された場合は、**x** を選択して閉じます。

5. <kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら、ターミナルに表示された `http://localhost:4321` を選択してサイトを開きます。

> [!NOTE]
> Codespace のターミナルで localhost の URL を選択すると、`https://<your-codespace-name>-4321.app.github.dev/` に転送されます。これは Web サーバーをホストする Codespace へのプライベートトンネルです。

### Web サイトを確認する

サイトを起動したら、機能を確認します。

- **ホームページ**: タイトル、画像、説明を含むボードゲームの一覧を表示します。
- **ゲーム詳細ページ**: ゲームを選択すると、タイトル、説明、パブリッシャー、カテゴリーなどの情報を表示します。

## Copilot でバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に Codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は各手順で説明します。

Web サイトの初期実装は動作しますが、さらに機能を追加します。まず、テンプレートからプロジェクトを作成したときに用意されたバックログを Copilot に表示してもらいます。

1. Chat ビューのエージェントドロップダウンから **Agent** を選択します。**Agent** はファイルを横断して変更を計画・実装し、ターミナルコマンドを実行してツールを呼び出します。

   ![Chat ビューのエージェント選択を示す画面](../../../_images/shared-chat-mode-selector.png)

2. 利用可能なモデル一覧から **Claude Sonnet 4.5** を選択します。

> [!CAUTION]
> このワークショップで特定のモデルを推奨する意図はありません。作成時に Claude Sonnet 4.5 を使用したため、手順に記載しています。提案の一貫性を高めることが目的ですが、大規模言語モデル (LLM) は確率的に動作するため、手順とは異なる提案が返る場合があります。これは正常な動作です。

> [!NOTE]
> LLM の確率的な性質により、Copilot が異なる Model Context Protocol (MCP) コマンドを使う場合がありますが、タスクは実行できるはずです。

3. 次のプロンプトを Copilot に送信して、バックログを表示します。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. **Continue** を選択して、すべての Issue を一覧表示するコマンドを実行します。
5. 生成された Issue の一覧を確認します。

Copilot がユーザーに最も役立つと考える項目に優先順位を付けていることを確認します。

## 指示ファイルを確認する

コード生成を開始する前に、作業のコンテキストを Copilot に与える指示ファイルを確認します。[ユーザーインターフェイス (UI) の指示ファイル](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)には、Web サイトへの機能追加に関する指針があります。

1. Codespace で `.github/instructions/ui.instructions.md` を開きます。
2. 機能追加のアーキテクチャ概要、コンポーネントの設計、テスト容易性、アクセシビリティの原則、次のファイル種別向けの指示へのリンクを確認します。
   - Astro
   - Tailwind CSS
   - Drizzle

> [!TIP]
> 指示ファイルから別の指示ファイルやプロジェクト内のファイルを参照できます。パスは指示ファイルからの相対パスです。指示を再利用し、複雑な内容を管理しやすい単位に分け、Copilot に例やテンプレートを示せます。

## フィルター機能を実装する

フィルター機能を完成させるには、少なくとも次の3種類の変更が必要です。

- データ層 (`src/lib/`) の残りのフィルターロジックを追加または調整する。
- フィルター動作のテストを追加または更新する。
- ゲーム一覧ページにフィルター UI を追加する。

コードをマージする前にテストを実行して成功させます。Copilot Agent Mode にこれらの作業を依頼します。

1. 現在の Copilot 会話を続けるか、**New Chat** を選択して新しい会話を始めます。
2. **Add Context**、**Instructions**、**ui** の順に選択して指示ファイルを指定します。

   ![UI 指示ファイルを選択する画面](../../../_images/ex3-select-instructions-file.png)

3. Chat ビューのエージェントドロップダウンで **Agent** が選択されていることを確認します。
4. モデルが **Claude Sonnet 4.5** のままであることを確認します。
5. バックログの関連 Issue に基づき、次のプロンプトで機能の実装を依頼します。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot がプロジェクトを調査し、関連するファイルを見つける様子を確認します。データ層のヘルパー、UI、テストを特定した後、ファイルを変更してテストを実行します。

   ![Copilot がプロジェクトファイルを調査する画面](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot はプロジェクトを調査し、ファイルを変更してテストを実行します。タスクやコードベースによって数分かかることがあります。作業中、エディターに **Keep** と **Undo** が表示されることがあります。完了するとすべての変更に **Keep** または **Undo** が表示されます。作業中に選択する必要はありません。

7. Copilot から求められたら、**Continue** を選択してテストを実行します。

   ![Copilot Chat ペインでテスト実行の確認を求めるダイアログ](../../../_images/ex3-agent-mode-run-tests.png)

8. 作業中に一時停止したりテストが失敗したりする場合があります。Copilot はタスクが完了し、エラーが見つからなくなるまでコード生成とテストを繰り返します。

   ![Copilot Agent Mode の完了した Chat セッション](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 生成されたコードに問題がないか確認します。

> [!CAUTION]
> Copilot やほかの人工知能 (AI) ツールが生成したコードは、必ずレビューしてください。

10. Web サイトを開いているブラウザーに戻り、新機能を確認します。
11. 動作とコードを確認したら、Copilot Chat ウィンドウで **Keep** を選択します。

## まとめと次のステップ

GitHub Copilot Agent Mode を使って Tailspin Toys に機能を追加する方法を確認しました。この演習では、次のことを学びました。

- Copilot Agent Mode がプロジェクトを調査し、関連ファイルを特定して、一貫した変更を加える方法を確認した。
- GitHub Copilot Agent Mode が UI とデータ層にまたがる新機能を実装する方法を確認した。
- Copilot Agent Mode が生成した変更とテストを、コードベースにマージする前にレビューした。

次は、[Playwright MCP サーバーで機能をテストし][next-lesson]、プルリクエストを作成します。

### 追加の探索演習 — ページネーションを実装する

ゲーム一覧が増えるとページネーションが必要になります。この演習で学んだことを使って、Copilot にサイトのページネーションを依頼します。次の点を検討してください。

- 既存の指示ファイルを使うなど、ベストプラクティスに従う。
- ページサイズをユーザーが選べるようにするか、固定にするかを決める。
- 希望する実装に必要な情報をプロンプトに含める。
- Copilot に変更を依頼し、コンテキストを追加して反復する。Copilot との通常の作業方法です。

## リソース

- [Copilot の Ask、Edit、Agent モード: 機能と使い分け][choose-mode]
- [VS Code のエージェントモード][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
