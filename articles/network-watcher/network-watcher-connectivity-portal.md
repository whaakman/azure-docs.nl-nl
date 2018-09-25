---
title: Problemen oplossen met verbindingen met Azure Network Watcher - Azure portal | Microsoft Docs
description: Meer informatie over het gebruik van de verbinding oplossen van de mogelijkheden van Azure Network Watcher met behulp van de Azure portal.
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
ms.openlocfilehash: 21e004e12a5111eb0e5fc7764c1e07fcb68c447d
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46986197"
---
# <a name="troubleshoot-connections-with-azure-network-watcher-using-the-azure-portal"></a>Problemen oplossen met verbindingen met Azure Network Watcher met behulp van de Azure portal

> [!div class="op_single_selector"]
> - [Portal](network-watcher-connectivity-portal.md)
> - [PowerShell](network-watcher-connectivity-powershell.md)
> - [Azure-CLI](network-watcher-connectivity-cli.md)
> - [Azure REST-API](network-watcher-connectivity-rest.md)

Informatie over het gebruik van de verbinding oplossen om te controleren of een rechtstreekse TCP-verbinding van een virtuele machine naar een bepaald eindpunt kan worden gemaakt.

## <a name="before-you-begin"></a>Voordat u begint

In dit artikel wordt ervan uitgegaan dat u hebt de volgende bronnen:

* Een exemplaar van Network Watcher in de regio die u wilt oplossen, een verbinding.
* Virtuele machines, problemen oplossen met verbindingen met.

> [!IMPORTANT]
> Probleemoplossing voor verbindingen vereist dat de virtuele machine bij het oplossen van problemen met van heeft de `AzureNetworkWatcherExtension` VM-extensie is geïnstalleerd. Ga voor het installeren van de extensie op een Windows-VM naar [Azure Network Watcher-Agent de extensie van de virtuele machine voor Windows](../virtual-machines/windows/extensions-nwa.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json) en voor Linux-VM naar [Azure Network Watcher-Agent de extensie van de virtuele machine voor Linux](../virtual-machines/linux/extensions-nwa.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json). De extensie is niet vereist op het eindpunt van de bestemming.

## <a name="check-connectivity-to-a-virtual-machine"></a>Controleer de verbinding met een virtuele machine

In dit voorbeeld controleert de verbinding met een virtuele doelmachine via poort 80.

Navigeer naar uw Network Watcher en klikt u op **probleemoplossing voor verbindingen**. Selecteer de virtuele machine om te controleren op connectiviteit van. In de **bestemming** sectie Kies **selecteert u een virtuele machine** en kies het juiste virtuele machine en de poort om te testen.

Nadat u op **controleren**, connectiviteit tussen de virtuele machines in de opgegeven poort is ingeschakeld. De bestemming VM is niet bereikbaar, een overzicht van hops worden weergegeven in het voorbeeld.

![Resultaten van de verbinding voor een virtuele machine][1]

## <a name="check-remote-endpoint-connectivity"></a>Controleer de connectiviteit van Extern eindpunt

Om te controleren of de connectiviteit en de latentie naar een extern eindpunt, kiest u de **handmatig opgeven** keuzerondje in de **bestemming** sectie, voer de url en de poort en klik op **controleren**.  Dit wordt gebruikt voor externe eindpunten, zoals websites en storage-eindpunten.

![Resultaten van de verbinding voor een website][2]

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het automatiseren van pakketopnamen met waarschuwingen van de virtuele machine via [maken van een waarschuwing geactiveerd pakketopname](network-watcher-alert-triggered-packet-capture.md)

Zoeken of bepaalde verkeer is toegestaan in of buiten uw virtuele machine door naar de pagina [controleren IP-stroom controleren](diagnose-vm-network-traffic-filtering-problem.md)

[1]: ./media/network-watcher-connectivity-portal/figure1.png
[2]: ./media/network-watcher-connectivity-portal/figure2.png