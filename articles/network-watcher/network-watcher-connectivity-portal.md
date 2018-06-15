---
title: Verbindingen met de Azure-netwerk-Watcher - Azure-portal oplossen | Microsoft Docs
description: Informatie over het gebruik van de verbinding mogelijkheid van netwerk-Watcher van Azure met Azure portal oplossen.
services: network-watcher
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: ''
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/03/2017
ms.author: jdial
ms.openlocfilehash: cf7b71a49b63a95ed535210125120c6b76d9de8f
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2018
ms.locfileid: "32181759"
---
# <a name="troubleshoot-connections-with-azure-network-watcher-using-the-azure-portal"></a>Verbindingen met de netwerk-Watcher van Azure met Azure portal oplossen

> [!div class="op_single_selector"]
> - [Portal](network-watcher-connectivity-portal.md)
> - [PowerShell](network-watcher-connectivity-powershell.md)
> - [CLI 2.0](network-watcher-connectivity-cli.md)
> - [Azure REST-API](network-watcher-connectivity-rest.md)

Informatie over het gebruik van verbinding oplossen om te controleren of een directe TCP-verbinding van een virtuele machine naar een opgegeven eindpunt kan worden gemaakt.

## <a name="before-you-begin"></a>Voordat u begint

In dit artikel wordt ervan uitgegaan dat u hebt de volgende bronnen:

* Een exemplaar van netwerk-Watcher in de regio die u wilt een verbinding oplossen.
* Virtuele machines verbindingen met oplossen.

> [!IMPORTANT]
> Het oplossen van de verbinding vereist dat de virtuele machine die u wilt van oplossen de `AzureNetworkWatcherExtension` VM-extensie is geïnstalleerd. Voor het installeren van de extensie op een Windows-virtuele machine gaat u naar [extensie voor het virtuele machine voor Windows Azure-netwerk-Watcher Agent](../virtual-machines/windows/extensions-nwa.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json) en voor Linux-VM naar [Azure-netwerk-Watcher Agent de extensie van de virtuele machine voor Linux](../virtual-machines/linux/extensions-nwa.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json). De extensie is niet vereist op de doeleindpunt.

## <a name="check-connectivity-to-a-virtual-machine"></a>Controleer de verbinding met een virtuele machine

In dit voorbeeld controleert de verbinding met een doel-virtuele machine via poort 80.

Navigeer naar uw netwerk-Watcher en klikt u op **verbinding oplossen**. Selecteer de virtuele machine, Controleer de verbinding tussen. In de **bestemming** sectie kiezen **Selecteer een virtuele machine** en kies de juiste virtuele machine en poort om te testen.

Nadat u op **controleren**, connectiviteit tussen de virtuele machines op de opgegeven poort is ingeschakeld. De doel-virtuele machine is niet bereikbaar, een overzicht van hops weergegeven in het voorbeeld.

![Resultaten van de verbinding voor een virtuele machine][1]

## <a name="check-remote-endpoint-connectivity"></a>Controleer de verbindingen van het externe eindpunt

Als u wilt controleren de connectiviteit en de latentie van een extern eindpunt, kies de **handmatig opgeven** keuzerondje in de **bestemming** sectie, voert u de url en de poort en klik op **controleren**.  Dit wordt gebruikt voor externe eindpunten zoals websites en -opslag-eindpunten.

![Resultaten van de verbinding voor een website][2]

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het automatiseren van pakket opnamen met waarschuwingen van de virtuele machine met weer te geven [maken van een waarschuwing geactiveerd pakketopname](network-watcher-alert-triggered-packet-capture.md)

Als bepaalde verkeer is toegestaan in of buiten uw virtuele machine in via vinden [controleren IP-stroom controleren](diagnose-vm-network-traffic-filtering-problem.md)

[1]: ./media/network-watcher-connectivity-portal/figure1.png
[2]: ./media/network-watcher-connectivity-portal/figure2.png