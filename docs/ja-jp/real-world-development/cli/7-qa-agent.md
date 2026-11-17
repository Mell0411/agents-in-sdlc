---
title: "レッスン 7 - QA エージェントを作成して使用する"
description: "Issue の要件、quality-checks スキル、Playwright MCP を組み合わせる QA カスタムエージェントを作成します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

`quality-checks` スキルを使って自動チェックを実行し、Playwright MCP でブラウザー上のフィルター機能を確認しました。次に、明確な QA プロセスを持つカスタムエージェントにこれらの機能をまとめます。

このレッスンでは、次の内容を学びます。

- カスタムエージェントが指示、スキル、MCP ツールと連携する方法を確認します。
- 再利用可能な品質保証プロファイルを作成して確認します。
- QA エージェントを選択し、フィルター機能の Issue に照らして報告をレビューします。

## シナリオ

Tailspin Toys は、pull request (PR) を作成する前に、要件、コード品質、自動チェック、テストカバレッジ、ブラウザーでの動作を一貫して確認したいと考えています。カスタムエージェントを使うと QA プロセスを調整し、再利用可能なレポートを作成できます。

## カスタムエージェントとは

カスタムエージェントは、Markdown プロファイルで定義する Copilot の専門的な役割です。プロファイルには、エージェントの目的、指示、利用可能なツールを記述します。このワークショップでは `.github/agents/qa.agent.md` に QA の役割を定義し、Copilot CLI で選択します。

これまでに使用したカスタマイズには、それぞれ異なる役割があります。リポジトリの指示はチームの標準を説明し、`quality-checks` スキルは繰り返し実行できるチェックをまとめます。Playwright MCP はブラウザーツールを提供します。QA プロファイルは、Copilot がそれらの機能を使って要件を評価し、結果を報告する方法を定義します。これらの機能は互いに置き換わるものではなく、別の会話も必要ありません。

## QA プロファイルを作成する

機能の PR を開く前に、Copilot に再利用可能な QA プロファイルの作成を依頼します。プロファイルでは、QA が実行するチェックと守るべき境界の両方を定義します。

1. Codespace に戻り、フィルター機能の会話が Interactive モードであることを確認します。
2. 既定のエージェントに、新しいカスタムエージェントの作成を依頼します。

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## プロファイルを確認する

新しいエージェントを使う前に、意図した QA のワークフローと境界がプロファイルに記載されていることを確認します。これにより、検証だけを行いたいときにエージェントが不完全な作業をしたり、範囲を超えてコードを変更したりすることを防ぎます。

1. `/diff` を入力し、`.github/agents/qa.agent.md` を開きます。
2. frontmatter を確認します。`description` は必須です。`name` は省略できますが、含めるとエージェントの表示名が明確になります。
3. プロファイルの指示を読み、QA が要件から開始し、リポジトリの指示に従い、`quality-checks` スキルと Playwright MCP を使うことを確認します。
4. QA が根拠を示し、実装コードを変更する前に確認し、変更をコミットせず pull request も開かないことを確認します。
5. これらの責務や境界が不足している場合は、続行する前に既定のエージェントに修正を依頼します。

## Issue に対して QA を実行する

Copilot CLI は会話の開始時にプロジェクトのエージェントを読み込みます。プロファイルを作成した後、同じフィルター機能の会話を再開し、QA を選択します。Issue と計画の内容をコンテキストとして使えます。

1. 次のプロンプトを入力してエージェントを選択します。

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> 作成したばかりのエージェントは、利用可能なエージェントの一覧にまだ表示されない場合があります。上記のコマンドで有効にできます。

2. 次のプロンプトで QA エージェントに機能をレビューするよう依頼します。

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. QA エージェントが作業を開始します。
4. 作業が完了したら、報告を確認します。

## まとめと次のステップ

ワークフローに再利用可能な専門的役割を追加し、その成果をレビューしました。

- カスタムエージェントと指示、スキル、MCP ツールの連携方法を確認しました。
- 再利用可能な品質保証プロファイルを作成して確認しました。
- QA エージェントを選択し、フィルター機能の Issue に照らして報告をレビューしました。

実装、スキルの更新、QA プロファイル、テスト、検証レポートが揃いました。次はこれらをまとめて機能の PR を作成し、Agent Merge を使います。

## リソース

- [Copilot CLI 用のカスタムエージェントを作成する][create-agents]
- [カスタムエージェントの設定][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
