---
title: "演習 4 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

直前の演習では、Copilot cloud agent にユーザー エクスペリエンスの改善と機能追加を目的とする 3 つのタスクを依頼しました。cloud agent は非同期かつ自律的に作業しますが、タスクの進行状況を監視することも重要です。

GitHub.com の[agents ページ][agents-page]など、cloud agent に割り当てたタスクを管理する方法はいくつかあります。この mission control では、オープンなプルリクエスト（PR）があるタスクを確認し、Copilot が行った操作を調べ、進行中のセッションに案内を追加できます。

このレッスンでは、次のことを行います。

- agents ページから cloud agent のタスクを監視する。
- 実行中のセッションに追加機能を依頼する。

## シナリオ

チームは、エージェントにハイコントラスト モードの作成を依頼した後、ライト モードも追加できることに気付きました。サイトのスタイル更新と切り替え機能をすでに実装しているため、同時に追加するのが合理的です。エージェントに作業を続けてもらい、ハイコントラストに加えてライト モードも実装するよう案内します。

## Copilot cloud agent のタスクを確認する

Copilot cloud agent に割り当てたタスクの現在の状態を確認します。

1. [Copilot agents ページ](https://github.com/copilot/agents)を開きます。
2. メイン ペインと左ペインのタスク一覧を確認します。次のようなタスクが表示されます。
   - コードベースのドキュメントを更新する。
   - ゲーム変更用 API を作成する。
   - Web サイトにハイコントラスト モードを追加する。
3. 実行中のタスクを選択し、Copilot が行った作業を確認します。リポジトリのチェックアウト、環境の作成、MCP サーバーのセットアップ、タスク完了に向けた各種操作などが含まれます。

> [!NOTE]
> 表示される手順は、作業の状態や Copilot が選択した方法によって異なります。

4. 右側に表示されるプルリクエスト（PR）ペインも確認します。PR と変更されたファイルを監視できます。

## cloud agent に追加の指示を出す

実行中のタスクを確認したので、ハイコントラスト モードと一緒にライト モードも実装するよう Copilot に依頼します。

1. ハイコントラスト モードを追加するセッションを選択します。Copilot の付けた名前や作業状態によってタイトルは異なります。

   ![mission control に表示されるアクセシビリティ セッション](../../../_images/ex6-accessibility-session.png)

2. セットアップが完了し、作業を開始したことが表示されるまで数分間セッションを確認します。
3. **Steer active session while Copilot is working** ダイアログに次のプロンプトを入力します。

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![agents ページの cloud agent タスク。Steer active session while Copilot is working ダイアログが強調表示されている](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot が内容を受け取り、作業の流れに追加したことを確認します。

## Copilot の作業を待つ

Copilot は更新されたタスクに取り組みます。メッセージを送信した時点で作業中だった手順を終えた後、新しい依頼を反映します。

完了まで数分かかります。これまでに学んだことを振り返りながら待ちましょう。

## まとめと次のステップ

このレッスンでは、GitHub Copilot cloud agent のタスクを監視、案内する中心的な場所である Copilot agents ページを確認しました。mission control から進行中および完了したタスクを追跡し、実行中の作業をレビューし、進行中のタスクの範囲を調整したり追加のガイダンスを与えたりできます。

このレッスンでは、次のことを行いました。

- mission control と agents ページで cloud agent のタスクを監視した。
- 実行中のセッションに追加機能を依頼した。

Copilot がアクセシビリティ機能を作成している間に、次は作成されたプルリクエストを[反復して改善します][next-lesson]。

## リソース

- [Copilot agents ページ][agents-page]
- [カスタム エージェントについて][custom-agents]

---

| [← 前のレッスン: カスタム エージェント][previous-lesson] | [次のレッスン: Copilot の作業を反復する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
