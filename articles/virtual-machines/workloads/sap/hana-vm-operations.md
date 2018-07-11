---
title: Bewerkingen van de SAP HANA op Azure | Microsoft Docs
description: Bedieningshandleiding voor SAP HANA-systemen die zijn geïmplementeerd op Azure virtual machines.
services: virtual-machines-linux,virtual-machines-windows
documentationcenter: ''
author: juergent
manager: patfilot
editor: ''
tags: azure-resource-manager
keywords: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 04/24/2018
ms.author: msjuergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 2480ad464f2fc716cf68672387a189aeb92f5737
ms.sourcegitcommit: a06c4177068aafc8387ddcd54e3071099faf659d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/09/2018
ms.locfileid: "37918829"
---
# <a name="sap-hana-on-azure-operations-guide"></a>SAP HANA op Azure-bedieningshandleiding
Dit document bevat richtlijnen voor het werken met SAP HANA-systemen die zijn geïmplementeerd op virtuele machines van Azure (VM's). Dit document is niet bedoeld als vervanging van de standaard SAP-documentatie, waaronder de volgende inhoud:

- [Gebruikershandleiding voor SAP](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.02/en-US/330e5550b09d4f0f8b6cceb14a64cd22.html)
- [SAP-installatiehandleidingen](https://service.sap.com/instguides)
- [SAP-opmerkingen](https://sservice.sap.com/notes)

## <a name="prerequisites"></a>Vereisten
Voor het gebruik van deze handleiding, moet u basiskennis hebt van de volgende Azure-onderdelen:

- [Virtuele machines van Azure](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-manage-vm)
- [Azure-netwerken en virtuele netwerken](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-virtual-network)
- [Azure Storage](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-manage-disks)

Zie voor meer informatie over SAP NetWeaver en andere onderdelen van SAP op Azure, de [SAP op Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/get-started) sectie van de [documentatie voor Azure](https://docs.microsoft.com/azure/).

## <a name="basic-setup-considerations"></a>Overwegingen bij de eenvoudige installatie
De volgende secties beschrijven de basisconfiguratie van overwegingen voor het implementeren van systemen voor SAP HANA op Azure Virtual machines.

### <a name="connect-into-azure-virtual-machines"></a>Verbinding maken met virtuele machines van Azure
Zoals beschreven in de [Planningshandleiding virtuele Azure-machines](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/planning-guide), er zijn twee basismethoden om verbinding te maken in Azure-VM's:

- Verbinding maken via het internet en openbare eindpunten op een VM gaan of op de virtuele machine die SAP HANA wordt uitgevoerd.
- Verbinding maken via een [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal) of Azure [ExpressRoute](https://azure.microsoft.com/services/expressroute/).

Site-naar-site-connectiviteit via VPN of ExpressRoute is nodig voor productiescenario's. Dit type verbinding is ook nodig voor niet-productie scenario's die in productiescenario's waarop SAP-software wordt gebruikt. De volgende afbeelding toont een voorbeeld van cross-site-verbinding:

![Cross-site-connectiviteit](media/virtual-machines-shared-sap-planning-guide/300-vpn-s2s.png)


### <a name="choose-azure-vm-types"></a>Kies typen Azure VM 's
De typen Azure VM's die kunnen worden gebruikt voor productiescenario's worden vermeld in de [SAP-documentatie voor IAAS](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html). Voor niet-productie scenario's is een groter aantal systeemeigen Azure-VM-typen beschikbaar.

>[!NOTE]
> Voor niet-productie scenario's, gebruikt u de VM-typen die worden vermeld in de [SAP-notitie 1928533 #](https://launchpad.support.sap.com/#/notes/1928533). Controleer voor het gebruik van Azure-VM's voor productiescenario's, voor SAP HANA-virtuele machines in de SAP gepubliceerd gecertificeerde [lijst met gecertificeerde IaaS Platforms](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html).

Implementeer de virtuele machines in Azure met behulp van:

- De Azure-portal.
- Azure PowerShell-cmdlets.
- De Azure CLI.

U kunt een volledige SAP HANA-platform is geïnstalleerd op de virtuele machine van Azure-services via ook implementeren de [SAP Cloud platform](https://cal.sap.com/). Het installatieproces wordt beschreven in [implementeren SAP S/4HANA of BW/4HANA on Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/cal-s4h) of met de automatisering die zijn uitgebracht [hier](https://github.com/AzureCAT-GSI/SAP-HANA-ARM).

### <a name="choose-azure-storage-type"></a>Kies Azure Storage-type
Azure biedt twee typen opslag die geschikt zijn voor Azure-VM's waarop SAP HANA:

- [Azure Standard-opslag](https://docs.microsoft.com/azure/virtual-machines/windows/standard-storage)
- [Azure Premium Storage](https://docs.microsoft.com/azure/virtual-machines/windows/premium-storage)

Azure biedt twee methoden voor het implementeren van VHD's op Azure Standard en Premium-opslag. Als de algehele scenario toestaat, profiteren van [Azure beheerde schijf](https://azure.microsoft.com/services/managed-disks/) implementaties.

Voor een lijst van opslagtypen en de bijbehorende Sla in IOP's en opslag doorvoer, raadpleegt u de [Azure-documentatie voor beheerde schijven](https://azure.microsoft.com/pricing/details/managed-disks/).

### <a name="configuring-the-storage-for-azure-virtual-machines"></a>De opslag voor virtuele machines van Azure configureren

Voor zover u SAP HANA-apparaten voor on-premises hebt gekocht, moest u nooit over de i/o-subsystemen en de mogelijkheden van belang omdat de leverancier van het apparaat nodig om ervoor te zorgen dat de minimale vereisten zijn voldaan voor SAP HANA. Als u de Azure-infrastructuur zelf bouwt, moet u ook rekening houden met enkele van deze vereisten te begrijpen ook de configuratievereisten raadzaam om in de volgende secties. Of voor gevallen waarin u de virtuele Machines configureert u SAP HANA uitvoeren op. Sommige van de kenmerken die wordt gevraagd worden de noodzaak om te leidt:

- Inschakelen voor lezen/schrijven-volume op /hana/log van een 250 MB per seconde ten minste 1 MB i/o-grootte
- Activiteit van ten minste 400MB per seconde voor /hana/data voor 16 en 64 MB i/o-grootten voor lezen inschakelen
- Inschakelen van schrijfactiviteiten van minimaal 250MB per seconde voor /hana/data met 16 en 64 MB i/o-grootte

Latentie is opgegeven die weinig opslagruimte essentieel voor DBMS-systemen, zelfs als die, zoals SAP HANA, gegevens in het geheugen houden. Het kritieke pad in de opslag is het meestal om de transactie logboek schrijven van de DBMS-systemen. Maar ook bewerkingen, zoals savepoints schrijven of gegevens in het geheugen laden na crashherstel kan essentieel zijn. Daarom is het verplichte gebruik kunt maken van Azure Premium-schijven voor /hana/data en /hana/log volumes. De minimale om doorvoer te realiseren van hana-/ logboek- en/hana/naar wens door SAP, moet u een RAID 0 bouwen met behulp van MDADM of LVM via meerdere Azure Premium Storage-schijven en de RAID-softwarevolumes als/hana/gegevens en logboekvolumes/hana/gebruiken. Als stripe-voor de RAID 0 de aanbeveling grootten is om te gebruiken:

- 64 kB of 128K voor hana-/ gegevens
- 32 kB voor hana-/ log

> [!NOTE]
> U hoeft niet te configureren van een willekeurig redundantieniveau met behulp van RAID-softwarevolumes omdat Azure Premium en Standard-opslag drie kopieën van een VHD bewaren. Het gebruik van een RAID-volume is uitsluitend bedoeld voor het configureren van volumes die voldoende i/o-doorvoer.

Een aantal Azure-VHD's onder een RAID opgebouwd, is cumulatief vanaf een doorvoer IOP's en opslag. Dus als u een RAID 0 via 3 x P30 Azure Premium Storage-schijven, geeft dit u drie keer de IOPS en drie keer de opslagdoorvoer van één Azure Premium Storage P30-schijf.

De onderstaande aanbevelingen opslaan in cache wordt uitgegaan van de i/o-kenmerken voor SAP HANA die lijst, zoals:

- Er zijn nauwelijks lezen workloads op basis van de gegevensbestanden van HANA. Uitzonderingen zijn grote grootte i/o's na het opnieuw opstarten van de HANA-exemplaar of opnieuw opstarten van de virtuele machine van Azure wanneer gegevens worden geladen in HANA. Een ander hoofdlettergebruik van grotere lezen dat i/o's op basis van bestanden met HANA databaseback-ups kunnen worden. Als gevolg hiervan moeten lezen vooral caching maakt geen zin omdat in de meeste gevallen, alle gegevensvolumes bestand volledig worden gelezen.
- Schrijven op basis van de gegevensbestanden in pieken op basis van door HANA savepoints en HANA crashherstel optreedt. Schrijven savepoints is asynchroon en zijn niet in het bezit van gebruikerstransacties die. Schrijven van gegevens tijdens het crashherstel zijn prestaties kritieke om op te halen van het systeem reageert snel opnieuw. Crashherstel moeten echter in plaats van uitzonderlijke situaties
- Er zijn bijna alle leesbewerkingen van de bestanden van HANA opnieuw uit. Uitzonderingen zijn grote i/o's bij het uitvoeren van transactielogboekback-ups, crashherstel, of in de fase opnieuw opstarten van een HANA-instantie.  
- Belangrijkste belasting op basis van het logboekbestand voor opnieuw uitvoeren van SAP HANA wordt geschreven. Afhankelijk van de aard van de werkbelasting, kunt u i/o's hebben zo klein is als 4 KB of in andere gevallen i/o-grootte van 1 MB of meer. Schrijven latentie op basis van het logboek voor het opnieuw uitvoeren van SAP HANA is een essentiële prestaties.
- Alle schrijfbewerkingen moeten worden opgeslagen op schijf op een betrouwbare manier

Als gevolg van deze waargenomen i/o-patronen door SAP HANA, moet de in cache opslaan voor de verschillende volumes met behulp van Azure Premium Storage worden ingesteld, zoals:

- / hana/gegevens - geen caching
- / hana/log - geen caching - uitzondering voor M-serie (Zie verderop in dit document)
- /Hana/Shared - lezen in cache opslaan


Houd er ook de algehele VM i/o-doorvoer in rekening moet houden bij het vergroten en verkleinen of kiezen voor een virtuele machine. Totale doorvoer van VM-opslag wordt beschreven in het artikel [grootten van virtuele machines geoptimaliseerd voor geheugen](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-memory).

#### <a name="cost-conscious-azure-storage-configuration"></a>Kosten prijsbewuste Azure Storage-configuratie
De volgende tabel toont een configuratie van VM-typen die klanten vaak gebruikt met host SAP HANA op Azure Virtual machines. Er zijn enkele VM-typen die niet voldoet aan alle minimale criteria voor SAP HANA. Maar tot nu toe die virtuele machines is om uit te voeren prima voor niet-productiescenario's. 

> [!NOTE]
> Voor productiescenario's, controleert u of een bepaalde VM-type voor SAP HANA wordt ondersteund door SAP in de [SAP-documentatie voor IAAS](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html).


| VM-SKU | RAM | Met maximaal VM-I/O<br /> Doorvoer | / hana/gegevens en/hana/log<br /> striped met LVM of MDADM | / hana/gedeeld | / Root-volume | / usr/sap | Hana/back-up |
| --- | --- | --- | --- | --- | --- | --- | -- |
| DS14v2 | 128 GiB | 768 MB/s | 3 x P20 | 1 x S20 | 1 x S6 | 1 x S6 | 1 x S15 |
| E16v3 | 128 GiB | 384 MB/s | 3 x P20 | 1 x S20 | 1 x S6 | 1 x S6 | 1 x S15 |
| E32v3 | 256 GiB | 768 MB/s | 3 x P20 | 1 x S20 | 1 x S6 | 1 x S6 | 1 x S20 |
| E64v3 | 443 GiB | 1200 MB/s | 3 x P20 | 1 x S20 | 1 x S6 | 1 x S6 | 1 x S30 |
| GS5 | 448 GiB | 2000 MB/s | 3 x P20 | 1 x S20 | 1 x S6 | 1 x S6 | 1 x S30 |
| M32ts | 192 giB | 500 MB/s | 3 x P20 | 1 x S20 | 1 x S6 | 1 x S6 | 1 x S20 |
| M32ls | 256 GiB | 500 MB/s | 3 x P20 | 1 x S20 | 1 x S6 | 1 x S6 | 1 x S20 |
| M64ls | 512 GiB | 1000 MB/s | 3 x P20 | 1 x S20 | 1 x S6 | 1 x S6 |1 x S30 |
| M64s | 1000 GiB | 1000 MB/s | 2 x P30 | 1 x S30 | 1 x S6 | 1 x S6 |2 x S30 |
| M64ms | 1750 GiB | 1000 MB/s | 3 x P30 | 1 x S30 | 1 x S6 | 1 x S6 | 3 x S30 |
| M128s | 2000 GiB | 2000 MB/s |3 x P30 | 1 x S30 | 1 x S6 | 1 x S6 | 2 x S40 |
| M128ms | 3800 GiB | 2000 MB/s | 5 x P30 | 1 x S30 | 1 x S6 | 1 x S6 | 2 x S50 |


De schijven die zijn aanbevolen voor de kleinere virtuele machine van het type met 3 x P20 oversize de volumes met betrekking tot de aanbevelingen ruimte volgens de [technisch document over opslag voor SAP-TDI](https://www.sap.com/documents/2015/03/74cdb554-5a7c-0010-82c7-eda71af511fa.html). Evenwel de keuze zoals weergegeven in de tabel is gemaakt voor schijfdoorvoer, met voldoende voor SAP HANA. Als u wijzigingen in het volume /hana/backup, die is aangepast voor het bewaren van back-ups die twee keer het volume geheugen vertegenwoordigen, moet u gerust om aan te passen.   
Controleer of de opslagdoorvoer van de voor de verschillende voorgestelde volumes voldoet aan de werkbelasting die u wilt uitvoeren. Als de werkbelasting grotere volumes voor /hana/data en /hana/log vereist, moet u het aantal Azure Premium Storage-VHD's verhogen. Formaat van een volume met meer VHD's dan vermeld verhoogt de IOPS en i/o-doorvoer binnen de grenzen van het type virtuele machine van Azure. 

> [!NOTE]
> De bovenstaande configuraties niet veel voordeel hebben van [virtuele Azure-machine enkele VM SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_6/) omdat deze maakt gebruik van een combinatie van Azure Premium Storage en Azure Standard-opslag. De selectie is echter gekozen om kosten te optimaliseren.


#### <a name="azure-storage-configuration-to-benefit-for-meeting-single-vm-sla"></a>Azure Storage-configuratie om te profiteren om aan de SLA voor één virtuele machine
Als u profiteren wilt van [virtuele Azure-machine enkele VM SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_6/), moet u Azure Premium Storage-VHD's uitsluitend gebruiken.

> [!NOTE]
> Voor productiescenario's, controleert u of een bepaalde VM-type voor SAP HANA wordt ondersteund door SAP in de [SAP-documentatie voor IAAS](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html).

| VM-SKU | RAM | Met maximaal VM-I/O<br /> Doorvoer | / hana/gegevens en/hana/log<br /> striped met LVM of MDADM | / hana/gedeeld | / Root-volume | / usr/sap | Hana/back-up |
| --- | --- | --- | --- | --- | --- | --- | -- |
| DS14v2 | 128 GiB | 768 MB/s | 3 x P20 | 1 x P20 | 1 x P6 | 1 x P6 | 1 x P15 |
| E16v3 | 128 GiB | 384 MB/s | 3 x P20 | 1 x P20 | 1 x P6 | 1 x P6 | 1 x P15 |
| E32v3 | 256 GiB | 768 MB/s | 3 x P20 | 1 x P20 | 1 x P6 | 1 x P6 | 1 x P20 |
| E64v3 | 443 GiB | 1200 MB/s | 3 x P20 | 1 x P20 | 1 x P6 | 1 x P6 | 1 x P30 |
| GS5 | 448 GiB | 2000 MB/s | 3 x P20 | 1 x P20 | 1 x P6 | 1 x P6 | 1 x P30 |
| M32ts | 192 giB | 500 MB/s | 3 x P20 | 1 x P20 | 1 x P6 | 1 x P6 | 1 x P20 |
| M32ls | 256 GiB | 500 MB/s | 3 x P20 | 1 x P20 | 1 x P6 | 1 x P6 | 1 x P20 |
| M64ls | 512 GiB | 1000 MB/s | 3 x P20 | 1 x P20 | 1 x P6 | 1 x P6 | 1 x P30 |
| M64s | 1000 GiB | 1000 MB/s | 2 x P30 | 1 x P30 | 1 x P6 | 1 x P6 |2 x P30 |
| M64ms | 1750 GiB | 1000 MB/s | 3 x P30 | 1 x P30 | 1 x P6 | 1 x P6 | 3 x P30 |
| M128s | 2000 GiB | 2000 MB/s |3 x P30 | 1 x P30 | 1 x P6 | 1 x P6 | 2 x P40 |
| M128ms | 3800 GiB | 2000 MB/s | 5 x P30 | 1 x P30 | 1 x P6 | 1 x P6 | 2 x P50 |


De schijven die zijn aanbevolen voor de kleinere virtuele machine van het type met 3 x P20 oversize de volumes met betrekking tot de aanbevelingen ruimte volgens de [technisch document over opslag voor SAP-TDI](https://www.sap.com/documents/2015/03/74cdb554-5a7c-0010-82c7-eda71af511fa.html). Evenwel de keuze zoals weergegeven in de tabel is gemaakt voor schijfdoorvoer, met voldoende voor SAP HANA. Als u wijzigingen in het volume /hana/backup, die is aangepast voor het bewaren van back-ups die twee keer het volume geheugen vertegenwoordigen, moet u gerust om aan te passen.  
Controleer of de opslagdoorvoer van de voor de verschillende voorgestelde volumes voldoet aan de werkbelasting die u wilt uitvoeren. Als de werkbelasting grotere volumes voor /hana/data en /hana/log vereist, moet u het aantal Azure Premium Storage-VHD's verhogen. Formaat van een volume met meer VHD's dan vermeld verhoogt de IOPS en i/o-doorvoer binnen de grenzen van het type virtuele machine van Azure. 



#### <a name="storage-solution-with-azure-write-accelerator-for-azure-m-series-virtual-machines"></a>Opslagoplossing met Azure Write Accelerator voor virtuele machines van Azure-M-serie
Azure Write Accelerator is een functie die wordt ophalen uitgerold voor M-serie virtuele machines uitsluitend. Als de naam van de staat, is het doel van de functionaliteit voor het verbeteren van i/o-latentie van schrijfbewerkingen ten opzichte van de Azure Premium Storage. Voor SAP HANA mag Write Accelerator is voor het volume /hana/log alleen worden gebruikt. Daarom moeten de configuraties die wordt weergegeven tot nu toe om te worden gewijzigd. De belangrijkste wijziging wordt de verdeling tussen de /hana/data en /hana/log om te kunnen gebruiken Azure Write Accelerator op basis van het volume /hana/log alleen. 

> [!IMPORTANT]
> Er is een SAP HANA-certificering voor Azure-M-serie virtuele machines met Azure Write Accelerator voor het volume /hana/log. Als gevolg hiervan zijn productie scenario-implementaties van SAP HANA op Azure M-serie virtuele machines worden geconfigureerd met Azure Write Accelerator voor het volume /hana/log verwacht.  

> [!NOTE]
> Voor productiescenario's, controleert u of een bepaalde VM-type voor SAP HANA wordt ondersteund door SAP in de [SAP-documentatie voor IAAS](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html).

De aanbevolen configuraties er als volgt uitzien:

| VM-SKU | RAM | Met maximaal VM-I/O<br /> Doorvoer | / hana/gegevens | / hana/log | / hana/gedeeld | / Root-volume | / usr/sap | Hana/back-up |
| --- | --- | --- | --- | --- | --- | --- | --- | -- |
| M32ts | 192 giB | 500 MB/s | 3 x P20 | 2 x P20 | 1 x P20 | 1 x P6 | 1 x P6 |1 x P20 |
| M32ls | 256 GiB | 500 MB/s | 3 x P20 | 2 x P20 | 1 x P20 | 1 x P6 | 1 x P6 |1 x P20 |
| M64ls | 512 GiB | 1000 MB/s | 3 x P20 | 2 x P20 | 1 x P20 | 1 x P6 | 1 x P6 |1 x P30 |
| M64s | 1000 GiB | 1000 MB/s | 4 x P20 | 2 x P20 | 1 x P30 | 1 x P6 | 1 x P6 |2 x P30 |
| M64ms | 1750 GiB | 1000 MB/s | 3 x P30 | 2 x P20 | 1 x P30 | 1 x P6 | 1 x P6 | 3 x P30 |
| M128s | 2000 GiB | 2000 MB/s |3 x P30 | 2 x P20 | 1 x P30 | 1 x P6 | 1 x P6 | 2 x P40 |
| M128ms | 3800 GiB | 2000 MB/s | 5 x P30 | 2 x P20 | 1 x P30 | 1 x P6 | 1 x P6 | 2 x P50 |

Controleer of de opslagdoorvoer van de voor de verschillende voorgestelde volumes voldoet aan de werkbelasting die u wilt uitvoeren. Als de werkbelasting grotere volumes voor /hana/data en /hana/log vereist, moet u het aantal Azure Premium Storage-VHD's verhogen. Formaat van een volume met meer VHD's dan vermeld verhoogt de IOPS en i/o-doorvoer binnen de grenzen van het type virtuele machine van Azure.

Azure Write Accelerator werkt alleen in combinatie met [Azure beheerde schijven](https://azure.microsoft.com/services/managed-disks/). Dus in ten minste de vormen van het volume /hana/log Azure Premium Storage-schijven moeten worden geïmplementeerd als beheerde schijven.

Er zijn beperkingen van Azure Premium Storage VHD's per virtuele machine die kan worden ondersteund door Azure Write Accelerator. De huidige limieten zijn:

- 16-VHD's voor een M128xx VM
- 8-VHD's voor een M64xx VM
- 4-VHD's voor een M32xx VM

Meer gedetailleerde instructies over het inschakelen van Azure Write Accelerator vindt u in het artikel [Write Accelerator](https://docs.microsoft.com/azure/virtual-machines/linux/how-to-enable-write-accelerator).

Details en -beperkingen voor Azure Write Accelerator kunnen worden gevonden in de documentatie van dezelfde.


### <a name="set-up-azure-virtual-networks"></a>Virtuele Azure-netwerken instellen
Als u site-naar-site-verbinding naar Azure via VPN of ExpressRoute hebt, moet u ten minste één hebben [virtueel Azure-netwerk](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview) die is verbonden via een virtuele Gateway naar het VPN of ExpressRoute-circuit. De virtuele Gateway zich bevinden in een subnet in de Azure-netwerk. Voor het installeren van SAP HANA, maakt u twee extra subnetten binnen het virtuele netwerk. Eén subnet als host fungeert voor de virtuele machines om uit te voeren van de SAP HANA-instanties. Het andere subnet voert Jumpbox of beheer-VM's voor het hosten van SAP HANA Studio of andere software voor beheer.

Wanneer u de virtuele machines om uit te voeren van SAP HANA installeert, moeten de virtuele machines:

- Twee virtuele NIC's geïnstalleerd: één NIC verbinding maken met het beheersubnet en één NIC verbinding maken tussen de on-premises netwerk of andere netwerken, en de SAP HANA-instantie in de Azure-VM.
- Statische privé IP-adressen die zijn geïmplementeerd voor zowel virtuele NIC's.

Zie voor een overzicht van de verschillende methoden voor het toewijzen van IP-adressen, [IP-adres adrestypen en toewijzingsmethoden in Azure](https://docs.microsoft.com/azure/virtual-network/virtual-network-ip-addresses-overview-arm). 

Voor VM's het uitvoeren van SAP HANA, dient u werken met statische IP-adressen die zijn toegewezen. Reden is dat bepaalde configuratiekenmerken voor HANA verwijzen naar IP-adressen.

[Azure Netwerkbeveiligingsgroepen (nsg's)](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) worden gebruikt voor het verkeer wordt gerouteerd naar de SAP HANA-exemplaar of de Jumpbox. De nsg's zijn gekoppeld aan de SAP HANA-subnet en het beheersubnet.

De volgende afbeelding toont een overzicht van een schema ruwe implementatie voor SAP HANA:

![Ruwe implementatie-schema voor SAP HANA](media/hana-vm-operations/hana-simple-networking.PNG)


Voor het implementeren van SAP HANA in Azure zonder dat een site-naar-site-verbinding, toegang tot de SAP HANA-exemplaar al is een openbaar IP-adres. Het IP-adres moet worden toegewezen aan de virtuele machine van Azure die de Jumpbox-VM wordt uitgevoerd. In dit eenvoudige scenario, is de implementatie maakt gebruik van Azure services in de ingebouwde DNS-hostnamen omzetten. Azure ingebouwde DNS-services zijn in een meer complexe implementatie waarbij openbare IP-adressen worden gebruikt, met name belangrijk. Gebruik Azure nsg's te beperken van de poorten openen of het IP-adresbereiken die verbinding maken met de Azure subnetten met activa met openbare IP-adressen. De volgende afbeelding toont een ruwe schema voor het implementeren van SAP HANA zonder een site-naar-site-verbinding:
  
![Ruwe implementatie schema voor SAP HANA als een site-naar-site-verbinding](media/hana-vm-operations/hana-simple-networking2.PNG)
 


## <a name="operations-for-deploying-sap-hana-on-azure-vms"></a>Bewerkingen voor het implementeren van SAP HANA op Azure Virtual machines
De volgende secties beschrijven enkele bewerkingen weergegeven die betrekking hebben op de implementatie van systemen voor SAP HANA op Azure Virtual machines.

### <a name="back-up-and-restore-operations-on-azure-vms"></a>Back-up en herstelbewerkingen op Azure Virtual machines
De volgende documenten wordt beschreven hoe u back-up en herstellen van uw SAP HANA-implementatie:

- [Overzicht van back-ups van SAP HANA](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-backup-guide)
- [Back-up van SAP HANA op bestandsniveau](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-backup-file-level)
- [Momentopname van de SAP HANA opslag-benchmark](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-backup-storage-snapshots)


### <a name="start-and-restart-vms-that-contain-sap-hana"></a>Starten en opnieuw opstarten van virtuele machines met SAP HANA
Een prominente functie van de openbare Azure-cloud is dat u alleen voor uw computeromgeving minuten betaalt. Bijvoorbeeld, wanneer u een virtuele machine waarop SAP HANA wordt afgesloten, wordt u gefactureerd voor de kosten voor opslag gedurende die tijd. Een andere functie is beschikbaar als u statische IP-adressen voor uw VM's in uw initiële implementatie opgeven. Wanneer u een virtuele machine met SAP HANA opnieuw start, wordt de virtuele machine wordt opnieuw gestart met de voorgaande IP-adressen. 


### <a name="use-saprouter-for-sap-remote-support"></a>SAProuter gebruiken voor externe ondersteuning voor SAP
Als u een site-naar-site-verbinding tussen uw on-premises locaties en Azure, en u SAP-onderdelen uitvoert, bent u waarschijnlijk al SAProuter uitgevoerd. In dit geval voert u de volgende artikelen voor ondersteuning voor externe:

- Het privé- en statische IP-adres van de virtuele machine die als host fungeert SAP HANA in de configuratie van SAProuter onderhouden.
- Configureer de NSG van het subnet dat als host fungeert voor de HANA VM voor het toestaan van verkeer via TCP/IP-poort 3299.

Als u verbinding met Azure via het internet maakt en u geen een SAP-router voor de virtuele machine met SAP HANA hebt, moet u het onderdeel te installeren. Installeer SAProuter in een afzonderlijke virtuele machine in het beheersubnet. De volgende afbeelding toont een ruwe schema voor het implementeren van SAP HANA met SAProuter en zonder een site-naar-site-verbinding:

![Implementatie-schema voor SAP HANA ruwe zonder dat u een site-naar-site-verbinding en SAProuter](media/hana-vm-operations/hana-simple-networking3.PNG)

Zorg ervoor dat SAProuter installeren in een afzonderlijke virtuele machine en niet in uw Jumpbox-VM. De afzonderlijke virtuele machine moet een statisch IP-adres hebben. Neem contact op met SAP voor een IP-adres voor uw SAProuter verbinding met de SAProuter die wordt gehost door SAP. (De SAProuter die wordt gehost door SAP is het equivalent van de SAProuter-instantie die u op de virtuele machine installeert.) Het IP-adres van het SAP gebruiken om te configureren van uw SAProuter-exemplaar. In de configuratie-instellingen is het alleen de benodigde poort TCP-poort 3299.

Zie voor meer informatie over het instellen en onderhouden van ondersteuning voor externe verbindingen via SAProuter de [SAP-documentatie](https://support.sap.com/en/tools/connectivity-tools/remote-support.html).

### <a name="high-availability-with-sap-hana-on-azure-native-vms"></a>Hoge beschikbaarheid met SAP HANA op Azure systeemeigen VM 's
Als u bij het uitvoeren van SUSE Linux 12 SP1 of hoger, u een Pacemaker-cluster met stonith instellen-apparaten maken kunt. U kunt de apparaten gebruiken voor het instellen van een SAP HANA-configuratie die gebruikmaakt van synchrone replicatie met HANA-Systeemreplicatie en automatische failover. Zie voor meer informatie over de installatieprocedure [hoge beschikbaarheid van SAP HANA-handleiding voor Azure virtual machines](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-availability-overview).
