--[[
    ╔══════════════════════════════════════════╗
    ║     HIDDEN X HUB - STEAL A BRAINROT    ║
    ║           v1.0 | Keyless | Mobile+PC     ║
    ╚══════════════════════════════════════════╝
    Inspired by community features
    Compatible: Delta, Xeno, Wave, KRNL, Fluxus, Arceus X, Codex
]]

local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local TweenService = game:GetService("TweenService")
local UserInputService = game:GetService("UserInputService")
local TeleportService = game:GetService("TeleportService")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local LocalPlayer = Players.LocalPlayer
local Character = LocalPlayer.Character or LocalPlayer.CharacterAdded:Wait()
local Humanoid = Character:WaitForChild("Humanoid")
local HRP = Character:WaitForChild("HumanoidRootPart")
local Camera = workspace.CurrentCamera

-- ═══════════════════════════════════════════════════
-- ANTI-KICK / ANTI-CHEAT
-- ═══════════════════════════════════════════════════
local oldNamecall
oldNamecall = hookmetamethod(game, "__namecall", function(self, ...)
    local method = getnamecallmethod()
    if method == "Kick" or method == "kick" then
        warn("[HIDDEN X] Anti-Kick blocked!")
        return nil
    end
    return oldNamecall(self, ...)
end)

-- ═══════════════════════════════════════════════════
-- GUI SETUP (Dark Theme - Matching KURD HUB Style)
-- ═══════════════════════════════════════════════════
local HiddenX = Instance.new("ScreenGui")
HiddenX.Name = "HiddenXHub"
HiddenX.ResetOnSpawn = false
HiddenX.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
HiddenX.Parent = game.CoreGui

-- Main Frame
local MainFrame = Instance.new("Frame")
MainFrame.Name = "Main"
MainFrame.Size = UDim2.new(0, 500, 0, 350)
MainFrame.Position = UDim2.new(0.5, -250, 0.5, -175)
MainFrame.BackgroundColor3 = Color3.fromRGB(18, 18, 25)
MainFrame.BorderSizePixel = 0
MainFrame.Active = true
MainFrame.Draggable = true
MainFrame.Parent = HiddenX

local MainCorner = Instance.new("UICorner")
MainCorner.CornerRadius = UDim.new(0, 12)
MainCorner.Parent = MainFrame

-- Red Glow Border
local Glow = Instance.new("UIStroke")
Glow.Color = Color3.fromRGB(200, 50, 50)
Glow.Thickness = 2
Glow.Parent = MainFrame

-- Header
local Header = Instance.new("Frame")
Header.Name = "Header"
Header.Size = UDim2.new(1, 0, 0, 50)
Header.BackgroundColor3 = Color3.fromRGB(25, 25, 35)
Header.BorderSizePixel = 0
Header.Parent = MainFrame

local HeaderCorner = Instance.new("UICorner")
HeaderCorner.CornerRadius = UDim.new(0, 12)
HeaderCorner.Parent = Header

-- Logo Circle
local LogoFrame = Instance.new("Frame")
LogoFrame.Size = UDim2.new(0, 36, 0, 36)
LogoFrame.Position = UDim2.new(0, 10, 0, 7)
LogoFrame.BackgroundColor3 = Color3.fromRGB(200, 50, 50)
LogoFrame.Parent = Header

local LogoCorner = Instance.new("UICorner")
LogoCorner.CornerRadius = UDim.new(1, 0)
LogoCorner.Parent = LogoFrame

local LogoText = Instance.new("TextLabel")
LogoText.Size = UDim2.new(1, 0, 1, 0)
LogoText.BackgroundTransparency = 1
LogoText.Text = "X"
LogoText.TextColor3 = Color3.fromRGB(255, 255, 255)
LogoText.TextSize = 20
LogoText.Font = Enum.Font.GothamBlack
LogoText.Parent = LogoFrame

-- Title
local Title = Instance.new("TextLabel")
Title.Size = UDim2.new(0, 300, 0, 25)
Title.Position = UDim2.new(0, 55, 0, 5)
Title.BackgroundTransparency = 1
Title.Text = "HIDDEN X HUB - STEAL A BRAINROT"
Title.TextColor3 = Color3.fromRGB(255, 255, 255)
Title.TextSize = 16
Title.Font = Enum.Font.GothamBold
Title.TextXAlignment = Enum.TextXAlignment.Left
Title.Parent = Header

local Version = Instance.new("TextLabel")
Version.Size = UDim2.new(0, 100, 0, 18)
Version.Position = UDim2.new(0, 55, 0, 28)
Version.BackgroundTransparency = 1
Version.Text = "v1.0"
Version.TextColor3 = Color3.fromRGB(200, 50, 50)
Version.TextSize = 12
Version.Font = Enum.Font.Gotham
Version.TextXAlignment = Enum.TextXAlignment.Left
Version.Parent = Header

-- Close Button
local CloseBtn = Instance.new("TextButton")
CloseBtn.Size = UDim2.new(0, 30, 0, 30)
CloseBtn.Position = UDim2.new(1, -40, 0, 10)
CloseBtn.BackgroundColor3 = Color3.fromRGB(200, 50, 50)
CloseBtn.Text = "X"
CloseBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
CloseBtn.TextSize = 16
CloseBtn.Font = Enum.Font.GothamBold
CloseBtn.Parent = Header

local CloseCorner = Instance.new("UICorner")
CloseCorner.CornerRadius = UDim.new(0, 6)
CloseCorner.Parent = CloseBtn

CloseBtn.MouseButton1Click:Connect(function()
    HiddenX:Destroy()
end)

-- Minimize Button
local MinBtn = Instance.new("TextButton")
MinBtn.Size = UDim2.new(0, 30, 0, 30)
MinBtn.Position = UDim2.new(1, -75, 0, 10)
MinBtn.BackgroundColor3 = Color3.fromRGB(45, 45, 60)
MinBtn.Text = "-"
MinBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
MinBtn.TextSize = 18
MinBtn.Font = Enum.Font.GothamBold
MinBtn.Parent = Header

