# Agentic Development

AIエージェントを活用したソフトウェア開発環境の構築・運用に関する指示書、開発規約、ワークフローを管理するリポジトリ。

主にCodex CLIを対象とし、特定のプロジェクトや技術スタックに依存しない、再利用可能な開発基盤の構築を目的とする。

## Objectives

- AIエージェントを活用した開発プロセスの標準化
- プロジェクトごとのAgent・Skill構築の効率化
- 複数の専門エージェントによる技術的な意思決定の支援
- 開発過程および設計判断のトレーサビリティの確保
- 開発ノウハウの蓄積と再利用
- AIエージェント開発環境の継続的な改善

## Design Principles

- Human-in-the-loop：重要な意思決定は人間が行う。
- Separation of Concerns：Agent、Skill、Ruleの責務を分離する。
- Multi-Agent Collaboration：必要に応じて複数の専門エージェントを活用する。
- Evidence-Based Decision：事実と根拠に基づいて判断する。
- Traceability：依頼、議論、判断、成果物を追跡可能にする。
- Reusability：特定のプロジェクトに依存しない指示書を管理する。
- Simplicity：必要以上に複雑な仕組みを導入しない。
- Progressive Enhancement：開発段階や課題に応じて段階的に拡張する。

## Directory Structure

```text
agentic-development/
├── README.md
└── prompts/
    ├── setup-multi-agent.md
    └── setup-worklog.md
```

## Prompts

| 指示書 | 概要 |
|---|---|
| [setup-multi-agent.md](prompts/setup-multi-agent.md) | プロジェクトに適したAgent・Skill・マルチエージェント議論環境を構築する |
| [setup-worklog.md](prompts/setup-worklog.md) | ユーザーおよびエージェント間の依頼・回答・議論履歴を保存する仕組みを構築する |

## Getting Started

対象プロジェクトのルートディレクトリでCodex CLIを起動する。

```bash
cd /path/to/project
codex
```

適用したい指示書のパスを指定して、Codexに構築を依頼する。

```text
以下の指示書を読み込み、
現在のプロジェクトに適用してください。

- /path/to/agentic-development/prompts/setup-multi-agent.md
- /path/to/agentic-development/prompts/setup-worklog.md

既存のプロジェクト構成と規約を尊重し、
まず構築方針を提示してください。
```

指示書は必要なものだけを選択して適用する。

## Development Workflow

以下を基本的な開発フローとする。

1. プロジェクトの目的・背景の整理
2. AIエージェント開発環境の構築
3. 要件定義
4. 技術調査・技術選定
5. アーキテクチャ設計
6. 実装計画・タスク分解
7. 実装
8. テスト・検証
9. レビュー
10. リリース・運用

必要に応じて、各工程に専門エージェントやSkillを追加する。

すべての工程を自動化することを目的とせず、プロジェクトの規模、開発段階、要求品質に応じて適用する。

## Worklog

ユーザーからの依頼、エージェントへの指示、回答、議論、決定事項を記録する。

保存形式：

```text
worklog/YYYYMMDD_HHMMSS_{topic}.txt
```

原則として1議題につき1ファイルとし、同じ議題の継続時は既存ファイルに追記する。

詳細な運用規約はsetup-worklog.mdを参照する。

## Maintenance

- 実際のプロジェクトで得られた知見を指示書へ反映する。
- プロジェクト固有の要件や規約は各プロジェクトで管理する。
- 共通化できる開発ノウハウのみを本リポジトリに追加する。
- Codexの仕様変更に応じて指示書を更新する。
- 不要なAgent、Skill、ルールの増加を避ける。
- 既存の指示書との責務の重複を避ける。

## Roadmap

- [x] マルチエージェント開発環境の構築指示書
- [x] 作業ログ管理の構築指示書
- [ ] プロジェクト初期構築
- [ ] 要件定義
- [ ] 技術調査
- [ ] アーキテクチャ設計
- [ ] 実装計画・タスク分解
- [ ] Issue駆動開発
- [ ] テスト・品質保証
- [ ] コードレビュー
- [ ] PR作成・管理
- [ ] CI/CD構築