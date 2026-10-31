---
title: "演習 5: GitHub Copilot の作業を反復改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前の演習: エージェントの管理][previous-lesson] |
|:--|

## 作業をレビューする

このラボでは、ユーザー体験と機能を改善する複数のタスクで GitHub Copilot と作業してきました。コードへのドキュメント追加、デザインチーム向けの関連ゲーム機能、高コントラストとライトモードの切り替えを含むアクセシビリティ機能を依頼しました。コードの変更を確認し、必要に応じて Copilot にフィードバックを送って改善しましょう。

### シナリオ

生成型人工知能 (AI) を利用しても、ソフトウェア設計と開発・運用 (DevOps) の基本は変わりません。生成されたコードをレビューし、通常の DevOps プロセスに沿って進めます。チーム全体のレビューを有効にする前に、ドキュメント、関連ゲーム機能、アクセシビリティ機能について GitHub Copilot が提案した変更を確認します。

## GitHub Copilot cloud agent のセキュリティ

Copilot cloud agent は監督なしで非同期にタスクを実行するため、安全性を保つための制約があります。

- Copilot はリポジトリへの読み取りアクセス権を持ち、書き込みは作業対象のブランチに限られます。
- Cloud agent は GitHub Actions 内で、作業用の独立した一時環境を作成します。
- GitHub Actions ワークフローの実行には、人間による承認が必要です。
- Model Context Protocol (MCP) サーバーを含む[外部リソースへのアクセスは既定で制限されています](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)。

## 生成されたドキュメントをレビューする

まず、GitHub Copilot cloud agent が作成した最初のプルリクエスト (PR)、つまりコードへのドキュメント追加をレビューします。GitHub.com の標準的な PR インターフェイスを使います。

> [!NOTE]
> PR を確認すると、GitHub Copilot がファイアウォールでブロックされたという警告が表示されることがあります。Copilot は MCP サーバーへの呼び出しなど、外部リソースへのアクセスが既定で制限されているため、これは想定された動作です。必要に応じて、[Copilot cloud agent のファイアウォールをカスタマイズまたは無効化][agent-firewall]できます。

1. github.com の自分のリポジトリに戻ります。
2. **Pull Requests** を選択して PR の一覧を開きます。
3. **Add missing documentation** など、同様のタイトルが付いたプルリクエストを開きます。

> [!NOTE]
> Copilot がまだ作業中の場合、PR のタイトルに **[WIP]** が含まれます。その場合は作業が完了するまで待ちます。数分かかることがあるため、休憩したり、これまでに学習した内容を振り返ったりしてください。

4. 準備ができたら **Files changed** タブを選択し、変更をレビューします。

    ![Files changed タブ](../../../_images/shared-pr-files-changed.png)

5. 新たに追加された TypeScript Documentation (TSDoc) のドキュメントコメントや、その他のドキュメントを含むコードを確認します。具体的な変更内容は状況によって異なります。演習 2 で確認した指示ファイルによって追加された TSDoc コメント、TypeScript 規約、ファイルコメントのヘッダーに注目してください。
6. 更新内容をレビューして問題がなければ **Conversation** タブに戻り、下にスクロールします。
7. ワークフローが承認待ちであることを示す表示を確認します。
8. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

    ![Approve and run workflows ボタン](../../../_images/shared-approve-workflows.png)

9. PR のチェック欄にワークフローが追加されます。問題がなければ、単一の Astro アプリのプロジェクトチェックが成功します。完了まで数分かかることがあります。

## GitHub Copilot に変更を依頼する

Copilot との PR 作業は一方通行ではありません。チームメンバーと同じように、PR やコードへのインラインコメントで Copilot に言及できます。Copilot はコメントを認識し、対応するセッションを開始します。結果は確定的ではないため、必ず依頼する文言は決められません。たとえば、次のような変更を依頼できます。

- 各コードファイルの先頭に、ファイルの役割を説明するコメントヘッダーを追加する。
- TypeScript と Astro のファイルに TSDoc のドキュメントコメントを追加する。
- Astro アプリの構成を説明する README を作成する。

1. 上記の例などを参考に、コードベースのドキュメントに関する変更を **@copilot** に依頼するコメントを追加します。ほかのユーザーと同じようにタグ付けしてください。
2. **View Session** を選択し、Copilot が作業する様子を確認します。Copilot が更新用の新しいセッションを開始します。
3. **Back to pull request** を選択すると、PR に戻れます。

    ![Back to pull request ボタン](../../../_images/ex7-back-to-pr.png)

4. Copilot の作業が完了すると、PR に新しいコミットが表示されます。
5. **Files changed** タブを選択して変更をレビューします。

問題がなくなるまで反復してかまいません。満足できたら、ドラフト PR をレビュー可能な状態に変更して `main` ブランチにマージします。

![PR をレビュー可能な状態に変更する画面](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

ゲーム詳細ページに関連ゲームを表示する Issue について、Copilot が作成した PR を確認します。

1. GitHub.com の自分のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Show related games on the game details page** など、同様のタイトルが付いた PR を選択します。
4. **Files changed** タブを選択し、生成されたコードをレビューします。
5. 更新内容に問題がなければ **Conversation** タブに戻り、下にスクロールします。
6. ワークフローが承認待ちであることを示す表示を確認します。
7. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows ボタン](../../../_images/shared-approve-workflows.png)

