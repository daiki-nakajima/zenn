---
title: "Wezterm を試してみたメモ"
emoji: ""
type: "tech"
topics:
  - "wezterm"
  - "terminal"
published: false
published_at: "2025-02-09 18:30"
---

Wezterm は設定ファイルによるカスタマイズが魅力のターミナルエミュレータです。  
この記事では、Wezterm の設定ファイルの記載例と構成について説明し、初見の人がWeztermを使えるようになることを目指します。  
なお、Windows を前提とした環境での設定例となります。

## インストール

インストール方法は、[公式サイト](https://wezterm.org/installation.html) に記載されています。  
私はインストーラ（setup.exe）を使用しました。

## 設定ファイル

Wezterm の設定ファイルは Lua スクリプトで記述します。  
[公式サイト](https://wezterm.org/config/files.html#configuration-files) に従い、設定ファイルを `$HOME/.config/wezterm/wezterm.lua` に配置すれば OK です。  

## サンプルコード

サンプルコードは [GitHub](https://github.com/daiki-nakajima/wezterm/tree/1.0.0) にアップロードしています。  
私の環境では、見やすさを考慮して、メインの設定ファイル `wezterm.lua` に加え、キーバインド用の `keybinds.lua` と起動メニュー用の `launch_menu.lua` の 3 つに分割しています。  
各ファイルの役割は以下の通りです。

### 1. `wezterm.lua`  

メインの設定ファイル

- 外観や動作に関する基本設定（作業ディレクトリ、フォント、カラースキーム、ウィンドウの装飾、背景の透過率など）を記述  
- キーバインド設定（`keybinds.lua`）や起動メニュー（`launch_menu.lua`）などを `require` を使って読み込み  
- イベントハンドラを利用して、GUI 起動時のウィンドウ最大化やタブタイトルのカスタマイズを実装

### 2. `keybinds.lua`  

キーバインドの設定

- コピー、貼り付け、タブ操作、アプリケーション終了、フォントサイズの変更など、ユーザーが快適に操作できるショートカットを設定

### 3. `launch_menu.lua`  

起動メニューの設定

- Command Prompt、PowerShell、WSL2 など、複数のシェルを起動するためのメニュー項目を定義  
- ショートカットから呼び出して各シェルを選択・起動できる

## 設定ファイルの構成

Wezterm の設定ファイルはオブジェクト指向の考え方に基づいており、設定内容をひとまとめにした `config` オブジェクトに対して各種設定を施します。  
最終的に、この `config` オブジェクトを返すことで、Wezterm は起動時にその内容を読み込み、ターミナルの動作や見た目を構築します。

```lua
local config = {}  -- 設定用のオブジェクト（テーブル）

-- 各種プロパティを設定
config.default_cwd = "C:/dev"
config.font = wezterm.font("Ricty Diminished")
config.font_size = 12
-- ...（他の設定項目もここに記述）

return config  -- このオブジェクトを返すことで、Wezterm が設定内容を取得
```

## Lua と JavaScript の比較

個人的には、Lua の基本的な書き方は JavaScript に似た部分が多く、直感的に理解しやすいと感じました。  
Lua と JavaScript のコード比較は以下のイメージです。（JavaScriptは想像で書いています）

```markdown

### 変数宣言

- *JavaScript*:

  ```javascript
  let a = 10;
  ```

- *Lua*:

  ```lua
  local a = 10
  ```

### イベントハンドラ

- *Lua*:

  ```lua
  wezterm.on('gui-startup', function(cmd)
    local tab, pane, window = wezterm.mux.spawn_window(cmd or {})
    window:gui_window():maximize()
  end)
  ```

- *JavaScript 相当*:

  ```javascript
  wezterm.on('gui-startup', (cmd) => {
    const [tab, pane, window] = wezterm.mux.spawn_window(cmd || {});
    window.gui_window().maximize();
  });
  ```

### オブジェクト（テーブル）

Lua のテーブルは、JavaScript のオブジェクトや配列に相当します。

- *JavaScript*:

  ```javascript
  const person = { name: "Alice", age: 30 };
  ```

- *Lua*:

  ```lua
  local person = { name = "Alice", age = 30 }
  ```

## まとめ

以上の基本を理解した上で、あとは利用用途に応じて[公式ドキュメント](https://wezterm.org/config/files.html)を参照すれば設定を変更していくことができると思います。  
参考になれば幸いです。
