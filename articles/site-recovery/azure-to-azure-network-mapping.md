---
title: Toewijzen van virtuele netwerken tussen twee Azure-regio's in Azure Site Recovery | Microsoft Docs
description: Azure Site Recovery coördineert de replicatie, failover en herstel van virtuele machines en fysieke servers. Meer informatie over failover naar Azure of naar een secundair datacenter.
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
ms.date: 07/06/2018
ms.author: manayar
ms.openlocfilehash: 7b7f9c079a1fc9d74fed4cc4d94d37f336ca5dc7
ms.sourcegitcommit: a06c4177068aafc8387ddcd54e3071099faf659d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/09/2018
ms.locfileid: "37916737"
---
# <a name="map-virtual-networks-in-different-azure-regions"></a>Virtuele netwerken in verschillende Azure-regio's toewijzen


In dit artikel wordt beschreven hoe u twee instanties van Azure Virtual Network die zich in verschillende Azure-regio's met elkaar worden toegewezen. Netwerktoewijzing zorgt ervoor dat wanneer een gerepliceerde virtuele machine wordt gemaakt in de doel-Azure-regio, de virtuele machine wordt ook gemaakt op het virtuele netwerk dat toegewezen aan het virtuele netwerk van de virtuele bronmachine.  

## <a name="prerequisites"></a>Vereisten
Zorg ervoor dat u hebt gemaakt voordat u netwerken toewijzen, een [virtueel Azure-netwerk](../virtual-network/virtual-networks-overview.md) in de regio van de gegevensbron en de doel-Azure-regio.

## <a name="map-virtual-networks"></a>Virtuele netwerken toewijzen

Als u wilt toewijzen van een Azure-netwerk dat zich bevindt in een Azure-regio (Bronnetwerk) naar een virtueel netwerk dat in een andere regio (doelnetwerk) voor virtuele machines van Azure bevindt zich, gaat u naar **Site Recovery-infrastructuur**  >  **Toewijzing netwerk**. Maak een netwerktoewijzing.

![Venster toewijzingen van netwerk - maken van een netwerktoewijzing](./media/site-recovery-network-mapping-azure-to-azure/network-mapping1.png)


In het volgende voorbeeld wordt wordt de virtuele machine uitgevoerd in de regio Oost-Azië. De virtuele machine wordt gerepliceerd naar de regio Zuidoost-Azië.

Voor het maken van een netwerktoewijzing van de regio Oost-Azië naar de regio Zuidoost-Azië, selecteer de locatie van het bron-netwerk en de locatie van het doelnetwerk. Selecteer vervolgens **OK**.

![Toevoegen van venster van de toewijzing van netwerk - bron- en doellocaties voor het Bronnetwerk selecteren](./media/site-recovery-network-mapping-azure-to-azure/network-mapping2.png)


Herhaal de voorgaande procedure voor het maken van een netwerktoewijzing van de regio Zuidoost-Azië naar de regio Oost-Azië.

![Toevoegen van deelvenster van de toewijzing van netwerk - bron- en doellocaties voor het doelnetwerk selecteren](./media/site-recovery-network-mapping-azure-to-azure/network-mapping3.png)


## <a name="map-a-network-when-you-enable-replication"></a>Een netwerk worden toegewezen wanneer u replicatie inschakelt

Wanneer u een virtuele machine van de ene Azure-regio naar een andere regio voor de eerste keer repliceren als er geen netwerktoewijzing bestaat, kunt u het doelnetwerk instellen bij het instellen van replicatie. Op basis van deze instelling, maakt Azure Site Recovery-netwerkkoppelingen van de bronregio naar de doelregio en van de doelregio naar de bronregio.   

![Deelvenster instellingen configureren - de doellocatie kiezen](./media/site-recovery-network-mapping-azure-to-azure/network-mapping4.png)

