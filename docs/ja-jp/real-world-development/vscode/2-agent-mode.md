---
title: "演習 2 - Copilot Agent Mode で新機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

アプリケーションの単純な更新でも、通常は複数のファイル変更やテスト実行などが必要です。開発者は必要なファイルを探し、変更し、テストを実行し、デバッグし、変更し忘れたファイルを見つけて、さらに更新することになります。作業は続きます。

ここで Copilot Agent Mode が役立ちます。

Copilot Agent Mode は、IDE でより自律的に作業するよう設計されています。開発者と同様に、既存のプロジェクト構造を調べ、必要な更新を行い、テストなどのタスクを実行し、見つかった問題を自動的に修正します。Agent Mode を使ってサイトに機能を追加する方法を確認しましょう。

> [!NOTE]
> 名前は似ていますが、agent mode と cloud agent は異なる用途向けです。Agent mode は IDE で作業し、すばやいフィードバックと対話を可能にします。Cloud agent はチームメンバーのように非同期で作業し、Issue や pull request を通じてやり取りします。

この演習では、次の内容を学びます。

- Copilot Agent Mode でプロジェクトを調べ、関係するファイルを特定し、複数のファイルを調整して変更する。
- GitHub Copilot Agent Mode で UI とデータレイヤーにまたがる新機能を実装する。
- コードベースにマージする前に、Copilot Agent Mode が生成した変更とテストをレビューする。

## シナリオ

ゲーム一覧が増えてきたため、ユーザーがカテゴリと発行元で絞り込めるようにします。前の演習で発行元ヘルパーを追加しました。次は Copilot Agent Mode を使って、残りのデータレイヤー、UI、テスト作業を完了します。

## Tailspin Toys Web サイトを実行する

変更を加える前に、Tailspin Toys Web サイトの現在の機能を確認します。

この Web サイトは、開発者をテーマにしたボードゲームのクラウドファンディングプラットフォームです。ゲームの一覧と詳細を表示します。サイトは単一の Astro アプリで、ビルド時にページを静的 HTML として生成します。ページは frontmatter 内で Drizzle ORM を使ってローカル SQLite データベースを直接クエリします。別のバックエンド API やクライアントサイド UI フレームワークはありません。再利用可能なデータアクセスヘルパーは `src/lib/` にあり、インタラクティブな機能は標準 DOM API を使う小規模な Astro `<script>` で実装します。

### Web サイトを起動する

`npm run dev` タスクを使うと、単一の Astro 開発サーバーを起動できます。GitHub Codespace で次の手順を実行します。

1. codespace に戻ります。前の演習の後、`main` に戻っているはずです。
2. Codespace 内で <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押して新しいターミナルを開きます。
3. フィルター機能用のブランチを作成して切り替えます。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 次のコマンドを実行して Web サイトを起動します。

   ```bash
   npm run dev
   ```

   Astro 開発サーバーが準備できると、次のような URL を示すメッセージが表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> `http://localhost:4321` を開くよう促すダイアログが表示された場合は、**x** を選択して閉じます。

5. ターミナルに表示された Astro サーバーのアドレス `http://localhost:4321` を、<kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら選択して Web サイトを開きます。

> [!NOTE]
> Codespace のターミナルで localhost の URL を選択すると、自動的に `https://<your-codespace-name>-4321.app.github.dev/` にリダイレクトされます。これは Codespace 上の Web サーバーに接続するプライベートトンネルです。

### Web サイトを確認する

Web サイトを起動したら、機能を確認します。主なページは次のとおりです。

- **ホームページ**: ボードゲームのタイトル、画像、説明を一覧表示します。
- **ゲーム詳細ページ**: ゲームを選択すると、タイトル、説明、発行元、カテゴリなどの詳細を表示します。

## Copilot でバックログを調べる

> [!TIP]
> **Copilot Chat を開く**
>
> 次の演習を始める前に codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を開始します。モードとモデルの選択は演習によって異なります。必要な場合は各手順に記載しています。

Web サイトの最初の実装は動作しますが、機能を追加して改善します。まずバックログを確認しましょう。テンプレートからプロジェクトをセットアップすると Issue のバックログが自動的に作成されます。GitHub Copilot に内容を表示させます。

1. Chat ビューのエージェントドロップダウンから **Agent** を選択します。**Agent** エージェントは自律的に計画を立て、複数ファイルを変更し、ターミナルコマンドを実行して、ツールを呼び出します。

   ![Chat ビューのエージェントピッカーのスクリーンショット](../../../_images/shared-chat-mode-selector.png)

2. 利用可能なモデル一覧から **Claude Sonnet 4.5** を選択します。

> [!CAUTION]
> このワークショップの作成者は特定のモデルを推奨するものではありません。ワークショップの作成時に Claude Sonnet 4.5 を使用したため、手順にも記載しています。提案されるコードに一貫性を持たせ、学習しやすくすることを目的としています。ただし LLM は確率的に動作するため、手順とは異なる提案が表示されることがあります。これは想定された動作です。

> [!NOTE]
> LLM の確率的な動作により、Copilot は別の MCP コマンドを使う場合がありますが、タスクを完了できるはずです。

