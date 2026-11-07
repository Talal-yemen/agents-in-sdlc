---
title: "レッスン 4 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

これまでのレッスンで、Copilot cloud agent にユーザー体験の改善や機能追加に関する3つのタスクを依頼しました。cloud agent は非同期かつ自律的に作業しますが、タスクを監視することも重要です。

GitHub.com の[エージェントページ][agents-page]など、cloud agent に割り当てたタスクを管理するためのツールがいくつかあります。この Mission control では、オープンなプルリクエスト (PR) があるすべてのエージェントタスクを確認できます。実行内容を調べたり、進行中のセッションに追加の指示を伝えたりできます。

このレッスンでは、次のことを行います。

- エージェントページで cloud agent のタスクを監視します。
- 進行中のセッションを調整し、機能の追加を依頼します。

## シナリオ

ハイコントラストモードの作成をエージェントに依頼した後、チームはライトモードも追加するよい機会だと気付きました。サイトのスタイルと切り替え機能を変更しているため、同じ作業にライトモードも含めるのが合理的です。ハイコントラストモードに加えてライトモードも実装するよう、エージェントの作業を調整します。

## Copilot cloud agent のタスクを確認する

Copilot cloud agent に割り当てたタスクの現在の状態を確認します。

1. [Copilot agents ページ](https://github.com/copilot/agents)を開きます。
2. メインペインと左ペインの両方にタスクが表示されます。ドキュメントの更新、ゲームを変更する API の生成、Web サイトへのハイコントラストモードの追加など、Copilot に割り当てたタスクを確認します。
3. 実行中のタスクを1つ選択し、Copilot が実行した手順を確認します。次のような作業が表示されます。
   - リポジトリのチェックアウト
   - Copilot の実行環境の作成
   - MCP サーバーの設定
   - タスクを完了するための各種作業

> [!NOTE]
> 表示される手順は、Copilot の作業状況や実施方法によって異なります。

4. 右側に表示されるプルリクエスト (PR) ペインも確認します。ここでは、PR や変更されたファイルを確認できます。

## Cloud agent に作業を調整する指示を送る

アクティブなタスクを確認できたら、cloud agent がハイコントラストモードを作成している間にライトモードの切り替えも追加するよう依頼します。

1. ハイコントラストモードを追加するセッションを選択します。タイトルは、Copilot が付けた名前や作業状況によって異なります。

   ![Mission control でアクセシビリティのセッションを表示している画面](../../../_images/ex6-accessibility-session.png)

2. セットアップが完了し、作業が始まったことを示すメッセージが表示されるまで、数分間セッションを確認します。
3. **Steer active session while Copilot is working** ダイアログに次のプロンプトを入力します。

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![Agents ページで、Copilot の作業中にアクティブなセッションを調整するダイアログを表示したスクリーンショット](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot がプロンプトを受け取り、作業計画に反映したことを確認します。

## Copilot の作業を待つ

Copilot は、更新されたタスクに取り組みます。プロンプトを送信した時点で実行中の手順を完了した後、新しい依頼を作業に取り込みます。

前と同じように、完了まで数分かかります。その間に、これまでに学んだことを振り返りましょう。

## まとめと次のステップ

このレッスンでは、GitHub Copilot cloud agent タスクの監視とガイドを行う中心的な場所、Copilot agents ページを確認しました。Mission control では、アクティブなタスクや完了したタスクを追跡し、実行中の作業をレビューし、進行中のタスクの範囲を調整したり追加の指示を伝えたりできます。

このレッスンでは、次のことを学びました。

- Mission control とエージェントページで cloud agent のタスクを監視する
- 進行中のセッションに追加機能を依頼する

Copilot がアクセシビリティ機能を実装している間に、[Copilot が作成したプルリクエストを改善します][next-lesson]。

## リソース

- [Copilot agents ページ][agents-page]
- [カスタムエージェントについて][custom-agents]

---

| [← 前のレッスン: カスタムエージェント][previous-lesson] | [次のレッスン: Copilot の作業を反復改善する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
