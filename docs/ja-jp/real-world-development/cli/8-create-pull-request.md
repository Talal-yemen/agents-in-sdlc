---
title: "レッスン 8 - 機能の PR を作成してマージする"
description: "フィルター機能、指示、スキルの更新、QA プロファイル、テストをまとめてレビューし、PR を作成して Agent Merge を使います。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

フィルター機能、指示の更新、スキルの更新、品質保証 (QA) プロファイル、テストはすべて1つのブランチに保存されています。これらをまとめてレビューし、プルリクエストを作成します。星評価のプルリクエスト (PR) は自分でマージしました。今回は **Agent Merge** にプロセスを任せます。

> [!NOTE]
> 通常は機能、指示の更新、スキルの更新、QA エージェントを複数の PR に分けます。このワークショップでは手順を簡潔にするため、フィルター機能と品質に関する作業を1つの会話とブランチにまとめ、1つの PR に含めました。

このレッスンでは、次のことを行います。

- Agent Merge と、マージまでの作業を自動化する方法を学びます。
- 機能全体の変更と検証の証拠を確認します。
- フィルター機能の PR を作成します。
- レビュー後に Agent Merge を有効にし、PR がマージされたことを確認します。

## シナリオ

フィルター機能のワークフローでは、Copilot を使って機能を計画、実装、検証しました。Tailspin Toys では、マージの許可を開発者が管理しながら、PR の残りの作業を自動化したいと考えています。

## Agent Merge とは

**Agent Merge** は、プルリクエストをマージするために必要な残りの作業を自動化します。有効にすると、Copilot は PR をブロックしている項目に取り組みます。たとえば、失敗した継続的インテグレーション (CI) チェックの修正、レビューコメントへの対応、必要な場合のリベースなどを行い、リポジトリで許可されている場合は GitHub の自動マージを有効にします。

これまでは **Merge pull request** を自分で選択していました。Agent Merge にその作業を任せられますが、有効にする前に変更をレビューし、マージしてよいことを確認してください。

## Agent Merge で PR を管理する

コードの作成が終わったら、変更をまとめてレビューし、PR を作成して、残りのプロセスを Agent Merge に任せます。

1. Codespace に戻ります。
2. `/agent` を入力してエージェントのダイアログを開きます。
3. 選択肢から **Default** を選び、<kbd>Enter</kbd> を押します。
4. `/pr create` コマンドで新しい PR を作成します。
5. `/pr agentmerge` を入力して Agent Merge を有効にします。
6. Copilot は PR の継続的インテグレーションを監視します。すべてのチェックが成功すると、マージを実行します。
7. Copilot が `PR #14 was squash-merged successfully.` のようなメッセージを表示したことを確認します。

> [!IMPORTANT]
> Agent Merge は、必須の承認、ブランチ保護、マージキュー、リポジトリ設定、権限の不足を回避しません。作業がブロックされた場合は、報告された理由を確認し、リポジトリで許可されている場合はレビュー済みの PR を手動でマージしてください。

## まとめと次のステップ

コード生成、テストと検証に加えて、プルリクエストのプロセスも自動化しました。このレッスンでは、次のことを行いました。

- Agent Merge と、マージまでの作業を自動化する方法を学びました。
- 機能全体の変更と検証の証拠を確認しました。
- フィルター機能の PR を作成しました。
- レビュー後に Agent Merge を有効にし、PR がマージされたことを確認しました。

次は、コンテキスト、モデル、共有、任意のクラウド委任に役立つ [Copilot CLI のスラッシュコマンドを確認します][next-lesson]。

## リソース

- [Copilot CLI でプルリクエストを管理する][manage-prs]
- [Copilot CLI コマンドリファレンス][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
