---
title: SAP HANA Azure virtuele machine opslagconfiguraties | Microsoft Docs
description: Aanbevelingen voor opslag voor virtuele machine met SAP HANA in ze zijn geïmplementeerd.
services: virtual-machines-linux,virtual-machines-windows
documentationcenter: ''
author: msjuergent
manager: bburns
editor: ''
tags: azure-resource-manager
keywords: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 06/05/2019
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: d062b6fff9693d5bda75edd65b8fe88d834eff57
ms.sourcegitcommit: 4cdd4b65ddbd3261967cdcd6bc4adf46b4b49b01
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/06/2019
ms.locfileid: "66735509"
---
# <a name="sap-hana-azure-virtual-machine-storage-configurations"></a>Configuraties van SAP HANA Azure virtuele machines opslag

Azure biedt verschillende typen opslag die geschikt zijn voor Azure-VM's waarop SAP HANA. De Azure storage-typen die kunnen worden overwogen om de lijst met de SAP HANA-implementaties, zoals: 

- Standaard SSD-schijven (SSD)
- Premium Solid-State drives (SSD)
- Ultra SSD in openbare preview en nog niet ondersteund met SAP-toepassingen voor productie

Zie het artikel voor meer informatie over deze schijftypen, [een schijf selecteren](https://docs.microsoft.com/azure/virtual-machines/linux/disks-types)

Azure biedt twee methoden voor het implementeren van VHD's op Azure Standard en Premium-opslag. Als de algehele scenario toestaat, profiteren van [Azure beheerde schijf](https://azure.microsoft.com/services/managed-disks/) implementaties. 

Voor een lijst van opslagtypen en de bijbehorende Sla in IOP's en opslag doorvoer, raadpleegt u de [Azure-documentatie voor beheerde schijven](https://azure.microsoft.com/pricing/details/managed-disks/).

**Aanbeveling: Azure Premium Storage gebruiken in combinatie met Azure Write Accelerator en gebruiken van Azure Managed Disks voor implementatie**

In de wereld van de on-premises moest u zelden over de i/o-subsystemen en de mogelijkheden ervan. Reden is dat de leverancier van het apparaat nodig om ervoor te zorgen dat de minimale vereisten zijn voldaan voor SAP HANA. Als u de Azure-infrastructuur zelf maakt, moet u rekening houden met enkele van deze vereisten. Enkele van de kenmerken van de minimale doorvoer die wordt gevraagd worden de noodzaak om te leidt:

- Inschakelen van lezen/schrijven op **/hana/log** van een 250 MB per seconde met i/o-groottes van 1 MB
- Activiteit van ten minste 400 MB per seconde voor voor lezen inschakelen **/hana/gegevens** voor 16 en 64 MB i/o-grootten
- Inschakelen van schrijfactiviteiten van minimaal 250 MB per seconde voor **/hana/gegevens** met 16 en 64 MB i/o-grootte

Latentie is opgegeven die weinig opslagruimte essentieel voor DBMS-systemen, zelfs als het DBMS, zoals SAP HANA, blijven de gegevens in het geheugen. Het kritieke pad in de opslag is het meestal om de transactie logboek schrijven van de DBMS-systemen. Maar ook bewerkingen, zoals savepoints schrijven of gegevens in het geheugen laden na crashherstel kan essentieel zijn. Het is daarom **verplichte** gebruikmaken van Azure Premium-schijven voor **/hana/gegevens** en **/hana/log** volumes. Om de minimale doorvoer van te realiseren **/hana/log** en **/hana/gegevens** naar wens door SAP, moet u een RAID 0 bouwen met behulp van MDADM of LVM via meerdere Azure Premium Storage-schijven. En gebruik van de RAID-volumes als **/hana/gegevens** en **/hana/log** volumes. 

**Aanbeveling: Als stripe-voor de RAID 0 de aanbeveling grootten is om te gebruiken:**

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

**Aanbeveling: Als gevolg van deze waargenomen i/o-patronen door SAP HANA, moet de in cache opslaan voor de verschillende volumes met behulp van Azure Premium Storage worden ingesteld, zoals:**

- **/ hana/gegevens** -geen caching
- **/ hana/log** - geen caching - uitzondering voor M-serie (Zie verderop in dit document)
- **/ hana/gedeelde** - lezen in cache opslaan


Houd er ook de algehele VM i/o-doorvoer in rekening moet houden bij het vergroten en verkleinen of kiezen voor een virtuele machine. Totale doorvoer van VM-opslag wordt beschreven in het artikel [grootten van virtuele machines geoptimaliseerd voor geheugen](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-memory).

## <a name="linux-io-scheduler-mode"></a>Linux-i/o-planner-modus
Linux heeft verschillende i/o planning modi. Algemene aanbeveling via Linux-leveranciers en SAP wordt opnieuw configureren van de i/o-planner-modus voor schijfvolumes van de **cfq** modus naar de **NoOperation** modus. Meer informatie wordt verwezen in [SAP Opmerking #1984798](https://launchpad.support.sap.com/#/notes/1984787). 


## <a name="production-storage-solution-with-azure-write-accelerator-for-azure-m-series-virtual-machines"></a>Oplossing voor productie met Azure Write Accelerator voor virtuele machines van Azure-M-serie
Azure Write Accelerator is een functie die wordt ophalen uitgerold voor M-serie virtuele Azure-machines uitsluitend. Als de naam van de staat, is het doel van de functionaliteit voor het verbeteren van i/o-latentie van schrijfbewerkingen ten opzichte van de Azure Premium Storage. Voor SAP HANA, Write Accelerator moet worden gebruikt voor de **/hana/log** alleen volume. Daarom de **/hana/gegevens** en **/hana/log** afzonderlijke volumes met de ondersteuning van Azure Write Accelerator de **/hana/log** alleen volume. 

> [!IMPORTANT]
> SAP HANA-certificering voor Azure-M-serie virtuele machines is uitsluitend in combinatie met Azure Write Accelerator voor de **/hana/log** volume. Als gevolg hiervan productie scenario-implementaties van SAP HANA op Azure M-serie virtuele machines worden verwacht kan worden geconfigureerd met Azure Write Accelerator voor de **/hana/log** volume.  

> [!NOTE]
> Voor productiescenario's, controleert u of een bepaalde VM-type voor SAP HANA wordt ondersteund door SAP in de [SAP-documentatie voor IAAS](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html).

**Aanbeveling: De aanbevolen configuraties voor productiescenario's er als volgt uitzien:**

| VM-SKU | RAM | Met maximaal VM-I/O<br /> Doorvoer | /hana/data | / hana/log | / hana/gedeeld | / Root-volume | / usr/sap | Hana/back-up |
| --- | --- | --- | --- | --- | --- | --- | --- | -- |
| M32ts | 192 GiB | 500 MB/s | 3 x P20 | 2 x P20 | 1 x P20 | 1 x P6 | 1 x P6 |1 x P20 |
| M32ls | 256 GiB | 500 MB/s | 3 x P20 | 2 x P20 | 1 x P20 | 1 x P6 | 1 x P6 |1 x P20 |
| M64ls | 512 GiB | 1000 MB/s | 3 x P20 | 2 x P20 | 1 x P20 | 1 x P6 | 1 x P6 |1 x P30 |
| M64s | 1000 GiB | 1000 MB/s | 4 x P20 | 2 x P20 | 1 x P30 | 1 x P6 | 1 x P6 |2 x P30 |
| M64ms | 1750 GiB | 1000 MB/s | 3 x P30 | 2 x P20 | 1 x P30 | 1 x P6 | 1 x P6 | 3 x P30 |
| M128s | 2000 GiB | 2000 MB/s |3 x P30 | 2 x P20 | 1 x P30 | 1 x P10 | 1 x P6 | 2 x P40 |
| M128ms | 3800 GiB | 2000 MB/s | 5 x P30 | 2 x P20 | 1 x P30 | 1 x P10 | 1 x P6 | 4 x P40 |
| M208s_v2 | 2850 GiB | 1000 MB/s | 4 x P30 | 2 x P20 | 1 x P30 | 1 x P10 | 1 x P6 | 3 x P40 |
| M208ms_v2 | 5700 GiB | 1000 MB/s | 4 x P40 | 2 x P20 | 1 x P30 | 1 x P10 | 1 x P6 | 3 x P50 |
| M416s_v2 | 5700 GiB | 2000 MB/s | 4 x P40 | 2 x P20 | 1 x P30 | 1 x P10 | 1 x P6 | 3 x P50 |
| M416ms_v2 | 11400 GiB | 2000 MB/s | 8 x P40 | 2 x P20 | 1 x P30 | 1 x P10 | 1 x P6 | 4 x P50 |

M416xx_v2 VM-typen worden niet nog beschikbaar gemaakt door Microsoft aan het publiek. Controleer of de opslagdoorvoer van de voor de verschillende voorgestelde volumes voldoet aan de werkbelasting die u wilt uitvoeren. Als de workload grotere volumes voor **/hana/gegevens** en **/hana/log**, moet u het aantal Azure Premium Storage-VHD's verhogen. Formaat van een volume met meer VHD's dan de vermelde verhoogt de IOPS-waarde, en i/o-doorvoer binnen de grenzen van het type virtuele machine van Azure.

Azure Write Accelerator werkt alleen in combinatie met [Azure beheerde schijven](https://azure.microsoft.com/services/managed-disks/). Dus in ten minste de Azure Premium Storage-schijven die de **/hana/log** volume moet worden geïmplementeerd als beheerde schijven.

Er zijn beperkingen van Azure Premium Storage VHD's per virtuele machine die kan worden ondersteund door Azure Write Accelerator. De huidige limieten zijn:

- 16-VHD's voor een virtuele machine M128xx en M416xx
- 8 VHD's voor een virtuele machine M64xx en M208xx
- 4-VHD's voor een M32xx VM

Meer gedetailleerde instructies over het inschakelen van Azure Write Accelerator vindt u in het artikel [Write Accelerator](https://docs.microsoft.com/azure/virtual-machines/linux/how-to-enable-write-accelerator).

Details en -beperkingen voor Azure Write Accelerator kunnen worden gevonden in de documentatie van dezelfde.

**Aanbeveling: U moet Write Accelerator voor schijven die het volume /hana/log gebruiken**


## <a name="cost-conscious-azure-storage-configuration"></a>Kosten prijsbewuste Azure Storage-configuratie
De volgende tabel toont een configuratie van VM-typen die klanten ook gebruiken met host SAP HANA op Azure Virtual machines. Er zijn enkele VM-typen die niet voldoet aan alle criteria voor minimale opslag voor SAP HANA of zijn niet officieel ondersteund met SAP HANA door SAP. Maar tot nu toe die virtuele machines is om uit te voeren prima voor niet-productiescenario's. De **/hana/gegevens** en **/hana/log** worden gecombineerd tot één volume. Als gevolg hiervan kan een beperking in termen van IOPS resulteren in het gebruik van Azure Write Accelerator.

> [!NOTE]
> Voor SAP ondersteund scenario's, controleert u of een bepaalde VM-type voor SAP HANA wordt ondersteund door SAP in de [SAP-documentatie voor IAAS](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html).

> [!NOTE]
> Een verandering van vroegere aanbevelingen voor een oplossing van de kosten-bewust is om te verplaatsen van [Azure standaard harde schijven](https://docs.microsoft.com/azure/virtual-machines/windows/disks-types#standard-hdd) tot het uitvoeren van betere en meer betrouwbare [Azure Standard-SSD-schijven](https://docs.microsoft.com/azure/virtual-machines/windows/disks-types#standard-ssd)


| VM-SKU | RAM | Met maximaal VM-I/O<br /> Doorvoer | / hana/gegevens en/hana/log<br /> striped met LVM of MDADM | / hana/gedeeld | / Root-volume | / usr/sap | Hana/back-up |
| --- | --- | --- | --- | --- | --- | --- | -- |
| DS14v2 | 112 GiB | 768 MB/s | 3 x P20 | 1 x E20 | 1 x E6 | 1 x E6 | 1 x E15 |
| E16v3 | 128 GiB | 384 MB/s | 3 x P20 | 1 x E20 | 1 x E6 | 1 x E6 | 1 x E15 |
| E32v3 | 256 GiB | 768 MB/s | 3 x P20 | 1 x E20 | 1 x E6 | 1 x E6 | 1 x E20 |
| E64v3 | 432 GiB | 1200 MB/s | 3 x P20 | 1 x E20 | 1 x E6 | 1 x E6 | 1 x E30 |
| GS5 | 448 GiB | 2000 MB/s | 3 x P20 | 1 x E20 | 1 x E6 | 1 x E6 | 1 x E30 |
| M32ts | 192 GiB | 500 MB/s | 3 x P20 | 1 x E20 | 1 x E6 | 1 x E6 | 1 x E20 |
| M32ls | 256 GiB | 500 MB/s | 3 x P20 | 1 x E20 | 1 x E6 | 1 x E6 | 1 x E20 |
| M64ls | 512 GiB | 1000 MB/s | 3 x P20 | 1 x E20 | 1 x E6 | 1 x E6 |1 x E30 |
| M64s | 1000 GiB | 1000 MB/s | 2 x P30 | 1 x E30 | 1 x E6 | 1 x E6 |2 x E30 |
| M64ms | 1750 GiB | 1000 MB/s | 3 x P30 | 1 x E30 | 1 x E6 | 1 x E6 | 3 x E30 |
| M128s | 2000 GiB | 2000 MB/s |3 x P30 | 1 x E30 | 1 x E10 | 1 x E6 | 2 x E40 |
| M128ms | 3800 GiB | 2000 MB/s | 5 x P30 | 1 x E30 | 1 x E10 | 1 x E6 | 2 x E50 |
| M208s_v2 | 2850 GiB | 1000 MB/s | 4 x P30 | 1 x E30 | 1 x E10 | 1 x E6 |  3 x E40 |
| M208ms_v2 | 5700 GiB | 1000 MB/s | 4 x P40 | 1 x E30 | 1 x E10 | 1 x E6 |  4 x E40 |
| M416s_v2 | 5700 GiB | 2000 MB/s | 4 x P40 | 1 x E30 | 1 x E10 | 1 x E6 |  4 x E40 |
| M416ms_v2 | 11400 GiB | 2000 MB/s | 8 x P40 | 1 x E30 | 1 x E10 | 1 x E6 |  4 x E50 |


M416xx_v2 VM-typen worden niet nog beschikbaar gemaakt door Microsoft aan het publiek. De schijven die zijn aanbevolen voor de kleinere virtuele machine van het type met 3 x P20 oversize de volumes met betrekking tot de aanbevelingen ruimte volgens de [technisch document over opslag voor SAP-TDI](https://www.sap.com/documents/2015/03/74cdb554-5a7c-0010-82c7-eda71af511fa.html). Evenwel de keuze zoals weergegeven in de tabel is gemaakt voor schijfdoorvoer, met voldoende voor SAP HANA. Als u wijzigingen in de **/hana/back-up** volume, die is aangepast voor het bewaren van back-ups die twee keer het volume geheugen vertegenwoordigen, gerust om aan te passen.   
Controleer of de opslagdoorvoer van de voor de verschillende voorgestelde volumes voldoet aan de werkbelasting die u wilt uitvoeren. Als de workload grotere volumes voor **/hana/gegevens** en **/hana/log**, moet u het aantal Azure Premium Storage-VHD's verhogen. Formaat van een volume met meer VHD's dan de vermelde verhoogt de IOPS-waarde, en i/o-doorvoer binnen de grenzen van het type virtuele machine van Azure. 

> [!NOTE]
> De bovenstaande configuraties niet veel voordeel hebben van [virtuele Azure-machine enkele VM SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_6/) omdat deze maakt gebruik van een combinatie van Azure Premium Storage en Azure Standard-opslag. De selectie is echter gekozen om kosten te optimaliseren. U moet kiezen Premium-opslag voor alle schijven boven die worden vermeld als Azure Standard-opslag (Sxx) om te voldoen aan de Azure SLA voor één virtuele machine maken van de VM-configuratie.


> [!NOTE]
> Vermeld aanbevelingen voor de configuratie van de schijf zijn die zijn gericht op minimumvereisten die SAP uitdrukking naar hun leveranciers van infrastructuur. In implementaties van echte klanten en -workloadscenario's, er zijn situaties waarin deze aanbevelingen nog steeds gaf geen voldoende mogelijkheden opgetreden. Dit kunnen zijn situaties waarin een klant een snellere opnieuw laden van de gegevens na het opnieuw opstarten HANA vereist of waarbij back-up maken van configuraties vereist meer bandbreedte naar de opslag. Andere gevallen opgenomen **/hana/log** waarbij 5000 IOP's zijn niet voldoende voor de specifieke werkbelasting. Dus nemen deze aanbevelingen als een punt en aan te passen op basis van de vereisten van de werkbelasting.
>  

## <a name="azure-ultra-ssd-storage-configuration-for-sap-hana"></a>Configuratie voor Azure Ultra SSD-opslag voor SAP HANA
Microsoft is momenteel de introductie van een nieuw Azure storage-type genoemd [Azure Ultra SSD](https://azure.microsoft.com/updates/ultra-ssd-new-azure-managed-disks-offering-for-your-most-latency-sensitive-workloads/). Het grote verschil tussen Azure-opslag die tot nu toe worden aangeboden en Ultra SSD is dat de mogelijkheden van de schijf niet aan de grootte van de schijf niet meer gebonden zijn. U kunt deze mogelijkheden voor Ultra SSD definiëren als een klant:

- Grootte van een schijf, variërend van 4 GiB in 65.536 GiB
- IOPS het bereik van 100 IOPS 160 kB IOPS (maximum aantal is afhankelijk van de VM-typen)
- Opslagdoorvoer van 300 MB per seconde naar 2.000 MB per seconde

Zoek voor meer informatie het artikel [Ultra SSD aankondiging – de volgende generatie van de schijven van de Azure-technologie (preview)](https://azure.microsoft.com/blog/announcing-ultra-ssd-the-next-generation-of-azure-disks-technology-preview/)

UltraSSD biedt u de mogelijkheid voor het definiëren van één schijf die voldoet aan uw grootte, IOPS en doorvoer bereik van schijf. In plaats van logische volume managers zoals LVM of MDADM boven op Azure Premium Storage om te maken van volumes die voldoen aan de IOPS en doorvoer van de opslagvereisten. U kunt een combinatie van de configuratie uitvoeren tussen UltraSSD en Premium-opslag. Als gevolg hiervan kunt u beperken van het gebruik van UltraSSD is op de prestaties van essentiële /hana/data en /hana/log volumes en betrekking hebben op de andere volumes met Azure Premium Storage

| VM-SKU | RAM | Met maximaal VM-I/O<br /> Doorvoer | / hana/gegevensvolume | / hana/gegevens-i/o-doorvoer | /hana/data IOPS | / hana/logboekvolume | / hana/log i/o-doorvoer | /hana/log IOPS |
| --- | --- | --- | --- | --- | --- | --- | --- | -- |
| M32ts | 192 GiB | 500 MB/s | 250 GB | 500 MBps | 7500 | 256 GB | 500 MBps  | 2000 |
| M32ls | 256 GiB | 500 MB/s | 300 GB | 500 MBps | 7500 | 256 GB | 500 MBps  | 2000 |
| M64ls | 512 GiB | 1000 MB/s | 600 GB | 500 MBps | 7500 | 512 GB | 500 MBps  | 2000 |
| M64s | 1000 GiB | 1000 MB/s |  1200 GB | 500 MBps | 7500 | 512 GB | 500 MBps  | 2000 |
| M64ms | 1750 GiB | 1000 MB/s | 2100 GB | 500 MBps | 7500 | 512 GB | 500 MBps  | 2000 |
| M128s | 2000 GiB | 2000 MB/s |2400 GB | 1200 MBps |9000 | 512 GB | 800 MBps  | 2000 | 
| M128ms | 3800 GiB | 2000 MB/s | 4800 GB | 1200 MBps |9000 | 512 GB | 800 MBps  | 2000 | 
| M208s_v2 | 2850 GiB | 1000 MB/s | 3500 GB | 1000 MBps | 9000 | 512 GB | 500 MBps  | 2000 | 
| M208ms_v2 | 5700 GiB | 1000 MB/s | 7200 GB | 1000 MBps | 9000 | 512 GB | 500 MBps  | 2000 | 
| M416s_v2 | 5700 GiB | 2000 MB/s | 7200 GB | 1500 M Bps | 9000 | 512 GB | 800 MBps  | 2000 | 
| M416ms_v2 | 11400 GiB | 2000 MB/s | 14400 GB | 1500 MBps | 9000 | 512 GB | 800 MBps  | 2000 |   

M416xx_v2 VM-typen worden niet nog beschikbaar gemaakt door Microsoft aan het publiek. De waarden in de lijst zijn bedoeld als een beginpunt en moeten worden geëvalueerd op basis van de echte eisen. Het voordeel van Azure Ultra SSD is dat de waarden voor IOPS en doorvoer kunnen worden aangepast zonder de noodzaak om de virtuele machine af te sluiten of stopgezet, kunt u de werkbelasting op het systeem toegepast.   

> [!NOTE]
> Storage-momentopnamen met UltraSSD opslag tot nu toe is niet beschikbaar. Het gebruik van momentopnamen van de VM met Azure Backup-Services geblokkeerd

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie:

- [Hoge beschikbaarheid van SAP HANA-handleiding voor Azure virtual machines](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-availability-overview).