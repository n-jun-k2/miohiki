---
name: flow-development
description: 承認済み要件から Issue、設計、計画、実装、検証、独立レビュー、PR までの開発を統括する。
---

自然文または要件・設計・計画文書やタスク番号を受け取り、既存の承認と未決事項を確認する。共通ログは AGENTS.md、Git 操作は `.codex/guidelines/git-workflow.md` に従う。

1. 承認済み要件・対象範囲を確認し、`.agents/skills/wf-issue/SKILL.md` で関連 Issue を調査する。必要な設計は `.agents/skills/flow-design/SKILL.md` に従い、未承認の設計は承認後に進む。
2. `.agents/skills/wf-plan/SKILL.md` で計画と Issue 案を作り、ユーザーに実行範囲を示す。既存の承認を尊重し、未承認の計画・新規 Issue は承認後に進む。
3. 承認範囲で Issue と `.agents/skills/wf-branch/SKILL.md` のブランチを準備する。`.agents/skills/wf-test/SKILL.md` と `.agents/skills/wf-implement/SKILL.md` に従い、可能な仕様は先にテストを定義し実装する。独立した作業だけを並列にし、担当ファイルを明示する。
4. `.agents/skills/wf-verify/SKILL.md` と `.agents/skills/wf-review/SKILL.md` に従い必要な検証と実装者とは別の Agent によるレビューを行う。指摘を修正したら再検証し、原則2回まで再レビューする。
5. PR に含める必要がある報告書は commit 前に `.agents/skills/wf-report/SKILL.md` で作成する。PR 作成後の追加報告は承認範囲に応じて追送または別管理する。
6. `.agents/skills/wf-pr/SKILL.md` に従い、承認範囲内の commit、push、PR 作成後の自己レビュー、CI 状態確認を行う。ユーザーの限定指示を優先し、権限不足・重大な失敗や計画外の変更は停止して報告する。マージは人間が行う。
7. 必要な場合に `.agents/skills/wf-report/SKILL.md` を使い独立した報告書を作成する。対応要件、実装、変更ファイル、判断、検証とレビュー、未解決・未検証、Issue・SHA・PR と残タスクを報告する。未承認の要件や設計を決定済みと扱わない。
