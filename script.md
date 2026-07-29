-- // inc0mubomdia1 - script by inc0mu
-- // Coloque em StarterPlayer > StarterPlayerScripts como um LocalScript

local Players = game:GetService("Players")
local Workspace = game:GetService("Workspace")
local Lighting = game:GetService("Lighting")
local RunService = game:GetService("RunService")

local player = Players.LocalPlayer
local camera = Workspace.CurrentCamera

-- Remover interface anterior se já existir
local playerGui = player:WaitForChild("PlayerGui")
if playerGui:FindFirstChild("Inc0muHubGui") then
	playerGui.Inc0muHubGui:Destroy()
end

local screenGui = Instance.new("ScreenGui")
screenGui.Name = "Inc0muHubGui"
screenGui.ResetOnSpawn = false
screenGui.DisplayOrder = 999
screenGui.Parent = playerGui

-- Janela Principal (Estilo Hub Flutuante)
local mainFrame = Instance.new("Frame")
mainFrame.Name = "MainFrame"
mainFrame.Size = UDim2.new(0, 500, 0, 310)
mainFrame.Position = UDim2.new(0.5, -250, 0.5, -155)
mainFrame.BackgroundColor3 = Color3.fromRGB(24, 24, 28)
mainFrame.BorderSizePixel = 0
mainFrame.Active = true
mainFrame.Draggable = true
mainFrame.Parent = screenGui

local mainCorner = Instance.new("UICorner")
mainCorner.CornerRadius = UDim.new(0, 8)
mainCorner.Parent = mainFrame

-- Barra Superior (Topbar)
local topBar = Instance.new("Frame")
topBar.Size = UDim2.new(1, 0, 0, 36)
topBar.BackgroundColor3 = Color3.fromRGB(30, 30, 36)
topBar.BorderSizePixel = 0
topBar.Parent = mainFrame

local topCorner = Instance.new("UICorner")
topCorner.CornerRadius = UDim.new(0, 8)
topCorner.Parent = topBar

-- Corrigir cantos inferiores da Topbar para ficarem retos
local topCover = Instance.new("Frame")
topCover.Size = UDim2.new(1, 0, 0, 4)
topCover.Position = UDim2.new(0, 0, 1, -4)
topCover.BackgroundColor3 = Color3.fromRGB(30, 30, 36)
topCover.BorderSizePixel = 0
topCover.Parent = topBar

local titleLabel = Instance.new("TextLabel")
titleLabel.Size = UDim2.new(1, -60, 1, 0)
titleLabel.Position = UDim2.new(0, 14, 0, 0)
titleLabel.BackgroundTransparency = 1
titleLabel.Text = "inc0mubomdia1  <font size='11' color='#888899'>script by inc0mu</font>"
titleLabel.RichText = true
titleLabel.TextColor3 = Color3.fromRGB(230, 230, 240)
titleLabel.TextSize = 13
titleLabel.Font = Enum.Font.GothamBold
titleLabel.TextXAlignment = Enum.TextXAlignment.Left
titleLabel.Parent = topBar

-- Botões de Fechar/Minimizar decorativos (estilo Windows)
local closeBtn = Instance.new("TextButton")
closeBtn.Size = UDim2.new(0, 30, 0, 24)
closeBtn.Position = UDim2.new(1, -34, 0.5, -12)
closeBtn.BackgroundColor3 = Color3.fromRGB(24, 24, 28)
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

-- Menu Lateral (Sidebar)
local sidebar = Instance.new("Frame")
sidebar.Size = UDim2.new(0, 140, 1, -36)
sidebar.Position = UDim2.new(0, 0, 0, 36)
sidebar.BackgroundColor3 = Color3.fromRGB(20, 20, 23)
sidebar.BorderSizePixel = 0
sidebar.Parent = mainFrame

local sidebarLayout = Instance.new("UIListLayout")
sidebarLayout.SortOrder = Enum.SortOrder.LayoutOrder
sidebarLayout.Padding = UDim.new(0, 4)
sidebarLayout.Parent = sidebar

local sidebarPadding = Instance.new("UIPadding")
sidebarPadding.PaddingTop = UDim.new(0, 8)
sidebarPadding.PaddingLeft = UDim.new(0, 8)
sidebarPadding.PaddingRight = UDim.new(0, 8)
sidebarPadding.Parent = sidebar

-- Container de Conteúdo (Lado Direito)
local container = Instance.new("Frame")
container.Size = UDim2.new(1, -140, 1, -36)
container.Position = UDim2.new(0, 140, 0, 36)
container.BackgroundTransparency = 1
container.Parent = mainFrame

