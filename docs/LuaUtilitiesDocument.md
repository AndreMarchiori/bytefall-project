# Documentação do Module Script Utilities

## Localização

```text
ReplicatedStorage
    └── SwarmGame
        └── Modules
            └── Utilities.lua
```
___
### Geral

O módulo Utilities fornece funções auxiliares para todos os sistemas do jogo, organizadas em categorias para fácil acesso e manutenção.

```luau
-- Em outros módulos:
local Utilities = require(ReplicatedStorage.SwarmGame.Modules.Utilities)

-- Exemplo 1: Calcular distância
local distance = Utilities:CalculateDistance(playerPosition, enemyPosition)

-- Exemplo 2: Debounce para habilidades
if Utilities:CreateDebounce("AbilityCooldown", 2.0) then
    -- Executar habilidade
end

-- Exemplo 3: Log de debug
Utilities:DebugLog("Jogador usou habilidade", "INFO")

-- Exemplo 4: Buscar recursiva
local part = Utilities:FindFirstChildRecursive(workspace, "SpecialPart")

-- Exemplo 5: Performance
local memoizedCalculation = Utilities:Memoize(function(x, y)
    return complexCalculation(x, y)
end)
```

# Funções
## Categorias

### 1. Funções Matemáticas e Cálculos
### ```Utilities:Clamp(value, min, max)```  
Descrição: Limita um valor entre um mínimo e máximo
Parâmetros:

- value: número a ser limitado

- min: valor mínimo

- max: valor máximo  
**Retorno:** Valor limitado entre min e max  
**Exemplo:**

```luau
local clamped = Utilities:Clamp(15, 0, 10) -- Retorna 10
```
### ```Utilities:Lerp(a, b, t)```

**Descrição:** Interpolação linear entre dois valores  
**Parâmetros:**  

-```a```: valor inicial

-```b```: valor final

-```t```: fator de interpolação (0-1)  

**Retorno:** Valor interpolado  
**Exemplo:**  
```luau
local result = Utilities:Lerp(0, 100, 0.5) -- Retorna 50
```

### ```Utilities:RandomWeightedChoice(choices)```

Descrição: Escolha aleatória com pesos  
Parâmetros:  

- ```choices```: tabela com opções e pesos ```{{value = "A", weight = 2}, {value = "B", weight = 1}}```  
Retorno: Valor escolhido  
Exemplo:  

```luau
local choice = Utilities:RandomWeightedChoice({
    {value = "Common", weight = 70},
    {value = "Rare", weight = 25},
    {value = "Epic", weight = 5}
})
```
___
### 2. Manipulação de tabelas
### ```Utilities:DeepCopy(original)```

Descrição: Cria uma cópia profunda de uma tabela  
Parâmetros:  

-- ```original```: tabela original  
Retorno: Cópia profunda da tabela  
Exemplo:  

```luau
local original = {a = 1, b = {c = 2}}
local copy = Utilities:DeepCopy(original)
```

### ```Utilities:FilterTable(t, predicate)```

**Descrição:** Filtra tabela baseado em predicate  
**Parâmetros:**  

- ```t:``` tabela a filtrar

- ```predicate:``` função function(value, key) return boolean end  
**Retorno**: Tabela filtrada  
**Exemplo**:  

```luau
local filtered = Utilities:FilterTable(items, function(item)
    return item.rarity == "Epic"
end)
```

### ```Utilities:FindInTable(t, predicate)```

**Descrição:** Encontra primeiro item que satisfaz predicate  
**Parâmetros:**  

- ```t:``` tabela para buscar

- ```predicate:``` função de condição  
**Retorno:** Valor e chave encontrados ou nil  
**Exemplo:**  

```luau
local item, key = Utilities:FindInTable(players, function(player)
    return player.health < 0.3
end)
```
___
### 3. Funções de Tempo e Delay
### ```Utilities:CreateDebounce(name, cooldown)```

**Descrição:** Cria um debounce para evitar spam de ações  
**Parâmetros:**  

- ```name:``` identificador único do debounce

- ```cooldown:``` tempo de cooldown em segundos  
**Retorno:** boolean - true se pode executar  
**Exemplo:**  

```luau
if Utilities:CreateDebounce("Attack", 0.5) then
    -- Executar ataque
end
```

### ```Utilities:WaitForCondition(conditionCallback, timeout, checkInterval)```

**Descrição:** Espera até condition ser verdadeira  
**Parâmetros:**  

- ```conditionCallback:``` função que retorna boolean

- ```timeout:``` tempo máximo de espera (opcional)

- ```checkInterval:``` intervalo de verificação (opcional)  
**Retorno:** boolean - true se condition foi satisfeita  
**Exemplo:**  

```luau
local success = Utilities:WaitForCondition(function()
    return player.Character ~= nil
end, 5, 0.1)
```
___
### 4. Funções de Instância e Objetos
### ```Utilities:FindFirstChildRecursive(parent, name)```

**Descrição:** Busca recursiva por child pelo nome  
**Parâmetros:**   

- ```parent:``` instância pai

- ```name:``` nome do child procurado  
**Retorno:** Instância encontrada ou nil  
**Exemplo:**  

