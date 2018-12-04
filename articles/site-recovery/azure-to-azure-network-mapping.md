---
title: Toewijzen van virtuele netwerken tussen twee Azure-regio's in Azure Site Recovery | Microsoft Docs
description: Azure Site Recovery coördineert de replicatie, failover en herstel van virtuele machines en fysieke servers. Meer informatie over failover naar Azure of naar een secundair datacenter.
author: mayurigupta13
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 11/27/2018
ms.author: mayg
ms.openlocfilehash: 6140687d583534d21ee50652811c2fd1624a5cf5
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52840449"
---
# <a name="set-up-network-mapping-and-ip-addressing-for-vnets"></a>Instellen van netwerktoewijzing en IP-adressering voor vnet 's

Dit artikel wordt beschreven hoe u kunt twee exemplaren van virtuele Azure-netwerken (VNets) zich in verschillende Azure-regio's toewijzen, en over het instellen van het IP-adressering tussen netwerken. Netwerktoewijzing zorgt ervoor dat een gerepliceerde virtuele machine is gemaakt in de doel-Azure-regio in het VNet dat toegewezen aan het VNet van de bron-VM wordt gemaakt.

## <a name="prerequisites"></a>Vereisten

Voordat u netwerken toewijzen, moet u beschikken over [Azure VNets](../virtual-network/virtual-networks-overview.md) in de bron en doel-Azure-regio's. 

## <a name="set-up-network-mapping"></a>Netwerktoewijzing instellen

Netwerken als volgt toewijzen:

1. In **Site Recovery-infrastructuur**, klikt u op **+ netwerktoewijzing**.

    ![ Maken van een netwerktoewijzing](./media/site-recovery-network-mapping-azure-to-azure/network-mapping1.png)

3. In **netwerktoewijzing toevoegen**, selecteert u de bron en doel locaties. In ons voorbeeld de bron virtuele machine wordt uitgevoerd in de regio Oost-Azië, en worden gerepliceerd naar de regio Zuidoost-Azië.

    ![Bron- en doel selecteren ](./media/site-recovery-network-mapping-azure-to-azure/network-mapping2.png)
3. Maak nu een netwerktoewijzing in de tegengestelde directory. In ons voorbeeld, de bron worden nu Zuidoost-Azië en het doel is Oost-Azië.

    ![Toevoegen van deelvenster van de toewijzing van netwerk - bron- en doellocaties voor het doelnetwerk selecteren](./media/site-recovery-network-mapping-azure-to-azure/network-mapping3.png)


## <a name="map-networks-when-you-enable-replication"></a>Netwerken toewijzen wanneer u replicatie inschakelt

Als u dit nog niet hebt voorbereid netwerktoewijzing voordat u herstel na noodgevallen voor Azure-VM's configureert, kunt u een doel netwerk wanneer u [instellen en inschakelen van replicatie](azure-to-azure-how-to-enable-replication.md). Wanneer u dit doen het volgende gebeurt:

- Op basis van het doel dat u selecteert, maakt Site Recovery automatisch Netwerktoewijzingen van de bron naar doelregio, en het doel naar de bronregio.
- Site Recovery maakt standaard een netwerk in de doelregio die identiek is aan het Bronnetwerk. Site Recovery voegt **-asr** als achtervoegsel aan de naam van het Bronnetwerk. U kunt het doelnetwerk kunt aanpassen.
- Als de netwerktoewijzing al is gebeurd, kunt u het virtuele netwerk niet wijzigen wanneer u replicatie inschakelt. Als u wilt wijzigen van het virtuele doelnetwerk, moet u de bestaande netwerktoewijzing wijzigt.
- Als u een netwerktoewijzing van de A-regio naar regio B wijzigt, zorgt u ervoor dat u ook de netwerktoewijzing van de B-regio naar regio A. wijzigen]

## <a name="specify-a-subnet"></a>Geef een subnet

Het subnet van het doel-VM is geselecteerd, is afhankelijk van de naam van het subnet van de bron-VM.

