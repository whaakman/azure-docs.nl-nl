---
title: SAP HANA-bewerkingen op Azure | Microsoft Docs
description: Bewerkingen van SAP HANA op virtuele machines in Azure systeemeigen
services: virtual-machines-linux,virtual-machines-windows
documentationcenter: 
author: juergent
manager: patfilot
editor: 
tags: azure-resource-manager
keywords: 
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 11/17/2017
ms.author: msjuergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 0328bdc40429e1e82a76f290f5bde39089db0a9d
ms.sourcegitcommit: 29bac59f1d62f38740b60274cb4912816ee775ea
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/29/2017
---
# <a name="sap-hana-on-azure-operations-guide"></a>SAP HANA op Azure-bedieningshandleiding
Deze handleiding bevat richtlijnen voor SAP HANA besturingssystemen die zijn geïmplementeerd op Azure Virtual Machines. Dit document is niet bedoeld ter vervanging van een van de standaard SAP-documentatie. SAP-handleidingen en -opmerkingen vindt u op de volgende locaties:

- [Beheerdershandleiding voor SAP](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.02/en-US/330e5550b09d4f0f8b6cceb14a64cd22.html)
- [SAP installatiehandleidingen](https://service.sap.com/instguides)
- [SAP-notitie](https://sservice.sap.com/notes)

Vereiste is dat u basiskennis van de verschillende Azure-onderdelen van:

- [Virtuele Machines in Azure](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-manage-vm)
- [Azure-netwerk- en vnet 's](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-virtual-network)
- [Azure Storage](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-manage-disks)

Aanvullende documentatie over SAP NetWeaver en andere onderdelen SAP op Azure vindt u in de [SAP op Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/get-started) sectie van de [Azure-documentatie](https://docs.microsoft.com/azure/).

## <a name="basic-setup-considerations"></a>Basisinstellingen overwegingen
### <a name="connecting-into-azure"></a>Verbinding maken in Azure
Zoals beschreven in [Azure Virtual Machines planning en implementatie voor SAP NetWeaver] [-Planningshandleiding], er zijn twee basismethoden in Azure virtuele Machines te koppelen. 

- Verbinding maken via Internet en openbare eindpunten op een VM gaan of de virtuele machine met SAP HANA
- Verbinding maken via een [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal) of Azure [ExpressRoute](https://azure.microsoft.com/services/expressroute/)

Voor productie scenario's of scenario's waarin niet productiescenario's die naar de productie-scenario's in combinatie met SAP-software die u wilt een site-naar-site-connectiviteit via VPN- of ExpressRoute hebben, zoals wordt weergegeven in deze afbeelding:

![Cross-site-connectiviteit](media/virtual-machines-shared-sap-planning-guide/300-vpn-s2s.png)


### <a name="choice-of-azure-vm-types"></a>Keuze van de virtuele machine van Azure-typen
Azure VM-typen die kunnen worden gebruikt voor productiescenario's kunnen worden opgezocht [hier](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html). Voor niet-productieve scenario's, kunt u een groter aantal systeemeigen Azure VM's. U moet echter zelf tot beperken de VM-typen die zijn [SAP-notitie #1928533](https://launchpad.support.sap.com/#/notes/1928533). Implementatie van deze virtuele machines in Azure kan worden uitgevoerd via:

- Azure Portal
- Azure Powershell-Cmdlets
- Azure CLI

U kunt volledige geïnstalleerde SAP HANA-platform op Azure Virtual machine-services via ook implementeren [SAP-cloudplatform](https://cal.sap.com/) zoals beschreven [hier](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/cal-s4h).

### <a name="choice-of-azure-storage"></a>Keuze van de Azure-opslag
Azure biedt twee belangrijke opslagtypen geschikt is voor Azure Virtual machines SAP HANA uitgevoerd

- [Azure Standard-opslag](https://docs.microsoft.com/azure/virtual-machines/windows/standard-storage)
- [Azure Premium-opslag](https://docs.microsoft.com/azure/virtual-machines/windows/premium-storage)

Azure biedt twee methoden voor het implementeren voor virtuele harde schijven op Azure Standard en Premium-opslag. Als het algemene scenario toestaat, is het raadzaam gebruikmaken van [beheerde Azure-schijf](https://azure.microsoft.com/services/managed-disks/) implementaties.

Controleer voor de exacte opslagtypen en sla's rond de opslagtypen [deze documentatie](https://azure.microsoft.com/pricing/details/managed-disks/)

Het verdient aanbeveling gebruik van Azure Premium-schijven voor /hana/data en /hana/log volumes. Voor het bouwen van een RAID LVM over meerdere schijven met Premium-opslag en gebruik deze RAID-volumes als /hana/data en /hana/log volumes wordt ondersteund.

Een mogelijke configuratie voor verschillende algemene typen van de VM die klanten gebruikt tot nu toe als host voor SAP HANA op Azure Virtual machines kan eruitzien als:

| VM-SKU | RAM | hana/gegevens en hana/logboekbestanden<br /> striped met LVM of MDADM | / hana/gedeeld | / Root-volume | / usr/sap | Hana/back-up |
| --- | --- | --- | --- | --- | --- | -- |
| E16v3 | 128GB | 2 x P20 | 1 x S20 | 1 x S6 | 1 x S6 | 1 x S10 |
| E32v3 | 256GB | 2 x P20 | 1 x S20 | 1 x S6 | 1 x S6 | 1 x S20 |
| E64v3 | 443GB | 2 x P20 | 1 x S20 | 1 x S6 | 1 x S6 | 1 x S30 |
| GS5 | 448 GB | 2 x P20 | 1 x S20 | 1 x S6 | 1 x S6 | 1 x S30 |
| M64s | 1TB | 2 x P30 | 1 x S30 | 1 x S6 | 1 x S6 |2 x S30 |
| M64ms | 1.7 TB | 3 x P30 | 1 x S30 | 1 x S6 | 1 x S6 | 3 x S30 |
| M128s | 2TB | 3 x P30 | 1 x S30 | 1 x S6 | 1 x S6 | 3 x S30 |
| M128ms | 3.8 TB | 5 x P30 | 1 x S30 | 1 x S6 | 1 x S6 | 5 x S30 |


### <a name="azure-networking"></a>Azure-netwerken
Ervan uitgaande dat u een verbinding van de site-naar-site VPN- of ExpressRoute in Azure hebt, hebt u minimaal één [Azure VNet](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview) die is verbonden via een virtuele Gateway aan de VPN- of ExpressRoute-circuit. De virtuele Gateway woont in een subnet in het Azure Vnet. Om te installeren HANA, maakt u een andere twee subnetten binnen het VNet. Één subnet dat de VM('s) met de SAP HANA-exemplaren en een ander subnet met uiteindelijke Jumpbox of Management VM('s) die hosts SAP HANA Studio of andere software voor beheer kunnen hosten.
Wanneer u de virtuele machines die moeten worden uitgevoerd HANA installeert, moeten de virtuele machines hebben:

- Twee virtuele NIC's geïnstalleerd die een verbinding met het management-subnet maakt en één NIC die wordt gebruikt vanaf op lokale of andere netwerken verbinding maken met de SAP HANA-exemplaar in de Azure VM.
- Statische privé IP-adressen die zijn geïmplementeerd voor beide vNICs

Een overzicht van de verschillende mogelijkheden om IP-adrestoewijzing vindt [hier](https://docs.microsoft.com/azure/virtual-network/virtual-network-ip-addresses-overview-arm). 

Door het verkeersroutering naar rechtstreeks naar het SAP HANA-exemplaar of de jumpbox wordt geleid [Netwerkbeveiligingsgroepen Azure](https://docs.microsoft.com/en-us/azure/virtual-network/virtual-networks-nsg) die zijn gekoppeld aan het subnet HANA en het subnet Management.

De algemene eruit als het schema ruwe implementatie:

![Schema voor SAP HANA ruwe implementatie](media/hana-vm-operations/hana-simple-networking.PNG)


Als u alleen SAP HANA in Azure implementeren zonder een site-naar-site (VPN- of ExpressRoute in Azure), opent u zou de SAP HANA-exemplaar al is een openbaar IP-adres dat is toegewezen aan de virtuele machine in Azure waarop uw Jumpbox VM wordt uitgevoerd. In het eenvoudige geval afhankelijk u ook van de Azure-ingebouwde DNS-services om om te zetten hostnamen. Met name wanneer u openbare internetgerichte IP-adressen, die u wilt Netwerkbeveiligingsgroepen Azure gebruiken om te beperken van de open poorten of IP-adresbereiken die verbinding mogen maken in de Azure-subnetten met activa met openbare internetgerichte IP-adressen. Het schema van deze implementatie kan eruitzien als:
  
![Implementatie van ruwe schema voor SAP HANA zonder site-naar-site-verbinding](media/hana-vm-operations/hana-simple-networking2.PNG)
 


## <a name="operations"></a>Bewerkingen
### <a name="backup-and-restore-operations-on-azure-vms"></a>Back-up en herstelbewerkingen op Azure Virtual machines
De mogelijkheden van SAP HANA Backup and Restore worden beschreven in deze documenten:

- [Back-up van SAP HANA-overzicht](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-backup-guide)
- [Back-ups maken van SAP HANA op bestandsniveau](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-backup-file-level)
- [SAP HANA opslag momentopname benchmark](https://docs.microsoft.com/en-us/azure/virtual-machines/workloads/sap/sap-hana-backup-storage-snapshots)



### <a name="start-and-restart-of-vms-containing-sap-hana"></a>Begin- en opnieuw opstarten van virtuele machines met SAP HANA
Een van de kracht van openbare Azure-cloud is het feit dat u alleen worden in rekening gebracht voor de compute-minuten die u besteden. Dit betekent dat als u een met SAP HANA uitgevoerd in deze virtuele machine afsluit, alleen de kosten voor opslag worden gefactureerd gedurende die tijd. Als u de virtuele machine met SAP HANA in deze begint opnieuw, wordt de virtuele machine zal opnieuw worden gestart en u dezelfde IP-adressen (als u hebt geïmplementeerd met statische IP-adressen). 


### <a name="saprouter-enabling-sap-remote-support"></a>SAPRouter inschakelen SAP ondersteuning voor externe
Als u een site-naar-site-verbinding tussen uw on-premises locatie (s) en Azure en u SAP onderdelen uitvoeren die al hebt, is het zeer waarschijnlijk dat u al uitgevoerd SAProuter al. In dit geval wordt is er niets hoeft te doen met SAP HANA-exemplaren die u in Azure implementeren. De VM HANA aangepast, behalve als u wilt onderhouden het privé- en statische IP-adres van de virtuele machine die als host fungeert HANA in de configuratie SAPRouter en hebben de NSG van het subnet hosten (verkeer via poort TCP/IP-poort 3299 toegestaan).

Als u SAP HANA implementeert en u verbinding met Azure via Internet maken en u hebt een SAP-Router in het Vnet dat een VM wordt uitgevoerd met SAP HANA geïnstalleerd, moet u de SAPRouter installeren op een afzonderlijke virtuele machine in het subnet Management zoals hier wordt weergegeven :


![Implementatie van ruwe schema voor SAP HANA zonder site-naar-site-verbinding en SAPRouter](media/hana-vm-operations/hana-simple-networking3.PNG)

U moet SAPRouter installeren in een afzonderlijke virtuele machine en niet in uw Jumpbox VM. De afzonderlijke virtuele machine moet een statisch IP-adres. Om SAPRouter verbinding met de SAPRouter die als host fungeert van SAP (equivalent van het exemplaar SAPRouter die u installeert VM), moet u contact opnemen met SAP voor het ophalen van een IP-adres uit SAP die u nodig hebt voor het configureren van uw SAPRouter-exemplaar. De enige poort die nodig is TCP-poort 3299.
Raadpleeg voor meer informatie over het instellen en onderhouden van ondersteuning voor externe verbindingen via SAPRouter dit [SAP bron](https://support.sap.com/en/tools/connectivity-tools/remote-support.html).

### <a name="high-availability-with-sap-hana-on-azure-native-vms"></a>Hoge beschikbaarheid met SAP HANA op virtuele machines in Azure systeemeigen
SUSE Linux 12 SP1 wordt uitgevoerd en meer recente kunt u een cluster pacemaker heeft instellen met STONITH apparaten voor het instellen van een SAP HANA-configuratie die gebruikmaakt van synchrone replicatie met HANA System Replication en automatische failover. De procedure van setup wordt beschreven in het artikel [hoge beschikbaarheid van SAP HANA op Azure Virtual Machines](https://docs.microsoft.com/en-us/azure/virtual-machines/workloads/sap/sap-hana-high-availability).

 










