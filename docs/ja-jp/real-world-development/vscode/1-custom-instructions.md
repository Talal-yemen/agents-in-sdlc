---
title: "レッスン 1 - カスタム指示 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: エージェントモード →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。作業方法に指定がある場合や、Copilot に知っておいてほしい背景情報がある場合は、そのコンテキストを参照できるようにします。Copilot に特定のコンテキストを伝える方法はいくつかあります。その中心となるのが[指示ファイル][instruction-files]です。コード生成で「何を」するかだけでなく、「どのように」行うかを伝えます。

このレッスンでは、次のことを行います。

- リポジトリのカスタム指示とパスに応じた指示ファイルを使って、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント標準を Copilot に伝える方法を確認します。
- 現在の指示に従って、フィルター機能の最初のデータ部分 (publisher ヘルパー) を生成します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加します。
- 同じプロンプトをもう一度実行し、生成コードが新しい標準に従う様子を確認します。
- 次のレッスンで Copilot が最新の指示を使えるように、指示の変更とフィルター機能を `main` にコミットします。

> [!CAUTION]
> 生成コードが設定した標準から外れる場合があります。Copilot は確定的に動作しません。このレッスンの目的は、出力を一字一句同じにすることではなく、指示を更新した後の動作の変化を確認することです。

## 指示ファイル

### シナリオ

Tailspin Toys には、開発プロセスに関するガイドラインと要件があります。たとえば、次のとおりです。

- データレイヤーには、必ずユニットテストを用意します。
- UI はダークモードにし、現代的な見た目にします。
- TSDoc のドキュメントコメントをコードに追加します。
- 各ファイルの先頭に、ファイルの役割を説明するコメントブロックを追加します。

指示ファイルを使って、Copilot がこれらの開発慣行に沿って作業できるようにします。

### カスタム指示

カスタム指示では、コーディングスタイルや要件を Copilot に伝え、より適切な提案やコードを生成できるようにします。推奨するコーディング規約やライブラリ、コメントの書き方などを指定できます。リポジトリ全体に適用する指示も、特定の種類のファイルだけに適用する指示も作成できます。

指示ファイルには次の2種類があります。

- `.github/copilot-instructions.md` は、リポジトリに関するすべてのリクエストで Copilot に送られる単一の指示ファイルです。多くのリクエストに関係するプロジェクト全体の情報を記載します。技術スタック、開発中の内容、ベストプラクティス、全体的なガイダンスなどが該当します。
- `.github/instructions/*.instructions.md` は、言語やファイルの種類など、特定の作業に対するガイドラインを記載するファイルです。TypeScript や Astro、UI コンポーネントの作成、ユニットテストの追加などに使えます。

> [!NOTE]
> IDE では、指示ファイルは Copilot Chat のコード生成にのみ使われ、コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、コード生成時にリポジトリレベルの指示と `applyTo` フロントマターを持つ `*.instructions.md` ファイルの両方を使用します。
>
> Copilot は AGENTS.md や CLAUDE.md など、ほかの規格に基づく指示ファイルにも対応しています。詳細は[カスタム指示のサポート][custom-instructions-support]を参照してください。

### 指示ファイルを管理するためのベストプラクティス

指示ファイルの作成について詳しく説明することは、このワークショップの範囲外です。ここでは、サンプルプロジェクトに含まれるファイルを使って、代表的な方法を紹介します。

- `copilot-instructions.md` には、開発中の内容、プロジェクト構成、全体的なコーディング標準など、プロジェクトレベルのガイダンスを記載します。
- `*.instructions.md` には、ユニットテスト、Astro コンポーネント、データレイヤーなどのファイルの種類や、特定の作業に関する指示を記載します。
- 自然な言葉で、明確なガイダンスとコードの良い例・悪い例を示します。

指示ファイルの作成方法は1つではありません。AI の使い方と同じように、試しながらプロジェクトに合う方法を見つけてください。

