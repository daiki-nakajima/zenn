---
title: "VSCode + Markdown + PDF 出力のテンプレートを作りました"
emoji: "📝"
type: "tech"
topics:
  - "markdown"
  - "vscode"
published: true
published_at: "2025-04-06 16:40"
---

技術資料やシナリオなどを Markdown で執筆する中で、  
日頃から**「PDF出力まで含めて、手軽に、見栄えよく仕上げたい」**と感じていました。  
そこで、**VSCode + Markdown + PDF出力** に対応したドキュメント制作テンプレートを作成しました。

GitHub リポジトリはこちらです。  
[daiki-nakajima/vscode-markdown-pdf-template](https://github.com/daiki-nakajima/vscode-markdown-pdf-template)

---

## こんな方におすすめ

- Markdown で技術文書・同人誌・シナリオなどを執筆したい方
- VSCode 上で執筆から PDF 出力までを完結させたい方
- テンプレートからすぐに制作を始めたい方

## テンプレートでできること

| 機能                         | 説明                                                        |
| ---------------------------- | ----------------------------------------------------------- |
| PDF一発出力                  | `Markdown PDF` 拡張で右クリック → PDF 出力                  |
| 見た目を整えた表紙付き       | HTML + CSS によってタイトル・ロゴ・日付などを自由に記載可能 |
| 改ページ／ヘッダー／フッター | `<div class="page"/>` や `footerTemplate` によって調整可能  |
| ファイル分割                 | `@[include](filename.md)` により文書を分割・統合管理可能    |
| Mermaid.js 図表対応          | フローチャートやシーケンス図を Markdown 上で記述可能        |
| 画像コピペ自動保存           | 画像を貼り付けるだけで自動保存され、パスも自動挿入          |
| MarkdownLint 対応            | `.vscode/settings.json` に整形・Lint 設定を含む             |

## プロジェクト構成

```txt
vscode-markdown-pdf-template/
├── .vscode/
│   ├── extensions.json      ← 推奨拡張一覧
│   └── settings.json        ← Markdown PDF 設定
├── docs/
│   ├── index.md             ← メイン文書
│   ├── cover.md             ← 表紙ページ（HTML）
│   ├── section1.md          ← メイン文書を構成する部分文書
│   ├── section2.md          ← 同上
│   └── section3.md          ← 同上
├── css/
│   ├── page-style.css       ← 本文スタイル
│   └── cover-style.css      ← 表紙スタイル
├── images/
│   └── section3/
│       └── section3.png     ← 文書で使用する画像
└── README.md
```

---

## 最後に

詳細はこの記事では省いているため、GitHub リポジトリの README.md をご覧ください。  
自分の制作環境を整える過程で生まれたものではありますが、Markdown で「見栄えの良い PDF 資料」を手軽に作りたい方にとって、参考になるものになっていれば幸いです。
