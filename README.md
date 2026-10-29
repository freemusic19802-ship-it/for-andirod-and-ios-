-- AllInOne: AutoDefense + RoleScanner + KnifeBoost merged into one LocalScript
--========== AutoDefenseUI ==========
local function Part1()
-- AutoDefense UI Builder: creates the HUD (banner + reticle). Logic is in AutoDefenseRun.
local Players = game:GetService("Players")

local localPlayer = Players.LocalPlayer
if not localPlayer then return end

local playerGui = localPlayer:WaitForChild("PlayerGui")

local screenGui = Instance.new("ScreenGui")
screenGui.Name = "AutoDefenseHUD"
screenGui.ResetOnSpawn = false
screenGui.Parent = playerGui

-- Top Status Banner
local statusBanner = Instance.new("Frame")
statusBanner.Name = "StatusBanner"
statusBanner.Size = UDim2.new(0, 420, 0, 56)
statusBanner.Position = UDim2.new(0.5, -210, 0, 20)
statusBanner.BackgroundColor3 = Color3.fromRGB(18, 20, 28)
statusBanner.BackgroundTransparency = 0.2
statusBanner.BorderSizePixel = 0
statusBanner.Parent = screenGui

local bannerCorner = Instance.new("UICorner")
bannerCorner.CornerRadius = UDim.new(0, 10)
bannerCorner.Parent = statusBanner

local bannerStroke = Instance.new("UIStroke")
bannerStroke.Name = "BannerStroke"
bannerStroke.Color = Color3.fromRGB(50, 150, 255)
bannerStroke.Thickness = 1.5
bannerStroke.Parent = statusBanner

local statusIcon = Instance.new("TextLabel")
statusIcon.Name = "Icon"
statusIcon.Size = UDim2.new(0, 44, 1, 0)
statusIcon.BackgroundTransparency = 1
statusIcon.Text = "\u{1F6E1}\u{FE0F}"
statusIcon.TextSize = 24
statusIcon.Parent = statusBanner

local statusTitle = Instance.new("TextLabel")
statusTitle.Name = "Title"
statusTitle.Size = UDim2.new(1, -145, 0.5, 0)
statusTitle.Position = UDim2.new(0, 48, 0.08, 0)
statusTitle.BackgroundTransparency = 1
statusTitle.Text = "AUTO-DEFENSE: ACTIVE"
statusTitle.TextColor3 = Color3.fromRGB(80, 200, 255)
statusTitle.Font = Enum.Font.GothamBold
statusTitle.TextSize = 13
statusTitle.TextXAlignment = Enum.TextXAlignment.Left
statusTitle.Parent = statusBanner

local statusDesc = Instance.new("TextLabel")
statusDesc.Name = "Desc"
statusDesc.Size = UDim2.new(1, -145, 0.45, 0)
statusDesc.Position = UDim2.new(0, 48, 0.52, 0)
statusDesc.BackgroundTransparency = 1
statusDesc.Text = "Scanning perimeter (35 studs) for knife threats..."
statusDesc.TextColor3 = Color3.fromRGB(180, 190, 205)
statusDesc.Font = Enum.Font.Gotham
statusDesc.TextSize = 11
statusDesc.TextXAlignment = Enum.TextXAlignment.Left
statusDesc.Parent = statusBanner

local toggleBtn = Instance.new("TextButton")
toggleBtn.Name = "ToggleBtn"
toggleBtn.Size = UDim2.new(0, 85, 0, 34)
toggleBtn.Position = UDim2.new(1, -95, 0.5, -17)
toggleBtn.BackgroundColor3 = Color3.fromRGB(30, 80, 150)
toggleBtn.BorderSizePixel = 0
toggleBtn.Text = "ENABLED [V]"
toggleBtn.TextColor3 = Color3.fromRGB(220, 240, 255)
toggleBtn.Font = Enum.Font.GothamBold
toggleBtn.TextSize = 11
toggleBtn.Parent = statusBanner

local toggleBtnCorner = Instance.new("UICorner")
toggleBtnCorner.CornerRadius = UDim.new(0, 6)
toggleBtnCorner.Parent = toggleBtn

-- Lock-on Reticle
local reticle = Instance.new("Frame")
reticle.Name = "Reticle"
reticle.Size = UDim2.new(0, 80, 0, 80)
reticle.AnchorPoint = Vector2.new(0.5, 0.5)
reticle.BackgroundTransparency = 1
reticle.Visible = false
reticle.Parent = screenGui

local function makeBracket(pos, size)
	local f = Instance.new("Frame")
	f.BackgroundColor3 = Color3.fromRGB(255, 50, 50)
	f.BorderSizePixel = 0
	f.Position = pos
	f.Size = size
	f.Parent = reticle
	return f
end

makeBracket(UDim2.new(0, 0, 0, 0), UDim2.new(0, 18, 0, 3))
makeBracket(UDim2.new(0, 0, 0, 0), UDim2.new(0, 3, 0, 18))
makeBracket(UDim2.new(1, -18, 0, 0), UDim2.new(0, 18, 0, 3))
makeBracket(UDim2.new(1, -3, 0, 0), UDim2.new(0, 3, 0, 18))
makeBracket(UDim2.new(0, 0, 1, -3), UDim2.new(0, 18, 0, 3))
makeBracket(UDim2.new(0, 0, 1, -18), UDim2.new(0, 3, 0, 18))
makeBracket(UDim2.new(1, -18, 1, -3), UDim2.new(0, 18, 0, 3))
makeBracket(UDim2.new(1, -3, 1, -18), UDim2.new(0, 3, 0, 18))

local centerDot = Instance.new("Frame")
centerDot.Size = UDim2.new(0, 6, 0, 6)
centerDot.AnchorPoint = Vector2.new(0.5, 0.5)
centerDot.Position = UDim2.new(0.5, 0, 0.5, 0)
centerDot.BackgroundColor3 = Color3.fromRGB(255, 60, 60)
centerDot.BorderSizePixel = 0
centerDot.Parent = reticle
local dotCorner = Instance.new("UICorner")
dotCorner.CornerRadius = UDim.new(1, 0)
dotCorner.Parent = centerDot

local targetLabel = Instance.new("TextLabel")
targetLabel.Name = "TargetLabel"
targetLabel.Size = UDim2.new(0, 240, 0, 20)
targetLabel.AnchorPoint = Vector2.new(0.5, 0)
targetLabel.Position = UDim2.new(0.5, 0, 1, 8)
targetLabel.BackgroundTransparency = 1
targetLabel.Text = "[LOCKED: KNIFE THREAT]"
targetLabel.TextColor3 = Color3.fromRGB(255, 60, 60)
targetLabel.Font = Enum.Font.GothamBold
targetLabel.TextSize = 11
targetLabel.Parent = reticle

print("[AutoDefenseUI] HUD built.")

end
Part1()

--========== AutoDefenseRun ==========
local function Part2()
-- AutoDefense Logic: toggle, detection, camera lock, auto-fire. UI is built by AutoDefenseUI.
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")

local localPlayer = Players.LocalPlayer
if not localPlayer then
	warn("[AutoDefenseRun] No LocalPlayer. Make sure this is a LocalScript inside a ScreenGui.")
	return
end
local camera = workspace.CurrentCamera

