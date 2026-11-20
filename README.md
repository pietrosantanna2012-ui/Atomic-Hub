--==================================================
-- ATOMIC HUB V2.1.2
-- Brookhaven RP | Vortex 777 e Bugadinho
--==================================================

local Players = game:GetService("Players")
local UserInputService = game:GetService("UserInputService")
local TweenService = game:GetService("TweenService")
local RunService = game:GetService("RunService")
local Workspace = game:GetService("Workspace")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local VirtualInputManager = game:GetService("VirtualInputManager")
local HttpService = game:GetService("HttpService")
local Lighting = game:GetService("Lighting")

local Player = Players.LocalPlayer
local Camera = Workspace.CurrentCamera

local AdminUserIds = {
	[3069018287] = true,
	[8431952921] = true
}

--==================================================
-- INICIALIZAÇÃO PANDA AUTH (PUSL-V4)
--==================================================

local PUSL = loadstring(game:HttpGet("https://secure.pandauth.com/pv4/lib"))()
if PUSL and type(PUSL.configure) == "function" then
	PUSL.configure({
		serviceId = "atomichub",
	})
else
	warn("[Panda Auth] Falha ao carregar a biblioteca.")
end

--==================================================
-- CONFIGURAÇÕES E ESTADOS
--==================================================

local IMAGE_ID = "rbxassetid://79083620757450"
local MUSIC_ID = "rbxassetid://124383118098366"

local WINDOW_WIDTH = 560
local WINDOW_HEIGHT = 350

local currentLanguage = "PT"

local Translations = {
	PT = {
		Title = "Atomic Hub",
		SubTitle = "Brookhaven RP | Vortex 777",
		TabMain = "Main",
		TabPlayer = "Jogador",
		TabServer = "Servidor",
		TabSkybox = "Skybox",
		TabAudio = "Áudio",
		TabConfig = "Config",
		InfoLabel = "Informações & Comunidade",
		KillLabel = "Matar",
		FlingLabel = "Fling",
		ViewLabel = "Visualizar/TP",
		AvatarLabel = "Skin",
		ServerLabel = "Ações do Servidor e Proteções",
		SkyboxLabel = "Skybox FE",
		AudioLabel = "Reprodutor de Áudio",
		ConfigLabel = "Configurações",
		CustomLabel = "Personalização",
		BtnDiscord = "Entrar no Discord",
		BtnExecutor = "Executor: ",
		BtnKill = "Executar Matar",
		BtnFling = "Executar Fling",
		BtnView = "Visualizar Jogador",
		BtnStopView = "Parar Visualização",
		BtnTp = "TP para Jogador",
		BtnBringCouch = "Puxar Jogador (Couch)",
		BtnPratfall = "Pratfall",
		BtnCopy = "Copiar Avatar",
		BtnJump = "Infinito Pulo",
		BtnApplySky = "Aplicar Skybox",
		BtnResetSky = "Resetar Skybox",
		BtnPlayAudio = "Tocar Áudio",
		BtnStopAudio = "Parar Todos os Áudios",
		BtnCenter = "Centralizar Janela",
		BtnLangPt = "Português",
		BtnLangEn = "English",
		BtnLangEs = "Español",
		BtnApplyColor = "Aplicar Cor Customizada",
		BtnApplyBubble = "Aplicar Imagem da Bolinha",
		ColorBlue = "Tema Azul",
		ColorPurple = "Tema Roxo",
		ColorRed = "Tema Vermelho",
		ColorGreen = "Tema Verde",
		ColorDark = "Tema Padrão (Dark)",
	},
	EN = {
		Title = "Atomic Hub",
		SubTitle = "Brookhaven RP | Vortex 777",
		TabMain = "Main",
		TabPlayer = "Player",
		TabServer = "Server",
		TabSkybox = "Skybox",
		TabAudio = "Audio",
		TabConfig = "Settings",
		InfoLabel = "Info & Community",
		KillLabel = "Kill",
		FlingLabel = "Fling",
		ViewLabel = "View/TP",
		AvatarLabel = "Avatar",
		ServerLabel = "Server Actions & Protections",
		SkyboxLabel = "Skybox FE",
		AudioLabel = "Audio Player",
		ConfigLabel = "Settings",
		CustomLabel = "Customization",
		BtnDiscord = "Join Discord",
		BtnExecutor = "Executor: ",
		BtnKill = "Execute Kill",
		BtnFling = "Execute Fling",
		BtnView = "View Player",
		BtnStopView = "Stop View",
		BtnTp = "TP to Player",
		BtnBringCouch = "Bring Player (Couch)",
		BtnPratfall = "Pratfall",
		BtnCopy = "Copy Avatar",
		BtnJump = "Infinite Jump",
		BtnApplySky = "Apply Skybox",
		BtnResetSky = "Reset Skybox",
		BtnPlayAudio = "Play Audio",
		BtnStopAudio = "Stop All Audio",
		BtnCenter = "Center Window",
		BtnLangPt = "Português",
		BtnLangEn = "English",
		BtnLangEs = "Español",
		BtnApplyColor = "Apply Custom Color",
		BtnApplyBubble = "Apply Bubble Image",
		ColorBlue = "Blue Theme",
		ColorPurple = "Purple Theme",
		ColorRed = "Red Theme",
		ColorGreen = "Green Theme",
		ColorDark = "Default Theme (Dark)",
	},
	ES = {
		Title = "Atomic Hub",
		SubTitle = "Brookhaven RP | Vortex 777",
		TabMain = "Main",
		TabPlayer = "Jugador",
		TabServer = "Servidor",
		TabSkybox = "Skybox",
		TabAudio = "Audio",
		TabConfig = "Ajustes",
		InfoLabel = "Información y Comunidad",
		KillLabel = "Matar",
		FlingLabel = "Fling",
		ViewLabel = "Ver/TP",
		AvatarLabel = "Skin",
		ServerLabel = "Acciones del Servidor y Protecciones",
		SkyboxLabel = "Skybox FE",
		AudioLabel = "Reproductor de Audio",
		ConfigLabel = "Configuración",
		CustomLabel = "Personalización",
		BtnDiscord = "Unirse al Discord",
		BtnExecutor = "Executor: ",
		BtnKill = "Ejecutar Matar",
		BtnFling = "Ejecutar Fling",
		BtnView = "Ver Jugador",
		BtnStopView = "Parar Vista",
		BtnTp = "TP al Jugador",
		BtnBringCouch = "Traer Jugador (Couch)",
		BtnPratfall = "Pratfall",
		BtnCopy = "Copiar Skin",
		BtnJump = "Salto Infinito",
		BtnApplySky = "Aplicar Skybox",
		BtnResetSky = "Restablecer Skybox",
		BtnPlayAudio = "Reproducir Audio",
		BtnStopAudio = "Parar Todos los Audios",
		BtnCenter = "Centrar Ventana",
		BtnLangPt = "Português",
		BtnLangEn = "English",
		BtnLangEs = "Español",
		BtnApplyColor = "Aplicar Color Personalizado",
		BtnApplyBubble = "Aplicar Imagen de Burbuja",
		ColorBlue = "Tema Azul",
		ColorPurple = "Tema Morado",
		ColorRed = "Tema Rojo",
		ColorGreen = "Tema Verde",
		ColorDark = "Tema Predeterminado (Oscuro)",
	}
}

local function getTranslation(key)
	return Translations[currentLanguage][key] or key
end

local function GetExecutorName()
	if identifyexecutor then
		return identifyexecutor()
	elseif getexecutorname then
		return getexecutorname()
	elseif KRNL_LOADED then
		return "Krnl"
	elseif Synapse then
		return "Synapse Z / X"
	elseif Fluxus then
		return "Fluxus"
	elseif Delta then
		return "Delta"
	elseif Wave then
		return "Wave"
	elseif Codex then
		return "Codex"
	elseif ArceusX then
		return "Arceus X"
	elseif Solara then
		return "Solara"
	end
	return "Desconhecido"
end

local antiLagEnabled = false
local antiFlingEnabled = false
local antiAdminPanelEnabled = false
local antiAdminStop = nil
local antiVoidEnabled = false
local antiVoidThreshold = -50
local antiVoidOriginalPosition = nil

local bombLagActive = false
local Viewing = nil
local ViewConnection = nil
local originalTorsoData = nil

local selectedAudioID = nil
local audioLoopActive = false
local loudSpamActive = false
local loudAudioID = 6314880174

local glitchActive = false
local glitchPart = nil

local PlayerGui = Player:WaitForChild("PlayerGui")
local oldGui = PlayerGui:FindFirstChild("AtomicHub")
if oldGui then oldGui:Destroy() end

local oldKeyGui = PlayerGui:FindFirstChild("AtomicKeySystem")
if oldKeyGui then oldKeyGui:Destroy() end

local function Corner(parent, radius)
	local corner = Instance.new("UICorner")
	corner.CornerRadius = UDim.new(0, radius)
	corner.Parent = parent
	return corner
end

local function Stroke(parent, transparency)
	local stroke = Instance.new("UIStroke")
	stroke.Color = Color3.fromRGB(50, 50, 60)
	stroke.Thickness = 1
	stroke.Transparency = transparency or 0
	stroke.Parent = parent
	return stroke
end

local function Tween(object, time, properties, style, direction)
	local info = TweenInfo.new(
		time,
		style or Enum.EasingStyle.Quad,
		direction or Enum.EasingDirection.Out
	)
	return TweenService:Create(object, info, properties)
end

