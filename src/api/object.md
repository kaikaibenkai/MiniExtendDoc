---
title: 对象管理 Object
---



# MiniExtend Object

对应源文件： `object.lua` 。

MiniExtend Object 主要是为 [MiniExtend Event](./event.html) 和 [MiniExtend UI](./ui.html) 设定的。

目前 MiniExtend Object 内容较少，且主要以理解为主。

Object 的核心是一个叫 `objid` 的虚有值（其实是局部变量），`objid` 默认为 0 。

在调用 MiniExtend 的一些函数时， `objid` 会作为游戏对象 id 参数（参数名以*id*结尾，如 `playerid` ）的默认值。

## 注意事项

在延时调用中是不会有正确的 `objid` 值的，在 `registerEvent` 回调中要特别注意。

因为在发动延时调用后，回调函数会正常结束，然后会恢复 `objid` 的值，这时已经失去了理想的 `objid` 的值。

## 函数介绍

以下函数都是在 `_GScriptFenv_` 中的全局函数。

### `getObjectId`

```lua
---@return any 取到的值
function getObjectId() end
```

获取 `objid` 的值。

### `setObjectId`

```lua
---@param objectid integer 要设置的值
function setObjectId(objectid) end
```

设置 `objid` 的值。

## 代码示例

::: tip

对于 `UI.UIView:show` 函数，你可以不传递 `playerid` 参数调用该函数，这时 `objid` 将代替 `playerid` 参数。

在 `registerEvent` 的回调中， `objid` 会被临时地设置为 `param["eventobjid"]` 。

:::

实例功能：玩家进入游戏后打开 `uiview` 界面。

```lua
Env.__init__()

---UI 界面 id
local uiid = [[]]

---引用对应的 UI 界面
uiview = UI.UIView:new(uiid)

registerEvent([[Game.AnyPlayer.EnterGame]], function(param)
	-- objid 已被隐式地设置为 param.eventobjid
	-- 等价于调用 uiview:show(getObjectId())
	uiview:show()
end, uiid)
```