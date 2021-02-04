# Base API
*This is API for both client and server scripts and plugins.*

`self:GetPlugin(String pluginName)`
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

---

`self:ListenToClientEvent(String name,Function function) [CLIENT ONLY]`
Listens to a specified remote event, and connects the function. Function passes player and its arguments.

Think of `name` as the name of a remote event. It otherwise works like `RemoteEvent:OnClientEvent`

Example usage:
```lua
local Player = game.Players.LocalPlayer

function ScriptName:Start()
  self:ListenToClientEvent("ScoreEffect",function(ScoreAmount, IsWinningScore)
    print(Player.Name .. " got " .. ScoreAmount .. " points!")
    
    if IsWinningScore == true then
      print("Congrats! You won!")
    end
  end)
end
```

---

`self:ListenToClientFunction(name,function) [CLIENT ONLY]`
Listens to a specified remote function, and connects the function. Function passes player and its arguments.

Think of `name` as the name of a remote function. It otherwise works like `RemoteFunction.OnClientInvoke`

Example usage:
```lua
local MyVeryFrigginCoolWord = "Yucon"

function ScriptName:Start()
  self:ListenToClientFunction("GetTheWord",function()
    print("The server wants my very cool word!")
    return MyFrigginCoolWord
  end)
end
```

---

`self:DisconnectClientEvent(name) [CLIENT ONLY]`
Disconnects from specified remote event.

Any function connected to the event will no longer work.

---

`self:DisconnectClientFunction(name) [CLIENT ONLY]`
Disconnects from specified remote function.

Any function connected to the event will no longer work.

---

`self:FireClient(player,name,...) [SERVER ONLY]`
Fires a remote event to a client.

---

`self:FireAllClients(name,...) [SERVER ONLY]`
Fires a remote to all clients.

---

`self:InvokeClient(player,name,...) [SERVER ONLY]`
Invokes a remote to a client.

---

`self:InvokeAllClients(name,...) [SERVER ONLY]`
Invokes a remote to all clients.
