---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前の演習: カスタム指示][previous-lesson] |
|:--|

未解決のセキュリティ問題、更新が必要なレガシーコード、実装する時間がなくバックログに残った機能リクエストなど、技術的負債に悩む Organization は少なくありません。GitHub Copilot の cloud agent は、コードの更新や機能の追加などを自律的に実行できます。作業が完了すると、人間の開発者がレビューできるドラフトプルリクエスト (PR) を作成します。これにより、手間のかかる作業を委任し、開発を加速して、開発者がより大きな課題に集中できます。

この演習では、Copilot cloud agent を使って次のことを確認します。

- コード生成用の環境をカスタマイズした。
- 操作が安全に行われるようにした。
- 範囲を明確にした Issue が重要である理由を確認した。
- Issue を Copilot に割り当てた。

## シナリオ

Tailspin Toys には解消したい技術的負債があります。最初の Web サイトを作成した委託チームがドキュメントをほとんど残していません。まず、アプリケーション内のエクスポートされた関数に TypeScript Documentation (TSDoc) のドキュメントコメントを追加します。

さらに、デザインチームはゲームを見つけやすくしたいと考えています。各ゲームの詳細ページに、同じカテゴリーに属するほかのゲームを表示し、閲覧を続けられるようにします。洗練された実装はまだ必要ありません。UX の受け入れテストに使える状態を目指します。この作業は現在のブロッカーですが、優先度の高い Issue がほかにあります。

このようなタスクは優先順位が下がりやすいため、Copilot cloud agent に割り当てるのに適しています。cloud agent が非同期に作業している間、開発者は別の作業に取り組み、後で結果をレビューできます。

## GitHub Copilot cloud agent の概要

[GitHub Copilot cloud agent][cloud-agent-overview] は、人間の開発者と同じようにバックグラウンドで作業できます。たとえば、[GitHub Issue を Copilot に割り当て][assign-issue]る方法があります。割り当て後、Copilot は進行状況を追跡するドラフトプルリクエストを作成し、環境を準備してタスクに取り組みます。実行中でも完了後でもセッションを確認できます。提案された解決策のレビュー準備ができると、プルリクエストで通知されます。

## 範囲を明確にした指示の重要性

GitHub Copilot に魔法はありません。考える作業を省略できるわけではありません。一見簡単な作業でも、詳しく見ると複雑さがあります。[Copilot cloud agent に依頼するタスクの範囲][cloud-agent-best-practices]を慎重に決めてください。人工知能 (AI) を活用するペアプログラマーとして扱い、段階を踏んで学び、試し、調整します。生成 AI を加えても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム指示

前の演習では、カスタム指示と Copilot の作業を導く方法を確認しました。カスタム指示の演習をすでに完了した場合は、`.github/copilot-instructions.md` が機能することを確認済みです。まだの場合は、この機会に内容を確認してください。cloud agent に作業を依頼する前に、このリポジトリに含まれる指示ファイルを読み取り専用で確認し、後でその影響を見つけられるようにします。

GitHub の Web ユーザーインターフェイス (UI) または起動済みの Codespace で、次のファイルを開きます。

- `.github/copilot-instructions.md` — **Code standards** セクション、特に TypeScript 規約と TypeScript Documentation (TSDoc) ドキュメントコメントの要件を確認します。
- `.github/instructions/unit-tests.instructions.md` — `applyTo` フロントマターを確認します。この指示は `**/*.test.ts` ファイルに適用されます。

次のセクションで `Code lacks documentation` Issue を cloud agent に割り当てます。生成されたプルリクエストでは、TSDoc ドキュメントコメント、TypeScript 規約、コメントヘッダーに注目してください。これらの規則は指示ファイルから取得されます。後の演習で PR をレビューするときに、改めて確認します。

## Copilot cloud agent の開発環境を設定する

コードの作成には、通常、適切な環境と初期化スクリプトが必要です。ソフトウェアエンジニアのように作業する Copilot にタスクを割り当てる場合も同じです。

Cloud agent は作業環境に [GitHub Actions][github-actions] を使います。`.github/workflows/copilot-setup-steps.yml` に設定する[専用のセットアップワークフロー][setup-workflow]を作成すると、作業前に実行するツールと依存関係を準備できます。このラボの流れを円滑に進めるため、事前に設定されています。単一の Astro アプリに必要な Node.js、プロジェクトの依存関係、エンドツーエンド (E2E) テスト用のブラウザー、移行およびシード済みの SQLite データベースを Copilot が利用できます。

