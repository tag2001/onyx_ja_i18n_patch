# Onyx Japanese i18n Patch

Onyx (`onyx-dot-app/onyx`) の主要 UI を日本語化するための非公式パッチです。

このリポジトリは Onyx 本体の fork ではなく、Onyx 本体へ適用するための `git am` パッチと `git bundle` を配布するためのものです。

## 対象

- 対象リポジトリ: [onyx-dot-app/onyx](https://github.com/onyx-dot-app/onyx)
- パッチ作成時の base commit: `b70008df6b538c40e2a44414d751e73494a0ab3e`
- パッチ commit: `135405e4e2 feat(web): add Japanese UI localization draft`
- 作成日: 2026-06-10

Onyx 本体は更新が速いため、最新の `main` や新しい release にそのまま適用できない場合があります。その場合は、上記 base commit 付近へ適用してから差分を移植してください。

## 同梱ファイル

| ファイル | 内容 |
| --- | --- |
| `0001-feat-web-add-Japanese-UI-localization-draft.patch` | Onyx 本体へ `git am` で適用するパッチ |
| `onyx-ja-i18n-patch.bundle` | 作成済み commit を含む Git bundle |
| `onyx_ja_i18n_patch_draft.md` | 変更内容、確認済み範囲、残作業のメモ |
| `PUSH_INSTRUCTIONS.md` | GitHub へ push する場合の補足手順 |

## 主な変更内容

- 日本語 locale の基盤を追加
- 認証系 UI を日本語化
- 共通エラー画面、グローバルエラー、OAuth コールバック画面を日本語化
- サイドバー、アカウントメニュー、チャット検索周辺を日本語化
- MCP / OpenAPI アクション管理画面を日本語化
- MCP / OpenAPI の追加モーダル、認証設定モーダルを日本語化
- 管理画面のルート名、サイドバー見出し、主要ボタンを日本語化

詳細は [onyx_ja_i18n_patch_draft.md](./onyx_ja_i18n_patch_draft.md) を参照してください。

## 推奨: パッチファイルから適用する

まず Onyx 本体を clone します。

```bash
git clone https://github.com/onyx-dot-app/onyx.git
cd onyx
```

パッチ作成時の base commit に合わせる場合:

```bash
git checkout b70008df6b538c40e2a44414d751e73494a0ab3e
git switch -c ja-i18n
git am /path/to/0001-feat-web-add-Japanese-UI-localization-draft.patch
```

最新の `main` に直接試す場合:

```bash
git switch -c ja-i18n
git am /path/to/0001-feat-web-add-Japanese-UI-localization-draft.patch
```

`/path/to/...` は、このリポジトリを clone した場所に置き換えてください。

適用できたら確認します。

```bash
git status
git log -1 --oneline
```

`git log -1 --oneline` に次のような commit が表示されれば成功です。

```text
135405e4e2 feat(web): add Japanese UI localization draft
```

## bundle から取り込む

`git am` ではなく bundle からブランチを取り込むこともできます。

```bash
git clone https://github.com/onyx-dot-app/onyx.git
cd onyx
git fetch /path/to/onyx-ja-i18n-patch.bundle codex/ja-i18n-patch:ja-i18n
git switch ja-i18n
```

bundle を使う場合も、base commit が手元の履歴に存在している必要があります。

## Docker でビルドして起動する

Onyx 公式ドキュメントでは Docker Compose での起動が案内されています。通常は `deployment/docker_compose` で `docker compose up -d` を実行します。ソースからビルドする場合は `--build --force-recreate` を付けます。

日本語化パッチを反映したイメージを作る場合は、パッチ適用後の Onyx 本体で次を実行してください。

```bash
cd deployment/docker_compose
docker compose up -d --build --force-recreate
```

起動後、初期化が終わると `http://localhost:3000` でアクセスできます。

公式の最新手順は次を確認してください。

- [Onyx Quickstart](https://docs.onyx.app/deployment/getting_started/quickstart)
- [Onyx Docker deployment](https://docs.onyx.app/deployment/local/docker)

## サーバに適用してインストールする

既にサーバ上に Onyx の Git clone がある場合は、Onyx 本体のリポジトリ直下でパッチを適用します。

正しい場所かどうかは、次のようなディレクトリが見えることで確認できます。

```text
backend
deployment
docs
web
README.md
package.json
```

例:

```bash
cd /home/ubuntu/onyx
git switch -c ja-i18n
git am /home/ubuntu/onyx_ja_i18n_patch/0001-feat-web-add-Japanese-UI-localization-draft.patch
```

その後、Docker Compose で再ビルドします。

```bash
cd deployment/docker_compose
docker compose up -d --build --force-recreate
```

本番サーバで既存データを扱う場合は、先にバックアップを取得してください。

## 動作確認

パッチ適用後、Onyx 本体で次を実行します。

```bash
cd web
npm run build
cd ..
git diff --check HEAD~1 HEAD
```

このパッチ作成時点では、`npm run build` と `git diff --check` が成功しています。

## 注意点

- このパッチは非公式です。
- `node_modules` や `.next` は含めていません。
- API から返るツール名、サーバー名、説明文、コネクタ名などのデータ由来テキストは翻訳対象外です。
- 技術用語の `OAuth`, `OpenAPI`, `MCP`, `Client ID`, `Dynamic Client Registration`, `SCIM` などは原則として英語表記を残しています。
- 最新版 Onyx で conflict が出る場合は、base commit へ適用してから手動で差分を移植してください。

