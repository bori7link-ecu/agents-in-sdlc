---
title: "演習 5 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: Custom agent][previous-lesson] |
|:--|

GitHub Copilot を agent mode で使うと、コードベースの調査、変更の提案、ファイルの編集、コマンドの実行を自律的に行います。作業が自分のマシンでリアルタイムに行われるため、VS Code の Copilot Chat では各ツール呼び出しとファイルの編集を確認できます。差分を inline で確認し、個々の変更を適用または取り消し、追加の prompt で会話を調整・拡張できます。エディターを離れる必要はありません。

この演習では、次のことを行います。

- アクセシビリティ agent の作業をリアルタイムで監視します。
- 提案されたファイル編集をレビューして適用します。
- セッションの途中で agent に指示し、ハイ コントラストに加えてライト モードの切り替えを追加します。

## シナリオ

[演習 4][previous-lesson]で使ったアクセシビリティ custom agent は、ハイ コントラスト モードを実装しています。作業中にデザイン チームから追加の要望がありました。既定のテーマが暗すぎると感じるユーザー向けに、ライト モードの切り替えも追加したいとのことです。新しい会話を始めずに、既存のセッションに指示を出して作業を拡張します。

## Copilot の進捗を監視する

Copilot Chat ビューには、agent が調べているファイル、実行しているコマンド、提案した編集など、作業の各段階が表示されます。ファイルの編集は差分と **Keep** / **Undo** 操作とともに Chat に表示されます。

1. 演習 4 から Copilot Chat ビューに戻ります。
2. agent の作業中に Chat をスクロールし、ツール操作を確認します。ファイルの読み込みや編集、実行を求められるターミナル コマンドが表示されます。
3. agent がファイルの編集を提案すると、差分が inline で表示されます。
   - **Keep** を選択して変更を適用します。
   - **Undo** を選択して変更を元に戻し、agent に再試行させます。
4. Chat 上部の **Files changed** を開くと、このセッション中に agent が変更したファイル全体を確認できます。

> [!NOTE]
> ツール操作の許可（たとえばターミナル コマンドの実行）を求められると、agent は一時停止して **Allow** を表示します。内容を確認し、許可してもよいコマンドだけを承認してください。

5. agent がハイ コントラスト モードの実装を終えるまで待ちます。最後に変更内容をまとめたメッセージが表示されます。

## ライト モードを追加するよう agent に指示する

ハイ コントラスト モードができたら、同じ会話でライト モードの切り替えも追加します。既存の会話を続けるため、agent は前の作業で変更した component、スタイルの方法、設定を local storage に保存する実装などのコンテキストを保持しています。

1. Copilot Chat の入力欄に、次の follow-up prompt を送信します。

    ```plaintext
    Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
    ```

2. agent は既存の変更を確認し、ライト モードの追加を提案します。
3. ハイ コントラストのときと同じように、新しい変更を確認します。各差分で **Keep** または **Undo** を使います。
4. agent が意図と異なる方向に進んだ場合（たとえば、気に入らない方法で toggle UI をリファクタリングする場合）は、Chat 入力欄の横にある **Stop** ボタンで停止し、明確な follow-up message を送ります。

> [!TIP]
> 進行中のセッションに指示を出すと、最初から始めるより短時間で修正できます。follow-up prompt で調整（「CSS を簡潔にし、selector を重複させず CSS variable を使う」）、方向修正（「3 つ目の toggle は追加せず、既存の設定パネルにライト モードを統合する」）、範囲の拡張（「両方の toggle にキーボード ショートカットも追加する」）を依頼できます。

## 作業対象をレビューする

作業を commit する前に、agent が変更したファイルを確認します。

1. VS Code で **Source Control** ビューを開きます。
2. 変更されたファイルの一覧を確認します。Astro component、スタイル、関連テストなどが含まれます。
3. 変更されたファイルをいくつか開いて差分を確認します。アクセシビリティ agent が示した ARIA 属性、キーボード操作、semantic HTML、local storage による永続化などが反映されていることを確認します。
4. 問題がなければ、Source Control パネルで変更を stage して commit します。次のレッスンで公開します。

## まとめと次のステップ

Copilot Chat の agent mode を使って、進行中のセッションを監視し、指示を出しました。各ファイル編集を確認しながら適用・取り消しを選び、コンテキストを失わず follow-up prompt で作業を拡張しました。

このレッスンでは、次のことを行いました。

- agent の作業をリアルタイムで監視しました。
- 個々のファイル編集を適用または取り消しました。
- follow-up prompt で作業中のセッションに指示を出しました。

次のレッスンでは、[アクセシビリティ対応の変更をプルリクエストとして公開し][next-lesson]、このラボでローカル custom agent が作成した変更をレビューします。

## リソース

- [VS Code の Copilot Chat][copilot-chat-vscode]
- [Agent mode を使う][agent-mode]

---

| [← 前のレッスン: Custom agent][previous-lesson] | [次のレッスン: Copilot の作業を反復して改善する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
