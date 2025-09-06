--[[
	WARNING: Heads up! This script has not been verified by ScriptBlox. Use at your own risk!
]]
-- LocalScript: StarterPlayer > StarterPlayerScripts
-- Criador: wendel (para uso no SEU jogo)
-- Nome do Hub: Wendell| Roube um Brainrot 1.0

local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")

local player = Players.LocalPlayer

-- === GUI PRINCIPAL ===
local gui = Instance.new("ScreenGui")
gui.Name = "wendel"
gui.ResetOnSpawn = false
gui.Parent = player:WaitForChild("PlayerGui")

-- Janela central
local mainFrame = Instance.new("Frame")
mainFrame.Size = UDim2.new(0, 280, 0, 280)
mainFrame.Position = UDim2.new(0.5, -140, 0.5, -140)
mainFrame.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
mainFrame.Visible = true
mainFrame.Parent = gui
local corner = Instance.new("UICorner", mainFrame)
corner.CornerRadius = UDim.new(0, 16)

-- Título
local title = Instance.new("TextLabel")
title.Size = UDim2.new(1, 0, 0, 36)
title.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
title.Text = "⚡ wendel ⚡"
title.TextScaled = true
title.Font = Enum.Font.GothamBold
title.TextColor3 = Color3.fromRGB(255, 255, 255)
title.Parent = mainFrame
local corner2 = Instance.new("UICorner", title)
corner2.CornerRadius = UDim.new(0, 12)

-- Botão Tirar/Voltar
local toggleUIBtn = Instance.new("TextButton")
toggleUIBtn.Size = UDim2.new(0, 120, 0, 36)
toggleUIBtn.Position = UDim2.new(0.5, -60, 0, -46)
toggleUIBtn.Text = "Tirar"
toggleUIBtn.TextScaled = true
toggleUIBtn.Font = Enum.Font.GothamBold
toggleUIBtn.BackgroundColor3 = Color3.fromRGB(200, 40, 40)
toggleUIBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
toggleUIBtn.Parent = gui
local corner3 = Instance.new("UICorner", toggleUIBtn)
corner3.CornerRadius = UDim.new(0, 12)

local uiVisible = true
toggleUIBtn.MouseButton1Click:Connect(function()
	uiVisible = not uiVisible
	mainFrame.Visible = uiVisible
	if uiVisible then
		toggleUIBtn.Text = "Tirar"
		toggleUIBtn.BackgroundColor3 = Color3.fromRGB(200, 40, 40)
	else
		toggleUIBtn.Text = "Voltar"
		toggleUIBtn.BackgroundColor3 = Color3.fromRGB(40, 200, 40)
	end
end)

-- Função para criar botões dentro da janela
local function makeButton(text, yPos)
	local btn = Instance.new("TextButton")
	btn.Size = UDim2.new(0, 240, 0, 46)
	btn.Position = UDim2.new(0, 20, 0, yPos)
	btn.Text = text
	btn.TextScaled = true
	btn.Font = Enum.Font.GothamBold
	btn.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
	btn.TextColor3 = Color3.fromRGB(255, 255, 255)
	btn.AutoButtonColor = true
	btn.Parent = mainFrame
	local corner = Instance.new("UICorner", btn)
	corner.CornerRadius = UDim.new(0, 12)
	return btn
end

-- Botões do Hub
local noclipBtn = makeButton("Noclip: OFF", 46)
local jumpBtn   = makeButton("Super Pulo (100 m): OFF", 102)
local tpBtn     = makeButton("Atravessar (7 m)", 158)
local invisBtn  = makeButton("Invisível: OFF", 214)

-- === Noclip ===
local noclipOn = false
local noclipConn
local function setNoClip(char, enabled)
	for _, part in ipairs(char:GetDescendants()) do
		if part:IsA("BasePart") then
			part.CanCollide = not enabled
		end
	end
end
local function enableNoclip()
	if noclipOn then return end
	noclipOn = true
	noclipBtn.Text = "Noclip: ON"
	noclipConn = RunService.Stepped:Connect(function()
		local char = player.Character
		if not char then return end
		setNoClip(char, true)
	end)
end
local function disableNoclip()
	noclipOn = false
	noclipBtn.Text = "Noclip: OFF"
	if noclipConn then
		noclipConn:Disconnect()
		noclipConn = nil
	end
	local char = player.Character
	if char then
		setNoClip(char, false)
	end
end
noclipBtn.MouseButton1Click:Connect(function()
	if noclipOn then disableNoclip() else enableNoclip() end
end)

-- === Super Pulo ===
local function jumpHeightForMeters(meters) return meters / 0.28 end
local superJumpOn = false
local defaultJumpHeight = 7.2
local jumpConn
local function enableSuperJump()
	if superJumpOn then return end
	superJumpOn = true
	jumpBtn.Text = "Super Pulo (100 m): ON"
	jumpConn = UserInputService.JumpRequest:Connect(function()
		local char = player.Character
		if not char then return end
		local hum = char:FindFirstChildOfClass("Humanoid")
		if not hum then return end
		defaultJumpHeight = hum.JumpHeight
		hum.UseJumpPower = false
		hum.JumpHeight = jumpHeightForMeters(100)
		hum.Jump = true
	end)
end
local function disableSuperJump()
	superJumpOn = false
	jumpBtn.Text = "Super Pulo (100 m): OFF"
	if jumpConn then jumpConn:Disconnect(); jumpConn = nil end
	local char = player.Character
	if char then
		local hum = char:FindFirstChildOfClass("Humanoid")
		if hum then hum.JumpHeight = defaultJumpHeight end
	end
end
jumpBtn.MouseButton1Click:Connect(function()
	if superJumpOn then disableSuperJump() else enableSuperJump() end
end)

-- === Atravessar ===
tpBtn.MouseButton1Click:Connect(function()
	local char = player.Character
	if char and char:FindFirstChild("HumanoidRootPart") then
		local hrp = char.HumanoidRootPart
		local lookVector = hrp.CFrame
