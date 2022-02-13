---
# try also 'default' to start simple
theme: seriph
# random image from a curated Unsplash collection by Anthony
# like them? see https://unsplash.com/collections/94734566/slidev
background: https://source.unsplash.com/collection/94734566/1920x1080
# apply any windi css classes to the current slide
class: 'text-center'
# https://sli.dev/custom/highlighters.html
highlighter: shiki
# show line numbers in code blocks
lineNumbers: false
# some information about the slides, markdown enabled
info: |
  ## History of electron-quick-start
# persist drawings in exports and build
drawings:
  persist: false
---

# History of electron-quick-start

Electron プログラミングモデルの変遷

2022/2/15 kondoumh

<div class="abs-br m-6 flex gap-2">
  <a href="https://github.com/kondoumh/history-of-electron-quick-start" target="_blank" alt="GitHub"
    class="text-xl icon-btn opacity-50 !border-none !hover:text-white">
    <carbon-logo-github />
  </a>
</div>

<!--
The last comment block of each slide will be treated as slide notes. It will be visible and editable in Presenter Mode along with the slide. [Read more in the docs](https://sli.dev/guide/syntax.html#notes)
-->

---

# electron-quick-start

[GitHub - electron/electron-quick-start: Clone to try a simple Electron app](https://github.com/electron/electron-quick-start)

- electron-quick-start は Electron でのプロジェクトのひな形となるアプリのリポジトリ
- Electron が1.0に到達する以前の2015年からメンテナンスされている
- Electron の進化とともにファイル構成もプログラミングモデルも変化してきた

---

# 2015/10/17
 [8113791cebec956796e5a10562b64fa965754f7e](https://github.com/electron/electron-quick-start/tree/8113791cebec956796e5a10562b64fa965754f7e)

最初期のコミットで、ファイルは2つ。(Node.js ではなく io.js を使用)

- <ph-file-html /> index.html
- <ph-file-js /> main.js

index.html の `script` タグで io.js の process API をダイレクトに呼び出し。

```html {8,9}
<!DOCTYPE html>
<html>
  <head>
    <title>Hello World!</title>
  </head>
  <body>
    <h1>Hello World!</h1>
    We are using io.js <script>document.write(process.version)</script>
    and Electron <script>document.write(process.versions['electron'])</script>.
  </body>
</html>
```

---

# 2015/10/20 - electron-prebuild 0.34.0

[fcc251352b84715b74be7ffff6cccf83eb796f91](https://github.com/electron/electron-quick-start/tree/fcc251352b84715b74be7ffff6cccf83eb796f91)

Node.js に移行、package.json が追加された。

- <ph-file-html /> index.html
- <ph-file-js /> main.js
- <codicon-json /> **package.json**

まだプロセスモデルが明確でなく、BrowserWindow に読み込まれた HTML の JavaScript でも Node.js API が使えるというユニークな開発環境のデモといった雰囲気。

<img src="/electron-1.drawio.png" class="ic">

<style>
  li:nth-child(3) {
    color: red;
  }
  .ic {
    display: block;
    margin-top: 1em;
    margin-bottom: 1em;
    margin-left: auto;
    margin-right: auto;
  }
</style>

---

# 2016/4/21 - electron-prebuild 0.37.0

[1e287bdb624bb1a62362316994c226ba7d26f6a9](https://github.com/electron/electron-quick-start/tree/1e287bdb624bb1a62362316994c226ba7d26f6a9)

renderer.js が追加された。

- <ph-file-html /> index.html
- <ph-file-js /> main.js
- <codicon-json /> package.json
- <ph-file-js /> **renderer.js**

<style>
  li:nth-child(4) {
    color: red;
  }
</style>

---

# 2016/4/21 - electron-prebuild 0.37.0

[1e287bdb624bb1a62362316994c226ba7d26f6a9](https://github.com/electron/electron-quick-start/tree/1e287bdb624bb1a62362316994c226ba7d26f6a9)

index.html のコメントに renderer process という言葉が出現。

```html {4,12}
<html>
  <body>
    <h1>Hello World!</h1>
    <!-- All of the Node.js APIs are available in this renderer process. -->
    We are using node <script>document.write(process.versions.node)</script>,
    Chromium <script>document.write(process.versions.chrome)</script>,
    and Electron <script>document.write(process.versions.electron)</script>.
  </body>

  <script>
    // You can also require other files to run in this process
    require('renderer.js')
  </script>
</html>
```

renderer.js は script タグで require で読み込まれている。

---

# 2016/4/21 - electron-prebuild 0.37.0

[1e287bdb624bb1a62362316994c226ba7d26f6a9](https://github.com/electron/electron-quick-start/tree/1e287bdb624bb1a62362316994c226ba7d26f6a9)


main process でウィンドウ制御、renderer process で Node.js の API も活用して UI のコードを書くというプログラミングモデル。

両プロセス間の通信もあたり前に行われていた。

<img src="/electron-2.drawio.png" class="ic">

この構成が3年ぐらい続くことになる。

<style>
  .ic {
    display: block;
    margin-top: 1em;
    margin-bottom: 1em;
    margin-left: auto;
    margin-right: auto;
  }
</style>

---

# 2019/6/8 - Electron 5.0.2

[1cb6d17b40c6fc14fd6dc4e6f2a951558b7e7297](https://github.com/electron/electron-quick-start/tree/1cb6d17b40c6fc14fd6dc4e6f2a951558b7e7297)

preload を使用する PR がマージされた。

[feat: use a preload script instead of enabling nodeIntegration by malept · Pull Request #279 · electron/electron-quick-start](https://github.com/electron/electron-quick-start/pull/279)

JS ファイルは preload.js を含む3ファイル構成に

- <ph-file-html /> index.html
- <ph-file-js /> main.js
- <codicon-json /> package.json
- <ph-file-js /> **preload.js**
- <ph-file-js /> renderer.js

<style>
  li:nth-child(2) {
    color: orange;
  }
  li:nth-child(4) {
    color: red;
  }
  li:nth-child(5) {
    color: orange;
  }
</style>

---

# 2019/6/8 - Electron 5.0.2

[1cb6d17b40c6fc14fd6dc4e6f2a951558b7e7297](https://github.com/electron/electron-quick-start/tree/1cb6d17b40c6fc14fd6dc4e6f2a951558b7e7297)

```html {14,9-11}
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8">
    <title>Hello World!</title>
  </head>
  <body>
    <h1>Hello World!</h1>
    We are using Node.js <span id="node-version"></span>,
    Chromium <span id="chrome-version"></span>,
    and Electron <span id="electron-version"></span>.

    <!-- You can also require other files to run in this process -->
    <script src="./renderer.js"></script>
  </body>
</html>
```

- Node の process API が直接使われていた部分が span 要素に
- require ではなく src で renderer.js を読み込むように変更

---

# 2019/6/8 - Electron 5.0.2

[1cb6d17b40c6fc14fd6dc4e6f2a951558b7e7297](https://github.com/electron/electron-quick-start/tree/1cb6d17b40c6fc14fd6dc4e6f2a951558b7e7297)

BrowserWindow 生成時の webPreferences で preload.js を読み込み。

```js {7}
function createWindow () {
  // Create the browser window.
  mainWindow = new BrowserWindow({
    width: 800,
    height: 600,
    webPreferences: {
      preload: path.join(__dirname, 'preload.js')
    }
```

preload.js では、index.html の span 要素を指定して process API でバージョンをレンダリング。

```js
window.addEventListener('DOMContentLoaded', () => {
  for (const versionType of ['chrome', 'electron', 'node']) {
    document.getElementById(`${versionType}-version`).innerText = process.versions[versionType]
  }
})
```

---

# 2019/6/8 - Electron 5.0.2

[1cb6d17b40c6fc14fd6dc4e6f2a951558b7e7297](https://github.com/electron/electron-quick-start/tree/1cb6d17b40c6fc14fd6dc4e6f2a951558b7e7297)

- renderer process を Node.js から分離し、ContextBridge を介して main process や Node.js API を使用
  - preload で定義されているメソッドやイベントは window オブジェクトのメソッドやイベントに見える
- renderer プロセスは DOM の操作に特化し、ブラウザで実行される JavaScript と同等のサンドボックス環境になった[^1]。

[^1]: preload.js で DOM 操作やってしまってるのでサンプルとしては微妙だが。

<img src="/electron-3.drawio.png" class="ic">

<style>
  .ic {
    display: block;
    margin-top: 1em;
    margin-bottom: 1em;
    margin-left: auto;
    margin-right: auto;
  }
</style>

---

# 2019/11/13 - Electron 7.1.1

[0ed07b8a7b21da2c9903769af44cb3a49c49fd68](https://github.com/electron/electron-quick-start/tree/0ed07b8a7b21da2c9903769af44cb3a49c49fd68)

index.html に Content Security Policy の meta タグが追加

```html {5-7}
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8">
    <!-- https://developer.mozilla.org/en-US/docs/Web/HTTP/CSP -->
    <meta http-equiv="Content-Security-Policy" content="default-src 'self'; script-src 'self'">
    <meta http-equiv="X-Content-Security-Policy" content="default-src 'self'; script-src 'self'">
    <title>Hello World!</title>
  </head>
  <body>
    <h1>Hello World!</h1>
    We are using Node.js <span id="node-version"></span>,
    Chromium <span id="chrome-version"></span>,
    and Electron <span id="electron-version"></span>.

    <!-- You can also require other files to run in this process -->
    <script src="./renderer.js"></script>
  </body>
</html>
```

---

# 2021/10/7 - Electron 15.1.1

[4ff40bc838cc4c009f569300a5ba8e6a68924223](https://github.com/electron/electron-quick-start/tree/4ff40bc838cc4c009f569300a5ba8e6a68924223)

デフォルトの CSS ファイルが追加された。

- <ph-file-html /> index.html
- <ph-file-js /> main.js
- <codicon-json /> package.json
- <ph-file-js /> preload.js
- <ph-file-js /> renderer.js
- <ph-file-css /> **styles.css**

<style>
  li:nth-child(6) {
    color: red;
  }
</style>

---

# 2022/1/3 - Electron 16.0.5

[bc9cce16d583ba3b69aae318b3904d8a04979058](https://github.com/electron/electron-quick-start/tree/bc9cce16d583ba3b69aae318b3904d8a04979058)

inline CSS を 許可する PR がマージされた。

[CORS: Allow inline CSS and style attributes by Kilian · Pull Request #550 · electron/electron-quick-start](https://github.com/electron/electron-quick-start/pull/550)

```html {6}
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8">
    <!-- https://developer.mozilla.org/en-US/docs/Web/HTTP/CSP -->
    <meta http-equiv="Content-Security-Policy" content="default-src 'self'; script-src 'self'; style-src 'self' 'unsafe-inline'">
    <link href="./styles.css" rel="stylesheet">
    <title>Hello World!</title>
  </head>
  <body>
    <h1>Hello World!</h1>
    We are using Node.js <span id="node-version"></span>,
    Chromium <span id="chrome-version"></span>,
    and Electron <span id="electron-version"></span>.
    <script src="./renderer.js"></script>
  </body>
</html>
```

---

# 最後に

``

- ここ数年で初期のプロセスモデルが大きく変わって renderer process は Node.js の世界から分離されたブラウザでの実行モデルに近づいた。
- electron-quick-start のコードは最小限で、実開発でコードをどう書くべきということまでは示していない。
- 標準的な構成を示すという意味で重要な存在。
- 今後は、ES Module 対応が入ってくるのではないかと予想。
