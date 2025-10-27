-- LocalScript (StarterGui)

local Players = game:GetService("Players")
local TweenService = game:GetService("TweenService")
local UserInputService = game:GetService("UserInputService")

local player = Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local root = character:WaitForChild("HumanoidRootPart")

-- Remove GUIs anteriores
for _, gui in pairs(player.PlayerGui:GetChildren()) do
	if gui.Name == "AutoFarmGui" then
		gui:Destroy()
	end
end

-- GUI Principal
local screenGui = Instance.new("ScreenGui")
screenGui.Name = "AutoFarmGui"
screenGui.ResetOnSpawn = false
screenGui.Parent = player:WaitForChild("PlayerGui")

local frame = Instance.new("Frame")
frame.Size = UDim2.new(0, 260, 0, 150)
frame.Position = UDim2.new(0.05, 0, 0.25, 0)
frame.BackgroundColor3 = Color3.fromRGB(20, 15, 25)
frame.BorderSizePixel = 0
frame.BackgroundTransparency = 0.15
frame.Parent = screenGui
frame.Active = true
frame.Draggable = true

-- Borda com brilho
local stroke = Instance.new("UIStroke")
stroke.Color = Color3.fromRGB(140, 80, 255)
stroke.Thickness = 2
stroke.ApplyStrokeMode = Enum.ApplyStrokeMode.Border
stroke.Parent = frame

-- Sombra
local shadow = Instance.new("ImageLabel")
shadow.Image = "rbxassetid://1316045217"
shadow.ImageColor3 = Color3.fromRGB(100, 50, 200)
shadow.ImageTransparency = 0.6
shadow.Size = UDim2.new(1, 30, 1, 30)
shadow.Position = UDim2.new(0, -15, 0, -15)
shadow.ZIndex = -1
shadow.BackgroundTransparency = 1
shadow.Parent = frame

-- Título principal
local title = Instance.new("TextLabel")
title.Text = "Brookhaven Rp 🎃"
title.Font = Enum.Font.GothamBold
title.TextSize = 20
title.BackgroundTransparency = 1
title.Size = UDim2.new(1, 0, 0, 28)
title.TextColor3 = Color3.fromRGB(255, 150, 80)
title.Parent = frame

-- Subtítulo
local subtitle = Instance.new("TextLabel")
subtitle.Text = "MeowScripts👻"
subtitle.Font = Enum.Font.GothamBold
subtitle.TextSize = 17
subtitle.BackgroundTransparency = 1
subtitle.Position = UDim2.new(0, 0, 0, 26)
subtitle.Size = UDim2.new(1, 0, 0, 20)
subtitle.TextColor3 = Color3.fromRGB(220, 180, 255)
subtitle.Parent = frame

-- Botões container
local holder = Instance.new("Frame")
holder.Size = UDim2.new(1, -20, 0, 70)
holder.Position = UDim2.new(0, 10, 0, 60)
holder.BackgroundTransparency = 1
holder.Parent = frame

local UIList = Instance.new("UIListLayout")
UIList.Padding = UDim.new(0, 8)
UIList.FillDirection = Enum.FillDirection.Vertical
UIList.HorizontalAlignment = Enum.HorizontalAlignment.Center
UIList.VerticalAlignment = Enum.VerticalAlignment.Top
UIList.Parent = holder

-- Função criar botão estilizado
local function createButton(text, color)
	local btn = Instance.new("TextButton")
	btn.Size = UDim2.new(1, 0, 0, 30)
	btn.BackgroundColor3 = color
	btn.Text = text
	btn.Font = Enum.Font.GothamBold
	btn.TextSize = 16
	btn.TextColor3 = Color3.new(1, 1, 1)
	btn.AutoButtonColor = false
	btn.Parent = holder

	local round = Instance.new("UICorner")
	round.CornerRadius = UDim.new(0, 6)
	round.Parent = btn

	local stroke = Instance.new("UIStroke")
	stroke.Color = Color3.fromRGB(255, 255, 255)
	stroke.Transparency = 0.7
	stroke.Thickness = 1.2
	stroke.Parent = btn

	btn.MouseEnter:Connect(function()
		TweenService:Create(btn, TweenInfo.new(0.25), {BackgroundColor3 = color:Lerp(Color3.new(1,1,1), 0.2)}):Play()
	end)
	btn.MouseLeave:Connect(function()
		TweenService:Create(btn, TweenInfo.new(0.25), {BackgroundColor3 = color}):Play()
	end)

	return btn
end

-- Botões
local autoFarmBtn = createButton("👻 AutoFarm", Color3.fromRGB(120, 50, 255))
local candyHuntBtn = createButton("🍬 CandyHunt", Color3.fromRGB(255, 100, 80))

-- Funções AutoFarm
local active = false
local startPos

local function getAllCandyCornParts()
	local parts = {}
	for _, folder in ipairs(workspace:GetChildren()) do
		if folder:IsA("Folder") and string.find(folder.Name, "EggHunt") then
			for _, obj in ipairs(folder:GetDescendants()) do
				if obj:IsA("BasePart") and string.find(obj.Name, "CandyCorn") then
					table.insert(parts, obj)
				end
			end
		end
	end
	return parts
end

autoFarmBtn.MouseButton1Click:Connect(function()
	active = not active
	if active then
		autoFarmBtn.Text = "👻 AutoFarm: On"
		startPos = root.Position

		task.spawn(function()
			while active do
				local candyParts = getAllCandyCornParts()
				if #candyParts == 0 then
					root.CFrame = CFrame.new(startPos)
					active = false
					autoFarmBtn.Text = "👻 AutoFarm: Off"
					break
				end

				for _, part in ipairs(candyParts) do
					if not active then break end
					root.CFrame = part.CFrame + Vector3.new(0, 10, 0)
					task.wait(1)
				end
				task.wait(0.5)
			end
		end)
	else
		autoFarmBtn.Text = "👻 AutoFarm: Off"
	end
end)

-- CandyHunt teleporte
candyHuntBtn.MouseButton1Click:Connect(function()
	root.CFrame = CFrame.new(216.448, 0.491, -160.355)
end)

-- Botão fechar
local closeBtn = Instance.new("TextButton")
closeBtn.Text = "✖"
closeBtn.Size = UDim2.new(0, 25, 0, 25)
closeBtn.Position = UDim2.new(1, -30, 0, 5)
closeBtn.BackgroundColor3 = Color3.fromRGB(255, 80, 80)
closeBtn.TextColor3 = Color3.new(1,1,1)
closeBtn.Font = Enum.Font.GothamBold
closeBtn.TextSize = 16
closeBtn.Parent = frame

local roundClose = Instance.new("UICorner", closeBtn)
roundClose.CornerRadius = UDim.new(1, 0)

closeBtn.MouseButton1Click:Connect(function()
	screenGui:Destroy()
end)
