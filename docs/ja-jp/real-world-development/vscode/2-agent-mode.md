---
title: "演習 2 - Copilot Agent モードで新機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

アプリケーションへの簡単な更新でも、複数のファイルを変更し、テストを実行する必要があります。開発者は、関連ファイルを探し、変更を行い、テストし、デバッグし、変更し忘れたファイルを見つけ、さらに修正するという作業を繰り返します。

このような作業に役立つのが Copilot Agent モードです。

Copilot Agent モードは IDE 内でより自律的に作業します。開発者と同様に、既存のプロジェクト構造を調査し、必要な変更を行い、テストなどのタスクを実行し、見つかった問題を自動的に修正します。ここでは Agent モードを使ってサイトに機能を追加します。

> [!NOTE]
> 名前は似ていますが、Agent モードと cloud agent は異なる用途向けに作られています。Agent モードは IDE 内で作業し、短いフィードバック サイクルと対話を可能にします。Cloud agent はチーム メンバーのように非同期で作業し、issue とプルリクエストを通じてやり取りします。

この演習では、次の方法を学びます。

- Copilot Agent モードでプロジェクトを調査し、関連ファイルを特定して、複数ファイルにまたがる変更を行う。
- UI とデータ レイヤーにまたがる新機能を実装する。
- コードベースにマージする前に、生成された変更とテストをレビューする。

## シナリオ

ゲームの一覧が増えてきたため、カテゴリと発行元で絞り込めるようにします。前の演習では出版社ヘルパーを追加しました。残りのデータ レイヤー、UI、テストの作業を Copilot Agent モードで完了します。

## Tailspin Toys の Web サイトを実行する

変更を始める前に、Tailspin Toys の現在の機能を確認します。

この Web サイトは、開発者をテーマにしたボードゲームのクラウドファンディング プラットフォームです。ゲームを一覧表示し、詳細を表示できます。単一の Astro アプリで、ビルド時にページを静的 HTML として生成します。ページは frontmatter 内で Drizzle ORM を使ってローカル SQLite データベースを直接参照します。独立したバックエンド API やクライアント側 UI フレームワークはありません。再利用可能なデータ アクセス ヘルパーは `src/lib/` に置き、インタラクティブな動作には標準 DOM API を使う小さな Astro `<script>` を使います。

### Web サイトを起動する

単一の Astro 開発サーバーを起動する `npm run dev` タスクが用意されています。

1. Codespace に戻ります。前の演習の後で `main` に戻っているはずです。
2. <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）または <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）を押して、新しいターミナルを開きます。
3. フィルタリング作業用のブランチを作成して切り替えます。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 次のコマンドで Web サイトを起動します。

   ```bash
   npm run dev
   ```

   Astro 開発サーバーの準備ができると、次のような URL が表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> `http://localhost:4321` を開くよう促すダイアログが表示された場合は、**x** を選択して閉じます。

5. ターミナルに表示された Astro サーバーのアドレス `http://localhost:4321` を、Mac では <kbd>Command</kbd>、Windows/Linux では <kbd>Ctrl</kbd> を押しながら選択して Web サイトを開きます。

> [!NOTE]
> Codespace のターミナルで localhost のリンクを選択すると、`https://<your-codespace-name>-4321.app.github.dev/` にリダイレクトされます。Codespace に安全に接続するプライベート トンネルを通じて Web サーバーにアクセスしています。

### Web サイトを確認する

Web サイトを起動したら、機能を確認します。

- **ホーム ページ**: タイトル、画像、説明を含むボードゲーム一覧を表示します。
- **ゲーム詳細ページ**: ゲームを選択すると、タイトル、説明、発行元、カテゴリなどの情報を表示します。

## Copilot でバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> 次の演習を始める前に Codespace に戻り、Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習ごとに異なります。必要な箇所で説明します。

Web サイトの初期実装は動作しますが、新しい機能を追加したいと考えています。まずバックログを確認しましょう。テンプレートからプロジェクトをセットアップすると issue のバックログも自動的に作成されています。GitHub Copilot に項目を表示してもらいます。

1. Chat ビューのエージェント ドロップダウンから **Agent** を選びます。Agent は自律的に計画し、複数ファイルに変更を加え、ターミナル コマンドやツールを実行します。

   ![Chat ビューのエージェント ピッカーのスクリーンショット](../../../_images/shared-chat-mode-selector.png)

2. 使用可能なモデル一覧から **Claude Sonnet 4.5** を選択します。

> [!CAUTION]
> このワークショップの作成者が特定のモデルを推奨するものではありません。作成時に Claude Sonnet 4.5 を使用したため、手順でも同じモデルを使っています。提案の一貫性を高め、学習しやすくするためです。LLM は確率的に動作するため、ワークショップの例と異なる提案が返る場合があります。これは正常な動作です。

