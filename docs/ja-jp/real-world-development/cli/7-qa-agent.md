---
title: "レッスン 7 - QA agent を作成して使う"
description: "Issue の要件、quality-checks skill、Playwright MCP を組み合わせた QA custom agent を作成します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

`quality-checks` skill を使って自動チェックを実行し、Playwright MCP を使ってブラウザーでフィルタリングの動作を確認しました。次は、明確に定義した QA プロセスを custom agent にまとめます。

このレッスンでは、次のことを行います。

- custom agent が instruction、skill、MCP ツールとどのように連携するかを確認します。
- 再利用できる quality assurance profile を作成して確認します。
- QA agent を選択し、フィルタリングの Issue に照らして結果をレビューします。

## シナリオ

Tailspin Toys では、プルリクエスト (PR) を作成する前に、要件、コード品質、自動チェック、テスト範囲、ブラウザー上の動作を一貫した方法で確認したいと考えています。custom agent を使うと、この QA プロセスを連携させ、再利用可能なレポートを作成できます。

## custom agent とは

custom agent は、Markdown profile で定義する Copilot の専門的な構成です。profile には agent の目的、指示、利用できるツールを記述します。このワークショップでは、`.github/agents/qa.agent.md` に QA の役割を定義し、Copilot CLI で選択します。

これまでに使ったカスタマイズには、それぞれ異なる役割があります。リポジトリの instruction はチームの標準を説明します。`quality-checks` skill は繰り返し実行するチェックをまとめます。Playwright MCP はブラウザー ツールを提供します。QA profile は、要件を評価して結果を報告するために、Copilot がそれらの機能をどう使うかを指示します。profile はそれらを置き換えるものではなく、別の会話も必要ありません。

## QA profile を作成する

機能の PR を作成する前に、再利用できる QA profile を Copilot に作成してもらいます。この profile では、QA で行うチェックと従うべき制約を定義します。

1. Codespace に戻り、フィルタリングの会話が Interactive モードであることを確認します。
2. default agent に、新しい custom agent を作成するよう依頼します。

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## profile を確認する

新しい agent を使う前に、Copilot が意図した QA ワークフローと制約を設定したことを確認します。これにより、検証だけを求めているときに agent が不完全な指示で機能を変更したり、範囲外の作業をしたりするのを防げます。

1. `/diff` を入力し、`.github/agents/qa.agent.md` を開きます。
2. frontmatter を読みます。`description` は必須です。`name` は任意ですが、含めると agent の表示名が明確になります。
3. profile の指示を読み、QA が要件を起点とし、リポジトリの instruction に従い、`quality-checks` skill を実行して Playwright MCP を使うことを確認します。
4. QA が根拠を示して報告し、実装コードを変更する前に確認を求め、変更を commit したりプルリクエストを作成したりしないことを確認します。
5. いずれかの役割や制約が欠けている場合は、次に進む前に default agent に修正を依頼します。

## Issue に対して QA を実行する

Copilot CLI は会話の開始時にプロジェクトの agent を読み込みます。profile の作成後、同じフィルタリングの会話を再開し、QA を選択します。これにより、すでに会話に含まれている Issue と計画時の決定を利用できます。

1. 次のプロンプトで agent を有効にします。

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> 作成直後は、利用可能な agent の一覧に新しい agent が表示されないことがあります。agent は読み込まれているので、上記のコマンドで有効にできます。

2. 次のプロンプトで QA agent に機能をレビューさせます。

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. QA agent の作業が終わるまで待ちます。
4. 完了したら、レポートを確認します。

## まとめと次のステップ

再利用できる専門的な役割をワークフローに追加し、その作業をレビューしました。このレッスンでは、次のことを行いました。

- custom agent が instruction、skill、MCP ツールとどのように連携するかを確認しました。
- 再利用できる quality assurance profile を作成して確認しました。
- QA agent を選択し、フィルタリングの Issue に照らして結果をレビューしました。

実装、skill の更新、QA profile、テスト、検証レポートの準備が整いました。次は、[これらを機能の PR にまとめ、Agent Merge を使います][next-lesson]。

## リソース

- [Copilot CLI 用 custom agent を作成する][create-agents]
- [custom agent の構成][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