> [!TIP]
> GitHub Copilot を使うプロジェクトでは、指示ファイルを充実させることをお勧めします。このプロジェクトには、[UI 更新][ui-instructions]や [Astro][astro-instructions]など、さまざまな作業向けのファイルが用意されています。
>
> Copilot に指示ファイルを作成させることもできます。操作方法は利用する環境によって異なります (例: VS Code の **Configure Chat → Generate Agent Instructions**、Copilot CLI の `/init`)。必要に応じて、それぞれのレッスンで説明します。
>
> テンプレートやひな形を探している場合は、指示ファイル、カスタムエージェントなどを集めた [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム指示ファイルを確認する

このリポジトリには、中心となる `copilot-instructions.md` と、さまざまな作業向けの `*.instructions.md` ファイルが含まれています。エディターまたは GitHub の Web UI で確認してください。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの簡単な説明と、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** 内には **GitHub Actions Workflows** のガイダンスもあります。これらは、Copilot とのあらゆるやりとりに適用されます。
3. `.github/instructions` フォルダーを開き、Astro ファイル、Drizzle データレイヤー、テストなどに関する指示を確認します。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドは、指示を適用するファイルを決める glob です。たとえば、`**/*.test.ts` に一致する TypeScript のテストファイルが対象になります。
5. このプロジェクトでユニットテストを作成するときの指示を確認します。
6. `.github/instructions/drizzle.instructions.md` を開き、末尾にある他の指示ファイル (`unit-tests.instructions.md` など) やプロジェクト内ファイルへのリンクを確認します。これにより、大きな指示を再利用可能な小さなファイルに分け、Copilot が参照する例を示せます。ここに記載されたパスはリポジトリルートではなく、指示ファイルからの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング標準が記載されていますが、コード内のドキュメントに関する規則はまだありません。次の手順で TSDoc コメントとファイルヘッダーの規則を追加します。

## 変更用のブランチを作成する

ここでは、変更用のブランチを作成するベストプラクティスに従います。

1. 前の演習で使った Codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. 新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## 指示を更新する前に Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 下の演習を始める前に Codespace に戻り、Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習ごとに異なります。必要な場合は、各手順で説明します。

指示の効果を確認するため、まずは現在の指示ファイルを使ってプロンプトを送信します。後で指示を更新し、同じプロンプトをもう一度送信して違いを見ます。

1. 前の演習で開いたエディタータブを閉じ、Copilot が必要なコンテキストだけを取得できるようにします。
2. `src/lib/publishers.ts` を開き、ヘルパーを置く場所を Copilot に示します。
3. Chat ビューのエージェントのドロップダウンから **Agent** を選択し、Copilot がファイルを変更できるようにします。

   ![Chat ビューでエージェントを選択する画面](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot がプロジェクトを調査してコードを更新します。通常、ヘルパーファイルとテストが変更されます。
6. 提案されたヘルパーは、最初の引数として `db` クライアントを受け取る型付き関数で、publisher の型付き配列を返します。これは、`src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` のデータレイヤー規約に基づいています。
7. 提案されたコードには TSDoc のドキュメントコメントとファイルレベルのコメントヘッダーが**ない**ことを確認します。

> [!CAUTION]
> Copilot は確定的に動作しないため、指示がなくてもドキュメントコメントを追加する可能性があります。その場合も問題ありません。指示を更新した後に動作の一貫性が高まることが、この演習の目的です。

## リポジトリ全体に適用する標準を追加する

前のセクションで確認したように、`.github/copilot-instructions.md` はプロジェクトレベルの情報を Copilot に提供します。コードの提案を改善するために、リポジトリのコーディング標準を追加しましょう。

1. `.github/copilot-instructions.md` を開き直します。
2. およそ27行目にある **Code formatting requirements** セクションを見つけます。コーディング標準は記載されていますが、コード内ドキュメントの規則はありません。そのため、生成されたヘルパーにはドキュメントコメントがありませんでした。
3. 既存の標準の直後に、次の Markdown を追加します。ファイルヘッダーと TSDoc のドキュメントコメントを追加するよう Copilot に指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したように、指示ファイルはリポジトリ全体に適用される `.github/copilot-instructions.md` と、言語やファイルの種類、作業に応じた `*.instructions.md` ファイルとして作成できます。ここで追加するドキュメントコメントのルールのように、プロジェクト全体の標準はリポジトリレベルのファイルに記載します。

## 同じプロンプトを再実行して変化を確認する

1. Copilot Chat に戻り、**New Chat** を選択して会話を新しくします。
2. `src/lib/publishers.ts` をもう一度開き、Copilot が正しいファイルに注目するようにします。
3. 先ほどと**同じプロンプト**を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 提案されたファイルの先頭に、次のようなコメントブロックが追加されたことを確認します。

   ```typescript
   /**
    * Tailspin Toys クラウドファンディングプラットフォームの publisher データアクセスヘルパー。
    * データベースから publisher の情報を取得する関数を提供します。
    */
   ```

5. 関数に、次のような TSDoc コメントが追加されたことを確認します。

   ```typescript
   /**
    * すべての publisher の ID と名前の一覧を返します。
    *
    * @param db - Drizzle データベースクライアント。
    * @returns publisher オブジェクトの配列で解決される Promise。
    */
   ```

これで、プロジェクトの標準に従うよう Copilot をガイドし、次の演習で使う実際のコードに適用できました。

## 指示と変更をコミットしてブランチをプッシュする

指示ファイルも他のリポジトリアセットと同様に、ソース管理で管理します。変更をコミットして、リポジトリにプッシュしましょう。

1. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
2. 次のコマンドを実行し、指示ファイルとヘルパーの変更を確認します。

   ```bash
   git status
   ```

3. ターミナルから、指示の更新とヘルパーの基礎となるコードをステージしてコミットします。Copilot がテストなどの追加ファイルを変更した場合は、そのファイルも追加します。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # git status にテストなどの追加変更が表示された場合は、それらのファイルも追加します。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. ブランチをリポジトリにプッシュします。

   ```bash
   git push -u origin custom-instructions
   ```

## プルリクエストを作成してマージする

ブランチをプッシュしたら、バックログのドキュメント標準に関する Issue と関連付けてプルリクエストを作成します。手動で行う代わりに、すでにプロジェクトに接続されている GitHub ツールを使って Copilot に作成を依頼します。Issue を検索し、解決する PR を作成してから、マージを依頼します。

> [!NOTE]
> Copilot に GitHub 上の操作を依頼するのは、このレッスンが初めてです。プロジェクトのテンプレートには GitHub ツールがすでに設定されています。Copilot が初めて GitHub ツールを使うときに、VS Code で GitHub へのサインインを求められる場合があります。画面の指示に従って許可してください。後のレッスンでは、この接続で使われる Model Context Protocol について確認します。

1. Codespace で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、エージェントの選択欄で **Agent** を選びます。
3. 指示ファイルの更新と、フィルター機能の準備となる publisher ヘルパーの追加について PR に記載し、関連 Issue を検索するよう Copilot に依頼します。

   ```plaintext
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. GitHub の操作を Copilot に許可するよう求められたら、コマンドを確認して必要に応じて **Allow** を選択します。
5. PR が作成されたら、Copilot に PR のマージと、ローカルブランチを `main` に戻して最新状態にするよう依頼します。

   ```plaintext
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

6. GitHub の操作やシェルコマンドの実行を Copilot に許可するよう求められたら、コマンドを確認して必要に応じて **Allow** を選択します。

GitHub Copilot の支援でプルリクエストを作成し、マージできました。

## まとめと次のステップ

このプロジェクトで Copilot が指示ファイルからコンテキストを得る方法を確認し、VS Code の Copilot Chat を使って次のことを行いました。

- 既存の指示でコード生成のプロンプトを送信し、Copilot の生成内容を確認しました。
- `.github/copilot-instructions.md` にリポジトリ全体の標準を追加しました。
- 同じプロンプトを再実行し、提案されたコードが新しい標準に従うことを確認しました。
- 指示の変更とヘルパーの基礎となるコードを `main` にコミットし、次のレッスンに引き継ぎました。

次は、エージェントモードで Copilot にコードベース全体の機能を追加させ、指示を活用します。

## リソース

- [GitHub Copilot のカスタマイズ用指示ファイル][instruction-files]
- [カスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Copilot のカスタム指示を改善する5つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用カスタム指示][personal-instructions]
- [Awesome Copilot — 指示ファイルのコレクションなど][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
[awesome-copilot]: https://github.com/github/awesome-copilot
