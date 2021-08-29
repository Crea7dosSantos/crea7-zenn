---
title: "microCMS + TailwindCSSでのHTMLコンテンツに対するスタイル適用"
emoji: "🖼️"
type: "tech"
topics: ["nextjs", "tailwindcss"]
published: true
---
以前[Crea7](https://crea7dos3tos.com)というタイムラインとブログ機能を持ったサイトを作成した時に、Next.js + microCMS + Tailwind CSSで実装を行いました。
# 問題点
記事の詳細画面では、microCMSのリッチエディタで作成した記事を、HTML形式で取得しinnerHTMLで表示を行なっています。
```tsx
<div
className="text-black pt-7"
dangerouslySetInnerHTML={{ __html: `${article.body}` }}
></div>
```

本来であればmicroCMS側が返すHTMLにスタイルが当たっているので、innerHTMLで表示を行うだけである程度いい感じの画面になるのですが、Tailwind CSSを適用している状態では標準のスタイルは以下のように適用されません。
![not-apply-style](https://storage.googleapis.com/zenn-user-upload/3edd44cba6454d672c0e412c.png =650x)

そこで今回は`@tailwindcss/typography`でスタイルを適用します

# @tailwindcss/typographyの導入
[tailwindcss/typography](https://github.com/tailwindlabs/tailwindcss-typography)はTailwindCSS v2.0 +用に設計されているHTMLにスタイルを適用してくれるプラグインです。
とても導入のステップも少ないので便利です。

## 導入手順
1. まずはnpmでプラグインをインストールします。
```
# Using npm
npm install @tailwindcss/typography

# Using Yarn
yarn add @tailwindcss/typography
```
2. 次にドキュメント通り`tailwind.config.js`に以下を追記します
``` js
// tailwind.config.js
module.exports = {
  theme: {
    // ...
  },
  plugins: [
    require('@tailwindcss/typography'),
    // ...
  ],
}
```
3. 最後に`prose`クラスをリッチエディタのコンテンツを吐き出しているクラスに追記します
``` tsx
<div
className="prose text-black pt-7"
dangerouslySetInnerHTML={{ __html: `${article.body}` }}
></div>
```
また[サイズ修飾子](https://github.com/tailwindlabs/tailwindcss-typography#size-modifiers)や[カラー修飾子](https://github.com/tailwindlabs/tailwindcss-typography#color-modifiers)があり、全体のサイズ調整やリンクのカラーを変更することが可能です。Tailwind CSSのレスポンシブとサイズ修飾子を組み合わせることで端末サイズに合わせて見やすいレイアウトを実装することが可能だと思います。

また上記のクラスを当てた結果が以下の通りです。
![apply-style](https://storage.googleapis.com/zenn-user-upload/c3d27f3636d6a5cf30e9e72e.png =650x)


簡単に導入しいい感じのスタイルを適用する事ができましたが、リポジトリの[Issue](https://github.com/tailwindlabs/tailwindcss-typography/issues)にある通り適用されているスタイルが気に入らない場合は、個別にスタイルを当てるなどをするしかなさそうです。
