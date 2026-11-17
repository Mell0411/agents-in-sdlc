---
title: "演習 4 - カスタムエージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタムエージェントとは

GitHub Copilot の[カスタムエージェント][custom-agents-concept]を使うと、開発ワークフローの特定のタスクや分野に合わせた AI アシスタントを作成できます。リポジトリの `.github/agents` フォルダーに Markdown ファイルを作成し、特定の作業を効果的に実行するための指示、ベストプラクティス、コーディングパターン、専門知識を Copilot に伝えます。チームの知識を再利用可能なエージェントにできます。たとえば、[WCAG][wcag] 準拠を確認するアクセシビリティエージェント、安全なコーディング手法に従うセキュリティエージェント、一貫したテストパターンを維持するテストエージェントを作成できます。

カスタムエージェントは、プロジェクト内の `.github/agents` フォルダー、またはグローバルな `~/.copilot/agents` にある Markdown ファイルで定義します。各ファイルには、少なくとも `name` と `description` を含む YAML frontmatter と、エージェントの動作、専門知識、指示を記述する Markdown プロンプトが必要です。

### カスタムエージェントとエージェントスキルの違い

カスタムエージェントと[エージェントスキル][agent-skills-concept]には一部重なる部分があります。どちらも主に Markdown ファイルで定義され、AI が作業を実行する方法を伝えます。違いを簡潔に表すと、**カスタムエージェントは作業者、スキルはツール**です。

カスタムエージェントは独自のコンテキストウィンドウを持ち、スキルやほかのエージェントを使って作業を調整します。このラボでは、アクセシビリティ用のカスタムエージェントがアクセシビリティガイドラインに沿ってサイトをレビュー、更新します。その作業の一部として、pull request ワークフローやテスト実行・管理用のスキルを呼び出すこともできます。

> [!NOTE]
> カスタムエージェントの作成に唯一の正解はありません。AI のほかの使い方と同じように、環境やシナリオに合わせて試し、改善してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## シナリオ

この演習では、次の内容を学びます。

- 既存のアクセシビリティ用カスタムエージェントを確認します。
- Copilot Chat でアクセシビリティエージェントを使い、ハイコントラストモードを実装します。

Tailspin Toys は、視覚能力や好みにかかわらず、すべてのユーザーがクラウドファンディングプラットフォームを利用できるようにしたいと考えています。最近のユーザーフィードバックでは、テキストと背景色のコントラストが不十分なため、現在のダークテーマが読みにくいという意見がありました。このアクセシビリティの課題に対処するため、デザインチームはユーザーが切り替えられるハイコントラストモードの追加を依頼しました。

アクセシビリティは重要なため、できるだけ早く実装します。カスタムエージェントを使って機能を生成します。

## アクセシビリティ用カスタムエージェントを確認する

アクセシビリティ用のカスタムエージェントはすでに作成されています。Copilot の作業にどのような指針を与えるか確認しましょう。

codespace に戻り、ターミナルを開いて、アクセシビリティ作業用の新しいブランチを `main` から作成します (演習 3 のフィルター機能 PR とは分けて作業します)。

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. `.github/agents/accessibility.md` を開きます。
2. `name` と `description` フィールドを含む YAML frontmatter を確認します。

> [!CAUTION]
> カスタムエージェントには `name` と `description` を含む frontmatter が必要です。

3. 続くセクションを読み、次の内容を確認します。
   - アクセシブルな Web サイトのコード生成に関する主要な責務。
   - アクセシビリティのベストプラクティス。
   - HTML、CSS、JavaScript のコード例。
   - よくある問題と間違い。

## Copilot Chat でカスタムエージェントを使う

VS Code では、`.github/agents` に定義したすべてのカスタムエージェントを Copilot Chat ビュー下部のエージェントドロップダウンに表示します。エージェントを選択すると、そのエージェントの指示とツールに合わせたチャットセッションを開始できます。

> [!TIP]
> **Copilot Chat を開く**
>
> 次の演習を始める前に codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を開始します。モードとモデルの選択は演習によって異なります。必要な場合は各手順に記載しています。

1. Chat ビューのエージェントドロップダウンで、まだ選択されていない場合は **Agent** を選択します。

   ![Chat ビューのエージェントピッカーのスクリーンショット](../../../_images/shared-chat-mode-selector.png)

2. Chat ビュー下部にあるエージェントドロップダウンを選択します。既定では、現在のエージェントとして **default** が表示されます。
3. 利用可能なエージェントの一覧から **Accessibility agent** を選択します。
4. 次のプロンプトをアクセシビリティエージェントに送信します。

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

5. Copilot Chat がコードベースを調べて、変更を提案し、プロジェクトファイルに適用します。変更はファイルパスと差分とともにチャットに表示されます。
6. 変更が適用されると、エージェントが編集したファイルの一覧が **Files changed** に反映されます。

> [!NOTE]
> この作業には数分かかる場合があります。Copilot は実際にリポジトリを変更し、既存の Astro コンポーネント、CSS、関連テストなどを編集します。

次の演習では、この作業の進行状況を確認し、追加の指示を出します。

## まとめと次のステップ

特定のタスクや分野に合わせた AI アシスタントである、GitHub Copilot の[カスタムエージェント][custom-agents]を学びました。カスタムエージェントを使うと、チームの専門知識や標準を再利用可能な形で記録し、Copilot の作業に適用できます。

次の内容を学びました。

- カスタムエージェントの定義方法。
- Copilot Chat の agent mode でカスタムエージェントを使う方法。

次は、エージェントの作業を[監視して誘導][next-lesson]し、進行中の変更をレビューして、同じセッションにライトモードの切り替えを追加します。

## リソース

- [カスタムエージェントについて][custom-agents]
- [IDE でカスタムエージェントを作成する][creating-custom-agents-ide]
- [VS Code のカスタムエージェント][custom-agents-vscode]
- [カスタムエージェントの設定][custom-agents-config]
- [awesome-copilot のカスタムエージェント][awesome-copilot-agents]

---

| [← 前のレッスン: Playwright MCP サーバーで機能をテストする][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
