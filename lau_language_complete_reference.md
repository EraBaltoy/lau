# Lau Language — Complete Reference

> Baseado na documentação oficial do jogo. Inspirada em Lua, mas com diferenças importantes.

---

## 1. Sintaxe Básica

### Variáveis
```lua
varol name   = "Drone"    -- String
varol level  = 5          -- Number
varol active = true       -- Boolean
varol items  = {1, 2, 3} -- List

-- Atualizar valor (sem varol):
level = 6
level += 1   -- equivale a level = level + 1
```

> Nunca use `varol` para atualizar uma propriedade dentro de uma lista já definida.  
> Use `list.prop = value` diretamente.

### Comentários
```lua
-- Linha única

--[[
  Bloco
  multilinha
]]
```

### Regras de Caracteres
Proibido: caracteres turcos (`ö ğ ş ç ı ü`), emojis, caracteres especiais.  
Causam substituição por `?` e quebram o parser.

### Palavras Reservadas
Não podem ser usadas como nomes de variável:
```
varol  func   if     else   elseif  then  end
for    while  do     break  continue      return
req    true   false  null   inpairs and   or   not
```

---

## 2. Operadores

### Concatenação de Strings
```lua
-- O operador de concatenacao e + (nao .. como em Lua)
varol nome  = "Drone"
varol nivel = 5
print("Nome: " + nome)               -- "Nome: Drone"
print("Nivel: " + tostring(nivel))   -- "Nivel: 5"
print("Char count: " + string.len(nome))  -- "Char count: 5"
```
> `..` NÃO existe em Lau. Usar `+` para concatenar strings.  
> Números precisam de `tostring()` antes de concatenar com `+`.

### Conversão de Tipos
```lua
tonumber(value)   -- converte string ou outro valor para Number (para calculos)
tostring(value)   -- converte qualquer valor (Number, Boolean, etc.) para String
```

### Aritméticos
| Operador | Função | Exemplo |
|---|---|---|
| `+` | Adição | `5 + 3 = 8` |
| `-` | Subtração | `10 - 4 = 6` |
| `*` | Multiplicação | `3 * 4 = 12` |
| `/` | Divisão | `20 / 2 = 10` |
| `^` | Potência | `2 ^ 3 = 8` |
| `%` | Módulo/Resto | `10 % 3 = 1` |
| `#` | Raiz quadrada (em números) | `#4 = 2` |
| `#` | Comprimento (em listas/strings) | `#myList = 5` |

> Para descobrir quantos elementos há em uma lista ou quantos caracteres há em uma string, coloque o símbolo `#` no início.

### Comparação
```
==   ~=   <   >   <=   >=
```

### Lógicos — SEMPRE MAIÚSCULO
```lua
AND   -- verdadeiro se ambos forem verdadeiros
OR    -- verdadeiro se ao menos um for verdadeiro
NOT   -- inverte o valor lógico
```
> `and`, `or`, `not` em minúsculo são palavras reservadas mas NÃO funcionam como operadores.  
> `&&`, `||`, `!` também são inválidos.

### Atribuição Composta
```
=    +=    -=    *=    /=    ^=    %=
```

---

## 3. Estruturas de Controle

### if / elseif / else
```lua
if condicao then
    -- codigo
elseif outraCondicao then
    -- codigo
else
    -- codigo
end
```

**Regra de linha única:** só uma operação por linha se estiver tudo na mesma linha.
```lua
-- Correto (1 operacao):
if speed > 100 then print("rapido") end

-- Erro (2 operacoes na mesma linha):
if speed > 100 then print("rapido") speed = 0 end

-- Correto (multiplas operacoes em linhas separadas):
if speed > 100 then
    print("rapido")
    speed = 0
end
```

### For Numérico
```lua
for i = 1, 10 do
    print(i)
end

-- Com step:
for i = 1, 10, 2 do   -- imprime 1, 3, 5, 7, 9
    print(i)
end

-- Step decrescente:
for i = 10, 1, -1 do
    print(i)
end
```
> Step não pode ser 0.

### For de Lista (Generic For)
```lua
for index, value inpairs(myList) do
    print(index, value)
end
```

