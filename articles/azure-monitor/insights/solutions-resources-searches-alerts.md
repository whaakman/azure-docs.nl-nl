---
title: Opgeslagen zoekopdrachten in oplossingen voor | Microsoft Docs
description: Beheeroplossingen bevatten meestal opgeslagen zoekopdrachten in Log Analytics voor het analyseren van gegevens die zijn verzameld door de oplossing. In dit artikel wordt beschreven hoe u Log Analytics opgeslagen zoekopdrachten in Resource Manager-sjabloon, zodat ze kunnen worden opgenomen in oplossingen voor het beheer te definiëren.
services: monitoring
documentationcenter: ''
author: bwren
manager: carmonm
editor: tysonn
ms.service: monitoring
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/27/2019
ms.author: bwren
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 64b79d613463ac2097a89a8e3ca3870b885a3332
ms.sourcegitcommit: 1afd2e835dd507259cf7bb798b1b130adbb21840
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/28/2019
ms.locfileid: "56985254"
---
# <a name="adding-log-analytics-saved-searches-to-management-solution-preview"></a>Log Analytics toevoegen opgeslagen zoekopdrachten naar management-oplossing (Preview)

> [!IMPORTANT]
> Een eerdere versie van dit artikel bevat informatie over het maken van een waarschuwing met behulp van Resource Manager-sjabloon. Deze informatie is van de datum nu die [Log Analytics-waarschuwingen zijn uitgebreid naar Azure Monitor](../platform/alerts-extend.md). Zie voor meer informatie over het maken van een waarschuwing met Resource Manager-sjabloon [waarschuwingen beheren met behulp van Azure-Resourcesjabloon](../platform/alerts-log.md#managing-log-alerts-using-azure-resource-template).

> [!NOTE]
> Dit is een voorlopige documentatie voor het maken van oplossingen die zich momenteel in preview. Er is geen schema die hieronder worden beschreven kan worden gewijzigd.

In dit artikel wordt beschreven hoe u voor het definiëren van Log Analytics opgeslagen zoekopdrachten in een [Resource Manager-sjabloon](../../azure-resource-manager/resource-manager-quickstart-create-templates-use-the-portal.md) , zodat ze kunnen worden opgenomen [beheeroplossingen](solutions-creating.md).

> [!NOTE]
> De voorbeelden in dit artikel Gebruik parameters en variabelen die zijn vereist of gemeenschappelijke beheeroplossingen en wordt beschreven in [ontwerpen en bouwen van een oplossing in Azure](solutions-creating.md)

## <a name="prerequisites"></a>Vereisten
In dit artikel wordt ervan uitgegaan dat u bent al bekend bent met het [maken van een oplossing voor](solutions-creating.md) en de structuur van een [Resource Manager-sjabloon](../../azure-resource-manager/resource-group-authoring-templates.md) en de bestandsnaam van de oplossing.


## <a name="log-analytics-workspace"></a>Log Analytics-werkruimte
Alle resources in Log Analytics zijn opgenomen in een [werkruimte](../../azure-monitor/platform/manage-access.md). Zoals beschreven in [Log Analytics-werkruimte en het Automation-account](solutions.md#log-analytics-workspace-and-automation-account), de werkruimte niet is opgenomen in de oplossing voor beheer, maar moet bestaan voordat de oplossing is geïnstalleerd. Als deze niet beschikbaar is, mislukt de installatie van de oplossing.

De naam van de werkruimte is de naam van elke Log Analytics-resource. Dit doet u in de oplossing met de **werkruimte** parameter zoals in het volgende voorbeeld van een resource SavedSearch.

    "name": "[concat(parameters('workspaceName'), '/', variables('SavedSearchId'))]"

## <a name="log-analytics-api-version"></a>Log Analytics-API-versie
Alle Log Analytics-resources in een Resource Manager-sjabloon gedefinieerd hebben een eigenschap **apiVersion** die de versie van de API moet worden gebruikt door de resource definieert.

De volgende tabel bevat de API-versie voor de resource die in dit voorbeeld worden gebruikt.

| Resourcetype | API-versie | Query’s uitvoeren |
|:---|:---|:---|
| savedSearches | 2017-03-15-preview | Event &#124; where EventLevelName == "Error"  |


## <a name="saved-searches"></a>Opgeslagen zoekopdrachten
Opnemen [opgeslagen zoekopdrachten](../../azure-monitor/log-query/log-query-overview.md) in een oplossing waarmee gebruikers query uitvoeren op gegevens die door uw oplossing worden verzameld. Opgeslagen zoekopdrachten worden weergegeven onder **opgeslagen zoekopdrachten** in Azure portal. Een opgeslagen zoekopdracht is ook vereist voor elke waarschuwing.

[Log Analytics opgeslagen zoekopdracht](../../azure-monitor/log-query/log-query-overview.md) resources zijn een type `Microsoft.OperationalInsights/workspaces/savedSearches` en hebben de volgende structuur. Dit omvat de algemene variabelen en parameters zodat u kunt kopiëren en plak dit codefragment in uw oplossingsbestand en wijzig de namen van parameters.

    {
        "name": "[concat(parameters('workspaceName'), '/', variables('SavedSearch').Name)]",
        "type": "Microsoft.OperationalInsights/workspaces/savedSearches",
        "apiVersion": "[variables('LogAnalyticsApiVersion')]",
        "dependsOn": [
        ],
        "tags": { },
        "properties": {
            "etag": "*",
            "query": "[variables('SavedSearch').Query]",
            "displayName": "[variables('SavedSearch').DisplayName]",
            "category": "[variables('SavedSearch').Category]"
        }
    }

Elke eigenschap van een opgeslagen zoekopdracht wordt in de volgende tabel beschreven.

| Eigenschap | Description |
|:--- |:--- |
| category | De categorie voor de opgeslagen zoekopdracht.  Alle opgeslagen zoekopdrachten in dezelfde oplossing delen vaak één categorie, zodat ze samen worden gegroepeerd in de console. |
| displayname | De naam om weer te geven voor de opgeslagen zoekopdracht in de portal. |
| query | De query wilt uitvoeren. |

> [!NOTE]
> Mogelijk moet u het escape-tekens gebruiken in de query bevat tekens die kunnen worden geïnterpreteerd als JSON. Bijvoorbeeld, als uw query is **AzureActivity | OperationName:"Microsoft.Compute/virtualMachines/write"**, ze moet worden geschreven in het oplossingsbestand als **AzureActivity | OperationName: /\"Microsoft.Compute/virtualMachines/write\"**.


## <a name="next-steps"></a>Volgende stappen
* [Weergaven toevoegen](solutions-resources-views.md) aan uw oplossing.
* [Automation-runbooks en andere resources toevoegen](solutions-resources-automation.md) aan uw oplossing.
