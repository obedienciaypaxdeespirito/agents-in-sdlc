---
title: "演習 4 - カスタム エージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタム エージェントとは

GitHub Copilot の[カスタム エージェント][custom-agents-concept]を使うと、開発ワークフローの特定のタスクや分野に合わせた AI アシスタントを作成できます。リポジトリの `.github/agents` フォルダーに Markdown ファイルを置き、命令、ベスト プラクティス、コーディング パターン、専門知識を定義すると、その種類の作業を効果的に実行できます。チームの知識を、[WCAG][wcag] に準拠するアクセシビリティ エージェント、安全なコーディングを支援するセキュリティ エージェント、一貫したテスト パターンを守るテスト エージェントなどとして再利用できます。

カスタム エージェントは、プロジェクト内の `.github/agents` またはグローバルな `~/.copilot/agents` にある Markdown ファイルで定義します。各ファイルには少なくとも `name` と `description` を持つ YAML frontmatter を記述し、その後にエージェントの動作、専門分野、命令を Markdown で記述します。

### カスタム エージェントとエージェント スキルの違い

カスタム エージェントと[エージェント スキル][agent-skills-concept]には重なる部分があります。どちらも主に Markdown ファイルで定義し、AI に作業方法を伝えます。わかりやすく区別すると、**カスタム エージェントは作業者、スキルはツール**です。

カスタム エージェントには独自のコンテキスト ウィンドウがあり、作業の一部としてスキルやほかのエージェントをまとめて利用できます。このラボのアクセシビリティ カスタム エージェントは、アクセシビリティ ガイドラインに沿ってサイトをレビュー、更新します。その作業の中で、プルリクエスト ワークフローやテスト実行・管理のスキルを呼び出すこともできます。

> [!NOTE]
> カスタム エージェントを作成する唯一の正解はありません。AI のほかの使い方と同様に、環境やシナリオに合わせて試し、改善してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、すべてのユーザーがクラウドファンディング プラットフォームを利用できるようにしたいと考えています。最近のフィードバックによると、文字色と背景色のコントラストが不十分で、現在のダーク テーマを読みづらいと感じるユーザーがいます。この問題に対処するため、デザイン チームは切り替え可能なハイコントラスト モードを求めています。

アクセシビリティは重要なため、カスタム エージェントを使って機能をすばやく作成します。

この演習では、次のことを行います。

- 既存のアクセシビリティ カスタム エージェントを確認する。
- Copilot Chat でアクセシビリティ エージェントを使い、ハイコントラスト モードを実装する。

## アクセシビリティ カスタム エージェントを確認する

アクセシビリティ用のカスタム エージェントはすでに作成されています。内容を確認して、Copilot をどのようにガイドするか把握します。

Codespace に戻り、アクセシビリティの作業用に `main` から新しいブランチを作成します。フィルタリング PR は演習 3 と分けておきます。

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. `.github/agents/accessibility.md` を開きます。
2. `name` と `description` フィールドがある YAML frontmatter を確認します。

> [!CAUTION]
> カスタム エージェントでは、`name` と `description` を含む frontmatter が必須です。

3. 続くセクションを読みます。アクセシブルな Web サイトを作成する際の主な役割、アクセシビリティのベスト プラクティス、HTML/CSS/JavaScript のコード例、よくある問題を確認します。

## Copilot Chat でカスタム エージェントを使う

VS Code では、`.github/agents` で定義したカスタム エージェントが Copilot Chat ビュー下部のエージェント ドロップダウンに表示されます。エージェントを選択すると、そのエージェントの命令とツールに合わせてチャット セッションが設定されます。

> [!TIP]
> **Copilot Chat を開く**
>
> 次の演習を始める前に Codespace に戻り、Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習ごとに異なります。必要な箇所で説明します。

1. Chat ビューのエージェント ドロップダウンで **Agent** を選択します（未選択の場合）。

   ![Chat ビューのエージェント ピッカーのスクリーンショット](../../../_images/shared-chat-mode-selector.png)

2. Chat ビューの下部にあるエージェント ドロップダウンを選択します。既定では、現在のエージェント（**default**）が表示されます。
3. 使用可能なエージェント一覧から **Accessibility agent** を選択します。
4. 次のプロンプトをアクセシビリティ エージェントに送信します。

   ```plaintext
   Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
   ```

5. Copilot Chat がコードベースを調査し、変更を提案してプロジェクト ファイルを編集します。編集ごとに、ファイル パスとレビュー可能な差分がチャットに表示されます。
6. エージェントが変更した作業対象ファイルの一覧は **Files changed** の表示で確認できます。

> [!NOTE]
> この作業には数分かかる場合があります。Copilot はリポジトリに実際の変更を加え、Astro コンポーネント、CSS、関連テストなど既存のファイルを編集します。

次の演習で、進行中の作業をレビューして案内します。

## まとめと次のステップ

[GitHub Copilot のカスタム エージェント][custom-agents]は、特定のタスクや分野に合わせた専門 AI アシスタントです。チームの知識と標準を再利用できるエージェントとしてまとめ、Copilot の作業をガイドできます。

このレッスンでは、次のことを確認しました。

- カスタム エージェントの定義方法。
- Copilot Chat の Agent モードでカスタム エージェントを使う方法。

次は[エージェントの作業を監視して案内し][next-lesson]、変更を確認しながら、同じセッションにライト モードの切り替えを追加します。

## リソース

- [カスタム エージェントについて][custom-agents]
- [IDE でカスタム エージェントを作成する][creating-custom-agents-ide]
- [VS Code のカスタム エージェント][custom-agents-vscode]
- [カスタム エージェントの構成][custom-agents-config]
- [awesome-copilot のカスタム エージェント][awesome-copilot-agents]

---

| [← 前のレッスン: Playwright MCP サーバーで機能をテストする][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
