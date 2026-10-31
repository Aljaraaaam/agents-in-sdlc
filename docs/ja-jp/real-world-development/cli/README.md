---
slug: ja-jp/real-world-development/cli
title: "GitHub Copilot CLI"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

**[GitHub Copilot CLI](https://docs.github.com/copilot/concepts/agents/about-copilot-cli)** を使うと、ターミナルで GitHub Copilot をエージェント型コーディングアシスタントとして利用できます。コードベースを調査し、コードを生成し、コマンドを実行し、外部ツールに接続できます。コマンドラインから作業できるため、グラフィカルエディターに切り替えずに開発の流れを保てます。

このワークショップでは、Tailspin Toys を使った一連のワークフローを実践します。

1. GitHub Codespaces でプロジェクトを準備し、Copilot CLI をインストールして、プロジェクトを確認する。
2. 星評価に対象を絞った変更を加え、ブラウザーでレビューして、最初のプルリクエスト (PR) を手動でマージする。
3. フィルター Issue から開始し、Plan モードでアプローチを定義し、Autopilot モードで実装して、Interactive モードでレビューする。
4. リポジトリの指示を更新し、フィルター機能の作業に適用する。
5. 既存の `quality-checks` スキルをカスタマイズし、プロジェクトのチェックを実行する。
6. Playwright Model Context Protocol (MCP) サーバーを追加し、ブラウザーからフィルター機能を確認する。
7. 品質保証 (QA) カスタムエージェントを作成し、要件、カバレッジ、検証の証拠をレビューする。
8. フィルター機能全体をレビューし、Agent Merge を使って PR を作成する。
9. コンテキスト、モデル、共有、任意のクラウドへの委任に使う便利なスラッシュコマンドを確認する。

ワークショップの焦点を絞るため、作成する PR は2つです。1つ目は星評価、2つ目はフィルター機能と指示・スキル・QA プロファイル・テストの更新です。フィルター機能と品質ワークフローでは1つの会話とブランチを共有し、各ツールの使い方を学びながら作業を積み重ねます。

## レッスン

| レッスン | トピック | 説明 |
| ------ | ----- | ----------- |
| [0. 前提条件][ex0] | セットアップ | リポジトリと Codespace を作成する |
| [1. Copilot CLI のインストール][ex1] | インストール | Copilot CLI をインストールして認証し、基本を確認する |
| [2. 星評価の追加で小さな成果を得る][ex2] | 最初の変更 | 既存の評価と未評価の場合の表示を追加し、最初の PR をマージする |
| [3. エージェントモード: Plan と Autopilot][ex3] | エージェントモード | Issue から機能を計画し、Autopilot で構築して、Interactive モードでレビューする |
| [4. カスタム指示で Copilot を導く][ex4] | コンテキスト | 指示を確認して更新し、フィルター機能に適用する |
| [5. quality-checks スキルのカスタマイズと使用][ex5] | 繰り返し実行できるチェック | 既存のスキルを確認し、報告形式を変更して実行する |
| [6. Playwright MCP で機能を検証する][ex6] | ブラウザーでの確認 | CLI で MCP を構成し、フィルターの動作を確認する |
| [7. QA エージェントの作成と使用][ex7] | 要件とカバレッジ | 専門家プロファイルを作成して選択し、最終検証の証拠を集める |
| [8. 機能の PR を作成してマージする][ex8] | レビューとマージ | 変更全体をレビューし、PR を作成して Agent Merge を使う |
| [9. GitHub Copilot CLI のスラッシュコマンド][ex9] | CLI 機能 | コンテキスト、モデル、共有、cloud agent への任意の委任を確認する |
| [10. 振り返りと次のステップ][ex10] | まとめ | ワークフロー、再利用可能なカスタマイズ、追加リソースを確認する |
| [任意: Foundry を組み込む][foundry] | ホスト型エージェント | モデルを準備し、カタログに基づくエージェントをデプロイして Web サイトに接続する |

## 前提条件

このワークショップに参加する前に、次のものを用意してください。

- [ ] 有効な **Copilot Student、Pro、Pro+、Business、Enterprise** のいずれかのプランが設定された GitHub アカウント
- [ ] リポジトリと Codespace を作成する権限
- [ ] ターミナルまたはコマンドラインの基本的な知識

> [!TIP]
> 有料プランを利用していない場合、認証済みの学生は [GitHub Education][callout-student-plan-education] を通じて GitHub Copilot を無料で利用できます。**Copilot Student** プランには、このワークショップで使用するエージェント、MCP、コードレビュー、Copilot CLI の機能が含まれています。

[callout-student-plan-education]: https://github.com/education/students

> [!NOTE]
> Copilot Business または Copilot Enterprise を利用している場合は、管理者が Copilot CLI を有効にしていることを確認してください。

## 開始する

**[前提条件から始める →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-install-copilot-cli/
[ex2]: 2-add-star-rating/
[ex3]: 3-agent-modes/
[ex4]: 4-custom-instructions/
[ex5]: 5-agent-skills/
[ex6]: 6-mcp-playwright/
[ex7]: 7-qa-agent/
[ex8]: 8-create-pull-request/
[ex9]: 9-cli-power-tools/
[ex10]: 10-review/
[foundry]: 8-foundry-agent/
