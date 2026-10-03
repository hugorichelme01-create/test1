
local Players = game:GetService("Players")
local UserInputService = game:GetService("UserInputService")
local RunService = game:GetService("RunService")
local TweenService = game:GetService("TweenService")

local player = Players.LocalPlayer

local State = {
	GodMode = false,
	InfiniteJump = false,
	AutoWin = false,
	NoClip = false,
	ESP = false
}

--==================================================
-- GUI
--==================================================

local gui = Instance.new("ScreenGui")
gui.Name = "TowerTestPanel"
gui.ResetOnSpawn = false
gui.IgnoreGuiInset = true
gui.Parent = player:WaitForChild("PlayerGui")

-- Tela de abertura
local intro = Instance.new("Frame")
intro.Size = UDim2.fromScale(1, 1)
intro.BackgroundColor3 = Color3.fromRGB(8, 8, 14)
intro.ZIndex = 100
intro.Parent = gui

local introTitle = Instance.new("TextLabel")
introTitle.Size = UDim2.new(1, 0, 0, 80)
introTitle.Position = UDim2.new(0, 0, 0.42, 0)
introTitle.BackgroundTransparency = 1
introTitle.Text = "TOWER TEST"
introTitle.TextColor3 = Color3.fromRGB(255, 255, 255)
introTitle.TextTransparency = 1
introTitle.TextSize = 42
introTitle.Font = Enum.Font.GothamBlack
introTitle.ZIndex = 101
introTitle.Parent = intro

TweenService:Create(
	introTitle,
	TweenInfo.new(0.6, Enum.EasingStyle.Quad),
	{TextTransparency = 0}
):Play()

task.wait(1)

TweenService:Create(
	introTitle,
	TweenInfo.new(0.5),
	{TextTransparency = 1}
):Play()

TweenService:Create(
	intro,
	TweenInfo.new(0.7),
	{BackgroundTransparency = 1}
):Play()

task.wait(0.7)
intro:Destroy()

--==================================================
-- PAINEL
--==================================================

local panel = Instance.new("Frame")
panel.Name = "MainPanel"
panel.Size = UDim2.fromOffset(330, 430)
panel.Position = UDim2.new(0.5, -165, 0.5, -215)
panel.BackgroundColor3 = Color3.fromRGB(18, 19, 29)
panel.BorderSizePixel = 0
panel.Parent = gui

local panelCorner = Instance.new("UICorner")
panelCorner.CornerRadius = UDim.new(0, 20)
panelCorner.Parent = panel

local panelStroke = Instance.new("UIStroke")
panelStroke.Color = Color3.fromRGB(120, 80, 255)
panelStroke.Thickness = 1.5
panelStroke.Transparency = 0.25
panelStroke.Parent = panel

--==================================================
-- HEADER
--==================================================

local header = Instance.new("Frame")
header.Size = UDim2.new(1, 0, 0, 80)
header.BackgroundTransparency = 1
header.Parent = panel

local title = Instance.new("TextLabel")
title.Size = UDim2.new(1, -30, 0, 35)
title.Position = UDim2.fromOffset(15, 12)
title.BackgroundTransparency = 1
title.Text = "TOWER TEST"
title.TextColor3 = Color3.fromRGB(255, 255, 255)
title.TextSize = 24
title.Font = Enum.Font.GothamBlack
title.TextXAlignment = Enum.TextXAlignment.Left
title.Parent = header

local subtitle = Instance.new("TextLabel")
subtitle.Size = UDim2.new(1, -30, 0, 20)
subtitle.Position = UDim2.fromOffset(15, 45)
subtitle.BackgroundTransparency = 1
subtitle.Text = "CONTROL PANEL • TEST MODE"
subtitle.TextColor3 = Color3.fromRGB(140, 145, 165)
subtitle.TextSize = 11
subtitle.Font = Enum.Font.GothamBold
subtitle.TextXAlignment = Enum.TextXAlignment.Left
subtitle.Parent = header

--==================================================
-- BOTÕES
--==================================================

