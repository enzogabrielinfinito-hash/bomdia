-- // inc0mubomdia1 - script by inc0mu (NoClip + Fly + FPS Booster + Dinheiro Visual + Otimizado)
-- // Coloque em StarterPlayer > StarterPlayerScripts como um LocalScript

local Players = game:GetService("Players")
local Workspace = game:GetService("Workspace")
local Lighting = game:GetService("Lighting")
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")

local player = Players.LocalPlayer
local camera = Workspace.CurrentCamera

-- Remover interface anterior se já existir
local playerGui = player:WaitForChild("PlayerGui")
if playerGui:FindFirstChild("DawidFluentGui") then
	playerGui.DawidFluentGui:Destroy()
end

local screenGui = Instance.new("ScreenGui")
screenGui.Name = "DawidFluentGui"
screenGui.ResetOnSpawn = false
screenGui.DisplayOrder = 999
screenGui.Parent = playerGui

-- Janela Principal
local mainFrame = Instance.new("Frame")
mainFrame.Name = "MainFrame"
mainFrame.Size = UDim2.new(0, 560, 0, 360)
mainFrame.Position = UDim2.new(0.5, -280, 0.5, -180)
mainFrame.BackgroundColor3 = Color3.fromRGB(18, 18, 20)
mainFrame.BorderSizePixel = 0
mainFrame.Active = true
mainFrame.Draggable = true
mainFrame.Parent = screenGui

local mainCorner = Instance.new("UICorner")
mainCorner.CornerRadius = UDim.new(0, 8)
mainCorner.Parent = mainFrame

local mainStroke = Instance.new("UIStroke")
mainStroke.Color = Color3.fromRGB(38, 38, 42)
mainStroke.Thickness = 1
mainStroke.Parent = mainFrame

-- Barra Superior (Topbar)
local topBar = Instance.new("Frame")
topBar.Size = UDim2.new(1, 0, 0, 36)
topBar.BackgroundColor3 = Color3.fromRGB(18, 18, 20)
topBar.BorderSizePixel = 0
topBar.Parent = mainFrame

local topCorner = Instance.new("UICorner")
topCorner.CornerRadius = UDim.new(0, 8)
topCorner.Parent = topBar

local titleLabel = Instance.new("TextLabel")
titleLabel.Size = UDim2.new(1, -120, 1, 0)
titleLabel.Position = UDim2.new(0, 16, 0, 0)
titleLabel.BackgroundTransparency = 1
titleLabel.Text = "inc0mubomdia1  <font size='11' color='#777788'>by inc0mu</font>"
titleLabel.RichText = true
titleLabel.TextColor3 = Color3.fromRGB(230, 230, 240)
titleLabel.TextSize = 13
titleLabel.Font = Enum.Font.GothamBold
titleLabel.TextXAlignment = Enum.TextXAlignment.Left
titleLabel.Parent = topBar

-- Container de Conteúdo e Sidebar
local sidebar = Instance.new("ScrollingFrame")
sidebar.Size = UDim2.new(0, 140, 1, -36)
sidebar.Position = UDim2.new(0, 0, 0, 36)
sidebar.BackgroundColor3 = Color3.fromRGB(14, 14, 16)
sidebar.BorderSizePixel = 0
sidebar.ScrollBarThickness = 0
sidebar.Parent = mainFrame

local sidebarLayout = Instance.new("UIListLayout")
sidebarLayout.SortOrder = Enum.SortOrder.LayoutOrder
sidebarLayout.Padding = UDim.new(0, 4)
sidebarLayout.Parent = sidebar

local sidebarPadding = Instance.new("UIPadding")
sidebarPadding.PaddingTop = UDim.new(0, 10)
sidebarPadding.PaddingLeft = UDim.new(0, 8)
sidebarPadding.PaddingRight = UDim.new(0, 8)
sidebarPadding.Parent = sidebar

local container = Instance.new("Frame")
container.Size = UDim2.new(1, -140, 1, -36)
container.Position = UDim2.new(0, 140, 0, 36)
container.BackgroundTransparency = 1
container.Parent = mainFrame

