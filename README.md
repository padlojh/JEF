-- สคริปต์ที่ปรับแต่ง Kavo UI Library เพื่อรองรับการลากบนมือถือและคอมพิวเตอร์
local Library = loadstring(game:HttpGet("https://raw.githubusercontent.com/xHeptc/Kavo-UI-Library/main/source.lua"))()
local Window = Library.CreateLib("PAD HUB", "DarkTheme")

local AutoTab = Window:NewTab("Auto")
local AutoSection = AutoTab:NewSection("Option Auto")

-- ฟังก์ชันสำหรับสร้างปุ่ม Auto
local function createAutoButton(buttonName, itemName)
    AutoSection:NewButton(buttonName, "Click to Toggle", function()
        local TweenService = game:GetService("TweenService")
        local player = game.Players.LocalPlayer
        local rootPart = player.Character.HumanoidRootPart
        local item = game:GetService("Workspace").Pickups:FindFirstChild(itemName)

        if item then
            local speed = 400
            local distance = (item.Position - rootPart.Position).Magnitude
            local time = distance / speed

            local goal = {}
            goal.CFrame = item.CFrame

            local tweenInfo = TweenInfo.new(time, Enum.EasingStyle.Linear, Enum.EasingDirection.Out)
            local tween = TweenService:Create(rootPart, tweenInfo, goal)

            tween:Play()

            tween.Completed:Connect(function()
                print("Teleported to " .. itemName .. "!")
                -- เพิ่มโค้ดที่ต้องการให้ทำงานเมื่อถึงตำแหน่งของ "itemName"
            end)
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

-- แท็บสำหรับการตั้งค่า
local SettingsTab = Window:NewTab("Settings")
local SettingsSection = SettingsTab:NewSection("GUI Settings")

-- สร้างกรอบเลื่อนสำหรับปุ่มต่าง ๆ
local ScrollingFrame = SettingsSection:NewScrollingFrame("Settings Scroll", "Scroll through settings", 200, 300)

-- ปุ่มสำหรับสลับการแสดงผลของ GUI
ScrollingFrame:NewButton("Toggle GUI", "คลิกเพื่อสลับการแสดงผลของ GUI", function()
    toggleGUI()
end)

-- การตั้งค่าปุ่มลัดสำหรับเปิด/ปิด GUI
ScrollingFrame:NewKeybind("Toggle Keybind", "กดปุ่มที่ต้องการใช้เปิด/ปิด GUI", Enum.KeyCode.F, function()
    toggleGUI()
end)

-- แท็บสำหรับการตั้งค่าผู้เล่น
local PlayerTab = Window:NewTab("Player")
local PlayerSection = PlayerTab:NewSection("Option players")
PlayerSection:NewSlider("WalkSpeed", "ปรับ WalkSpeed สูงสุด 200", 200, 0, function(value)
    game.Players.LocalPlayer.Character.Humanoid.WalkSpeed = value
end)
