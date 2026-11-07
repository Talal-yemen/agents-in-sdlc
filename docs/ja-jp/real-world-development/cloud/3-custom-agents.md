---
title: "レッスン 3 - カスタムエージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタムエージェントとは

GitHub Copilot の[カスタムエージェント][custom-agents-concept]を使うと、開発ワークフローの特定の作業や分野に特化した AI アシスタントを作成できます。リポジトリの `.github/agents` フォルダーに Markdown ファイルを作成し、作業に必要な指示、ベストプラクティス、コーディングパターン、専門知識を定義します。たとえば、[WCAG][wcag] 準拠を確認するアクセシビリティエージェント、セキュアコーディングを支援するセキュリティエージェント、一貫したテストパターンを守るテストエージェントなどを作成して、チームの知識を再利用できます。

カスタムエージェントは、プロジェクト内の `.github/agents` フォルダー、またはグローバルな `~/.copilot/agents` にある Markdown ファイルで定義します。各ファイルには、少なくとも `name` と `description` を含む YAML フロントマターと、エージェントの動作、専門知識、指示を記載する Markdown プロンプトが必要です。

### カスタムエージェントとエージェントスキルの違い

カスタムエージェントと[エージェントスキル][agent-skills-concept]には共通点があります。どちらも主に Markdown ファイルで定義され、AI に作業の方法を伝えます。わかりやすく区別すると、**カスタムエージェントは作業者、スキルはツール**です。

カスタムエージェントは独自のコンテキストウィンドウを持ち、スキルや他のエージェントを使って作業を進めます。このワークショップでは、アクセシビリティのカスタムエージェントがサイトをアクセシビリティのガイドラインに照らして確認し、必要な変更を提案します。作業中に、プルリクエストの手順やテスト実行を管理するスキルを呼び出すこともできます。

> [!NOTE]
> カスタムエージェントの作成に唯一の正解はありません。AI の他の使い方と同様に、使う環境やシナリオに合わせて試し、改善してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

このレッスンでは、カスタムエージェントを使って次のことを学びます。

- カスタムエージェントの定義方法
- タスクをカスタムエージェントに割り当てる方法

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、すべてのユーザーがクラウドファンディングプラットフォームを利用できるようにしたいと考えています。最近、現在のダークテーマはテキストと背景色のコントラストが不十分で、読みづらいというフィードバックが寄せられました。このアクセシビリティ上の課題に対応するため、デザインチームはユーザーが切り替えられるハイコントラストモードの実装を依頼しました。

アクセシビリティは重要なので、できるだけ早く実装します。カスタムエージェントを使って機能を生成しましょう。

## アクセシビリティのカスタムエージェントを確認する

アクセシビリティ向けのカスタムエージェントがすでに用意されています。内容を確認して、Copilot の作業にどのような指針を与えるかを見てみましょう。

Codespace に戻り、アクセシビリティエージェントのファイルを確認します。

1. `.github/agents/accessibility.md` を開きます。
2. `name` と `description` を含む YAML フロントマターを確認します。

> [!CAUTION]
> カスタムエージェントには、`name` と `description` を含むフロントマターが必要です。

3. 続くセクションを確認します。次の内容が記載されています。
   - アクセシブルな Web サイトのコードを生成する際の主な責務
   - アクセシビリティのベストプラクティス
   - HTML、CSS、JavaScript のコード例
   - よくある落とし穴や間違い

## Issue を作成して割り当てる

Mission control は、すべてのエージェントを管理する場所です。Copilot cloud agent にタスクを割り当てたり、進捗を監視したり、追加の指示を伝えたりできます。まず、ハイコントラストモードの作成タスクを Copilot に割り当てます。

1. リポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue のダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `Add high contrast mode to website` と入力します。
6. **Description** に次のテキストを入力します。

   ```plaintext
   We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
   ```

7. **Create** を選択して Issue を作成します。
8. 右側にある **Assign to Copilot** を選択して、割り当てのダイアログを開きます。
9. カスタムエージェントの一覧から **Accessibility agent** を選択します。

   ![カスタムエージェントの割り当て画面で、Accessibility agent が選択されているスクリーンショット](../../../_images/ex5-select-custom-agent.png)

10. **Assign** を選択します。
11. Copilot がバックグラウンドで作業を開始します。

## まとめと次のステップ

[カスタムエージェント][custom-agents]は、特定の作業や分野に特化した GitHub Copilot の AI アシスタントです。チームの知識や標準を再利用可能なエージェントにまとめ、Copilot の作業をガイドできます。

このレッスンでは、次のことを学びました。

- カスタムエージェントの定義方法
- タスクをカスタムエージェントに割り当てる方法

Copilot がハイコントラストモードの実装を進めている間に、[Mission control でエージェントのセッションを監視し、作業を調整します][next-lesson]。

## リソース

- [カスタムエージェントについて][custom-agents]
- [組織でカスタムエージェントを使用する準備][org-custom-agents]
- [Enterprise でカスタムエージェントを使用する準備][enterprise-custom-agents]

---

| [← 前のレッスン: GitHub Copilot cloud agent][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
