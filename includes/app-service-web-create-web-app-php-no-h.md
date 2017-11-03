In de Cloud-Shell, maakt u een web-app in de `myAppServicePlan` App Service-abonnement met de [az webapp maken](/cli/azure/webapp#create) opdracht. Vergeet niet om te vervangen `<app_name>` met een unieke app-naam.

De runtime in de volgende opdracht is ingesteld op `PHP|7.0`. Als u wilt zien alle ondersteunde runtimes, [az webapp lijst-runtimes](/cli/azure/webapp#list-runtimes). 

```azurecli-interactive
az webapp create --resource-group myResourceGroup --plan myAppServicePlan --name <app_name> --runtime "PHP|7.0" --deployment-local-git
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

U hebt een leeg nieuwe web-app gemaakt met git-implementatie is ingeschakeld.

> [!NOTE]
> De URL van de externe Git wordt weergegeven in de `deploymentLocalGitUrl` eigenschap met de indeling `https://<username>@<app_name>.scm.azurewebsites.net/<app_name>.git`. Deze URL niet opslaan omdat u hebt deze later nodig.
>