-- Botão Fechar (×)
local closeBtn = Instance.new("TextButton")
closeBtn.Size = UDim2.new(0, 32, 0, 24)
closeBtn.Position = UDim2.new(1, -36, 0.5, -12)
closeBtn.BackgroundColor3 = Color3.fromRGB(18, 18, 20)
closeBtn.Text = "×"
closeBtn.TextColor3 = Color3.fromRGB(160, 160, 170)
closeBtn.TextSize = 16
closeBtn.Font = Enum.Font.GothamBold
closeBtn.Parent = topBar

local closeCorner = Instance.new("UICorner")
closeCorner.CornerRadius = UDim.new(0, 4)
closeCorner.Parent = closeBtn

closeBtn.MouseButton1Click:Connect(function()
	screenGui:Destroy()
end)

-- Botão Minimizar (—)
local minimized = false
local minimizeBtn = Instance.new("TextButton")
minimizeBtn.Size = UDim2.new(0, 32, 0, 24)
minimizeBtn.Position = UDim2.new(1, -72, 0.5, -12)
minimizeBtn.BackgroundColor3 = Color3.fromRGB(18, 18, 20)
minimizeBtn.Text = "-"
minimizeBtn.TextColor3 = Color3.fromRGB(160, 160, 170)
minimizeBtn.TextSize = 16
minimizeBtn.Font = Enum.Font.GothamBold
minimizeBtn.Parent = topBar

local minCorner = Instance.new("UICorner")
minCorner.CornerRadius = UDim.new(0, 4)
minCorner.Parent = minimizeBtn

minimizeBtn.MouseButton1Click:Connect(function()
	minimized = not minimized
	if minimized then
		container.Visible = false
		sidebar.Visible = false
		mainFrame.Size = UDim2.new(0, 560, 0, 36)
	else
		mainFrame.Size = UDim2.new(0, 560, 0, 360)
		container.Visible = true
		sidebar.Visible = true
	end
end)

-- Tecla K para Abrir/Fechar a UI inteira
UserInputService.InputBegan:Connect(function(input, gameProcessed)
	if not gameProcessed and input.KeyCode == Enum.KeyCode.K then
		mainFrame.Visible = not mainFrame.Visible
	end
end)

-- Gerenciamento de Páginas
local pages = {}

local function createPage(name)
	local page = Instance.new("ScrollingFrame")
	page.Name = name .. "Page"
	page.Size = UDim2.new(1, 0, 1, 0)
	page.BackgroundTransparency = 1
	page.BorderSizePixel = 0
	page.Visible = false
	page.ScrollBarThickness = 3
	page.ScrollBarImageColor3 = Color3.fromRGB(50, 50, 60)
	page.Parent = container

	local layout = Instance.new("UIListLayout")
	layout.SortOrder = Enum.SortOrder.LayoutOrder
	layout.Padding = UDim.new(0, 8)
	layout.Parent = page

	local padding = Instance.new("UIPadding")
	padding.PaddingTop = UDim.new(0, 12)
	padding.PaddingLeft = UDim.new(0, 14)
	padding.PaddingRight = UDim.new(0, 14)
	padding.Parent = page

	local catTitle = Instance.new("TextLabel")
	catTitle.Size = UDim2.new(1, 0, 0, 30)
	catTitle.BackgroundTransparency = 1
	catTitle.Text = name
	catTitle.TextColor3 = Color3.fromRGB(240, 240, 250)
	catTitle.TextSize = 20
	catTitle.Font = Enum.Font.GothamBold
	catTitle.TextXAlignment = Enum.TextXAlignment.Left
	catTitle.Parent = page

	pages[name] = page
	return page
end

local aimPage = createPage("Aim")
local visualPage = createPage("Visual")
local movementPage = createPage("Movement")
local miscPage = createPage("Misc")
local settingsPage = createPage("Settings")

-- Indicador Ativo da Sidebar
local activeIndicator = Instance.new("Frame")
activeIndicator.Size = UDim2.new(0, 3, 0, 18)
activeIndicator.BackgroundColor3 = Color3.fromRGB(0, 120, 212)
activeIndicator.BorderSizePixel = 0
activeIndicator.Parent = sidebar

local indCorner = Instance.new("UICorner")
indCorner.CornerRadius = UDim.new(1, 0)
indCorner.Parent = activeIndicator

