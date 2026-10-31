---
title: "演習 3 - Playwright MCP サーバーで機能をテストする"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前の演習: エージェントモード][previous-lesson] · [次の演習: カスタムエージェント →][next-lesson]

エージェントモードでフィルター機能を作成しました。プルリクエストを開く前に、ブラウザーで動作を確認します。自分でアプリを操作する代わりに、**Playwright Model Context Protocol (MCP) サーバー**を接続して Copilot にブラウザーを操作させ、機能をテストします。その後、ブランチを公開してプルリクエスト (PR) を作成します。

この演習では、次の作業を行います。

- Model Context Protocol (MCP) と MCP サーバーが Copilot にツールを追加する仕組みを学ぶ。
- Playwright MCP サーバーをワークスペースに追加する。
- Copilot にサーバーを使わせ、ブラウザーでフィルター機能を手動テストする。
- ブランチを公開し、フィルター機能の PR を作成する。

## Model Context Protocol (MCP) とは

エージェントモードは、エディターの外にあるツールを使えるとさらに便利になります。Copilot は Model Context Protocol (MCP) を使って、外部のツールやサービスと通信します。

![エージェントモードとコンテキスト、大規模言語モデル (LLM)、MCP サーバーや VS Code 拡張機能が提供するツールの関係を示す図](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) は、人工知能 (AI) エージェントが外部ツールやサービスと通信するための仕組みです。MCP を使うと、エージェントはリソースを通じて最新情報にアクセスし、ツールを通じて開発者に代わって操作できます。

ツールやリソースには、AI エージェントと外部ツール・サービスの間を仲介する MCP サーバーを通じてアクセスします。MCP サーバーは、既存のアプリケーションプログラミングインターフェイス (API) や npm パッケージなどのローカルツールとの通信を管理します。各サーバーは、AI エージェントが使えるツールとリソースのセットを提供します。

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: GitHub リポジトリを管理する API にアクセスします。リポジトリの作成・更新、Issue やプルリクエストの管理などを実行できます。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: Playwright によるブラウザー自動化機能を提供します。Web ページの移動、フォームへの入力、ボタンの選択などを実行できます。

