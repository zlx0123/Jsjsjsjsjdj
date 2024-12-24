-- Configurações
local tempoEspera = 300 -- 5 minutos
local mensagemRecrutamento = "Alguém gostaria de se juntar a uma comunidade de Entrenched? Temos treinos todos os dias e batalhas no final de semana, sem contar eventos valendo Robux. (fale: \"Eu quero\")"
local mensagemResposta = "Adicione Zlx0#0000 no Discord para se juntar à nossa comunidade!"
local jogoId = game.PlaceId

-- Funções
local function enviarMensagem(mensagem)
    game.ReplicatedStorage.DefaultChatSystemChatEvents.SayMessageRequest:FireServer(mensagem, "All")
end

local function mudarServidor()
    local resposta = game.HttpService:RequestAsync({Url = "(link unavailable)" .. jogoId .. "/servers/Public?sortOrder=Asc&limit=100"}).Body
    local dados = game.HttpService:JSONDecode(resposta)
    local servidores = {}
    for _, servidor in pairs(dados.data) do
        table.insert(servidores, (link unavailable))
    end
    local servidorAleatorio = servidores[math.random(1, #servidores)]
    game.ReplicatedStorage.JoinServerEvent:FireServer(servidorAleatorio)
end

-- Execução
enviarMensagem(mensagemRecrutamento)

-- Loop para verificar respostas
local tempoUltimaMensagem = os.clock()
while true do
    for _, v in pairs(game.ReplicatedStorage.DefaultChatSystemChatEvents.OnNewMessageEvent:Wait()) do
        if v.Message == "Eu quero" then
            tempoUltimaMensagem = os.clock()
            enviarMensagem(mensagemResposta)
        end
    end
    if os.clock() - tempoUltimaMensagem >= tempoEspera then
        mudarServidor()
        tempoUltimaMensagem = os.clock()
        enviarMensagem(mensagemRecrutamento)
    end
    wait(1)
end
