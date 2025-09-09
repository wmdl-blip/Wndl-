local redzlib = loadstring(game:HttpGet("https://raw.githubusercontent.com/tbao143/Library-ui/refs/heads/main/Redzhubui"))()
local Window = redzlib:MakeWindow({
  Title = "kakah Hub | Brookhaven 1.0.0🏡",
  SubTitle = "by kakah",
  SaveFolder = "testando | kakah 1v"
})

Window:AddMinimizeButton({
    Button = { Image = "rbxassetid://87585721979738", BackgroundTransparency = 0 },
    Corner = { CornerRadius = UDim.new(35, 1) },
})

local Tab1 = Window:MakeTab({"Credits", "info"})

Tab1:AddDiscordInvite({
    Name = "kakah hub",
    Description = "Join server",
    Logo = "rbxassetid://18751483361",
    Invite = "https://discord.gg/tsxKumHC",
})

local Section = Tab1:AddSection({"Credits"})

local Paragraph = Tab1:AddParagraph({"Criador", "kakah"})

local Paragraph = Tab1:AddParagraph({"Versão", "1.0.1v"})

local Paragraph = Tab1:AddParagraph({"Executor", "Delta"})

local Paragraph = Tab1:AddParagraph({"total players", "20/30"})

local Tab2 = Window:MakeTab({"Fun", "fun"})

local Section = Tab2:AddSection({"Nenhum evento..."})

local Tab3 = Window:MakeTab({"Main", "Home"})

