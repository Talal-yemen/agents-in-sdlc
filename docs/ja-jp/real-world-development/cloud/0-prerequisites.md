---
title: "レッスン 0: 前提条件"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Copilot cloud agent のレッスンを始める前に、準備を整えましょう。Tailspin Toys リポジトリの自分用コピーを作成し、[Codespace][codespaces] を起動します。Codespace は、指示ファイルの編集や cloud agent が作成した変更のレビューに使います。

## ワークショップ用リポジトリを設定する

コード作業用のリポジトリを作るため、[テンプレート][template-repository]から自分用のコピーを作成します。新しいリポジトリには、ワークショップに必要なファイルがすべて含まれます。

1. 新しいブラウザーウィンドウで [Tailspin Toys テンプレート][tailspin-template]を開きます。
2. ワークショップ用リポジトリのページで **Use this template** を選択し、次に **Create a new repository** を選択してコピーを作成します。

   ![Use this template ボタン](../../../_images/ex0-use-template.png)

3. GitHub または Microsoft が主催するイベントでワークショップを受講している場合は、メンターの指示に従ってください。それ以外の場合は、GitHub Copilot を利用できる組織にリポジトリを作成します。

   ![リポジトリのテンプレート設定を入力する画面](../../../_images/ex0-repository-settings.png)

4. 作成したリポジトリのパス (**organization-or-user-name/repository-name**) をメモします。後でワークショップ中に使用します。

## Codespace を作成する

Codespace を使ってワークショップを進めます。

[GitHub Codespaces][codespaces] は、ブラウザーから直接コードを作成、実行、デバッグできるクラウド開発環境です。複数のプログラミング言語、拡張機能、ツールを利用できる、機能豊富な IDE が用意されています。

1. 作成したリポジトリを開きます。
2. 緑色の **Code** ボタンを選択します。

   ![Code ボタンを選択する画面](../../../_images/ex0-code-button.png)

3. **Codespaces** タブを選択し、**+** ボタンを選択して Codespace を作成します。

   ![新しい Codespace を作成する画面](../../../_images/ex0-create-codespace.png)

Codespace の作成には数分かかりますが、サービスをすべて手作業でインストールするよりも早く完了します。その間、次のレッスンで紹介する GitHub Copilot の機能を確認してみましょう。

> [!CAUTION]
> 今後のレッスンでこの Codespace に戻ります。ひとまず、ブラウザーのタブを開いたままにしてください。

> [!NOTE]
> このワークショップは Codespace またはローカルの [dev container][dev-containers] で実行できます。どちらも、必要な前提条件が整った環境を提供します。ローカルで実行する場合は、クローンしたリポジトリを VS Code で開き、プロンプトが表示されたら **Reopen in Container** を選択してください。Codespace と同じ dev container が作成されます。

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## まとめ

ワークショップ用リポジトリのコピーを作成し、Copilot cloud agent と一緒に作業するための Codespace の作成を始めました。

## 次のステップ

次は、cloud agent が従うカスタム指示を追加します。[レッスン 1「カスタム指示」][next-lesson]に進みます。

## リソース

- [GitHub Codespaces の概要][codespaces]
- [テンプレートからリポジトリを作成する][template-repository]
- [Codespaces を使い始める][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[next-lesson]: ../1-custom-instructions/
