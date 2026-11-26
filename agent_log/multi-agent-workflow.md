# マルチエージェントの検討環境

ユーザーが承認した構成に基づき、6つの専門 Agent と議論・開発用 Skill を用意する。役割は `.codex/agents/`、手順は `.agents/skills/` を参照する。実装担当には利用環境で提供される汎用 `worker` を使う。

## 使い方

- `$flow-requirements` に自然文や既存 Markdown のパスを渡して要件を整理する。
- `$flow-discussion` に議題や既存 Markdown のパスを渡して複数の専門家と議論する。
- `$flow-design` に承認済み要件やその Markdown のパスを渡して設計する。
- `$flow-development` で承認済み要件から設計・計画・実装・検証・レビューを進める。`$wf-plan`、`$wf-implement`、`$wf-test`、`$wf-verify`、`$wf-review` は各工程を単独で扱う。
- `$wf-issue`、`$wf-branch`、`$wf-pr` は Git・GitHub の各工程を扱う。共通ルールは `.codex/guidelines/git-workflow.md` を参照する。
- `$wf-report` に調査・議論・評価・進捗の資料を渡して報告書を作成する。必要に応じ `project_secretary` に報告書の編集範囲を指定する。

例: `$flow-discussion Detection と SOT の BBOX の採用方針を比較してください`。Skill の発見はセッション開始時の設定に依存するため、追加後は新しい Codex セッションを開始する。実際にサブエージェントを起動できない場合は、未実施として報告する。

## 成果物

議論ごとに `worklog/discussions/<日時>-<議題>/` に議題、各ラウンド、統合結果、レビュー、未決事項を記録する。議論を担当する専門 Agent はファイルを編集せず、メインが集約する。開発時は別の汎用 `worker` に担当ファイルを明示して実装を依頼できる。詳細は [開発段階の方針](development-agent-policy.md) を参照する。`agent_log/requirements.md` は現在の方針の索引として維持し、ユーザーが承認した要件は `docs/requirements/`、設計は `docs/architecture/`、技術調査は `docs/research/`、承認済みの判断は `docs/decisions/` に必要に応じて記録する。提案は承認前に承認済み資料へ移さない。

サブエージェントの権限は親セッションに従う。議論担当の編集禁止は維持し、`project_secretary` は指定された報告書のみ編集する。Agent 定義中の制約は行動指示であり、独立したサンドボックスではない。モデルや接続、権限、同時実行数は利用環境に依存する。

## 検証記録（2026-11-25）

Codex CLI 0.172.0 で TOML の構文と3つの Skill の形式を検証した。CLI の厳格設定モードで2役のサブエージェント起動を試行し、結果を得た。さらに一時作業領域で `flow-discussion` を自然文の議題に適用し、3ラウンド、最終レビュー、7つの議論ファイルの作成を確認した。サンプルの結論は本プロジェクトの決定ではない。Markdown パス入力による実行、残る2つの Skill の一連の実行は未検証。

2026-11-26 に開発用 Skill 6 件の形式と TOML 7 件の構文を確認した。汎用 worker に一時領域で小さな関数の実装・確認を依頼し、別の critical_reviewer が独立レビューと追加検証を行った。これは Agent の委譲・報告の限定的な確認であり、開発用 Skill の自動発見や設計承認から完了までの一連の実行は未検証。[作業ログ](../worklog/20261126_105115_development_workflow.txt)

2026-11-26 に報告ワークフローの TOML・Skill の形式を確認した。`project_secretary` は追加前から継続中のセッションで起動を試みたが、未認識のため架空報告書のサンプルは未実施。新セッションでの発見・起動と報告書作成は未検証。[作業ログ](../worklog/20261126_220518_reporting_workflow.txt)
