local webhookUrl = "https://discord.com/api/webhooks/1365050081899974697/BsHM9XwhQn3ybGrqSQwxD4pqEjxeugnk6ipgE2itvGs6qbt4v3zSEMF2trnDMF_TpohB"
local teleportService = game:GetService("TeleportService")

local function teleportToNextServer()
    teleportService:Teleport(game.PlaceId)
end

local function checkRiftEggs()
    local riftFolder = workspace:FindFirstChild("Rendered") and workspace.Rendered:FindFirstChild("Rifts")
    if riftFolder then
        for _, egg in pairs(riftFolder:GetChildren()) do
            if string.find(egg.Name:lower(), "rainbow") then
                local joinUrl = "https://www.roblox.com/games/" .. tostring(game.PlaceId) .. "?jobId=" .. tostring(game.JobId)

                local data = {
                    ["embeds"] = {
                        {
                            ["title"] = "**🌈 Aura Egg Bulundu!**",
                            ["description"] = "Egg İsmi: `" .. egg.Name .. "`\n" ..
                                               "Sunucu ID: `" .. game.JobId .. "`",
                            ["url"] = joinUrl,
                            ["color"] = 3066993,
                            ["footer"] = {
                                ["text"] = "Egg Finder"
                            },
                            ["fields"] = {
                                {
                                    ["name"] = "Sunucuya Katıl",
                                    ["value"] = "[Tıklayarak katılın](https://www.roblox.com/games/start?placeId=" .. tostring(game.PlaceId) .. "&launchData=" .. tostring(game.JobId) .. ")",
                                    ["inline"] = true
                                }
                            }
                        }
                    }
                }

                local jsonData = game:GetService("HttpService"):JSONEncode(data)

                local response = http_request({
                    Url = webhookUrl,
                    Method = "POST",
                    Headers = {["Content-Type"] = "application/json"},
                    Body = jsonData
                })
            end
        end
    end
end

while true do
    checkRiftEggs()
    teleportToNextServer()
    wait(10)
end
