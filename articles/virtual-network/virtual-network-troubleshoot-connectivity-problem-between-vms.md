---
title: Het oplossen van problemen met connectiviteit tussen virtuele Azure-machines | Microsoft Docs
description: Informatie over het oplossen van connectiviteitsproblemen met de tussen Azure-VM's.
services: virtual-network
documentationcenter: na
author: chadmath
manager: cshepard
editor: ''
tags: azure-resource-manager
ms.service: virtual-network
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/30/2018
ms.author: genli
ms.openlocfilehash: fc3d6ab1d7fdf05963d9ecd350deccd940a95b87
ms.sourcegitcommit: 3aa0fbfdde618656d66edf7e469e543c2aa29a57
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/05/2019
ms.locfileid: "55732512"
---
# <a name="troubleshooting-connectivity-problems-between-azure-vms"></a>Het oplossen van problemen met connectiviteit tussen virtuele Azure-machines

Problemen met connectiviteit tussen virtuele Azure-machines (VM's) kunt u mogelijk ondervindt. Dit artikel bevat stappen waarmee u kunt dit probleem oplossen. 

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="symptom"></a>Symptoom

Een die virtuele machine van Azure kan geen verbinding met een andere Azure-VM maken.

## <a name="troubleshooting-guidance"></a>Hulp bij het oplossen van problemen 

1. [Controleer of NIC is onjuist geconfigureerd](#step-1-check-whether-nic-is-misconfigured)
2. [Controleert of het netwerkverkeer wordt geblokkeerd door NSG- of UDR](#step-2-check-whether-network-traffic-is-blocked-by-nsg-or-udr)
3. [Controleer of het netwerkverkeer wordt geblokkeerd door de firewall-VM](#step-3-check-whether-network-traffic-is-blocked-by-vm-firewall)
4. [Controleer of app van de virtuele machine of service op poort luistert](#step-4-check-whether-vm-app-or-service-is-listening-on-the-port)
5. [Controleer of het probleem wordt veroorzaakt door SNAT](#step-5-check-whether-the-problem-is-caused-by-snat)
6. [Controleren of verkeer wordt geblokkeerd door de ACL's voor de klassieke virtuele machine](#step-6-check-whether-traffic-is-blocked-by-acls-for-the-classic-vm)
7. [Controleer of het eindpunt voor de klassieke virtuele machine is gemaakt](#step-7-check-whether-the-endpoint-is-created-for-the-classic-vm)
8. [Verbinding gemaakt met een VM-netwerkshare](#step-8-try-to-connect-to-a-vm-network-share)
9. [Inter-Vnet-connectiviteit controleren](#step-9-check-inter-vnet-connectivity)

## <a name="troubleshooting-steps"></a>Stappen voor probleemoplossing

Volg deze stappen voor het probleem op te lossen. Nadat u elke stap hebt voltooid, controleert u of het probleem opgelost is. 

### <a name="step-1-check-whether-nic-is-misconfigured"></a>Stap 1: Controleer of NIC is onjuist geconfigureerd

Volg de stappen in [opnieuw instellen van de netwerkinterface voor Azure Windows VM](../virtual-machines/windows/reset-network-interface.md). 

Als het probleem zich voordoet nadat u de netwerkinterface (NIC) hebt gewijzigd, volgt u deze stappen:

**Multi-NIC VM 's**

1. Toevoegen van een NIC.
2. Los de problemen oplossen tijdens de ongeldige NIC of verwijder de ongeldige NIC.  Vervolgens voegt u de NIC opnieuw toe.

Zie voor meer informatie, [netwerkinterfaces toevoegen of verwijderen van virtuele machines](virtual-network-network-interface-vm.md).

**VM één NIC 's** 

- [Windows virtuele machine opnieuw implementeren](../virtual-machines/windows/redeploy-to-new-node.md)
- [Linux virtuele machine opnieuw implementeren](../virtual-machines/linux/redeploy-to-new-node.md)

### <a name="step-2-check-whether-network-traffic-is-blocked-by-nsg-or-udr"></a>Stap 2: Controleert of het netwerkverkeer wordt geblokkeerd door NSG- of UDR

Gebruik [Network Watcher IP-stroom controleren](../network-watcher/network-watcher-ip-flow-verify-overview.md) en [NSG Stroomregistratie](../network-watcher/network-watcher-nsg-flow-logging-overview.md) om te bepalen of er een Netwerkbeveiligingsgroep (NSG) of door de gebruiker gedefinieerde Route (UDR) waarmee netwerkverkeer verstoort.

### <a name="step-3-check-whether-network-traffic-is-blocked-by-vm-firewall"></a>Stap 3: Controleer of het netwerkverkeer wordt geblokkeerd door de firewall-VM

De firewall uitschakelen en vervolgens het resultaat te testen. Als het probleem is opgelost, Controleer of de firewall-instellingen en de firewall opnieuw inschakelt.

### <a name="step-4-check-whether-vm-app-or-service-is-listening-on-the-port"></a>Stap 4: Controleer of app van de virtuele machine of service op poort luistert

U kunt een van de volgende methoden gebruiken om te controleren of de VM-app of service op de poort luistert.

- Voer de volgende opdrachten om te controleren of de server bij die poort luistert.

**Windows-VM**

    netstat –ano

**Linux-VM**

    netstat -l

- Voer de **telnet** opdracht op de virtuele machine zelf voor het testen van de poort. Als de test mislukt, wordt de toepassing of service niet geconfigureerd om te luisteren op poort.

### <a name="step-5-check-whether-the-problem-is-caused-by-snat"></a>Stap 5: Controleer of het probleem wordt veroorzaakt door SNAT

De virtuele machine wordt geplaatst in sommige scenario's achter een load saldo-oplossing die is afhankelijk van resources buiten Azure. In deze scenario's als u problemen met een onregelmatige verbinding, ondervindt het probleem kan worden veroorzaakt door [SNAT poortuitputting](../load-balancer/load-balancer-outbound-connections.md). Los het probleem, moet u een VIP (of ILPIP voor klassiek) maken voor elke virtuele machine die zich achter de load balancer en beveiligen met NSG of ACL. 

### <a name="step-6-check-whether-traffic-is-blocked-by-acls-for-the-classic-vm"></a>Stap 6: Controleren of verkeer wordt geblokkeerd door de ACL's voor de klassieke virtuele machine

Een toegangsbeheerlijst (ACL) biedt de mogelijkheid selectief toestaan of weigeren van verkeer voor het eindpunt van een virtuele machine. Zie voor meer informatie, [beheren de ACL voor een eindpunt](/previous-versions/azure/virtual-machines/windows/classic/setup-endpoints#manage-the-acl-on-an-endpoint).

### <a name="step-7-check-whether-the-endpoint-is-created-for-the-classic-vm"></a>Stap 7: Controleer of het eindpunt voor de klassieke virtuele machine is gemaakt

Alle virtuele machines die u in Azure maakt met behulp van het klassieke implementatiemodel kunnen automatisch communiceren via een privénetwerkkanaal met andere virtuele machines in de dezelfde cloudservice of een virtueel netwerk. Computers op andere virtuele netwerken moeten echter eindpunten om te leiden van het binnenkomende netwerkverkeer op een virtuele machine. Zie voor meer informatie, [over het instellen van eindpunten](../virtual-machines/windows/classic/setup-endpoints.md).

### <a name="step-8-try-to-connect-to-a-vm-network-share"></a>Stap 8: Verbinding gemaakt met een VM-netwerkshare

Als u geen verbinding naar een netwerkshare voor de virtuele machine maken, kan het probleem worden veroorzaakt door niet beschikbaar NIC's in de virtuele machine. Als u wilt verwijderen van de niet beschikbaar NIC's, Zie [de niet beschikbaar NIC's verwijderen](../virtual-machines/troubleshooting/reset-network-interface.md#delete-the-unavailable-nics)

### <a name="step-9-check-inter-vnet-connectivity"></a>Stap 9: Inter-Vnet-connectiviteit controleren

Gebruik [Network Watcher IP-stroom controleren](../network-watcher/network-watcher-ip-flow-verify-overview.md) en [NSG Stroomregistratie](../network-watcher/network-watcher-nsg-flow-logging-overview.md) om te bepalen of er een NSG of een UDR die netwerkverkeer verstoort. U kunt ook uw Inter-Vnet-configuratie controleren [hier](https://support.microsoft.com/en-us/help/4032151/configuring-and-validating-vnet-or-vpn-connections).

### <a name="need-help-contact-support"></a>Hulp nodig? Neem contact op met ondersteuning.
Als u nog steeds hulp nodig hebt, [contact op met ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) om uw probleem snel worden opgelost.