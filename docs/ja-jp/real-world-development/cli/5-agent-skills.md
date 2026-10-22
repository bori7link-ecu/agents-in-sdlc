---
title: "レッスン 5 - quality-checks skill をカスタマイズして使う"
description: "既存の quality-checks skill を確認し、レポート形式をカスタマイズして、フィルタリングを検証するために実行します。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

コードを書くこと以外にも、さまざまな作業があります。コードが動作することを手動で確認し、instruction file を使って標準に従うようにできました。次はテスト、lint、継続的インテグレーション (CI) のほかの作業にも対処します。

このような作業には **agent skill** が適しています。skill は、これらの操作を適切に実行する方法を Copilot に伝えます。

このレッスンでは、次のことを行います。

- 既存の `quality-checks` skill を確認します。
- 結果のレポート形式をカスタマイズします。
- skill を再読み込みして実行します。

## シナリオ

Tailspin Toys では unit test、lint、型チェックに `quality-checks` skill を使っています。チームは、結果を読みやすくするためにレポートを改善したいと考えています。

## Instruction、script、resource

Agent skill は、agent が必要に応じて読み込む、再利用可能なタスクの指示、実行可能な script、補助 resource をまとめたものです。基本的には、skill と同じ名前のフォルダーと、`SKILL.md` という Markdown ファイルで構成されます。Markdown ファイルには、skill の名前と説明を定義する frontmatter、skill の概要、呼び出すタイミングのガイダンスを記載します。フォルダーに script やその他の resource を含むサブフォルダーを追加することもできます。

> [!NOTE]
> skill に追加のフォルダーやファイルは必須ではありません。Tailspin Toys の `quality-checks` skill は、プロジェクト既存のコマンドを使うため `SKILL.md` だけで構成されています。

Skill はプロジェクトの `.github/skills` フォルダーに配置し、チームで共有・再利用できます。また、ユーザー共通の `~/.copilot/skills` フォルダーにも配置できます。

## Skill を確認する

Tailspin Toys チームが unit test、lint、型チェックを実行するために作成した `quality-checks` skill を確認します。

1. Codespace に戻ります。Codespaces editor で `.github/skills/quality-checks/SKILL.md` を開きます。
2. 冒頭の `name` と `description` を読みます。description は Copilot が skill をいつ呼び出すか判断する助けになります。
3. 指示を読み、テストと lint の手順をどのように Copilot に案内しているかを確認します。
4. skill に **Results output formatting** セクションがまだないことを確認します。

## 変更前に skill を実行する

Skill は Copilot CLI から直接呼び出すことも、自然言語で依頼することもできます。skill の 3 つのチェックを実行するよう Copilot に依頼します。

1. フィルタリングの会話に戻り、Interactive mode であることを確認します。
2. 次の prompt を送信します。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 最後に表示されるレポートを確認します。

## レポートをカスタマイズする

どの処理を実行し、成功したかどうか、ツールが何を報告したかがわかるレポートに改善します。

1. `.github/skills/quality-checks/SKILL.md` を開きます。
2. ファイル末尾に次のセクションを追加します。

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. ファイルは自動的に保存されます。

## 更新した skill を実行する

変更を加えたので、実際に試します。Copilot CLI は会話を再起動せずに、編集した skill を再読み込みできます。

1. 次を入力します。

   ```plaintext
   /skills reload
   ```

2. 変更前と**まったく同じ prompt** を送信します。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 最後に表示されるレポートを確認し、最初のレポートと比較します。

## まとめと次のステップ

既存の agent skill をカスタマイズして実行しました。このレッスンでは、次のことを行いました。

- unit test、lint、型チェックを実行する `quality-checks` skill を確認しました。
- 結果のレポート形式をカスタマイズしました。
- skill を再読み込みして実行しました。

この変更はフィルタリング機能の PR に含めます。次は [Playwright MCP server を使って Copilot にサイトを操作させます][next-lesson]。

## Skill の追加例

次のコミュニティの例は参照用であり、追加の作業ではありません。

- [Agent Skills 仕様][skill-spec]
- [コントリビューションのワークフロー: `make-repo-contribution`][contribution-example]
- [要件ドキュメント: `prd`][prd-example]
- [図と同梱の export script: `drawio`][drawio-example]
- [ブラウザー テスト: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
