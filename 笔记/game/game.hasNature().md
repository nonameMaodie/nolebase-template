---
tags:
  - game
  - tofix
date: 2025-07-24
---
判断卡牌信息/事件是否有某个属性

## 类型

``` ts
game.hasNature(
	item: Card | GameEvent,
	nature?: string | "linked",
	player?: Player
): boolean
```

## 参数

参数一是**卡牌/事件**；参数二若对应的布尔值为 `false`，则判断**卡牌/事件本身是否具备属性**，若为属性字符串，则判断**卡牌/事件本身是否与该属性有交集**，若为 `"linked"`则判断 [[lib.linked]] 数组是否和**卡牌/事件本身是否与该属性有交集**，参数三不知道是干什么的。

## 例子

``` js
game.hasNature(card)
```

``` js
game.hasNature(card, "fire");
```

``` js
game.hasNature(card, "linked");
```