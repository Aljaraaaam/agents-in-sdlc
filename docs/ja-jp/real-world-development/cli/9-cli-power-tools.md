---
title: "レッスン 9 - GitHub Copilot CLI のスラッシュコマンド"
description: "コンテキストの管理、モデルの選択、セッションの共有、任意のクラウドへの委任に使うスラッシュコマンドを確認します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

多くの CLI ツールと同様に、GitHub Copilot CLI には対話に使えるスラッシュコマンドが多数あります。高度な機能、内部情報、追加の構成オプションにアクセスできます。これまでに `/diff`、`/mcp`、`/skills`、`/agent`、`/pr` などを使いました。ここでは、ほかの便利なコマンドを確認します。

このレッスンでは、次の作業を行います。

- `/context` と `/compact` を使い、Copilot が会話のコンテキストを管理する方法を確認する。
- `/model` を使い、利用できるモデルを確認する。
- `/share` でセッションをエクスポートまたは共有する方法を学ぶ。
- 並列作業、worktree、cloud agent への委任に使う任意のコマンドを確認する。

## シナリオ

CLI の主なワークフローを完了しました。次は、コンテキストの管理、モデルの切り替え、セッションの共有、[Copilot cloud agent][about-cloud-agent] への任意の委任など、追加機能を確認します。

## Copilot CLI のコンテキストを確認する

大規模または複雑なタスクでは、モデルの最大コンテキストウィンドウに達することがあります。Copilot CLI は必要に応じて会話を自動的に圧縮します。また、スラッシュコマンドを使ってコンテキストを確認したり圧縮したりできます。

1. Codespace に戻り、Copilot CLI が起動していない場合はリポジトリルートから起動します。
2. 次を入力します。

   ```plaintext
   /context
   ```

3. モデル、現在のトークン使用量、システム指示、ツール、メッセージ、空き領域へのコンテキストの割り当てを確認します。
4. 会話を圧縮します。

   ```plaintext
   /compact
   ```

5. もう一度 `/context` を入力して結果を比較します。会話が短い場合は、大きな違いがないこともあります。

> [!NOTE]
> コンテキストウィンドウが埋まると、Copilot CLI は自動的にコンテキストを圧縮します。自分のタイミングで圧縮する場合は `/compact` を使います。関係のない別のタスクに移るときに新しい会話を始めるには、`/clear` または `/new` を使います。

## モデルを選ぶ

モデルごとに得意分野が異なり、開発者によって好みも異なります。Copilot CLI では、利用できるモデルを一覧表示し、使用するモデルを選択できます。

1. 次を入力します。

   ```plaintext
   /model
   ```

2. 利用可能なモデルと使用状況に関する情報を確認します。
3. 現在のモデルを維持するか、別のモデルを選択します。一覧を閉じるには <kbd>Esc</kbd> を押します。

## セッションを共有する

チームで作業し、学んだことを共有すると、AI ツールをより効果的に使えます。`/share` コマンドを使うと、セッションを Markdown または HTML ファイルにエクスポートしたり、共有可能なリンクを作成したり、GitHub gist として公開したりできます。

1. `/help` を入力し、使用中のバージョンで利用できる `/share` のオプションを確認します。
2. セッションを共有する場合は、ローカルの Markdown へのエクスポートに使う `/share file` など、目的に合った出力先を選択します。
3. エクスポートした内容を誰かに送信したり公開したりする前にレビューします。セッションのエクスポートにはプロンプト、応答、プロジェクトの詳細が含まれることがあります。

リンクや gist の公開は任意です。チームが共有するつもりのないリポジトリや会話の内容は公開しないでください。

## 任意: 作業を拡張または委任する

ワークショップの主な作業は完了しました。大規模なタスクでは、Copilot CLI の次のコマンドも利用できます。

- `/fleet` は独立したサブタスクをサブエージェントに分割し、並行して実行します。
- `/worktree` は別のタスク用に分離された Git worktree を作成します。
- `/delegate` はタスクを Copilot cloud agent に送信します。エージェントは非同期に作業し、プルリクエストを作成する場合があります。

これらのコマンドは追加の worktree やリモート作業を作成する可能性があるため、任意です。試すときは、範囲を明確にした新しいタスクから始め、通常のワークフローで結果をレビューします。非同期エージェントについて詳しく知るには、[cloud agent ワークショップ][cloud-workshop]に進んでください。

## まとめと次のステップ

Copilot CLI のスラッシュコマンドを使って構成を変更し、セッションを共有し、内部で起きていることを確認しました。このレッスンでは、次の作業を行いました。

- `/context` と `/compact` を使い、Copilot が会話のコンテキストを管理する方法を確認した。
- `/model` を使い、利用できるモデルを確認した。
- `/share` でセッションをエクスポートまたは共有する方法を学んだ。
- 並列作業、worktree、cloud agent への委任に使う任意のコマンドを確認した。

Copilot CLI にはほかにもスラッシュコマンドがあります。次は[学んだ内容を振り返り][next-lesson]、学習を続けるための方法を確認します。

## リソース

- [Copilot CLI コマンドリファレンス][cli-reference]
- [Copilot CLI のコンテキスト管理][context-management]
- [Copilot cloud agent について][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
