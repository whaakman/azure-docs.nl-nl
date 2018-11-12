---
title: Weergaven in oplossingen voor | Microsoft Docs
description: 'Beheeroplossingen bevatten meestal een of meer weergaven om gegevens te visualiseren.  In dit artikel wordt beschreven hoe u een weergave die zijn gemaakt door de ontwerper exporteren en deze opnemen in een oplossing voor beheer. '
services: monitoring
documentationcenter: ''
author: bwren
manager: jwhit
editor: tysonn
ms.assetid: 570b278c-2d47-4e5a-9828-7f01f31ddf8c
ms.service: monitoring
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/16/2018
ms.author: bwren
ms.openlocfilehash: a0d543ebe435b616306690bfb5e3de63ecc1ff6c
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51259119"
---
# <a name="views-in-management-solutions-preview"></a>Weergaven in oplossingen voor beheer (Preview)
> [!NOTE]
> Dit is een voorlopige documentatie voor het maken van oplossingen die zich momenteel in preview. Er is geen schema die hieronder worden beschreven kan worden gewijzigd.    


[Beheeroplossingen](monitoring-solutions.md) bevatten meestal een of meer weergaven om gegevens te visualiseren.  In dit artikel wordt beschreven hoe u voor het exporteren van een weergave die zijn gemaakt door de [Weergaveontwerper](../log-analytics/log-analytics-view-designer.md) en deze opnemen in een oplossing voor beheer.  

> [!NOTE]
> De voorbeelden in dit artikel Gebruik parameters en variabelen die zijn vereist of gemeenschappelijke beheeroplossingen en wordt beschreven in [ontwerpen en bouwen van een oplossing in Azure](monitoring-solutions-creating.md)
>
>

## <a name="prerequisites"></a>Vereisten
In dit artikel wordt ervan uitgegaan dat u bent al bekend bent met het [maken van een oplossing voor](monitoring-solutions-creating.md) en de structuur van een oplossingsbestand.

## <a name="overview"></a>Overzicht
Als u wilt opnemen een weergave in een oplossing voor beheer, maakt u een **resource** voor in de [oplossingsbestand](monitoring-solutions-creating.md).  De JSON die wordt beschreven gedetailleerde configuratie van de weergave is doorgaans complexe echter en niet iets dat de auteur van een typische oplossing zou kunnen handmatig maken.  De meest voorkomende methode is het maken van de weergave met de [Weergaveontwerper](../log-analytics/log-analytics-view-designer.md), exporteren en vervolgens de gedetailleerde configuratie toevoegen aan de oplossing.

De eenvoudige stappen een weergave toevoegen aan een oplossing zijn als volgt.  Elke stap wordt nader besproken in de onderstaande secties beschreven.

1. De weergave exporteren naar een bestand.
2. Maak de weergave-resource in de oplossing.
3. De weergavedetails toevoegen.

## <a name="export-the-view-to-a-file"></a>De weergave exporteren naar een bestand
Volg de instructies op [Weergaveontwerper van Log Analytics](../log-analytics/log-analytics-view-designer.md) een weergave exporteren naar een bestand.  Het geëxporteerde bestand worden weergegeven in JSON-indeling met dezelfde [elementen als het oplossingsbestand](monitoring-solutions-solution-file.md).  

De **resources** element van het weergavebestand heeft een resource met een type **Microsoft.OperationalInsights/workspaces** die de Log Analytics-werkruimte representeert.  Dit element heeft een subelement van het type **weergaven** die staat voor de weergave en de gedetailleerde configuratie bevat.  U kopieert u de details van dit element en kopieer deze vervolgens in uw oplossing.

## <a name="create-the-view-resource-in-the-solution"></a>De resource weergeven in de oplossing maken
Toevoegen van de volgende weergave resource toe aan de **resources** element van het oplossingsbestand van uw.  Dit maakt gebruik van variabelen die hieronder worden beschreven die u ook moet toevoegen.  Houd er rekening mee dat de **Dashboard** en **OverviewTile** eigenschappen zijn tijdelijke aanduidingen die u met de bijbehorende eigenschappen van het bestand met geëxporteerde weergave wordt overschreven.

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

De volgende variabelen toevoegen aan het element variabelen van de oplossingsbestand en vervang de waarden die voor uw oplossing.

    "LogAnalyticsApiVersion": "<api-version>",
    "ViewAuthor": "Your name."
    "ViewDescription": "Optional description of the view."
    "ViewName": "Provide a name for the view here."

Houd er rekening mee dat u de volledige weergave-resource uit het bestand met geëxporteerde weergeven kopiëren kan, maar u de volgende wijzigingen voor het moet werken in uw oplossing.  

* De **type** voor de weergave resource moet worden gewijzigd van **weergaven** naar **Microsoft.OperationalInsights/workspaces**.
* De **naam** eigenschap voor de weergave-bron moet worden gewijzigd om op te nemen van de naam van de werkruimte.
* De afhankelijkheid van de werkruimte moet worden verwijderd omdat de werkruimteresource is niet gedefinieerd in de oplossing.
* **DisplayName** eigenschap moet worden toegevoegd aan de weergave.  De **Id**, **naam**, en **DisplayName** moet alle overeenkomen.
* Namen van parameters moeten worden gewijzigd zodat deze overeenkomt met de vereiste set parameters.
* Variabelen moeten worden gedefinieerd in de oplossing en gebruikt in de gewenste eigenschappen.

### <a name="log-analytics-api-version"></a>Log Analytics-API-versie
Alle Log Analytics-resources in een Resource Manager-sjabloon gedefinieerd hebben een eigenschap **apiVersion** die de versie van de API moet worden gebruikt door de resource definieert.  Deze versie is verschillend voor weergaven met query's die gebruikmaken van de [verouderde systemen en de bijgewerkte querytaal](../log-analytics/log-analytics-queries.md).  

 De volgende tabel worden de Log Analytics-API-versies voor weergaven in de verouderde en bijgewerkte werkruimten: 

| Werkruimteversie | API-versie | Query’s uitvoeren |
|:---|:---|:---|
| V1 (verouderde)   | 2015-11-01-preview | Oudere indeling.<br> Bijvoorbeeld: Typ = gebeurtenis EventLevelName fout =  |
| v2 (bijgewerkt) | 2015-11-01-preview | Oudere indeling.  Geconverteerd naar bijgewerkte notatie op installeren.<br> Bijvoorbeeld: Typ = gebeurtenis EventLevelName fout =<br>Geconverteerd naar: gebeurtenis &#124; waar EventLevelName == "Error"  |
| v2 (bijgewerkt) | 2017-03-03-preview | De indeling van de upgrade. <br>Voorbeeld: Event &#124; waar EventLevelName == "Error"  |


## <a name="add-the-view-details"></a>De weergavedetails toevoegen
De resource weergeven in het bestand met geëxporteerde weergave bevat twee elementen in de **eigenschappen** element met de naam **Dashboard** en **OverviewTile** waarin de gedetailleerde configuratie van de weergave.  Kopieer deze twee elementen en de inhoud in de **eigenschappen** element van de resource weergeven in het oplossingsbestand van uw.

## <a name="example"></a>Voorbeeld
Het volgende voorbeeld ziet u bijvoorbeeld een bestand eenvoudige oplossing met een weergave.  Weglatingstekens (...) worden weergegeven voor de **Dashboard** en **OverviewTile** inhoud omwille van de ruimte.

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
* Meer informatie voor het maken van meer [beheeroplossingen](monitoring-solutions-creating.md).
* Opnemen [Automation-runbooks in het beheersysteem](monitoring-solutions-resources-automation.md).
