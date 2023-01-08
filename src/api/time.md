---
title: 游戏时间 Time
---



# MiniExtend Time

对应源文件： `time.lua` 。

## 函数介绍

### `getTick`

获取当前是第几游戏帧。

```lua
---@return integer
function getTick() end
```

### scheduleCall

延时调用。

```lua
---@param ticks integer 多少游戏帧后执行回调
---@param func fun():any 执行的回调
---@param ...data any[] 执行回调的带的参数
---@return integer | false 成功时返回延时调用的 ID ，失败返回 false
function scheduleCall(ticks, func, ...) end
```

### `nextTick`

下个游戏帧执行回调。

```lua
---@param func fun():any 下个游戏帧执行的回调
---@param ...data any[] 执行回调的带的参数
function nextTick(func, ...) end
```

等价于 `scheduleCall(1, func, ...);` 。

::: tip

如果延时回调中再执行延时，会从下一帧开始倒计时。

:::

### `cancelScheduleCall`

取消延时调用。

```lua
---@param id integer 延时调用的 ID
---@return boolean 调用是否成功
function cancelScheduleCall(id) end
```

## 类介绍

### `Timer`

`Timer` 类，即计时器类，以下简称 `Timer` 。

注意，计时器运行时间晚于延时调用函数（`scheduleCall`）

在延时回调函数中启动计时器会使计时器在那一帧运行。

`delay` 值会递减，然后判断 `delay` 并调用 `callback` ，调用 `callback` 并重置 `delay` 。

构造函数：使用 `Timer:new()` 构造一个 `Timer` 对象，计时器默认是**暂停**的。

析构函数：使用 `Timer:delete()` 使计时器永久停止。

成员变量：

| 名称 | 类型 | 简介 | 可读 | 可写 |
| :-: | :-: | :-: | :-: | :-: |
| `createTime` | `number` | 对象创建时的**CPU 时间**| √ | |
| `tick` | `integer` | 计时器**累计运行**的游戏帧数 | √ | |
| `delay` | `integer` | 下次调用 `callback` 的间隔帧数（≤0时恢复为1） | √ | |
| `callback` | `function` \| `nil` | `delay` ≤0时的回调 | √ |  |

执行回调时传递两个参数：`self`（计时器对象） 和 `tick` （当前游戏帧数）。

成员函数：

```lua
---启动计时器。
---@return boolean 是否成功（调用前是否暂停）
function Timer:start() end
```

```lua
---暂停计时器。
---@return boolean 是否成功（调用前是否启动）
function Timer:pause() end
```

```lua
---获取计时器是否启动。
---@return boolean 计时器是否启动
function Timer:isRunning() end
```

## 代码示例

### 背景

定义 `tick` 表示当前游戏帧， `i` 的值为 2 。

从第一帧开始，每隔两帧执行以下操作：

- 在日志输出当前帧，以及 `Fibonacci[i]` 的值。
- 将 `i` 的值加 1 。

`i` = 50 后，停止输出。
`Fibonacci` 表示斐波那契数列，以下简称 `fib` ，它的定义如下:

- `fib[0] = 0`
- `fib[1] = 1`
- `fib[i] = fib[i-1] + fib[i-2] ( i ≥ 2 , i ∈ N* )`

### 分析

首先需要知道递推求 `fib[1...n]` 的方法:

> 定义 `fibs[0...n]` ，满足 `fibs[i]` = `fib[i]`。<br/>令 `fibs[0]` = `fib[0]` = `0`, `fib[1]` = `fib[1]` = `1` ，该步骤符合 `fibs` 的性质。<br/>	令 `k` 从 `2` 到 `n` 进行如下运算：<br/>- 令 `fibs[k]` = `fibs[k-1]` + `fibs[k-2]`<br/>- 该步骤也符合 `fibs` 的性质<br/>`fibs` 即为所求。

实际上并不是真的需要 `fibs` 数组，因为计算 `fibs[k]` 只需要 `fibs[k-1]` 和 `fibs[k-2]` 的值，所以只需要存储这三个变量即可计算。

使用 MiniExtend Time 来完成异步输出，使用 `nextTick()` 函数实现"从第一帧开始" 。
使用 `Timer` 计时器对象，通过设置其 `delay` 值为 2 实现"每隔两帧" 。
`i >= 50` 时调用 `Timer:pause()` 来实现"停止循环"。

### 代码

```lua
Env.__init__()
timer = Timer:new()
-- fib1 表示 fib[i-2] , fib2 表示 fib[i-1]
-- fib[0] = 0, fib[1] = 1
local i, fib1, fib2 = 2, 0, 1
-- 另一种方法:
-- timer.calback = function(self, tick)
function timer:callback(tick)
	-- 回调时 delay 已被默认地设为 1 ，因此需要重新设置 delay
	self.delay = 2
	-- fib3 表示 Fibonacci[i]
	local fib3 = fib1 + fib2
	local tickInfo, fibInfo = "tick "..tostring(tick)
	local fibInfo = "fib["..tostring(i).."] = "..tostring(fib3)
	Log.logtag("fib", tickInfo, fibInfo)
	if i >= 50 then
		timer:pause()
	else
		-- 为新的 fib 计算准备
		i, fib1, fib2 = i+1, fib2, fib3
	end
end
-- 另一种方法:
-- scheduleCall(1, function()
nextTick(function()
	-- 计时器运行晚于 nextTick
	-- 在这个 nextTick 回调后的那一帧 timer 就会开始运行
	timer:start()
end)
```

部分输出:

```
[...][fib] tick 1 fib[2] = 1
[...][fib] tick 3 fib[3] = 2
[...][fib] tick 5 fib[4] = 31
[...][fib] tick 7 fib[5] = 5
[...][fib] tick 9 fib[6] = 8
[...][fib] tick 11 fib[7] = 13
[...][fib] tick 13 fib[8] = 21
[...][fib] tick 15 fib[9] = 34
[...][fib] tick 17 fib[10] = 55
```

```
[...][fib] tick 87 fib[45] = 1134903170
[...][fib] tick 89 fib[46] = 1836311903
[...][fib] tick 91 fib[47] = 2971215073
[...][fib] tick 93 fib[48] = 4807526976
[...][fib] tick 95 fib[49] = 7778742049
[...][fib] tick 97 fib[50] = 12586269025
```