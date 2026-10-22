---
title: "演習 3 - Custom agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## Custom agent とは

GitHub Copilot の [custom agent][custom-agents-concept]を使うと、開発ワークフローの特定のタスクや分野に合わせた、専門的な AI アシスタントを作成できます。リポジトリの `.github/agents` フォルダーに Markdown ファイルを作成し、対象を絞った指示、ベスト プラクティス、コーディング パターン、専門知識を Copilot に提供します。チームの専門知識を再利用できる agent にまとめられます。たとえば、[WCAG][wcag] 準拠を確認するアクセシビリティ agent、安全なコーディングを確認するセキュリティ agent、一貫したテスト パターンを維持するテスト agent などです。

Custom agent は、プロジェクト内の `.github/agents` フォルダー、またはユーザー共通の `~/.copilot/agents` にある Markdown ファイルで定義します。各ファイルには、少なくとも `name` と `description` を持つ YAML frontmatter と、その後に agent の振る舞い、専門性、指示を定義する Markdown prompt が必要です。

### Custom agent と agent skill の比較

Custom agent と [agent skill][agent-skills-concept]には共通点があります。どちらも主に Markdown ファイルで定義し、AI に作業方法を伝えます。簡潔に区別するなら、**custom agent は作業者、skill はツール**です。

Custom agent は独自のコンテキスト ウィンドウを持ち、作業の一部として skill や別の agent を調整します。このラボでは、アクセシビリティ custom agent がアクセシビリティ ガイドラインに沿ってサイトをレビューし、必要な変更を行います。その過程で、プルリクエストのワークフロー用 skill や、テストを実行・管理する skill を呼び出すこともできます。

> [!NOTE]
> Custom agent の作成に唯一の正解はありません。AI のほかの使い方と同じく、自分の環境とシナリオで試して調整してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

Custom agent を使って、次のことを行います。

- custom agent の定義方法を確認します。
- custom agent にタスクを割り当てます。

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、すべてのユーザーがクラウドファンディング プラットフォームを利用できるようにしたいと考えています。ユーザーから、テキストと背景色のコントラストが不十分で、現在のダーク テーマが読みにくいというフィードバックがありました。このアクセシビリティの課題に対応するため、デザイン チームはユーザーが切り替えられるハイ コントラスト モードを求めています。

アクセシビリティは重要なので、できるだけ早く実装します。custom agent に機能の生成を依頼します。

## アクセシビリティ custom agent を確認する

アクセシビリティ用の custom agent がすでに用意されています。Copilot を導く方法を理解するため、内容を確認します。

codespace に戻り、アクセシビリティ custom agent のファイルを確認します。

1. `.github/agents/accessibility.md` を開きます。
2. `name` と `description` を含む YAML frontmatter を確認します。

> [!CAUTION]
> Custom agent には `name` と `description` を含む frontmatter が必要です。

3. その後のセクションを読み、次の内容を確認します。
   - アクセシブルな Web サイトのコード生成で agent が担う役割
   - アクセシビリティのベスト プラクティス
   - HTML、CSS、JavaScript のコード例
   - よくある問題や間違い

## Issue を作成して割り当てる

Mission control は、環境内のすべての agent を扱う中心的な場所です。Copilot cloud agent にタスクを割り当て、監視し、進行中のセッションに追加の指示を出せます。まず、ハイ コントラスト モードの作成を Copilot に依頼します。

1. リポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue のダイアログを開きます。
4. **Blank issue** を選択して Issue を作成します。
5. **Title** に `Add high contrast mode to website` と入力します。
6. **Description** に次の内容を入力します。

    ```plaintext
    We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
    ```

7. **Create** を選択して Issue を作成します。
8. 右側にある **Assign to Copilot** を選択し、割り当てダイアログを開きます。
9. custom agent の一覧から **Accessibility agent** を選択します。

    ![cloud agent の割り当て画面で、custom agent とアクセシビリティを選択した状態](../../../_images/ex5-select-custom-agent.png)

10. **Assign** を選択します。
11. Copilot がバックグラウンドで作業を開始します。

## まとめと次のステップ

GitHub Copilot の [custom agent][custom-agents]について学びました。custom agent は特定のタスクや分野向けに構成された AI アシスタントです。チームの専門知識と標準を再利用可能な agent にまとめると、Copilot の作業をより効果的に導けます。

この演習では、次のことを行いました。

- custom agent の定義方法を確認しました。
- custom agent にタスクを割り当てました。

Copilot がハイ コントラスト モードを実装している間に、[mission control から agent のセッションを監視して指示を出す][next-lesson]方法を確認します。

## リソース

- [Custom agent について][custom-agents]
- [組織で custom agent を使うための準備][org-custom-agents]
- [Enterprise で custom agent を使うための準備][enterprise-custom-agents]

---

| [← 前のレッスン: GitHub Copilot cloud agent][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
