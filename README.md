
-- Create a simple GUI for the player
local player = game.Players.LocalPlayer
local screenGui = Instance.new("ScreenGui", player.PlayerGui)
local hubFrame = Instance.new("Frame", screenGui)
hubFrame.Size = UDim2.new(0.3, 0, 0.3, 0)
hubFrame.Position = UDim2.new(0.35, 0, 0.35, 0)
hubFrame.BackgroundColor3 = Color3.new(0, 0, 0)
hubFrame.BackgroundTransparency = 0.5

-- Create Hitbox Button
local hitboxButton = Instance.new("TextButton", hubFrame)
hitboxButton.Size = UDim2.new(1, 0, 0.5, 0)
hitboxButton.Text = "Activate Hitbox"
hitboxButton.BackgroundColor3 = Color3.new(1, 0, 0)

-- Create Aimbot Button
local aimbotButton = Instance.new("TextButton", hubFrame)
aimbotButton.Size = UDim2.new(1, 0, 0.5, 0)
aimbotButton.Position = UDim2.new(0, 0, 0.5, 0)
aimbotButton.Text = "Activate Aimbot"
aimbotButton.BackgroundColor3 = Color3.new(0, 1, 0)

-- Hitbox function
local hitboxActive = false
hitboxButton.MouseButton1Click:Connect(function()
    hitboxActive = not hitboxActive
    if hitboxActive then
        hitboxButton.Text = "Deactivate Hitbox"
        -- Code to create hitboxes around other players
        for _, target in ipairs(game.Players:GetPlayers()) do
            if target ~= player then
                local hitbox = Instance.new("Part")
                hitbox.Size = Vector3.new(5, 5, 5) -- Size of the hitbox
                hitbox.Position = target.Character.HumanoidRootPart.Position
                hitbox.Anchored = true
                hitbox.Transparency = 0.5
                hitbox.BrickColor = BrickColor.new("Bright red")
                hitbox.Parent = workspace
            end
        end
    else
        hitboxButton.Text = "Activate Hitbox"
        -- Code to remove hitboxes
        for _, v in ipairs(workspace:GetChildren()) do
            if v:IsA("Part") and v.BrickColor == BrickColor.new("Bright red") then
                v:Destroy()
            end
        end
    end
end)

-- Aimbot function
local aimbotActive = false
aimbotButton.MouseButton1Click:Connect(function()
    aimbotActive = not aimbotActive
    if aimbotActive then
        aimbotButton.Text = "Deactivate Aimbot"
        -- Code to activate aimbot logic
        game:GetService("RunService").RenderStepped:Connect(function()
            if aimbotActive then
                local closestPlayer = nil
                local closestDistance = math.huge
                for _, target in ipairs(game.Players:GetPlayers()) do
                    if target ~= player and target.Character and target.Character:FindFirstChild("HumanoidRootPart") then
                        local distance = (target.Character.HumanoidRootPart.Position - player.Character.HumanoidRootPart.Position).magnitude
                        if distance < closestDistance then
                            closestDistance = distance
                            closestPlayer = target
                        end
                    end
                end
                if closestPlayer then
                    -- Aim at the closest player
                    local targetPosition = closestPlayer.Character.HumanoidRootPart.Position
                    player.Character.HumanoidRootPart.CFrame = CFrame.new(targetPosition)
                end
            end
        end)
    else
        aimbotButton.Text = "Activate Aimbot"
    end
end)
```
