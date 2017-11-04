Maak in de Cloud Shell met de opdracht [az webapp create](/cli/azure/webapp#create) een [web-app](../articles/app-service/app-service-web-overview.md) in het App Service-plan `myAppServicePlan`. 

De web-app biedt hostruimte voor uw code en biedt een URL waarmee u de geïmplementeerde app kunt weergeven.

Vervang in de volgende opdracht *\<app_name>* door een unieke naam (geldige tekens zijn `a-z`, `0-9` en `-`). Als `<app_name>` niet uniek is, wordt er een foutbericht weergegeven met de mededeling dat er al een website bestaat met die naam. De standaard-URL van de web-app is `https://<app_name>.azurewebsites.net`. 

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

U hebt een leeg web-app gemaakt met git-implementatie is ingeschakeld.

> [!NOTE]
> De URL van de externe Git wordt weergegeven in de `deploymentLocalGitUrl` eigenschap met de indeling `https://<username>@<app_name>.scm.azurewebsites.net/<app_name>.git`. Deze URL niet opslaan omdat u hebt deze later nodig.
>

Blader naar de site om uw nieuwe web-app te bekijken.

```bash
http://<app_name>.azurewebsites.net
```
