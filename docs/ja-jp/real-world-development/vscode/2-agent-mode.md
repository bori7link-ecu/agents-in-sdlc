---
title: "演習 2 - Copilot Agent Mode で新しい機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

アプリケーションの小さな更新でも、複数のファイルの変更やテストの実行が必要になることがあります。開発者は通常、必要なファイルを探し、変更し、テストを実行し、問題をデバッグし、変更漏れのファイルを見つけて再度更新します。こうした作業は続いていきます。

このような場合に Copilot Agent Mode が役立ちます。

Copilot Agent Mode は、IDE 内でより自律的に作業します。プロジェクト構造を調査し、必要な変更を加え、テストなどを実行し、見つかった問題を自動的に修正します。Agent Mode を使ってサイトに新しい機能を追加する方法を確認しましょう。

> [!NOTE]
> 名前は似ていますが、agent mode と cloud agent は異なる体験向けに作られています。Agent mode は IDE 内でタスクを実行し、素早くフィードバックを得ながら対話できます。Cloud agent はチームの一員のように非同期で作業し、Issue とプルリクエストを通じてやり取りします。

この演習では、次のことを学びます。

- Copilot Agent Mode がプロジェクトを調査し、関連ファイルを特定し、複数ファイルにわたる変更を行う方法。
- GitHub Copilot Agent Mode で UI とデータ レイヤーにまたがる新機能を実装する方法。
- コードベースにマージする前に、Copilot Agent Mode が生成した変更とテストをレビューする方法。

## シナリオ

ゲームの一覧が増えたので、カテゴリとパブリッシャーで絞り込めるようにします。前の演習で publishers helper を追加しました。今回は Copilot Agent Mode を使い、残りのデータ レイヤー、UI、テストを完成させます。

## Tailspin Toys Web サイトを実行する

変更を加える前に、Tailspin Toys Web サイトの現在の機能を確認します。

この Web サイトは、開発者をテーマにしたボードゲームのクラウドファンディング プラットフォームです。ゲーム一覧と各ゲームの詳細を表示します。単一の Astro app であり、build 時にページを静的 HTML としてレンダリングします。ページは frontmatter から Drizzle ORM を通じてローカル SQLite データベースを直接参照します。独立したバックエンド API やクライアント側 UI framework はありません。再利用可能なデータアクセス helper は `src/lib/` にあり、操作性は標準 DOM API を使った小さな scoped Astro `<script>` で追加します。

### Web サイトを起動する

`npm run dev` タスクを実行すると、単一の Astro 開発サーバーが起動します。GitHub Codespace で次の手順を行います。

1. codespace に戻ります。前の演習を終えて `main` にいる状態です。
2. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押し、codespace 内に新しいターミナルを開きます。
3. フィルタリング作業用ブランチを作成して切り替えます。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 次のコマンドで Web サイトを起動します。

   ```bash
   npm run dev
   ```

   Astro 開発サーバーが準備できると、次のような URL を含むメッセージが表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> `http://localhost:4321` をブラウザーで開くか尋ねるダイアログが表示された場合は、**x** を選択して閉じます。

5. ターミナルに表示された Astro server のアドレス `http://localhost:4321` を、Mac では <kbd>Command</kbd>、Windows/Linux では <kbd>Ctrl</kbd> を押しながら選択して Web サイトを開きます。

> [!NOTE]
> Codespace でターミナルの localhost URL を選択すると、自動的に `https://<your-codespace-name>-4321.app.github.dev/` にリダイレクトされます。これは codespace 内で起動している Web サーバーへのプライベートなトンネルです。

### Web サイトを確認する

Web サイトが起動したら、機能を確認します。主な機能は次のとおりです。

- **ホーム ページ**: ボードゲームのタイトル、画像、説明を一覧表示します。
- **ゲーム詳細ページ**: ゲームを選択すると、タイトル、説明、パブリッシャー、カテゴリなどの詳しい情報を表示します。

## Copilot でバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> 次の演習を始める前に codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な手順は各演習で説明します。

Web サイトの初期実装は動作していますが、新しい機能を追加して改善します。まず、バックログを確認します。テンプレートからプロジェクトを作成したときに Issue のバックログが自動的に作成されています。GitHub Copilot に項目を表示してもらいましょう。

1. Chat ビューの agent dropdown で **Agent** を選択します。**Agent** は自律的に計画を立て、ファイルにまたがる変更を実装し、ターミナル コマンドを実行し、ツールを呼び出します。

   ![Chat ビューで agent picker を表示した画面](../../../_images/shared-chat-mode-selector.png)

2. 利用可能なモデルの一覧から **Claude Sonnet 4.5** を選択します。

> [!CAUTION]
> このワークショップの作成者は、特定のモデルを推奨しているわけではありません。ワークショップの作成時には Claude Sonnet 4.5 を使用したため、提案の一貫性を高めて学習体験を整える目的で手順に記載しています。ただし、LLM は確率的に動作するため、ワークショップの例と異なる提案が表示される場合があります。これは正常で想定される動作です。

> [!NOTE]
> LLM の確率的な性質により、Copilot は異なる MCP コマンドを使う場合がありますが、同じタスクを完了できます。

