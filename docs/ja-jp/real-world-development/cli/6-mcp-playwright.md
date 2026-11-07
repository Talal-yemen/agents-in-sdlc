---
title: "レッスン 6 - Playwright MCP で機能を検証する"
description: "Copilot CLI で Playwright MCP を確認または設定し、ブラウザーでフィルター機能を確認します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

すでに説明したように、コードを書く作業には、データや外部サービスとの連携、Copilot で使える追加の自動化機能も含まれます。MCP サーバーを使うと、Copilot CLI に組み込まれている機能を越えて、ツールやサービスを追加できます。

このレッスンでは、次のことを行います。

- Model Context Protocol (MCP) と、Copilot CLI での利用方法を学びます。
- Playwright MCP サーバーが利用できない場合は追加します。
- エージェントにブラウザーを操作させ、フィルター機能を確認します。

## シナリオ

ユニットテストやエンドツーエンドテストは重要ですが、UI の変更を検証するには、実際に UI を操作する必要があります。Copilot にユーザーと同じように Web サイトを使わせることで、変更が期待どおりに動作する確信を高め、作業をさらに自動化します。

## Model Context Protocol (MCP) とは

[Model Context Protocol (MCP)][mcp-blog-post] を使うと、AI エージェントが外部のツールやサービスと通信できます。最新情報にアクセスしたり、ユーザーに代わって操作を実行したりできます。

MCP サーバーは、AI エージェントと外部のツールやサービスをつなぎます。各 MCP サーバーが、AI エージェントから利用できるツールやリソースを提供します。

よく使われる MCP サーバーの例を次に示します。

- **[GitHub MCP Server][github-mcp]**: GitHub リポジトリ、Issue、プルリクエストを管理する API にアクセスできます。
- **[Playwright MCP Server][playwright-mcp-server]**: Playwright を使ったブラウザー自動化機能を提供します。

他にも、さまざまな MCP サーバーがあります。GitHub は、エコシステム内のサーバーを見つけて参加できる [MCP registry][mcp-registry] をホストしています。

> [!CAUTION]
> MCP サーバーも他の依存関係と同様に扱ってください。使用前にソースコードを確認し、公開元を検証して、セキュリティへの影響を検討します。

## Playwright MCP サーバーを追加する

Playwright MCP サーバーがまだ利用できない場合は、Copilot がユーザーと同じように Web サイトを操作できるように追加します。

> [!NOTE]
> Copilot CLI に Playwright MCP がすでに設定されている場合は、ここでのセットアップを省略して次のセクションに進みます。

1. Codespace に戻ります。
2. Copilot CLI で次のコマンドを入力して MCP サーバーの追加ダイアログを開きます。

   ```plaintext
   /mcp add
   ```

3. 名前として `playwright` を入力し、<kbd>Tab</kbd> を押します。
4. サーバーの種類として **STDIO** を確認して <kbd>Enter</kbd> を押し、続けて <kbd>Tab</kbd> を押します。
5. **Command** ダイアログに次のテキストを貼り付けます。

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. <kbd>Control</kbd>+<kbd>S</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) を押して、新しい MCP サーバーを保存します。
7. <kbd>Esc</kbd> を押して MCP ダイアログを閉じます。

## Playwright で機能を確認するよう Copilot に依頼する

これまでは、機能が期待どおりに動作することを手動で確認しました。今度は Copilot CLI で利用できる Playwright MCP サーバーを使います。

1. 次のプロンプトを使い、Playwright MCP サーバーで機能を検証するよう Copilot に依頼します。

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> 通常は MCP サーバーを使うよう Copilot に伝えなくても、自動的に判断します。ただし、使用するツールがわかっている場合はプロンプトに含めると、適切な方向を示せます。結果が一貫しやすくなり、トークンも節約できます。

2. Copilot がブラウザーで機能を確認するために行っている手順を確認します。
3. レポートを読み、すべて期待どおりに動作することを確認します。

Copilot はサーバーを起動し、Playwright で Web サイトを操作し、作業後にサーバーを停止して結果を報告します。

## まとめと次のステップ

Copilot CLI から Playwright MCP サーバーを使い、実際のブラウザーで機能を確認しました。このレッスンでは、次のことを行いました。

- Model Context Protocol (MCP) と、Copilot CLI での利用方法を学びました。
- Playwright MCP サーバーが利用できない場合に追加しました。
- エージェントにブラウザーを操作させ、フィルター機能を確認しました。

次は、スキルとブラウザーツールを専門的な役割としてまとめる [QA カスタムエージェントを作成します][next-lesson]。

## リソース

- [MCP とは何か、なぜ注目されているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [Copilot CLI に MCP サーバーを追加する][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
