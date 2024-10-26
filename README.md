-- Configuração da biblioteca de UI
local Players = game:GetService("Players")
local TweenService = game:GetService("TweenService")
local LocalPlayer = Players.LocalPlayer

-- Função para criar uma UI simples
local function createUI()
    local screenGui = Instance.new("ScreenGui", LocalPlayer:WaitForChild("PlayerGui"))
    local frame = Instance.new("Frame", screenGui)
    frame.Size = UDim2.new(0.2, 0, 0.2, 0)
    frame.Position = UDim2.new(0.4, 0, 0.4, 0)
    frame.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
    frame.BackgroundTransparency = 0.5

    local titleLabel = Instance.new("TextLabel", frame)
    titleLabel.Size = UDim2.new(1, 0, 0.2, 0)
    titleLabel.Position = UDim2.new(0, 0, 0, 0)
    titleLabel.Text = "ESP Ativado"
    titleLabel.TextColor3 = Color3.fromRGB(0, 0, 0)
    titleLabel.BackgroundTransparency = 1
end

-- Função para criar um ESP
local function createESP(player)
    if player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
        local highlight = Instance.new("Highlight")
        highlight.Parent = player.Character
        highlight.FillColor = player.TeamColor == LocalPlayer.TeamColor and Color3.fromRGB(0, 0, 255) or Color3.fromRGB(255, 0, 0) -- Azul para a equipe, Vermelho para inimigos
        highlight.OutlineColor = Color3.fromRGB(0, 0, 0) -- Cor da borda
        highlight.FillTransparency = 0.5 -- Transparência do preenchimento
        highlight.OutlineTransparency = 0 -- Transparência da borda
    end
end

-- Função para remover o ESP
local function removeESP(player)
    if player.Character then
        local highlight = player.Character:FindFirstChildOfClass("Highlight")
        if highlight then
            highlight:Destroy()
        end
    end
end

-- Conectar eventos para adicionar/remover ESP quando um jogador entra ou sai
Players.PlayerAdded:Connect(function(player)
    player.CharacterAdded:Connect(function(character)
        wait(1) -- Espera um pouco para garantir que o personagem esteja totalmente carregado
        createESP(player)
    end)
end)

Players.PlayerRemoving:Connect(function(player)
    removeESP(player)
end)

-- Aplicar ESP a todos os jogadores já presentes
for _, player in pairs(Players:GetPlayers()) do
    if player ~= LocalPlayer then
        createESP(player)
    end
end

-- Criar a UI
createUI()
