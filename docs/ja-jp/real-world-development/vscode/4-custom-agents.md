---
title: "演習 4 - カスタムエージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタムエージェントとは

GitHub Copilot の[カスタムエージェント][custom-agents-concept]を使うと、開発ワークフローの特定のタスクや分野に特化した人工知能 (AI) アシスタントを作成できます。リポジトリの `.github/agents` フォルダーに Markdown ファイルを作成し、指示、ベストプラクティス、コーディングパターン、専門知識を定義します。たとえば、[Web Content Accessibility Guidelines (WCAG)][wcag] 準拠を確認するアクセシビリティエージェント、安全なコーディング慣行に従うセキュリティエージェント、一貫したテストパターンを守るテストエージェントを作成できます。チームの知識を再利用可能なエージェントとしてまとめられます。

カスタムエージェントは、プロジェクト内の `.github/agents` フォルダーまたはグローバルな `~/.copilot/agents` にある Markdown ファイルで定義します。各ファイルには、少なくとも `name` と `description` を含む YAML フロントマターと、エージェントの動作、専門知識、指示を記載する Markdown プロンプトが必要です。

### カスタムエージェントとエージェントスキルの違い

カスタムエージェントと[エージェントスキル][agent-skills-concept]には共通点があります。どちらも主に Markdown ファイルで定義し、AI に作業方法を指示します。わかりやすく言うと、**カスタムエージェントは作業者、スキルはツール**です。

カスタムエージェントは独自のコンテキストウィンドウを持ち、スキルやほかのエージェントを使いながら作業を進めます。このラボでは、アクセシビリティのカスタムエージェントがガイドラインに沿ってサイトをレビューし、更新します。その作業の中で、プルリクエストワークフローのスキルや、テストを実行・管理するスキルなどを呼び出すこともできます。

> [!NOTE]
> カスタムエージェントの作成方法に唯一の正解はありません。AI を使うほかの作業と同じように、環境やシナリオに合わせて試し、改善してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## シナリオ

Tailspin Toys は、視覚的な能力や好みにかかわらず、すべてのユーザーがクラウドファンディングプラットフォームを利用できるようにしたいと考えています。最近のフィードバックによると、テキストと背景色のコントラストが十分でないため、現在のダークテーマが読みにくいユーザーがいます。このアクセシビリティ上の課題に対応するため、デザインチームはユーザーが切り替えられるハイコントラストモードを求めています。

アクセシビリティは重要なため、できるだけ早く実装します。カスタムエージェントを使って機能を生成します。

この演習では、次の作業を行います。

- 既存のアクセシビリティカスタムエージェントを確認する。
- Copilot Chat でアクセシビリティエージェントを使い、ハイコントラストモードを実装する。

## アクセシビリティのカスタムエージェントを確認する

アクセシビリティ向けのカスタムエージェントは作成済みです。フィルター機能のプルリクエスト (PR) とは作業を分けるため、`main` からアクセシビリティ作業用のブランチを作成し、エージェントのファイルを確認します。

1. Codespace に戻り、ターミナルで次のコマンドを実行します。

   ```bash
   git checkout main
   git pull
   git checkout -b accessibility-vscode
   ```

2. `.github/agents/accessibility.md` を開きます。
3. `name` と `description` を含む YAML フロントマターを確認します。

> [!CAUTION]
> カスタムエージェントには、`name` と `description` を含むフロントマターが必要です。

4. アクセシブルな Web サイトのコード生成に関する主な責任、アクセシビリティのベストプラクティス、HTML・CSS・JavaScript のコード例、よくある問題を説明する後続のセクションを確認します。

## Copilot Chat でカスタムエージェントを使う

VS Code の Copilot Chat ビュー下部にあるエージェントドロップダウンには、`.github/agents` に定義されたカスタムエージェントが表示されます。選択すると、そのエージェントの指示とツールに合わせた会話が始まります。

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に Codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は各手順で説明します。

1. Chat ビューのエージェントドロップダウンで、**Agent** を選択します。すでに選択されている場合はそのまま進みます。

   ![Chat ビューのエージェント選択を示す画面](../../../_images/shared-chat-mode-selector.png)

2. Chat ビュー下部のエージェントドロップダウンを選択します。既定では、現在のエージェントとして **default** が表示されています。
3. 利用可能なエージェントの一覧から **Accessibility agent** を選択します。
4. 次のプロンプトをアクセシビリティエージェントに送信します。

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

5. Copilot Chat がコードベースを調査し、変更を提案してプロジェクトファイルに適用します。各編集はファイルパスと差分とともにチャット内に表示されます。
6. エージェントが変更した作業ファイルを示す **Files changed** の表示を確認します。

> [!NOTE]
> この作業には数分かかることがあります。Copilot は Astro コンポーネント、CSS、関連テストなどの既存ファイルを実際に変更します。

次の演習では、この作業の進行を確認し、エージェントに追加の指示を送ります。

## まとめと次のステップ

[GitHub Copilot のカスタムエージェント][custom-agents]を確認しました。カスタムエージェントは、特定のタスクや分野に対応する AI アシスタントです。チームの知識と規約を再利用可能なエージェントにまとめ、Copilot の作業を導けます。

このレッスンでは、次の作業を行いました。

- 既存のアクセシビリティカスタムエージェントを確認した。
- Copilot Chat でアクセシビリティエージェントを使い、ハイコントラストモードを実装した。

次は、同じセッションにライトモードの切り替えを追加するよう依頼し、エージェントの作業を[監視して誘導します][next-lesson]。

## リソース

- [カスタムエージェントについて][custom-agents]
- [IDE でカスタムエージェントを作成する][creating-custom-agents-ide]
- [VS Code のカスタムエージェント][custom-agents-vscode]
- [カスタムエージェントの構成][custom-agents-config]
- [Awesome Copilot のカスタムエージェント][awesome-copilot-agents]

---

| [← 前の演習: Playwright MCP サーバーで機能をテストする][previous-lesson] | [次の演習: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
