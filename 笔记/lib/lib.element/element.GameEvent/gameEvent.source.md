---
tags:
  - lib
  - lib-element
---
[[GameEvent|事件]]实例的来源

## 类型

``` ts
source: Player
```

`"damage"`,`"dying"`,`"die"` 等事件具备此属性，用于判断。

## 例子

``` js
skill: {
	trigger: {
		global: ["damageEnd", "olhuaquan_wrong"],
	},
	forced: true,
	filter(event, player) {
		if (event.name == "damage") {
			return event.source == player &&
			 event.player != player && event.player.isIn();
		}
		return event.target.isIn();
	},
}
```

``` js
skill: {
	trigger: { player: "damageBegin4" },
	filter(event, player) {
		return event.source && event.source.isIn();
	},
}
```

``` js
skill: {
	trigger: { global: "dieBegin" },
	check(event, player) {
		if (event.source && event.source.isIn() &&
		 get.attitude(player, event.source) > 0 &&
		  player.identity == "fan") {
			return false;
		}
		return get.attitude(player, event.player) > 3.5;
	},
}
```

