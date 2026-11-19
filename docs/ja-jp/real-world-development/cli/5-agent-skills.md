---
title: "演習 5 - 品質チェック スキルをカスタマイズして使う"
description: "既存の quality-checks スキルを確認し、レポート形式をカスタマイズして、フィルタリングを検証します。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

コードを書くことだけが開発ではありません。手動でコードを検証し、instruction ファイルを使って規約に従うようにしてきました。では、テストや lint、継続的インテグレーション（CI）のほかの作業はどうでしょうか。

このようなタスクには、**agent skill** が適しています。skill は、こうした操作を適切に実行する方法を Copilot が理解するのに役立ちます。

この演習では、次のことを行います。

- 既存の `quality-checks` skill を確認する。
- 結果の形式をカスタマイズする。
- skill を再読み込みして実行する。

## シナリオ

Tailspin Toys では、単体テスト、lint、型チェックに `quality-checks` skill を使っています。チームは、結果を読みやすくするため、レポートを改善したいと考えています。

## Instructions、スクリプト、リソース

Agent skill は、再利用可能なタスク指示、実行可能なスクリプト、補助リソースをまとめたもので、agent が必要に応じて読み込みます。基本的には、skill 名のフォルダーと、その中の `SKILL.md` で構成されます。Markdown ファイルには、skill の内容と呼び出すタイミングを定義する `name` と `description` の frontmatter、および概要とガイダンスが含まれます。skill のフォルダーには、スクリプトなどのリソース用サブフォルダーも追加できます。

> [!NOTE]
> skill に追加のフォルダーやファイルは必須ではありません。Tailspin Toys の `quality-checks` skill は、プロジェクトにあるコマンドを使うため、`SKILL.md` だけで構成されています。

skill は、チームで共有して再利用できるようプロジェクトの `.github/skills` フォルダーに配置することも、ユーザーの skill フォルダー `~/.copilot/skills` に配置することもできます。

## skill を確認する

Tailspin Toys チームが単体テスト、lint、型チェックの実行用に作成した `quality-checks` skill を確認しましょう。

1. Codespace に戻り、`.github/skills/quality-checks/SKILL.md` を開きます。
2. ファイル先頭の `name` と `description` を読みます。description は、Copilot が skill を呼び出すタイミングを判断するのに役立ちます。
3. 指示を読み、テストと lint の手順をどのように案内しているか確認します。
4. まだ **Results output formatting** セクションがないことに注目してください。

## 変更を加える前に skill を実行する

Copilot CLI から直接、または自然言語を使って skill を呼び出せます。skill に 3 つのチェックを実行するよう依頼しましょう。

1. Interactive mode のフィルタリング会話に戻ります。
2. 次のプロンプトを使います。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 最後に表示されるレポートを確認します。

## レポートをカスタマイズする

実行したチェック、成功したかどうか、ツールが実際に報告した内容がわかるレポートに改善しましょう。

1. `.github/skills/quality-checks/SKILL.md` に戻ります。
2. ファイルの末尾に次のセクションを追加します。

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. ファイルは自動的に保存されます。

## 更新した skill を実行する

変更を加えたので、実際に試してみましょう。Copilot CLI では、会話を再起動せずに編集した skill を再読み込みできます。

1. 次を入力します。

   ```plaintext
   /skills reload
   ```

2. 先ほどとまったく同じプロンプトを使います。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 最後に表示されるレポートを確認し、最初のレポートと比較します。

## まとめと次のステップ

既存の agent skill をカスタマイズして使いました。この演習では、次のことを行いました。

- 単体テスト、lint、型チェック用の `quality-checks` skill を確認した。
- 結果の形式をカスタマイズした。
- skill を再読み込みして実行した。

この変更は、フィルタリング機能の PR に含めます。次は、[Playwright MCP server を使って][next-lesson] サイトと直接やり取りできるようにします。

## その他の skill の例

以下は参考資料であり、追加のタスクではありません。

- [Agent Skills 仕様][skill-spec]
- [コントリビューション ワークフロー: `make-repo-contribution`][contribution-example]
- [要件ドキュメント: `prd`][prd-example]
- [図とバンドルされたエクスポート スクリプト: `drawio`][drawio-example]
- [ブラウザー テスト: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
