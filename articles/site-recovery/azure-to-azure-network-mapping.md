---
title: Toewijzen van virtuele netwerken tussen twee Azure-regio's in Azure Site Recovery | Microsoft Docs
description: Azure Site Recovery coördineert de replicatie, failovers en herstel van virtuele machines en fysieke servers. Meer informatie over failover naar Azure of naar een secundair datacenter.
services: site-recovery
documentationcenter: ''
author: mayanknayar
manager: rochakm
editor: ''
ms.assetid: 44813a48-c680-4581-a92e-cecc57cc3b1e
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 02/27/2018
ms.author: manayar
ms.openlocfilehash: 8f347827c640729112e2e8f4c11288b6bcb176ea
ms.sourcegitcommit: c765cbd9c379ed00f1e2394374efa8e1915321b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/28/2018
---
# <a name="map-virtual-networks-in-different-azure-regions"></a>Virtuele netwerken in verschillende Azure-regio's toewijzen


In dit artikel wordt beschreven hoe twee exemplaren van Azure Virtual Network zich in verschillende Azure-regio's met elkaar worden toegewezen. Netwerktoewijzing zorgt ervoor dat wanneer een gerepliceerde virtuele machine wordt gemaakt in de doel-Azure-regio, de virtuele machine wordt ook gemaakt op het virtuele netwerk dat toegewezen aan het virtuele netwerk van de virtuele bronmachine.  

## <a name="prerequisites"></a>Vereisten
Zorg ervoor dat u hebt gemaakt voordat u netwerken toewijst, een [virtuele Azure-netwerk](../virtual-network/virtual-networks-overview.md) in zowel de bron-regio als de doel-Azure-regio.

## <a name="map-virtual-networks"></a>Virtuele netwerken toewijzen

Als u wilt toewijzen in een Azure-netwerk dat zich bevindt in een Azure-regio (Bronnetwerk) met een virtueel netwerk dat in een andere regio (doelnetwerk) voor virtuele machines in Azure bevindt zich, gaat u naar **Site Recovery-infrastructuur**  >  **Toewijzing netwerk**. Maak de netwerktoewijzing van een.

![Toewijzingen venster netwerk - Maak de netwerktoewijzing van een](./media/site-recovery-network-mapping-azure-to-azure/network-mapping1.png)


In het volgende voorbeeld wordt wordt de virtuele machine uitgevoerd in de regio Oost-Azië. De virtuele machine wordt gerepliceerd naar de regio Zuidoost-Azië.

Als u wilt maken een netwerktoewijzing van de Oost-Azië regio voor de regio Zuidoost-Azië, selecteer de locatie van het bron-netwerk en de locatie van het doelnetwerk. Selecteer **OK**.

![Toevoegen van venster van de toewijzing van netwerk - bron- en doellocaties voor het Bronnetwerk selecteren](./media/site-recovery-network-mapping-azure-to-azure/network-mapping2.png)


Herhaal de voorgaande procedure voor het maken van een netwerktoewijzing uit Zuidoost-Azië-gebied voor de regio Oost-Azië.

![Toevoegen van netwerk toewijzing deelvenster - bron- en doellocaties voor het doelnetwerk selecteren](./media/site-recovery-network-mapping-azure-to-azure/network-mapping3.png)


## <a name="map-a-network-when-you-enable-replication"></a>Toewijzen van een netwerk wanneer u replicatie inschakelen

Wanneer u een virtuele machine van een Azure-regio naar een andere regio voor de eerste keer repliceren als er geen netwerktoewijzing bestaat, kunt u het doelnetwerk instellen bij het instellen van replicatie. Op basis van deze instelling, maakt Azure Site Recovery-netwerkkoppelingen van de bron-regio voor de doelregio en van de doelregio voor de bron-regio.   

![Deelvenster instellingen configureren - de doellocatie kiezen](./media/site-recovery-network-mapping-azure-to-azure/network-mapping4.png)

