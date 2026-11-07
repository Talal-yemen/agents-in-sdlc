---
title: "レッスン 5 - quality-checks スキルをカスタマイズして使う"
description: "既存の quality-checks スキルを確認し、レポート形式をカスタマイズして、フィルター機能を検証するために使います。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

コードを書くことは、コードを書く作業だけではありません。手動で動作を確認し、指示ファイルで標準に従うようにしました。テストや lint、継続的インテグレーション (CI) の他の作業はどうでしょうか。

このような作業には、**エージェントスキル**が適しています。スキルを使うと、これらの操作を正しく実行する方法を Copilot に伝えられます。

このレッスンでは、次のことを行います。

- 既存の `quality-checks` スキルを確認します。
- 結果の表示形式をカスタマイズします。
- スキルを再読み込みして実行します。

## シナリオ

Tailspin Toys は、ユニットテスト、lint、型チェックに `quality-checks` スキルを使っています。チームでは、結果を読みやすくするため、レポート形式を改善したいと考えています。

## 指示、スクリプト、リソース

エージェントスキルは、エージェントが必要に応じて読み込む、再利用可能なタスク指示、実行可能なスクリプト、補助リソースをまとめたものです。基本的には、スキル名と同じフォルダー内に `SKILL.md` という Markdown ファイルを置きます。ファイルにはスキルの名前と説明を定義するフロントマター、役割の概要、呼び出すべき状況を示すガイダンスを記載します。スキルのフォルダーには、スクリプトや他のリソースを含むサブフォルダーを追加できます。

> [!NOTE]
> スキルに追加のフォルダーやファイルは必須ではありません。Tailspin Toys の `quality-checks` スキルは、プロジェクトにある既存のコマンドを使うため、`SKILL.md` だけで構成されています。

スキルは、チームで共有して再利用できるリポジトリアセットとしてプロジェクトの `.github/skills` に配置するか、ユーザー用の `~/.copilot/skills` に配置します。

## スキルを確認する

Tailspin Toys チームがユニットテスト、lint、型チェックに使う `quality-checks` スキルを確認します。

1. Codespace に戻ります。Codespaces のエディターで `.github/skills/quality-checks/SKILL.md` を開きます。
2. 先頭にある `name` と `description` を読みます。説明によって、Copilot がスキルを呼び出すタイミングを判断しやすくなります。
3. 指示を読み、Copilot にテストと lint を実行させる方法を確認します。
4. まだ **Results output formatting** セクションがないことに注目します。

## 変更前にスキルを実行する

Copilot CLI からスキルを直接呼び出すか、自然言語で依頼できます。スキルに3種類のチェックを実行するよう依頼します。

1. Interactive モードでフィルター機能の会話に戻ります。
2. 次のプロンプトを送信します。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 最後に表示されるレポートを確認します。

## レポートをカスタマイズする

実行したチェック、成功したかどうか、ツールが報告した内容がわかる、より良いレポートが必要です。スキルを更新してその形式で報告させます。

1. `.github/skills/quality-checks/SKILL.md` に戻ります。
2. ファイルの最後に次のセクションを追加します。

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. ファイルは自動的に保存されます。

## 更新したスキルを実行する

Copilot CLI では、会話を再起動せずに編集したスキルを再読み込みできます。更新したスキルを実行しましょう。

1. 次のコマンドを入力します。

   ```plaintext
   /skills reload
   ```

2. 先ほどと同じプロンプトを送信します。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 最後に表示されたレポートを確認し、最初の結果と比較します。

## まとめと次のステップ

既存のエージェントスキルをカスタマイズして使用しました。このレッスンでは、次のことを行いました。

- ユニットテスト、lint、型チェック用の `quality-checks` スキルを確認しました。
- 結果の形式をカスタマイズしました。
- スキルを再読み込みして実行しました。

この変更は、フィルター機能の PR に含めます。次は、[Playwright MCP サーバーを使って Copilot がサイトを直接操作できるようにします][next-lesson]。

## その他のスキルの例

次のコミュニティの例は、追加の作業ではなく参考資料です。

- [Agent Skills 仕様][skill-spec]
- [コントリビューションのワークフロー: `make-repo-contribution`][contribution-example]
- [要件文書: `prd`][prd-example]
- [図とエクスポートスクリプト: `drawio`][drawio-example]
- [ブラウザーテスト: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
