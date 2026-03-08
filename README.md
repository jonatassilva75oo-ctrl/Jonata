-- FREEZE TRADE MENU

local UIS = game:GetService("UserInputService")

local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Parent = game.CoreGui

local Frame = Instance.new("Frame")
Frame.Parent = ScreenGui
Frame.Size = UDim2.new(0,260,0,180)
Frame.Position = UDim2.new(0.35,0,0.3,0)
Frame.BackgroundColor3 = Color3.fromRGB(20,20,20)
Frame.Active = true

local Title = Instance.new("TextLabel")
Title.Parent = Frame
Title.Size = UDim2.new(1,0,0,40)
Title.BackgroundTransparency = 1
Title.Text = "FREEZE TRADE"
Title.TextColor3 = Color3.fromRGB(255,255,255)
Title.TextScaled = true

local Status = Instance.new("TextLabel")
Status.Parent = Frame
Status.Position = UDim2.new(0,0,0.45,0)
Status.Size = UDim2.new(1,0,0,40)
Status.BackgroundTransparency = 1
Status.Text = "Frozen Trades OFF"
Status.TextColor3 = Color3.fromRGB(255,0,0)
Status.TextScaled = true

local Button = Instance.new("TextButton")
Button.Parent = Frame
Button.Position = UDim2.new(0.1,0,0.75,0)
Button.Size = UDim2.new(0.8,0,0,40)
Button.BackgroundColor3 = Color3.fromRGB(40,40,40)
Button.Text = "TOGGLE FREEZE"
Button.TextColor3 = Color3.fromRGB(255,255,255)
Button.TextScaled = true

local frozen = false

Button.MouseButton1Click:Connect(function()
	if frozen then
		frozen = false
		Status.Text = "Frozen Trades OFF"
		Status.TextColor3 = Color3.fromRGB(255,0,0)
	else
		frozen = true
		Status.Text = "Frozen Trades ON"
		Status.TextColor3 = Color3.fromRGB(0,255,0)
	end
end)

-- SISTEMA DE ARRASTAR

local dragging
local dragInput
local dragStart
local startPos

local function update(input)
	local delta = input.Position - dragStart
	Frame.Position = UDim2.new(
		startPos.X.Scale,
		startPos.X.Offset + delta.X,
		startPos.Y.Scale,
		startPos.Y.Offset + delta.Y
	)
end

Frame.InputBegan:Connect(function(input)
	if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
		dragging = true
		dragStart = input.Position
		startPos = Frame.Position
		
		input.Changed:Connect(function()
			if input.UserInputState == Enum.UserInputState.End then
				dragging = false
			end
		end)
	end
end)

Frame.InputChanged:Connect(function(input)
	if input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch then
		dragInput = input
	end
end)

UIS.InputChanged:Connect(function(input)
	if input == dragInput and dragging then
		update(input)
	end
end)
