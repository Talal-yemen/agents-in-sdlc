---
title: "レッスン 10 - 振り返りと次のステップ"
description: "Copilot CLI のワークフロー、2つのプルリクエスト、再利用可能なカスタマイズ、参考資料を振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

GitHub Copilot CLI を使い、Tailspin Toys で一連のワークフローに取り組みました。次のことを行いました。

- Codespace を準備し、Copilot CLI をインストールし、プロジェクトを確認して、フィルター機能の Issue を見つけました。
- 星評価を追加し、転送されたブラウザーで結果をレビューして、最初のプルリクエスト (PR) を手動でマージしました。
- フィルター機能の Issue から始め、Plan モードで方針を決め、Autopilot モードで構築して、Interactive モードでレビューしました。
- カスタム指示でエージェントをガイドし、既存の `quality-checks` スキルをカスタマイズして、ユニットテスト、lint、型チェックを実行しました。
- Playwright Model Context Protocol (MCP) サーバーを追加し、実際のブラウザーでフィルター機能を確認しました。
- 品質保証 (QA) カスタムエージェントを作成して選択し、要件、カバレッジ、スキルの結果、ブラウザーでの確認内容を評価しました。
- フィルター機能全体をレビューし、Agent Merge を許可して PR をマージしました。
- コンテキスト、モデル、共有、任意のクラウド委任に使うスラッシュコマンドを確認しました。

## リリースしたもの

このワークショップでは、更新された `main` からそれぞれ別のブランチを作り、2つの PR を作成します。

1. **星評価:** ゲームカードに既存の `starRating` と、評価がないことを示すテキストを表示します。
2. **フィルター機能と品質ワークフロー:** フィルター機能を実装し、指示を更新して機能に適用し、`quality-checks` のレポートをカスタマイズし、QA プロファイルを作成して、関連するテストを追加します。

ワークショップを簡潔に進めるため、フィルター機能の計画から PR 作成まで同じ会話とブランチを使いました。この作業を1つの PR にまとめました。

## 検証の種類

自動テスト、自分で行うブラウザーでの確認、MCP 経由で Copilot が行うブラウザー操作など、複数の方法でコードを確認しました。`quality-checks` スキルはユニットテスト、lint、型チェックを実行し、新しい形式で報告しました。QA はこれらの結果に要件とテストカバレッジのレビューを加えました。

追加のテストは、実際のカバレッジ不足を補うものである必要があります。QA の実行で新しいテストが不要と判断される場合もあります。マージを許可する前に、コードと検証の証拠を確認し、変更後は関連する証拠を更新してください。

## ベストプラクティス

Copilot に与えるコンテキストやツールが作業の質を左右します。このワークショップでは、指示を更新し、スキルをカスタマイズし、QA プロファイルを作成して、MCP サーバーを設定しました。これらのカスタマイズを会話間で再利用し、チームのニーズの変化に合わせて調整してください。指示は標準を設定し、スキルは繰り返し実行できるタスクを説明し、カスタムエージェントは専門的な役割を定義し、MCP サーバーは外部ツールに接続します。エージェントの要約だけでなく、実際の変更やツールの結果を確認してください。

タスクに合った**モードとモデル**を選びます。実装前に方針を検討する場合は **Plan**、範囲を絞った変更を対話しながら進める場合は **Interactive**、範囲が明確なタスクを自律的に実行する場合は **Autopilot** を使います。定型的な編集には高速なモデルを、複雑な作業にはより高性能なモデルを選びます。

環境と同様に、コンテキストも重要です。何を作るか、なぜ必要か、どのように進めたいかを明確に伝えることで、出力は大きく変わります。

## さらに試す

基本的なワークフローは完了しました。Copilot CLI の機能をさらに確認する場合は、次のコマンドを試せます。

- `/review` でコードレビューエージェントに変更を分析させる
- `/rubber-duck` で問題を相談し、別の視点を得る
- `/fleet` で独立したサブタスクを並行して調整する
- `/worktree` で別のタスクを分離する
- `/delegate` で Copilot cloud agent にタスクを送信する

## 次のステップ

ツールのスキルを伸ばす最善の方法は使い続けることです。本番コード、趣味のコード、何年も頭の中にあった小さなアプリなど、さまざまな場面で活用してください。学んだことをチームと共有し、チームからも学びましょう。ドキュメントも確認してください。

ターミナルから Tailspin Toys を拡張する場合は、オプションの [Foundry Backer Concierge シリーズ][foundry]に進みます。他の環境も比較する場合は、[VS Code ワークショップ][vscode]、[GitHub Copilot app ワークショップ][app]、または [Copilot cloud agent ワークショップ][cloud]を確認してください。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンドリファレンス][cli-reference]
- [Copilot CLI をカスタマイズする][customize-cli]
- [Copilot CLI でプルリクエストを管理する][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
