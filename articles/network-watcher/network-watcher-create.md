---
title: Maak een instantie van Azure Network Watcher | Microsoft Docs
description: Informatie over het inschakelen van Network Watcher in een Azure-regio.
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
ms.openlocfilehash: ea10e83e8a5963c1ea0073179c15b1c2f3230805
ms.sourcegitcommit: b62f138cc477d2bd7e658488aff8e9a5dd24d577
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/13/2018
ms.locfileid: "51615197"
---
# <a name="create-an-azure-network-watcher-instance"></a>Maak een instantie van Azure Network Watcher

Network Watcher is een regionale service waarmee u kunt bewaken en diagnoses uitvoeren omstandigheden op netwerkscenarioniveau in, en naar Azure. Scenario niveau bewaking, kunt u vaststellen op een weergave voor het niveau van end-to-netwerk. Diagnose en visualisatie hulpprogramma's die beschikbaar zijn met Network Watcher kunnen u begrijpen, diagnosticeren en inzicht verkrijgen in uw netwerk in Azure. Network Watcher is ingeschakeld via het maken van een Network Watcher-resource. Deze bron kunt u gebruikmaken van Network Watcher-mogelijkheden.

## <a name="network-watcher-is-automatically-enabled"></a>Network Watcher is automatisch ingeschakeld.
Wanneer u maken of bijwerken van een virtueel netwerk in uw abonnement, wordt Network Watcher automatisch ingeschakeld in de regio van uw virtuele netwerk. Er zijn geen gevolgen voor uw resources of de bijbehorende kosten in rekening gebracht voor het automatisch inschakelen van Network Watcher.

#### <a name="opt-out-of-network-watcher-automatic-enablement"></a>Opt-out van de automatische activering van Network Watcher
Als u afmelden voor automatische activering van Network Watcher wilt, kunt u dit doen door het uitvoeren van de volgende opdrachten:

> [!WARNING]
> Wanneer-out van de automatische activering van Network Watcher is een permanente wijziging. Zodra u zich afmelden u kan niet aanmelden zonder [contact opnemen met ondersteuning](https://azure.microsoft.com/support/options/)

```azurepowershell-interactive
Register-AzureRmProviderFeature -FeatureName DisableNetworkWatcherAutocreation -ProviderNamespace Microsoft.Network
Register-AzureRMResourceProvider -ProviderNamespace Microsoft.Network
```

```azurecli-interactive
az feature register --name DisableNetworkWatcherAutocreation --namespace Microsoft.Network
az provider register -n Microsoft.Network
```



## <a name="create-a-network-watcher-in-the-portal"></a>Maken van een Network Watcher in de portal

Navigeer naar **alle Services** > **netwerken** > **Network Watcher**. U kunt alle abonnementen die u wilt inschakelen, Network Watcher voor selecteren. Deze actie wordt een Network Watcher in elke regio die beschikbaar is gemaakt.

![Maken van een network watcher](./media/network-watcher-create/figure1.png)

Wanneer u een Network Watcher met behulp van de portal inschakelt, wordt de naam van de Network Watcher-exemplaar automatisch ingesteld op *NetworkWatcher_region_name* waar *region_name* komt overeen met de Azure-regio Wanneer het exemplaar is ingeschakeld. Bijvoorbeeld, een Network Watcher ingeschakeld in de regio West-Centraal VS heet *NetworkWatcher_westcentralus*.

Het exemplaar van Network Watcher wordt automatisch gemaakt in een resourcegroep met de naam *NetworkWatcherRG*. De resourcegroep wordt gemaakt als deze niet al bestaat.

Als u wilt aanpassen van de naam van een Network Watcher-exemplaar en de resourcegroep wordt geplaatst in, kunt u Powershell, de Azure CLI, de REST-API of ARMClient methoden die worden beschreven in de volgende secties. Bij elke optie worden de resourcegroep moet bestaan voordat u een Network Watcher in het maken.  

## <a name="create-a-network-watcher-with-powershell"></a>Een Network Watcher maken met PowerShell

Voer het volgende voorbeeld voor het maken van een exemplaar van Network Watcher:

```powershell
New-AzureRmNetworkWatcher -Name "NetworkWatcher_westcentralus" -ResourceGroupName "NetworkWatcherRG" -Location "West Central US"
```

## <a name="create-a-network-watcher-with-the-azure-cli"></a>Een Network Watcher maken met de Azure CLI

Voer het volgende voorbeeld voor het maken van een exemplaar van Network Watcher:

```azurecli
az network watcher configure --resource-group NetworkWatcherRG --locations westcentralus --enabled
```

## <a name="create-a-network-watcher-with-the-rest-api"></a>Een Network Watcher maken met de REST-API

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

Nu dat u een exemplaar van Network Watcher hebt, kunt u informatie over de beschikbare functies:

* [Topologie](network-watcher-topology-overview.md)
* [Pakketopname](network-watcher-packet-capture-overview.md)
* [IP-stroomverificatie](network-watcher-ip-flow-verify-overview.md)
* [Volgende hop](network-watcher-next-hop-overview.md)
* [Beveiligingsgroepweergave](network-watcher-security-group-view-overview.md)
* [NSG-stroomlogboeken](network-watcher-nsg-flow-logging-overview.md)
* [Virtuele netwerkgateway oplossen](network-watcher-troubleshoot-overview.md)

Nadat een Network Watcher-exemplaar is, kunt u pakketopname binnen virtuele machines inschakelen. Voor meer informatie Zie [maken van een waarschuwing geactiveerd pakketopname](network-watcher-alert-triggered-packet-capture.md)
