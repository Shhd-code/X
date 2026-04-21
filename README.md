--[[ 
  Shahad Sound Hub - Radio Controller
  تم إضافة الأكواد الجديدة: ازعاج، اندلس، كداوت، شاورما، امبيه.
]]

local Players = game:GetService("Players")
local localPlayer = Players.LocalPlayer
local spamming = false

-- وظيفة البحث الديناميكي عن الراديو
local function getRemote()
    local gui = localPlayer.PlayerGui:FindFirstChild("MountedGui")
    if gui then
        for _, v in pairs(gui:GetDescendants()) do
            if v.Name == "Remote" and v:IsA("RemoteEvent") then return v end
        end
    end
    return nil
end

-- إنشاء الواجهة
local sg = Instance.new("ScreenGui", game:GetService("CoreGui"))
local mainFrame = Instance.new("Frame", sg)
mainFrame.Size = UDim2.new(0, 450, 0, 40)
mainFrame.Position = UDim2.new(0.5, -225, 0.1, 0)
mainFrame.BackgroundColor3 = Color3.fromRGB(15, 15, 15); mainFrame.Active = true; mainFrame.Draggable = true
mainFrame.ClipsDescendants = true; Instance.new("UICorner", mainFrame)

-- الهيدر
local header = Instance.new("Frame", mainFrame)
header.Size = UDim2.new(1, 0, 0, 40); header.BackgroundColor3 = Color3.fromRGB(138, 43, 226)
Instance.new("UICorner", header)

local title = Instance.new("TextLabel", header)
title.Size = UDim2.new(1, -80, 1, 0); title.Position = UDim2.new(0, 10, 0, 0)
title.Text = "Shahad Sound Hub 🎵"; title.TextColor3 = Color3.new(1, 1, 1)
title.BackgroundTransparency = 1; title.Font = Enum.Font.FredokaOne; title.TextSize = 18

local minBtn = Instance.new("TextButton", header)
minBtn.Size = UDim2.new(0, 30, 0, 30); minBtn.Position = UDim2.new(1, -70, 0, 5)
minBtn.Text = "+"; minBtn.BackgroundColor3 = Color3.fromRGB(40, 40, 40); minBtn.TextColor3 = Color3.new(1, 1, 1)
Instance.new("UICorner", minBtn)

local isMinimized = true
minBtn.MouseButton1Click:Connect(function()
    isMinimized = not isMinimized
    mainFrame:TweenSize(isMinimized and UDim2.new(0, 450, 0, 40) or UDim2.new(0, 450, 0, 500), "Out", "Quart", 0.3, true)
    minBtn.Text = isMinimized and "+" or "-"
end)

local contentFrame = Instance.new("Frame", mainFrame)
contentFrame.Size = UDim2.new(1, 0, 0, 460); contentFrame.Position = UDim2.new(0, 0, 0, 40); contentFrame.BackgroundTransparency = 1

-- [ قائمة اللاعبين ]
local playerList = Instance.new("ScrollingFrame", contentFrame)
playerList.Size = UDim2.new(0.4, 0, 0.95, 0); playerList.Position = UDim2.new(0.05, 0, 0.02, 0)
playerList.BackgroundColor3 = Color3.fromRGB(20, 20, 20); playerList.CanvasSize = UDim2.new(0, 0, 5, 0)
Instance.new("UIListLayout", playerList).Padding = UDim.new(0, 5); Instance.new("UICorner", playerList)

local controlSide = Instance.new("Frame", contentFrame)
controlSide.Size = UDim2.new(0.45, 0, 0.95, 0); controlSide.Position = UDim2.new(0.5, 0, 0.02, 0); controlSide.BackgroundTransparency = 1

local targetLabel = Instance.new("TextLabel", controlSide)
targetLabel.Size = UDim2.new(1, 0, 0, 25); targetLabel.Text = "المستهدف: الكل"; targetLabel.BackgroundColor3 = Color3.fromRGB(40, 40, 40); targetLabel.TextColor3 = Color3.new(1,1,1); Instance.new("UICorner", targetLabel)

local codeInput = Instance.new("TextBox", controlSide)
codeInput.Size = UDim2.new(1, 0, 0, 40); codeInput.Position = UDim2.new(0, 0, 0.08, 0); codeInput.Text = "140217738705613"
codeInput.BackgroundColor3 = Color3.fromRGB(30, 30, 30); codeInput.TextColor3 = Color3.new(1,1,1); codeInput.TextScaled = true; Instance.new("UICorner", codeInput)

local selectedTarget = "الكل"

local function fireRemote(target)
    local currentRemote = getRemote()
    if not currentRemote then return end
    if target == "الكل" then
        for _, p in pairs(Players:GetPlayers()) do currentRemote:FireServer(p, codeInput.Text) end
    else
        local p = Players:FindFirstChild(target)
        if p then currentRemote:FireServer(p, codeInput.Text) end
    end
end

