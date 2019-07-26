---
title: Overzicht van de definitie van de weer gave in Azure Managed Applications | Microsoft Docs
description: Hierin wordt het concept van het maken van de weergave definitie voor Azure Managed Applications beschreven.
services: managed-applications
ms.service: managed-applications
ms.topic: conceptual
ms.author: lazinnat
author: lazinnat
ms.date: 06/12/2019
ms.openlocfilehash: ff96bddef1b34f5a8bf743ccaaccba2da01534dc
ms.sourcegitcommit: e9c866e9dad4588f3a361ca6e2888aeef208fc35
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/19/2019
ms.locfileid: "68335066"
---
# <a name="view-definition-artifact-in-azure-managed-applications"></a>Definitie artefact in Azure Managed Applications weer geven

Weergave definitie is een optioneel artefact in Azure Managed Applications. Hiermee kunt u de overzichts pagina aanpassen en meer weer gaven toevoegen, zoals metrische gegevens en aangepaste resources.

Dit artikel bevat een overzicht van het weer geven van definitie artefacten en de mogelijkheden ervan.

## <a name="view-definition-artifact"></a>Definitie-artefact weergeven

De weergave definitie artefact moet de naam **viewDefinition. json** hebben en op hetzelfde niveau worden geplaatst als **createUiDefinition. json** en **mainTemplate. json** in het zip-pakket dat een definitie van een beheerde toepassing maakt. Zie [een definitie van een door Azure beheerde toepassing publiceren](publish-managed-app-definition-quickstart.md) voor meer informatie over het maken van een zip-pakket en het publiceren van een definitie van een beheerde toepassing.

## <a name="view-definition-schema"></a>Definitie schema weer geven

Het bestand **viewDefinition. json** heeft slechts één eigenschap van `views` het hoogste niveau. Dit is een matrix met weer gaven. Elke weer gave wordt weer gegeven in de gebruikers interface van de beheerde toepassing als een afzonderlijke menu opdracht in de inhouds opgave. Elke weer gave heeft `kind` een eigenschap waarmee het type weer gave wordt ingesteld. Deze moet worden ingesteld op een van de volgende waarden: [Overzicht](#overview), [metrische gegevens](#metrics), [CustomResources](#custom-resources). Zie het huidige [JSON-schema voor viewDefinition. json](https://schema.management.azure.com/schemas/viewdefinition/0.0.1-preview/ViewDefinition.json#)voor meer informatie.

Voor beeld van JSON voor weergave definitie:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/viewdefinition/0.0.1-preview/ViewDefinition.json#",
    "contentVersion": "0.0.0.1",
    "views": [
        {
            "kind": "Overview",
            "properties": {
                "header": "Welcome to your Azure Managed Application",
                "description": "This managed application is for demo purposes only.",
                "commands": [
                    {
                        "displayName": "Test Action",
                        "path": "testAction"
                    }
                ]
            }
        },
        {
            "kind": "Metrics",
            "properties": {
                "displayName": "This is my metrics view",
                "version": "1.0.0",
                "charts": [
                    {
                        "displayName": "Sample chart",
                        "chartType": "Bar",
                        "metrics": [
                            {
                                "name": "Availability",
                                "aggregationType": "avg",
                                "resourceTagFilter": [ "tag1" ],
                                "resourceType": "Microsoft.Storage/storageAccounts",
                                "namespace": "Microsoft.Storage/storageAccounts"
                            }
                        ]
                    }
                ]
            }
        },
        {
            "kind": "CustomResources",
            "properties": {
                "displayName": "Test custom resource type",
                "version": "1.0.0",
                "resourceType": "testCustomResource",
                "createUIDefinition": { },
                "commands": [
                    {
                        "displayName": "Custom Test Action",
                        "path": "testAction"
                    },
                    {
                        "displayName": "Custom Context Action",
                        "path": "testCustomResource/testContextAction",
                        "icon": "Stop",
                        "createUIDefinition": { }
                    }
                ],
                "columns": [
                    {"key": "name", "displayName": "Name"},
                    {"key": "properties.myProperty1", "displayName": "Property 1"},
                    {"key": "properties.myProperty2", "displayName": "Property 2", "optional": true}
                ]
            }
        }
    ]
}

