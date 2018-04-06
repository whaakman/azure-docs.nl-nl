---
title: Netwerkverbindingen met de Azure-netwerk-Watcher - Azure-portal bewaken | Microsoft Docs
description: Informatie over het bewaken van netwerkverbinding met de netwerk-Watcher van Azure met Azure portal.
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
ms.date: 02/16/2018
ms.author: jdial
ms.openlocfilehash: b0eb10d373f47191933eb1c3c22b779e9e8d6685
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/05/2018
---
# <a name="monitor-network-connections-with-azure-network-watcher-using-the-azure-portal"></a>Netwerkverbindingen met de netwerk-Watcher van Azure met Azure portal controleren

Informatie over het bewaken van de verbinding gebruiken voor het bewaken van netwerkverbinding tussen een Azure-virtuele Machine (VM) en een IP-adres. Monitor voor de verbinding biedt bewaking tussen de bron en doel-IP-adres en poort. Monitor verbinding maakt scenario's, zoals de bewaking van de verbinding van een virtuele machine in een virtueel netwerk met een virtuele machine waarop SQL server in het hetzelfde of een ander virtueel netwerk, via poort 1433. Monitor verbinding biedt u de latentie van de verbinding als een Azure-Monitor-meting vastgelegd elke 60 seconden. Ook vindt u een topologie hop door hop en problemen met de configuratie die invloed hebben op uw verbinding identificeert.

## <a name="prerequisites"></a>Vereisten

U moet voldoen aan de volgende vereisten voordat u de stappen in dit artikel uitvoert:

* Een exemplaar van netwerk-Watcher in de regio die u wilt bewaken van een verbinding voor. Als u dit niet al hebt, kunt u een door de stappen in [maken van een exemplaar van Azure-netwerk-Watcher](network-watcher-create.md).
* Een virtuele machine voor het bewaken van. Zie voor meer informatie over het maken van een virtuele machine, maak een [Windows](../virtual-machines/windows/quick-create-portal.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json) of [Linux](../virtual-machines/linux/quick-create-portal.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json) VM.
* Hebben de `AzureNetworkWatcherExtension` geïnstalleerd in de virtuele machine die u wilt bewaken van een verbinding van. Zie voor informatie over het installeren van de extensie in een Windows-VM [extensie voor het virtuele machine voor Windows Azure-netwerk-Watcher Agent](../virtual-machines/windows/extensions-nwa.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json) en installeren van de extensie in een Linux-VM-Zie [Azure-netwerk-Watcher Agent de extensie van de virtuele machine voor Linux](../virtual-machines/linux/extensions-nwa.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json). De extensie is niet vereist op de doeleindpunt dat u wilt bewaken.

## <a name="sign-in-to-azure"></a>Aanmelden bij Azure 

Meld u aan bij [Azure Portal](http://portal.azure.com).

## <a name="create-a-connection-monitor"></a>Een monitor verbinding maken

De volgende stappen schakelt verbinding naar een bestemming VM poorten 80 en 1433 bewaking:

1. Selecteer aan de linkerkant van de portal **alle services**.
2. Typ *netwerk-watcher* in de **Filter** vak. Wanneer **netwerk-Watcher** wordt weergegeven in de zoekresultaten, selecteer deze.
3. Onder **bewaking**, selecteer **verbinding monitor**.
4. Selecteer **+ toevoegen**.
5. Typ of Selecteer de gegevens voor de verbinding die u wilt bewaken, en selecteer vervolgens **toevoegen**. In het voorbeeld weergegeven in de volgende afbeelding wordt de verbinding bewaakt is afkomstig uit de *MultiTierApp0* VM naar de *Database0* VM via poort 80:

    ![Beeldscherm verbinding toevoegen](./media/connection-monitor/add-connection-monitor.png)

    Controle begint. Monitor voor verbinding tests elke 60 seconden.
6. Voer stap 5 opnieuw opgeven dezelfde bron- en virtuele machines en de volgende waarden:
    
    |Instelling  |Waarde          |
    |---------|---------      |
    |Naam     | AppToDB(1433) |
    |Poort     | 1433          |

## <a name="view-connection-monitoring"></a>Weergave-verbinding controleren

1. Voltooi de stappen 1-3 in [maken van een monitor verbinding](#create-a-connection-monitor) om bewaking van de verbinding weer te geven.
2. De volgende afbeelding ziet u details voor de *AppToDB(80)* verbinding. De **Status** bereikbaar is. De **grafiek weergeven** toont de **gemiddelde Round Trip Time** en **-tests is mislukt %**. De grafiek bevat hop door hop informatie, en wordt aangegeven dat geen problemen invloed zijn op de bestemming bereikbaarheid.

    ![Grafiekweergave](./media/connection-monitor/view-graph.png)

3. Weergeven van de *AppToDB(1433)* verbinding wordt weergegeven in de volgende afbeelding ziet u dat voor dezelfde bron en doel-virtuele machines, de status is niet bereikbaar via poort 1433. De **rasterweergave** in dit scenario biedt de informatie hop door hop en het probleem van invloed op de bereikbaarheid. In dit geval blokkeert een NSG-regel al het verkeer op poort 1433 op de tweede hop.

    ![Rasterweergave](./media/connection-monitor/view-grid.png)

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over het automatiseren van pakket opnamen met VM-waarschuwingen per [maken van een waarschuwing gegeven pakketopname](network-watcher-alert-triggered-packet-capture.md).
- Bepalen of bepaalde verkeer is toegestaan in of buiten uw virtuele machine met behulp van [IP-stroom controleren](network-watcher-check-ip-flow-verify-portal.md).