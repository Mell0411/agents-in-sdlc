---
title: "演習 3 - Playwright MCP サーバーで機能をテストする"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: Agent mode][previous-lesson] · [次のレッスン: カスタムエージェント →][next-lesson]

Agent mode でフィルター機能を作成しました。pull request を開く前に、ブラウザーで動作を確認します。自分でアプリを操作する代わりに、**Playwright MCP サーバー**を接続し、Copilot に実際のブラウザーを操作させて機能をテストします。その後、ブランチを公開して PR を作成します。

この演習では、次の内容を学びます。

- Model Context Protocol (MCP) とは何か、MCP サーバーが Copilot に新しいツールを提供する方法を学びます。
- Playwright MCP サーバーをワークスペースに追加します。
- Copilot にブラウザーでフィルター機能を手動テストさせます。
- ブランチを公開し、フィルター機能の pull request を作成します。

## Model Context Protocol (MCP) とは

Agent mode は、エディターの外部にあるツールにアクセスできると、さらに便利になります。Copilot は Model Context Protocol (MCP) を使って外部ツールに接続します。MCP は、エージェントが外部のツールやサービスと通信するための標準的な方法です。

![Agent mode がコンテキスト、LLM、ツールと連携する仕組み、および MCP サーバーと VS Code 拡張機能が提供するツールを示す図](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) は、AI エージェントが外部のツールやサービスと通信する方法を提供します。MCP を使うと、AI エージェントはリソースを使って最新情報にアクセスしたり、ツールを使ってユーザーに代わって操作を実行したりできます。

これらのツールとリソースには MCP サーバーを介してアクセスします。MCP サーバーは AI エージェントと外部ツール (既存の API や NPM パッケージなどのローカルツール) との通信を管理する橋渡し役です。各 MCP サーバーは、エージェントが利用できるツールとリソースのセットを提供します。

よく使われる MCP サーバーをいくつか紹介します。

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: GitHub リポジトリを管理する API にアクセスできます。新しいリポジトリの作成、既存リポジトリの更新、Issue や pull request の管理などを実行できます。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: Playwright を使ったブラウザーの自動操作機能を提供します。Web ページへの移動、フォームへの入力、ボタンの選択などを実行できます。

