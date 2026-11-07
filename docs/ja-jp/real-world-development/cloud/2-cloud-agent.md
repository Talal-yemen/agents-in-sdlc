---
title: "レッスン 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタム指示][previous-lesson] |
|:--|

多くの組織では、技術的負債が課題になっています。未解決のセキュリティ問題、更新が必要なレガシーコード、実装する時間がなくバックログに残った機能リクエストなどがあります。GitHub Copilot cloud agent は、コードの更新や機能追加といった作業を自律的に実行できます。作業が完了すると、人間の開発者がレビューできるドラフト PR を作成します。これにより、時間のかかる作業を任せて開発を加速し、より大きな課題に集中できます。

Copilot cloud agent では、次のことを学びます。

- コード生成環境のカスタマイズ
- 安全に操作を実行する方法
- Issue の範囲を明確に定めることの重要性
- Copilot への Issue の割り当て

## シナリオ

Tailspin Toys には解消したい技術的負債があります。最初のサイトを開発した担当者が、ドキュメントを十分に作成していません。まず、アプリケーション内のすべてのエクスポートされた関数に TSDoc のドキュメントコメントを追加します。

また、デザインチームはゲームを見つけやすくしたいと考えています。各ゲームの詳細ページに、同じカテゴリに属する他のゲームを表示したいとのことです。まだ完成度の高い実装は必要なく、UX の受け入れテストに使えるものを求めています。この作業は現在ブロッカーですが、優先度がより高い Issue もあります。

どちらも、優先順位が下がりやすい作業の例です。Copilot cloud agent に割り当てれば、開発者が別の作業を進めている間に非同期で処理できます。後から Copilot の作業をレビューし、期待どおりか確認できます。

## GitHub Copilot cloud agent の概要

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) は、人間の開発者と同じようにバックグラウンドでタスクを実行します。たとえば、[GitHub Issue を Copilot に割り当てる][assign-issue]方法があります。割り当て後、Copilot は進捗を記録するドラフトプルリクエストを作成し、環境を準備して作業を開始します。作業中でも完了後でも、Copilot のセッションを確認できます。提案された解決策のレビュー準備ができると、プルリクエストで通知されます。

## 適切な範囲の指示が重要

GitHub Copilot に魔法はありません。考える作業を省略できるわけではありません。一見簡単な作業でも、詳細を確認すると複雑な場合があります。[Copilot cloud agent に依頼する作業の範囲を適切に定めてください][cloud-agent-best-practices]。AI ペアプログラマーとして扱い、段階的に作業し、学び、試し、必要に応じて調整します。生成 AI を加えても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム指示

前のレッスンでは、カスタム指示とその役割を確認しました。カスタム指示のレッスンを完了していれば `.github/copilot-instructions.md` の動作を確認済みです。まだの場合は、この機会に簡単に見てみましょう。Copilot cloud agent に作業を割り当てる前に、このリポジトリに用意された指示ファイルを読み取り専用で確認して、後でその影響を探せるようにします。

リポジトリの GitHub Web UI、または起動済みの Codespace で次のファイルを開きます。

- `.github/copilot-instructions.md` — **Code standards** セクションを確認します。特に TypeScript の規約と TSDoc のドキュメントコメントに関する要件を見ます。
- `.github/instructions/unit-tests.instructions.md` — `applyTo` フロントマターを確認します。この指示は `**/*.test.ts` ファイルに適用されます。

次のセクションで `Code lacks documentation` Issue を cloud agent に割り当てたら、作成されるプルリクエストに TSDoc コメント、TypeScript の規約、コメントヘッダーが反映されているか確認してください。後のレッスンで PR をレビューするときにも、改めて説明します。

## Copilot cloud agent の開発環境を設定する

コードの作成では、開発環境やセットアップスクリプトが必要な場合があります。Copilot にタスクを割り当てる場合も同様です。cloud agent はソフトウェアエンジニアのように作業します。

Cloud agent は [GitHub Actions][github-actions] を使って作業環境を用意します。作業開始前に実行する[セットアップワークフロー][setup-workflow]を `.github/workflows/copilot-setup-steps.yml` に設定すると、必要な開発ツールや依存関係を使えるようになります。ワークショップの流れを円滑に進めるため、このプロジェクトではあらかじめ設定されています。単一の Astro アプリであるプロジェクトを対象に、Node.js、プロジェクトの依存関係、エンドツーエンドテスト用のブラウザーバイナリ、マイグレーションと初期データ投入済みの SQLite データベースを利用できるようにします。

