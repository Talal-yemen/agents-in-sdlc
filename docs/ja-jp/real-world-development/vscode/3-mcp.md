---
title: "レッスン 3 - Playwright MCP サーバーで機能をテストする"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: エージェントモード][previous-lesson] · [次のレッスン: カスタムエージェント →][next-lesson]

エージェントモードでフィルター機能を作成しました。プルリクエストを作成する前に、ブラウザーで実際に動作することを確認しましょう。自分でアプリを操作する代わりに、**Playwright MCP サーバー**を接続し、Copilot に実際のブラウザーを使って機能をテストさせます。その後、ブランチを公開し、PR を作成します。

このレッスンでは、次のことを行います。

- Model Context Protocol (MCP) と、MCP サーバーで Copilot にツールを追加する方法を学びます。
- Playwright MCP サーバーをワークスペースに追加します。
- ブラウザーで機能を手動テストするよう Copilot に依頼します。
- ブランチを公開し、フィルター機能のプルリクエストを作成します。

## Model Context Protocol (MCP) とは

エージェントモードは、エディターの外部にあるツールを使えると、さらに便利になります。Model Context Protocol (MCP) は、Copilot が外部のツールやサービスと通信するための標準です。

![エージェントモードがコンテキスト、LLM、ツールと連携する様子を示す図。MCP サーバーと VS Code 拡張機能が提供するツールを含む](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) を使うと、AI エージェントが外部のツールやサービスと通信できます。MCP によって、リソースを使って最新情報にアクセスしたり、ツールを使ってユーザーに代わって操作を実行したりできます。

MCP サーバーは、AI エージェントと外部のツールやサービスをつなぎます。既存の API や npm パッケージのようなローカルツールとの通信を管理します。各 MCP サーバーが、AI エージェントから利用できるツールやリソースを提供します。

よく使われる MCP サーバーの例を次に示します。

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: GitHub リポジトリを管理する API にアクセスできます。リポジトリの作成や更新、Issue やプルリクエストの管理などを行います。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: Playwright を使ったブラウザー自動化機能を提供します。Web ページへの移動、フォームへの入力、ボタンの選択などを行います。

他にも、さまざまなツールやリソースを利用できる MCP サーバーがあります。GitHub は、エコシステム内の MCP サーバーを見つけて参加できる [MCP registry](https://github.com/mcp) をホストしています。

> [!CAUTION]
> セキュリティの観点から、MCP サーバーも他の依存関係と同様に扱ってください。使用前にソースコードを確認し、公開元を検証して、セキュリティへの影響を検討します。信頼できる MCP サーバーだけを使い、機密情報や操作へのアクセスを許可する際は注意してください。

## MCP の設定を確認する

`.vscode/mcp.json` ファイルで、この VS Code ワークスペースで利用する MCP サーバーを設定します。

1. Codespace で `.vscode/mcp.json` を開きます。
2. すでに `github` サーバーが設定されていることを確認します。

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       }
     }
   }
   ```

`github` の設定はプロジェクトのテンプレートに含まれています。そのため、前のレッスンで Copilot が Issue のバックログを読み取れました。この設定では[リモート GitHub MCP サーバー][remote-github-mcp-server]を使います。ローカルにインストールする必要はありません。VS Code は HTTP で接続し、Copilot がサーバーのツールを初めて使うときに GitHub で認証します。

## Playwright MCP サーバーを追加する

次に、2つ目のサーバーを追加します。[Playwright MCP サーバー][playwright-mcp-server]を使うと、Copilot がブラウザーを操作できます。これを使って機能をテストします。

1. `.vscode/mcp.json` の `github` と並べて `playwright` を追加し、ファイルを次のようにします。

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       },
       "playwright": {
         "command": "npx",
         "args": ["@playwright/mcp@latest", "--headless"]
       }
     }
   }
   ```

2. ファイルを保存します。

リモートの `github` サーバーと異なり、`playwright` は**ローカル**サーバーです。VS Code は `npx @playwright/mcp@latest` を実行してサーバーを起動します。`--headless` フラグは Playwright を画面のないモードで実行します。デスクトップ画面がない Codespace では、この設定が必要です。

> [!NOTE]
> Tailspin Toys はエンドツーエンドテストで Playwright を使うため、通常は Playwright に必要なブラウザーがすでにインストールされています。Copilot がブラウザーが見つからないと報告した場合は、`npx playwright install chromium` を実行してから再試行します。

