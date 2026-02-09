---
agent: "agent"
description: "Escreva um documento de padroes de codificacao para um projeto usando os estilos de codificacao do(s) arquivo(s) e/ou pasta(s) passados como argumentos no prompt."
tools: ['createFile', 'editFiles', 'web/fetch', 'githubRepo', 'search', 'testFailure']
---

# Escrever Padroes de Codificacao a partir de Arquivo

Use a sintaxe existente do(s) arquivo(s) para estabelecer os padroes e guias de estilo do projeto. Se mais de um arquivo ou uma pasta for passado, percorra cada arquivo ou arquivos na pasta, anexando os dados do arquivo a memoria temporaria ou a um arquivo, e quando concluir use os dados temporarios como uma unica instancia; como se fosse o nome do arquivo para basear os padroes e o guia de estilo.

## Regras e Configuracao

Abaixo ha um conjunto de variaveis quasi-configuracao `boolean` e `string[]`. As condicoes para lidar com `true`, ou outros valores para cada variavel, estao sob o cabecalho de nivel dois `## Variable and Parameter Configuration Conditions`.

Parametros para o prompt tem uma definicao textual. Ha um parametro obrigatorio **`${fileName}`** e varios parametros opcionais **`${folderName}`**, **`${instructions}`** e qualquer **`[configVariableAsParameter]`**.

### Variaveis de Configuracao

* addStandardsTest = false;
* addToREADME = false;
* addToREADMEInsertions = ["atBegin", "middle", "beforeEnd", "bestFitUsingContext"];
  - Padrao para **beforeEnd**.
* createNewFile = true;
* fetchStyleURL = true;
* findInconsistencies = true;
* fixInconsistencies = true;
* newFileName = ["CONTRIBUTING.md", "STYLE.md", "CODE_OF_CONDUCT.md", "CODING_STANDARDS.md", "DEVELOPING.md", "CONTRIBUTION_GUIDE.md", "GUIDELINES.md", "PROJECT_STANDARDS.md", "BEST_PRACTICES.md", "HACKING.md"];
  - Para cada arquivo em `${newFileName}`, se o arquivo nao existir, use esse nome e `break`, caso contrario continue para o proximo nome de arquivo de `${newFileName}`.
* outputSpecToPrompt = false;
* useTemplate = "verbose"; // ou "v"
  - Valores possiveis sao `[["v", "verbose"], ["m", "minimal"], ["b", "best fit"], ["custom"]]`.
  - Seleciona um dos dois templates de exemplo no fim do arquivo do prompt sob o cabecalho de nivel dois `## Coding Standards Templates`, ou use outra composicao que seja um melhor ajuste.
  - Se **custom**, entao aplique conforme solicitado.

### Variaveis de Configuracao como Parametros do Prompt

Se algum dos nomes das variaveis for passado ao prompt como esta, ou como um valor de texto semelhante mas claramente relacionado, entao substitua o valor padrao da variavel pelo valor passado ao prompt.

### Parametros do Prompt

* **fileName** = O nome do arquivo que sera analisado em termos de: indentacao, nomeacao de variaveis, comentarios, procedimentos condicionais, procedimentos funcionais e outros dados de sintaxe relacionados a linguagem do arquivo.
* folderName = O nome da pasta que sera usada para extrair dados de multiplos arquivos em um unico conjunto de dados agregado que sera analisado em termos de: indentacao, nomeacao de variaveis, comentarios, procedimentos condicionais, procedimentos funcionais e outros dados de sintaxe relacionados a linguagem dos arquivos.
* instructions = Instrucoes adicionais, regras e procedimentos que serao fornecidos para casos unicos.
* [configVariableAsParameter] = Se passado, substituira o estado padrao da variavel de configuracao. Exemplo:
  - useTemplate = Se passado, substituira o padrao de configuracao `${useTemplate}`. Valores sao `[["v", "verbose"], ["m", "minimal"], ["b", "best fit"]]`.

#### Parametros Obrigatorios e Opcionais

* **fileName** - obrigatorio
* folderName - *opcional*
* instructions - *opcional*
* [configVariableAsParameter] - *opcional*

## Variable and Parameter Configuration Conditions