local function createTabButton(name, targetPage)
	local btn = Instance.new("TextButton")
	btn.Size = UDim2.new(1, 0, 0, 32)
	btn.BackgroundColor3 = Color3.fromRGB(14, 14, 16)
	btn.TextColor3 = Color3.fromRGB(150, 150, 160)
	btn.TextSize = 12
	btn.Font = Enum.Font.GothamSemibold
	btn.Text = "     " .. name
	btn.TextXAlignment = Enum.TextXAlignment.Left
	btn.Parent = sidebar

	local corner = Instance.new("UICorner")
	corner.CornerRadius = UDim.new(0, 6)
	corner.Parent = btn

	btn.MouseButton1Click:Connect(function()
		for _, p in pairs(pages) do p.Visible = false end
		for _, b in pairs(sidebar:GetChildren()) do
			if b:IsA("TextButton") then
				b.TextColor3 = Color3.fromRGB(150, 150, 160)
			end
		end
		targetPage.Visible = true
		btn.TextColor3 = Color3.fromRGB(255, 255, 255)
		activeIndicator.Position = UDim2.new(0, 0, 0, btn.AbsolutePosition.Y - sidebar.AbsolutePosition.Y + 8)
	end)

	return btn
end

createTabButton("Aim", aimPage)
local btnVisual = createTabButton("Visual", visualPage)
btnVisual.TextColor3 = Color3.fromRGB(255, 255, 255)
visualPage.Visible = true
activeIndicator.Position = UDim2.new(0, 0, 0, 50)

createTabButton("Movement", movementPage)
createTabButton("Misc", miscPage)
createTabButton("Settings", settingsPage)

-- Função para criar Toggle Cards
local function createToggleCard(parent, title, desc, callback)
	local card = Instance.new("Frame")
	card.Size = UDim2.new(1, 0, 0, 52)
	card.BackgroundColor3 = Color3.fromRGB(24, 24, 28)
	card.BorderSizePixel = 0
	card.Parent = parent

	local cCorner = Instance.new("UICorner")
	cCorner.CornerRadius = UDim.new(0, 6)
	cCorner.Parent = card

	local tLabel = Instance.new("TextLabel")
	tLabel.Size = UDim2.new(1, -70, 0, 20)
	tLabel.Position = UDim2.new(0, 14, 0, 8)
	tLabel.BackgroundTransparency = 1
	tLabel.Text = title
	tLabel.TextColor3 = Color3.fromRGB(220, 220, 230)
	tLabel.TextSize = 12
	tLabel.Font = Enum.Font.GothamBold
	tLabel.TextXAlignment = Enum.TextXAlignment.Left
	tLabel.Parent = card

	local dLabel = Instance.new("TextLabel")
	dLabel.Size = UDim2.new(1, -70, 0, 16)
	dLabel.Position = UDim2.new(0, 14, 0, 26)
	dLabel.BackgroundTransparency = 1
	dLabel.Text = desc
	dLabel.TextColor3 = Color3.fromRGB(130, 130, 140)
	dLabel.TextSize = 11
	dLabel.Font = Enum.Font.Gotham
	dLabel.TextXAlignment = Enum.TextXAlignment.Left
	dLabel.Parent = card

	local toggleBtn = Instance.new("TextButton")
	toggleBtn.Size = UDim2.new(0, 40, 0, 20)
	toggleBtn.Position = UDim2.new(1, -50, 0.5, -10)
	toggleBtn.BackgroundColor3 = Color3.fromRGB(45, 45, 52)
	toggleBtn.Text = ""
	toggleBtn.Parent = card

	local tbCorner = Instance.new("UICorner")
	tbCorner.CornerRadius = UDim.new(1, 0)
	tbCorner.Parent = toggleBtn

	local circle = Instance.new("Frame")
	circle.Size = UDim2.new(0, 14, 0, 14)
	circle.Position = UDim2.new(0, 3, 0.5, -7)
	circle.BackgroundColor3 = Color3.fromRGB(180, 180, 190)
	circle.Parent = toggleBtn

	local ciCorner = Instance.new("UICorner")
	ciCorner.CornerRadius = UDim.new(1, 0)
	ciCorner.Parent = circle

	local active = false
	toggleBtn.MouseButton1Click:Connect(function()
		active = not active
		if active then
			toggleBtn.BackgroundColor3 = Color3.fromRGB(0, 120, 212)
			circle:TweenPosition(UDim2.new(1, -17, 0.5, -7), Enum.EasingDirection.Out, Enum.EasingStyle.Quad, 0.15, true)
		else
			toggleBtn.BackgroundColor3 = Color3.fromRGB(45, 45, 52)
			circle:TweenPosition(UDim2.new(0, 3, 0.5, -7), Enum.EasingDirection.Out, Enum.EasingStyle.Quad, 0.15, true)
		end
		callback(active)
	end)

	return card