### While
```lua
varol counter = 0
while counter < 5 do
    counter += 1
    print("Looping...", counter)
    task.wait(0.5)  -- SEMPRE necessario para nao travar o sistema
end
```

> O loop `while` continua executando enquanto a condição for VERDADEIRA. O loop termina quando a condição é quebrada.

### break / continue

Comandos que permitem alterar o fluxo do loop com base na iteração atual.

- **`continue`**: Encerra imediatamente a iteração atual do loop e pula para a próxima. (Não executa o código abaixo dele naquela iteração).
- **`break`**: Para o loop completamente e sai dele.

```lua
-- Loop que imprime apenas numeros impares e para no 7:
for i = 1, 10 do
    if i % 2 == 0 then
        continue  -- Pula numeros pares, vai para o proximo
    end

    if i == 7 then
        break     -- Finaliza o loop completamente quando 7 e alcancado
    end

    print(i)
end
```

---

## 4. Funções

### Definição Nomeada
```lua
func add(a, b)
    return a + b
end

varol result = add(5, 10)
```

### Definição Anônima (em variável)
```lua
varol multiply = func(x, y)
    return x * y
end

print(multiply(5, 2))  -- 10
```

### Múltiplos Retornos
```lua
func getCoords()
    return 10, 25
end

varol x, z = getCoords()
```

### Escopo de Variável
```lua
varol global = "visivel em todo lugar"

if true then
    varol local = "visivel so aqui"
    print(global)  -- funciona
end

print(local)  -- ERRO: variavel destruida com o end
```

---

## 5. Listas (Tables)

### Criação
```lua
-- Numerica (ordenada):
varol fruits = {"Apple", "Pear", "Banana"}

-- Dicionario (chave-valor):
varol config = {
    ["Speed"]  = 15,
    ["Mode"]   = "Auto",
    ["Active"] = true
}
```

### Acesso e Atribuição
```lua
-- Tres formas equivalentes:
config.Speed
config:Speed
config["Speed"]

-- Indexacao matematica:
fruits[2 - 1]        -- acessa indice 1
fruits[index + 2]    -- calcula antes de acessar

-- Atualizar (sem varol):
config.Speed = 20
config["Mode"] = "Manual"
```

### Deletar Propriedade
```lua
config.Speed = null   -- remove a chave permanentemente
```
> Chaves com valor `null` são ignoradas pelo `inpairs`.

### Comprimento e Iteração
```lua
print(#fruits)   -- numero de elementos

for k, v inpairs(fruits) do
    print(k, v)
end
```

### Funções de Lista
```lua
list.insert(myList, value)         -- adiciona ao final
list.remove(myList, index)         -- remove no indice, desloca os seguintes
list.clear(myList)                 -- esvazia completamente
list.sort(myList, func?)           -- ordena (ascendente por padrao)
list.check(myList)                 -- true se tem conteudo, false se vazio
list.find(myList, value)           -- retorna indice se encontrado, null se nao
```

---

## 6. Módulos

### Tipos de Arquivo
- **`.lau`** — script principal. Roda diretamente ao pressionar RUN.
- **`.laum`** — módulo. Não roda sozinho. Importado via `req()`.

### Criar um Módulo (arquivo.laum)
```lua
varol mod = {}

mod.saudar = func(name)
    return "Ola, " + name
end

mod.versao = "1.0"

return mod   -- OBRIGATORIO: todo .laum deve terminar com return
```

### Usar um Módulo (main.lau)
```lua
varol m = req("arquivo.laum")

print(m.saudar("Drone"))  -- Ola, Drone
print(m.versao)           -- 1.0
```

---

## 7. Eventos

```lua
-- Listener permanente:
event:connect(func(data)
    -- executa cada vez que o evento dispara
end)

-- Listener de uma vez:
event:once(func(data)
    -- executa uma vez e se destroi automaticamente
end)

-- Desconectar manualmente:
varol conn = event:connect(func(data)
    -- ...
end)
conn:disconnect()
```

> Nunca use `:connect` dentro de um loop `while` — estoura o limite de eventos.  
> Limite: **10 eventos simultâneos**.

---

## 8. Pragmas

