-- Exploit para jogos do Roblox
-- Cria um hub na tela do jogador com funções de hitbox e aimbot

-- Função para criar o hub
local function createHub()
    -- Cria a janela do hub
    local hub = Instance.new("ScreenGui")
    hub.Name = "RobloxExploit"
    
    -- Cria os botões do hub
    local hitboxButton = Instance.new("TextButton")
    hitboxButton.Name = "HitboxButton"
    hitboxButton.Text = "Ativar Hitbox"
    hitboxButton.Position = UDim2.new(0.1, 0, 0.1, 0)
    hitboxButton.Size = UDim2.new(0.2, 0, 0.1, 0)
    hitboxButton.Parent = hub
    
    local aimBotButton = Instance.new("TextButton")
    aimBotButton.Name = "AimBotButton"
    aimBotButton.Text = "Ativar Aimbot"
    aimBotButton.Position = UDim2.new(0.1, 0, 0.3, 0)
    aimBotButton.Size = UDim2.new(0.2, 0, 0.1, 0)
    aimBotButton.Parent = hub
    
    -- Funções dos botões
    hitboxButton.MouseButton1Click:Connect(function()
        enableHitbox()
    end)
    
    aimBotButton.MouseButton1Click:Connect(function()
        enableAimbot()
    end)
    
    -- Retorna o hub
    return hub
end

-- Função para ativar a hitbox
function enableHitbox()
    -- Percorre todos os jogadores no jogo
    for _, player in pairs(game:GetService("Players"):GetPlayers()) do
        -- Cria uma caixa de hitbox para cada jogador
        local hitbox = Instance.new("Part")
        hitbox.Name = player.Name .. "Hitbox"
        hitbox.Size = Vector3.new(3, 6, 3)
        hitbox.Color = Color3.fromRGB(255, 0, 0)
        hitbox.Transparency = 0.5
        hitbox.Parent = player.Character
    end
end

-- Função para ativar o aimbot
function enableAimbot()
    -- Percorre todos os jogadores no jogo
    for _, player in pairs(game:GetService("Players"):GetPlayers()) do
        -- Mira automaticamente no jogador mais próximo
        local character = player.Character
        if character then
            local humanoid = character:FindFirstChildOfClass("Humanoid")
            if humanoid then
                local target = getClosestPlayer(character.HumanoidRootPart.Position)
                if target then
                    character:SetPrimaryPartCFrame(CFrame.new(character.HumanoidRootPart.Position, target.HumanoidRootPart.Position))
                end
            end
        end
    end
end

-- Função auxiliar para encontrar o jogador mais próximo
function getClosestPlayer(position)
    local closestPlayer = nil
    local closestDistance = math.huge
    
    for _, player in pairs(game:GetService("Players"):GetPlayers()) do
        if player ~= game.Players.LocalPlayer then
            local character = player.Character
            if character then
                local distance = (character.HumanoidRootPart.Position - position).magnitude
                if distance < closestDistance then
                    closestPlayer = character
                    closestDistance = distance
                end
            end
        end
    end
    
    return closestPlayer
end

-- Cria o hub e o adiciona à interface do jogo
createHub().Parent = game.Players.LocalPlayer:WaitForChild("PlayerGui")
