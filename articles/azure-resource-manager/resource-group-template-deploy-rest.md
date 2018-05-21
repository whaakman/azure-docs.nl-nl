---
title: Resources met REST-API en de sjabloon implementeren | Microsoft Docs
description: Azure Resource Manager en de REST-API van Resource Manager gebruiken voor het implementeren van een bronnen in Azure. De resources zijn gedefinieerd in een Resource Manager-sjabloon.
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
ms.openlocfilehash: 6915395abfedb75ec3ec0a5bd8f569ef2490e5be
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/20/2018
---
# <a name="deploy-resources-with-resource-manager-templates-and-resource-manager-rest-api"></a>Resources implementeren met Resource Manager-sjablonen en Resource Manager REST API

In dit artikel wordt uitgelegd hoe de REST-API van Resource Manager gebruiken met Resource Manager-sjablonen voor het implementeren van uw resources in Azure.  

> [!TIP]
> Zie voor informatie over wanneer foutopsporing is een fout opgetreden tijdens de implementatie:
> 
> * [Implementatiebewerkingen weergeven](resource-manager-deployment-operations.md) voor meer informatie over het ophalen van informatie die helpt u de fout oplossen
> * [Oplossen van veelvoorkomende fouten bij het implementeren van resources in Azure met Azure Resource Manager](resource-manager-common-deployment-errors.md) voor meer informatie over het oplossen van veelvoorkomende implementatiefouten
> 
> 

De sjabloon is een lokaal bestand of een extern bestand dat is beschikbaar via een URI. Wanneer de sjabloon bevindt zich in een opslagaccount, kunt u het beperken van toegang aan de sjabloon en een shared access signature (SAS)-token opgeven tijdens de implementatie.

[!INCLUDE [resource-manager-deployments](../../includes/resource-manager-deployments.md)]

## <a name="deploy-with-the-rest-api"></a>Met de REST-API implementeren
1. Stel [algemene parameters en -koppen](/rest/api/azure/), met inbegrip van verificatietokens.

2. Als u een bestaande resourcegroep niet hebt, kunt u een resourcegroep maken. Geef uw abonnements-ID, de naam van de nieuwe resourcegroep en locatie die u nodig hebt voor uw oplossing. Zie voor meer informatie [een resourcegroep maken](/rest/api/resources/resourcegroups/createorupdate).

  ```HTTP
  PUT https://management.azure.com/subscriptions/<YourSubscriptionId>/resourcegroups/<YourResourceGroupName>?api-version=2015-01-01
  {
    "location": "West US",
    "tags": {
      "tagname1": "tagvalue1"
    }
  }
  ```

3. Valideren van uw implementatie voordat deze wordt uitgevoerd door het uitvoeren van de [valideren van de sjabloonimplementatie van een](/rest/api/resources/deployments/validate) bewerking. Bij het testen van de implementatie, Geef parameters op exact dezelfde manier als bij het uitvoeren van de implementatie (weergegeven in de volgende stap).

4. Maakt een implementatie. Geef uw abonnements-ID, de naam van de resourcegroep, de naam van de implementatie en een koppeling naar uw sjabloon. Zie voor meer informatie over het sjabloonbestand [parameterbestand](#parameter-file). Zie voor meer informatie over de REST-API om een resourcegroep te maken, [sjabloonimplementatie met een maakt](https://docs.microsoft.com/rest/api/resources/deployments#Deployments_CreateOrUpdate). U ziet de **modus** is ingesteld op **incrementele**. Als u wilt uitvoeren van een volledige implementatie, **modus** naar **Complete**. Wees voorzichtig met behulp van de volledige modus kunt u de resources die zich niet in uw sjabloon per ongeluk verwijderen.

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

    Als u registreren antwoordinhoud, aanvraaginhoud of beide wilt, opnemen **debugSetting** in de aanvraag.

  ```HTTP
  "debugSetting": {
    "detailLevel": "requestContent, responseContent"
  }
  ```

    U kunt uw storage-account instellen om te gebruiken een shared access signature (SAS)-token. Zie voor meer informatie [toegang delegeren met Shared Access Signature](https://docs.microsoft.com/rest/api/storageservices/delegating-access-with-a-shared-access-signature).

5. De status van de sjabloonimplementatie niet ophalen. Zie voor meer informatie [informatie ophalen over de sjabloonimplementatie van een](/rest/api/resources/deployments/get).

  ```HTTP
  GET https://management.azure.com/subscriptions/<YourSubscriptionId>/resourcegroups/<YourResourceGroupName>/providers/Microsoft.Resources/deployments/<YourDeploymentName>?api-version=2015-01-01
  ```

## <a name="redeploy-when-deployment-fails"></a>Implementeer opnieuw wanneer de implementatie mislukt

Voor implementaties die niet voldoen, kunt u opgeven dat van de geschiedenis van uw implementatie van een eerdere implementatie automatisch opnieuw worden geïmplementeerd. Om deze optie gebruikt, moeten uw implementaties van unieke namen hebben zodat ze kunnen worden geïdentificeerd in de geschiedenis. Als u geen unieke namen, overschrijft de huidige mislukte implementatie mogelijk de eerder geslaagde implementatie in de geschiedenis. U kunt deze optie alleen gebruiken met niveau basis-implementaties. Implementaties van een geneste sjabloon niet beschikbaar voor opnieuw installeren.

Als u wilt implementeren de laatste geslaagde implementatie als de huidige implementatie mislukt, gebruikt u:

```HTTP
"onErrorDeployment": {
  "type": "LastSuccessful",
},
```

Als u wilt implementeren een specifieke implementatie als de huidige implementatie mislukt, gebruikt u:

```HTTP
"onErrorDeployment": {
  "type": "SpecificDeployment",
  "deploymentName": "<deploymentname>"
}
```

De opgegeven implementatie moet zijn geslaagd.

## <a name="parameter-file"></a>Parameterbestand

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

Als u wilt een gevoelige waarde opgeven voor een parameter (zoals een wachtwoord), moet u die waarde toevoegen aan een sleutelkluis. De sleutelkluis ophalen tijdens de implementatie, zoals wordt weergegeven in het vorige voorbeeld. Zie voor meer informatie [beveiligde waarden doorgeven tijdens de implementatie van](resource-manager-keyvault-parameter.md). 

## <a name="next-steps"></a>Volgende stappen
* Zie voor meer informatie over het verwerken van asynchrone REST-bewerkingen, [bijhouden asynchrone Azure bewerkingen](resource-manager-async-operations.md).
* Zie voor een voorbeeld van het implementeren van resources via de .NET-clientbibliotheek [resources met behulp van .NET-bibliotheken en een sjabloon implementeren](../virtual-machines/windows/csharp-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
* Om parameters te definiëren in de sjabloon, Zie [sjablonen](resource-group-authoring-templates.md#parameters).
* Zie voor instructies over het implementeren van uw oplossing in verschillende omgevingen [Ontwikkeling en testomgevingen in Microsoft Azure](solution-dev-test-environments.md).
* Voor begeleiding bij de manier waarop ondernemingen Resource Manager effectief kunnen gebruiken voor het beheer van abonnementen, gaat u naar [Azure enterprise-platform - Prescriptieve abonnementsgovernance](resource-manager-subscription-governance.md).

