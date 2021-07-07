--//Script making
placeid = 286090429
if game.PlaceId == placeid then
    local Library = loadstring(game:HttpGet("https://raw.githubusercontent.com/xHeptc/Kavo-UI-Library/main/source.lua"))()
    local Window = Library.CreateLib("Hub", "BloodTheme")

    -- Main
    local Main = Window:NewTab("Main")
    local MainSection = Main:NewSection("Main")

    MainSection:NewToggle("WallBang", "ToggleInfo", function(state)
        if state then
                _G.Enable = true
        local MT = getrawmetatable(game)
        local OldIndex = MT.__index
        setreadonly(MT, false)
        MT.__index = newcclosure(function(A, B)
            if B == "Clips" then
                if _G.Enable then
                    return workspace.Map
                end
            end
            return OldIndex(A, B)
        end)
        game:GetService("UserInputService").InputBegan:Connect(function(Key, _)
            if not _ and Key.KeyCode == Enum.KeyCode.T then
                _G.Enable = not _G.Enable
            end
        end)
        else
                _G.Enable = false
        local MT = getrawmetatable(game)
        local OldIndex = MT.__index
        setreadonly(MT, false)
        MT.__index = newcclosure(function(A, B)
            if B == "Clips" then
                if _G.Enable then
                    return workspace.Map
                end
            end
            return OldIndex(A, B)
        end)
        game:GetService("UserInputService").InputBegan:Connect(function(Key, _)
            if not _ and Key.KeyCode == Enum.KeyCode.T then
                _G.Enable = not _G.Enable
            end
        end)
        end
    end)

    MainSection:NewButton("Kill all", "Kills everyone in the server", function(v)
		local Gun = game.ReplicatedStorage.Weapons:FindFirstChild(game.Players.LocalPlayer.NRPBS.EquippedTool.Value);
        local Crit = math.random() > .6 and true or false;
        for i,v in pairs(game.Players:GetPlayers()) do
            if v and v ~= game.Players.LocalPlayer and v.Character and v.Character:FindFirstChild("Head") then
                for i =1,10 do
                    local Distance = (game.Players.LocalPlayer.Character.Head.Position - v.Character.Head.Position).magnitude 
                    game.ReplicatedStorage.Events.HitPart:FireServer(v.Character.Head, -- Hit Part
                    v.Character.Head.Position + Vector3.new(math.random(), math.random(), math.random()), -- Hit Position
                    Gun.Name, 
                    Crit and 2 or 1, 
                    Distance,
                    Backstab, 
                    Crit, 
                    false, 
                    1, 
                    false, 
                    Gun.FireRate.Value,
                    Gun.ReloadTime.Value,
                    Gun.Ammo.Value,
                    Gun.StoredAmmo.Value,
                    Gun.Bullets.Value,
                    Gun.EquipTime.Value,
                    Gun.RecoilControl.Value,
                    Gun.Auto.Value,
                    Gun['Speed%'].Value,
                    game.ReplicatedStorage.wkspc.DistributedTime.Value);
                end 
            end
        end
    end)

