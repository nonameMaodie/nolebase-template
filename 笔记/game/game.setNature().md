---
tags:
  - game
date: 2025-07-24
---
设置卡牌信息/事件的属性，返回该卡牌信息/事件的 `nature` 属性（`item.nature`）。

## 类型

``` ts
game.setNature(
	item: Card | GameEvent, 
	nature: string | string[], 
	addNature: boolean
): string | void
```

## 参数

参数一是**卡牌或者事件**，参数二是**一个属性字符串数组或者如`"fire|thunder|ice"`的字符串**（默认值为空数组），参数三是**设置模式**，若对应的布尔值为 false ，则将参数 `nature` 以字符串的形式赋值到`item.nature` ，否则追加到 `item.nature` 里。

## 例子

``` js
game.setNature(trigger, "thunder")
```

``` js
const next = target.damage();
if (!get.is.single()) {
	game.setNature(next, "thunder", true);
}
```