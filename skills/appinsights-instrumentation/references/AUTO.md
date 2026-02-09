# Auto-instrumentar app

Use o Azure Portal para auto-instrumentar uma webapp hospedada no Azure App Service para App Insights sem fazer mudancas de codigo. Apenas os seguintes tipos de app podem ser auto-instrumentados. Veja [ambientes e resource providers suportados](https://learn.microsoft.com/azure/azure-monitor/app/codeless-overview#supported-environments-languages-and-resource-providers).

- App ASP.NET Core hospedado no Azure App Service
- App Node.js hospedado no Azure App Service

Construa uma URL para levar o usuario ao blade do Application Insights no Azure Portal para o App Service App.
```
https://portal.azure.com/#resource/subscriptions/{subscription_id}/resourceGroups/{resource_group_name}/providers/Microsoft.Web/sites/{app_service_name}/monitoringSettings
```

Use o contexto ou pergunte ao usuario para obter subscription_id, resource_group_name e app_service_name que hospedam a webapp.
