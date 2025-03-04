-- INÍCIO: Configuração Inicial
local player = game.Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()

-- Criando a interface gráfica (ESP)
local screenGui = Instance.new("ScreenGui")
screenGui.Parent = player:WaitForChild("PlayerGui")

-- Função para criar ESP nos jogadores
local function createESP(targetPlayer)
    -- Verifica se o ESP já existe
    if targetPlayer.Character and targetPlayer.Character:FindFirstChild("HumanoidRootPart") then
        local humanoidRootPart = targetPlayer.Character.HumanoidRootPart

        -- Criando o marcador para o ESP
        local espLabel = Instance.new("BillboardGui")
        espLabel.Adornee = humanoidRootPart
        espLabel.Size = UDim2.new(0, 100, 0, 50)
        espLabel.StudsOffset = Vector3.new(0, 3, 0) -- Eleva o marcador acima do jogador
        espLabel.AlwaysOnTop = true
        espLabel.Parent = screenGui

        -- Criando o texto para o ESP
        local espText = Instance.new("TextLabel")
        espText.Text = targetPlayer.Name
        espText.Size = UDim2.new(1, 0, 1, 0)
        espText.BackgroundTransparency = 1
        espText.TextColor3 = Color3.fromRGB(255, 0, 0) -- Cor vermelha para destacar
        espText.TextStrokeTransparency = 0.8
        espText.Parent = espLabel
    end
end

-- Loop para atualizar a posição do ESP
game:GetService("RunService").Heartbeat:Connect(function()
    for _, otherPlayer in pairs(game.Players:GetPlayers()) do
        if otherPlayer ~= player and otherPlayer.Character and otherPlayer.Character:FindFirstChild("HumanoidRootPart") then
            -- Cria ESP para os jogadores se ainda não houver
            if not otherPlayer.Character:FindFirstChild("ESP") then
                createESP(otherPlayer)
                -- Marcando para não recriar o ESP
                local espFlag = Instance.new("ObjectValue")
                espFlag.Name = "ESP"
                espFlag.Parent = otherPlayer.Character
            end
        end
    end
end)

-- FIM: Finalização e Mensagem de Log
print("Script de ESP carregado com sucesso! Agora você pode ver os jogadores através das paredes.")