Tab3:AddButton({"Painel Adm", function(Value)
local Version = "1.6.41"
local WindUI = loadstring(game:HttpGet("https://github.com/Footagesus/WindUI/releases/download/" .. Version .. "/main.lua"))()
local Window = WindUI:CreateWindow({
    Title = "Admin Incomun",
    Icon = "shield",
    Author = "By kakah",
    Folder = "kakah hub",
    Size = UDim2.fromOffset(330, 220),
    Transparent = true,
    Theme = "Red",
    Resizable = true,
    SideBarWidth = 200,
    Background = "87585721979738", -- rbxassetid or video 
    BackgroundImageTransparency = 0.10,
    HideSearchBar = true,
    ScrollBarEnabled = false,
    User = {
        Enabled = true,
        Xxxhey_lorenzo1 = true,
        Callback = function()
            print("clicked")
        end,
    },
    KeySystem = {
        Key = { "kakah", "PainelAdm" },
        Note = "⚒️Painel Key Staff⚒️",
        Thumbnail = {
            Image = "rbxassetid://87585721979738",
            Title = "Key system",
        },
        URL = "https://github.com/Footagesus/WindUI",
        SaveKey = true,
    },
})

Window:EditOpenButton({
    Title = "kakah Adm",
    Icon = "shield",
    CornerRadius = UDim.new(0,16),
    StrokeThickness = 2,
    Color = ColorSequence.new(
        Color3.fromHex("FF0000"), 
        Color3.fromHex("FFFFFF")
    ),
    OnlyMobile = false,
    Enabled = true,
    Draggable = true,
})

local Tab = Window:Tab({
    Title = "Protetion",
    Icon = "skull",
    Locked = false,
})

local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local StarterGui = game:GetService("StarterGui")
local HttpService = game:GetService("HttpService")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local TextChatService = game:GetService("TextChatService")

-- Gera lista com todos os nomes dos jogadores
local playerNames = {}
for _, plr in ipairs(Players:GetPlayers()) do
    table.insert(playerNames, plr.Name)
end

-- Variável para armazenar os jogadores selecionados no Dropdown
local selectedPlayers = {}

local Dropdown = Tab:Dropdown({
    Title = "Selecionar Jogadores",
    Values = playerNames,
    Value = {},
    Multi = true,
    AllowNone = true,
    Callback = function(newSelectedPlayers)
        selectedPlayers = newSelectedPlayers
        print("Jogadores selecionados: " .. HttpService:JSONEncode(selectedPlayers))
    end
})

-- Atualiza lista automaticamente quando entra ou sai jogador
Players.PlayerAdded:Connect(function(plr)
    table.insert(playerNames, plr.Name)
    Dropdown:SetValues(playerNames)
end)

Players.PlayerRemoving:Connect(function(plr)
    for i, name in ipairs(playerNames) do
        if name == plr.Name then
            table.remove(playerNames, i)
            break
        end
    end
    Dropdown:SetValues(playerNames)
end)

-- Adiciona o Toggle para View Player na aba Protetion
local ViewToggle = Tab:Toggle({
    Title = "Visualizar Jogador",
    Icon = "eye", -- Ícone de olho para representar visualização
    Default = false, -- Estado inicial: desativado
    Callback = function(state)
        if state then
            -- Verifica se há jogadores selecionados no Dropdown
            if #selectedPlayers > 0 then
                local targetPlayerName = selectedPlayers[1] -- Pega o primeiro jogador selecionado
                local targetPlayer = Players:FindFirstChild(targetPlayerName)
                if targetPlayer and targetPlayer.Character and targetPlayer.Character:FindFirstChild("HumanoidRootPart") then
                    local camera = workspace.CurrentCamera
                    camera.CameraSubject = targetPlayer.Character.Humanoid
                    camera.CameraType = Enum.CameraType.Follow
                    StarterGui:SetCore("SendNotification", {
                        Title = "Visualizando",
                        Text = "Câmera focada em " .. targetPlayer.Name,
                        Duration = 3
                    })
                else
                    -- Desativar o toggle se o jogador não for encontrado
                    ViewToggle:Set(false)
                    StarterGui:SetCore("SendNotification", {
                        Title = "Erro",
                        Text = "kakah " .. targetPlayerName .. " não encontrado ou sem personagem!",
                        Duration = 3
                    })
                end
            else
                -- Desativar o toggle se nenhum jogador estiver selecionado
                ViewToggle:Set(false)
                StarterGui:SetCore("SendNotification", {
                    Title = "Erro",
                    Text = "Nenhum jogador selecionado no Dropdown!",
                    Duration = 3
                })
            end
        else
            -- Restaurar a câmera padrão
            local camera = workspace.CurrentCamera
            camera.CameraSubject = LocalPlayer.Character and LocalPlayer.Character.Humanoid
            camera.CameraType = Enum.CameraType.Custom
            StarterGui:SetCore("SendNotification", {
                Title = "Visualização Desativada",
                Text = "Câmera restaurada para o seu personagem.",
                Duration = 3
            })
        end
    end
})

-- Função para enviar mensagem no chat
local function sendChatMessage(message)
    -- Tenta usar o TextChatService (novo sistema de chat)
    if TextChatService.ChatVersion == Enum.ChatVersion.TextChatService then
        local textChannel = TextChatService.TextChannels.RBXGeneral
        if textChannel then
            textChannel:SendAsync(message)
            return true
        else
            return false
        end
    else
        -- Fallback para o sistema de chat legado
        local success, err = pcall(function()
            ReplicatedStorage.DefaultChatSystemChatEvents.SayMessageRequest:FireServer(message, "All")
        end)
        return success
    end
end

-- Adiciona o Botão Verifique na aba Protetion
local VerifyButton = Tab:Button({
    Title = "Verifique",
    Icon = "check", -- Ícone de check para representar verificação
    Callback = function()
        -- Enviar "/Verify" no chat
        local success1 = sendChatMessage("/Verify")
        -- Pequeno atraso para evitar problemas com o sistema de chat
        wait(0.5)
        -- Enviar "Coquette Hub" no chat
        local success2 = sendChatMessage("Coquette Hub")
        
        -- Notificação de confirmação ou erro
        if success1 and success2 then
            StarterGui:SetCore("SendNotification", {
                Title = "Verificação",
                Text = "Comandos '/Verify' e 'kakah Hub' enviados no chat!",
                Duration = 3
            })
        else
            StarterGui:SetCore("SendNotification", {
                Title = "Erro",
                Text = "Falha ao enviar uma ou ambas as mensagens no chat!",
                Duration = 3
            })
        end
    end
})

-- Aviso de uso responsável
print("Toggle de Visualização de Jogador e Botão Verifique (com TextChatService) adicionados à WindUI para Brookhaven RP! Use com responsabilidade.")
print("Painel adm adicionado!")
end})

local Section = Tab3:AddSection({"Chat Spy"})

