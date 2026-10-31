---
title: "レッスン 10 - 振り返りと次のステップ"
description: "Copilot CLI のワークフロー、2つのプルリクエスト、再利用可能なカスタマイズ、追加のリソースを振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

GitHub Copilot CLI を使い、Tailspin Toys の一連のワークフローを完了しました。

- Codespace を準備し、Copilot CLI をインストールし、プロジェクトとシード済みのフィルター Issue を確認した。
- 星評価を追加し、転送されたブラウザーで結果を確認して、最初のプルリクエスト (PR) を手動でマージした。
- フィルター Issue から開始し、Plan モードでアプローチを定義し、Autopilot モードで実装して、Interactive モードでレビューした。
- カスタム指示でエージェントを導き、既存の `quality-checks` スキルをカスタマイズして、単体テスト、lint、型チェックを実行した。
- Playwright Model Context Protocol (MCP) サーバーを追加し、実際のブラウザーでフィルター機能を確認した。
- 品質保証 (QA) カスタムエージェントを作成して選択し、要件、カバレッジ、スキルの結果、ブラウザーでの証拠を評価した。
- フィルター機能全体をレビューし、フィルター PR に Agent Merge を許可した。
- コンテキスト、モデル、共有、任意の cloud agent への委任に使うスラッシュコマンドを確認した。

## リリースした内容

ワークショップには、それぞれ更新済みの `main` から作成する2つの PR があります。

1. **星評価:** 既存の `starRating` と未評価状態をゲームカードに表示する。
2. **フィルターと品質ワークフロー:** フィルター機能を実装し、指示を更新して機能に適用し、`quality-checks` のレポートをカスタマイズし、QA プロファイルを作成して関連テストを追加する。

フィルター機能の計画から PR 作成まで、同じ会話とブランチを使用しました。ワークショップを簡略化するため、これらの作業を1つの PR にまとめました。

## さまざまな検証方法

自動テスト、自分で行うブラウザー確認、MCP を通じた Copilot のブラウザー操作という複数の方法でコードを確認しました。`quality-checks` スキルは単体テスト、lint、型チェックを実行し、新しい形式で報告しました。QA はこれらの結果に要件とテストカバレッジのレビューを加え、PR 作成前の確認を支援しました。

新しいテストは実際の不足を補うものである必要があります。QA の実行で追加テストが不要と判断されることもあります。マージを許可する前に、コードと検証の証拠をレビューし、変更後は関連する証拠を更新してください。

## ベストプラクティス

Copilot に与えるコンテキストとツールが、作業の内容に影響します。このワークショップでは、指示を更新し、スキルをカスタマイズし、QA プロファイルを作成し、MCP サーバーを構成しました。これらのカスタマイズを会話間で再利用し、チームのニーズに合わせて調整してください。指示は規約を定め、スキルは繰り返し実行できるタスクを記述し、カスタムエージェントは専門的な役割を定義し、MCP サーバーは外部ツールに接続します。エージェントの要約だけでなく、実際の変更とツールの結果を確認してください。

タスクに合った**モードとモデル**を選びます。構築前にアプローチを検討するには **Plan**、範囲を絞った変更に関与し続けるには **Interactive**、範囲が明確なタスクには **Autopilot** を使います。定型的な編集には高速なモデルを、複雑な作業には高性能なモデルを選びます。

インフラストラクチャと同じように、コンテキストも重要です。何を、なぜ、どのように構築するのかを明確に説明すると、結果が大きく変わります。

## さらに試す

主要なワークフローを完了しました。Copilot CLI には、さらに次の機能があります。

- `/review` でコードレビューエージェントに変更を分析させる。
- `/rubber-duck` で問題について話し合い、別の視点を得る。
- `/fleet` で独立したサブタスクを並行して実行する。
- `/worktree` で別のタスクを分離する。
- `/delegate` でタスクを Copilot cloud agent に送信する。

## 次のステップ

ツールの習熟には、使い続けることが一番です。本番コード、趣味のコード、何年も作ろうと思いながら手を付けていなかった小さなアプリなどで試してください。学んだことをチームで共有し、互いの経験から学びましょう。ドキュメントも引き続き確認してください。

ターミナルから Tailspin Toys を拡張するには、任意の [Foundry Backer Concierge シリーズ][foundry]に進みます。別の環境と比較するには、[VS Code ワークショップ][vscode]、[GitHub Copilot app ワークショップ][app]、または [Copilot cloud agent ワークショップ][cloud]を確認してください。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンドリファレンス][cli-reference]
- [Copilot CLI のカスタマイズ][customize-cli]
- [Copilot CLI でプルリクエストを管理する][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
