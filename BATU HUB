-- Servicios necesarios
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local LocalPlayer = Players.LocalPlayer

-- Configuración
local CONFIG = {
    ESP_Enabled = true,
    BoxColor = Color3.fromRGB(0, 255, 0), -- Verde
    Hitbox_Enabled = true,
    HitboxSize = Vector3.new(5, 5, 5), -- Tamaño grande para el Head o HumanoidRootPart
    HitboxTransparency = 0.5 -- Transparencia (0 = opaco, 1 = invisible)
}

-- Función para crear o actualizar el ESP de un jugador
local function crearESP(player)
    if player == LocalPlayer then return end

    -- Esperar a que el personaje cargue
    player.CharacterAdded:Connect(function(character)
        local rootPart = character:WaitForChild("HumanoidRootPart", 5)
        if not rootPart then return end

        -- Crear Highlight para el ESP visual
        local highlight = Instance.new("Highlight")
        highlight.Name = "CustomESP"
        highlight.Adornee = character
        highlight.FillColor = CONFIG.BoxColor
        highlight.FillTransparency = 0.7
        highlight.OutlineColor = CONFIG.BoxColor
        highlight.OutlineTransparency = 0
        highlight.Enabled = CONFIG.ESP_Enabled
        highlight.Parent = character
    end)

    -- Si ya tiene personaje cargado al unirse
    if player.Character then
        task.spawn(function()
            local character = player.Character
            local rootPart = character:WaitForChild("HumanoidRootPart", 5)
            if rootPart and not character:FindFirstChild("CustomESP") then
                local highlight = Instance.new("Highlight")
                highlight.Name = "CustomESP"
                highlight.Adornee = character
                highlight.FillColor = CONFIG.BoxColor
                highlight.FillTransparency = 0.7
                highlight.OutlineColor = CONFIG.BoxColor
                highlight.OutlineTransparency = 0
                highlight.Enabled = CONFIG.ESP_Enabled
                highlight.Parent = character
            end
        end)
    end
end

-- Aplicar ESP a jugadores actuales y futuros
for _, player in ipairs(Players:GetPlayers()) do
    crearESP(player)
end
Players.PlayerAdded:Connect(crearESP)

-- Bucle para mantener y aplicar el Hitbox Expander
RunService.RenderStepped:Connect(function()
    if not CONFIG.Hitbox_Enabled then return end

    for _, player in ipairs(Players:GetPlayers()) do
        if player ~= LocalPlayer and player.Character then
            local character = player.Character
            local humanoid = character:FindFirstChildOfClass("Humanoid")
            
            -- Verificar que esté vivo
            if humanoid and humanoid.Health > 0 then
                local rootPart = character:FindFirstChild("HumanoidRootPart") or character:FindFirstChild("Head")
                
                if rootPart then
                    -- Modificar tamaño y propiedades de la hitbox
                    rootPart.Size = CONFIG.HitboxSize
                    rootPart.Transparency = CONFIG.HitboxTransparency
                    rootPart.CanCollide = false -- Evita que empuje al jugador local
                end
            end
        end
    end
end)

print("Script de ESP y Hitbox cargado correctamente.")