Tab3:AddButton({"Chat Spy", function(Value)
local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local Character = LocalPlayer.Character or LocalPlayer.CharacterAdded:Wait()

-- Espera o objeto Estilo dentro do personagem
local estiloValue = Character:WaitForChild("Estilo", 5) -- espera até 5 segundos

if estiloValue and estiloValue:IsA("StringValue") then
    local estilo = estiloValue.Value
    print("Estilo detectado:", estilo)
    
    -- Se estilo for o esperado, teleportar
    if estilo == "MandrakeStyle" then -- mude para o nome do estilo que deseja detectar
        local mandrake = workspace:FindFirstChild("Mandrake")
        if mandrake and mandrake:IsA("BasePart") then
            local hrp = Character:WaitForChild("HumanoidRootPart")
            hrp.CFrame = mandrake.CFrame + Vector3.new(0, 5, 0) -- teleporta 5 studs acima
            print("Teleportado para Mandrake!")
        else
            warn("Mandrake não encontrado no workspace")
        end
    else
        print("Estilo diferente, não vai teleportar")
    end
else
    warn("Objeto 'Estilo' não encontrado no personagem")
end
print("chat a pasmado!")
end})

local Section = Tab3:AddSection({"Domain Expansion"})

local TrollTab = Window:MakeTab({ Title = "Scripts Trolls", Icon = "rbxassetid://87585721979738" })


TrollTab:AddSection({ "Black Hole" })
TrollTab:AddButton({
    Name = "Black Hole",
    Description = " Ativando isso você puxa Parts até o seu personagem",
    Callback = function()
        local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local LocalPlayer = Players.LocalPlayer
local Workspace = game:GetService("Workspace")

local angle = 1
local radius = 10
local blackHoleActive = false

local function setupPlayer()
    local character = LocalPlayer.Character or LocalPlayer.CharacterAdded:Wait()
    local humanoidRootPart = character:WaitForChild("HumanoidRootPart")

    local Folder = Instance.new("Folder", Workspace)
    local Part = Instance.new("Part", Folder)
    local Attachment1 = Instance.new("Attachment", Part)
    Part.Anchored = true
    Part.CanCollide = false
    Part.Transparency = 1

    return humanoidRootPart, Attachment1
end

local humanoidRootPart, Attachment1 = setupPlayer()

if not getgenv().Network then
    getgenv().Network = {
        BaseParts = {},
        Velocity = Vector3.new(14.46262424, 14.46262424, 14.46262424)
    }

    Network.RetainPart = function(part)
        if typeof(part) == "Instance" and part:IsA("BasePart") and part:IsDescendantOf(Workspace) then
            table.insert(Network.BaseParts, part)
            part.CustomPhysicalProperties = PhysicalProperties.new(0, 0, 0, 0, 0)
            part.CanCollide = false
        end
    end

    local function EnablePartControl()
        LocalPlayer.ReplicationFocus = Workspace
        RunService.Heartbeat:Connect(function()
            sethiddenproperty(LocalPlayer, "SimulationRadius", math.huge)
            for _, part in pairs(Network.BaseParts) do
                if part:IsDescendantOf(Workspace) then
                    part.Velocity = Network.Velocity
                end
            end
        end)
    end

    EnablePartControl()
end

local function ForcePart(v)
    if v:IsA("Part") and not v.Anchored and not v.Parent:FindFirstChild("Humanoid") and not v.Parent:FindFirstChild("Head") and v.Name ~= "Handle" then
        for _, x in next, v:GetChildren() do
            if x:IsA("BodyAngularVelocity") or x:IsA("BodyForce") or x:IsA("BodyGyro") or x:IsA("BodyPosition") or x:IsA("BodyThrust") or x:IsA("BodyVelocity") or x:IsA("RocketPropulsion") then
                x:Destroy()
            end
        end
        if v:FindFirstChild("Attachment") then
            v:FindFirstChild("Attachment"):Destroy()
        end
        if v:FindFirstChild("AlignPosition") then
            v:FindFirstChild("AlignPosition"):Destroy()
        end
        if v:FindFirstChild("Torque") then
            v:FindFirstChild("Torque"):Destroy()
        end
        v.CanCollide = false
        
        local Torque = Instance.new("Torque", v)
        Torque.Torque = Vector3.new(1000000, 1000000, 1000000)
        local AlignPosition = Instance.new("AlignPosition", v)
        local Attachment2 = Instance.new("Attachment", v)
        Torque.Attachment0 = Attachment2
        AlignPosition.MaxForce = math.huge
        AlignPosition.MaxVelocity = math.huge
        AlignPosition.Responsiveness = 500
        AlignPosition.Attachment0 = Attachment2
        AlignPosition.Attachment1 = Attachment1
    end
end

local function toggleBlackHole()
    blackHoleActive = not blackHoleActive
    if blackHoleActive then
        for _, v in next, Workspace:GetDescendants() do
            ForcePart(v)
        end

        Workspace.DescendantAdded:Connect(function(v)
            if blackHoleActive then
                ForcePart(v)
            end
        end)

        spawn(function()
            while blackHoleActive and RunService.RenderStepped:Wait() do
                angle = angle + math.rad(2)

                local offsetX = math.cos(angle) * radius
                local offsetZ = math.sin(angle) * radius

                Attachment1.WorldCFrame = humanoidRootPart.CFrame * CFrame.new(offsetX, 0, offsetZ)
            end
        end)
    else
        Attachment1.WorldCFrame = CFrame.new(0, -1000, 0)
    end
end

LocalPlayer.CharacterAdded:Connect(function()
    humanoidRootPart, Attachment1 = setupPlayer()
    if blackHoleActive then
        toggleBlackHole()
    end
end)

local library = loadstring(game:HttpGet("https://raw.githubusercontent.com/miroeramaa/TurtleLib/main/TurtleUiLib.lua"))()
local window = library:Window("Projeto LKB")

window:Slider("Radius Blackhole",1,100,10, function(Value)
   radius = Value
end)

window:Toggle("Blackhole", true, function(Value)
       if Value then
            toggleBlackHole()
        else
            blackHoleActive = false
        end
end)

spawn(function()
    while true do
        RunService.RenderStepped:Wait()
        if blackHoleActive then
            angle = angle + math.rad(angleSpeed)
        end
    end
end)

toggleBlackHole()
    end
})


