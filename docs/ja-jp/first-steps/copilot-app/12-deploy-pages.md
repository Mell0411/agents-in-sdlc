---
title: "レッスン 12 - GitHub Pages でサイトをデプロイする"
description: "GitHub Copilot app を使って Pages ワークフローを設定し、pull request をレビューして、公開された Space Quiz を確認します。"
authors:
  - jamesmontemagno
lastUpdated: 2026-10-12
---

GitHub Pages で Space Quiz を公開し、誰でもアクセスできるようにします。Copilot はワークフローの準備とデプロイを支援しますが、変更をレビューしてサイトが公開されたことを確認するのは自分の役割です。

このレッスンでは、次の内容を学びます。

- Copilot に GitHub Pages のデプロイを設定させます。
- マージする前にワークフローと pull request をレビューします。
- デプロイの進行状況を確認し、公開されたサイトを検証します。

## シナリオ

Space Quiz は GitHub リポジトリにありますが、まだ簡単に試せる場所がありません。単一ファイルのサイトを GitHub Pages で公開すると、サーバーやランタイム依存関係を追加せず、公開 URL でアクセスできるようになります。

## Copilot にデプロイの準備を依頼する

> [!WARNING]
> pull request の作成によってコードがコミット、プッシュされることがあります。プロンプトを送る前に、承認設定と対象のリポジトリを確認してください。**Approve all** が有効な場合、別途確認が求められないことがあります。

`space-quiz` の Copilot app セッションで、次のプロンプトを送信します。

```plaintext
Prepare a GitHub Pages deployment for this repository. Create a GitHub Actions workflow that builds the website if needed and deploys it to GitHub Pages after changes reach the default branch. Add a README describing the app and how to run it locally if one does not already exist. Create a pull request with these changes and report its link and any required Pages settings or permissions. Do not merge, enable auto-merge, change Pages settings, or deploy. Stop and wait for my review.
```

## ワークフローと pull request をレビューする

1. README はリポジトリにまだ存在しない場合にのみ追加されていることを確認します。
2. pull request の変更をレビューします。ワークフローのビルド・デプロイ手順、トリガー、権限を確認します。
3. ワークフローが、マージされていない pull request ではなく既定のブランチからデプロイすることを確認します。
4. 修正が必要な場合は依頼し、更新された差分をレビューします。マージを許可する前に、必須の pull request チェックが成功するまで待ちます。

## マージとデプロイを許可する

> [!WARNING]
> 次のプロンプトは、レビュー済みの pull request のマージ、リポジトリの Pages 設定の変更、サイトの公開をエージェントに許可します。レビューを完了し、リポジトリと pull request を確認した後にのみ送信してください。承認設定によっては、別途確認されずにこれらの操作が実行されることがあります。

`<PR_NUMBER>` をレビューした pull request の番号に置き換えてから、別のプロンプトとして送信します。

```plaintext
I have reviewed pull request <PR_NUMBER> in this repository and authorize its merge and deployment. Enable GitHub Pages using GitHub Actions, merge that pull request only after required checks pass, and monitor the deployment until it succeeds. Report the final URL of the live website. If you lack permission to enable Pages or merge, tell me exactly what I need to approve or do; do not report success until the site is live.
```

![マージされた space-quiz pull request、Build website・Upload artifact・Deploy to Pages の各手順が成功した GitHub Actions の Deploy website 実行、yourname.github.io/space-quiz/ に公開された Space Quiz の 3 段階を示す図。](../../../_images/first-steps-app-pages-deploy.svg)

## デプロイを確認する

1. GitHub Actions のデプロイ実行が成功するまで確認します。
2. 報告された URL を開き、Space Quiz が表示されることを確認します。

エージェントが Pages の有効化や pull request のマージを実行できない場合は、提示された具体的な手順に従います。デプロイが成功し、公開サイトが読み込まれるまで、タスクを完了したと判断しないでください。

## まとめと次のステップ

Space Quiz をライブサイトとして公開し、エージェントの報告だけに頼らず、デプロイを検証しました。[レッスン 13「レビューと次のステップ」][next-lesson]に進みます。

[next-lesson]: ../13-review/
