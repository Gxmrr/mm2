local player = game.Players.LocalPlayer
local gui = Instance.new("ScreenGui", player:WaitForChild("PlayerGui"))
gui.Name = "MM2AutoReset"

local frame = Instance.new("Frame", gui)
frame.Size = UDim2.new(0, 180, 0, 40)
frame.Position = UDim2.new(0, 10, 0, 10)
frame.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
frame.BackgroundTransparency = 0.2
frame.BorderSizePixel = 0

local label = Instance.new("TextLabel", frame)
label.Size = UDim2.new(1, 0, 1, 0)
label.BackgroundTransparency = 1
label.TextColor3 = Color3.fromRGB(255, 255, 255)
label.Font = Enum.Font.SourceSansBold
label.TextSize = 16
label.Text = "🔁 Ожидание раунда..."

-- Функция для получения объекта монет в GUI MM2
local function getCoinLabel()
	local mainGui = player:FindFirstChild("PlayerGui"):FindFirstChild("MainGUI")
	if not mainGui then return nil end
	local container = mainGui:FindFirstChild("CoinContainer")
	if not container then return nil end
	return container:FindFirstChild("Coins")
end

-- Извлечение числа из текста
local function getCoins(text)
	local num = text:match("%d+")
	return tonumber(num) or 0
end

-- Ожидание новой катки и счёт монет
while true do
	label.Text = "🔄 Ожидание старта..."

	-- Ждём появления GUI с монетами (значит, раунд начался)
	repeat
		wait(1)
	until getCoinLabel() ~= nil

	local coinLabel = getCoinLabel()
	local humanoid = player.Character or player.CharacterAdded:Wait()
	humanoid = humanoid:WaitForChild("Humanoid")

	-- Начинаем следить за монетами
	while coinLabel and coinLabel:IsDescendantOf(game) do
		local currentCoins = getCoins(coinLabel.Text)
		label.Text = "💰 " .. tostring(40) .. " / " .. tostring(currentCoins)

		if currentCoins >= 40 then
			label.Text = "✅ 40 / 40 — ресет..."
			wait(1)
			humanoid.Health = 0
			break
		end

		wait(0.3)
	end

	wait(5) -- подождать, пока идёт ресет и перезагрузка
end

        wait(1)
        humanoid.Health = 0
    end
end)
