---
title: "演習 1 - Custom instruction (cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot cloud agent →][next-lesson]

生成 AI を使うときはコンテキストが重要です。タスクの進め方に決まりがある場合や、Copilot に知っておいてほしい背景情報がある場合は、そのコンテキストを参照できるようにします。[Instruction file][instruction-files]を使うと、Copilot に何をしてほしいかだけでなく、どのように進めてほしいかも伝えられます。

この演習では、次のことを行います。

- リポジトリの custom instruction とパス単位で適用される instruction file を使って、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント標準が Copilot にどう伝わるかを確認します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用される新しい標準を追加します。

> [!NOTE]
> VS Code と CLI の harness とは異なり、ここでは変更前後のプロンプトを実行しません。Copilot cloud agent は GitHub Issue に対して非同期で作業するため、リアルタイムで前後を比較しにくいためです。後の演習で cloud agent が作成したプルリクエストをレビューするときに、instruction file の影響を確認します。

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

## リポジトリ全体に適用する標準を追加する

この演習で編集するのは、コード内ドキュメントとして TSDoc の doc comment とファイル レベルのコメント ヘッダーを必須にするプロジェクト共通のルールです。次の演習で Issue を割り当てると、cloud agent はこのルールを使います。

編集する前に、作業用ブランチを作成します。cloud agent は Issue の対象ブランチから instruction を読みますが、ここでは変更を整理して commit します。

1. codespace のターミナルで、新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

先ほど説明したように、`.github/copilot-instructions.md` は Copilot にプロジェクト レベルの情報を提供します。リポジトリのコーディング標準を記述し、提案の質を高めましょう。

1. `.github/copilot-instructions.md` をもう一度開きます。
2. **Code formatting requirements** セクションを探します。27 行目付近にあります。既存のコーディング標準は記載されていますが、コード内ドキュメントのルールはありません。そのため、生成された helper に doc comment がありませんでした。
3. 既存の標準の直後に、次の Markdown を追加します。これは Copilot にファイル コメント ヘッダーと TSDoc の doc comment を追加するよう指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで見たように、instruction file は全体的なガイダンス向けのリポジトリ レベル（`.github/copilot-instructions.md`）にも、特定の言語、ファイル種別、タスク向けの `*.instructions.md` としても作成できます。いま追加した doc comment ルールのようなプロジェクト共通の標準には、リポジトリ レベルのファイルが適しています。

## instruction の更新を commit、push、マージする

Cloud agent は Issue の対象ブランチから instruction file を読みます。次の演習で Issue を Copilot に割り当てると、Copilot は `main` からブランチを作成します。cloud agent が instruction の変更を使えるように、変更を `main` にマージします。

1. 変更を stage して commit します。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチを push します。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` に向けたプルリクエストを作成してマージします。次の演習で Issue を割り当てるとき、cloud agent はこの instruction を読み取ります。

> [!TIP]
> このワークショップで `main` に直接作業する場合は、ブランチ作成を省略して `main` に直接 commit できます。ブランチの手順は、実際のプロジェクトでの作業方法に合わせています。

## まとめと次のステップ

このプロジェクトで Copilot が instruction file からコンテキストを取得する方法を確認し、`.github/copilot-instructions.md` にリポジトリ共通の標準を追加しました。後続の harness で cloud agent が作成するプルリクエストをレビューするときに、この標準の適用を確認します。

次は [Copilot cloud agent に最初の Issue を割り当てます][next-lesson]。

## リソース

- [GitHub Copilot のカスタマイズ用 instruction file][instruction-files]
- [Custom instruction 作成のベスト プラクティス][instructions-best-practices]
- [Copilot 向けの custom instruction をより良く書くための 5 つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — instruction file などのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
