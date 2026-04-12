# 共通開発ルール (Common Development Rules)

dev-apps 配下の各プロジェクトに共通する開発指針。
各プロジェクトの CLAUDE.md から共通項を抽出したもの。

---

## 改修ワークフロー

コード変更時は必ず以下の順序で進めること。

### 1. 変更内容の把握

- 関連ファイルを **必ず読んでから** 着手する
- 型定義・サービスインターフェース・状態管理の関係を確認する
- 影響範囲を特定し、変更が波及するファイルを洗い出す

### 2. 実装

- 既存の規約・パターンに従う
- 既存ファイルの編集を優先し、不要なファイル作成を避ける
- 過剰な抽象化・将来の仮定に基づく設計をしない
- 不要なリファクタは行わない

### 3. 動作確認

変更後、以下を順に実行してエラーがないことを確認する。

```bash
npm run build     # ビルド確認
npm run lint      # Lint 実行（設定がある場合）
npx tsc --noEmit  # 型チェック（TypeScript プロジェクトの場合）
```

### 4. バージョン履歴の更新検討

- 機能追加: minor (+0.1.0)
- バグ修正: patch (+0.0.1)
- 破壊的変更: major (+1.0.0)
- 軽微な修正（typo、スタイル微調整など）では更新不要
- `package.json` の `version` 更新が必要か検討し、ユーザーに確認してから実施する

### 5. コミット

- **ユーザーから明示的に依頼された場合のみ** コミットする
- 自発的にコミットしない
- `.env*` や認証情報を含むファイルをコミットしない

---

## 技術スタック共通事項

| 項目 | 標準 |
|---|---|
| 言語 | TypeScript (strict mode) |
| フレームワーク | Next.js 16 (App Router / Turbopack) + React 19 |
| スタイル | Tailwind CSS v4 または CSS Modules（プロジェクトによる） |
| UI基盤 | shadcn/ui（該当プロジェクト） |
| アイコン | lucide-react |
| バリデーション | Zod + react-hook-form |
| 状態管理 | Zustand v5 + persist / localStorage ベース（プロジェクトによる） |

---

## コーディング規約

### 命名規則

| 対象 | 規則 | 例 |
|---|---|---|
| ファイル名 | kebab-case | `expense-card.tsx` |
| コンポーネント | PascalCase | `ExpenseCard` |
| 関数・変数 | camelCase | `handleSubmit` |
| 型定義 | PascalCase, `type` 優先 | `type ExpenseItem = {...}` |

### コンポーネント

- 関数コンポーネント（default export）
- クライアントコンポーネントには `'use client'` を付与
- 機能別にディレクトリを分離（`components/features/{機能名}/`）

### プロジェクト構成の基本パターン

```
src/
├── app/              # Next.js App Router ページ
├── components/
│   ├── ui/           # 基盤 UI コンポーネント
│   ├── features/     # 機能別コンポーネント
│   └── layouts/      # レイアウト
├── lib/              # ユーティリティ・定数
├── services/         # サービス層
├── stores/           # 状態管理
├── types/            # TypeScript 型定義
└── validations/      # Zod スキーマ
```

---

## 参照元

- [finance-app/CLAUDE.md](../finance-app/CLAUDE.md)
- [yukkuri-nextjs/CLAUDE.md](../yukkuri-nextjs/CLAUDE.md)
