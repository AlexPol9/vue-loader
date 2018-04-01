# ホットリロード

"ホットリロード"はファイルを編集するときに単にページをリロードするだけではありません。ホットリロードを有効にすると、`*.vue` ファイルを編集するとき、すべてのコンポーネントのインスタンスは **ページのリロードをせずに** 取り替えられます。アプリの現在の状態を保持し、コンポーネントを取り替えることが出来ます！これはコンポーネントのテンプレートやスタイリングを微調整するときの開発体験を劇的に改善します。

![hot-reload](http://blog.evanyou.me/images/vue-hot.gif)

## 状態維持ルール

- コンポーネントの `<template>` を編集するとき、編集されたコンポーネントのインスタンスは、そのまま再描画し、全て現在のプライベートな状態に維持します。これは、テンプレートが副作用を発生させない新しい描画関数にコンパイルされるため可能です。

- コンポーネントの `<script>` 部分を編集するとき、編集されたコンポーネントのインスタンスは破棄され、そのまま再作成されます。(アプリケーション内の他のコンポーネントの状態は維持されています。) これは、`<script>` が副作用を生むかもしれないライフサイクルフックを含むことができるため、一貫性のある動作を保証するためには再描画の代わりに"リロード"が必要です。これはまた、コンポーネントライフサイクルフック内のタイマーのようなグローバルな副作用に注意する必要があることを意味します。場合によっては、コンポーネントでグローバルな副作用が発生する場合は、フルページリロードの必要があるかもしれません。

- `<style>` ホットリロードは `vue-style-loader` を介して単独で動作するため、アプリケーションの状態に影響しません。

## 使い方

`vue-cli` を使ってプロジェクトをスキャホールドすると、ホットリロードはすぐに使えるようになります。

プロジェクトを手動で設定するときは、ホットリロードは、`webpack-dev-server --hot` によってプロジェクトを提供するとき、自動的に有効なります。

上級者は、`vue-loader` によって内部的に使用されいてる [vue-hot-reload-api](https://github.com/vuejs/vue-hot-reload-api) を確認することができます。

## ホットリロードの無効

ホットリロードは以下の状況を除いて常に有効です:

 * webpack の `target` が `node` (SSR)
 * webpack ミニファイコード
 * `process.env.NODE_ENV === 'production'`
  
ホットリロードを明示的に無効にするためには、`hotReload: false` オプションを使用してください:

``` js
module: {
  rules: [
    {
      test: /\.vue$/,
      loader: 'vue-loader',
      options: {
        hotReload: false // ホットリロードを無効
      }
    }
  ]
}
```