local repo = 'https://raw.githubusercontent.com/violin-suzutsuki/LinoriaLib/main/'

local Library = loadstring(game:HttpGet('https://raw.githubusercontent.com/Zyai-Lua/Inertia.mov/refs/heads/main/Linoria'))()
local ThemeManager = loadstring(game:HttpGet('https://raw.githubusercontent.com/Zyai-Lua/Inertia.mov/refs/heads/main/LinoriaTheme'))()
local SaveManager = loadstring(game:HttpGet(repo .. 'addons/SaveManager.lua'))()

local Players = game:GetService('Players');
local Workspace = game:GetService('Workspace');
local RunService = game:GetService('RunService');
local ReplicatedStorage = game:GetService('ReplicatedStorage');
local UserInputService = game:GetService('UserInputService');

local LocalPlayer = Players.LocalPlayer;
local Character = LocalPlayer.Character or LocalPlayer.CharacterAdded:Wait()
local humanoid = Character:FindFirstChildOfClass("Humanoid")
local HumanoidRootPart = Character.HumanoidRootPart

local Basketballs = game.Workspace.Basketballs

local Camera = Workspace.CurrentCamera;
local ShootFunction;
for i,v in next, getgc(false) do
	if type(v) == "function" then
		if debug.getinfo(v).name == "shoot" or debug.getinfo(v).name == "Shoot" then
			ShootFunction = v
		end
	end
end

local args = {
    true,
    100,
    [4] = false
}

local Window = Library:CreateWindow({
  Title = 'Inertia | <font color=\"#00ff00\">Free</font> | Arcade Basketball',
  Center = true,
  AutoShow = true,
  TabPadding = 8,
  MenuFadeTime = 0.2
})

local Tabs = {
  Main = Window:AddTab('Main'),
  ['UI Settings'] = Window:AddTab('UI Settings'),
}

local Main = Tabs.Main:AddLeftGroupbox('Main')

Main:AddToggle('AutoGreen', {Text = 'Auto Green',})
Main:AddToggle('WalkSpeed', {Text = 'Walk Speed',})
Main:AddSlider('Speed', {
  Text = 'Speed',
  Default = 16,
  Min = 0,
  Max = 40,
  Rounding = 2,
})
Main:AddToggle('AutoGreenRageToggle', {
  Text = '<font color=\"#ff0000\">Force Green</font>',
  Default = false,
  Tooltip = 'Very obvious -- works the best',

})


local ShootRemote = game:GetService("ReplicatedStorage"):WaitForChild("Events"):WaitForChild("Shoot")
local namecall

namecall = hookmetamethod(game,"__namecall",function(self,...)
    local args = {...}
    local method = getnamecallmethod():lower()
    if not checkcaller() and self == ShootRemote and method == "fireserver" and Toggles.AutoGreen.Value then
        args[2] = -0.98
        return namecall(self,unpack(args))
    end
    return namecall(self,...)
end)

local old; old = hookfunction(ShootFunction, function(...)
    if Toggles.AutoGreenRageToggle.Value then
        local v36 = -0.99
        
        game:GetService("ReplicatedStorage"):WaitForChild("Events"):WaitForChild("Shoot"):FireServer(unpack(args))

        ShootRemote:FireServer(false, v36, false)
    else
        old(...)
    end
end)

RunService.RenderStepped:Connect(function()
    if Toggles.WalkSpeed.Value then
        Character.Humanoid.WalkSpeed = Options.Speed.Value
    end
end)

Library:SetWatermarkVisibility(true)

local FrameTimer = tick()
local FrameCounter = 0;
local FPS = 60;

Fag = RunService.RenderStepped:Connect(function()
  FrameCounter += 1;

  if (tick() - FrameTimer) >= 1 then
    FPS = FrameCounter;
    FrameTimer = tick();
    FrameCounter = 0;
  end;

  Library:SetWatermark(('Inertia | Free | %s fps | %s ms'):format(
    math.floor(FPS),
    math.floor(game:GetService('Stats').Network.ServerStatsItem['Data Ping']:GetValue())
    ));
end);

Library.KeybindFrame.Visible = true;

Library:OnUnload(function()
  Library.Unloaded = true
end)

local MenuGroup = Tabs['UI Settings']:AddLeftGroupbox('Menu')
MenuGroup:AddLabel('Made by: Zyai, The')
MenuGroup:AddButton('Copy Discord', function() setclipboard("https://discord.gg/J7A8pzqPYE") end)
MenuGroup:AddButton('Unload', function() Library:Unload() end)
MenuGroup:AddToggle('WatermarkVisibiltyToggle', {
  Text = 'Hide Watermark',
  Default = false,
  Tooltip = 'Hides Watermark',

  Callback = function(Value)
    Library:SetWatermarkVisibility(not Value)
  end
})
MenuGroup:AddLabel('Menu bind'):AddKeyPicker('MenuKeybind', { Default = 'End', NoUI = true, Text = 'Menu keybind' })
Library.ToggleKeybind = Options.MenuKeybind
ThemeManager:SetLibrary(Library)
SaveManager:SetLibrary(Library)
SaveManager:IgnoreThemeSettings()
SaveManager:SetIgnoreIndexes({ 'MenuKeybind' })
ThemeManager:SetFolder('Inertia.mov')
SaveManager:SetFolder('Inertia.mov/ArcadeBasketball')
SaveManager:BuildConfigSection(Tabs['UI Settings'])
ThemeManager:ApplyToTab(Tabs['UI Settings'])
SaveManager:LoadAutoloadConfig()

Library:Notify(("Welcome to Inertia.Mov "..LocalPlayer.Name.." 👋"), 6)
Library:Notify(("Status: 🟢 - Undetected (Safe to use)"), 6)