local playBtn = Instance.new("TextButton", controlSide)
playBtn.Size = UDim2.new(1, 0, 0, 35); playBtn.Position = UDim2.new(0, 0, 0.18, 0); playBtn.Text = "تشغيل ▶️"; playBtn.BackgroundColor3 = Color3.fromRGB(0, 150, 0); playBtn.TextColor3 = Color3.new(1, 1, 1); Instance.new("UICorner", playBtn)

local spamBtn = Instance.new("TextButton", controlSide)
spamBtn.Size = UDim2.new(1, 0, 0, 35); spamBtn.Position = UDim2.new(0, 0, 0.28, 0); spamBtn.Text = "سبام 🚀"; spamBtn.BackgroundColor3 = Color3.fromRGB(50, 50, 50); spamBtn.TextColor3 = Color3.new(1, 1, 1); Instance.new("UICorner", spamBtn)

-- [[ مكتبة الموسيقى المحدثة ]]
local musicLabel = Instance.new("TextLabel", controlSide)
musicLabel.Size = UDim2.new(1, 0, 0, 20); musicLabel.Position = UDim2.new(0, 0, 0.38, 0); musicLabel.Text = "🎵 مكتبة شهد المحدثة:"; musicLabel.BackgroundTransparency = 1; musicLabel.TextColor3 = Color3.new(1,1,1)

local musicScroll = Instance.new("ScrollingFrame", controlSide)
musicScroll.Size = UDim2.new(1, 0, 0.55, 0); musicScroll.Position = UDim2.new(0, 0, 0.44, 0)
musicScroll.BackgroundColor3 = Color3.fromRGB(20, 20, 20); musicScroll.CanvasSize = UDim2.new(0, 0, 3, 0); Instance.new("UICorner", musicScroll)
local musicListLayout = Instance.new("UIListLayout", musicScroll); musicListLayout.Padding = UDim.new(0, 3)

local songs = {
    -- الأكواد الأصلية
    ["kill me 💀"] = "136373350899288",
    ["cant steal 💎"] = "91214368916078",
    ["هعهعهع 😂"] = "140669499902711",
    ["رعب 👻"] = "133594398909422",
    ["omg hack 💻"] = "121670695729530",
    ["طننننن 🔊"] = "17070340316",
    -- الأكواد الجديدة المطلوبة
    ["ازعاج 📢"] = "7713890963",
    ["اندلس 🏛️"] = "132039307762001",
    ["كداوت 💥"] = "139208046340684",
    ["شاورما 🌯"] = "80487723481385",
    ["امبيه 😮"] = "7657178494"
}

for name, id in pairs(songs) do
    local sBtn = Instance.new("TextButton", musicScroll)
    sBtn.Size = UDim2.new(1, -5, 0, 35); sBtn.Text = name; sBtn.BackgroundColor3 = Color3.fromRGB(40, 40, 40); sBtn.TextColor3 = Color3.new(1,1,1)
    sBtn.Font = Enum.Font.SourceSans; sBtn.TextSize = 14; Instance.new("UICorner", sBtn)
    sBtn.MouseButton1Click:Connect(function()
        codeInput.Text = id
        fireRemote(selectedTarget)
    end)
end

playBtn.MouseButton1Click:Connect(function() fireRemote(selectedTarget) end)
spamBtn.MouseButton1Click:Connect(function()
    spamming = not spamming
    spamBtn.Text = spamming and "🛑 STOP" or "سبام 🚀"
    spamBtn.BackgroundColor3 = spamming and Color3.fromRGB(150, 0, 0) or Color3.fromRGB(50, 50, 50)
    task.spawn(function()
        while spamming do fireRemote(selectedTarget); task.wait(0.5) end
    end)
end)

local function refreshList()
    for _, child in pairs(playerList:GetChildren()) do if child:IsA("TextButton") then child:Destroy() end end
    local allBtn = Instance.new("TextButton", playerList)
    allBtn.Size = UDim2.new(1, -10, 0, 35); allBtn.Text = "الكل (Global)"; allBtn.BackgroundColor3 = Color3.fromRGB(138, 43, 226); allBtn.TextColor3 = Color3.new(1,1,1); Instance.new("UICorner", allBtn)
    allBtn.MouseButton1Click:Connect(function() selectedTarget = "الكل"; targetLabel.Text = "المستهدف: الكل" end)
    for _, p in pairs(Players:GetPlayers()) do
        local pBtn = Instance.new("TextButton", playerList)
        pBtn.Size = UDim2.new(1, -10, 0, 35); pBtn.Text = p.Name; pBtn.BackgroundColor3 = Color3.fromRGB(45, 45, 45); pBtn.TextColor3 = Color3.new(1,1,1); Instance.new("UICorner", pBtn)
        pBtn.MouseButton1Click:Connect(function() selectedTarget = p.Name; targetLabel.Text = "المستهدف: " .. p.Name end)
    end
end

refreshList(); Players.PlayerAdded:Connect(refreshList); Players.PlayerRemoving:Connect(refreshList)
