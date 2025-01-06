local Fluent = loadstring(game:HttpGet("https://github.com/dawid-scripts/Fluent/releases/latest/download/main.lua"))()
local SaveManager = loadstring(game:HttpGet("https://raw.githubusercontent.com/dawid-scripts/Fluent/master/Addons/SaveManager.lua"))()
local InterfaceManager = loadstring(game:HttpGet("https://raw.githubusercontent.com/dawid-scripts/Fluent/master/Addons/InterfaceManager.lua"))()

local Window = Fluent:CreateWindow({
    Title = "",
    SubTitle = "Zombie Merge Tycoon",
    TabWidth = 160,
    Size = UDim2.fromOffset(580, 460),
    Acrylic = true, -- The blur may be detectable, setting this to false disables blur entirely
    Theme = "Darker",
    MinimizeKey = Enum.KeyCode.LeftControl -- Used when theres no MinimizeKeybind
})


--Fluent provides Lucide Icons https://lucide.dev/icons/ for the tabs, icons are optional
local Tabs = {
    Exclusive = Window:AddTab({ Title = "Exclusive", Icon = "smile" }),
    Automatic = Window:AddTab({ Title = "Automatic", Icon = "refresh-ccw" }),
    Teleport = Window:AddTab({ Title = "Teleport", Icon = "compass" }),
    Settings = Window:AddTab({ Title = "Settings", Icon = "settings" })
}
--Thanks to SQK for the functions/variables💓 love from dusty

----------[ VARIABLES! ]----------
local Players = game:GetService("Players")

local lp = Players.LocalPlayer
local char = lp.Character
local hum = char.Humanoid
local hrp = char.HumanoidRootPart

local yourtycoon
local zombies
local ChoosedWeapon
local list_param_weapon = {[1] = "Damage",[2] = "Firerate",[3] = "HeadshotMultiplier"}
local param_weapon

----------[ FUNCTIONS! ]----------
local function GetTycoon()
    for i,v in pairs(game:GetService("Workspace").Tycoon.Tycoons:GetDescendants()) do
        if v:IsA("ObjectValue") and v.Name == "Owner" then
            if v.Value == lp then
                yourtycoon = v.Parent.Name
            end
        end
    end
end
GetTycoon()



local Options = Fluent.Options
do

    --------------------------------------------[ Exclusive Tab ]---------------------------------------------
    local InfSample = Tabs.Exclusive:AddToggle("SampleToggle", {Title = "Generate Samples", Default = false })
    local runLoop = false  -- Variable to control the loop
    
    InfSample:OnChanged(function(state)
        if state then
            getgenv().InfSampleExploit = true
            runLoop = true --Begins the loop cause i fucking forgot how getgenv works
            task.spawn(function()
                while runLoop do
                    task.wait(SampleDelay)
                        local args = {[1] = ZombieTier,[2] = Vector3.new(1, 1, 1)}
                        game:GetService("ReplicatedStorage").Signals.RemoteEvents.GetWoolRemote:FireServer(unpack(args))
                end
            end)
        else
            getgenv().InfSampleExploit = false
            runLoop = false  -- ends the loop
        end
    end)
    Options.SampleToggle:SetValue(false)
