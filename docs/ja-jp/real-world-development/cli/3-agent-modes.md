---
title: "レッスン 3 - エージェントモード: Plan と Autopilot"
description: "Plan で実装方針を合意し、Autopilot で Issue に基づくフィルター機能を構築して、Interactive モードで結果をレビューします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

プロジェクトに小さな機能を追加しました。大きな変更には、より確実なプロセスが必要です。GitHub Copilot CLI は、組織の既存の開発フローに沿って適切な成果を作るために役立ちます。このレッスンから数回にわたり、一般的なエージェント主導の開発プロセスを体験します。Issue に基づく新機能の作成から始め、コードの妥当性と機能の動作を確認し、最終的にプロジェクトにマージします。

> [!NOTE]
> 機能開発の間は、同じ会話とブランチを続けて使います。通常、ファイルの種類ごとに異なるブランチや PR を作成しますが、ここでは主な概念に集中するため手順を簡略化します。

このレッスンでは、次の内容を学びます。

- GitHub Issue から Copilot の新しい会話を開始します。
- Plan モードで要件を決めます。
- Autopilot モードで新機能を実装します。
- コードをレビューします。
- 転送されたブラウザーで機能を手動で検証します。

続くレッスンでは、リポジトリの指示を更新し、既存の `quality-checks` スキルをカスタマイズし、MCP による検証を追加して QA エージェントを作成し、機能の PR を開きます。

## シナリオ

Tailspin Toys のカタログが増えてきたため、訪問者がゲームをカテゴリと発行元で絞り込めるようにします。バックログの Issue には機能が記載されていますが、カテゴリを組み合わせる方法など、コーディング前に合意しておきたい詳細があります。Plan モードで決定し、Autopilot には範囲を限定した実装を許可します。

## 背景

AI コーディングエージェントを開発に取り入れても、基本は変わりません。むしろ、より重要になります。多くの開発者は次のような流れで作業します。

1. 実装する作業を記述した Issue から始めます。
2. 構築する内容を計画します。
3. コードを作成してレビューします。
4. テストを実行してコードを検証します。
5. 新しい機能を手動で検証します。
6. pull request (PR) を作成します。
7. コードレビューが完了し、継続的インテグレーションが成功したら、コードをマージします。

> [!NOTE]
> チームや組織によって具体的な手順は異なりますが、多くの場合は上記の流れを調整したものです。

標準的な手順を守ることで、AI が生成したコードが要件を満たし、手作業で書いたコードと同じレビューと検証を受けるようにします。

## 会話モード

**会話モード**は、エージェントに許可する自律性の度合いを決めます。<kbd>Shift</kbd>+<kbd>Tab</kbd> を押すとモードを切り替えられます。

- **Interactive**: エージェントと一緒に作業します。エージェントは変更を提案し、次の指示を待ちます。
- **Plan**: エージェントが計画を作成します。プロジェクトファイルの編集は行いません。
- **Autopilot**: エージェントが自律的に作業し、コードを書き、テストを実行して、タスクが完了するまで反復します。

まず Plan モードで計画を確認し、その後 Autopilot モードで実装します。

## Issue から開始する

フィルター機能の作業を始める前に、Codespace に戻ってリポジトリとターミナルを準備します。

1. Codespace に戻ります。停止している場合は、再起動します。
2. 星評価の PR がマージ済みであることを確認します。
3. ターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押します。
4. `main` を更新し、フィルター機能用のブランチを作成します。

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Copilot CLI を起動します。

   ```bash
   copilot --yolo
   ```

6. <kbd>Tab</kbd> を 2 回押して **Issues** タブを開きます。
7. <kbd>A</kbd> を押してすべての Issue を表示します。
8. 矢印キーを使って **Allow users to filter games by category and publisher** というタイトルの Issue を選択します。
9. <kbd>C</kbd> を押して Issue をプロンプトに追加し、**Session** タブに戻ります。

プロンプトの先頭に `#7` (または同様の番号) が追加されます。`#` を使うと、GitHub 上の Issue や pull request (PR) をコンテキストに含められます。

## フィルター機能を計画する

計画を立てることで、機能やタスクを Copilot に任せる前に実装方針を決められます。複雑な作業では、少し時間をかけて計画することをお勧めします。Plan モードに切り替えて計画の作成を依頼しましょう。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Plan モードに切り替えます。プロンプトの下に **Plan** と表示されることを確認します。
2. 前の手順で追加した Issue の後ろに、次のプロンプトを入力します。

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot はプロジェクトを調べ、最適な方法を決めて計画を作成します。

3. 作業中、フィルターの動作について質問されることがあります。希望する動作を回答します。どの選択も間違いではありません。
4. 計画ができたら、<kbd>Control</kbd>+<kbd>E</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) を押して展開します。
5. 上下にスクロールして計画を確認します。
6. 自分の決定と合わない部分があれば、Copilot に修正を依頼します。

## Autopilot を承認する

計画を作成してレビューしました。次に Autopilot を使って実装します。

Autopilot では、Copilot がタスクの完了を判断するまで問題に取り組みます。

1. **Accept plan and build on autopilot (recommended)**、またはインストール済みのバージョンで同様のラベルの項目を選択します。
2. プロンプトの下に **Autopilot** と表示されることを確認します。
3. Copilot が計画に従って反復し、コードを生成してテストを実行する様子を確認します。

> [!NOTE]
> 承認すると実装がすぐに始まる場合があります。先に計画をレビューしてください。依存関係が不足している、またはポートが競合していると Copilot が報告した場合は、チェック完了と判断する前にセットアップの問題を解決します。

## 実装をレビューして検証する

生成されたコードは、ほかのコードと同じようにマージ前にレビューします。コードを確認し、サイトを起動して動作も確かめましょう。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Interactive モードに切り替えます。モード表示に **Plan** または **Autopilot** が含まれないことを確認します。
2. `/diff` を入力して、フィルターの実装とテストを確認します。
3. Issue の要件と計画で決めた内容に照らし合わせます。
4. コードをレビューしたら <kbd>Esc</kbd> を押して差分ビューを閉じます。
5. プロジェクトのチェック結果を確認し、失敗があれば Copilot に解決を依頼します。

## 新しい機能を試す

コードに問題がないように見えても、実際に動くでしょうか。前と同じようにアプリを起動し、Codespaces の転送ポートからサイトを開きます。

1. Copilot にアプリの起動を依頼します。

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces からポート `4321` が利用可能だと通知されたら、**Open in Browser** を選択します。
3. 計画で決めたカテゴリフィルター、発行元フィルター、およびその組み合わせを試します。
4. リセットと結果なしの場合の動作が、Issue と決定した内容に合っていることを確認します。
5. Codespace に戻り、Copilot に起動した開発サーバーを停止するよう依頼します。

## まとめと次のステップ

会話モードを切り替えて機能を実装し、レビューしました。

- GitHub Issue から Copilot の会話を開始しました。
- Plan モードで要件を決めました。
- Autopilot モードで新機能を実装しました。
- コードをレビューしました。
- 転送されたブラウザーで機能を手動で検証しました。

次は、ドキュメント化された開発手法に沿ったコードを生成するため、[カスタム指示を使用します][next-lesson]。

## リソース

- [GitHub Copilot CLI の Autopilot][autopilot]
- [Copilot CLI コマンドリファレンス][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
