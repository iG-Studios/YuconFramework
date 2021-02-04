# Yucon API
This is the **API** for Yucon Framework.
Below are sorted function descriptions for Yucon scripts and plugins.

# Understanding the API
Whenever `script` is referenced, it is referring to the Yucon Script. The same description applies for the term `plugin`.
The word `self` would be what you actually type in a Yucon Plugin or Script.

### Example
Let's say you wanted to get a plugin from a script. In this example, let's say we want to use the *ThreadHandler* plugin.

```lua
local ThreadHandler = self:GetPlugin("ThreadHandler") -- Stores the plugin as a variable
--// Do what you want with it
```

---

# Server Script
script:Preload [REQUIRED]
Fires when the script is first called and stored. Use to load any needed data.
Will yeild other scripts until the preload function is complete.

script:Step(delta) [REQUIRED]
Fires every framework tick (a little over heartbeat time)
Using this can help avoid using wait()
delta is the amount of time that passed between frames.

script:Start [REQUIRED]
Fires when the script is initialized. Use for main coding.

self:GetPlugin(pluginName)
Requests access to a plugin for use of it’s functions.

self:ListenToClientEvent(name,function)
Listens to a specified remote event, and connects to the function. Function passes player and it’s arguments.

self:ListenToClientFunction(name,function)
Listens to a specified remote function, and connects to the function. Function passes player and it’s arguments.

self:DisconnectClientEvent(name)
Disconnects from specified remote event.

self:DisconnectClientFunction(name)
Disconnects from specified remote function.

self:FireClient(player,name,...)
Fires a remote to a client.

self:FireAllClients(name,...)
Fires a remote to all clients.

self:InvokeClient(player,name,...)
Invokes a remote to a client.

self:InvokeAllClients(name,...)
Invokes a remote to all clients.
