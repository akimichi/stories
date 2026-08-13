# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## このリポジトリについて

コードベースではなく、日本語の短編小説・散文集である。作品の実体は、リポジトリ直下に置かれた `*.md` ファイル 1 つにつき 1 作品。同名の `*.pdf` は**生成物**であり、手で編集してはならない。

## ビルド

PDF は GitHub Actions (`.github/workflows/main.yml`) が `main` への push 時に自動生成し、`Update output pdf files` というコミットで自動コミットする。したがって通常は `.md` のみを編集してコミットすればよい。

ローカルで PDF を確認したい場合:

```bash
npm ci
npx md-to-pdf --gray-matter-options=true ./*.md   # 全ファイル
npx md-to-pdf --gray-matter-options=true ./虫.md  # 単一ファイル
```

- Node は `.nvmrc` の v16.16.0 (CI も 16.x)。
- 日本語フォントが無い環境では文字化けする。CI では `fonts-ipafont fonts-ipaexfont` を導入している。
- `package.json` の `"test": "mocha"` は残骸である。mocha は依存に無く、テストも存在しない。テストを走らせようとしないこと。

## フロントマターの規約

md-to-pdf は gray-matter でフロントマターを読み、`pdf_options` をそのまま Puppeteer の PDF 出力設定として使う。

- `title` / `author` / `date` — `date` は `\the\year/\the\month/\the\day` (LaTeX 由来の名残) を使う作品と、`2017-12-11` 形式の実日付を使う「架空の症例」シリーズが混在する。既存作品に倣うこと。
- `status: plotting | maintenance | complete` — 執筆段階を表す作者独自のフィールド。出力には影響しない。
  - `plotting`: あらすじのみ (`avenge.md`, `demons-mother.md`, `okite.md`)
  - `maintenance`: 本文完成後の推敲中 (`虫.md`)
  - `complete`: 完成 (`aphorism.md`, `約束.md`, `産まれてきた娘に贈る言葉.md`)
- `pdf_options` — A4 / margin 30mm 40mm / ヘッダ (タイトル) とフッタ (ページ番号) の HTML テンプレートを持つブロック。完成に近い作品にのみ付く。新しく体裁を整えるときは `約束.md` のブロックをそのままコピーするのが定石。
- `golden-fields.md` と `heirs-to-the-throne.md` はフロントマター無しの構想メモ。

## 本文の書き方

- ルビは HTML の `<ruby>` を直接埋め込む: `<ruby>柄<rp>（</rp><rt>がら</rt><rp>）</rp></ruby>`
- 中央寄せなどの体裁も生の HTML (`<div style="text-align: center;">`) と `<br/>` で行う。
- 「架空の症例(エピソード1〜4)」シリーズは精神科の症例を題材にしたフィクションで、他の小説とは別系統。体裁を変更するときはシリーズ 4 本を揃えること。

## コミット

コミットメッセージは日本語。`Update output pdf files` は CI の自動コミットなので、人手で同じメッセージを使わないこと。
