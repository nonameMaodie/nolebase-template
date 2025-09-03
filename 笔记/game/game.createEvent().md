---
tags:
  - game
  - tofix
date: 2025-07-24
---
创建并返回一个`lib.element.GameEvent` 实例

## 类型

``` ts
game.createEvent(
	name: string, 
	trigger?: boolean, 
	triggerEvent?: GameEvent
): GameEvent
```

## 参数  

第一个参数是**指定创建的`GameEvent`实例的名字**，第二个参数默认为true，**好像是用于指定该事件实例的触发环节**，第三个参数是**指定该事件实例的父事件**。

## 例子

``` js
const next = game.createEvent("chooseCharacter");
next.showConfig = true;
next.setContent(/*传入一个函数，执行相关逻辑*/)
```