local function StartAtomicHub()

	local Gui = Instance.new("ScreenGui")
	Gui.Name = "AtomicHub"
	Gui.ResetOnSpawn = false
	Gui.IgnoreGuiInset = true
	Gui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
	Gui.Parent = PlayerGui

	-- Tela de Carregamento com Blur
	local LoadingBlur = Instance.new("BlurEffect")
	LoadingBlur.Size = 24
	LoadingBlur.Parent = Lighting

	local LoadingScreen = Instance.new("Frame")
	LoadingScreen.Name = "LoadingScreen"
	LoadingScreen.Size = UDim2.fromScale(1, 1)
	LoadingScreen.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
	LoadingScreen.BackgroundTransparency = 0.4
	LoadingScreen.BorderSizePixel = 0
	LoadingScreen.ZIndex = 100
	LoadingScreen.Parent = Gui

	local LoadingCard = Instance.new("Frame")
	LoadingCard.Size = UDim2.fromOffset(420, 250)
	LoadingCard.Position = UDim2.fromScale(0.5, 0.5)
	LoadingCard.AnchorPoint = Vector2.new(0.5, 0.5)
	LoadingCard.BackgroundColor3 = Color3.fromRGB(22, 22, 29)
	LoadingCard.BorderSizePixel = 0
	LoadingCard.ZIndex = 101
	LoadingCard.Parent = LoadingScreen

	Corner(LoadingCard, 20)
	Stroke(LoadingCard)

	local LoadingIcon = Instance.new("ImageLabel")
	LoadingIcon.Size = UDim2.fromOffset(80, 80)
	LoadingIcon.Position = UDim2.new(0.5, 0, 0, 25)
	LoadingIcon.AnchorPoint = Vector2.new(0.5, 0)
	LoadingIcon.BackgroundTransparency = 1
	LoadingIcon.Image = IMAGE_ID
	LoadingIcon.ZIndex = 102
	LoadingIcon.Parent = LoadingCard
	Corner(LoadingIcon, 20)

	local LoadingTitle = Instance.new("TextLabel")
	LoadingTitle.Size = UDim2.new(1, -40, 0, 35)
	LoadingTitle.Position = UDim2.new(0, 20, 0, 110)
	LoadingTitle.BackgroundTransparency = 1
	LoadingTitle.Text = "Atomic Hub"
	LoadingTitle.TextColor3 = Color3.fromRGB(255, 255, 255)
	LoadingTitle.TextSize = 25
	LoadingTitle.Font = Enum.Font.GothamBold
	LoadingTitle.ZIndex = 102
	LoadingTitle.Parent = LoadingCard

	local LoadingText = Instance.new("TextLabel")
	LoadingText.Size = UDim2.new(1, -40, 0, 25)
	LoadingText.Position = UDim2.new(0, 20, 0, 145)
	LoadingText.BackgroundTransparency = 1
	LoadingText.Text = "Carregando..."
	LoadingText.TextColor3 = Color3.fromRGB(170, 170, 180)
	LoadingText.TextSize = 14
	LoadingText.Font = Enum.Font.Gotham
	LoadingText.ZIndex = 102
	LoadingText.Parent = LoadingCard

	local ProgressBackground = Instance.new("Frame")
	ProgressBackground.Size = UDim2.new(1, -60, 0, 10)
	ProgressBackground.Position = UDim2.new(0, 30, 0, 185)
	ProgressBackground.BackgroundColor3 = Color3.fromRGB(45, 45, 55)
	ProgressBackground.BorderSizePixel = 0
	ProgressBackground.ZIndex = 102
	ProgressBackground.Parent = LoadingCard
	Corner(ProgressBackground, 10)

	local Progress = Instance.new("Frame")
	Progress.Size = UDim2.new(0, 0, 1, 0)
	Progress.BackgroundColor3 = Color3.fromRGB(100, 150, 255)
	Progress.BorderSizePixel = 0
	Progress.ZIndex = 103
	Progress.Parent = ProgressBackground
	Corner(Progress, 10)

	local Percentage = Instance.new("TextLabel")
	Percentage.Size = UDim2.new(1, 0, 0, 20)
	Percentage.Position = UDim2.new(0, 0, 0, 205)
	Percentage.BackgroundTransparency = 1
	Percentage.Text = "0%"
	Percentage.TextColor3 = Color3.fromRGB(160, 160, 170)
	Percentage.TextSize = 12
	Percentage.Font = Enum.Font.Gotham
	Percentage.ZIndex = 102
	Percentage.Parent = LoadingCard

	local LoadingMusic = Instance.new("Sound")
	LoadingMusic.Name = "AtomicHubLoadingMusic"
	LoadingMusic.SoundId = MUSIC_ID
	LoadingMusic.Volume = 0.5
	LoadingMusic.Looped = true
	LoadingMusic.Parent = Gui
	pcall(function() LoadingMusic:Play() end)

	-- Janela Principal
	local Main = Instance.new("Frame")
	Main.Name = "Main"
	Main.Size = UDim2.fromOffset(WINDOW_WIDTH, WINDOW_HEIGHT)
	Main.Position = UDim2.fromScale(0.5, 0.5)
	Main.AnchorPoint = Vector2.new(0.5, 0.5)
	Main.BackgroundColor3 = Color3.fromRGB(20, 20, 27)
	Main.BorderSizePixel = 0
	Main.Visible = false
	Main.Active = true
	Main.ZIndex = 10
	Main.Parent = Gui
	Corner(Main, 16)
	Stroke(Main)

	-- Header
	local Header = Instance.new("Frame")
	Header.Name = "Header"
	Header.Size = UDim2.new(1, 0, 0, 55)
	Header.BackgroundColor3 = Color3.fromRGB(27, 27, 35)
	Header.BorderSizePixel = 0
	Header.ZIndex = 11
	Header.Parent = Main
	Corner(Header, 16)

	local HeaderFix = Instance.new("Frame")
	HeaderFix.Size = UDim2.new(1, 0, 0, 20)
	HeaderFix.Position = UDim2.new(0, 0, 1, -20)
	HeaderFix.BackgroundColor3 = Color3.fromRGB(27, 27, 35)
	HeaderFix.BorderSizePixel = 0
	HeaderFix.ZIndex = 11
	HeaderFix.Parent = Header

	local HeaderIcon = Instance.new("ImageLabel")
	HeaderIcon.Size = UDim2.fromOffset(36, 36)
	HeaderIcon.Position = UDim2.new(0, 10, 0.5, -18)
	HeaderIcon.BackgroundTransparency = 1
	HeaderIcon.Image = IMAGE_ID
	HeaderIcon.ZIndex = 12
	HeaderIcon.Parent = Header
	Corner(HeaderIcon, 10)

	local Title = Instance.new("TextLabel")
	Title.Size = UDim2.new(1, -150, 1, 0)
	Title.Position = UDim2.new(0, 55, 0, 0)
	Title.BackgroundTransparency = 1
	Title.Text = "Atomic Hub"
	Title.TextColor3 = Color3.fromRGB(255, 255, 255)
	Title.TextSize = 17
	Title.TextXAlignment = Enum.TextXAlignment.Left
	Title.Font = Enum.Font.GothamBold
	Title.ZIndex = 12
	Title.Parent = Header

	local Minimize = Instance.new("TextButton")
	Minimize.Size = UDim2.fromOffset(35, 35)
	Minimize.Position = UDim2.new(1, -80, 0.5, -17)
	Minimize.BackgroundColor3 = Color3.fromRGB(35, 35, 45)
	Minimize.BorderSizePixel = 0
	Minimize.Text = "—"
	Minimize.TextColor3 = Color3.fromRGB(255, 255, 255)
	Minimize.TextSize = 18
	Minimize.Font = Enum.Font.GothamBold
	Minimize.AutoButtonColor = false
	Minimize.ZIndex = 13
	Minimize.Parent = Header
	Corner(Minimize, 9)

	local Close = Instance.new("TextButton")
	Close.Size = UDim2.fromOffset(35, 35)
	Close.Position = UDim2.new(1, -40, 0.5, -17)
	Close.BackgroundColor3 = Color3.fromRGB(55, 25, 30)
	Close.BorderSizePixel = 0
	Close.Text = "×"
	Close.TextColor3 = Color3.fromRGB(255, 255, 255)
	Close.TextSize = 22
	Close.Font = Enum.Font.GothamBold
	Close.AutoButtonColor = false
	Close.ZIndex = 13
	Close.Parent = Header
	Corner(Close, 9)

	-- Sidebar & Content
	local Sidebar = Instance.new("Frame")
	Sidebar.Name = "Sidebar"
	Sidebar.Size = UDim2.new(0, 135, 1, -55)
	Sidebar.Position = UDim2.new(0, 0, 0, 55)
	Sidebar.BackgroundColor3 = Color3.fromRGB(16, 16, 22)
	Sidebar.BorderSizePixel = 0
	Sidebar.ZIndex = 11
	Sidebar.Parent = Main

	local Content = Instance.new("Frame")
	Content.Name = "Content"
	Content.Size = UDim2.new(1, -135, 1, -55)
	Content.Position = UDim2.new(0, 135, 0, 55)
	Content.BackgroundTransparency = 1
	Content.BorderSizePixel = 0
	Content.ZIndex = 11
	Content.Parent = Main

	local ContentPadding = Instance.new("UIPadding")
	ContentPadding.PaddingTop = UDim.new(0, 15)
	ContentPadding.PaddingBottom = UDim.new(0, 15)
	ContentPadding.PaddingLeft = UDim.new(0, 15)
	ContentPadding.PaddingRight = UDim.new(0, 15)
	ContentPadding.Parent = Content

	local Tabs = {}
	local CurrentTab = nil
	local TabCount = 0

	local activeThemeColor = Color3.fromRGB(65, 90, 150)
	local defaultTabBgColor = Color3.fromRGB(14, 14, 18)
	local elementBaseColor = Color3.fromRGB(15, 15, 20)
	local elementHoverColor = Color3.fromRGB(25, 25, 35)

	local function CreateTab(NameKey)
		TabCount += 1
		local Button = Instance.new("TextButton")
		Button.Name = NameKey .. "Btn"
		Button.Size = UDim2.new(1, -16, 0, 36)
		Button.Position = UDim2.new(0, 8, 0, 8 + ((TabCount - 1) * 40))
		Button.BackgroundColor3 = defaultTabBgColor
		Button.BorderSizePixel = 0
		Button.Text = getTranslation(NameKey)
		Button.TextColor3 = Color3.fromRGB(190, 190, 200)
		Button.TextSize = 12
		Button.Font = Enum.Font.GothamMedium
		Button.AutoButtonColor = false
		Button.ZIndex = 12
		Button.Parent = Sidebar
		Corner(Button, 9)
		Button:SetAttribute("NameKey", NameKey)

		local Page = Instance.new("ScrollingFrame")
		Page.Name = NameKey .. "Page"
		Page.Size = UDim2.fromScale(1, 1)
		Page.BackgroundTransparency = 1
		Page.BorderSizePixel = 0
		Page.ScrollBarThickness = 4
		Page.ScrollBarImageColor3 = Color3.fromRGB(60, 60, 70)
		Page.CanvasSize = UDim2.new(0, 0, 0, 0)
		Page.AutomaticCanvasSize = Enum.AutomaticSize.Y
		Page.Visible = false
		Page.ZIndex = 12
		Page.Parent = Content

		local Layout = Instance.new("UIListLayout")
		Layout.Padding = UDim.new(0, 8)
		Layout.SortOrder = Enum.SortOrder.LayoutOrder
		Layout.Parent = Page

		local Tab = { Button = Button, Page = Page }
		table.insert(Tabs, Tab)

		Button.MouseButton1Click:Connect(function()
			for _, Other in ipairs(Tabs) do
				Other.Page.Visible = false
				Other.Button.BackgroundColor3 = defaultTabBgColor
				Other.Button.TextColor3 = Color3.fromRGB(190, 190, 200)
			end
			Page.Visible = true
			Button.BackgroundColor3 = activeThemeColor
			Button.TextColor3 = Color3.fromRGB(255, 255, 255)
			CurrentTab = Tab
		end)

		return Page
	end

	local function AddLabel(Page, TextKey, DirectText)
		local Label = Instance.new("TextLabel")
		Label.Size = UDim2.new(1, 0, 0, 25)
		Label.BackgroundTransparency = 1
		Label.Font = Enum.Font.GothamBold
		Label.Text = DirectText or getTranslation(TextKey)
		Label.TextSize = 12
		Label.TextColor3 = Color3.fromRGB(170, 170, 180)
		Label.TextXAlignment = Enum.TextXAlignment.Left
		Label.ZIndex = 13
		Label.Parent = Page
		if TextKey then Label:SetAttribute("TextKey", TextKey) end
		return Label
	end

	local function AddButton(Page, TextKey, Callback)
		local Button = Instance.new("TextButton")
		Button.Size = UDim2.new(1, 0, 0, 34)
		Button.BackgroundColor3 = elementBaseColor
		Button.BorderSizePixel = 0
		Button.Font = Enum.Font.Gotham
		Button.Text = getTranslation(TextKey)
		Button.TextSize = 11
		Button.TextColor3 = Color3.fromRGB(225, 225, 228)
		Button.TextXAlignment = Enum.TextXAlignment.Left
		Button.AutoButtonColor = false
		Button.ZIndex = 13
		Button.Parent = Page
		Button:SetAttribute("TextKey", TextKey)
		Corner(Button, 6)
		Stroke(Button, 0.5)

		local Padding = Instance.new("UIPadding")
		Padding.PaddingLeft = UDim.new(0, 10)
		Padding.Parent = Button

		Button.MouseEnter:Connect(function() Tween(Button, 0.15, {BackgroundColor3 = elementHoverColor}):Play() end)
		Button.MouseLeave:Connect(function() Tween(Button, 0.15, {BackgroundColor3 = elementBaseColor}):Play() end)
		Button.MouseButton1Click:Connect(function() if Callback then Callback(Button) end end)
		return Button
	end

	local function AddToggle(Page, TextKey, Default, Callback)
		local State = Default or false
		local Button = Instance.new("TextButton")
		Button.Size = UDim2.new(1, 0, 0, 34)
		Button.BackgroundColor3 = elementBaseColor
		Button.BorderSizePixel = 0
		Button.Text = ""
		Button.AutoButtonColor = false
		Button.ZIndex = 13
		Button.Parent = Page
		Corner(Button, 6)
		Stroke(Button, 0.5)

		local Label = Instance.new("TextLabel")
		Label.Position = UDim2.fromOffset(11, 0)
		Label.Size = UDim2.new(1, -55, 1, 0)
		Label.BackgroundTransparency = 1
		Label.Font = Enum.Font.Gotham
		Label.Text = getTranslation(TextKey)
		Label.TextSize = 11
		Label.TextColor3 = Color3.fromRGB(225, 225, 228)
		Label.TextXAlignment = Enum.TextXAlignment.Left
		Label.ZIndex = 13
		Label.Parent = Button
		Label:SetAttribute("TextKey", TextKey)

		local Toggle = Instance.new("Frame")
		Toggle.Position = UDim2.new(1, -34, 0.5, -7)
		Toggle.Size = UDim2.fromOffset(24, 14)
		Toggle.BackgroundColor3 = Color3.fromRGB(30, 30, 40)
		Toggle.BorderSizePixel = 0
		Toggle.ZIndex = 13
		Toggle.Parent = Button
		Corner(Toggle, 7)

		local Knob = Instance.new("Frame")
		Knob.Position = UDim2.fromOffset(2, 2)
		Knob.Size = UDim2.fromOffset(10, 10)
		Knob.BackgroundColor3 = Color3.fromRGB(135, 135, 140)
		Knob.BorderSizePixel = 0
		Knob.ZIndex = 14
		Knob.Parent = Toggle
		Corner(Knob, 5)

		local function Update()
			if State then
				Tween(Toggle, 0.15, {BackgroundColor3 = Color3.fromRGB(90, 135, 205)}):Play()
				Tween(Knob, 0.15, {Position = UDim2.new(1, -12, 0, 2)}):Play()
				Knob.BackgroundColor3 = Color3.new(1, 1, 1)
			else
				Tween(Toggle, 0.15, {BackgroundColor3 = Color3.fromRGB(30, 30, 40)}):Play()
				Tween(Knob, 0.15, {Position = UDim2.fromOffset(2, 2)}):Play()
				Knob.BackgroundColor3 = Color3.fromRGB(135, 135, 140)
			end
		end

		Button.MouseEnter:Connect(function() Button.BackgroundColor3 = elementHoverColor end)
		Button.MouseLeave:Connect(function() Button.BackgroundColor3 = elementBaseColor end)
		Button.MouseButton1Click:Connect(function()
			State = not State
			Update()
			if Callback then Callback(State) end
		end)
		Update()
		return Button
	end

	local function AddTextBox(Page, Placeholder, Callback)
		local Box = Instance.new("TextBox")
		Box.Size = UDim2.new(1, 0, 0, 34)
		Box.BackgroundColor3 = elementBaseColor
		Box.BorderSizePixel = 0
		Box.ClearTextOnFocus = false
		Box.Font = Enum.Font.Gotham
		Box.PlaceholderText = Placeholder or "Type here..."
		Box.PlaceholderColor3 = Color3.fromRGB(135, 135, 140)
		Box.Text = ""
		Box.TextSize = 11
		Box.TextColor3 = Color3.fromRGB(225, 225, 228)
		Box.TextXAlignment = Enum.TextXAlignment.Left
		Box.ZIndex = 13
		Box.Parent = Page
		Corner(Box, 6)
		Stroke(Box, 0.5)

		local Padding = Instance.new("UIPadding")
		Padding.PaddingLeft = UDim.new(0, 10)
		Padding.PaddingRight = UDim.new(0, 10)
		Padding.Parent = Box

		Box.Focused:Connect(function() Box.BackgroundColor3 = elementHoverColor end)
		Box.FocusLost:Connect(function(EnterPressed)
			Box.BackgroundColor3 = elementBaseColor
			if Callback then Callback(Box.Text, EnterPressed) end
		end)
		return Box
	end

	-- CRIAR LISTA RETRÁTIL (ESTILO DROPDOWN)
	local function CreateCustomList(Parent, TitleText)
		local isOpen = false
		local selectedItem = nil
		local itemsData = {}
		local callbackFunc = nil

		local Container = Instance.new("Frame")
		Container.Name = TitleText .. "Dropdown"
		Container.Size = UDim2.new(1, 0, 0, 34)
		Container.BackgroundColor3 = elementBaseColor
		Container.BorderSizePixel = 0
		Container.ZIndex = 13
		Container.Parent = Parent
		Corner(Container, 6)
		Stroke(Container, 0.5)

		local HeaderBtn = Instance.new("TextButton")
		HeaderBtn.Size = UDim2.new(1, 0, 0, 34)
		HeaderBtn.BackgroundTransparency = 1
		HeaderBtn.Text = ""
		HeaderBtn.ZIndex = 14
		HeaderBtn.Parent = Container

		local LabelTitle = Instance.new("TextLabel")
		LabelTitle.Position = UDim2.fromOffset(10, 0)
		LabelTitle.Size = UDim2.new(1, -30, 1, 0)
		LabelTitle.BackgroundTransparency = 1
		LabelTitle.Font = Enum.Font.GothamMedium
		LabelTitle.Text = TitleText .. ": (Nenhum selecionado)"
		LabelTitle.TextSize = 11
		LabelTitle.TextColor3 = Color3.fromRGB(225, 225, 228)
		LabelTitle.TextXAlignment = Enum.TextXAlignment.Left
		LabelTitle.ZIndex = 14
		LabelTitle.Parent = HeaderBtn

		local Arrow = Instance.new("TextLabel")
		Arrow.Position = UDim2.new(1, -25, 0, 0)
		Arrow.Size = UDim2.fromOffset(20, 34)
		Arrow.BackgroundTransparency = 1
		Arrow.Font = Enum.Font.GothamBold
		Arrow.Text = "▼"
		Arrow.TextSize = 10
		Arrow.TextColor3 = Color3.fromRGB(170, 170, 180)
		Arrow.ZIndex = 14
		Arrow.Parent = HeaderBtn

		local DropListFrame = Instance.new("ScrollingFrame")
		DropListFrame.Position = UDim2.new(0, 0, 0, 38)
		DropListFrame.Size = UDim2.new(1, 0, 0, 0)
		DropListFrame.BackgroundColor3 = Color3.fromRGB(12, 12, 16)
		DropListFrame.BorderSizePixel = 0
		DropListFrame.Visible = false
		DropListFrame.ScrollBarThickness = 2
		DropListFrame.ScrollBarImageColor3 = Color3.fromRGB(45, 45, 55)
		DropListFrame.AutomaticCanvasSize = Enum.AutomaticSize.Y
		DropListFrame.CanvasSize = UDim2.new(0, 0, 0, 0)
		DropListFrame.ZIndex = 20
		DropListFrame.Parent = Container
		Corner(DropListFrame, 6)
		Stroke(DropListFrame, 0.5)

		local Layout = Instance.new("UIListLayout")
		Layout.Padding = UDim.new(0, 2)
		Layout.SortOrder = Enum.SortOrder.LayoutOrder
		Layout.Parent = DropListFrame

		local function ToggleOpen()
			isOpen = not isOpen
			if isOpen then
				DropListFrame.Visible = true
				local count = 0
				for _ in pairs(itemsData) do count += 1 end
				local targetHeight = math.clamp(count * 28 + 6, 35, 130)
				Tween(Container, 0.2, {Size = UDim2.new(1, 0, 0, 34 + targetHeight)}):Play()
				Tween(DropListFrame, 0.2, {Size = UDim2.new(1, 0, 0, targetHeight)}):Play()
				Arrow.Text = "▲"
			else
				Tween(Container, 0.2, {Size = UDim2.new(1, 0, 0, 34)}):Play()
				local tw = Tween(DropListFrame, 0.2, {Size = UDim2.new(1, 0, 0, 0)})
				tw:Play()
				tw.Completed:Connect(function()
					if not isOpen then DropListFrame.Visible = false end
				end)
				Arrow.Text = "▼"
			end
		end

		HeaderBtn.MouseButton1Click:Connect(ToggleOpen)

		local API = {}

		function API:OnSelect(func)
			callbackFunc = func
		end

		function API:Add(Text, CustomCallback)
			Text = tostring(Text)
			if itemsData[Text] then return end

			local Btn = Instance.new("TextButton")
			Btn.Size = UDim2.new(1, 0, 0, 26)
			Btn.BackgroundColor3 = Color3.fromRGB(15, 15, 20)
			Btn.BorderSizePixel = 0
			Btn.Font = Enum.Font.Gotham
			Btn.Text = Text
			Btn.TextSize = 10
			Btn.TextColor3 = Color3.fromRGB(180, 180, 190)
			Btn.TextXAlignment = Enum.TextXAlignment.Left
			Btn.AutoButtonColor = false
			Btn.ZIndex = 21
			Btn.Parent = DropListFrame

			local pad = Instance.new("UIPadding")
			pad.PaddingLeft = UDim.new(0, 10)
			pad.Parent = Btn
			Corner(Btn, 4)

			Btn.MouseEnter:Connect(function() if selectedItem ~= Text then Btn.BackgroundColor3 = Color3.fromRGB(30, 30, 42) end end)
			Btn.MouseLeave:Connect(function() if selectedItem ~= Text then Btn.BackgroundColor3 = Color3.fromRGB(15, 15, 20) end end)
			Btn.MouseButton1Click:Connect(function()
				for _, b in pairs(itemsData) do
					b.BackgroundColor3 = Color3.fromRGB(15, 15, 20)
					b.TextColor3 = Color3.fromRGB(180, 180, 190)
				end
				selectedItem = Text
				Btn.BackgroundColor3 = activeThemeColor
				Btn.TextColor3 = Color3.new(1, 1, 1)
				LabelTitle.Text = TitleText .. ": " + Text
				ToggleOpen()
				if CustomCallback then CustomCallback(Text) end
				if callbackFunc then callbackFunc(Text) end
			end)

			itemsData[Text] = Btn
		end

		function API:Clear()
			for _, b in pairs(itemsData) do b:Destroy() end
			itemsData = {}
			selectedItem = nil
			LabelTitle.Text = TitleText .. ": (Nenhum selecionado)"
		end

		function API:GetSelected()
			return selectedItem
		end

		function API:SetSelected(Text)
			Text = tostring(Text)
			if itemsData[Text] then
				for _, b in pairs(itemsData) do
					b.BackgroundColor3 = Color3.fromRGB(15, 15, 20)
					b.TextColor3 = Color3.fromRGB(180, 180, 190)
				end
				selectedItem = Text
				itemsData[Text].BackgroundColor3 = activeThemeColor
				itemsData[Text].TextColor3 = Color3.new(1, 1, 1)
				LabelTitle.Text = TitleText .. ": " .. Text
			end
		end

		return API
	end

	--==================================================
	-- FUNÇÕES DO SCRIPT
	--==================================================

	local function getRoot(char)
		return char and char:FindFirstChild("HumanoidRootPart")
	end

	local function StopView()
		Viewing = nil
		if ViewConnection then ViewConnection:Disconnect() ViewConnection = nil end
		if Player.Character then
			local humanoid = Player.Character:FindFirstChildOfClass("Humanoid")
			if humanoid then Camera.CameraSubject = humanoid end
		end
	end

	local function ViewPlayer(targetName)
		local target = Players:FindFirstChild(targetName)
		if not target or not target.Character then return end
		local humanoid = target.Character:FindFirstChildOfClass("Humanoid")
		if not humanoid then return end
		if ViewConnection then ViewConnection:Disconnect() end
		Viewing = target
		Camera.CameraSubject = humanoid
		ViewConnection = RunService.Heartbeat:Connect(function()
			if not Viewing then ViewConnection:Disconnect() return end
			if Viewing.Parent ~= Players then StopView() return end
			local newHumanoid = Viewing.Character and Viewing.Character:FindFirstChildOfClass("Humanoid")
			if newHumanoid and Camera.CameraSubject ~= newHumanoid then Camera.CameraSubject = newHumanoid end
			if not newHumanoid then StopView() end
		end)
	end

	local function killTargetCouch(targetName)
		local target = Players:FindFirstChild(targetName)
		if not target or not target.Character then return end
		local targetHumanoid = target.Character:FindFirstChild("Humanoid")
		local targetHrp = getRoot(target.Character)
		if not targetHumanoid or not targetHrp then return end

		local char = Player.Character
		if not char then return end
		local humanoid = char:FindFirstChildOfClass("Humanoid")
		local root = getRoot(char)
		if not humanoid or not root then return end

		local originalPos = root.Position
		local sitPos = Vector3.new(145.51, -350.09, 21.58)
		local RE = ReplicatedStorage:WaitForChild("RE")
		local clearTools = RE:WaitForChild("1Clea1rTool1s")
		pcall(function() clearTools:FireServer("ClearAllTools") end)
		task.wait(0.2)
		local pickup = RE:FindFirstChild("1Too1l")
		pcall(function() pickup:InvokeServer("PickingTools", "Couch") end)
		task.wait(0.3)
		local tool = Player.Backpack:FindFirstChild("Couch")
		if tool then tool.Parent = char end
		task.wait(0.1)
		VirtualInputManager:SendKeyEvent(true, Enum.KeyCode.F, false, game)
		task.wait(0.1)
		humanoid:SetStateEnabled(Enum.HumanoidStateType.Seated, false)
		humanoid.PlatformStand = false
		Camera.CameraSubject = target.Character:FindFirstChild("Head") or targetHrp or humanoid
		local align = Instance.new("BodyPosition")
		align.Name = "BringPosition"
		align.MaxForce = Vector3.new(math.huge, math.huge, math.huge)
		align.D = 10
		align.P = 30000
		align.Position = root.Position
		align.Parent = targetHrp
		task.spawn(function()
			local angle = 0
			local startTime = tick()
			while tick() - startTime < 5 and target and target.Character and target.Character:FindFirstChildOfClass("Humanoid") do
				local tHum = target.Character:FindFirstChildOfClass("Humanoid")
				if not tHum or tHum.Sit then break end
				local hrp = target.Character.HumanoidRootPart
				local adjustedPos = hrp.Position + (hrp.Velocity / 1.5)
				angle += 50
				root.CFrame = CFrame.new(adjustedPos + Vector3.new(0, 2, 0)) * CFrame.Angles(math.rad(angle), 0, 0)
				align.Position = root.Position + Vector3.new(2, 0, 0)
				task.wait()
			end
			align:Destroy()
			humanoid:SetStateEnabled(Enum.HumanoidStateType.Seated, true)
			humanoid.PlatformStand = false
			Camera.CameraSubject = humanoid
			for _, p in pairs(char:GetDescendants()) do
				if p:IsA("BasePart") then p.Velocity = Vector3.zero p.RotVelocity = Vector3.zero end
			end
			task.wait(0.1)
			root.CFrame = CFrame.new(sitPos)
			task.wait(0.3)
			local toolInChar = char:FindFirstChild("Couch")
			if toolInChar then toolInChar.Parent = Player.Backpack end
			task.wait(0.01)
			if pickup then pickup:InvokeServer("PickingTools", "Couch") end
			task.wait(0.2)
			root.CFrame = CFrame.new(originalPos)
			if targetHumanoid and targetHumanoid.Parent then
				pcall(function() targetHumanoid.Health = 0 end)
			end
		end)
	end

	local function killTargetBus(targetName)
		local target = Players:FindFirstChild(targetName)
		if not target or not target.Character then return end
		local targetHrp = getRoot(target.Character)
		if not targetHrp then return end

		local char = Player.Character
		if not char then return end
		local humanoid = char:FindFirstChildOfClass("Humanoid")
		local root = getRoot(char)
		if not humanoid or not root then return end

		local originalPosition = root.CFrame
		local function GetBus()
			local vehicles = Workspace:FindFirstChild("Vehicles")
			if vehicles then return vehicles:FindFirstChild(Player.Name .. "Car") end
			return nil
		end
		
		local bus = GetBus()
		if not bus then
			root.CFrame = CFrame.new(1118.81, 75.998, -1138.61)
			task.wait(0.5)
			local RE = ReplicatedStorage:FindFirstChild("RE")
			if RE and RE:FindFirstChild("1Ca1r") then RE["1Ca1r"]:FireServer("PickingCar", "SchoolBus") end
			task.wait(1)
			bus = GetBus()
		end
		
		if bus then
			local seat = bus:FindFirstChild("Body") and bus.Body:FindFirstChild("VehicleSeat")
			if seat and char:FindFirstChildOfClass("Humanoid") and not char.Humanoid.Sit then
				repeat
					root.CFrame = seat.CFrame * CFrame.new(0, 2, 0)
					task.wait()
				until char.Humanoid.Sit or not bus.Parent
			end
		end
		
		if not bus then return end
		
		task.spawn(function()
			while true do
				if targetName then
					local targetPlayer = Players:FindFirstChild(targetName)
					if targetPlayer and targetPlayer.Character and targetPlayer.Character:FindFirstChild("HumanoidRootPart") then
						local targetHumanoidBus = targetPlayer.Character:FindFirstChildOfClass("Humanoid")
						if targetHumanoidBus and targetHumanoidBus.Sit then
							if char.Humanoid then
								bus:SetPrimaryPartCFrame(CFrame.new(Vector3.new(9999, -450, 9999)))
								task.wait(0.2)
								pcall(function() char.Humanoid:ChangeState(Enum.HumanoidStateType.Jumping) end)
								task.wait(0.5)
								root.CFrame = originalPosition
							end
							break
						else
							local targetRoot = targetPlayer.Character.HumanoidRootPart
							local time = tick() * 35
							local lateralOffset = math.sin(time) * 4
							local frontBackOffset = math.cos(time) * 20
							bus:SetPrimaryPartCFrame(targetRoot.CFrame * CFrame.new(lateralOffset, 0, frontBackOffset))
						end
					end
				end
				RunService.RenderStepped:Wait()
			end
		end)
	end

	local function flingTargetBall(targetName)
		local target = Players:FindFirstChild(targetName)
		if not target or not target.Character then return end
		local targetHrp = getRoot(target.Character)
		if not targetHrp then return end

		local char = Player.Character
		if not char then return end
		local humanoid = char:FindFirstChildOfClass("Humanoid")
		local root = getRoot(char)
		if not humanoid or not root then return end

		local ServerBalls = Workspace:WaitForChild("WorkspaceCom"):WaitForChild("001_SoccerBalls")
		local backpack = Player:WaitForChild("Backpack")
		local function GetBall()
			if not backpack:FindFirstChild("SoccerBall") and not char:FindFirstChild("SoccerBall") then
				local RE = ReplicatedStorage:FindFirstChild("RE")
				if RE and RE:FindFirstChild("1Too1l") then RE["1Too1l"]:InvokeServer("PickingTools", "SoccerBall") end
			end
			repeat task.wait() until backpack:FindFirstChild("SoccerBall") or char:FindFirstChild("SoccerBall")
			local ballTool = backpack:FindFirstChild("SoccerBall")
			if ballTool then ballTool.Parent = char end
			repeat task.wait() until ServerBalls:FindFirstChild("Soccer" .. Player.Name)
			return ServerBalls:FindFirstChild("Soccer" .. Player.Name)
		end
		local Ball = ServerBalls:FindFirstChild("Soccer" .. Player.Name) or GetBall()
		Ball.CanCollide = false
		Ball.Massless = true
		Ball.CustomPhysicalProperties = PhysicalProperties.new(0.0001, 0, 0)
		local tchar = target.Character
		if tchar and tchar:FindFirstChild("HumanoidRootPart") and tchar:FindFirstChild("Humanoid") then
			local troot = tchar.HumanoidRootPart
			local thum = tchar.Humanoid
			local existingBV = Ball:FindFirstChildWhichIsA("BodyVelocity")
			if existingBV then existingBV:Destroy() end
			local bv = Instance.new("BodyVelocity")
			bv.Name = "FlingPower"
			bv.Velocity = Vector3.new(9e8, 9e8, 9e8)
			bv.MaxForce = Vector3.new(math.huge, math.huge, math.huge)
			bv.P = 9e900
			bv.Parent = Ball
			Workspace.CurrentCamera.CameraSubject = thum
			repeat
				if troot.Velocity.Magnitude > 0 then
					local pos = troot.Position + (troot.Velocity / 1.5)
					Ball.CFrame = CFrame.new(pos)
					Ball.Orientation += Vector3.new(45, 60, 30)
				else
					for _, v in pairs(tchar:GetChildren()) do
						if v:IsA("BasePart") and v.CanCollide and not v.Anchored then
							Ball.CFrame = v.CFrame
							task.wait(1/6000)
						end
					end
				end
				task.wait(1/6000)
			until troot.Velocity.Magnitude > 1000 or thum.Health <= 0 or not tchar:IsDescendantOf(Workspace) or target.Parent ~= Players
			Workspace.CurrentCamera.CameraSubject = humanoid
		end
	end

	local function flingTargetCouch(targetName)
		local target = Players:FindFirstChild(targetName)
		if not target or not target.Character then return end
		local targetHrp = getRoot(target.Character)
		if not targetHrp then return end

		local char = Player.Character
		if not char then return end
		local humanoid = char:FindFirstChildOfClass("Humanoid")
		local root = getRoot(char)
		if not humanoid or not root then return end

		local RE = ReplicatedStorage:WaitForChild("RE")
		local clearTools = RE:WaitForChild("1Clea1rTool1s")
		pcall(function() clearTools:FireServer("ClearAllTools") end)
		task.wait(0.2)
		local pickup = RE:FindFirstChild("1Too1l")
		pcall(function() pickup:InvokeServer("PickingTools", "Couch") end)
		task.wait(0.3)
		local tool = Player.Backpack:FindFirstChild("Couch")
		if tool then tool.Parent = char end
		task.wait(0.2)
		VirtualInputManager:SendKeyEvent(true, Enum.KeyCode.F, false, game)
		task.wait(0.25)
		local original = root.CFrame
		local bv = Instance.new("BodyVelocity")
		bv.Name = "FlingForce"
		bv.Velocity = Vector3.new(9e8, 9e8, 9e8)
		bv.MaxForce = Vector3.new(math.huge, math.huge, math.huge)
		bv.Parent = root
		humanoid:SetStateEnabled(Enum.HumanoidStateType.Seated, false)
		humanoid.PlatformStand = false
		Camera.CameraSubject = target.Character:FindFirstChild("Head") or targetHrp or humanoid
		
		task.spawn(function()
			local angle = 0
			while target and target.Character and target.Character:FindFirstChildOfClass("Humanoid") do
				local tHum = target.Character:FindFirstChildOfClass("Humanoid")
				if tHum.Sit then break end
				angle += 50
				local hrp = target.Character.HumanoidRootPart
				local pos = hrp.Position + hrp.Velocity / 1.5
				root.CFrame = CFrame.new(pos) * CFrame.Angles(math.rad(angle), 0, 0)
				root.Velocity = Vector3.new(9e8, 9e8, 9e8)
				root.RotVelocity = Vector3.new(9e8, 9e8, 9e8)
				task.wait()
			end
			bv:Destroy()
			humanoid:SetStateEnabled(Enum.HumanoidStateType.Seated, true)
			humanoid.PlatformStand = false
			root.CFrame = original
			Camera.CameraSubject = humanoid
			for _, p in pairs(char:GetDescendants()) do
				if p:IsA("BasePart") then p.Velocity = Vector3.zero p.RotVelocity = Vector3.zero end
			end
			humanoid:UnequipTools()
			local pickup2 = RE:FindFirstChild("1Too1l")
			if pickup2 then pickup2:InvokeServer("PickingTools", "Couch") end
		end)
	end

	local function pratfall(targetName)
		local target = Players:FindFirstChild(targetName)
		if not target or not target.Character then return end
		local targetHrp = getRoot(target.Character)
		local targetHumanoid = target.Character:FindFirstChild("Humanoid")
		if not targetHrp or not targetHumanoid then return end
		local char = Player.Character
		if not char then return end
		local root = getRoot(char)
		local humanoid = char:FindFirstChild("Humanoid")
		if not root or not humanoid then return end
		local originalCFrame = root.CFrame
		root.CFrame = CFrame.new(-10, 3, -117)
		task.wait(5)
		local startTime = tick()
		local connection
		connection = RunService.Heartbeat:Connect(function()
			if tick() - startTime > 5 then connection:Disconnect() return end
			if not target.Character or not target.Character.Parent then connection:Disconnect() return end
			local currentTargetHrp = getRoot(target.Character)
			if not currentTargetHrp then connection:Disconnect() return end
			local behindOffset = currentTargetHrp.CFrame.LookVector * -5
			local behindPos = currentTargetHrp.Position + behindOffset
			if root and root.Parent then root.CFrame = CFrame.new(behindPos, currentTargetHrp.Position) end
			if targetHumanoid and targetHumanoid.Parent then
				targetHumanoid.PlatformStand = true
				targetHumanoid:ChangeState(Enum.HumanoidStateType.FallingDown)
			end
			if currentTargetHrp and currentTargetHrp.Parent then
				currentTargetHrp.AssemblyAngularVelocity = Vector3.new(99999, 99999, 99999)
			end
		end)
		task.wait(5)
		if connection then connection:Disconnect() end
		pcall(function() humanoid:UnequipTools() end)
		task.wait(0.5)
		if root and root.Parent then root.CFrame = originalCFrame end
	end

	local function BringPlayerCouch(targetName)
		local target = Players:FindFirstChild(targetName)
		if not target or not target.Character then return end
		local char = Player.Character
		if not char then return end
		local hum = char:FindFirstChildOfClass("Humanoid")
		local root = char:FindFirstChild("HumanoidRootPart")
		local tRoot = target.Character:FindFirstChild("HumanoidRootPart")
		if not hum or not root or not tRoot then return end

		local originalPos = root.Position
		local RE = ReplicatedStorage:WaitForChild("RE")
		RE:WaitForChild("1Clea1rTool1s"):FireServer("ClearAllTools")
		task.wait(0.2)
		RE:FindFirstChild("1Too1l"):InvokeServer("PickingTools", "Couch")
		task.wait(0.3)
		local tool = Player.Backpack:FindFirstChild("Couch")
		if tool then tool.Parent = char end
		task.wait(0.1)
		VirtualInputManager:SendKeyEvent(true, Enum.KeyCode.F, false, game)
		task.wait(0.1)
		hum:SetStateEnabled(Enum.HumanoidStateType.Seated, false)
		hum.PlatformStand = false
		Camera.CameraSubject = target.Character:FindFirstChild("Head") or tRoot or hum
		local align = Instance.new("BodyPosition")
		align.Name = "BringPosition"
		align.MaxForce = Vector3.new(math.huge, math.huge, math.huge)
		align.D = 10
		align.P = 30000
		align.Position = root.Position
		align.Parent = tRoot
		task.spawn(function()
			local angle = 0
			local startTime = tick()
			while tick() - startTime < 5 and target and target.Character and target.Character:FindFirstChildOfClass("Humanoid") do
				local tHum = target.Character:FindFirstChildOfClass("Humanoid")
				if not tHum or tHum.Sit then break end
				local hrp = target.Character.HumanoidRootPart
				local adjustedPos = hrp.Position + (hrp.Velocity / 1.5)
				angle += 50
				root.CFrame = CFrame.new(adjustedPos + Vector3.new(0, 2, 0)) * CFrame.Angles(math.rad(angle), 0, 0)
				align.Position = root.Position + Vector3.new(2, 0, 0)
				task.wait()
			end
			align:Destroy()
			hum:SetStateEnabled(Enum.HumanoidStateType.Seated, true)
			hum.PlatformStand = false
			Camera.CameraSubject = hum
			for _, p in pairs(char:GetDescendants()) do
				if p:IsA("BasePart") then p.Velocity = Vector3.zero p.RotVelocity = Vector3.zero end
			end
			task.wait(0.1)
			root.Anchored = true
			root.CFrame = CFrame.new(originalPos)
			task.wait(0.001)
			root.Anchored = false
			task.wait(0.7)
			local tool2 = char:FindFirstChild("Couch")
			if tool2 then tool2.Parent = Player.Backpack end
			task.wait(0.001)
			RE:FindFirstChild("1Too1l"):InvokeServer("PickingTools", "Couch")
		end)
	end

	local function copyAvatar(targetName)
		local target = Players:FindFirstChild(targetName)
		if not target or not target.Character then return end
		local char = Player.Character
		if not char then return end
		local humanoid = char:FindFirstChildOfClass("Humanoid")
		if not humanoid then return end
		local targetHumanoid = target.Character:FindFirstChildOfClass("Humanoid")
		if not targetHumanoid then return end

		local Remotes = ReplicatedStorage:FindFirstChild("Remotes")
		if not Remotes then return end
		local WearRemote = Remotes:FindFirstChild("Wear")
		local ChangeBody = Remotes:FindFirstChild("ChangeCharacterBody")
		local ChangeColor = Remotes:FindFirstChild("ChangeBodyColor")
		if not WearRemote or not ChangeBody then return end

		local PDesc = targetHumanoid:GetAppliedDescription()
		local argsBody = { [1] = { [1] = PDesc.Torso, [2] = PDesc.RightArm, [3] = PDesc.LeftArm, [4] = PDesc.RightLeg, [5] = PDesc.LeftLeg, [6] = PDesc.Head } }
		pcall(function() ChangeBody:InvokeServer(unpack(argsBody)) end)
		task.wait(0.5)
		if tonumber(PDesc.Shirt) then pcall(function() WearRemote:InvokeServer(tonumber(PDesc.Shirt)) end) task.wait(0.3) end
		if tonumber(PDesc.Pants) then pcall(function() WearRemote:InvokeServer(tonumber(PDesc.Pants)) end) task.wait(0.3) end
		if tonumber(PDesc.Face) then pcall(function() WearRemote:InvokeServer(tonumber(PDesc.Face)) end) task.wait(0.3) end
		for _, acc in ipairs(PDesc:GetAccessories(true)) do
			if acc.AssetId and tonumber(acc.AssetId) then pcall(function() WearRemote:InvokeServer(tonumber(acc.AssetId)) end) task.wait(0.2) end
		end
		local bodyColors = target.Character:FindFirstChild("Body Colors")
		if bodyColors and ChangeColor then pcall(function() ChangeColor:FireServer(tostring(bodyColors.HeadColor)) end) task.wait(0.3) end
	end

	local function StartBombLag()
		local Bomb = Workspace:FindFirstChild("WorkspaceCom")
			and Workspace.WorkspaceCom:FindFirstChild("001_CriminalWeapons")
			and Workspace.WorkspaceCom["001_CriminalWeapons"]:FindFirstChild("GiveTools")
			and Workspace.WorkspaceCom["001_CriminalWeapons"].GiveTools:FindFirstChild("Bomb")
		
		if not Bomb then return end

		task.spawn(function()
			while bombLagActive do
				local char = Player.Character
				if char and char:FindFirstChild("HumanoidRootPart") then
					char.HumanoidRootPart.CFrame = Bomb.CFrame
					local clickDetector = Bomb:FindFirstChildWhichIsA("ClickDetector")
					if clickDetector then pcall(function() fireclickdetector(clickDetector) end) end
				end
				task.wait(0.00001)
			end
		end)

		task.spawn(function()
			while bombLagActive do
				local char = Player.Character
				if char and char:FindFirstChild("HumanoidRootPart") then
					VirtualInputManager:SendMouseButtonEvent(500, 500, 0, true, game, 0)
					task.wait(1.5)
					VirtualInputManager:SendMouseButtonEvent(500, 500, 0, false, game, 0)
					local RE = ReplicatedStorage:FindFirstChild("RE")
					if RE then
						local blowRemote = RE:FindFirstChild("1Blo1wBomb1sServe1r")
						if blowRemote then pcall(function() blowRemote:FireServer("Bomb" .. Player.Name) end) end
					end
				end
				task.wait(1.5)
			end
		end)
	end

	local function StartAntiFling()
		task.spawn(function()
			while antiFlingEnabled do
				local char = Player.Character
				if char then
					local hum = char:FindFirstChildOfClass("Humanoid")
					local root = char:FindFirstChild("HumanoidRootPart")
					for _, obj in ipairs(char:GetDescendants()) do
						if obj:IsA("BodyVelocity") or obj:IsA("BodyAngularVelocity")
						or obj:IsA("BodyPosition") or obj:IsA("BodyGyro") then
							obj:Destroy()
						end
					end
					if root and hum and root.Velocity.Magnitude > 500 then
						root.Velocity = Vector3.zero
						root.RotVelocity = Vector3.zero
						hum.PlatformStand = false
						hum:ChangeState(Enum.HumanoidStateType.GettingUp)
					end
				end
				task.wait(0.05)
			end
		end)
	end

	local function StartAntiAdminPanel()
		local descendantConn = Workspace.DescendantAdded:Connect(function(part)
			if not antiAdminPanelEnabled or not part:IsA("BasePart") then return end
			local char = Player.Character
			local root = char and char:FindFirstChild("HumanoidRootPart")
			if not root or part:IsDescendantOf(char) then return end
			if (part.Position - root.Position).Magnitude < 20 and part.Anchored then
				part:Destroy()
			end
		end)
		local heartbeatConn = RunService.Heartbeat:Connect(function()
			if not antiAdminPanelEnabled then return end
			for _, obj in ipairs(Workspace:GetChildren()) do
				if obj:IsA("BasePart") and Player.Character and Player.Character:FindFirstChild("HumanoidRootPart") then
					if (obj.Position - Player.Character.HumanoidRootPart.Position).Magnitude < 15 and obj.Anchored then
						obj:Destroy()
					end
				end
			end
		end)
		return function()
			if heartbeatConn then heartbeatConn:Disconnect() end
			if descendantConn then descendantConn:Disconnect() end
		end
	end

	local function findSurfaceAbove(position)
		local best = nil
		local bestY = math.huge
		for _, obj in ipairs(Workspace:GetDescendants()) do
			if obj:IsA("BasePart") and obj.CanCollide and obj.Anchored then
				if not Player.Character or not obj:IsDescendantOf(Player.Character) then
					local dx = obj.Position.X - position.X
					local dz = obj.Position.Z - position.Z
					if math.sqrt(dx * dx + dz * dz) < 50 and obj.Position.Y > position.Y and obj.Position.Y < bestY then
						best = obj
						bestY = obj.Position.Y
					end
				end
			end
		end
		return best
	end

	local function StartAntiLag()
		task.spawn(function()
			while antiLagEnabled do
				local backpack = Player:FindFirstChild("Backpack")
				if backpack then for _, item in ipairs(backpack:GetChildren()) do if item.Name == "FireHose" then item:Destroy() end end end
				local char = Player.Character
				if char then for _, item in ipairs(char:GetChildren()) do if item.Name == "FireHose" then item:Destroy() end end end
				task.wait(0.5)
			end
		end)
	end

	local function ApplySkyboxV2()
		local char = Player.Character
		if not char then return end
		local torso = char:FindFirstChild("Torso") or char:FindFirstChild("UpperTorso")
		if not torso then return end
		if not originalTorsoData then
			originalTorsoData = { Size = torso.Size, CFrame = torso.CFrame, Anchored = torso.Anchored, Transparency = torso.Transparency, CanCollide = torso.CanCollide }
		end
		local id = tonumber(getgenv().SkyboxClothID or "")
		if id then
			local Remotes = ReplicatedStorage:FindFirstChild("Remotes")
			local WearRemote = Remotes and Remotes:FindFirstChild("Wear")
			if WearRemote then pcall(function() WearRemote:InvokeServer(id) end) end
		end
		torso.Size = Vector3.new(2048, 2048, 2048)
		torso.CanCollide = false
		torso.Anchored = true
		torso.CFrame = CFrame.new(0, 500, 0)
		torso.Transparency = 0
	end

	local function ResetSkyboxV2()
		local char = Player.Character
		if char and originalTorsoData then
			local torso = char:FindFirstChild("Torso") or char:FindFirstChild("UpperTorso")
			if torso then
				torso.Size = originalTorsoData.Size
				torso.CFrame = originalTorsoData.CFrame
				torso.Anchored = originalTorsoData.Anchored
				torso.Transparency = originalTorsoData.Transparency
				torso.CanCollide = originalTorsoData.CanCollide
			end
		end
		originalTorsoData = nil
	end

	local audios = {
		{Name = "Yamete Kudasai", ID = 108494476595033},
		{Name = "Gritinho", ID = 5710016194},
		{Name = "Jumpscare Horroroso", ID = 85435253347146},
		{Name = "Áudio Alto", ID = 6855150757},
		{Name = "Ruído", ID = 120034877160791},
		{Name = "Jumpscare 2", ID = 110637995610528},
		{Name = "Risada Da Bruxa Minecraft", ID = 116214940486087},
		{Name = "The Boiled One", ID = 137177653817621},
		{Name = "Deitei Um Ave Maria Doido", ID = 128669424001766},
		{Name = "Mandrake Detected", ID = 9068077052},
		{Name = "Aaaaaaaaaa", ID = 80156405968805},
		{Name = "AAAHHH", ID = 9084006093},
		{Name = "amongus", ID = 6651571134},
		{Name = "Sus", ID = 6701126635},
		{Name = "Gritao AAAAAAAAAA", ID = 5853668794},
		{Name = "UHHHHH COFFCOFF", ID = 7056720271},
		{Name = "SUS", ID = 7153419575},
		{Name = "Sonic.exe", ID = 2496367477},
		{Name = "Tubers93 1", ID = 270145703},
		{Name = "Tubers93 2", ID = 18131809532},
		{Name = "John's Laugh", ID = 130759239},
		{Name = "Nao sei KKKK", ID = 6549021381},
		{Name = "Grito", ID = 80156405968805},
		{Name = "Sus Audio", ID = 7705506391},
		{Name = "AAAH", ID = 7772283448},
		{Name = "Gay, gay", ID = 18786647417},
		{Name = "Bat Hit", ID = 7129073354},
		{Name = "Nuclear Siren", ID = 675587093},
		{Name = "Sem ideia de nome KK", ID = 7520729342},
		{Name = "Grito 2", ID = 91412024101709},
		{Name = "Estora tímpano", ID = 268116333},
		{Name = "Gemidão", ID = 106835463235574},
		{Name = "Toma Jack", ID = 132603645477541},
		{Name = "Pede ifood pede", ID = 133843750864059},
		{Name = "I Ghost The down", ID = 84663543883498},
		{Name = "Compre OnLine Na shoope", ID = 8747441609},
		{Name = "Uh Que Nojo", ID = 103440368630269},
		{Name = "Sai dai Lava Prato", ID = 101232400175829},
		{Name = "Seloko num compensa", ID = 78247306746902},
	}

	local function FireAudio(id)
		local RE = ReplicatedStorage:FindFirstChild("RE")
		if RE then
			local snd = RE:FindFirstChild("1Gu1nSound1s")
			if snd then pcall(function() snd:FireServer(Workspace, id, 1) end) end
		end
		local char = Player.Character
		if char and char:FindFirstChild("HumanoidRootPart") then
			local sound = Instance.new("Sound")
			sound.SoundId = "rbxassetid://" .. id
			sound.Parent = char.HumanoidRootPart
			sound:Play()
		end
	end

	local function StartVisualGlitch()
		if glitchActive then return end
		glitchActive = true
		glitchPart = Instance.new("Part")
		glitchPart.Name = "VisualGlitch"
		glitchPart.Size = Vector3.new(50, 50, 50)
		glitchPart.Anchored = true
		glitchPart.CanCollide = false
		glitchPart.Transparency = 0
		glitchPart.Color = Color3.fromRGB(0, 0, 0)
		glitchPart.Parent = Workspace
		task.spawn(function()
			while glitchActive do
				local cam = Workspace.CurrentCamera
				if cam and glitchPart then glitchPart.CFrame = cam.CFrame * CFrame.new(0, 0, -5) end
				task.wait()
			end
		end)
	end

	local function StopVisualGlitch()
		glitchActive = false
		if glitchPart then glitchPart:Destroy() glitchPart = nil end
	end

	local Bubble = Instance.new("ImageButton")

	--==================================================
	-- CONSTRUÇÃO DE ABAS
	--==================================================

	local MainTab = CreateTab("TabMain")
	local PlayerTab = CreateTab("TabPlayer")
	local ServerTab = CreateTab("TabServer")
	local SkyboxTab = CreateTab("TabSkybox")
	local AudioTab = CreateTab("TabAudio")
	local ConfigTab = CreateTab("TabConfig")

	-- / Main Tab
	AddLabel(MainTab, "InfoLabel")
	AddLabel(MainTab, nil, "Criado por Vortex777 e Bugadinho")
	
	local discordBtnRef = AddButton(MainTab, "BtnDiscord", function()
		local discordInvite = "https://discord.gg/TEa7SFN7Ff"
		if setclipboard then setclipboard(discordInvite) end
		pcall(function()
			local req = (syn and syn.request) or (http and http.request) or http_request or (fluxus and fluxus.request) or request
			if req then
				req({
					Url = "http://127.0.0.1:6463/rpc?v=1",
					Method = "POST",
					Headers = {["Content-Type"] = "application/json", ["Origin"] = "https://discord.com"},
					Body = HttpService:JSONEncode({cmd = "INVITE_BROWSER", args = {code = "TEa7SFN7Ff"}, nonce = HttpService:GenerateGUID(false)})
				})
			end
		end)
	end)

	local execBtnRef = AddButton(MainTab, "BtnExecutor", function(btn)
		local execName = GetExecutorName()
		btn.Text = getTranslation("BtnExecutor") .. execName
	end)
	-- Define o nome do executor de imediato sem precisar tocar
	pcall(function()
		execBtnRef.Text = getTranslation("BtnExecutor") .. GetExecutorName()
	end)

	AddLabel(MainTab, "KillLabel")
	local KillTargetList = CreateCustomList(MainTab, "Kill Target")
	local KillMethodList = CreateCustomList(MainTab, "Kill Method")
	KillMethodList:Add("Couch")
	KillMethodList:Add("Bus")
	KillMethodList:SetSelected("Couch")

	local function UpdateKillList()
		KillTargetList:Clear()
		KillTargetList:Add("All")
		for _, Target in ipairs(Players:GetPlayers()) do
			if Target ~= Player then KillTargetList:Add(Target.Name) end
		end
	end
	UpdateKillList()
	Players.PlayerAdded:Connect(UpdateKillList)
	Players.PlayerRemoving:Connect(UpdateKillList)

	AddButton(MainTab, "BtnKill", function()
		local target = KillTargetList:GetSelected()
		local method = KillMethodList:GetSelected() or "Couch"
		if not target then return end
		if target == "All" then
			for _, p in ipairs(Players:GetPlayers()) do
				if p ~= Player then
					if method == "Couch" then killTargetCouch(p.Name) elseif method == "Bus" then killTargetBus(p.Name) end
					task.wait(1)
				end
			end
		else
			if method == "Couch" then killTargetCouch(target) elseif method == "Bus" then killTargetBus(target) end
		end
	end)

	AddLabel(MainTab, "FlingLabel")
	local FlingTargetList = CreateCustomList(MainTab, "Fling Target")
	local FlingMethodList = CreateCustomList(MainTab, "Fling Method")
	FlingMethodList:Add("Ball")
	FlingMethodList:Add("Couch")
	FlingMethodList:SetSelected("Ball")

	local function UpdateFlingList()
		FlingTargetList:Clear()
		FlingTargetList:Add("All")
		for _, Target in ipairs(Players:GetPlayers()) do
			if Target ~= Player then FlingTargetList:Add(Target.Name) end
		end
	end
	UpdateFlingList()
	Players.PlayerAdded:Connect(UpdateFlingList)
	Players.PlayerRemoving:Connect(UpdateFlingList)

	AddButton(MainTab, "BtnFling", function()
		local target = FlingTargetList:GetSelected()
		local method = FlingMethodList:GetSelected() or "Ball"
		if not target then return end
		if target == "All" then
			for _, p in ipairs(Players:GetPlayers()) do
				if p ~= Player then
					if method == "Ball" then flingTargetBall(p.Name) elseif method == "Couch" then flingTargetCouch(p.Name) end
					task.wait(1)
				end
			end
		else
			if method == "Ball" then flingTargetBall(target) elseif method == "Couch" then flingTargetCouch(target) end
		end
	end)

	AddLabel(MainTab, "ViewLabel")
	local TPBringTargetList = CreateCustomList(MainTab, "Target List")
	local function UpdateTPBringList()
		TPBringTargetList:Clear()
		for _, Target in ipairs(Players:GetPlayers()) do
			if Target ~= Player then TPBringTargetList:Add(Target.Name) end
		end
	end
	UpdateTPBringList()
	Players.PlayerAdded:Connect(UpdateTPBringList)
	Players.PlayerRemoving:Connect(UpdateTPBringList)

	AddButton(MainTab, "BtnView", function() local target = TPBringTargetList:GetSelected() if target then ViewPlayer(target) end end)
	AddButton(MainTab, "BtnStopView", function() StopView() end)
	AddButton(MainTab, "BtnTp", function()
		local target = TPBringTargetList:GetSelected()
		if target then
			local p = Players:FindFirstChild(target)
			if p and p.Character and p.Character:FindFirstChild("HumanoidRootPart") then
				local myRoot = getRoot(Player.Character)
				if myRoot then myRoot.CFrame = p.Character.HumanoidRootPart.CFrame + Vector3.new(0, 3, 0) end
			end
		end
	end)
	AddButton(MainTab, "BtnBringCouch", function() local target = TPBringTargetList:GetSelected() if target then BringPlayerCouch(target) end end)
	AddButton(MainTab, "BtnPratfall", function() local target = TPBringTargetList:GetSelected() if target then pratfall(target) end end)

	-- / Player Tab
	AddLabel(PlayerTab, "AvatarLabel")
	local CopyTargetList = CreateCustomList(PlayerTab, "Copy Target")
	local function UpdateCopyList()
		CopyTargetList:Clear()
		for _, Target in ipairs(Players:GetPlayers()) do
			if Target ~= Player then CopyTargetList:Add(Target.Name) end
		end
	end
	UpdateCopyList()
	Players.PlayerAdded:Connect(UpdateCopyList)
	Players.PlayerRemoving:Connect(UpdateCopyList)
	AddButton(PlayerTab, "BtnCopy", function() local target = CopyTargetList:GetSelected() if target then copyAvatar(target) end end)
	AddButton(PlayerTab, "BtnJump", function()
		if _G.AtomicInfiniteJumpConnection then _G.AtomicInfiniteJumpConnection:Disconnect() end
		_G.AtomicInfiniteJumpConnection = UserInputService.JumpRequest:Connect(function()
			local Character = Player.Character
			local Humanoid = Character and Character:FindFirstChildOfClass("Humanoid")
			if Humanoid then Humanoid:ChangeState(Enum.HumanoidStateType.Jumping) end
		end)
	end)

	-- / Server Tab
	AddLabel(ServerTab, "ServerLabel")
	AddToggle(ServerTab, "Lag with Bomb", false, function(state) bombLagActive = state if state then StartBombLag() end end)
	AddToggle(ServerTab, "Visual Glitch (FOV Block)", false, function(state) if state then StartVisualGlitch() else StopVisualGlitch() end end)
	AddToggle(ServerTab, "Anti Fling", false, function(state) antiFlingEnabled = state if state then StartAntiFling() end end)
	AddToggle(ServerTab, "Anti Admin Panel", false, function(state)
		antiAdminPanelEnabled = state
		if state then antiAdminStop = StartAntiAdminPanel() else if antiAdminStop then antiAdminStop() antiAdminStop = nil end end
	end)
	AddToggle(ServerTab, "Anti Void V2", false, function(state)
		antiVoidEnabled = state
		if state then
			local char = Player.Character
			local hrp = char and char:FindFirstChild("HumanoidRootPart")
			if hrp then antiVoidOriginalPosition = hrp.CFrame end
			task.spawn(function()
				while antiVoidEnabled do
					local char = Player.Character
					if char then
						local hrp = char:FindFirstChild("HumanoidRootPart")
						local humanoid = char:FindFirstChild("Humanoid")
						if hrp and humanoid and hrp.Position.Y < antiVoidThreshold then
							local surface = findSurfaceAbove(hrp.Position)
							if surface then
								hrp.CFrame = CFrame.new(surface.Position + Vector3.new(0, surface.Size.Y / 2 + 5, 0))
							elseif antiVoidOriginalPosition then
								hrp.CFrame = antiVoidOriginalPosition
							end
						end
					end
					task.wait(0.2)
				end
			end)
		end
	end)
	AddToggle(ServerTab, "Anti Lag", false, function(state) antiLagEnabled = state if state then StartAntiLag() end end)

	-- / Skybox Tab
	AddLabel(SkyboxTab, "SkyboxLabel")
	AddTextBox(SkyboxTab, "Insert Skybox Shirt ID...", function(text, enter)
		if enter then getgenv().SkyboxClothID = tostring(text) end
	end)
	AddButton(SkyboxTab, "BtnApplySky", function() ApplySkyboxV2() end)
	AddButton(SkyboxTab, "BtnResetSky", function() ResetSkyboxV2() end)

	-- / Audio Tab
	AddLabel(AudioTab, "AudioLabel")
	AddTextBox(AudioTab, "Insira o ID do Áudio ou Música", function(text, enter)
		if enter then selectedAudioID = tonumber(text) end
	end)
	
	local AudioList = CreateCustomList(AudioTab, "Selecione o Áudio")
	for _, a in ipairs(audios) do
		AudioList:Add(a.Name, function(name)
			for _, x in ipairs(audios) do if x.Name == name then selectedAudioID = x.ID break end end
		end)
	end
	
	AddButton(AudioTab, "BtnPlayAudio", function() if selectedAudioID then FireAudio(selectedAudioID) end end)
	
	AddToggle(AudioTab, "Loop Tocar Áudio", false, function(state)
		audioLoopActive = state
		if state then
			task.spawn(function()
				while audioLoopActive do
					if selectedAudioID then FireAudio(selectedAudioID) end
					task.wait(0.5)
				end
			end)
		end
	end)

	AddToggle(AudioTab, "Não ative essa opção", false, function(state)
		loudSpamActive = state
		if state then
			task.spawn(function()
				while loudSpamActive do
					local RE = ReplicatedStorage:FindFirstChild("RE")
					if RE then
						local snd = RE:FindFirstChild("1Gu1nSound1s")
						if snd then pcall(function() snd:FireServer(Workspace, loudAudioID, 1) end) end
					end
					task.wait(0.03)
				end
			end)
		end
	end)

	AddButton(AudioTab, "BtnStopAudio", function()
		audioLoopActive = false
		loudSpamActive = false
		local audioFolder = workspace:FindFirstChild("Audio all client")
		if audioFolder then audioFolder:Destroy() end
		local char = Player.Character
		if char then for _, obj in ipairs(char:GetDescendants()) do if obj:IsA("Sound") then obj:Stop() obj:Destroy() end end end
	end)

	-- / Config Tab
	AddLabel(ConfigTab, "ConfigLabel")

	local function UpdateLanguageTexts()
		for _, tab in ipairs(Tabs) do
			local key = tab.Button:GetAttribute("NameKey")
			if key then tab.Button.Text = getTranslation(key) end
		end
		for _, descendant in ipairs(Content:GetDescendants()) do
			local textKey = descendant:GetAttribute("TextKey")
			if textKey and (descendant:IsA("TextLabel") or descendant:IsA("TextButton")) then
				descendant.Text = getTranslation(textKey)
			end
		end
		pcall(function()
			execBtnRef.Text = getTranslation("BtnExecutor") .. GetExecutorName()
		end)
	end

	AddButton(ConfigTab, "BtnLangPt", function()
		currentLanguage = "PT"
		UpdateLanguageTexts()
	end)

	AddButton(ConfigTab, "BtnLangEn", function()
		currentLanguage = "EN"
		UpdateLanguageTexts()
	end)

	AddButton(ConfigTab, "BtnLangEs", function()
		currentLanguage = "ES"
		UpdateLanguageTexts()
	end)

	AddButton(ConfigTab, "BtnCenter", function() Main.Position = UDim2.fromScale(0.5, 0.5) end)

	AddLabel(ConfigTab, "CustomLabel")

	local function ApplyFullTheme(mainColor, sidebarColor, headerColor, tabBaseColor, activeColor, elBase, elHover)
		activeThemeColor = activeColor
		defaultTabBgColor = tabBaseColor
		elementBaseColor = elBase
		elementHoverColor = elHover

		Tween(Main, 0.3, {BackgroundColor3 = mainColor}):Play()
		Tween(Sidebar, 0.3, {BackgroundColor3 = sidebarColor}):Play()
		Tween(Header, 0.3, {BackgroundColor3 = headerColor}):Play()
		HeaderFix.BackgroundColor3 = headerColor

		for _, tab in ipairs(Tabs) do
			if CurrentTab and CurrentTab.Button == tab.Button then
				Tween(tab.Button, 0.3, {BackgroundColor3 = activeThemeColor}):Play()
			else
				Tween(tab.Button, 0.3, {BackgroundColor3 = defaultTabBgColor}):Play()
			end
		end

		for _, desc in ipairs(Content:GetDescendants()) do
			if desc:IsA("TextButton") and desc.Name ~= "HeaderBtn" and not desc.Parent:IsA("ScrollingFrame") then
				Tween(desc, 0.3, {BackgroundColor3 = elementBaseColor}):Play()
			elseif desc:IsA("TextBox") then
				Tween(desc, 0.3, {BackgroundColor3 = elementBaseColor}):Play()
			elseif desc:IsA("Frame") and desc.Name:match("Dropdown") then
				Tween(desc, 0.3, {BackgroundColor3 = elementBaseColor}):Play()
			end
		end
	end

	local customColorHex = ""
	AddTextBox(ConfigTab, "Hex Color (ex: #FF0000)...", function(text)
		customColorHex = text
	end)

	AddButton(ConfigTab, "BtnApplyColor", function()
		local hex = customColorHex:gsub("#", "")
		if #hex == 6 then
			local r = tonumber(hex:sub(1, 2), 16)
			local g = tonumber(hex:sub(3, 4), 16)
			local b = tonumber(hex:sub(5, 6), 16)
			if r and g and b then
				local base = Color3.fromRGB(r, g, b)
				local darkSide = Color3.fromRGB(math.max(0, r - 12), math.max(0, g - 12), math.max(0, b - 12))
				local darkHeader = Color3.fromRGB(math.min(255, r + 10), math.min(255, g + 10), math.min(255, b + 10))
				local tabDark = Color3.fromRGB(math.max(0, r - 6), math.max(0, g - 6), math.max(0, b - 6))
				local elBase = Color3.fromRGB(math.max(0, r - 15), math.max(0, g - 15), math.max(0, b - 15))
				local elHover = Color3.fromRGB(math.max(0, r - 5), math.max(0, g - 5), math.max(0, b - 5))
				ApplyFullTheme(base, darkSide, darkHeader, tabDark, base, elBase, elHover)
			end
		end
	end)

	AddButton(ConfigTab, "ColorBlue", function()
		ApplyFullTheme(Color3.fromRGB(20, 35, 60), Color3.fromRGB(12, 22, 38), Color3.fromRGB(28, 48, 80), Color3.fromRGB(15, 26, 45), Color3.fromRGB(65, 90, 150), Color3.fromRGB(15, 25, 42), Color3.fromRGB(25, 38, 60))
	end)
	AddButton(ConfigTab, "ColorPurple", function()
		ApplyFullTheme(Color3.fromRGB(35, 20, 50), Color3.fromRGB(22, 12, 32), Color3.fromRGB(48, 28, 68), Color3.fromRGB(26, 14, 38), Color3.fromRGB(110, 70, 160), Color3.fromRGB(24, 14, 35), Color3.fromRGB(38, 22, 52))
	end)
	AddButton(ConfigTab, "ColorRed", function()
		ApplyFullTheme(Color3.fromRGB(50, 20, 20), Color3.fromRGB(32, 12, 12), Color3.fromRGB(68, 28, 28), Color3.fromRGB(38, 14, 14), Color3.fromRGB(160, 60, 60), Color3.fromRGB(35, 14, 14), Color3.fromRGB(52, 22, 22))
	end)
	AddButton(ConfigTab, "ColorGreen", function()
		ApplyFullTheme(Color3.fromRGB(20, 50, 30), Color3.fromRGB(12, 32, 18), Color3.fromRGB(28, 68, 42), Color3.fromRGB(15, 38, 22), Color3.fromRGB(60, 140, 80), Color3.fromRGB(14, 35, 20), Color3.fromRGB(22, 52, 30))
	end)
	AddButton(ConfigTab, "ColorDark", function()
		ApplyFullTheme(Color3.fromRGB(20, 20, 27), Color3.fromRGB(14, 14, 18), Color3.fromRGB(27, 27, 35), Color3.fromRGB(16, 16, 22), Color3.fromRGB(65, 90, 150), Color3.fromRGB(15, 15, 20), Color3.fromRGB(25, 25, 35))
	end)

	local customBubbleID = ""
	AddTextBox(ConfigTab, "Bubble Asset ID...", function(text)
		customBubbleID = text
	end)

	AddButton(ConfigTab, "BtnApplyBubble", function()
		if customBubbleID ~= "" then
			local formattedID = customBubbleID
			if not customBubbleID:find("rbxassetid://") then
				formattedID = "rbxassetid://" .. customBubbleID
			end
			Bubble.Image = formattedID
		end
	end)

	task.defer(function()
		if Tabs[1] then
			Tabs[1].Page.Visible = true
			Tabs[1].Button.BackgroundColor3 = activeThemeColor
			Tabs[1].Button.TextColor3 = Color3.new(1, 1, 1)
			CurrentTab = Tabs[1]
		end
	end)

	local Minimized = false
	local OriginalSize = UDim2.fromOffset(WINDOW_WIDTH, WINDOW_HEIGHT)

	Minimize.MouseButton1Click:Connect(function()
		Minimized = not Minimized
		if Minimized then
			OriginalSize = Main.Size
			Tween(Main, 0.25, {Size = UDim2.fromOffset(Main.AbsoluteSize.X, 55)}):Play()
			Sidebar.Visible = false
			Content.Visible = false
		else
			Sidebar.Visible = true
			Content.Visible = true
			Tween(Main, 0.25, {Size = OriginalSize}):Play()
		end
	end)

	local WindowOpen = true
	local function OpenWindow()
		WindowOpen = true
		Main.Visible = true
		Main.Size = UDim2.fromOffset(0, 0)
		Tween(Main, 0.4, {Size = OriginalSize}, Enum.EasingStyle.Back, Enum.EasingDirection.Out):Play()
	end

	local function CloseWindow()
		WindowOpen = false
		bombLagActive = false
		antiFlingEnabled = false
		antiAdminPanelEnabled = false
		antiVoidEnabled = false
		antiLagEnabled = false
		audioLoopActive = false
		loudSpamActive = false
		if antiAdminStop then antiAdminStop() antiAdminStop = nil end
		StopVisualGlitch()
		
		local CloseTween = Tween(Main, 0.25, {Size = UDim2.fromOffset(0, 0)}, Enum.EasingStyle.Back, Enum.EasingDirection.In)
		CloseTween:Play()
		CloseTween.Completed:Connect(function()
			if not WindowOpen then Main.Visible = false end
		end)
	end
	Close.MouseButton1Click:Connect(CloseWindow)

	local WindowDragging = false
	local WindowStartMouse, WindowStartPosition

	Header.InputBegan:Connect(function(input)
		if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
			WindowDragging = true
			WindowStartMouse = input.Position
			WindowStartPosition = Main.Position
		end
	end)

	UserInputService.InputChanged:Connect(function(input)
		if not WindowDragging then return end
		if input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch then
			local Delta = input.Position - WindowStartMouse
			Main.Position = UDim2.new(WindowStartPosition.X.Scale, WindowStartPosition.X.Offset + Delta.X, WindowStartPosition.Y.Scale, WindowStartPosition.Y.Offset + Delta.Y)
		end
	end)

	UserInputService.InputEnded:Connect(function(input)
		if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
			WindowDragging = false
		end
	end)

	Bubble.Name = "Bubble"
	Bubble.Size = UDim2.fromOffset(70, 70)
	Bubble.Position = UDim2.new(0, 30, 0.5, -35)
	Bubble.BackgroundColor3 = Color3.fromRGB(20, 20, 27)
	Bubble.BorderSizePixel = 0
	Bubble.Image = IMAGE_ID
	Bubble.AutoButtonColor = false
	Bubble.Visible = false
	Bubble.Active = true
	Bubble.ZIndex = 50
	Bubble.Parent = Gui
	Corner(Bubble, 35)
	Stroke(Bubble)

	local BubbleDragging = false
	local BubbleMoved = false
	local BubbleStartMouse, BubbleStartPosition

	Bubble.InputBegan:Connect(function(input)
		if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
			BubbleDragging = true
			BubbleMoved = false
			BubbleStartMouse = input.Position
			BubbleStartPosition = Bubble.Position
		end
	end)

	UserInputService.InputChanged:Connect(function(input)
		if not BubbleDragging then return end
		if input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch then
			local Delta = input.Position - BubbleStartMouse
			if Delta.Magnitude > 6 then BubbleMoved = true end
			if BubbleMoved then
				Bubble.Position = UDim2.new(BubbleStartPosition.X.Scale, BubbleStartPosition.X.Offset + Delta.X, BubbleStartPosition.Y.Scale, BubbleStartPosition.Y.Offset + Delta.Y)
			end
		end
	end)

	UserInputService.InputEnded:Connect(function(input)
		if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
			if BubbleDragging then
				if not BubbleMoved then
					if WindowOpen then CloseWindow() else OpenWindow() end
				end
				BubbleDragging = false
				BubbleMoved = false
			end
		end
	end)

	task.spawn(function()
		local StartTime = os.clock()
		local Duration = 3

		while os.clock() - StartTime < Duration do
			local ProgressValue = math.clamp((os.clock() - StartTime) / Duration, 0, 1)
			Progress.Size = UDim2.new(ProgressValue, 0, 1, 0)
			Percentage.Text = math.floor(ProgressValue * 100) .. "%"
			task.wait()
		end

		Progress.Size = UDim2.new(1, 0, 1, 0)
		Percentage.Text = "100%"
		LoadingText.Text = "Pronto!"
		task.wait(0.25)

		pcall(function()
			LoadingMusic:Stop()
			LoadingMusic:Destroy()
		end)

		local Fade = Tween(LoadingScreen, 0.35, {BackgroundTransparency = 1})
		Fade:Play()
		Fade.Completed:Wait()
		LoadingScreen:Destroy()
		if LoadingBlur then LoadingBlur:Destroy() end

		Bubble.Visible = true
		Main.Visible = true
		WindowOpen = true
		Main.Size = UDim2.fromOffset(0, 0)
		Tween(Main, 0.5, {Size = OriginalSize}, Enum.EasingStyle.Back, Enum.EasingDirection.Out):Play()
	end)