local systemEnabled = true
local DETECTION_RADIUS = 35
local FIRE_COOLDOWN = 0.3
local lastShotTime = 0

-- Find the HUD built by AutoDefenseUI
local playerGui = localPlayer:WaitForChild("PlayerGui")
local hud = playerGui:WaitForChild("AutoDefenseHUD", 10)
if not hud then
	warn("[AutoDefenseRun] AutoDefenseHUD not found. Make sure the AutoDefenseUI LocalScript is complete and enabled.")
	return
end
local statusBanner = hud:WaitForChild("StatusBanner", 5)
local reticle = hud:WaitForChild("Reticle", 5)
local toggleBtn = statusBanner:WaitForChild("ToggleBtn", 5)
local bannerStroke = statusBanner:WaitForChild("BannerStroke", 5)
local statusIcon = statusBanner:WaitForChild("Icon", 5)
local statusTitle = statusBanner:WaitForChild("Title", 5)
local statusDesc = statusBanner:WaitForChild("Desc", 5)
local targetLabel = reticle:WaitForChild("TargetLabel", 5)

local function toggleSystem()
	systemEnabled = not systemEnabled
	localPlayer:SetAttribute("AutoDefenseEnabled", systemEnabled)
	if systemEnabled then
		toggleBtn.BackgroundColor3 = Color3.fromRGB(30, 80, 150)
		toggleBtn.Text = "ENABLED [V]"
		bannerStroke.Color = Color3.fromRGB(50, 150, 255)
		statusIcon.Text = "\u{1F6E1}\u{FE0F}"
		statusTitle.Text = "AUTO-DEFENSE: ACTIVE"
		statusTitle.TextColor3 = Color3.fromRGB(80, 200, 255)
		statusDesc.Text = "Scanning perimeter (35 studs) for knife threats..."
		statusDesc.TextColor3 = Color3.fromRGB(180, 190, 205)
	else
		toggleBtn.BackgroundColor3 = Color3.fromRGB(60, 60, 70)
		toggleBtn.Text = "DISABLED [V]"
		bannerStroke.Color = Color3.fromRGB(120, 120, 130)
		statusIcon.Text = "\u{26A0}\u{FE0F}"
		statusTitle.Text = "AUTO-DEFENSE: DISABLED"
		statusTitle.TextColor3 = Color3.fromRGB(180, 180, 190)
		statusDesc.Text = "System paused. Press V or click button to re-enable."
		statusDesc.TextColor3 = Color3.fromRGB(140, 140, 150)
	end
end

toggleBtn.MouseButton1Click:Connect(toggleSystem)
UserInputService.InputBegan:Connect(function(input, gameProcessed)
	if gameProcessed then return end
	if input.KeyCode == Enum.KeyCode.V then
		toggleSystem()
	end
end)

-- Helper: Check if model has a knife
local function hasKnife(model)
	if not model or not model:IsA("Model") then return false end
	for _, item in ipairs(model:GetChildren()) do
		if item:IsA("Tool") and string.find(item.Name:lower(), "knife") then
			return true
		end
	end
	if model:GetAttribute("HasKnife") == true or model:GetAttribute("WeaponType") == "Knife" then
		return true
	end
	local targetPlayer = Players:GetPlayerFromCharacter(model)
	if targetPlayer and targetPlayer:FindFirstChild("Backpack") then
		for _, item in ipairs(targetPlayer.Backpack:GetChildren()) do
			if item:IsA("Tool") and string.find(item.Name:lower(), "knife") then
				return true
			end
		end
	end
	if model:FindFirstChild("Knife") then
		return true
	end
	return false
end

-- Helper: Find nearest knife threat
local function findNearestKnifeThreat(localHrp)
	local closestModel = nil
	local closestDist = DETECTION_RADIUS
	for _, obj in ipairs(workspace:GetChildren()) do
		if obj:IsA("Model") and obj ~= localPlayer.Character then
			local hum = obj:FindFirstChildOfClass("Humanoid")
			local hrp = obj:FindFirstChild("HumanoidRootPart")
			if hum and hum.Health > 0 and hrp then
				local dist = (hrp.Position - localHrp.Position).Magnitude
				if dist <= closestDist and hasKnife(obj) then
					closestDist = dist
					closestModel = obj
				end
			end
		end
	end
	return closestModel, closestDist
end

-- Helper: Auto-equip Gun
local function ensureGunEquipped(character, humanoid)
	local currentTool = character:FindFirstChildOfClass("Tool")
	if currentTool and string.find(currentTool.Name:lower(), "gun") then
		return currentTool
	end
	local backpack = localPlayer:FindFirstChild("Backpack")
	if backpack then
		for _, item in ipairs(backpack:GetChildren()) do
			if item:IsA("Tool") and string.find(item.Name:lower(), "gun") then
				humanoid:EquipTool(item)
				return item
			end
		end
	end
	return nil
end

-- Camera Lock RenderStep
RunService:BindToRenderStep("AutoDefenseLock", Enum.RenderPriority.Camera.Value + 1, function()
	local char = localPlayer.Character
	if not char or not systemEnabled then
		reticle.Visible = false
		return
	end
	local hum = char:FindFirstChildOfClass("Humanoid")
	local hrp = char:FindFirstChild("HumanoidRootPart")
	if not hum or hum.Health <= 0 or not hrp then
		reticle.Visible = false
		return
	end
	local target, dist = findNearestKnifeThreat(hrp)
	if target then
		local targetHrp = target:FindFirstChild("HumanoidRootPart")
		local targetHead = target:FindFirstChild("Head") or targetHrp
		local targetHum = target:FindFirstChildOfClass("Humanoid")
		if targetHrp and targetHead and targetHum and targetHum.Health > 0 then
			local aimPos = targetHead.Position
			ensureGunEquipped(char, hum)
			local camPos = camera.CFrame.Position
			camera.CFrame = CFrame.new(camPos, aimPos)
			local flatDiff = Vector3.new(aimPos.X - hrp.Position.X, 0, aimPos.Z - hrp.Position.Z)
			if flatDiff.Magnitude > 0.1 then
				hrp.CFrame = CFrame.new(hrp.Position, hrp.Position + flatDiff)
			end
			local holdingGun = char:FindFirstChildOfClass("Tool")
			if holdingGun and string.find(holdingGun.Name:lower(), "gun") then
				if (tick() - lastShotTime >= FIRE_COOLDOWN) then
					local fireEvent = holdingGun:FindFirstChild("FireWeapon")
					if fireEvent then
						lastShotTime = tick()
						fireEvent:FireServer(aimPos)
					end
				end
			end
				local screenPos, onScreen = camera:WorldToViewportPoint(aimPos)
			if onScreen then
				reticle.Position = UDim2.new(0, screenPos.X, 0, screenPos.Y)
				reticle.Visible = true
				targetLabel.Text = string.format("[LOCKED: %s | %.0f STUDS | HP: %.0f]", target.Name, dist, targetHum.Health)
			else
				reticle.Visible = false
			end
			bannerStroke.Color = Color3.fromRGB(255, 60, 60)
			statusBanner.BackgroundColor3 = Color3.fromRGB(35, 15, 15)
			statusIcon.Text = "\u{1F3AF}"
			statusTitle.Text = "THREAT ENGAGED: " .. string.upper(target.Name)
			statusTitle.TextColor3 = Color3.fromRGB(255, 70, 70)
			statusDesc.Text = string.format("Knife threat detected %.1f studs away! Auto-firing...", dist)
			statusDesc.TextColor3 = Color3.fromRGB(255, 190, 190)
			return
		end
	end
	reticle.Visible = false
	if systemEnabled then
		bannerStroke.Color = Color3.fromRGB(50, 150, 255)
		statusBanner.BackgroundColor3 = Color3.fromRGB(18, 20, 28)
		statusIcon.Text = "\u{1F6E1}\u{FE0F}"
		statusTitle.Text = "AUTO-DEFENSE: ACTIVE"
		statusTitle.TextColor3 = Color3.fromRGB(80, 200, 255)
		statusDesc.Text = "Scanning perimeter (35 studs) for knife threats..."
		statusDesc.TextColor3 = Color3.fromRGB(180, 190, 205)
	end
end)

