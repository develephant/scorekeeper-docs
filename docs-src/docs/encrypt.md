# Requirements

To use the encryption functionality, you must include the free [OpenSSL plugin](https://docs.coronalabs.com/plugin/openssl/index.html) provided by [Corona](https://coronalabs.com/) in your __build.settings__ plugins section:

```
...

plugins =
{
    ["plugin.openssl"] =
    {
        publisherId = "com.coronalabs",
    },
}, 

...
```

## Enabling Encryption

Enabling encryption will encrypt the score values in the storage file in which they are saved.

___Once you enter encryption mode you can not go back to unencrypted scores unless you delete the storage file using the [flush](/api/#flush) API method.___

To enable encryption, pass `true` when initializing the __ScoreKeeper__ object. See also __[new](/api/#new)__.

__Examples__

_Default_

```lua
local ScoreKeeper = require("plugin.scorekeeper")
local Scores = ScoreKeeper.new(true) -- enable encryption
```

_Compact_

```lua
local Scores = require("plugin.scorekeeper").new(true)
```