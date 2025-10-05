-- Prints are required ("May not work") 
-- Version 1.0
-- Commands Hub 
-- Script made by a Brazilian, so pay close attention to what is in the Prints and some local names. 
-- Anti-cheat only on Speed, then I'll move on to the whole code ("In an experiment it can be detected, I will think about improving it later")
--Roblox Script 

local TweenService = game:GetService("TweenService")
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local Lighting = game:GetService("Lighting")
local Workspace = game:GetService("Workspace")

local player = Players.LocalPlayer
local playerGui = player:WaitForChild("PlayerGui")

print("Commands Hub iniciado.")

local speedEnabled = false
local espEnabled = false
local antiLagEnabled = false
local hitboxEnabled = false

local currentSpeed = 19
local normalSpeed = 16

local espConnections = {}
local highlights = {}

local originalSettings = {}
local disabledObjects = {}

local hitboxSize = 5
local hitboxTransparency = 0.7
local hitboxParts = {}

local espConfig = {
    showNames = true,
    showDistance = true,
    highlightEnabled = true,
    customColor = Color3.fromRGB(70, 110, 200),
    maxDistance = 1000
}

local function tweenProperty(object, property, goal, duration)
    local tweenInfo = TweenInfo.new(duration, Enum.EasingStyle.Quad, Enum.EasingDirection.Out)
    local tween = TweenService:Create(object, tweenInfo, {[property] = goal})
    tween:Play()
    return tween
end

-- 
local screenGui = Instance.new("ScreenGui")
screenGui.Name = "CommandsHub"
screenGui.ResetOnSpawn = false
screenGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
screenGui.Parent = playerGui

-- 
local mainFrame = Instance.new("Frame")
mainFrame.Name = "MainFrame"
mainFrame.Size = UDim2.new(0, 450, 0, 350)
mainFrame.Position = UDim2.new(0.5, -225, 0.5, -175)
mainFrame.BackgroundColor3 = Color3.fromRGB(12, 12, 12)
mainFrame.BorderSizePixel = 0
mainFrame.Active = true
mainFrame.Draggable = true
mainFrame.Parent = screenGui

local mainFrameCorner = Instance.new("UICorner")
mainFrameCorner.CornerRadius = UDim.new(0, 12)
mainFrameCorner.Parent = mainFrame

local mainFrameStroke = Instance.new("UIStroke")
mainFrameStroke.Color = Color3.fromRGB(45, 45, 45)
mainFrameStroke.Thickness = 1
mainFrameStroke.Transparency = 0.3
mainFrameStroke.Parent = mainFrame

-- 
local shadow = Instance.new("Frame")
shadow.Name = "Shadow"
shadow.Size = UDim2.new(1, 16, 1, 16)
shadow.Position = UDim2.new(0, -8, 0, -8)
shadow.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
shadow.BackgroundTransparency = 0.7
shadow.BorderSizePixel = 0
shadow.ZIndex = 0
shadow.Parent = mainFrame

local shadowCorner = Instance.new("UICorner")
shadowCorner.CornerRadius = UDim.new(0, 14)
shadowCorner.Parent = shadow

-- 
local header = Instance.new("Frame")
header.Name = "Header"
header.Size = UDim2.new(1, 0, 0, 45)
header.BackgroundColor3 = Color3.fromRGB(18, 18, 18)
header.BorderSizePixel = 0
header.Parent = mainFrame

local headerCorner = Instance.new("UICorner")
headerCorner.CornerRadius = UDim.new(0, 10)
headerCorner.Parent = header

local headerStroke = Instance.new("UIStroke")
headerStroke.Color = Color3.fromRGB(35, 35, 35)
headerStroke.Thickness = 1
headerStroke.Transparency = 0.5
headerStroke.ApplyStrokeMode = Enum.ApplyStrokeMode.Border
headerStroke.Parent = header

-- 
local title = Instance.new("TextLabel")
title.Size = UDim2.new(0, 300, 1, 0)
title.Position = UDim2.new(0, 15, 0, 0)
title.BackgroundTransparency = 1
title.Text = "Commands Hub ⌘"
title.TextColor3 = Color3.fromRGB(255, 255, 255)
title.TextSize = 16
title.Font = Enum.Font.GothamBold
title.TextXAlignment = Enum.TextXAlignment.Left
title.Parent = header


local minimizeBtn = Instance.new("TextButton")
minimizeBtn.Size = UDim2.new(0, 32, 0, 32)
minimizeBtn.Position = UDim2.new(1, -75, 0.5, -16)
minimizeBtn.BackgroundColor3 = Color3.fromRGB(35, 35, 35)
minimizeBtn.Text = "−"
minimizeBtn.TextColor3 = Color3.fromRGB(220, 220, 220)
minimizeBtn.TextSize = 18
minimizeBtn.Font = Enum.Font.GothamBold
minimizeBtn.Parent = header

local minCorner = Instance.new("UICorner")
minCorner.CornerRadius = UDim.new(0, 6)
minCorner.Parent = minimizeBtn

local minStroke = Instance.new("UIStroke")
minStroke.Color = Color3.fromRGB(50, 50, 50)
minStroke.Thickness = 1
minStroke.Transparency = 0.5
minStroke.Parent = minimizeBtn

local closeBtn = Instance.new("TextButton")
closeBtn.Size = UDim2.new(0, 32, 0, 32)
closeBtn.Position = UDim2.new(1, -38, 0.5, -16)
closeBtn.BackgroundColor3 = Color3.fromRGB(150, 40, 40)
closeBtn.Text = "×"
closeBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
closeBtn.TextSize = 22
closeBtn.Font = Enum.Font.GothamBold
closeBtn.Parent = header

local closeCorner = Instance.new("UICorner")
closeCorner.CornerRadius = UDim.new(0, 6)
closeCorner.Parent = closeBtn

local closeStroke = Instance.new("UIStroke")
closeStroke.Color = Color3.fromRGB(180, 50, 50)
closeStroke.Thickness = 1
closeStroke.Transparency = 0.3
closeStroke.Parent = closeBtn

-- Container tabs
local tabContainer = Instance.new("Frame")
tabContainer.Size = UDim2.new(1, 0, 0, 38)
tabContainer.Position = UDim2.new(0, 0, 0, 45)
tabContainer.BackgroundColor3 = Color3.fromRGB(15, 15, 15)
tabContainer.BorderSizePixel = 0
tabContainer.Parent = mainFrame

local tabDivider = Instance.new("Frame")
tabDivider.Size = UDim2.new(1, 0, 0, 1)
tabDivider.Position = UDim2.new(0, 0, 1, 0)
tabDivider.BackgroundColor3 = Color3.fromRGB(35, 35, 35)
tabDivider.BorderSizePixel = 0
tabDivider.Parent = tabContainer

-- Tab Main
local mainTab = Instance.new("TextButton")
mainTab.Size = UDim2.new(0, 140, 0, 30)
mainTab.Position = UDim2.new(0, 12, 0.5, -15)
mainTab.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
mainTab.Text = "Main"
mainTab.TextColor3 = Color3.fromRGB(255, 255, 255)
mainTab.TextSize = 13
mainTab.Font = Enum.Font.GothamMedium
mainTab.Parent = tabContainer

local mainTabCorner = Instance.new("UICorner")
mainTabCorner.CornerRadius = UDim.new(0, 6)
mainTabCorner.Parent = mainTab

local mainTabStroke = Instance.new("UIStroke")
mainTabStroke.Color = Color3.fromRGB(80, 120, 200)
mainTabStroke.Thickness = 1
mainTabStroke.Transparency = 0
mainTabStroke.Parent = mainTab

-- Tab Settings
local settingsTab = Instance.new("TextButton")
settingsTab.Size = UDim2.new(0, 140, 0, 30)
settingsTab.Position = UDim2.new(0, 160, 0.5, -15)
settingsTab.BackgroundColor3 = Color3.fromRGB(22, 22, 22)
settingsTab.Text = "Settings"
settingsTab.TextColor3 = Color3.fromRGB(140, 140, 140)
settingsTab.TextSize = 13
settingsTab.Font = Enum.Font.Gotham
settingsTab.Parent = tabContainer

local settingsTabCorner = Instance.new("UICorner")
settingsTabCorner.CornerRadius = UDim.new(0, 6)
settingsTabCorner.Parent = settingsTab

local settingsTabStroke = Instance.new("UIStroke")
settingsTabStroke.Color = Color3.fromRGB(40, 40, 40)
settingsTabStroke.Thickness = 1
settingsTabStroke.Transparency = 0.5
settingsTabStroke.Parent = settingsTab

-- 
local contentFrame = Instance.new("ScrollingFrame")
contentFrame.Size = UDim2.new(1, -24, 1, -115)
contentFrame.Position = UDim2.new(0, 12, 0, 88)
contentFrame.BackgroundColor3 = Color3.fromRGB(15, 15, 15)
contentFrame.BorderSizePixel = 0
contentFrame.ScrollBarThickness = 4
contentFrame.ScrollBarImageColor3 = Color3.fromRGB(60, 60, 60)
contentFrame.CanvasSize = UDim2.new(0, 0, 0, 370)
contentFrame.Parent = mainFrame