```yaml
name: "Copilot Setup Steps"

# リポジトリの "Actions" タブからセットアップ手順をテストできます
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # この手順に必要な範囲で、できるだけ最小限の権限を設定します。Copilot は操作用の独自トークンを受け取ります。
    permissions:
      # 依存関係のインストールなど、セットアップ手順の一部としてリポジトリをクローンする場合は `contents: read` 権限が必要です。セットアップ手順でクローンしない場合は、手順の完了後に Copilot が自動的にクローンします。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # フロントエンド / アプリのセットアップ - Node.js (アプリ全体が Astro + Drizzle/libSQL になりました)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # ビルドとテストでデータを使えるように、ローカルの SQLite データベースをマイグレーションして初期データを登録します。
      - name: Set up the database
        run: npm run db:setup
```

一般的な GitHub Actions ワークフローと同じ構成ですが、次の点を確認してください。

- `copilot-setup-steps` という1つのジョブがあります。このジョブは、Copilot がプルリクエストの作業を始める前に GitHub Actions で実行されます。
- `workflow_dispatch` トリガーを使うと、リポジトリの **Actions** タブからワークフローを手動実行できます。Copilot が実行するのを待たずに、正常に動作するか確認できます。

## ドキュメントを追加する

ドキュメントの重要性は広く理解されていますが、情報が古いまま、あるいは全くないプロジェクトもあります。こうした技術的負債を放置すると、開発の効率が下がり、コードベースの保守や新しいメンバーの受け入れが難しくなります。Copilot はドキュメントの作成を支援できるため、この作業は Copilot cloud agent に依頼するのに適しています。Copilot がバックグラウンドでドキュメントを作成し、後のレッスンでその結果をレビューします。

1. 新しいブラウザータブで github.com のリポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue のダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次のテキストを入力します。

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

7. **Create** を選択して Issue を作成します。
8. 右側にある **Assign to Copilot** を選択して、割り当てダイアログを開きます。

   ![Copilot を Issue に割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。
10. **Pull Requests** タブを選択します。
11. `[WIP]: Code lacks documentation` のようなタイトルの新しいプルリクエスト (PR) を開きます。一覧に新しい PR が表示されない場合は、少し待ってからブラウザーを更新してください。
12. 数分後、Copilot が To Do リストを作成したことを確認します。

> [!NOTE]
> Copilot の To Do リストが PR に表示されるまで数分かかる場合があります。Copilot は先ほど紹介したワークフローを実行して環境を作り、プロジェクトを分析して、作業方法を検討しています。

13. リストと、Copilot が実行するタスクを確認します。
14. プルリクエストのタイムラインを下にスクロールし、Copilot が Issue の作業を開始したことを示す更新を確認します。
15. **View session** ボタンを選択します。

   ![Copilot セッションの表示画面](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 更新されたインジケーターを表示するには、ウィンドウの再読み込みが必要な場合があります。

16. ライブセッションをスクロールして、Copilot の作業を確認します。コードを調査して現状を把握し、計画を考え、コードを作成する様子がわかります。

この作業には数分かかります。Copilot cloud agent の大きな利点は、タスクを非同期で実行し、その間に別の作業を進められることです。この利点を活かすため、もう1つの作業を cloud agent に依頼してから、アプリケーションへの機能追加に取り組みます。

## ゲーム詳細ページに関連ゲームを追加する

GitHub Copilot cloud agent の利点の1つは、作業を分担できることです。デザインチーム向けの関連ゲームの表示は、時間がかからない作業かもしれません。それでも cloud agent に任せれば、他の作業に時間を使えます。さっそく Copilot cloud agent に依頼しましょう。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue のダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次のテキストを入力します。

   ```markdown
   We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

7. **Create** を選択して Issue を作成します。
8. 右側にある **Assign to Copilot** を選択して、割り当てダイアログを開きます。

   ![Copilot を Issue に割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

Issue の最初のコメントに 👀 が表示され、Copilot が作業を開始したことを示します。

![Copilot が Issue の作業中であることを目の絵文字で示す画面](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は2つ目の依頼にも取り組んでいます。cloud agent はソフトウェアエンジニアのように作業するので、積極的に監視する必要はなく、完了後にレビューできます。次はカスタムエージェントの作成と使用を確認します。

## まとめと次のステップ

[GitHub Copilot cloud agent][copilot-agents] を使うと、Issue を割り当てて非同期に作業を進められます。技術的負債の解消、新機能の作成、別のフレームワークへのコード移行などに利用できます。

このレッスンでは、次のことを学びました。

- コード生成環境のカスタマイズ
- 安全に操作を実行する方法
- Issue の範囲を明確に定めることの重要性
- Copilot への Issue の割り当て

cloud agent がバックグラウンドで作業している間に、カスタムエージェントの作成と使用に取り組みます。[Copilot cloud agent は MCP サーバーも利用できます][cloud-agent-mcp]。また、前のレッスンで確認したカスタム指示にも対応しています。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップワークフローのベストプラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタムエージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
