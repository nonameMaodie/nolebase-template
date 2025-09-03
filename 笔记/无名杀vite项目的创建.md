## 制作空白扩展

打开无名杀 -> 制作扩展 -> 输入扩展名（如：无名扩展）-> 保存并退出无名杀

## 创建vite项目

**注意：vite 需要 v7.0.5 以上版本（cmd** `**npm list vite**` **查看版本）**

使用 vs code 打开无名杀的 extension 文件夹，`control + j`打开 vs code 终端，须先安装 nodejs

创建普通vite项目，可以将 noname-ext 改为其他自定义项目名

```bash
npm create vite@latest noname-ext
```

创建vite + vue项目

```bash
npm init vue@latest noname-ext
```

## 安装依赖

```bash
cd noname-ext
```

```bash
npm install
```

## 项目文件删减与补充

### 删除文件

删除多余文件如`noname-ext`项目根目录的`public`文件夹、`index.html`, src目录下的`assets`文件夹、用不到的`*.vue`文件、`*.css`文件。

### 补充文件

将`extension/无名扩展/LICENSE`协议文件复制粘贴到`noname-ext`项目根目录下。

将`extension/无名扩展/extension.js`文件复制粘贴到`noname-ext`项目根目录下并更名为`index.js`（作为项目的主入口）。

`noname-ext`项目的src目录下创建一个`extension`文件夹。

1. 创建一个`src/extension/updateHistory.js`文件

提供扩展更新信息

> `src/extension/updateHistory.js
```js
export default [
	{
		"version": "v0.0",
		"date": "20xx-xx-xx",
		"changes": [
			"创建了新的扩展"
		]
	}
]
```

2. 创建一个`src/extension/info.js`文件

提供扩展的基本信息，用于生成扩展的 info.json

> `src/extension/info.js`
```js
import updateHistory from "./updateHistory.js";
const lastest = updateHistory[0];

export default {
	intro: "",
	name: "无名扩展",
	author: "无名玩家",
	diskURL: "",
	forumURL: "",
	version: lastest.version,
};
```

3. 创建一个`src/extension/writeFile.js`文件

生成扩展的 info.json 和 LICENSE 的执行函数，可以加一些自定义的逻辑用于生成扩展的其他文件

> `src/extension/writeFile.js`
```js
import info from './info.js';

export default function writeFile(fs, path, projectRoot) {
	const outDir = path.resolve(projectRoot, `../${info.name}`).replace(/\\/g, "/");
	fs.mkdirSync(outDir, { recursive: true });

	// 构建结束时生成版本文件
	const content = JSON.stringify(info);
	fs.writeFileSync(`${outDir}/info.json`, content);
	console.log(`文件已生成：`, `${outDir}/info.json`);

	// 读写协议文件 
	const data = fs.readFileSync(path.resolve(projectRoot, 'LICENSE'));
	fs.writeFileSync(`${outDir}/LICENSE`, data);
	console.log(`文件已生成：`, `${outDir}/LICENSE`);
}
```

4. 项目根目录下创建一个`./vite-plugins/vite-write-file-plugin.js`文件。

vite 插件，用于调用 writeFile 函数

> `./vite-plugins/vite-write-file-plugin.js`
```js
import fs from "fs";
import path from "path";
import writeFile from "../src/extension/writeFile.js";
import { fileURLToPath } from 'url';
import { dirname, resolve } from 'path';

// 获取项目根目录
const projectRoot = resolve(dirname(fileURLToPath(import.meta.url)), '..')

// 构建结束后写入自定义文件
export default function writeFilePlugin() {
	return {
		name: "generate-write-file",
		config(config, { command }) {
			if (command === 'build') {
				writeFile(fs, path, projectRoot);
			}
		}
	};
}

```

5. 项目根目录下创建一个`./vite-plugins/vite-auto-run-build-plugin.js`文件。

>  `./vite-plugins/vite-auto-run-build-plugin.js`
```js
import { exec } from "node:child_process";
import { fileURLToPath } from 'url';
import { dirname, resolve } from 'path';

// 获取项目根目录
const projectRoot = resolve(dirname(fileURLToPath(import.meta.url)), '..')

