---
tags:
  - game
date: 2025-07-24
---
交换任意两个dom元素的位置，附带过渡动画

## 类型

``` ts
game.$swapElement(
	e1: HTMLDivElement,
	e2: HTMLDivElement,
	duration?: number, 
	timefun?: "linear" | "ease-in-out"
): Promise<void>
```

## 参数

参数一和参数二是一个**div元素**，参数三是**动画完成的时间** `ms`（默认值400），参数四是**动画过度的时间曲线**，很多，这里只列举两个（默认值`"linear"`）。

## 例子

``` js
game.$swapElement(card, event.dialog.selectedCard, 300).then(() => {
	event.dialog.isBusy = false;
	updateButtons();
});
```