---
title: "演習 3 - Playwright MCP サーバーで機能をテストする"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: Agent mode][previous-lesson] · [次のレッスン: Custom agent →][next-lesson]

Agent mode でフィルタリング機能を構築しました。プルリクエストを作成する前に、ブラウザーで実際に動作することを確認します。自分でアプリを操作する代わりに、**Playwright MCP server** を接続し、Copilot に実際のブラウザーで機能をテストしてもらいます。その後、ブランチを公開して PR を作成します。

この演習では、次のことを行います。

- Model Context Protocol (MCP) とは何か、MCP server が Copilot にツールを追加する仕組みを学びます。
- Playwright MCP server を workspace に追加します。
- Copilot にブラウザー上でフィルタリング機能を手動テストさせます。
- ブランチを公開し、フィルタリング作業のプルリクエストを作成します。

## Model Context Protocol (MCP) とは

Agent mode は、エディターの外にアクセスできるとさらに強力になります。Copilot がそのために使うのが Model Context Protocol (MCP) です。agent が外部ツールやサービスと通信するための標準です。

![Agent mode の内部と、コンテキスト、LLM、MCP server と VS Code 拡張機能が提供するツールとの連携を示す図](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) は、AI agent が外部のツールやサービスと通信する方法を提供します。MCP を使うと、agent は外部ツールやサービスとリアルタイムでやり取りできます。リソースを使って最新情報にアクセスしたり、ツールを使ってユーザーに代わって操作したりできます。

ツールやリソースには、AI agent と外部のツールやサービスを橋渡しする MCP server を通じてアクセスします。MCP server は、既存 API や NPM package などのローカル ツールと AI agent の通信を管理します。それぞれの MCP server が、agent が利用できる異なるツールやリソースを提供します。

よく使われる MCP server の例を挙げます。

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: GitHub リポジトリを管理する API にアクセスできます。AI agent はリポジトリの作成、更新、Issue やプルリクエストの管理などを行えます。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: Playwright を使ったブラウザー自動化機能を提供します。AI agent は Web ページへの移動、フォームへの入力、ボタンの選択などを行えます。

