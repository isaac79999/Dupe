local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local LocalPlayer = Players.LocalPlayer
local Backpack = LocalPlayer:WaitForChild("Backpack")
local Character = LocalPlayer.Character or LocalPlayer.CharacterAdded:Wait()

-- Criar UI protegida
local hiddenGui = Instance.new("ScreenGui")
hiddenGui.Name = "UI_"..tostring(math.random(100000,999999))
hiddenGui.DisplayOrder = math.random(1000,9999)
hiddenGui.ResetOnSpawn = false
hiddenGui.IgnoreGuiInset = true
hiddenGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
hiddenGui.Parent = (LocalPlayer:FindFirstChildOfClass("PlayerGui") or LocalPlayer:WaitForChild("PlayerGui"))

-- Container centralizado e leve
local container = Instance.new("Frame")
container.Size = UDim2.new(0, 280, 0, 60)
container.Position = UDim2.new(0.5, -140, 0.88, 0)
container.BackgroundTransparency = 1
container.Name = "Container_"..math.random(100000,999999)
container.Parent = hiddenGui

local layout = Instance.new("UIGridLayout")
layout.CellSize = UDim2.new(0, 55, 0, 55)
layout.CellPadding = UDim2.new(0, 3, 0, 3)
layout.FillDirectionMaxCells = 6
layout.HorizontalAlignment = Enum.HorizontalAlignment.Center
layout.Parent = container

local toolButtons = {}

local function safe(func)
	local ok, result = pcall(func)
	if not ok then
		-- Não exibir erros nem warnings
	end
end

local function atualizarInventario()
	local tools = {}

	for _, obj in ipairs(Backpack:GetChildren()) do
		if obj:IsA("Tool") then
			table.insert(tools, obj)
		end
	end

	for _, obj in ipairs(Character:GetChildren()) do
		if obj:IsA("Tool") then
			table.insert(tools, obj)
		end
	end

	for _, tool in ipairs(tools) do
		if not toolButtons[tool] then
			safe(function()
				local btn = Instance.new("ImageButton")
				btn.Size = UDim2.new(0, 55, 0, 55)
				btn.BackgroundTransparency = 0.3
				btn.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
				btn.BorderSizePixel = 0
				btn.Image = tool.TextureId ~= "" and tool.TextureId or "rbxasset://Textures/Tool.png"
				btn.Name = "Slot_" .. math.random(10000,99999)
				btn.Parent = container

				local label = Instance.new("TextLabel")
				label.Size = UDim2.new(1, 0, 0.3, 0)
				label.Position = UDim2.new(0, 0, 0.7, 0)
				label.BackgroundTransparency = 1
				label.Text = tool.Name
				label.TextColor3 = Color3.new(1, 1, 1)
				label.TextScaled = true
				label.Font = Enum.Font.Gotham
				label.Parent = btn

				btn.MouseButton1Click:Connect(function()
					safe(function()
						if tool.Parent == Backpack then
							tool.Parent = Character
						elseif tool.Parent == Character then
							tool.Parent = Backpack
						end
					end)
				end)

				tool.AncestryChanged:Connect(function()
					safe(function()
						if not (tool:IsDescendantOf(Backpack) or tool:IsDescendantOf(Character)) then
							if toolButtons[tool] then
								toolButtons[tool]:Destroy()
								toolButtons[tool] = nil
							end
						end
					end)
				end)

				toolButtons[tool] = btn
			end)
		end
	end
end

-- Conexões protegidas
safe(function()
	Backpack.ChildAdded:Connect(atualizarInventario)
	Backpack.ChildRemoved:Connect(atualizarInventario)
	Character.ChildAdded:Connect(atualizarInventario)
	Character.ChildRemoved:Connect(atualizarInventario)
end)

-- Atualizar inicialmente
safe(atualizarInventario)
