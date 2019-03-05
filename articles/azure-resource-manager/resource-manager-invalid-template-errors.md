---
title: Ongeldige Azure-sjabloon fouten | Microsoft Docs
description: Beschrijft hoe u ongeldige sjabloon oplossen.
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
ms.openlocfilehash: ee4bce38e0fcde93ba0417617ae90dab2eefda67
ms.sourcegitcommit: 3f4ffc7477cff56a078c9640043836768f212a06
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/04/2019
ms.locfileid: "57316197"
---
# <a name="resolve-errors-for-invalid-template"></a>Los fouten voor ongeldige sjabloon

In dit artikel wordt beschreven hoe u ongeldige sjabloon oplossen.

## <a name="symptom"></a>Symptoom

Bij het implementeren van een sjabloon, ontvangt u een fout die aangeeft:

```
Code=InvalidTemplate
Message=<varies>
```

Het foutbericht is afhankelijk van het type fout.

## <a name="cause"></a>Oorzaak

Deze fout kan worden veroorzaakt door verschillende typen fouten. Ze omvatten doorgaans een syntaxisfout of structurele fout in de sjabloon.

<a id="syntax-error" />

## <a name="solution-1---syntax-error"></a>Oplossing 1: syntaxisfout

Als u een foutbericht dat aangeeft de sjabloonvalidatie is mislukt ontvangt dat, hebt u mogelijk een probleem syntaxis in de sjabloon.

```
Code=InvalidTemplate
Message=Deployment template validation failed
```

Deze fout is eenvoudig te maken omdat het sjabloonexpressies kunnen ingewikkeld zijn. De naamtoewijzing van de volgende voor een storage-account heeft bijvoorbeeld één set met vierkante haken, drie functies, drie sets met behulp van haakjes, één set met enkele aanhalingstekens en één eigenschap:

```json
"name": "[concat('storage', uniqueString(resourceGroup().id))]",
```

Als u de syntaxis van overeenkomende niet opgeeft, wordt in de sjabloon een waarde die is anders dan uw bedoeling produceert.

Wanneer u dit type fout ontvangt, moet u zorgvuldig de expressiesyntaxis van controleren. Overweeg het gebruik van een JSON-editor, zoals [Visual Studio](vs-azure-tools-resource-groups-deployment-projects-create-deploy.md) of [Visual Studio Code](resource-manager-vs-code.md), die u kunt waarschuwen over syntaxisfouten.

<a id="incorrect-segment-lengths" />

## <a name="solution-2---incorrect-segment-lengths"></a>Oplossing 2 - onjuiste segmentlengte

Een ongeldige sjabloon-fout treedt op wanneer de resourcenaam is niet de juiste indeling.

```
Code=InvalidTemplate
Message=Deployment template validation failed: 'The template resource {resource-name}'
for type {resource-type} has incorrect segment lengths.
```

Een niveau root-bron moet één minder segment hebben in de naam dan in het resourcetype. Elk segment wordt onderscheiden door een schuine streep. In het volgende voorbeeld wordt het type heeft twee segmenten en de naam van de één segment, zodat er een **geldige naam**.

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

Hebben hetzelfde aantal segmenten voor de onderliggende resources, het type en de naam. Dit aantal segmenten zinvol is omdat de volledige naam en type van het onderliggende object bevat de naam van bovenliggend en het type. Daarom heeft de volledige naam nog steeds één minder segment dan het volledige type.

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

Aan de segmenten rechts kan met Resource Manager-typen die worden toegepast op resourceproviders lastig zijn. Bijvoorbeeld: een resourcevergrendeling toepassen op een website heeft een type met vier segmenten. Daarom is de naam van de drie segmenten:

```json
{
    "type": "Microsoft.Web/sites/providers/locks",
    "name": "[concat(variables('siteName'),'/Microsoft.Authorization/MySiteLock')]",
    ...
}
```

<a id="parameter-not-valid" />

## <a name="solution-3---parameter-is-not-valid"></a>Oplossing 3 - parameter is niet geldig

Als u een parameterwaarde die niet een van de toegestane waarden opgeven, ontvangt u een bericht dat lijkt op de volgende fout:

