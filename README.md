-- Stack Hub com botão de abrir/fechar usando imagem

-- Serviços principais
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local LocalPlayer = Players.LocalPlayer
local Character = LocalPlayer.Character or LocalPlayer.CharacterAdded:Wait()
local HumanoidRootPart = Character:WaitForChild("HumanoidRootPart")

-- Criar GUI
local gui = Instance.new("ScreenGui", LocalPlayer:WaitForChild("PlayerGui"))
gui.Name = "StackHub"

-- Botão de abrir/fechar com imagem
local toggleButton = Instance.new("ImageButton", gui)
toggleButton.Size = UDim2.new(0, 50, 0, 50)
toggleButton.Position = UDim2.new(0, 10, 0, 10)
toggleButton.BackgroundTransparency = 1
toggleButton.Image = "rbxassetid://INSERT_IMAGE_ID_HERE" -- substitua pelo ID da imagem do logo

-- Painel principal
local frame = Instance.new("Frame", gui)
frame.Size = UDim2.new(0, 240, 0, 200)
frame.Position = UDim2.new(0.05, 0, 0.25, 0)
frame.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
frame.BorderColor3 = Color3.fromRGB(255, 0, 0)
frame.BorderSizePixel = 2
frame.Visible = false
frame.Active = true
frame.Draggable = true

-- Título
local title = Instance.new("TextLabel", frame)
title.Size = UDim2.new(1, 0, 0, 25)
title.Text = "STACK HUB VIP+"
title.Font = Enum.Font.Code
title.TextSize = 18
title.TextColor3 = Color3.fromRGB(255, 0, 0)
title.BackgroundTransparency = 1

-- Texto secundário
local tiktok = Instance.new("TextLabel", frame)
tiktok.Size = UDim2.new(1, -10, 0, 20)
tiktok.Position = UDim2.new(0, 5, 0, 25)
tiktok.Text = "TikTok: @stackhuboficial"
tiktok.Font = Enum.Font.Code
tiktok.TextSize = 14
tiktok.TextColor3 = Color3.fromRGB(255, 255, 255)
tiktok.BackgroundTransparency = 1

-- Criar botão com função
local function createButton(text, posY, callback)
    local btn = Instance.new("TextButton", frame)
    btn.Size = UDim2.new(1, -10, 0, 30)
    btn.Position = UDim2.new(0, 5, 0, posY)
    btn.Text = text
    btn.Font = Enum.Font.Code
    btn.TextSize = 16
    btn.TextColor3 = Color3.fromRGB(255, 0, 0)
    btn.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
    btn.MouseButton1Click:Connect(callback)
end

-- Funções do Stack Hub
createButton("Super Pulo", 50, function()
    local humanoid = Character:FindFirstChildWhichIsA("Humanoid")
    if humanoid then
        humanoid.UseJumpPower = true
        humanoid.JumpPower = 200
    end
end)

local flying = false
createButton("Voar Para Onde Olhar", 85, function()
    flying = not flying
    if flying then
        RunService:BindToRenderStep("StackHubFly", Enum.RenderPriority.Character.Value, function()
            local cam = workspace.CurrentCamera
            HumanoidRootPart.Velocity = cam.CFrame.LookVector * 100
        end)
    else
        RunService:UnbindFromRenderStep("StackHubFly")
    end
end)

createButton("Marcar Jogadores", 120, function()
    for _, plr in pairs(Players:GetPlayers()) do
        if plr ~= LocalPlayer and plr.Character and plr.Character:FindFirstChild("HumanoidRootPart") then
            local highlight = Instance.new("Highlight")
            highlight.FillColor = Color3.fromRGB(255, 255, 0)
            highlight.OutlineColor = Color3.fromRGB(255, 0, 0)
            highlight.Parent = plr.Character
        end
    end
end)

createButton("Rejoin", 155, function()
    LocalPlayer:Kick("Rejoining...")
    wait(1)
    game:GetService("TeleportService"):Teleport(game.PlaceId, LocalPlayer)
end)

-- Toggle de abertura/fechamento
toggleButton.MouseButton1Click:Connect(function()
    frame.Visible = not frame.Visible
end)
