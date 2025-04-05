local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local Mouse = LocalPlayer:GetMouse()
local AutoCombo = false

local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Parent = game.Players.LocalPlayer.PlayerGui
ScreenGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling

local Frame = Instance.new("Frame")
Frame.Parent = ScreenGui
Frame.Size = UDim2.new(0, 300, 0, 100)
Frame.Position = UDim2.new(0, 10, 0, 10)
Frame.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
Frame.BackgroundTransparency = 0.5

local Title = Instance.new("TextLabel")
Title.Parent = Frame
Title.Size = UDim2.new(1, 0, 0, 20)
Title.Text = "Blox Fruits - Auto Combo"
Title.TextColor3 = Color3.fromRGB(255, 255, 255)
Title.BackgroundTransparency = 1
Title.TextSize = 16

local BountyLabel = Instance.new("TextLabel")
BountyLabel.Parent = Frame
BountyLabel.Size = UDim2.new(1, 0, 0, 40)
BountyLabel.Position = UDim2.new(0, 0, 0, 25)
BountyLabel.Text = "Tên: " .. LocalPlayer.Name .. "\nBounty: " .. (LocalPlayer.leaderstats and LocalPlayer.leaderstats.Bounty and LocalPlayer.leaderstats.Bounty.Value or 0)
BountyLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
BountyLabel.BackgroundTransparency = 1
BountyLabel.TextSize = 14

local function UpdateBounty()
    local bounty = LocalPlayer.leaderstats and LocalPlayer.leaderstats.Bounty and LocalPlayer.leaderstats.Bounty.Value or 0
    BountyLabel.Text = "Tên: " .. LocalPlayer.Name .. "\nBounty: " .. bounty
end

spawn(function()
    while true do
        UpdateBounty()
        wait(1)
    end
end)

function GetClosestPlayer()
    local closestPlayer, shortestDistance = nil, math.huge
    for _, player in pairs(Players:GetPlayers()) do
        if player ~= LocalPlayer and player.Character and player.Character:FindFirstChild("HumanoidRootPart") and player.Character:FindFirstChild("Humanoid") and player.Character.Humanoid.Health > 0 then
            local distance = (LocalPlayer.Character.HumanoidRootPart.Position - player.Character.HumanoidRootPart.Position).Magnitude
            if distance < shortestDistance and distance <= 50 then
                shortestDistance = distance
                closestPlayer = player
            end
        end
    end
    return closestPlayer
end

function Equip(weapon)
    local tool = LocalPlayer.Backpack:FindFirstChild(weapon)
    if tool then
        LocalPlayer.Character.Humanoid:EquipTool(tool)
    end
end

function UseSkill(skillKey)
    keypress(Enum.KeyCode[skillKey])
    wait(0.2)
    keyrelease(Enum.KeyCode[skillKey])
end

function DoCombo()
    local enemy = GetClosestPlayer()
    if enemy then
        local hrp = enemy.Character:FindFirstChild("HumanoidRootPart")
        if hrp then
            LocalPlayer.Character.HumanoidRootPart.CFrame = hrp.CFrame * CFrame.new(0, 0, 2)

            Equip("Godhuman") UseSkill("Z") wait(0.5)
            UseSkill("X") wait(0.6)

            Equip("Soul Guitar") UseSkill("Z") wait(0.4)

            Equip("Dual Cursed Katana") UseSkill("X") wait(0.4)

            Equip("Magma-Magma") UseSkill("X") wait(0.5)

            Equip("Godhuman") UseSkill("C") wait(0.3)

            Equip("Dual Cursed Katana") UseSkill("Z") wait(0.3)

            Equip("Soul Guitar") UseSkill("X") wait(0.3)
        end
    end
end

spawn(function()
    while true do
        if AutoCombo then
            DoCombo()
        end
        wait(4.5)
    end
end)

Mouse.KeyDown:Connect(function(key)
    if key == "k" then
        AutoCombo = not AutoCombo
        print("Auto Combo:", AutoCombo and "BẬT" or "TẮT")
    end
end)
