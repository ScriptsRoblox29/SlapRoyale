local Rayfield = loadstring(game:HttpGet('https://sirius.menu/rayfield'))()

local Window = Rayfield:CreateWindow({
   Name = "Rayfield Example Window",
   Icon = 0, -- Icon in Topbar. Can use Lucide Icons (string) or Roblox Image (number). 0 to use no icon (default).
   LoadingTitle = "Rayfield Interface Suite",
   LoadingSubtitle = "by Sirius",
   Theme = "Default", -- Check https://docs.sirius.menu/rayfield/configuration/themes

   DisableRayfieldPrompts = false,
   DisableBuildWarnings = false, -- Prevents Rayfield from warning when the script has a version mismatch with the interface

   ConfigurationSaving = {
      Enabled = true,
      FolderName = nil, -- Create a custom folder for your hub/game
      FileName = "Big Hub"
   },

   Discord = {
      Enabled = false, -- Prompt the user to join your Discord server if their executor supports it
      Invite = "noinvitelink", -- The Discord invite code, do not include discord.gg/. E.g. discord.gg/ ABCD would be ABCD
      RememberJoins = true -- Set this to false to make them join the discord every time they load it up
   },

   KeySystem = false, -- Set this to true to use our key system
   KeySettings = {
      Title = "Untitled",
      Subtitle = "Key System",
      Note = "No method of obtaining the key is provided", -- Use this to tell the user how to get a key
      FileName = "Key", -- It is recommended to use something unique as other scripts using Rayfield may overwrite your key file
      SaveKey = true, -- The user's key will be saved, but if you change the key, they will be unable to use your script
      GrabKeyFromSite = false, -- If this is true, set Key below to the RAW site you would like Rayfield to get the key from
      Key = {"Hello"} -- List of keys that will be accepted by the system, can be RAW file links (pastebin, github etc) or simple strings ("hello","key22")
   }
})

local homeTab = Window:CreateTab("Home", 93059210162475)
local Section = homeTab:CreateSection("starting place")

local Button = homeTab:CreateButton({
   Name = "Auto Collect Items",
   Callback = function()
      local Players = game:GetService("Players")
      local LocalPlayer = Players.LocalPlayer
      local Character = LocalPlayer.Character or LocalPlayer.CharacterAdded:Wait()
      local HumanoidRootPart = Character:WaitForChild("HumanoidRootPart")
      local TweenService = game:GetService("TweenService")
      local VirtualInputManager = game:GetService("VirtualInputManager")

      local itemsFolder = workspace:FindFirstChild("Items")
      if not itemsFolder then return end

      local function moveToTarget(target)
         local distance = (HumanoidRootPart.Position - target.Position).Magnitude
         local duration = distance / 175
         local tween = TweenService:Create(HumanoidRootPart, TweenInfo.new(duration, Enum.EasingStyle.Linear), {CFrame = target.CFrame})
         tween:Play()
         tween.Completed:Wait()
      end

      for _, tool in pairs(itemsFolder:GetChildren()) do
         if tool:IsA("Tool") and tool:FindFirstChild("Handle") then
            moveToTarget(tool.Handle)
            task.wait(0.1)
            VirtualInputManager:SendKeyEvent(true, "F", false, game)
            VirtualInputManager:SendKeyEvent(false, "F", false, game)
            task.wait(0.2)
         end
      end
   end,
})
