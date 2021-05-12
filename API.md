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

---

# Documentation

All functions are available in *all* types of Yucon instances.

### `[Plugin] self:GetPlugin(String PluginName)`

Returns a plugin instance with the given plugin name.
It will look for plugins only in the same server/client parent (client to client, server to server), or for a shared parent.

The below example waits 5 seconds using the thread handler plugin, and then prints text.

```
function MyScript:Preload()
  local ThreadHandler = self:GetPlugin("ThreadHandler")
  
  ThreadHandler:Wait(5) -- Wait 5 seconds with precision
  
  print("Waited 5 seconds!")
end
```

### `[void] {SERVER-ONLY} self:ListenToClientEvent(String Name, function DedicatedFunction)`

Listens to a remote **event** from the server, given the name of the remote event and the function that is used on call.

In the below example, we assume this function is in a server script that listens to an event.

```
function MyScript:Start()
  self:ListenToClientEvent("MoneyRemote", function(Player, MoneyAmount, ...) -- When called, gives money. Won't work with this code alone.
    print("Player that fired the remote: " .. Player.Name)
    print("Money requested: " .. MoneyAmount)
    
    --// Whatever else to add money
  end)
end
```

### `[void] {SERVER-ONLY} self:ListenToClientFunction(String Name, function DedicatedFunction)`

Listens to a remote **function** from the server, given the name of the remote function and the function that is used on call.

In the below example, we assume this function is in a server script that listens to an function.

```
function MyScript:Start()
  self:ListenToClientFunction("MoneyRemote", function(Player, MoneyAmount, ...) -- When called, gives money. Won't work with this code alone.
    print("Player that fired the remote: " .. Player.Name)
    print("Money requested: " .. MoneyAmount)
    
    --// Whatever else to add money
  end)
end
```

---

# Documentation IN PROGRESS!