MainSection:NewButton("Silent Aim", "Allows you to use Silent Aim", function()
	local CurrentCamera = workspace.CurrentCamera
local Players = game.GetService(game, "Players")
local LocalPlayer = Players.LocalPlayer
local Mouse = LocalPlayer:GetMouse()
function ClosestPlayer()
    local MaxDist, Closest = math.huge
    for I,V in pairs(Players.GetPlayers(Players)) do
        if V == LocalPlayer then continue end
        if V.Team == LocalPlayer then continue end
        if not V.Character then continue end
        local Head = V.Character.FindFirstChild(V.Character, "Head")
        if not Head then continue end
        local Pos, Vis = CurrentCamera.WorldToScreenPoint(CurrentCamera, Head.Position)
        if not Vis then continue end
        local MousePos, TheirPos = Vector2.new(Mouse.X, Mouse.Y), Vector2.new(Pos.X, Pos.Y)
        local Dist = (TheirPos - MousePos).Magnitude
        if Dist < MaxDist then
            MaxDist = Dist
            Closest = V
        end
    end
    return Closest
end
local MT = getrawmetatable(game)
local OldNC = MT.__namecall
local OldIDX = MT.__index
setreadonly(MT, false)
MT.__namecall = newcclosure(function(self, ...)
    local Args, Method = {...}, getnamecallmethod()
    if Method == "FindPartOnRayWithIgnoreList" and not checkcaller() then
        local CP = ClosestPlayer()
        if CP and CP.Character and CP.Character.FindFirstChild(CP.Character, "Head") then
            Args[1] = Ray.new(CurrentCamera.CFrame.Position, (CP.Character.Head.Position - CurrentCamera.CFrame.Position).Unit * 1000)
            return OldNC(self, unpack(Args))
        end
    end
    return OldNC(self, ...)
end)
MT.__index = newcclosure(function(self, K)
    if K == "Clips" then
        return workspace.Map
    end
    return OldIDX(self, K)
end)
setreadonly(MT, true)
end)


    -- Player
    local Player = Window:NewTab("Player")
    local PlayerSection = Player:NewSection("Player")

    PlayerSection:NewButton("ESP", "ButtonInfo", function()
		local Holder = Instance.new("Folder", game.CoreGui)
Holder.Name = "ESP"

local Box = Instance.new("BoxHandleAdornment")
Box.Name = "nilBox"
Box.Size = Vector3.new(4, 7, 4)
Box.Color3 = Color3.new(100 / 255, 100 / 255, 100 / 255)
Box.Transparency = 0.7
Box.ZIndex = 0
Box.AlwaysOnTop = true
Box.Visible = true

local NameTag = Instance.new("BillboardGui")
NameTag.Name = "nilNameTag"
NameTag.Enabled = false
NameTag.Size = UDim2.new(0, 200, 0, 50)
NameTag.AlwaysOnTop = true
NameTag.StudsOffset = Vector3.new(0, 1.8, 0)
local Tag = Instance.new("TextLabel", NameTag)
Tag.Name = "Tag"
Tag.BackgroundTransparency = 1
Tag.Position = UDim2.new(0, -50, 0, 0)
Tag.Size = UDim2.new(0, 300, 0, 20)
Tag.TextSize = 20
Tag.TextColor3 = Color3.new(100 / 255, 100 / 255, 100 / 255)
Tag.TextStrokeColor3 = Color3.new(0 / 255, 0 / 255, 0 / 255)
Tag.TextStrokeTransparency = 0.4
Tag.Text = "nil"
Tag.Font = Enum.Font.SourceSansBold
Tag.TextScaled = false

local LoadCharacter = function(v)
	repeat wait() until v.Character ~= nil
	v.Character:WaitForChild("Humanoid")
	local vHolder = Holder:FindFirstChild(v.Name)
	vHolder:ClearAllChildren()
	local b = Box:Clone()
	b.Name = v.Name .. "Box"
	b.Adornee = v.Character
	b.Parent = vHolder
	local t = NameTag:Clone()
	t.Name = v.Name .. "NameTag"
	t.Enabled = true
	t.Parent = vHolder
	t.Adornee = v.Character:WaitForChild("Head", 5)
	if not t.Adornee then
		return UnloadCharacter(v)
	end
	t.Tag.Text = v.Name
	b.Color3 = Color3.new(v.TeamColor.r, v.TeamColor.g, v.TeamColor.b)
	t.Tag.TextColor3 = Color3.new(v.TeamColor.r, v.TeamColor.g, v.TeamColor.b)
	local Update
	local UpdateNameTag = function()
		if not pcall(function()
			v.Character.Humanoid.DisplayDistanceType = Enum.HumanoidDisplayDistanceType.None
			
		end) then
			Update:Disconnect()
		end
	end
	UpdateNameTag()
	Update = v.Character.Humanoid.Changed:Connect(UpdateNameTag)
end

local UnloadCharacter = function(v)
	local vHolder = Holder:FindFirstChild(v.Name)
	if vHolder and (vHolder:FindFirstChild(v.Name .. "Box") ~= nil or vHolder:FindFirstChild(v.Name .. "NameTag") ~= nil) then
		vHolder:ClearAllChildren()
	end
end

local LoadPlayer = function(v)
	local vHolder = Instance.new("Folder", Holder)
	vHolder.Name = v.Name
	v.CharacterAdded:Connect(function()
		pcall(LoadCharacter, v)
	end)
	v.CharacterRemoving:Connect(function()
		pcall(UnloadCharacter, v)
	end)
	v.Changed:Connect(function(prop)
		if prop == "TeamColor" then
			UnloadCharacter(v)
			wait()
			LoadCharacter(v)
		end
	end)
	LoadCharacter(v)
end

local UnloadPlayer = function(v)
	UnloadCharacter(v)
	local vHolder = Holder:FindFirstChild(v.Name)
	if vHolder then
		vHolder:Destroy()
	end
end

for i,v in pairs(game:GetService("Players"):GetPlayers()) do
	spawn(function() pcall(LoadPlayer, v) end)
end

game:GetService("Players").PlayerAdded:Connect(function(v)
	pcall(LoadPlayer, v)
end)

game:GetService("Players").PlayerRemoving:Connect(function(v)
	pcall(UnloadPlayer, v)
end)

game:GetService("Players").LocalPlayer.NameDisplayDistance = 0
    end)

    PlayerSection:NewButton("Rejoin", "Rejoin's the game", function()
        TeleportService = game:GetService("TeleportService")
		PlaceID = 286090429
		wait()
		TeleportService:Teleport(PlaceID, plr)
    end)

    local Player = Window:NewTab("Extra")
    local ExtraSection = Player:NewSection("Extra")

    ExtraSection:NewKeybind("Toggle UI", "Allows you to toggle the UI with a keybind", Enum.KeyCode.V, function()
        Library:ToggleUI()
    end)

    -- Credits
    local Player = Window:NewTab("Credits")
    local CreditsSection = Player:NewSection("CrispyExploitz")
    local CreditsSection = Player:NewSection("Crispy#3017")
    local CreditsSection = Player:NewSection("https://discord.gg/FkadkNN5")
    end
