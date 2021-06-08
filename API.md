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

All functions are available in *all* types of Yucon instances, **unless otherwise specified**.

---

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

---

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

---

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

### `[void] {SERVER-ONLY} self:DisconnectClientEvent(String name)`
If you used `self:ListenToClientEvent`, use this method to disconnect a remote event of the same name.

---

### `[void] {SERVER-ONLY} self:DisconnectClientFunction(String name)`
If you used `self:ListenToClientFunction`, use this method to disconnect a remote function of the same name.

---

### `[void] {SERVER-ONLY} self:FireClient(Instance player, String name, ...)`
Sends information from the server to a single specified client, given the remote name and any additional arguments the developer may add.

The below example sends the number `5`, and the word `banana` to a client, via a remote named `howmanyfruit`:
```
function asdasd:Start()
	local Player = game.Players:WaitForChild("iGottic")
	self:FireClient(Player, "howmanyfruit", 5, "banana")
end
```

---

### `[void] {SERVER-ONLY} self:FireAllClients(String name, ...)`
Sends information from the server to all clienst, given the remote name and any additional arguments the developer may add.
This works almost exactly like `self:FireClient`, but instead of passing data to *one* player, it passes it to *all* players.

---

### `[Tuple] {SERVER-ONLY} self:InvokeClient(Instance player, String name, ...)`
Sends information from the server to a single specified client, given the remote name and any additional arguments the developer may add, and may recieve information in return.
This is like `self:FireClient`, except this can have information returned.

This method is not recommended.

---

### `[void] {SERVER-ONLY} self:InvokeAllClients(String name, ...)`
This method is not documented as it does not serve any special purpose.
Use `FireAllClients` instead.

---

### `[any] self:NewInstance(String className, ...)`
This method will do the following in order:
* Find a class the developer made with the given name
* Call the `.New(...)` constructor method of that class, passing all arguments where `...` would be
* Return the new instance of that class returned by the constructor

To better explain this, consider the below code, found in a class named "Ball:"
```
local Ball = {}
Ball.__index = Ball

--// Constructor

function Ball.New(Name, Radius)
	local self = {}
	self.Name = Name
	self.Radius = Radius
	
	return setmetatable(self, Ball)
end

--// Methods

function Ball:PrintVolume()
	local Volume = 4/3 * math.pi * self.Radius ^ 3
	print("The ball named" .. self.Name .. "has a volume of " .. Volume .. " units cubed.")
end

return Ball
```

Now, let's consider some more code, which is located in a script.

```
function MyScript:Start()
	local MyBall = self:NewInstance("Ball", "MySpecialBall", 3)
	MyBall:PrintVolume()
end
```

This code will print:
*"The ball named MySpecialBall has a volume of 113.097336 units cubed."*

---

# Documentation IN PROGRESS!
