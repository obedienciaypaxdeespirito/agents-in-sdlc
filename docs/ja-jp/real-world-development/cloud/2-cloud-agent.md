---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタム命令][previous-lesson] |
|:--|

技術的負債に悩む組織は少なくありません。未解決のセキュリティ問題、更新が必要なレガシー コード、実装する時間がなくバックログに残った機能要望などがあります。GitHub Copilot cloud agent は、コードの更新や機能追加などを自律的に実行できます。作業が完了すると、人間の開発者がレビューできるドラフト PR を作成します。時間のかかる作業を任せることで、開発者はより大きな課題に集中できます。

Copilot cloud agent を使って、次のことを確認します。

- コード生成環境をカスタマイズする。
- 操作を安全に行う方法を確認する。
- 範囲を明確にした issue の重要性を理解する。
- issue を Copilot に割り当てる。

## シナリオ

Tailspin Toys には解決したい技術的負債があります。最初のサイトを作成した委託先はドキュメントを残しておらず、アプリケーションにはドキュメントがありません。まず、エクスポートされたすべての関数に TSDoc doc comment を追加します。

また、デザイン チームはゲームを見つけやすくしたいと考えています。各ゲームの詳細ページに、同じカテゴリに属する関連ゲームを表示したいとのことです。まだ完成度の高い実装は必要なく、UX の受け入れテストに使える状態を求めています。この作業は現在ブロックされていますが、優先度の高い issue がほかにあります。

どちらも優先順位が下がりやすいタスクで、Copilot cloud agent に適しています。非同期で作業してもらい、ほかのタスクに取り組んだ後で結果をレビューできます。

## GitHub Copilot cloud agent の概要

[GitHub Copilot cloud agent][copilot-agents] は、人間の開発者と同様にバックグラウンドで作業できます。たとえば [GitHub issue を Copilot に割り当て][assign-issue]ると、Copilot は進行状況を追跡するドラフト プルリクエストを作成し、環境を準備してタスクに着手します。実行中または完了後にセッションを確認できます。提案された解決方法をレビューする準備ができると、プルリクエストで通知されます。

## 範囲を明確にした命令の重要性

GitHub Copilot に魔法はありません。考える作業を省略できるわけではありません。単純に見える操作でも、詳しく調べると複雑なことがあります。[Copilot cloud agent に依頼するタスクの範囲][cloud-agent-best-practices]を慎重に決めてください。AI ペア プログラマーとして扱い、段階的に作業し、学びながら試して調整します。生成 AI を導入しても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム命令

前の演習ではカスタム命令と、その働きを確認しました。まだ `.github/copilot-instructions.md` を見ていない場合は、Copilot cloud agent に作業を割り当てる前に、このリポジトリに用意された命令ファイルを読み、後で影響を確認できるようにしましょう。

リポジトリの GitHub Web UI、または起動済みの Codespace で次のファイルを開きます。

- `.github/copilot-instructions.md`: **Code standards** セクションで TypeScript の規約と TSDoc doc comment の要件を確認します。
- `.github/instructions/unit-tests.instructions.md`: `applyTo` frontmatter が `**/*.test.ts` に命令を適用することを確認します。

次のセクションで `Code lacks documentation` issue を cloud agent に割り当てたら、プルリクエストに TSDoc doc comment、TypeScript の規約、コメント ヘッダーが反映されているか確認します。後の演習で PR をレビューするときにも取り上げます。

## Copilot cloud agent の開発環境を設定する

コードを作成するときは、必要な開発ツールや依存関係を整えるために環境の準備が必要です。Copilot にタスクを割り当てる場合も同様です。cloud agent は SWE と同じように作業します。

cloud agent は作業環境に [GitHub Actions][github-actions] を使います。`.github/workflows/copilot-setup-steps.yml` に [セットアップ ワークフロー][setup-workflow]を作成すると、作業を始める前に開発ツールや依存関係を用意できます。ラボではこの設定をあらかじめ用意し、単一 Astro アプリの Node.js、プロジェクト依存関係、エンドツーエンド テスト用のブラウザー、移行とシードが済んだ SQLite データベースを利用できるようにしています。

