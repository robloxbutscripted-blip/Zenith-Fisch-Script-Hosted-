# Zenith-Fisch-Script-Hosted-
This is a hosted fisch script

-- XENO SAFE SPLASH SCRIPT (IMAGE FIXED)

-- CONFIG
local allowedPlaceId = 168556275
local IMAGE_ID = "rbxassetid://125535244058482"
local SOUND_ID = "rbxassetid://101580595290802"

-- SERVICES
local Players = game:GetService("Players")
local TweenService = game:GetService("TweenService")
local ContentProvider = game:GetService("ContentProvider")
local SoundService = game:GetService("SoundService")

local player = Players.LocalPlayer

-- PLACE ID CHECK
if game.PlaceId ~= allowedPlaceId then
    player:Kick("Wrong Place ID. Script not allowed here.")
    return
end

-- GUI
local gui = Instance.new("ScreenGui")
gui.Name = "XenoSplash"
gui.IgnoreGuiInset = true
gui.ResetOnSpawn = false
gui.Parent = player:WaitForChild("PlayerGui")

-- FULLSCREEN IMAGE
local image = Instance.new("ImageLabel")
image.Parent = gui
image.Size = UDim2.new(1, 0, 1, 0)
image.Position = UDim2.new(0, 0, 0, 0)
image.BackgroundTransparency = 1
image.Image = IMAGE_ID
image.ImageTransparency = 1
image.ScaleType = Enum.ScaleType.Stretch -- IMPORTANT FOR XENO

-- PRELOAD IMAGE (SAFE)
pcall(function()
    ContentProvider:PreloadAsync({ image })
end)

-- SHINE EFFECT
local shine = Instance.new("Frame")
shine.Parent = image
shine.Size = UDim2.new(1, 0, 1, 0)
shine.BackgroundTransparency = 1

local gradient = Instance.new("UIGradient")
gradient.Parent = shine
gradient.Color = ColorSequence.new({
    ColorSequenceKeypoint.new(0, Color3.fromRGB(255,255,255)),
    ColorSequenceKeypoint.new(1, Color3.fromRGB(140,255,255))
})

-- SOUND (XENO SAFE)
local sound = Instance.new("Sound")
sound.SoundId = SOUND_ID
sound.Volume = 10
sound.Looped = false
sound.Parent = SoundService

task.wait(0.25)
pcall(function()
    sound:Play()
end)

-- FADE IN IMAGE
task.wait(0.2)
TweenService:Create(
    image,
    TweenInfo.new(0.8, Enum.EasingStyle.Quad, Enum.EasingDirection.Out),
    { ImageTransparency = 0 }
):Play()

-- SHINE LOOP
task.spawn(function()
    while gui.Parent do
        gradient.Rotation = 0
        TweenService:Create(gradient, TweenInfo.new(1.3), { Rotation = 180 }):Play()
        task.wait(1.3)
        TweenService:Create(gradient, TweenInfo.new(1.3), { Rotation = 360 }):Play()
        task.wait(1.3)
    end
end)

-- FADE OUT
task.delay(6, function()
    TweenService:Create(image, TweenInfo.new(0.6), {
        ImageTransparency = 1
    }):Play()

    pcall(function()
        TweenService:Create(sound, TweenInfo.new(0.6), {
            Volume = 0
        }):Play()
    end)

    task.wait(0.8)
    pcall(function() sound:Destroy() end)
    pcall(function() gui:Destroy() end)
end)

-- LOAD MAIN SCRIPT
task.wait(0.1)
loadstring(game:HttpGet("https://zenithhub.cloud/panel/script"))()