local contentCorner = Instance.new("UICorner")
contentCorner.CornerRadius = UDim.new(0, 6)
contentCorner.Parent = contentFrame

local contentStroke = Instance.new("UIStroke")
contentStroke.Color = Color3.fromRGB(30, 30, 30)
contentStroke.Thickness = 1
contentStroke.Transparency = 0.5
contentStroke.Parent = contentFrame

--  Main
local mainContent = Instance.new("Frame")
mainContent.Size = UDim2.new(1, 0, 1, 0)
mainContent.BackgroundTransparency = 1
mainContent.Visible = true
mainContent.Parent = contentFrame

--  Settings
local settingsContent = Instance.new("Frame")
settingsContent.Size = UDim2.new(1, 0, 1, 0)
settingsContent.BackgroundTransparency = 1
settingsContent.Visible = false
settingsContent.Parent = contentFrame

 -- Bypass Speed (average) -- 
local function setupSpeedProtection()
    local getgenv, getnamecallmethod, hookmetamethod, hookfunction, newcclosure, checkcaller, gsub = 
        getgenv, getnamecallmethod, hookmetamethod, hookfunction, newcclosure, checkcaller, string.gsub
    if getgenv and getgenv().SpeedProtection then return true end
    local cloneref = cloneref or function(...) return ... end
    local clonefunction = clonefunction or function(...) return ... end
    if not (getgenv and hookmetamethod and hookfunction) then
        print("SpeedProtection não suportado")
        return false
    end
    local Players, LocalPlayer = cloneref(game:GetService("Players")), cloneref(game:GetService("Players").LocalPlayer)
    local FindFirstChild = clonefunction(game.FindFirstChild)
    local CompareInstances = function(Instance1, Instance2)
        return (typeof(Instance1) == "Instance" and typeof(Instance2) == "Instance")
    end
    local CanCastToSTDString = function(...)
        return pcall(FindFirstChild, game, ...)
    end
    getgenv().SpeedProtection = { Enabled = true, CheckCaller = true }
    local OldNamecall; OldNamecall = hookmetamethod(game, "__namecall", newcclosure(function(...)
        local self, message = ...
        local method = getnamecallmethod()  
        if ((getgenv().SpeedProtection.CheckCaller and not checkcaller()) or true) and 
           CompareInstances(self, LocalPlayer) and gsub(method, "^%l", string.upper) == "Kick" and 
           getgenv().SpeedProtection.Enabled then
            if CanCastToSTDString(message) then return end
        end
        return OldNamecall(...)
    end))
    local OldFunction; OldFunction = hookfunction(LocalPlayer.Kick, function(...)
        local self, Message = ...
        if ((getgenv().SpeedProtection.CheckCaller and not checkcaller()) or true) and 
           CompareInstances(self, LocalPlayer) and getgenv().SpeedProtection.Enabled then
            if CanCastToSTDString(Message) then return end
        end
    end)
    return true
end

local function applySpeed(speed)
    local character = player.Character
    if character and character:FindFirstChild("Humanoid") then
        character.Humanoid.WalkSpeed = speed
    end
end

-- ===== FUNÇÕES DO ESP =====
local function createESPBillboard(targetPlayer)
    if not targetPlayer or targetPlayer == player then return end
    if not espConnections[targetPlayer] then
        espConnections[targetPlayer] = {}
    end
    local playerESPData = espConnections[targetPlayer]
    local function setupESPForCharacter(character)
        if not character or not espEnabled then return end
        local head = character:FindFirstChild("Head")
        local rootPart = character:FindFirstChild("HumanoidRootPart")
        if not head or not rootPart then return end
        if playerESPData.Heartbeat then playerESPData.Heartbeat:Disconnect() end
        if playerESPData.Billboard then playerESPData.Billboard:Destroy() end
        if playerESPData.Highlight then playerESPData.Highlight:Destroy() end
        if highlights[character] then highlights[character]:Destroy() end
        playerESPData.Heartbeat = nil
        playerESPData.Billboard = nil
        playerESPData.Highlight = nil
        highlights[character] = nil
        local currentHighlight = nil
        if espConfig.highlightEnabled then
            currentHighlight = Instance.new("Highlight")
            currentHighlight.Name = "ESPHighlight"
            currentHighlight.FillColor = espConfig.customColor
            currentHighlight.FillTransparency = 0.7
            currentHighlight.OutlineColor = Color3.fromRGB(255, 255, 255)
            currentHighlight.OutlineTransparency = 0
            currentHighlight.Parent = character
            highlights[character] = currentHighlight
        end
        local billboard = nil
        local distanceLabel = nil
        local heartbeatConnection = nil
        if espConfig.showNames or espConfig.showDistance then
            billboard = Instance.new("BillboardGui")
            billboard.Name = "ESPBillboard"
            billboard.Size = UDim2.new(0, 200, 0, 50)
            billboard.StudsOffset = Vector3.new(0, 3, 0)
            billboard.AlwaysOnTop = true
            billboard.Parent = head
            local yOffset = 0
            if espConfig.showNames then
                local nameLabel = Instance.new("TextLabel")
                nameLabel.Size = UDim2.new(1, 0, 0.5, 0)
                nameLabel.Position = UDim2.new(0, 0, yOffset, 0)
                nameLabel.BackgroundTransparency = 1
                nameLabel.Text = targetPlayer.Name
                nameLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
                nameLabel.TextSize = 12
                nameLabel.Font = Enum.Font.GothamBold
                nameLabel.TextStrokeTransparency = 0
                nameLabel.TextStrokeColor3 = Color3.fromRGB(0, 0, 0)
                nameLabel.Parent = billboard
                yOffset = yOffset + 0.5
            end
            if espConfig.showDistance then
                distanceLabel = Instance.new("TextLabel")
                distanceLabel.Size = UDim2.new(1, 0, 0.5, 0)
                distanceLabel.Position = UDim2.new(0, 0, yOffset, 0)
                distanceLabel.BackgroundTransparency = 1
                distanceLabel.Text = "0m"
                distanceLabel.TextColor3 = Color3.fromRGB(100, 255, 100)
                distanceLabel.TextSize = 12
                distanceLabel.Font = Enum.Font.Gotham
                distanceLabel.TextStrokeTransparency = 0
                distanceLabel.TextStrokeColor3 = Color3.fromRGB(0, 0, 0)
                distanceLabel.Parent = billboard
            end
            if distanceLabel then
                heartbeatConnection = RunService.Heartbeat:Connect(function()
                    if not player.Character or not player.Character:FindFirstChild("HumanoidRootPart") or
                       not targetPlayer.Character or not targetPlayer.Character:FindFirstChild("HumanoidRootPart") or
                       not espEnabled then
                        return
                    end
                    local dist = (player.Character.HumanoidRootPart.Position - targetPlayer.Character.HumanoidRootPart.Position).Magnitude
                    if dist > espConfig.maxDistance then
                        billboard.Enabled = false
                        if currentHighlight then currentHighlight.Enabled = false end
                    else
                        billboard.Enabled = true
                        if currentHighlight then currentHighlight.Enabled = true end
                        distanceLabel.Text = math.floor(dist) .. "m"
                    end
                end)
            end
        end
        playerESPData.Heartbeat = heartbeatConnection
        playerESPData.Billboard = billboard
        playerESPData.Highlight = currentHighlight
        playerESPData.CurrentCharacter = character
    end
    if not playerESPData.CharacterAdded then
        playerESPData.CharacterAdded = targetPlayer.CharacterAdded:Connect(function(character)
            if espEnabled then
                character:WaitForChild("Head")
                character:WaitForChild("HumanoidRootPart")
                wait(0.5)
                setupESPForCharacter(character)
            end
        end)
    end
    if not playerESPData.CharacterRemoved then
        playerESPData.CharacterRemoved = targetPlayer.CharacterRemoving:Connect(function(character)
            if highlights[character] then
                highlights[character]:Destroy()
                highlights[character] = nil
            end
            if playerESPData.CurrentCharacter == character then
                if playerESPData.Heartbeat then playerESPData.Heartbeat:Disconnect() end
                if playerESPData.Billboard then playerESPData.Billboard:Destroy() end 
                if playerESPData.Highlight then playerESPData.Highlight:Destroy() end 
                playerESPData.Heartbeat = nil
                playerESPData.Billboard = nil
                playerESPData.Highlight = nil
                playerESPData.CurrentCharacter = nil
            end
        end)
    end
    if targetPlayer.Character then
        setupESPForCharacter(targetPlayer.Character)
    end
end

local function removeESP(targetPlayer)
    if espConnections[targetPlayer] then
        local data = espConnections[targetPlayer]
        if data.Heartbeat then data.Heartbeat:Disconnect() end
        if data.Billboard then data.Billboard:Destroy() end
        if data.Highlight then data.Highlight:Destroy() end
        if data.CharacterAdded then data.CharacterAdded:Disconnect() end
        if data.CharacterRemoved then data.CharacterRemoved:Disconnect() end        
        if data.CurrentCharacter and highlights[data.CurrentCharacter] then
            highlights[data.CurrentCharacter]:Destroy()
            highlights[data.CurrentCharacter] = nil
        end
        espConnections[targetPlayer] = nil
    end
    for character, h in pairs(highlights) do
        if character.Parent and character.Parent:IsA("Model") and Players:GetPlayerFromCharacter(character.Parent) == targetPlayer then
            h:Destroy()
            highlights[character] = nil
        end
    end
