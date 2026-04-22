local Players = game:GetService("Players")
local TweenService = game:GetService("TweenService")
local UIS = game:GetService("UserInputService")
local LP = Players.LocalPlayer

local spamming = false
local selectedTarget = "الكل"

---
-- remote

local function getRemote()
    local gui = LP.PlayerGui:FindFirstChild("MountedGui")
    if gui then
        for _,v in pairs(gui:GetDescendants()) do
            if v.Name=="Remote" and v:IsA("RemoteEvent") then
                return v
            end
        end
    end
end

local function fireRemote(target,code)
    local r = getRemote()
    if not r then return end

    if target=="الكل" then    
        for _,p in pairs(Players:GetPlayers()) do    
            r:FireServer(p,code)    
        end    
    else    
        local p = Players:FindFirstChild(target)    
        if p then r:FireServer(p,code) end    
    end
end

---
-- GUI

pcall(function()
    game.CoreGui.ShahadSoundHub:Destroy()
end)

local gui = Instance.new("ScreenGui", game.CoreGui)
gui.Name = "ShahadSoundHub"

---
-- 🎧 زر عائم (تحريك سلس ناعم)

local toggle = Instance.new("TextButton", gui)
toggle.Size = UDim2.new(0, 55, 0, 55)
toggle.Position = UDim2.new(0.05, 0, 0.4, 0)
toggle.Text = "🎧"
toggle.Font = Enum.Font.GothamBold
toggle.TextSize = 22
toggle.BackgroundColor3 = Color3.fromRGB(140, 80, 255)
toggle.TextColor3 = Color3.new(1, 1, 1)
Instance.new("UICorner", toggle).CornerRadius = UDim.new(1, 0)

local dragToggle = nil
local dragStart = nil
local startPos = nil

local function update(input)
    local delta = input.Position - dragStart
    local targetPos = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)
    TweenService:Create(toggle, TweenInfo.new(0.15, Enum.EasingStyle.Quad, Enum.EasingDirection.Out), {Position = targetPos}):Play()
end

toggle.InputBegan:Connect(function(input)
    if (input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch) then
        dragToggle = true
        dragStart = input.Position
        startPos = toggle.Position
        
        input.Changed:Connect(function()
            if input.UserInputState == Enum.UserInputState.End then
                dragToggle = false
            end
        end)
    end
end)

UIS.InputChanged:Connect(function(input)
    if dragToggle and (input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch) then
        update(input)
    end
end)

---
-- 🟣 اللوحة

local main = Instance.new("Frame", gui)
main.Size = UDim2.new(0, 420, 0, 450)
main.Position = UDim2.new(0.5, -210, 0.1, 0)
main.BackgroundColor3 = Color3.fromRGB(20, 20, 30)
main.BackgroundTransparency = 0.25
main.Active = true
main.Draggable = true
Instance.new("UICorner", main).CornerRadius = UDim.new(0, 16)

local stroke = Instance.new("UIStroke", main)
stroke.Color = Color3.fromRGB(180, 90, 255)
stroke.Thickness = 2

local header = Instance.new("Frame", main)
header.Size = UDim2.new(1, 0, 0, 45)
header.BackgroundColor3 = Color3.fromRGB(60, 30, 90)
header.BackgroundTransparency = 0.3
Instance.new("UICorner", header).CornerRadius = UDim.new(0, 16)

local title = Instance.new("TextLabel", header)
title.BackgroundTransparency = 1
title.Size = UDim2.new(1, -10, 1, 0)
title.Position = UDim2.new(0, 10, 0, 0)
title.Text = "Shahad Sound Hub 🎵"
title.Font = Enum.Font.GothamBold
title.TextSize = 16
title.TextColor3 = Color3.new(1, 1, 1)
title.TextXAlignment = Enum.TextXAlignment.Left

local content = Instance.new("Frame", main)
content.Position = UDim2.new(0, 0, 0, 45)
content.Size = UDim2.new(1, 0, 0, 405)
content.BackgroundTransparency = 1

---
-- open/close

local open = true

toggle.MouseButton1Click:Connect(function()
    open = not open
    main.Visible = open
end)

---
-- style

local function style(btn, col)
    btn.BackgroundColor3 = col
    btn.TextColor3 = Color3.new(1, 1, 1)
    btn.Font = Enum.Font.GothamBold
    btn.TextSize = 12
    Instance.new("UICorner", btn).CornerRadius = UDim.new(0, 10)
end

---
-- players

local playerList = Instance.new("ScrollingFrame", content)
playerList.Size = UDim2.new(0, 120, 0, 330)
playerList.Position = UDim2.new(0, 10, 0, 10)
playerList.BackgroundColor3 = Color3.fromRGB(20, 20, 30)
playerList.ScrollBarThickness = 3
Instance.new("UICorner", playerList)

local layout = Instance.new("UIListLayout", playerList)
layout.Padding = UDim.new(0, 4)

---
-- side

local side = Instance.new("Frame", content)
side.Position = UDim2.new(0, 140, 0, 10)
side.Size = UDim2.new(0, 260, 0, 330)
side.BackgroundTransparency = 1

