---
title: "レッスン 5: GitHub Copilot の作業を反復改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

## 作業をレビューする

このハーネスでは、ユーザー体験の改善や機能追加を目的に、GitHub Copilot にいくつかの作業を依頼しました。コードへのドキュメント追加、デザインチームが改善できる関連ゲーム機能の作成、ハイコントラストとライトモードの切り替えを含むアクセシビリティ機能の実装を行いました。コード変更を確認し、必要に応じて Copilot にフィードバックを伝えて改善してみましょう。

### シナリオ

生成 AI を利用しても、ソフトウェア設計と DevOps の基本は変わりません。生成されたコードをレビューし、通常の DevOps プロセスに沿って作業します。チームメンバーにもレビューを依頼する前に、ドキュメント、関連ゲーム機能、アクセシビリティ機能について GitHub Copilot が提案した内容を確認します。

## GitHub Copilot cloud agent のセキュリティ

Copilot cloud agent は非同期に、監督を受けずに作業するため、安全性を保つ仕組みが用意されています。

- Copilot はリポジトリに読み取りアクセスでき、書き込みできるのは作業に使用するブランチ **のみ** です。
- Cloud agent は GitHub Actions で実行され、作業用の独立した一時環境を作成します。
- GitHub Actions ワークフローを実行するには、人間による承認が必要です。
- [外部リソースへのアクセスは、既定では制限されています](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)。MCP サーバーへのアクセスも制限されます。

## 生成されたドキュメントをレビューする

最初に、GitHub Copilot cloud agent が作成した1つ目のプルリクエスト (PR)、コードへのドキュメント追加をレビューします。GitHub.com の標準の PR 画面を使用します。

> [!NOTE]
> PR に、GitHub Copilot がファイアウォールでブロックされたという警告が表示されることがあります。Copilot は MCP サーバーなどの外部リソースへのアクセスを既定で制限されているため、これは**想定される動作**です。必要であれば、[Copilot cloud agent のファイアウォールをカスタマイズまたは無効化][agent-firewall]できます。

1. github.com のリポジトリに戻ります。
2. **Pull Requests** を選択して PR の一覧を開きます。
3. **Add missing documentation** などのタイトルの PR を開きます。

> [!NOTE]
> Copilot がまだ作業中の場合、プルリクエストのタイトルに **[WIP]** が付きます。その場合は、Copilot が作業を完了するまで待ちます。数分かかることがあります。その間に休憩したり、これまでに学んだことを振り返ったりしてください。

4. PR の準備ができたら **Files changed** タブを選択し、変更をレビューします。

   ![Files changed タブ](../../../_images/shared-pr-files-changed.png)

5. TSDoc のドキュメントコメントなど、追加されたドキュメントを確認します。実際の変更は異なる場合があります。レビュー時は、TSDoc コメント、TypeScript の規約、コメントヘッダーに注目してください。これらはレッスン 2 の最初に確認したカスタム指示ファイルによるものです。
6. 変更を確認したら **Conversation** タブに戻り、下にスクロールします。
7. ワークフローの承認待ちを示すインジケーターがあるか確認します。
8. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. PR のチェック欄にワークフローが登録されたことを確認します。問題がなければ、単一の Astro アプリに対するプロジェクトのチェックが成功します。完了まで数分かかる場合があります。

## GitHub Copilot に変更を依頼する

プルリクエストで Copilot とやりとりする方法は一方向ではありません。チームメンバーと同じように、PR やコードへのインラインコメントで Copilot にメンションできます。Copilot はコメントを確認して、変更に対応する新しいセッションを開始します。結果は確定的ではないため、必ず同じ結果になると決めつけず、たとえば次の変更を依頼できます。

- 各コードファイルの先頭に、役割を簡潔に説明するコメントヘッダーを追加する。
- TypeScript と Astro ファイルに TSDoc のドキュメントコメントを追加する。
- Astro アプリの構造を説明する README を作成する。

1. 上記の例などを使い、生成されたドキュメントに対する変更をコメントで依頼します。通常のユーザーと同じように **@copilot** をメンションします。
2. **View Session** を選択し、Copilot が作業する様子を確認します。更新のために新しいセッションが始まります。
3. **Back to pull request** を選択すると、プルリクエストに戻れます。

   ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Copilot の作業が終わると、プルリクエストに新しいコミットが表示されます。
5. **Files changed** タブを選択し、変更をレビューします。

満足するまで改善を繰り返してください。準備ができたら、ドラフト PR をレビュー可能な状態にして `main` ブランチにマージします。

