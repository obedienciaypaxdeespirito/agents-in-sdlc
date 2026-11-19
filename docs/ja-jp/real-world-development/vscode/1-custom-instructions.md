---
title: "演習 1 - カスタム命令 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Agent モード →][next-lesson]

生成 AI を使うときはコンテキストが重要です。タスクを特定の方法で進める必要がある場合や、Copilot に知っておいてほしい背景情報がある場合は、そのコンテキストを参照できるようにします。特定のコンテキストを Copilot と共有する方法はいくつかあります。特に重要なのは、コード生成のガイダンスを提供する[命令ファイル][instruction-files]です。

この演習では、次のことを行います。

- リポジトリのカスタム命令とパス単位の命令ファイルを通じて、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント標準が Copilot に伝わる方法を確認する。
- *現在の*命令を使って、フィルタリングに必要な最初のデータ部分（publishers ヘルパー）を作成する。
- `.github/copilot-instructions.md` に新しいリポジトリ標準を追加する。
- 同じプロンプトを再実行し、生成コードが新しい標準を取り入れる様子を確認する。
- 命令の更新とフィルタリングの基礎を `main` にコミットし、次の演習で Copilot が最新のガイダンスを利用できるようにする。

> [!CAUTION]
> 生成コードが設定した標準から外れる場合があります。Copilot は確率的に動作します。この演習の目的は、命令を更新した後の動作の*傾向*を確認することであり、出力を 1 文字ずつ一致させることではありません。

## 命令ファイル

### シナリオ

優れた開発チームと同じように、Tailspin Toys にも開発プラクティスに関するガイドラインと要件があります。

- データ レイヤーには必ずユニット テストを用意する。
- UI はダーク モードで、モダンな印象にする。
- コードのドキュメントには TSDoc の doc comment を使う。
- 各ファイルの先頭に、ファイルの役割を説明するコメント ブロックを追加する。

命令ファイルを使い、これらのプラクティスに沿ってタスクを実行するための情報を Copilot に渡します。

### カスタム命令

カスタム命令では、コンテキストや希望するコーディング スタイル、要件を Copilot に伝えられます。コーディング規約、ライブラリ、追加したいコメントの種類などを指定すると、より関連性の高い提案やコードを得られます。命令はリポジトリ全体に適用することも、特定のファイル種別に適用することもできます。

命令ファイルには 2 種類あります。

- `.github/copilot-instructions.md` は、リポジトリへの**すべて**のリクエストで Copilot に送信される単一のファイルです。技術スタック、プロジェクトの概要、ベスト プラクティスなど、ほとんどのチャットや CLI リクエストに関係する情報を記載します。
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

命令ファイルの作成方法や AI の使い方に唯一の正解はありません。試しながらプロジェクトに合った方法を見つけてください。

