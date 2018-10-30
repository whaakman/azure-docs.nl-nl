---
title: IP-adressen als u wilt verbinden na herstel na noodgevallen en failover naar Azure met Azure Site Recovery instellen | Microsoft Docs
description: Beschrijft hoe u voor het instellen van IP-adressering voor verbinding met virtuele Azure-machines na het herstel na noodgevallen en failover van on-premises met Azure Site Recovery
services: site-recovery
author: mayurigupta13
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 10/16/2018
ms.author: mayg
ms.openlocfilehash: ce7e5fde60503a969b88892be890173861fb15be
ms.sourcegitcommit: 6e09760197a91be564ad60ffd3d6f48a241e083b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/29/2018
ms.locfileid: "50215222"
---
# <a name="set-up-ip-addressing-to-connect-to-azure-vms-after-failover"></a>Instellen van IP-adressen voor de verbinding met virtuele Azure-machines na een failover

Dit artikel wordt uitgelegd dat de netwerkvereisten voor de verbinding met virtuele machines van Azure, nadat u de [Azure Site Recovery](site-recovery-overview.md) service voor replicatie en failover naar Azure.

In dit artikel leert u over:

> [!div class="checklist"]
> * De verbindingsmethoden die u kunt gebruiken
> * Over het gebruik van een ander IP-adres voor de gerepliceerde Azure-VM 's
> * Het IP-adressen behouden voor Azure VM's na failover

## <a name="connecting-to-replica-vms"></a>Verbinding maken met replica-VM 's

Bij het plannen van de replicatie- en failoverstrategie, is een van de belangrijkste vragen verbinding maken met de virtuele Azure-machine na een failover. Er zijn een aantal opties bij het ontwerpen van uw netwerk-strategie voor de replica virtuele machines van Azure:

- **Gebruik verschillende IP-adres**: U kunt selecteren met een ander IP-adresbereik voor het gerepliceerde Azure-VM-netwerk. In dit scenario wordt de virtuele machine een nieuwe IP-adres na een failover en een DNS-update is vereist.
- **Hetzelfde IP-adres behouden**: kunt u hetzelfde IP-adresbereik als die op uw primaire on-premises site, gebruiken voor het Azure-netwerk na een failover. Het hetzelfde IP-adres bijhouden vereenvoudigt adressen het herstel door te beperken netwerk na een failover problemen met betrekking tot. Echter, als u naar Azure repliceert, u moet bijwerken van routes met de nieuwe locatie van de IP-adressen na een failover.

## <a name="retaining-ip-addresses"></a>IP-adressen behouden

Site Recovery biedt de mogelijkheid voor het behouden van vaste IP-adressen bij het uitvoeren van een failover naar Azure, met de failover van een subnet.

- Met subnet failover is een specifiek subnet aanwezig in Site 1 of 2, maar niet op beide locaties tegelijk.
- Om te kunnen handhaven van de IP-adresruimte in het geval van een failover, rangschikt u via een programma voor de router-infrastructuur te verplaatsen van de subnetten van de ene site naar een andere.
- Tijdens de failover Verplaats de subnetten met de bijbehorende beveiligde virtuele machines. Het grootste nadeel is dat er een storing optreedt, moet u het hele subnet verplaatsen.


### <a name="failover-example"></a>Voorbeeld van de failover

We bekijken een voorbeeld voor failover naar Azure met behulp van een fictieve bedrijf Woodgrove Bank.

- Woodgrove Bank als host fungeert voor de business-apps in een on-premises site. Ze hosten hun mobiele apps op Azure.
- Er is een site-naar-site-VPN-verbinding tussen hun edge on-premises netwerk en de Azure-netwerk. Vanwege de VPN-verbinding, wordt het virtuele netwerk in Azure weergegeven als een uitbreiding van de on-premises netwerk.
- Voor het repliceren van on-premises werkbelastingen naar Azure met Site Recovery wil Woodgrove.
 - Woodgrove heeft apps die afhankelijk van de vastgelegde IP-adressen, zijn daarom ze moeten voor het IP-adressen voor de apps behouden na een failover naar Azure.
 - Resources die worden uitgevoerd in Azure gebruiken de IP-adresbereik 172.16.1.0/24, 172.16.2.0/24.

![Voordat de failover van subnet](./media/site-recovery-network-design/network-design7.png)

**Infrastructuur voordat de failover**


Voor de Woodgrove kunnen aan de VM's repliceren naar Azure en behoud van de IP-adressen, hier van wat bedrijf het te doen:


1. Azure-netwerk waarin de Azure VM's worden gemaakt nadat de failover van on-premises machines maken. Het moet een extensie van de on-premises netwerk, zodat toepassingen kunnen naadloos voor failover.
2. Voordat de failover in Site Recovery, wijzen ze hetzelfde IP-adres in de eigenschappen van de machine. Site Recovery wijst dit adres na een failover toe aan de Azure VM.
3. Nadat de failover wordt uitgevoerd en de Azure VM's worden gemaakt met hetzelfde IP-adres, ze verbinding maken met het netwerk met een [Vnet-naar-Vnet-verbinding](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md). Deze actie kan scripts worden gebruikt.
4. Ze wilt wijzigen, routes, zodat die 192.168.1.0/24 is nu verplaatst naar Azure.


**Infrastructuur na een failover**

![Na een failover van subnet](./media/site-recovery-network-design/network-design9.png)

#### <a name="site-to-site-connection"></a>Site-naar-site-verbinding

Naast de vnet-naar-vnet-verbinding na een failover kunt Woodgrove site-naar-site VPN-verbinding instellen:
- Bij het instellen van een site-naar-site-verbinding, in het netwerk van Azure dat kunt u alleen routeren wijkt verkeer naar de on-premises locatie (LAN-netwerk) als het IP-adresbereik af van het lokale IP-adresbereik. Dit komt doordat de gespreide subnetten biedt geen ondersteuning voor Azure. Dus als u subnet 192.168.1.0/24 on-premises hebt, kunt u een lokaal netwerk 192.168.1.0/24 niet in het Azure-netwerk toevoegen. Dit is normaal omdat Azure niet bekend is dat er geen actieve VM's in het subnet zijn en dat het subnet wordt gemaakt voor alleen herstel na noodgevallen.
- Als u goed netwerkverkeer te routeren buiten een Azure-netwerk, de subnetten in het netwerk en het LAN-netwerk kunnen niet in strijd zijn.




## <a name="assigning-new-ip-addresses"></a>Nieuwe IP-adressen toewijzen

Dit [blogbericht](http://azure.microsoft.com/blog/2014/09/04/networking-infrastructure-setup-for-microsoft-azure-as-a-disaster-recovery-site/) wordt uitgelegd hoe u de Azure-netwerkinfrastructuur instellen wanneer u niet wilt IP-adressen behouden na een failover. Deze begint met een beschrijving, gekeken naar het on-premises netwerken en in Azure instellen en eindigt met informatie over het uitvoeren van failovers.

## <a name="next-steps"></a>Volgende stappen
[Een failover uitvoeren](site-recovery-failover.md)
