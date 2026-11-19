---
title: "レッスン 9 - GitHub Copilot CLI のスラッシュ コマンド"
description: "コンテキスト管理、モデル選択、セッション共有、任意のクラウドへの委任に使うスラッシュ コマンドを確認します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

多くの CLI ツールと同様に、GitHub Copilot CLI にも操作に使えるスラッシュ コマンドが多数あります。高度な機能、内部情報、追加の構成オプションを利用できます。これまでに `/diff`、`/mcp`、`/skills`、`/agent`、`/pr` などを使いました。ここでは、ほかの便利なコマンドを確認します。

このレッスンでは、次のことを行います。

- `/context` と `/compact` で Copilot の会話コンテキスト管理を確認する。
- `/model` で利用可能なモデルを確認する。
- `/share` でセッションをエクスポートまたは共有する方法を学ぶ。
- 並行作業、worktree、cloud agent への委任に使える任意のコマンドを確認する。

## シナリオ

CLI の主要なワークフローを終えました。次は、コンテキスト管理、モデルの切り替え、セッションの共有、任意で [Copilot cloud agent][about-cloud-agent] に作業を委任する方法を確認します。

## Copilot CLI のコンテキストを確認する

大規模または複雑なタスクでは、モデルの最大コンテキスト ウィンドウに達する場合があります。Copilot CLI は必要に応じて会話を自動的に圧縮しますが、スラッシュ コマンドで状態を確認したり、自分で圧縮したりできます。

1. Codespace に戻り、Copilot CLI が起動していない場合はリポジトリ ルートから起動します。
2. 次を入力します。

   ```plaintext
   /context
   ```

3. モデル、現在のトークン使用量、システム命令、ツール、メッセージ、空き領域に分割されたコンテキストを確認します。
4. 会話を圧縮します。

   ```plaintext
   /compact
   ```

5. もう一度 `/context` を入力して結果を比べます。会話がすでに短い場合、変化が小さいことがあります。

> [!NOTE]
> コンテキスト ウィンドウが埋まると、Copilot CLI は自動的にコンテキストを圧縮します。自分でタイミングを選ぶ場合は `/compact` を使います。関連しないタスクに切り替えて会話を新しくする場合は `/clear` または `/new` を使います。

## モデルを選ぶ

モデルごとに得意分野が異なり、開発者によって好みも違います。Copilot CLI では、モデルの一覧表示と選択ができます。

1. 次を入力します。

   ```plaintext
   /model
   ```

2. 利用可能なモデルと使用状況に関する情報を確認します。
3. 現在のモデルを使うか、別のモデルを選ぶか、<kbd>Esc</kbd> を押して一覧を閉じます。

## セッションを共有する

チームで協力して学びを共有すると、AI ツールの活用方法を改善できます。`/share` コマンドでは、セッションを Markdown または HTML ファイルにエクスポートしたり、共有リンクを作成したり、GitHub gist を公開したりできます。

1. `/help` を入力し、インストール済みバージョンの `/share` オプションを確認します。
2. セッションを共有する場合は、用途に合った出力先を選びます。たとえば `/share file` はローカルに Markdown ファイルを出力します。
3. エクスポートした内容を誰かに送ったり公開したりする前に確認します。セッションのエクスポートにはプロンプト、応答、プロジェクトの詳細が含まれることがあります。

リンクや gist の公開は任意です。チームが共有するつもりのないリポジトリや会話の内容は公開しないでください。

## 任意: 作業の拡張や委任

ワークショップの中心的な内容は完了しました。Copilot CLI には、大規模なタスクに使えるコマンドもあります。

- `/fleet` は独立したサブタスクを複数のサブエージェントに割り当て、並行して実行します。
- `/worktree` は別のタスク用に独立した Git worktree を作成できます。
- `/delegate` はタスクを Copilot cloud agent に送り、非同期に作業してもらいます。プルリクエストが作成される場合があります。

追加の worktree やリモート作業が発生するため、これらのコマンドは任意です。試す場合は範囲の明確な新しいタスクから始め、通常のワークフローで結果をレビューしてください。非同期エージェントについて詳しく知りたい場合は、[cloud agent ワークショップ][cloud-workshop]に進みます。

## まとめと次のステップ

Copilot CLI のスラッシュ コマンドを使うと、設定やセッション共有を行い、内部状態を確認できます。このレッスンでは、次のことを行いました。

- `/context` と `/compact` で会話コンテキストを確認した。
- `/model` で利用可能なモデルを確認した。
- `/share` でセッションをエクスポートする方法を学んだ。
- 並行作業、worktree、cloud agent への委任に使える任意のコマンドを確認した。

ほかにも多くのスラッシュ コマンドがあります。次は、[学んだ内容を振り返り][next-lesson]、さらに学習を続ける方法を確認しましょう。

## リソース

- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI のコンテキスト管理][context-management]
- [Copilot cloud agent について][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
