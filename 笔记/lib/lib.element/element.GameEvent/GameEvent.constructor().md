---
tags:
  - lib
  - tofix
  - lib-element
date: 2025-07-25
---
构造函数，返回一个[[GameEvent|事件]]实例。

## 类型

``` ts
constructor (
	name?: string | GameEvent,
	trigger?: boolean, 
	manager?: GameEventManager
): GameEvent
```

## 参数

参数一是若传入一个**字符串**，则作为新创建实例的 [[gameEvent.name|.name]] 属性，若是传入一个[[GameEvent|事件]]实例，则该实例的 [[gameEvent.name|.name]] , `.manager` 属性赋值给新创建实例的`.name` ,`.manager`。（默认值 `""`）
参数二传入一个布尔值，决定新创建实例是否能触发其他事件。（默认值 `true`）
参数三传入一个 `GameEventManager` 实例，暂时不知道是干什么的。（默认值 `_status.eventManager`）

## 例子

``` js
const evt = new lib.element.GameEvent();
```

``` js
const next = new lib.element.GameEvent(name, trigger, _status.eventManager);
```