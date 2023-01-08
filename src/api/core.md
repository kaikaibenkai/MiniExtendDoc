---
title: 核心环境 Core
---



# MiniExtend Core

对应源文件：`core.lua`。

MiniExtend 已尽可能恢复标准 Lua 环境，这意味着你能正常调用那些本来被"删除"的基本函数。

以下是相对标准 Lua 环境有所不同的部分（前提是执行了 `Env.__init__()`）：

### `print`

```lua
function print(...) end
```

由于控制台是不可见的，在脚本中 `print()` 函数被重定向到日志，MiniExtend 保留了这个更改，虽然进行了重写。

### `loadstring2`

```lua
function loadstring2(string [, chuckname]) end
```

环境为 `_GScriptFenv_` 的 `loadstring()` 函数。

::: tip

由于 `genv["_GScriptFenv_"]` 是脚本环境，因此函数中可以通过 `_GScriptFenv_` 访问脚本环境，从而进一步访问 MiniExtend 。

:::

### `Env.__init__`

初始化脚本运行环境。

```lua
function Env.__init() end
```

**在每个自定义脚本开头都要最先执行**，不包括 UI 作用域中的 `ui_main.lua` ，否则可能导致脚本效率降低，出现错误。

（实际上就是把脚本对应的函数得环境设为 `_GScriptFenv_` ）

事实上，脚本环境不是真正的 `_GScriptFenv_` ，而是内部定义的一个表，这个表首先索引游戏 API （例如 Game 、Customui），然后才去索引 `_GScriptFenv_` 。

并且每个脚本都会有不同的这样的环境，因为每个脚本都对应一个函数，而调用脚本即调用该函数，在此之前会设置环境。

### `Env.indexAPI`

获取游戏 API 对象，最好使用一个局部变量存储返回值。

函数会访问新的 `_GScriptFenv_` ，但只应用于获取游戏 API 对象。

由于 `Env.__init__()` 把脚本对应的函数的环境设为 `_GScriptFenv_` ，因此无法直接访问游戏 API ，使用该函数访问游戏 API 。

```lua
---@param key string 访问的对象名称
---@return any 访问结果
function Env.indexAPI(key) end
```

### `Env.index`

使用旧的 `_GScriptFenv_`（实际上已经不存在）索引全局变量，例如脚本常量、 `printtag` 、`warn` 等函数。

这等同于以常规方式访问不为游戏 API 的全局变量。

```lua
---@param key string 索引的键
---@return any 引索结果
function Env.index(key) end
```

### 访问范围表

| 方式 | `_GScriptFenv_` 中的值 | 旧的 `_GScriptFenv_` 可访问的值 | 游戏 API
| :-: | :-: | :-: | :-: |
| 直接访问(`_G`) | √ | | |
| `Env.indexAPI(key)` | √ | | √ |
| `Env.index(key)` | | √ | |


### `genv`

该表表示迷你世界脚本内部环境，想知道它包含什么可以遍历它，但要注意该表包含上万个键。

开发者编辑的脚本的环境 `_GScriptFenv_` 不同于 `genv` ，但 `genv["_GScriptFenv_"]` 就等于这个 `_GScriptFenv_` ，也就是可以以此来通过 `genv` 访问脚本环境。

在使用 `Env.__init__()` 之前，脚本函数本身的环境是一个特殊的表，它首先控制对游戏 API 的访问，然后才去索引 `_GScriptFenv_` 。

### `_G2`

在任何可能的环境下(包括 `_GScriptFenv_` 和 `genv`)，只要脚本在 `core` 脚本后运行都可以访问该表，因此可以通过 `_G2` 来在各个脚本之间交换数据。

不要使用 `"__MiniExtend__"` 开头的变量来作为自己的索引，因为 MiniExtend 使用这种标识符作为索引存储一些变量，你可以遍历 `_G2` 来确定 MiniExtend 使用了哪些标识符。

### `deepcopy`

深拷贝一个 `table`。

```lua
---@param table table 要深拷贝的表
---@return table 拷贝结果
function deepcopy(table) end
```

基本等价于 `copy_table(table)`，使用它替代 `copy_table` 。

## 注意事项

迷你世界不是简单的 lua 解释器，有时脚本还会在服务器上运行，因此一些 lua 基本函数可能有歧义。

可以近似的认为开发者脚本在服务器上运行，因此 `io`, `package`, `os.execute` 等访问操作系统的函数的行为取决于服务器。

对于任何访问系统的函数，例如 `io`, `package`, `os.execute` ，它们大多本是被删除的函数和库，其行为取决于游戏的服务器。

- 在单人模式下，服务器就是玩家的设备。
- 由房主开启的多人游戏，服务器是房主的设备。
- 开发者申请的云服，服务器是迷你世界管理该云服的服务器（未证实）。

::: danger 某些基本函数可能很危险

必须注意这些危险行为，以下是一些例子：

- `io.close()` 导致<span title="这似乎会使游戏保存，再次打开地图仍是玩法模式，然后继续关闭……所以请使用编辑模式打开">程序突然关闭</span>。
- 在 Windows 下执行 `os.execute("cmd")` ，会弹出一个 cmd 窗口，而且该窗口会**阻塞**程序运行，除非手动关闭。

:::