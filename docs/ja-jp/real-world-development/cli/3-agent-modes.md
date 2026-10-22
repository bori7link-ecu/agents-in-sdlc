---
title: "レッスン 3 - Agent mode: Plan と Autopilot"
description: "Plan mode で方針を決め、Autopilot mode で Issue の機能を実装し、Interactive mode で結果をレビューします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

小さな機能をプロジェクトに追加するところから始めました。しかし、大きな変更には、より確実なプロセスが必要です。GitHub Copilot CLI は組織で既に使われているフローに沿って作業できるため、適切な方法で必要なものを構築できます。このレッスンは、一般的な agent 主導の開発プロセスをたどる複数レッスンの最初です。まず Issue を使って新しい機能を生成し、コードが正しく、期待どおりに動作することを確認して、最終的にプロジェクトにマージします。

> [!NOTE]
> 機能のワークフローを進める間、同じ会話とブランチを使います。通常は変更するファイルの種類ごとに異なるブランチや PR を作成しますが、ここでは中核となる概念に集中するため手順を簡略化します。

このレッスンでは、次のことを行います。

- GitHub Issue から新しい Copilot 会話を開始します。
- Plan mode で要件を定義します。
- Autopilot mode で新機能を実装します。
- コードをレビューします。
- 転送されたブラウザーで機能を手動検証します。

機能の作業を続けながら、リポジトリの instruction を更新し、既存の `quality-checks` skill をカスタマイズし、MCP を使った検証を追加し、QA agent を作成し、機能の PR を作成します。

## シナリオ

Tailspin Toys のカタログが増えているため、訪問者がゲームをカテゴリやパブリッシャーで絞り込めるようにします。バックログの Issue に機能の説明はありますが、カテゴリの組み合わせなど、実装前に合意が必要な詳細があります。Plan mode で決定し、Autopilot に範囲を限定した実装を任せます。

## 会話モード

**Conversation mode** は、agent にどれだけ自律性を持たせるかを制御します。<kbd>Shift</kbd>+<kbd>Tab</kbd> を押してモードを切り替えます。

- **Interactive**: agent と共同で作業します。agent は変更を提案し、続行前に入力を待ちます。
- **Plan**: agent が最初に計画を作成します。プロジェクト ファイルの編集はできません。
- **Autopilot**: agent がコードを書き、テストを実行し、タスクが完了するまで反復します。

最初に Plan mode を使い、計画をレビューしてから Autopilot で実装します。

## Issue から始める

フィルタリング作業を始める前に、Codespace に戻り、リポジトリとターミナルの準備ができていることを確認します。

1. Codespace に戻ります。停止している場合は再起動します。
2. 星評価の PR がマージ済みであることを確認します。
3. ターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押します。
4. `main` を更新してから、フィルタリング作業用のブランチを作成します。

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Copilot CLI を起動します。

   ```bash
   copilot --yolo
   ```

6. <kbd>Tab</kbd> を 2 回押して **Issues** タブを開きます。
7. <kbd>A</kbd> を押してすべての Issue を表示します。
8. 矢印キーで **Allow users to filter games by category and publisher** というタイトルの Issue を選択します。
9. <kbd>C</kbd> を押して Issue を prompt に追加し、**Session** タブに戻ります。

prompt の先頭に `#7`（または同様の番号）が追加されます。`#` を使うと、GitHub 上の Issue やプルリクエスト (PR) をコンテキストに追加できます。

## フィルタリング機能を計画する

計画を立てると、機能やタスクを Copilot に任せる前に実装方法を決められます。複雑な作業では、時間をかけて計画することをお勧めします。Plan mode に切り替えて、Copilot に計画を作成してもらいましょう。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Plan mode に切り替えます。prompt の下にあるモード表示が **Plan** であることを確認します。
2. 前の手順で追加した Issue reference の後に、次の prompt を入力します。

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot はプロジェクトを調査し、最適な方法を検討して計画を作成します。

3. フィルタリング機能の動作について Copilot が質問する場合があります。希望に基づいて回答してください。どの回答も間違いではありません。
4. 計画が完成したら、<kbd>Control</kbd>+<kbd>E</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) を押して展開します。
5. 上下にスクロールして計画をレビューします。
6. 希望と異なる箇所があれば、Copilot に修正を依頼します。

## Autopilot を承認する

計画ができてレビューも終わったら、実装に進みます。Autopilot を使って Copilot に作業を任せます。

1. **Accept plan and build on autopilot (recommended)**（またはインストール済みのバージョンで同様のラベルが付いた選択肢）を選びます。
2. prompt の下にあるモード表示が **Autopilot** であることを確認します。
3. Copilot が計画に沿って反復し、コードを生成してテストを実行する様子を確認します。

> [!NOTE]
> 承認すると、すぐに実装が始まる場合があります。必ず先に計画をレビューしてください。依存関係が不足している、またはポートが競合していると Copilot が報告した場合は、チェックを完了扱いにする前にセットアップの問題を解決します。

## 実装をレビューして検証する

コードの生成後、ほかのコードと同じように、マージ前にレビューします。コードを確認してテストを実行し、サイトが正しく表示されることも確認します。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Interactive mode にします。モード表示が **Plan** または **Autopilot** ではないことを確認します。
2. `/diff` を入力してフィルタリングの実装とテストを確認します。
3. 結果を Issue と計画時の決定に照らして確認します。
4. コードをレビューしたら <kbd>Esc</kbd> を押して diff view を閉じます。
5. プロジェクトのチェック結果を確認し、失敗があれば Copilot に修正を依頼します。

## 新機能を試す

コードはよさそうですが、実際に動作するでしょうか。前と同じようにアプリを起動し、Codespaces の転送ポート経由でサイトを開きます。

1. Copilot にアプリの起動を依頼します。

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces にポート `4321` が使用可能と表示されたら、**Open in Browser** を選択します。
3. カテゴリのフィルタリング、パブリッシャーのフィルタリング、計画で合意した組み合わせを試します。
4. リセット時と検索結果がない場合の動作が Issue と計画時の決定に一致することを確認します。
5. Codespace に戻り、Copilot が起動した開発サーバーを停止するよう依頼します。

## まとめと次のステップ

異なる conversation mode を使って機能を構築し、レビューしました。このレッスンでは、次のことを行いました。

- GitHub Issue から新しい Copilot 会話を開始しました。
- Plan mode で要件を定義しました。
- Autopilot mode で新機能を実装しました。
- コードをレビューしました。
- 転送されたブラウザーで機能を手動検証しました。

次は、[custom instruction を使ってコード生成の仕組みを確認します][next-lesson]。

## リソース

- [GitHub Copilot CLI の Autopilot][autopilot]
- [Copilot CLI コマンド リファレンス][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