end

-- // FUNÇÕES

-- 1. Aim (FOV)
createToggleCard(aimPage, "Field of View", "Altera o FOV da câmera para 90.", function(state)
	camera.FieldOfView = state and 90 or 70
end)

-- 2. Visual (ESP Pneus, Fullbright e FPS Booster)
createToggleCard(visualPage, "ESP Pneus", "Destaca todos os pneus no Workspace.", function(state)
	for _, obj in pairs(Workspace:GetDescendants()) do
		if obj.Name == "Pneu" or obj.Name:lower():find("pneu") then
			if state then
				if not obj:FindFirstChild("DawidESP") then
					local highlight = Instance.new("Highlight")
					highlight.Name = "DawidESP"
					highlight.Adornee = obj
					highlight.FillColor = Color3.fromRGB(255, 255, 0)
					highlight.FillTransparency = 0.4
					highlight.OutlineColor = Color3.fromRGB(255, 255, 255)
					highlight.DepthMode = Enum.HighlightDepthMode.AlwaysOnTop
					highlight.Parent = obj
				end
			else
				local h = obj:FindFirstChild("DawidESP")
				if h then h:Destroy() end
			end
		end
	end
end)

local originalAmbient = Lighting.Ambient
local originalOutdoor = Lighting.OutdoorAmbient
local originalBrightness = Lighting.Brightness
local originalFog = Lighting.FogEnd

createToggleCard(visualPage, "Fullbright & Remove Fog", "Remove nevoeiros e ilumina todo o mapa.", function(state)
	if state then
		Lighting.Ambient = Color3.fromRGB(255, 255, 255)
		Lighting.OutdoorAmbient = Color3.fromRGB(255, 255, 255)
		Lighting.Brightness = 2
		Lighting.FogEnd = 1000000
		Lighting.GlobalShadows = false
	else
		Lighting.Ambient = originalAmbient
		Lighting.OutdoorAmbient = originalOutdoor
		Lighting.Brightness = originalBrightness
		Lighting.FogEnd = originalFog
		Lighting.GlobalShadows = true
	end
end)

createToggleCard(visualPage, "FPS Booster", "Remove sombras, efeitos e texturas pesadas do jogo.", function(state)
	if state then
		Lighting.GlobalShadows = false
		Lighting.FogEnd = 999999
		
		for _, v in pairs(Lighting:GetChildren()) do
			if v:IsA("PostEffect") or v:IsA("BlurEffect") or v:IsA("ColorCorrectionEffect") or v:IsA("DepthOfFieldEffect") or v:IsA("SunRaysEffect") or v:IsA("BloomEffect") then
				v.Enabled = false
			end
		end
		
		for _, obj in pairs(Workspace:GetDescendants()) do
			if obj:IsA("BasePart") then
				obj.CastShadow = false
			elseif obj:IsA("Decal") or obj:IsA("Texture") then
				obj.Transparency = 1
			elseif obj:IsA("ParticleEmitter") or obj:IsA("Fire") or obj:IsA("Smoke") or obj:IsA("Sparkles") then
				obj.Enabled = false
			end
		end
		
		if not _G.FPSBoostConnection then
			_G.FPSBoostConnection = Workspace.DescendantAdded:Connect(function(obj)
				if obj:IsA("BasePart") then
					obj.CastShadow = false
				elseif obj:IsA("Decal") or obj:IsA("Texture") then
					obj.Transparency = 1
				elseif obj:IsA("ParticleEmitter") or obj:IsA("Fire") or obj:IsA("Smoke") or obj:IsA("Sparkles") then
					obj.Enabled = false
				end
			end)
		end
	else
		Lighting.GlobalShadows = true
		Lighting.FogEnd = originalFog
		for _, v in pairs(Lighting:GetChildren()) do
			if v:IsA("PostEffect") then
				v.Enabled = true
			end
		end
	end
end)

