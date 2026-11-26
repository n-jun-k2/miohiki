# 開発ワークフロー構築案（承認済み）

2026-11-26。入力: [setup-dev-workflow.md](../prompts/setup-dev-workflow.md)。既存の [開発 Agent 方針](development-agent-policy.md)、[議論手順](../.agents/skills/references/discussion.md)、[合意済み要件](../docs/requirements/sot-detection-evaluation.md) を再利用する。IO_ENV の要件は一部検討中で、全体の設計や実装計画は未承認。アプリケーションコード、言語・ビルド・テスト環境は現時点で未確定。Codex CLI は 0.172.0。公式の [Codex multi-agent](https://developers.openai.com/codex/multi-agent) と [skills](https://developers.openai.com/codex/skills) を確認したが、実際の設定認識と実行可能性は作成後に検証する。

## Agent の責務

| 指示書上の責務 | 実現方法 | 境界 |
| --- | --- | --- |
| project_leader | メインが計画・依存関係・完了条件を担当 | アプリケーション実装と独立した確認は別担当に依頼 |
| software_architect | 既存の同名 Agent | 読み取り専用の設計・レビュー |
| software_engineer | 既存の汎用 worker | 担当ファイルを指定し実装・必要な検証。ほかの編集を戻さない |
| test_engineer | 別タスクとして worker | 所有範囲を指定しテスト設計・作成。期待値を便宜上変更しない |
| qa_engineer | evaluation_specialist を評価設計に、実装と別の Agent を検証実行に利用 | アプリ・テストのコードは変更しない。未実施は未実施と報告 |
| code_reviewer | 既存 critical_reviewer を実装担当とは別に起動 | コード変更せず具体的な問題・根拠・重大度を報告 |

汎用 worker の名称・利用可能性は起動環境に依存。worker がない環境では同等の編集担当ロールを別途用意する必要がある。既存の algorithm_engineer、mot_researcher、requirements_analyst は必要時に利用。追加の専任ロールは現段階で設けない。既存の3同時枠設定は変更せず、実際の利用環境の制限を優先する。

## 追加する Skill の案

すべて自然文または関連文書・タスクを入力でき、単独でも使用可能。スクリプト不要。`wf-design` は同等責務を持つ既存 `flow-design` を使用する。

| 予定ファイル | 責務・出力 |
| --- | --- |
| `.agents/skills/flow-development/SKILL.md` | 要件確認→既存 flow-design→計画→テスト・実装→検証→独立レビューを統括。設計と計画は明示的な承認後に次へ進む |
| `.agents/skills/wf-plan/SKILL.md` | 承認済み設計を要件対応、順序、依存、担当範囲、検証と完了条件付きのタスクに分解。`docs/plans/` への確定保存は承認後 |
| `.agents/skills/wf-implement/SKILL.md` | 承認済み計画の担当タスクを実装し、変更・検証・未解決事項を報告 |
| `.agents/skills/wf-test/SKILL.md` | 要件・設計に対応した正常・異常・境界等の必要なテストを設計し、必要に応じて作成。実装前に仕様化可能なケースを整理 |
| `.agents/skills/wf-verify/SKILL.md` | 利用可能なビルド・静的解析・テスト・性能評価から変更に必要なものを実行。未実施と失敗を明記。原則コードを変更しない |
| `.agents/skills/wf-review/SKILL.md` | 実装者と別の Agent で要件・設計・コード・テストをレビュー。Critical / Warning / Info と根拠を記載。修正後に再検証し原則2回まで再レビュー |

各 Skill は自身の対象工程だけ実行し、承認が必要な前提のない状態では前提の不足を明示する。`flow-development` が全体を調整する。開発工程での承認要件は元指示書に従い、ユーザーが明示的に承認した範囲を再承認しない。

## ワークフロー

1. 対象の承認済み要件・スコープ・未決を確認し、必要な専門 Agent で設計する。既存 `flow-design` の独立検討・レビュー・ログ手順を使い、ユーザーが設計を承認するまで実装に進まない。
2. 承認済み設計から計画案を作り、タスクごとの要件、依存、所有ファイル、テスト、完了条件を定義。承認後に実装する。
3. 仕様として定まったテスト可能なケースは可能な限り先に定義。バグ修正は再現と失敗確認を試み、できなければ理由を記録。作業担当の所有ファイルを分ける。
4. メインが変更を統合し、必要なビルド・静的解析・テスト・回帰・性能評価を実行。独立 Agent にレビューを依頼し、修正後は再検証・再レビューする。
5. 対応要件、変更、設計判断、検証とレビュー、未解決・未検証、残タスクを報告。Git・PR 操作は後続で承認された [Git ワークフロー](git-workflow-proposal.md) の承認範囲に従う。

## 変更予定

- 新規: 上記6つの Skill 定義。独立した Agent の TOML は追加しない。
- 更新: `agent_log/development-agent-policy.md` と `agent_log/multi-agent-workflow.md` の役割・手順、`README.md` の利用方法。必要に応じ `AGENTS.md` は共通方針への参照のみ追加。既存の議論 Skill と共通ログルールは重複定義しない。
- `.codex/config.toml` と既存 Agent は原則維持。`docs/plans/` は計画承認後の実際の成果物作成時に使用する。

## 検証計画

TOML・Skill の形式、参照先、Skill の認識を確認。実際のサンプルは一時領域で、既存 worker に限定的な変更を依頼し、別のレビュー担当による指摘と検証・報告を確認する。設計や計画の承認待ちを回避する目的では使わず、失敗・未実施は明記。既存 Skill と重複した起動や変更を避ける。

## 要確認

ユーザーが本構成を承認した。Skill の認識とサンプル検証の結果は運用文書に記録する。