> [!NOTE]
> LLM の確率的な性質により、Copilot は別の MCP コマンドを使うことがありますが、タスクは同様に完了できるはずです。

3. 次のプロンプトを送信し、バックログを確認します。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. **Continue** を選択して、すべての issue を一覧表示するコマンドを実行します。
5. Copilot が作成した issue の一覧を確認します。

Copilot はユーザーに最も役立つと思われる項目を優先順位付けします。

## 命令ファイルを確認する

コード生成をエージェントに依頼する前に、作業のコンテキストを与える命令ファイルを確認します。Web サイトへの機能追加方法を説明する [ユーザー インターフェイス (UI) 命令][ui-instructions]を使います。

1. Codespace で `.github/instructions/ui.instructions.md` を開きます。
2. 機能追加に関する全体的なガイダンスを確認します。アーキテクチャの概要、コンポーネントの設計、テスト容易性、アクセシビリティの原則、次のファイル種別向けの命令へのリンクが含まれます。
   - Astro
   - Tailwind CSS
   - Drizzle

> [!TIP]
> 命令ファイルから、ほかの命令ファイルやプロジェクト内のファイルを参照できます。パスは命令ファイルからの相対パスです。これにより、命令を再利用し、複雑な内容を小さな単位に分け、例やテンプレートを示せます。

## フィルタリング機能を実装する

フィルタリングを完成させるには、少なくとも次の 3 つを更新します。

- データ レイヤー（`src/lib/`）の残りのフィルタリング ロジックを追加または調整する。
- フィルタリング動作のテストを追加または更新する。
- ゲーム一覧ページにフィルタリング UI を追加する。

コードベースにマージする前にテストを実行し、すべて成功させる必要があります。Copilot Agent モードにこれらの作業を依頼します。

1. Copilot の現在の会話を続けるか、**New Chat** を選択して新しい会話を始めます。
2. **Add Context**、**Instructions**、**ui** の順に選択して命令ファイルを指定します。

   ![UI 命令ファイルを選択する例のスクリーンショット](../../../_images/ex3-select-instructions-file.png)

3. Chat ビューのエージェント ドロップダウンで **Agent** が選択されていることを確認します。

   ![Chat ビューのエージェント ピッカーのスクリーンショット](../../../_images/shared-chat-mode-selector.png)

4. モデルが **Claude Sonnet 4.5** のままであることを確認します。
5. 関連するバックログ issue に基づいて機能を実装するよう、次のプロンプトを送信します。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot がプロジェクトを調査し、必要なファイルを特定して、データ レイヤー、UI、テストを変更し、テストを実行する様子を確認します。

   ![Copilot がプロジェクト ファイルを調査するスクリーンショット](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot はプロジェクトを調査し、ファイルを編集してテストを実行します。タスクやコードベースの複雑さによっては数分かかります。作業中、コード エディターに **Keep** と **Undo** が表示されることがあります。完了するとすべての変更に **Keep** または **Undo** が表示されるため、処理中に選択する必要はありません。

7. Copilot が求めたら **Continue** を選択してテストを実行します。

   ![Copilot Chat でテスト実行の確認を求めるダイアログ](../../../_images/ex3-agent-mode-run-tests.png)

8. 処理中に一時停止したり、テストが失敗したりすることがあります。Copilot はコード生成とテストを繰り返し、エラーが見つからなくなるまで作業します。

   ![Copilot Agent モードのチャット セッションが完了した画面](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 生成されたコードに問題がないか確認します。

> [!CAUTION]
> Copilot やほかの AI ツールが生成したコードは、必ずレビューしてください。

10. Web サイトを表示しているブラウザーに戻り、新機能を試します。
11. 動作とコードを確認したら、Copilot Chat ウィンドウで **Keep** を選択します。

## まとめと次のステップ

Copilot Agent モードを使って Tailspin Toys の Web サイトに機能を追加しました。この演習では、次のことを学びました。

- UI とデータ レイヤーにまたがる新機能を実装する方法。
- Agent モードがプロジェクトを調査し、関連ファイルを特定して、連携した変更を行う方法。
- コードベースにマージする前に、生成された変更とテストをレビューする方法。

次は [Playwright MCP サーバーで機能をテストし][next-lesson]、プルリクエストを作成します。

### ボーナスの演習 — ページ分割を実装する

ゲーム一覧が増えるとページ分割が必要になります。この演習で学んだことを使い、Copilot にページ分割の実装を依頼します。次の点を検討してください。

- 既存の命令ファイルを含むベスト プラクティスに従う。
- ページ サイズをユーザーが選べるようにするか、固定にするかを決める。
- 希望する実装に必要なガイダンスを、プロンプトに含める。
- Copilot に変更やコンテキストを追加しながら反復する。これは通常の作業方法です。

## リソース

- [Copilot の Ask、Edit、Agent モード: 機能と使い分け][choose-mode]
- [VS Code の Agent モード][vs-code-agent-mode]

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