-- 3. Movement (WalkSpeed customizável & NoClip/Fly)
local speedCard = Instance.new("Frame")
speedCard.Size = UDim2.new(1, 0, 0, 52)
speedCard.BackgroundColor3 = Color3.fromRGB(24, 24, 28)
speedCard.BorderSizePixel = 0
speedCard.Parent = movementPage

local scCorner = Instance.new("UICorner")
scCorner.CornerRadius = UDim.new(0, 6)
scCorner.Parent = speedCard

local stLabel = Instance.new("TextLabel")
stLabel.Size = UDim2.new(1, -120, 0, 20)
stLabel.Position = UDim2.new(0, 14, 0, 8)
stLabel.BackgroundTransparency = 1
stLabel.Text = "WalkSpeed Customizável"
stLabel.TextColor3 = Color3.fromRGB(220, 220, 230)
stLabel.TextSize = 12
stLabel.Font = Enum.Font.GothamBold
stLabel.TextXAlignment = Enum.TextXAlignment.Left
stLabel.Parent = speedCard

local sdLabel = Instance.new("TextLabel")
sdLabel.Size = UDim2.new(1, -120, 0, 16)
sdLabel.Position = UDim2.new(0, 14, 0, 26)
sdLabel.BackgroundTransparency = 1
sdLabel.Text = "Define a velocidade do personagem (1-1000)."
sdLabel.TextColor3 = Color3.fromRGB(130, 130, 140)
sdLabel.TextSize = 11
sdLabel.Font = Enum.Font.Gotham
sdLabel.TextXAlignment = Enum.TextXAlignment.Left
sdLabel.Parent = speedCard

local speedInput = Instance.new("TextBox")
speedInput.Size = UDim2.new(0, 48, 0, 24)
speedInput.Position = UDim2.new(1, -100, 0.5, -12)
speedInput.BackgroundColor3 = Color3.fromRGB(36, 36, 42)
speedInput.TextColor3 = Color3.fromRGB(255, 255, 255)
speedInput.TextSize = 12
speedInput.Font = Enum.Font.Gotham
speedInput.Text = "32"
speedInput.Parent = speedCard

local siCorner = Instance.new("UICorner")
siCorner.CornerRadius = UDim.new(0, 4)
siCorner.Parent = speedInput

local speedToggleBtn = Instance.new("TextButton")
speedToggleBtn.Size = UDim2.new(0, 40, 0, 20)
speedToggleBtn.Position = UDim2.new(1, -48, 0.5, -10)
speedToggleBtn.BackgroundColor3 = Color3.fromRGB(45, 45, 52)
speedToggleBtn.Text = ""
speedToggleBtn.Parent = speedCard

local stCorner = Instance.new("UICorner")
stCorner.CornerRadius = UDim.new(1, 0)
stCorner.Parent = speedToggleBtn

local scircle = Instance.new("Frame")
scircle.Size = UDim2.new(0, 14, 0, 14)
scircle.Position = UDim2.new(0, 3, 0.5, -7)
scircle.BackgroundColor3 = Color3.fromRGB(180, 180, 190)
scircle.Parent = speedToggleBtn

local sccCorner = Instance.new("UICorner")
sccCorner.CornerRadius = UDim.new(1, 0)
sccCorner.Parent = scircle

local speedActive = false
local targetSpeed = 32

speedToggleBtn.MouseButton1Click:Connect(function()
	local num = tonumber(speedInput.Text)
	if num then
		targetSpeed = math.clamp(num, 1, 1000)
	else
		targetSpeed = 32
		speedInput.Text = "32"
	end

	speedActive = not speedActive
	if speedActive then
		speedToggleBtn.BackgroundColor3 = Color3.fromRGB(0, 120, 212)
		scircle:TweenPosition(UDim2.new(1, -17, 0.5, -7), Enum.EasingDirection.Out, Enum.EasingStyle.Quad, 0.15, true)
	else
		speedToggleBtn.BackgroundColor3 = Color3.fromRGB(45, 45, 52)
		scircle:TweenPosition(UDim2.new(0, 3, 0.5, -7), Enum.EasingDirection.Out, Enum.EasingStyle.Quad, 0.15, true)
	end
end)