8. PR のチェック欄にワークフローが追加されます。問題がなければ、単一の Astro アプリのプロジェクトチェックが成功します。完了まで数分かかることがあります。
9. **任意:** 関連ゲーム機能を手動でテストするため、Codespace でこのブランチに切り替えることもできます。Codespace に移動してターミナルを開き、Copilot が作成したブランチ名 (例: **copilot/fix-8**) に置き換えて、次のコマンドを実行します。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot は関連ゲーム機能を作成しました。前と同じように、Copilot cloud agent に追加の変更を依頼して反復できます。たとえば、表示する関連ゲームの数を調整する、コメントヘッダーと TSDoc コメントを追加する、といった依頼ができます。この Issue はカスタム指示を追加する前に割り当てたことを思い出してください。変更は、前と同じように **Conversation** タブでコメントを追加して依頼できます。Copilot はコメントを認識して新しいセッションを開始します。

## アクセシビリティ機能をレビューする

最後に、アクセシビリティのカスタムエージェントが実装した機能をレビューします。この PR には、演習 3 で依頼したハイコントラストモードと、演習 4 でミッションコントロールから依頼したライトモードの両方が含まれます。

1. GitHub.com の自分のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Add high contrast mode to website** など、同様のタイトルが付いた PR を選択します。

> [!NOTE]
> Copilot がまだ作業中の場合、PR のタイトルに **[WIP]** が含まれます。作業が完了するまで待ちます。数分かかることがあります。

4. **Files changed** タブを選択し、生成されたコードをレビューします。
5. 実装を確認し、特に次の点に注意します。
   - 表示モードを切り替えるユーザーインターフェイス (UI) コンポーネント
   - ユーザー設定を保持するためのローカルストレージの使用
   - ハイコントラストモードとライトモードの CSS またはスタイルの変更
   - アクセシビリティ属性 (Accessible Rich Internet Applications (ARIA) ラベル、キーボード操作など)
   - モード切り替えを管理する JavaScript または TypeScript コード
6. 更新内容に問題がなければ **Conversation** タブに戻り、下にスクロールします。
7. ワークフローが承認待ちであることを示す表示を確認します。
8. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows ボタン](../../../_images/shared-approve-workflows.png)

9. PR のチェック欄にワークフローが追加されます。問題がなければ、単一の Astro アプリのプロジェクトチェックが成功します。完了まで数分かかることがあります。
10. **任意:** Codespace でこのブランチに切り替え、アクセシビリティ機能を手動でテストできます。Codespace に移動してターミナルを開き、Copilot が作成したブランチ名に置き換えて、次のコマンドを実行します。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    その後、アプリケーションを起動し、ブラウザーでハイコントラストモードとライトモードを切り替えます。ページを再読み込みしても設定が保持されることを確認します。

アクセシビリティのカスタムエージェントが、アクセシビリティのベストプラクティスに沿って Copilot の実装を導いたことを確認しました。アクセシビリティ上の問題や改善点があれば、前の PR と同様にコメントで **@copilot** に修正を依頼できます。

## 任意の演習 — 作業の委任を続ける

cloud agent は、実際のバックログのタスクを委任し、その間に別の作業に取り組むときに役立ちます。いくつか Issue を作成して Copilot に割り当て、習慣として身に付けてください。たとえば、次の作業を依頼できます。

- ゲーム詳細ページに支援者向けの関心フォームを作成する。
- ゲーム一覧ページにページネーションを実装する。
- データアクセスヘルパーに入力検証とエラー処理を追加する。

## まとめ

Cloud agent ハーネスを完了しました。このレッスンでは、次のことを行いました。

- このリポジトリに含まれるカスタム指示ファイルを確認し、後で cloud agent の出力に適用される内容を把握した。
- Issue を Copilot cloud agent に割り当て、環境の準備、計画、非同期の実行を確認した。
- アクセシビリティ向けのカスタムエージェントを作成して使用し、ハイコントラストモードとライトモードを追加した。
- Copilot agents ページをミッションコントロールとして使用し、実行中のアクセシビリティセッションを監視して誘導した。
- cloud agent のプルリクエストをレビューして改善し、**@copilot** に変更を依頼し、ワークフローを承認した。

## 振り返りと次のステップ

Cloud agent ハーネスを完了しました。さらに学ぶ場合は、ほかのハーネスで今回の内容を発展できます。

- 🖥️ **[VS Code ハーネス](../../vscode/)** — 統合開発環境 (IDE) から Copilot Chat のエージェントモードと MCP 統合を試します。
- 💻 **[CLI ハーネス](../../cli/)** — コマンドラインインターフェイス (CLI) から Plan と Autopilot モード、エージェントスキル、QA カスタムエージェント、Playwright MCP、Agent Merge、任意の `/delegate` による cloud agent への連携を使って、レビュー済みの変更を2つ作成します。

自分のリポジトリでは、次の作業も試してください。

- リファクタリングやテストカバレッジの Issue を cloud agent に割り当てる。
- 別の分野向けのカスタムエージェントを作成する。
- 別の技術スタック向けに `copilot-setup-steps.yml` を設定する。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot agents について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップワークフローに関するベストプラクティス][cloud-agent-best-practices]
- [Copilot cloud agent のファイアウォールを設定する][agent-firewall]

---

| [← 前の演習: エージェントの管理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall
[previous-lesson]: ../4-managing-agents/