local function createButton(name, icon, y)
	local button = Instance.new("TextButton")
	button.Name = name
	button.Size = UDim2.new(1, -30, 0, 52)
	button.Position = UDim2.fromOffset(15, y)
	button.BackgroundColor3 = Color3.fromRGB(30, 32, 46)
	button.BorderSizePixel = 0
	button.Text = ""
	button.AutoButtonColor = false
	button.Parent = panel

	local corner = Instance.new("UICorner")
	corner.CornerRadius = UDim.new(0, 12)
	corner.Parent = button

	local iconLabel = Instance.new("TextLabel")
	iconLabel.Size = UDim2.fromOffset(40, 52)
	iconLabel.Position = UDim2.fromOffset(8, 0)
	iconLabel.BackgroundTransparency = 1
	iconLabel.Text = icon
	iconLabel.TextSize = 20
	iconLabel.Font = Enum.Font.GothamBold
	iconLabel.Parent = button

	local text = Instance.new("TextLabel")
	text.Size = UDim2.new(1, -120, 1, 0)
	text.Position = UDim2.fromOffset(52, 0)
	text.BackgroundTransparency = 1
	text.Text = name
	text.TextColor3 = Color3.fromRGB(240, 240, 250)
	text.TextSize = 14
	text.Font = Enum.Font.GothamBold
	text.TextXAlignment = Enum.TextXAlignment.Left
	text.Parent = button

	local status = Instance.new("TextLabel")
	status.Size = UDim2.fromOffset(48, 25)
	status.Position = UDim2.new(1, -60, 0.5, -12)
	status.BackgroundColor3 = Color3.fromRGB(60, 62, 78)
	status.Text = "OFF"
	status.TextColor3 = Color3.fromRGB(180, 182, 195)
	status.TextSize = 10
	status.Font = Enum.Font.GothamBold
	status.Parent = button

	local statusCorner = Instance.new("UICorner")
	statusCorner.CornerRadius = UDim.new(0, 8)
	statusCorner.Parent = status

	button.MouseEnter:Connect(function()
		TweenService:Create(
			button,
			TweenInfo.new(0.15),
			{BackgroundColor3 = Color3.fromRGB(42, 44, 62)}
		):Play()
	end)

	button.MouseLeave:Connect(function()
		TweenService:Create(
			button,
			TweenInfo.new(0.15),
			{BackgroundColor3 = Color3.fromRGB(30, 32, 46)}
		):Play()
	end)

	return button, status
end

local godButton, godStatus =
	createButton("God Mode", "🛡️", 90)

local jumpButton, jumpStatus =
	createButton("Infinite Jump", "🦘", 148)

local winButton, winStatus =
	createButton("Auto Win", "🏆", 206)

local noclipButton, noclipStatus =
	createButton("NoClip", "👻", 264)

local espButton, espStatus =
	createButton("Player ESP", "👁️", 322)

--==================================================
-- STATUS
--==================================================

local statusLabel = Instance.new("TextLabel")
statusLabel.Size = UDim2.new(1, -30, 0, 25)
statusLabel.Position = UDim2.fromOffset(15, 388)
statusLabel.BackgroundTransparency = 1
statusLabel.Text = "● SYSTEM READY"
statusLabel.TextColor3 = Color3.fromRGB(70, 220, 130)
statusLabel.TextSize = 11
statusLabel.Font = Enum.Font.GothamBold
statusLabel.TextXAlignment = Enum.TextXAlignment.Left
statusLabel.Parent = panel

--==================================================
-- STATUS DOS BOTÕES
--==================================================

local function setButtonStatus(status, enabled)
	if enabled then
		status.Text = "ON"

		TweenService:Create(
			status,
			TweenInfo.new(0.2),
			{
				BackgroundColor3 = Color3.fromRGB(55, 190, 105),
				TextColor3 = Color3.new(1, 1, 1)
			}
		):Play()
	else
		status.Text = "OFF"

		TweenService:Create(
			status,
			TweenInfo.new(0.2),
			{
				BackgroundColor3 = Color3.fromRGB(60, 62, 78),
				TextColor3 = Color3.fromRGB(180, 182, 195)
			}
		):Play()
	end
end

local function updateStatus(text)
	statusLabel.Text = "● " .. text
end

--==================================================
-- GOD MODE
--==================================================

local function applyGodMode()
	local character = player.Character
	local humanoid = character and character:FindFirstChildOfClass("Humanoid")

	if not humanoid then
		return
	end

	if State.GodMode then
		humanoid.MaxHealth = math.huge
		humanoid.Health = math.huge
	else
		humanoid.MaxHealth = 100
		humanoid.Health = 100
	end
end

godButton.MouseButton1Click:Connect(function()
	State.GodMode = not State.GodMode

	setButtonStatus(godStatus, State.GodMode)
	applyGodMode()

	updateStatus(
		State.GodMode
		and "GOD MODE ENABLED"
		or "GOD MODE DISABLED"
	)
end)

--==================================================
-- INFINITE JUMP
--==================================================

UserInputService.JumpRequest:Connect(function()
	if not State.InfiniteJump then
		return
	end

	local character = player.Character
	local humanoid = character and character:FindFirstChildOfClass("Humanoid")

	if humanoid then
		humanoid:ChangeState(Enum.HumanoidStateType.Jumping)
	end
end)

jumpButton.MouseButton1Click:Connect(function()
	State.InfiniteJump = not State.InfiniteJump

	setButtonStatus(jumpStatus, State.InfiniteJump)

	updateStatus(
		State.InfiniteJump
		and "INFINITE JUMP ENABLED"
		or "INFINITE JUMP DISABLED"
	)
end)

--==================================================
-- AUTO WIN
--==================================================

winButton.MouseButton1Click:Connect(function()
	State.AutoWin = not State.AutoWin

	setButtonStatus(winStatus, State.AutoWin)

	if State.AutoWin then
		local finish = workspace:FindFirstChild("Finish", true)
		local character = player.Character

		if finish and finish:IsA("BasePart") and character then
			character:PivotTo(
				finish.CFrame + Vector3.new(0, 4, 0)
			)

			updateStatus("AUTO WIN EXECUTED")
		else
			updateStatus("CREATE A PART NAMED 'Finish'")
		end
	else
		updateStatus("AUTO WIN DISABLED")
	end
end)

