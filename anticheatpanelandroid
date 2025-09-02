-- Script Anti-Cheat con Panel para Android
-- Ejecutar en KRNL

local Players = game:GetService("Players")
local Workspace = game:GetService("Workspace")
local HttpService = game:GetService("HttpService")
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")

local player = Players.LocalPlayer
local antiCheatEnabled = false
local detectedCheaters = {}
local detectionCount = 0

-- Crear interfaz GUI
local screenGui = Instance.new("ScreenGui")
screenGui.Name = "AntiCheatPanel"
screenGui.Parent = game:GetService("CoreGui")
screenGui.ResetOnSpawn = false

-- Marco principal
local mainFrame = Instance.new("Frame")
mainFrame.Size = UDim2.new(0, 350, 0, 400)
mainFrame.Position = UDim2.new(0.5, -175, 0.5, -200)
mainFrame.AnchorPoint = Vector2.new(0.5, 0.5)
mainFrame.BackgroundColor3 = Color3.fromRGB(30, 30, 40)
mainFrame.BorderSizePixel = 0
mainFrame.ClipsDescendants = true
mainFrame.Parent = screenGui

local corner = Instance.new("UICorner")
corner.CornerRadius = UDim.new(0, 12)
corner.Parent = mainFrame

-- Barra de título
local titleBar = Instance.new("Frame")
titleBar.Size = UDim2.new(1, 0, 0, 40)
titleBar.BackgroundColor3 = Color3.fromRGB(20, 20, 30)
titleBar.BorderSizePixel = 0
titleBar.Parent = mainFrame

local titleCorner = Instance.new("UICorner")
titleCorner.CornerRadius = UDim.new(0, 12)
titleCorner.Parent = titleBar

local title = Instance.new("TextLabel")
title.Size = UDim2.new(1, -50, 1, 0)
title.Position = UDim2.new(0, 15, 0, 0)
title.BackgroundTransparency = 1
title.Text = "🛡️ Anti-Cheat System v2.1"
title.TextColor3 = Color3.fromRGB(255, 255, 255)
title.TextXAlignment = Enum.TextXAlignment.Left
title.Font = Enum.Font.GothamBold
title.TextSize = 16
title.Parent = titleBar

local toggleButton = Instance.new("TextButton")
toggleButton.Size = UDim2.new(0, 30, 0, 30)
toggleButton.Position = UDim2.new(1, -35, 0.5, -15)
toggleButton.BackgroundColor3 = Color3.fromRGB(200, 60, 60)
toggleButton.Text = "OFF"
toggleButton.TextColor3 = Color3.fromRGB(255, 255, 255)
toggleButton.Font = Enum.Font.GothamBold
toggleButton.TextSize = 12
toggleButton.Parent = titleBar

local toggleCorner = Instance.new("UICorner")
toggleCorner.CornerRadius = UDim.new(1, 0)
toggleCorner.Parent = toggleButton

-- Panel de estadísticas
local statsFrame = Instance.new("Frame")
statsFrame.Size = UDim2.new(1, -20, 0, 80)
statsFrame.Position = UDim2.new(0, 10, 0, 50)
statsFrame.BackgroundColor3 = Color3.fromRGB(40, 40, 50)
statsFrame.BorderSizePixel = 0
statsFrame.Parent = mainFrame

local statsCorner = Instance.new("UICorner")
statsCorner.CornerRadius = UDim.new(0, 8)
statsCorner.Parent = statsFrame

local playersText = Instance.new("TextLabel")
playersText.Size = UDim2.new(0.5, -5, 0.5, -5)
playersText.Position = UDim2.new(0, 10, 0, 10)
playersText.BackgroundTransparency = 1
playersText.Text = "👥 Jugadores: 0"
playersText.TextColor3 = Color3.fromRGB(200, 200, 255)
playersText.TextXAlignment = Enum.TextXAlignment.Left
playersText.Font = Enum.Font.Gotham
playersText.TextSize = 14
playersText.Parent = statsFrame

local detectedText = Instance.new("TextLabel")
detectedText.Size = UDim2.new(0.5, -5, 0.5, -5)
detectedText.Position = UDim2.new(0.5, 5, 0, 10)
detectedText.BackgroundTransparency = 1
detectedText.Text = "⚠️ Detectados: 0"
detectedText.TextColor3 = Color3.fromRGB(255, 150, 150)
detectedText.TextXAlignment = Enum.TextXAlignment.Left
detectedText.Font = Enum.Font.Gotham
detectedText.TextSize = 14
detectedText.Parent = statsFrame

local statusText = Instance.new("TextLabel")
statusText.Size = UDim2.new(1, -20, 0.5, -5)
statusText.Position = UDim2.new(0, 10, 0.5, 5)
statusText.BackgroundTransparency = 1
statusText.Text = "🟢 Sistema Inactivo"
statusText.TextColor3 = Color3.fromRGB(150, 255, 150)
statusText.TextXAlignment = Enum.TextXAlignment.Left
statusText.Font = Enum.Font.GothamMedium
statusText.TextSize = 14
statusText.Parent = statsFrame

