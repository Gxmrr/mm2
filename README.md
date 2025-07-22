local player = game.Players.LocalPlayer
local gui = Instance.new("ScreenGui", player:WaitForChild("PlayerGui"))
gui.Name = "MM2AutoResetGUI"

local frame = Instance.new("Frame", gui)
frame.Size = UDim2.new(0, 180, 0, 40)
frame.Position = UDim2.new(0, 10, 0, 10)
frame.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
frame.BackgroundTransparency = 0.2
frame.BorderSizePixel = 0

local label = Instance.new("TextLabel", frame)
label.Size = UDim2.new(1, 0, 1, 0)
label.BackgroundTransparency = 1
label.TextColor3 = Color3.fromRGB(255, 255, 255)
label.Font = Enum.Font.SourceSansBold
label.TextSize = 16
label.Text = "⏳ Ожидание раунда..."

local function getCoinLabel()
    local pg = player:FindFirstChild("PlayerGui")
    if not pg then return nil end

    local main = pg:FindFirstChild("MainGUI")
    if not main then return nil end

    local coinContainer = main:FindFirstChild("CoinContainer")
    if not coinContainer then return nil end

    return coinContainer:FindFirstChild("Coins")
end

local function getCurrentCoins(text)
    return tonumber(text:match("%d+")) or 0
end

while true do
    label.Text = "🔄 Ожидание монет..."
    
    local coinLabel = nil
    repeat
        wait(1)
        coinLabel = getCoinLabel()
    until coinLabel and coinLabel:IsA("TextLabel")

    label.Text = "💰 0 / 40"

    -- Получаем актуального персонажа и гуманоид
    local character = player.Character or player.CharacterAdded:Wait()
    local humanoid = character:WaitForChild("Humanoid")

    repeat
        if not coinLabel:IsDescendantOf(game) then
            break
        end

        local coins = getCurrentCoins(coinLabel.Text)
        label.Text = "💰 " .. tostring(coins) .. " / 40"

        if coins >= 40 then
            label.Text = "✅ 40 / 40 — ресет..."
            wait(1)
            humanoid.Health = 0
            break
        end

        wait(0.3)
    until false

    wait(5) -- немного паузы на загрузку нового раунда
end
