---
title: "レッスン 8 - 機能 PR を作成してマージする"
description: "フィルタリング、命令、スキル、QA プロファイル、テストをまとめてレビューし、PR を作成して Agent Merge を使います。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

フィルタリングの実装、命令の更新、スキルの更新、品質保証（QA）プロファイル、テストは、すべて 1 つのブランチに保存されています。まとめてレビューし、プルリクエストを作成しましょう。星評価のプルリクエスト（PR）は自分でマージしました。今回は **Agent Merge** にマージ プロセスを任せます。

> [!NOTE]
> 通常は機能、命令、スキル、QA エージェントを複数の PR に分けます。このワークショップでは、流れを簡略化するため、フィルタリングと品質確認の作業を 1 つの会話とブランチにまとめました。

このレッスンでは、次のことを行います。

- Agent Merge と、マージ ライフサイクルの自動化方法を確認する。
- 機能全体の変更と検証結果を確認する。
- フィルタリングの PR を作成する。
- レビュー後に Agent Merge を有効にし、PR がマージされたことを確認する。

## シナリオ

フィルタリング機能を進める中で、Copilot を使って計画、実装、検証を行いました。Tailspin Toys では、開発者がマージを承認する権限を保ったまま、残りの PR 作業を自動化したいと考えています。

## Agent Merge とは

**Agent Merge** は、プルリクエストをマージ可能にするための残りの作業を自動化します。有効にすると、Copilot は PR をブロックしている問題（失敗した継続的インテグレーション（CI）チェックの修正、レビュー コメントへの対応、必要に応じたリベースなど）に取り組みます。その後、リポジトリで許可されていれば GitHub の自動マージを有効にします。

これまでは自分で **Merge pull request** を選択していました。Agent Merge に任せることもできますが、有効にする前に作業をレビューし、準備ができていることを確認します。

## Agent Merge で PR を管理する

すべてのコードを作成できたので、まとめてレビューし、PR を作成して、残りのプロセスを Agent Merge に任せます。

1. Codespace に戻ります。
2. `/agent` を入力してエージェント ダイアログを開きます。
3. 一覧から **Default** を選択し、<kbd>Enter</kbd> を押します。
4. `/pr create` コマンドで新しい PR を作成します。
5. `/pr agentmerge` を実行して Agent Merge を有効にします。
6. Copilot が PR の継続的インテグレーションを確認します。すべて成功すると、マージが実行されます。
7. Copilot から「PR #14 was squash-merged successfully.」のようなメッセージが表示されたことを確認します。

> [!IMPORTANT]
> Agent Merge は、必須の承認、ブランチ保護、マージ キュー、リポジトリ設定、権限の不足を回避しません。ブロックされた場合は報告内容を確認し、リポジトリで許可されていればレビュー済みのマージを手動で完了します。

## まとめと次のステップ

コードの生成とテスト、検証に加えて、プルリクエストのプロセスも自動化しました。このレッスンでは、次のことを行いました。

- Agent Merge と、マージ ライフサイクルを自動化する方法を確認した。
- 機能全体の変更と検証結果を確認した。
- フィルタリングの PR を作成した。
- レビュー後に Agent Merge を有効にし、PR がマージされたことを確認した。

次は、コンテキスト、モデル、共有、任意のクラウドへの委任に使える[便利な Copilot CLI スラッシュ コマンドを確認します][next-lesson]。

## リソース

- [Copilot CLI でプルリクエストを管理する][manage-prs]
- [Copilot CLI コマンド リファレンス][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
