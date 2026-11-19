---
title: "演習 5: GitHub Copilot の作業を反復する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

## 作業をレビューする

このラボでは、ユーザー エクスペリエンスと機能を改善する複数のタスクで GitHub Copilot を使いました。コードへのドキュメント追加、デザイン チーム向けの関連ゲーム機能、高コントラストとライト モードを含むアクセシビリティ機能を依頼しました。コードの変更を確認し、必要なら Copilot にフィードバックを与えて改善してから、チームのレビューを有効にします。

### シナリオ

これまでに何度も説明したように、生成 AI を加えてもソフトウェア設計と DevOps の基本は変わりません。生成コードをレビューし、通常の DevOps プロセスを行います。GitHub Copilot が作成したドキュメント、関連ゲーム機能、アクセシビリティ機能を確認してから、チームによるレビューを有効にします。

## GitHub Copilot cloud agent のセキュリティ

Copilot cloud agent は非同期に、監督者なしでタスクを実行するため、安全性を保つためにいくつかの制約があります。

- Copilot はリポジトリへの読み取りアクセスのみを持ち、書き込みできるのは作業に使用するブランチだけです。
- cloud agent は GitHub Actions 内で、作業用の独立した一時環境を作成します。
- GitHub Actions ワークフローの実行には、人間による承認が必要です。
- MCP サーバーを含む[外部リソースへのアクセスは、既定で制限][agent-firewall]されています。

## 生成されたドキュメントをレビューする

まず、GitHub Copilot cloud agent が作成した最初のプルリクエスト（PR）、つまりコードへのドキュメント追加をレビューします。GitHub.com の標準的な PR インターフェイスを使います。

> [!NOTE]
> PR に GitHub Copilot がファイアウォールでブロックされているという警告が表示される場合があります。Copilot は外部 MCP サーバーへの呼び出しを含む外部リソースへのアクセスが制限されているため、これは**想定どおりの動作**です。必要であれば、[Copilot cloud agent のファイアウォールをカスタマイズまたは無効化][agent-firewall]できます。

1. github.com のリポジトリに戻ります。
2. **Pull Requests** を選択して PR の一覧を開きます。
3. **Add missing documentation** などのタイトルのプルリクエストを開きます。

> [!NOTE]
> Copilot がまだ作業中の場合、プルリクエストのタイトルに **[WIP]** が含まれます。その場合は完了するまで待ちます。数分かかることがあるので、休憩したり、これまでに学んだことを振り返ったりしてください。

4. PR の準備ができたら **Files changed** タブを選択し、変更を確認します。

   ![Files changed タブ](../../../_images/shared-pr-files-changed.png)

5. 新たに追加された TSDoc doc comment などのドキュメントを確認します。具体的な変更内容は実行によって異なります。演習 2 の最初に確認したカスタム命令に基づく、TSDoc doc comment、TypeScript の規約、コメント ヘッダーを確認してください。
6. 更新内容のレビューを終えたら **Conversation** タブに戻り、下にスクロールします。
7. ワークフローの実行承認を待っていることを示す表示を探します。
8. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. プルリクエストのチェック欄にワークフローが登録されたことを確認します。問題がなければ、単一 Astro アプリのプロジェクト チェックは成功します。完了まで数分かかる場合があります。

## GitHub Copilot に変更を依頼する

プルリクエストで Copilot と作業するのは一方通行ではありません。チーム メンバーと同じように PR のコメントやコードへのインライン コメントで Copilot にメンションできます。Copilot はコメントを認識し、修正用の新しいセッションを開始します。結果は確率的に変わるため、固定の文面は示せません。たとえば、次のような変更を依頼できます。

- 各コード ファイルの先頭に、役割を簡潔に説明するコメント ヘッダーを追加する。
- TypeScript と Astro ファイルに TSDoc doc comment を追加する。
- Astro アプリの構造を説明する README を作成する。

1. 生成されたドキュメントへの変更を依頼するコメントを追加し、ほかのユーザーと同じように **@copilot** をメンションします。上記の例、または追加したい別のドキュメントの提案を使います。
2. **View Session** を選択して Copilot の作業を確認します。Copilot は変更を行う新しいセッションを開始します。
3. **Back to pull request** を選択して PR に戻れます。

   ![プルリクエストに戻る](../../../_images/ex7-back-to-pr.png)

4. Copilot の作業完了後、プルリクエストに新しいコミットが表示されたことを確認します。
5. **Files changed** タブを選択して変更をレビューします。

納得できるまで反復してかまいません。準備ができたら、ドラフト PR をレビュー可能な状態にして `main` ブランチへマージします。

