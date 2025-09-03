---
tags:
  - game
  - tofix
date: 2025-07-24
---
添加新的属性杀，返回传入的属性字符串参数，其内部会触发 [[game.callHook()|game.callHook("addNature", [nature, translation, config])]] 钩子。

## 类型

``` ts
game.addNature(
	nature: string,
	translation: string,
	config: object
): string
```

## 参数

参数一是**自定义的属性杀id**（相关属性杀id请参照 [[lib.nature]] ），参数二是**该属性杀id的翻译**，参数三是**该属性杀的相关配置**。