--small update, i remebered how to loop with bool but i already did it with state which took 8 minutes of my time so im keeping it here.

    local DelaySlider = Tabs.Exclusive:AddSlider("Slider", {
        Title = "Samples Delay",
        Description = "The delay between each sample generated.",
        Default = 1,
        Min = 0.1,
        Max = 1,
        Rounding = 1,
        Callback = function(Value)
            SampleDelay = Value
        end
    })



    local ZombieTier = Tabs.Exclusive:AddSlider("Slider", {
        Title = "Generate Samples Tier",
        Description = "Uses zombie tiers to generate samples, look at 'ZOMBIE Tier' board to see how much a tier gives.",
        Default = 1,
        Min = 1,
        Max = 19,
        Rounding = 0,
        Callback = function(Value)
            ZombieTier = Value
        end
    })
    ZombieTier:SetValue(1)

    -------------------------------------------[ Automatic Tab ]---------------------------------------------
    local CollectToggle = Tabs.Automatic:AddToggle("AutoCollect", {Title = "Auto Collect Sample", Default = false })
    CollectToggle:OnChanged(function(bool)
        getgenv().AutoCollect = bool
        runLoop = true --Begins the loop cause i fucking forgot how getgenv works
        task.spawn(function()
            while runLoop do
                task.wait(1)

                pcall(function()
                    local char = lp.Character    
                    local hum = char.Humanoid
                    local hrp = char.HumanoidRootPart
                    if AutoCollect then
                        for i,v in pairs(game:GetService("Workspace").Tycoon.Tycoons[yourtycoon].Drops:GetDescendants()) do
                            if v:IsA("Part") then
                                v.Transparency = 1
                                v.CFrame = hrp.CFrame
                            end
                        end
                    end
                end)
            end
        end)
    end)
    Options.AutoCollect:SetValue(false)

    local DepositToggle = Tabs.Automatic:AddToggle("AutoDeposit", {Title = "Auto Deposit Sample", Default = false })
    DepositToggle:OnChanged(function(bool)
        getgenv().AutoDeposit = bool
        task.spawn(function()
            while task.wait(1) do
                if AutoDeposit then
                    game:GetService("ReplicatedStorage").Signals.RemoteEvents.PutRemote:FireServer()
                    task.wait(.3)
                end
            end
        end)
    end)
    Options.AutoDeposit:SetValue(false)
    
    local UpgradeToggle = Tabs.Automatic:AddToggle("AutoUpgradeRate", {Title = "Auto Upgrade Rate", Default = false })
    UpgradeToggle:OnChanged(function(bool)
        getgenv().AutoUpgrade = bool
    
        task.spawn(function()
            while task.wait() do
                pcall(function()
                    
                    local char = lp.Character    
                    local hum = char.Humanoid
                    local hrp = char.HumanoidRootPart
                    
                    if AutoUpgrade then
                        local UpgradeButton = game:GetService("Workspace").Tycoon.Tycoons[yourtycoon]["Buttons_E"].Upgrade.Head
                        UpgradeButton.CanCollide = false
                    
                        firetouchinterest(UpgradeButton, hrp, 0)
                        firetouchinterest(UpgradeButton, hrp, 1)
                    
                        task.wait(1)
                    end
                end)
            end
        end)
    end)
    Options.AutoUpgradeRate:SetValue(false)

    local MergeToggle = Tabs.Automatic:AddToggle("AutoMergeZombies", {Title = "Auto Merge Zombies", Default = false })
    MergeToggle:OnChanged(function(bool)
        getgenv().AutoMerge = bool
    
        task.spawn(function()
            while task.wait() do
                pcall(function()
                    
                    local char = lp.Character    
                    local hum = char.Humanoid
                    local hrp = char.HumanoidRootPart
                    
                    if AutoMerge then
                        local MergeButton = game:GetService("Workspace").Tycoon.Tycoons[yourtycoon]["Buttons_E"].Merge.Head
                        MergeButton.CanCollide = false
                        
                        firetouchinterest(MergeButton, hrp, 0)
                        firetouchinterest(MergeButton, hrp, 1)
                        
                        task.wait(3)
                    end
                end)
            end
        end)
    end)
    Options.AutoMergeZombies:SetValue(false)

    local DropsToggle = Tabs.Automatic:AddToggle("AutoCollectDrops", {Title = "Auto Collect Drops", Default = false })
    DropsToggle:OnChanged(function(bool)
        getgenv().CollectDrops = bool
    
        task.spawn(function()
            while task.wait() do
                if CollectDrops then
                    pcall(function()
                        
                        local char = lp.Character    
                        local hum = char.Humanoid
                        local hrp = char.HumanoidRootPart
                        
                        for i,v in pairs(game:GetService("Workspace"):GetChildren()) do
                            if v:IsA("Part") and v.Name == "Money" then
                                v.CanCollide = false
                                v.CFrame = hrp.CFrame
                            end
                        end
                    end)
                end
            end
        end)
    end)
    Options.AutoCollectDrops:SetValue(false)


    local BuyToggle = Tabs.Automatic:AddToggle("AutoBuyZombies", {Title = "Auto Buy Zombies", Default = false })
    
    BuyToggle:OnChanged(function(bool)
        getgenv().BuyZombie = bool
        
        task.spawn(function()
            while task.wait(1) do
                pcall(function()
                    local lp = game.Players.LocalPlayer  -- Reference to the local player
                    local char = lp.Character
                    if char then
                        local hum = char:FindFirstChild("Humanoid")
                        local hrp = char:FindFirstChild("HumanoidRootPart")
                        if hum and hrp then
                            -- Only proceed if the character and its components exist
                            if getgenv().BuyZombie then
                                for i, v in pairs(game:GetService("Workspace").Tycoon.Tycoons[yourtycoon]["Buttons_E"]:GetDescendants()) do
                                    if v:IsA("MeshPart") and v.Parent.Name == zombies then
                                        v.CanCollide = false
                                        firetouchinterest(v, hrp, 0)
                                        firetouchinterest(v, hrp, 1)
                                        task.wait(0.5)
                                    end
                                end
                            end
                        end
                    end
                end)
            end
        end)
    end)




    local Dropdown = Tabs.Automatic:AddDropdown("Dropdown", {
        Title = "Auto Buy Zombies Amount",
        Values = {"+1 Zombie", "+3 Zombies", "+10 Zombies", "+30 Zombies", "+50 Zombies", "+100 Zombies"},
        Multi = false,
        Default = 1,  -- Set default to "one"
    })
    
    -- Default value, correct value from the dropdown list, not hardcoded
    Dropdown:SetValue("ten")  -- Initial selection, you can change it based on your needs
    

        Dropdown:OnChanged(function(mob)
        -- Mapping dropdown options to appropriate actions
        if mob == "+1 Zombie" then
            zombies = "Add"
        elseif mob == "+3 Zombies" then
            zombies = "Add3"
        elseif mob == "+10 Zombies" then
            zombies = "Add10"
        elseif mob == "+30 Zombies" then
            zombies = "Add30"
        elseif mob == "+50 Zombies" then
            zombies = "Add50"
        elseif mob == "+100 Zombies" then
            zombies = "Add100"
        end
    end)

    

    -------------------------------------------[ Teleport Tab ]---------------------------------------------
    local ZombieToggle = Tabs.Teleport:AddToggle("TeleportZombies", {Title = "Teleport Zombies to You", Default = false })
    ZombieToggle:OnChanged(function(bool)
        getgenv().TPZombies = bool
    
        task.spawn(function()
            while task.wait(1) do  -- Check every second
                if TPZombies then
                    pcall(function()
                        local char = game.Players.LocalPlayer.Character    
                        local hum = char:FindFirstChild("Humanoid")
                        local hrp = char:FindFirstChild("HumanoidRootPart")
                        
                        if hum and hrp then
                            local gate = game:GetService("Workspace").Tycoon.Tycoons[yourtycoon].Essentials.Gate.Beam

                            -- Teleport all zombies to the player's position
                            for i, v in pairs(game:GetService("Workspace").Zombies:GetDescendants()) do
                                if v:IsA("Part") and v.Name == "HumanoidRootPart" then  
                                    v.CFrame = hrp.CFrame + hrp.CFrame.LookVector * 7  -- Position zombies near the player
                                    task.wait(0.2)
                                end
                            end
                        end
                    end)
                end
            end
        end)
    end)
    Options.TeleportZombies:SetValue(false)


    Tabs.Teleport:AddButton({
        Title = "Copy XYZ",
        Description = "Useless unless your dusty",
        Callback = function()
                local XYZ = tostring(game.Players.LocalPlayer.Character.HumanoidRootPart.Position)
                setclipboard("game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(" .. XYZ .. ")")
        end
    })
    Tabs.Teleport:AddParagraph({
        Title = "Credits",
        Content = "Script mad by dusty.\nVariables made by SQK!"
    })
end


-- Addons:
-- SaveManager (Allows you to have a configuration system)
-- InterfaceManager (Allows you to have a interface managment system)

-- Hand the library over to our managers
SaveManager:SetLibrary(Fluent)
InterfaceManager:SetLibrary(Fluent)

-- Ignore keys that are used by ThemeManager.
-- (we dont want configs to save themes, do we?)
SaveManager:IgnoreThemeSettings()

-- You can add indexes of elements the save manager should ignore
SaveManager:SetIgnoreIndexes({})

-- use case for doing it this way:
-- a script hub could have themes in a global folder
-- and game configs in a separate folder per game
InterfaceManager:SetFolder("FluentScriptHub")
SaveManager:SetFolder("FluentScriptHub/specific-game")

InterfaceManager:BuildInterfaceSection(Tabs.Settings)
SaveManager:BuildConfigSection(Tabs.Settings)


Window:SelectTab(1)

Fluent:Notify({
    Title = "DustyHub",
    Content = "Zombie Merge Tycoon Loaded!",
    Duration = 8
})

-- You can use the SaveManager:LoadAutoloadConfig() to load a config
-- which has been marked to be one that auto loads!
SaveManager:LoadAutoloadConfig()
