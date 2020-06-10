![logo](img/logo128.png)

## ScoreKeeper

### new

Initialize the __ScoreKeeper__ object.

```lua
new([useEncryption])
```

__Arguments__

|Name|Description|Type|Required|
|----|-----------|----|--------|
|useEncryption|Encrypt player scores. See the __[Encryption](/encrypt/)__ section.|_Boolean_|__N__|

__Returns__

A __ScoreKeeper__ object on which all other methods are called.

__Examples__

_Default_

```lua
local ScoreKeeper = require("plugin.scorekeeper")
local Scores = ScoreKeeper.new() -- ScoreKeeper Object
```

_Compact_

```lua
local Scores = require("plugin.scorekeeper").new()
```

__Notes__

___You should only initialize one ScoreKeeper object in your app.___ 

You can do this by placing the initialization in a "globals" Lua module, or adding it to the global Lua space (generally you don't want to add to the global space, but this would be considered a "responsable" global):

```lua
_G.Scores = require("plugin.scorekeeper").new()
```

You can then access the __ScoreKeeper__ object anywhere in your code files.

```lua
--anyfile.lua
Scores:save(200)
```

---

## ScoreKeeper Object

All of the following methods are called on a __ScoreKeeper__ object. See __[new](api/#scorekeeper)__ above.

### save

Save the players score.

```lua
save(score[, limit])
```

__Arguments__

|Name|Description|Type|Required|
|----|-----------|----|--------|
|score|The players score value to save.|_Number_|__Y__|
|limit|Cap the amount of scores saved. See __Notes__ below.|_Number_|__N__|

__Returns__

A _Table_ array of the current saved scores from _highest to lowest_, or __nil__ and an error.

__Examples__

_Default_

```lua
Scores:save(2000)
```

_Using return value_

```lua
local scores = Scores:save(2000)

for i=1, #scores do
  print(i, scores[i])
end
```

_Save and cap to 10 scores_

```lua
local scores = Scores:save(154, 10)

for i=1, #scores do
  print(i, scores[i])
end
```

_Using error checking_

```lua
local scores, err = Scores:save(1500)

if not scores then
  print(err)
else
  for i=1, #scores do
    print(i, scores[i])
  end
end
```

__Notes__

If you pass the `limit` argument, the score list will be "capped" at that number. For example if you only want to keep the top 20 scores set the `limit` to 20. Any new scores that are _higher than the lowest_ score in the list will be replaced to keep the list capped to 20 scores.

You can change the `limit` at any time, but be aware if you set it to a smaller number than what you started with, the extra scores (lowest to highest) will be dropped to match the new "cap" size.

---

### list

List players saved scores.

```lua
list([sort][, limit])
```

__Arguments__

|Name|Description|Type|Required|
|----|-----------|----|--------|
|sort|Either 'high' or 'low'. The default is 'high'. See __Notes__ below.|_String_|__N__|
|limit|Cap the amount of scores returned. Defaults to all scores.|_Number_|__N__|


__Returns__

A _Table_ array of score values. By default the scores are sorted from _highest to lowest_. If no values have been saved yet, the array will be empty.

__Examples__

_List all saved scores, highest to lowest_

```lua
local scores = Scores:list()

for i=1, #scores do
  print(i, scores[i])
end
```

_List top 5 saved scores (see also the __[top](/api/#top)__ method)_

```lua
local scores = Scores:list(5)

for i=1, #scores do
  print(i, scores[i])
end
```

_List all scores, lowest to highest_

```lua
local scores = Scores:list('low')

for i=1, #scores do
  print(i, scores[i])
end
```

_List 10 scores, lowest to highest_

```lua
local scores = Scores:list('low', 10)

for i=1, #scores do
  print(i, scores[i])
end
```

__Notes__

Possible `sort` values

  - 'high' returns the scores from _highest to lowest_.
  - 'low' returns the scores from _lowest to highest_.

See the __[top](/api/#top)__ method below, which is a convenience function to get the top scores.

---

### top

List the players top saved scores from _highest to lowest_.

```lua
top([limit])
```

__Arguments__

|Name|Description|Type|Required|
|----|-----------|----|--------|
|limit|Cap the amount of scores returned. Defaults to 10.|_Number_|__N__|

__Returns__

A _Table_ array of the top 10 score values (unless `limit` is provided), sorted from _highest to lowest_. If no values have been saved yet, the array will be empty.

__Examples__

_Default (top 10)_

```lua
local scores = Scores:top()

for i=1, #scores do
  print(i, scores[i])
end
```

_Load top 25 scores_

```lua
local scores = Scores:top(25)

for i=1, #scores do
  print(i, scores[i])
end
```

---

### bottom

Load player scores from the bottom of the list. See __Notes__ below.

```lua
bottom(limit)
```

__Arguments__

|Name|Description|Type|Required|
|----|-----------|----|--------|
|limit|Cap the amount of scores returned.|_Number_|__Y__|


__Returns__

A _Table_ array of saved score values. If no values have been saved yet, the array will be empty. See __Notes__ below.

__Example__

_List the bottom 5 scores, sorted highest to lowest_

```lua
local scores = Scores:bottom(5)

for i=1, #scores do
  print(i, scores[i])
end
```

__Notes__

If you want an ordered list of scores from _lowest to highest_, you must use the __[list](/api/#list)__ method. This method returns the lowest scores saved, capped by the `limit` argument, sorted from _highest to lowest_.

If the `limit` argument is equal to or greater than the amount of current scores saved, this method oprerate exactly like the __[top](/api/#top)__ method.

If you don't pass the `limit` argument, the lowest score will be returned in the _Table_ array. See also __[lowest](/api/#lowest)__.

---

### highest

Get the players highest score saved.

```lua
highest()
```

__Arguments__

_This method takes no arguments._

__Returns__

A _Number_ value of the highest score. If no saved values exist returns 0.

__Example__

```lua
local score = Scores:highest()
```

__Notes__

This method returns a single value directly as a _Number_. It is not a _Table_ array.

---

### lowest

Get the players lowest score saved.

```lua
lowest()
```

__Arguments__

_This method takes no arguments._

__Returns__

A _Number_ value of the lowest score. If no saved values exist returns 0.

__Example__

```lua
local score = Scores:lowest()
```

__Notes__

This method returns a single value directly as a _Number_. It is not a _Table_ array.

---

### trim

Trim the players saved score list to the specified size and update the storage file.

```lua
trim(size)
```

__Arguments__

|Name|Description|Type|Required|
|----|-----------|----|--------|
|size|The length of the new score list. Must be greater than zero. See __Notes__ below.|_Number_|__Y__|

__Returns__

A _Table_ array of score values, sorted from _highest to lowest_, or __nil__ and an error.

__Examples__

_Trim the list to contain only the top 10 scores_

```lua
local scores = Scores:trim(10)

for i=1, #scores do
  print(i, scores[i])
end
```

_Using error checking_

```lua
local scores, err = Scores:trim(10)

if not scores then
  print(err)
else
  for i=1, #scores do
    print(i, scores[i])
  end
end
```

__Notes__

This method is destructive by nature. After the trim has been the processed, the results will immediately be saved to the stroage file.

Trim will remove any scores beyond the provided `size` parameter. Scores are first sorted _highest to lowest_ before the trim.

The `size` argument must be greater than zero. To clear all score values from the list use the __[empty](/api/#empty)__ method below.

---

### empty

Empty the players score list. All saved scores are removed and the storage file is updated.

```lua
empty()
```

__Arguments__

_This method takes no arguments._

__Returns__

An empty _Table_ array, or __nil__ and an error.

__Examples__

_Default_

```lua
Scores:empty()
```

_Using error checking_

```lua
local scores, err = Scores:empty()

if not scores then
  print(err)
else
  print("success")
end
```

__Notes__

This method is destructive by nature. After the empty method has run, the results will immediately be saved to the stroage file (an empty _Table_ array).

---

### flush

Deletes all scores and the storage file from the device.

```lua
flush()
```

__Arguments__

_This method takes no arguments._

__Returns__

A _Boolean_ `true` on success, or __nil__ and an error.

__Example__

```lua
local ok, err = Scores:flush()

if not ok then
  print(err)
else
  print("storage file removed")
end
```

__Notes__

This method is destructive by nature. All scores and the storage file will no longer be accessible.

A new storage file will be created the next time the __[save](/api/#save)__ method is used.

---

### debug

Utility method. Prints all saved scores to the Corona console.

__Example__

```lua
Scores:debug()
```