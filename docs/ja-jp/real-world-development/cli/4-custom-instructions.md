---
title: "レッスン 4 - Custom instruction で Copilot を導く"
description: "リポジトリの instruction を確認し、ドキュメント標準を追加して、フィルタリングのコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を使うときはコンテキストが重要です。タスクの進め方に決まりがある場合は、そのガイダンスを Copilot が参照できるようにします。[Instruction file][instruction-files]を使うと、どのコードを作るかだけでなく、コードをどのように構成するかも伝えられます。フィルタリング機能を作成したので、Copilot が使った instruction を確認し、ドキュメント標準を追加して、コードに適用します。

このレッスンでは、次のことを行います。

- リポジトリの instruction とパス単位で適用される instruction file が agent にどう届くかを確認します。
- コーディング標準に従うよう instruction file を更新します。
- instruction file がコードに与える影響を確認します。

## シナリオ

Tailspin Toys には、開発プラクティスに関するガイドラインと要件があります。たとえば次のとおりです。

- コメントは、コードの言い換えではなく、意図や自明でない判断を説明する必要があります。
- `db/` と `src/lib/` で export される関数は、TSDoc/JSDoc で目的、parameter、戻り値を説明します。`db` を注入する場合は、その引数も説明します。
- 再利用可能な Astro component は `Props` の契約を説明し、関連コードの変更に応じてコメントを最新に保ちます。
- 既存のフォーマットと lint のガイダンスを維持します。

Instruction file を使って、これらのプラクティスに沿ってタスクを進めるために必要な情報を Copilot に渡します。

## Instruction file

Custom instruction を使うと、コンテキストや設定を Copilot に渡し、コーディング スタイルや要件をより正確に理解させられます。コーディング規約、使用するライブラリ、含めたいコメントの種類などを指定できます。instruction はリポジトリ全体に適用することも、特定のファイル種別に対するタスク単位のコンテキストとして作成することもできます。

Instruction file には 2 種類あります。

- `.github/copilot-instructions.md` は、リポジトリに対する**すべて**のリクエストで Copilot に送られる単一の instruction file です。ほとんどのリクエストに共通するプロジェクト レベルの情報を記述します。
- `.github/instructions/*.instructions.md` ファイルは、特定の言語、ファイル種別、タスクに関するガイドラインを提供します。

> [!NOTE]
> ほかの形式の instruction とそのサポート状況は、環境によって異なります。特定の形式に依存する前に、[custom instruction のサポートに関するリファレンス][custom-instructions-support]を確認してください。

## このプロジェクトの custom instruction file を確認する

最初に、starter project に含まれる instruction file を確認し、変更前の状態を把握します。中核となる instruction file と、タスクごとの複数の instruction file が含まれています。

1. Codespace に戻ります。
2. Codespaces editor で（ターミナルではなく）`.github/copilot-instructions.md` を開きます。
3. ファイルを確認し、プロジェクトの説明とコーディング ガイダンスを読みます。この instruction は、このリポジトリで Copilot と行うすべてのやり取りに適用されます。
4. `.github/instructions` フォルダーを開いてファイルを確認します。Astro ファイル、Drizzle データ レイヤー、テストなどの instruction があります。
5. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドは、instruction を適用するファイルを指定する glob です。
6. `.github/instructions/drizzle.instructions.md` を開き、ほかの instruction file や既存のプロジェクト ファイルへのリンクを確認します。これにより大きな instruction を再利用可能な小さな単位に分け、コード生成時に従うべき例を示せます。

## Instruction file をチームのガイダンスに合わせて更新する

既存の instruction file はよい出発点ですが、まだ不足があります。新しく生成する TypeScript に TSDoc comment を追加するよう、中核となる `copilot-instructions.md` を変更します。

1. `.github/copilot-instructions.md` で **Code formatting guidance** セクションを探します。35 行目付近にあります。
2. 最後の bullet として、次の行を追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

ファイルは自動的に保存されます。

## 更新したガイダンスを使う

Copilot CLI は会話開始時にリポジトリの instruction を読み込みます。保存後、フィルタリングの会話を終了して再開し、機能のコンテキストを保ったまま新しいガイダンスを読み込ませます。

1. フィルタリングの会話が開いたままの場合は `/exit` を入力して閉じます。Copilot CLI は会話を自動的に保存します。
2. `game-filters-cli` ブランチにある repository root から、保存済みの会話を再開します。

   ```bash
   copilot --resume
   ```

   選択肢が表示されたらフィルタリングの会話を選び、`game-filters-cli` ブランチが checkout されていることを確認します。
3. 生成済みのコードに更新したガイダンスを適用するよう Copilot に依頼します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. `/diff` を入力し、変更された TypeScript ファイルを確認します。新しい TSDoc comment が追加され、コードを正しく説明していることを確認します。

## まとめと次のステップ

Copilot CLI がプロジェクトの instruction からコンテキストを取得する方法を確認し、フィルタリング機能に新しい標準を適用しました。このレッスンでは、次のことを行いました。

- リポジトリ instruction とパス単位で適用される instruction file が agent にどう届くかを確認しました。
- コーディング標準に従うよう instruction file を更新しました。
- instruction file がコードに与える影響を確認しました。

次は、[再利用可能な quality-checks skill をカスタマイズして実行し][next-lesson]、lint とテストを一貫して実行します。

## リソース

- [Copilot CLI の custom instruction を追加する][instruction-files]
- [Custom instruction のサポート][custom-instructions-support]
- [Custom instruction 作成のベスト プラクティス][instructions-best-practices]
- [Awesome Copilot — instruction file などのリソース集][awesome-copilot]

[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