// 监听src目录下的文件变化并自动打包
export default function autoRunBuildPlugin() {
	return {
		name: "auto-run-build",
		configureServer(server) {
			server.watcher.on('change', (file) => {
				const absoluteDir = projectRoot.replace(/\\/g, "/");
				const relativePath = file.replace(/\\/g, "/").replace(absoluteDir, "");
				if (relativePath.startsWith("/src/") || ["/index.js", "/external.js"].includes(relativePath)) {
					exec("npm run build", (error, stdout, stderr) => {
						if (error) {
							console.error("❌ 构建失败:", stderr);
						} else {
							console.log("✅ 构建成功:", stdout);
						}
					});
				}
			});
		},
	};
}
```

6. 项目根目录下创建一个`external.js`文件。

来源于 无名杀 的所有依赖请写在这个文件里，项目源码再依赖这个文件，防止打包体积过大

```js
export { lib, game, ui, get, ai, _status } from '../../noname.js';
```

7. 完善`vite.config.js`文件。

普通项目请手动删除 vue 和 vueDevTools 等相关vue插件

> `vite.config.js`
```js
import { fileURLToPath, URL } from "node:url";
import vueDevTools from "vite-plugin-vue-devtools";
import { defineConfig } from "vite";
import vue from "@vitejs/plugin-vue";
import { resolve, dirname } from "path";
import info from "./src/extension/info.js";
import writeFilePlugin from "./vite-plugins/vite-write-file-plugin.js";
import autoRunBuildPlugin from "./vite-plugins/vite-auto-run-build-plugin.js";

export default defineConfig(({ command }) => {
	command = "serve"; //&& false; // 此处用于强制使用开发模式，方便调试
	const isDev = command === "serve";

	return {
		plugins: [vue(), vueDevTools(), autoRunBuildPlugin(), writeFilePlugin()],
		define: {
			"process.env.NODE_ENV": JSON.stringify(process.env.NODE_ENV || "production"),
		}, // 保证在浏览器环境下运行
		resolve: {
			alias: {
				"@": fileURLToPath(new URL("./src", import.meta.url)),
			},
		},
		build: {
			minify: !isDev && "terser", // 开发环境，禁用压缩，方便调试
			sourcemap: isDev, // 开发环境，生成 sourcemap 文件
			outDir: resolve(__dirname, `../${info.name}`), // 输出到与项目同级的 dist 目录

			// lib: {
			// 	entry: resolve(__dirname, "index.js"), // 库入口文件
			// 	fileName: "extension", // 输出文件名（不含扩展名）
			// 	formats: ["es"],
			// },
			rollupOptions: {
				external(id, parent) {
					if (parent && parent.includes("external.js")) {
						return true;
					}
					return false;
				},
				input: "index.js", // 入口文件
				output: {
					entryFileNames: "extension.js",
					chunkFileNames: "[name].js",
					manualChunks: id => (id.includes("node_modules") ? "vendor" : null),
					assetFileNames(assetInfo) {
						if (assetInfo.name.endsWith(".css")) {
							return "extension.css";
						}
						return "[name][extname]";
					},
					inlineDynamicImports: false,
					preserveModules: false, // 禁止将模块拆分到多个文件[1](@ref)
					format: isDev ? "es" : "iife",
				},
			},
			copyPublicDir: false, // 禁止复制 public 目录[1](@ref)
		},
		css: {
			devSourcemap: isDev, // 映射 CSS 到原始文件
		},
		terserOptions: {
			compress: {
				drop_console: !isDev, // 删除所有 console.* 调用
			},
		},
	};
});
```

8. 其他待完善文件请自行补充。

比如：

> index.js
```js
import ext from "./src/extension.js";
import { game } from "./external.js";

game.import("extension", ext);
```

vue 项目的mian.js

> `mian.js`
```js
import { createApp } from "vue";
import App from "./App.vue";
import info from "./extension/info.js";
import { lib, ui } from "../external.js";

lib.init.css(lib.assetURL + `extension/${info.name}`, 'extension');//调用css样式

export default function openApp(parentElement) {
	const container = ui.create.div(parentElement);
	container.style.cssText = "width: 100%; height: 100%; position: absolute; top: 0; left: 0; z-index: 20; background: rgba(0, 0, 0, 0.5);";
	// 提供 removeContainer 方法
	let app = createApp(App);
	app.provide("removeContainer", () => {
		app.unmount();
		container.remove();
		app = null;
	});
	app.mount(container);
	return app;
}
```

./src/extension.js

> `./src/extension.js`
```js
import { lib, game, ui, get, ai, _status } from "../external";
import openApp from "./main.js";
import info from "./extension/info.js";

function open(){
  const getSystem = setInterval(() => {
				if (ui.system1 || ui.system2) {
					clearInterval(getSystem);
					ui.create.system(`vue`, function () {
						openApp(ui.window);
					});
				}
			}, 500);
}
```

## 打包项目

### 开发环境

```js
export default defineConfig(({ command }) => {
	command = 'serve' //&& false; // 手动注释'serve'后面的代码，设置为开发环境
	/* ... */
```

```bash
npm run build
```

或者

借助 autoRunBuildPlugin 插件实现自动打包，只需要运行一次

```bash
npm run dev
```

### 生产环境

> `vite.config.js`
```js
export default defineConfig(({ command }) => {
	command = 'serve' && false; // 手动删除'&&'前面的"//"，设置为生产环境
	...
```

```bash
npm run build
```