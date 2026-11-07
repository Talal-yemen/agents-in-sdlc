---
title: "レッスン 4 - カスタム指示で Copilot をガイドする"
description: "リポジトリの指示を確認し、ドキュメント標準を追加して、フィルター機能のコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を使うときは、コンテキストが重要です。作業方法に指定がある場合は、そのガイダンスを Copilot が参照できるようにします。[指示ファイル][instruction-files]では、求めるコードだけでなく、その構成方法も説明できます。フィルター機能を構築したので、Copilot が参照した指示を確認し、ドキュメント標準を追加してコードに適用します。

このレッスンでは、次のことを行います。

- リポジトリの指示とパスに応じた指示ファイルがエージェントに適用される方法を確認します。
- 指示ファイルを更新し、コーディング標準が守られるようにします。
- 指示ファイルがコードに与える影響を確認します。

## シナリオ

Tailspin Toys には、開発プロセスに関するガイドラインと要件があります。

- コメントでは、コードの言い換えではなく、意図や自明でない決定を説明します。
- `db/` と `src/lib/` のエクスポートされた関数では、目的、パラメーター、戻り値を TSDoc/JSDoc で説明します。`db` 引数を注入できる場合は、そのことも記載します。
- 再利用可能な Astro コンポーネントでは `Props` の契約を説明し、関連コードが変わったらコメントも更新します。
- 既存の書式設定と lint のガイダンスを維持します。

指示ファイルを使い、Copilot がこれらの慣行に沿って作業できるようにします。

## 指示ファイル

カスタム指示では、コーディングスタイルや要件を Copilot に伝え、より適切な提案やコードを生成できるようにします。推奨するコーディング規約やライブラリ、コメントの書き方などを指定できます。リポジトリ全体に適用する指示も、特定の種類のファイルだけに適用する指示も作成できます。

指示ファイルには次の2種類があります。

- `.github/copilot-instructions.md` は、リポジトリに関するすべてのリクエストで Copilot に送られる単一の指示ファイルです。多くのリクエストに関係するプロジェクト全体の情報を記載します。
- `.github/instructions/*.instructions.md` は、特定の言語、ファイルの種類、タスクに対するガイドラインを記載するファイルです。

> [!NOTE]
> 他の指示形式への対応状況は環境によって異なります。形式に頼る前に[カスタム指示のサポート][custom-instructions-support]を確認してください。

### このプロジェクトのカスタム指示ファイルを確認する

このプロジェクトには、開始点として指示ファイルが用意されています。変更を加える前に、まず既存のファイルを確認します。

1. Codespace に戻ります。
2. Codespaces のエディターで `.github/copilot-instructions.md` を開きます。ターミナルではなく、エディターで開いてください。
3. プロジェクトの概要と、リポジトリ内のすべての Copilot 操作に適用される指示を確認します。
4. `.github/instructions` フォルダーを開き、Astro、Drizzle データレイヤー、テストなどの指示を確認します。
5. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドが、適用対象を決める glob です。たとえば、`**/*.test.ts` に一致する TypeScript のテストファイルに適用されます。
6. `.github/instructions/drizzle.instructions.md` を開きます。別の指示ファイルやプロジェクトのファイルへのリンクを確認します。指示を小さく再利用可能なファイルに分け、Copilot が参照する例を示せます。

このプロジェクトには、コードにコメントを追加する標準はまだありません。次の手順で追加します。

## プロジェクトの指示ファイルを更新する

スターターリポジトリには、開発慣行を Copilot に伝える指示ファイルがすでに含まれています。コードコメントを追加するルールがないため、今回のフィルター機能の作業に合わせて `.github/copilot-instructions.md` を更新します。

1. Codespace のリポジトリルートで、`game-filters-cli` ブランチがチェックアウトされていることを確認します。
2. `.github/copilot-instructions.md` を開きます。
3. **Code formatting guidance** セクションを見つけます。およそ35行目にあります。
4. 最後の箇条書きとして、次の行を追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

ファイルは自動的に保存されます。

## 更新したガイダンスを適用する

Copilot CLI は会話の開始時にリポジトリの指示を読み込みます。フィルター機能のコンテキストを保ったまま新しいガイダンスを読み込ませるため、会話を終了して再開します。

1. フィルター機能の会話を開いている場合は、`/exit` を入力して終了します。Copilot CLI は会話を自動的に保存します。
2. リポジトリルートの `game-filters-cli` ブランチで、保存した会話を再開します。

   ```bash
   copilot --resume
   ```

   選択肢が表示された場合はフィルター機能の会話を選び、`game-filters-cli` ブランチがチェックアウトされていることを確認します。

3. Copilot に、更新したガイダンスを適用するよう依頼します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. `/diff` を入力して、変更された TypeScript ファイルを確認します。追加された TSDoc コメントがコードを正確に説明していることを確認します。

## まとめと次のステップ

Copilot CLI が指示ファイルからコンテキストを得る方法を確認し、新しい標準をフィルター機能に適用しました。このレッスンでは、次のことを行いました。

- リポジトリの指示とパスに応じた指示ファイルがエージェントに適用される方法を確認しました。
- コーディング標準を追加するため、指示ファイルを更新しました。
- 指示ファイルがコードに与える影響を確認しました。

次は、[再利用可能な `quality-checks` スキルをカスタマイズして実行します][next-lesson]。lint とテストが一貫して実行されるようにします。

## リソース

- [Copilot CLI にカスタム指示を追加する][instruction-files]
- [カスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Awesome Copilot — 指示ファイルのコレクションなど][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
