---
title: "レッスン 10 - 振り返りと次のステップ"
description: "Copilot CLI のワークフロー、2 つの pull request、再利用可能なカスタマイズ、追加のリソースを振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Tailspin Toys の一連のワークフローで GitHub Copilot CLI を使いました。次のことを行いました。

- Codespace を準備して Copilot CLI をインストールし、プロジェクトを調べて、seed 済みのフィルタリング Issue を確認しました。
- 星評価を追加し、転送されたブラウザーで結果を確認して、最初のプルリクエスト (PR) を手動でマージしました。
- フィルタリングの Issue から作業を開始し、Plan mode で方針を決め、Autopilot mode で実装して、Interactive mode でレビューしました。
- Custom instruction で agent を導き、既存の `quality-checks` skill をカスタマイズして、unit test、lint、型チェックを実行しました。
- Playwright Model Context Protocol (MCP) server を追加し、実際のブラウザーでフィルタリングを確認しました。
- quality assurance (QA) custom agent を作成して選択し、要件、テスト範囲、skill の結果、ブラウザーでの検証結果を評価しました。
- フィルタリングの変更全体をレビューし、フィルタリング PR のために Agent Merge を許可しました。
- コンテキスト、モデル、共有、cloud agent への任意の委任に使う slash command を確認しました。

## リリースした変更

ワークショップでは、更新済みの `main` からそれぞれ別のブランチを作り、2 つの PR を作成しました。

1. **星評価**: 既存の `starRating` と、評価がない場合の明示的な表示をゲーム カードに追加します。
2. **フィルタリングと品質ワークフロー**: フィルタリングを実装し、instruction を更新して機能に適用します。`quality-checks` のレポートをカスタマイズし、QA profile を作成して関連テストを追加します。

フィルタリングの計画から PR の作成まで、同じ会話とブランチを使いました。ワークショップを簡潔にするため、これらの作業は 1 つの PR にまとめました。

## さまざまな検証方法

自動テスト、自分でのブラウザー確認、MCP を使った Copilot のブラウザー確認など、複数の方法でコードを検証しました。`quality-checks` skill は unit test、lint、型チェックを実行し、カスタマイズした形式で結果を報告しました。QA agent は要件とテスト範囲のレビューにそれらの結果を加え、PR 作成前に確認しました。

追加したテストは、実際に不足していた範囲を補う必要があります。QA の実行時にテストを追加する必要がないと判断されることもあります。コードと根拠を確認してからマージを許可し、変更後は影響を受ける検証結果を更新してください。

## ベスト プラクティス

Copilot に渡すコンテキストやツールは、その作業に影響します。このワークショップでは、instruction を更新し、skill をカスタマイズし、QA profile を作成し、MCP server を設定しました。これらのカスタマイズは会話をまたいで再利用し、チームのニーズに合わせて調整できます。Instruction は標準を定め、skill は繰り返し可能なタスクを説明し、custom agent は専門的な役割を定義し、MCP server は外部ツールに接続します。agent の要約だけでなく、実際の変更とツールの結果をレビューしてください。

タスクに合う **mode と model** を選びます。構築前に方針を検討するには **Plan**、範囲を絞った変更に自分も参加するには **Interactive**、範囲が明確なタスクを任せるには **Autopilot** を使います。定型的な編集には高速なモデル、複雑な作業にはより高性能なモデルを選びます。

基盤と同じくらい、コンテキストも重要です。何を作るか、なぜ必要か、どのように進めるかを明確に伝えると、出力に大きく影響します。

## さらに試す

中核となるワークフローを学びました。次の CLI 機能も確認できます。

- `/review` で code review agent に変更を分析させます。
- `/rubber-duck` で問題について話し合い、別の視点を得ます。
- `/fleet` で独立したサブタスクを並列に調整します。
- `/worktree` で別のタスクを分離します。
- `/delegate` でタスクを Copilot cloud agent に送ります。

## 次のステップ

どのツールでも、スキルを高める最善の方法は使い続けることです。本番コード、趣味のコード、何年も頭の中にあったけれどまだ形にしていない小さなアプリなど、さまざまな場面で活用してください。学びをチームと共有し、チームからも学んでください。そして、いつものようにドキュメントを確認しましょう。

ターミナルから Tailspin Toys を拡張したい場合は、任意の[Foundry Backer Concierge シリーズ][foundry]に進みます。ほかの環境を比較するには、[VS Code ワークショップ][vscode]、[GitHub Copilot app ワークショップ][app]、[Copilot cloud agent ワークショップ][cloud]を確認してください。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI をカスタマイズする][customize-cli]
- [Copilot CLI でプルリクエストを管理する][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