TrollTab:AddSection({ "Puxar Parts" })
TrollTab:AddButton({
    Name = "Puxar Parts",
    Description = "Para usar, chegue perto do Player Selecionado",
    Callback = function()
        -- Gui to Lua
-- Version: 1.0

-- Instances:

local Gui = Instance.new("ScreenGui")
local Main = Instance.new("Frame")
local Box = Instance.new("TextBox")
local UITextSizeConstraint = Instance.new("UITextSizeConstraint")
local Label = Instance.new("TextLabel")
local UITextSizeConstraint_2 = Instance.new("UITextSizeConstraint")
local Button = Instance.new("TextButton")
local UITextSizeConstraint_3 = Instance.new("UITextSizeConstraint")

--Properties:

Gui.Name = "Gui"
Gui.Parent = gethui()
Gui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling

Main.Name = "Main"
Main.Parent = Gui
Main.BackgroundColor3 = Color3.fromRGB(75, 75, 75)
Main.BorderColor3 = Color3.fromRGB(0, 0, 0)
Main.BorderSizePixel = 0
Main.Position = UDim2.new(0.335954279, 0, 0.542361975, 0)
Main.Size = UDim2.new(0.240350261, 0, 0.166880623, 0)
Main.Active = true
Main.Draggable = true

Box.Name = "Box"
Box.Parent = Main
Box.BackgroundColor3 = Color3.fromRGB(95, 95, 95)
Box.BorderColor3 = Color3.fromRGB(0, 0, 0)
Box.BorderSizePixel = 0
Box.Position = UDim2.new(0.0980926454, 0, 0.218712583, 0)
Box.Size = UDim2.new(0.801089942, 0, 0.364963502, 0)
Box.FontFace = Font.new("rbxasset://fonts/families/SourceSansSemibold.json", Enum.FontWeight.Bold, Enum.FontStyle.Normal)
Box.PlaceholderText = "Player here"
Box.Text = ""
Box.TextColor3 = Color3.fromRGB(255, 255, 255)
Box.TextScaled = true
Box.TextSize = 31.000
Box.TextWrapped = true

UITextSizeConstraint.Parent = Box
UITextSizeConstraint.MaxTextSize = 31

Label.Name = "Label"
Label.Parent = Main
Label.BackgroundColor3 = Color3.fromRGB(95, 95, 95)
Label.BorderColor3 = Color3.fromRGB(0, 0, 0)
Label.BorderSizePixel = 0
Label.Size = UDim2.new(1, 0, 0.160583943, 0)
Label.FontFace = Font.new("rbxasset://fonts/families/Nunito.json", Enum.FontWeight.Bold, Enum.FontStyle.Normal)
Label.Text = "Bring Parts | Made by: Lusquinha_067"
Label.TextColor3 = Color3.fromRGB(255, 255, 255)
Label.TextScaled = true
Label.TextSize = 14.000
Label.TextWrapped = true

UITextSizeConstraint_2.Parent = Label
UITextSizeConstraint_2.MaxTextSize = 21

Button.Name = "Button"
Button.Parent = Main
Button.BackgroundColor3 = Color3.fromRGB(95, 95, 95)
Button.BorderColor3 = Color3.fromRGB(0, 0, 0)
Button.BorderSizePixel = 0
Button.Position = UDim2.new(0.183284417, 0, 0.656760991, 0)
Button.Size = UDim2.new(0.629427791, 0, 0.277372271, 0)
Button.Font = Enum.Font.Nunito
Button.Text = "Bring | Off"
Button.TextColor3 = Color3.fromRGB(255, 255, 255)
Button.TextScaled = true
Button.TextSize = 28.000
Button.TextWrapped = true

UITextSizeConstraint_3.Parent = Button
UITextSizeConstraint_3.MaxTextSize = 28

-- Scripts:

local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local LocalPlayer = Players.LocalPlayer
local UserInputService = game:GetService("UserInputService")
local Workspace = game:GetService("Workspace")

local character
local humanoidRootPart

mainStatus = true
UserInputService.InputBegan:Connect(function(input, gameProcessedEvent)
	if input.KeyCode == Enum.KeyCode.RightControl and not gameProcessedEvent then
		mainStatus = not mainStatus
		Main.Visible = mainStatus
	end
end)

local Folder = Instance.new("Folder", Workspace)
local Part = Instance.new("Part", Folder)
local Attachment1 = Instance.new("Attachment", Part)
Part.Anchored = true
Part.CanCollide = false
Part.Transparency = 1

if not getgenv().Network then
	getgenv().Network = {
		BaseParts = {},
		Velocity = Vector3.new(14.46262424, 14.46262424, 14.46262424)
	}

	Network.RetainPart = function(Part)
		if Part:IsA("BasePart") and Part:IsDescendantOf(Workspace) then
			table.insert(Network.BaseParts, Part)
			Part.CustomPhysicalProperties = PhysicalProperties.new(0, 0, 0, 0, 0)
			Part.CanCollide = false
		end
	end

	local function EnablePartControl()
		LocalPlayer.ReplicationFocus = Workspace
		RunService.Heartbeat:Connect(function()
			sethiddenproperty(LocalPlayer, "SimulationRadius", math.huge)
			for _, Part in pairs(Network.BaseParts) do
				if Part:IsDescendantOf(Workspace) then
					Part.Velocity = Network.Velocity
				end
			end
		end)
	end

	EnablePartControl()
end

local function ForcePart(v)
	if v:IsA("BasePart") and not v.Anchored and not v.Parent:FindFirstChildOfClass("Humanoid") and not v.Parent:FindFirstChild("Head") and v.Name ~= "Handle" then
		for _, x in ipairs(v:GetChildren()) do
			if x:IsA("BodyMover") or x:IsA("RocketPropulsion") then
				x:Destroy()
			end
		end
		if v:FindFirstChild("Attachment") then
			v:FindFirstChild("Attachment"):Destroy()
		end
		if v:FindFirstChild("AlignPosition") then
			v:FindFirstChild("AlignPosition"):Destroy()
		end
		if v:FindFirstChild("Torque") then
			v:FindFirstChild("Torque"):Destroy()
		end
		v.CanCollide = false
		local Torque = Instance.new("Torque", v)
		Torque.Torque = Vector3.new(100000, 100000, 100000)
		local AlignPosition = Instance.new("AlignPosition", v)
		local Attachment2 = Instance.new("Attachment", v)
		Torque.Attachment0 = Attachment2
		AlignPosition.MaxForce = math.huge
		AlignPosition.MaxVelocity = math.huge
		AlignPosition.Responsiveness = 200
		AlignPosition.Attachment0 = Attachment2
		AlignPosition.Attachment1 = Attachment1
	end
end

local blackHoleActive = false
local DescendantAddedConnection

local function toggleBlackHole()
	blackHoleActive = not blackHoleActive
	if blackHoleActi