![PR をレビュー可能な状態にする画面](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

ゲーム詳細ページに関連ゲームを表示する Issue に対して Copilot が作成した PR を確認します。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Show related games on the game details page** などのタイトルの PR を選択します。
4. **Files changed** タブを選択し、生成されたコードをレビューします。
5. 変更を確認したら **Conversation** タブに戻り、下にスクロールします。
6. ワークフローの承認待ちを示すインジケーターがあるか確認します。
7. ワークフローが承認待ちの場合は **Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. PR のチェック欄にワークフローが登録されたことを確認します。問題がなければ、単一の Astro アプリに対するプロジェクトのチェックが成功します。完了まで数分かかる場合があります。
9. **任意:** Codespace でこのブランチに切り替えて、関連ゲーム機能を手動でテストできます。Codespace のターミナルを開き、`<branch-name>` を Copilot が作成したブランチ名 (例: **copilot/fix-8**) に置き換えて、次のコマンドを実行します。

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

Copilot が関連ゲーム機能を作成しました。前の作業と同じように、Copilot cloud agent に追加の更新を依頼できます。たとえば、表示する関連ゲームの数を調整したり、コメントヘッダーと TSDoc コメントを追加したりできます。この作業はカスタム指示を更新する**前**に割り当てたことを思い出してください。変更は、**Conversation** タブでコメントを追加して依頼できます。Copilot がコメントを確認し、新しいセッションを開始します。

## アクセシビリティ機能をレビューする

最後に、アクセシビリティ用のカスタムエージェントで実装した機能をレビューします。この PR には、レッスン 3 で依頼したハイコントラストモードと、レッスン 4 の Mission control で追加を依頼したライトモードの両方が含まれます。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Add high contrast mode to website** などのタイトルの PR を選択します。

> [!NOTE]
> Copilot がまだ作業中の場合、PR のタイトルに **[WIP]** が付きます。Copilot が作業を完了するまで待ってください。数分かかる場合があります。

4. **Files changed** タブを選択し、生成されたコードをレビューします。
5. 実装を確認します。特に、次の点に注目してください。
   - モード切り替え用の UI コンポーネント
   - ユーザー設定を保存する local storage の使用
   - ハイコントラストモードとライトモード向けの CSS またはスタイルの変更
   - アクセシビリティ属性 (ARIA ラベル、キーボード操作など)
   - モードの切り替えを管理する JavaScript/TypeScript コード
6. 変更を確認したら **Conversation** タブに戻り、下にスクロールします。
7. ワークフローの承認待ちを示すインジケーターがあるか確認します。
8. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. PR のチェック欄にワークフローが登録されたことを確認します。問題がなければ、単一の Astro アプリに対するプロジェクトのチェックが成功します。完了まで数分かかる場合があります。
10. **任意:** Codespace でこのブランチに切り替えて、アクセシビリティ機能を手動でテストできます。Codespace のターミナルで `<branch-name>` を Copilot が作成したブランチ名に置き換えて、次のコマンドを実行します。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    次にアプリケーションを起動し、ブラウザーでハイコントラストモードとライトモードの切り替えをテストします。ページを再読み込みしても設定が保持されることを確認してください。

アクセシビリティのカスタムエージェントが、アクセシビリティのベストプラクティスに従うよう Copilot をガイドしたことがわかります。アクセシビリティに関する懸念や改善点がある場合は、前の PR と同様にコメントで **@copilot** をメンションして変更を依頼できます。

## 任意の演習 — 委任を続ける

Cloud agent は、バックログの実際の作業を任せ、その間に別の作業を進めるときに効果を発揮します。練習として、リポジトリに Issue を作成し、Copilot に割り当ててください。たとえば、次の作業を依頼できます。

- ゲーム詳細ページに支援者向けの関心フォームを作成する
- ゲーム一覧ページにページネーションを実装する
- データアクセスヘルパーに入力検証とエラー処理を追加する

## まとめ

Cloud agent のハーネスを完了しました。レッスンを通じて、次のことを行いました。

- **このリポジトリに用意されたカスタム指示ファイルを確認し**、cloud agent の出力に反映される様子を見ました。
- **Issue を Copilot cloud agent に割り当て**、環境の準備、計画、非同期の作業を確認しました。
- **アクセシビリティのカスタムエージェントを作成して使い**、ハイコントラストとライトモードを追加しました。
- **Copilot agents ページを Mission control として使い**、アクセシビリティのセッションを監視して作業を調整しました。
- **cloud agent のプルリクエストをレビューして改善し**、`@copilot` に変更を依頼し、ワークフローを承認しました。

## レビューと次のステップ

Cloud agent のハーネスを完了しました。引き続き学ぶ場合は、他のハーネスも活用してください。

- 🖥️ **[VS Code ハーネス](../../vscode/)** — IDE 内で Copilot Chat のエージェントモードと MCP 統合を確認します。
- 💻 **[CLI ハーネス](../../cli/)** — Plan モードと Autopilot モード、エージェントスキル、QA カスタムエージェント、Playwright MCP、Agent Merge、任意の `/delegate` による cloud agent との連携を使い、ターミナルで2つの変更をレビューしてリリースします。

自分のリポジトリでは、次のような作業も試してください。

- リファクタリングやテストカバレッジの Issue を cloud agent に割り当てる
- 別の分野のカスタムエージェントを作成する
- 異なる技術スタック向けに `copilot-setup-steps.yml` を設定する

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot agents について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップワークフローのベストプラクティス][cloud-agent-best-practices]
- [Copilot cloud agent のファイアウォールを設定する][agent-firewall]

---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
