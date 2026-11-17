---
title: "レッスン 6 - Playwright MCP で機能を検証する"
description: "Copilot CLI で Playwright MCP を確認または設定し、ブラウザーでフィルター機能を試します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

コードを書くこと以外にも、データや外部サービスを使ったり、Copilot が自動化に利用できるツールを追加したりする必要があります。MCP サーバーを使うと、Copilot CLI に組み込まれていないツールやサービスを追加できます。

このレッスンでは、次の内容を学びます。

- Model Context Protocol (MCP) とは何か、Copilot CLI がどのように使うかを理解します。
- まだ利用できない場合は、Playwright MCP サーバーを追加します。
- エージェントにブラウザーを操作させ、フィルター機能を確認します。

## シナリオ

ユニットテストやエンドツーエンドテストは重要ですが、UI の更新を検証するには実際に操作する必要があります。Copilot がユーザーと同じように Web サイトを使えるようにし、変更の実行結果に自信を持てるようにします。

## Model Context Protocol (MCP) とは

[Model Context Protocol (MCP)][mcp-blog-post] は、AI エージェントが外部のツールやサービスと通信するための仕組みです。MCP を使うことで、AI エージェントは外部ツールやサービスにリアルタイムでアクセスできます。最新情報を取得したり、ユーザーに代わって操作を実行したりできます。

ツールやリソースには MCP サーバーを介してアクセスします。MCP サーバーは AI エージェントと外部ツール・サービスの橋渡しをします。各 MCP サーバーは、エージェントが利用できるツールとリソースのセットを提供します。

よく使われる MCP サーバーをいくつか紹介します。

- **[GitHub MCP Server][github-mcp]**: GitHub リポジトリ、Issue、pull request を管理する API にアクセスできます。
- **[Playwright MCP Server][playwright-mcp-server]**: Playwright を使ったブラウザーの自動操作機能を提供します。

ほかにも多くの MCP サーバーがあります。GitHub は、エコシステム内のサーバーを見つけたり貢献したりしやすくするため、[MCP レジストリ][mcp-registry]をホストしています。

> [!CAUTION]
> MCP サーバーはプロジェクトのほかの依存関係と同様に扱ってください。使用する前にソースコードを確認し、発行元を検証して、セキュリティへの影響を考慮します。

## Playwright MCP サーバーを追加する

Playwright MCP サーバーがまだ利用できない場合は、Copilot がユーザーのように Web サイトを操作できるように追加します。

> [!NOTE]
> Copilot CLI に Playwright MCP がすでに設定されている場合は、セットアップ手順を省略して次のセクションに進みます。

1. Codespace に戻ります。
2. Copilot CLI で次のコマンドを入力し、MCP サーバー追加ダイアログを開きます。

   ```plaintext
   /mcp add
   ```

3. 名前として `playwright` を入力し、<kbd>Tab</kbd> を押します。
4. <kbd>Enter</kbd> を押してサーバーの種類を **STDIO** にし、続けて <kbd>Tab</kbd> を押します。
5. **Command** ダイアログに次の内容を貼り付けます。

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. <kbd>Control</kbd>+<kbd>S</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) を押して MCP サーバーを保存します。
7. <kbd>Esc</kbd> を押して MCP ダイアログを閉じます。

## Playwright で機能を調べるよう Copilot に依頼する

先ほど、機能が期待どおりに動作することを手動で確認しました。今度は Copilot で利用できる Playwright MCP サーバーを使って確認します。

1. 次のプロンプトを使い、Playwright MCP サーバーで機能を検証するよう Copilot に指示します。

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> Copilot は通常、自動的に MCP サーバーを判断して使うため、明示的に指定しなくてもかまいません。使うサーバーが分かっている場合はプロンプトで指定すると、より一貫した結果を得られ、トークンを節約できます。

2. Copilot がブラウザーで機能を確認するために実行する手順を見ます。
3. 報告を読み、すべてが期待どおりに動作していることを確認します。

Copilot はサーバーを起動し、Playwright で Web サイトを操作し、作業後にサーバーを停止して結果を報告します。

## まとめと次のステップ

Playwright MCP サーバーを使い、Copilot CLI から実際のブラウザーで機能を確認しました。

- Model Context Protocol (MCP) とは何か、Copilot CLI がどのように使うかを学びました。
- まだ利用できない場合は、Playwright MCP サーバーを追加しました。
- エージェントにブラウザーを操作させ、フィルター機能を確認しました。

次は、スキルとブラウザーツールを専門的な役割として組み合わせるため、[QA カスタムエージェントを作成します][next-lesson]。

## リソース

- [MCP とは何か、なぜ注目されているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [Copilot CLI に MCP サーバーを追加する][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
