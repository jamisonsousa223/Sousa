-- Script Delta: Debug GUI para "99 Noites na Floresta"
-- USO SEGURO: apenas no seu próprio jogo

-- SERVIÇOS
local Players = game:GetService("Players")
local ReplicatedStorage = game:GetService("ReplicatedStorage")

-- JOGADOR E PERSONAGEM
local player = Players.LocalPlayer
local char = player.Character or player.CharacterAdded:Wait()
local hrp = char:WaitForChild("HumanoidRootPart")
local backpack = player:WaitForChild("Backpack")
local playerGui = player:WaitForChild("PlayerGui")

-- CONFIGURAÇÕES
local teleportPoints = {
    Fogueira = Vector3.new(100,5,50),
    Casa = Vector3.new(200,5,150),
    Lago = Vector3.new(50,5,250)
}
local itensParaDar = {"Sword","Pickaxe","Shield"}
local atravessaAtivo = false

-- FUNÇÕES
local function teleportToPoint(pos)
    hrp.CFrame = CFrame.new(pos)
end

local function toggleAtravessar()
    atravessaAtivo = not atravessaAtivo
    for _, part in ipairs(char:GetDescendants()) do
        if part:IsA("BasePart") then
            part.CanCollide = not atravessaAtivo
        end
    end
    statusLabel.Text = "Atravessa-Paredes: "..(atravessaAtivo and "Ativo" or "Desativado")
end

local function darItensFunc()
    for _, itemName in ipairs(itensParaDar) do
        local tool = ReplicatedStorage:FindFirstChild(itemName)
        if tool and tool:IsA("Tool") then
            local clone = tool:Clone()
            clone.Parent = backpack
        end
    end
    statusLabel.Text = "Itens dados!"
end

-- CRIANDO GUI
local screenGui = Instance.new("ScreenGui")
screenGui.Name = "DeltaDebugGUI"
screenGui.Parent = playerGui
screenGui.ResetOnSpawn = false

-- Botões de Teleporte
local yPos = 50
for name, pos in pairs(teleportPoints) do
    local btn = Instance.new("TextButton")
    btn.Size = UDim2.new(0,200,0,40)
    btn.Position = UDim2.new(0,50,0,yPos)
    btn.Text = "Teleport: "..name
    btn.BackgroundColor3 = Color3.fromRGB(0,170,255)
    btn.Parent = screenGui
    btn.MouseButton1Click:Connect(function() teleportToPoint(pos) end)
    yPos = yPos + 50
end

-- Botão Atravessa-Paredes
local atravessarButton = Instance.new("TextButton")
atravessarButton.Size = UDim2.new(0,200,0,40)
atravessarButton.Position = UDim2.new(0,50,0,yPos)
atravessarButton.Text = "Ativar/Desativar Atravessa-Paredes"
atravessarButton.BackgroundColor3 = Color3.fromRGB(255,170,0)
atravessarButton.Parent = screenGui
atravessarButton.MouseButton1Click:Connect(toggleAtravessar)
yPos = yPos + 50

-- Botão Dar Itens
local darItensButton = Instance.new("TextButton")
darItensButton.Size = UDim2.new(0,200,0,40)
darItensButton.Position = UDim2.new(0,50,0,yPos)
darItensButton.Text = "Dar Itens"
darItensButton.BackgroundColor3 = Color3.fromRGB(0,255,128)
darItensButton.Parent = screenGui
darItensButton.MouseButton1Click:Connect(darItensFunc)
yPos = yPos + 50

-- Botão Fechar GUI
local fecharButton = Instance.new("TextButton")
fecharButton.Size = UDim2.new(0,200,0,40)
fecharButton.Position = UDim2.new(0,50,0,yPos)
fecharButton.Text = "Fechar GUI"
fecharButton.BackgroundColor3 = Color3.fromRGB(255,0,0)
fecharButton.Parent = screenGui
fecharButton.MouseButton1Click:Connect(function()
    screenGui:Destroy()
end)

-- Label de Status
local statusLabel = Instance.new("TextLabel")
statusLabel.Size = UDim2.new(0,300,0,40)
statusLabel.Position = UDim2.new(0,300,0,50)
statusLabel.Text = "Debug GUI Ativa"
statusLabel.TextScaled = true
statusLabel.BackgroundColor3 = Color3.fromRGB(255,255,255)
statusLabel.TextColor3 = Color3.fromRGB(0,0,0)
statusLabel.Parent = screenGui
