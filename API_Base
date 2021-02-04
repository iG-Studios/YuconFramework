# Base API
*This is API for both client and server scripts and plugins.*

`self:GetPlugin(pluginName)`
Requests access to a plugin for use of its functions.
This would be similar to Lua's built-in method for requiring modules to get the module data.

If the plugin was a module, it would be using `require(ModulePath)`.

Since we are using Yucon Plugins though, we type something different for a similar function, a.k.a. `self:GetPlugin(PluginNameGoesHere)`

Consider the following example below for getting the Thread Handler plugin:
```lua
local ThreadHandler -- Don't initialize the variable yet

function ScriptName:Preload()
  ThreadHandler = self:GetPlugin("ThreadHandler") -- Gets the thread handler plugin
end

function ScriptName:Start()
  print("Yay, script is started!")
  print("Waiting 5 seconds...")
  
  ThreadHandler:Wait(5)
  
  print("We waited five seconds! ^-^")
end
```

`self:ListenToClientEvent(name,function)`
Listens to a specified remote event, and connects to the function. Function passes player and its arguments.

`self:ListenToClientFunction(name,function)`
Listens to a specified remote function, and connects to the function. Function passes player and its arguments.

`self:DisconnectClientEvent(name)`
Disconnects from specified remote event.

`self:DisconnectClientFunction(name)`
Disconnects from specified remote function.

`self:FireClient(player,name,...)`
Fires a remote to a client.

`self:FireAllClients(name,...)`
Fires a remote to all clients.

`self:InvokeClient(player,name,...)`
Invokes a remote to a client.

`self:InvokeAllClients(name,...)`
Invokes a remote to all clients.
