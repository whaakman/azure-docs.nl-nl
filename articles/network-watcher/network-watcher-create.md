---
title: Maak een instantie van de Azure-netwerk-Watcher | Microsoft Docs
description: Deze pagina bevat de stappen voor het maken van een exemplaar van netwerk-Watcher met behulp van de portal en Azure REST-API
services: network-watcher
documentationcenter: na
author: jimdial
manager: timlt
editor: 
ms.assetid: b1314119-0b87-4f4d-b44c-2c4d0547fb76
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: jdial
ms.openlocfilehash: fb1e3f9a93d18e949cb42ac0a4e09129cfe414f6
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/21/2018
---
# <a name="create-an-azure-network-watcher-instance"></a>Maak een instantie van de Azure-netwerk-Watcher

Netwerk-Watcher is een regionale service waarmee u kunt bewaken en onderzoeken van de voorwaarden op een netwerk scenario niveau in, en naar Azure. Scenario niveau bewaking, kunt u problemen op een weergave voor het niveau van end-to-end-netwerk. Netwerkcontrole en visualisatie hulpprogramma's beschikbaar met de netwerk-Watcher kunnen u begrijpen, diagnoses stellen en Verkrijg inzicht in uw netwerk in Azure.

> [!NOTE]
> Zoals netwerk-Watcher ondersteunt momenteel alleen CLI 1.0, krijgt u de instructies voor het maken van een nieuw exemplaar van de netwerk-Watcher voor CLI 1.0.

## <a name="create-a-network-watcher-in-the-portal"></a>Maken van een netwerk-Watcher in de portal

Navigeer naar **alle Services** > **Networking** > **netwerk-Watcher**. U kunt alle abonnementen die u wilt inschakelen, netwerk-Watcher voor selecteren. Deze actie wordt een netwerk-Watcher in elke regio die beschikbaar is gemaakt.

![maken van een netwerk-watcher][1]

Wanneer u via de Portal van netwerk-Watcher inschakelt, is de naam van het exemplaar van de netwerk-Watcher wordt automatisch ingesteld op NetworkWatcher_region_name waarbij region_name overeenkomt met de Azure-regio waar het exemplaar is ingeschakeld.  Een netwerk-Watcher ingeschakeld in de regio West-Centraal VS wordt bijvoorbeeld de naam NetworkWatcher_westcentralus

Het exemplaar van de netwerk-Watcher wordt daarnaast automatisch worden toegevoegd aan een resourcegroep NetworkWatcherRG aangeroepen.  Deze resourcegroep wordt gemaakt als deze niet al bestaat.

Als u wilt aanpassen, de naam van een netwerk-Watcher-exemplaar en de resourcegroep wordt deze geplaatst in, kunt u Powershell, de REST-API of ARMClient methoden die hieronder worden beschreven.  Bij elke optie worden de resourcegroep moet bestaan voordat u de netwerk-Watcher in de App plaatsen.  

## <a name="create-a-network-watcher-with-powershell"></a>Maken van een netwerk-Watcher met PowerShell

Voer het volgende voorbeeld voor het maken van een exemplaar van netwerk-Watcher:

```powershell
New-AzureRmNetworkWatcher -Name "NetworkWatcher_westcentralus" -ResourceGroupName "NetworkWatcherRG" -Location "West Central US"
```

## <a name="create-a-network-watcher-with-the-rest-api"></a>Een netwerk-Watcher maken met de REST-API

ARMclient wordt gebruikt voor het aanroepen van de REST-API met behulp van PowerShell. ARMClient is gevonden op chocolatey op [ARMClient op Chocolatey](https://chocolatey.org/packages/ARMClient)

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

Zodra een netwerk-Watcher-exemplaar is gemaakt, pakket vastleggen kan worden geconfigureerd door het artikel te volgen: [maken van een waarschuwing geactiveerd pakketopname](network-watcher-alert-triggered-packet-capture.md)

[1]: ./media/network-watcher-create/figure1.png











