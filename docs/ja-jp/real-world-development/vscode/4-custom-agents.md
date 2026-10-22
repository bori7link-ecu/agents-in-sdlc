---
title: "演習 4 - Custom agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## Custom agent とは

GitHub Copilot の [custom agent][custom-agents-concept]を使うと、開発ワークフロー内の特定のタスクや分野に合わせた、専門的な AI アシスタントを作成できます。リポジトリの `.github/agents` フォルダーに Markdown ファイルを作成し、対象を絞った指示、ベスト プラクティス、コーディング パターン、専門知識を Copilot に提供します。チームの専門知識を再利用できる agent にまとめられます。たとえば、[WCAG][wcag] 準拠を確認するアクセシビリティ agent、安全なコーディングを確認するセキュリティ agent、一貫したテスト パターンを維持するテスト agent などです。

Custom agent は、プロジェクト内の `.github/agents` フォルダー、またはユーザー共通の `~/.copilot/agents` にある Markdown ファイルで定義します。各ファイルには、少なくとも `name` と `description` を持つ YAML frontmatter と、その後に agent の振る舞い、専門性、指示を定義する Markdown prompt が必要です。

### Custom agent と agent skill の比較

Custom agent と [agent skill][agent-skills-concept]には共通点があります。どちらも主に Markdown ファイルで定義し、AI に作業方法を伝えます。簡潔に区別するなら、**custom agent は作業者、skill はツール**です。

Custom agent は独自のコンテキスト ウィンドウを持ち、作業の一部として skill や別の agent を調整します。このラボでは、アクセシビリティ custom agent がアクセシビリティ ガイドラインに沿ってサイトをレビューし、必要な変更を行います。その過程で、プルリクエストのワークフロー用 skill や、テストを実行・管理する skill を呼び出すこともできます。

> [!NOTE]
> Custom agent の作成に唯一の正解はありません。AI のほかの使い方と同じく、自分の環境とシナリオで試して調整してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、すべてのユーザーがクラウドファンディング プラットフォームを利用できるようにしたいと考えています。ユーザーから、テキストと背景色のコントラストが不十分で、現在のダーク テーマが読みにくいというフィードバックがありました。このアクセシビリティの課題に対応するため、デザイン チームはユーザーが切り替えられるハイ コントラスト モードを求めています。

アクセシビリティは重要なので、できるだけ早く実装します。custom agent に機能の生成を依頼します。

この演習では、次のことを行います。

- 既存のアクセシビリティ custom agent を確認します。
- Copilot Chat でアクセシビリティ agent を使い、ハイ コントラスト モードを実装します。

## アクセシビリティ custom agent を確認する

アクセシビリティ用の custom agent がすでに作成されています。Copilot を導く方法を理解するため、内容を確認します。

codespace に戻り、新しいブランチを作成してアクセシビリティ作業を始めます。演習 3 のフィルタリング PR とは別に管理します。

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. `.github/agents/accessibility.md` を開きます。
2. `name` と `description` を含む YAML frontmatter を確認します。

> [!CAUTION]
> Custom agent には `name` と `description` を含む frontmatter が必要です。

3. 後続のセクションを確認します。
   - アクセシブルな Web サイトのコード生成で agent が担う役割
   - アクセシビリティのベスト プラクティス
   - HTML、CSS、JavaScript のコード例
   - よくある問題や間違い

## Copilot Chat で custom agent を使う

VS Code は `.github/agents` にあるすべての custom agent を Copilot Chat ビュー下部の agent dropdown に表示します。custom agent を選ぶと、その agent の指示とツールに合わせて Chat セッションを設定できます。

> [!TIP]
> **Copilot Chat を開く**
>
> 次の手順を始める前に codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な手順は各演習で説明します。

1. Chat ビューの agent dropdown で **Agent** が選択されていることを確認します。未選択の場合は選択します。

   ![Chat ビューで agent picker を表示した画面](../../../_images/shared-chat-mode-selector.png)

2. Chat ビュー下部の agent dropdown を選択します。既定では有効な agent として **default** が表示されます。
3. 利用できる agent の一覧から **Accessibility agent** を選択します。
4. 次の prompt をアクセシビリティ agent に送信します。

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

5. Copilot Chat がコードベースを調査し、変更を提案してプロジェクトのファイルを編集します。編集ごとにファイル パスと差分が Chat に表示されます。
6. agent が変更したファイルの一覧を示す **Files changed** の表示が更新されることを確認します。

> [!NOTE]
> 作業には数分かかる場合があります。Copilot は Astro component、CSS、関連するテストなど、既存ファイルを実際に変更します。

次の演習では、進行中の作業をレビューし、指示を追加します。

## まとめと次のステップ

このレッスンでは、特定のタスクや分野に合わせた AI アシスタントである GitHub Copilot の [custom agent][custom-agents]について学びました。custom agent を使うと、チームの専門知識と標準を再利用可能な形にまとめ、Copilot の作業を効果的に導けます。

次の内容を確認しました。

- custom agent の定義方法
- Copilot Chat の agent mode で custom agent を使う方法

次は[agent の作業を監視して指示を出し][next-lesson]、変更を確認しながら同じセッションにライト モードの切り替えを追加します。

## リソース

- [Custom agent について][custom-agents]
- [IDE で custom agent を作成する][creating-custom-agents-ide]
- [VS Code の custom agent][custom-agents-vscode]
- [Custom agent の構成][custom-agents-config]
- [awesome-copilot の custom agent][awesome-copilot-agents]

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
