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
ms.openlocfilehash: c7d98350dc8f66ebd4097f22b44dcbbe2653b25d
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/23/2018
---
# <a name="monitor-network-connections-with-azure-network-watcher-using-the-azure-portal"></a>Netwerkverbindingen met de netwerk-Watcher van Azure met Azure portal controleren

Informatie over het bewaken van de verbinding gebruiken voor het bewaken van netwerkverbinding tussen Azure een virtuele machine en een IP-adres. Monitor voor de verbinding biedt bewaking op het verbindingsniveau van een. Een verbinding wordt gedefinieerd als een combinatie van de bron en doel-IP-adres en poort. Monitor verbinding maakt scenario's, zoals de bewaking van de verbinding tussen een virtuele machine in een virtueel netwerk en een virtuele machine met SQL server in het hetzelfde of een ander virtueel netwerk, via poort 1433. Monitor verbinding biedt u de latentie van de verbinding als een Azure-Monitor-meting vastgelegd elke 60 seconden. Ook vindt u een topologie hop door hop en problemen met de configuratie die invloed hebben op uw verbinding identificeert.


## <a name="prerequisites"></a>Vereisten

U moet voldoen aan de volgende vereisten voordat u de stappen in dit artikel uitvoert:

* Een exemplaar van netwerk-Watcher in de regio die u wilt bewaken van een verbinding voor. Als u dit niet al hebt, kunt u een door de stappen in [maken van een exemplaar van Azure-netwerk-Watcher](network-watcher-create.md).
* Een virtuele machine voor het bewaken van.
* Hebben de `AzureNetworkWatcherExtension` geïnstalleerd in de virtuele machine die u wilt bewaken van een verbinding van. Zie voor informatie over het installeren van de uitbreiding in virtuele Windows-machine [extensie voor het virtuele machine voor Windows Azure-netwerk-Watcher Agent](../virtual-machines/windows/extensions-nwa.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json) en installeren van de extensie in een virtuele machine Linux Zie [Azure-netwerk-Watcher-Agent de extensie van de virtuele machine voor Linux](../virtual-machines/linux/extensions-nwa.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json).

## <a name="sign-in-to-azure"></a>Aanmelden bij Azure 

Meld u aan bij [Azure Portal](http://portal.azure.com).

## <a name="create-a-connection-monitor"></a>Een monitor verbinding maken

De volgende stappen schakelt verbinding controleren met een doel-virtuele machine via de poorten 80 en 1433:

1. Selecteer aan de linkerkant van de portal **meer services**.
2. Typ *netwerk-watcher*. Wanneer **netwerk-Watcher** wordt weergegeven in de zoekresultaten, selecteer deze.
3. Onder **bewaking**, selecteer **verbinding monitor (Preview)**. Functies in de preview-versie hoeft niet dezelfde mate van betrouwbaarheid of beschikbaarheid in regio's als functies in het algemeen release.
4. Selecteer **+ toevoegen**.
5. Typ of Selecteer de gegevens voor de verbinding die u wilt bewaken, en selecteer vervolgens **toevoegen**. In het voorbeeld in de volgende afbeelding, de bewaakt verbinding is tussen de *MultiTierApp0* en *Database0* virtuele machines:

    ![Beeldscherm verbinding toevoegen](./media/connection-monitor/add-connection-monitor.png)

    Controle begint. Monitor voor verbinding tests elke 60 seconden.

## <a name="view-connection-monitoring"></a>Weergave-verbinding controleren

1. Voltooi de stappen 1-3 in [maken van een monitor verbinding](#create-a-connection-monitor) om bewaking van de verbinding weer te geven.
2. De volgende afbeelding ziet details voor de verbinding AppToDB(80). De **Status** bereikbaar is. De **grafiek weergeven** toont de **gemiddelde Round Trip Time** en **-tests is mislukt %**. De grafiek bevat hop door hop informatie, en wordt aangegeven dat geen problemen invloed zijn op de bestemming bereikbaarheid.

    ![Monitor voor verbinding weergeven](./media/connection-monitor/view-connection-monitor.png)

3. Weergeven van de *AppToDB(1433)* monitor, weergegeven in de volgende afbeelding ziet u dat voor de dezelfde bron en doel-virtuele machines, de status is niet bereikbaar via poort 1433. De **rasterweergave** in dit scenario biedt de informatie hop door hop en het probleem van invloed op de bereikbaarheid. In dit geval blokkeert een NSG-regel al het verkeer op poort 1433 op de tweede hop.

    ![Monitor voor verbinding weergeven](./media/connection-monitor/view-connection-monitor-2.png)

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over het automatiseren van pakket opnamen met waarschuwingen van de virtuele machine door [maken van een waarschuwing gegeven pakketopname](network-watcher-alert-triggered-packet-capture.md).
- Bepalen of bepaalde verkeer is toegestaan in of buiten uw virtuele machine met behulp van [IP-stroom controleren](network-watcher-check-ip-flow-verify-portal.md).