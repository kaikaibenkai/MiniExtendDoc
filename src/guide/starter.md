---
title: 简介与约定
---



# MiniExtend 简介

MiniExtend 是一个为<span title="理论上支持任何使用迷你世界引擎的软件，例如迷你编程">迷你世界</span>脚本设计的 Lua 库，其主要目的是方便开发者使用开发者脚本开发游戏。

MiniExtend 使用 [MIT License](https://mit-license.org/) 作为许可证，它要求你保留作者的版权，侵权行为包括但不限于私自删除源代码中的作者信息。

## MiniExtend 的优势

### 简化开发

使用 MiniExtend 可以极大降低迷你世界脚本开发复杂度。

特别是 [MiniExtend UI](/api/ui.html) ，它允许以面相对象方式操作自定义 UI ，大大降低了调用函数时传递的参数数量。例如 `UI.Element:show` 函数，其对应的 API 为 `Customui:showElement` ，你甚至可以不传任何参数调用 `UI.Element:show` 并正确运行，而 API 却需要 3 个参数。

MiniExtend 使你将脚本代码集中在[全局作用域](/api/#全局作用域)下管理。
只需在 [UI 作用域](/api/#ui-作用域)下添加简短的脚本并稍作修改即可支持 [UI 事件](/api/#ui-事件)，无需在各个作用域之间来回切换。

原生 API 无法在全局作用域下绑定绑定 UI 事件，而 MiniExtend Event 做到了。

### 提高效率

MiniExtend 可提高脚本运行效率，例如访问全局变量的速度是默认的 <strong style="color: red;">22~35</strong> 倍。
例如，访问 20 个不同的不存在的全局变量，不使用 MiniExtend 的脚本花了 24.691s ，而使用 MiniExtend 的脚本只花了 1.014s（数据排除循环花费），几乎是瞬间完成，效率为默认的 <strong style="color: red;">2435%</strong> 。

多次实验得到的平均值：

> 原生 API ：17.48s
> 
> MiniExtend：0.66s
> 
> 运行效率：<strong style="color: red;">2658%</strong>

不要过于震惊，这就是实验事实，你也可以手动验证实验数据的真实性。

### 易于学习

学习和使用 MiniExtend 并不困难。

如果你会使用 Lua ，那么使用 MiniExtend 也很简单。

代码始终会包含详细的注释以便开发者阅读，MiniExtend 每次更新都会包含最新的正确详尽的官方文档，当然也还有其它可读性更高的文档可供选择。

现在 MiniExtend 的总文档数据量比其它任何迷你世界脚本库都要多的多，但无需因此恐慌，你只需阅读其中的一部分即可学习完 MiniExtend 的全部内容。

### 唯一选择

目前 MiniExtend 是**唯一的**免费开源的迷你世界 Lua 库，网上找到的其它公开的迷你世界代码往往没有明示许可。

但这并不是一件好事，它意味着许多开发者没有想过或不愿公开他们的代码，这会导致开发成本提升，整体开发水平停滞，不利于创建良好的游戏开发生态。

我们希望开发者们能像 MiniExtend 一样，愿意公开应该被公开的开发成果，共建良好迷你世界开源生态。

## 使用 MiniExtend

作者会维护比较准确详细的[开发文档](https://0-0000.github.io/MiniExtend/)，文档会指引你搭建 MiniExtend 环境和使用 MiniExtend 。

你现在看到是[@凯凯本凯](https://github.com/kaikaibenkai) 整理优化后的版本，同样[以 MIT License 开源](https://github.com/kaikaibenkai/MiniExtendDoc)。

> 如果你也想编写 MiniExtend 文档：
>
> **Fork** 一份 [MiniExtend 的 `docs` 分支](https://github.com/0-0000/MiniExtend/tree/docs) ，然后自由发挥！
>
> 如果你的文档内容完整且较为准确，你可以联系[作者](https://github.com/0-0000)推荐你的文档。

## 致开发者

无论你的 Pull Request 质量如何，很高兴你能为 MiniExtend 开发做贡献！

如果你想开发 MiniExtend 代码：

> - 把开发重点放在**基础**或**重要**的功能上。
> - 代码开头标记你的更改（或创建）。

## 附加链接

- [MiniWorld Development Organiztion](https://github.com/Mini-World-Dev-Org/)
迷你世界开发组织
- [MiniWorld Wiki](https://github.com/Mini-World-Dev-Org/Mini-World-Wiki/wiki/)
迷你世界 Wiki
- [MiniWorldGenv](https://github.com/Mini-World-Dev-Org/MiniWorldGenv/)
研究迷你世界的内部脚本环境 `genv` 。
- [Lua 教程 | 菜鸟教程](https://www.runoob.com/lua/lua-tutorial.html)
可通过该教程入门学习 lua 。
- [Lua 5.1 Reference Manual - contents](http://www.lua.org/manual/5.1/)
入门后建议通过该手册进阶学习 lua 。
- QQ群 [MiniExtend 开源库](https://jq.qq.com/?_wv=1027&k=PfLcOMQw)