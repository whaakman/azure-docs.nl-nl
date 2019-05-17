---
title: Resourcewijzigingen ophalen
description: Meer informatie over het zoeken wanneer een resource is gewijzigd en een overzicht van de eigenschappen die gewijzigd.
services: resource-graph
author: DCtheGeek
ms.author: dacoulte
ms.date: 05/10/2019
ms.topic: conceptual
ms.service: resource-graph
manager: carmonm
ms.openlocfilehash: 4e28ca15197f89caeaeaca0aabb648755b8235f1
ms.sourcegitcommit: f013c433b18de2788bf09b98926c7136b15d36f1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/13/2019
ms.locfileid: "65551547"
---
# <a name="get-resource-changes"></a>Resourcewijzigingen ophalen

Resources gewijzigd in de loop van dagelijks gebruik herconfiguratie en zelfs opnieuw implementeren.
Wijziging kan afkomstig zijn van een persoon of door een geautomatiseerd proces. De meeste wijzigen is standaard, maar soms is het niet. Met de afgelopen 14 dagen van de wijzigingsgeschiedenis kunt u Azure Resource Graph:

- Zien wanneer wijzigingen in een Azure Resource Manager-eigenschap zijn gedetecteerd.
- Zien welke eigenschappen tijdens die gebeurtenis zijn gewijzigd.

Wijziging detectie en gegevens zijn waardevol zijn bij de volgende voorbeeldscenario:

- Tijdens de incidentbeheer om te begrijpen _mogelijk_ gerelateerde wijzigingen. Zoeken naar gebeurtenissen gedurende een bepaalde periode en de details van de wijziging evalueren.
- Houdt een Configuratiebeheer-Database, bekend als een CMDB is up-to-date. In plaats van de vernieuwing van alle resources en hun volledige eigenschap ingesteld op een geplande frequentie, moet u alleen krijgen wat is gewijzigd.
- Inzicht in welke andere eigenschappen mogelijk zijn gewijzigd wanneer een resource nalevingsstatus gewijzigd. Evaluatie van de aanvullende eigenschappen kan inzicht geven in andere eigenschappen die mogelijk moeten worden beheerd via de definitie van een Azure Policy.

