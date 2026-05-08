# AI Agents

## 1. 目的

このファイルは AI エージェント（Cursor, Claude Code, GitHub Copilot, Devin 等）が本リポジトリ（Zenn 記事・書籍管理）のコンテキストを迅速に理解し、既存のスタイルや規約に沿った執筆・メンテナンスを支援するための指示書です。

## 2. プロジェクト概要

このリポジトリは、技術情報共有サービス [Zenn](https://zenn.dev) に投稿する記事（Articles）および本（Books）を管理するためのものです。
主にクラウドインフラ、Web3、モダンなフロントエンド/バックエンド開発に関する知見をアウトプットしています。

## 3. 主要なトピックと技術スタック

既存の記事・書籍に基づき、以下の技術領域がプロジェクトの中心です。

- **Infrastructure & DevOps**:
  - AWS (App Runner, S3, CloudFront, ECR, Route53)
  - Terraform (Terraform Cloud)
  - Google Cloud Platform
- **Web3 & Blockchain**:
  - XRPL (XRP Ledger), XUMM (Xaman)
  - Aleph Zero
- **Frontend**:
  - Next.js (App Router), React, Astro, Vite, Tailwind CSS
- **Backend & Tooling**:
  - Ruby on Rails (Albaシリアライザ), FastAPI
  - Google Apps Script (GAS)
- **Identity**:
  - Auth0, Auth.js (旧 Next-auth)

## 4. ディレクトリ構成

- `articles/`: 個別の技術記事。ファイル名は `<slug>.md`。
- `books/`: 技術書。
  - 各ディレクトリ（例: `books/nextjs-auth0/`）に `config.yaml` と各チャプターの `.md` ファイルを配置。
- `package.json`: `zenn-cli` の依存関係を管理。

## 5. 運用コマンド (Zenn CLI)

AI エージェントは以下のコマンドを理解し、必要に応じて提案してください。

- **プレビュー起動**: `npx zenn preview`
- **新規記事作成**: `npx zenn new:article --slug <slug_name>`
- **新規本作成**: `npx zenn new:book`

## 6. 執筆ガイドライン

- **フロントマター**:
  - 記事の先頭には必ず YAML 形式のメタデータ（`title`, `emoji`, `type`, `topics`, `published`）を含める。
- **言語**: 日本語（Japanese）。
- **トーン**:
  - 簡潔で実用的なコード例を含む「技術チュートリアル」のトーンを維持。
  - 読者が詰まりやすい「コンソール操作と実態の差分」や「ハマりポイント」を明記する。
- **画像**:
  - `storage.googleapis.com/zenn-user-upload/` からの外部参照画像が多用されている。
- **コード規約**:
  - Next.js の記事では App Router を優先。
  - AWS 関連では Terraform を用いた IaC 手順を好む。

## 7. 注意事項

- `published: true` で保存すると、GitHub 連携により Zenn 上に公開されるため、下書き段階では `false` を維持すること。
- 既存の slug や ID (例: `5624cbde6ce7b1.md`) を変更すると、Zenn 上の URL が変わり SEO やリンクに影響するため注意。
