---
title: "レッスン 4 - カスタム命令で Copilot をガイドする"
description: "リポジトリの命令を確認し、ドキュメント標準を追加して、フィルタリングのコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を使うときはコンテキストが重要です。特定の方法でタスクを進める必要があるなら、そのガイダンスを Copilot が参照できるようにします。[命令ファイル][instruction-files]には、どのようなコードが必要かだけでなく、どのように構成するかも記述できます。フィルタリング機能を構築したので、Copilot が使った命令を確認し、ドキュメント標準を追加して、コードに適用します。

このレッスンでは、次のことを行います。

- リポジトリの命令とパス単位の命令ファイルがエージェントに適用される方法を確認する。
- コーディング標準に従うため、命令ファイルを更新する。
- 命令ファイルがコードに与える影響を確認する。

## シナリオ

優れた開発チームと同様に、Tailspin Toys にも開発プラクティスに関するガイドラインがあります。

- コメントにはコードの内容を言い換えるのではなく、意図や自明でない判断を説明する。
- `db/` と `src/lib/` のエクスポート関数では、`db` 引数を注入できる場合も含め、目的、パラメーター、戻り値を TSDoc/JSDoc で説明する。
- 再利用可能な Astro コンポーネントでは `Props` の契約を説明し、関連するコードの変更時にはコメントも最新に保つ。
- 既存の書式設定と lint のガイダンスを維持する。

命令ファイルを使い、これらのプラクティスに沿ってタスクを実行するための情報を Copilot に渡します。

## 命令ファイル

カスタム命令を使うと、コンテキストや好みを Copilot に渡して、コーディング スタイルや要件を理解させられます。希望する規約、ライブラリ、コメントの種類などを指定できます。リポジトリ全体に適用する命令と、特定のファイル種別に適用する命令を作成できます。

命令ファイルには 2 種類あります。

- `.github/copilot-instructions.md` は、リポジトリに対する**すべての**リクエストで Copilot に送信される単一のファイルです。ほとんどのリクエストに関係するプロジェクト レベルの情報を記載します。
- `.github/instructions/*.instructions.md` は、特定の言語、ファイル種別、タスク向けのガイドラインを記載します。

> [!NOTE]
> ほかの命令形式とサポート状況は環境によって異なります。特定の形式に依存する前に、[カスタム命令のサポート リファレンス][custom-instructions-support]を確認してください。

## このプロジェクトのカスタム命令を確認する

スターター プロジェクトには、すぐに確認できる命令ファイルが含まれています。

1. Codespace に戻ります。
2. Codespaces エディターで `.github/copilot-instructions.md` を開きます（ターミナルではありません）。
3. プロジェクトの説明とコーディング ガイダンスを確認します。これらの命令は、このリポジトリでの Copilot とのすべてのやり取りに適用されます。
4. `.github/instructions` フォルダーを開き、ファイルを確認します。Astro ファイル、Drizzle データ レイヤー、テストなどの命令があります。
5. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドが、適用対象を決める glob を指定しています。
6. `.github/instructions/drizzle.instructions.md` を開き、ほかの命令ファイルや既存プロジェクト ファイルへのリンクを確認します。大きな命令セットを再利用できる小さな単位に分け、Copilot が参照できる例を示せます。

## チームのガイダンスに合わせて命令ファイルを更新する

既存のファイルに不足している点があるため、コアの `copilot-instructions.md` に TypeScript の新しいコードに TSDoc コメントを追加するルールを設定します。

1. `.github/copilot-instructions.md` で **Code formatting guidance** セクション（27 行目前後）を探します。
2. 最後の箇条書きとして、次の行を追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

ファイルは自動的に保存されます。

## 更新したガイダンスを適用する

Copilot CLI は会話の開始時にリポジトリの命令を読み込みます。フィルタリング機能のコンテキストを保ったまま新しい命令を読み込めるよう、セッションを終了して再開します。

1. フィルタリングの会話が開いている場合は `/exit` で終了します。Copilot CLI は会話を自動的に保存します。
2. リポジトリ ルートの `game-filters-cli` ブランチから、保存した会話を再開します。

   ```bash
   copilot --resume
   ```

   選択を求められた場合はフィルタリングの会話を選び、`game-filters-cli` ブランチがチェックアウトされていることを確認します。
3. Copilot に次のプロンプトを送ります。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. `/diff` を入力し、変更された TypeScript ファイルを確認します。新しい TSDoc コメントが追加され、コードを正確に説明していることを確認します。

## まとめと次のステップ

命令ファイルから Copilot がコンテキストを得る方法を確認し、新しい標準を機能に適用しました。このレッスンでは、次のことを行いました。

- リポジトリの命令とパス単位の命令ファイルがエージェントに適用される方法を確認した。
- 命令ファイルを更新してコーディング標準に従うようにした。
- 命令ファイルがコードに与える影響を確認した。

次は、[再利用可能な quality-checks スキルをカスタマイズして実行し][next-lesson]、lint とテストを一貫して実行します。

## リソース

- [Copilot CLI のカスタム命令を追加する][instruction-files]
- [カスタム命令の作成に関するベスト プラクティス][instructions-best-practices]
- [Copilot 向けのより良いカスタム命令を書くためのベスト プラクティス][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