end

local function removeAllESP()
    for targetPlayer, _ in pairs(espConnections) do
        removeESP(targetPlayer)
    end
    for character, highlight in pairs(highlights) do
        highlight:Destroy()
    end
    highlights = {}
end

-- ===== ANTI-LAG =====
local function saveOriginalSettings()
    originalSettings = {
        Brightness = Lighting.Brightness,
        GlobalShadows = Lighting.GlobalShadows,
        FogEnd = Lighting.FogEnd,
        FogStart = Lighting.FogStart,
        Ambient = Lighting.Ambient,
        OutdoorAmbient = Lighting.OutdoorAmbient
    }
end

local function applyAntiLagSettings()
    saveOriginalSettings()
    pcall(function()
        -- Configurações de iluminação
        Lighting.Brightness = 4.5
        Lighting.GlobalShadows = false
        Lighting.FogEnd = 100000
        Lighting.FogStart = 0
        Lighting.Ambient = Color3.fromRGB(150, 150, 150)
        Lighting.OutdoorAmbient = Color3.fromRGB(150, 150, 150)        
        -- Remove e desativa efeitos visuais
        for _, obj in pairs(workspace:GetDescendants()) do
            pcall(function()
                -- Remove completamente
                if obj:IsA("Fire") or obj:IsA("Smoke") or obj:IsA("Explosion") or 
                   obj:IsA("Sparkles") or obj:IsA("PointLight") or obj:IsA("SpotLight") or 
                   obj:IsA("SurfaceLight") then
                    obj:Destroy()                      
                elseif obj:IsA("Decal") or obj:IsA("Texture") then
                    obj:Destroy()                          
                elseif obj:IsA("ParticleEmitter") or obj:IsA("Trail") or obj:IsA("Beam") then
                    if obj.Enabled then
                        obj.Enabled = false
                        table.insert(disabledObjects, {obj = obj, property = "Enabled", originalValue = true})
                    end
                elseif obj:IsA("MeshPart") then
                    if obj.RenderFidelity ~= Enum.RenderFidelity.Performance then
                        local original = obj.RenderFidelity
                        obj.RenderFidelity = Enum.RenderFidelity.Performance
                        table.insert(disabledObjects, {obj = obj, property = "RenderFidelity", originalValue = original})
                    end
                end
            end)
        end
        print("ANTI-LAG: Efeitos removidos e otimização aplicada")
    end)
end

local function restoreOriginalSettings()
    pcall(function()
        -- Restaura iluminação
        for property, value in pairs(originalSettings) do
            if Lighting[property] ~= nil then
                Lighting[property] = value
            end
        end   
        for _, data in pairs(disabledObjects) do
            pcall(function()
                if data.obj and data.obj.Parent then
                    data.obj[data.property] = data.originalValue
                end
            end)
        end
        disabledObjects = {} 
        print("ANTI-LAG: Configurações restauradas")
    end)
end

-- Hitbox functions 
local function createHitbox(targetPlayer)
    if not targetPlayer or targetPlayer == player or not targetPlayer.Character then return end    
    local hrp = targetPlayer.Character:FindFirstChild("HumanoidRootPart")
    if not hrp then return end   
    if hitboxParts[targetPlayer] then
        removeHitbox(targetPlayer)
    end   
    local originalData = {
        size = hrp.Size,
        transparency = hrp.Transparency,
        color = hrp.Color,
        material = hrp.Material,
        canCollide = hrp.CanCollide
    }    
    hrp.Size = Vector3.new(hitboxSize, hitboxSize, hitboxSize)
    hrp.Transparency = hitboxTransparency
    hrp.Color = Color3.fromRGB(255, 255, 255)
    hrp.Material = Enum.Material.SmoothPlastic
    hrp.CanCollide = false  
    hitboxParts[targetPlayer] = {
        originalData = originalData,
        part = hrp,
        character = targetPlayer.Character
    }
end

local function removeHitbox(targetPlayer)
    if hitboxParts[targetPlayer] then
        local data = hitboxParts[targetPlayer]      
        if data.character and data.character.Parent and data.character:FindFirstChild("HumanoidRootPart") then
            local hrp = data.character.HumanoidRootPart
            hrp.Size = data.originalData.size
            hrp.Transparency = data.originalData.transparency
            hrp.Color = data.originalData.color
            hrp.Material = data.originalData.material
            hrp.CanCollide = data.originalData.canCollide
        end    
        hitboxParts[targetPlayer] = nil
    end
end

local function createHitboxForAllPlayers()
    for _, targetPlayer in pairs(Players:GetPlayers()) do
        if targetPlayer ~= player and targetPlayer.Character and targetPlayer.Character:FindFirstChild("HumanoidRootPart") then
            createHitbox(targetPlayer)
        end
    end
end

local function removeAllHitboxes()
    for targetPlayer, _ in pairs(hitboxParts) do
        removeHitbox(targetPlayer)
    end
    hitboxParts = {}
end

local function updateHitboxSizes()
    if hitboxEnabled then
        for targetPlayer, data in pairs(hitboxParts) do
            if data.character and data.character.Parent and data.character:FindFirstChild("HumanoidRootPart") then
                local hrp = data.character.HumanoidRootPart
                hrp.Size = Vector3.new(hitboxSize, hitboxSize, hitboxSize)
                hrp.Transparency = hitboxTransparency
            end
        end
    end
end

-- Speed Option
local speedOption = Instance.new("Frame")
speedOption.Size = UDim2.new(1, -20, 0, 50)
speedOption.Position = UDim2.new(0, 10, 0, 15)
speedOption.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
speedOption.BorderSizePixel = 0
speedOption.Parent = mainContent

local speedCorner = Instance.new("UICorner")
speedCorner.CornerRadius = UDim.new(0, 8)
speedCorner.Parent = speedOption

local speedStroke = Instance.new("UIStroke")
speedStroke.Color = Color3.fromRGB(40, 40, 40)
speedStroke.Thickness = 1
speedStroke.Transparency = 0.5
speedStroke.Parent = speedOption

local speedLabel = Instance.new("TextLabel")
speedLabel.Size = UDim2.new(0, 200, 1, 0)
speedLabel.Position = UDim2.new(0, 15, 0, 0)
speedLabel.BackgroundTransparency = 1
speedLabel.Text = "Speed"
speedLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
speedLabel.TextSize = 15
speedLabel.Font = Enum.Font.GothamMedium
speedLabel.TextXAlignment = Enum.TextXAlignment.Left
speedLabel.Parent = speedOption

local toggleBg = Instance.new("Frame")
toggleBg.Size = UDim2.new(0, 50, 0, 26)
toggleBg.Position = UDim2.new(1, -65, 0.5, -13)
toggleBg.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
toggleBg.BorderSizePixel = 0
toggleBg.Parent = speedOption

local toggleBgCorner = Instance.new("UICorner")
toggleBgCorner.CornerRadius = UDim.new(1, 0)
toggleBgCorner.Parent = toggleBg

local toggleBall = Instance.new("Frame")
toggleBall.Size = UDim2.new(0, 20, 0, 20)
toggleBall.Position = UDim2.new(0, 3, 0.5, -10)
toggleBall.BackgroundColor3 = Color3.fromRGB(180, 180, 180)
toggleBall.BorderSizePixel = 0
toggleBall.Parent = toggleBg

local toggleBallCorner = Instance.new("UICorner")
toggleBallCorner.CornerRadius = UDim.new(1, 0)
toggleBallCorner.Parent = toggleBall

local toggleButton = Instance.new("TextButton")
toggleButton.Size = UDim2.new(1, 0, 1, 0)
toggleButton.BackgroundTransparency = 1
toggleButton.Text = ""
toggleButton.Parent = toggleBg

toggleButton.MouseButton1Click:Connect(function()
    speedEnabled = not speedEnabled
    local tweenInfo = TweenInfo.new(0.3, Enum.EasingStyle.Quad, Enum.EasingDirection.Out)    
    if speedEnabled then
        setupSpeedProtection()
        applySpeed(currentSpeed)
        TweenService:Create(toggleBg, tweenInfo, {BackgroundColor3 = Color3.fromRGB(70, 110, 200)}):Play()
        TweenService:Create(toggleBall, tweenInfo, {
            Position = UDim2.new(1, -23, 0.5, -10),
            BackgroundColor3 = Color3.fromRGB(255, 255, 255)
        }):Play()
        print("SPEED ATIVADO")
    else
        applySpeed(normalSpeed)
        TweenService:Create(toggleBg, tweenInfo, {BackgroundColor3 = Color3.fromRGB(40, 40, 40)}):Play()
        TweenService:Create(toggleBall, tweenInfo, {
            Position = UDim2.new(0, 3, 0.5, -10),
            BackgroundColor3 = Color3.fromRGB(180, 180, 180)
        }):Play()
        print("SPEED DESATIVADO")
    end
end)

local speedValueContainer = Instance.new("Frame")
speedValueContainer.Size = UDim2.new(1, -20, 0, 40)
speedValueContainer.Position = UDim2.new(0, 10, 0, 75)
speedValueContainer.BackgroundTransparency = 1
speedValueContainer.Parent = mainContent

