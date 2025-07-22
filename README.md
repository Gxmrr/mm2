-- MM2 Auto-Reset with GUI (for KRNL)

-- === Создаём простое GUI-окно ===
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "MM2AutoResetGUI"
ScreenGui.ResetOnSpawn = false
ScreenGui.Parent = game:GetService("Players").LocalPlayer:WaitForChild("PlayerGui")

local Frame = Instance.new("Frame")
Frame.Size = UDim2.new(0, 200, 0, 60)
Frame.Position = UDim2.new(0, 10, 0, 10)
Frame.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
Frame.BorderSizePixel = 0
Frame.BackgroundTransparency = 0.2
Frame.Parent = ScreenGui

local Title = Instance.new("TextLabel")
Title.Text = "🔁 MM2 Auto-Reset"
Title.Font = Enum.Font.SourceSansBold
Title.TextSize = 16
Title.TextColor3 = Color3.fromRGB(255, 255, 255)
Title.Size = UDim2.new(1, 0, 0.5, 0)
Title.BackgroundTransparency = 1
Title.Parent = Frame

local Status = Instance.new("TextLabel")
Status.Text = "⏳ Ожидание монет..."
Status.Font = Enum.Font.SourceSans
Status.TextSize = 14
Status.TextColor3 = Color3.fromRGB(180, 255, 180)
Status.Position = UDim2.new(0, 0, 0.5, 0)
Status.Size = UDim2.new(1, 0, 0.5, 0)
Status.BackgroundTransparency = 1
Status.Parent = Frame

-- === Логика авто-ресета ===
local player = game.Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local humanoid = character:WaitForChild("Humanoid")

local coinUI
repeat
    coinUI = player:FindFirstChild("PlayerGui"):FindFirstChild("MainGUI")
    wait(1)
until coinUI and coinUI:FindFirstChild("CoinContainer") and coinUI.CoinContainer:FindFirstChild("Coins")

local coinTextLabel = coinUI.CoinContainer.Coins

while true do
    local text = coinTextLabel.Text
    local coins = tonumber(text:match("%d+"))
    if coins then
        Status.Text = "💰 Монеты: " .. coins
        if coins >= 40 then
            Status.Text = "✅ 40 монет! Ресет..."
            wait(1)
            humanoid.Health = 0
            break
        end
    else
        Status.Text = "❌ Монеты не найдены"
    end
    wait(0.5)
end
