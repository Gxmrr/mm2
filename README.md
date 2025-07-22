-- MM2 Auto Reset with GUI when Coins >= 40 (KRNL version)

-- === Создание GUI интерфейса ===
local gui = Instance.new("ScreenGui", game.Players.LocalPlayer:WaitForChild("PlayerGui"))
gui.Name = "MM2AutoResetGUI"

local frame = Instance.new("Frame", gui)
frame.Size = UDim2.new(0, 200, 0, 60)
frame.Position = UDim2.new(0, 10, 0, 10)
frame.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
frame.BackgroundTransparency = 0.1
frame.BorderSizePixel = 0

local title = Instance.new("TextLabel", frame)
title.Text = "🔁 MM2 Auto Reset"
title.Font = Enum.Font.SourceSansBold
title.TextSize = 16
title.TextColor3 = Color3.fromRGB(255, 255, 255)
title.Size = UDim2.new(1, 0, 0.5, 0)
title.BackgroundTransparency = 1

local status = Instance.new("TextLabel", frame)
status.Text = "⏳ Ожидание начала..."
status.Font = Enum.Font.SourceSans
status.TextSize = 14
status.TextColor3 = Color3.fromRGB(200, 255, 200)
status.Position = UDim2.new(0, 0, 0.5, 0)
status.Size = UDim2.new(1, 0, 0.5, 0)
status.BackgroundTransparency = 1

-- === Основная логика авто-ресета ===
local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer

-- функция для поиска элемента с монетами в MM2
local function getCoinLabel()
    local gui = LocalPlayer:FindFirstChild("PlayerGui")
    if not gui then return nil end

    local main = gui:FindFirstChild("MainGUI")
    if not main then return nil end

    local container = main:FindFirstChild("CoinContainer")
    if not container then return nil end

    local label = container:FindFirstChild("Coins")
    return label
end

-- ожидание старта раунда и появления монет
local coinLabel = nil
repeat
    status.Text = "🔍 Поиск монет..."
    coinLabel = getCoinLabel()
    wait(1)
until coinLabel

status.Text = "💰 Монеты: 0"

-- отслеживаем монеты и делаем ресет на 40
local humanoid = LocalPlayer.Character:WaitForChild("Humanoid")

local function extractCoins(text)
    local num = text:match("%d+")
    return tonumber(num) or 0
end

-- следим за обновлением текста
local connection
connection = game:GetService("RunService").RenderStepped:Connect(function()
    if not coinLabel or not coinLabel:IsDescendantOf(game) then
        status.Text = "⏳ Ожидание новой катки..."
        return
    end

    local coins = extractCoins(coinLabel.Text)
    status.Text = "💰 Монеты: " .. tostring(coins)

    if coins >= 40 then
        status.Text = "✅ 40 монет! Ресет..."
        connection:Disconnect()
        wait(1)
        humanoid.Health = 0
    end
end)
