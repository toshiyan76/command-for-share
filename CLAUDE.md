# AI-DLC and Spec-Driven Development

Kiro-style Spec Driven Development implementation on AI-DLC (AI Development Life Cycle)

## Project Context

### Paths
- Steering: `.kiro/steering/`
- Specs: `.kiro/specs/`

### Steering vs Specification

**Steering** (`.kiro/steering/`) - Guide AI with project-wide rules and context
**Specs** (`.kiro/specs/`) - Formalize development process for individual features

### Active Specifications
- Check `.kiro/specs/` for active specifications
- Use `/kiro:spec-status [feature-name]` to check progress

## Development Guidelines
- Think in English, but generate responses in Japanese (思考は英語、回答の生成は日本語で行うように)

## Minimal Workflow
- Phase 0 (optional): `/kiro:steering`, `/kiro:steering-custom`
- Phase 1 (Specification):
  - `/kiro:spec-init "description"`
  - `/kiro:spec-requirements {feature}`
  - `/kiro:validate-gap {feature}` (optional: for existing codebase)
  - `/kiro:spec-design {feature} [-y]`
  - `/kiro:validate-design {feature}` (optional: design review)
  - `/kiro:spec-tasks {feature} [-y]`
- Phase 2 (Implementation): `/kiro:spec-impl {feature} [tasks]`
  - `/kiro:validate-impl {feature}` (optional: after implementation)
- Progress check: `/kiro:spec-status {feature}` (use anytime)

## Development Rules
- 3-phase approval workflow: Requirements → Design → Tasks → Implementation
- Human review required each phase; use `-y` only for intentional fast-track
- Keep steering current and verify alignment with `/kiro:spec-status`

## Steering Configuration
- Load entire `.kiro/steering/` as project memory
- Default files: `product.md`, `tech.md`, `structure.md`
- Custom files are supported (managed via `/kiro:steering-custom`)

# Repository Guidelines
## プロジェクト構造とモジュール編成
このモノレポは Next.js 製フロントエンドの `frontend/`、API ルートをまとめた `backend/`、仕様ドキュメントの `docs/`に分割されています。`frontend/src/` では画面は `app/`、共通 UI は `components/`、状態やデータ取得は `hooks/`、ユーティリティは `lib/`、型定義は `types/` に配置します。バックエンドは `src/app/api/**` に各種ハンドラー、サービス層は `src/lib/`、共通スキーマは `src/lib/validation` に置きます。静的アセットは各パッケージの `public/` に集約し、API テストは `backend/__tests__/api/<feature>/name.test.ts` に追加します。

## コーディングスタイルと命名規則
全体を TypeScript (.ts/.tsx) で統一し、React は関数コンポーネント前提・コンポーネント名はパスカルケース、ユーティリティはキャメルケースに揃えます。`frontend/eslint.config.mjs` と `backend/eslint.config.mjs` は `next/core-web-vitals` を継承しているため、`npm run lint -- --fix` で警告を解消してください。JSX ではシングルクォートと 2 スペースインデントを基準とし、Tailwind CSS のユーティリティクラスを優先的に使用します。再利用ロジックは `lib/` 配下にまとめ、公開シンボルとファイル名を一致させます（例: `useAuthStore` → `use-auth-store.ts`）。

## テストガイドライン
Jest を用いて `backend/__tests__` にテストを追加し、API ルート構成を模倣したディレクトリに配置します（例: `/api/auth/login.test.ts`）。スイート名は対象ルートやサービスに合わせ、`describe` でエンドポイント単位の振る舞いを明記してください。ローカルで `npm test` を実行し、`npm run test:coverage` で既存水準（目安 80% 以上）を維持します。フロントエンドの振る舞いテストは NextAuth フローに重点を置き、必要に応じて Supertest のフィクスチャで API をスタブします。

## コミュニケーション方針
すべてのコミュニケーションは日本語で記述してください。

## ドキュメント管理
全体の構成や設計に変更が入った場合は`docs/`を更新してください。
`add-feat/`は追加仕様のためのテンポラリードキュメントです。