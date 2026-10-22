---
title: "演習 0: 前提条件"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Copilot cloud agent の演習を始める前に、必要な環境を準備します。Tailspin Toys リポジトリのコピーを作成し、instruction file の編集と cloud agent が作成した変更のレビューに使う [codespace][codespaces] を起動します。

## 演習用リポジトリを設定する

コードを作成するためのリポジトリを用意します。[テンプレート][template-repository]から自分用のリポジトリを作成します。演習に必要なファイルがすべて含まれており、以降の作業で使用します。

1. 新しいブラウザー ウィンドウで [Tailspin Toys テンプレート][tailspin-template]を開きます。
2. 演習用リポジトリのページで **Use this template** を選択し、続いて **Create a new repository** を選択して、自分用のコピーを作成します。

    ![「Use this template」ボタン](../../../_images/ex0-use-template.png)

3. GitHub または Microsoft が主催するイベントでワークショップに参加している場合は、メンターの指示に従ってください。それ以外の場合は、GitHub Copilot を利用できる組織に新しいリポジトリを作成します。

    ![リポジトリ テンプレートの設定を入力する画面](../../../_images/ex0-repository-settings.png)

4. 後の演習で参照するため、作成したリポジトリのパス（**organization-or-user-name/repository-name**）を控えておきます。

## codespace を作成する

次に、codespace を使って演習を進めます。

[GitHub Codespaces][codespaces] はクラウドベースの開発環境です。ブラウザーから直接コードを書き、実行し、デバッグできます。複数のプログラミング言語、拡張機能、ツールを使える、機能が揃った IDE です。

1. 作成したリポジトリを開きます。
2. 緑色の **Code** ボタンを選択します。

    ![「Code」ボタンを選択する](../../../_images/ex0-code-button.png)

3. **Codespaces** タブを選択し、**+** ボタンを選択して新しい Codespace を作成します。

    ![新しい codespace を作成する](../../../_images/ex0-create-codespace.png)

codespace の作成には数分かかりますが、必要なサービスを手動でインストールするよりずっと早く完了します。その間に GitHub Copilot のほかの機能を確認してもよいでしょう。次の演習で詳しく見ていきます。

> [!CAUTION]
> 今後の演習でこの codespace に戻ります。今は、ブラウザーのタブで開いたままにしてください。

> [!NOTE]
> このワークショップは codespace またはローカルの [dev container][dev-containers] で実行できます。どちらも演習に必要な前提条件を備えています。ローカルで実行する場合は、クローンしたリポジトリを VS Code で開き、プロンプトが表示されたら **Reopen in Container** を選択します。VS Code が codespace と同じ dev container を構築します。

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## まとめ

演習用リポジトリのコピーを作成し、codespace の作成を開始しました。以降の演習では、Copilot cloud agent と並行して作業するために、この環境を使います。

## 次のステップ

次は cloud agent が従う custom instruction を追加します。[演習 1 - Custom instruction][next-lesson]に進みます。

## リソース

- [GitHub Codespaces の概要][codespaces]
- [テンプレートからリポジトリを作成する][template-repository]
- [Codespaces のクイックスタート][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[next-lesson]: ../1-custom-instructions/
