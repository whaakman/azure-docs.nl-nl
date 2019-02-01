---
title: Azure Resource Manager-sjabloonfuncties - resources | Microsoft Docs
description: Beschrijft de functies in een Azure Resource Manager-sjabloon gebruikt voor het ophalen van waarden over resources.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: ''
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/30/2019
ms.author: tomfitz
ms.openlocfilehash: 5b4eaa6080f3ff238ffeb26e442c5d694c38c0c1
ms.sourcegitcommit: 5978d82c619762ac05b19668379a37a40ba5755b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55496313"
---
# <a name="resource-functions-for-azure-resource-manager-templates"></a>Functies van de resource voor Azure Resource Manager-sjablonen

Resource Manager biedt de volgende functies voor het ophalen van waarden van resources:

* [lijst met *](#list)
* [Providers](#providers)
* [Verwijzing](#reference)
* [resourceGroup](#resourcegroup)
* [ResourceId](#resourceid)
* [abonnement](#subscription)

Waarden van parameters, variabelen of de huidige implementatie Zie [waarde implementatiefuncties](resource-group-template-functions-deployment.md).

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

<a id="listkeys" />
<a id="list" />

## <a name="list"></a>lijst met *

`list{Value}(resourceName or resourceIdentifier, apiVersion, functionValues)`

De syntaxis voor deze functie is afhankelijk van de naam van de lijst met bewerkingen. Elke implementatie retourneert de waarden voor het brontype die ondersteuning biedt voor een lijstbewerking. Naam van de bewerking moet beginnen met `list`. Een veelvoorkomend gebruik zijn `listKeys` en `listSecrets`. 

### <a name="parameters"></a>Parameters

| Parameter | Vereist | Type | Description |
|:--- |:--- |:--- |:--- |
| resourceName of resourceIdentifier |Ja |string |De unieke id voor de resource. |
| apiVersion |Ja |string |API-versie van de runtimestatus van de resource. Normaal gesproken in de indeling, **jjjj-mm-dd**. |
| functionValues |Nee |object | Een object met waarden voor de functie. Geef alleen dit object voor functies die ondersteuning bieden voor ontvangst van een object met parameterwaarden, zoals **listAccountSas** op een storage-account. In dit artikel ziet u een voorbeeld van het doorgeven van waarden voor de functie. | 

### <a name="implementations"></a>Implementaties

Het gebruik van lijst * worden weergegeven in de volgende tabel.

| Resourcetype | Functienaam |
| ------------- | ------------- |
| Microsoft.AnalysisServices/servers | [listGatewayStatus](/rest/api/analysisservices/servers/listgatewaystatus) |
| Microsoft.Automation/automationAccounts | [listKeys](/rest/api/automation/keys/listbyautomationaccount) |
| Microsoft.Batch/batchAccounts | [listkeys](/rest/api/batchmanagement/batchaccount/getkeys) |
| Microsoft.BatchAI/workspaces/experiments/jobs | [listoutputfiles](/rest/api/batchai/jobs/listoutputfiles) |
| Microsoft.Cache/redis | [listKeys](/rest/api/redis/redis/listkeys) |
| Microsoft.CognitiveServices/accounts | [listKeys](/rest/api/cognitiveservices/accountmanagement/accounts/listkeys) |
| Microsoft.ContainerRegistry/registries | [listBuildSourceUploadUrl](/rest/api/containerregistry/registries%20(tasks)/getbuildsourceuploadurl) |
| Microsoft.ContainerRegistry/registries | [listCredentials](/rest/api/containerregistry/registries/listcredentials) |
| Microsoft.ContainerRegistry/registries | [listPolicies](/rest/api/containerregistry/registries/listpolicies) |
| Microsoft.ContainerRegistry/registries | [listUsages](/rest/api/containerregistry/registries/listusages) |
| Microsoft.ContainerRegistry/registries/webhooks | [listEvents](/rest/api/containerregistry/webhooks/listevents) |
| Microsoft.ContainerService/managedClusters | [listClusterAdminCredential](/rest/api/aks/managedclusters/listclusteradmincredentials) |
| Microsoft.ContainerService/managedClusters | [listClusterUserCredential](/rest/api/aks/managedclusters/listclusterusercredentials) |
| Microsoft.DataFactory/datafactories/gateways | listauthkeys |
| Microsoft.DataFactory/factories/integrationruntimes | [listauthkeys](/rest/api/datafactory/integrationruntimes/listauthkeys) |
| Microsoft.DataLakeAnalytics/accounts/storageAccounts/Containers | [listSasTokens](/rest/api/datalakeanalytics/storageaccounts/listsastokens) |
| Microsoft.Devices/iotHubs | [listkeys](/rest/api/iothub/iothubresource/listkeys) |
| Microsoft.Devices/provisioningServices/keys | [listkeys](/rest/api/iot-dps/iotdpsresource/listkeysforkeyname) |
| Microsoft.Devices/provisioningServices | [listkeys](/rest/api/iot-dps/iotdpsresource/listkeys) |
| Microsoft.DevTestLab/labs | [ListVhds](/rest/api/dtl/labs/listvhds) |
| Microsoft.DevTestLab/labs/schedules | [ListApplicable](/rest/api/dtl/schedules/listapplicable) |
| Microsoft.DevTestLab/labs/users/serviceFabrics | [ListApplicableSchedules](/rest/api/dtl/servicefabrics/listapplicableschedules) |
| Microsoft.DevTestLab/labs/virtualMachines | [ListApplicableSchedules](/rest/api/dtl/virtualmachines/listapplicableschedules) |
| Microsoft.DocumentDB/databaseAccounts | [listConnectionStrings](/rest/api/cosmos-db-resource-provider/databaseaccounts/listconnectionstrings) |
| Microsoft.DocumentDB/databaseAccounts | [listKeys](/rest/api/cosmos-db-resource-provider/databaseaccounts/listkeys) |
| Microsoft.DomainRegistration | [listDomainRecommendations](/rest/api/appservice/domains/listrecommendations) |
| Microsoft.DomainRegistration/topLevelDomains | [listAgreements](/rest/api/appservice/topleveldomains/listagreements) |
| Microsoft.EventGrid/topics | [listKeys](/rest/api/eventgrid/topics/listsharedaccesskeys) |
| Microsoft.EventHub/namespaces/authorizationRules | [listkeys](/rest/api/eventhub/namespaces/listkeys) |
| Microsoft.EventHub/namespaces/disasterRecoveryConfigs/authorizationRules | [listkeys](/rest/api/eventhub/disasterrecoveryconfigs/listkeys) |
| Microsoft.EventHub/namespaces/eventhubs/authorizationRules | [listkeys](/rest/api/eventhub/eventhubs/listkeys) |
| Microsoft.ImportExport/jobs | [listBitLockerKeys](/rest/api/storageimportexport/bitlockerkeys/list) |
| Microsoft.Logic/integrationAccounts/agreements | [listContentCallbackUrl](/rest/api/logic/agreements/listcontentcallbackurl) |
| Microsoft.Logic/integrationAccounts/assemblies | [listContentCallbackUrl](/rest/api/logic/integrationaccountassemblies/listcontentcallbackurl) |
| Microsoft.Logic/integrationAccounts | [listCallbackUrl](/rest/api/logic/integrationaccounts/getcallbackurl) |
| Microsoft.Logic/integrationAccounts | [listKeyVaultKeys](/rest/api/logic/integrationaccounts/listkeyvaultkeys) |
| Microsoft.Logic/integrationAccounts/maps | [listContentCallbackUrl](/rest/api/logic/maps/listcontentcallbackurl) |
| Microsoft.Logic/integrationAccounts/partners | [listContentCallbackUrl](/rest/api/logic/partners/listcontentcallbackurl) |
| Microsoft.Logic/integrationAccounts/schemas | [listContentCallbackUrl](/rest/api/logic/schemas/listcontentcallbackurl) |
| Microsoft.Logic/workflows | [listCallbackUrl](/rest/api/logic/workflows/listcallbackurl) |
| Microsoft.Logic/workflows | [listSwagger](/rest/api/logic/workflows/listswagger) |
| Microsoft.MachineLearning/webServices | [listkeys](/rest/api/machinelearning/webservices/listkeys) |
| Microsoft.MachineLearning/Workspaces | listworkspacekeys |
| Microsoft.MachineLearningServices/workspaces/computes | listKeys |
| Microsoft.MachineLearningServices/workspaces | listKeys |
| Microsoft.Maps/accounts | [listKeys](/rest/api/maps-management/accounts/listkeys) |
| Microsoft.Media/mediaservices/assets | [listContainerSas](/rest/api/media/assets/listcontainersas) |
| Microsoft.Media/mediaservices/assets | [listStreamingLocators](/rest/api/media/assets/liststreaminglocators) |
| Microsoft.Media/mediaservices/streamingLocators | [listContentKeys](/rest/api/media/streaminglocators/listcontentkeys) |
| Microsoft.Media/mediaservices/streamingLocators | [listPaths](/rest/api/media/streaminglocators/listpaths) |
| Microsoft.NotificationHubs/Namespaces/authorizationRules | [listkeys](/rest/api/notificationhubs/namespaces/listkeys) |
| Microsoft.NotificationHubs/Namespaces/NotificationHubs/authorizationRules | [listkeys](/rest/api/notificationhubs/notificationhubs/listkeys) |
| Microsoft.OperationalInsights/workspaces | [listKeys](/rest/api/loganalytics/workspaces%202015-03-20/listkeys) |
| Microsoft.Relay/namespaces/authorizationRules | [listkeys](/rest/api/relay/namespaces/listkeys) |
| Microsoft.Relay/namespaces/HybridConnections/authorizationRules | [listkeys](/rest/api/relay/hybridconnections/listkeys) |
| Microsoft.Relay/namespaces/WcfRelays/authorizationRules | [listkeys](/rest/api/relay/wcfrelays/listkeys) |
| Microsoft.Search/searchServices | [listAdminKeys](/rest/api/searchmanagement/adminkeys/get) |
| Microsoft.Search/searchServices | [listQueryKeys](/rest/api/searchmanagement/querykeys/listbysearchservice) |
| Microsoft.ServiceBus/namespaces/authorizationRules | [listkeys](/rest/api/servicebus/namespaces/listkeys) |
| Microsoft.ServiceBus/namespaces/disasterRecoveryConfigs/authorizationRules | [listkeys](/rest/api/servicebus/disasterrecoveryconfigs/listkeys) |
| Microsoft.ServiceBus/namespaces/queues/authorizationRules | [listkeys](/rest/api/servicebus/queues/listkeys) |
| Microsoft.ServiceBus/namespaces/topics/authorizationRules | [listkeys](/rest/api/servicebus/topics/listkeys) |
| Microsoft.SignalRService/SignalR | [listkeys](/rest/api/signalr/signalr/listkeys) |
| Microsoft.Storage/storageAccounts | [listAccountSas](/rest/api/storagerp/storageaccounts/listaccountsas) |
| Microsoft.Storage/storageAccounts | [listkeys](/rest/api/storagerp/storageaccounts/listkeys) |
| Microsoft.Storage/storageAccounts | [listServiceSas](/rest/api/storagerp/storageaccounts/listservicesas) |
| Microsoft.StorSimple/managers/devices | [listFailoverSets](/rest/api/storsimple/devices/listfailoversets) |
| Microsoft.StorSimple/managers/devices | [listFailoverTargets](/rest/api/storsimple/devices/listfailovertargets) |
| Microsoft.StorSimple/managers | [listActivationKey](/rest/api/storsimple/managers/getactivationkey) |
| Microsoft.StorSimple/managers | [listPublicEncryptionKey](/rest/api/storsimple/managers/getpublicencryptionkey) |
| Microsoft.Web/connectionGateways | ListStatus |
| microsoft.web/connections | listconsentlinks |
| Microsoft.Web/customApis | listWsdlInterfaces |
| microsoft.web/locations | listwsdlinterfaces |
| microsoft.web/sites/functions | [listsecrets](/rest/api/appservice/webapps/listfunctionsecrets) |
| microsoft.web/sites/hybridconnectionnamespaces/relays | [listkeys](/rest/api/appservice/webapps/listhybridconnectionkeys) |
| microsoft.web/sites | [listsyncfunctiontriggerstatus](/rest/api/appservice/webapps/listsyncfunctiontriggers) |
| microsoft.web/sites/slots/functions | [listsecrets](/rest/api/appservice/webapps/listfunctionsecretsslot) |

Om te bepalen welke resourcetypen de bewerking voor een lijst met hebt, hebt u de volgende opties:

* Weergave de [REST API-bewerkingen](/rest/api/) voor een resourceprovider, en zoekt u bewerkingen na opvragen. Bijvoorbeeld storage-accounts hebben de [listKeys bewerking](/rest/api/storagerp/storageaccounts#StorageAccounts_ListKeys).
* Gebruik de [Get-AzProviderOperation](/powershell/module/az.resources/get-azprovideroperation) PowerShell-cmdlet. Het volgende voorbeeld haalt alle bewerkingen na opvragen voor storage-accounts:

  ```powershell
  Get-AzProviderOperation -OperationSearchString "Microsoft.Storage/*" | where {$_.Operation -like "*list*"} | FT Operation
  ```
* Gebruik de volgende Azure CLI-opdracht voor het filteren van alleen de bewerkingen na opvragen:

  ```azurecli
  az provider operation show --namespace Microsoft.Storage --query "resourceTypes[?name=='storageAccounts'].operations[].name | [?contains(@, 'list')]"
  ```

### <a name="return-value"></a>Retourwaarde

Het geretourneerde object is afhankelijk van de functie van de lijst die u gebruikt. De listKeys voor een opslagaccount retourneert bijvoorbeeld de volgende indeling:

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

Andere functies van de lijst met hebben verschillende retour bestandsindelingen. Als u wilt zien van de indeling van een functie, opnemen in de uitvoersectie zoals wordt weergegeven in de voorbeeldsjabloon. 

### <a name="remarks"></a>Opmerkingen

De resource met behulp van de resourcenaam opgeven of de [resourceId functie](#resourceid). Wanneer u een lijst met door de functie in dezelfde sjabloon waarmee de resource waarnaar wordt verwezen, wordt geïmplementeerd, gebruikt u de naam van de resource.

### <a name="example"></a>Voorbeeld

De volgende [voorbeeldsjabloon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/listkeys.json) laat zien hoe de primaire en secundaire sleutels uit een opslagaccount in de uitvoersectie geretourneerd. Deze retourneert ook een SAS-token voor het opslagaccount. 

Als u het token SAS-token, geeft u een object voor het verlooptijdstip. Het verlooptijdstip moet in de toekomst. In dit voorbeeld is bedoeld om ziet u hoe u de lijst met functies. Normaal gesproken u zou het SAS-token in de waarde van een resource gebruiken in plaats een uitvoerwaarde terug. Uitvoerwaarden worden opgeslagen in de geschiedenis van de implementatie en niet is beveiligd.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "storagename": {
            "type": "string"
        },
        "location": {
            "type": "string",
            "defaultValue": "southcentralus"
        },
        "accountSasProperties": {
            "type": "object",
            "defaultValue": {
                "signedServices": "b",
                "signedPermission": "r",
                "signedExpiry": "2018-08-20T11:00:00Z",
                "signedResourceTypes": "s"
            }
        }
    },
    "resources": [
        {
            "apiVersion": "2018-02-01",
            "name": "[parameters('storagename')]",
            "location": "[parameters('location')]",
            "type": "Microsoft.Storage/storageAccounts",
            "sku": {
                "name": "Standard_LRS"
            },
            "kind": "StorageV2",
            "properties": {
                "supportsHttpsTrafficOnly": false,
                "accessTier": "Hot",
                "encryption": {
                    "services": {
                        "blob": {
                            "enabled": true
                        },
                        "file": {
                            "enabled": true
                        }
                    },
                    "keySource": "Microsoft.Storage"
                }
            },
            "dependsOn": []
        }
    ],
    "outputs": {
        "keys": {
            "type": "object",
            "value": "[listKeys(parameters('storagename'), '2018-02-01')]"
        },
        "accountSAS": {
            "type": "object",
            "value": "[listAccountSas(parameters('storagename'), '2018-02-01', parameters('accountSasProperties'))]"
        }
    }
}
``` 

In dit als voorbeeldsjabloon wilt implementeren met Azure CLI, gebruikt u:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/listkeys.json --parameters storagename=<your-storage-account>
```

In dit als voorbeeldsjabloon wilt implementeren met PowerShell, gebruikt u:

```powershell
New-AzResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/listkeys.json -storagename <your-storage-account>
```

<a id="providers" />

## <a name="providers"></a>Providers
`providers(providerNamespace, [resourceType])`

Retourneert informatie over een resourceprovider en de ondersteunde resourcetypen. Als u een resourcetype niet opgeeft, retourneert de functie de ondersteunde typen voor de resourceprovider.

### <a name="parameters"></a>Parameters

| Parameter | Vereist | Type | Description |
|:--- |:--- |:--- |:--- |
| providerNamespace |Ja |string |Namespace van de provider |
| ResourceType |Nee |string |Het type resource binnen de opgegeven naamruimte. |

### <a name="return-value"></a>Retourwaarde

Elk type ondersteunde wordt geretourneerd in de volgende indeling: 

```json
{
    "resourceType": "{name of resource type}",
    "locations": [ all supported locations ],
    "apiVersions": [ all supported API versions ]
}
```

Matrix volgorde van de geretourneerde waarden wordt niet gegarandeerd.

### <a name="example"></a>Voorbeeld

De volgende [voorbeeldsjabloon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/providers.json) laat zien hoe u de provider-functie te gebruiken:

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

In dit als voorbeeldsjabloon wilt implementeren met Azure CLI, gebruikt u:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/providers.json --parameters providerNamespace=Microsoft.Web resourceType=sites
```

In dit als voorbeeldsjabloon wilt implementeren met PowerShell, gebruikt u:

```powershell
New-AzResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/providers.json -providerNamespace Microsoft.Web -resourceType sites
```

<a id="reference" />

## <a name="reference"></a>Verwijzing
`reference(resourceName or resourceIdentifier, [apiVersion], ['Full'])`

Retourneert een object waarmee de runtimestatus van een resource.

### <a name="parameters"></a>Parameters

| Parameter | Vereist | Type | Description |
|:--- |:--- |:--- |:--- |
| resourceName of resourceIdentifier |Ja |string |Naam of de unieke id van een resource. |
| apiVersion |Nee |string |API-versie van de opgegeven resource. Deze parameter opgeeft als de bron is niet in dezelfde sjabloon ingericht. Normaal gesproken in de indeling, **jjjj-mm-dd**. |
| 'Volledige' |Nee |string |De waarde die aangeeft of het volledige resource-object te retourneren. Als u geen opgeeft `'Full'`, alleen het object voor eigenschappen van de resource wordt geretourneerd. De volledige-object bevat waarden, zoals de resource-ID en de locatie. |

### <a name="return-value"></a>Retourwaarde

Elk resourcetype geeft als resultaat van de verschillende eigenschappen voor de referentie-functie. De functie retourneren niet een enkele, vooraf gedefinieerde indeling. De geretourneerde waarde verschilt ook, op basis van of u het volledige object hebt opgegeven. Overzicht van de eigenschappen voor een resourcetype, het object in de uitvoersectie zoals wordt weergegeven in het voorbeeld te retourneren.

### <a name="remarks"></a>Opmerkingen

De functie verwijzing haalt de runtimestatus van een eerder geïmplementeerde resource of een resource in de huidige sjabloon wordt geïmplementeerd. In dit artikel ziet u voorbeelden voor beide scenario's. Wanneer u verwijst naar een resource in de huidige sjabloon, geef de naam van de resource als parameter. Wanneer u verwijst naar een eerder geïmplementeerde resource, bieden u de resource-ID en een API-versie voor de resource. U kunt geldige API-versies bepalen voor uw resource in de [sjabloonverwijzing](/azure/templates/).

De referentie-functie kan alleen worden gebruikt in de eigenschappen van de resourcedefinitie van een en de uitvoersectie van een sjabloon of de implementatie.

Met behulp van de referentie-functie, declareert u impliciet dat een resource afhankelijk van een andere resource is als de bron waarnaar wordt verwezen, is ingericht in dezelfde sjabloon en u naar de resource met de naam (geen resource-ID verwijst). U hoeft niet te gebruiken ook de eigenschap DEPENDSON te maken. De functie wordt niet geëvalueerd totdat de resource waarnaar wordt verwezen, de implementatie is voltooid.

Als u wilt zien de namen van eigenschappen en waarden voor een resourcetype, een sjabloon die wordt geretourneerd van het object in de uitvoersectie te maken. Als u een bestaande resource van dat type hebt, retourneert de sjabloon voor het object zonder nieuwe resources te implementeren. 

Meestal gebruikt u de **verwijzing** functie voor het retourneren van een bepaalde waarde van een object, zoals het eindpunt van blob-URI of de volledig gekwalificeerde domeinnaam.

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

Gebruik `'Full'` wanneer u waarden in de resource die geen deel uitmaken van het schema eigenschappen nodig hebt. Bijvoorbeeld, om toegangsbeleid voor sleutelkluizen, Hiermee worden de identiteitseigenschappen voor een virtuele machine.

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

Zie voor het complete voorbeeld van de voorgaande sjabloon [Windows naar Key Vault](https://github.com/rjmax/AzureSaturday/blob/master/Demo02.ManagedServiceIdentity/demo08.msiWindowsToKeyvault.json). Een voorbeeld van een soortgelijke is beschikbaar voor [Linux](https://github.com/rjmax/AzureSaturday/blob/master/Demo02.ManagedServiceIdentity/demo07.msiLinuxToArm.json).

### <a name="example"></a>Voorbeeld

De volgende [voorbeeldsjabloon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/referencewithstorage.json) implementeert een resource, en verwijst naar die resource.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
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

Het vorige voorbeeld retourneert de twee objecten. Het object voor eigenschappen is in de volgende indeling:

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

Het volledige object verkeert in de volgende indeling:

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

In dit als voorbeeldsjabloon wilt implementeren met Azure CLI, gebruikt u:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/referencewithstorage.json --parameters storageAccountName=<your-storage-account>
```

In dit als voorbeeldsjabloon wilt implementeren met PowerShell, gebruikt u:

```powershell
New-AzResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/referencewithstorage.json -storageAccountName <your-storage-account>
```

De volgende [voorbeeldsjabloon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/reference.json) verwijst naar een opslagaccount die in deze sjabloon is niet geïmplementeerd. Het opslagaccount bestaat al binnen hetzelfde abonnement.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "storageResourceGroup": {
            "type": "string"
        },
        "storageAccountName": {
            "type": "string"
        }
    },
    "resources": [],
    "outputs": {
        "ExistingStorage": {
            "value": "[reference(resourceId(parameters('storageResourceGroup'), 'Microsoft.Storage/storageAccounts', parameters('storageAccountName')), '2018-07-01')]",
            "type": "object"
        }
    }
}
```

In dit als voorbeeldsjabloon wilt implementeren met Azure CLI, gebruikt u:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/reference.json --parameters storageResourceGroup=<rg-for-storage> storageAccountName=<your-storage-account>
```

In dit als voorbeeldsjabloon wilt implementeren met PowerShell, gebruikt u:

```powershell
New-AzResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/reference.json -storageResourceGroup <rg-for-storage> -storageAccountName <your-storage-account>
```

<a id="resourcegroup" />

## <a name="resourcegroup"></a>ResourceGroup
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

De `resourceGroup()` functie kan niet worden gebruikt in een sjabloon die is [geïmplementeerd op het abonnementsniveau](deploy-to-subscription.md). Het kan alleen worden gebruikt in de sjablonen die zijn geïmplementeerd voor een resourcegroep.

Een algemene gebruik van de resourceGroup-functie is het maken van resources in dezelfde locatie als de resourcegroep. Het volgende voorbeeld wordt de locatie voor resourcegroep om toe te wijzen de locatie voor een website.

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

In dit als voorbeeldsjabloon wilt implementeren met Azure CLI, gebruikt u:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/resourcegroup.json
```

In dit als voorbeeldsjabloon wilt implementeren met PowerShell, gebruikt u:

```powershell
New-AzResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/resourcegroup.json 
```

<a id="resourceid" />

## <a name="resourceid"></a>resourceId
`resourceId([subscriptionId], [resourceGroupName], resourceType, resourceName1, [resourceName2]...)`

Retourneert de unieke id van een resource. U kunt deze functie gebruiken als de resourcenaam van de niet eenduidig of niet ingericht binnen dezelfde sjabloon is. 

### <a name="parameters"></a>Parameters

| Parameter | Vereist | Type | Description |
|:--- |:--- |:--- |:--- |
| subscriptionId |Nee |tekenreeks (In GUID-indeling) |Standaard wordt het huidige abonnement. Deze waarde opgeven wanneer u nodig hebt om op te halen van een resource in een ander abonnement. |
| resourceGroupName |Nee |string |Standaardwaarde is de huidige resourcegroep. Deze waarde opgeven wanneer u nodig hebt om op te halen van een resource in een andere resourcegroep. |
| ResourceType |Ja |string |Het type resource, met inbegrip van de naamruimte van de resource-provider. |
| resourceName1 |Ja |string |De naam van de resource. |
| resourceName2 |Nee |string |Volgende resource naam segment als bron is genest. |

### <a name="return-value"></a>Retourwaarde

De id wordt geretourneerd in de volgende indeling:

```json
/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/{resourceProviderNamespace}/{resourceType}/{resourceName}
```

### <a name="remarks"></a>Opmerkingen

Gebruikt in combinatie met een [abonnementsniveau implementatie](deploy-to-subscription.md), wordt de `resourceId()` functie kan alleen de ID van de resources die zijn geïmplementeerd op dat niveau ophalen. U kunt bijvoorbeeld de ID van een beleidsdefinitie of roldefinitie, maar niet de ID van een storage-account krijgen. Het omgekeerde geldt voor implementaties in een resourcegroep. U kan de resource-ID van de resources die zijn geïmplementeerd op het abonnementsniveau niet ophalen.

De parameterwaarden die u opgeeft, is afhankelijk van of de resource in het hetzelfde abonnement en resourcegroep als de huidige implementatie is. Voor de resource-ID voor een opslagaccount in hetzelfde abonnement en resourcegroep, gebruikt u:

```json
"[resourceId('Microsoft.Storage/storageAccounts','examplestorage')]"
```

Voor de resource-ID voor een opslagaccount in hetzelfde abonnement maar een andere resourcegroep, gebruikt u:

```json
"[resourceId('otherResourceGroup', 'Microsoft.Storage/storageAccounts','examplestorage')]"
```

Voor de resource-ID voor een opslagaccount in een ander abonnement en resourcegroep, gebruikt u:

```json
"[resourceId('xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx', 'otherResourceGroup', 'Microsoft.Storage/storageAccounts','examplestorage')]"
```

Voor de resource-ID voor een database in een andere resourcegroep, gebruikt u:

```json
"[resourceId('otherResourceGroup', 'Microsoft.SQL/servers/databases', parameters('serverName'), parameters('databaseName'))]"
```

Voor de resource-ID van een resource op abonnementsniveau bij het implementeren van op het abonnementsbereik, gebruikt u:

```json
"[resourceId('Microsoft.Authorization/policyDefinitions', 'locationpolicy')]"
```

Vaak het geval is, moet u deze functie wilt gebruiken bij het gebruik van een storage-account of een virtueel netwerk in een andere resourcegroep. Het volgende voorbeeld laat zien hoe een resource vanaf een externe resourcegroep eenvoudig kan worden gebruikt:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
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

De volgende [voorbeeldsjabloon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/resourceid.json) geeft als resultaat de resource-ID voor een opslagaccount in de resourcegroep:

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

De uitvoer uit het vorige voorbeeld met de standaardwaarden is:

| Name | Type | Waarde |
| ---- | ---- | ----- |
| sameRGOutput | Reeks | /Subscriptions/{Current-sub-id}/resourceGroups/examplegroup/providers/Microsoft.Storage/storageAccounts/examplestorage |
| differentRGOutput | Reeks | /Subscriptions/{Current-sub-id}/resourceGroups/otherResourceGroup/providers/Microsoft.Storage/storageAccounts/examplestorage |
| differentSubOutput | Reeks | /Subscriptions/11111111-1111-1111-1111-111111111111/resourceGroups/otherResourceGroup/providers/Microsoft.Storage/storageAccounts/examplestorage |
| nestedResourceOutput | Reeks | /Subscriptions/{Current-sub-id}/resourceGroups/examplegroup/providers/Microsoft.SQL/servers/servername/databases/databaseName |

In dit als voorbeeldsjabloon wilt implementeren met Azure CLI, gebruikt u:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/resourceid.json
```

In dit als voorbeeldsjabloon wilt implementeren met PowerShell, gebruikt u:

```powershell
New-AzResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/resourceid.json 
```

<a id="subscription" />

## <a name="subscription"></a>abonnement
`subscription()`

Retourneert informatie over het abonnement voor de huidige implementatie. 

### <a name="return-value"></a>Retourwaarde

De functie geeft als resultaat van de volgende indeling:

```json
{
    "id": "/subscriptions/{subscription-id}",
    "subscriptionId": "{subscription-id}",
    "tenantId": "{tenant-id}",
    "displayName": "{name-of-subscription}"
}
```

### <a name="example"></a>Voorbeeld

De volgende [voorbeeldsjabloon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/subscription.json) toont de abonnement-functie in de uitvoersectie aangeroepen. 

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

In dit als voorbeeldsjabloon wilt implementeren met Azure CLI, gebruikt u:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/subscription.json
```

In dit als voorbeeldsjabloon wilt implementeren met PowerShell, gebruikt u:

```powershell
New-AzResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/subscription.json 
```

## <a name="next-steps"></a>Volgende stappen
* Zie voor een beschrijving van de secties in een Azure Resource Manager-sjabloon, [Authoring Azure Resource Manager-sjablonen](resource-group-authoring-templates.md).
* U kunt meerdere sjablonen samenvoegen, Zie [gekoppelde sjablonen gebruiken met Azure Resource Manager](resource-group-linked-templates.md).
* Op een opgegeven aantal keren herhalen bij het maken van een type resource, Zie [meerdere exemplaren van resources maken in Azure Resource Manager](resource-group-create-multiple.md).
* Zie voor meer informatie over het implementeren van de sjabloon die u hebt gemaakt, [een toepassing implementeren met Azure Resource Manager-sjabloon](resource-group-template-deploy.md).

