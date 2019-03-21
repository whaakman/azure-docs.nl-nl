---
title: Logboeken met Azure Network Watcher - PowerShell Network Security Group stromen beheren | Microsoft Docs
description: Deze pagina wordt uitgelegd hoe u voor het beheren van Network Security Group Flow Logboeken in Azure Network Watcher met PowerShell
services: network-watcher
documentationcenter: na
author: jimdial
manager: timlt
editor: ''
ms.assetid: 2dfc3112-8294-4357-b2f8-f81840da67d3
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: jdial
ms.openlocfilehash: ebeebfa4490797493a781bf462d363d1cbcf2d55
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57857977"
---
# <a name="configuring-network-security-group-flow-logs-with-powershell"></a>Network Security Group Flow logboeken configureren met PowerShell

> [!div class="op_single_selector"]
> - [Azure-portal](network-watcher-nsg-flow-logging-portal.md)
> - [PowerShell](network-watcher-nsg-flow-logging-powershell.md)
> - [Azure-CLI](network-watcher-nsg-flow-logging-cli.md)
> - [REST API](network-watcher-nsg-flow-logging-rest.md)

Stroomlogboeken van Netwerkbeveiligingsgroep zijn een functie van Network Watcher waarmee u informatie wilt weergeven over inkomende en uitgaande IP-verkeer via een Netwerkbeveiligingsgroep. Deze logboeken van de stroom zijn geschreven in json-indeling en weergeven van binnenkomende en uitgaande stromen op basis van per regel, de NIC die de stroom is van toepassing op, 5-tuple-informatie over de stroom (bron-/ doel-IP-adres, poort van de bron-/ doel, Protocol), en als het verkeer is toegestaan of geweigerd.

## <a name="register-insights-provider"></a>Insights-provider registreren

Stroom logboekregistratie om te werken, zodat de **Microsoft.Insights** provider moet worden geregistreerd. Als u niet zeker weet als de **Microsoft.Insights** provider is geregistreerd, voer het volgende script.

```powershell
Register-AzResourceProvider -ProviderNamespace Microsoft.Insights
```

## <a name="enable-network-security-group-flow-logs-and-traffic-analytics"></a>Stroomlogboeken van Netwerkbeveiligingsgroep inschakelen en Traffic Analytics

De opdracht uit om Logboeken van de stroom wordt weergegeven in het volgende voorbeeld:

```powershell
$NW = Get-AzNetworkWatcher -ResourceGroupName NetworkWatcherRg -Name NetworkWatcher_westcentralus
$nsg = Get-AzNetworkSecurityGroup -ResourceGroupName nsgRG -Name nsgName
$storageAccount = Get-AzStorageAccount -ResourceGroupName StorageRG -Name contosostorage123
Get-AzNetworkWatcherFlowLogStatus -NetworkWatcher $NW -TargetResourceId $nsg.Id

#Traffic Analytics Parameters
$workspaceResourceId = "/subscriptions/bbbbbbbb-bbbb-bbbb-bbbb-bbbbbbbbbbbb/resourcegroups/trafficanalyticsrg/providers/microsoft.operationalinsights/workspaces/taworkspace"
$workspaceGUID = "cccccccc-cccc-cccc-cccc-cccccccccccc"
$workspaceLocation = "westeurope"

#Configure Version 1 Flow Logs
Set-AzNetworkWatcherConfigFlowLog -NetworkWatcher $NW -TargetResourceId $nsg.Id -StorageAccountId $storageAccount.Id -EnableFlowLog $true -FormatType Json -FormatVersion 1

#Configure Version 2 Flow Logs, and configure Traffic Analytics
Set-AzNetworkWatcherConfigFlowLog -NetworkWatcher $NW -TargetResourceId $nsg.Id -StorageAccountId $storageAccount.Id -EnableFlowLog $true -FormatType Json -FormatVersion 2

#Configure Version 2 FLow Logs with Traffic Analytics Configured
Set-AzNetworkWatcherConfigFlowLog -NetworkWatcher $NW -TargetResourceId $nsg.Id -StorageAccountId $storageAccount.Id -EnableFlowLog $true -FormatType Json -FormatVersion 2 -EnableTrafficAnalytics -WorkspaceResourceId $workspaceResourceId -WorkspaceGUID $workspaceid -WorkspaceLocation $workspaceRegion

#Query Flow Log Status
Get-AzNetworkWatcherFlowLogStatus -NetworkWatcher $NW -TargetResourceId $nsg.Id
```

Het opslagaccount dat u opgeeft kan niet zijn geconfigureerd voor het netwerkregels waarmee toegang tot het netwerk wordt beperkt tot alleen Microsoft-services of specifieke virtuele netwerken. Het opslagaccount kan zich in dezelfde of een ander Azure-abonnement, dan de NSG waarmee het stroomlogboek voor. Als u verschillende abonnementen vallen, moeten ze beide zijn gekoppeld aan dezelfde Azure Active Directory-tenant. Het account dat u voor elk abonnement gebruikt moet hebben de [benodigde machtigingen](required-rbac-permissions.md).

## <a name="disable-traffic-analytics-and-network-security-group-flow-logs"></a>Logboeken voor Traffic Analytics en Network Security Group Flow uitschakelen

Gebruik het volgende voorbeeld verkeersanalyse uitschakelen en logboeken flow:

```powershell
#Disable Traffic Analaytics by removing -EnableTrafficAnalytics property
Set-AzNetworkWatcherConfigFlowLog -NetworkWatcher $NW -TargetResourceId $nsg.Id -StorageAccountId $storageAccount.Id -EnableFlowLog $true -FormatType Json -FormatVersion 2 -WorkspaceResourceId $workspaceResourceId -WorkspaceGUID $workspaceGUID -WorkspaceLocation $workspaceLocation

#Disable Flow Logging
Set-AzNetworkWatcherConfigFlowLog -NetworkWatcher $NW -TargetResourceId $nsg.Id -StorageAccountId $storageAccount.Id -EnableFlowLog $false
```

## <a name="download-a-flow-log"></a>Een Flow-log Download

De opslaglocatie van het stroomlogboek van een wordt gedefinieerd bij het maken ervan. Een handig hulpmiddel voor toegang tot deze stroomlogboeken die zijn opgeslagen op een storage-account is Microsoft Azure Storage Explorer, die u kunt hier downloaden:  https://storageexplorer.com/

Als een storage-account is opgegeven, worden logboekbestanden van de stroom worden opgeslagen in een storage-account op de volgende locatie:

```
https://{storageAccountName}.blob.core.windows.net/insights-logs-networksecuritygroupflowevent/resourceId=/SUBSCRIPTIONS/{subscriptionID}/RESOURCEGROUPS/{resourceGroupName}/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/{nsgName}/y={year}/m={month}/d={day}/h={hour}/m=00/macAddress={macAddress}/PT1H.json
```

Ga voor informatie over de structuur van het logboek naar [Network Security Group Flow-log overzicht](network-watcher-nsg-flow-logging-overview.md)

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het [visualiseren met Power BI uw NSG-stroomlogboeken](network-watcher-visualize-nsg-flow-logs-power-bi.md)

Meer informatie over het [visualiseren van uw NSG-stroomlogboeken met open-sourcehulpprogramma's](network-watcher-visualize-nsg-flow-logs-open-source-tools.md)
