local UserInputService = game:GetService("UserInputService")
local RunService = game:GetService("RunService")
local Players = game:GetService("Players")
local player = Players.LocalPlayer
local playerGui = player:WaitForChild("PlayerGui")

-- Configurações
local clickSpeed = 1 -- Intervalo entre cliques em segundos (ajuste para mais rápido)
local isClicking = false
local lastClickTime = 0

-- Cria a ScreenGui
local screenGui = Instance.new("ScreenGui")
screenGui.Name = "AutoClickerGUI"
screenGui.Parent = playerGui

-- Cria o Frame principal
local frame = Instance.new("Frame")
frame.Size = UDim2.new(0, 200, 0, 100)
frame.Position = UDim2.new(0.5, -100, 0.5, -50)
frame.AnchorPoint = Vector2.new(0.5, 0.5)
frame.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
frame.BorderSizePixel = 0
frame.Parent = screenGui

-- Título
local title = Instance.new("TextLabel")
title.Size = UDim2.new(1, 0, 0, 30)
title.Position = UDim2.new(0, 0, 0, 0)
title.Text = "AutoClicker Rápido"
title.TextColor3 = Color3.fromRGB(255, 255, 255)
title.BackgroundTransparency = 1
title.Font = Enum.Font.SourceSansBold
title.TextSize = 18
title.Parent = frame

-- Botão Ligar/Desligar
local toggleButton = Instance.new("TextButton")
toggleButton.Size = UDim2.new(0.8, 0, 0, 40)
toggleButton.Position = UDim2.new(0.1, 0, 0.4, 0)
toggleButton.Text = "LIGAR"
toggleButton.TextColor3 = Color3.fromRGB(255, 255, 255)
toggleButton.BackgroundColor3 = Color3.fromRGB(30, 136, 229)
toggleButton.Font = Enum.Font.SourceSansBold
toggleButton.TextSize = 16
toggleButton.Parent = frame

-- Função para simular clique
local function simulateClick()
    local viewportSize = workspace.CurrentCamera.ViewportSize
    local center = Vector2.new(viewportSize.X/2, viewportSize.Y/2)
    UserInputService:SetMouseLocation(center)
    
    -- Simula pressionar e soltar o botão do mouse
    mouse1press()
    task.wait(0.01)
    mouse1release()
end

-- Conexão para o clique rápido
local connection
connection = RunService.Heartbeat:Connect(function(deltaTime)
    if isClicking then
        lastClickTime = lastClickTime + deltaTime
        if lastClickTime >= clickSpeed then
            simulateClick()
            lastClickTime = 0
        end
    end
end)

-- Função para alternar o clique
local function toggleAutoClick()
    isClicking = not isClicking
    if isClicking then
        toggleButton.Text = "DESLIGAR"
        toggleButton.BackgroundColor3 = Color3.fromRGB(229, 57, 53)
    else
        toggleButton.Text = "LIGAR"
        toggleButton.BackgroundColor3 = Color3.fromRGB(30, 136, 229)
    end
end

-- Conecta o botão
toggleButton.MouseButton1Click:Connect(toggleAutoClick)

-- Limpeza quando o script é destruído
screenGui.Destroying:Connect(function()
    connection:Disconnect()
end)
