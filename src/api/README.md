---
title: 简介与约定
---



# API 风格&约定

## 代码规范

- MiniExtend 的所有函数都无法获得 `...` 末尾的 `nil` 。
- 使用 `.` 访问作用域的静态函数，使用 `:` 访问类函数。
- 未明确声明可写的成员变量，不要修改，否则可能导致错误。
- 未明确声明检查参数时，MiniExtend 不检查参数，因此请保证参数正确。

## 定义

### 脚本作用域

脚本文件所处的位置叫做脚本文件的作用域。

所处的位置和脚本文件在文件结构上的位置有关，例如有的脚本文件在<span title="这里包括子文件夹，后同">地图文件夹</span>中，而有的在 UI 界面文件夹中。

所处的位置决定了脚本在什么时候执行，以及脚本执行时的环境，这就是定义它的意义。

::: warning

不同的脚本作用域的 `_G` 是不同的，所以**不同作用域下的脚本是隔离的**，但有其它方式来让它们交流。

更多内容详见 `core.lua` 。

:::

在 MiniExtend ，一般将作用域分为两类：[全局作用域](#全局作用域)、[UI 作用域](#ui-作用域)。

## 全局作用域

全局作用域下的脚本**在脚本编辑器创建**。

该脚本文件位于地图文件夹中，其性质是该作用域下的文件总是在<span title='这里指开发者可编辑的脚本，可能不包含插件包，这有待测试'>可见脚本</span>中最先执行。

## UI 作用域

UI 作用域下的脚本**在 UI 编辑器中创建**。

该脚本文件位于自定义 UI 界面文件夹中，其性质是该作用域下的文件晚于全局作用域下的脚本文件执行。

UI 作用域下的脚本可以使用 `ScriptSupportEvent` 监听所属 UI 界面的事件，全局作用域和其它 UI 作用域无法使用 `ScriptSupportEvent` 。
 
::: tip 这可能很难理解

没关系！你只需知道如何区分全局作用域和 UI 作用域即可！

除非你是 MiniExtend 代码仓库贡献者，那样的话建议你理解它！

:::

### 游戏帧

游戏帧是游戏的基本事件单位，游戏帧从 0 开始（以下简称“帧”）。

游戏数据每帧改变一次。

从第 0 帧开始，平均每 **0.05秒** 会进入新的游戏帧。每个游戏帧的时长是无规律，不确定的，这可能与游戏的计算量有关。

可以近似的把 1 帧当做 0.05s ，但要知道它并不是真正的 0.05s 。记住，不要利用游戏帧来做精密的计时器，类似的功能可以用 `os.clock` 替代。

对于延续性事件（例如玩家挖掘方块），每一帧会触发一次事件，这也体现了游戏帧的意义。

### `genv`

见 [wiki](https://github.com/Mini-World-Dev-Org/Mini-World-Wiki/wiki/script) 。

### `_GScriptFenv_`

见 [wiki](https://github.com/Mini-World-Dev-Org/Mini-World-Wiki/wiki/script) 。

### UI 事件

自定义 UI 中才会发生的事件，原生 UI 事件包括：

- `UI.Show`
- `UI.Hide`
- `UI.Button.TouchBegin`
- `UI.Button.Click`
- `UI.LostFocus`

对于原生 UI 事件，建议使用[自定义事件](/api/event#自定义事件名)代替，提升代码可读性。

## 代码示例

本文档包含了一些代码示例，让它们正确运行的前提是你**已经**搭建好 MiniExtend 环境，有的代码可能会有其它要求。

::: tip

对于 UI 示例，在代码开头会列出一些局部变量，表示 UI 界面或元件的 ID ，**不要忘记替换它们**！

:::

## 关键字

以下是 MiniExtend 关键字，避免意外地使用它们作为自己的标识符，改变脚本添加的函数的意义不会影响 MiniExtend 的使用，但有的 API 可能会使用这些函数从而导致脚本无法正常运行。

全局函数：

- `loadstring2`
- `deepcopy`
- `getTick`
- `scheduleCall`
- `nextTick`
- `cancelScheduleCall`
- `getObjectId`
- `setObjectId`
- `registerEvent`
- `cancelRegisterEvent`

全局表：

- `genv`
- `_G2`

全局作用域、类：

- `Env`
- `Log`
- `Timer`
- `UI`