```

## <a name="overview"></a>Overzicht

`"kind": "Overview"`

Wanneer u deze weer gave in **viewDefinition. json**opgeeft, wordt de standaard overzichts pagina in uw beheerde toepassing overschreven.

```json
{
    "kind": "Overview",
    "properties": {
        "header": "Welcome to your Azure Managed Application",
        "description": "This managed application is for demo purposes only.",
        "commands": [
            {
                "displayName": "Test Action",
                "path": "testAction"
            }
        ]
    }
}
```

|Eigenschap|Vereist|Description|
|---------|---------|---------|
|koptekst|Nee|De koptekst van de pagina overzicht.|
|description|Nee|De beschrijving van de beheerde toepassing.|
|opdrachten|Nee|De matrix met aanvullende werkbalk knoppen van de overzichts pagina, Zie [opdrachten](#commands).|

![Overzicht](./media/view-definition/overview.png)

## <a name="metrics"></a>Metrische gegevens

`"kind": "Metrics"`

Met de weer gave metrieken kunt u gegevens verzamelen en samen voegen uit uw beheerde toepassings resources in [Azure monitor metrieken](../azure-monitor/platform/data-platform-metrics.md).

```json
{
    "kind": "Metrics",
    "properties": {
        "displayName": "This is my metrics view",
        "version": "1.0.0",
        "charts": [
            {
                "displayName": "Sample chart",
                "chartType": "Bar",
                "metrics": [
                    {
                        "name": "Availability",
                        "aggregationType": "avg",
                        "resourceTagFilter": [ "tag1" ],
                        "resourceType": "Microsoft.Storage/storageAccounts",
                        "namespace": "Microsoft.Storage/storageAccounts"
                    }
                ]
            }
        ]
    }
}
```

|Eigenschap|Vereist|Description|
|---------|---------|---------|
|displayName|Nee|De weer gegeven titel van de weer gave.|
|version|Nee|De versie van het platform waarmee de weer gave wordt weer gegeven.|
|diagrammen|Ja|De matrix van grafieken van de pagina metrische gegevens.|

### <a name="chart"></a>Grafiek

|Eigenschap|Vereist|Description|
|---------|---------|---------|
|displayName|Ja|De weer gegeven titel van de grafiek.|
|chartType|Nee|De visualisatie die moet worden gebruikt voor deze grafiek. Standaard wordt een lijn diagram gebruikt. Ondersteunde grafiek typen: `Bar, Line, Area, Scatter`.|
|metrische gegevens|Ja|De matrix met metrische gegevens die in deze grafiek moeten worden getekend. Zie [ondersteunde metrische gegevens met Azure monitor](../azure-monitor/platform/metrics-supported.md) voor meer informatie over metrische gegevens die worden ondersteund in azure Portal.|

### <a name="metric"></a>Gegevens

|Eigenschap|Vereist|Description|
|---------|---------|---------|
|name|Ja|De naam van de metriek.|
|aggregationType|Ja|Het aggregatie type dat moet worden gebruikt voor deze metriek. Ondersteunde aggregatie typen:`none, sum, min, max, avg, unique, percentile, count`|
|naamruimte|Nee|Aanvullende informatie die moet worden gebruikt bij het bepalen van de juiste metrics-provider.|
|resourceTagFilter|Nee|De matrix van de bron Tags (wordt gescheiden `or` met Word) waarvoor metrische gegevens worden weer gegeven. Is boven op het resource type filter.|
|resourceType|Ja|Het resource type waarvoor metrische gegevens worden weer gegeven.|

![Metrische gegevens](./media/view-definition/metrics.png)

## <a name="custom-resources"></a>Aangepaste resources

`"kind": "CustomResources"`

U kunt meerdere weer gaven van dit type definiëren. Elke weer gave vertegenwoordigt een **uniek** aangepast resource type van de aangepaste provider die u hebt gedefinieerd in **mainTemplate. json**. Zie [Preview-versie van Azure Custom providers](custom-providers-overview.md)voor een inleiding tot aangepaste providers.

In deze weer gave kunt u GET-, PUT-, DELETE-en POST-bewerkingen uitvoeren voor uw aangepaste resource type. POST-bewerkingen kunnen globale aangepaste acties of aangepaste acties zijn in een context van uw aangepaste resource type.

```json
{
    "kind": "CustomResources",
    "properties": {
        "displayName": "Test custom resource type",
        "version": "1.0.0",
        "resourceType": "testCustomResource",
        "createUIDefinition": { },
        "commands": [
            {
                "displayName": "Custom Test Action",
                "path": "testAction"
            },
            {
                "displayName": "Custom Context Action",
                "path": "testCustomResource/testContextAction",
                "icon": "Stop",
                "createUIDefinition": { },
            }
        ],
        "columns": [
            {"key": "name", "displayName": "Name"},
            {"key": "properties.myProperty1", "displayName": "Property 1"},
            {"key": "properties.myProperty2", "displayName": "Property 2", "optional": true}
        ]
    }
}
```

|Eigenschap|Vereist|Description|
|---------|---------|---------|
|displayName|Ja|De weer gegeven titel van de weer gave. De titel moet **uniek** zijn voor elke CustomResources-weer gave in uw **viewDefinition. json**.|
|version|Nee|De versie van het platform waarmee de weer gave wordt weer gegeven.|
|resourceType|Ja|Het aangepaste resource type. Moet een **uniek** aangepast resource type van uw aangepaste provider zijn.|
|createUIDefinition|Nee|Maak een UI-definitie schema voor het maken van een aangepaste resource opdracht. Zie aan de slag [met CreateUiDefinition](create-uidefinition-overview.md) voor een inleiding tot het maken van UI-definities.|
|opdrachten|Nee|De matrix met aanvullende werkbalk knoppen van de weer gave CustomResources Zie [Commands](#commands).|
|Kolommen|Nee|De matrix van kolommen van de aangepaste resource. Indien niet gedefinieerd, `name` wordt de kolom standaard weer gegeven. De kolom moet en `"key"` `"displayName"`bevatten. Geef bij sleutel de sleutel van de eigenschap op die in een weer gave moet worden weer gegeven. Als genest, gebruikt u punt als scheidings teken `"key": "name"` , `"key": "properties.property1"`bijvoorbeeld of. Geef bij weergave naam de weergave naam op van de eigenschap die in een weer gave moet worden weer gegeven. U kunt ook een `"optional"` eigenschap opgeven. Als deze eigenschap is ingesteld op True, wordt de kolom standaard verborgen in een weer gave.|

![CustomResources](./media/view-definition/customresources.png)

## <a name="commands"></a>Opdrachten

Opdrachten is een matrix met aanvullende werkbalk knoppen die op de pagina worden weer gegeven. Elke opdracht vertegenwoordigt een bericht actie van uw aangepaste Azure-provider die is gedefinieerd in **mainTemplate. json**. Zie [overzicht van aangepaste Azure-providers](custom-providers-overview.md)voor een inleiding tot aangepaste providers.

```json
{
    "commands": [
        {
            "displayName": "Start Test Action",
            "path": "testAction",
            "icon": "Start",
            "createUIDefinition": { }
        },
    ]
}
```

|Eigenschap|Vereist|Description|
|---------|---------|---------|
|displayName|Ja|De weer gegeven naam van de opdracht knop.|
|path|Ja|De actie naam van de aangepaste provider. De actie moet worden gedefinieerd in **mainTemplate. json**.|
|Pictogram|Nee|Het pictogram van de opdracht knop. Een lijst met ondersteunde pictogrammen wordt gedefinieerd in het [JSON-schema](https://schema.management.azure.com/schemas/viewdefinition/0.0.1-preview/ViewDefinition.json#).|
|createUIDefinition|Nee|Het definitie schema voor de gebruikers interface maken voor de opdracht. Zie aan de slag [met CreateUiDefinition](create-uidefinition-overview.md)voor een inleiding tot het maken van UI-definities.|

## <a name="looking-for-help"></a>Zoeken naar Help

Als u vragen hebt over Azure Managed Applications, kunt u vragen om [stack overflow](http://stackoverflow.com/questions/tagged/azure-managedapps). Er is mogelijk al een vergelijk bare vraag gesteld en beantwoord, dus controleer eerst vóór het boeken. Voeg de tag `azure-managedapps` toe om een snel antwoord te krijgen.

## <a name="next-steps"></a>Volgende stappen

- Zie [Overzicht van door Azure beheerde toepassingen](overview.md) voor algemene informatie over beheerde toepassingen.
- Zie [overzicht van aangepaste Azure-providers](custom-providers-overview.md)voor een inleiding tot aangepaste providers.
- Zie [zelf studie voor het maken van een door Azure beheerde toepassing met aangepaste Azure-providers: Een beheerde toepassing maken met aangepaste provider acties en resource typen](tutorial-create-managed-app-with-custom-provider.md)
