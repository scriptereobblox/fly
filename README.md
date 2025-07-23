local button = script.Parent
local flying = false
local UIS = game:GetService("UserInputService")
local player = game.Players.LocalPlayer
local char = player.Character or player.CharacterAdded:Wait()
local humanoidRootPart = char:WaitForChild("HumanoidRootPart")

local bodyGyro
local bodyVelocity

-- Fly function
local function toggleFly()
	flying = not flying
	if flying then
		bodyGyro = Instance.new("BodyGyro")
		bodyGyro.P = 9e4
		bodyGyro.maxTorque = Vector3.new(9e9, 9e9, 9e9)
		bodyGyro.cframe = humanoidRootPart.CFrame
		bodyGyro.Parent = humanoidRootPart

		bodyVelocity = Instance.new("BodyVelocity")
		bodyVelocity.velocity = Vector3.new(0, 0, 0)
		bodyVelocity.maxForce = Vector3.new(9e9, 9e9, 9e9)
		bodyVelocity.Parent = humanoidRootPart

		-- Control flying with WASD
		UIS.InputBegan:Connect(function(input, gameProcessed)
			if not flying or gameProcessed then return end
			if input.KeyCode == Enum.KeyCode.W then
				bodyVelocity.velocity = humanoidRootPart.CFrame.LookVector * 50
			elseif input.KeyCode == Enum.KeyCode.S then
				bodyVelocity.velocity = -humanoidRootPart.CFrame.LookVector * 50
			elseif input.KeyCode == Enum.KeyCode.A then
				bodyVelocity.velocity = -humanoidRootPart.CFrame.RightVector * 50
			elseif input.KeyCode == Enum.KeyCode.D then
				bodyVelocity.velocity = humanoidRootPart.CFrame.RightVector * 50
			elseif input.KeyCode == Enum.KeyCode.Space then
				bodyVelocity.velocity = Vector3.new(0, 50, 0)
			end
		end)

		UIS.InputEnded:Connect(function()
			if flying then
				bodyVelocity.velocity = Vector3.new(0, 0, 0)
			end
		end)

	else
		if bodyGyro then bodyGyro:Destroy() end
		if bodyVelocity then bodyVelocity:Destroy() end
	end
end

-- Button click
button.MouseButton1Click:Connect(toggleFly)
