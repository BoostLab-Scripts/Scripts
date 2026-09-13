--[[
    BoostLab - FPS Booster (Rayfield Edition)
    Reduces client-side graphics settings to improve FPS.
    Includes: polished intro animation, part/instance streaming control,
    graphics reduction, and a "block new parts" toggle to stop new
    parts/effects from spawning into the workspace while enabled.
]]

local Players = game:GetService("Players")
local Lighting = game:GetService("Lighting")
local Workspace = game:GetService("Workspace")
local RunService = game:GetService("RunService")
local TweenService = game:GetService("TweenService")
local Settings = settings()

local LocalPlayer = Players.LocalPlayer

local Rayfield = loadstring(game:HttpGet('https://sirius.menu/rayfield'))()

-- ============================================================
-- INTRO ANIMATION - BoostLab polished startup
-- ============================================================
local function playIntro()
	local playerGui = LocalPlayer:WaitForChild("PlayerGui")
	local old = playerGui:FindFirstChild("BoostLabIntro")
	if old then old:Destroy() end

	local gui = Instance.new("ScreenGui")
	gui.Name = "BoostLabIntro"
	gui.IgnoreGuiInset = true
	gui.ResetOnSpawn = false
	gui.DisplayOrder = 99999
	gui.Parent = playerGui

	local root = Instance.new("Frame")
	root.Size = UDim2.fromScale(1, 1)
	root.BackgroundColor3 = Color3.fromRGB(4, 2, 9)
	root.BorderSizePixel = 0
	root.Parent = gui

	local bgGradient = Instance.new("UIGradient")
	bgGradient.Color = ColorSequence.new({
		ColorSequenceKeypoint.new(0, Color3.fromRGB(5, 2, 12)),
		ColorSequenceKeypoint.new(0.48, Color3.fromRGB(31, 7, 54)),
		ColorSequenceKeypoint.new(1, Color3.fromRGB(4, 2, 10)),
	})
	bgGradient.Rotation = 35
	bgGradient.Parent = root

	local glow = Instance.new("Frame")
	glow.AnchorPoint = Vector2.new(0.5, 0.5)
	glow.Position = UDim2.fromScale(0.5, 0.43)
	glow.Size = UDim2.fromScale(0.18, 0.18)
	glow.BackgroundColor3 = Color3.fromRGB(170, 55, 255)
	glow.BackgroundTransparency = 0.74
	glow.BorderSizePixel = 0
	glow.Parent = root
	local glowCorner = Instance.new("UICorner")
	glowCorner.CornerRadius = UDim.new(1, 0)
	glowCorner.Parent = glow
	local glowScale = Instance.new("UIScale")
	glowScale.Scale = 0.7
	glowScale.Parent = glow

	local rings = {}
	for i = 1, 3 do
		local ring = Instance.new("Frame")
		ring.AnchorPoint = Vector2.new(0.5, 0.5)
		ring.Position = UDim2.fromScale(0.5, 0.43)
		ring.Size = UDim2.fromScale(0.17 + i * 0.045, 0.17 + i * 0.045)
		ring.BackgroundTransparency = 1
		ring.BorderSizePixel = 0
		ring.Parent = root

		local stroke = Instance.new("UIStroke")
		stroke.Color = Color3.fromRGB(176, 65, 245)
		stroke.Transparency = 0.55 + i * 0.1
		stroke.Thickness = 1.5
		stroke.Parent = ring

		local corner = Instance.new("UICorner")
		corner.CornerRadius = UDim.new(1, 0)
		corner.Parent = ring
		rings[i] = ring
	end

	local logo = Instance.new("TextLabel")
	logo.AnchorPoint = Vector2.new(0.5, 0.5)
	logo.Position = UDim2.fromScale(0.5, 0.43)
	logo.Size = UDim2.fromScale(0.62, 0.14)
	logo.BackgroundTransparency = 1
	logo.Text = "FPS BOOSTER"
	logo.Font = Enum.Font.GothamBlack
	logo.TextScaled = true
	logo.TextColor3 = Color3.fromRGB(250, 241, 255)
	logo.TextStrokeColor3 = Color3.fromRGB(154, 45, 235)
	logo.TextStrokeTransparency = 0.25
	logo.TextTransparency = 1
	logo.Parent = root

	local sub = Instance.new("TextLabel")
	sub.AnchorPoint = Vector2.new(0.5, 0.5)
	sub.Position = UDim2.fromScale(0.5, 0.535)
	sub.Size = UDim2.fromScale(0.55, 0.045)
	sub.BackgroundTransparency = 1
	sub.Text = "INITIALIZING  •  PERFORMANCE BOOST"
	sub.Font = Enum.Font.GothamMedium
	sub.TextScaled = true
	sub.TextColor3 = Color3.fromRGB(205, 177, 225)
	sub.TextTransparency = 1
	sub.Parent = root

	local status = Instance.new("TextLabel")
	status.AnchorPoint = Vector2.new(0.5, 0.5)
	status.Position = UDim2.fromScale(0.5, 0.59)
	status.Size = UDim2.fromScale(0.5, 0.035)
	status.BackgroundTransparency = 1
	status.Text = "Loading modules..."
	status.Font = Enum.Font.Gotham
	status.TextScaled = true
	status.TextColor3 = Color3.fromRGB(167, 133, 190)
	status.TextTransparency = 1
	status.Parent = root

	local track = Instance.new("Frame")
	track.AnchorPoint = Vector2.new(0.5, 0.5)
	track.Position = UDim2.fromScale(0.5, 0.67)
	track.Size = UDim2.fromScale(0.34, 0.009)
	track.BackgroundColor3 = Color3.fromRGB(45, 25, 58)
	track.BackgroundTransparency = 0.2
	track.BorderSizePixel = 0
	track.Parent = root
	local trackCorner = Instance.new("UICorner")
	trackCorner.CornerRadius = UDim.new(1, 0)
	trackCorner.Parent = track

	local fill = Instance.new("Frame")
	fill.Size = UDim2.new(0, 0, 1, 0)
	fill.BackgroundColor3 = Color3.fromRGB(196, 76, 255)
	fill.BorderSizePixel = 0
	fill.Parent = track
	local fillCorner = Instance.new("UICorner")
	fillCorner.CornerRadius = UDim.new(1, 0)
	fillCorner.Parent = fill

	local percent = Instance.new("TextLabel")
	percent.AnchorPoint = Vector2.new(0.5, 0.5)
	percent.Position = UDim2.fromScale(0.5, 0.705)
	percent.Size = UDim2.fromScale(0.12, 0.03)
	percent.BackgroundTransparency = 1
	percent.Text = "0%"
	percent.Font = Enum.Font.GothamBold
	percent.TextScaled = true
	percent.TextColor3 = Color3.fromRGB(210, 180, 235)
	percent.TextTransparency = 1
	percent.Parent = root

	local statuses = {
		"Loading FPS modules...",
		"Reducing render load...",
		"Optimizing graphics...",
		"Calibrating performance...",
		"FPS Booster ready."
	}

	TweenService:Create(glowScale, TweenInfo.new(1.2, Enum.EasingStyle.Quint, Enum.EasingDirection.Out), {Scale = 1.35}):Play()
	TweenService:Create(logo, TweenInfo.new(0.7, Enum.EasingStyle.Back, Enum.EasingDirection.Out), {TextTransparency = 0}):Play()
	local _introPulse = task.spawn(function()
		while logo.Parent do
			TweenService:Create(logo, TweenInfo.new(0.9, Enum.EasingStyle.Sine, Enum.EasingDirection.InOut), {TextStrokeTransparency = 0.05, TextTransparency = 0.02}):Play()
			task.wait(0.9)
			TweenService:Create(logo, TweenInfo.new(0.9, Enum.EasingStyle.Sine, Enum.EasingDirection.InOut), {TextStrokeTransparency = 0.45, TextTransparency = 0.08}):Play()
			task.wait(0.9)
		end
	end)
	task.delay(0.18, function()
		TweenService:Create(sub, TweenInfo.new(0.55), {TextTransparency = 0.15}):Play()
	end)
	task.delay(0.3, function()
		TweenService:Create(status, TweenInfo.new(0.45), {TextTransparency = 0.2}):Play()
		TweenService:Create(percent, TweenInfo.new(0.45), {TextTransparency = 0.1}):Play()
	end)

	for i, ring in ipairs(rings) do
		task.spawn(function()
			local direction = (i % 2 == 0) and 1 or -1
			while ring.Parent do
				TweenService:Create(ring, TweenInfo.new(2.2 + i * 0.35, Enum.EasingStyle.Linear), {
					Rotation = ring.Rotation + 360 * direction
				}):Play()
				task.wait(2.2 + i * 0.35)
			end
		end)
	end

	local duration = 2.8
	local start = os.clock()
	while true do
		local alpha = math.clamp((os.clock() - start) / duration, 0, 1)
		local eased = 1 - (1 - alpha)^3
		fill.Size = UDim2.new(eased, 0, 1, 0)
		percent.Text = string.format("%d%%", math.floor(eased * 100 + 0.5))
		status.Text = statuses[math.clamp(math.floor(alpha * #statuses) + 1, 1, #statuses)]
		if alpha >= 1 then break end
		RunService.RenderStepped:Wait()
	end

	task.wait(0.18)
	TweenService:Create(root, TweenInfo.new(0.6, Enum.EasingStyle.Quad, Enum.EasingDirection.In), {BackgroundTransparency = 1}):Play()
	TweenService:Create(logo, TweenInfo.new(0.45), {TextTransparency = 1, TextStrokeTransparency = 1}):Play()
	TweenService:Create(sub, TweenInfo.new(0.35), {TextTransparency = 1}):Play()
	TweenService:Create(status, TweenInfo.new(0.25), {TextTransparency = 1}):Play()
	TweenService:Create(percent, TweenInfo.new(0.25), {TextTransparency = 1}):Play()
	TweenService:Create(track, TweenInfo.new(0.3), {BackgroundTransparency = 1}):Play()
	TweenService:Create(glow, TweenInfo.new(0.5), {BackgroundTransparency = 1}):Play()
	task.wait(0.65)
	gui:Destroy()
end

playIntro()

-- ============================================================
-- CORE FPS BOOSTER LOGIC
-- ============================================================

local originalSettings = {
	GlobalShadows = Lighting.GlobalShadows,
	Brightness = Lighting.Brightness,
	FogEnd = Lighting.FogEnd,
	Technology = Lighting.Technology,
	QualityLevel = Settings.Rendering.QualityLevel,
	MeshPartDetailLevel = Settings.Rendering.MeshPartDetailLevel,
	WaterWaveSize = Workspace.Terrain.WaterWaveSize,
	WaterWaveSpeed = Workspace.Terrain.WaterWaveSpeed,
	WaterReflectance = Workspace.Terrain.WaterReflectance,
	WaterTransparency = Workspace.Terrain.WaterTransparency,
	StreamingEnabled = Workspace.StreamingEnabled,
}

-- Flag: when true, newly added parts/instances into Workspace are
-- immediately hidden (transparency/no collision) or disabled if they
-- are purely cosmetic, to reduce rendering load from things spawning
-- during play (e.g. effects, debris, decorative clutter).
local blockNewSpawns = false
local blockedClasses = {
	Part = true,
	MeshPart = true,
	UnionOperation = true,
	Sparkles = true,
	Fire = true,
	Smoke = true,
	ParticleEmitter = true,
	Trail = true,
	Explosion = true,
}

local spawnBlockConnection = nil

local function isProtected(obj)
	-- Never touch the local player's own character
	local character = LocalPlayer.Character
	if character and (obj == character or obj:IsDescendantOf(character)) then
		return true
	end
	return false
end

-- Only destroy loose/decorative parts (no scripts inside, not tools,
-- not tied to a model with a Humanoid). This avoids breaking game logic
-- that depends on named parts still existing, while actually freeing up
-- the physics/render cost instead of just hiding it.
local function isSafeToDestroy(obj)
	if not obj:IsA("BasePart") then return true end

	-- Don't touch parts that belong to a Tool, a Model with a Humanoid,
	-- or that contain child scripts (those are almost always functional,
	-- not decorative).
	local model = obj:FindFirstAncestorOfClass("Model")
	if model and (model:FindFirstChildOfClass("Humanoid") or model:IsA("Tool")) then
		return false
	end
	if obj:FindFirstChildOfClass("Script") or obj:FindFirstChildOfClass("LocalScript") then
		return false
	end
	if obj:FindFirstAncestorOfClass("Tool") then
		return false
	end
	return true
end

local function handleNewInstance(obj)
	if not blockNewSpawns then return end
	if isProtected(obj) then return end

	task.defer(function()
		if not obj or not obj.Parent then return end
		local className = obj.ClassName
		if not blockedClasses[className] then return end

		if obj:IsA("BasePart") then
			if isSafeToDestroy(obj) then
				-- Actually remove it: hiding alone still costs physics
				-- simulation and memory, this frees both.
				pcall(function() obj:Destroy() end)
			else
				-- Fallback for parts we shouldn't fully remove: at least
				-- anchor + disable collision/rendering/shadows so it stops
				-- costing physics and draw calls, not just visibility.
				pcall(function()
					obj.Anchored = true
					obj.CanCollide = false
					obj.CanQuery = false
					obj.CanTouch = false
					obj.CastShadow = false
					obj.Transparency = 1
				end)
			end
		elseif obj:IsA("ParticleEmitter") or obj:IsA("Trail") then
			pcall(function() obj:Destroy() end)
		elseif obj:IsA("Explosion") then
			pcall(function() obj:Destroy() end)
		else
			pcall(function() obj:Destroy() end)
		end
	end)
end

local function enableSpawnBlocking()
	if spawnBlockConnection then return end
	spawnBlockConnection = Workspace.DescendantAdded:Connect(handleNewInstance)
end

local function disableSpawnBlocking()
	if spawnBlockConnection then
		spawnBlockConnection:Disconnect()
		spawnBlockConnection = nil
	end
end

local function removeInstancesOfClass(className)
	for _, obj in ipairs(Workspace:GetDescendants()) do
		if obj:IsA(className) then
			obj.Enabled = false
		end
	end
end

local function restoreInstancesOfClass(className)
	for _, obj in ipairs(Workspace:GetDescendants()) do
		if obj:IsA(className) then
			obj.Enabled = true
		end
	end
end

local function setPartsSmooth()
	for _, obj in ipairs(Workspace:GetDescendants()) do
		if obj:IsA("BasePart") then
			obj.Material = Enum.Material.SmoothPlastic
			obj.Reflectance = 0
		end
	end
end

local function applyLowGraphics()
	Lighting.GlobalShadows = false
	Lighting.FogEnd = 100000
	Lighting.Brightness = 1
	Lighting.Technology = Enum.Technology.Compatibility

	Workspace.Terrain.WaterWaveSize = 0
	Workspace.Terrain.WaterWaveSpeed = 0
	Workspace.Terrain.WaterReflectance = 0
	Workspace.Terrain.WaterTransparency = 0.5

	pcall(function()
		Settings.Rendering.QualityLevel = Enum.QualityLevel.Level01
		Settings.Rendering.MeshPartDetailLevel = Enum.MeshPartDetailLevel.DistanceBased
	end)

	removeInstancesOfClass("Sparkles")
	removeInstancesOfClass("Fire")
	removeInstancesOfClass("Smoke")
	removeInstancesOfClass("ParticleEmitter")

	setPartsSmooth()
end

local function restoreGraphics()
	Lighting.GlobalShadows = originalSettings.GlobalShadows
	Lighting.Brightness = originalSettings.Brightness
	Lighting.FogEnd = originalSettings.FogEnd
	Lighting.Technology = originalSettings.Technology

	Workspace.Terrain.WaterWaveSize = originalSettings.WaterWaveSize
	Workspace.Terrain.WaterWaveSpeed = originalSettings.WaterWaveSpeed
	Workspace.Terrain.WaterReflectance = originalSettings.WaterReflectance
	Workspace.Terrain.WaterTransparency = originalSettings.WaterTransparency

	pcall(function()
		Settings.Rendering.QualityLevel = originalSettings.QualityLevel
		Settings.Rendering.MeshPartDetailLevel = originalSettings.MeshPartDetailLevel
	end)

	restoreInstancesOfClass("Sparkles")
	restoreInstancesOfClass("Fire")
	restoreInstancesOfClass("Smoke")
	restoreInstancesOfClass("ParticleEmitter")
end

-- ============================================================
-- ULTIMATE FPS ENGINE
-- Local-only, reversible render optimization.
-- Gameplay-critical models, tools and interactive objects are protected.
-- ============================================================

local ultimateFPS = false
local ultimateRemoved = 0
local ultimateOriginalParents = {}
local ultimateStates = {}
local ultimateConnections = {}
local ultimateHiddenGuis = {}
local ultimateLightingState = {}
local ultimateSkyParents = {}
local ultimateQualityState = {}
local ultimateBlockNewObjects = true
local ultimateKnownWorkspaceRoots = {}
local ultimateBlockedObjects = {}

local function ultimateProtected(obj)
	if not obj then return true end
	local char = LocalPlayer.Character
	if char and obj:IsDescendantOf(char) then return true end
	if obj:GetAttribute("BoostLabKeep") == true or obj:GetAttribute("NoRemove") == true then return true end
	if obj:IsA("Terrain") then return true end

	local model = obj:FindFirstAncestorOfClass("Model")
	if model then
		if model:FindFirstChildOfClass("Humanoid") then return true end
		if model:FindFirstChildOfClass("Tool") then return true end
	end
	return false
end

local function ultimateTinyDecoration(part)
	if not part:IsA("BasePart") or ultimateProtected(part) then return false end

	local size = part.Size
	local volume = size.X * size.Y * size.Z

	-- Very small visual clutter only.
	if volume > 1.5 or math.max(size.X, size.Y, size.Z) > 2 then return false end

	-- Never remove anything that participates in normal gameplay physics.
	if part.CanCollide or part.CanTouch or part.CanQuery then return false end

	for _, child in ipairs(part:GetChildren()) do
		if child:IsA("WeldConstraint") or child:IsA("Weld")
			or child:IsA("Motor6D") or child:IsA("ClickDetector")
			or child:IsA("ProximityPrompt") or child:IsA("TouchTransmitter") then
			return false
		end
	end

	local ok, connected = pcall(function()
		return part:GetConnectedParts(true)
	end)
	if ok and #connected > 1 then return false end

	return true
end

local function ultimateSave(obj, state)
	if ultimateStates[obj] == nil then
		ultimateStates[obj] = state
	end
end

local function ultimateHideGui(obj)
	if obj:GetAttribute("BoostLabKeep") == true then return end

	if obj:IsA("GuiObject") and obj.Visible then
		ultimateHiddenGuis[obj] = true
		obj.Visible = false
	elseif obj:IsA("BillboardGui") or obj:IsA("SurfaceGui") then
		if obj.Enabled then
			ultimateHiddenGuis[obj] = true
			obj.Enabled = false
		end
	end
end

local function ultimateHideLighting()
	ultimateLightingState = {
		GlobalShadows = Lighting.GlobalShadows,
		Brightness = Lighting.Brightness,
		FogEnd = Lighting.FogEnd,
		EnvironmentDiffuseScale = Lighting.EnvironmentDiffuseScale,
		EnvironmentSpecularScale = Lighting.EnvironmentSpecularScale,
		ExposureCompensation = Lighting.ExposureCompensation,
	}

	Lighting.GlobalShadows = false
	Lighting.Brightness = 0
	Lighting.FogEnd = 1000000
	Lighting.EnvironmentDiffuseScale = 0
	Lighting.EnvironmentSpecularScale = 0
	Lighting.ExposureCompensation = 0

	for _, child in ipairs(Lighting:GetChildren()) do
		if child:IsA("Sky") then
			ultimateSkyParents[child] = child.Parent
			child.Parent = nil
		elseif child:IsA("PostEffect") then
			ultimateSave(child, {Enabled = child.Enabled})
			child.Enabled = false
		end
	end
end

local function ultimateRestoreLighting()
	for sky, parent in pairs(ultimateSkyParents) do
		if sky and not sky.Parent then
			pcall(function() sky.Parent = parent or Lighting end)
		end
	end
	table.clear(ultimateSkyParents)

	for property, value in pairs(ultimateLightingState) do
		pcall(function() Lighting[property] = value end)
	end
	table.clear(ultimateLightingState)
end

local function ultimateRememberWorkspaceRoots()
	table.clear(ultimateKnownWorkspaceRoots)
	for _, child in ipairs(Workspace:GetChildren()) do
		ultimateKnownWorkspaceRoots[child] = true
	end
end

local function ultimateCanBlockNewObject(obj)
	if not ultimateBlockNewObjects or not obj then return false end
	if ultimateProtected(obj) then return false end
	if obj:IsA("Terrain") then return false end

	-- Only block clearly visual-only roots. Interactive/gameplay roots stay alive.
	local hasVisual = false
	local hasInteractive = false

	local function inspect(item)
		if item:IsA("BasePart") then
			hasVisual = true
			if item.CanCollide or item.CanTouch or item.CanQuery then
				hasInteractive = true
			end
		elseif item:IsA("Decal") or item:IsA("Texture")
			or item:IsA("ParticleEmitter") or item:IsA("Trail")
			or item:IsA("Beam") or item:IsA("Smoke") or item:IsA("Fire")
			or item:IsA("Sparkles") or item:IsA("Highlight")
			or item:IsA("BillboardGui") or item:IsA("SurfaceGui") then
			hasVisual = true
		elseif item:IsA("ProximityPrompt") or item:IsA("ClickDetector")
			or item:IsA("Tool") or item:IsA("Humanoid") then
			hasInteractive = true
		end
	end

	inspect(obj)
	for _, item in ipairs(obj:GetDescendants()) do
		if item:IsA("Humanoid") or item:IsA("Tool") then
			return false
		end
		inspect(item)
	end

	return hasVisual and not hasInteractive
end

local function ultimateBlockNewWorkspaceRoot(obj)
	if not ultimateBlockNewObjects or not obj or not obj.Parent then return end
	if obj.Parent ~= Workspace then return end
	if ultimateKnownWorkspaceRoots[obj] then return end
	if not ultimateCanBlockNewObject(obj) then return end

	-- Keep a reference so disabling Ultimate can restore the object.
	ultimateBlockedObjects[obj] = true
	ultimateOriginalParents[obj] = Workspace
	obj.Parent = nil
	ultimateRemoved += 1
end

local function ultimateOptimize(obj)
	if not obj or ultimateProtected(obj) then return end

	if obj:IsA("BasePart") then
		if ultimateTinyDecoration(obj) then
			ultimateOriginalParents[obj] = obj.Parent
			obj.Parent = nil
			ultimateRemoved += 1
			return
		end

		ultimateSave(obj, {
			Material = obj.Material,
			Reflectance = obj.Reflectance,
			CastShadow = obj.CastShadow,
		})
		obj.Material = Enum.Material.SmoothPlastic
		obj.Reflectance = 0
		obj.CastShadow = false

	elseif obj:IsA("Decal") or obj:IsA("Texture") then
		ultimateSave(obj, {Transparency = obj.Transparency})
		obj.Transparency = 1

	elseif obj:IsA("ParticleEmitter") or obj:IsA("Trail")
		or obj:IsA("Beam") or obj:IsA("Smoke")
		or obj:IsA("Fire") or obj:IsA("Sparkles")
		or obj:IsA("Highlight") then

		ultimateSave(obj, {Enabled = obj.Enabled})
		obj.Enabled = false
		ultimateRemoved += 1

	elseif obj:IsA("BillboardGui") or obj:IsA("SurfaceGui") then
		ultimateHideGui(obj)
	end
end

local function ultimateOptimizeWorld()
	for _, obj in ipairs(Workspace:GetDescendants()) do
		ultimateOptimize(obj)
	end
end

local function ultimateStartConnections()
	for _, c in ipairs(ultimateConnections) do
		pcall(function() c:Disconnect() end)
	end
	table.clear(ultimateConnections)

	table.insert(ultimateConnections, Workspace.DescendantAdded:Connect(function(obj)
		if not ultimateFPS then return end

		task.defer(function()
			if not ultimateFPS or not obj or not obj.Parent then return end

			-- If a completely new top-level Workspace object appears,
			-- block it locally before it can become part of the rendered world.
			if obj.Parent == Workspace then
				ultimateBlockNewWorkspaceRoot(obj)
				return
			end

			-- For content inside an existing protected/gameplay root,
			-- only apply render optimizations.
			if obj.Parent then
				ultimateOptimize(obj)
			end
		end)
	end))

	-- PlayerGui is intentionally not modified by Ultimate.
	-- The game's UI remains visible while this mode is active.
end

local function ultimateStopConnections()
	for _, c in ipairs(ultimateConnections) do
		pcall(function() c:Disconnect() end)
	end
	table.clear(ultimateConnections)
end

local function enableUltimateFPS()
	if ultimateFPS then return end
	ultimateFPS = true
	ultimateRemoved = 0

	-- Normal booster is always part of Ultimate mode.
	applyLowGraphics()

	-- UI stays visible in Ultimate mode.
	-- Remember everything that already exists so only NEW
	-- Workspace roots are blocked after activation.
	ultimateRememberWorkspaceRoots()
	ultimateHideLighting()
	ultimateOptimizeWorld()
	ultimateStartConnections()

	Rayfield:Notify({
		Title = "Ultimate FPS Booster",
		Content = "Extreme local render optimization enabled. Hidden/optimized: " .. tostring(ultimateRemoved) .. " effects/objects.",
		Duration = 4
	})
end

local function restoreUltimate()
	ultimateFPS = false
	ultimateStopConnections()
	ultimateRestoreLighting()

	for obj, parent in pairs(ultimateOriginalParents) do
		if obj and not obj.Parent and parent then
			pcall(function() obj.Parent = parent end)
		end
	end
	table.clear(ultimateOriginalParents)
	table.clear(ultimateBlockedObjects)
	table.clear(ultimateKnownWorkspaceRoots)

	for obj, state in pairs(ultimateStates) do
		if obj and obj.Parent then
			pcall(function()
				if state.Material then
					obj.Material = state.Material
					obj.Reflectance = state.Reflectance
					obj.CastShadow = state.CastShadow
				elseif state.Transparency ~= nil then
					obj.Transparency = state.Transparency
				elseif state.Enabled ~= nil then
					obj.Enabled = state.Enabled
				end
			end)
		end
	end
	table.clear(ultimateStates)
	ultimateRemoved = 0
end

local function disableUltimateFPS()
	if not ultimateFPS then return end
	restoreUltimate()

	Rayfield:Notify({
		Title = "Ultimate FPS Booster",
		Content = "Original locally changed visuals restored.",
		Duration = 3
	})
end

-- ============================================================
-- RAYFIELD UI
-- ============================================================

local Window = Rayfield:CreateWindow({
	Name = "FPS Booster",
	LoadingTitle = "FPS Booster is loading...",
	LoadingSubtitle = "Boost your performance",
	Theme = "DarkBlue",
	ConfigurationSaving = {
		Enabled = true,
		FolderName = "FPSBoosterConfig",
		FileName = "Settings"
	},
	Discord = {
		Enabled = false
	},
	KeySystem = false
})

-- ------------------------------------------------------------
-- TAB 1: BOOST (everything performance-related, one place)
-- ------------------------------------------------------------
local BoostTab = Window:CreateTab("Boost", 4483362458)

BoostTab:CreateSection("One-Click Presets")

BoostTab:CreateButton({
	Name = "Enable FPS Boost",
	Callback = function()
		applyLowGraphics()
		Rayfield:Notify({
			Title = "FPS Booster",
			Content = "Graphics reduced. FPS should improve.",
			Duration = 4,
			Image = 4483362458,
		})
	end,
})

BoostTab:CreateButton({
	Name = "Restore Original Graphics",
	Callback = function()
		restoreGraphics()
		Rayfield:Notify({
			Title = "FPS Booster",
			Content = "Original graphics settings restored.",
			Duration = 4,
			Image = 4483362458,
		})
	end,
})

local fpsLabelQuick = BoostTab:CreateLabel("FPS: Calculating...")

BoostTab:CreateSection("Graphics")

BoostTab:CreateToggle({
	Name = "Disable Shadows",
	CurrentValue = false,
	Flag = "ShadowToggle",
	Callback = function(Value)
		Lighting.GlobalShadows = not Value
	end,
})

BoostTab:CreateToggle({
	Name = "Disable Particles (Fire/Smoke/Sparkles)",
	CurrentValue = false,
	Flag = "ParticleToggle",
	Callback = function(Value)
		if Value then
			removeInstancesOfClass("Sparkles")
			removeInstancesOfClass("Fire")
			removeInstancesOfClass("Smoke")
			removeInstancesOfClass("ParticleEmitter")
		else
			restoreInstancesOfClass("Sparkles")
			restoreInstancesOfClass("Fire")
			restoreInstancesOfClass("Smoke")
			restoreInstancesOfClass("ParticleEmitter")
		end
	end,
})

BoostTab:CreateToggle({
	Name = "Disable Streaming (load everything, no pop-in)",
	CurrentValue = originalSettings.StreamingEnabled,
	Flag = "StreamingToggle",
	Callback = function(Value)
		pcall(function()
			Workspace.StreamingEnabled = Value
		end)
	end,
})

BoostTab:CreateSlider({
	Name = "Render Distance",
	Range = {100, 5000},
	Increment = 50,
	Suffix = " Studs",
	CurrentValue = 1000,
	Flag = "RenderDistance",
	Callback = function(Value)
		pcall(function()
			Settings.Rendering.ViewDistance = Value
		end)
	end,
})

BoostTab:CreateSection("Spawn Control")

BoostTab:CreateParagraph({
	Title = "How this works",
	Content = "When enabled, new parts/effects that spawn into the game are removed or fully disabled (not just hidden), so they stop costing FPS instead of running invisibly in the background."
})

BoostTab:CreateToggle({
	Name = "Block New Spawns (stop new parts/effects)",
	CurrentValue = false,
	Flag = "BlockSpawnsToggle",
	Callback = function(Value)
		blockNewSpawns = Value
		if Value then
			enableSpawnBlocking()
			Rayfield:Notify({
				Title = "FPS Booster",
				Content = "New parts/effects will now be removed as they spawn.",
				Duration = 4,
				Image = 4483362458,
			})
		else
			disableSpawnBlocking()
			Rayfield:Notify({
				Title = "FPS Booster",
				Content = "Spawn blocking disabled.",
				Duration = 4,
				Image = 4483362458,
			})
		end
	end,
})

BoostTab:CreateSection("Workspace Load")
local instanceLabel = BoostTab:CreateLabel("Workspace Parts: 0")

-- ------------------------------------------------------------
-- TAB 2: MISC (links, socials, extras)
-- ------------------------------------------------------------
local MiscTab = Window:CreateTab("Misc", 4483362458)

MiscTab:CreateSection("Community")

MiscTab:CreateButton({
	Name = "Copy Discord Link",
	Callback = function()
		setclipboard("https://discord.gg/RfVsQcyUZp")
		Rayfield:Notify({ Title = "Copied!", Content = "Discord link copied to clipboard", Duration = 3 })
	end,
})

MiscTab:CreateButton({
	Name = "Copy YouTube Link",
	Callback = function()
		setclipboard("https://www.youtube.com/@BoostLab-dll")
		Rayfield:Notify({ Title = "Copied!", Content = "YouTube link copied to clipboard", Duration = 3 })
	end,
})

MiscTab:CreateSection("Script Info")

MiscTab:CreateParagraph({
	Title = "FPS Booster",
	Content = "Client-side performance tool. Reduces graphics load, blocks new spawns from draining FPS, and shows live stats. Made with Rayfield UI."
})

MiscTab:CreateSection("Ultimate FPS Booster")

MiscTab:CreateParagraph({
	Title = "Extreme Mode",
	Content = "Extreme local render mode. Your UI stays visible while the sky, post effects, shadows, textures, particles and tiny decorative clutter are optimized. New top-level Workspace objects can be blocked locally so newly spawned world content is not rendered.",
})

MiscTab:CreateToggle({
	Name = "Ultimate FPS Booster",
	CurrentValue = false,
	Flag = "UltimateFPSBoosterToggle",
	Callback = function(value)
		if value then
			enableUltimateFPS()
		else
			disableUltimateFPS()
		end
	end,
})

MiscTab:CreateSection("Danger Zone")

MiscTab:CreateButton({
	Name = "Unload FPS Booster",
	Callback = function()
		disableSpawnBlocking()
		if ultimateFPS then
			restoreUltimate()
		end
		restoreGraphics()
		Rayfield:Destroy()
	end,
})

-- ============================================================
-- LIVE FPS + WORKSPACE TRACKING (shared across tabs)
-- ============================================================

local frameCount = 0
local lastTime = tick()

RunService.RenderStepped:Connect(function()
	frameCount = frameCount + 1
	local now = tick()
	if now - lastTime >= 1 then
		local fpsText = "FPS: " .. frameCount
		fpsLabelQuick:Set(fpsText)
		frameCount = 0
		lastTime = now
	end
end)

task.spawn(function()
	while true do
		local count = 0
		for _, obj in ipairs(Workspace:GetDescendants()) do
			if obj:IsA("BasePart") then
				count += 1
			end
		end
		instanceLabel:Set("Workspace Parts: " .. count)
		task.wait(2)
	end
end)

Rayfield:Notify({
	Title = "FPS Booster Loaded",
	Content = "Go to 'Boost' and click 'Enable FPS Boost' to get started.",
	Duration = 5,
	Image = 4483362458,
})
