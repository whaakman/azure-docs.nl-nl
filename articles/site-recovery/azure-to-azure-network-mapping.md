---
title: Toewijzen van virtuele netwerken tussen twee Azure-regio's in Azure Site Recovery | Microsoft Docs
description: Azure Site Recovery coördineert de replicatie, failover en herstel van virtuele machines en fysieke servers. Meer informatie over failover naar Azure of naar een secundair datacenter.
author: mayurigupta13
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 2/28/2018
ms.author: mayg
ms.openlocfilehash: c4309b20664520bc1912adadee72d614a085f573
ms.sourcegitcommit: 8b41b86841456deea26b0941e8ae3fcdb2d5c1e1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/05/2019
ms.locfileid: "57338950"
---
# <a name="set-up-network-mapping-and-ip-addressing-for-vnets"></a>Instellen van netwerktoewijzing en IP-adressering voor vnet 's

Dit artikel wordt beschreven hoe u kunt twee exemplaren van virtuele Azure-netwerken (VNets) zich in verschillende Azure-regio's toewijzen, en over het instellen van het IP-adressering tussen netwerken. Netwerktoewijzing biedt een standaardgedrag voor het doel netwerk selecteren op basis van de bron-netwerk op het moment van het inschakelen van replicatie.

## <a name="prerequisites"></a>Vereisten

Voordat u netwerken toewijzen, moet u beschikken over [Azure VNets](../virtual-network/virtual-networks-overview.md) in de bron en doel-Azure-regio's. 

## <a name="set-up-network-mapping-manually-optional"></a>Instellen van netwerktoewijzing handmatig (optioneel)

Netwerken als volgt toewijzen:

1. In **Site Recovery-infrastructuur**, klikt u op **+ netwerktoewijzing**.

    ![ Maken van een netwerktoewijzing](./media/site-recovery-network-mapping-azure-to-azure/network-mapping1.png)

3. In **netwerktoewijzing toevoegen**, selecteert u de bron en doel locaties. In ons voorbeeld de bron virtuele machine wordt uitgevoerd in de regio Oost-Azië, en worden gerepliceerd naar de regio Zuidoost-Azië.

    ![Bron- en doel selecteren](./media/site-recovery-network-mapping-azure-to-azure/network-mapping2.png)
3. Maak nu een netwerktoewijzing in de tegengestelde directory. In ons voorbeeld, de bron worden nu Zuidoost-Azië en het doel is Oost-Azië.

    ![Toevoegen van deelvenster van de toewijzing van netwerk - bron- en doellocaties voor het doelnetwerk selecteren](./media/site-recovery-network-mapping-azure-to-azure/network-mapping3.png)


## <a name="map-networks-when-you-enable-replication"></a>Netwerken toewijzen wanneer u replicatie inschakelt

Als u dit nog niet hebt voorbereid netwerktoewijzing voordat u herstel na noodgevallen voor Azure-VM's configureert, kunt u een doel netwerk wanneer u [instellen en inschakelen van replicatie](azure-to-azure-how-to-enable-replication.md). Wanneer u dit doen het volgende gebeurt:

- Op basis van het doel dat u selecteert, maakt Site Recovery automatisch Netwerktoewijzingen van de bron naar doelregio, en het doel naar de bronregio.
- Site Recovery maakt standaard een netwerk in de doelregio die identiek is aan het Bronnetwerk. Site Recovery voegt **-asr** als achtervoegsel aan de naam van het Bronnetwerk. U kunt het doelnetwerk kunt aanpassen.
- Als de netwerktoewijzing al heeft plaatsgevonden voor een Bronnetwerk, worden het toegewezen netwerk standaard altijd op het moment van het inschakelen van replicaties voor meer virtuele machines. U kunt het virtuele doelnetwerk wijzigen door het kiezen van andere beschikbare opties in de vervolgkeuzelijst. 
- Als u wilt wijzigen van het virtuele doelnetwerk standaard voor nieuwe replicaties, moet u de bestaande netwerktoewijzing wijzigt.
- Als u wilt een netwerktoewijzing van de A-regio naar regio B wijzigt, zorg ervoor dat u eerst de netwerktoewijzing verwijderen van de B-regio naar regio A. Na het verwijderen van de omgekeerde toewijzing wijzigen van de netwerktoewijzing van de A-regio naar regio B en maak vervolgens de relevante omgekeerde toewijzing.

