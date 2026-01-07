# SouzaHub.lua
--// Souza HUB - Key System (2026)

local Players = game:GetService("Players")
local HttpService = game:GetService("HttpService")
local CoreGui = game:GetService("CoreGui")

local player = Players.LocalPlayer
local KEY_FILE = "SouzaHubKey.json"

--// Key correta
local VALID_KEY = SOUZA_KEY

--// Load Saved Key
local function LoadSavedKey()
	if not SAVE_KEY then return nil end
	if isfile and isfile(KEY_FILE) then
		local data = HttpService:JSONDecode(readfile(KEY_FILE))
		return data.Key
	end
	return nil
end

--// Save Key
local function SaveKey(key)
	if SAVE_KEY and writefile then
		writefile(KEY_FILE, HttpService:JSONEncode({Key = key}))
	end
end

--// Check Key
local function IsValidKey(key)
	return key == VALID_KEY
end

--// GUI
local KeyGui = Instance.new("ScreenGui")
KeyGui.Name = "SouzaKeySystem"
KeyGui.ResetOnSpawn = false
KeyGui.Parent = CoreGui

local Frame = Instance.new("Frame", KeyGui)
Frame.Size = UDim2.new(0, 300, 0, 180)
Frame.Position = UDim2.new(0.5, -150, 0.5, -90)
Frame.BackgroundColor3 = Color3.fromRGB(22,22,22)
Frame.Active = true
Frame.Draggable = true

Instance.new("UICorner", Frame).CornerRadius = UDim.new(0,12)

local Title = Instance.new("TextLabel", Frame)
Title.Size = UDim2.new(1,0,0,40)
Title.BackgroundTransparency = 1
Title.Text = "🔐 Souza HUB - Key System"
Title.TextColor3 = Color3.fromRGB(255,80,80)
Title.Font = Enum.Font.GothamBold
Title.TextSize = 16

local Box = Instance.new("TextBox", Frame)
Box.PlaceholderText = "Digite sua Key..."
Box.Size = UDim2.new(0.9,0,0,35)
Box.Position = UDim2.new(0.05,0,0,55)
Box.BackgroundColor3 = Color3.fromRGB(30,30,30)
Box.TextColor3 = Color3.new(1,1,1)
Box.Font = Enum.Font.Gotham
Box.TextSize = 14
Box.ClearTextOnFocus = false
Instance.new("UICorner", Box).CornerRadius = UDim.new(0,8)

local Status = Instance.new("TextLabel", Frame)
Status.Size = UDim2.new(1,0,0,20)
Status.Position = UDim2.new(0,0,0,95)
Status.BackgroundTransparency = 1
Status.Text = ""
Status.Font = Enum.Font.Gotham
Status.TextSize = 13
Status.TextColor3 = Color3.fromRGB(255,80,80)

local Submit = Instance.new("TextButton", Frame)
Submit.Size = UDim2.new(0.9,0,0,35)
Submit.Position = UDim2.new(0.05,0,0,120)
Submit.BackgroundColor3 = Color3.fromRGB(180,40,40)
Submit.Text = "VALIDAR KEY"
Submit.TextColor3 = Color3.new(1,1,1)
Submit.Font = Enum.Font.GothamBold
Submit.TextSize = 14
Submit.BorderSizePixel = 0
Instance.new("UICorner", Submit).CornerRadius = UDim.new(0,8)

--// Auto check saved key
local saved = LoadSavedKey()
if saved and IsValidKey(saved) then
	KeyGui:Destroy()
	_G.SOUZA_KEY_VERIFIED = true
	return
end

--// Submit
Submit.MouseButton1Click:Connect(function()
	if IsValidKey(Box.Text) then
		Status.Text = "✅ Key válida!"
		Status.TextColor3 = Color3.fromRGB(80,255,120)
		SaveKey(Box.Text)

		task.wait(0.5)
		KeyGui:Destroy()
		_G.SOUZA_KEY_VERIFIED = true
	else
		Status.Text = "❌ Key inválida!"
		Status.TextColor3 = Color3.fromRGB(255,80,80)
	end
end)

--// Bloqueio do HUB até validar
repeat task.wait() until _G.SOUZA_KEY_VERIFIED == true