Site Recovery maakt standaard een netwerk in de doelregio die identiek is aan het Bronnetwerk. Site Recovery maakt u een netwerk door toe te voegen **-asr** als achtervoegsel aan de naam van het Bronnetwerk. Als u een netwerk dat al is gemaakt, selecteert u **aanpassen**.

![Deelvenster instellingen voor doel - Set doel Resourcegroepnaam en de naam van de doel-virtuele netwerk aanpassen](./media/site-recovery-network-mapping-azure-to-azure/network-mapping5.png)

Als de netwerktoewijzing al is gebeurd, kunt u het virtuele netwerk niet wijzigen wanneer u replicatie inschakelt. In dit geval, als u wilt wijzigen van het virtuele netwerk, de bestaande netwerktoewijzing wijzigt.  

![Doel aanpassen instellingenvenster - naam van de resourcegroep doel instellen](./media/site-recovery-network-mapping-azure-to-azure/network-mapping6.png)

![Netwerk toewijzing deelvenster wijzigen - wijzigen van een bestaande virtuele-netwerknaam van doel](./media/site-recovery-network-mapping-azure-to-azure/modify-network-mapping.png)

> [!IMPORTANT]
> Als u een netwerktoewijzing van de A-regio naar regio B wijzigt, zorgt u ervoor dat u ook de netwerktoewijzing van de B-regio naar regio A. wijzigen
>
>


## <a name="subnet-selection"></a>Subnet is geselecteerd
Het subnet van de virtuele doelmachine wordt geselecteerd op basis van de naam van het subnet van de virtuele bronmachine. Als een subnet met dezelfde naam als de virtuele bronmachine beschikbaar in het doelnetwerk is, wordt dat subnet is ingesteld voor de virtuele doelmachine. Als een subnet met dezelfde naam in het doelnetwerk bestaat, wordt het alfabetisch eerste subnet is ingesteld als het doelsubnet.

Als u wilt wijzigen van het subnet, gaat u naar de **berekening en netwerk** instellingen voor de virtuele machine.

![Reken- en compute-eigenschappenvenster](./media/site-recovery-network-mapping-azure-to-azure/modify-subnet.png)


## <a name="ip-address"></a>IP-adres

Het IP-adres voor elke netwerkinterface van de virtuele doelmachine is ingesteld, zoals beschreven in de volgende secties.

### <a name="dhcp"></a>DHCP
Als de netwerkinterface van de virtuele bronmachine DHCP gebruikt, wordt de netwerkinterface van de virtuele doelmachine ook instellen op het gebruik van DHCP.

### <a name="static-ip-address"></a>Statisch IP-adres
Als de netwerkinterface van de virtuele machine gebruikmaakt van een statisch IP-adres, wordt de netwerkinterface van de virtuele doelmachine ook ingesteld op statisch IP-adres. De volgende secties wordt beschreven hoe een statisch IP-adres is ingesteld.

#### <a name="same-address-space"></a>Dezelfde-adresruimte

Als het Bronsubnet en het doelsubnet hebt dezelfde-adresruimte, is het IP-adres van de netwerkinterface van de virtuele bronmachine ingesteld als het doel-IP-adres. Als hetzelfde IP-adres niet beschikbaar is, wordt het volgende beschikbare IP-adres ingesteld als het doel-IP-adres.

#### <a name="different-address-spaces"></a>Verschillende adresruimten

Als het Bronsubnet en het doelsubnet hebt verschillende adresruimten, wordt de eerstvolgende beschikbare IP-adres in het doelsubnet is ingesteld als het doel-IP-adres.

Als u wilt wijzigen van de doel-IP-adres op elke netwerkinterface, gaat u naar de **berekening en netwerk** instellingen voor de virtuele machine.

## <a name="next-steps"></a>Volgende stappen

* Beoordeling [richtlijnen voor het repliceren van virtuele machines van Azure networking](site-recovery-azure-to-azure-networking-guidance.md).
