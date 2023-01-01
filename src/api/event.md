# MiniExtend Event

对应源文件：`event.lua`、`ui_main.lua`。

MiniExtend Event 的前身是 `ScriptSupportEvent` ，但更便于使用，支持 [MiniExtend Object](/api/object.html) 。

## 函数介绍

### `registerEvent`

```lua
---本函数检查参数！
---@param eventname string 事件名称
---@param callback fun():any 事件触发回调
---@param uiid 如果是 UI 事件，提供界面 ID
function registerEvent(eventname, callback [, uiid]) end
```

监听游戏事件，在 `eventname` 所描述的事件发生时，回调 `callback` 。
返回描述事件监听的 [`Register` 表](#register)，用于取消事件监听。

如果 `eventname` 所描述的事件为 [UI 事件](./document.html#UI-事件)，需要指定 `uiid` 需要监听的 UI 界面 id 。

监听 UI 事件会发生预绑定，常规流程（在第 0 帧和 `ui_main` 脚本前）不会真的监听事件，而是将信息存储在返回的 `Register` 表中（不含 `id` 值，暂存了临时值）。

在对应的 `ui_main` 脚本执行后会完成绑定并完成 `Register` 表（移除临时值，赋值 `id`），在中途修改返回的 `Register` 可能导致错误。

该函数是 MiniExtend Event 的核心。

该函数的原型为 `ScriptSupportEvent:registerEvent` 。

回调是以**保护模式**运行的，如果发生错误，会输出错误信息，带上自定义的错误头。

回调函数调用时，还会传递一个哈希表作为参数，以下简称 `param` ，存储着该事件发生时的一些信息。

`param` 就是 `ScriptSupportEvent:registerEvent` 回调函数传递的参数，详见[官方文档](https://developers.mini1.cn/wiki/event.html)。

但除此之外， `param` 还必包含一个 `register` ，表示事件监听对应的 `Register` 表。

回调函数前会提前隐式地设置 `objid` 的值为 `param["eventobjd"]` ，在回调结束后会恢复 `objid` 的值。

### `cancelRegisterEvent`

```lua
---@param register Register[] 需要取消的监听
function cancelRegisterEvent(register) end
```

取消事件监听。

## 自定义事件名

下文将事件名称为 `eventname` ，原始事件名称为 `msgStr` 。

需要使用 `msgStr` 作为参数调用 API ，但 MiniExtend 提供了更简短，可读性更高的自定义事件，于是就有了 `eventname` ，它作为调用 `registerEvent` 时实际传入的参数。

既可以使用 MiniExtend 自定义事件名，也可以使用 API 事件名。 

MiniExtend 自定义事件名在 `event.lua` 中的 `CustomEvents`（局部变量) 中定义。

如果 `eventname` 以 `"$"` 开头，则认为其为 MiniExtend 自定义事件名，否则为 API 事件名。

如果为 MiniExtend 自定义事件名，则 `msgStr = CustomEvents[eventname]` 。否则，`msgStr = eventname` 。

### 自定义事件名列表

- `$ui.onShow`：UI 界面显示，对应 `UI.Show` 。
- `$ui.onHide`：UI 界面隐藏，对应 `UI.Show` 。
- `$ui.onPress`：按钮按下，对应 `UI.Show` 。
- `$ui.onClick`：按钮点击，对应 `UI.Show ` 。
- `$ui.onLostFocus`：输入框失去焦点，对应 `UI.Show` 。

你会发现这些自定义事件名描述的都是 UI 事件，实际上通常通过 [MiniExtend UI](/api/ui.html) 监听这些事件。

::: tip 点击 = 按下 + 释放

按下事件不是延续性事件，只会在玩家开始按下的那一帧触发。

:::

### `Register`<Badge text="虚拟" type="warning"/>

::: tip

注意 `Register` 表是 MiniExtend 理论中的一个表，`_GScriptFenv_["Register"]` 为 `nil` 。

:::

`Register` 表是 `registerEvent` 的返回值，用于作为 `cancelRegisterEvent()` 的参数来取消事件监听。

不要修改 `Register` 表的值，仅将整个表作为参数调用 `cancelRegisterEvent()`，以下内容只是为了方便开发者。

| 成员键 | 类型 | 说明 | 可读 | 可写 |
| :--: | :--: | :---: | :---: | :---: |
| `id` | `integer` | 监听实例 ID | √ | |
| `uiId` | `string  |  nil` | UI 界面 ID ，非 UI 事件则为 `nil` | √ | |
| `msgStr` | `string` | API 事件名 | √ | |
| `callback` | `function` | 回调函数 | 预绑定时 | |
| `eventName` | `string` | 调用时传递的 `eventname` | 预绑定时 | |