>[!NOTE]
>* De standaardinstellingen voor nieuwe VM-replicaties wijzigen van de netwerktoewijzing alleen worden gewijzigd. Dit heeft geen invloed op de selecties voor het virtuele netwerk van doel voor bestaande replicaties. 
>* Als u wijzigen van het doelnetwerk voor een bestaande replicatie wilt, gaat u naar de reken- en netwerkinstellingen van het gerepliceerde item.

## <a name="specify-a-subnet"></a>Geef een subnet

Het subnet van het doel-VM is geselecteerd, is afhankelijk van de naam van het subnet van de bron-VM.

- Als een subnet met dezelfde naam als de bron-VM-subnet in het netwerk beschikbaar is, wordt dat subnet is ingesteld voor de doel-VM.
- Als een subnet met dezelfde naam in het doelnetwerk bestaat, is het eerste subnet in alfabetische volgorde ingesteld als het doelsubnet.
- U kunt de in de **berekening en netwerk** instellingen voor de virtuele machine.

    ![Reken- en compute-eigenschappenvenster](./media/site-recovery-network-mapping-azure-to-azure/modify-subnet.png)


## <a name="set-up-ip-addressing-for-target-vms"></a>IP-adressering instellen voor doel-VM 's

Het IP-adres voor elke NIC op een virtuele doelmachine wordt als volgt geconfigureerd:

- **DHCP**: Als de NIC van de bron-VM gebruikmaakt van DHCP, wordt de NIC van de doel-VM ook instellen op het gebruik van DHCP.
- **Statisch IP-adres**: Als de NIC van de bron-VM gebruikmaakt van vaste IP-adressen, zal de doel-VM NIC ook een statisch IP-adres gebruiken.


## <a name="ip-address-assignment-during-failover"></a>Toewijzing van IP-adres tijdens de failover

**Bron- en subnetten** | **Details**
--- | ---
Dezelfde-adresruimte | IP-adres van de VM NIC-bron is ingesteld als de doel-VM NIC IP-adres.<br/><br/> Als het adres beschikbaar is, wordt het volgende beschikbare IP-adres is ingesteld als het doel.
Andere adresruimte<br/><br/> Het eerstvolgende beschikbare IP-adres in het doelsubnet is ingesteld als de doel-VM NIC-adres.



## <a name="ip-address-assignment-during-test-failover"></a>Toewijzing van IP-adres tijdens de testfailover

**Doelnetwerk** | **Details**
--- | ---
Doelnetwerk is failover VNet | -Doel IP-adres wordt statisch, maar niet hetzelfde IP-adres als gereserveerd voor failover.<br/><br/>  -Het toegewezen adres is het eerstvolgende beschikbare adres vanaf het einde van het subnetbereik.<br/><br/> Bijvoorbeeld: Als de bron-IP-adres 10.0.0.19 is en failovernetwerk netwerkadresbereik 10.0.0.0/24 wordt gebruikt, is het volgende IP-adres toegewezen aan de doel-VM 10.0.0.254 beschikbaar.
Doelnetwerk is niet de failover VNet | -Doel IP-adres worden statische met hetzelfde IP-adres gereserveerd voor failover.<br/><br/>  -Als hetzelfde IP-adres al toegewezen is, is het IP-adres het volgende object beschikbaar zijn op elk van de subnetbereik.<br/><br/> Bijvoorbeeld: Als het statische IP-adres van bron 10.0.0.19 is en failover bevindt op een netwerk dat niet het failovernetwerk, met het netwerkadresbereik 10.0.0.0/24, klikt u vervolgens het statische IP-adres van doel is 10.0.0.0.19 indien beschikbaar, en anders het 10.0.0.254 beschikbaar zal zijn.

- De failover VNet is het doelnetwerk die u selecteert bij het instellen van herstel na noodgevallen.
- U wordt aangeraden dat u altijd een niet-productie-netwerk voor test-failover gebruiken.
- U kunt de doel-IP-adres in de **berekening en netwerk** instellingen van de virtuele machine.


## <a name="next-steps"></a>Volgende stappen

- Beoordeling [netwerken richtlijnen](site-recovery-azure-to-azure-networking-guidance.md) voor noodherstel van de virtuele machine van Azure.
- [Meer informatie](site-recovery-retain-ip-azure-vm-failover.md) over het IP-adressen behouden na een failover.

Als het doelnetwerk gekozen, het vnet failover is"en 2e punt om in te spreken"Als het doelnetwerk gekozen wijkt af van het vnet failover maar heeft hetzelfde subnetbereik bevinden als failover vnet"