---
title: Configuraties van SAP HANA-infrastructuur en bewerkingen op Azure | Microsoft Docs
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
ms.date: 07/27/2018
ms.author: msjuergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 98c7bd5daf3b84499e8e31c0a7a2da612834b83e
ms.sourcegitcommit: 9819e9782be4a943534829d5b77cf60dea4290a2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/06/2018
ms.locfileid: "39521979"
---
# <a name="sap-hana-infrastructure-configurations-and-operations-on-azure"></a>Configuraties van SAP HANA-infrastructuur en bewerkingen op Azure
Dit document biedt richtlijnen voor het configureren van Azure-infrastructuur en SAP HANA besturingssystemen die zijn geïmplementeerd op virtuele machines van Azure (VM's). Het document bevat ook informatie over de configuratie voor SAP HANA scale-out voor de M128s VM-SKU. Dit document is niet bedoeld als vervanging van de standaard SAP-documentatie, waaronder de volgende inhoud:

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
> Voor niet-productie scenario's, gebruikt u de VM-typen die worden vermeld in de [SAP-notitie 1928533 #](https://launchpad.support.sap.com/#/notes/1928533). Controleer voor het gebruik van Azure-VM's voor productiescenario's, voor SAP HANA-virtuele machines in de SAP gepubliceerd gecertificeerde [lijst met gecertificeerde IaaS Platforms](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html#categories=Microsoft%20Azure).

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

Voor zover u SAP HANA-apparaten voor on-premises hebt gekocht, moest u nooit over de i/o-subsystemen en de mogelijkheden ervan. Omdat de leverancier van het apparaat nodig om ervoor te zorgen dat de minimale vereisten zijn voldaan voor SAP HANA. Als u de Azure-infrastructuur zelf maakt, ook moet u rekening houden met enkele van deze vereisten. En ook informatie over de configuratievereisten voorgesteld in de volgende secties. Of voor gevallen waarin u de virtuele Machines configureert u SAP HANA uitvoeren op. Sommige van de kenmerken die wordt gevraagd worden de noodzaak om te leidt:

- Inschakelen van lezen/schrijven-volume op **/hana/log** van een 250 MB per seconde ten minste 1 MB i/o-grootte
- Activiteit van ten minste 400 MB per seconde voor voor lezen inschakelen **/hana/gegevens** voor 16 en 64 MB i/o-grootten
- Inschakelen van schrijfactiviteiten van minimaal 250 MB per seconde voor **/hana/gegevens** met 16 en 64 MB i/o-grootte

Latentie is opgegeven die weinig opslagruimte essentieel voor DBMS-systemen, zelfs als deze DBMS-systemen, zoals SAP HANA, blijven de gegevens in het geheugen. Het kritieke pad in de opslag is het meestal om de transactie logboek schrijven van de DBMS-systemen. Maar ook bewerkingen, zoals savepoints schrijven of gegevens in het geheugen laden na crashherstel kan essentieel zijn. Daarom is het verplichte gebruik kunt maken van Azure Premium-schijven voor **/hana/gegevens** en **/hana/log** volumes. Om de minimale doorvoer van te realiseren **/hana/log** en **/hana/gegevens** naar wens door SAP, moet u een RAID 0 bouwen met behulp van MDADM of LVM via meerdere Azure Premium Storage-schijven. En gebruik van de RAID-volumes als **/hana/gegevens** en **/hana/log** volumes. Als stripe-voor de RAID 0 de aanbeveling grootten is om te gebruiken:

- 64 KB of 128 KB voor   **/hana/gegevens**
- 32 KB voor   **/hana/log**

> [!NOTE]
> U hoeft niet te configureren van een willekeurig redundantieniveau met behulp van RAID-softwarevolumes omdat Azure Premium en Standard-opslag drie kopieën van een VHD bewaren. Het gebruik van een RAID-volume is uitsluitend bedoeld voor het configureren van volumes die voldoende i/o-doorvoer.

Een aantal Azure-VHD's onder een RAID opgebouwd, is cumulatief vanaf een doorvoer IOP's en opslag. Dus als u een RAID 0 via 3 x P30 Azure Premium Storage-schijven, geeft dit u drie keer de IOPS en drie keer de opslagdoorvoer van één Azure Premium Storage P30-schijf.

De onderstaande aanbevelingen opslaan in cache wordt uitgegaan van de i/o-kenmerken voor SAP HANA die lijst, zoals:

- Er zijn nauwelijks lezen workloads op basis van de gegevensbestanden van HANA. Uitzonderingen zijn grote grootte i/o's na het opnieuw opstarten van de HANA-instantie of wanneer gegevens worden geladen in HANA. Een ander hoofdlettergebruik van grotere lezen dat i/o's op basis van bestanden met HANA databaseback-ups kunnen worden. Als gevolg hiervan moeten lezen vooral caching maakt geen zin omdat in de meeste gevallen, alle gegevensvolumes bestand volledig worden gelezen.
- Schrijven op basis van de gegevensbestanden in pieken op basis van door HANA savepoints en HANA crashherstel optreedt. Schrijven savepoints is asynchroon en zijn niet in het bezit van gebruikerstransacties die. Schrijven van gegevens tijdens het crashherstel zijn prestaties kritieke om op te halen van het systeem reageert snel opnieuw. Crashherstel moeten echter in plaats van uitzonderlijke situaties
- Er zijn bijna alle leesbewerkingen van de bestanden van HANA opnieuw uit. Uitzonderingen zijn grote i/o's bij het uitvoeren van transactielogboekback-ups, crashherstel, of in de fase opnieuw opstarten van een HANA-instantie.  
- Belangrijkste belasting op basis van het logboekbestand voor opnieuw uitvoeren van SAP HANA wordt geschreven. Afhankelijk van de aard van de werkbelasting, kunt u i/o's hebben zo klein is als 4 KB of in andere gevallen i/o-grootte van 1 MB of meer. Schrijven latentie op basis van het logboek voor het opnieuw uitvoeren van SAP HANA is een essentiële prestaties.
- Alle schrijfbewerkingen moeten worden opgeslagen op schijf op een betrouwbare manier

Als gevolg van deze waargenomen i/o-patronen door SAP HANA, moet de in cache opslaan voor de verschillende volumes met behulp van Azure Premium Storage worden ingesteld, zoals:

- **/ hana/gegevens** -geen caching
- **/ hana/log** - geen caching - uitzondering voor M-serie (Zie verderop in dit document)
- **/ hana/gedeelde** - lezen in cache opslaan


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


De schijven die zijn aanbevolen voor de kleinere virtuele machine van het type met 3 x P20 oversize de volumes met betrekking tot de aanbevelingen ruimte volgens de [technisch document over opslag voor SAP-TDI](https://www.sap.com/documents/2015/03/74cdb554-5a7c-0010-82c7-eda71af511fa.html). Evenwel de keuze zoals weergegeven in de tabel is gemaakt voor schijfdoorvoer, met voldoende voor SAP HANA. Als u wijzigingen in de **/hana/back-up**p-volume, die is aangepast voor het bewaren van back-ups die twee keer het volume geheugen vertegenwoordigen, gerust om aan te passen.   
Controleer of de opslagdoorvoer van de voor de verschillende voorgestelde volumes voldoet aan de werkbelasting die u wilt uitvoeren. Als de workload grotere volumes voor **/hana/gegevens** en **/hana/log**, moet u het aantal Azure Premium Storage-VHD's verhogen. Formaat van een volume met meer VHD's dan vermeld verhoogt de IOPS en i/o-doorvoer binnen de grenzen van het type virtuele machine van Azure. 

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


De schijven die zijn aanbevolen voor de kleinere virtuele machine van het type met 3 x P20 oversize de volumes met betrekking tot de aanbevelingen ruimte volgens de [technisch document over opslag voor SAP-TDI](https://www.sap.com/documents/2015/03/74cdb554-5a7c-0010-82c7-eda71af511fa.html). Evenwel de keuze zoals weergegeven in de tabel is gemaakt voor schijfdoorvoer, met voldoende voor SAP HANA. Als u wijzigingen in de **/hana/back-up** volume, die is aangepast voor het bewaren van back-ups die twee keer het volume geheugen vertegenwoordigen, gerust om aan te passen.  
Controleer of de opslagdoorvoer van de voor de verschillende voorgestelde volumes voldoet aan de werkbelasting die u wilt uitvoeren. Als de workload grotere volumes voor **/hana/gegevens** en **/hana/log**, moet u het aantal Azure Premium Storage-VHD's verhogen. Formaat van een volume met meer VHD's dan vermeld verhoogt de IOPS en i/o-doorvoer binnen de grenzen van het type virtuele machine van Azure. 


#### <a name="storage-solution-with-azure-write-accelerator-for-azure-m-series-virtual-machines"></a>Opslagoplossing met Azure Write Accelerator voor virtuele machines van Azure-M-serie
Azure Write Accelerator is een functie die wordt ophalen uitgerold voor M-serie virtuele machines uitsluitend. Als de naam van de staat, is het doel van de functionaliteit voor het verbeteren van i/o-latentie van schrijfbewerkingen ten opzichte van de Azure Premium Storage. Voor SAP HANA, Write Accelerator moet worden gebruikt voor de **/hana/log** alleen volume. Daarom moeten de configuraties die wordt weergegeven tot nu toe om te worden gewijzigd. De belangrijkste wijziging wordt de verdeling tussen de **/hana/gegevens** en **/hana/log** om te kunnen gebruiken Azure Write Accelerator op basis van de **/hana/log** alleen volume. 

> [!IMPORTANT]
> SAP HANA-certificering voor Azure-M-serie virtuele machines is uitsluitend in combinatie met Azure Write Accelerator voor de **/hana/log** volume. Als gevolg hiervan productie scenario-implementaties van SAP HANA op Azure M-serie virtuele machines worden verwacht kan worden geconfigureerd met Azure Write Accelerator voor de **/hana/log** volume.  

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

Controleer of de opslagdoorvoer van de voor de verschillende voorgestelde volumes voldoet aan de werkbelasting die u wilt uitvoeren. Als de workload grotere volumes voor **/hana/gegevens** en **/hana/log**, moet u het aantal Azure Premium Storage-VHD's verhogen. Formaat van een volume met meer VHD's dan vermeld verhoogt de IOPS en i/o-doorvoer binnen de grenzen van het type virtuele machine van Azure.

Azure Write Accelerator werkt alleen in combinatie met [Azure beheerde schijven](https://azure.microsoft.com/services/managed-disks/). Dus in ten minste de Azure Premium Storage-schijven die de **/hana/log** volume moet worden geïmplementeerd als beheerde schijven.

Er zijn beperkingen van Azure Premium Storage VHD's per virtuele machine die kan worden ondersteund door Azure Write Accelerator. De huidige limieten zijn:

- 16-VHD's voor een M128xx VM
- 8-VHD's voor een M64xx VM
- 4-VHD's voor een M32xx VM

Meer gedetailleerde instructies over het inschakelen van Azure Write Accelerator vindt u in het artikel [Write Accelerator](https://docs.microsoft.com/azure/virtual-machines/linux/how-to-enable-write-accelerator).

Details en -beperkingen voor Azure Write Accelerator kunnen worden gevonden in de documentatie van dezelfde.



### <a name="set-up-azure-virtual-networks"></a>Virtuele Azure-netwerken instellen
Als u site-naar-site-verbinding naar Azure via VPN of ExpressRoute hebt, moet u ten minste één Azure-netwerk dat is verbonden via een virtuele Gateway naar het VPN of ExpressRoute-circuit hebben. In eenvoudige implementaties kan de virtuele Gateway worden geïmplementeerd in een subnet van de Azure-netwerk (VNet) dat als host fungeert voor de SAP HANA-instanties. Voor het installeren van SAP HANA, maakt u twee extra subnetten binnen het Azure-netwerk. Eén subnet als host fungeert voor de virtuele machines om uit te voeren van de SAP HANA-instanties. Het andere subnet voert Jumpbox of beheer-VM's voor het hosten van SAP HANA Studio of andere software voor beheer van de toepassingssoftware van uw.

Wanneer u de virtuele machines om uit te voeren van SAP HANA installeert, moeten de virtuele machines:

- Twee virtuele NIC's geïnstalleerd: één NIC verbinding maken met het beheersubnet en één NIC verbinding maken tussen de on-premises netwerk of andere netwerken, en de SAP HANA-instantie in de Azure-VM.
- Statische privé IP-adressen die zijn geïmplementeerd voor zowel virtuele NIC's.

Voor implementaties die zijn onmisbaar, moet u echter een virtueel datacenter netwerkarchitectuur maken in Azure. Deze architectuur wordt aanbevolen de scheiding van de Azure-VNet-Gateway die verbinding maakt on-premises in een afzonderlijke Azure-VNet. Deze apart VNet moet al het verkeer dat een naar on-premises hosten of tot het internet. Deze benadering kunt u software voor controle en logboekregistratie-verkeer dat het virtuele datacenter in Azure in deze afzonderlijke hub VNet invoert te implementeren. Daarom moet u een VNet die als host fungeert voor de software en configuraties die is gekoppeld aan in- en uitgaand verkeer naar uw Azure-implementatie.

De artikelen [Azure Virtual Datacenter: een Netwerkperspectief](https://docs.microsoft.com/azure/networking/networking-virtual-datacenter) en [Azure Virtual Datacenter en de Enterprise-Controlelaag](https://docs.microsoft.com/azure/architecture/vdc/) geven u meer informatie over de virtual datacenter-aanpak en gerelateerde Azure VNet-ontwerp.


>[!NOTE]
>Verkeer tussen een VNet-hub en spoke met behulp van VNet [Azure VNet-peering](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview) onderwerp is van aanvullende [kosten](https://azure.microsoft.com/pricing/details/virtual-network/). Op basis van de kosten, moet u mogelijk overwegen compromissen tussen een strikte hub en spoke-netwerkontwerp uitgevoerd en op meerdere [Azure ExpressRoute-Gateways](https://docs.microsoft.com/azure/expressroute/expressroute-about-virtual-network-gateways) dat u verbinding met 'knooppunten' maken om het overslaan van VNet-peering. Echter, Azure ExpressRoute-Gateways geïntroduceerd extra [kosten](https://azure.microsoft.com/pricing/details/vpn-gateway/) ook. U kunt ook extra kosten voor software van derden die u voor het netwerkverkeer logboekregistratie gebruikt, controleren en bewaken tegenkomen. Afhankelijk van de kosten voor het uitwisselen van gegevens via VNet-peering op de een-zijde en de kosten die zijn gemaakt door aanvullende Azure ExpressRoute-Gateways en aanvullende softwarelicenties, kunt u beslissen voor micro-segmentatie binnen een VNet met behulp van subnetten als isolatie-eenheid in plaats van vnet's.


Zie voor een overzicht van de verschillende methoden voor het toewijzen van IP-adressen, [IP-adres adrestypen en toewijzingsmethoden in Azure](https://docs.microsoft.com/azure/virtual-network/virtual-network-ip-addresses-overview-arm). 

Voor VM's het uitvoeren van SAP HANA, dient u werken met statische IP-adressen die zijn toegewezen. Reden is dat bepaalde configuratiekenmerken voor HANA verwijzen naar IP-adressen.

[Azure Netwerkbeveiligingsgroepen (nsg's)](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) worden gebruikt voor het verkeer wordt gerouteerd naar de SAP HANA-exemplaar of de jumpbox. De nsg's en uiteindelijk [Toepassingsbeveiligingsgroepen](https://docs.microsoft.com/azure/virtual-network/security-overview#application-security-groups) zijn gekoppeld aan de SAP HANA-subnet en het beheersubnet.

De volgende afbeelding toont een overzicht van een schema ruwe implementatie voor SAP HANA een hub en spoke-VNet-architectuur te volgen:

![Ruwe implementatie-schema voor SAP HANA](media/hana-vm-operations/hana-simple-networking.PNG)

Voor het implementeren van SAP HANA in Azure zonder dat een site-naar-site-verbinding, wilt u nog steeds afschermen van het SAP HANA-exemplaar van het openbare internet en te verbergen achter een proxy voor doorsturen. In dit eenvoudige scenario, is de implementatie maakt gebruik van Azure services in de ingebouwde DNS-hostnamen omzetten. Azure ingebouwde DNS-services zijn in een meer complexe implementatie waarbij openbare IP-adressen worden gebruikt, met name belangrijk. Gebruik Azure nsg's en [NVA's in Azure](https://azure.microsoft.com/solutions/network-appliances/) beheren, controleert de routering via internet in uw Azure-VNet-architectuur in Azure. De volgende afbeelding toont een ruwe schema voor het implementeren van SAP HANA zonder een site-naar-site-verbinding in een hub en spoke-architectuur VNet:
  
![Ruwe implementatie schema voor SAP HANA als een site-naar-site-verbinding](media/hana-vm-operations/hana-simple-networking2.PNG)
 

Een andere beschrijving over het NVA's in Azure gebruiken voor toegang tot beheren en te controleren van Internet zonder de hub- en -spokenetwerktopologie VNet-architectuur kunt u vinden in het artikel [maximaal beschikbare virtuele netwerkapparaten implementeren](https://docs.microsoft.com/azure/architecture/reference-architectures/dmz/nva-ha).


## <a name="configuring-azure-infrastructure-for-sap-hana-scale-out"></a>Configureren van Azure-infrastructuur voor SAP HANA-uitschalen
Microsoft heeft een M-serie VM-SKU die is gecertificeerd voor de configuratie van een SAP HANA-scale-out. De VM-type M128s is gecertificeerd voor een scale-out van maximaal 16 knooppunten. Controleren op wijzigingen in scale-out-certificeringen voor SAP HANA op Azure Virtual machines, [lijst met gecertificeerde IaaS Platforms](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html#categories=Microsoft%20Azure).

De minimale OS-versies voor het implementeren van scale-out-configuraties in Azure VM's zijn:

- SUSE Linux 12 SP3
- Red hat Linux 7.4

Van de certificering van de scale-out 16 knooppunten

- Een knooppunt is het hoofdknooppunt
- Een maximum van 15 knooppunten zijn worker-knooppunten

>[!NOTE]
>In Azure VM scale-out implementaties is geen mogelijkheid voor het gebruik van een stand-by-knooppunt
>

De reden voor het niet meer kunt configureren van een stand-by-knooppunt zijn tweeledig:

- Azure heeft op dit moment geen systeemeigen NFS-service. Als gevolg hiervan moeten de NFS-shares kan worden geconfigureerd met behulp van de functionaliteit van andere leveranciers.
- Geen van de NFS-configuraties van derden kunnen om te voldoen aan de criteria van de latentie van opslag voor SAP HANA met hun oplossingen die zijn geïmplementeerd op Azure.

Als gevolg hiervan **/hana/gegevens** en **/hana/log** volumes kunnen niet worden gedeeld. Niet delen van deze volumes van de afzonderlijke knooppunten, voorkomt u dat het gebruik van een stand-by-SAP HANA-knooppunt in de configuratie van een scale-out.

Het ontwerp van de basis voor een enkel knooppunt in een scale-out-configuratie is als gevolg hiervan gaan er als volgt uitzien:

![Basisprincipes van de scale-out van één knooppunt](media/hana-vm-operations/scale-out-basics.PNG)

De basisconfiguratie van een VM-knooppunt voor SAP HANA scale-out ziet eruit zoals:

- Voor **/hana/gedeelde**, u een maximaal beschikbare NFS-cluster op basis van SUSE Linux 12 SP3 bouwen. Dit cluster fungeert als host voor de **/hana/gedeelde** NFS share (s) van uw scale-out-configuratie en SAP NetWeaver of BW/4HANA Central Services. Documentatie voor het bouwen van een dergelijke configuratie is beschikbaar in het artikel [hoge beschikbaarheid voor NFS op Azure VM's in SUSE Linux Enterprise Server](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-nfs)
- Alle volumes op andere schijven zijn **niet** gedeeld tussen de verschillende knooppunten en worden **niet** op basis van NFS. Installatie van configuraties en stappen voor het scale-out HANA installaties met niet-gedeelde **/hana/gegevens** en **/hana/log** verderop in dit document wordt geleverd.

>[!NOTE]
>De maximaal beschikbare NFS-cluster wordt zoals weergegeven in de afbeeldingen tot nu toe ondersteund met SUSE Linux alleen. Een maximaal beschikbare NFS-oplossing op basis van Red Hat wordt later opnieuw te worden gesteld.

Grootte van de volumes voor de knooppunten is dezelfde als scale-up, met uitzondering van **/hana/gedeelde**. Voor de SKU van de virtuele machine M128s de voorgestelde grootte en de typen er als volgt uitzien:

| VM-SKU | RAM | Met maximaal VM-I/O<br /> Doorvoer | / hana/gegevens | / hana/log | / Root-volume | / usr/sap | Hana/back-up |
| --- | --- | --- | --- | --- | --- | --- | --- |
| M128s | 2000 GiB | 2000 MB/s |3 x P30 | 2 x P20 | 1 x P6 | 1 x P6 | 2 x P40 |


Controleer of de opslagdoorvoer van de voor de verschillende voorgestelde volumes voldoet aan de werkbelasting die u wilt uitvoeren. Als de workload grotere volumes voor **/hana/gegevens** en **/hana/log**, moet u het aantal Azure Premium Storage-VHD's verhogen. Formaat van een volume met meer VHD's dan vermeld verhoogt de IOPS en i/o-doorvoer binnen de grenzen van het type virtuele machine van Azure. Gelden ook Azure Write Accelerator voor de schijven dat formulier de **/hana/log** volume.
 
In het document [opslagvereisten voor SAP HANA TDI](https://www.sap.com/documents/2015/03/74cdb554-5a7c-0010-82c7-eda71af511fa.html), een formule met de naam die de grootte van definieert de **/hana/gedeelde** volume voor scale-out als de geheugengrootte van een knooppunt één werknemer per 4 worker-knooppunten.

Ervan uitgaande dat u rekening houden met de SAP HANA scale-out gecertificeerde M128s Azure VM met ongeveer 2 TB geheugen, worden de SAP-aanbevelingen samengevat, zoals:

- Een hoofdknooppunt en maximaal vier worker-knooppunt, de **/hana/gedeelde** volume zou moeten zijn van de grootte van 2 TB. 
- Een hoofdknooppunt en vijf tot acht worker-knooppunten, de grootte van **/hana/gedeelde** moet 4 TB. 
- Een hoofdknooppunt en 9 12 worker-knooppunten, een grootte van 6 TB opslagruimte voor **/hana/gedeelde** is vereist. 
- Een hoofdknooppunt en gebruik te maken tussen 12 en 15 worker-knooppunten, u bent op te geven een **/hana/gedeelde** volume dat is 8 TB in grootte.

Het andere belangrijke ontwerp dat wordt weergegeven in de afbeeldingen van de configuratie met één knooppunt voor een scale-out SAP HANA-virtuele machine is het VNet of beter de subnetconfiguratie. SAP raadt een scheiding van de client-/ toepassingsbeleid gerichte verkeer van de communicatie tussen de HANA-knooppunten. Zoals wordt weergegeven in de afbeeldingen, wordt dit doel wordt bereikt door twee verschillende vnic's die zijn gekoppeld aan de virtuele machine. Beide vnic's zich in verschillende subnetten, hebt u twee verschillende IP-adressen. U kunt vervolgens de verkeersstroom beheren met regels voor doorsturen met nsg's of de gebruiker gedefinieerde routes.

Met name in Azure, moet u er geen middelen en methoden om af te dwingen quality of service en quota's voor specifieke vnic's zijn. Als gevolg hiervan de scheiding van client-/ toepassingsbeleid gerichte en intraknooppuntcommunicatie communicatie niet wordt geopend elke mogelijkheden om één verkeer stream prioriteit boven de andere. In plaats daarvan blijft de scheiding tussen een meting van de beveiliging in de intra-node-communicatie van de scale-out-configuraties afscherming.  

>[!IMPORTANT]
>SAP raadt het scheiden van netwerkverkeer naar de toepassing/client-zijde en intraknooppuntcommunicatie verkeer zoals beschreven in dit document. Daarom een architectuur plaatsen in plaats van zoals wordt weergegeven in de laatste afbeeldingen wordt sterk aanbevolen.
>

Vanaf een netwerk standpunt de minimale vereiste netwerkarchitectuur zou er als volgt uitzien:

![Basisprincipes van de scale-out van één knooppunt](media/hana-vm-operations/scale-out-networking-overview.PNG)

De limieten ondersteund tot nu toe zijn 15 werknemer als aanvulling op het één hoofdknooppunt.

Vanuit een opslag-oogpunt van de opslagarchitectuur zou er als volgt uitzien:


![Basisprincipes van de scale-out van één knooppunt](media/hana-vm-operations/scale-out-storage-overview.PNG)

De **/hana/gedeelde** volume bevindt zich op de maximaal beschikbare configuratie van de NFS-share. Terwijl alle andere stations zijn 'lokaal' gekoppeld aan de afzonderlijke virtuele machines. 

### <a name="highly-available-nfs-share"></a>Maximaal beschikbare NFS-share
De maximaal beschikbare NFS-cluster tot nu toe werkt samen met SUSE Linux alleen. Het document [hoge beschikbaarheid voor NFS op Azure VM's in SUSE Linux Enterprise Server](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-nfs) wordt beschreven hoe u jenkins instelt. Als u het NFS-cluster niet met andere HANA configuraties buiten het azure VNet met de SAP HANA-instanties delen, kunt u dit in hetzelfde VNet installeren. Installeer het in een eigen subnet en zorg ervoor dat het subnet toegang hebben tot willekeurige niet al het verkeer. In plaats daarvan wilt u het verkeer op dat subnet naar het IP-adressen van de virtuele machine die het verkeer om te worden uitgevoerd beperken **/hana/gedeelde** volume.

Met betrekking tot de vNIC van een HANA scale-out-virtuele machine die moet routeren de **/hana/gedeelde** verkeer, de aanbevelingen zijn:

- Sinds het verkeer naar **/hana/gedeelde** is normaal, rondsturen via de vNIC die is toegewezen aan de clientnetwerk in de minimale configuratie
- Uiteindelijk, voor het verkeer naar **/hana/gedeelde**, het implementeren van een derde subnet in het VNet die u implementeert de SAP HANA uitbreidbare configuratie en het toewijzen van een derde vNIC die wordt gehost in dat subnet. Gebruik de derde vNIC en de bijbehorende IP-adres voor het verkeer naar de NFS-share. Vervolgens kunt u afzonderlijke toegang en routeringsregels toepassen.

>[!IMPORTANT]
>Netwerkverkeer tussen de VM's waarop SAP HANA op een uitbreidbare manier geïmplementeerd en de maximaal beschikbare NFS kan onder geen enkele omstandigheid worden gerouteerd via een [NVA](https://azure.microsoft.com/solutions/network-appliances/) of vergelijkbare virtuele apparaten. Terwijl Azure nsg's geen dergelijke apparaten zijn. Controleer uw routeringsregels om ervoor te zorgen dat de NVA's of vergelijkbare virtuele apparaten zijn detoured wanneer toegang krijgen tot de maximaal beschikbare NFS-share van de virtuele machines met SAP HANA.
> 

Als u delen van de maximaal beschikbare NFS-cluster tussen SAP HANA-configuraties wilt, verplaatst u alle die HANA-configuraties in hetzelfde VNet. 
 

### <a name="installing-sap-hana-scale-out-n-azure"></a>SAP HANA scale-out n Azure installeren
U installeert een scale-out SAP-configuratie, die u wilt uitvoeren van ruwe stappen:

- Implementatie van nieuwe of aanpassen van de nieuwe Azure-VNet-infrastructuur
- Implementatie van de nieuwe beheerde virtuele machines met behulp van Azure Premium Storage-volumes
- Implementatie van een nieuwe of een bestaande maximaal beschikbare NFS-cluster aanpassen
- Aanpassing van de routering van om ervoor te zorgen dat bijvoorbeeld intraknooppuntcommunicatie communicatie tussen VM's wordt niet doorgestuurd via een [NVA](https://azure.microsoft.com/solutions/network-appliances/). Hetzelfde geldt voor verkeer tussen de virtuele machines en het maximaal beschikbare NFS-cluster.
- Installeer het hoofdknooppunt van SAP HANA.
- Parameters voor de configuratie van het hoofdknooppunt van SAP HANA aanpassen
- Ga door met de installatie van de SAP HANA worker-knooppunten

#### <a name="installation-of-sap-hana-in-scale-out-configuration"></a>Installatie van SAP HANA in scale-out-configuratie
Als uw Azure-VM-infrastructuur is geïmplementeerd en alle andere voorbereidingen klaar bent, moet u de scale-out-configuraties van SAP HANA installeren in de volgende stappen uit:

- Installeren van het hoofdknooppunt van SAP HANA op basis van de SAP-documentatie
- **Na de installatie, moet u het bestand global.ini wijzigen en toevoegen van de parameter ' basepath_shared = Nee ' aan de global.ini**. Deze parameter kunt SAP HANA om uit te voeren in scale-out zonder 'gedeelde' **/hana/gegevens** en **/hana/log** volumes tussen de knooppunten. Details worden gedocumenteerd in [SAP Opmerking #2080991](https://launchpad.support.sap.com/#/notes/2080991).
- Na het wijzigen van de parameter global.ini, de SAP HANA-exemplaar opnieuw opstarten
- Toevoegen van extra werkknooppunten. Zie ook <https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.00/en-US/0d9fe701e2214e98ad4f8721f6558c34.html>. Geef het interne netwerk voor SAP HANA de communicatie tussen knooppunten tijdens de installatie van of later gebruikt, bijvoorbeeld, de lokale hdblcm. Zie voor meer documentatie gedetailleerde, ook [SAP Opmerking #2183363](https://launchpad.support.sap.com/#/notes/2183363). 

Na deze routine setup gaat de configuratie van de scale-out die u hebt geïnstalleerd voor het gebruik van niet-gedeelde schijven voor het werken met **/hana/gegevens** en **/hana/log**. Terwijl de **/hana/gedeelde** volume worden geplaatst op de maximaal beschikbare NFS-share.


## <a name="sap-hana-dynamic-tiering-20-for-azure-virtual-machines"></a>SAP HANA dynamische Opslaglagen 2.0 voor virtuele machines van Azure

Naast de certificeringen die SAP HANA op Azure uit de M-serie VM's, ook SAP HANA dynamische Opslaglagen 2.0 wordt ondersteund op Microsoft Azure (Zie documentatie linsk SAP HANA dynamische Opslaglagen verder omlaag). Hoewel er geen verschil in het product installeert of het besturingssysteem, bijvoorbeeld zijn via SAP HANA Cockpit binnen een Azure-Machine, er enkele belangrijke items, die verplicht voor officiële ondersteuning op Azure zijn. Deze belangrijke punten worden hieronder beschreven. In het artikel wordt de afkorting van "IT 2.0" worden gebruikt in plaats van de volledige naam dynamische 2.0 met meerdere lagen.

SAP HANA dynamische Opslaglagen 2.0 wordt niet ondersteund door SAP BW of S4HANA. Belangrijkste use-cases zijn nu systeemeigen HANA-toepassingen.


### <a name="overview"></a>Overzicht

De volgende afbeelding geeft een overzicht met betrekking tot DT 2.0-ondersteuning op Microsoft Azure. Er is een set verplichte vereisten dat moet worden gevolgd om te voldoen aan de officiële certificeringen:

- DT 2.0 moet worden geïnstalleerd op een specifieke Azure-VM. Het kan niet worden uitgevoerd op dezelfde virtuele machine waarop SAP HANA wordt uitgevoerd
- SAP HANA en DT 2.0-VM's moeten worden geïmplementeerd in hetzelfde Azure-Vnet
- De SAP HANA en DT 2.0-VM's moeten worden geïmplementeerd met Azure versneld netwerkondersteuning ingeschakeld
- Opslagtype voor de DT 2.0 virtuele machines moet Azure Premium Storage
- Meerdere Azure-schijven moeten worden gekoppeld aan de DT 2.0-VM
- Dit is vereist voor het maken van een software-raid / striped volumes (ofwel via een lvm of mdadm) maakt gebruik van gesegmenteerd te verdelen over de Azure-schijven

Meer informatie worden in de volgende secties beschreven.

![Overzicht van de SAP HANA DT 2.0-architectuur](media/hana-vm-operations/hana-dt-20.PNG)



### <a name="dedicated-azure-vm-for-sap-hana-dt-20"></a>Toegewezen virtuele machines van Azure voor SAP HANA DT 2.0

Op Azure IaaS, wordt DT 2.0 alleen ondersteund op een specifieke virtuele machine. Het is niet toegestaan om uit te voeren DT 2.0 op de dezelfde Azure-VM waarop de HANA-instantie wordt uitgevoerd. In eerste instantie twee VM-typen kunnen worden gebruikt voor het uitvoeren van SAP HANA DT 2.0:

M64-32ms, E32sv3 

Zie de beschrijving van de VM-type [hier](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-memory)

Gezien het uitgangspunt van DT 2.0, die over offloading van 'warme' gegevens om op te slaan kosten is het verstandig om het gebruik van de bijbehorende VM-grootten. Er is geen strikte regel al met betrekking tot de mogelijke combinaties. Dat hangt ervan af op de workload van een specifieke klant.

Aanbevolen configuraties zijn:

| SAP HANA VM-type | DT 2.0 VM-type |
| --- | --- | 
| M128ms | M64-32ms |
| M128s | M64-32ms |
| M64ms | E32sv3 |
| M64s | E32sv3 |


Alle combinaties van SAP HANA-gecertificeerde M-serie VM's met ondersteunde DT 2.0-VM's (M64 32ms, E32sv3) zijn mogelijk.


### <a name="azure-networking-and-sap-hana-dt-20"></a>Azure-netwerken en SAP HANA DT 2.0

DT 2.0 installeren op een specifieke virtuele machine is netwerkdoorvoer tussen de DT 2.0 virtuele machine en de SAP HANA-VM van 10 Gb minimaal vereist. Daarom is het verplicht te plaatsen van alle virtuele machines binnen hetzelfde Azure-Vnet en het inschakelen van Azure-versnelde netwerken.

Meer informatie over Azure-versnelde netwerken [hier](https://docs.microsoft.com/azure/virtual-network/create-vm-accelerated-networking-cli)

### <a name="vm-storage-for-sap-hana-dt-20"></a>VM-opslag voor SAP HANA DT 2.0

Op basis van DT 2.0 richtlijnen voor best practices moet de schijf-i/o-doorvoer ten minste 50 MB per seconde per fysieke kern. De specificaties voor de twee typen Azure VM's, die worden ondersteund voor DT 2.0 een bekijkt, ziet de maximale schijf-i/o-doorvoerlimiet voor de virtuele machine:

- E32sv3: 768 MB per seconde (zonder caching) wat betekent een ratio van 48 MB per seconde per fysieke kern dat
- M64-32ms: 1000 MB/sec (uncached) wat betekent dat een ratio van 62,5 MB per seconde per fysieke kern

Dit is vereist voor meerdere Azure-schijven koppelen aan de DT 2.0 virtuele machine en het maken van een software-raid (gesegmenteerd te verdelen) op besturingssysteemniveau te bereiken van het maximum aantal schijfdoorvoer per virtuele machine. Een enkel Azure-schijf opgeven niet de doorvoer voor het bereiken van de maximale limiet voor de virtuele machine in dit opzicht. Azure Premium storage is verplicht om uit te voeren DT 2.0. 

- Meer informatie over beschikbare Azure-schijftypes vindt [hier](https://docs.microsoft.com/azure/virtual-machines/windows/premium-storage)
- Meer informatie over het maken van software-raid via mdadm vindt [hier](https://docs.microsoft.com/azure/virtual-machines/linux/configure-raid)
- Meer informatie over het configureren van LVM voor het maken van een striped volume voor maximale doorvoer vindt [hier](https://docs.microsoft.com/azure/virtual-machines/linux/configure-lvm)

Afhankelijk van de vereisten voor de grootte zijn er verschillende opties voor het bereiken van de maximale doorvoer van een virtuele machine. Hier vindt u mogelijk gegevens volume schijfconfiguraties voor elk type DT 2.0 VM om de bovengrens voor doorvoer van virtuele machine. De E32sv3 virtuele machine moet worden beschouwd als een post-niveau voor kleinere workloads. In het geval uit te schakelen is niet snel genoeg kan het nodig zijn om het formaat van de virtuele machine M64 32ms aan.
Als de VM M64 32ms veel geheugen heeft, kan de i/o-belasting de limiet voor lees-intensieve workloads met name niet bereiken. Daarom minder schijven in de streep kan set voldoende zijn afhankelijk van de workload van een specifieke klant. Maar als u wilt worden voor de zekerheid van de schijf volgende configuraties zijn gekozen omdat de maximale doorvoer garanderen:


| VM-SKU | Configuratie van de schijf 1 | Configuratie van de schijf 2 | Schijf-configuratie 3 | Schijf Config 4 | Schijf Config 5 | 
| ---- | ---- | ---- | ---- | ---- | ---- | 
| M64-32ms | 16 TB-4 x P50 > | 4 x P40 -> 8 TB | 5 x P30 -> 5 TB | 7 x P20 -> 3,5 TB | 8 x P15 -> 2 TB | 
| E32sv3 | 3 x P50 -> 12 TB | 3 x P40 -> 6 TB opslagruimte | 4 x P30 -> 4 TB | 5 x P20 -> 2,5 TB | 6 x P15 -> 1,5 TB | 


Met name als de werkbelasting Lees-intensieve kan het i/o-prestaties om in te schakelen op Azure-host cache 'alleen-lezen-zoals aanbevolen voor de gegevensvolumes van database-software te verbeteren. Dat voor de transactie log schijfcache voor Azure-host moet 'none'. 

Met betrekking tot de grootte van het logboekvolume is een aanbevolen beginpunt een heuristiek van 15% van de grootte van de gegevens. Het maken van het logboekvolume kan worden bereikt met behulp van verschillende Azure-schijftypes, afhankelijk van de vereisten voor kosten en doorvoer. Ook voor het logboek met hoge doorvoer van volume heeft de voorkeur en in het geval van M64 32ms het wordt sterk aanbevolen om in te schakelen Write Accelerator op (dit verplicht voor SAP HANA is). Dit biedt schrijflatentie van optimale schijf voor het transactielogboek (alleen beschikbaar voor M-serie). Er zijn enkele items rekening houden met al, zoals het maximum aantal schijven per VM-type. Meer informatie over WA vindt [hier](https://docs.microsoft.com/azure/virtual-machines/windows/how-to-enable-write-accelerator)


Hier volgen enkele voorbeelden over het formaat van het logboekvolume:

| het volume grootte en de schijf-gegevenstype | Typ config 1 logboekvolume en schijf | Typ configuratie 2 logboekvolume en schijf |
| --- | --- | --- |
| 16 TB-4 x P50 > | 5 x P20 -> 2,5 TB | 3 x P30 -> 3 TB |
| 6 x P15 -> 1,5 TB | 4 x P6 -> 256 GB | 1 x-P15 -> 256 GB |


Net als voor SAP HANA-scale-out heeft de map /hana/shared om te worden gedeeld tussen de SAP HANA en de DT 2.0 VM's. De architectuur van dezelfde als voor SAP HANA met behulp van scale-out toegewezen virtuele machines die fungeren als een maximaal beschikbare NFS-server wordt aanbevolen. Om te voorzien van een gedeeld volume back-up, kan het ontwerp van de identieke worden gebruikt. Maar het is aan de klant als HA nodig zou zijn of alleen een specifieke virtuele machine met voldoende opslagcapaciteit gebruiken om te fungeren als een back-upserver voldoende is.



### <a name="links-to-dt-20-documentation"></a>Koppelingen naar DT 2.0-documentatie 

- [Dynamische Opslaglagen voor SAP HANA-installatie- en update voor](https://help.sap.com/viewer/88f82e0d010e4da1bc8963f18346f46e/2.0.03/en-US)
- [SAP HANA dynamische lagen zelfstudies en bronnen](https://www.sap.com/developer/topics/hana-dynamic-tiering.html)
- [SAP HANA dynamische Cloudlagen PoC](https://blogs.sap.com/2017/12/08/sap-hana-dynamic-tiering-delivering-on-low-tco-with-impressive-performance/)
- [SAP HANA 2.0 SP's 02 dynamische cloudlagen verbeteringen](https://blogs.sap.com/2017/07/31/sap-hana-2.0-sps-02-dynamic-tiering-enhancements/)




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