```yaml
name: "Copilot Setup Steps"

# リポジトリの「Actions」タブからセットアップ手順をテストできます
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # *手順*の実行に必要な最小限の権限を設定します。Copilot の操作には専用のトークンが使われます。
    permissions:
      # 依存関係のインストールなどでセットアップ手順内にリポジトリのクローンが必要な場合は、`contents: read` 権限が必要です。セットアップ手順でクローンしない場合は、手順の完了後に Copilot が自動的にクローンします。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # フロントエンド / アプリのセットアップ - Node.js（アプリ全体は Astro + Drizzle/libSQL です）
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # ビルドとテストでデータを利用できるよう、ローカル SQLite データベースを移行してシードします。
      - name: Set up the database
        run: npm run db:setup
```

一般的な GitHub ワークフローと同じ形式ですが、次の点に注目してください。

- `copilot-setup-steps` という 1 つのジョブがあります。Copilot が PR で作業を始める前に GitHub Actions で実行されます。
- `workflow_dispatch` トリガーがあり、リポジトリの **Actions** タブから手動でワークフローを実行できます。Copilot の実行を待たずに、ワークフローが成功するか確認できます。

## ドキュメントを追加する

ドキュメントの重要性は広く理解されていますが、情報が古かったり、まったくなかったりするプロジェクトもあります。こうした技術的負債は見過ごされ、生産性を下げ、コードベースの保守や新しい開発者の参加を難しくします。Copilot はドキュメント作成を得意とするため、cloud agent に依頼するのに適したタスクです。後の演習で作業をレビューします。

1. 新しいブラウザー タブで github.com のリポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択します。
4. **Blank issue** を選択します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次を入力します。

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

7. **Create** を選択して issue を作成します。
8. 右側の **Assign to Copilot** を選択して割り当てダイアログを開きます。

   ![issue に Copilot を割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。
10. **Pull Requests** タブを選択します。
11. `[WIP]: Code lacks documentation` のようなタイトルのプルリクエスト（PR）を開きます。一覧に PR が表示されない場合は、少し待ってからブラウザーを更新します。
12. 数分後、Copilot が ToDo リストを作成したことを確認します。

> [!NOTE]
> Copilot が ToDo リストを作成するまで数分かかることがあります。Copilot は環境を準備し（前述のワークフローを実行）、プロジェクトを分析して、タスクへの取り組み方を決めています。

13. リストと、実行予定の作業を確認します。
14. プルリクエストのタイムラインを下にスクロールし、Copilot が issue に取り組み始めたことを示す更新を確認します。
15. **View session** ボタンを選択します。

   ![Copilot のセッション ビュー](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 表示を更新しないと、状態の変更が見えない場合があります。

16. ライブ セッションを読み、Copilot がコードを調査して状態を把握し、計画を考えてコードを作成する様子を確認します。

しばらく時間がかかります。cloud agent の主な目的は、非同期にタスクを実行して、ほかの作業に集中できるようにすることです。もう 1 つのタスクを cloud agent に割り当てた後、アプリケーションへの機能追加に取り組みます。

## ゲーム詳細ページに関連ゲームを追加する

GitHub Copilot cloud agent の利点の 1 つは、作業を分けられることです。デザイン チーム向けの関連ゲーム機能は時間がかからないかもしれませんが、その間に別の作業ができます。Copilot cloud agent に割り当てましょう。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択します。
4. 空のテンプレートを使うため、**Blank issue** を選択します。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次を入力します。

   ```markdown
   We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

7. **Create** を選択して issue を作成します。
8. 右側の **Assign to Copilot** を選択して割り当てダイアログを開きます。

   ![issue に Copilot を割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

その後、issue の最初のコメントに 👀 が表示され、Copilot が作業を開始したことがわかります。

![Copilot が作業中であることを示すために目の絵文字を使っている画面](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は 2 つ目の依頼に取り組んでいます。cloud agent は SWE と同様に動くため、常に監視する必要はありません。完了後にレビューできます。続けて、カスタム エージェントの作成と使用を確認します。

## まとめと次のステップ

[GitHub Copilot cloud agent][copilot-agents] を使うと、issue を割り当てて非同期に作業を依頼できます。技術的負債の解消、新機能の作成、別のフレームワークへのコード移行などに利用できます。

このレッスンでは、次のことを確認しました。

- コード生成環境をカスタマイズする。
- 操作を安全に行う方法を確認する。
- 範囲を明確にした issue の重要性を理解する。
- issue を Copilot に割り当てる。

cloud agent がバックグラウンドで作業している間に、次はカスタム エージェントを作成して使います。[Copilot cloud agent は MCP サーバー][cloud-agent-mcp]とカスタム命令も利用できます。カスタム命令は前のモジュールで確認しました。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent セットアップ ワークフローのベスト プラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタム エージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
