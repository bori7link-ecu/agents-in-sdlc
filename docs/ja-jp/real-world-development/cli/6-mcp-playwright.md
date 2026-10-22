---
title: "レッスン 6 - Playwright MCP で機能を検証する"
description: "Copilot CLI に Playwright MCP を追加し、ブラウザーでフィルタリング機能を確認します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

コードを書く以外にも、データや外部サービスを扱い、Copilot が利用できる自動化機能を追加することがあります。ここで MCP server が役立ちます。MCP server を使うと、Copilot は CLI に組み込まれた機能のほかに、さらに多くのツールやサービスを利用できます。

このレッスンでは、次のことを行います。

- Model Context Protocol (MCP) とは何か、Copilot CLI でどのように使うかを学びます。
- Playwright MCP server がまだ利用できない場合は追加します。
- agent にブラウザーを操作させ、フィルタリング機能を確認します。

## シナリオ

unit test と E2E test は重要ですが、UI の更新を検証するには実際に UI を操作する必要があります。Copilot がユーザーと同じように作業中の Web サイトを使えるようにし、変更の自動化をさらに進め、想定どおりに動作するという確信を高めます。

## Model Context Protocol (MCP) とは

[Model Context Protocol (MCP)][mcp-blog-post] は、AI agent が外部ツールやサービスと通信する方法を提供します。MCP を使うと、AI agent は外部ツールやサービスとリアルタイムにやり取りできます。最新情報にアクセスしたり、ユーザーに代わって操作したりできます。

これらのツールやリソースには、AI agent と外部のツールやサービスを橋渡しする MCP server を通じてアクセスします。それぞれの MCP server が、agent が利用できる異なるツールとリソースを提供します。

広く使われている MCP server の例を挙げます。

- **[GitHub MCP Server][github-mcp]**: GitHub のリポジトリ、Issue、プルリクエストを管理する API を利用できます。
- **[Playwright MCP Server][playwright-mcp-server]**: Playwright を使ったブラウザー自動化機能を利用できます。

ほかにも多数の MCP server が公開されています。GitHub は、エコシステム内の発見や貢献を促進する [MCP registry][mcp-registry] を提供しています。

> [!CAUTION]
> MCP server は、プロジェクト内のほかの依存関係と同じように扱ってください。利用前にソース コードを確認し、公開元を検証して、セキュリティへの影響を検討します。

## Playwright MCP server を追加する

Playwright MCP server がまだ利用できない場合は追加します。Copilot がユーザーと同じように Web サイトを操作できるようになります。

> [!NOTE]
> Copilot CLI に Playwright MCP がすでに設定されている場合は、セットアップ手順を飛ばして次のセクションに進んでください。

1. Codespace に戻ります。
2. Copilot CLI で次のコマンドを入力し、MCP server の追加ダイアログを開きます。

   ```plaintext
   /mcp add
   ```

3. 名前として `playwright` を入力し、<kbd>Tab</kbd> を押します。
4. <kbd>Enter</kbd> を押して server type が **STDIO** であることを確認し、続けて <kbd>Tab</kbd> を押します。
5. **Command** ダイアログに次の内容を貼り付けます。

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. <kbd>Control</kbd>+<kbd>S</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) を押して、新しい MCP server を保存します。
7. <kbd>Esc</kbd> を押して MCP ダイアログを閉じます。

## Playwright で機能を確認するよう Copilot に依頼する

先ほど、機能が想定どおりに動作することを手動で確認しました。次は、Copilot が利用できる Playwright MCP server を使って同じ確認を行います。

1. 次の prompt を Copilot に送信し、Playwright MCP server で機能を検証するよう依頼します。

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> 通常、Copilot は MCP server を自動的に見つけて使うため、明示的に指示する必要はありません。ただし、使用する server がわかっている場合は prompt で伝えると、より一貫した結果を得やすくなり、token の節約にもなります。

2. Copilot がブラウザー内で機能を確認する手順を実行する様子を見ます。
3. レポートを読み、すべてが想定どおり動作することを確認します。

Copilot は server を起動し、Playwright で Web サイトを操作してから server を停止し、結果を報告します。

## まとめと次のステップ

Playwright MCP server を使い、Copilot CLI から実際のブラウザーで機能を確認しました。このレッスンでは次のことを行いました。

- Model Context Protocol (MCP) とは何か、Copilot CLI でどのように使うかを学びました。
- 必要に応じて Playwright MCP server を追加しました。
- agent にブラウザーを操作させ、フィルタリング機能を確認しました。

次は、skill とブラウザー ツールを専門的な役割にまとめる [QA custom agent を作成します][next-lesson]。

## リソース

- [MCP とは何か、なぜ話題になっているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [Copilot CLI に MCP server を追加する][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
