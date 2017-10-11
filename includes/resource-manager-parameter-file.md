Als u een parameterbestand parameterwaarden doorgeven tijdens de implementatie gebruikt, moet u een JSON-bestand maken met een indeling als in het volgende voorbeeld:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "webSiteName": {
            "value": "ExampleSite"
        },
        "webSiteHostingPlanName": {
            "value": "DefaultPlan"
        },
        "webSiteLocation": {
            "value": "West US"
        },
        "adminPassword": {
            "reference": {
               "keyVault": {
                  "id": "/subscriptions/{guid}/resourceGroups/{group-name}/providers/Microsoft.KeyVault/vaults/{vault-name}"
               }, 
               "secretName": "sqlAdminPassword" 
            }   
        }
   }
}
```

De grootte van het parameterbestand mag niet meer dan 64 KB.

Als u wilt een gevoelige waarde opgeven voor een parameter (zoals een wachtwoord), moet u die waarde toevoegen aan een sleutelkluis. De sleutelkluis ophalen tijdens de implementatie, zoals wordt weergegeven in het vorige voorbeeld. Zie voor meer informatie [beveiligde waarden doorgeven tijdens de implementatie van](../articles/azure-resource-manager/resource-manager-keyvault-parameter.md). 