end

--==================================================
-- SISTEMA DE KEY (ATOMICVIPDS01 & PANDA AUTH)
--==================================================

local DiscordLink = "https://discord.com/invite/TEa7SFN7Ff"
local VIP_KEY = "ATOMICVIPDS01"

local function TryUnlockHub(keyToTest, guiToDestroy, btnToUpdate)
	if keyToTest == VIP_KEY then
		if btnToUpdate then
			btnToUpdate.Text = "Key VIP Aceita! Carregando..."
			btnToUpdate.BackgroundColor3 = Color3.fromRGB(46, 204, 113)
		end
		task.wait(1)
		pcall(function() guiToDestroy:Destroy() end)
		StartAtomicHub()
		return true
	end

	if AdminUserIds[Player.UserId] and keyToTest == "AtomicADM" then
		if btnToUpdate then
			btnToUpdate.Text = "ADM Reconhecido! Carregando..."
			btnToUpdate.BackgroundColor3 = Color3.fromRGB(46, 204, 113)
		end
		task.wait(1)
		pcall(function() guiToDestroy:Destroy() end)
		StartAtomicHub()
		return true
	end

	if PUSL and type(PUSL.validate) == "function" then
		local success, result = pcall(function()
			return PUSL.validate(keyToTest)
		end)
		if success and result and result.success then
			if btnToUpdate then
				btnToUpdate.Text = "Key Correta! Carregando..."
				btnToUpdate.BackgroundColor3 = Color3.fromRGB(46, 204, 113)
			end
			task.wait(1)
			pcall(function() guiToDestroy:Destroy() end)
			StartAtomicHub()
			return true
		end
	end

	return false
