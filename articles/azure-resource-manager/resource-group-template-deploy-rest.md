---
title: Resources implementeren met REST-API en een sjabloon | Microsoft Docs
description: Gebruik Azure Resource Manager en Resource Manager REST API om een resources implementeren op Azure. De resources zijn gedefinieerd in een Resource Manager-sjabloon.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
ms.assetid: 1d8fbd4c-78b0-425b-ba76-f2b7fd260b45
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/26/2018
ms.author: tomfitz
ms.openlocfilehash: 058d6d398f6bb54e8569e727f118a325c338049d
ms.sourcegitcommit: 48592dd2827c6f6f05455c56e8f600882adb80dc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/26/2018
ms.locfileid: "50154739"
---
# <a name="deploy-resources-with-resource-manager-templates-and-resource-manager-rest-api"></a>Resources implementeren met Resource Manager-sjablonen en Resource Manager REST API

In dit artikel wordt uitgelegd hoe u de Resource Manager REST-API gebruiken met Resource Manager-sjablonen voor het implementeren van uw resources naar Azure.  

> [!TIP]
> Zie voor hulp bij foutopsporing in een fout opgetreden tijdens de implementatie:
> 
> * [Implementatiebewerkingen bekijken](resource-manager-deployment-operations.md) voor meer informatie over het ophalen van gegevens waarmee u uw fout oplossen
> * [Oplossen van veelvoorkomende fouten bij het implementeren van resources naar Azure met Azure Resource Manager](resource-manager-common-deployment-errors.md) voor meer informatie over het oplossen van veelvoorkomende implementatiefouten
> 
> 

U kunt uw sjabloon opnemen in de hoofdtekst van de aanvraag of een koppeling naar een bestand. Wanneer u een bestand, kan een lokaal bestand of een extern bestand dat is beschikbaar via een URI zijn. Als de sjabloon in een storage-account is, kunt u beperken van toegang aan de sjabloon en de token van een shared access signature (SAS) opgeven tijdens de implementatie.

## <a name="deploy-with-the-rest-api"></a>Implementeren met de REST-API
1. Stel [algemene parameters en headers](/rest/api/azure/), met inbegrip van verificatietokens.

1. Als u een bestaande resourcegroep hebt, maakt u een resourcegroep. Geef uw abonnements-ID, de naam van de nieuwe resourcegroep en locatie die u nodig hebt voor uw oplossing. Zie voor meer informatie, [een resourcegroep maken](/rest/api/resources/resourcegroups/createorupdate).

  ```HTTP
  PUT https://management.azure.com/subscriptions/<YourSubscriptionId>/resourcegroups/<YourResourceGroupName>?api-version=2018-05-01
  ```

  Met een aanvraagtekst, zoals:
  ```json
  {
    "location": "West US",
    "tags": {
      "tagname1": "tagvalue1"
    }
  }
  ```

1. Controleren of uw implementatie voordat deze wordt uitgevoerd door het uitvoeren van de [controleren of de sjabloonimplementatie van een](/rest/api/resources/deployments/validate) bewerking. Bij het testen van de implementatie, parameters opgeven precies zoals u zou doen bij het uitvoeren van de implementatie (weergegeven in de volgende stap).

