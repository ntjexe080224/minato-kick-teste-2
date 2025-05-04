local Admins = {
    ["eominatoz kick beta"] = true -- Altere para seu nome de usuário
}

local BannedPlayers = {}

local function isAdmin(player)
    return Admins[player.Name]
end

local function kickPlayer(player, reason)
    player:Kick(reason or "Você foi expulso.")
end

local function kickAll(reason)
    for _, p in pairs(game.Players:GetPlayers()) do
        if not isAdmin(p) then
            p:Kick(reason or "Todos foram expulsos.")
        end
    end
end

local function banPlayer(player)
    BannedPlayers[player.UserId] = true
    player:Kick("Você foi banido.")
end

game.Players.PlayerAdded:Connect(function(player)
    if BannedPlayers[player.UserId] then
        player:Kick("Você está banido.")
    end
end)

-- Exemplo de uso via loadstring
local commandScript = [[
    return function(command, targetName)
        local Players = game:GetService("Players")
        local target = Players:FindFirstChild(targetName)

        if command == "kick" and target then
            target:Kick("Você foi expulso.")
        elseif command == "kickall" then
            for _, p in pairs(Players:GetPlayers()) do
                if p.Name ~= "SeuNomeAqui" then
                    p:Kick("Todos foram expulsos.")
                end
            end
        elseif command == "ban" and target then
            target:Kick("Você foi banido.")
            _G.Banned = _G.Banned or {}
            _G.Banned[target.UserId] = true
        end
    end
]]

local runCommand = loadstring(commandScript)()

-- Exemplo de execução:
-- runCommand("kick", "NomeDoJogador")
-- runCommand("kickall")
-- runCommand("ban", "NomeDoJogador")
