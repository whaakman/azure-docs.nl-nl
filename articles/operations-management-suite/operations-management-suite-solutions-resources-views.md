---
title: Weergaven beheersystemen Operations Management Suite (OMS) | Microsoft Docs
description: 'Oplossingen in de Operations Management Suite (OMS) wordt gewoonlijk een of meer weergaven om gegevens bevatten.  In dit artikel wordt beschreven hoe een weergave die is gemaakt door de ontwerper exporteren en deze opnemen in een oplossing. '
services: operations-management-suite
documentationcenter: 
author: bwren
manager: jwhit
editor: tysonn
ms.assetid: 570b278c-2d47-4e5a-9828-7f01f31ddf8c
ms.service: operations-management-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/16/2018
ms.author: bwren
ms.openlocfilehash: c103ee748446c4819b7925af04d90c22225a21a3
ms.sourcegitcommit: 7edfa9fbed0f9e274209cec6456bf4a689a4c1a6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/17/2018
---
# <a name="views-in-operations-management-suite-oms-management-solutions-preview"></a>Weergaven in de Operations Management Suite (OMS) oplossingen (Preview)
> [!NOTE]
> Dit is voorlopige documentatie voor het maken van oplossingen voor het beheer in OMS die zich momenteel in preview. De hieronder beschreven schema kan worden gewijzigd.    
>
>

[Oplossingen voor het beheer in de Operations Management Suite (OMS)](operations-management-suite-solutions.md) omvatten een of meer weergaven om gegevens te visualiseren.  Dit artikel wordt beschreven hoe u een weergave die is gemaakt door exporteert de [ontwerper](../log-analytics/log-analytics-view-designer.md) en deze opnemen in een oplossing.  

> [!NOTE]
> De voorbeelden in dit artikel gebruiken parameters en variabelen die zijn vereist of gemeenschappelijke voor beheeroplossingen en wordt beschreven in [beheeroplossingen maken in Operations Management Suite (OMS)](operations-management-suite-solutions-creating.md)
>
>

## <a name="prerequisites"></a>Vereisten
In dit artikel wordt ervan uitgegaan dat u al bekend met het bent [maken van een beheeroplossing](operations-management-suite-solutions-creating.md) en de structuur van een oplossingsbestand.

## <a name="overview"></a>Overzicht
Als u wilt opnemen een weergave in een oplossing voor, die u maakt een **resource** voor in de [oplossingsbestand](operations-management-suite-solutions-creating.md).  De JSON die de gedetailleerde configuratie van de weergave beschrijft is doorgaans complexe echter en niet iets dat de auteur van een standaardoplossing zou kunnen handmatig maken.  De meest voorkomende methode is het maken van de weergave met behulp van de [ontwerper](../log-analytics/log-analytics-view-designer.md)exporteren en vervolgens de gedetailleerde configuratie toe te voegen aan de oplossing.

De eenvoudige stappen een weergave toevoegen aan een oplossing zijn als volgt.  Elke stap wordt beschreven in de secties hieronder nader.

1. De weergave naar een bestand exporteren.
2. De weergave-resource maken in de oplossing.
3. De weergavedetails toevoegen.

## <a name="export-the-view-to-a-file"></a>De weergave exporteren naar een bestand
Volg de instructies voor [Log Analytics-ontwerper](../log-analytics/log-analytics-view-designer.md) een weergave exporteren naar een bestand.  Het geëxporteerde bestand worden weergegeven in de JSON-indeling met dezelfde [elementen als het oplossingsbestand](operations-management-suite-solutions-solution-file.md).  

De **resources** element van het weergavebestand heeft een resource met een type **Microsoft.OperationalInsights/workspaces** die staat voor de OMS-werkruimte.  Dit element heeft een subelement van het type **weergaven** die staat voor de weergave en de gedetailleerde configuratie bevat.  U de details van dit element kopiëren en kopieert u deze naar uw oplossing.

## <a name="create-the-view-resource-in-the-solution"></a>De weergave-resource maken in de oplossing
Voeg de volgende weergave resource toe aan de **resources** element van het oplossingsbestand van uw.  Dit maakt gebruik van variabelen die hieronder worden beschreven die u ook moet toevoegen.  Houd er rekening mee dat de **Dashboard** en **OverviewTile** eigenschappen zijn tijdelijke aanduidingen die door de corresponderende eigenschappen van het geëxporteerde weergave-bestand wordt overschreven.

    {
        "apiVersion": "[variables('LogAnalyticsApiVersion')]",
        "name": "[concat(parameters('workspaceName'), '/', variables('ViewName'))]",
        "type": "Microsoft.OperationalInsights/workspaces/views",
        "location": "[parameters('workspaceregionId')]",
        "id": "[Concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', resourceGroup().name, '/providers/Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'),'/views/', variables('ViewName'))]",
        "dependson": [
            ],
        "properties": {
            "Id": "[variables('ViewName')]",
            "Name": "[variables('ViewName')]",
            "DisplayName": "[variables('ViewName')]",
            "Description": "",
            "Author": "[variables('ViewAuthor')]",
            "Source": "Local",
            "Dashboard": ,
            "OverviewTile":
        }
    }

De volgende variabelen toevoegen aan het element variabelen van het oplossingsbestand en vervang de waarden die voor uw oplossing.

    "LogAnalyticsApiVersion": "<api-version>",
    "ViewAuthor": "Your name."
    "ViewDescription": "Optional description of the view."
    "ViewName": "Provide a name for the view here."

Houd er rekening mee dat u de hele weergave resource vanuit het bestand met geëxporteerde weergave kopiëren kan, maar u moet de volgende wijzigingen te werken in uw oplossing.  

