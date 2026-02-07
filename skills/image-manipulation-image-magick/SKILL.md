---
name: image-manipulation-image-magick
description: Processe e manipule imagens usando ImageMagick. Suporta redimensionamento, conversao de formato, processamento em lote e obtencao de metadados de imagens. Use ao trabalhar com imagens, criar thumbnails, redimensionar wallpapers ou executar operacoes em lote.
compatibility: Requer ImageMagick instalado e disponivel como `magick` no PATH. Exemplos cross-platform para PowerShell (Windows) e Bash (Linux/macOS).
---

# Manipulacao de Imagens com ImageMagick

Esta skill habilita tarefas de processamento e manipulacao de imagens usando ImageMagick
em sistemas Windows, Linux e macOS.

## Quando Usar Esta Skill

Use esta skill quando precisar:

- Redimensionar imagens (unica ou em lote)
- Obter dimensoes e metadados de imagens
- Converter entre formatos de imagem
- Criar thumbnails
- Processar wallpapers para diferentes tamanhos de tela
- Processar em lote multiplas imagens com criterios especificos

## Pre-requisitos

- ImageMagick instalado no sistema
- **Windows**: PowerShell com ImageMagick disponivel como `magick` (ou em `C:\Program Files\ImageMagick-*\magick.exe`)
- **Linux/macOS**: Bash com ImageMagick instalado via package manager (`apt`, `brew`, etc.)

## Capacidades Principais

### 1. Informacoes da Imagem

- Obter dimensoes da imagem (largura x altura)
- Recuperar metadados detalhados (formato, espaco de cor, etc.)
- Identificar formato da imagem

### 2. Redimensionamento de Imagens

- Redimensionar imagens individuais
- Redimensionar em lote varias imagens
- Criar thumbnails com dimensoes especificas
- Manter proporcoes

### 3. Processamento em Lote

- Processar imagens com base em dimensoes
- Filtrar e processar tipos de arquivo especificos
- Aplicar transformacoes em multiplos arquivos

## Exemplos de Uso

### Exemplo 0: Resolver executavel `magick`

**PowerShell (Windows):**
```powershell
# Prefer ImageMagick on PATH
$magick = (Get-Command magick -ErrorAction SilentlyContinue)?.Source

# Fallback: common install pattern under Program Files
if (-not $magick) {
    $magick = Get-ChildItem "C:\Program Files\ImageMagick-*\magick.exe" -ErrorAction SilentlyContinue |
        Select-Object -First 1 -ExpandProperty FullName
}

if (-not $magick) {
    throw "ImageMagick not found. Install it and/or add 'magick' to PATH."
}
```

**Bash (Linux/macOS):**
```bash
# Check if magick is available on PATH
if ! command -v magick &> /dev/null; then
    echo "ImageMagick not found. Install it using your package manager:"
    echo "  Ubuntu/Debian: sudo apt install imagemagick"
    echo "  macOS: brew install imagemagick"
    exit 1
fi
```

### Exemplo 1: Obter Dimensoes da Imagem

**PowerShell (Windows):**
```powershell
# For a single image
& $magick identify -format "%wx%h" path/to/image.jpg

# For multiple images
Get-ChildItem "path/to/images/*" | ForEach-Object { 
    $dimensions = & $magick identify -format "%f: %wx%h`n" $_.FullName
    Write-Host $dimensions 
}
```

**Bash (Linux/macOS):**
```bash
# For a single image
magick identify -format "%wx%h" path/to/image.jpg