localPlayer:SetAttribute("AutoDefenseEnabled", systemEnabled)
print("[AutoDefenseRun] Active. Press V to toggle.")

end
Part2()

--========== RoleScannerUI ==========
local function Part3()
-- RoleScanner UI Part 1: main HUD panel. Part 2 (RoleScannerUIExtra) builds the target card.
local screenGui = script.Parent

local COLOR_NO_WEAPON = Color3.fromRGB(46, 213, 115)
local COLOR_GUN = Color3.fromRGB(0, 168, 255)
local COLOR_KNIFE = Color3.fromRGB(255, 45, 85)

local hudFrame = Instance.new("Frame")
hudFrame.Name = "HUDFrame"
hudFrame.Size = UDim2.new(0, 310, 0, 270)
hudFrame.Position = UDim2.new(1, -330, 0, 45)
hudFrame.BackgroundColor3 = Color3.fromRGB(18, 22, 30)
hudFrame.BackgroundTransparency = 0.12
hudFrame.BorderSizePixel = 0
hudFrame.Parent = screenGui

local hudCorner = Instance.new("UICorner")
hudCorner.CornerRadius = UDim.new(0, 10)
hudCorner.Parent = hudFrame

local hudStroke = Instance.new("UIStroke")
hudStroke.Color = Color3.fromRGB(55, 70, 95)
hudStroke.Thickness = 1.5
hudStroke.Parent = hudFrame

local header = Instance.new("Frame")
header.Name = "Header"
header.Size = UDim2.new(1, 0, 0, 36)
header.BackgroundColor3 = Color3.fromRGB(26, 33, 46)
header.BorderSizePixel = 0
header.Parent = hudFrame

local headerCorner = Instance.new("UICorner")
headerCorner.CornerRadius = UDim.new(0, 10)
headerCorner.Parent = header

local titleLabel = Instance.new("TextLabel")
titleLabel.Size = UDim2.new(1, -90, 1, 0)
titleLabel.Position = UDim2.new(0, 12, 0, 0)
titleLabel.BackgroundTransparency = 1
titleLabel.Text = "ROLE SCANNER & HIGHLIGHT"
titleLabel.TextColor3 = Color3.fromRGB(240, 245, 255)
titleLabel.TextSize = 12
titleLabel.Font = Enum.Font.GothamBold
titleLabel.TextXAlignment = Enum.TextXAlignment.Left
titleLabel.Parent = header

local minBtn = Instance.new("TextButton")
minBtn.Name = "MinBtn"
minBtn.Size = UDim2.new(0, 26, 0, 26)
minBtn.Position = UDim2.new(1, -32, 0, 5)
minBtn.BackgroundColor3 = Color3.fromRGB(42, 52, 70)
minBtn.Text = "-"
minBtn.TextColor3 = Color3.fromRGB(220, 225, 235)
minBtn.TextSize = 15
minBtn.Font = Enum.Font.GothamBold
minBtn.BorderSizePixel = 0
minBtn.Parent = header

local minBtnCorner = Instance.new("UICorner")
minBtnCorner.CornerRadius = UDim.new(0, 5)
minBtnCorner.Parent = minBtn

local closeBtn = Instance.new("TextButton")
closeBtn.Name = "CloseBtn"
closeBtn.Size = UDim2.new(0, 26, 0, 26)
closeBtn.Position = UDim2.new(1, -62, 0, 5)
closeBtn.BackgroundColor3 = Color3.fromRGB(120, 40, 45)
closeBtn.Text = "X"
closeBtn.TextColor3 = Color3.fromRGB(255, 220, 220)
closeBtn.TextSize = 13
closeBtn.Font = Enum.Font.GothamBold
closeBtn.BorderSizePixel = 0
closeBtn.Parent = header

local closeBtnCorner = Instance.new("UICorner")
closeBtnCorner.CornerRadius = UDim.new(0, 5)
closeBtnCorner.Parent = closeBtn

local content = Instance.new("Frame")
content.Name = "Content"
content.Size = UDim2.new(1, -20, 1, -44)
content.Position = UDim2.new(0, 10, 0, 40)
content.BackgroundTransparency = 1
content.Parent = hudFrame

local hintLabel = Instance.new("TextLabel")
hintLabel.Size = UDim2.new(1, 0, 0, 18)
hintLabel.BackgroundTransparency = 1
hintLabel.Text = "Click any Player or NPC to Highlight"
hintLabel.TextColor3 = Color3.fromRGB(170, 185, 210)
hintLabel.TextSize = 11
hintLabel.Font = Enum.Font.GothamMedium
hintLabel.TextXAlignment = Enum.TextXAlignment.Left
hintLabel.Parent = content

local legendFrame = Instance.new("Frame")
legendFrame.Name = "LegendFrame"
legendFrame.Size = UDim2.new(1, 0, 0, 38)
legendFrame.Position = UDim2.new(0, 0, 0, 22)
legendFrame.BackgroundColor3 = Color3.fromRGB(12, 15, 22)
legendFrame.BackgroundTransparency = 0.5
legendFrame.BorderSizePixel = 0
legendFrame.Parent = content

local legendCorner = Instance.new("UICorner")
legendCorner.CornerRadius = UDim.new(0, 6)
legendCorner.Parent = legendFrame

local function makeLegendItem(parent, label, color, xPos, width)
	local badge = Instance.new("Frame")
	badge.Size = UDim2.new(0, width, 0, 22)
	badge.Position = UDim2.new(0, xPos, 0, 8)
	badge.BackgroundColor3 = color
	badge.BackgroundTransparency = 0.8
	badge.BorderSizePixel = 0
	badge.Parent = parent
	local c = Instance.new("UICorner")
	c.CornerRadius = UDim.new(0, 5)
	c.Parent = badge
	local st = Instance.new("UIStroke")
	st.Color = color
	st.Thickness = 1.2
	st.Parent = badge
	local txt = Instance.new("TextLabel")
	txt.Size = UDim2.new(1, 0, 1, 0)
	txt.BackgroundTransparency = 1
	txt.Text = label
	txt.TextColor3 = color
	txt.TextSize = 10
	txt.Font = Enum.Font.GothamBold
	txt.Parent = badge
end

