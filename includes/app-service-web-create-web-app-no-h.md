Maak in de Cloud Shell met de opdracht [az webapp create](/cli/azure/webapp?view=azure-cli-latest#az_webapp_create) een [web-app](../articles/app-service/app-service-web-overview.md) in het App Service-plan `myAppServicePlan`. 

Vervang in het volgende voorbeeld *\<app_name>* door een unieke naam (geldige tekens zijn `a-z`, `0-9` en `-`). 

```azurecli-interactive
az webapp create --name <app_name> --resource-group myResourceGroup --plan myAppServicePlan --deployment-local-git
```

Wanneer de web-app is gemaakt, toont de Azure CLI soortgelijke informatie als in het volgende voorbeeld:

```json
Local git is configured with url of 'https://<username>@<app_name>.scm.azurewebsites.net/<app_name>.git'
{
  "availabilityState": "Normal",
  "clientAffinityEnabled": true,
  "clientCertEnabled": false,
  "cloningInfo": null,
  "containerSize": 0,
  "dailyMemoryTimeQuota": 0,
  "defaultHostName": "<app_name>.azurewebsites.net",
  "deploymentLocalGitUrl": "https://<username>@<app_name>.scm.azurewebsites.net/<app_name>.git",
  "enabled": true,
  < JSON data removed for brevity. >
}
```

U hebt een lege web-app gemaakt, met Git-implementatie ingeschakeld.

> [!NOTE]
> De URL van de externe Git wordt weergegeven in de eigenschap `deploymentLocalGitUrl`, met de indeling `https://<username>@<app_name>.scm.azurewebsites.net/<app_name>.git`. Sla deze URL op, want u hebt deze later nodig.
>

Blader naar de nieuwe web-app.

```bash
http://<app_name>.azurewebsites.net
```
