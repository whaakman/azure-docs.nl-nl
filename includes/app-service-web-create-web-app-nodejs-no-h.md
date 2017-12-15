In de Cloud-Shell, maakt u een web-app in de `myAppServicePlan` App Service-abonnement met de [az webapp maken](/cli/azure/webapp?view=azure-cli-latest#az_webapp_create) opdracht. 

Vervang in het volgende voorbeeld wordt `<app_name>` met een globaal unieke app-naam (geldige tekens zijn `a-z`, `0-9`, en `-`). De runtime is ingesteld op `NODE|6.9`. Als u wilt zien alle ondersteunde runtimes, [az webapp lijst-runtimes](/cli/azure/webapp?view=azure-cli-latest#az_webapp_list_runtimes). 

```azurecli-interactive
az webapp create --resource-group myResourceGroup --plan myAppServicePlan --name <app_name> --runtime "NODE|6.9" --deployment-local-git
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

U hebt een leeg web-app gemaakt met git-implementatie is ingeschakeld.

> [!NOTE]
> De URL van de externe Git wordt weergegeven in de `deploymentLocalGitUrl` eigenschap met de indeling `https://<username>@<app_name>.scm.azurewebsites.net/<app_name>.git`. Deze URL niet opslaan omdat u hebt deze later nodig.
>
