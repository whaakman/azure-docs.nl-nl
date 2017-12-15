Maak in de Cloud Shell met de opdracht [az webapp create](/cli/azure/webapp?view=azure-cli-latest#az_webapp_create) een [web-app](../articles/app-service/containers/app-service-linux-intro.md) in het App Service-plan `myAppServicePlan`. 

Vervang in het volgende voorbeeld wordt `<app_name>` met een globaal unieke app-naam (geldige tekens zijn `a-z`, `0-9`, en `-`). De runtime is ingesteld op `dotnetcore|1.1`. Als u wilt zien alle ondersteunde runtimes, [az webapp lijst-runtimes](/cli/azure/webapp?view=azure-cli-latest#az_webapp_list_runtimes). 

```azurecli-interactive
az webapp create --resource-group myResourceGroup --plan myAppServicePlan --name <app_name> --runtime "dotnetcore|1.1" --deployment-local-git
```

Wanneer de web-app is gemaakt, ziet de Azure CLI u uitvoer lijkt op het volgende voorbeeld:

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

U hebt een leeg web-app gemaakt in een container Linux met git-implementatie is ingeschakeld.

> [!NOTE]
> De URL van de externe Git wordt weergegeven in de `deploymentLocalGitUrl` eigenschap met de indeling `https://<username>@<app_name>.scm.azurewebsites.net/<app_name>.git`. Deze URL niet opslaan omdat u hebt deze later nodig.
>
