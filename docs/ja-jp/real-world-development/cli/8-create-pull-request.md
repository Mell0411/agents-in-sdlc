---
title: "レッスン 8 - 機能の PR を作成してマージする"
description: "フィルター機能、指示、スキルの更新、QA プロファイル、テストをまとめてレビューし、PR を作成して Agent Merge を使用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

フィルター機能の実装、指示の更新、スキルの更新、品質保証 (QA) プロファイル、テストが、1 つのブランチに保存されています。すべてをまとめてレビューし、pull request を作成しましょう。最初の pull request (PR) は自分でマージしました。今回は **Agent Merge** にプロセスを任せます。

> [!NOTE]
> 通常は、機能、指示の更新、スキルの更新、QA エージェントを複数の PR に分けます。このワークショップでは手順を簡略化するため、フィルター機能と品質ワークフローに関する作業を 1 つの会話とブランチにまとめました。

このレッスンでは、次の内容を学びます。

- Agent Merge と、マージのライフサイクルを自動化する方法を確認します。
- 機能の変更全体と検証の証拠を確認します。
- フィルター機能の PR を作成します。
- レビュー後にのみ Agent Merge を有効にし、PR がマージされたことを確認します。

## シナリオ

フィルター機能のワークフローで、Copilot を使って機能を計画、実装、検証しました。Tailspin Toys は、マージを許可する権限は開発者が持ったまま、残りの PR 作業を自動化したいと考えています。

## Agent Merge について

**Agent Merge** は、pull request をマージするために必要な残りの作業を自動化します。有効にすると、Copilot は失敗した継続的インテグレーション (CI) チェックの修正、レビューコメントへの対応、必要に応じたリベースなど、PR を妨げている問題に取り組みます。その後、リポジトリが許可していれば GitHub の自動マージを有効にします。

これまでは自分で **Merge pull request** を選択していました。Agent Merge にその作業を任せることもできます。Agent Merge を有効にする前に、作業をレビューして問題がないことを確認します。

## Agent Merge で PR を管理する

コードの作成が終わったら、すべてをレビューして PR を作成し、残りの作業を Agent Merge に任せます。

1. Codespace に戻ります。
2. `/agent` を入力してエージェントダイアログを開きます。
3. 一覧から **Default** を選択し、<kbd>Enter</kbd> を押します。
4. `/pr create` コマンドで新しい PR を作成します。
5. `/pr agentmerge` コマンドで Agent Merge を有効にします。
6. Copilot は PR の継続的インテグレーションを監視します。すべてのチェックが成功すると、マージを実行します。
7. Copilot から `PR #14 was squash-merged successfully.` のようなメッセージが表示されたことを確認します。

> [!IMPORTANT]
> Agent Merge は、必須の承認、ブランチ保護、マージキュー、リポジトリの設定、権限不足を回避しません。処理がブロックされた場合は、報告された理由を確認し、リポジトリで許可されていればレビュー済みの変更を手動でマージします。

## まとめと次のステップ

コードの生成、テストと検証、pull request のプロセスなど、開発プロセスの一部を自動化しました。

- Agent Merge と、マージのライフサイクルを自動化する方法を確認しました。
- 機能の変更全体と検証の証拠を確認しました。
- フィルター機能の PR を作成しました。
- レビュー後にのみ Agent Merge を有効にし、PR がマージされたことを確認しました。

次は、コンテキスト、モデル、共有、オプションのクラウドへの委任に使える[便利な Copilot CLI のスラッシュコマンド][next-lesson]を確認します。

## リソース

- [Copilot CLI で pull request を管理する][manage-prs]
- [Copilot CLI コマンドリファレンス][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
