---
title: "レッスン 3 - エージェントモード: Plan と Autopilot"
description: "Plan でアプローチに合意し、Autopilot で Issue からフィルター機能を構築して、Interactive モードで結果をレビューします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

最初に小さな機能をプロジェクトに追加しました。より大きな変更には、堅牢なプロセスが必要です。GitHub Copilot CLI は組織の既存の開発フローに沿って動作します。このレッスンから数回にわたり、Issue から新機能を作成し、コードの有効性と動作を確認して、プロジェクトにマージする一般的なエージェント主導の開発プロセスを実践します。

> [!NOTE]
> 機能の作業を続ける間は、同じ会話とブランチを使用します。通常は作業対象のファイル種別ごとに異なるブランチや PR を使いますが、ここでは主な概念に集中できるよう手順を簡略化します。

このレッスンでは、次の作業を行います。

- GitHub Issue から Copilot の会話を開始する。
- Plan モードで要件を定義する。
- Autopilot モードで機能を実装する。
- コードをレビューする。
- 転送されたブラウザーで機能を手動検証する。

この機能の作業を続けながら、リポジトリの指示を更新し、既存の `quality-checks` スキルをカスタマイズし、MCP による検証を追加して QA エージェントを作成し、機能の PR を開きます。

## シナリオ

Tailspin Toys のカタログが増え、訪問者がカテゴリーとパブリッシャーでゲームを絞り込む必要があります。バックログの Issue には機能が記載されていますが、カテゴリーの組み合わせ方など、コーディング前に合意すべき点があります。Plan モードで決定し、Autopilot で範囲を限定して実装します。

## 背景

AI コーディングエージェントを開発フローに導入しても基本は変わりません。多くの開発者は次のような流れで作業します。

1. 必要な作業を説明する既存の Issue を開く。
2. 構築する内容を計画する。
3. コードを作成してレビューする。
4. テストを実行してコードを検証する。
5. 新機能を手動で検証する。
6. プルリクエスト (PR) を作成する。
7. コードレビューと継続的インテグレーションが成功したら、コードをマージする。

> [!NOTE]
> チームや Organization によって具体的な手順は異なりますが、通常は上記の流れを基にしています。

この標準的な方法に従うことで、AI が生成したコードが要件を満たし、手作業で作成したコードと同じレビューを受けるようにできます。

## 会話モード

**会話モード**は、エージェントがどの程度自律して動作するかを制御します。<kbd>Shift</kbd>+<kbd>Tab</kbd> でモードを切り替えます。

- **Interactive**: 開発者とエージェントが共同で作業します。エージェントは変更を提案し、続行前に入力を待ちます。
- **Plan**: エージェントは最初に計画を作成し、プロジェクトファイルを編集する前に承認を待ちます。
- **Autopilot**: エージェントは自律してコードを書き、テストを実行し、タスクが完了するまで反復します。

Plan モードで開始して計画をレビューし、その後 Autopilot で実装します。

## Issue から作業を開始する

フィルター機能の作業を始める前に、Codespace とターミナルを準備します。

1. Codespace に戻ります。停止している場合は再起動します。
2. 星評価の PR がマージ済みであることを確認します。
3. ターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押します。
4. `main` を更新し、フィルター機能用のブランチを作成します。

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Copilot CLI を起動します。

   ```bash
   copilot --yolo
   ```

6. <kbd>Tab</kbd> を2回押して **Issues** タブを開きます。
7. <kbd>A</kbd> を押してすべての Issue を表示します。
8. 矢印キーで **Allow users to filter games by category and publisher** というタイトルの Issue を選択します。
9. <kbd>C</kbd> を押して Issue をプロンプトに追加し、**Session** タブに戻ります。

プロンプトの先頭に `#7` (または同様の番号) が追加されます。`#` を使うと、GitHub 上の Issue やプルリクエスト (PR) をコンテキストに含められます。

## フィルター機能を計画する

計画を立てると、Copilot に実装を任せる前にアプローチを定義できます。複雑なタスクでは、計画に時間をかけることをお勧めします。Plan モードに切り替えて計画の作成を依頼します。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Plan モードに切り替えます。プロンプトの下に **Plan** と表示されていることを確認します。
2. 直前の手順で追加した Issue 参照の後に、次のプロンプトを入力します。

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot はプロジェクトを調査し、最適な方法を検討して計画を作成します。

3. Copilot からフィルターの動作について質問された場合は、希望に沿って回答します。唯一の正解はありません。
4. 計画ができたら、<kbd>Control</kbd>+<kbd>E</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) を押して展開します。
5. 計画を読みます。
6. 自分の決定と一致しない部分があれば、Copilot に修正を依頼します。

## Autopilot を承認する

計画を確認したら、Autopilot を使って実装します。Autopilot は、タスクが完了したと判断するまで Copilot に反復させます。

1. **Accept plan and build on autopilot (recommended)**、または使用中のバージョンで表示される同等のオプションを選択します。
2. プロンプトの下に **Autopilot** と表示されていることを確認します。
3. Copilot が計画に沿って作業し、コードを生成してテストを実行する様子を確認します。

> [!NOTE]
> 承認すると実装がすぐ始まる場合があるため、先に計画をレビューしてください。依存関係の不足やポート競合が報告された場合は、チェック完了と判断する前にセットアップの問題を解決します。

## 実装をレビューして検証する

生成されたコードは、ほかのコードと同じようにマージ前にレビューします。コードを確認し、サイトを実行して動作を検証します。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Interactive モードに切り替えます。モード表示が **Plan** または **Autopilot** ではないことを確認します。
2. `/diff` を入力し、フィルター機能の実装とテストを確認します。
3. 結果を Issue と計画中に決めた内容に照らし合わせます。
4. コードをレビューしたら <kbd>Esc</kbd> を押して差分画面を閉じます。
5. プロジェクトのチェック結果を確認し、失敗があれば Copilot に解消を依頼します。

## 新機能を確認する

コードに問題がないように見えても、実際に動くか確認します。アプリを起動し、Codespaces で転送されたポートからサイトを開きます。

1. 次のプロンプトでアプリの起動を依頼します。

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces でポート `4321` が利用可能と表示されたら、**Open in Browser** を選択します。
3. 計画で決めたカテゴリー、パブリッシャー、およびその組み合わせによる絞り込みを試します。
4. リセットと検索結果がない場合の動作が、Issue と決定した内容に一致することを確認します。
5. Codespace に戻り、Copilot に起動した開発サーバーを停止するよう依頼します。

## まとめと次のステップ

異なる会話モードで機能を構築し、レビューしました。このレッスンでは、次の作業を行いました。

- GitHub Issue から Copilot の会話を開始した。
- Plan モードで要件を定義した。
- Autopilot モードで機能を実装した。
- コードをレビューした。
- 転送されたブラウザーで機能を手動検証した。

次は、[カスタム指示を使って][next-lesson]、コードが文書化された慣行に従うようにします。

## リソース

- [GitHub Copilot CLI の Autopilot][autopilot]
- [Copilot CLI コマンドリファレンス][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
