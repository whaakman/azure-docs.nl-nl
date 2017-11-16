---
title: Instellen van IP-adressering verbinding maken na een failover naar Azure met Azure Site Recovery | Microsoft Docs
description: Beschrijft het instellen van IP-adressen voor verbinding met virtuele Azure-machines na een failover van on-premises met Azure Site Recovery
services: site-recovery
documentationcenter: 
author: prateek9us
manager: carmonm
editor: 
ms.assetid: f02cdbea-0940-48bf-9fa5-f38d9e584fae
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 11/14/2017
ms.author: pratshar
ms.openlocfilehash: 5519a965d9828cfa1e73ba12f8acd1d509a36a66
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/15/2017
---
# <a name="set-up-ip-addressing-to-connect-after-failover-to-azure"></a>IP-adressen als u wilt verbinden na een failover naar Azure instellen

Dit artikel wordt uitgelegd dat de netwerkvereisten voor de verbinding met virtuele Azure-machines, nadat u de [Azure Site Recovery](site-recovery-overview.md) service voor replicatie en failover naar Azure.

In dit artikel leert u over:

> [!div class="checklist"]
> * De verbindingsmethoden die u kunt gebruiken
> * Het gebruik van een ander IP-adres voor gerepliceerde virtuele machines van Azure
> * Het IP-adressen behouden voor virtuele Azure-machines na een failover

## <a name="connecting-to-replica-vms"></a>Verbinding maken met de replica virtuele machines

Bij het plannen van uw replicatie en failoverstrategie is een van de belangrijke vragen verbinding maken met de Azure VM na een failover. Er zijn een aantal opties bij het ontwerpen van uw netwerk-strategie voor de replica virtuele Azure-machines:

- **Gebruik verschillende IP-adres**: U kunt selecteren om een ander IP-adresbereik voor de gerepliceerde Azure VM-netwerk. In dit scenario wordt de virtuele machine een nieuw IP-adres na een failover en een DNS-update is vereist.
- **Hetzelfde IP-adres behouden**: U kunt hetzelfde IP-adresbereik gebruiken als die in uw primaire on-premises-site voor het Azure-netwerk na een failover. Houden dezelfde IP adressen het herstel vereenvoudigt door te beperken netwerkgerelateerde problemen na een failover. Echter, wanneer u naar Azure repliceert, moet u het bijwerken van routes met de nieuwe locatie van de IP-adressen na een failover. 

## <a name="retaining-ip-addresses"></a>IP-adressen behouden

Site Recovery biedt de mogelijkheid voor het bewaren van vaste IP-adressen wanneer failover wordt uitgevoerd naar Azure, met de failover van een subnet.

- Met subnet-failover is een specifiek subnet aanwezig op de Site 1 of 2, maar niet op beide sites tegelijkertijd.
- Om toegang te houden van de IP-adresruimte in het geval van een failover, rangschikt u programmatisch de router-infrastructuur voor het verplaatsen van de subnetten van de ene site naar een andere.
- Tijdens de failover verplaatst de subnetten met de bijbehorende beveiligde virtuele machines. Het grootste nadeel is dat bij een storing, die u moet het hele subnet verplaatsen.


### <a name="failover-example"></a>Failover-voorbeeld

Bekijk een voorbeeld is voor failover naar Azure usng een bedrijf ficticious Woodgrove Bank.

- Woodgrove Bank host die zakelijke apps in een on-premises site. Ze hosten hun mobiele apps in Azure.
- Er is een VPN-site-naar-site-verbinding tussen hun rand on-premises netwerk en de virtuele Azure-netwerk. Vanwege de VPN-verbinding, wordt het virtuele netwerk in Azure weergegeven als een uitbreiding van de on-premises netwerk.
- Woodgrove wil voor lokale werkbelastingen repliceren naar Azure met Site Recovery.
 - Woodgrove heeft apps die afhankelijk van de vastgelegde IP-adressen, zijn zodat ze nodig hebben voor het bewaren van IP-adressen voor de apps na een failover naar Azure.
 - Resources die worden uitgevoerd in Azure gebruikmaken van de IP-adresbereik 172.16.1.0/24, 172.16.2.0/24.

![Vóór de subnet-failover](./media/site-recovery-network-design/network-design7.png)

**Infrastructuur voor failover**


Voor de Woodgrove kunnen aan de bijbehorende virtuele machines repliceren naar Azure behoud de IP-adressen, hier van wat moet het bedrijf doen:


1. Azure-netwerk waarin de Azure VM's worden gemaakt nadat de failover van lokale computers maken. Deze moet een uitbreiding van de on-premises netwerk, zodat toepassingen naadloos kunnen failover.
2. Vóór de failover in Site Recovery toewijzen ze hetzelfde IP-adres in de eigenschappen van de machine. Na een failover wordt met Site Recovery dit adres toegewezen aan de Azure VM.
3. Nadat de failover wordt uitgevoerd en de Azure VM's zijn gemaakt met hetzelfde IP-adres, ze verbinding maken met het netwerk met een [Vnet-naar-Vnet-verbinding](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md). Deze actie kan scripts worden gebruikt.
4. Ze moeten naar routes, zodat die 192.168.1.0/24 is nu verplaatst naar Azure te wijzigen.


**Infrastructuur na een failover**

![Na een failover van subnet](./media/site-recovery-network-design/network-design9.png)

#### <a name="site-to-site-connection"></a>Site-naar-site-verbinding

Naast de vnet-naar-vnet-verbinding, na een failover kunt Woodgrove site-naar-site VPN-verbinding instellen:
- Wanneer u een site-naar-site-verbinding instellen, in het netwerk van Azure dat kunt u alleen routeren wijkt verkeer naar de on-premises locatie (lokale ntwork) als het IP-adresbereik af van het lokale IP-adresbereik. Dit is omdat Azure biedt geen ondersteuning voor gespreide subnetten. Dus als u subnet 192.168.1.0/24 lokale hebt, kunt u een lokaal netwerk 192.168.1.0/24 niet in het Azure-netwerk toevoegen. Dit is normaal omdat Azure niet weet dat er zijn geen actieve VM's in het subnet en het subnet voor noodherstel alleen wordt gemaakt.
- Om te kunnen routeren correct netwerkverkeer buiten een Azure-netwerk, zijn de subnetten in het netwerk en het LAN-netwerk mag niet in conflict.




## <a name="assigning-new-ip-addresses"></a>Nieuwe IP-adressen toewijzen

Dit [blogbericht](http://azure.microsoft.com/blog/2014/09/04/networking-infrastructure-setup-for-microsoft-azure-as-a-disaster-recovery-site/) wordt uitgelegd hoe u de Azure-netwerkinfrastructuur wilt instellen wanneer u hoeft niet te IP-adressen behouden na een failover. Deze begint met een toepassingsbeschrijving, het instellen van het lokale netwerk en in Azure wordt bekeken en wordt afgesloten met informatie over het uitvoeren van failovers. 

## <a name="next-steps"></a>Volgende stappen
[Een failover uitvoeren](site-recovery-failover.md)




