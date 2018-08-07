---
title: Azure-resource niet vinden fouten | Microsoft Docs
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
ms.date: 06/06/2018
ms.author: tomfitz
ms.openlocfilehash: 176de6f19274dfd8a6cf0335bb4cf16a8baa874b
ms.sourcegitcommit: 9819e9782be4a943534829d5b77cf60dea4290a2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/06/2018
ms.locfileid: "39525341"
---
# <a name="resolve-not-found-errors-for-azure-resources"></a>Los fouten niet gevonden voor de Azure-resources

Dit artikel beschrijft de fouten die mogelijk ziet u wanneer een resource kan niet worden gevonden tijdens de implementatie.

## <a name="symptom"></a>Symptoom

Wanneer de sjabloon de naam van een resource die kan niet worden omgezet bevat, ontvangt u een fout op vergelijkbaar met:

```
Code=NotFound;
Message=Cannot find ServerFarm with name exampleplan.
```

Als u de [verwijzing](resource-group-template-functions-resource.md#reference) of [listKeys](resource-group-template-functions-resource.md#listkeys) functies met een resource die kan niet worden omgezet, ontvangt u de volgende fout:

```
Code=ResourceNotFound;
Message=The Resource 'Microsoft.Storage/storageAccounts/{storage name}' under resource
group {resource group name} was not found.
```

## <a name="cause"></a>Oorzaak

Resource Manager nodig heeft om op te halen van de eigenschappen voor een resource, maar de resource in uw abonnement kan niet worden geïdentificeerd.

## <a name="solution-1---set-dependencies"></a>Oplossing 1: set-afhankelijkheden

Als u de ontbrekende resource in de sjabloon implementeert probeert, controleert u of u wilt toevoegen van een afhankelijkheid. Resource Manager optimaliseert implementatie door het maken van resources tegelijk, indien mogelijk. Als een resource moet worden geïmplementeerd nadat een andere resource, moet u de **dependsOn** element in de sjabloon. Bijvoorbeeld, wanneer u een web-app implementeert, moet de App Service-plan bestaan. Als u dit nog niet hebt opgegeven dat de web-app afhankelijk van het App Service-plan is, Resource Manager wordt gemaakt van beide bronnen op hetzelfde moment. Er treedt een fout met de mededeling dat de App Service plan resource kan niet worden gevonden, omdat deze nog niet bestaat wanneer wordt geprobeerd een eigenschap instellen op de web-app. U kunt deze fout voorkomen door in te stellen van de afhankelijkheid in de web-app.

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

Maar u wilt voorkomen dat de afhankelijkheden die nodig zijn niet instellen. Wanneer u onnodige afhankelijkheden hebt, kunt u de duur van de implementatie verlengen door te voorkomen dat resources die niet afhankelijk van elkaar worden geïmplementeerd in parallelle. U kunt bovendien circulaire afhankelijkheden die de implementatie blokkeert maken. De [verwijzing](resource-group-template-functions-resource.md#reference) functie en [lijst *](resource-group-template-functions-resource.md#list) functies maakt een impliciete afhankelijkheid van de resource waarnaar wordt verwezen, wanneer deze resource is geïmplementeerd in dezelfde sjabloon en wordt verwezen door de naam (geen resource-ID ). Daarom moet u wellicht meer afhankelijkheden dan de afhankelijkheden die zijn opgegeven de **dependsOn** eigenschap. De [resourceId](resource-group-template-functions-resource.md#resourceid) functie niet afhankelijk van een impliciete maken of te valideren dat de resource bestaat. De [verwijzing](resource-group-template-functions-resource.md#reference) functie en [lijst *](resource-group-template-functions-resource.md#list) functies niet afhankelijk van een impliciete maken wanneer de resource wordt verwezen door de resource-ID. Geeft de naam van de resource die is geïmplementeerd in dezelfde sjabloon voor het maken van een impliciete afhankelijkheid.

Wanneer u problemen met afhankelijkheid ziet, moet u meer inzicht krijgen in de volgorde van de resource-implementatie. De volgorde van implementatiebewerkingen weergeven:

1. Selecteer de geschiedenis van de implementatie voor de resourcegroep.

   ![Selecteer de implementatiegeschiedenis](./media/resource-manager-not-found-errors/select-deployment.png)

2. Selecteer een implementatie uit de geschiedenis, en selecteer **gebeurtenissen**.

   ![gebeurtenissen van de implementatie selecteren](./media/resource-manager-not-found-errors/select-deployment-events.png)

3. Controleer de volgorde van gebeurtenissen voor elke resource. Let op de status van elke bewerking. De volgende afbeelding ziet u bijvoorbeeld drie opslagaccounts die geïmplementeerd parallel. U ziet dat de drie opslagaccounts op hetzelfde moment worden gestart.

   ![parallelle implementatie](./media/resource-manager-not-found-errors/deployment-events-parallel.png)

   De volgende afbeelding ziet u drie opslagaccounts die niet zijn geïmplementeerd parallel. Het tweede opslagaccount, is afhankelijk van de eerste storage-account en het derde storage-account is afhankelijk van de tweede storage-account. De eerste storage-account is gestart, geaccepteerd en voltooid voordat het volgende wordt gestart.

   ![sequentiële implementatie](./media/resource-manager-not-found-errors/deployment-events-sequence.png)

## <a name="solution-2---get-resource-from-different-resource-group"></a>Oplossing 2: get-resource in een andere resourcegroep

Wanneer de resource is opgeslagen in een andere resourcegroep dan de versie die op worden geïmplementeerd, gebruikt u de [resourceId functie](resource-group-template-functions-resource.md#resourceid) om op te halen van de volledig gekwalificeerde naam van de resource.

```json
"properties": {
    "name": "[parameters('siteName')]",
    "serverFarmId": "[resourceId('plangroup', 'Microsoft.Web/serverfarms', parameters('hostingPlanName'))]"
}
```

## <a name="solution-3---check-reference-function"></a>Oplossing 3 - functie voor controle-verwijzing

Zoek naar een expressie met de [verwijzing](resource-group-template-functions-resource.md#reference) functie. De waarden die u opgeeft, afhankelijk van of de resource de dezelfde sjabloon, resourcegroep en -abonnement heeft. Controleer dat u de vereiste parameterwaarden voor uw scenario bieden bent. Als de resource in een andere resourcegroep, geeft u de volledige resource-ID. Bijvoorbeeld, om te verwijzen naar een opslagaccount in een andere resourcegroep, gebruiken:

```json
"[reference(resourceId('exampleResourceGroup', 'Microsoft.Storage/storageAccounts', 'myStorage'), '2017-06-01')]"
```