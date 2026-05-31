local OrionLib = loadstring(game:HttpGet(('https://raw.githubusercontent.com/shlexware/Orion/main/source')))()
local Window = OrionLib:MakeWindow({Name = "⚡ Blox Fruits VIP Menu", HidePremium = false, SaveConfig = false})

local FarmTab = Window:MakeTab({Name = "🌾 Auto Farm", Icon = "rbxassetid://4483345998"})
local ItemsTab = Window:MakeTab({Name = "🍎 Săn Vật Phẩm", Icon = "rbxassetid://4483345998"})
local ServerTab = Window:MakeTab({Name = "🌐 Máy Chủ", Icon = "rbxassetid://4483345998"})

FarmTab:AddToggle({
    Name = "Fast Attack (Đánh Siêu Tốc)",
    Default = false,
    Callback = function(Value)
        _G.FastAttack = Value
        while _G.FastAttack do task.wait()
            pcall(function()
                local combat = game:GetService("ReplicatedStorage").RigControllerToClient
                if game.Players.LocalPlayer.Character:FindFirstChildOfClass("Tool") then
                    combat.AttackClick(combat)
                end
            end)
        end
    end
})

FarmTab:AddToggle({
    Name = "Auto Farm Sương Katakuri",
    Default = false,
    Callback = function(Value)
        _G.FarmKatakuri = Value
        while _G.FarmKatakuri do task.wait(1)
            pcall(function()
                local boss = game.Workspace.Enemies:FindFirstChild("Dough King") or game.Workspace.Enemies:FindFirstChild("Katakuri")
                if boss and boss:FindFirstChild("Humanoid") and boss.Humanoid.Health > 0 then
                    game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = boss.HumanoidRootPart.CFrame * CFrame.new(0, 20, 0)
                end
            end)
        end
    end
})

ItemsTab:AddButton({
    Name = "Auto Thu Hoạch Trái Ác Quỷ",
    Callback = function()
        for _, obj in pairs(game.Workspace:GetChildren()) do
            if obj:IsA("Tool") and obj:FindFirstChild("Handle") then
                game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = obj.Handle.CFrame
                task.wait(0.5)
            end
        end
    end
})

ItemsTab:AddToggle({
    Name = "Auto Farm Sea Beast",
    Default = false,
    Callback = function(Value)
        _G.AutoSeaBeast = Value
    end
})

ServerTab:AddButton({
    Name = "Server Hop Hot",
    Callback = function()
        local ts = game:GetService("TeleportService")
        local hs = game:GetService("HttpService")
        local api = "https://games.roblox.com/v1/games/" .. game.PlaceId .. "/servers/Public?sortOrder=Desc&limit=100"
        local servers = hs:JSONDecode(game:HttpGet(api))
        for _, s in pairs(servers.data) do
            if s.playing < s.maxPlayers and s.id ~= game.JobId then
                ts:TeleportToPlaceInstance(game.PlaceId, s.id, game.Players.LocalPlayer)
                break
            end
        end
    end
})

OrionLib:Init()
