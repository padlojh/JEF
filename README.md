local Library = loadstring(game:HttpGet("https://raw.githubusercontent.com/xHeptc/Kavo-UI-Library/main/source.lua"))()
local Window = Library.CreateLib("PAD HUB", "DarkTheme")

-- Auto Tab
local AutoTab = Window:NewTab("Auto")
local AutoSection = AutoTab:NewSection("Option Auto")

local function createAutoButton(buttonName, itemName)
    AutoSection:NewButton(buttonName, "Click to Toggle", function()
        local player = game.Players.LocalPlayer
        local rootPart = player.Character.HumanoidRootPart
        local pickups = game:GetService("Workspace").Pickups:GetChildren()

        local furthestDistance = 0
        local furthestItem = nil

        for _, item in pairs(pickups) do
            if item.Name == itemName then
                local distance = (item.Position - rootPart.Position).Magnitude
                if distance > furthestDistance then
                    furthestDistance = distance
                    furthestItem = item
                end
            end
        end

        if furthestItem then
            local currentPosition = rootPart.CFrame
            rootPart.CFrame = furthestItem.CFrame
            print("Teleported to " .. itemName .. "!")
            while furthestItem.Parent do
                wait(1)
            end
            rootPart.CFrame = currentPosition
            print(itemName .. " disappeared, teleported back to original position!")
        else
            print(itemName .. " not found! Please wait for item respawn.")
        end
    end)
end

createAutoButton("Auto Food", "Food")
createAutoButton("Auto Soda", "Soda")
createAutoButton("Auto Medkit", "Medkit")
createAutoButton("Auto Bat", "Bat")
createAutoButton("Auto Crowbar", "Crowbar")
createAutoButton("Auto Handgun", "Handgun")
createAutoButton("Auto Shotgun", "Shotgun")
createAutoButton("Auto Shells", "Shells")
createAutoButton("Auto Hammer", "Hammer")
createAutoButton("Auto Lantern", "Lantern")

local CollectSection = AutoTab:NewSection("Collect")
local moneyToggle

local function toggleAutoMoney(state)
    if state then
        moneyToggle = true
        spawn(function()
            while moneyToggle do
                local moneyItem = game:GetService("Workspace").Pickups:FindFirstChild("Money")
                if moneyItem then
                    local rootPart = game.Players.LocalPlayer.Character.HumanoidRootPart
                    local currentPosition = rootPart.CFrame
                    rootPart.CFrame = moneyItem.CFrame
                    print("Teleported to Money!")
                    while moneyItem.Parent do
                        wait(1)
                    end
                    rootPart.CFrame = currentPosition
                    print("Money disappeared, teleported back to original position!")
                    wait(0)
                else
                    print("No money! Please wait for item respawn.")
                    wait(2)
                end
            end
        end)
    else
        moneyToggle = false
    end
end

CollectSection:NewToggle("Auto Money", "Click to Toggle Auto Money", function(state)
    toggleAutoMoney(state)
end)

-- Player Tab
local PlayerTab = Window:NewTab("Player")
local PlayerSection = PlayerTab:NewSection("Option players")
PlayerSection:NewSlider("WalkSpeed", "ปรับ WalkSpeed สูงสุด 200", 200, 0, function(value)
    game.Players.LocalPlayer.Character.Humanoid.WalkSpeed = value
end)

-- Misc Tab
local MiscTab = Window:NewTab("Misc")
local MiscSection = MiscTab:NewSection("Misc Option")

-- สร้าง Dropdowns ชื่อ Theme UI ใน Section Select Theme UI
MiscSection:NewDropdown("Theme UI", "เลือกธีมของ UI", {"LightTheme", "DarkTheme", "GrapeTheme", "BloodTheme", "Ocean", "Midnight", "Sentinel", "Synapse"}, function(selectedTheme)
    -- เปลี่ยนธีมของ UI ตามที่เลือก
    Window:ChangeTheme(selectedTheme)
end)

-- สร้างปุ่ม Change Theme ใน Misc Tab
MiscSection:NewButton("Change Theme", "เปลี่ยนธีมของ UI ตามที่เลือก", function()
    local selectedTheme = MiscSection:GetDropdown("Theme UI"):GetSelected()
    if selectedTheme then
        Window:ChangeTheme(selectedTheme)
        print("ธีมของ UI ถูกเปลี่ยนเป็น " .. selectedTheme)
    else
        print("กรุณาเลือกธีมจาก Dropdown ก่อน")
    end
end)

-- เพิ่มปุ่มในแท็บ Misc
MiscSection:NewButton("Open ESP Player", "สร้าง Highlight สำหรับผู้เล่นทั้งหมดในเซิร์ฟเวอร์", function()
    for _, player in pairs(game.Players:GetPlayers()) do
        if player.Character and player.Character:FindFirstChild("Head") then
            local highlight = Instance.new("Highlight")
            highlight.Parent = player.Character
            highlight.Adornee = player.Character
            highlight.FillColor = Color3.fromRGB(255, 0, 0) -- สีแดง
            highlight.FillTransparency = 0.5
            highlight.OutlineColor = Color3.fromRGB(0, 0, 0) -- สีดำ
            highlight.OutlineTransparency = 0.5
        end
    end
    print("สร้าง Highlight สำหรับผู้เล่นทั้งหมดในเซิร์ฟเวอร์แล้ว")
end)

MiscSection:NewButton("Remove ESP Player", "ลบ Highlight ที่สร้างขึ้นทั้งหมด", function()
    for _, player in pairs(game.Players:GetPlayers()) do
        if player.Character and player.Character:FindFirstChild("Head") then
            local highlight = player.Character:FindFirstChildOfClass("Highlight")
            if highlight then
                highlight:Destroy()
            end
        end
    end
    print("ลบ Highlight ที่สร้างขึ้นทั้งหมดแล้ว")
end)