-- Lista de cheaters detectados
local cheatersFrame = Instance.new("Frame")
cheatersFrame.Size = UDim2.new(1, -20, 0, 200)
cheatersFrame.Position = UDim2.new(0, 10, 0, 140)
cheatersFrame.BackgroundColor3 = Color3.fromRGB(40, 40, 50)
cheatersFrame.BorderSizePixel = 0
cheatersFrame.Parent = mainFrame

local cheatersCorner = Instance.new("UICorner")
cheatersCorner.CornerRadius = UDim.new(0, 8)
cheatersCorner.Parent = cheatersFrame

local cheatersTitle = Instance.new("TextLabel")
cheatersTitle.Size = UDim2.new(1, -10, 0, 30)
cheatersTitle.Position = UDim2.new(0, 10, 0, 5)
cheatersTitle.BackgroundTransparency = 1
cheatersTitle.Text = "🚨 Cheaters Detectados:"
cheatersTitle.TextColor3 = Color3.fromRGB(255, 100, 100)
cheatersTitle.TextXAlignment = Enum.TextXAlignment.Left
cheatersTitle.Font = Enum.Font.GothamBold
cheatersTitle.TextSize = 14
cheatersTitle.Parent = cheatersFrame

local cheatersList = Instance.new("ScrollingFrame")
cheatersList.Size = UDim2.new(1, -10, 1, -40)
cheatersList.Position = UDim2.new(0, 5, 0, 35)
cheatersList.BackgroundTransparency = 1
cheatersList.BorderSizePixel = 0
cheatersList.ScrollBarThickness = 4
cheatersList.CanvasSize = UDim2.new(0, 0, 0, 0)
cheatersList.Parent = cheatersFrame

-- Configuración
local configFrame = Instance.new("Frame")
configFrame.Size = UDim2.new(1, -20, 0, 60)
configFrame.Position = UDim2.new(0, 10, 0, 350)
configFrame.BackgroundColor3 = Color3.fromRGB(40, 40, 50)
configFrame.BorderSizePixel = 0
configFrame.Parent = mainFrame

local configCorner = Instance.new("UICorner")
configCorner.CornerRadius = UDim.new(0, 8)
configCorner.Parent = configFrame

local autoReport = Instance.new("TextButton")
autoReport.Size = UDim2.new(0.6, -5, 0.6, -5)
autoReport.Position = UDim2.new(0, 10, 0.2, 0)
autoReport.BackgroundColor3 = Color3.fromRGB(60, 60, 80)
autoReport.Text = "📢 Auto-Report: OFF"
autoReport.TextColor3 = Color3.fromRGB(255, 150, 150)
autoReport.Font = Enum.Font.Gotham
autoReport.TextSize = 12
autoReport.Parent = configFrame

local autoReportCorner = Instance.new("UICorner")
autoReportCorner.CornerRadius = UDim.new(0, 6)
autoReportCorner.Parent = autoReport

local clearButton = Instance.new("TextButton")
clearButton.Size = UDim2.new(0.35, -5, 0.6, -5)
clearButton.Position = UDim2.new(0.65, 5, 0.2, 0)
clearButton.BackgroundColor3 = Color3.fromRGB(80, 60, 60)
clearButton.Text = "🧹 Limpiar"
clearButton.TextColor3 = Color3.fromRGB(255, 200, 200)
clearButton.Font = Enum.Font.Gotham
clearButton.TextSize = 12
clearButton.Parent = configFrame

local clearCorner = Instance.new("UICorner")
clearCorner.CornerRadius = UDim.new(0, 6)
clearCorner.Parent = clearButton

-- Funciones de detección
local function detectSpeedHack(character)
    if not character then return false end
    local humanoid = character:FindFirstChildOfClass("Humanoid")
    if not humanoid then return false end
    
    -- Detectar velocidad anormal
    if humanoid.WalkSpeed > 25 then
        return true, "Speed Hack ("..humanoid.WalkSpeed..")"
    end
    
    -- Detectar jump power anormal
    if humanoid.JumpPower > 60 then
        return true, "Jump Hack ("..humanoid.JumpPower..")"
    end
    
    return false
end

local function detectFlyHack(character)
    if not character then return false end
    local humanoid = character:FindFirstChildOfClass("Humanoid")
    local rootPart = character:FindFirstChild("HumanoidRootPart")
    
    if not humanoid or not rootPart then return false end
    
    -- Detectar si está volando sin usar las herramientas normales
    if humanoid:GetState() == Enum.HumanoidStateType.Freefall then
        local velocity = rootPart.Velocity
        if velocity.Y > 5 and not humanoid:GetState() == Enum.HumanoidStateType.Jumping then
            return true, "Fly Hack"
        end
    end
    
    return false
end

local function detectNoclip(character)
    if not character then return false end
    local humanoid = character:FindFirstChildOfClass("Humanoid")
    
    if humanoid and humanoid:GetState() == Enum.HumanoidStateType.GettingUp then
        return true, "NoClip"
    end
    
    return false