local MinCorner = Instance.new("UICorner")
MinCorner.CornerRadius = UDim.new(0, 6)
MinCorner.Parent = MinBtn

local Minimized = false
MinBtn.MouseButton1Click:Connect(function()
    Minimized = not Minimized
    MainFrame.Size = Minimized and UDim2.new(0, 500, 0, 50) or UDim2.new(0, 500, 0, 350)
end)

-- ═══════════════════════════════════════════════════
-- SIDEBAR TABS
-- ═══════════════════════════════════════════════════
local Sidebar = Instance.new("Frame")
Sidebar.Name = "Sidebar"
Sidebar.Size = UDim2.new(0, 100, 1, -50)
Sidebar.Position = UDim2.new(0, 0, 0, 50)
Sidebar.BackgroundColor3 = Color3.fromRGB(22, 22, 30)
Sidebar.BorderSizePixel = 0
Sidebar.Parent = MainFrame

local SidebarCorner = Instance.new("UICorner")
SidebarCorner.CornerRadius = UDim.new(0, 0)
SidebarCorner.Parent = Sidebar

-- Tab Buttons
local Tabs = {"Player", "Anti", "Steal", "Helper", "Server", "Credits"}
local TabFrames = {}
local CurrentTab = "Player"

local function CreateTabButton(name, index)
    local btn = Instance.new("TextButton")
    btn.Name = name.."Tab"
    btn.Size = UDim2.new(1, -10, 0, 35)
    btn.Position = UDim2.new(0, 5, 0, 10 + (index-1)*42)
    btn.BackgroundColor3 = name == CurrentTab and Color3.fromRGB(200, 50, 50) or Color3.fromRGB(35, 35, 45)
    btn.Text = name
    btn.TextColor3 = Color3.fromRGB(255, 255, 255)
    btn.TextSize = 13
    btn.Font = Enum.Font.GothamSemibold
    btn.Parent = Sidebar
    
    local corner = Instance.new("UICorner")
    corner.CornerRadius = UDim.new(0, 6)
    corner.Parent = btn
    
    -- Red indicator line
    local indicator = Instance.new("Frame")
    indicator.Size = UDim2.new(0, 3, 1, 0)
    indicator.Position = UDim2.new(0, 0, 0, 0)
    indicator.BackgroundColor3 = Color3.fromRGB(200, 50, 50)
    indicator.BorderSizePixel = 0
    indicator.Visible = name == CurrentTab
    indicator.Parent = btn
    
    return btn, indicator
end

-- ═══════════════════════════════════════════════════
-- CONTENT FRAMES
-- ═══════════════════════════════════════════════════
local ContentFrame = Instance.new("Frame")
ContentFrame.Name = "Content"
ContentFrame.Size = UDim2.new(1, -100, 1, -50)
ContentFrame.Position = UDim2.new(0, 100, 0, 50)
ContentFrame.BackgroundTransparency = 1
ContentFrame.Parent = MainFrame

for _, tabName in pairs(Tabs) do
    local frame = Instance.new("ScrollingFrame")
    frame.Name = tabName.."Content"
    frame.Size = UDim2.new(1, -10, 1, -10)
    frame.Position = UDim2.new(0, 5, 0, 5)
    frame.BackgroundTransparency = 1
    frame.ScrollBarThickness = 4
    frame.ScrollBarImageColor3 = Color3.fromRGB(200, 50, 50)
    frame.Visible = tabName == CurrentTab
    frame.AutomaticCanvasSize = Enum.AutomaticSize.Y
    frame.CanvasSize = UDim2.new(0, 0, 0, 0)
    frame.Parent = ContentFrame
    
    local grid = Instance.new("UIGridLayout")
    grid.CellSize = UDim2.new(0, 185, 0, 40)
    grid.CellPadding = UDim2.new(0, 8, 0, 8)
    grid.FillDirection = Enum.FillDirection.Horizontal
    grid.HorizontalAlignment = Enum.HorizontalAlignment.Left
    grid.VerticalAlignment = Enum.VerticalAlignment.Top
    grid.Parent = frame
    
    TabFrames[tabName] = frame
end

-- ═══════════════════════════════════════════════════
-- TOGGLE CREATOR (Red/Black Style)
-- ═══════════════════════════════════════════════════
local ActiveToggles = {}

local function CreateToggle(parent, name, callback)
    local frame = Instance.new("Frame")
    frame.Size = UDim2.new(0, 185, 0, 40)
    frame.BackgroundColor3 = Color3.fromRGB(30, 30, 40)
    frame.BorderSizePixel = 0
    frame.Parent = parent
    
    local corner = Instance.new("UICorner")
    corner.CornerRadius = UDim.new(0, 8)
    corner.Parent = frame
    
    local label = Instance.new("TextLabel")
    label.Size = UDim2.new(0, 120, 1, 0)
    label.Position = UDim2.new(0, 10, 0, 0)
    label.BackgroundTransparency = 1
    label.Text = name
    label.TextColor3 = Color3.fromRGB(200, 200, 200)
    label.TextSize = 12
    label.Font = Enum.Font.GothamSemibold
    label.TextXAlignment = Enum.TextXAlignment.Left
    label.Parent = frame
    
    -- Toggle Switch
    local switch = Instance.new("Frame")
    switch.Size = UDim2.new(0, 44, 0, 22)
    switch.Position = UDim2.new(1, -54, 0.5, -11)
    switch.BackgroundColor3 = Color3.fromRGB(45, 45, 55)
    switch.BorderSizePixel = 0
    switch.Parent = frame
    
    local switchCorner = Instance.new("UICorner")
    switchCorner.CornerRadius = UDim.new(1, 0)
    switchCorner.Parent = switch
    
    local knob = Instance.new("Frame")
    knob.Size = UDim2.new(0, 18, 0, 18)
    knob.Position = UDim2.new(0, 2, 0.5, -9)
    knob.BackgroundColor3 = Color3.fromRGB(100, 100, 110)
    knob.BorderSizePixel = 0
    knob.Parent = switch
    
    local knobCorner = Instance.new("UICorner")
    knobCorner.CornerRadius = UDim.new(1, 0)
    knobCorner.Parent = knob
    
    local enabled = false
    ActiveToggles[name] = function() return enabled end
    
    local clickArea = Instance.new("TextButton")
    clickArea.Size = UDim2.new(1, 0, 1, 0)
    clickArea.BackgroundTransparency = 1
    clickArea.Text = ""
    clickArea.Parent = frame
    
    clickArea.MouseButton1Click:Connect(function()
        enabled = not enabled
        ActiveToggles[name] = function() return enabled end
        
        -- Animate
        TweenService:Create(switch, TweenInfo.new(0.2), {
            BackgroundColor3 = enabled and Color3.fromRGB(200, 50, 50) or Color3.fromRGB(45, 45, 55)
        }):Play()
        
        TweenService:Create(knob, TweenInfo.new(0.2), {
            Position = enabled and UDim2.new(0, 24, 0.5, -9) or UDim2.new(0, 2, 0.5, -9),
            BackgroundColor3 = enabled and Color3.fromRGB(255, 255, 255) or Color3.fromRGB(100, 100, 110)
        }):Play()
        
        label.TextColor3 = enabled and Color3.fromRGB(255, 255, 255) or Color3.fromRGB(200, 200, 200)
        
        pcall(callback, enabled)
    end)
    
    return function() return enabled end
