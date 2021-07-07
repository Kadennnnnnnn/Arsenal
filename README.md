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

    MainSection:NewButton("Gun Mods", "Allows you to remove recoil, always auto, etc.", function(v)
local Functions = {}
	for i,v in pairs(getreg()) do
		if type(v) == "function" then
			for i2,v2 in pairs(getfenv(v)) do
				if type(v2) == "function" then
					Functions[tostring(i2)] = v2
				end
			end
		end
	end

	function GetLocalWeapon()
		return getfenv(Functions.usethatgun).gun
	end

	game:GetService("RunService"):BindToRenderStep("gunmodsarecool", 1, function()
		getfenv(Functions.usethatgun).currentspread = 0 -- NoSpread

		getfenv(Functions.usethatgun).recoil = 0 -- NoRecoil

		if GetLocalWeapon() ~= "none" and GetLocalWeapon():FindFirstChild("Ammo") then -- Inf Ammo
			debug.setupvalue(Functions["updateInventory"], 3, GetLocalWeapon():FindFirstChild("Ammo").Value)
		end

		if game.Players.LocalPlayer.Character and game.Players.LocalPlayer.Character:FindFirstChild("Spawned") and game:GetService("UserInputService"):IsMouseButtonPressed(Enum.UserInputType.MouseButton1) then -- RapidFire
			if game.Players.LocalPlayer.Character:FindFirstChild("Humanoid").Health ~= 0 and game.Players.LocalPlayer.Character:FindFirstChild("Spawned") then
				pcall(function()
					Functions.firebullet(true)
				end)
			end
		end
	end)
    end)

