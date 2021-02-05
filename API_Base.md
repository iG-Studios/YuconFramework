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

`self:ListenToClientFunction(String name, Function function) [CLIENT ONLY]`

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

`self:DisconnectClientEvent(String name) [CLIENT ONLY]`

Disconnects from specified remote **event**.

Any function connected to the event will no longer work.

For example, if you used `self:ListenToClientEvent(name,...)` beforehand, disconnecting will render as an "undo" to it, given that you input the same name.

---

`self:DisconnectClientFunction(name) [CLIENT ONLY]`

Disconnects from specified remote **function**.

Any function connected to the event will no longer work.

For example, if you used `self:ListenToClientFunction(name,...)` beforehand, disconnecting will render as an "undo" to it, given that you input the same name.

---

`self:FireClient(Instance player, String name, Tuple Args[...]) [SERVER ONLY]`

Fires a remote **event** to a client.

Think back to the example shown for `self:ListenToClientEvent()`, when *ScoreEffect* was the category.

The event can be traced back to the server, which may have something like:

```lua
-- (Inside a method)
self:FireClient(ExamplePlayer, 50, true)

--[[
  In ExamplePlayer's client output, you would see:
    ExamplePlayer got 50 points!
    Congrats! You won!
--]]
```

---

`self:FireAllClients(String name, Tuple Args[...]) [SERVER ONLY]`

Fires a remote to all clients. This is like similar to `self:FireClient()`, except you do not have to input a player (as it performs the action for all players).

---

`self:InvokeClient(player,name,...) [SERVER ONLY] [UNSTABLE]`

Invokes a remote to a client. This is similar to `self:FireClient()`, except data can be retrieved.

---

`self:InvokeAllClients(name,...) [SERVER ONLY] [UNSTABLE]`

Invokes a remote to all clients. This is similar to `self:FireAllClients()`, except data can be retrieved.
