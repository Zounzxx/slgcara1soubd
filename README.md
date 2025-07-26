
local Players = game:GetService("Players")
local TweenService = game:GetService("TweenService")
local player = Players.LocalPlayer

local allowedLogins = {
    ["bucks"] = "1",
    ["Duo"] = "1",
    ["r3"] = "1"
}

local function createRounded(inst, radius)
    local uic = Instance.new("UICorner")
    uic.CornerRadius = UDim.new(0, radius)
    uic.Parent = inst
end

local gui = Instance.new("ScreenGui", game.CoreGui)
gui.Name = "MarketadaLoginUI"
gui.ResetOnSpawn = false

-- Tela de loading
local loadingFrame = Instance.new("Frame", gui)
loadingFrame.Size = UDim2.new(0, 400, 0, 200)
loadingFrame.Position = UDim2.new(0.5, -200, 0.5, -100)
loadingFrame.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
createRounded(loadingFrame, 10)

local logo = Instance.new("ImageLabel", loadingFrame)
logo.Size = UDim2.new(0, 100, 0, 100)
logo.Position = UDim2.new(0.5, -50, 0, 10)
logo.BackgroundTransparency = 1
logo.Image = "https://cdn.discordapp.com/attachments/1388579111877742606/1398432491810848778/marketadalogo-removebg-preview.png"

local loadingText = Instance.new("TextLabel", loadingFrame)
loadingText.Size = UDim2.new(1, 0, 0, 50)
loadingText.Position = UDim2.new(0, 0, 1, -60)
loadingText.BackgroundTransparency = 1
loadingText.Text = "Loading Marketada Bypass..."
loadingText.Font = Enum.Font.GothamBold
loadingText.TextSize = 18
loadingText.TextColor3 = Color3.fromRGB(255, 255, 255)

task.wait(2)
loadingFrame:Destroy()

-- GUI principal
local mainFrame = Instance.new("Frame", gui)
mainFrame.Size = UDim2.new(0, 400, 0, 430)
mainFrame.Position = UDim2.new(0.5, -200, 0.5, -215)
mainFrame.BackgroundColor3 = Color3.fromRGB(28, 28, 28)
mainFrame.Active = true
mainFrame.Draggable = true
createRounded(mainFrame, 10)

local titleBar = Instance.new("Frame", mainFrame)
titleBar.Size = UDim2.new(1, 0, 0, 40)
titleBar.BackgroundColor3 = Color3.fromRGB(36, 36, 36)
createRounded(titleBar, 10)

local title = Instance.new("TextLabel", titleBar)
title.Size = UDim2.new(1, 0, 1, 0)
title.BackgroundTransparency = 1
title.Text = "Marketada"
title.Font = Enum.Font.GothamBold
title.TextSize = 20
title.TextColor3 = Color3.fromRGB(255, 255, 255)

-- Tabs
local tabLogin = Instance.new("TextButton", mainFrame)
tabLogin.Size = UDim2.new(0.5, 0, 0, 30)
tabLogin.Position = UDim2.new(0, 0, 0, 50)
tabLogin.Text = "Login"
tabLogin.BackgroundColor3 = Color3.fromRGB(0, 170, 127)
tabLogin.TextColor3 = Color3.fromRGB(255, 255, 255)
tabLogin.Font = Enum.Font.GothamBold
tabLogin.TextSize = 16
createRounded(tabLogin, 6)

local tabRegister = Instance.new("TextButton", mainFrame)
tabRegister.Size = UDim2.new(0.5, 0, 0, 30)
tabRegister.Position = UDim2.new(0.5, 0, 0, 50)
tabRegister.Text = "Register"
tabRegister.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
tabRegister.TextColor3 = Color3.fromRGB(255, 255, 255)
tabRegister.Font = Enum.Font.GothamBold
tabRegister.TextSize = 16
createRounded(tabRegister, 6)

-- Input com rótulo
local function createInputWithLabel(labelText, posY)
    local label = Instance.new("TextLabel", mainFrame)
    label.Text = labelText
    label.Font = Enum.Font.Gotham
    label.TextSize = 14
    label.TextColor3 = Color3.fromRGB(200, 200, 200)
    label.BackgroundTransparency = 1
    label.Position = UDim2.new(0, 20, 0, posY)
    label.Size = UDim2.new(1, -40, 0, 20)

    local input = Instance.new("TextBox", mainFrame)
    input.Size = UDim2.new(1, -40, 0, 40)
    input.Position = UDim2.new(0, 20, 0, posY + 20)
    input.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
    input.TextColor3 = Color3.fromRGB(255, 255, 255)
    input.Font = Enum.Font.Gotham
    input.TextSize = 16
    input.Text = ""
    createRounded(input, 7)
    return input, label
end

local usernameBox, usernameLabel = createInputWithLabel("Username", 100)
local passwordBox, passwordLabel = createInputWithLabel("Password", 160)
local keyBox, keyLabel = createInputWithLabel("Key", 220)
keyBox.Visible = false
keyLabel.Visible = false

-- Notificação
local function notify(text, color)
    local n = Instance.new("TextLabel", gui)
    n.AnchorPoint = Vector2.new(1, 1)
    n.Position = UDim2.new(1, -10, 1, -10)
    n.Size = UDim2.new(0, 250, 0, 30)
    n.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
    n.BorderSizePixel = 0
    n.Text = text
    n.Font = Enum.Font.GothamBold
    n.TextColor3 = color or Color3.fromRGB(255, 255, 255)
    n.TextSize = 14
    createRounded(n, 6)
    TweenService:Create(n, TweenInfo.new(0.3), {Position = UDim2.new(1, -10, 1, -50)}):Play()
    task.delay(3, function() if n then n:Destroy() end end)
