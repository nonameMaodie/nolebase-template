---
tags:
  - game
date: 2025-07-24
---
基于钩子的添加势力方法，触发 [[game.callHook()|game.callHook("addGroup", [id, short, name, config]);]] 钩子，返回传入的势力id，新增的势力id会在 [[lib.group]] 里面追加。

## 类型

``` ts
game.addGroup(
	id: string,
	short: string | object,
	name: string | object,
	config: object
): string
```

## 参数

参数一是要添加的势力id，如 `"shu"`，参数二是该势力的短翻译或配置对象（如：`"蜀"`），参数三是该势力的长翻译或配置对象（如：`"蜀国"`），参数四是该势力的配置对象。

## 例子

``` js
game.addGroup('mo', '魔', '魔化', {
	color: "#000000",
	image: /* 图片路径 */,
})
```