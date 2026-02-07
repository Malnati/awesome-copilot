# Ferramentas de Biblioteca do Excalidraw

Este diretorio contem scripts para trabalhar com bibliotecas do Excalidraw.

## split-excalidraw-library.py

Divide um arquivo de biblioteca Excalidraw (`*.excalidrawlib`) em arquivos JSON individuais de icones para uso eficiente de tokens por assistentes de IA.

### Pre-requisitos

- Python 3.6 ou superior
- Sem dependencias adicionais (usa apenas a standard library)

### Uso

```bash
python split-excalidraw-library.py <path-to-library-directory>
```

### Fluxo Passo a Passo

1. **Criar diretorio da biblioteca**:
   ```bash
   mkdir -p skills/excalidraw-diagram-generator/libraries/aws-architecture-icons
   ```

2. **Baixar e posicionar o arquivo de biblioteca**:
   - Visite: https://libraries.excalidraw.com/
   - Busque por "AWS Architecture Icons" e baixe o arquivo `.excalidrawlib`
   - Renomeie para combinar com o nome do diretorio: `aws-architecture-icons.excalidrawlib`
   - Coloque no diretorio criado no passo 1

3. **Executar o script**:
   ```bash
   python skills/excalidraw-diagram-generator/scripts/split-excalidraw-library.py skills/excalidraw-diagram-generator/libraries/aws-architecture-icons/
   ```

### Estrutura de Saida

O script cria a seguinte estrutura no diretorio da biblioteca:

```
skills/excalidraw-diagram-generator/libraries/aws-architecture-icons/
  aws-architecture-icons.excalidrawlib  # Original file (kept)
  reference.md                          # Generated: Quick reference table
  icons/                                # Generated: Individual icon files
    API-Gateway.json
    CloudFront.json
    EC2.json
    S3.json
    ...
```

### O que o Script Faz

1. **Le** o arquivo `.excalidrawlib`
2. **Extrai** cada icone do array `libraryItems`
3. **Sanitiza** nomes de icone para criar nomes de arquivo validos (espacos → hifens, remove caracteres especiais)
4. **Salva** cada icone como um arquivo JSON separado no diretorio `icons/`
5. **Gera** um arquivo `reference.md` com uma tabela mapeando nomes de icone para nomes de arquivo

### Beneficios

- **Eficiencia de Tokens**: a IA pode ler primeiro o `reference.md` leve para encontrar icones relevantes e depois carregar apenas os arquivos de icone necessarios
- **Organizacao**: icones organizados em uma estrutura de diretorios clara
- **Extensibilidade**: usuarios podem adicionar multiplos conjuntos de bibliotecas lado a lado

### Fluxo Recomendado

1. Baixe bibliotecas Excalidraw desejadas em https://libraries.excalidraw.com/
2. Execute este script em cada arquivo de biblioteca
3. Mova as pastas geradas para `../libraries/`
4. O assistente de IA usara arquivos `reference.md` para localizar e usar icones com eficiencia

### Fontes de Bibliotecas (Exemplos — verifique disponibilidade)

- Exemplos encontrados em https://libraries.excalidraw.com/ podem incluir conjuntos de icones de cloud/servicos.
- A disponibilidade muda ao longo do tempo; verifique os nomes exatos das bibliotecas no site antes de usar.
- Este script funciona com qualquer arquivo `.excalidrawlib` valido que voce fornecer.

### Solucao de Problemas

**Erro: Arquivo nao encontrado**
- Verifique se o caminho do arquivo esta correto
- Garanta que o arquivo tem extensao `.excalidrawlib`

**Erro: Formato de arquivo de biblioteca invalido**
- Garanta que o arquivo e uma biblioteca Excalidraw valida
- Verifique se contem o array `libraryItems`

### Consideracoes de Licenca

Ao usar bibliotecas de icones de terceiros:
- **AWS Architecture Icons**: Sujeito a AWS Content License
- **GCP Icons**: Sujeito aos termos do Google
- **Outras bibliotecas**: Check each library's license