Colocados no topo do arquivo `.lau` (não funcionam em `.laum`).

```lua
--!ndrone
```
**`--!ndrone` (Non-Blocking Drone):** ativa modo assíncrono. Comandos do drone retornam imediatamente sem aguardar a animação. Requer verificação manual com `drone.status()` antes de cada comando para evitar sobreposição.

---

## 9. Funções Globais

```lua
print(...)             -- imprime no output E faz o drone falar em voz alta
printn(...)            -- imprime so no output (silencioso, ideal para debug)
clearn()               -- limpa o painel de output

req("modulo.laum")     -- carrega e executa um modulo
type(value)            -- retorna tipo como string: "Number", "String", "Boolean", "Function", "List", "null"

pcall(func, ...)       -- chamada protegida: retorna (true, resultado) ou (false, erro)
error("mensagem")      -- para o codigo e exibe mensagem de erro vermelha

tonumber(value)        -- converte para Number (null se falhar)
tostring(value)        -- converte qualquer valor para String

colorRgb(r, g, b)      -- cria valor de cor (cada canal 0-255)
colorHex(hexString)    -- cria cor a partir de hex: "FF5733" ou "#FF5733"
```

---

## 10. Limites do Sistema

| Limite | Valor |
|---|---|
| Recursão (Stack Overflow) | 500 chamadas profundidade |
| Tamanho máximo de String | 10.000 caracteres |
| Eventos simultâneos | 10 |
| Elementos por Lista | 5.000 |

---

## 11. Tratamento de Erros

### Tipos de Erro
- **Undefined Variable:** uso de variável sem `varol`
- **Type Error:** operação com tipo errado (ex: ponto após número)
- **Division by Zero:** divisão ou módulo por 0
- **Negative Square Root:** `#` em número negativo
- **Call Error:** parênteses após não-função (`5()`)
- **For Step Zero:** step de for igual a 0

---

## 12. Biblioteca: `task`

```lua
task.wait(seconds)    -- pausa a execucao pelo tempo especificado (sempre usar em while)
task.spawn(func)      -- executa funcao em thread paralela

task.time()           -- retorna o total de tempo decorrido desde 1 de janeiro de 1970,
                      -- em segundos (Unix Timestamp) como numero

task.date(seconds?)   -- converte dados de tempo numericos em formato legivel como string
                      -- (Ano-Mes-Dia Hora:Min:Seg). Se nenhum numero for fornecido,
                      -- retorna o horario real atual diretamente

task.clock()          -- retorna um valor de tempo altamente preciso com sensibilidade
                      -- de milissegundos. Ideal para medir o tempo decorrido entre
                      -- duas operacoes (para ver quao rapido o codigo roda)
```

**Exemplo de uso de `task.clock()` para benchmark:**
```lua
varol start = task.clock()
-- ... codigo a ser medido ...
varol elapsed = task.clock() - start
print("Tempo decorrido:", elapsed)
```

---

## 13. Biblioteca: `math`

```lua
math.random(min, max)  -- inteiro aleatorio entre min e max
math.round(number)     -- arredonda para inteiro mais proximo
math.abs(number)       -- valor absoluto
math.pi                -- 3.1415...
```

---

## 14. Biblioteca: `string`

```lua
string.upper(text)           -- converte todos os caracteres para MAIUSCULO
string.lower(text)           -- converte todos os caracteres para minusculo
string.len(text)             -- conta quantos caracteres ha no texto
                             -- (tambem pode ser usado brevemente com o operador '#', ex: #message)
string.sub(text, start, end) -- extrai substring (end opcional)
string.find(text, word)      -- retorna a palavra se encontrada, null se nao
string.match(text, pattern)  -- retorna o match se encontrado, null se nao
```

---

## 15. Biblioteca: `cache` (volátil)

Dados apagados ao sair ou reentrar no jogo.

```lua
cache.set(key, value)   -- armazena valor (retorna true se ok)
cache.get(key)          -- recupera valor (null se nao existe)
cache.remove(key)       -- deleta chave
cache.reset()           -- limpa tudo
```

| Servidor | Limite de chaves |
|---|---|
| Público | 10 |
| Privado | 25 |

