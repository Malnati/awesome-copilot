## Modificar codigo

Faca as mudancas necessarias no app.

- Instale a client library
```
dotnet add package Azure.Monitor.OpenTelemetry.AspNetCore
```

- Configure o app para usar Azure Monitor
Um app ASP.NET Core tipicamente tem um arquivo Program.cs que "builda" o app. Encontre esse arquivo e aplique estas mudancas.
  - Adicione `using Azure.Monitor.OpenTelemetry.AspNetCore;` no topo
  - Antes de chamar `builder.Build()`, adicione esta linha `builder.Services.AddOpenTelemetry().UseAzureMonitor();`.

> Nota: como modificamos o codigo do app, o app precisa ser feito deploy para surtir efeito.

## Configurar connection string do App Insights

O recurso App Insights tem uma connection string. Adicione a connection string como variavel de ambiente do app em execucao. Voce pode usar o Azure CLI para consultar a connection string do recurso App Insights. Veja [scripts/appinsights.ps1](scripts/appinsights.ps1) para o comando Azure CLI a executar para consultar a connection string.

Depois de obter a connection string, defina esta variavel de ambiente com o valor.

```
"APPLICATIONINSIGHTS_CONNECTION_STRING={your_application_insights_connection_string}"
```

Se o app tiver template de IaC como arquivos Bicep ou terraform representando a instancia na nuvem, essa variavel de ambiente deve ser adicionada ao template IaC para ser aplicada em cada deployment. Caso contrario, use Azure CLI para aplicar manualmente a variavel de ambiente na instancia em nuvem do app. Veja [scripts/appinsights.ps1](scripts/appinsights.ps1) para o comando Azure CLI que configura essa variavel de ambiente.

> Importante: Nao modifique appsettings.json. Essa era uma forma deprecated de configurar App Insights. A variavel de ambiente e o novo caminho recomendado.
