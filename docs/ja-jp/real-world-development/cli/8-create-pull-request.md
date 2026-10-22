---
title: "レッスン 8 - 機能の PR を作成してマージする"
description: "フィルタリング、instruction、skill の更新、QA profile、テストをまとめてレビューし、PR を作成して Agent Merge を使います。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

フィルタリングの実装、instruction の更新、skill の更新、quality assurance (QA) profile、テストは、1 つのブランチに保存されています。まとめてレビューし、プルリクエストを作成しましょう。星評価の PR は手動でマージしましたが、今回は **Agent Merge** にプロセスを管理させます。

> [!NOTE]
> 通常は機能、instruction の更新、skill の更新、QA agent を複数の PR に分けます。このワークショップでは、要点に集中できるよう、フィルタリングと品質向上の作業を 1 つの会話とブランチにまとめています。

このレッスンでは、次のことを行います。

- Agent Merge と、マージまでの流れを自動化する仕組みを学びます。
- 機能全体の変更と検証結果を確認します。
- フィルタリングの PR を作成します。
- レビュー後に Agent Merge を有効にして、PR がマージされたことを確認します。

## シナリオ

フィルタリング機能の作業を通じて、Copilot で機能の計画、実装、検証を行ってきました。Tailspin Toys では、残りの PR 作業を自動化しつつ、マージの許可は開発者が管理します。

## Agent Merge の概要

**Agent Merge** は、プルリクエストをマージするために必要な残りの作業を自動化します。有効にすると、Copilot は PR を妨げている要因に対応します。たとえば、失敗した継続的インテグレーション (CI) のチェックを修正し、レビューコメントに対応し、必要に応じて rebase します。その後、リポジトリで許可されていれば GitHub の auto-merge を有効にします。

これまでは **Merge pull request** を自分で選択してきました。Agent Merge にその作業を任せる場合も、有効にする前に変更をレビューし、準備ができていることを確認してください。

## Agent Merge で PR を管理する

コードが揃ったので、まとめてレビューして PR を作成し、残りのプロセスを Agent Merge に任せます。

1. Codespace に戻ります。
2. `/agent` を入力して agent ダイアログを開きます。
3. 選択肢から **Default** を選択し、<kbd>Enter</kbd> を押します。
4. `/pr create` を使って新しい PR を作成します。
5. `/pr agentmerge` を使って Agent Merge を有効にします。
6. Copilot が PR の継続的インテグレーションを監視します。すべて成功すると、マージが実行されます。
7. Copilot から「PR #14 was squash-merged successfully.」のようなメッセージが届いたことを確認します。

> [!IMPORTANT]
> Agent Merge は、必須の承認、ブランチ保護、マージ キュー、リポジトリ設定、権限不足を回避しません。処理がブロックされた場合は、表示された理由を確認し、リポジトリで許可されていればレビュー済みの変更を手動でマージしてください。

## まとめと次のステップ

コードの生成、テストと検証、プルリクエストの処理など、開発プロセスの複数の部分を自動化しました。このレッスンでは、次のことを行いました。

- Agent Merge と、マージまでの流れを自動化する仕組みを学びました。
- 機能全体の変更と検証結果を確認しました。
- フィルタリングの PR を作成しました。
- レビュー後に Agent Merge を有効にして、PR がマージされたことを確認しました。

次は、コンテキスト、モデル、共有、cloud agent への任意の委任に使える [Copilot CLI の slash command][next-lesson] を確認します。

## リソース

- [Copilot CLI でプルリクエストを管理する][manage-prs]
- [Copilot CLI コマンド リファレンス][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