--==================================================
-- NOCLIP
--==================================================

noclipButton.MouseButton1Click:Connect(function()
	State.NoClip = not State.NoClip

	setButtonStatus(noclipStatus, State.NoClip)

	updateStatus(
		State.NoClip
		and "NOCLIP ENABLED"
		or "NOCLIP DISABLED"
	)
end)

RunService.Stepped:Connect(function()
	if not State.NoClip then
		return
	end

	local character = player.Character

	if character then
		for _, object in ipairs(character:GetDescendants()) do
			if object:IsA("BasePart") then
				object.CanCollide = false
			end
		end
	end
end)

--==================================================
-- ESP
--==================================================

local function addESP(target)
	if target == player then
		return
	end

	local character = target.Character

	if not character then
		return
	end

	if character:FindFirstChild("PlayerESP") then
		return
	end

	local highlight = Instance.new("Highlight")
	highlight.Name = "PlayerESP"
	highlight.FillColor = Color3.fromRGB(255, 65, 90)
	highlight.OutlineColor = Color3.fromRGB(255, 255, 255)
	highlight.FillTransparency = 0.45
	highlight.OutlineTransparency = 0
	highlight.DepthMode = Enum.HighlightDepthMode.AlwaysOnTop
	highlight.Parent = character
end

local function removeESP(target)
	local character = target.Character

	if not character then
		return
	end

	local highlight = character:FindFirstChild("PlayerESP")

	if highlight then
		highlight:Destroy()
	end
end

local function updateESP()
	for _, target in ipairs(Players:GetPlayers()) do
		if target ~= player then
			if State.ESP then
				addESP(target)
			else
				removeESP(target)
			end
		end
	end
end

espButton.MouseButton1Click:Connect(function()
	State.ESP = not State.ESP

	setButtonStatus(espStatus, State.ESP)
	updateESP()

	updateStatus(
		State.ESP
		and "PLAYER ESP ENABLED"
		or "PLAYER ESP DISABLED"
	)
end)

Players.PlayerAdded:Connect(function(target)
	target.CharacterAdded:Connect(function()
		task.wait(0.5)

		if State.ESP then
			addESP(target)
		end
	end)
end)

--==================================================
-- RESPAWN
--==================================================

player.CharacterAdded:Connect(function()
	task.wait(0.5)

	if State.GodMode then
		applyGodMode()
	end
end)

--==================================================
-- ANIMAÇÃO DO PAINEL
--==================================================

local panelOpenSize = UDim2.fromOffset(330, 430)

panel.Size = UDim2.fromOffset(250, 330)

TweenService:Create(
	panel,
	TweenInfo.new(
		0.6,
		Enum.EasingStyle.Back,
		Enum.EasingDirection.Out
	),
	{
		Size = panelOpenSize
	}
):Play()

--==================================================
-- ESCONDER / MOSTRAR COM A TECLA P
--==================================================

local panelVisible = true
local panelAnimating = false

UserInputService.InputBegan:Connect(function(input, gameProcessed)
	if gameProcessed then
		return
	end

	if input.KeyCode ~= Enum.KeyCode.P then
		return
	end

	if panelAnimating then
		return
	end

	panelAnimating = true

	if panelVisible then
		panelVisible = false

		local hideTween = TweenService:Create(
			panel,
			TweenInfo.new(
				0.3,
				Enum.EasingStyle.Quad,
				Enum.EasingDirection.In
			),
			{
				Size = UDim2.fromOffset(0, 0)
			}
		)

		hideTween:Play()
		hideTween.Completed:Wait()

		panel.Visible = false
	else
		panelVisible = true

		panel.Visible = true
		panel.Size = UDim2.fromOffset(0, 0)

		local showTween = TweenService:Create(
			panel,
			TweenInfo.new(
				0.45,
				Enum.EasingStyle.Back,
				Enum.EasingDirection.Out
			),
			{
				Size = panelOpenSize
			}
		)

		showTween:Play()
		showTween.Completed:Wait()
	end

	panelAnimating = false
end)

--==================================================
-- ARRASTAR PAINEL
--==================================================

local dragging = false
local dragStart
local startPosition

header.InputBegan:Connect(function(input)
	if input.UserInputType == Enum.UserInputType.MouseButton1 then
		dragging = true
		dragStart = input.Position
		startPosition = panel.Position
	end
end)

header.InputEnded:Connect(function(input)
	if input.UserInputType == Enum.UserInputType.MouseButton1 then
		dragging = false
	end
end)

UserInputService.InputChanged:Connect(function(input)
	if not dragging then
		return
	end

	if input.UserInputType == Enum.UserInputType.MouseMovement then
		local delta = input.Position - dragStart

		panel.Position = UDim2.new(
			startPosition.X.Scale,
			startPosition.X.Offset + delta.X,
			startPosition.Y.Scale,
			startPosition.Y.Offset + delta.Y
		)
	end
end)
