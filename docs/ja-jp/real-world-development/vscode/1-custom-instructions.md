---
title: "演習 1 - Custom instruction (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Agent mode →][next-lesson]

生成 AI を使うときはコンテキストが重要です。タスクの進め方に決まりがある場合や、Copilot に知っておいてほしい背景情報がある場合は、そのコンテキストを参照できるようにします。Copilot と特定のコンテキストを共有する方法はいくつかあります。その中心となるのが [instruction file][instruction-files] です。instruction file は、コード生成のガイダンスを提供します。

この演習では、次のことを行います。

- リポジトリの custom instruction とパス単位で適用される instruction file を使って、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント標準が Copilot にどう伝わるかを確認します。
- *現在の* instruction を使って、フィルタリング用の最初のデータ スライス（publishers helper）を生成します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用される新しい標準を追加します。
- 同じ prompt を再実行し、生成コードが新しい標準を取り入れる様子を確認します。
- instruction の更新とフィルタリングのデータ スライスを `main` に commit し、次の演習で Copilot が新しいガイダンスを使えるようにします。

> [!CAUTION]
> 生成されたコードは、設定した標準から外れることがあります。Copilot は非決定的です。この演習の目的は、instruction の更新後に振る舞いがどう変化するかを見ることであり、出力を 1 文字単位で一致させることではありません。

## Instruction file

### シナリオ

Tailspin Toys には、開発プラクティスに関するガイドラインと要件があります。たとえば次のとおりです。

- データ レイヤーには常に unit test が必要です。
- UI はダーク モードで、モダンな印象にする必要があります。
- ドキュメントは TSDoc の doc comment としてコード内に追加する必要があります。
- 各ファイルの先頭には、そのファイルの役割を説明するコメント ブロックを追加する必要があります。

Instruction file を使って、これらのプラクティスに沿ってタスクを進めるために必要な情報を Copilot に渡します。

### Custom instruction

Custom instruction を使うと、コンテキストや設定を Copilot に渡し、コーディング スタイルや要件をより正確に理解させられます。コーディング規約、使用するライブラリ、含めたいコメントの種類などを指定できます。instruction はリポジトリ全体に適用することも、特定のファイル種別に対するタスク単位のコンテキストとして作成することもできます。

Instruction file には 2 種類あります。

- `.github/copilot-instructions.md` は、リポジトリに対する**すべて**のリクエストで Copilot に送られる単一の instruction file です。使用している技術スタック、開発中のものの概要、ベスト プラクティス、その他の共通ガイダンスなど、プロジェクト レベルの情報を記述します。
- `.github/instructions/*.instructions.md` は、特定のタスクやファイル種別に適用できます。TypeScript や Astro などの言語、UI component や unit test の作成などに関するガイドラインを記述できます。

> [!NOTE]
> IDE で作業する場合、instruction file は Copilot Chat でのコード生成に使われます。コード補完や next-edit suggestion には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、コード生成時にリポジトリ レベルの instruction file と `applyTo` frontmatter を持つ `*.instructions.md` ファイルの両方を使います。
>
> Copilot は [ほかの標準に基づく instruction file][custom-instructions-support]もサポートしています。AGENTS.md や CLAUDE.md も使用できます。

### Instruction file を管理するためのベスト プラクティス

Instruction file の作成について詳しく扱うことは、このワークショップの範囲外です。サンプル プロジェクトの例は、代表的な方法を示しています。

- `copilot-instructions.md` には、開発中のプロジェクト、プロジェクト構造、全体的なコーディング標準など、プロジェクト レベルのガイダンスを記述します。
- `*.instructions.md` ファイルには、ファイル種別（unit test、Astro component、データ レイヤー）や特定タスクに関する具体的な指示を記述します。
- 自然な言葉で明確に書きます。コードの望ましい例と望ましくない例の両方を示します。

Instruction file の作り方にも、AI の使い方にも唯一の正解はありません。試行錯誤しながら、プロジェクトに適した方法を見つけてください。