---

## 16. Biblioteca: `data` (persistente)

Salvo na nuvem permanentemente (mesmo após sair do jogo).

```lua
data.save(slotNo, list)   -- salva uma Lista no slot (retorna true se ok)
data.load(slotNo)         -- carrega slot (null se vazio)
data.clear(slotNo)        -- deleta slot permanentemente
```

> Só é possível salvar **Listas**. Dados privados por perfil de jogador.  
> Dados muito grandes podem causar erros — prefira salvar coordenadas e configurações.

---

## 17. Biblioteca: `http`

```lua
http.get(url)           -- requisicao GET, retorna resposta como string
http.post(url, data)    -- requisicao POST com string de dados
http.jsonDecode(str)    -- JSON string -> Lau List
http.jsonEncode(list)   -- Lau List   -> JSON string
```

| Parâmetro | Valor |
|---|---|
| Rate limit (público) | 70 req/min |
| Rate limit (privado) | 200 req/min |
| Timeout | 5 segundos |
| Domínios | Somente whitelist aprovada |

---

## 18. Enums

```lua
Enum.Direction        -- North, South, East, West
Enum.Seed             -- Apple, Wheat, Potato, Carrot, ... (dinamico)
Enum.Gear             -- Watering_Can, Fertilizer, Sprinkler, ... (dinamico)
Enum.KeyCode          -- teclas padrao Roblox
Enum.Camera           -- Player, Drone, World, Base
Enum.CameraMode       -- Classic, Follow, Stable, Attach, Fixed
Enum.ClickType        -- Left, Right, Mid
Enum.DroneStatus      -- Busy, Sleep
```

---

## 19. Biblioteca: `drone`

### Movimento
```lua
drone.move(Enum.Direction)   -- move 1 tile na direcao especificada
drone.doFlip()               -- animacao de mortal
```

### Posição
```lua
varol x, z = drone.getPosition()   -- retorna X e Z simultaneamente
drone.getPositionX()               -- so X
drone.getPositionZ()               -- so Z
```

### Farming
```lua
drone.plant(Enum.Seed)             -- planta semente no tile atual
drone.useItem(Enum.Gear)           -- usa gear no tile (Watering_Can, Fertilizer, Sprinkler)
drone.crop()                       -- colhe planta nao-frutífera (trigo, batata, cenoura)
drone.harvest()                    -- colhe fruto de planta frutífera (maca, cacto, abacaxi)
```

> `drone.water()` NÃO EXISTE. Use `drone.useItem(Enum.Gear.Watering_Can)`.

### Verificações
```lua
drone.canCrop()      -- true se o crop esta pronto para cortar
drone.canHarvest()   -- true se o fruto esta pronto para colher
```

### Sensores de Planta
```lua
drone.getPlant()           -- objeto com {Percent, Weight, HasFruit, ...}
drone.getPlantPercent()    -- percentual de crescimento (null se sem planta)
drone.getPlantWeight()     -- peso da planta
drone.getPlantHasFruit()   -- boolean: planta produz fruto?
drone.getFruitData()       -- dados do fruto (null se planta nao frutífera)
drone.getFruitPercent()    -- percentual de crescimento do fruto
drone.getFruitWeight()     -- peso do fruto
```

### Status (modo assíncrono)
```lua
drone.status()   -- Enum.DroneStatus.Busy ou Enum.DroneStatus.Sleep
```

---

## 20. Biblioteca: `droneV2`

O módulo Drone V2 fornece manobras avançadas, sistemas de proteção de plantas e mecânicas de troca de posição. Acessado com o prefixo `droneV2`.

### Animações Avançadas
```lua
droneV2.doSpin()     -- rotacao 360 graus
droneV2.doWobble()   -- animacao de inclinacao (wobble)
droneV2.doBig()      -- drone parece se expandir brevemente
```

### Movimento Avançado
```lua
droneV2.swap(Enum.Direction)   -- troca de posicao com a planta (ou espaco vazio) no tile
                               -- adjacente na direcao especificada. O drone vai para o tile
                               -- alvo e a planta vai para a posicao anterior do drone.

droneV2.goto(x, z)             -- comanda o drone para viajar diretamente para as
                               -- coordenadas X e Z especificadas na grade
```

