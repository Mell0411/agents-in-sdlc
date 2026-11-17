---
title: "演習 1 - カスタム指示 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Agent mode →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。タスクを特定の方法で実行する必要がある場合や、Copilot が知っておくべき背景情報がある場合は、そのコンテキストを参照できるようにします。Copilot と特定のコンテキストを共有する方法はいくつかあります。重要な方法の 1 つが[指示ファイル][instruction-files]です。指示ファイルはコード生成に関するガイダンスを提供します。

この演習では、次の内容を学びます。

- リポジトリのカスタム指示とパス別の指示ファイルを使って、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント標準を Copilot に伝える方法を確認します。
- 現在の指示に従ってフィルター機能の最初のデータスライス (発行元ヘルパー) を生成します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加します。
- 同じプロンプトを再送信し、生成されたコードが新しい標準に従うか確認します。
- 次の演習で Copilot が更新したガイダンスを使えるよう、指示の更新とフィルター機能の土台を `main` にコミットします。

> [!CAUTION]
> 生成されたコードが設定した標準から外れることがあります。Copilot は確率的に動作します。この演習では、出力が文字単位で一致するかではなく、指示の更新後に傾向が変わることを確認します。

## 指示ファイル

### シナリオ

Tailspin Toys には開発作業に関するガイドラインと要件があります。

- データレイヤーには必ずユニットテストを用意します。
- UI はダークモードで、現代的な見た目にします。
- コードのドキュメントには TSDoc のドキュメントコメントを追加します。
- 各ファイルの先頭に、ファイルの役割を説明するコメントブロックを追加します。

指示ファイルを使って Copilot に適切な情報を伝え、これらの開発慣行に沿ってタスクを実行できるようにします。

### カスタム指示

カスタム指示を使うと、コーディングスタイルや要件を Copilot に伝え、より適切な提案やコードスニペットを生成できるようにします。コーディング規約、ライブラリ、コメントの書き方などを指定できます。リポジトリ全体に適用する指示も、特定のファイルやタスクに適用する指示も作成できます。

指示ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリに対する**すべての**要求とともに Copilot に送られる単一の指示ファイルです。技術スタック、開発対象の概要、ベストプラクティスなど、多くのやり取りに共通する情報を記載します。
- `.github/instructions/*.instructions.md` は、特定の言語、ファイル形式、タスクに関する指針を記載します。

> [!NOTE]
> IDE では、指示ファイルは Copilot Chat でのコード生成にのみ使われ、コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、リポジトリレベルの指示と、`applyTo` frontmatter を持つ `*.instructions.md` ファイルの両方を使います。
>
> Copilot は AGENTS.md や CLAUDE.md など、[ほかの形式の指示ファイル][custom-instructions-support]にも対応しています。

### 指示ファイルを管理するためのベストプラクティス

この演習では指示ファイルの作成方法を詳しく扱いませんが、サンプルプロジェクトの例から基本的な方針を確認できます。

- `copilot-instructions.md` には、構築中のもの、プロジェクトの構造、全体的なコーディング標準など、プロジェクト全体の指針を記載します。
- `*.instructions.md` には、ユニットテスト、Astro、データレイヤーなどのファイル形式や特定のタスクに関する指針を記載します。
- 自然な言葉で、明確なガイダンスと例を示します。

指示ファイルの作成方法に決まった形はありません。AI の使い方と同様に、試しながらプロジェクトに合う方法を見つけます。