makeLegendItem(legendFrame, "Unarmed", COLOR_NO_WEAPON, 6, 84)
makeLegendItem(legendFrame, "Gun", COLOR_GUN, 96, 84)
makeLegendItem(legendFrame, "Knife", COLOR_KNIFE, 186, 96)

local btnAll = Instance.new("TextButton")
btnAll.Name = "BtnHighlightAll"
btnAll.Size = UDim2.new(0.48, -4, 0, 30)
btnAll.Position = UDim2.new(0, 0, 0, 68)
btnAll.BackgroundColor3 = Color3.fromRGB(28, 95, 195)
btnAll.Text = "Highlight All"
btnAll.TextColor3 = Color3.new(1, 1, 1)
btnAll.TextSize = 11
btnAll.Font = Enum.Font.GothamBold
btnAll.BorderSizePixel = 0
btnAll.Parent = content

local btnAllCorner = Instance.new("UICorner")
btnAllCorner.CornerRadius = UDim.new(0, 6)
btnAllCorner.Parent = btnAll

local btnClear = Instance.new("TextButton")
btnClear.Name = "BtnClear"
btnClear.Size = UDim2.new(0.48, -4, 0, 30)
btnClear.Position = UDim2.new(0.52, 4, 0, 68)
btnClear.BackgroundColor3 = Color3.fromRGB(48, 58, 76)
btnClear.Text = "Clear All"
btnClear.TextColor3 = Color3.fromRGB(230, 230, 230)
btnClear.TextSize = 11
btnClear.Font = Enum.Font.GothamBold
btnClear.BorderSizePixel = 0
btnClear.Parent = content

local btnClearCorner = Instance.new("UICorner")
btnClearCorner.CornerRadius = UDim.new(0, 6)
btnClearCorner.Parent = btnClear

local btnScanner = Instance.new("TextButton")
btnScanner.Name = "BtnScanner"
btnScanner.Size = UDim2.new(1, 0, 0, 26)
btnScanner.Position = UDim2.new(0, 0, 0, 104)
btnScanner.BackgroundColor3 = Color3.fromRGB(20, 60, 40)
btnScanner.Text = "Scanner: [ON]"
btnScanner.TextColor3 = Color3.fromRGB(80, 255, 160)
btnScanner.TextSize = 11
btnScanner.Font = Enum.Font.GothamBold
btnScanner.BorderSizePixel = 0
btnScanner.Parent = content

local btnScannerCorner = Instance.new("UICorner")
btnScannerCorner.CornerRadius = UDim.new(0, 6)
btnScannerCorner.Parent = btnScanner

local btnScannerStroke = Instance.new("UIStroke")
btnScannerStroke.Color = Color3.fromRGB(40, 130, 85)
btnScannerStroke.Thickness = 1
btnScannerStroke.Parent = btnScanner

local btnXRay = Instance.new("TextButton")
btnXRay.Name = "BtnXRay"
btnXRay.Size = UDim2.new(1, 0, 0, 26)
btnXRay.Position = UDim2.new(0, 0, 0, 136)
btnXRay.BackgroundColor3 = Color3.fromRGB(24, 32, 45)
btnXRay.Text = "See Through Walls: [ON]"
btnXRay.TextColor3 = Color3.fromRGB(0, 210, 255)
btnXRay.TextSize = 11
btnXRay.Font = Enum.Font.GothamBold
btnXRay.BorderSizePixel = 0
btnXRay.Parent = content

local btnXRayCorner = Instance.new("UICorner")
btnXRayCorner.CornerRadius = UDim.new(0, 6)
btnXRayCorner.Parent = btnXRay

local btnXRayStroke = Instance.new("UIStroke")
btnXRayStroke.Color = Color3.fromRGB(45, 65, 95)
btnXRayStroke.Thickness = 1
btnXRayStroke.Parent = btnXRay

local btnDefense = Instance.new("TextButton")
btnDefense.Name = "BtnAutoDefense"
btnDefense.Size = UDim2.new(1, 0, 0, 26)
btnDefense.Position = UDim2.new(0, 0, 0, 168)
btnDefense.BackgroundColor3 = Color3.fromRGB(24, 45, 75)
btnDefense.Text = "Auto-Defense: [ON]"
btnDefense.TextColor3 = Color3.fromRGB(0, 210, 255)
btnDefense.TextSize = 11
btnDefense.Font = Enum.Font.GothamBold
btnDefense.BorderSizePixel = 0
btnDefense.Parent = content

local btnDefenseCorner = Instance.new("UICorner")
btnDefenseCorner.CornerRadius = UDim.new(0, 6)
btnDefenseCorner.Parent = btnDefense

local btnDefenseStroke = Instance.new("UIStroke")
btnDefenseStroke.Color = Color3.fromRGB(45, 85, 130)
btnDefenseStroke.Thickness = 1
btnDefenseStroke.Parent = btnDefense

local countLabel = Instance.new("TextLabel")
countLabel.Name = "CountLabel"
countLabel.Size = UDim2.new(1, 0, 0, 18)
countLabel.Position = UDim2.new(0, 0, 0, 200)
countLabel.BackgroundTransparency = 1
countLabel.Text = "Active Highlights: 0"
countLabel.TextColor3 = Color3.fromRGB(140, 155, 180)
countLabel.TextSize = 10
countLabel.Font = Enum.Font.Gotham
countLabel.TextXAlignment = Enum.TextXAlignment.Center
countLabel.Parent = content

print("[RoleScannerUI] Main HUD built.")

end
Part3()

--========== RoleScannerUIExtra ==========
local function Part4()
-- RoleScanner UI Part 2: target card, mini pill, window controls, toggle buttons.
-- Part 1 (RoleScannerUI) builds the main HUD panel. This script adds the rest.
-- Toggles communicate with RoleScannerEngine via player attributes.
local UserInputService = game:GetService("UserInputService")
local Players = game:GetService("Players")
local SoundService = game:GetService("SoundService")

local localPlayer = Players.LocalPlayer
if not localPlayer then
	local tries = 0
	repeat task.wait(0.25) localPlayer = Players.LocalPlayer tries += 1 until localPlayer or tries >= 40
end
if not localPlayer then
	warn("[RoleScannerUIExtra] No LocalPlayer found.")
	return
end

local screenGui = script.Parent

local function playFeedback(pitch)
	local s = Instance.new("Sound")
	s.SoundId = "rbxassetid://9114223170"
	s.Volume = 0.5
	s.PlaybackSpeed = pitch or 1
	s.Parent = SoundService
	s:Play()
	game:GetService("Debris"):AddItem(s, 1.5)
end

-- Wait for the main HUD built by Part 1 (up to 10 seconds)
local hudFrame = screenGui:FindFirstChild("HUDFrame", true)
local tries = 0
while not hudFrame and tries < 40 do
	task.wait(0.25)
	hudFrame = screenGui:FindFirstChild("HUDFrame", true)
	tries += 1
end
if not hudFrame then
	warn("[RoleScannerUIExtra] Main HUD not found. RoleScannerUI (Part 1) may be truncated or missing.")
	return
end
local header = hudFrame:FindFirstChild("Header")
local minBtn = header and header:FindFirstChild("MinBtn")
local closeBtn = header and header:FindFirstChild("CloseBtn")

