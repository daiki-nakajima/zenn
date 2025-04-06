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
日頃から「PDF出力まで含めて、手軽に、見栄えよく仕上げたい」と感じていました。  
そこで、**VSCode + Markdown + PDF出力** に対応したGithubテンプレートを作成しました。  
技術文書などをMarkdownで書きたい方に向けて、VSCodeのみでPDF出力まで完結できる執筆環境のテンプレートを目指しています。

GitHub リポジトリはこちらです。  
[daiki-nakajima/vscode-markdown-pdf-template](https://github.com/daiki-nakajima/vscode-markdown-pdf-template)

## こんな方におすすめ

- Markdown で技術文書・同人誌・シナリオなどを執筆したい
- VSCode 上で執筆から PDF 出力までを完結させたい
- テンプレートからすぐに制作を始めたい

## テンプレートでできること

| 機能                   | 説明                                             |
| ---------------------- | ------------------------------------------------ |
| PDF 出力               | Markdown PDF 拡張によって一発変換                |
| 目次                   | Markdown All in Oneによる目次の自動生成          |
| 改ページ指定           | `<div class="page"/>` を挿入するだけ             |
| ファイル分割執筆       | `markdown-it-include` で複数 Markdown を結合可能 |
| フローチャート図       | mermaid.js によるフローチャート図も記述可        |
| 文書フォーマット       | markdownlint 拡張で Markdown の文法チェック      |
| 画像のコピー＆ペースト | 文書内に貼り付けると自動的に画像保存             |
| カスタムデザイン       | CSSによるフォントや装飾はカスタム可能            |

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

## Markdownサンプル

```markdown
:[表紙](cover.md)

# 目次

- [目次](#目次)
- [１章](#１章)
  - [Markdown PDF 拡張機能の設定](#markdown-pdf-拡張機能の設定)
  - [Mermaid フローチャートとシーケンス図](#mermaid-フローチャートとシーケンス図)
- [２章](#２章)
  - [画像の貼り付け](#画像の貼り付け)

<div class="page"/>

# １章

## Markdown PDF 拡張機能の設定

:[セクション１](section1.md)

## Mermaid フローチャートとシーケンス図

:[セクション２](section2.md)  

<div class="page"/>

# ２章

## 画像の貼り付け

:[セクション３](section3.md)

```

## PDF完成サンプル

![image](https://storage.googleapis.com/zenn-user-upload/5a5a750d032d-20250406.png)

### 参考

- [VSCODE+MarkdownでキレイなPDFとHTMLを出力して、TRPGシナリオリリース当日まで寝て過ごす。](https://tthrr.com/article/15168c8f-2401-8194-9a60-cdc3d8df5e35#15168c8f240181e6a0fee7031e9365dd)
- [VSCode＋MarkdownでPDF資料作成する際に表紙とフッターを良い感じにした備忘録](https://qiita.com/matcha_kinako/items/5c62784db4919e048925)
- [Visual Studio Code で Markdown に画像を貼り付けられる markdown.copyFiles.destination 設定メモ 2024 年 5 月改良版](https://www.1ft-seabass.jp/memo/2024/04/26/vscode-current-markdown-copyfiles-destination-setting-ver2/)

---

## 最後に

詳細はこの記事では省いているため、GitHub リポジトリの README.md をご覧ください。  
自分の制作環境を整える過程で生まれたものではありますが、Markdown で「見栄えの良い PDF 資料」を手軽に作りたい方にとって、参考になるものになっていれば幸いです。
