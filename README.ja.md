# Paper-es

Paper.js (ベクターグラフィックススクリプティングのスイスアーミーナイフ) からフォークされたESモジュール版

## 使い方

```js
import { Paper } from "https://code4fukui.github.io/Paper-es/Paper.js";

Paper.install(window);
```

Paper.jsは `dist/paper-core.js` (PaperScript非搭載) をベースにしています。

## 例

```html
<!DOCTYPE html><html lang="ja"><head><meta charset="utf-8"><meta name="viewport" content="width=device-width">
<title>rounded-rectangles - Paper-es</title>
<script type="module">
import { Paper, Tool, Path, Rectangle } from "https://code4fukui.github.io/Paper-es/Paper.js";

Paper.install(window); // -> project, tool, view

window.onload = () => {
	Paper.setup("canvas");

	let mousePoint = view.center;
	view.onMouseMove = (e) => {
		mousePoint = e.point;
	};

	const colors = ["red", "orange", "yellow", "green", "aqua", "blue", "purple"];
	const amount = colors.length * 3;
	const size = 20;
	for (let i = 0; i < amount; i++) {
		const rect = new Rectangle([0, 0], [size, size]);
		rect.center = mousePoint;
		const path = new Path.Rectangle(rect, size / 4);
		path.fillColor = colors[i % colors.length];
		const scale = (1 - i / amount) * 20;
		path.scale(scale);
	}

	const children = project.activeLayer.children;
	view.onFrame = (e) => {
		for (let i = 0; i < children.length; i++) {
			const item = children[i];
			const deltax = (mousePoint.x - item.position.x) / (i + 1);
			const deltay = (mousePoint.y - item.position.y) / (i + 1);
			item.rotate(Math.sin((e.count + i) / 8) * 7);
			if (deltax * deltax + deltay * deltay > 0.1 * .1) {
				item.position.x += deltax;
				item.position.y += deltay;
			}
		}
	}
};
</script>
</head>
<body>
<h1>rounded-rectangles - Paper-es</h1>
<canvas id="canvas" resize></canvas>

<style>
body {
	margin: 0;
	height: 100vh;
	text-align: center;
}
canvas {
	display: block;
	width: 100%;
	height: 80%;
}
</style>
</body>

</html>
```
[Paper-es examples](https://code4fukui.github.io/Paper-es/)

## 関連情報

Paper.jsを使用したい場合は、[http://paperjs.org/download/](http://paperjs.org/download/) から最新の "stable" (安定) 版をダウンロードしてください。

- ウェブサイト: <http://paperjs.org/>
- 質問: <https://stackoverflow.com/questions/tagged/paperjs>
- ディスカッションフォーラム: <https://groups.google.com/group/paperjs>
- メインラインのソースコード: <https://github.com/paperjs/paper.js>
- Twitter: [@paperjs](https://twitter.com/paperjs)
- 最新リリース: <http://paperjs.org/download/>
- ビルド済みの開発バージョン:
  [`prebuilt/module`](https://github.com/paperjs/paper.js/tree/prebuilt/module) および [`prebuilt/dist`](https://github.com/paperjs/paper.js/tree/prebuilt/dist) ブランチ。

### どのバージョンを使用すべきか

各ディストリビューションには、圧縮版 (minified) と通常版の2種類のビルド済み Paper.js が同梱されています。

- `paper-full.js` – ブラウザ用の完全版。PaperScriptのサポートとAcorn.jsが含まれます。
- `paper-core.js` – ブラウザ用のコア版。PaperScriptのサポートやAcorn.jsは含まれません。JavaScriptを直接記述する場合に、ファイルサイズやコンパイル時間を削減するために使用できます。

### Node.js と NPM のインストール

Node.jsにはNode Package Manager (NPM) が付属しています。さまざまなプラットフォームにNode.jsをインストールする方法については、多くのチュートリアルが存在します。Node.jsの開発サイクルは非常に速く、OSが提供するパッケージマネージャーのバージョンは古くなっていることが多いため、それらを使用してインストールすることは一般的に推奨されません。

macOSでは、`brew upgrade` で1つのNode.jsバージョンを最新に保つだけで十分な場合、[Homebrew](https://brew.sh/) が良い選択肢となります:  
<https://treehouse.github.io/installation-guides/mac/node-mac.html>

プロジェクトごとに異なるバージョンが要求される場合など、同じプラットフォーム上で複数のNode.jsバージョンをインストールして管理するには、代わりに [NVM](https://github.com/creationix/nvm) を使用できます:  
<https://nodesource.com/blog/installing-node-js-tutorial-using-nvm-on-mac-os-x-and-ubuntu/>

次の段落で説明するように、Node.js上でCanvasへのレンダリングを伴うPaper.jsをインストールする場合も、macOSではHomebrewが推奨されます。

Linuxの場合は、<https://nodejs.org/download/> を参照して32ビットおよび64ビットのNode.jsバイナリやソースコードを入手するか、前の段落で説明したようにNVMを使用してください。

### NPMを使用した Paper.js のインストール

Paper.jsはNPMで `paper`、`paper-jsdom`、`paper-jsdom-canvas` の3つの異なるバージョンで提供されています。ユースケースに応じて、適切なものを require する必要があります:

- `paper` はメインライブラリであり、Webブラウザやワーカーなどのブラウザコンテキストで直接使用できます。
- `paper-jsdom` はNode.js用のシムモジュールで、[jsdom](https://github.com/tmpvar/jsdom) を通じたSVGのインポートおよびエクスポートによるヘッドレスでの使用を提供します。
- `paper-jsdom-canvas` はNode.js用のシムモジュールで、[Node-Canvas](https://github.com/Automattic/node-canvas) を通じたCanvasレンダリングと、[jsdom](https://github.com/tmpvar/jsdom) を通じたSVGのインポートおよびエクスポートを提供します。

`paper-jsdom-canvas` をインストールするには、システムに [Cairo Graphics ライブラリ](https://cairographics.org/) がインストールされている必要があります:

### ネイティブ依存関係のインストール

Paper.jsはレンダリングに [Node-Canvas](https://github.com/Automattic/node-canvas) に依存しており、これはさらにネイティブライブラリである [Cairo](https://cairographics.org/) と [Pango](https://www.pango.org/) に依存しています。

#### macOSでのネイティブ依存関係のインストール

Paper.jsはレンダリングにNode-Canvasに依存しており、これはさらにCairoとPangoに依存しています。Cairoをインストールする最も簡単な方法は、[Homebrew](https://brew.sh/) を使用して次のコマンドを実行することです:

    brew install cairo pango

現在、macOS上のCairoには問題があることに注意してください。上記でエラーが発生した場合、以下のコマンドで解決する可能性が高いです:

    PKG_CONFIG_PATH=/opt/X11/lib/pkgconfig/ npm install paper

また、モジュールを更新する際には、常に以下を実行する必要があります:

    PKG_CONFIG_PATH=/opt/X11/lib/pkgconfig/ npm update

この要件を忘れがちな場合や、シンプルでクリーンなコマンドを入力できるようにしたい場合は、`.bash_profile` ファイルに以下を追加してください:

    # PKG Config for Pango / Cairo
    export PKG_CONFIG_PATH=/usr/local/lib/pkgconfig:/opt/X11/lib/pkgconfig

この行を追加した後は、コマンドが期待通りに動作するはずです:

    npm install paper
    npm update

#### Debian/Ubuntu Linuxでのネイティブ依存関係のインストール

    sudo apt-get install pkg-config libcairo2-dev libpango1.0-dev lib

MIT License — [LICENSE](LICENSE) を参照してください。
