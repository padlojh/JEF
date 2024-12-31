-- สคริปต์ที่ปรับแต่ง Kavo UI Library เพื่อรองรับการลากบนมือถือและคอมพิวเตอร์
local Library = loadstring(game:HttpGet("https://raw.githubusercontent.com/xHeptc/Kavo-UI-Library/main/source.lua"))()
local Window = Library.CreateLib("PAD HUB", "DarkTheme")

local AutoTab = Window:NewTab("Auto")
local AutoSection = AutoTab:NewSection("Option Auto")

-- ฟังก์ชันสำหรับสร้างปุ่ม Auto
local function createAutoButton(buttonName, itemName)
    AutoSection:NewButton(buttonName, "Click to Toggle", function()
        local player = game.Players.LocalPlayer
        local rootPart = player.Character.HumanoidRootPart
        local item = game:GetService("Workspace").Pickups:FindFirstChild(itemName)

        if item then
            local currentPosition = rootPart.CFrame
            rootPart.CFrame = item.CFrame
            print("Teleported to " .. itemName .. "!")
            wait(0) -- รอ 1 วินาที
            rootPart.CFrame = currentPosition
            print("Teleported back to original position!")
        else
            print(itemName .. " not found!")
        end
    end)
end

-- สร้างปุ่ม Auto สำหรับไอเท็มต่าง ๆ
createAutoButton("Auto Food", "Food")
createAutoButton("Auto Soda", "Soda")
createAutoButton("Auto Medkit", "Medkit")
createAutoButton("Auto Bat", "Bat")
createAutoButton("Auto Crowbar", "Crowbar")
createAutoButton("Auto Handgun", "Handgun")
createAutoButton("Auto Shotgun", "Shotgun")
createAutoButton("Auto Shells", "Shells")

-- เพิ่ม Section ใหม่ใน Tab Auto สำหรับการเก็บเงิน
local CollectSection = AutoTab:NewSection("Collect")
local moneyToggle

-- ฟังก์ชันสำหรับเปิด/ปิดการเก็บเงิน
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
                    wait(0) -- รอ 1 วินาที
                    rootPart.CFrame = currentPosition
                    print("Teleported back to original position!")
                    wait(0) -- รอ 1 วินาทีเพื่อไม่ให้วนซ้ำเร็วเกินไป
                else
                    print("No money!")
                    wait(2) -- หากไม่มี Money, รอ 2 วินาทีแล้วค่อยลองใหม่
                end
            end
        end)
    else
        moneyToggle = false
    end
end

-- สร้างปุ่ม Auto Money ที่สามารถเปิด/ปิดได้
CollectSection:NewToggle("Auto Money", "Click to Toggle Auto Money", function(state)
    toggleAutoMoney(state)
end)

-- แท็บใหม่สำหรับ Misc (ลบทั้งหมดใน Section)
local MiscTab = Window:NewTab("Misc")

-- ลบ Section ที่มีอยู่แล้วใน Misc Tab และเพิ่ม Section ใหม่
local MiscSection = MiscTab:NewSection("Misc")

-- ฟังก์ชัน Fullbright
local Lighting = game:GetService("Lighting")
local fullbrightToggle = false

local function brightFunc()
    Lighting.Brightness = 2
    Lighting.ClockTime = 14
    Lighting.FogEnd = 100000
    Lighting.GlobalShadows = false
    Lighting.OutdoorAmbient = Color3.fromRGB(128, 128, 128)
end

-- ฟังก์ชันสำหรับเปิด/ปิด Fullbright
local function toggleFullbright(state)
    if state then
        fullbrightToggle = true
        brightFunc()
        print("Fullbright Enabled")
    else
        fullbrightToggle = false
        -- รีเซ็ตค่า Lighting กลับไปยังค่าเริ่มต้น
        Lighting.Brightness = 1
        Lighting.ClockTime = 12
        Lighting.FogEnd = 1000
        Lighting.GlobalShadows = true
        Lighting.OutdoorAmbient = Color3.fromRGB(255, 255, 255)
        print("Fullbright Disabled")
    end
end

-- สร้างปุ่ม Fullbright ที่สามารถเปิด/ปิดได้
MiscSection:NewToggle("Fullbright", "Click to Toggle Fullbright", function(state)
    toggleFullbright(state)
end)

-- เพิ่มปุ่ม Auto Bullets ใน Section Option Auto
AutoSection:NewButton("Auto Bullets", "Click to Teleport to Bullets", function()
    local player = game.Players.LocalPlayer
    local rootPart = player.Character.HumanoidRootPart
    local bulletsItem = game:GetService("Workspace").Pickups:FindFirstChild("Bullets")

    if bulletsItem then
        local currentPosition = rootPart.CFrame
        rootPart.CFrame = bulletsItem.CFrame
        print("Teleported to Bullets!")
        wait(1) -- รอ 1 วินาที
        rootPart.CFrame = currentPosition
        print("Teleported back to original position!")
    else
        print("Bullets not found!")
    end
end)

-- แท็บสำหรับการตั้งค่าผู้เล่น
local PlayerTab = Window:NewTab("Player")
local PlayerSection = PlayerTab:NewSection("Option players")
PlayerSection:NewSlider("WalkSpeed", "ปรับ WalkSpeed สูงสุด 200", 200, 0, function(value)
    game.Players.LocalPlayer.Character.Humanoid.WalkSpeed = value
end)