3. 次のプロンプトを送信し、GitHub リポジトリの Issue バックログについて Copilot に質問します。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. **Continue** を選択して、すべての Issue を一覧表示するコマンドを実行します。
5. 生成された Issue の一覧を確認します。

Copilot が、ユーザーに最も役立つと思われる項目に基づいて優先順位も付けていることを確認します。

## 指示ファイルを確認する

コード生成をエージェントに依頼する前に、作業のコンテキストを Copilot に提供する指示ファイルを確認します。Web サイトの機能追加方法に関するコンテキストを含む [ユーザーインターフェイス (UI)][ui-instructions] ファイルを使います。

1. codespace で `.github/instructions/ui.instructions.md` を開きます。
2. 機能追加のガイダンスを確認します。次の内容が含まれます。
   - アーキテクチャの概要。
   - コンポーネントの設計、テスト容易性、アクセシビリティの原則。
   - Astro、Tailwind CSS、Drizzle など各ファイル形式に固有の指示へのリンク。

> [!TIP]
> 指示ファイルでは他の指示ファイルやプロジェクト内ファイルを参照できます。パスは指示ファイルからの相対パスです。指示を再利用可能な小さなファイルに分け、例やひな形を示せます。

## フィルター機能を実装する

フィルター機能の実装には、少なくとも次の 3 つの変更が必要です。

- データレイヤー (`src/lib/`) に残りのフィルターロジックを追加または調整する。
- フィルターの動作を確認するテストを追加または更新する。
- ゲーム一覧ページにフィルター UI を追加する。

さらに、コードベースにマージする前にテストを実行し、成功する必要があります。Copilot Agent Mode にこれらの作業を依頼しましょう。

1. Copilot との現在の会話を続けるか、**New Chat** を選択して新しい会話を開始します。
2. **Add Context**、**Instructions**、**ui** の順に選択し、指示ファイルを指定します。

   ![UI 指示ファイルを選択する画面のスクリーンショット](../../../_images/ex3-select-instructions-file.png)

3. Chat ビューのエージェントドロップダウンで **Agent** が選択されていることを確認します。

   ![Chat ビューのエージェントピッカーのスクリーンショット](../../../_images/shared-chat-mode-selector.png)

4. モデルが **Claude Sonnet 4.5** のままであることを確認します。
5. 次のプロンプトで、バックログの関連 Issue に基づいて実装するよう Copilot に依頼します。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot がプロジェクトを調べて、機能に関係するファイルを見つける様子を確認します。データレイヤーのヘルパー、UI、テストを探し、ファイルを変更してテストを実行します。

   ![Copilot がプロジェクトファイルを調べるスクリーンショット](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot はプロジェクトの調査、変更、テスト実行など複数の作業を行います。タスクやコードベースの複雑さによって、数分かかることがあります。作業中、コードエディターに **Keep** と **Undo** が表示される場合があります。完了後に変更ごとに **Keep** または **Undo** を選べるため、作業中に選択する必要はありません。

7. Copilot から求められたら、**Continue** を選択してテストを実行します。

   ![Copilot Chat でテスト実行の確認を求めるダイアログのスクリーンショット](../../../_images/ex3-agent-mode-run-tests.png)

8. 作業中に一時停止したり、テストが失敗したりすることがあります。問題ありません。Copilot はエラーを検出しなくなるまでコード生成とテストを繰り返します。

   ![Copilot Agent Mode の完了した Chat セッションのスクリーンショット](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 生成されたコードに問題がないか確認します。

> [!CAUTION]
> Copilot などの AI ツールが生成したコードは、必ずレビューしてください。

10. Web サイトを起動したブラウザーに戻り、新しい機能を試します。
11. 動作を確認してコードをレビューしたら、Copilot Chat ウィンドウで **Keep** を選択します。

## まとめと次のステップ

GitHub Copilot Agent Mode を使って、Tailspin Toys Web サイトに機能を追加しました。

- GitHub Copilot Agent Mode で UI とデータレイヤーにまたがる新機能を実装しました。
- Copilot Agent Mode でプロジェクトを調べ、関係するファイルを特定し、複数のファイルを調整して変更しました。
- コードベースにマージする前に、Copilot Agent Mode が生成した変更とテストをレビューしました。

次は [Playwright MCP サーバーで機能をテスト][next-lesson]し、pull request を作成します。

### 発展演習 — ページネーションを実装する

ゲーム一覧が増えると、ページネーションが必要になります。この演習で学んだスキルを使い、Copilot にページネーションの実装を依頼します。次の点を検討してください。

- 既存の指示ファイルを使うなど、現在のベストプラクティスに従います。
- ユーザーがページサイズを選べるようにするか、値を固定するかなど、ページネーションの方式を検討します。
- 希望する実装に必要なガイダンスを、プロンプトで Copilot に伝えます。
- Copilot に変更を依頼し、コンテキストを追加しながら反復することもあります。これは Copilot を使う通常のワークフローです。

## リソース

- [Copilot の ask、edit、agent mode: それぞれの役割と使い分け][choose-mode]
- [VS Code の agent mode][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
