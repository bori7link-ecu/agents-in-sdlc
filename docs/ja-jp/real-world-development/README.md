---
slug: ja-jp/real-world-development
title: "実践的な開発"
authors:
  - geektrainer
lastUpdated: 2026-09-16
---

現実的なシナリオ、完成したアプリケーション、共有のバックログを使って、ソフトウェア開発ライフサイクル全体で GitHub Copilot を活用します。変更の計画、コードの作成、動作のテスト、プルリクエストのレビュー、エージェントを使った作業管理を繰り返しながら進めます。

## シナリオ

ボードゲームのクラウドファンディングを提供する架空の会社、Tailspin Toys に新しく加わった開発者として作業します。チームのバックログは GitHub Issue として登録済みです。フィルタリングやページネーションなどの機能に加え、アクセシビリティやコーディング標準などの品質向上タスクもあります。

## 作業環境を選ぶ

GitHub Copilot はさまざまな作業環境で利用できます。自分に合った環境を選んでください。各ワークショップは個別のセットアップから始まり、共通の Tailspin Toys シナリオを使います。

### [GitHub Copilot CLI][cli]

ターミナルでエージェント型のアシスタントを使います。Plan と Autopilot のモード、instruction、skill、custom agent、Playwright MCP、Agent Merge、実用的な slash command を扱います。

### [GitHub Copilot app][app]

デスクトップ アプリで複数のエージェント セッションを並行して実行し、セッション モードを切り替え、Canvas で共同作業し、GitHub Issue とプルリクエストを管理します。

### [GitHub Copilot cloud agent][cloud]

GitHub Issue を非同期エージェントに割り当て、作業を案内して進捗を監視し、作成されたプルリクエストをレビューします。

### [Visual Studio Code][vscode]

Visual Studio Code と GitHub Codespaces で GitHub Copilot を使います。エディターを離れずに Copilot Chat の agent mode、MCP サーバー、custom agent を扱います。

[cli]: cli/
[app]: app/
[cloud]: cloud/
[vscode]: vscode/
