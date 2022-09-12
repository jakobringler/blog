---
title: "Lua"
draft: false
tags:
- lua
---

### Syntax

```lua
print("hello world")

-- comment

--[[
multiline 
comment
]]--

local var = 1 -- local var (can only be accessed inside script)

local name = "Jack"

print("My name is: " .. name .. ".") -- insert variables in print strings

name = nil -- clears memory

descripting = [[multi
	line
string
]] -- multi lines strings

VAR = 2 -- global variable starts with capital letter

_G.var = 2 --makes sure its global even if its lower case

if true then -- if statement
	print("something")
end

--[[
	==
	<=
	>=
	~=
	!=
	<
	>
]]--

if (a > b ) and (a > c ) then -- combine booleans with and / or
	print("something")
end

if (a > b ) or (a > c ) then
	print("something else")
else
	print("something more else")
end

local age = 33
local old = age > 30 and true or false -- returns true if age over 30

-- for loops

for i = 1, 10, 1 do -- for (current iteration), (iterations), (stepsize)
	print(i)
end

for i = 1, 10 do -- works too. default step size is 1
	print(i)
end

for i = 10, 1, -1 do -- reverse loop
	print(i)
end

local arr = {2, 3, 45, 65676, 34, 21, 2, 30}

for i = 1, #arr do -- loop over array
	print(arr[i])
end

local n = 10

while n > 0 do
	print("foo")
	n = n -1
end

------- functions

local function fn() -- local
	local x = 1
	return foo
end

-- x cant be accessed outside of the function

function globalfn(input) -- global
	return input * input
end

local add10 = function(number) -- define function as variable
	local outcome = 10 + number
	return outcome
end

print(add10(5))
-- output: 15

local function Pet(name)
	name = name or "Default" -- set default values if nothing is entered
	return name
end
```

### Operators

```lua
--[[
	__add = +
	__sub = -
	__mul = *
	__div = /
	__mod = %
	__pow = ^
	__concat = ..
	__len = #
	__eq = ==
]]--
```

### User Input

```lua
print("Type something:")
local input = io.read() 
print("You wrote: " .. input)

io.write("Type something:") -- can be answered in same line
print("You wrote: " .. input)

```

### Arrays / "Tables"

```lua
local arr = {1, 3, 51, 9, 17}

table.sort(arr)

for i = 1, #arr do 
	print(arr[i])
end

table.insert(arr, 2, "foo")

table.remove(arr, 4)

print(table.concat(arr, ", "))
-- output: 1, foo, 3, 17, 51

local arr2 = {
	{1, 2, 3},
	{4, 5, 6},
	{7, 8, 9}
} -- nested arrays

print(arr[1][1])

  

for i = 1, #arr2 do
	for j = 1, #arr2[i] do
		print(arr2[i][j])
	end
end

```

### Recursion

call function in itself

```lua
local function counter(num, enum)
	local count = num + 1
	
	if(count < enum) then
		print(count)
		return counter(count, enum)
	end
end

print(counter(10, 15))

-- output: 11, 12, 13, 14
```

### Key - Value Pairs

```lua
local function sum(...) -- (...) allows an infinite amount of arguments
	local sums = 0
	
	for key, value in pairs({...}) do -- swirly braces transforms the arguments into a table
		print(key, value)
	end
end

sum(10, 5, 6 ,4, 12)

-- output:
-- 1  10
-- 2  5
-- 3  6
-- 4  4
-- 5  12
```

### Coroutines

useful to wait for other part of programm. e.g. unpacking files 

```lua
local routine_1 = coroutine.create(
	function ()
		for i = 1, 10, 1 do
			print("(Routine 1): " .. i)
			
			if i == 5 then
				coroutine.yield()
			end
		end
end
)

local routine_func = function ()
	for i = 11, 20 do
		print("(Routine 2): ".. i)
	end
end

local routine_2 = coroutine.create(routine_func)

coroutine.resume(routine_1)
coroutine.resume(routine_2)
coroutine.resume(routine_1)
print(coroutine.status(routine_1))
```

### File I/O

```lua
io.output("file.txt")
io.write("Hello World")
io.close()

io.read(5) -- reads 5 characters


local file = io.open("file.txt", "w") -- "w" write, "r" read, "a" append
local reads = file:read("*all")
file:write("foo bar")
file:close()
```

### OS Module

```lua
print(os.time())

-- deleting stuff
os.remove("path/file.ext")

-- exec commands
os.execute("whoami")

-- time code
local start = os.clock()

for i = 1, 10000 do
	print(i)
end

print(os.clock() - start)

-- exit programm
os.exit()
```

### Custom Modules

```lua
mymodule = {}

function mymodule.add(x, y)
	return x + y
end

function mymodule.power(x, y)
	return x ^ y
end

return mymodule
```

```lua
local mod = require("mymodule")
print(mod.add(5, 10))
print(mod.power(5, 10))
```

### Metamethods

```lua
local function addTableValues(x, y)
	return x.num + y.num
end

local metatable = {
	__add = addTableValues,
	__sub = function (x, y)
		return x.num - y.num
	end
}

local tbl1 = { num = 50 }
local tbl2 = { num = 10 }

setmetatable(tbl1, metatable)

local ans = tbl1 - tbl2

print(ans)
```

### More
[[notes/Lua Scripting in Resolve |Lua Scripting in Resolve]]

---

sources / further reading:
- [Lua.org](https://www.lua.org/)
- [Full Lua Programming Crash Course - Beginner to Advanced](https://www.youtube.com/watch?v=1srFmjt1Ib0)
- [LuaRocks](https://www.youtube.com/watch?v=1srFmjt1Ib0)

