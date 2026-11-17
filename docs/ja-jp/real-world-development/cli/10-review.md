---
title: "レッスン 10 - まとめと次のステップ"
description: "Copilot CLI のワークフロー、2 つの pull request、再利用可能なカスタマイズ、追加リソースを振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Tailspin Toys の一連のワークフローで GitHub Copilot CLI を使用しました。

- Codespace を準備し、Copilot CLI をインストールしてプロジェクトを確認し、フィルター機能の Issue を見つけました。
- 星評価を追加し、転送されたブラウザーで結果を確認して、最初の pull request (PR) を手動でマージしました。
- フィルター機能の Issue から開始し、Plan モードで方針を決め、Autopilot モードで構築して、Interactive モードでレビューしました。
- カスタム指示でエージェントをガイドし、既存の `quality-checks` スキルをカスタマイズして、ユニットテスト、lint、型チェックを実行しました。
- Playwright Model Context Protocol (MCP) サーバーを追加し、実際のブラウザーでフィルター機能を確認しました。
- 品質保証 (QA) 用のカスタムエージェントを作成して選択し、要件、カバレッジ、スキルの結果、ブラウザーでの証拠を評価しました。
- フィルター機能の変更全体をレビューし、Agent Merge を許可して PR をマージしました。
- コンテキスト、モデル、共有、オプションのクラウド委任に使えるスラッシュコマンドを確認しました。

## リリースした機能

このワークショップでは、更新済みの `main` を基点とするそれぞれ別のブランチで、2 つの PR を作成しました。

1. **星評価:** 既存の `starRating` と未評価の場合の状態をゲームカードに表示します。
2. **フィルター機能と品質ワークフロー:** フィルター機能を実装し、指示を更新して機能に適用し、`quality-checks` の報告をカスタマイズして、QA プロファイルと関連テストを追加します。

フィルター機能の計画から PR の作成までは、同じ会話とブランチを使いました。ワークショップを簡潔にするため、これらの作業を 1 つの PR にまとめています。

## さまざまな検証方法

自動テスト、自分で行うブラウザー確認、MCP を介した Copilot のブラウザー確認を実施しました。`quality-checks` スキルはユニットテスト、lint、型チェックを実行し、新しい形式で報告しました。QA はこれらの結果と要件・テストカバレッジのレビューをまとめ、PR の前に確認しました。

追加したテストで実際の不足を解決できていることを確認します。新しいテストが不要と判断される QA 実行もあります。マージを許可する前に、コードと検証の証拠をレビューし、変更後は影響する証拠を更新します。

## ベストプラクティス

Copilot に与えるコンテキストとツールが作業の質に影響します。このワークショップでは指示を更新し、スキルをカスタマイズし、QA プロファイルを作成し、MCP サーバーを設定しました。会話をまたいでこれらのカスタマイズを再利用し、チームのニーズに合わせて調整してください。指示は標準を設定し、スキルは繰り返し実行できるタスクを説明し、カスタムエージェントは専門的な役割を定義し、MCP サーバーは外部ツールに接続します。エージェントの要約だけでなく、実際の変更とツールの結果を確認します。

タスクに合った**モードとモデル**を選びます。**Plan** で構築前に方針を考え、**Interactive** で対象を絞った変更を確認しながら進め、範囲が明確なタスクには **Autopilot** を使います。定型的な編集には高速なモデルを、複雑な作業にはより高性能なモデルを選びます。

インフラと同じくらいコンテキストも重要です。何を、なぜ、どのように作るかを明確に説明すると、結果が大きく変わります。

## さらに確認する

主なワークフローを学びました。Copilot CLI には、次のような機能もあります。

- `/review` でコードレビューエージェントに変更を分析させる。
- `/rubber-duck` で問題を話し合い、別の視点を得る。
- `/fleet` で独立したサブタスクを並列に調整する。
- `/worktree` で別のタスクを分離する。
- `/delegate` で Copilot cloud agent にタスクを送信する。

## 次のステップ

どのツールも、使い続けることで上達します。実際のプロダクションコード、趣味のコード、何年も作りたいと思っていた小さなアプリなどで活用してください。チームと学びを共有し、お互いの経験から学びましょう。いつものように、ドキュメントも確認してください。

ターミナルから Tailspin Toys を拡張したい場合は、オプションの[Foundry Backer Concierge シリーズ][foundry]に進みます。異なる環境と比較するには、[VS Code ワークショップ][vscode]、[GitHub Copilot app ワークショップ][app]、または [Copilot cloud agent ワークショップ][cloud]を確認してください。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンドリファレンス][cli-reference]
- [Copilot CLI をカスタマイズする][customize-cli]
- [Copilot CLI で pull request を管理する][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
