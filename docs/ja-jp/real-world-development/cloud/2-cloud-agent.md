---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: Custom instruction][previous-lesson] |
|:--|

技術的負債を抱えていない組織はほとんどありません。未解決のセキュリティ課題、更新が必要なレガシー コード、実装する時間がなくバックログに残った機能要望など、さまざまな課題があります。GitHub Copilot の cloud agent は、コードの更新や機能の追加などを自律的に実行します。作業が終わると、人間の開発者がレビューできる draft PR を作成します。手間のかかる作業を委任して開発を加速し、開発者はより大きな視点での作業に集中できます。

この演習では、Copilot cloud agent を使って次のことを行います。

- コード生成に使う環境をカスタマイズします。
- 操作を安全に行う方法を確認します。
- Issue の範囲を明確にする重要性を学びます。
- Issue を Copilot に割り当てます。

## シナリオ

Tailspin Toys には解消したい技術的負債があります。サイトの初期版を作成したコントラクターは、ドキュメントを整備していません。実際のところ、ドキュメントはほとんどありません。最初の課題として、アプリケーション内のすべての exported function に TSDoc の doc comment を追加します。

また、デザイン チームはゲームを見つけやすくしたいと考えています。各ゲームの詳細ページに、同じカテゴリに属する別のタイトルを「関連ゲーム」として表示したいとのことです。完成された実装でなくてもかまいません。UX の受け入れテストに使えるものを求めています。これは現在ブロッカーですが、より優先度の高い Issue がほかにあります。

どちらも優先度が下がりやすい作業の例です。Copilot cloud agent に割り当てれば、非同期に進めている間に開発者は別のタスクに集中でき、後から作業内容をレビューできます。

## GitHub Copilot cloud agent の概要

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) は、人間の開発者と同じように、バックグラウンドでタスクを実行できます。たとえば、[GitHub Issue を Copilot に割り当てる][assign-issue]方法があります。割り当てると、Copilot は進捗を追跡する draft pull request を作成し、環境を設定してタスクを開始します。進行中でも完了後でも Copilot のセッションを確認できます。解決策をレビューできる状態になると、Copilot がプルリクエストで通知します。

## 範囲を明確にした指示の重要性

GitHub Copilot に魔法はありません。考える作業を省略できるわけではありません。一見単純な作業でも、詳細を確認すると複雑な場合があります。[Copilot cloud agent に依頼するタスクの範囲を適切に設定][cloud-agent-best-practices]してください。AI ペア プログラマーとして扱い、段階を踏んで学び、試し、状況に合わせて調整します。生成 AI を追加しても、ソフトウェア開発の基本は変わりません。

## このリポジトリの custom instruction

これまでの演習で custom instruction と、Copilot を導く方法を学びました。すでに custom instruction の演習を行った場合は、`.github/copilot-instructions.md` の効果を確認したはずです。まだの場合は、ここで確認してみましょう。Copilot cloud agent に作業を割り当てる前に、このリポジトリに含まれる instruction file を読み取り専用で確認し、後で効果を見つけられるようにします。

リポジトリの GitHub Web UI、または起動済みの codespace で次のファイルを開きます。

- `.github/copilot-instructions.md` — **Code standards** セクションを確認し、特に TypeScript の規約と TSDoc の doc comment の要件に注目します。
- `.github/instructions/unit-tests.instructions.md` — `applyTo` frontmatter が `**/*.test.ts` ファイルに指示を適用することを確認します。

次のセクションで `Code lacks documentation` Issue を cloud agent に割り当てます。そのプルリクエストに TSDoc の doc comment、TypeScript の規約、コメント ヘッダーが含まれているか確認してください。これらは instruction file によるもので、後の演習で PR をレビューするときにも取り上げます。

## Copilot cloud agent の開発環境を設定する

コードを作成する際には、担当者に関係なく、適切な環境とセットアップ スクリプトが必要です。Copilot にタスクを割り当てるときも同様です。Copilot はソフトウェア エンジニアのように作業します。

Cloud agent は作業環境に [GitHub Actions][github-actions] を使います。`.github/workflows/copilot-setup-steps.yml` に[専用のセットアップ ワークフロー][setup-workflow]を作成すると、作業開始前に必要な開発ツールや依存関係を用意できます。このラボでは、学習の流れを整えるために事前設定されています。Node.js、プロジェクトの依存関係、E2E テスト用のブラウザー バイナリ、単一の Astro app 用にマイグレーションと seed を済ませた SQLite データベースを Copilot が利用できます。

