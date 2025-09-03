---
tags:
  - game
date: 2025-07-24
---
元素去到某个父元素的某个位置，附带过度动画

## 类型

``` ts
game.$elementGoto(
	element: HTMLDivElement,
	Parent: HTMLDivElement,
	position?: number | "first" | "last" | Node,
	duration?: number,
	timefun?: "linear" | "ease-in-out"
): Promise<void>
```

## 参数

参数一是**要移动的元素**，参数二是**目标父元素**，参数三是**元素要去到的位置**（默认值`"last"`）
，参数四是**动画完成的时间** `ms`（默认值400），参数五是**动画过度的时间曲线**，很多，这里只列举两个（默认值`"linear"`）

## 例子

``` js
game.$elementGoto(event.dialog.selectedCard, itemContainer, null, 300).then(() => {
	event.dialog.isBusy = false;
	updateButtons();
});
```
