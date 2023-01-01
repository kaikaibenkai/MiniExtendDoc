---
title: 日志管理 Log
---



# MiniExtend Log

对应源文件：`Log.lua`。

---

## `Log` 作用域

这里的 Log 指日志：

![日志界面](/static/console.png)

在控制台输出的信息几乎是不可见的，但是日志中的信息是可见的，所以需要在日志输出信息。

::: tip

<!--图片自动维持长宽比，故只需设定宽度-->

在开发者工具中调整<img style="width: 52px;" src="/static/test-button.png" />至图示状态来启用日志。

在玩法模式下按<img style="width: 20px;" src="/static/console-button.png" alt="“！”按钮" />来打开日志窗口。

按<img style="width: 54px;" src="/static/clear-console.png" alt="“清空日志”按钮" />清除日志。

:::

## 函数介绍

`Log` 作用域包含以下**静态**函数:
以下函数都会返回一个 `boolean` ，`true` 表示调用成功，`false` 表示调用失败。　　

### `log`

```lua
---@param ... any 输出的内容
function Log.log(...)
  Log.logtag("global", ...); -- 等价
end
```

输出日志，以 `"global"` 为标签。

### `logtag`

```lua
---@param tag string 标签
---@param ... any 输出的内容
function Log.logtag(tag, ...) end
```

以 `tag` 为标签输出 `...` 。

格式化方法与 `print` 函数相同。

### `warn`

```lua
---@param message string 警告信息
function Log.warn(message) end
```

以黄色高亮输出警告信息。

### `error`

```lua
---@param message string 错误信息
function Log.error(message) end
```

红色高亮输出错误信息，不抛出实际错误。

### `clear`<Badge text="不稳定" type="warning"/>

```lua
function Log.clear() end
```

清空日志。