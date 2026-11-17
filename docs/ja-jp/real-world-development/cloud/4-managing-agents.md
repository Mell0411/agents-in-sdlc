---
title: "演習 4 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

ここまでの演習で、GitHub Copilot cloud agent にユーザーエクスペリエンスの改善や機能追加を目的とする 3 つのタスクを依頼しました。cloud agent は非同期かつ自律的に作業しますが、タスクを監視することも重要です。

GitHub.com の[エージェントページ][agents-page]など、cloud agent に割り当てたタスクを管理する方法はいくつかあります。このミッションコントロールでは、pull request (PR) が開いているすべてのエージェントタスクを確認できます。実行内容を調べたり、進行中のセッションに指示を出したりできます。

このレッスンでは、次の内容を学びます。

- エージェントページで cloud agent のタスクを監視します。
- 実行中のセッションに追加の機能を依頼します。

## シナリオ

ハイコントラストモードの作成をエージェントに依頼した後、チームはライトモードも追加するとよいと考えました。サイトのスタイルと切り替え機能を更新しているため、ライトモードも含めるのが自然です。エージェントの作業を誘導し、ハイコントラストとライトモードの両方を追加します。

## Copilot cloud agent のタスクを確認する

Copilot cloud agent に割り当てたすべてのタスクの現在の状態を確認します。

1. [Copilot agents ページ](https://github.com/copilot/agents)に移動します。
2. メインペインと左ペインのタスクリストを確認します。Copilot に割り当てた次のタスクが表示されます。
    - コードベースのドキュメントを更新する。
    - ゲーム変更用の API を生成する。
    - Web サイトにハイコントラストモードを追加する。
3. 実行中のタスクを選択し、Copilot が実行した作業を確認します。次のような内容があります。
    - リポジトリをチェックアウトする。
    - Copilot の作業環境を作成する。
    - MCP サーバーをセットアップする。
    - タスクを完了するためのさまざまな手順を実行する。

> [!NOTE]
> 表示される手順は、作業の状態や Copilot が採用した方法によって異なります。

4. 右側に表示される pull request (PR) ペインも確認します。PR や変更されたファイルを表示し、作業をさらに監視できます。

## Cloud agent に指示を追加する

実行中のタスクを確認したので、ハイコントラストモードの作業中にライトモードの切り替えも追加するよう Copilot に依頼します。

1. ハイコントラストモードの追加を示すセッションを選択します。タイトルは Copilot が付けた名前や作業の状態によって異なります。

    ![ミッションコントロールのアクセシビリティセッション](../../../_images/ex6-accessibility-session.png)

2. セットアップが完了して作業が始まるまで、数分間セッションを確認します。以下のようなメッセージが表示されたら、作業が開始しています。
3. **Steer active session while Copilot is working** ダイアログに次のプロンプトを入力します。

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Copilot が作業中のエージェントページで、Steer active session while Copilot is working ダイアログが強調表示されたスクリーンショット](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot がプロンプトを受け取り、作業計画に追加することを確認します。

## Copilot の作業を続ける

Copilot は更新されたタスクに取り組みます。メッセージを送信した時点で作業中だった手順を完了した後、新しい依頼を反映します。

これまでと同様に数分かかります。作業を待つ間に、これまでに学んだことを振り返りましょう。

## まとめと次のステップ

GitHub Copilot cloud agent のタスクを監視し、誘導する中心的な場所である Copilot agents ページを学びました。このミッションコントロールを使うと、進行中と完了済みのタスクを追跡し、実行内容を確認し、進行中のタスクに追加の指示を出せます。

次の内容を学びました。

- ミッションコントロールとエージェントページで cloud agent のタスクを監視する。
- 実行中のセッションに指示を追加して機能を依頼する。

Copilot がアクセシビリティ機能を実装している間に、次は Copilot が作成した pull request を[反復して改善][next-lesson]します。

## リソース

- [Copilot agents ページ][agents-page]
- [カスタムエージェントについて][custom-agents]

---

| [← 前のレッスン: カスタムエージェント][previous-lesson] | [次のレッスン: Copilot の作業を反復する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