local speedValueBox = Instance.new("TextBox")
speedValueBox.Size = UDim2.new(0, 120, 0, 35)
speedValueBox.Position = UDim2.new(0, 0, 0, 0)
speedValueBox.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
speedValueBox.BorderSizePixel = 0
speedValueBox.Text = tostring(currentSpeed)
speedValueBox.PlaceholderText = "Value..."
speedValueBox.TextColor3 = Color3.fromRGB(255, 255, 255)
speedValueBox.PlaceholderColor3 = Color3.fromRGB(100, 100, 100)
speedValueBox.TextSize = 14
speedValueBox.Font = Enum.Font.Gotham
speedValueBox.ClearTextOnFocus = false
speedValueBox.Parent = speedValueContainer

local valueBoxCorner = Instance.new("UICorner")
valueBoxCorner.CornerRadius = UDim.new(0, 6)
valueBoxCorner.Parent = speedValueBox

local valueBoxStroke = Instance.new("UIStroke")
valueBoxStroke.Color = Color3.fromRGB(50, 50, 50)
valueBoxStroke.Thickness = 1
valueBoxStroke.Transparency = 0.5
valueBoxStroke.Parent = speedValueBox

local applyButton = Instance.new("TextButton")
applyButton.Size = UDim2.new(0, 80, 0, 35)
applyButton.Position = UDim2.new(0, 130, 0, 0)
applyButton.BackgroundColor3 = Color3.fromRGB(60, 100, 180)
applyButton.BorderSizePixel = 0
applyButton.Text = "Apply"
applyButton.TextColor3 = Color3.fromRGB(255, 255, 255)
applyButton.TextSize = 13
applyButton.Font = Enum.Font.GothamBold
applyButton.Parent = speedValueContainer

local applyCorner = Instance.new("UICorner")
applyCorner.CornerRadius = UDim.new(0, 6)
applyCorner.Parent = applyButton

local applyStroke = Instance.new("UIStroke")
applyStroke.Color = Color3.fromRGB(80, 120, 200)
applyStroke.Thickness = 1
applyStroke.Transparency = 0.3
applyStroke.Parent = applyButton

applyButton.MouseEnter:Connect(function()
    tweenProperty(applyButton, "BackgroundColor3", Color3.fromRGB(70, 110, 200), 0.2)
end)

applyButton.MouseLeave:Connect(function()
    tweenProperty(applyButton, "BackgroundColor3", Color3.fromRGB(60, 100, 180), 0.2)
end)

applyButton.MouseButton1Click:Connect(function()
    local speedValue = tonumber(speedValueBox.Text)
    if speedValue and speedValue >= 1 and speedValue <= 200 then
        currentSpeed = speedValue
        if speedEnabled and player.Character and player.Character:FindFirstChild("Humanoid") then
            player.Character.Humanoid.WalkSpeed = currentSpeed
        end
        print("Velocidade: " .. currentSpeed)
        local originalColor = applyButton.BackgroundColor3
        TweenService:Create(applyButton, TweenInfo.new(0.1), {BackgroundColor3 = Color3.fromRGB(80, 180, 100)}):Play()
        wait(0.2)
        TweenService:Create(applyButton, TweenInfo.new(0.3), {BackgroundColor3 = originalColor}):Play()
    else
        speedValueBox.Text = tostring(currentSpeed)
        print("Valor inválido")
    end
end)

-- Highlight Option
local highlightOption = Instance.new("Frame")
highlightOption.Size = UDim2.new(1, -20, 0, 50)
highlightOption.Position = UDim2.new(0, 10, 0, 125)
highlightOption.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
highlightOption.BorderSizePixel = 0
highlightOption.Parent = mainContent

local highlightCorner = Instance.new("UICorner")
highlightCorner.CornerRadius = UDim.new(0, 8)
highlightCorner.Parent = highlightOption

local highlightStroke = Instance.new("UIStroke")
highlightStroke.Color = Color3.fromRGB(40, 40, 40)
highlightStroke.Thickness = 1
highlightStroke.Transparency = 0.5
highlightStroke.Parent = highlightOption

local highlightLabel = Instance.new("TextLabel")
highlightLabel.Size = UDim2.new(0, 200, 1, 0)
highlightLabel.Position = UDim2.new(0, 15, 0, 0)
highlightLabel.BackgroundTransparency = 1
highlightLabel.Text = "Highlight - Player"
highlightLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
highlightLabel.TextSize = 15
highlightLabel.Font = Enum.Font.GothamMedium
highlightLabel.TextXAlignment = Enum.TextXAlignment.Left
highlightLabel.Parent = highlightOption

local highlightToggleBg = Instance.new("Frame")
highlightToggleBg.Size = UDim2.new(0, 50, 0, 26)
highlightToggleBg.Position = UDim2.new(1, -65, 0.5, -13)
highlightToggleBg.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
highlightToggleBg.BorderSizePixel = 0
highlightToggleBg.Parent = highlightOption

local highlightToggleBgCorner = Instance.new("UICorner")
highlightToggleBgCorner.CornerRadius = UDim.new(1, 0)
highlightToggleBgCorner.Parent = highlightToggleBg

local highlightToggleBall = Instance.new("Frame")
highlightToggleBall.Size = UDim2.new(0, 20, 0, 20)
highlightToggleBall.Position = UDim2.new(0, 3, 0.5, -10)
highlightToggleBall.BackgroundColor3 = Color3.fromRGB(180, 180, 180)
highlightToggleBall.BorderSizePixel = 0
highlightToggleBall.Parent = highlightToggleBg

local highlightToggleBallCorner = Instance.new("UICorner")
highlightToggleBallCorner.CornerRadius = UDim.new(1, 0)
highlightToggleBallCorner.Parent = highlightToggleBall

local highlightToggleButton = Instance.new("TextButton")
highlightToggleButton.Size = UDim2.new(1, 0, 1, 0)
highlightToggleButton.BackgroundTransparency = 1
highlightToggleButton.Text = ""
highlightToggleButton.Parent = highlightToggleBg

highlightToggleButton.MouseButton1Click:Connect(function()
    espEnabled = not espEnabled
    local tweenInfo = TweenInfo.new(0.3, Enum.EasingStyle.Quad, Enum.EasingDirection.Out)    
    if espEnabled then
        for _, targetPlayer in pairs(Players:GetPlayers()) do
            if targetPlayer ~= player then
                createESPBillboard(targetPlayer)
            end
        end  
        TweenService:Create(highlightToggleBg, tweenInfo, {BackgroundColor3 = Color3.fromRGB(70, 110, 200)}):Play()
        TweenService:Create(highlightToggleBall, tweenInfo, {
            Position = UDim2.new(1, -23, 0.5, -10),
            BackgroundColor3 = Color3.fromRGB(255, 255, 255)
        }):Play()
        print("ESP ATIVADO")
    else
        removeAllESP()
        TweenService:Create(highlightToggleBg, tweenInfo, {BackgroundColor3 = Color3.fromRGB(40, 40, 40)}):Play()
        TweenService:Create(highlightToggleBall, tweenInfo, {
            Position = UDim2.new(0, 3, 0.5, -10),
            BackgroundColor3 = Color3.fromRGB(180, 180, 180)
        }):Play()
        print("ESP DESATIVADO")
    end
end)

-- Anti-lag Option
local antilagOption = Instance.new("Frame")
antilagOption.Size = UDim2.new(1, -20, 0, 50)
antilagOption.Position = UDim2.new(0, 10, 0, 185)
antilagOption.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
antilagOption.BorderSizePixel = 0
antilagOption.Parent = mainContent

local antilagCorner = Instance.new("UICorner")
antilagCorner.CornerRadius = UDim.new(0, 8)
antilagCorner.Parent = antilagOption

local antilagStroke = Instance.new("UIStroke")
antilagStroke.Color = Color3.fromRGB(40, 40, 40)
antilagStroke.Thickness = 1
antilagStroke.Transparency = 0.5
antilagStroke.Parent = antilagOption

local antilagLabel = Instance.new("TextLabel")
antilagLabel.Size = UDim2.new(0, 200, 1, 0)
antilagLabel.Position = UDim2.new(0, 15, 0, 0)
antilagLabel.BackgroundTransparency = 1
antilagLabel.Text = "Anti - lag"
antilagLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
antilagLabel.TextSize = 15
antilagLabel.Font = Enum.Font.GothamMedium
antilagLabel.TextXAlignment = Enum.TextXAlignment.Left
antilagLabel.Parent = antilagOption

local antilagToggleBg = Instance.new("Frame")
antilagToggleBg.Size = UDim2.new(0, 50, 0, 26)
antilagToggleBg.Position = UDim2.new(1, -65, 0.5, -13)
antilagToggleBg.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
antilagToggleBg.BorderSizePixel = 0
antilagToggleBg.Parent = antilagOption

local antilagToggleBgCorner = Instance.new("UICorner")
antilagToggleBgCorner.CornerRadius = UDim.new(1, 0)
antilagToggleBgCorner.Parent = antilagToggleBg

