---
title: Azure-resource niet gevonden fouten | Microsoft Docs
description: Beschrijft hoe u fouten oplossen wanneer een resource kan niet worden gevonden.
services: azure-resource-manager
documentationcenter: ''
author: tfitzmac
manager: timlt
editor: ''
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: troubleshooting
ms.date: 03/08/2018
ms.author: tomfitz
ms.openlocfilehash: f5da2a74b3a399c60c518f386ccf2e60a617aeda
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/20/2018
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

## <a name="solution-1---set-dependencies"></a>Oplossing 1 - set afhankelijkheden

Als u de ontbrekende resource in de sjabloon implementeert probeert, controleert u of u moet een afhankelijkheid toevoegen. Resource Manager optimaliseert implementatie door het maken van resources parallel indien mogelijk. Als een resource moet worden geïmplementeerd nadat een andere resource, moet u de **dependsOn** element in de sjabloon. Bijvoorbeeld, wanneer u een web-app implementeert, moet de App Service-abonnement bestaan. Als u dit nog niet hebt opgegeven dat de web-app, afhankelijk van de App Service-abonnement, maakt Resource Manager beide resources op hetzelfde moment. U krijgt een fout met de mededeling dat de App Service plan bron kan niet worden gevonden, omdat deze nog niet bestaat bij een poging tot een eigenschap instellen voor de web-app. U kunt deze fout voorkomen door het instellen van de afhankelijkheid in de web-app.

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

Maar u wilt voorkomen dat afhankelijkheden die nodig zijn niet instellen. Wanneer u onnodige afhankelijkheden hebt, kunt u de duur van de implementatie verlengen door te voorkomen dat de bronnen die niet afhankelijk van elkaar worden geïmplementeerd parallel. U kunt bovendien circulaire afhankelijkheden die de implementatie blokkeren maken. De [verwijzing](resource-group-template-functions-resource.md#reference) functie maakt een impliciete afhankelijkheid voor de bron waarnaar wordt verwezen, wanneer deze resource is geïmplementeerd in dezelfde sjabloon. Daarom kunnen er meer afhankelijkheden dan de afhankelijkheden die zijn opgegeven de **dependsOn** eigenschap. De [resourceId](resource-group-template-functions-resource.md#resourceid) functie niet maken van een impliciete afhankelijkheid of valideren dat de resource bestaat.

Wanneer u afhankelijkheid problemen ondervindt, moet u meer inzicht krijgen in de volgorde van de resource-implementatie. De volgorde van implementatiebewerkingen weergeven:

1. Selecteer de geschiedenis van de implementatie voor de resourcegroep.

   ![Geschiedenis van implementatie selecteren](./media/resource-manager-not-found-errors/select-deployment.png)

2. Selecteer een implementatie van de geschiedenis en selecteer **gebeurtenissen**.

   ![gebeurtenissen van de implementatie selecteren](./media/resource-manager-not-found-errors/select-deployment-events.png)

3. Controleer de volgorde van gebeurtenissen voor elke resource. Let op de status van elke bewerking. De volgende afbeelding ziet u bijvoorbeeld drie storage-accounts die geïmplementeerd parallel. U ziet dat de drie storage-accounts op hetzelfde moment worden gestart.

   ![Parallelle implementatie](./media/resource-manager-not-found-errors/deployment-events-parallel.png)

   De volgende afbeelding ziet u drie opslagaccounts die niet zijn geïmplementeerd parallel. Het tweede storage-account is afhankelijk van het eerste storage-account en het derde storage-account is afhankelijk van de tweede storage-account. Het eerste opslagaccount wordt gestart, geaccepteerd en voltooid voordat de volgende is gestart.

   ![sequentiële implementatie](./media/resource-manager-not-found-errors/deployment-events-sequence.png)

## <a name="solution-2---get-resource-from-different-resource-group"></a>Oplossing 2 - resource ophalen uit de andere resourcegroep

Wanneer de bron in een andere resourcegroep dan de versie die wordt geïmplementeerd bestaat, gebruiken de [resourceId functie](resource-group-template-functions-resource.md#resourceid) ophalen van de volledig gekwalificeerde naam van de resource.

```json
"properties": {
    "name": "[parameters('siteName')]",
    "serverFarmId": "[resourceId('plangroup', 'Microsoft.Web/serverfarms', parameters('hostingPlanName'))]"
}
```

## <a name="solution-3---check-reference-function"></a>Oplossing 3 - controle van verwijzing naar functie

Zoek naar een expressie met de [verwijzing](resource-group-template-functions-resource.md#reference) functie. De waarden die u opgeeft variëren, afhankelijk van of de resource in de sjabloon, resourcegroep en -abonnement is. Controleer dat u de vereiste parameterwaarden voor uw scenario opgeeft. Als de bron in een andere resourcegroep is, geeft u de volledige resource-ID. Bijvoorbeeld, om te verwijzen naar een opslagaccount in een andere resourcegroep, gebruiken:

```json
"[reference(resourceId('exampleResourceGroup', 'Microsoft.Storage/storageAccounts', 'myStorage'), '2017-06-01')]"
```