local label = Instance.new("TextLabel", side)
label.Size = UDim2.new(1, 0, 0, 28)
label.Text = "المستهدف: الكل"
label.BackgroundColor3 = Color3.fromRGB(35, 35, 50)
label.TextColor3 = Color3.new(1, 1, 1)
label.Font = Enum.Font.GothamBold
label.TextSize = 12
Instance.new("UICorner", label)

local box = Instance.new("TextBox", side)
box.Size = UDim2.new(1, 0, 0, 32)
box.Position = UDim2.new(0, 0, 0, 35)
box.Text = "140217738705613"
box.BackgroundColor3 = Color3.fromRGB(30, 30, 40)
box.TextColor3 = Color3.new(1, 1, 1)
Instance.new("UICorner", box)

local play = Instance.new("TextButton", side)
play.Size = UDim2.new(1, 0, 0, 30)
play.Position = UDim2.new(0, 0, 0, 75)
play.Text = "تشغيل ▶"
style(play, Color3.fromRGB(0, 170, 100))

local spamBtn = Instance.new("TextButton", side)
spamBtn.Size = UDim2.new(1, 0, 0, 30)
spamBtn.Position = UDim2.new(0, 0, 0, 110)
spamBtn.Text = "سبام 🚀"
style(spamBtn, Color3.fromRGB(80, 80, 90))

---
-- 📝 نص "مكتبة شهد :"

local libraryTitle = Instance.new("TextLabel", side)
libraryTitle.Size = UDim2.new(1, 0, 0, 20)
libraryTitle.Position = UDim2.new(0, 5, 0, 145)
libraryTitle.BackgroundTransparency = 1
libraryTitle.Text = "مكتبة شهد :"
libraryTitle.TextColor3 = Color3.fromRGB(180, 90, 255)
libraryTitle.Font = Enum.Font.GothamBold
libraryTitle.TextSize = 14
libraryTitle.TextXAlignment = Enum.TextXAlignment.Left

---
-- songs

local songsFrame = Instance.new("ScrollingFrame", side)
songsFrame.Size = UDim2.new(1, 0, 0, 160) -- تم تصغير الارتفاع قليلاً لترك مساحة للعنوان
songsFrame.Position = UDim2.new(0, 0, 0, 170)
songsFrame.BackgroundColor3 = Color3.fromRGB(25, 25, 35)
songsFrame.ScrollBarThickness = 3
Instance.new("UICorner", songsFrame)

local songs = {
    ["kill me 💀"] = "136373350899288",
    ["cant steal 💎"] = "91214368916078",
    ["هعهعهع 😂"] = "140669499902711",
    ["رعب 👻"] = "133594398909422",
    ["omg hack 💻"] = "121670695729530",
    ["طننننن 🔊"] = "17070340316",
    ["ازعاج 📢"] = "7713890963",
    ["اندلس 🏛️"] = "132039307762001",
    ["كداوت 💥"] = "139208046340684",
    ["شاورما 🌯"] = "80487723481385",
    ["امبيه 😮"] = "7657178494"
}

local sl = Instance.new("UIListLayout", songsFrame)
sl.Padding = UDim.new(0, 4)

for n, id in pairs(songs) do
    local b = Instance.new("TextButton", songsFrame)
    b.Size = UDim2.new(1, -6, 0, 26)
    b.Text = n
    style(b, Color3.fromRGB(60, 60, 85))

    b.MouseButton1Click:Connect(function()    
        box.Text = id    
        fireRemote(selectedTarget, id)    
    end)
end

---
-- actions

play.MouseButton1Click:Connect(function()
    fireRemote(selectedTarget, box.Text)
end)

spamBtn.MouseButton1Click:Connect(function()
    spamming = not spamming
    spamBtn.Text = spamming and "إيقاف 🛑" or "سبام 🚀"

    task.spawn(function()    
        while spamming do    
            fireRemote(selectedTarget, box.Text)    
            task.wait(0.5)    
        end    
    end)
end)

---
-- players refresh

local function refresh()
    for _, v in pairs(playerList:GetChildren()) do
        if v:IsA("TextButton") then v:Destroy() end
    end

    local all = Instance.new("TextButton", playerList)    
    all.Size = UDim2.new(1, -6, 0, 26)    
    all.Text = "الكل"    
    style(all, Color3.fromRGB(120, 50, 220))    

    all.MouseButton1Click:Connect(function()    
        selectedTarget = "الكل"    
        label.Text = "المستهدف: الكل"    
    end)    

    for _, p in pairs(Players:GetPlayers()) do    
        local b = Instance.new("TextButton", playerList)    
        b.Size = UDim2.new(1, -6, 0, 26)    
        b.Text = p.Name    
        style(b, Color3.fromRGB(45, 45, 60))    

        b.MouseButton1Click:Connect(function()    
            selectedTarget = p.Name    
            label.Text = "المستهدف: "..p.Name    
        end)    
    end
end

refresh()
Players.PlayerAdded:Connect(refresh)
Players.PlayerRemoving:Connect(refresh)
