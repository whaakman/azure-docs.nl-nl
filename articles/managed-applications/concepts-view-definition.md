---
title: Overzicht van de weergavedefinitie in Azure Managed Applications | Microsoft Docs
description: Beschrijving van het concept van het maken van de weergavedefinitie voor Azure beheerde toepassingen.
services: managed-applications
ms.service: managed-applications
ms.topic: conceptual
ms.author: lazinnat
author: lazinnat
ms.date: 06/12/2019
ms.openlocfilehash: 6735787f9b43f98ab611584f3c7191c9f927dbc2
ms.sourcegitcommit: f811238c0d732deb1f0892fe7a20a26c993bc4fc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/29/2019
ms.locfileid: "67478743"
---
# <a name="view-definition-artifact-in-azure-managed-applications"></a>Weergave definitie artefact in Azure beheerde toepassingen

De weergavedefinitie van de is een optionele artefact in Azure beheerde toepassingen. Hierdoor kan meer weergaven, zoals metrische gegevens en aangepaste resources toevoegen en aanpassen van de overzichtspagina.

Dit artikel bevat een overzicht van definitie-artefact weergeven en de mogelijkheden ervan.

## <a name="view-definition-artifact"></a>Definitie-artefact weergeven

De weergave definitie artefact moet de naam **viewDefinition.json** en geplaatst op hetzelfde niveau als **createUiDefinition.json** en **mainTemplate.json** in de .zip het pakket dat wordt gemaakt van de definitie van een beheerde toepassing. Zie voor meer informatie over het maken van het ZIP-pakket en de definitie van een beheerde toepassing publiceren, [de definitie van een Azure beheerde toepassing publiceren](publish-managed-app-definition-quickstart.md)

## <a name="view-definition-schema"></a>Schema van de definitie weergeven

