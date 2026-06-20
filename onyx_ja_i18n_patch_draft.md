# Onyx 日本語化パッチドラフト

作成日: 2026-06-10

対象リポジトリ: `onyx-dot-app/onyx`

ローカル作業ディレクトリ: `C:\Users\bttaguchihj\Documents\Codex\2026-06-10\Onyx`

対象コミット: `b70008df6b538c40e2a44414d751e73494a0ab3e`

対象 Web パッケージ: `web@1.0.0-dev`

主要フロントエンドバージョン: Next.js `16.2.6`, React `19.2.4`

## 目的

Onyx の主要 UI を日本語で利用できるようにするためのパッチドラフトです。

今回のドラフトでは、以前の作業で発生したエラーを踏まえ、単純な文字列置換だけではなく、ビルド確認とブラウザ確認を行いながら修正範囲を再整理しています。

## 今回の到達点

- 日本語 locale の基盤を追加
- 認証系 UI を日本語化
- 共通エラー画面、グローバルエラー、OAuth コールバック画面を日本語化
- サイドバー、アカウントメニュー、チャット検索周辺を日本語化
- MCP / OpenAPI アクション管理画面を日本語化
- MCP / OpenAPI の追加モーダル、認証設定モーダルを日本語化
- 管理画面のルート名、サイドバー見出し、主要ボタンを日本語化
- `npm run build` と `git diff --check` が成功することを確認

## バージョン更新時の効率的な確認観点

Onyx は UI コンポーネントや管理画面の構成が更新で変わる可能性があります。そのため、次回以降は「ファイル名だけ」ではなく「画面要素の種類」で確認すると効率的です。

### 最優先で確認する要素

- 画面タイトル: ページヘッダー、モーダルタイトル、空状態カードのタイトル
- 主要説明文: ページヘッダー説明、モーダル説明、フォーム補足文
- 主要操作ボタン: 追加、保存、接続、認証、キャンセル、削除、切断
- フォームラベル: 入力欄名、任意/必須表示、placeholder、helper text
- 空状態: データ未登録時の説明、追加導線
- エラー表示: global error、backend unavailable、OAuth callback error、validation error
- サイドバー: セクション見出し、ナビゲーション項目、検索 placeholder、アカウントメニュー
- 認証フロー: login、signup、forgot/reset password、verify email、waiting verification
- OAuth / MCP / OpenAPI callback: loading、success、failure、redirect 関連文言
- toast / notification: 成功、失敗、削除、接続、保存、更新

### バージョン更新で変わりやすい要素

- 管理画面のページ構成とルート名
- MCP / OpenAPI のカード表示、認証方式、フォーム項目
- エージェント、コネクタ、モデル設定などの管理画面本文
- Opal UI コンポーネント由来の共通ラベル
- API から返る description、tool name、connector name などのデータ由来テキスト

これらはコード検索だけでは取りこぼしやすいため、実 backend または mock backend でログイン済み画面を開き、DOM の可視テキストを確認するのが有効です。

### 変更可能性が低く、ファイル単位で追いやすい要素

以下は固定メッセージ中心のため、バージョン更新後も同じファイルを見れば確認しやすい箇所です。

- `web/src/components/errorPages/ErrorPage.tsx`
- `web/src/app/global-error.tsx`
- `web/src/components/auth/AuthFlowContainer.tsx`
- `web/src/components/oauth/OAuthCallbackPage.tsx`
- `web/src/app/oauth-config/callback/page.tsx`
- `web/src/app/mcp/oauth/callback/page.tsx`
- `web/src/app/federated/oauth/callback/page.tsx`
- `web/src/app/craft/v1/apps/oauth/callback/page.tsx`
- `web/src/sections/AppHealthBanner.tsx`
- `web/src/sections/LicenseExpiryBanner.tsx`

## 現在の主な変更内容

### 日本語化基盤

- `web/src/lib/i18n.ts`
- `web/src/providers/LocaleProvider.tsx`
- `web/src/components/LanguageSwitcher.tsx`
- `web/src/app/layout.tsx`