-- Target Info Card (pop-up shown when an entity is clicked)
local targetCard = Instance.new("Frame")
targetCard.Name = "TargetCard"
targetCard.Size = UDim2.new(0, 290, 0, 95)
targetCard.Position = UDim2.new(0.5, -145, 1, -120)
targetCard.BackgroundColor3 = Color3.fromRGB(16, 20, 28)
targetCard.BackgroundTransparency = 0.1
targetCard.BorderSizePixel = 0
targetCard.Visible = false
targetCard.Parent = screenGui

local targetCardCorner = Instance.new("UICorner")
targetCardCorner.CornerRadius = UDim.new(0, 8)
targetCardCorner.Parent = targetCard

local targetCardStroke = Instance.new("UIStroke")
targetCardStroke.Name = "Stroke"
targetCardStroke.Color = Color3.fromRGB(46, 213, 115)
targetCardStroke.Thickness = 2
targetCardStroke.Parent = targetCard

local targetTitle = Instance.new("TextLabel")
targetTitle.Name = "TargetTitle"
targetTitle.Size = UDim2.new(1, -38, 0, 24)
targetTitle.Position = UDim2.new(0, 12, 0, 6)
targetTitle.BackgroundTransparency = 1
targetTitle.Text = "Target Name"
targetTitle.TextColor3 = Color3.new(1, 1, 1)
targetTitle.TextSize = 13
targetTitle.Font = Enum.Font.GothamBold
targetTitle.TextXAlignment = Enum.TextXAlignment.Left
targetTitle.Parent = targetCard

local roleBadge = Instance.new("TextLabel")
roleBadge.Name = "RoleBadge"
roleBadge.Size = UDim2.new(1, -24, 0, 22)
roleBadge.Position = UDim2.new(0, 12, 0, 32)
roleBadge.BackgroundColor3 = Color3.fromRGB(28, 34, 46)
roleBadge.BackgroundTransparency = 0.3
roleBadge.Text = "ROLE: UNARMED"
roleBadge.TextColor3 = Color3.fromRGB(46, 213, 115)
roleBadge.TextSize = 11
roleBadge.Font = Enum.Font.GothamBold
roleBadge.TextXAlignment = Enum.TextXAlignment.Center
roleBadge.Parent = targetCard

local roleCorner = Instance.new("UICorner")
roleCorner.CornerRadius = UDim.new(0, 5)
roleCorner.Parent = roleBadge

local targetDetail = Instance.new("TextLabel")
targetDetail.Name = "TargetDetail"
targetDetail.Size = UDim2.new(1, -24, 0, 20)
targetDetail.Position = UDim2.new(0, 12, 0, 62)
targetDetail.BackgroundTransparency = 1
targetDetail.Text = "Type: NPC | Weapon: None | Dist: 15 studs"
targetDetail.TextColor3 = Color3.fromRGB(180, 195, 215)
targetDetail.TextSize = 10
targetDetail.Font = Enum.Font.GothamMedium
targetDetail.TextXAlignment = Enum.TextXAlignment.Left
targetDetail.Parent = targetCard

local cardClose = Instance.new("TextButton")
cardClose.Name = "CloseBtn"
cardClose.Size = UDim2.new(0, 20, 0, 20)
cardClose.Position = UDim2.new(1, -26, 0, 8)
cardClose.BackgroundColor3 = Color3.fromRGB(36, 44, 58)
cardClose.Text = "X"
cardClose.TextColor3 = Color3.fromRGB(200, 200, 200)
cardClose.TextSize = 10
cardClose.Font = Enum.Font.GothamBold
cardClose.BorderSizePixel = 0
cardClose.Parent = targetCard

local cardCloseCorner = Instance.new("UICorner")
cardCloseCorner.CornerRadius = UDim.new(0, 4)
cardCloseCorner.Parent = cardClose

cardClose.MouseButton1Click:Connect(function()
	targetCard.Visible = false
end)

-- Minimized floating pill
local miniBtn = Instance.new("TextButton")
miniBtn.Name = "MiniPill"
miniBtn.Size = UDim2.new(0, 130, 0, 34)
miniBtn.Position = UDim2.new(1, -145, 0, 45)
miniBtn.BackgroundColor3 = Color3.fromRGB(18, 22, 30)
miniBtn.Text = "Open Scanner"
miniBtn.TextColor3 = Color3.fromRGB(0, 200, 255)
miniBtn.TextSize = 11
miniBtn.Font = Enum.Font.GothamBold
miniBtn.BorderSizePixel = 0
miniBtn.Visible = false
miniBtn.Parent = screenGui

local miniCorner = Instance.new("UICorner")
miniCorner.CornerRadius = UDim.new(0, 17)
miniCorner.Parent = miniBtn

local miniStroke = Instance.new("UIStroke")
miniStroke.Color = Color3.fromRGB(0, 180, 255)
miniStroke.Thickness = 1.5
miniStroke.Parent = miniBtn

-- Window controls
if minBtn then
	minBtn.MouseButton1Click:Connect(function()
		hudFrame.Visible = false
		miniBtn.Visible = true
	end)
end
miniBtn.MouseButton1Click:Connect(function()
	hudFrame.Visible = true
	miniBtn.Visible = false
end)
if closeBtn then
	closeBtn.MouseButton1Click:Connect(function()
		hudFrame.Visible = false
		miniBtn.Visible = false
		targetCard.Visible = false
		playFeedback(0.8)
	end)
end

-- Draggable header
local dragging = false
local dragStart, startPos
if header then
	header.InputBegan:Connect(function(input)
		if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
			dragging = true
			dragStart = input.Position
			startPos = hudFrame.Position
			input.Changed:Connect(function()
				if input.UserInputState == Enum.UserInputState.End then
					dragging = false
				end
			end)
		end
	end)
end
UserInputService.InputChanged:Connect(function(input)
	if dragging and (input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch) then
		local delta = input.Position - dragStart
		hudFrame.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)
	end
end)

-- Generic toggle button: flips a player attribute and updates its visuals
local function makeToggle(btn, attrName, label, onColor, offColor, onBg, offBg)
	if not btn then return end
	local stroke = btn:FindFirstChildOfClass("UIStroke")
	local function update()
		local on = localPlayer:GetAttribute(attrName)
		if on == nil then on = true end
		btn.Text = label .. ": [" .. (on and "ON" or "OFF") .. "]"
		if on then
			btn.TextColor3 = onColor
			btn.BackgroundColor3 = onBg
			if stroke then stroke.Color = onColor end
		else
			btn.TextColor3 = offColor
			btn.BackgroundColor3 = offBg
			if stroke then stroke.Color = offColor end
		end
	end
	btn.MouseButton1Click:Connect(function()
		local on = localPlayer:GetAttribute(attrName)
		if on == nil then on = true end
		localPlayer:SetAttribute(attrName, not on)
		playFeedback(on and 0.8 or 1.2)
		update()
	end)
	localPlayer:GetAttributeChangedSignal(attrName):Connect(update)
	update()
end

makeToggle(hudFrame:FindFirstChild("BtnScanner", true), "ScannerEnabled", "Scanner",
	Color3.fromRGB(80, 255, 160), Color3.fromRGB(180, 185, 195), Color3.fromRGB(20, 60, 40), Color3.fromRGB(28, 34, 46))
