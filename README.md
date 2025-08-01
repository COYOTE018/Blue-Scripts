```lua
-- ScriptMenu.lua

local Players = game:GetService("Players")
local player = Players.LocalPlayer
local mouse = player:GetMouse()
local UIS = game:GetService("UserInputService")

local screenGui = Instance.new("ScreenGui", player:WaitForChild("PlayerGui"))
screenGui.Name = "ScriptMenu"

local menuFrame = Instance.new("Frame", screenGui)
menuFrame.Size = UDim2.new(0, 160, 0, 180)
menuFrame.Position = UDim2.new(0, 20, 0, 80)
menuFrame.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
menuFrame.Visible = true

local toggleButton = Instance.new("TextButton", screenGui)
toggleButton.Size = UDim2.new(0, 100, 0, 40)
toggleButton.Position = UDim2.new(0, 20, 0, 20)
toggleButton.Text = "Abrir/Fechar"
toggleButton.BackgroundColor3 = Color3.fromRGB(80, 80, 80)
toggleButton.TextColor3 = Color3.new(1, 1, 1)

local flyButton = Instance.new("TextButton", menuFrame)
local speedButton = Instance.new("TextButton", menuFrame)
local tpButton = Instance.new("TextButton", menuFrame)local function setupButton(btn, text, posY)
	btn.Size = UDim2.new(0, 140, 0, 40)
	btn.Position = UDim2.new(0, 10, 0, posY)
	btn.Text = text
	btn.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
	btn.TextColor3 = Color3.new(1, 1, 1)
end

setupButton(flyButton, "Voar", 10)
setupButton(speedButton, "Velocidade", 60)
setupButton(tpButton, "Teleportar", 110)

toggleButton.MouseButton1Click:Connect(function()
	menuFrame.Visible = not menuFrame.Visible
end)

local flying = false
flyButton.MouseButton1Click:Connect(function()
	flying = not flying
	local char = player.Character
	local hrp = char:WaitForChild("HumanoidRootPart")
	local bv = hrp:FindFirstChild("FlyForce") or Instance.new("BodyVelocity", hrp)
	bv.Name = "FlyForce"
	bv.MaxForce = Vector3.new(1e9, 1e9, 1e9)
	bv.Velocity = Vector3.zero

	if flying then
		game:GetService("RunService").Heartbeat:Connect(function()
			if flying and bv and hrp then
				local dir = Vector3.new()
				if UIS:IsKeyDown(Enum.KeyCode.W) then dir += Vector3.new(0, 0, -1) end
				if UIS:IsKeyDown(Enum.KeyCode.S) then dir += Vector3.new(0, 0, 1) end
				if UIS:IsKeyDown(Enum.KeyCode.A) then dir += Vector3.new(-1, 0, 0) end
				if UIS:IsKeyDown(Enum.KeyCode.D) then dir += Vector3.new(1, 0, 0) end
				bv.Velocity = hrp.CFrame:VectorToWorldSpace(dir) * 50
			endend)
	else
		if bv then bv:Destroy() end
	end
end)

speedButton.MouseButton1Click:Connect(function()
	local humanoid = player.Character and player.Character:FindFirstChildOfClass("Humanoid")
	if humanoid then
		humanoid.WalkSpeed = 100
	end
end)

local teleporting = false
tpButton.MouseButton1Click:Connect(function()
	teleporting = not teleporting
	if teleporting then
		mouse.Button1Down:Connect(function()
			if teleporting and mouse.Hit then
				player.Character:MoveTo(mouse.Hit.Position)
			end
		end)
	end
end)
```