さまざまなツールやリソースにアクセスできるほかの MCP server もあります。GitHub は、エコシステム内の発見や貢献を促進する [MCP registry](https://github.com/mcp) を提供しています。

> [!CAUTION]
> セキュリティの観点では、MCP server をほかの依存関係と同じように扱ってください。利用前にソース コードを慎重に確認し、公開元を検証して、セキュリティへの影響を検討します。信頼できる MCP server だけを使い、機密リソースや操作へのアクセスを許可する際は注意してください。

## MCP の設定を確認する

`.vscode/mcp.json` ファイルでは、この VS Code workspace で利用できる MCP server を設定します。

1. codespace で `.vscode/mcp.json` を開きます。
2. `github` server がすでに設定されていることを確認します。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

この `github` entry はプロジェクト テンプレートに含まれているため、前の演習で Copilot が Issue のバックログを読み取れました。[リモート GitHub MCP server][remote-github-mcp-server]を使うため、ローカルにインストールする必要はありません。VS Code が HTTP で接続し、Copilot がツールを初めて使うときに GitHub で認証します。

## Playwright MCP server を追加する

次に、2 つ目の server を追加します。[Playwright MCP server][playwright-mcp-server] は Copilot が操作できるブラウザーを提供します。機能をテストするために使います。

1. `.vscode/mcp.json` で `github` と並べて `playwright` entry を追加し、ファイルを次のようにします。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. ファイルを保存します。

リモートの `github` server とは異なり、`playwright` は**ローカル** server です。VS Code が `npx @playwright/mcp@latest` を実行して起動します。`--headless` flag は、画面を表示せずに Playwright を実行するよう指定します。codespace にはブラウザーを表示するデスクトップがないため、この設定が必要です。

> [!NOTE]
> Tailspin Toys プロジェクトでは E2E test にすでに Playwright を使っているため、必要なブラウザーは通常インストール済みです。後で Copilot がブラウザーがないと報告した場合は、`npx playwright install chromium` を実行するよう依頼してから再試行してください。

## Server を起動して信頼する

VS Code は MCP server を**必要に応じて**起動します。Copilot が server のツールを初めて使うときに VS Code が server を起動し、信頼するか確認します。

1. `.vscode/mcp.json` の `playwright` entry の上に **Start** 操作が表示されます。選択して server を起動し、設定が正しいことを確認します。
2. VS Code が server を信頼するか確認した場合は、設定を確認して、server が起動できるよう信頼することを選択します。

手動での起動は任意です。省略しても、次の手順で Copilot がツールを初めて必要としたときに VS Code が server を起動し、信頼を確認します。

## フィルタリング機能をテストする

[Playwright MCP server][playwright-mcp-server] を使うと、Copilot が実際のブラウザーを操作できます。自分でアプリを選択して動作を確認する代わりに、agent がページを開いて移動し、フィルターを適用して結果を読み取り、確認内容をまとめます。会話を離れずに機能をすばやく確認できます。

Playwright MCP server は、スクリーンショットではなくページの[アクセシビリティ ツリー][playwright-mcp-server]を使って操作します。agent は button、link、list item などの構造化されたラベル付き要素を使って推論します。支援技術と同じ情報を使うため、機能テストで簡単なアクセシビリティ確認も行えます。

Server を接続し、app を起動した状態で、次の prompt を Copilot に送り、フィルタリング機能を試します。

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot は Playwright MCP server を使ってブラウザーを起動し、各手順を実行して結果を報告します。要約を Issue の受け入れ条件と照らして確認してください。想定と異なる場合は追加の質問をしたり、PR を作成する前にコードの修正を依頼したりします。

> [!NOTE]
> このテストでは app が `http://localhost:4321` で起動している必要があります。開発サーバーを停止した場合は、prompt を送信する前に再起動します。Copilot が Playwright MCP server を初めて使うとき、ブラウザーをダウンロードする場合があります。ブラウザーがないと報告された場合は、`npx playwright install chromium` を実行するよう依頼して再試行してください。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## ブランチを公開してプルリクエストを作成する

機能が動作することを確認できたので、チームのレビュー用にプルリクエスト (PR) を作成します。最初に `filtering-vscode` ブランチを公開します。

1. codespace の **Source Control** パネルを開き、Copilot が行った変更を確認します。
2. **+** アイコンを選択して変更を stage します。
3. **Sparkle** ボタンを使って commit message を生成します。

   ![Copilot が行った変更を表示する Source Control パネル](../../../_images/ex3-source-control-changes.png)

4. **Publish** を選択し、ブランチをリポジトリに push します。

## プルリクエストを作成する

github.com や GitHub command-line interface (CLI) など、PR の作成方法はいくつかあります。GitHub Copilot を使っているので、PR を作成してもらいましょう。関連する Issue を探し、それに紐づいた PR を作成できます。

1. Copilot Chat パネルに移動し、**New Chat** を選択して新しいセッションを開始します。
2. Copilot が GitHub ツールを使えるように、agent dropdown で **Agent** が選択されていることを確認します。
3. 次の prompt を Copilot に送り、PR を作成します。

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. 情報の取得や操作の実行を許可するため、必要に応じて **Continue** を選択します。GitHub ツールを初めて使う場合、GitHub への認証を求められることがあります。画面の指示に従って許可します。
5. Copilot が Issue を検索して正しいものを見つけ、PR を作成したことを確認します。
6. Copilot が作成した link を選択してプルリクエストをレビューします。ただし、まだ**マージしないでください**。

## まとめと次のステップ

この演習では、次のことを行いました。

- Model Context Protocol (MCP) とは何か、MCP server が Copilot にツールを追加する仕組みを学びました。
- Playwright MCP server を workspace に追加しました。
- MCP server を使って、リリース前にブラウザー上でフィルタリング機能を手動テストしました。
- ブランチを公開し、フィルタリング作業のプルリクエストを作成しました。

MCP server を使って機能を**テスト**しましたが、MCP は Copilot を拡張する方法の 1 つにすぎません。次は、アクセシビリティ レビューなど対象を絞った作業を効率化する[custom agent を作成します][next-lesson]。

## リソース

- [MCP とは何か、なぜ話題になっているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP Registry][mcp-registry]
- [VS Code の MCP server][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
