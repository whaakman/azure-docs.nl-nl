---
title: Netwerk beveiliging groep overgebracht logboeken met Azure-netwerk-Watcher - PowerShell beheren | Microsoft Docs
description: Deze pagina wordt uitgelegd hoe u voor het beheren van Logboeken netwerk beveiliging groep overgebracht in Azure-netwerk-Watcher met PowerShell
services: network-watcher
documentationcenter: na
author: jimdial
manager: timlt
editor: 
ms.assetid: 2dfc3112-8294-4357-b2f8-f81840da67d3
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: jdial
ms.openlocfilehash: 5c514cc3d281d9e2baeae415aed240579af75650
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="configuring-network-security-group-flow-logs-with-powershell"></a>Netwerk beveiliging groep overgebracht logboeken configureren met PowerShell

> [!div class="op_single_selector"]
> - [Azure Portal](network-watcher-nsg-flow-logging-portal.md)
> - [PowerShell](network-watcher-nsg-flow-logging-powershell.md)
> - [CLI 1.0](network-watcher-nsg-flow-logging-cli-nodejs.md)
> - [CLI 2.0](network-watcher-nsg-flow-logging-cli.md)
> - [REST API](network-watcher-nsg-flow-logging-rest.md)

Netwerkbeveiligingsgroep stroom logboeken zijn een functie van netwerk-Watcher waarmee u informatie bekijken over inkomende en uitgaande IP-verkeer via een Netwerkbeveiligingsgroep. Deze stroom logboeken zijn geschreven in json-indeling en binnenkomende en uitgaande stromen weergeven op basis van een per regel, de NIC die de stroom van toepassing, 5-tuple informatie over de stroom (bron/het doel-IP, bron/het doel-poort, het Protocol) en als het verkeer is toegestaan of geweigerd.

## <a name="register-insights-provider"></a>Insights provider registreren

Stroom logboekregistratie om te werken, zodat de **Microsoft.Insights** provider moet worden geregistreerd. Als u niet zeker weet of de **Microsoft.Insights** provider is geregistreerd, voer het volgende script.

```powershell
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Insights
```

## <a name="enable-network-security-group-flow-logs"></a>Netwerkbeveiligingsgroep inschakelen stroom Logboeken

De opdracht uit om Logboeken van de stroom wordt weergegeven in het volgende voorbeeld:

```powershell
$NW = Get-AzurermNetworkWatcher -ResourceGroupName NetworkWatcherRg -Name NetworkWatcher_westcentralus
$nsg = Get-AzureRmNetworkSecurityGroup -ResourceGroupName nsgRG-Name nsgName
$storageAccount = Get-AzureRmStorageAccount -ResourceGroupName StorageRG -Name contosostorage123
Get-AzureRmNetworkWatcherFlowLogStatus -NetworkWatcher $NW -TargetResourceId $nsg.Id
Set-AzureRmNetworkWatcherConfigFlowLog -NetworkWatcher $NW -TargetResourceId $nsg.Id -StorageAccountId $storageAccount.Id -EnableFlowLog $true
```

## <a name="disable-network-security-group-flow-logs"></a>Netwerkbeveiligingsgroep uitschakelen stroom Logboeken

Gebruik het volgende voorbeeld stroom Logboeken uitschakelen:

```powershell
Set-AzureRmNetworkWatcherConfigFlowLog -NetworkWatcher $NW -TargetResourceId $nsg.Id -StorageAccountId $storageAccount.Id -EnableFlowLog $false
```

## <a name="download-a-flow-log"></a>Downloaden van een stroom-logboek

De locatie voor de opslag van een stroom-logboek is gedefinieerd bij het maken. Een handig hulpmiddel voor toegang tot deze stroom logboeken opgeslagen naar een opslagaccount is Microsoft Azure Storage Explorer, die u kunt hier downloaden: http://storageexplorer.com/

Als een opslagaccount is opgegeven, worden bestanden voor pakket worden opgeslagen in een opslagaccount op de volgende locatie:

```
https://{storageAccountName}.blob.core.windows.net/insights-logs-networksecuritygroupflowevent/resourceId=/SUBSCRIPTIONS/{subscriptionID}/RESOURCEGROUPS/{resourceGroupName}/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/{nsgName}/y={year}/m={month}/d={day}/h={hour}/m=00/macAddress={macAddress}/PT1H.json
```

Ga naar voor informatie over de structuur van het logboek [netwerk beveiliging groep overgebracht logboek overzicht](network-watcher-nsg-flow-logging-overview.md)

## <a name="next-steps"></a>Volgende stappen

Meer informatie over hoe [visualiseren van uw NSG stroom logboeken met Power BI](network-watcher-visualize-nsg-flow-logs-power-bi.md)

Meer informatie over hoe [visualiseren van uw NSG stroom logboeken met open-source hulpprogramma's](network-watcher-visualize-nsg-flow-logs-open-source-tools.md)