```luau
local weapon = Utilities:FindFirstChildRecursive(character, "Sword")
```
### ```Utilities:WaitForChildSafe(parent, childName, timeout)```

**Descrição:** Espera por child com timeout seguro  
**Parâmetros:**  

- ```parent:``` instância pai

- ```childName:``` nome do child

- ```timeout:``` tempo máximo de espera (opcional)  
**Retorno:** Instância encontrada ou nil  
**Exemplo:**  

```luau
local humanoid = Utilities:WaitForChildSafe(character, "Humanoid", 3)
```
___
### 5. Funções de Performance
### ```Utilities:Memoize(func)```

**Descrição:** Memoização de função para melhor performance
**Parâmetros:**

- ```func:``` função a ser memoizada
**Retorno:** Função memoizada
**Exemplo:**

```luau
local expensiveCalc = Utilities:Memoize(function(x, y)
    return complexMath(x, y)
end)

-- Primeira chamada: calcula
local result1 = expensiveCalc(10, 20)

-- Segunda chamada com mesmos parâmetros: retorna cache
local result2 = expensiveCalc(10, 20)
```

### ```Utilities:ProfileFunction(func, ...)```

**Descrição:** Mede tempo de execução de função
**Parâmetros:**

- ```func:``` função a ser profileada

- ```...:``` parâmetros da função
**Retorno:** Tabela com resultados e tempo de execução
**Exemplo:**

```luau
local profile = Utilities:ProfileFunction(complexFunction, param1, param2)
print("Tempo de execução:", profile.executionTime)
```
___
### 6. Funções de Geometria e Física
### ```Utilities:FindPartsInSphere(position, radius, overlapParams)```

**Descrição:** Encontra partes dentro de esfera
**Parâmetros:**

- ```position:``` centro da esfera

- ```radius:``` raio da esfera

- ```overlapParams:``` parâmetros de overlap (opcional)
**Retorno:** Array de partes encontradas
**Exemplo:**

```luau
local parts = Utilities:FindPartsInSphere(Vector3.new(0, 5, 0), 10)
```

### ```Utilities:GetPlayersInRadius(position, radius)```

**Descrição:** Encontra jogadores dentro do raio
**Parâmetros:**

- ```position:``` posição central

- ```radius:``` raio de busca
**Retorno:** Array de jogadores
**Exemplo:**

```luau
local nearbyPlayers = Utilities:GetPlayersInRadius(explosionPos, 15)
```
__
### 7. Funções de Serialização de Dados
### ```Utilities:SerializeTable(t)```

**Descrição:** Serializa tabela para JSON string
**Parâmetros:**

- ```t:``` tabela a serializar
**Retorno:** String JSON
**Exemplo:**

```luau
local json = Utilities:SerializeTable(playerData)
```
### ```Utilities:DeserializeTable(json)```

**Descrição:** Desserializa JSON string para tabela
**Parâmetros:**

- ```json:``` string JSON
**Retorno:** Tabela ou nil em caso de erro
**Exemplo:**

```luau
local data = Utilities:DeserializeTable(jsonString)
```
___
### 8. Funções de Debug e Logging
### ```Utilities:DebugLog(message, level)```

**Descrição:** Log de debug com timestamp e nível
**Parâmetros:**

- ```message:``` mensagem a logar

- ```level:``` nível de log ("INFO", "WARN", "ERROR")
**Exemplo:**

```luau
Utilities:DebugLog("Jogador coletou item raro", "INFO")
```

### ```Utilities:PrintTable(t, indent)```

**Descrição:** Print formatado de tabela para debug
**Parâmetros:**

- ```t:``` tabela a printar

- ```indent:``` indentação inicial (opcional)
**Exemplo:**

```luau
Utilities:PrintTable(playerAttributes, 2)
```

# Inicialização e Uso
### Importação do Módulo
```luau
local Utilities = require(ReplicatedStorage.SwarmGame.Modules.Utilities)
```

### Inicialização (Automática)
```luau
Utilities:Initialize() -- Chamado automaticamente
```

### Exemplo de uso completo
```luau
-- Encontrar jogadores próximos e aplicar efeito
local centerPosition = character.HumanoidRootPart.Position
local nearbyPlayers = Utilities:GetPlayersInRadius(centerPosition, 10)

for _, player in ipairs(nearbyPlayers) do
    if Utilities:IsInRange(centerPosition, player.Character.HumanoidRootPart.Position, 8) then
        -- Aplicar buff com debounce
        if Utilities:CreateDebounce("ApplyBuff_" .. player.Name, 1.0) then
            applyBuffToPlayer(player)
            Utilities:DebugLog("Buff aplicado para " .. player.Name, "INFO")
        end
    end
end
```

# Configuração
As funções de debug são controladas por GameSettings.lua:
```luau
DEBUG_MODE = true        -- Habilita/desabilita logs
DEBUG_TO_CLIENTS = false -- Envia logs para clients
PRELOAD_UTILITIES = true -- Pré-carrega funções comuns
```

# Shutdown
```luau
Utilities:Shutdown() -- Limpa cache e prepara para shutdown
```
