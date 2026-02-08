# Tasks by microsoft/edge-ai

Task Researcher e Task Planner para usuarios intermediarios a avancados e codebases grandes - Trazido a voce por microsoft/edge-ai

**Tags:** architecture, planning, research, tasks, implementation

## Items nesta Collection

| Title | Type | Description | MCP Servers |
| ----- | ---- | ----------- | ----------- |
| [Task Plan Implementation Instructions](../instructions/task-implementation.instructions.md)<br />[![Install in VS Code](https://img.shields.io/badge/VS_Code-Install-0098FF?style=flat-square&logo=visualstudiocode&logoColor=white)](https://aka.ms/awesome-copilot/install/instructions?url=vscode%3Achat-instructions%2Finstall%3Furl%3Dhttps%3A%2F%2Fraw.githubusercontent.com%2Fgithub%2Fawesome-copilot%2Fmain%2Finstructions%2Ftask-implementation.instructions.md)<br />[![Install in VS Code Insiders](https://img.shields.io/badge/VS_Code_Insiders-Install-24bfa5?style=flat-square&logo=visualstudiocode&logoColor=white)](https://aka.ms/awesome-copilot/install/instructions?url=vscode-insiders%3Achat-instructions%2Finstall%3Furl%3Dhttps%3A%2F%2Fraw.githubusercontent.com%2Fgithub%2Fawesome-copilot%2Fmain%2Finstructions%2Ftask-implementation.instructions.md) | Instruction | Instrucoes para implementar planos de tarefa com acompanhamento progressivo e registro de mudancas - Trazido a voce por microsoft/edge-ai [ver uso](#task-plan-implementation-instructions) |  |
| [Task Planner Instructions](../agents/task-planner.agent.md)<br />[![Install in VS Code](https://img.shields.io/badge/VS_Code-Install-0098FF?style=flat-square&logo=visualstudiocode&logoColor=white)](https://aka.ms/awesome-copilot/install/agent?url=vscode%3Achat-agent%2Finstall%3Furl%3Dhttps%3A%2F%2Fraw.githubusercontent.com%2Fgithub%2Fawesome-copilot%2Fmain%2Fagents%2Ftask-planner.agent.md)<br />[![Install in VS Code Insiders](https://img.shields.io/badge/VS_Code_Insiders-Install-24bfa5?style=flat-square&logo=visualstudiocode&logoColor=white)](https://aka.ms/awesome-copilot/install/agent?url=vscode-insiders%3Achat-agent%2Finstall%3Furl%3Dhttps%3A%2F%2Fraw.githubusercontent.com%2Fgithub%2Fawesome-copilot%2Fmain%2Fagents%2Ftask-planner.agent.md) | Agent | Task planner para criar planos de implementacao acionaveis - Trazido a voce por microsoft/edge-ai [ver uso](#task-planner-instructions) |  |
| [Task Researcher Instructions](../agents/task-researcher.agent.md)<br />[![Install in VS Code](https://img.shields.io/badge/VS_Code-Install-0098FF?style=flat-square&logo=visualstudiocode&logoColor=white)](https://aka.ms/awesome-copilot/install/agent?url=vscode%3Achat-agent%2Finstall%3Furl%3Dhttps%3A%2F%2Fraw.githubusercontent.com%2Fgithub%2Fawesome-copilot%2Fmain%2Fagents%2Ftask-researcher.agent.md)<br />[![Install in VS Code Insiders](https://img.shields.io/badge/VS_Code_Insiders-Install-24bfa5?style=flat-square&logo=visualstudiocode&logoColor=white)](https://aka.ms/awesome-copilot/install/agent?url=vscode-insiders%3Achat-agent%2Finstall%3Furl%3Dhttps%3A%2F%2Fraw.githubusercontent.com%2Fgithub%2Fawesome-copilot%2Fmain%2Fagents%2Ftask-researcher.agent.md) | Agent | Especialista em pesquisa de tasks para analise abrangente de projetos - Trazido a voce por microsoft/edge-ai [ver uso](#task-researcher-instructions) |  |

## Uso da Collection

### Task Plan Implementation Instructions

Continue usando `task-planner` para iterar no plano ate ter exatamente o que deseja fazer no codebase.

Quando estiver pronto para implementar o plano, **crie um novo chat** e alterne para o modo `Agent`, depois dispare o prompt recem-gerado.

```markdown, implement-fabric-rti-changes.prompt.md
---
mode: agent
title: Implement microsoft fabric realtime intelligence terraform support
---
/implement-fabric-rti-blueprint-modification phaseStop=true
```

Este prompt tem o beneficio adicional de anexar o plano como instructions, o que ajuda a manter o plano em contexto durante toda a conversa.

**Expert Warning** ->> Use `phaseStop=false` para que o Copilot implemente o plano inteiro sem parar. Alem disso, voce pode usar `taskStop=true` para fazer o Copilot parar apos cada implementacao de Task para controle mais detalhado.

Para usar essas instructions e prompts gerados, voce precisara atualizar seu `settings.json` adequadamente:

```json
    "chat.instructionsFilesLocations": {
        // Existing instructions folders...
        ".copilot-tracking/plans": true
    },
    "chat.promptFilesLocations": {
        // Existing prompts folders...
        ".copilot-tracking/prompts": true
    },
```

---

### Task Planner Instructions

Além disso, task-researcher fornecera ideias adicionais para implementacao que voce pode trabalhar com o GitHub Copilot para selecionar a melhor para focar.

```markdown, task-plan.prompt.md
---
mode: task-planner
title: Plan microsoft fabric realtime intelligence terraform support
---
#file: .copilot-tracking/research/*-fabric-rti-blueprint-modification-research.md
Build a plan to support adding fabric rti to this project
```

`task-planner` ajudara voce a criar um plano para implementar sua(s) task(s). Ele usara suas ideias totalmente pesquisadas ou fara novas pesquisas se ainda nao tiverem sido fornecidas.

`task-planner` produzira tres (3) arquivos que serao usados por `task-implementation.instructions.md`.

* `.copilot-tracking/plan/*-plan.instructions.md`

  * Um novo arquivo de instructions gerado que contem o plano como checklist de Phases e Tasks.
* `.copilot-tracking/details/*-details.md`

  * Os detalhes da implementacao; o arquivo de plano faz referencia a este arquivo para detalhes especificos (importante se voce tiver um plano grande).
* `.copilot-tracking/prompts/implement-*.prompt.md`

  * Um novo arquivo de prompt gerado que criara um arquivo `.copilot-tracking/changes/*-changes.md` e prosseguira para implementar as mudancas.

Continue usando `task-planner` para iterar no plano ate ter exatamente o que deseja fazer no codebase.

---

### Task Researcher Instructions

Agora voce pode iterar sobre pesquisa das suas tasks!

```markdown, research.prompt.md
---
mode: task-researcher
title: Research microsoft fabric realtime intelligence terraform support
---
Review the microsoft documentation for fabric realtime intelligence
and come up with ideas on how to implement this support into our terraform components.
```

A pesquisa e descarregada em um arquivo .copilot-tracking/research/*-research.md e incluira descobertas para GHCP junto com exemplos e schema que serao uteis durante a implementacao.

Além disso, task-researcher fornecera ideias adicionais para implementacao que voce pode trabalhar com o GitHub Copilot para selecionar a melhor para focar.

---