end

local autoLoaded = false
if AdminUserIds[Player.UserId] then
	autoLoaded = TryUnlockHub("AtomicADM", nil, nil)
end

if not autoLoaded then
	local KeyBlur = Instance.new("BlurEffect")
	KeyBlur.Size = 24
	KeyBlur.Parent = Lighting

	local KeyGui = Instance.new("ScreenGui")
	KeyGui.Name = "AtomicKeySystem"
	KeyGui.ResetOnSpawn = false
	KeyGui.IgnoreGuiInset = true
	KeyGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
	KeyGui.Parent = PlayerGui

	-- Fundo escuro translúcido atrás da Key
	local KeyBackground = Instance.new("Frame")
	KeyBackground.Size = UDim2.fromScale(1, 1)
	KeyBackground.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
	KeyBackground.BackgroundTransparency = 0.4
	KeyBackground.BorderSizePixel = 0
	KeyBackground.Parent = KeyGui

	local KeyFrame = Instance.new("Frame")
	KeyFrame.Size = UDim2.fromOffset(380, 260)
	KeyFrame.Position = UDim2.fromScale(0.5, 0.5)
	KeyFrame.AnchorPoint = Vector2.new(0.5, 0.5)
	KeyFrame.BackgroundColor3 = Color3.fromRGB(20, 20, 27)
	KeyFrame.BorderSizePixel = 0
	KeyFrame.Parent = KeyGui
	Corner(KeyFrame, 16)
	Stroke(KeyFrame)

	local KeyTitle = Instance.new("TextLabel")
	KeyTitle.Size = UDim2.new(1, -50, 0, 40)
	KeyTitle.Position = UDim2.fromOffset(15, 10)
	KeyTitle.BackgroundTransparency = 1
	KeyTitle.Text = "Atomic Hub - Key System"
	KeyTitle.TextColor3 = Color3.fromRGB(255, 255, 255)
	KeyTitle.TextSize = 18
	KeyTitle.TextXAlignment = Enum.TextXAlignment.Left
	KeyTitle.Font = Enum.Font.GothamBold
	KeyTitle.Parent = KeyFrame

	-- Botão X para fechar a janela da Key
	local KeyClose = Instance.new("TextButton")
	KeyClose.Size = UDim2.fromOffset(32, 32)
	KeyClose.Position = UDim2.new(1, -40, 0, 14)
	KeyClose.BackgroundColor3 = Color3.fromRGB(55, 25, 30)
	KeyClose.BorderSizePixel = 0
	KeyClose.Text = "×"
	KeyClose.TextColor3 = Color3.fromRGB(255, 255, 255)
	KeyClose.TextSize = 20
	KeyClose.Font = Enum.Font.GothamBold
	KeyClose.AutoButtonColor = false
	KeyClose.Parent = KeyFrame
	Corner(KeyClose, 8)

	KeyClose.MouseButton1Click:Connect(function()
		pcall(function() KeyBlur:Destroy() end)
		KeyGui:Destroy()
	end)

	local KeyBox = Instance.new("TextBox")
	KeyBox.Size = UDim2.new(1, -40, 0, 38)
	KeyBox.Position = UDim2.new(0, 20, 0, 55)
	KeyBox.BackgroundColor3 = Color3.fromRGB(15, 15, 20)
	KeyBox.BorderSizePixel = 0
	KeyBox.PlaceholderText = "Cole sua key aqui..."
	KeyBox.PlaceholderColor3 = Color3.fromRGB(135, 135, 140)
	KeyBox.Text = ""
	KeyBox.TextSize = 12
	KeyBox.TextColor3 = Color3.fromRGB(255, 255, 255)
	KeyBox.Font = Enum.Font.Gotham
	KeyBox.Parent = KeyFrame
	Corner(KeyBox, 8)
	Stroke(KeyBox, 0.5)

	if AdminUserIds[Player.UserId] then
		KeyBox.Text = "AtomicADM"
	end

	local CheckKeyBtn = Instance.new("TextButton")
	CheckKeyBtn.Size = UDim2.new(1, -40, 0, 36)
	CheckKeyBtn.Position = UDim2.new(0, 20, 0, 102)
	CheckKeyBtn.BackgroundColor3 = Color3.fromRGB(90, 135, 205)
	CheckKeyBtn.BorderSizePixel = 0
	CheckKeyBtn.Text = "Verificar Key"
	CheckKeyBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
	CheckKeyBtn.TextSize = 13
	CheckKeyBtn.Font = Enum.Font.GothamBold
	CheckKeyBtn.AutoButtonColor = false
	CheckKeyBtn.Parent = KeyFrame
	Corner(CheckKeyBtn, 8)

	local GetKeyBtn = Instance.new("TextButton")
	GetKeyBtn.Size = UDim2.new(0.5, -25, 0, 35)
	GetKeyBtn.Position = UDim2.new(0, 20, 0, 148)
	GetKeyBtn.BackgroundColor3 = Color3.fromRGB(46, 204, 113)
	GetKeyBtn.BorderSizePixel = 0
	GetKeyBtn.Text = "Pegar Key"
	GetKeyBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
	GetKeyBtn.TextSize = 12
	GetKeyBtn.Font = Enum.Font.GothamBold
	GetKeyBtn.Parent = KeyFrame
	Corner(GetKeyBtn, 8)

	local DiscordBtn = Instance.new("TextButton")
	DiscordBtn.Size = UDim2.new(0.5, -25, 0, 35)
	DiscordBtn.Position = UDim2.new(0.5, 5, 0, 148)
	DiscordBtn.BackgroundColor3 = Color3.fromRGB(88, 101, 242)
	DiscordBtn.BorderSizePixel = 0
	DiscordBtn.Text = "Discord"
	DiscordBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
	DiscordBtn.TextSize = 12
	DiscordBtn.Font = Enum.Font.GothamBold
	DiscordBtn.Parent = KeyFrame
	Corner(DiscordBtn, 8)

	DiscordBtn.MouseButton1Click:Connect(function()
		if setclipboard then setclipboard(DiscordLink) end
		DiscordBtn.Text = "Copiado!"
		task.wait(1.5)
		DiscordBtn.Text = "Discord"
	end)

	GetKeyBtn.MouseButton1Click:Connect(function()
		if PUSL and type(PUSL.getKeyUrl) == "function" then
			if setclipboard then setclipboard(PUSL.getKeyUrl()) end
			GetKeyBtn.Text = "Link Copiado!"
			task.wait(1.5)
			GetKeyBtn.Text = "Pegar Key"
		else
			if setclipboard then setclipboard("https://pandauth.com/") end
			GetKeyBtn.Text = "Link Copiado!"
			task.wait(1.5)
			GetKeyBtn.Text = "Pegar Key"
		end
	end)

	CheckKeyBtn.MouseButton1Click:Connect(function()
		local userKey = KeyBox.Text
		if userKey == "" then
			CheckKeyBtn.Text = "Digite uma key!"
			CheckKeyBtn.BackgroundColor3 = Color3.fromRGB(231, 76, 60)
			task.wait(1.5)
			CheckKeyBtn.Text = "Verificar Key"
			CheckKeyBtn.BackgroundColor3 = Color3.fromRGB(90, 135, 205)
			return
		end

		CheckKeyBtn.Text = "Verificando..."
		CheckKeyBtn.BackgroundColor3 = Color3.fromRGB(241, 196, 15)

		pcall(function() KeyBlur:Destroy() end)
		local successUnlocked = TryUnlockHub(userKey, KeyGui, CheckKeyBtn)

		if not successUnlocked then
			local newBlur = Instance.new("BlurEffect")
			newBlur.Size = 24
			newBlur.Parent = Lighting
			KeyBlur = newBlur

			CheckKeyBtn.Text = "Key Incorreta ou Expirada!"
			CheckKeyBtn.BackgroundColor3 = Color3.fromRGB(231, 76, 60)
			task.wait(1.5)
			CheckKeyBtn.Text = "Verificar Key"
			CheckKeyBtn.BackgroundColor3 = Color3.fromRGB(90, 135, 205)
		end
	end)
end

print("Atomic Hub Finalizado com Sucesso!")
