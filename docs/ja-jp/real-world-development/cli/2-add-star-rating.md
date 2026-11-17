---
title: "レッスン 2 - 星評価を追加する: 小さな成果"
description: "Copilot CLI でゲームカードに小さな変更を加え、転送されたブラウザーで確認し、最初の pull request をマージします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Copilot CLI をインストールし、会話を試しました。次はプロジェクトに初めて変更を加えます。変更は小さく保ちましょう。ゲームのデータには星評価がありますが、ホームページのゲームカードには表示されていません。エージェントに評価を表示するよう依頼し、変更をレビューして、最初の pull request としてマージします。

このレッスンでは、次の内容を学びます。

- 機能ブランチで Copilot の会話を開始します。
- エージェントに小さな変更を依頼します。
- `/diff` で変更をレビューします。
- 転送されたブラウザーでアプリを実行し、変更を確認します。
- 最初の pull request を作成してマージします。

## シナリオ

Tailspin Toys の各ゲームには星評価があり、ゲーム詳細ページにはすでに表示されています。しかし、ホームページのゲームカードにはタイトル、カテゴリ、発行元、説明しか表示されません。ウォームアップとして、各カードに既存の評価を表示します。最初のセッションにぴったりの、小さく独立した変更です。

## 会話の構成

**会話**は、Copilot CLI でタスクに取り組む場所です。Copilot app とは異なり、通常の CLI 会話では、専用の worktree ではなく、ターミナルで現在チェックアウトしているリポジトリと Git ブランチを使います。保存した会話は後で再開できます。ファイルとブランチは通常の Git 状態としてディスク上に残ります。

会話には 3 つの情報が表示されます。プロンプトとエージェントの応答、ファイルを調べて編集する際のツール操作、そして `/diff` で確認できる変更です。

## 会話を開始して変更を依頼する

機能の実装を始めるため、新しい会話を開始します。

1. Codespace に戻ります。
2. まだターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押します。
3. まだ起動していない場合は、次のコマンドで Copilot を起動します。

   ```bash
   copilot --yolo
   ```

4. スラッシュコマンド `/new` を実行して <kbd>Enter</kbd> を押し、新しいセッションを開始します。
5. 次のプロンプトで変更を依頼します。

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

Copilot はプロジェクトを調べ、ゲームの詳細を表示するファイルを探して必要なコードを作成します。Copilot CLI で新機能を追加できました。

## 差分をレビューする

AI が生成した変更は、小さなものでもマージ前にレビューします。Copilot CLI で変更を確認しましょう。

1. `/diff` を入力し、変更されたすべてのファイルを確認します。
2. ゲームカードに数値の評価がある場合はそれが表示され、`starRating` が `null` の場合は `No rating yet` と表示されることを確認します。
3. テストで両方の状態がカバーされていることを確認します。
4. Copilot が実行したチェックの結果を確認し、失敗があれば修正を依頼します。
5. レビューが終わったら <kbd>Esc</kbd> を押して差分画面を閉じます。

> [!NOTE]
> Copilot を含む生成 AI ツールは確率的に動作するため、コードは毎回異なる場合があります。特定の実装を期待するのではなく、動作を確認してください。

## 変更を確認する

コードを読むだけで動作すると判断せず、Copilot に Web サイトを起動してもらい、Codespaces で転送されたブラウザーから更新された UI を確認します。

1. Copilot にアプリの起動を依頼します。

   ```plaintext
   Start the app so I can inspect the star-rating change in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces からポート `4321` が利用可能だと通知されたら、**Open in Browser** を選択します。
3. ゲームカードに 5 段階の評価が表示されることを確認します。
4. Copilot に戻り、起動したサーバーを停止するよう依頼します。

   ```plaintext
   Stop the development server you started.
   ```

## 最初の pull request を作成してマージする

機能を作成しました。次に、新しいコードをプロジェクトにマージする pull request (PR) を作成します。

1. 既定のエージェントに変更をコミットするよう依頼します。

   ```plaintext
   Commit the reviewed star-rating changes with an appropriate commit message.
   ```

2. `/pr create` を入力します。Copilot CLI は PR の作成時に既存のコミットをプッシュし、PR の URL を表示します。
3. Copilot CLI が表示した URL を、<kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら選択して PR を開きます。
4. 変更されたファイルとチェックを確認します。
5. 問題がなければ **Merge pull request** を選択し、マージを確定します。
6. Codespace に戻り、`/exit` で Copilot CLI を終了します。
7. ローカルの `main` を更新します。

   ```bash
   git checkout main
   git pull
   ```

## まとめと次のステップ

おめでとうございます。GitHub Copilot CLI を使って、最初の変更をリリースしました。

- 機能ブランチで Copilot の会話を開始しました。
- エージェントにゲームカードの小さな変更を依頼しました。
- `/diff` で変更をレビューしました。
- 転送されたブラウザーでアプリを実行し、星評価を確認しました。
- 最初の pull request を作成してマージしました。

次は [フィルター機能の Issue から開始し、Plan と Autopilot モードを使って][next-lesson]より大きな機能を作ります。

## リソース

- [GitHub Copilot CLI について][about-copilot-cli]
- [Copilot CLI コマンドリファレンス][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
