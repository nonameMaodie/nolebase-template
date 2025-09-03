---
tags:
  - lib
  - tofix
date: 2025-07-24
---
一个对象，这里存放着函数钩子，其格式为：

``` js
{
	addGroup: [func1, func2],
	addNature: [func3, func4],
	checkBegin: [func5, func6],
	...
}
```

你也可以往这里加入 { 钩子名 : 函数数组 }，并在数组里增加你的自定义函数。调用钩子函数的用法请参照 [[game.callHook()]] ，可以将hook机制类比为`event.trigger()`，但是这里只能放同步代码。


当前无名杀已有的钩子名如下：

``` js
[
	"addGroup",
	"addNature",
	"checkBegin",
	"checkCard",
	"checkTarget",
	"checkButton",
	"checkEnd",
	"uncheckBegin",
	"uncheckCard",
	"uncheckTarget",
	"uncheckButton",
	"uncheckEnd",
	"checkOverflow",
	"checkTipBottom",
	"checkDamage1",
	"checkDamage2",
	"checkDamage3",
	"checkDamage4",
	"checkDie",
	"checkUpdate",
	"checkSkillAnimate",
	"addSkillCheck",
	"removeSkillCheck",
]
```
