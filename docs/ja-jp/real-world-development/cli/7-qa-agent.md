---
title: "レッスン 7 - 品質保証 (QA) エージェントの作成と使用"
description: "Issue の要件、quality-checks スキル、Playwright MCP を組み合わせる QA カスタムエージェントを作成します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

`quality-checks` スキルで自動チェックを実行し、Playwright Model Context Protocol (MCP) でブラウザーからフィルター機能を確認しました。次は、これらの機能を明確な品質保証 (QA) プロセスを持つカスタムエージェントにまとめます。

このレッスンでは、次の作業を行います。

- カスタムエージェントが指示、スキル、MCP ツールと連携する方法を確認する。
- 再利用可能な品質保証プロファイルを作成して調べる。
- QA エージェントを選択し、フィルター機能の Issue に照らして結果を確認する。

## シナリオ

Tailspin Toys では、プルリクエスト (PR) を作成する前に要件、コード品質、自動チェック、テストカバレッジ、ブラウザー上の動作を一貫してレビューしたいと考えています。カスタムエージェントを使って QA プロセスをまとめ、再利用可能なレポートを作成します。

## カスタムエージェントとは

カスタムエージェントは、Markdown プロファイルで定義する Copilot の専門的な構成です。プロファイルにはエージェントの目的、指示、利用可能なツールを記載します。このワークショップでは、`.github/agents/qa.agent.md` に QA の役割を定義し、Copilot CLI で選択します。

これまでに使ったカスタマイズには、それぞれ異なる役割があります。リポジトリの指示はチーム規約を説明し、`quality-checks` スキルは繰り返し実行できるチェックをまとめ、Playwright MCP はブラウザーツールを提供します。QA プロファイルは、これらを使って要件を評価し、結果を報告するよう Copilot に指示します。既存の機能に代わるものではなく、別の会話も必要ありません。

## QA プロファイルを作成する

機能の PR を開く前に、再利用可能な QA プロファイルを Copilot に作成してもらいます。プロファイルに QA のチェック内容と守るべき境界を明確に定義します。

1. Codespace に戻り、フィルター機能の会話が Interactive モードであることを確認します。
2. 既定のエージェントに、次のカスタムエージェントを作成するよう依頼します。

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## プロファイルを確認する

新しいエージェントを使う前に、Copilot が QA の流れと境界を正しく取り込んだかレビューします。検証だけを依頼したのに、機能を変更してしまう事態を防げます。

1. `/diff` を入力し、`.github/agents/qa.agent.md` を開きます。
2. フロントマターを確認します。`description` は必須です。`name` は任意ですが、含めるとエージェントを識別しやすくなります。
3. プロファイルの指示を読み、QA が要件を確認し、リポジトリの指示に従い、`quality-checks` スキルを実行し、Playwright MCP を使うことを確認します。
4. QA が証拠を添えて結果を報告し、実装コードを変更する前に確認を求め、コミットやプルリクエスト作成を行わないことを確認します。
5. これらの役割や制約が不足していたら、続行する前に既定のエージェントに修正を依頼します。

## Issue に対して QA を実行する

Copilot CLI は会話の開始時にプロジェクトのエージェントを読み込みます。プロファイルを作成した後、同じフィルター機能の会話を再開し、QA を選択すると、すでにある Issue と計画の内容を利用できます。

1. 次のプロンプトでエージェントを有効にします。

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> 作成直後は、利用可能なエージェントの一覧に表示されない場合があります。次のコマンドで有効にできます。

2. 次のプロンプトで QA エージェントに機能をレビューするよう依頼します。

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. QA エージェントの作業が始まります。
4. 作業が完了したら、レポートを読みます。

## まとめと次のステップ

再利用可能な専門的な役割をワークフローに加え、結果を確認しました。このレッスンでは、次の作業を行いました。

- カスタムエージェントが指示、スキル、MCP ツールと連携する方法を確認した。
- 再利用可能な品質保証プロファイルを作成して調べた。
- QA エージェントを選択し、フィルター機能の Issue に照らして結果を確認した。

実装、スキルの更新、QA プロファイル、テスト、検証レポートが揃いました。次は、[まとめて機能の PR を作成し、Agent Merge を使います][next-lesson]。

## リソース

- [Copilot CLI 用のカスタムエージェントを作成する][create-agents]
- [カスタムエージェントの構成][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
