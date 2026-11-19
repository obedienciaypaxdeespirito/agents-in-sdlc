---
title: "演習 1 - GitHub Copilot CLI をインストールする"
description: "Codespace で Copilot CLI をインストールして認証し、基本操作を確認して、最初に取り組む issue を見つけます。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

[GitHub Copilot CLI][about-copilot-cli] は、ターミナルで動作する強力なエージェント型コーディング アシスタントです。コードベースの探索、コード生成、コマンド実行、外部ツールとの連携をコマンド ラインから行えます。タスクを任せたり、変更を依頼したりしながら、作業に集中できます。最初のステップはツールのインストールです。すでに使い慣れたツールでインストールできます。

この演習では、次のことを行います。

- npm を使って GitHub Copilot CLI をインストールする。
- GitHub アカウントで認証する。
- ワークショップのリポジトリを信頼し、簡単な会話を試す。
- 組み込みの GitHub MCP server を使ってフィルタリング issue を見つける。

## シナリオ

チームでは、増え続けるバックログに対応するため、AI agent を使い始めています。Copilot CLI は、多くの開発者が日常的に使うターミナルにその機能をもたらします。この演習では、インストールと認証を済ませ、ワークショップの残りで利用できる状態にします。

## Copilot CLI をインストールする

Copilot CLI は [npm][install-cli]、WinGet、Homebrew からインストールできます。GitHub Codespaces には Node.js があらかじめインストールされているため、ここでは npm を使います。

1. Codespace に戻り、ターミナルを開きます。
2. Node.js がインストールされており、バージョン要件を満たしていることを確認します。

   ```bash
   node --version
   ```

   バージョン 24 以上が表示されます。

3. Copilot CLI をグローバルにインストールします。

   ```bash
   npm install -g @github/copilot
   ```

4. インストールを確認します。

   ```bash
   copilot --version
   ```

   バージョン番号が表示されます。

## GitHub で認証する

初回起動時に、Copilot CLI は GitHub アカウントでの認証を求めます。

1. Copilot CLI を起動します。

   ```bash
   copilot
   ```

2. 指示が表示されたら、device code を使って認証し、Copilot CLI を承認します。
3. Copilot CLI に次のプロンプトが表示されます。

   ```plaintext
   Copilot can read files in this folder and, with your permission, edit them or run code and shell commands. It will remember your permissions for the rest of this session.

   Do you trust the files in this folder?
   ```

4. 表示されているパスが Tailspin Toys リポジトリであることを確認してから、**Yes, and remember this folder for future sessions** を選択します。

> [!NOTE]
> Codespace では、GitHub セッションを通じてすでに認証されている場合があります。Copilot CLI が認証を求めずに起動した場合は、そのまま進めてください。

## 基本操作を確認する

通常の shell プロンプトで入力したコマンドは、Codespace で直接実行されます。Copilot CLI を起動すると、自然言語は agent に送られ、slash command は会話を操作します。

1. `/model` を入力し、矢印キーで **Auto** を選択して <kbd>Enter</kbd> を押し、もう一度 <kbd>Enter</kbd> を押して確定します。
2. `/help` を入力して、インストールされているバージョンで利用できるコマンドを確認し、<kbd>Esc</kbd> を押してヘルプ画面を閉じます。
3. Copilot が正しく動作していることを確認するため、簡単な質問をします。

   ```plaintext
   What are the key files in this project?
   ```

4. 応答を読み、Copilot が回答前にリポジトリを探索することを確認します。
5. `/mcp list` を入力し、組み込みの GitHub MCP server が利用できることを確認します。
6. Copilot にフィルタリング issue を探すよう依頼します。

   ```plaintext
   Using GitHub MCP, find the issue in this repository titled "Allow users to filter games by category and publisher." Give me its URL and a short summary. Don't change anything.
   ```

7. 表示された URL を開き、issue を読みます。最初の変更に取りかかる前に、この issue を使います。

> [!TIP]
> 通常の Copilot CLI セッションは、ターミナルで現在チェックアウトされている branch で動作し、自動的に worktree を作成することはありません。各変更の前に feature branch を作成します。

## ワークショップ用のショートカットを使う

Copilot CLI は通常、許可されている範囲外のツールを使う前に確認します。このワークショップでは `--yolo` を付けて再起動します。ユーザーが許可したこのショートカットを使うと、Codespace 内の承認プロンプトを省略でき、演習に集中できます。

> [!CAUTION]
> `--yolo` は、すべての自動権限（`--allow-all-tools`、`--allow-all-paths`、`--allow-all-urls`）を有効にします。Codespace や VM などの分離された環境でのみ使用し、日常の開発で既定値として alias に設定しないでください。詳しくは[ツールの使用を許可または拒否する][allow-all-warning]を参照してください。

このワークショップでは `--enable-all-github-mcp-tools` を使い、後の演習で issue や pull request を操作するための、読み取り / 書き込み用 GitHub MCP tool を有効にします。Codespace によってローカル コンピューターへのアクセスは制限されますが、認証済みの GitHub リソースは実際のものです。公開またはマージする前に変更を確認してください。

1. `/exit` で Copilot CLI を終了します。
2. リポジトリのルートから再起動します。

   ```bash
   copilot --yolo --enable-all-github-mcp-tools
   ```

3. プロジェクトについて簡単な質問をし、会話が動作することを確認してから `/exit` で終了します。

Copilot は会話を自動的に保存します。後の演習で instruction を変更したり agent を追加したりした後、`copilot --resume` を使って同じ feature の会話と branch に戻ります。

## まとめと次のステップ

この演習では、次のことを行いました。

- npm を使って GitHub Copilot CLI をインストールした。
- GitHub アカウントで認証した。
- ワークショップのリポジトリを信頼し、簡単な会話を試した。
- 組み込みの GitHub MCP server を使ってフィルタリング issue を見つけた。

次は、[最初の対象を絞った変更][next-lesson]に取りかかり、ゲーム カードに星評価を表示します。

## リソース

- [GitHub Copilot CLI をインストールする][install-cli]
- [GitHub Copilot CLI について][about-copilot-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-add-star-rating/
[install-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/set-up-copilot-cli/install-copilot-cli
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[allow-all-warning]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli#allowing-and-denying-tool-use