- Als een subnet met dezelfde naam als de bron-VM-subnet in het netwerk beschikbaar is, wordt dat subnet is ingesteld voor de doel-VM.
- Als een subnet met dezelfde naam in het doelnetwerk bestaat, is het eerste subnet in alfabetische volgorde ingesteld als het doelsubnet.
- U kunt de in de **berekening en netwerk** instellingen voor de virtuele machine.

    ![Reken- en compute-eigenschappenvenster](./media/site-recovery-network-mapping-azure-to-azure/modify-subnet.png)


## <a name="set-up-ip-addressing-for-target-vms"></a>IP-adressering instellen voor doel-VM 's

Het IP-adres voor elke NIC op een virtuele doelmachine wordt als volgt geconfigureerd:

- **DHCP**: als de NIC van de bron-VM gebruikmaakt van DHCP, de NIC van de doel-VM ook is ingesteld op het gebruik van DHCP.
- **Statisch IP-adres**: als de NIC van de bron-VM gebruikmaakt van vaste IP-adressen, de doel-VM NIC ook een statisch IP-adres wordt gebruikt.


## <a name="ip-address-assignment-during-failover"></a>Toewijzing van IP-adres tijdens de failover

**Bron- en subnetten** | **Details**
--- | ---
Dezelfde-adresruimte | IP-adres van de VM NIC-bron is ingesteld als de doel-VM NIC IP-adres.<br/><br/> Als het adres beschikbaar is, wordt het volgende beschikbare IP-adres is ingesteld als het doel.
Andere adresruimte<br/><br/> Het eerstvolgende beschikbare IP-adres in het doelsubnet is ingesteld als de doel-VM NIC-adres.



## <a name="ip-address-assignment-during-test-failover"></a>Toewijzing van IP-adres tijdens de testfailover

**Doelnetwerk** | **Details**
--- | ---
Doelnetwerk is failover VNet | -Doel IP-adres wordt statisch, maar niet hetzelfde IP-adres als gereserveerd voor failover.<br/><br/>  -Het toegewezen adres is het eerstvolgende beschikbare adres vanaf het einde van het subnetbereik.<br/><br/> Bijvoorbeeld: als de bron-IP-adres 10.0.0.19 is en failovernetwerk netwerkadresbereik 10.0.0.0/24 wordt gebruikt, wordt het volgende IP-adres toegewezen aan de doel-VM 10.0.0.254 beschikbaar is.
Doelnetwerk is niet de failover VNet | -Doel IP-adres worden statische met hetzelfde IP-adres gereserveerd voor failover.<br/><br/>  -Als hetzelfde IP-adres al toegewezen is, is het volgende object beschikbaar op de eeach van het subnetbereik met het IP-adres.<br/><br/> Bijvoorbeeld: als het statische IP-adres van bron 10.0.0.19 is en failover zich op een netwerk dat niet het failovernetwerk, met het netwerkadresbereik 10.0.0.0/24, en vervolgens het statische IP-adres van doel 10.0.0.0.19 als deze beschikbaar zijn en anders het 10.0.0.254 beschikbaar zal zijn.

- De failover VNet is het doelnetwerk die u selecteert bij het instellen van herstel na noodgevallen.
- U wordt aangeraden dat u altijd een niet-productie-netwerk voor test-failover gebruiken.
- U kunt de doel-IP-adres in de **berekening en netwerk** instellingen van de virtuele machine.


## <a name="next-steps"></a>Volgende stappen

- Beoordeling [netwerken richtlijnen](site-recovery-azure-to-azure-networking-guidance.md) voor noodherstel van de virtuele machine van Azure.
- [Meer informatie](site-recovery-retain-ip-azure-vm-failover.md) over het IP-adressen behouden na een failover.

Als het doelnetwerk gekozen, het vnet failover is"en 2e punt om in te spreken"Als het doelnetwerk gekozen wijkt af van het vnet failover maar heeft hetzelfde subnetbereik bevinden als failover vnet"