1. Maak een implementatie. Geef uw abonnements-ID, de naam van de resourcegroep, de naam van de implementatie en een koppeling naar uw sjabloon. Zie voor meer informatie over het sjabloonbestand [parameterbestand](#parameter-file). Zie voor meer informatie over de REST-API voor het maken van een resourcegroep, [maken van een sjabloonimplementatie](/rest/api/resources/deployments/createorupdate). U ziet dat de **modus** is ingesteld op **incrementele**. Als u wilt uitvoeren van een volledige implementatie, **modus** naar **voltooid**. Wees voorzichtig bij het gebruik van de volledige-modus, kunt u per ongeluk resources die zich niet in uw sjabloon verwijderen.

  ```HTTP
  PUT https://management.azure.com/subscriptions/<YourSubscriptionId>/resourcegroups/<YourResourceGroupName>/providers/Microsoft.Resources/deployments/<YourDeploymentName>?api-version=2018-05-01
  ```

  Met een aanvraagtekst, zoals:

   ```json
  {
    "properties": {
      "templateLink": {
        "uri": "http://mystorageaccount.blob.core.windows.net/templates/template.json",
        "contentVersion": "1.0.0.0"
      },
      "mode": "Incremental",
      "parametersLink": {
        "uri": "http://mystorageaccount.blob.core.windows.net/templates/parameters.json",
        "contentVersion": "1.0.0.0"
      }
    }
  }
  ```

    Als u wilt vastleggen van de inhoud van de reactie en/of de inhoud van de aanvraag, **debugSetting** in de aanvraag.

  ```json
  "debugSetting": {
    "detailLevel": "requestContent, responseContent"
  }
  ```

    U kunt uw storage-account instellen om te gebruiken van een token shared access signature (SAS). Zie voor meer informatie, [toegang delegeren met een Shared Access Signature](https://docs.microsoft.com/rest/api/storageservices/delegating-access-with-a-shared-access-signature).

1. In plaats van koppelingen naar bestanden voor de sjabloon en parameters, kunt u ze opneemt in de aanvraagtekst.

  ```json
  {
      "properties": {
      "mode": "Incremental",
      "template": {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
          "storageAccountType": {
            "type": "string",
            "defaultValue": "Standard_LRS",
            "allowedValues": [
              "Standard_LRS",
              "Standard_GRS",
              "Standard_ZRS",
              "Premium_LRS"
            ],
            "metadata": {
              "description": "Storage Account type"
            }
          },
          "location": {
            "type": "string",
            "defaultValue": "[resourceGroup().location]",
            "metadata": {
              "description": "Location for all resources."
            }
          }
        },
        "variables": {
          "storageAccountName": "[concat(uniquestring(resourceGroup().id), 'standardsa')]"
        },
        "resources": [
          {
            "type": "Microsoft.Storage/storageAccounts",
            "name": "[variables('storageAccountName')]",
            "apiVersion": "2018-02-01",
            "location": "[parameters('location')]",
            "sku": {
              "name": "[parameters('storageAccountType')]"
            },
            "kind": "StorageV2",
            "properties": {}
          }
        ],
        "outputs": {
          "storageAccountName": {
            "type": "string",
            "value": "[variables('storageAccountName')]"
          }
        }
      },
      "parameters": {
        "location": {
          "value": "eastus2"
        }
      }
    }
  }
  ```

5. Haal de status van de sjabloonimplementatie. Zie voor meer informatie, [informatie over de sjabloonimplementatie van een](/rest/api/resources/deployments/get).

  ```HTTP
  GET https://management.azure.com/subscriptions/<YourSubscriptionId>/resourcegroups/<YourResourceGroupName>/providers/Microsoft.Resources/deployments/<YourDeploymentName>?api-version=2018-05-01
  ```

## <a name="redeploy-when-deployment-fails"></a>Opnieuw implementeren wanneer de implementatie mislukt

Voor implementaties die niet voldoen, kunt u opgeven dat van de implementatiegeschiedenis van uw van een eerdere implementatie automatisch opnieuw worden geïmplementeerd. Als u wilt deze optie gebruikt, moeten uw implementaties unieke namen hebben, zodat ze kunnen worden geïdentificeerd in de geschiedenis. Als u geen unieke namen, overschrijft de huidige mislukte implementatie mogelijk de eerder geslaagde implementatie in de geschiedenis. U kunt deze optie alleen gebruiken met niveau root-implementaties. Implementaties van een geneste sjabloon zijn niet beschikbaar voor opnieuw implementeren.

Als u wilt implementeren de laatste geslaagde implementatie als de huidige implementatie mislukt, gebruikt u:

```json
"onErrorDeployment": {
  "type": "LastSuccessful",
},
```

Als u wilt een specifieke implementatie opnieuw implementeren als de huidige implementatie mislukt, gebruikt u:

```json
"onErrorDeployment": {
  "type": "SpecificDeployment",
  "deploymentName": "<deploymentname>"
}
```

De opgegeven implementatie moet zijn geslaagd.

## <a name="parameter-file"></a>Parameterbestand

Als u een parameterbestand parameterwaarden doorgeven tijdens de implementatie hebt gebruikt, moet u een JSON-bestand maken met een indeling die vergelijkbaar is met het volgende voorbeeld:

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

De grootte van de parameterbestand mag niet meer dan 64 KB.

Als u wilt een gevoelige waarde opgeven voor een parameter (zoals een wachtwoord), moet u die waarde toevoegen aan een key vault. De key vault ophalen tijdens de implementatie, zoals wordt weergegeven in het vorige voorbeeld. Zie voor meer informatie, [beveiligde waarden doorgeven tijdens implementatie](resource-manager-keyvault-parameter.md). 

## <a name="next-steps"></a>Volgende stappen
* Als u wilt opgeven voor het verwerken van resources die aanwezig zijn in de resourcegroep, maar niet zijn gedefinieerd in de sjabloon, Zie [Azure Resource Manager-implementatiemodi](deployment-modes.md).
* Zie voor meer informatie over het verwerken van asynchrone REST-bewerkingen, [asynchrone bewerkingen van Azure bijhouden](resource-manager-async-operations.md).
* Zie voor een voorbeeld van de implementatie van resources via de .NET-clientbibliotheek [resources implementeren met behulp van .NET-bibliotheken en een sjabloon](../virtual-machines/windows/csharp-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
* Zie voor het definiëren van parameters in sjabloon, [-sjablonen maken](resource-group-authoring-templates.md#parameters).
* Voor begeleiding bij de manier waarop ondernemingen Resource Manager effectief kunnen gebruiken voor het beheer van abonnementen, gaat u naar [Azure enterprise-platform - Prescriptieve abonnementsgovernance](/azure/architecture/cloud-adoption-guide/subscription-governance).

