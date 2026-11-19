---
title: "レッスン 12 - GitHub Pages でサイトをデプロイする"
description: "GitHub Copilot app で Pages ワークフローを構成し、そのプルリクエストをレビューして公開された Space Quiz を確認します。"
authors:
  - jamesmontemagno
lastUpdated: 2026-10-12
---

GitHub Pages を使って Space Quiz を公開しましょう。Copilot はワークフローの準備とデプロイを案内できますが、変更をレビューし、サイトが公開されたことを確認するのはユーザーの役割です。

このレッスンでは、次のことを行います。

- Copilot に GitHub Pages のデプロイを構成してもらう。
- マージ前にワークフローとプルリクエストをレビューする。
- デプロイの状況を確認し、公開されたサイトを検証する。

## Copilot にデプロイの準備を依頼する

> [!WARNING]
> プルリクエストを作成すると、コードがコミットおよび push されることがあります。プロンプトを送る前に、承認設定と変更先のリポジトリを確認してください。**Approve all** を選んでいる場合、個別の確認プロンプトが表示されないことがあります。

`space-quiz` の Copilot app セッションで、次のプロンプトを送信します。

```plaintext
Prepare a GitHub Pages deployment for this repository. Create a GitHub Actions workflow that builds the website if needed and deploys it to GitHub Pages after changes reach the default branch. Add a README describing the app and how to run it locally if one does not already exist. Create a pull request with these changes and report its link and any required Pages settings or permissions. Do not merge, enable auto-merge, change Pages settings, or deploy. Stop and wait for my review.
```

## ワークフローとプルリクエストをレビューする

1. README は、リポジトリにまだ存在しない場合にだけ追加されていることを確認します。
2. ワークフローのビルドとデプロイ手順、トリガー、権限を含めて、プルリクエストの変更をレビューします。
3. ワークフローが、マージされていないプルリクエストではなく既定のブランチからデプロイすることを確認します。
4. 修正が必要であれば依頼し、更新された差分をレビューします。マージを許可する前に、必須のプルリクエスト チェックが成功するまで待ちます。

## マージとデプロイを許可する

> [!WARNING]
> 次のプロンプトを送信すると、レビュー済みプルリクエストのマージ、リポジトリの Pages 設定の変更、サイトの公開をエージェントに許可します。レビューを完了し、リポジトリとプルリクエストを確認してから送信してください。承認設定によっては、再確認なしでこれらの操作が実行されます。

`<PR_NUMBER>` をレビューしたプルリクエストの番号に置き換え、次のプロンプトを別に送信します。

```plaintext
I have reviewed pull request <PR_NUMBER> in this repository and authorize its merge and deployment. Enable GitHub Pages using GitHub Actions, merge that pull request only after required checks pass, and monitor the deployment until it succeeds. Report the final URL of the live website. If you lack permission to enable Pages or merge, tell me exactly what I need to approve or do; do not report success until the site is live.
```

![マージ済みの space-quiz プルリクエスト、Build website、Upload artifact、Deploy to Pages の各ステップが成功した GitHub Actions の Deploy website 実行、yourname.github.io/space-quiz/ で公開された Space Quiz を示す 3 段階の図。](../../../_images/first-steps-app-pages-deploy.svg)

## デプロイを検証する

1. GitHub Actions のデプロイ実行が成功するまで確認します。
2. 報告された URL を開き、Space Quiz が表示されることを確認します。

エージェントが Pages の有効化やプルリクエストのマージを実行できない場合は、案内された手順に従います。デプロイが成功し、公開サイトが読み込まれるまで完了と判断しないでください。

## まとめと次のステップ

Space Quiz が公開され、エージェントの報告に頼らず、実際にサイトを確認できました。[レッスン 13: 振り返りと次のステップ][next-lesson]に進みます。

[next-lesson]: ../13-review/
