-- MM2 Auto-Reset after 40 coins (KRNL Version)
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
    if coins and coins >= 40 then
        humanoid.Health = 0 -- reset
        break
    end
    wait(0.5)
end