locale を保持し、`ja` / `en` の表示切替ができる基盤を追加しています。Cookie と localStorage を使い、画面遷移後も locale を維持します。

### 認証系 UI

- `web/src/app/auth/login/**`
- `web/src/app/auth/signup/**`
- `web/src/app/auth/create-account/page.tsx`
- `web/src/app/auth/forgot-password/page.tsx`
- `web/src/app/auth/reset-password/page.tsx`
- `web/src/app/auth/verify-email/Verify.tsx`
- `web/src/app/auth/waiting-on-verification/**`
- `web/src/app/auth/error/AuthErrorContent.tsx`

ログイン、サインアップ、メール確認、パスワードリセット、認証エラー周辺の表示文言を日本語化しています。

### 共通エラーと OAuth コールバック

- `web/src/components/errorPages/ErrorPage.tsx`
- `web/src/components/auth/AuthFlowContainer.tsx`
- `web/src/app/global-error.tsx`
- `web/src/components/oauth/OAuthCallbackPage.tsx`
- `web/src/app/oauth-config/callback/page.tsx`
- `web/src/app/mcp/oauth/callback/page.tsx`
- `web/src/app/federated/oauth/callback/page.tsx`
- `web/src/app/craft/v1/apps/oauth/callback/page.tsx`

backend 未起動時の fallback、予期しないエラー、OAuth 成功/失敗/リダイレクト中の文言を日本語化しています。

### サイドバーと管理メニュー

- `web/src/lib/admin-routes.ts`
- `web/src/sections/sidebar/AdminSidebar.tsx`
- `web/src/sections/sidebar/AppSidebar.tsx`
- `web/src/sections/sidebar/AccountPopover.tsx`
- `web/src/sections/sidebar/ChatButton.tsx`
- `web/src/sections/sidebar/ChatSearchCommandMenu.tsx`
- `web/src/sections/sidebar/sidebarUtils.ts`

管理画面ルート名、サイドバーセクション、検索、チャット操作、アカウントメニュー周辺の表示を日本語化しています。

### MCP / OpenAPI アクション管理

- `web/src/app/admin/actions/mcp/page.tsx`
- `web/src/app/admin/actions/open-api/page.tsx`
- `web/src/sections/actions/MCPPageContent.tsx`
- `web/src/sections/actions/OpenApiPageContent.tsx`
- `web/src/sections/actions/MCPActionCard.tsx`
- `web/src/sections/actions/OpenApiActionCard.tsx`
- `web/src/sections/actions/ActionCardHeader.tsx`
- `web/src/sections/actions/PerUserAuthConfig.tsx`
- `web/src/sections/actions/modals/AddMCPServerModal.tsx`
- `web/src/sections/actions/modals/AddOpenAPIActionModal.tsx`
- `web/src/sections/actions/modals/MCPAuthenticationModal.tsx`
- `web/src/sections/actions/modals/OpenAPIAuthenticationModal.tsx`
- `web/src/sections/actions/modals/DisconnectEntityModal.tsx`

MCP / OpenAPI の一覧、カード、追加モーダル、認証設定モーダル、切断確認モーダルを日本語化しています。

## ブラウザ確認済み範囲

mock backend を用いて管理者ログイン済み相当の状態を作り、以下を確認しました。

- `/admin/actions/mcp`
- MCP 追加モーダル
- MCP 認証モーダル
- `/admin/actions/open-api`
- OpenAPI 追加モーダル
- OpenAPI 認証モーダル
- backend 未起動時の `/auth/login`, `/auth/signup`, `/admin/actions` fallback 表示

mock データ由来の `Mock MCP Server`, `Weather API`, `Weather lookup action` などは翻訳対象外として扱っています。

## 確認コマンド

`web` ディレクトリで以下を実行済みです。

```powershell
npm run build
```

結果: 成功

リポジトリルートで以下を実行済みです。

```powershell
git diff --check
```

結果: 成功

## 安全なパッチ適用手順

パッチは「ZIP を同じフォルダーに置くだけ」では反映されません。Onyx 本体の Git リポジトリ直下で `git am` を実行し、パッチ内の差分をソースへ適用します。

### 前提

