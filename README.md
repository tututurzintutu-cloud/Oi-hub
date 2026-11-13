--[[
    OiHub / Sintonia payload - statically deobfuscated edition (pass 2)

    Transformations performed without executing the payload:
      * restored indentation and statement layout;
      * decoded printable UTF-8 byte escapes;
      * applied 284 scope-aware, high-confidence binding renames;
      * retained ambiguous generated identifiers to avoid changing behavior.

    The remaining \000 escape is intentional: it represents a NUL byte.
]]

local __oihub_env = (getgenv and getgenv()) or _G
__oihub_env.__OIHUB_LICENSE = {
    user = "1279977271335059456",
    marker = "c34d34acb02ea90a57d123c6",
    build = "c75ce44d9cfddac0",
    expires = 1787957652,
}
((getgenv and getgenv()) or _G).__OIHUB_INTERNAL_TRACE = {
    "t1_c34d34acb02ea90a57d123c6",
    "t2_v3_ac2884fd5a3fe8b91256",
    "t3_46d66071931d1d771309",
}

-- Core UI library plus executor/filesystem compatibility helpers.
do
    local uiEnvironment = (getgenv and getgenv()) or _G
    local previousUiLibrary = uiEnvironment.__OIHUB_UI_V2_LIBRARY
    if previousUiLibrary and type(previousUiLibrary.Unload) == "function" then
        pcall(function()
            previousUiLibrary:Unload()
        end)
    end
    do
        local B = "LPH_NO_VIRTUALIZE"
        local u = (getgenv and getgenv()) or _G
        u[B] = u[B] or function(B)
            return B
        end
    end
    cloneref = cloneref or function(B)
        return B
    end
    gethui = gethui or get_hidden_gui
    getcustomasset = getcustomasset or getsynasset
    getgenv = getgenv or getfenv
    local installStudioFilesystemShim = LPH_NO_VIRTUALIZE(function()
        if (game:GetService("RunService")):IsStudio() then
            local B = (game:GetService("ReplicatedFirst")):FindFirstChild("PRI_WORKSPACE")
                or Instance.new("Folder", game:GetService("ReplicatedFirst"))
            B.Name = "PRI\000."
                .. (
                    tostring(string.char(math.random(50, 120)))
                    .. (
                        tostring(string.char(math.random(50, 120)))
                        .. (
                            tostring(string.char(math.random(50, 120)))
                            .. (
                                tostring(string.char(math.random(50, 120)))
                                .. (
                                    tostring(string.char(math.random(50, 120)))
                                    .. tostring(string.char(math.random(50, 120)))
                                )
                            )
                        )
                    )
                )
            local u = function(B)
                return (string.find(B, "/", 1, true) and string.split(B, "/"))
                    or (string.find(B, "\\", 1, true) and string.split(B, "\\"))
                    or {
                        B,
                    }
            end
            local k = function(k)
                local T = u(k)
                local W = B
                for B, u in next, T do
                    W = W[u]
                end
                return W
            end
            (getgenv()).readfile = function(B)
                local u = k(B)
                return u.Value
            end
            (getgenv()).isfile = function(B)
                local u, T = pcall(function()
                    return k(B)
                end)
                if u and not T:IsA("Folder") then
                    return true
                end
                return false
            end
            (getgenv()).isfolder = function(B)
                local u, T = pcall(function()
                    return k(B)
                end)
                if u and T:IsA("Folder") then
                    return true
                end
                return false
            end
            (getgenv()).writefile = function(k, T)
                local W = u(k)
                local l = B
                for B, u in next, W do
                    local k = l:FindFirstChild(u)
                    if not k then
                        local B = Instance.new("StringValue", l)
                        B.Name = tostring(u)
                        B.Value = T
                    else
                        if k:IsA("StringValue") and tostring(k) == u then
                            k.Name = tostring(u)
                            k.Value = T
                        end
                        l = k
                    end
                end
            end
            (getgenv()).listfiles = function(B)
                local u = k(B)
                local T = {}
                for u, k in next, u:GetChildren() do
                    if k:IsA("StringValue") then
                        table.insert(T, B .. ("/" .. tostring(k)))
                    end
                end
                return T
            end
            (getgenv()).makefolder = function(k)
                local T = u(k)
                local W = B
                for B, u in next, T do
                    local k = W:FindFirstChild(u)
                    if not k then
                        local B = Instance.new("Folder", W)
                        B.Name = tostring(u)
                    else
                        W = k
                    end
                end
            end
            (getgenv()).delfile = function(k)
                local T = u(k)
                local W = B
                for B, u in next, T do
                    local k = W:FindFirstChild(u)
                    if k and k:IsA("StringValue") then
                        k:Destroy()
                    else
                        W = k
                    end
                end
            end
        end
    end)
    installStudioFilesystemShim()
    writefile = writefile or (getgenv()).writefile
    makefolder = makefolder or (getgenv()).makefolder
    readfile = readfile or (getgenv()).readfile
    delfolder = delfolder or (getgenv()).delfolder
    delfile = delfile or (getgenv()).delfile
    listfiles = listfiles or (getgenv()).listfiles
    isfolder = isfolder or (getgenv()).isfolder
    isfile = isfile or (getgenv()).isfile
    local UiLibraryCore = {}
    local function createFontFace(B)
        local u, k = pcall(
            Font.new,
            "rbxasset://LuaPackages/Packages/_Index/BuilderIcons/BuilderIcons/BuilderIcons.json",
            B,
            Enum.FontStyle.Normal
        )
        if u and k then
            return k
        end
        return Font.new("rbxasset://fonts/families/GothamSSm.json", B, Enum.FontStyle.Normal)
    end
    UiLibraryCore.Name = "OiHubUIV2"
    UiLibraryCore.Version = "0.1.0-transition"
    UiLibraryCore.BuiltInRegular = createFontFace(Enum.FontWeight.Regular)
    UiLibraryCore.BuiltInBold = createFontFace(Enum.FontWeight.Bold)
    UiLibraryCore.GlobalSignals = {}
    UiLibraryCore.UnloadEnabled = true
    UiLibraryCore.Windows = {}
    UiLibraryCore.ConfigNamespace = "OiHub/configs/v2"
    UiLibraryCore.__Unloaded = false
    UiLibraryCore.ConfigSupported = type(writefile) == "function"
        and (
            type(readfile) == "function"
            and (
                type(listfiles) == "function"
                and (type(makefolder) == "function" and (type(isfolder) == "function" and type(isfile) == "function"))
            )
        )
    function UiLibraryCore.EnsureFolder(u, B)
        if not UiLibraryCore.ConfigSupported then
            return false
        end
        local k = ""
        for B, u in ipairs(string.split(tostring(B), "/")) do
            if u ~= "" then
                k = k == "" and u or (k .. ("/" .. u))
                if not isfolder(k) then
                    makefolder(k)
                end
            end
        end
        return true
    end
    local cloneReferenceSafe = cloneref or function(B)
        return B
    end
    local TweenServiceUi = cloneReferenceSafe(game:GetService("TweenService"))
    local UserInputServiceUi = cloneReferenceSafe(game:GetService("UserInputService"))
    local TextServiceUi = cloneReferenceSafe(game:GetService("TextService"))
    local RunServiceUi = cloneReferenceSafe(game:GetService("RunService"))
    local PlayersServiceUi = cloneReferenceSafe(game:GetService("Players"))
    local HttpServiceUi = cloneReferenceSafe(game:GetService("HttpService"))
    local localPlayerUi = PlayersServiceUi.LocalPlayer
    local function resolveUiParent()
        for B, u in
            next,
            {
                gethui,
                get_hidden_gui,
            }
        do
            if type(u) == "function" then
                local B, k = pcall(u)
                if B and k then
                    return k
                end
            end
        end
        local B = game:FindFirstChild("CoreGui")
        return (B and cloneReferenceSafe(B)) or localPlayerUi:WaitForChild("PlayerGui")
    end
    local uiParent = resolveUiParent()
    local localMouse = localPlayerUi:GetMouse()
    local currentCameraUi = cloneReferenceSafe(workspace.CurrentCamera)
    local protectGuiSafe = protect_gui
        or protectgui
        or (syn and syn.protect_gui)
        or function(B)
            return B
        end
    local uiScreenGui = Instance.new("ScreenGui")
    local tweenFast = TweenInfo.new(0.1)
    local tweenNormal = TweenInfo.new(0.175)
    local tweenInstant = TweenInfo.new(0.05)
    local tweenQuint = TweenInfo.new(0.5, Enum.EasingStyle.Quint)
    local stringCodec = {}
    UiLibraryCore.UserProfile = PlayersServiceUi:GetUserThumbnailAsync(
        localPlayerUi.UserId,
        Enum.ThumbnailType.HeadShot,
        Enum.ThumbnailSize.Size150x150
    )
    UiLibraryCore.RandomString = LPH_NO_VIRTUALIZE(function()
        return string.rep(string.char(math.random(1, 7)), math.random(1, 4))
            .. (
                string.rep(string.char(math.random(1, 7)), math.random(1, 4))
                .. (
                    string.rep(string.char(math.random(1, 7)), math.random(1, 4))
                    .. (
                        string.rep(string.char(math.random(1, 7)), math.random(1, 4))
                        .. (
                            string.rep(string.char(math.random(1, 7)), math.random(1, 4))
                            .. (
                                string.rep(string.char(math.random(1, 7)), math.random(1, 4))
                                .. (
                                    string.rep(string.char(math.random(1, 7)), math.random(1, 4))
                                    .. (
                                        string.rep(string.char(math.random(1, 7)), math.random(1, 4))
                                        .. (
                                            string.rep(string.char(math.random(1, 7)), math.random(1, 4))
                                            .. (
                                                string.rep(string.char(math.random(1, 7)), math.random(1, 4))
                                                .. string.rep(string.char(math.random(1, 7)), math.random(1, 4))
                                            )
                                        )
                                    )
                                )
                            )
                        )
                    )
                )
            )
    end)
    protectGuiSafe(uiScreenGui)
    local previousScreenGui = uiParent:FindFirstChild("OiHubUIV2")
    if previousScreenGui then
        previousScreenGui:Destroy()
    end
    uiScreenGui.Name = "OiHubUIV2"
    uiScreenGui.IgnoreGuiInset = true
    uiScreenGui.ZIndexBehavior = Enum.ZIndexBehavior.Global
    uiScreenGui.ResetOnSpawn = false
    uiScreenGui.Parent = uiParent

    -- [WINTER UI] Permanent top banner, independent from the menu visibility.
    do
        local previousWinterGui = uiParent:FindFirstChild("WinterCrackOverlay")
        if previousWinterGui then
            previousWinterGui:Destroy()
        end

        local winterCrackGui = Instance.new("ScreenGui")
        winterCrackGui.Name = "WinterCrackOverlay"
        winterCrackGui.IgnoreGuiInset = false
        winterCrackGui.ResetOnSpawn = false
        winterCrackGui.ZIndexBehavior = Enum.ZIndexBehavior.Global
        winterCrackGui.DisplayOrder = 10000
        winterCrackGui.Parent = uiParent
        pcall(protectGuiSafe, winterCrackGui)

        local winterCrackNotice = Instance.new("TextLabel")
        winterCrackNotice.Name = "WinterCrackNotice"
        winterCrackNotice.Parent = winterCrackGui
        winterCrackNotice.BackgroundColor3 = Color3.fromRGB(8, 8, 13)
        winterCrackNotice.BackgroundTransparency = 0.12
        winterCrackNotice.BorderSizePixel = 0
        winterCrackNotice.Position = UDim2.new(0.5, -300, 0, 5)
        winterCrackNotice.Size = UDim2.fromOffset(600, 36)
        winterCrackNotice.ZIndex = 1000
        winterCrackNotice.Font = Enum.Font.GothamBold
        winterCrackNotice.Text = "Crack By Winter discord.gg/winterbypass"
        winterCrackNotice.TextColor3 = Color3.fromRGB(255, 255, 255)
        winterCrackNotice.TextSize = 18
        winterCrackNotice.TextXAlignment = Enum.TextXAlignment.Center
        winterCrackNotice.TextYAlignment = Enum.TextYAlignment.Center
        winterCrackNotice.Visible = true
        local winterCrackStroke = Instance.new("UIStroke")
        winterCrackStroke.Color = Color3.fromRGB(78, 127, 252)
        winterCrackStroke.Transparency = 0.35
        winterCrackStroke.Thickness = 1
        winterCrackStroke.Parent = winterCrackNotice
    end

    UiLibraryCore.Scales = {
        Small = UDim2.fromOffset(560, 430),
        Mobile = UDim2.fromOffset(640, 385),
        Default = UDim2.fromOffset(720, 500),
        Large = UDim2.fromOffset(800, 600),
    }
    UiLibraryCore.IconColor = Color3.fromRGB(255, 255, 255)
    UiLibraryCore.ScreenGui = uiScreenGui
    UiLibraryCore.Flags = {}
    UiLibraryCore.AccentColor = Color3.fromRGB(78, 127, 252)
    UiLibraryCore.MainColor = Color3.fromRGB(8, 8, 13)
    UiLibraryCore.RegisiteryColor = {}
    UiLibraryCore.NameRegisitry = {}
    UiLibraryCore.IsMosueOverOtherFrame = false
    function UiLibraryCore.SetMemberStatusLabel(k, B, u)
        local W = tostring(u or "Never")
        B.RichText = false
        B.Text = W
        local l = B:FindFirstChild("OiMemberNeon")
        local Y = B:FindFirstChild("OiMemberNeonGlow")
        if W ~= "Oi Membro" then
            if l then
                l:Destroy()
            end
            if Y then
                Y:Destroy()
            end
            return
        end
        if l and Y then
            Y.Text = W
            return
        end
        l = Instance.new("UIGradient")
        l.Name = "OiMemberNeon"
        l.Color = ColorSequence.new({
            ColorSequenceKeypoint.new(0, Color3.fromRGB(75, 245, 255)),
            ColorSequenceKeypoint.new(0.35, Color3.fromRGB(40, 145, 255)),
            ColorSequenceKeypoint.new(0.68, Color3.fromRGB(195, 95, 255)),
            ColorSequenceKeypoint.new(1, Color3.fromRGB(75, 245, 255)),
        })
        l.Offset = Vector2.new(-1, 0)
        l.Parent = B
        Y = Instance.new("TextLabel")
        Y.Name = "OiMemberNeonGlow"
        Y.Parent = B
        Y.BackgroundTransparency = 1
        Y.BorderSizePixel = 0
        Y.Position = UDim2.fromOffset(0, 0)
        Y.Size = UDim2.fromScale(1, 1)
        Y.ZIndex = math.max(1, B.ZIndex - 1)
        Y.FontFace = B.FontFace
        Y.Text = W
        Y.TextColor3 = Color3.fromRGB(255, 255, 255)
        Y.TextSize = B.TextSize
        Y.TextTransparency = 0.72
        Y.TextStrokeTransparency = 1
        Y.TextXAlignment = B.TextXAlignment
        Y.TextYAlignment = B.TextYAlignment
        Y.TextTruncate = B.TextTruncate
        Y.TextWrapped = B.TextWrapped
        local q = l:Clone()
        q.Name = "GlowGradient"
        q.Parent = Y
        local x = TweenInfo.new(1.75, Enum.EasingStyle.Linear, Enum.EasingDirection.InOut, -1, true)
        local o = TweenServiceUi:Create(l, x, {
            Offset = Vector2.new(1, 0),
        })
        local i = TweenServiceUi:Create(q, x, {
            Offset = Vector2.new(1, 0),
        })
        local E =
            TweenServiceUi:Create(Y, TweenInfo.new(1.05, Enum.EasingStyle.Sine, Enum.EasingDirection.InOut, -1, true), {
                TextTransparency = 0.48,
            })
        o:Play()
        i:Play()
        E:Play()
        UiLibraryCore:AddSignal(B.Destroying:Connect(function()
            pcall(function()
                o:Cancel()
            end)
            pcall(function()
                i:Cancel()
            end)
            pcall(function()
                E:Cancel()
            end)
        end))
    end
    function UiLibraryCore.ApplyVersionNeon(u, B)
        local k = Instance.new("UIGradient")
        k.Name = "OiVersionNeon"
        k.Color = ColorSequence.new({
            ColorSequenceKeypoint.new(0, Color3.fromRGB(35, 105, 255)),
            ColorSequenceKeypoint.new(0.24, Color3.fromRGB(55, 255, 238)),
            ColorSequenceKeypoint.new(0.52, Color3.fromRGB(205, 255, 255)),
            ColorSequenceKeypoint.new(0.78, Color3.fromRGB(0, 205, 255)),
            ColorSequenceKeypoint.new(1, Color3.fromRGB(35, 105, 255)),
        })
        k.Transparency = NumberSequence.new({
            NumberSequenceKeypoint.new(0, 0.22),
            NumberSequenceKeypoint.new(0.3, 0),
            NumberSequenceKeypoint.new(0.62, 0.1),
            NumberSequenceKeypoint.new(1, 0.22),
        })
        k.Rotation = -180
        k.Parent = B
        local W = TweenServiceUi:Create(
            k,
            TweenInfo.new(6.2, Enum.EasingStyle.Linear, Enum.EasingDirection.InOut, -1, false),
            {
                Rotation = 180,
            }
        )
        W:Play()
        UiLibraryCore:AddSignal(B.Destroying:Connect(function()
            pcall(function()
                W:Cancel()
            end)
        end))
    end
    UiLibraryCore.GlobalLogo = "rbxassetid://120358385035996"
    UiLibraryCore.ImageColorMapping = "rbxassetid://4155801252"
    if getcustomasset and (writefile and (makefolder and (isfolder and isfile))) then
        local B =
            "https://raw.githubusercontent.com/4lpaca-pin/NeverLose/2308c101331855d7ca09fecc1a1db814d489aaf4/assets/%s"
        local u = "OiHubV2Assets"
        if not isfolder(u) then
            makefolder(u)
        end
        pcall(function()
            if not isfile(u .. "/logo.png") then
                local k = game:HttpGet(string.format(B, "logo.png"))
                writefile(u .. "/logo.png", k)
                task.wait()
            end
            if isfile(u .. "/logo.png") then
                UiLibraryCore.GlobalLogo = getcustomasset(u .. "/logo.png")
            end
        end)
        pcall(function()
            if not isfile(u .. "/saturation_value_gradient.png") then
                local k = game:HttpGet(string.format(B, "saturation_value_gradient.png"))
                writefile(u .. "/saturation_value_gradient.png", k)
                task.wait()
            end
            if isfile(u .. "/saturation_value_gradient.png") then
                UiLibraryCore.ImageColorMapping = getcustomasset(u .. "/saturation_value_gradient.png")
            end
        end)
    end
    local function downloadAsset(u)
        local k = (syn and syn.request)
            or (http and http.request)
            or uiEnvironment.http_request
            or uiEnvironment.request
            or (fluxus and fluxus.request)
        if type(k) == "function" then
            local B, T = pcall(k, {
                Url = u,
                Method = "GET",
                Headers = {
                    ["User-Agent"] = "Mozilla/5.0",
                },
            })
            if B and type(T) == "table" then
                local B = T.StatusCode or T.status_code or 200
                local u = T.Body or T.body
                if type(u) == "string" and (u ~= "" and (B >= 200 and B < 400)) then
                    return u
                end
            end
        end
        local T, W = pcall(function()
            return game:HttpGet(u)
        end)
        if T and (type(W) == "string" and W ~= "") then
            return W
        end
        return nil
    end
    function UiLibraryCore.ResolveImage(T, B, u, k)
        if type(B) ~= "string" or B == "" then
            return k or ""
        end
        if not string.match(B, "^https?://") then
            return B
        end
        if
            type(getcustomasset) ~= "function"
            or type(writefile) ~= "function"
            or type(isfile) ~= "function"
            or type(makefolder) ~= "function"
            or type(isfolder) ~= "function"
        then
            return k or ""
        end
        local W = "OiHubV2Assets"
        local l = W .. ("/" .. tostring(u or "image.png"))
        local I, Y = pcall(function()
            if not isfolder(W) then
                makefolder(W)
            end
            local u = false
            if isfile(l) and type(readfile) == "function" then
                local B, k = pcall(readfile, l)
                u = B and (type(k) == "string" and #k > 0)
            end
            if not u then
                local u = downloadAsset(B)
                if not u then
                    return k or ""
                end
                writefile(l, u)
            end
            return getcustomasset(l)
        end)
        return (I and Y) or k or ""
    end
    UiLibraryCore.OiHubWordmark =
        UiLibraryCore:ResolveImage("https://i.imgur.com/4mGyRZy.png", "oihub-wordmark-v2.png", "")
    -- [WINTER UI] Embedded Winter wordmark from the supplied logo.
    do
        local winterLogoPath = "OiHubV2Assets/WinterWordmark.png"
        local winterLogoData = "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KFRVVWUymUQinsvlq6qqoFisaUXP8yslTEGvo5RYyYppmaCBHQgELNNiWcb1PddxMUKu5wpAtWMYOiCA3kZKbqazKgqFQmNj48KFC0ePHt3f359IJAYGBs47/7yln37KcTyQ47fToq6trbUsS9d1WVauuvKK8y84f9iwZss0RUlq27L5z3fe+dprr2ez2Xg8TonhNAuDj+zv76+pqamurv7FL35x3HHHjRo1qnKax39vXBF87fb29pNPPqmnpxc4zZB5wUEKBoNnnH7G6jWrP/rwo1A4VGLBOjZG+C9/+ctJJ5001IIUECsefvjhP/7x9lgsCgo41M4Cmm4Yhm3bjgN5u27b9uLFiydMmDAkSwfv2dXVdeyxx4JWQ31d3fwXX2xsbPxpGvJT/I/qmXMc993338865hiYEQZcWBp4g9GhBE461wluI5B6YrHY4OAgbHTaq0xjcjAoqVQqGom6nkvzX1r3pLEDhBIQaNCxZflcLhyJBINBhmE6OzsVRXE9z3McjBndMDiOyecLo0ePfuqpp0aMGDGk/Au84E033XTXXXeBDjREoxSVhyxMFEXohIDYMBQKQQDS09MjyXI0EmlsbAyGgrZtVyWqPM+bvP/ktWvWHnTQQd3d3WPGjM1mMnX1db5PJFlUZIXlWIEX4FsDGxPyFL/cAu6XR7999dVXr7/++uDg4Nq1a0VRDIfDpmnS1kIKYpbCedO8+693T58+QzcMWRId1+U53nFtluNRSfYQ0QEBfLmjjbbFPfzww3fffTd0KUNZKZfL+b4/fvx4URSPOfqYQw87LBaPhYJByEYpgEthXGCTh0Kh777//uSTTvJL+CP2PB8RgssYEFifES0tb7399vDhw5PJVG1NzYaNG0444YR169bFYrEdTA/LcZ7r4kgkUm6iZQYHB8aPH3/uueddftllIMbM8/x777334IMPfvDBBwE1IEoiNd5AroGDB83HQL2/4IILTjvttHHjxlXWgP8blgiOxLJly4477jiQ7KU4HGQZEyZMqK+rX7RokSAKrusqiooQcT33sEMP24WQBPgEPM//+je/XfDSP0DPlM4DcF23rq6uUChEo9GtW7fCSmuaduysY//+5N+Hegfgq910081PPDGvpqamr6/vkksu+f3vfw+GA2KKnURkL7n00nfefhsOG+SMtHRAJTXp5AmI8IFLASGM7/vQKgnHCc4tOB46LpHinRACQP5i2zbGDMaIyrACvBKNRkEzCIRp0un0Pvvs09fXBwxVTdNs29YNg+c4x3GKxeLxxx8/f/78IbkKuDO6rv/1nnug5ATlXUVRTMsKlHWmMMbffPPNsmXLampq6MVAT/Kc2bNHjBo1dsyYqVOn/vc8qGmYL/zvC48++mg+n3NdDxot6Z2Hu9TX13fkzCMXvLxg1z5iYGDgnHPOWbFiBXSuZLNZnuePOuroadOmHnvssTU1NTv/VuvWrZs7dy7UjkE0FmPEYEzKoYCu6xMmTFiwYEF9Q306la6qqvrmm6/PPPOsnp4eKOBSYK6EoPs+AuYVbQwDD5bP5/fae+9f/epXp592mizLrusZhvH88889+uijHR0dDMPEYjHA23zPxxgjjKgch+/7kMxPmTLltttua2hoqESL/xsGaNHiRXNmzx42rBnIijQaRAg1NjQODg7YjgPFODgJnustXLhw9JjRu5AJwifefPMtDz30IHQkUS1tYBU1NDSA3QFvlkqm3nv/vQMPPHBIXx9evHHjxjlz5kBDUCKRWLhw4c8ys0HRmWFKyQK0U7/11lvfffedLMu9vb1r1qyBYhxYH0pfpnET7aiAoiH4FaCnA24CoQdwf8DgQiEJrE8gEAD2FtTXIpEItDtIoiQrcn19Q01NtWlaNTXVn33+uee6/f39juOAiDioFIEL7O7uNgxj6tRpTU2NsXi0UNCaGhsZhtlvv/1aWlqGCgDt5OGcO3duOp0G/l4mkxk7duzFF188fvz4iRMnVsaS//p9duHCqIQFQqizs/OUU07u6OiEGiKsBcBGruPohvG/L/zvjCNmDCkApGXrxYsXz549G251OBy+4IILZ806Zt99963E6f/19UMcvWLFiqOPOcYr01x8nwDQTnd1oVA44ogjHn3ssUAg4FhWMBR69tlnb775ZsijK5Fs+EljKxyLRUFcGZJ/6DQGDvSIESMeeuihww47TNd1jFEqlX7yySfnz5+/cePG2tpaCsLTciDMJ4GmAbBzxx577KmnnnrEEUeA+6LR/n9qdpLne5deetlbb77JsizgGgD0lCBthBmWoZ5cUZR0Oj3/f+dPnzF91wwi/NXll1/+8ssvR6Mx0zQ4jgdFbUANbdtuaGjYtGkTdIT+9re/vfHGG4eKPQOP4ZeXXPL+woX19fWDg4MHHnjg3XffvXXr1lwu5zj2F198LgiCKMqqqqxZs7a3ryebydq2A/QIaDsKBIP9vb1A24eeI2pcqJx4JZSTy+VA1ALif7Cq4VDI8/14PC5JEsY4m83S4ZQQ9Hmum8lmGxobJk6Y2NLSEgioJ5540m677VaeR0KYCuE3CL4HB/rv+svd33zzTW9vLxyM8ePH+76/evXqiRMnPvDAg01Njf9BF7VNha5CrdnzPIzQeeef/9lnn6mq6vu+YRh/+MMfzj///Mo6SeXIrf/Gg06S6OjoOOqoI03TggIltDh4nuf5/vDm4Z988jEVYxsSQIEQuuGGG+67796q6upDDj7kT3/60/CW4UPKS2DDr1q1aubMmZqmRSJRhEg+n+d5znFcaoAKhcLJJ53017/9jWUYQlAoGLzjT3fcc889oFhAJaLQjrNYEcuyuLq6BiRgKf+QynFAs/+pp536299e29TQaJqmGlB//PHHlxYsePyxx+BcQfgNQB0UvMHTwk5Np1OCIB540IEXnH/+CSecWKG69u8mZaABblrmvnvvM5hMAu5TOfuYEh+gDhUOh3t6es4444xHHnlk1y4AYqu2trbzzj9va/tWaN/FGMuyUijkoUALPKBisZjP55ubmz/77DOIGoZaev/www/POeec6upqgCSKxWJtXV1vTw9I00PKAHihGlCJTxBGkPQxDANpIHSoFfL5YCgEWjOAy4CoANUMA+AMYzxy5MixY8futttukKqAvj2wKGDeyV//+tfFixeDpifkYoFA4Mijjjr66KMSicTk/Sb/67O6w0348KMPf/e73zm2Ayr0DMOsW7fu3HPPpWlmJbb1nx03CofqgQce/Mtf7goGg5quTznooKuvuWby5MmVzT3o/60HhDx33XXXzTffPHLkyGw2qwYClmlKstzV2fn4Y4+d84tfDNVfwt3u6upqGTFi3733vummm2cdOwtVjE3fmQ0JR/i77747+uijQR1RlmVItSoNtKZpF//q4ht/d4PjuhzPi4J44403zJs3D8g3dNYYjVTg6FE9HxyNRuEfqEgHsJWoqmMul6tvqL/wggsvv/xyluUwgzzX6+zouO+B+z/5+JOenp54PA7mVhRFz/MZho6ssGVZti3bMA2M8YQJEy6//PKZR86MhCOVio277D0wxhs3bpwxYwYAqFS4hE7joU1hQAtKJBIffvBhbV3trvHZ4IJXrlx53HHHhUIhSGpkWclmM4IggNycbdsjR47s7u7u6el57LHHzjrrrKF+FnAojj/++E2bNtF+dNC6BbgUNENgIA+QwaDkD7xkRVGqqqocx4lGo52dndlsFr4+oDOapl144YUzZkyPxWJVVdWQeoAjAgT9n11VsVicNWtWb29vW1vb+HHja2trr7v+ukmTJoFtqsR9/0/LTt3+rbfd+srLrzAMM3z48GAotOqHH75avhys4X+7cLFx48aTTj7ZMkzHdaZOnfrYo4/xAj/UisR/6gHu8NVXX7n66msIIaDlBF0vkydPfuWVVwKBwC5QEDDGc+fOHT58+O1/vD0QDLguTBvGQzLT69atmz59OlA3wESIopTLZctirK5h6FdcccXNt9yczWRlWcEYXXPNNfPnz49Go375AXeVJky0oAHPcJT+SA0VWCla1a+qqsqkM3feeef777//u99dP336DI+4NVXVDz3wwDcrVjz2yKMffvQRjKAihIiiAPCQ65Zkj2m71urVqy+66KK99t7rF+f84swzzhBE8d8xQ3B5jz76KKAwEAFROjmlZsHxA8jjlxf/cpetD3XIff19QCfP5XKBgJpKZcAJQH0HZhDpunbIIYfMmjWrkiG686v+j5deWrduHbSY5vN5lmXb29uDwWBHR0c4HE4mB8eP3y0cDre0tMAcV2iUq6qqMk3zoIMOEkUR2ACPPPLIH//4x7q6OsDmi8XizJkzb7/99v87Z6EmHmHbsSVJeuihh7744otJkybdfffdB+x/wAEHHlB5fiD+3cnTC6k6IeTqq65e8NJLINa5ZfPma3/7W8AQ/3sGCByyaZq33nrr4MBAoioxqnHUgw8++P+h9aGI7MgRIxVFdhzXNE1JLJUO99lnn2AwONRrgwhO1/XLL7t8xhEzyoeFHZJN5Dhu9erVxxxzDBUngrVTVZauka7rl176q5tuvimTzoDdvPrqqxcuXBiPx6mYLORSECuVadbboBvf9zlangSpISrJSkc4Azbp+/6aNWvOOeecE+eecOWVV4wdPz6fL4wfN/7Bhx768MP3nnzq6W+/WQkkDjoQhtZcqGgWx3Grflx1zTXXPDFv3lVXX33iiSfu8qrDLQDJGKhiUhVIoCZDUSagBlzPNQzjkEMOuezyy/79/a0qqiAIuVwBY9zX109Jj1AGyuVyzc3NnZ2dZ599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FAqFw0CqNk1TUVWwbq7rQk13F84YQmjVqlVAsadC9GDdIMKC+X+33HwLL/D/QWoJCNzBlFGoedEBZLlc7vprfytJ8i6cnFQq9eyzz9bX12ez2aqqqu7u7om7Tzxm1jFDVSAFbOjOu+6srqp2HRfkmfr6+s4/73zoCN81+iVsIyqYC4561wBXChC8/PLLG1tboX23ubk5k8nEYjHf96uqqv7nf/4H7RLv7s477+zp6eE4rlAoYIQN05BlOZ/LA+oME4F4nt9r77122203mBc8pGv++9//3tHRMXbMWJAogW2gaZooSul0uqqqataxx6IhqrtgjN999922tjZRFCGISCQSJ5xwwr8fq+6wcEA+JgiBIDoIOVAOlyAIK1eunDdvXiwWgyCOfmsQI2RZ9pabf99Q3yjwou96N99086LFi4HrTOVoKqFJOtaZKgLDnoGpkHAx8DIOfDhmsOd6dDKZ4zp0JlTluBuEkOM6Ai94rocQyqQz0GgeiUTAxBiG0dDQsHLFSiAuuj7RNK2zs5NOWTR0HXTmJ06cCAZo13r5XvjfF8Cy0j4suiGCoWBPd89VV14194S5/1mCma7rMPsYJtyDvoHneYah77333odPmzakuAC8k+/7jzzyCESIIOASjUanTp22M42CPz2EX3715YYNGzCDiV+alTpq1Kg9J+25C/UUyuAoSR1+9x0hpLW1NZ/PBwKBww47DArzQ31PjuP6+/vfXbiwoaFhcHCwurq6s6vzoAMP2rp1q6Zpx82ZQwecDymEzGSyixYtoswPII6Ypgnau1CZDQaDmUzmjNPPIITswul+7733EMJ9fX2qqubzeVBWAqhREITddpsArfM7r1SJENI0bd68eRRgTafTN954M9CId60mWFkxAK21L774wvO8T5cuXblipSSJG1tb47F4NpNpGdGSy+UPO+yQ5ubhI0eOvPbaa4GxUSm5U6oXFQtzTzhxjz33JIgIAn/HHXe89eab0VgMAisGMxhhOp2Zmh46J75StwysGx2s6jgOV9avdcDuUODA8z2O5ejwVoS2tby7rstyLIOYR555RAko1197ff9gP2joNjc3d3V1iaI4csQIRIgqy5lstrOrixAEZFxRknzPsyzroIMOgjx8F3hWGOPu7m6IfWirAci1BYKBXDY3ftz4q66+6j9S2a18wKCrcuVLA+wZIPnzzz+/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"
        pcall(function()
            if type(makefolder) == "function" and type(isfolder) == "function" and not isfolder("OiHubV2Assets") then
                makefolder("OiHubV2Assets")
            end
            if type(writefile) == "function" and type(isfile) == "function" and type(getcustomasset) == "function" then
                if not isfile(winterLogoPath) then
                    writefile(winterLogoPath, UiLibraryCore.Base64Decode(winterLogoData))
                end
                if isfile(winterLogoPath) then
                    UiLibraryCore.WinterLogo = getcustomasset(winterLogoPath)
                end
            end
        end)
        if UiLibraryCore.WinterLogo and UiLibraryCore.WinterLogo ~= "" then
            UiLibraryCore.OiHubWordmark = UiLibraryCore.WinterLogo
        end
    end
    UiLibraryCore.OiHubIcon =
        UiLibraryCore:ResolveImage("https://i.imgur.com/gwkbuF6.png", "oihub-icon-v2.png", UiLibraryCore.GlobalLogo)
    UiLibraryCore.ESPPreviewAvatar = UiLibraryCore:ResolveImage(
        "https://tr.rbxcdn.com/30DAY-Avatar-A1F92E1796A78BF86A136E342AB7B3B1-Png/352/352/Avatar/Webp/noFilter",
        "esp-preview-avatar.webp",
        ""
    )
    function UiLibraryCore.AddSignal(u, B)
        if UiLibraryCore.UnloadEnabled then
            table.insert(UiLibraryCore.GlobalSignals, B)
        end
        return B
    end
    function UiLibraryCore.AddQuery(k, B, u)
        table.insert(UiLibraryCore.NameRegisitry, {
            Root = B,
            Idx = u,
        })
    end
    function stringCodec.new(B)
        local u = {}
        local k = ((#B + 3782) % 111) + 1
        string.gsub(
            B,
            ".",
            LPH_NO_VIRTUALIZE(function(B)
                table.insert(u, tostring(B:byte() + k))
            end)
        )
        local T = table.concat(u, "?")
        table.clear(u)
        return "{" .. (tostring(k + 72667) .. ("}?" .. T))
    end
    function stringCodec.reverse(B)
        local u = string.split(B, "?")
        local k = (u[1]:gsub("{", "")):gsub("}", "")
        local T = tonumber(k)
        local W = {}
        local l = T - 72667
        for B, u in next, u do
            if B > 1 then
                local B = tonumber(u)
                table.insert(W, string.char(B - l))
            end
        end
        local I = table.concat(W)
        table.clear(W)
        return I
    end
    do
        local B = "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789+/"
        UiLibraryCore.Base64Encode = LPH_NO_VIRTUALIZE(function(u)
            return (
                (u:gsub(".", function(B)
                    local u, k = "", B:byte()
                    for B = 8, 1, -1 do
                        u = u .. (k % 2 ^ B - k % 2 ^ (B - 1) > 0 and "1" or "0")
                    end
                    return u
                end) .. "0000"):gsub("%d%d%d?%d?%d?%d?", function(u)
                    if #u < 6 then
                        return ""
                    end
                    local k = 0
                    for B = 1, 6, 1 do
                        k = k + (u:sub(B, B) == "1" and 2 ^ (6 - B) or 0)
                    end
                    return B:sub(k + 1, k + 1)
                end)
                .. ({
                    "",
                    "==",
                    "=",
                })[#u % 3 + 1]
            )
        end)
        UiLibraryCore.Base64Decode = LPH_NO_VIRTUALIZE(function(u)
            u = string.gsub(u, "[^" .. (B .. "=]"), "")
            return (
                (u:gsub(".", function(u)
                    if u == "=" then
                        return ""
                    end
                    local k, T = "", (B:find(u) - 1)
                    for B = 6, 1, -1 do
                        k = k .. (T % 2 ^ B - T % 2 ^ (B - 1) > 0 and "1" or "0")
                    end
                    return k
                end)):gsub("%d%d%d?%d?%d?%d?%d?%d?", function(B)
                    if #B ~= 8 then
                        return ""
                    end
                    local u = 0
                    for k = 1, 8, 1 do
                        u = u + (B:sub(k, k) == "1" and 2 ^ (8 - k) or 0)
                    end
                    return string.char(u)
                end)
            )
        end)
    end
    UiLibraryCore.LoadIcon = LPH_NO_VIRTUALIZE(function()
        UiLibraryCore.RobloxIcon = {
            ["3d-cube-arrow-left"] = "3d-cube-arrow-left",
            amazon = "amazon",
            ["arm-left"] = "arm-left",
            ["arm-right"] = "arm-right",
            ["arrow-curl-to-left"] = "arrow-curl-to-left",
            ["arrow-curl-to-right"] = "arrow-curl-to-right",
            ["arrow-down-to-line"] = "arrow-down-to-line",
            ["arrow-large-down"] = "arrow-large-down",
            ["arrow-large-left"] = "arrow-large-left",
            ["arrow-large-right"] = "arrow-large-right",
            ["arrow-large-up"] = "arrow-large-up",
            ["arrow-right-from-portrait-rectangle"] = "arrow-right-from-portrait-rectangle",
            ["arrow-right-to-portrait-rectangle"] = "arrow-right-to-portrait-rectangle",
            ["arrow-rotate-down-dashed"] = "arrow-rotate-down-dashed",
            ["arrow-rotate-right"] = "arrow-rotate-right",
            ["arrow-rotate-right-dashed"] = "arrow-rotate-right-dashed",
            ["arrow-small-down"] = "arrow-small-down",
            ["arrow-small-left"] = "arrow-small-left",
            ["arrow-small-right"] = "arrow-small-right",
            ["arrow-small-up"] = "arrow-small-up",
            ["arrow-spin-clockwise"] = "arrow-spin-clockwise",
            ["arrow-spin-clockwise-10"] = "arrow-spin-clockwise-10",
            ["arrow-spin-clockwise-15"] = "arrow-spin-clockwise-15",
            ["arrow-spin-clockwise-30"] = "arrow-spin-clockwise-30",
            ["arrow-spin-counter-clockwise-10"] = "arrow-spin-counter-clockwise-10",
            ["arrow-spin-counter-clockwise-15"] = "arrow-spin-counter-clockwise-15",
            ["arrow-spin-counter-clockwise-30"] = "arrow-spin-counter-clockwise-30",
            ["arrow-thick-to-left"] = "arrow-thick-to-left",
            ["arrow-thick-to-right"] = "arrow-thick-to-right",
            ["arrow-up-from-landscape-rectangle"] = "arrow-up-from-landscape-rectangle",
            ["arrow-up-right-from-square"] = "arrow-up-right-from-square",
            ["arrow-wide-short-down"] = "arrow-wide-short-down",
            ["arrow-wide-short-left"] = "arrow-wide-short-left",
            ["arrow-wide-short-right"] = "arrow-wide-short-right",
            ["arrow-wide-short-up"] = "arrow-wide-short-up",
            ["arrows-small-directional"] = "arrows-small-directional",
            ["audio-wave-dotted-line"] = "audio-wave-dotted-line",
            backpack = "backpack",
            beard = "beard",
            bell = "bell",
            ["bell-clock"] = "bell-clock",
            ["bell-plus"] = "bell-plus",
            ["bell-slash"] = "bell-slash",
            belt = "belt",
            binoculars = "binoculars",
            ["book-closed"] = "book-closed",
            bookmark = "bookmark",
            ["bow-tie"] = "bow-tie",
            ["building-store"] = "building-store",
            ["bullet-flying"] = "bullet-flying",
            ["butterfly-wings"] = "butterfly-wings",
            calendar = "calendar",
            ["calendar-plus"] = "calendar-plus",
            ["calendar-star"] = "calendar-star",
            ["camera-small"] = "camera-small",
            ["caret-small-down"] = "caret-small-down",
            ["caret-small-left"] = "caret-small-left",
            ["caret-small-right"] = "caret-small-right",
            ["caret-small-up"] = "caret-small-up",
            ["chain-link"] = "chain-link",
            ["chart-four-vertical-bars"] = "chart-four-vertical-bars",
            ["chart-line"] = "chart-line",
            ["chart-pie"] = "chart-pie",
            ["chart-scatter-plot"] = "chart-scatter-plot",
            ["chart-three-vertical-bars"] = "chart-three-vertical-bars",
            check = "check",
            ["check-large"] = "check-large",
            ["check-small"] = "check-small",
            ["chevron-large-down"] = "chevron-large-down",
            ["chevron-large-down-to-line"] = "chevron-large-down-to-line",
            ["chevron-large-left"] = "chevron-large-left",
            ["chevron-large-left-to-line"] = "chevron-large-left-to-line",
            ["chevron-large-right"] = "chevron-large-right",
            ["chevron-large-right-to-line"] = "chevron-large-right-to-line",
            ["chevron-large-up"] = "chevron-large-up",
            ["chevron-large-up-to-line"] = "chevron-large-up-to-line",
            ["chevron-small-down"] = "chevron-small-down",
            ["chevron-small-down-to-line"] = "chevron-small-down-to-line",
            ["chevron-small-left"] = "chevron-small-left",
            ["chevron-small-left-to-line"] = "chevron-small-left-to-line",
            ["chevron-small-right"] = "chevron-small-right",
            ["chevron-small-right-to-line"] = "chevron-small-right-to-line",
            ["chevron-small-up"] = "chevron-small-up",
            ["chevron-small-up-to-line"] = "chevron-small-up-to-line",
            ["circle-check"] = "circle-check",
            ["circle-i"] = "circle-i",
            ["circle-minus"] = "circle-minus",
            ["circle-person"] = "circle-person",
            ["circle-person-three-horizontal-bars-wrapping-right"] = "circle-person-three-horizontal-bars-wrapping-right",
            ["circle-play"] = "circle-play",
            ["circle-plus"] = "circle-plus",
            ["circle-question"] = "circle-question",
            ["circle-slash"] = "circle-slash",
            ["circle-star"] = "circle-star",
            ["circle-three-dots-horizontal"] = "circle-three-dots-horizontal",
            ["circle-three-dots-vertical"] = "circle-three-dots-vertical",
            ["circle-x"] = "circle-x",
            clock = "clock",
            ["clock-dashed"] = "clock-dashed",
            ["clock-spin-reverse"] = "clock-spin-reverse",
            ["clock-spin-reverse-dashed"] = "clock-spin-reverse-dashed",
            ["clothes-hanger"] = "clothes-hanger",
            cloud = "cloud",
            ["cloud-arrow-down"] = "cloud-arrow-down",
            code = "code",
            ["compact-makeup-brush"] = "compact-makeup-brush",
            compass = "compass",
            ["controller-with-cog"] = "controller-with-cog",
            crop = "crop",
            crosshairs = "crosshairs",
            ["crosshairs-slash"] = "crosshairs-slash",
            ["cube-vertexes"] = "cube-vertexes",
            ["curved-rectangle-megaphone"] = "curved-rectangle-megaphone",
            ["diagonal-line-pattern"] = "diagonal-line-pattern",
            ["diagonal-line-pattern-sticker"] = "diagonal-line-pattern-sticker",
            ["diamond-simplified"] = "diamond-simplified",
            discord = "discord",
            ["disguise-nose-glasses"] = "disguise-nose-glasses",
            ["document-circle-slash"] = "document-circle-slash",
            ["document-list-heart"] = "document-list-heart",
            ["door-open-arrow-to-bottom-right"] = "door-open-arrow-to-bottom-right",
            dress = "dress",
            ["dual-arrows-horizontal"] = "dual-arrows-horizontal",
            ["dual-arrows-to-corners"] = "dual-arrows-to-corners",
            ["dual-arrows-vertical"] = "dual-arrows-vertical",
            envelope = "envelope",
            eraser = "eraser",
            eye = "eye",
            ["eye-slash"] = "eye-slash",
            ["eye-with-eyeliner"] = "eye-with-eyeliner",
            eyebrows = "eyebrows",
            eyelashes = "eyelashes",
            ["face-winking"] = "face-winking",
            facebook = "facebook",
            ["file-box"] = "file-box",
            fingerprint = "fingerprint",
            flag = "flag",
            flame = "flame",
            folder = "folder",
            ["fountain-pen-nib"] = "fountain-pen-nib",
            ["four-bars-horizontal-center-aligned"] = "four-bars-horizontal-center-aligned",
            ["four-bars-horizontal-chevron-left"] = "four-bars-horizontal-chevron-left",
            ["four-bars-horizontal-chevron-right"] = "four-bars-horizontal-chevron-right",
            ["four-bars-horizontal-justified-aligned"] = "four-bars-horizontal-justified-aligned",
            ["four-bars-horizontal-left-aligned"] = "four-bars-horizontal-left-aligned",
            ["four-bars-horizontal-right-aligned"] = "four-bars-horizontal-right-aligned",
            ["frame-bubble-slash"] = "frame-bubble-slash",
            ["frame-bubble-soundwave"] = "frame-bubble-soundwave",
            ["frame-camera"] = "frame-camera",
            ["frame-camera-center"] = "frame-camera-center",
            ["frame-collapsed"] = "frame-collapsed",
            ["frame-corners"] = "frame-corners",
            ["frame-expanded"] = "frame-expanded",
            ["frame-face"] = "frame-face",
            ["frame-person-torso"] = "frame-person-torso",
            ["frame-record"] = "frame-record",
            ["frame-single-bar-horizontal"] = "frame-single-bar-horizontal",
            ["frame-soundwave"] = "frame-soundwave",
            ["frame-video-camera"] = "frame-video-camera",
            gear = "gear",
            ["generic-dpad"] = "generic-dpad",
            ["gift-box"] = "gift-box",
            ["gift-card"] = "gift-card",
            glasses = "glasses",
            ["globe-detailed"] = "globe-detailed",
            ["globe-simplified"] = "globe-simplified",
            ["globe-simplipfied-speech-bubble"] = "globe-simplipfied-speech-bubble",
            grid = "grid",
            guilded = "guilded",
            ["hack-week"] = "hack-week",
            ["hammer-code"] = "hammer-code",
            ["hand-curved-arrow-left"] = "hand-curved-arrow-left",
            ["hand-dual-arrows"] = "hand-dual-arrows",
            ["hand-ellipse"] = "hand-ellipse",
            ["hand-half-ellipse"] = "hand-half-ellipse",
            ["hand-two-arrows-horizontal"] = "hand-two-arrows-horizontal",
            hashtag = "hashtag",
            ["hat-fedora"] = "hat-fedora",
            ["hat-toque"] = "hat-toque",
            ["head-blank"] = "head-blank",
            ["head-blush"] = "head-blush",
            ["head-female"] = "head-female",
            ["head-freckles"] = "head-freckles",
            ["head-lips"] = "head-lips",
            ["head-male"] = "head-male",
            headphones = "headphones",
            ["headphones-arrow-up"] = "headphones-arrow-up",
            ["headphones-arrow-up-lock"] = "headphones-arrow-up-lock",
            ["headphones-slash"] = "headphones-slash",
            ["headphones-x"] = "headphones-x",
            ["headphones-x-lock"] = "headphones-x-lock",
            heart = "heart",
            house = "house",
            image = "image",
            ["image-stacked"] = "image-stacked",
            instagram = "instagram",
            jacket = "jacket",
            key = "key",
            ["key-alt"] = "key-alt",
            ["key-apostrophe"] = "key-apostrophe",
            ["key-arrow-down"] = "key-arrow-down",
            ["key-arrow-right"] = "key-arrow-right",
            ["key-arrow-up"] = "key-arrow-up",
            ["key-asterisk"] = "key-asterisk",
            ["key-backspace"] = "key-backspace",
            ["key-caps-lock"] = "key-caps-lock",
            ["key-caret"] = "key-caret",
            ["key-comma"] = "key-comma",
            ["key-command"] = "key-command",
            ["key-control"] = "key-control",
            ["key-grave-accent"] = "key-grave-accent",
            ["key-period"] = "key-period",
            ["key-return"] = "key-return",
            ["key-shift"] = "key-shift",
            ["key-space"] = "key-space",
            ["key-tab"] = "key-tab",
            ["language-characters"] = "language-characters",
            ["leg-left"] = "leg-left",
            ["leg-right"] = "leg-right",
            ["lightning-bolt"] = "lightning-bolt",
            linkedin = "linkedin",
            lips = "lips",
            lipstick = "lipstick",
            ["list-bulleted"] = "list-bulleted",
            ["location-pin"] = "location-pin",
            ["location-pin-map"] = "location-pin-map",
            ["lock-closed"] = "lock-closed",
            lollipop = "lollipop",
            ["magnifying-glass"] = "magnifying-glass",
            ["magnifying-glass-minus"] = "magnifying-glass-minus",
            ["magnifying-glass-plus"] = "magnifying-glass-plus",
            mascara = "mascara",
            megaphone = "megaphone",
            ["memory-card"] = "memory-card",
            messenger = "messenger",
            microphone = "microphone",
            ["microphone-slash"] = "microphone-slash",
            ["microphone-text-box"] = "microphone-text-box",
            ["microphone-triangle-exclamation"] = "microphone-triangle-exclamation",
            minus = "minus",
            ["minus-small"] = "minus-small",
            ["mirror-standing"] = "mirror-standing",
            moments = "moments",
            moon = "moon",
            ["mouse-button-left"] = "mouse-button-left",
            ["mouse-button-right"] = "mouse-button-right",
            ["mouse-scrollwheel"] = "mouse-scrollwheel",
            ["music-note"] = "music-note",
            nebula = "nebula",
            necklace = "necklace",
            ["nine-dots-grid"] = "nine-dots-grid",
            ninja = "ninja",
            nose = "nose",
            page = "page",
            ["paint-brush"] = "paint-brush",
            ["paint-bucket"] = "paint-bucket",
            pants = "pants",
            ["pants-2d-text"] = "pants-2d-text",
            ["paper-airplane"] = "paper-airplane",
            parrot = "parrot",
            ["pause-large"] = "pause-large",
            ["pause-small"] = "pause-small",
            pencil = "pencil",
            ["pencil-square"] = "pencil-square",
            person = "person",
            ["person-arrow-from-bottom-right"] = "person-arrow-from-bottom-right",
            ["person-check"] = "person-check",
            ["person-circle-slash"] = "person-circle-slash",
            ["person-climbing"] = "person-climbing",
            ["person-clock"] = "person-clock",
            ["person-falling"] = "person-falling",
            ["person-graduate"] = "person-graduate",
            ["person-jumping"] = "person-jumping",
            ["person-magnifying-glass"] = "person-magnifying-glass",
            ["person-photo-camera"] = "person-photo-camera",
            ["person-play"] = "person-play",
            ["person-play-clock"] = "person-play-clock",
            ["person-plus"] = "person-plus",
            ["person-racing"] = "person-racing",
            ["person-running"] = "person-running",
            ["person-standing"] = "person-standing",
            ["person-standing-arrow-reverse"] = "person-standing-arrow-reverse",
            ["person-standing-dual-arrows-vertical"] = "person-standing-dual-arrows-vertical",
            ["person-standing-gear"] = "person-standing-gear",
            ["person-swimming"] = "person-swimming",
            ["person-teleport"] = "person-teleport",
            ["person-trash-can"] = "person-trash-can",
            ["person-walking"] = "person-walking",
            ["person-with-smaller-person"] = "person-with-smaller-person",
            phone = "phone",
            ["phone-down"] = "phone-down",
            ["phone-plus"] = "phone-plus",
            ["phone-volume"] = "phone-volume",
            ["phone-x"] = "phone-x",
            ["photo-camera"] = "photo-camera",
            ["photo-camera-face"] = "photo-camera-face",
            ["photo-camera-slash"] = "photo-camera-slash",
            ["picture-in-picture"] = "picture-in-picture",
            pig = "pig",
            pin = "pin",
            ["pin-slash"] = "pin-slash",
            ["play-large"] = "play-large",
            ["play-small"] = "play-small",
            ["plus-large"] = "plus-large",
            ["plus-small"] = "plus-small",
            premium = "premium",
            ["ps-circle"] = "ps-circle",
            ["ps-dpad-down"] = "ps-dpad-down",
            ["ps-dpad-left"] = "ps-dpad-left",
            ["ps-dpad-right"] = "ps-dpad-right",
            ["ps-dpad-up"] = "ps-dpad-up",
            ["ps-l1"] = "ps-l1",
            ["ps-l2"] = "ps-l2",
            ["ps-l3"] = "ps-l3",
            ["ps-r1"] = "ps-r1",
            ["ps-r2"] = "ps-r2",
            ["ps-r3"] = "ps-r3",
            ["ps-square"] = "ps-square",
            ["ps-stick-left"] = "ps-stick-left",
            ["ps-stick-right"] = "ps-stick-right",
            ["ps-triagle"] = "ps-triagle",
            ["ps-x"] = "ps-x",
            ["ps4-options"] = "ps4-options",
            ["ps4-share"] = "ps4-share",
            ["ps4-touchpad"] = "ps4-touchpad",
            ["ps5-options"] = "ps5-options",
            ["ps5-share"] = "ps5-share",
            ["ps5-touchpad"] = "ps5-touchpad",
            pumpkin = "pumpkin",
            purse = "purse",
            ["rectangle-list"] = "rectangle-list",
            ["rectangle-numbers-counting"] = "rectangle-numbers-counting",
            ["rectangle-person-with-three-horizontal-lines"] = "rectangle-person-with-three-horizontal-lines",
            robux = "robux",
            ["rosette-seven-point"] = "rosette-seven-point",
            ["rosette-ten-point"] = "rosette-ten-point",
            ["seven-point-rosette"] = "seven-point-rosette",
            ["shield-check"] = "shield-check",
            ["shield-lock"] = "shield-lock",
            shirt = "shirt",
            ["shirt-2d-text"] = "shirt-2d-text",
            ["shirt-pants"] = "shirt-pants",
            ["shoe-left"] = "shoe-left",
            ["shoe-right"] = "shoe-right",
            ["shopping-basket"] = "shopping-basket",
            ["shopping-basket-check"] = "shopping-basket-check",
            ["shopping-cart"] = "shopping-cart",
            shorts = "shorts",
            sidebar = "sidebar",
            ["signal-exclamation"] = "signal-exclamation",
            ["six-dots-two-column-grid"] = "six-dots-two-column-grid",
            ["skip-end-large"] = "skip-end-large",
            ["skip-end-small"] = "skip-end-small",
            ["skip-next-large"] = "skip-next-large",
            ["skip-next-small"] = "skip-next-small",
            ["skip-previous-large"] = "skip-previous-large",
            ["skip-previous-small"] = "skip-previous-small",
            ["skip-start-large"] = "skip-start-large",
            ["skip-start-small"] = "skip-start-small",
            ["smartphone-portrait"] = "smartphone-portrait",
            speaker = "speaker",
            ["speaker-slash"] = "speaker-slash",
            ["speaker-triangle-exclamation"] = "speaker-triangle-exclamation",
            ["speaker-x"] = "speaker-x",
            ["speech-bubble-align-center"] = "speech-bubble-align-center",
            ["speech-bubble-align-left"] = "speech-bubble-align-left",
            ["speech-bubble-exclamation"] = "speech-bubble-exclamation",
            ["speech-bubble-round"] = "speech-bubble-round",
            ["square-bone"] = "square-bone",
            ["square-books"] = "square-books",
            ["square-check"] = "square-check",
            ["square-code"] = "square-code",
            ["square-dashed-person-standing"] = "square-dashed-person-standing",
            ["square-dual-arrows-horizontal"] = "square-dual-arrows-horizontal",
            ["square-dual-arrows-to-corner"] = "square-dual-arrows-to-corner",
            ["square-face-sound"] = "square-face-sound",
            ["square-face-waving-hand"] = "square-face-waving-hand",
            ["square-face-winking"] = "square-face-winking",
            ["square-minus"] = "square-minus",
            ["square-person"] = "square-person",
            ["squares-grid-plus"] = "squares-grid-plus",
            ["squares-grid-qr"] = "squares-grid-qr",
            ["stacked-squares-arrow-down-left"] = "stacked-squares-arrow-down-left",
            ["stacked-squares-arrow-up-right"] = "stacked-squares-arrow-up-right",
            ["stacked-squares-plus"] = "stacked-squares-plus",
            star = "star",
            ["stop-large"] = "stop-large",
            ["stop-small"] = "stop-small",
            studio = "studio",
            sun = "sun",
            sweater = "sweater",
            sword = "sword",
            ["tag-sparkle"] = "tag-sparkle",
            teletype = "teletype",
            ["tencent-qq"] = "tencent-qq",
            ["text-b-bold"] = "text-b-bold",
            ["text-box-microphone"] = "text-box-microphone",
            ["text-h-subscript-1"] = "text-h-subscript-1",
            ["text-h-subscript-2"] = "text-h-subscript-2",
            ["text-h-subscript-3"] = "text-h-subscript-3",
            ["text-i-italic"] = "text-i-italic",
            ["text-s-strikethrough"] = "text-s-strikethrough",
            ["text-u-underline"] = "text-u-underline",
            ["text-uppercase-a-lowercase-a"] = "text-uppercase-a-lowercase-a",
            ["text-x-subscript-2"] = "text-x-subscript-2",
            ["text-x-superscript-2"] = "text-x-superscript-2",
            ["three-bars-horizontal"] = "three-bars-horizontal",
            ["three-bars-horizontal-chevron-left"] = "three-bars-horizontal-chevron-left",
            ["three-bars-horizontal-narrowing"] = "three-bars-horizontal-narrowing",
            ["three-bars-horizontal-triangles-vertical"] = "three-bars-horizontal-triangles-vertical",
            ["three-bars-vertical-triangles-horizontal"] = "three-bars-vertical-triangles-horizontal",
            ["three-chevrons-enlarging-down"] = "three-chevrons-enlarging-down",
            ["three-chevrons-enlarging-up"] = "three-chevrons-enlarging-up",
            ["three-dots-horizontal"] = "three-dots-horizontal",
            ["three-dots-vertical"] = "three-dots-vertical",
            ["three-horizontal-bars-wrapping-right"] = "three-horizontal-bars-wrapping-right",
            ["three-people"] = "three-people",
            ["three-ring-note"] = "three-ring-note",
            ["three-sliders-horizontal"] = "three-sliders-horizontal",
            ["three-stacked-squares-tilted"] = "three-stacked-squares-tilted",
            ["thumb-down"] = "thumb-down",
            ["thumb-up"] = "thumb-up",
            ["tik-tok"] = "tik-tok",
            tilt = "tilt",
            torso = "torso",
            ["trash-can"] = "trash-can",
            ["triangle-exclamation"] = "triangle-exclamation",
            trophy = "trophy",
            tshirt = "tshirt",
            ["tshirt-2d-text"] = "tshirt-2d-text",
            ["tshirt-dual-arrows"] = "tshirt-dual-arrows",
            twitch = "twitch",
            twitter = "twitter",
            ["two-arrows-down-and-up"] = "two-arrows-down-and-up",
            ["two-arrows-from-center"] = "two-arrows-from-center",
            ["two-arrows-left-right"] = "two-arrows-left-right",
            ["two-arrows-loop-clockwise"] = "two-arrows-loop-clockwise",
            ["two-arrows-loop-clockwise-1"] = "two-arrows-loop-clockwise-1",
            ["two-arrows-loop-clockwise-infinity"] = "two-arrows-loop-clockwise-infinity",
            ["two-arrows-spin-clockwise"] = "two-arrows-spin-clockwise",
            ["two-arrows-spin-clockwise-plus"] = "two-arrows-spin-clockwise-plus",
            ["two-arrows-switch-right"] = "two-arrows-switch-right",
            ["two-arrows-to-center"] = "two-arrows-to-center",
            ["two-folders"] = "two-folders",
            ["two-location-pins-connecting-arrow"] = "two-location-pins-connecting-arrow",
            ["two-makeup-brushes"] = "two-makeup-brushes",
            ["two-people"] = "two-people",
            ["two-people-speech-bubble"] = "two-people-speech-bubble",
            ["two-stacked-squares"] = "two-stacked-squares",
            ["two-switches-horizontal"] = "two-switches-horizontal",
            ["verified-backplate"] = "verified-backplate",
            ["verified-check"] = "verified-check",
            ["verified-mono"] = "verified-mono",
            ["video-camera"] = "video-camera",
            ["video-camera-arrow-to-bottom-left"] = "video-camera-arrow-to-bottom-left",
            ["video-camera-arrow-to-top-right"] = "video-camera-arrow-to-top-right",
            ["video-camera-slash"] = "video-camera-slash",
            ["video-camera-triangle-exclamation"] = "video-camera-triangle-exclamation",
            ["video-camera-x"] = "video-camera-x",
            wallet = "wallet",
            ["we-chat"] = "we-chat",
            whatsapp = "whatsapp",
            x = "x",
            ["x-small"] = "x-small",
            ["xbox-a"] = "xbox-a",
            ["xbox-a-pressed"] = "xbox-a-pressed",
            ["xbox-a-unpressed"] = "xbox-a-unpressed",
            ["xbox-b"] = "xbox-b",
            ["xbox-dpad"] = "xbox-dpad",
            ["xbox-dpad-down"] = "xbox-dpad-down",
            ["xbox-dpad-left"] = "xbox-dpad-left",
            ["xbox-dpad-right"] = "xbox-dpad-right",
            ["xbox-dpad-up"] = "xbox-dpad-up",
            ["xbox-lb"] = "xbox-lb",
            ["xbox-lt"] = "xbox-lt",
            ["xbox-menu"] = "xbox-menu",
            ["xbox-rb"] = "xbox-rb",
            ["xbox-rt"] = "xbox-rt",
            ["xbox-stick-left"] = "xbox-stick-left",
            ["xbox-stick-left-directional"] = "xbox-stick-left-directional",
            ["xbox-stick-left-horizontal"] = "xbox-stick-left-horizontal",
            ["xbox-stick-left-vertical"] = "xbox-stick-left-vertical",
            ["xbox-stick-right"] = "xbox-stick-right",
            ["xbox-stick-right-directional"] = "xbox-stick-right-directional",
            ["xbox-stick-right-horizontal"] = "xbox-stick-right-horizontal",
            ["xbox-stick-right-vertical"] = "xbox-stick-right-vertical",
            ["xbox-view"] = "xbox-view",
            ["xbox-x"] = "xbox-x",
            ["xbox-y"] = "xbox-y",
            ["xr-headset"] = "xr-headset",
            youtube = "youtube",
        }
    end)
    UiLibraryCore.IsMouseOverFrame = LPH_NO_VIRTUALIZE(function(B, u)
        if not u then
            return
        end
        if UiLibraryCore.Global3DRenderMode then
            if u.GuiState == Enum.GuiState.Hover or u.GuiState == Enum.GuiState.Press then
                return true
            end
            return false
        end
        local k, W = u.AbsolutePosition, u.AbsoluteSize
        if
            localMouse.X >= k.X and (localMouse.X <= k.X + W.X and (localMouse.Y >= k.Y and localMouse.Y <= k.Y + W.Y))
        then
            return true
        end
    end)
    UiLibraryCore.CreateSignal = LPH_NO_VIRTUALIZE(function(B, u)
        local k = Instance.new("BindableEvent")
        local W = {
            Value = u,
            __event = k,
        }
        function W.GetValue(B)
            return W.Value
        end
        function W.SetValue(u, B)
            W.Value = B
            return k:Fire(B)
        end
        function W.Connect(u, B)
            local W = k.Event:Connect(B)
            UiLibraryCore:AddSignal(W)
            return W
        end
        return W
    end)
    UiLibraryCore.SetIconMode = LPH_NO_VIRTUALIZE(function(B, u, k)
        local W = string.lower(string.sub(k, -5)) == "-bold"
        if W then
            u.Text = k:sub(1, -6)
            u.FontFace = UiLibraryCore.BuiltInBold
        else
            u.Text = k
            u.FontFace = UiLibraryCore.BuiltInRegular
        end
    end)
    function UiLibraryCore.GetIconFont(u, B)
        local k = string.lower(string.sub(B, -5)) == "-bold"
        if k then
            return UiLibraryCore.BuiltInBold
        end
        return UiLibraryCore.BuiltInRegular
    end
    function UiLibraryCore.MoreThanHalfY(u, B)
        return (UiLibraryCore.ScreenGui.AbsoluteSize.Y / 2) < B
    end
    UiLibraryCore.IsStudio = RunServiceUi:IsStudio()
    UiLibraryCore.IsMobile = UserInputServiceUi.TouchEnabled
    UiLibraryCore.CreateInput = LPH_NO_VIRTUALIZE(function(B, u, k)
        local T = Instance.new("ImageButton", u)
        T.ZIndex = u.ZIndex + 10
        T.Size = UDim2.fromScale(1, 1)
        T.BackgroundTransparency = 1
        T.ImageTransparency = 1
        T.Image = "rbxasset://textuers/translateIcon.png"
        if k then
            local B = T.MouseButton1Click:Connect(k)
            return T, B
        end
        return T
    end)
    UiLibraryCore.PlayAnimate = LPH_NO_VIRTUALIZE(function(B, u, k)
        local T = TweenServiceUi:Create(B, u or TweenInfo.new(0.25), k)
        T:Play()
        return T
    end)
    UiLibraryCore.Drag = LPH_NO_VIRTUALIZE(function(B, u, k)
        local W = false
        local l = nil
        local I = nil
        local q = TweenInfo.new(k)
        local x = function(B)
            local k = B.Position - l
            local W = UDim2.new(I.X.Scale, I.X.Offset + k.X, I.Y.Scale, I.Y.Offset + k.Y)
            if UiLibraryCore.Global3DRenderMode then
                UiLibraryCore.PlayAnimate(u, q, {
                    Position = UDim2.fromScale(0.5, 0.5),
                })
            else
                UiLibraryCore.PlayAnimate(u, q, {
                    Position = W,
                })
            end
        end
        UiLibraryCore:AddSignal(B.InputBegan:Connect(function(B)
            if B.UserInputType == Enum.UserInputType.MouseButton1 or B.UserInputType == Enum.UserInputType.Touch then
                W = true
                l = B.Position
                I = u.Position
                local k
                k = B.Changed:Connect(function()
                    if B.UserInputState == Enum.UserInputState.End then
                        W = false
                        k:Disconnect()
                    end
                end)
            end
        end))
        UiLibraryCore:AddSignal(UserInputServiceUi.InputChanged:Connect(function(B)
            if B.UserInputType == Enum.UserInputType.MouseMovement or B.UserInputType == Enum.UserInputType.Touch then
                if W then
                    x(B)
                end
            end
        end))
    end)
    UiLibraryCore.Rounding = LPH_NO_VIRTUALIZE(function(B, u)
        local k = 10 ^ (u or 0)
        return math.floor(B * k + 0.5) / k
    end)
    UiLibraryCore.ProcessParams = LPH_NO_VIRTUALIZE(function(B, u, k)
        u = u or {}
        local T = u or {}
        for B, k in next, k do
            T[B] = u[B] or k
        end
        table.clear(k)
        return T
    end)
    UiLibraryCore.EnabledBlur = true
    UiLibraryCore.BlurModuleParent = workspace.CurrentCamera
    UiLibraryCore.GetCalculatePosition = LPH_NO_VIRTUALIZE(function(B, u, k, T)
        local W = u
        local l = T
        local I = k - B
        local Y = ((W.x * I.x) + (W.y * I.y)) + (W.z * I.z)
        local q = ((W.x * l.x) + (W.y * l.y)) + (W.z * l.z)
        local x = -Y / q
        return k + (x * T)
    end)
    UiLibraryCore.CreateBlurModule = LPH_NO_VIRTUALIZE(function(B, u, k)
        if not UiLibraryCore.EnabledBlur then
            return UiLibraryCore:AddSignal((Instance.new("BindableEvent")).Event:Connect(function()
                return "nl"
            end))
        end
        local W = Instance.new("Part", UiLibraryCore.BlurModuleParent)
        local I = Instance.new("DepthOfFieldEffect", cloneReferenceSafe(game:GetService("Lighting")))
        local q = Instance.new("BlockMesh")
        q.Parent = W
        W.Material = Enum.Material.Glass
        W.Transparency = 1
        W.Reflectance = 1
        W.CastShadow = false
        W.Anchored = true
        W.CanCollide = false
        W.CanQuery = false
        W.CollisionGroup = UiLibraryCore.RandomString()
        W.Size = Vector3.new(1, 1, 1) * 0.01
        W.Color = Color3.fromRGB(0, 0, 0)
        I.Enabled = true
        I.FarIntensity = 0
        I.FocusDistance = 0
        I.InFocusRadius = 1000
        I.NearIntensity = 1
        I.Name = UiLibraryCore.RandomString()
        W.Name = UiLibraryCore.RandomString()
        local x
        local o = function()
            local B = k:GetValue()
            if B and not UiLibraryCore.Global3DRenderMode then
                UiLibraryCore.PlayAnimate(I, TweenInfo.new(0.1), {
                    NearIntensity = 1,
                })
                UiLibraryCore.PlayAnimate(W, TweenInfo.new(0.1), {
                    Transparency = 0.97,
                    Size = Vector3.new(1, 1, 1) * 0.01,
                })
                W.Parent = UiLibraryCore.BlurModuleParent
            else
                UiLibraryCore.PlayAnimate(I, TweenInfo.new(0.1), {
                    NearIntensity = 0,
                })
                UiLibraryCore.PlayAnimate(W, TweenInfo.new(0.1), {
                    Size = Vector3.zero,
                    Transparency = 1.5,
                })
                W.Parent = nil
                return false
            end
            if B then
                local B = u.AbsolutePosition
                local k = B + u.AbsoluteSize
                local l = currentCameraUi.ScreenPointToRay(currentCameraUi, B.X, B.Y, 1)
                local I = currentCameraUi.ScreenPointToRay(currentCameraUi, k.X, k.Y, 1)
                local Y = currentCameraUi.CFrame.Position
                    + currentCameraUi.CFrame.LookVector * (0.05 - currentCameraUi.NearPlaneZ)
                local x = currentCameraUi.CFrame.LookVector
                local o = UiLibraryCore.GetCalculatePosition(Y, x, l.Origin, l.Direction)
                local i = UiLibraryCore.GetCalculatePosition(Y, x, I.Origin, I.Direction)
                o = currentCameraUi.CFrame:PointToObjectSpace(o)
                i = currentCameraUi.CFrame:PointToObjectSpace(i)
                local E = i - o
                local L = (o + i) / 2
                q.Offset = L
                q.Scale = E / 0.0101
                W.CFrame = currentCameraUi.CFrame
            end
        end
        local i = UiLibraryCore:AddSignal((currentCameraUi:GetPropertyChangedSignal("CFrame")):Connect(o))
        local E = UiLibraryCore:AddSignal(UserInputServiceUi.InputChanged:Connect(function(B)
            if
                B.UserInputType == Enum.UserInputType.MouseButton1
                or B.UserInputType == Enum.UserInputType.MouseMovement
                or B.UserInputType == Enum.UserInputType.Touch
            then
                pcall(o)
            end
        end))
        local L = task.spawn(function()
            while true do
                task.wait(0.1)
                pcall(o)
            end
        end)
        x = function()
            i:Disconnect()
            E:Disconnect()
            task.cancel(L)
            W:Destroy()
            I:Destroy()
        end
        u.Destroying:Connect(x)
        return i
    end)
    local v = function() end
    function UiLibraryCore.RollingEffect(u, B)
        local k = Instance.new("UIGradient")
        k.Transparency = NumberSequence.new({
            NumberSequenceKeypoint.new(0, 0.4),
            NumberSequenceKeypoint.new(1, 0),
        })
        k.Parent = B
        return k
    end
    function UiLibraryCore.CreateShadow(k, B, u)
        local W = {}
        local l = Instance.new("UIStroke")
        local I = Instance.new("UIStroke")
        local Y = Instance.new("UIStroke")
        local q = Instance.new("UIStroke")
        l.Thickness = 6
        l.Transparency = 1
        l.Parent = B
        I.Thickness = 5
        I.Transparency = 1
        I.Parent = B
        Y.Thickness = 4
        Y.Transparency = 1
        Y.Parent = B
        q.Thickness = 3
        q.Transparency = 1
        q.Parent = B
        local x
        local o, i, E, L
        if u then
            o = UiLibraryCore:RollingEffect(l)
            i = UiLibraryCore:RollingEffect(I)
            E = UiLibraryCore:RollingEffect(Y)
            L = UiLibraryCore:RollingEffect(q)
        end
        W.Render = LPH_NO_VIRTUALIZE(function(B, k)
            if x then
                task.cancel(x)
                x = nil
            end
            if k then
                UiLibraryCore.PlayAnimate(l, tweenNormal, {
                    Transparency = 0.9,
                })
                UiLibraryCore.PlayAnimate(I, tweenNormal, {
                    Transparency = 0.9,
                })
                UiLibraryCore.PlayAnimate(Y, tweenNormal, {
                    Transparency = 0.9,
                })
                UiLibraryCore.PlayAnimate(q, tweenNormal, {
                    Transparency = 0.9,
                })
                if u then
                    x = task.spawn(function()
                        local B = 20
                        while true do
                            task.wait(0.025)
                            UiLibraryCore.PlayAnimate(o, tweenNormal, {
                                Rotation = o.Rotation + B,
                            })
                            UiLibraryCore.PlayAnimate(i, tweenNormal, {
                                Rotation = i.Rotation + B,
                            })
                            UiLibraryCore.PlayAnimate(E, tweenNormal, {
                                Rotation = E.Rotation + B,
                            })
                            UiLibraryCore.PlayAnimate(L, tweenNormal, {
                                Rotation = L.Rotation + B,
                            })
                        end
                    end)
                end
            else
                UiLibraryCore.PlayAnimate(l, tweenNormal, {
                    Transparency = 1,
                })
                UiLibraryCore.PlayAnimate(I, tweenNormal, {
                    Transparency = 1,
                })
                UiLibraryCore.PlayAnimate(Y, tweenNormal, {
                    Transparency = 1,
                })
                UiLibraryCore.PlayAnimate(q, tweenNormal, {
                    Transparency = 1,
                })
            end
        end)
        return W
    end
    function UiLibraryCore.CreateOptionWindow(k, B, u)
        u = u or 9
        local W = {
            Signal = UiLibraryCore:CreateSignal(false),
        }
        local l = Instance.new("Frame")
        local I = Instance.new("UICorner")
        local Y = Instance.new("UIListLayout")
        local q = Instance.new("UIStroke")
        local x = UiLibraryCore:CreateShadow(l)
        l.Name = UiLibraryCore.RandomString()
        l.Parent = UiLibraryCore.ScreenGui
        l.AnchorPoint = Vector2.new(0, 0)
        l.BackgroundColor3 = Color3.fromRGB(20, 22, 27)
        l.BackgroundTransparency = 0.035
        l.BorderColor3 = Color3.fromRGB(0, 0, 0)
        l.BorderSizePixel = 0
        l.ClipsDescendants = true
        l.Position = UDim2.new(255, 255, 255, 255)
        l.Size = UDim2.new(0, 220, 0, 75)
        l.ZIndex = u + 9
        I.CornerRadius = UDim.new(0, 10)
        I.Parent = l
        Y.Parent = l
        Y.HorizontalAlignment = Enum.HorizontalAlignment.Center
        Y.SortOrder = Enum.SortOrder.LayoutOrder
        q.Transparency = 0.65
        q.Color = Color3.fromRGB(45, 48, 58)
        q.Parent = l
        UiLibraryCore:AddSignal((Y:GetPropertyChangedSignal("AbsoluteContentSize")):Connect(LPH_NO_VIRTUALIZE(function()
            UiLibraryCore.PlayAnimate(l, tweenNormal, {
                Size = UDim2.new(0, 220, 0, Y.AbsoluteContentSize.Y - 1),
            })
        end)))
        UiLibraryCore:AddSignal(
            (l:GetPropertyChangedSignal("BackgroundTransparency")):Connect(LPH_NO_VIRTUALIZE(function()
                if l.BackgroundTransparency > 0.9 then
                    l.Visible = false
                    Y.Parent = nil
                    l.Parent = nil
                else
                    l.Visible = true
                    Y.Parent = l
                    if UiLibraryCore.Global3DRenderMode then
                        l.Parent = UiLibraryCore.GlobalSurfaceGui
                    else
                        l.Parent = UiLibraryCore.ScreenGui
                    end
                end
            end))
        )
        local o
        local i = LPH_NO_VIRTUALIZE(function()
            if UiLibraryCore:MoreThanHalfY(B.AbsolutePosition.Y + 65) then
                l.AnchorPoint = Vector2.new(0, 1)
            else
                l.AnchorPoint = Vector2.new(0, 0)
            end
            l.Position = UDim2.fromOffset(B.AbsolutePosition.X + 18, B.AbsolutePosition.Y + 65)
        end)
        W.SetRender = LPH_NO_VIRTUALIZE(function(B)
            if o then
                task.cancel(o)
                o = nil
            end
            if B then
                i()
                UiLibraryCore.PlayAnimate(l, tweenNormal, {
                    BackgroundTransparency = 0.035,
                })
                UiLibraryCore.PlayAnimate(q, tweenNormal, {
                    Transparency = 0.65,
                })
                x:Render(true)
                if UiLibraryCore.Global3DRenderMode then
                    l.Parent = UiLibraryCore.GlobalSurfaceGui
                else
                    l.Parent = UiLibraryCore.ScreenGui
                end
                o = task.spawn(function()
                    while true do
                        task.wait()
                        i()
                    end
                end)
            else
                UiLibraryCore.PlayAnimate(l, tweenNormal, {
                    BackgroundTransparency = 1,
                })
                UiLibraryCore.PlayAnimate(q, tweenNormal, {
                    Transparency = 1,
                })
                x:Render(false)
            end
        end)
        W.SetRender(false)
        W.Signal:Connect(W.SetRender)
        local E = UiLibraryCore:RegisiterItem(l, W.Signal)
        E.Winbdow = W
        E.Root = l
        E.Signal = W.Signal
        return E
    end
    function UiLibraryCore.CreateColorPicker(u, B)
        local k = B.ZIndex
        local W = {}
        local l = Instance.new("Frame")
        local I = Instance.new("UICorner")
        local q = Instance.new("UIStroke")
        local x = Instance.new("ImageLabel")
        local o = Instance.new("UICorner")
        local i = Instance.new("Frame")
        local E = Instance.new("UICorner")
        local L = Instance.new("UIStroke")
        local Z = Instance.new("Frame")
        local F = Instance.new("UIGradient")
        local b = Instance.new("UICorner")
        local d = Instance.new("Frame")
        local D = Instance.new("UIStroke")
        local H = Instance.new("UICorner")
        local R = Instance.new("TextLabel")
        local m = Instance.new("UICorner")
        local w = UiLibraryCore:CreateShadow(l)
        l.Name = UiLibraryCore.RandomString()
        l.Parent = UiLibraryCore.ScreenGui
        l.AnchorPoint = Vector2.new(0, 0)
        l.BackgroundColor3 = Color3.fromRGB(20, 22, 27)
        l.BackgroundTransparency = 0.035
        l.BorderColor3 = Color3.fromRGB(0, 0, 0)
        l.BorderSizePixel = 0
        l.ClipsDescendants = true
        l.Position = UDim2.new(255, 0, 255, 20)
        l.Size = UDim2.new(0, 200, 0, 240)
        l.ZIndex = k + 125
        UiLibraryCore:AddSignal(
            (l:GetPropertyChangedSignal("BackgroundTransparency")):Connect(LPH_NO_VIRTUALIZE(function()
                if l.BackgroundTransparency > 0.9 then
                    l.Visible = false
                    l.Parent = nil
                else
                    l.Visible = true
                    if UiLibraryCore.Global3DRenderMode then
                        l.Parent = UiLibraryCore.GlobalSurfaceGui
                    else
                        l.Parent = UiLibraryCore.ScreenGui
                    end
                end
            end))
        )
        I.CornerRadius = UDim.new(0, 10)
        I.Parent = l
        q.Transparency = 0.65
        q.Color = Color3.fromRGB(45, 48, 58)
        q.Parent = l
        x.Name = UiLibraryCore.RandomString()
        x.Parent = l
        x.AnchorPoint = Vector2.new(0.5, 0)
        x.BackgroundColor3 = Color3.fromRGB(255, 0, 4)
        x.BorderColor3 = Color3.fromRGB(0, 0, 0)
        x.BorderSizePixel = 0
        x.Position = UDim2.new(0.5, 0, 0, 5)
        x.Size = UDim2.new(0, 185, 0, 185)
        x.ZIndex = k + 126
        x.Image = UiLibraryCore.ImageColorMapping
        o.CornerRadius = UDim.new(0, 5)
        o.Parent = x
        i.Name = UiLibraryCore.RandomString()
        i.Parent = x
        i.AnchorPoint = Vector2.new(0.5, 0.5)
        i.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
        i.BackgroundTransparency = 1
        i.BorderColor3 = Color3.fromRGB(0, 0, 0)
        i.BorderSizePixel = 0
        i.Position = UDim2.new(0.5, 0, 0.5, 0)
        i.Size = UDim2.new(0, 10, 0, 10)
        i.ZIndex = k + 127
        E.CornerRadius = UDim.new(1, 0)
        E.Parent = i
        L.Color = Color3.fromRGB(255, 255, 255)
        L.Parent = i
        Z.Name = UiLibraryCore.RandomString()
        Z.Parent = l
        Z.AnchorPoint = Vector2.new(0.5, 0)
        Z.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
        Z.BorderColor3 = Color3.fromRGB(0, 0, 0)
        Z.BorderSizePixel = 0
        Z.Position = UDim2.new(0.5, 0, 0, 200)
        Z.Size = UDim2.new(1, -15, 0, 10)
        Z.ZIndex = k + 126
        F.Color = ColorSequence.new({
            ColorSequenceKeypoint.new(0, Color3.fromRGB(255, 0, 0)),
            ColorSequenceKeypoint.new(0.1, Color3.fromRGB(255, 153, 0)),
            ColorSequenceKeypoint.new(0.2, Color3.fromRGB(203, 255, 0)),
            ColorSequenceKeypoint.new(0.3, Color3.fromRGB(50, 255, 0)),
            ColorSequenceKeypoint.new(0.4, Color3.fromRGB(0, 255, 102)),
            ColorSequenceKeypoint.new(0.5, Color3.fromRGB(0, 255, 255)),
            ColorSequenceKeypoint.new(0.6, Color3.fromRGB(0, 101, 255)),
            ColorSequenceKeypoint.new(0.7, Color3.fromRGB(50, 0, 255)),
            ColorSequenceKeypoint.new(0.8, Color3.fromRGB(204, 0, 255)),
            ColorSequenceKeypoint.new(0.9, Color3.fromRGB(255, 0, 153)),
            ColorSequenceKeypoint.new(1, Color3.fromRGB(255, 0, 0)),
        })
        F.Parent = Z
        b.CornerRadius = UDim.new(0, 3)
        b.Parent = Z
        d.Name = UiLibraryCore.RandomString()
        d.Parent = Z
        d.AnchorPoint = Vector2.new(0.5, 0.5)
        d.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
        d.BackgroundTransparency = 1
        d.BorderColor3 = Color3.fromRGB(0, 0, 0)
        d.BorderSizePixel = 0
        d.Position = UDim2.new(0, 0, 0.5, 0)
        d.Size = UDim2.new(0, 5, 1, 0)
        d.ZIndex = k + 126
        D.Thickness = 2
        D.Color = Color3.fromRGB(255, 255, 255)
        D.Parent = d
        H.CornerRadius = UDim.new(0, 3)
        H.Parent = d
        R.Name = UiLibraryCore.RandomString()
        R.Parent = l
        R.BackgroundColor3 = Color3.fromRGB(26, 28, 36)
        R.BackgroundTransparency = 0.75
        R.BorderColor3 = Color3.fromRGB(0, 0, 0)
        R.BorderSizePixel = 0
        R.Position = UDim2.new(0, 10, 0, 217)
        R.Size = UDim2.new(1, -20, 0, 15)
        R.ZIndex = k + 127
        R.Font = Enum.Font.GothamBold
        R.Text = "#FFFFFF"
        R.TextColor3 = Color3.fromRGB(255, 255, 255)
        R.TextSize = 12
        R.TextTransparency = 0.4
        R.TextXAlignment = Enum.TextXAlignment.Left
        m.CornerRadius = UDim.new(0, 4)
        m.Parent = R
        W.SetRender = LPH_NO_VIRTUALIZE(function(u)
            if u then
                l.Position = UDim2.new(0, B.AbsolutePosition.X + 20, 0, B.AbsolutePosition.Y + 75)
                UiLibraryCore.PlayAnimate(l, tweenNormal, {
                    BackgroundTransparency = 0.035,
                })
                UiLibraryCore.PlayAnimate(q, tweenNormal, {
                    Transparency = 0.65,
                })
                UiLibraryCore.PlayAnimate(x, tweenNormal, {
                    BackgroundTransparency = 0,
                    ImageTransparency = 0,
                })
                UiLibraryCore.PlayAnimate(L, tweenNormal, {
                    Transparency = 0,
                })
                UiLibraryCore.PlayAnimate(Z, tweenNormal, {
                    BackgroundTransparency = 0,
                })
                UiLibraryCore.PlayAnimate(D, tweenNormal, {
                    Transparency = 0,
                })
                UiLibraryCore.PlayAnimate(R, tweenNormal, {
                    BackgroundTransparency = 0.75,
                    TextTransparency = 0.4,
                })
                w:Render(true)
            else
                UiLibraryCore.PlayAnimate(l, tweenNormal, {
                    BackgroundTransparency = 1,
                })
                UiLibraryCore.PlayAnimate(q, tweenNormal, {
                    Transparency = 1,
                })
                UiLibraryCore.PlayAnimate(x, tweenNormal, {
                    BackgroundTransparency = 1,
                    ImageTransparency = 1,
                })
                UiLibraryCore.PlayAnimate(L, tweenNormal, {
                    Transparency = 1,
                })
                UiLibraryCore.PlayAnimate(Z, tweenNormal, {
                    BackgroundTransparency = 1,
                })
                UiLibraryCore.PlayAnimate(D, tweenNormal, {
                    Transparency = 1,
                })
                UiLibraryCore.PlayAnimate(R, tweenNormal, {
                    BackgroundTransparency = 1,
                    TextTransparency = 1,
                })
                w:Render(false)
            end
        end)
        W.SetRender(false)
        W.Root = l
        W.H = 1
        W.S = 1
        W.V = 1
        W.Callback = v
        function W.Update(B)
            local u = Color3.fromHSV(W.H, W.S, W.V)
            UiLibraryCore.PlayAnimate(i, tweenFast, {
                Position = UDim2.fromScale(W.S, 1 - W.V),
            })
            UiLibraryCore.PlayAnimate(x, tweenFast, {
                BackgroundColor3 = Color3.fromHSV(W.H, 1, 1),
            })
            UiLibraryCore.PlayAnimate(d, tweenFast, {
                Position = UDim2.fromScale(W.H, 0.5),
            })
            R.Text = "#" .. u:ToHex()
            W.Callback(u)
        end
        function W.SetValue(u, B)
            if typeof(B) == "string" then
                B = Color3.fromHex(B)
            end
            local k, T, l = B:ToHSV()
            W.H = k
            W.S = T
            W.V = l
            W:Update()
        end
        W.IsHold = false
        UiLibraryCore:AddSignal(l.InputBegan:Connect(function(B)
            if B.UserInputType == Enum.UserInputType.MouseButton1 or B.UserInputType == Enum.UserInputType.Touch then
                W.IsHold = true
            end
        end))
        UiLibraryCore:AddSignal(l.InputEnded:Connect(function(B)
            if B.UserInputType == Enum.UserInputType.MouseButton1 or B.UserInputType == Enum.UserInputType.Touch then
                W.IsHold = false
            end
        end))
        UiLibraryCore:AddSignal(Z.InputBegan:Connect(LPH_NO_VIRTUALIZE(function(B)
            if B.UserInputType == Enum.UserInputType.MouseButton1 or B.UserInputType == Enum.UserInputType.Touch then
                W.IsHold = true
                while UserInputServiceUi:IsMouseButtonPressed(Enum.UserInputType.MouseButton1) or W.IsHold do
                    task.wait()
                    local B = Z.AbsolutePosition.X
                    local u = B + Z.AbsoluteSize.X
                    local k = math.clamp(localMouse.X, B, u)
                    local T = ((k - B) / (u - B))
                    W.H = T
                    W:Update()
                end
            end
        end)))
        UiLibraryCore:AddSignal(x.InputBegan:Connect(LPH_NO_VIRTUALIZE(function(B)
            if B.UserInputType == Enum.UserInputType.MouseButton1 or B.UserInputType == Enum.UserInputType.Touch then
                W.IsHold = true
                while UserInputServiceUi:IsMouseButtonPressed(Enum.UserInputType.MouseButton1) or W.IsHold do
                    task.wait()
                    local B = x.AbsolutePosition.X
                    local u = B + x.AbsoluteSize.X
                    local k, T = math.clamp(localMouse.X, B, u), x.AbsolutePosition.Y
                    local l = T + x.AbsoluteSize.Y
                    local I = math.clamp(localMouse.Y, T, l)
                    W.S = (k - B) / (u - B)
                    W.V = (1 - ((I - T) / (l - T)))
                    W:Update()
                end
            end
        end)))
        return W
    end
    UiLibraryCore.KeyEnum = {
        One = "1",
        Two = "2",
        Three = "3",
        Four = "4",
        Five = "5",
        Six = "6",
        Seven = "7",
        Eight = "8",
        Nine = "9",
        Zero = "0",
        Minus = "-",
        Plus = "+",
        BackSlash = "\\",
        Slash = "/",
        Period = ".",
        Semicolon = ";",
        Colon = ":",
        LeftControl = "LCtrl",
        RightControl = "RCtrl",
        LeftShift = "LShift",
        RightShift = "RShift",
        Return = "Enter",
        LeftBracket = "[",
        RightBracket = "]",
        Quote = "'",
        Comma = ",",
        Equals = "=",
        LeftSuper = "Super",
        RightSuper = "Super",
        LeftAlt = "LAlt",
        RightAlt = "RAlt",
        Escape = "Esc",
    }
    UiLibraryCore.EnumReverse = {}
    for B, u in next, UiLibraryCore.KeyEnum do
        UiLibraryCore.EnumReverse[u] = B
    end
    function UiLibraryCore.KeyCodeToStr(u, B)
        if typeof(B) == "string" then
            if UiLibraryCore.KeyEnum[B] then
                return UiLibraryCore.KeyEnum[B]
            end
            return B
        end
        return (UiLibraryCore.KeyEnum[B.Name] or B.Name)
    end
    function UiLibraryCore.StrToKeyCode(u, B)
        if UiLibraryCore.EnumReverse[B] then
            return Enum.KeyCode[UiLibraryCore.EnumReverse[B]]
        end
        return Enum.KeyCode[B]
    end
    function UiLibraryCore.RegisiterHandler(k, B, u)
        local W = {}
        local l = B.ZIndex
        function W.AddToggle(W, k)
            k = UiLibraryCore:ProcessParams(k, {
                Default = false,
                Flag = nil,
                Callback = v,
            })
            local I = Instance.new("Frame")
            local Y = Instance.new("UICorner")
            local q = Instance.new("Frame")
            local x = Instance.new("UICorner")
            I.Name = UiLibraryCore.RandomString()
            I.Parent = B
            I.BackgroundColor3 = Color3.fromRGB(10, 13, 21)
            I.BorderColor3 = Color3.fromRGB(0, 0, 0)
            I.BorderSizePixel = 0
            I.ClipsDescendants = true
            I.Size = UDim2.new(0, 30, 0, 18)
            I.ZIndex = l + 13
            I.LayoutOrder = -(#B:GetChildren() + 5)
            Y.CornerRadius = UDim.new(1, 0)
            Y.Parent = I
            q.Name = UiLibraryCore.RandomString()
            q.Parent = I
            q.AnchorPoint = Vector2.new(0.5, 0.5)
            q.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
            q.BackgroundTransparency = 0.5
            q.BorderColor3 = Color3.fromRGB(0, 0, 0)
            q.BorderSizePixel = 0
            q.Position = UDim2.new(0.300000012, 0, 0.5, 0)
            q.Size = UDim2.new(0, 16, 0, 16)
            q.ZIndex = l + 14
            x.CornerRadius = UDim.new(1, 0)
            x.Parent = q
            local o = {
                Root = I,
            }
            o.SetUI = LPH_NO_VIRTUALIZE(function(B)
                if B then
                    UiLibraryCore.PlayAnimate(I, tweenNormal, {
                        BackgroundTransparency = 0,
                        BackgroundColor3 = UiLibraryCore.AccentColor,
                    })
                    UiLibraryCore.PlayAnimate(q, tweenNormal, {
                        BackgroundColor3 = Color3.fromRGB(255, 255, 255),
                        BackgroundTransparency = 0,
                        Position = UDim2.new(0.7, 0, 0.5, 0),
                    })
                else
                    UiLibraryCore.PlayAnimate(I, tweenNormal, {
                        BackgroundTransparency = 0,
                        BackgroundColor3 = Color3.fromRGB(10, 13, 21),
                    })
                    UiLibraryCore.PlayAnimate(q, tweenNormal, {
                        BackgroundColor3 = Color3.fromRGB(255, 255, 255),
                        BackgroundTransparency = 0.5,
                        Position = UDim2.new(0.300000012, 0, 0.5, 0),
                    })
                end
            end)
            o.SetVisible = LPH_NO_VIRTUALIZE(function(B)
                if B then
                    o.SetUI(k.Default)
                else
                    UiLibraryCore.PlayAnimate(I, tweenNormal, {
                        BackgroundTransparency = 1,
                        BackgroundColor3 = Color3.fromRGB(10, 13, 21),
                    })
                    UiLibraryCore.PlayAnimate(q, tweenNormal, {
                        BackgroundColor3 = Color3.fromRGB(255, 255, 255),
                        BackgroundTransparency = 1,
                        Position = UDim2.new(0.300000012, 0, 0.5, 0),
                    })
                end
            end)
            o.SetUI(k.Default)
            o.SetVisible(u:GetValue())
            UiLibraryCore:CreateInput(
                I,
                LPH_NO_VIRTUALIZE(function()
                    k.Default = not k.Default
                    o.SetUI(k.Default)
                    k.Callback(k.Default)
                end)
            )
            o.Signal = u:Connect(o.SetVisible)
            function o.GetValue(B)
                return k.Default
            end
            function o.SetValue(T, B)
                k.Default = B
                if u:GetValue() then
                    o.SetUI(k.Default)
                end
                k.Callback(k.Default)
            end
            if k.Flag then
                UiLibraryCore.Flags[k.Flag] = o
            end
            return o
        end
        function W.AddSlider(W, k)
            k = UiLibraryCore:ProcessParams(k, {
                Default = 50,
                Min = 0,
                Max = 10,
                Type = "",
                Rounding = 0,
                Nums = {},
                Flag = nil,
                Size = 125,
                Callback = v,
            })
            local x = {}
            x.GetSize = LPH_NO_VIRTUALIZE(function()
                return (k.Default - k.Min) / (k.Max - k.Min)
            end)
            local o = TextServiceUi:GetTextSize(
                string.rep("0", (k.Rounding + #tostring(k.Max)) + 1) .. tostring(k.Type),
                10,
                Enum.Font.GothamMedium,
                Vector2.new(math.huge, math.huge)
            )
            x.MaximumSize = o.X
            if k.Nums then
                local B = 0
                for u, k in next, k.Nums do
                    local T = TextServiceUi:GetTextSize(
                        string.rep("m", string.len(tostring(k))),
                        10,
                        Enum.Font.GothamMedium,
                        Vector2.new(math.huge, math.huge)
                    )
                    if B < T.X then
                        B = T.X
                    end
                end
                if x.MaximumSize < B then
                    x.MaximumSize = B
                end
            end
            local i = Instance.new("Frame")
            local E = Instance.new("UICorner")
            local L = Instance.new("Frame")
            local Z = Instance.new("UICorner")
            local J = Instance.new("UIStroke")
            local F = Instance.new("TextBox")
            local b = Instance.new("Frame")
            local d = Instance.new("Frame")
            local D = Instance.new("UICorner")
            local H = Instance.new("Frame")
            local R = Instance.new("UICorner")
            local m = Instance.new("Frame")
            local w = Instance.new("UICorner")
            local e = 2
            i.Name = UiLibraryCore.RandomString()
            i.Parent = B
            i.BackgroundColor3 = Color3.fromRGB(26, 28, 36)
            i.BackgroundTransparency = 1
            i.BorderColor3 = Color3.fromRGB(0, 0, 0)
            i.BorderSizePixel = 0
            i.ClipsDescendants = false
            i.Size = UDim2.new(0, k.Size, 0, 18)
            i.ZIndex = l + 13
            i.LayoutOrder = -(#B:GetChildren() + 5)
            E.CornerRadius = UDim.new(0, 4)
            E.Parent = i
            L.Name = UiLibraryCore.RandomString()
            L.Parent = i
            L.AnchorPoint = Vector2.new(1, 0)
            L.BackgroundColor3 = Color3.fromRGB(26, 28, 36)
            L.BorderColor3 = Color3.fromRGB(0, 0, 0)
            L.BorderSizePixel = 0
            L.ClipsDescendants = true
            L.Position = UDim2.new(1, 0, 0, 0)
            L.Size = UDim2.new(0, x.MaximumSize + e, 0, 18)
            L.ZIndex = l + 13
            Z.CornerRadius = UDim.new(0, 4)
            Z.Parent = L
            J.Transparency = 0.65
            J.Color = Color3.fromRGB(45, 48, 58)
            J.Parent = L
            F.Name = UiLibraryCore.RandomString()
            F.Parent = L
            F.AnchorPoint = Vector2.new(0.5, 0.5)
            F.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
            F.BackgroundTransparency = 1
            F.BorderColor3 = Color3.fromRGB(0, 0, 0)
            F.BorderSizePixel = 0
            F.Position = UDim2.new(0.5, 0, 0.5, 0)
            F.Size = UDim2.new(1, 0, 1, 0)
            F.ZIndex = l + 14
            F.Font = Enum.Font.GothamMedium
            F.Text = tostring(k.Default) .. tostring(k.Type)
            F.TextColor3 = Color3.fromRGB(255, 255, 255)
            F.TextSize = 10
            F.ClearTextOnFocus = false
            F.TextTransparency = 0.35
            b.Name = UiLibraryCore.RandomString()
            b.Parent = i
            b.AnchorPoint = Vector2.new(0, 0.5)
            b.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
            b.BackgroundTransparency = 1
            b.BorderColor3 = Color3.fromRGB(0, 0, 0)
            b.BorderSizePixel = 0
            b.Position = UDim2.new(0, 0, 0.5, 0)
            b.Size = UDim2.new(1, -(x.MaximumSize + 11), 0, 18)
            b.ZIndex = l + 13
            d.Name = UiLibraryCore.RandomString()
            d.Parent = b
            d.AnchorPoint = Vector2.new(0, 0.5)
            d.BackgroundColor3 = Color3.fromRGB(30, 29, 36)
            d.BorderColor3 = Color3.fromRGB(0, 0, 0)
            d.BorderSizePixel = 0
            d.Position = UDim2.new(0, 0, 0.5, 0)
            d.Size = UDim2.new(1, 0, 0, 5)
            d.ZIndex = l + 13
            D.CornerRadius = UDim.new(1, 0)
            D.Parent = d
            H.Name = UiLibraryCore.RandomString()
            H.Parent = d
            H.BackgroundColor3 = UiLibraryCore.AccentColor
            H.BorderColor3 = Color3.fromRGB(0, 0, 0)
            H.BorderSizePixel = 0
            H.Size = UDim2.new(x.GetSize(), 0, 1, 0)
            H.ZIndex = l + 14
            R.CornerRadius = UDim.new(1, 0)
            R.Parent = H
            m.Parent = H
            m.AnchorPoint = Vector2.new(1, 0.5)
            m.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
            m.BorderColor3 = Color3.fromRGB(0, 0, 0)
            m.BorderSizePixel = 0
            m.Position = UDim2.new(1, 5, 0.5, 0)
            m.Size = UDim2.new(0, 10, 0, 10)
            m.ZIndex = l + 15
            w.CornerRadius = UDim.new(1, 0)
            w.Parent = m
            local G = LPH_NO_VIRTUALIZE(function()
                if k.Nums[k.Default] then
                    F.Text = k.Nums[k.Default]
                else
                    F.Text = tostring(k.Default) .. tostring(k.Type)
                end
            end)
            F.FocusLost:Connect(LPH_NO_VIRTUALIZE(function()
                local B = UiLibraryCore:ParseInput(F.Text, true)
                if B then
                    local u = math.clamp(B, k.Min, k.Max)
                    local W = UiLibraryCore.Rounding(u, k.Rounding)
                    if W then
                        k.Default = W;
                        (TweenServiceUi:Create(H, tweenFast, {
                            Size = UDim2.new(x.GetSize(), 0, 1, 0),
                        })):Play()
                        G()
                        k.Callback(k.Default)
                    else
                        G()
                    end
                else
                    G()
                end
            end))
            x.SetRender = LPH_NO_VIRTUALIZE(function(B)
                if B then
                    UiLibraryCore.PlayAnimate(L, tweenNormal, {
                        BackgroundTransparency = 0,
                        Size = UDim2.new(0, x.MaximumSize + e, 0, 18),
                    })
                    UiLibraryCore.PlayAnimate(J, tweenNormal, {
                        Transparency = 0.65,
                    })
                    UiLibraryCore.PlayAnimate(F, tweenNormal, {
                        TextTransparency = 0.35,
                    })
                    UiLibraryCore.PlayAnimate(d, tweenNormal, {
                        BackgroundTransparency = 0,
                    })
                    UiLibraryCore.PlayAnimate(H, tweenNormal, {
                        BackgroundTransparency = 0,
                        Size = UDim2.new(x.GetSize(), 0, 1, 0),
                    })
                    UiLibraryCore.PlayAnimate(m, tweenNormal, {
                        BackgroundTransparency = 0,
                    })
                else
                    UiLibraryCore.PlayAnimate(L, tweenNormal, {
                        BackgroundTransparency = 1,
                    })
                    UiLibraryCore.PlayAnimate(J, tweenNormal, {
                        Transparency = 1,
                    })
                    UiLibraryCore.PlayAnimate(F, tweenNormal, {
                        TextTransparency = 1,
                    })
                    UiLibraryCore.PlayAnimate(d, tweenNormal, {
                        BackgroundTransparency = 1,
                    })
                    UiLibraryCore.PlayAnimate(H, tweenNormal, {
                        BackgroundTransparency = 1,
                        Size = UDim2.new(0, 0, 1, 0),
                    })
                    UiLibraryCore.PlayAnimate(m, tweenNormal, {
                        BackgroundTransparency = 1,
                    })
                end
            end)
            x.SetRender(u:GetValue())
            x.Signal = u:Connect(x.SetRender)
            local U = function(B)
                local u = math.clamp(((B.Position.X - b.AbsolutePosition.X) / b.AbsoluteSize.X), 0, 1)
                local W = ((k.Max - k.Min) * u) + k.Min
                local l = UiLibraryCore.Rounding(W, k.Rounding)
                local Y = UDim2.fromScale(u, 1)
                local q = ((l - k.Min) / (k.Max - k.Min)) + 0.02
                k.Default = l;
                (TweenServiceUi:Create(H, tweenFast, {
                    Size = UDim2.new(x.GetSize(), 0, 1, 0),
                })):Play()
                G()
                k.Callback(l)
            end
            local f = false
            do
                b.InputBegan:Connect(LPH_NO_VIRTUALIZE(function(B)
                    if
                        B.UserInputType == Enum.UserInputType.MouseButton1
                        or B.UserInputType == Enum.UserInputType.Touch
                    then
                        f = true
                        U(B)
                    end
                end))
                b.InputEnded:Connect(LPH_NO_VIRTUALIZE(function(B)
                    if
                        B.UserInputType == Enum.UserInputType.MouseButton1
                        or B.UserInputType == Enum.UserInputType.Touch
                    then
                        if UserInputServiceUi.TouchEnabled then
                            if not UiLibraryCore:IsMouseOverFrame(b) then
                                f = false
                            end
                        else
                            f = false
                        end
                    end
                end))
                UserInputServiceUi.InputChanged:Connect(LPH_NO_VIRTUALIZE(function(B)
                    if f then
                        if
                            B.UserInputType == Enum.UserInputType.MouseMovement
                            or B.UserInputType == Enum.UserInputType.Touch
                        then
                            if UserInputServiceUi.TouchEnabled then
                                if not UiLibraryCore:IsMouseOverFrame(b) then
                                    f = false
                                else
                                    U(B)
                                end
                            else
                                U(B)
                            end
                        end
                    end
                end))
            end
            function x.GetValue(B)
                return k.Default
            end
            function x.SetValue(W, B)
                k.Default = B
                if u:GetValue() then
                    UiLibraryCore.PlayAnimate(H, tweenNormal, {
                        BackgroundTransparency = 0,
                        Size = UDim2.new(x.GetSize(), 0, 1, 0),
                    })
                end
                G()
                k.Callback(k.Default)
            end
            if k.Flag then
                UiLibraryCore.Flags[k.Flag] = x
            end
            return x
        end
        function W.AddOption(W, k)
            local I = Instance.new("Frame")
            local q = Instance.new("TextLabel")
            local x = Instance.new("UICorner")
            I.Name = UiLibraryCore.RandomString()
            I.Parent = B
            I.BackgroundColor3 = Color3.fromRGB(39, 40, 49)
            I.BackgroundTransparency = 1
            I.BorderColor3 = Color3.fromRGB(0, 0, 0)
            I.BorderSizePixel = 0
            I.ClipsDescendants = true
            I.Size = UDim2.new(0, 20, 0, 18)
            I.ZIndex = l + 13
            I.LayoutOrder = -(#B:GetChildren() + 5)
            q.Name = UiLibraryCore.RandomString()
            q.Parent = I
            q.AnchorPoint = Vector2.new(0.5, 0.5)
            q.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
            q.BackgroundTransparency = 1
            q.BorderColor3 = Color3.fromRGB(0, 0, 0)
            q.BorderSizePixel = 0
            q.Position = UDim2.new(0.5, 0, 0.5, 0)
            q.Size = UDim2.new(1, 0, 1, 0)
            q.ZIndex = l + 14
            q.FontFace = UiLibraryCore.BuiltInBold
            q.Text = (k == 1 and "gear") or (k == 2 and "chevron-large-right") or "three-dots-horizontal"
            q.TextColor3 = Color3.fromRGB(223, 223, 223)
            q.TextSize = 16
            q.TextTransparency = 0.4
            q.TextWrapped = true
            x.CornerRadius = UDim.new(0, 4)
            x.Parent = I
            local o = UiLibraryCore:CreateOptionWindow(I, l + 13)
            local i
            o.SetRender = LPH_NO_VIRTUALIZE(function(B)
                if B then
                    UiLibraryCore.PlayAnimate(q, tweenNormal, {
                        TextTransparency = 0.4,
                    })
                else
                    UiLibraryCore.PlayAnimate(q, tweenNormal, {
                        TextTransparency = 1,
                    })
                end
            end)
            o.SetRender(u:GetValue())
            u:Connect(o.SetRender)
            local E = UiLibraryCore:CreateInput(
                I,
                LPH_NO_VIRTUALIZE(function()
                    if i then
                        i:Disconnect()
                        i = nil
                    end
                    o.Signal:SetValue(true)
                    i = UserInputServiceUi.InputBegan:Connect(function(B)
                        if
                            B.UserInputType == Enum.UserInputType.MouseButton1
                            or B.UserInputType == Enum.UserInputType.Touch
                        then
                            if not UiLibraryCore:IsMouseOverFrame(o.Root) and not UiLibraryCore:IsMouseOverFrame(I) then
                                if i then
                                    i:Disconnect()
                                    i = nil
                                end
                                o.Signal:SetValue(false)
                            end
                        end
                    end)
                end)
            )
            UiLibraryCore:AddSignal(E.MouseEnter:Connect(LPH_NO_VIRTUALIZE(function()
                UiLibraryCore.PlayAnimate(I, tweenNormal, {
                    BackgroundTransparency = 0.5,
                })
                UiLibraryCore.PlayAnimate(q, tweenNormal, {
                    TextTransparency = 0.25,
                })
            end)))
            UiLibraryCore:AddSignal(E.MouseLeave:Connect(LPH_NO_VIRTUALIZE(function()
                UiLibraryCore.PlayAnimate(I, tweenNormal, {
                    BackgroundTransparency = 1,
                })
                UiLibraryCore.PlayAnimate(q, tweenNormal, {
                    TextTransparency = 0.4,
                })
            end)))
            return o
        end
        function W.AddColorPicker(W, k)
            k = UiLibraryCore:ProcessParams(k, {
                Default = Color3.fromRGB(255, 255, 255),
                Callback = v,
            })
            if typeof(k.Default) == "string" then
                k.Default = Color3.fromHex(k.Default:gsub("#", ""))
            end
            local I = {}
            local q = Instance.new("Frame")
            local x = Instance.new("UICorner")
            local o = Instance.new("UIStroke")
            local i = Instance.new("ImageLabel")
            local E = Instance.new("UICorner")
            q.Name = UiLibraryCore.RandomString()
            q.Parent = B
            q.BackgroundColor3 = k.Default
            q.BackgroundTransparency = 0
            q.BorderColor3 = Color3.fromRGB(0, 0, 0)
            q.BorderSizePixel = 0
            q.ClipsDescendants = true
            q.Size = UDim2.new(0, 18, 0, 18)
            q.ZIndex = l + 13
            q.LayoutOrder = -(#B:GetChildren() + 5)
            x.CornerRadius = UDim.new(0, 4)
            x.Parent = q
            o.Transparency = 0.65
            o.Color = Color3.fromRGB(45, 48, 58)
            o.Parent = q
            i.Parent = q
            i.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
            i.BorderColor3 = Color3.fromRGB(0, 0, 0)
            i.BorderSizePixel = 0
            i.Size = UDim2.new(1, 0, 1, 0)
            i.ZIndex = l + 11
            i.Image = "rbxasset://textures/meshPartFallback.png"
            i.ImageTransparency = 0.9
            i.BackgroundTransparency = 1
            i.ScaleType = Enum.ScaleType.Crop
            E.CornerRadius = UDim.new(0, 4)
            E.Parent = i
            local L = UiLibraryCore:CreateColorPicker(q)
            L:SetValue(k.Default)
            L.Callback = function(B)
                q.BackgroundColor3 = B
                k.Default = B
                k.Callback(k.Default)
            end
            local Z
            UiLibraryCore:CreateInput(
                q,
                LPH_NO_VIRTUALIZE(function()
                    if Z then
                        Z:Disconnect()
                        Z = nil
                    end
                    L.SetRender(true)
                    Z = UserInputServiceUi.InputBegan:Connect(function(B)
                        if
                            B.UserInputType == Enum.UserInputType.MouseButton1
                            or B.UserInputType == Enum.UserInputType.Touch
                        then
                            if not UiLibraryCore:IsMouseOverFrame(q) and not UiLibraryCore:IsMouseOverFrame(L.Root) then
                                if Z then
                                    Z:Disconnect()
                                    Z = nil
                                end
                                L.SetRender(false)
                            end
                        end
                    end)
                end)
            )
            I.SetRender = LPH_NO_VIRTUALIZE(function(B)
                if B then
                    UiLibraryCore.PlayAnimate(q, tweenNormal, {
                        BackgroundTransparency = 0,
                    })
                    UiLibraryCore.PlayAnimate(o, tweenNormal, {
                        Transparency = 0.65,
                    })
                    UiLibraryCore.PlayAnimate(i, tweenNormal, {
                        ImageTransparency = 0.9,
                    })
                else
                    UiLibraryCore.PlayAnimate(q, tweenNormal, {
                        BackgroundTransparency = 1,
                    })
                    UiLibraryCore.PlayAnimate(o, tweenNormal, {
                        Transparency = 1,
                    })
                    UiLibraryCore.PlayAnimate(i, tweenNormal, {
                        ImageTransparency = 1,
                    })
                end
            end)
            I.SetRender(u:GetValue())
            u:Connect(I.SetRender)
            function I.GetValue(B)
                return k.Default
            end
            function I.SetValue(u, B)
                k.Default = B
                L:SetValue(k.Default)
            end
            if k.Flag then
                UiLibraryCore.Flags[k.Flag] = I
            end
            return I
        end
        function W.AddKeybind(W, k)
            k = UiLibraryCore:ProcessParams(k, {
                Default = nil,
                Blacklist = {},
                Callback = v,
                Flag = nil,
            })
            local I = {}
            local x = Instance.new("Frame")
            local o = Instance.new("UICorner")
            local i = Instance.new("UIStroke")
            local E = Instance.new("TextLabel")
            x.Name = UiLibraryCore.RandomString()
            x.Parent = B
            x.BackgroundColor3 = Color3.fromRGB(26, 28, 36)
            x.BorderColor3 = Color3.fromRGB(0, 0, 0)
            x.BorderSizePixel = 0
            x.ClipsDescendants = true
            x.Size = UDim2.new(0, 45, 0, 18)
            x.ZIndex = l + 13
            x.LayoutOrder = -(#B:GetChildren() + 5)
            o.CornerRadius = UDim.new(0, 4)
            o.Parent = x
            i.Transparency = 0.65
            i.Color = Color3.fromRGB(45, 48, 58)
            i.Parent = x
            E.Name = UiLibraryCore.RandomString()
            E.Parent = x
            E.AnchorPoint = Vector2.new(0.5, 0.5)
            E.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
            E.BackgroundTransparency = 1
            E.BorderColor3 = Color3.fromRGB(0, 0, 0)
            E.BorderSizePixel = 0
            E.ClipsDescendants = true
            E.Position = UDim2.new(0.5, 0, 0.5, 0)
            E.Size = UDim2.new(1, 0, 1, 0)
            E.ZIndex = l + 14
            E.Font = Enum.Font.GothamMedium
            E.Text = UiLibraryCore:KeyCodeToStr(k.Default or "None")
            E.TextColor3 = Color3.fromRGB(255, 255, 255)
            E.TextSize = 10
            E.TextTransparency = 0.5
            I.SetRender = LPH_NO_VIRTUALIZE(function(B)
                if B then
                    UiLibraryCore.PlayAnimate(x, tweenNormal, {
                        BackgroundTransparency = 0,
                    })
                    UiLibraryCore.PlayAnimate(i, tweenNormal, {
                        Transparency = 0.65,
                    })
                    UiLibraryCore.PlayAnimate(E, tweenNormal, {
                        TextTransparency = 0.5,
                    })
                else
                    UiLibraryCore.PlayAnimate(x, tweenNormal, {
                        BackgroundTransparency = 1,
                    })
                    UiLibraryCore.PlayAnimate(i, tweenNormal, {
                        Transparency = 1,
                    })
                    UiLibraryCore.PlayAnimate(E, tweenNormal, {
                        TextTransparency = 1,
                    })
                end
            end)
            function I.Update(B)
                local u = TextServiceUi:GetTextSize(E.Text, E.TextSize, E.Font, Vector2.new(math.huge, math.huge))
                UiLibraryCore.PlayAnimate(x, tweenNormal, {
                    Size = UDim2.new(0, u.X + 7, 0, 18),
                })
            end
            local L = LPH_NO_VIRTUALIZE(function(B)
                return k.Blacklist and (k.Blacklist[B] or table.find(k.Blacklist, B))
            end)
            I:Update()
            I.SetRender(u:GetValue())
            u:Connect(I.SetRender)
            local Z = false
            UiLibraryCore:CreateInput(x, function()
                if Z then
                    return
                end
                Z = true
                E.Text = "..."
                I:Update()
                local B = nil
                while not B do
                    local u = UserInputServiceUi.InputBegan:Wait()
                    if u.KeyCode ~= Enum.KeyCode.Unknown and (not L(u.KeyCode) and not L(u.KeyCode.Name)) then
                        B = u.KeyCode
                    else
                        if
                            u.UserInputType == Enum.UserInputType.MouseButton1
                            and (not L(Enum.UserInputType.MouseButton1) and not L("M1B"))
                        then
                            B = "M1B"
                        elseif
                            u.UserInputType == Enum.UserInputType.MouseButton2
                            and (not L(Enum.UserInputType.MouseButton2) and not L("M2B"))
                        then
                            B = "M2B"
                        end
                    end
                end
                Z = false
                local u = typeof(B) == "string" and B or B.Name
                k.Default = u
                E.Text = UiLibraryCore:KeyCodeToStr(u)
                I:Update()
                k.Callback(u)
            end)
            function I.GetValue(B)
                return k.Default
            end
            function I.SetValue(u, B)
                k.Default = B
                E.Text = UiLibraryCore:KeyCodeToStr(B)
                I:Update()
                k.Callback(k.Default)
            end
            if k.Flag then
                UiLibraryCore.Flags[k.Flag] = I
            end
            return I
        end
        function W.AddTextInput(W, k)
            k = UiLibraryCore:ProcessParams(k, {
                Default = "",
                Placeholder = "Placeholder",
                Callback = print,
                Flag = nil,
                Size = 100,
                Numeric = false,
            })
            local I = {}
            local Y = Instance.new("Frame")
            local q = Instance.new("UICorner")
            local x = Instance.new("UIStroke")
            local o = Instance.new("TextBox")
            Y.Name = UiLibraryCore.RandomString()
            Y.Parent = B
            Y.BackgroundColor3 = Color3.fromRGB(26, 28, 36)
            Y.BorderColor3 = Color3.fromRGB(0, 0, 0)
            Y.BorderSizePixel = 0
            Y.ClipsDescendants = true
            Y.Size = UDim2.new(0, k.Size, 0, 18)
            Y.ZIndex = l + 13
            Y.LayoutOrder = -(#B:GetChildren() + 5)
            q.CornerRadius = UDim.new(0, 4)
            q.Parent = Y
            x.Transparency = 0.65
            x.Color = Color3.fromRGB(45, 48, 58)
            x.Parent = Y
            o.Parent = Y
            o.AnchorPoint = Vector2.new(0, 0.5)
            o.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
            o.BackgroundTransparency = 1
            o.BorderColor3 = Color3.fromRGB(0, 0, 0)
            o.BorderSizePixel = 0
            o.Position = UDim2.new(0, 5, 0.5, 0)
            o.Size = UDim2.new(1, -5, 0, 17)
            o.ZIndex = l + 14
            o.ClearTextOnFocus = false
            o.Font = Enum.Font.GothamMedium
            o.PlaceholderText = k.Placeholder
            o.Text = tostring(k.Default)
            o.TextColor3 = Color3.fromRGB(255, 255, 255)
            o.TextSize = 11
            o.TextTransparency = 0.35
            o.TextXAlignment = Enum.TextXAlignment.Left
            I.SetRender = LPH_NO_VIRTUALIZE(function(B)
                if B then
                    UiLibraryCore.PlayAnimate(Y, tweenNormal, {
                        BackgroundTransparency = 0,
                    })
                    UiLibraryCore.PlayAnimate(x, tweenNormal, {
                        Transparency = 0.65,
                    })
                    UiLibraryCore.PlayAnimate(o, tweenNormal, {
                        TextTransparency = 0.35,
                    })
                else
                    UiLibraryCore.PlayAnimate(Y, tweenNormal, {
                        BackgroundTransparency = 1,
                    })
                    UiLibraryCore.PlayAnimate(x, tweenNormal, {
                        Transparency = 1,
                    })
                    UiLibraryCore.PlayAnimate(o, tweenNormal, {
                        TextTransparency = 1,
                    })
                end
            end)
            UiLibraryCore:AddSignal((o:GetPropertyChangedSignal("Text")):Connect(LPH_NO_VIRTUALIZE(function()
                local B = UiLibraryCore:ParseInput(o.Text, k.Numeric)
                if k.Numeric then
                    o.Text = string.gsub(o.Text, "[^0-9.]", "")
                end
                if B then
                    k.Default = B
                    k.Callback(B)
                end
            end)))
            I.SetRender(u:GetValue())
            u:Connect(I.SetRender)
            function I.GetValue(B)
                return k.Default
            end
            function I.SetValue(u, B)
                k.Default = B
                o.Text = tostring(B)
                k.Callback(k.Default)
            end
            if k.Flag then
                UiLibraryCore.Flags[k.Flag] = I
            end
            return I
        end
        function W.AddDropdown(W, k)
            k = UiLibraryCore:ProcessParams(k, {
                Default = nil,
                Values = {},
                Multi = false,
                Callback = v,
                AutoUpdate = false,
                Flag = nil,
                Size = 100,
            })
            k.Default = UiLibraryCore.ProcessDropdown(k.Default)
            local I = Instance.new("Frame")
            local x = Instance.new("TextLabel")
            local o = Instance.new("UICorner")
            local i = Instance.new("UIStroke")
            local E = Instance.new("TextLabel")
            I.Name = UiLibraryCore.RandomString()
            I.Parent = B
            I.BackgroundColor3 = Color3.fromRGB(26, 28, 36)
            I.BorderColor3 = Color3.fromRGB(0, 0, 0)
            I.BorderSizePixel = 0
            I.ClipsDescendants = true
            I.Size = UDim2.new(0, k.Size, 0, 18)
            I.ZIndex = l + 13
            I.LayoutOrder = -(#B:GetChildren() + 5)
            x.Name = UiLibraryCore.RandomString()
            x.Parent = I
            x.AnchorPoint = Vector2.new(1, 0.5)
            x.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
            x.BackgroundTransparency = 1
            x.BorderColor3 = Color3.fromRGB(0, 0, 0)
            x.BorderSizePixel = 0
            x.Position = UDim2.new(1, -2, 0.5, 0)
            x.Size = UDim2.new(0, 18, 0, 18)
            x.ZIndex = l + 14
            x.FontFace = UiLibraryCore.BuiltInBold
            x.Text = "chevron-small-down"
            x.TextColor3 = Color3.fromRGB(223, 223, 223)
            x.TextSize = 16
            x.TextTransparency = 0.25
            x.TextWrapped = true
            o.CornerRadius = UDim.new(0, 4)
            o.Parent = I
            i.Transparency = 0.65
            i.Color = Color3.fromRGB(45, 48, 58)
            i.Parent = I
            E.Name = UiLibraryCore.RandomString()
            E.Parent = I
            E.AnchorPoint = Vector2.new(0, 0.5)
            E.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
            E.BackgroundTransparency = 1
            E.BorderColor3 = Color3.fromRGB(0, 0, 0)
            E.BorderSizePixel = 0
            E.ClipsDescendants = true
            E.Position = UDim2.new(0, 5, 0.5, 0)
            E.Size = UDim2.new(1, -25, 0, 15)
            E.ZIndex = l + 14
            E.Font = Enum.Font.GothamMedium
            E.Text = UiLibraryCore.ParseDropdown(k.Default)
            E.TextColor3 = Color3.fromRGB(255, 255, 255)
            E.TextSize = 12
            E.TextTransparency = 0.5
            E.TextXAlignment = Enum.TextXAlignment.Left
            do
                local B = Instance.new("UIGradient")
                B.Transparency = NumberSequence.new({
                    NumberSequenceKeypoint.new(0, 0),
                    NumberSequenceKeypoint.new(0.85, 0.23),
                    NumberSequenceKeypoint.new(1, 1),
                })
                B.Parent = E
            end
            UiLibraryCore:AddSignal(I.MouseEnter:Connect(LPH_NO_VIRTUALIZE(function()
                UiLibraryCore.PlayAnimate(E, tweenNormal, {
                    TextTransparency = 0.2,
                })
            end)))
            UiLibraryCore:AddSignal(I.MouseLeave:Connect(LPH_NO_VIRTUALIZE(function()
                UiLibraryCore.PlayAnimate(E, tweenNormal, {
                    TextTransparency = 0.5,
                })
            end)))
            local L = {
                OpenSignal = UiLibraryCore:CreateSignal(false),
                Signals = {},
                Refuse = {},
            }
            L.SetRender = LPH_NO_VIRTUALIZE(function(B)
                if B then
                    UiLibraryCore.PlayAnimate(I, tweenNormal, {
                        BackgroundTransparency = 0,
                    })
                    UiLibraryCore.PlayAnimate(x, tweenNormal, {
                        TextTransparency = 0.25,
                    })
                    UiLibraryCore.PlayAnimate(i, tweenNormal, {
                        Transparency = 0.65,
                    })
                    UiLibraryCore.PlayAnimate(E, tweenNormal, {
                        TextTransparency = 0.5,
                    })
                else
                    UiLibraryCore.PlayAnimate(I, tweenNormal, {
                        BackgroundTransparency = 1,
                    })
                    UiLibraryCore.PlayAnimate(x, tweenNormal, {
                        TextTransparency = 1,
                    })
                    UiLibraryCore.PlayAnimate(i, tweenNormal, {
                        Transparency = 1,
                    })
                    UiLibraryCore.PlayAnimate(E, tweenNormal, {
                        TextTransparency = 1,
                    })
                end
            end)
            L.SetRender(u:GetValue())
            u:Connect(L.SetRender)
            L.ExtentSize = 0
            do
                local B = Instance.new("Frame")
                local u = Instance.new("UICorner")
                local W = Instance.new("UIStroke")
                local Y = Instance.new("ScrollingFrame")
                local q = Instance.new("UIListLayout")
                local x = UiLibraryCore:CreateShadow(B)
                B.Name = UiLibraryCore.RandomString()
                B.Parent = UiLibraryCore.ScreenGui
                B.AnchorPoint = Vector2.new(0.5, 0)
                B.BackgroundColor3 = Color3.fromRGB(20, 22, 27)
                B.BackgroundTransparency = 0.5
                B.BorderColor3 = Color3.fromRGB(0, 0, 0)
                B.BorderSizePixel = 0
                B.ClipsDescendants = true
                B.Position = UDim2.new(255, 255, 255, 255)
                B.Size = UDim2.new(0, 125, 0, 50)
                B.ZIndex = l + 125
                L.BlockRoot = B
                UiLibraryCore:AddSignal((B:GetPropertyChangedSignal("BackgroundTransparency")):Connect(function()
                    if B.BackgroundTransparency > 0.9 then
                        B.Visible = false
                        B.Parent = nil
                    else
                        B.Visible = true
                        if UiLibraryCore.Global3DRenderMode then
                            B.Parent = UiLibraryCore.GlobalSurfaceGui
                        else
                            B.Parent = UiLibraryCore.ScreenGui
                        end
                    end
                end))
                u.CornerRadius = UDim.new(0, 10)
                u.Parent = B
                W.Transparency = 0.65
                W.Color = Color3.fromRGB(45, 48, 58)
                W.Parent = B
                Y.Name = UiLibraryCore.RandomString()
                Y.Parent = B
                Y.Active = true
                Y.AnchorPoint = Vector2.new(0.5, 0.5)
                Y.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
                Y.BackgroundTransparency = 1
                Y.BorderColor3 = Color3.fromRGB(0, 0, 0)
                Y.BorderSizePixel = 0
                Y.Position = UDim2.new(0.5, 0, 0.5, 0)
                Y.Size = UDim2.new(1, -5, 1, -5)
                Y.ZIndex = l + 127
                Y.ScrollBarThickness = 0
                L.RootItem = Y
                q.Parent = Y
                q.HorizontalAlignment = Enum.HorizontalAlignment.Center
                q.SortOrder = Enum.SortOrder.LayoutOrder
                UiLibraryCore:AddSignal(
                    (q:GetPropertyChangedSignal("AbsoluteContentSize")):Connect(LPH_NO_VIRTUALIZE(function()
                        Y.CanvasSize = UDim2.fromOffset(0, q.AbsoluteContentSize.Y)
                        UiLibraryCore.PlayAnimate(B, tweenNormal, {
                            Size = UDim2.new(
                                0,
                                (I.AbsoluteSize.X + 5) + L.ExtentSize,
                                0,
                                math.min(q.AbsoluteContentSize.Y + 5, 250)
                            ),
                        })
                    end))
                )
                local o = LPH_NO_VIRTUALIZE(function()
                    if UiLibraryCore:MoreThanHalfY(I.AbsolutePosition.Y + 85) then
                        B.AnchorPoint = Vector2.new(0.5, 1)
                    else
                        B.AnchorPoint = Vector2.new(0.5, 0)
                    end
                    B.Position =
                        UDim2.fromOffset(I.AbsolutePosition.X + (B.AbsoluteSize.X / 2), I.AbsolutePosition.Y + 85)
                end)
                L.SetFrameRender = LPH_NO_VIRTUALIZE(function(u)
                    L.OpenSignal:SetValue(u)
                    if u then
                        x:Render(true)
                        B.Size = UDim2.new(
                            0,
                            (I.AbsoluteSize.X + 5) + L.ExtentSize,
                            0,
                            math.min(q.AbsoluteContentSize.Y + 5, 250)
                        )
                        o()
                        UiLibraryCore.PlayAnimate(B, tweenNormal, {
                            BackgroundTransparency = 0.035,
                        })
                        if k.AutoUpdate then
                            L:Generate()
                        end
                    else
                        UiLibraryCore.PlayAnimate(B, tweenNormal, {
                            BackgroundTransparency = 1,
                        })
                        x:Render(false)
                    end
                end)
                L.SetFrameRender(false)
            end
            local Z
            UiLibraryCore:CreateInput(
                I,
                LPH_NO_VIRTUALIZE(function()
                    if Z then
                        Z:Disconnect()
                        Z = nil
                    end
                    L.SetFrameRender(true)
                    UiLibraryCore.IsMosueOverOtherFrame = true
                    Z = UserInputServiceUi.InputBegan:Connect(function(B)
                        if
                            B.UserInputType == Enum.UserInputType.MouseButton1
                            or B.UserInputType == Enum.UserInputType.Touch
                        then
                            if
                                not UiLibraryCore:IsMouseOverFrame(L.BlockRoot)
                                and not UiLibraryCore:IsMouseOverFrame(I)
                            then
                                if Z then
                                    Z:Disconnect()
                                    Z = nil
                                end
                                UiLibraryCore.IsMosueOverOtherFrame = false
                                L.SetFrameRender(false)
                            end
                        end
                    end)
                end)
            )
            L.IsMatch = LPH_NO_VIRTUALIZE(function(B)
                if typeof(k.Default) == "table" then
                    if k.Default[B] or table.find(k.Default, B) then
                        return true
                    end
                end
                if k.Default == B then
                    return true
                end
            end)
            function L.Generate(B)
                for B, u in next, L.RootItem:GetChildren() do
                    if u:IsA("Frame") then
                        u:Destroy()
                    end
                end
                for B, u in next, L.Signals do
                    u:Disconnect()
                end
                table.clear(L.Signals)
                table.clear(L.Refuse)
                local u
                for B, W in next, k.Values do
                    local I = Instance.new("Frame")
                    local Y = Instance.new("TextLabel")
                    local x = Instance.new("UICorner")
                    I.Name = UiLibraryCore.RandomString()
                    I.Parent = L.RootItem
                    I.BackgroundColor3 = Color3.fromRGB(29, 31, 38)
                    I.BackgroundTransparency = 1
                    I.BorderColor3 = Color3.fromRGB(0, 0, 0)
                    I.BorderSizePixel = 0
                    I.Size = UDim2.new(1, 0, 0, 25)
                    I.ZIndex = l + 1258
                    Y.Name = UiLibraryCore.RandomString()
                    Y.Parent = I
                    Y.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
                    Y.BackgroundTransparency = 1
                    Y.BorderColor3 = Color3.fromRGB(0, 0, 0)
                    Y.BorderSizePixel = 0
                    Y.Position = UDim2.new(0, 15, 0, 4)
                    Y.Size = UDim2.new(0, 1, 0, 15)
                    Y.ZIndex = l + 1258
                    Y.Font = Enum.Font.GothamMedium
                    Y.Text = tostring(W)
                    Y.TextColor3 = Color3.fromRGB(255, 255, 255)
                    Y.TextSize = 13
                    Y.TextTransparency = 0.2
                    Y.TextXAlignment = Enum.TextXAlignment.Left
                    x.CornerRadius = UDim.new(0, 10)
                    x.Parent = I
                    local o = TextServiceUi:GetTextSize(Y.Text, Y.TextSize, Y.Font, Vector2.new(math.huge, math.huge))
                    L.ExtentSize = math.max(L.ExtentSize, o.X)
                    local i, Z = nil, nil
                    if k.Multi then
                        local B = Instance.new("TextLabel")
                        B.Parent = I
                        B.AnchorPoint = Vector2.new(0, 0.5)
                        B.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
                        B.BackgroundTransparency = 1
                        B.BorderColor3 = Color3.fromRGB(0, 0, 0)
                        B.BorderSizePixel = 0
                        B.Position = UDim2.new(0, 5, 0.5, 0)
                        B.Size = UDim2.new(0, 20, 0, 20)
                        B.ZIndex = l + 1259
                        B.FontFace = UiLibraryCore.BuiltInBold
                        B.Text = "check"
                        B.TextColor3 = Color3.fromRGB(223, 223, 223)
                        B.TextSize = 18
                        B.TextTransparency = 1
                        B.TextWrapped = true
                        local k = LPH_NO_VIRTUALIZE(function()
                            if L.IsMatch(W) then
                                UiLibraryCore.PlayAnimate(Y, tweenQuint, {
                                    TextTransparency = 0.2,
                                    Position = UDim2.new(0, 30, 0, 4),
                                })
                                UiLibraryCore.PlayAnimate(B, vs, {
                                    TextTransparency = 0.25,
                                })
                                u = Y
                            else
                                UiLibraryCore.PlayAnimate(B, tweenNormal, {
                                    TextTransparency = 1,
                                })
                                UiLibraryCore.PlayAnimate(Y, tweenQuint, {
                                    TextTransparency = 0.5,
                                    Position = UDim2.new(0, 15, 0, 4),
                                })
                            end
                        end)
                        i = B
                        Z = k
                    else
                        local B = LPH_NO_VIRTUALIZE(function()
                            if L.IsMatch(W) then
                                UiLibraryCore.PlayAnimate(Y, tweenNormal, {
                                    TextTransparency = 0.2,
                                })
                                u = Y
                            else
                                UiLibraryCore.PlayAnimate(Y, tweenNormal, {
                                    TextTransparency = 0.5,
                                })
                            end
                        end)
                        Z = B
                    end
                    Z()
                    table.insert(L.Refuse, Z)
                    table.insert(
                        L.Signals,
                        I.MouseEnter:Connect(LPH_NO_VIRTUALIZE(function()
                            UiLibraryCore.PlayAnimate(I, tweenNormal, {
                                BackgroundTransparency = 0.1,
                            })
                        end))
                    )
                    table.insert(
                        L.Signals,
                        I.MouseLeave:Connect(LPH_NO_VIRTUALIZE(function()
                            UiLibraryCore.PlayAnimate(I, tweenNormal, {
                                BackgroundTransparency = 1,
                            })
                        end))
                    )
                    table.insert(
                        L.Signals,
                        L.OpenSignal:Connect(LPH_NO_VIRTUALIZE(function(B)
                            if B then
                                Z()
                            else
                                UiLibraryCore.PlayAnimate(Y, tweenNormal, {
                                    TextTransparency = 1,
                                })
                                if i then
                                    UiLibraryCore.PlayAnimate(i, tweenNormal, {
                                        TextTransparency = 1,
                                    })
                                end
                            end
                        end))
                    )
                    if k.Multi then
                        local B, u = UiLibraryCore:CreateInput(
                            I,
                            LPH_NO_VIRTUALIZE(function()
                                k.Default[W] = not k.Default[W]
                                Z()
                                E.Text = UiLibraryCore.ParseDropdown(k.Default)
                                k.Callback(k.Default)
                            end)
                        )
                        table.insert(L.Signals, u)
                    else
                        local B, u = UiLibraryCore:CreateInput(
                            I,
                            LPH_NO_VIRTUALIZE(function()
                                k.Default = W
                                for B, u in next, L.Refuse do
                                    task.spawn(u)
                                end
                                E.Text = UiLibraryCore.ParseDropdown(k.Default)
                                k.Callback(k.Default)
                            end)
                        )
                        table.insert(L.Signals, u)
                    end
                end
            end
            L:Generate()
            function L.GetValue(B)
                return k.Default
            end
            function L.SetValue(u, B)
                k.Default = B
                E.Text = UiLibraryCore.ParseDropdown(k.Default)
                for B, u in next, L.Refuse do
                    task.spawn(u)
                end
                k.Callback(k.Default)
            end
            function L.SetValues(u, B)
                k.Values = B
                if not k.AutoUpdate then
                    L:Generate()
                end
            end
            if k.Flag then
                UiLibraryCore.Flags[k.Flag] = L
            end
            return L
        end
        return W
    end
    UiLibraryCore.ProcessDropdown = LPH_NO_VIRTUALIZE(function(B)
        if typeof(B) == "table" then
            local u = {}
            for B, k in next, B do
                if typeof(k) == "boolean" and typeof(B) ~= "number" then
                    u[B] = k
                else
                    u[k] = true
                end
            end
            return u
        else
            return B
        end
    end)
    UiLibraryCore.ParseDropdown = LPH_NO_VIRTUALIZE(function(B)
        if not B then
            return "Select"
        end
        local u
        if typeof(B) == "table" then
            if #B > 0 then
                local k = {}
                for B, u in next, B do
                    table.insert(k, tostring(u))
                end
                u = table.concat(k, " , ")
                table.clear(k)
            else
                local k = {}
                for B, u in next, B do
                    if u == true then
                        table.insert(k, tostring(B))
                    end
                end
                u = table.concat(k, " , ")
                table.clear(k)
                if not u:byte() then
                    u = "Select"
                end
            end
        else
            u = tostring(B or "Select")
        end
        return u
    end)
    function UiLibraryCore.ParseInput(k, B, u)
        if not B then
            return (u and nil) or ""
        end
        if u then
            local u = string.gsub(tostring(B), "[^0-9.%-]", "")
            if tonumber(u) then
                return tonumber(u)
            end
            return nil
        end
        return B
    end
    function UiLibraryCore.CreateToolTips(W, B, u, k)
        local l = Instance.new("Frame")
        local I = Instance.new("UICorner")
        local Y = Instance.new("UIStroke")
        local x = Instance.new("TextLabel")
        local o = Instance.new("TextLabel")
        local i = UiLibraryCore:CreateShadow(l)
        l.Name = UiLibraryCore.RandomString()
        l.BackgroundColor3 = Color3.fromRGB(20, 22, 27)
        l.BackgroundTransparency = 0.075
        l.BorderColor3 = Color3.fromRGB(0, 0, 0)
        l.BorderSizePixel = 0
        l.ClipsDescendants = true
        l.Position = UDim2.new(255, 255, 255, 255)
        l.Size = UDim2.new(0, 0, 0, 0)
        l.ZIndex = 130
        I.CornerRadius = UDim.new(0, 10)
        I.Parent = l
        Y.Transparency = 0.65
        Y.Color = Color3.fromRGB(45, 48, 58)
        Y.Parent = l
        x.Name = UiLibraryCore.RandomString()
        x.Parent = l
        x.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
        x.BackgroundTransparency = 1
        x.BorderColor3 = Color3.fromRGB(0, 0, 0)
        x.BorderSizePixel = 0
        x.Position = UDim2.new(0, 15, 0, 5)
        x.Size = UDim2.new(0, 1, 0, 20)
        x.ZIndex = 132
        x.Font = Enum.Font.GothamBold
        x.Text = u
        x.TextColor3 = Color3.fromRGB(255, 255, 255)
        x.TextSize = 15
        x.TextXAlignment = Enum.TextXAlignment.Left
        o.Name = UiLibraryCore.RandomString()
        o.Parent = l
        o.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
        o.BackgroundTransparency = 1
        o.BorderColor3 = Color3.fromRGB(0, 0, 0)
        o.BorderSizePixel = 0
        o.Position = UDim2.new(0, 15, 0, 30)
        o.Size = UDim2.new(0, 1, 0, 15)
        o.ZIndex = 132
        o.Font = Enum.Font.GothamBold
        o.Text = k
        o.TextColor3 = Color3.fromRGB(255, 255, 255)
        o.TextSize = 12
        o.TextTransparency = 0.65
        o.TextXAlignment = Enum.TextXAlignment.Left
        o.TextYAlignment = Enum.TextYAlignment.Top
        local E = {}
        E.Update = LPH_NO_VIRTUALIZE(function()
            local B = TextServiceUi:GetTextSize(x.Text, x.TextSize, x.Font, Vector2.new(math.huge, math.huge))
            local u = TextServiceUi:GetTextSize(o.Text, o.TextSize, o.Font, Vector2.new(math.huge, math.huge))
            local k = math.max(B.X, u.X) + 65
            local W = (B.Y + u.Y) + 30
            UiLibraryCore.PlayAnimate(l, tweenNormal, {
                Size = UDim2.new(0, k, 0, W),
            })
        end)
        UiLibraryCore:AddSignal(
            (l:GetPropertyChangedSignal("BackgroundTransparency")):Connect(LPH_NO_VIRTUALIZE(function()
                if l.BackgroundTransparency > 0.9 then
                    l.Visible = false
                    l.Parent = nil
                else
                    l.Visible = true
                    if UiLibraryCore.Global3DRenderMode then
                        l.Parent = UiLibraryCore.GlobalSurfaceGui
                    else
                        l.Parent = UiLibraryCore.ScreenGui
                    end
                end
            end))
        )
        E.SetRender = LPH_NO_VIRTUALIZE(function(u)
            if u then
                l.Position = UDim2.fromOffset(
                    B.AbsolutePosition.X + B.AbsoluteSize.X,
                    B.AbsolutePosition.Y + (B.AbsoluteSize.Y + 25)
                )
                UiLibraryCore.PlayAnimate(l, tweenNormal, {
                    BackgroundTransparency = 0.075,
                })
                UiLibraryCore.PlayAnimate(Y, tweenNormal, {
                    Transparency = 0.65,
                })
                UiLibraryCore.PlayAnimate(x, tweenNormal, {
                    TextTransparency = 0,
                })
                UiLibraryCore.PlayAnimate(o, tweenNormal, {
                    TextTransparency = 0.65,
                })
                E.Update()
                i:Render(true)
            else
                UiLibraryCore.PlayAnimate(l, tweenNormal, {
                    BackgroundTransparency = 1,
                })
                UiLibraryCore.PlayAnimate(Y, tweenNormal, {
                    Transparency = 1,
                })
                UiLibraryCore.PlayAnimate(x, tweenNormal, {
                    TextTransparency = 1,
                })
                UiLibraryCore.PlayAnimate(o, tweenNormal, {
                    TextTransparency = 1,
                })
                i:Render(false)
            end
        end)
        E.SetRender(false)
        E.Update()
        local L
        UiLibraryCore:AddSignal(B.MouseEnter:Connect(LPH_NO_VIRTUALIZE(function()
            if L then
                task.cancel(L)
                L = nil
            end
            L = task.delay(1, E.SetRender, true)
        end)))
        UiLibraryCore:AddSignal(B.MouseLeave:Connect(LPH_NO_VIRTUALIZE(function()
            if L then
                task.cancel(L)
                L = nil
            end
            E.SetRender(false)
            E.Update()
        end)))
        return E
    end
    function UiLibraryCore.RegisiterItem(k, B, u)
        local W = {}
        local l = B.ZIndex
        function W.AddLabel(I, k, W)
            local Y = Instance.new("Frame")
            local x = Instance.new("TextLabel")
            local o = Instance.new("Frame")
            local i = Instance.new("Frame")
            local E = Instance.new("UIListLayout")
            local L = Instance.new("UICorner")
            Y.Name = UiLibraryCore.RandomString()
            Y.Parent = B
            Y.BackgroundColor3 = Color3.fromRGB(25, 27, 33)
            Y.BackgroundTransparency = 1
            Y.BorderColor3 = Color3.fromRGB(0, 0, 0)
            Y.BorderSizePixel = 0
            Y.Size = UDim2.new(1, 0, 0, 30)
            Y.ZIndex = l + 8
            UiLibraryCore:AddQuery(Y, k)
            x.Name = UiLibraryCore.RandomString()
            x.Parent = Y
            x.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
            x.BackgroundTransparency = 1
            x.BorderColor3 = Color3.fromRGB(0, 0, 0)
            x.BorderSizePixel = 0
            x.Position = UDim2.new(0, 11, 0, 6)
            x.Size = UDim2.new(0, 1, 0, 15)
            x.ZIndex = l + 9
            x.Font = Enum.Font.GothamMedium
            x.Text = k
            x.TextColor3 = Color3.fromRGB(255, 255, 255)
            x.TextSize = 13
            x.TextTransparency = 0.35
            x.TextTruncate = Enum.TextTruncate.AtEnd
            x.TextXAlignment = Enum.TextXAlignment.Left
            o.Name = UiLibraryCore.RandomString()
            o.Parent = Y
            o.AnchorPoint = Vector2.new(0.5, 1)
            o.BackgroundColor3 = Color3.fromRGB(45, 48, 58)
            o.BackgroundTransparency = 0.65
            o.BorderColor3 = Color3.fromRGB(0, 0, 0)
            o.BorderSizePixel = 0
            o.Position = UDim2.new(0.5, 0, 1, 0)
            o.Size = UDim2.new(1, -20, 0, 1)
            o.ZIndex = l + 11
            i.Name = UiLibraryCore.RandomString()
            i.Parent = Y
            i.AnchorPoint = Vector2.new(1, 0)
            i.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
            i.BackgroundTransparency = 1
            i.BorderColor3 = Color3.fromRGB(0, 0, 0)
            i.BorderSizePixel = 0
            i.Position = UDim2.new(1, -11, 0, 2)
            i.Size = UDim2.new(1, -20, 0, 25)
            i.ZIndex = l + 12
            E.Parent = i
            E.FillDirection = Enum.FillDirection.Horizontal
            E.HorizontalAlignment = Enum.HorizontalAlignment.Right
            E.SortOrder = Enum.SortOrder.LayoutOrder
            E.VerticalAlignment = Enum.VerticalAlignment.Center
            E.Padding = UDim.new(0, 5)
            L.CornerRadius = UDim.new(0, 10)
            L.Parent = Y
            local Z
            local J = LPH_NO_VIRTUALIZE(function()
                if W or not Y.Parent then
                    return
                end
                local B = Y.AbsoluteSize.X
                if B <= 1 then
                    return
                end
                local u = (TextServiceUi:GetTextSize(x.Text, x.TextSize, x.Font, Vector2.new(math.huge, 15))).X
                local k = E.AbsoluteContentSize.X
                local T = k > 0 and ((u + k) + 27 > B)
                if Z ~= T then
                    Z = T
                    if T then
                        Y.Size = UDim2.new(1, 0, 0, 52)
                        x.Position = UDim2.new(0, 11, 0, 5)
                        i.Position = UDim2.new(1, -11, 0, 24)
                        i.Size = UDim2.new(1, -22, 0, 25)
                    else
                        Y.Size = UDim2.new(1, 0, 0, 30)
                        x.Position = UDim2.new(0, 11, 0, 6)
                        i.Position = UDim2.new(1, -11, 0, 2)
                        i.Size = UDim2.new(1, -20, 0, 25)
                    end
                end
                if T then
                    x.Size = UDim2.new(1, -22, 0, 15)
                else
                    x.Size = UDim2.fromOffset(math.max(8, (B - k) - 27), 15)
                end
            end)
            UiLibraryCore:AddSignal((Y:GetPropertyChangedSignal("AbsoluteSize")):Connect(J))
            UiLibraryCore:AddSignal((E:GetPropertyChangedSignal("AbsoluteContentSize")):Connect(J))
            task.defer(J)
            local F = LPH_NO_VIRTUALIZE(function()
                local B = TextServiceUi:GetTextSize(x.Text, x.TextSize, x.Font, Vector2.new(math.huge, math.huge))
                UiLibraryCore.PlayAnimate(Y, tweenNormal, {
                    Size = UDim2.new(1, 0, 0, B.Y + 13),
                })
                x.Size = UDim2.new(1, -35, 1, 0)
                x.TextYAlignment = Enum.TextYAlignment.Top
            end)
            if W then
                F()
            end
            local b = UiLibraryCore:RegisiterHandler(i, u)
            b.Root = Y
            b.SetRender = LPH_NO_VIRTUALIZE(function(B)
                if B then
                    UiLibraryCore.PlayAnimate(Y, tweenNormal, {
                        BackgroundTransparency = 1,
                    })
                    UiLibraryCore.PlayAnimate(x, tweenNormal, {
                        TextTransparency = 0.35,
                    })
                    UiLibraryCore.PlayAnimate(o, tweenNormal, {
                        BackgroundTransparency = 0.65,
                    })
                else
                    UiLibraryCore.PlayAnimate(Y, tweenNormal, {
                        BackgroundTransparency = 1,
                    })
                    UiLibraryCore.PlayAnimate(x, tweenNormal, {
                        TextTransparency = 1,
                    })
                    UiLibraryCore.PlayAnimate(o, tweenNormal, {
                        BackgroundTransparency = 1,
                    })
                end
            end)
            function b.SetVisible(u, B)
                Y.Visible = B
            end
            UiLibraryCore:AddSignal(Y.MouseEnter:Connect(LPH_NO_VIRTUALIZE(function()
                UiLibraryCore.PlayAnimate(Y, tweenNormal, {
                    BackgroundTransparency = 0.35,
                })
                UiLibraryCore.PlayAnimate(x, tweenNormal, {
                    TextTransparency = 0.25,
                })
            end)))
            UiLibraryCore:AddSignal(Y.MouseLeave:Connect(LPH_NO_VIRTUALIZE(function()
                UiLibraryCore.PlayAnimate(Y, tweenNormal, {
                    BackgroundTransparency = 1,
                })
                UiLibraryCore.PlayAnimate(x, tweenNormal, {
                    TextTransparency = 0.35,
                })
            end)))
            function b.SetText(u, B)
                local k = x.Text
                x.Text = B
                if W and k ~= B then
                    F()
                end
            end
            function b.ToolTip(u, B)
                b.ToolTip = UiLibraryCore:CreateToolTips(Y, k, B)
                return b
            end
            b.SetRender(u:GetValue())
            u:Connect(b.SetRender)
            return b
        end
        function W.AddButton(W, k)
            k = UiLibraryCore:ProcessParams(k, {
                Icon = "chevron-large-left",
                Name = "Button",
                Callback = v,
                ToolTip = nil,
            })
            local I = {}
            local Y = Instance.new("Frame")
            local q = Instance.new("TextLabel")
            local x = Instance.new("Frame")
            local o = Instance.new("UICorner")
            local i = Instance.new("TextLabel")
            UiLibraryCore:AddQuery(Y, k.Name)
            Y.Name = UiLibraryCore.RandomString()
            Y.Parent = B
            Y.BackgroundColor3 = Color3.fromRGB(25, 27, 33)
            Y.BackgroundTransparency = 1
            Y.BorderColor3 = Color3.fromRGB(0, 0, 0)
            Y.BorderSizePixel = 0
            Y.Size = UDim2.new(1, 0, 0, 30)
            Y.ZIndex = l + 8
            q.Name = UiLibraryCore.RandomString()
            q.Parent = Y
            q.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
            q.BackgroundTransparency = 1
            q.BorderColor3 = Color3.fromRGB(0, 0, 0)
            q.BorderSizePixel = 0
            q.Position = UDim2.new(0, 35, 0, 6)
            q.Size = UDim2.new(0, 1, 0, 15)
            q.ZIndex = l + 9
            q.Font = Enum.Font.GothamMedium
            q.Text = k.Name
            q.TextColor3 = Color3.fromRGB(255, 255, 255)
            q.TextSize = 13
            q.TextTransparency = 0.2
            q.TextXAlignment = Enum.TextXAlignment.Left
            x.Name = UiLibraryCore.RandomString()
            x.Parent = Y
            x.AnchorPoint = Vector2.new(0.5, 1)
            x.BackgroundColor3 = Color3.fromRGB(45, 48, 58)
            x.BackgroundTransparency = 0.65
            x.BorderColor3 = Color3.fromRGB(0, 0, 0)
            x.BorderSizePixel = 0
            x.Position = UDim2.new(0.5, 0, 1, 0)
            x.Size = UDim2.new(1, -20, 0, 1)
            x.ZIndex = l + 11
            o.CornerRadius = UDim.new(0, 10)
            o.Parent = Y
            i.Name = UiLibraryCore.RandomString()
            i.Parent = Y
            i.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
            i.BackgroundTransparency = 1
            i.BorderColor3 = Color3.fromRGB(0, 0, 0)
            i.BorderSizePixel = 0
            i.Position = UDim2.new(0, 11, 0, 5)
            i.Size = UDim2.new(0, 18, 0, 18)
            i.ZIndex = l + 9
            i.FontFace = UiLibraryCore.BuiltInBold
            i.Text = k.Icon
            i.TextColor3 = Color3.fromRGB(223, 223, 223)
            i.TextSize = 16
            i.TextTransparency = 0.25
            i.TextWrapped = true
            function I.SetText(u, B)
                q.Text = B
            end
            function I.SetIcon(u, B)
                i.Text = B
            end
            local E = UiLibraryCore:CreateInput(
                Y,
                LPH_NO_VIRTUALIZE(function()
                    k.Callback()
                end)
            )
            UiLibraryCore:AddSignal(E.MouseEnter:Connect(LPH_NO_VIRTUALIZE(function()
                UiLibraryCore.PlayAnimate(Y, tweenNormal, {
                    BackgroundTransparency = 0.35,
                })
            end)))
            UiLibraryCore:AddSignal(E.MouseLeave:Connect(LPH_NO_VIRTUALIZE(function()
                UiLibraryCore.PlayAnimate(Y, tweenNormal, {
                    BackgroundTransparency = 1,
                })
            end)))
            I.SetRender = LPH_NO_VIRTUALIZE(function(B)
                if B then
                    UiLibraryCore.PlayAnimate(Y, tweenNormal, {
                        BackgroundTransparency = 1,
                    })
                    UiLibraryCore.PlayAnimate(q, tweenNormal, {
                        TextTransparency = 0.2,
                    })
                    UiLibraryCore.PlayAnimate(x, tweenNormal, {
                        BackgroundTransparency = 0.65,
                    })
                    UiLibraryCore.PlayAnimate(i, tweenNormal, {
                        TextTransparency = 0.25,
                    })
                else
                    UiLibraryCore.PlayAnimate(Y, tweenNormal, {
                        BackgroundTransparency = 1,
                    })
                    UiLibraryCore.PlayAnimate(q, tweenNormal, {
                        TextTransparency = 1,
                    })
                    UiLibraryCore.PlayAnimate(x, tweenNormal, {
                        BackgroundTransparency = 1,
                    })
                    UiLibraryCore.PlayAnimate(i, tweenNormal, {
                        TextTransparency = 1,
                    })
                end
            end)
            if k.ToolTip then
                I.ToolTip = UiLibraryCore:CreateToolTips(Y, k.Name, k.ToolTip)
            end
            I.SetRender(u:GetValue())
            u:Connect(I.SetRender)
            I.Root = Y
            I.Label = q
            I.Icon = i
            return I
        end
        function W.AddUserFrame(Y, k, W, I)
            local q = Instance.new("Frame")
            local x = Instance.new("TextLabel")
            local o = Instance.new("Frame")
            local i = Instance.new("UICorner")
            local E = Instance.new("ImageLabel")
            local L = Instance.new("UICorner")
            local Z = Instance.new("TextLabel")
            q.Name = UiLibraryCore.RandomString()
            q.Parent = B
            q.BackgroundColor3 = Color3.fromRGB(25, 27, 33)
            q.BackgroundTransparency = 1
            q.BorderColor3 = Color3.fromRGB(0, 0, 0)
            q.BorderSizePixel = 0
            q.Size = UDim2.new(1, 0, 0, 60)
            q.ZIndex = l + 8
            x.Name = UiLibraryCore.RandomString()
            x.Parent = q
            x.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
            x.BackgroundTransparency = 1
            x.BorderColor3 = Color3.fromRGB(0, 0, 0)
            x.BorderSizePixel = 0
            x.Position = UDim2.new(0, 65, 0, 10)
            x.Size = UDim2.new(1, -35, 0, 15)
            x.ZIndex = l + 9
            x.Font = Enum.Font.GothamMedium
            x.Text = k or "User"
            x.TextColor3 = Color3.fromRGB(255, 255, 255)
            x.TextSize = 13
            x.TextTransparency = 0.2
            x.TextXAlignment = Enum.TextXAlignment.Left
            o.Name = UiLibraryCore.RandomString()
            o.Parent = q
            o.AnchorPoint = Vector2.new(0.5, 1)
            o.BackgroundColor3 = Color3.fromRGB(45, 48, 58)
            o.BackgroundTransparency = 0.65
            o.BorderColor3 = Color3.fromRGB(0, 0, 0)
            o.BorderSizePixel = 0
            o.Position = UDim2.new(0.5, 0, 1, 0)
            o.Size = UDim2.new(1, -20, 0, 1)
            o.ZIndex = l + 11
            i.CornerRadius = UDim.new(0, 10)
            i.Parent = q
            E.Name = UiLibraryCore.RandomString()
            E.Parent = q
            E.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
            E.BackgroundTransparency = 1
            E.BorderColor3 = Color3.fromRGB(0, 0, 0)
            E.BorderSizePixel = 0
            E.Position = UDim2.new(0, 10, 0, 5)
            E.Size = UDim2.new(0, 45, 0, 45)
            E.ZIndex = l + 9
            E.Image = W or "rbxasset://textures/ui/clb_robux_20@3x.png"
            L.CornerRadius = UDim.new(1, 0)
            L.Parent = E
            Z.Name = UiLibraryCore.RandomString()
            Z.Parent = q
            Z.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
            Z.BackgroundTransparency = 1
            Z.BorderColor3 = Color3.fromRGB(0, 0, 0)
            Z.BorderSizePixel = 0
            Z.Position = UDim2.new(0, 65, 0, 25)
            Z.Size = UDim2.new(1, -35, 0, 15)
            Z.ZIndex = l + 9
            Z.Font = Enum.Font.GothamMedium
            Z.Text = tostring(I or "Never")
            Z.TextColor3 = Color3.fromRGB(255, 255, 255)
            Z.TextSize = 13
            Z.TextTransparency = 0.2
            Z.TextXAlignment = Enum.TextXAlignment.Left
            UiLibraryCore:SetMemberStatusLabel(Z, I)
            local J = {}
            J.SetRender = LPH_NO_VIRTUALIZE(function(B)
                if B then
                    UiLibraryCore.PlayAnimate(x, tweenNormal, {
                        TextTransparency = 0.2,
                    })
                    UiLibraryCore.PlayAnimate(o, tweenNormal, {
                        BackgroundTransparency = 0.65,
                    })
                    UiLibraryCore.PlayAnimate(E, tweenNormal, {
                        ImageTransparency = 0,
                    })
                    UiLibraryCore.PlayAnimate(Z, tweenNormal, {
                        TextTransparency = 0.2,
                    })
                else
                    UiLibraryCore.PlayAnimate(x, tweenNormal, {
                        TextTransparency = 1,
                    })
                    UiLibraryCore.PlayAnimate(o, tweenNormal, {
                        BackgroundTransparency = 1,
                    })
                    UiLibraryCore.PlayAnimate(E, tweenNormal, {
                        ImageTransparency = 1,
                    })
                    UiLibraryCore.PlayAnimate(Z, tweenNormal, {
                        TextTransparency = 1,
                    })
                end
            end)
            J.SetRender(u:GetValue())
            u:Connect(J.SetRender)
            function J.SetUsername(u, B)
                x.Text = B or "User"
            end
            function J.SetProfile(u, B)
                E.Image = B or "rbxasset://textures/ui/clb_robux_20@3x.png"
            end
            function J.SetExpires(u, B)
                UiLibraryCore:SetMemberStatusLabel(Z, B)
            end
            return J
        end
        return W
    end
    function UiLibraryCore.CreateWindow(u, B)
        B = UiLibraryCore:ProcessParams(B, {
            Logo = UiLibraryCore.OiHubIcon,
            TitleImage = UiLibraryCore.OiHubWordmark,
            Name = "Oi Hub",
            Content = "Ui Library V2",
            Size = UDim2.new(0, 640, 0, 480),
            ConfigFolder = UiLibraryCore.ConfigNamespace,
            Enable3DRenderer = false,
            Keybind = "Insert",
            Responsive = true,
        })
        local k = {
            Logo = UiLibraryCore:ResolveImage(B.Logo, "window-icon.png", UiLibraryCore.OiHubIcon),
            TitleImage = UiLibraryCore:ResolveImage(B.TitleImage, "window-wordmark.png", UiLibraryCore.OiHubWordmark),
            Name = B.Name,
            Content = B.Content,
            Size = B.Size,
            BaseSize = B.Size,
            ConfigFolder = B.ConfigFolder,
            Signal = UiLibraryCore:CreateSignal(true),
            Tabs = {},
            CurrentTab = 1,
            Keybind = B.Keybind,
            Responsive = B.Responsive ~= false,
            Enable3DRenderer = B.Enable3DRenderer,
        }
        UiLibraryCore.GlobalLogo = k.Logo
        local W = UiLibraryCore:CreateLogger()
        if UiLibraryCore.ConfigSupported and not isfolder(k.ConfigFolder) then
            UiLibraryCore:EnsureFolder(k.ConfigFolder)
        end
        local l = Instance.new("Frame")
        local I = Instance.new("UICorner")
        local x = Instance.new("Frame")
        local o = Instance.new("Frame")
        local L = Instance.new("ImageLabel")
        local Z = Instance.new("UICorner")
        local J = Instance.new("TextLabel")
        local b = Instance.new("ImageLabel")
        local d = Instance.new("TextLabel")
        local X = Instance.new("TextLabel")
        local D = Instance.new("Frame")
        local m = Instance.new("ScrollingFrame")
        local w = Instance.new("UIListLayout")
        local v = Instance.new("Frame")
        local e = Instance.new("ImageLabel")
        local G = Instance.new("UICorner")
        local U = Instance.new("TextLabel")
        local f = Instance.new("TextLabel")
        local p = Instance.new("Frame")
        local y = Instance.new("TextLabel")
        local P = Instance.new("Frame")
        local z = Instance.new("UIStroke")
        local N = Instance.new("UICorner")
        local g = Instance.new("Frame")
        local M = Instance.new("Frame")
        local A = Instance.new("Frame")
        local O = Instance.new("UIStroke")
        local Q = Instance.new("UICorner")
        local n = Instance.new("TextLabel")
        local K = Instance.new("Frame")
        local a = Instance.new("TextLabel")
        local c = Instance.new("TextLabel")
        local C = Instance.new("Frame")
        local V = Instance.new("TextLabel")
        local S = Instance.new("TextBox")
        local r = Instance.new("Frame")
        l.Name = UiLibraryCore.RandomString()
        l.Parent = UiLibraryCore.ScreenGui
        l.AnchorPoint = Vector2.new(0.5, 0.5)
        l.BackgroundColor3 = Color3.fromRGB(8, 8, 13)
        l.BackgroundTransparency = 0.055
        l.BorderColor3 = Color3.fromRGB(0, 0, 0)
        l.BorderSizePixel = 0
        l.ClipsDescendants = true
        l.Position = UDim2.new(255, 0, 255, 0)
        l.Size = k.Size
        l.Active = true
        if not UiLibraryCore.EnabledBlur then
            l.BackgroundTransparency = 0.0255
        end
        local t = LPH_NO_VIRTUALIZE(function()
            if k.__3DRender then
                if l.BackgroundTransparency > 0.9 then
                    l.Visible = false
                    l.Parent = nil
                else
                    l.Visible = true
                    UiLibraryCore.PlayAnimate(l, tweenQuint, {
                        Position = UDim2.fromScale(0.5, 0.5),
                    })
                    l.Parent = k.SurfaceGui
                end
            else
                if l.BackgroundTransparency > 0.9 then
                    l.Visible = false
                    l.Parent = nil
                else
                    l.Visible = true
                    l.Parent = UiLibraryCore.ScreenGui
                end
            end
        end)
        UiLibraryCore:AddSignal((l:GetPropertyChangedSignal("BackgroundTransparency")):Connect(t))
        k.SetRender = LPH_NO_VIRTUALIZE(function(B, u)
            if u then
                UiLibraryCore.PlayAnimate(l, tweenNormal, {
                    BackgroundTransparency = (UiLibraryCore.EnabledBlur and 0.055) or 0.0255,
                    Size = k.Size,
                })
                UiLibraryCore.PlayAnimate(L, tweenNormal, {
                    ImageTransparency = 0,
                })
                UiLibraryCore.PlayAnimate(J, tweenNormal, {
                    TextTransparency = 0,
                })
                UiLibraryCore.PlayAnimate(b, tweenNormal, {
                    ImageTransparency = 0,
                })
                UiLibraryCore.PlayAnimate(d, tweenNormal, {
                    TextTransparency = 0.65,
                })
                UiLibraryCore.PlayAnimate(X, tweenNormal, {
                    TextTransparency = 0.05,
                })
                UiLibraryCore.PlayAnimate(D, tweenNormal, {
                    BackgroundTransparency = 0.65,
                })
                UiLibraryCore.PlayAnimate(e, tweenNormal, {
                    ImageTransparency = 0,
                })
                UiLibraryCore.PlayAnimate(U, tweenNormal, {
                    TextTransparency = 0,
                })
                UiLibraryCore.PlayAnimate(f, tweenNormal, {
                    TextTransparency = 0.12,
                })
                UiLibraryCore.PlayAnimate(p, tweenNormal, {
                    BackgroundTransparency = 0.65,
                })
                UiLibraryCore.PlayAnimate(y, tweenNormal, {
                    TextTransparency = 0.5,
                })
                UiLibraryCore.PlayAnimate(P, tweenNormal, {
                    BackgroundTransparency = 0.6,
                })
                UiLibraryCore.PlayAnimate(z, tweenNormal, {
                    Transparency = 0.65,
                })
                UiLibraryCore.PlayAnimate(M, tweenNormal, {
                    BackgroundTransparency = 0.65,
                })
                UiLibraryCore.PlayAnimate(A, tweenNormal, {
                    BackgroundTransparency = 0.75,
                })
                UiLibraryCore.PlayAnimate(O, tweenNormal, {
                    Transparency = 0.65,
                })
                UiLibraryCore.PlayAnimate(n, tweenNormal, {
                    TextTransparency = 0.25,
                })
                UiLibraryCore.PlayAnimate(K, tweenNormal, {
                    BackgroundTransparency = 0.65,
                })
                UiLibraryCore.PlayAnimate(a, tweenNormal, {
                    TextTransparency = 0.35,
                })
                UiLibraryCore.PlayAnimate(c, tweenNormal, {
                    TextTransparency = 0.25,
                })
                UiLibraryCore.PlayAnimate(V, tweenNormal, {
                    TextTransparency = 0.25,
                })
                UiLibraryCore.PlayAnimate(S, tweenNormal, {
                    TextTransparency = 0.35,
                })
                k.Shadow:Render(true)
            else
                UiLibraryCore.PlayAnimate(l, tweenNormal, {
                    BackgroundTransparency = 1,
                    Size = k.Size + UDim2.fromOffset(-15, -15),
                })
                UiLibraryCore.PlayAnimate(L, tweenNormal, {
                    ImageTransparency = 1,
                })
                UiLibraryCore.PlayAnimate(J, tweenNormal, {
                    TextTransparency = 1,
                })
                UiLibraryCore.PlayAnimate(b, tweenNormal, {
                    ImageTransparency = 1,
                })
                UiLibraryCore.PlayAnimate(d, tweenNormal, {
                    TextTransparency = 1,
                })
                UiLibraryCore.PlayAnimate(X, tweenNormal, {
                    TextTransparency = 1,
                })
                UiLibraryCore.PlayAnimate(D, tweenNormal, {
                    BackgroundTransparency = 1,
                })
                UiLibraryCore.PlayAnimate(e, tweenNormal, {
                    ImageTransparency = 1,
                })
                UiLibraryCore.PlayAnimate(U, tweenNormal, {
                    TextTransparency = 1,
                })
                UiLibraryCore.PlayAnimate(f, tweenNormal, {
                    TextTransparency = 1,
                })
                UiLibraryCore.PlayAnimate(p, tweenNormal, {
                    BackgroundTransparency = 1,
                })
                UiLibraryCore.PlayAnimate(y, tweenNormal, {
                    TextTransparency = 1,
                })
                UiLibraryCore.PlayAnimate(P, tweenNormal, {
                    BackgroundTransparency = 1,
                })
                UiLibraryCore.PlayAnimate(z, tweenNormal, {
                    Transparency = 1,
                })
                UiLibraryCore.PlayAnimate(M, tweenNormal, {
                    BackgroundTransparency = 1,
                })
                UiLibraryCore.PlayAnimate(A, tweenNormal, {
                    BackgroundTransparency = 1,
                })
                UiLibraryCore.PlayAnimate(O, tweenNormal, {
                    Transparency = 1,
                })
                UiLibraryCore.PlayAnimate(n, tweenNormal, {
                    TextTransparency = 1,
                })
                UiLibraryCore.PlayAnimate(K, tweenNormal, {
                    BackgroundTransparency = 1,
                })
                UiLibraryCore.PlayAnimate(a, tweenNormal, {
                    TextTransparency = 1,
                })
                UiLibraryCore.PlayAnimate(c, tweenNormal, {
                    TextTransparency = 1,
                })
                UiLibraryCore.PlayAnimate(V, tweenNormal, {
                    TextTransparency = 1,
                })
                UiLibraryCore.PlayAnimate(S, tweenNormal, {
                    TextTransparency = 1,
                })
                k.Shadow:Render(false)
            end
        end)
        k.Shadow = UiLibraryCore:CreateShadow(l)
        k.Shadow:Render(false)
        task.delay(0.25, function()
            l.Position = UDim2.new(0.5, 0, 0.5, 0)
            k:SetRender(k.Signal:GetValue())
            UiLibraryCore:AddSignal(k.Signal:Connect(LPH_NO_VIRTUALIZE(function(...)
                k:SetRender(...)
            end)))
        end)
        if UiLibraryCore.EnabledBlur then
            UiLibraryCore:CreateBlurModule(l, k.Signal)
        end
        do
            local B = Instance.new("Frame")
            B.Parent = l
            B.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
            B.BorderColor3 = Color3.fromRGB(0, 0, 0)
            B.BorderSizePixel = 0
            B.Size = UDim2.new(1, 0, 0, 50)
            B.ZIndex = 7
            B.BackgroundTransparency = 1
            UiLibraryCore.Drag(B, l, 0.15)
        end
        I.Parent = l
        x.Name = UiLibraryCore.RandomString()
        x.Parent = l
        x.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
        x.BackgroundTransparency = 1
        x.BorderColor3 = Color3.fromRGB(0, 0, 0)
        x.BorderSizePixel = 0
        x.Size = UDim2.new(0, 175, 1, 0)
        o.Name = UiLibraryCore.RandomString()
        o.Parent = x
        o.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
        o.BackgroundTransparency = 1
        o.BorderColor3 = Color3.fromRGB(0, 0, 0)
        o.BorderSizePixel = 0
        o.Size = UDim2.new(1, 0, 0, 50)
        o.ZIndex = 7
        L.Name = UiLibraryCore.RandomString()
        L.Parent = o
        L.AnchorPoint = Vector2.new(0.5, 0.5)
        L.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
        L.BackgroundTransparency = 1
        L.BorderColor3 = Color3.fromRGB(0, 0, 0)
        L.BorderSizePixel = 0
        L.Position = UDim2.new(0.5, 0, 0.5, 0)
        L.Size = UDim2.fromOffset(0, 0)
        L.ZIndex = 7
        L.Image = ""
        L.ImageColor3 = UiLibraryCore.IconColor
        L.ScaleType = Enum.ScaleType.Fit
        L.Visible = false
        Z.CornerRadius = UDim.new(0, 7)
        Z.Parent = L
        J.Name = UiLibraryCore.RandomString()
        J.Parent = o
        J.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
        J.BackgroundTransparency = 1
        J.BorderColor3 = Color3.fromRGB(0, 0, 0)
        J.BorderSizePixel = 0
        J.AnchorPoint = Vector2.new(0.5, 0)
        J.Position = UDim2.new(0.5, 0, 0, 4)
        J.Size = UDim2.new(1, -20, 0, 25)
        J.ZIndex = 7
        J.Font = Enum.Font.GothamBold
        J.Text = "discord.gg/winterbypass"
        J.TextColor3 = Color3.fromRGB(255, 255, 255)
        J.TextSize = 15
        J.TextXAlignment = Enum.TextXAlignment.Center
        b.Name = "OiHubWordmark"
        b.Parent = o
        b.BackgroundTransparency = 1
        b.BorderSizePixel = 0
        b.AnchorPoint = Vector2.new(0.5, 0)
        b.Position = UDim2.new(0.5, 0, 0, 5)
        b.Size = UDim2.new(0, 150, 0, 40)
        b.ZIndex = 8
        b.Visible = false
        b.Image = k.TitleImage
        b.ImageColor3 = Color3.fromRGB(255, 255, 255)
        b.ScaleType = Enum.ScaleType.Fit
        d.Name = UiLibraryCore.RandomString()
        d.Parent = o
        d.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
        d.BackgroundTransparency = 1
        d.BorderColor3 = Color3.fromRGB(0, 0, 0)
        d.BorderSizePixel = 0
        d.AnchorPoint = Vector2.new(0, 0)
        d.ZIndex = 7
        d.Font = Enum.Font.GothamBold
        local h = tostring(k.Content or "Ui Library V2")
        local j = string.match(h, "%s+(V2)$")
        d.Text = j and string.gsub(h, "%s+V2$", "") or h
        d.TextColor3 = Color3.fromRGB(255, 255, 255)
        d.TextSize = 9
        d.TextTransparency = 0.65
        d.Visible = false
        d.TextXAlignment = Enum.TextXAlignment.Left
        local B0 = (TextServiceUi:GetTextSize(d.Text, d.TextSize, d.Font, Vector2.new(math.huge, 15))).X
        local u0 = j and (TextServiceUi:GetTextSize(j, d.TextSize, d.Font, Vector2.new(math.huge, 15))).X or 0
        local k0 = j and 3 or 0
        local T0 = (B0 + u0) + k0
        d.Position = UDim2.new(0.5, -T0 / 2, 0, 27)
        d.Size = UDim2.fromOffset(B0, 15)
        X.Name = "OiHubVersionNeon"
        X.Parent = o
        X.BackgroundTransparency = 1
        X.BorderSizePixel = 0
        X.Position = UDim2.new(0.5, (-T0 / 2 + B0) + k0, 0, 27)
        X.Size = UDim2.fromOffset(u0 + 2, 15)
        X.ZIndex = 8
        X.Font = Enum.Font.GothamBold
        X.Text = j or ""
        X.TextColor3 = Color3.fromRGB(255, 255, 255)
        X.TextSize = 9
        X.TextTransparency = 0.05
        X.TextXAlignment = Enum.TextXAlignment.Left
        X.Visible = j ~= nil
        UiLibraryCore:ApplyVersionNeon(X)
        D.Name = UiLibraryCore.RandomString()
        D.Parent = o
        D.AnchorPoint = Vector2.new(0.5, 1)
        D.BackgroundColor3 = Color3.fromRGB(45, 48, 58)
        D.BackgroundTransparency = 0.65
        D.BorderColor3 = Color3.fromRGB(0, 0, 0)
        D.BorderSizePixel = 0
        D.Position = UDim2.new(0.5, 0, 1, 0)
        D.Size = UDim2.new(1, -10, 0, 1)
        D.ZIndex = 5
        m.Name = UiLibraryCore.RandomString()
        m.Parent = x
        m.Active = true
        m.AnchorPoint = Vector2.new(0.5, 0)
        m.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
        m.BackgroundTransparency = 1
        m.BorderColor3 = Color3.fromRGB(0, 0, 0)
        m.BorderSizePixel = 0
        m.Position = UDim2.new(0.5, 0, 0, 60)
        m.Size = UDim2.new(1, -10, 1, -115)
        m.ZIndex = 7
        m.ScrollBarThickness = 0
        w.Parent = m
        w.HorizontalAlignment = Enum.HorizontalAlignment.Center
        w.SortOrder = Enum.SortOrder.LayoutOrder
        w.Padding = UDim.new(0, 5)
        UiLibraryCore:AddSignal((w:GetPropertyChangedSignal("AbsoluteContentSize")):Connect(LPH_NO_VIRTUALIZE(function()
            m.CanvasSize = UDim2.fromOffset(0, w.AbsoluteContentSize.Y + 1)
        end)))
        v.Name = UiLibraryCore.RandomString()
        v.Parent = x
        v.AnchorPoint = Vector2.new(0, 1)
        v.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
        v.BackgroundTransparency = 1
        v.BorderColor3 = Color3.fromRGB(0, 0, 0)
        v.BorderSizePixel = 0
        v.Position = UDim2.new(0, 0, 1, 0)
        v.Size = UDim2.new(1, 0, 0, 50)
        v.ZIndex = 7
        e.Name = UiLibraryCore.RandomString()
        e.Parent = v
        e.AnchorPoint = Vector2.new(0, 0.5)
        e.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
        e.BackgroundTransparency = 1
        e.BorderColor3 = Color3.fromRGB(0, 0, 0)
        e.BorderSizePixel = 0
        e.Position = UDim2.new(0, 10, 0.5, 0)
        e.Size = UDim2.new(0, 35, 0, 35)
        e.ZIndex = 7
        e.Image = UiLibraryCore.UserProfile or ""
        G.CornerRadius = UDim.new(1, 0)
        G.Parent = e
        U.Name = UiLibraryCore.RandomString()
        U.Parent = v
        U.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
        U.BackgroundTransparency = 1
        U.BorderColor3 = Color3.fromRGB(0, 0, 0)
        U.BorderSizePixel = 0
        U.Position = UDim2.new(0, 55, 0, 5)
        U.Size = UDim2.new(0, 100, 0, 25)
        U.ZIndex = 7
        U.Font = Enum.Font.GothamBold
        U.Text = ""
        U.TextColor3 = Color3.fromRGB(255, 255, 255)
        U.TextSize = 14
        U.TextXAlignment = Enum.TextXAlignment.Left
        U.TextTruncate = Enum.TextTruncate.SplitWord
        f.Name = UiLibraryCore.RandomString()
        f.Parent = v
        f.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
        f.BackgroundTransparency = 1
        f.BorderColor3 = Color3.fromRGB(0, 0, 0)
        f.BorderSizePixel = 0
        f.Position = UDim2.new(0, 55, 0, 25)
        f.Size = UDim2.new(0, 200, 0, 15)
        f.ZIndex = 7
        f.Font = Enum.Font.GothamBold
        f.Text = "Never"
        f.TextColor3 = Color3.fromRGB(255, 255, 255)
        f.TextSize = 10
        f.TextTransparency = 0.12
        f.TextXAlignment = Enum.TextXAlignment.Left
        UiLibraryCore:SetMemberStatusLabel(f, "Never")
        p.Name = UiLibraryCore.RandomString()
        p.Parent = v
        p.AnchorPoint = Vector2.new(0.5, 0)
        p.BackgroundColor3 = Color3.fromRGB(45, 48, 58)
        p.BackgroundTransparency = 0.65
        p.BorderColor3 = Color3.fromRGB(0, 0, 0)
        p.BorderSizePixel = 0
        p.Position = UDim2.new(0.5, 0, 0, 0)
        p.Size = UDim2.new(1, -10, 0, 1)
        p.ZIndex = 5
        y.Name = UiLibraryCore.RandomString()
        y.Parent = v
        y.AnchorPoint = Vector2.new(1, 0.5)
        y.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
        y.BackgroundTransparency = 1
        y.BorderColor3 = Color3.fromRGB(0, 0, 0)
        y.BorderSizePixel = 0
        y.Position = UDim2.new(1, -7, 0.5, 0)
        y.Size = UDim2.new(0, 25, 0, 25)
        y.ZIndex = 7
        y.FontFace = UiLibraryCore.BuiltInBold
        y.Text = "chevron-large-right"
        y.TextColor3 = Color3.fromRGB(255, 255, 255)
        y.TextSize = 13
        y.TextTransparency = 0.5
        UiLibraryCore:AddSignal(v.MouseEnter:Connect(LPH_NO_VIRTUALIZE(function()
            UiLibraryCore.PlayAnimate(y, tweenNormal, {
                TextTransparency = 0.25,
            })
        end)))
        UiLibraryCore:AddSignal(v.MouseLeave:Connect(LPH_NO_VIRTUALIZE(function()
            UiLibraryCore.PlayAnimate(y, tweenNormal, {
                TextTransparency = 0.5,
            })
        end)))
        P.Name = UiLibraryCore.RandomString()
        P.Parent = l
        P.BackgroundColor3 = Color3.fromRGB(8, 8, 13)
        P.BackgroundTransparency = 0.6
        P.BorderColor3 = Color3.fromRGB(0, 0, 0)
        P.BorderSizePixel = 0
        P.ClipsDescendants = true
        P.Position = UDim2.new(0, 176, 0, 0)
        P.Size = UDim2.new(1, -176, 1, 0)
        P.ZIndex = 8
        z.Transparency = 0.65
        z.Color = Color3.fromRGB(45, 48, 58)
        z.Parent = P
        N.CornerRadius = UDim.new(0, 13)
        N.Parent = P
        g.Name = UiLibraryCore.RandomString()
        g.Parent = P
        g.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
        g.BackgroundTransparency = 1
        g.BorderColor3 = Color3.fromRGB(0, 0, 0)
        g.BorderSizePixel = 0
        g.Size = UDim2.new(1, 0, 0, 50)
        g.ZIndex = 9
        M.Name = UiLibraryCore.RandomString()
        M.Parent = g
        M.AnchorPoint = Vector2.new(0.5, 1)
        M.BackgroundColor3 = Color3.fromRGB(45, 48, 58)
        M.BackgroundTransparency = 0.65
        M.BorderColor3 = Color3.fromRGB(0, 0, 0)
        M.BorderSizePixel = 0
        M.Position = UDim2.new(0.5, 0, 1, 0)
        M.Size = UDim2.new(1, -10, 0, 1)
        M.ZIndex = 9
        A.Name = UiLibraryCore.RandomString()
        A.Parent = g
        A.AnchorPoint = Vector2.new(0, 0.5)
        A.BackgroundColor3 = Color3.fromRGB(13, 17, 22)
        A.BackgroundTransparency = 0.75
        A.BorderColor3 = Color3.fromRGB(0, 0, 0)
        A.BorderSizePixel = 0
        A.Position = UDim2.new(0, 10, 0.5, 0)
        A.Size = UDim2.new(0, 115, 0, 30)
        A.ZIndex = 9
        O.Transparency = 0.65
        O.Color = Color3.fromRGB(45, 48, 58)
        O.Parent = A
        Q.CornerRadius = UDim.new(0, 4)
        Q.Parent = A
        n.Name = UiLibraryCore.RandomString()
        n.Parent = A
        n.AnchorPoint = Vector2.new(0, 0.5)
        n.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
        n.BackgroundTransparency = 1
        n.BorderColor3 = Color3.fromRGB(0, 0, 0)
        n.BorderSizePixel = 0
        n.Position = UDim2.new(0, 2, 0.5, 0)
        n.Size = UDim2.new(0, 25, 0, 25)
        n.ZIndex = 9
        n.FontFace = UiLibraryCore.BuiltInBold
        n.Text = "floppy-disk"
        n.TextColor3 = Color3.fromRGB(223, 223, 223)
        n.TextSize = 16
        n.TextTransparency = 0.25
        n.TextWrapped = true
        K.Name = UiLibraryCore.RandomString()
        K.Parent = A
        K.BackgroundColor3 = Color3.fromRGB(45, 48, 58)
        K.BackgroundTransparency = 0.65
        K.BorderColor3 = Color3.fromRGB(0, 0, 0)
        K.BorderSizePixel = 0
        K.Position = UDim2.new(0, 30, 0, 0)
        K.Size = UDim2.new(0, 1, 1, 0)
        a.Name = UiLibraryCore.RandomString()
        a.Parent = A
        a.AnchorPoint = Vector2.new(0, 0.5)
        a.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
        a.BackgroundTransparency = 1
        a.BorderColor3 = Color3.fromRGB(0, 0, 0)
        a.BorderSizePixel = 0
        a.Position = UDim2.new(0, 40, 0.5, 0)
        a.Size = UDim2.new(1, -7, 0, 15)
        a.ZIndex = 9
        a.Font = Enum.Font.GothamMedium
        a.Text = "Default"
        a.TextColor3 = Color3.fromRGB(255, 255, 255)
        a.TextSize = 12
        a.TextTransparency = 0.35
        a.TextXAlignment = Enum.TextXAlignment.Left
        c.Name = UiLibraryCore.RandomString()
        c.Parent = A
        c.AnchorPoint = Vector2.new(1, 0.5)
        c.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
        c.BackgroundTransparency = 1
        c.BorderColor3 = Color3.fromRGB(0, 0, 0)
        c.BorderSizePixel = 0
        c.Position = UDim2.new(1, -2, 0.5, 0)
        c.Size = UDim2.new(0, 25, 0, 25)
        c.ZIndex = 9
        c.FontFace = UiLibraryCore.BuiltInBold
        c.Text = "chevron-small-down"
        c.TextColor3 = Color3.fromRGB(223, 223, 223)
        c.TextSize = 16
        c.TextTransparency = 0.25
        c.TextWrapped = true
        C.Name = UiLibraryCore.RandomString()
        C.Parent = g
        C.AnchorPoint = Vector2.new(1, 0.5)
        C.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
        C.BackgroundTransparency = 1
        C.BorderColor3 = Color3.fromRGB(0, 0, 0)
        C.BorderSizePixel = 0
        C.ClipsDescendants = true
        C.Position = UDim2.new(1, -10, 0.5, 0)
        C.Size = UDim2.new(0, 30, 0, 30)
        C.ZIndex = 12
        V.Name = UiLibraryCore.RandomString()
        V.Parent = C
        V.AnchorPoint = Vector2.new(0, 0.5)
        V.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
        V.BackgroundTransparency = 1
        V.BorderColor3 = Color3.fromRGB(0, 0, 0)
        V.BorderSizePixel = 0
        V.Position = UDim2.new(0, 2, 0.5, 0)
        V.Size = UDim2.new(0, 25, 0, 25)
        V.ZIndex = 12
        V.FontFace = UiLibraryCore.BuiltInBold
        V.Text = "magnifying-glass"
        V.TextColor3 = Color3.fromRGB(223, 223, 223)
        V.TextSize = 14
        V.TextTransparency = 0.45
        V.TextWrapped = true
        S.Name = UiLibraryCore.RandomString()
        S.Parent = C
        S.AnchorPoint = Vector2.new(0, 0.5)
        S.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
        S.BackgroundTransparency = 1
        S.BorderColor3 = Color3.fromRGB(0, 0, 0)
        S.BorderSizePixel = 0
        S.Position = UDim2.new(0, 35, 0.5, 0)
        S.Size = UDim2.new(1, -35, 0, 25)
        S.ZIndex = 12
        S.ClearTextOnFocus = false
        S.Font = Enum.Font.GothamMedium
        S.PlaceholderText = "Search"
        S.Text = ""
        S.TextColor3 = Color3.fromRGB(255, 255, 255)
        S.TextSize = 13
        S.TextTransparency = 1
        S.TextXAlignment = Enum.TextXAlignment.Left
        r.Name = UiLibraryCore.RandomString()
        r.Parent = P
        r.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
        r.BackgroundTransparency = 1
        r.BorderColor3 = Color3.fromRGB(0, 0, 0)
        r.BorderSizePixel = 0
        r.ClipsDescendants = true
        r.Position = UDim2.new(0, 0, 0, 50)
        r.Size = UDim2.new(1, 0, 1, -50)
        r.ZIndex = 5
        do
            k.Searching = false
            local B = UiLibraryCore:CreateInput(
                V,
                LPH_NO_VIRTUALIZE(function()
                    k.Searching = not k.Searching
                    if k.Searching then
                        UiLibraryCore.PlayAnimate(C, tweenQuint, {
                            Size = UDim2.new(0, 220, 0, 30),
                        })
                        UiLibraryCore.PlayAnimate(V, tweenNormal, {
                            TextTransparency = 0.25,
                        })
                        UiLibraryCore.PlayAnimate(S, tweenQuint, {
                            TextTransparency = 0.35,
                        })
                    else
                        UiLibraryCore.PlayAnimate(C, tweenQuint, {
                            Size = UDim2.new(0, 30, 0, 30),
                        })
                        UiLibraryCore.PlayAnimate(V, tweenNormal, {
                            TextTransparency = 0.45,
                        })
                        UiLibraryCore.PlayAnimate(S, tweenNormal, {
                            TextTransparency = 1,
                        })
                        S.Text = ""
                    end
                end)
            )
            local u = tick()
            local W
            local l = 0.2
            UiLibraryCore:AddSignal((S:GetPropertyChangedSignal("Text")):Connect(LPH_NO_VIRTUALIZE(function()
                if not S.Text:byte() then
                    for B, u in next, UiLibraryCore.NameRegisitry do
                        u.Root.Visible = true
                    end
                    return
                end
                u = tick()
                if W then
                    task.cancel(W)
                    W = nil
                end
                W = task.delay(l, function()
                    if S.Text:byte() and (tick() - u) > l then
                        for B, u in next, UiLibraryCore.NameRegisitry do
                            if string.find(string.lower(u.Idx), string.lower(S.Text), 1, true) then
                                u.Root.Visible = true
                            else
                                u.Root.Visible = false
                            end
                        end
                    end
                end)
            end)))
            UiLibraryCore:AddSignal(B.MouseEnter:Connect(LPH_NO_VIRTUALIZE(function()
                UiLibraryCore.PlayAnimate(V, tweenNormal, {
                    TextTransparency = 0.25,
                })
            end)))
            UiLibraryCore:AddSignal(B.MouseLeave:Connect(LPH_NO_VIRTUALIZE(function()
                if k.Searching then
                    UiLibraryCore.PlayAnimate(V, tweenNormal, {
                        TextTransparency = 0.25,
                    })
                else
                    UiLibraryCore.PlayAnimate(V, tweenNormal, {
                        TextTransparency = 0.45,
                    })
                end
            end)))
        end
        if k.Enable3DRenderer then
            local B = Instance.new("Part")
            B.Name = UiLibraryCore.RandomString()
            B.Anchored = true
            B.Transparency = 1
            B.CanCollide = false
            B.CanTouch = false
            B.AudioCanCollide = false
            B.CollisionGroup = UiLibraryCore.RandomString()
            B.CFrame = CFrame.new(0, 0, 0)
            B.Size = Vector3.zero
            local u = Instance.new("SurfaceGui")
            u.Parent = UiLibraryCore.ScreenGui
            u.Adornee = B
            u.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
            u.AlwaysOnTop = true
            u.LightInfluence = 1
            u.ZIndexBehavior = Enum.ZIndexBehavior.Global
            u.SizingMode = Enum.SurfaceGuiSizingMode.FixedSize
            u.PixelsPerStud = 40
            k.SurfaceGui = u
            UiLibraryCore.GlobalSurfaceGui = u
            local W = Vector2.new(1920, 1380)
            k.Load3DBlock = LPH_NO_VIRTUALIZE(function()
                if not k.Signal:GetValue() then
                    local u, k = currentCameraUi:WorldToViewportPoint(B.Position)
                    if k then
                        UiLibraryCore.PlayAnimate(B, tweenQuint, {
                            CFrame = (currentCameraUi.CFrame * CFrame.new(0, 0, -15))
                                * CFrame.Angles(0, math.rad(180), 0),
                        })
                    end
                    return
                end
                local u = 50
                local l = Vector2.new(W.X + 5, W.Y * 1.35) / (u / 2)
                local I = l.Magnitude
                local Y = I / 1.35
                B.Parent = UiLibraryCore.BlurModuleParent or workspace
                UiLibraryCore.PlayAnimate(B, tweenQuint, {
                    CFrame = (currentCameraUi.CFrame * CFrame.new(0, 0, -25)) * CFrame.Angles(0, math.rad(180), 0),
                })
                B.Size = Vector3.new(W.X / Y, W.Y / Y, 0)
            end)
            function k.Set3DRender(W, u)
                k.__3DRender = u
                UiLibraryCore.Global3DRenderMode = u
                if u then
                    k.Load3DBlock()
                else
                    B.Parent = nil
                end
                t()
            end
        end
        function k.AddTabLabel(u, B)
            local W = Instance.new("TextLabel")
            local l = Instance.new("Frame")
            W.Name = UiLibraryCore.RandomString()
            W.Parent = m
            W.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
            W.BackgroundTransparency = 1
            W.BorderColor3 = Color3.fromRGB(0, 0, 0)
            W.BorderSizePixel = 0
            W.Size = UDim2.new(1, -7, 0, 15)
            W.ZIndex = 8
            W.Font = Enum.Font.GothamMedium
            W.Text = B
            W.TextColor3 = Color3.fromRGB(255, 255, 255)
            W.TextSize = 11
            W.TextTransparency = 0.5
            W.TextXAlignment = Enum.TextXAlignment.Left
            local I = (TextServiceUi:GetTextSize(B, W.TextSize, W.Font, Vector2.new(math.huge, 15))).X
            l.Name = "TabDividerLine"
            l.Parent = W
            l.AnchorPoint = Vector2.new(0, 0.5)
            l.Position = UDim2.new(0, I + 8, 0.5, 0)
            l.Size = UDim2.new(1, -(I + 18), 0, 1)
            l.BackgroundColor3 = Color3.fromRGB(45, 48, 58)
            l.BackgroundTransparency = 0.65
            l.BorderSizePixel = 0
            l.ZIndex = 8
            local Y = LPH_NO_VIRTUALIZE(function(B)
                if B then
                    UiLibraryCore.PlayAnimate(W, tweenNormal, {
                        TextTransparency = 0.5,
                    })
                    UiLibraryCore.PlayAnimate(l, tweenNormal, {
                        BackgroundTransparency = 0.65,
                    })
                else
                    UiLibraryCore.PlayAnimate(W, tweenNormal, {
                        TextTransparency = 1,
                    })
                    UiLibraryCore.PlayAnimate(l, tweenNormal, {
                        BackgroundTransparency = 1,
                    })
                end
            end)
            Y(k.Signal:GetValue())
            return k.Signal:Connect(Y)
        end
        function k.AddTab(u, B)
            B = UiLibraryCore:ProcessParams(B, {
                Icon = "crosshairs",
                Name = "Tab",
                Type = "Double",
            })
            local W = {
                Signal = UiLibraryCore:CreateSignal(false),
                Sections = {},
            }
            local l = Instance.new("Frame")
            local I = Instance.new("UICorner")
            local Y = Instance.new("TextLabel")
            local q = Instance.new("TextLabel")
            W.Idx = l
            l.Name = UiLibraryCore.RandomString()
            l.Parent = m
            l.BackgroundColor3 = Color3.fromRGB(41, 45, 49)
            l.BackgroundTransparency = 0.5
            l.BorderColor3 = Color3.fromRGB(0, 0, 0)
            l.BorderSizePixel = 0
            l.Size = UDim2.new(1, -1, 0, 30)
            l.ZIndex = 8
            I.CornerRadius = UDim.new(0, 6)
            I.Parent = l
            Y.Name = UiLibraryCore.RandomString()
            Y.Parent = l
            Y.AnchorPoint = Vector2.new(0, 0.5)
            Y.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
            Y.BackgroundTransparency = 1
            Y.BorderColor3 = Color3.fromRGB(0, 0, 0)
            Y.BorderSizePixel = 0
            Y.Position = UDim2.new(0, 2, 0.5, 0)
            Y.Size = UDim2.new(0, 25, 0, 25)
            Y.ZIndex = 9
            Y.FontFace = UiLibraryCore.BuiltInBold
            Y.Text = B.Icon
            Y.TextColor3 = UiLibraryCore.AccentColor
            Y.TextSize = 16
            Y.TextWrapped = true
            q.Name = UiLibraryCore.RandomString()
            q.Parent = l
            q.AnchorPoint = Vector2.new(0, 0.5)
            q.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
            q.BackgroundTransparency = 1
            q.BorderColor3 = Color3.fromRGB(0, 0, 0)
            q.BorderSizePixel = 0
            q.Position = UDim2.new(0, 30, 0.5, 0)
            q.Size = UDim2.new(1, -7, 0, 15)
            q.ZIndex = 9
            q.Font = Enum.Font.GothamMedium
            q.Text = B.Name
            q.TextColor3 = Color3.fromRGB(255, 255, 255)
            q.TextSize = 12
            q.TextXAlignment = Enum.TextXAlignment.Left
            local x = Instance.new("Frame")
            local o = Instance.new("ScrollingFrame")
            local i = Instance.new("UIListLayout")
            local E = Instance.new("ScrollingFrame")
            local L = Instance.new("UIListLayout")
            x.Name = UiLibraryCore.RandomString()
            x.Parent = r
            x.AnchorPoint = Vector2.new(0.5, 0.5)
            x.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
            x.BackgroundTransparency = 1
            x.BorderColor3 = Color3.fromRGB(0, 0, 0)
            x.BorderSizePixel = 0
            x.ClipsDescendants = true
            x.Position = UDim2.new(0.5, 0, 0.5, 0)
            x.Size = UDim2.new(1, 0, 1, 0)
            x.Visible = true
            o.Name = UiLibraryCore.RandomString()
            o.Parent = x
            o.Active = true
            o.AnchorPoint = Vector2.new(0.5, 0.5)
            o.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
            o.BackgroundTransparency = 1
            o.BorderColor3 = Color3.fromRGB(0, 0, 0)
            o.BorderSizePixel = 0
            o.ClipsDescendants = false
            o.Position = UDim2.new(0.25, 0, 0.5, 0)
            o.Size = UDim2.new(0.5, 0, 1, -5)
            o.ScrollBarThickness = 0
            i.Parent = o
            i.HorizontalAlignment = Enum.HorizontalAlignment.Right
            i.SortOrder = Enum.SortOrder.LayoutOrder
            i.Padding = UDim.new(0, 5)
            UiLibraryCore:AddSignal(
                (i:GetPropertyChangedSignal("AbsoluteContentSize")):Connect(LPH_NO_VIRTUALIZE(function()
                    o.CanvasSize = UDim2.fromOffset(0, i.AbsoluteContentSize.Y + 1)
                end))
            )
            E.Name = UiLibraryCore.RandomString()
            E.Parent = x
            E.Active = true
            E.AnchorPoint = Vector2.new(0.5, 0.5)
            E.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
            E.BackgroundTransparency = 1
            E.BorderColor3 = Color3.fromRGB(0, 0, 0)
            E.BorderSizePixel = 0
            E.ClipsDescendants = false
            E.Position = UDim2.new(0.75, 0, 0.5, 0)
            E.Size = UDim2.new(0.5, 0, 1, -5)
            E.ScrollBarThickness = 0
            L.Parent = E
            L.SortOrder = Enum.SortOrder.LayoutOrder
            L.Padding = UDim.new(0, 5)
            if B.Type == "Single" then
                L:Destroy()
                E:Destroy()
                E = o
                L = i
                o.Size = UDim2.new(1, 0, 1, -5)
                o.Position = UDim2.new(0.5, 0, 0.5, 0)
            else
                UiLibraryCore:AddSignal(
                    (L:GetPropertyChangedSignal("AbsoluteContentSize")):Connect(LPH_NO_VIRTUALIZE(function()
                        E.CanvasSize = UDim2.fromOffset(0, L.AbsoluteContentSize.Y + 1)
                    end))
                )
            end
            local function Z()
                if B.Type == "Single" or not x.Parent then
                    return
                end
                local u = x.AbsoluteSize.X < 500
                if W.Compact == u then
                    return
                end
                W.Compact = u
                if u then
                    o.Position = UDim2.new(0.5, 0, 0.5, 0)
                    o.Size = UDim2.new(1, 0, 1, -5)
                    E.Visible = false
                    for B, u in ipairs(W.Sections) do
                        u.Frame.Parent = o
                    end
                else
                    o.Position = UDim2.new(0.25, 0, 0.5, 0)
                    o.Size = UDim2.new(0.5, 0, 1, -5)
                    E.Position = UDim2.new(0.75, 0, 0.5, 0)
                    E.Size = UDim2.new(0.5, 0, 1, -5)
                    E.Visible = true
                    for B, u in ipairs(W.Sections) do
                        u.Frame.Parent = u.Column == "left" and o or E
                    end
                end
            end
            W.UpdateResponsiveLayout = Z
            UiLibraryCore:AddSignal((x:GetPropertyChangedSignal("AbsoluteSize")):Connect(Z))
            task.defer(Z)
            UiLibraryCore:AddSignal(
                (Y:GetPropertyChangedSignal("TextTransparency")):Connect(LPH_NO_VIRTUALIZE(function()
                    if Y.TextTransparency > 0.4 then
                        i.Parent = nil
                        L.Parent = nil
                        x.Visible = false
                        x.Parent = nil
                    else
                        i.Parent = o
                        L.Parent = E
                        x.Visible = true
                        x.Parent = r
                    end
                end))
            )
            W.SetValue = LPH_NO_VIRTUALIZE(function(B)
                W.Signal:SetValue(B)
                if B then
                    UiLibraryCore.PlayAnimate(l, tweenNormal, {
                        BackgroundTransparency = 0.5,
                    })
                    UiLibraryCore.PlayAnimate(Y, tweenNormal, {
                        TextTransparency = 0,
                        TextColor3 = UiLibraryCore.AccentColor,
                    })
                    UiLibraryCore.PlayAnimate(q, tweenNormal, {
                        TextTransparency = 0,
                    })
                else
                    UiLibraryCore.PlayAnimate(l, tweenNormal, {
                        BackgroundTransparency = 1,
                    })
                    UiLibraryCore.PlayAnimate(Y, tweenNormal, {
                        TextTransparency = 0.5,
                        TextColor3 = Color3.fromRGB(252, 252, 252),
                    })
                    UiLibraryCore.PlayAnimate(q, tweenNormal, {
                        TextTransparency = 0.5,
                    })
                end
            end)
            table.insert(k.Tabs, W)
            if k.Tabs[k.CurrentTab] == W then
                W.SetValue(true)
            else
                W.SetValue(false)
            end
            local J = UiLibraryCore:CreateInput(
                l,
                LPH_NO_VIRTUALIZE(function()
                    for B, u in next, k.Tabs do
                        if u.Idx == l then
                            u.SetValue(true)
                            k.CurrentTab = B
                        else
                            u.SetValue(false)
                        end
                    end
                end)
            )
            UiLibraryCore:AddSignal(J.MouseEnter:Connect(LPH_NO_VIRTUALIZE(function()
                if k.Tabs[k.CurrentTab] == W then
                    UiLibraryCore.PlayAnimate(l, tweenNormal, {
                        BackgroundTransparency = 0.5,
                    })
                else
                    UiLibraryCore.PlayAnimate(l, tweenNormal, {
                        BackgroundTransparency = 0.8,
                    })
                end
            end)))
            UiLibraryCore:AddSignal(J.MouseLeave:Connect(LPH_NO_VIRTUALIZE(function()
                if k.Tabs[k.CurrentTab] == W then
                    UiLibraryCore.PlayAnimate(l, tweenNormal, {
                        BackgroundTransparency = 0.5,
                    })
                else
                    UiLibraryCore.PlayAnimate(l, tweenNormal, {
                        BackgroundTransparency = 1,
                    })
                end
            end)))
            k.Signal:Connect(LPH_NO_VIRTUALIZE(function(B)
                if B then
                    if k.Tabs[k.CurrentTab] == W then
                        W.SetValue(true)
                    else
                        W.SetValue(false)
                    end
                else
                    W.SetValue(false)
                    UiLibraryCore.PlayAnimate(l, tweenNormal, {
                        BackgroundTransparency = 1,
                    })
                    UiLibraryCore.PlayAnimate(Y, tweenNormal, {
                        TextTransparency = 1,
                    })
                    UiLibraryCore.PlayAnimate(q, tweenNormal, {
                        TextTransparency = 1,
                    })
                end
            end))
            function W.AddSection(u, B)
                B = UiLibraryCore:ProcessParams(B, {
                    Name = "SECTION",
                    Position = "left",
                })
                local k = Instance.new("Frame")
                local l = Instance.new("TextLabel")
                local I = Instance.new("Frame")
                local Y = Instance.new("UIStroke")
                local q = Instance.new("UICorner")
                local x = Instance.new("UIListLayout")
                local i = string.lower(B.Position) == "left" and "left" or "right"
                k.Name = UiLibraryCore.RandomString()
                k.Parent = (W.Compact or i == "left") and o or E
                k.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
                k.BackgroundTransparency = 1
                k.BorderColor3 = Color3.fromRGB(0, 0, 0)
                k.BorderSizePixel = 0
                k.ClipsDescendants = true
                k.Size = UDim2.new(1, -5, 0, 0)
                k.ZIndex = 9
                k.LayoutOrder = #W.Sections + 1
                table.insert(W.Sections, {
                    Frame = k,
                    Column = i,
                })
                l.Name = UiLibraryCore.RandomString()
                l.Parent = k
                l.AnchorPoint = Vector2.new(0.5, 0)
                l.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
                l.BackgroundTransparency = 1
                l.BorderColor3 = Color3.fromRGB(0, 0, 0)
                l.BorderSizePixel = 0
                l.Position = UDim2.new(0.5, 0, 0, 0)
                l.Size = UDim2.new(1, -35, 0, 15)
                l.ZIndex = 9
                l.Font = Enum.Font.GothamMedium
                l.Text = B.Name
                l.TextColor3 = Color3.fromRGB(255, 255, 255)
                l.TextSize = 11
                l.TextTransparency = 0.5
                l.TextXAlignment = Enum.TextXAlignment.Left
                I.Name = UiLibraryCore.RandomString()
                I.Parent = k
                I.AnchorPoint = Vector2.new(0.5, 0)
                I.BackgroundColor3 = Color3.fromRGB(20, 22, 27)
                I.BackgroundTransparency = 0.5
                I.BorderColor3 = Color3.fromRGB(0, 0, 0)
                I.BorderSizePixel = 0
                I.ClipsDescendants = true
                I.Position = UDim2.new(0.5, 0, 0, 20)
                I.Size = UDim2.new(1, -10, 1, -21)
                I.ZIndex = 9
                Y.Transparency = 0.65
                Y.Color = Color3.fromRGB(45, 48, 58)
                Y.Parent = I
                q.CornerRadius = UDim.new(0, 10)
                q.Parent = I
                x.Parent = I
                x.HorizontalAlignment = Enum.HorizontalAlignment.Center
                x.SortOrder = Enum.SortOrder.LayoutOrder;
                (x:GetPropertyChangedSignal("AbsoluteContentSize")):Connect(LPH_NO_VIRTUALIZE(function()
                    if x.AbsoluteContentSize.Y <= 1 then
                        UiLibraryCore.PlayAnimate(k, tweenQuint, {
                            Size = UDim2.new(1, -5, 0, 0),
                        })
                    else
                        UiLibraryCore.PlayAnimate(k, tweenQuint, {
                            Size = UDim2.new(1, -5, 0, x.AbsoluteContentSize.Y + 19.5),
                        })
                    end
                end))
                local L = UiLibraryCore:RegisiterItem(I, W.Signal)
                L.Root = I
                L.Frame = k
                L.SetRender = LPH_NO_VIRTUALIZE(function(B)
                    if B then
                        UiLibraryCore.PlayAnimate(l, tweenNormal, {
                            TextTransparency = 0.5,
                        })
                        UiLibraryCore.PlayAnimate(I, tweenNormal, {
                            BackgroundTransparency = 0.5,
                        })
                        UiLibraryCore.PlayAnimate(Y, tweenNormal, {
                            Transparency = 0.65,
                        })
                    else
                        UiLibraryCore.PlayAnimate(l, tweenNormal, {
                            TextTransparency = 1,
                        })
                        UiLibraryCore.PlayAnimate(I, tweenNormal, {
                            BackgroundTransparency = 1,
                        })
                        UiLibraryCore.PlayAnimate(Y, tweenNormal, {
                            Transparency = 1,
                        })
                    end
                end)
                L.SetRender(W.Signal:GetValue())
                W.Signal:Connect(L.SetRender)
                return L
            end
            return W
        end
        function k._InitConfig(B)
            local u = UiLibraryCore:CreateSignal(false)
            local l = {
                Signals = {},
            }
            local I = Instance.new("Frame")
            local x = Instance.new("UICorner")
            local o = Instance.new("UIListLayout")
            local E = Instance.new("UIStroke")
            local L = Instance.new("Frame")
            local Z = Instance.new("TextLabel")
            local J = Instance.new("Frame")
            local F = Instance.new("Frame")
            local b = Instance.new("UIListLayout")
            local d = Instance.new("Frame")
            local X = Instance.new("UICorner")
            local D = Instance.new("UIStroke")
            local H = Instance.new("TextBox")
            local m = Instance.new("Frame")
            local w = Instance.new("TextLabel")
            local v = Instance.new("UICorner")
            local e = Instance.new("UICorner")
            local G = UiLibraryCore:CreateShadow(I)
            l.SetRender = LPH_NO_VIRTUALIZE(function(B)
                if B then
                    I.Position = UDim2.fromOffset(A.AbsolutePosition.X + 110, A.AbsolutePosition.Y + 96)
                    UiLibraryCore.PlayAnimate(I, tweenNormal, {
                        BackgroundTransparency = 0.035,
                        Position = UDim2.fromOffset(A.AbsolutePosition.X + 110, A.AbsolutePosition.Y + 95),
                    })
                    UiLibraryCore.PlayAnimate(E, tweenNormal, {
                        Transparency = 0.65,
                    })
                    UiLibraryCore.PlayAnimate(Z, tweenNormal, {
                        TextTransparency = 0.2,
                    })
                    UiLibraryCore.PlayAnimate(D, tweenNormal, {
                        Transparency = 0.65,
                    })
                    UiLibraryCore.PlayAnimate(J, tweenNormal, {
                        BackgroundTransparency = 0.65,
                    })
                    UiLibraryCore.PlayAnimate(d, tweenNormal, {
                        BackgroundTransparency = 0,
                    })
                    UiLibraryCore.PlayAnimate(H, tweenNormal, {
                        TextTransparency = 0.35,
                    })
                    UiLibraryCore.PlayAnimate(w, tweenNormal, {
                        TextTransparency = 0.35,
                    })
                    UiLibraryCore.PlayAnimate(c, tweenNormal, {
                        Rotation = 180,
                    })
                    G:Render(true)
                else
                    UiLibraryCore.PlayAnimate(c, tweenNormal, {
                        Rotation = 0,
                    })
                    UiLibraryCore.PlayAnimate(I, tweenNormal, {
                        BackgroundTransparency = 1,
                        Position = UDim2.fromOffset(A.AbsolutePosition.X + 110, A.AbsolutePosition.Y + 96),
                    })
                    UiLibraryCore.PlayAnimate(D, tweenNormal, {
                        Transparency = 1,
                    })
                    UiLibraryCore.PlayAnimate(E, tweenNormal, {
                        Transparency = 1,
                    })
                    UiLibraryCore.PlayAnimate(Z, tweenNormal, {
                        TextTransparency = 1,
                    })
                    UiLibraryCore.PlayAnimate(J, tweenNormal, {
                        BackgroundTransparency = 1,
                    })
                    UiLibraryCore.PlayAnimate(d, tweenNormal, {
                        BackgroundTransparency = 1,
                    })
                    UiLibraryCore.PlayAnimate(H, tweenNormal, {
                        TextTransparency = 1,
                    })
                    UiLibraryCore.PlayAnimate(w, tweenNormal, {
                        TextTransparency = 1,
                    })
                    G:Render(false)
                end
            end)
            UiLibraryCore:AddSignal(
                (I:GetPropertyChangedSignal("BackgroundTransparency")):Connect(LPH_NO_VIRTUALIZE(function()
                    if I.BackgroundTransparency > 0.9 then
                        I.Visible = false
                        o.Parent = nil
                        I.Parent = nil
                    else
                        I.Visible = true
                        o.Parent = I
                        if UiLibraryCore.Global3DRenderMode then
                            I.Parent = UiLibraryCore.GlobalSurfaceGui
                        else
                            I.Parent = UiLibraryCore.ScreenGui
                        end
                    end
                end))
            )
            I.Name = UiLibraryCore.RandomString()
            I.Parent = UiLibraryCore.ScreenGui
            I.AnchorPoint = Vector2.new(0.5, 0)
            I.BackgroundColor3 = Color3.fromRGB(20, 22, 27)
            I.BackgroundTransparency = 0.035
            I.BorderColor3 = Color3.fromRGB(0, 0, 0)
            I.BorderSizePixel = 0
            I.ClipsDescendants = true
            I.Position = UDim2.new(255, 255, 255, 255)
            I.Size = UDim2.new(0, 220, 0, 110)
            I.ZIndex = 151
            x.CornerRadius = UDim.new(0, 10)
            x.Parent = I
            o.Parent = I
            o.HorizontalAlignment = Enum.HorizontalAlignment.Center
            o.SortOrder = Enum.SortOrder.LayoutOrder
            o.Padding = UDim.new(0, 4)
            E.Transparency = 0.65
            E.Color = Color3.fromRGB(45, 48, 58)
            E.Parent = I
            L.Name = UiLibraryCore.RandomString()
            L.Parent = I
            L.BackgroundColor3 = Color3.fromRGB(25, 27, 33)
            L.BackgroundTransparency = 1
            L.BorderColor3 = Color3.fromRGB(0, 0, 0)
            L.BorderSizePixel = 0
            L.Size = UDim2.new(1, 0, 0, 30)
            L.ZIndex = 154
            Z.Name = UiLibraryCore.RandomString()
            Z.Parent = L
            Z.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
            Z.BackgroundTransparency = 1
            Z.BorderColor3 = Color3.fromRGB(0, 0, 0)
            Z.BorderSizePixel = 0
            Z.Position = UDim2.new(0, 11, 0, 6)
            Z.Size = UDim2.new(0, 1, 0, 15)
            Z.ZIndex = 154
            Z.Font = Enum.Font.GothamMedium
            Z.Text = "Config"
            Z.TextColor3 = Color3.fromRGB(255, 255, 255)
            Z.TextSize = 13
            Z.TextTransparency = 0.2
            Z.TextXAlignment = Enum.TextXAlignment.Left
            J.Name = UiLibraryCore.RandomString()
            J.Parent = L
            J.AnchorPoint = Vector2.new(0.5, 1)
            J.BackgroundColor3 = Color3.fromRGB(45, 48, 58)
            J.BackgroundTransparency = 0.65
            J.BorderColor3 = Color3.fromRGB(0, 0, 0)
            J.BorderSizePixel = 0
            J.Position = UDim2.new(0.5, 0, 1, 0)
            J.Size = UDim2.new(1, -20, 0, 1)
            J.ZIndex = 154
            F.Name = UiLibraryCore.RandomString()
            F.Parent = L
            F.AnchorPoint = Vector2.new(1, 0)
            F.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
            F.BackgroundTransparency = 1
            F.BorderColor3 = Color3.fromRGB(0, 0, 0)
            F.BorderSizePixel = 0
            F.Position = UDim2.new(1, -11, 0, 2)
            F.Size = UDim2.new(1, -20, 0, 25)
            F.ZIndex = 154
            b.Parent = F
            b.FillDirection = Enum.FillDirection.Horizontal
            b.HorizontalAlignment = Enum.HorizontalAlignment.Right
            b.SortOrder = Enum.SortOrder.LayoutOrder
            b.VerticalAlignment = Enum.VerticalAlignment.Center
            b.Padding = UDim.new(0, 5)
            UiLibraryCore:AddSignal(
                (o:GetPropertyChangedSignal("AbsoluteContentSize")):Connect(LPH_NO_VIRTUALIZE(function()
                    if #l.Signals <= 0 then
                        UiLibraryCore.PlayAnimate(I, tweenNormal, {
                            Size = UDim2.new(0, 220, 0, o.AbsoluteContentSize.Y + 0),
                        })
                    else
                        UiLibraryCore.PlayAnimate(I, tweenNormal, {
                            Size = UDim2.new(0, 220, 0, o.AbsoluteContentSize.Y + 5),
                        })
                    end
                end))
            )
            d.Name = UiLibraryCore.RandomString()
            d.Parent = F
            d.BackgroundColor3 = Color3.fromRGB(26, 28, 36)
            d.BorderColor3 = Color3.fromRGB(0, 0, 0)
            d.BorderSizePixel = 0
            d.ClipsDescendants = true
            d.Size = UDim2.new(0, 100, 0, 18)
            d.ZIndex = 154
            X.CornerRadius = UDim.new(0, 4)
            X.Parent = d
            D.Transparency = 0.65
            D.Color = Color3.fromRGB(45, 48, 58)
            D.Parent = d
            H.Parent = d
            H.AnchorPoint = Vector2.new(0, 0.5)
            H.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
            H.BackgroundTransparency = 1
            H.BorderColor3 = Color3.fromRGB(0, 0, 0)
            H.BorderSizePixel = 0
            H.Position = UDim2.new(0, 5, 0.5, 0)
            H.Size = UDim2.new(1, -5, 0, 17)
            H.ZIndex = 154
            H.ClearTextOnFocus = false
            H.Font = Enum.Font.GothamMedium
            H.PlaceholderText = "Config Name ..."
            H.Text = ""
            H.TextColor3 = Color3.fromRGB(255, 255, 255)
            H.TextSize = 11
            H.TextTransparency = 0.35
            H.TextXAlignment = Enum.TextXAlignment.Left
            m.Name = UiLibraryCore.RandomString()
            m.Parent = F
            m.BackgroundColor3 = Color3.fromRGB(39, 40, 49)
            m.BackgroundTransparency = 1
            m.BorderColor3 = Color3.fromRGB(0, 0, 0)
            m.BorderSizePixel = 0
            m.ClipsDescendants = true
            m.Size = UDim2.new(0, 20, 0, 18)
            m.ZIndex = 153
            w.Name = UiLibraryCore.RandomString()
            w.Parent = m
            w.AnchorPoint = Vector2.new(0.5, 0.5)
            w.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
            w.BackgroundTransparency = 1
            w.BorderColor3 = Color3.fromRGB(0, 0, 0)
            w.BorderSizePixel = 0
            w.Position = UDim2.new(0.5, 0, 0.5, 0)
            w.Size = UDim2.new(1, 0, 1, 0)
            w.ZIndex = 153
            w.FontFace = UiLibraryCore.BuiltInBold
            w.Text = "plus-large"
            w.TextColor3 = Color3.fromRGB(223, 223, 223)
            w.TextSize = 16
            w.TextTransparency = 0.35
            w.TextWrapped = true
            v.CornerRadius = UDim.new(0, 4)
            v.Parent = m
            e.CornerRadius = UDim.new(0, 10)
            e.Parent = L
            local U = Instance.new("TextButton")
            local f = Instance.new("UICorner")
            U.Name = UiLibraryCore.RandomString()
            U.Parent = A
            U.AnchorPoint = Vector2.new(0, 0.5)
            U.BackgroundColor3 = Color3.fromRGB(20, 22, 27)
            U.BackgroundTransparency = 1
            U.BorderColor3 = Color3.fromRGB(0, 0, 0)
            U.BorderSizePixel = 0
            U.Position = UDim2.new(0, 31, 0.5, 0)
            U.Size = UDim2.new(1, -31, 1, 0)
            U.ZIndex = 10
            U.Font = Enum.Font.SourceSans
            U.Text = ""
            U.TextColor3 = Color3.fromRGB(0, 0, 0)
            U.TextSize = 14
            f.CornerRadius = UDim.new(0, 4)
            f.Parent = U
            l.SetRender(false)
            u:Connect(l.SetRender)
            l.UnsafeThread = nil
            l.SelectedConfig = "Default"
            local p = LPH_NO_VIRTUALIZE(function()
                local B = TextServiceUi:GetTextSize(a.Text, a.TextSize, a.Font, Vector2.new(math.huge, math.huge))
                UiLibraryCore.PlayAnimate(A, tweenNormal, {
                    Size = UDim2.fromOffset(B.X + 75, 30),
                })
            end)
            p()
            function l.GetData(u, B)
                local k = {}
                local W = 0
                for u, T in next, UiLibraryCore.Flags do
                    if T and T.GetValue then
                        local B = T:GetValue()
                        if typeof(B) == "Color3" then
                            table.insert(k, {
                                Idx = u,
                                Value = B:ToHex(),
                            })
                        else
                            table.insert(k, {
                                Idx = u,
                                Value = B,
                            })
                        end
                    end
                    if B then
                        if W % 35 == 1 then
                            task.wait()
                        end
                    end
                    W = W + 1
                end
                return UiLibraryCore.Base64Encode(stringCodec.new(HttpServiceUi:JSONEncode(k)))
            end
            function l.LoadData(u, B)
                local k = HttpServiceUi:JSONDecode(stringCodec.reverse(UiLibraryCore.Base64Decode(B)))
                for B, u in next, k do
                    if u.Idx then
                        if UiLibraryCore.Flags[u.Idx] then
                            task.spawn(function()
                                UiLibraryCore.Flags[u.Idx]:SetValue(u.Value)
                            end)
                        end
                    end
                end
            end
            function l.RefreshConfig(B)
                if not isfolder(k.ConfigFolder) then
                    UiLibraryCore:EnsureFolder(k.ConfigFolder)
                end
                if not isfile(k.ConfigFolder .. "/Default") then
                    writefile(k.ConfigFolder .. "/Default", l:GetData())
                end
                for B, u in next, I:GetChildren() do
                    if u:GetAttribute("ConfigItem") then
                        u:Destroy()
                    end
                end
                for B, u in next, l.Signals do
                    u:Disconnect()
                end
                table.clear(l.Signals)
                local Y = {}
                for B, u in next, listfiles(k.ConfigFolder) do
                    local T = string.sub(u, #k.ConfigFolder + 2)
                    table.insert(Y, T)
                end
                for B, Y in next, Y do
                    local q = Instance.new("Frame")
                    local x = Instance.new("Frame")
                    local o = Instance.new("UIListLayout")
                    local i = Instance.new("Frame")
                    local E = Instance.new("TextLabel")
                    local L = Instance.new("UICorner")
                    local Z = Instance.new("Frame")
                    local J = Instance.new("TextLabel")
                    local F = Instance.new("UICorner")
                    local b = Instance.new("UICorner")
                    local d = Instance.new("TextLabel")
                    local X = Instance.new("UIStroke")
                    q.Name = UiLibraryCore.RandomString()
                    q.Parent = I
                    q.BackgroundColor3 = Color3.fromRGB(21, 20, 27)
                    q.BorderColor3 = Color3.fromRGB(0, 0, 0)
                    q.BorderSizePixel = 0
                    q.Size = UDim2.new(1, -10, 0, 30)
                    q.ZIndex = 153
                    q:SetAttribute("ConfigItem", true)
                    x.Name = UiLibraryCore.RandomString()
                    x.Parent = q
                    x.AnchorPoint = Vector2.new(1, 0)
                    x.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
                    x.BackgroundTransparency = 1
                    x.BorderColor3 = Color3.fromRGB(0, 0, 0)
                    x.BorderSizePixel = 0
                    x.Position = UDim2.new(1, -11, 0, 2)
                    x.Size = UDim2.new(1, -20, 0, 25)
                    x.ZIndex = 153
                    o.Parent = x
                    o.FillDirection = Enum.FillDirection.Horizontal
                    o.HorizontalAlignment = Enum.HorizontalAlignment.Right
                    o.SortOrder = Enum.SortOrder.LayoutOrder
                    o.VerticalAlignment = Enum.VerticalAlignment.Center
                    o.Padding = UDim.new(0, 5)
                    i.Name = UiLibraryCore.RandomString()
                    i.Parent = x
                    i.BackgroundColor3 = Color3.fromRGB(39, 40, 49)
                    i.BackgroundTransparency = 1
                    i.BorderColor3 = Color3.fromRGB(0, 0, 0)
                    i.BorderSizePixel = 0
                    i.ClipsDescendants = true
                    i.Size = UDim2.new(0, 20, 0, 18)
                    i.ZIndex = 153
                    E.Name = UiLibraryCore.RandomString()
                    E.Parent = i
                    E.AnchorPoint = Vector2.new(0.5, 0.5)
                    E.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
                    E.BackgroundTransparency = 1
                    E.BorderColor3 = Color3.fromRGB(0, 0, 0)
                    E.BorderSizePixel = 0
                    E.Position = UDim2.new(0.5, 0, 0.5, 0)
                    E.Size = UDim2.new(1, 0, 1, 0)
                    E.ZIndex = 153
                    E.FontFace = UiLibraryCore.BuiltInBold
                    E.Text = "trash-can"
                    E.TextColor3 = Color3.fromRGB(223, 223, 223)
                    E.TextSize = 16
                    E.TextTransparency = 0.4
                    E.TextWrapped = true
                    L.CornerRadius = UDim.new(0, 4)
                    L.Parent = i
                    Z.Name = UiLibraryCore.RandomString()
                    Z.Parent = x
                    Z.BackgroundColor3 = Color3.fromRGB(39, 40, 49)
                    Z.BackgroundTransparency = 1
                    Z.BorderColor3 = Color3.fromRGB(0, 0, 0)
                    Z.BorderSizePixel = 0
                    Z.ClipsDescendants = true
                    Z.Size = UDim2.new(0, 20, 0, 18)
                    Z.ZIndex = 153
                    J.Name = UiLibraryCore.RandomString()
                    J.Parent = Z
                    J.AnchorPoint = Vector2.new(0.5, 0.5)
                    J.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
                    J.BackgroundTransparency = 1
                    J.BorderColor3 = Color3.fromRGB(0, 0, 0)
                    J.BorderSizePixel = 0
                    J.Position = UDim2.new(0.5, 0, 0.5, 0)
                    J.Size = UDim2.new(1, 0, 1, 0)
                    J.ZIndex = 153
                    J.FontFace = UiLibraryCore.BuiltInBold
                    J.Text = "arrow-right-from-portrait-rectangle"
                    J.TextColor3 = Color3.fromRGB(223, 223, 223)
                    J.TextSize = 16
                    J.TextTransparency = 0.4
                    J.TextWrapped = true
                    F.CornerRadius = UDim.new(0, 4)
                    F.Parent = Z
                    b.CornerRadius = UDim.new(0, 5)
                    b.Parent = q
                    d.Name = UiLibraryCore.RandomString()
                    d.Parent = q
                    d.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
                    d.BackgroundTransparency = 1
                    d.BorderColor3 = Color3.fromRGB(0, 0, 0)
                    d.BorderSizePixel = 0
                    d.Position = UDim2.new(0, 11, 0, 7)
                    d.Size = UDim2.new(0, 1, 0, 15)
                    d.ZIndex = 153
                    d.Font = Enum.Font.GothamMedium
                    d.Text = Y
                    d.TextColor3 = Color3.fromRGB(255, 255, 255)
                    d.TextSize = 13
                    d.TextTransparency = 0.2
                    d.TextXAlignment = Enum.TextXAlignment.Left
                    X.Transparency = 0.5
                    X.Color = Color3.fromRGB(45, 48, 58)
                    X.Parent = q
                    local D = LPH_NO_VIRTUALIZE(function(B)
                        if B then
                            UiLibraryCore.PlayAnimate(q, tweenNormal, {
                                BackgroundTransparency = 0,
                            })
                            UiLibraryCore.PlayAnimate(E, tweenNormal, {
                                TextTransparency = 0.4,
                            })
                            UiLibraryCore.PlayAnimate(J, tweenNormal, {
                                TextTransparency = 0.4,
                            })
                            UiLibraryCore.PlayAnimate(d, tweenNormal, {
                                TextTransparency = 0.2,
                            })
                            UiLibraryCore.PlayAnimate(X, tweenNormal, {
                                Transparency = 0.5,
                            })
                        else
                            UiLibraryCore.PlayAnimate(q, tweenNormal, {
                                BackgroundTransparency = 1,
                            })
                            UiLibraryCore.PlayAnimate(E, tweenNormal, {
                                TextTransparency = 1,
                            })
                            UiLibraryCore.PlayAnimate(J, tweenNormal, {
                                TextTransparency = 1,
                            })
                            UiLibraryCore.PlayAnimate(d, tweenNormal, {
                                TextTransparency = 1,
                            })
                            UiLibraryCore.PlayAnimate(X, tweenNormal, {
                                Transparency = 1,
                            })
                        end
                    end)
                    D(u:GetValue())
                    table.insert(l.Signals, u:Connect(D))
                    table.insert(
                        l.Signals,
                        q.MouseEnter:Connect(LPH_NO_VIRTUALIZE(function()
                            UiLibraryCore.PlayAnimate(X, tweenNormal, {
                                Transparency = 0.25,
                            })
                        end))
                    )
                    table.insert(
                        l.Signals,
                        q.MouseLeave:Connect(LPH_NO_VIRTUALIZE(function()
                            UiLibraryCore.PlayAnimate(X, tweenNormal, {
                                Transparency = 0.5,
                            })
                        end))
                    )
                    local H, R = UiLibraryCore:CreateInput(i, function()
                        if Y == "Default" then
                            W.new("trash-can", "You can't delete default config!", 3.5)
                            return
                        end
                        delfile(k.ConfigFolder .. ("/" .. Y))
                        p()
                        l:RefreshConfig()
                        W.new("trash-can", "Deleted " .. tostring(Y), 3.5)
                    end)
                    local m, w = UiLibraryCore:CreateInput(Z, function()
                        local B = k.ConfigFolder .. ("/" .. Y)
                        if isfile(B) then
                            local u = readfile(B)
                            l:LoadData(u)
                            l.SelectedConfig = Y
                            a.Text = Y
                            p()
                            l:RefreshConfig()
                            W.new("folder", "Loaded " .. tostring(Y), 3.5)
                        end
                    end)
                    table.insert(l.Signals, R)
                    table.insert(l.Signals, w)
                    table.insert(
                        l.Signals,
                        H.MouseEnter:Connect(LPH_NO_VIRTUALIZE(function()
                            UiLibraryCore.PlayAnimate(E, tweenNormal, {
                                TextTransparency = 0.2,
                                TextColor3 = Color3.fromRGB(223, 125, 125),
                            })
                        end))
                    )
                    table.insert(
                        l.Signals,
                        H.MouseLeave:Connect(LPH_NO_VIRTUALIZE(function()
                            UiLibraryCore.PlayAnimate(E, tweenNormal, {
                                TextTransparency = 0.4,
                                TextColor3 = Color3.fromRGB(223, 223, 223),
                            })
                        end))
                    )
                    table.insert(
                        l.Signals,
                        Z.MouseEnter:Connect(LPH_NO_VIRTUALIZE(function()
                            UiLibraryCore.PlayAnimate(J, tweenNormal, {
                                TextTransparency = 0.2,
                                TextColor3 = UiLibraryCore.AccentColor,
                            })
                        end))
                    )
                    table.insert(
                        l.Signals,
                        Z.MouseLeave:Connect(LPH_NO_VIRTUALIZE(function()
                            UiLibraryCore.PlayAnimate(J, tweenNormal, {
                                TextTransparency = 0.4,
                                TextColor3 = Color3.fromRGB(223, 223, 223),
                            })
                        end))
                    )
                end
                table.clear(Y)
            end
            task.delay(1, function()
                if l.SelectedConfig == "Default" then
                    local B = k.ConfigFolder .. "/Default"
                    local u = "Default"
                    if isfile(B) then
                        local T = readfile(B)
                        l:LoadData(T)
                        l.SelectedConfig = u
                        a.Text = u
                        p()
                        l:RefreshConfig()
                        W.new("folder", "Loaded Default Config", 3.5)
                        task.spawn(function()
                            while true do
                                task.wait(5.75)
                                if isfile(B) and l.SelectedConfig == "Default" then
                                    writefile(k.ConfigFolder .. "/Default", l:GetData(true))
                                end
                            end
                        end)
                    end
                end
            end)
            local y = UiLibraryCore:CreateInput(n, function()
                local B = k.ConfigFolder .. ("/" .. (l.SelectedConfig or "Default"))
                if isfile(B) then
                    writefile(k.ConfigFolder .. ("/" .. (l.SelectedConfig or "Default")), l:GetData())
                    W.new("folder", "Saved " .. tostring(l.SelectedConfig), 3.5)
                end
            end)
            UiLibraryCore:AddSignal(y.MouseEnter:Connect(LPH_NO_VIRTUALIZE(function()
                UiLibraryCore.PlayAnimate(n, tweenNormal, {
                    TextTransparency = 0.1,
                })
            end)))
            UiLibraryCore:AddSignal(y.MouseLeave:Connect(LPH_NO_VIRTUALIZE(function()
                UiLibraryCore.PlayAnimate(n, tweenNormal, {
                    TextTransparency = 0.25,
                })
            end)))
            local P = UiLibraryCore:CreateInput(m, function()
                local B = H.Text
                if B and (B:byte() and (not B:find("/", 1, true) and not B:find("\\", 1, true))) then
                    B = string.sub(B, 1, 24)
                    writefile(k.ConfigFolder .. ("/" .. B), l:GetData())
                    l.SelectedConfig = B
                    a.Text = B
                    W.new("folder", "Created " .. tostring(B), 3.5)
                    H.Text = ""
                    p()
                    l:RefreshConfig()
                end
            end)
            UiLibraryCore:AddSignal(P.MouseEnter:Connect(function()
                UiLibraryCore.PlayAnimate(w, tweenNormal, {
                    TextTransparency = 0.1,
                })
            end))
            UiLibraryCore:AddSignal(P.MouseLeave:Connect(function()
                UiLibraryCore.PlayAnimate(w, tweenNormal, {
                    TextTransparency = 0.35,
                })
            end))
            l:RefreshConfig()
            U.MouseButton1Click:Connect(LPH_NO_VIRTUALIZE(function()
                if l.UnsafeThread then
                    l.UnsafeThread:Disconnect()
                    l.UnsafeThread = nil
                end
                u:SetValue(true)
                l.UnsafeThread = UserInputServiceUi.InputBegan:Connect(function(B)
                    if
                        B.UserInputType == Enum.UserInputType.MouseButton1
                        or B.UserInputType == Enum.UserInputType.Touch
                    then
                        if not UiLibraryCore:IsMouseOverFrame(I) then
                            if l.UnsafeThread then
                                l.UnsafeThread:Disconnect()
                                l.UnsafeThread = nil
                            end
                            u:SetValue(false)
                        end
                    end
                end)
            end))
            return l
        end
        if UiLibraryCore.ConfigSupported then
            k:_InitConfig()
        end
        local W0 = UiLibraryCore:CreateOptionWindow(v, v.ZIndex + 13)
        local l0
        UiLibraryCore:CreateInput(
            v,
            LPH_NO_VIRTUALIZE(function()
                if l0 then
                    l0:Disconnect()
                    l0 = nil
                end
                W0.Signal:SetValue(true)
                l0 = UserInputServiceUi.InputBegan:Connect(function(B)
                    if
                        B.UserInputType == Enum.UserInputType.MouseButton1
                        or B.UserInputType == Enum.UserInputType.Touch
                    then
                        if
                            not UiLibraryCore:IsMouseOverFrame(W0.Root)
                            and (not UiLibraryCore:IsMouseOverFrame(v) and not UiLibraryCore.IsMosueOverOtherFrame)
                        then
                            if l0 then
                                l0:Disconnect()
                                l0 = nil
                            end
                            W0.Signal:SetValue(false)
                        end
                    end
                end)
            end)
        )
        k.UserSettings = W0
        UiLibraryCore:AddSignal(k.Signal:Connect(function(B)
            if B then
                return
            end
            if l0 then
                l0:Disconnect()
                l0 = nil
            end
            W0.Signal:SetValue(false)
        end))
        function k.SetAccount(u, B)
            B = UiLibraryCore:ProcessParams(B, {
                Profile = UiLibraryCore.UserProfile,
                Username = localPlayerUi.DisplayName,
                Expires = "Never",
            })
            U.Text = B.Username
            e.Image = B.Profile
            UiLibraryCore:SetMemberStatusLabel(f, B.Expires)
            k.Username = B.Username or k.Username
            k.Profile = B.Profile or k.Profile
            k.Expires = B.Expires or k.Expires
            if k.UserSettings.UserFrame then
                k.UserSettings.UserFrame:SetUsername(k.Username)
                k.UserSettings.UserFrame:SetProfile(k.Profile)
                k.UserSettings.UserFrame:SetExpires(k.Expires)
            else
                k.UserSettings.UserFrame = W0:AddUserFrame(k.Username, k.Profile, k.Expires)
            end
        end
        function k.SetSize(u, B)
            k.BaseSize = B or k.BaseSize
            k.Size = k.BaseSize
            if k.Responsive and (currentCameraUi and (k.BaseSize.X.Scale == 0 and k.BaseSize.Y.Scale == 0)) then
                local B = currentCameraUi.ViewportSize
                local u = math.max(320, B.X - 24)
                local T = math.max(260, B.Y - 24)
                local W = math.min(1, u / k.BaseSize.X.Offset, T / k.BaseSize.Y.Offset)
                k.Size = UDim2.fromOffset(math.floor(k.BaseSize.X.Offset * W), math.floor(k.BaseSize.Y.Offset * W))
            end
            if k.Signal:GetValue() then
                UiLibraryCore.PlayAnimate(l, tweenQuint, {
                    Size = k.Size,
                })
            end
        end
        k:SetAccount()
        k:SetSize(k.BaseSize)
        if currentCameraUi then
            UiLibraryCore:AddSignal((currentCameraUi:GetPropertyChangedSignal("ViewportSize")):Connect(function()
                if not k.__Destroyed then
                    k:SetSize(k.BaseSize)
                end
            end))
        end
        if k.Keybind then
            UiLibraryCore:AddSignal(UserInputServiceUi.InputBegan:Connect(LPH_NO_VIRTUALIZE(function(B, u)
                if
                    B.UserInputType == Enum.UserInputType.Keyboard
                    and (B.KeyCode == k.Keybind or B.KeyCode.Name == k.Keybind)
                then
                    if not u then
                        k:ToggleInterface()
                    end
                end
            end)))
        end
        function k.ToggleInterface(B)
            k.Signal:SetValue(not k.Signal:GetValue())
            if k.__3DRender then
                k.Load3DBlock()
            end
        end
        function k.SetVisible(u, B)
            k.Signal:SetValue(B == true)
            if k.__3DRender then
                k.Load3DBlock()
            end
        end
        function k.Toggle(B)
            k:ToggleInterface()
        end
        function k.Notify(u, B)
            B = B or {}
            return (UiLibraryCore:CreateNotification()).new({
                Title = B.Title or "Oi Hub",
                Content = B.Content or B.Text or "",
                Duration = B.Duration or 4,
            })
        end
        function k.Watermark(B)
            if UiLibraryCore.__WatermarkCache then
                return UiLibraryCore.__WatermarkCache
            end
            local u = {}
            local k = Instance.new("Frame")
            local W = Instance.new("UICorner")
            local l = Instance.new("UIListLayout")
            local I = UiLibraryCore:CreateShadow(k)
            k.Name = UiLibraryCore.RandomString()
            k.Parent = UiLibraryCore.ScreenGui
            k.AnchorPoint = Vector2.new(1, 0)
            k.BackgroundColor3 = Color3.fromRGB(8, 8, 13)
            k.BackgroundTransparency = 0.2
            k.BorderColor3 = Color3.fromRGB(0, 0, 0)
            k.BorderSizePixel = 0
            k.ClipsDescendants = true
            k.Position = UDim2.new(1, -10, 0, 10)
            k.Size = UDim2.new(0, 120, 0, 30)
            k.ZIndex = 16
            W.CornerRadius = UDim.new(0, 25)
            W.Parent = k
            l.Parent = k
            l.FillDirection = Enum.FillDirection.Horizontal
            l.SortOrder = Enum.SortOrder.LayoutOrder
            l.HorizontalAlignment = Enum.HorizontalAlignment.Right
            local Y = Instance.new("Frame")
            Y.Size = UDim2.fromOffset(15, 0)
            Y.BackgroundTransparency = 1
            Y.Parent = k
            Y.LayoutOrder = 5
            (k:GetPropertyChangedSignal("BackgroundTransparency")):Connect(LPH_NO_VIRTUALIZE(function()
                if k.BackgroundTransparency > 0.9 then
                    k.Visible = false
                    k.Parent = nil
                else
                    k.Parent = UiLibraryCore.ScreenGui
                    k.Visible = true
                end
            end));
            (l:GetPropertyChangedSignal("AbsoluteContentSize")):Connect(LPH_NO_VIRTUALIZE(function()
                UiLibraryCore.PlayAnimate(k, tweenNormal, {
                    Size = UDim2.new(0, l.AbsoluteContentSize.X + 5, 0, 30),
                })
            end))
            UiLibraryCore.__WatermarkCache = u
            I:Render(true)
            u.Renders = {}
            u.Status = true
            function u.SetRender(W, B)
                u.Status = B
                if B then
                    UiLibraryCore.PlayAnimate(k, tweenNormal, {
                        BackgroundTransparency = 0.2,
                    })
                    I:Render(true)
                    for B, u in next, u.Renders do
                        pcall(u, true)
                    end
                else
                    UiLibraryCore.PlayAnimate(k, tweenNormal, {
                        BackgroundTransparency = 1,
                    })
                    I:Render(false)
                    for B, u in next, u.Renders do
                        pcall(u, false)
                    end
                end
            end
            function u.AddBlock(l, B, W)
                local I = {}
                local Y = Instance.new("Frame")
                local x = Instance.new("TextLabel")
                local o = Instance.new("TextLabel")
                Y.Parent = k
                Y.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
                Y.BackgroundTransparency = 1
                Y.BorderColor3 = Color3.fromRGB(0, 0, 0)
                Y.BorderSizePixel = 0
                Y.Size = UDim2.new(0, 50, 0, 30)
                x.Name = UiLibraryCore.RandomString()
                x.Parent = Y
                x.AnchorPoint = Vector2.new(0, 0.5)
                x.BackgroundColor3 = Color3.fromRGB(186, 186, 186)
                x.BackgroundTransparency = 1
                x.BorderColor3 = Color3.fromRGB(0, 0, 0)
                x.BorderSizePixel = 0
                x.Position = UDim2.new(0, 35, 0.5, 0)
                x.Size = UDim2.new(0, 1, 0, 25)
                x.ZIndex = 17
                x.Font = Enum.Font.GothamBold
                x.Text = W
                x.TextColor3 = Color3.fromRGB(186, 186, 186)
                x.TextSize = 15
                x.TextTransparency = 0.2
                x.TextXAlignment = Enum.TextXAlignment.Left
                o.Name = UiLibraryCore.RandomString()
                o.Parent = Y
                o.AnchorPoint = Vector2.new(0, 0.5)
                o.BackgroundColor3 = Color3.fromRGB(186, 186, 186)
                o.BackgroundTransparency = 1
                o.BorderColor3 = Color3.fromRGB(0, 0, 0)
                o.BorderSizePixel = 0
                o.Position = UDim2.new(0, 10, 0.5, 0)
                o.Size = UDim2.new(0, 20, 0, 20)
                o.ZIndex = 17
                o.FontFace = UiLibraryCore.BuiltInBold
                o.Text = B
                o.TextColor3 = UiLibraryCore.AccentColor
                o.TextSize = 18
                o.TextTransparency = 0.25
                o.TextWrapped = true
                I.Update = LPH_NO_VIRTUALIZE(function(B)
                    local u = TextServiceUi:GetTextSize(x.Text, x.TextSize, x.Font, Vector2.new(math.huge, math.huge))
                    if I.Visible then
                        UiLibraryCore.PlayAnimate(Y, tweenQuint, {
                            Size = UDim2.new(0, u.X + 35, 0, 30),
                        })
                    else
                        UiLibraryCore.PlayAnimate(Y, tweenQuint, {
                            Size = UDim2.new(0, 0, 0, 30),
                        })
                    end
                end)
                I.Visible = true
                I.Update()
                function I.SetVisible(k, B)
                    I.Visible = B
                    if u.Status then
                        I.SetRender(B)
                    end
                    I.Update()
                end
                I.SetRender = LPH_NO_VIRTUALIZE(function(B)
                    if B and I.Visible then
                        UiLibraryCore.PlayAnimate(x, tweenNormal, {
                            TextTransparency = 0.2,
                        })
                        UiLibraryCore.PlayAnimate(o, tweenNormal, {
                            TextTransparency = 0.25,
                        })
                    else
                        UiLibraryCore.PlayAnimate(x, tweenNormal, {
                            TextTransparency = 1,
                        })
                        UiLibraryCore.PlayAnimate(o, tweenNormal, {
                            TextTransparency = 1,
                        })
                    end
                end)
                table.insert(u.Renders, I.SetRender)
                function I.SetText(u, B)
                    x.Text = B
                    I.Update()
                end
                function I.Input(u, B)
                    local k, W = UiLibraryCore:CreateInput(Y, B)
                    return W
                end
                return I
            end
            return u
        end
        k.Frame = l
        table.insert(UiLibraryCore.Windows, k)
        function k.Destroy(B)
            if k.__Destroyed then
                return
            end
            k.__Destroyed = true
            UiLibraryCore:Unload()
        end
        k:SetRender(false)
        return k
    end
    function UiLibraryCore.CreateNotification(B)
        if UiLibraryCore.__Notification_Cache then
            return UiLibraryCore.__Notification_Cache
        end
        local u = {}
        local k = Instance.new("Frame")
        local W = Instance.new("UIListLayout")
        k.Name = UiLibraryCore.RandomString()
        k.Parent = UiLibraryCore.ScreenGui
        k.AnchorPoint = Vector2.new(1, 0)
        k.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
        k.BackgroundTransparency = 1
        k.BorderColor3 = Color3.fromRGB(0, 0, 0)
        k.BorderSizePixel = 0
        k.Position = UDim2.new(1, -25, 0, 25)
        k.Size = UDim2.new(0, 25, 0, 25)
        W.Parent = k
        W.HorizontalAlignment = Enum.HorizontalAlignment.Right
        W.SortOrder = Enum.SortOrder.LayoutOrder
        W.Padding = UDim.new(0, 0)
        UiLibraryCore.__Notification_Cache = u
        function u.new(B)
            B = UiLibraryCore:ProcessParams(B, {
                Title = "Notification",
                Content = "Hello World!",
                Logo = UiLibraryCore.GlobalLogo or "rbxasset://textures/ui/VerifiedBadgeNameIcon.png",
                Duration = 5,
            })
            if UiLibraryCore.__WatermarkCache then
                UiLibraryCore.PlayAnimate(k, tweenNormal, {
                    Position = UDim2.new(1, -25, 0, 55),
                })
            end
            local u = Instance.new("Frame")
            local W = Instance.new("Frame")
            local l = Instance.new("UICorner")
            local I = Instance.new("UIStroke")
            local Y = Instance.new("ImageLabel")
            local x = Instance.new("UICorner")
            local o = Instance.new("TextLabel")
            local i = Instance.new("TextLabel")
            local E = UiLibraryCore:CreateShadow(W, true)
            u.Name = UiLibraryCore.RandomString()
            u.Parent = k
            u.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
            u.BackgroundTransparency = 1
            u.BorderColor3 = Color3.fromRGB(0, 0, 0)
            u.BorderSizePixel = 0
            u.Size = UDim2.new(0, 0, 0, 100)
            W.Name = UiLibraryCore.RandomString()
            W.Parent = u
            W.AnchorPoint = Vector2.new(1, 0)
            W.BackgroundColor3 = Color3.fromRGB(20, 22, 27)
            W.BackgroundTransparency = 0.075
            W.BorderColor3 = Color3.fromRGB(0, 0, 0)
            W.BorderSizePixel = 0
            W.ClipsDescendants = true
            W.Position = UDim2.new(0, 750, 0, 0)
            W.Size = UDim2.new(0, 220, 0, 55)
            W.ZIndex = 130
            l.CornerRadius = UDim.new(0, 10)
            l.Parent = W
            I.Transparency = 0.65
            I.Color = Color3.fromRGB(45, 48, 58)
            I.Parent = W
            Y.Name = UiLibraryCore.RandomString()
            Y.Parent = W
            Y.AnchorPoint = Vector2.new(0, 0.5)
            Y.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
            Y.BackgroundTransparency = 1
            Y.BorderColor3 = Color3.fromRGB(0, 0, 0)
            Y.BorderSizePixel = 0
            Y.Position = UDim2.new(0, 10, 0.5, 0)
            Y.Size = UDim2.new(0, 35, 0, 35)
            Y.ZIndex = 131
            Y.Image = B.Logo
            Y.ImageColor3 = UiLibraryCore.IconColor
            x.CornerRadius = UDim.new(0, 7)
            x.Parent = Y
            o.Name = UiLibraryCore.RandomString()
            o.Parent = W
            o.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
            o.BackgroundTransparency = 1
            o.BorderColor3 = Color3.fromRGB(0, 0, 0)
            o.BorderSizePixel = 0
            o.Position = UDim2.new(0, 50, 0, 7)
            o.Size = UDim2.new(0, 200, 0, 20)
            o.ZIndex = 132
            o.Font = Enum.Font.GothamBold
            o.Text = B.Title
            o.TextColor3 = Color3.fromRGB(255, 255, 255)
            o.TextSize = 17
            o.TextXAlignment = Enum.TextXAlignment.Left
            i.Name = UiLibraryCore.RandomString()
            i.Parent = W
            i.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
            i.BackgroundTransparency = 1
            i.BorderColor3 = Color3.fromRGB(0, 0, 0)
            i.BorderSizePixel = 0
            i.Position = UDim2.new(0, 50, 0, 28)
            i.Size = UDim2.new(0, 200, 0, 15)
            i.ZIndex = 132
            i.Font = Enum.Font.GothamBold
            i.Text = B.Content
            i.TextColor3 = Color3.fromRGB(255, 255, 255)
            i.TextSize = 12
            i.TextTransparency = 0.65
            i.TextXAlignment = Enum.TextXAlignment.Left
            local L = TextServiceUi:GetTextSize(o.Text, o.TextSize, o.Font, Vector2.new(math.huge, math.huge))
            local Z = TextServiceUi:GetTextSize(i.Text, i.TextSize, i.Font, Vector2.new(math.huge, math.huge))
            local J = math.max(L.X, Z.X)
            W.Size = UDim2.new(0, J + 65, 0, 55)
            E:Render(true)
            UiLibraryCore.PlayAnimate(W, tweenQuint, {
                Position = UDim2.new(1, 0, 0, 0),
            })
            u.Size = UDim2.new(0, 0, 0, 65)
            task.delay(
                B.Duration or 5,
                LPH_NO_VIRTUALIZE(function()
                    if UiLibraryCore.__WatermarkCache then
                        UiLibraryCore.PlayAnimate(k, tweenNormal, {
                            Position = UDim2.new(1, -25, 0, 55),
                        })
                    end
                    E:Render(false)
                    UiLibraryCore.PlayAnimate(W, tweenNormal, {
                        BackgroundTransparency = 1,
                    })
                    UiLibraryCore.PlayAnimate(I, tweenNormal, {
                        Transparency = 1,
                    })
                    UiLibraryCore.PlayAnimate(Y, tweenNormal, {
                        ImageTransparency = 1,
                    })
                    UiLibraryCore.PlayAnimate(o, tweenNormal, {
                        TextTransparency = 1,
                    })
                    UiLibraryCore.PlayAnimate(i, tweenNormal, {
                        TextTransparency = 1,
                    })
                    task.wait(0.125)
                    UiLibraryCore.PlayAnimate(u, tweenNormal, {
                        Size = UDim2.new(0, 0, 0, 0),
                    })
                    task.wait(0.125)
                    u:Destroy()
                end)
            )
        end
        return u
    end
    function UiLibraryCore.CreateLogger(B)
        if UiLibraryCore.__LogSystem then
            return UiLibraryCore.__LogSystem
        end
        local u = {}
        local k = Instance.new("Frame")
        local W = Instance.new("UIListLayout")
        k.Name = UiLibraryCore.RandomString()
        k.Parent = UiLibraryCore.ScreenGui
        k.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
        k.BackgroundTransparency = 1
        k.BorderColor3 = Color3.fromRGB(0, 0, 0)
        k.BorderSizePixel = 0
        k.Position = UDim2.new(0, 25, 0, 5 + math.abs(UiLibraryCore.ScreenGui.AbsolutePosition.Y))
        k.Size = UDim2.new(0, 25, 0, 25)
        W.Parent = k
        W.SortOrder = Enum.SortOrder.LayoutOrder
        W.Padding = UDim.new(0, 12)
        UiLibraryCore.__LogSystem = u
        function u.new(B, u, W)
            W = W or 3
            u = u or "Log"
            B = B or "crosshairs"
            local l = Instance.new("Frame")
            local I = Instance.new("UICorner")
            local Y = Instance.new("UIStroke")
            local x = Instance.new("TextLabel")
            local o = Instance.new("Frame")
            local i = Instance.new("UICorner")
            local E = Instance.new("TextLabel")
            local L = UiLibraryCore:CreateShadow(l, true)
            l.Name = UiLibraryCore.RandomString()
            l.Parent = k
            l.AnchorPoint = Vector2.new(0.5, 0)
            l.BackgroundColor3 = Color3.fromRGB(20, 22, 27)
            l.BackgroundTransparency = 1
            l.BorderColor3 = Color3.fromRGB(0, 0, 0)
            l.BorderSizePixel = 0
            l.ClipsDescendants = true
            l.Position = UDim2.new(0, 0, 0, 0)
            l.Size = UDim2.new(0, 0, 0, 20)
            l.ZIndex = 130
            I.CornerRadius = UDim.new(0, 4)
            I.Parent = l
            Y.Transparency = 1
            Y.Color = Color3.fromRGB(45, 48, 58)
            Y.Parent = l
            x.Name = UiLibraryCore.RandomString()
            x.Parent = l
            x.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
            x.BackgroundTransparency = 1
            x.BorderColor3 = Color3.fromRGB(0, 0, 0)
            x.BorderSizePixel = 0
            x.Position = UDim2.new(0, 25, 0, 2)
            x.Size = UDim2.new(0, 200, 0, 15)
            x.ZIndex = 132
            x.Font = Enum.Font.GothamBold
            x.Text = u
            x.TextColor3 = Color3.fromRGB(255, 255, 255)
            x.TextSize = 12
            x.TextTransparency = 1
            x.TextXAlignment = Enum.TextXAlignment.Left
            o.Name = UiLibraryCore.RandomString()
            o.Parent = l
            o.AnchorPoint = Vector2.new(0, 0.5)
            o.BackgroundColor3 = UiLibraryCore.AccentColor
            o.BorderColor3 = Color3.fromRGB(0, 0, 0)
            o.BackgroundTransparency = 1
            o.BorderSizePixel = 0
            o.Position = UDim2.new(0, -2, 0.5, 0)
            o.Size = UDim2.new(0, 5, 1, 0)
            o.ZIndex = 131
            i.CornerRadius = UDim.new(0, 4)
            i.Parent = o
            E.Name = UiLibraryCore.RandomString()
            E.Parent = l
            E.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
            E.BackgroundTransparency = 1
            E.BorderColor3 = Color3.fromRGB(0, 0, 0)
            E.BorderSizePixel = 0
            E.Position = UDim2.new(0, 7, 0, 3)
            E.Size = UDim2.new(0, 15, 0, 15)
            E.ZIndex = 133
            E.FontFace = UiLibraryCore.BuiltInBold
            E.Text = B
            E.TextColor3 = Color3.fromRGB(223, 223, 223)
            E.TextSize = 13
            E.TextTransparency = 1
            E.TextWrapped = true
            local Z = TextServiceUi:GetTextSize(x.Text, x.TextSize, x.Font, Vector2.new(math.huge, math.huge))
            UiLibraryCore.PlayAnimate(l, tweenNormal, {
                Size = UDim2.new(0, Z.X + 35, 0, 20),
                BackgroundTransparency = 0.075,
            })
            task.delay(
                0.15,
                LPH_NO_VIRTUALIZE(function()
                    L:Render(true)
                    UiLibraryCore.PlayAnimate(Y, tweenNormal, {
                        Transparency = 0.65,
                    })
                    UiLibraryCore.PlayAnimate(x, tweenNormal, {
                        TextTransparency = 0.25,
                    })
                    UiLibraryCore.PlayAnimate(o, tweenNormal, {
                        BackgroundTransparency = 0,
                    })
                    UiLibraryCore.PlayAnimate(E, tweenNormal, {
                        TextTransparency = 0.25,
                    })
                    task.wait(W + 0.1)
                    L:Render(false)
                    UiLibraryCore.PlayAnimate(l, tweenNormal, {
                        BackgroundTransparency = 1,
                    })
                    UiLibraryCore.PlayAnimate(Y, tweenNormal, {
                        Transparency = 1,
                    })
                    UiLibraryCore.PlayAnimate(x, tweenNormal, {
                        TextTransparency = 1,
                    })
                    UiLibraryCore.PlayAnimate(o, tweenNormal, {
                        BackgroundTransparency = 1,
                    })
                    UiLibraryCore.PlayAnimate(E, tweenNormal, {
                        TextTransparency = 1,
                    })
                    task.wait(0.25)
                    l:Destroy()
                end)
            )
        end
        return u
    end
    function UiLibraryCore.CreateIndicator(B)
        local u = Instance.new("Frame")
        local k = Instance.new("UIListLayout")
        u.Name = UiLibraryCore.RandomString()
        u.Parent = UiLibraryCore.ScreenGui
        u.AnchorPoint = Vector2.new(0, 0.5)
        u.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
        u.BackgroundTransparency = 1
        u.BorderColor3 = Color3.fromRGB(0, 0, 0)
        u.BorderSizePixel = 0
        u.Position = UDim2.new(0, 15, 0.5, 0)
        u.Size = UDim2.new(0, 100, 0, 100)
        u.ZIndex = 15
        k.Parent = u
        k.SortOrder = Enum.SortOrder.LayoutOrder
        k.Padding = UDim.new(0, 10)
        local W = {}
        W.Color = {
            Red = Color3.fromRGB(255, 102, 105),
            Green = Color3.fromRGB(135, 255, 143),
            White = Color3.fromRGB(186, 186, 186),
        }
        W.Root = u
        function W.new(B)
            B = UiLibraryCore:ProcessParams(B, {
                Name = "Indicator",
                Icon = "crosshairs",
                Color = "Red",
            })
            local k = {
                CurrentColor = B.Color,
                Visible = false,
            }
            local l = Instance.new("Frame")
            local I = Instance.new("UICorner")
            local Y = Instance.new("Frame")
            local x = Instance.new("UICorner")
            local o = Instance.new("UIGradient")
            local i = Instance.new("TextLabel")
            local E = Instance.new("TextLabel")
            local L = UiLibraryCore:CreateShadow(l)
            l.Name = UiLibraryCore.RandomString()
            l.BackgroundColor3 = Color3.fromRGB(8, 8, 13)
            l.BackgroundTransparency = 1
            l.BorderColor3 = Color3.fromRGB(0, 0, 0)
            l.BorderSizePixel = 0
            l.ClipsDescendants = true
            l.Size = UDim2.new(0, 85, 0, 40)
            l.ZIndex = 16
            l.Visible = false
            (l:GetPropertyChangedSignal("BackgroundTransparency")):Connect(LPH_NO_VIRTUALIZE(function()
                if l.BackgroundTransparency > 0.9 then
                    l.Parent = nil
                    l.Visible = false
                else
                    l.Parent = u
                    l.Visible = true
                end
            end))
            I.CornerRadius = UDim.new(0, 25)
            I.Parent = l
            Y.Name = UiLibraryCore.RandomString()
            Y.Parent = l
            Y.AnchorPoint = Vector2.new(0, 0.5)
            Y.BackgroundColor3 = Color3.fromRGB(186, 186, 186)
            Y.BorderColor3 = Color3.fromRGB(0, 0, 0)
            Y.BorderSizePixel = 0
            Y.Position = UDim2.new(0, 2, 0.5, 0)
            Y.BackgroundTransparency = 1
            Y.Size = UDim2.new(0, 3, 0.649999976, 0)
            Y.ZIndex = 17
            x.CornerRadius = UDim.new(0, 25)
            x.Parent = Y
            o.Rotation = 90
            o.Transparency = NumberSequence.new({
                NumberSequenceKeypoint.new(0, 1),
                NumberSequenceKeypoint.new(0.5, 0),
                NumberSequenceKeypoint.new(1, 1),
            })
            o.Parent = Y
            i.Name = UiLibraryCore.RandomString()
            i.Parent = l
            i.AnchorPoint = Vector2.new(0, 0.5)
            i.BackgroundColor3 = Color3.fromRGB(186, 186, 186)
            i.BackgroundTransparency = 1
            i.BorderColor3 = Color3.fromRGB(0, 0, 0)
            i.BorderSizePixel = 0
            i.Position = UDim2.new(0, 10, 0.5, 0)
            i.Size = UDim2.new(0, 25, 0, 25)
            i.ZIndex = 17
            i.FontFace = UiLibraryCore.BuiltInBold
            i.Text = B.Icon
            i.TextColor3 = Color3.fromRGB(186, 186, 186)
            i.TextSize = 21
            i.TextTransparency = 1
            i.TextWrapped = true
            E.Name = UiLibraryCore.RandomString()
            E.Parent = l
            E.AnchorPoint = Vector2.new(0, 0.5)
            E.BackgroundColor3 = Color3.fromRGB(186, 186, 186)
            E.BackgroundTransparency = 1
            E.BorderColor3 = Color3.fromRGB(0, 0, 0)
            E.BorderSizePixel = 0
            E.Position = UDim2.new(0, 40, 0.5, 0)
            E.Size = UDim2.new(1, -40, 0, 25)
            E.ZIndex = 17
            E.Font = Enum.Font.GothamBold
            E.Text = B.Name
            E.TextColor3 = Color3.fromRGB(186, 186, 186)
            E.TextSize = 20
            E.TextTransparency = 1
            E.TextXAlignment = Enum.TextXAlignment.Left
            k.Update = LPH_NO_VIRTUALIZE(function()
                local B = TextServiceUi:GetTextSize(E.Text, E.TextSize, E.Font, Vector2.new(math.huge, math.huge))
                UiLibraryCore.PlayAnimate(l, tweenNormal, {
                    Size = UDim2.new(0, B.X + 60, 0, 40),
                })
            end)
            k.SetRender = LPH_NO_VIRTUALIZE(function(B, u)
                k.Visible = u
                if u then
                    UiLibraryCore.PlayAnimate(l, tweenNormal, {
                        BackgroundTransparency = 0.2,
                    })
                    UiLibraryCore.PlayAnimate(Y, tweenNormal, {
                        BackgroundTransparency = 0,
                        BackgroundColor3 = W.Color[k.CurrentColor],
                    })
                    UiLibraryCore.PlayAnimate(i, tweenQuint, {
                        TextTransparency = 0.25,
                        TextColor3 = W.Color[k.CurrentColor],
                    })
                    UiLibraryCore.PlayAnimate(E, tweenQuint, {
                        TextTransparency = 0.2,
                        TextColor3 = W.Color[k.CurrentColor],
                    })
                    L:Render(true)
                else
                    UiLibraryCore.PlayAnimate(l, tweenNormal, {
                        BackgroundTransparency = 1,
                    })
                    UiLibraryCore.PlayAnimate(Y, tweenNormal, {
                        BackgroundTransparency = 1,
                        BackgroundColor3 = W.Color[k.CurrentColor],
                    })
                    UiLibraryCore.PlayAnimate(i, tweenQuint, {
                        TextTransparency = 1,
                        TextColor3 = W.Color[k.CurrentColor],
                    })
                    UiLibraryCore.PlayAnimate(E, tweenQuint, {
                        TextTransparency = 1,
                        TextColor3 = W.Color[k.CurrentColor],
                    })
                    L:Render(false)
                end
                k.Update()
            end)
            k.Update()
            k:SetRender(false)
            function k.SetColor(u, B)
                k.CurrentColor = B
                if k.Visible then
                    k:SetRender(true)
                end
            end
            function k.SetText(T, u)
                B.Name = u
                E.Text = B.Name
                k.Update()
            end
            return k
        end
        return W
    end
    function UiLibraryCore.CreateESPPreview(u, B)
        B = UiLibraryCore:ProcessParams(B, {
            Title = "ESP PREVIEW",
            Visible = false,
            Position = UDim2.new(1, -270, 1, -350),
            Image = UiLibraryCore.ESPPreviewAvatar,
            Name = "PLAYER",
            NameSide = "Top",
            Box = true,
            ShowName = true,
            Color = UiLibraryCore.AccentColor,
        })
        local k = {
            Visible = B.Visible == true,
            Signal = UiLibraryCore:CreateSignal(B.Visible == true),
            NameSide = B.NameSide,
        }
        local W = Instance.new("Frame")
        local l = Instance.new("Frame")
        local I = Instance.new("UICorner")
        local x = Instance.new("UIStroke")
        local o = Instance.new("Frame")
        local i = Instance.new("Frame")
        local E = Instance.new("TextLabel")
        local L = Instance.new("Frame")
        local Z = Instance.new("UICorner")
        local J = Instance.new("ImageLabel")
        local F = Instance.new("Frame")
        local b = Instance.new("UIStroke")
        local d = Instance.new("TextLabel")
        W.Name = "OiHubESPPreview"
        W.Parent = UiLibraryCore.ScreenGui
        W.Position = B.Position
        W.Size = UDim2.fromOffset(250, 330)
        W.BackgroundTransparency = 1
        W.BorderSizePixel = 0
        W.Active = true
        W.Visible = k.Visible
        W.ZIndex = 29
        l.Name = "Panel"
        l.Parent = W
        l.Size = UDim2.fromScale(1, 1)
        l.BackgroundColor3 = Color3.fromRGB(8, 8, 13)
        l.BackgroundTransparency = UiLibraryCore.EnabledBlur and 0.055 or 0.0255
        l.BorderSizePixel = 0
        l.ClipsDescendants = true
        l.ZIndex = 30
        I.CornerRadius = UDim.new(0, 8)
        I.Parent = l
        x.Color = Color3.fromRGB(45, 48, 58)
        x.Transparency = 0.65
        x.Thickness = 1
        x.ApplyStrokeMode = Enum.ApplyStrokeMode.Border
        x.Parent = l
        local X = UiLibraryCore:CreateShadow(l)
        X:Render(k.Visible)
        if UiLibraryCore.EnabledBlur then
            UiLibraryCore:CreateBlurModule(l, k.Signal)
        end
        o.Name = "Header"
        o.Parent = l
        o.Size = UDim2.new(1, 0, 0, 38)
        o.BackgroundTransparency = 1
        o.Active = true
        o.ZIndex = 31
        i.Name = "Divider"
        i.Parent = o
        i.AnchorPoint = Vector2.new(0, 1)
        i.Position = UDim2.fromScale(0, 1)
        i.Size = UDim2.new(1, 0, 0, 1)
        i.BackgroundColor3 = Color3.fromRGB(42, 45, 56)
        i.BackgroundTransparency = 0.3
        i.BorderSizePixel = 0
        i.ZIndex = 32
        E.Parent = o
        E.Position = UDim2.fromScale(0, 0)
        E.Size = UDim2.fromScale(1, 1)
        E.BackgroundTransparency = 1
        E.Font = Enum.Font.GothamBold
        E.Text = B.Title
        E.TextColor3 = Color3.fromRGB(235, 237, 244)
        E.TextSize = 12
        E.TextXAlignment = Enum.TextXAlignment.Center
        E.TextYAlignment = Enum.TextYAlignment.Center
        E.ZIndex = 32
        L.Name = "Stage"
        L.Parent = l
        L.Position = UDim2.fromOffset(12, 50)
        L.Size = UDim2.new(1, -24, 1, -62)
        L.BackgroundColor3 = Color3.fromRGB(12, 13, 19)
        L.BackgroundTransparency = 0.2
        L.BorderSizePixel = 0
        L.ClipsDescendants = true
        L.ZIndex = 31
        Z.CornerRadius = UDim.new(0, 6)
        Z.Parent = L
        J.Name = "Avatar"
        J.Parent = L
        J.AnchorPoint = Vector2.new(0.5, 0.5)
        J.Position = UDim2.fromScale(0.5, 0.53)
        J.Size = UDim2.new(1, -18, 1, -18)
        J.BackgroundTransparency = 1
        J.Image = B.Image
        J.ScaleType = Enum.ScaleType.Fit
        J.ZIndex = 32
        F.Name = "Box"
        F.Parent = L
        F.AnchorPoint = Vector2.new(0.5, 0.5)
        F.Position = UDim2.fromScale(0.5, 0.5)
        F.Size = UDim2.new(0.58, 0, 0.72, 0)
        F.BackgroundTransparency = 1
        F.BorderSizePixel = 0
        F.Visible = B.Box == true
        F.ZIndex = 33
        b.Color = B.Color
        b.Thickness = 1.5
        b.Transparency = 0
        b.ApplyStrokeMode = Enum.ApplyStrokeMode.Border
        b.Parent = F
        d.Name = "ESPName"
        d.Parent = L
        d.AnchorPoint = Vector2.new(0.5, 0.5)
        d.Position = UDim2.fromScale(0.5, 0.09)
        d.Size = UDim2.fromOffset(120, 18)
        d.BackgroundTransparency = 1
        d.Active = true
        d.Font = Enum.Font.GothamBold
        d.Text = tostring(B.Name)
        d.TextColor3 = B.Color
        d.TextSize = 11
        d.TextStrokeColor3 = Color3.new(0, 0, 0)
        d.TextStrokeTransparency = 0.35
        d.Visible = B.ShowName == true
        d.ZIndex = 34
        UiLibraryCore.Drag(o, W, 0.1)
        local s = false
        local D = B.NameSide
        local function H()
            local B = L.AbsoluteSize
            local u = B.X > 0 and B.X or 226
            local k = B.Y > 0 and B.Y or 268
            local T = TextServiceUi:GetTextSize(d.Text, d.TextSize, d.Font, Vector2.new(math.huge, 18))
            local W = 7
            return {
                Top = Vector2.new(0.5, 0.14 - (T.Y / 2 + W) / k),
                Right = Vector2.new(0.79 + (T.X / 2 + W) / u, 0.5),
                Bottom = Vector2.new(0.5, 0.86 + (T.Y / 2 + W) / k),
                Left = Vector2.new(0.21 - (T.X / 2 + W) / u, 0.5),
            }
        end
        local function R(u)
            u = tostring(u or "Top")
            u = string.upper(string.sub(u, 1, 1)) .. string.lower(string.sub(u, 2))
            local T = H()
            if not T[u] then
                u = "Top"
            end
            k.NameSide = u
            k.NamePosition = T[u]
            d.Position = UDim2.fromScale(k.NamePosition.X, k.NamePosition.Y)
            if type(B.NameSideChanged) == "function" then
                B.NameSideChanged(u)
            end
        end
        local function m(B)
            local u = L.AbsoluteSize
            if u.X <= 0 or u.Y <= 0 then
                return
            end
            local k = Vector2.new(
                math.clamp((B.Position.X - L.AbsolutePosition.X) / u.X, 0.02, 0.98),
                math.clamp((B.Position.Y - L.AbsolutePosition.Y) / u.Y, 0.035, 0.965)
            )
            d.Position = UDim2.fromScale(k.X, k.Y)
            local T = k - Vector2.new(0.5, 0.5)
            D = math.abs(T.X / 0.29) > math.abs(T.Y / 0.36) and (T.X > 0 and "Right" or "Left")
                or (T.Y > 0 and "Bottom" or "Top")
        end
        R(B.NameSide)
        UiLibraryCore:AddSignal(d.InputBegan:Connect(function(B)
            if B.UserInputType ~= Enum.UserInputType.MouseButton1 and B.UserInputType ~= Enum.UserInputType.Touch then
                return
            end
            s = true
            D = k.NameSide
            m(B)
            local u
            u = B.Changed:Connect(function()
                if B.UserInputState ~= Enum.UserInputState.End then
                    return
                end
                s = false
                R(D)
                u:Disconnect()
            end)
        end))
        UiLibraryCore:AddSignal(UserInputServiceUi.InputChanged:Connect(function(B)
            if
                s
                and (B.UserInputType == Enum.UserInputType.MouseMovement or B.UserInputType == Enum.UserInputType.Touch)
            then
                m(B)
            end
        end))
        function k.SetVisible(u, B)
            k.Visible = B == true
            W.Visible = k.Visible
            k.Signal:SetValue(k.Visible)
            X:Render(k.Visible)
        end
        function k.SetBox(u, B)
            F.Visible = B == true
        end
        function k.SetNameVisible(u, B)
            d.Visible = B == true
        end
        function k.SetName(u, B)
            d.Text = tostring(B or "PLAYER")
            R(k.NameSide)
        end
        function k.SetNamePosition(u, B)
            local k = B - Vector2.new(0.5, 0.5)
            R(math.abs(k.X) > math.abs(k.Y) and (k.X > 0 and "Right" or "Left") or (k.Y > 0 and "Bottom" or "Top"))
        end
        function k.GetNamePosition(B)
            return k.NamePosition
        end
        function k.SetNameSide(u, B)
            R(B)
        end
        function k.GetNameSide(B)
            return k.NameSide
        end
        function k.SetColor(u, B)
            b.Color = B
            d.TextColor3 = B
        end
        function k.SetImage(u, B)
            J.Image = UiLibraryCore:ResolveImage(B, "esp-preview-custom.webp", UiLibraryCore.ESPPreviewAvatar)
        end
        function k.Destroy(B)
            k.Signal:SetValue(false)
            X:Render(false)
            if W then
                W:Destroy()
            end
        end
        k.Instance = W
        k.Panel = l
        k.Avatar = J
        return k
    end
    function UiLibraryCore.CreateKeybindList(u, B)
        B = UiLibraryCore:ProcessParams(B, {
            Title = "KEYBIND",
            Visible = true,
            Position = UDim2.new(1, -250, 0, 55),
        })
        local k = {
            Items = {},
            Visible = B.Visible == true,
            Signal = UiLibraryCore:CreateSignal(B.Visible == true),
        }
        local W = Instance.new("Frame")
        local l = Instance.new("Frame")
        local I = Instance.new("UICorner")
        local Y = Instance.new("UIStroke")
        local q = Instance.new("Frame")
        local x = Instance.new("Frame")
        local o = Instance.new("TextLabel")
        local i = Instance.new("Frame")
        local E = Instance.new("UIListLayout")
        W.Name = "OiHubKeybindHUD"
        W.Parent = UiLibraryCore.ScreenGui
        W.Position = B.Position
        W.Size = UDim2.fromOffset(230, 38)
        W.BackgroundTransparency = 1
        W.BorderSizePixel = 0
        W.Active = true
        W.Visible = k.Visible
        W.ZIndex = 29
        l.Name = "Panel"
        l.Parent = W
        l.Position = UDim2.fromScale(0, 0)
        l.Size = UDim2.fromOffset(230, 38)
        l.BackgroundColor3 = Color3.fromRGB(8, 8, 13)
        l.BackgroundTransparency = UiLibraryCore.EnabledBlur and 0.055 or 0.0255
        l.BorderSizePixel = 0
        l.Active = true
        l.ClipsDescendants = true
        l.ZIndex = 30
        I.CornerRadius = UDim.new(0, 8)
        I.Parent = l
        Y.Color = Color3.fromRGB(45, 48, 58)
        Y.Transparency = 0.65
        Y.Thickness = 1
        Y.ApplyStrokeMode = Enum.ApplyStrokeMode.Border
        Y.Parent = l
        local L = UiLibraryCore:CreateShadow(l)
        L:Render(k.Visible)
        if UiLibraryCore.EnabledBlur then
            UiLibraryCore:CreateBlurModule(l, k.Signal)
        end
        q.Name = "Header"
        q.Parent = l
        q.Size = UDim2.new(1, 0, 0, 38)
        q.BackgroundTransparency = 1
        q.Active = true
        q.ZIndex = 31
        x.Name = "Divider"
        x.Parent = q
        x.AnchorPoint = Vector2.new(0, 1)
        x.Position = UDim2.fromScale(0, 1)
        x.Size = UDim2.new(1, 0, 0, 1)
        x.BackgroundColor3 = Color3.fromRGB(42, 45, 56)
        x.BackgroundTransparency = 0.3
        x.BorderSizePixel = 0
        x.ZIndex = 32
        o.Parent = q
        o.Position = UDim2.fromScale(0, 0)
        o.Size = UDim2.fromScale(1, 1)
        o.BackgroundTransparency = 1
        o.Font = Enum.Font.GothamBold
        o.Text = B.Title
        o.TextColor3 = Color3.fromRGB(235, 237, 244)
        o.TextSize = 12
        o.TextXAlignment = Enum.TextXAlignment.Center
        o.TextYAlignment = Enum.TextYAlignment.Center
        o.ZIndex = 32
        i.Parent = l
        i.Position = UDim2.fromOffset(0, 38)
        i.Size = UDim2.new(1, 0, 0, 0)
        i.BackgroundTransparency = 1
        i.ClipsDescendants = true
        i.ZIndex = 31
        E.Parent = i
        E.SortOrder = Enum.SortOrder.LayoutOrder
        local function Z()
            local B = 0
            for u, k in next, k.Items do
                if k.Visible then
                    B = B + 1
                end
            end
            i.Size = UDim2.new(1, 0, 0, B * 28)
            W.Size = UDim2.fromOffset(230, 38 + B * 28)
            l.Size = UDim2.fromOffset(230, 38 + B * 28)
        end
        UiLibraryCore.Drag(q, W, 0.1)
        function k.Add(l, B, u, W)
            local I = {
                Visible = true,
                Active = W == true,
            }
            local Y = Instance.new("Frame")
            local q = Instance.new("Frame")
            local x = Instance.new("UICorner")
            local o = Instance.new("TextLabel")
            local E = Instance.new("TextLabel")
            Y.Parent = i
            Y.Size = UDim2.new(1, 0, 0, 28)
            Y.BackgroundColor3 = UiLibraryCore.MainColor
            Y.BackgroundTransparency = 1
            Y.BorderSizePixel = 0
            Y.ZIndex = 31
            q.Parent = Y
            q.Position = UDim2.fromOffset(12, 11)
            q.Size = UDim2.fromOffset(6, 6)
            q.BorderSizePixel = 0
            q.ZIndex = 32
            x.CornerRadius = UDim.new(1, 0)
            x.Parent = q
            o.Parent = Y
            o.Position = UDim2.fromOffset(26, 0)
            o.Size = UDim2.new(1, -96, 1, 0)
            o.BackgroundTransparency = 1
            o.Font = Enum.Font.GothamMedium
            o.Text = tostring(B or "Bind")
            o.TextColor3 = Color3.fromRGB(220, 222, 230)
            o.TextSize = 11
            o.TextXAlignment = Enum.TextXAlignment.Left
            o.ZIndex = 32
            E.Parent = Y
            E.Position = UDim2.new(1, -72, 0, 0)
            E.Size = UDim2.fromOffset(60, 28)
            E.BackgroundTransparency = 1
            E.Font = Enum.Font.GothamBold
            E.Text = string.upper(tostring(u or "NONE"))
            E.TextColor3 = UiLibraryCore.AccentColor
            E.TextSize = 10
            E.TextXAlignment = Enum.TextXAlignment.Right
            E.ZIndex = 32
            local function L()
                q.BackgroundColor3 = I.Active and UiLibraryCore.AccentColor or Color3.fromRGB(68, 70, 80)
                o.TextTransparency = I.Active and 0 or 0.4
                E.TextTransparency = I.Active and 0 or 0.45
            end
            function I.SetActive(u, B)
                I.Active = B == true
                L()
            end
            function I.SetKey(u, B)
                E.Text = string.upper(tostring(B or "NONE"))
            end
            function I.SetText(u, B)
                o.Text = tostring(B or "Bind")
            end
            function I.SetVisible(u, B)
                I.Visible = B == true
                Y.Visible = I.Visible
                Z()
            end
            I.Instance = Y
            table.insert(k.Items, I)
            L()
            Z()
            return I
        end
        function k.SetVisible(u, B)
            k.Visible = B == true
            W.Visible = k.Visible
            k.Signal:SetValue(k.Visible)
            L:Render(k.Visible)
        end
        function k.Destroy(B)
            k.Signal:SetValue(false)
            L:Render(false)
            if W then
                W:Destroy()
            end
        end
        k.Instance = W
        k.Panel = l
        return k
    end
    function UiLibraryCore.Unload(u)
        if UiLibraryCore.__Unloaded then
            return
        end
        UiLibraryCore.__Unloaded = true
        for B, u in next, UiLibraryCore.Windows do
            u.__Destroyed = true
            if u.Set3DRender then
                pcall(u.Set3DRender, u, false)
            end
        end
        for B, u in next, UiLibraryCore.GlobalSignals do
            pcall(u.Disconnect, u)
        end
        table.clear(UiLibraryCore.GlobalSignals)
        table.clear(UiLibraryCore.Windows)
        table.clear(UiLibraryCore.Flags)
        if UiLibraryCore.ScreenGui and UiLibraryCore.ScreenGui.Parent then
            UiLibraryCore.ScreenGui:Destroy()
        end
        if uiEnvironment.__OIHUB_UI_V2_LIBRARY == UiLibraryCore then
            uiEnvironment.__OIHUB_UI_V2_LIBRARY = nil
        end
    end
    UiLibraryCore.DestroyAll = UiLibraryCore.Unload
    uiEnvironment.__OIHUB_UI_V2_LIBRARY = UiLibraryCore
    uiEnvironment.OiHubUIV2 = UiLibraryCore
end

-- Compatibility adapter used by the Sintonia feature UI.
do
    local adapterEnvironment = (getgenv and getgenv()) or _G
    local baseUiLibrary = adapterEnvironment.OiHubUIV2 or adapterEnvironment.__OIHUB_UI_V2_LIBRARY
    assert(baseUiLibrary, "OiHub UI Lib V2 nao carregada")
    local OiHubAdapter = {
        Config = {
            Folder = "OiHub/sintonia",
        },
    }
    local UserInputServiceAdapter = game:GetService("UserInputService")
    local TextServiceAdapter = game:GetService("TextService")
    local iconAliases = {
        user = "person",
        cross = "mascara",
        ["repeat"] = "two-arrows-spin-clockwise",
        ["trash-2"] = "trash-can",
        wrench = "gear",
        target = "crosshairs",
        crosshair = "crosshairs",
        globe = "globe-detailed",
        ["map-pin"] = "location-pin-map",
        hammer = "hammer-code",
        car = "compass",
        settings = "gear",
    }
    local function resolveIconAlias(B)
        return iconAliases[B] or B or "crosshairs"
    end
    OiHubAdapter.ResolveIcon = resolveIconAlias
    local function getExecutorFunction(B)
        return (getgenv and (getgenv())[B]) or (_G and _G[B])
    end
    local function ensureConfigFolder(B)
        local u, k = getExecutorFunction("isfolder"), getExecutorFunction("makefolder")
        if not u or not k then
            return false
        end
        local T = ""
        for B in string.gmatch(B, "[^/]+") do
            T = T == "" and B or (T .. ("/" .. B))
            if not u(T) then
                k(T)
            end
        end
        return true
    end
    function OiHubAdapter.Config.isSupported()
        return getExecutorFunction("writefile") ~= nil
            and (
                getExecutorFunction("readfile") ~= nil
                and (
                    getExecutorFunction("listfiles") ~= nil
                    and (getExecutorFunction("isfolder") ~= nil and getExecutorFunction("makefolder") ~= nil)
                )
            )
    end
    function OiHubAdapter.Config._sanitize(B)
        return (((tostring(B or "")):gsub("[^%w%-_ ]", "")):gsub("^%s+", "")):gsub("%s+$", "")
    end
    function OiHubAdapter.Config.list()
        local B, u = {}, getExecutorFunction("listfiles")
        if not u or not ensureConfigFolder(OiHubAdapter.Config.Folder) then
            return B
        end
        local T, W = pcall(u, OiHubAdapter.Config.Folder)
        if not T then
            return B
        end
        for u, k in ipairs(W) do
            local T = (tostring(k)):match("([^/\\]+)%.json$")
            if T then
                B[#B + 1] = T
            end
        end
        table.sort(B)
        return B
    end
    function OiHubAdapter.Config.save(B, u)
        B = OiHubAdapter.Config._sanitize(B)
        if B == "" then
            return false, "nome invalido"
        end
        if not OiHubAdapter.Config.isSupported() or not ensureConfigFolder(OiHubAdapter.Config.Folder) then
            return false, "executor sem filesystem"
        end
        local T, W = pcall(function()
            return (game:GetService("HttpService")):JSONEncode(u or {})
        end)
        if not T then
            return false, "falha ao serializar"
        end
        local l = pcall(getExecutorFunction("writefile"), OiHubAdapter.Config.Folder .. ("/" .. (B .. ".json")), W)
        return l, l and nil or "falha ao escrever"
    end
    function OiHubAdapter.Config.load(B)
        B = OiHubAdapter.Config._sanitize(B)
        local u = getExecutorFunction("readfile")
        if B == "" or not u then
            return nil, "config invalida"
        end
        local T, W = pcall(u, OiHubAdapter.Config.Folder .. ("/" .. (B .. ".json")))
        if not T then
            return nil, "falha ao ler"
        end
        local l, I = pcall(function()
            return (game:GetService("HttpService")):JSONDecode(W)
        end)
        return l and I or nil, l and nil or "arquivo corrompido"
    end
    function OiHubAdapter.Config.delete(B)
        B = OiHubAdapter.Config._sanitize(B)
        local u = getExecutorFunction("delfile")
        if B == "" or not u then
            return false, "falha ao apagar"
        end
        local T = pcall(u, OiHubAdapter.Config.Folder .. ("/" .. (B .. ".json")))
        return T, T and nil or "falha ao apagar"
    end
    local function attachTooltip(B, u)
        if u and (u ~= "" and type(B.ToolTip) == "function") then
            B:ToolTip(u)
        end
    end
    local function wrapNativeControl(B, u, k, T)
        local W = {
            Instance = u.Root,
            Native = B,
        }
        function W.Get(u)
            local T = B:GetValue()
            return k and k(T) or T
        end
        function W.Set(k, u)
            B:SetValue(T and T(u) or u)
        end
        function W.SetOptions(k, u)
            if B.SetValues then
                B:SetValues(u or {})
            end
        end
        return W
    end
    local SectionAdapter = {}
    SectionAdapter.__index = SectionAdapter
    function SectionAdapter._order(B)
        B._nextOrder = (B._nextOrder or 0) + 1
        return B._nextOrder
    end
    local function assignLayoutOrder(B, u)
        if u then
            u.LayoutOrder = B:_order()
        end
    end
    function SectionAdapter._row(u, B)
        local k = u.Native:AddLabel(tostring(B.Text or B.Name or "Controle"))
        assignLayoutOrder(u, k.Root)
        attachTooltip(k, B.Hint or B.ToolTip)
        return k
    end
    function SectionAdapter.AddToggle(u, B)
        B = B or {}
        local k = u:_row(B)
        return wrapNativeControl(
            k:AddToggle({
                Default = B.Default == true,
                Callback = B.Callback or function() end,
                Flag = B.Flag,
            }),
            k
        )
    end
    function SectionAdapter.AddSlider(u, B)
        B = B or {}
        local k = u:_row(B)
        local T = tonumber(B.Step) or 1
        local W = (tostring(T)):match("%.(%d+)")
        return wrapNativeControl(
            k:AddSlider({
                Default = tonumber(B.Default) or 0,
                Min = tonumber(B.Min) or 0,
                Max = tonumber(B.Max) or 100,
                Rounding = W and #W or 0,
                Type = B.Suffix or "",
                Callback = B.Callback or function() end,
                Flag = B.Flag,
            }),
            k
        )
    end
    function SectionAdapter.AddDropdown(u, B)
        B = B or {}
        local k = u:_row(B)
        return wrapNativeControl(
            k:AddDropdown({
                Default = B.Default,
                Values = B.Options or B.Values or {},
                Multi = B.Multi == true,
                Callback = B.Callback or function() end,
                Flag = B.Flag,
            }),
            k
        )
    end
    function SectionAdapter.AddTextbox(u, B)
        B = B or {}
        local k = u:_row(B)
        return wrapNativeControl(
            k:AddTextInput({
                Default = B.Default or "",
                Placeholder = B.Placeholder or "",
                Numeric = B.Numeric == true,
                Callback = B.Callback or function() end,
                Flag = B.Flag,
            }),
            k
        )
    end
    SectionAdapter.AddTextInput = SectionAdapter.AddTextbox
    function SectionAdapter.AddKeybind(k, B)
        B = B or {}
        local W = k:_row(B)
        local l = B.Default
        local function I(B)
            if typeof(B) == "EnumItem" then
                return B
            end
            local u, k = pcall(function()
                return Enum.KeyCode[tostring(B)]
            end)
            return u and k or B
        end
        local Y = W:AddKeybind({
            Default = typeof(l) == "EnumItem" and l.Name or l,
            Callback = function(u)
                l = I(u)
                if B.OnChanged then
                    B.OnChanged(l)
                end
            end,
            Flag = B.Flag,
        })
        baseUiLibrary:AddSignal(UserInputServiceAdapter.InputBegan:Connect(function(u, k)
            if
                not k
                and (
                    u.UserInputType == Enum.UserInputType.Keyboard
                    and (not UserInputServiceAdapter:GetFocusedTextBox() and (B.Callback and u.KeyCode == I(l)))
                )
            then
                B.Callback()
            end
        end))
        return wrapNativeControl(Y, W, function()
            return I(l)
        end, function(B)
            l = I(B)
            return typeof(l) == "EnumItem" and l.Name or l
        end)
    end
    function SectionAdapter.AddButton(u, B)
        B = B or {}
        local k = u.Native:AddButton({
            Name = tostring(B.Text or B.Name or "Button"),
            Icon = "mouse-button-left",
            ToolTip = B.Hint or B.ToolTip,
            Callback = B.Callback or function() end,
        })
        assignLayoutOrder(u, k.Root)
        return {
            Instance = k.Root,
            Label = k.Label,
            Icon = k.Icon,
            Native = k,
        }
    end
    function SectionAdapter.AddLabel(k, B)
        local T = k.Native:AddLabel(tostring(B or ""), true)
        assignLayoutOrder(k, T.Root)
        local l = T.Root and T.Root:FindFirstChildWhichIsA("TextLabel")
        if not l then
            return {
                Instance = T.Root,
                Native = T,
                SetText = function(B, u)
                    T:SetText(u)
                end,
            }
        end
        l.TextWrapped = true
        l.TextTruncate = Enum.TextTruncate.None
        l.TextYAlignment = Enum.TextYAlignment.Center
        local I = l.Position.Y.Offset
        if I < 0 then
            I = 0
        end
        l.Size = UDim2.new(1, -22, 1, -I * 2)
        local Y = T.Root.Size.Y.Offset
        if Y < 2 then
            Y = 30
        end
        local function q()
            if not T.Root.Parent then
                return
            end
            local B = l.AbsoluteSize.X
            if B <= 1 then
                B = math.max(1, T.Root.AbsoluteSize.X - 22)
            end
            local u = l.RichText and (l.Text:gsub("<[^>]->", "")) or l.Text
            local k = (TextServiceAdapter:GetTextSize(u, l.TextSize, l.Font, Vector2.new(B, math.huge))).Y
            T.Root.Size = UDim2.new(1, 0, 0, math.max(Y, (k + I * 2) + 4))
        end
        baseUiLibrary:AddSignal((l:GetPropertyChangedSignal("Text")):Connect(q))
        baseUiLibrary:AddSignal((T.Root:GetPropertyChangedSignal("AbsoluteSize")):Connect(q))
        task.defer(q)
        task.delay(0.25, q)
        return {
            Instance = l,
            Native = T,
            SetText = function(B, u)
                l.Text = tostring(u)
            end,
        }
    end
    function OiHubAdapter.MergeKeybindRow(B, u)
        local k = B and B.Instance
        local T = u and u.Instance
        if not k or not T then
            return u
        end
        local function W(B)
            for B, u in ipairs(B:GetChildren()) do
                if u:IsA("Frame") and u:FindFirstChildWhichIsA("UIListLayout") then
                    return u
                end
            end
        end
        local l, I = W(T), W(k)
        if not l or not I then
            return u
        end
        local Y = 0
        for B, u in ipairs(I:GetChildren()) do
            if u:IsA("GuiObject") and u.LayoutOrder < Y then
                Y = u.LayoutOrder
            end
        end
        for B, u in ipairs(l:GetChildren()) do
            if u:IsA("GuiObject") then
                u.LayoutOrder = Y - 1
                u.Parent = I
            end
        end
        T:Destroy()
        return u
    end
    function SectionAdapter.AddDivider(u, B)
        return u:AddLabel("— " .. (tostring(B or "") .. " —"))
    end
    local ColumnAdapter = {}
    ColumnAdapter.__index = ColumnAdapter
    function ColumnAdapter.AddSection(u, B)
        local k = u.Tab.Native:AddSection({
            Name = tostring(B or "SECAO"),
            Position = u.Position,
        })
        return setmetatable({
            Native = k,
            Body = k.Root,
            Container = k.Frame,
            _nextOrder = 0,
        }, SectionAdapter)
    end
    local TabAdapter = {}
    TabAdapter.__index = TabAdapter
    function TabAdapter.AddColumn(B)
        B.ColumnCount = B.ColumnCount + 1
        return setmetatable({
            Tab = B,
            Position = B.ColumnCount % 2 == 1 and "left" or "right",
        }, ColumnAdapter)
    end
    function TabAdapter.AddSection(u, B)
        if not u.DefaultColumn then
            u.DefaultColumn = u:AddColumn()
        end
        return u.DefaultColumn:AddSection(B)
    end
    local SideTabAdapter = {}
    SideTabAdapter.__index = SideTabAdapter
    function SideTabAdapter.AddTopTab(u, B)
        B = B or {}
        return setmetatable({
            Native = u.Window.Native:AddTab({
                Name = B.Name or "Tab",
                Icon = resolveIconAlias(B.Icon),
            }),
            ColumnCount = 0,
        }, TabAdapter)
    end
    local WindowAdapter = {}
    WindowAdapter.__index = WindowAdapter
    function WindowAdapter._track(k, B)
        return baseUiLibrary:AddSignal(B)
    end
    function WindowAdapter.SetVisible(u, B)
        B = B == true
        u._targetVisible = B
        u.Native:SetVisible(B)
    end
    function WindowAdapter.Toggle(B)
        local u = os.clock()
        if u - (B._lastToggleAt or 0) < 0.22 then
            return
        end
        B._lastToggleAt = u
        B:SetVisible(not B._targetVisible)
    end
    WindowAdapter.ToggleInterface = WindowAdapter.Toggle
    function WindowAdapter.SetUser(k, B)
        B = B or {}
        k.Native:SetAccount({
            Username = B.Username or (game:GetService("Players")).LocalPlayer.DisplayName,
            Profile = B.Profile or baseUiLibrary.UserProfile,
            Expires = "Oi Membro",
        })
    end
    function WindowAdapter.Notify(u, B)
        return u.Native:Notify(B)
    end
    function WindowAdapter.Destroy(B)
        return B.Native:Destroy()
    end
    function WindowAdapter.AddSideTab(u, B)
        B = B or {}
        u.Native:AddTabLabel(B.Name or B.Group or "SECAO")
        return setmetatable({
            Window = u,
        }, SideTabAdapter)
    end
    local adapterTheme = {
        Background = Color3.fromRGB(8, 8, 13),
        Panel = Color3.fromRGB(17, 18, 24),
        Card = Color3.fromRGB(25, 27, 33),
        Border = Color3.fromRGB(45, 48, 58),
        Accent = baseUiLibrary.AccentColor,
        Text = Color3.fromRGB(235, 237, 244),
        TextMuted = Color3.fromRGB(150, 152, 160),
        Cyan = Color3.fromRGB(75, 245, 255),
    }
    function OiHubAdapter.CreateWindow(k, B)
        B = B or {}
        local T = baseUiLibrary:ResolveImage(B.LogoUrl, "sintonia-logo.png", baseUiLibrary.OiHubIcon)
        local W = baseUiLibrary:CreateWindow({
            Name = "Oi Hub",
            Content = B.Subtitle or "Sintonia V2",
            Logo = T,
            TitleImage = baseUiLibrary.OiHubWordmark,
            Size = UDim2.fromOffset(tonumber(B.Width) or 900, tonumber(B.Height) or 560),
            ConfigFolder = "OiHub/configs/sintonia-v2",
            Enable3DRenderer = false,
            Responsive = true,
            Keybind = false,
        })
        local l = tonumber(B.ToggleButtonSize) or 48
        local I = tonumber(B.ToggleButtonImageInset) or 0
        local Y = Instance.new("ImageButton")
        Y.Name = "OiHubSintoniaToggle"
        Y.Parent = baseUiLibrary.ScreenGui
        Y.Position = UDim2.new(0, 14, 0.5, -l / 2)
        Y.Size = UDim2.fromOffset(l, l)
        Y.BackgroundColor3 = adapterTheme.Panel
        Y.BackgroundTransparency = 0.08
        Y.BorderSizePixel = 0
        Y.Image = ""
        Y.ZIndex = 80
        local q = Instance.new("UICorner", Y)
        q.CornerRadius = UDim.new(1, 0)
        local x = Instance.new("UIStroke", Y)
        x.Color = adapterTheme.Border
        x.Thickness = 1
        local o = Instance.new("ImageLabel")
        o.Name = "Glyph"
        o.BackgroundTransparency = 1
        o.Position = UDim2.fromOffset(I, I)
        o.Size = UDim2.new(1, -I * 2, 1, -I * 2)
        o.Image = baseUiLibrary:ResolveImage(B.ToggleButtonImage, "sintonia-toggle.png", T)
        o.ScaleType = Enum.ScaleType.Fit
        o.ZIndex = 81
        o.Parent = Y;
        (Instance.new("UICorner", o)).CornerRadius = UDim.new(1, 0)
        local i = setmetatable({
            Native = W,
            Root = W.Frame,
            ScreenGui = baseUiLibrary.ScreenGui,
            ToggleButton = Y,
            Theme = adapterTheme,
            _targetVisible = true,
        }, WindowAdapter)
        baseUiLibrary:AddSignal(Y.Activated:Connect(function()
            i:Toggle()
        end))
        return i
    end
    function OiHubAdapter.CreateLoader(u, B)
        local k = {
            Closed = false,
        }
        function k.Close(B)
            B.Closed = true
        end
        task.defer(function()
            if not k.Closed and (B and B.OnLoad) then
                B.OnLoad()
            end
        end)
        return k
    end
    function OiHubAdapter.DestroyAll(B)
        baseUiLibrary:Unload()
    end
    adapterEnvironment.OiHubUI = OiHubAdapter
    adapterEnvironment.__OIHUB_UI_LIBRARY = OiHubAdapter
    adapterEnvironment.__KNWARE_UI_LIBRARY = OiHubAdapter
end

-- Main Sintonia runtime, shared state, and feature configuration.
local PlayersService = game:GetService("Players")
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")
local TweenService = game:GetService("TweenService")
local PathfindingService = game:GetService("PathfindingService")
local WorkspaceService = game:GetService("Workspace")
local localPlayer = PlayersService.LocalPlayer
local mainEnvironment = getgenv and getgenv() or _G
local targetPlaceId = "125299131646017"
local espMinimumDistance = 50
local espMaximumDistance = 5000
local defaultEspDistance = 1500
local defaultMenuKey = Enum.KeyCode.K
if mainEnvironment.__KNWARE_SINTONIA_CLEANUP then
    pcall(mainEnvironment.__KNWARE_SINTONIA_CLEANUP)
end
if mainEnvironment.__KNWARE_MOBILIDADE_CLEANUP then
    pcall(mainEnvironment.__KNWARE_MOBILIDADE_CLEANUP)
    mainEnvironment.__KNWARE_MOBILIDADE_CLEANUP = nil
end
local runtimeState = {
    connections = {},
    placeMatched = tostring(game.PlaceId) == targetPlaceId,
    destroyed = false,
    itemTeleportRunId = 0,
    farmPartesRunId = 0,
    farmPartesColetadas = 0,
    bestWeaponRunId = 0,
    bestWeaponLastAttempt = 0,
    bestWeaponLastName = nil,
    garbageFarmRunId = 0,
    garbageFarmStatus = "Parado",
    durabilidadeRunId = 0,
    relationships = {},
    relationshipConnections = {},
}
local featureSettings = {
    espEnabled = false,
    box = false,
    name = false,
    distance = false,
    heldItem = false,
    skeleton = false,
    highlight = false,
    tracer = false,
    arrow = false,
    box3d = false,
    teamTag = false,
    maxDistance = defaultEspDistance,
    bundles = {},
    spoofWalkEnabled = false,
    spoofJumpEnabled = false,
    spoofWalkSpeed = 40,
    spoofJumpPower = 90,
    walkSpeedKeybind = Enum.KeyCode.V,
    menuKeybind = defaultMenuKey,
    antiRagdollEnabled = false,
    antiRagdollDelay = 0,
    antiTiroEnabled = false,
    rolarEnabled = false,
    noclipEnabled = false,
    noclipKeybind = Enum.KeyCode.N,
    flyEnabled = false,
    flyKeybind = Enum.KeyCode.F,
    flySpeed = 60,
    silentAimEnabled = false,
    silentAimFov = 120,
    silentAimDistance = 500,
    silentAimVisibleOnly = false,
    silentAimFovVisible = false,
    silentAimExpandido = false,
    silentAimHitChance = 100,
    silentAimBodyPart = "Cabeca",
    friendCheck = true,
    adminCheck = true,
    armaNoRecoil = false,
    armaMunicaoInfinita = false,
    armaReloadRapido = false,
    armaSemLentidao = false,
    armaCadenciaCustom = false,
    armaTirosPorSegundo = 10,
    armaTracer = false,
    armaTracerCor = "Ciano",
    perfectLockpick = false,
    roubosHudEnabled = false,
    coletaHudEnabled = false,
    teleportHudEnabled = false,
    bestWeaponEnabled = false,
    itemTeleportSpeed = 65,
    farmPartesEnabled = false,
    autoFarmLixeiroEnabled = false,
    autoFarmLixeiroQuantidade = 20,
    durabilidadeHudEnabled = false,
    bestCarsEspEnabled = false,
    autoCintoEnabled = false,
    noclipCarEnabled = false,
    vFlyEnabled = false,
    vFlyHorizontalSpeed = 60,
    vFlyVerticalSpeed = 30,
    floatingRepairEnabled = false,
    floatingStealEnabled = false,
    boostFactor = 1,
    alwaysDay = false,
    fullbright = false,
    noShadow = false,
    noFog = false,
    noBlur = false,
}
local registeredControls = {}
local function registerControl(B, u)
    if u then
        registeredControls[B] = u
    end
    return u
end
local function trackConnection(B)
    table.insert(runtimeState.connections, B)
    return B
end
local GuiService = game:GetService("GuiService")
local function getScreenBounds(B)
    local u = WorkspaceService.CurrentCamera
    local k = u and u.ViewportSize or Vector2.new(800, 600)
    local T = B:FindFirstAncestorWhichIsA("ScreenGui")
    local W = GuiService:GetGuiInset()
    if T and T.IgnoreGuiInset then
        return W, k
    end
    return Vector2.zero, Vector2.new(k.X, k.Y - W.Y)
end
local function makeDraggable(B, u, T)
    u = u or B
    local W, l, I
    trackConnection(u.InputBegan:Connect(function(u)
        local k = u.UserInputType
        if k ~= Enum.UserInputType.MouseButton1 and k ~= Enum.UserInputType.Touch then
            return
        end
        W, l, I = k, u.Position, B.AbsolutePosition
        trackConnection(u.Changed:Connect(function()
            if u.UserInputState == Enum.UserInputState.End then
                W = nil
                if T then
                    T(B.Position)
                end
            end
        end))
    end))
    trackConnection(UserInputService.InputChanged:Connect(function(u)
        if not W then
            return
        end
        local k = u.UserInputType
        if
            not (
                (W == Enum.UserInputType.MouseButton1 and k == Enum.UserInputType.MouseMovement)
                or (W == Enum.UserInputType.Touch and k == Enum.UserInputType.Touch)
            )
        then
            return
        end
        local T, Y = getScreenBounds(B)
        local q = B.AbsoluteSize
        local x = (I.X + (u.Position.X - l.X)) + T.X
        local o = (I.Y + (u.Position.Y - l.Y)) + T.Y
        B.Position = UDim2.fromOffset(
            math.clamp(x, T.X + 8, math.max(T.X + 8, (Y.X - q.X) - 8)),
            math.clamp(o, T.Y + 8, math.max(T.Y + 8, (Y.Y - q.Y) - 8))
        )
    end))
end
local hudBackgroundColor = Color3.fromRGB(8, 8, 13)
local hudBorderColor = Color3.fromRGB(45, 48, 58)
local hudDividerColor = Color3.fromRGB(42, 45, 56)
local hudTextColor = Color3.fromRGB(235, 237, 244)
local function styleHudFrame(B, u)
    u = u or {}
    local k = mainEnvironment.OiHubUIV2 or mainEnvironment.__OIHUB_UI_V2_LIBRARY
    local T = k and k.EnabledBlur
    B.BackgroundColor3 = hudBackgroundColor
    B.BackgroundTransparency = T and 0.055 or 0.0255
    B.BorderSizePixel = 0
    local W = B:FindFirstChildOfClass("UICorner") or Instance.new("UICorner")
    W.CornerRadius = UDim.new(0, 8)
    W.Parent = B
    local l = B:FindFirstChildOfClass("UIStroke") or Instance.new("UIStroke")
    l.Color = hudBorderColor
    l.Transparency = 0.65
    l.Thickness = 1
    l.ApplyStrokeMode = Enum.ApplyStrokeMode.Border
    l.Parent = B
    if u.Divisor then
        local k = Instance.new("Frame")
        k.Name = "Divisor"
        k.BackgroundColor3 = hudDividerColor
        k.BackgroundTransparency = 0.3
        k.BorderSizePixel = 0
        k.Position = UDim2.fromOffset(0, u.Divisor)
        k.Size = UDim2.new(1, 0, 0, 1)
        k.ZIndex = (B.ZIndex or 1) + 1
        k.Parent = B
    end
    if u.Titulo then
        u.Titulo.Font = Enum.Font.GothamBold
        u.Titulo.TextSize = 12
        u.Titulo.TextColor3 = hudTextColor
    end
    local I = {
        Frame = B,
    }
    local q = k and k:CreateSignal(false)
    local x = k and k:CreateShadow(B)
    if T and q then
        k:CreateBlurModule(B, q)
    end
    function I.SetVisible(k, u)
        u = u == true
        B.Visible = u
        if q then
            q:SetValue(u)
        end
        if x then
            x:Render(u)
        end
    end
    I:SetVisible(B.Visible)
    return I
end
(function()
    local B = game:GetService("ReplicatedStorage")
    local function u(B)
        return (getgenv and (getgenv())[B]) or (_G and _G[B])
    end
    local k = u("filtergc")
    local T = u("hookfunction")
    local W = u("newcclosure")
    local l = debug or {}
    local Y, q = l.getconstants, l.getprotos
    local x, o = l.getupvalues, l.setupvalue
    if not (k and (T and (W and (Y and (q and (x and o)))))) then
        runtimeState.acStatus = "Executor sem filtergc/hookfunction/debug.* — AC intacto."
        return
    end
    local i = {}
    local function E(B, u, k)
        k = k or 0
        if k > 8 then
            return false
        end
        local T, W = pcall(Y, B)
        if T and type(W) == "table" then
            for B, k in next, W do
                if type(k) == "string" and u(k) then
                    return true
                end
            end
        end
        local l, I = pcall(q, B)
        if l and type(I) == "table" then
            for B, T in next, I do
                if E(T, u, k + 1) then
                    return true
                end
            end
        end
        return false
    end
    local function Z(B)
        return E(B, function(B)
            return string.find(B, "__ACK__", 1, true) ~= nil
        end)
    end
    local J = {
        "Hitbox Expander",
        "ESP Detectado",
    }
    local function F(B)
        local u, k = false, true
        E(B, function(B)
            if #B < 4 or not string.find(B, " ", 1, true) then
                return false
            end
            u = true
            for u, k in ipairs(J) do
                if string.find(B, k, 1, true) then
                    return false
                end
            end
            k = false
            return true
        end)
        return u and k
    end
    local d = {
        "CFrame TP: ",
        "Tween TP: ",
        "TP Absurdo",
        "Velocity",
        "VFly",
        "WalkFling IY",
        "Noclip",
        "WalkSpeed: ",
        "JumpPower: ",
        "Fly mal feito",
    }
    local X = {
        FireServer = function() end,
    }
    local s = {
        WaitForChild = function()
            return X
        end,
        FindFirstChild = function()
            return X
        end,
    }
    local D = {
        "CFrame TP: ",
        "TP Absurdo",
        "VFly",
    }
    local function H(B)
        local u, k = pcall(Y, B)
        if not u or type(k) ~= "table" then
            return false
        end
        for B, u in next, k do
            if type(u) == "string" then
                for B, k in ipairs(D) do
                    if u == k then
                        return true
                    end
                end
            end
        end
        return false
    end
    local function R()
        local B, u = pcall(k, "function", {
            Constants = {
                "VFly",
            },
            IgnoreExecutor = true,
        })
        if not B or type(u) ~= "table" then
            return nil
        end
        return u
    end
    local function m(u)
        local k, T = pcall(x, u)
        if not k or type(T) ~= "table" then
            return true
        end
        for u, k in next, T do
            if k == B then
                return true
            end
        end
        return false
    end
    local function w(B)
        local u = localPlayer.Character
        local k = u and u:FindFirstChild("HumanoidRootPart")
        if not k then
            return false
        end
        local T, W = pcall(x, B)
        if not T or type(W) ~= "table" then
            return false
        end
        for B, u in next, W do
            if u == k then
                return true
            end
        end
        return false
    end
    local function v(u)
        local k, T = pcall(x, u)
        if not k or type(T) ~= "table" then
            return false
        end
        for k, T in next, T do
            if T == B then
                return (pcall(o, u, k, s)) and true or false
            end
        end
        return true
    end
    local e = u("getconnections")
    local function G(B, u)
        u = u or 0
        if u > 6 then
            return nil
        end
        local k, T = pcall(Y, B)
        if k and type(T) == "table" then
            for B, u in next, T do
                if type(u) == "string" then
                    for B, k in ipairs(d) do
                        if u == k then
                            return k
                        end
                    end
                end
            end
        end
        local W, l = pcall(q, B)
        if W and type(l) == "table" then
            for B, k in next, l do
                local T = G(k, u + 1)
                if T then
                    return T
                end
            end
        end
        return nil
    end
    local function U()
        local B = {}
        local u = localPlayer.Character
        local k = u and u:FindFirstChildOfClass("Humanoid")
        if k then
            B[#B + 1] = k:GetPropertyChangedSignal("WalkSpeed")
            B[#B + 1] = k:GetPropertyChangedSignal("JumpPower")
            B[#B + 1] = k.StateChanged
        end
        if u then
            B[#B + 1] = u.DescendantAdded
            for u, k in ipairs(u:GetChildren()) do
                if k:IsA("BasePart") then
                    B[#B + 1] = k:GetPropertyChangedSignal("CanCollide")
                end
            end
        end
        B[#B + 1] = (game:GetService("RunService")).Heartbeat
        return B
    end
    local function f()
        if not e then
            return nil
        end
        local B, u = {}, {}
        for k, T in ipairs(U()) do
            local W, l = pcall(e, T)
            if W and type(l) == "table" then
                for k, T in ipairs(l) do
                    local W = T.Function
                    if type(W) == "function" and T.Connected then
                        local k = G(W)
                        if k and not B[k] then
                            B[k] = true
                            u[#u + 1] = k
                        end
                    end
                end
            end
        end
        local k = R()
        if k == nil then
            return nil
        end
        local T, W = false, false
        for B, u in next, k do
            if w(u) then
                if m(u) then
                    W = true
                else
                    T = true
                end
            end
        end
        local l = W or not T
        if l and not B["CFrame TP: "] then
            B["CFrame TP: "] = true
            u[#u + 1] = "CFrame TP: "
        end
        return u
    end
    local function p()
        if not B:FindFirstChild("SignalSendACK") then
            return 0
        end
        local u, l = pcall(k, "function", {
            Constants = {
                "SignalSendACK",
                "FireServer",
            },
            IgnoreExecutor = true,
        })
        if not u or type(l) ~= "table" then
            return 0
        end
        local I, Y = 0, 0
        for B, u in next, l do
            if not i[u] then
                if H(u) then
                    if v(u) then
                        i[u] = true
                        I = I + 1
                    end
                else
                    i[u] = true
                    if not Z(u) and not F(u) then
                        if pcall(T, u, W(function() end)) then
                            I = I + 1
                        end
                    end
                end
                Y = Y + 1
                if Y >= 200 then
                    Y = 0
                    task.wait()
                end
            end
        end
        return I
    end
    local y = false
    local function P(B)
        if y or runtimeState.destroyed then
            return
        end
        y = true
        runtimeState.acSilenciados = 0
        runtimeState.acMotivosVivos = nil
        runtimeState.acStatus = "Aplicando (" .. (B .. ")...")
        task.spawn(function()
            local u, k = 0, nil
            for B = 1, 120, 1 do
                if runtimeState.destroyed then
                    break
                end
                u = u + p()
                k = f()
                if k == nil then
                    break
                end
                if #k == 0 then
                    break
                end
                task.wait(0.5)
            end
            y = false
            runtimeState.acSilenciados = u
            runtimeState.acMotivosVivos = k
            if k == nil then
                runtimeState.acStatus = u
                    .. (" silenciado(s) (" .. (B .. "), mas sem getconnections nao da pra provar — travas ligadas."))
            elseif #k > 0 then
                runtimeState.acStatus = "AC PARCIAL ("
                    .. (
                        B
                        .. (
                            "): "
                            .. (
                                u
                                .. (
                                    " silenciado(s), mas ainda respondem: "
                                    .. (table.concat(k, ", ") .. ". As travas de velocidade continuam ligadas.")
                                )
                            )
                        )
                    )
            elseif u > 0 then
                runtimeState.acStatus = u
                    .. (" denunciante(s) silenciado(s) (" .. (B .. "); nenhum motivo critico responde."))
            else
                runtimeState.acStatus = "Nenhum denunciante encontrado (" .. (B .. ") — AC pode estar intacto.")
            end
        end)
    end
    runtimeState.acReaplicar = P
    P("carga")
    trackConnection(localPlayer.CharacterAdded:Connect(function()
        P("respawn")
    end))
    if game:FindService("VirtualUser") or game:FindService("VirtualInputManager") then
        runtimeState.acAviso =
            'VirtualUser/VirtualInputManager existe neste cliente: o report "Auto-farming" sai pela closure que carrega o heartbeat e NAO e silenciado.'
    end
end)()
do
    local function B()
        local B = game:GetService("Lighting")
        local u = featureSettings.alwaysDay
            or featureSettings.fullbright
            or featureSettings.noShadow
            or featureSettings.noFog
            or featureSettings.noBlur
        if not runtimeState.worldOriginal and u then
            runtimeState.worldOriginal = {
                Brightness = B.Brightness,
                ClockTime = B.ClockTime,
                FogEnd = B.FogEnd,
                FogStart = B.FogStart,
                GlobalShadows = B.GlobalShadows,
                Ambient = B.Ambient,
                OutdoorAmbient = B.OutdoorAmbient,
                ExposureCompensation = B.ExposureCompensation,
            }
        end
        local k = runtimeState.worldOriginal
        if not u then
            if k then
                for u, k in pairs(k) do
                    pcall(function()
                        B[u] = k
                    end)
                end
            end
            return
        end
        if featureSettings.alwaysDay or featureSettings.fullbright then
            B.ClockTime = 14
        elseif k then
            B.ClockTime = k.ClockTime
        end
        if featureSettings.fullbright then
            B.Brightness = 2
            B.ExposureCompensation = 0
            B.Ambient = Color3.new(1, 1, 1)
            B.OutdoorAmbient = Color3.new(1, 1, 1)
        elseif k then
            B.Brightness = k.Brightness
            B.ExposureCompensation = k.ExposureCompensation
            B.Ambient = k.Ambient
            B.OutdoorAmbient = k.OutdoorAmbient
        end
        if featureSettings.noShadow or featureSettings.fullbright then
            B.GlobalShadows = false
        elseif k then
            B.GlobalShadows = k.GlobalShadows
        end
        if featureSettings.noFog or featureSettings.fullbright then
            B.FogStart = 0
            B.FogEnd = 1000000000
            local u = B:FindFirstChildOfClass("Atmosphere")
            if u then
                u.Density = 0
            end
        elseif k then
            B.FogStart = k.FogStart
            B.FogEnd = k.FogEnd
        end
        if featureSettings.noBlur then
            for B, u in ipairs(B:GetDescendants()) do
                if u:IsA("BlurEffect") then
                    u.Enabled = false
                end
            end
        end
    end
    runtimeState.applyWorld = B
    local k = 0
    trackConnection(RunService.Heartbeat:Connect(function(u)
        if featureSettings.alwaysDay or featureSettings.fullbright then
            pcall(function()
                (game:GetService("Lighting")).ClockTime = 14
            end)
        end
        k = k + u
        if k < 0.25 then
            return
        end
        k = 0
        if
            featureSettings.alwaysDay
            or featureSettings.fullbright
            or featureSettings.noShadow
            or featureSettings.noFog
            or featureSettings.noBlur
        then
            pcall(B)
        end
    end))
end
do
    local B = 0
    local function k()
        local B = localPlayer:FindFirstChild("Status")
        local u = B and B:FindFirstChild("Cinto")
        if not u or u.Value then
            return
        end
        local k = localPlayer.Character
        local T = k and k:FindFirstChildOfClass("Humanoid")
        local W = T and T.SeatPart
        if not W or W.Name ~= "DriveSeat" then
            return
        end
        u.Value = true
        pcall(function()
            localPlayer.PlayerGui.OrpheusCarInterface.Velocimetro.Icons.Cinto.Visible = true
        end)
        pcall(function()
            local B = W:FindFirstChild("ColocaCinto")
            if B then
                B:Play()
            end
        end)
    end
    trackConnection(RunService.Heartbeat:Connect(function(u)
        if not featureSettings.autoCintoEnabled then
            return
        end
        B = B + u
        if B < 0.3 then
            return
        end
        B = 0
        pcall(k)
    end))
end
-- Character movement, anti-ragdoll, noclip, and flight helpers.
local function getHumanoid()
    local B = localPlayer.Character
    return B and B:FindFirstChildOfClass("Humanoid") or nil
end
local function getRootPart()
    local B = localPlayer.Character
    return B and B:FindFirstChild("HumanoidRootPart")
end
local lastMoveDirection = Vector3.new(0, 0, 0)
task.spawn(function()
    while not runtimeState.destroyed do
        local B = getHumanoid()
        if B then
            lastMoveDirection = B.MoveDirection
        end
        task.wait(0.05)
    end
end)
local baseWalkSpeed = 28
local function isAntiCheatSilenced()
    local B = runtimeState.acMotivosVivos
    return (runtimeState.acSilenciados or 0) > 0 and (B ~= nil and #B == 0)
end
local function applyMovementSpoof(B)
    if not B then
        return
    end
    if featureSettings.spoofWalkEnabled then
        local u = math.min(featureSettings.spoofWalkSpeed, baseWalkSpeed)
        if B.WalkSpeed ~= u then
            B.WalkSpeed = u
        end
    end
    if featureSettings.spoofJumpEnabled then
        if B.JumpPower ~= 50 then
            B.JumpPower = 50
        end
        if not B.UseJumpPower then
            B.UseJumpPower = true
        end
    end
end
local blockedMovementStates = {
    [Enum.HumanoidStateType.Seated] = true,
    [Enum.HumanoidStateType.Dead] = true,
    [Enum.HumanoidStateType.Climbing] = true,
}
local movementHistoryWindow = 0.5
local strictMovementDistanceLimit = 18
local relaxedMovementDistanceLimit = 34
local safeMovementSpeedCap = 36
local strictLimitFrameThreshold = 0.08
local movementReuseThreshold = 16
local function sumRecentMovement(B, u)
    local k = 0
    for T = #B, 1, -1 do
        if u - B[T].t > movementHistoryWindow then
            table.remove(B, T)
        else
            k = k + B[T].passo
        end
    end
    return k
end
local function appendMovementSample(B, u, k, T)
    if not k then
        return
    end
    local W = (T - k).Magnitude
    if W > 0 then
        table.insert(B, {
            t = u,
            passo = W,
        })
    end
end
local movementTracker = {
    root = nil,
    posicao = nil,
    ultimaAmostra = 0,
    historico = {},
}
local function measureRecentMovement(B, u)
    local k = movementTracker
    if k.root ~= B or u - k.ultimaAmostra > movementHistoryWindow then
        k.root = B
        k.posicao = B.Position
        k.ultimaAmostra = u
        table.clear(k.historico)
        return 0
    end
    appendMovementSample(k.historico, u, k.posicao, B.Position)
    k.posicao = B.Position
    k.ultimaAmostra = u
    return sumRecentMovement(k.historico, u)
end
local function useStrictMovementLimit(B, u)
    local k = getHumanoid()
    if B.Anchored or not k or k.Health <= 0 or u >= strictLimitFrameThreshold then
        return true
    end
    local T = k:GetState()
    return B.AssemblyLinearVelocity.Magnitude < 12
        and (T ~= Enum.HumanoidStateType.Freefall and T ~= Enum.HumanoidStateType.Jumping)
end
local function calculateMovementBudget(B, u, k, T)
    local W = math.clamp(u or 0.016666666666667, 0.0041666666666667, movementHistoryWindow)
    local l = measureRecentMovement(B, os.clock())
    local I = not T or useStrictMovementLimit(B, W)
    local Y = T and (I and l > strictMovementDistanceLimit)
    local q = T and (I and strictMovementDistanceLimit or relaxedMovementDistanceLimit) or strictMovementDistanceLimit
    if Y then
        q = relaxedMovementDistanceLimit
    end
    local x = math.max(0, q - l)
    local o = math.min(math.max(k or 0, 0), x / W)
    local i = math.max(0, x - o * W)
    if Y then
        i = 0
    end
    return W, o, i, l, I
end
local walkStepCap = 4
local silencedWalkStepCap = 8
local maximumWalkSpeedCap = 105
local laggedWalkSpeedCap = 65
local walkLagFrameThreshold = 0.05
local walkLagPenaltyDuration = 0.6
local minimumWalkVelocity = 14
local walkVelocityAssist = 20
local idleMoveAssistSpeed = 14
local idleDirectionGracePeriod = 0.55
local lastWalkDirection = Vector3.zero
local lastWalkDirectionExpiresAt = 0
local walkPenaltyExpiresAt = 0
local maxWalkStepCarry = 4
local walkStepCarry = 0
local function applyWalkSpoofStep(B)
    if not featureSettings.spoofWalkEnabled then
        walkStepCarry = 0
        return
    end
    if featureSettings.flyEnabled or featureSettings.farmPartesEnabled then
        walkStepCarry = 0
        return
    end
    local u = getRootPart()
    if not u then
        return
    end
    local k = getHumanoid()
    if not k then
        return
    end
    if k.Sit or blockedMovementStates[k:GetState()] then
        walkStepCarry = 0
        return
    end
    if lastMoveDirection.Magnitude <= 0 then
        walkStepCarry = 0
        if os.clock() < lastWalkDirectionExpiresAt and lastWalkDirection.Magnitude > 0 then
            local B = math.max(k.WalkSpeed, 1)
            local u = math.clamp(idleMoveAssistSpeed / B, 0, 1)
            pcall(k.Move, k, lastWalkDirection * u, false)
        end
        return
    end
    lastWalkDirection = lastMoveDirection.Unit
    lastWalkDirectionExpiresAt = os.clock() + idleDirectionGracePeriod
    local T = u.AssemblyLinearVelocity
    local W = (Vector3.new(T.X, 0, T.Z)).Magnitude
    if (B or 0) >= walkLagFrameThreshold or u.Anchored or W < 12 then
        walkPenaltyExpiresAt = os.clock() + walkLagPenaltyDuration
    end
    local l = isAntiCheatSilenced()
    local I = math.min(
        featureSettings.spoofWalkSpeed,
        l and featureSettings.spoofWalkSpeed
            or (os.clock() < walkPenaltyExpiresAt and laggedWalkSpeedCap or maximumWalkSpeedCap)
    )
    local Y = I - baseWalkSpeed
    if Y <= 0 then
        walkStepCarry = 0
        return
    end
    local q = 0.9 + math.random() * 0.1
    local x = (Y * B) * q + walkStepCarry
    local o = math.min(x, Y * B, l and silencedWalkStepCap or walkStepCap)
    walkStepCarry = math.clamp(x - o, 0, maxWalkStepCarry)
    local i = lastMoveDirection.Unit
    u.CFrame = u.CFrame + i * o
    local E = u.AssemblyLinearVelocity
    if (Vector3.new(E.X, 0, E.Z)).Magnitude < minimumWalkVelocity then
        u.AssemblyLinearVelocity = Vector3.new(i.X * walkVelocityAssist, E.Y, i.Z * walkVelocityAssist)
    end
end
local function applyJumpSpoof()
    if not featureSettings.spoofJumpEnabled then
        return
    end
    local B = getRootPart()
    if not B then
        return
    end
    local u = featureSettings.spoofJumpPower - 50
    if u > 0 then
        B.Velocity = B.Velocity + Vector3.new(0, u, 0)
    end
end
local ragdollStates = {
    [Enum.HumanoidStateType.Ragdoll] = true,
    [Enum.HumanoidStateType.FallingDown] = true,
    [Enum.HumanoidStateType.Physics] = true,
    [Enum.HumanoidStateType.PlatformStanding] = true,
}
local ragdollRecovery = {
    recovering = false,
}
local function getRagdollFlag()
    local B = localPlayer.Character
    return B and B:FindFirstChild("IsRagdoll")
end
local function getRagdollClient()
    local B = localPlayer.Character
    return B and B:FindFirstChild("RagdollClient")
end
local function setRagdollDisabled(B, u)
    if not B then
        return
    end
    B:SetStateEnabled(Enum.HumanoidStateType.Ragdoll, not u)
    B:SetStateEnabled(Enum.HumanoidStateType.FallingDown, not u)
end
local function recoverFromRagdoll(B)
    if not B or B.Health <= 0 then
        return
    end
    if not B:GetStateEnabled(Enum.HumanoidStateType.GettingUp) then
        B:SetStateEnabled(Enum.HumanoidStateType.GettingUp, true)
    end
    if B.PlatformStand then
        B.PlatformStand = false
    end
    local u = getRagdollFlag()
    if u and u.Value then
        u.Value = false
    end
    if ragdollStates[B:GetState()] then
        B:ChangeState(Enum.HumanoidStateType.GettingUp)
    end
end
local function queueRagdollRecovery()
    if ragdollRecovery.recovering then
        return
    end
    ragdollRecovery.recovering = true
    task.delay(featureSettings.antiRagdollDelay / 1000, function()
        ragdollRecovery.recovering = false
        if not featureSettings.antiRagdollEnabled or runtimeState.destroyed then
            return
        end
        pcall(recoverFromRagdoll, getHumanoid())
    end)
end
local ragdollRemote
do
    local B = (game:GetService("ReplicatedStorage")):FindFirstChild("InGameRemotes")
    ragdollRemote = B and B:FindFirstChild("RagdollService")
end
local function findRagdollConnections()
    if not (getconnections and (ragdollRemote and (debug and debug.getupvalues))) then
        return {}
    end
    local B = {}
    local k, T = pcall(getconnections, RunService.Heartbeat)
    if not k then
        return {}
    end
    for u, k in ipairs(T) do
        local T, W = pcall(function()
            return k.Function
        end)
        if T and type(W) == "function" then
            local u, T = pcall(debug.getupvalues, W)
            if u and type(T) == "table" then
                for u, T in pairs(T) do
                    if T == ragdollRemote then
                        table.insert(B, k)
                        break
                    end
                end
            end
        end
    end
    return B
end
local function applyAntiRagdoll()
    local B = featureSettings.antiRagdollEnabled
    for u, k in ipairs(findRagdollConnections()) do
        pcall(function()
            if B then
                k:Disable()
            else
                k:Enable()
            end
        end)
    end
    local u = getRagdollClient()
    if u then
        u.Disabled = B
    end
    local k = getHumanoid()
    if not k then
        return
    end
    setRagdollDisabled(k, B and featureSettings.antiRagdollDelay <= 0)
    if B then
        recoverFromRagdoll(k)
    else
        k:SetStateEnabled(Enum.HumanoidStateType.GettingUp, true)
    end
end
local function updateAntiRagdoll()
    local B = getHumanoid()
    if not B or B.Health <= 0 then
        return
    end
    if B.SeatPart or localPlayer:GetAttribute("QuedaLivre") == true then
        return
    end
    if not B:GetStateEnabled(Enum.HumanoidStateType.GettingUp) then
        B:SetStateEnabled(Enum.HumanoidStateType.GettingUp, true)
    end
    local u = featureSettings.antiRagdollDelay <= 0
    if u then
        setRagdollDisabled(B, true)
    end
    local k = getRagdollFlag()
    local T = B.PlatformStand or ragdollStates[B:GetState()] ~= nil or (k ~= nil and k.Value == true)
    if not T then
        return
    end
    if u then
        recoverFromRagdoll(B)
    else
        queueRagdollRecovery()
    end
end
local characterPartNames = {
    "Head",
    "Torso",
    "Left Arm",
    "Right Arm",
    "Left Leg",
    "Right Leg",
    "HumanoidRootPart",
}
local defaultPartCollisions = {
    Head = true,
    Torso = true,
    ["Left Arm"] = false,
    ["Right Arm"] = false,
    ["Left Leg"] = false,
    ["Right Leg"] = false,
    HumanoidRootPart = false,
}
local noclipGroundHeight = nil
local function setCharacterNoclip(B)
    local u = localPlayer.Character
    if not u then
        return
    end
    for k, T in ipairs(characterPartNames) do
        local W = u:FindFirstChild(T)
        if W and W:IsA("BasePart") then
            local u = not B and defaultPartCollisions[T] or false
            if W.CanCollide ~= u then
                W.CanCollide = u
            end
        end
    end
end
local function applyCharacterNoclip()
    local B = localPlayer.Character
    if not B then
        return
    end
    local u = B:FindFirstChild("IsRagdoll")
    local k = B:FindFirstChild("RagdollClient")
    if featureSettings.noclipEnabled then
        if k then
            k.Disabled = true
        end
        if u and not u.Value then
            u.Value = true
        end
        setCharacterNoclip(true)
    else
        noclipGroundHeight = nil
        setCharacterNoclip(false)
        if u and u.Value then
            u.Value = false
        end
        if k and not featureSettings.antiRagdollEnabled then
            k.Disabled = false
        end
    end
end
local noclipJumpVelocity = 50
local function updateNoclipJump()
    local B = getRootPart()
    if not B then
        return
    end
    if not noclipGroundHeight then
        noclipGroundHeight = B.Position.Y
        return
    end
    local u = B.AssemblyLinearVelocity
    local T = B.Position.Y <= noclipGroundHeight + 0.5
    local W = getHumanoid()
    local l = (W and W.Jump) or UserInputService:IsKeyDown(Enum.KeyCode.Space)
    if T and l then
        B.AssemblyLinearVelocity = Vector3.new(u.X, noclipJumpVelocity, u.Z)
        return
    end
    if u.Y < 0 and B.Position.Y <= noclipGroundHeight then
        B.AssemblyLinearVelocity = Vector3.new(u.X, 0, u.Z)
    end
end
local flyMoveSpeedCap = 60
local flyVelocityCap = 30
local flyRequestedVelocity = 14
local flyIdleSpeedCap = flyMoveSpeedCap
local lastFlyDirection = Vector3.zero
local flyHadInput = false
local function updateFly(B)
    local u = getRootPart()
    local T = getHumanoid()
    if not u or not T then
        return
    end
    if not featureSettings.flyEnabled then
        T:SetStateEnabled(Enum.HumanoidStateType.Swimming, true)
        T:SetStateEnabled(Enum.HumanoidStateType.Jumping, true)
        if T:GetState() == Enum.HumanoidStateType.Swimming then
            local B = T.FloorMaterial == Enum.Material.Air and Enum.HumanoidStateType.Freefall
                or Enum.HumanoidStateType.Running
            pcall(T.ChangeState, T, B)
        end
        if featureSettings.farmPartesEnabled then
            flyHadInput = false
            return
        end
        if isAntiCheatSilenced() then
            flyHadInput = false
            u.AssemblyLinearVelocity = Vector3.zero
            return
        end
        local k, W, l, I = calculateMovementBudget(u, B, flyRequestedVelocity, true)
        if flyHadInput and (I > movementReuseThreshold and lastFlyDirection.Magnitude > 0) then
            u.AssemblyLinearVelocity = lastFlyDirection * W
            return
        end
        flyHadInput = false
        u.AssemblyLinearVelocity = Vector3.zero
        return
    end
    if T.Health <= 0 then
        return
    end
    if featureSettings.farmPartesEnabled then
        runtimeState.farmPartesRunId = runtimeState.farmPartesRunId + 1
        featureSettings.farmPartesEnabled = false
    end
    T:SetStateEnabled(Enum.HumanoidStateType.Jumping, false)
    T:ChangeState(Enum.HumanoidStateType.Swimming)
    local W = 0
    if UserInputService:IsKeyDown(Enum.KeyCode.Space) then
        W = 1
    elseif UserInputService:IsKeyDown(Enum.KeyCode.LeftControl) then
        W = -1
    end
    local l = T.MoveDirection + Vector3.new(0, W, 0)
    if l.Magnitude <= 0 then
        if isAntiCheatSilenced() then
            u.AssemblyLinearVelocity = Vector3.zero
            return
        end
        local k, T, W, l = calculateMovementBudget(u, B, flyRequestedVelocity, true)
        if l > movementReuseThreshold and lastFlyDirection.Magnitude > 0 then
            u.AssemblyLinearVelocity = lastFlyDirection * T
        else
            u.AssemblyLinearVelocity = Vector3.zero
        end
        return
    end
    l = l.Unit
    lastFlyDirection = l
    flyHadInput = true
    if isAntiCheatSilenced() then
        u.AssemblyLinearVelocity = l * featureSettings.flySpeed
        return
    end
    local I = T.MoveDirection.Magnitude > 0 and flyMoveSpeedCap or flyIdleSpeedCap
    local Y = math.min(featureSettings.flySpeed, I)
    local q = math.min(Y, flyVelocityCap)
    local x, o, i, E, J = calculateMovementBudget(u, B, q, true)
    local F = math.min(Y, J and safeMovementSpeedCap or I)
    local b = math.max(0, F - o)
    if b > 0 and i > 0 then
        u.CFrame = u.CFrame + l * math.min(b * x, i)
    end
    u.AssemblyLinearVelocity = l * o
end
local rollAnimationState = {
    track = nil,
}
local function findRollAnimation()
    local B = localPlayer.Character
    local u = B and B:FindFirstChild("mainClient")
    if not u then
        return nil
    end
    return u:FindFirstChild("QuedaAnim") or u:FindFirstChild("QuedaAnim2")
end
local function updateRollAnimation()
    if rollAnimationState.track then
        pcall(function()
            rollAnimationState.track:Stop(0.2)
        end)
        rollAnimationState.track = nil
    end
    if not featureSettings.rolarEnabled then
        return
    end
    local B = getHumanoid()
    local u = B and B:FindFirstChildOfClass("Animator")
    local k = findRollAnimation()
    if not u or not k then
        return
    end
    local T, W = pcall(function()
        return u:LoadAnimation(k)
    end)
    if not T or not W then
        return
    end
    rollAnimationState.track = W
    W.Priority = Enum.AnimationPriority.Action4
    W.Looped = true
    W:Play(0.15)
end
local function findVehicleController(B)
    if type(getconnections) ~= "function" then
        return nil
    end
    if type(debug) ~= "table" or type(debug.getupvalue) ~= "function" then
        return nil
    end
    for u, k in ipairs(getconnections(RunService.Stepped)) do
        local T = k.Function
        if type(T) == "function" then
            local u, k
            for W = 1, 8, 1 do
                local l, I = pcall(debug.getupvalue, T, W)
                if not l then
                    break
                end
                if I == B then
                    u = true
                end
                if type(I) == "table" and (I.RemapMultiplier ~= nil and type(I.BuildHPCache) == "function") then
                    k = I
                end
            end
            if u and k then
                return k
            end
        end
    end
    return nil
end
local function getCurrentVehicle()
    local B = getHumanoid()
    local u = B and B.SeatPart
    if not u or u.Name ~= "DriveSeat" then
        return nil
    end
    local k = WorkspaceService:FindFirstChild("CarrosSpawnados")
    if not k then
        return nil
    end
    local T = u
    while T and T.Parent ~= k do
        T = T.Parent
    end
    return T
end
-- Vehicle noclip and vehicle-flight motion controller.
do
    local vehicleMotionState = {
        carro = nil,
        originais = {},
        raiz = nil,
        assento = nil,
        relativoAssento = nil,
        altura = nil,
        piso = nil,
        raycast = nil,
        velocidade = 0,
        frente = nil,
        personagemAtivo = false,
        altitude = nil,
        vFlyAtivo = false,
        vFlyReduzidoAte = 0,
    }
    local function getVehicleRootAndSeat(B)
        local u = B:FindFirstChild("DriveSeat", true) or B:FindFirstChildWhichIsA("VehicleSeat", true)
        return (u and u.AssemblyRootPart) or B.PrimaryPart or u, u
    end
    local function resetVehicleMotion()
        for B, u in pairs(vehicleMotionState.originais) do
            if B.Parent then
                pcall(function()
                    B.CanCollide = u
                end)
            end
        end
        table.clear(vehicleMotionState.originais)
        if vehicleMotionState.personagemAtivo and not featureSettings.noclipEnabled then
            pcall(setCharacterNoclip, false)
        end
        vehicleMotionState.carro = nil
        vehicleMotionState.raiz = nil
        vehicleMotionState.assento = nil
        vehicleMotionState.relativoAssento = nil
        vehicleMotionState.altura = nil
        vehicleMotionState.piso = nil
        vehicleMotionState.raycast = nil
        vehicleMotionState.velocidade = 0
        vehicleMotionState.frente = nil
        vehicleMotionState.personagemAtivo = false
        vehicleMotionState.altitude = nil
        vehicleMotionState.vFlyAtivo = false
        vehicleMotionState.vFlyReduzidoAte = 0
    end
    local function initializeVehicleMotion(u)
        local k, W = getVehicleRootAndSeat(u)
        if not k or not W then
            return
        end
        local Y = RaycastParams.new()
        Y.FilterType = Enum.RaycastFilterType.Exclude
        Y.FilterDescendantsInstances = {
            u,
            localPlayer.Character,
        }
        local q = WorkspaceService:Raycast(k.Position + Vector3.new(0, 20, 0), Vector3.new(0, -120, 0), Y)
        vehicleMotionState.raiz = k
        vehicleMotionState.assento = W
        vehicleMotionState.relativoAssento = (k.CFrame - k.Position):ToObjectSpace(W.CFrame - W.Position)
        vehicleMotionState.raycast = Y
        vehicleMotionState.piso = q and q.Position.Y or (k.Position.Y - 3)
        vehicleMotionState.altura = q and math.max(1, k.Position.Y - q.Position.Y) or 3
        local x = Vector3.new(W.CFrame.LookVector.X, 0, W.CFrame.LookVector.Z)
        vehicleMotionState.frente = x.Magnitude > 0 and x.Unit or Vector3.new(0, 0, -1)
        vehicleMotionState.altitude = k.Position.Y
        vehicleMotionState.velocidade = x.Magnitude > 0 and k.AssemblyLinearVelocity:Dot(x.Unit) or 0
    end
    local function approachNumber(B, u, k)
        if B < u then
            return math.min(B + k, u)
        end
        return math.max(B - k, u)
    end
    local function updateVehicleMotion(u)
        local T, W = vehicleMotionState.raiz, vehicleMotionState.assento
        local I, Y, x = vehicleMotionState.raycast, vehicleMotionState.altura, vehicleMotionState.piso
        local o = vehicleMotionState.relativoAssento
        if not T or not T.Parent or not W or not W.Parent or not I or not Y or not x or not o then
            return
        end
        local i = math.min(u or 0.016666666666667, 0.1)
        local E = featureSettings.vFlyEnabled
        local L = false
        if E and not vehicleMotionState.vFlyAtivo then
            vehicleMotionState.altitude = T.Position.Y
            vehicleMotionState.vFlyReduzidoAte = os.clock() + 0.6
            vehicleMotionState.velocidade = math.clamp(vehicleMotionState.velocidade, -24, 36)
        elseif not E and vehicleMotionState.vFlyAtivo then
            local u = WorkspaceService:Raycast(T.Position + Vector3.new(0, 20, 0), Vector3.new(0, -520, 0), I)
            if u then
                x = u.Position.Y
                vehicleMotionState.piso = x
            end
        end
        vehicleMotionState.vFlyAtivo = E
        local J = 0
        if E then
            if i >= 0.05 or T.Anchored or T.AssemblyLinearVelocity.Magnitude < 12 then
                vehicleMotionState.vFlyReduzidoAte = os.clock() + 0.6
            end
        else
            local u = Vector3.new(T.Position.X, x + 20, T.Position.Z)
            local k = WorkspaceService:Raycast(u, Vector3.new(0, -60, 0), I)
            if k and k.Position.Y - x <= 4 then
                x = k.Position.Y
                vehicleMotionState.piso = x
            end
            J = (x + Y) - T.Position.Y
            L = math.abs(J) > 1
            if L then
                J = math.clamp(J, -20 * i, 20 * i)
            end
        end
        local F = vehicleMotionState.frente or Vector3.new(0, 0, -1)
        local b = 0
        if F.Magnitude > 0.001 then
            F = F.Unit
            local u = W.ThrottleFloat
            local l = W.SteerFloat
            if UserInputService:IsKeyDown(Enum.KeyCode.W) or UserInputService:IsKeyDown(Enum.KeyCode.Up) then
                u = 1
            elseif UserInputService:IsKeyDown(Enum.KeyCode.S) or UserInputService:IsKeyDown(Enum.KeyCode.Down) then
                u = -1
            end
            if UserInputService:IsKeyDown(Enum.KeyCode.A) or UserInputService:IsKeyDown(Enum.KeyCode.Left) then
                l = -1
            elseif UserInputService:IsKeyDown(Enum.KeyCode.D) or UserInputService:IsKeyDown(Enum.KeyCode.Right) then
                l = 1
            end
            local I = isAntiCheatSilenced()
            local Y = E and (not I and os.clock() < vehicleMotionState.vFlyReduzidoAte)
            local x = math.clamp(featureSettings.boostFactor, 1, 2)
            local d = E
                    and (I and featureSettings.vFlyHorizontalSpeed or math.min(
                        featureSettings.vFlyHorizontalSpeed,
                        Y and 36 or 60
                    ))
                or (L and 60 or 80 * x)
            local X = E and d * 0.65 or (L and 40 or 45 * x)
            local s = u > 0.05 and d or (u < -0.05 and -X or 0)
            local D = (math.abs(u) > 0.05 and 75 or 110) * i
            vehicleMotionState.velocidade = approachNumber(vehicleMotionState.velocidade, s, D)
            if math.abs(l) > 0.05 and math.abs(vehicleMotionState.velocidade) > 0.5 then
                local u = vehicleMotionState.velocidade >= 0 and 1 or -1
                local k = ((-l * u) * math.rad(85)) * i
                F = (CFrame.Angles(0, k, 0)):VectorToWorldSpace(F)
            end
            vehicleMotionState.frente = F.Unit
            if E then
                local u = 0
                if UserInputService:IsKeyDown(Enum.KeyCode.Space) or UserInputService:IsKeyDown(Enum.KeyCode.E) then
                    u = 1
                elseif
                    UserInputService:IsKeyDown(Enum.KeyCode.LeftControl) or UserInputService:IsKeyDown(Enum.KeyCode.Q)
                then
                    u = -1
                end
                local W = I and featureSettings.vFlyVerticalSpeed
                    or math.min(featureSettings.vFlyVerticalSpeed, Y and 20 or 30)
                if u ~= 0 then
                    b = u * W
                    vehicleMotionState.altitude = T.Position.Y + b * i
                else
                    local u = (vehicleMotionState.altitude or T.Position.Y) - T.Position.Y
                    b = math.clamp(u * 10, -W, W)
                end
            end
            local H = CFrame.lookAt(Vector3.zero, F.Unit, Vector3.yAxis)
            local R = H * o:Inverse()
            T.CFrame = CFrame.new(T.Position + Vector3.new(0, J, 0)) * R
        elseif math.abs(J) > 0.01 then
            T.CFrame = T.CFrame + Vector3.new(0, J, 0)
        end
        if F.Magnitude > 0.001 then
            T.AssemblyLinearVelocity = F.Unit * vehicleMotionState.velocidade + Vector3.new(0, b, 0)
        else
            T.AssemblyLinearVelocity = Vector3.new(0, b, 0)
        end
        T.AssemblyAngularVelocity = Vector3.zero
    end
    local function applyVehicleNoclip()
        if not featureSettings.noclipCarEnabled and not featureSettings.vFlyEnabled then
            resetVehicleMotion()
            return
        end
        local u = getCurrentVehicle()
        if u ~= vehicleMotionState.carro then
            resetVehicleMotion()
            vehicleMotionState.carro = u
            if u then
                initializeVehicleMotion(u)
            end
        end
        if not u then
            return
        end
        setCharacterNoclip(true)
        vehicleMotionState.personagemAtivo = true
        for u, k in ipairs(u:GetDescendants()) do
            if k:IsA("BasePart") then
                if vehicleMotionState.originais[k] == nil then
                    vehicleMotionState.originais[k] = k.CanCollide
                end
                if k.CanCollide then
                    k.CanCollide = false
                end
            end
        end
    end
    runtimeState.aplicarNoclipCar = applyVehicleNoclip
    runtimeState.restaurarNoclipCar = resetVehicleMotion
    trackConnection(RunService.Stepped:Connect(function()
        if not runtimeState.destroyed then
            pcall(applyVehicleNoclip)
        end
    end))
    trackConnection(RunService.Heartbeat:Connect(function(B)
        if not runtimeState.destroyed and (featureSettings.noclipCarEnabled or featureSettings.vFlyEnabled) then
            pcall(updateVehicleMotion, B)
        end
    end))
end
local function applyVehicleBoost(B)
    local u = findVehicleController(B)
    if not u then
        return false
    end
    u.RemapMultiplier = featureSettings.boostFactor
    pcall(function()
        u:BuildHPCache()
    end)
    return true
end
runtimeState.state = featureSettings
mainEnvironment.__KNWARE_SINTONIA_RUNTIME = runtimeState
local function setCharacterQueryDisabled(B)
    local u = getHumanoid()
    local k = u and u.Parent
    if not k then
        return
    end
    for u, k in ipairs(k:GetChildren()) do
        if k:IsA("BasePart") then
            k.CanQuery = not B
        end
    end
end
local function updateCharacterMovement(B)
    if runtimeState.destroyed then
        return
    end
    local u = getHumanoid()
    if u then
        applyMovementSpoof(u)
    end
    pcall(applyWalkSpoofStep, B)
    if featureSettings.antiRagdollEnabled then
        pcall(updateAntiRagdoll)
    end
    if featureSettings.antiTiroEnabled and u then
        local B = u.Parent
        if B then
            for B, u in ipairs(B:GetChildren()) do
                if u:IsA("BasePart") and u.CanQuery then
                    u.CanQuery = false
                end
            end
        end
    end
    if featureSettings.noclipEnabled then
        pcall(applyCharacterNoclip)
        if not featureSettings.flyEnabled and not featureSettings.farmPartesEnabled then
            pcall(updateNoclipJump)
        end
    end
    if featureSettings.flyEnabled or flyHadInput then
        pcall(updateFly, B)
    end
end
trackConnection(RunService.Heartbeat:Connect(updateCharacterMovement))
trackConnection(RunService.Stepped:Connect(function()
    if not runtimeState.destroyed and featureSettings.noclipEnabled then
        pcall(setCharacterNoclip, true)
    end
end))
task.spawn(function()
    local B
    while not runtimeState.destroyed do
        local u = getCurrentVehicle()
        if u then
            runtimeState.lastDrivenCar = u
        end
        if featureSettings.boostFactor > 1.001 then
            if u and u ~= B then
                if applyVehicleBoost(u) then
                    B = u
                end
            elseif not u then
                B = nil
            end
        else
            B = nil
        end
        task.wait(0.5)
    end
end)
local function initializeCharacter(B)
    task.wait(0.5)
    local u = B:WaitForChild("Humanoid", 5)
    if u then
        applyMovementSpoof(u)
        u.Jumping:Connect(applyJumpSpoof)
    end
    task.spawn(function()
        B:WaitForChild("RagdollClient", 5)
        if runtimeState.destroyed then
            return
        end
        if featureSettings.antiRagdollEnabled then
            pcall(applyAntiRagdoll)
        end
        if featureSettings.rolarEnabled then
            pcall(updateRollAnimation)
        end
    end)
end
trackConnection(localPlayer.CharacterAdded:Connect(initializeCharacter))
if localPlayer.Character then
    initializeCharacter(localPlayer.Character)
end
-- Player relationship and staff classification service.
local playerRelationships = {}
do
    local CollectionService = game:GetService("CollectionService")
    local staffRankThreshold = 254
    local staffColor = Color3.fromRGB(255, 75, 120)
    local friendColor = Color3.fromRGB(60, 210, 255)
    local neutralPlayerColor = Color3.fromRGB(255, 255, 255)
    local staffKeywords = {
        "admin",
        "staff",
        "moder",
        "owner",
        "founder",
        "creator",
        "developer",
        "dono",
        "fundador",
        "gerente",
        "supervis",
        "diretor",
        "gestor",
    }
    local function containsStaffKeyword(B)
        local u = string.lower(tostring(B or ""))
        for B, k in ipairs(staffKeywords) do
            if string.find(u, k, 1, true) then
                return true
            end
        end
        return false
    end
    local function isTruthyMarker(B)
        local u = typeof(B)
        if u == "boolean" then
            return B
        end
        if u == "number" then
            return B > 0
        end
        if u == "string" then
            local u = string.lower(B)
            return u ~= "" and (u ~= "false" and (u ~= "0" and (u ~= "none" and (u ~= "guest" and u ~= "member"))))
        end
        return B ~= nil
    end
    local function isConfiguredStaff(B)
        local u = mainEnvironment.KNWARE_SINTONIA_STAFF_USER_IDS
        if type(u) ~= "table" then
            return false
        end
        return u[B.UserId] == true or u[tostring(B.UserId)] == true or table.find(u, B.UserId) ~= nil
    end
    local function detectStaffPlayer(B)
        if isConfiguredStaff(B) then
            return true, "configured"
        end
        if game.CreatorType == Enum.CreatorType.User and B.UserId == game.CreatorId then
            return true, "creator"
        end
        local k, T = pcall(function()
            return B:GetAttributes()
        end)
        if k then
            for B, u in pairs(T) do
                if containsStaffKeyword(B) and isTruthyMarker(u) then
                    return true, "attribute:" .. B
                end
            end
        end
        local W, l = pcall(CollectionService.GetTags, CollectionService, B)
        if W then
            for B, u in ipairs(l) do
                if containsStaffKeyword(u) then
                    return true, "tag:" .. u
                end
            end
        end
        return false, nil
    end
    local getRelationshipState = LPH_NO_VIRTUALIZE(function(B)
        local u = runtimeState.relationships[B]
        if u then
            return u
        end
        u = {
            ready = false,
            checking = false,
            friend = false,
            admin = false,
            rank = 0,
            role = "",
            source = nil,
        }
        runtimeState.relationships[B] = u
        return u
    end)
    local function refreshRelationship(u)
        if u == localPlayer or u.Parent ~= PlayersService then
            return
        end
        local T = getRelationshipState(u)
        if T.checking then
            return
        end
        T.checking = true
        task.spawn(function()
            local W = false
            local l, Y = pcall(function()
                return localPlayer:IsFriendsWith(u.UserId)
            end)
            if l then
                W = Y == true
            end
            local q, o = detectStaffPlayer(u)
            local i, Z = 0, ""
            if game.CreatorType == Enum.CreatorType.Group and game.CreatorId > 0 then
                local B, T = pcall(function()
                    return u:GetRankInGroup(game.CreatorId)
                end)
                if B and type(T) == "number" then
                    i = T
                end
                local W, l = pcall(function()
                    return u:GetRoleInGroup(game.CreatorId)
                end)
                if W and type(l) == "string" then
                    Z = l
                end
                if i >= staffRankThreshold then
                    q, o = true, "group-rank:" .. tostring(i)
                elseif containsStaffKeyword(Z) then
                    q, o = true, "group-role:" .. Z
                end
            end
            if u.Parent == PlayersService and runtimeState.relationships[u] == T then
                T.friend = W
                T.admin = q
                T.rank = i
                T.role = Z
                T.source = o
                T.ready = true
                T.checking = false
            end
        end)
    end
    local getRelationship = LPH_NO_VIRTUALIZE(function(B)
        local u = getRelationshipState(B)
        if not u.ready and not u.checking then
            refreshRelationship(B)
        end
        return u
    end)
    function playerRelationships.Protected(B)
        local u = getRelationship(B)
        if (featureSettings.friendCheck or featureSettings.adminCheck) and not u.ready then
            return true
        end
        if featureSettings.adminCheck and u.admin then
            return true
        end
        if featureSettings.friendCheck and u.friend then
            return true
        end
        return false
    end
    playerRelationships.Admin = LPH_NO_VIRTUALIZE(function(B)
        local u = getRelationship(B)
        return u.ready and u.admin
    end)
    playerRelationships.Appearance = LPH_NO_VIRTUALIZE(function(B)
        local u = getRelationship(B)
        if u.ready and (featureSettings.adminCheck and u.admin) then
            return staffColor, " [STAFF]"
        end
        if u.ready and (featureSettings.friendCheck and u.friend) then
            return friendColor, " [AMIGO]"
        end
        return neutralPlayerColor, ""
    end)
    local function trackPlayerRelationship(B)
        if B == localPlayer then
            return
        end
        refreshRelationship(B)
        local u = B.AttributeChanged:Connect(function(u)
            if containsStaffKeyword(u) then
                local u = runtimeState.relationships[B]
                if u then
                    u.ready = false
                end
                refreshRelationship(B)
            end
        end)
        runtimeState.relationshipConnections[B] = u
        trackConnection(u)
    end
    for B, u in ipairs(PlayersService:GetPlayers()) do
        trackPlayerRelationship(u)
    end
    trackConnection(PlayersService.PlayerAdded:Connect(trackPlayerRelationship))
    trackConnection(PlayersService.PlayerRemoving:Connect(function(B)
        runtimeState.relationships[B] = nil
        runtimeState.relationshipConnections[B] = nil
    end))
end
local drawingCapabilities = {
    medido = false,
    drawing = false,
    texto = false,
}
function drawingCapabilities.set(B, u, k)
    local T = pcall(function()
        B[u] = k
    end)
    return T
end
function drawingCapabilities.medir()
    if drawingCapabilities.medido then
        return drawingCapabilities
    end
    if Drawing == nil or type(Drawing.new) ~= "function" then
        return drawingCapabilities
    end
    local B, u = pcall(Drawing.new, "Square")
    if not B then
        return drawingCapabilities
    end
    drawingCapabilities.drawing = true
    drawingCapabilities.filled = drawingCapabilities.set(u, "Filled", false)
    drawingCapabilities.thickness = drawingCapabilities.set(u, "Thickness", 1)
    pcall(function()
        u:Remove()
    end)
    local k, T = pcall(Drawing.new, "Text")
    if k then
        drawingCapabilities.texto = true
        drawingCapabilities.center = drawingCapabilities.set(T, "Center", true)
        drawingCapabilities.outline = drawingCapabilities.set(T, "Outline", true)
        drawingCapabilities.outlineColor = drawingCapabilities.set(T, "OutlineColor", Color3.new())
        drawingCapabilities.font = drawingCapabilities.set(T, "Font", 2)
        pcall(function()
            T.Text = "MMM"
        end)
        local B, u = pcall(function()
            return T.TextBounds
        end)
        drawingCapabilities.textBounds = B and (typeof(u) == "Vector2" and u.X > 0)
        pcall(function()
            T:Remove()
        end)
    end
    local W, l = pcall(Drawing.new, "Triangle")
    drawingCapabilities.triangle = W == true
    if W then
        pcall(function()
            l:Remove()
        end)
    end
    drawingCapabilities.medido = true
    return drawingCapabilities
end
function drawingCapabilities.ok()
    return (drawingCapabilities.medir()).drawing
end
local function createDrawingObject(B)
    if not drawingCapabilities.ok() then
        return nil
    end
    if B == "Triangle" and not drawingCapabilities.triangle then
        return nil
    end
    local u, k = pcall(function()
        return Drawing.new(B)
    end)
    if not u then
        return nil
    end
    drawingCapabilities.set(k, "Visible", false)
    drawingCapabilities.set(k, "Color", Color3.fromRGB(255, 255, 255))
    drawingCapabilities.set(k, "Transparency", 1)
    drawingCapabilities.set(k, "Thickness", 1)
    return k
end
local function createDrawingText(B)
    if not drawingCapabilities.ok() or not drawingCapabilities.texto then
        return nil
    end
    local u, k = pcall(function()
        return Drawing.new("Text")
    end)
    if not u then
        return nil
    end
    drawingCapabilities.set(k, "Visible", false)
    drawingCapabilities.set(k, "Color", B or Color3.fromRGB(255, 255, 255))
    drawingCapabilities.set(k, "Transparency", 1)
    drawingCapabilities.set(k, "Size", 13)
    if drawingCapabilities.center then
        drawingCapabilities.set(k, "Center", true)
    end
    if drawingCapabilities.outline then
        drawingCapabilities.set(k, "Outline", true)
    end
    if drawingCapabilities.outlineColor then
        drawingCapabilities.set(k, "OutlineColor", Color3.fromRGB(0, 0, 0))
    end
    if drawingCapabilities.font then
        drawingCapabilities.set(k, "Font", 2)
    end
    return k
end
function drawingCapabilities.largura(B)
    if not B then
        return 0
    end
    if drawingCapabilities.textBounds then
        local u, k = pcall(function()
            return B.TextBounds
        end)
        if u and k then
            return k.X
        end
    end
    local u, k = pcall(function()
        return B.Text
    end)
    if not u or type(k) ~= "string" then
        return 0
    end
    local T, W = pcall(function()
        return B.Size
    end)
    return (#k * (T and W or 13)) * 0.52
end
local r15SkeletonPairs = {
    {
        "Head",
        "UpperTorso",
    },
    {
        "UpperTorso",
        "LowerTorso",
    },
    {
        "UpperTorso",
        "LeftUpperArm",
    },
    {
        "LeftUpperArm",
        "LeftLowerArm",
    },
    {
        "LeftLowerArm",
        "LeftHand",
    },
    {
        "UpperTorso",
        "RightUpperArm",
    },
    {
        "RightUpperArm",
        "RightLowerArm",
    },
    {
        "RightLowerArm",
        "RightHand",
    },
    {
        "LowerTorso",
        "LeftUpperLeg",
    },
    {
        "LeftUpperLeg",
        "LeftLowerLeg",
    },
    {
        "LeftLowerLeg",
        "LeftFoot",
    },
    {
        "LowerTorso",
        "RightUpperLeg",
    },
    {
        "RightUpperLeg",
        "RightLowerLeg",
    },
    {
        "RightLowerLeg",
        "RightFoot",
    },
}
local r6SkeletonPairs = {
    {
        "Head",
        "Torso",
    },
    {
        "Torso",
        "Left Arm",
    },
    {
        "Torso",
        "Right Arm",
    },
    {
        "Torso",
        "Left Leg",
    },
    {
        "Torso",
        "Right Leg",
    },
}
local r15SkeletonLineCount = #r15SkeletonPairs
local box3dEdges = {
    {
        1,
        2,
    },
    {
        1,
        3,
    },
    {
        1,
        5,
    },
    {
        2,
        4,
    },
    {
        2,
        6,
    },
    {
        3,
        4,
    },
    {
        3,
        7,
    },
    {
        4,
        8,
    },
    {
        5,
        6,
    },
    {
        5,
        7,
    },
    {
        6,
        8,
    },
    {
        7,
        8,
    },
}
local nearEspDistance = 300
local nearEspUpdateInterval = 0.016666666666667
local farEspUpdateInterval = 0.033333333333333
local hiddenEspUpdateInterval = 0.05
local maxEspBundlesPerFrame = 4
local espTextGap = 5
local advanceUpdateDeadline = LPH_NO_VIRTUALIZE(function(B, u, k)
    if B <= 0 or u - B > k then
        return u + k
    end
    return B + k
end)
local createEspBundle = LPH_NO_VIRTUALIZE(function(B)
    local u = {
        player = B,
        box = createDrawingObject("Square"),
        name = createDrawingText(Color3.fromRGB(255, 255, 255)),
        distance = createDrawingText(Color3.fromRGB(190, 200, 225)),
        item = createDrawingText(Color3.fromRGB(190, 200, 225)),
        team = createDrawingText(Color3.fromRGB(255, 255, 255)),
        lines = {},
        tracer = nil,
        arrow = nil,
        box3dLines = {},
        highlight = nil,
        character = nil,
        humanoid = nil,
        root = nil,
        bodyParts = {},
        skeletonPairs = {},
        relationshipColor = nil,
        relationshipLabel = "",
        nextVisualUpdate = 0,
        nextSkeletonUpdate = 0,
    }
    if u.box and drawingCapabilities.filled then
        u.box.Filled = false
    end
    for B = 1, r15SkeletonLineCount, 1 do
        table.insert(u.lines, createDrawingObject("Line"))
    end
    featureSettings.bundles[B] = u
    return u
end)
local hideEspDrawings = LPH_NO_VIRTUALIZE(function(B)
    if B.box then
        B.box.Visible = false
    end
    if B.name then
        B.name.Visible = false
    end
    if B.distance then
        B.distance.Visible = false
    end
    if B.item then
        B.item.Visible = false
    end
    if B.team then
        B.team.Visible = false
    end
    if B.tracer then
        B.tracer.Visible = false
    end
    if B.arrow then
        B.arrow.Visible = false
    end
    for B, u in ipairs(B.lines) do
        u.Visible = false
    end
    for B, u in ipairs(B.box3dLines) do
        u.Visible = false
    end
end)
local hideEspBundle = LPH_NO_VIRTUALIZE(function(B)
    hideEspDrawings(B)
    if B.highlight then
        B.highlight.Enabled = false
    end
end)
local destroyEspBundle = LPH_NO_VIRTUALIZE(function(B)
    if not B then
        return
    end
    local function u(B)
        if B then
            pcall(function()
                B:Remove()
            end)
        end
    end
    u(B.box)
    u(B.name)
    u(B.distance)
    u(B.item)
    u(B.team)
    u(B.tracer)
    u(B.arrow)
    for B, k in ipairs(B.lines) do
        u(k)
    end
    for B, k in ipairs(B.box3dLines) do
        u(k)
    end
    if B.highlight then
        pcall(function()
            B.highlight:Destroy()
        end)
        B.highlight = nil
    end
end)
local hideAllEspBundles = LPH_NO_VIRTUALIZE(function()
    for B, u in pairs(featureSettings.bundles) do
        hideEspBundle(u)
    end
end)
local worldToViewport = LPH_NO_VIRTUALIZE(function(B, u)
    local k, T = B:WorldToViewportPoint(u)
    return Vector2.new(k.X, k.Y), T, k.Z
end)
local ensureTracer = LPH_NO_VIRTUALIZE(function(B)
    if B.tracer then
        return B.tracer
    end
    B.tracer = createDrawingObject("Line")
    if B.tracer then
        if drawingCapabilities.thickness then
            B.tracer.Thickness = 1.5
        end
        B.tracer.Color = B.relationshipColor or Color3.fromRGB(255, 255, 255)
    end
    return B.tracer
end)
local ensureOffscreenArrow = LPH_NO_VIRTUALIZE(function(B)
    if B.arrow then
        return B.arrow
    end
    B.arrow = createDrawingObject("Triangle")
    if B.arrow then
        if drawingCapabilities.filled then
            B.arrow.Filled = true
        end
        if drawingCapabilities.thickness then
            B.arrow.Thickness = 1
        end
        B.arrow.Color = B.relationshipColor or Color3.fromRGB(255, 255, 255)
    end
    return B.arrow
end)
local ensureBox3dLines = LPH_NO_VIRTUALIZE(function(B)
    if #B.box3dLines > 0 then
        return B.box3dLines
    end
    for u = 1, #box3dEdges, 1 do
        local k = createDrawingObject("Line")
        if k then
            if drawingCapabilities.thickness then
                k.Thickness = 1
            end
            k.Color = B.relationshipColor or Color3.fromRGB(255, 255, 255)
            table.insert(B.box3dLines, k)
        end
    end
    return B.box3dLines
end)
local ensureHighlight = LPH_NO_VIRTUALIZE(function(B, u)
    local k = B.highlight
    if not k or not k.Parent then
        local u, T = pcall(function()
            local u = Instance.new("Highlight")
            u.Name = "KnwareESP_" .. tostring(B.player.UserId)
            u.DepthMode = Enum.HighlightDepthMode.AlwaysOnTop
            u.FillTransparency = 0.78
            u.OutlineTransparency = 0
            u.Parent = WorkspaceService.CurrentCamera or WorkspaceService
            return u
        end)
        if not u then
            return nil
        end
        k = T
        B.highlight = k
    elseif k.Parent ~= (WorkspaceService.CurrentCamera or WorkspaceService) then
        k.Parent = WorkspaceService.CurrentCamera or WorkspaceService
    end
    local T = B.relationshipColor or Color3.fromRGB(255, 255, 255)
    k.Adornee = u
    k.FillColor = T
    k.OutlineColor = T
    return k
end)
local renderOffscreenArrow = LPH_NO_VIRTUALIZE(function(B, u, k, T)
    local W = ensureOffscreenArrow(B)
    if not W then
        return
    end
    local l = u.ViewportSize
    local I = Vector2.new(l.X * 0.5, l.Y * 0.5)
    local Y = k - I
    if T <= 0 then
        Y = -Y
    end
    if Y.Magnitude < 0.001 then
        Y = Vector2.new(0, -1)
    end
    local q = Y.Unit
    local x = math.max(40, math.min(l.X, l.Y) * 0.42)
    local o = I + q * x
    local i = o - q * 14
    local E = Vector2.new(-q.Y, q.X) * 8
    W.PointA = o
    W.PointB = i + E
    W.PointC = i - E
    W.Visible = true
end)
local refreshEspCharacter = LPH_NO_VIRTUALIZE(function(B, u, k, T)
    if B.character == u and (B.humanoid == k and B.root == T) then
        return
    end
    B.character = u
    B.humanoid = k
    B.root = T
    table.clear(B.bodyParts)
    table.clear(B.skeletonPairs)
    for u, k in ipairs(u:GetChildren()) do
        if k:IsA("BasePart") then
            table.insert(B.bodyParts, k)
        end
    end
    local W = (k.RigType == Enum.HumanoidRigType.R15) and r15SkeletonPairs or r6SkeletonPairs
    for k, T in ipairs(W) do
        table.insert(B.skeletonPairs, {
            u:FindFirstChild(T[1]),
            u:FindFirstChild(T[2]),
        })
    end
end)
local getPlayerTeamLabel = LPH_NO_VIRTUALIZE(function(B)
    local u = B.Team
    local k = u and u.Name or nil
    if playerRelationships.Admin(B) and k ~= "STAFF" then
        return k and ("STAFF | " .. k) or "STAFF"
    end
    return k
end)
local getPlayerTeamColor = LPH_NO_VIRTUALIZE(function(B)
    local u = B.Team
    local k, T = pcall(function()
        return u.TeamColor.Color
    end)
    return (k and T) or Color3.fromRGB(255, 255, 255)
end)
local updateEspRelationshipStyle = LPH_NO_VIRTUALIZE(function(B, u)
    local k, T = playerRelationships.Appearance(B)
    u.relationshipLabel = T
    if u.relationshipColor == k then
        return
    end
    u.relationshipColor = k
    if u.box then
        u.box.Color = k
    end
    if u.name then
        u.name.Color = k
    end
    if u.distance then
        u.distance.Color = k
    end
    if u.item then
        u.item.Color = k
    end
    if u.tracer then
        u.tracer.Color = k
    end
    if u.arrow then
        u.arrow.Color = k
    end
    if u.highlight then
        u.highlight.FillColor = k
        u.highlight.OutlineColor = k
    end
    for B, u in ipairs(u.lines) do
        if u then
            u.Color = k
        end
    end
    for B, u in ipairs(u.box3dLines) do
        if u then
            u.Color = k
        end
    end
end)
local renderPlayerEsp = LPH_NO_VIRTUALIZE(function(B, u, k, T, W)
    local l = B.Character
    local I = l and l:FindFirstChildOfClass("Humanoid")
    local Y = l and l:FindFirstChild("HumanoidRootPart")
    if not l or not I or I.Health <= 0 or not Y then
        u.character = nil
        u.nextVisualUpdate = W + hiddenEspUpdateInterval
        hideEspBundle(u)
        return
    end
    refreshEspCharacter(u, l, I, Y)
    local q = (Y.Position - T).Magnitude
    if q > featureSettings.maxDistance then
        u.nextVisualUpdate = W + hiddenEspUpdateInterval
        hideEspBundle(u)
        return
    end
    updateEspRelationshipStyle(B, u)
    if featureSettings.highlight then
        local B = ensureHighlight(u, l)
        if B then
            B.Enabled = true
        end
    elseif u.highlight then
        u.highlight.Enabled = false
    end
    local x, o, i = worldToViewport(k, Y.Position)
    if not o or i <= 0 then
        u.nextVisualUpdate = W + hiddenEspUpdateInterval
        hideEspDrawings(u)
        if featureSettings.arrow then
            renderOffscreenArrow(u, k, x, i)
        end
        return
    end
    local E = q <= nearEspDistance
    local L = E and nearEspUpdateInterval or farEspUpdateInterval
    u.nextVisualUpdate = advanceUpdateDeadline(u.nextVisualUpdate, W, L)
    if u.arrow then
        u.arrow.Visible = false
    end
    local J, F, b, d = math.huge, math.huge, -math.huge, -math.huge
    local X = false
    local s, D = {}, {}
    if
        featureSettings.box
        or featureSettings.name
        or featureSettings.distance
        or featureSettings.box3d
        or featureSettings.teamTag
    then
        local B, T
        for u, k in ipairs(u.bodyParts) do
            if k.Parent == l then
                local u = k.Size * 0.5
                local W, l = k.Position - u, k.Position + u
                if B then
                    B = Vector3.new(math.min(B.X, W.X), math.min(B.Y, W.Y), math.min(B.Z, W.Z))
                    T = Vector3.new(math.max(T.X, l.X), math.max(T.Y, l.Y), math.max(T.Z, l.Z))
                else
                    B, T = W, l
                end
            end
        end
        if B then
            local u = 0
            for W = 0, 1, 1 do
                for l = 0, 1, 1 do
                    for I = 0, 1, 1 do
                        u = u + 1
                        local Y = Vector3.new(W == 0 and B.X or T.X, l == 0 and B.Y or T.Y, I == 0 and B.Z or T.Z)
                        local q, x, o = worldToViewport(k, Y)
                        s[u] = q
                        D[u] = o
                        if o > 0.5 then
                            X = true
                            J = math.min(J, q.X)
                            F = math.min(F, q.Y)
                            b = math.max(b, q.X)
                            d = math.max(d, q.Y)
                        end
                    end
                end
            end
        end
        if X then
            local B = k.ViewportSize
            J = math.clamp(J, -32, B.X + 32)
            b = math.clamp(b, -32, B.X + 32)
            F = math.clamp(F, -32, B.Y + 32)
            d = math.clamp(d, -32, B.Y + 32)
            if b - J < 1 or d - F < 1 then
                X = false
            end
        end
    end
    if featureSettings.box and u.box then
        if X then
            u.box.Position = Vector2.new(J, F)
            u.box.Size = Vector2.new(b - J, d - F)
            u.box.Visible = true
        else
            u.box.Visible = false
        end
    elseif u.box then
        u.box.Visible = false
    end
    if featureSettings.name and u.name then
        if X then
            u.name.Text = B.Name .. u.relationshipLabel
            u.name.Position = Vector2.new((J + b) / 2, F - 18)
            u.name.Visible = true
        else
            u.name.Visible = false
        end
    elseif u.name then
        u.name.Visible = false
    end
    if featureSettings.teamTag and u.team then
        local k = X and getPlayerTeamLabel(B) or nil
        if k then
            u.team.Text = k
            u.team.Color = getPlayerTeamColor(B)
            u.team.Position = Vector2.new(
                (J + b) / 2,
                (featureSettings.name and (u.name and u.name.Visible)) and (F - 32) or (F - 18)
            )
            u.team.Visible = true
        else
            u.team.Visible = false
        end
    elseif u.team then
        u.team.Visible = false
    end
    if featureSettings.distance and u.distance then
        if X then
            u.distance.Text = string.format("%dm", math.floor(q))
            if drawingCapabilities.center then
                u.distance.Center = true
            end
            u.distance.Position = Vector2.new((J + b) / 2, d + 3)
            u.distance.Visible = true
        else
            u.distance.Visible = false
        end
    elseif u.distance then
        u.distance.Visible = false
    end
    if featureSettings.heldItem and u.item then
        local k = u.character or B.Character
        local T = X and (k and k:FindFirstChildOfClass("Tool"))
        if T then
            u.item.Text = T.Name
            local B = (J + b) / 2
            local k = featureSettings.distance and (u.distance and u.distance.Visible)
            if k then
                local k = drawingCapabilities.largura(u.distance)
                local T = drawingCapabilities.largura(u.item)
                local W = B - ((k + espTextGap) + T) / 2
                if drawingCapabilities.center then
                    u.distance.Center = false
                end
                u.distance.Position = Vector2.new(W, d + 3)
                if drawingCapabilities.center then
                    u.item.Center = false
                end
                u.item.Position = Vector2.new((W + k) + espTextGap, d + 3)
            else
                if drawingCapabilities.center then
                    u.item.Center = true
                end
                u.item.Position = Vector2.new(B, d + 3)
            end
            u.item.Visible = true
        else
            u.item.Visible = false
        end
    elseif u.item then
        u.item.Visible = false
    end
    if featureSettings.tracer then
        local B = ensureTracer(u)
        if B then
            local u = k.ViewportSize
            B.From = Vector2.new(u.X * 0.5, u.Y - 4)
            B.To = X and Vector2.new((J + b) * 0.5, (F + d) * 0.5) or x
            B.Visible = true
        end
    elseif u.tracer then
        u.tracer.Visible = false
    end
    if featureSettings.box3d then
        local B = ensureBox3dLines(u)
        for u, k in ipairs(box3dEdges) do
            local T = B[u]
            local W, l = s[k[1]], s[k[2]]
            local I, Y = D[k[1]], D[k[2]]
            if T and (W and (l and (I and (Y and (I > 0.5 and Y > 0.5))))) then
                T.From = W
                T.To = l
                T.Visible = true
            elseif T then
                T.Visible = false
            end
        end
    else
        for B, u in ipairs(u.box3dLines) do
            u.Visible = false
        end
    end
    if not featureSettings.skeleton then
        for B, u in ipairs(u.lines) do
            u.Visible = false
        end
    elseif W >= u.nextSkeletonUpdate then
        local B = E and nearEspUpdateInterval or farEspUpdateInterval
        u.nextSkeletonUpdate = advanceUpdateDeadline(u.nextSkeletonUpdate, W, B)
        for B, T in ipairs(u.lines) do
            local W = u.skeletonPairs[B]
            if W and T then
                local B, u = W[1], W[2]
                if B and (u and (B.Parent == l and u.Parent == l)) then
                    local W, l, I = worldToViewport(k, B.Position)
                    local Y, q, x = worldToViewport(k, u.Position)
                    if l and (q and (I > 0.5 and x > 0.5)) then
                        T.From = W
                        T.To = Y
                        T.Visible = true
                    else
                        T.Visible = false
                    end
                else
                    T.Visible = false
                end
            else
                T.Visible = false
            end
        end
    end
end)
trackConnection(PlayersService.PlayerRemoving:Connect(function(B)
    local u = featureSettings.bundles[B]
    if u then
        destroyEspBundle(u)
        featureSettings.bundles[B] = nil
    end
end))
local espWasEnabled = false
trackConnection(RunService.RenderStepped:Connect(LPH_NO_VIRTUALIZE(function()
    if not featureSettings.espEnabled then
        if espWasEnabled then
            hideAllEspBundles()
        end
        espWasEnabled = false
        return
    end
    espWasEnabled = true
    local u = WorkspaceService.CurrentCamera
    if not u then
        hideAllEspBundles()
        return
    end
    local k = localPlayer.Character
    local T = k and k:FindFirstChild("HumanoidRootPart")
    local W = T and T.Position or u.CFrame.Position
    local Y = PlayersService:GetPlayers()
    local q = os.clock()
    local x = maxEspBundlesPerFrame
    for B, k in ipairs(Y) do
        if k ~= localPlayer then
            local B = featureSettings.bundles[k]
            if not B and x > 0 then
                local u, T = pcall(createEspBundle, k)
                if u then
                    B = T
                elseif not runtimeState.espErro then
                    runtimeState.espErro = tostring(T)
                end
                x = x - 1
            end
            if B and q >= B.nextVisualUpdate then
                local T, l = pcall(renderPlayerEsp, k, B, u, W, q)
                if not T and not runtimeState.espErro then
                    runtimeState.espErro = tostring(l)
                end
            end
        end
    end
end)))
-- Silent-aim targeting and raycast hook.
local weaponRaycastController
do
    local B, u = pcall(function()
        local B = game:GetService("ReplicatedStorage")
        return require(B.OrpheusShared.GunSys.Framework.WeaponRaycastController)
    end)
    weaponRaycastController = B and u or nil
end
local function hasLineOfSight(B, u, k)
    local T = B.CFrame.Position
    local W = RaycastParams.new()
    W.FilterType = Enum.RaycastFilterType.Exclude
    W.FilterDescendantsInstances = {
        localPlayer.Character,
        k,
        B,
    }
    local Y = WorkspaceService:Raycast(T, u.Position - T, W)
    return Y == nil
end
local silentAimBodyParts = {
    Cabeca = {
        "Head",
    },
    Tronco = {
        "Torso",
        "UpperTorso",
        "LowerTorso",
        "HumanoidRootPart",
    },
    Bracos = {
        "Right Arm",
        "Left Arm",
        "RightUpperArm",
        "LeftUpperArm",
        "RightLowerArm",
        "LeftLowerArm",
    },
    Pernas = {
        "Right Leg",
        "Left Leg",
        "RightUpperLeg",
        "LeftUpperLeg",
        "RightLowerLeg",
        "LeftLowerLeg",
    },
}
local function getSilentAimPart(B)
    local u = silentAimBodyParts[featureSettings.silentAimBodyPart] or silentAimBodyParts.Cabeca
    for u, k in ipairs(u) do
        local T = B:FindFirstChild(k)
        if T and T:IsA("BasePart") then
            return T
        end
    end
    return B:FindFirstChild("Head")
end
local function findSilentAimTarget(u)
    local k = u.CFrame.Position
    local T = u.ViewportSize / 2
    local W, l = nil, math.huge
    for B, Y in ipairs(PlayersService:GetPlayers()) do
        if Y ~= localPlayer and not playerRelationships.Protected(Y) then
            local B = Y.Character
            local I = B and getSilentAimPart(B)
            local q = B and B:FindFirstChildOfClass("Humanoid")
            if I and (q and (q.Health > 0 and (I.Position - k).Magnitude <= featureSettings.silentAimDistance)) then
                local k, Y = u:WorldToViewportPoint(I.Position)
                if Y and k.Z > 0 then
                    local Y = (Vector2.new(k.X, k.Y) - T).Magnitude
                    if Y <= featureSettings.silentAimFov and Y < l then
                        if not featureSettings.silentAimVisibleOnly or hasLineOfSight(u, I, B) then
                            W, l = I, Y
                        end
                    end
                end
            end
        end
    end
    return W
end
local function installSilentAimHook()
    if not weaponRaycastController or weaponRaycastController.__knwareOriginal then
        return
    end
    local B = weaponRaycastController.PerformRaycast
    weaponRaycastController.__knwareOriginal = B
    weaponRaycastController.PerformRaycast = function(u, k, T)
        if
            featureSettings.silentAimEnabled
            and (featureSettings.silentAimHitChance >= 100 or math.random(100) <= featureSettings.silentAimHitChance)
        then
            local B = u.Camera or WorkspaceService.CurrentCamera
            if B then
                local u = findSilentAimTarget(B)
                if u then
                    local k = B.CFrame.Position
                    local T = (u.Position - k).Unit
                    return u, u.Position, -T, T
                end
            end
        end
        return B(u, k, T)
    end
end
local function restoreSilentAimHook()
    if weaponRaycastController and weaponRaycastController.__knwareOriginal then
        weaponRaycastController.PerformRaycast = weaponRaycastController.__knwareOriginal
        weaponRaycastController.__knwareOriginal = nil
    end
end
installSilentAimHook()
local silentAimFovCircle
if drawingCapabilities.ok() then
    local B, u = pcall(function()
        return Drawing.new("Circle")
    end)
    if B then
        silentAimFovCircle = u
        drawingCapabilities.set(silentAimFovCircle, "Visible", false)
        drawingCapabilities.set(silentAimFovCircle, "Color", Color3.fromRGB(255, 255, 255))
        drawingCapabilities.set(silentAimFovCircle, "Thickness", 2)
        drawingCapabilities.set(silentAimFovCircle, "Transparency", 1)
        drawingCapabilities.set(silentAimFovCircle, "Filled", false)
        drawingCapabilities.set(silentAimFovCircle, "NumSides", 64)
    end
end
trackConnection(RunService.RenderStepped:Connect(function()
    if not silentAimFovCircle then
        return
    end
    local B = WorkspaceService.CurrentCamera
    if not featureSettings.silentAimFovVisible or not B then
        silentAimFovCircle.Visible = false
        return
    end
    silentAimFovCircle.Position = B.ViewportSize / 2
    silentAimFovCircle.Radius = featureSettings.silentAimFov
    silentAimFovCircle.Visible = true
end))
local function getLockpickElements()
    local B = localPlayer:FindFirstChild("PlayerGui")
    local u = B and B:FindFirstChild("Lockpick")
    local k = u and u:FindFirstChild("Lockpick")
    if not k or not k.Visible then
        return nil
    end
    local T = k:FindFirstChild("Bloco")
    local W = k:FindFirstChild("Objetivo")
    local l = k:FindFirstChild("Btn")
    if T and (W and (l and (T.Visible and W.Visible))) then
        return T, W, l
    end
    return nil
end
local function isLockpickAligned(B, u)
    local k, T = B.AbsolutePosition, B.AbsoluteSize
    local W, l = u.AbsolutePosition, u.AbsoluteSize
    if k.X + T.X < W.X or k.X > W.X + l.X then
        return false
    end
    local I = k.Y + T.Y / 2
    local Y = l.Y * 0.25
    return I >= W.Y + Y and I <= (W.Y + l.Y) - Y
end
local function activateGuiButton(B)
    if type(getconnections) == "function" then
        local u, k = pcall(getconnections, B.MouseButton1Click)
        if u and (type(k) == "table" and #k > 0) then
            for B, u in ipairs(k) do
                local k = u.Function
                if type(k) == "function" then
                    task.spawn(k)
                end
            end
            return
        end
    end
    if type(firesignal) == "function" then
        pcall(firesignal, B.MouseButton1Click)
    end
end
local lockpickCooldown = 1.15
local lastLockpickActivation = 0
trackConnection(RunService.RenderStepped:Connect(function()
    if not featureSettings.perfectLockpick then
        return
    end
    local B, u, k = getLockpickElements()
    if not B then
        return
    end
    if os.clock() - lastLockpickActivation < lockpickCooldown then
        return
    end
    if not isLockpickAligned(B, u) then
        u.Position = UDim2.new(0.5, 0, B.Position.Y.Scale, B.Position.Y.Offset)
        return
    end
    lastLockpickActivation = os.clock()
    activateGuiButton(k)
end))
-- Dropped-item teleport, proximity interaction, and weapon-part farming.
local itemTeleportRequestedVelocity = 20
local itemTeleportExitSpeed = 14
local itemTeleportRecoveryDuration = 1.5
local itemTeleportMovementThreshold = movementReuseThreshold
local itemTeleportHeightOffset = 18
local itemTeleportNormalSpeedCap = 65
local itemTeleportDefaultSpeed = itemTeleportNormalSpeedCap
local itemTeleportSliderMaximum = 160
local function getDroppedItemsFolder()
    return WorkspaceService:FindFirstChild("DropadosInv")
end
local function getItemBasePart(B)
    if B:IsA("BasePart") then
        return B
    end
    if B:IsA("Model") then
        return B.PrimaryPart or B:FindFirstChildWhichIsA("BasePart")
    end
    return nil
end
local function listDroppedItems()
    local B = {}
    local u = getRootPart()
    local k = getDroppedItemsFolder()
    if not u or not k then
        return B
    end
    for k, T in ipairs(k:GetChildren()) do
        local W = getItemBasePart(T)
        if W then
            table.insert(B, {
                item = T,
                parte = W,
                nome = T.Name,
                distancia = (u.Position - W.Position).Magnitude,
            })
        end
    end
    table.sort(B, function(B, u)
        return B.distancia < u.distancia
    end)
    return B
end
local function triggerProximityPrompt(B)
    local u = B:FindFirstChildWhichIsA("ProximityPrompt", true)
    if not u or not u.Enabled then
        return false
    end
    if type(fireproximityprompt) == "function" then
        return (pcall(fireproximityprompt, u, 0))
    end
    return (
        pcall(function()
            u:InputHoldBegin()
            task.wait(math.max(0.15, u.HoldDuration + 0.1))
            u:InputHoldEnd()
        end)
    )
end
local function teleportToItem(B, k)
    runtimeState.itemTeleportRunId = runtimeState.itemTeleportRunId + 1
    local T = runtimeState.itemTeleportRunId
    local W = isAntiCheatSilenced()
    local l = featureSettings.noclipEnabled
    if not l then
        featureSettings.noclipEnabled = true
        pcall(applyCharacterNoclip)
    end
    local I = false
    local Y = Vector3.zero
    if W then
        local u = os.clock() + 8
        while os.clock() < u do
            if runtimeState.destroyed or T ~= runtimeState.itemTeleportRunId then
                break
            end
            if not B.item.Parent or not B.parte.Parent then
                I = true
                break
            end
            local u = getRootPart()
            if not u then
                break
            end
            local k = B.parte.Position + Vector3.new(0, 3, 0)
            if (k - u.Position).Magnitude <= 4 then
                I = true
                break
            end
            u.CFrame = CFrame.new(k) * u.CFrame.Rotation
            u.AssemblyLinearVelocity = Vector3.zero
            task.wait(0.1)
        end
    end
    local q = getRootPart()
    local x = q and (B.parte.Position - q.Position).Magnitude or 0
    local o = B.parte.Position + Vector3.new(0, 3, 0)
    local i = math.max(q and q.Position.Y or o.Y, o.Y) + itemTeleportHeightOffset
    local E = math.min(featureSettings.itemTeleportSpeed, safeMovementSpeedCap)
    local J = (os.clock() + 20) + (x / math.max(E, 1)) * 2
    while not W and os.clock() < J do
        local k = RunService.RenderStepped:Wait()
        if runtimeState.destroyed or T ~= runtimeState.itemTeleportRunId then
            break
        end
        local W, l = B.item, B.parte
        if not W.Parent or not l.Parent then
            I = true
            break
        end
        local q = getRootPart()
        if not q then
            break
        end
        o = l.Position + Vector3.new(0, 3, 0)
        local x = Vector3.new(o.X - q.Position.X, 0, o.Z - q.Position.Z)
        local E = o
        if x.Magnitude > 4 then
            E = q.Position.Y < i - 1 and Vector3.new(q.Position.X, i, q.Position.Z) or Vector3.new(o.X, i, o.Z)
        end
        local J = E - q.Position
        local F = J.Magnitude
        if (o - q.Position).Magnitude <= 4 then
            I = true
            break
        end
        local b, d, X, s, D = calculateMovementBudget(q, k, itemTeleportRequestedVelocity, true)
        local H = math.min(featureSettings.itemTeleportSpeed, D and safeMovementSpeedCap or itemTeleportNormalSpeedCap)
        local R = math.min(F, math.max(0, H - d) * k, X)
        local m = J.Unit
        Y = m
        if R > 0 then
            q.CFrame = q.CFrame + m * R
        end
        q.AssemblyLinearVelocity = m * d
        local w = getHumanoid()
        if w then
            pcall(w.ChangeState, w, Enum.HumanoidStateType.Freefall)
        end
    end
    local F = getRootPart()
    local b = getHumanoid()
    local d = Vector3.new(Y.X, 0, Y.Z)
    if not W and (F and b) then
        if d.Magnitude <= 0 then
            local B = F.CFrame.LookVector
            d = Vector3.new(B.X, 0, B.Z)
        end
        d = d.Magnitude > 0 and d.Unit or Vector3.new(0, 0, -1)
        local B = os.clock() + itemTeleportRecoveryDuration
        while os.clock() < B and (not runtimeState.destroyed and T == runtimeState.itemTeleportRunId) do
            local B = RunService.RenderStepped:Wait()
            local k, T, W, l = calculateMovementBudget(F, B, itemTeleportExitSpeed, true)
            if l <= itemTeleportMovementThreshold then
                break
            end
            local I = math.clamp(T / math.max(b.WalkSpeed, 1), 0, 1)
            pcall(b.Move, b, d * I, false)
            F.AssemblyLinearVelocity = d * T
        end
    end
    if F then
        F.AssemblyLinearVelocity = Vector3.zero
    end
    if I and T == runtimeState.itemTeleportRunId then
        local u = k or function()
            return not B.item.Parent
        end
        for k = 1, 3, 1 do
            if u() then
                break
            end
            triggerProximityPrompt(B.item)
            task.wait(0.35)
        end
    end
    if not l and T == runtimeState.itemTeleportRunId then
        featureSettings.noclipEnabled = false
        pcall(applyCharacterNoclip)
    end
    return I
end
local function findWeaponPartLocation()
    local B = WorkspaceService:FindFirstChild("FarmPartesDeArma")
    B = B and B:FindFirstChild("Locais")
    if not B then
        return nil
    end
    for B, u in ipairs(B:GetChildren()) do
        if u:IsA("BasePart") and u:FindFirstChild("GUI") then
            return u
        end
    end
    local u = getRootPart()
    if not u then
        return nil
    end
    local k, T
    for B, W in ipairs(B:GetChildren()) do
        local l = W:IsA("BasePart") and W:FindFirstChild("Interagir")
        if l and l.Enabled then
            local B = (u.Position - W.Position).Magnitude
            if not T or B < T then
                k, T = W, B
            end
        end
    end
    return k
end
local function stopWeaponPartsFarm()
    runtimeState.farmPartesRunId = runtimeState.farmPartesRunId + 1
    featureSettings.farmPartesEnabled = false
end
local weaponPartCollectDelay = 15
local function runWeaponPartsFarm()
    if featureSettings.flyEnabled then
        featureSettings.flyEnabled = false
        pcall(updateFly)
    end
    runtimeState.farmPartesRunId = runtimeState.farmPartesRunId + 1
    local B = runtimeState.farmPartesRunId
    local u = 0
    local k = featureSettings.noclipEnabled
    if not k then
        featureSettings.noclipEnabled = true
        pcall(applyCharacterNoclip)
    end
    while featureSettings.farmPartesEnabled and (not runtimeState.destroyed and B == runtimeState.farmPartesRunId) do
        local k = findWeaponPartLocation()
        if not k then
            task.wait(1)
        else
            local T = k
            local W = teleportToItem({
                item = T,
                parte = T,
            }, function()
                return findWeaponPartLocation() ~= T
            end)
            if B ~= runtimeState.farmPartesRunId then
                break
            end
            local l = W and findWeaponPartLocation() ~= T
            if l then
                u = u + 1
            end
            local I = l and weaponPartCollectDelay or 1
            local Y = os.clock() + I
            while os.clock() < Y do
                if
                    not featureSettings.farmPartesEnabled
                    or runtimeState.destroyed
                    or B ~= runtimeState.farmPartesRunId
                then
                    break
                end
                task.wait(0.2)
            end
        end
    end
    if B == runtimeState.farmPartesRunId then
        featureSettings.farmPartesEnabled = false
        runtimeState.farmPartesColetadas = u
    end
    if not k and (not featureSettings.farmPartesEnabled or B == runtimeState.farmPartesRunId) then
        featureSettings.noclipEnabled = false
        pcall(applyCharacterNoclip)
    end
    return u
end
-- Weapon scoring and automatic best-weapon selection.
local fallbackWeaponScores = {
    ArmaPDSecreta = 720,
    Barret = 700,
    ["FuzilTrovão"] = 340,
    FuzilPhantom = 335,
    FAL = 285,
    FALPolicial = 285,
    AK47 = 275,
    Fuzil = 265,
    FuzilPolicial = 265,
    G3 = 250,
    G3Policial = 250,
    Submetralhadora = 120,
    Uzi = 115,
    Glock = 185,
    GlockPolicial = 185,
    Pt = 155,
    ["Oitão"] = 145,
}
local measuredWeaponScores = {}
local lastWeaponScoreScan = 0
local function calculateWeaponScore(B)
    if not B or not B:IsA("Tool") then
        return nil
    end
    local u = B:FindFirstChild("Cfg")
    if not u or not u:IsA("ModuleScript") then
        return nil
    end
    local k, T = pcall(require, u)
    if not k or type(T) ~= "table" then
        return nil
    end
    local W = tonumber(T.BaseDamage)
    local l = tonumber(T.FireRate)
    if not W or not l then
        return nil
    end
    local I = tonumber(T.Range) or 0
    local Y = tonumber(T.AmmoPerMag) or 0
    return ((W / math.max(0.05, l) + W * 2) + I * 0.01) + Y * 0.1
end
local function scanWeaponScores(u)
    local k = os.clock()
    if not u and k - lastWeaponScoreScan < 5 then
        return
    end
    lastWeaponScoreScan = k
    for B, u in ipairs(PlayersService:GetPlayers()) do
        local k = {
            u:FindFirstChild("Backpack"),
            u.Character,
        }
        for B, u in ipairs(k) do
            if u then
                for B, u in ipairs(u:GetChildren()) do
                    local k = calculateWeaponScore(u)
                    if k then
                        measuredWeaponScores[u.Name] = k
                    end
                end
            end
        end
    end
end
local function getWeaponScore(B)
    return measuredWeaponScores[B] or fallbackWeaponScores[B]
end
local function getPersonalInventoryContent()
    local B = localPlayer:FindFirstChild("PlayerGui")
    local u = B and B:FindFirstChild("InventoryUI")
    local k = u and u:FindFirstChild("InvFrame")
    local T = k and k:FindFirstChild("InvPessoal")
    return T and T:FindFirstChild("ConteudoInv")
end
local function collectOwnedWeapons()
    scanWeaponScores(false)
    local B = {}
    local u = getPersonalInventoryContent()
    if u then
        for u, k in ipairs(u:GetChildren()) do
            local T = k:FindFirstChild("In")
            if T then
                for u, k in ipairs(T:GetChildren()) do
                    if k:IsA("Frame") and getWeaponScore(k.Name) then
                        B[k.Name] = true
                    end
                end
            end
        end
    end
    for u, k in ipairs({
        localPlayer:FindFirstChild("Backpack"),
        localPlayer.Character,
    }) do
        if k then
            for u, k in ipairs(k:GetChildren()) do
                if k:IsA("Tool") then
                    local u = calculateWeaponScore(k)
                    if u then
                        measuredWeaponScores[k.Name] = u
                        B[k.Name] = true
                    end
                end
            end
        end
    end
    return B
end
local function getBestOwnedWeapon()
    local B, u = nil, -math.huge
    for k in pairs(collectOwnedWeapons()) do
        local T = getWeaponScore(k)
        if T and (T > u or (T == u and (not B or k < B))) then
            B, u = k, T
        end
    end
    return B, u
end
local function findEquippedWeapon(B)
    local u = localPlayer:FindFirstChild("Backpack")
    local k = localPlayer.Character
    local T = u and u:FindFirstChild(B)
    if not T and k then
        T = k:FindFirstChild(B)
    end
    return T and (T:IsA("Tool") and T) or nil
end
local function listEquippedTools()
    local B, u = {}, {}
    for k, T in ipairs({
        localPlayer:FindFirstChild("Backpack"),
        localPlayer.Character,
    }) do
        if T then
            for k, T in ipairs(T:GetChildren()) do
                if T:IsA("Tool") and not u[T] then
                    u[T] = true
                    table.insert(B, T)
                end
            end
        end
    end
    return B
end
local function equipBestWeapon()
    if not featureSettings.bestWeaponEnabled or runtimeState.destroyed then
        return
    end
    local B, u = getBestOwnedWeapon()
    if not B or findEquippedWeapon(B) then
        return
    end
    local k = os.clock()
    if runtimeState.bestWeaponLastName == B and k - runtimeState.bestWeaponLastAttempt < 2 then
        return
    end
    runtimeState.bestWeaponLastName = B
    runtimeState.bestWeaponLastAttempt = k
    local T = game:GetService("ReplicatedStorage")
    local W = T:FindFirstChild("OrpheusShared")
    local l = W and W:FindFirstChild("InvRemotes")
    local Y = l and l:FindFirstChild("InvRequest")
    if not Y or not Y:IsA("RemoteFunction") then
        return
    end
    local q = listEquippedTools()
    local x = localPlayer:FindFirstChild("InvVals")
    local o = x and x:FindFirstChild("SlotsAtivos")
    local i = o and tonumber(o.Value) or 3
    if #q >= i then
        local k, T = nil, math.huge
        for u, W in ipairs(q) do
            if W.Name ~= B then
                local B = getWeaponScore(W.Name) or -1
                if B < T then
                    k, T = W, B
                end
            end
        end
        if k and T < u then
            pcall(function()
                Y:InvokeServer("desequipainv", k)
            end)
            task.wait(0.2)
        end
    end
    if not findEquippedWeapon(B) then
        pcall(function()
            Y:InvokeServer("semespacoinv", B)
        end)
    end
end
local function setBestWeaponEnabled(B)
    featureSettings.bestWeaponEnabled = B == true
    runtimeState.bestWeaponRunId = runtimeState.bestWeaponRunId + 1
    runtimeState.bestWeaponLastAttempt = 0
    runtimeState.bestWeaponLastName = nil
    local u = runtimeState.bestWeaponRunId
    if not featureSettings.bestWeaponEnabled then
        return
    end
    task.spawn(function()
        while
            featureSettings.bestWeaponEnabled and (not runtimeState.destroyed and runtimeState.bestWeaponRunId == u)
        do
            pcall(equipBestWeapon)
            task.wait(0.8)
        end
    end)
end
-- Vehicle durability HUD.
do
    local defaultVehicleDurability = 1500
    local healthyDurabilityColor = Color3.fromRGB(70, 190, 120)
    local warningDurabilityColor = Color3.fromRGB(232, 190, 70)
    local criticalDurabilityColor = Color3.fromRGB(232, 92, 84)
    local durabilityHudBackground = Color3.fromRGB(16, 16, 18)
    local durabilityHudBorder = Color3.fromRGB(46, 46, 52)
    local durabilityBarBackground = Color3.fromRGB(38, 38, 43)
    local durabilityTextColor = Color3.fromRGB(233, 234, 238)
    local durabilityMutedTextColor = Color3.fromRGB(150, 152, 160)
    local durabilityFontPath = "rbxasset://fonts/families/Nunito.json"
    local durabilityBoldFont = Font.new(durabilityFontPath, Enum.FontWeight.Bold, Enum.FontStyle.Normal)
    local durabilityRegularFont = Font.new(durabilityFontPath, Enum.FontWeight.Regular, Enum.FontStyle.Normal)
    local durabilityHudWidth, durabilityHudHeight = 208, 64
    local durabilityTweenInfo = TweenInfo.new(0.25, Enum.EasingStyle.Quart, Enum.EasingDirection.Out)
    local durabilityHudState = {
        gui = nil,
        frame = nil,
        fill = nil,
        percentLabel = nil,
        valueLabel = nil,
        carLabel = nil,
        uiScale = nil,
        vidaConn = nil,
        car = nil,
        maxVida = defaultVehicleDurability,
        savedPosition = nil,
    }
    local function createUiInstance(B, u, k)
        local T = Instance.new(B)
        for B, u in pairs(u) do
            T[B] = u
        end
        T.Parent = k
        return T
    end
    local function resolveHudParent()
        if gethui then
            local B, u = pcall(gethui)
            if B and u then
                return u
            end
        end
        return localPlayer:WaitForChild("PlayerGui")
    end
    local function getDurabilityColor(B)
        if B <= 20 then
            return criticalDurabilityColor
        end
        if B <= 50 then
            return warningDurabilityColor
        end
        return healthyDurabilityColor
    end
    local function getViewportSize()
        local B = WorkspaceService.CurrentCamera
        return B and B.ViewportSize or Vector2.new(800, 600)
    end
    local function updateDurabilityHudScale()
        if not durabilityHudState.uiScale then
            return
        end
        local B = getViewportSize()
        durabilityHudState.uiScale.Scale = math.clamp(B.X / 1280, 0.85, 1.1)
    end
    local function getDefaultDurabilityHudPosition()
        local B = getViewportSize()
        local u = math.max(40, (B.Y - durabilityHudHeight) - 150)
        return UDim2.fromOffset(14, u)
    end
    local function updateDurabilityHud(B)
        if not durabilityHudState.frame then
            return
        end
        if B > durabilityHudState.maxVida then
            durabilityHudState.maxVida = B
        end
        local u = math.clamp((B / durabilityHudState.maxVida) * 100, 0, 100)
        local k = getDurabilityColor(u)
        durabilityHudState.percentLabel.Text = string.format("%d%%", math.floor(u + 0.5))
        durabilityHudState.percentLabel.TextColor3 = k
        durabilityHudState.valueLabel.Text =
            string.format("%d / %d", math.floor(B + 0.5), math.floor(durabilityHudState.maxVida + 0.5));
        (TweenService:Create(durabilityHudState.fill, durabilityTweenInfo, {
            Size = UDim2.new(u / 100, 0, 1, 0),
            BackgroundColor3 = k,
        })):Play()
    end
    local function createDurabilityHud()
        if durabilityHudState.gui and durabilityHudState.gui.Parent then
            return
        end
        local B = createUiInstance("ScreenGui", {
            Name = "OiDurabilidade",
            ResetOnSpawn = false,
            IgnoreGuiInset = true,
            DisplayOrder = 9999,
            ZIndexBehavior = Enum.ZIndexBehavior.Sibling,
        }, resolveHudParent())
        local k = createUiInstance("Frame", {
            Name = "HUD",
            Active = true,
            AnchorPoint = Vector2.new(0, 0),
            Position = durabilityHudState.savedPosition or getDefaultDurabilityHudPosition(),
            Size = UDim2.fromOffset(durabilityHudWidth, durabilityHudHeight),
            BackgroundColor3 = durabilityHudBackground,
            BackgroundTransparency = 0.08,
            BorderSizePixel = 0,
            Visible = false,
        }, B)
        durabilityHudState.uiScale = createUiInstance("UIScale", {
            Scale = 1,
        }, k)
        createUiInstance("UICorner", {
            CornerRadius = UDim.new(0, 10),
        }, k)
        createUiInstance("UIStroke", {
            Color = durabilityHudBorder,
            Transparency = 0.2,
            Thickness = 1,
            ApplyStrokeMode = Enum.ApplyStrokeMode.Border,
        }, k)
        createUiInstance("UIPadding", {
            PaddingTop = UDim.new(0, 9),
            PaddingBottom = UDim.new(0, 9),
            PaddingLeft = UDim.new(0, 11),
            PaddingRight = UDim.new(0, 11),
        }, k)
        durabilityHudState.carLabel = createUiInstance("TextLabel", {
            Name = "Carro",
            Position = UDim2.fromOffset(0, 0),
            Size = UDim2.new(1, -46, 0, 15),
            BackgroundTransparency = 1,
            Text = "Veiculo",
            TextColor3 = durabilityTextColor,
            TextSize = 14,
            FontFace = durabilityBoldFont,
            TextXAlignment = Enum.TextXAlignment.Left,
            TextTruncate = Enum.TextTruncate.AtEnd,
        }, k)
        durabilityHudState.percentLabel = createUiInstance("TextLabel", {
            Name = "Percent",
            AnchorPoint = Vector2.new(1, 0),
            Position = UDim2.new(1, 0, 0, 0),
            Size = UDim2.fromOffset(46, 15),
            BackgroundTransparency = 1,
            Text = "--%",
            TextColor3 = healthyDurabilityColor,
            TextSize = 15,
            FontFace = durabilityBoldFont,
            TextXAlignment = Enum.TextXAlignment.Right,
        }, k)
        local T = createUiInstance("Frame", {
            Name = "Barra",
            Position = UDim2.fromOffset(0, 24),
            Size = UDim2.new(1, 0, 0, 9),
            BackgroundColor3 = durabilityBarBackground,
            BorderSizePixel = 0,
            ClipsDescendants = true,
        }, k)
        createUiInstance("UICorner", {
            CornerRadius = UDim.new(1, 0),
        }, T)
        durabilityHudState.fill = createUiInstance("Frame", {
            Name = "Fill",
            Size = UDim2.new(1, 0, 1, 0),
            BackgroundColor3 = healthyDurabilityColor,
            BorderSizePixel = 0,
        }, T)
        createUiInstance("UICorner", {
            CornerRadius = UDim.new(1, 0),
        }, durabilityHudState.fill)
        durabilityHudState.valueLabel = createUiInstance("TextLabel", {
            Name = "Valor",
            Position = UDim2.fromOffset(0, 37),
            Size = UDim2.new(1, 0, 0, 12),
            BackgroundTransparency = 1,
            Text = "-- / --",
            TextColor3 = durabilityMutedTextColor,
            TextSize = 12,
            FontFace = durabilityRegularFont,
            TextXAlignment = Enum.TextXAlignment.Left,
        }, k)
        durabilityHudState.gui = B
        durabilityHudState.frame = k
        updateDurabilityHudScale()
        trackConnection((WorkspaceService:GetPropertyChangedSignal("CurrentCamera")):Connect(updateDurabilityHudScale))
        local W = WorkspaceService.CurrentCamera
        if W then
            trackConnection((W:GetPropertyChangedSignal("ViewportSize")):Connect(updateDurabilityHudScale))
        end
        durabilityHudState.painel = styleHudFrame(k, {
            Titulo = durabilityHudState.carLabel,
        })
        makeDraggable(k, nil, function(B)
            durabilityHudState.savedPosition = B
        end)
    end
    local function getVehicleDurabilityValue(B)
        local u = B:FindFirstChild("Body")
        local k = u and u:FindFirstChild("Pintura")
        local T = k and k:FindFirstChild("LifeController")
        return T and T:FindFirstChild("Vida")
    end
    runtimeState.vidaDoCarro = getVehicleDurabilityValue
    local function getSeatedVehicle()
        local B = getHumanoid()
        local u = B and B.SeatPart
        if not u then
            return nil
        end
        local k = WorkspaceService:FindFirstChild("CarrosSpawnados")
        if not k then
            return nil
        end
        local T = u
        while T and T.Parent do
            if T.Parent == k then
                return T
            end
            T = T.Parent
        end
        return nil
    end
    local function disconnectDurabilityVehicle()
        if durabilityHudState.vidaConn then
            durabilityHudState.vidaConn:Disconnect()
            durabilityHudState.vidaConn = nil
        end
        durabilityHudState.car = nil
        if durabilityHudState.painel then
            durabilityHudState.painel:SetVisible(false)
        end
    end
    local function bindDurabilityVehicle(u)
        local k = getVehicleDurabilityValue(u)
        if not k then
            return
        end
        if durabilityHudState.vidaConn then
            durabilityHudState.vidaConn:Disconnect()
        end
        durabilityHudState.car = u
        durabilityHudState.maxVida = math.max(defaultVehicleDurability, k.Value)
        if durabilityHudState.carLabel then
            durabilityHudState.carLabel.Text = u.Name
        end
        if durabilityHudState.painel then
            durabilityHudState.painel:SetVisible(true)
        end
        updateDurabilityHud(k.Value)
        durabilityHudState.vidaConn = (k:GetPropertyChangedSignal("Value")):Connect(function()
            updateDurabilityHud(k.Value)
        end)
    end
    function runtimeState.ativarDurabilidade()
        createDurabilityHud()
        runtimeState.durabilidadeRunId = runtimeState.durabilidadeRunId + 1
        local B = runtimeState.durabilidadeRunId
        task.spawn(function()
            while
                featureSettings.durabilidadeHudEnabled
                and (not runtimeState.destroyed and runtimeState.durabilidadeRunId == B)
            do
                local B = getSeatedVehicle()
                if B ~= durabilityHudState.car then
                    if B and getVehicleDurabilityValue(B) then
                        pcall(bindDurabilityVehicle, B)
                    else
                        pcall(disconnectDurabilityVehicle)
                    end
                elseif B and not durabilityHudState.vidaConn then
                    pcall(bindDurabilityVehicle, B)
                end
                task.wait(0.3)
            end
        end)
    end
    function runtimeState.desativarDurabilidade()
        runtimeState.durabilidadeRunId = runtimeState.durabilidadeRunId + 1
        pcall(disconnectDurabilityVehicle)
        if durabilityHudState.painel then
            durabilityHudState.painel:SetVisible(false)
        end
    end
    function runtimeState.destruirDurabilidade()
        pcall(disconnectDurabilityVehicle)
        if durabilityHudState.gui then
            pcall(function()
                durabilityHudState.gui:Destroy()
            end)
        end
        durabilityHudState.gui, durabilityHudState.frame, durabilityHudState.fill, durabilityHudState.painel =
            nil, nil, nil, nil
        durabilityHudState.percentLabel, durabilityHudState.valueLabel, durabilityHudState.carLabel, durabilityHudState.uiScale =
            nil, nil, nil, nil
    end
end
-- Best-vehicle catalog ranking and ESP.
do
    local maxCatalogVehicleValue = 1e+50
    local bestCarCatalogLimit = 10
    local bestCarsUpdateInterval = 0.1
    local bestCarsState = {
        bundles = {},
        catalogo = {},
        proximaAtualizacao = 0,
    }
    local function getNumericChildValue(B, u)
        local k = B and B:FindFirstChild(u)
        return k and tonumber(k.Value) or 0
    end
    local function readVehicleCatalogEntry(B)
        local u = B and B:FindFirstChild("Config")
        if not u then
            return nil
        end
        return {
            nome = B.Name,
            valor = getNumericChildValue(u, "Valor"),
        }
    end
    local function rebuildBestCarsCatalog()
        local B = game:GetService("ReplicatedStorage")
        local u = B:FindFirstChild("OrpheusShared")
        local W = u and u:FindFirstChild("Concessionaria")
        local l = W and W:FindFirstChild("Veiculos")
        if not l then
            bestCarsState.catalogo = {}
            return
        end
        local Y = {}
        for B, u in ipairs(l:GetChildren()) do
            if u:IsA("Model") then
                local B = readVehicleCatalogEntry(u)
                if B and (B.valor > 0 and B.valor < maxCatalogVehicleValue) then
                    table.insert(Y, B)
                end
            end
        end
        table.sort(Y, function(B, u)
            if B.valor == u.valor then
                return B.nome < u.nome
            end
            return B.valor > u.valor
        end)
        bestCarsState.catalogo = {}
        for B = 1, math.min(bestCarCatalogLimit, #Y), 1 do
            local u = Y[B]
            bestCarsState.catalogo[u.nome] = u
        end
        for B = 2, #Y, 1 do
            assert(Y[B - 1].valor >= Y[B].valor, "ranking de carros fora de ordem")
        end
    end
    local getVehicleEspPart = LPH_NO_VIRTUALIZE(function(B)
        return B.PrimaryPart or B:FindFirstChild("DriveSeat", true) or B:FindFirstChildWhichIsA("BasePart", true)
    end)
    local formatCurrency = LPH_NO_VIRTUALIZE(function(B)
        local u = ((((tostring(math.floor(B + 0.5))):reverse()):gsub("(%d%d%d)", "%1.")):reverse()):gsub("^%.", "")
        return u
    end)
    local escapeRichText = LPH_NO_VIRTUALIZE(function(B)
        return (((((tostring(B)):gsub("&", "&amp;")):gsub("<", "&lt;")):gsub(">", "&gt;")):gsub('"', "&quot;")):gsub(
            "'",
            "&apos;"
        )
    end)
    local getVehicleOccupant = LPH_NO_VIRTUALIZE(function(u)
        local k = u:FindFirstChild("DriveSeat", true) or u:FindFirstChildWhichIsA("VehicleSeat", true)
        local T = k and (k:IsA("VehicleSeat") and k.Occupant) or nil
        local W = T and PlayersService:GetPlayerFromCharacter(T.Parent) or nil
        return W and W.Name or "Desocupado", W ~= nil
    end)
    local updateVehicleBillboardOffset = LPH_NO_VIRTUALIZE(function(B, u, k)
        local T, W, I = pcall(function()
            return B:GetBoundingBox()
        end)
        if not T then
            u.gui.StudsOffsetWorldSpace = Vector3.new(0, 6, 0)
            return
        end
        local Y = WorkspaceService.CurrentCamera
        local q = W.Position + Vector3.new(0, I.Y * 0.5, 0)
        if Y then
            local B = math.max(1, (q - Y.CFrame.Position):Dot(Y.CFrame.LookVector))
            local u = math.max(1, Y.ViewportSize.Y)
            local k = ((2 * B) * math.tan(math.rad(Y.FieldOfView * 0.5))) / u
            q = q + (Y.CFrame.UpVector * k) * 50
        else
            q = q + Vector3.new(0, 6, 0)
        end
        u.gui.StudsOffsetWorldSpace = q - k.Position
    end)
    local function destroyBestCarBundle(B)
        local u = bestCarsState.bundles[B]
        if not u then
            return
        end
        if u.highlight then
            pcall(function()
                u.highlight:Destroy()
            end)
        end
        if u.gui then
            pcall(function()
                u.gui:Destroy()
            end)
        end
        bestCarsState.bundles[B] = nil
    end
    local function createBestCarBundle(B, u)
        local k = Color3.fromRGB(255, 196, 72)
        local T = Instance.new("Highlight")
        T.Name = "KnwareBestCarESP"
        T.Adornee = B
        T.DepthMode = Enum.HighlightDepthMode.AlwaysOnTop
        T.FillColor = k
        T.OutlineColor = k
        T.FillTransparency = 0.78
        T.OutlineTransparency = 0
        T.Parent = WorkspaceService.CurrentCamera or WorkspaceService
        local W = Instance.new("BillboardGui")
        W.Name = "KnwareBestCarLabel"
        W.Adornee = u
        W.AlwaysOnTop = true
        W.LightInfluence = 0
        W.MaxDistance = 10000
        W.Size = UDim2.fromOffset(260, 84)
        W.Parent = WorkspaceService.CurrentCamera or WorkspaceService
        local Y = Instance.new("Frame")
        Y.BackgroundColor3 = Color3.fromRGB(16, 16, 18)
        Y.BackgroundTransparency = 0.08
        Y.BorderSizePixel = 0
        Y.Size = UDim2.fromScale(1, 1)
        Y.Parent = W
        local q = Instance.new("UICorner")
        q.CornerRadius = UDim.new(0, 9)
        q.Parent = Y
        local x = Instance.new("UIStroke")
        x.Color = Color3.fromRGB(46, 46, 52)
        x.Transparency = 0.2
        x.Thickness = 1
        x.Parent = Y
        local o = Instance.new("TextLabel")
        o.BackgroundTransparency = 1
        o.Position = UDim2.fromOffset(10, 7)
        o.Size = UDim2.new(1, -20, 1, -14)
        o.Font = Enum.Font.GothamBold
        o.RichText = true
        o.TextColor3 = Color3.fromRGB(233, 234, 238)
        o.TextSize = 13
        o.TextWrapped = true
        o.TextXAlignment = Enum.TextXAlignment.Left
        o.TextYAlignment = Enum.TextYAlignment.Center
        o.Parent = Y
        local i = {
            highlight = T,
            gui = W,
            texto = o,
            parte = u,
        }
        bestCarsState.bundles[B] = i
        return i
    end
    local hideBestCarBundles = LPH_NO_VIRTUALIZE(function()
        for B, u in pairs(bestCarsState.bundles) do
            if u.highlight then
                u.highlight.Enabled = false
            end
            if u.gui then
                u.gui.Enabled = false
            end
        end
    end)
    local updateBestCarsEsp = LPH_NO_VIRTUALIZE(function()
        if not featureSettings.bestCarsEspEnabled then
            hideBestCarBundles()
            return
        end
        if next(bestCarsState.catalogo) == nil then
            rebuildBestCarsCatalog()
        end
        local B = WorkspaceService:FindFirstChild("CarrosSpawnados")
        local u = getRootPart()
        if not B or not u then
            hideBestCarBundles()
            return
        end
        local k = {}
        for B, T in ipairs(B:GetChildren()) do
            if T:IsA("Model") then
                local B = bestCarsState.catalogo[T.Name]
                local W = getVehicleEspPart(T)
                if W and B then
                    k[T] = true
                    local l = bestCarsState.bundles[T] or createBestCarBundle(T, W)
                    l.highlight.Enabled = true
                    l.gui.Enabled = true
                    if l.parte ~= W then
                        l.parte = W
                        l.gui.Adornee = W
                    end
                    updateVehicleBillboardOffset(T, l, W)
                    local Y, q = getVehicleOccupant(T)
                    local x = q and "#E85C54" or "#46BE78"
                    l.texto.Text = string.format(
                        'Ocupante: <font color="%s">%s</font>\nVeículo: %s\nValor: $<font color="#46BE78">%s</font>\nDistância: %.0fm',
                        x,
                        escapeRichText(Y),
                        escapeRichText(T.Name),
                        formatCurrency(B.valor),
                        (W.Position - u.Position).Magnitude
                    )
                else
                    destroyBestCarBundle(T)
                end
            end
        end
        for B in pairs(bestCarsState.bundles) do
            if not k[B] or not B.Parent then
                destroyBestCarBundle(B)
            end
        end
    end)
    function runtimeState.ativarBestCarsEsp()
        rebuildBestCarsCatalog()
        bestCarsState.proximaAtualizacao = 0
        pcall(updateBestCarsEsp)
    end
    function runtimeState.desativarBestCarsEsp()
        hideBestCarBundles()
    end
    function runtimeState.destruirBestCarsEsp()
        for B in pairs(bestCarsState.bundles) do
            destroyBestCarBundle(B)
        end
        bestCarsState.catalogo = {}
    end
    trackConnection(RunService.RenderStepped:Connect(LPH_NO_VIRTUALIZE(function()
        local B = os.clock()
        if B < bestCarsState.proximaAtualizacao then
            return
        end
        bestCarsState.proximaAtualizacao = B + bestCarsUpdateInterval
        pcall(updateBestCarsEsp)
    end)))
end
-- UI assembly and callbacks for every exposed feature.
local legacyUiLibrary = mainEnvironment.__KNWARE_UI_LIBRARY
local mainWindow
local uiSetupSucceeded, uiSetupError = xpcall(function()
    local q = mainEnvironment.OiHubUI or mainEnvironment.__OIHUB_UI_LIBRARY
    if q then
        local i = 10
        local E = 2.5
        local function d()
            return localPlayer.Team and localPlayer.Team.Name or nil
        end
        local function X()
            local B = localPlayer:FindFirstChild("NewStats")
            local u = B and B:FindFirstChild("Level")
            return u and tonumber(u.Value) or 0
        end
        local function D(B)
            if B >= 1000 then
                return string.format("%.0fk", B / 1000)
            end
            return tostring(B)
        end
        local function H()
            local B, u = pcall(function()
                return require((game:GetService("ReplicatedStorage")).Mercadinho.PrefModule)
            end)
            if not B or type(u) ~= "table" or type(u.Empregos) ~= "table" then
                return {}
            end
            local k = {}
            for B, u in pairs(u.Empregos) do
                table.insert(k, {
                    nome = B,
                    level = tonumber(u.Level) or 0,
                    salario = tonumber(u.Salario) or 0,
                })
            end
            table.sort(k, function(B, u)
                if B.level == u.level then
                    return B.nome < u.nome
                end
                return B.level < u.level
            end)
            return k
        end
        local function R()
            local B = game:GetService("ReplicatedStorage")
            local u = B:FindFirstChild("Mercadinho")
            if not u then
                return {}
            end
            local k = {}
            local function T(B, T)
                local W, l = pcall(function()
                    return require(u[B])
                end)
                if not W or type(l) ~= "table" or type(l[T]) ~= "table" then
                    return
                end
                for B, u in pairs(l[T]) do
                    local T = tonumber(type(u) == "table" and u.Valor or u)
                    if T then
                        k[B] = math.max(k[B] or 0, T)
                    end
                end
            end
            T("MercadoModule", "ItemsMercadinho")
            T("MercadoNegroModule", "ItemsMercadoNegro")
            local W = {}
            for B, u in pairs(k) do
                table.insert(W, {
                    chave = B,
                    preco = u,
                })
            end
            local l, I = pcall(function()
                return require(u.HPModule)
            end)
            if l and (type(I) == "table" and type(I.ItensHP) == "table") then
                for B, u in pairs(I.ItensHP) do
                    local k = tonumber(type(u) == "table" and u.Valor or u)
                    if k then
                        table.insert(W, {
                            chave = B,
                            preco = k,
                            hpItem = true,
                        })
                    end
                end
            end
            table.sort(W, function(B, u)
                if B.preco == u.preco then
                    return B.chave < u.chave
                end
                return B.preco < u.preco
            end)
            return W
        end
        local function m()
            local B = localPlayer:FindFirstChild("leaderstats")
            local u = B and B:FindFirstChild("Dinheiro")
            return u and tonumber(u.Value) or 0
        end
        local function G()
            local B = localPlayer:FindFirstChild("PlayerGui")
            local u = B and B:FindFirstChild("InventoryUI")
            local k = u and u:FindFirstChild("InvFrame")
            local T = k and k:FindFirstChild("InvPessoal")
            return T and T:FindFirstChild("ConteudoInv")
        end
        local function U(B)
            local u = G()
            if u then
                local k = u:FindFirstChild(B, true)
                if k then
                    return k
                end
            end
            local k = localPlayer.Character
            local T = localPlayer:FindFirstChild("Backpack")
            return (k and k:FindFirstChild(B)) or (T and T:FindFirstChild(B)) or nil
        end
        local function f(B)
            local u = U(B)
            if not u then
                return 0
            end
            local k = u:FindFirstChild("Qnt", true)
            return (k and tonumber((k.Text or ""):match("%d+"))) or 1
        end
        local p
        local y = false
        local P
        local z = setmetatable({}, {
            __mode = "k",
        })
        local N = setmetatable({}, {
            __mode = "k",
        })
        local function g(B)
            runtimeState.garbageFarmStatus = tostring(B or "")
            if P and (P.Instance and P.Instance.Parent) then
                P.Instance.Text = runtimeState.garbageFarmStatus
            end
        end
        local function M(B)
            return featureSettings.autoFarmLixeiroEnabled
                and (not runtimeState.destroyed and runtimeState.garbageFarmRunId == B)
        end
        local function A(B, u)
            local k = os.clock() + math.max(0, B or 0)
            repeat
                if not M(u) then
                    return false
                end
                task.wait(math.min(0.1, math.max(0.01, k - os.clock())))
            until os.clock() >= k
            return M(u)
        end
        local function O()
            local B = localPlayer.Character
            return B, B and B:FindFirstChildOfClass("Humanoid"), B and B:FindFirstChild("HumanoidRootPart")
        end
        local function Q()
            local B = localPlayer:FindFirstChild("Inv")
            local u = 0
            if B then
                for B, k in ipairs(B:GetChildren()) do
                    if k.Name == "Saco de Lixo" then
                        u = u + 1
                    end
                end
            end
            return u
        end
        local function n()
            local B = localPlayer:FindFirstChild("leaderstats")
            local u = B and B:FindFirstChild("Dinheiro")
            return u and tonumber(u.Value) or nil
        end
        local function K()
            for B, u in ipairs(H()) do
                if string.lower(u.nome) == "lixeiro" then
                    return u
                end
            end
            return {
                nome = "Lixeiro",
                level = 1,
                salario = 0,
            }
        end
        local function a(B, u)
            B:FireServer(u.nome, u.level, true)
            local k = localPlayer:FindFirstChild("PlayerGui")
            local T = k and k:FindFirstChild("LixeiroUI")
            local W = T and T:FindFirstChild("LixeiroFrame")
            if W and W.Visible then
                W.Visible = false
            end
        end
        local function c(B)
            if d() == "Lixeiro" then
                return true
            end
            local u = (game:GetService("ReplicatedStorage")):FindFirstChild("Mercadinho")
            local k = u and u:FindFirstChild("PrefRemote")
            if not k or not k:IsA("RemoteEvent") then
                g("PrefRemote nao encontrado; tentando novamente...")
                return false
            end
            local T = K()
            if X() < T.level then
                g(string.format("Lixeiro pede nivel %d; seu nivel e %d", T.level, X()))
                return false
            end
            g("Selecionando o emprego de lixeiro...")
            k:FireServer(T.nome, T.level)
            A(0.25, B)
            if not M(B) then
                return false
            end
            a(k, T)
            local W = os.clock() + i
            local l = os.clock() + 1.25
            repeat
                if not M(B) then
                    return false
                end
                if d() == "Lixeiro" then
                    g("Emprego aceito; aguardando o teleporte...")
                    A(1.2, B)
                    return true
                end
                if os.clock() >= l then
                    a(k, T)
                    l = os.clock() + 1.25
                end
                task.wait(0.15)
            until os.clock() >= W
            g("O servidor ainda nao confirmou o emprego.")
            return false
        end
        local function C(B, u)
            return B
                and (
                    B.Parent
                    and (
                        B:IsA("BasePart")
                        and (
                            B.Name == "Lixo"
                            and (
                                B.Transparency < 0.95
                                and (u and (u.Parent == B and (u:IsA("ProximityPrompt") and u.Enabled)))
                            )
                        )
                    )
                )
        end
        local function V()
            local B, u, k = O()
            local T = WorkspaceService:FindFirstChild("Lixeiro")
            local W = {}
            if not k or not T then
                return W
            end
            for B, u in ipairs(T:GetChildren()) do
                local T = u:FindFirstChild("PegarLixo")
                if C(u, T) and (not N[u] and (not z[u] or z[u] <= os.clock())) then
                    table.insert(W, {
                        parte = u,
                        prompt = T,
                        posicao = u.Position,
                        distancia = (u.Position - k.Position).Magnitude,
                    })
                end
            end
            table.sort(W, function(B, u)
                return B.distancia < u.distancia
            end)
            return W
        end
        local function S(B, u, k)
            if not B or not u or not u.Parent then
                return false
            end
            local T = k and math.max(2.5, k.MaxActivationDistance - 0.65) or 4.25
            return (B.Position - u.Position).Magnitude <= T
        end
        local function r(B, u, k)
            local T = k and math.max(2.5, k.MaxActivationDistance - 0.8) or 4
            local W = Vector3.new(B.Position.X - u.Position.X, 0, B.Position.Z - u.Position.Z)
            local l = W.Magnitude > 0.1 and W.Unit or Vector3.new(0, 0, 1)
            local I = math.max(u.Size.X, u.Size.Z) * 0.5
            local Y = math.clamp(I + 1.25, 1.25, math.max(1.25, T - 0.35))
            return u.Position + l * Y
        end
        local function t(B, k, T)
            local I, Y, q = O()
            if not I or not Y or not q or Y.Health <= 0 then
                return false
            end
            if S(q, B, k) then
                return true
            end
            for x = 1, 4, 1 do
                if not M(T) or not B.Parent then
                    return false
                end
                I, Y, q = O()
                if not I or not Y or not q or Y.Health <= 0 then
                    return false
                end
                if S(q, B, k) then
                    return true
                end
                local o = r(q, B, k)
                local i = PathfindingService:CreatePath({
                    AgentRadius = 2,
                    AgentHeight = 5,
                    AgentCanJump = true,
                    AgentCanClimb = true,
                    WaypointSpacing = 5,
                })
                local E = pcall(function()
                    i:ComputeAsync(q.Position, o)
                end)
                local L = E and (i.Status == Enum.PathStatus.Success and i:GetWaypoints())
                    or {
                        {
                            Position = o,
                            Action = Enum.PathWaypointAction.Walk,
                        },
                    }
                local Z = 0
                local J = false
                local F = i.Blocked:Connect(function(B)
                    if B <= Z + 1 then
                        J = true
                    end
                end)
                for W, x in ipairs(L) do
                    Z = W
                    if not M(T) or J then
                        break
                    end
                    I, Y, q = O()
                    if not I or not Y or not q or Y.Health <= 0 then
                        break
                    end
                    if S(q, B, k) then
                        F:Disconnect()
                        return true
                    end
                    if x.Action == Enum.PathWaypointAction.Jump then
                        Y.Jump = true
                        pcall(function()
                            Y:ChangeState(Enum.HumanoidStateType.Jumping)
                        end)
                    end
                    Y:MoveTo(x.Position)
                    local o = q.Position
                    local i = os.clock()
                    local E = 0
                    local L = os.clock() + 8
                    repeat
                        RunService.Heartbeat:Wait()
                        if not M(T) then
                            F:Disconnect()
                            return false
                        end
                        I, Y, q = O()
                        if not I or not Y or not q then
                            break
                        end
                        if S(q, B, k) then
                            F:Disconnect()
                            return true
                        end
                        local W = os.clock()
                        if (q.Position - o).Magnitude >= 0.8 then
                            o = q.Position
                            i = W
                        elseif W - i >= 1.25 and W - E >= 1 then
                            E = W
                            Y.Jump = true
                            pcall(function()
                                Y:ChangeState(Enum.HumanoidStateType.Jumping)
                            end)
                            Y:MoveTo(x.Position)
                        end
                        local Z = Vector3.new(x.Position.X - q.Position.X, 0, x.Position.Z - q.Position.Z)
                        if Z.Magnitude > 1 then
                            local u = RaycastParams.new()
                            u.FilterType = Enum.RaycastFilterType.Exclude
                            u.FilterDescendantsInstances = {
                                I,
                                B,
                            }
                            local k = WorkspaceService:Raycast(q.Position + Vector3.new(0, 1.5, 0), Z.Unit * 3.5, u)
                            if k and (k.Instance and (k.Instance.CanCollide and W - E >= 0.8)) then
                                E = W
                                Y.Jump = true
                                pcall(function()
                                    Y:ChangeState(Enum.HumanoidStateType.Jumping)
                                end)
                            end
                        end
                        if W - i >= 3 then
                            J = true
                        end
                    until J or os.clock() >= L or (q.Position - x.Position).Magnitude <= 3
                end
                F:Disconnect()
                I, Y, q = O()
                if q and S(q, B, k) then
                    return true
                end
                g(string.format("Obstaculo detectado; recalculando rota (%d/4)...", x))
                A(0.2, T)
            end
            return false
        end
        local function h(B, u)
            if not B or not B.Parent or not B.Enabled then
                return false
            end
            if not u and type(fireproximityprompt) == "function" then
                return pcall(fireproximityprompt, B, 0)
            end
            return pcall(function()
                B:InputHoldBegin()
                task.wait(math.max(0.15, B.HoldDuration + 0.1))
                B:InputHoldEnd()
            end)
        end
        local function j(B, u, k)
            local T = Q()
            for W = 1, 3, 1 do
                if not M(k) or not C(B, u) then
                    return not B.Parent or B.Transparency >= 0.95
                end
                local l, I, Y = O()
                if not S(Y, B, u) then
                    return false
                end
                h(u, W > 1)
                local q = os.clock() + 2.75
                repeat
                    if not M(k) then
                        return false
                    end
                    if not B.Parent or B.Transparency >= 0.95 or Q() > T then
                        return true
                    end
                    task.wait(0.1)
                until os.clock() >= q
            end
            return false
        end
        local function B0()
            local B = WorkspaceService:FindFirstChild("EntregaLixo")
            local u = B and B:FindFirstChild("Lixeira")
            local k = u and u:FindFirstChild("EntregarLixo")
            if u and (u:IsA("BasePart") and (k and k:IsA("ProximityPrompt"))) then
                return u, k
            end
            return nil, nil
        end
        local function u0(B, u)
            local k, T = B0()
            if not k or not T then
                g("Ponto EntregaLixo nao encontrado.")
                return false
            end
            g(string.format("%d/%d coletados; indo entregar...", B, B))
            if not t(k, T, u) then
                if not M(u) then
                    return false
                end
                g("Rota da entrega bloqueada; tentando novamente...")
                return false
            end
            local W = Q()
            local l = n()
            for k = 1, 3, 1 do
                if not M(u) then
                    return false
                end
                g(string.format("Entregando lixos (%d/3)...", k))
                h(T, k > 1)
                local I = os.clock() + 4
                repeat
                    if not M(u) then
                        return false
                    end
                    local k = Q()
                    local T = n()
                    if (W > 0 and k < W) or (l and (T and T > l)) then
                        local u = l and (T and math.max(0, T - l)) or 0
                        g(
                            string.format(
                                "Entrega concluida: %d lixos%s",
                                B,
                                u > 0 and string.format(" (+$%d)", u) or ""
                            )
                        )
                        return true
                    end
                    task.wait(0.15)
                until os.clock() >= I
            end
            g("Entrega ainda nao confirmada; repetindo a rota...")
            return false
        end
        local function k0(B)
            while M(B) do
                if not c(B) then
                    A(2, B)
                else
                    local u = math.clamp(math.floor(tonumber(featureSettings.autoFarmLixeiroQuantidade) or 20), 1, 20)
                    local k = 0
                    table.clear(N)
                    while M(B) and (d() == "Lixeiro" and k < u) do
                        local T = V()
                        if #T == 0 then
                            g(string.format("Coletados %d/%d; aguardando lixos disponiveis...", k, u))
                            table.clear(N)
                            A(1.25, B)
                        else
                            local W = T[1]
                            if W.distancia > 60 and k == 0 then
                                g("Indo para a area de coleta de lixos...")
                            else
                                g(string.format("Indo ao lixo %d/%d...", k + 1, u))
                            end
                            if t(W.parte, W.prompt, B) then
                                g(string.format("Coletando lixo %d/%d...", k + 1, u))
                                if j(W.parte, W.prompt, B) then
                                    k = k + 1
                                    N[W.parte] = true
                                    g(string.format("Coletados: %d/%d", k, u))
                                    A(0.45, B)
                                else
                                    if not M(B) then
                                        break
                                    end
                                    z[W.parte] = os.clock() + 3
                                    g("Coleta nao confirmou; procurando outro lixo...")
                                    A(0.35, B)
                                end
                            else
                                if not M(B) then
                                    break
                                end
                                z[W.parte] = os.clock() + 5
                                g("Caminho bloqueado; procurando outra rota...")
                                A(0.4, B)
                            end
                        end
                    end
                    if not M(B) then
                        break
                    end
                    if d() ~= "Lixeiro" then
                        g("Emprego alterado; retomando lixeiro...")
                    elseif k >= u then
                        while M(B) and not u0(k, B) do
                            A(1, B)
                        end
                        if M(B) then
                            A(1.25, B)
                        end
                    end
                end
            end
        end
        local function T0(B)
            featureSettings.autoFarmLixeiroEnabled = B == true
            runtimeState.garbageFarmRunId = runtimeState.garbageFarmRunId + 1
            local u = runtimeState.garbageFarmRunId
            if featureSettings.autoFarmLixeiroEnabled then
                g("Verificando emprego de lixeiro...")
                task.spawn(function()
                    local B, k = pcall(k0, u)
                    if not B and M(u) then
                        g("Erro no Auto Farm: " .. tostring(k))
                        featureSettings.autoFarmLixeiroEnabled = false
                        runtimeState.garbageFarmRunId = runtimeState.garbageFarmRunId + 1
                    end
                end)
            else
                g("Auto Farm parado.")
                local B, u, k = O()
                if u and k then
                    u:MoveTo(k.Position)
                end
            end
        end
        local function W0(B)
            local u = B.ToggleButton
            if not u then
                return function() end
            end
            local T = 0.18
            local W = 0.045
            local l = 0.24
            local I = 0.55
            local Y = 12
            local q = 48
            local x = false
            local o = nil
            local i = {}
            local E = 0
            local L = 0
            local Z = 0
            local J = nil
            local function F(B)
                return Vector2.new(B.Position.X, B.Position.Y)
            end
            local function b()
                E = 0
                L = 0
                Z = 0
                J = nil
            end
            local function d(B)
                local u = B.UserInputType
                local k = {
                    posicao = F(B),
                    instante = os.clock(),
                }
                if u == Enum.UserInputType.MouseButton1 then
                    o = k
                elseif u == Enum.UserInputType.Touch then
                    i[B] = k
                end
            end
            local function X(B)
                local k = B.UserInputType
                local d
                if k == Enum.UserInputType.MouseButton1 then
                    d = o
                    o = nil
                elseif k == Enum.UserInputType.Touch then
                    d = i[B]
                    i[B] = nil
                else
                    return
                end
                if not d then
                    return
                end
                local X = os.clock()
                local s = X - d.instante
                local D = F(B)
                local H = (D - d.posicao).Magnitude
                if s > T or H > Y then
                    b()
                    return
                end
                if not x or not u.Parent then
                    b()
                    return
                end
                if L > 0 and X - L < W then
                    return
                end
                local R = X - L
                local m = E == 0 or R > l or X - Z > I or not J or (D - J).Magnitude > q
                if m then
                    E = 1
                    Z = X
                    J = D
                else
                    E = E + 1
                end
                L = X
                if E >= 3 then
                    u.Visible = not u.Visible
                    b()
                end
            end
            B:_track(UserInputService.InputBegan:Connect(function(B, u)
                local k = B.UserInputType
                if k == Enum.UserInputType.MouseButton1 or k == Enum.UserInputType.Touch then
                    if u then
                        if k == Enum.UserInputType.MouseButton1 then
                            o = nil
                        else
                            i[B] = nil
                        end
                        b()
                        return
                    end
                    d(B)
                end
            end))
            B:_track(UserInputService.InputEnded:Connect(function(B)
                X(B)
            end))
            return function()
                x = true
                b()
            end
        end
        local l0 = q:CreateWindow({
            Id = "SintoniaOi",
            Title = "Oi",
            Accent = "Hub",
            Subtitle = "Sintonia",
            Width = 1000,
            Height = 470,
            LogoUrl = "https://i.imgur.com/4mGyRZy.png",
            ToggleButtonImage = "https://i.imgur.com/gwkbuF6.png",
            ToggleButtonSize = 66,
            ToggleButtonImageInset = 3,
        })
        mainWindow = l0
        local I0 = W0(l0)
        l0:SetUser({
            Role = "Cliente Oi",
        })
        local Y0 = l0:AddSideTab({
            Name = "Seu Jogador",
            Group = "Geral",
            Icon = "user",
        })
        local q0 = Y0:AddTopTab({
            Name = "Geral",
            Icon = "user",
        })
        local x0 = q0:AddColumn()
        local o0 = 62
        local i0 = 6
        local function E0(B, u)
            return q.MergeKeybindRow(B, u)
        end
        local L0 = x0:AddSection("Walkspeed")
        local Z0
        Z0 = registerControl(
            "spoofWalkEnabled",
            L0:AddToggle({
                Text = "WalkSpeed",
                Default = featureSettings.spoofWalkEnabled,
                Callback = function(B)
                    featureSettings.spoofWalkEnabled = B
                    if not B then
                        local B = getHumanoid()
                        if B then
                            B.WalkSpeed = 16
                        end
                    end
                end,
            })
        )
        registerControl(
            "spoofWalkSpeed",
            L0:AddSlider({
                Text = "Velocidade",
                Hint = "Com o ClientAC silenciado o slider vale cru, movido por CFrame. Sem prova de silencio o teto cai para 105 studs/s e reduz sozinho para 65 em lag ou colisao. A propriedade WalkSpeed fica sempre em 28 nos dois casos: o servidor le ela replicada e kicka ate com o boneco parado.",
                Min = 16,
                Max = 250,
                Default = featureSettings.spoofWalkSpeed,
                Callback = function(B)
                    featureSettings.spoofWalkSpeed = B
                end,
            })
        )
        E0(
            Z0,
            registerControl(
                "walkSpeedKeybind",
                L0:AddKeybind({
                    Text = "Tecla",
                    Default = featureSettings.walkSpeedKeybind,
                    OnChanged = function(B)
                        featureSettings.walkSpeedKeybind = B
                    end,
                    Callback = function()
                        Z0:Set(not Z0:Get())
                    end,
                })
            )
        )
        local J0 = x0:AddSection("Jumppower")
        registerControl(
            "spoofJumpEnabled",
            J0:AddToggle({
                Text = "JumpPower",
                Default = featureSettings.spoofJumpEnabled,
                Callback = function(B)
                    featureSettings.spoofJumpEnabled = B
                    if not B then
                        local B = getHumanoid()
                        if B then
                            B.JumpPower = 50
                        end
                    end
                end,
            })
        )
        registerControl(
            "spoofJumpPower",
            J0:AddSlider({
                Text = "Forca",
                Min = 50,
                Max = 150,
                Default = featureSettings.spoofJumpPower,
                Callback = function(B)
                    featureSettings.spoofJumpPower = B
                end,
            })
        )
        local F0 = q0:AddColumn()
        local b0 = F0:AddSection("Seu Boneco")
        local d0 = registerControl(
            "noclipEnabled",
            b0:AddToggle({
                Text = "Noclip",
                Default = featureSettings.noclipEnabled,
                Callback = function(B)
                    featureSettings.noclipEnabled = B
                    pcall(applyCharacterNoclip)
                end,
            })
        )
        E0(
            d0,
            registerControl(
                "noclipKeybind",
                b0:AddKeybind({
                    Text = "Tecla Noclip",
                    Default = featureSettings.noclipKeybind,
                    OnChanged = function(B)
                        featureSettings.noclipKeybind = B
                    end,
                    Callback = function()
                        d0:Set(not d0:Get())
                    end,
                })
            )
        )
        local s0
        local D0 = registerControl(
            "flyEnabled",
            b0:AddToggle({
                Text = "Fly",
                Default = featureSettings.flyEnabled,
                Callback = function(B)
                    if B and featureSettings.farmPartesEnabled then
                        stopWeaponPartsFarm()
                        if s0 and s0:Get() then
                            s0:Set(false)
                        end
                    end
                    featureSettings.flyEnabled = B
                    pcall(updateFly)
                end,
            })
        )
        E0(
            D0,
            registerControl(
                "flyKeybind",
                b0:AddKeybind({
                    Text = "Tecla Fly",
                    Default = featureSettings.flyKeybind,
                    OnChanged = function(B)
                        featureSettings.flyKeybind = B
                    end,
                    Callback = function()
                        D0:Set(not D0:Get())
                    end,
                })
            )
        )
        registerControl(
            "flySpeed",
            b0:AddSlider({
                Text = "Vel. Fly",
                Hint = "Com o ClientAC silenciado vale o valor cru. Sem ele, o teto cai para 60 studs/s e reduz sozinho para 36 em lag, colisao ou velocidade fisica baixa.",
                Min = 16,
                Max = 250,
                Default = featureSettings.flySpeed,
                Callback = function(B)
                    featureSettings.flySpeed = B
                end,
            })
        )
        registerControl(
            "rolarEnabled",
            b0:AddToggle({
                Text = "Rolar",
                Default = featureSettings.rolarEnabled,
                Callback = function(B)
                    featureSettings.rolarEnabled = B
                    pcall(updateRollAnimation)
                end,
            })
        )
        registerControl(
            "antiRagdollEnabled",
            b0:AddToggle({
                Text = "Anti Ragdoll",
                Default = featureSettings.antiRagdollEnabled,
                Callback = function(B)
                    featureSettings.antiRagdollEnabled = B
                    pcall(applyAntiRagdoll)
                end,
            })
        )
        do
            local B = Y0:AddTopTab({
                Name = "Cosméticos",
                Icon = "cross",
            })
            local u = (B:AddColumn()):AddSection("Curar")
            local k
            local T
            local W = false
            local function l(B)
                for u, k in ipairs(R()) do
                    if k.chave == B then
                        return k
                    end
                end
            end
            local function Y(B, u, k)
                return math.max(0, math.ceil((u - B) / k))
            end
            assert(Y(0, 100, 100) == 1 and Y(100, 100, 100) == 0, "cálculo de insumos inválido")
            local function q(B, u)
                local function k(B)
                    if u and (u.Instance and u.Instance.Parent) then
                        u.Instance.Text = B
                    end
                end
                if W or y then
                    k("Outra ação está em andamento.")
                    return
                end
                W = true
                local function T(B)
                    y = false
                    W = false
                    k(B)
                end
                local I, q = B.valor()
                if not I then
                    T(B.nome .. " indisponível.")
                    return
                end
                if I >= B.cheio then
                    T(B.nome .. " já está cheio.")
                    return
                end
                local x = l(B.item)
                if not x then
                    T(B.item .. " não foi encontrado na Shop.")
                    return
                end
                local o = math.max(1, Y(I, q, B.ganho))
                local i = f(B.item)
                local E = math.max(0, o - i)
                local Z = E * x.preco
                if m() < Z then
                    T(string.format("Faltam $%d para %s x%d.", Z - m(), B.item, E))
                    return
                end
                if E > 0 then
                    y = true
                    k(string.format("Comprando %s x%d...", B.item, E))
                    local u = p(x, E)
                    y = false
                    if not u then
                        T("A compra automática de " .. (B.item .. " foi recusada."))
                        return
                    end
                    local W = os.clock() + 10
                    repeat
                        task.wait(0.15)
                    until f(B.item) >= o or runtimeState.destroyed or os.clock() >= W
                end
                local J = (game:GetService("ReplicatedStorage")):FindFirstChild("InGameRemotes")
                J = J and J:FindFirstChild("FomeRem")
                if not J or not J:IsA("RemoteEvent") then
                    T("Remote FomeRem não encontrado.")
                    return
                end
                for u = 1, o, 1 do
                    if f(B.item) == 0 then
                        T(B.item .. " não chegou ao inventário.")
                        return
                    end
                    I = B.valor()
                    k(string.format("Usando %s %d/%d...", B.item, u, o))
                    J:FireServer(B.item)
                    task.wait(6)
                    local W = B.valor()
                    if W <= I then
                        T("O servidor não confirmou o uso de " .. (B.item .. "."))
                        return
                    end
                    if W >= B.cheio then
                        break
                    end
                end
                I = B.valor()
                T(string.format("%s preenchido: %d/%d.", B.nome, math.floor(I + 0.5), q))
            end
            local x = {
                cura = {
                    nome = "Vida",
                    item = "Tratamento",
                    ganho = 100,
                    cheio = 100,
                    valor = function()
                        local B = getHumanoid()
                        return B and B.Health, B and B.MaxHealth or 100
                    end,
                },
                fome = {
                    nome = "Fome",
                    item = "Lanche",
                    ganho = 100,
                    cheio = 98,
                    valor = function()
                        local B = localPlayer:FindFirstChild("Status")
                        local u = B and B:FindFirstChild("Fome")
                        return u and tonumber(u.Value), 100
                    end,
                },
                sede = {
                    nome = "Sede",
                    item = "Água",
                    ganho = 100,
                    cheio = 98,
                    valor = function()
                        local B = localPlayer:FindFirstChild("Status")
                        local u = B and B:FindFirstChild("Sede")
                        return u and tonumber(u.Value), 100
                    end,
                },
            }
            u:AddButton({
                Text = "Curar",
                Callback = function()
                    q(x.cura, k)
                end,
            })
            u:AddDivider("Insumos")
            u:AddButton({
                Text = "Preencher fome",
                Callback = function()
                    q(x.fome, T)
                end,
            })
            u:AddButton({
                Text = "Preencher Sede",
                Callback = function()
                    q(x.sede, T)
                end,
            })
        end
        local function H0(B)
            l0:Notify({
                Title = "Sintonia",
                Text = tostring(B),
            })
        end
        if not runtimeState.placeMatched then
            H0("Você está num place diferente do alvo.")
        end
        local R0 = l0:AddSideTab({
            Name = "Auto Farm",
            Group = "Geral",
            Icon = "repeat",
        })
        local m0 = ((R0:AddTopTab({
            Name = "Lixeiro",
            Icon = "trash-2",
        })):AddColumn()):AddSection("Farm Lixo")
        registerControl(
            "autoFarmLixeiroEnabled",
            m0:AddToggle({
                Text = "Auto Farm Lixeiro",
                Default = featureSettings.autoFarmLixeiroEnabled,
                Callback = function(B)
                    T0(B)
                end,
            })
        )
        registerControl(
            "autoFarmLixeiroQuantidade",
            m0:AddSlider({
                Text = "Lixos por entrega",
                Min = 1,
                Max = 20,
                Step = 1,
                Default = featureSettings.autoFarmLixeiroQuantidade,
                Callback = function(B)
                    featureSettings.autoFarmLixeiroQuantidade = math.clamp(math.floor(B), 1, 20)
                end,
            })
        )
        local v0 = ((R0:AddTopTab({
            Name = "Partes de Arma",
            Icon = "wrench",
        })):AddColumn()):AddSection("Farm Partes")
        s0 = registerControl(
            "farmPartesEnabled",
            v0:AddToggle({
                Text = "Auto Farm Partes de Arma",
                Hint = "TP direto ate o ponto marcado no mapa e dispara o prompt. Sem o ClientAC silenciado, volta ao arrasto lento com a velocidade do TP de item, em Diversos.",
                Default = featureSettings.farmPartesEnabled,
                Callback = function(B)
                    if B and featureSettings.flyEnabled then
                        if D0 and D0:Get() then
                            D0:Set(false)
                        else
                            featureSettings.flyEnabled = false
                            pcall(updateFly)
                        end
                    end
                    featureSettings.farmPartesEnabled = B
                    if not B then
                        stopWeaponPartsFarm()
                        return
                    end
                    task.spawn(function()
                        local B = runWeaponPartsFarm()
                        l0:Notify({
                            Title = "Partes de Arma",
                            Text = string.format("Farm encerrado. Pecas coletadas: %d", B),
                        })
                    end)
                end,
            })
        )
        local e0 = l0:AddSideTab({
            Name = "Combat",
            Group = "Geral",
            Icon = "target",
        })
        local G0 = ((e0:AddTopTab({
            Name = "ESP",
            Icon = "eye",
        })):AddColumn()):AddSection("ESP")
        local U0
        local f0 = {
            "box",
            "box3d",
            "name",
            "distance",
            "heldItem",
            "skeleton",
            "highlight",
            "tracer",
            "arrow",
            "teamTag",
        }
        local function p0()
            for B, u in ipairs(f0) do
                if featureSettings[u] then
                    return true
                end
            end
            return false
        end
        U0 = registerControl(
            "espEnabled",
            G0:AddToggle({
                Text = "Ativar ESP",
                Default = featureSettings.espEnabled,
                Callback = function(B)
                    featureSettings.espEnabled = B
                    if not B then
                        pcall(hideAllEspBundles)
                        return
                    end
                    local u = drawingCapabilities.medir()
                    if not u.drawing then
                        H0(
                            "Sem Drawing API neste executor: caixa, nome, esqueleto e tracer nao vao aparecer. Chams funciona."
                        )
                    elseif not u.texto then
                        H0(
                            "Drawing sem suporte a Text neste executor: nome, distancia e time nao aparecem. Caixa, esqueleto e chams funcionam."
                        )
                    elseif not u.triangle then
                        H0("Drawing sem Triangle neste executor: a seta de fora da tela nao aparece. O resto funciona.")
                    end
                    if not p0() then
                        H0("ESP ligado, mas nenhum visual marcado. Escolha Chams, Caixa 2D ou Nome logo abaixo.")
                    end
                end,
            })
        )
        task.spawn(function()
            while not runtimeState.destroyed do
                if runtimeState.espErro then
                    H0("ESP falhou: " .. runtimeState.espErro)
                    return
                end
                task.wait(2)
            end
        end)
        registerControl(
            "box",
            G0:AddToggle({
                Text = "Caixa 2D",
                Default = featureSettings.box,
                Callback = function(B)
                    featureSettings.box = B
                    if not B then
                        for B, u in pairs(featureSettings.bundles) do
                            if u.box then
                                u.box.Visible = false
                            end
                        end
                    end
                end,
            })
        )
        registerControl(
            "box3d",
            G0:AddToggle({
                Text = "Caixa 3D",
                Default = featureSettings.box3d,
                Callback = function(B)
                    featureSettings.box3d = B
                    if not B then
                        for B, u in pairs(featureSettings.bundles) do
                            for B, u in ipairs(u.box3dLines) do
                                u.Visible = false
                            end
                        end
                    end
                end,
            })
        )
        registerControl(
            "name",
            G0:AddToggle({
                Text = "Nome",
                Default = featureSettings.name,
                Callback = function(B)
                    featureSettings.name = B
                    if not B then
                        for B, u in pairs(featureSettings.bundles) do
                            if u.name then
                                u.name.Visible = false
                            end
                        end
                    end
                end,
            })
        )
        registerControl(
            "distance",
            G0:AddToggle({
                Text = "Distancia",
                Default = featureSettings.distance,
                Callback = function(B)
                    featureSettings.distance = B
                    if not B then
                        for B, u in pairs(featureSettings.bundles) do
                            if u.distance then
                                u.distance.Visible = false
                            end
                        end
                    end
                end,
            })
        )
        registerControl(
            "heldItem",
            G0:AddToggle({
                Text = "Item na mao",
                Hint = "Nome da Tool equipada, ao lado direito da distancia. So aparece quando o jogador esta segurando algo.",
                Default = featureSettings.heldItem,
                Callback = function(B)
                    featureSettings.heldItem = B
                    if not B then
                        for B, u in pairs(featureSettings.bundles) do
                            if u.item then
                                u.item.Visible = false
                            end
                        end
                    end
                end,
            })
        )
        registerControl(
            "skeleton",
            G0:AddToggle({
                Text = "Esqueleto",
                Default = featureSettings.skeleton,
                Callback = function(B)
                    featureSettings.skeleton = B
                    if not B then
                        for B, u in pairs(featureSettings.bundles) do
                            for B, u in ipairs(u.lines) do
                                u.Visible = false
                            end
                        end
                    end
                end,
            })
        )
        registerControl(
            "highlight",
            G0:AddToggle({
                Text = "Highlight",
                Default = featureSettings.highlight,
                Callback = function(B)
                    featureSettings.highlight = B
                    if not B then
                        for B, u in pairs(featureSettings.bundles) do
                            if u.highlight then
                                u.highlight.Enabled = false
                            end
                        end
                    end
                end,
            })
        )
        registerControl(
            "tracer",
            G0:AddToggle({
                Text = "Tracer",
                Default = featureSettings.tracer,
                Callback = function(B)
                    featureSettings.tracer = B
                    if not B then
                        for B, u in pairs(featureSettings.bundles) do
                            if u.tracer then
                                u.tracer.Visible = false
                            end
                        end
                    end
                end,
            })
        )
        registerControl(
            "arrow",
            G0:AddToggle({
                Text = "Seta fora da tela",
                Default = featureSettings.arrow,
                Callback = function(B)
                    featureSettings.arrow = B
                    if not B then
                        for B, u in pairs(featureSettings.bundles) do
                            if u.arrow then
                                u.arrow.Visible = false
                            end
                        end
                    end
                end,
            })
        )
        registerControl(
            "teamTag",
            G0:AddToggle({
                Text = "Times",
                Default = featureSettings.teamTag,
                Callback = function(B)
                    featureSettings.teamTag = B
                    if not B then
                        for B, u in pairs(featureSettings.bundles) do
                            if u.team then
                                u.team.Visible = false
                            end
                        end
                    end
                end,
            })
        )
        registerControl(
            "maxDistance",
            G0:AddSlider({
                Text = "Distancia maxima",
                Min = espMinimumDistance,
                Max = espMaximumDistance,
                Default = featureSettings.maxDistance,
                Callback = function(B)
                    featureSettings.maxDistance = B
                end,
            })
        )
        local y0 = ((e0:AddTopTab({
            Name = "Silent Aim",
            Icon = "crosshair",
        })):AddColumn()):AddSection("Silent Aim")
        local P0
        P0 = registerControl(
            "silentAimEnabled",
            y0:AddToggle({
                Text = "Silent Aim (cabeca)",
                Default = featureSettings.silentAimEnabled,
                Callback = function(B)
                    featureSettings.silentAimEnabled = B
                    if B and not weaponRaycastController then
                        H0("sem GunSys")
                        featureSettings.silentAimEnabled = false
                        P0:Set(false)
                    end
                end,
            })
        )
        do
            local B = game:GetService("TweenService")
            local u = TweenInfo.new(0.22, Enum.EasingStyle.Quart, Enum.EasingDirection.Out)
            local k = TweenInfo.new(0.11, Enum.EasingStyle.Quad, Enum.EasingDirection.Out)
            local T = Instance.new("Frame")
            T.Name = "SilentAimAvancado"
            T.BackgroundTransparency = 1
            T.BorderSizePixel = 0
            T.ClipsDescendants = true
            T.Size = UDim2.new(1, 0, 0, 0)
            T.LayoutOrder = 2
            T.Parent = y0.Body
            local W = Instance.new("UIListLayout")
            W.SortOrder = Enum.SortOrder.LayoutOrder
            W.Padding = UDim.new(0, 0)
            W.Parent = T
            local l = Instance.new("ImageButton")
            l.Name = "Expandir"
            l.AnchorPoint = Vector2.new(1, 0.5)
            l.Position = UDim2.new(1, -46, 0.5, 0)
            l.Size = UDim2.fromOffset(16, 16)
            l.BackgroundTransparency = 1
            l.Image = "rbxassetid://7733723321"
            l.ImageColor3 = l0.Theme.TextMuted
            l.AutoButtonColor = false
            l.Parent = P0.Instance
            local function I(k)
                local l = UDim2.new(1, 0, 0, featureSettings.silentAimExpandido and W.AbsoluteContentSize.Y or 0)
                if k then
                    (B:Create(T, u, {
                        Size = l,
                    })):Play()
                else
                    T.Size = l
                end
            end
            trackConnection((W:GetPropertyChangedSignal("AbsoluteContentSize")):Connect(function()
                if featureSettings.silentAimExpandido then
                    I(true)
                end
            end))
            trackConnection(l.MouseButton1Click:Connect(function()
                featureSettings.silentAimExpandido = not featureSettings.silentAimExpandido
                I(true);
                (B:Create(l, k, {
                    ImageTransparency = 1,
                    Rotation = 90,
                })):Play()
                task.delay(0.11, function()
                    if not l.Parent then
                        return
                    end
                    l.Image = featureSettings.silentAimExpandido and "rbxassetid://7733720483"
                        or "rbxassetid://7733723321"
                    l.Rotation = -90
                    (B:Create(l, k, {
                        ImageTransparency = 0,
                        Rotation = 0,
                    })):Play()
                end)
            end));
            (registerControl(
                "silentAimHitChance",
                y0:AddSlider({
                    Text = "Taxa de acerto (%)",
                    Min = 0,
                    Max = 100,
                    Default = featureSettings.silentAimHitChance,
                    Callback = function(B)
                        featureSettings.silentAimHitChance = B
                    end,
                })
            )).Instance.Parent =
                T;
            (registerControl(
                "silentAimBodyPart",
                y0:AddDropdown({
                    Text = "Parte do corpo",
                    Options = {
                        "Cabeca",
                        "Tronco",
                        "Bracos",
                        "Pernas",
                    },
                    Default = featureSettings.silentAimBodyPart,
                    Callback = function(B)
                        featureSettings.silentAimBodyPart = B
                    end,
                })
            )).Instance.Parent =
                T
            I(false)
        end
        registerControl(
            "silentAimVisibleOnly",
            y0:AddToggle({
                Text = "Visibility Check",
                Default = featureSettings.silentAimVisibleOnly,
                Callback = function(B)
                    featureSettings.silentAimVisibleOnly = B
                end,
            })
        )
        registerControl(
            "friendCheck",
            y0:AddToggle({
                Text = "Friend Check",
                Default = featureSettings.friendCheck,
                Callback = function(B)
                    featureSettings.friendCheck = B
                end,
            })
        )
        registerControl(
            "adminCheck",
            y0:AddToggle({
                Text = "Admin / Staff Check",
                Default = featureSettings.adminCheck,
                Callback = function(B)
                    featureSettings.adminCheck = B
                end,
            })
        )
        registerControl(
            "silentAimFovVisible",
            y0:AddToggle({
                Text = "Mostrar FOV",
                Default = featureSettings.silentAimFovVisible,
                Callback = function(B)
                    featureSettings.silentAimFovVisible = B
                end,
            })
        )
        registerControl(
            "silentAimFov",
            y0:AddSlider({
                Text = "FOV",
                Min = 10,
                Max = 600,
                Default = featureSettings.silentAimFov,
                Callback = function(B)
                    featureSettings.silentAimFov = B
                end,
            })
        )
        registerControl(
            "silentAimDistance",
            y0:AddSlider({
                Text = "Distancia do alvo",
                Min = 50,
                Max = 1000,
                Default = featureSettings.silentAimDistance,
                Callback = function(B)
                    featureSettings.silentAimDistance = B
                end,
            })
        )
        local N0 = {
            {
                nome = "Banco",
                raiz = "Banco",
                pasta = "Malotes",
                peca = "Grana",
                unidade = "Malas",
            },
            {
                nome = "Joalheria",
                raiz = "Joalheria",
                pasta = "JoiasPraRoubar",
                peca = "Roubador",
                unidade = "Joias",
            },
            {
                nome = "Cassino",
                raiz = "Cassino",
                pasta = "Malotes",
                peca = "Grana",
                unidade = "Malas",
            },
        }
        local function g0(B)
            local u = WorkspaceService:FindFirstChild(B.raiz)
            local k = u and u:FindFirstChild(B.pasta)
            if not k then
                return 0
            end
            local T = 0
            for u, k in ipairs(k:GetChildren()) do
                if k.Name == B.peca then
                    local B = k:FindFirstChildWhichIsA("ProximityPrompt", true)
                    if B and B.Enabled then
                        T = T + 1
                    end
                end
            end
            return T
        end
        do
            local B = ((e0:AddTopTab({
                Name = "Armas",
                Icon = "sword",
            })):AddColumn()):AddSection("Arma equipada")
            local k = B:AddLabel("Nenhuma arma equipada.")
            local T = setmetatable({}, {
                __mode = "k",
            })
            local W = {
                "CameraShakingEnabled",
                "Recoil",
                "AngleX",
                "AngleY",
                "FireRate",
                "ReloadTime",
                "WalkSpeedRedutionEnabled",
                "WalkSpeedRedution",
                "VisualizerEnabled",
                "BulletMaterial",
                "BulletTransparency",
                "FadeTime",
                "BulletSize",
            }
            local l = {
                {
                    "Ciano",
                    Color3.fromRGB(0, 255, 255),
                },
                {
                    "Verde",
                    Color3.fromRGB(60, 255, 90),
                },
                {
                    "Rosa",
                    Color3.fromRGB(255, 60, 190),
                },
                {
                    "Roxo",
                    Color3.fromRGB(160, 70, 255),
                },
                {
                    "Vermelho",
                    Color3.fromRGB(255, 45, 45),
                },
                {
                    "Azul",
                    Color3.fromRGB(70, 130, 255),
                },
                {
                    "Laranja",
                    Color3.fromRGB(255, 145, 30),
                },
                {
                    "Branco",
                    Color3.fromRGB(255, 255, 255),
                },
                {
                    "Arco-iris",
                    nil,
                },
            }
            local Y = nil
            local function q()
                local B = localPlayer:FindFirstChild("OrpheusGunCFG")
                local u = B and B:FindFirstChild("CorBala")
                if u and Y == nil then
                    Y = u.Color
                end
                return u
            end
            local x = nil
            local function o()
                local B = localPlayer:FindFirstChild("PlayerScripts")
                local u = B and B:FindFirstChild("ArmaClient")
                local k = u and u:FindFirstChild("TrailBulletBlock")
                return k and k:FindFirstChild("BulletTrail")
            end
            local function i(B)
                local u = o()
                if not u or not u:IsA("Beam") then
                    return
                end
                if x == nil then
                    x = {
                        LightEmission = u.LightEmission,
                        LightInfluence = u.LightInfluence,
                        Brightness = u.Brightness,
                        Width0 = u.Width0,
                        Width1 = u.Width1,
                        Transparency = u.Transparency,
                    }
                end
                if B then
                    if u.LightEmission == 1 and u.Brightness == 6 then
                        return
                    end
                    u.LightEmission = 1
                    u.LightInfluence = 0
                    u.Brightness = 6
                    u.Width0, u.Width1 = 0.9, 0.9
                    u.Transparency = NumberSequence.new(0)
                elseif u.LightEmission ~= x.LightEmission then
                    for B, k in pairs(x) do
                        u[B] = k
                    end
                end
            end
            local function E()
                local B = q()
                i(featureSettings.armaTracer)
                if not featureSettings.armaTracer then
                    if B and Y then
                        B.Color = Y
                    end
                    return
                end
                if not B then
                    return
                end
                if featureSettings.armaTracerCor == "Arco-iris" then
                    B.Color = Color3.fromHSV((os.clock() * 0.25) % 1, 1, 1)
                    return
                end
                for u, k in ipairs(l) do
                    if k[1] == featureSettings.armaTracerCor and k[2] then
                        B.Color = k[2]
                        return
                    end
                end
            end
            local function J(B)
                E()
                if not featureSettings.armaTracer then
                    return
                end
                B.VisualizerEnabled = true
                B.BulletMaterial = Enum.Material.Neon
                B.BulletTransparency = 0
                B.FadeTime = 0.45
            end
            local function d()
                local B = localPlayer.Character
                local u = B and B:FindFirstChildOfClass("Tool")
                local k = u and u:FindFirstChild("Cfg")
                if not k or not k:IsA("ModuleScript") then
                    return nil, u
                end
                local T, W = pcall(require, k)
                if not T or type(W) ~= "table" or W.BaseDamage == nil then
                    return nil, u
                end
                return W, u
            end
            local function X(B)
                local u = T[B]
                if u then
                    return u
                end
                u = {}
                for k, T in ipairs(W) do
                    u[T] = B[T]
                end
                T[B] = u
                return u
            end
            local function s(B)
                local u = X(B)
                if featureSettings.armaNoRecoil then
                    B.CameraShakingEnabled = false
                    B.Recoil, B.AngleX, B.AngleY = 0, 0, 0
                else
                    B.CameraShakingEnabled = u.CameraShakingEnabled
                    B.Recoil, B.AngleX, B.AngleY = u.Recoil, u.AngleX, u.AngleY
                end
                B.ReloadTime = featureSettings.armaReloadRapido and 0 or u.ReloadTime
                B.FireRate = featureSettings.armaCadenciaCustom and 1 / math.max(1, featureSettings.armaTirosPorSegundo)
                    or u.FireRate
                if featureSettings.armaSemLentidao then
                    B.WalkSpeedRedutionEnabled = false
                    B.WalkSpeedRedution = 0
                else
                    B.WalkSpeedRedutionEnabled = u.WalkSpeedRedutionEnabled
                    B.WalkSpeedRedution = u.WalkSpeedRedution
                end
                if featureSettings.armaTracer then
                    J(B)
                else
                    B.VisualizerEnabled = u.VisualizerEnabled
                    B.BulletMaterial = u.BulletMaterial
                    B.BulletTransparency = u.BulletTransparency
                    B.FadeTime = u.FadeTime
                    B.BulletSize = u.BulletSize
                    J(B)
                end
            end
            local function D()
                pcall(i, false)
                local B = localPlayer:FindFirstChild("OrpheusGunCFG")
                B = B and B:FindFirstChild("CorBala")
                if B and Y then
                    pcall(function()
                        B.Color = Y
                    end)
                end
                for B, u in pairs(T) do
                    for u, k in pairs(u) do
                        pcall(function()
                            B[u] = k
                        end)
                    end
                end
            end
            runtimeState.restaurarArmas = D
            local H = getupvalues or (debug and debug.getupvalues)
            local R = setupvalue or (debug and debug.setupvalue)
            local m = {
                tool = nil,
                alvos = nil,
                proxima = 0,
            }
            local function w(B, u)
                if m.tool == B and m.alvos then
                    return m.alvos
                end
                if m.tool == B and os.clock() < m.proxima then
                    return nil
                end
                m = {
                    tool = B,
                    proxima = os.clock() + 3,
                }
                if not (getgc and (H and R)) then
                    return nil
                end
                local k = B:FindFirstChild("OrpheusGunServer")
                local T = k and k:FindFirstChild("Mag")
                local W = T and T.Value
                if not W or W <= 0 then
                    return nil
                end
                local l = {}
                for k, T in ipairs(getgc(false)) do
                    if type(T) == "function" then
                        local k = debug.info(T, "n")
                        if k == "Reload" or k == "HandleFire" or k == "UpdateGUI" then
                            local k = H(T)
                            local I = false
                            for k, T in pairs(k) do
                                if T == u or T == B then
                                    I = true
                                    break
                                end
                            end
                            if I then
                                for B, u in pairs(k) do
                                    if u == W then
                                        l[#l + 1] = {
                                            fn = T,
                                            idx = B,
                                        }
                                    end
                                end
                            end
                        end
                    end
                end
                if #l == 0 then
                    return nil
                end
                m.alvos = l
                return l
            end
            local function v(B, u)
                local k = w(B, u)
                if not k then
                    return false
                end
                for B, k in ipairs(k) do
                    local T = H(k.fn)
                    if T[k.idx] ~= u.AmmoPerMag then
                        R(k.fn, k.idx, u.AmmoPerMag)
                    end
                end
                return true
            end
            registerControl(
                "armaNoRecoil",
                B:AddToggle({
                    Text = "Sem recuo",
                    Hint = "Zera Recoil/AngleX/AngleY e o tremor de camera da arma equipada.",
                    Default = featureSettings.armaNoRecoil,
                    Callback = function(B)
                        featureSettings.armaNoRecoil = B
                    end,
                })
            )
            registerControl(
                "armaMunicaoInfinita",
                B:AddToggle({
                    Text = "Municao infinita",
                    Hint = "Mantem o pente cheio pelo upvalue da arma. Precisa de getgc/setupvalue no executor.",
                    Default = featureSettings.armaMunicaoInfinita,
                    Callback = function(B)
                        featureSettings.armaMunicaoInfinita = B
                    end,
                })
            )
            registerControl(
                "armaReloadRapido",
                B:AddToggle({
                    Text = "Recarga instantanea",
                    Hint = "ReloadTime = 0. A animacao ainda toca, mas a arma ja volta a atirar.",
                    Default = featureSettings.armaReloadRapido,
                    Callback = function(B)
                        featureSettings.armaReloadRapido = B
                    end,
                })
            )
            registerControl(
                "armaSemLentidao",
                B:AddToggle({
                    Text = "Sem lentidao com arma",
                    Hint = "Desliga a reducao de WalkSpeed que algumas armas aplicam.",
                    Default = featureSettings.armaSemLentidao,
                    Callback = function(B)
                        featureSettings.armaSemLentidao = B
                    end,
                })
            )
            registerControl(
                "armaCadenciaCustom",
                B:AddToggle({
                    Text = "Cadencia custom",
                    Hint = "Sobrescreve o FireRate da arma. Cada tiro dispara remotes de som e efeito -- cadencia muito alta e barulho na rede.",
                    Default = featureSettings.armaCadenciaCustom,
                    Callback = function(B)
                        featureSettings.armaCadenciaCustom = B
                    end,
                })
            )
            registerControl(
                "armaTirosPorSegundo",
                B:AddSlider({
                    Text = "Tiros por segundo",
                    Min = 1,
                    Max = 30,
                    Step = 1,
                    Default = featureSettings.armaTirosPorSegundo,
                    Callback = function(B)
                        featureSettings.armaTirosPorSegundo = math.clamp(math.floor(B), 1, 30)
                    end,
                })
            )
            registerControl(
                "armaTracer",
                B:AddToggle({
                    Text = "Tracer neon",
                    Hint = "Bala em material Neon, sem transparencia e com rastro mais longo. O jogo replica o tracer pelo BulletRemote, entao os outros jogadores tambem veem a cor.",
                    Default = featureSettings.armaTracer,
                    Callback = function(B)
                        featureSettings.armaTracer = B
                    end,
                })
            )
            do
                local u = {}
                for B, k in ipairs(l) do
                    u[#u + 1] = k[1]
                end
                registerControl(
                    "armaTracerCor",
                    B:AddDropdown({
                        Text = "Cor do tracer",
                        Options = u,
                        Default = featureSettings.armaTracerCor,
                        Callback = function(B)
                            featureSettings.armaTracerCor = B
                        end,
                    })
                )
            end
            local e = 0
            trackConnection(RunService.Heartbeat:Connect(function()
                if runtimeState.destroyed then
                    return
                end
                if os.clock() - e < 0.1 then
                    return
                end
                e = os.clock()
                local B, u = d()
                if not B then
                    pcall(E)
                    k:SetText(u and ("Sem Cfg: " .. u.Name) or "Nenhuma arma equipada.")
                    return
                end
                pcall(s, B)
                local T = ""
                if featureSettings.armaMunicaoInfinita and not v(u, B) then
                    T = " | municao: procurando..."
                end
                k:SetText(
                    string.format(
                        "%s  -  dano %s | recuo %s | %.2fs/tiro%s",
                        u.Name,
                        tostring(B.BaseDamage),
                        tostring(B.Recoil),
                        B.FireRate,
                        T
                    )
                )
            end))
        end
        local A0 = l0:AddSideTab({
            Name = "Diversos",
            Group = "Geral",
            Icon = "globe",
        })
        local n0 = A0:AddTopTab({
            Name = "Diversos",
            Icon = "wrench",
        })
        local K0 = n0:AddColumn()
        local a0 = K0:AddSection("Diversos")
        local c0 = (n0:AddColumn()):AddSection("Coletar Itens")
        registerControl(
            "perfectLockpick",
            a0:AddToggle({
                Text = "Perfect Lock Pick",
                Default = featureSettings.perfectLockpick,
                Callback = function(B)
                    featureSettings.perfectLockpick = B
                end,
            })
        )
        registerControl(
            "bestWeaponEnabled",
            a0:AddToggle({
                Text = "Equipar melhor arma.",
                Default = featureSettings.bestWeaponEnabled,
                Callback = function(B)
                    setBestWeaponEnabled(B)
                end,
            })
        )
        do
            local B = {}
            local k = nil
            local T
            local W = Instance.new("Frame")
            W.Name = "ColetaItensHUD"
            W.Position = UDim2.new(1, -328, 0, 96)
            W.Size = UDim2.fromOffset(310, 300)
            W.BackgroundColor3 = l0.Theme.Panel
            W.BorderSizePixel = 0
            W.Active = true
            W.ZIndex = 50
            W.Visible = featureSettings.coletaHudEnabled
            W.Parent = l0.ToggleButton.Parent
            local l = Instance.new("UICorner")
            l.CornerRadius = UDim.new(0, 10)
            l.Parent = W
            local I = Instance.new("UIStroke")
            I.Color = l0.Theme.Border
            I.Thickness = 1
            I.Parent = W
            local Y = Instance.new("TextLabel")
            Y.BackgroundTransparency = 1
            Y.Position = UDim2.fromOffset(12, 8)
            Y.Size = UDim2.new(1, -24, 0, 24)
            Y.Font = Enum.Font.GothamMedium
            Y.Text = "Itens no chao"
            Y.TextColor3 = l0.Theme.Text
            Y.TextSize = 14
            Y.TextXAlignment = Enum.TextXAlignment.Left
            Y.Active = true
            Y.ZIndex = 51
            Y.Parent = W
            local q = Instance.new("ScrollingFrame")
            q.BackgroundTransparency = 1
            q.BorderSizePixel = 0
            q.Position = UDim2.fromOffset(10, 38)
            q.Size = UDim2.new(1, -20, 1, -48)
            q.ScrollBarThickness = 3
            q.ScrollBarImageColor3 = l0.Theme.TextMuted
            q.AutomaticCanvasSize = Enum.AutomaticSize.Y
            q.CanvasSize = UDim2.new()
            q.ZIndex = 51
            q.Parent = W
            local x = Instance.new("UIListLayout")
            x.Padding = UDim.new(0, 5)
            x.SortOrder = Enum.SortOrder.LayoutOrder
            x.Parent = q
            local o = styleHudFrame(W, {
                Titulo = Y,
                Divisor = 32,
            })
            makeDraggable(W, Y)
            registerControl(
                "coletaHudEnabled",
                c0:AddToggle({
                    Text = "Mostrar HUD de itens",
                    Hint = "Lista flutuante dos itens dropados com teleporte rapido.",
                    Default = featureSettings.coletaHudEnabled,
                    Callback = function(B)
                        featureSettings.coletaHudEnabled = B
                        o:SetVisible(B)
                    end,
                })
            )
            T = c0:AddDropdown({
                Text = "Item mais proximo",
                Options = {
                    "(procurando itens...)",
                },
                Callback = function(u)
                    local T = B[u]
                    k = T and T.item or nil
                end,
            })
            local function i()
                local u = listDroppedItems()
                table.clear(B)
                local W, l = {}, nil
                for u, T in ipairs(u) do
                    if u > 20 then
                        break
                    end
                    local I = string.format("%d. %s  -  %dm", u, T.nome, math.floor(T.distancia + 0.5))
                    W[#W + 1] = I
                    B[I] = T
                    if T.item == k then
                        l = I
                    end
                end
                if #W == 0 then
                    W[1] = "(nenhum item por perto)"
                end
                T:SetOptions(W, true)
                if l then
                    T:Set(l)
                else
                    k = nil
                end
                for B, u in ipairs(q:GetChildren()) do
                    if u:IsA("GuiObject") then
                        u:Destroy()
                    end
                end
                if #u == 0 then
                    local B = Instance.new("TextLabel")
                    B.BackgroundTransparency = 1
                    B.Size = UDim2.new(1, 0, 0, 28)
                    B.Font = Enum.Font.Gotham
                    B.Text = "Nenhum item dropado"
                    B.TextColor3 = l0.Theme.TextMuted
                    B.TextSize = 13
                    B.ZIndex = 52
                    B.Parent = q
                else
                    for B, u in ipairs(u) do
                        if B > 20 then
                            break
                        end
                        local k = Instance.new("Frame")
                        k.Name = "Item_" .. B
                        k.BackgroundColor3 = l0.Theme.Card
                        k.BorderSizePixel = 0
                        k.Size = UDim2.new(1, -4, 0, 34)
                        k.LayoutOrder = B
                        k.ZIndex = 52
                        k.Parent = q
                        local T = Instance.new("UICorner")
                        T.CornerRadius = UDim.new(0, 6)
                        T.Parent = k
                        local W = Instance.new("TextLabel")
                        W.BackgroundTransparency = 1
                        W.Position = UDim2.fromOffset(9, 0)
                        W.Size = UDim2.new(1, -62, 1, 0)
                        W.Font = Enum.Font.Gotham
                        W.Text = string.format("%s  -  %dm", u.nome, math.floor(u.distancia + 0.5))
                        W.TextColor3 = l0.Theme.Text
                        W.TextSize = 12
                        W.TextTruncate = Enum.TextTruncate.AtEnd
                        W.TextXAlignment = Enum.TextXAlignment.Left
                        W.ZIndex = 53
                        W.Parent = k
                        local l = Instance.new("TextButton")
                        l.AnchorPoint = Vector2.new(1, 0.5)
                        l.Position = UDim2.new(1, -5, 0.5, 0)
                        l.Size = UDim2.fromOffset(42, 24)
                        l.BackgroundColor3 = l0.Theme.Accent
                        l.BorderSizePixel = 0
                        l.Font = Enum.Font.GothamMedium
                        l.Text = "TP"
                        l.TextColor3 = l0.Theme.Background
                        l.TextSize = 12
                        l.ZIndex = 53
                        l.Parent = k
                        local I = Instance.new("UICorner")
                        I.CornerRadius = UDim.new(0, 5)
                        I.Parent = l
                        l.Activated:Connect(function()
                            if not u.item.Parent then
                                return
                            end
                            task.spawn(function()
                                teleportToItem(u)
                                pcall(i)
                            end)
                        end)
                    end
                end
            end
            c0:AddButton({
                Text = "Tp",
                Callback = function()
                    local u = B[T:Get()]
                    if not u or not u.item.Parent then
                        i()
                        l0:Notify({
                            Title = "Coletar Itens",
                            Text = "Escolha um item da lista primeiro.",
                        })
                        return
                    end
                    task.spawn(function()
                        local B = teleportToItem(u)
                        i()
                        l0:Notify({
                            Title = "Coletar Itens",
                            Text = B and ("Coletado: " .. u.nome) or ("Nao alcancou: " .. u.nome),
                        })
                    end)
                end,
            })
            registerControl(
                "itemTeleportSpeed",
                c0:AddSlider({
                    Text = "Velocidade do TP (studs/s)",
                    Hint = "Ate "
                        .. (
                            itemTeleportDefaultSpeed
                            .. " studs/s; reduz automaticamente para 36 quando a condicao fica instavel."
                        ),
                    Min = 20,
                    Max = itemTeleportSliderMaximum,
                    Step = 5,
                    Default = featureSettings.itemTeleportSpeed,
                    Callback = function(B)
                        featureSettings.itemTeleportSpeed = B
                    end,
                })
            )
            local E = 0
            pcall(i)
            trackConnection(RunService.Heartbeat:Connect(function()
                if runtimeState.destroyed then
                    return
                end
                if os.clock() - E < 2 then
                    return
                end
                E = os.clock()
                pcall(i)
            end))
        end
        do
            local k = K0:AddSection("Roubos")
            local T = "#50DC64"
            local W = "#DC5050"
            local l = "#60A5FA"
            local I = Color3.fromHex(T)
            local Y = Color3.fromHex(W)
            local q = Color3.fromHex(l)
            local function x(B, u)
                local k = u > 0
                return string.format(
                    '%s: <font color="%s">%s</font> - <font color="%s">%d %s</font>',
                    B.nome,
                    k and T or W,
                    k and "Aberto" or "Fechado",
                    l,
                    u,
                    B.unidade
                )
            end
            local o = Instance.new("Frame")
            o.Name = "RoubosHUD"
            o.Position = UDim2.new(0, 18, 0, 96)
            o.AutomaticSize = Enum.AutomaticSize.Y
            o.Size = UDim2.fromOffset(250, 0)
            o.BackgroundColor3 = l0.Theme.Panel
            o.BorderSizePixel = 0
            o.Active = true
            o.ZIndex = 50
            o.Visible = featureSettings.roubosHudEnabled
            o.Parent = l0.ToggleButton.Parent
            local i = Instance.new("UICorner")
            i.CornerRadius = UDim.new(0, 10)
            i.Parent = o
            local E = Instance.new("UIStroke")
            E.Color = l0.Theme.Border
            E.Thickness = 1
            E.Parent = o
            local J = Instance.new("UIPadding")
            J.PaddingTop = UDim.new(0, 8)
            J.PaddingBottom = UDim.new(0, 8)
            J.PaddingLeft = UDim.new(0, 12)
            J.PaddingRight = UDim.new(0, 12)
            J.Parent = o
            local d = Instance.new("UIListLayout")
            d.FillDirection = Enum.FillDirection.Vertical
            d.SortOrder = Enum.SortOrder.LayoutOrder
            d.Padding = UDim.new(0, 3)
            d.Parent = o
            makeDraggable(o)
            local X = Instance.new("Frame")
            X.Name = "Cabecalho"
            X.BackgroundTransparency = 1
            X.Size = UDim2.new(1, 0, 0, 18)
            X.LayoutOrder = 0
            X.ZIndex = 51
            X.Parent = o
            local D = Instance.new("TextLabel")
            D.BackgroundTransparency = 1
            D.AutomaticSize = Enum.AutomaticSize.X
            D.Size = UDim2.new(0, 0, 1, 0)
            D.Font = Enum.Font.GothamMedium
            D.TextSize = 13
            D.TextXAlignment = Enum.TextXAlignment.Left
            D.TextColor3 = l0.Theme.TextMuted
            D.Text = "Roubos"
            D.ZIndex = 51
            D.Parent = X
            local H = styleHudFrame(o, {
                Titulo = D,
            })
            local R = Instance.new("TextLabel")
            R.Name = "Policiais"
            R.BackgroundTransparency = 1
            R.AnchorPoint = Vector2.new(1, 0)
            R.Position = UDim2.new(1, 0, 0, 0)
            R.AutomaticSize = Enum.AutomaticSize.X
            R.Size = UDim2.new(0, 0, 1, 0)
            R.Font = Enum.Font.Gotham
            R.TextSize = 13
            R.RichText = true
            R.TextXAlignment = Enum.TextXAlignment.Right
            R.TextColor3 = l0.Theme.TextMuted
            R.Text = "Policiais: 0"
            R.ZIndex = 51
            R.Parent = X
            local function m(B, u, k, T, W)
                local l = Instance.new("TextLabel")
                l.BackgroundTransparency = 1
                l.AnchorPoint = Vector2.new(u, 0)
                l.Position = UDim2.new(k, 0, 0, 0)
                l.AutomaticSize = Enum.AutomaticSize.X
                l.Size = UDim2.new(0, 0, 1, 0)
                l.Font = Enum.Font.Gotham
                l.TextSize = 13
                l.TextXAlignment = T
                l.TextColor3 = W
                l.Text = ""
                l.ZIndex = 51
                l.Parent = B
                return l
            end
            local v = {}
            for B, u in ipairs(N0) do
                local T = Instance.new("Frame")
                T.Name = "Linha_" .. u.nome
                T.BackgroundTransparency = 1
                T.Size = UDim2.new(1, 0, 0, 18)
                T.LayoutOrder = B
                T.ZIndex = 51
                T.Parent = o
                local W = k:AddLabel(u.nome)
                W.Instance.RichText = true
                W.Instance.TextColor3 = Color3.new(1, 1, 1)
                v[B] = {
                    box = W,
                    nome = m(T, 0, 0, Enum.TextXAlignment.Left, Color3.new(1, 1, 1)),
                    estado = m(T, 0.5, 0.5, Enum.TextXAlignment.Center, Y),
                    quantidade = m(T, 1, 1, Enum.TextXAlignment.Right, q),
                }
                v[B].nome.Text = u.nome
            end
            local function e(B)
                return B:find("Pol") ~= nil or B:find("GCM") ~= nil or B:find("GATE") ~= nil
            end
            local function G()
                local u = 0
                for B, k in ipairs(PlayersService:GetPlayers()) do
                    local T = k.Team
                    if T and e(T.Name) then
                        u = u + 1
                    end
                end
                return u
            end
            local U
            local function f()
                for B, u in ipairs(N0) do
                    local k = g0(u)
                    local T = k > 0
                    local W = v[B]
                    W.estado.Text = T and "Aberto" or "Fechado"
                    W.estado.TextColor3 = T and I or Y
                    W.quantidade.Text = string.format("%d %s", k, u.unidade)
                    W.box.Instance.Text = x(u, k)
                end
                local B = string.format('Policiais: <font color="%s">%d</font>', l, G())
                R.Text = B
                if U then
                    U.Instance.Text = B
                end
            end
            registerControl(
                "roubosHudEnabled",
                k:AddToggle({
                    Text = "Mostrar HUD de roubos",
                    Hint = "Painel flutuante na tela. A lista aqui na caixa atualiza de qualquer jeito.",
                    Default = featureSettings.roubosHudEnabled,
                    Callback = function(B)
                        featureSettings.roubosHudEnabled = B
                        H:SetVisible(B)
                    end,
                })
            )
            U = k:AddLabel("Policiais: 0")
            U.Instance.RichText = true
            U.Instance.TextColor3 = Color3.new(1, 1, 1)
            U.Instance.TextXAlignment = Enum.TextXAlignment.Center
            U.Instance.TextYAlignment = Enum.TextYAlignment.Center
            f()
            local p = 0
            trackConnection(RunService.Heartbeat:Connect(function()
                if runtimeState.destroyed then
                    return
                end
                if os.clock() - p < 1 then
                    return
                end
                p = os.clock()
                pcall(f)
            end))
        end
        do
            local B = ((A0:AddTopTab({
                Name = "Teleport",
                Icon = "map-pin",
            })):AddColumn()):AddSection("Locais")
            local u = WorkspaceService:FindFirstChild("GPS")
            u = u and u:FindFirstChild("Locais")
            local k = {}
            local T = Instance.new("Frame")
            T.Name = "TeleportLocaisHUD"
            T.Position = UDim2.new(1, -648, 0, 96)
            T.Size = UDim2.fromOffset(310, 300)
            T.BackgroundColor3 = l0.Theme.Panel
            T.BorderSizePixel = 0
            T.Active = true
            T.ZIndex = 50
            T.Visible = featureSettings.teleportHudEnabled
            T.Parent = l0.ToggleButton.Parent
            local W = Instance.new("UICorner")
            W.CornerRadius = UDim.new(0, 10)
            W.Parent = T
            local I = Instance.new("UIStroke")
            I.Color = l0.Theme.Border
            I.Thickness = 1
            I.Parent = T
            local Y = Instance.new("TextLabel")
            Y.BackgroundTransparency = 1
            Y.Position = UDim2.fromOffset(12, 8)
            Y.Size = UDim2.new(1, -24, 0, 24)
            Y.Font = Enum.Font.GothamMedium
            Y.Text = "Teleport - Locais"
            Y.TextColor3 = l0.Theme.Text
            Y.TextSize = 14
            Y.TextXAlignment = Enum.TextXAlignment.Left
            Y.Active = true
            Y.ZIndex = 51
            Y.Parent = T
            local q = Instance.new("ScrollingFrame")
            q.BackgroundTransparency = 1
            q.BorderSizePixel = 0
            q.Position = UDim2.fromOffset(10, 38)
            q.Size = UDim2.new(1, -20, 1, -48)
            q.ScrollBarThickness = 3
            q.ScrollBarImageColor3 = l0.Theme.TextMuted
            q.AutomaticCanvasSize = Enum.AutomaticSize.Y
            q.CanvasSize = UDim2.new()
            q.ZIndex = 51
            q.Parent = T
            local x = Instance.new("UIListLayout")
            x.Padding = UDim.new(0, 5)
            x.SortOrder = Enum.SortOrder.LayoutOrder
            x.Parent = q
            local o = styleHudFrame(T, {
                Titulo = Y,
                Divisor = 32,
            })
            makeDraggable(T, Y)
            registerControl(
                "teleportHudEnabled",
                B:AddToggle({
                    Text = "Mostrar HUD de teleports",
                    Hint = "Lista flutuante dos locais do GPS com teleporte rapido.",
                    Default = featureSettings.teleportHudEnabled,
                    Callback = function(B)
                        featureSettings.teleportHudEnabled = B
                        o:SetVisible(B)
                    end,
                })
            )
            if u then
                for B, u in ipairs(u:GetChildren()) do
                    local T = u:FindFirstChildWhichIsA("BillboardGui", true)
                    local W = u:IsA("BasePart") and u or u:FindFirstChildWhichIsA("BasePart", true)
                    if T and W then
                        k[#k + 1] = {
                            nome = u.Name,
                            posicao = W.Position,
                        }
                    end
                end
            end
            table.sort(k, function(B, u)
                return string.lower(B.nome) < string.lower(u.nome)
            end)
            local function i(B)
                local u = getRootPart()
                if not u then
                    H0("Personagem ainda nao carregou.")
                    return
                end
                u.CFrame = CFrame.new(B + Vector3.new(0, 3, 0)) * u.CFrame.Rotation
                u.AssemblyLinearVelocity = Vector3.zero
                u.AssemblyAngularVelocity = Vector3.zero
            end
            for u, k in ipairs(k) do
                local T, W = k.nome, k.posicao
                B:AddButton({
                    Text = T,
                    Hint = string.format("Teleportar para %s", T),
                    Callback = function()
                        i(W)
                    end,
                })
                local l = Instance.new("Frame")
                l.Name = "Local_" .. T
                l.BackgroundColor3 = l0.Theme.Card
                l.BorderSizePixel = 0
                l.Size = UDim2.new(1, -4, 0, 34)
                l.LayoutOrder = u
                l.ZIndex = 52
                l.Parent = q
                local I = Instance.new("UICorner")
                I.CornerRadius = UDim.new(0, 6)
                I.Parent = l
                local Y = Instance.new("TextLabel")
                Y.BackgroundTransparency = 1
                Y.Position = UDim2.fromOffset(9, 0)
                Y.Size = UDim2.new(1, -62, 1, 0)
                Y.Font = Enum.Font.Gotham
                Y.Text = T
                Y.TextColor3 = l0.Theme.Text
                Y.TextSize = 12
                Y.TextTruncate = Enum.TextTruncate.AtEnd
                Y.TextXAlignment = Enum.TextXAlignment.Left
                Y.ZIndex = 53
                Y.Parent = l
                local x = Instance.new("TextButton")
                x.AnchorPoint = Vector2.new(1, 0.5)
                x.Position = UDim2.new(1, -5, 0.5, 0)
                x.Size = UDim2.fromOffset(42, 24)
                x.BackgroundColor3 = l0.Theme.Accent
                x.BorderSizePixel = 0
                x.Font = Enum.Font.GothamMedium
                x.Text = "TP"
                x.TextColor3 = l0.Theme.Background
                x.TextSize = 12
                x.ZIndex = 53
                x.Parent = l
                local o = Instance.new("UICorner")
                o.CornerRadius = UDim.new(0, 5)
                o.Parent = x
                x.Activated:Connect(function()
                    i(W)
                end)
            end
            if #k == 0 then
                B:AddLabel("Nenhum local do GPS encontrado.")
            end
        end
        do
            local k = ((A0:AddTopTab({
                Name = "Tp Players",
                Icon = "person-teleport",
            })):AddColumn()):AddSection("Players")
            local W = k.Body
            local l = k:AddLabel("Nenhum outro jogador no servidor.")
            local Y = {}
            local function q(B)
                local u = getRootPart()
                if not u then
                    H0("Personagem ainda nao carregou.")
                    return
                end
                local k = B.Character
                local T = k and k:FindFirstChild("HumanoidRootPart")
                if not T then
                    H0(B.Name .. " nao tem personagem no mundo.")
                    return
                end
                u.CFrame = CFrame.new(T.Position + Vector3.new(0, 3, 0)) * u.CFrame.Rotation
                u.AssemblyLinearVelocity = Vector3.zero
                u.AssemblyAngularVelocity = Vector3.zero
            end
            local x = W.ZIndex + 8
            local function o(B)
                local u = Instance.new("Frame")
                u.Name = "Player_" .. B.Name
                u.BackgroundTransparency = 1
                u.BorderSizePixel = 0
                u.Size = UDim2.new(1, 0, 0, 44)
                u.ZIndex = x
                u.Parent = W
                local k = Instance.new("Frame")
                k.AnchorPoint = Vector2.new(0.5, 1)
                k.BackgroundColor3 = Color3.fromRGB(45, 48, 58)
                k.BackgroundTransparency = 0.65
                k.BorderSizePixel = 0
                k.Position = UDim2.new(0.5, 0, 1, 0)
                k.Size = UDim2.new(1, -20, 0, 1)
                k.ZIndex = x + 3
                k.Parent = u
                local l = Instance.new("ImageLabel")
                l.AnchorPoint = Vector2.new(0, 0.5)
                l.BackgroundColor3 = l0.Theme.Card
                l.BorderSizePixel = 0
                l.Position = UDim2.new(0, 11, 0.5, 0)
                l.Size = UDim2.fromOffset(30, 30)
                l.Image = string.format("rbxthumb://type=AvatarHeadShot&id=%d&w=48&h=48", B.UserId)
                l.ZIndex = x + 1
                l.Parent = u
                local I = Instance.new("UICorner")
                I.CornerRadius = UDim.new(1, 0)
                I.Parent = l
                local Y = Instance.new("TextLabel")
                Y.BackgroundTransparency = 1
                Y.Position = UDim2.new(0, 50, 0, 7)
                Y.Size = UDim2.new(1, -120, 0, 15)
                Y.Font = Enum.Font.GothamMedium
                Y.Text = B.DisplayName ~= "" and B.DisplayName or B.Name
                Y.TextColor3 = l0.Theme.Text
                Y.TextSize = 13
                Y.TextTransparency = 0.35
                Y.TextTruncate = Enum.TextTruncate.AtEnd
                Y.TextXAlignment = Enum.TextXAlignment.Left
                Y.ZIndex = x + 1
                Y.Parent = u
                local o = Instance.new("TextLabel")
                o.BackgroundTransparency = 1
                o.Position = UDim2.new(0, 50, 0, 23)
                o.Size = UDim2.new(1, -120, 0, 14)
                o.Font = Enum.Font.Gotham
                o.RichText = true
                o.Text = "@" .. B.Name
                o.TextColor3 = l0.Theme.TextMuted
                o.TextSize = 11
                o.TextTruncate = Enum.TextTruncate.AtEnd
                o.TextXAlignment = Enum.TextXAlignment.Left
                o.ZIndex = x + 1
                o.Parent = u
                local i = Instance.new("TextButton")
                i.AnchorPoint = Vector2.new(0.5, 0.5)
                i.Position = UDim2.new(1, -34, 0.5, 0)
                i.Size = UDim2.fromOffset(46, 22)
                i.AutoButtonColor = false
                i.BackgroundColor3 = l0.Theme.Accent
                i.BorderSizePixel = 0
                i.Font = Enum.Font.GothamMedium
                i.Text = "TP"
                i.TextColor3 = l0.Theme.Background
                i.TextSize = 12
                i.ZIndex = x + 2
                i.Parent = u
                local E = Instance.new("UICorner")
                E.CornerRadius = UDim.new(0, 5)
                E.Parent = i
                local L = Instance.new("ImageLabel")
                L.BackgroundTransparency = 1
                L.AnchorPoint = Vector2.new(0.5, 0.5)
                L.Position = i.Position
                L.Size = UDim2.fromOffset(88, 56)
                L.Image = "rbxassetid://8992230677"
                L.ImageColor3 = l0.Theme.Accent
                L.ImageTransparency = 0.55
                L.ZIndex = x + 1
                L.Parent = u
                local Z = TweenInfo.new(0.12, Enum.EasingStyle.Quad, Enum.EasingDirection.Out)
                local function J(B, u, k, W)
                    (TweenService:Create(i, Z, {
                        Size = UDim2.fromOffset(B, u),
                        TextSize = k,
                    })):Play();
                    (TweenService:Create(L, Z, {
                        Size = UDim2.fromOffset(B + 42, u + 34),
                        ImageTransparency = W,
                    })):Play()
                end
                trackConnection(i.MouseEnter:Connect(function()
                    J(56, 27, 15, 0.25)
                end))
                trackConnection(i.MouseLeave:Connect(function()
                    J(46, 22, 12, 0.55)
                end))
                trackConnection(i.MouseButton1Down:Connect(function()
                    J(51, 24, 13, 0.1)
                end))
                trackConnection(i.MouseButton1Up:Connect(function()
                    J(56, 27, 15, 0.25)
                end))
                trackConnection(i.Activated:Connect(function()
                    q(B)
                end))
                return {
                    Root = u,
                    Nome = Y,
                    Sub = o,
                }
            end
            local function i(B)
                local u = getRootPart()
                local k = B.Character
                local T = k and k:FindFirstChild("HumanoidRootPart")
                if not u or not T then
                    return nil
                end
                return (T.Position - u.Position).Magnitude
            end
            local function E()
                local u = {}
                for B, k in ipairs(PlayersService:GetPlayers()) do
                    if k ~= localPlayer then
                        u[k] = true
                        local B = Y[k]
                        if not B then
                            B = o(k)
                            Y[k] = B
                        end
                        local T = i(k)
                        B.Sub.Text = T and string.format("@%s  -  %dm", k.Name, math.floor(T + 0.5))
                            or ("@" .. (k.Name .. '  -  <font color="#e05252">sem personagem</font>'))
                        B.Root.LayoutOrder = T and math.clamp(math.floor(T), 0, 99998) or 99999
                    end
                end
                for B, k in pairs(Y) do
                    if not u[B] then
                        k.Root:Destroy()
                        Y[B] = nil
                    end
                end
                l.Native.Root.Visible = next(Y) == nil
            end
            E()
            local Z = 0
            trackConnection(RunService.Heartbeat:Connect(function()
                if runtimeState.destroyed then
                    return
                end
                if os.clock() - Z < 0.5 then
                    return
                end
                Z = os.clock()
                pcall(E)
            end))
        end
        local V0, S0
        do
            local B = A0:AddTopTab({
                Name = "Trabalho e compras",
                Icon = "hammer",
            })
            V0 = (B:AddColumn()):AddSection("Empregos")
            S0 = B:AddColumn()
            local u = S0:AddSection("GCM")
            local function k(B, u)
                local k = (game:GetService("ReplicatedStorage")):FindFirstChild("Mercadinho")
                local T = k and k:FindFirstChild(B)
                if T then
                    pcall(function()
                        T:FireServer(u)
                    end)
                end
            end
            local function T(B)
                local u = localPlayer:FindFirstChild("Inv")
                if u and u:FindFirstChild(B) then
                    return true
                end
                for u, k in ipairs({
                    localPlayer:FindFirstChild("Backpack"),
                    localPlayer.Character,
                }) do
                    if k and k:FindFirstChild(B) then
                        return true
                    end
                end
                return false
            end
            u:AddButton({
                Text = "Equipar farda de GCM",
                Callback = function()
                    k("FardasRemote", "GCM")
                end,
            })
            local W = {}
            for B, l in ipairs({
                "Algemas",
                "GlockPolicial",
                "Radin",
                "G3Policial",
                "FuzilPolicial",
                "Tablet",
            }) do
                local I = u:AddLabel("<b>" .. (l .. "</b>: Você já possui esse item"))
                I.Instance.RichText = true
                local Y
                Y = u:AddButton({
                    Text = "Equipar " .. l,
                    Callback = function()
                        k("EquiparArmamento", l)
                        Y.Instance.Visible = false
                        I.Instance.Visible = true
                    end,
                })
                local function q()
                    local B = T(l)
                    I.Instance.Visible = B
                    Y.Instance.Visible = not B
                end
                q()
                table.insert(W, q)
            end
            local function l()
                for B, u in ipairs(W) do
                    pcall(u)
                end
            end
            local function Y()
                return localPlayer.Team ~= nil and localPlayer.Team.Name == "GCM"
            end
            u.Container.Visible = Y()
            trackConnection((localPlayer:GetPropertyChangedSignal("Team")):Connect(function()
                u.Container.Visible = Y()
                l()
            end))
            trackConnection((u.Container:GetPropertyChangedSignal("Visible")):Connect(function()
                if u.Container.Visible ~= Y() then
                    u.Container.Visible = Y()
                end
            end))
            local q = localPlayer:FindFirstChild("Inv")
            if q then
                trackConnection(q.ChildAdded:Connect(l))
                trackConnection(q.ChildRemoved:Connect(l))
            end
        end
        local r0 = V0:AddLabel("Emprego atual: " .. (d() or "nenhum"))
        local t0 = {}
        local function h0(B, u, k)
            return string.format(
                "%s   ·   Nv %d   ·   $%s%s",
                B.nome,
                B.level,
                D(B.salario),
                u and "   (Atual)" or (k and "   (Nv insuf.)" or "")
            )
        end
        local function j0()
            local B, u = d(), X()
            for k, T in ipairs(t0) do
                local W = B == T.emprego.nome
                T.btn.Label.Text = h0(T.emprego, W, u < T.emprego.level)
                T.btn.Label.TextColor3 = W and l0.Theme.Text or l0.Theme.TextMuted
            end
        end
        local Bo = false
        local function uo(B)
            if Bo then
                return
            end
            if d() == B.nome then
                r0.Instance.Text = "Você já trabalha como " .. B.nome
                return
            end
            local u = (game:GetService("ReplicatedStorage")):FindFirstChild("Mercadinho")
            local k = u and u:FindFirstChild("PrefRemote")
            if not k or not k:IsA("RemoteEvent") then
                r0.Instance.Text = "PrefRemote não encontrado"
                return
            end
            Bo = true
            r0.Instance.Text = "Trocando para " .. (B.nome .. "...")
            task.spawn(function()
                k:FireServer(B.nome, B.level, true)
                local u = os.clock() + i
                repeat
                    task.wait(0.2)
                until runtimeState.destroyed or d() == B.nome or os.clock() >= u
                if runtimeState.destroyed then
                    return
                end
                if d() == B.nome then
                    r0.Instance.Text = "Agora você é " .. B.nome
                else
                    r0.Instance.Text =
                        string.format("Não rolou — %s pede nível %d, você tem %d", B.nome, B.level, X())
                end
                Bo = false
                j0()
            end)
        end
        for B, u in ipairs(H()) do
            local k = V0:AddButton({
                Text = h0(u, false, false),
                Callback = function()
                    uo(u)
                end,
            })
            table.insert(t0, {
                emprego = u,
                btn = k,
            })
        end
        trackConnection((localPlayer:GetPropertyChangedSignal("Team")):Connect(function()
            r0.Instance.Text = "Emprego atual: " .. (d() or "nenhum")
            j0()
        end))
        j0()
        local ko = S0:AddSection("Comprar Itens")
        local To = ko:AddLabel(string.format("Saldo: $%d", m()))
        local Wo = {}
        y = false
        local function lo(B)
            if not y and not B then
                To.Instance.Text = string.format("Saldo: $%d", m())
            end
            local u = m()
            for B, k in ipairs(Wo) do
                local T = not k.produto.hpItem and U(k.produto.chave) ~= nil
                k.btn.Label.Text = string.format(
                    "%s   ·   $%d%s",
                    k.produto.chave,
                    k.produto.preco,
                    T and "   (Tem)" or (u < k.produto.preco and "   (Sem saldo)" or "")
                )
                k.btn.Label.TextColor3 = T and l0.Theme.Text or l0.Theme.TextMuted
            end
        end
        local function Io()
            local B = localPlayer:FindFirstChild("PlayerGui")
            for B, u in ipairs(B and B:GetChildren() or {}) do
                local k = u.Name == "TextBoxUI" and u:FindFirstChild("Box", true)
                if k and k.PlaceholderText == "Quantidade" then
                    return u, k
                end
            end
        end
        p = function(B, u)
            local k, T = m(), f(B.chave)
            u = u and math.max(1, math.floor(u)) or nil
            if k < B.preco * (u or 1) then
                return false, "saldo"
            end
            local W = (game:GetService("ReplicatedStorage")):FindFirstChild("Mercadinho")
            local l = B.hpItem and "HPRemote" or "GlobalComprarItem"
            local q = W and W:FindFirstChild(l)
            if not q or not q:IsA("RemoteEvent") then
                return false, l
            end
            if u and not B.hpItem then
                local B = Io()
                if B then
                    local u = B:FindFirstChild("BRecusar", true)
                    if u and mainEnvironment.firesignal then
                        mainEnvironment.firesignal(u.MouseButton1Click)
                    else
                        local u = (game:GetService("ReplicatedStorage")):FindFirstChild("InGameRemotes")
                        u = u and u:FindFirstChild("MandaTextbox")
                        if u then
                            u:FireServer("Response", false)
                        end
                        B:Destroy()
                    end
                    task.wait(0.35)
                end
            end
            for u = 1, (B.hpItem and (u or 1) or 1), 1 do
                q:FireServer(B.chave)
                if B.hpItem then
                    task.wait(0.3)
                end
            end
            if u and not B.hpItem then
                local B, k
                local T = os.clock() + 3
                repeat
                    task.wait(0.05)
                    B, k = Io()
                until B or runtimeState.destroyed or os.clock() >= T
                if not B then
                    return false, "quantidade"
                end
                local W = B:FindFirstChild("Aceitar", true)
                k.Text = tostring(u)
                if W and mainEnvironment.getconnections then
                    local B = os.clock() + 2
                    repeat
                        task.wait(0.05)
                    until #mainEnvironment.getconnections(W.MouseButton1Click) > 0 or os.clock() >= B
                else
                    task.wait(0.5)
                end
                if W and mainEnvironment.firesignal then
                    pcall(mainEnvironment.firesignal, W.MouseButton1Click)
                    task.wait(0.4)
                end
                if B.Parent then
                    local u = (game:GetService("ReplicatedStorage")):FindFirstChild("InGameRemotes")
                    u = u and u:FindFirstChild("MandaTextbox")
                    if not u then
                        return false, "quantidade"
                    end
                    u:FireServer("Response", k.Text)
                    B:Destroy()
                end
            end
            local function x()
                local B = localPlayer:FindFirstChild("PlayerGui")
                local u = B and B:FindFirstChild("MsgsUI")
                local k = u and u:FindFirstChild("BGFrame", true)
                local T = k and k:FindFirstChild("Aceitar", true)
                local W = k and k:FindFirstChild("Not", true)
                if T and (mainEnvironment.firesignal and (W and ((tostring(W.Text)):lower()):find("comprar"))) then
                    pcall(mainEnvironment.firesignal, T.MouseButton1Click)
                    return true
                end
                return false
            end
            local o = false
            local i = os.clock() + 30
            repeat
                if not o and x() then
                    o = true
                end
                task.wait(0.15)
            until m() < k or f(B.chave) > T or runtimeState.destroyed or os.clock() >= i
            if runtimeState.destroyed then
                return false, "encerrado"
            end
            if m() < k or f(B.chave) > T then
                return true, k - m()
            end
            return false, "recusado"
        end
        local function Yo(B)
            if y then
                return
            end
            if not B.hpItem and U(B.chave) then
                To.Instance.Text = B.chave .. " já está no inventário"
                return
            end
            local u = m()
            if u < B.preco then
                To.Instance.Text = string.format("Faltam $%d para %s", B.preco - u, B.chave)
                return
            end
            local k = (game:GetService("ReplicatedStorage")):FindFirstChild("Mercadinho")
            local T = B.hpItem and "HPRemote" or "GlobalComprarItem"
            local W = k and k:FindFirstChild(T)
            if not W or not W:IsA("RemoteEvent") then
                To.Instance.Text = T .. " não encontrado"
                return
            end
            y = true
            To.Instance.Text = "Comprando " .. (B.chave .. "...")
            task.spawn(function()
                local k, T = p(B)
                if runtimeState.destroyed then
                    return
                end
                y = false
                if k then
                    To.Instance.Text = T > 0 and string.format("%s comprado por $%d", B.chave, T)
                        or B.chave .. " chegou no inventário"
                else
                    To.Instance.Text = T == "saldo" and string.format("Faltam $%d para %s", B.preco - u, B.chave)
                        or "Servidor recusou: " .. B.chave
                end
                lo(true)
            end)
        end
        for B, u in ipairs(R()) do
            local k = ko:AddButton({
                Text = string.format("%s   ·   $%d", u.chave, u.preco),
                Callback = function()
                    Yo(u)
                end,
            })
            table.insert(Wo, {
                produto = u,
                btn = k,
            })
        end
        do
            local B = localPlayer:FindFirstChild("leaderstats")
            local u = B and B:FindFirstChild("Dinheiro")
            if u then
                trackConnection((u:GetPropertyChangedSignal("Value")):Connect(function()
                    lo()
                end))
            end
        end
        lo()
        local qo = (A0:AddTopTab({
            Name = "Veículo",
            Icon = "car",
        })):AddColumn()
        local xo = qo:AddSection("Veículo")
        registerControl(
            "durabilidadeHudEnabled",
            xo:AddToggle({
                Text = "Durabilidade (HUD)",
                Default = featureSettings.durabilidadeHudEnabled,
                Hint = "Mostra a durabilidade do carro em que voce senta, em tempo real. Arraste o HUD para reposicionar.",
                Callback = function(B)
                    featureSettings.durabilidadeHudEnabled = B
                    if B then
                        pcall(runtimeState.ativarDurabilidade)
                    else
                        pcall(runtimeState.desativarDurabilidade)
                    end
                end,
            })
        )
        do
            local B = false
            local u
            local function T(B)
                if u then
                    u.Instance.Text = B
                end
            end
            local function W(B)
                local u = game:GetService("TextChatService")
                if u.ChatVersion == Enum.ChatVersion.TextChatService then
                    local k = u:FindFirstChild("TextChannels")
                    local T = k and (k:FindFirstChild("RBXGeneral") or k:FindFirstChildWhichIsA("TextChannel"))
                    if not T then
                        return false
                    end
                    return pcall(function()
                        T:SendAsync(B)
                    end)
                end
                local k = (game:GetService("ReplicatedStorage")):FindFirstChild("DefaultChatSystemChatEvents")
                local T = k and k:FindFirstChild("SayMessageRequest")
                if not T or not T:IsA("RemoteEvent") then
                    return false
                end
                return pcall(function()
                    T:FireServer(B, "All")
                end)
            end
            local function I()
                for B, u in ipairs(R()) do
                    if u.chave == "MiliTec" and not u.hpItem then
                        return u
                    end
                end
            end
            local function Y()
                if B or y then
                    T("A Shop está ocupada.")
                    return
                end
                B = true
                local function u(u)
                    y = false
                    B = false
                    T(u)
                    lo(true)
                end
                local k = getCurrentVehicle()
                if not k then
                    u("Entre e sente no seu carro primeiro.")
                    return
                end
                if f("MiliTec") == 0 then
                    local B = I()
                    if not B then
                        u("MiliTec não encontrado na Shop.")
                        return
                    end
                    if m() < B.preco then
                        u(string.format("Faltam $%d para comprar a MiliTec.", B.preco - m()))
                        return
                    end
                    y = true
                    T("Comprando MiliTec...")
                    local k = p(B, 1)
                    y = false
                    if not k then
                        u("A compra automática da MiliTec foi recusada.")
                        return
                    end
                end
                T("Enviando /consertar...")
                if not W("/consertar") then
                    u("Não consegui enviar o comando no chat.")
                    return
                end
                u("Comando /consertar enviado. O jogo vai reparar o carro.")
            end
            xo:AddButton({
                Text = "Reparar carro",
                Callback = Y,
            })
            do
                local B = Instance.new("TextButton")
                B.Name = "FloatingRepair"
                B.AnchorPoint = Vector2.new(0, 0.5)
                B.Position = UDim2.new(0, 18, 0.5, 84)
                B.Size = UDim2.fromOffset(132, 40)
                B.BackgroundColor3 = l0.Theme.Panel
                B.BorderSizePixel = 0
                B.AutoButtonColor = false
                B.Text = "Reparar carro"
                B.TextColor3 = l0.Theme.Text
                B.TextSize = 14
                B.Font = Enum.Font.GothamMedium
                B.ZIndex = 50
                B.Visible = featureSettings.floatingRepairEnabled
                B.Parent = l0.ToggleButton.Parent
                local u = Instance.new("UICorner")
                u.CornerRadius = UDim.new(0, 10)
                u.Parent = B
                local T = Instance.new("UIStroke")
                T.Color = l0.Theme.Border
                T.Thickness = 1
                T.Parent = B
                local W = {
                    moved = false,
                    kind = nil,
                    start = Vector2.new(),
                    origin = Vector2.new(),
                }
                trackConnection(B.InputBegan:Connect(function(u)
                    local k = u.UserInputType
                    if k ~= Enum.UserInputType.MouseButton1 and k ~= Enum.UserInputType.Touch then
                        return
                    end
                    W.kind, W.moved = k, false
                    W.start = Vector2.new(u.Position.X, u.Position.Y)
                    W.origin = B.AbsolutePosition
                end))
                trackConnection(UserInputService.InputChanged:Connect(function(u)
                    if not W.kind then
                        return
                    end
                    local k = u.UserInputType
                    if
                        not (
                            (W.kind == Enum.UserInputType.MouseButton1 and k == Enum.UserInputType.MouseMovement)
                            or (W.kind == Enum.UserInputType.Touch and k == Enum.UserInputType.Touch)
                        )
                    then
                        return
                    end
                    local T = Vector2.new(u.Position.X, u.Position.Y) - W.start
                    if T.Magnitude >= 4 then
                        W.moved = true
                    end
                    local I = WorkspaceService.CurrentCamera
                    local Y = I and I.ViewportSize or Vector2.new(800, 600)
                    local q = math.clamp(W.origin.X + T.X, 8, math.max(8, (Y.X - B.AbsoluteSize.X) - 8))
                    local x = math.clamp(W.origin.Y + T.Y, 8, math.max(8, (Y.Y - B.AbsoluteSize.Y) - 8))
                    B.Position = UDim2.fromOffset(q, x + B.AbsoluteSize.Y / 2)
                end))
                trackConnection(UserInputService.InputEnded:Connect(function(B)
                    if W.kind and B.UserInputType == W.kind then
                        W.kind = nil
                    end
                end))
                trackConnection(B.MouseButton1Click:Connect(function()
                    if not W.moved then
                        Y()
                    end
                end))
                registerControl(
                    "floatingRepairEnabled",
                    xo:AddToggle({
                        Text = "Botão flutuante de reparar",
                        Default = featureSettings.floatingRepairEnabled,
                        Hint = "Mostra um botão 'Reparar carro' arrastável fora do menu.",
                        Callback = function(u)
                            featureSettings.floatingRepairEnabled = u
                            B.Visible = u
                        end,
                    })
                )
            end
            do
                local B = Instance.new("TextButton")
                B.Name = "FloatingSteal"
                B.AnchorPoint = Vector2.new(0, 0.5)
                B.Position = UDim2.new(0, 18, 0.5, 132)
                B.Size = UDim2.fromOffset(132, 40)
                B.BackgroundColor3 = l0.Theme.Panel
                B.BorderSizePixel = 0
                B.AutoButtonColor = false
                B.Text = "Roubar carro"
                B.TextColor3 = l0.Theme.Text
                B.TextSize = 14
                B.Font = Enum.Font.GothamMedium
                B.ZIndex = 50
                B.Visible = featureSettings.floatingStealEnabled
                B.Parent = l0.ToggleButton.Parent
                local u = Instance.new("UICorner")
                u.CornerRadius = UDim.new(0, 10)
                u.Parent = B
                local T = Instance.new("UIStroke")
                T.Color = l0.Theme.Border
                T.Thickness = 1
                T.Parent = B
                local I = {
                    moved = false,
                    kind = nil,
                    start = Vector2.new(),
                    origin = Vector2.new(),
                }
                trackConnection(B.InputBegan:Connect(function(u)
                    local k = u.UserInputType
                    if k ~= Enum.UserInputType.MouseButton1 and k ~= Enum.UserInputType.Touch then
                        return
                    end
                    I.kind, I.moved = k, false
                    I.start = Vector2.new(u.Position.X, u.Position.Y)
                    I.origin = B.AbsolutePosition
                end))
                trackConnection(UserInputService.InputChanged:Connect(function(u)
                    if not I.kind then
                        return
                    end
                    local k = u.UserInputType
                    if
                        not (
                            (I.kind == Enum.UserInputType.MouseButton1 and k == Enum.UserInputType.MouseMovement)
                            or (I.kind == Enum.UserInputType.Touch and k == Enum.UserInputType.Touch)
                        )
                    then
                        return
                    end
                    local T = Vector2.new(u.Position.X, u.Position.Y) - I.start
                    if T.Magnitude >= 4 then
                        I.moved = true
                    end
                    local W = WorkspaceService.CurrentCamera
                    local Y = W and W.ViewportSize or Vector2.new(800, 600)
                    local q = math.clamp(I.origin.X + T.X, 8, math.max(8, (Y.X - B.AbsoluteSize.X) - 8))
                    local x = math.clamp(I.origin.Y + T.Y, 8, math.max(8, (Y.Y - B.AbsoluteSize.Y) - 8))
                    B.Position = UDim2.fromOffset(q, x + B.AbsoluteSize.Y / 2)
                end))
                trackConnection(UserInputService.InputEnded:Connect(function(B)
                    if I.kind and B.UserInputType == I.kind then
                        I.kind = nil
                    end
                end))
                local function Y()
                    if f("Micha") > 0 or y then
                        return
                    end
                    local B
                    for u, k in ipairs(R()) do
                        if k.chave == "Micha" and not k.hpItem then
                            B = k
                            break
                        end
                    end
                    if not B or m() < B.preco then
                        return
                    end
                    y = true
                    pcall(p, B, 1)
                    y = false
                end
                trackConnection(B.MouseButton1Click:Connect(function()
                    if I.moved then
                        return
                    end
                    task.spawn(function()
                        Y()
                        W("/roubar")
                    end)
                end))
                registerControl(
                    "floatingStealEnabled",
                    xo:AddToggle({
                        Text = "Botão flutuante roubar",
                        Default = featureSettings.floatingStealEnabled,
                        Hint = "Mostra um botão 'Roubar carro' arrastável que envia /roubar.",
                        Callback = function(u)
                            featureSettings.floatingStealEnabled = u
                            B.Visible = u
                        end,
                    })
                )
            end
        end
        registerControl(
            "autoCintoEnabled",
            xo:AddToggle({
                Text = "Cinto automático",
                Default = featureSettings.autoCintoEnabled,
                Hint = "Coloca o cinto sozinho ao entrar no carro (evita ser ejetado). PC e mobile.",
                Callback = function(B)
                    featureSettings.autoCintoEnabled = B
                end,
            })
        )
        registerControl(
            "noclipCarEnabled",
            xo:AddToggle({
                Text = "Noclip Car",
                Default = featureSettings.noclipCarEnabled,
                Hint = "Remove a colisao de todo o carro, inclusive rodas, e trava a altura do piso por raycast.",
                Callback = function(B)
                    featureSettings.noclipCarEnabled = B
                    pcall(runtimeState.aplicarNoclipCar)
                end,
            })
        )
        registerControl(
            "vFlyEnabled",
            xo:AddToggle({
                Text = "VFly",
                Default = featureSettings.vFlyEnabled,
                Hint = "Vehicle Fly: W/S move, A/D vira, Espaco/E sobe e Ctrl/Q desce. Mantem altitude ao soltar.",
                Callback = function(B)
                    featureSettings.vFlyEnabled = B
                    pcall(runtimeState.aplicarNoclipCar)
                end,
            })
        )
        registerControl(
            "vFlyHorizontalSpeed",
            xo:AddSlider({
                Text = "Vel. horizontal VFly",
                Min = 20,
                Max = 250,
                Step = 5,
                Default = featureSettings.vFlyHorizontalSpeed,
                Hint = "Com o ClientAC silenciado vale o valor cru. Sem ele, o teto cai para 60 e vira 36 em lag ou baixa velocidade fisica.",
                Callback = function(B)
                    featureSettings.vFlyHorizontalSpeed = B
                end,
            })
        )
        registerControl(
            "vFlyVerticalSpeed",
            xo:AddSlider({
                Text = "Vel. vertical VFly",
                Min = 10,
                Max = 150,
                Step = 5,
                Default = featureSettings.vFlyVerticalSpeed,
                Hint = "Com o ClientAC silenciado vale o valor cru. Sem ele, o teto cai para 30 e vira 20 em condicao instavel.",
                Callback = function(B)
                    featureSettings.vFlyVerticalSpeed = B
                end,
            })
        )
        registerControl(
            "boostFactor",
            xo:AddSlider({
                Text = "Boost aceleração (%)",
                Min = 100,
                Max = 1000,
                Default = math.floor(featureSettings.boostFactor * 100 + 0.5),
                Hint = "Aumenta o torque do motor (mesmo knob do remap/chip do jogo). So dirigindo. 100 = stock.",
                Callback = function(B)
                    featureSettings.boostFactor = B / 100
                    pcall(function()
                        local B = getCurrentVehicle()
                        if B then
                            applyVehicleBoost(B)
                        end
                    end)
                end,
            })
        )
        registerControl(
            "bestCarsEspEnabled",
            xo:AddToggle({
                Text = "Mostrar melhores carros",
                Default = featureSettings.bestCarsEspEnabled,
                Hint = "Mostra somente os 10 carros com maior valor real na concessionária.",
                Callback = function(B)
                    featureSettings.bestCarsEspEnabled = B
                    if B then
                        pcall(runtimeState.ativarBestCarsEsp)
                    else
                        pcall(runtimeState.desativarBestCarsEsp)
                    end
                end,
            })
        )
        do
            local B = ((Y0:AddTopTab({
                Name = "Universal",
                Icon = "sun",
            })):AddColumn()):AddSection("Mundo")
            local function u(u, k, T)
                registerControl(
                    u,
                    B:AddToggle({
                        Text = k,
                        Default = featureSettings[u],
                        Hint = T,
                        Callback = function(B)
                            featureSettings[u] = B
                            pcall(runtimeState.applyWorld)
                        end,
                    })
                )
            end
            u("alwaysDay", "Sempre dia", "Trava o horário no meio-dia.")
            u("fullbright", "Fullbright", "Mundo totalmente iluminado, sem sombra/fog.")
            u("noFog", "Sem névoa", "Remove fog e atmosfera.")
        end
        local oo = l0:AddSideTab({
            Name = "Config",
            Group = "Geral",
            Icon = "settings",
        })
        local Eo = oo:AddTopTab({
            Name = "Config",
            Icon = "settings",
        })
        local Lo = Eo:AddColumn()
        local Zo = Eo:AddColumn()
        registerControl(
            "menuKeybind",
            (Zo:AddSection("Menu")):AddKeybind({
                Text = "Tecla abrir/fechar",
                Default = featureSettings.menuKeybind,
                OnChanged = function(B)
                    featureSettings.menuKeybind = B
                end,
                Callback = function()
                    l0:Toggle()
                end,
            })
        )
        do
            local B = Zo:AddSection("Anticheat")
            local u = B:AddLabel(runtimeState.acStatus or "Aplicando...")
            local function k()
                local B = runtimeState.acStatus or "Aplicando..."
                if runtimeState.acAviso then
                    B = B .. ("\n" .. runtimeState.acAviso)
                end
                u.Instance.Text = B
            end
            k()
            task.spawn(function()
                while not runtimeState.destroyed and u.Instance.Parent do
                    k()
                    task.wait(1)
                end
            end)
            B:AddButton({
                Text = "Reaplicar silenciador",
                Callback = function()
                    if runtimeState.acReaplicar then
                        runtimeState.acReaplicar("manual")
                    else
                        l0:Notify({
                            Title = "Anticheat",
                            Text = "Executor nao suporta o bypass.",
                            Duration = 4,
                        })
                    end
                end,
            })
        end
        q.Config.Folder = "OiHub/sintonia"
        local Jo = Lo:AddSection("Configs")
        if not q.Config.isSupported() then
            Jo:AddLabel(
                "Seu executor nao expoe filesystem (writefile/readfile), entao salvar configs nao esta disponivel."
            )
        else
            local function B(B)
                if typeof(B) == "EnumItem" then
                    return (string.gsub(tostring(B), "^Enum%.%a+%.", ""))
                end
                return B and tostring(B) or nil
            end
            local function u(B)
                if typeof(B) == "EnumItem" then
                    return B
                end
                if type(B) ~= "string" then
                    return nil
                end
                local u, k = pcall(function()
                    return Enum.KeyCode[B]
                end)
                if u and k then
                    return k
                end
                return nil
            end
            local function k()
                local u = {}
                for k, T in pairs(registeredControls) do
                    local W, l = pcall(function()
                        return T:Get()
                    end)
                    if W then
                        if typeof(l) == "EnumItem" then
                            l = B(l)
                        end
                        u[k] = l
                    end
                end
                return u
            end
            local function T(B)
                if type(B) ~= "table" then
                    return
                end
                for k, T in pairs(registeredControls) do
                    local W = B[k]
                    if W ~= nil then
                        if type(W) == "string" and typeof(T:Get()) == "EnumItem" then
                            W = u(W)
                        end
                        if W ~= nil then
                            pcall(function()
                                T:Set(W)
                            end)
                        end
                    end
                end
            end
            local W = Jo:AddTextbox({
                Text = "Nome",
                Placeholder = "nome da config...",
            })
            local l = Jo:AddDropdown({
                Text = "Configs salvas",
                Options = q.Config.list(),
            })
            local function I(B)
                l:SetOptions(q.Config.list(), true)
                if B then
                    l:Set(B)
                end
            end
            Jo:AddButton({
                Text = "Criar nova config",
                Callback = function()
                    local B = q.Config._sanitize(W:Get())
                    local u, T = q.Config.save(B, k())
                    if u then
                        I(B)
                        l0:Notify({
                            Title = "Config salva",
                            Text = "'" .. (B .. "' criada."),
                            Duration = 3,
                        })
                    else
                        l0:Notify({
                            Title = "Erro ao salvar",
                            Text = tostring(T or "falha"),
                            Duration = 4,
                        })
                    end
                end,
            })
            Jo:AddButton({
                Text = "Carregar config",
                Callback = function()
                    local B = l:Get()
                    if not B then
                        l0:Notify({
                            Title = "Selecione uma config",
                            Text = "Escolha no dropdown antes de carregar.",
                            Duration = 3,
                        })
                        return
                    end
                    local u, k = q.Config.load(B)
                    if u then
                        T(u)
                        l0:Notify({
                            Title = "Config carregada",
                            Text = "'" .. (B .. "' aplicada."),
                            Duration = 3,
                        })
                    else
                        l0:Notify({
                            Title = "Erro ao carregar",
                            Text = tostring(k or "falha"),
                            Duration = 4,
                        })
                    end
                end,
            })
            Jo:AddButton({
                Text = "Editar config selecionada",
                Callback = function()
                    local B = l:Get()
                    if not B then
                        l0:Notify({
                            Title = "Selecione uma config",
                            Text = "Escolha a config a editar no dropdown.",
                            Duration = 3,
                        })
                        return
                    end
                    local u, T = q.Config.save(B, k())
                    if u then
                        l0:Notify({
                            Title = "Config atualizada",
                            Text = "'" .. (B .. "' sobrescrita com os valores atuais."),
                            Duration = 3,
                        })
                    else
                        l0:Notify({
                            Title = "Erro ao editar",
                            Text = tostring(T or "falha"),
                            Duration = 4,
                        })
                    end
                end,
            })
            Jo:AddButton({
                Text = "Excluir config selecionada",
                Callback = function()
                    local B = l:Get()
                    if not B then
                        l0:Notify({
                            Title = "Selecione uma config",
                            Text = "Escolha a config a excluir no dropdown.",
                            Duration = 3,
                        })
                        return
                    end
                    local u, k = q.Config.delete(B)
                    if u then
                        I()
                        l0:Notify({
                            Title = "Config excluida",
                            Text = "'" .. (B .. "' apagada."),
                            Duration = 3,
                        })
                    else
                        l0:Notify({
                            Title = "Erro ao excluir",
                            Text = tostring(k or "falha"),
                            Duration = 4,
                        })
                    end
                end,
            })
        end
        l0:SetVisible(false)
        if l0.ToggleButton then
            l0.ToggleButton.Visible = false
        end
        local Fo = q:CreateLoader({
            Title = "Oi Hub",
            Version = "1.4.2",
            Role = "Cliente Oi",
            Date = "26/07/2026",
            Changelog = {
                "Nova UI (OiHub)",
                "Migração da Sintonia",
                "Correções gerais",
            },
            Terms = "Ao injetar o menu, você está ciente do risco de banimento. Não nos responsabilizamos por isso, use com responsabilidade!",
            OnLoad = function()
                l0:SetVisible(true)
                if l0.ToggleButton then
                    l0.ToggleButton.Visible = true
                end
                I0()
            end,
        })
        mainEnvironment.__OIHUB_SINTONIA_WINDOW = l0
        mainEnvironment.__OIHUB_SINTONIA_LOADER = Fo
    end
end, debug.traceback)
mainEnvironment.__OIHUB_SINTONIA_V2_UI_ERROR = uiSetupSucceeded and nil or uiSetupError
mainEnvironment.__OIHUB_SINTONIA_BUILD = "armas-tracer-beam"
mainEnvironment.__KNWARE_SINTONIA_CLEANUP = function()
    runtimeState.destroyed = true
    mainEnvironment.__KNWARE_SINTONIA_RUNTIME = nil
    pcall(function()
        if mainWindow then
            mainWindow:Destroy()
        end
    end)
    featureSettings.espEnabled = false
    featureSettings.highlight = false
    featureSettings.tracer = false
    featureSettings.arrow = false
    featureSettings.box3d = false
    featureSettings.spoofWalkEnabled = false
    featureSettings.spoofJumpEnabled = false
    featureSettings.antiRagdollEnabled = false
    featureSettings.antiTiroEnabled = false
    pcall(setCharacterQueryDisabled, false)
    featureSettings.silentAimEnabled = false
    featureSettings.silentAimFovVisible = false
    featureSettings.armaNoRecoil = false
    featureSettings.armaMunicaoInfinita = false
    featureSettings.armaReloadRapido = false
    featureSettings.armaSemLentidao = false
    featureSettings.armaCadenciaCustom = false
    featureSettings.armaTracer = false
    if runtimeState.restaurarArmas then
        pcall(runtimeState.restaurarArmas)
    end
    featureSettings.bestWeaponEnabled = false
    runtimeState.bestWeaponRunId = runtimeState.bestWeaponRunId + 1
    featureSettings.autoFarmLixeiroEnabled = false
    runtimeState.garbageFarmRunId = runtimeState.garbageFarmRunId + 1
    featureSettings.durabilidadeHudEnabled = false
    if runtimeState.destruirDurabilidade then
        pcall(runtimeState.destruirDurabilidade)
    end
    featureSettings.bestCarsEspEnabled = false
    if runtimeState.destruirBestCarsEsp then
        pcall(runtimeState.destruirBestCarsEsp)
    end
    featureSettings.vFlyEnabled = false
    featureSettings.noclipCarEnabled = false
    if runtimeState.restaurarNoclipCar then
        pcall(runtimeState.restaurarNoclipCar)
    end
    featureSettings.boostFactor = 1
    table.clear(runtimeState.relationships)
    table.clear(runtimeState.relationshipConnections)
    featureSettings.rolarEnabled = false
    pcall(updateRollAnimation)
    featureSettings.noclipEnabled = false
    pcall(applyCharacterNoclip)
    featureSettings.flyEnabled = false
    pcall(updateFly)
    pcall(restoreSilentAimHook)
    pcall(function()
        if silentAimFovCircle then
            silentAimFovCircle:Remove()
        end
    end)
    pcall(applyAntiRagdoll)
    pcall(function()
        local B = getHumanoid()
        if B then
            setRagdollDisabled(B, false)
            B:SetStateEnabled(Enum.HumanoidStateType.GettingUp, true)
        end
        local u = getRagdollClient()
        if u then
            u.Disabled = false
        end
    end)
    for B, u in ipairs(runtimeState.connections) do
        pcall(function()
            u:Disconnect()
        end)
    end
    runtimeState.connections = {}
    for B, u in pairs(featureSettings.bundles) do
        destroyEspBundle(u)
        featureSettings.bundles[B] = nil
    end
    pcall(function()
        local B = getHumanoid()
        if B then
            local u = getRootPart()
            if u then
                B:MoveTo(u.Position)
            end
            B.WalkSpeed = 16
            B.JumpPower = 50
        end
    end)
end
return true