> [!TIP]
> GitHub Copilot を使うプロジェクトには、十分な instruction file を用意すると役立ちます。このプロジェクトの instruction file には、[UI の更新][ui-instructions]や [Astro][astro-instructions]など、さまざまなタスク向けのものがあります。
>
> Copilot に instruction file の作成を依頼することもできます。VS Code の **Configure Chat → Generate Agent Instructions** や Copilot CLI の `/init` など、各 surface で方法が異なります。必要な場面では、それぞれの surface のレッスンで説明します。
>
> テンプレートや出発点を探す場合は、instruction file、custom agent、その他のリソースを集めた [awesome-copilot][awesome-copilot]を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトの custom instruction file を確認する

このリポジトリには、中核となる `copilot-instructions.md` と、さまざまなタスク向けの `*.instructions.md` ファイルがあります。エディターまたは GitHub の Web UI で開いて確認します。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの概要と、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** 内の **GitHub Actions Workflows** に関するガイダンスにも注目してください。これらは Copilot とのすべてのやり取りに適用されます。
3. `.github/instructions` フォルダーを開き、Astro ファイル、Drizzle データ レイヤー、テストなどに関する instruction を確認します。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドは、instruction を適用するファイルを決める glob（repo root からの相対パス）です。たとえば `**/*.test.ts` に一致する TypeScript test ファイルが対象です。
5. このプロジェクトの unit test 作成に関する instruction を確認します。
6. 最後に `.github/instructions/drizzle.instructions.md` を開き、末尾まで確認します。`unit-tests.instructions.md` などの instruction file や、プロジェクトの既存ファイルへのリンクがあります。これにより、大きな instruction を再利用しやすい小さな単位に分割し、コード生成時に従うべき例を示せます。（記載されたパスは repo root ではなく、instruction file からの相対パスです。）

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションには、このプロジェクトのコーディング標準がありますが、コード内ドキュメントの要件はまだありません。次の手順で TSDoc の doc comment とファイル コメント ヘッダーのルールを追加します。

## 変更用のブランチを作成する

ここではベスト プラクティスに従って、変更用のブランチを作成します。

1. 前の演習で使った codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. 新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## instruction を更新する*前に* Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 次の演習を始める前に codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な手順は各演習で説明します。

Custom instruction の効果を確認するため、まず現在の instruction を使って prompt を送信します。その後で instruction を更新し、同じ prompt を再送信して違いを確認します。

1. 前の演習で開いたエディター タブを閉じ、Copilot が必要なコンテキストだけを取得できるようにします。
2. helper を配置する場所を Copilot に知らせるため、`src/lib/publishers.ts` を開きます。
3. Chat ビューの agent dropdown で **Agent** を選択し、Copilot がファイルを変更できるようにします。

   ![Chat ビューで agent picker を表示した画面](../../../_images/shared-chat-mode-selector.png)

