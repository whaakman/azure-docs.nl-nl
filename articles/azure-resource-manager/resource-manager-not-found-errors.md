---
title: Azure-resource niet gevonden fouten | Microsoft Docs
description: Beschrijft hoe u fouten oplossen wanneer een resource kan niet worden gevonden.
services: azure-resource-manager,azure-portal
documentationcenter: 
author: tfitzmac
manager: timlt
editor: 
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: support-article
ms.date: 09/13/2017
ms.author: tomfitz
ms.openlocfilehash: c76c965c43ca8217faa9488c01975ce09a21daaf
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="resolve-not-found-errors-for-azure-resources"></a>Los fouten niet worden gevonden voor de Azure-resources

In dit artikel beschrijft de fouten die optreden kunnen wanneer een resource kan niet worden gevonden tijdens de implementatie. 

## <a name="symptom"></a>Symptoom

Wanneer de sjabloon de naam van een resource die niet kan worden omgezet bevat, wordt een foutbericht zoals:

```
Code=NotFound;
Message=Cannot find ServerFarm with name exampleplan.
```

Als u probeert te gebruiken de [verwijzing](resource-group-template-functions-resource.md#reference) of [listKeys](resource-group-template-functions-resource.md#listkeys) werkt in combinatie met een resource die niet kunnen worden omgezet, het volgende foutbericht:

```
Code=ResourceNotFound;
Message=The Resource 'Microsoft.Storage/storageAccounts/{storage name}' under resource
group {resource group name} was not found.
```

## <a name="cause"></a>Oorzaak

Resource Manager hoeft op te halen van de eigenschappen voor een bron, maar de bron in uw abonnement kan niet worden geïdentificeerd.

## <a name="solution"></a>Oplossing

### <a name="solution-1"></a>Oplossing 1

Als u probeert de ontbrekende resource in de sjabloon te implementeren, controleert u of u moet een afhankelijkheid toevoegen. Resource Manager optimaliseert implementatie door het maken van resources parallel indien mogelijk. Als een resource moet worden geïmplementeerd nadat een andere resource, moet u de **dependsOn** element in de sjabloon voor het maken van een afhankelijkheid op de andere resource. Bijvoorbeeld, wanneer u een web-app implementeert, moet de App Service-abonnement bestaan. Als de web-app is afhankelijk van de App Service-abonnement is niet opgegeven, wordt met Resource Manager beide resources maakt op hetzelfde moment. U ontvangt een foutmelding weergegeven dat de resource voor de App Service-abonnement kan niet worden gevonden, omdat deze niet bestaat nog bij een poging tot een eigenschap instellen voor de web-app. U kunt deze fout voorkomen door het instellen van de afhankelijkheid in de web-app.

```json
{
  "apiVersion": "2015-08-01",
  "type": "Microsoft.Web/sites",
  "dependsOn": [
    "[variables('hostingPlanName')]"
  ],
  ...
}
```

Zie voor suggesties over het oplossen van afhankelijkheidsfouten [volgorde van de implementatie controleren](resource-manager-troubleshoot-tips.md#check-deployment-sequence).

### <a name="solution-2"></a>Oplossing 2

Wanneer de bron in een andere resourcegroep dan de versie die wordt geïmplementeerd bestaat, gebruiken de [resourceId functie](resource-group-template-functions-resource.md#resourceid) ophalen van de volledig gekwalificeerde naam van de resource.

```json
"properties": {
    "name": "[parameters('siteName')]",
    "serverFarmId": "[resourceId('plangroup', 'Microsoft.Web/serverfarms', parameters('hostingPlanName'))]"
}
```

### <a name="solution-3"></a>Oplossing 3

Zoek naar een expressie met de [verwijzing](resource-group-template-functions-resource.md#reference) functie. De waarden die u opgeeft variëren, afhankelijk van of de resource in de sjabloon, resourcegroep en -abonnement is. Controleer dat u de vereiste parameterwaarden voor uw scenario opgeeft. Als de bron in een andere resourcegroep is, geeft u de volledige resource-ID. Bijvoorbeeld, om te verwijzen naar een opslagaccount in een andere resourcegroep, gebruiken:

```json
"[reference(resourceId('exampleResourceGroup', 'Microsoft.Storage/storageAccounts', 'myStorage'), '2017-06-01')]"
```