---
title: "レッスン 9 - GitHub Copilot CLI のスラッシュコマンド"
description: "コンテキスト管理、モデル選択、セッション共有、任意のクラウド委任に役立つスラッシュコマンドを確認します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

多くの CLI ツールと同様に、GitHub Copilot CLI には対話に使えるスラッシュコマンドがあります。これらのコマンドでは、高度な機能、内部情報、追加の設定オプションを利用できます。これまでに `/diff`、`/mcp`、`/skills`、`/agent`、`/pr` などを使いました。ほかの便利なコマンドも確認しましょう。

このレッスンでは、次の内容を学びます。

- `/context` と `/compact` で Copilot が会話コンテキストを管理する方法を確認します。
- `/model` で利用可能なモデルを確認します。
- `/share` でセッションをエクスポート、共有する方法を学びます。
- 並列作業、worktree、cloud agent への委任に使うオプションのコマンドを確認します。

## シナリオ

CLI の主なワークフローが完了しました。次は、コンテキストの管理、モデルの切り替え、セッションの共有、[Copilot cloud agent][about-cloud-agent] への任意の委任など、追加機能を見ていきます。

## Copilot CLI のコンテキストを確認する

規模が大きい、または複雑なタスクに取り組むと、モデルの最大コンテキストウィンドウに達することがあります。Copilot CLI は必要に応じて会話を自動的に圧縮します。また、スラッシュコマンドを使って自分で状態を確認したり圧縮したりできます。

1. Codespace に戻り、Copilot CLI をまだ起動していない場合はリポジトリルートから起動します。
2. 次のコマンドを入力します。

   ```plaintext
   /context
   ```

3. モデル、現在のトークン使用量、システム指示、ツール、メッセージ、空き領域にコンテキストがどのように分けられているかを確認します。
4. 会話を圧縮します。

   ```plaintext
   /compact
   ```

5. もう一度 `/context` を入力して、結果を比較します。会話が短い場合は、大きな違いがないこともあります。

> [!NOTE]
> コンテキストウィンドウが埋まると、Copilot CLI は自動的にコンテキストを圧縮します。自分でタイミングを決めたいときは `/compact` を使います。関係のない別タスクを始める場合は、`/clear` または `/new` で新しい会話を開始します。

## モデルを選ぶ

モデルごとに得意分野があり、開発者によって好みも異なります。Copilot CLI では、利用できるモデルを表示して選択できます。

1. 次のコマンドを入力します。

   ```plaintext
   /model
   ```

2. 利用可能なモデルと使用状況の情報を確認します。
3. 現在のモデルを使うか、別のモデルを選ぶか、<kbd>Esc</kbd> を押して一覧を閉じます。

## セッションを共有する

チームで協力して学びを共有すると、AI ツールをより効果的に使えるようになります。`/share` コマンドでは、セッションを Markdown または HTML ファイルにエクスポートしたり、共有可能なリンクを作成したり、GitHub gist を公開したりできます。

1. `/help` を入力し、インストール済みのバージョンにある `/share` のオプションを確認します。
2. セッションを共有する場合は、ローカルの Markdown へのエクスポートに使う `/share file` など、用途に合った方法を選びます。
3. エクスポートした内容を送信または公開する前に確認します。セッションのエクスポートには、プロンプト、応答、プロジェクトの詳細が含まれる場合があります。

リンクや gist の公開は任意です。チームが共有を意図していないリポジトリや会話の内容を公開しないでください。

## オプション: 作業を並列化または委任する

ワークショップの主要な内容は完了しました。Copilot CLI には、大規模なタスク向けのコマンドもあります。

- `/fleet` は独立したサブタスクを複数のサブエージェントに分配し、並列実行できます。
- `/worktree` は別のタスク用に独立した Git worktree を作成できます。
- `/delegate` は非同期に作業する Copilot cloud agent にタスクを送り、pull request を作成させることができます。

これらのコマンドは worktree の作成やリモート作業を伴うため、任意です。試すときは、新しい範囲の明確なタスクから始め、通常のワークフローで結果をレビューします。非同期エージェントの作業を詳しく確認する場合は、[cloud agent ワークショップ][cloud-workshop]に進んでください。

## まとめと次のステップ

Copilot CLI のスラッシュコマンドを使って、設定を変更し、セッションを共有し、内部の状態を確認しました。

- `/context` と `/compact` で Copilot が会話コンテキストを管理する方法を確認しました。
- `/model` で利用可能なモデルを確認しました。
- `/share` でセッションをエクスポート、共有する方法を学びました。
- 並列作業、worktree、cloud agent への委任に使うオプションのコマンドを確認しました。

Copilot CLI にはほかにも多くのスラッシュコマンドがあります。最後に、これまでに学んだことを[振り返り][next-lesson]、次の学習方法を確認しましょう。

## リソース

- [Copilot CLI コマンドリファレンス][cli-reference]
- [Copilot CLI のコンテキスト管理][context-management]
- [Copilot cloud agent について][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
