---
title: "演習 3 - Playwright MCP サーバーで機能をテストする"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: Agent モード][previous-lesson] · [次のレッスン: カスタム エージェント →][next-lesson]

Agent モードでフィルタリング機能を作成しました。プルリクエストを開く前に、ブラウザー上で正しく動作することを確認します。アプリを手動で操作する代わりに、**Playwright MCP サーバー**を接続し、Copilot に実際のブラウザーを操作してもらいましょう。その後、ブランチを公開して PR を作成します。

この演習では、次のことを行います。

- Model Context Protocol (MCP) と MCP サーバーが Copilot にツールを追加する仕組みを学ぶ。
- Playwright MCP サーバーをワークスペースに追加する。
- Copilot にブラウザーでフィルタリング機能を手動テストしてもらう。
- ブランチを公開し、フィルタリング作業のプルリクエストを作成する。

## Model Context Protocol (MCP) とは

Agent モードはエディターの外部に接続できると、より強力になります。Model Context Protocol (MCP) は Copilot が外部ツールやサービスと通信するための標準です。

![コンテキスト、LLM、MCP サーバーや VS Code 拡張機能が提供するツールと、Agent モードのやり取りを示す図](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) を使うと、AI エージェントは外部ツールやサービスと通信できます。リソースを使って最新情報にアクセスしたり、ツールを使ってユーザーに代わって操作したりできます。

ツールやリソースには MCP サーバーを介してアクセスします。MCP サーバーは AI エージェントと外部ツール（既存の API や npm パッケージなどのローカル ツール）との通信を管理します。各 MCP サーバーは、利用可能なツールやリソースを提供します。

よく使われる MCP サーバーには、次のものがあります。

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: GitHub リポジトリを管理する API にアクセスします。リポジトリの作成や更新、issue やプルリクエストの管理などを実行できます。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: Playwright によるブラウザー自動化機能を提供します。Web ページの表示、フォームの入力、ボタンの選択などを実行できます。

ほかにもさまざまなツールやリソースにアクセスできる MCP サーバーがあります。GitHub はエコシステム内のサーバーを見つけやすくし、貢献できるように [MCP registry](https://github.com/mcp) をホストしています。

> [!CAUTION]
> セキュリティの観点から、MCP サーバーはほかの依存関係と同じように扱ってください。使用前にソース コードと公開元を慎重に確認し、セキュリティへの影響を検討します。信頼できる MCP サーバーのみを使い、機密リソースや操作へのアクセスを許可する際は注意してください。

## MCP の構成を確認する

`.vscode/mcp.json` ファイルで、この VS Code ワークスペースから利用できる MCP サーバーを設定します。

1. Codespace で `.vscode/mcp.json` を開きます。
2. すでに `github` サーバーが構成されていることを確認します。

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       }
     }
   }
   ```

この `github` エントリはプロジェクト テンプレートに含まれているため、前の演習で Copilot が issue のバックログを読み取れました。[リモート GitHub MCP サーバー][remote-github-mcp-server]を使用しているため、ローカルにインストールする必要はありません。VS Code は HTTP で接続し、Copilot が初めてツールを使うときに GitHub で認証します。

## Playwright MCP サーバーを追加する

2 つ目のサーバーを追加します。[Playwright MCP サーバー][playwright-mcp-server]は Copilot が操作できるブラウザーを提供するため、機能のテストに使えます。

1. `.vscode/mcp.json` の `github` と同じ階層に `playwright` エントリを追加し、ファイルが次のようになるようにします。

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       },
       "playwright": {
         "command": "npx",
         "args": ["@playwright/mcp@latest", "--headless"]
       }
     }
   }
   ```

2. ファイルを保存します。

リモートの `github` サーバーとは異なり、`playwright` は**ローカル** サーバーです。VS Code が `npx @playwright/mcp@latest` を実行して起動します。`--headless` フラグを指定すると、ブラウザー ウィンドウを表示せずに実行できます。デスクトップ環境のない Codespace ではこれが必要です。

> [!NOTE]
> Tailspin Toys ではエンドツーエンド テストにすでに Playwright を使っているため、通常ブラウザーはインストール済みです。後で Copilot がブラウザーが見つからないと報告した場合は、`npx playwright install chromium` を実行して再試行するよう依頼します。

