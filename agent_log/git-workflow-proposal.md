# Git・GitHub 開発ワークフロー構築案（承認済み）

2026-11-26。[setup-git-workflow.md](../prompts/setup-git-workflow.md) に基づく案。既存の [開発ワークフロー](development-workflow-proposal.md) と AGENTS.md の共通ログを再利用する。アプリ本体・Issue・PR は今回作らない。

ユーザーが本構築案を承認。以下の現状は調査時点の記録。

## 現状

- ローカルは main、HEAD `21cdcea`、origin は `n-jun-k2/miohiki`。origin/main はローカルで gone と表示。未コミット変更が複数あるため保全する。
- GitHub 連携は `n-jun-k2` で対象リポジトリへの push 権限を返した。一方、`gh auth status` は別の設定済みアカウントの認証無効を報告。Git の HTTPS push も以前認証エラー。GitHub 連携と Git/gh 認証は別。
- GitHub のリポジトリ情報は size 0、default branch main を返した。空リポジトリの可能性はあるがブランチ実体の確認は未了。初回の main 確立が必要な場合、通常の main 直接 push 禁止と衝突するため別途、具体的な初期化手順と承認を要する。勝手に保護回避や初期化はしない。
- `.github/pull_request_template.md` が未追跡で存在する。Issue テンプレートと CI は見当たらない。アプリケーションの言語・ビルド・テストは未確定。Codex CLI 0.172.0、gh 2.103.3。

## 運用案

1. 関連 Issue を確認して重複を避け、既存の承認済み要件から設計・実装計画と Issue 案を作成。新規 Issue はその内容と計画を承認後に作成する。
2. 原則リモートの最新デフォルトブランチから `{type}/{issue-number}-{short-description}` を作成。未コミット変更は保全し、並列作業は必要に応じ作業領域を分ける。main 直接コミット・push、force push、破壊的 reset/clean、AI マージは禁止。
3. 承認範囲内でテスト・実装・検証・実装者と別の独立レビューを行う。Critical は解消し、未解決 Warning や未検証は記録する。
4. 指定ファイルだけ登録し、差分・機密・`git diff --check`・必要なテストを確認して Conventional Commits でコミット。push 後、Issue・検証・レビュー・残課題を含む PR を作成し、CI 状態を確認する。Issue が完了する PR のみ Closes、部分対応は Refs。人間が最終承認・マージ。
5. 計画と Issue 案の承認が、明示された範囲のブランチ・コミット・push・PR までを含むと説明し、その範囲の操作を連続して進める。既存 `flow-development` の「Git 操作はユーザー指示」より広い運用となるため、その差を明文化する。仕様変更、範囲外変更、重大な不具合・競合や権限不足は停止して報告。ユーザーが限定した指示は優先し、リモート設定変更や第三者への通知の権限には拡張しない。

## 設定・ファイルの変更案

| パス | 内容 |
| --- | --- |
| `.codex/guidelines/git-workflow.md` | ブランチ・Issue・コミット・PR・禁止事項を一箇所に集約。AGENTS と Skill は参照のみ |
| `AGENTS.md` | Git 共通ルールへの参照を追加 |
| `.agents/skills/wf-issue/SKILL.md` | Issue 調査・重複確認、起票案、承認後作成 |
| `.agents/skills/wf-branch/SKILL.md` | 作業ツリー・最新ベース確認、命名、承認範囲でのブランチ作成 |
| `.agents/skills/wf-pr/SKILL.md` | 検証・独立レビューの確認、対象限定の commit/push、PR、CI 状態報告 |
| `.agents/skills/flow-development/SKILL.md` | 既存の設計・計画・実装フローに上記3工程と承認範囲を統合 |
| `.github/ISSUE_TEMPLATE/` | 要件・スコープ・完了条件・テスト・依存と未決事項を記す最小テンプレート |
| `.github/pull_request_template.md` | 既存未追跡ファイルを尊重し、必要な不足項目だけ加える。サンプル `Closes #12` は誤リンク防止の案内に変更 |
| `.github/workflows/` | PR の差分の基本検査を行う最小 CI を検討。アプリのビルド・テストが未定のためそれらの成功を偽装しない。実装後に実際のコマンドを追加 |
| `README.md`、`agent_log/` | 運用・認証の境界と現状、検証結果を記録 |

Agent は既存の worker、evaluation_specialist、critical_reviewer とメインを使う。Git/PR の準備はメインが担当し、現時点で release 専任 Agent を増やさない。利用環境で GitHub plugin を利用可能なら権限に応じて利用する。

## リモート設定の提案（今回は変更しない）

main への PR 必須、必要な CI チェック、人間のレビュー承認、force push とブランチ削除の禁止を提案する。利用可能な GitHub プラン・ルールセットと CI の実体を確認し、具体的な設定内容を別途提示して承認後に変更する。

## 確認方法

Skill 形式とリンク、テンプレートの形式、CI の構文・条件を確認する。一時作業領域でブランチ名・変更範囲・差分検査、Issue/PR 案の作成と失敗時停止を外部書き込みなしで試す。リモート Issue、PR、push、ブランチ保護は操作しない。認証、空リモート初期化、CI の GitHub 上での実行、新規 Skill の自動発見は未検証として報告する。

## 検証結果（2026-11-26）

追加・更新した Skill 4 件は形式検証を通過。CI YAML の構文、Issue/PR テンプレートの主な項目、ブランチ名の例を確認。一時 Git リポジトリで CI と同じ差分検査を実行し、正常差分の成功と末尾空白の失敗を確認した。Issue/PR の実起票や一連の Skill 実行、権限不足時の自動停止、GitHub 上の CI、Skill の自動発見は未検証。`actionlint` は環境にないため実行していない。