makeToggle(hudFrame:FindFirstChild("BtnXRay", true), "ScannerXRay", "See Through Walls",
	Color3.fromRGB(0, 210, 255), Color3.fromRGB(160, 170, 185), Color3.fromRGB(24, 32, 45), Color3.fromRGB(28, 34, 46))
makeToggle(hudFrame:FindFirstChild("BtnAutoDefense", true), "AutoDefenseEnabled", "Auto-Defense",
	Color3.fromRGB(0, 210, 255), Color3.fromRGB(160, 170, 185), Color3.fromRGB(24, 45, 75), Color3.fromRGB(28, 34, 46))

print("[RoleScannerUIExtra] Target card, mini pill, window controls, and toggles ready.")
end
Part4()

--========== RoleScannerEngine ==========
local function Part5()
-- RoleScannerEngine: highlight + click detection logic.
-- Depends on: RoleScannerUI (Part 1 HUD) + RoleScannerUIExtra (Part 2 card/toggles).
-- Toggles are read from player attributes set by RoleScannerUIExtra.
local Players = game:GetService("Players")
local UserInputService = game:GetService("UserInputService")
local RunService = game:GetService("RunService")
local SoundService = game:GetService("SoundService")
local localPlayer = Players.LocalPlayer
if not localPlayer then
	local tries = 0
	repeat task.wait(0.25) localPlayer = Players.LocalPlayer tries += 1 until localPlayer or tries >= 40
end
if not localPlayer then
	warn("[RoleScannerEngine] No LocalPlayer found.")
	return
end
local camera = workspace.CurrentCamera
local screenGui = script.Parent
local COLOR_NO_WEAPON = Color3.fromRGB(46, 213, 115)
local COLOR_GUN = Color3.fromRGB(0, 168, 255)
local COLOR_KNIFE = Color3.fromRGB(255, 45, 85)
local KNIFE_WORDS = {"knife", "blade", "sword", "dagger", "machete", "murder", "katana"}
local GUN_WORDS = {"gun", "pistol", "revolver", "rifle", "shotgun", "sniper", "sheriff", "firearm", "blaster"}
local activeHighlights = {}
local selectedTarget = nil
local function matchesAny(name, words)
	for _, w in ipairs(words) do
		if name:find(w) then return true end
	end
	return false
end
local function playFeedback(pitch)
	local s = Instance.new("Sound")
	s.SoundId = "rbxassetid://9114223170"
	s.Volume = 0.5
	s.PlaybackSpeed = pitch or 1
	s.Parent = SoundService
	s:Play()
	game:GetService("Debris"):AddItem(s, 1.5)
end
-- Wait (up to 10s) for a UI element built by the UI scripts
local function waitForUI(name)
	local obj = screenGui:FindFirstChild(name, true)
	local tries = 0
	while not obj and tries < 40 do
		task.wait(0.25)
		obj = screenGui:FindFirstChild(name, true)
		tries += 1
	end
	if not obj then
		warn("[RoleScannerEngine] Missing UI element: " .. name .. " (a UI script may be truncated)")
	end
	return obj
end
local hudFrame = waitForUI("HUDFrame")
local miniBtn = waitForUI("MiniPill")
local countLabel = waitForUI("CountLabel")
local btnAll = waitForUI("BtnHighlightAll")
local btnClear = waitForUI("BtnClear")
local targetCard = waitForUI("TargetCard")
local targetTitle = targetCard and targetCard:FindFirstChild("TargetTitle")
local roleBadge = targetCard and targetCard:FindFirstChild("RoleBadge")
local targetDetail = targetCard and targetCard:FindFirstChild("TargetDetail")
local cardStroke = targetCard and targetCard:FindFirstChild("Stroke")
-- Weapon classification for Players and NPCs
local function classifyEntityWeapons(model)
	local tools = {}
	for _, item in ipairs(model:GetDescendants()) do
		if item:IsA("Tool") then table.insert(tools, item) end
	end
	local player = Players:GetPlayerFromCharacter(model)
	if player and player:FindFirstChild("Backpack") then
		for _, item in ipairs(player.Backpack:GetChildren()) do
			if item:IsA("Tool") then table.insert(tools, item) end
		end
	end
	local hasKnife, knifeName = false, nil
	local hasGun, gunName = false, nil
	for _, tool in ipairs(tools) do
		local n = string.lower(tool.Name)
		local wt = tool:GetAttribute("WeaponType")
		local isKnife = wt == "Knife" or matchesAny(n, KNIFE_WORDS)
		local isGun = wt == "Gun" or matchesAny(n, GUN_WORDS)
		if isKnife then hasKnife, knifeName = true, tool.Name
		elseif isGun then hasGun, gunName = true, tool.Name end
	end
	if hasKnife then return "Knife", knifeName or "Knife", COLOR_KNIFE end
	if hasGun then return "Gun", gunName or "Gun", COLOR_GUN end
	return "Unarmed", "None", COLOR_NO_WEAPON
end
local function updateCountDisplay()
	local c = 0
	for _ in pairs(activeHighlights) do c += 1 end
	if countLabel then countLabel.Text = "Active Highlights: " .. tostring(c) end
end
local function removeHighlight(model)
	local entry = activeHighlights[model]
	if not entry then return end
	if entry.Highlight then entry.Highlight:Destroy() end
	for _, conn in ipairs(entry.Conns) do conn:Disconnect() end
	activeHighlights[model] = nil
	if selectedTarget == model and targetCard then
		targetCard.Visible = false
		selectedTarget = nil
	end
	updateCountDisplay()
end
local function applyEntityHighlight(model)
	if not model or not model:IsA("Model") then return end
	if not model:FindFirstChildOfClass("Humanoid") then return end
	local role, weaponName, color = classifyEntityWeapons(model)
	local oldEntry = activeHighlights[model]
	if oldEntry then
		for _, conn in ipairs(oldEntry.Conns) do conn:Disconnect() end
	end
	local highlight = model:FindFirstChild("RoleHighlight")
	if not highlight then
		highlight = Instance.new("Highlight")
		highlight.Name = "RoleHighlight"
		highlight.Parent = model
	end
	local xray = localPlayer:GetAttribute("ScannerXRay")
	if xray == nil then xray = true end
	highlight.Adornee = model
	highlight.FillColor = color
	highlight.FillTransparency = 0.35
	highlight.OutlineColor = Color3.new(1, 1, 1):Lerp(color, 0.4)
	highlight.OutlineTransparency = 0
	highlight.DepthMode = xray and Enum.HighlightDepthMode.AlwaysOnTop or Enum.HighlightDepthMode.Occluded
	local conns = {}
	local function reapply()
		task.wait(0.05)
		if activeHighlights[model] then applyEntityHighlight(model) end
	end
	table.insert(conns, model.ChildAdded:Connect(function(child)
		if child:IsA("Tool") then reapply() end
	end))
	table.insert(conns, model.ChildRemoved:Connect(function(child)
		if child:IsA("Tool") then reapply() end
	end))
	local player = Players:GetPlayerFromCharacter(model)
	if player and player:FindFirstChild("Backpack") then
		table.insert(conns, player.Backpack.ChildAdded:Connect(function(child)
			if child:IsA("Tool") then reapply() end
		end))
		table.insert(conns, player.Backpack.ChildRemoved:Connect(function(child)
			if child:IsA("Tool") then reapply() end
		end))
	end
	activeHighlights[model] = { Highlight = highlight, Conns = conns, Role = role, Weapon = weaponName, Color = color }
	updateCountDisplay()
	return role, weaponName, color
