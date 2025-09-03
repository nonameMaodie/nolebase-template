---
tags:
  - game
date: 2025-07-24
---
通用的调用钩子函数。调用此方法时，会按顺序将 [[lib.hooks]] 对应钩子名的数组中的每个函数运行一遍。

## 类型

``` ts
game.callHook<HookType extends NonameHookType, Name extends keyof NonameHookType>(
	name: Name,
	args: Parameters<HookType[Name]>
): void
```

## 参数

参数一是**一个钩子名字符串**，参数二是**一个可迭代的对象，作为钩子函数所需要传入的参数**。

## 例子

``` js
game.callHook("checkDamage1", [event, player]);
```

``` js
game.callHook("checkSkillAnimate", [this, name, popname]);
```

<template>
  <div class="box">
    <input 
       type="text" 
       :value="abc" 
       @input="emit('update:abc',$event.target.value)"
    >
  </div>
</template>
