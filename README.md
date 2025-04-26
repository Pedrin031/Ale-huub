ocal Pedrin031 = loadstring(game:HttpGet(\"https://raw.githubusercontent.com/hassanxzayn-lua/NEOXHUBMAIN/refs/heads/main/newneoxlib\"))()

local Window = Neox:CreateWindow({
    Title = \"ALE HUB  | Dead Rails v1.9.4\",
    SubTitle = \"by Pedrin031\",
    TabWidth = 120,
    Size = UDim2.fromOffset(600, 450),
    Acrylic = false,
    Theme = \"Darker\",
    MinimizeKey = Enum.KeyCode.Q
})
local Tabs = {
    Home = Window:AddTab({ Title = \" Home\", Icon = \"rbxassetid://7733960981\" }),
    Main = Window:AddTab({ Title = \" Main\", Icon = \"rbxassetid://7743869612\" }),
    Autofarm = Window:AddTab({ Title = \" Auto\", Icon = \"rbxassetid://7733917120\" }),
    Aimbot = Window:AddTab({ Title = \" Aimbot\", Icon = \"rbxassetid://7743878857\" }),
    Misc = Window:AddTab({ Title = \" Misc\", Icon = \"rbxassetid://7733789088\" }),
    Weather = Window:AddTab({ Title = \" Environment\", Icon = \"rbxassetid://7734068495\" }),
    Performance = Window:AddTab({ Title = \" Performance\", Icon = \"rbxassetid://7733955511\" }),
    Esp = Window:AddTab({ Title = \" Esp\", Icon = \"rbxassetid://7743875962\" }),
    Train = Window:AddTab({ Title = \" Train Stuff\", Icon = \"rbxassetid://7733954611\" }),
    Teleport = Window:AddTab({ Title = \" Teleport\", Icon = \"rbxassetid://7733992789\" }),
}
Window:SelectTab(1)
local Section = Tabs.Home:AddSection(\"Executor: \" .. (identifyexecutor() or \"Unknown Executor\"))
local Section = Tabs.Home:AddSection(\"\")

Tabs.Home:AddButton({
    Title = \"Join our discord\",
    Description = \"Click on this button to copy the invite link!\",
    Callback = function()
        local inviteLink = \"https://discord.gg/wj5xpM8q\"
        
        if setclipboard then
            setclipboard(inviteLink)
            print(\"bro coming to our discord yay!\")
        else
            print(\"setclipboard function not available\")
        end
    end
})

Tabs.Home:AddParagraph({
    Title = \"Update v1.9.4\",
    Content = \"[+] = Added     [-] = Fixed\\n-----------------------------\\n[+] Added Transparent House Walls\\n\"
})

Tabs.Main:AddParagraph({
    Title = \"Patched - 3/2/25\",
    Content = \"Hello everyone sorry to say but due to the latest update in Dead Rails the Bring Items feature has been patched i am currently looking for a workaround to make it functional again thank you for your understanding\"
})

local function GetItemNames()
    local items = {}
    local itemCounts = {}

    local runtimeItems = workspace:FindFirstChild(\"RuntimeItems\")
    if runtimeItems then
        for _, item in ipairs(runtimeItems:GetChildren()) do
            if item:IsA(\"Model\") then
                if not itemCounts[item.Name] then
                    itemCounts[item.Name] = 1
                    table.insert(items, item.Name) 
                else
                    itemCounts[item.Name] = itemCounts[item.Name] + 1
                end
            end
        end
    else
        warn(\"not found\")
    end
    
    for i, itemName in ipairs(items) do
        local count = itemCounts[itemName]
        if count > 1 then
            items[i] = itemName .. \" x\" .. count 
        end
    end
    
    return items
end

local selectedItemName = \"None\"

local Dropdown = Tabs.Main:AddDropdown(\"ItemDropdown\", {
    Title = \"Choose Item\",
    Description = \"\",
    Values = GetItemNames(),
    Multi = false,
    Default = \"None\",
    Callback = function(option)
        selectedItemName = option
    end
})

Tabs.Main:AddButton({
    Title = \"Refresh Items\",
    Description = \"\",
    Callback = function()
        local newItems = GetItemNames()
        Dropdown:SetValues(newItems)
        Dropdown:SetValue(\"None\")
        selectedItemName = \"None\"
    end
})

Tabs.Main:AddButton({
    Title = \"Collect Selected Item\",
    Description = \"\",
    Callback = function()
        if selectedItemName == \"None\" then
            warn(\"nno item selected\")
            return
        end

        local itemName = selectedItemName:match(\"^(.-) x%d+$\") or selectedItemName

        local runtimeItems = workspace:FindFirstChild(\"RuntimeItems\")
        if not runtimeItems then
            warn(\"not found\")
            return
        end

        local selectedItem
        for _, item in ipairs(runtimeItems:GetChildren()) do
            if item:IsA(\"Model\") and item.Name == itemName then
                selectedItem = item
                break
            end
        end

        if not selectedItem then
            warn(\"not found:\", itemName)
            return
        end

        local LocalPlayer = game:GetService(\"Players\").LocalPlayer
        if not LocalPlayer then
            warn(\"not found!\")
            return
        end

        local Character = LocalPlayer.Character or LocalPlayer.CharacterAdded:Wait()
        local HumanoidRootPart = Character:WaitForChild(\"HumanoidRootPart\")

        if not selectedItem.PrimaryPart then
            warn(selectedItem.Name .. \"  cant moved\")
            return
        end

        selectedItem:SetPrimaryPartCFrame(HumanoidRootPart.CFrame + Vector3.new(0, 1, 0))
        print(\"collected:\", selectedItem.Name)
    end
})

Tabs.Main:AddButton({
    Title = \"Collect All Items\",
    Description = \"\",
    Callback = function()
        local runtimeItems = workspace:FindFirstChild(\"RuntimeItems\")
        if not runtimeItems then
            warn(\"not found\")
            return
        end

        local LocalPlayer = game:GetService(\"Players\").LocalPlayer
        local Character = LocalPlayer.Character or LocalPlayer.CharacterAdded:Wait()
        local HumanoidRootPart = Character:WaitForChild(\"HumanoidRootPart\")

        for _, item in ipairs(runtimeItems:GetChildren()) do
            if item:IsA(\"Model\") then
                if item.PrimaryPart then
                    local offset = HumanoidRootPart.CFrame.LookVector * 5
                    item:SetPrimaryPartCFrame(HumanoidRootPart.CFrame + offset)
                else
                    warn(item.Name .. \" has no part\")
                end
            end
        end
    end
})

Tabs.Main:AddButton({
    Title = \"Bring Coal\",
    Description = \"\",
    Callback = function()
        local runtimeItems = workspace:FindFirstChild(\"RuntimeItems\")
        if not runtimeItems then
            warn(\"not found\")
            return
        end

        local LocalPlayer = game:GetService(\"Players\").LocalPlayer
        if not LocalPlayer then
            warn(\"not found\")
            return
        end

        local Character = LocalPlayer.Character or LocalPlayer.CharacterAdded:Wait()
        local HumanoidRootPart = Character:WaitForChild(\"HumanoidRootPart\")

        local coalCount = 0
        for _, item in ipairs(runtimeItems:GetChildren()) do
            if item:IsA(\"Model\") and item.Name == \"Coal\" then
                if not item.PrimaryPart then
                    warn(\"cant moved\")
                    continue
                end
                
                local offset = Vector3.new(
                    math.random(-10, 10) * 0.1,  
                    1 + (coalCount * 0.2),       
                    math.random(-10, 10) * 0.1   
                )
                
                item:SetPrimaryPartCFrame(HumanoidRootPart.CFrame + offset)
                coalCount = coalCount + 1
            end
        end

        if coalCount == 0 then
            warn(\"coal not found\")
        else
            print(\"collected:\", coalCount, \"coal items\")
        end
    end
})

Tabs.Main:AddButton({
    Title = \"Bring Bandages\",
    Description = \"\",
    Callback = function()
        local runtimeItems = workspace:FindFirstChild(\"RuntimeItems\")
        if not runtimeItems then
            warn(\"not found\")
            return
        end

        local LocalPlayer = game:GetService(\"Players\").LocalPlayer
        if not LocalPlayer then
            warn(\"not found\")
            return
        end

        local Character = LocalPlayer.Character or LocalPlayer.CharacterAdded:Wait()
        local HumanoidRootPart = Character:WaitForChild(\"HumanoidRootPart\")

        local bandageCount = 0
        for _, item in ipairs(runtimeItems:GetChildren()) do
            if item:IsA(\"Model\") and item.Name == \"Bandage\" then
                if not item.PrimaryPart then
                    warn(\"cant moved\")
                    continue
                end
                
                local offset = Vector3.new(
                    math.random(-10, 10) * 0.1, 
                    1 + (bandageCount * 0.2),
                    math.random(-10, 10) * 0.1  
                )
                
                item:SetPrimaryPartCFrame(HumanoidRootPart.CFrame + offset)
                bandageCount = bandageCount + 1
            end
        end

        if bandageCount == 0 then
            warn(\"noo bandages found\")
        else
            print(\"collected:\", bandageCount, \"bandages\")
        end
    end
})

local Section = Tabs.Main:AddSection(\"_____________________________________________________________\")

local Players = game:GetService(\"Players\")
local RunService = game:GetService(\"RunService\")
local ProximityPromptService = game:GetService(\"ProximityPromptService\")
local Lighting = game:GetService(\"Lighting\")
local LocalPlayer = Players.LocalPlayer

local nobandagedelay = false
local ShowTextRuntimeItems = false  
local ApplyGunMods = false
local AutoHeal = false
local Fb = false 
local AmbientChanger = false 
local noholddelay = false
local HealAt = 45
local firerate = 0.3
local spread = 0.7
local reloadtime = 2
local XSizeText = 1  
local YSizeText = 1
local AmbientColor = Color3.new(1, 1, 1) 
local RuntimeItems = game.Workspace 

RunService.RenderStepped:Connect(function()
    if nobandagedelay and LocalPlayer.PlayerGui.BandageUse.Enabled and LocalPlayer.Character then
        local Bandage = LocalPlayer.Character and LocalPlayer.Character:FindFirstChild(\"Bandage\")
        if Bandage ~= nil then
            Bandage.Use:FireServer()
        end
    end

    if ShowTextRuntimeItems then
        for i, v in next, RuntimeItems:GetChildren() do
            if v:IsA(\"Model\") and v:FindFirstChild(\"ObjectInfo\") then
                v.ObjectInfo.Enabled = true
                v.ObjectInfo.StudsOffset = Vector3.new(0, 0, 0)
                v.ObjectInfo.Size = UDim2.new(XSizeText, 0, YSizeText, 0)
            end
        end
    end

    if ApplyGunMods and LocalPlayer.Character and LocalPlayer.Character:FindFirstChildWhichIsA(\"Tool\") then
        local Tool = LocalPlayer.Character:FindFirstChildWhichIsA(\"Tool\")
        local Config = Tool:FindFirstChildWhichIsA(\"Configuration\")
        if Tool.Name == \"NavyRevolver\" or Tool.Name == \"Shotgun\" or Tool.Name == \"Rifle\" or Tool.Name == \"Sawed-Off Shotgun\" or Tool.Name == \"Revolver\" or Tool.Name == \"Mauser\" then
            if Config and (Config:FindFirstChild(\"FireDelay\") or Config:FindFirstChild(\"SpreadAngle\") or Config:FindFirstChild(\"ReloadDuration\")) then
                Config.FireDelay.Value = firerate
                Config.SpreadAngle.Value = spread
                Config.ReloadDuration.Value = reloadtime
            end
        end
    end

    if AutoHeal and LocalPlayer.Character and LocalPlayer.Character.Humanoid and LocalPlayer.Character.Humanoid.Health < HealAt then
        local Bandage = game:GetService(\"Players\").LocalPlayer.Backpack:FindFirstChild(\"Bandage\")
        if Bandage ~= nil then
            Bandage.Use:FireServer()
        end
    end

    if Fb then
        Lighting.ClockTime = 14.5
        Lighting.Brightness = 3
    end

    if AmbientChanger then
        Lighting.Ambient = AmbientColor
    end
end)

ProximityPromptService.PromptButtonHoldBegan:Connect(function(prompt)
    if noholddelay then
        prompt.HoldDuration = 0
    end
end)

local AutoHealAt = Tabs.Main:AddSlider(\"AutoHealAt\", {
    Title = \"Auto Heal At:\",
    Description = \"\",
    Default = 45,
    Min = 1,
    Max = 100,
    Rounding = 0,
    Callback = function(Value)
        HealAt = Value
    end
})

local AutoHealToggle = Tabs.Main:AddToggle(\"AutoHeal\", {
    Title = \"Auto Heal\",
    Description = \"\",
    Default = false,
    Callback = function(Value)
        AutoHeal = Value
    end
})

local NoPromptHold = Tabs.Main:AddToggle(\"NoPromptHold\", {
    Title = \"No proximityprompts hold time\",
    Description = \"\",
    Default = false,
    Callback = function(Value)
        noholddelay = Value
    end
})

local NoBandageDelay = Tabs.Main:AddToggle(\"NoBandageDelay\", {
    Title = \"No bandage delay use\",
    Description = \"\",
    Default = false,
    Callback = function(Value)
        nobandagedelay = Value
    end
})

local FirerateChanger = Tabs.Aimbot:AddSlider(\"FirerateChanger\", {
    Title = \"Firerate Changer\",
    Description = \"\",
    Default = 0.3,
    Min = 0.1,
    Max = 3,
    Rounding = 1,
    Callback = function(Value)
        firerate = Value
    end
})

local SpreadChanger = Tabs.Aimbot:AddSlider(\"SpreadChanger\", {
    Title = \"Spread Changer\",
    Description = \"\",
    Default = 0.7,
    Min = 0,
    Max = 10,
    Rounding = 1,
    Callback = function(Value)
        spread = Value
    end
})

local ReloadTimeChanger = Tabs.Aimbot:AddSlider(\"ReloadTimeChanger\", {
    Title = \"Reload Time Changer\",
    Description = \"\",
    Default = 2,
    Min = 0.1,
    Max = 3,
    Rounding = 1,
    Callback = function(Value)
        reloadtime = Value
    end
})



local ApplyGunModsToggle = Tabs.Aimbot:AddToggle(\"ApplyGunMods\", {
    Title = \"Apply Gun Mods\",
    Description = \"Enable gun modifications\",
    Default = false,
    Callback = function(Value)
        ApplyGunMods = Value
    end
})



local Players = game:GetService(\"Players\")
local RunService = game:GetService(\"RunService\")
local Lighting = game:GetService(\"Lighting\")
local LocalPlayer = Players.LocalPlayer

local Fb = false
local AmbientChanger = false
local AmbientColor = Color3.fromRGB(156, 156, 156)

RunService.RenderStepped:Connect(function()
    if Fb then
        Lighting.ClockTime = 14.5
        Lighting.Brightness = 3
    end

    if AmbientChanger then
        Lighting.Ambient = AmbientColor
    end
end)

local originalSettings = nil 

local Toggle = Tabs.Weather:AddToggle(\"RemoveDarknessToggle\", 
{
    Title = \"Remove Darkness\", 
    Description = \"Removes all dark spots in the game\",
    Default = false,
    Callback = function(state)
        local lighting = game:GetService(\"Lighting\")
        
        if state then
            originalSettings = {
                Brightness = lighting.Brightness,
                GlobalShadows = lighting.GlobalShadows,
                FogEnd = lighting.FogEnd,
                FogStart = lighting.FogStart,
                Ambient = lighting.Ambient,
                OutdoorAmbient = lighting.OutdoorAmbient,
                ShadowSoftness = lighting.ShadowSoftness,
                ClockTime = lighting.ClockTime
            }
            
            lighting.Brightness = 2
            lighting.GlobalShadows = false
            lighting.FogEnd = 10000
            lighting.FogStart = 0
            lighting.Ambient = Color3.fromRGB(128, 128, 128)
            lighting.OutdoorAmbient = Color3.fromRGB(150, 150, 150)
            lighting.ShadowSoftness = 0
            lighting.ClockTime = 12

            if lighting:FindFirstChild(\"Atmosphere\") then
                lighting.Atmosphere:Destroy()
            end

            local sunRays = lighting:FindFirstChild(\"SunRays\") or Instance.new(\"SunRaysEffect\")
            sunRays.Intensity = 0.3
            sunRays.Spread = 0.7
            sunRays.Parent = lighting

            if lighting:FindFirstChild(\"ColorCorrection\") then
                lighting.ColorCorrection:Destroy()
            end

            print(\"darkness removed \")
            
        else
            if originalSettings then
                lighting.Brightness = originalSettings.Brightness
                lighting.GlobalShadows = originalSettings.GlobalShadows
                lighting.FogEnd = originalSettings.FogEnd
                lighting.FogStart = originalSettings.FogStart
                lighting.Ambient = originalSettings.Ambient
                lighting.OutdoorAmbient = originalSettings.OutdoorAmbient
                lighting.ShadowSoftness = originalSettings.ShadowSoftness
                lighting.ClockTime = originalSettings.ClockTime
            end
        end
    end 
})

Tabs.Weather:AddButton({
    Title = \"Remove Clouds\",
    Description = \"\",
    Callback = function()
local workspace = game:GetService(\"Workspace\")
local terrain = workspace.Terrain

if terrain:FindFirstChild(\"Clouds\") then
    terrain.Clouds:Destroy()
    print(\"clouds removed\")
else
    print(\"no clouds found\")
end
    end
})

Tabs.Weather:AddButton({
    Title = \"Remove LightingPresets\",
    Description = \"\",
    Callback = function()
local replicatedStorage = game:GetService(\"ReplicatedStorage\")
local sharedFolder = replicatedStorage:FindFirstChild(\"Shared\")
local dataBanksFolder = sharedFolder and sharedFolder:FindFirstChild(\"DataBanks\")
local lightingPresets = dataBanksFolder and dataBanksFolder:FindFirstChild(\"LightingPresets\")

if lightingPresets then
    lightingPresets:Destroy()
    print(\"lightingpresets removed\")
elseif dataBanksFolder then
    print(\"noo lightingpresets found\")
elseif sharedFolder then
    print(\"no folder found\")
else
    print(\"no folder found\")
end
    end
})

local AmbientChangerToggle = Tabs.Weather:AddToggle(\"AmbientChanger\", {
    Title = \"Ambient Changer\",
    Description = \"\",
    Default = false,
    Callback = function(Value)
        AmbientChanger = Value
        if not Value then
            Lighting.Ambient = Color3.fromRGB(255, 255, 255)
        end
    end    
})

local AmbientColorPicker = Tabs.Weather:AddColorpicker(\"AmbientColorChanger\", {
    Title = \"Ambient Color Changer\",
    Description = \"\",
    Default = Color3.fromRGB(156, 156, 156),
    Callback = function(Value)
        AmbientColor = Value
    end
})

local Players = game:GetService(\"Players\")
local Workspace = game:GetService(\"Workspace\")
local CoreGui = game:GetService(\"CoreGui\")
local LocalPlayer = Players.LocalPlayer
local Character = LocalPlayer.Character or LocalPlayer.CharacterAdded:Wait()
local HumanoidRootPart = Character:WaitForChild(\"HumanoidRootPart\")

local screenGui = Instance.new(\"ScreenGui\")
screenGui.Name = \"EnemyDetector\"
screenGui.ResetOnSpawn = false 
screenGui.Parent = CoreGui    

local enemyLabel = Instance.new(\"TextLabel\")
enemyLabel.Parent = screenGui
enemyLabel.Size = UDim2.new(0, 200, 0, 30)
enemyLabel.Position = UDim2.new(0.5, -100, 0, 20)
enemyLabel.BackgroundTransparency = 1
enemyLabel.Text = \"\"
enemyLabel.TextColor3 = Color3.fromRGB(255, 0, 0)
enemyLabel.TextSize = 24
enemyLabel.Font = Enum.Font.SourceSansBold
enemyLabel.TextStrokeTransparency = 0.5

local distanceLabel = Instance.new(\"TextLabel\")
distanceLabel.Parent = screenGui
distanceLabel.Size = UDim2.new(0, 200, 0, 20)
distanceLabel.Position = UDim2.new(0.5, -100, 0, 50)
distanceLabel.BackgroundTransparency = 1
distanceLabel.Text = \"\"
distanceLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
distanceLabel.TextSize = 18
distanceLabel.Font = Enum.Font.SourceSans
distanceLabel.TextStrokeTransparency = 0.5

local detectionEnabled = false

local function checkEnemies()
    if not detectionEnabled then
        enemyLabel.Text = \"\"
        distanceLabel.Text = \"\"
        return
    end
    
    if not HumanoidRootPart or not HumanoidRootPart.Parent then
        enemyLabel.Text = \"\"
        distanceLabel.Text = \"\"
        return
    end
    
    local closestDistance = math.huge
    local enemyCount = 0
    
    local success, err = pcall(function()
        for _, obj in ipairs(Workspace:GetDescendants()) do
            local humanoid = obj:FindFirstChild(\"Humanoid\")
            local rootPart = obj:FindFirstChild(\"HumanoidRootPart\")
            if humanoid and humanoid.Health > 0 and obj ~= Character and not Players:GetPlayerFromCharacter(obj.Parent) then
                local targetPos = rootPart and rootPart.Position or humanoid:GetPivot().Position
                local distance = (HumanoidRootPart.Position - targetPos).Magnitude
                if distance <= 50 and distance > 0.1 then
                    enemyCount = enemyCount + 1
                    if distance < closestDistance then
                        closestDistance = distance
                    end
                end
            end
        end
    end)
    
    if not success then
        warn(\"enemy error: \" .. err)
        enemyLabel.Text = \"\"
        distanceLabel.Text = \"\"
        return
    end
    
    if enemyCount > 0 then
        enemyLabel.Text = \"Someone Nearby x\" .. enemyCount
        distanceLabel.Text = string.format(\"%.1f studs\", closestDistance)
    else
        enemyLabel.Text = \"\"
        distanceLabel.Text = \"\"
    end
end

LocalPlayer.CharacterAdded:Connect(function(newChar)
    Character = newChar
    HumanoidRootPart = Character:WaitForChild(\"HumanoidRootPart\", 5)
    if detectionEnabled then
        checkEnemies()
    end
end)

LocalPlayer.CharacterRemoving:Connect(function()
end)

task.spawn(function()
    while true do
        local success, err = pcall(checkEnemies)
        if not success then
            warn(\"loop error: \" .. err)
            enemyLabel.Text = \"\"
            distanceLabel.Text = \"\"
        end
        task.wait(0.3)
    end
end)

local EnemyDetection = Tabs.Main:AddToggle(\"EnemyDetection\", {
    Title = \"Enemy Detection\",
    Description = \"The enemy detection feature works best in single-player mode because, in multiplayer, it also detects your teammates. Additionally, the detection range is set to 50 studs\",
    Default = false,
    Callback = function(Value)
        detectionEnabled = Value
        if not Value then
            enemyLabel.Text = \"\"
            distanceLabel.Text = \"\"
        end
    end
})

script.Destroying:Connect(function()
    if screenGui then
        screenGui:Destroy()
    end
end)

local folder = game.Workspace.RandomBuildings
local CoreGui = game:GetService(\"CoreGui\")
local StarterGui = game:GetService(\"StarterGui\")

local isMonitoring = false
local connection = nil

local function createGUI(modelName)
    local player = game.Players.LocalPlayer
    local playerGui = player:WaitForChild(\"PlayerGui\")
    
    local spawnedgui = Instance.new(\"ScreenGui\")
    spawnedgui.Name = \"spawned gui\"
    spawnedgui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
    spawnedgui.ResetOnSpawn = false
    
    local main = Instance.new(\"TextLabel\")
    main.Name = \"main\"
    main.Parent = spawnedgui
    main.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
    main.BackgroundTransparency = 1.000
    main.BorderColor3 = Color3.fromRGB(0, 0, 0)
    main.BorderSizePixel = 0
    main.Position = UDim2.new(0.321157932, 0, 0.0558240898, 0)
    main.Size = UDim2.new(0.36191377, 0, 0.0477249362, 0)
    main.Font = Enum.Font.Highway
    main.Text = modelName .. \" has been spawned\"
    main.TextColor3 = Color3.fromRGB(255, 0, 4)
    main.TextScaled = true
    main.TextSize = 14.000
    main.TextStrokeTransparency = 0.000
    main.TextWrapped = true
    
    spawnedgui.Parent = CoreGui
    
    spawn(function()
        wait(3)
        for i = 1, 20 do
            main.TextTransparency = i/20
            main.TextStrokeTransparency = i/20 + 0.5
            wait(0.05)
        end
        spawnedgui:Destroy()
    end)
end

local function toggleMonitoring(state)
    if state then
        if not isMonitoring then
            isMonitoring = true
            local previousChildren = {}
            for _, child in pairs(folder:GetChildren()) do
                if child:IsA(\"Model\") then
                    previousChildren[child] = true
                end
            end
            
            local RunService = game:GetService(\"RunService\")
            connection = RunService.Heartbeat:Connect(function()
                for _, child in pairs(folder:GetChildren()) do
                    if child:IsA(\"Model\") and not previousChildren[child] then
                        createGUI(child.Name)
                        previousChildren[child] = true
                    end
                end
                
                for child in pairs(previousChildren) do
                    if not child.Parent then
                        previousChildren[child] = nil
                    end
                end
            end)
            
            local player = game.Players.LocalPlayer
            player.CharacterAdded:Connect(function()
                if isMonitoring then
                    for _, child in pairs(folder:GetChildren()) do
                        if child:IsA(\"Model\") and not previousChildren[child] then
                            createGUI(child.Name)
                            previousChildren[child] = true
                        end
                    end
                end
            end)
        end
    else
        if isMonitoring and connection then
            connection:Disconnect()
            connection = nil
            isMonitoring = false
        end
    end
end

local Toggle = Tabs.Main:AddToggle(\"SpawnNotifyToggle\",  
{
    Title = \"Building Spawn Message\", 
    Description = \"\",
    Default = false,
    Callback = function(state)
        toggleMonitoring(state)
    end 
})

Tabs.Main:AddButton({
    Title = \"Transparent House Walls\",
    Description = \"\",
    Callback = function()
        while true do
            local randomBuildings = game.Workspace:WaitForChild(\"RandomBuildings\")
            
            for _, child in pairs(randomBuildings:GetChildren()) do
                if child:IsA(\"Model\") then
                    local colorWall = child:FindFirstChild(\"ColorWall\")
                    if colorWall and colorWall:IsA(\"MeshPart\") then
                        colorWall.Transparency = 1
                    end
                end
            end
            
            wait(0.1) 
        end
    end
})

local Section = Tabs.Aimbot:AddSection(\"_____________________________________________________________\")

local Players = game:GetService(\"Players\")
local Workspace = game:GetService(\"Workspace\")
local ReplicatedStorage = game:GetService(\"ReplicatedStorage\")
local LocalPlayer = Players.LocalPlayer

local aimbotEnabled = false
_G.AutoSwingEnabled = false
_G.AutoSwingTask = nil

local MeleeKillAura = Tabs.Aimbot:AddToggle(\"MeleeKillAura\", {
    Title = \"Melee Kill Aura\",
    Description = \"\",
    Default = false,
    Callback = function(state)
        if state then
            local Character = LocalPlayer.Character or LocalPlayer.CharacterAdded:Wait()
            local Humanoid = Character:WaitForChild(\"Humanoid\")
            local HumanoidRootPart = Character:WaitForChild(\"HumanoidRootPart\")

            local function getTool()
                for _, tool in ipairs(Character:GetChildren()) do
                    if tool:IsA(\"Tool\") and tool:FindFirstChild(\"MeleeSwing\") and tool.Parent == Character then
                        return tool
                    end
                end
                return nil
            end

            local function checkNPCs()
                local tool = getTool()
                if Humanoid.Health <= 0 or not tool then return end
                local RemoteEvent = tool:FindFirstChild(\"SwingEvent\")
                if not RemoteEvent or tool.Parent ~= Character then return end
                for _, obj in ipairs(Workspace:GetDescendants()) do
                    local humanoid, rootPart = obj:FindFirstChild(\"Humanoid\"), obj:FindFirstChild(\"HumanoidRootPart\")
                    if humanoid and humanoid.Health > 0 and not Players:GetPlayerFromCharacter(obj) then
                        local targetPos = rootPart and rootPart.Position or humanoid.Position
                        if (HumanoidRootPart.Position - targetPos).Magnitude <= 18 then
                            RemoteEvent:FireServer(targetPos)
                        end
                    end
                end
            end

            _G.AutoSwingTask = task.spawn(function()
                while task.wait(0.3) do 
                    if not _G.AutoSwingEnabled then break end
                    checkNPCs() 
                end
            end)
            _G.AutoSwingEnabled = true
        else
            _G.AutoSwingEnabled = false
            if _G.AutoSwingTask then
                task.cancel(_G.AutoSwingTask)
                _G.AutoSwingTask = nil
            end
        end
    end    
})




local AutoAimShoot = Tabs.Aimbot:AddToggle(\"AutoAimShoot\", {
    Title = \"Gun Kill Aura\",
    Description = \"\",
    Default = false,
    Callback = function(value)
        aimbotEnabled = value
        if aimbotEnabled then
            spawn(function()
                local Character = LocalPlayer.Character or LocalPlayer.CharacterAdded:Wait()
                local HumanoidRootPart = Character:WaitForChild(\"HumanoidRootPart\", 5)

                local utility = {}
                function utility.getEquippedTool()
                    for _, tool in ipairs(Character:GetChildren()) do
                        if tool:IsA(\"Tool\") and tool:FindFirstChild(\"ClientWeaponState\") then
                            return tool
                        end
                    end
                    return nil
                end

                function utility.getClosestNPC(maxDistance)
                    local closestNPC, minDist = nil, maxDistance or 30
                    for _, obj in ipairs(Workspace:GetDescendants()) do
                        local root = obj:FindFirstChild(\"HumanoidRootPart\")
                        local humanoid = obj:FindFirstChild(\"Humanoid\")
                        if root and humanoid and humanoid.Health > 0 and not Players:GetPlayerFromCharacter(obj) then
                            local distance = (HumanoidRootPart.Position - root.Position).Magnitude
                            if distance < minDist then
                                closestNPC, minDist = humanoid, distance
                            end
                        end
                    end
                    return closestNPC
                end

                function utility.reloadWeapon(weapon, state)
                    local ammo = state:FindFirstChild(\"CurrentAmmo\")
                    if ammo and ammo.Value == 0 and not state.IsReloading.Value then
                        ReplicatedStorage.Remotes.Weapon.Reload:FireServer(Workspace:GetServerTimeNow(), weapon)
                        repeat task.wait() until ammo.Value > 0
                    end
                end

                function utility.fireAtNPC(npc, weapon, state)
                    if npc and weapon and state then
                        local ammo = state:FindFirstChild(\"CurrentAmmo\")
                        if ammo and ammo.Value > 0 then
                            ReplicatedStorage.Remotes.Weapon.Shoot:FireServer(Workspace:GetServerTimeNow(), weapon, HumanoidRootPart.CFrame, { [\"1\"] = npc })
                        end
                    end
                end

                while aimbotEnabled and task.wait(0.2) do
                    local npc = utility.getClosestNPC(30)
                    local weapon = utility.getEquippedTool()
                    if weapon then
                        local state = weapon:FindFirstChild(\"ClientWeaponState\")
                        if state then
                            utility.reloadWeapon(weapon, state)
                            utility.fireAtNPC(npc, weapon, state)
                        end
                    end
                end
            end)
        end
    end
})




Tabs.Aimbot:AddButton({
    Title = \"NPC Camlock + NPC Hitbox + NPC Lock GUI\",
    Description = \"\",
    Callback = function()
        local Players = game:GetService(\"Players\")
        local RunService = game:GetService(\"RunService\")
        local UserInputService = game:GetService(\"UserInputService\")
        local Workspace = game:GetService(\"Workspace\")
        local StarterGui = game:GetService(\"StarterGui\")
        
        local player = Players.LocalPlayer
        player.CameraMode = Enum.CameraMode.Classic
        local cam = Workspace.CurrentCamera
        local camlockEnabled = false
        local camlockConnection
        local targetESP = nil
        local hitboxToggleEnabled = false
        local npcLock = false
        local lastTarget = nil
        local toggleLoop
        
        local defaultHRPValues = {}
        local trackedNPCs = {}
        
        for _, obj in ipairs(Workspace:GetDescendants()) do
            if obj:IsA(\"Model\") 
               and obj:FindFirstChild(\"HumanoidRootPart\") 
               and obj:FindFirstChild(\"Humanoid\") 
               and not Players:GetPlayerFromCharacter(obj) then
                local hrp = obj.HumanoidRootPart
                local humanoid = obj.Humanoid
                if defaultHRPValues[hrp] == nil then
                    defaultHRPValues[hrp] = {
                        Size = hrp.Size,
                        Transparency = hrp.Transparency,
                        CanCollide = hrp.CanCollide
                    }
                end
                humanoid.Died:Connect(function()
                    task.wait(0.2)
                    local defaults = defaultHRPValues[hrp]
                    if defaults then
                        hrp.Size = defaults.Size
                        hrp.Transparency = defaults.Transparency
                        hrp.CanCollide = defaults.CanCollide
                    end
                end)
            end
        end
        
        local screenGui = Instance.new(\"ScreenGui\")
        screenGui.Name = \"NPCCamlockGUI\"
        screenGui.ResetOnSpawn = false
        screenGui.Parent = player:WaitForChild(\"PlayerGui\")
        
        local mainFrame = Instance.new(\"Frame\")
        mainFrame.Size = UDim2.new(0, 200, 0, 240)
        mainFrame.Position = UDim2.new(0.85, -100, 0.75, -120)
        mainFrame.AnchorPoint = Vector2.new(0.5, 0.5)
        mainFrame.BackgroundColor3 = Color3.fromRGB(20, 20, 25)
        mainFrame.BackgroundTransparency = 0
        mainFrame.BorderSizePixel = 0
        mainFrame.Active = true
        mainFrame.Draggable = true
        mainFrame.Parent = screenGui
        
        local uicorner = Instance.new(\"UICorner\")
        uicorner.CornerRadius = UDim.new(0, 8)
        uicorner.Parent = mainFrame
        
        local uiStroke = Instance.new(\"UIStroke\")
        uiStroke.Thickness = 1
        uiStroke.Color = Color3.fromRGB(50, 50, 60)
        uiStroke.Transparency = 0.5
        uiStroke.Parent = mainFrame
        
        local headerFrame = Instance.new(\"Frame\")
        headerFrame.Size = UDim2.new(1, 0, 0, 35)
        headerFrame.BackgroundColor3 = Color3.fromRGB(25, 25, 30)
        headerFrame.BorderSizePixel = 0
        headerFrame.Parent = mainFrame
        
        local headerCorner = Instance.new(\"UICorner\")
        headerCorner.CornerRadius = UDim.new(0, 8)
        headerCorner.Parent = headerFrame
        
        local headerLabel = Instance.new(\"TextLabel\")
        headerLabel.Size = UDim2.new(0.9, 0, 0, 20)
        headerLabel.Position = UDim2.new(0.05, 0, 0.5, -10)
        headerLabel.BackgroundTransparency = 1
        headerLabel.Text = \"NEOX HUB\"
        headerLabel.TextColor3 = Color3.fromRGB(220, 220, 255)
        headerLabel.Font = Enum.Font.GothamBold
        headerLabel.TextSize = 16
        headerLabel.Parent = headerFrame
        
        local minimizeButton = Instance.new(\"TextButton\")
        minimizeButton.Size = UDim2.new(0, 25, 0, 25)
        minimizeButton.Position = UDim2.new(1, -35, 0.5, -12)
        minimizeButton.BackgroundColor3 = Color3.fromRGB(40, 40, 50)
        minimizeButton.Text = \"-\"
        minimizeButton.TextColor3 = Color3.fromRGB(200, 200, 200)
        minimizeButton.Font = Enum.Font.Gotham
        minimizeButton.TextSize = 16
        minimizeButton.Parent = headerFrame
        
        local minCorner = Instance.new(\"UICorner\")
        minCorner.CornerRadius = UDim.new(0, 5)
        minCorner.Parent = minimizeButton
        
        local toggleButton = Instance.new(\"TextButton\")
        toggleButton.Size = UDim2.new(0.9, 0, 0, 35)
        toggleButton.Position = UDim2.new(0.05, 0, 0, 50)
        toggleButton.BackgroundColor3 = Color3.fromRGB(30, 30, 35)
        toggleButton.Text = \"NPC Camlock: OFF\"
        toggleButton.TextColor3 = Color3.fromRGB(180, 180, 200)
        toggleButton.Font = Enum.Font.GothamSemibold
        toggleButton.TextSize = 14
        toggleButton.Parent = mainFrame
        
        local uicorner2 = Instance.new(\"UICorner\")
        uicorner2.CornerRadius = UDim.new(0, 6)
        uicorner2.Parent = toggleButton
        
        local toggleStroke = Instance.new(\"UIStroke\")
        toggleStroke.Thickness = 1
        toggleStroke.Color = Color3.fromRGB(60, 60, 80)
        toggleStroke.Transparency = 0.6
        toggleStroke.Parent = toggleButton
        
        local toggleHitboxButton = Instance.new(\"TextButton\")
        toggleHitboxButton.Size = UDim2.new(0.9, 0, 0, 35)
        toggleHitboxButton.Position = UDim2.new(0.05, 0, 0, 90)
        toggleHitboxButton.BackgroundColor3 = Color3.fromRGB(30, 30, 35)
        toggleHitboxButton.Text = \"NPC Hitbox: Default\"
        toggleHitboxButton.TextColor3 = Color3.fromRGB(180, 180, 200)
        toggleHitboxButton.Font = Enum.Font.GothamSemibold
        toggleHitboxButton.TextSize = 14
        toggleHitboxButton.Parent = mainFrame
        
        local uicorner3 = Instance.new(\"UICorner\")
        uicorner3.CornerRadius = UDim.new(0, 6)
        uicorner3.Parent = toggleHitboxButton
        
        local hitboxStroke = Instance.new(\"UIStroke\")
        hitboxStroke.Thickness = 1
        hitboxStroke.Color = Color3.fromRGB(60, 60, 80)
        hitboxStroke.Transparency = 0.6
        hitboxStroke.Parent = toggleHitboxButton
        
        local npcLockButton = Instance.new(\"TextButton\")
        npcLockButton.Size = UDim2.new(0.9, 0, 0, 35)
        npcLockButton.Position = UDim2.new(0.05, 0, 0, 130)
        npcLockButton.BackgroundColor3 = Color3.fromRGB(30, 30, 35)
        npcLockButton.Text = \"NPC Lock: OFF (X)\"
        npcLockButton.TextColor3 = Color3.fromRGB(180, 180, 200)
        npcLockButton.Font = Enum.Font.GothamSemibold
        npcLockButton.TextSize = 14
        npcLockButton.Parent = mainFrame
        
        local uicorner4 = Instance.new(\"UICorner\")
        uicorner4.CornerRadius = UDim.new(0, 6)
        uicorner4.Parent = npcLockButton
        
        local npcLockStroke = Instance.new(\"UIStroke\")
        npcLockStroke.Thickness = 1
        npcLockStroke.Color = Color3.fromRGB(60, 60, 80)
        npcLockStroke.Transparency = 0.6
        npcLockStroke.Parent = npcLockButton
        
        local separatorLine = Instance.new(\"Frame\")
        separatorLine.Size = UDim2.new(0.9, 0, 0, 1)
        separatorLine.Position = UDim2.new(0.05, 0, 0, 170)
        separatorLine.BackgroundColor3 = Color3.fromRGB(60, 60, 80)
        separatorLine.BorderSizePixel = 0
        separatorLine.Parent = mainFrame
        
        local discordButton = Instance.new(\"TextButton\")
        discordButton.Size = UDim2.new(0.9, 0, 0, 35)
        discordButton.Position = UDim2.new(0.05, 0, 0, 180)
        discordButton.BackgroundColor3 = Color3.fromRGB(30, 30, 35)
        discordButton.Text = \"Copy Discord Invite\"
        discordButton.TextColor3 = Color3.fromRGB(180, 180, 200)
        discordButton.Font = Enum.Font.GothamSemibold
        discordButton.TextSize = 14
        discordButton.Parent = mainFrame
        
        local uicorner5 = Instance.new(\"UICorner\")
        uicorner5.CornerRadius = UDim.new(0, 6)
        uicorner5.Parent = discordButton
        
        local discordStroke = Instance.new(\"UIStroke\")
        discordStroke.Thickness = 1
        discordStroke.Color = Color3.fromRGB(60, 60, 80)
        discordStroke.Transparency = 0.6
        discordStroke.Parent = discordButton
        
        local creditLabel = Instance.new(\"TextLabel\")
        creditLabel.Size = UDim2.new(0.9, 0, 0, 15)
        creditLabel.Position = UDim2.new(0.05, 0, 1, -20)
        creditLabel.BackgroundTransparency = 1
        creditLabel.Text = \"Made by hassanxzayn\"
        creditLabel.TextColor3 = Color3.fromRGB(100, 100, 120)
        creditLabel.Font = Enum.Font.Gotham
        creditLabel.TextSize = 10
        creditLabel.Parent = mainFrame
        
        mainFrame.InputBegan:Connect(function(input)
            if input.UserInputType == Enum.UserInputType.Touch then
                UserInputService.MouseBehavior = Enum.MouseBehavior.Default
            end
        end)
        mainFrame.InputEnded:Connect(function(input)
            if input.UserInputType == Enum.UserInputType.Touch then
                UserInputService.MouseBehavior = Enum.MouseBehavior.LockCenter
            end
        end)
        
        local minimized = false
        minimizeButton.MouseButton1Click:Connect(function()
            minimized = not minimized
            if minimized then
                mainFrame.Size = UDim2.new(0, 200, 0, 35)
                minimizeButton.Text = \"+\"
            else
                mainFrame.Size = UDim2.new(0, 200, 0, 240) 
                minimizeButton.Text = \"-\"
            end
            toggleButton.Visible = not minimized
            toggleHitboxButton.Visible = not minimized
            npcLockButton.Visible = not minimized
            separatorLine.Visible = not minimized
            discordButton.Visible = not minimized
            creditLabel.Visible = not minimized
        end)
        
        discordButton.MouseButton1Click:Connect(function()
            setclipboard(\"https://discord.gg/99UuEwM9sX\")
            discordButton.Text = \"Copied!\"
            task.wait(1)
            discordButton.Text = \"Copy Discord Invite\"
        end)
        
        local function createESP(target)
            if target and target.Parent then
                if targetESP then targetESP:Destroy() end
                targetESP = Instance.new(\"Highlight\")
                targetESP.FillTransparency = 1
                targetESP.OutlineColor = Color3.fromRGB(255, 0, 0)
                targetESP.OutlineTransparency = 0
                targetESP.Parent = target.Parent
            end
        end
        
        local function removeESP()
            if targetESP then
                targetESP:Destroy()
                targetESP = nil
            end
        end
        
        local function hasClearLineOfSight(targetHead)
            local origin = cam.CFrame.Position
            local direction = (targetHead.Position - origin).unit * (targetHead.Position - origin).Magnitude
            local raycastParams = RaycastParams.new()
            raycastParams.FilterType = Enum.RaycastFilterType.Blacklist
            raycastParams.FilterDescendantsInstances = {player.Character}
            local result = Workspace:Raycast(origin, direction, raycastParams)
            return result == nil or result.Instance:IsDescendantOf(targetHead.Parent)
        end
        
        local function getClosestNPCTarget()
            local character = player.Character
            if not character or not character:FindFirstChild(\"HumanoidRootPart\") then
                return nil
            end
            local closestNPCHead = nil
            local shortestDistance = math.huge
            for npc, _ in pairs(trackedNPCs) do
                if npc and npc.Parent then
                    local humanoid = npc:FindFirstChild(\"Humanoid\")
                    local head = npc:FindFirstChild(\"Head\")
                    if humanoid and head then
                        local distance = (head.Position - character.HumanoidRootPart.Position).Magnitude
                        if distance <= 330 
                           and humanoid.Health > 0 
                           and humanoid:GetState() ~= Enum.HumanoidStateType.Dead 
                           and distance < shortestDistance 
                           and hasClearLineOfSight(head) then
                            shortestDistance = distance
                            closestNPCHead = head
                        end
                    end
                else
                    trackedNPCs[npc] = nil
                end
            end
            return closestNPCHead
        end
        
        local targetHead = nil
        local lastTargetUpdate = 0
        local targetUpdateInterval = 0.1
        
        local function startCamlock()
            camlockConnection = RunService.RenderStepped:Connect(function()
                local currentTime = tick()
                if currentTime - lastTargetUpdate >= targetUpdateInterval then
                    targetHead = getClosestNPCTarget()
                    lastTargetUpdate = currentTime
                end
                if targetHead then
                    local distance = (targetHead.Position - cam.CFrame.Position).Magnitude
                    if distance > 300 then
                        removeESP()
                    else
                        local heightCompensation = math.clamp(distance * 0, 0.2, 1.8)
                        local headPos = targetHead.Position + Vector3.new(0, targetHead.Size.Y / 2 - heightCompensation, 0)
                        local camPos = cam.CFrame.Position
                        local direction = (headPos - camPos).unit
                        local newCF = CFrame.lookAt(camPos, camPos + direction)
                        cam.CFrame = newCF
                        createESP(targetHead)
                    end
                else
                    removeESP()
                end
            end)
        end
        
        local function stopCamlock()
            if camlockConnection then
                camlockConnection:Disconnect()
                camlockConnection = nil
            end
            removeESP()
        end
        
        toggleButton.MouseButton1Click:Connect(function()
            camlockEnabled = not camlockEnabled
            if camlockEnabled then
                toggleButton.Text = \"NPC Camlock: ON\"
                toggleButton.BackgroundColor3 = Color3.fromRGB(40, 40, 60)
                toggleButton.TextColor3 = Color3.fromRGB(200, 200, 255)
                startCamlock()
            else
                toggleButton.Text = \"NPC Camlock: OFF\"
                toggleButton.BackgroundColor3 = Color3.fromRGB(30, 30, 35)
                toggleButton.TextColor3 = Color3.fromRGB(180, 180, 200)
                stopCamlock()
            end
        end)
        
        local function addPlayerHighlight()
            if player.Character then
                local highlight = player.Character:FindFirstChild(\"PlayerHighlightESP\")
                if not highlight then
                    highlight = Instance.new(\"Highlight\")
                    highlight.Name = \"PlayerHighlightESP\"
                    highlight.FillColor = Color3.new(1, 1, 1)
                    highlight.OutlineColor = Color3.new(1, 1, 1)
                    highlight.FillTransparency = 0.5
                    highlight.OutlineTransparency = 0
                    highlight.Parent = player.Character
                end
            end
        end
        
        local function removePlayerHighlight()
            if player.Character and player.Character:FindFirstChild(\"PlayerHighlightESP\") then
                player.Character.PlayerHighlightESP:Destroy()
            end
        end
        
        local function getClosestNPC()
            local closestNPC = nil
            local closestDistance = math.huge
            for _, object in ipairs(Workspace:GetDescendants()) do
                if object:IsA(\"Model\") then
                    local humanoid = object:FindFirstChild(\"Humanoid\") or object:FindFirstChildWhichIsA(\"Humanoid\")
                    local hrp = object:FindFirstChild(\"HumanoidRootPart\") or object.PrimaryPart
                    if humanoid and hrp and humanoid.Health > 0 and object.Name ~= \"Horse\" then
                        local isPlayer = false
                        for _, pl in ipairs(Players:GetPlayers()) do
                            if pl.Character == object then
                                isPlayer = true
                                break
                            end
                        end
                        if not isPlayer then
                            local distance = (hrp.Position - player.Character.HumanoidRootPart.Position).Magnitude
                            if distance < closestDistance then
                                closestDistance = distance
                                closestNPC = object
                            end
                        end
                    end
                end
            end
            return closestNPC
        end
        
        local function toggleNPCLock()
            npcLock = not npcLock
            if npcLock then
                npcLockButton.Text = \"NPC Lock: ON (X)\"
                npcLockButton.BackgroundColor3 = Color3.fromRGB(40, 40, 60)
                npcLockButton.TextColor3 = Color3.fromRGB(200, 200, 255)
                toggleLoop = RunService.RenderStepped:Connect(function()
                    local npc = getClosestNPC()
                    if npc and npc:FindFirstChild(\"Humanoid\") then
                        local npcHumanoid = npc:FindFirstChild(\"Humanoid\")
                        if npcHumanoid.Health > 0 then
                            cam.CameraSubject = npcHumanoid
                            lastTarget = npc
                            addPlayerHighlight()
                        else
                            StarterGui:SetCore(\"SendNotification\", {
                                Title = \"Killed NPC\",
                                Text = npc.Name,
                                Duration = 0.4
                            })
                            lastTarget = nil
                            removePlayerHighlight()
                            if player.Character and player.Character:FindFirstChild(\"Humanoid\") then
                                cam.CameraSubject = player.Character:FindFirstChild(\"Humanoid\")
                            end
                        end
                    else
                        if player.Character and player.Character:FindFirstChild(\"Humanoid\") then
                            cam.CameraSubject = player.Character:FindFirstChild(\"Humanoid\")
                        end
                        lastTarget = nil
                        removePlayerHighlight()
                    end
                end)
            else
                npcLockButton.Text = \"NPC Lock: OFF (X)\"
                npcLockButton.BackgroundColor3 = Color3.fromRGB(30, 30, 35)
                npcLockButton.TextColor3 = Color3.fromRGB(180, 180, 200)
                if toggleLoop then
                    toggleLoop:Disconnect()
                    toggleLoop = nil
                end
                removePlayerHighlight()
                if player.Character and player.Character:FindFirstChild(\"Humanoid\") then
                    cam.CameraSubject = player.Character:FindFirstChild(\"Humanoid\")
                end
            end
        end
        
        npcLockButton.MouseButton1Click:Connect(toggleNPCLock)
        UserInputService.InputBegan:Connect(function(input, gameProcessed)
            if not gameProcessed and input.KeyCode == Enum.KeyCode.X then
                toggleNPCLock()
            end
        end)
        
        local function updateNPCHitbox(npc)
            if npc:IsA(\"Model\") and npc:FindFirstChild(\"Humanoid\") and npc:FindFirstChild(\"HumanoidRootPart\") then
                if Players:GetPlayerFromCharacter(npc) or npc.Name == \"Horse\" then return end
                local humanoid = npc.Humanoid
                local hrp = npc.HumanoidRootPart
                if defaultHRPValues[hrp] == nil then
                    defaultHRPValues[hrp] = {
                        Size = hrp.Size,
                        Transparency = hrp.Transparency,
                        CanCollide = hrp.CanCollide
                    }
                end
                if humanoid.Health <= 0 then
                    local defaults = defaultHRPValues[hrp]
                    if defaults then
                        hrp.Size = defaults.Size
                        hrp.Transparency = defaults.Transparency
                        hrp.CanCollide = defaults.CanCollide
                    end
                    return
                end
                if hitboxToggleEnabled then
                    hrp.Size = Vector3.new(10, 10, 10)
                    hrp.Transparency = 0.85
                    hrp.CanCollide = false
                else
                    local defaults = defaultHRPValues[hrp]
                    if defaults then
                        hrp.Size = defaults.Size
                        hrp.Transparency = defaults.Transparency
                        hrp.CanCollide = defaults.CanCollide
                    end
                end
            end
        end
        
        local function updateAllNPCsHitbox()
            for _, obj in ipairs(Workspace:GetDescendants()) do
                if obj:IsA(\"Model\") 
                   and obj:FindFirstChild(\"HumanoidRootPart\") 
                   and obj:FindFirstChild(\"Humanoid\") 
                   and not Players:GetPlayerFromCharacter(obj) then
                    updateNPCHitbox(obj)
                end
            end
        end
        
        toggleHitboxButton.MouseButton1Click:Connect(function()
            hitboxToggleEnabled = not hitboxToggleEnabled
            if hitboxToggleEnabled then
                toggleHitboxButton.Text = \"NPC Hitbox: ON\"
                toggleHitboxButton.BackgroundColor3 = Color3.fromRGB(40, 40, 60)
                toggleHitboxButton.TextColor3 = Color3.fromRGB(200, 200, 255)
            else
                toggleHitboxButton.Text = \"NPC Hitbox: Default\"
                toggleHitboxButton.BackgroundColor3 = Color3.fromRGB(30, 30, 35)
                toggleHitboxButton.TextColor3 = Color3.fromRGB(180, 180, 200)
            end
            updateAllNPCsHitbox()
        end)
        
        Workspace.DescendantAdded:Connect(function(obj)
            if obj:IsA(\"Model\") then
                local humanoid = obj:FindFirstChild(\"Humanoid\") or obj:WaitForChild(\"Humanoid\", 5)
                if humanoid then
                    local hrp = obj:FindFirstChild(\"HumanoidRootPart\") or obj:WaitForChild(\"HumanoidRootPart\", 5)
                    if hrp and not Players:GetPlayerFromCharacter(obj) then
                        if defaultHRPValues[hrp] == nil then
                            defaultHRPValues[hrp] = {
                                Size = hrp.Size,
                                Transparency = hrp.Transparency,
                                CanCollide = hrp.CanCollide
                            }
                        end
                        humanoid.Died:Connect(function()
                            task.wait(0.2)
                            local defaults = defaultHRPValues[hrp]
                            if defaults then
                                hrp.Size = defaults.Size
                                hrp.Transparency = defaults.Transparency
                                hrp.CanCollide = defaults.CanCollide
                            end
                        end)
                        task.wait(0.5)
                        updateNPCHitbox(obj)
                    end
                end
            end
        end)
        
        local function addNPC(npc)
            if npc:IsA(\"Model\") 
               and npc:FindFirstChild(\"Humanoid\") 
               and npc:FindFirstChild(\"HumanoidRootPart\") 
               and not Players:GetPlayerFromCharacter(npc)
               and npc.Name ~= \"Horse\" then
                trackedNPCs[npc] = true
            end
        end
        
        for _, obj in ipairs(Workspace:GetDescendants()) do
            if obj:IsA(\"Model\") 
               and obj:FindFirstChild(\"Humanoid\") 
               and obj:FindFirstChild(\"HumanoidRootPart\") 
               and not Players:GetPlayerFromCharacter(obj)
               and npc.Name ~= \"Horse\" then
                addNPC(obj)
            end
        end
        
        Workspace.DescendantAdded:Connect(function(obj)
            if obj:IsA(\"Model\") then
                task.wait(0.5)
                addNPC(obj)
            end
        end)
        
        Workspace.DescendantRemoving:Connect(function(obj)
            if trackedNPCs[obj] then
                trackedNPCs[obj] = nil
            end
        end)
        
        spawn(function()
            while task.wait(1) do
                for npc, _ in pairs(trackedNPCs) do
                    if npc and npc.Parent then
                        local humanoid = npc:FindFirstChild(\"Humanoid\")
                        local hrp = npc:FindFirstChild(\"HumanoidRootPart\")
                        if humanoid and hrp and humanoid.Health <= 0 then
                            local defaults = defaultHRPValues[hrp]
                            if defaults then
                                hrp.Size = defaults.Size
                                hrp.Transparency = defaults.Transparency
                                hrp.CanCollide = defaults.CanCollide
                            end
                        end
                    else
                        trackedNPCs[npc] = nil
                    end
                end
            end
        end)
    end
})

local ESPHandles = {}
local ESPEnabled = false
local Players = game:GetService(\"Players\")
local RunService = game:GetService(\"RunService\")
local TweenService = game:GetService(\"TweenService\")
local LocalPlayer = Players.LocalPlayer

local ESPConfig = {
    Colors = {
        Items = {Color3.fromRGB(255, 105, 105), Color3.fromRGB(255, 180, 180)},
        Animals = {Color3.fromRGB(170, 85, 255), Color3.fromRGB(220, 160, 255)},
        NightEnemies = {Color3.fromRGB(85, 170, 255), Color3.fromRGB(150, 220, 255)},
        Zombies = {Color3.fromRGB(85, 255, 85), Color3.fromRGB(180, 255, 180)}
    },
    Settings = {
        OutlineTransparency = 0,
        FillTransparency = 0.4, 
        TextSize = 12,
        BillboardSize = UDim2.new(0, 120, 0, 20),
        MaxDistance = 500,
        PulseSpeed = 1, 
        PulseRange = 0.3, 
        UpdateInterval = 0.5,
        StackOffsets = {1.0, 3.0, 5.0}, 
        ProximityThreshold = 2
    }
}

local function CreateESP(object, colorPair, category)
    if not object or not object.PrimaryPart then return end

    local highlight = Instance.new(\"Highlight\")
    highlight.Name = \"ESP_ProHighlight\"
    highlight.Adornee = object
    highlight.FillColor = colorPair[1]
    highlight.OutlineColor = colorPair[2]
    highlight.FillTransparency = ESPConfig.Settings.FillTransparency
    highlight.OutlineTransparency = ESPConfig.Settings.OutlineTransparency
    highlight.Parent = object

    local billboard = Instance.new(\"BillboardGui\")
    billboard.Name = \"ESP_ProBillboard\"
    billboard.Adornee = object.PrimaryPart
    billboard.Size = ESPConfig.Settings.BillboardSize
    billboard.StudsOffset = Vector3.new(0, 3, 0)
    billboard.AlwaysOnTop = true
    billboard.LightInfluence = 0
    billboard.Parent = object

    local textLabel = Instance.new(\"TextLabel\")
    textLabel.Text = object.Name .. \" [0]\" 
    textLabel.Size = UDim2.new(1, 0, 1, 0)
    textLabel.TextColor3 = colorPair[1]
    textLabel.BackgroundTransparency = 1
    textLabel.TextSize = ESPConfig.Settings.TextSize
    textLabel.Font = Enum.Font.GothamBlack
    textLabel.TextStrokeTransparency = 0.2
    textLabel.TextStrokeColor3 = Color3.fromRGB(0, 0, 0)
    textLabel.Parent = billboard

    local tweenInfo = TweenInfo.new(
        ESPConfig.Settings.PulseSpeed,
        Enum.EasingStyle.Sine,
        Enum.EasingDirection.InOut,
        -1,
        true
    )
    local pulseTween = TweenService:Create(highlight, tweenInfo, {
        FillTransparency = ESPConfig.Settings.FillTransparency - ESPConfig.Settings.PulseRange
    })
    pulseTween:Play()

    ESPHandles[object] = {
        Highlight = highlight,
        Billboard = billboard,
        TextLabel = textLabel,
        PulseTween = pulseTween
    }
end

local function ClearESP()
    for obj, handles in pairs(ESPHandles) do
        if handles.PulseTween then handles.PulseTween:Cancel() end
        if handles.Highlight then handles.Highlight:Destroy() end
        if handles.Billboard then handles.Billboard:Destroy() end
    end
    ESPHandles = {}
end

local function UpdateESP()
    for obj, handles in pairs(ESPHandles) do
        if not obj or not obj.Parent or not obj.PrimaryPart then
            if handles.PulseTween then handles.PulseTween:Cancel() end
            for _, instance in pairs(handles) do
                if instance:IsA(\"Instance\") then instance:Destroy() end
            end
            ESPHandles[obj] = nil
        end
    end

    local function ProcessCategory(parent, colorPair, category)
        if parent then
            for _, obj in ipairs(parent:GetDescendants()) do
                if obj:IsA(\"Model\") and obj.PrimaryPart and not ESPHandles[obj] then
                    CreateESP(obj, colorPair, category)
                end
            end
        end
    end

    ProcessCategory(workspace:FindFirstChild(\"RuntimeItems\"), ESPConfig.Colors.Items, \"Items\")
    
    local baseplates = workspace:FindFirstChild(\"Baseplates\")
    if baseplates and #baseplates:GetChildren() >= 2 then
        local animals = baseplates:GetChildren()[2]:FindFirstChild(\"CenterBaseplate\"):FindFirstChild(\"Animals\")
        ProcessCategory(animals, ESPConfig.Colors.Animals, \"Animals\")
    end

    ProcessCategory(workspace:FindFirstChild(\"NightEnemies\"), ESPConfig.Colors.NightEnemies, \"NightEnemies\")

    local destroyedHouse = workspace:FindFirstChild(\"RandomBuildings\") and workspace.RandomBuildings:FindFirstChild(\"DestroyedHouse\")
    local zombies = destroyedHouse and destroyedHouse:FindFirstChild(\"StandaloneZombiePart\"):FindFirstChild(\"Zombies\")
    ProcessCategory(zombies, ESPConfig.Colors.Zombies, \"Zombies\")
end

local function UpdateDistanceAndStacking()
    local character = LocalPlayer.Character
    local rootPart = character and character:FindFirstChild(\"HumanoidRootPart\")
    
    if not rootPart then 
        for _, handles in pairs(ESPHandles) do
            if handles.TextLabel then handles.TextLabel.Text = handles.TextLabel.Text:match(\"^.+%s\") .. \"[N/A]\" end
            handles.Billboard.StudsOffset = Vector3.new(0, 3, 0) 
        end
        return 
    end
    
    local playerPos = rootPart.Position
    
    local objects = {}
    for obj, handles in pairs(ESPHandles) do
        if not obj or not obj.Parent or not obj.PrimaryPart then
            ESPHandles[obj] = nil
        else
            local targetPos = obj.PrimaryPart.Position
            local distance = (playerPos - targetPos).Magnitude
            handles.TextLabel.Text = string.format(\"%s [%d]\", obj.Name, math.floor(distance))
            handles.Billboard.Enabled = (distance <= ESPConfig.Settings.MaxDistance)
            
            if handles.Billboard.Enabled then
                table.insert(objects, {Obj = obj, Pos = targetPos, Handles = handles})
            end
        end
    end

    local processed = {}
    for _, entry in ipairs(objects) do
        local obj = entry.Obj
        if not processed[obj] then
            local nearby = {entry}
            processed[obj] = true
            
            for _, otherEntry in ipairs(objects) do
                local otherObj = otherEntry.Obj
                if otherObj ~= obj and not processed[otherObj] then
                    local dist = (entry.Pos - otherEntry.Pos).Magnitude
                    if dist < ESPConfig.Settings.ProximityThreshold then
                        table.insert(nearby, otherEntry)
                        processed[otherObj] = true
                    end
                end
            end
            
            for i, item in ipairs(nearby) do
                local offsetIndex = (i - 1) % 3 + 1
                local offsetY = ESPConfig.Settings.StackOffsets[offsetIndex]
                item.Handles.Billboard.StudsOffset = Vector3.new(0, offsetY, 0)
            end
        end
    end
end

local function AutoUpdateESP()
    local distanceConnection
    local updateConnection

    distanceConnection = RunService.RenderStepped:Connect(function()
        if not ESPEnabled then
            distanceConnection:Disconnect()
            if updateConnection then updateConnection:Disconnect() end
            ClearESP()
            return
        end
        UpdateDistanceAndStacking()
    end)

    spawn(function()
        while ESPEnabled do
            UpdateESP()
            task.wait(ESPConfig.Settings.UpdateInterval)
        end
    end)
end

local Toggle = Tabs.Esp:AddToggle(\"ESP_Toggle\", {
    Title = \"All In One ESP\",
    Description = \"Vivid highlights, compact tags\",
    Default = false,
    Callback = function(Value)
        ESPEnabled = Value
        if Value then
            AutoUpdateESP()
        else
            ClearESP()
        end
    end
})

local Players = game:GetService(\"Players\")
local RunService = game:GetService(\"RunService\")

local espEnabled = false

local function hideDefaultNameTag(character)
    local humanoid = character:FindFirstChildOfClass(\"Humanoid\")
    if humanoid then
        humanoid.DisplayDistanceType = Enum.HumanoidDisplayDistanceType.None 
    end
end

local function createHealthDisplay(player)
    if not player.Character then return end
    local character = player.Character
    local head = character:FindFirstChild(\"Head\")
    if not head then return end

    local humanoid = character:FindFirstChildOfClass(\"Humanoid\")
    if not humanoid then return end

    hideDefaultNameTag(character)

    local billboardGui = Instance.new(\"BillboardGui\")
    billboardGui.Size = UDim2.new(4, 0, 2, 0)
    billboardGui.Adornee = head
    billboardGui.StudsOffset = Vector3.new(0, 2.5, 0)
    billboardGui.MaxDistance = math.huge
    billboardGui.Parent = head
    billboardGui.AlwaysOnTop = true

    local nameLabel = Instance.new(\"TextLabel\")
    nameLabel.Size = UDim2.new(1, 0, 0.3, 0)
    nameLabel.Position = UDim2.new(0, 0, 0, 0)
    nameLabel.BackgroundTransparency = 1
    nameLabel.TextScaled = true
    nameLabel.Font = Enum.Font.GothamBold
    nameLabel.TextColor3 = Color3.fromRGB(255, 255, 0)
    nameLabel.TextStrokeTransparency = 0.2
    nameLabel.TextStrokeColor3 = Color3.new(0, 0, 0)
    nameLabel.Text = player.Name
    nameLabel.Parent = billboardGui

    local leaderstatsLabel = Instance.new(\"TextLabel\")
    leaderstatsLabel.Size = UDim2.new(1, 0, 0.3, 0)
    leaderstatsLabel.Position = UDim2.new(0, 0, 0.35, 0)
    leaderstatsLabel.BackgroundTransparency = 1
    leaderstatsLabel.TextScaled = true
    leaderstatsLabel.Font = Enum.Font.GothamBold
    leaderstatsLabel.TextColor3 = Color3.fromRGB(0, 255, 255)
    leaderstatsLabel.TextStrokeTransparency = 0.2
    leaderstatsLabel.TextStrokeColor3 = Color3.new(0, 0, 0)
    leaderstatsLabel.Text = \"\" 
    leaderstatsLabel.Parent = billboardGui

    local healthText = Instance.new(\"TextLabel\")
    healthText.Size = UDim2.new(1, 0, 0.3, 0)
    healthText.Position = UDim2.new(0, 0, 0.7, 0)
    healthText.BackgroundTransparency = 1
    healthText.TextScaled = true
    healthText.Font = Enum.Font.GothamBold
    healthText.TextColor3 = Color3.fromRGB(255, 255, 255)
    healthText.TextStrokeTransparency = 0.2
    healthText.TextStrokeColor3 = Color3.new(0, 0, 0)
    healthText.Parent = billboardGui

    local function updateDisplay()
        if humanoid and humanoid.Health > 0 then
            healthText.Text = tostring(math.floor(humanoid.Health)) .. \" HP\"
        else
            billboardGui:Destroy()
            return
        end

        local leaderstats = player:FindFirstChild(\"leaderstats\")
        if leaderstats then
            local statsText = \"\"
            for _, stat in pairs(leaderstats:GetChildren()) do
                if stat:IsA(\"IntValue\") or stat:IsA(\"StringValue\") or stat:IsA(\"NumberValue\") then
                    statsText = statsText .. stat.Name .. \": \" .. tostring(stat.Value) .. \"  \"
                end
            end
            leaderstatsLabel.Text = statsText
        else
            leaderstatsLabel.Text = \"\"
        end
    end

    local connection
    connection = RunService.Heartbeat:Connect(function()
        if not espEnabled or not player or not player.Parent or not character.Parent then
            billboardGui:Destroy()
            connection:Disconnect()
            return
        end
        updateDisplay()
    end)

    updateDisplay()
end

local function onPlayerAdded(player)
    local function onCharacterAdded(character)
        task.wait(0.5) 
        if espEnabled then
            hideDefaultNameTag(character)
            createHealthDisplay(player)
        end
    end

    player.CharacterAdded:Connect(onCharacterAdded)
    if player.Character then
        onCharacterAdded(player.Character)
    end
end

local function onPlayerRemoving(player)
    if player.Character then
        local head = player.Character:FindFirstChild(\"Head\")
        if head then
            local billboardGui = head:FindFirstChildOfClass(\"BillboardGui\")
            if billboardGui then
                billboardGui:Destroy()
            end
        end
    end
end

local Toggle = Tabs.Esp:AddToggle(\"ESP_Toggle\", {
    Title = \"Player ESP\",
    Description = \"Show Player Health, Name, and Money\",
    Default = false,
    Callback = function(state)
        espEnabled = state
        if state then
            for _, player in ipairs(Players:GetPlayers()) do
                if player ~= Players.LocalPlayer then 
                    if player.Character then
                        createHealthDisplay(player)
                    end
                end
            end
        else
            for _, player in ipairs(Players:GetPlayers()) do
                if player.Character then
                    local head = player.Character:FindFirstChild(\"Head\")
                    if head then
                        local billboardGui = head:FindFirstChildOfClass(\"BillboardGui\")
                        if billboardGui then
                            billboardGui:Destroy()
                        end
                    end
                end
            end
        end
    end
})

for _, player in ipairs(Players:GetPlayers()) do
    onPlayerAdded(player)
end

Players.PlayerAdded:Connect(onPlayerAdded)
Players.PlayerRemoving:Connect(onPlayerRemoving)

local RunService = game:GetService(\"RunService\")
local Lighting = game:GetService(\"Lighting\")

local DayOnlyLoop
local NightOnlyLoop

local DayToggle = Tabs.Weather:AddToggle(\"OnlyDay_Toggle\", {
    Title = \"Only Day\",
    Description = \"\",
    Default = false,
    Callback = function(Value)
        if Value then
            DayOnlyLoop = RunService.Heartbeat:Connect(function()
                Lighting.TimeOfDay = \"12:00:00\"
            end)
        else
            if DayOnlyLoop then
                DayOnlyLoop:Disconnect()
                DayOnlyLoop = nil
            end
        end
    end
})

local NightToggle = Tabs.Weather:AddToggle(\"OnlyNight_Toggle\", {
    Title = \"Only Night\",
    Description = \"\",
    Default = false,
    Callback = function(Value)
        if Value then
            NightOnlyLoop = RunService.Heartbeat:Connect(function()
                Lighting.TimeOfDay = \"22:00:00\"
            end)
        else
            if NightOnlyLoop then
                NightOnlyLoop:Disconnect()
                NightOnlyLoop = nil
            end
        end
    end
})

local FogToggle = Tabs.Weather:AddToggle(\"RemoveFog_Toggle\", {
    Title = \"Remove Fog\",
    Description = \"\",
    Default = false,
    Callback = function(Value)
        if Value then
            local sky = Lighting:FindFirstChild(\"GrayCloudSky\")
            if sky then
                sky.Parent = Lighting.Bloom
            end
        else
            local skyInBloom = Lighting.Bloom:FindFirstChild(\"GrayCloudSky\")
            if skyInBloom then
                skyInBloom.Parent = Lighting
            end
        end
    end
})


Tabs.Misc:AddButton({
    Title = \"Anti AFK\",
    Description = \"\",
    Callback = function()
        loadstring(game:HttpGet(\"https://raw.githubusercontent.com/hassanxzayn-lua/Anti-afk/main/antiafkbyhassanxzyn\"))();
    end
})

Tabs.Misc:AddButton({
    Title = \"Infiniteyield Reborn\",
    Description = \"\",
    Callback = function()
        loadstring(game:HttpGet(\"https://raw.githubusercontent.com/RyXeleron/infiniteyield-reborn/refs/heads/scriptblox/source\"))()
    end
})


local Section = Tabs.Misc:AddSection(\"Character\")



local TeleportToggle = Tabs.Misc:AddToggle(\"TeleportToggle\", 
{
    Title = \"T + Left Click Teleport\", 
    Description = \"\",
    Default = false,
    Callback = function(Value)
        if not hasUserToggled and not isTeleportEnabled then
            hasUserToggled = true
            isTeleportEnabled = Value
            return
        end

        isTeleportEnabled = Value
        if Value then
            if not connection then
                connection = game:GetService(\"UserInputService\").InputBegan:Connect(function(input, gameProcessed)
                    if not gameProcessed and isTeleportEnabled and input.UserInputType == Enum.UserInputType.MouseButton1 then
                        if game:GetService(\"UserInputService\"):IsKeyDown(Enum.KeyCode.T) then
                            local player = game:GetService(\"Players\").LocalPlayer
                            local mouse = player:GetMouse()
                            player.Character:MoveTo(Vector3.new(mouse.Hit.x, mouse.Hit.y, mouse.Hit.z))
                        end
                    end
                end)
            end
        else
            if connection then
                connection:Disconnect()
                connection = nil
            end
        end
    end
})


local InfiniteJumpToggle = Tabs.Misc:AddToggle(\"InfiniteJumpToggle\", 
{
    Title = \"Infinite Jumps\", 
    Description = \"\",
    Default = false,
    Callback = function(Value)
        _G.infinjump = Value
    end
})

if not _G.infinJumpStarted then
    _G.infinJumpStarted = true
    _G.infinjump = false

    local plr = game:GetService('Players').LocalPlayer
    local m = plr:GetMouse()

    m.KeyDown:Connect(function(key)
        if _G.infinjump then
            if key:byte() == 32 then
                local humanoid = plr.Character and plr.Character:FindFirstChildOfClass('Humanoid')
                if humanoid then
                    humanoid:ChangeState('Jumping')
                    wait()
                    humanoid:ChangeState('Seated')
                end
            end
        end
    end)
end



local Noclip = nil
local Clip = true

function noclip()
	Clip = false
	local function Nocl()
		if Clip == false and game.Players.LocalPlayer.Character ~= nil then
			for _, v in pairs(game.Players.LocalPlayer.Character:GetDescendants()) do
				if v:IsA('BasePart') and v.CanCollide and v.Name ~= floatName then
					v.CanCollide = false
				end
			end
		end
		wait(0.21)
	end
	Noclip = game:GetService('RunService').Stepped:Connect(Nocl)
end

function clip()
	if Noclip then 
		Noclip:Disconnect()
	end
	Clip = true
	if game.Players.LocalPlayer.Character then
		for _, v in pairs(game.Players.LocalPlayer.Character:GetDescendants()) do
			if v:IsA('BasePart') and v.CanCollide == false then
				v.CanCollide = true
			end
		end
	end
end

local Toggle = Tabs.Misc:AddToggle(\"MyToggle\", {
    Title = \"Noclip\",
    Description = \"\",
    Default = false,
    Callback = function(state)
        if state then
            noclip()
        else
            clip()
        end
    end
})




local Slider = Tabs.Misc:AddSlider(\"SpeedSlider\", 
{
    Title = \"WalkSpeed\",
    Description = \"\",
    Default = 16,
    Min = 16,
    Max = 500,
    Rounding = 1,
    Callback = function(Value)
        game.Players.LocalPlayer.Character.Humanoid.WalkSpeed = (Value)
    end
})

local Slider = Tabs.Misc:AddSlider(\"JumpSlider\", 
{
    Title = \"Jump Power\",
    Description = \"\",
    Default = 50,
    Min = 50,
    Max = 500,
    Rounding = 1,
    Callback = function(Value)
        game.Players.LocalPlayer.Character.Humanoid.JumpPower = (Value)
    end
})



local Input = Tabs.Misc:AddInput(\"FOVInput\", 
{
    Title = \"Change FOV\",
    Description = \"\",
    Default = \"70\",
    Placeholder = \"Enter FOV value\",
    Numeric = true,
    Finished = true,
    Callback = function(Value)
        local fov = tonumber(Value)
        if fov then
            game.Workspace.Camera.FieldOfView = fov
            print(\"FOV set to:\", fov)
        else
            warn(\"Invalid fpv valuee\")
        end
    end
})

Tabs.Performance:AddButton({
    Title = \"Enable FPS Counter & Boost\",
    Description = \"\",
    Callback = function()
        if not gethui then
            warn(\"incompatible executor: unavailable\")
            return
        end

        local runService = game:GetService(\"RunService\")
        local lighting = game:GetService(\"Lighting\")

        local fpsCounterGui = Instance.new(\"ScreenGui\", gethui())
        local fpsLabel = Instance.new(\"TextLabel\", fpsCounterGui)

        fpsCounterGui.Name = \"FpsCounterGui\"
        fpsCounterGui.IgnoreGuiInset = true
        fpsLabel.Name = \"FpsLabel\"
        fpsLabel.BackgroundTransparency = 1
        fpsLabel.Position = UDim2.new(1, -100, 0, 0)
        fpsLabel.Size = UDim2.new(0, 100, 0, 30)
        fpsLabel.Text = \"\"
        fpsLabel.TextSize = 16
        fpsLabel.TextStrokeTransparency = 0.6
        fpsLabel.Draggable = true

        local function GetFPS(delay)
            local startTime = tick()
            local frames = 0
            local heartbeatConnection = runService.Heartbeat:Connect(function()
                frames = frames + 1
            end)
            task.wait(delay)
            heartbeatConnection:Disconnect()
            local elapsedTime = tick() - startTime
            local fps = frames / elapsedTime
            return math.ceil(fps)
        end

        task.spawn(function()
            while true do
                local fps = GetFPS(0.5)
                if fps >= 60 then
                    fpsLabel.TextColor3 = Color3.fromRGB(0, 255, 0)
                elseif fps <= 59 and fps > 50 then
                    fpsLabel.TextColor3 = Color3.fromRGB(255, 170, 0)
                elseif fps <= 49 and fps > 30 then
                    fpsLabel.TextColor3 = Color3.fromRGB(255, 85, 0)
                elseif fps <= 29 then
                    fpsLabel.TextColor3 = Color3.fromRGB(255, 0, 0)
                end
                fpsLabel.Text = tostring(fps) .. \" FPS\"
            end
        end)

        lighting.GlobalShadows = false
        lighting.FogEnd = 9e9
        lighting.EnvironmentDiffuseScale = 0.5
        lighting.EnvironmentSpecularScale = 0.5

        for _, descendant in ipairs(game:GetDescendants()) do
            if descendant:IsA(\"BasePart\") then
                descendant.CastShadow = false
                descendant.Material = Enum.Material.SmoothPlastic
                descendant.Reflectance = 0
                if descendant:IsA(\"MeshPart\") then
                    descendant.CollisionFidelity = Enum.CollisionFidelity.Box
                end
            end
            if descendant:IsA(\"Decal\") or descendant:IsA(\"Texture\") then
                if descendant.Transparency > 0.25 then
                    descendant.Transparency = 0.25
                end
            end
            if descendant:IsA(\"ParticleEmitter\") or descendant:IsA(\"Trail\") then
                descendant.Lifetime = NumberRange.new(0)
            end
        end
    end
})

local VirtualInputManager = game:GetService(\"VirtualInputManager\")
local isHoldingW = false

Tabs.Train:AddToggle(\"AutoRunTrain\", {
    Title = \"Auto Run Train\",
    Description = \"\",
    Default = false,
    Callback = function(state)
        isHoldingW = state
        if isHoldingW then
            task.spawn(function()
                while isHoldingW do
                    VirtualInputManager:SendKeyEvent(true, Enum.KeyCode.W, false, game)
                    task.wait(0.1)
                end
            end)
        else
            VirtualInputManager:SendKeyEvent(false, Enum.KeyCode.W, false, game)
        end
    end
})

getgenv().esptrain = false 
local runService = game:GetService(\"RunService\")
local trainFolder = workspace:FindFirstChild(\"Train\")

local function addTrainESP(train)
    if not train:FindFirstChild(\"ESP_Highlight\") then
        pcall(function()
            local highlight = Instance.new(\"Highlight\")
            highlight.Name = \"ESP_Highlight\"
            highlight.FillColor = Color3.fromRGB(255, 255, 255)
            highlight.OutlineColor = Color3.fromRGB(255, 255, 255)
            highlight.FillTransparency = 0.5
            highlight.OutlineTransparency = 0
            highlight.Adornee = train
            highlight.Parent = train
        end)
    end
end

local function removeTrainESP(train)
    if train:FindFirstChild(\"ESP_Highlight\") then
        train.ESP_Highlight:Destroy()
    end
end

runService.RenderStepped:Connect(function()
    if trainFolder then
        for _, train in ipairs(trainFolder:GetChildren()) do
            if getgenv().esptrain then
                addTrainESP(train)
            else
                removeTrainESP(train)
            end
        end
    end
end)

local Toggle = Tabs.Train:AddToggle(\"MyToggle\", 
{
    Title = \"Train ESP\", 
    Description = \"\",
    Default = false,
    Callback = function(state)
        getgenv().esptrain = state
    end 
})

local textLabel1 = game.Workspace.Train.TrainControls.DistanceDial.SurfaceGui.TextLabel
local textLabel2 = game.Workspace.Train.TrainControls.TimeDial.SurfaceGui.TextLabel

local meterGui, timeGui = nil, nil

local function createDisplayGui(state, guiType)
    local CoreGui = game:GetService(\"CoreGui\")

    if guiType == \"meter\" then
        if state then
            if not meterGui then
                meterGui = Instance.new(\"ScreenGui\")
                meterGui.Name = \"DistanceDisplay\"
                meterGui.Parent = CoreGui
                meterGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling

                local distanceLabel = Instance.new(\"TextLabel\")
                distanceLabel.Name = \"Distance\"
                distanceLabel.Parent = meterGui
                distanceLabel.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
                distanceLabel.BackgroundTransparency = 1 
                distanceLabel.BorderSizePixel = 0
                distanceLabel.Position = UDim2.new(0.0102, 0, 0.5585, 0)
                distanceLabel.Size = UDim2.new(0.0961, 0, 0.0832, 0)
                distanceLabel.Font = Enum.Font.SourceSansBold
                distanceLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
                distanceLabel.TextScaled = true
                distanceLabel.TextStrokeTransparency = 0
                distanceLabel.TextWrapped = true
                distanceLabel.TextXAlignment = Enum.TextXAlignment.Left

                local function updateDistance()
                    distanceLabel.Text = textLabel1.Text
                end

                updateDistance()
                textLabel1:GetPropertyChangedSignal(\"Text\"):Connect(updateDistance)
            end
            meterGui.Enabled = true
        else
            if meterGui then
                meterGui.Enabled = false
            end
        end
    elseif guiType == \"time\" then
        if state then
            if not timeGui then
                timeGui = Instance.new(\"ScreenGui\")
                timeGui.Name = \"TimeDisplay\"
                timeGui.Parent = CoreGui
                timeGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling

                local timeLabel = Instance.new(\"TextLabel\")
                timeLabel.Name = \"Time\"
                timeLabel.Parent = timeGui
                timeLabel.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
                timeLabel.BackgroundTransparency = 1
                timeLabel.BorderSizePixel = 0
                timeLabel.Position = UDim2.new(0.0102, 0, 0.6406, 0)
                timeLabel.Size = UDim2.new(0.0961, 0, 0.0832, 0)
                timeLabel.Font = Enum.Font.SourceSansBold
                timeLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
                timeLabel.TextScaled = true
                timeLabel.TextStrokeTransparency = 0
                timeLabel.TextWrapped = true
                timeLabel.TextXAlignment = Enum.TextXAlignment.Left

                local function updateTime()
                    timeLabel.Text = textLabel2.Text
                end

                updateTime()
                textLabel2:GetPropertyChangedSignal(\"Text\"):Connect(updateTime)
            end
            timeGui.Enabled = true
        else
            if timeGui then
                timeGui.Enabled = false
            end
        end
    end
end

local DistanceToggle = Tabs.Train:AddToggle(\"DistanceToggle\", {
    Title = \"Distance Display\",
    Description = \"\",
    Default = false,
    Callback = function(state)
        createDisplayGui(state, \"meter\")
    end
})

local TimeToggle = Tabs.Train:AddToggle(\"TimeToggle\", {
    Title = \"Time Display\",
    Description = \"\",
    Default = false,
    Callback = function(state)
        createDisplayGui(state, \"time\")
    end
})


local CoreGui = game:GetService(\"CoreGui\")

local FuelGui = Instance.new(\"ScreenGui\")
FuelGui.Name = \"FuelDisplay\"
FuelGui.Parent = CoreGui
FuelGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
FuelGui.Enabled = false

local FuelLabel = Instance.new(\"TextLabel\")
FuelLabel.Name = \"FuelIndicator\"
FuelLabel.Parent = FuelGui
FuelLabel.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
FuelLabel.BackgroundTransparency = 1 
FuelLabel.BorderSizePixel = 0
FuelLabel.Position = UDim2.new(0.0102, 0, 0.3421, 0)
FuelLabel.Size = UDim2.new(0.0961, 0, 0.0832, 0)
FuelLabel.Font = Enum.Font.Cartoon
FuelLabel.TextColor3 = Color3.fromRGB(255, 0, 4)
FuelLabel.TextScaled = true
FuelLabel.TextStrokeTransparency = 0
FuelLabel.TextWrapped = true
FuelLabel.TextXAlignment = Enum.TextXAlignment.Left

local function updateFuelDisplay()
    local train = game.Workspace:FindFirstChild(\"Train\")
    local fuelValue = train and train:FindFirstChild(\"Fuel\")
    
    if fuelValue and fuelValue:IsA(\"IntValue\") then
        FuelLabel.Text = string.format(\"Fuel: %d\", fuelValue.Value)
    else
        FuelLabel.Text = \"Fuel unavailable\"
    end
end

updateFuelDisplay()

local train = game.Workspace:FindFirstChild(\"Train\")
local fuel = train and train:FindFirstChild(\"Fuel\")
if fuel then
    fuel.Changed:Connect(updateFuelDisplay)
end

local FuelToggle = Tabs.Train:AddToggle(\"FuelToggle\", {
    Title = \"Fuel Display\",
    Description = \"\",
    Default = false,
    Callback = function(state)
        FuelGui.Enabled = state
    end
})

local Players = game:GetService(\"Players\")
local RunService = game:GetService(\"RunService\")
local Workspace = game:GetService(\"Workspace\")

local player = Players.LocalPlayer
local train = Workspace:FindFirstChild(\"Train\")
if not train then
    warn(\"train not found\")
    return
end

local trainPart = train.PrimaryPart or train:FindFirstChildWhichIsA(\"BasePart\")
if not trainPart then
    warn(\"part not found in train\")
    return
end

local traindistance = Instance.new(\"ScreenGui\")
traindistance.Name = \"TrainDistance\"
traindistance.Parent = game:GetService(\"CoreGui\")
traindistance.ZIndexBehavior = Enum.ZIndexBehavior.Sibling

local TutorialText = Instance.new(\"TextLabel\")
TutorialText.Name = \"TutorialText\"
TutorialText.Parent = traindistance
TutorialText.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
TutorialText.BackgroundTransparency = 1.000
TutorialText.BorderColor3 = Color3.fromRGB(0, 0, 0)
TutorialText.BorderSizePixel = 0
TutorialText.Position = UDim2.new(0.882562518, 0, 0.465028077, 0)
TutorialText.Size = UDim2.new(0.117042422, 0, 0.0666883513, 0)
TutorialText.Font = Enum.Font.Highway
TutorialText.Text = \"Train Distance: 999999 Studs\"
TutorialText.TextColor3 = Color3.fromRGB(251, 255, 0)
TutorialText.TextScaled = true
TutorialText.TextSize = 14.000
TutorialText.TextStrokeTransparency = 0.000
TutorialText.TextWrapped = true
TutorialText.Visible = false

local toggleActive = false

local Toggle = Tabs.Train:AddToggle(\"MyToggle\", {
    Title = \"Train Distance GUI\", 
    Description = \"\",
    Default = false,
    Callback = function(state)
        toggleActive = state
        TutorialText.Visible = state 
    end
})

RunService.Heartbeat:Connect(function()
    if toggleActive and player.Character and player.Character.PrimaryPart then
        local playerPos = player.Character.PrimaryPart.Position
        local trainPos = trainPart.Position
        local distance = (playerPos - trainPos).Magnitude

        TutorialText.Text = \"Train Distance: \" .. math.floor(distance) .. \" Studs\"
    end
end)

local clickDetector = game.Workspace.Train.TrainControls.Lever.HitBox.ClickDetector

Tabs.Train:AddButton({
    Title = \"Honk Train\",
    Description = \"Also Has A Keybind (Press H)\",
    Callback = function()
        if clickDetector then
            fireclickdetector(clickDetector)
            print(\"triggered\")
        else
            warn(\"not found\")
        end
    end
})

local UserInputService = game:GetService(\"UserInputService\")

UserInputService.InputBegan:Connect(function(input, gameProcessed)
    if not gameProcessed and input.KeyCode == Enum.KeyCode.H then
        if clickDetector then
            fireclickdetector(clickDetector)
            print(\"triggered\")
        else
            warn(\"not found\")
        end
    end
end)

Tabs.Teleport:AddButton({
    Title = \"Teleport To Test Server\",
    Description = \"\",
    Callback = function()
        local TeleportService = game:GetService(\"TeleportService\")
        local gameId = 98018823628597
        
        local success, error = pcall(function()
            TeleportService:Teleport(gameId, game.Players.LocalPlayer)
        end)
        
        if success then
            print(\"teleport to game id: \" .. gameId)
        else
            warn(\"teleport failed: \" .. error)
        end
    end
})

local Players = game:GetService(\"Players\")
local ProximityPromptService = game:GetService(\"ProximityPromptService\")
local Workspace = game:GetService(\"Workspace\")
local VirtualInputManager = game:GetService(\"VirtualInputManager\")

local player = Players.LocalPlayer
local character = player.Character
local humanoidRootPart = nil
local runtimeItems = nil
local moneybag = nil
local bandage = nil
local bond = nil  
local promptConnection = nil
local isRunning = false
local moneybagToggle = false
local bandageToggle = false
local bondToggle = false  
local isHoldingE = false
local eKeyTask = nil

local function updateCharacter(newCharacter)
    character = newCharacter
    humanoidRootPart = character and character:FindFirstChild(\"HumanoidRootPart\")
end

local function setupPromptListener()
    if promptConnection then return end
    
    local success, err = pcall(function()
        promptConnection = ProximityPromptService.PromptButtonHoldBegan:Connect(function(prompt)
            local noholddelay = true
            if noholddelay and prompt:IsA(\"ProximityPrompt\") then
                prompt.HoldDuration = 0
            end
        end)
    end)
end

local function cleanupPromptListener()
    if promptConnection then
        pcall(function()
            promptConnection:Disconnect()
            promptConnection = nil
        end)
    end
end

local function stopEKey()
    if eKeyTask then
        isHoldingE = false
        VirtualInputManager:SendKeyEvent(false, Enum.KeyCode.E, false, game)
        eKeyTask = nil
    end
end

local function startDetectionLoop()
    if isRunning then return end
    isRunning = true
    
    while isRunning do
        if not (moneybagToggle or bandageToggle or bondToggle) then
            stopEKey()
            cleanupPromptListener()
            task.wait(0.5)
        else
            if not runtimeItems then
                local success, err = pcall(function()
                    runtimeItems = Workspace:WaitForChild(\"RuntimeItems\", 5)
                end)
                if not success or not runtimeItems then
                    break
                end
            end
            
            moneybag = moneybagToggle and runtimeItems:FindFirstChild(\"Moneybag\") or nil
            bandage = bandageToggle and runtimeItems:FindFirstChild(\"Bandage\") or nil
            bond = bondToggle and runtimeItems:FindFirstChild(\"Bond\") or nil
            
            if moneybag or bandage or bond then
                setupPromptListener()
                
                if not eKeyTask and (moneybagToggle or bandageToggle or bondToggle) then
                    isHoldingE = true
                    eKeyTask = task.spawn(function()
                        while isHoldingE do
                            VirtualInputManager:SendKeyEvent(true, Enum.KeyCode.E, false, game)
                            task.wait(0.1)
                        end
                        VirtualInputManager:SendKeyEvent(false, Enum.KeyCode.E, false, game)
                    end)
                end
                
                task.wait(0.1)
            else
                stopEKey()
                cleanupPromptListener()
                task.wait(0.5)
            end
        end
    end
    isRunning = false
    stopEKey()
    cleanupPromptListener()
end

if character then
    updateCharacter(character)
end

player.CharacterAdded:Connect(function(newCharacter)
    updateCharacter(newCharacter)
end)

player.CharacterRemoving:Connect(function()
    character = nil
    humanoidRootPart = nil
end)

coroutine.wrap(startDetectionLoop)()

local MoneybagToggle = Tabs.Autofarm:AddToggle(\"MoneybagCollect\", 
{
    Title = \"Auto Collect Cash\", 
    Description = \"\",
    Default = false,
    Callback = function(state)
        moneybagToggle = state
        if not state then
            stopEKey()
        end
    end 
})

local BandageToggle = Tabs.Autofarm:AddToggle(\"BandageCollect\", 
{
    Title = \"Auto Pickup Bandage\", 
    Description = \"\",
    Default = false,
    Callback = function(state)
        bandageToggle = state
        if not state then
            stopEKey()
        end
    end 
})

local BondToggle = Tabs.Autofarm:AddToggle(\"BondCollect\", 
{
    Title = \"Auto Collect Bond(near)\", 
    Description = \"\",
    Default = false,
    Callback = function(state)
        bondToggle = state
        if not state then
            stopEKey()
        end
    end 
})

Tabs.Autofarm:AddButton({
    Title = \"Auto Farm Wins (PATCHED)\",
    Description = \"\",
    Callback = function()

        local EnhancedTeleportGUI = {}
        EnhancedTeleportGUI.__index = EnhancedTeleportGUI
        
        local Players = game:GetService(\"Players\")
        local UserInputService = game:GetService(\"UserInputService\")
        local RunService = game:GetService(\"RunService\")
        local ProximityPromptService = game:GetService(\"ProximityPromptService\")
        
        local GUI_SIZE = UDim2.new(0, 240, 0, 280)
        local MINIMIZED_SIZE = UDim2.new(0, 240, 0, 30)
        local CENTER_OFFSET = UDim2.new(0.5, -120, 0.5, -140)
        local TELEPORT_POSITION = CFrame.new(-346, -69, -49060)
        
        local Colors = {
            FrameBackground = Color3.fromRGB(25, 25, 25),
            Border = Color3.fromRGB(50, 150, 255),
            TimerText = Color3.fromRGB(100, 255, 255),
            ButtonBackground = Color3.fromRGB(35, 35, 35),
            ButtonHover = Color3.fromRGB(45, 45, 45),
            TeleportActive = Color3.fromRGB(0, 120, 200),
            TeleportCooldown = Color3.fromRGB(200, 50, 50),
            NoclipOn = Color3.fromRGB(50, 200, 50),
            NoclipOff = Color3.fromRGB(60, 60, 60),
            AutoTPOn = Color3.fromRGB(50, 180, 50),
            AutoTPOff = Color3.fromRGB(60, 60, 60),
            CreditText = Color3.fromRGB(80, 140, 255),
            InfoText = Color3.fromRGB(180, 180, 180),
            White = Color3.fromRGB(220, 220, 220)
        }
        
        function EnhancedTeleportGUI.new()
            local self = setmetatable({}, EnhancedTeleportGUI)
            self.Player = Players.LocalPlayer
            self.IsNoclipEnabled = false
            self.IsAutoTeleportEnabled = false
            self.IsMinimized = false
            self.TimerSeconds = 600
            self.IsTeleportOnCooldown = false
            self.NoclipConnection = nil
            
            self:InitializeGUI()
            self:SetupEventListeners()
            self:StartTimer()
            return self
        end
        
        function EnhancedTeleportGUI:InitializeGUI()
            self.ScreenGui = Instance.new(\"ScreenGui\")
            self.ScreenGui.Parent = self.Player:WaitForChild(\"PlayerGui\")
        
            self.MainFrame = Instance.new(\"Frame\")
            self.MainFrame.Size = GUI_SIZE
            self.MainFrame.Position = CENTER_OFFSET
            self.MainFrame.BackgroundColor3 = Colors.FrameBackground
            self.MainFrame.BackgroundTransparency = 0.05
            self.MainFrame.BorderSizePixel = 0
            self.MainFrame.Active = true
            self.MainFrame.Draggable = true
            self.MainFrame.Parent = self.ScreenGui
        
            local border = Instance.new(\"UIGradient\")
            border.Color = ColorSequence.new(Colors.Border, Color3.fromRGB(20, 80, 150))
            border.Rotation = 45
            local stroke = Instance.new(\"UIStroke\")
            stroke.Thickness = 2
            stroke.ApplyStrokeMode = Enum.ApplyStrokeMode.Border
            stroke.Parent = self.MainFrame
            border.Parent = stroke
        
            local corner = Instance.new(\"UICorner\")
            corner.CornerRadius = UDim.new(0, 8)
            corner.Parent = self.MainFrame
        
            self:CreateTimerLabel()
            self:CreateTeleportButton()
            self:CreateNoclipButton()
            self:CreateAutoTeleportCheckbox()
            self:CreateInfoLabel()
            self:CreateCreditLabel()
            self:CreateMinimizeButton()
            self:CreateCloseButton()
        end
        
        function EnhancedTeleportGUI:CreateTimerLabel()
            self.TimerLabel = Instance.new(\"TextLabel\")
            self.TimerLabel.Size = UDim2.new(0, 120, 0, 30)
            self.TimerLabel.Position = UDim2.new(0.5, -60, 0.05, 8)
            self.TimerLabel.Text = \"10:00\"
            self.TimerLabel.Font = Enum.Font.GothamBlack
            self.TimerLabel.TextSize = 22
            self.TimerLabel.TextColor3 = Colors.TimerText
            self.TimerLabel.BackgroundTransparency = 1
            self.TimerLabel.Parent = self.MainFrame
            
            local glow = Instance.new(\"UIStroke\")
            glow.Thickness = 1
            glow.Color = Colors.TimerText
            glow.Transparency = 0.7
            glow.Parent = self.TimerLabel
        end
        
        function EnhancedTeleportGUI:CreateTeleportButton()
            self.TeleportButton = Instance.new(\"TextButton\")
            self.TeleportButton.Size = UDim2.new(0, 200, 0, 40)
            self.TeleportButton.Position = UDim2.new(0.5, -100, 0.22, 0)
            self.TeleportButton.Text = \"Teleport: Bridge [T]\"
            self.TeleportButton.Font = Enum.Font.GothamBold
            self.TeleportButton.TextSize = 18
            self.TeleportButton.TextColor3 = Colors.White
            self.TeleportButton.BackgroundColor3 = Colors.ButtonBackground
            self.TeleportButton.BorderSizePixel = 0
            self.TeleportButton.Parent = self.MainFrame
            
            local corner = Instance.new(\"UICorner\")
            corner.CornerRadius = UDim.new(0, 6)
            corner.Parent = self.TeleportButton
        end
        
        function EnhancedTeleportGUI:CreateNoclipButton()
            self.NoclipButton = Instance.new(\"TextButton\")
            self.NoclipButton.Size = UDim2.new(0, 200, 0, 40)
            self.NoclipButton.Position = UDim2.new(0.5, -100, 0.4, 0)
            self.NoclipButton.Text = \"NoClip: OFF [C]\"
            self.NoclipButton.Font = Enum.Font.GothamBold
            self.NoclipButton.TextSize = 18
            self.NoclipButton.TextColor3 = Colors.White
            self.NoclipButton.BackgroundColor3 = Colors.NoclipOff
            self.NoclipButton.BorderSizePixel = 0
            self.NoclipButton.Parent = self.MainFrame
            
            local corner = Instance.new(\"UICorner\")
            corner.CornerRadius = UDim.new(0, 6)
            corner.Parent = self.NoclipButton
        end
        
        function EnhancedTeleportGUI:CreateAutoTeleportCheckbox()
            self.AutoTeleportCheckbox = Instance.new(\"TextButton\")
            self.AutoTeleportCheckbox.Size = UDim2.new(0, 200, 0, 35)
            self.AutoTeleportCheckbox.Position = UDim2.new(0.5, -100, 0.58, 0)
            self.AutoTeleportCheckbox.Text = \"Auto-TP when 0: OFF\"
            self.AutoTeleportCheckbox.Font = Enum.Font.GothamSemibold
            self.AutoTeleportCheckbox.TextSize = 16
            self.AutoTeleportCheckbox.TextColor3 = Colors.White
            self.AutoTeleportCheckbox.BackgroundColor3 = Colors.AutoTPOff
            self.AutoTeleportCheckbox.BorderSizePixel = 0
            self.AutoTeleportCheckbox.Parent = self.MainFrame
            
            local corner = Instance.new(\"UICorner\")
            corner.CornerRadius = UDim.new(0, 6)
            corner.Parent = self.AutoTeleportCheckbox
        end
        
        function EnhancedTeleportGUI:CreateInfoLabel()
            self.InfoLabel = Instance.new(\"TextLabel\")
            self.InfoLabel.Size = UDim2.new(0, 200, 0, 50)
            self.InfoLabel.Position = UDim2.new(0.5, -100, 0.71, 0)
            self.InfoLabel.Text = \"PATCHED - DON'T WORK ANYMORE\"
            self.InfoLabel.Font = Enum.Font.Gotham
            self.InfoLabel.TextSize = 11
            self.InfoLabel.TextColor3 = Colors.InfoText
            self.InfoLabel.BackgroundTransparency = 1
            self.InfoLabel.TextWrapped = true
            self.InfoLabel.TextYAlignment = Enum.TextYAlignment.Top
            self.InfoLabel.Parent = self.MainFrame
        end
        
        function EnhancedTeleportGUI:CreateCreditLabel()
            self.CreditLabel = Instance.new(\"TextLabel\")
            self.CreditLabel.Size = UDim2.new(0, 200, 0, 25)
            self.CreditLabel.Position = UDim2.new(0.5, -100, 0.87, 0)
            self.CreditLabel.Text = \"Created by hassanxzayn\"
            self.CreditLabel.Font = Enum.Font.Gotham
            self.CreditLabel.TextSize = 14
            self.CreditLabel.TextColor3 = Colors.CreditText
            self.CreditLabel.BackgroundTransparency = 1
            self.CreditLabel.Parent = self.MainFrame
        end
        
        function EnhancedTeleportGUI:CreateMinimizeButton()
            self.MinimizeButton = Instance.new(\"TextButton\")
            self.MinimizeButton.Size = UDim2.new(0, 25, 0, 25)
            self.MinimizeButton.Position = UDim2.new(1, -65, 0, 3)
            self.MinimizeButton.Text = \"−\"
            self.MinimizeButton.Font = Enum.Font.GothamBold
            self.MinimizeButton.TextSize = 20
            self.MinimizeButton.TextColor3 = Colors.White
            self.MinimizeButton.BackgroundColor3 = Colors.FrameBackground
            self.MinimizeButton.BorderSizePixel = 0
            self.MinimizeButton.Parent = self.MainFrame
            
            local corner = Instance.new(\"UICorner\")
            corner.CornerRadius = UDim.new(0, 4)
            corner.Parent = self.MinimizeButton
        end
        
        function EnhancedTeleportGUI:CreateCloseButton()
            self.CloseButton = Instance.new(\"TextButton\")
            self.CloseButton.Size = UDim2.new(0, 25, 0, 25)
            self.CloseButton.Position = UDim2.new(1, -35, 0, 3)
            self.CloseButton.Text = \"×\"
            self.CloseButton.Font = Enum.Font.GothamBold
            self.CloseButton.TextSize = 20
            self.CloseButton.TextColor3 = Colors.White
            self.CloseButton.BackgroundColor3 = Colors.FrameBackground
            self.CloseButton.BorderSizePixel = 0
            self.CloseButton.Parent = self.MainFrame
            
            local corner = Instance.new(\"UICorner\")
            corner.CornerRadius = UDim.new(0, 4)
            corner.Parent = self.CloseButton
        end
        
        function EnhancedTeleportGUI:Teleport(isAutoTriggered)
            if self.TimerSeconds > 0 and not isAutoTriggered then
                self.IsTeleportOnCooldown = true
                self.TeleportButton.BackgroundColor3 = Colors.TeleportCooldown
                self.TeleportButton.Text = \"Wait\"
                task.wait(1)
                self.TeleportButton.BackgroundColor3 = Colors.ButtonBackground
                self.TeleportButton.Text = \"Teleport: Bridge [T]\"
                self.IsTeleportOnCooldown = false
                return
            end
            
            local character = self.Player.Character
            if character and character:FindFirstChild(\"HumanoidRootPart\") then
                character:SetPrimaryPartCFrame(TELEPORT_POSITION)
                self.TeleportButton.BackgroundColor3 = Colors.TeleportActive
                task.wait(0.2)
                self.TeleportButton.BackgroundColor3 = Colors.ButtonBackground
            end
        end
        
        function EnhancedTeleportGUI:EnableNoclip()
            if self.NoclipConnection then return end
            
            self.IsNoclipEnabled = true
            self.NoclipButton.Text = \"NoClip: ON [C]\"
            self.NoclipButton.BackgroundColor3 = Colors.NoclipOn
            
            self.NoclipConnection = RunService.Stepped:Connect(function()
                if self.Player.Character then
                    for _, part in pairs(self.Player.Character:GetDescendants()) do
                        if part:IsA(\"BasePart\") and part.CanCollide then
                            part.CanCollide = false
                        end
                    end
                end
            end)
        end
        
        function EnhancedTeleportGUI:DisableNoclip()
            if not self.NoclipConnection then return end
            
            self.NoclipConnection:Disconnect()
            self.NoclipConnection = nil
            
            if self.Player.Character then
                for _, part in pairs(self.Player.Character:GetDescendants()) do
                    if part:IsA(\"BasePart\") then
                        part.CanCollide = true
                    end
                end
            end
            
            self.IsNoclipEnabled = false
            self.NoclipButton.Text = \"NoClip: OFF [C]\"
            self.NoclipButton.BackgroundColor3 = Colors.NoclipOff
        end
        
        function EnhancedTeleportGUI:StartTimer()
            task.spawn(function()
                while self.TimerSeconds > 0 and self.ScreenGui.Parent do
                    task.wait(1)
                    self.TimerSeconds -= 1
                    local minutes = math.floor(self.TimerSeconds / 60)
                    local seconds = self.TimerSeconds % 60
                    self.TimerLabel.Text = string.format(\"%02d:%02d\", minutes, seconds)
                    
                    if self.TimerSeconds <= 0 and self.IsAutoTeleportEnabled then
                        self:Teleport(true)
                    end
                end
            end)
        end
        
        function EnhancedTeleportGUI:ToggleMinimize()
            self.IsMinimized = not self.IsMinimized
            self.MainFrame.Size = self.IsMinimized and MINIMIZED_SIZE or GUI_SIZE
            
            self.TeleportButton.Visible = not self.IsMinimized
            self.NoclipButton.Visible = not self.IsMinimized
            self.AutoTeleportCheckbox.Visible = not self.IsMinimized
            self.InfoLabel.Visible = not self.IsMinimized
            self.CreditLabel.Visible = not self.IsMinimized
            
            self.TimerLabel.Position = self.IsMinimized and UDim2.new(0.5, -60, 0, 0) or UDim2.new(0.5, -60, 0.05, 8)
            self.MinimizeButton.Text = self.IsMinimized and \"+\" or \"−\"
        end
        
        function EnhancedTeleportGUI:SetupEventListeners()
            local function addHoverEffect(button)
                button.MouseEnter:Connect(function()
                    if (button == self.NoclipButton and not self.IsNoclipEnabled) or 
                       (button == self.AutoTeleportCheckbox and not self.IsAutoTeleportEnabled) or
                       (button == self.TeleportButton and not self.IsTeleportOnCooldown) or
                       (button == self.MinimizeButton or button == self.CloseButton) then
                        button.BackgroundColor3 = Colors.ButtonHover
                    end
                end)
                button.MouseLeave:Connect(function()
                    if button == self.NoclipButton then
                        button.BackgroundColor3 = self.IsNoclipEnabled and Colors.NoclipOn or Colors.NoclipOff
                    elseif button == self.AutoTeleportCheckbox then
                        button.BackgroundColor3 = self.IsAutoTeleportEnabled and Colors.AutoTPOn or Colors.AutoTPOff
                    elseif button == self.TeleportButton then
                        button.BackgroundColor3 = self.IsTeleportOnCooldown and Colors.TeleportCooldown or Colors.ButtonBackground
                    else
                        button.BackgroundColor3 = Colors.FrameBackground
                    end
                end)
            end
        
            self.TeleportButton.MouseButton1Click:Connect(function()
                self:Teleport(false)
            end)
            
            self.NoclipButton.MouseButton1Click:Connect(function()
                if self.IsNoclipEnabled then
                    self:DisableNoclip()
                else
                    self:EnableNoclip()
                end
            end)
            
            self.AutoTeleportCheckbox.MouseButton1Click:Connect(function()
                self.IsAutoTeleportEnabled = not self.IsAutoTeleportEnabled
                self.AutoTeleportCheckbox.Text = \"Auto-TP when 0: \" .. (self.IsAutoTeleportEnabled and \"ON\" or \"OFF\")
                self.AutoTeleportCheckbox.BackgroundColor3 = self.IsAutoTeleportEnabled and Colors.AutoTPOn or Colors.AutoTPOff
            end)
            
            self.MinimizeButton.MouseButton1Click:Connect(function()
                self:ToggleMinimize()
            end)
            
            self.CloseButton.MouseButton1Click:Connect(function()
                self:Destroy()
            end)
            
            addHoverEffect(self.TeleportButton)
            addHoverEffect(self.NoclipButton)
            addHoverEffect(self.AutoTeleportCheckbox)
            addHoverEffect(self.MinimizeButton)
            addHoverEffect(self.CloseButton)
            
            UserInputService.InputBegan:Connect(function(input, gameProcessed)
                if gameProcessed then return end
                if input.KeyCode == Enum.KeyCode.T then 
                    self:Teleport(false)
                elseif input.KeyCode == Enum.KeyCode.C then
                    if self.IsNoclipEnabled then self:DisableNoclip() else self:EnableNoclip() end
                end
            end)
            
            ProximityPromptService.PromptButtonHoldBegan:Connect(function(prompt)
                prompt.HoldDuration = 0
            end)
        end
        
        function EnhancedTeleportGUI:Destroy()
            if self.NoclipConnection then
                self.NoclipConnection:Disconnect()
                self.NoclipConnection = nil
            end
            self.ScreenGui:Destroy()
        end
        
        local gui = EnhancedTeleportGUI.new()
        return gui
    end
})

Tabs.Esp:AddButton({
    Title = \"Buildings ESP + Zombies Count In Building\",
    Description = \"Sometimes, it may display 0 zombies even when there are zombies present. If this happens, please click the button again to refresh the count. If it still shows 0 after pressing the button, then there are truly no zombies. However, to confirm, always click the button when it shows 0 zombies\",
    Callback = function()
        local buildingsFolder = game.Workspace:FindFirstChild(\"RandomBuildings\")

        if not buildingsFolder then
            warn(\"folder not found\")
            return
        end
        
        local function ensurePrimaryPart(model)
            if not model.PrimaryPart then
                for _, part in ipairs(model:GetChildren()) do
                    if part:IsA(\"BasePart\") then
                        model.PrimaryPart = part
                        break
                    end
                end
            end
        end
        
        local function countZombies(model)
            local zombiePart = model:FindFirstChild(\"StandaloneZombiePart\")
            if not zombiePart then 
                return 0 
            end
            
            local zombieFolder = zombiePart:FindFirstChild(\"Zombies\")
            if not zombieFolder then 
                return 0 
            end
            
            local count = 0
            for _, item in ipairs(zombieFolder:GetChildren()) do
                if item:IsA(\"Model\") then
                    count = count + 1
                end
            end
            return count
        end
        
        local function createBillboard(model, text, offset, name)
            local primaryPart = model.PrimaryPart
            if not primaryPart then return nil end
        
            local oldGui = model:FindFirstChild(name)
            if oldGui then oldGui:Destroy() end
        
            local billboardGui = Instance.new(\"BillboardGui\")
            billboardGui.Name = name
            billboardGui.Size = UDim2.new(50, 0, 15, 0)
            billboardGui.StudsOffset = Vector3.new(0, offset, 0)
            billboardGui.Adornee = primaryPart
            billboardGui.AlwaysOnTop = true
            billboardGui.Parent = model
        
            local textLabel = Instance.new(\"TextLabel\")
            textLabel.Size = UDim2.new(1, 0, 1, 0)
            textLabel.BackgroundTransparency = 1
            textLabel.TextScaled = true
            textLabel.TextColor3 = Color3.new(1, 1, 0)
            textLabel.TextStrokeColor3 = Color3.new(0, 0, 0)
            textLabel.TextStrokeTransparency = 0
            textLabel.Font = Enum.Font.GothamBlack
            textLabel.Text = text
            textLabel.Parent = billboardGui
            
            return billboardGui
        end
        
        local function updateZombieCount(model)
            local zombieGui = model:FindFirstChild(\"ZombieCounter\")
            if zombieGui then
                local textLabel = zombieGui:FindFirstChildOfClass(\"TextLabel\")
                if textLabel then
                    local count = countZombies(model)
                    textLabel.Text = \"Zombies: \" .. count
                    return count
                end
            end
            return 0
        end
        
        local function setupModel(model)
            if not model:IsA(\"Model\") then return end
            
            ensurePrimaryPart(model)
            
            local zombieCount = countZombies(model)
            local zombieGui = createBillboard(model, \"Zombies: \" .. zombieCount, 50, \"ZombieCounter\")
            createBillboard(model, model.Name, 65, \"ModelName\")
            
            task.spawn(function()
                local attempts = 0
                local maxAttempts = 5
                local lastCount = 0
                
                while attempts < maxAttempts do
                    local currentCount = updateZombieCount(model)
                    if currentCount > 0 and currentCount == lastCount then
                        break
                    end
                    lastCount = currentCount
                    attempts = attempts + 1
                    task.wait(0.5)
                end
            end)
            
            local zombiePart = model:FindFirstChild(\"StandaloneZombiePart\")
            if zombiePart then
                local zombieFolder = zombiePart:FindFirstChild(\"Zombies\")
                if zombieFolder then
                    zombieFolder.ChildAdded:Connect(function()
                        if zombieGui then
                            updateZombieCount(model)
                        end
                    end)
                    
                    zombieFolder.ChildRemoved:Connect(function()
                        if zombieGui then
                            updateZombieCount(model)
                        end
                    end)
                end
            end
            
            model:GetPropertyChangedSignal(\"Name\"):Connect(function()
                local nameGui = model:FindFirstChild(\"ModelName\")
                if nameGui then
                    local textLabel = nameGui:FindFirstChildOfClass(\"TextLabel\")
                    if textLabel then
                        textLabel.Text = model.Name
                    end
                end
            end)
        end
        
        local function cleanupModels()
            for _, child in ipairs(buildingsFolder:GetChildren()) do
                if child:IsA(\"BillboardGui\") then
                    if not child.Parent or not child.Parent:IsA(\"Model\") then
                        child:Destroy()
                    end
                end
            end
        end
        
        local function refreshModels()
            cleanupModels()
            for _, model in ipairs(buildingsFolder:GetChildren()) do
                setupModel(model)
            end
        end
        
        refreshModels()
        
        buildingsFolder.ChildAdded:Connect(function(model)
            task.wait(0.1)
            setupModel(model)
        end)
        
        buildingsFolder.ChildRemoved:Connect(function()
            task.wait(0.1)
            cleanupModels()
        end)
    end
})




Tabs.Esp:AddButton({
    Title = \"Ores ESP\",
    Description = \"\",
    Callback = function()
        local oreFolder = workspace:FindFirstChild(\"Ore\")
        local runService = game:GetService(\"RunService\")
        local camera = workspace.CurrentCamera
        local player = game.Players.LocalPlayer
        
        local espData = {} 
        
        local function createESP(model)
            if not model.PrimaryPart then return end
        
            local highlight = Instance.new(\"Highlight\")
            highlight.Parent = model
            highlight.FillColor = Color3.fromRGB(255, 128, 0)
            highlight.FillTransparency = 0.3 
            highlight.OutlineColor = Color3.fromRGB(255, 255, 255) 
            highlight.OutlineTransparency = 0
        
            local text = Drawing.new(\"Text\")
            text.Size = 22 
            text.Color = Color3.fromRGB(0, 255, 204) 
            text.Outline = true
            text.OutlineColor = Color3.fromRGB(10, 10, 10)
            text.Center = true
            text.Font = 3 
        
            espData[model] = {highlight = highlight, text = text}
        end
        
        local function removeESP(model)
            if espData[model] then
                espData[model].highlight:Destroy()
                espData[model].text:Remove()
                espData[model] = nil
            end
        end
        
        local function updateESP()
            for model, data in pairs(espData) do
                if model and model.PrimaryPart then
                    local screenPosition, onScreen = camera:WorldToViewportPoint(model.PrimaryPart.Position)
                    if onScreen then
                        local char = player.Character
                        local charPart = char and char:FindFirstChild(\"HumanoidRootPart\")
        
                        local distance = charPart and math.floor((charPart.Position - model.PrimaryPart.Position).Magnitude) or 0
        
                        if distance < 50 then
                            data.text.Color = Color3.fromRGB(255, 80, 80) 
                            data.text.Size = 24 
                        elseif distance < 150 then
                            data.text.Color = Color3.fromRGB(255, 190, 0) 
                            data.text.Size = 22
                        else
                            data.text.Color = Color3.fromRGB(0, 255, 204)
                            data.text.Size = 20 
                        end
        
                        data.text.Text = string.format(\"%s [%d Studs]\", model.Name:upper(), distance)
                        data.text.Position = Vector2.new(screenPosition.X, screenPosition.Y - 30)
                        data.text.Visible = true
                    else
                        data.text.Visible = false
                    end
                else
                    removeESP(model)
                end
            end
        end
        
        local function refreshESP()
            if oreFolder then
                for _, model in pairs(oreFolder:GetChildren()) do
                    if model:IsA(\"Model\") and not espData[model] and model.PrimaryPart then
                        createESP(model)
                    end
                end
        
                for model in pairs(espData) do
                    if not model.Parent then
                        removeESP(model)
                    end
                end
            end
        end
        
        runService.RenderStepped:Connect(function()
            refreshESP()
            updateESP()
        end)
    end
})

local fov = 136
local RunService = game:GetService(\"RunService\")
local UserInputService = game:GetService(\"UserInputService\")
local Cam = workspace.CurrentCamera
local Player = game:GetService(\"Players\").LocalPlayer

local FOVring = Drawing.new(\"Circle\")
FOVring.Visible = false
FOVring.Thickness = 2
FOVring.Color = Color3.fromRGB(128, 0, 128)
FOVring.Filled = false
FOVring.Radius = fov
FOVring.Position = Cam.ViewportSize / 2

local isAiming = false
local validNPCs = {}
local raycastParams = RaycastParams.new()
raycastParams.FilterType = Enum.RaycastFilterType.Blacklist

local Toggle = Tabs.Aimbot:AddToggle(\"MyToggle\", 
{
    Title = \"Aimbot + FOV Circle\", 
    Description = \"Also Has A Keybind (Press L)\",
    Default = false,
    Callback = function(value)
        isAiming = value
        FOVring.Visible = value
    end 
})

local function isNPC(obj)
    return obj:IsA(\"Model\") 
        and obj:FindFirstChild(\"Humanoid\")
        and obj.Humanoid.Health > 0
        and obj:FindFirstChild(\"Head\")
        and obj:FindFirstChild(\"HumanoidRootPart\")
        and not game:GetService(\"Players\"):GetPlayerFromCharacter(obj)
end

local function updateNPCs()
    local tempTable = {}
    for _, obj in ipairs(workspace:GetDescendants()) do
        if isNPC(obj) then
            tempTable[obj] = true
        end
    end
    for i = #validNPCs, 1, -1 do
        if not tempTable[validNPCs[i]] then
            table.remove(validNPCs, i)
        end
    end
    for obj in pairs(tempTable) do
        if not table.find(validNPCs, obj) then
            table.insert(validNPCs, obj)
        end
    end
end

local function handleDescendant(descendant)
    if isNPC(descendant) then
        table.insert(validNPCs, descendant)
        local humanoid = descendant:WaitForChild(\"Humanoid\")
        humanoid.Destroying:Connect(function()
            for i = #validNPCs, 1, -1 do
                if validNPCs[i] == descendant then
                    table.remove(validNPCs, i)
                    break
                end
            end
        end)
    end
end

workspace.DescendantAdded:Connect(handleDescendant)

local function updateDrawings()
    FOVring.Position = Cam.ViewportSize / 2
    FOVring.Radius = fov * (Cam.ViewportSize.Y / 1080)
end

local function predictPos(target)
    local rootPart = target:FindFirstChild(\"HumanoidRootPart\")
    local head = target:FindFirstChild(\"Head\")
    if not rootPart or not head then
        return head and head.Position or rootPart and rootPart.Position
    end
    local velocity = rootPart.Velocity
    local predictionTime = 0.02
    local basePosition = rootPart.Position + velocity * predictionTime
    local headOffset = head.Position - rootPart.Position
    return basePosition + headOffset
end

local function getTarget()
    local nearest = nil
    local minDistance = math.huge
    local viewportCenter = Cam.ViewportSize / 2
    raycastParams.FilterDescendantsInstances = {Player.Character}
    for _, npc in ipairs(validNPCs) do
        local predictedPos = predictPos(npc)
        local screenPos, visible = Cam:WorldToViewportPoint(predictedPos)
        if visible and screenPos.Z > 0 then
            local ray = workspace:Raycast(
                Cam.CFrame.Position,
                (predictedPos - Cam.CFrame.Position).Unit * 1000,
                raycastParams
            )
            if ray and ray.Instance:IsDescendantOf(npc) then
                local distance = (Vector2.new(screenPos.X, screenPos.Y) - viewportCenter).Magnitude
                if distance < minDistance and distance < fov then
                    minDistance = distance
                    nearest = npc
                end
            end
        end
    end
    return nearest
end

local function aim(targetPosition)
    local currentCF = Cam.CFrame
    local targetDirection = (targetPosition - currentCF.Position).Unit
    local smoothFactor = 0.581
    local newLookVector = currentCF.LookVector:Lerp(targetDirection, smoothFactor)
    Cam.CFrame = CFrame.new(currentCF.Position, currentCF.Position + newLookVector)
end

local heartbeat = RunService.Heartbeat
local lastUpdate = 0
local UPDATE_INTERVAL = 0.4

heartbeat:Connect(function(dt)
    updateDrawings()
    lastUpdate = lastUpdate + dt
    if lastUpdate >= UPDATE_INTERVAL then
        updateNPCs()
        lastUpdate = 0
    end
    if isAiming then
        local target = getTarget()
        if target then
            local predictedPosition = predictPos(target)
            aim(predictedPosition)
        end
    end
end)

UserInputService.InputBegan:Connect(function(input)
    if input.KeyCode == Enum.KeyCode.L then
        isAiming = not isAiming
        FOVring.Visible = isAiming
        Toggle:SetValue(isAiming) 
    end
end)

updateNPCs()
workspace.DescendantRemoved:Connect(function(descendant)
    if isNPC(descendant) then
        for i = #validNPCs, 1, -1 do
            if validNPCs[i] == descendant then
                table.remove(validNPCs, i)
                break
            end
        end
    end
end)

game:GetService(\"Players\").PlayerRemoving:Connect(function()
    FOVring:Remove()
end)
