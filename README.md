<div align="center">
    <img src="assets/HDAdminLogo.png" alt="Rojo" height="217" />
</div>

<div>&nbsp;</div>

**HD Admin** is an open-source admin application for the Roblox platform.

HD Admin is comprised of an extensive range of features and commands designed to enhance games for both the player and developer.

<br>

# Resources
- MainModule: https://www.roblox.com/library/2686494999/HD
- Loader: https://www.roblox.com/library/857927023/HD
<br>

# Tutorials
1. ### [An introduction to HD Admin](https://www.youtube.com/watch?v=8_wMQLJF5ds&list=PLsbxI7NIoTthZXpsd_-_Ab5c0OZ5alA0j&index=1)
2. ### [Setting up Ranks and Specific Users](https://www.youtube.com/watch?v=8_wMQLJF5ds&list=PLsbxI7NIoTthZXpsd_-_Ab5c0OZ5alA0j&index=2)
3. ### [Setting up Gamepasses and Assets](https://www.youtube.com/watch?v=8_wMQLJF5ds&list=PLsbxI7NIoTthZXpsd_-_Ab5c0OZ5alA0j&index=3)
4. ### [Setting up Groups](https://www.youtube.com/watch?v=8_wMQLJF5ds&list=PLsbxI7NIoTthZXpsd_-_Ab5c0OZ5alA0j&index=4)
5. ### [Setting up Friends, VIP Servers and Free Admin](https://www.youtube.com/watch?v=8_wMQLJF5ds&list=PLsbxI7NIoTthZXpsd_-_Ab5c0OZ5alA0j&index=5)
6. ### [Modify System Settings](https://www.youtube.com/watch?v=8_wMQLJF5ds&list=PLsbxI7NIoTthZXpsd_-_Ab5c0OZ5alA0j&index=6)
7. ### [Adding Custom Morphs and Tools](https://www.youtube.com/watch?v=8_wMQLJF5ds&list=PLsbxI7NIoTthZXpsd_-_Ab5c0OZ5alA0j&index=7)
8. ### [Modify Commands (coming soon)]()
<br>

# API
## Retrieving HD
```lua
local hdClient = game:GetService("ReplicatedStorage"):WaitForChild("HDAdminClient")
local hdMain = require(hdClient:WaitForChild("SharedModules").MainFramework):CheckInitialized(hdClient)
local hd = hdMain.modules.API
```

## Key Parameters
- **player** - the player instance (e.g. ``local player = game:GetService("Players").ForeverHD``)
- **rank** - the rankId or rankName (e.g. 1 or "VIP")
- **rankType** - determines the duration the player keeps their rank for:
    - **"Perm"** - for all servers for an infinite period of time
    - **"Server"** - for the server the rank is given in until the server ends
    - **"Temp"** - for the server the rank is given in until the player leaves


## Methods
### Server
#### `hd:SetRank(player, rank, rankType)`
Sets the Rank and RankType for the specified player. Example: `hd:SetRank(player, "Mod", "Perm")`

#### `hd:UnRank(player)`
Sets the Rank to 0 (NonAdmin) and clears the RankType for the specified player. Example: `hd:UnRank(player)`

#### `hd:GetRank(player)`
Returns the rankId, rankName and rankType for the specified player. Example: `local rankId, rankName, rankType = hd:GetRank(player)`

#### `hd:DisableCommands(player, boolean)`
Disables the ability to use commands for the specified player when set to true. Example: `hd:DisableCommands(player, true)`

<br>

### Client
#### `hd:SetTopbarTransparency(number)`
Sets the transparency of the HD Topbar. Example: `hd:SetTopbarTransparency(0.5)`

#### `hd:SetTopbarEnabled(boolean)`
Hides and disables the HD Topbar when set to false. Example: `hd:SetTopbarEnabled(false)`

<br>

### Shared
#### `hd:GetRankName(rankId)`
Returns the corresponding rankName from the given rankId. Example: `local rankName = hd:GetRankName(1)`

#### `hd:GetRankId(rankName)`
Returns the corresponding rankId from the given rankName. Example: `local rankId = hd:GetRankId("VIP")`

#### `hd:Notice(player, message)`
Displays a notification to the specified player. If used on the client, 'player' must be the LocalPlayer. Example: `hd:Notice(player, "Hello world!")'

#### `hd:Error(player, message)`
Displays an error notification to the specified player. If used on the client, 'player' must be the LocalPlayer.. Example: `hd:Error(player, "Error!")`

<br>


### Admin Pad Example
```lua
--In a Server Script

--Retrieve API
local hdContainer = game:GetService("ReplicatedStorage"):WaitForChild("HDAdminContainer") local hdMain = require(hdContainer:WaitForChild("SharedModules").MainFramework):CheckInitialized(hdContainer)
local hd = hdMain.modules.API

--Define the rank-to-reward and setup the corresponding rankId and rankName
local rank = "Mod"
local rankType = "Server"
local rankId = tonumber(rank) or hd:GetRankId(rank)
local rankName = hd:GetRankName(rankId)

--Define debounce
local touchDe = {}

--Touch event ('touchPart' is the part players have to step on to receive the rank) 
touchPart.Touched:Connect(function(hit)
	
	--Check for character and player
	local character = hit.Parent
	local player = game:GetService("Players"):GetPlayerFromCharacter(character)
	if player and not touchDe[player] then
		
		--Setup debounce for player
		touchDe[player] = true
		
		--Check rank is lower than giver rank
		local plrRankId, plrRankName, plrRankType = hd:GetRank(player)
		if plrRankId < rankId then
			--Give rank
			hd:SetRank(player, rankId, rankType)
		else
			--Error message
			local errorMessage = "Your rank is already higher than '"..rankName.."'!"
			if plrRankId == rankId then
				errorMessage = "You've already been ranked to '"..rankName.."'!"
			end
			hd:Error(player, errorMessage)
		end
		wait(1)
		
		--End debounce
		touchDe[player] = false
	end
end)
```

You can take a completed copy [here](https://www.roblox.com/library/3076171971/HD-Admin-Pad).

<br>



# Get In Touch
- **Twitter** - [@ForeverhdRBX](https://twitter.com/ForeverhdRBX)
- **DevForum** - [ForeverHD](https://devforum.roblox.com/u/ForeverHD/summary)
- **Roblox** - [ForeverHD](https://www.roblox.com/users/82347291/profile)
- **Discord** - [HD Community](http://discord.gg/hJBGFa5)