> [!TIP]
> GitHub Copilot を使うプロジェクトには、十分な指示ファイルを用意することをお勧めします。このプロジェクトには、[UI 更新][ui-instructions]や [Astro][astro-instructions]など、さまざまなタスク向けのファイルがあります。
>
> Copilot に指示ファイルを作成させることもできます。環境ごとに方法は異なります (例: VS Code の **Configure Chat → Generate Agent Instructions**、Copilot CLI の `/init`)。詳細は該当する環境のレッスンで説明します。
>
> テンプレートやひな形を探している場合は、指示ファイル、カスタムエージェント、その他のリソースをまとめた [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム指示ファイルを確認する

スタータープロジェクトには、中心となる `copilot-instructions.md` と、さまざまなタスク向けの `*.instructions.md` ファイルがあります。変更の影響を確認するため、先に既存の内容を調べます。

1. Codespace に戻ります。
2. Codespaces エディターで (ターミナルではなく) `.github/copilot-instructions.md` を開きます。
3. プロジェクトの簡単な説明と指針を確認します。これらの指示は、リポジトリで Copilot と行うすべてのやり取りに適用されます。
4. `.github/instructions` フォルダーを開いて内容を確認します。Astro ファイル、Drizzle データレイヤー、テストなどの指示があります。
5. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドが、適用対象を決める glob です。
6. `.github/instructions/drizzle.instructions.md` を開き、他の指示ファイルやプロジェクト内ファイルへのリンクを確認します。指示を小さく再利用可能なファイルに分け、Copilot が参考にできる例を示せます。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング標準が記載されていますが、コード内のドキュメントはまだ必須ではありません。次の手順で TSDoc のドキュメントコメントとファイルヘッダーのルールを追加します。

## 変更用ブランチを作成する

ここではベストプラクティスに従って、変更用のブランチを作成します。

1. 前の演習で使った codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して新しいターミナルを開きます。
3. 新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## 指示を更新する**前**に Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 次の演習を始める前に codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を開始します。モードとモデルの選択は演習によって異なります。必要な場合は各手順に記載しています。

カスタム指示の効果を確認するため、まず現在の指示ファイルを使ったプロンプトを送信します。その後、指示を更新して同じプロンプトを再送信し、結果の違いを見ます。

1. 前の演習で使ったエディタータブを閉じ、Copilot が必要なコンテキストだけを使うようにします。
2. Copilot にヘルパーを配置する場所を伝えるため、`src/lib/publishers.ts` を開きます。
3. Chat ビューのエージェントドロップダウンから **Agent** を選択し、Copilot がファイルを変更できるようにします。

   ![Chat ビューのエージェントピッカーのスクリーンショット](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot はプロジェクトを調べてコードを更新します。多くの場合、ヘルパーファイルとテストの両方が変更されます。
6. 提案されたヘルパーが `db` クライアントを最初の引数として受け取り、型付きの発行元配列を返す関数であることを確認します。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` のデータレイヤー規約に基づいています。
7. 提案されたコードに TSDoc のドキュメントコメントとファイルレベルのコメントヘッダーが**ない**ことを確認します。

> [!CAUTION]
> Copilot は確率的に動作するため、指示がなくてもドキュメントコメントを追加する可能性があります。その場合も問題ありません。指示を更新した後の一貫性の改善が、この演習の目的です。

## リポジトリ標準を追加する

前述のとおり、`.github/copilot-instructions.md` は Copilot にプロジェクトレベルの情報を提供します。コード提案を改善するため、リポジトリのコーディング標準を記述します。

1. `.github/copilot-instructions.md` を再度開きます。
2. **Code formatting requirements** セクションを見つけます (27 行目付近)。コーディング標準は記載されていますが、コード内ドキュメントのルールがありません。そのため、生成されたヘルパーにはドキュメントコメントがありませんでした。
3. 既存の標準の直後に、次の Markdown 行を追加し、ファイルコメントヘッダーと TSDoc コメントを指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで学んだように、指示ファイルはリポジトリレベル (`.github/copilot-instructions.md`) に作成して全体に適用することも、`*.instructions.md` として言語、ファイル形式、タスクに適用することもできます。ドキュメントコメントのルールのようなプロジェクト全体の標準には、リポジトリレベルのファイルが適しています。

## プロンプトを再送信して違いを確認する

1. Copilot Chat に戻り、**New Chat** を選択して会話を新しくします。
2. `src/lib/publishers.ts` を再び開き、Copilot が適切なファイルに注目できるようにします。
3. 前と**同じプロンプト**を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 提案されたファイルの先頭に、次のようなコメントブロックが追加されたことを確認します。

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 提案された関数に、次のような TSDoc コメントが含まれていることを確認します。

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

新しいプロジェクト標準に従うよう Copilot をガイドし、次の演習で使う実際のコードに適用しました。

## 指示をコミットしてブランチをプッシュする

指示ファイルも他のリポジトリアセットと同じようにソース管理します。変更をコミットしてブランチをリポジトリにプッシュしましょう。

1. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して新しいターミナルウィンドウを開きます。
2. ターミナルで次のコマンドを実行し、指示とヘルパーの変更を確認します。

   ```bash
   git status
   ```

3. ターミナルで指示の更新と、ヘルパーの基礎として Copilot が変更したすべてのファイルをステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # git status にテストなどの追加変更が表示された場合は、それらのファイルも追加します。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. ターミナルからブランチをリポジトリにプッシュします。

   ```bash
   git push -u origin custom-instructions
   ```

## pull request を作成してマージする

ブランチをプッシュしたら、バックログにあるドキュメント標準の Issue と pull request を関連付けます。手動で行う代わりに、すでにプロジェクトに接続されている GitHub ツールを使って Copilot に依頼します。Issue を検索し、Issue をクローズする PR を作成してから、マージを依頼します。

> [!NOTE]
> この演習で初めて Copilot に GitHub 上の操作を依頼します。プロジェクトのテンプレートでは GitHub ツールがすでにワークスペースに接続されています。Copilot が初めてツールを使うとき、VS Code で GitHub へのサインインを求められることがあります。表示された指示に従って許可します。後の演習で、この接続の仕組み (Model Context Protocol) を学びます。

1. codespace で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、エージェントピッカーで **Agent** が選択されていることを確認します。
3. 指示ファイルの更新に関する Issue を探し、現在のブランチから PR を作成するよう Copilot に依頼します。指示の更新と、これからのフィルター作業に向けた発行元ヘルパーの土台の両方を説明に含めます。

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot が Issue の検索と PR の作成を始めます。
5. Copilot が GitHub 操作の許可を求めたら、実行する内容を確認し、必要に応じて **Allow** を選択します。
6. PR が作成されたら、PR をマージしてローカルのブランチを `main` に戻すよう、次のプロンプトで依頼します。

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Copilot が GitHub 操作やシェルコマンドの実行許可を求めたら、内容を確認して必要に応じて **Allow** を選択します。

GitHub Copilot を使って pull request を作成し、マージしました。

## まとめと次のステップ

Copilot が指示ファイルを読み込む方法を確認し、VS Code の Copilot Chat で次の作業を行いました。

- 既存の指示を使ってコード生成プロンプトを送り、Copilot の出力を確認しました。
- `.github/copilot-instructions.md` に新しい標準を追加しました。
- 同じプロンプトを再送信し、生成されたコードが新しい標準に従うことを確認しました。
- 指示の更新とヘルパーの土台をコミット、マージし、次の演習で使えるよう `main` に反映しました。

次は [agent mode][next-lesson] で指示を使い、Copilot にコードベース全体で新機能を追加させます。

## リソース

- [GitHub Copilot のカスタマイズに使う指示ファイル][instruction-files]
- [カスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Copilot のカスタム指示を改善する 5 つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用カスタム指示][personal-instructions]
- [awesome-copilot — 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
[awesome-copilot]: https://github.com/github/awesome-copilot
