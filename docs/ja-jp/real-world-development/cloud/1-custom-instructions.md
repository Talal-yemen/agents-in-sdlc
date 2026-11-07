---
title: "レッスン 1 - カスタム指示 (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot cloud agent →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。作業方法に指定がある場合や、Copilot に知っておいてほしい背景情報がある場合は、そのコンテキストを参照できるようにします。[指示ファイル][instruction-files]を使うと、Copilot に「何を」行うかだけでなく、「どのように」行うかも伝えられます。

このレッスンでは、次のことを行います。

- リポジトリのカスタム指示とパスに応じた指示ファイルを使って、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント標準を Copilot に伝える方法を確認します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加します。

> [!NOTE]
> VS Code や CLI のハーネスとは異なり、このレッスンでは変更前後のプロンプトを比較しません。Copilot cloud agent は GitHub Issue に対して非同期で動作するため、リアルタイムに比較するのが難しいためです。このハーネスの後半で cloud agent が作成したプルリクエストをレビューするときに、指示ファイルの影響を確認します。

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

- `.github/copilot-instructions.md` は、リポジトリに関するすべてのリクエストで Copilot に送られる単一の指示ファイルです。多くのリクエストに関係するプロジェクト全体の情報を記載します。
- `.github/instructions/*.instructions.md` は、言語やファイルの種類など、特定の作業に対するガイドラインを記載するファイルです。

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

このリポジトリには、中心となる `copilot-instructions.md` と、さまざまな作業向けの `*.instructions.md` ファイルがあります。エディターまたは GitHub の Web UI で確認してください。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの簡単な説明と、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** 内には **GitHub Actions Workflows** のガイダンスもあります。これらは、Copilot とのあらゆるやりとりに適用されます。
3. `.github/instructions` フォルダーを開き、Astro ファイル、Drizzle データレイヤー、テストなどに関する指示を確認します。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドは、指示を適用するファイルを決める glob です。たとえば、`**/*.test.ts` に一致する TypeScript のテストファイルが対象になります。
5. このプロジェクトでユニットテストを作成するときの指示を確認します。
6. `.github/instructions/drizzle.instructions.md` を開き、末尾にある他の指示ファイル (`unit-tests.instructions.md` など) やプロジェクト内ファイルへのリンクを確認します。これにより、大きな指示を再利用可能な小さなファイルに分け、Copilot が参照する例を示せます。ここに記載されたパスはリポジトリルートではなく、指示ファイルからの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング標準が記載されていますが、コード内のドキュメントに関する規則はまだありません。次の手順で TSDoc コメントとファイルヘッダーの規則を追加します。

## リポジトリ全体に適用する標準を追加する

このレッスンで編集するのは、cloud agent が以降のレッスンで割り当てられた Issue に取り組むときに参照する、プロジェクト全体のルールです。

編集の前にブランチを作成します。cloud agent は Issue の対象ブランチから指示を読み込みますが、この変更も適切にコミットしておきます。

1. Codespace のターミナルで、新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

`.github/copilot-instructions.md` はプロジェクトレベルの情報を Copilot に提供します。コーディング標準を追加して、生成されるコードの提案を改善します。

1. `.github/copilot-instructions.md` を開き直します。
2. **Code formatting requirements** セクションを見つけます。およそ27行目にあります。既存の標準を確認します。TSDoc コメントがまだ求められていないため、生成コードにもコメントがありません。
3. 既存の標準の直後に、次の Markdown を追加します。これにより、ファイルヘッダーと TSDoc コメントを追加するよう Copilot に指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のセクションで確認したように、指示ファイルはリポジトリ全体に適用される `.github/copilot-instructions.md` と、言語やファイルの種類、作業に応じた `*.instructions.md` ファイルとして作成できます。ここで追加するドキュメントコメントのルールのように、プロジェクト全体の標準はリポジトリレベルのファイルに記載します。

## 指示の変更をコミット、プッシュ、マージする

Cloud agent は、Issue が対象とするブランチにある指示ファイルを読み込みます。次のレッスンで Issue を Copilot に割り当てると、Copilot は `main` からブランチを作成します。そのため、指示を読み込ませるには変更を `main` にマージする必要があります。

1. 変更をステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチをプッシュします。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` に向けたプルリクエストを作成し、マージします。次のレッスンで割り当てる Issue に取り組む際、cloud agent はこの指示を読み込みます。

> [!TIP]
> このワークショップでは、ブランチの作成手順を省略して `main` に直接コミットしてもかまいません。実際のプロジェクトと同じ手順を体験できるように、ここではブランチを使っています。

## まとめと次のステップ

このプロジェクトで Copilot が指示ファイルからコンテキストを得る方法を確認し、`.github/copilot-instructions.md` にリポジトリ全体の標準を追加しました。このハーネスの残りのレッスンで cloud agent が作成するプルリクエストをレビューすると、この標準が反映されていることを確認できます。

次は、[最初の Issue を Copilot cloud agent に割り当てます][next-lesson]。

## リソース

- [GitHub Copilot のカスタム指示ファイル][instruction-files]
- [カスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Copilot のカスタム指示を改善する5つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — 指示ファイルのコレクションなど][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
[awesome-copilot]: https://github.com/github/awesome-copilot