end

-- ═══════════════════════════════════════════════════
-- RUN BUTTON CREATOR
-- ═══════════════════════════════════════════════════
local function CreateRunButton(parent, name, callback)
    local frame = Instance.new("Frame")
    frame.Size = UDim2.new(0, 185, 0, 40)
    frame.BackgroundColor3 = Color3.fromRGB(30, 30, 40)
    frame.BorderSizePixel = 0
    frame.Parent = parent
    
    local corner = Instance.new("UICorner")
    corner.CornerRadius = UDim.new(0, 8)
    corner.Parent = frame
    
    local label = Instance.new("TextLabel")
    label.Size = UDim2.new(0, 100, 1, 0)
    label.Position = UDim2.new(0, 10, 0, 0)
    label.BackgroundTransparency = 1
    label.Text = name
    label.TextColor3 = Color3.fromRGB(200, 200, 200)
    label.TextSize = 12
    label.Font = Enum.Font.GothamSemibold
    label.TextXAlignment = Enum.TextXAlignment.Left
    label.Parent = frame
    
    local runBtn = Instance.new("TextButton")
    runBtn.Size = UDim2.new(0, 55, 0, 26)
    runBtn.Position = UDim2.new(1, -65, 0.5, -13)
    runBtn.BackgroundColor3 = Color3.fromRGB(200, 50, 50)
    runBtn.Text = "RUN"
    runBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
    runBtn.TextSize = 12
    runBtn.Font = Enum.Font.GothamBold
    runBtn.Parent = frame
    
    local runCorner = Instance.new("UICorner")
    runCorner.CornerRadius = UDim.new(0, 6)
    runCorner.Parent = runBtn
    
    runBtn.MouseButton1Click:Connect(function()
        TweenService:Create(runBtn, TweenInfo.new(0.1), {BackgroundColor3 = Color3.fromRGB(150, 30, 30)}):Play()
        wait(0.1)
        TweenService:Create(runBtn, TweenInfo.new(0.1), {BackgroundColor3 = Color3.fromRGB(200, 50, 50)}):Play()
        pcall(callback)
    end)
end

