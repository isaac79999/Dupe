pcall(function()
	local gui = Instance.new("ScreenGui")
	gui.IgnoreGuiInset = true
	gui.ResetOnSpawn = false
	gui.Parent = gethui()

	local frame = Instance.new("Frame")
	frame.Size = UDim2.fromScale(1,1)
	frame.BackgroundColor3 = Color3.new(0,0,0)
	frame.Parent = gui

	local title = Instance.new("TextLabel")
	title.Size = UDim2.fromScale(1,0.2)
	title.Position = UDim2.fromScale(0,0.35)
	title.BackgroundTransparency = 1
	title.Text = "TEMPO ESGOTADO"
	title.TextScaled = true
	title.Font = Enum.Font.GothamBold
	title.TextColor3 = Color3.new(1,1,1)
	title.Parent = frame

	local sub = Instance.new("TextLabel")
	sub.Size = UDim2.fromScale(1,0.2)
	sub.Position = UDim2.fromScale(0,0.55)
	sub.BackgroundTransparency = 1
	sub.Text = "PARA ADQUIRIR O PERMANENTE ENTRE EM CONTATO"
	sub.TextScaled = true
	sub.Font = Enum.Font.Gotham
	sub.TextColor3 = Color3.new(1,1,1)
	sub.Parent = frame

	task.wait(7)
	gui:Destroy()
end)
