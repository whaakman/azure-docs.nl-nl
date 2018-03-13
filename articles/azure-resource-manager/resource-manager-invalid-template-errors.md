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
ms.date: 03/08/2018
ms.author: tomfitz
ms.openlocfilehash: 1c6712eaf17cf55c1422baca355ce99ed319df28
ms.sourcegitcommit: a0be2dc237d30b7f79914e8adfb85299571374ec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/12/2018
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

## <a name="solution-1---syntax-error"></a>Oplossing 1 - syntaxisfout

Als u een foutbericht dat aangeeft de validatie van de sjabloon is mislukt ontvangt dat, hebt u mogelijk een probleem met de syntaxis in de sjabloon.

```
Code=InvalidTemplate
Message=Deployment template validation failed
```

Dit is een fout gemakkelijk maken omdat sjabloonexpressies kunnen ingewikkeld zijn. De naamtoewijzing van de volgende voor een opslagaccount heeft bijvoorbeeld een reeks vierkante haken, drie functies drie sets van haakjes, één set met enkele aanhalingstekens en één eigenschap:

```json
"name": "[concat('storage', uniqueString(resourceGroup().id))]",
```

Als u de syntaxis van de overeenkomende niet opgeeft, wordt in de sjabloon een waarde die is anders dan uw bedoeling produceert.

Wanneer u dit type fout ontvangt, zorgvuldig door de expressiesyntaxis van de. Overweeg het gebruik van een JSON-editor, zoals [Visual Studio](vs-azure-tools-resource-groups-deployment-projects-create-deploy.md) of [Visual Studio Code](resource-manager-vs-code.md), die u kunt gewaarschuwd over syntaxisfouten.

## <a name="solution-2---incorrect-segment-lengths"></a>Oplossing 2 - onjuist segment lengten

Een andere ongeldige sjabloon-fout treedt op wanneer de resourcenaam is niet de juiste indeling.

```
Code=InvalidTemplate
Message=Deployment template validation failed: 'The template resource {resource-name}'
for type {resource-type} has incorrect segment lengths.
```

Een bron voor het niveau van hoofdmap moet één minder segment hebben van de naam dan in het brontype. Elk segment wordt onderscheiden door een slash. In het volgende voorbeeld wordt het type heeft twee segmenten en de naam van de één segment zodat er een **geldige naam**.

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

## <a name="solution-3---parameter-is-not-valid"></a>Oplossing 3 - parameter is niet geldig

Als u een parameterwaarde die is niet een van de toegestane waarden opgeven, ontvangt u een bericht dat lijkt op de volgende fout:

```
Code=InvalidTemplate;
Message=Deployment template validation failed: 'The provided value {parameter value}
for the template parameter {parameter name} is not valid. The parameter value is not
part of the allowed values
```

Dubbele de toegestane waarden in de sjabloon en geef een tijdens de implementatie. Zie voor meer informatie over de toegestane parameterwaarden [sectie opdrachtregelparameters van Azure Resource Manager-sjablonen](resource-manager-templates-parameters.md).

## <a name="solution-4---too-many-target-resource-groups"></a>Oplossing 4: te veel doel resourcegroepen

Als u meer dan vijf doel resourcegroepen in een implementatie met één opgeeft, ontvangt deze foutmelding. Houd rekening met het aantal resourcegroepen in uw implementatie of implementeren van sommige sjablonen als afzonderlijke-implementaties. Zie voor meer informatie [implementeren Azure-resources aan meer dan één abonnement of resourcegroep](resource-manager-cross-resource-group-deployment.md).

## <a name="solution-5---circular-dependency-detected"></a>Oplossing 5 - circulaire afhankelijkheid gedetecteerd

U ontvangt deze foutmelding wanneer resources afhankelijk van elkaar op een manier waardoor de implementatie zijn niet worden gestart. Een combinatie van afhankelijkheden maakt twee of meer resources, wacht u totdat de andere bronnen die ook wachten. Bijvoorbeeld resource1 is afhankelijk van resource3, resource2, is afhankelijk van resource1 en resource3 is afhankelijk van resource2. Meestal kunt u dit probleem oplossen door het verwijderen van onnodige afhankelijkheden.

Voor het oplossen van een circulaire afhankelijkheid:

1. In de sjabloon de bron die is geïdentificeerd in de circulaire afhankelijkheid te vinden. 
2. Voor die bron onderzoekt de **dependsOn** eigenschap en het gebruik van de **verwijzing** om te zien welke resources afhankelijk van is de functie. 
3. Controleer deze bronnen om te zien welke bronnen ze afhankelijk zijn. Volg de afhankelijkheden totdat er een resource die afhankelijk van de oorspronkelijke bron is.
5. Voor de resources die zijn betrokken bij de circulaire afhankelijkheid zorgvuldig al gebruik van de **dependsOn** eigenschap voor het identificeren van eventuele afhankelijkheden die niet nodig zijn. Verwijder deze afhankelijkheden. Als u niet zeker weet dat er een afhankelijkheid is vereist, probeert u het verwijdert. 
6. De sjabloon opnieuw implementeert.

Verwijderen van de waarden uit de **dependsOn** eigenschap kan fouten veroorzaken wanneer u de sjabloon implementeert. Als u een fout optreedt, wordt de afhankelijkheid terug naar de sjabloon toevoegen. 

Als deze aanpak circulaire afhankelijkheid niet is opgelost, kunt u het onderdeel van uw implementatie logica verplaatsen naar onderliggende resources (zoals extensies of configuratie-instellingen). Deze onderliggende resources implementeren na de resources die zijn betrokken bij de circulaire afhankelijkheid configureren. Stel bijvoorbeeld dat u twee virtuele machines implementeert, maar u moet eigenschappen instellen voor elk criterium die verwijzen naar de andere. U kunt deze implementeren in de volgende volgorde:

1. vm1
2. vm2
3. Extensie op vm1, is afhankelijk van vm1 en vm2. De extensie waarden ingesteld op vm1 die van vm2 krijgt.
4. Extensie op vm2, is afhankelijk van vm1 en vm2. De extensie waarden ingesteld op vm2 van van vm1 krijgt.

Dezelfde manier werkt voor App Service-apps. Overweeg configuratiewaarden verplaatsen naar een onderliggende resource van resource voor de app. U kunt twee web-apps in de volgende volgorde kunt implementeren:

1. webapp1
2. webapp2
3. configuratie voor webapp1, is afhankelijk van webapp1 en webapp2. Het bevat app-instellingen met waarden uit webapp2.
4. configuratie voor webapp2, is afhankelijk van webapp1 en webapp2. Het bevat app-instellingen met waarden uit webapp1.
