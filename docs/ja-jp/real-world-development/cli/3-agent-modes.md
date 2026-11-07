---
title: "レッスン 3 - エージェントモード: Plan と Autopilot"
description: "Plan で方針を合意し、Autopilot で Issue に基づくフィルター機能を作成し、Interactive モードで結果をレビューします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

最初に小さな機能をプロジェクトに追加しました。より大きな変更には、さらにしっかりとしたプロセスが必要です。GitHub Copilot CLI は組織の既存の流れに合わせて作業できるため、適切な方法で必要な機能を構築できます。このレッスンから、典型的なエージェント主導の開発プロセスに沿って作業します。Issue から機能を作成し、コードが有効で期待どおりに動作することを確認して、最終的にプロジェクトにマージします。

> [!NOTE]
> 機能の作業を続ける間は、同じ会話とブランチを使います。通常はファイルの種類ごとに異なるブランチや PR を作りますが、このレッスンでは基本概念に集中するため手順を簡略化します。

このレッスンでは、次のことを行います。

- GitHub Issue から新しい Copilot の会話を始めます。
- Plan モードで要件を定義します。
- Autopilot モードで新機能を実装します。
- コードをレビューします。
- Codespaces から転送されたブラウザーで機能を手動で検証します。

引き続き、リポジトリの指示を更新し、既存の `quality-checks` スキルをカスタマイズし、MCP による検証を追加して QA エージェントを作成し、機能の PR を開きます。

## シナリオ

Tailspin Toys のカタログが増えているため、訪問者がゲームをカテゴリと publisher で絞り込めるようにします。バックログの Issue には機能が記載されていますが、複数カテゴリの組み合わせなど、コードを書く前に合意しておく必要のある詳細があります。Plan モードで決定し、Autopilot には範囲を明確にした実装を任せます。

## 背景

AI コーディングエージェントを開発に取り入れても、基本的な工程は変わりません。むしろ、より重要になります。多くの開発者は次のような流れで作業します。

1. 必要な作業を記載した Issue から始めます。
2. 実装する内容を計画します。
3. コードを作成してレビューします。
4. テストを実行してコードを検証します。
5. 新機能を手動で検証します。
6. プルリクエスト (PR) を作成します。
7. コードレビューが完了し、継続的インテグレーションが成功したら、変更をマージします。

> [!NOTE]
> 具体的な手順はチームや組織によって異なりますが、通常は上記の流れを基にしています。

標準的な手順に沿うことで、AI が生成したコードが要件を満たし、手作業で書いたコードと同じレビューと検証を受けるようにします。

## 会話モード

**会話モード**は、エージェントがどれだけ自律的に作業するかを制御します。<kbd>Shift</kbd>+<kbd>Tab</kbd> を押すと、モードを切り替えられます。

- **Interactive**: エージェントと一緒に作業します。エージェントは変更を提案し、次に進む前にユーザーの入力を待ちます。
- **Plan**: エージェントが最初に計画を作成します。プロジェクトファイルの編集はできません。
- **Autopilot**: エージェントが自律的にコードを書き、テストを実行し、タスクが完了するまで反復します。

最初に Plan モードで計画を確認し、次に Autopilot で実装します。

## Issue から始める

フィルター機能の作業を始める前に、Codespace とリポジトリ、ターミナルを準備します。

1. Codespace に戻ります。停止している場合は、再起動してから続けます。
2. 星評価の PR がマージ済みであることを確認します。
3. ターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押します。
4. `main` を更新し、フィルター機能のブランチを作成します。

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Copilot CLI を起動します。

   ```bash
   copilot --yolo
   ```

6. <kbd>Tab</kbd> を2回押して **Issues** タブを開きます。
7. <kbd>A</kbd> を押してすべての Issue を表示します。
8. 矢印キーを使って、**Allow users to filter games by category and publisher** というタイトルの Issue を選択します。
9. <kbd>C</kbd> を押してプロンプトに Issue を追加し、**Session** タブに戻ります。

プロンプトの先頭に `#7` (または同様の番号) が表示されます。`#` を使うと、GitHub 上の Issue やプルリクエスト (PR) を会話のコンテキストに追加できます。

## フィルター機能を計画する

計画すると、機能を実装したりタスクを Copilot に任せたりする前に、方針を決められます。複雑な作業では、時間をかけて計画することをお勧めします。Plan モードに切り替え、Copilot に計画を依頼します。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Plan モードに切り替えます。プロンプトの下にあるモード表示が **Plan** になっていることを確認します。
2. 直前の手順で追加した Issue の後ろに、次のプロンプトを入力します。

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot はプロジェクトを調査し、適切な方法を検討して計画を作成します。

3. フィルターの動作について質問された場合は、希望に合わせて回答します。正解は1つではありません。
4. 計画が完成したら、<kbd>Control</kbd>+<kbd>E</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) を押して計画を展開します。
5. スクロールして計画を確認します。
6. 自分の決定と合わない箇所があれば、Copilot に修正を依頼します。

## Autopilot を承認する

計画を作成してレビューしたら、Copilot に実装を依頼します。Autopilot では、Copilot がタスクを完了したと判断するまで問題に取り組みます。

1. **Accept plan and build on autopilot (recommended)** または、インストールしているバージョンにある同様の選択肢を選択します。
2. プロンプトの下にあるモード表示が **Autopilot** になっていることを確認します。
3. Copilot が計画に沿ってコードを生成し、テストを実行する様子を確認します。

> [!NOTE]
> 承認すると、すぐに実装が始まる場合があります。計画を先に確認してください。依存関係が不足していたり、ポートが競合していたりするという報告があれば、チェック完了とみなす前にセットアップの問題を解決します。

## 実装をレビューして検証する

コードを生成した後、他のコードと同じように、マージ前にレビューします。コードを確認してサイトを実行し、問題がないか検証します。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Interactive モードに切り替えます。モード表示に **Plan** や **Autopilot** が表示されていないことを確認します。
2. `/diff` を入力し、フィルター機能とテストを確認します。
3. Issue と、計画で決めた内容を基に実装を確認します。
4. レビューが終わったら <kbd>Esc</kbd> を押して差分画面を閉じます。
5. プロジェクトのチェック結果を確認し、失敗があれば Copilot に修正を依頼します。

## 新機能を試す

コードを確認したら、Codespaces のポート転送を使ってサイトを開きます。前と同じようにアプリを起動します。

1. 次のプロンプトを Copilot に送り、アプリの起動を依頼します。

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces でポート `4321` が利用可能になったら **Open in Browser** を選択します。
3. 計画で決定したカテゴリと publisher のフィルター、およびその組み合わせを試します。
4. リセットと結果が空の場合の動作が、Issue と決定した内容に合っていることを確認します。
5. Codespace に戻り、Copilot に起動した開発サーバーを停止するよう依頼します。

## まとめと次のステップ

さまざまな会話モードを使って、機能を構築してレビューしました。このレッスンでは、次のことを行いました。

- GitHub Issue から新しい Copilot の会話を始めました。
- Plan モードで要件を定義しました。
- Autopilot モードで新機能を実装しました。
- コードをレビューしました。
- Codespaces から転送されたブラウザーで機能を手動で検証しました。

次は、コード生成の指針を詳しく確認するため、[カスタム指示を使います][next-lesson]。

## リソース

- [GitHub Copilot CLI の Autopilot][autopilot]
- [Copilot CLI コマンドリファレンス][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
