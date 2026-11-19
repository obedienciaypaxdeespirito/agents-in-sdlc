---
title: "演習 0: 前提条件"
description: "Tailspin Toys の自分用コピーを作成し、Copilot CLI ワークショップ用の GitHub Codespace を準備します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Copilot CLI の演習を始める前に、必要な準備を整えます。Tailspin Toys リポジトリの自分用コピーを作成し、[codespace][codespaces] を立ち上げます。次の演習では、その統合ターミナルを使って Copilot CLI をインストールし、実行します。

この演習では、次のことを行います。

- テンプレートから Tailspin Toys プロジェクトの自分用コピーを作成する。
- Codespace を作成し、プロジェクトの準備ができていることを確認する。

## ラボ用リポジトリを設定する

これから作成するコード用にリポジトリのコピーを作成するため、[template][template-repository] からインスタンスを作成します。新しいインスタンスにはラボに必要なすべてのファイルが含まれており、演習を進める間はこのリポジトリを使用します。

1. 新しいブラウザー ウィンドウで、[Tailspin Toys のテンプレート リポジトリ][tailspin-template] に移動します。
2. ラボ用リポジトリ ページの **Use this template** ボタンを選択して、自分用のリポジトリ コピーを作成します。次に **Create a new repository** を選択します。

    ![「Use this template」ボタン](../../../_images/ex0-use-template.png)

3. GitHub または Microsoft が主催するイベントの一環としてこのワークショップを進めている場合は、メンターから案内された手順に従ってください。そうでない場合は、GitHub Copilot にアクセスできる organization に新しいリポジトリを作成できます。

    ![リポジトリ テンプレートの設定を入力する画面](../../../_images/ex0-repository-settings.png)

4. 後でラボ内で参照するため、作成したリポジトリ パス（**organization-or-user-name/repository-name**）を控えておいてください。

> [!NOTE]
> **バックログの準備は完了しています**
>
> テンプレートからリポジトリを作成すると、GitHub issue のバックログが自動的に作成されます。ワークショップ全体を通してこれらの issue を使って作業するため、自分で起票する必要はありません。

ワークショップ テンプレートの新しいコピーを使ってください。必要なリポジトリ命令、アプリケーション コード、テスト、`quality-checks` skill、バックログが含まれています。古いコピーを使う場合は、必要なファイルがそろっているか、ファシリテーターに確認してください。

## Codespace を作成する

次は、codespace を使ってラボの演習を進めます。

[GitHub Codespaces][codespaces] はクラウドベースの開発環境であり、ブラウザーから直接コードの作成、実行、デバッグを行えます。複数のプログラミング言語、拡張機能、ツールに対応したフル機能の IDE を提供します。

1. 新しく作成したリポジトリに移動します。
2. **Code** を選択します。

    ![「Code」ボタンを選択する](../../../_images/ex0-code-button.png)

3. **Codespaces** タブを選択し、**Create codespace on main** を選択します。

    ![新しい codespace を作成する](../../../_images/ex0-create-codespace.png)

Codespace の作成には数分かかりますが、すべてのサービスを手動でインストールするよりははるかに速く完了します。その間に、次に扱う GitHub Copilot のほかの機能を確認しておくこともできます。

4. Codespace のセットアップが完了するまで待ちます。テンプレートがプロジェクトの依存関係、Playwright Chromium、ローカル データベースをインストールします。
5. **Do you trust the authors of the files in this folder?** と表示されたら、**Trust Folder & Continue** を選択します。
6. リポジトリ ルートでターミナルを開き、次のコマンドでアプリケーションを起動します。

   ```bash
   npm run dev
   ```

7. Codespaces でポート `4321` が利用可能になったら、**Open in Browser** を選択して Tailspin Toys のサイトが読み込まれることを確認します。
8. ターミナルに戻り、<kbd>Ctrl</kbd>+<kbd>C</kbd> を押して開発サーバーを停止します。

[codespaces]: https://github.com/features/codespaces
[tailspin-template]: https://github.com/github-samples/tailspin-toys
## まとめ

準備が整いました。このレッスンでは、次のことを行いました。

- Tailspin Toys プロジェクトの自分用コピーをテンプレートから作成した。
- Codespace を作成し、プロジェクトの準備ができていることを確認した。

## 次のステップ

Copilot CLI をインストールし、GitHub アカウントで認証しましょう。[演習 1 - GitHub Copilot CLI のインストール][next-lesson] に進みます。

## リソース

- [GitHub Codespaces の概要][codespaces]
- [テンプレートからリポジトリを作成する][template-repository]
- [Codespaces クイックスタート][codespaces-quickstart]

[next-lesson]: ../1-install-copilot-cli/
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