4. 次の prompt を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot がプロジェクトを調査してコードを更新します。helper ファイルとテストの両方に変更が加わる場合があります。
6. 提案された helper が型付き関数であり、第一引数として `db` client を受け取り、publisher の型付き配列を返すことを確認します。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` のデータ レイヤー規約によるものです。
7. 提案コードに TSDoc の doc comment とファイル レベルのコメント ヘッダーが**含まれていない**ことを確認します。

> [!CAUTION]
> Copilot は確率的に動作するため、指示がなくても doc comment を追加する場合があります。その場合も問題ありません。instruction を更新した後に一貫性が向上することが重要です。

## リポジトリ全体に適用する標準を追加する

先ほど説明したように、`.github/copilot-instructions.md` は Copilot にプロジェクト レベルの情報を提供するファイルです。リポジトリのコーディング標準を記述して、コード提案を改善します。

1. `.github/copilot-instructions.md` をもう一度開きます。
2. **Code formatting requirements** セクションを探します。27 行目付近にあります。コーディング標準は記載されていますが、コード内ドキュメントのルールはまだありません。そのため、生成した helper に doc comment がありませんでした。
3. 既存の標準の直後に、次の Markdown 行を追加します。ファイル コメント ヘッダーと TSDoc の doc comment を Copilot に追加させます。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで見たように、instruction file は全体的なガイダンス向けのリポジトリ レベル（`.github/copilot-instructions.md`）にも、特定の言語、ファイル種別、タスク向けの `*.instructions.md` としても作成できます。いま追加した doc comment ルールのようなプロジェクト共通の標準には、リポジトリ レベルのファイルが適しています。

## prompt を再実行して変化を確認する

1. Copilot Chat に戻り、**New Chat** を選択して会話を新しくします。
2. `src/lib/publishers.ts` を選択し、Copilot が対象ファイルに注目するようにします。
3. 先ほどと**同じ prompt** を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 提案されたファイルの先頭に、次のようなコメント ブロックがあることを確認します。

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 提案された関数に、次のような TSDoc comment があることを確認します。

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

Copilot に新しいプロジェクト標準に従うよう指示し、次の演習で使う実際のコードに適用できました。

## instruction を commit してブランチを push する

Instruction file も、ほかのファイルと同じようにソース管理で扱います。変更を commit して、リポジトリにブランチを push します。

1. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
2. 次のコマンドを実行して、instruction と helper の変更があることを確認します。

   ```bash
   git status
   ```

3. ターミナルで instruction の更新と helper の基礎に関する変更を stage して commit します。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # git status にテストなどの追加変更が表示された場合は、それらのファイルも追加します。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. ブランチをリポジトリに push します。

   ```bash
   git push -u origin custom-instructions
   ```

## プルリクエストを作成してマージする

ブランチを push したので、バックログにあるドキュメント標準の Issue に関連付けたプルリクエストを作成します。手動でも作成できますが、このプロジェクトに接続済みの GitHub ツールを使って Copilot に依頼できます。Issue を探し、関連付けた PR を作成してマージするよう prompt を送ります。

> [!NOTE]
> この演習では、Copilot に初めて GitHub 上の操作を依頼します。プロジェクト テンプレートには GitHub のツールが設定済みなので、追加のセットアップは不要です。Copilot が初めてツールを使う際、VS Code で GitHub へのサインインを求められる場合があります。画面の指示に従って許可してください。後の演習で、この接続に使われる Model Context Protocol について学びます。

1. codespace 内で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、agent picker で **Agent** が選択されていることを確認します。
3. instruction file の更新に関連する Issue を探し、現在のブランチから PR を作成するよう Copilot に依頼します。PR には instruction の更新と、今後のフィルタリング作業向け publishers helper の基礎の両方を記載します。

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot が Issue の検索と PR 作成を始めます。
5. Copilot が代わりに GitHub の操作を行うための **Allow** を求めた場合は、コマンドを確認し、許可する場合は **Allow** を選択します。
6. PR が作成されたら、次の prompt でマージを依頼し、ローカル ブランチを `main` に戻して最新のコードを取得します。

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Copilot が代わりに GitHub 操作や shell command を実行するための **Allow** を求めた場合は、操作を確認して許可します。

これで GitHub Copilot の支援を受けてプルリクエストを作成し、マージできました。

## まとめと次のステップ

このプロジェクトで Copilot が instruction file からコンテキストを取得する方法を確認し、VS Code の Copilot Chat で次の作業を行いました。

- *既存の* instruction を使ってコード生成 prompt を送り、Copilot の出力を確認しました。
- `.github/copilot-instructions.md` にリポジトリ全体の標準を追加しました。
- 同じ prompt を再実行し、提案コードが新しい標準を取り入れる様子を確認しました。
- instruction の更新と helper の基礎を `main` に commit し、次の演習に備えました。

次は、[agent mode][next-lesson]で Copilot にコードベース全体にわたる機能を追加してもらい、instruction を活用します。

## リソース

- [GitHub Copilot のカスタマイズ用 instruction file][instruction-files]
- [Custom instruction 作成のベスト プラクティス][instructions-best-practices]
- [Copilot 向けの custom instruction をより良く書くための 5 つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人向け custom instruction][personal-instructions]
- [Awesome Copilot — instruction file などのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
[awesome-copilot]: https://github.com/github/awesome-copilot
