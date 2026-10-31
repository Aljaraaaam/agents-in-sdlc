---
title: "演習 4 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前の2つの演習では、ユーザー体験の改善と機能追加を目的に、Copilot cloud agent に3つの別々のタスクを依頼しました。cloud agent は非同期かつ自律的に作業しますが、タスクを監視することも重要です。

GitHub.com の[エージェントページ][agents-page]など、cloud agent に割り当てたタスクを管理するツールがあります。このミッションコントロールでは、オープンなプルリクエスト (PR) があるすべてのエージェントタスクを確認できます。実行内容を調べたり、進行中のセッションを誘導したりできます。

このレッスンでは、次のことを行います。

- エージェントページで cloud agent のタスクを監視する。
- 実行中のセッションに追加の機能を依頼する。

## シナリオ

ハイコントラストモードの作成をエージェントに依頼した後、チームはライトモードも追加するとよいと気付きました。サイトのスタイル更新と切り替え機能をすでに実装中のため、これも含めるのが合理的です。エージェントに追加の作業を依頼し、ハイコントラストとライトモードの両方を実装してもらいます。

## Copilot cloud agent のタスクを確認する

Copilot cloud agent に割り当てたタスクの現在の状態を確認します。

1. [Copilot agents ページ](https://github.com/copilot/agents)を開きます。
2. メインペインと左ペインのタスクリストを確認します。割り当てた次のタスクが表示されます。
   - コードベースのドキュメントを更新する。
   - ゲームを変更するアプリケーションプログラミングインターフェイス (API) を生成する。
   - Web サイトにハイコントラストモードを追加する。
3. 実行中のタスクを選択し、Copilot が実行した作業を確認します。たとえば、コードのチェックアウト、作業環境の作成、Model Context Protocol (MCP) サーバーの設定、タスクを完了するための各手順などが表示されます。

> [!NOTE]
> 表示される手順は、Copilot の作業状況や採用した方法によって異なります。

4. 右側に表示される PR ペインも確認します。ここから PR と変更ファイルを確認し、さらに監視できます。

## cloud agent の作業を誘導する

実行中のタスクを確認したので、ハイコントラストモードの作業中にライトモードの切り替えも追加するよう Copilot に依頼します。

1. ハイコントラストモードのセッションを選択します。タイトルは Copilot が付けた名前や作業状況に応じて異なります。

    ![ミッションコントロールのアクセシビリティセッション](../../../_images/ex6-accessibility-session.png)

2. セットアップが完了し、作業が始まったことを示すメッセージが表示されるまで、数分間セッションを確認します。
3. **Steer active session while Copilot is working** ダイアログに次のプロンプトを入力します。

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![エージェントページで、Steer active session while Copilot is working ダイアログが強調表示された cloud agent タスク](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot がプロンプトを受け取り、作業に追加したことを確認します。

## Copilot の作業を見守る

Copilot は更新されたタスクに取り組みます。メッセージを送信した時点で作業中だった手順を終えると、新しい依頼をワークフローに加えます。

この作業にも数分かかります。その間に、これまでに学習した内容を振り返ってください。

## まとめと次のステップ

Copilot cloud agent のタスクを監視し、誘導する中心的な場所である Copilot agents ページを確認しました。進行中と完了済みのタスクを追跡し、実行中の作業をレビューし、追加指示で範囲を調整できます。

この演習では、次のことを行いました。

- ミッションコントロールとエージェントページで cloud agent のタスクを監視した。
- 実行中のセッションに追加機能を依頼した。

Copilot がアクセシビリティ機能を実装している間、次は[作成されたプルリクエストを改善します][next-lesson]。

## リソース

- [Copilot agents ページ][agents-page]
- [カスタムエージェントについて][custom-agents]

---

| [← 前の演習: カスタムエージェント][previous-lesson] | [次の演習: Copilot の作業を反復改善する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
