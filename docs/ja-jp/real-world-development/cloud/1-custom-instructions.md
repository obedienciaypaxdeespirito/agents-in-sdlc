---
title: "演習 1 - カスタム命令 (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot cloud agent →][next-lesson]

生成 AI を使うときはコンテキストが重要です。タスクを特定の方法で進める必要がある場合や、Copilot に知っておいてほしい背景情報がある場合は、必要なときにそのコンテキストを参照できるようにします。[命令ファイル][instruction-files]を使うと、何をしてほしいかだけでなく、どのように進めるかも Copilot に伝えられます。

この演習では、次のことを行います。

- リポジトリのカスタム命令とパス単位の命令ファイルを通じて、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント標準が Copilot に伝わる方法を確認する。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加する。

> [!NOTE]
> VS Code や CLI のハーネスとは異なり、この演習では変更前後のプロンプトを比較しません。Copilot cloud agent は GitHub issue を非同期に処理するため、影響をリアルタイムで並べて示すのは困難です。後続の演習で cloud agent が作成したプルリクエストをレビューするときに、命令ファイルの影響を確認します。

## 命令ファイル

### シナリオ

優れた開発チームと同じように、Tailspin Toys にも開発プラクティスのガイドラインと要件があります。

- データ レイヤーには必ずユニット テストを用意する。
- UI はダーク モードで、モダンな印象にする。
- コードのドキュメントには TSDoc の doc comment を使う。
- 各ファイルの先頭に、ファイルの役割を説明するコメント ブロックを追加する。

命令ファイルを使い、これらのプラクティスに沿ってタスクを実行するための情報を Copilot に渡します。

### カスタム命令

カスタム命令では、コンテキストや希望するコーディング スタイル、要件を Copilot に伝えられます。コーディング規約、ライブラリ、追加したいコメントの種類などを指定すると、より関連性の高い提案やコードを得られます。命令はリポジトリ全体に適用することも、特定のファイル種別に適用することもできます。

命令ファイルには 2 種類あります。

- `.github/copilot-instructions.md` は、リポジトリへの**すべて**のリクエストで Copilot に送信される単一のファイルです。使用する技術スタック、プロジェクトの概要、ベスト プラクティスなど、ほとんどのチャットや CLI リクエストに関係するプロジェクト レベルの情報を記載します。
- `.github/instructions/*.instructions.md` は、特定のタスクやファイル種別向けの命令です。TypeScript や Astro などの言語、UI コンポーネントの作成、ユニット テストの追加などに使えます。

> [!NOTE]
> IDE で作業する場合、命令ファイルは Copilot Chat のコード生成にのみ使われます。コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、コード生成時にリポジトリ レベルの命令と `applyTo` frontmatter を持つ `*.instructions.md` ファイルの両方を使います。
>
> Copilot は [AGENTS.md や CLAUDE.md][custom-instructions-support] など、別の標準に基づく命令ファイルもサポートしています。

### 命令ファイル管理のベスト プラクティス

命令ファイルの作成方法を詳しく説明することは、このワークショップの範囲外です。サンプル プロジェクトの例は一般的な方法を示しています。

- `copilot-instructions.md` には、構築するものの説明、プロジェクト構造、全体に適用されるコーディング標準などのプロジェクト レベルのガイダンスを記載します。
- `*.instructions.md` ファイルは、ファイル種別（ユニット テスト、Astro コンポーネント、データ レイヤー）やタスクに固有の命令に使います。
- 自然言語で明確に書き、望ましいコードと望ましくないコードの例を示します。

命令ファイルの作り方や AI の使い方に唯一の正解はありません。試しながらプロジェクトに合った方法を見つけてください。

