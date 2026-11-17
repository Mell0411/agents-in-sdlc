---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタム指示][previous-lesson] |
|:--|

技術的負債に悩んでいない組織は、ほとんどないでしょう。未解決のセキュリティ問題、更新が必要なレガシーコード、実装する時間がなくバックログに残った機能リクエストなどがあります。GitHub Copilot cloud agent は、コードの更新や機能の追加といったタスクを自律的に実行できます。作業が終わると、人間の開発者がレビューできるドラフト PR を作成します。手間のかかるタスクを委任して開発を加速し、開発者はより大きな課題に集中できます。

Copilot cloud agent を使って、次の内容を学びます。

- コード生成環境をカスタマイズする。
- 操作を安全に実行する。
- Issue のスコープを明確にする重要性を理解する。
- Issue を Copilot に割り当てる。

## シナリオ

Tailspin Toys には解決したい技術的負債があります。最初のサイトを開発した委託先は、ドキュメントを十分に作成していません。まず、アプリケーションのすべてのエクスポート関数に TSDoc のドキュメントコメントを追加します。

また、デザインチームはゲームを見つけやすくしたいと考えています。各ゲームの詳細ページに、同じカテゴリに属する関連ゲームを表示します。洗練された実装はまだ必要なく、UX の受け入れテストに使えるものを用意します。現在は作業を妨げているものの、より優先度の高い Issue がほかにあります。

どちらも優先順位が下がりやすいタスクです。Copilot cloud agent に割り当てれば、非同期に作業を進めてもらい、別の作業に集中した後で結果をレビューできます。

## GitHub Copilot cloud agent について

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) は、人間の開発者と同様にバックグラウンドでタスクを実行します。[GitHub Issue を Copilot に割り当てる](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)など、いくつかの方法で作業を開始できます。割り当てると Copilot は進捗を追跡するドラフト pull request を作成し、環境を準備してタスクを始めます。作業中も完了後もセッションを確認できます。提案された解決策のレビュー準備ができると、pull request で通知されます。

## 適切なスコープの指示が重要

GitHub Copilot に魔法はありません。考える作業を省略できるわけではありません。一見簡単な操作でも、詳しく調べると複雑なことがあります。[Copilot cloud agent に依頼するタスクのスコープ][cloud-agent-best-practices]を慎重に決めてください。AI ペアプログラマーとして扱い、段階を踏んで学び、試し、調整します。生成 AI が加わっても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム指示

前の演習では、カスタム指示と Copilot への指示方法を学びました。すでにカスタム指示の演習を完了していれば、`.github/copilot-instructions.md` が機能することを確認しています。未実施の場合は、cloud agent に作業を割り当てる前に、このリポジトリに含まれる指示ファイルを読み取り専用で確認して、後で効果を確かめましょう。

リポジトリの GitHub Web UI、または起動済みの codespace で次のファイルを開きます。

- `.github/copilot-instructions.md` — **Code standards** セクション、特に TypeScript の規約と TSDoc ドキュメントコメントの要件を確認します。
- `.github/instructions/unit-tests.instructions.md` — `applyTo` frontmatter を確認します。この指示は `**/*.test.ts` ファイルに適用されます。

次のセクションで `Code lacks documentation` Issue を cloud agent に割り当てると、これらの指示ファイルに基づく TSDoc コメント、TypeScript の規約、コメントヘッダーが pull request に反映されます。後続の演習で PR をレビューするときに、改めて確認します。

## Copilot cloud agent の開発環境をセットアップする

誰がコードを書く場合でも、通常は適切な環境と初期設定が必要です。タスクを Copilot に割り当てる場合も同様で、Copilot はソフトウェアエンジニアのように作業します。

Cloud agent は作業環境に [GitHub Actions][github-actions] を使います。`.github/workflows/copilot-setup-steps.yml` に設定する[専用のセットアップワークフロー][setup-workflow]を作成して、作業開始前に実行するツールや依存関係を指定できます。このラボの流れを円滑にし、学習に集中できるよう、あらかじめ設定されています。単一の Astro アプリに必要な Node.js、プロジェクトの依存関係、エンドツーエンドテスト用のブラウザーバイナリ、マイグレーションと初期データ投入済みの SQLite データベースを用意します。

