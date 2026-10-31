---
title: "レッスン 12 - GitHub Pages でサイトを公開する"
description: "GitHub Copilot app を使って Pages ワークフローを構成し、プルリクエストをレビューして、公開された Space Quiz を確認します。"
authors:
  - jamesmontemagno
lastUpdated: 2026-10-12
---

GitHub Pages を使って Space Quiz を公開し、誰でもアクセスできる場所を用意します。Copilot はワークフローの準備とデプロイを支援しますが、変更をレビューし、サイトが公開されたことを確認するのは開発者の役割です。

このレッスンでは、次の内容を学習します。

- GitHub Pages のデプロイを Copilot に依頼する。
- マージ前にワークフローとプルリクエストをレビューする。
- デプロイを追跡し、公開されたサイトを確認する。

## Copilot にサイトのデプロイを依頼する

GitHub Pages の設定、デプロイ用ワークフロー、公開後の確認をまとめて依頼します。次のプロンプトは、必要な権限がない場合に成功と誤って報告しないよう制約も指定しています。

1. `space-quiz` の Copilot app セッションで、次のプロンプトを送信します。

   ```plaintext
   Enable GitHub Pages for this repository. Create a GitHub Actions workflow that builds the website if needed and deploys it to GitHub Pages. Add a README describing the app and how to run it locally if one does not already exist. Create a pull request with these changes and merge it after required checks pass. Monitor the deployment until it succeeds, then report the final URL of the live website. If you lack permission to enable Pages or merge, tell me exactly what I need to approve or do; do not report success until the site is live.
   ```

![マージ済みの space-quiz プルリクエスト、Build website、Upload artifact、Deploy to Pages の各ステップが成功した GitHub Actions の Deploy website 実行、および yourname.github.io/space-quiz/ で公開された Space Quiz の3段階の図。](../../../_images/first-steps-app-pages-deploy.svg)

> [!WARNING]
> このプロンプトは、エージェントにプルリクエストの作成とマージ、およびリポジトリの Pages 設定の変更を依頼します。続行する前に、差分、デプロイ権限、GitHub の承認要求を確認してください。

## ワークフローをレビューしてデプロイを確認する

デプロイの準備が適切に行われ、サイトが実際に公開されたことを確認します。

1. README は、リポジトリにまだ存在しない場合にのみ追加されていることを確認します。
2. ワークフローのビルド手順とデプロイ手順を含め、プルリクエストの変更をレビューします。
3. 必須のプルリクエストチェックが成功した後にのみマージします。
4. GitHub Actions のデプロイ実行が成功するまで確認します。
5. 報告された URL を開き、Space Quiz が読み込まれることを確認します。

エージェントが Pages の有効化やプルリクエストのマージを実行できない場合は、案内された手順に従います。デプロイが成功し、公開サイトが読み込まれるまでは完了とみなさないでください。

## まとめと次のステップ

Space Quiz を公開し、エージェントの報告だけに頼らず、デプロイを確認しました。

- GitHub Pages のデプロイを Copilot に依頼した。
- マージ前にワークフローとプルリクエストをレビューした。
- デプロイを追跡し、公開されたサイトを確認した。

次は、[レッスン 13: 振り返りと次のステップ][next-lesson]に進みます。

[next-lesson]: ../13-review/
