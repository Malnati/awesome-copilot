---
name: plantuml-ascii
description: "Gere diagramas ASCII art usando PlantUML em modo texto. Use quando o usuario pedir diagramas ASCII, diagramas baseados em texto, diagramas amigaveis para terminal, ou mencionar plantuml ascii, text diagram, ascii art diagram. Suporta: Converter diagramas PlantUML para ASCII art, Criar diagramas de sequencia, classe e fluxogramas em formato ASCII, Gerar ASCII art com Unicode usando a flag -utxt"
license: MIT
allowed-tools: Bash, Write, Read
---

# Gerador de Diagramas ASCII Art com PlantUML

## Visao Geral

Crie diagramas ASCII art baseados em texto usando PlantUML. Perfeito para documentacao em ambientes de terminal, arquivos README, emails ou qualquer cenario onde diagramas graficos nao sao adequados.

## O que e ASCII Art do PlantUML?

PlantUML pode gerar diagramas como texto puro (ASCII art) em vez de imagens. Isso e util para:

- Workflows baseados em terminal
- Commits/PRs no Git sem suporte a imagens
- Documentacao que precisa de versionamento
- Ambientes onde ferramentas graficas nao estao disponiveis

## Instalacao

```bash
# macOS
brew install plantuml

# Linux (varia por distro)
sudo apt-get install plantuml  # Ubuntu/Debian
sudo yum install plantuml      # RHEL/CentOS

# Or download JAR directly
wget https://github.com/plantuml/plantuml/releases/download/v1.2024.0/plantuml-1.2024.0.jar
```

## Formatos de Saida

| Flag    | Formato        | Descricao                          |
| ------- | ------------- | ------------------------------------ |
| `-txt`  | ASCII         | Caracteres ASCII puros               |
| `-utxt` | Unicode ASCII | Melhorado com caracteres de box-drawing |

## Workflow Basico

### 1. Criar Arquivo de Diagrama PlantUML

```plantuml
@startuml
participant Bob
actor Alice

Bob -> Alice : hello
Alice -> Bob : Is it ok?
@enduml
```

### 2. Gerar ASCII Art

```bash
# Standard ASCII output
plantuml -txt diagram.puml

# Unicode-enhanced output (better looking)
plantuml -utxt diagram.puml

# Using JAR directly
java -jar plantuml.jar -txt diagram.puml
java -jar plantuml.jar -utxt diagram.puml
```

### 3. Ver a Saida

A saida e salva como `diagram.atxt` (ASCII) ou `diagram.utxt` (Unicode).

## Tipos de Diagramas Suportados

### Diagrama de Sequencia

```plantuml
@startuml
actor User
participant "Web App" as App
database "Database" as DB

User -> App : Login Request
App -> DB : Validate Credentials
DB --> App : User Data
App --> User : Auth Token
@enduml
```

### Diagrama de Classes

```plantuml
@startuml
class User {
  +id: int
  +name: string
  +email: string
  +login(): bool
}

class Order {
  +id: int
  +total: float
  +items: List
  +calculateTotal(): float
}

User "1" -- "*" Order : places
@enduml
```

### Diagrama de Atividades

```plantuml
@startuml
start
:Initialize;
if (Is Valid?) then (yes)
  :Process Data;
  :Save Result;
else (no)
  :Log Error;
  stop
endif
:Complete;
stop
@enduml
```

### Diagrama de Estados

```plantuml
@startuml
[*] --> Idle
Idle --> Processing : start
Processing --> Success : complete
Processing --> Error : fail
Success --> [*]
Error --> Idle : retry
@enduml
```

### Diagrama de Componentes

```plantuml
@startuml
[Client] as client
[API Gateway] as gateway
[Service A] as svcA
[Service B] as svcB
[Database] as db

client --> gateway
gateway --> svcA
gateway --> svcB
svcA --> db
svcB --> db
@enduml
```

### Diagrama de Caso de Uso

```plantuml
@startuml
actor "User" as user
actor "Admin" as admin

rectangle "System" {
  user -- (Login)
  user -- (View Profile)
  user -- (Update Settings)
  admin -- (Manage Users)
  admin -- (Configure System)
}
@enduml
```

### Diagrama de Deployment

```plantuml
@startuml
actor "User" as user
node "Load Balancer" as lb
node "Web Server 1" as ws1
node "Web Server 2" as ws2
database "Primary DB" as db1
database "Replica DB" as db2

user --> lb
lb --> ws1
lb --> ws2
ws1 --> db1
ws2 --> db1
db1 --> db2 : replicate
@enduml
```