### Proteção de Plantas
```lua
droneV2.lock()       -- bloqueia a planta diretamente abaixo do drone.
                     -- Plantas bloqueadas sao IMOVEIS e PROTEGIDAS.
                     -- Nao podem ser colhidas, cortadas ou trocadas.
                     -- Funcoes como canCrop() e canHarvest() sempre retornam false.

droneV2.unlock()     -- desbloqueia a planta abaixo do drone,
                     -- restaurando sua capacidade de ser colhida, cortada ou trocada.

droneV2.isLocked()   -- verifica o status da planta abaixo do drone.
                     -- Retorna true se estiver bloqueada.
```

> Uma planta bloqueada está totalmente protegida de qualquer alteração física ou coleta.

### Sensores de Gear e Solo
Esses comandos permitem ao drone ler informações detalhadas sobre gears (máquinas) implantadas e buffs de solo no tile atual.

```lua
droneV2.getGear()            -- retorna um objeto completo com todos os detalhes da maquina
                             -- e dados de buff de solo no tile

droneV2.hasGear()            -- retorna true/false indicando se uma maquina esta
                             -- posicionada no tile atual

droneV2.getGearName()        -- retorna o nome especifico do gear no tile

droneV2.getGearDuration()    -- retorna a duracao ativa restante do gear em segundos

droneV2.getFertilizer()      -- retorna objeto com 'Duration' (duracao) e 'Multi'
                             -- (multiplicador de eficacia) para o buff de fertilizante no tile

droneV2.getManualWater()     -- retorna objeto com 'Duration' e 'Multi'
                             -- para o buff de agua manual no tile

droneV2.getMachineWater()    -- retorna objeto com 'Duration' e 'Multi'
                             -- para o buff de agua de maquina no tile

droneV2.getLightning()       -- retorna os segundos restantes do efeito de protecao
                             -- do para-raios no tile
```

---

## 21. Biblioteca: `player`

### Inventário
```lua
player.getFruitCount()        -- total de tipos de fruta no inventario
player.getFruitCapacity()     -- capacidade maxima de frutas
player.getInventorySize()     -- total de tipos de itens (frutas + sementes)
player.getInventory()         -- inventario completo como lista
player.getItem(index)         -- detalhes do item no indice especificado
```

### Estatísticas
```lua
player.getTileNumber()                -- nivel de expansao do terreno
player.calculateFinalScrap(basePrice) -- valor real de venda com multiplicadores ativos
player.scrap()                        -- scrap atual do jogador como numero
```

### Notificações
```lua
player.alert(message)          -- alerta vermelho na tela
player.sent(message, color?)   -- mensagem normal (cor opcional)
```

### Eventos
```lua
player.chatted   -- dispara ao digitar no chat (recebe: string)
player.input     -- dispara ao pressionar tecla (recebe: Enum.KeyCode)
```

### Localização
```lua
player.getCurrentTile()   -- (X, Z) do tile onde o jogador esta. null se fora da fazenda
```

### Câmera
```lua
player.camera(Enum.Camera)         -- alvo da camera (Player, Drone, World, Base)
player.cameraMode(Enum.CameraMode) -- comportamento (Classic, Follow, Stable, Attach, Fixed)
```

---

## 22. Biblioteca: `playerV2`

O módulo Player V2 introduz eventos avançados para interações de mercado, sistema de presente diário e funções utilitárias avançadas. Acessado com o prefixo `playerV2`.

### Presentes Diários
```lua
playerV2.getGift()   -- tenta resgatar o presente de login diario.
                     -- Exibe uma notificacao e abre o menu de presente se bem-sucedido.
```

### Leaderboard
```lua
playerV2.getScrapLeaderboardRank()   -- retorna sua posicao atual no Top 50 do Leaderboard
                                     -- de Scrap como numero.
                                     -- Retorna null se voce nao estiver no top 50.
```

