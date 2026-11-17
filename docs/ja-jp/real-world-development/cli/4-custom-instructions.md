---
title: "レッスン 4 - カスタム指示で Copilot をガイドする"
description: "リポジトリの指示を確認し、ドキュメント標準を追加して、フィルター機能のコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を使うときは、コンテキストが重要です。タスクを特定の方法で実行する必要がある場合、そのガイダンスを Copilot が利用できるようにします。[指示ファイル][instruction-files]には、求めるコードだけでなく、その構成方法も記述できます。フィルター機能を作成したので、Copilot が参照した指示を確認し、ドキュメント標準を追加してコードに適用します。

このレッスンでは、次の内容を学びます。

- リポジトリの指示とパス別の指示ファイルがエージェントに適用される方法を確認します。
- コーディング標準を適用するため、指示ファイルを更新します。
- 指示ファイルがコードに与える影響を確認します。

## シナリオ

Tailspin Toys には開発作業に関するガイドラインと要件があります。

- コメントにはコードの内容を言い換えるのではなく、意図や自明ではない判断を記述します。
- `db/` と `src/lib/` のエクスポート関数には、目的、パラメーター、戻り値を説明する TSDoc/JSDoc を記述します。該当する場合は、注入可能な `db` 引数も含めます。
- 再利用可能な Astro コンポーネントには `Props` コントラクトの説明を記述し、関連するコードを更新した場合はコメントも最新に保ちます。
- 既存の書式設定と lint の指針を維持します。

指示ファイルを使って Copilot に適切な情報を伝え、これらの開発慣行に沿ってタスクを実行できるようにします。

## 指示ファイル

カスタム指示を使うと、コーディングスタイルや要件を Copilot に伝え、より適切な提案やコードスニペットを生成できるようにします。コーディング規約、ライブラリ、コメントの書き方などを指定できます。リポジトリ全体に適用する指示も、特定のファイルやタスクに適用する指示も作成できます。

指示ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリに対する**すべての**要求とともに Copilot に送られる単一の指示ファイルです。多くの要求に関係するプロジェクト情報を記載します。
- `.github/instructions/*.instructions.md` は、特定の言語、ファイル形式、タスクに関する指針を記載します。

> [!NOTE]
> ほかの形式の指示と対応状況は環境によって異なります。特定の形式に依存する前に、[カスタム指示の対応状況][custom-instructions-support]を確認してください。

## このプロジェクトのカスタム指示ファイルを確認する

変更前の状態を確認するため、スタータープロジェクトに含まれる指示ファイルを調べます。

1. Codespace に戻ります。
2. Codespaces エディターで (ターミナルではなく) `.github/copilot-instructions.md` を開きます。
3. プロジェクトの概要とコーディングガイダンスを確認します。この指示は、リポジトリ内で Copilot と行うすべてのやり取りに適用されます。
4. `.github/instructions` フォルダーを開いて内容を確認します。Astro ファイル、Drizzle データレイヤー、テストなどの指示があります。
5. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドが、適用対象を決める glob です。
6. `.github/instructions/drizzle.instructions.md` を開き、他の指示ファイルやプロジェクト内のファイルへのリンクを確認します。これにより、指示を小さく再利用可能なファイルに分け、Copilot が参考にできる例を示せます。

## チームの方針に合わせて指示ファイルを更新する

指示ファイルはすでに用意されていますが、TypeScript に TSDoc コメントを追加する標準が不足しています。中心となる `copilot-instructions.md` を変更します。

1. `.github/copilot-instructions.md` の 35 行目付近にある **Code formatting guidance** セクションを見つけます。
2. 最後の箇条書きとして、次の内容を追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

変更は自動的に保存されます。

## 更新したガイダンスを使う

Copilot CLI は会話の開始時にリポジトリの指示を読み込みます。フィルター機能のコンテキストを保持しながら新しい指示を読み込むため、会話を終了して再開します。

1. フィルター機能の会話が開いている場合は、`/exit` を入力して終了します。Copilot CLI は会話を自動的に保存します。
2. リポジトリルートの `game-filters-cli` ブランチから、保存した会話を再開します。

   ```bash
   copilot --resume
   ```

   プロンプトが表示された場合は、フィルター機能の会話を選び、`game-filters-cli` ブランチをチェックアウトしていることを確認します。
3. 更新した指示を適用するよう Copilot に依頼します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. `/diff` を入力し、変更された TypeScript ファイルを確認します。新しく追加された TSDoc コメントがコードを正確に説明していることを確認します。

## まとめと次のステップ

Copilot CLI が指示ファイルを読み込む方法を確認し、機能に新しい標準を適用しました。

- リポジトリの指示とパス別の指示ファイルがエージェントに適用される方法を確認しました。
- コーディング標準を適用するため、指示ファイルを更新しました。
- 指示ファイルがコードに与える影響を確認しました。

次は、再利用可能な `quality-checks` スキルを[カスタマイズして実行][next-lesson]し、lint とテストが一貫して実行されるようにします。

## リソース

- [GitHub Copilot CLI にカスタム指示を追加する][instruction-files]
- [カスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Copilot のカスタム指示を改善する 5 つのヒント][copilot-instructions-five-tips]
- [awesome-copilot — 指示ファイルなどのリソース集][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
