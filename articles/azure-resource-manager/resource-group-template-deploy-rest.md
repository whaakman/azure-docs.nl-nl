---
title: Resources implementeren met REST-API en een sjabloon | Microsoft Docs
description: Gebruik Azure Resource Manager en Resource Manager REST API om een resources implementeren op Azure. De resources zijn gedefinieerd in een Resource Manager-sjabloon.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 1d8fbd4c-78b0-425b-ba76-f2b7fd260b45
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/01/2018
ms.author: tomfitz
ms.openlocfilehash: ae2393d16d2c9c1000b00f5514e63c988303a83c
ms.sourcegitcommit: 4de6a8671c445fae31f760385710f17d504228f8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/08/2018
ms.locfileid: "39628508"
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

De sjabloon kan een lokaal bestand of een extern bestand dat is beschikbaar via een URI zijn. Wanneer de sjabloon bevindt zich in een storage-account, kunt u beperken van toegang aan de sjabloon en de token van een shared access signature (SAS) opgeven tijdens de implementatie.

## <a name="deploy-with-the-rest-api"></a>Implementeren met de REST-API
1. Stel [algemene parameters en headers](/rest/api/azure/), met inbegrip van verificatietokens.

2. Als u een bestaande resourcegroep hebt, maakt u een resourcegroep. Geef uw abonnements-ID, de naam van de nieuwe resourcegroep en locatie die u nodig hebt voor uw oplossing. Zie voor meer informatie, [een resourcegroep maken](/rest/api/resources/resourcegroups/createorupdate).

  ```HTTP
  PUT https://management.azure.com/subscriptions/<YourSubscriptionId>/resourcegroups/<YourResourceGroupName>?api-version=2015-01-01
  {
    "location": "West US",
    "tags": {
      "tagname1": "tagvalue1"
    }
  }
  ```

3. Controleren of uw implementatie voordat deze wordt uitgevoerd door het uitvoeren van de [controleren of de sjabloonimplementatie van een](/rest/api/resources/deployments/validate) bewerking. Bij het testen van de implementatie, parameters opgeven precies zoals u zou doen bij het uitvoeren van de implementatie (weergegeven in de volgende stap).

4. Maak een implementatie. Geef uw abonnements-ID, de naam van de resourcegroep, de naam van de implementatie en een koppeling naar uw sjabloon. Zie voor meer informatie over het sjabloonbestand [parameterbestand](#parameter-file). Zie voor meer informatie over de REST-API voor het maken van een resourcegroep, [maken van een sjabloonimplementatie](https://docs.microsoft.com/rest/api/resources/deployments#Deployments_CreateOrUpdate). U ziet dat de **modus** is ingesteld op **incrementele**. Als u wilt uitvoeren van een volledige implementatie, **modus** naar **voltooid**. Wees voorzichtig bij het gebruik van de volledige-modus, kunt u per ongeluk resources die zich niet in uw sjabloon verwijderen.

  ```HTTP
  PUT https://management.azure.com/subscriptions/<YourSubscriptionId>/resourcegroups/<YourResourceGroupName>/providers/Microsoft.Resources/deployments/<YourDeploymentName>?api-version=2015-01-01
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

  ```HTTP
  "debugSetting": {
    "detailLevel": "requestContent, responseContent"
  }
  ```

    U kunt uw storage-account instellen om te gebruiken van een token shared access signature (SAS). Zie voor meer informatie, [toegang delegeren met een Shared Access Signature](https://docs.microsoft.com/rest/api/storageservices/delegating-access-with-a-shared-access-signature).

5. Haal de status van de sjabloonimplementatie. Zie voor meer informatie, [informatie over de sjabloonimplementatie van een](/rest/api/resources/deployments/get).

  ```HTTP
  GET https://management.azure.com/subscriptions/<YourSubscriptionId>/resourcegroups/<YourResourceGroupName>/providers/Microsoft.Resources/deployments/<YourDeploymentName>?api-version=2015-01-01
  ```

## <a name="redeploy-when-deployment-fails"></a>Opnieuw implementeren wanneer de implementatie mislukt

Voor implementaties die niet voldoen, kunt u opgeven dat van de implementatiegeschiedenis van uw van een eerdere implementatie automatisch opnieuw worden geïmplementeerd. Als u wilt deze optie gebruikt, moeten uw implementaties unieke namen hebben, zodat ze kunnen worden geïdentificeerd in de geschiedenis. Als u geen unieke namen, overschrijft de huidige mislukte implementatie mogelijk de eerder geslaagde implementatie in de geschiedenis. U kunt deze optie alleen gebruiken met niveau root-implementaties. Implementaties van een geneste sjabloon zijn niet beschikbaar voor opnieuw implementeren.

Als u wilt implementeren de laatste geslaagde implementatie als de huidige implementatie mislukt, gebruikt u:

```HTTP
"onErrorDeployment": {
  "type": "LastSuccessful",
},
```

Als u wilt een specifieke implementatie opnieuw implementeren als de huidige implementatie mislukt, gebruikt u:

```HTTP
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

