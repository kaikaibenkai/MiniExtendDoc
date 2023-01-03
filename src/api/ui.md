---
title: 界面管理 UI
---



# MiniExtend UI

对应源文件：`ui.lua`

MiniExtend UI 前身为 `Customui` ，但以面向对象方式描述界面与元件，且支持 [MiniExtend Object](./object.html) 。

## 约定

大多数 `UI` 下的函数都会包含一个 `playerid` 可选参数，它表示响应该操作的玩家 id 。该参数不会被检查，如果 `playerid` 的布尔值为 `false` ，则以 `objid` 代替。

`UI` 下的所有未表明返回值的函数或方法，一律返回 `true` 表示<span title='调用 API 成功不一定代表函数正确工作'>调用 API 成功</span>，返回 `false` 表示<u title='有任何一部分执行失败，即视为全部失败'>失败</u>。

## 函数介绍

### `UI.getRootSize`

获取回自定义 UI 界面的大小。

```lua
---@return float, float 宽和高
function UI:getRootSize() end
```

返回的是服务器上定义的 UI 界面大小，例如在多人游戏中调用 `UI:getRootSize()` 会返回房主（或者云服务器）的 UI 界面大小。

### `UI` 下的类之间的关系

| 类名 | 类描述 | 父类 | 具象 |
| :-: | :-: | :-: | :-: |
| UIView | UI 界面 | 无 | √ |
| Element | UI 元件 | 无 | × |
| Texture | UI 图片 | Element | √ |
| Button | UI 按钮 | Texture | √ |
| Label | UI 文字 | Element | √ |
| EditBox | UI 输入框 | Label | √ |

### `UI.UIView`

UI 界面，以下简称 `UIView` 。

### `UI.UIView:new`

