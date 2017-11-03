---
title: Het oplossen van problemen met de netwerkconnectiviteit tussen Azure Virtual machines | Microsoft Docs
description: Informatie over het oplossen van de connectiviteit tussen virtuele Azure-machines.
services: virtual-network
documentationcenter: na
author: chadmath
manager: cshepard
editor: 
tags: azure-resource-manager
ms.service: virtual-network
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/25/2017
ms.author: genli
ms.openlocfilehash: 3011ef7eced5a24ba07d06e2db2f5e4d344b94de
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="troubleshooting-connectivity-problems-between-azure-vms"></a>Het oplossen van problemen met de netwerkconnectiviteit tussen Azure VM 's

Er kunnen problemen met de netwerkconnectiviteit tussen Azure virtuele machines (VM's). Dit artikel bevat de stappen voor probleemoplossing waarmee u kunt dit probleem oplossen. 

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="symptom"></a>Symptoom

Een die virtuele machine van Azure kan geen verbinding met een andere virtuele machine in Azure.

## <a name="troubleshooting-guidance"></a>Hulp bij het oplossen van problemen 

1. [Controleer of NIC is onjuist geconfigureerd](#step-1-check-whether-nic-is-misconfigured)
2. [Controleer of het netwerkverkeer wordt geblokkeerd door NSG of UDR](#step-2-check-whether-network-traffic-is-blocked-by-nsg-or-udr)
3. [Controleer of netwerkverkeer wordt geblokkeerd door VM-firewall](#step-3-check-whether-network-traffic-is-blocked-by-vm-firewall)
4. [Controleer of VM-app of service op poort luistert](#step-4-check-whether-vm-app-or-service-is-listening-on-the-port)
5. [Controleer of het probleem wordt veroorzaakt door snat omzetten](#step-5-check-whether-the-problem-is-caused-by-snat)
6. [Controleer of verkeer wordt geblokkeerd door de ACL's voor de klassieke virtuele machine](#step-6-check-whether-traffic-is-blocked-by-acls-for-the-classic-vm)
7. [Controleer of het eindpunt voor de klassieke virtuele machine wordt gemaakt](#step-7-check-whether-the-endpoint-is-created-for-the-classic-vm)
8. [Maak verbinding met een VM-netwerkshare](#step-8-try-to-connect-to-a-vm-network-share)
9. [Inter Vnet-connectiviteit controleren](#step-9-check-inter-vnet-connectivity)

## <a name="troubleshooting-steps"></a>Stappen voor probleemoplossing

Volg deze stappen voor het oplossen van het probleem. Nadat u elke stap hebt voltooid, controleert u of het probleem opgelost is. 

### <a name="step-1-check-whether-nic-is-misconfigured"></a>Stap 1: Controleer of NIC is onjuist geconfigureerd

Volg de stappen in [netwerkinterface herstellen voor virtuele machine van Windows Azure](../virtual-machines/windows/reset-network-interface.md). 

Als het probleem optreedt nadat u de netwerkinterface (NIC) hebt gewijzigd, voert u deze stappen:

**Virtuele machines van meerdere NIC 's**

1. Toevoegen van een NIC.
2. Los de problemen op de onjuiste NIC of verwijder de ongeldige NIC.  Vervolgens voegt u de NIC opnieuw toe.

Zie voor meer informatie [netwerkinterfaces toevoegen of verwijderen van virtuele machines](virtual-network-network-interface-vm.md).

**Één NIC VM** 

- [Implementeer Windows VM opnieuw](../virtual-machines/windows/redeploy-to-new-node.md)
- [Virtuele Linux-machine implementeren](../virtual-machines/linux/redeploy-to-new-node.md)

### <a name="step-2-check-whether-network-traffic-is-blocked-by-nsg-or-udr"></a>Stap 2: Controleer of het netwerkverkeer wordt geblokkeerd door NSG of UDR

Gebruik [netwerk-Watcher IP-stroom controleren](../network-watcher/network-watcher-ip-flow-verify-overview.md) en [NSG stroom logboekregistratie](../network-watcher/network-watcher-nsg-flow-logging-overview.md) om te bepalen of er is een Netwerkbeveiligingsgroep (NSG) of door de gebruiker gedefinieerde Route (UDR) die netwerkverkeer verstoort.

### <a name="step-3-check-whether-network-traffic-is-blocked-by-vm-firewall"></a>Stap 3: Controleren of netwerkverkeer wordt geblokkeerd door VM-firewall

De firewall uitschakelen en vervolgens het resultaat te testen. Als het probleem is opgelost, Controleer of de firewall-instellingen en de firewall opnieuw inschakelen.

### <a name="step-4-check-whether-vm-app-or-service-is-listening-on-the-port"></a>Stap 4: Controleren of VM-app of service op poort luistert

U kunt een van de volgende methoden gebruiken om te controleren of de VM-app of de service op de poort luistert.

- Voer de volgende opdrachten om te controleren of de server op deze poort luistert.

**Windows-VM**

    netstat –ano

**Linux-VM**

    netstat -l

- Voer de **telnet** opdracht op de virtuele machine zelf voor het testen van de poort. Als de test mislukt, wordt de toepassing of service niet geconfigureerd om op deze poort te luisteren.

### <a name="step-5-check-whether-the-problem-is-caused-by-snat"></a>Stap 5: Controleer of het probleem wordt veroorzaakt door snat omzetten

In sommige scenario's, wordt de virtuele machine geplaatst achter een load balance oplossing met een afhankelijkheid op resources buiten Azure. In deze scenario's, als u met de verbinding problemen, het probleem mogelijk worden veroorzaakt door [snat omzetten poort bronuitputting](../load-balancer/load-balancer-outbound-connections.md). Het probleem op te lossen moet u een VIP (of ILPIP voor klassiek) maken voor elke virtuele machine die zich achter de load balancer en beveiligen met NSG of ACL. 

### <a name="step-6-check-whether-traffic-is-blocked-by-acls-for-the-classic-vm"></a>Stap 6: Controleren of verkeer wordt geblokkeerd door de ACL's voor de klassieke virtuele machine

Een toegangsbeheerlijst (ACL) biedt de mogelijkheid om selectief toestaan of weigeren van verkeer voor het eindpunt van een virtuele machine. Zie voor meer informatie [beheren de ACL voor een eindpunt](../virtual-machines/windows/classic/setup-endpoints.md#manage-the-acl-on-an-endpoint).

### <a name="step-7-check-whether-the-endpoint-is-created-for-the-classic-vm"></a>Stap 7: Controleren of het eindpunt voor de klassieke virtuele machine wordt gemaakt

Alle virtuele machines die u met behulp van het klassieke implementatiemodel in Azure maakt kunnen automatisch via een particulier netwerkkanaal met andere virtuele machines in dezelfde cloudservice- of virtueel netwerk communiceren. Computers met andere virtuele netwerken vereisen echter eindpunten het binnenkomend netwerkverkeer naar een virtuele machine wordt omgeleid. Zie voor meer informatie [het instellen van eindpunten](../virtual-machines/windows/classic/setup-endpoints.md).

### <a name="step-8-try-to-connect-to-a-vm-network-share"></a>Stap 8: Maak verbinding met een VM-netwerkshare

Als u geen verbinding met een VM-netwerkshare maken, kan het probleem worden veroorzaakt door niet beschikbaar NIC's in de virtuele machine. Zie het verwijderen van de niet beschikbaar NIC's [het verwijderen van de niet beschikbaar NIC's](../virtual-machines/windows/reset-network-interface.md#delete-the-unavailable-nics)

### <a name="step-9-check-inter-vnet-connectivity"></a>Stap 9: Selectievakje Inter-Vnet-connectiviteit

Gebruik [netwerk-Watcher IP-stroom controleren](../network-watcher/network-watcher-ip-flow-verify-overview.md) en [NSG stroom logboekregistratie](../network-watcher/network-watcher-nsg-flow-logging-overview.md) om te bepalen of er een NSG of UDR die netwerkverkeer verstoort. U kunt ook controleren of uw configuratie Inter Vnet [hier](https://support.microsoft.com/en-us/help/4032151/configuring-and-validating-vnet-or-vpn-connections).

### <a name="need-help-contact-support"></a>Hulp nodig? Neem contact op met ondersteuning.
Als u nog hulp nodig hebt, [contact op met ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) ophalen van uw probleem snel worden opgelost.