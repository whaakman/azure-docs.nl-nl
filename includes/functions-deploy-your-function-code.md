## <a name="deploy-your-function-code"></a>Uw functiecode implementeren  

Er zijn verschillende manieren om uw functiecode te maken in uw nieuwe functie app. In dit onderwerp wordt verbinding gemaakt met een voorbeeldopslagplaats in GitHub. Net als voorheen moet u in de volgende code de plaatsaanduiding `<app_name>` vervangen door de naam van de functie-app die u hebt gemaakt. 

```azurecli-interactive
az functionapp deployment source config --name <app_name> --resource-group myResourceGroup --branch master \
--repo-url https://github.com/Azure-Samples/functions-quickstart \
--manual-integration 
```
Nadat de implementatiebron is ingesteld, toont de Azure CLI informatie vergelijkbaar met het volgende voorbeeld (null-waarden voor de leesbaarheid verwijderd):

```json
{
  "branch": "master",
  "deploymentRollbackEnabled": false,
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/...",
  "isManualIntegration": true,
  "isMercurial": false,
  "kind": null,
  "name": "quickstart",
  "repoUrl": "https://github.com/Azure-Samples/functions-quickstart",
  "resourceGroup": "myResourceGroup",
  "type": "Microsoft.Web/sites/sourcecontrols"
}
```
