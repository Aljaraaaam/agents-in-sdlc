---
title: "レッスン 4 - カスタム指示で Copilot を導く"
description: "リポジトリの指示を確認し、ドキュメントの規約を追加して、フィルター機能のコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を使うときは、コンテキストが重要です。タスクの進め方に決まりがある場合は、Copilot がその指針を参照できるようにします。[指示ファイル][instruction-files]では、作成するコードだけでなく、その構成方法も説明できます。フィルター機能を実装したので、Copilot が使った指示を確認し、ドキュメント規約を追加して、コードに適用します。

このレッスンでは、次の作業を行います。

- リポジトリの指示とパス別の指示ファイルがエージェントに適用される方法を確認する。
- コーディング規約に従うよう指示ファイルを更新する。
- 指示ファイルがコードに与える影響を確認する。

## シナリオ

Tailspin Toys には開発慣行に関するガイドラインがあります。

- コメントではコードを言い換えるのではなく、意図や自明でない判断を説明する。
- `db/` と `src/lib/` のエクスポートされた関数では、目的、パラメーター、戻り値を TSDoc/JSDoc で説明する。該当する場合は、注入可能な `db` 引数も含める。
- 再利用可能な Astro コンポーネントでは `Props` の契約を説明し、関連するコードの変更に合わせてコメントを更新する。
- 既存のフォーマットと lint の指示を維持する。

指示ファイルを使い、Copilot がこれらの慣行に沿って作業できるようにします。

## 指示ファイル

カスタム指示は、コーディングスタイルや要件に関するコンテキストを Copilot に提供します。規約、ライブラリ、コードコメントの形式などを指定できます。リポジトリ全体または特定のファイル種別に指示を設定できます。

- `.github/copilot-instructions.md` は、リポジトリへのすべてのリクエストで Copilot に送られる単一の指示ファイルです。多くのリクエストに関係するプロジェクト全体の情報を記載します。
- `.github/instructions/*.instructions.md` は、特定の言語、ファイル種別、タスク向けの指針を記載します。

> [!NOTE]
> ほかの指示形式やサポート状況は環境によって異なります。特定の形式に依存する前に、[カスタム指示のサポートに関するリファレンス][custom-instructions-support]を確認してください。

## このプロジェクトのカスタム指示ファイルを確認する

スターターには指示ファイルが含まれています。変更前に内容を確認します。

1. Codespace に戻ります。
2. Codespaces エディターで (ターミナルではなく) `.github/copilot-instructions.md` を開きます。
3. プロジェクトの概要とコーディング指針を確認します。この指示は、リポジトリ内で Copilot と行うすべてのやりとりに適用されます。
4. `.github/instructions` フォルダーを開き、Astro、Drizzle データ層、テストなどの指示ファイルを確認します。
5. `.github/instructions/unit-tests.instructions.md` を開き、先頭の `applyTo` フィールドが対象ファイルを決める glob であることを確認します。
6. `.github/instructions/drizzle.instructions.md` を開き、ほかの指示ファイルや既存ファイルへのリンクを確認します。指示を再利用可能な小さなファイルに分け、Copilot に例を示せます。

## チームの指針に合わせて指示ファイルを更新する

既存の指示に、追加で明確にすべき点があります。コアの `copilot-instructions.md` を変更し、新しく生成される TypeScript に TSDoc コメントが含まれるようにします。

1. `.github/copilot-instructions.md` で、27 行目付近にある **Code formatting guidance** セクションを見つけます。
2. 最後の箇条書きとして、次の行を追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

ファイルは自動的に保存されます。

## 更新した指針を使う

Copilot CLI は会話の開始時にリポジトリの指示を読み込みます。フィルター機能のコンテキストを維持したまま新しい指示を読み込むため、変更後に会話を終了して再開します。

1. フィルター機能の会話が開いている場合は `/exit` を入力して閉じます。Copilot CLI は会話を自動的に保存します。
2. `game-filters-cli` ブランチのリポジトリルートで、保存した会話を再開します。

   ```bash
   copilot --resume
   ```

   選択を求められた場合はフィルター機能の会話を選び、`game-filters-cli` ブランチがチェックアウトされていることを確認します。
3. Copilot に更新した指針を適用するよう依頼します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. `/diff` を入力し、変更された TypeScript ファイルを確認します。新しい TSDoc コメントがコードを正確に説明していることを確認します。

## まとめと次のステップ

Copilot CLI が指示ファイルからコンテキストを取得する方法を確認し、新しい規約を機能に適用しました。このレッスンでは、次の作業を行いました。

- リポジトリの指示とパス別の指示ファイルがエージェントに適用される方法を確認した。
- コーディング規約に従うよう指示ファイルを更新した。
- 指示ファイルがコードに与える影響を確認した。

次は、[再利用可能な quality-checks スキルをカスタマイズして実行し][next-lesson]、lint とテストが一貫して実行されるようにします。

## リソース

- [Copilot CLI にカスタム指示を追加する][instruction-files]
- [カスタム指示のサポート][custom-instructions-support]
- [カスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
