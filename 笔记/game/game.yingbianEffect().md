---
tags:
  - game
date: 2025-07-25
---
创建并返回一个名为 `"yingbianEffect"` 的应变事件

## 类型

``` js
game.yingbianEffect(
	event: GameEvent, 
	content: function,
	...args: any[]
): GameEvent
```

## 参数

参数一是**将要触发应变效果的事件**，参数二是**应变事件的逻辑执行函数**

## 例子

``` js
game.yingbianEffect(trigger, value)
```