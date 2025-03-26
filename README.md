local Player = game.Players.LocalPlayer
local UIS = game:GetService("UserInputService")

-- Cria a GUI
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "AutoClickerGui"
ScreenGui.Parent = Player.PlayerGui

local Frame = Instance.new("Frame")
Frame.Size = UDim2.new(0, 200, 0, 100)
Frame.Position = UDim2.new(0.5, -100, 0.5, -50)
Frame.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
Frame.Active = true  -- Permite arrastar
Frame.Draggable = true  -- Permite arrastar
Frame.Parent = ScreenGui

-- Adiciona UICorner (bordas arredondadas)
local UICorner = Instance.new("UICorner")
UICorner.CornerRadius = UDim.new(0, 10)
UICorner.Parent = Frame

-- Texto "Modo Malhar"
local Title = Instance.new("TextLabel")
Title.Text = "Modo Malhar"
Title.Size = UDim2.new(1, 0, 0, 30)
Title.Position = UDim2.new(0, 0, 0, 0)
Title.BackgroundTransparency = 1
Title.TextColor3 = Color3.fromRGB(255, 255, 255)
Title.Font = Enum.Font.SourceSansBold
Title.Parent = Frame

-- Botão ON/OFF
local ToggleButton = Instance.new("TextButton")
ToggleButton.Size = UDim2.new(0, 80, 0, 40)
ToggleButton.Position = UDim2.new(0.5, -40, 0.5, -20)
ToggleButton.Text = "OFF"
ToggleButton.BackgroundColor3 = Color3.fromRGB(200, 50, 50)
ToggleButton.TextColor3 = Color3.fromRGB(255, 255, 255)
ToggleButton.Font = Enum.Font.SourceSansBold
ToggleButton.Parent = Frame

-- Adiciona UICorner ao botão
local ButtonCorner = Instance.new("UICorner")
ButtonCorner.CornerRadius = UDim.new(0, 8)
ButtonCorner.Parent = ToggleButton

-- Variável para controle
local isClicking = false

-- Função de autoclique
local function autoClick()
    while isClicking do
        game:GetService("VirtualInputManager"):SendMouseButtonEvent(0, 0, 0, true, game, 1)  -- Mouse Down
        task.wait(0.1)
        game:GetService("VirtualInputManager"):SendMouseButtonEvent(0, 0, 0, false, game, 1) -- Mouse Up
        task.wait(0.1)
    end
end

-- Botão ON/OFF
ToggleButton.MouseButton1Click:Connect(function()
    isClicking = not isClicking
    if isClicking then
        ToggleButton.Text = "ON"
        ToggleButton.BackgroundColor3 = Color3.fromRGB(50, 200, 50)
        autoClick()
    else
        ToggleButton.Text = "OFF"
        ToggleButton.BackgroundColor3 = Color3.fromRGB(200, 50, 50)
    end
end)
