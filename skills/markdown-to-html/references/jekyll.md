# Referencia de Jekyll

Jekyll e um gerador de sites estaticos que transforma conteudo Markdown em sites completos. Ele e blog-aware e alimenta o GitHub Pages.

## Instalacao

### Pre-requisitos

- Ruby 2.7.0 ou superior
- RubyGems
- GCC e Make

### Instalar Jekyll

```bash
# Install Jekyll and Bundler
gem install jekyll bundler
```

### Instalacao por Plataforma

```bash
# macOS (install Xcode CLI tools first)
xcode-select --install
gem install jekyll bundler

# Ubuntu/Debian
sudo apt-get install ruby-full build-essential zlib1g-dev
gem install jekyll bundler

# Windows (use RubyInstaller)
# Download from https://rubyinstaller.org/
gem install jekyll bundler
```

## Inicio Rapido

### Criar Novo Site

```bash
# Create new Jekyll site
jekyll new myblog

# Navigate to site
cd myblog

# Build and serve
bundle exec jekyll serve

# Open http://localhost:4000
```

### Estrutura de Diretorios

```
myblog/
├── _config.yml      # Site configuration
├── _posts/          # Blog posts
│   └── 2025-01-28-welcome.md
├── _layouts/        # Page templates
├── _includes/       # Reusable components
├── _data/           # Data files (YAML, JSON, CSV)
├── _sass/           # Sass partials
├── assets/          # CSS, JS, images
├── index.md         # Home page
└── Gemfile          # Ruby dependencies
```

## Comandos de CLI

| Comando | Descricao |
|---------|-----------|
| `jekyll new <name>` | Create new site |
| `jekyll build` | Build to `_site/` |
| `jekyll serve` | Build and serve locally |
| `jekyll clean` | Remove generated files |
| `jekyll doctor` | Check for issues |

### Opcoes de Build

```bash
# Build site
bundle exec jekyll build

# Build with production environment
JEKYLL_ENV=production bundle exec jekyll build

# Build to custom directory
bundle exec jekyll build --destination ./public

# Build with incremental regeneration
bundle exec jekyll build --incremental
```

### Opcoes de Serve

```bash
# Serve with live reload
bundle exec jekyll serve --livereload

# Include draft posts
bundle exec jekyll serve --drafts

# Specify port
bundle exec jekyll serve --port 8080

# Bind to all interfaces
bundle exec jekyll serve --host 0.0.0.0
```

## Configuracao (_config.yml)

```yaml
# Site settings
title: My Blog
description: A great blog
baseurl: ""
url: "https://example.com"

# Build settings
markdown: kramdown
theme: minima
plugins:
  - jekyll-feed
  - jekyll-seo-tag

# Kramdown settings
kramdown:
  input: GFM
  syntax_highlighter: rouge
  hard_wrap: false

# Collections
collections:
  docs:
    output: true
    permalink: /docs/:name/

# Defaults
defaults:
  - scope:
      path: ""
      type: "posts"
    values:
      layout: "post"

# Exclude from processing
exclude:
  - Gemfile
  - Gemfile.lock
  - node_modules
  - vendor
```

## Front Matter

Todo arquivo de conteudo precisa de YAML front matter:

```markdown
---
layout: post
title: "My First Post"
date: 2025-01-28 12:00:00 -0500
categories: blog tutorial
tags: [jekyll, markdown]
author: John Doe
excerpt: "A brief introduction..."
published: true
---

Your content here...
```

## Markdown Processors

### Kramdown (Padrao)

```yaml
# _config.yml
markdown: kramdown
kramdown:
  input: GFM                    # GitHub Flavored Markdown
  syntax_highlighter: rouge
  syntax_highlighter_opts:
    block:
      line_numbers: true
```

### CommonMark

```ruby
# Gemfile
gem 'jekyll-commonmark-ghpages'
```

```yaml
# _config.yml
markdown: CommonMarkGhPages
commonmark:
  options: ["SMART", "FOOTNOTES"]