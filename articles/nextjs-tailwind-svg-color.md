---
title: "Next.js + Tailwind CSSでsvg画像の色を動的に切り替える"
emoji: "🌐"
type: "tech"
topics:
  - "nextjs"
  - "react"
  - "tailwindcss"
  - "svg"
published: true
published_at: "2023-10-29 18:18"
---

`@svgr/webpack`というパッケージを使用すると svg 画像を React コンポーネントとして使用できました。

### svg 画像側の準備

SVG ファイルの色を動的に変更するためには、SVG の fill と stroke の属性を currentColor に設定する必要があります。これにより SVG の色は CSS や Tailwind CSS の text-color クラスで動的に制御できるようになります。

- fill: SVG の内部を塗りつぶす色を指定。
- stroke: SVG のアウトラインの色を指定。

```xml:sample-icon.svg
<svg ...>
  <path fill="currentColor" stroke="currentColor" ... />
</svg>
```

参考  
https://runebook.dev/ja/docs/tailwindcss/fill

### Next.js 側の準備

パッケージのインストール

```sh
npm install @svgr/webpack --save-dev
```

設定の修正

```diff js:next.config.js
/** @type {import('next').NextConfig} */
const nextConfig = {
  reactStrictMode: true,
  swcMinify: true,
+ webpack(config) {
+   config.module.rules.push({
+     test: /\.svg$/,
+     use: ['@svgr/webpack'],
+   });
+
+   return config;
+ },
};

module.exports = nextConfig;
```

### 使用例

```tsx
import SampleIcon from "images/sample-icon.svg";

export default function Sample() {
  return (
    <div className="w-24 h-24 p-6 text-white">
      <SampleIcon className="w-full h-full" />
    </div>
  );
}
```