```yaml
name: "Copilot Setup Steps"

# リポジトリの「Actions」タブからセットアップ手順をテストできます
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # *この手順*に必要な最小限の権限を設定します。Copilot の操作には専用のトークンが使われます。
    permissions:
      # 依存関係のインストールなど、セットアップ手順の一部としてリポジトリを clone する場合は `contents: read` 権限が必要です。clone しない場合は、手順の完了後に Copilot が自動的に行います。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # フロントエンド / app のセットアップ - Node.js（app 全体が Astro + Drizzle/libSQL になりました）
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # build とテスト用のデータを用意するため、ローカル SQLite データベースをマイグレーションして seed します。
      - name: Set up the database
        run: npm run db:setup
```

ほかの GitHub Actions workflow と同様の形式ですが、いくつか重要な点があります。

- `copilot-setup-steps` という job が 1 つあります。この job は、Copilot がプルリクエストの作業を始める前に GitHub Actions で実行されます。
- `workflow_dispatch` トリガーを使うと、リポジトリの **Actions** タブから手動でワークフローを実行できます。Copilot の実行を待たずにワークフローの動作を確認するのに便利です。

## ドキュメントを追加する

ドキュメントの重要性は誰もが理解していますが、情報が古くなっていたり、まったく整備されていなかったりするプロジェクトは少なくありません。この種の技術的負債は放置されやすく、生産性を下げ、コードベースの保守や新しい開発者の参加を難しくします。Copilot はドキュメントの作成を支援できます。cloud agent に任せる Issue として適しています。Copilot がバックグラウンドでドキュメントを作成している間、別の作業を進められます。後の演習で生成結果をレビューします。

1. 新しいブラウザー タブで github.com の自分のリポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択して新しい Issue ダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次の内容を入力します。

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選択して割り当てダイアログを開きます。

  ![Copilot を Issue に割り当てる](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。
10. **Pull Requests** タブを選択します。
11. 新しく作成されたプルリクエスト (PR) を開きます。タイトルは `[WIP]: Code lacks documentation` のようになります。PR が一覧に表示されない場合は、しばらく待ってからブラウザーを更新してください。
12. 数分後、Copilot が todo list を作成したことを確認します。

> [!NOTE]
> Copilot の todo list が PR に表示されるまで、数分かかる場合があります。Copilot は環境を準備し（前述の workflow を実行します）、プロジェクトを分析してタスクの進め方を決めています。

13. todo list と、Copilot が実行する予定のタスクを確認します。
14. プルリクエストのタイムラインを下にスクロールし、Copilot が Issue の作業を開始したことを示す更新を確認します。
15. **View session** ボタンを選択します。

  ![Copilot のセッションを表示する](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 更新された表示が現れるまで、ブラウザーのウィンドウを更新する必要がある場合があります。

16. セッションをスクロールして、Copilot が問題に取り組む様子を確認します。コードの調査、状態の把握、計画を考えるための一時停止、コードの作成などが行われます。

作業には数分かかることがあります。Copilot cloud agent の主な利点の 1 つは、タスクを非同期で実行し、その間にほかの作業を進められることです。別のタスクを cloud agent に割り当て、アプリケーションに機能を追加するコード作成に取り組みましょう。

## ゲーム詳細ページに関連ゲームのセクションを追加する

GitHub Copilot cloud agent の利点の 1 つは、作業を分担できることです。デザイン チームが求める関連ゲームのセクションは短時間で実装できるかもしれませんが、その時間を別の作業に使うこともできます。関連ゲームの作業を Copilot cloud agent に割り当てます。

1. github.com の自分のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択して新しい Issue ダイアログを開きます。
4. 空のテンプレートを使うため、**Blank issue** を選択します。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次の内容を入力します。

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選択して割り当てダイアログを開きます。

  ![Copilot を Issue に割り当てる](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

しばらくすると Issue の最初のコメントに 👀 が表示され、Copilot が作業を開始したことがわかります。

![Copilot が Issue に取り組んでいることを示す目の絵文字](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は 2 つ目の依頼にも取り組んでいます。Copilot cloud agent はソフトウェア エンジニアのように作業するため、完了まで常に監視する必要はありません。完了後にレビューできます。次は custom agent を作成して使います。

## まとめと次のステップ

[GitHub Copilot cloud agent][copilot-agents]について学びました。cloud agent に Issue を割り当てると、非同期で作業を依頼できます。技術的負債への対応、新機能の作成、別のフレームワークへのコード移行などにも活用できます。

この演習では、次のことを行いました。

- コード生成に使う環境をカスタマイズしました。
- 操作を安全に行う方法を確認しました。
- Issue の範囲を明確にする重要性を学びました。
- Issue を Copilot に割り当てました。

cloud agent がバックグラウンドで作業している間に、custom agent の作成と利用に進みます。[Copilot cloud agent は MCP サーバーも利用できます][cloud-agent-mcp]。また、前のモジュールで確認した custom instruction も使えます。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップ workflow のベスト プラクティス][cloud-agent-best-practices]

| [次のレッスン: Custom agent →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
