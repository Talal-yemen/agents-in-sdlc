---
title: "レッスン 2 - Copilot エージェントモードで機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: カスタム指示][previous-lesson] · [次のレッスン: Playwright MCP で機能をテストする →][next-lesson]

アプリケーションの簡単な更新でも、通常は複数のファイルを変更し、テストを実行する必要があります。開発者は、関連ファイルを探し、変更を加え、テストを実行し、問題を修正し、見落としたファイルを見つけて再度更新します。作業は複雑になりがちです。

Copilot のエージェントモードを使うと、IDE 内でより自律的に作業できます。開発者と同じように既存のプロジェクト構成を調査し、必要なファイルを更新し、テストなどのタスクを実行して、見つかった問題を修正します。エージェントモードでサイトに機能を追加する方法を確認しましょう。

> [!NOTE]
> 名前は似ていますが、エージェントモードと cloud agent は異なる用途向けです。エージェントモードは IDE 内で動作するため、すぐにフィードバックを得て対話できます。Cloud agent はチームメンバーのように非同期で作業し、Issue やプルリクエストを通じてやりとりします。

このレッスンでは、次のことを学びます。

- Copilot エージェントモードがプロジェクトを調査し、関連ファイルを見つけ、複数のファイルをまとめて変更する方法
- UI とデータレイヤーにまたがる新機能を実装する方法
- Copilot エージェントモードが生成した変更とテストを、コードベースにマージする前にレビューする方法

## シナリオ

Tailspin Toys のゲーム一覧が増えてきたため、カテゴリと publisher で絞り込めるようにします。前のレッスンで publisher ヘルパーを追加しました。残りのデータレイヤー、UI、テストを Copilot エージェントモードで完成させます。

## Tailspin Toys の Web サイトを実行する

変更を加える前に、Tailspin Toys の Web サイトを開いて、現在の機能を確認します。

この Web サイトは、開発者向けテーマのボードゲームのクラウドファンディングプラットフォームです。ゲーム一覧と詳細を表示します。Astro アプリ1つで構成され、ビルド時に静的 HTML を生成します。ページは frontmatter 内で Drizzle ORM を使ってローカル SQLite データベースを直接参照します。個別のバックエンド API やクライアント側 UI フレームワークはありません。再利用可能なデータアクセスヘルパーは `src/lib/` にあり、インタラクティブな動作には標準 DOM API を使う小規模な Astro `<script>` を使用します。

### Web サイトを起動する

Web サイトを起動するために、`npm run dev` タスクを用意しています。GitHub Codespace で次の手順を実行します。

1. Codespace に戻ります。前のレッスンの後であれば、`main` にいるはずです。
2. Codespace 内で <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. フィルター機能用のブランチを作成して切り替えます。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 次のコマンドを実行して、Web サイトを起動します。

   ```bash
   npm run dev
   ```

   Astro の開発サーバーが起動すると、次のような URL が表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> `http://localhost:4321` を開くダイアログが表示された場合は、**x** を選択して閉じます。

5. ターミナルに表示された Astro サーバーのアドレス `http://localhost:4321` を、<kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら選択して、Web サイトを開きます。

> [!NOTE]
> Codespace のターミナルで localhost URL を選択すると、自動的に `https://<your-codespace-name>-4321.app.github.dev/` に転送されます。これは Web サーバーをホストする Codespace へのプライベートトンネルです。

### Web サイトを確認する

Web サイトを起動したら、機能を確認できます。

- **ホームページ**: タイトル、画像、説明を含むボードゲームの一覧を表示します。
- **ゲーム詳細ページ**: ゲームを選択すると、タイトル、説明、publisher、カテゴリなどの情報を表示します。

## Copilot でバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> 下の演習を始める前に Codespace に戻り、Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習ごとに異なります。必要な場合は、各手順で説明します。

Web サイトの初期実装は動作しますが、新しい機能を追加します。まずバックログを確認しましょう。テンプレートからプロジェクトを作成したときに、Issue のバックログも自動的に作成されています。GitHub Copilot に内容を表示するよう依頼します。

1. Chat ビューのエージェントのドロップダウンから **Agent** を選択します。**Agent** はファイルをまたぐ変更を計画して実装し、ターミナルコマンドを実行してツールを呼び出します。

   ![Chat ビューでエージェントを選択する画面](../../../_images/shared-chat-mode-selector.png)

2. 利用可能なモデルの一覧から **Claude Sonnet 4.5** を選択します。

> [!CAUTION]
> ワークショップの作成者が特定のモデルを推奨する意図はありません。このワークショップは Claude Sonnet 4.5 を使って作成したため、手順に含めています。モデルによって提案が比較的一貫し、演習を進めやすくなることを期待しています。ただし LLM は確率的に動作するため、手順に記載されたものと異なる提案が表示されることがあります。これは通常の動作です。

> [!NOTE]
> LLM の確率的な性質により、Copilot が別の MCP コマンドを使う場合がありますが、タスクは同様に完了できます。

