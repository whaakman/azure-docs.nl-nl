
Maak met de opdracht [az webapp create](/cli/azure/webapp?view=azure-cli-latest#az_webapp_create) een app in het App Service-plan `myAppServicePlan`. 

De web-app biedt hostruimte voor uw API en biedt een URL waarmee u de geïmplementeerde app kunt weergeven.

Vervang in de volgende opdracht *\<app_name>* door een unieke naam. Als `<app_name>` niet uniek is, wordt er een foutbericht weergegeven met de mededeling dat er al een website bestaat met die naam. De standaard-URL van de web-app is `https://<app_name>.azurewebsites.net`. 

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