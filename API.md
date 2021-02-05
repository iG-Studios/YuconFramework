# Yucon Framework API Page
This is the **API** for Yucon Framework.
Below are sorted function descriptions for Yucon scripts and plugins.

# Understanding the API
Whenever `script` is referenced, it is referring to the Yucon Script. The same description applies for the term `plugin`.
The word `self` would be what you actually type in a Yucon Plugin or Script.

### Example
Let's say you wanted to get a plugin from a script. In this example, let's say we want to use the *ThreadHandler* plugin.

```lua
local ThreadHandler = self:GetPlugin("ThreadHandler") -- Stores the plugin as a variable
--// Do what you want with it from this point on
```

Remember: `self` in the code directly refers to Yucon Framework API

> You can **not** use `self` outside of methods. For example, you *can* use `self` inside of the **Preload** method, but *not* in the beginning of a script.

### [Base API](API_Base.md) | [Script-Only API](API_Script.md) | [Plugin API](API_Plugins)