```yaml
name: "Copilot Setup Steps"

# リポジトリの "Actions" タブからセットアップ手順をテストできます
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # セットアップ手順に必要な最小限の権限を指定します。Copilot は操作用の独自トークンを受け取ります。
    permissions:
      # セットアップ手順の一部としてリポジトリをクローンする場合 (依存関係のインストールなど) は、`contents: read` 権限が必要です。ここでクローンしない場合は、手順の完了後に Copilot が自動的にクローンします。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # フロントエンド / アプリのセットアップ - Node.js (アプリ全体は Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # ローカル SQLite データベースをマイグレーションして初期データを投入し、ビルドとテスト用のデータを用意します。
      - name: Set up the database
        run: npm run db:setup
```

一般的な GitHub ワークフローと同じように見えますが、いくつか重要な点があります。

- `copilot-setup-steps` という 1 つのジョブが含まれます。このジョブは Copilot が pull request の作業を開始する前に GitHub Actions で実行されます。
- `workflow_dispatch` トリガーが設定されています。これにより、Copilot の実行を待たずにリポジトリの **Actions** タブからワークフローを手動実行し、正常に動作するかテストできます。

## ドキュメントを追加する

ドキュメントが重要なのは誰もが理解していますが、情報が古かったり不足していたりするプロジェクトは少なくありません。こうした技術的負債は放置されがちで、生産性を下げ、コードベースの保守や新しい開発者の参加を難しくします。Copilot はドキュメント作成が得意なため、cloud agent に割り当てるのに適した Issue です。バックグラウンドで必要なドキュメントを作成し、後の演習で成果をレビューします。

1. 新しいブラウザータブで github.com のリポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue のダイアログを開きます。
4. **Blank issue** を選択して Issue を作成します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次の内容を入力します。

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選択して、割り当てダイアログを開きます。

  ![Issue に Copilot を割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。
10. **Pull Requests** タブを選択します。
11. 新しく作成された pull request (PR) を開きます。タイトルは `[WIP]: Code lacks documentation` のような名前です。PR が一覧に表示されない場合は、少し待ってからブラウザーを更新してください。
12. 数分後、Copilot が作成した todo リストを確認できるはずです。

> [!NOTE]
> Copilot が PR に todo リストを表示するまで、数分かかる場合があります。Copilot は環境を準備し (前述のワークフローを実行)、プロジェクトを分析して、課題に取り組む方法を決めています。

13. リストと、実行予定のタスクを確認します。
14. pull request のタイムラインを下にスクロールして、Copilot が Issue の作業を開始したことを示す更新を確認します。
15. **View session** ボタンを選択します。

  ![Copilot のセッションビュー](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 表示が更新されない場合は、ウィンドウを更新してください。

16. 実行中のセッションをスクロールし、Copilot の作業を確認します。コードを調べて状態を理解し、適切な計画を考え、コードを作成する様子を確認できます。

この作業には数分かかる場合があります。Copilot cloud agent の主な目的の 1 つは、タスクを非同期で実行して別の作業に集中できるようにすることです。この機能を活用して、もう 1 つタスクを cloud agent に割り当ててから、アプリケーションの機能追加に取り組みます。

## ゲーム詳細ページに関連ゲームのセクションを追加する

GitHub Copilot cloud agent の利点の 1 つは作業を分担できることです。デザインチーム向けに関連ゲームのセクションを追加する作業は短時間で終わるかもしれませんが、その時間をほかの作業に使えます。Copilot cloud agent に割り当てましょう。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue のダイアログを開きます。
4. **Blank issue** を選択して空のテンプレートを使います。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次の内容を入力します。

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選択して、割り当てダイアログを開きます。

  ![Issue に Copilot を割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

まもなく、Issue の最初のコメントに 👀 が表示され、Copilot が作業を開始したことを確認できます。

![Copilot が Issue の作業中であることを示す目の絵文字](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は 2 つ目の依頼に取り組んでいます。Copilot cloud agent はソフトウェアエンジニアと同じように作業するため、常時監視する必要はありません。完了後にレビューします。次に、カスタムエージェントの作成と使用を見ていきましょう。

## まとめと次のステップ

[GitHub Copilot cloud agent][copilot-agents] を学びました。cloud agent に Issue を割り当てると、非同期に作業できます。技術的負債の解消、新機能の作成、別のフレームワークへのコード移行などに利用できます。

次の内容を学びました。

- コード生成環境をカスタマイズする。
- 操作を安全に実行する。
- Issue のスコープを明確にする重要性を理解する。
- Issue を Copilot に割り当てる。

Cloud agent がバックグラウンドで作業している間に、カスタムエージェントの作成と使用に進みます。[Copilot cloud agent は MCP サーバーも使用でき][cloud-agent-mcp]、前のモジュールで学んだカスタム指示も利用できます。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップワークフローに関するベストプラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタムエージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