-- Gerenciamento de Páginas
local pages = {}
local function createPage(name)
	local page = Instance.new("ScrollingFrame")
	page.Name = name .. "Page"
	page.Size = UDim2.new(1, 0, 1, 0)
	page.BackgroundTransparency = 1
	page.BorderSizePixel = 0
	page.Visible = false
	page.ScrollBarThickness = 2
	page.Parent = container

	local layout = Instance.new("UIListLayout")
	layout.SortOrder = Enum.SortOrder.LayoutOrder
	layout.Padding = UDim.new(0, 6)
	layout.Parent = page

	local padding = Instance.new("UIPadding")
	padding.PaddingTop = UDim.new(0, 10)
	padding.PaddingLeft = UDim.new(0, 12)
	padding.PaddingRight = UDim.new(0, 12)
	padding.Parent = page

	pages[name] = page
	return page
end

local verityPage = createPage("Verity")
local settingsPage = createPage("Settings")

-- Botões das Abas da Sidebar
local function createTabButton(name, targetPage)
	local btn = Instance.new("TextButton")
	btn.Size = UDim2.new(1, 0, 0, 32)
	btn.BackgroundColor3 = Color3.fromRGB(20, 20, 23)
	btn.TextColor3 = Color3.fromRGB(150, 150, 160)
	btn.TextSize = 12
	btn.Font = Enum.Font.GothamSemibold
	btn.Text = "   " .. name
	btn.TextXAlignment = Enum.TextXAlignment.Left
	btn.Parent = sidebar

	local corner = Instance.new("UICorner")
	corner.CornerRadius = UDim.new(0, 6)
	corner.Parent = btn

	btn.MouseButton1Click:Connect(function()
		for _, p in pairs(pages) do p.Visible = false end
		for _, b in pairs(sidebar:GetChildren()) do
			if b:IsA("TextButton") then
				b.BackgroundColor3 = Color3.fromRGB(20, 20, 23)
				b.TextColor3 = Color3.fromRGB(150, 150, 160)
			end
		end
		targetPage.Visible = true
		btn.BackgroundColor3 = Color3.fromRGB(32, 32, 38)
		btn.TextColor3 = Color3.fromRGB(255, 255, 255)
	end)

	return btn
end

local btnTab1 = createTabButton("Verity Cheats", verityPage)
btnTab1.BackgroundColor3 = Color3.fromRGB(32, 32, 38)
btnTab1.TextColor3 = Color3.fromRGB(255, 255, 255)
verityPage.Visible = true

createTabButton("Settings", settingsPage)

-- Criar Linhas de Toggle (Estilo idêntico ao da imagem de referência)
local function createToggleRow(parent, text, callback)
	local row = Instance.new("Frame")
	row.Size = UDim2.new(1, 0, 0, 38)
	row.BackgroundColor3 = Color3.fromRGB(28, 28, 34)
	row.BorderSizePixel = 0
	row.Parent = parent

	local rCorner = Instance.new("UICorner")
	rCorner.CornerRadius = UDim.new(0, 6)
	rCorner.Parent = row

	local label = Instance.new("TextLabel")
	label.Size = UDim2.new(1, -60, 1, 0)
	label.Position = UDim2.new(0, 12, 0, 0)
	label.BackgroundTransparency = 1
	label.Text = text
	label.TextColor3 = Color3.fromRGB(210, 210, 220)
	label.TextSize = 12
	label.Font = Enum.Font.GothamMedium
	label.TextXAlignment = Enum.TextXAlignment.Left
	label.Parent = row

	local toggleBtn = Instance.new("TextButton")
	toggleBtn.Size = UDim2.new(0, 40, 0, 20)
	toggleBtn.Position = UDim2.new(1, -48, 0.5, -10)
	toggleBtn.BackgroundColor3 = Color3.fromRGB(45, 45, 52)
	toggleBtn.Text = ""
	toggleBtn.Parent = row

	local tCorner = Instance.new("UICorner")
	tCorner.CornerRadius = UDim.new(1, 0)
	tCorner.Parent = toggleBtn

	local circle = Instance.new("Frame")
	circle.Size = UDim2.new(0, 14, 0, 14)
	circle.Position = UDim2.new(0, 3, 0.5, -7)
	circle.BackgroundColor3 = Color3.fromRGB(180, 180, 190)
	circle.Parent = toggleBtn

	local cCorner = Instance.new("UICorner")
	cCorner.CornerRadius = UDim.new(1, 0)
	cCorner.Parent = circle

	local active = false
	toggleBtn.MouseButton1Click:Connect(function()
		active = not active
		if active then
			toggleBtn.BackgroundColor3 = Color3.fromRGB(0, 120, 215)
			circle:TweenPosition(UDim2.new(1, -17, 0.5, -7), Enum.EasingDirection.Out, Enum.EasingStyle.Quad, 0.15, true)
		else
			toggleBtn.BackgroundColor3 = Color3.fromRGB(45, 45, 52)
			circle:TweenPosition(UDim2.new(0, 3, 0.5, -7), Enum.EasingDirection.Out, Enum.EasingStyle.Quad, 0.15, true)
		end
		callback(active)
	end)

	return row
end

