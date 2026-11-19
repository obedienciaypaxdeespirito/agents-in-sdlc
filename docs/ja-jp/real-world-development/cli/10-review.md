---
title: "レッスン 10 - まとめと次のステップ"
description: "Copilot CLI のワークフロー、2 つのプルリクエスト、再利用可能なカスタマイズ、参考資料を振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Tailspin Toys の一連のワークフローで GitHub Copilot CLI を使いました。次のことを行いました。

- Codespace を準備し、Copilot CLI をインストールしてプロジェクトを確認し、登録済みのフィルタリング issue を見つけた。
- 星評価を追加し、転送したブラウザーで結果を確認して、最初のプルリクエスト（PR）を手動でマージした。
- フィルタリング issue を起点に、Plan モードで方法を決め、Autopilot モードで実装し、Interactive モードでレビューした。
- カスタム命令でエージェントをガイドし、`quality-checks` スキルをカスタマイズして、ユニット テスト、lint、型チェックを実行した。
- Playwright Model Context Protocol（MCP）サーバーを追加し、実際のブラウザーでフィルタリングを試した。
- 品質保証（QA）カスタム エージェントを作成して選択し、要件、カバレッジ、スキルの結果、ブラウザーの証跡を評価した。
- フィルタリング変更全体をレビューし、フィルタリング PR に対して Agent Merge を承認した。
- コンテキスト、モデル、共有、任意の cloud agent への委任に使えるスラッシュ コマンドを確認した。

## 作成したもの

ワークショップには、それぞれ `main` を更新した状態から作成する 2 つの PR があります。

1. **星評価**: 既存の `starRating` と未評価状態をゲーム カードに表示する。
2. **フィルタリングと品質ワークフロー**: フィルタリングを実装し、命令を更新して機能に適用し、`quality-checks` のレポートをカスタマイズして QA プロファイルと関連テストを追加する。

フィルタリングの計画から PR の作成まで、同じ会話とブランチを使いました。ワークショップを簡略化するため、作業を 1 つの PR にまとめています。

## さまざまな検証方法

自動テスト、自分で行うブラウザー確認、MCP 経由の Copilot によるブラウザー操作でコードを確認しました。`quality-checks` スキルは新しい形式でレポートを作成し、ユニット テスト、lint、型チェックを実行しました。QA はこれらの結果に要件とテスト カバレッジのレビューをまとめました。

追加するテストは実際の不足を補うものであるべきです。QA の実行で追加テストが不要と判断される場合もあります。マージを承認する前にコードと証拠を確認し、変更後は影響する証拠を更新します。

## ベスト プラクティス

Copilot に与えるコンテキストとツールが、作業結果に影響します。このワークショップでは、命令を更新し、スキルをカスタマイズして QA プロファイルを作成し、MCP サーバーを構成しました。これらのカスタマイズは会話をまたいで再利用し、チームのニーズに合わせて調整できます。命令は標準を設定し、スキルは繰り返し行う作業を説明し、カスタム エージェントは専門家の役割を定義し、MCP サーバーは外部ツールに接続します。エージェントの要約だけでなく、実際の変更とツールの結果を確認してください。

タスクに合わせて**モードとモデル**を選びます。構築前に進め方を検討するには **Plan**、範囲を絞った変更に参加しながら取り組むには **Interactive**、範囲が明確なタスクを任せるには **Autopilot** を使います。定型的な編集には高速なモデルを、複雑な作業にはより高性能なモデルを選びます。

インフラストラクチャと同じくらい、コンテキストも重要です。何を、なぜ、どのように構築するのかを明確に説明すると、結果が大きく変わります。

## さらに試す

中心的なワークフローは完了しました。ほかにも、次の CLI 機能を試せます。

- `/review` でコードレビュー エージェントに変更を分析してもらう。
- `/rubber-duck` で問題を話し合い、別の視点を得る。
- `/fleet` で独立したサブタスクを並行して進める。
- `/worktree` で別のタスクを分離する。
- `/delegate` で Copilot cloud agent にタスクを送信する。

## 次のステップ

ツールを上達させる一番の方法は使い続けることです。本番コード、趣味のコード、何年も作りたいと思っていた小さなアプリなどで活用してください。チームと学んだことを共有し、ほかのメンバーから学びましょう。ドキュメントも引き続き確認してください。

ターミナルから Tailspin Toys を拡張したい場合は、任意の [Foundry Backer Concierge シリーズ][foundry]に進みます。別の環境と比較するには、[VS Code ワークショップ][vscode]、[GitHub Copilot app ワークショップ][app]、または [Copilot cloud agent ワークショップ][cloud]を確認します。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI をカスタマイズする][customize-cli]
- [Copilot CLI でプルリクエストを管理する][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
