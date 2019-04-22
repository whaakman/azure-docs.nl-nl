---
title: Configuraties van SAP HANA-infrastructuur en bewerkingen op Azure | Microsoft Docs
description: Bedieningshandleiding voor SAP HANA-systemen die zijn geïmplementeerd op Azure virtual machines.
services: virtual-machines-linux,virtual-machines-windows
documentationcenter: ''
author: msjuergent
manager: patfilot
editor: ''
tags: azure-resource-manager
keywords: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 12/04/2018
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: acccc553c5b63b2acd0f9793b0397b25145449dd
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/17/2019
ms.locfileid: "59699327"
---
# <a name="sap-hana-infrastructure-configurations-and-operations-on-azure"></a>Configuraties en bewerkingen van SAP HANA-infrastructuur in Azure
Dit artikel bevat richtlijnen over het configureren van Azure-infrastructuur en SAP HANA-systemen die zijn geïmplementeerd op virtuele machines van Azure (VM's) werken. Dit artikel bevat ook informatie over de configuratie voor SAP HANA scale-out voor de M128s VM-SKU. In dit artikel is niet bedoeld als vervanging van de standaard SAP-documentatie, waaronder de volgende inhoud:

- [Gebruikershandleiding voor SAP](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.02/330e5550b09d4f0f8b6cceb14a64cd22.html)
- [SAP-installatiehandleidingen](https://service.sap.com/instguides)
- [SAP-opmerkingen](https://sservice.sap.com/notes)

## <a name="prerequisites"></a>Vereisten
Voor het gebruik van deze handleiding, moet u basiskennis hebt van de volgende Azure-onderdelen:

- [Azure Virtual Machines](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-manage-vm)
- [Azure-netwerken en virtuele netwerken](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-virtual-network)
- [Azure Storage](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-manage-disks)

Zie voor meer informatie over SAP NetWeaver en andere onderdelen van SAP op Azure, de [SAP op Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/get-started) sectie van de [documentatie voor Azure](https://docs.microsoft.com/azure/).

## <a name="basic-setup-considerations"></a>Overwegingen bij de eenvoudige installatie
De volgende secties beschrijven de basisconfiguratie van overwegingen voor het implementeren van systemen voor SAP HANA op Azure Virtual machines.

### <a name="connect-to-azure-virtual-machines"></a>Verbinding maken met virtuele machines van Azure
Zoals beschreven in de [Planningshandleiding virtuele Azure-machines](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/planning-guide), twee basismethoden worden gebruikt om te verbinden met Azure-VM's:

- Verbinding maken via het internet en openbare eindpunten op een JumpBox-VM of op de virtuele machine die SAP HANA wordt uitgevoerd.
- Verbinding maken via een [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal) of Azure [ExpressRoute](https://azure.microsoft.com/services/expressroute/).

Site-naar-site-connectiviteit via VPN of ExpressRoute is nodig voor productiescenario's. Dit type verbinding is ook nodig voor niet-productieve-scenario's die in productiescenario's waarop SAP-software wordt gebruikt. De volgende afbeelding toont een voorbeeld van cross-site-verbinding:

![Cross-site-connectiviteit](media/virtual-machines-shared-sap-planning-guide/300-vpn-s2s.png)


### <a name="choose-azure-vm-types"></a>Kies typen Azure VM 's
De typen Azure VM's te gebruiken voor productiescenario's worden vermeld in de [SAP-documentatie voor IAAS](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html). Voor niet-productieve scenario's is een groter aantal systeemeigen Azure-VM-typen beschikbaar.

>[!NOTE]
> Voor niet-productieve scenario's, gebruikt u de VM-typen die worden vermeld in de [SAP Opmerking #1928533](https://launchpad.support.sap.com/#/notes/1928533). Voor het gebruik van Azure-VM's voor productiescenario's controleren voor SAP HANA-virtuele machines in de SAP gepubliceerd gecertificeerde [IaaS platforms lijst met gecertificeerde](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html#categories=Microsoft%20Azure).

Als u de virtuele machines in Azure implementeert, wilt gebruiken:

- De Azure-portal.
- Azure PowerShell-cmdlets.
- De Azure CLI.

U kunt een volledige SAP HANA-platform is geïnstalleerd op de virtuele machine van Azure-services via ook implementeren de [SAP cloudplatform](https://cal.sap.com/). Zie voor informatie over het installatieproces, [implementeren SAP S/4HANA of BW/4HANA on Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/cal-s4h). Zie voor informatie over de automatisering die zijn uitgebracht, [in dit artikel GitHub](https://github.com/AzureCAT-GSI/SAP-HANA-ARM).

### <a name="choose-an-azure-storage-type"></a>Kies een Azure Storage-type
Azure biedt twee typen opslag die geschikt zijn voor Azure-VM's waarop SAP HANA:  

- Standaard harde schijven (HDD's)
- Premium SSD-schijven (SSD's)

Zie voor meer informatie over deze schijftypen, [Selecteer een schijftype](../../windows/disks-types.md).

Azure biedt twee methoden voor het implementeren van VHD's op Azure standard storage en premium-opslag. Als de algehele scenario toestaat, profiteren van [Azure beheerde schijf](https://azure.microsoft.com/services/managed-disks/) implementaties.

Zie voor een lijst van opslagtypen en de bijbehorende Sla in IOP's en opslag doorvoer, [Azure-documentatie voor beheerde schijven](https://azure.microsoft.com/pricing/details/managed-disks/).

### <a name="configure-the-storage-for-azure-virtual-machines"></a>De opslag voor virtuele machines van Azure configureren

U hebt waarschijnlijk nooit verzorgd over i/o-subsystemen en de bijbehorende mogelijkheden omdat de leverancier van het apparaat hebt ervoor gezorgd dat aan de minimale vereisten zijn voldaan voor SAP HANA. Wanneer u de Azure-infrastructuur zelf maakt, moet u rekening houden met enkele van deze vereisten. U moet ook de configuratievereisten voorgesteld in de volgende secties begrijpen. Voor gevallen waarin u de virtuele machines configureren die u wilt SAP HANA om uit te voeren, moet u mogelijk:

- Inschakelen voor lezen/schrijven-volume op /hana/log minimaal 250 MB per seconde voor i/o-grootten van 1 MB.
- Activiteit van ten minste 400 MB per seconde voor /hana/data voor 16 MB en 64 MB i/o-grootte voor lezen inschakelen.
- Schakel schrijfactiviteiten van minimaal 250 MB per seconde voor /hana/data met 16 MB en 64 MB i/o-grootte.

Opslag met lage latentie is essentieel voor de DBMS-systemen, hoewel DBMS-systemen, zoals SAP HANA, blijft de gegevens in het geheugen. Het kritieke pad in de opslag is het meestal om de transactie logboek schrijven van de DBMS-systemen. Maar bewerkingen zoals savepoints schrijven of gegevens in het geheugen laden na crashherstel ook kan essentieel zijn. 

Gebruik van Azure premium-schijven voor /hana/data en /hana/log volumes is verplicht. Bouw een RAID 0 met behulp van mdadm of een logische Volume Manager (LVM) over meerdere Azure premium storage-schijven voor het bereiken van de minimale doorvoer van /hana/log en /hana/data naar wens door SAP. Ook de RAID-softwarevolumes als /hana/data en /hana/log volumes gebruiken. U wordt aangeraden dat u de volgende stripe-grootten voor de RAID-0 gebruiken:

- 64 KB of 128 KB voor hana-/ gegevens
- 32 KB voor hana-/ log

> [!NOTE]
> U hoeft niet te configureren van elk redundantieniveau met behulp van RAID-softwarevolumes omdat Azure premium en standard-opslag drie kopieën van een VHD bewaren. Het gebruik van een RAID-volume is uitsluitend bedoeld voor het configureren van volumes die voldoende i/o-doorvoer.

Samenvoegen van een aantal Azure-VHD's onder een RAID is cumulatief vanaf een doorvoer IOP's en opslag. Als u een RAID 0 via 3 x P30 Azure premium storage-schijven, biedt dit u drie keer de IOPS en drie keer de opslagdoorvoer van een enkele Azure premium storage P30-schijf.

De volgende aanbevelingen voor het opslaan in cache wordt ervan uitgegaan dat de i/o-kenmerken voor SAP HANA:

- Er is nauwelijks lezen werkbelasting op basis van de gegevensbestanden van HANA. Uitzonderingen zijn grote i/o's na het opnieuw opstarten van de HANA-instantie of wanneer gegevens worden geladen in HANA. Een ander hoofdlettergebruik van grotere lezen dat i/o's op basis van bestanden met HANA databaseback-ups kunnen worden. Als gevolg hiervan, zin Lees-caching geen omdat in de meeste gevallen alle gegevensvolumes bestand volledig moeten worden gelezen.
- Schrijven op basis van de gegevensbestanden in pieken op basis van door HANA savepoints en HANA crashherstel optreedt. Savepoints schrijven is asynchroon en geen van de gebruikerstransacties die. Schrijven van gegevens tijdens het crashherstel zijn prestaties kritieke om op te halen van het systeem reageert snel opnieuw. Crashherstel moet in plaats van uitzonderlijke situaties.
- Er zijn bijna alle leesbewerkingen van de bestanden van HANA opnieuw uit. Uitzonderingen zijn grote i/o's wanneer u transactielogboekback-ups, crashherstel of de fase opnieuw opstarten van een HANA-exemplaar uitvoeren.  
- De belangrijkste belasting op basis van het logboekbestand voor opnieuw uitvoeren van SAP HANA wordt geschreven. U kunt i/o's hebben, afhankelijk van de aard van de werkbelasting, zo klein is als 4 KB of, in andere gevallen, i/o's de grootte van 1 MB of meer. Schrijven latentie op basis van het logboek voor het opnieuw uitvoeren van SAP HANA is een essentiële prestaties.
- Alle schrijfbewerkingen moeten worden opgeslagen op schijf op een betrouwbare manier worden geactiveerd.

Als gevolg van deze waargenomen i/o-patronen door SAP HANA, stel opslaan in de cache voor de verschillende volumes die gebruikmaken van Azure premiumopslag aan:

- **/ hana/gegevens**: Geen caching.
- **/ hana/log**: Geen caching, met een uitzondering voor M-serie VM's (Zie verderop in dit artikel).
- **/ hana/gedeelde**: Lees in cache opslaan.


Houd er ook rekening met de algemene VM i/o-doorvoer wanneer u het formaat of over een virtuele machine beslissen. De algemene doorvoer van VM-opslag wordt gedocumenteerd in [grootten van virtuele machines geoptimaliseerd voor geheugen](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-memory).

#### <a name="linux-io-scheduler-mode"></a>Modus voor Linux i/o-planner
Linux heeft verschillende i/o planning modi. Een algemene aanbeveling van Linux-leveranciers als SAP is het instellen van de i/o-planner-modus voor schijfvolumes klikken beschikbaar op de **cfq** modus naar de **NoOperation** modus. Zie voor meer informatie, [SAP Opmerking #1984798](https://launchpad.support.sap.com/#/notes/1984787). 


#### <a name="storage-solution-with-write-accelerator-for-azure-m-series-virtual-machines"></a>Opslagoplossing met Write Accelerator voor Azure uit de M-serie virtuele machines
 Write dat Accelerator is als een Azure-functie beschikbaar voor Azure uit de M-serie VM's exclusief. Het doel van de functionaliteit is voor het verbeteren van de i/o-latentie van schrijfbewerkingen op basis van Azure premium storage. Voor SAP HANA mag Write Accelerator is voor het volume /hana/log alleen worden gebruikt. Om deze reden moeten de configuraties die wordt weergegeven tot nu toe worden gewijzigd. De belangrijkste wijziging wordt de verdeling tussen /hana/data en /hana/log om te kunnen gebruiken Write Accelerator op basis van het volume /hana/log alleen. 

> [!IMPORTANT]
> SAP HANA-certificering voor Azure M-serie met virtuele machines is exclusief voor Write Accelerator voor het volume/hana/log. Als gevolg hiervan, productie-scenario-implementaties voor SAP HANA op Azure M-serie met virtuele machines moet worden geconfigureerd met Write Accelerator voor/hana/aanmelden volume.

> [!NOTE]
> Voor productiescenario's, controleert u of een bepaalde VM-type voor SAP HANA wordt ondersteund door SAP in de [SAP-documentatie voor IAAS](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html).

De volgende tabel bevat de aanbevolen configuraties.

| VM-SKU | RAM | Maximum aantal VM-i/o<br /> Doorvoer | /hana/data | / hana/log | / hana/gedeeld | / Root-volume | / usr/sap | Hana/back-up |
| --- | --- | --- | --- | --- | --- | --- | --- | -- |
| M32ts | 192 GiB | 500 MB per seconde | 3 x P20 | 2 x P20 | 1 x P20 | 1 x P6 | 1 x P6 |1 x P20 |
| M32ls | 256 GiB | 500 MB per seconde | 3 x P20 | 2 x P20 | 1 x P20 | 1 x P6 | 1 x P6 |1 x P20 |
| M64ls | 512 GiB | 1000 MB per seconde | 3 x P20 | 2 x P20 | 1 x P20 | 1 x P6 | 1 x P6 |1 x P30 |
| M64s | 1000 giB | 1000 MB per seconde | 4 x P20 | 2 x P20 | 1 x P30 | 1 x P6 | 1 x P6 |2 x P30 |
| M64ms | 1,750 GiB | 1000 MB per seconde | 3 x P30 | 2 x P20 | 1 x P30 | 1 x P6 | 1 x P6 | 3 x P30 |
| M128s | 2000 giB | 2000 MB per seconde |3 x P30 | 2 x P20 | 1 x P30 | 1 x P6 | 1 x P6 | 2 x P40 |
| M128ms | 3,800 GiB | 2000 MB per seconde | 5 x P30 | 2 x P20 | 1 x P30 | 1 x P6 | 1 x P6 | 2 x P50 |

Controleer of de opslagdoorvoer van de voor de verschillende voorgestelde volumes voldoet aan de werkbelasting die u wilt uitvoeren. Als de werkbelasting grotere volumes voor /hana/data en /hana/log vereist, het aantal Azure premium storage VHD's verhogen. Formaat van een volume met meer VHD's dan de vermelde verhoogt de IOPS-waarde, en i/o-doorvoer binnen de grenzen van het type virtuele machine van Azure.

Write Accelerator werkt alleen in combinatie met [Azure beheerde schijven](https://azure.microsoft.com/services/managed-disks/). Ten minste, moeten de Azure premium storage-schijven die het volume /hana/log als beheerde schijven worden geïmplementeerd.

Er gelden beperkingen voor Azure premium storage VHD's per virtuele machine die Write Accelerator kan ondersteunen. De huidige limieten zijn:

- 16-VHD's voor een M128xx VM.
- 8-VHD's voor een M64xx VM.
- 4-VHD's voor een M32xx VM.

Zie voor meer informatie over het inschakelen van Write Accelerator [Write Accelerator](https://docs.microsoft.com/azure/virtual-machines/linux/how-to-enable-write-accelerator).

Zie voor meer informatie over en beperkingen voor Write Accelerator de dezelfde documentatie.


#### <a name="cost-conscious-azure-storage-configuration"></a>Kostenbewuste Azure Storage-configuratie
De volgende tabel toont een configuratie van VM-typen die klanten vaak gebruikt met host SAP HANA op Azure Virtual machines. Er zijn enkele VM-typen die niet voldoen aan alle minimale criteria voor SAP HANA. Ook mogelijk zijn er enkele VM-typen die officieel met SAP HANA worden niet ondersteund door SAP. Tot nu toe hebt die virtuele machines goed uitgevoerd voor niet-productieve scenario's. 

> [!NOTE]
> Voor productiescenario's, controleert u of een bepaalde VM-type voor SAP HANA wordt ondersteund door SAP in de [SAP-documentatie voor IAAS](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html).


| VM-SKU | RAM | Maximum aantal VM-i/o<br /> Doorvoer | / hana/gegevens en/hana/log<br /> striped met LVM of mdadm | / hana/gedeeld | / Root-volume | / usr/sap | Hana/back-up |
| --- | --- | --- | --- | --- | --- | --- | -- |
| DS14v2 | 112 GiB | 768 MB/sec | 3 x P20 | 1 x S20 | 1 x S6 | 1 x S6 | 1 x S15 |
| E16v3 | 128 GiB | 384 MB/sec | 3 x P20 | 1 x S20 | 1 x S6 | 1 x S6 | 1 x S15 |
| E32v3 | 256 GiB | 768 MB/sec | 3 x P20 | 1 x S20 | 1 x S6 | 1 x S6 | 1 x S20 |
| E64v3 | 432 GiB | 1200 MB per seconde | 3 x P20 | 1 x S20 | 1 x S6 | 1 x S6 | 1 x S30 |
| GS5 | 448 GiB | 2000 MB per seconde | 3 x P20 | 1 x S20 | 1 x S6 | 1 x S6 | 1 x S30 |
| M32ts | 192 GiB | 500 MB per seconde | 3 x P20 | 1 x S20 | 1 x S6 | 1 x S6 | 1 x S20 |
| M32ls | 256 GiB | 500 MB per seconde | 3 x P20 | 1 x S20 | 1 x S6 | 1 x S6 | 1 x S20 |
| M64ls | 512 GiB | 1000 MB per seconde | 3 x P20 | 1 x S20 | 1 x S6 | 1 x S6 |1 x S30 |
| M64s | 1000 giB | 1000 MB per seconde | 2 x P30 | 1 x S30 | 1 x S6 | 1 x S6 |2 x S30 |
| M64ms | 1,750 GiB | 1000 MB per seconde | 3 x P30 | 1 x S30 | 1 x S6 | 1 x S6 | 3 x S30 |
| M128s | 2000 giB | 2000 MB per seconde |3 x P30 | 1 x S30 | 1 x S6 | 1 x S6 | 2 x S40 |
| M128ms | 3,800 GiB | 2000 MB per seconde | 5 x P30 | 1 x S30 | 1 x S6 | 1 x S6 | 2 x S50 |


De schijven die worden aanbevolen voor de kleinere virtuele machine van het type met 3 x P20 oversize de volumes voor de ruimte aanbevelingen die worden vermeld in de [SAP TDI-opslag-whitepaper](https://www.sap.com/documents/2015/03/74cdb554-5a7c-0010-82c7-eda71af511fa.html). De keuze die wordt weergegeven in de tabel is gemaakt voor schijfdoorvoer, met voldoende voor SAP HANA. Als u wilt wijzigen de **/hana/back-up** volume, die is uitgebreid tot de back-ups die staan voor twee keer het volume geheugen houden, kunt u aanpassingen aan te brengen. 

Controleer of de opslagdoorvoer van de voor de verschillende voorgestelde volumes voldoet aan de werkbelasting die u wilt uitvoeren. Als de werkbelasting grotere volumes voor /hana/data en /hana/log vereist, het aantal Azure premium storage VHD's verhogen. Formaat van een volume met meer VHD's dan de vermelde verhoogt de IOPS-waarde, en i/o-doorvoer binnen de grenzen van het type virtuele machine van Azure. 

> [!NOTE]
> De vorige configuraties niet profiteren van [virtuele Azure-machine enkele VM SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_6/) omdat deze een combinatie van Azure premium storage en Azure standard-opslag gebruikt. De selectie heeft ervoor gekozen om kosten te optimaliseren. Kies premiumopslag voor alle schijven in de tabel die worden weergegeven als Azure standard-opslag (Sxx) om te voldoen aan de Azure SLA voor één virtuele machine maken van de VM-configuratie.


> [!NOTE]
> De aanbevelingen voor de configuratie schijf als doel de minimale vereisten die SAP de infrastructuur-providers biedt. Deze aanbevelingen is niet voldoende mogelijkheden in sommige gevallen bieden in implementaties van werkelijke klanten en -workloadscenario's. Bijvoorbeeld, een klant die nodig is een snellere opnieuw laden van de gegevens na het opnieuw opstarten HANA of back-upconfiguraties hogere bandbreedte naar de opslag vereist. Voor andere gegevensuitwisseling /hana/log, waar 5000 IOP's zijn niet voldoende voor de specifieke werkbelasting. Gebruik deze aanbevelingen als uitgangspunt, en pas ze en op basis van de vereisten van de werkbelasting.
>  

### <a name="set-up-azure-virtual-networks"></a>Virtuele Azure-netwerken instellen
Als u site-naar-site-verbinding met Azure via VPN of Azure ExpressRoute hebt, moet u ten minste één Azure-netwerk dat verbonden via een virtuele gateway naar het VPN of ExpressRoute-circuit hebben. Bij eenvoudige implementaties, kan de virtuele gateway in een subnet van het Azure-netwerk die als host de SAP HANA-exemplaren te fungeert worden geïmplementeerd. 

Voor het installeren van SAP HANA, twee extra subnetten binnen het Azure-netwerk te maken. Eén subnet als host fungeert voor de virtuele machines om uit te voeren van de SAP HANA-instanties. Het andere subnet uitgevoerd JumpBox of beheer-VM's naar host SAP HANA Studio, andere software voor beheer of de toepassingssoftware van uw.

> [!IMPORTANT]
> Configureren van [Azure virtuele netwerkapparaten](https://azure.microsoft.com/solutions/network-appliances/) in het communicatiepad voor tussen de SAP-toepassing en de DBMS-laag van een SAP-NetWeaver - Hybris- of S/4HANA gebaseerde SAP-systeem wordt niet ondersteund. Deze beperking is omwille van de functionaliteit en prestaties. Het communicatiepad tussen het niveau van de SAP-toepassing en de DBMS-laag moet een direct. De beperking bevat geen [toepassingsbeveiligingsgroep (ASG) en -netwerkbeveiliging groep (NSG) regels](https://docs.microsoft.com/azure/virtual-network/security-overview) als deze ASG en NSG-regels toestaat een rechtstreekse communicatie-pad dat. 
>
> Er zijn andere scenario's waar virtuele netwerkapparaten worden niet ondersteund in: 
>
> - Communicatiepaden tussen Azure-VM's die staan voor Linux Pacemaker cluster knooppunten en SBD apparaten, zoals beschreven in [hoge beschikbaarheid voor SAP NetWeaver op Azure VM's in SUSE Linux Enterprise Server voor SAP-toepassingen](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse).
> - Communicatiepaden tussen Azure VM's en Windows Server Scale-Out-bestandsserver instellen zoals beschreven in maximaal [Cluster een SAP ASCS/SCS-exemplaar op een Windows-failovercluster met behulp van een bestandsshare in Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-guide-wsfc-file-share). 
>
> Virtuele netwerkapparaten in communicatiepaden kunnen eenvoudig dubbele de netwerklatentie tussen twee communicatie-partners. Ze kunnen ook doorvoer in kritieke paden tussen het niveau van de SAP-toepassing en de DBMS-laag beperken. In sommige gevallen klant virtuele netwerkapparaten kunnen leiden tot Pacemaker Linux-clusters mislukken. Dit zijn gevallen waar de communicatie tussen de knooppunten van het Linux-Pacemaker op hun apparaat SBD via een virtueel netwerkapparaat communiceren.  
> 

> [!IMPORTANT]
> Een andere die ontwerpen van *niet* de scheiding van het niveau van de SAP-toepassing en de DBMS-laag in verschillende virtuele netwerken die niet wordt ondersteund is [gekoppeld](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview) met elkaar. Het is raadzaam dat u de SAP-toepassingslaag en DBMS-laag scheiden met behulp van subnetten binnen een Azure-netwerk in plaats van met behulp van verschillende virtuele netwerken. 
>
>Als u besluit niet op de aanbeveling volgen en in plaats daarvan de twee lagen te scheiden in verschillende virtuele netwerken, de twee virtuele netwerken moeten [gekoppeld](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview). 
>
> Houd er rekening mee dat netwerkverkeer tussen twee [gekoppeld](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview) virtuele netwerken van Azure is onderworpen aan de kosten van gegevensoverdracht. Grote gegevensvolume heeft dat uit vele terabytes bestaat worden uitgewisseld tussen de SAP-toepassingslaag en DBMS-laag. Als de SAP-toepassingslaag en DBMS-laag zijn gescheiden tussen twee gekoppelde virtuele netwerken in Azure, kunt u aanzienlijke kosten oplopen. 

Wanneer u de virtuele machines om uit te voeren van SAP HANA installeert, moeten de virtuele machines:

- Twee virtuele NIC's geïnstalleerd. Één NIC is verbonden met het beheersubnet. Een andere NIC verbinding maakt vanaf de on-premises netwerk of andere netwerken de SAP HANA-instantie in de Azure-VM.
- Statische privé IP-adressen die zijn geïmplementeerd voor zowel virtuele NIC's.

> [!NOTE]
> Toewijzen van vaste IP-adressen via Azure betekent toe te wijzen aan afzonderlijke virtuele NIC's. Statische IP-adressen binnen het gastbesturingssysteem niet toewijzen aan een virtuele NIC. Sommige Azure-services zoals Azure Backup is afhankelijk van het feit dat op minimaal de primaire virtuele NIC is ingesteld op DHCP- en niet op de vaste IP-adressen. Zie voor meer informatie, [los problemen met Azure virtuele machine back-up](https://docs.microsoft.com/azure/backup/backup-azure-vms-troubleshoot#networking). Toewijzen als u wilt meerdere statische IP-adressen toewijzen aan een virtuele machine, meerdere virtuele NIC's aan een virtuele machine.
>
>

Voor implementaties die zijn onmisbaar, maakt u een virtueel datacenter netwerkarchitectuur in Azure. Deze architectuur wordt aanbevolen de scheiding tussen de gateway van de Azure-netwerk dat is verbonden met on-premises in een afzonderlijke Azure-netwerk. Dit afzonderlijke virtuele netwerk als host fungeert voor al het verkeer dat naar on-premises of met het internet. U kunt software om te controleren en logboek-verkeer dat het virtuele datacenter in Azure in dit virtuele netwerk van afzonderlijke hub voert implementeren met behulp van deze benadering. Deze manier hebt u één virtueel netwerk die als host fungeert alle software en configuraties die is gekoppeld aan inkomend en uitgaand verkeer naar uw Azure-implementatie.


Zie voor meer informatie over de virtual datacenter-aanpak en het ontwerp van de gerelateerde Azure-netwerk: 

- [Azure virtual datacenter: Een netwerkperspectief](https://docs.microsoft.com/azure/architecture/vdc/networking-virtual-datacenter).
- [Azure virtual datacenter en de enterprise-controlelaag](https://docs.microsoft.com/azure/architecture/vdc/).


>[!NOTE]
>Verkeer dat tussen een virtueel netwerk van de hub en spoke virtueel netwerk met behulp van stromen [peering op virtueel netwerk](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview) is onderworpen aan aanvullende [kosten](https://azure.microsoft.com/pricing/details/virtual-network/). Op basis van de kosten, moet u mogelijk maken compromissen tussen een strikte hub en spoke-netwerkontwerp uitgevoerd en op meerdere [Azure ExpressRoute-gateways](https://docs.microsoft.com/azure/expressroute/expressroute-about-virtual-network-gateways) dat u verbinding met knooppunten maken om te kunnen slaan peering op virtueel netwerk. 
>
> Azure ExpressRoute-gateways geïntroduceerd extra [kosten](https://azure.microsoft.com/pricing/details/vpn-gateway/) te. U kunt ook extra kosten voor software van derden die u gebruiken om te melden, controleren en bewaking van netwerkverkeer tegenkomen. Houd rekening met de kosten voor het uitwisselen van gegevens via virtuele netwerkpeering ten opzichte van de kosten die zijn gemaakt door extra ExpressRoute-gateways en softwarelicenties. Kunt u besluiten op micro-segmentatie binnen één virtueel netwerk met behulp van subnetten als isolatie-eenheden in plaats van virtuele netwerken.


Zie voor een overzicht van de verschillende methoden die u gebruiken kunt om toe te wijzen IP-adressen, [IP-adres adrestypen en toewijzingsmethoden in Azure](https://docs.microsoft.com/azure/virtual-network/virtual-network-ip-addresses-overview-arm). 

Voor VM's waarop SAP HANA, werkt u met de statische IP-adressen die zijn toegewezen. De reden is dat bepaalde configuratiekenmerken voor HANA verwijzen naar IP-adressen.

[Azure nsg's](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) worden gebruikt om te leiden verkeer dat wordt doorgestuurd naar het SAP HANA-exemplaar of de JumpBox. De nsg's en uiteindelijk [toepassingsbeveiligingsgroepen](https://docs.microsoft.com/azure/virtual-network/security-overview#application-security-groups) zijn gekoppeld aan de SAP HANA-subnet en het beheersubnet.

De volgende afbeelding toont een overzicht van een schema ruwe implementatie voor SAP HANA die volgt op een virtueel netwerk hub en spoke-architectuur:

![Ruwe implementatie-schema voor SAP HANA](media/hana-vm-operations/hana-simple-networking.PNG)

Voor het implementeren van SAP HANA in Azure zonder dat een site-naar-site-verbinding, afschermen van het SAP HANA-exemplaar van het openbare internet en het te verbergen achter een proxy voor doorsturen. In dit eenvoudige scenario, is de implementatie afhankelijk van Azure ingebouwde DNS-services voor het omzetten van hostnamen. Azure ingebouwde DNS-services zijn in een meer complexe implementatie waarbij openbare IP-adressen worden gebruikt, met name belangrijk. Gebruik Azure nsg's en [Azure virtuele netwerkapparaten](https://azure.microsoft.com/solutions/network-appliances/) voor het bewaken van de routering via internet in uw Azure-netwerk-architectuur in Azure. 

De volgende afbeelding toont een ruwe schema voor het implementeren van SAP HANA zonder een site-naar-site-verbinding in een virtueel netwerk hub en spoke-architectuur:
  
![Ruwe implementatie schema voor SAP HANA als een site-naar-site-verbinding](media/hana-vm-operations/hana-simple-networking2.PNG)
 

Zie voor een andere beschrijving van het gebruik van Azure virtuele netwerkapparaten te beheren en te controleren toegang vanaf internet, zonder de virtuele netwerkarchitectuur van hub en spoke- [maximaal beschikbare virtuele netwerkapparaten implementeren](https://docs.microsoft.com/azure/architecture/reference-architectures/dmz/nva-ha).


## <a name="configure-azure-infrastructure-for-sap-hana-scale-out"></a>Configureren van Azure-infrastructuur voor SAP HANA-uitschalen
Microsoft heeft een M-serie VM-SKU die gecertificeerd voor de configuratie van een SAP HANA-scale-out. De VM-type M128s is gecertificeerd voor een scale-out van maximaal 16 knooppunten. Zie voor wijzigingen in scale-out-certificeringen voor SAP HANA op Azure Virtual machines, de [IaaS platforms lijst met gecertificeerde](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html#categories=Microsoft%20Azure).

De minimale OS-versies gebruikt voor het implementeren van scale-out-configuraties in Azure VM's zijn:

- SUSE Linux 12 SP3.
- Red Hat Linux 7.4.

Voor de certificering van 16 knooppunten scale-out:

- Een knooppunt staat voor het hoofdknooppunt.
- Een maximum van 15 knooppunten zijn worker-knooppunten.

>[!NOTE]
>In Azure VM scale-out implementaties is het niet mogelijk met gebruik van een stand-by-knooppunt.
>

Er zijn twee redenen waarom u een stand-by-knooppunt niet configureren:

- Azure heeft op dit moment geen systeemeigen NFS-service. Als gevolg hiervan moeten het NFS-shares worden geconfigureerd met behulp van de functionaliteit van andere leveranciers.
- Geen van de NFS-configuraties van derden kunt voldoen aan de criteria van de latentie van opslag voor SAP HANA met hun oplossingen die zijn geïmplementeerd op Azure.

Als gevolg hiervan kunnen niet /hana/data en /hana/log volumes worden gedeeld. Niet delen van deze volumes van de afzonderlijke knooppunten wordt voorkomen dat het gebruik van een stand-by-SAP HANA-knooppunt in de configuratie van een scale-out.

De volgende afbeelding ziet het ontwerp van de basis voor een enkel knooppunt in een scale-out-configuratie:

![Basisprincipes van de scale-out van één knooppunt](media/hana-vm-operations/scale-out-basics.PNG)

De basisconfiguratie van een VM-knooppunt voor SAP HANA scale-out ziet eruit zoals:

- Voor /hana/shared bouwt u van een maximaal beschikbare NFS-cluster op basis van SUSE Linux 12 SP3. Dit cluster als host fungeert voor de NFS-shares /hana/shared van uw scale-out-configuratie en SAP NetWeaver of BW/4HANA Central Services. Zie voor meer informatie over het bouwen van een dergelijke configuratie [hoge beschikbaarheid voor NFS op Azure VM's in SUSE Linux Enterprise Server](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-nfs).
- Alle volumes op andere schijven worden niet gedeeld tussen de verschillende knooppunten en zijn niet gebaseerd op NFS. Installatie van configuraties en stappen voor het scale-out HANA installaties met niet-gedeelde /hana/data en /hana/log vindt u verderop in dit artikel.

>[!NOTE]
>De maximaal beschikbare NFS-cluster wordt zoals weergegeven in de afbeeldingen tot nu toe ondersteund met SUSE Linux alleen. Een maximaal beschikbare NFS-oplossing op basis van Red Hat wordt later opnieuw te worden gesteld.

Grootte van de volumes voor de knooppunten is dezelfde als die voor scale-up, met uitzondering van /hana/shared. De volgende tabel ziet de voorgestelde grootten en -typen voor de SKU van de virtuele machine M128s.

| VM-SKU | RAM | Maximum aantal VM-i/o<br /> Doorvoer | /hana/data | / hana/log | / Root-volume | / usr/sap | Hana/back-up |
| --- | --- | --- | --- | --- | --- | --- | --- |
| M128s | 2000 giB | 2000 MB/s |3 x P30 | 2 x P20 | 1 x P6 | 1 x P6 | 2 x P40 |


Controleer of de opslagdoorvoer van de voor de verschillende voorgestelde volumes voldoet aan de werkbelasting die u wilt uitvoeren. Als de werkbelasting grotere volumes voor /hana/data en /hana/log vereist, het aantal Azure premium storage VHD's verhogen. Formaat van een volume met meer VHD's dan de vermelde verhoogt de IOPS-waarde, en i/o-doorvoer binnen de grenzen van het type virtuele machine van Azure. Write Accelerator ook van toepassing op de schijven die het volume /hana/log vormen.
 

Zie voor een formule die de grootte van het volume/hana/gedeelde voor scale-out als de geheugengrootte van een knooppunt één werknemer per vier worker-knooppunten definieert, [opslagvereisten voor SAP HANA TDI](https://www.sap.com/documents/2015/03/74cdb554-5a7c-0010-82c7-eda71af511fa.html).

Ervan uitgaande dat u de SAP HANA scale-out gecertificeerde M128s Azure virtuele machine met ongeveer 2 TB aan geheugen houden, worden de SAP-aanbevelingen als samengevat:

- Voor één hoofdknooppunt en maximaal vier worker-knooppunten is de grootte van het volume /hana/shared 2 TB.
- Voor één hoofdknooppunt en vijf tot acht worker-knooppunten is de grootte van het volume /hana/shared 4 TB.
- Voor één hoofdknooppunt en 9 12 worker-knooppunten is de grootte van het volume /hana/shared 6 TB opslagruimte.
- Voor één hoofdknooppunt en 12 tot en met 15 worker-knooppunten is de grootte van het volume /hana/shared 8 TB.

De andere belangrijke ontwerp dat wordt weergegeven in de afbeeldingen van de configuratie met één knooppunt voor een scale-out SAP HANA-virtuele machine is het virtuele netwerk met de subnetconfiguratie. SAP raadt een scheiding van het verkeer clientgerichte en toepassing van de communicatie tussen de HANA-knooppunten. 

Als u wilt dit doel te bereiken, twee verschillende virtuele NIC's koppelen aan de virtuele machine, zoals wordt weergegeven in de afbeeldingen. Beide virtuele NIC's zijn in verschillende subnetten en twee verschillende IP-adressen hebben. U vervolgens beheren de verkeersstroom met routeringsregels met behulp van nsg's of de gebruiker gedefinieerde routes.

Met name in Azure, moet u er geen middelen en methoden om af te dwingen quality of service en quota's voor specifieke virtuele NIC's zijn. De scheiding van de communicatie met clientgerichte en toepassing en intra-knooppunt niet alle mogelijkheden om één verkeer stream prioriteit boven de andere als gevolg hiervan wordt geopend. In plaats daarvan blijft de scheiding tussen een meting van de beveiliging in de intra-node-communicatie van de scale-out-configuraties afscherming.  

>[!IMPORTANT]
>SAP raadt het scheiden van netwerkverkeer naar de client en de toepassing aan clientzijde en intraknooppuntcommunicatie verkeer zoals beschreven in dit artikel. Het is raadzaam om een architectuur op locatie te plaatsen zoals wordt weergegeven in de vorige afbeeldingen.
>

De volgende afbeelding ziet u de minimale vereiste netwerk-architectuur van een netwerk standpunt:

![Basisprincipes van de scale-out van één knooppunt](media/hana-vm-operations/scale-out-networking-overview.PNG)

De limieten ondersteund tot nu toe zijn 15 worker-knooppunten naast een hoofdknooppunt.

De volgende afbeelding toont de opslagarchitectuur vanuit het oogpunt van een opslagruimte:


![Basisprincipes van de scale-out van één knooppunt](media/hana-vm-operations/scale-out-storage-overview.PNG)

Het volume /hana/shared bevindt zich op de maximaal beschikbare configuratie van de NFS-share. De stations zijn lokaal gekoppeld aan de afzonderlijke virtuele machines. 

### <a name="highly-available-nfs-share"></a>Maximaal beschikbare NFS-share
Tot nu toe, de maximaal beschikbare NFS-cluster werkt samen met SUSE Linux alleen. Voor installatie-informatie, Zie [hoge beschikbaarheid voor NFS op Azure VM's in SUSE Linux Enterprise Server](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-nfs). Als u het NFS-cluster niet met andere HANA configuraties buiten het Azure-netwerk met de SAP HANA-instanties delen, kunt u dit in hetzelfde virtuele netwerk installeren. Installeer het in een eigen subnet en zorg ervoor dat het subnet toegang hebben tot willekeurige niet al het verkeer. In plaats daarvan beperken het verkeer op dat subnet naar het IP-adressen van de virtuele machine die het verkeer naar /hana/shared volume worden uitgevoerd.

Met betrekking tot de virtuele NIC van een HANA scale-out-virtuele machine die het verkeer /hana/shared, zijn de aanbevelingen:

- Omdat het verkeer naar /hana/shared wordt gematigd, route u deze via de virtuele NIC die toegewezen aan de clientnetwerk in de minimale configuratie.
- Uiteindelijk, voor het verkeer naar/hana/gedeelde, implementeert u een derde subnet in het virtuele netwerk waarin u de configuratie van de SAP HANA-scale-out implementeren. Toewijzen van een derde virtuele NIC die wordt gehost in dat subnet. Gebruik de derde virtuele NIC en de bijbehorende IP-adres voor het verkeer naar de NFS-share. Vervolgens kunt u afzonderlijke toegang en routeringsregels toepassen.

>[!IMPORTANT]
>Netwerkverkeer tussen de VM's waarop SAP HANA op een uitbreidbare manier geïmplementeerd en de maximaal beschikbare NFS onder geen enkele omstandigheid kan worden gerouteerd via een [virtueel netwerkapparaat](https://azure.microsoft.com/solutions/network-appliances/) of vergelijkbare virtuele apparaten. Azure nsg's zijn geen dergelijke apparaten. Controleer uw routeringsregels om ervoor te zorgen dat virtuele netwerkapparaten of vergelijkbare virtuele apparaten zijn detoured wanneer ze toegang krijgen de maximaal beschikbare NFS tot delen vanuit de VM's waarop SAP HANA.
> 

Als u delen van de maximaal beschikbare NFS-cluster tussen SAP HANA-configuraties wilt, verplaatst u alle die HANA-configuraties in hetzelfde virtuele netwerk. 
 

### <a name="install-an-sap-hana-scale-out-in-azure"></a>Installeren van een SAP HANA-scale-out in Azure
Volg deze algemene stappen voor het installeren van een scale-out SAP-configuratie:

- Nieuwe implementeren of nieuwe met Azure virtual network-infrastructuur aan te passen.
- De nieuwe virtuele machines implementeren met behulp van Azure-beheerde premium-opslagvolumes.
- Implementeer een nieuwe of aanpassen van een bestaande maximaal beschikbare NFS-cluster.
- Aanpassing van de routering van om ervoor te zorgen dat bijvoorbeeld intraknooppuntcommunicatie communicatie tussen VM's wordt niet doorgestuurd via een [virtueel netwerkapparaat](https://azure.microsoft.com/solutions/network-appliances/). Hetzelfde geldt voor verkeer tussen de virtuele machines en het maximaal beschikbare NFS-cluster.
- Installeer het hoofdknooppunt van SAP HANA.
- Aanpassing van parameters voor de configuratie van het hoofdknooppunt van SAP HANA.
- Ga door met de installatie van de SAP HANA worker-knooppunten.

#### <a name="install-sap-hana-in-a-scale-out-configuration"></a>SAP HANA installeren in een scale-out-configuratie
Uw Azure-VM-infrastructuur is geïmplementeerd en alle andere voorbereidingen zijn voltooid. Nu als volgt te werk voor het installeren van de scale-out-configuraties van SAP HANA:

- Installeer het hoofdknooppunt van SAP HANA op basis van de SAP-documentatie.
- *Na de installatie, het bestand global.ini wijzigen en toevoegen van de parameter ' basepath_shared = Nee ' aan de global.ini*. Deze parameter kunt SAP HANA om uit te voeren in scale-out zonder gedeelde /hana/data en /hana/log volumes tussen de knooppunten. Zie voor meer informatie, [SAP Opmerking #2080991](https://launchpad.support.sap.com/#/notes/2080991).
- Nadat u de parameter global.ini wijzigt, moet u de SAP HANA-exemplaar opnieuw opstarten.
- Toevoegen van extra werkknooppunten. Zie voor meer informatie, [beheerdershandleiding voor SAP HANA](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.00/en-US/0d9fe701e2214e98ad4f8721f6558c34.html). Geef het interne netwerk voor de communicatie tussen knooppunten SAP HANA tijdens de installatie of daarna met behulp van, bijvoorbeeld: de lokale hdblcm. Zie voor meer informatie, [SAP Opmerking #2183363](https://launchpad.support.sap.com/#/notes/2183363). 

Na deze routine setup gebruikt de configuratie van de scale-out die u hebt geïnstalleerd niet-gedeelde schijven /hana/data en /hana/log uit te voeren. Het volume/hana/gedeeld wordt geplaatst op de maximaal beschikbare NFS-share.


## <a name="sap-hana-dynamic-tiering-20-for-azure-virtual-machines"></a>SAP HANA, dynamische cloudlagen 2.0 voor virtuele machines van Azure

Naast de certificeringen die SAP HANA op Azure uit de M-serie VM's, wordt SAP HANA dynamische cloudlagen 2.0 (DT 2.0) ook ondersteund op Microsoft Azure. Voor koppelingen naar SAP HANA-documentatie voor dynamische cloudlagen, Zie de sectie 'Is gekoppeld aan DT 2.0 documentatie' verderop in dit artikel. Er is geen verschil in het product installeert of operationele, bijvoorbeeld via SAP HANA Cockpit binnen een Azure-VM, maar een paar belangrijke items zijn verplicht voor officiële ondersteuning op Azure. Deze belangrijke punten worden in de volgende secties beschreven. 

SAP HANA DT 2.0 wordt niet ondersteund door SAP BW of S4HANA. De belangrijkste use cases zijn nu systeemeigen HANA-toepassingen.


### <a name="overview"></a>Overzicht

De volgende afbeelding geeft een overzicht van DT 2.0-ondersteuning op Microsoft Azure. Volg deze verplichte vereisten om te voldoen aan de officiële certificeringen:

- DT 2.0 moet worden geïnstalleerd op een specifieke Azure-VM. Deze mogelijk niet worden uitgevoerd op dezelfde virtuele machine waarop SAP HANA wordt uitgevoerd.
- SAP HANA en DT 2.0-VM's moeten worden geïmplementeerd in dezelfde Azure virtual network.
- De SAP HANA en DT 2.0-VM's moeten worden geïmplementeerd met Azure Accelerated Networking ingeschakeld.
- Het opslagtype voor de DT 2.0 virtuele machines moet Azure premium-opslag.
- Meerdere Azure-schijven moeten worden gekoppeld aan de DT 2.0 virtuele machine.
- Het maken van een software RAID- of striped volumes, via LVM of mdadm, met behulp van gesegmenteerd te verdelen over de Azure-schijven is vereist.

De volgende secties vindt u meer uitleg over.

![Overzicht van de architectuur van SAP HANA DT 2.0](media/hana-vm-operations/hana-dt-20.PNG)



### <a name="dedicated-azure-vm-for-sap-hana-dt-20"></a>Toegewezen virtuele machines van Azure voor SAP HANA DT 2.0

Op Azure IaaS, wordt DT 2.0 alleen ondersteund op een specifieke virtuele machine. DT 2.0 is niet toegestaan om uit te voeren op de dezelfde Azure-VM waarop de HANA-instantie wordt uitgevoerd. In eerste instantie kunnen twee VM-typen worden gebruikt voor het uitvoeren van SAP HANA DT 2.0:

- M64-32ms 
- E32sv3 

Zie voor beschrijvingen van de VM-typen, [grootten van virtuele machines geoptimaliseerd voor geheugen](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-memory).

Het uitgangspunt van DT 2.0, die over offloading van warme gegevens om kosten te besparen, krijgt verstandig het om het gebruik van de bijbehorende VM-grootten. Er is geen strikte regel voor de mogelijke combinaties. Dat hangt ervan af op de workload van een specifieke klant.

De volgende tabel bevat de aanbevolen configuraties.

| SAP HANA VM-type | DT 2.0 VM-type |
| --- | --- | 
| M128ms | M64-32ms |
| M128s | M64-32ms |
| M64ms | E32sv3 |
| M64s | E32sv3 |


Alle combinaties van SAP HANA-gecertificeerde M-serie VM's met ondersteunde DT 2.0-VM's, zoals M64 32ms en E32sv3, zijn mogelijk.


### <a name="azure-networking-and-sap-hana-dt-20"></a>Azure-netwerken en SAP HANA DT 2.0

DT 2.0 installeren op een specifieke virtuele machine vereist netwerkdoorvoer tussen de DT 2.0 virtuele machine en de SAP HANA-VM met een minimum van 10 GB. Als gevolg hiervan is het verplicht te plaatsen van alle virtuele machines binnen hetzelfde Azure virtual network en Azure Accelerated Networking inschakelen.

Zie voor meer informatie over Azure Accelerated Networking [maken van een virtuele Linux-machine met versnelde netwerken](https://docs.microsoft.com/azure/virtual-network/create-vm-accelerated-networking-cli).

### <a name="vm-storage-for-sap-hana-dt-20"></a>VM-opslag voor SAP HANA DT 2.0

Op basis van DT 2.0 best practice-richtlijnen is de minimale schijf i/o-doorvoer 50 MB per seconde per fysieke kern. Kijken naar de specificaties voor de twee typen Azure VM's, die worden ondersteund voor DT 2.0, is de maximale schijf-i/o-doorvoerlimiet voor de virtuele machine:

- **E32sv3**:   768 MB per seconde, zonder caching, wat betekent een ratio van 48 MB per seconde per fysieke kern dat
- **M64-32ms**:  1000 MB per seconde, zonder caching, wat betekent een ratio van 62,5 MB per seconde per fysieke kern dat

Meerdere Azure-schijven koppelen aan de DT 2.0 virtuele machine en het maken van een software-RAID met behulp van striping op het niveau van het bereiken van het maximum aantal schijfdoorvoer per virtuele machine is vereist. Een enkel Azure-schijf opgeven niet de doorvoer voor het bereiken van de maximumlimiet van de virtuele machine. Azure premium storage is verplicht om uit te voeren DT 2.0. 

- Zie voor meer informatie over beschikbare Azure-schijftypes [een schijf selecteren voor Azure IaaS Windows VM's](../../windows/disks-types.md).
- Zie voor meer informatie over het maken van een software-RAID via mdadm [een software-RAID op Linux configureren](https://docs.microsoft.com/azure/virtual-machines/linux/configure-raid).
- Zie voor meer informatie over het configureren van LVM voor het maken van een striped volume voor een maximale doorvoer [LVM configureren op een Linux-VM in Azure](https://docs.microsoft.com/azure/virtual-machines/linux/configure-lvm).

Afhankelijk van de vereisten voor de grootte zijn er verschillende opties voor het bereiken van de maximale doorvoer van een virtuele machine. Hier vindt u mogelijk gegevens volume schijfconfiguraties voor elk type DT 2.0 VM om de bovengrens voor doorvoer van virtuele machine. De VM E32sv3 wordt beschouwd als een post-niveau voor kleinere workloads. Als dit niet snel genoeg is, kan het nodig zijn om het formaat van de virtuele machine M64 32ms te zijn.

Omdat de M64 32ms virtuele machine een grote hoeveelheid geheugen heeft, kan de limiet, met name voor lees-intensieve werkbelastingen niet bereiken door de i/o-belasting. Minder schijven in de stripeset met mogelijk voldoende, afhankelijk van de klant-specifieke werkbelasting. Als u aan de veilige, zijn gekozen omdat de volgende schijfconfiguraties maximale doorvoer garanderen.


| VM-SKU | Configuratie van de schijf 1 | Configuratie van de schijf 2 | Schijf-configuratie 3 | Schijf config 4 | Schijf config 5 | 
| ---- | ---- | ---- | ---- | ---- | ---- | 
| M64-32ms | 4 x P50 -> 16 TB | 4 x P40 -> 8 TB | 5 x P30 -> 5 TB | 7 x P20 -> 3.5 TB | 8 x P15 -> 2 TB | 
| E32sv3 | 3 x P50 -> 12 TB | 3 x P40 -> 6 TB | 4 x P30 -> 4 TB | 5 x P20 -> 2.5 TB | 6 x P15 -> 1.5 TB | 


Met name als de werkbelasting Lees-intensieve is, inschakelen van de alleen-lezen-'-instelling voor de cache voor Azure-host, zoals aanbevolen voor de gegevensvolumes van database-software i/o-prestaties mogelijk verbeteren. Voor het transactielogboek moet de Azure-host-schijfcache 'none'. 

Het beginpunt dat we voor de grootte van het logboekvolume aanraden is een heuristiek van 15 procent van de grootte van de gegevens. Gebruik verschillende Azure-schijftypes, afhankelijk van de vereisten voor kosten en doorvoer voor het maken van het logboekvolume. Hoge i/o-doorvoer is vereist voor het logboekvolume. 

Als u de virtuele machine M64 32ms typt, is het raadzaam dat u inschakelt [Write Accelerator](https://docs.microsoft.com/azure/virtual-machines/linux/how-to-enable-write-accelerator). Write Accelerator is als een functie van Azure met optimale schijf schrijven latentie voor het transactielogboek. Het is alleen beschikbaar voor de M-serie. Er zijn enkele items om te overwegen, zoals het maximum aantal schijven per VM-type. Zie voor meer informatie over Write Accelerator [Write Accelerator inschakelt](https://docs.microsoft.com/azure/virtual-machines/windows/how-to-enable-write-accelerator).


De volgende tabel ziet u enkele voorbeelden kunt u het formaat van het logboekvolume.

| het volume grootte en de schijf-gegevenstype | Typ config 1 logboekvolume en schijf | Typ configuratie 2 logboekvolume en schijf |
| --- | --- | --- |
| 4 x P50 -> 16 TB | 5 x P20 -> 2.5 TB | 3 x P30 -> 3 TB |
| 6 x P15 -> 1.5 TB | 4 x P6 -> 256 GB | 1 x P15 -> 256 GB |


Net als bij de SAP HANA scale-out, de map /hana/shared moet tussen de SAP HANA en de DT 2.0 VM's worden gedeeld. U wordt aangeraden dat u dezelfde architectuur voor SAP HANA scale-out gebruiken met behulp van de toegewezen virtuele machines die als een maximaal beschikbare NFS-server fungeren. Gebruik het identieke ontwerp voor een gedeeld volume back-up. Maar het is aan u om te bepalen als hoge beschikbaarheid nodig is of als deze voldoende is voor gebruik van een specifieke virtuele machine met voldoende opslagcapaciteit om te fungeren als een back-upserver.



### <a name="links-to-dt-20-documentation"></a>Koppelingen naar DT 2.0-documentatie 

- [Dynamische cloudlagen installatie van de SAP HANA- en updatehandleiding](https://help.sap.com/viewer/88f82e0d010e4da1bc8963f18346f46e/2.0.03/en-US)
- [SAP HANA dynamische cloudlagen zelfstudies en bronnen](https://help.sap.com/viewer/fb9c3779f9d1412b8de6dd0788fa167b/2.0.03/en-US)
- [SAP HANA dynamische cloudlagen testen van concept](https://blogs.sap.com/2017/12/08/sap-hana-dynamic-tiering-delivering-on-low-tco-with-impressive-performance/)
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
Als u een site-naar-site-verbinding tussen uw on-premises locaties en Azure hebt en u SAP-onderdelen uitvoert, bent u waarschijnlijk al SAProuter uitgevoerd. In dit geval, als volgt te werk voor ondersteuning voor externe:

- Het privé- en statische IP-adres van de virtuele machine die als host fungeert SAP HANA in de configuratie van SAProuter onderhouden.
- Configureer de NSG van het subnet dat als host fungeert voor de HANA VM voor het toestaan van verkeer via TCP/IP-poort 3299.

Als u verbinding met Azure via het internet maken en u geen een SAP-router voor de virtuele machine met SAP HANA hebt, installeer het onderdeel. Installeer SAProuter in een afzonderlijke virtuele machine in het beheersubnet. 

De volgende afbeelding toont een ruwe schema voor het implementeren van SAP HANA met SAProuter en zonder een site-naar-site-verbinding:

![Implementatie-schema voor SAP HANA ruwe zonder dat u een site-naar-site-verbinding en SAProuter](media/hana-vm-operations/hana-simple-networking3.PNG)

Zorg ervoor dat SAProuter installeren in een afzonderlijke virtuele machine en niet in uw JumpBox-VM. De afzonderlijke virtuele machine moet een statisch IP-adres hebben. Neem contact op met SAP voor een IP-adres voor uw SAProuter verbinding met de SAProuter die wordt gehost door SAP. De SAProuter die wordt gehost door SAP is het equivalent van de SAProuter-instantie die u op de virtuele machine installeert. Het IP-adres van het SAP gebruiken om te configureren van uw SAProuter-exemplaar. In de configuratie-instellingen is het alleen de benodigde poort TCP-poort 3299.

Zie voor meer informatie over het instellen en onderhouden van ondersteuning voor externe verbindingen via SAProuter [SAP-documentatie](https://support.sap.com/en/tools/connectivity-tools/remote-support.html).

### <a name="high-availability-with-sap-hana-on-azure-native-vms"></a>Hoge beschikbaarheid met SAP HANA op Azure systeemeigen VM 's
Als u SUSE Linux Enterprise Server voor SAP-toepassingen 12 SP1 of hoger uitvoert, kunt u een cluster Pacemaker kunt maken met stonith instellen-apparaten. De apparaten gebruiken voor het instellen van een SAP HANA-configuratie die gebruikmaakt van synchrone replicatie met HANA-Systeemreplicatie en automatische failover. Zie voor meer informatie over de installatieprocedure [SAP HANA hoge beschikbaarheid-handleiding voor Azure virtual machines](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-availability-overview).
