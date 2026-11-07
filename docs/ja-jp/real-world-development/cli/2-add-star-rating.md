---
title: "レッスン 2 - 星評価を追加する: 小さな成果"
description: "Copilot CLI でゲームカードに小さな変更を加え、転送されたブラウザーで確認し、最初のプルリクエストをマージします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Copilot CLI をインストールして会話を試したので、プロジェクトに最初の変更を加えましょう。小さな変更から始めます。ゲームのデータには星評価がすでにありますが、ホームページのゲームカードには表示されていません。エージェントに表示を依頼し、変更をレビューして、最初のプルリクエストとしてマージします。

このレッスンでは、次のことを行います。

- 機能ブランチで Copilot の会話を始めます。
- エージェントに小さな変更を依頼します。
- `/diff` で変更をレビューします。
- アプリを起動し、Codespaces で転送されたブラウザーから確認します。
- 最初のプルリクエストを作成してマージします。

## シナリオ

Tailspin Toys の各ゲームには星評価を設定できます。ゲーム詳細ページにはすでに表示されていますが、ホームページのゲームカードに表示されるのはタイトル、カテゴリ、publisher、説明のみです。最初の練習として、既存の評価を各カードに表示するようエージェントに依頼します。小さく完結した変更なので、初回のセッションに適しています。

## 会話の構成

**会話**は、Copilot CLI でタスクに取り組む場所です。Copilot app と異なり、通常の CLI 会話はターミナルで現在チェックアウトされているリポジトリと Git ブランチを使い、専用の worktree は作成しません。保存された会話を使うと、後で同じやりとりに戻れます。ファイルとブランチは通常の Git 状態としてディスク上に残ります。

会話では、プロンプトとエージェントの応答、ファイルを調査して編集するエージェントのツール操作、`/diff` で確認できる変更の3つを確認できます。

## 会話を始めて変更を依頼する

機能の実装を始めるため、新しい会話を開始します。

1. Codespace に戻ります。
2. ターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押します。
3. Copilot CLI が起動していない場合は、次のコマンドを実行します。

   ```bash
   copilot --yolo
   ```

4. slash command `/new` を入力して <kbd>Enter</kbd> を押し、新しいセッションを開始します。
5. 次のプロンプトで変更を依頼します。

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

Copilot はプロジェクトを調査し、ゲーム詳細を表示するファイルなどを特定して必要なコードを作成します。Copilot CLI で新機能を追加できました。

## 差分をレビューする

小さな変更でも、AI が生成した変更はマージする前にレビューします。Copilot CLI で変更を確認しましょう。

1. `/diff` を入力し、変更されたファイルをすべて確認します。
2. ゲームカードに数値評価が表示され、`starRating` が `null` の場合は `No rating yet` と表示されることを確認します。
3. 両方のケースがテストされていることを確認します。
4. Copilot が実行したチェックの結果を確認し、失敗があれば修正を依頼します。
5. レビューが終わったら <kbd>Esc</kbd> を押して差分画面を閉じます。

> [!NOTE]
> すべての生成 AI ツールと同様に、Copilot は確率的に動作するため、正確なコードは異なる場合があります。特定の実装を期待するのではなく、動作を確認してください。

## 変更を確認する

コードを読むだけで動作すると判断せず、Codespaces で転送されたブラウザーから UI を確認します。

1. Copilot に次のプロンプトを送り、アプリの起動を依頼します。

   ```plaintext
   Start the app so I can inspect the star-rating change in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces でポート `4321` が利用可能になったら **Open in Browser** を選択します。
3. ゲームカードに5段階の評価が表示されることを確認します。
4. Copilot に戻り、次のプロンプトで起動したサーバーを停止します。

   ```plaintext
   Stop the development server you started.
   ```

## 最初のプルリクエストを作成してマージする

機能ができたので、新しいコードをプロジェクトにマージするプルリクエスト (PR) を作成します。

1. 既定のエージェントに変更をコミットするよう依頼します。

   ```plaintext
   Commit the reviewed star-rating changes with an appropriate commit message.
   ```

2. `/pr create` を入力します。Copilot CLI は PR の作成時に既存のコミットをプッシュし、PR の URL を表示します。
3. Copilot CLI が表示した URL を、<kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら選択して PR を開きます。
4. 変更されたファイルとチェックをレビューします。
5. 問題がなければ **Merge pull request** を選択し、マージを確定します。
6. Codespace に戻り、`/exit` を入力して Copilot CLI を終了します。
7. ローカルの `main` を更新します。

   ```bash
   git checkout main
   git pull
   ```

## まとめと次のステップ

GitHub Copilot CLI を使って、最初の変更をリリースしました。このレッスンでは、次のことを行いました。

- 機能ブランチで Copilot の会話を始めました。
- ゲームカードに小さな変更を加えるようエージェントに依頼しました。
- `/diff` で変更をレビューしました。
- アプリを実行し、転送されたブラウザーで星評価を確認しました。
- 最初のプルリクエストを作成してマージしました。

次は、[フィルター機能の Issue から始め、Plan と Autopilot モードを使って][next-lesson]より大きな機能を作成します。

## リソース

- [GitHub Copilot CLI について][about-copilot-cli]
- [Copilot CLI コマンドリファレンス][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
