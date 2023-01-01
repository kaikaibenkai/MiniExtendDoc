---
title: 简介与约定
---



# MiniExtend 简介

MiniExtend 是一个为<span title="理论上支持任何使用迷你世界引擎的软件，例如迷你编程">迷你世界</span>脚本设计的 Lua 库，其主要目的是方便开发者使用开发者脚本开发游戏。

MiniExtend 使用 MIT Licence 作为许可证，它要求你保留作者的版权，侵权行为包括但不限于私自删除源代码中的作者信息。

（详见 [MiniExtend 的优势](../README.md#使用-MiniExtend-的理由)）

# 约定

## 定义

### UI 事件

发生在 UI 界面中的事件就是该 UI 界面的事件，简称 UI 事件。

UI 事件包括：

- 界面显示 `$ui.onShow`
- 界面隐藏 `$ui.onHide`
- 按钮被按下 `$ui.onPress`
- 按钮被点击 `$ui.onClick`
- 输入框失去焦点 `$ui.onLostFocus`"

### 脚本作用域

脚本文件所处的位置就是脚本文件的作用域。

所处的位置和脚本文件在文件结构上的位置有关，例如有的脚本文件在<span title="这里包括子文件夹，后同">地图文件夹</span>中，而有的在 UI 界面文件夹中。

所处的位置决定了脚本在什么时候执行，以及脚本执行时的环境。

需要注意，不同的脚本作用域下的脚本的[环境](http://www.lua.org/manual/5.1/manual.html#2.9) `_GScriptFenv_`（`_G`）是不同的，所以**不同作用域下的脚本不共享全局变量**，但有其它方式来让它们交流（详见 [core](../api/core) ）。

一般将作用域分为全局作用域、UI 作用域。

### 全局作用域

该脚本文件位于地图文件夹中，辨别方式是**在脚本编辑器创建**，性质是该作用域下的文件总是在开发者<span title="这里忽略插件包，因为没有严谨测试">可编辑脚本</span>中最先执行。

### UI 作用域

该脚本文件位于自定义 UI 界面文件夹中，辨别方式是**在 UI 编辑器中创建**，性质是该作用域下的文件晚于全局作用域下的脚本文件执行。

该作用域下的脚本可以使用 `ScriptSupportEvent` 监听所属 UI 界面的事件，但全局作用域和其它 UI 作用域无法使用它们的 `ScriptSupportEvent` 来监听该 UI 界面的事件。

### 游戏帧

详见 [wiki](https://github.com/Mini-World-Dev-Org/Mini-World-Wiki/wiki/mechanism-tick) 。


## 代码规范

非常重要！

- 所有含可变参数 `...` 的 MiniExtend 函数都会正常解析 `...` 开头和中间的 `nil` ，但末尾的 `nil` 是意外。

  原因是发生了数据碰撞，无论在末尾添加几个 nil ，函数内得到的输入都是相同的。

  例如 `Log.log(nil, 8, nil)` 输出 `nil` 和 `8`，但函数不知道第 3 个参数的存在，所以第 3 个 nil 不会输出。

- 总是使用 `.` 访问作用域的静态函数，使用 `:` 访问类函数（包括构造函数）。

- 除非明确指明可以修改对象属性或修改它的作用，不要修改这个属性，否则可能导致错误。

- MiniExtend 没有严格的保护函数和对象属性的机制，意外地修改（例如传入类型错误地实参）可能导致错误。

- 除非有特别说明会检查你调用函数时的参数，MiniExtend 不会检查参数是否正确，请务必保证实参的正确性！

---

## 代码实例

本文档包含了一些代码实例，让它们正确运行的前提是你已经**搭建好 MiniExtend 环境**。

首先必须注意使用 `Env.__init__()` 初始化脚本环境。

对于与 UI 相关的实例，要求搭建 UI 环境，在代码开头会列出一些局部变量，表示 UI 界面或元件的 id ，不要忘记**替换它们**！

---

### 文档图片

文档中的图片可能**不完全适用于新版本的 MiniExtend** ，也就是这些图片可能来自就版本的 MiniExtend 但没有同步更新。

每次都同步更新这些图片实在是太复杂了，但这些来自旧版本的图片与理想的新图片差距不会太大。

如果你发现图片严重不符实际，可以 [Create issue](https://github.com/0-0000/MiniExtend/issues/new) 通知开发者。

---

### 关键字

以下是 MiniExtend 关键字，不要使用它们作为自己的标识符。

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

全局作用域/类：

- `Env`
- `Log`
- `Timer`
- `UI`