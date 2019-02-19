---
title: Stroomlogboeken van Netwerkbeveiligingsgroep beheren met Azure Network Watcher - REST-API | Microsoft Docs
description: Deze pagina wordt uitgelegd hoe u voor het beheren van stroomlogboeken van Netwerkbeveiligingsgroep in Azure Network Watcher met REST-API
services: network-watcher
documentationcenter: na
author: jimdial
manager: timlt
editor: ''
ms.assetid: 2ab25379-0fd3-4bfe-9d82-425dfc7ad6bb
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: jdial
ms.openlocfilehash: 565b5bcce114ab5ebd2169ece11a958013cf2ee5
ms.sourcegitcommit: fcb674cc4e43ac5e4583e0098d06af7b398bd9a9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/18/2019
ms.locfileid: "56338900"
---
# <a name="configuring-network-security-group-flow-logs-using-rest-api"></a>Stroomlogboeken van Netwerkbeveiligingsgroep configureren met behulp van REST-API

> [!div class="op_single_selector"]
> - [Azure Portal](network-watcher-nsg-flow-logging-portal.md)
> - [PowerShell](network-watcher-nsg-flow-logging-powershell.md)
> - [Azure-CLI](network-watcher-nsg-flow-logging-cli.md)
> - [REST API](network-watcher-nsg-flow-logging-rest.md)

Stroomlogboeken van Netwerkbeveiligingsgroep zijn een functie van Network Watcher waarmee u informatie wilt weergeven over inkomende en uitgaande IP-verkeer via een Netwerkbeveiligingsgroep. Deze logboeken van de stroom zijn geschreven in json-indeling en weergeven van binnenkomende en uitgaande stromen op basis van per regel, de NIC die de stroom is van toepassing op, 5-tuple-informatie over de stroom (bron-/ doel-IP-adres, poort van de bron-/ doel, Protocol), en als het verkeer is toegestaan of geweigerd.

## <a name="before-you-begin"></a>Voordat u begint

