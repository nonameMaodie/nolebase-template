---
tags:
  - lib
  - lib-element
---
[[GameEvent|事件]]实例的类型

## 类型

``` ts
type: string
```

一般用于判断。

## 例子

``` js
skill: {
	trigger: {
		player: "loseAfter",
		global: "loseAsyncAfter",
	},
	filter(event, player) {
		if (event.type != "discard") {
			return false;
		}
		return true;
	}
}
```

``` js
skill: {
	trigger: { player: ["damageBegin3", "loseHpBegin"] },
	filter(event, player) {
		if (event.name == "loseHp") {
			return event.type == "du";
		}
		return true;
	}
}
```

``` js
skill: {
	enable: "chooseToUse",
	filter(event, player) {
		if (event.type != "dying") {
			return false;
		}
		return true;
	}
}
```

``` js
skill: {
	enable: "chooseToUse",
	filter(event, player) {
		if (event.type == "wuxie") {
			return false;
		}
		return true;
	}
}
```