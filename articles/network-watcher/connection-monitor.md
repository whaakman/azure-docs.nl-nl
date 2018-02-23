---
title: Netwerkverbindingen met de Azure-netwerk-Watcher - Azure-portal bewaken | Microsoft Docs
description: Informatie over het bewaken van netwerkverbinding met de netwerk-Watcher van Azure met Azure portal.
services: network-watcher
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: 
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/16/2018
ms.author: jdial
ms.openlocfilehash: beaa458d727a05eccf496933deb3c998828868cd
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/21/2018
---
# <a name="monitor-network-connections-with-azure-network-watcher-using-the-azure-portal"></a>Netwerkverbindingen met de netwerk-Watcher van Azure met Azure portal controleren

Informatie over het bewaken van de verbinding gebruiken voor het bewaken van netwerkverbinding tussen Azure een virtuele machine en een IP-adres. Het IP-adres kan worden toegewezen aan een andere Azure-resource of een Internet- of on-premises resource.

## <a name="prerequisites"></a>Vereisten

U moet voldoen aan de volgende vereisten voordat u de stappen in dit artikel uitvoert:

* Een exemplaar van netwerk-Watcher in de regio die u wilt bewaken van een verbinding voor. Als u dit niet al hebt, kunt u een door de stappen in [maken van een exemplaar van Azure-netwerk-Watcher](network-watcher-create.md).
* Een virtuele machine voor het bewaken van.
* Hebben de `AzureNetworkWatcherExtension` geïnstalleerd in de virtuele machine die u wilt bewaken van een verbinding van. Zie voor informatie over het installeren van de uitbreiding in virtuele Windows-machine [extensie voor het virtuele machine voor Windows Azure-netwerk-Watcher Agent](../virtual-machines/windows/extensions-nwa.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json) en installeren van de extensie in een virtuele machine Linux Zie [Azure-netwerk-Watcher-Agent de extensie van de virtuele machine voor Linux](../virtual-machines/linux/extensions-nwa.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json).

## <a name="sign-in-to-azure"></a>Aanmelden bij Azure 

Meld u aan bij [Azure Portal](http://portal.azure.com).

## <a name="create-a-connection-monitor"></a>Een monitor verbinding maken

1. Selecteer aan de linkerkant van de portal **meer services**.
2. Typ *netwerk-watcher*. Wanneer **netwerk-Watcher** wordt weergegeven in de zoekresultaten, selecteer deze.
3. Onder **bewaking**, selecteer **verbinding monitor (Preview)**. Functies in de preview-versie hoeft niet dezelfde mate van betrouwbaarheid of beschikbaarheid in regio's als functies in het algemeen release.
4. Selecteer **+ toevoegen**.
5. Typ of Selecteer de juiste informatie voor de verbinding die u wilt bewaken, en selecteer vervolgens **toevoegen**. In dit voorbeeld wordt een verbinding tussen de *myVmSource* en *myVmDestination* virtuele machines via poort 80 wordt bewaakt.
    
    |  Instelling                                 |  Waarde               |
    |  -------------------------------------   |  ------------------- |
    |  Naam                                    |  myConnectionMonitor |
    |  Virtuele bronmachine                  |  myVmSource          |
    |  Bronpoort                             |                      |
    |  Bestemming, selecteer een virtuele machine   |  myVmDestination     |
    |  Doelpoort                        |  80                  |

6. Controle begint. Monitor voor verbinding tests elke 60 seconden.
7. Monitor voor verbinding toont de gemiddelde round trip time en percentage tests die niet voldoen aan. U kunt gegevens van de monitor in een raster of een grafiek weergeven.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over het automatiseren van pakket opnamen met waarschuwingen van de virtuele machine door [maken van een waarschuwing gegeven pakketopname](network-watcher-alert-triggered-packet-capture.md).

- Bepalen of bepaalde verkeer is toegestaan in of buiten uw virtuele machine met behulp van [IP-stroom controleren](network-watcher-check-ip-flow-verify-portal.md).