Este script e para uso pessoal/organizacional. Redistribuicao de arquivos de icones divididos deve cumprir os termos de licenca da biblioteca original.

## add-icon-to-diagram.py

Adiciona um icone especifico de uma biblioteca Excalidraw dividida a um diagrama `.excalidraw` existente. O script lida com traducao de coordenadas e evita colisao de IDs, e pode opcionalmente adicionar um rotulo abaixo do icone.

### Pre-requisitos

- Python 3.6 ou superior
- Um arquivo de diagrama (`.excalidraw`)
- Um diretorio de biblioteca de icones divididos (criado por `split-excalidraw-library.py`)

### Uso

```bash
python add-icon-to-diagram.py <diagram-path> <icon-name> <x> <y> [OPTIONS]
```

**Opcoes**
- `--library-path PATH` : Path to the icon library directory (default: `aws-architecture-icons`)
- `--label TEXT` : Adicionar um rotulo de texto abaixo do icone
-- `--use-edit-suffix` : Edit via `.excalidraw.edit` to avoid editor overwrite issues (enabled by default; pass `--no-use-edit-suffix` to disable)

### Exemplos

```bash
# Add EC2 icon at position (400, 300)
python add-icon-to-diagram.py diagram.excalidraw EC2 400 300

# Add VPC icon with label
python add-icon-to-diagram.py diagram.excalidraw VPC 200 150 --label "VPC"

# Safe edit mode is enabled by default (avoids editor overwrite issues)
# Use `--no-use-edit-suffix` to disable
python add-icon-to-diagram.py diagram.excalidraw EC2 500 300

# Add icon from another library
python add-icon-to-diagram.py diagram.excalidraw Compute-Engine 500 200 \
   --library-path libraries/gcp-icons --label "API Server"
```

### O que o Script Faz

1. **Loads** o JSON do icone a partir do diretorio `icons/` da biblioteca
2. **Calcula** o bounding box do icone
3. **Desloca** todas as coordenadas para a posicao alvo
4. **Gera** IDs unicos para todos os elementos e grupos
5. **Anexa** os elementos transformados ao diagrama
6. **(Optional)** Adiciona um rotulo abaixo do icone

---

## add-arrow.py

Adiciona uma seta reta entre dois pontos em um diagrama `.excalidraw` existente. Suporta rotulos opcionais e estilos de linha.

### Pre-requisitos

- Python 3.6 ou superior
- Um arquivo de diagrama (`.excalidraw`)

### Uso

```bash
python add-arrow.py <diagram-path> <from-x> <from-y> <to-x> <to-y> [OPTIONS]
```

**Opcoes**
- `--style {solid|dashed|dotted}` : Estilo de linha (default: `solid`)
- `--color HEX` : Cor da seta (default: `#1e1e1e`)
- `--label TEXT` : Adicionar um rotulo de texto na seta
-- `--use-edit-suffix` : Edit via `.excalidraw.edit` to avoid editor overwrite issues (enabled by default; pass `--no-use-edit-suffix` to disable)

### Exemplos

```bash
# Simple arrow
python add-arrow.py diagram.excalidraw 300 200 500 300

# Arrow with label
python add-arrow.py diagram.excalidraw 300 200 500 300 --label "HTTPS"

# Dashed arrow with custom color
python add-arrow.py diagram.excalidraw 400 350 600 400 --style dashed --color "#7950f2"

# Safe edit mode is enabled by default (avoids editor overwrite issues)
# Use `--no-use-edit-suffix` to disable
python add-arrow.py diagram.excalidraw 300 200 500 300
```

### O que o Script Faz

1. **Cria** um elemento de seta a partir das coordenadas fornecidas
2. **(Optional)** Adiciona um rotulo proximo ao ponto medio da seta
3. **Anexa** os elementos ao diagrama
4. **Salva** o arquivo atualizado