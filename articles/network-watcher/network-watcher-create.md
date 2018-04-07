---
title: Maak een instantie van de Azure-netwerk-Watcher | Microsoft Docs
description: Informatie over het inschakelen van netwerk-Watcher in een Azure-regio.
services: network-watcher
documentationcenter: na
author: jimdial
manager: timlt
editor: ''
ms.assetid: b1314119-0b87-4f4d-b44c-2c4d0547fb76
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: jdial
ms.openlocfilehash: 9d3e579cd58bc6c7d67b29998ea5a48a65548b0a
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/06/2018
---
# <a name="create-an-azure-network-watcher-instance"></a>Maak een instantie van de Azure-netwerk-Watcher

Netwerk-Watcher is een regionale service waarmee u kunt bewaken en onderzoeken van de voorwaarden op een netwerk scenario niveau in, en naar Azure. Scenario niveau bewaking, kunt u problemen op een weergave voor het niveau van end-to-end-netwerk. Netwerkcontrole en visualisatie hulpprogramma's beschikbaar met de netwerk-Watcher kunnen u begrijpen, diagnoses stellen en Verkrijg inzicht in uw netwerk in Azure.

## <a name="create-a-network-watcher-in-the-portal"></a>Maken van een netwerk-Watcher in de portal

Navigeer naar **alle Services** > **Networking** > **netwerk-Watcher**. U kunt alle abonnementen die u wilt inschakelen, netwerk-Watcher voor selecteren. Deze actie wordt een netwerk-Watcher in elke regio die beschikbaar is gemaakt.

![maken van een netwerk-watcher](./media/network-watcher-create/figure1.png)

Wanneer u via de portal van netwerk-Watcher inschakelt, wordt de naam van het exemplaar van de netwerk-Watcher automatisch ingesteld op *NetworkWatcher_region_name* waar *region_name* komt overeen met de Azure-regio Wanneer het exemplaar is ingeschakeld. Bijvoorbeeld een netwerk-Watcher ingeschakeld in de regio West-Centraal VS heet *NetworkWatcher_westcentralus*.

Het exemplaar van de netwerk-Watcher wordt automatisch gemaakt in een resourcegroep met de naam *NetworkWatcherRG*. De resourcegroep wordt gemaakt als deze niet al bestaat.

Als u wilt aanpassen, de naam van een netwerk-Watcher-exemplaar en de resourcegroep in geplaatst, kunt u Powershell, de Azure CLI, de REST-API of ARMClient-methoden die in de volgende secties beschreven. Bij elke optie worden de resourcegroep moet bestaan voordat u een netwerk-Watcher maken.  

## <a name="create-a-network-watcher-with-powershell"></a>Maken van een netwerk-Watcher met PowerShell

Voer het volgende voorbeeld voor het maken van een exemplaar van netwerk-Watcher:

```powershell
New-AzureRmNetworkWatcher -Name "NetworkWatcher_westcentralus" -ResourceGroupName "NetworkWatcherRG" -Location "West Central US"
```

## <a name="create-a-network-watcher-with-the-azure-cli"></a>Een netwerk-Watcher maken met de Azure CLI

Voer het volgende voorbeeld voor het maken van een exemplaar van netwerk-Watcher:

```azurecli
az network watcher configure --resource-group NetworkWatcherRG --locations westcentralus --enabled
```

## <a name="create-a-network-watcher-with-the-rest-api"></a>Een netwerk-Watcher maken met de REST-API

De ARMclient wordt gebruikt voor het aanroepen van de REST-API met behulp van PowerShell. De ARMClient is gevonden op chocolatey op [ARMClient op Chocolatey](https://chocolatey.org/packages/ARMClient)

### <a name="log-in-with-armclient"></a>Meld u aan met ARMClient

```powerShell
armclient login
```

### <a name="create-the-network-watcher"></a>Maken van de netwerk-watcher

```powershell
$subscriptionId = '<subscription id>'
$networkWatcherName = '<name of network watcher>'
$resourceGroupName = '<resource group name>'
$apiversion = "2016-09-01"
$requestBody = @"
{
'location': 'West Central US'
}
"@

armclient put "https://management.azure.com/subscriptions/${subscriptionId}/resourceGroups/${resourceGroupName}/providers/Microsoft.Network/networkWatchers/${networkWatcherName}?api-version=${api-version}" $requestBody
```

## <a name="next-steps"></a>Volgende stappen

Nu dat u een exemplaar van netwerk-Watcher hebt, kunt u meer over de beschikbare functies:

* [Topologie](network-watcher-topology-overview.md)
* [Pakketopname](network-watcher-packet-capture-overview.md)
* [IP-stroomverificatie](network-watcher-ip-flow-verify-overview.md)
* [Volgende hop](network-watcher-next-hop-overview.md)
* [Beveiligingsgroepweergave](network-watcher-security-group-view-overview.md)
* [NSG stroom logboekregistratie](network-watcher-nsg-flow-logging-overview.md)
* [Virtuele netwerkgateway probleemoplossing](network-watcher-troubleshoot-overview.md)

Zodra een netwerk-Watcher-exemplaar is, kunt u pakketopname binnen de virtuele machines kunt inschakelen. Voor meer informatie Zie [maken van een waarschuwing geactiveerd pakketopname](network-watcher-alert-triggered-packet-capture.md)
