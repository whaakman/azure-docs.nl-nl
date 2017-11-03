---
title: Bewaken van bewerkingen, gebeurtenissen en prestatiemeteritems voor het nsg's | Microsoft Docs
description: Informatie over het inschakelen van tellers, gebeurtenissen en operationele logboekregistratie voor het nsg 's
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 2e699078-043f-48bd-8aa8-b011a32d98ca
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/31/2017
ms.author: jdial
ms.openlocfilehash: 552f37dd704de25159bc0f0ad34fdae9ed8b73f5
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="log-analytics-for-network-security-groups-nsgs"></a>Logboekanalyses voor netwerkbeveiligingsgroepen (NSG's)

U kunt de volgende categorieën van diagnostische logboeken inschakelen voor het nsg's:

* **Gebeurtenis:** bevat vermeldingen voor welke NSG regels worden toegepast op virtuele machines en functies op basis van MAC-adres-instantie. De status voor deze regels worden verzameld om de 60 seconden.
* **Regelteller:** bevat vermeldingen voor hoe vaak elke NSG regel wordt toegepast om te weigeren of verkeer toestaan.

> [!NOTE]
> Diagnostische logboeken zijn alleen beschikbaar voor het nsg's geïmplementeerd via het Azure Resource Manager-implementatiemodel. U kunt Diagnostische logboekregistratie voor het nsg's geïmplementeerd via het klassieke implementatiemodel niet inschakelen. Voor een beter begrip van de twee modellen, raadpleegt u de [wat Azure-implementatiemodellen](../resource-manager-deployment-model.md) artikel.

Logboekregistratie van activiteit (voorheen bekend als audit- of operationele Logboeken) is standaard ingeschakeld voor het nsg's die zijn gemaakt via de Azure-implementatiemodel. Om te bepalen welke bewerkingen zijn voltooid op het nsg's in het activiteitenlogboek, zoeken naar gegevens die de volgende resourcetypen bevatten: 

- Microsoft.ClassicNetwork/networkSecurityGroups 
- Microsoft.ClassicNetwork/networkSecurityGroups/securityRules
- Microsoft.Network/networkSecurityGroups
- Microsoft.Network/networkSecurityGroups/securityRules 

Lees de [overzicht van de Azure Activity Log](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md) artikel voor meer informatie over activiteitenlogboeken. 

## <a name="enable-diagnostic-logging"></a>Bijhouden van diagnostische gegevens inschakelen

Diagnostische logboekregistratie moet zijn ingeschakeld voor *elke* NSG die u wenst te verzamelen van gegevens voor. De [overzicht van Azure diagnostische logboeken](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) artikel wordt uitgelegd waar diagnostische logboeken kunnen worden verzonden. Als u een bestaande NSG geen hebt, voer de stappen in de [maken van een netwerkbeveiligingsgroep](virtual-networks-create-nsg-arm-pportal.md) artikel een maken. U kunt NSG Diagnostische logboekregistratie met behulp van een van de volgende methoden inschakelen:

### <a name="azure-portal"></a>Azure Portal

Gebruik de portal voor het inschakelen van logboekregistratie, meld u aan bij de [portal](https://portal.azure.com). Klik op **meer services**, typ *netwerkbeveiligingsgroepen*. Selecteer het NSG u logboekregistratie wilt inschakelen voor. Volg de instructies voor niet-rekenresources in de [Schakel diagnostische logboeken in de portal](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md#how-to-enable-collection-of-resource-diagnostic-logs) artikel. Selecteer **NetworkSecurityGroupEvent**, **NetworkSecurityGroupRuleCounter**, of beide categorieën van Logboeken.

### <a name="powershell"></a>PowerShell

Voor het gebruik van PowerShell logboekregistratie in te schakelen, volg de instructies in de [Schakel diagnostische logboeken via PowerShell](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md#how-to-enable-collection-of-resource-diagnostic-logs) artikel. Evalueer de volgende informatie voordat u een opdracht invoert in het artikel:

- U kunt bepalen dat de waarde moet worden gebruikt voor de `-ResourceId` parameter door de volgende vervangen [tekst] waar nodig, typt u de opdracht `Get-AzureRmNetworkSecurityGroup -Name [nsg-name] -ResourceGroupName [resource-group-name]`. De uitvoer van de ID van de opdracht ziet er ongeveer uit */subscriptions/ [abonnement Id]/resourceGroups/[resource-group]/providers/Microsoft.Network/networkSecurityGroups/[NSG name]*.
- Als u alleen gegevens wilt verzamelen uit logboek categorie toevoegen `-Categories [category]` aan het einde van de opdracht in het artikel, waarbij categorie is *NetworkSecurityGroupEvent* of *NetworkSecurityGroupRuleCounter*. Als u niet de `-Categories` parameter gegevensverzameling is ingeschakeld voor zowel logboekbestand categorieën.

### <a name="azure-command-line-interface-cli"></a>Azure-opdrachtregelinterface (CLI)

Voor het gebruik van de CLI logboekregistratie in te schakelen, volg de instructies in de [Schakel diagnostische logboeken via CLI](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md#how-to-enable-collection-of-resource-diagnostic-logs) artikel. Evalueer de volgende informatie voordat u een opdracht invoert in het artikel:

- U kunt bepalen dat de waarde moet worden gebruikt voor de `-ResourceId` parameter door de volgende vervangen [tekst] waar nodig, typt u de opdracht `azure network nsg show [resource-group-name] [nsg-name]`. De uitvoer van de ID van de opdracht ziet er ongeveer uit */subscriptions/ [abonnement Id]/resourceGroups/[resource-group]/providers/Microsoft.Network/networkSecurityGroups/[NSG name]*.
- Als u alleen gegevens wilt verzamelen uit logboek categorie toevoegen `-Categories [category]` aan het einde van de opdracht in het artikel, waarbij categorie is *NetworkSecurityGroupEvent* of *NetworkSecurityGroupRuleCounter*. Als u niet de `-Categories` parameter gegevensverzameling is ingeschakeld voor zowel logboekbestand categorieën.

## <a name="logged-data"></a>Logboekgegevens

Gegevens in JSON-indeling is geschreven voor beide logboeken. De specifieke gegevens die zijn geschreven voor elk Logboektype wordt vermeld in de volgende secties:

### <a name="event-log"></a>Gebeurtenislogboek
Dit logboek bevat informatie over welke NSG regels worden toegepast op virtuele machines en cloud service rolinstanties, op basis van MAC-adres. Het volgende voorbeeldgegevens worden geregistreerd voor elke gebeurtenis:

```json
{
    "time": "[DATE-TIME]",
    "systemId": "007d0441-5d6b-41f6-8bfd-930db640ec03",
    "category": "NetworkSecurityGroupEvent",
    "resourceId": "/SUBSCRIPTIONS/[SUBSCRIPTION-ID]/RESOURCEGROUPS/[RESOURCE-GROUP-NAME]/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/[NSG-NAME]",
    "operationName": "NetworkSecurityGroupEvents",
    "properties": {
        "vnetResourceGuid":"{5E8AEC16-C728-441F-B0CA-B791E1DBC2F4}",
        "subnetPrefix":"192.168.1.0/24",
        "macAddress":"00-0D-3A-92-6A-7C",
        "primaryIPv4Address":"192.168.1.4",
        "ruleName":"UserRule_default-allow-rdp",
        "direction":"In",
        "priority":1000,
        "type":"allow",
        "conditions":{
            "protocols":"6",
            "destinationPortRange":"3389-3389",
            "sourcePortRange":"0-65535",
            "sourceIP":"0.0.0.0/0",
            "destinationIP":"0.0.0.0/0"
            }
        }
}
```

### <a name="rule-counter-log"></a>Regel teller logboek

Dit logboek bevat informatie over elke regel toegepast op resources. Het volgende voorbeeldgegevens worden geregistreerd telkens wanneer die een regel wordt toegepast:

```json
{
    "time": "[DATE-TIME]",
    "systemId": "007d0441-5d6b-41f6-8bfd-930db640ec03",
    "category": "NetworkSecurityGroupRuleCounter",
    "resourceId": "/SUBSCRIPTIONS/[SUBSCRIPTION ID]/RESOURCEGROUPS/[RESOURCE-GROUP-NAME]TESTRG/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/[NSG-NAME]",
    "operationName": "NetworkSecurityGroupCounters",
    "properties": {
        "vnetResourceGuid":"{5E8AEC16-C728-441F-B0CA-791E1DBC2F4}",
        "subnetPrefix":"192.168.1.0/24",
        "macAddress":"00-0D-3A-92-6A-7C",
        "primaryIPv4Address":"192.168.1.4",
        "ruleName":"UserRule_default-allow-rdp",
        "direction":"In",
        "type":"allow",
        "matchedConnections":125
        }
}
```

## <a name="view-and-analyze-logs"></a>Weergeven en analyseren van Logboeken

Lees voor meer informatie over het weergeven van de logboekgegevens van de activiteit, de [overzicht van de Azure Activity Log](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) artikel. Lees voor meer informatie over het weergeven van diagnostische logboekgegevens, de [overzicht van Azure diagnostische logboeken](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) artikel. Als u diagnostische gegevens naar Log Analytics verzenden, kunt u de [Netwerkbeveiligingsgroep Azure analytics](../log-analytics/log-analytics-azure-networking-analytics.md) oplossing voor verbeterde insights (preview). 