local antilagToggleBall = Instance.new("Frame")
antilagToggleBall.Size = UDim2.new(0, 20, 0, 20)
antilagToggleBall.Position = UDim2.new(0, 3, 0.5, -10)
antilagToggleBall.BackgroundColor3 = Color3.fromRGB(180, 180, 180)
antilagToggleBall.BorderSizePixel = 0
antilagToggleBall.Parent = antilagToggleBg

local antilagToggleBallCorner = Instance.new("UICorner")
antilagToggleBallCorner.CornerRadius = UDim.new(1, 0)
antilagToggleBallCorner.Parent = antilagToggleBall

local antilagToggleButton = Instance.new("TextButton")
antilagToggleButton.Size = UDim2.new(1, 0, 1, 0)
antilagToggleButton.BackgroundTransparency = 1
antilagToggleButton.Text = ""
antilagToggleButton.Parent = antilagToggleBg

antilagToggleButton.MouseButton1Click:Connect(function()
    antiLagEnabled = not antiLagEnabled
    local tweenInfo = TweenInfo.new(0.3, Enum.EasingStyle.Quad, Enum.EasingDirection.Out)    
    if antiLagEnabled then
        applyAntiLagSettings()
        TweenService:Create(antilagToggleBg, tweenInfo, {BackgroundColor3 = Color3.fromRGB(70, 110, 200)}):Play()
        TweenService:Create(antilagToggleBall, tweenInfo, {
            Position = UDim2.new(1, -23, 0.5, -10),
            BackgroundColor3 = Color3.fromRGB(255, 255, 255)
        }):Play()
        print("ANTI-LAG ATIVADO")
    else
        restoreOriginalSettings()
        TweenService:Create(antilagToggleBg, tweenInfo, {BackgroundColor3 = Color3.fromRGB(40, 40, 40)}):Play()
        TweenService:Create(antilagToggleBall, tweenInfo, {
            Position = UDim2.new(0, 3, 0.5, -10),
            BackgroundColor3 = Color3.fromRGB(180, 180, 180)
        }):Play()
        print("ANTI-LAG DESATIVADO")
    end
end)

-- Hitbox Option
local hitboxOption = Instance.new("Frame")
hitboxOption.Size = UDim2.new(1, -20, 0, 50)
hitboxOption.Position = UDim2.new(0, 10, 0, 245)
hitboxOption.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
hitboxOption.BorderSizePixel = 0
hitboxOption.Parent = mainContent

local hitboxCorner = Instance.new("UICorner")
hitboxCorner.CornerRadius = UDim.new(0, 8)
hitboxCorner.Parent = hitboxOption

local hitboxStroke = Instance.new("UIStroke")
hitboxStroke.Color = Color3.fromRGB(40, 40, 40)
hitboxStroke.Thickness = 1
hitboxStroke.Transparency = 0.5
hitboxStroke.Parent = hitboxOption

local hitboxLabel = Instance.new("TextLabel")
hitboxLabel.Size = UDim2.new(0, 200, 1, 0)
hitboxLabel.Position = UDim2.new(0, 15, 0, 0)
hitboxLabel.BackgroundTransparency = 1
hitboxLabel.Text = "Hitbox - Player"
hitboxLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
hitboxLabel.TextSize = 15
hitboxLabel.Font = Enum.Font.GothamMedium
hitboxLabel.TextXAlignment = Enum.TextXAlignment.Left
hitboxLabel.Parent = hitboxOption

local hitboxToggleBg = Instance.new("Frame")
hitboxToggleBg.Size = UDim2.new(0, 50, 0, 26)
hitboxToggleBg.Position = UDim2.new(1, -65, 0.5, -13)
hitboxToggleBg.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
hitboxToggleBg.BorderSizePixel = 0
hitboxToggleBg.Parent = hitboxOption

local hitboxToggleBgCorner = Instance.new("UICorner")
hitboxToggleBgCorner.CornerRadius = UDim.new(1, 0)
hitboxToggleBgCorner.Parent = hitboxToggleBg

local hitboxToggleBall = Instance.new("Frame")
hitboxToggleBall.Size = UDim2.new(0, 20, 0, 20)
hitboxToggleBall.Position = UDim2.new(0, 3, 0.5, -10)
hitboxToggleBall.BackgroundColor3 = Color3.fromRGB(180, 180, 180)
hitboxToggleBall.BorderSizePixel = 0
hitboxToggleBall.Parent = hitboxToggleBg

local hitboxToggleBallCorner = Instance.new("UICorner")
hitboxToggleBallCorner.CornerRadius = UDim.new(1, 0)
hitboxToggleBallCorner.Parent = hitboxToggleBall

local hitboxToggleButton = Instance.new("TextButton")
hitboxToggleButton.Size = UDim2.new(1, 0, 1, 0)
hitboxToggleButton.BackgroundTransparency = 1
hitboxToggleButton.Text = ""
hitboxToggleButton.Parent = hitboxToggleBg

hitboxToggleButton.MouseButton1Click:Connect(function()
    hitboxEnabled = not hitboxEnabled
    local tweenInfo = TweenInfo.new(0.3, Enum.EasingStyle.Quad, Enum.EasingDirection.Out)    
    if hitboxEnabled then
        createHitboxForAllPlayers()
        TweenService:Create(hitboxToggleBg, tweenInfo, {BackgroundColor3 = Color3.fromRGB(70, 110, 200)}):Play()
        TweenService:Create(hitboxToggleBall, tweenInfo, {
            Position = UDim2.new(1, -23, 0.5, -10),
            BackgroundColor3 = Color3.fromRGB(255, 255, 255)
        }):Play()
        print("HITBOX ATIVADO")
    else
        removeAllHitboxes()
        TweenService:Create(hitboxToggleBg, tweenInfo, {BackgroundColor3 = Color3.fromRGB(40, 40, 40)}):Play()
        TweenService:Create(hitboxToggleBall, tweenInfo, {
            Position = UDim2.new(0, 3, 0.5, -10),
            BackgroundColor3 = Color3.fromRGB(180, 180, 180)
        }):Play()
        print("HITBOX DESATIVADO")
    end
end)

-- Hitbox Value Container
local hitboxValueContainer = Instance.new("Frame")
hitboxValueContainer.Size = UDim2.new(1, -20, 0, 40)
hitboxValueContainer.Position = UDim2.new(0, 10, 0, 305)
hitboxValueContainer.BackgroundTransparency = 1
hitboxValueContainer.Parent = mainContent

local hitboxSizeBox = Instance.new("TextBox")
hitboxSizeBox.Size = UDim2.new(0, 80, 0, 35)
hitboxSizeBox.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
hitboxSizeBox.BorderSizePixel = 0
hitboxSizeBox.Text = tostring(hitboxSize)
hitboxSizeBox.PlaceholderText = "Tamanho"
hitboxSizeBox.TextColor3 = Color3.fromRGB(255, 255, 255)
hitboxSizeBox.PlaceholderColor3 = Color3.fromRGB(100, 100, 100)
hitboxSizeBox.TextSize = 14
hitboxSizeBox.Font = Enum.Font.Gotham
hitboxSizeBox.ClearTextOnFocus = false
hitboxSizeBox.Parent = hitboxValueContainer

local hitboxSizeBoxCorner = Instance.new("UICorner")
hitboxSizeBoxCorner.CornerRadius = UDim.new(0, 6)
hitboxSizeBoxCorner.Parent = hitboxSizeBox

local hitboxSizeBoxStroke = Instance.new("UIStroke")
hitboxSizeBoxStroke.Color = Color3.fromRGB(50, 50, 50)
hitboxSizeBoxStroke.Thickness = 1
hitboxSizeBoxStroke.Transparency = 0.5
hitboxSizeBoxStroke.Parent = hitboxSizeBox

local hitboxTransBox = Instance.new("TextBox")
hitboxTransBox.Size = UDim2.new(0, 80, 0, 35)
hitboxTransBox.Position = UDim2.new(0, 90, 0, 0)
hitboxTransBox.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
hitboxTransBox.BorderSizePixel = 0
hitboxTransBox.Text = tostring(hitboxTransparency)
hitboxTransBox.PlaceholderText = "Transp."
hitboxTransBox.TextColor3 = Color3.fromRGB(255, 255, 255)
hitboxTransBox.PlaceholderColor3 = Color3.fromRGB(100, 100, 100)
hitboxTransBox.TextSize = 14
hitboxTransBox.Font = Enum.Font.Gotham
hitboxTransBox.ClearTextOnFocus = false
hitboxTransBox.Parent = hitboxValueContainer

local hitboxTransBoxCorner = Instance.new("UICorner")
hitboxTransBoxCorner.CornerRadius = UDim.new(0, 6)
hitboxTransBoxCorner.Parent = hitboxTransBox

local hitboxTransBoxStroke = Instance.new("UIStroke")
hitboxTransBoxStroke.Color = Color3.fromRGB(50, 50, 50)
hitboxTransBoxStroke.Thickness = 1
hitboxTransBoxStroke.Transparency = 0.5
hitboxTransBoxStroke.Parent = hitboxTransBox