## サーバーを起動して信頼する

VS Code は MCP サーバーを**必要に応じて起動**します。Copilot がそのサーバーのツールを初めて使うときにサーバーを起動し、信頼するかを確認します。

1. `.vscode/mcp.json` の `playwright` エントリの上に **Start** 操作が表示されます。選択してサーバーを起動し、設定が正しいことを確認します。
2. サーバーを信頼するかどうか尋ねられたら、設定を確認して信頼することを選択し、サーバーを起動します。

手動で起動する必要はありません。ここで起動しない場合は、次の手順で Copilot がツールを初めて使うときに VS Code が起動し、信頼を確認します。

## フィルター機能をテストする

[Playwright MCP サーバー][playwright-mcp-server]を使うと、Copilot が実際のブラウザーを操作できます。ユーザーがアプリを操作して確認する代わりに、エージェントがページを開き、移動し、フィルターを適用し、結果を読み取って報告します。会話を離れずに機能を確認できます。

Playwright MCP サーバーは、スクリーンショットではなくページの[アクセシビリティツリー][playwright-mcp-server]を使います。そのため、エージェントはボタン、リンク、リスト項目などの構造化されたラベル付き要素を基に判断します。これは支援技術と同じような方法です。つまり、簡単な機能テストをアクセシビリティの基本確認にも利用できます。

サーバーを接続し、アプリケーションを起動した状態で、Copilot にフィルター機能をテストするよう依頼します。

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot は Playwright MCP サーバーを通じてブラウザーを起動し、各手順を実行して結果を報告します。Issue の受け入れ条件と照らし合わせます。問題があれば、追加の質問をするか、プルリクエストを作成する前にコードの修正を依頼します。

> [!NOTE]
> テストするには、アプリを `http://localhost:4321` で実行しておく必要があります。開発サーバーを停止した場合は、プロンプトを送る前にもう一度起動してください。Copilot が Playwright MCP サーバーを初めて使うときに、ブラウザーのダウンロードが必要な場合があります。ブラウザーが見つからないと報告されたら、`npx playwright install chromium` を実行してから再試行します。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## ブランチを公開してプルリクエストを作成する

機能が動作することを確認できたら、チームのレビュー用にプルリクエスト (PR) を作成します。まず `filtering-vscode` ブランチを公開します。

1. Codespace で **Source Control** パネルを開き、Copilot が変更したファイルを確認します。
2. **+** アイコンを選択して変更をステージします。
3. **Sparkle** ボタンを選択し、コミットメッセージを生成します。

   ![変更されたファイルを表示する Source Control パネルのスクリーンショット](../../../_images/ex3-source-control-changes.png)

4. **Publish** を選択して、ブランチをリポジトリにプッシュします。

## プルリクエストを作成する

github.com や GitHub コマンドラインインターフェイス (CLI) など、プルリクエストを作成する方法はいくつかあります。ここでは、GitHub Copilot に PR を作成してもらいます。関連する Issue を見つけ、PR と関連付けられます。

1. Copilot Chat パネルを開き、**New Chat** を選択して新しいセッションを始めます。
2. GitHub ツールを使用できるように、エージェントのドロップダウンで **Agent** が選択されていることを確認します。
3. 次のプロンプトで PR の作成を依頼します。

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. 情報収集や操作の実行を Copilot に許可するため、必要に応じて **Continue** を選択します。GitHub ツールを初めて使うときは、GitHub への認証を求められることがあります。画面の指示に従って許可します。
5. Copilot が Issue を検索し、適切なものを見つけて PR を作成する様子を確認します。
6. Copilot が作成したリンクを選択してプルリクエストをレビューします。ただし、まだマージしないでください。

## まとめと次のステップ

このレッスンでは、次のことを行いました。

- Model Context Protocol (MCP) と、MCP サーバーで Copilot にツールを追加する方法を学びました。
- Playwright MCP サーバーをワークスペースに追加しました。
- Playwright MCP サーバーを使って、リリース前にブラウザーで機能をテストしました。
- ブランチを公開し、フィルター機能のプルリクエストを作成しました。

MCP サーバーを使って機能をテストしました。Copilot を拡張する方法は MCP だけではありません。次は、アクセシビリティレビューなどの作業を効率化する[カスタムエージェントを作成します][next-lesson]。

## リソース

- [MCP とは何か、なぜ注目されているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP Registry][mcp-registry]
- [VS Code の MCP サーバー][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