end

-- Botão
local loginButton = Instance.new("TextButton", mainFrame)
loginButton.Size = UDim2.new(1, -40, 0, 40)
loginButton.Position = UDim2.new(0, 20, 1, -60)
loginButton.Text = "Login"
loginButton.BackgroundColor3 = Color3.fromRGB(0, 170, 127)
loginButton.TextColor3 = Color3.fromRGB(255, 255, 255)
loginButton.Font = Enum.Font.GothamBold
loginButton.TextSize = 16
createRounded(loginButton, 7)

-- Troca de abas
tabLogin.MouseButton1Click:Connect(function()
    keyBox.Visible = false
    keyLabel.Visible = false
    loginButton.Text = "Login"
    tabLogin.BackgroundColor3 = Color3.fromRGB(0, 170, 127)
    tabRegister.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
end)

tabRegister.MouseButton1Click:Connect(function()
    keyBox.Visible = true
    keyLabel.Visible = true
    loginButton.Text = "Register"
    tabRegister.BackgroundColor3 = Color3.fromRGB(0, 170, 127)
    tabLogin.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
end)

-- Lógica
loginButton.MouseButton1Click:Connect(function()
    local user = usernameBox.Text
    local pass = passwordBox.Text

    if loginButton.Text == "Login" then
        if allowedLogins[user] and allowedLogins[user] == pass then
            notify("Login Sucedido!", Color3.fromRGB(100, 255, 100))
            gui:Destroy()
            loadstring(_G.marketadaReal or "")()
        else
            notify("Login incorreto!", Color3.fromRGB(255, 80, 80))
        end
    else
        if keyBox.Text == "" then
            notify("Digite uma key para registrar", Color3.fromRGB(255, 200, 0))
        else
            notify("Key inválida!", Color3.fromRGB(255, 100, 100))
        end
    end
end)