ほかにもさまざまな MCP サーバーがあります。GitHub は、エコシステム内での発見や貢献を促進するために [MCP レジストリ](https://github.com/mcp)をホストしています。

> [!CAUTION]
> セキュリティの観点から、MCP サーバーはプロジェクト内のほかの依存関係と同様に扱ってください。利用前にソースコードを確認し、発行元を検証し、セキュリティへの影響を検討します。信頼できる MCP サーバーのみを使い、機密情報や操作へのアクセス許可には注意してください。

## MCP の構成を確認する

`.vscode/mcp.json` ファイルで、この VS Code ワークスペースで使う MCP サーバーを構成します。

1. Codespace で `.vscode/mcp.json` を開きます。
2. 次のような `github` サーバーが構成済みであることを確認します。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

`github` の設定はプロジェクトテンプレートに含まれているため、前の演習で Copilot が Issue のバックログを読み取れました。[リモート GitHub MCP サーバー][remote-github-mcp-server]を使用します。ローカルにインストールする必要はありません。VS Code は HTTP 経由で接続し、Copilot が初めてツールを使うときに GitHub で認証します。

## Playwright MCP サーバーを追加する

2つ目のサーバーを追加します。[Playwright MCP サーバー][playwright-mcp-server]は、Copilot が操作できるブラウザーを提供します。機能をテストするために使用します。

1. `.vscode/mcp.json` で `github` と並べて `playwright` エントリを追加し、ファイルを次の内容にします。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. ファイルを保存します。

リモートの `github` サーバーとは異なり、`playwright` はローカルサーバーです。VS Code が `npx @playwright/mcp@latest` を実行して起動します。`--headless` は画面を表示せずにブラウザーを実行する指定です。デスクトップ環境がない Codespace で必要です。

> [!NOTE]
> Tailspin Toys はエンドツーエンド (E2E) テストに Playwright を使うため、通常はブラウザーもインストール済みです。Copilot がブラウザーがないと報告した場合は、`npx playwright install chromium` を実行して再試行するよう依頼します。

## サーバーを起動して信頼する

VS Code は MCP サーバーを**必要に応じて起動**します。Copilot が初めてサーバーのツールを必要とするとき、VS Code が起動し、サーバーを信頼するか確認します。

1. `.vscode/mcp.json` の `playwright` エントリの上に表示される **Start** 操作を選択し、サーバーを起動して構成を確認します。
2. VS Code からサーバーを信頼するか尋ねられた場合は、構成を確認し、起動を許可します。

手動で起動する必要はありません。省略した場合は、次の手順で Copilot が初めてツールを使うときに VS Code が起動し、信頼の確認を表示します。

## フィルター機能をテストする

[Playwright MCP サーバー][playwright-mcp-server]を使うと、Copilot が実際のブラウザーを操作できます。開発者がアプリを操作する代わりに、エージェントがページを開き、移動し、フィルターを適用して結果を確認し、観察内容を要約します。会話を離れずに動作を確認できます。

Playwright MCP サーバーはスクリーンショットではなく、ページの[アクセシビリティツリー][playwright-mcp-server]を使います。ボタン、リンク、リスト項目など、ラベル付きの構造化要素を基にエージェントが判断します。支援技術と同じような情報を確認するため、機能テストと簡易的なアクセシビリティ確認を兼ねられます。

サーバーに接続し、アプリを起動した状態で、Copilot にフィルター機能をテストするよう依頼します。

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot は Playwright MCP サーバーでブラウザーを起動し、各手順を実行して結果を報告します。Issue の受け入れ条件に照らして報告を読みます。問題があれば、PR を開く前に追加の質問をするか、コードを修正するよう依頼します。

> [!NOTE]
> テスト時はアプリが `http://localhost:4321` で起動している必要があります。開発サーバーを停止した場合は、プロンプトを送る前に再起動します。Copilot が Playwright MCP サーバーを初めて使うとき、ブラウザーをダウンロードする場合があります。ブラウザーがないと報告されたら、`npx playwright install chromium` を実行して再試行するよう依頼します。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## ブランチを公開してプルリクエストを作成する

機能が動作することを確認したので、チームでレビューするためにプルリクエスト (PR) を作成します。まず `filtering-vscode` ブランチを公開します。

1. Codespace の **Source Control** パネルに移動し、Copilot が加えた変更をレビューします。
2. **+** アイコンを選択して変更をステージします。
3. **Sparkle** ボタンでコミットメッセージを生成します。

   ![Copilot の変更が表示された Source Control パネル](../../../_images/ex3-source-control-changes.png)

4. **Publish** を選択してブランチをリポジトリにプッシュします。

## プルリクエストを作成する

github.com や GitHub コマンドラインインターフェイス (CLI) など、複数の方法で PR を作成できます。ここでは GitHub Copilot に Issue を検索させ、関連付けた PR を作成してもらいます。

1. Copilot Chat パネルに移動し、**New Chat** を選択して新しいセッションを開始します。
2. エージェントドロップダウンで **Agent** が選択され、Copilot が GitHub ツールを使えることを確認します。
3. 次のプロンプトで PR の作成を依頼します。

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. 必要に応じて **Continue** を選択し、情報の取得や操作を許可します。GitHub ツールを初めて使うときに GitHub の認証を求められた場合は、画面の指示に従います。
5. Copilot が Issue を検索し、該当するものを見つけて PR を作成することを確認します。
6. Copilot が作成したリンクを選択して PR をレビューします。ただし、**まだマージしないでください**。

## まとめと次のステップ

この演習では、次の作業を行いました。

- Model Context Protocol (MCP) と MCP サーバーが Copilot にツールを追加する仕組みを学んだ。
- Playwright MCP サーバーをワークスペースに追加した。
- ブラウザーでフィルター機能を手動テストしてからリリースした。
- ブランチを公開し、フィルター機能の PR を作成した。

MCP サーバーを使って機能を**テスト**しました。MCP は Copilot を拡張する方法の1つです。次は、アクセシビリティレビューなど、対象を絞ったタスクを効率化する[カスタムエージェントを作成します][next-lesson]。

## リソース

- [MCP とは何か、なぜ注目されているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP Registry][mcp-registry]
- [VS Code の MCP サーバー][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
