---
title: "レッスン 3 - エージェント モード: Plan と Autopilot"
description: "Plan で進め方を合意し、Autopilot で issue に基づくフィルタリングを構築して、Interactive モードで結果をレビューします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

まずは小さな機能をプロジェクトに追加しました。より大きな変更には、さらにしっかりしたプロセスが必要です。GitHub Copilot CLI は組織の既存の開発フローに沿って動作できるため、適切な方法で必要な機能を構築できます。このレッスンから数回にわたり、一般的なエージェント型開発のプロセスに沿って進めます。issue を起点に新機能を作成し、コードの妥当性と機能の動作を確認して、最終的にプロジェクトにマージします。

> [!NOTE]
> 機能開発を続ける間、同じ会話とブランチを使います。通常はファイル種別ごとに異なるブランチや PR を使いますが、ここでは中心的な概念に集中するため簡略化しています。

このレッスンでは、次のことを行います。

- GitHub issue から新しい Copilot の会話を開始する。
- Plan モードで要件を定義する。
- Autopilot モードで新機能を実装する。
- コードをレビューする。
- 転送したブラウザーで機能を手動で検証する。

この機能の作業を続けながら、リポジトリ命令の更新、既存の `quality-checks` スキルのカスタマイズ、MCP 検証、QA エージェントの作成、機能 PR の作成を行います。

## シナリオ

Tailspin Toys のカタログが増えているため、訪問者がカテゴリや発行元でゲームを絞り込めるようにする必要があります。バックログの issue には機能が記載されていますが、カテゴリを組み合わせる方法など、コーディング前に合意が必要な詳細があります。Plan モードで決定事項を整理し、範囲を明確にした実装を Autopilot に任せます。

## 背景

AI コーディング エージェントを開発フローに導入しても、基本は変わりません。むしろ、基本がさらに重要になります。多くの開発者は、次のような流れで作業します。

1. 必要な作業を説明する issue から始める。
2. 実装する内容の計画を立てる。
3. コードを作成してレビューする。
4. テストを実行してコードを検証する。
5. 新機能を手動で検証する。
6. プルリクエスト（PR）を作成する。
7. コードレビューと継続的インテグレーションが成功したら、コードをマージする。

> [!NOTE]
> チームや組織によって詳細は異なりますが、多くの場合は上記の流れを基にしています。

この標準的な進め方に従えば、AI が生成したコードが要件を満たし、手作業で書いたコードと同じレビュー プロセスを通ることを確認できます。

## 会話モード

**会話モード**は、エージェントがどこまで自律的に作業するかを制御します。<kbd>Shift</kbd>+<kbd>Tab</kbd> を押すとモードを切り替えられます。

- **Interactive**: エージェントが変更を提案し、次に進む前にユーザーの入力を待ちます。
- **Plan**: エージェントが最初に計画を作成します。プロジェクト ファイルは編集できません。
- **Autopilot**: エージェントがコードを書き、テストを実行し、タスクが完了するまで自律的に反復します。

まず Plan モードで計画を確認し、その後 Autopilot で実装します。

## issue から始める

フィルタリング作業を始める前に、Codespace に戻り、リポジトリとターミナルを準備します。

1. Codespace に戻ります。停止している場合は、再起動してから続けます。
2. 星評価の PR がマージ済みであることを確認します。
3. ターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>\`</kbd>（Mac）または <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）を押します。
4. `main` を更新して、フィルタリング用ブランチを作成します。

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Copilot CLI を起動します。

   ```bash
   copilot --yolo
   ```

6. <kbd>Tab</kbd> を 2 回押して **Issues** タブを開きます。
7. <kbd>A</kbd> を押してすべての issue を表示します。
8. 矢印キーで **Allow users to filter games by category and publisher** というタイトルの issue を選択します。
9. <kbd>C</kbd> を押して issue をプロンプトに追加し、**Session** タブに戻ります。

プロンプトの先頭に `#7`（または同様の番号）が表示されます。`#` を使うと、GitHub 上の issue やプルリクエスト（PR）をコンテキストに含められます。

## フィルタリング機能を計画する

計画を立てると、機能を実装したりタスクを Copilot に渡したりする前に、進め方を決められます。複雑な作業では、時間をかけて計画を立てることをお勧めします。Plan モードに切り替え、Copilot に計画の作成を依頼します。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Plan モードに切り替えます。プロンプトの下にあるモード表示が **Plan** であることを確認します。
2. 直前の手順で追加した issue の参照に続けて、次のプロンプトを入力します。

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot はプロジェクトを調査し、適切な実装方法を判断して計画を作成します。

3. フィルタリングの動作について質問された場合は、希望に基づいて回答します。唯一の正解はありません。
4. 計画が完成したら、<kbd>Control</kbd>+<kbd>E</kbd>（Mac）または <kbd>Ctrl</kbd>+<kbd>E</kbd>（Windows/Linux）を押して計画を展開します。
5. 計画を読み返します。
6. 決定した内容と合わない箇所があれば、Copilot に修正を依頼します。

## Autopilot を承認する

計画を作成してレビューできたら、実装に進みます。Autopilot を使って Copilot に作業を任せます。

Autopilot では、Copilot が完了したと判断するまで問題に取り組み続けます。

1. **Accept plan and build on autopilot (recommended)**（またはインストール済みバージョンにある同様の名前の選択肢）を選択します。
2. プロンプトの下にあるモード表示が **Autopilot** であることを確認します。
3. Copilot が計画に沿って反復し、コードを生成してテストを実行する様子を確認します。

> [!NOTE]
> 承認すると、すぐに実装が始まる場合があります。まず計画を確認してください。依存関係が不足している、またはポートが競合していると Copilot が報告した場合は、チェック完了と判断する前にセットアップの問題を解決します。

## 実装をレビューして検証する

ほかのコードと同じく、生成されたコードもマージ前にレビューします。コードを確認し、サイトを起動して問題なく動くことも確かめます。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Interactive モードに切り替えます。モード表示が **Plan** または **Autopilot** ではないことを確認します。
2. `/diff` を入力し、フィルタリングの実装とテストを確認します。
3. 結果を issue と計画で決めた内容に照らし合わせます。
4. レビューが終わったら <kbd>Esc</kbd> を押して差分画面を閉じます。
5. プロジェクトのチェック結果を確認し、失敗があれば Copilot に解決を依頼します。

## 新機能を試す

コードは問題なさそうですが、実際に動くでしょうか。前と同じようにアプリを起動し、Codespaces の転送ポートからサイトを開きます。

1. Copilot に次のプロンプトを送ってアプリの起動を依頼します。

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces でポート `4321` が利用可能になったら、**Open in Browser** を選択します。
3. 計画で合意したカテゴリ、発行元、およびその組み合わせでフィルターを試します。
4. リセット時と結果がない場合の動作が、issue と決定事項に合っていることを確認します。
5. Codespace に戻り、Copilot に起動した開発サーバーの停止を依頼します。

## まとめと次のステップ

さまざまな会話モードを使って機能を構築し、レビューしました。このレッスンでは、次のことを行いました。

- GitHub issue から新しい Copilot の会話を開始した。
- Plan モードで要件を定義した。
- Autopilot モードで新機能を実装した。
- コードをレビューした。
- 転送したブラウザーで機能を手動検証した。

次は、[カスタム命令を使って][next-lesson]、コードが文書化された規約に従う方法を確認します。

## リソース

- [GitHub Copilot CLI の Autopilot][autopilot]
- [Copilot CLI コマンド リファレンス][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