_G.marketadaReal = [==[
local runs = game:GetService("RunService")
local players = game:GetService("Players")
local uis = game:GetService("UserInputService")

local lplayer = players.LocalPlayer
local camera = workspace.CurrentCamera
local mouse = lplayer:GetMouse()

local Rayfield = loadstring(game:HttpGet('https://sirius.menu/rayfield'))()

local partcancollide = {}

for _,v in pairs(lplayer.Character:GetChildren()) do
    if v:IsA("Part") or v:IsA("BasePart") then
        partcancollide[v.Name] = v.CanCollide
    end
end

local smoothing = 1
local fov = 100
local showfov = false
local usefov = false
local aimbotteamcheck = true
local aimbotwallcheck = true
local aimbotkeybind = Enum.UserInputType.MouseButton2
local aimbotconnect = nil

local fovgui = Instance.new("ScreenGui",game.CoreGui)
fovgui.IgnoreGuiInset = true

local fovframe = Instance.new("Frame",fovgui)
fovframe.Position = UDim2.new(0.5,0,0.5,0)
fovframe.AnchorPoint = Vector2.new(0.5,0.5)
fovframe.Visible = false
fovframe.Active = false
fovframe.BackgroundTransparency = 1

local fovuic = Instance.new("UICorner",fovframe)
fovuic.CornerRadius = UDim.new(1,0)

local fovuis = Instance.new("UIStroke",fovframe)
fovuis.Color = Color3.new(1,1,1)

-- whatever godawful mess this is
local chamsoutlinecolor = Color3.new(1,1,1)
local chamsfillcolor = Color3.new(1,1,1)
local chamsoutlinetransparency = 1
local chamsfilltransparency = 0.5
local chamsconnect = nil
local occchamsconnect = nil

local boxesoutlinecolor = Color3.new(1,1,1)
local boxesfillcolor = Color3.new(1,1,1)
local boxesoutlinetransparency = 0
local boxesfilltransparency = 1
local boxesconnect = nil

local flyconnect = nil
local flyspeed = 50

local noclipconnect = nil

local triggerbotteamcheck = true
local triggerbotconnect = nil

local infammoconnect = nil

local ambientconnect = nil

local ambient = Color3.fromRGB(104,113,128)
local brightness = 2
local outdoorambient = Color3.fromRGB(104,113,128)

local atmospheredensity = 0.395
local atmosphereoffset = 0
local atmospherecolor = Color3.fromRGB(199,195,133)
local atmospheredecay = Color3.fromRGB(199,167,70)
local atmosphereglare = 0
local atmospherehaze = 0

local hrpatt = Instance.new("Attachment",lplayer.Character.HumanoidRootPart)

local version = "u-1a_041725"

-- Window
local Window = Rayfield:CreateWindow({
   Name = "Marketada",
   Icon = 0,
   LoadingTitle = "Marketada",
   LoadingSubtitle = "by bucksmatarolando",
   Theme = {
        TextColor = Color3.fromRGB(240, 240, 240),

        Background = Color3.fromRGB(36,36,36),
        Topbar = Color3.fromRGB(28,28,28),
        Shadow = Color3.fromRGB(10,10,10),

        NotificationBackground = Color3.fromRGB(36,36,36),
        NotificationActionsBackground = Color3.fromRGB(230, 230, 230),

        TabBackground = Color3.fromRGB(64,64,64),
        TabStroke = Color3.fromRGB(36,36,36),
        TabBackgroundSelected = Color3.fromRGB(210, 210, 210),
        TabTextColor = Color3.fromRGB(240, 240, 240),
        SelectedTabTextColor = Color3.fromRGB(36,36,36),

        ElementBackground = Color3.fromRGB(64,64,64),
        ElementBackgroundHover = Color3.fromRGB(40, 40, 40),
        SecondaryElementBackground = Color3.fromRGB(48,48,48),
        ElementStroke = Color3.fromRGB(48,48,48),
        SecondaryElementStroke = Color3.fromRGB(32,32,32),
                
        SliderBackground = Color3.fromRGB(220, 50, 138),
        SliderProgress = Color3.fromRGB(220, 50, 138),
        SliderStroke = Color3.fromRGB(255, 58, 163),

        ToggleBackground = Color3.fromRGB(30, 30, 30),
        ToggleEnabled = Color3.fromRGB(214, 0, 146),
        ToggleDisabled = Color3.fromRGB(100, 100, 100),
        ToggleEnabledStroke = Color3.fromRGB(255, 0, 170),
        ToggleDisabledStroke = Color3.fromRGB(125, 125, 125),
        ToggleEnabledOuterStroke = Color3.fromRGB(100, 100, 100),
        ToggleDisabledOuterStroke = Color3.fromRGB(65, 65, 65),

        DropdownSelected = Color3.fromRGB(40, 40, 40),
        DropdownUnselected = Color3.fromRGB(30, 30, 30),

        InputBackground = Color3.fromRGB(30, 30, 30),
        InputStroke = Color3.fromRGB(65, 65, 65),
        PlaceholderColor = Color3.fromRGB(178, 178, 178)
   },

   DisableRayfieldPrompts = false,
   DisableBuildWarnings = false,

   ConfigurationSaving = {
      Enabled = true,
      FolderName = "Marketada",
      FileName = "Marketada"
   },

   Discord = {
      Enabled = true,
      Invite = "https://discord.gg/NfkXp3Vvr6",
      RememberJoins = true
   },

    KeySystem = false,
    KeySettings = {
      Title = "Untitled",
      Subtitle = "Key System",
      Note = "No method of obtaining the key is provided",
      FileName = "Key",
      SaveKey = true,
      GrabKeyFromSite = false,
      Key = {"Hello"}
    }
})

-- tabs
local AimTab = Window:CreateTab("Aim", "crosshair")
local VisualTab = Window:CreateTab("ESP", "eye")
local SelfTab = Window:CreateTab("Self", "user")
local WorldTab = Window:CreateTab("World", "earth")

-- sections
-- aimbot
local AimbotSection = AimTab:CreateSection("Aimbot")
local AimbotToggle = AimTab:CreateToggle({
    Name = "Aimbot",
    CurrentValue = false,
    Flag = "Aimbot",
    Callback = function(value)
        if value then
            aimbotconnect = runs.RenderStepped:Connect(aimbot)
        else
            if aimbotconnect then
                aimbotconnect:Disconnect()
            end
        end
    end,
})


AimTab:CreateToggle({
    Name = "Silent Aim",
    CurrentValue = false,
    Flag = "silentaim",
    Callback = function(val)
        silentAimEnabled = val
    end
})

AimTab:CreateDivider()

local SmoothnessRange = AimTab:CreateSlider({
   Name = "Smoothness",
   Range = {1, 50},
   Increment = 0.1,
   Suffix = "Smoothing",
   CurrentValue = 1,
   Flag = "smoothness", 
   Callback = function(value)
      smoothing = value
   end,
})

AimTab:CreateDivider()

local aTeamCheckToggle = AimTab:CreateToggle({
    Name = "Team Check",
    CurrentValue = true,
    Flag = "AimbotTeamCheck",
    Callback = function(value)
        aimbotteamcheck = value
    end,
})

local aWallCheckToggle = AimTab:CreateToggle({
    Name = "Wall Check",
    CurrentValue = true,
    Flag = "AimbotWallCheck",
    Callback = function(value)
        aimbotwallcheck = value
    end,
})

AimTab:CreateDivider()

local ShowFovToggle = AimTab:CreateToggle({
    Name = "Show FOV",
    CurrentValue = false,
    Flag = "showfovtoggle",
    Callback = function(value)
        showfov = value
    end,
})

local UseFovToggle = AimTab:CreateToggle({
    Name = "Use FOV",
    CurrentValue = false,
    Flag = "usefovtoggle",
    Callback = function(value)
        usefov = value
    end,
})

local FovRange = AimTab:CreateSlider({
   Name = "FOV Range",
   Range = {0, 1000},
   Increment = 1,
   Suffix = "Pixels",
   CurrentValue = 100,
   Flag = "fovrange", 
   Callback = function(value)
      fov = value
   end,
})

local FovColorPicker = AimTab:CreateColorPicker({
    Name = "FOV Color",
    Color = Color3.fromRGB(255,255,255),
    Flag = "fovcolor", 
    Callback = function(value)
        fovuis.Color = value
    end
})

-- Triggerbot
local TriggerbotSection = AimTab:CreateSection("Triggerbot")
local TriggerbotToggle = AimTab:CreateToggle({
    Name = "Triggerbot",
    CurrentValue = false,
    Flag = "Triggerbot",
    Callback = function(value)
        if value then
            triggerbotconnect = runs.RenderStepped:Connect(triggerbot)
        else
            if triggerbotconnect then
                triggerbotconnect:Disconnect()
            end
        end
    end,
})

AimTab:CreateDivider()

local tTeamCheckToggle = AimTab:CreateToggle({
    Name = "Team Check",
    CurrentValue = true,
    Flag = "TriggerbotTeamCheck",
    Callback = function(value)
        triggerbotteamcheck = value
    end,
})

-- Chams
local ChamsSection = VisualTab:CreateSection("Chams")
local ChamsToggle = VisualTab:CreateToggle({
    Name = "Chams",
    CurrentValue = false,
    Flag = "chamstoggle",
    Callback = function(value)
        if value then
            chamsconnect = runs.RenderStepped:Connect(playerchams)
        else
            if chamsconnect then
                chamsconnect:Disconnect()
            end
        end
    end,
})

local OccChamsToggle = VisualTab:CreateToggle({
    Name = "Occluded Chams",
    CurrentValue = false,
    Flag = "occchamstoggle",
    Callback = function(value)
        if value then
            occchamsconnect = runs.RenderStepped:Connect(playeroccchams)
        else
            if occchamsconnect then
                occchamsconnect:Disconnect()
            end
        end
    end,
})

VisualTab:CreateDivider()

local ChamsOutlineColorPicker = VisualTab:CreateColorPicker({
    Name = "Chams Outline Color",
    Color = Color3.fromRGB(255,255,255),
    Flag = "chamsoutlinecolor", 
    Callback = function(value)
        chamsoutlinecolor = value
    end
})

local ChamsFillColorPicker = VisualTab:CreateColorPicker({
    Name = "Chams Fill Color",
    Color = Color3.fromRGB(255,255,255),
    Flag = "chamsfillcolor", 
    Callback = function(value)
        chamsfillcolor = value
    end
})

local ChamsOutlineTRange = VisualTab:CreateSlider({
   Name = "Chams Outline Transparency",
   Range = {0, 1},
   Increment = 0.01,
   Suffix = "Transparency",
   CurrentValue = 1,
   Flag = "chamsoutlinetransparency", 
   Callback = function(value)
      chamsoutlinetransparency = value
   end,
})

local ChamsFillTRange = VisualTab:CreateSlider({
   Name = "Chams Fill Transparency",
   Range = {0, 1},
   Increment = 0.01,
   Suffix = "Transparency",
   CurrentValue = 0.5,
   Flag = "chamsfilltransparency", 
   Callback = function(value)
      chamsfilltransparency = value
   end,
})

-- Boxes
local BoxesSection = VisualTab:CreateSection("Boxes")
local BoxesToggle = VisualTab:CreateToggle({
    Name = "Boxes",
    CurrentValue = false,
    Flag = "boxestoggle",
    Callback = function(value)
        if value then
            boxesconnect = runs.RenderStepped:Connect(playerbox)
        else
            if boxesconnect then
                boxesconnect:Disconnect()
            end
        end
    end,
})

VisualTab:CreateDivider()

local BoxesOutlineColorPicker = VisualTab:CreateColorPicker({
    Name = "Boxes Outline Color",
    Color = Color3.fromRGB(255,255,255),
    Flag = "boxesoutlinecolor", 
    Callback = function(value)
        boxesoutlinecolor = value
    end
})

local BoxesFillColorPicker = VisualTab:CreateColorPicker({
    Name = "Boxes Fill Color",
    Color = Color3.fromRGB(255,255,255),
    Flag = "boxesfillcolor", 
    Callback = function(value)
        boxesfillcolor = value
    end
})

local BoxesOutlineTRange = VisualTab:CreateSlider({
   Name = "Boxes Outline Transparency",
   Range = {0, 1},
   Increment = 0.01,
   Suffix = "Transparency",
   CurrentValue = 0,
   Flag = "boxesoutlinetransparency", 
   Callback = function(value)
      boxesoutlinetransparency = value
   end,
})

local BoxesFillTRange = VisualTab:CreateSlider({
   Name = "Boxes Fill Transparency",
   Range = {0, 1},
   Increment = 0.01,
   Suffix = "Transparency",
   CurrentValue = 1,
   Flag = "boxesfilltransparency", 
   Callback = function(value)
      boxesfilltransparency = value
   end,
})

-- movement
local MovementSection = SelfTab:CreateSection("Movement")
local FlyToggle = SelfTab:CreateToggle({
    Name = "Fly",
    CurrentValue = false,
    Flag = "flytoggle",
    Callback = function(value)
        if value then
            flyconnect = runs.RenderStepped:Connect(playerfly)
        else
            if flyconnect then
                flyconnect:Disconnect()
            end
        end
    end,
})

local FlyRange = SelfTab:CreateSlider({
   Name = "Fly Speed",
   Range = {0, 300},
   Increment = 1,
   Suffix = "Studs",
   CurrentValue = 50,
   Flag = "flyrange", 
   Callback = function(value)
      flyspeed = value
   end,
})

SelfTab:CreateDivider()

local NoclipToggle = SelfTab:CreateToggle({
    Name = "Noclip",
    CurrentValue = false,
    Flag = "nocliptoggle",
    Callback = function(value)
        if value then
            noclipconnect = runs.RenderStepped:Connect(playernoclip)
        else
            if noclipconnect then
                noclipconnect:Disconnect()
            end
        end
    end,
})

-- gunmods
local GunmodsSection = SelfTab:CreateSection("Gun Mods")
local InfAmmoToggle = SelfTab:CreateToggle({
    Name = "Infinite Ammo",
    CurrentValue = false,
    Flag = "infammotoggle",
    Callback = function(value)
        if value then
            infammoconnect = runs.RenderStepped:Connect(infammo)
        else
            if infammoconnect then
                infammoconnect:Disconnect()
            end
        end
    end,
})

local NoSpreadButton = SelfTab:CreateButton({
   Name = "No Spread",
   Callback = function()
      nospread()
   end,
})

local NoRecoilButton = SelfTab:CreateButton({
   Name = "No Recoil",
   Callback = function()
      norecoil()
   end,
})

local FastFireButton = SelfTab:CreateButton({
   Name = "Faster Firerate",
   Callback = function()
      fastfire()
   end,
})

local AlwaysAutoButton = SelfTab:CreateButton({
   Name = "Always Auto",
   Callback = function()
      alwaysauto()
   end,
})

local InstReloadButton = SelfTab:CreateButton({
   Name = "Instant Reload",
   Callback = function()
      instreload()
   end,
})

local IncrRangeButton = SelfTab:CreateButton({
   Name = "Increased Range",
   Callback = function()
      incrrange()
   end,
})

-- lighting
local AmbienceSection = WorldTab:CreateSection("Ambience")
local LightingToggle = WorldTab:CreateToggle({
    Name = "Override Lighting",
    CurrentValue = false,
    Flag = "lightingtoggle",
    Callback = function(value)
        if value then
            ambienceconnect = runs.RenderStepped:Connect(overrideambience)
        else
            if ambienceconnect then
                ambienceconnect:Disconnect()
            end
        end
    end,
})

WorldTab:CreateDivider()

local brightnessrange = WorldTab:CreateSlider({
   Name = "Brightness",
   Range = {0, 5},
   Increment = 0.01,
   Suffix = "Brightness",
   CurrentValue = 2,
   Flag = "brightnessrange", 
   Callback = function(value)
      brightness = value
   end,
})

local atmospheredensityrange = WorldTab:CreateSlider({
   Name = "Atmosphere Density",
   Range = {0, 1},
   Increment = 0.01,
   Suffix = "Density",
   CurrentValue = 0.395,
   Flag = "atmospheredensityrange", 
   Callback = function(value)
      atmospheredensity = value
   end,
})

local atmosphereoffsetrange = WorldTab:CreateSlider({
   Name = "Atmosphere Offset",
   Range = {0, 1},
   Increment = 0.01,
   Suffix = "Offset",
   CurrentValue = 0,
   Flag = "atmosphereoffsetrange", 
   Callback = function(value)
      atmosphereoffset = value
   end,
})

local atmosphereglarerange = WorldTab:CreateSlider({
   Name = "Atmosphere Glare",
   Range = {0, 1},
   Increment = 0.01,
   Suffix = "Glare",
   CurrentValue = 0,
   Flag = "atmosphereglarerange", 
   Callback = function(value)
      atmosphereglare = value
   end,
})

local atmospherehazerange = WorldTab:CreateSlider({
   Name = "Atmosphere Haze",
   Range = {0, 1},
   Increment = 0.01,
   Suffix = "Haze",
   CurrentValue = 0,
   Flag = "atmospherehazerange", 
   Callback = function(value)
      atmospherehaze = value
   end,
})

WorldTab:CreateDivider()

local AmbientPicker = WorldTab:CreateColorPicker({
    Name = "Ambience Color",
    Color = Color3.fromRGB(104,113,128),
    Flag = "ambientcolor", 
    Callback = function(value)
        ambient = value
    end
})

local OutdoorAmbientPicker = WorldTab:CreateColorPicker({
    Name = "Outdoor Ambience Color",
    Color = Color3.fromRGB(104,113,128),
    Flag = "outdoorambientcolor", 
    Callback = function(value)
        ourdoorambient = value
    end
})

local AtmosphereColorPicker = WorldTab:CreateColorPicker({
    Name = "Atmosphere Color",
    Color = Color3.fromRGB(199,195,133),
    Flag = "atmospherecolor", 
    Callback = function(value)
        atmospherecolor = value
    end
})

local AtmosphereDecayPicker = WorldTab:CreateColorPicker({
    Name = "Atmosphere Decay Color",
    Color = Color3.fromRGB(199,167,70),
    Flag = "atmospheredecaycolor", 
    Callback = function(value)
        atmospheredecay = value
    end
})

-- functions
function wallcheckRay(teamcheck: boolean,p: Vector3)
	if camera:FindFirstChild("Arms") then
		local params = RaycastParams.new()
		params.FilterType = Enum.RaycastFilterType.Exclude
		params:AddToFilter(workspace.Debris)
		params:AddToFilter(lplayer.Character)
		params:AddToFilter(camera.Arms)
        params.RespectCanCollide = true

		if teamcheck then
			for _,v in pairs(players:GetChildren()) do
				if v.Name ~= lplayer.Name then
					if v.TeamColor == lplayer.TeamColor then
						if v.Character then
							params:AddToFilter(v.Character)
						end
					end
				end
			end
		end

		local ray
		
		if p then
			ray = workspace:Raycast(camera.CFrame.Position,p - camera.CFrame.Position,params)
		else
			ray = workspace:Raycast(camera.CFrame.Position,camera.CFrame.LookVector*1000,params)
		end

		if ray then
			if ray.Instance.Parent:IsA("Model") then
				if players:GetPlayerFromCharacter(ray.Instance.Parent) ~= nil then
					return false
				end
			end
		end
	end
	
	return true
end

function aimbot() 
	task.spawn(function()
		if showfov then
			fovframe.Visible = true
			fovframe.Size = UDim2.new(0,fov,0,fov)

			task.wait(0.1)
			fovframe.Visible = false
		end
	end)
	
	local keybindpressed = false
	
	if aimbotkeybind == Enum.UserInputType.MouseButton1 or aimbotkeybind == Enum.UserInputType.MouseButton2 then
		keybindpressed = uis:IsMouseButtonPressed(aimbotkeybind)
	else
		keybindpressed = uis:IsKeyDown(aimbotkeybind)
	end
	
	if keybindpressed then
		local distance,position = 500,nil

		if usefov then
			distance = fov * 0.5
			
			for _,v in pairs(players:GetChildren()) do
				if v.Name ~= lplayer.Name then
					if aimbotteamcheck then
						if v.TeamColor ~= lplayer.TeamColor then
							if v.Character then
								if v.Character:FindFirstChild("Head") then
									local tpos,onscreen = camera:WorldToScreenPoint(v.Character.Head.Position)
									
									local tdist = math.huge
									
									if onscreen then
										tdist = (Vector3.new(mouse.ViewSizeX/2,mouse.ViewSizeY/2,0) - Vector3.new(tpos.X,tpos.Y,0)).Magnitude
									end
									if tdist < distance then
										if aimbotwallcheck then
											local ray = wallcheckRay(teamcheck,v.Character.Head.Position)
											if not ray then
												distance = tdist
												position = v.Character.Head.Position
											end
										else
											distance = tdist
											position = v.Character.Head.Position
										end
									end
								end
							end
						end
					else
						if v.Character then
							if v.Character:FindFirstChild("Head") then
								local tpos,onscreen = camera:WorldToScreenPoint(v.Character.Head.Position)

								local tdist = math.huge

								if onscreen then
									tdist = (Vector3.new(mouse.ViewSizeX/2,mouse.ViewSizeY/2,0) - Vector3.new(tpos.X,tpos.Y,0)).Magnitude
								end

								if tdist < distance then
									if aimbotwallcheck then
										local ray = wallcheckRay(teamcheck,v.Character.Head.Position)
										if not ray then
											distance = tdist
											position = v.Character.Head.Position
										end
									else
										distance = tdist
										position = v.Character.Head.Position
									end
								end
							end
						end
					end
				end
			end
		else
			for _,v in pairs(players:GetChildren()) do
				if v.Name ~= lplayer.Name then
					if aimbotteamcheck then
						if v.TeamColor ~= lplayer.TeamColor then
							if v.Character then
								if v.Character:FindFirstChild("Head") then
									local tdist = (camera.CFrame.Position - v.Character.Head.Position).Magnitude

									if tdist < distance then
										if aimbotwallcheck then
											local ray = wallcheckRay(teamcheck,v.Character.Head.Position)
											if not ray then
												distance = tdist
												position = v.Character.Head.Position
											end
										else
											distance = tdist
											position = v.Character.Head.Position
										end
									end
								end
							end
						end
					else
						if v.Character then
							if v.Character:FindFirstChild("Head") then
								local tdist = (camera.CFrame.Position - v.Character.Head.Position).Magnitude

								if tdist < distance then
									if aimbotwallcheck then
										local ray = wallcheckRay(teamcheck,v.Character.Head.Position)
										if not ray then
											distance = tdist
											position = v.Character.Head.Position
										end
									else
										distance = tdist
										position = v.Character.Head.Position
									end
								end
							end
						end
					end
				end
			end
		end
		
		if position then
			local destination = CFrame.new(camera.CFrame.Position,position)
			
			camera.CFrame = camera.CFrame:Lerp(destination,1/smoothing)
		end
	end
end

function triggerbot()
	local ray = wallcheckRay(triggerbotteamcheck)

	if not ray then
		mouse1press()
		task.wait(0)
		mouse1release()
	end
end

function playeroccchams()
	for _,hl in pairs(lplayer.PlayerGui:GetChildren()) do
		if hl:IsA("Highlight") then
			local exists = false
			
			for _,player in pairs(players:GetChildren()) do
				local hlname = player.Name.."hl"
				if hlname == hl.Name then
					exists = true
					break
				end
			end
			
			if not exists then
				hl:Destroy()
			end
		end
	end
	
	for _,player in pairs(players:GetChildren()) do
		if not lplayer.PlayerGui:FindFirstChild(player.Name.."hl") then
			local hl = Instance.new("Highlight",lplayer.PlayerGui)
			hl.Name = player.Name.."hl"

			hl.Adornee = player.Character
			hl.FillTransparency = 0.5
			hl.FillColor = Color3.new(1,1,1)
			hl.OutlineTransparency = 1
		else
			local hl = lplayer.PlayerGui[player.Name.."hl"]

            hl.DepthMode = Enum.HighlightDepthMode.Occluded

            hl.OutlineTransparency = chamsoutlinetransparency
            hl.OutlineColor = chamsoutlinecolor
            hl.FillTransparency = chamsfilltransparency
            hl.FillColor = chamsfillcolor
			
			if player.TeamColor == lplayer.TeamColor then
				hl.Enabled = false
			else
				hl.Enabled = true
			end
			
			task.wait()
			
			hl.Enabled = false
		end
	end
end

function playerchams()
	for _,hl in pairs(lplayer.PlayerGui:GetChildren()) do
		if hl:IsA("Highlight") then
			local exists = false

			for _,player in pairs(players:GetChildren()) do
				local hlname = player.Name.."hl"
				if hlname == hl.Name then
					exists = true
					break
				end
			end

			if not exists then
				hl:Destroy()
			end
		end
	end

	for _,player in pairs(players:GetChildren()) do
		if not lplayer.PlayerGui:FindFirstChild(player.Name.."hl") then
			local hl = Instance.new("Highlight",lplayer.PlayerGui)
			hl.Name = player.Name.."hl"

			hl.Adornee = player.Character
			hl.FillTransparency = 0.5
			hl.FillColor = Color3.new(1,1,1)
			hl.OutlineTransparency = 1
		else
			local hl = lplayer.PlayerGui[player.Name.."hl"]

            hl.DepthMode = Enum.HighlightDepthMode.AlwaysOnTop

            hl.OutlineTransparency = chamsoutlinetransparency
            hl.OutlineColor = chamsoutlinecolor
            hl.FillTransparency = chamsfilltransparency
            hl.FillColor = chamsfillcolor

			if player.TeamColor == lplayer.TeamColor then
				hl.Enabled = false
			else
				hl.Enabled = true
			end

			task.wait()

			hl.Enabled = false
		end
	end
end

function playerbox()
	for _,player in pairs(players:GetChildren()) do
		if player.Character then
			if not player.Character:FindFirstChild(player.Name.."bg") then
				local bg = Instance.new("BillboardGui",player.Character)
				bg.Name = player.Name.."bg"

				bg.AlwaysOnTop = true
				bg.Size = UDim2.new(2,0,4,0)

				local box = Instance.new("Frame",bg)
				box.BackgroundTransparency = 1
				box.Size = UDim2.new(1,0,1,0)
				
				local boxuis = Instance.new("UIStroke",box)
				boxuis.Color = Color3.new(1,1,1)
			else
				local bg = player.Character[player.Name.."bg"]

                bg.Frame.UIStroke.Transparency = boxesoutlinetransparency
                bg.Frame.UIStroke.Color = boxesoutlinecolor
                bg.Frame.BackgroundTransparency = boxesfilltransparency
                bg.Frame.BackgroundColor3 = boxesfillcolor


				if player.TeamColor == lplayer.TeamColor then
					bg.Frame.UIStroke.Enabled = false
				else
					bg.Frame.UIStroke.Enabled = true
				end

				task.wait()

				bg.Frame.UIStroke.Enabled = false
			end
		end
	end
end

function playerfly()
    local linearvelocity = Instance.new("LinearVelocity",lplayer.Character)
    linearvelocity.VelocityConstraintMode = Enum.VelocityConstraintMode.Vector
    linearvelocity.MaxForce = math.huge

    local rotation = camera.CFrame

    for _,v in pairs(lplayer.Character:GetChildren()) do
        if v:IsA("Part") or v:IsA("BasePart") then
            v.CanCollide = false
        end
    end

    linearvelocity.Attachment0 = hrpatt
    
    if (uis:IsKeyDown(Enum.KeyCode.W)) then
        linearvelocity.VectorVelocity += rotation.LookVector * flyspeed
    end

    if (uis:IsKeyDown(Enum.KeyCode.S)) then
        linearvelocity.VectorVelocity += rotation.LookVector * -flyspeed
    end

    if (uis:IsKeyDown(Enum.KeyCode.A)) then
        linearvelocity.VectorVelocity += rotation.RightVector * -flyspeed
    end

    if (uis:IsKeyDown(Enum.KeyCode.D)) then
        linearvelocity.VectorVelocity += rotation.RightVector * flyspeed
    end

    if (uis:IsKeyDown(Enum.KeyCode.LeftShift)) then
        linearvelocity.VectorVelocity += CFrame.new().UpVector * -flyspeed
    end

    if (uis:IsKeyDown(Enum.KeyCode.Space)) then
        linearvelocity.VectorVelocity += CFrame.new().UpVector * flyspeed
    end

    task.wait(0)

    linearvelocity:Destroy()

    for _,v in pairs(lplayer.Character:GetChildren()) do
        if v:IsA("Part") or v:IsA("BasePart") then
            v.CanCollide = partcancollide[v.Name]
        end
    end
end

function playernoclip()
    for _,v in pairs(lplayer.Character:GetChildren()) do
        if v:IsA("Part") or v:IsA("BasePart") then
            v.CanCollide = false
        end
    end

    task.wait(0)

    for _,v in pairs(lplayer.Character:GetChildren()) do
        if v:IsA("Part") or v:IsA("BasePart") then
            v.CanCollide = partcancollide[v.Name]
        end
    end
end

function infammo()
	lplayer.PlayerGui.GUI.Client.Variables.ammocount.Value = 999
end

function nospread()
	lplayer.PlayerGui.GUI.Client.Variables.currentspread.Value = 0
end

function norecoil()
	for _,v in pairs(game:GetService("ReplicatedStorage").Weapons:GetChildren()) do
		if v:FindFirstChild("RecoilControl") then
			v.RecoilControl.Value = 0
		end
	end
end

function instreload()
	for _,v in pairs(game:GetService("ReplicatedStorage").Weapons:GetChildren()) do
		if v:FindFirstChild("ReloadTime") then
			v.ReloadTime.Value = 0
		end
	end
end

function fastfire()
	for _,v in pairs(game:GetService("ReplicatedStorage").Weapons:GetChildren()) do
		if v:FindFirstChild("FireRate") then
			v.FireRate.Value = 0.02
		end
	end
end

function overridespeed()
	local speed = getrange("Speed Modifier")
	
	for _,v in pairs(game:GetService("ReplicatedStorage").Weapons:GetChildren()) do
		if v:FindFirstChild("Speed%") then
			v:FindFirstChild("Speed%").Value = -speed
		end
	end
end

function incrrange()
	for _,v in pairs(game:GetService("ReplicatedStorage").Weapons:GetChildren()) do
		if v:FindFirstChild("EquipTime") then
			v.Range.Value = 10000
		end
	end
end

function alwaysauto()
	for _,v in pairs(game:GetService("ReplicatedStorage").Weapons:GetChildren()) do
		if v:FindFirstChild("Auto") then
			v.Auto.Value = true
		end
	end
end

function overrideambience()
    game.Lighting.Ambient = ambient
    game.Lighting.Brightness = brightness
    game.Lighting.OutdoorAmbient = outdoorambient

    if not game.Lighting:FindFirstChild("Atmosphere") then
        Instance.new("Atmosphere",game.Lighting)
    else
        game.Lighting.Atmosphere.Density = atmospheredensity
        game.Lighting.Atmosphere.Offset = atmosphereoffset
        game.Lighting.Atmosphere.Color = atmospherecolor
        game.Lighting.Atmosphere.Decay = atmospheredecay
        game.Lighting.Atmosphere.Glare = atmosphereglare
        game.Lighting.Atmosphere.Haze = atmospherehaze
    end
end

Rayfield:LoadConfiguration()

smoothing = SmoothnessRange.CurrentValue
fov = FovRange.CurrentValue
showfov = ShowFovToggle.CurrentValue
usefov = UseFovToggle.CurrentValue
aimbotteamcheck = aTeamCheckToggle.CurrentValue
aimbotwallcheck = aWallCheckToggle.CurrentValue
fovuis.Color = FovColorPicker.Color

chamsoutlinecolor = ChamsOutlineColorPicker.Color
chamsfillcolor = ChamsFillColorPicker.Color
chamsoutlinetransparency = ChamsOutlineTRange.CurrentValue
chamsfilltransparency = ChamsFillTRange.CurrentValue

boxesoutlinecolor = BoxesOutlineColorPicker.Color
boxesfillcolor = BoxesFillColorPicker.Color
boxesoutlinetransparency = BoxesOutlineTRange.CurrentValue
boxesfilltransparency = BoxesFillTRange.CurrentValue
flyspeed = FlyRange.CurrentValue

triggerbotteamcheck = tTeamCheckToggle.CurrentValue

ambient = AmbientPicker.Color
brightness = brightnessrange.CurrentValue
outdoorambient = OutdoorAmbientPicker.Color

atmospheredensity = atmospheredensityrange.CurrentValue
atmosphereoffset = atmosphereoffsetrange.CurrentValue
atmospherecolor = AtmosphereColorPicker.Color
atmospheredecay = AtmosphereDecayPicker.Color
atmosphereglare = atmosphereglarerange.CurrentValue
atmospherehaze = atmospherehazerange.CurrentValue

-- TAB: Players
local PlayersTab = Window:CreateTab("Players", "user")
local playerNames = {}
for _, p in ipairs(players:GetPlayers()) do
    if p ~= lplayer then table.insert(playerNames, p.Name) end
end

local selectedPlayer = nil

PlayersTab:CreateDropdown({
    Name = "Selecionar Jogador",
    Options = playerNames,
    CurrentOption = "",
    Callback = function(nome)
        selectedPlayer = players:FindFirstChild(nome)
    end
})

PlayersTab:CreateButton({
    Name = "Teleportar até Jogador",
    Callback = function()
        if selectedPlayer and selectedPlayer.Character and selectedPlayer.Character:FindFirstChild("HumanoidRootPart") then
            lplayer.Character:MoveTo(selectedPlayer.Character.HumanoidRootPart.Position + Vector3.new(0, 5, 0))
        end
    end
})

PlayersTab:CreateButton({
    Name = "Puxar Jogador",
    Callback = function()
        if selectedPlayer and selectedPlayer.Character and selectedPlayer.Character:FindFirstChild("HumanoidRootPart") then
            selectedPlayer.Character:MoveTo(lplayer.Character.HumanoidRootPart.Position + Vector3.new(2, 0, 0))
        end
    end
})

PlayersTab:CreateButton({
    Name = "Fling Jogador",
    Callback = function()
        if selectedPlayer and selectedPlayer.Character then
            local hrp = selectedPlayer.Character:FindFirstChild("HumanoidRootPart")
            if hrp then
                local bv = Instance.new("BodyVelocity")
                bv.Velocity = Vector3.new(0, 500, 0)
                bv.MaxForce = Vector3.new(1e5, 1e5, 1e5)
                bv.Parent = hrp
                task.delay(1, function() bv:Destroy() end)
            end
        end
    end
})

PlayersTab:CreateButton({
    Name = "Crash Jogador",
    Callback = function()
        if selectedPlayer and selectedPlayer.Character then
            for _ = 1, 100 do
                local s = Instance.new("Sound")
                s.SoundId = "rbxassetid://0"
                s.Parent = selectedPlayer.Character
            end
        end
    end
})

PlayersTab:CreateButton({
    Name = "Copiar Roupa do Jogador",
    Callback = function()
        if selectedPlayer and selectedPlayer.Character then
            for _, item in ipairs(selectedPlayer.Character:GetChildren()) do
                if item:IsA("Accessory") or item:IsA("Shirt") or item:IsA("Pants") then
                    item:Clone().Parent = lplayer.Character
                end
            end
        end
    end
})

-- TAB: Configs
local ConfigTab = Window:CreateTab("Configs", "settings")

ConfigTab:CreateButton({
    Name = "Bypass SS",
    Callback = function()
        pcall(function()
            for _, gui in ipairs(game:GetService("CoreGui"):GetChildren()) do
                if gui:IsA("ScreenGui") and gui.Name:lower():find("rayfield") then
                    gui:Destroy()
                end
            end
        end)
    end
})

ConfigTab:CreateButton({
    Name = "Salvar Configuração",
    Callback = function()
        pcall(function()
            Rayfield:SaveConfiguration()
        end)
    end
})

ConfigTab:CreateButton({
    Name = "Carregar Configuração",
    Callback = function()
        pcall(function()
            Rayfield:LoadConfiguration()
        end)
    end
})

ConfigTab:CreateButton({
    Name = "Abrir Discord",
    Callback = function()
        setclipboard("https://discord.gg/kAV7cZsxvc")
        Rayfield:Notify({
            Title = "Discord copiado",
            Content = "Link copiado para área de transferência.",
            Duration = 5
        })
    end
})
]==]
