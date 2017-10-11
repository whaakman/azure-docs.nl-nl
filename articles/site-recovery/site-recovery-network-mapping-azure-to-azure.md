---
title: De netwerkkoppeling tussen twee Azure-regio's in Azure Site Recovery | Microsoft Docs
description: "Azure Site Recovery coördineert de replicatie, failovers en herstel van virtuele machines en fysieke servers. Meer informatie over failover naar Azure of een secundair datacenter."
services: site-recovery
documentationcenter: 
author: prateek9us
manager: gauravd
editor: 
ms.assetid: 44813a48-c680-4581-a92e-cecc57cc3b1e
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 08/11/2017
ms.author: pratshar
ms.openlocfilehash: 9d6a806ec533259797080fbfee2c38f918ebd8a2
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2017
---
# <a name="network-mapping-between-two-azure-regions"></a>De netwerkkoppeling tussen twee Azure-regio 's


In dit artikel wordt beschreven hoe virtuele Azure-netwerken van twee Azure-regio's met elkaar worden toegewezen. Netwerktoewijzing zorgt ervoor dat wanneer gerepliceerde virtuele machine wordt gemaakt in de doel-Azure-regio, wordt gemaakt op het virtuele netwerk dat is toegewezen aan het virtuele netwerk van de virtuele bronmachine.  

## <a name="prerequisites"></a>Vereisten
Voordat u toewijzen netwerken zeker dat u hebt gemaakt kunt [virtuele netwerken van Azure](../virtual-network/virtual-networks-overview.md) in zowel bron en doel van de Azure-regio's.

## <a name="map-networks"></a>Netwerken toewijzen

Als u wilt een Azure-netwerk in een Azure-regio worden toegewezen aan een ander virtueel netwerk in een andere regio, gaat u naar Site Recovery-infrastructuur -> netwerktoewijzing (voor Azure Virtual Machines) en maak de netwerktoewijzing van een.

![Netwerktoewijzing](./media/site-recovery-network-mapping-azure-to-azure/network-mapping1.png)


In het onderstaande voorbeeld is mijn virtuele machine wordt uitgevoerd in de regio Oost-Azië, en wordt gerepliceerd naar Zuidoost-Azië.

Selecteer de bron en doel-netwerk en klik vervolgens op OK als u wilt maken een netwerktoewijzing tussen Oost-Azië Zuidoost-Azië.

![Netwerktoewijzing](./media/site-recovery-network-mapping-azure-to-azure/network-mapping2.png)


Doe hetzelfde voor een netwerktoewijzing maken tussen Zuidoost-Azië Oost-Azië.  
![Netwerktoewijzing](./media/site-recovery-network-mapping-azure-to-azure/network-mapping3.png)


## <a name="mapping-network-when-enabling-replication"></a>Netwerkverbinding maken bij het inschakelen van replicatie

Als de netwerktoewijzing is niet uitgevoerd wanneer u een virtuele machine voor de eerste keer formulier een Azure-regio worden gerepliceerd naar een andere, kunt u doelnetwerk als onderdeel van hetzelfde proces. Uit de regio van bron naar doelregio en doelregio bron regio op basis van deze selectie, maakt site Recovery-netwerkkoppelingen.   

![Netwerktoewijzing](./media/site-recovery-network-mapping-azure-to-azure/network-mapping4.png)

Standaard maakt Site Recovery een netwerk in de doelregio die identiek is aan het Bronnetwerk en toe te voegen '-asr' als achtervoegsel aan de naam van het bron-netwerk. U kunt een bestaande netwerk door te klikken op aanpassen.

![Netwerktoewijzing](./media/site-recovery-network-mapping-azure-to-azure/network-mapping5.png)


Als de netwerktoewijzing al wordt uitgevoerd, kunt u het virtuele netwerk niet wijzigen tijdens het inschakelen van replicatie. Als u wilt wijzigen, bestaande netwerktoewijzing te wijzigen.  

![Netwerktoewijzing](./media/site-recovery-network-mapping-azure-to-azure/network-mapping6.png)

![Netwerktoewijzing](./media/site-recovery-network-mapping-azure-to-azure/modify-network-mapping.png)

> [!IMPORTANT]
> Als u een netwerktoewijzing van gebied-1 tot regio 2 wijzigt, zorg er dan voor dat u de netwerktoewijzing van regio 2 op regio 1 ook wijzigen.
>
>


## <a name="subnet-selection"></a>Selectie van subnet
Subnet van de virtuele doelmachine is geselecteerd op basis van de naam van het subnet van de virtuele bronmachine. Als er een subnet met dezelfde naam als die van de virtuele bronmachine beschikbaar zijn in het doelnetwerk, is voor de virtuele doelmachine die gekozen. Als er geen subnet met dezelfde naam in het doelnetwerk vervolgens alfabetisch is eerste subnet gekozen als het doelsubnet. U kunt dit subnet wijzigen door te gaan naar de berekening en netwerk-instellingen van de virtuele machine.

![Wijzigen van Subnet](./media/site-recovery-network-mapping-azure-to-azure/modify-subnet.png)


## <a name="ip-address"></a>IP-adres

IP-adres voor elk van de netwerkinterface van de virtuele doelmachine gekozen als volgt:

### <a name="dhcp"></a>DHCP
Als de netwerkinterface van de virtuele bronmachine DHCP gebruikt, wordt de netwerkinterface van de virtuele doelmachine ook als DHCP ingesteld.

### <a name="static-ip"></a>Vaste IP-adres
Als de netwerkinterface van de virtuele bronmachine van vaste IP-adres gebruikmaakt, wordt de netwerkinterface van de virtuele doelmachine ook ingesteld op statische IP gebruikt. Vaste IP-adres is als volgt kiezen:

#### <a name="same-address-space"></a>Dezelfde adresruimte

Als het subnet van de bron en het doelsubnet hebt dezelfde adresruimte, wordt IP-adres van doel hetzelfde als het IP-adres van de netwerkinterface van de virtuele bronmachine instellen. Als dezelfde IP niet beschikbaar is, is enkele beschikbare IP ingesteld als het doel-IP.

#### <a name="different-address-space"></a>Andere adresruimte

Als het subnet van de bron en het doelsubnet hebt verschillende adresruimte, wordt IP-adres doel ingesteld als een beschikbare IP in het doelsubnet.

U kunt het doel-IP op elke netwerkinterface wijzigen door te gaan naar berekenings- en instellingen van de virtuele machine.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [richtlijnen voor het repliceren van virtuele Azure-machines networking](site-recovery-azure-to-azure-networking-guidance.md).
