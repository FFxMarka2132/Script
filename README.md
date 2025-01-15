local OrionLib = loadstring(game:HttpGet(('https://raw.githubusercontent.com/shlexware/Orion/main/source')))();

OrionLib:MakeNotification({
	Name = "War Tycoon Hack",
	Content = "Welcome!",
	Time = 3
});

local ReplicatedStorage = game:GetService("ReplicatedStorage");
local Workspace = game:GetService("Workspace");
local Players = game:GetService("Players");
local Player = Players.LocalPlayer;

local Window = OrionLib:MakeWindow({ Name = "War Tycoon Hack - defriz", IntroEnabled = false });
local MainTab = Window:MakeTab({ Name = "Main" });
local HacksSection = MainTab:AddSection({ Name = "Hack Buttons" });
local TycoonsTab = Window:MakeTab({ Name = "Tycoons" });
local TycoonsSection = TycoonsTab:AddSection({ Name = "Tycoons" });
local TeleportsTab = Window:MakeTab({ Name = "Teleports" });
local TeleportsSection = TeleportsTab:AddSection({ Name = "Teleports" });

local speedHackValue = 40;

HacksSection:AddButton({
    Name = "Delete Doors and Gates",
    Callback = function ()
        for k,v in pairs(Workspace.Tycoon.Tycoons:GetChildren()) do
            for x,y in pairs(v.PurchasedObjects:GetChildren()) do
                if(y.Name:find("Door") or y.Name:find("Gate")) then y:destroy(); end;
            end;
        end;
    end
});

HacksSection:AddButton({
    Name = "Get Ready (deletes your visual armor and helmet)",
    Callback = function ()
        for k,v in pairs(Player.Character:GetChildren()) do
            if(v.ClassName == "Accessory") then v:destroy() end;
            if(v.Name:find("Armor")) then v.Mesh:destroy() end;
            if(v.Name:find("Helmet")) then v:destroy() end;
        end;
    end
});

HacksSection:AddButton({
	Name = "Anti-Riot Shield",
	Callback = function ()
		while true do
			wait(0);
			for k,v in pairs(Players:GetChildren()) do
				if(v.Name ~= Player.Name) then
					for _,l in pairs(Workspace[v.Name]:GetChildren()) do
						if(l.Name == "SRiot Shield" or l.Name == "Riot Shield") then l:destroy(); end;
					end;
				end;
			end;
		end;
	end    
})

HacksSection:AddButton({
	Name = "Infinite Ammo and NoReload",
	Callback = function ()
        ReplicatedStorage.BulletFireSystem.GunReload:destroy();
        local gunReload = Instance.new("Part");
        gunReload.Name = "GunReload";
        gunReload.Parent = ReplicatedStorage.BulletFireSystem;
        while true do
            wait(0);
            for _,v in pairs(Player.Character:GetChildren()) do
                if(v.ClassName == "Tool") then v.ACS_Modulo.Variaveis.Ammo.Value = 9999; end;
            end;
        end;
	end    
});

HacksSection:AddSlider({
	Name = "SpeedHack",
	Min = 5,
	Max = 100,
	Default = 25,
	Color = Color3.fromRGB(255,255,255),
	Increment = 1,
	Callback = function (value)
		speedHackValue = value;
	end    
});

TycoonsSection:AddButton({
    Name = "Your Base ("..Player.leaderstats.Team.Value..")",
    Callback = function ()
        Player.Character.HumanoidRootPart.CFrame = Workspace.Tycoon.Tycoons[Player.leaderstats.Team.Value].Essentials.Spawn.CFrame;
    end
});

for _, v in pairs({"Alpha","Bravo","Charlie","Delta","Echo","Foxtrot","Kilo","Lima","Omega","Tango","Victor","Zulu"}) do
    if(v ~= Player.leaderstats.Team.Value) then
        TycoonsSection:AddButton({
            Name = v,
            Callback = function ()
                Player.Character.HumanoidRootPart.CFrame = Workspace.Tycoon.Tycoons[v].Essentials.Spawn.CFrame;
            end 
        });
    end;
end;

TeleportsTab:AddButton({
    Name = "Capture Point",
    Callback = function ()
        Player.Character.HumanoidRootPart.CFrame = Workspace.Beams.CapturePoint1.CFrame;
    end
});

TeleportsTab:AddButton({
    Name = "Oil Barrel",
    Callback = function ()
        for _, v in pairs(Workspace.Beams:GetChildren()) do
            if(v.Name:find("Warehouse")) then
                Player.Character.HumanoidRootPart.CFrame = Workspace.Beams[v.Name].CFrame;
                break;
            end;
        end;
    end
});

TeleportsTab:AddButton({
    Name = "AirDrop",
    Callback = function ()
        for _,v in pairs(Workspace.Beams:GetChildren()) do
            if(v.Name:find("Airdrop_")) then Player.Character.HumanoidRootPart.CFrame = v.CFrame; end;
        end;
    end
});

TeleportsTab:AddButton({
    Name = "Vehicle Crate",
    Callback = function ()
        Player.Character.HumanoidRootPart.CFrame = CFrame.new(Workspace.Beams.VehicleCrate.Position.X, Workspace.Beams.VehicleCrate.Position.Y + 20, Workspace.Beams.VehicleCrate.Position.Z);
    end
});

local playersTable = {};

for _,v in pairs(Workspace:GetChildren()) do
    for z, d in pairs(v:GetChildren()) do
        if(v.Name ~= Player.Name and d.Name == "Humanoid") then table.insert(playersTable, v.Name.." ("..Players[v.Name].leaderstats.Team.Value..")"); end;
    end;
end;

local playerDropdown = TeleportsTab:AddDropdown({
    Name = "Players",
    Options = playersTable,
    Callback = function (v)
        Player.Character.HumanoidRootPart.CFrame = Workspace[v].Head.CFrame;
    end
});

OrionLib:Init();

local function getSpeedHackValue ()
    return speedHackValue;
end;

while true do
    wait(0);
	Workspace[Player.Name].Humanoid.WalkSpeed = getSpeedHackValue();
    playersTable = {};
    for _,v in pairs(Workspace:GetChildren()) do
        for z, d in pairs(v:GetChildren()) do
            if(v.Name ~= Player.Name and d.Name == "Humanoid") then
                for b, a in pairs(Players[v.Name]:GetChildren()) do
                    if(a.Name == "leaderstats") then table.insert(playersTable, v.Name.." ("..a.Team.Value..")"); end;
                end;
            end;
        end;
    end;
    playerDropdown:Refresh(playersTable, true);
end;