RunService.Heartbeat:Connect(function()
	if speedActive then
		local char = player.Character
		if char then
			local humanoid = char:FindFirstChild("Humanoid")
			if humanoid and humanoid.WalkSpeed ~= targetSpeed then
				humanoid.WalkSpeed = targetSpeed
			end
		end
	end
end)

local noclipFlyActive = false
createToggleCard(movementPage, "NoClip + Fly", "Atravessa paredes e permite voar livremente.", function(state)
	noclipFlyActive = state
end)

RunService.Stepped:Connect(function()
	local char = player.Character
	if not char then return end

	if noclipFlyActive then
		for _, part in pairs(char:GetDescendants()) do
			if part:IsA("BasePart") then
				part.CanCollide = false
			end
		end

		local rootPart = char:FindFirstChild("HumanoidRootPart")
		if rootPart then
			rootPart.Velocity = Vector3.new(0, 1, 0)
			local camCFrame = camera.CFrame
			local moveDir = Vector3.new()
			
			if UserInputService:IsKeyDown(Enum.KeyCode.W) then
				moveDir = moveDir + camCFrame.LookVector
			end
			if UserInputService:IsKeyDown(Enum.KeyCode.S) then
				moveDir = moveDir - camCFrame.LookVector
			end
			if UserInputService:IsKeyDown(Enum.KeyCode.A) then
				moveDir = moveDir - camCFrame.RightVector
			end
			if UserInputService:IsKeyDown(Enum.KeyCode.D) then
				moveDir = moveDir + camCFrame.RightVector
			end
			
			if moveDir.Magnitude > 0 then
				rootPart.CFrame = rootPart.CFrame + (moveDir.Unit * 1.5)
			end
		end
	end
end)

-- 4. Misc (Interação Instantânea & Dinheiro Infinito Visual)
createToggleCard(miscPage, "Interação Instantânea", "Remove o tempo de espera de ProximityPrompts.", function(state)
	if state then
		local function setupPrompt(prompt)
			if prompt:IsA("ProximityPrompt") then
				prompt.HoldDuration = 0
				prompt.RequiresLineOfSight = false
			end
		end
		
		for _, obj in pairs(Workspace:GetDescendants()) do
			setupPrompt(obj)
		end
		
		if not _G.PromptConnection then
			_G.PromptConnection = Workspace.DescendantAdded:Connect(setupPrompt)
		end
	end
end)

createToggleCard(miscPage, "Dinheiro Infinito (Visual)", "Modifica o valor de dinheiro/cash localmente no player.", function(state)
	local leaderstats = player:FindFirstChild("leaderstats")
	if leaderstats then
		for _, stat in pairs(leaderstats:GetChildren()) do
			if stat:IsA("IntValue") or stat:IsA("NumberValue") then
				local nameLower = stat.Name:lower()
				if nameLower:find("money") or nameLower:find("cash") or nameLower:find("coin") or nameLower:find("gold") or nameLower:find("dinheiro") or nameLower:find("bux") then
					if state then
						stat.Value = 9999999999999
					else
						stat.Value = 0
					end
				end
			end
		end
	end
end)

-- 5. Settings Tab
local settingsInfo = Instance.new("TextLabel")
settingsInfo.Size = UDim2.new(1, 0, 0, 80)
settingsInfo.BackgroundTransparency = 1
settingsInfo.Text = "<b>inc0mubomdia1</b>\nscript by inc0mu\n<font size='11' color='#888899'>Pressione K para abrir/fechar a interface</font>"
settingsInfo.RichText = true
settingsInfo.TextColor3 = Color3.fromRGB(220, 220, 230)
settingsInfo.TextSize = 13
settingsInfo.Font = Enum.Font.Gotham
settingsInfo.TextXAlignment = Enum.TextXAlignment.Left
settingsInfo.Parent = settingsPage