# For multiple images
for img in path/to/images/*; do
    magick identify -format "%f: %wx%h\n" "$img"
done
```

### Exemplo 2: Redimensionar Imagens

**PowerShell (Windows):**
```powershell
# Resize a single image
& $magick input.jpg -resize 427x240 output.jpg

# Batch resize images
Get-ChildItem "path/to/images/*" | ForEach-Object { 
    & $magick $_.FullName -resize 427x240 "path/to/output/thumb_$($_.Name)"
}
```

**Bash (Linux/macOS):**
```bash
# Resize a single image
magick input.jpg -resize 427x240 output.jpg

# Batch resize images
for img in path/to/images/*; do
    filename=$(basename "$img")
    magick "$img" -resize 427x240 "path/to/output/thumb_$filename"
done
```

### Exemplo 3: Obter Informacoes Detalhadas da Imagem

**PowerShell (Windows):**
```powershell
# Get verbose information about an image
& $magick identify -verbose path/to/image.jpg
```

**Bash (Linux/macOS):**
```bash
# Get verbose information about an image
magick identify -verbose path/to/image.jpg
```

### Exemplo 4: Processar Imagens com Base em Dimensoes

**PowerShell (Windows):**
```powershell
Get-ChildItem "path/to/images/*" | ForEach-Object { 
    $dimensions = & $magick identify -format "%w,%h" $_.FullName
    if ($dimensions) {
        $width,$height = $dimensions -split ','
        if ([int]$width -eq 2560 -or [int]$height -eq 1440) {
            Write-Host "Processing $($_.Name)"
            & $magick $_.FullName -resize 427x240 "path/to/output/thumb_$($_.Name)"
        }
    }
}
```

**Bash (Linux/macOS):**
```bash
for img in path/to/images/*; do
    dimensions=$(magick identify -format "%w,%h" "$img")
    if [[ -n "$dimensions" ]]; then
        width=$(echo "$dimensions" | cut -d',' -f1)
        height=$(echo "$dimensions" | cut -d',' -f2)
        if [[ "$width" -eq 2560 || "$height" -eq 1440 ]]; then
            filename=$(basename "$img")
            echo "Processing $filename"
            magick "$img" -resize 427x240 "path/to/output/thumb_$filename"
        fi
    fi
done
```

## Diretrizes

1. **Sempre cite caminhos de arquivo** - Use aspas ao redor de caminhos que possam conter espacos
2. **Use o operador `&` (PowerShell)** - Invoque o executavel magick usando `&` no PowerShell
3. **Guarde o caminho em variavel (PowerShell)** - Atribua o caminho do ImageMagick a `$magick` para codigo mais limpo
4. **Envolva em loops** - Ao processar multiplos arquivos, use `ForEach-Object` (PowerShell) ou loops `for` (Bash)
5. **Verifique dimensoes primeiro** - Cheque dimensoes antes de processar para evitar operacoes desnecessarias
6. **Use flags de resize apropriadas** - Considere usar `!` para dimensoes exatas ou `^` para dimensoes minimas

## Padroes Comuns

### Padroes PowerShell

#### Padrao: Armazenar caminho do ImageMagick

```powershell
$magick = (Get-Command magick).Source
```

#### Padrao: Obter dimensoes como variaveis

```powershell
$dimensions = & $magick identify -format "%w,%h" $_.FullName
$width,$height = $dimensions -split ','
```

#### Padrao: Processamento condicional

```powershell
if ([int]$width -gt 1920) {
    & $magick $_.FullName -resize 1920x1080 $outputPath
}
```

#### Padrao: Criar thumbnails

```powershell
& $magick $_.FullName -resize 427x240 "thumbnails/thumb_$($_.Name)"
```

### Padroes Bash

#### Padrao: Checar instalacao do ImageMagick

```bash
command -v magick &> /dev/null || { echo "ImageMagick required"; exit 1; }
```

#### Padrao: Obter dimensoes como variaveis

```bash
dimensions=$(magick identify -format "%w,%h" "$img")
width=$(echo "$dimensions" | cut -d',' -f1)
height=$(echo "$dimensions" | cut -d',' -f2)
```

#### Padrao: Processamento condicional

```bash
if [[ "$width" -gt 1920 ]]; then
    magick "$img" -resize 1920x1080 "$outputPath"
fi
```

#### Padrao: Criar thumbnails

```bash
filename=$(basename "$img")
magick "$img" -resize 427x240 "thumbnails/thumb_$filename"
```

## Limitacoes

- Operacoes em lote grandes podem consumir muita memoria
- Algumas operacoes complexas podem exigir delegates adicionais do ImageMagick
- Em sistemas Linux antigos, use `convert` em vez de `magick` (ImageMagick 6.x vs 7.x)
