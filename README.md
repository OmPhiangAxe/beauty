-- ═══════════════════════════════════════════════
--  FPS BOOST + GRAPHICS ENHANCER  COMBINED
--  ปิดของกินเฟรม + เพิ่มความสวยจากแสงและสี
-- ═══════════════════════════════════════════════

local Lighting = game:GetService("Lighting")
local RunService = game:GetService("RunService")
local Workspace = game:GetService("Workspace")
local TweenService = game:GetService("TweenService")

pcall(function() settings().Rendering.QualityLevel = Enum.QualityLevel.Level10 end)

-- ลบเอฟเฟกต์เก่าที่สคริปอื่นทิ้งไว้
for _, v in ipairs(Lighting:GetChildren()) do
    if v:IsA("PostEffect") and v.Name ~= "GameEffect" then
        -- เก็บไว้เฉพาะที่เราสร้างใหม่
    end
end

-- ─── [ส่วนเพิ่มความลื่น] ปิดของกินเฟรม ───
Lighting.GlobalShadows = false            -- ปิดเงา (กินเฟรมมากที่สุด)
Lighting.FogEnd = 999999                  -- ปิดหมอก
Lighting.FogStart = 0
Workspace.Terrain.WaterWaveSize = 0
Workspace.Terrain.WaterWaveSpeed = 0
Workspace.Terrain.Decoration = false      -- ปิดหญ้า/ก้อนหินเล็กๆ

-- ลด Particle / Smoke / Fire ที่ไม่จำเป็น
spawn(function()
    while task.wait(1) do
        for _, obj in ipairs(Workspace:GetDescendants()) do
            if obj:IsA("ParticleEmitter") or obj:IsA("Smoke") or obj:IsA("Fire") then
                if obj.Rate > 5 then obj.Rate = 5 end
            end
        end
    end
end)

-- ─── [ส่วนเพิ่มความสวย] ปรับแสง + สี + เลนส์ฟลา ───
Lighting.ClockTime = 14.5                  -- เวลากลางวันแสงสวย
Lighting.Brightness = 2.2
Lighting.Ambient = Color3.fromRGB(70, 75, 95)
Lighting.OutdoorAmbient = Color3.fromRGB(130, 135, 155)
Lighting.ColorShift_Top = Color3.fromRGB(255, 240, 210)
Lighting.ColorShift_Bottom = Color3.fromRGB(200, 210, 255)
Lighting.EnvironmentDiffuseScale = 1
Lighting.EnvironmentSpecularScale = 1
Lighting.ShadowSoftness = 0.2

-- Color Correction (เพิ่มความคมชัด สีสด ไม่กินเฟรม)
local cc = Instance.new("ColorCorrectionEffect")
cc.Name = "BoostCC"
cc.Parent = Lighting
cc.Brightness = 0.05
cc.Contrast = 0.18
cc.Saturation = 0.12
cc.TintColor = Color3.fromRGB(255, 250, 245)

-- Sun Rays (แสงแดดไล่ลงมา สวยไม่กินเฟรม)
local sr = Instance.new("SunRaysEffect")
sr.Name = "BoostSunRays"
sr.Parent = Lighting
sr.Intensity = 0.12
sr.Spread = 0.7

-- Bloom อ่อนๆ (ไม่กินเฟรมเท่า)
local bloom = Instance.new("BloomEffect")
bloom.Name = "BoostBloom"
bloom.Parent = Lighting
bloom.Intensity = 0.25
bloom.Size = 24
bloom.Threshold = 2.5

-- แจ้งเตือน
game.StarterGui:SetCore("SendNotification", {
    Title = "✅ โหลดสำเร็จ",
    Text = "FPS Boost + Graphics Enhancer ทำงานแล้ว",
    Duration = 5
})

print("[COMBINED] FPS Boost + Beauty Enhancer loaded!")