### `${fileName}.length > 1 || ${folderName} != undefined`

* Se verdadeiro, alterne `${fixInconsistencies}` para false.

### `${addToREADME} == true`

* Insira os padroes de codificacao no `README.md` em vez de emitir no prompt ou criar um novo arquivo.
* Se verdadeiro, alterne tanto `${createNewFile}` quanto `${outputSpecToPrompt}` para false.

### `${addToREADMEInsertions} == "atBegin"`

* Se `${addToREADME}` for true, entao insira os dados de padroes de codificacao no **inicio** do arquivo `README.md` apos o titulo.

### `${addToREADMEInsertions} == "middle"`

* Se `${addToREADME}` for true, entao insira os dados de padroes de codificacao no **meio** do arquivo `README.md`, alterando o cabecalho de titulo dos padroes para corresponder ao da composicao do `README.md`.

### `${addToREADMEInsertions} == "beforeEnd"`

* Se `${addToREADME}` for true, entao insira os dados de padroes de codificacao no **fim** do arquivo `README.md`, inserindo uma nova linha apos o ultimo caractere, e depois inserindo os dados em uma nova linha.

### `${addToREADMEInsertions} == "bestFitUsingContext"`

* Se `${addToREADME}` for true, entao insira os dados de padroes de codificacao na **linha de melhor ajuste** do arquivo `README.md` em relacao ao contexto da composicao e ao fluxo de dados do `README.md`.

### `${addStandardsTest} == true`

* Uma vez que o arquivo de padroes de codificacao esteja completo, escreva um arquivo de teste para garantir que o arquivo ou arquivos passados a ele aderem aos padroes de codificacao.

### `${createNewFile} == true`

* Crie um novo arquivo usando o valor, ou um dos valores possiveis, de `${newFileName}`.
* Se verdadeiro, alterne tanto `${outputSpecToPrompt}` quanto `${addToREADME}` para false.

### `${fetchStyleURL} == true`

* Alem disso, use os dados buscados a partir dos links aninhados sob o cabecalho de nivel tres `### Fetch Links` como contexto para criar padroes, especificacoes e dados de estilo para o novo arquivo, prompt ou `README.md`.
* Para cada item relevante em `### Fetch Links`, execute `#fetch ${item}`.

### `${findInconsistencies} == true`

* Avalie sintaxe relacionada a indentacoes, quebras de linha, comentarios, aninhamento condicional e de funcoes, delimitadores de strings como `'` ou `"`, etc., e categorize.
* Para cada categoria, faca uma contagem, e se um item nao corresponder a maioria da contagem, entao registre na memoria temporaria.
* Dependendo do status de `${fixInconsistencies}`, edite e corrija as categorias de baixa contagem para corresponder a maioria, ou apresente no prompt inconsistencias armazenadas na memoria temporaria.

### `${fixInconsistencies} == true`

* Edite e corrija as categorias de baixa contagem de dados de sintaxe para corresponder a maioria dos dados de sintaxe correspondentes usando inconsistencias armazenadas na memoria temporaria.

### `typeof ${newFileName} == "string"`

* Se especificamente definido como uma `string`, crie um novo arquivo usando o valor de `${newFileName}`.

### `typeof ${newFileName} != "string"`

* Se **NAO** especificamente definido como `string`, mas sim um `object` ou um array, crie um novo arquivo usando um valor de `${newFileName}` aplicando esta regra:
  - Para cada nome de arquivo em `${newFileName}`, se o arquivo nao existir, use esse nome e `break`, caso contrario continue para o proximo.

### `${outputSpecToPrompt} == true`

* Apresente os padroes de codificacao no prompt em vez de criar um arquivo ou adicionar ao README.
* Se verdadeiro, alterne tanto `${createNewFile}` quanto `${addToREADME}` para false.

### `${useTemplate} == "v" || ${useTemplate} == "verbose"`

* Use os dados sob o cabecalho de nivel tres `### "v", "verbose"` como template guia ao compor os dados para padroes de codificacao.

### `${useTemplate} == "m" || ${useTemplate} == "minimal"`