ARMclient wordt gebruikt voor het aanroepen van de REST-API met behulp van PowerShell. ARMClient is gevonden op chocolatey op [ARMClient op Chocolatey](https://chocolatey.org/packages/ARMClient)

In dit scenario wordt ervan uitgegaan dat u de stappen in al hebt gevolgd [maken van een Network Watcher](network-watcher-create.md) te maken van een Network Watcher.

> [!Important]
> Voor Network Watcher REST API-aanroepen dat de naam van de resourcegroep in de aanvraag-URI is de resourcegroep met de netwerk-Watcher, niet de resources u de diagnostische acties op uitvoert.

## <a name="scenario"></a>Scenario

Het scenario beschreven in dit artikel leest u hoe het inschakelen, uitschakelen en het opvragen van Logboeken van de stroom met behulp van de REST-API. Voor meer informatie over Network Security Group stroom loggings, gaat u naar [stroomlogboeken van Netwerkbeveiligingsgroep - overzicht](network-watcher-nsg-flow-logging-overview.md).

U wordt in dit scenario:

* Logboeken van de stroom (versie 2) inschakelen
* Logboeken van de stroom uitschakelen
* Status van het flow-Logboeken

## <a name="log-in-with-armclient"></a>Meld u aan met ARMClient

Meld u aan bij armclient met uw Azure-referenties.

```PowerShell
armclient login
```

## <a name="register-insights-provider"></a>Insights-provider registreren

Stroom logboekregistratie om te werken, zodat de **Microsoft.Insights** provider moet worden geregistreerd. Als u niet zeker weet als de **Microsoft.Insights** provider is geregistreerd, voer het volgende script.

```powershell
$subscriptionId = "00000000-0000-0000-0000-000000000000"
armclient post "https://management.azure.com//subscriptions/${subscriptionId}/providers/Microsoft.Insights/register?api-version=2016-09-01"
```

## <a name="enable-network-security-group-flow-logs"></a>Stroomlogboeken van Netwerkbeveiligingsgroep inschakelen

De opdracht uit om flow logboeken versie 2 wordt weergegeven in het volgende voorbeeld. Vervang het versieveld met '1' voor versie 1:

```powershell
$subscriptionId = "00000000-0000-0000-0000-000000000000"
$targetUri = "" # example /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/{resourceGroupName/providers/Microsoft.Network/networkSecurityGroups/{nsgName}"
$storageId = "/subscriptions/00000000-0000-0000-0000-000000000000/{resourceGroupName/providers/Microsoft.Storage/storageAccounts/{saName}"
$resourceGroupName = "NetworkWatcherRG"
$networkWatcherName = "NetworkWatcher_westcentralus"
$requestBody = @"
{
    'targetResourceId': '${targetUri}',
    'properties': {
    'storageId': '${storageId}',
    'enabled': 'true',
    'retentionPolicy' : {
            days: 5,
            enabled: true
        },
    'format': {
        'type': 'JSON',
        'version': 2
    }
    }
}
"@

armclient post "https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${resourceGroupName}/providers/Microsoft.Network/networkWatchers/${networkWatcherName}/configureFlowLog?api-version=2016-12-01" $requestBody
```

Het antwoord geretourneerd uit het vorige voorbeeld is als volgt:

```json
{
  "targetResourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/networkSecurityGroups/{nsgName}",
  "properties": {
    "storageId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{saName}",
    "enabled": true,
    "retentionPolicy": {
      "days": 5,
      "enabled": true
    },
    "format": {
    "type": "JSON",
    "version": 2
    }
  }
}
```

## <a name="disable-network-security-group-flow-logs"></a>Stroomlogboeken van Netwerkbeveiligingsgroep uitschakelen

Gebruik het volgende voorbeeld om uit te schakelen van Logboeken van de stroom. De aanroep is hetzelfde als het inschakelen van Logboeken van de stroom, met uitzondering van **false** voor de eigenschap enabled is ingesteld.

```powershell
$subscriptionId = "00000000-0000-0000-0000-000000000000"
$targetUri = "" # example /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/{resourceGroupName/providers/Microsoft.Network/networkSecurityGroups/{nsgName}"
$storageId = "/subscriptions/00000000-0000-0000-0000-000000000000/{resourceGroupName/providers/Microsoft.Storage/storageAccounts/{saName}"
$resourceGroupName = "NetworkWatcherRG"
$networkWatcherName = "NetworkWatcher_westcentralus"
$requestBody = @"
{
    'targetResourceId': '${targetUri}',
    'properties': {
    'storageId': '${storageId}',
    'enabled': 'false',
    'retentionPolicy' : {
            days: 5,
            enabled: true
        },
    'format': {
        'type': 'JSON',
        'version': 2
    }
    }
}
"@

armclient post "https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${resourceGroupName}/providers/Microsoft.Network/networkWatchers/${networkWatcherName}/configureFlowLog?api-version=2016-12-01" $requestBody
```

Het antwoord geretourneerd uit het vorige voorbeeld is als volgt:

```json
{
  "targetResourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/networkSecurityGroups/{nsgName}",
  "properties": {
    "storageId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{saName}",
    "enabled": false,
    "retentionPolicy": {
      "days": 5,
      "enabled": true
    },
    "format": {
    "type": "JSON",
    "version": 2
    }
  }
}
```

## <a name="query-flow-logs"></a>Query-stroomlogboeken

De volgende query's voor REST-aanroep van de status van flow registreert op een Netwerkbeveiligingsgroep.

```powershell
$subscriptionId = "00000000-0000-0000-0000-000000000000"
$targetUri = "" # example /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/{resourceGroupName/providers/Microsoft.Network/networkSecurityGroups/{nsgName}"
$resourceGroupName = "NetworkWatcherRG"
$networkWatcherName = "NetworkWatcher_westcentralus"
$requestBody = @"
{
    'targetResourceId': '${targetUri}',
}
"@

armclient post "https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${resourceGroupName}/providers/Microsoft.Network/networkWatchers/${networkWatcherName}/queryFlowLogStatus?api-version=2016-12-01" $requestBody
```

Hier volgt een voorbeeld van het antwoord geretourneerd:

```json
{
  "targetResourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/networkSecurityGroups/{nsgName}",
  "properties": {
    "storageId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{saName}",
    "enabled": true,
   "retentionPolicy": {
      "days": 5,
      "enabled": true
    },
    "format": {
    "type": "JSON",
    "version": 2
    }
  }
}
```

## <a name="download-a-flow-log"></a>Een stroomlogboek downloaden

De opslaglocatie van het stroomlogboek van een wordt gedefinieerd bij het maken ervan. Een handig hulpmiddel voor toegang tot deze stroomlogboeken die zijn opgeslagen op een storage-account is Microsoft Azure Storage Explorer, die u kunt hier downloaden:  http://storageexplorer.com/

Als een storage-account is opgegeven, worden pakketten vastleggen van bestanden worden opgeslagen in een storage-account op de volgende locatie:

```
https://{storageAccountName}.blob.core.windows.net/insights-logs-networksecuritygroupflowevent/resourceId=/SUBSCRIPTIONS/{subscriptionID}/RESOURCEGROUPS/{resourceGroupName}/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/{nsgName}/y={year}/m={month}/d={day}/h={hour}/m=00/macAddress={macAddress}/PT1H.json
```

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het [visualiseren met Power BI uw NSG-stroomlogboeken](network-watcher-visualize-nsg-flow-logs-power-bi.md)

Meer informatie over het [visualiseren van uw NSG-stroomlogboeken met open-sourcehulpprogramma's](network-watcher-visualize-nsg-flow-logs-open-source-tools.md)
