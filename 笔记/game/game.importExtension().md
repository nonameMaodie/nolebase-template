---
tags:
  - game
date: 2025-08-08
---
导入扩展（支持 zip/对象/字符串等多种格式），并可指定导入后回调、导出名、扩展包信息等。

## 类型

``` ts
game.importExtension(
	extensionData: object | string,
	callback: function,
	name: string,
	packageInfo: object
): Promise<any>
```


## 例子

``` js
game.importExtension(extension, null, page.currentExtension, {
	intro: introExtLine.querySelector("input").value || "",
	author: authorExtLine.querySelector("input").value || "",
	netdisk: diskExtLine.querySelector("input").value || "",
	forum: forumExtLine.querySelector("input").value || "",
	version: versionExtLine.querySelector("input").value || "",
});
```

``` js
game.importExtension(extension, function () {
	exportExtLine.style.display = "";
});
```