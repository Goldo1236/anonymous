local TpClicOn = false 









-- fonction

_G.FLYING = false
local CONTROL = {F = 0, B = 0, L = 0, R = 0}
local lCONTROL = {F = 0, B = 0, L = 0, R = 0}
local SPEED = 5
local MOUSE = game.Players.LocalPlayer:GetMouse()
local flySpeed = 50


local function FLY()
	local BG = Instance.new('BodyGyro', game.Players.LocalPlayer.Character.HumanoidRootPart)
	local BV = Instance.new('BodyVelocity', game.Players.LocalPlayer.Character.HumanoidRootPart)
	BG.P = 9e4
	BG.maxTorque = Vector3.new(9e9, 9e9, 9e9)
	BG.cframe = game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame
	BV.velocity = Vector3.new(0, 0.1, 0)
	BV.maxForce = Vector3.new(9e9, 9e9, 9e9)


	spawn(function()
		repeat wait()
			game.Players.LocalPlayer.Character.Humanoid.PlatformStand = true
			if CONTROL.L + CONTROL.R ~= 0 or CONTROL.F + CONTROL.B ~= 0 then
				SPEED = flySpeed
			elseif not (CONTROL.L + CONTROL.R ~= 0 or CONTROL.F + CONTROL.B ~= 0) and SPEED ~= 0 then
				SPEED = 0
			end
			if (CONTROL.L + CONTROL.R) ~= 0 or (CONTROL.F + CONTROL.B) ~= 0 then
				BV.velocity = ((game.Workspace.CurrentCamera.CoordinateFrame.lookVector * (CONTROL.F + CONTROL.B)) + ((game.Workspace.CurrentCamera.CoordinateFrame * CFrame.new(CONTROL.L + CONTROL.R, (CONTROL.F + CONTROL.B) * 0.2, 0).p) - game.Workspace.CurrentCamera.CoordinateFrame.p)) * SPEED
				lCONTROL = {F = CONTROL.F, B = CONTROL.B, L = CONTROL.L, R = CONTROL.R}
			elseif (CONTROL.L + CONTROL.R) == 0 and (CONTROL.F + CONTROL.B) == 0 and SPEED ~= 0 then
				BV.velocity = ((game.Workspace.CurrentCamera.CoordinateFrame.lookVector * (lCONTROL.F + lCONTROL.B)) + ((game.Workspace.CurrentCamera.CoordinateFrame * CFrame.new(lCONTROL.L + lCONTROL.R, (lCONTROL.F + lCONTROL.B) * 0.2, 0).p) - game.Workspace.CurrentCamera.CoordinateFrame.p)) * SPEED
			else
				BV.velocity = Vector3.new(0, 0.1, 0)
			end
			BG.cframe = game.Workspace.CurrentCamera.CoordinateFrame
		until not _G.FLYING
		CONTROL = {F = 0, B = 0, L = 0, R = 0}
		lCONTROL = {F = 0, B = 0, L = 0, R = 0}
		SPEED = 0
		BG:destroy()
		BV:destroy()
		game.Players.LocalPlayer.Character.Humanoid.PlatformStand = false
	end)
end

MOUSE.KeyDown:connect(function(KEY)
	if KEY:lower() == 'w' then
		CONTROL.F = 1
	elseif KEY:lower() == 's' then
		CONTROL.B = -1
	elseif KEY:lower() == 'a' then
		CONTROL.L = -1 
	elseif KEY:lower() == 'd' then 
		CONTROL.R = 1
	end
end)

MOUSE.KeyUp:connect(function(KEY)
	if KEY:lower() == 'w' then
		CONTROL.F = 0
	elseif KEY:lower() == 's' then
		CONTROL.B = 0
	elseif KEY:lower() == 'a' then
		CONTROL.L = 0
	elseif KEY:lower() == 'd' then
		CONTROL.R = 0
	end
end)







-- init
local library = loadstring(game:HttpGet("https://raw.githubusercontent.com/oxilegeek/gui-creator/main/gui%20creator"))()
local Cheat = library.new("goldo cheats", 5013109572)


local function deleteGui()
	game.CoreGui:FindFirstChild("goldo cheats"):Destroy()
end


-- first page
local page1 = Cheat:addPage("Général", 5012544693)
local section1 = page1:addSection("Section 1")


section1:addButton("drop inv", function()
	for i,v in pairs(game.Players.LocalPlayer.Backpack:GetChildren()) do
		v.Parent = game.Players.LocalPlayer.Character
	end
	wait(1)
	game.Players.LocalPlayer.Character.Humanoid:Destroy()
end)


section1:addToggle("fly", nil, function(value)
	_G.FLYING = value
	FLY()
end)

section1:addToggle("Tp clic", nil, function(value)
	if value == true then
		TpClicOn = true
	end
	if value == false then
		TpClicOn = false
	end
end)

section1:addKeybind("touche tp", Enum.KeyCode.E, function()
	if TpClicOn == true then
		local mouse = game.Players.LocalPlayer:GetMouse()
		if mouse.Target then
			game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(mouse.Hit.x, mouse.Hit.y + 5, mouse.Hit.z)
		end
	end
end)



-- cheats
local page3 = Cheat:addPage("cheats", 5012544693)
local oxi = page3:addSection("oxi")
local AdminCmds = page3:addSection("admin cmds")

oxi:addButton("oxi cheats", function()
	loadstring(game:HttpGet("https://raw.githubusercontent.com/oxigeeklecheater/oxiscript/main/oxiscript", true))()
end)
AdminCmds:addButton("admin cmds", function()
	loadstring(game:HttpGet("https://raw.githubusercontent.com/Goldo1236/cheats/main/admin%20script", true))()
end)

--visual
local page4 = Cheat:addPage("visual", 5012544693)
local camera = page4:addSection("camera")


camera:addToggle("camera noclip", nil, function(value)
	if value == true then
		game.Players.LocalPlayer.DevCameraOcclusionMode = 1

	end
	if value == false then
		game.Players.LocalPlayer.DevCameraOcclusionMode = 0	
	end
end)




-- paramètres
local page2 = Cheat:addPage("paramètres", 5012544693)
local parametres = page2:addSection("paramètres")


parametres:addKeybind("Toggle Keybind", Enum.KeyCode.PageUp, function()
	Cheat:toggle()
end, function()
end)









game.CoreGui.ChildAdded:Connect(function(child)
	if child.Name == "goldo cheats" then
		deleteGui()
	end
end)


-- load
Cheat:SelectPage(Cheat.pages[1], true)
