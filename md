local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local Mouse = LocalPlayer:GetMouse()
local AutoBounty = false
local UsingHaki = false

local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Parent = game.Players.LocalPlayer.PlayerGui
ScreenGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling

local Frame = Instance.new("Frame")
Frame.Parent = ScreenGui
Frame.Size = UDim2.new(0, 300, 0, 150)
Frame.Position = UDim2.new(0, 10, 0, 10)
Frame.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
Frame.BackgroundTransparency = 0.5

local Title = Instance.new("TextLabel")
Title.Parent = Frame
Title.Size = UDim2.new(1, 0, 0, 20)
Title.Text = "Blox Fruits - Auto Bounty"
Title.TextColor3 = Color3.fromRGB(255, 255, 255)
Title.BackgroundTransparency = 1
Title.TextSize = 16

local BountyLabel = Instance.new("TextLabel")
BountyLabel.Parent = Frame
BountyLabel.Size = UDim2.new(1, 0, 0, 40)
BountyLabel.Position = UDim2.new(0, 0, 0, 25)
BountyLabel.Text = "Bounty: " .. (LocalPlayer.leaderstats and LocalPlayer.leaderstats.Bounty and LocalPlayer.leaderstats.Bounty.Value or 0)
BountyLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
BountyLabel.BackgroundTransparency = 1
BountyLabel.TextSize = 14

local ToggleButton = Instance.new("TextButton")
ToggleButton.Parent = Frame
ToggleButton.Size = UDim2.new(1, 0, 0, 40)
ToggleButton.Position = UDim2.new(0, 0, 0, 65)
ToggleButton.Text = "Toggle Bounty Hunt"
ToggleButton.TextColor3 = Color3.fromRGB(255, 255, 255)
ToggleButton.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
ToggleButton.TextSize = 14

ToggleButton.MouseButton1Click:Connect(function()
    AutoBounty = not AutoBounty
    ToggleButton.Text = AutoBounty and "Stop Bounty Hunt" or "Start Bounty Hunt"
    ToggleButton.BackgroundColor3 = AutoBounty and Color3.fromRGB(0, 255, 0) or Color3.fromRGB(255, 0, 0)
end)

function UpdateBounty()
    local bounty = LocalPlayer.leaderstats and LocalPlayer.leaderstats.Bounty and LocalPlayer.leaderstats.Bounty.Value or 0
    BountyLabel.Text = "Bounty: " .. bounty
end

function ToggleHaki()
    if LocalPlayer.Backpack:FindFirstChild("Haki Vũ Trang") and not UsingHaki then
        local haki = LocalPlayer.Backpack:FindFirstChild("Haki Vũ Trang")
        haki:Activate()
        UsingHaki = true
    elseif UsingHaki then
        local haki = LocalPlayer.Backpack:FindFirstChild("Haki Vũ Trang")
        haki:Deactivate()
        UsingHaki = false
    end
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
            if distance < shortestDistance then
                shortestDistance = distance
                closestPlayer = player
            end
        end
    end
    return closestPlayer
end

function EquipWeapon(weapon)
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

function AutoCombo()
    local enemy = GetClosestPlayer()
    if enemy then
        ToggleHaki()
        local hrp = enemy.Character:FindFirstChild("HumanoidRootPart")
        if hrp then
            LocalPlayer.Character.HumanoidRootPart.CFrame = hrp.CFrame * CFrame.new(0, 0, 2)

            if LocalPlayer.Backpack:FindFirstChild("Godhuman") then
                EquipWeapon("Godhuman")
                UseSkill("Z") wait(0.5)
                UseSkill("X") wait(0.6)
            end

            if LocalPlayer.Backpack:FindFirstChild("Soul Guitar") then
                EquipWeapon("Soul Guitar")
                UseSkill("Z") wait(0.4)
            end

            if LocalPlayer.Backpack:FindFirstChild("Dual Cursed Katana") then
                EquipWeapon("Dual Cursed Katana")
                UseSkill("X") wait(0.4)
            end

            if LocalPlayer.Backpack:FindFirstChild("Magma-Magma") then
                EquipWeapon("Magma-Magma")
                UseSkill("X") wait(0.5)
            end
        end
    end
end

spawn(function()
    while true do
        if AutoBounty then
            AutoCombo()
        end
        wait(4.5)
    end
end)
