---
title: "レッスン 9 - GitHub Copilot CLI のスラッシュコマンド"
description: "コンテキストの管理、モデルの選択、セッションの共有、任意のクラウド委任に使うスラッシュコマンドを確認します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

多くの CLI ツールと同様に、GitHub Copilot CLI には対話に使うスラッシュコマンドが多数用意されています。高度な機能や内部の情報を確認したり、追加設定を変更したりできます。これまで `/diff`、`/mcp`、`/skills`、`/agent`、`/pr` などを使いました。ここでは、他の便利なコマンドを確認します。

このレッスンでは、次のことを行います。

- `/context` と `/compact` を使い、Copilot が会話のコンテキストを管理する方法を確認します。
- `/model` を使い、利用できるモデルを確認します。
- `/share` でセッションをエクスポート、共有する方法を学びます。
- 並行作業、worktree、cloud agent への委任に使うオプションのコマンドを確認します。

## シナリオ

Copilot CLI の基本的なワークフローを終えました。ここでは、コンテキストの管理、モデルの切り替え、セッションの共有、[Copilot cloud agent][about-cloud-agent]への委任など、追加機能を確認します。

## Copilot CLI のコンテキストを確認する

大規模または複雑なタスクに取り組むと、モデルの最大コンテキストウィンドウに達することがあります。Copilot CLI は必要に応じて会話を自動的に圧縮します。スラッシュコマンドを使って、コンテキストを確認したり、自分で圧縮したりすることもできます。

1. Codespace に戻り、Copilot CLI が起動していない場合はリポジトリルートから起動します。
2. 次のコマンドを入力します。

   ```plaintext
   /context
   ```

3. モデル、現在のトークン使用量、システム指示、ツール、メッセージ、空き領域にコンテキストがどのように分けられているかを確認します。
4. 会話を圧縮します。

   ```plaintext
   /compact
   ```

5. もう一度 `/context` を入力し、結果を比較します。会話が短い場合は、大きな違いがないこともあります。

> [!NOTE]
> Copilot CLI は、コンテキストウィンドウが埋まると自動的に圧縮します。圧縮するタイミングを選びたい場合は `/compact` を使います。関係のない別タスクを始める場合は、`/clear` または `/new` で会話を新しくします。

## モデルを選ぶ

モデルごとに得意分野が異なり、開発者によって好みも異なります。Copilot CLI では、使用するモデルを一覧表示して選択できます。

1. 次のコマンドを入力します。

   ```plaintext
   /model
   ```

2. 利用可能なモデルと使用状況に関する情報を確認します。
3. 現在のモデルを維持するか、別のモデルを選ぶか、<kbd>Esc</kbd> を押して一覧を閉じます。

## セッションを共有する

チームで作業し、学んだことを共有すると、AI ツールの活用を改善できます。`/share` コマンドを使うと、セッションを Markdown または HTML ファイルにエクスポートしたり、共有リンクを作成したり、GitHub gist を公開したりできます。

1. `/help` を入力し、インストール済みのバージョンにある `/share` のオプションを確認します。
2. セッションを共有する場合は、用途に合った出力先を選びます。たとえば `/share file` はローカルの Markdown ファイルにエクスポートします。
3. エクスポートした内容を、誰かに送信したり公開したりする前に確認します。セッションのエクスポートには、プロンプト、応答、プロジェクトの詳細が含まれる場合があります。

リンクや gist の公開は任意です。チームが共有する意図のないリポジトリや会話の内容を公開しないでください。

## オプション: 作業の並列化や委任

このワークショップの主要な手順は完了しました。Copilot CLI には、大きなタスクに使える次のコマンドもあります。

- `/fleet` は、独立したサブタスクを複数のサブエージェントに分割し、並行実行できます。
- `/worktree` は、別のタスク用に分離された Git worktree を作成できます。
- `/delegate` は、タスクを Copilot cloud agent に送って非同期で処理します。プルリクエストが作成される場合があります。

worktree やリモート作業が追加される場合があるため、これらのコマンドは任意です。新しく範囲を明確にしたタスクから始め、通常のワークフローで結果をレビューしてください。非同期エージェントの作業を詳しく学ぶ場合は、[cloud agent ワークショップ][cloud-workshop]に進みます。

## まとめと次のステップ

Copilot CLI のスラッシュコマンドを使うと、設定を変更し、セッションを共有し、内部で起きていることを確認できます。このレッスンでは、次のことを行いました。

- `/context` と `/compact` を使い、Copilot が会話のコンテキストを管理する方法を確認しました。
- `/model` を使い、利用できるモデルを確認しました。
- `/share` でセッションをエクスポート、共有する方法を学びました。
- 並行作業、worktree、cloud agent への委任に使うオプションのコマンドを確認しました。

Copilot CLI には、他にも多くのスラッシュコマンドがあります。次は、[学んだことと今後の手順を確認します][next-lesson]。

## リソース

- [Copilot CLI コマンドリファレンス][cli-reference]
- [Copilot CLI のコンテキスト管理][context-management]
- [Copilot cloud agent について][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
