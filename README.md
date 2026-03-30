-- CORINGA HUB - by Joao
-- Biblioteca: Rayfield UI (https://sirius.menu/rayfield)
-- Compativel com: Delta (Mobile), Synapse X, Fluxus, Solara
-- Mobile & PC Compatible

getgenv().CORINGA = getgenv().CORINGA or {}
local G = getgenv().CORINGA

G.AimBot=false; G.SilentAim=false; G.AimLock=false
G.AimSmooth=0.3; G.AimFOV=120; G.AimPart="Head"; G.Prediction=false
G.FOVCircle=false; G.FOVRadius=120; G.FOVColor=Color3.fromRGB(255,0,0)
G.AimKill=false; G.TriggerBot=false; G.KillAura=false; G.KillAuraRad=20
G.BoxESP=false; G.NameESP=false; G.HealthBar=false; G.HealthText=false
G.DistESP=false; G.Tracers=false; G.HeadDot=false; G.TeamCheck=false
G.ESPThick=1; G.ESPTransp=1; G.ESPTextSize=13
G.ESPEnemyColor=Color3.fromRGB(255,0,0); G.ESPTeamColor=Color3.fromRGB(0,255,0)
G.SpinBot=false; G.SpinSpeed=10; G.Fly=false; G.FlySpeed=50
G.SpeedHack=false; G.SpeedMult=1; G.InfJump=false; G.NoClip=false
G.WalkSpeed=16; G.JumpPower=50
G.GodMode=false; G.AntiAFK=false; G.AntiKick=true; G.AntiBan=true
G.FakeLag=false; G.AutoFarm=false; G.RemoveFog=false; G.Fullbright=false
G.FPSCounter=false

local Players=game:GetService("Players")
local RunService=game:GetService("RunService")
local UserInputService=game:GetService("UserInputService")
local Lighting=game:GetService("Lighting")
local TeleportService=game:GetService("TeleportService")
local VirtualUser=game:GetService("VirtualUser")
local Camera=workspace.CurrentCamera
local LocalPlayer=Players.LocalPlayer

-- Carrega Rayfield
local Rayfield
pcall(function() Rayfield=loadstring(game:HttpGet("https://sirius.menu/rayfield"))() end)
if not Rayfield then
    pcall(function() Rayfield=loadstring(game:HttpGet("https://raw.githubusercontent.com/shlexware/Rayfield/main/source"))() end)
end
if not Rayfield then
    Rayfield={CreateWindow=function(_,c) local w={}
        function w:CreateTab() return{CreateSection=function()end,CreateToggle=function(_,x)if x.Callback then x.Callback(x.CurrentValue or false)end end,CreateSlider=function(_,x)if x.Callback then x.Callback(x.CurrentValue or 0)end end,CreateDropdown=function(_,x)if x.Callback then x.Callback({x.CurrentOption and x.CurrentOption[1] or x.Options[1]})end end,CreateColorPicker=function(_,x)if x.Callback then x.Callback(x.Color or Color3.new(1,0,0))end end,CreateButton=function()end,CreateLabel=function()end} end
        function w:Notify(d) print("[CORINGA]",d.Title,d.Content) end
        return w
    end}
end

local Window=Rayfield:CreateWindow({Name="CORINGA HUB",LoadingTitle="CORINGA HUB",LoadingSubtitle="by Joao",Theme="Default",DisableRayfieldPrompts=false,DisableBuildWarnings=true,ConfigurationSaving={Enabled=false},KeySystem=false})

-- HOME TAB
local HomeTab=Window:CreateTab("Home",4483362458)
HomeTab:CreateSection("Bem-vindo")
HomeTab:CreateLabel("CORINGA HUB - by Joao")
HomeTab:CreateLabel("Biblioteca: Rayfield UI")
HomeTab:CreateLabel("Toggle UI: RightShift")
HomeTab:CreateLabel("Compat.: Delta Mobile | Synapse | Fluxus | Solara")
HomeTab:CreateSection("Status")
HomeTab:CreateLabel("Anti-Kick: ATIVO por padrao")
HomeTab:CreateLabel("Anti-Ban:  ATIVO por padrao")

-- COMBAT TAB
local CombatTab=Window:CreateTab("Combat",4483362458)
CombatTab:CreateSection("AimBot")
CombatTab:CreateToggle({Name="AimBot (SilentAim+AimLock)",CurrentValue=false,Flag="AimBot",Callback=function(v) G.AimBot=v;G.SilentAim=v;G.AimLock=v end})
CombatTab:CreateSlider({Name="Smoothness",Range={1,10},Increment=1,CurrentValue=3,Flag="AimSmooth",Callback=function(v) G.AimSmooth=v/10 end})
CombatTab:CreateSlider({Name="FOV Aiming",Range={10,360},Increment=1,CurrentValue=120,Flag="AimFOV",Suffix="°",Callback=function(v) G.AimFOV=v;G.FOVRadius=v end})
CombatTab:CreateDropdown({Name="Alvo (Hitbox)",Options={"Head","Torso","Random"},CurrentOption={"Head"},MultipleOptions=false,Flag="AimPart",Callback=function(opts) G.AimPart=opts[1] or opts end})
CombatTab:CreateToggle({Name="Prediction",CurrentValue=false,Flag="Prediction",Callback=function(v) G.Prediction=v end})
CombatTab:CreateSection("FOV Circle")
CombatTab:CreateToggle({Name="AimFOV Circle",CurrentValue=false,Flag="FOVCircle",Callback=function(v) G.FOVCircle=v end})
CombatTab:CreateSlider({Name="FOV Circle Size",Range={10,500},Increment=1,CurrentValue=120,Flag="FOVRadius",Suffix="px",Callback=function(v) G.FOVRadius=v end})
CombatTab:CreateColorPicker({Name="Cor do FOV Circle",Color=Color3.fromRGB(255,0,0),Flag="FOVColor",Callback=function(v) G.FOVColor=v end})
CombatTab:CreateSection("Extras")
CombatTab:CreateToggle({Name="AimKill",CurrentValue=false,Flag="AimKill",Callback=function(v) G.AimKill=v end})
CombatTab:CreateToggle({Name="TriggerBot",CurrentValue=false,Flag="TriggerBot",Callback=function(v) G.TriggerBot=v end})
CombatTab:CreateToggle({Name="Kill Aura",CurrentValue=false,Flag="KillAura",Callback=function(v) G.KillAura=v end})
CombatTab:CreateSlider({Name="Kill Aura Raio",Range={5,100},Increment=1,CurrentValue=20,Flag="KillAuraRad",Suffix=" studs",Callback=function(v) G.KillAuraRad=v end})

-- VISUALS TAB
local VisualsTab=Window:CreateTab("Visuals",4483362458)
VisualsTab:CreateSection("ESP")
VisualsTab:CreateToggle({Name="Box ESP",CurrentValue=false,Flag="BoxESP",Callback=function(v) G.BoxESP=v end})
VisualsTab:CreateDropdown({Name="Tipo de Box",Options={"2D","3D"},CurrentOption={"2D"},MultipleOptions=false,Flag="BoxType",Callback=function(opts) G.BoxType=opts[1] or opts end})
VisualsTab:CreateToggle({Name="Name ESP",CurrentValue=false,Flag="NameESP",Callback=function(v) G.NameESP=v end})
VisualsTab:CreateToggle({Name="Health Bar",CurrentValue=false,Flag="HealthBar",Callback=function(v) G.HealthBar=v end})
VisualsTab:CreateToggle({Name="Health Text",CurrentValue=false,Flag="HealthText",Callback=function(v) G.HealthText=v end})
VisualsTab:CreateToggle({Name="Distance ESP",CurrentValue=false,Flag="DistESP",Callback=function(v) G.DistESP=v end})
VisualsTab:CreateToggle({Name="Tracers",CurrentValue=false,Flag="Tracers",Callback=function(v) G.Tracers=v end})
VisualsTab:CreateToggle({Name="Head Dot",CurrentValue=false,Flag="HeadDot",Callback=function(v) G.HeadDot=v end})
VisualsTab:CreateToggle({Name="Team Check (Enemy Only)",CurrentValue=false,Flag="TeamCheck",Callback=function(v) G.TeamCheck=v end})
VisualsTab:CreateSection("Configuracoes ESP")
VisualsTab:CreateSlider({Name="Espessura",Range={1,5},Increment=1,CurrentValue=1,Flag="ESPThick",Callback=function(v) G.ESPThick=v end})
VisualsTab:CreateSlider({Name="Tamanho Texto",Range={8,24},Increment=1,CurrentValue=13,Flag="ESPTextSize",Callback=function(v) G.ESPTextSize=v end})
VisualsTab:CreateColorPicker({Name="Cor Inimigo",Color=Color3.fromRGB(255,0,0),Flag="ESPEnemy",Callback=function(v) G.ESPEnemyColor=v end})
VisualsTab:CreateColorPicker({Name="Cor Time",Color=Color3.fromRGB(0,255,0),Flag="ESPTeam",Callback=function(v) G.ESPTeamColor=v end})

-- MOVEMENT TAB
local MovementTab=Window:CreateTab("Movement",4483362458)
MovementTab:CreateSection("SpinBot")
MovementTab:CreateToggle({Name="SpinBot",CurrentValue=false,Flag="SpinBot",Callback=function(v) G.SpinBot=v end})
MovementTab:CreateSlider({Name="SpinBot Velocidade",Range={0,500},Increment=1,CurrentValue=10,Flag="SpinSpeed",Suffix=" vel",Callback=function(v) G.SpinSpeed=v end})
MovementTab:CreateSection("Fly")
MovementTab:CreateToggle({Name="Fly",CurrentValue=false,Flag="Fly",Callback=function(v)
    G.Fly=v
    local c=LocalPlayer.Character; if not c then return end
    local h=c:FindFirstChild("HumanoidRootPart"); if not h then return end
    if v then
        local g=Instance.new("BodyGyro",h); g.Name="FlyBodyGyro"; g.MaxTorque=Vector3.new(1e9,1e9,1e9); g.P=1e4
        local bv=Instance.new("BodyVelocity",h); bv.Name="FlyBodyVelocity"; bv.Velocity=Vector3.zero; bv.MaxForce=Vector3.new(1e9,1e9,1e9)
    else
        local bg=h:FindFirstChild("FlyBodyGyro"); if bg then bg:Destroy() end
        local bv2=h:FindFirstChild("FlyBodyVelocity"); if bv2 then bv2:Destroy() end
    end
end})
MovementTab:CreateSlider({Name="Fly Velocidade",Range={1,500},Increment=1,CurrentValue=50,Flag="FlySpeed",Suffix=" vel",Callback=function(v) G.FlySpeed=v end})
MovementTab:CreateSection("Speed / Jump")
MovementTab:CreateToggle({Name="Speed Hack",CurrentValue=false,Flag="SpeedHack",Callback=function(v) G.SpeedHack=v end})
MovementTab:CreateSlider({Name="Speed Multiplicador",Range={1,100},Increment=1,CurrentValue=1,Flag="SpeedMult",Suffix="x",Callback=function(v)
    G.SpeedMult=v
    if G.SpeedHack then local c=LocalPlayer.Character; if c then local h=c:FindFirstChildOfClass("Humanoid"); if h then h.WalkSpeed=16*v end end end
end})
MovementTab:CreateToggle({Name="Infinite Jump",CurrentValue=false,Flag="InfJump",Callback=function(v) G.InfJump=v end})
MovementTab:CreateToggle({Name="NoClip",CurrentValue=false,Flag="NoClip",Callback=function(v) G.NoClip=v end})
MovementTab:CreateSlider({Name="WalkSpeed",Range={0,250},Increment=1,CurrentValue=16,Flag="WalkSpeed",Suffix=" ws",Callback=function(v)
    G.WalkSpeed=v; local c=LocalPlayer.Character; if c then local h=c:FindFirstChildOfClass("Humanoid"); if h then h.WalkSpeed=v end end
end})
MovementTab:CreateSlider({Name="JumpPower",Range={0,500},Increment=1,CurrentValue=50,Flag="JumpPower",Suffix=" jp",Callback=function(v)
    G.JumpPower=v; local c=LocalPlayer.Character; if c then local h=c:FindFirstChildOfClass("Humanoid"); if h then h.JumpPower=v end end
end})

-- MISC TAB
local MiscTab=Window:CreateTab("Misc",4483362458)
MiscTab:CreateSection("Protecao")
MiscTab:CreateToggle({Name="Anti-Kick",CurrentValue=true,Flag="AntiKick",Callback=function(v)
    G.AntiKick=v
    Window:Notify({Title="CORINGA HUB",Content=v and "Anti-Kick ATIVADO!" or "Anti-Kick desativado.",Duration=3})
end})
MiscTab:CreateToggle({Name="Anti-Ban",CurrentValue=true,Flag="AntiBan",Callback=function(v)
    G.AntiBan=v
    Window:Notify({Title="CORINGA HUB",Content=v and "Anti-Ban ATIVADO!" or "Anti-Ban desativado.",Duration=3})
end})
MiscTab:CreateToggle({Name="God Mode",CurrentValue=false,Flag="GodMode",Callback=function(v)
    G.GodMode=v
    local c=LocalPlayer.Character; if c then local h=c:FindFirstChildOfClass("Humanoid"); if h then h.MaxHealth=v and math.huge or 100; h.Health=v and math.huge or 100 end end
end})
MiscTab:CreateToggle({Name="Anti-AFK",CurrentValue=false,Flag="AntiAFK",Callback=function(v)
    G.AntiAFK=v
    if v then pcall(function() VirtualUser:CaptureController(); VirtualUser:ClickButton2(Vector2.new()) end) end
end})
MiscTab:CreateSection("Utilitarios")
MiscTab:CreateButton({Name="Kill All (Jogadores)",Callback=function()
    for _,p in ipairs(Players:GetPlayers()) do
        if p~=LocalPlayer and p.Character then
            local h=p.Character:FindFirstChildOfClass("Humanoid"); if h then h.Health=0 end
        end
    end
end})
MiscTab:CreateButton({Name="Rejoin",Callback=function()
    pcall(function() TeleportService:Teleport(game.PlaceId,LocalPlayer) end)
end})
MiscTab:CreateSection("Ambiente")
MiscTab:CreateToggle({Name="Remove Fog",CurrentValue=false,Flag="RemoveFog",Callback=function(v)
    G.RemoveFog=v
    if v then Lighting.FogEnd=1e8; Lighting.FogStart=0 else Lighting.FogEnd=100000; Lighting.FogStart=0 end
end})
MiscTab:CreateToggle({Name="Fullbright",CurrentValue=false,Flag="Fullbright",Callback=function(v)
    G.Fullbright=v
    if v then Lighting.Brightness=2; Lighting.GlobalShadows=false; Lighting.Ambient=Color3.fromRGB(178,178,178)
    else Lighting.Brightness=1; Lighting.GlobalShadows=true; Lighting.Ambient=Color3.fromRGB(70,70,70) end
end})
MiscTab:CreateToggle({Name="FPS Counter",CurrentValue=false,Flag="FPSCounter",Callback=function(v) G.FPSCounter=v end})

-- SETTINGS TAB
local SettingsTab=Window:CreateTab("Settings",4483362458)
SettingsTab:CreateSection("Sistema")
SettingsTab:CreateButton({Name="Resetar Configuracoes",Callback=function()
    G.AimBot=false;G.SilentAim=false;G.AimLock=false;G.AimSmooth=0.3;G.AimFOV=120;G.AimPart="Head"
    G.FOVCircle=false;G.AimKill=false;G.TriggerBot=false;G.KillAura=false;G.KillAuraRad=20
    G.BoxESP=false;G.NameESP=false;G.HealthBar=false;G.DistESP=false;G.Tracers=false;G.HeadDot=false
    G.SpinBot=false;G.Fly=false;G.SpeedHack=false;G.InfJump=false;G.NoClip=false
    G.GodMode=false;G.AntiAFK=false;G.AntiKick=true;G.AntiBan=true
    G.FakeLag=false;G.RemoveFog=false;G.Fullbright=false;G.FPSCounter=false
    Window:Notify({Title="CORINGA HUB",Content="Configuracoes resetadas!",Duration=3})
end})
SettingsTab:CreateSection("Creditos")
SettingsTab:CreateLabel("CORINGA HUB - Feito com Rayfield UI")
SettingsTab:CreateLabel("by Joao | Mobile & PC")

-- DRAWING API
local drawingOk=(typeof(Drawing)=="table" or typeof(Drawing)=="userdata")
local function ND(t,p) if not drawingOk then return{Remove=function()end} end; local ok,d=pcall(function() return Drawing.new(t) end); if not ok then return{Remove=function()end} end; for k,v in pairs(p) do pcall(function() d[k]=v end) end; return d end
local function W2V(pos) local vp,on=Camera:WorldToViewportPoint(pos); return Vector2.new(vp.X,vp.Y),vp.Z>0 and on end

-- ESP Objects
local ESPObjects={}
local function IsEnemy(p) return not G.TeamCheck or p.Team~=LocalPlayer.Team end
local function ESPColor(p) return IsEnemy(p) and G.ESPEnemyColor or G.ESPTeamColor end
local function BBox(char)
    local r=char:FindFirstChild("HumanoidRootPart"); if not r then return nil end
    local pos,on=W2V(r.Position); if not on then return nil end
    local ty=W2V(r.Position+Vector3.new(0,3,0)); local by=W2V(r.Position-Vector3.new(0,3,0))
    local h=math.abs(ty.Y-by.Y); local w=h*0.6
    return{X=pos.X-w/2,Y=ty.Y,Width=w,Height=h,Center=pos}
end
local function MkESP(p)
    local o={}
    o.Box=ND("Square",{Visible=false,Filled=false,Color=Color3.fromRGB(255,0,0),Thickness=1,Transparency=1})
    o.Name=ND("Text",{Visible=false,Center=true,Outline=true,Size=13,Font=2,Color=Color3.fromRGB(255,255,255)})
    o.Dist=ND("Text",{Visible=false,Center=true,Outline=true,Size=11,Font=2,Color=Color3.fromRGB(255,255,255)})
    o.HealthBG=ND("Square",{Visible=false,Filled=true,Color=Color3.fromRGB(0,0,0),Transparency=0.6})
    o.HealthFG=ND("Square",{Visible=false,Filled=true,Color=Color3.fromRGB(0,255,0),Transparency=1})
    o.Tracer=ND("Line",{Visible=false,Thickness=1,Color=Color3.fromRGB(255,0,0),Transparency=1})
    o.HeadDot=ND("Circle",{Visible=false,Filled=false,Thickness=1,Color=Color3.fromRGB(255,0,0),Transparency=1,Radius=4})
    ESPObjects[p]=o
end
local function RmESP(p) if ESPObjects[p] then for _,d in pairs(ESPObjects[p]) do pcall(function() d:Remove() end) end; ESPObjects[p]=nil end end
for _,p in ipairs(Players:GetPlayers()) do if p~=LocalPlayer then MkESP(p) end end
Players.PlayerAdded:Connect(MkESP)
Players.PlayerRemoving:Connect(RmESP)

local FOVDraw=ND("Circle",{Visible=false,Filled=false,Thickness=1,Color=Color3.fromRGB(255,0,0),Transparency=1,NumSides=64,Radius=120})
local FPSDraw=ND("Text",{Visible=false,Center=false,Outline=true,Size=14,Font=2,Color=Color3.fromRGB(0,255,0),Position=Vector2.new(10,10),Text="FPS: 0"})

local function GetTarget()
    local best,bd=nil,math.huge
    local vc=Vector2.new(Camera.ViewportSize.X/2,Camera.ViewportSize.Y/2)
    for _,p in ipairs(Players:GetPlayers()) do
        if p==LocalPlayer or not IsEnemy(p) then continue end
        local c=p.Character; if not c then continue end
        local hm=c:FindFirstChildOfClass("Humanoid"); if not hm or hm.Health<=0 then continue end
        local pn=G.AimPart=="Random" and "HumanoidRootPart" or G.AimPart
        local pt=c:FindFirstChild(pn) or c:FindFirstChild("HumanoidRootPart"); if not pt then continue end
        local p2,on=W2V(pt.Position); if not on then continue end
        local d=(p2-vc).Magnitude
        if d<G.AimFOV and d<bd then bd=d; best=p end
    end
    return best
end

local frames,fps,lastT=0,0,0
RunService.RenderStepped:Connect(function()
    frames=frames+1; if tick()-lastT>=1 then fps=frames; frames=0; lastT=tick() end
    pcall(function() FPSDraw.Visible=G.FPSCounter; if G.FPSCounter then FPSDraw.Text="FPS: "..fps end end)
    pcall(function()
        FOVDraw.Visible=G.FOVCircle
        if G.FOVCircle then FOVDraw.Radius=G.FOVRadius; FOVDraw.Color=G.FOVColor; FOVDraw.Position=Vector2.new(Camera.ViewportSize.X/2,Camera.ViewportSize.Y/2) end
    end)
    local tgt=GetTarget()
    if G.AimBot and G.AimLock and tgt and UserInputService:IsMouseButtonPressed(Enum.UserInputType.MouseButton2) then
        local c=tgt.Character; if c then
            local pn=G.AimPart=="Random" and "HumanoidRootPart" or G.AimPart
            local pt=c:FindFirstChild(pn) or c:FindFirstChild("HumanoidRootPart")
            if pt then
                local pred=G.Prediction and ((pt.AssemblyLinearVelocity or Vector3.zero)*0.1) or Vector3.zero
                Camera.CFrame=Camera.CFrame:Lerp(CFrame.lookAt(Camera.CFrame.Position,pt.Position+pred),G.AimSmooth)
            end
        end
    end
    if G.AimKill and tgt then local c=tgt.Character; if c then local h=c:FindFirstChildOfClass("Humanoid"); if h then h.Health=0 end end end
    if G.KillAura then
        local lc=LocalPlayer.Character; local lrp=lc and lc:FindFirstChild("HumanoidRootPart")
        if lrp then
            for _,p in ipairs(Players:GetPlayers()) do
                if p==LocalPlayer or not IsEnemy(p) then continue end
                local c=p.Character; if not c then continue end
                local rp=c:FindFirstChild("HumanoidRootPart"); local hm=c:FindFirstChildOfClass("Humanoid")
                if rp and hm and (lrp.Position-rp.Position).Magnitude<=G.KillAuraRad then hm.Health=0 end
            end
        end
    end
    if G.SpinBot then local lc=LocalPlayer.Character; local h=lc and lc:FindFirstChild("HumanoidRootPart"); if h then h.CFrame=h.CFrame*CFrame.Angles(0,math.rad(G.SpinSpeed),0) end end
    if G.Fly then
        local lc=LocalPlayer.Character; local h=lc and lc:FindFirstChild("HumanoidRootPart")
        local bv=h and h:FindFirstChild("FlyBodyVelocity"); local bg=h and h:FindFirstChild("FlyBodyGyro")
        if bv and bg then
            local d=Vector3.zero
            if UserInputService:IsKeyDown(Enum.KeyCode.W) then d=d+Camera.CFrame.LookVector end
            if UserInputService:IsKeyDown(Enum.KeyCode.S) then d=d-Camera.CFrame.LookVector end
            if UserInputService:IsKeyDown(Enum.KeyCode.A) then d=d-Camera.CFrame.RightVector end
            if UserInputService:IsKeyDown(Enum.KeyCode.D) then d=d+Camera.CFrame.RightVector end
            if UserInputService:IsKeyDown(Enum.KeyCode.Space) then d=d+Vector3.new(0,1,0) end
            if UserInputService:IsKeyDown(Enum.KeyCode.LeftShift) then d=d-Vector3.new(0,1,0) end
            bv.Velocity=d.Magnitude>0 and d.Unit*G.FlySpeed or Vector3.zero; bg.CFrame=Camera.CFrame
        end
    end
    if G.NoClip then local lc=LocalPlayer.Character; if lc then for _,pt in ipairs(lc:GetDescendants()) do if pt:IsA("BasePart") then pt.CanCollide=false end end end end
    -- ESP Render
    local vc2=Vector2.new(Camera.ViewportSize.X/2,Camera.ViewportSize.Y/2)
    for _,p in ipairs(Players:GetPlayers()) do
        if p==LocalPlayer then continue end
        local o=ESPObjects[p]; if not o then continue end
        local c=p.Character; local hm=c and c:FindFirstChildOfClass("Humanoid"); local hrp=c and c:FindFirstChild("HumanoidRootPart")
        local vis=c and hm and hrp and hm.Health>0 and IsEnemy(p)
        local bb=vis and BBox(c) or nil; local col=ESPColor(p)
        pcall(function() o.Box.Visible=G.BoxESP and bb~=nil; if G.BoxESP and bb then o.Box.Position=Vector2.new(bb.X,bb.Y);o.Box.Size=Vector2.new(bb.Width,bb.Height);o.Box.Color=col;o.Box.Thickness=G.ESPThick end end)
        pcall(function() o.Name.Visible=G.NameESP and bb~=nil; if G.NameESP and bb then o.Name.Text=p.Name;o.Name.Size=G.ESPTextSize;o.Name.Position=Vector2.new(bb.Center.X,bb.Y-G.ESPTextSize-2);o.Name.Color=col end end)
        pcall(function()
            o.Dist.Visible=G.DistESP and bb~=nil
            if G.DistESP 
