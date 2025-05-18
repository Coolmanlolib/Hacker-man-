local Workspace = game:GetService("Workspace")
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local PhysicsService = game:GetService("PhysicsService")

local LocalPlayer = Players.LocalPlayer
local Character = LocalPlayer.Character
local Handle;
local FakeHandles = {}
local AutoclickerConnections = {}
local Notifications = {}
local Connections = {"Touched", "TouchEnded", "LocalSimulationTouched", "StoppedTouching"}
local Temporary = {}

local Keybinds = {
    ["Increase"] = "q";
    ["Decrease"] = "e";
    ["Autoclicker"] = "";
    ["Visibility"] = "t";
    ["Toggle"] = "x";
    ["Mute"] = "m";
}

local KeybindAction = {
    ["Autoclicker"] = false;
    ["Visible"] = true;
    ["Toggled"] = true;
    ["Muted"] = true;
}

local Configuration = {
    ["Size"] = 1.5;
    ["Increment"] = 0.5;
    ["Teamkill"] = false;
    ["MaxHealth"] = 250;
    ["Elemental Sight"] = false; 
    ["PerpetualTouch"] = false; 
    ["Moon Phase"] = false; 
    ["SpoofHandle"] = true; 
}

local Library = loadstring(game:HttpGet("https://raw.githubusercontent.com/Blissful4992/ESPs/main/3D%20Drawing%20Api.lua"))()
local Circle = Library:New3DCircle()
Circle.Visible = true
Circle.ZIndex = 0
Circle.Transparency = 1
Circle.Color = Color3.fromRGB(255, 0, 0)
Circle.Thickness = 4
Circle.Radius = Configuration["Size"]

local Circle2 = Library:New3DCircle()
Circle2.Visible = true
Circle2.ZIndex = 0
Circle2.Transparency = 1
Circle2.Color = Color3.fromRGB(255, 0, 0)
Circle2.Thickness = 4
Circle2.Radius = Configuration["Size"]
Circle2.Rotation = Vector3.new(0, 50, 0)

local Circle3 = Library:New3DCircle()
Circle3.Visible = true
Circle3.ZIndex = 0
Circle3.Transparency = 1
Circle3.Color = Color3.fromRGB(255, 0, 0)
Circle3.Thickness = 4
Circle3.Radius = Configuration["Size"]
Circle3.Rotation = Vector3.new(50, 0, 0)

local Circle4 = Library:New3DCircle()
Circle4.Visible = true
Circle4.ZIndex = 0
Circle4.Transparency = 1
Circle4.Color = Color3.fromRGB(255, 0, 0)
Circle4.Thickness = 4
Circle4.Radius = Configuration["Size"]
Circle4.Rotation = Vector3.new(0, -50, 0)

local Circle5 = Library:New3DCircle()
Circle5.Visible = true
Circle5.ZIndex = 0
Circle5.Transparency = 1
Circle5.Color = Color3.fromRGB(0, 0, 0)
Circle5.Thickness = 4
Circle5.Radius = Configuration["Size"]
Circle5.Rotation = Vector3.new(-50, 0, 0)

coroutine.wrap(function()
    while RunService.Heartbeat:Wait() do
        if #Notifications > 0 then
            local message = Notifications[1]
            local notif = Drawing.new("Text")
            notif.Visible = true
            notif.Color = Color3.fromRGB(255,255,255)
            notif.ZIndex = 1
            notif.Transparency = 1
            notif.Text = message
            notif.Size = 50
            notif.Center = true
            notif.Outline = true
            notif.OutlineColor = Color3.fromRGB(1,1,1)
            local screenSize = workspace.Camera.ViewportSize
            notif.Position = Vector2.new(screenSize.X * 0.5, screenSize.Y * 0.89) 
            notif.Font = 1
            task.wait(0.235)
            notif:Remove()
            table.remove(Notifications, 1)
        end
    end
end)()

local function NotificationSend(Message)
    table.insert(Notifications, Message)
end

