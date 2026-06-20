# Onyx 日本語化パッチの push / 適用手順

この zip には、以下を同梱しています。

- `0001-feat-web-add-Japanese-UI-localization-draft.patch`
- `onyx-ja-i18n-patch.bundle`
- `onyx_ja_i18n_patch_draft.md`
- `PUSH_INSTRUCTIONS.md`

## この配布リポジトリを GitHub に push する

この zip を展開したフォルダーを、配布用リポジトリ `tag2001/onyx_ja_i18n_patch.git` として公開する場合の手順です。

```bash
git init
git branch -M main
git remote add origin https://github.com/tag2001/onyx_ja_i18n_patch.git
git add README.md .gitignore 0001-feat-web-add-Japanese-UI-localization-draft.patch onyx-ja-i18n-patch.bundle onyx_ja_i18n_patch_draft.md PUSH_INSTRUCTIONS.md
git commit -m "Add Onyx Japanese i18n patch package"
git push -u origin main
```

zip 本体は `.gitignore` で除外しています。GitHub には zip の中身だけを置く想定です。

## 推奨手順: Onyx 本体に patch を適用する

Onyx 本体を clone して patch を適用します。

```bash
git clone https://github.com/onyx-dot-app/onyx.git
cd onyx
git checkout b70008df6b538c40e2a44414d751e73494a0ab3e
git switch -c ja-i18n
git am /path/to/0001-feat-web-add-Japanese-UI-localization-draft.patch
```

## サーバ上の Onyx に安全に適用する手順

ZIP を同じフォルダーに置いて展開するだけでは、日本語化は反映されません。Onyx 本体の Git リポジトリ直下で `git am` を実行し、patch を適用します。

Onyx 本体が `/home/ubuntuuser/onyx-dot-app/` に clone されており、その直下に `web`、`backend`、`deployment` が見えている場合:

```bash
cd /home/ubuntuuser/onyx-dot-app
unzip onyx_ja_i18n_transfer_20260610.zip -d onyx_ja_patch

git switch -c ja-i18n
git am onyx_ja_patch/0001-feat-web-add-Japanese-UI-localization-draft.patch
```

Onyx 本体が `/home/ubuntuuser/onyx-dot-app/onyx/` のように 1 階層下に clone されている場合:

```bash
cd /home/ubuntuuser/onyx-dot-app
unzip onyx_ja_i18n_transfer_20260610.zip -d onyx_ja_patch

cd /home/ubuntuuser/onyx-dot-app/onyx
git switch -c ja-i18n
git am ../onyx_ja_patch/0001-feat-web-add-Japanese-UI-localization-draft.patch
```

正しい場所かどうかは、`git am` の前に以下で確認してください。

```bash
ls
```

以下のように見えれば Onyx 本体の直下です。

```text
backend
deployment
docs
web
README.md
package.json
```

適用後の確認:

```bash
git status
git log -1 --oneline
```

`git log -1 --oneline` に以下が表示されれば成功です。

```text
135405e4e2 feat(web): add Japanese UI localization draft
```

## bundle から取り込む方法

patch 適用ではなく、こちらで作成済みの commit を bundle から取り込む場合の手順です。

```bash
git clone https://github.com/onyx-dot-app/onyx.git
cd onyx
git fetch /path/to/onyx-ja-i18n-patch.bundle codex/ja-i18n-patch:ja-i18n
git switch ja-i18n
```

## 適用後の確認

```bash
cd web
npm run build
cd ..
git diff --check HEAD~1 HEAD
```

## 元コミット情報

- base commit: `b70008df6b538c40e2a44414d751e73494a0ab3e`
- patch commit: `135405e4e2 feat(web): add Japanese UI localization draft`
- source branch: `codex/ja-i18n-patch`

## 注意

- `node_modules` や `.next` は同梱していません。
- `onyx_ja_i18n_patch_draft.md` は説明用の文書です。実際のリポジトリ内にも `docs/onyx_ja_i18n_patch_draft.md` として patch に含まれています。
- GitHub push には `tag2001/onyx_ja_i18n_patch.git` への write 権限が必要です。
- `/path/to/...` や `/to/...` は説明用の表記です。その名前のフォルダーを作る必要はありません。
- `git am` は必ず Onyx 本体の Git リポジトリ直下で実行してください。
