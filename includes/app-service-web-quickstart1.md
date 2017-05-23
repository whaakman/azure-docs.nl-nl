## <a name="create-a-resource-group"></a>Een resourcegroep maken

 Een [Azure-resourcegroep](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#resource-groups) is een logische container waarin resources zoals web-apps en databases worden beheerd. U werkt de resources bij en implementeert en verwijdert ze in een resourcegroep.

Een resourcegroep maken met de opdracht [az group create](/cli/azure/group#create).

```azurecli
az group create --name myResourceGroup --location westeurope
```

Gebruik de opdracht `az appservice list-locations` voor een overzicht van de beschikbare locaties. In het algemeen maakt u resources in een regio bij u in de buurt.

## <a name="create-an-azure-app-service-plan"></a>Een Azure App Service-plan maken

Maak een gratis [App Service-plan](../articles/app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) met de opdracht [az appservice plan create](/cli/azure/appservice/plan#create).

[!INCLUDE [app-service-plan](app-service-plan.md)]

Met de volgende opdracht wordt een App Service-plan gemaakt met de naam `quickStartPlan` en de prijscategorie **Gratis**.

```azurecli
az appservice plan create --name quickStartPlan --resource-group myResourceGroup --sku FREE
```

Wanneer het App Service-plan is gemaakt, toont de Azure CLI soortgelijke informatie als in het volgende voorbeeld:

```json
{ 
  "adminSiteName": null,
  "appServicePlanName": "quickStartPlan",
  "geoRegion": "West Europe",
  "hostingEnvironmentProfile": null,
  "id": "/subscriptions/0000-0000/resourceGroups/myResourceGroup/providers/Microsoft.Web/serverfarms/quickStartPlan",
  "kind": "app",
  "location": "West Europe",
  "maximumNumberOfWorkers": 1,
  "name": "quickStartPlan",
  < JSON data removed for brevity. >
  "targetWorkerSizeId": 0,
  "type": "Microsoft.Web/serverfarms",
  "workerTierName": null
} 
```

## <a name="create-a-web-app"></a>Een web-app maken