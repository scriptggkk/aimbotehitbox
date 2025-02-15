```lua
-- Roblox exploit script with a movable UI and aimbot/hitbox features

local player = game.Players.LocalPlayer
local mouse = player:GetMouse()
local screenGui = Instance.new("ScreenGui", player.PlayerGui)

-- Create a medium-sized frame for the exploit
local frame = Instance.new("Frame", screenGui)
frame.Size = UDim2.new(0.3, 0, 0.3, 0)
frame.Position = UDim2.new(0.35, 0, 0.35, 0)
frame.BackgroundColor3 = Color3.new(1, 1, 1)
frame.Active = true
frame.Draggable = true

-- Create a close button
local closeButton = Instance.new("TextButton", frame)
closeButton.Size = UDim2.new(0.1, 0, 0.1, 0)
closeButton.Position = UDim2.new(0.9, 0, 0, 0)
closeButton.Text = "X"
closeButton.BackgroundColor3 = Color3.new(1, 0, 0)

-- Function to close the exploit frame
closeButton.MouseButton1Click:Connect(function()
    screenGui:Destroy()
end)

-- Aimbot functionality
local aimbotEnabled = false
local aimbotTarget = nil

-- Function to toggle aimbot
local function toggleAimbot()
    aimbotEnabled = not aimbotEnabled
end

-- Create a button for aimbot
local aimbotButton = Instance.new("TextButton", frame)
aimbotButton.Size = UDim2.new(0.3, 0, 0.1, 0)
aimbotButton.Position = UDim2.new(0.35, 0, 0.2, 0)
aimbotButton.Text = "Toggle Aimbot"
aimbotButton.BackgroundColor3 = Color3.new(0, 1, 0)

-- Function to focus on a player
aimbotButton.MouseButton1Click:Connect(function()
    toggleAimbot()
    if aimbotEnabled then
        while aimbotEnabled do
            wait()
            local closestPlayer = nil
            local closestDistance = math.huge
            for _, target in ipairs(game.Players:GetPlayers()) do
                if target ~= player and target.Character and target.Character:FindFirstChild("Head") then
                    local distance = (target.Character.Head.Position - player.Character.Head.Position).magnitude
                    if distance < closestDistance then
                        closestDistance = distance
                        closestPlayer = target
                    end
                end
            end
            if closestPlayer then
                aimbotTarget = closestPlayer.Character.Head
                -- Aim at the target's head
                mouse.Target = aimbotTarget
            end
        end
    end
end)

-- Hitbox functionality
local hitboxEnabled = false

-- Function to toggle hitbox
local function toggleHitbox()
    hitboxEnabled = not hitboxEnabled
end

-- Create a button for hitbox
local hitboxButton = Instance.new("TextButton", frame)
hitboxButton.Size = UDim2.new(0.3, 0, 0.1, 0)
hitboxButton.Position = UDim2.new(0.35, 0, 0.4, 0)
hitboxButton.Text = "Toggle Hitbox"
hitboxButton.BackgroundColor3 = Color3.new(0, 0, 1)

-- Function to display hitboxes around players
hitboxButton.MouseButton1Click:Connect(function()
    toggleHitbox()
    if hitboxEnabled then
        while hitboxEnabled do
            wait()
            for _, target in ipairs(game.Players:GetPlayers()) do
                if target ~= player and target.Character and target.Character:FindFirstChild("HumanoidRootPart") then
                    local box = Instance.new("Part")
                    box.Size = Vector3.new(4, 4, 4)
                    box.Position = target.Character.HumanoidRootPart.Position
                    box.Anchored = true
                    box.CanCollide = false
                    box.BrickColor = BrickColor.new("Bright blue")
                    box.Transparency = 0.5
                    box.Parent = workspace
                    wait(1) -- Adjust duration as needed
                    box:Destroy()
                end
            end
        end
    end
end)
```
