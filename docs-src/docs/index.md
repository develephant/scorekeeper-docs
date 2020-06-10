![logo](img/logo256.png)

# ScoreKeeper Plugin

__Easily keep track of your players scores locally on the device in a persistent manner.__

---

## Get the Plugin

If you don't already have it, get the __ScoreKeeper__ plugin from the __[Corona Marketplace](https://marketplace.coronalabs.com/plugin/score-keeper)__.

---

## Load the Plugin

Load the plugin by adding an entry to the __plugins__ table of __build.settings__ file:

```
settings =
{
    plugins =
    {
        ["plugin.scorekeeper"] =
        {
            publisherId = "com.develephant"
        },
    },
}
```

---

## Initialize the Plugin

See the __[API documentation](/api/)__ for details on initializing the plugin in your code.