---
title: "演習 5: GitHub Copilot の作業を反復する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

## 作業をレビューする

このラボでは、GitHub Copilot に複数のタスクを依頼して、ユーザーエクスペリエンスを改善し、機能を追加しました。コードにドキュメントを追加し、デザインチーム向けの関連ゲーム機能を作り、ハイコントラストとライトモードの切り替えを含むアクセシビリティ機能を実装しました。コードの変更を確認し、必要に応じて Copilot にフィードバックを伝えて改善してもらいましょう。

### シナリオ

生成 AI を利用しても、ソフトウェア設計と DevOps の基本は変わりません。生成されたコードをレビューし、通常の DevOps プロセスに沿って作業します。チームでコードレビューを有効にする前に、ドキュメント、関連ゲーム機能、アクセシビリティ機能に関する GitHub Copilot の提案を確認しましょう。

## GitHub Copilot cloud agent のセキュリティ

Copilot cloud agent は非同期に、監督を受けずに作業するため、安全性を維持するための制約があります。

- Copilot はリポジトリに対する読み取りアクセスと、コード用のブランチに対する書き込みアクセスのみを持ちます。
- Cloud agent は GitHub Actions 内で実行され、作業用の独立した一時環境を作成します。
- GitHub Actions ワークフローを実行するには、人間の承認が必要です。
- MCP サーバーを含む[外部リソースへのアクセスは、既定で制限されています](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)。

## 生成されたドキュメントをレビューする

まず、GitHub Copilot cloud agent がコードにドキュメントを追加した最初の pull request (PR) を、GitHub.com の標準的な PR インターフェイスで確認します。

> [!NOTE]
> PR を確認すると、GitHub Copilot がファイアウォールでブロックされたという警告が表示される場合があります。Copilot は外部 MCP サーバーへの呼び出しを含む外部リソースへのアクセスが既定で制限されているため、これは**想定どおり**です。必要であれば、Copilot cloud agent の[ファイアウォールをカスタマイズまたは無効化][agent-firewall]できます。

1. github.com のリポジトリに戻ります。
2. **Pull Requests** を選択して PR の一覧を開きます。
3. **Add missing documentation** などのタイトルの pull request を開きます。

> [!NOTE]
> Copilot が作業中の場合、pull request のタイトルに **[WIP]** が含まれます。その場合は作業が完了するまで待ちます。数分かかることがあります。休憩したり、これまで学んだ内容を振り返ったりしてください。

4. PR の準備ができたら **Files changed** タブを選択して変更を確認します。

    ![Files changed タブ](../../../_images/shared-pr-files-changed.png)

5. 新しく追加された TSDoc のドキュメントコメントやほかのドキュメントを確認します。実際の変更は作業内容によって異なります。

    変更を確認するときは、演習 2 の開始時に確認したカスタム指示ファイルに基づく TSDoc のドキュメントコメント、TypeScript の規約、コメントヘッダーに注目します。

6. 変更を確認したら **Conversation** タブに戻り、下にスクロールします。
7. ワークフローの承認待ちを示す表示を探します。
8. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する](../../../_images/shared-approve-workflows.png)

9. PR のチェック欄にワークフローがキューに入ったことが表示されます。問題がなければ、単一の Astro アプリのプロジェクトチェックが成功します。完了まで数分かかることがあります。

## GitHub Copilot に変更を依頼する

Copilot との pull request 作業は一方通行ではありません。チームメンバーと同じように、pull request やコードへのインラインコメントで Copilot にメンションできます。Copilot はコメントを確認し、対応するための新しいセッションを開始します。結果は確定的ではないため、必ずしも適切なプロンプトを指定できるわけではありません。Copilot に依頼できる変更の例を示します。

- 各コードファイルの先頭に、役割を説明するコメントヘッダーを追加する。
- TypeScript と Astro ファイルに TSDoc のドキュメントコメントを追加する。
- Astro アプリの構造を説明する README を作成する。

1. 生成されたドキュメントを変更するよう依頼し、他のユーザーと同じ方法で **@copilot** をメンションします。上記の例や、コードベースのドキュメントに関する別の提案を使ってください。
2. **View Session** を選択して Copilot の作業を確認します。更新のために新しいセッションが始まります。
3. **Back to pull request** を選択して pull request に戻ります。

    ![pull request に戻る](../../../_images/ex7-back-to-pr.png)

4. Copilot の作業が完了すると、pull request に新しいコミットが表示されます。
5. **Files changed** タブを選択して変更を確認します。

満足するまで反復できます。作業が完了したら、ドラフト PR をレビュー可能な状態にして `main` ブランチにマージします。

![PR をレビュー可能な状態にする](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

ゲーム詳細ページに関連ゲームを表示する Issue を解決するため、Copilot が作成した PR を確認します。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Show related games on the game details page** などのタイトルの PR を選択します。
4. **Files changed** タブを選択して、生成されたコードを確認します。
5. 変更を確認したら **Conversation** タブに戻り、下にスクロールします。
6. ワークフローの承認待ちを示す表示を探します。
7. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する](../../../_images/shared-approve-workflows.png)