In dit artikel laat zien hoe deze informatie via de SDK van de grafiek van de resources te verzamelen. Deze gegevens in Azure portal wilt bekijken, zien van Azure Policy [wijzigingsoverzicht](../../policy/how-to/determine-non-compliance.md#change-history-preview) of Azure-activiteitenlogboek [wijzigingsoverzicht](../../../azure-monitor/platform/activity-logs-overview.md#view-change-history).

> [!NOTE]
> Details van de wijziging in de grafiek Resource zijn voor Resource Manager-eigenschappen. Zie voor het bijhouden van wijzigingen in een virtuele machine van Azure Automation [wijzigingen bijhouden](../../../automation/automation-change-tracking.md) of Azure-beleid [Gast-configuratie voor virtuele machines](../../policy/concepts/guest-configuration.md).

> [!IMPORTANT]
> Wijzigingsoverzicht in Azure Resource Graph is in openbare Preview.

## <a name="find-when-changes-were-detected"></a>Zoeken wanneer wijzigingen gedetecteerd

De eerste stap bij het zien wat er gewijzigd voor een bron is te vinden van de gebeurtenissen met betrekking tot die resource binnen een periode. Deze stap vindt plaats via de **resourceChanges** REST-eindpunt.

De **resourceChanges** eindpunt vereist twee parameters in de aanvraagtekst:

- **resourceId**: De Azure-resource om te zoeken om wijzigingen op.
- **interval**: Een eigenschap met de _start_ en _end_ datums voor het controleren op een wijziging gebeurtenis met de **Zulu tijdzone (Z)**.

Voorbeeld van de aanvraag hoofdtekst:

```json
{
    "resourceId": "/subscriptions/{subscriptionId}/resourceGroups/MyResourceGroup/providers/Microsoft.Storage/storageAccounts/mystorageaccount",
    "interval": {
        "start": "2019-03-28T00:00:00.000Z",
        "end": "2019-03-31T00:00:00.000Z"
    }
}
```

Met de bovenstaande aanvraagtekst, de REST-API-URI voor **resourceChanges** is:

```http
POST https://management.azure.com/providers/Microsoft.ResourceGraph/resourceChanges?api-version=2018-09-01-preview
```

Het antwoord lijkt op het volgende voorbeeld:

```json
{
    "changes": [{
            "changeId": "{\"beforeId\":\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\",\"beforeTime\":'2019-05-09T00:00:00.000Z\",\"afterId\":\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\",\"beforeTime\":'2019-05-10T00:00:00.000Z\"}",
            "beforeSnapshot": {
                "timestamp": "2019-03-29T01:32:05.993Z"
            },
            "afterSnapshot": {
                "timestamp": "2019-03-29T01:54:24.42Z"
            }
        },
        {
            "changeId": "9dc352cb-b7c1-4198-9eda-e5e3ed66aec8",
            "beforeSnapshot": {
                "timestamp": "2019-03-28T10:30:19.68Z"
            },
            "afterSnapshot": {
                "timestamp": "2019-03-28T21:12:31.337Z"
            }
        }
    ]
}
```

Elke gedetecteerde wijzigingsgebeurtenis voor de **resourceId** heeft een **changeId** die uniek is voor die bron. Terwijl de **changeId** tekenreeks kan soms ook andere eigenschappen bevatten, is er alleen gegarandeerd uniek te zijn. De tijden bevatten die de wijzigingsrecord die de vóór en na-momentopnamen zijn gemaakt.
De wijzigingsgebeurtenis heeft plaatsgevonden op een bepaald moment in dit venster van de tijd.

## <a name="see-what-properties-changed"></a>Zie wat er eigenschappen gewijzigd

Met de **changeId** uit de **resourceChanges** eindpunt, de **resourceChangeDetails** REST-eindpunt wordt vervolgens gebruikt om de details van de wijzigingsgebeurtenis ophalen.

De **resourceChangeDetails** eindpunt vereist twee parameters in de aanvraagtekst:

- **resourceId**: De Azure-resource om te zoeken om wijzigingen op.
- **changeId**: De unieke wijzigingsgebeurtenis voor de **resourceId** die zijn verzameld uit **resourceChanges**.

Voorbeeld van de aanvraag hoofdtekst:

```json
{
    "resourceId": "/subscriptions/{subscriptionId}/resourceGroups/MyResourceGroup/providers/Microsoft.Storage/storageAccounts/mystorageaccount",
    "changeId": "{\"beforeId\":\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\",\"beforeTime\":'2019-05-09T00:00:00.000Z\",\"afterId\":\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\",\"beforeTime\":'2019-05-10T00:00:00.000Z\"}"
}
```

Met de bovenstaande aanvraagtekst, de REST-API-URI voor **resourceChangeDetails** is:

```http
POST https://management.azure.com/providers/Microsoft.ResourceGraph/resourceChangeDetails?api-version=2018-09-01-preview
```

Het antwoord lijkt op het volgende voorbeeld:

```json
{
    "changeId": "{\"beforeId\":\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\",\"beforeTime\":'2019-05-09T00:00:00.000Z\",\"afterId\":\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\",\"beforeTime\":'2019-05-10T00:00:00.000Z\"}",
    "beforeSnapshot": {
        "timestamp": "2019-03-29T01:32:05.993Z",
        "content": {
            "sku": {
                "name": "Standard_LRS",
                "tier": "Standard"
            },
            "kind": "Storage",
            "id": "/subscriptions/{subscriptionId}/resourceGroups/MyResourceGroup/providers/Microsoft.Storage/storageAccounts/mystorageaccount",
            "name": "mystorageaccount",
            "type": "Microsoft.Storage/storageAccounts",
            "location": "westus",
            "tags": {},
            "properties": {
                "networkAcls": {
                    "bypass": "AzureServices",
                    "virtualNetworkRules": [],
                    "ipRules": [],
                    "defaultAction": "Allow"
                },
                "supportsHttpsTrafficOnly": false,
                "encryption": {
                    "services": {
                        "file": {
                            "enabled": true,
                            "lastEnabledTime": "2018-07-27T18:37:21.8333895Z"
                        },
                        "blob": {
                            "enabled": true,
                            "lastEnabledTime": "2018-07-27T18:37:21.8333895Z"
                        }
                    },
                    "keySource": "Microsoft.Storage"
                },
                "provisioningState": "Succeeded",
                "creationTime": "2018-07-27T18:37:21.7708872Z",
                "primaryEndpoints": {
                    "blob": "https://mystorageaccount.blob.core.windows.net/",
                    "queue": "https://mystorageaccount.queue.core.windows.net/",
                    "table": "https://mystorageaccount.table.core.windows.net/",
                    "file": "https://mystorageaccount.file.core.windows.net/"
                },
                "primaryLocation": "westus",
                "statusOfPrimary": "available"
            }
        }
    },
    "afterSnapshot": {
        "timestamp": "2019-03-29T01:54:24.42Z",
        "content": {
            "sku": {
                "name": "Standard_LRS",
                "tier": "Standard"
            },
            "kind": "Storage",
            "id": "/subscriptions/{subscriptionId}/resourceGroups/MyResourceGroup/providers/Microsoft.Storage/storageAccounts/mystorageaccount",
            "name": "mystorageaccount",
            "type": "Microsoft.Storage/storageAccounts",
            "location": "westus",
            "tags": {},
            "properties": {
                "networkAcls": {
                    "bypass": "AzureServices",
                    "virtualNetworkRules": [],
                    "ipRules": [],
                    "defaultAction": "Allow"
                },
                "supportsHttpsTrafficOnly": true,
                "encryption": {
                    "services": {
                        "file": {
                            "enabled": true,
                            "lastEnabledTime": "2018-07-27T18:37:21.8333895Z"
                        },
                        "blob": {
                            "enabled": true,
                            "lastEnabledTime": "2018-07-27T18:37:21.8333895Z"
                        }
                    },
                    "keySource": "Microsoft.Storage"
                },
                "provisioningState": "Succeeded",
                "creationTime": "2018-07-27T18:37:21.7708872Z",
                "primaryEndpoints": {
                    "blob": "https://mystorageaccount.blob.core.windows.net/",
                    "queue": "https://mystorageaccount.queue.core.windows.net/",
                    "table": "https://mystorageaccount.table.core.windows.net/",
                    "file": "https://mystorageaccount.file.core.windows.net/"
                },
                "primaryLocation": "westus",
                "statusOfPrimary": "available"
            }
        }
    }
}
```

**beforeSnapshot** en **afterSnapshot** elk geeft de tijd die de momentopname is gemaakt en de eigenschappen op dat moment. De wijziging is doorgevoerd op een bepaald moment tussen deze momentopnamen. Het bovenstaande voorbeeld bekijkt, ziet u dat de eigenschap die gewijzigd is **supportsHttpsTrafficOnly**.

Als u wilt de resultaten een programmatische manier vergelijken, vergelijkt de **inhoud** gedeelte van elke momentopname om het verschil te bepalen. Als u de volledige momentopname, vergelijkt de **timestamp** altijd wordt weergegeven als een verschil ondanks wordt verwacht.

## <a name="next-steps"></a>Volgende stappen

- Zie de taal die wordt gebruikt in [Starter query's](../samples/starter.md).
- Maakt gebruik van geavanceerde Zie in [geavanceerde query's](../samples/advanced.md).
- Meer informatie over het [resources verkennen](../concepts/explore-resources.md).