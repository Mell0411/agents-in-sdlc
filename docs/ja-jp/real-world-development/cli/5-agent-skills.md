---
title: "レッスン 5 - quality-checks スキルをカスタマイズして使う"
description: "既存の quality-checks スキルを確認し、レポート形式をカスタマイズして、フィルター機能を検証します。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

コードを書くこと以外にも、コードを検証する必要があります。手動で動作を確認し、指示ファイルでコーディング標準に従わせました。テスト、lint、継続的インテグレーション (CI) の他の作業についてはどうでしょうか。

このようなタスクには、**エージェントスキル**が適しています。スキルは、こうした操作を正しく実行する方法を Copilot に伝えます。

このレッスンでは、次の内容を学びます。

- 既存の `quality-checks` スキルを確認します。
- 結果の形式をカスタマイズします。
- スキルを再読み込みして実行します。

## シナリオ

Tailspin Toys は、ユニットテスト、lint、型チェックに `quality-checks` スキルを使っています。チームは、結果を読みやすくするため、レポートを改善したいと考えています。

## 指示、スクリプト、リソース

エージェントスキルは、再利用可能なタスクの指示、実行可能なスクリプト、補助リソースをまとめ、必要なときにエージェントが読み込みます。スキルは、スキル名が付いたフォルダーと、その中にある `SKILL.md` で構成されます。Markdown ファイルには、スキルの内容を定義する `name` と `description` の frontmatter、概要、呼び出すべき状況についての説明があります。スキルのフォルダーには、呼び出し時に使用するスクリプトやその他のリソースを含めることもできます。

> [!NOTE]
> スキルに追加のファイルやフォルダーは必須ではありません。Tailspin Toys の `quality-checks` スキルは、プロジェクトにあるコマンドを使うため、`SKILL.md` のみで構成されています。

スキルをプロジェクトの `.github/skills` に配置すると、チームで共有して再利用できます。ユーザー用の `~/.copilot/skills` に配置することもできます。

## スキルを確認する

Tailspin Toys チームがユニットテスト、lint、型チェックを実行するために作成した `quality-checks` スキルを確認します。

1. Codespace に戻り、Codespaces エディターで `.github/skills/quality-checks/SKILL.md` を開きます。
2. 先頭の `name` と `description` を読みます。description によって、Copilot はスキルを呼び出すタイミングを判断します。
3. 指示を読み、テストと lint の手順を Copilot に伝える方法を確認します。
4. まだ **Results output formatting** セクションがないことを確認します。

## 変更前にスキルを実行する

スキルは Copilot CLI から直接呼び出すことも、自然言語を使って呼び出すこともできます。3 つのチェックを実行するよう Copilot に依頼します。

1. Interactive モードでフィルター機能の会話に戻ります。
2. 次のプロンプトを使います。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 最後に出力されるレポートを確認します。

## レポートをカスタマイズする

実行したチェック、成功したかどうか、ツールが実際に報告した内容がわかる、より良いレポートを作成しましょう。スキルを更新してその形式を指定します。

1. `.github/skills/quality-checks/SKILL.md` に戻ります。
2. ファイルの末尾に次のセクションを追加します。

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. ファイルは自動的に保存されます。

## 更新したスキルを実行する

Copilot CLI は会話を再起動せずに、編集したスキルを再読み込みできます。更新したスキルを使ってみましょう。

1. 次のコマンドを入力します。

   ```plaintext
   /skills reload
   ```

2. 前とまったく同じプロンプトを使います。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. レポートを読み、最初の結果と比較します。

## まとめと次のステップ

既存のエージェントスキルをカスタマイズして使用しました。

- ユニットテスト、lint、型チェックを実行する `quality-checks` スキルを確認しました。
- 結果の形式をカスタマイズしました。
- スキルを再読み込みして実行しました。

更新したスキルは、フィルター機能の PR に含めます。次は [Playwright MCP サーバー][next-lesson]を通じて、Copilot がサイトを直接操作できるようにします。

## スキルの例

コミュニティの次の例は参考資料であり、追加のタスクではありません。

- [Agent Skills 仕様][skill-spec]
- [コントリビューションのワークフロー: `make-repo-contribution`][contribution-example]
- [要件ドキュメント: `prd`][prd-example]
- [図とバンドルされたエクスポートスクリプト: `drawio`][drawio-example]
- [ブラウザーテスト: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