3. 次の prompt を Copilot に送信して、Issue のバックログを確認します。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. **Continue** を選択して、すべての Issue を一覧表示するコマンドを実行します。
5. Copilot が生成した Issue の一覧を確認します。

Copilot がユーザーにとって有用だと判断した項目を優先順位付けしたことがわかります。

## Instruction file を確認する

Copilot にコード生成を依頼する前に、作業のコンテキストを伝える instruction file を確認します。Web サイトへの機能追加の進め方を説明する [user interface (UI)][ui-instructions] instruction file を使います。

1. codespace で `.github/instructions/ui.instructions.md` を開きます。
2. 機能追加の全体的なガイダンスを確認します。次の内容が含まれます。
   - アーキテクチャの概要
   - component 設計、テスト容易性、アクセシビリティの原則
   - ファイル種別ごとの instruction へのリンク。次のものが含まれます。
     - Astro
     - Tailwind CSS
     - Drizzle

> [!TIP]
> Instruction file では、ほかの instruction file やプロジェクト内のファイルを参照できます。パスは instruction file の場所からの相対パスです。これにより再利用が可能になり、複雑な指示を小さな管理しやすい単位に分け、Copilot に例やテンプレートを示せます。

## フィルタリング機能を実装する

フィルタリングを完成させるには、少なくとも次の 3 つの変更が必要です。

- データ レイヤー（`src/lib/`）にフィルタリング ロジックを追加または調整する
- フィルタリング動作のテストを追加または更新する
- ゲーム一覧ページにフィルタリング UI を追加する

さらに、コードベースにマージする前にテストを実行して成功させる必要があります。Copilot Agent Mode にこれらの作業を依頼します。

1. 現在の Copilot 会話を続けるか、**New Chat** を選択して新しい会話を始めます。
2. **Add Context**、**Instructions**、**ui** の順に選択し、instruction file を追加します。

   ![UI instruction file を選択する例](../../../_images/ex3-select-instructions-file.png)

3. Chat ビューの agent dropdown で **Agent** が選択されていることを確認します。

   ![Chat ビューで agent picker を表示した画面](../../../_images/shared-chat-mode-selector.png)

4. モデルが **Claude Sonnet 4.5** のままであることを確認します。
5. バックログの関連 Issue に基づいて機能を実装するよう、次の prompt を Copilot に送ります。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot がプロジェクトを調査し、目的の機能に関係するファイルを探す様子を確認します。データ レイヤーの helper、UI、テストが見つかり、その後ファイルが変更されてテストが実行されます。

   ![Copilot がプロジェクトのファイルを調査する画面](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot はプロジェクトの調査、ファイルの変更、テストの実行などを行います。タスクやコードベースの複雑さによって数分かかる場合があります。作業中に、コード エディターに **Keep** と **Undo** が表示されることがあります。処理中に選択する必要はありません。Copilot の作業が完了すると、すべての変更に **Keep** または **Undo** が表示されます。

7. Copilot から求められたら、**Continue** を選択してテストを実行します。

   ![Copilot Chat でテスト実行の確認を求めるダイアログ](../../../_images/ex3-agent-mode-run-tests.png)

8. 作業中に一時停止したり、テストが失敗したりする場合があります。問題ありません。Copilot はタスクが完了してエラーが検出されなくなるまで、コード生成とテストを繰り返します。

   ![Copilot Agent Mode の Chat セッションが完了した画面](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 生成されたコードに問題がないか確認します。

> [!CAUTION]
> Copilot やほかの AI ツールが生成したコードは、必ずレビューしてください。

10. Web サイトが起動しているブラウザーに戻り、新しい機能を試します。
11. 動作とコードを確認したら、Copilot Chat ウィンドウで **Keep** を選択します。

## まとめと次のステップ

この演習では、GitHub Copilot Agent Mode を使って Tailspin Toys Web サイトに新しい機能を追加する方法を学びました。

- GitHub Copilot Agent Mode で UI とデータ レイヤーにまたがる機能を実装しました。
- Copilot Agent Mode がプロジェクトを調査し、関連ファイルを見つけ、複数ファイルにわたる変更を行うことを確認しました。
- コードベースにマージする前に、Copilot Agent Mode が生成した変更とテストをレビューしました。

次は [Playwright MCP サーバーで機能をテストし][next-lesson]、プルリクエストを作成します。

### ボーナスの探索 — ページネーションを実装する

ゲーム一覧が増えると、ページネーションが必要になります。この演習で学んだことを使って、Copilot にページネーションの実装を依頼します。次の点を検討してください。

- 既存の instruction file を使うなど、ベスト プラクティスに従います。
- ユーザーがページ サイズを選べるようにするか、固定値にするかを検討します。
- 希望する実装に必要なガイダンスを prompt に含めます。
- Copilot に変更を依頼してコンテキストを追加しながら、何度か反復する場合があります。Copilot を使う通常の開発フローです。

## リソース

- [Copilot の ask、edit、agent mode: 役割と使い分け][choose-mode]
- [VS Code の agent mode][vs-code-agent-mode]

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