end
local function refreshAllDepthModes()
	local xray = localPlayer:GetAttribute("ScannerXRay")
	if xray == nil then xray = true end
	local mode = xray and Enum.HighlightDepthMode.AlwaysOnTop or Enum.HighlightDepthMode.Occluded
	for model, entry in pairs(activeHighlights) do
		if entry.Highlight and entry.Highlight.Parent then
			entry.Highlight.DepthMode = mode
		else
			activeHighlights[model] = nil
		end
	end
	updateCountDisplay()
end
local function highlightAllEntities()
	for _, player in ipairs(Players:GetPlayers()) do
		if player.Character and player.Character:FindFirstChildOfClass("Humanoid") then
			applyEntityHighlight(player.Character)
		end
	end
	for _, desc in ipairs(workspace:GetDescendants()) do
		if desc:IsA("Model") and desc:FindFirstChildOfClass("Humanoid") and not Players:GetPlayerFromCharacter(desc) then
			applyEntityHighlight(desc)
		end
	end
	playFeedback(1.2)
	updateCountDisplay()
end
local function clearAllHighlights()
	for model in pairs(activeHighlights) do removeHighlight(model) end
	if targetCard then targetCard.Visible = false end
	selectedTarget = nil
	updateCountDisplay()
	playFeedback(0.8)
end
local function showTargetCard(model)
	if not targetCard or not targetTitle or not roleBadge or not targetDetail or not cardStroke then return end
	selectedTarget = model
	local player = Players:GetPlayerFromCharacter(model)
	local role, weaponName, color = classifyEntityWeapons(model)
	targetTitle.Text = player and (player.Name .. " [Player]") or model.Name
	cardStroke.Color = color
	if role == "Knife" then
		roleBadge.Text = "THREAT: ARMED WITH KNIFE"
		roleBadge.TextColor3 = COLOR_KNIFE
		roleBadge.BackgroundColor3 = Color3.fromRGB(55, 18, 28)
	elseif role == "Gun" then
		roleBadge.Text = "SHERIFF: ARMED WITH GUN"
		roleBadge.TextColor3 = COLOR_GUN
		roleBadge.BackgroundColor3 = Color3.fromRGB(15, 35, 65)
	else
		roleBadge.Text = "INNOCENT: NO WEAPON"
		roleBadge.TextColor3 = COLOR_NO_WEAPON
		roleBadge.BackgroundColor3 = Color3.fromRGB(15, 48, 28)
	end
	local distText = ""
	local myChar = localPlayer.Character
	if myChar and myChar:FindFirstChild("HumanoidRootPart") and model:FindFirstChild("HumanoidRootPart") then
		local dist = math.floor((myChar.HumanoidRootPart.Position - model.HumanoidRootPart.Position).Magnitude)
		distText = " | Dist: " .. dist .. " studs"
	end
	targetDetail.Text = "Name: " .. model.Name .. " | Type: " .. (player and "Player" or "NPC") .. " | Weapon: " .. weaponName .. distText
	targetCard.Visible = true
end
local function getCharacterFromHit(hitInstance)
	if not hitInstance then return nil end
	local curr = hitInstance
	while curr and curr ~= workspace do
		if curr:IsA("Model") and curr:FindFirstChildOfClass("Humanoid") then
			return curr
		end
		curr = curr.Parent
	end
	return nil
end
-- Click detection + H key to reopen the HUD
UserInputService.InputBegan:Connect(function(input, gameProcessed)
	if gameProcessed then return end
	if input.KeyCode == Enum.KeyCode.H then
		if hudFrame and not hudFrame.Visible then
			hudFrame.Visible = true
			if miniBtn then miniBtn.Visible = false end
			playFeedback(1.1)
		end
		return
	end
	if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
		if localPlayer:GetAttribute("ScannerEnabled") == false then return end
		local mousePos = UserInputService:GetMouseLocation()
		local unitRay = camera:ViewportPointToRay(mousePos.X, mousePos.Y)
		local rayParams = RaycastParams.new()
		rayParams.FilterType = RaycastFilterType.Exclude
		local ignoreList = {}
		if localPlayer.Character and localPlayer.Character:FindFirstChild("Head") then
			local headDist = (camera.CFrame.Position - localPlayer.Character.Head.Position).Magnitude
			if headDist < 2.5 then table.insert(ignoreList, localPlayer.Character) end
		end
		rayParams.FilterDescendantsInstances = ignoreList
		local result = workspace:Raycast(unitRay.Origin, unitRay.Direction * 1500, rayParams)
		if result and result.Instance then
			local model = getCharacterFromHit(result.Instance)
			if model then
				if activeHighlights[model] and selectedTarget == model then
					removeHighlight(model)
					playFeedback(0.9)
				else
					applyEntityHighlight(model)
					showTargetCard(model)
					playFeedback(1.1)
				end
			end
		end
	end
end)
if btnAll then btnAll.MouseButton1Click:Connect(highlightAllEntities) end
if btnClear then btnClear.MouseButton1Click:Connect(clearAllHighlights) end
-- React to toggles made in RoleScannerUIExtra (via player attributes)
localPlayer:GetAttributeChangedSignal("ScannerXRay"):Connect(refreshAllDepthModes)
localPlayer:GetAttributeChangedSignal("ScannerEnabled"):Connect(function()
	if localPlayer:GetAttribute("ScannerEnabled") == false then
		clearAllHighlights()
	end
end)
-- Live distance/weapon update for the target card
RunService.RenderStepped:Connect(function()
	if targetCard and targetCard.Visible and selectedTarget and selectedTarget.Parent then
		local myChar = localPlayer.Character
		if myChar and myChar:FindFirstChild("HumanoidRootPart") and selectedTarget:FindFirstChild("HumanoidRootPart") then
			local dist = math.floor((myChar.HumanoidRootPart.Position - selectedTarget.HumanoidRootPart.Position).Magnitude)
			local isPlayer = Players:GetPlayerFromCharacter(selectedTarget) ~= nil
			local _, weaponName = classifyEntityWeapons(selectedTarget)
			targetDetail.Text = "Name: " .. selectedTarget.Name .. " | Type: " .. (isPlayer and "Player" or "NPC") .. " | Weapon: " .. weaponName .. " | Dist: " .. dist .. " studs"
		end
	end
end)
print("[RoleScannerEngine] Active. Highlight logic and click detection running.")
end
Part5()

--========== KnifeBoost ==========
local function Part6()
-- KnifeBoost Featur

-- 1) +20% walk speed while holding a knife
-- 2) Any player/NPC within 7 studs of the knife holder gets auto-stabbed
-- 3) Press E while holding a knife to auto-throw it at whoever you are looking at
-- Press B or click the button to toggle
local Players = game:GetService("Players")
local UserInputService = game:GetService("UserInputService")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local SoundService = game:GetService("SoundService")

local localPlayer = Players.LocalPlayer
-- Wait for the player to exist (prevents "attempt to index nil" if the script
-- starts before the player is created)
if not localPlayer then
	warn("[KnifeBoost] No LocalPlayer found. Make sure this is a LocalScript inside a ScreenGui, not a server Script.")
	return