构造一个 [`UIView`](#ui-uiview) 对象。

```lua
---@param uiid string 指定该 UI 界面的 ID（不检查可用性）
---@return 
function UI.UIView:new(uiid) end
```

函数返回值有缓存，如果 `uiid` 相同，返回之前构造的对象。

成员变量：

| 名称 | 类型 | 简介 | 可读 | 可写 |
| :-: | :-: | :-: | :-: | :-: |
| `id` | `string` | UI 界面 ID | √ | |
| `showEvent` | `function` \| `nil` | 界面显示时的回调 | √ | √ |
| `hideEvent` | `function` \| `nil` | 界面隐藏时的回调（ PC 端可按下 `ESC` 触发） | √ | √ |

成员函数：

```lua
---打开 UI 界面。
---@param playerid integer 目标玩家
---@return boolean 是否成功
function UI.UIView:show([playerid]) end

---隐藏（关闭）UI 界面。
---@param playerid integer 目标玩家
---@return boolean 是否成功
function UI.UIView:hide([playerid]) end
```

```lua
---修改该 UI 界面的所有子元件的状态。
---@param state string 界面状态
---@param playerid integer 目标玩家
---@return boolean 是否成功
function UI.UIView:setState(state [, playerid]) end
```

```lua
---构造一个 `UI.Texture` 对象。
---@param elementid string 元件 ID
---@return UI.Texture
function UI.UIView:newTexture(elementid) end

---构造一个 `UI.Button` 对象。
---@param elementid string 元件 ID
---@return UI.Button
function UI.UIView:newButton(elementid) end

---构造一个 `UI.Label` 对象。
---@param elementid string 元件 ID
---@return UI.Label
function UI.UIView:newLabel(elementid) end

---构造一个 `UI.EditBox` 对象。
---@param elementid string 元件 ID
---@return UI.EditBox
function UI.UIView:newEditBox(elementid) end
```

### `UI.Element`

UI 元件，以下简称 `Element` 。

该类是抽象类，不能直接构造 `Element` 对象，应构造 `Element` 子类的对象。

成员变量：

| 名称 | 类型 | 简介 | 可读 | 可写 |
| :-: | :-: | :-: | :-: | :-: |
| `uiView` | `UIView[]` | 所属 UI 界面 | √ | |
| `id` | `string` | 元件 ID | √ | |

成员函数：

```lua
---显示元件。
---@param elementid string 元件 ID
---@param playerid integer 目标玩家
---@return boolean 是否成功
function UI.Element:show([playerid]) end

---隐藏元件。
---@param elementid string 元件 ID
---@param playerid integer 目标玩家
---@return boolean 是否成功
function UI.Element:hide([playerid]) end
```

```lua
---显示或隐藏元件。
---@param display boolean 为真，显示；否则，隐藏
---@param playerid integer 目标玩家
---@return boolean 是否成功
function UI.Element:setDisplay(display [, playerid]) end
```

```lua
---修改元件状态。
---@param state string 状态
---@param playerid integer 目标玩家
---@return boolean 是否成功
function UI.Element:setState(state [, playerid]) end
```

```lua
---修改元件位置。
---@param x number 位置 x 坐标
---@param y number 位置 y 坐标
---@param playerid integer 目标玩家
---@return boolean 是否成功
function UI.Element:setPosition(x, y [, playerid]) end
```

```lua
---修改元件大小。
---@param width number 元件宽度
---@param height number 元件高度
---@param playerid integer 目标玩家
---@return boolean 是否成功
function UI.Element:setSize(width, height [, playerid]) end
```

```lua
---将原始元件（旋转0°）绕其位置顺时针旋转 `angle` 度。
---@param angle number 旋转度数
---@param playerid integer 目标玩家
---@return boolean 是否成功
function UI.Element:setAngle(angle [, playerid]) end
```

```lua
---@param color integer 元件 RGB 颜色（ `0x000000` ~ `0xffffff` ）
---修改元件颜色。
---@param playerid integer 目标玩家
---@return boolean 是否成功
function UI.Element:setColor(color [, playerid]) end
```

```lua
---修改元件透明度。
---@param alpha number 元件透明度（0为完全透明，100为完全不透明）
---@param playerid integer 目标玩家
---@return boolean 是否成功
function UI.Element:setColor(alpha [, playerid]) end
```

### `UI.Texture`

UI 图片元件，以下简称 `Texture` 。

```lua
---构造一个 Texture 对象。
---@param uiview UI.UIView[] 所属界面
---@param elementid string 元件 ID
---@return UI.Texture
function UI.Texture:new(uiview, elementid) end
```

等价于 `uiview:newTexture(elementid)` 。

成员函数：

```lua
---修改图片纹理。
---@param url string 图案纹理 ID
function UI.Texture:setTexture(url [, playerid]) end
```

游戏内可以通过 “ID库” → “图片” 获取图案纹理 ID 。

### `UI.Button`

UI 按钮元件，以下简称 `Button` 。

```lua
---构造一个 Button 对象。
---@param uiview UI.UIView[] 所属界面
---@param elementid string 元件 ID
---@return UI.Button
function UI.Button:new(uiview, elementid) end
```

等价于 `uiview:newButton(elementid)` 。

成员变量：

| 名称 | 类型 | 简介 | 可读 | 可写 |
| :-: | :-: | :-: | :-: | :-: |
| `pressEvent` | `function` \| `nil` | 按下按钮时的回调 | √ | √ |

### `UI.Label`

UI 文字元件，以下简称 `Label` 。

```lua
---构造一个 Label 对象。
---@param uiview UI.UIView[] 所属界面
---@param elementid string 元件 ID
---@return UI.Label
function UI.Label:new(uiview, elementid) end
```

等价于 `uiview:newLabel(elementid)` 。

成员函数：

```lua
---修改显示文本。
---@param text string 文本（不会被屏蔽，但不能太长）
---@param playerid integer 目标玩家
---@return boolean 是否成功
function UI.Label:setText(text [, playerid]) end
```

```lua
---修改字体大小。
---@param size integer 字体大小
---@param playerid integer 目标玩家
---@return boolean 是否成功
function UI.Label:setFontSize(size [, playerid]) end
```

### `UI.EditBox`

UI 输入框元件，以下简称 `EditBox` 。

```lua
---构造一个 EditBox 对象。
---@param uiview UI.UIView[] 所属界面
---@param elementid string 元件 ID
---@return UI.EditBox
function UI.EditBox:new(uiview, elementid) end
```

等价于 `uiview:newEditBox(elementid)` 。

成员变量：

| 名称 | 类型 | 简介 | 可读 | 可写 |
| :-: | :-: | :-: | :-: | :-: |
| `lostFocusEvent` | 输入框失去焦点（输入完成）时的回调 || √ | √ |

回调传递的输入框内容，会先被和谐处理。

### 代码示例

### 背景

已知一 UI 界面 `u` 下有一个输入框元件 `e` 和一个文字元件 `l` ，要求如下：

- 在玩家没有输入 `e` 时， `l` 总是显示 `"输入 16 进制数"` ，开始输入时 `e` 的内容清空 。
- `e` 完成输入时，尝试将其解释为 16 进制数，并转化为 10 进制数，并将结果反应在 `l`上。
- 如果输入不合法，无法将其转化，则设置 `l` 的文本为 `"错误的输入"` 。

### 分析

- 要在开始输入时清空 `e` 的内容，需要处理一个 `$ui.onClick` 事件。实现方法是再创建一个按钮元件，设为 `b` ，将 `e` 设为 `b` 的子集，调整 `b` 的外观并将使其刚好覆盖 `e` ，这样当玩家点击输入框开始输入时，也会触发按钮的点击事件(`clickEvent`)，这时就能修改 `e` 的内容了。
- 经测试，当按钮被点击时焦点才会进入输入框，所以不要使用按下事件。
- 使用 `tonumber(e [, base])` 函数来转化数字。

### 代码

```lua
Env.__init__()
-- 需替换以下 id
local uiview_id = [[]]
local editBox_id = [[]]
local label_id = [[]]
local button_id = [[]]
uiview = UI.UIView:new(uiview_id)
editBox = uiview:newEditBox(editBox_id)
label = uiview:newLabel(label_id)
button = uiview:newButton(button_id)
-- 初始化
registerEvent([[Game.AnyPlayer.EnterGame]], function(param)
	uiview:show()
	label:setText("")
	editBox:setText("输入 16 进制数")
end)
function button.clickEvent(param)
	-- 清空输入框
	editBox:setText("")
end
function editBox.lostFocusEvent(param)
	local num = tonumber(param["content"], 16)
	if num then
		label:setText(tostring(num))
	else
		label:setText("错误的输入")
	end
	editBox:setText("输入 16 进制数")
end
```