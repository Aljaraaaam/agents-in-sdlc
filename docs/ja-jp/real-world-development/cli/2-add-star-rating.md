---
title: "レッスン 2 - 星評価の追加で小さな成果を得る"
description: "Copilot CLI でゲームカードに小さな変更を加え、転送されたブラウザーでレビューし、最初のプルリクエストとしてマージします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Copilot CLI をインストールし、会話を試しました。次は、プロジェクトに最初の変更を加えます。ゲームのデータにはすでに星評価がありますが、ホームページのゲームカードにはまだ表示されていません。エージェントに表示を依頼し、変更をレビューして、最初のプルリクエストとしてマージします。

このレッスンでは、次の作業を行います。

- 機能ブランチで Copilot との会話を開始する。
- プロジェクトに小規模な変更を加えるようエージェントに依頼する。
- `/diff` を使って変更をレビューする。
- 転送されたブラウザーでアプリを実行し、変更を確認する。
- 最初のプルリクエストを作成してマージする。

## シナリオ

Tailspin Toys の各ゲームには星評価を設定でき、ゲーム詳細ページにはすでに表示されています。一方、ホームページのゲームカードには、タイトル、カテゴリー、パブリッシャー、説明だけが表示されています。準備運動として、各カードに既存の評価を表示するようエージェントに依頼します。小規模で自己完結した、最初のセッションに最適な変更です。

## 会話の構成

**会話**では、タスクについて Copilot CLI と作業します。Copilot app と異なり、通常の CLI 会話では専用の worktree は作成されず、ターミナルで現在チェックアウトしているリポジトリと Git ブランチが使われます。保存済みの会話に後から戻ることもできます。ファイルとブランチは、通常の Git 状態としてディスク上に保持されます。

会話では、プロンプトとエージェントの回答、ファイルを調査・編集する際のツールアクティビティ、`/diff` で確認できる変更の3つを扱います。

## 会話を開始して変更を依頼する

新しい会話を開始し、機能を実装します。

1. Codespace に戻ります。
2. ターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押します。
3. Copilot が起動していない場合は、次のコマンドを実行します。

   ```bash
   copilot --yolo
   ```

4. スラッシュコマンド `/new` を入力し、<kbd>Enter</kbd> を押して新しいセッションを開始します。
5. 次のプロンプトで変更を依頼します。

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

Copilot はプロジェクトを調査し、ゲーム詳細の表示に使われているファイルを見つけ、必要なコードを作成します。Copilot CLI で新機能を追加できました。

## 差分をレビューする

AI が生成した変更は、小さなものでもマージ前にレビューします。Copilot CLI 内で変更を確認します。

1. `/diff` を入力し、変更されたすべてのファイルを確認します。
2. 評価があるゲームカードに数値が表示され、`starRating` が `null` の場合は `No rating yet` と表示されることを確認します。
3. テストで両方の状態がカバーされていることを確認します。
4. Copilot が実行したチェックの結果を確認し、失敗があれば修正を依頼します。
5. レビューが終わったら <kbd>Esc</kbd> を押して差分画面を閉じます。

> [!NOTE]
> Copilot は、すべての生成 AI ツールと同様に確率的に動作するため、コードは例と異なる場合があります。特定の実装に一致するかではなく、動作をレビューしてください。

## 変更を確認する

コードを読むだけで動くと判断せず、Copilot に Web サイトを起動してもらい、Codespaces で転送されたブラウザーから更新されたユーザーインターフェイス (UI) を確認します。

1. 次のプロンプトでアプリの起動を依頼します。

   ```plaintext
   Start the app so I can inspect the star-rating change in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces でポート `4321` が利用可能と表示されたら、**Open in Browser** を選択します。
3. ゲームカードに5点満点の評価が表示されることを確認します。
4. Copilot に戻り、起動したサーバーを停止するよう依頼します。

   ```plaintext
   Stop the development server you started.
   ```

## 最初のプルリクエストを作成してマージする

機能を作成できました。新しいコードをプロジェクトにマージするため、プルリクエスト (PR) を作成します。

1. 既定のエージェントに、変更のコミットを依頼します。

   ```plaintext
   Commit the reviewed star-rating changes with an appropriate commit message.
   ```

2. `/pr create` を入力します。Copilot CLI は PR の作成時に既存のコミットをプッシュし、PR の URL を表示します。
3. Copilot CLI が表示した URL を、<kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら選択して PR を開きます。
4. 変更されたファイルとチェックをレビューします。
5. 準備ができたら **Merge pull request** を選択し、マージを確認します。
6. Codespace に戻り、`/exit` で Copilot CLI を終了します。
7. ローカルの `main` を更新します。

   ```bash
   git checkout main
   git pull
   ```

## まとめと次のステップ

GitHub Copilot CLI を使って最初の変更をリリースしました。このレッスンでは、次の作業を行いました。

- 機能ブランチで Copilot との会話を開始した。
- ゲームカードに小規模な変更を加えるようエージェントに指示した。
- `/diff` を使って変更をレビューした。
- 転送されたブラウザーでアプリを実行し、星評価を確認した。
- 最初のプルリクエストを作成してマージした。

次は、[フィルター機能の Issue から開始し、Plan と Autopilot モードを使って][next-lesson]、より大きな機能を構築します。

## リソース

- [GitHub Copilot CLI について][about-copilot-cli]
- [Copilot CLI コマンドリファレンス][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
