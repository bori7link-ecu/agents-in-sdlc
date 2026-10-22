---
title: "レッスン 9 - GitHub Copilot CLI の slash command"
description: "Copilot CLI のコンテキスト管理、モデル選択、セッション共有、cloud agent への任意の委任に使う slash command を確認します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

便利な CLI ツールと同様に、GitHub Copilot CLI には多くの slash command が用意されています。高度な機能、内部状態に関する情報、追加の設定オプションを使えます。これまでに `/diff`、`/mcp`、`/skills`、`/agent`、`/pr` などを使いました。ここでは、ほかの便利なコマンドをいくつか確認します。

このレッスンでは、次のことを行います。

- `/context` と `/compact` で、Copilot が会話のコンテキストをどう管理するかを確認します。
- `/model` で利用できるモデルを確認します。
- `/share` でセッションをエクスポートまたは共有する方法を学びます。
- 並行作業、worktree、cloud agent への委任に使う任意のコマンドを確認します。

## シナリオ

CLI の中心的なワークフローは完了しました。ここでは、コンテキスト管理、モデルの切り替え、セッション共有、[Copilot cloud agent][about-cloud-agent] への任意の委任など、追加機能を確認します。

## Copilot CLI のコンテキストを確認する

大きなタスクや複雑なタスクでは、モデルの最大 context window に近づくことがあります。Copilot CLI は必要に応じて会話を自動的に compact します。slash command を使うと、状態を確認したり、手動で compact したりできます。

1. Codespace に戻り、まだ起動していない場合は repository root から Copilot CLI を起動します。
2. 次を入力します。

   ```plaintext
   /context
   ```

3. 使用中のモデル、現在の token 使用量、system instruction、tool、message、空き領域の間でコンテキストがどう分けられているかを確認します。
4. 会話を compact します。

   ```plaintext
   /compact
   ```

5. もう一度 `/context` を入力し、結果を比較します。会話が短い場合は大きな変化がないことがあります。

> [!NOTE]
> Copilot CLI は context window が埋まると自動的に compact します。タイミングを自分で決めたい場合は `/compact` を使います。無関係なタスクに移るため会話を新しくしたい場合は、`/clear` または `/new` を使います。

## モデルを選ぶ

モデルごとに強みが異なり、開発者によって好みも異なります。Copilot CLI では利用可能なモデルを確認し、使用するモデルを選択できます。

1. 次を入力します。

   ```plaintext
   /model
   ```

2. 利用可能なモデルと使用量に関する情報を確認します。
3. 現在のモデルを維持するか、別のモデルを選ぶか、<kbd>Esc</kbd> を押して一覧を閉じます。

## セッションを共有する

チームで協力して学びを共有すると、AI ツールの活用方法を改善できます。`/share` コマンドは、セッションを Markdown または HTML ファイルにエクスポートしたり、共有可能な link を作成したり、GitHub gist として公開したりできます。

1. `/help` を入力し、インストール済みのバージョンで利用できる `/share` のオプションを確認します。
2. セッションを共有する場合は、目的に合った方法を選びます。たとえば `/share file` はローカルの Markdown ファイルにエクスポートします。
3. エクスポートした内容を、送信または公開する前に確認します。session export には prompt、応答、プロジェクトの詳細が含まれる場合があります。

Link や gist の公開は任意です。チームが共有する予定のないリポジトリや会話の内容を公開しないでください。

## 任意: 作業を並列化または委任する

ワークショップの中核部分は完了しました。Copilot CLI には、大きなタスク向けのコマンドもあります。

- `/fleet` は、独立したサブタスクを subagent に割り当てて並列実行できます。
- `/worktree` は、別のタスク用に独立した Git worktree を作成できます。
- `/delegate` は、タスクを Copilot cloud agent に送信します。cloud agent は非同期で作業し、プルリクエストを作成する場合があります。

これらのコマンドは worktree やリモートの作業を追加することがあるため任意です。試す場合は、範囲を明確にした新しいタスクから始め、通常のワークフローで結果をレビューしてください。非同期の agent 作業について詳しく学ぶには、[Cloud agent workshop][cloud-workshop]に進みます。

## まとめと次のステップ

Copilot CLI の slash command を使うと、設定を行い、セッションを共有し、内部状態を確認できます。このレッスンでは、次のことを行いました。

- `/context` と `/compact` で、Copilot が会話のコンテキストをどう管理するかを確認しました。
- `/model` で利用できるモデルを確認しました。
- `/share` でセッションをエクスポートまたは共有する方法を学びました。
- 並行作業、worktree、cloud agent への委任に使う任意のコマンドを確認しました。

Copilot CLI には、さらに多くの slash command と機能があります。最後に[これまでに学んだことを振り返り][next-lesson]、次のステップを確認しましょう。

## リソース

- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI のコンテキスト管理][context-management]
- [Copilot cloud agent について][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