* Use os dados sob o cabecalho de nivel tres `### "m", "minimal"` como template guia ao compor os dados para padroes de codificacao.

### `${useTemplate} == "b" || ${useTemplate} == "best"`

* Use os dados sob o cabecalho de nivel tres `### "v", "verbose"` ou `### "m", "minimal"`, dependendo dos dados extraidos de `${fileName}`, e use o melhor ajuste como template guia ao compor os dados para padroes de codificacao.

### `${useTemplate} == "custom" || ${useTemplate} == "<ANY_NAME>"`

* Use o prompt personalizado, instrucoes, template ou outros dados passados como template guia ao compor os dados para padroes de codificacao.

## **if** `${fetchStyleURL} == true`

Dependendo da linguagem de programacao, para cada link na lista abaixo, execute `#fetch (URL)`, se a linguagem de programacao for `${fileName} == [<Language> Style Guide]`.

### Fetch Links

- [C Style Guide](https://users.ece.cmu.edu/~eno/coding/CCodingStandard.html)
- [C# Style Guide](https://learn.microsoft.com/en-us/dotnet/csharp/fundamentals/coding-style/coding-conventions)
- [C++ Style Guide](https://isocpp.github.io/CppCoreGuidelines/CppCoreGuidelines)
- [Go Style Guide](https://github.com/golang-standards/project-layout)
- [Java Style Guide](https://coderanch.com/wiki/718799/Style)
- [AngularJS App Style Guide](https://github.com/mgechev/angularjs-style-guide)
- [jQuery Style Guide](https://contribute.jquery.org/style-guide/js/)
- [JavaScript Style Guide](https://www.w3schools.com/js/js_conventions.asp)
- [JSON Style Guide](https://google.github.io/styleguide/jsoncstyleguide.xml)
- [Kotlin Style Guide](https://kotlinlang.org/docs/coding-conventions.html)
- [Markdown Style Guide](https://cirosantilli.com/markdown-style-guide/)
- [Perl Style Guide](https://perldoc.perl.org/perlstyle)
- [PHP Style Guide](https://phptherightway.com/)
- [Python Style Guide](https://peps.python.org/pep-0008/)
- [Ruby Style Guide](https://rubystyle.guide/)
- [Rust Style Guide](https://github.com/rust-lang/rust/tree/HEAD/src/doc/style-guide/src)
- [Swift Style Guide](https://www.swift.org/documentation/api-design-guidelines/)
- [TypeScript Style Guide](https://www.typescriptlang.org/docs/handbook/declaration-files/do-s-and-don-ts.html)
- [Visual Basic Style Guide](https://en.wikibooks.org/wiki/Visual_Basic/Coding_Standards)
- [Shell Script Style Guide](https://google.github.io/styleguide/shellguide.html)
- [Git Usage Style Guide](https://github.com/agis/git-style-guide)
- [PowerShell Style Guide](https://github.com/PoshCode/PowerShellPracticeAndStyle)
- [CSS](https://cssguidelin.es/)
- [Sass Style Guide](https://sass-guidelin.es/)
- [HTML Style Guide](https://github.com/marcobiedermann/html-style-guide)
- [Linux kernel Style Guide](https://www.kernel.org/doc/html/latest/process/coding-style.html)
- [Node.js Style Guide](https://github.com/felixge/node-style-guide)
- [SQL Style Guide](https://www.sqlstyle.guide/)
- [Angular Style Guide](https://angular.dev/style-guide)
- [Vue Style Guide](https://vuejs.org/style-guide/rules-strongly-recommended.html)
- [Django Style Guide](https://docs.djangoproject.com/en/dev/internals/contributing/writing-code/coding-style/)
- [SystemVerilog Style Guide](https://github.com/lowRISC/style-guides/blob/master/VerilogCodingStyle.md)

## Coding Standards Templates

### `"m", "minimal"`

```text
    ```markdown
    ## 1. Introduction
    *   **Purpose:** Briefly explain why the coding standards are being established (e.g., to improve code quality, maintainability, and team collaboration).
    *   **Scope:** Define which languages, projects, or modules this specification applies to.

    ## 2. Naming Conventions
    *   **Variables:** `camelCase`
    *   **Functions/Methods:** `PascalCase` or `camelCase`.
    *   **Classes/Structs:** `PascalCase`.
    *   **Constants:** `UPPER_SNAKE_CASE`.

    ## 3. Formatting and Style
    *   **Indentation:** Use 4 spaces per indent (or tabs).
    *   **Line Length:** Limit lines to a maximum of 80 or 120 characters.
    *   **Braces:** Use the "K&R" style (opening brace on the same line) or the "Allman" style (opening brace on a new line).
    *   **Blank Lines:** Specify how many blank lines to use for separating logical blocks of code.

    ## 4. Commenting
    *   **Docstrings/Function Comments:** Describe the function's purpose, parameters, and return values.
    *   **Inline Comments:** Explain complex or non-obvious logic.
    *   **File Headers:** Specify what information should be included in a file header, such as author, date, and file description.

    ## 5. Error Handling
    *   **General:** How to handle and log errors.
    *   **Specifics:** Which exception types to use, and what information to include in error messages.

    ## 6. Best Practices and Anti-Patterns
    *   **General:** List common anti-patterns to avoid (e.g., global variables, magic numbers).
    *   **Language-specific:** Specific recommendations based on the project's programming language.

    ## 7. Examples
    *   Provide a small code example demonstrating the correct application of the rules.
    *   Provide a small code example of an incorrect implementation and how to fix it.

    ## 8. Contribution and Enforcement
    *   Explain how the standards are to be enforced (e.g., via code reviews).
    *   Provide a guide for contributing to the standards document itself.
    ```
```

### `"v", verbose"`

```text
    ```markdown

    # Style Guide

    This document defines the style and conventions used in this project.
    All contributions should follow these rules unless otherwise noted.

    ## 1. General Code Style

    - Favor clarity over brevity.
    - Keep functions and methods small and focused.
    - Avoid repeating logic; prefer shared helpers/utilities.
    - Remove unused variables, imports, code paths, and files.

    ## 2. Naming Conventions

    Use descriptive names. Avoid abbreviations unless well-known.

    | Item            | Convention           | Example            |
    |-----------------|----------------------|--------------------|
    | Variables       | `lower_snake_case`   | `buffer_size`      |
    | Functions       | `lower_snake_case()` | `read_file()`      |
    | Constants       | `UPPER_SNAKE_CASE`   | `MAX_RETRIES`      |
    | Types/Structs   | `PascalCase`         | `FileHeader`       |
    | File Names      | `lower_snake_case`   | `file_reader.c`    |

    ## 3. Formatting Rules

    - Indentation: **4 spaces**
    - Line length: **max 100 characters**
    - Encoding: **UTF-8**, no BOM
    - End files with a newline

    ### Braces (example in C, adjust for your language)

        ```c
        if (condition) {
            do_something();
        } else {
            do_something_else();
        }
        ```

    ### Spacing

    - One space after keywords: `if (x)`, not `if(x)`
    - One blank line between top-level functions

    ## 4. Comments & Documentation

    - Explain *why*, not *what*, unless intent is unclear.
    - Keep comments up-to-date as code changes.
    - Public functions should include a short description of purpose and parameters.

    Recommended tags:

        ```text
        TODO: follow-up work
        FIXME: known incorrect behavior
        NOTE: non-obvious design decision
        ```

    ## 5. Error Handling

    - Handle error conditions explicitly.
    - Avoid silent failures; either return errors or log them appropriately.
    - Clean up resources (files, memory, handles) before returning on failure.

    ## 6. Commit & Review Practices

    ### Commits
    - One logical change per commit.
    - Write clear commit messages:

        ```text
        Short summary (max ~50 chars)
        Optional longer explanation of context and rationale.
        ```

    ### Reviews
    - Keep pull requests reasonably small.
    - Be respectful and constructive in review discussions.
    - Address requested changes or explain if you disagree.

    ## 7. Tests

    - Write tests for new functionality.
    - Tests should be deterministic (no randomness without seeding).
    - Prefer readable test cases over complex test abstraction.

    ## 8. Changes to This Guide

    Style evolves.
    Propose improvements by opening an issue or sending a patch updating this document.
    ```
```
