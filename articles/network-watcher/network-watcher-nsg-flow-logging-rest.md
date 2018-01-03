---
title: Netwerkbeveiligingsgroep beheren stroom logboeken met Azure-netwerk-Watcher - REST-API | Microsoft Docs
description: Deze pagina wordt uitgelegd hoe u voor het beheren van Netwerkbeveiligingsgroep stroom Logboeken in Azure-netwerk-Watcher met REST-API
services: network-watcher
documentationcenter: na
author: jimdial
manager: timlt
editor: 
ms.assetid: 2ab25379-0fd3-4bfe-9d82-425dfc7ad6bb
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: jdial
ms.openlocfilehash: 4444e83adecdc1afa170a184705b9be3be67c026
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="configuring-network-security-group-flow-logs-using-rest-api"></a>Configureren van Netwerkbeveiligingsgroep stroom logboeken met REST-API

> [!div class="op_single_selector"]
> - [Azure Portal](network-watcher-nsg-flow-logging-portal.md)
> - [PowerShell](network-watcher-nsg-flow-logging-powershell.md)
> - [CLI 1.0](network-watcher-nsg-flow-logging-cli-nodejs.md)
> - [CLI 2.0](network-watcher-nsg-flow-logging-cli.md)
> - [REST API](network-watcher-nsg-flow-logging-rest.md)

Netwerkbeveiligingsgroep stroom logboeken zijn een functie van netwerk-Watcher waarmee u informatie bekijken over inkomende en uitgaande IP-verkeer via een Netwerkbeveiligingsgroep. Deze stroom logboeken zijn geschreven in json-indeling en binnenkomende en uitgaande stromen weergeven op basis van een per regel, de NIC die de stroom van toepassing, 5-tuple informatie over de stroom (bron/het doel-IP, bron/het doel-poort, het Protocol) en als het verkeer is toegestaan of geweigerd.

## <a name="before-you-begin"></a>Voordat u begint

ARMclient wordt gebruikt voor het aanroepen van de REST-API met behulp van PowerShell. ARMClient is gevonden op chocolatey op [ARMClient op Chocolatey](https://chocolatey.org/packages/ARMClient)

Dit scenario wordt ervan uitgegaan dat u de stappen in al hebt gevolgd [maken van een netwerk-Watcher](network-watcher-create.md) voor het maken van een netwerk-Watcher.

> [!Important]
> Voor netwerk-Watcher REST API-aanroepen dat de naam van de resourcegroep in de aanvraag-URI is de resourcegroep met de netwerk-Watcher, niet de resources u diagnostische acties op uitvoert.

## <a name="scenario"></a>Scenario

Het scenario beschreven in dit artikel wordt beschreven hoe u inschakelen, uitschakelen en query stroom logboeken met behulp van de REST-API. Voor meer informatie over Netwerkbeveiligingsgroep stroom loggings, gaat u naar [Netwerkbeveiligingsgroep stroom logboekregistratie - overzicht](network-watcher-nsg-flow-logging-overview.md).

U wordt in dit scenario:

* Stroom Logboeken inschakelen
* Stroom Logboeken uitschakelen
* Status logboeken van de stroom

## <a name="log-in-with-armclient"></a>Meld u aan met ARMClient

Aanmelden bij armclient met uw Azure-referenties.

```PowerShell
armclient login
```

## <a name="register-insights-provider"></a>Insights provider registreren

Stroom logboekregistratie om te werken, zodat de **Microsoft.Insights** provider moet worden geregistreerd. Als u niet zeker weet of de **Microsoft.Insights** provider is geregistreerd, voer het volgende script.

```powershell
$subscriptionId = "00000000-0000-0000-0000-000000000000"
armclient post "https://management.azure.com//subscriptions/${subscriptionId}/providers/Microsoft.Insights/register?api-version=2016-09-01"
```

## <a name="enable-network-security-group-flow-logs"></a>Netwerkbeveiligingsgroep inschakelen stroom Logboeken

De opdracht uit om Logboeken van de stroom wordt weergegeven in het volgende voorbeeld:

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
    }
  }
}
```

## <a name="disable-network-security-group-flow-logs"></a>Netwerkbeveiligingsgroep uitschakelen stroom Logboeken

Gebruik het volgende voorbeeld stroom Logboeken uitschakelen. De aanroep is hetzelfde als het inschakelen van Logboeken van de stroom, met uitzondering van **false** voor de eigenschap enabled is ingesteld.

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
    }
  }
}
```

## <a name="query-flow-logs"></a>Query stroom Logboeken

De volgende query's voor REST-aanroep van de status van de stroom meldt zich aan een Netwerkbeveiligingsgroep.

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
    }
  }
}
```

## <a name="download-a-flow-log"></a>Downloaden van een stroom-logboek

De locatie voor de opslag van een stroom-logboek is gedefinieerd bij het maken. Een handig hulpmiddel voor toegang tot deze stroom logboeken opgeslagen naar een opslagaccount is Microsoft Azure Storage Explorer, die u kunt hier downloaden: http://storageexplorer.com/

Als een opslagaccount is opgegeven, worden bestanden voor pakket worden opgeslagen in een opslagaccount op de volgende locatie:

```
https://{storageAccountName}.blob.core.windows.net/insights-logs-networksecuritygroupflowevent/resourceId=/SUBSCRIPTIONS/{subscriptionID}/RESOURCEGROUPS/{resourceGroupName}/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/{nsgName}/y={year}/m={month}/d={day}/h={hour}/m=00/macAddress={macAddress}/PT1H.json
```

## <a name="next-steps"></a>Volgende stappen

Meer informatie over hoe [visualiseren van uw NSG stroom logboeken met Power BI](network-watcher-visualize-nsg-flow-logs-power-bi.md)

Meer informatie over hoe [visualiseren van uw NSG stroom logboeken met open-source hulpprogramma's](network-watcher-visualize-nsg-flow-logs-open-source-tools.md)
