local NPCS = {}

    for i, v in pairs(workspace.__GAME.__Mobs:GetDescendants()) do
        if v:IsA "Model" and v:FindFirstChild("HumanoidRootPart") then
            if not table.find(NPCS, tostring(v)) then
            table.insert(NPCS, tostring(v))
        end
    end
end
local Material = loadstring(game:HttpGet("https://raw.githubusercontent.com/Kinlei/MaterialLua/master/Module.lua"))()

local X = Material.Load({
    Title = "One Fruit",
    Style = 1,
    SizeX = 350,
    SizeY = 350,
    Theme = "Dark",
    ColorOverrides = {
        MainFrame = Color3.fromRGB(0,0,0),
        Toggle = Color3.fromRGB(124,37,255),
        ToggleAccent = Color3.fromRGB(255,255,255), 
        Dropdown = Color3.fromRGB(124,37,255),
		DropdownAccent = Color3.fromRGB(255,255,255),
        Slider = Color3.fromRGB(124,37,255),
		SliderAccent = Color3.fromRGB(255,255,255),
        NavBarAccent = Color3.fromRGB(0,0,0),
        Content = Color3.fromRGB(0,0,0),
    }
})

local Y = X.New({
    Title = "Main"
})

local Z = X.New({
    Title = "LocalPlayer"
})

local Cred = X.New({
    Title = "Credits"
})
    Cred.Button({
    Text = "Free|Script",
    Callback = function()
        setclipboard("Good")
        toclipboard("Nice|Scripts")
    end,
})

local ii = Y.Dropdown({
    Text = "Select Mobs!",
    Callback = function(Value)
        getgenv().mobname = Value
end,
    Options = NPCS
})

local function getNPC()
    local dist, thing = math.huge
    for i, v in pairs(workspace.__GAME.__Mobs:GetDescendants()) do
        if v:IsA "Model" and v:FindFirstChild("HumanoidRootPart") and v.Name == mobname then
            local mag = (game.Players.LocalPlayer.Character.HumanoidRootPart.Position - v.Parent.Position).magnitude
            if mag < dist then
                dist = mag
                thing = v
            end
        end
    end
    return thing
end
Y.Button(
    {
        Text = "Refresh Mobs",
        Callback = function()
            table.clear(NPCS)
            for i, v in pairs(workspace.__GAME.__Mobs:GetDescendants()) do
                if v:IsA "Model" and v:FindFirstChild("HumanoidRootPart") then
                    if not table.find(NPCS, tostring(v)) then
                        table.insert(NPCS, tostring(v))
                        ii:SetOptions(NPCS)
                    end
                end
            end
        end
    }
)
Y.Toggle({
    Text = "Auto Mob",
    Callback = function(Value)
        b = Value
        while b do task.wait()
                    pcall(function()
                    
                game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = getNPC().UpperTorso.CFrame
            end)
        end
	end,
    Enabled = false
})
Y.Toggle({
    Text = "Auto Attack",
    Callback = function(Value)
        a = Value
        while a do task.wait()
            local args = {
                [1] = {
                    [1] = {
                        [1] = "\4",
                        [2] = "Combat",
                        [3] = 1,
                        [4] = false,
                        [5] = game:GetService("Players").LocalPlayer.Character.Mera,
                        [6] = "Fruit"
                    },
                    [2] = {
                        [1] = "\n",
                        [2] = workspace:WaitForChild("__GAME"):WaitForChild("__Mobs"):WaitForChild("Ilha_01"):WaitForChild(Options),
                        [3] = game:GetService("Players").LocalPlayer.Character.Mera
                    }
                }
            }
            
            game:GetService("ReplicatedStorage"):WaitForChild("RemoteEvent"):FireServer(unpack(args))            
        end
	end,
    Enabled = false
})
Z.Slider({
	Text = "WalkSpeed",
	Callback = function(Value)
		getgenv().bool = Value
	end,
	Min = 50,
	Max = 500,
	Def = 50
})

    Z.Toggle({
    Text = "Toggle WalkSpeed",
    Callback = function(Value)
        g = Value
        while g do task.wait()
            game.Players.LocalPlayer.Character.Humanoid.WalkSpeed = bool
        end
	end,
    Enabled = false
})
for i,v in pairs(getconnections(game.Players.LocalPlayer.Idled)) do
    v:Disable()
end