* De **type** voor de weergave resource moet worden gewijzigd van **weergaven** naar **Microsoft.OperationalInsights/workspaces**.
* De **naam** eigenschap voor de weergave-bron moet worden gewijzigd en bevat de naam van de werkruimte.
* De afhankelijkheid van de werkruimte moet worden verwijderd omdat de bron van de werkruimte niet is gedefinieerd in de oplossing.
* **DisplayName** eigenschap moet worden toegevoegd aan de weergave.  De **Id**, **naam**, en **DisplayName** moeten alle overeenkomen.
* Namen van parameters moeten worden gewijzigd zodat deze overeenkomt met het vereiste aantal parameters.
* Variabelen moeten worden gedefinieerd in de oplossing en gebruikt in de toepasselijke eigenschappen.

### <a name="log-analytics-api-version"></a>Log Analytics-API-versie
Alle logboekanalyse resources die zijn gedefinieerd in het Resource Manager-sjabloon hebben een eigenschap **apiVersion** waarmee wordt gedefinieerd met de versie van de API van de bron moet gebruiken.  Deze versie is verschillend voor weergaven met query's die gebruikmaken van de [legacy en de bijgewerkte querytaal](../log-analytics/log-analytics-log-search-upgrade.md).  

 De volgende tabel geeft de Log Analytics-API-versies voor weergaven in eerdere versies en bijgewerkte werkruimten: 

| Werkruimteversie | API-versie | Query’s uitvoeren |
|:---|:---|:---|
| V1 (verouderd)   | 2015-11-01-preview | Verouderde indeling.<br> Bijvoorbeeld: Typ = gebeurtenis EventLevelName fout =  |
| v2 (upgrade) | 2015-11-01-preview | Verouderde indeling.  Geconverteerd naar bijgewerkte indeling op installeren.<br> Bijvoorbeeld: Typ = gebeurtenis EventLevelName fout =<br>Geconverteerd naar: gebeurtenis &#124; waar EventLevelName == "Error"  |
| v2 (upgrade) | 2017-03-03-preview | De indeling van de upgrade. <br>Voorbeeld: De gebeurtenis &#124; waar EventLevelName == "Error"  |


## <a name="add-the-view-details"></a>De weergavedetails toevoegen
De resource weergeven in het bestand met geëxporteerde weergave bevat twee elementen in de **eigenschappen** element met de naam **Dashboard** en **OverviewTile** die de gedetailleerde bevatten configuratie van de weergave.  Kopieer deze twee elementen en de inhoud in de **eigenschappen** element van de resource weergeven in uw oplossingsbestand.

## <a name="example"></a>Voorbeeld
Het volgende voorbeeld ziet u bijvoorbeeld een eenvoudige oplossing-bestand met een weergave.  Weglatingstekens (...) worden weergegeven voor de **Dashboard** en **OverviewTile** inhoud omwille van de ruimte.

    {
        "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "workspaceName": {
                "type": "string"
            },
            "accountName": {
                "type": "string"
            },
            "workspaceRegionId": {
                "type": "string"
            },
            "regionId": {
                "type": "string"
            },
            "pricingTier": {
                "type": "string"
            }
        },
        "variables": {
            "SolutionVersion": "1.1",
            "SolutionPublisher": "Contoso",
            "SolutionName": "Contoso Solution",
            "LogAnalyticsApiVersion": "2015-11-01-preview",
            "ViewAuthor":  "user@contoso.com",
            "ViewDescription":  "This is a sample view.",
            "ViewName":  "Contoso View"
        },
        "resources": [
            {
                "name": "[concat(variables('SolutionName'), '(' ,parameters('workspacename'), ')')]",
                "location": "[parameters('workspaceRegionId')]",
                "tags": { },
                "type": "Microsoft.OperationsManagement/solutions",
                "apiVersion": "[variables('LogAnalyticsApiVersion')]",
                "dependsOn": [
                    "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspacename'), '/views/', variables('ViewName'))]"
                ],
                "properties": {
                    "workspaceResourceId": "[concat(resourceGroup().id, '/providers/Microsoft.OperationalInsights/workspaces/', parameters('workspacename'))]",
                    "referencedResources": [
                    ],
                    "containedResources": [
                        "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'), '/views/', variables('ViewName'))]"
                    ]
                },
                "plan": {
                    "name": "[concat(variables('SolutionName'), '(' ,parameters('workspaceName'), ')')]",
                    "Version": "[variables('SolutionVersion')]",
                    "product": "ContosoSolution",
                    "publisher": "[variables('SolutionPublisher')]",
                    "promotionCode": ""
                }
            },
            {
                "apiVersion": "[variables('LogAnalyticsApiVersion')]",
                "name": "[concat(parameters('workspaceName'), '/', variables('ViewName'))]",
                "type": "Microsoft.OperationalInsights/workspaces/views",
                "location": "[parameters('workspaceregionId')]",
                "id": "[Concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', resourceGroup().name, '/providers/Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'),'/views/', variables('ViewName'))]",
                "dependson": [
                ],
                "properties": {
                    "Id": "[variables('ViewName')]",
                    "Name": "[variables('ViewName')]",
                    "DisplayName": "[variables('ViewName')]",
                    "Description": "[variables('ViewDescription')]",
                    "Author": "[variables('ViewAuthor')]",
                    "Source": "Local",
                    "Dashboard": ...,
                    "OverviewTile": ...
                }
            }
          ]
    }




## <a name="next-steps"></a>Volgende stappen
* Meer informatie over alle details van het maken van [beheeroplossingen](operations-management-suite-solutions-creating.md).
* Omvatten [Automation-runbooks in uw beheeroplossing](operations-management-suite-solutions-resources-automation.md).
