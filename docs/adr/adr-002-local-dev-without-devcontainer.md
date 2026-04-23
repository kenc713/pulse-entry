# ADR-002: Local Development Without Dev Container

## Status

Accepted

## Context

**Problem:**

- pulse-entry は Tauri を使用した Windows デスクトップアプリケーション（ADR-001）
- Rust + Node.js 環境が必要
- 開発コンテナ（WSL + Docker）と直接ホスト開発のトレードオフ検討が必要

**Candidates:**

1. **開発コンテナ（WSL + Docker）**
   - メリット: 環境の統一、チーム間での再現性
   - デメリット:
     - Windows ネイティブ API テストが複雑化
     - WSL から Windows 側の UI イベント（システムトレイ、ウィンドウ通知）テストが難しい
     - ビルド時に Windows ネイティブツール連携が必須

2. **ホストOS（Windows）へのローカルセットアップ**
   - メリット:
     - ネイティブ Windows API に直接アクセス可能
     - 実装環境でそのまますぐテスト可能
     - Tauri ビルドチェーン（Rust + WebView2 + Windows SDK）が自然に連携
   - デメリット: 環境構築は手動、マシン依存性あり

## Decision

**ホストOS（Windows）へのローカルセットアップを採用する**

Tauri はネイティブ Windows 機能（システムトレイ、スケジューラ連携、Windows 通知など）を直接使用する必要があり、コンテナを経由するメリットが小さい。

単一開発者での開発であり、環境統一の必要性が低い。

## Rationale

- **Windows ネイティブテストの効率性**: ビルド・テストが直接実行でき、デバッグが迅速
- **Tauri の設計思想**: デスクトップアプリケーション開発は OS 上での直接実行を想定
- **セットアップの簡潔性**: Rust + Node.js をインストールするだけで開発開始可能
- **リアルタイムテスト**: システムトレイ、タスクスケジューラ連携など、本番環境に近い状態でテスト可能

## Consequences

**利点:**

- ビルド時間が短い（コンテナイメージ作成不要）
- Windows API テストが簡単かつ高速
- IDE（VS Code）との統合が自然（拡張機能の互換性向上）

**制約:**

- 開発環境は Windows マシンに限定
- 環境統一の仕組みがない（チームが拡大した場合は再検討）
- Rust ツールチェーン（cargo、rustc）をホス上で管理

**今後の検討:**

- チーム開発に拡大する場合は、Docker 環境を整備する可能性

## Related Decisions

- ADR-001: Use Tauri for Desktop Application
- セットアップ手順: `docs/` に `development-setup.md` で Rust + Node.js インストール手順を文書化