end
local camera = workspace.CurrentCamera
local screenGui = script.Parent

local stabEvent = ReplicatedStorage:FindFirstChild("KnifeStabEvent")

local KNIFE_STAB_RANGE = 7 -- studs
local knifeBoostEnabled = true

local function playFeedback(soundId, pitch)
	local s = Instance.new("Sound")
	s.SoundId = soundId
	s.Volume = 0.5
	s.PlaybackSpeed = pitch or 1
	s.Parent = SoundService
	s:Play()
	game:GetService("Debris"):AddItem(s, 1.5)
end

-- Toggle Button (sits below the Role Scanner HUD)
local btnKnife = Instance.new("TextButton")
btnKnife.Name = "BtnKnifeBoost"
btnKnife.Size = UDim2.new(0, 310, 0, 26)
btnKnife.Position = UDim2.new(1, -330, 0, 325)
btnKnife.BackgroundColor3 = Color3.fromRGB(55, 30, 55)
btnKnife.Text = "Knife Boost: [ON]"
btnKnife.TextColor3 = Color3.fromRGB(255, 120, 160)
btnKnife.TextSize = 11
btnKnife.Font = Enum.Font.GothamBold
btnKnife.BorderSizePixel = 0
btnKnife.Parent = screenGui

local btnKnifeCorner = Instance.new("UICorner")
btnKnifeCorner.CornerRadius = UDim.new(0, 6)
btnKnifeCorner.Parent = btnKnife

local btnKnifeStroke = Instance.new("UIStroke")
btnKnifeStroke.Color = Color3.fromRGB(130, 55, 85)
btnKnifeStroke.Thickness = 1
btnKnifeStroke.Parent = btnKnife

local function updateKnifeBtn()
	if knifeBoostEnabled then
		btnKnife.Text = "Knife Boost: [ON]"
		btnKnife.TextColor3 = Color3.fromRGB(255, 120, 160)
		btnKnife.BackgroundColor3 = Color3.fromRGB(55, 30, 55)
		btnKnifeStroke.Color = Color3.fromRGB(130, 55, 85)
	else
		btnKnife.Text = "Knife Boost: [OFF]"
		btnKnife.TextColor3 = Color3.fromRGB(160, 170, 185)
		btnKnife.BackgroundColor3 = Color3.fromRGB(28, 34, 46)
		btnKnifeStroke.Color = Color3.fromRGB(50, 60, 75)
	end
end

local function toggleKnifeBoost()
	knifeBoostEnabled = not knifeBoostEnabled
	updateKnifeBtn()
	playFeedback("rbxassetid://9114223170", knifeBoostEnabled and 1.2 or 0.8)
end

btnKnife.MouseButton1Click:Connect(toggleKnifeBoost)
UserInputService.InputBegan:Connect(function(input, gameProcessed)
	if gameProcessed then return end
	if input.KeyCode == Enum.KeyCode.B then
		toggleKnifeBoost()
	end
end)
updateKnifeBtn()

-- Find the knife tool currently equipped in the local character
local function getEquippedKnife()
	local char = localPlayer.Character
	if not char then return nil end
	for _, child in ipairs(char:GetChildren()) do
		if child:IsA("Tool") and string.lower(child.Name):find("knife") then
			return child
		end
	end
	return nil
end

-- 1) Speed boost: watch for knife equip/unequip and adjust WalkSpeed
local function watchCharacterForKnife(char)
	local humanoid = char:FindFirstChildOfClass("Humanoid")
	if not humanoid then return end

	local function hookTool(tool)
		if not tool:IsA("Tool") then return end
		if not string.lower(tool.Name):find("knife") then return end
		tool.Equipped:Connect(function()
			if knifeBoostEnabled then
				task.wait(0.1) -- let the knife's own script set its speed first
				if humanoid.Parent and humanoid.Health > 0 then
					humanoid.WalkSpeed = 24 -- 20 * 1.2 = 20% faster
				end
			end
		end)
		tool.Unequipped:Connect(function()
			task.wait(0.15) -- override the knife script's unequip speed
			if humanoid.Parent and humanoid.Health > 0 then
				humanoid.WalkSpeed = 16
			end
		end)
	end

	for _, child in ipairs(char:GetChildren()) do
		hookTool(child)
	end
	char.ChildAdded:Connect(hookTool)
end

localPlayer.CharacterAdded:Connect(function(char)
	task.wait(1)
	watchCharacterForKnife(char)
end)
if localPlayer.Character then
	watchCharacterForKnife(localPlayer.Character)
end

-- 2) Proximity stab: anyone within 7 studs of the knife holder gets stabbed
-- (damage is validated and applied on the server by KnifeStabHandler)
task.spawn(function()
	while true do
		task.wait(0.3)
		if knifeBoostEnabled and stabEvent then
			local knife = getEquippedKnife()
			local char = localPlayer.Character
			if knife and char then
				local myHrp = char:FindFirstChild("HumanoidRootPart")
				if myHrp then
					for _, model in ipairs(workspace:GetChildren()) do
						if model ~= char and model:IsA("Model") then
							local hum = model:FindFirstChildOfClass("Humanoid")
							local targetHrp = model:FindFirstChild("HumanoidRootPart")
							if hum and hum.Health > 0 and targetHrp then
								if (myHrp.Position - targetHrp.Position).Magnitude <= KNIFE_STAB_RANGE then
									stabEvent:FireServer(model)
									playFeedback("rbxassetid://9114223170", 0.7)
									break -- one stab per check
								end
							end
						end
					end
				end
			end
		end
	end
end)

-- 3) Press E to auto-throw the knife at the player/NPC you are looking at
UserInputService.InputBegan:Connect(function(input, gameProcessed)
	if gameProcessed then return end
	if input.KeyCode ~= Enum.KeyCode.E then return end
	if not knifeBoostEnabled then return end

	local knife = getEquippedKnife()
	if not knife then return end

	-- Raycast from the center of the screen
	local rayParams = RaycastParams.new()
	rayParams.FilterType = RaycastFilterType.Exclude
	local ignoreList = {}
	if localPlayer.Character then
		table.insert(ignoreList, localPlayer.Character)
	end
	rayParams.FilterDescendantsInstances = ignoreList

	local result = workspace:Raycast(camera.CFrame.Position, camera.CFrame.LookVector * 1500, rayParams)
	if not result or not result.Instance then return end

	-- Climb up the ancestry to find the character model (has a Humanoid)
	local model = result.Instance:FindFirstAncestorOfClass("Model")
	while model and not model:FindFirstChildOfClass("Humanoid") do
		model = model:FindFirstAncestorOfClass("Model")
	end
	if not model or model == localPlayer.Character then return end

	local targetHrp = model:FindFirstChild("HumanoidRootPart")
	if not targetHrp then return end

	-- Fire the knife tool's built-in throw remote at the target
	local throwRemote = knife:FindFirstChild("RequestThrow")
	if throwRemote and knife.Enabled then
		throwRemote:FireServer(targetHrp.Position)
		playFeedback("rbxassetid://9114223170", 1.3)
	end
end)

print("[KnifeBoost] Speed boost, proximity stab (7 studs), and E-to-throw active.")

end
Part6()