local hitboxApplyButton = Instance.new("TextButton")
hitboxApplyButton.Size = UDim2.new(0, 80, 0, 35)
hitboxApplyButton.Position = UDim2.new(0, 180, 0, 0)
hitboxApplyButton.BackgroundColor3 = Color3.fromRGB(60, 100, 180)
hitboxApplyButton.BorderSizePixel = 0
hitboxApplyButton.Text = "Apply"
hitboxApplyButton.TextColor3 = Color3.fromRGB(255, 255, 255)
hitboxApplyButton.TextSize = 13
hitboxApplyButton.Font = Enum.Font.GothamBold
hitboxApplyButton.Parent = hitboxValueContainer

local hitboxApplyCorner = Instance.new("UICorner")
hitboxApplyCorner.CornerRadius = UDim.new(0, 6)
hitboxApplyCorner.Parent = hitboxApplyButton

local hitboxApplyStroke = Instance.new("UIStroke")
hitboxApplyStroke.Color = Color3.fromRGB(80, 120, 200)
hitboxApplyStroke.Thickness = 1
hitboxApplyStroke.Transparency = 0.3
hitboxApplyStroke.Parent = hitboxApplyButton

hitboxApplyButton.MouseEnter:Connect(function()
    tweenProperty(hitboxApplyButton, "BackgroundColor3", Color3.fromRGB(70, 110, 200), 0.2)
end)

hitboxApplyButton.MouseLeave:Connect(function()
    tweenProperty(hitboxApplyButton, "BackgroundColor3", Color3.fromRGB(60, 100, 180), 0.2)
end)

hitboxApplyButton.MouseButton1Click:Connect(function()
    local sizeValue = tonumber(hitboxSizeBox.Text)
    local transValue = tonumber(hitboxTransBox.Text)    
    if sizeValue and sizeValue >= 5 and sizeValue <= 25 and transValue and transValue >= 0 and transValue <= 1 then
        hitboxSize = sizeValue
        hitboxTransparency = transValue
        updateHitboxSizes()
        print("Hitbox atualizada")
        local originalColor = hitboxApplyButton.BackgroundColor3
        TweenService:Create(hitboxApplyButton, TweenInfo.new(0.1), {BackgroundColor3 = Color3.fromRGB(80, 180, 100)}):Play()
        wait(0.2)
        TweenService:Create(hitboxApplyButton, TweenInfo.new(0.3), {BackgroundColor3 = originalColor}):Play()
    else
        hitboxSizeBox.Text = tostring(hitboxSize)
        hitboxTransBox.Text = tostring(hitboxTransparency)
        print("Valores inválidos")
    end
end)

-- Player Counter Display
local playerCountFrame = Instance.new("Frame")
playerCountFrame.Size = UDim2.new(1, -20, 0, 50)
playerCountFrame.Position = UDim2.new(0, 10, 0, 15)
playerCountFrame.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
playerCountFrame.BorderSizePixel = 0
playerCountFrame.Parent = settingsContent

local playerCountCorner = Instance.new("UICorner")
playerCountCorner.CornerRadius = UDim.new(0, 8)
playerCountCorner.Parent = playerCountFrame

local playerCountStroke = Instance.new("UIStroke")
playerCountStroke.Color = Color3.fromRGB(40, 40, 40)
playerCountStroke.Thickness = 1
playerCountStroke.Transparency = 0.5
playerCountStroke.Parent = playerCountFrame

local playerCountLabel = Instance.new("TextLabel")
playerCountLabel.Size = UDim2.new(0, 200, 1, 0)
playerCountLabel.Position = UDim2.new(0, 15, 0, 0)
playerCountLabel.BackgroundTransparency = 1
playerCountLabel.Text = "Players Counter :"
playerCountLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
playerCountLabel.TextSize = 15
playerCountLabel.Font = Enum.Font.GothamMedium
playerCountLabel.TextXAlignment = Enum.TextXAlignment.Left
playerCountLabel.Parent = playerCountFrame

