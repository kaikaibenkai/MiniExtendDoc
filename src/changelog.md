---
title: 更新日志
---



# MiniExtend 更新记录

::: tip

此处的最新版本是文档的支持版本，更新不一定及时。

你可以查看[官方文档](https://github.com/0-0000/MiniExtend/blob/master/CHANGELOG.md)以获取最新版本。

:::

# v3

## v3.0.3<Badge>最新</Badge>

commit：[b97c68e](https://github.com/0-0000/MiniExtend/commit/b97c68ea6e42388da0c12d4ac8fe302c028c2bd6)

tag：[Release-3.0.3](https://github.com/0-0000/MiniExtend/releases/tag/Release-3.0.3)

- 修复了使用 `scheduleCall` 延时调用的函数在等待帧数超过 1 时无法正常倒计时的问题
- 优化部分文档

## v3.0.2

commit：[a83cd5e](https://github.com/0-0000/MiniExtend/commit/a83cd5efb1d58085d3abf7ee2c11df50d158acf9)

- 修复了 `event.lua` 错误使用 `table.concat` 函数的问题
- 文档统一转为 HTML

## v3.0.1

commit：[59df44f](https://github.com/0-0000/MiniExtend/commit/59df44f174f177e24aef27f1d28460e54585f3c6)

- 修复 `event.lua` 中 `_G2["__MiniExtend__sendSSEObject"]` 函数中使用中文逗号的问题
- 优化 `log.lua` 的注释
- 修正 `environment` 文档中的代码

## v3.0.0

commit：[a0744e1](https://github.com/0-0000/MiniExtend/commit/a0744e17c804a4e1110fd80a6b96a7376a9458ba)

- <Badge type='error' vertical='middle'>重要</Badge>
  **访问静态函数的操作符由 `:` 变为 `.`**
- `_G2` 表中 MiniExtend 占用的键值名称由 `"__*"` 变为 `"__MiniExtend__*"` 
- 重置了部分函数使之更高效
- 优化了代码的注释
- 文档同步代码更改
- 修改部分文档
- 新增 `Env` 作用域
- 删除 `GameVM` 表
- 新增 `loadstring2` 函数，比 `LoadLuaScript` 更高效，效果基本相同（但是不会在安全调用返回的函数仍然报错）
- 新增 `deepcopy` 函数，比 `copy_table` 更高效
- MiniExtend Console 改为 MiniExtend Log
- `Log` 作用域中的函数新增表示调用结果的 `boolean` 返回值
- 移除 `Time` 作用域，成员成为全局变量
- 延时调用函数（`scheduleCall` 和 `nextTick`）返回一个 id 表示延时调用的 id ，用于取消延时调用
- 新增 `cancelScheduleCall` 函数，用于取消一个延时调用
- 修复延时调用传参时在 `nil` 处截断的问题
- 优化 `objid` 的实现，不再使用 `_G2["__OBJID"]` 存储 `objid`
- 限制 `setObjectId` 参数类型为 `number`
- 移除 `Event` 作用域，成员成为全局变量（ `CustomEvents` 成为局部变量）
- 使用 `Register` 类代替 `Connecter` 类
- 使用 `registerEvent` 代替 `Connecter` 类的构造函数
- 使用 `cancelRegisterEvent` 代替 `Connecter` 类的析构函数
- 新增事件监听回调前设置 `objid` 的值，回调结束后恢复 `objid` 的值
- 以保护模式回调，拦截错误并添加信息再抛出
- <Badge type='error' vertical='middle'>重要</Badge>
  **`MUI` 作用域改回 `UI` 作用域**
- 修复 `UI.Texture:setTexture` 函数的一些问题

# v2

## v2.1.0

commit：[c8b5c76](https://github.com/0-0000/MiniExtend/commit/c8b5c7610927df501b7b2a922a13c17232a5114b)

tag：[Release-2.1.0](https://github.com/0-0000/MiniExtend/releases/tag/Release-2.1.0)


- 修复部分文件未删除合并标识的问题
- 修复 `object.lua` 中仍使用 `_LUAG` 的问题
- 修复文档引用静态资源失败的问题
- `Time.Timer` 类增加 `delay` 成员变量，表示下一次调用 `callback` 的间隔帧数
- <Badge type='error' vertical='middle'>重要</Badge>
  **将 `UI` 作用域改为 `MUI` 作用域**
- 优化文档

## v2.0.2

commit：[efa3442](https://github.com/0-0000/MiniExtend/commit/efa344254d0bc55fa31d0b2babbf78a97f9f1d50)

- 修复了 [v2.0.1](#v2-0-1) 发布时一些文件中没有及时更新版本的问题
- 修复了 `UI.Texture` 类被设置为全局变量的问题
- 在文档中增加以前未提出的 `UI:getRootSize` 函数

## v2.0.1

commit：[9eeab2f](https://github.com/0-0000/MiniExtend/commit/9eeab2fdea510f3139a623e49be6dffad5312440)

- 修改 `ide.lua`
- 使用 MIT Licence 许可证
- 增加了引用文档的仓库链接

## v2.0.0

commit：[fdf5ac9](https://github.com/0-0000/MiniExtend/commit/fdf5ac91f84e602203248ef00c39d806444a478a)

- 修改自述文件
- 使用 Apache Licence 2.0 许可证，并在代码开头添加作者信息
- MiniExtend 代码不再使用 MarkdownPad 风格文档
- 分离文档让开发者自己维护自己风格的 MiniExtend 文档
- 优化代码，原则为**空间换时间**
- 添加更详细的注释
- 删除 `_LUAG`
- 新增 `GameVM` 全局变量，用于存储游戏 API 对象
- 修改 `_G` 元表的 `__index` 函数，使其能更快索引不在 `_G` 中的键
- 新增 `Time` 作用域，将全局函数 `getTick`, `scheduleCall` 和 `nextTick` 转移到 `Time` 作用域中
- 新增计时器类 `Time.Timer`
- 优化 `Console` 作用域下的函数
- `_G2["__SENDSSE"]` 修改为 `_G2["__CONNECT"]`
- 修改 MiniExtend 自定义事件名的名称，并且使用 `"$"` 标记 MiniExtend 自定义事件
- 将 `Event.Listener` 更改为 `Event.Connecter`
- 将 `Event:connect` 函数更改为 `Event.Connecter:new`
- 修改 `CustomUI` 作用域名称为 `UI` ，使用 `GameVM.UI` 来访问因此被顶替的 UI API
- 在 `ui.lua` 中将 `Image` 和 `Text` 分别更改为 `Texture` 和 `Label` ，即修改了相应的类名和函数名
- 允许通过 `UI.$CLASSNAME:new` 构造 `$CLASSNAME` 对象（ `$CLASSNAME` 包含 `Texture` 、`Button` 、`Label` 、`EditBox` ）

# v1

## v1.0.1

commit：[571a979](https://github.com/0-0000/MiniExtend/commit/571a979cd0c5c10a34179aa293b5b428a2e8e80b)

- 修改自述文件
- 更新 MarkdownPad 风格文档
- 删除了 `order.md`
- 修复了 `scheduleCall` 函数在 `ticks` 大于 1 时会多等待 1 帧的问题
- `_G2["__newUI"]` 替换为 `_G2["__SENDSSE"]` ，并同步修改
- `_G2["__objid"]` 替换为 `_G2["__OBJID"]` ，并同步修改
- `useObjectId()` 函数的名称替换为 `setObjectId()` ，并同步修改
- 修复其它小 bug

## v1.0.0

- 正式发布 MiniExtend

# v0

非正式版本。