De **viewDefinition.json** bestand heeft slechts één hoogste niveau `views` eigenschap, dit een matrix van weergaven is. Elke weergave wordt weergegeven in de gebruikersinterface van de beheerde toepassing als een afzonderlijke menu-item in de tabel van inhoud. Elke weergave bevat een `kind` eigenschap die Hiermee stelt u het type van de weergave. Het moet worden ingesteld op een van de volgende waarden: [Overzicht](#overview), [metrische gegevens](#metrics), [CustomResources](#custom-resources). Zie voor meer informatie, huidige [JSON-schema voor viewDefinition.json](https://schema.management.azure.com/schemas/viewdefinition/0.0.1-preview/ViewDefinition.json#).

Voorbeeld-JSON voor de weergavedefinitie van de:

```json
{
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
    ]
}

```

## <a name="overview"></a>Overzicht

`"kind": "Overview"`

Wanneer u voorzien in deze weergave in **viewDefinition.json**, heeft deze voorrang op de pagina overzicht van standaard in uw beheerde toepassing.

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
|koptekst|Nee|De koptekst van de overzichtspagina.|
|description|Nee|De beschrijving van uw beheerde toepassing.|
|Opdrachten|Nee|Zie de reeks aanvullende knoppen aan de overzichtspagina [opdrachten](#commands).|

## <a name="metrics"></a>Metrische gegevens

`"kind": "Metrics"`

De weergave voor metrische gegevens kunt verzamelen en samenvoegen van gegevens van de resources van uw beheerde toepassing in [Azure Monitor Metrics](../azure-monitor/platform/data-platform-metrics.md).

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
|displayName|Nee|De weergegeven naam van de weergave.|
|version|Nee|De versie van het platform dat wordt gebruikt om de weergave.|
|Grafieken|Ja|De matrix met grafieken van de pagina metrische gegevens.|

### <a name="chart"></a>Grafiek

|Eigenschap|Vereist|Description|
|---------|---------|---------|
|displayName|Ja|De weergegeven naam van de grafiek.|
|chartType|Nee|De visualisatie moet worden gebruikt voor deze grafiek. Standaard maakt het gebruik van een lijndiagram. Grafiektypen ondersteund: `Bar, Line, Area, Scatter`.|
|metrics|Ja|De matrix van metrische gegevens moeten worden getekend in deze grafiek. Zie voor meer informatie over metrische gegevens die worden ondersteund in Azure portal, [ondersteunde metrische gegevens met Azure Monitor](../azure-monitor/platform/metrics-supported.md)|

### <a name="metric"></a>Gegevens

|Eigenschap|Vereist|Description|
|---------|---------|---------|
|name|Ja|De naam van de metrische gegevens.|
|aggregationType|Ja|Het aggregatietype moet worden gebruikt voor deze metrische gegevens. Van de ondersteunde aggregatietypen: `none, sum, min, max, avg, unique, percentile, count`|
|naamruimte|Nee|Als u meer informatie over het gebruik bij het bepalen van de provider van de juiste metrische gegevens.|
|resourceTagFilter|Nee|De resource tags matrix (worden gescheiden met `or` word) voor welke metrische gegevens worden weergegeven. Is van toepassing op type resourcefilter.|
|ResourceType|Ja|Het brontype waarvoor metrische gegevens worden weergegeven.|

## <a name="custom-resources"></a>Aangepaste resources

`"kind": "CustomResources"`

U kunt meerdere weergaven van dit type definiëren. Elke weergave vertegenwoordigt een **unieke** aangepaste resource typen van de aangepaste provider die u hebt gedefinieerd in **mainTemplate.json**. Zie voor een inleiding tot aangepaste providers, [Azure aangepaste Providers Preview overzicht](custom-providers-overview.md).

In deze weergave die kunt u uitvoeren GET, PUT, verwijderen en te plaatsen van bewerkingen voor uw aangepaste brontype. POST-bewerkingen kunnen worden algemene aangepaste acties of aangepaste acties in de context van uw aangepaste brontype.

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
|displayName|Ja|De weergegeven naam van de weergave. De titel moet **unieke** voor elke weergave CustomResources in uw **viewDefinition.json**.|
|version|Nee|De versie van het platform dat wordt gebruikt om de weergave.|
|ResourceType|Ja|Het aangepaste brontype. Moet een **unieke** aangepaste resourcetype van uw aangepaste provider.|
|createUIDefinition|Nee|UI-definitie maken-schema voor de aangepaste resource-opdracht maken. Zie voor een inleiding tot het maken van definities van de gebruikersinterface, [aan de slag met CreateUiDefinition](create-uidefinition-overview.md)|
|Opdrachten|Nee|Zie de reeks aanvullende werkbalkknoppen van de weergave CustomResources [opdrachten](#commands).|
|Kolommen|Nee|De matrix van de kolommen van de aangepaste resource. Als niet is gedefinieerd de `name` kolom wordt standaard weergegeven. De kolom moet hebben `"key"` en `"displayName"`. Voor de sleutel, moet u de sleutel van de eigenschap om weer te geven in een weergave opgeven. Als genest, gebruik een punt als scheidingsteken, bijvoorbeeld `"key": "name"` of `"key": "properties.property1"`. Voor de weergavenaam, geeft u de weergavenaam van de eigenschap om weer te geven in een weergave. U kunt ook opgeven een `"optional"` eigenschap. Wanneer is ingesteld op true, wordt de kolom is verborgen in een standaard.|

## <a name="commands"></a>Opdrachten

Opdrachten is een matrix van aanvullende werkbalkknoppen die worden weergegeven op de pagina. Elke opdracht vertegenwoordigt een actie na de van uw Azure-aangepaste Provider gedefinieerd in **mainTemplate.json**. Zie voor een inleiding tot aangepaste providers, [Azure aangepaste Providers overzicht](custom-providers-overview.md).

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
|displayName|Ja|De weergegeven naam van de knop.|
|pad|Ja|De naam van de aangepaste provider-actie. De actie moet worden gedefinieerd in **mainTemplate.json**.|
|Pictogram|Nee|Het pictogram van de knop. Lijst met ondersteunde pictogrammen is gedefinieerd in [JSON-Schema](https://schema.management.azure.com/schemas/viewdefinition/0.0.1-preview/ViewDefinition.json#).|
|createUIDefinition|Nee|UI-definitie-schema voor opdracht maken. Zie voor een inleiding tot het maken van definities van de gebruikersinterface, [aan de slag met CreateUiDefinition](create-uidefinition-overview.md).|

## <a name="next-steps"></a>Volgende stappen

- Zie [Overzicht van door Azure beheerde toepassingen](overview.md) voor algemene informatie over beheerde toepassingen.
- Zie voor een inleiding tot aangepaste providers, [Azure aangepaste Providers overzicht](custom-providers-overview.md).