Onyx 本体の直下で `ls` を実行したとき、以下のようなディレクトリが見えていることを確認します。

```bash
backend
deployment
docs
web
README.md
package.json
```

たとえば Onyx 本体が `/home/ubuntuuser/onyx-dot-app/` に clone されている場合は、以下のように進めます。

```bash
cd /home/ubuntuuser/onyx-dot-app
unzip onyx_ja_i18n_transfer_20260610.zip -d onyx_ja_patch

git switch -c ja-i18n
git am onyx_ja_patch/0001-feat-web-add-Japanese-UI-localization-draft.patch
```

Onyx 本体が `/home/ubuntuuser/onyx-dot-app/onyx/` のように 1 階層下に clone されている場合は、以下のようにします。

```bash
cd /home/ubuntuuser/onyx-dot-app
unzip onyx_ja_i18n_transfer_20260610.zip -d onyx_ja_patch

cd /home/ubuntuuser/onyx-dot-app/onyx
git switch -c ja-i18n
git am ../onyx_ja_patch/0001-feat-web-add-Japanese-UI-localization-draft.patch
```

### 適用後の確認

```bash
git status
git log -1 --oneline
```

`git log -1 --oneline` に以下のようなコミットが表示されれば、パッチ適用は成功です。

```text
135405e4e2 feat(web): add Japanese UI localization draft
```

### 注意点

- `/path/to/...` や `/to/...` というフォルダーを作る必要はありません。これは「実際のファイルパスに置き換える」という説明用の表記です。
- ZIP は Onyx 本体と同じ場所に置いて構いませんが、`unzip -d onyx_ja_patch` のように専用フォルダーへ展開してください。
- `git am` は必ず Onyx 本体の Git リポジトリ直下で実行してください。
- `web` や `deployment` の中で `git am` を実行しないでください。
- 既に `ja-i18n` ブランチが存在する場合は、`git switch ja-i18n` を使うか、別名のブランチを作成してください。

## 残作業

- 実 backend / DB / 管理者アカウントでの最終ブラウザ確認
- Agents、Connectors、Language Models、Chat Preferences など、MCP/OpenAPI 以外の管理画面本文の追加日本語化
- チャット実行画面、ファイル添付、検索結果、フィードバック導線の可視英語確認
- API 由来データと UI 固定文言の切り分け
- パッチファイルの書き出し

## 実 backend 付き確認で見るべき画面

優先度順に確認します。

1. `/auth/login`, `/auth/signup`, `/auth/forgot-password`
2. `/app`
3. `/app/settings`
4. `/admin/actions/mcp`
5. `/admin/actions/open-api`
6. `/admin/agents`
7. `/admin/add-connector`
8. `/admin/indexing/status`
9. `/admin/configuration/language-models`
10. `/admin/configuration/chat-preferences`

各画面では、画面タイトル、説明文、主要ボタン、フォームラベル、空状態、エラー表示、モーダル、toast を確認します。

## セキュリティ確認メモ

今回の UI 翻訳修正では、以下を確認済みです。

- OAuth callback の redirect は外部 URL へ直接飛ばないよう制御されている
- markdown 表示は既存の sanitize 経路を利用している
- 外部リンクは既存の link/button コンポーネントの `noopener noreferrer` 経路に乗る
- 翻訳文字列に `dangerouslySetInnerHTML` は追加していない

残る確認対象は、実 backend 付きでの認証済み画面、フォーム送信、toast、OAuth 接続フローです。

## 注意点

- PowerShell の表示では日本語が文字化けして見えることがあります。ビルドが通っており、ブラウザ表示で日本語が確認できる場合は、ファイル破損ではなくコンソール表示の問題である可能性が高いです。
- 技術用語の `OAuth`, `OpenAPI`, `MCP`, `Client ID`, `Dynamic Client Registration`, `SCIM` などは、原則として英語表記を残しています。
- API から返るツール名、サーバー名、説明文、コネクタ名はデータ由来のため、このパッチの固定 UI 翻訳対象外です。
- バージョン更新後は、ファイル差分だけでなく、上記の画面要素単位で再確認してください。