```yaml
name: "Copilot Setup Steps"

# リポジトリの Actions タブからセットアップ手順をテストできます
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 必要な手順に対して最小限の権限を設定します。Copilot は操作用の独自トークンを受け取ります。
    permissions:
      # セットアップ手順でリポジトリをクローンする場合 (依存関係のインストールなど) は、`contents: read` 権限が必要です。クローンしない場合、手順の完了後に Copilot が自動的にクローンします。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # フロントエンド / アプリのセットアップ - Node.js (アプリは Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # 単一の Astro アプリ向けに、ローカル SQLite データベースを移行してシードします
      - name: Set up the database
        run: npm run db:setup
```

一般的な GitHub ワークフローファイルと同じですが、次の点に注意します。

- `copilot-setup-steps` という単一のジョブがあります。Copilot がプルリクエストの作業を開始する前に、GitHub Actions でこのジョブが実行されます。
- `workflow_dispatch` トリガーを使うと、リポジトリの **Actions** タブからワークフローを手動実行できます。Copilot の実行を待たずに、正常に動くかテストできます。

## ドキュメントを追加する

多くのプロジェクトには、古くなった情報や不足しているドキュメントがあります。これは放置されやすい技術的負債で、生産性を下げ、コードベースの保守や新しい開発者の参加を難しくします。Copilot はドキュメントの作成を支援できるため、このタスクを cloud agent に割り当てます。後の演習で結果をレビューします。

1. 新しいブラウザータブで github.com の自分のリポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択して新しい Issue ダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次の内容を入力します。

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. **Create** を選択して Issue を作成します。
8. 右側にある **Assign to Copilot** を選択して割り当てダイアログを開きます。

    ![Copilot を Issue に割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。
10. **Pull Requests** タブを選択します。
11. `[WIP]: Code lacks documentation` のようなタイトルの新しいプルリクエスト (PR) を開きます。一覧に表示されない場合は、少し待ってからブラウザーを更新します。
12. 数分後、Copilot が作成した To Do リストが表示されます。

> [!NOTE]
> Copilot の To Do リストが PR に表示されるまで数分かかることがあります。Copilot は先ほどのワークフローを実行して環境を構築し、プロジェクトを分析して、問題への最適な取り組み方を判断しています。

13. リストを確認し、Copilot が実行するタスクを確認します。
14. プルリクエストのタイムラインを下にスクロールし、Copilot が Issue の作業を開始したことを示す更新を確認します。
15. **View session** ボタンを選択します。

    ![Copilot セッションの表示画面](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 更新された状態が表示されない場合は、ウィンドウを更新してください。

16. ライブセッションをスクロールして Copilot の作業を確認します。コードを調査して状態を把握し、計画を考え、コードを作成する様子を確認できます。

この作業には数分かかることがあります。Copilot cloud agent の主な目的は、非同期に作業して開発者がほかのタスクに集中できるようにすることです。この機能を活用するため、もう1つタスクを割り当ててから、アプリケーションの機能追加に取り組みます。

## ゲーム詳細ページに関連ゲームを追加する

Copilot cloud agent の利点の1つは、作業を分担できることです。デザインチーム向けに関連ゲームを表示する作業を依頼し、その間に別のタスクに取り組みます。

1. github.com の自分のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択して新しい Issue ダイアログを開きます。
4. 空のテンプレートを使うため、**Blank issue** を選択します。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次の内容を入力します。

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. **Create** を選択して Issue を作成します。
8. 右側にある **Assign to Copilot** を選択して割り当てダイアログを開きます。

    ![Copilot を Issue に割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

その後、最初の Issue のコメントに 👀 が表示され、Copilot が作業中であることがわかります。

![Copilot が Issue に取り組んでいることを示す目の絵文字](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は2つ目の依頼にも取り組み始めました。Copilot cloud agent はソフトウェアエンジニアと同じように非同期で動作するため、常に監視する必要はありません。完了後にレビューできます。次はカスタムエージェントの作成と使用を確認します。

## まとめと次のステップ

[GitHub Copilot cloud agent][copilot-agents]を確認しました。cloud agent に Issue を割り当て、非同期で実行できます。技術的負債への対応、新機能の作成、別のフレームワークへのコード移行などを依頼できます。

この演習では、次のことを行いました。

- コード生成用の環境をカスタマイズした。
- 操作が安全に行われるようにした。
- 範囲を明確にした Issue が重要である理由を確認した。
- Issue を Copilot に割り当てた。

cloud agent がバックグラウンドで作業する間、カスタムエージェントを作成して使います。[Copilot cloud agent は Model Context Protocol (MCP) サーバーも利用でき][cloud-agent-mcp]、前の演習で確認したカスタム指示にも対応しています。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップワークフローに関するベストプラクティス][cloud-agent-best-practices]

| [次の演習: カスタムエージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
