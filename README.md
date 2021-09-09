# vue-problem-of-slow-async-handler

vue でイベントハンドラの処理にかかる時間がユーザ操作よりも遅い場合に，
ユーザが最終的に行った操作と，最終的な画面表示が不整合になる問題の簡単なサンプル．
Promise に標準的な cancellation の仕組みが備わっててほしいなとおもいます．

fetch のための AbortController という仕組みがあり，本来はこれを正しく利用すればよいのかな，という感じなのかな: https://developer.mozilla.org/en-US/docs/Web/API/AbortController

ky や axios にも同様の仕組みがある．

ワクチンの副反応がおさまったら考察など追記する予定．

動画：

<video src="https://user-images.githubusercontent.com/8944787/132773386-9ae1bc88-6b85-4e38-943c-064a376852e4.mp4"></video>

## Project setup

```
yarn install
```

### Compiles and hot-reloads for development

```
yarn serve
```

### Compiles and minifies for production

```
yarn build
```

### Run your unit tests

```
yarn test:unit
```

### Run your end-to-end tests

```
yarn test:e2e
```

### Lints and fixes files

```
yarn lint
```

### Customize configuration

See [Configuration Reference](https://cli.vuejs.org/config/).