```
Code=InvalidTemplate;
Message=Deployment template validation failed: 'The provided value {parameter value}
for the template parameter {parameter name} is not valid. The parameter value is not
part of the allowed values
```

Dubbele controleren van de toegestane waarden in de sjabloon en opgeeft tijdens de implementatie. Zie voor meer informatie over de toegestane parameterwaarden [sectie opdrachtregelparameters van Azure Resource Manager-sjablonen](resource-group-authoring-templates.md#parameters).

<a id="too-many-resource-groups" />

## <a name="solution-4---too-many-target-resource-groups"></a>Oplossing 4: te veel resourcegroepen van doelserver

Als u meer dan vijf doelgroepen resource in een implementatie met één opgeeft, ontvangt deze foutmelding. Overweeg de consolidatie van het aantal resourcegroepen in uw implementatie of de implementatie van enkele van de sjablonen als afzonderlijke implementaties. Zie voor meer informatie, [implementeren-Azure-resources aan meer dan één abonnement of resourcegroep](resource-manager-cross-resource-group-deployment.md).

<a id="circular-dependency" />

## <a name="solution-5---circular-dependency-detected"></a>Oplossing 5 - circulaire afhankelijkheid gedetecteerd

U ontvangt deze foutmelding wanneer resources afhankelijk van elkaar op een manier waarmee wordt voorkomen de implementatie zijn niet kan worden gestart dat. Een combinatie van afhankelijkheden kunt u twee of meer resources die wachten op andere resources die ook wachten. Bijvoorbeeld resource1 is afhankelijk van resource3, resource2, is afhankelijk van resource1 en resource3 is afhankelijk van resource2. Meestal kunt u dit probleem oplossen door het verwijderen van onnodige afhankelijkheden.

Om op te lossen een circulaire afhankelijkheid:

1. In de sjabloon, de bron die is geïdentificeerd in de circulaire afhankelijkheid te vinden. 
2. Voor die bron, onderzoekt de **dependsOn** eigenschap en elk gebruik van de **verwijzing** functie om te zien welke resources die deze afhankelijk is. 
3. Bekijk deze resources om te zien welke bronnen ze afhankelijk zijn. Volg de afhankelijkheden totdat er een resource die is gebouwd op de oorspronkelijke bron.
5. Voor de resources die betrokken zijn bij een circulaire afhankelijkheid, zorgvuldig al het gebruik van de **dependsOn** eigenschap voor het identificeren van eventuele afhankelijkheden die niet nodig zijn. Verwijder deze afhankelijkheden. Als u niet zeker weet dat er een afhankelijkheid nodig is, probeert u deze verwijdert. 
6. De sjabloon opnieuw implementeert.

Verwijderen van waarden uit de **dependsOn** eigenschap kan fouten veroorzaken wanneer u de sjabloon implementeert. Als u een fout optreedt, moet u de afhankelijkheid terug naar de sjabloon toevoegen. 

Als deze aanpak is een circulaire afhankelijkheid niet is opgelost, kunt u overwegen om deel uit van de logica van uw implementatie in onderliggende resources (zoals extensies of configuratie-instellingen). Configureer de onderliggende resources na de resources die betrokken zijn bij een circulaire afhankelijkheid te implementeren. Stel bijvoorbeeld dat u twee virtuele machines implementeert, maar moet u de eigenschappen instellen op elk die verwijzen naar de andere. U kunt deze implementeren in de volgende volgorde:

1. vm1
2. vm2
3. Extensie op vm1, is afhankelijk van vm1 en vm2. De extensie stelt waarden in voor vm1 die van vm2 krijgt.
4. Extensie op vm2, is afhankelijk van vm1 en vm2. De extensie stelt waarden in voor vm2 die van vm1 krijgt.

Diezelfde aanpak werkt voor App Service-apps. Houd rekening met configuratiewaarden verplaatsen naar een onderliggende resource van de app-resource. U kunt twee web-apps in de volgende volgorde kunt implementeren:

1. webapp1
2. webapp2
3. configuratie voor webapp1, is afhankelijk van webapp1 en webapp2. Het bevat app-instellingen met waarden uit webapp2.
4. configuratie voor webapp2, is afhankelijk van webapp1 en webapp2. Het bevat app-instellingen met waarden uit webapp1.
