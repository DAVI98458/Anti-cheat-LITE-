loadstring(game:HttpGet("https://raw.githubusercontent.com/xHeptc/Kavo-UI-Library/main/source.lua"))()

-- UI Library - Criado por @coroadinho_37203(beta)
local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local RunService = game:GetService("RunService")

-- UI Library BÁSICA
local function createButton(text, parent, position, onClick)
    local button = Instance.new("TextButton")
    button.Text = text
    button.Size = UDim2.new(0, 180, 0, 30)
    button.Position = position
    button.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
    button.TextColor3 = Color3.fromRGB(255, 255, 255)
    button.Font = Enum.Font.SourceSansBold
    button.TextSize = 16
    button.Parent = parent
    button.MouseButton1Click:Connect(onClick)
    return button
end

local ScreenGui = Instance.new("ScreenGui", LocalPlayer:WaitForChild("PlayerGui"))
ScreenGui.Name = "UILibrary"
ScreenGui.ResetOnSpawn = false

local Frame = Instance.new("Frame", ScreenGui)
Frame.Size = UDim2.new(0, 220, 0, 200)
Frame.Position = UDim2.new(0.5, -110, 0.4, 0)
Frame.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
Frame.BorderSizePixel = 0
Frame.Active = true
Frame.Draggable = true

local Title = Instance.new("TextLabel", Frame)
Title.Size = UDim2.new(1, 0, 0, 30)
Title.Text = "Criado por @coroadinho_37203(beta)"
Title.TextColor3 = Color3.new(1, 1, 1)
Title.BackgroundTransparency = 1
Title.Font = Enum.Font.SourceSansBold
Title.TextSize = 16

-- Anti-Cheat Lite
local function activateAntiCheat()
    local msg = Instance.new("Message", workspace)
    msg.Text = "Anti-Cheat ligado"
    task.delay(10, function() msg:Destroy() end)

    for _, v in pairs(getgc(true)) do
        if typeof(v) == "function" and getfenv(v).script and tostring(getfenv(v).script):lower():find("anticheat") then
            pcall(function()
                hookfunction(v, function(...) return end)
            end)
        end
    end
end

-- ESP
local espEnabled = false
local function toggleESP()
    espEnabled = not espEnabled
    for _, v in pairs(workspace:GetChildren()) do
        if v:IsA("Model") and v:FindFirstChild("Humanoid") and v ~= LocalPlayer.Character then
            local head = v:FindFirstChild("Head")
            if head then
                local existing = v:FindFirstChild("ESP")
                if espEnabled and not existing then
                    local tag = Instance.new("BillboardGui")
                    tag.Name = "ESP"
                    tag.Size = UDim2.new(0, 100, 0, 40)
                    tag.AlwaysOnTop = true
                    tag.Adornee = head
                    tag.Parent = v

                    local text = Instance.new("TextLabel", tag)
                    text.Size = UDim2.new(1, 0, 1, 0)
                    text.Text = v.Name
                    text.TextColor3 = v.Team == LocalPlayer.Team and Color3.new(0,0,1) or Color3.new(1,0,0)
                    text.BackgroundTransparency = 1
                    text.TextScaled = true
                elseif not espEnabled and existing then
                    existing:Destroy()
                end
            end
        end
    end
end

-- Botões
createButton("Ativar Anti-Cheat Lite", Frame, UDim2.new(0, 20, 0, 40), activateAntiCheat)
createButton("Toggle ESP", Frame, UDim2.new(0, 20, 0, 80), toggleESP)
createButton("Fechar Menu", Frame, UDim2.new(0, 20, 0, 140), function()
    Frame.Visible = false
    OpenBtn.Visible = true
end)

-- Abrir Menu
local OpenBtn = Instance.new("TextButton", ScreenGui)
OpenBtn.Size = UDim2.new(0, 120, 0, 30)
OpenBtn.Position = UDim2.new(0, 10, 0.9, -40)
OpenBtn.Text = "Abrir Menu"
OpenBtn.BackgroundColor3 = Color3.fromRGB(0, 80, 0)
OpenBtn.TextColor3 = Color3.new(1,1,1)
OpenBtn.Font = Enum.Font.SourceSansBold
OpenBtn.TextSize = 16
OpenBtn.Visible = false
OpenBtn.MouseButton1Click:Connect(function()
    Frame.Visible = true
    OpenBtn.Visible = false
end)
