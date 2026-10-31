---
title: "レッスン 8 - 機能の PR を作成してマージする"
description: "フィルター機能、指示、スキルの更新、QA プロファイル、テストをまとめてレビューし、PR を作成して Agent Merge を使用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

フィルター機能の実装、指示の更新、スキルの更新、品質保証 (QA) プロファイル、テストが1つのブランチに保存されています。まとめてレビューし、プルリクエストを作成します。星評価のプルリクエスト (PR) は自分でマージしました。今回は **Agent Merge** にプロセスを任せます。

> [!NOTE]
> 通常は、機能、指示、スキル、QA エージェントを複数の PR に分けます。ワークショップを簡略化するため、フィルター機能と品質ワークフロー全体を1つの会話とブランチで進め、この PR にまとめました。

このレッスンでは、次の作業を行います。

- Agent Merge とマージライフサイクルを自動化する方法を学ぶ。
- 機能全体と検証の証拠を確認する。
- フィルター機能の PR を作成する。
- レビュー後に Agent Merge を有効にし、PR のマージを確認する。

## シナリオ

フィルター機能の作業を通じて、Copilot を使って機能を計画、実装、検証しました。Tailspin Toys では、マージの承認を開発者の管理下に置きながら、残りの PR 作業を自動化したいと考えています。

## Agent Merge の概要

**Agent Merge** は、プルリクエストをマージ可能にするための残りの作業を自動化します。有効にすると、Copilot は PR を妨げている問題に取り組みます。失敗した継続的インテグレーション (CI) チェックの修正、レビューコメントへの対応、必要な場合のリベースなどを行い、リポジトリで許可されていれば GitHub の自動マージを有効にします。

これまでは **Merge pull request** を自分で選択していました。Agent Merge に任せることもできます。Agent Merge を有効にする前に、作業をレビューし、マージ可能であることを確認してください。

## Agent Merge で PR を管理する

コードを作成したら、まとめてレビューして PR を作成し、残りの作業を Agent Merge に任せます。

1. Codespace に戻ります。
2. `/agent` を入力してエージェントダイアログを開きます。
3. **Default** を選択し、<kbd>Enter</kbd> を押します。
4. `/pr create` コマンドで PR を作成します。
5. `/pr agentmerge` を使って Agent Merge を有効にします。
6. Copilot が PR の継続的インテグレーションを監視します。すべて成功すると、マージが実行されます。
7. `PR #14 was squash-merged successfully.` のようなメッセージが Copilot から表示されたことを確認します。

> [!IMPORTANT]
> Agent Merge は、必須の承認、ブランチ保護、マージキュー、リポジトリ設定、権限の不足を回避しません。ブロックされた場合は理由を確認し、リポジトリで許可されている方法でレビュー済みのマージを手動で完了します。

## まとめと次のステップ

コードの生成、テストと検証、プルリクエストのプロセスを自動化しました。このレッスンでは、次の作業を行いました。

- Agent Merge とマージライフサイクルを自動化する方法を学んだ。
- 機能全体と検証の証拠を確認した。
- フィルター機能の PR を作成した。
- レビュー後に Agent Merge を有効にし、PR がマージされたことを確認した。

次は、[GitHub Copilot CLI のスラッシュコマンド][next-lesson]を使い、コンテキスト、モデル、共有、任意のクラウドへの委任を確認します。

## リソース

- [Copilot CLI でプルリクエストを管理する][manage-prs]
- [Copilot CLI コマンドリファレンス][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
