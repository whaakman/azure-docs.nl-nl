Maak een [web-app](../articles/app-service-web/app-service-web-overview.md) in het App Service-plan `myAppServicePlan` met de opdracht [az webapp create](/cli/azure/webapp#create). 

De web-app biedt hostruimte voor uw code en biedt een URL waarmee u de geïmplementeerde app kunt weergeven.

Vervang in de volgende opdracht *\<app_name>* door een unieke naam (geldige tekens zijn `a-z`, `0-9` en `-`). Als `<app_name>` niet uniek is, wordt er een foutbericht weergegeven met de mededeling dat er al een website bestaat met die naam. De standaard-URL van de web-app is `https://<app_name>.azurewebsites.net`. 

```azurecli-interactive
az webapp create --name <app_name> --resource-group myResourceGroup --plan myAppServicePlan
```

Wanneer de web-app is gemaakt, toont de Azure CLI soortgelijke informatie als in het volgende voorbeeld:

```json
{
  "availabilityState": "Normal",
  "clientAffinityEnabled": true,
  "clientCertEnabled": false,
  "cloningInfo": null,
  "containerSize": 0,
  "dailyMemoryTimeQuota": 0,
  "defaultHostName": "<app_name>.azurewebsites.net",
  "enabled": true,
  "enabledHostNames": [
    "<app_name>.azurewebsites.net",
    "<app_name>.scm.azurewebsites.net"
  ],
  "gatewaySiteName": null,
  "hostNameSslStates": [
    {
      "hostType": "Standard",
      "name": "<app_name>.azurewebsites.net",
      "sslState": "Disabled",
      "thumbprint": null,
      "toUpdate": null,
      "virtualIp": null
    }
    < JSON data removed for brevity. >
}
```

Blader naar de site om uw nieuwe web-app te bekijken.

```bash
http://<app_name>.azurewebsites.net
```