8. PR のチェック欄にワークフローがキューに入ったことが表示されます。問題がなければ、単一の Astro アプリのプロジェクトチェックが成功します。完了まで数分かかることがあります。
9. **任意:** Codespace でこのブランチに切り替えて、関連ゲーム機能を手動でテストすることもできます。Codespace に移動してターミナルを開き、次のコマンドを実行します (`<branch-name>` は Copilot が作成したブランチ名、例: **copilot/fix-8** に置き換えます)。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot が関連ゲーム機能を作成しました。前と同じように、Copilot cloud agent に変更を依頼して反復できます。たとえば、表示する関連ゲームの数を調整したり、コメントヘッダーと TSDoc のドキュメントコメントを追加したりできます (この Issue はカスタム指示を更新する**前に**割り当てたことを思い出してください)。**Conversation** タブでコメントすると、Copilot が確認して新しいセッションを開始します。

## アクセシビリティ機能をレビューする

最後に、アクセシビリティ用のカスタムエージェントで実装した機能を確認します。この PR には、演習 3 で依頼したハイコントラストモードと、演習 4 のミッションコントロールで依頼したライトモードが含まれます。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Add high contrast mode to website** などのタイトルの PR を選択します。

> [!NOTE]
> Copilot が作業中の場合、pull request のタイトルに **[WIP]** が含まれます。作業が完了するまで待ちます。数分かかることがあります。

4. **Files changed** タブを選択して生成されたコードを確認します。
5. 実装をレビューし、特に次の点を確認します。
   - モード切り替え用の UI コンポーネント。
   - ユーザー設定を保存するためのローカルストレージの使用。
   - ハイコントラストとライトモード用の CSS またはスタイルの変更。
   - アクセシビリティ属性 (ARIA ラベル、キーボード操作など)。
   - モードを切り替える JavaScript/TypeScript コード。

6. 変更を確認したら **Conversation** タブに戻り、下にスクロールします。
7. ワークフローの承認待ちを示す表示を探します。
8. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する](../../../_images/shared-approve-workflows.png)

9. PR のチェック欄にワークフローがキューに入ったことが表示されます。問題がなければ、単一の Astro アプリのプロジェクトチェックが成功します。完了まで数分かかることがあります。
10. **任意:** Codespace でこのブランチに切り替えて、アクセシビリティ機能を手動でテストできます。Codespace に移動してターミナルを開き、次のコマンドを実行します (`<branch-name>` は Copilot が作成したブランチ名に置き換えます)。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    次にアプリケーションを起動し、ブラウザーでハイコントラストとライトモードの切り替えをテストします。ページを再読み込みしても設定が維持されることを確認します。

アクセシビリティ用のカスタムエージェントが、アクセシビリティのベストプラクティスに従って Copilot の機能実装を支援したことを確認できました。アクセシビリティに懸念や改善点があれば、前の PR と同様にコメントで **@copilot** に更新を依頼できます。

## 任意の演習 — 委任を続ける

Cloud agent は、バックログの実際のタスクを任せて別の作業に集中するときに役立ちます。リポジトリに Issue をいくつか作成して、Copilot に割り当ててみましょう。次のようなアイデアがあります。

- ゲーム詳細ページに支援者向けの関心フォームを作成する。
- ゲーム一覧ページにページネーションを実装する。
- データアクセスヘルパーに入力検証とエラー処理を追加する。

## まとめ

Cloud agent ハーネスを完了しました。このレッスンでは、次の内容を学びました。

- **このリポジトリにあるカスタム指示ファイルを確認**し、その指示が cloud agent の成果にどう反映されるかを確認しました。
- **Issue を Copilot cloud agent に割り当て**、環境のセットアップ、計画、非同期の実行を確認しました。
- **カスタムエージェントを作成して使用**し、ハイコントラストとライトモードの切り替えを追加しました。
- **Copilot agents ページをミッションコントロールとして使用**し、実行中のアクセシビリティセッションを監視して誘導しました。
- **cloud agent の pull request をレビューして反復**し、`@copilot` に変更を依頼してワークフローを承認しました。

## 振り返りと次のステップ

Cloud agent ハーネスを完了しました。他のハーネスで学ぶ内容も役立ちます。

- 🖥️ **[VS Code ハーネス](../../vscode/)** — IDE で Copilot Chat の agent mode と MCP 統合を体験します。
- 💻 **[CLI ハーネス](../../cli/)** — Plan と Autopilot モード、エージェントスキル、カスタム QA エージェント、Playwright MCP、Agent Merge、オプションの `/delegate` による cloud agent への連携を使い、ターミナルからレビュー済みの変更を 2 つ届けます。

自分のリポジトリでは、次のアイデアを試してください。

- リファクタリングやテストカバレッジの Issue を cloud agent に割り当てる。
- 別の分野向けのカスタムエージェントを作成する。
- 別の技術スタック向けに `copilot-setup-steps.yml` を設定する。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot エージェントについて][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップワークフローに関するベストプラクティス][cloud-agent-best-practices]
- [Copilot cloud agent のファイアウォールを設定する][agent-firewall]

---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
