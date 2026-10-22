---
title: "演習 4 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前の 2 つの演習では、ユーザー体験の向上と機能追加に関する 3 つのタスクを Copilot cloud agent に依頼しました。cloud agent は非同期かつ自律的に動作しますが、タスクを監視することも重要です。

cloud agent に割り当てたタスクを管理するツールは複数あります。GitHub.com の [agents ページ][agents-page]もその 1 つです。この mission control では、開いているプルリクエスト (PR) があるすべての agent タスクを確認できます。実行された操作を調べ、進行中のセッションに指示を出すこともできます。

このレッスンでは、次のことを行います。

- agents ページを使って cloud agent のタスクを監視します。
- 進行中のセッションに追加機能を依頼します。

## シナリオ

ハイ コントラスト モードの作成を agent に依頼した後、チームはライト モードも追加したいと考えました。サイトのスタイルを更新し、切り替え機能を追加している途中なので、まとめて実装するのが合理的です。ハイ コントラスト モードとライト モードの両方を追加するよう agent に指示します。

## Copilot cloud agent のタスクを確認する

Copilot cloud agent に割り当てたタスクの現在の状態を確認します。

1. [Copilot agents ページ](https://github.com/copilot/agents)を開きます。
2. メイン ペインと左ペインにタスクの一覧が表示されます。次のような、Copilot に割り当てたタスクを確認します。
   - コードベースのドキュメントを更新する
   - ゲームを変更する API を生成する
   - Web サイトにハイ コントラスト モードを追加する
3. 実行中のタスクを選択し、Copilot が行った操作を確認します。次のような操作が含まれる場合があります。
   - リポジトリからコードを checkout する
   - Copilot が作業する環境を作成する
   - MCP サーバーを設定する
   - 割り当てられたタスクを完了するための各種手順を実行する

> [!NOTE]
> 表示される具体的な手順は、Copilot の作業状況や進め方によって異なります。

4. 右側に表示されるプルリクエスト (PR) ペインも確認します。ここで PR と変更されたファイルを確認し、作業の進捗を監視できます。

## Cloud agent に追加の指示を出す

実行中のタスクを確認したら、Copilot がハイ コントラスト モードを作業している間にライト モードの切り替えも追加するよう依頼します。

1. ハイ コントラスト モードの追加に関するセッションを選択します。表示されるタイトルは、Copilot が付けた名前や作業状況によって異なります。

    ![Mission control のアクセシビリティ セッション](../../../_images/ex6-accessibility-session.png)

2. セットアップが完了して作業を開始するまで、数分間セッションを確認します。作業が始まると、次のようなメッセージが表示されます。
3. **Steer active session while Copilot is working** ダイアログに、次の prompt を入力します。

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![agents ページで、作業中の cloud agent セッションに指示を出すダイアログ](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押して prompt を送信します。
5. Copilot が prompt を確認し、作業計画に加えたことを確認します。

## Copilot の作業を続ける

Copilot は更新されたタスクに取り組みます。メッセージを送信した時点で作業中だった手順を完了した後、新しい依頼を計画に反映します。

作業には数分かかります。その間に、これまで学んだ内容を振り返ることができます。

## まとめと次のステップ

Copilot agents ページを、GitHub Copilot cloud agent のタスクを監視して案内する中心的な場所として使う方法を学びました。実行中と完了済みのタスクを追跡し、作業内容を確認して、進行中のタスクの範囲を変更したり、追加のガイダンスを出したりできます。

このレッスンでは、次のことを行いました。

- mission control と agents ページで cloud agent のタスクを監視しました。
- 進行中のセッションに追加機能を依頼しました。

Copilot がアクセシビリティ機能を完成させている間に、[Copilot が作成したプルリクエストを改善する][next-lesson]方法を確認します。

## リソース

- [Copilot agents ページ][agents-page]
- [Custom agent について][custom-agents]

---

| [← 前のレッスン: Custom agent][previous-lesson] | [次のレッスン: Copilot の作業を反復して改善する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