-- // 1. ESP Pneus
createToggleRow(verityPage, "ESP Pneus (Highlight)", function(state)
	for _, obj in pairs(Workspace:GetDescendants()) do
		if obj.Name == "Pneu" or obj.Name:lower():find("pneu") then
			if state then
				if not obj:FindFirstChild("Inc0muESP") then
					local highlight = Instance.new("Highlight")
					highlight.Name = "Inc0muESP"
					highlight.Adornee = obj
					highlight.FillColor = Color3.fromRGB(255, 255, 0)
					highlight.FillTransparency = 0.4
					highlight.OutlineColor = Color3.fromRGB(255, 255, 255)
					highlight.DepthMode = Enum.HighlightDepthMode.AlwaysOnTop
					highlight.Parent = obj
				end
			else
				local h = obj:FindFirstChild("Inc0muESP")
				if h then h:Destroy() end
			end
		end
	end
end)

-- // 2. Fullbright & Remover Fog
local originalAmbient = Lighting.Ambient
local originalOutdoor = Lighting.OutdoorAmbient
local originalBrightness = Lighting.Brightness
local originalFog = Lighting.FogEnd

createToggleRow(verityPage, "Fullbright & Remove Fog", function(state)
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

-- // 3. WalkSpeed Personalizado (1-1000) com Anti-Lentidão para Caixas
local speedRow = Instance.new("Frame")
speedRow.Size = UDim2.new(1, 0, 0, 38)
speedRow.BackgroundColor3 = Color3.fromRGB(28, 28, 34)
speedRow.BorderSizePixel = 0
speedRow.Parent = verityPage

local srCorner = Instance.new("UICorner")
srCorner.CornerRadius = UDim.new(0, 6)
srCorner.Parent = speedRow

local speedLabel = Instance.new("TextLabel")
speedLabel.Size = UDim2.new(1, -110, 1, 0)
speedLabel.Position = UDim2.new(0, 12, 0, 0)
speedLabel.BackgroundTransparency = 1
speedLabel.Text = "WalkSpeed (1-1000)"
speedLabel.TextColor3 = Color3.fromRGB(210, 210, 220)
speedLabel.TextSize = 12
speedLabel.Font = Enum.Font.GothamMedium
speedLabel.TextXAlignment = Enum.TextXAlignment.Left
speedLabel.Parent = speedRow

local speedInput = Instance.new("TextBox")
speedInput.Size = UDim2.new(0, 45, 0, 22)
speedInput.Position = UDim2.new(1, -100, 0.5, -11)
speedInput.BackgroundColor3 = Color3.fromRGB(40, 40, 48)
speedInput.TextColor3 = Color3.fromRGB(255, 255, 255)
speedInput.TextSize = 12
speedInput.Font = Enum.Font.Gotham
speedInput.Text = "32"
speedInput.Parent = speedRow

local inputCorner = Instance.new("UICorner")
inputCorner.CornerRadius = UDim.new(0, 4)
inputCorner.Parent = speedInput

local speedToggleBtn = Instance.new("TextButton")
speedToggleBtn.Size = UDim2.new(0, 40, 0, 20)
speedToggleBtn.Position = UDim2.new(1, -48, 0.5, -10)
speedToggleBtn.BackgroundColor3 = Color3.fromRGB(45, 45, 52)
speedToggleBtn.Text = ""
speedToggleBtn.Parent = speedRow

local stCorner = Instance.new("UICorner")
stCorner.CornerRadius = UDim.new(1, 0)
stCorner.Parent = speedToggleBtn

local scircle = Instance.new("Frame")
scircle.Size = UDim2.new(0, 14, 0, 14)
scircle.Position = UDim2.new(0, 3, 0.5, -7)
scircle.BackgroundColor3 = Color3.fromRGB(180, 180, 190)
scircle.Parent = speedToggleBtn

local scCorner = Instance.new("UICorner")
scCorner.CornerRadius = UDim.new(1, 0)
scCorner.Parent = scircle

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
		speedToggleBtn.BackgroundColor3 = Color3.fromRGB(0, 120, 215)
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

-- // 4. FOV Changer
createToggleRow(verityPage, "Field of View (FOV 90)", function(state)
	camera.FieldOfView = state and 90 or 70
end)

-- // 5. Interação Instantânea (ProximityPrompts sem Cooldown)
RunService.Stepped:Connect(function()
	for _, prompt in pairs(Workspace:GetDescendants()) do
		if prompt:IsA("ProximityPrompt") then
			prompt.HoldDuration = 0
			prompt.RequiresLineOfSight = false
		end
	end
end)

-- // Aba Settings (Créditos)
local infoLabel = Instance.new("TextLabel")
infoLabel.Size = UDim2.new(1, 0, 0, 80)
infoLabel.BackgroundTransparency = 1
infoLabel.Text = "<b>inc0mubomdia1</b>\nscript by inc0mu\n<font size='11' color='#888899'>Interface limpa inspirada em painéis modernos</font>"
infoLabel.RichText = true
infoLabel.TextColor3 = Color3.fromRGB(220, 220, 230)
infoLabel.TextSize = 13
infoLabel.Font = Enum.Font.Gotham
infoLabel.TextXAlignment = Enum.TextXAlignment.Left
infoLabel.Parent = settingsPage