3. 次のプロンプトを Copilot に送信して、バックログを確認します。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. すべての Issue を一覧表示するコマンドを実行するため **Continue** を選択します。
5. Copilot が生成した Issue の一覧を確認します。

Copilot がユーザーに最も役立つと考える順に、Issue を優先順位付けしたことを確認してください。

## 指示ファイルを確認する

Copilot にコードを生成させる前に、作業のコンテキストを伝えるために使う指示ファイルを確認します。[ユーザーインターフェイス (UI) 指示ファイル](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)には、Web サイトに機能を追加する方法が記載されています。

1. Codespace で `.github/instructions/ui.instructions.md` を開きます。
2. 機能を追加する方法に関するガイダンスを確認します。次の内容が含まれます。
   - アーキテクチャの概要
   - コンポーネントの設計、テスト容易性、アクセシビリティの原則
   - Astro、Tailwind CSS、Drizzle などの各ファイルに対応する指示へのリンク

> [!TIP]
> 指示ファイルから別の指示ファイルやプロジェクトのファイルを参照できます。パスは指示ファイルからの相対パスです。指示を再利用し、大きな指示を扱いやすいサイズに分割して、Copilot がコードを生成するときに参考にする例を指定できます。

## フィルター機能を実装する

フィルター機能を完成させるには、少なくとも次の3種類の変更が必要です。

- データレイヤー (`src/lib/`) にフィルター処理を追加または調整する
- フィルターの動作をテストするテストを追加または更新する
- ゲーム一覧ページにフィルター UI を追加する

さらに、コードベースにマージする前にテストが成功する必要があります。Copilot エージェントモードに作業を依頼しましょう。

1. Copilot の現在の会話を続けるか、**New Chat** を選択して新しい会話を始めます。
2. **Add Context**、**Instructions**、**ui** を選択し、UI 指示ファイルを追加します。

   ![UI 指示ファイルを選択する画面の例](../../../_images/ex3-select-instructions-file.png)

3. Chat ビューのエージェントのドロップダウンで、引き続き **Agent** が選択されていることを確認します。

   ![Chat ビューでエージェントを選択する画面](../../../_images/shared-chat-mode-selector.png)

4. モデルとして **Claude Sonnet 4.5** が選択されていることを確認します。
5. バックログにある関連 Issue を基に機能を実装するよう、次のプロンプトを送信します。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot がプロジェクトを調査し、機能に関連するファイルを見つける様子を確認します。データレイヤーのヘルパー、UI、テストを探した後、ファイルを変更してテストを実行します。

   ![Copilot がプロジェクトファイルを調査している画面](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot はプロジェクトの調査、ファイルの変更、テストの実行などを行います。タスクやコードベースの複雑さによっては数分かかります。処理中にコードエディターで **Keep** と **Undo** が表示される場合があります。作業中に選択する必要はありません。Copilot が作業を完了すると、すべての変更に **Keep** または **Undo** が表示されます。

7. Copilot に求められたら **Continue** を選択して、テストを実行します。

   ![Copilot Chat で、テストを実行してよいか確認するダイアログ](../../../_images/ex3-agent-mode-run-tests.png)

8. 作業中に待ち時間が発生したり、テストが失敗したりすることがあります。問題ありません。Copilot はタスクが完了し、エラーが見つからなくなるまで、コード生成とテストを繰り返します。

   ![Copilot エージェントモードでの Chat セッションの完了画面](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 生成されたコードに問題がないか確認します。

> [!CAUTION]
> Copilot などの AI ツールが生成したコードは、必ずレビューしてください。

10. Web サイトを開いたブラウザーに戻り、新しい機能を確認します。
11. 動作とコードをレビューしたら、Copilot Chat ウィンドウで **Keep** を選択します。

## まとめと次のステップ

GitHub Copilot エージェントモードで Tailspin Toys の Web サイトに機能を追加する方法を確認しました。

- Copilot エージェントモードを使って、UI とデータレイヤーにまたがる機能を実装しました。
- Copilot エージェントモードでプロジェクトを調査し、関連ファイルを見つけ、複数のファイルをまとめて変更しました。
- Copilot エージェントモードが生成した変更とテストをレビューしました。

次は、[Playwright MCP サーバーを使って機能をテストし][next-lesson]、プルリクエストを作成します。

### 追加の演習 — ページネーションを実装する

ゲーム一覧が増えると、ページネーションが必要になります。このレッスンで学んだことを使い、Copilot にページネーションの実装を依頼してください。プロンプトを作成する際は、次の点を検討します。

- 既存の指示ファイルを使うなど、現在のベストプラクティスに従う
- ユーザーがページサイズを選べるようにするか、固定にするかを決める
- 求める実装に必要なガイダンスを Copilot に伝える
- Copilot に変更を依頼し、コンテキストを追加しながら反復する。Copilot を使う際によくある進め方です。

## リソース

- [Copilot の Ask、Edit、Agent モード: 機能と使い分け][choose-mode]
- [VS Code のエージェントモード][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
