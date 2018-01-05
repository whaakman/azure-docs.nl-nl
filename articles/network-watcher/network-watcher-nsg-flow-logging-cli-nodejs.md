---
title: Logboeken met Azure-netwerk-Watcher - Azure CLI 1.0 netwerk beveiliging groep overgebracht beheren | Microsoft Docs
description: Deze pagina wordt uitgelegd hoe u Logboeken netwerk beveiliging groep overgebracht in Azure-netwerk-Watcher met Azure CLI 1.0 beheren
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
ms.openlocfilehash: 1cdef3454a326fbdbdef5ab51ac93cc8a558f34d
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="configuring-network-security-group-flow-logs-with-azure-cli-10"></a>Netwerk beveiliging groep overgebracht logboeken configureren met Azure CLI 1.0

> [!div class="op_single_selector"]
> - [Azure Portal](network-watcher-nsg-flow-logging-portal.md)
> - [PowerShell](network-watcher-nsg-flow-logging-powershell.md)
> - [CLI 1.0](network-watcher-nsg-flow-logging-cli-nodejs.md)
> - [CLI 2.0](network-watcher-nsg-flow-logging-cli.md)
> - [REST API](network-watcher-nsg-flow-logging-rest.md)

Netwerkbeveiligingsgroep stroom logboeken zijn een functie van netwerk-Watcher waarmee u informatie bekijken over inkomende en uitgaande IP-verkeer via een Netwerkbeveiligingsgroep. Deze stroom logboeken zijn geschreven in json-indeling en binnenkomende en uitgaande stromen weergeven op basis van een per regel, de NIC die de stroom van toepassing, 5-tuple informatie over de stroom (bron/het doel-IP, bron/het doel-poort, het Protocol) en als het verkeer is toegestaan of geweigerd.

Dit artikel wordt platformoverschrijdende Azure CLI 1.0 gebruikt die beschikbaar is voor Windows, Mac en Linux. Netwerk-Watcher gebruikt momenteel de Azure CLI 1.0 voor CLI-ondersteuning.

## <a name="register-insights-provider"></a>Insights provider registreren

Stroom logboekregistratie om te werken, zodat de **Microsoft.Insights** provider moet worden geregistreerd. Als u niet zeker weet of de **Microsoft.Insights** provider is geregistreerd, voer het volgende script.

```azurecli
azure provider register --namespace Microsoft.Insights --subscription <subscriptionid>
```

## <a name="enable-network-security-group-flow-logs"></a>Netwerkbeveiligingsgroep inschakelen stroom Logboeken

De opdracht uit om Logboeken van de stroom wordt weergegeven in het volgende voorbeeld:

```azurecli
azure network watcher configure-flow-log -g resourceGroupName -n networkWatcherName -t nsgId -i storageAccountId -e true
```

## <a name="disable-network-security-group-flow-logs"></a>Netwerkbeveiligingsgroep uitschakelen stroom Logboeken

Gebruik het volgende voorbeeld stroom Logboeken uitschakelen:

```azurecli
azure network watcher configure-flow-log -g resourceGroupName -n networkWatcherName -t nsgId -i storageAccountId -e false
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
