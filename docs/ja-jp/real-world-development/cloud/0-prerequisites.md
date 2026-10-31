---
title: "演習 0: 前提条件"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Copilot cloud agent の演習を始める前に、必要なものを準備します。Tailspin Toys リポジトリの自分用コピーを作成し、指示ファイルの編集や cloud agent が作成した変更のレビューに使う [Codespace][codespaces] を起動します。

## ラボ用リポジトリを設定する

コード作成に使うリポジトリのコピーを、[テンプレート][template-repository]から作成します。新しいリポジトリにはラボに必要なファイルがすべて含まれています。演習ではこのリポジトリを使用します。

1. 新しいブラウザーウィンドウで [Tailspin Toys のテンプレート][tailspin-template]を開きます。
2. ラボ用リポジトリのページで **Use this template** ボタンを選択し、**Create a new repository** を選択して自分用のコピーを作成します。

    ![Use this template ボタン](../../../_images/ex0-use-template.png)

3. GitHub または Microsoft が主催するイベントの一環としてワークショップに参加している場合は、メンターの指示に従ってください。それ以外の場合は、GitHub Copilot を利用できる Organization に新しいリポジトリを作成できます。

    ![リポジトリテンプレートの設定を入力するフォーム](../../../_images/ex0-repository-settings.png)

4. 作成したリポジトリのパス (**organization-or-user-name/repository-name**) を記録します。このラボで後ほど使用します。

## Codespace を作成する

次に、ラボの演習で使用する Codespace を作成します。

[GitHub Codespaces][codespaces] はクラウドベースの開発環境です。ブラウザーで直接コードを書き、実行し、デバッグできます。複数のプログラミング言語、拡張機能、ツールに対応した統合開発環境 (IDE) を利用できます。

1. 作成したリポジトリを開きます。
2. 緑色の **Code** ボタンを選択します。

    ![Code ボタンを選択する画面](../../../_images/ex0-code-button.png)

3. **Codespaces** タブを選択し、**+** ボタンを選択して新しい Codespace を作成します。

    ![新しい Codespace を作成する画面](../../../_images/ex0-create-codespace.png)

Codespace の作成には数分かかりますが、サービスを手動でインストールするよりも早く準備できます。その間に GitHub Copilot のほかの機能を確認してもかまいません。次の演習で詳しく扱います。

> [!CAUTION]
> 今後の演習で Codespace に戻ります。現時点では、ブラウザーのタブを閉じずに開いたままにしてください。

> [!NOTE]
> このワークショップは Codespace またはローカルの [dev container][dev-containers] で実行できます。どちらも必要な前提条件がインストールされた環境を提供します。ローカルで実行する場合は、クローンしたリポジトリを VS Code で開き、求められたら **Reopen in Container** を選択してください。Codespace と同じ dev container が構築されます。

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## まとめ

ラボ用リポジトリのコピーを作成し、Codespace の作成を開始しました。以降の演習では、Copilot cloud agent と並行して作業するためにこの Codespace を使用します。

## 次のステップ

cloud agent が従うカスタム指示を追加します。[演習 1 - カスタム指示][next-lesson]に進みます。

## リソース

- [GitHub Codespaces の概要][codespaces]
- [テンプレートからのリポジトリの作成][template-repository]
- [Codespaces の開始方法][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[next-lesson]: ../1-custom-instructions/
