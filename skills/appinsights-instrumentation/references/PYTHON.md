## Modificar codigo

Faca as mudancas necessarias no app.

- Instale a client library
```
pip install azure-monitor-opentelemetry
```

- Configure o app para usar Azure Monitor
Aplicacoes Python enviam telemetria via a classe logger da biblioteca padrao do Python. Crie um modulo que configure e crie um logger que possa enviar telemetria.

```python
import logging
from azure.monitor.opentelemetry import configure_azure_monitor

configure_azure_monitor(
    logger_name="<your_logger_namespace>"
)
logger = logging.getLogger("<your_logger_namespace>")
```

> Nota: como modificamos o codigo do app, ele precisa ser feito deploy para surtir efeito.

## Configurar connection string do App Insights

O recurso App Insights tem uma connection string. Adicione a connection string como variavel de ambiente do app em execucao. Voce pode usar Azure CLI para consultar a connection string do recurso App Insights. Veja [scripts/appinsights.ps1] para o comando Azure CLI a executar para consultar a connection string.

Depois de obter a connection string, defina esta variavel de ambiente com o valor.

```
"APPLICATIONINSIGHTS_CONNECTION_STRING={your_application_insights_connection_string}"
```

Se o app tiver template de IaC como arquivos Bicep ou terraform representando a instancia na nuvem, essa variavel de ambiente deve ser adicionada ao template IaC para ser aplicada em cada deployment. Caso contrario, use Azure CLI para aplicar manualmente a variavel de ambiente na instancia em nuvem do app. Veja o comando Azure CLI a executar para configurar essa variavel de ambiente.

## Enviar dados

Crie um logger configurado para enviar telemetria.
```python
logger = logging.getLogger("<your_logger_namespace>")
logger.setLevel(logging.INFO)
```

Em seguida, envie eventos de telemetria chamando os metodos de logging.
```python
logger.info("info log")
```