![PR をレビュー可能な状態にする画面](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

ゲーム詳細ページに関連ゲームを表示する issue に対し、Copilot が作成した PR を確認します。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Show related games on the game details page** などのタイトルの PR を選択します。
4. **Files changed** タブを選択して生成されたコードをレビューします。
5. 内容に問題がなければ **Conversation** タブに戻り、下にスクロールします。
6. ワークフローの実行承認を待っていることを示す表示を探します。
7. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. プルリクエストのチェック欄にワークフローが登録されたことを確認します。問題がなければ、単一 Astro アプリのプロジェクト チェックが成功します。完了まで数分かかる場合があります。
9. **任意:** Codespace でこのブランチに切り替えて関連ゲーム機能を手動でテストできます。Codespace でターミナルを開き、Copilot が作成したブランチ名（例: **copilot/fix-8**）に `<branch-name>` を置き換えて、次のコマンドを実行します。

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

関連ゲーム機能ができました。ほかの PR と同様に、Copilot cloud agent に追加の変更を依頼できます。たとえば、表示する関連ゲームの数を調整したり、コード コメントと TSDoc doc comment を追加したりできます（この issue はカスタム命令を更新する**前**に割り当てたことを思い出してください）。**Conversation** タブにコメントを追加すると、Copilot が認識して新しいセッションを開始します。

## アクセシビリティ機能をレビューする

最後に、アクセシビリティ カスタム エージェントを使って実装した機能をレビューします。この PR には、演習 3 で依頼したハイコントラスト モードと、演習 4 の mission control で追加依頼したライト モードの両方が含まれているはずです。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Add high contrast mode to website** などのタイトルの PR を選択します。

> [!NOTE]
> Copilot がまだ作業中の場合、プルリクエストのタイトルに **[WIP]** が含まれます。完了するまで待ちます。数分かかる場合があります。

4. **Files changed** タブを選択して生成されたコードをレビューします。
5. 実装を確認し、特に次の点に注意します。
   - モード切り替え用の UI コンポーネント
   - ユーザー設定を保存する local storage の利用
   - ハイコントラスト モードとライト モードの CSS またはスタイル変更
   - ARIA ラベルやキーボード操作などのアクセシビリティ属性
   - モード切り替えを管理する JavaScript/TypeScript コード
6. レビューに問題がなければ **Conversation** タブに戻り、下にスクロールします。
7. ワークフローの実行承認を待っていることを示す表示を探します。
8. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. プルリクエストのチェック欄にワークフローが登録されたことを確認します。問題がなければ、単一 Astro アプリのプロジェクト チェックが成功します。完了まで数分かかる場合があります。
10. **任意:** Codespace でブランチに切り替えてアクセシビリティ機能を手動でテストできます。Codespace のターミナルで、`<branch-name>` を Copilot が作成したブランチ名に置き換えて、次のコマンドを実行します。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    その後、アプリケーションを起動し、ブラウザーでハイコントラストとライト モードの切り替えを試します。ページを再読み込みしても設定が保持されることを確認します。

アクセシビリティ カスタム エージェントが、ベスト プラクティスに沿ってこれらの機能を実装するよう Copilot をガイドしたことを確認できます。アクセシビリティ上の問題や改善点があれば、前の PR と同様にコメントで **@copilot** をメンションして変更を依頼します。

## 任意の演習 — 委任を続ける

cloud agent は、実際のバックログ項目を割り当てて、別の作業に集中できるときに効果を発揮します。練習としてリポジトリに issue を追加して Copilot に割り当ててみてください。

- ゲーム詳細ページに支援者向け問い合わせフォームを作成する。
- ゲーム一覧ページにページネーションを実装する。
- データ アクセス ヘルパーに入力検証とエラー処理を追加する。

## まとめ

Cloud agent ハーネスを完了しました。各レッスンを通じて、次のことを行いました。

- **リポジトリに用意されたカスタム命令ファイルを確認し**、cloud agent の出力に与える影響を確認した。
- **issue を Copilot cloud agent に割り当て**、環境を準備し、計画して、非同期に作業する様子を確認した。
- アクセシビリティ用のカスタム エージェントを作成して使い、ハイコントラストとライト モードの切り替えを追加した。
- **Copilot agents ページを mission control として使い**、セッションの進行中に監視して案内した。
- cloud agent のプルリクエストをレビューして反復し、**@copilot** に変更を依頼してワークフローを承認した。

## レビューと次のステップ

Cloud agent ハーネスを完了しました。ほかのハーネスで、今回学んだ内容をさらに試せます。

- 🖥️ **[VS Code ハーネス](../../vscode/)** — IDE 内で Copilot Chat のエージェント モードと MCP 統合を確認します。
- 💻 **[CLI ハーネス](../../cli/)** — Plan と Autopilot モード、エージェント スキル、QA カスタム エージェント、Playwright MCP、Agent Merge、cloud agent に戻る任意の `/delegate` を使い、ターミナルからレビュー済み変更を 2 つ届けます。

自分のリポジトリでは、次の案を試してください。

- リファクタリングやテスト カバレッジの issue を cloud agent に割り当てる。
- 別の分野向けのカスタム エージェントを作成する。
- 異なるスタック向けに `copilot-setup-steps.yml` を設定する。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot agents について][copilot-agents]
- [GitHub issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent セットアップ ワークフローのベスト プラクティス][cloud-agent-best-practices]
- [Copilot cloud agent のファイアウォールを構成する][agent-firewall]

---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall
[previous-lesson]: ../4-managing-agents/
