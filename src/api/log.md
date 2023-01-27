---
title: 日志管理 Log
---



# MiniExtend Log

对应源文件：`Log.lua`。

这里的 Log 指日志：

![日志界面](/static/console.png)

在控制台输出的信息几乎是不可见的，但是日志中的信息是可见的，所以需要在日志输出信息。

::: tip

<!--图片自动维持长宽比，故只需设定宽度-->

在开发者工具中调整<img style="width: 52px;" :src="$withBase('/static/test-button.png')" />至图示状态来启用日志。

在玩法模式下按<img style="width: 20px;" :src="$withBase('/static/console-button.png')" alt="“！”按钮" />来打开日志窗口。

按<img style="width: 54px;" :src="$withBase('/static/clear-console.png')" alt="“清空日志”按钮" />清除日志。

:::

## 函数介绍

`Log` 作用域包含以下**静态**函数：

以下函数都会返回一个 `boolean` ，`true` 表示调用成功，`false` 表示调用失败。

### `log`

输出日志，以 `"global"` 为标签。

等价于 `Log.logtag("global", ...)` ，格式化方法与原生 Lua 中的 `print` 函数相同。

```lua
---@param ...data any 输出的内容
function Log.log(...) end
```

### `logtag`

以 `tag` 为标签输出 `...` 。

格式化方法与原生 Lua 中的 `print` 函数相同。

```lua
---@param tag string 标签
---@param ...data any 输出的内容
function Log.logtag(tag, ...) end
```

### `warn`

以黄色高亮输出警告信息。

```lua
---@param message string 警告信息
function Log.warn(message) end
```

### `error`

红色高亮输出错误信息，不抛出实际错误。

```lua
---@param message string 错误信息
function Log.error(message) end
```

### `clear`

清空日志。

```lua
function Log.clear() end
```

> 日志清空功能存在 bug 。
> 
> 例如在观测空的日志界面时日志输出一条信息的话，背景会正确变化，信息却不会显示，切换日志 tag 刷新显示的日志后会正确显示。