Standaard maakt Site Recovery een netwerk in de doelregio die identiek is aan de bron-netwerk. Maakt site Recovery een netwerk door toe te voegen **-asr** als achtervoegsel aan de naam van het bron-netwerk. Als u een netwerk dat al is gemaakt, selecteert u **aanpassen**.

![Deelvenster instellingen voor target - Set resource doelgroepnaam en doel-virtuele-netwerknaam aanpassen](./media/site-recovery-network-mapping-azure-to-azure/network-mapping5.png)

Als u netwerktoewijzing al heeft plaatsgevonden, kunt u het virtuele netwerk niet wijzigen wanneer u replicatie inschakelt. In dit geval om te wijzigen van het virtuele netwerk, wijzig de bestaande netwerktoewijzing.  

![Doel aanpassen instellingen deelvenster - naam van de resourcegroep doel instellen](./media/site-recovery-network-mapping-azure-to-azure/network-mapping6.png)

![Netwerk toewijzing deelvenster wijzigen - wijzigen van een bestaande virtuele-netwerknaam van doel](./media/site-recovery-network-mapping-azure-to-azure/modify-network-mapping.png)

> [!IMPORTANT]
> Als u een netwerktoewijzing van regio A naar B regio wijzigt, zorg ervoor dat u ook de netwerktoewijzing van regio B regio A. wijzigen
>
>


## <a name="subnet-selection"></a>Selectie van subnet
Het subnet van de virtuele doelmachine is geselecteerd op basis van de naam van het subnet van de virtuele bronmachine. Als een subnet met dezelfde naam als de virtuele bronmachine beschikbaar in het doelnetwerk is, wordt dat subnet ingesteld voor de virtuele doelmachine. Als een subnet met dezelfde naam niet in het doelnetwerk bestaat, wordt het alfabetisch eerste subnet ingesteld als het doelsubnet.

Voor het wijzigen van het subnet, gaat u naar de **berekening en netwerk** instellingen voor de virtuele machine.

![Reken- en het eigenschappenvenster berekenen](./media/site-recovery-network-mapping-azure-to-azure/modify-subnet.png)


## <a name="ip-address"></a>IP-adres

Het IP-adres voor elke netwerkinterface van de virtuele doelmachine is ingesteld, zoals beschreven in de volgende secties.

### <a name="dhcp"></a>DHCP
Als de netwerkinterface van de virtuele bronmachine DHCP gebruikt, wordt de netwerkinterface van de virtuele doelmachine ook instellen op het gebruik van DHCP.

### <a name="static-ip-address"></a>Statisch IP-adres
Als de netwerkinterface van de virtuele bronmachine gebruikmaakt van een statisch IP-adres, wordt de netwerkinterface van de virtuele doelmachine ook instellen op een statisch IP-adres gebruiken. De volgende secties wordt beschreven hoe u een statisch IP-adres is ingesteld.

#### <a name="same-address-space"></a>Dezelfde adresruimte

Als het subnet van de bron en het doelsubnet hebt dezelfde adresruimte, wordt het IP-adres van de netwerkinterface van de virtuele bronmachine is ingesteld als het doel-IP-adres. Als hetzelfde IP-adres niet beschikbaar is, wordt het volgende beschikbare IP-adres is ingesteld als het doel-IP-adres.

#### <a name="different-address-spaces"></a>Andere adresruimten

Als het subnet van de bron en het doelsubnet hebt verschillende adresruimten, is het volgende beschikbare IP-adres in het doelsubnet ingesteld als het doel-IP-adres.

Voor het wijzigen van het doel-IP op elke netwerkinterface, gaat u naar de **berekening en netwerk** instellingen voor de virtuele machine.

## <a name="next-steps"></a>Volgende stappen

* Bekijk [richtlijnen voor het repliceren van virtuele machines van Azure toegang](site-recovery-azure-to-azure-networking-guidance.md).
