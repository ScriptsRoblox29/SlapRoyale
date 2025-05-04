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
   Name = "Buscar Itens",
   Callback = function()
      task.spawn(function()
         while true do
            local tools = workspace:WaitForChild("Items"):GetChildren()
            if #tools == 0 then break end

            for _, tool in ipairs(tools) do
               if tool:IsA("Tool") and tool:FindFirstChild("Handle") then
                  local character = game.Players.LocalPlayer.Character or game.Players.LocalPlayer.CharacterAdded:Wait()
                  local hrp = character:WaitForChild("HumanoidRootPart")

                  hrp.CFrame = hrp.CFrame * CFrame.new(0, -30, 0)
                  task.wait(0.05)

                  local directionXZ = Vector3.new(tool.Handle.Position.X, hrp.Position.Y, tool.Handle.Position.Z) - hrp.Position
                  directionXZ = directionXZ.Unit * 175
                  local bv = Instance.new("BodyVelocity")
                  bv.Velocity = directionXZ
                  bv.MaxForce = Vector3.new(1, 0, 1) * 1e9
                  bv.Parent = hrp
                  task.wait(0.25)
                  bv:Destroy()

                  local directionY = Vector3.new(0, tool.Handle.Position.Y - hrp.Position.Y, 0).Unit * 175
                  local bvY = Instance.new("BodyVelocity")
                  bvY.Velocity = directionY
                  bvY.MaxForce = Vector3.new(0, 1, 0) * 1e9
                  bvY.Parent = hrp

                  repeat task.wait() until (tool.Handle.Position - hrp.Position).Magnitude <= 3 or not tool.Parent
                  bvY:Destroy()

                  local item = game.Players.LocalPlayer.Backpack:FindFirstChild(tool.Name)
                  if item and item:FindFirstChild("Handle") then
                     local args = {
                        item.Handle
                     }
                     game:GetService("ReplicatedStorage"):WaitForChild("Events"):WaitForChild("Item"):FireServer(unpack(args))
                  end
               end
            end
            task.wait(0.5)
         end
      end)
   end
})