LocalPlayer:GetMouse().KeyDown:Connect(function(Key)
	if Key == Keybinds["Toggle"] then
        KeybindAction["Toggled"] = not KeybindAction["Toggled"]
        Circle.Color = KeybindAction["Toggled"] and Color3.fromRGB(255, 0, 0) or Color3.fromRGB(211, 211, 211)
        Circle2.Color = KeybindAction["Toggled"] and Color3.fromRGB(255, 0, 0) or Color3.fromRGB(211, 211, 211)
        Circle3.Color = KeybindAction["Toggled"] and Color3.fromRGB(255, 0, 0) or Color3.fromRGB(211, 211, 211)
        Circle4.Color = KeybindAction["Toggled"] and Color3.fromRGB(255, 0, 0) or Color3.fromRGB(211, 211, 211)
        Circle5.Color = KeybindAction["Toggled"] and Color3.fromRGB(255, 0, 0) or Color3.fromRGB(211, 211, 211)
        
        if not KeybindAction["Muted"] then
            NotificationSend("x is now ".. tostring(KeybindAction["Toggled"]))
        end
	elseif Key == Keybinds["Increase"] then
		Configuration["Size"] = Configuration["Size"] + Configuration["Increment"]
        Circle.Radius = Configuration["Size"]
        Circle2.Radius = Configuration["Size"]
        Circle3.Radius = Configuration["Size"]
        Circle4.Radius = Configuration["Size"]
        Circle5.Radius = Configuration["Size"]
        if not KeybindAction["Muted"]  then
            NotificationSend("x is now size ".. Configuration["Size"]*2)
        end
	elseif Key == Keybinds["Decrease"] then
		Configuration["Size"] = Configuration["Size"] - Configuration["Increment"]
        Circle.Radius = Configuration["Size"]
        Circle2.Radius = Configuration["Size"]
        Circle3.Radius = Configuration["Size"]
        Circle4.Radius = Configuration["Size"]
        Circle5.Radius = Configuration["Size"]
        if not KeybindAction["Muted"]  then
            NotificationSend("x is now size ".. Configuration["Size"]*2)
        end
	elseif Key == Keybinds["Visibility"] then
		if KeybindAction["Visible"] then
			Circle.Transparency = 0
            Circle2.Transparency = 0
            Circle3.Transparency = 0
            Circle4.Transparency = 0
            Circle5.Transparency = 0
			KeybindAction["Visible"] = not KeybindAction["Visible"]
            if not KeybindAction["Muted"]  then
                NotificationSend("Visibility is ".. tostring(KeybindAction["Visible"]))
            end
		else
			Circle.Transparency = 1
            Circle2.Transparency = 1
            Circle3.Transparency = 1
            Circle4.Transparency = 1
            Circle5.Transparency = 1
			KeybindAction["Visible"] = not KeybindAction["Visible"]
            if not KeybindAction["Muted"]  then
                NotificationSend("Visibility is ".. tostring(KeybindAction["Visible"]))
            end
		end
    elseif Key == Keybinds["Autoclicker"] then
		if KeybindAction["Autoclicker"] then
			KeybindAction["Autoclicker"] = not KeybindAction["Autoclicker"]
		else
			KeybindAction["Autoclicker"] = not KeybindAction["Autoclicker"]
		end
		while KeybindAction["Autoclicker"] do
            task.wait()
			pcall(function()
                local Sword = Handle.Parent or Players.LocalPlayer.Character:FindFirstChildOfClass("Tool")
                if Sword.Enabled == true or Sword.GripUp.Z == 1 then
                    for _,v in pairs(getconnections(Sword.Activated)) do
                        if not AutoclickerConnections[v.Function] then
                            AutoclickerConnections[v.Function] = true
                            v:Disable()
                        end
                    end
                    Sword:Activate()
                    task.wait(0.14)
                    Sword:Activate()
                end 
			end)
		end
    elseif Key == Keybinds["Mute"] then
        KeybindAction["Muted"] = not KeybindAction["Muted"]
        NotificationSend("Notification mute is ".. tostring(KeybindAction["Muted"]))
	end
end)

local function SpoofHandles(Real, Fakes)
    for _,v in pairs(Connections) do
        for _,vv in pairs(getconnections(Real[v])) do
            vv:Disable()
        end
        for _,fake in pairs(Fakes) do
            for _,vv in pairs(getconnections(fake[v])) do
                vv:Disable()
            end
        end
    end
end

local function GetHandles()
    Handle = LocalPlayer.Character:FindFirstChild("Right Arm"):WaitForChild("RightGrip", math.huge).Part1
    FakeHandles = {}
    for _,v in pairs(Handle:GetConnectedParts()) do
        if v ~= Handle and v.Position == Handle.Position and v.Size == Handle.Size and v.Transparency >= 0.5 and v.Parent ~= Character and PhysicsService:CollisionGroupsAreCollidable(v.CollisionGroup, Handle.CollisionGroup) then
            table.insert(FakeHandles, v)
        end
    end
    if Configuration["SpoofHandle"] then
        SpoofHandles(Handle, FakeHandles)
    end
    Character = LocalPlayer.Character
end

local Params = OverlapParams.new()
Params.FilterType = Enum.RaycastFilterType.Blacklist
Params.FilterDescendantsInstances = {}
Params.MaxParts = 0
Params.RespectCanCollide = false

RunService.Heartbeat:Connect(function()
    pcall(function()
        if Handle and LocalPlayer.Character == Character and Handle == LocalPlayer.Character:FindFirstChild("Right Arm"):WaitForChild("RightGrip", math.huge).Part1 then
            Circle.Position = Handle.Position
            Circle2.Position = Handle.Position
            Circle3.Position = Handle.Position
            Circle4.Position = Handle.Position
            Circle5.Position = Handle.Position
        else
            GetHandles()
        end
    end)
end)

while true do
    task.wait()
    pcall(function()
        if KeybindAction["Toggled"] then
            if Handle then
                local SpatialQuery = Workspace:GetPartBoundsInRadius(Handle.Position, Configuration["Size"], Params)
                for _,v in pairs(SpatialQuery) do
                    if v.Position ~= Handle.Position and v.Size.X > 0.4 and v.Parent:IsA("Model") and v.Parent ~= LocalPlayer.Character and v.Parent:FindFirstChild("Humanoid") and v.Parent:FindFirstChild("Humanoid").Health > 0
                    and v.Parent:FindFirstChild("Humanoid").MaxHealth <= Configuration["MaxHealth"] and v.Parent:FindFirstChild("Torso").Transparency == 0 then
                        for _,vv in pairs(v.Parent:GetChildren()) do
                            if vv:IsA("BasePart") and vv.Anchored == false and vv.CanTouch == true and PhysicsService:CollisionGroupsAreCollidable(vv.CollisionGroup, Handle.CollisionGroup) and
                            (vv.Position - Handle.Position).Magnitude < Configuration["Size"] + 4.5 then
                                if #FakeHandles >= 1 then
                                    task.spawn(function()
                                        for _,vvv in pairs(FakeHandles) do
                                            firetouchinterest(vvv, vv, 1)
                                            firetouchinterest(vvv, vv, 0)
                                        end
                                    end)
                                end
                                firetouchinterest(Handle, vv, 1)
                                firetouchinterest(Handle, vv, 0)
                            end
                        end
                    end
                end
            end
        end
    end)
end
