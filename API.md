# Yucon Framework API Page
This is the **API** for Yucon Framework.
Below are sorted method and functions descriptions for Yucon instances.

![](/TERMINOLOGY.png)

There are certain terms you need to keep in mind while reading the API!

A Yucon "instance" is a script, class, or plugin. All instances share the Yucon functionality.

Whenever `script` is referenced in code examples, it is referring to the Yucon instance. The same description applies for the term `plugin`.
The word `self` would be what you actually type in a Yucon instance to refer to the framework functionality.

### Example
A perfectly common example would be wanting to use `ProfileService`, a plugin that handles data saving for the player.
However, we need to know how to actually know where it is. We can use `self` to refer to Yucon Framework, and from there, call `GetPlugin()` to get `ProfileService`.

```lua
local ProfileService = self:GetPlugin("ProfileService") -- Stores the plugin as a variable
--// Do what you want with it from this point on
```

Remember: `self` in the code directly refers to Yucon Framework API!

> You can **not** use `self` outside of methods. For example, you *can* use `self` inside of the **Preload** method, but *not* in the beginning of a script.

---

![](SHARED.png)

### `[Plugin] self:GetPlugin(String PluginName)`

Returns a plugin instance with the given plugin name.
It will look for plugins only in the same server/client parent (client to client, server to server), or for a shared parent.
This can only return a plugin, NOT a script or plugin.

The below example waits 5 seconds using the thread handler plugin, and then prints text.

```
function MyServerScript:Start()
	local ProfileService = self:GetPlugin("ProfileService") -- Store profile service as a variable
	
	ProfileService.GetProfileStore("Index", {}) -- We can then use profile service :)
end
```

---

### `[any] self:NewInstance(String className, ...)`
This method will do the following in order:
* Find a class with the given name (if it exists)
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

### `[Class] self:GetClass(String ClassName)`

Returns a class instance with the given class name.
Unlike `self:NewInstance`, this does NOT return an object of a class, but rather, it returns the class itself.

This may be used to access class-wide methods not available to a class object.


---

### `[void] self:ListenToFramework(String eventName, Function bindedFunction)`

This is a replication of the `BindableEvent` object for Roblox. In other words, this acts a remote between scripts of the same parent.
For example, firing this on the client will also cause the client to hear it, but the server will not hear it.

```
self:ListenToFramework("ShowUI", function(UiName, ...)
	PlayerGuiList[UiName].Visible = true
end)
```

---

### `[void] self:DisconnectFromFramework(String eventName)`

To connect to the framework event, you use `ListenToFramework`. As such, you can stop listening by using the counter: `self:DisconnectFromFramework()`

---

### `[Instance] self:GetSharedAsset(String assetName, Boolean? recurseSearch)`

Gets an instance stored in the `ASSETS` folder in `ReplicatedStorage`, or nil if it does not exist.

If `recurseSearch` is set to `true`, then the search will continue until the instance is not `nil`.

---

### `[void] self:FireScripts(String eventName, ...)`

This provokes any connections that have binded to the framework event of the specified name.

The below code fires the code example from `self:ListenToFramework()`:

```
self:FireScripts("ShowUI", "MainGui")
```


---

### `[void] self:Warn(...)`

A more descriptive version of Luau's built in `warn` method.

![](/SERVER.png)

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
function MyServerScript:Start()
	local Player = Players:WaitForChild("iGottic")
	
	self:FireClient(Player, "FruitGivingRemote", 5, "Bananas")
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

### `[void] {SERVER-ONLY} self:ListenToServerClose(Function bindedFunction)`

When the server closes (such as when all players leave, when it shuts down, migrates an update, etc.), the function `bindedFunction` will be called.

```
self:ListenToServerClose(function()
	print("Oh no, the server is closing!")
end)
```


---

### `[Instance] {SERVER-ONLY} self:GetAsset(String assetName, Boolean? recurseSearch)`

Gets an instance stored in the `ASSETS` folder in `ServerStorage`, or nil if it does not exist.

If `recurseSearch` is set to `true`, then the search will continue until the instance is not `nil`.

![](CLIENT.png)

### `[void] {CLIENT-ONLY} self:ListenToServerEvent(String Name, Function function)`
This listens to a remote **event**, and any data sent from the server to the specified remote will pass to the specified function.

Both `self:FireClient` and `self:FireAllClients`, which are triggered from the *server*, can invoke the function passed into this method.

For example, let's say we have a round system on the server, which fires a remote named "Timer," and this is telling all of the clients how much time is left in a game. On the server, it will use a line of code such as `self:FireAllClients("Timer", TimeLeft)` to send this data. On the client, we have:

```
self:ListenToServerEvent("Timer", function(TimeLeft)
	print("Timer ticked! Time left: " .. TimeLeft)
	--// Do whatever here
end)
```

---

### `[void] {CLIENT-ONLY} self:ListenToServerFunction(String name, Function function)`
This listens to a remote **function**, and any data sent from the server to the specified remote will pass to the specified function.

This works similarly to `self:ListenToServerEvent`

This is not documented, as it is not recommended the developer uses this.

---

### `[void] {CLIENT-ONLY} self:DisconnectServerEvent(String name)`
If you used `self:ListenToServerEvent`, use this method to disconnect a remote event of the same name.

---

### `[void] {CLIENT-ONLY} self:DisconnectServerFunction(String name)`
If you used `self:ListenToServerFunction`, use this method to disconnect a remote function of the same name.

---

### `[void] {CLIENT-ONLY} self:FireServer(String name, ...)`
Sends information from the client to the server, given the remote name and any additional arguments the developer may add.

The below example sends the phrase "your mom" to the server, through the remote named "meme channel".
```
function MyClientScript:Start()
	local WeaponName = "PaintballGun"
	
	self:FireServer("GiveWeaponRemote", WeaponName)
end
```

---

### `[Tuple] {CLIENT-ONLY} self:InvokeServer(String name, ...)`
Sends information from the client to the server, given the remote name and any additional arguments the developer may add, and may recieve information in return from the server.

This is like `self:FireServer`, except this can have information returned.

---

### `[void] self:GetGui(String GuiName, Boolean? recurseSearcj, number?)`

Retrieve a `ScreenGui` under `PlayerGui`, by it's name, with the options to yeild, until a Gui has loaded.
