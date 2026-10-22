---
title: "演習 5: GitHub Copilot の作業を反復して改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

## 作業をレビューする

このラボでは、ユーザー体験の向上と機能の追加を目的に、GitHub Copilot と複数のタスクに取り組んできました。コードにドキュメントを追加し、デザイン チームが改善できる関連ゲーム機能を作成し、ハイ コントラストとライト モードの切り替えを含むアクセシビリティ機能を実装するよう依頼しました。変更を確認し、必要に応じて Copilot にフィードバックを与えて改善しましょう。

### シナリオ

生成 AI を使っても、ソフトウェア設計と DevOps の基本は変わりません。Copilot が生成したコードをレビューし、通常の DevOps プロセスを進めます。GitHub Copilot が作成したドキュメント、関連ゲーム機能、アクセシビリティ機能の提案を確認して、チームのレビューを有効にする前に変更を評価しましょう。

## GitHub Copilot cloud agent のセキュリティ

Copilot cloud agent は非同期に、監視されずにタスクを進めるため、安全性を保つための制約があります。

- Copilot はリポジトリへの読み取りアクセスを持ち、書き込みアクセスは作業用ブランチに**限られます**。
- Cloud agent は GitHub Actions 内で実行され、作業用に独立した一時的な環境を作成します。
- GitHub Actions workflow の実行には、人間による承認が必要です。
- [外部リソースへのアクセスは、既定で制限されています](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)。MCP サーバーも対象です。

## 生成されたドキュメントをレビューする

まず GitHub Copilot cloud agent が作成した最初のプルリクエスト (PR)、つまりコードへのドキュメント追加を確認します。GitHub.com の標準的な PR インターフェイスを使います。

> [!NOTE]
> PR を確認するとき、GitHub Copilot がファイアウォールによってブロックされているという警告が表示される場合があります。Copilot は外部 MCP サーバーへの呼び出しなど、外部リソースへのアクセスが既定で制限されているため、これは**想定される動作です**。必要であれば、[Copilot cloud agent のファイアウォールをカスタマイズまたは無効化][agent-firewall]できます。

1. github.com の自分のリポジトリに戻ります。
2. **Pull Requests** を選択してプルリクエストの一覧を開きます。
3. **Add missing documentation** のようなタイトルの PR を開きます。

> [!NOTE]
> Copilot がまだ作業中の場合、PR のタイトルに **[WIP]** が含まれています。その場合は作業が完了するまで待ちます。数分かかることがあります。

4. PR の準備ができたら、**Files changed** タブを選択して変更をレビューします。

    ![「Files changed」タブ](../../../_images/shared-pr-files-changed.png)

5. TSDoc の doc comment など、ドキュメントが追加されたコードを確認します。具体的な変更は状況によって異なります。

   変更を確認するときは、TSDoc の doc comment、TypeScript の規約、コメント ヘッダーに注目します。これらは演習 2 の最初に確認した custom instruction file によるものです。

6. 変更を確認して問題がなければ、**Conversation** タブに戻り、下にスクロールします。
7. workflow の承認待ちを示す表示があるか確認します。
8. 承認待ちがある場合は、**Approve and run workflows** を選択します。

   ![workflow の実行を承認する](../../../_images/shared-approve-workflows.png)

9. PR のチェック欄で workflow が実行待ちになったことを確認します。単一の Astro app のプロジェクト チェックが成功するまで待ちます。完了まで数分かかることがあります。

## GitHub Copilot に変更を依頼する

プルリクエストで Copilot とやり取りするのは一方通行ではありません。チームメンバーと同じように、PR のコメントやコードへの inline comment で Copilot にメンションできます。Copilot はコメントを確認し、対応するための新しいセッションを開始します。結果は確定的ではないため、依頼文を厳密に指定することはできません。依頼例を次に示します。

- 各コード ファイルの先頭に、役割を説明するコメント ヘッダーを追加する。
- TypeScript と Astro のファイルに TSDoc の doc comment を追加する。
- Astro app の構成を説明する README を作成する。

1. 上記の例などを使い、生成されたドキュメントの変更を依頼するコメントを追加します。通常のユーザーと同じように **@copilot** をメンションします。
2. **View Session** を選択して Copilot の作業を確認します。Copilot が新しいセッションを開始して変更を行います。
3. **Back to pull request** を選択すると、プルリクエストに戻れます。

    ![プルリクエストに戻る](../../../_images/ex7-back-to-pr.png)

4. Copilot の作業が終わると、PR に新しい commit が表示されます。
5. **Files changed** タブを選択して変更をレビューします。

満足するまで、必要に応じて改善を繰り返します。問題がなければ、PR を draft から ready に変更して `main` ブランチにマージできます。

