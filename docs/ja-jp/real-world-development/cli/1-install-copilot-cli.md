---
title: "レッスン 1 - GitHub Copilot CLI をインストールする"
description: "Codespace に Copilot CLI をインストールして認証し、基本を確認して、登録済みのフィルタリング Issue を見つけます。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

[GitHub Copilot CLI][about-copilot-cli] は、ターミナルで動作するエージェント型のコーディング アシスタントです。コードベースの調査、コードの生成、コマンドの実行、外部ツールとの連携を、すべてコマンド ラインから行えます。多くの開発者が日常的に使うターミナルで、AI agent の機能を活用できます。このレッスンでは、Copilot CLI をインストールして認証し、ワークショップで使う準備をします。

この演習では、次のことを学びます。

- npm を使って GitHub Copilot CLI をインストールする。
- GitHub アカウントで認証する。
- ワークショップのリポジトリを信頼し、Copilot CLI との会話を試す。
- GitHub MCP server を使って、フィルタリングの Issue を見つける。

## シナリオ

チームでは、増え続けるバックログに対応するために AI agent を使い始めています。Copilot CLI はその機能を、多くの開発者が日常的に使うターミナルに持ち込みます。このレッスンでは、インストールと認証を済ませ、ワークショップの残りで使える状態にします。

## Copilot CLI をインストールする

Copilot CLI は [npm][install-cli]、WinGet、Homebrew でインストールできます。GitHub Codespaces には Node.js があらかじめインストールされているため、このレッスンでは npm を使います。

1. Codespace に戻り、ターミナルを開きます。
2. Node.js がインストールされ、必要なバージョンを満たしていることを確認します。

   ```bash
   node --version
   ```

   バージョン 24 以上が表示されます。

3. Copilot CLI をグローバルにインストールします。

   ```bash
   npm install -g @github/copilot
   ```

4. インストールを確認します。

   ```bash
   copilot --version
   ```

   バージョン番号が表示されます。

## GitHub で認証する

初回起動時に、Copilot CLI は GitHub アカウントでの認証を求めます。

1. Copilot CLI を起動します。

   ```bash
   copilot
   ```

2. 認証を求められた場合は、device code の手順に従って認証し、Copilot CLI を許可します。
3. Copilot CLI に次の確認が表示されます。

   ```plaintext
   Copilot can read files in this folder and, with your permission, edit them or run code and shell commands. It will remember your permissions for the rest of this session.

   Do you trust the files in this folder?
   ```

4. 表示されたパスが Tailspin Toys のリポジトリであることを確認し、**Yes, and remember this folder for future sessions** を選択します。

> [!NOTE]
> Codespace では、GitHub のセッションを通じてすでに認証されている場合があります。Copilot CLI が認証を求めずに起動した場合は、そのまま進めて問題ありません。

## 基本を確認する

通常のターミナル プロンプトで入力したコマンドは、Codespace 上で直接実行されます。Copilot CLI を起動すると、自然言語は agent に送られ、slash command は会話を操作します。

1. `/model` を入力し、矢印キーで **Auto** を選択して <kbd>Enter</kbd> を押し、もう一度 <kbd>Enter</kbd> を押して確定します。
2. `/help` を入力して、インストール済みのバージョンで使えるコマンドを確認し、<kbd>Esc</kbd> を押してヘルプ画面を閉じます。
3. Copilot に簡単な質問をして、正しく動作することを確認します。

   ```plaintext
   What are the key files in this project?
   ```

4. 回答を読み、Copilot がリポジトリを調査してから回答することを確認します。
5. `/mcp list` を入力して、組み込みの GitHub MCP server が利用できることを確認します。
6. Copilot にフィルタリングの Issue を探してもらいます。

   ```plaintext
   Using GitHub MCP, find the issue in this repository titled "Allow users to filter games by category and publisher." Give me its URL and a short summary. Don't change anything.
   ```

7. URL を開いて Issue を読みます。最初の変更を終えた後に、この Issue を使います。

> [!TIP]
> 通常の Copilot CLI セッションは、ターミナルで checkout しているブランチ上で動作し、自動的に worktree を作成しません。変更を加える前に、機能ブランチを作成します。

## ワークショップ用のショートカットを使う

通常、Copilot CLI は設定済みの権限を超えるツールを使う前に確認を求めます。このワークショップでは、Codespace 内の承認確認を省略して演習に集中できるよう、`--yolo` を付けて再起動します。

> [!CAUTION]
> `--yolo` はすべての権限を自動的に許可します（`--allow-all-tools`、`--allow-all-paths`、`--allow-all-urls`）。Codespace や VM などの分離された環境でのみ使用してください。日常の開発で既定の設定として alias に登録しないでください。詳細は[ツールの使用を許可または拒否する][allow-all-warning]を参照してください。

`--enable-all-github-mcp-tools` を指定すると、後のレッスンで Issue やプルリクエストを操作するために使う、読み取りおよび書き込み用の GitHub MCP ツールが有効になります。Codespace からローカル コンピューターへのアクセスは制限されますが、認証済みの GitHub リソースには実際にアクセスします。公開やマージの前に変更をレビューしてください。

1. `/exit` で Copilot CLI を終了します。
2. repository root から再起動します。

   ```bash
   copilot --yolo --enable-all-github-mcp-tools
   ```

3. プロジェクトについてもう一度簡単な質問を送り、会話が機能することを確認してから `/exit` で終了します。

Copilot は会話を自動的に保存します。後で instruction を変更したり agent を追加したりした後、`copilot --resume` で同じ機能の会話とブランチに戻ります。

## まとめと次のステップ

おめでとうございます。このレッスンでは、次のことを行いました。

- npm を使って Copilot CLI をインストールする。
- GitHub アカウントで認証する。
- ワークショップのリポジトリを信頼し、会話を試す。
- GitHub MCP server でフィルタリングの Issue を見つける。

次は[最初の変更を行い][next-lesson]、ゲーム カードに星評価を表示します。

## リソース

- [GitHub Copilot CLI のインストール][install-cli]
- [Copilot CLI について][about-copilot-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-add-star-rating/
[install-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/set-up-copilot-cli/install-copilot-cli
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[allow-all-warning]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli#allowing-and-denying-tool-use