-- ═══════════════════════════════════════════════════
-- DROPDOWN CREATOR (For Auto Grab Target)
-- ═══════════════════════════════════════════════════
local function CreateDropdown(parent, name, options, callback)
    local frame = Instance.new("Frame")
    frame.Size = UDim2.new(0, 380, 0, 40)
    frame.BackgroundColor3 = Color3.fromRGB(30, 30, 40)
    frame.BorderSizePixel = 0
    frame.Parent = parent
    
    local corner = Instance.new("UICorner")
    corner.CornerRadius = UDim.new(0, 8)
    corner.Parent = frame
    
    local label = Instance.new("TextLabel")
    label.Size = UDim2.new(0, 150, 1, 0)
    label.Position = UDim2.new(0, 10, 0, 0)
    label.BackgroundTransparency = 1
    label.Text = name
    label.TextColor3 = Color3.fromRGB(200, 200, 200)
    label.TextSize = 12
    label.Font = Enum.Font.GothamSemibold
    label.TextXAlignment = Enum.TextXAlignment.Left
    label.Parent = frame
    
    local dropBtn = Instance.new("TextButton")
    dropBtn.Size = UDim2.new(0, 200, 0, 30)
    dropBtn.Position = UDim2.new(1, -210, 0.5, -15)
    dropBtn.BackgroundColor3 = Color3.fromRGB(45, 45, 55)
    dropBtn.Text = "Select Target ▼"
    dropBtn.TextColor3 = Color3.fromRGB(200, 200, 200)
    dropBtn.TextSize = 12
    dropBtn.Font = Enum.Font.Gotham
    dropBtn.Parent = frame
    
    local dropCorner = Instance.new("UICorner")
    dropCorner.CornerRadius = UDim.new(0, 6)
    dropCorner.Parent = dropBtn
    
    local dropdownFrame = Instance.new("Frame")
    dropdownFrame.Size = UDim2.new(0, 200, 0, 0)
    dropdownFrame.Position = UDim2.new(1, -210, 0, 35)
    dropdownFrame.BackgroundColor3 = Color3.fromRGB(35, 35, 45)
    dropdownFrame.BorderSizePixel = 0
    dropdownFrame.ClipsDescendants = true
    dropdownFrame.Visible = false
    dropdownFrame.ZIndex = 10
    dropdownFrame.Parent = frame
    
    local dropList = Instance.new("UIListLayout")
    dropList.Padding = UDim.new(0, 2)
    dropList.Parent = dropdownFrame
    
    local open = false
    
    for _, opt in pairs(options) do
        local optBtn = Instance.new("TextButton")
        optBtn.Size = UDim2.new(1, 0, 0, 28)
        optBtn.BackgroundColor3 = Color3.fromRGB(40, 40, 50)
        optBtn.Text = opt
        optBtn.TextColor3 = Color3.fromRGB(200, 200, 200)
        optBtn.TextSize = 11
        optBtn.Font = Enum.Font.Gotham
        optBtn.ZIndex = 11
        optBtn.Parent = dropdownFrame
        
        local optCorner = Instance.new("UICorner")
        optCorner.CornerRadius = UDim.new(0, 4)
        optCorner.Parent = optBtn
        
        optBtn.MouseButton1Click:Connect(function()
            dropBtn.Text = opt .. " ▼"
            dropdownFrame.Visible = false
            open = false
            pcall(callback, opt)
        end)
    end
    
    dropBtn.MouseButton1Click:Connect(function()
        open = not open
        dropdownFrame.Visible = open
        dropdownFrame.Size = open and UDim2.new(0, 200, 0, math.min(#options * 30, 150)) or UDim2.new(0, 200, 0, 0)
    end)
end

-- ═══════════════════════════════════════════════════
-- TAB SWITCHING
-- ═══════════════════════════════════════════════════
local TabButtons = {}
local TabIndicators = {}

for i, tabName in pairs(Tabs) do
    local btn, indicator = CreateTabButton(tabName, i)
    TabButtons[tabName] = btn
    TabIndicators[tabName] = indicator
    
    btn.MouseButton1Click:Connect(function()
        CurrentTab = tabName
        for _, frame in pairs(TabFrames) do
            frame.Visible = false
        end
        TabFrames[tabName].Visible = true
        
        for name, button in pairs(TabButtons) do
            button.BackgroundColor3 = name == tabName and Color3.fromRGB(200, 50, 50) or Color3.fromRGB(35, 35, 45)
            TabIndicators[name].Visible = name == tabName
        end
    end)
end

-- ═══════════════════════════════════════════════════
-- ═══════ PLAYER TAB FEATURES ══════════════════════
-- ═══════════════════════════════════════════════════

-- Player ESP
local ESPEnabled = false
local ESPFolder = Instance.new("Folder")
ESPFolder.Name = "HiddenX_ESP"
ESPFolder.Parent = HiddenX

local function CreatePlayerESP(player)
    if player == LocalPlayer then return end
    local char = player.Character
    if not char then return end
    local head = char:FindFirstChild("Head")
    if not head then return end
    
    local billboard = Instance.new("BillboardGui")
    billboard.Name = player.Name.."_ESP"
    billboard.Size = UDim2.new(0, 120, 0, 40)
    billboard.StudsOffset = Vector3.new(0, 2.5, 0)
    billboard.AlwaysOnTop = true
    billboard.Adornee = head
    billboard.Parent = ESPFolder
    
    local bg = Instance.new("Frame")
    bg.Size = UDim2.new(1, 0, 1, 0)
    bg.BackgroundColor3 = Color3.fromRGB(200, 50, 50)
    bg.BackgroundTransparency = 0.8
    bg.BorderSizePixel = 0
    bg.Parent = billboard
    
    local bgCorner = Instance.new("UICorner")
    bgCorner.CornerRadius = UDim.new(0, 4)
    bgCorner.Parent = bg
    
    local nameLabel = Instance.new("TextLabel")
    nameLabel.Size = UDim2.new(1, 0, 0.5, 0)
    nameLabel.BackgroundTransparency = 1
    nameLabel.Text = player.Name
    nameLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
    nameLabel.TextStrokeTransparency = 0
    nameLabel.TextSize = 11
    nameLabel.Font = Enum.Font.GothamBold
    nameLabel.Parent = bg
    
    local distLabel = Instance.new("TextLabel")
    distLabel.Size = UDim2.new(1, 0, 0.5, 0)
    distLabel.Position = UDim2.new(0, 0, 0.5, 0)
    distLabel.BackgroundTransparency = 1
    distLabel.Text = "0 studs"
    distLabel.TextColor3 = Color3.fromRGB(200, 200, 200)
    distLabel.TextStrokeTransparency = 0
    distLabel.TextSize = 10
    distLabel.Font = Enum.Font.Gotham
    distLabel.Parent = bg
    
    -- Update distance
    spawn(function()
        while billboard and billboard.Parent do
            if player.Character and player.Character:FindFirstChild("HumanoidRootPart") and LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart") then
                local dist = (player.Character.HumanoidRootPart.Position - LocalPlayer.Character.HumanoidRootPart.Position).Magnitude
                distLabel.Text = math.floor(dist) .. " studs"
            end
            wait(0.5)
        end
    end)
end

CreateToggle(TabFrames["Player"], "Player ESP", function(enabled)
    ESPEnabled = enabled
    if enabled then
        for _, p in pairs(Players:GetPlayers()) do
            CreatePlayerESP(p)
        end
        Players.PlayerAdded:Connect(function(p)
            if ESPEnabled then
                p.CharacterAdded:Connect(function()
                    wait(1)
                    if ESPEnabled then CreatePlayerESP(p) end
                end)
            end
        end)
    else
        ESPFolder:ClearAllChildren()
    end
end)

-- Jump Boost
CreateToggle(TabFrames["Player"], "Jump Boost", function(enabled)
    Humanoid.JumpPower = enabled and 100 or 50
end)

-- Infinite Jump
local InfJumpConn
CreateToggle(TabFrames["Player"], "Infinite Jump", function(enabled)
    if enabled then
        InfJumpConn = UserInputService.JumpRequest:Connect(function()
            if LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("Humanoid") then
                LocalPlayer.Character.Humanoid:ChangeState(Enum.HumanoidStateType.Jumping)
            end
        end)
    else
        if InfJumpConn then InfJumpConn:Disconnect() end
    end
end)

-- Auto Buy
CreateToggle(TabFrames["Player"], "Auto Buy", function(enabled)
    if enabled then
        spawn(function()
            while ActiveToggles["Auto Buy"] and ActiveToggles["Auto Buy"]() do
                -- Generic auto-buy logic - scans for buy buttons
                for _, gui in pairs(LocalPlayer.PlayerGui:GetDescendants()) do
                    if gui:IsA("TextButton") and (gui.Text:lower():find("buy") or gui.Text:lower():find("purchase")) then
                        pcall(function() gui.MouseButton1Click:Fire() end)
                    end
                end
                wait(1)
            end
        end)
    end
end)

-- Auto Kick
CreateToggle(TabFrames["Player"], "Auto Kick", function(enabled)
    -- Placeholder - would require specific game remote
    if enabled then
        game.StarterGui:SetCore("SendNotification", {
            Title = "Hidden X",
            Text = "Auto Kick requires game-specific remotes",
            Duration = 3
        })
    end
end)

-- Auto Leave
CreateToggle(TabFrames["Player"], "Auto Leave", function(enabled)
    if enabled then
        spawn(function()
            while ActiveToggles["Auto Leave"] and ActiveToggles["Auto Leave"]() do
                -- Leave if health is low or targeted
                if Humanoid.Health < 20 then
                    LocalPlayer:Kick("Auto Leave triggered - Low health")
                end
                wait(0.5)
            end
        end)
    end
end)

-- Auto Buy Fight
CreateToggle(TabFrames["Player"], "Auto Buy Fight", function(enabled)
    -- Placeholder for combat auto-buy
end)

-- AP Spammer
CreateToggle(TabFrames["Player"], "AP Spammer", function(enabled)
    if enabled then
        spawn(function()
            while ActiveToggles["AP Spammer"] and ActiveToggles["AP Spammer"]() do
                -- Spam attack pattern
                for i = 1, 5 do
                    mouse1click()
                    wait(0.05)
                end
                wait(0.3)
            end
        end)
    end
end)

-- Instant Reset
CreateToggle(TabFrames["Player"], "Instant Reset", function(enabled)
    if enabled then
        if LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("Humanoid") then
            LocalPlayer.Character.Humanoid.Health = 0
        end
    end
end)

-- AP Circle
CreateToggle(TabFrames["Player"], "AP Circle", function(enabled)
    if enabled then
        spawn(function()
            while ActiveToggles["AP Circle"] and ActiveToggles["AP Circle"]() do
                local angle = 0
                while ActiveToggles["AP Circle"] and ActiveToggles["AP Circle"]() do
                    angle = angle + 0.5
                    local x = math.cos(angle) * 10
                    local z = math.sin(angle) * 10
                    if HRP then
                        HRP.CFrame = HRP.CFrame * CFrame.new(x, 0, z)
                    end
                    wait(0.05)
                end
            end
        end)
    end
end)

-- ═══════════════════════════════════════════════════
-- ═══════ ANTI TAB FEATURES ════════════════════════
-- ═══════════════════════════════════════════════════

-- Anti Ragdoll
CreateToggle(TabFrames["Anti"], "Anti Ragdoll", function(enabled)
    if enabled then
        Humanoid:SetStateEnabled(Enum.HumanoidStateType.Physics, false)
        Humanoid:SetStateEnabled(Enum.HumanoidStateType.FallingDown, false)
        Humanoid:SetStateEnabled(Enum.HumanoidStateType.Ragdoll, false)
    else
        Humanoid:SetStateEnabled(Enum.HumanoidStateType.Physics, true)
        Humanoid:SetStateEnabled(Enum.HumanoidStateType.FallingDown, true)
        Humanoid:SetStateEnabled(Enum.HumanoidStateType.Ragdoll, true)
    end
end)

-- Anti LAG
CreateToggle(TabFrames["Anti"], "Anti LAG", function(enabled)
    if enabled then
        settings().Rendering.QualityLevel = 1
        for _, v in pairs(workspace:GetDescendants()) do
            if v:IsA("ParticleEmitter") or v:IsA("Trail") then
                v.Enabled = false
            end
            if v:IsA("BasePart") and not v:IsA("MeshPart") then
                v.Material = Enum.Material.SmoothPlastic
            end
        end
    else
        settings().Rendering.QualityLevel = 7
    end
end)

-- Anti Sentry
CreateToggle(TabFrames["Anti"], "Anti Sentry", function(enabled)
    if enabled then
        spawn(function()
            while ActiveToggles["Anti Sentry"] and ActiveToggles["Anti Sentry"]() do
                for _, obj in pairs(workspace:GetDescendants()) do
                    if obj.Name:lower():find("sentry") or obj.Name:lower():find("turret") then
                        if obj:IsA("BasePart") then
                            obj.CanTouch = false
                            obj.CanQuery = false
                        end
                    end
                end
                wait(1)
            end
        end)
    end
end)

-- Anti Bee-Disco
CreateToggle(TabFrames["Anti"], "Anti Bee-Disco", function(enabled)
    if enabled then
        spawn(function()
            while ActiveToggles["Anti Bee-Disco"] and ActiveToggles["Anti Bee-Disco"]() do
                for _, obj in pairs(workspace:GetDescendants()) do
                    if obj.Name:lower():find("bee") or obj.Name:lower():find("disco") then
                        if obj:IsA("BasePart") then
                            obj.CanTouch = false
                        end
                    end
                end
                wait(1)
            end
        end)
    end
end)

-- Anti Jumpscare
CreateToggle(TabFrames["Anti"], "Anti Jumpscare", function(enabled)
    if enabled then
        for _, gui in pairs(LocalPlayer.PlayerGui:GetDescendants()) do
            if gui:IsA("ImageLabel") or gui:IsA("Frame") then
                if gui.Name:lower():find("jumpscare") or gui.Name:lower():find("scare") then
                    gui.Visible = false
                end
            end
        end
    end
end)

-- ═══════════════════════════════════════════════════
-- ═══════ STEAL TAB FEATURES ════════════════════════
-- ═══════════════════════════════════════════════════

-- Auto Invisible
CreateToggle(TabFrames["Steal"], "Auto Invisible", function(enabled)
    if enabled then
        spawn(function()
            while ActiveToggles["Auto Invisible"] and ActiveToggles["Auto Invisible"]() do
                for _, part in pairs(Character:GetDescendants()) do
                    if part:IsA("BasePart") then
                        part.Transparency = 1
                    end
                end
                wait(0.1)
            end
            -- Restore visibility
            for _, part in pairs(Character:GetDescendants()) do
                if part:IsA("BasePart") and part.Name ~= "HumanoidRootPart" then
                    part.Transparency = 0
                end
            end
        end)
    end
end)

-- Grif Stealer
CreateToggle(TabFrames["Steal"], "Grif Stealer", function(enabled)
    -- Placeholder for specific grif stealing mechanic
end)

-- Auto Grab Target
local SelectedTarget = nil
local brainrotTargets = {
    "Bananito $15M/s",
    "Los Cucarachas $13.7M/s", 
    "Craburger $10.7M/s",
    "Berryno $9.3M/s",
    "Los Trios $6.3M/s",
    "Auto Select Best"
}

CreateDropdown(TabFrames["Steal"], "Auto Grab Target", brainrotTargets, function(selected)
    SelectedTarget = selected
    game.StarterGui:SetCore("SendNotification", {
        Title = "Hidden X",
        Text = "Target set: " .. selected,
        Duration = 2
    })
end)

-- Auto Grab All
CreateToggle(TabFrames["Steal"], "Auto Grab All", function(enabled)
    if enabled then
        spawn(function()
            while ActiveToggles["Auto Grab All"] and ActiveToggles["Auto Grab All"]() do
                for _, obj in pairs(workspace:GetDescendants()) do
                    if obj:IsA("Model") and (obj.Name:lower():find("brainrot") or obj.Name:lower():find("pet") or obj.Name:lower():find("character")) then
                        local owner = obj:FindFirstChild("Owner") or obj:FindFirstChild("OwnerValue")
                        if owner and owner.Value ~= LocalPlayer then
                            local dist = (HRP.Position - obj:GetPivot().Position).Magnitude
                            if dist < 100 then
                                HRP.CFrame = obj:GetPivot() * CFrame.new(0, 5, 5)
                                wait(0.2)
                                -- Try to interact
                                for _, child in pairs(obj:GetDescendants()) do
                                    if child:IsA("ClickDetector") then
                                        fireclickdetector(child)
                                    elseif child:IsA("ProximityPrompt") then
                                        fireproximityprompt(child)
                                    end
                                end
                            end
                        end
                    end
                end
                wait(0.5)
            end
        end)
    end
end)

-- Instant Clone
CreateToggle(TabFrames["Steal"], "Instant Clone", function(enabled)
    if enabled then
        -- Clone your own brainrots
        for _, obj in pairs(workspace:GetDescendants()) do
            local owner = obj:FindFirstChild("Owner") or obj:FindFirstChild("OwnerValue")
            if owner and owner.Value == LocalPlayer then
                local clone = obj:Clone()
                clone.Parent = workspace
                if clone:FindFirstChild("Owner") then
                    clone.Owner.Value = LocalPlayer
                end
            end
        end
    end
end)

-- Steal Floor
CreateToggle(TabFrames["Steal"], "Steal Floor", function(enabled)
    if enabled then
        spawn(function()
            while ActiveToggles["Steal Floor"] and ActiveToggles["Steal Floor"]() do
                for _, base in pairs(workspace:GetDescendants()) do
                    if base.Name:lower():find("base") or base.Name:lower():find("plot") or base.Name:lower():find("floor") then
                        local owner = base:FindFirstChild("Owner") or base:FindFirstChild("OwnerValue")
                        if owner and owner.Value ~= LocalPlayer then
                            -- Steal floor items
                            for _, item in pairs(base:GetDescendants()) do
                                if item:IsA("BasePart") and item.Name:lower():find("brainrot") then
                                    item.CFrame = HRP.CFrame * CFrame.new(0, 5, 0)
                                end
                            end
                        end
                    end
                end
                wait(1)
            end
        end)
    end
end)

-- XRay Base
CreateToggle(TabFrames["Steal"], "XRay Base", function(enabled)
    if enabled then
        for _, base in pairs(workspace:GetDescendants()) do
            if base.Name:lower():find("base") or base.Name:lower():find("plot") then
                for _, part in pairs(base:GetDescendants()) do
                    if part:IsA("BasePart") then
                        part.Transparency = 0.7
                    end
                end
            end
        end
    else
        for _, base in pairs(workspace:GetDescendants()) do
            if base.Name:lower():find("base") or base.Name:lower():find("plot") then
                for _, part in pairs(base:GetDescendants()) do
                    if part:IsA("BasePart") then
                        part.Transparency = 0
                    end
                end
            end
        end
    end
end)

-- AimBot
local AimBotConn
CreateToggle(TabFrames["Steal"], "AimBot", function(enabled)
    if enabled then
        AimBotConn = RunService.RenderStepped:Connect(function()
            local closest = nil
            local minDist = math.huge
            for _, p in pairs(Players:GetPlayers()) do
                if p ~= LocalPlayer and p.Character and p.Character:FindFirstChild("HumanoidRootPart") then
                    local dist = (p.Character.HumanoidRootPart.Position - HRP.Position).Magnitude
                    if dist < minDist and dist < 200 then
                        minDist = dist
                        closest = p.Character.HumanoidRootPart
                    end
                end
            end
            if closest then
                Camera.CFrame = CFrame.new(Camera.CFrame.Position, closest.Position)
            end
        end)
    else
        if AimBotConn then AimBotConn:Disconnect() end
    end
end)

-- Laser Stealer
CreateToggle(TabFrames["Steal"], "Laser Stealer", function(enabled)
    if enabled then
        spawn(function()
            while ActiveToggles["Laser Stealer"] and ActiveToggles["Laser Stealer"]() do
                for _, p in pairs(Players:GetPlayers()) do
                    if p ~= LocalPlayer and p.Character and p.Character:FindFirstChild("HumanoidRootPart") then
                        local dist = (p.Character.HumanoidRootPart.Position - HRP.Position).Magnitude
                        if dist < 50 then
                            -- Draw laser beam
                            local beam = Instance.new("Part")
                            beam.Anchored = true
                            beam.CanCollide = false
                            beam.Material = Enum.Material.Neon
                            beam.Color = Color3.fromRGB(200, 50, 50)
                            beam.Size = Vector3.new(0.2, 0.2, dist)
                            beam.CFrame = CFrame.lookAt(HRP.Position, p.Character.HumanoidRootPart.Position) * CFrame.new(0, 0, -dist/2)
                            beam.Parent = workspace
                            game.Debris:AddItem(beam, 0.1)
                            
                            -- Steal effect
                            HRP.CFrame = p.Character.HumanoidRootPart.CFrame * CFrame.new(0, 0, 5)
                        end
                    end
                end
                wait(0.1)
            end
        end)
    end
end)

-- ═══════════════════════════════════════════════════
-- ═══════ HELPER TAB FEATURES ══════════════════════
-- ═══════════════════════════════════════════════════

-- ESP Brainrot
local BrainrotESPFolder = Instance.new("Folder")
BrainrotESPFolder.Name = "BrainrotESP"
BrainrotESPFolder.Parent = HiddenX

CreateToggle(TabFrames["Helper"], "ESP Brainrot", function(enabled)
    if enabled then
        spawn(function()
            while ActiveToggles["ESP Brainrot"] and ActiveToggles["ESP Brainrot"]() do
                BrainrotESPFolder:ClearAllChildren()
                for _, obj in pairs(workspace:GetDescendants()) do
                    if obj:IsA("Model") and (obj.Name:lower():find("brainrot") or obj.Name:lower():find("pet")) then
                        local owner = obj:FindFirstChild("Owner") or obj:FindFirstChild("OwnerValue")
                        if owner and owner.Value ~= LocalPlayer then
                            local head = obj:FindFirstChild("Head") or obj:FindFirstChildWhichIsA("BasePart")
                            if head then
                                local billboard = Instance.new("BillboardGui")
                                billboard.Size = UDim2.new(0, 100, 0, 30)
                                billboard.StudsOffset = Vector3.new(0, 3, 0)
                                billboard.AlwaysOnTop = true
                                billboard.Adornee = head
                                billboard.Parent = BrainrotESPFolder
                                
                                local bg = Instance.new("Frame")
                                bg.Size = UDim2.new(1, 0, 1, 0)
                                bg.BackgroundColor3 = Color3.fromRGB(255, 0, 255)
                                bg.BackgroundTransparency = 0.7
                                bg.Parent = billboard
                                
                                local txt = Instance.new("TextLabel")
                                txt.Size = UDim2.new(1, 0, 1, 0)
                                txt.BackgroundTransparency = 1
                                txt.Text = obj.Name
                                txt.TextColor3 = Color3.fromRGB(255, 255, 255)
                                txt.TextStrokeTransparency = 0
                                txt.TextSize = 10
                                txt.Font = Enum.Font.GothamBold
                                txt.Parent = bg
                            end
                        end
                    end
                end
                wait(2)
            end
            BrainrotESPFolder:ClearAllChildren()
        end)
    end
end)

-- Drop Brainrot
CreateToggle(TabFrames["Helper"], "Drop Brainrot", function(enabled)
    if enabled then
        -- Drop all owned brainrots
        for _, obj in pairs(workspace:GetDescendants()) do
            local owner = obj:FindFirstChild("Owner") or obj:FindFirstChild("OwnerValue")
            if owner and owner.Value == LocalPlayer then
                if obj:IsA("Model") and (obj.Name:lower():find("brainrot") or obj.Name:lower():find("pet")) then
                    for _, part in pairs(obj:GetDescendants()) do
                        if part:IsA("BasePart") then
                            part.CFrame = HRP.CFrame * CFrame.new(math.random(-10, 10), 5, math.random(-10, 10))
                        end
                    end
                end
            end
        end
    end
end)

-- ESP Base Time
CreateToggle(TabFrames["Helper"], "ESP Base Time", function(enabled)
    if enabled then
        spawn(function()
            while ActiveToggles["ESP Base Time"] and ActiveToggles["ESP Base Time"]() do
                for _, base in pairs(workspace:GetDescendants()) do
                    if base.Name:lower():find("base") or base.Name:lower():find("plot") then
                        local timer = base:FindFirstChild("Timer") or base:FindFirstChild("Time")
                        if timer then
                            -- Show timer ESP
                        end
                    end
                end
                wait(1)
            end
        end)
    end
end)

-- Unlock Base
CreateToggle(TabFrames["Helper"], "Unlock Base", function(enabled)
    if enabled then
        for _, base in pairs(workspace:GetDescendants()) do
            if base.Name:lower():find("base") or base.Name:lower():find("plot") then
                local lock = base:FindFirstChild("Locked") or base:FindFirstChild("Lock")
                if lock then
                    lock.Value = false
                end
                for _, part in pairs(base:GetDescendants()) do
                    if part:IsA("BasePart") and part.Name:lower():find("lock") then
                        part.CanCollide = false
                        part.Transparency = 1
                    end
                end
            end
        end
    end
end)

-- Unwalk (NoClip alternative)
local UnwalkConn
CreateToggle(TabFrames["Helper"], "Unwalk", function(enabled)
    if enabled then
        UnwalkConn = RunService.Stepped:Connect(function()
            for _, part in pairs(Character:GetDescendants()) do
                if part:IsA("BasePart") then
                    part.CanCollide = false
                end
            end
        end)
    else
        if UnwalkConn then UnwalkConn:Disconnect() end
        for _, part in pairs(Character:GetDescendants()) do
            if part:IsA("BasePart") then
                part.CanCollide = true
            end
        end
    end
end)

-- ESP Stealers
CreateToggle(TabFrames["Helper"], "ESP Stealers", function(enabled)
    if enabled then
        spawn(function()
            while ActiveToggles["ESP Stealers"] and ActiveToggles["ESP Stealers"]() do
                for _, p in pairs(Players:GetPlayers()) do
                    if p ~= LocalPlayer and p.Character then
                        local head = p.Character:FindFirstChild("Head")
                        if head then
                            -- Check if they're stealing
                        end
                    end
                end
                wait(1)
            end
        end)
    end
end)

-- TP To Best
CreateToggle(TabFrames["Helper"], "TP To Best", function(enabled)
    if enabled then
        local bestValue = 0
        local bestBase = nil
        for _, base in pairs(workspace:GetDescendants()) do
            if base.Name:lower():find("base") or base.Name:lower():find("plot") then
                local value = base:FindFirstChild("Value") or base:FindFirstChild("Money")
                if value and value.Value > bestValue then
                    bestValue = value.Value
                    bestBase = base
                end
            end
        end
        if bestBase then
            local pivot = bestBase:GetPivot()
            HRP.CFrame = pivot * CFrame.new(0, 10, 0)
        end
    end
end)

-- TP ON JOIN
CreateToggle(TabFrames["Helper"], "TP ON JOIN", function(enabled)
    -- Auto teleport when joining
end)

-- ═══════════════════════════════════════════════════
-- ═══════ SERVER TAB FEATURES ══════════════════════
-- ═══════════════════════════════════════════════════

-- Rejoin
CreateRunButton(TabFrames["Server"], "Rejoin", function()
    TeleportService:Teleport(game.PlaceId, LocalPlayer)
end)

-- Server Hopper
CreateRunButton(TabFrames["Server"], "Server Hopper", function()
    local servers = {}
    local req = game:HttpGet("https://games.roblox.com/v1/games/" .. game.PlaceId .. "/servers/Public?sortOrder=Asc&limit=100")
    local data = game:GetService("HttpService"):JSONDecode(req)
    for _, server in pairs(data.data) do
        if server.playing < server.maxPlayers and server.id ~= game.JobId then
            table.insert(servers, server.id)
        end
    end
    if #servers > 0 then
        TeleportService:TeleportToPlaceInstance(game.PlaceId, servers[math.random(1, #servers)], LocalPlayer)
    end
end)

-- ═══════════════════════════════════════════════════
-- ═══════ CREDITS TAB ══════════════════════════════
-- ═══════════════════════════════════════════════════

local CreditsText = Instance.new("TextLabel")
CreditsText.Size = UDim2.new(1, -20, 0, 200)
CreditsText.Position = UDim2.new(0, 10, 0, 10)
CreditsText.BackgroundTransparency = 1
CreditsText.Text = [[
╔══════════════════════════════════╗
║      HIDDEN X HUB v1.0           ║
║                                  ║
║  Inspired by community scripts   ║
║  Built for Steal a Brainrot      ║
║                                  ║
║  Features:                       ║
║  • Auto Steal / Grab / Clone     ║
║  • ESP (Players, Brainrots)      ║
║  • Anti Ragdoll / Sentry / LAG   ║
║  • AimBot / Laser Stealer         ║
║  • Server Hopper / Rejoin        ║
║  • XRay / Unlock Base            ║
║                                  ║
║  Compatible:                     ║
║  Delta | Xeno | Wave | KRNL      ║
║  Fluxus | Arceus X | Codex       ║
║                                  ║
║  Keyless | Mobile + PC           ║
╚══════════════════════════════════╝
]]
CreditsText.TextColor3 = Color3.fromRGB(200, 200, 200)
CreditsText.TextSize = 12
CreditsText.Font = Enum.Font.Gotham
CreditsText.TextXAlignment = Enum.TextXAlignment.Left
CreditsText.TextYAlignment = Enum.TextYAlignment.Top
CreditsText.Parent = TabFrames["Credits"]

-- ═══════════════════════════════════════════════════
-- TOGGLE GUI WITH KEYBIND (RightCtrl)
-- ═══════════════════════════════════════════════════
local GuiVisible = true
UserInputService.InputBegan:Connect(function(input, gameProcessed)
    if not gameProcessed and input.KeyCode == Enum.KeyCode.RightControl then
        GuiVisible = not GuiVisible
        MainFrame.Visible = GuiVisible
    end
end)

-- ═══════════════════════════════════════════════════
-- MOBILE TOGGLE BUTTON
-- ═══════════════════════════════════════════════════
local MobileToggle = Instance.new("TextButton")
MobileToggle.Size = UDim2.new(0, 50, 0, 50)
MobileToggle.Position = UDim2.new(0, 10, 0.5, -25)
MobileToggle.BackgroundColor3 = Color3.fromRGB(200, 50, 50)
MobileToggle.Text = "X"
MobileToggle.TextColor3 = Color3.fromRGB(255, 255, 255)
MobileToggle.TextSize = 24
MobileToggle.Font = Enum.Font.GothamBlack
MobileToggle.Parent = HiddenX

local MobileCorner = Instance.new("UICorner")
MobileCorner.CornerRadius = UDim.new(1, 0)
MobileCorner.Parent = MobileToggle

MobileToggle.MouseButton1Click:Connect(function()
    GuiVisible = not GuiVisible
    MainFrame.Visible = GuiVisible
end)

-- Make mobile button draggable
local dragStart2, startPos2
MobileToggle.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.Touch then
        dragStart2 = input.Position
        startPos2 = MobileToggle.Position
    end
end)

MobileToggle.InputChanged:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.Touch and dragStart2 then
        local delta = input.Position - dragStart2
        MobileToggle.Position = UDim2.new(startPos2.X.Scale, startPos2.X.Offset + delta.X, startPos2.Y.Scale, startPos2.Y.Offset + delta.Y)
    end
end)

-- ═══════════════════════════════════════════════════
-- NOTIFICATION
-- ═══════════════════════════════════════════════════
game.StarterGui:SetCore("SendNotification", {
    Title = "🔴 HIDDEN X HUB",
    Text = "v1.0 Loaded! | RightCtrl to toggle | Mobile: Tap X",
    Duration = 5
})

print("[Hidden X Hub] Loaded successfully!")
print("[Hidden X Hub] Total features: 25+ toggles across 6 tabs")