### Interface e Teleporte
```lua
playerV2.mainScreenEnable(Boolean)   -- habilita (true) ou desabilita (false) a UI
                                     -- principal da tela

playerV2.tpToDrone()                 -- teleporta instantaneamente seu personagem para
                                     -- a posicao atual do drone

playerV2.distanceToDrone()           -- retorna a distancia numerica entre seu personagem
                                     -- e o drone
```

### Eventos de Mundo e Mercado
Esses eventos permitem que o script reaja a ações específicas do jogador no mundo do jogo e no mercado.

```lua
playerV2.clicked    -- dispara quando o jogador clica no mundo.
                    -- Parametros: Enum.ClickType, PositionX, PositionZ
                    -- IMPORTANTE: so dispara em tiles ou plantas proprias.
                    -- Retorna null para areas nao-proprias.

playerV2.boughtSeed -- dispara quando uma semente e comprada.
                    -- Retorna o Enum.Seed comprado como parametro.

playerV2.boughtGear -- dispara quando um gear e comprado.
                    -- Retorna o Enum.Gear comprado como parametro.
```

**Exemplo de uso:**
```lua
playerV2.clicked:connect(func(clickType, posX, posZ)
    print("Clicou em:", posX, posZ, "com", clickType)
end)

playerV2.boughtSeed:connect(func(seed)
    print("Semente comprada:", seed)
end)
```

---

## 23. Biblioteca: `market`

### Sementes
```lua
market.getSeedStockTime()         -- segundos ate proximo restock
market.getSeedStock()             -- estoque atual como lista {Seed, Amount}
market.getSeedPrice(Enum.Seed)    -- preco de compra da semente
market.buySeed(Enum.Seed)         -- compra semente (retorna true se ok)
```

### Venda
```lua
market.whatValue(inventorySlot)   -- valor de venda do item no slot (frutas: dinamico por peso)
market.sellItem(inventorySlot)    -- vende item no slot especifico
market.sellAllItem()              -- vende todos os itens valaveis de uma vez
```

> `market.sellAll()` NÃO EXISTE. O correto é `market.sellAllItem()`.

### Eventos
```lua
market.changedSeedStock   -- dispara quando seeds sao reabastecidas
market.changedGearStock   -- dispara quando gears sao reabastecidas
```

---

## 24. Biblioteca: `game`

### Clima
```lua
game.weather()          -- clima atual como string ("Rain", "Thunder", ...)
game.weatherDuration()  -- segundos restantes do clima atual
game.changedWeather     -- evento: dispara ao mudar o clima
```

### Servidor
```lua
game.uptime      -- segundos de uptime do servidor (propriedade, sem parenteses)
game.version     -- versao atual do jogo (propriedade)
game.lauversion  -- versao do engine Lau (propriedade)
game.players()   -- lista de jogadores conectados {Name, DisplayName, Scrap}
game.rejoin()    -- desconecta e reconecta ao mesmo servidor
```

---

## 25. Sistema de Coordenadas

```
Centro do mundo: (0, 0)
X positivo = Leste   |   X negativo = Oeste
Z positivo = Sul     |   Z negativo = Norte

Expansao maxima: 13 upgrades
Area total maxima: 729 tiles (27 x 27)
```

---

## 26. Regras Críticas — Resumo

| Regra | Detalhe |
|---|---|
| `AND` `OR` `NOT` | SEMPRE maiúsculo |
| Blocos | Todo `then`/`do` precisa de `end` |
| Múltiplas operações | Obrigatório usar nova linha |
| Atualizar lista | `list.prop = val` (sem `varol`) |
| Módulo `.laum` | DEVE terminar com `return` |
| `while` | SEMPRE usar `task.wait()` dentro |
| `:connect` em `while` | PROIBIDO — usar `:once` ou mover pra fora |
| Eventos simultâneos | Máximo 10 |
| `drone.water()` | Não existe — usar `drone.useItem(Enum.Gear.Watering_Can)` |
| `market.sellAll()` | Não existe — usar `market.sellAllItem()` |
| `#` em número | Raiz quadrada |
| `#` em lista/string | Comprimento |
| Planta bloqueada | `droneV2.lock()` — canCrop/canHarvest sempre false |
| `playerV2.clicked` | Só dispara em tiles/plantas próprias |