local playerCountNumber = Instance.new("TextLabel")
playerCountNumber.Size = UDim2.new(0, 100, 1, 0)
playerCountNumber.Position = UDim2.new(1, -115, 0, 0)
playerCountNumber.BackgroundTransparency = 1
playerCountNumber.Text = tostring(#Players:GetPlayers())
playerCountNumber.TextColor3 = Color3.fromRGB(255, 255, 255)
playerCountNumber.TextSize = 18
playerCountNumber.Font = Enum.Font.GothamBold
playerCountNumber.TextXAlignment = Enum.TextXAlignment.Right
playerCountNumber.Parent = playerCountFrame

Players.PlayerAdded:Connect(function()
    playerCountNumber.Text = tostring(#Players:GetPlayers())
    local originalSize = playerCountNumber.TextSize
    TweenService:Create(playerCountNumber, TweenInfo.new(0.2), {TextSize = 22}):Play()
    wait(0.2)
    TweenService:Create(playerCountNumber, TweenInfo.new(0.2), {TextSize = originalSize}):Play()
end)

Players.PlayerRemoving:Connect(function()
    wait(0.1)
    playerCountNumber.Text = tostring(#Players:GetPlayers())
end)

local highlightTitle = Instance.new("TextLabel")
highlightTitle.Size = UDim2.new(1, -20, 0, 30)
highlightTitle.Position = UDim2.new(0, 10, 0, 80)
highlightTitle.BackgroundTransparency = 1
highlightTitle.Text = "Highlight - Player"
highlightTitle.TextColor3 = Color3.fromRGB(200, 200, 200)
highlightTitle.TextSize = 14
highlightTitle.Font = Enum.Font.GothamBold
highlightTitle.TextXAlignment = Enum.TextXAlignment.Left
highlightTitle.Parent = settingsContent

local removeNamesOption = Instance.new("Frame")
removeNamesOption.Size = UDim2.new(1, -20, 0, 45)
removeNamesOption.Position = UDim2.new(0, 10, 0, 115)
removeNamesOption.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
removeNamesOption.BorderSizePixel = 0
removeNamesOption.Parent = settingsContent

local removeNamesCorner = Instance.new("UICorner")
removeNamesCorner.CornerRadius = UDim.new(0, 8)
removeNamesCorner.Parent = removeNamesOption

local removeNamesStroke = Instance.new("UIStroke")
removeNamesStroke.Color = Color3.fromRGB(40, 40, 40)
removeNamesStroke.Thickness = 1
removeNamesStroke.Transparency = 0.5
removeNamesStroke.Parent = removeNamesOption

local removeNamesLabel = Instance.new("TextLabel")
removeNamesLabel.Size = UDim2.new(0, 280, 1, 0)
removeNamesLabel.Position = UDim2.new(0, 15, 0, 0)
removeNamesLabel.BackgroundTransparency = 1
removeNamesLabel.Text = "Retirar nomes dos Jogadores"
removeNamesLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
removeNamesLabel.TextSize = 14
removeNamesLabel.Font = Enum.Font.Gotham
removeNamesLabel.TextXAlignment = Enum.TextXAlignment.Left
removeNamesLabel.Parent = removeNamesOption

local removeNamesToggleBg = Instance.new("Frame")
removeNamesToggleBg.Size = UDim2.new(0, 50, 0, 26)
removeNamesToggleBg.Position = UDim2.new(1, -65, 0.5, -13)
removeNamesToggleBg.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
removeNamesToggleBg.BorderSizePixel = 0
removeNamesToggleBg.Parent = removeNamesOption

local removeNamesToggleBgCorner = Instance.new("UICorner")
removeNamesToggleBgCorner.CornerRadius = UDim.new(1, 0)
removeNamesToggleBgCorner.Parent = removeNamesToggleBg

local removeNamesToggleBall = Instance.new("Frame")
removeNamesToggleBall.Size = UDim2.new(0, 20, 0, 20)
removeNamesToggleBall.Position = UDim2.new(0, 3, 0.5, -10)
removeNamesToggleBall.BackgroundColor3 = Color3.fromRGB(180, 180, 180)
removeNamesToggleBall.BorderSizePixel = 0
removeNamesToggleBall.Parent = removeNamesToggleBg

local removeNamesToggleBallCorner = Instance.new("UICorner")
removeNamesToggleBallCorner.CornerRadius = UDim.new(1, 0)
removeNamesToggleBallCorner.Parent = removeNamesToggleBall

local removeNamesToggleButton = Instance.new("TextButton")
removeNamesToggleButton.Size = UDim2.new(1, 0, 1, 0)
removeNamesToggleButton.BackgroundTransparency = 1
removeNamesToggleButton.Text = ""
removeNamesToggleButton.Parent = removeNamesToggleBg

removeNamesToggleButton.MouseButton1Click:Connect(function()
    espConfig.showNames = not espConfig.showNames
    local tweenInfo = TweenInfo.new(0.3, Enum.EasingStyle.Quad, Enum.EasingDirection.Out)
    if not espConfig.showNames then
        TweenService:Create(removeNamesToggleBg, tweenInfo, {BackgroundColor3 = Color3.fromRGB(70, 110, 200)}):Play()
        TweenService:Create(removeNamesToggleBall, tweenInfo, {
            Position = UDim2.new(1, -23, 0.5, -10),
            BackgroundColor3 = Color3.fromRGB(255, 255, 255)
        }):Play()
    else
        TweenService:Create(removeNamesToggleBg, tweenInfo, {BackgroundColor3 = Color3.fromRGB(40, 40, 40)}):Play()
        TweenService:Create(removeNamesToggleBall, tweenInfo, {
            Position = UDim2.new(0, 3, 0.5, -10),
            BackgroundColor3 = Color3.fromRGB(180, 180, 180)
        }):Play()
    end
    if espEnabled then
        removeAllESP()
        for _, targetPlayer in pairs(Players:GetPlayers()) do
            if targetPlayer ~= player then
                createESPBillboard(targetPlayer)
            end
        end
    end
end)

local removeESPOption = Instance.new("Frame")
removeESPOption.Size = UDim2.new(1, -20, 0, 45)
removeESPOption.Position = UDim2.new(0, 10, 0, 170)
removeESPOption.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
removeESPOption.BorderSizePixel = 0
removeESPOption.Parent = settingsContent

local removeESPCorner = Instance.new("UICorner")
removeESPCorner.CornerRadius = UDim.new(0, 8)
removeESPCorner.Parent = removeESPOption

local removeESPStroke = Instance.new("UIStroke")
removeESPStroke.Color = Color3.fromRGB(40, 40, 40)
removeESPStroke.Thickness = 1
removeESPStroke.Transparency = 0.5
removeESPStroke.Parent = removeESPOption

local removeESPLabel = Instance.new("TextLabel")
removeESPLabel.Size = UDim2.new(0, 280, 1, 0)
removeESPLabel.Position = UDim2.new(0, 15, 0, 0)
removeESPLabel.BackgroundTransparency = 1
removeESPLabel.Text = "Retirar ESP"
removeESPLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
removeESPLabel.TextSize = 14
removeESPLabel.Font = Enum.Font.Gotham
removeESPLabel.TextXAlignment = Enum.TextXAlignment.Left
removeESPLabel.Parent = removeESPOption

local removeESPToggleBg = Instance.new("Frame")
removeESPToggleBg.Size = UDim2.new(0, 50, 0, 26)
removeESPToggleBg.Position = UDim2.new(1, -65, 0.5, -13)
removeESPToggleBg.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
removeESPToggleBg.BorderSizePixel = 0
removeESPToggleBg.Parent = removeESPOption

local removeESPToggleBgCorner = Instance.new("UICorner")
removeESPToggleBgCorner.CornerRadius = UDim.new(1, 0)
removeESPToggleBgCorner.Parent = removeESPToggleBg

local removeESPToggleBall = Instance.new("Frame")
removeESPToggleBall.Size = UDim2.new(0, 20, 0, 20)
removeESPToggleBall.Position = UDim2.new(0, 3, 0.5, -10)
removeESPToggleBall.BackgroundColor3 = Color3.fromRGB(180, 180, 180)
removeESPToggleBall.BorderSizePixel = 0
removeESPToggleBall.Parent = removeESPToggleBg

local removeESPToggleBallCorner = Instance.new("UICorner")
removeESPToggleBallCorner.CornerRadius = UDim.new(1, 0)
removeESPToggleBallCorner.Parent = removeESPToggleBall

local removeESPToggleButton = Instance.new("TextButton")
removeESPToggleButton.Size = UDim2.new(1, 0, 1, 0)
removeESPToggleButton.BackgroundTransparency = 1
removeESPToggleButton.Text = ""
removeESPToggleButton.Parent = removeESPToggleBg

removeESPToggleButton.MouseButton1Click:Connect(function()
    espConfig.highlightEnabled = not espConfig.highlightEnabled
    local tweenInfo = TweenInfo.new(0.3, Enum.EasingStyle.Quad, Enum.EasingDirection.Out)
    if not espConfig.highlightEnabled then
        TweenService:Create(removeESPToggleBg, tweenInfo, {BackgroundColor3 = Color3.fromRGB(70, 110, 200)}):Play()
        TweenService:Create(removeESPToggleBall, tweenInfo, {
            Position = UDim2.new(1, -23, 0.5, -10),
            BackgroundColor3 = Color3.fromRGB(255, 255, 255)
        }):Play()
    else
        TweenService:Create(removeESPToggleBg, tweenInfo, {BackgroundColor3 = Color3.fromRGB(40, 40, 40)}):Play()
        TweenService:Create(removeESPToggleBall, tweenInfo, {
            Position = UDim2.new(0, 3, 0.5, -10),
            BackgroundColor3 = Color3.fromRGB(180, 180, 180)
        }):Play()
    end
    if espEnabled then
        removeAllESP()
        for _, targetPlayer in pairs(Players:GetPlayers()) do
            if targetPlayer ~= player then
                createESPBillboard(targetPlayer)
            end
        end
    end
end)

local removeDistanceOption = Instance.new("Frame")
removeDistanceOption.Size = UDim2.new(1, -20, 0, 45)
removeDistanceOption.Position = UDim2.new(0, 10, 0, 225)
removeDistanceOption.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
removeDistanceOption.BorderSizePixel = 0
removeDistanceOption.Parent = settingsContent

local removeDistanceCorner = Instance.new("UICorner")
removeDistanceCorner.CornerRadius = UDim.new(0, 8)
removeDistanceCorner.Parent = removeDistanceOption

local removeDistanceStroke = Instance.new("UIStroke")
removeDistanceStroke.Color = Color3.fromRGB(40, 40, 40)
removeDistanceStroke.Thickness = 1
removeDistanceStroke.Transparency = 0.5
removeDistanceStroke.Parent = removeDistanceOption

local removeDistanceLabel = Instance.new("TextLabel")
removeDistanceLabel.Size = UDim2.new(0, 280, 1, 0)
removeDistanceLabel.Position = UDim2.new(0, 15, 0, 0)
removeDistanceLabel.BackgroundTransparency = 1
removeDistanceLabel.Text = "Retirar Números de distância"
removeDistanceLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
removeDistanceLabel.TextSize = 14
removeDistanceLabel.Font = Enum.Font.Gotham
removeDistanceLabel.TextXAlignment = Enum.TextXAlignment.Left
removeDistanceLabel.Parent = removeDistanceOption

local removeDistanceToggleBg = Instance.new("Frame")
removeDistanceToggleBg.Size = UDim2.new(0, 50, 0, 26)
removeDistanceToggleBg.Position = UDim2.new(1, -65, 0.5, -13)
removeDistanceToggleBg.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
removeDistanceToggleBg.BorderSizePixel = 0
removeDistanceToggleBg.Parent = removeDistanceOption

local removeDistanceToggleBgCorner = Instance.new("UICorner")
removeDistanceToggleBgCorner.CornerRadius = UDim.new(1, 0)
removeDistanceToggleBgCorner.Parent = removeDistanceToggleBg

local removeDistanceToggleBall = Instance.new("Frame")
removeDistanceToggleBall.Size = UDim2.new(0, 20, 0, 20)
removeDistanceToggleBall.Position = UDim2.new(0, 3, 0.5, -10)
removeDistanceToggleBall.BackgroundColor3 = Color3.fromRGB(180, 180, 180)
removeDistanceToggleBall.BorderSizePixel = 0
removeDistanceToggleBall.Parent = removeDistanceToggleBg

local removeDistanceToggleBallCorner = Instance.new("UICorner")
removeDistanceToggleBallCorner.CornerRadius = UDim.new(1, 0)
removeDistanceToggleBallCorner.Parent = removeDistanceToggleBall

local removeDistanceToggleButton = Instance.new("TextButton")
removeDistanceToggleButton.Size = UDim2.new(1, 0, 1, 0)
removeDistanceToggleButton.BackgroundTransparency = 1
removeDistanceToggleButton.Text = ""
removeDistanceToggleButton.Parent = removeDistanceToggleBg

removeDistanceToggleButton.MouseButton1Click:Connect(function()
    espConfig.showDistance = not espConfig.showDistance
    local tweenInfo = TweenInfo.new(0.3, Enum.EasingStyle.Quad, Enum.EasingDirection.Out)
    if not espConfig.showDistance then
        TweenService:Create(removeDistanceToggleBg, tweenInfo, {BackgroundColor3 = Color3.fromRGB(70, 110, 200)}):Play()
        TweenService:Create(removeDistanceToggleBall, tweenInfo, {
            Position = UDim2.new(1, -23, 0.5, -10),
            BackgroundColor3 = Color3.fromRGB(255, 255, 255)
        }):Play()
    else
        TweenService:Create(removeDistanceToggleBg, tweenInfo, {BackgroundColor3 = Color3.fromRGB(40, 40, 40)}):Play()
        TweenService:Create(removeDistanceToggleBall, tweenInfo, {
            Position = UDim2.new(0, 3, 0.5, -10),
            BackgroundColor3 = Color3.fromRGB(180, 180, 180)
        }):Play()
    end
    if espEnabled then
        removeAllESP()
        for _, targetPlayer in pairs(Players:GetPlayers()) do
            if targetPlayer ~= player then
                createESPBillboard(targetPlayer)
            end
        end
    end
end)

-- ===== FOOTER =====
local footer = Instance.new("Frame")
footer.Size = UDim2.new(1, 0, 0, 28)
footer.Position = UDim2.new(0, 0, 1, -28)
footer.BackgroundColor3 = Color3.fromRGB(15, 15, 15)
footer.BorderSizePixel = 0
footer.Parent = mainFrame

local footerCorner = Instance.new("UICorner")
footerCorner.CornerRadius = UDim.new(0, 10)
footerCorner.Parent = footer

local footerDivider = Instance.new("Frame")
footerDivider.Size = UDim2.new(1, 0, 0, 1)
footerDivider.BackgroundColor3 = Color3.fromRGB(35, 35, 35)
footerDivider.BorderSizePixel = 0
footerDivider.Parent = footer

local footerLabel = Instance.new("TextLabel")
footerLabel.Size = UDim2.new(1, -24, 1, 0)
footerLabel.Position = UDim2.new(0, 12, 0, 0)
footerLabel.BackgroundTransparency = 1
footerLabel.Text = "Ready • Main Mode"
footerLabel.TextColor3 = Color3.fromRGB(120, 120, 120)
footerLabel.TextSize = 11
footerLabel.Font = Enum.Font.Gotham
footerLabel.TextXAlignment = Enum.TextXAlignment.Left
footerLabel.Parent = footer

local isMinimized = false
local originalSize = mainFrame.Size

-- ===== Button event  =====

-- Minimize
minimizeBtn.MouseButton1Click:Connect(function()
    print("Minimizar clicado")
    isMinimized = not isMinimized
    local tweenInfo = TweenInfo.new(0.3, Enum.EasingStyle.Quad, Enum.EasingDirection.Out)    
    if isMinimized then
        local sizeTween = TweenService:Create(mainFrame, tweenInfo, {Size = UDim2.new(0, 450, 0, 45)})
        local shadowTween = TweenService:Create(shadow, tweenInfo, {Size = UDim2.new(1, 16, 0, 61)})       
        sizeTween:Play()
        shadowTween:Play()        
        wait(0.15)
        tabContainer.Visible = false
        contentFrame.Visible = false
        footer.Visible = false
    else
        tabContainer.Visible = true
        contentFrame.Visible = true
        footer.Visible = true        
        local sizeTween = TweenService:Create(mainFrame, tweenInfo, {Size = originalSize})
        local shadowTween = TweenService:Create(shadow, tweenInfo, {Size = UDim2.new(1, 16, 1, 16)})        
        sizeTween:Play()
        shadowTween:Play()
    end
end)

-- Close
closeBtn.MouseButton1Click:Connect(function()
    print("Fechar clicado")
    if espEnabled then removeAllESP() end
    if speedEnabled then speedEnabled = false; applySpeed(normalSpeed) end
    if antiLagEnabled then antiLagEnabled = false; restoreOriginalSettings() end
    if hitboxEnabled then removeAllHitboxes() end    
    local closeTweenInfo = TweenInfo.new(0.4, Enum.EasingStyle.Back, Enum.EasingDirection.In)    
    local frameTween = TweenService:Create(mainFrame, closeTweenInfo, {
        BackgroundTransparency = 1,
        Size = UDim2.new(0, 0, 0, 0),
        Position = UDim2.new(0.5, 0, 0.5, 0)
    })    
    local shadowTween = TweenService:Create(shadow, closeTweenInfo, {BackgroundTransparency = 1})    
    for _, element in pairs(mainFrame:GetDescendants()) do
        if element:IsA("GuiObject") then
            TweenService:Create(element, closeTweenInfo, {BackgroundTransparency = 1}):Play()
        end
        if element:IsA("TextLabel") or element:IsA("TextButton") then
            TweenService:Create(element, closeTweenInfo, {TextTransparency = 1}):Play()
        end
        if element:IsA("UIStroke") then
            TweenService:Create(element, closeTweenInfo, {Transparency = 1}):Play()
        end
    end    
    frameTween:Play()
    shadowTween:Play()    
    wait(0.4)
    screenGui:Destroy()
end)

-- Tab Main
mainTab.MouseButton1Click:Connect(function()
    print("Tab Main clicada")
    local mainTweenInfo = TweenInfo.new(0.4, Enum.EasingStyle.Quad, Enum.EasingDirection.Out)
    TweenService:Create(mainTab, mainTweenInfo, {BackgroundColor3 = Color3.fromRGB(30, 30, 30)}):Play()
    TweenService:Create(mainTab, mainTweenInfo, {TextColor3 = Color3.fromRGB(255, 255, 255)}):Play()
    TweenService:Create(mainTabStroke, mainTweenInfo, {Color = Color3.fromRGB(80, 120, 200), Transparency = 0}):Play()
    mainTab.Font = Enum.Font.GothamMedium    
    local settingsTweenInfo = TweenInfo.new(0.4, Enum.EasingStyle.Quad, Enum.EasingDirection.Out)
    TweenService:Create(settingsTab, settingsTweenInfo, {BackgroundColor3 = Color3.fromRGB(22, 22, 22)}):Play()
    TweenService:Create(settingsTab, settingsTweenInfo, {TextColor3 = Color3.fromRGB(140, 140, 140)}):Play()
    TweenService:Create(settingsTabStroke, settingsTweenInfo, {Color = Color3.fromRGB(40, 40, 40), Transparency = 0.5}):Play()
    settingsTab.Font = Enum.Font.Gotham  
    settingsContent.Visible = false
    mainContent.Visible = true    
    mainContent.GroupTransparency = 1
    local contentTween = TweenService:Create(mainContent, TweenInfo.new(0.5, Enum.EasingStyle.Quad, Enum.EasingDirection.Out), {GroupTransparency = 0})
    contentTween:Play()    
    contentFrame.CanvasSize = UDim2.new(0, 0, 0, 370)
    footerLabel.Text = "Ready • Main Mode"
end)

-- Tab Settings
settingsTab.MouseButton1Click:Connect(function()
    print("Tab Settings clicada")
    local settingsTweenInfo = TweenInfo.new(0.4, Enum.EasingStyle.Quad, Enum.EasingDirection.Out)
    TweenService:Create(settingsTab, settingsTweenInfo, {BackgroundColor3 = Color3.fromRGB(30, 30, 30)}):Play()
    TweenService:Create(settingsTab, settingsTweenInfo, {TextColor3 = Color3.fromRGB(255, 255, 255)}):Play()
    TweenService:Create(settingsTabStroke, settingsTweenInfo, {Color = Color3.fromRGB(150, 100, 200), Transparency = 0}):Play()
    settingsTab.Font = Enum.Font.GothamMedium  
    local mainTweenInfo = TweenInfo.new(0.4, Enum.EasingStyle.Quad, Enum.EasingDirection.Out)
    TweenService:Create(mainTab, mainTweenInfo, {BackgroundColor3 = Color3.fromRGB(22, 22, 22)}):Play()
    TweenService:Create(mainTab, mainTweenInfo, {TextColor3 = Color3.fromRGB(140, 140, 140)}):Play()
    TweenService:Create(mainTabStroke, mainTweenInfo, {Color = Color3.fromRGB(40, 40, 40), Transparency = 0.5}):Play()
    mainTab.Font = Enum.Font.Gotham    
    mainContent.Visible = false
    settingsContent.Visible = true    
    settingsContent.GroupTransparency = 1
    local contentTween = TweenService:Create(settingsContent, TweenInfo.new(0.5, Enum.EasingStyle.Quad, Enum.EasingDirection.Out), {GroupTransparency = 0})
    contentTween:Play()    
    contentFrame.CanvasSize = UDim2.new(0, 0, 0, 300)
    footerLabel.Text = "Ready • Settings Mode"
end)

-- Hover Effects
local function addHoverEffect(button, normalColor, hoverColor)
    button.MouseEnter:Connect(function()
        tweenProperty(button, "BackgroundColor3", hoverColor, 0.2)
    end)
    button.MouseLeave:Connect(function()
        tweenProperty(button, "BackgroundColor3", normalColor, 0.2)
    end)
end

addHoverEffect(minimizeBtn, Color3.fromRGB(35, 35, 35), Color3.fromRGB(50, 50, 50))
addHoverEffect(closeBtn, Color3.fromRGB(150, 40, 40), Color3.fromRGB(180, 60, 60))

-- ===== RUNSERVICE SPEED =====
RunService.Heartbeat:Connect(function()
    if speedEnabled and player.Character and player.Character:FindFirstChild("Humanoid") then
        local humanoid = player.Character.Humanoid
        if humanoid.WalkSpeed ~= currentSpeed then
            humanoid.WalkSpeed = currentSpeed
        end
    end
end)

-- ===== EVENT Players =====
Players.PlayerAdded:Connect(function(newPlayer)
    if newPlayer ~= player then
        createESPBillboard(newPlayer)
        newPlayer.CharacterAdded:Connect(function(character)
            if espEnabled then wait(0.5) end
            if hitboxEnabled then wait(0.5); createHitbox(newPlayer) end
        end)
    end
end)

Players.PlayerRemoving:Connect(function(leavingPlayer)
    if espConnections[leavingPlayer] then removeESP(leavingPlayer) end
    if hitboxParts[leavingPlayer] then removeHitbox(leavingPlayer) end
end)

player.CharacterAdded:Connect(function()
    wait(1)
    if speedEnabled then player.Character.Humanoid.WalkSpeed = currentSpeed end
    if hitboxEnabled then wait(2); createHitboxForAllPlayers() end
end)

print("loaded!")