> [!TIP]
> GitHub Copilot を使う各プロジェクトには、十分な命令ファイルを用意することをお勧めします。このプロジェクトには、[UI の更新][ui-instructions]や [Astro][astro-instructions]など、さまざまなタスク向けのファイルがあります。
>
> Copilot に命令ファイルを作成してもらうこともできます。VS Code の **Configure Chat → Generate Agent Instructions** や Copilot CLI の `/init` など、各環境での方法は異なります。関連する手順は各環境のレッスンで説明します。
>
> テンプレートや出発点が必要な場合は、命令ファイル、カスタム エージェントなどを集めた [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム命令ファイルを確認する

このリポジトリに用意された、中心となる `copilot-instructions.md` と各タスク向けの `*.instructions.md` ファイルを、エディターまたは GitHub Web UI で確認します。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの簡潔な説明に加え、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** 内の **GitHub Actions Workflows** のガイダンスにも注目してください。これらは Copilot とのあらゆるやり取りに適用されます。
3. `.github/instructions` フォルダーを開きます。Astro ファイル、Drizzle データ レイヤー、テストなどの命令があります。
4. `.github/instructions/unit-tests.instructions.md` を開き、先頭の `applyTo` を確認します。このフィールドはリポジトリ ルートからの相対 glob を指定します。この例では `**/*.test.ts` に一致する TypeScript テスト ファイルが対象です。
5. このプロジェクトのユニット テスト作成に特化した命令を確認します。
6. `.github/instructions/drizzle.instructions.md` の末尾まで読みます。`unit-tests.instructions.md` などのほかの命令ファイルや、プロジェクト内の既存ファイルへのリンクがあります。大きな命令セットを再利用しやすい単位に分け、Copilot が参照する例を示せます（記載されているパスはリポジトリ ルートではなく命令ファイルからの相対パスです）。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング標準が記載されていますが、コード内ドキュメントはまだ必須ではありません。次の手順で TSDoc doc comment とファイル コメント ヘッダーのルールを追加します。

## リポジトリ標準を追加する

`.github/copilot-instructions.md` は Copilot にプロジェクト レベルの情報を提供します。コード提案の品質を高めるため、コード内の TSDoc doc comment とファイル レベルのコメント ヘッダーを標準に追加します。cloud agent は、このハーネスの後半で割り当てる issue に取り組むときにこの標準を使います。

編集する前に、作業ブランチを作成します。cloud agent は issue の対象ブランチにある命令を読み込みますが、ここでは変更を分けてコミットします。

1. Codespace のターミナルで、新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

2. `.github/copilot-instructions.md` を開き直します。
3. **Code formatting requirements** セクション（27 行目前後）を探します。プロジェクト標準は記載されていますが、コード内ドキュメントのルールがありません。
4. 既存の標準の直下に、次の Markdown 行を追加し、ファイル コメント ヘッダーと TSDoc doc comment を加えるよう指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

5. `copilot-instructions.md` を保存します。

> [!TIP]
> 命令ファイルは、全体的なガイダンスにはリポジトリ レベル（`.github/copilot-instructions.md`）、特定の言語、ファイル種別、タスクには `*.instructions.md` として作成できます。ここで追加した doc comment ルールのように、プロジェクト全体の標準はリポジトリ レベルに置きます。

## 命令の更新をコミット、push、マージする

cloud agent は issue の対象ブランチから命令ファイルを読み込みます。次の演習で Copilot に issue を割り当てると、cloud agent は `main` からブランチを作成するため、命令の変更を `main` にマージしておく必要があります。

1. 変更をステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチを push します。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` へのプルリクエストを作成してマージします。これで次の演習で cloud agent が命令を読み取れます。

> [!TIP]
> このワークショップでは `main` に直接作業してもかまいません。その場合はブランチを作らずに `main` にコミットできます。ここでは実際のプロジェクトと同じ流れにするため、ブランチを使っています。

## まとめと次のステップ

Copilot が命令ファイルからコンテキストを得る方法を確認し、リポジトリ全体に適用される標準を追加しました。残りのハーネスで cloud agent が作成するプルリクエストをレビューするときに、この標準が使われます。

次は、[最初の issue を Copilot cloud agent に割り当てます][next-lesson]。

## リソース

- [GitHub Copilot のカスタマイズ用命令ファイル][instruction-files]
- [カスタム命令を作成するためのベスト プラクティス][instructions-best-practices]
- [Copilot 向けのより良いカスタム命令を書くための 5 つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — 命令ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