ほかにも多くの MCP サーバーがあります。GitHub は、エコシステム内のサーバーを見つけたり貢献したりしやすくするため、[MCP レジストリ](https://github.com/mcp)をホストしています。

> [!CAUTION]
> セキュリティの観点から、MCP サーバーはプロジェクトのほかの依存関係と同様に扱ってください。使用する前にソースコードを確認し、発行元を検証して、セキュリティへの影響を考慮します。信頼できる MCP サーバーのみを使い、機密リソースや操作へのアクセスを許可する際は注意してください。

## MCP 設定を確認する

`.vscode/mcp.json` ファイルで、この VS Code ワークスペースが利用できる MCP サーバーを設定します。

1. codespace で `.vscode/mcp.json` を開きます。
2. `github` サーバーがすでに設定されていることを確認します。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

この `github` エントリはプロジェクトのテンプレートに含まれています。そのため、前の演習で Copilot は Issue のバックログを読み取れました。[リモート GitHub MCP サーバー][remote-github-mcp-server]を使うため、ローカルへのインストールは不要です。VS Code は HTTP 経由で接続し、Copilot が最初にツールを使用するときに GitHub で認証します。

## Playwright MCP サーバーを追加する

次に 2 つ目のサーバーを追加します。[Playwright MCP サーバー][playwright-mcp-server]を使うと、Copilot が操作できるブラウザーを利用できます。これを使って機能をテストします。

1. `.vscode/mcp.json` の `github` と並べて `playwright` エントリを追加し、ファイルを次のようにします。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. ファイルを保存します。

リモートサーバーの `github` とは異なり、`playwright` はローカルサーバーです。VS Code はコンピューター上で `npx @playwright/mcp@latest` を実行して起動します。`--headless` フラグは、表示ウィンドウなしで Playwright を実行します。デスクトップ環境がない codespace では、この設定が必要です。

> [!NOTE]
> Tailspin Toys プロジェクトではエンドツーエンドテスト用に Playwright をすでに使っているため、必要なブラウザーもインストール済みのはずです。Copilot がブラウザーの不足を報告した場合は、`npx playwright install chromium` を実行して再試行します。

## サーバーを起動して信頼する

VS Code は MCP サーバーを**必要に応じて**起動します。Copilot がサーバーのツールを初めて必要としたときにサーバーを起動し、信頼するか確認します。

1. `.vscode/mcp.json` の `playwright` エントリの上に **Start** 操作が表示されます。選択してサーバーを起動し、設定が正しいことを確認します。
2. VS Code からサーバーを信頼するか確認された場合は、設定を確認して信頼し、起動を許可します。

手動で起動する必要はありません。省略した場合は、次の手順で Copilot がツールを初めて使うときにサーバーが起動し、信頼するか確認されます。

## フィルター機能をテストする

[Playwright MCP サーバー][playwright-mcp-server]は、Copilot が操作できる実際のブラウザーを提供します。エージェントはページを開き、移動し、フィルターを適用して、結果を読み取って報告できます。会話を続けながら機能が期待どおりに動作するかをすばやく確認できます。

Playwright MCP サーバーは、スクリーンショットではなくページの[アクセシビリティツリー][playwright-mcp-server]を使います。エージェントはボタン、リンク、リスト項目などの構造化されたラベル付き要素を使って推論します。これは支援技術と同じ方法なので、簡単な機能確認をアクセシビリティの予備確認としても利用できます。

サーバーを接続してアプリを起動したら、次のプロンプトを Copilot に送り、作成したフィルター機能を試します。

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot は Playwright MCP サーバーを使ってブラウザーを起動し、各手順を実行して、結果を報告します。Issue の受け入れ基準に照らして報告を確認します。問題があれば、pull request を作成する前に質問するか、コードを修正するよう依頼します。

> [!NOTE]
> このテストでは、アプリが `http://localhost:4321` で実行されている必要があります。開発サーバーを停止した場合は、プロンプトを送る前に再起動します。Playwright MCP サーバーを初めて使うときにブラウザーをダウンロードする場合があります。ブラウザーがないと報告されたら、`npx playwright install chromium` を実行して再試行します。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## ブランチを公開して pull request を作成する

機能が動作することを確認したので、チームがレビューできるよう pull request (PR) を開きます。まず `filtering-vscode` ブランチを公開します。

1. codespace の **Source Control** パネルに移動し、Copilot が行った変更を確認します。
2. **+** アイコンを選択して変更をステージします。
3. **Sparkle** ボタンを使ってコミットメッセージを生成します。

   ![Copilot が変更したファイルを示す Source Control パネルのスクリーンショット](../../../_images/ex3-source-control-changes.png)

4. **Publish** を選択して、ブランチをリポジトリにプッシュします。

## pull request を作成する

pull request の作成には、github.com や GitHub コマンドラインインターフェイス (CLI) など、いくつかの方法があります。今回は GitHub Copilot に作成を依頼します。関連する Issue を見つけて、PR に関連付けられます。

1. Copilot Chat パネルに移動し、**New Chat** を選択して新しいセッションを開始します。
2. Copilot が GitHub ツールを使えるよう、エージェントドロップダウンで **Agent** が選択されていることを確認します。
3. 次のプロンプトで PR の作成を依頼します。

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. 必要に応じて **Continue** を選択し、Copilot に情報収集や操作の実行を許可します。GitHub ツールの初回使用時に GitHub 認証を求められた場合は、指示に従って許可します。
5. Copilot が Issue を検索し、適切なものを見つけて PR を作成することを確認します。
6. Copilot が生成したリンクを選択して PR を確認します。ただし、まだ**マージしないでください**。

## まとめと次のステップ

この演習では、次の内容を学びました。

- Model Context Protocol (MCP) とは何か、MCP サーバーが Copilot に新しいツールを提供する方法を学びました。
- Playwright MCP サーバーをワークスペースに追加しました。
- MCP サーバーを使って、リリース前にブラウザーでフィルター機能を手動テストしました。
- ブランチを公開し、フィルター機能の pull request を作成しました。

MCP サーバーを使って機能をテストしましたが、Copilot を拡張する方法は MCP だけではありません。次は、アクセシビリティレビューなどの作業を効率化するため、[カスタムエージェントを作成します][next-lesson]。

## リソース

- [MCP とは何か、なぜ注目されているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP Registry][mcp-registry]
- [VS Code の MCP サーバー][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
