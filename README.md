local player = game.Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local humanoid = character:WaitForChild("Humanoid")
local UIS = game:GetService("UserInputService")

-- Botões
local gui = script.Parent
local halfHealthBtn = gui:WaitForChild("HalfHealthButton")
local flyBtn = gui:WaitForChild("FlyButton")
local noClipBtn = gui:WaitForChild("NoClipButton")
local speedBtn = gui:WaitForChild("SpeedButton")
local resetBtn = gui:WaitForChild("ResetButton")

-- Estados
local flying = false
local noclip = false
local speedActive = false

-- Função: Perder metade da vida
halfHealthBtn.MouseButton1Click:Connect(function()
	humanoid.Health = humanoid.Health / 2
end)

-- Função: Voar
flyBtn.MouseButton1Click:Connect(function()
	flying = not flying
	if flying then
		local bodyVelocity = Instance.new("BodyVelocity")
		bodyVelocity.Velocity = Vector3.new(0, 50, 0)
		bodyVelocity.MaxForce = Vector3.new(0, math.huge, 0)
		bodyVelocity.Parent = character:WaitForChild("HumanoidRootPart")
		bodyVelocity.Name = "FlyVelocity"
	else
		local fly = character:FindFirstChild("HumanoidRootPart"):FindFirstChild("FlyVelocity")
		if fly then fly:Destroy() end
	end
end)

-- Função: Atravessar parede (NoClip)
noClipBtn.MouseButton1Click:Connect(function()
	noclip = not noclip
end)

game:GetService("RunService").Stepped:Connect(function()
	if noclip and character then
		for _, part in pairs(character:GetDescendants()) do
			if part:IsA("BasePart") and part.CanCollide == true then
				part.CanCollide = false
			end
		end
	end
end)

-- Função: Super velocidade
speedBtn.MouseButton1Click:Connect(function()
	speedActive = not speedActive
	if speedActive then
		humanoid.WalkSpeed = 100
	else
		humanoid.WalkSpeed = 16
	end
end)

-- Resetar tudo
resetBtn.MouseButton1Click:Connect(function()
	humanoid.WalkSpeed = 16
	local fly = character:FindFirstChild("HumanoidRootPart"):FindFirstChild("FlyVelocity")
	if fly then fly:Destroy() end
	flying = false
	noclip = false
	speedActive = false
end)