![PR をレビュー可能な状態に変更する](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

ゲーム詳細ページに関連ゲームを表示する Issue に対して Copilot が作成した PR を確認します。

1. GitHub.com の自分のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Show related games on the game details page** のようなタイトルの PR を選択します。
4. **Files changed** タブを選択して、生成されたコードを確認します。
5. 変更をレビューして問題がなければ、**Conversation** タブに戻り、下にスクロールします。
6. workflow の承認待ちを示す表示があるか確認します。
7. 承認待ちがある場合は、**Approve and run workflows** を選択します。

   ![workflow の実行を承認する](../../../_images/shared-approve-workflows.png)

8. PR のチェック欄で workflow が実行待ちになったことを確認します。単一の Astro app のプロジェクト チェックが成功するまで待ちます。完了まで数分かかることがあります。
9. **任意:** codespace でブランチを切り替え、関連ゲーム機能を手動でテストできます。codespace に移動してターミナルを開き、Copilot が作成したブランチ名（例: **copilot/fix-8**）を `<branch-name>` に入れて、次のコマンドを実行します。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot が関連ゲーム機能を作成しました。cloud agent に追加の変更を依頼して反復できます。たとえば、表示する関連ゲームの数の調整や、コメント ヘッダーと TSDoc の doc comment の追加を依頼できます（この Issue を割り当てた時点では、custom instruction を更新する前でした）。前と同様、**Conversation** タブにコメントを追加すると、Copilot が確認して新しいセッションを開始します。

## アクセシビリティ機能をレビューする

最後に、アクセシビリティ custom agent が実装した機能を確認します。この PR には、演習 3 で依頼したハイ コントラスト モードと、演習 4 の mission control から依頼したライト モードが含まれているはずです。

1. GitHub.com の自分のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Add high contrast mode to website** のようなタイトルの PR を選択します。

> [!NOTE]
> Copilot がまだ作業中の場合、PR のタイトルに **[WIP]** が含まれています。完了するまで待ちます。数分かかることがあります。

4. **Files changed** タブを選択して、生成されたコードを確認します。
5. 次の点に注目して実装をレビューします。
   - モードを切り替える UI component
   - ユーザー設定を保持する local storage の使用
   - ハイ コントラスト モードとライト モード用の CSS またはスタイル変更
   - アクセシビリティ属性（ARIA label、キーボード操作など）
   - モード切り替えを管理する JavaScript/TypeScript コード

6. 変更を確認して問題がなければ、**Conversation** タブに戻り、下にスクロールします。
7. workflow の承認待ちを示す表示があるか確認します。
8. 承認待ちがある場合は、**Approve and run workflows** を選択します。

   ![workflow の実行を承認する](../../../_images/shared-approve-workflows.png)

9. PR のチェック欄で workflow が実行待ちになったことを確認します。単一の Astro app のプロジェクト チェックが成功するまで待ちます。完了まで数分かかることがあります。
10. **任意:** codespace でブランチを切り替えて、アクセシビリティ機能を手動でテストできます。codespace に移動してターミナルを開き、Copilot が作成したブランチ名を `<branch-name>` に入れて、次のコマンドを実行します。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

その後、アプリケーションを起動し、ハイ コントラストとライト モードの切り替えが想定どおり動き、ページを再読み込みしても設定が保持されることを確認します。

アクセシビリティ custom agent が、アクセシビリティのベスト プラクティスに沿って機能を実装するよう Copilot を導いたことがわかります。アクセシビリティ上の問題や改善点があれば、前の PR と同じようにコメントで **@copilot** に変更を依頼できます。

## 任意の演習 — 委任を続ける

Cloud agent に実際のバックログ項目を任せ、自分は別の作業に移ると効果的です。習慣を身につけるため、リポジトリにいくつか Issue を作成して Copilot に割り当ててみましょう。

- ゲーム詳細ページに支援者向けの問い合わせフォームを作成する
- ゲーム一覧ページにページネーションを実装する
- データアクセス helper に入力検証とエラー処理を追加する

## まとめ

Cloud agent harness を完了しました。このレッスンでは、次のことを行いました。

- **このリポジトリの custom instruction file を確認しました。** 後で cloud agent の出力にどう反映されるかを確認するためです。
- **Issue を Copilot cloud agent に割り当てました。** 環境を準備して、非同期に計画・実行する様子を確認しました。
- **アクセシビリティ用の custom agent を作成して使用しました。** ハイ コントラストとライト モードの切り替えを追加しました。
- **Copilot agents ページを mission control として使いました。** アクセシビリティのセッションを監視し、作業中に指示を出しました。
- **cloud agent のプルリクエストをレビューし、反復して改善しました。** `@copilot` に変更を依頼し、workflow の実行を承認しました。

## 振り返りと次のステップ

Cloud agent harness は完了です。さらに学びたい場合は、ほかの harness で関連するシナリオを試せます。

- 🖥️ **[VS Code harness](../../vscode/)** — IDE から Copilot Chat の agent mode と MCP 統合を試します。
- 💻 **[CLI harness](../../cli/)** — Plan と Autopilot のモード、agent skill、QA custom agent、Playwright MCP、Agent Merge、cloud agent に戻る任意の `/delegate` 機能を使い、ターミナルから 2 つの変更をレビューしてリリースします。

自分のリポジトリでは、次のような作業も試せます。

- リファクタリングやテスト範囲に関する Issue を cloud agent に割り当てる
- 別分野向けの custom agent を作成する
- 別の技術スタック向けに `copilot-setup-steps.yml` を設定する

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot agent について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップ workflow のベスト プラクティス][cloud-agent-best-practices]
- [Copilot cloud agent のファイアウォールを設定する][agent-firewall]

---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
