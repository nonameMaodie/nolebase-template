---
tags:
  - game
date: 2025-07-24
---
初始化并返回一个武将列表，仅无参时修改_status.characterlist

## 类型

``` ts
game.initCharacterList(filter?: boolean): string[]
```

## 参数

第一个参数是**筛选逻辑**：false 跳过移除逻辑，否则执行默认移除逻辑（若为`undefined`，将把`_status.characterlist`赋值为筛选后的武将列表）


## 例子

``` js
if (!_status.characterlist) {
	game.initCharacterList();
}
```