MainSection:NewButton("Silent Aim", "Allows you to use Silent Aim", function()
local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local mouse = LocalPlayer:GetMouse()
local Camera = workspace.CurrentCamera
local Debris = game:GetService("Debris")
local UserInputService = game:GetService("UserInputService")
local target = false
local RunService = game:GetService("RunService")


getfenv().lock = "Head" -- Head Hitbox, or Random

fov = 250;
local fovCircle = true;
local st = tonumber(tick());

if fovCircle then
	function createcircle()
	    local a=Drawing.new('Circle');a.Transparency=1;a.Thickness=1.5;a.Visible=true;a.Color=Color3.fromRGB(0,255,149);a.Filled=false;a.Radius=fov;
	    return a;
	end;  
	local fovc = createcircle();
	spawn(function()
	    RunService:BindToRenderStep("FovCircle",1,function()
	        fovc.Position = Vector2.new(mouse.X,mouse.Y)
	    end);
	end);
end;

function isFfa()
	local am = #Players:GetChildren();
	local amm = 0;
	for i , v in pairs(Players:GetChildren()) do
		if v.Team == LocalPlayer.Team then
			amm = amm + 1;
		end;
	end;
	return am == amm;
end;
function getnearest()
	local nearestmagnitude = math.huge
	local nearestenemy = nil
	local vector = nil
	local ffa = isFfa();
	for i,v in next, Players:GetChildren() do
		if ffa == false and v.Team ~= LocalPlayer.Team or ffa == true then
			if v.Character and  v.Character:FindFirstChild("HumanoidRootPart") and v.Character:FindFirstChild("Humanoid") and v.Character.Humanoid.Health > 0 then
				local vector, onScreen = Camera:WorldToScreenPoint(v.Character["HumanoidRootPart"].Position)
				if onScreen then
					local ray = Ray.new(
					Camera.CFrame.p,
					(v.Character["Head"].Position-Camera.CFrame.p).unit*500
					)
					local ignore = {
					LocalPlayer.Character,
					}
					local hit,position,normal=workspace:FindPartOnRayWithIgnoreList(ray,ignore)
					if hit and hit:FindFirstAncestorOfClass("Model") and Players:FindFirstChild(hit:FindFirstAncestorOfClass("Model").Name)then
						local magnitude = (Vector2.new(mouse.X, mouse.Y) - Vector2.new(vector.X, vector.Y)).magnitude
						if magnitude < nearestmagnitude and magnitude <= fov then
							nearestenemy = v
							nearestmagnitude = magnitude
						end
					end
				end
			end
		end
	end
	return nearestenemy
end


local meta = getrawmetatable(game)
setreadonly(meta, false)
local oldNamecall = meta.__namecall
meta.__namecall = newcclosure(function(...)
    
	local method = getnamecallmethod()
	local args = {...}
	if string.find(method,'Ray') then
		if target then
		    if args[1].Name ~= "Workspace" then
   		        print(args[1])
   		    end;
			args[2] = Ray.new(workspace.CurrentCamera.CFrame.Position, (target.Position + Vector3.new(0,(workspace.CurrentCamera.CFrame.Position-target.Position).Magnitude/500,0) - workspace.CurrentCamera.CFrame.Position).unit * 5000)
		end
	end
	return oldNamecall(unpack(args))
end)

RunService:BindToRenderStep("SilentAim",1,function()
	if UserInputService:IsMouseButtonPressed(0) and Players.LocalPlayer.Character and Players.LocalPlayer.Character:FindFirstChild("Humanoid") and Players.LocalPlayer.Character.Humanoid.Health > 0 then
		local enemy = getnearest()
		if enemy and enemy.Character and enemy.Character:FindFirstChild("Humanoid") and enemy.Character.Humanoid.Health > 0 then                
			local vector, onScreen = Camera:WorldToScreenPoint(enemy.Character["Head"].Position)
			local head = (Vector2.new(mouse.X, mouse.Y) - Vector2.new(vector.X, vector.Y)).magnitude
			local vector, onScreen = Camera:WorldToScreenPoint(enemy.Character["HumanoidRootPart"].Position)
			local hitbox = (Vector2.new(mouse.X, mouse.Y) - Vector2.new(vector.X, vector.Y)).magnitude
			if head <= hitbox then
				magnitude = head
			else
				magnitude = hitbox;
			end;
			if getfenv().lock == "Head" then
				target = workspace[enemy.Name]["Head"]
			else
				if getfenv().lock == "Random" then
					if magnitude == hitbox then
						target = workspace[enemy.Name]["HumanoidRootPart"];
					else
						target = workspace[enemy.Name]["Head"]
					end;
				else
					target = workspace[enemy.Name]["HumanoidRootPart"];
				end;

			end;
		else
			target = nil
		end
	end
end)
end)


    -- Player
    local Player = Window:NewTab("Player")
    local PlayerSection = Player:NewSection("Player")

    PlayerSection:NewButton("ESP", "ButtonInfo", function()
        local localPlayer=game.Players.LocalPlayer

        function highlightModel(objObject)
           for i,v in pairs(objObject:children())do
               if v:IsA'BasePart'and v.Name~='HumanoidRootPart'then
                   local bHA=Instance.new('BoxHandleAdornment',v)
                   bHA.Adornee=v
                   bHA.Size= v.Name=='Head' and Vector3.new(1.25,1.25,1.25) or v.Size
                   bHA.Color3=v.Name=='Head'and Color3.new(1,0,0)or v.Name=='Torso'and Color3.new(0,1,0)or Color3.new(0,0,1)
                   bHA.Transparency=.5
                   bHA.ZIndex=1
                   bHA.AlwaysOnTop=true
               end
               if #v:children()>0 then
                   highlightModel(v)
               end
           end
        end
        
        function unHighlightModel(objObject)
           for i,v in pairs(objObject:children())do
               if v:IsA'BasePart' and v:findFirstChild'BoxHandleAdornment' then
                   v.BoxHandleAdornment:Destroy()
               end
               if #v:children()>0 then
                   unHighlightModel(v)
               end
           end
        end
        
        function sortTeamHighlights(objPlayer)
           repeat wait() until objPlayer.Character
           if objPlayer.TeamColor~=localPlayer.TeamColor then
               highlightModel(objPlayer.Character)
           else
               unHighlightModel(objPlayer.Character)
           end
           if objPlayer~=localPlayer then
               objPlayer.Changed:connect(function(strProp)
                   if strProp=='TeamColor'then
                       if objPlayer.TeamColor~=localPlayer.TeamColor then
                           unHighlightModel(objPlayer.Character)
                           highlightModel(objPlayer.Character)
                       else
                           unHighlightModel(objPlayer.Character)
                       end
                   end
               end)
           else
               objPlayer.Changed:connect(function(strProp)
                   if strProp=='TeamColor'then
                       wait(.5)
                       for i,v in pairs(game.Players:GetPlayers())do
                           unHighlightModel(v)
                           if v.TeamColor~=localPlayer.TeamColor then
                               highlightModel(v.Character)
                           end
                       end
                   end
               end)
           end
        end
        
        for i,v in pairs(game.Players:GetPlayers())do
           v.CharacterAdded:connect(function()
               sortTeamHighlights(v)
           end)
           sortTeamHighlights(v)
        end
        game.Players.PlayerAdded:connect(function(objPlayer)
           objPlayer.CharacterAdded:connect(function(objChar)
               sortTeamHighlights(objPlayer)
           end)
        end)
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