> [!TIP]
> GitHub Copilot を使う各プロジェクトには、十分な命令ファイルを用意することをお勧めします。このプロジェクトには、[UI の更新][ui-instructions]や [Astro][astro-instructions]など、さまざまなタスク向けのファイルがあります。
>
> Copilot に命令ファイルを作成してもらうこともできます。VS Code の **Configure Chat → Generate Agent Instructions** や Copilot CLI の `/init` など、環境によって方法は異なります。関連する手順は各環境のレッスンで説明します。
>
> テンプレートや出発点が必要な場合は、命令ファイル、カスタム エージェントなどを集めた [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム命令ファイルを確認する

リポジトリに用意された中心的な `copilot-instructions.md` と、タスク別の `*.instructions.md` ファイルをエディターまたは GitHub Web UI で確認します。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの簡潔な説明、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などを確認します。**Code standards** 内の **GitHub Actions Workflows** のガイダンスにも注目します。これらは Copilot とのすべてのやり取りに適用されます。
3. `.github/instructions` フォルダーを開き、Astro ファイル、Drizzle データ レイヤー、テストなどの命令を確認します。
4. `.github/instructions/unit-tests.instructions.md` を開き、先頭の `applyTo` を確認します。このフィールドはリポジトリ ルートからの相対 glob を指定します。この例では `**/*.test.ts` に一致する TypeScript テスト ファイルが対象です。
5. このプロジェクトのユニット テスト作成に特化した命令を確認します。
6. `.github/instructions/drizzle.instructions.md` の末尾まで読みます。`unit-tests.instructions.md` などのほかの命令ファイルや既存ファイルへのリンクがあります。大きな命令を再利用しやすい単位に分け、Copilot が参照できる例を示せます（記載されているパスはリポジトリ ルートではなく命令ファイルからの相対パスです）。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトの標準が記載されていますが、コード内ドキュメントはまだ必須ではありません。次の手順で TSDoc doc comment とファイル コメント ヘッダーのルールを追加します。

## 変更用ブランチを作成する

ベスト プラクティスに従い、変更用ブランチを作成します。

1. 前の演習で使った Codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>`</kbd>（Mac）または <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）を押して、新しいターミナルを開きます。
3. 新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## 命令を更新する*前に* Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 次の演習を始める前に Codespace に戻り、Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習ごとに異なります。必要な箇所で説明します。

カスタム命令の影響を確認するため、まず現在の命令ファイルを使ってプロンプトを送信します。その後、命令を更新して同じプロンプトをもう一度送信し、違いを確認します。

1. Copilot が必要なコンテキストだけを取得するよう、前の演習で開いたエディター タブを閉じます。
2. Copilot にヘルパーの場所を知らせるため、`src/lib/publishers.ts` を開きます。
3. Chat ビューのエージェント ドロップダウンから **Agent** を選び、Copilot がファイルを変更できるようにします。

   ![Chat ビューのエージェント ピッカーのスクリーンショット](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot はプロジェクトを調査し、コードを更新します。ヘルパーとテストのファイルが変更されることがあります。
6. 提案されたヘルパーが、最初の引数に `db` クライアントを受け取り、型付きの出版社配列を返すことを確認します。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` のデータ レイヤー規約によるものです。
7. 提案されたコードに TSDoc doc comment とファイル レベルのコメント ヘッダーが**ない**ことを確認します。

> [!CAUTION]
> Copilot は確率的に動作するため、命令がなくても doc comment を追加する場合があります。その場合も問題ありません。重要なのは、命令を追加した後に動作の一貫性が高まることです。

## リポジトリ標準を追加する

`.github/copilot-instructions.md` はプロジェクト レベルの情報を Copilot に提供します。コード提案の質を高めるため、リポジトリのコーディング標準を明記します。

1. `.github/copilot-instructions.md` を開き直します。
2. **Code formatting requirements** セクション（27 行目前後）を探します。生成されたヘルパーに doc comment がなかった理由は、コード内ドキュメントのルールがなかったためです。
3. 既存の標準の直下に、次の Markdown 行を追加し、ファイル コメント ヘッダーと TSDoc doc comment を作成するよう Copilot に指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 命令ファイルは、全体的なガイダンスにはリポジトリ レベル（`.github/copilot-instructions.md`）、特定の言語、ファイル種別、タスクには `*.instructions.md` として作成できます。ここで追加した doc comment ルールのように、プロジェクト全体の標準はリポジトリ レベルに置きます。

## プロンプトを再実行して変化を確認する

1. Copilot Chat に戻り、**New Chat** を選択して会話を新しくします。
2. `src/lib/publishers.ts` を選択し、Copilot に対象ファイルを知らせます。
3. 先ほどと**同じプロンプト**を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 提案されたファイルの先頭に、次のようなコメント ブロックが追加されていることを確認します。

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 提案された関数に、次のような TSDoc コメントが含まれていることを確認します。

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

Copilot を新しいプロジェクト標準に従わせ、次の演習で利用する実際のコードに適用できました。

## 命令とブランチをコミットして push する

命令ファイルもリポジトリの資産です。ほかのファイルと同じようにバージョン管理します。

1. <kbd>Control</kbd>+<kbd>`</kbd>（Mac）または <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）を押し、新しいターミナルを開きます。
2. 次のコマンドで命令の更新とヘルパーの変更を確認します。

   ```bash
   git status
   ```

3. 命令の更新とヘルパーの基礎に関するファイルをステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # git status に追加の関連変更（テストなど）が表示された場合は、それらのファイルも追加します。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. ブランチをリポジトリに push します。

   ```bash
   git push -u origin custom-instructions
   ```

## プルリクエストを作成してマージする

ブランチを push したので、バックログにあるドキュメント標準の issue に関連付けてプルリクエストを作成します。手動でも行えますが、プロジェクトに接続済みの GitHub ツールを使って Copilot に任せましょう。issue を検索して PR を作成し、マージします。

> [!NOTE]
> Copilot が GitHub 上の操作を行う最初の演習です。プロジェクト テンプレートでは GitHub ツールがすでに設定されています。Copilot が初めてツールを使うとき、VS Code で GitHub へのサインインを求められることがあります。表示される手順に従ってください。後の演習で、この接続（Model Context Protocol）の仕組みを確認します。

1. Codespace の Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd>（Mac）または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd>（Windows/Linux）を押して Copilot Chat ビューを開き、エージェント ピッカーで **Agent** が選択されていることを確認します。
3. 命令ファイル更新の issue を探し、現在のブランチから PR を作成して、命令の更新と出版社ヘルパーの基礎の両方を説明するよう Copilot に依頼します。

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot が issue を探して PR を作成します。
5. GitHub 操作の **Allow** を求められたら、コマンドを確認して許可します。
6. PR が作成されたら、次のプロンプトでマージし、ローカル ブランチを `main` に戻すよう依頼します。

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. GitHub 操作やシェル コマンドの **Allow** を求められたら、コマンドを確認して適切に許可します。

GitHub Copilot の支援でプルリクエストを作成してマージできました。

## まとめと次のステップ

このプロジェクトの命令ファイルから Copilot がコンテキストを得る方法を確認し、VS Code の Copilot Chat で次のことを行いました。

- *既存の*命令を使ってコード生成プロンプトを送信し、出力を確認した。
- `.github/copilot-instructions.md` に新しいリポジトリ標準を追加した。
- 同じプロンプトを再実行し、提案されたコードが新しい標準を取り入れることを確認した。
- 命令の更新とヘルパーの基礎を `main` にコミットし、次の演習で使えるようにした。

次は、Copilot がコードベース全体に新機能を追加するときに、[Agent モードで命令を活用します][next-lesson]。

## リソース

- [GitHub Copilot のカスタマイズ用命令ファイル][instruction-files]
- [カスタム命令を作成するためのベスト プラクティス][instructions-best-practices]
- [Copilot 向けのより良いカスタム命令を書くための 5 つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用カスタム命令][personal-instructions]
- [Awesome Copilot — 命令ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
