---
name: wf-pr
description: 承認範囲の変更を検証・レビュー後にコミット、push、PR 作成し CI を確認する。
---

Issue と変更・承認範囲を確認し `.codex/guidelines/git-workflow.md` に従う。必要な検証と実装者以外の独立レビュー、完了条件、Critical・Warning、ベースとの競合を確認する。未実施や失敗を成功と扱わず、重大問題が残れば通常 PR を作らず報告する。

対象ファイルを個別に確認して登録し、差分・機密・`git diff --check` と必要なテストを確認して commit、push、PR 作成、CI の状態確認まで行う。既存の承認された範囲を超えない。Issue 対応の完了と部分対応で Closes と Refs を使い分け、結果・未解決・未検証・SHA と URL を報告し、共通ログに記録する。AI はマージしない。
