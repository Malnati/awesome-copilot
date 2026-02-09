## Modificar codigo

Faca as mudancas necessarias no app.

- Instale a client library
```
npm install @azure/monitor-opentelemetry
```

- Configure o app para usar Azure Monitor
Um app Node.js tipicamente tem um arquivo de entrada listado como propriedade "main" no package.json. Encontre esse arquivo e aplique estas mudancas nele.
  - Requerer a client library no topo. `const { useAzureMonitor } = require("@azure/monitor-opentelemetry");`
  - Chamar o metodo de setup. `useAzureMonitor();`

> Nota: O metodo de setup deve ser chamado o mais cedo possivel, mas precisa ser apos a configuracao de variaveis de ambiente, pois precisa da connection string do App Insights a partir da variavel de ambiente. Por exemplo, se o app usa dotenv para carregar variaveis de ambiente, o metodo de setup deve ser chamado depois disso, mas antes de qualquer outra coisa.
> Nota: como modificamos o codigo do app, ele precisa ser feito deploy para surtir efeito.

## Configurar connection string do App Insights

O recurso App Insights tem uma connection string. Adicione a connection string como variavel de ambiente do app em execucao. Voce pode usar Azure CLI para consultar a connection string do recurso App Insights. Veja [scripts/appinsights.ps1] para o comando Azure CLI a executar para consultar a connection string.

Depois de obter a connection string, defina esta variavel de ambiente com o valor.

```
"APPLICATIONINSIGHTS_CONNECTION_STRING={your_application_insights_connection_string}"
```

Se o app tiver template de IaC como arquivos Bicep ou terraform representando a instancia na nuvem, essa variavel de ambiente deve ser adicionada ao template IaC para ser aplicada em cada deployment. Caso contrario, use Azure CLI para aplicar manualmente a variavel de ambiente na instancia em nuvem do app. Veja o comando Azure CLI a executar para configurar essa variavel de ambiente.
