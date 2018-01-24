---
title: Azure Resource Manager-sjabloonfuncties - resources | Microsoft Docs
description: Beschrijft de functies te gebruiken in een Azure Resource Manager-sjabloon voor het ophalen van waarden over bronnen.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/22/2018
ms.author: tomfitz
ms.openlocfilehash: f92afd27540e935ed901151d980377b9b34ea8f5
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2018
---
# <a name="resource-functions-for-azure-resource-manager-templates"></a>Resource-functies voor Azure Resource Manager-sjablonen

Resource Manager biedt de volgende functies voor het ophalen van waarden van de resource:

* [listKeys en de lijst {Value}](#listkeys)
* [providers](#providers)
* [verwijzing](#reference)
* [resourceGroup](#resourcegroup)
* [resourceId](#resourceid)
* [abonnement](#subscription)

Als u de waarden van de parameters en variabelen en de huidige implementatie, Zie [implementatie waarde functies](resource-group-template-functions-deployment.md).

<a id="listkeys" />
<a id="list" />

## <a name="listkeys-and-listvalue"></a>listKeys en de lijst {Value}
`listKeys(resourceName or resourceIdentifier, apiVersion)`

`list{Value}(resourceName or resourceIdentifier, apiVersion)`

Retourneert de waarden voor elk resourcetype die ondersteuning biedt voor de bewerking van de lijst. Het meest voorkomende gebruik `listKeys`. 

### <a name="parameters"></a>Parameters

| Parameter | Vereist | Type | Beschrijving |
|:--- |:--- |:--- |:--- |
| resourceName of resourceIdentifier |Ja |tekenreeks |De unieke id voor de resource. |
| apiVersion |Ja |tekenreeks |API-versie van de status van de runtime. Normaal gesproken in de notatie **jjjj-mm-dd**. |

### <a name="return-value"></a>Retourwaarde

Het geretourneerde object uit listKeys heeft de volgende indeling:

```json
{
  "keys": [
    {
      "keyName": "key1",
      "permissions": "Full",
      "value": "{value}"
    },
    {
      "keyName": "key2",
      "permissions": "Full",
      "value": "{value}"
    }
  ]
}
```

Andere functies van de lijst met hebben verschillende retour-indelingen. Overzicht van de indeling van een functie, opnemen in de sectie uitvoer zoals weergegeven in de voorbeeldsjabloon. 

### <a name="remarks"></a>Opmerkingen

Een bewerking die met begint **lijst** kan worden gebruikt als een functie in uw sjabloon. De beschikbare bewerkingen zijn niet alleen listKeys, maar ook bewerkingen, zoals `list`, `listAdminKeys`, en `listStatus`. U kunt geen echter gebruiken **lijst** bewerkingen uit waarbij de waarden in de aanvraagtekst. Bijvoorbeeld, de [lijst Account-SAS](/rest/api/storagerp/storageaccounts#StorageAccounts_ListAccountSAS) bewerking moet de aanvraag hoofdtekst parameters, zoals *signedExpiry*, zodat u deze niet binnen een sjabloon gebruiken.

Om te bepalen welke resourcetypen er een lijstbewerking, hebt u de volgende opties:

* Weergave de [REST-API-bewerkingen](/rest/api/) voor een resourceprovider en zoekt u bewerkingen na opvragen. Bijvoorbeeld: storage-accounts hebben de [listKeys bewerking](/rest/api/storagerp/storageaccounts#StorageAccounts_ListKeys).
* Gebruik de [Get-AzureRmProviderOperation](/powershell/module/azurerm.resources/get-azurermprovideroperation) PowerShell-cmdlet. Het volgende voorbeeld wordt alle bewerkingen na opvragen voor storage-accounts:

  ```powershell
  Get-AzureRmProviderOperation -OperationSearchString "Microsoft.Storage/*" | where {$_.Operation -like "*list*"} | FT Operation
  ```
* Gebruik de volgende opdracht in de Azure CLI voor het filteren van alleen de bewerkingen na opvragen:

  ```azurecli
  az provider operation show --namespace Microsoft.Storage --query "resourceTypes[?name=='storageAccounts'].operations[].name | [?contains(@, 'list')]"
  ```

Geef de bron met behulp van de [resourceId functie](#resourceid), of de indeling `{providerNamespace}/{resourceType}/{resourceName}`.

### <a name="example"></a>Voorbeeld

De volgende [voorbeeldsjabloon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/listkeys.json) laat zien hoe het retourneren van de primaire en secundaire sleutels op een opslagaccount in de sectie uitvoer.

```json
{
  "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
      "storageAccountName": { 
          "type": "string"
      }
  },
  "resources": [
    {
      "name": "[parameters('storageAccountName')]",
      "type": "Microsoft.Storage/storageAccounts",
      "apiVersion": "2016-12-01",
      "sku": {
        "name": "Standard_LRS"
      },
      "kind": "Storage",
      "location": "[resourceGroup().location]",
      "tags": {},
      "properties": {
      }
    }
  ],
  "outputs": {
      "referenceOutput": {
          "type": "object",
          "value": "[listKeys(parameters('storageAccountName'), '2016-12-01')]"
      }
    }
}
``` 

Voor het implementeren van deze voorbeeldsjabloon met Azure CLI gebruiken:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/listkeys.json --parameters storageAccountName=<your-storage-account>
```

Voor het implementeren van deze voorbeeldsjabloon met PowerShell gebruiken:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/listkeys.json -storageAccountName <your-storage-account>
```

<a id="providers" />

## <a name="providers"></a>providers
`providers(providerNamespace, [resourceType])`

Retourneert informatie over een resourceprovider en de volgende resourcetypen ondersteund. Als u een resourcetype niet opgeeft, worden de ondersteunde typen voor de resourceprovider geretourneerd.

### <a name="parameters"></a>Parameters

| Parameter | Vereist | Type | Beschrijving |
|:--- |:--- |:--- |:--- |
| providerNamespace |Ja |tekenreeks |Namespace van de provider |
| resourceType |Nee |tekenreeks |Het type bron binnen de opgegeven naamruimte. |

### <a name="return-value"></a>Retourwaarde

Elk type ondersteunde wordt geretourneerd in de volgende indeling: 

```json
{
    "resourceType": "{name of resource type}",
    "locations": [ all supported locations ],
    "apiVersions": [ all supported API versions ]
}
```

Matrix ordening van de geretourneerde waarden kan niet worden gegarandeerd.

### <a name="example"></a>Voorbeeld

De volgende [voorbeeldsjabloon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/providers.json) laat zien hoe de provider-functie wilt gebruiken:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "providerNamespace": {
            "type": "string"
        },
        "resourceType": {
            "type": "string"
        }
    },
    "resources": [],
    "outputs": {
        "providerOutput": {
            "value": "[providers(parameters('providerNamespace'), parameters('resourceType'))]",
            "type" : "object"
        }
    }
}
```

Voor de **Microsoft.Web** resourceprovider en **sites** brontype in het voorgaande voorbeeld retourneert een object in de volgende indeling:

```json
{
  "resourceType": "sites",
  "locations": [
    "South Central US",
    "North Europe",
    "West Europe",
    "Southeast Asia",
    ...
  ],
  "apiVersions": [
    "2016-08-01",
    "2016-03-01",
    "2015-08-01-preview",
    "2015-08-01",
    ...
  ]
}
```

Voor het implementeren van deze voorbeeldsjabloon met Azure CLI gebruiken:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/providers.json --parameters providerNamespace=Microsoft.Web resourceType=sites
```

Voor het implementeren van deze voorbeeldsjabloon met PowerShell gebruiken:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/providers.json -providerNamespace Microsoft.Web -resourceType sites
```

<a id="reference" />

## <a name="reference"></a>Verwijzing
`reference(resourceName or resourceIdentifier, [apiVersion], ['Full'])`

Retourneert een object dat de runtimestatus van de bron.

### <a name="parameters"></a>Parameters

| Parameter | Vereist | Type | Beschrijving |
|:--- |:--- |:--- |:--- |
| resourceName of resourceIdentifier |Ja |tekenreeks |Naam of de unieke id van een resource. |
| apiVersion |Nee |tekenreeks |API-versie van de opgegeven resource. Deze parameter bevatten wanneer de bron niet binnen dezelfde sjabloon ingericht is. Normaal gesproken in de notatie **jjjj-mm-dd**. |
| 'Volledige' |Nee |tekenreeks |De waarde die aangeeft of de volledige resource-object als resultaat gegeven. Als u geen opgeeft `'Full'`, alleen de Eigenschappenobject van de bron wordt geretourneerd. Het volledige object bevat de waarden zoals de resource-ID en de locatie. |

### <a name="return-value"></a>Retourwaarde

Elk resourcetype verschillende eigenschappen voor de verwijzing naar functie geretourneerd. De functie retourneert een enkele, vooraf gedefinieerde indeling. De geretourneerde waarde verschilt ook op basis van of u het volledige object opgegeven. Overzicht van de eigenschappen voor een resourcetype, het object in de sectie uitvoer zoals weergegeven in het voorbeeld te retourneren.

### <a name="remarks"></a>Opmerkingen

De functie verwijzing is afgeleid van de waarde van een runtimestatus en daarom niet worden gebruikt in het gedeelte variabelen. Het kan worden gebruikt in het gedeelte van de uitvoer van een sjabloon of [gekoppelde sjabloon](resource-group-linked-templates.md#link-or-nest-a-template). Het kan niet worden gebruikt in de sectie uitvoer van een [geneste sjabloon](resource-group-linked-templates.md#link-or-nest-a-template). Als u wilt de waarden voor een geïmplementeerde resource in een geneste sjabloon, de sjabloon voor geneste niet converteren naar een gekoppelde sjabloon. 

Met behulp van de functie verwijzing impliciet kunt declareren u één resource afhankelijk is van een andere bron, als de bron waarnaar wordt verwezen is ingericht binnen dezelfde sjabloon. U hoeft niet de eigenschap dependsOn ook gebruiken. De functie wordt niet geëvalueerd als de bron waarnaar wordt verwezen implementatie is voltooid.

Overzicht van de eigenschapnamen en waarden voor een resourcetype, een sjabloon waarmee het object wordt geretourneerd in de sectie uitvoer te maken. Als u een bestaande resource van dat type hebt, retourneert de sjabloon voor het object zonder eventuele nieuwe resources implementeren. 

Normaal gesproken gebruikt u de **verwijzing** functie voor het retourneren van een bepaalde waarde van een object, zoals het eindpunt van blob-URI of de volledig gekwalificeerde domeinnaam.

```json
"outputs": {
    "BlobUri": {
        "value": "[reference(concat('Microsoft.Storage/storageAccounts/', parameters('storageAccountName')), '2016-01-01').primaryEndpoints.blob]",
        "type" : "string"
    },
    "FQDN": {
        "value": "[reference(concat('Microsoft.Network/publicIPAddresses/', parameters('ipAddressName')), '2016-03-30').dnsSettings.fqdn]",
        "type" : "string"
    }
}
```

Gebruik `'Full'` wanneer u waarden voor resources die geen deel uitmaken van het schema eigenschappen nodig hebt. Bijvoorbeeld, het toegangsbeleid voor sleutelkluizen stelt de identiteitseigenschappen ophalen voor een virtuele machine

```json
{
  "type": "Microsoft.KeyVault/vaults",
  "properties": {
    "tenantId": "[reference(concat('Microsoft.Compute/virtualMachines/', variables('vmName')), '2017-03-30', 'Full').identity.tenantId]",
    "accessPolicies": [
      {
        "tenantId": "[reference(concat('Microsoft.Compute/virtualMachines/', variables('vmName')), '2017-03-30', 'Full').identity.tenantId]",
        "objectId": "[reference(concat('Microsoft.Compute/virtualMachines/', variables('vmName')), '2017-03-30', 'Full').identity.principalId]",
        "permissions": {
          "keys": [
            "all"
          ],
          "secrets": [
            "all"
          ]
        }
      }
    ],
    ...
```

Zie voor het volledige voorbeeld van de voorgaande sjabloon, [Windows voor Sleutelkluis](https://github.com/rjmax/AzureSaturday/blob/master/Demo02.ManagedServiceIdentity/demo08.msiWindowsToKeyvault.json). Een vergelijkbaar voorbeeld is beschikbaar voor [Linux](https://github.com/rjmax/AzureSaturday/blob/master/Demo02.ManagedServiceIdentity/demo07.msiLinuxToArm.json).

### <a name="example"></a>Voorbeeld

De volgende [voorbeeldsjabloon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/referencewithstorage.json) implementeert een resource en verwijst naar deze bron.

```json
{
  "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
      "storageAccountName": { 
          "type": "string"
      }
  },
  "resources": [
    {
      "name": "[parameters('storageAccountName')]",
      "type": "Microsoft.Storage/storageAccounts",
      "apiVersion": "2016-12-01",
      "sku": {
        "name": "Standard_LRS"
      },
      "kind": "Storage",
      "location": "[resourceGroup().location]",
      "tags": {},
      "properties": {
      }
    }
  ],
  "outputs": {
      "referenceOutput": {
          "type": "object",
          "value": "[reference(parameters('storageAccountName'))]"
      },
      "fullReferenceOutput": {
        "type": "object",
        "value": "[reference(parameters('storageAccountName'), '2016-12-01', 'Full')]"
      }
    }
}
``` 

Het vorige voorbeeld retourneert de twee objecten. Het Eigenschappenobject is in de volgende indeling:

```json
{
   "creationTime": "2017-10-09T18:55:40.5863736Z",
   "primaryEndpoints": {
     "blob": "https://examplestorage.blob.core.windows.net/",
     "file": "https://examplestorage.file.core.windows.net/",
     "queue": "https://examplestorage.queue.core.windows.net/",
     "table": "https://examplestorage.table.core.windows.net/"
   },
   "primaryLocation": "southcentralus",
   "provisioningState": "Succeeded",
   "statusOfPrimary": "available",
   "supportsHttpsTrafficOnly": false
}
```

De volledige-object is in de volgende indeling:

```json
{
  "apiVersion":"2016-12-01",
  "location":"southcentralus",
  "sku": {
    "name":"Standard_LRS",
    "tier":"Standard"
  },
  "tags":{},
  "kind":"Storage",
  "properties": {
    "creationTime":"2017-10-09T18:55:40.5863736Z",
    "primaryEndpoints": {
      "blob":"https://examplestorage.blob.core.windows.net/",
      "file":"https://examplestorage.file.core.windows.net/",
      "queue":"https://examplestorage.queue.core.windows.net/",
      "table":"https://examplestorage.table.core.windows.net/"
    },
    "primaryLocation":"southcentralus",
    "provisioningState":"Succeeded",
    "statusOfPrimary":"available",
    "supportsHttpsTrafficOnly":false
  },
  "subscriptionId":"<subscription-id>",
  "resourceGroupName":"functionexamplegroup",
  "resourceId":"Microsoft.Storage/storageAccounts/examplestorage",
  "referenceApiVersion":"2016-12-01",
  "condition":true,
  "isConditionTrue":true,
  "isTemplateResource":false,
  "isAction":false,
  "provisioningOperation":"Read"
}
```

Voor het implementeren van deze voorbeeldsjabloon met Azure CLI gebruiken:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/referencewithstorage.json --parameters storageAccountName=<your-storage-account>
```

Voor het implementeren van deze voorbeeldsjabloon met PowerShell gebruiken:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/referencewithstorage.json -storageAccountName <your-storage-account>
```

De volgende [voorbeeldsjabloon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/reference.json) verwijst naar een opslagaccount dat niet is geïmplementeerd in deze sjabloon. Het opslagaccount bestaat al in dezelfde resourcegroep.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "storageAccountName": {
            "type": "string"
        }
    },
    "resources": [],
    "outputs": {
        "ExistingStorage": {
            "value": "[reference(concat('Microsoft.Storage/storageAccounts/', parameters('storageAccountName')), '2016-01-01')]",
            "type" : "object"
        }
    }
}
```

Voor het implementeren van deze voorbeeldsjabloon met Azure CLI gebruiken:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/reference.json --parameters storageAccountName=<your-storage-account>
```

Voor het implementeren van deze voorbeeldsjabloon met PowerShell gebruiken:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/reference.json -storageAccountName <your-storage-account>
```

<a id="resourcegroup" />

## <a name="resourcegroup"></a>resourceGroup
`resourceGroup()`

Retourneert een object dat de huidige resourcegroep vertegenwoordigt. 

### <a name="return-value"></a>Retourwaarde

Het geretourneerde object is in de volgende indeling:

```json
{
  "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}",
  "name": "{resourceGroupName}",
  "location": "{resourceGroupLocation}",
  "tags": {
  },
  "properties": {
    "provisioningState": "{status}"
  }
}
```

### <a name="remarks"></a>Opmerkingen

De functie resourceGroup worden vaak gebruikt is het maken van resources op dezelfde locatie als de resourcegroep. Het volgende voorbeeld wordt de locatie van de resourcegroep om toe te wijzen de locatie voor een website.

```json
"resources": [
   {
      "apiVersion": "2016-08-01",
      "type": "Microsoft.Web/sites",
      "name": "[parameters('siteName')]",
      "location": "[resourceGroup().location]",
      ...
   }
]
```

### <a name="example"></a>Voorbeeld

De volgende [voorbeeldsjabloon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/resourcegroup.json) worden de eigenschappen van de resourcegroep geretourneerd.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [],
    "outputs": {
        "resourceGroupOutput": {
            "value": "[resourceGroup()]",
            "type" : "object"
        }
    }
}
```

Het vorige voorbeeld retourneert een object in de volgende indeling:

```json
{
  "id": "/subscriptions/{subscription-id}/resourceGroups/examplegroup",
  "name": "examplegroup",
  "location": "southcentralus",
  "properties": {
    "provisioningState": "Succeeded"
  }
}
```

Voor het implementeren van deze voorbeeldsjabloon met Azure CLI gebruiken:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/resourcegroup.json
```

Voor het implementeren van deze voorbeeldsjabloon met PowerShell gebruiken:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/resourcegroup.json 
```

<a id="resourceid" />

## <a name="resourceid"></a>resourceId
`resourceId([subscriptionId], [resourceGroupName], resourceType, resourceName1, [resourceName2]...)`

Retourneert de unieke id van een resource. U kunt deze functie gebruiken wanneer de resourcenaam niet eenduidig of niet ingericht binnen dezelfde sjabloon is. 

### <a name="parameters"></a>Parameters

| Parameter | Vereist | Type | Beschrijving |
|:--- |:--- |:--- |:--- |
| subscriptionId |Nee |tekenreeks (In GUID-indeling) |Standaard wordt het huidige abonnement. Deze waarde opgeven als u nodig hebt voor het ophalen van een resource in een ander abonnement. |
| resourceGroupName |Nee |tekenreeks |Standaardwaarde is de huidige resourcegroep. Deze waarde opgeven als u nodig hebt voor het ophalen van een resource in een andere resourcegroep. |
| resourceType |Ja |tekenreeks |Type resource, met inbegrip van de naamruimte van de resource-provider. |
| resourceName1 |Ja |tekenreeks |Naam van de resource. |
| resourceName2 |Nee |tekenreeks |Volgende naam resourcesegment als bron is genest. |

### <a name="return-value"></a>Retourwaarde

De id wordt geretourneerd in de volgende indeling:

```json
/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/{resourceProviderNamespace}/{resourceType}/{resourceName}
```

### <a name="remarks"></a>Opmerkingen

De parameterwaarden die u opgeeft, is afhankelijk van of de resource in de hetzelfde abonnement en de resource-groep als de huidige implementatie is.

Als u de resource-ID voor een opslagaccount in hetzelfde abonnement en resourcegroep, gebruikt u:

```json
"[resourceId('Microsoft.Storage/storageAccounts','examplestorage')]"
```

Als u de resource-ID voor een opslagaccount in hetzelfde abonnement, maar een andere resourcegroep, gebruikt u:

```json
"[resourceId('otherResourceGroup', 'Microsoft.Storage/storageAccounts','examplestorage')]"
```

Als u de resource-ID voor een opslagaccount in een ander abonnement en resourcegroep, gebruikt u:

```json
"[resourceId('xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx', 'otherResourceGroup', 'Microsoft.Storage/storageAccounts','examplestorage')]"
```

Als u de resource-ID voor een database in een andere resourcegroep, gebruikt u:

```json
"[resourceId('otherResourceGroup', 'Microsoft.SQL/servers/databases', parameters('serverName'), parameters('databaseName'))]"
```

Vaak het geval is, moet u deze functie wilt gebruiken wanneer u een opslagaccount of een virtueel netwerk in een andere resourcegroep. Het volgende voorbeeld ziet u hoe een resource van een externe resourcegroep gemakkelijk kan worden gebruikt:

```json
{
  "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
      "virtualNetworkName": {
          "type": "string"
      },
      "virtualNetworkResourceGroup": {
          "type": "string"
      },
      "subnet1Name": {
          "type": "string"
      },
      "nicName": {
          "type": "string"
      }
  },
  "variables": {
      "vnetID": "[resourceId(parameters('virtualNetworkResourceGroup'), 'Microsoft.Network/virtualNetworks', parameters('virtualNetworkName'))]",
      "subnet1Ref": "[concat(variables('vnetID'),'/subnets/', parameters('subnet1Name'))]"
  },
  "resources": [
  {
      "apiVersion": "2015-05-01-preview",
      "type": "Microsoft.Network/networkInterfaces",
      "name": "[parameters('nicName')]",
      "location": "[parameters('location')]",
      "properties": {
          "ipConfigurations": [{
              "name": "ipconfig1",
              "properties": {
                  "privateIPAllocationMethod": "Dynamic",
                  "subnet": {
                      "id": "[variables('subnet1Ref')]"
                  }
              }
          }]
       }
  }]
}
```

### <a name="example"></a>Voorbeeld

De volgende [voorbeeldsjabloon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/resourceid.json) retourneert de resource-ID voor een opslagaccount in de resourcegroep:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [],
    "outputs": {
        "sameRGOutput": {
            "value": "[resourceId('Microsoft.Storage/storageAccounts','examplestorage')]",
            "type" : "string"
        },
        "differentRGOutput": {
            "value": "[resourceId('otherResourceGroup', 'Microsoft.Storage/storageAccounts','examplestorage')]",
            "type" : "string"
        },
        "differentSubOutput": {
            "value": "[resourceId('11111111-1111-1111-1111-111111111111', 'otherResourceGroup', 'Microsoft.Storage/storageAccounts','examplestorage')]",
            "type" : "string"
        },
        "nestedResourceOutput": {
            "value": "[resourceId('Microsoft.SQL/servers/databases', 'serverName', 'databaseName')]",
            "type" : "string"
        }
    }
}
```

De uitvoer van het vorige voorbeeld met de standaardwaarde is:

| Naam | Type | Waarde |
| ---- | ---- | ----- |
| sameRGOutput | Tekenreeks | /subscriptions/{current-sub-id}/resourceGroups/examplegroup/providers/Microsoft.Storage/storageAccounts/examplestorage |
| differentRGOutput | Tekenreeks | /Subscriptions/{Current-sub-id}/resourceGroups/otherResourceGroup/providers/Microsoft.Storage/storageAccounts/examplestorage |
| differentSubOutput | Tekenreeks | /Subscriptions/11111111-1111-1111-1111-111111111111/resourceGroups/otherResourceGroup/providers/Microsoft.Storage/storageAccounts/examplestorage |
| nestedResourceOutput | Tekenreeks | /subscriptions/{current-sub-id}/resourceGroups/examplegroup/providers/Microsoft.SQL/servers/serverName/databases/databaseName |

Voor het implementeren van deze voorbeeldsjabloon met Azure CLI gebruiken:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/resourceid.json
```

Voor het implementeren van deze voorbeeldsjabloon met PowerShell gebruiken:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/resourceid.json 
```

<a id="subscription" />

## <a name="subscription"></a>abonnement
`subscription()`

Retourneert informatie over het abonnement voor de huidige implementatie. 

### <a name="return-value"></a>Retourwaarde

De functie retourneert de volgende indeling:

```json
{
    "id": "/subscriptions/{subscription-id}",
    "subscriptionId": "{subscription-id}",
    "tenantId": "{tenant-id}",
    "displayName": "{name-of-subscription}"
}
```

### <a name="example"></a>Voorbeeld

De volgende [voorbeeldsjabloon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/subscription.json) toont de abonnement-functie is aangeroepen in de sectie uitvoer. 

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [],
    "outputs": {
        "subscriptionOutput": {
            "value": "[subscription()]",
            "type" : "object"
        }
    }
}
```

Voor het implementeren van deze voorbeeldsjabloon met Azure CLI gebruiken:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/subscription.json
```

Voor het implementeren van deze voorbeeldsjabloon met PowerShell gebruiken:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/subscription.json 
```

## <a name="next-steps"></a>Volgende stappen
* Zie voor een beschrijving van de secties in een Azure Resource Manager-sjabloon [Azure Resource Manager-sjablonen samenstellen](resource-group-authoring-templates.md).
* U kunt meerdere sjablonen samenvoegen, Zie [gekoppelde sjablonen gebruiken met Azure Resource Manager](resource-group-linked-templates.md).
* Voor een opgegeven aantal keer herhalen bij het maken van een type resource, Zie [maken van meerdere exemplaren van resources in Azure Resource Manager](resource-group-create-multiple.md).
* Zie voor het implementeren van de sjabloon die u hebt gemaakt, [Implementeer een toepassing met Azure Resource Manager-sjabloon](resource-group-template-deploy.md).

