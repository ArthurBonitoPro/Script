-- Configurações SCRIPT

-- Gui to Lua (VIP VERSION)
-- Version: 6.9

-- Instances:

local ScreenGui = Instance.new("ScreenGui")
local Frame = Instance.new("Frame")
local Frame_2 = Instance.new("Frame")
local TextLabel = Instance.new("TextLabel")
local TextButton = Instance.new("TextButton")
local UICorner = Instance.new("UICorner")
local UICorner_2 = Instance.new("UICorner")
local UICorner_3 = Instance.new("UICorner")

--Properties:

ScreenGui.Parent = game.Players.LocalPlayer:WaitForChild("PlayerGui")
ScreenGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
ScreenGui.ResetOnSpawn = false
print("Menu de Invisibilidade Carregado")

Frame.Parent = ScreenGui
Frame.BackgroundColor3 = Color3.fromRGB(34, 34, 34)
Frame.BorderColor3 = Color3.fromRGB(0, 0, 0)
Frame.BorderSizePixel = 0
Frame.Position = UDim2.new(0.388539821, 0, 0.427821517, 0)
Frame.Size = UDim2.new(0, 200, 0, 120)

UICorner.Parent = Frame
UICorner.CornerRadius = UDim.new(0, 12)

Frame_2.Parent = Frame
Frame_2.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
Frame_2.BorderColor3 = Color3.fromRGB(0, 0, 0)
Frame_2.BorderSizePixel = 0
Frame_2.Size = UDim2.new(0, 200, 0, 30)

UICorner_2.Parent = Frame_2
UICorner_2.CornerRadius = UDim.new(0, 12)

TextLabel.Parent = Frame_2
TextLabel.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
TextLabel.BackgroundTransparency = 1.000
TextLabel.BorderColor3 = Color3.fromRGB(0, 0, 0)
TextLabel.BorderSizePixel = 0
TextLabel.Position = UDim2.new(0.05, 0, 0, 0)
TextLabel.Size = UDim2.new(0, 180, 0, 30)
TextLabel.Font = Enum.Font.Sarpanch
TextLabel.Text = "Menu Invisível"
TextLabel.TextColor3 = Color3.fromRGB(0, 255, 0)
TextLabel.TextSize = 20.000

TextButton.Parent = Frame
TextButton.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
TextButton.BorderColor3 = Color3.fromRGB(0, 0, 0)
TextButton.BorderSizePixel = 0
TextButton.Position = UDim2.new(0.1, 0, 0.4, 0)
TextButton.Size = UDim2.new(0, 160, 0, 40)
TextButton.Font = Enum.Font.SourceSansItalic
TextButton.Text = "FICAR INVISÍVEL"
TextButton.TextColor3 = Color3.fromRGB(0, 0, 0)
TextButton.TextSize = 20.000

UICorner_3.Parent = TextButton
UICorner_3.CornerRadius = UDim.new(0, 8)

-- Scripts:

local function InvisibilityScript() -- TextButton.LocalScript
    local script = Instance.new('LocalScript', TextButton)

    local Players = game:GetService("Players")
    local LocalPlayer = Players.LocalPlayer
    local Character = LocalPlayer.Character or LocalPlayer.CharacterAdded:Wait()
    local HumanoidRootPart = Character:WaitForChild("HumanoidRootPart")

    local isInvisible = false

    TextButton.MouseButton1Click:Connect(function()
        isInvisible = not isInvisible

        if isInvisible then
            -- Torna o personagem invisível
            for _, part in ipairs(Character:GetDescendants()) do
                if part:IsA("BasePart") and part ~= HumanoidRootPart then
                    part.Transparency = 1
                end
            end
            TextButton.Text = "VISÍVEL"
        else
            -- Torna o personagem visível novamente
            for _, part in ipairs(Character:GetDescendants()) do
                if part:IsA("BasePart") and part ~= HumanoidRootPart then
                    part.Transparency = 0
                end
            end
            TextButton.Text = "FICAR INVISÍVEL"
        end
    end)
end
coroutine.wrap(InvisibilityScript)()

local function DragScript() -- Frame.LocalScript
    local script = Instance.new('LocalScript', Frame)

    local UserInputService = game:GetService("UserInputService")
    local dragging
    local dragInput
    local dragStart
    local startPos

    local function update(input)
        local delta = input.Position - dragStart
        Frame.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)
    end

    Frame.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
            dragging = true
            dragStart = input.Position
            startPos = Frame.Position

            input.Changed:Connect(function()
                if input.UserInputState == Enum.UserInputState.End then
                    dragging = false
                end
            end)
        end
    end)

    Frame.InputChanged:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch then
            dragInput = input
        end
    end)

    UserInputService.InputChanged:Connect(function(input)
        if input == dragInput and dragging then
            update(input)
        end
    end)
end
coroutine.wrap(DragScript)()
