---
title: Ongeldige Azure-sjabloon fouten | Microsoft Docs
description: Beschrijft hoe ongeldige sjabloon fouten op te lossen.
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
ms.openlocfilehash: e58c10cfb4cdd4ba49945e6c19845cbc957d6326
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="resolve-errors-for-invalid-template"></a>Los de fouten voor ongeldige sjabloon

In dit artikel wordt beschreven hoe ongeldige sjabloon fouten op te lossen.

## <a name="symptom"></a>Symptoom

Bij het implementeren van een sjabloon, wordt een fout die aangeeft:

```
Code=InvalidTemplate
Message=<varies>
```

Het foutbericht is afhankelijk van het type fout.

## <a name="cause"></a>Oorzaak

Deze fout kan leiden tot verschillende typen fouten. Ze hebben meestal betrekking op een syntaxis of structurele fout in de sjabloon.

## <a name="solution"></a>Oplossing

### <a name="solution-1---syntax-error"></a>Oplossing 1 - syntaxisfout

Als u een foutbericht dat aangeeft de validatie van de sjabloon is mislukt ontvangt dat, hebt u mogelijk een probleem met de syntaxis in de sjabloon.

```
Code=InvalidTemplate
Message=Deployment template validation failed
```

Dit is een fout gemakkelijk maken omdat sjabloonexpressies kunnen ingewikkeld zijn. De naamtoewijzing van de volgende voor een opslagaccount bevat bijvoorbeeld een reeks vierkante haken, drie functies drie sets van haakjes, één set met enkele aanhalingstekens en één eigenschap:

```json
"name": "[concat('storage', uniqueString(resourceGroup().id))]",
```

Als u de syntaxis van de overeenkomende niet opgeeft, wordt in de sjabloon een waarde die is anders dan uw bedoeling produceert.

Wanneer u dit type fout ontvangt, zorgvuldig door de expressiesyntaxis van de. Overweeg het gebruik van een JSON-editor, zoals [Visual Studio](vs-azure-tools-resource-groups-deployment-projects-create-deploy.md) of [Visual Studio Code](resource-manager-vs-code.md), die u kunt gewaarschuwd over syntaxisfouten.

### <a name="solution-2---incorrect-segment-lengths"></a>Oplossing 2 - onjuist segment lengten

Een andere ongeldige sjabloon-fout treedt op wanneer de resourcenaam niet de juiste indeling is.

```
Code=InvalidTemplate
Message=Deployment template validation failed: 'The template resource {resource-name}'
for type {resource-type} has incorrect segment lengths.
```

Een bron voor het niveau van hoofdmap moet één minder segment hebben van de naam dan in het brontype. Elk segment wordt onderscheiden door een slash. In het volgende voorbeeld wordt het type heeft twee segmenten en de naam van de één segment dus is het een **geldige naam**.

```json
{
  "type": "Microsoft.Web/serverfarms",
  "name": "myHostingPlanName",
  ...
}
```

Maar het volgende voorbeeld is **niet de naam van een geldige** omdat er hetzelfde aantal segmenten als het type.

```json
{
  "type": "Microsoft.Web/serverfarms",
  "name": "appPlan/myHostingPlanName",
  ...
}
```

Hebben hetzelfde aantal segmenten voor onderliggende resources, het type en de naam. Dit aantal segmenten zin omdat de volledige naam en type voor het onderliggende element bevat de naam van het bovenliggende en het type. Daarom heeft de volledige naam nog steeds één minder segment dan het volledige type.

```json
"resources": [
    {
        "type": "Microsoft.KeyVault/vaults",
        "name": "contosokeyvault",
        ...
        "resources": [
            {
                "type": "secrets",
                "name": "appPassword",
                ...
            }
        ]
    }
]
```

Ophalen van de segmenten rechts is lastig met Resource Manager-typen die worden toegepast op de resourceproviders. Bijvoorbeeld, een resource-vergrendeling wordt toegepast op een website, is een type met vier segmenten vereist. Daarom is de naam van de drie segmenten:

```json
{
    "type": "Microsoft.Web/sites/providers/locks",
    "name": "[concat(variables('siteName'),'/Microsoft.Authorization/MySiteLock')]",
    ...
}
```

### <a name="solution-3---parameter-is-not-valid"></a>Oplossing 3 - parameter is niet geldig

Als de sjabloon toegestane waarden voor een parameter worden, en u een waarde die niet een van deze waarden opgeven, ontvangt u een bericht dat lijkt op de volgende fout:

```
Code=InvalidTemplate;
Message=Deployment template validation failed: 'The provided value {parameter value}
for the template parameter {parameter name} is not valid. The parameter value is not
part of the allowed values
```

Dubbele de toegestane waarden in de sjabloon en geef een tijdens de implementatie.

### <a name="solution-4---circular-dependency-detected"></a>Oplossing 4 - circulaire afhankelijkheid gedetecteerd

U ontvangt deze foutmelding wanneer resources afhankelijk van elkaar op een manier waardoor de implementatie zijn niet worden gestart. Een combinatie van afhankelijkheden maakt twee of meer resources, wacht u totdat de andere bronnen die ook wachten. Bijvoorbeeld resource1 is afhankelijk van resource3, resource2, is afhankelijk van resource1 en resource3 is afhankelijk van resource2. Meestal kunt u dit probleem oplossen door het verwijderen van onnodige afhankelijkheden.