end

local function checkPlayer(player)
    if player == Players.LocalPlayer then return end
    
    local character = player.Character
    if not character then return end
    
    local detected, cheatType = detectSpeedHack(character)
    if not detected then
        detected, cheatType = detectFlyHack(character)
    end
    if not detected then
        detected, cheatType = detectNoclip(character)
    end
    
    if detected and not detectedCheaters[player.Name] then
        detectedCheaters[player.Name] = {
            Player = player,
            CheatType = cheatType,
            DetectionTime = os.time()
        }
        detectionCount = detectionCount + 1
        
        -- Añadir a la lista
        local cheatEntry = Instance.new("TextLabel")
        cheatEntry.Size = UDim2.new(1, -10, 0, 25)
        cheatEntry.Position = UDim2.new(0, 5, 0, #cheatersList:GetChildren() * 25)
        cheatEntry.BackgroundTransparency = 1
        cheatEntry.Text = "👤 "..player.Name.. " - "..cheatType
        cheatEntry.TextColor3 = Color3.fromRGB(255, 100, 100)
        cheatEntry.TextXAlignment = Enum.TextXAlignment.Left
        cheatEntry.Font = Enum.Font.Gotham
        cheatEntry.TextSize = 12
        cheatEntry.Parent = cheatersList
        
        cheatersList.CanvasSize = UDim2.new(0, 0, 0, #cheatersList:GetChildren() * 25)
        
        -- Actualizar UI
        detectedText.Text = "⚠️ Detectados: "..detectionCount
        statusText.Text = "🔴 Cheater Detectado: "..player.Name
        statusText.TextColor3 = Color3.fromRGB(255, 100, 100)
        
        -- Auto-report si está activado
        if autoReport.Text:find("ON") then
            -- Simular reporte (en un script real aquí iría el código de reporte)
            print("[AUTO-REPORT] Reportando a "..player.Name.. " por "..cheatType)
        end
    end
end

-- Función para actualizar UI
local function updateUI()
    playersText.Text = "👥 Jugadores: "..#Players:GetPlayers()
    detectedText.Text = "⚠️ Detectados: "..detectionCount
    
    if antiCheatEnabled then
        toggleButton.Text = "ON"
        toggleButton.BackgroundColor3 = Color3.fromRGB(60, 200, 60)
        statusText.Text = "🟢 Sistema Activo - Escaneando..."
        statusText.TextColor3 = Color3.fromRGB(150, 255, 150)
    else
        toggleButton.Text = "OFF"
        toggleButton.BackgroundColor3 = Color3.fromRGB(200, 60, 60)
        statusText.Text = "🔴 Sistema Inactivo"
        statusText.TextColor3 = Color3.fromRGB(255, 150, 150)
    end
end

-- Conectar eventos
toggleButton.MouseButton1Click:Connect(function()
    antiCheatEnabled = not antiCheatEnabled
    updateUI()
end)

autoReport.MouseButton1Click:Connect(function()
    if autoReport.Text:find("OFF") then
        autoReport.Text = "📢 Auto-Report: ON"
        autoReport.TextColor3 = Color3.fromRGB(150, 255, 150)
    else
        autoReport.Text = "📢 Auto-Report: OFF"
        autoReport.TextColor3 = Color3.fromRGB(255, 150, 150)
    end
end)

clearButton.MouseButton1Click:Connect(function()
    detectedCheaters = {}
    detectionCount = 0
    for _, child in pairs(cheatersList:GetChildren()) do
        if child:IsA("TextLabel") then
            child:Destroy()
        end
    end
    detectedText.Text = "⚠️ Detectados: 0"
    statusText.Text = "🟢 Lista limpiada"
    wait(2)
    updateUI()
end)

-- Loop principal de detección
spawn(function()
    while true do
        if antiCheatEnabled then
            for _, player in pairs(Players:GetPlayers()) do
                if player ~= Players.LocalPlayer then
                    checkPlayer(player)
                end
            end
        end
        wait(1) -- Escanear cada segundo
    end
end)

-- Actualizar jugadores en tiempo real
Players.PlayerAdded:Connect(function()
    updateUI()
end)

Players.PlayerRemoving:Connect(function()
    updateUI()
end)

-- Hacer la ventana arrastrable
local dragging = false
local dragInput, dragStart, startPos

mainFrame.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
        dragging = true
        dragStart = input.Position
        startPos = mainFrame.Position
        
        input.Changed:Connect(function()
            if input.UserInputState == Enum.UserInputState.End then
                dragging = false
            end
        end
    end
end)

mainFrame.InputChanged:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch then
        dragInput = input
    end
end)

UserInputService.InputChanged:Connect(function(input)
    if input == dragInput and dragging then
        local delta = input.Position - dragStart
        mainFrame.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)
    end
end)

-- Inicializar UI
updateUI()
print("🛡️ Anti-Cheat System cargado correctamente!")
print("📱 Panel disponible en la pantalla")
print("⚙️ Configura las opciones según necesites")
