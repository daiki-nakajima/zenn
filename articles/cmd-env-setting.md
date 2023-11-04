---
title: "コマンドプロンプトで環境変数を設定する"
emoji: "🌐"
type: "tech"
topics:
  - "windows"
  - "コマンドプロンプト"
published: true
published_at: "2023-11-04 23:00"
---

環境変数の設定をGUIでやるのは結構面倒なのでメモ。

Win + r で「cmd」と入力してOK。コマンドプロンプトが立ち上がる。

### 環境変数の設定

```sh
set PATH=C:\Program Files\Java\jdk1.8.0_144\bin;
```

### 環境変数の一覧表示

```sh
set
```

## 環境変数一覧から文字列を抽出

```sh
set | find "PATH"
```

