---
title: SAP HANA-bewerkingen op Azure | Microsoft Docs
description: "Bedieningshandleiding voor SAP HANA-systemen die zijn geïmplementeerd op virtuele machines in Azure."
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
ms.openlocfilehash: 1d6991d40b9bb8543898bbbdc9d7c905dfe11536
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/02/2018
---
# <a name="sap-hana-on-azure-operations-guide"></a>SAP HANA op Azure-bedieningshandleiding
Dit document biedt richtlijnen voor SAP HANA besturingssystemen die worden geïmplementeerd op Azure systeemeigen virtuele machines (VM's). Dit document is niet bedoeld voor het vervangen van de standaard SAP-documentatie, waaronder de volgende inhoud:

- [Beheerdershandleiding voor SAP](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.02/en-US/330e5550b09d4f0f8b6cceb14a64cd22.html)
- [SAP installatiehandleidingen](https://service.sap.com/instguides)
- [SAP-opmerkingen](https://sservice.sap.com/notes)

## <a name="prerequisites"></a>Vereisten
Voor het gebruik van deze handleiding, moet u basiskennis hebt van de volgende Azure onderdelen:

- [Virtuele machines van Azure](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-manage-vm)
- [Azure-netwerken en virtuele netwerken](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-virtual-network)
- [Azure Storage](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-manage-disks)

Zie voor meer informatie over SAP NetWeaver en andere onderdelen SAP op Azure, de [SAP op Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/get-started) sectie van de [Azure-documentatie](https://docs.microsoft.com/azure/).

## <a name="basic-setup-considerations"></a>Basisinstellingen overwegingen
De volgende secties beschrijven basisinstellingen overwegingen voor het implementeren van SAP HANA-systemen op Azure Virtual machines.

### <a name="connect-into-azure-virtual-machines"></a>Verbinding maken in Azure virtuele machines
Zoals beschreven in de [Planningshandleiding virtuele Azure-machines](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/planning-guide), er zijn twee basismethoden om verbinding te maken in Azure Virtual machines:

- Verbinding maken via het internet en de openbare eindpunten op een VM gaan of op de virtuele machine met SAP HANA.
- Verbinding maken via een [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal) of Azure [ExpressRoute](https://azure.microsoft.com/services/expressroute/).

Site-naar-site-connectiviteit via VPN- of ExpressRoute is nodig voor productiescenario's. Dit type verbinding is ook nodig voor niet-productieve scenario's die naar de productie-scenario's waar SAP-software wordt gebruikt. De volgende afbeelding toont een voorbeeld van cross-site-verbinding:

![Cross-site-connectiviteit](media/virtual-machines-shared-sap-planning-guide/300-vpn-s2s.png)


### <a name="choose-azure-vm-types"></a>Virtuele machine van Azure typen kiezen
De virtuele machine van Azure-typen die kunnen worden gebruikt voor productiescenario's worden vermeld in de [SAP-documentatie voor IAAS](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html). Voor scenario's met niet-productieve is een groter aantal systeemeigen Azure VM-typen beschikbaar.

>[!NOTE]
>Gebruik voor niet-productiescenario's de VM-typen die worden vermeld in de [SAP-notitie #1928533](https://launchpad.support.sap.com/#/notes/1928533).

De virtuele machines in Azure implementeren door gebruik te maken:

- De Azure-portal.
- Azure PowerShell-cmdlets.
- De Azure CLI.

U kunt een volledige SAP HANA-platform is geïnstalleerd op de virtuele machine van Azure-services via ook implementeren de [SAP-Cloud-platform](https://cal.sap.com/). Het installatieproces wordt beschreven in [implementeren SAP S/4HANA of BW/4HANA op Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/cal-s4h).

### <a name="choose-azure-storage-type"></a>Azure Storage type kiezen
Azure biedt twee typen opslag die geschikt zijn voor Azure VM's waarop SAP HANA:

- [Azure Standard-opslag](https://docs.microsoft.com/azure/virtual-machines/windows/standard-storage)
- [Azure Premium-opslag](https://docs.microsoft.com/azure/virtual-machines/windows/premium-storage)

Azure biedt twee methoden voor het implementeren voor virtuele harde schijven op Azure Standard en Premium-opslag. Als het algemene scenario toestaat, profiteren van [Azure beheerd schijf](https://azure.microsoft.com/services/managed-disks/) implementaties.

Raadpleeg voor een lijst van de opslagtypen en de bijbehorende Sla de [Azure-documentatie voor beheerde schijven](https://azure.microsoft.com/pricing/details/managed-disks/).

Azure Premium-schijven wordt aanbevolen voor /hana/data en /hana/log volumes. U kunt een RAID LVM bouwen via meerdere schijven met Premium-opslag en de RAID-volumes als /hana/data en /hana/log volumes gebruiken.

De volgende tabel toont een configuratie van VM-typen die klanten wordt meestal gebruikt voor SAP HANA op Azure Virtual machines host:

| VM-SKU | RAM | Met maximaal VM-I/O<br /> Doorvoer | hana/gegevens en hana/logboekbestanden<br /> striped met LVM of MDADM | / hana/gedeeld | / Root-volume | / usr/sap | Hana/back-up |
| --- | --- | --- | --- | --- | --- | --- | -- |
| E16v3 | 128 GiB | 384 MB | 3 x P20 | 1 x S20 | 1 x S6 | 1 x S6 | 1 x S15 |
| E32v3 | 256 giB | 768 MB | 3 x P20 | 1 x S20 | 1 x S6 | 1 x S6 | 1 x S20 |
| E64v3 | 443 giB | 1200 GB | 3 x P20 | 1 x S20 | 1 x S6 | 1 x S6 | 1 x S30 |
| GS5 | 448 giB | 2000 GB | 3 x P20 | 1 x S20 | 1 x S6 | 1 x S6 | 1 x S30 |
| M64s | 1000 giB | 1000 GB | 2 x P30 | 1 x S30 | 1 x S6 | 1 x S6 |2 x S30 |
| M64ms | 1750 giB | 1000 GB | 3 x P30 | 1 x S30 | 1 x S6 | 1 x S6 | 3 x S30 |
| M128s | 2000 giB | 2000 GB |3 x P30 | 1 x S30 | 1 x S6 | 1 x S6 | 2 x S40 |
| M128ms | 3800 giB | 2000 GB | 5 x P30 | 1 x S30 | 1 x S6 | 1 x S6 | 2 x S50 |


> [!NOTE]
> De schijven die zijn aanbevolen voor de kleinere VM met 3 x P20 oversize de volumes met betrekking tot de aanbevelingen ruimte volgens typen de [SAP TDI opslag technisch document](https://www.sap.com/documents/2015/03/74cdb554-5a7c-0010-82c7-eda71af511fa.html). Evenwel de keuze zoals weergegeven in de tabel is gedaan voor voldoende doorvoercapaciteit van de schijf voor SAP HANA. Als u minder i/o-doorvoer nodig hebt, kunt u de keuze van Premium-opslag-schijven voor /hana/data en /hana/log aanpassen. Hetzelfde geldt voor de grootte van het volume /hana/backup, die is aangepast voor het bewaren van back-ups die twee keer het volume geheugen vertegenwoordigen. Als u minder ruimte nodig hebt, kunt u aanpassen. Houd er ook de algehele VM i/o-doorvoer rekening moet houden bij het formaat of bepalen voor een virtuele machine. De algemene VM doorvoer wordt beschreven in het artikel [geoptimaliseerd voor geheugen grootten van virtuele machines](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-memory)  

> [!NOTE]
> Als u profiteren wilt van [virtuele machine van Azure VM SLA eenmalige](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_6/) moet u alle VHD's die worden weergegeven als de Standard-opslag (Sxx) naar Premium-opslag (Pxx) wijzigen. 


### <a name="set-up-azure-virtual-networks"></a>Virtuele netwerken in Azure instellen
Als u site-naar-site-verbinding in Azure via de VPN- of ExpressRoute hebt, moet u ten minste één hebben [virtuele Azure-netwerk](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview) die is verbonden via een virtuele Gateway aan de VPN- of ExpressRoute-circuit. De virtuele Gateway woont in een subnet in het Azure-netwerk. Als u wilt installeren SAP HANA, moet u twee extra subnetten in het virtuele netwerk maken. Eén subnet als host fungeert voor de virtuele machines voor uitvoering van de SAP HANA-exemplaren. Jumpbox of beheer van virtuele machines als host voor SAP HANA Studio of andere software voor beheer, wordt het andere subnet uitgevoerd.

Wanneer u de virtuele machines uitvoeren SAP HANA installeert, moeten de virtuele machines:

- Twee virtuele NIC's geïnstalleerd: één NIC verbinding maken met het management-subnet en één NIC die vanaf de on-premises netwerk of andere netwerken verbinding maken met de SAP HANA-exemplaar in de Azure VM.
- Statische privé IP-adressen die zijn geïmplementeerd voor zowel virtuele NIC's.

Zie voor een overzicht van de verschillende methoden voor het toewijzen van IP-adressen [IP-adres typen en toewijzingsmethoden in Azure](https://docs.microsoft.com/azure/virtual-network/virtual-network-ip-addresses-overview-arm). 

[Azure Netwerkbeveiligingsgroepen (nsg's)](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) worden gebruikt om verkeer dat wordt doorgestuurd naar de SAP HANA-exemplaar of de Jumpbox sturen. Het nsg's zijn gekoppeld aan het SAP HANA-subnet en het subnet Management.

De volgende afbeelding toont een overzicht van een schema ruwe implementatie voor SAP HANA:

![Schema voor SAP HANA ruwe implementatie](media/hana-vm-operations/hana-simple-networking.PNG)


Als u wilt SAP HANA in Azure implementeren zonder een site-naar-site-verbinding, toegang krijgen tot het SAP HANA-exemplaar al een openbaar IP-adres. Het IP-adres moet worden toegewezen aan de virtuele machine in Azure waarop uw Jumpbox VM wordt uitgevoerd. In dit eenvoudige scenario is de implementatie is afhankelijk van Azure geïntegreerde DNS-services hostnamen omzetten. Azure geïntegreerde DNS-services zijn in een complexere implementatie waar de openbare IP-adressen worden gebruikt, met name belangrijk. Gebruik Azure nsg's te beperken van de open poorten of IP-adresbereiken die verbinding kunnen maken in de Azure-subnetten met activa met openbare IP-adressen. De volgende afbeelding toont een ruwe schema voor SAP HANA implementeren zonder een site-naar-site-verbinding:
  
![Implementatie van ruwe schema voor SAP HANA zonder een site-naar-site-verbinding](media/hana-vm-operations/hana-simple-networking2.PNG)
 


## <a name="operations-for-deploying-sap-hana-on-azure-vms"></a>Bewerkingen voor het implementeren van SAP HANA op Azure Virtual machines
De volgende secties worden enkele van de bewerkingen die betrekking hebben op de implementatie van SAP HANA-systemen op Azure Virtual machines beschreven.

### <a name="back-up-and-restore-operations-on-azure-vms"></a>Back-up en herstelbewerkingen op Azure Virtual machines
De volgende documenten wordt beschreven hoe u back-up en herstellen van uw SAP HANA-implementatie:

- [Overzicht van back-ups van SAP HANA](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-backup-guide)
- [SAP HANA-back-up op bestandsniveau](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-backup-file-level)
- [SAP HANA opslag momentopname benchmark](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-backup-storage-snapshots)


### <a name="start-and-restart-vms-that-contain-sap-hana"></a>Start en opnieuw opstarten van virtuele machines die SAP HANA bevatten
Een belangrijk onderdeel van de openbare Azure-cloud is dat u in rekening alleen uw computing minuten gebracht bent. Bijvoorbeeld, wanneer u een virtuele machine waarop SAP HANA wordt afgesloten, bent u gefactureerd alleen voor de opslagkosten gedurende die tijd. Een andere functie is beschikbaar wanneer u statische IP-adressen voor uw virtuele machines in uw initiële implementatie opgeven. Wanneer u opnieuw een VM die SAP HANA heeft, de virtuele machine wordt opnieuw gestart met de eerdere IP-adressen. 


### <a name="use-saprouter-for-sap-remote-support"></a>SAProuter gebruiken voor externe SAP-ondersteuning
Als u een site-naar-site-verbinding tussen uw lokale locaties en Azure hebt en u SAP onderdelen uitvoert, bent u waarschijnlijk al SAProuter uitgevoerd. In dit geval voert u de volgende items voor externe ondersteuning:

- Het privé- en statische IP-adres van de virtuele machine die als host fungeert SAP HANA in de configuratie SAProuter onderhouden.
- Configureer het NSG van het subnet dat als host fungeert voor de VM HANA voor verkeer via TCP/IP-poort 3299.

Als u verbinding met Azure via internet maakt en u een SAP-router voor de virtuele machine met SAP HANA hoeft, moet u het onderdeel te installeren. SAProuter installeren op een afzonderlijke virtuele machine in het subnet Management. De volgende afbeelding toont een ruwe schema voor SAP HANA implementeren zonder een site-naar-site-verbinding en met SAProuter:

![Implementatie-schema voor SAP HANA ruwe zonder een site-naar-site-verbinding en SAProuter](media/hana-vm-operations/hana-simple-networking3.PNG)

Zorg ervoor dat SAProuter installeren in een afzonderlijke virtuele machine en niet in uw Jumpbox VM. De afzonderlijke virtuele machine moet een statisch IP-adres hebben. Neem contact op met SAP voor een IP-adres voor uw SAProuter verbinding met de SAProuter die wordt gehost door SAP. (De SAProuter die wordt gehost door SAP is het equivalent van het exemplaar SAProuter die u op de virtuele machine installeert.) Het IP-adres van het SAP gebruiken voor het configureren van uw SAProuter-exemplaar. In de configuratie-instellingen is het alleen de benodigde poort TCP-poort 3299.

Zie voor meer informatie over het instellen en onderhouden van ondersteuning voor externe verbindingen via SAProuter de [SAP documentatie](https://support.sap.com/en/tools/connectivity-tools/remote-support.html).

### <a name="high-availability-with-sap-hana-on-azure-native-vms"></a>Hoge beschikbaarheid met SAP HANA op virtuele machines in Azure systeemeigen
Als u werkt met SUSE Linux 12 SP1 of later kunt u een cluster pacemaker heeft met STONITH apparaten kunt instellen. U kunt de apparaten gebruiken voor het instellen van een SAP HANA-configuratie die gebruikmaakt van synchrone replicatie met HANA System Replication en automatische failover. Zie voor meer informatie over de installatieprocedure [hoge beschikbaarheid van SAP HANA op virtuele machines in Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-high-availability).
