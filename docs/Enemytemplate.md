# Estrutura
```text
ServerStorage
    └── SwarmGame
        └── EnemyTemplates
            └── Basic (Model)
                ├── Humanoid (Humanoid)
                ├── HumanoidRootPart (Part)
                ├── Head (Part)
                ├── Torso (Part)
                ├── LeftArm (Part)
                ├── RightArm (Part)
                ├── LeftLeg (Part)
                ├── RightLeg (Part)
                ├── EnemyScript (Script) - Opcional
                ├── AnimationController (AnimationController)
                ├── Animations (Folder)
                └── Configuration (Folder)
```
## 1. Estrutura do Model:
```luau
-- ServerStorage.SwarmGame.EnemyTemplates.Basic
-- Este é um Model contendo:

Humanoid:
  - MaxHealth: 100
  - Health: 100
  - WalkSpeed: 16
  - JumpPower: 50

HumanoidRootPart:
  - Name: "HumanoidRootPart"
  - Anchored: false
  - CanCollide: true
  - Size: Vector3.new(2, 2, 1)
  - CustomPhysicalProperties: Personalizado para movimento

Head:
  - Size: Vector3.new(2, 1, 1)
  - Mesh: SpecialMesh com meshType "Head"
  - Decal: Face texture

-- Outras partes do corpo com meshes e textures apropriadas
```
## 2. Script de Comportamento (Opcional)
```luau
-- EnemyScript dentro do template
local EnemyScript = {}

local enemy = script.Parent
local humanoid = enemy:WaitForChild("Humanoid")
local rootPart = enemy:WaitForChild("HumanoidRootPart")

-- Configurações específicas deste tipo de inimigo
local CONFIG = {
    AttackRange = 5,
    AttackDamage = 15,
    AttackCooldown = 2.0,
    ChaseDistance = 30
}

function EnemyScript:OnAttack(target)
    -- Lógica de ataque específica para este inimigo
    if target and target:FindFirstChild("Humanoid") then
        target.Humanoid:TakeDamage(CONFIG.AttackDamage)
    end
end

-- Conexão com o Humanoid
humanoid.Died:Connect(function()
    -- Limpeza quando o inimigo morre
    wait(3)
    enemy:Destroy()
end)

return EnemyScript
```
## 3. Animações
```text
Animations (Folder)
    ├── Walk (Animation)
    ├── Run (Animation)
    ├── Attack (Animation)
    ├── Idle (Animation)
    └── Death (Animation)
```
## 4. Configuration Folder
```text
Configuration (Folder)
    ├── EnemyType (StringValue) - Valor: "Basic"
    ├── EXPValue (NumberValue) - Valor: 10
    ├── GoldValue (NumberValue) - Valor: 0
    └── DropChance (NumberValue) - Valor: 0.01
```

___
como o EnemyManager usa os templates:
```luau
function EnemyManager:SpawnEnemy(enemyType, level)
    local enemyData = EnemyData.GetEnemy(enemyType)
    local template = enemyTemplates:FindFirstChild(enemyType)
    
    if template then
        -- Clone o template para o workspace
        local enemy = template:Clone()
        enemy.Parent = Workspace
        
        -- Posicione no spawn point
        enemy:SetPrimaryPartCFrame(spawnPoint.CFrame)
        
        -- Configure atributos baseados nos dados
        enemy:SetAttribute("IsEnemy", true)
        enemy:SetAttribute("EnemyType", enemyType)
        enemy:SetAttribute("Level", level)
        
        -- Aplique estatísticas escaladas
        self:SetupEnemyStats(enemy, enemyData, level)
        
        return enemy
    end
end
```

### Como Criar os Templates:

- Modelo Básico: Crie um R15 ou Rthro character no Roblox Studio

- Ajuste Estatísticas: Configure Humanoid properties

- Adicione Scripts: Comportamento específico se necessário

- Configure Animações: Para movimentos naturais

- Adicione Atributos: Para identificação e configuração