## サーバーを起動して信頼する

VS Code は MCP サーバーを**必要に応じて**起動します。Copilot がサーバーのツールを初めて必要としたときに起動し、信頼するか確認します。

1. `.vscode/mcp.json` で `playwright` エントリの上に表示される **Start** を選択します。サーバーを起動し、設定が正しいことを確認できます。
2. VS Code からサーバーを信頼するか尋ねられたら、構成を確認して信頼することを選択し、サーバーを起動します。

手動で起動する必要はありません。ここで起動しない場合は、次の手順で Copilot が初めてツールを使うときに VS Code が起動し、信頼の確認を表示します。

## フィルタリング機能をテストする

[Playwright MCP サーバー][playwright-mcp-server]を使うと、Copilot が実際のブラウザーを操作できます。ユーザーがアプリをクリックして確認する代わりに、エージェントがページを開き、移動し、フィルターを適用し、結果を読み取って要約します。会話を離れずに機能を確認できます。

Playwright MCP サーバーは、スクリーンショットではなくページの[アクセシビリティ ツリー][playwright-mcp-server]を使います。ボタン、リンク、リスト項目などの構造化されたラベル付き要素を、支援技術と同じように読み取ります。そのため、機能を簡単に確認するだけで、基本的なアクセシビリティも確認できます。

サーバーに接続してアプリを起動したら、次のプロンプトでフィルタリング機能を試します。

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot は Playwright MCP サーバー経由でブラウザーを起動し、各手順を実行して結果を報告します。issue の受け入れ基準に照らして報告を確認してください。問題があれば、プルリクエストを開く前に追加の質問をするか、コードを修正するよう依頼します。

> [!NOTE]
> テストの実行中はアプリが `http://localhost:4321` で起動している必要があります。開発サーバーを停止した場合は、プロンプトを送る前にもう一度起動します。Copilot が Playwright MCP サーバーを初めて使うときは、ブラウザーのダウンロードが必要な場合があります。ブラウザーがないと報告されたら、`npx playwright install chromium` を実行して再試行するよう依頼します。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## ブランチを公開してプルリクエストを作成する

機能が動作することを確認できたので、チームのレビュー用にプルリクエスト（PR）を作成します。まず `filtering-vscode` ブランチを公開します。

1. Codespace の **Source Control** パネルに移動し、Copilot が行った変更を確認します。
2. **+** アイコンを選択して変更をステージします。
3. **Sparkle** ボタンでコミット メッセージを作成します。

   ![変更が表示された Source Control パネルのスクリーンショット](../../../_images/ex3-source-control-changes.png)

4. **Publish** を選択してブランチをリポジトリに push します。

## プルリクエストを作成する

github.com や GitHub コマンド ライン インターフェイス（CLI）など、PR を作成する方法はいくつかあります。GitHub Copilot を使っているので、PR の作成も依頼しましょう。Copilot は関連 issue を見つけて PR に関連付けられます。

1. Copilot Chat パネルに移動し、**New Chat** を選択して新しいセッションを開始します。
2. エージェント ドロップダウンで **Agent** が選択され、Copilot が GitHub ツールを使えることを確認します。
3. 次のプロンプトで PR の作成を依頼します。

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. Copilot が情報を集めて操作を行うため **Continue** を選択します。GitHub ツールを初めて使うときは GitHub への認証を求められることがあります。案内に従って認証します。
5. Copilot が issue を検索して該当するものを見つけ、PR を作成したことを確認します。
6. Copilot が表示したリンクから PR を確認しますが、**まだマージしないでください**。

## まとめと次のステップ

この演習では、次のことを行いました。

- Model Context Protocol (MCP) と MCP サーバーが Copilot にツールを追加する仕組みを学んだ。
- Playwright MCP サーバーをワークスペースに追加した。
- ブラウザーでフィルタリング機能をテストするために使った。
- ブランチを公開し、フィルタリング作業のプルリクエストを作成した。

MCP サーバーで機能を*テスト*しましたが、MCP は Copilot を拡張する方法の 1 つです。次は、アクセシビリティ レビューなどの作業に役立つ[カスタム エージェントを作成します][next-lesson]。

## リソース

- [MCP とは何か、なぜ話題になっているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP Registry][mcp-registry]
- [VS Code の MCP サーバー][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
