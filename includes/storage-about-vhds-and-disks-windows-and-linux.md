---
title: bestand opnemen
description: bestand opnemen
services: storage
author: roygara
ms.service: storage
ms.topic: include
ms.date: 04/09/2018
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 76ccbb3eabbf7186498ad0d57753314bdeffc2ea
ms.sourcegitcommit: 345b96d564256bcd3115910e93220c4e4cf827b3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/28/2018
ms.locfileid: "52585680"
---
## <a name="about-vhds"></a>Over VHD's

De VHD's die worden gebruikt in Azure zijn .vhd-bestanden opgeslagen als pagina-blobs in een Standard Storage- of Premium Storage-account in Azure. Zie [Understanding block blobs and page blobs](/rest/api/storageservices/Understanding-Block-Blobs--Append-Blobs--and-Page-Blobs/) (Inzicht in blok-blobs en pagina-blobs) voor meer informatie over pagina-blobs. Zie [High-performance premium storage and Azure VMs](../articles/virtual-machines/windows/premium-storage.md) (Hoogwaardige Premium Storage en virtuele Azure-machines) voor meer informatie over Premium Storage.

Azure biedt ondersteuning voor de VHD-indeling met vaste schijf. Bij een vaste indeling wordt de logische schijf lineair ingedeeld binnen het bestand, zodat de schijf-offset X wordt opgeslagen op blob-offset X. Een kleine voettekst aan het einde van de blob beschrijft de eigenschappen van de VHD. De vaste indeling afval ruimte vaak het geval is, omdat de meeste schijven grote ongebruikte bereiken erin bevatten. Azure slaat VHD-bestanden echter in een sparse-indeling op, zodat u profiteert van de voordelen van zowel vaste als dynamische schijven. Zie voor meer informatie, [aan de slag met virtuele harde schijven](https://technet.microsoft.com/library/dd979539.aspx).

Alle VHD-bestanden in Azure die u gebruiken als bron wilt voor het maken van schijven of installatiekopieën zijn alleen-lezen, met uitzondering van de VHD-bestanden geüpload naar of gekopieerd naar Azure storage door de gebruiker (die kan worden alleen-lezen of alleen-lezen). Wanneer u een schijf of installatiekopie maakt, maakt Azure kopieën van de bron-VHD-bestanden. Deze kopieën kunnen alleen-lezen of lezen/schrijven zijn, afhankelijk van hoe u de VHD gebruikt.

Wanneer u een virtuele machine van een installatiekopie maakt, maakt Azure een schijf voor de virtuele machine die een kopie is van het VHD-bronbestand. Als bescherming tegen onbedoeld verwijderen, plaatst Azure een lease op elk VHD-bronbestand dat wordt gebruikt voor het maken van een installatiekopie, een besturingssysteemschijf of een gegevensschijf.

Voordat u een VHD-bronbestand kunt verwijderen, dient u de lease te verwijderen door de schijf of installatiekopie te verwijderen. Als u een VHD-bestand wilt verwijderen dat door een virtuele machine wordt gebruikt als besturingssysteemschijf, kunt u de virtuele machine, de besturingssysteemschijf en het VHD-bronbestand in één keer verwijderen door de virtuele machine en alle gekoppelde schijven te verwijderen. Het verwijderen van een VHD-bronbestand voor een gegevensschijf vereist echter een aantal stappen in een vaste volgorde. Koppel eerst de schijf los van de virtuele machine, verwijder daarna de schijf en verwijder vervolgens het VHD-bestand.

> [!WARNING]
> Als u een VHD-bronbestand uit de opslag verwijdert of uw opslagaccount verwijdert, kan Microsoft die gegevens niet voor u herstellen.

## <a name="types-of-disks"></a>Typen schijven

Azure-schijven zijn ontworpen om 99,999% van de tijd beschikbaar te zijn. Azure-schijven hebben dan geavanceerde duurzaamheid, met een toonaangevende Annualized foutpercentage van nul % consistent geleverd.

Er zijn drie prestatielagen voor opslag die u kiezen kunt uit bij het maken van uw schijven--Premium SSD-schijven, Standard-SSD en HDD standaardopslag. Er zijn ook twee soorten schijven, niet-beheerd en beheerd.

### <a name="standard-hdd-disks"></a>Standaard harde schijven

Standaard harde schijven worden ondersteund door HDD's en rendabele opslag bieden. Standard HDD-opslag kan lokaal worden gerepliceerd in één datacenter of kan geografisch redundant met primaire en secundaire datacenters. Zie voor meer informatie over de storage-replicatie, [Azure Storage-replicatie](../articles/storage/common/storage-redundancy.md).

Zie voor meer informatie over het gebruik van standaard harde schijven [Standard Storage en schijven](../articles/virtual-machines/windows/standard-storage.md).

### <a name="standard-ssd-disks"></a>Standard-SSD-schijven

Standard-SSD-schijven zijn ontworpen voor het adres van de dezelfde soort werkbelastingen als standaard harde schijven, maar bieden consistente prestaties en betrouwbaarheid is dan de harde schijf. Standard-SSD-schijven combineren elementen van de Premium-SSD-schijven en standaard harde schijven om een kosteneffectieve oplossing het beste geschikt voor toepassingen, zoals webservers die hoge IOPS op schijven niet meer nodig. Indien beschikbaar, zijn Standard-SSD-schijven in de aanbevolen Implementatieoptie voor de meeste workloads. Standard-SSD-schijven zijn beschikbaar als beheerde schijven in alle regio's, maar zijn momenteel alleen beschikbaar met het tolerantietype voor lokaal redundante opslag (LRS).

### <a name="premium-ssd-disks"></a>Premium-SSD-schijven

Premium-SSD-schijven worden ondersteund door SSD's en biedt ondersteuning voor hoge prestaties en lage latentie schijven voor virtuele machines met I/O-intensieve workloads. Doorgaans kunt u Premium-SSD-schijven met grootten die een "s" in de naam van de reeks. Bijvoorbeeld, er is de Dv3-serie en de Dsv3-serie, de Dsv3-serie met Premium-SSD-schijven kunnen worden gebruikt.  Ga voor meer informatie naar [Premium Storage](../articles/virtual-machines/windows/premium-storage.md).

### <a name="unmanaged-disks"></a>Niet-beheerde schijven

Oorspronkelijk werden niet-beheerde schijven gebruikt door virtuele machines. Met deze schijven die u kunt uw eigen opslagaccount maakt en dat opslagaccount opgeeft bij het maken van de schijf. Zorg ervoor dat u niet te veel schijven geplaatst in hetzelfde opslagaccount, omdat u kan groter zijn dan de [schaalbaarheidsdoelen](../articles/storage/common/storage-scalability-targets.md) van het opslagaccount (20.000 IOP's, bijvoorbeeld), wat resulteert in de virtuele machines worden vertraagd. Met niet-beheerde schijven moet u bepalen hoe u het beste gebruik kunt maken van een of meer opslagaccounts om de beste prestaties voor uw virtuele machines te realiseren.

### <a name="managed-disks"></a>Managed Disks

Managed Disks beheert het maken/beheren van het opslagaccount op de achtergrond en zorgt ervoor dat u zich geen zorgen hoeft te maken over de schaalbaarheidslimieten van het opslagaccount. U hoeft alleen maar de schijfgrootte en prestatielaag (Standard/Premium) op te geven en Azure maakt en beheert de schijf voor u. Ook als u schijven toevoegt of de virtuele machine omhoog of omlaag schaalt, hoeft u zich geen zorgen te maken over de gebruikte opslag.

U kunt ook uw aangepaste installatiekopieën in één opslagaccount per Azure-regio beheren en deze gebruiken om honderden virtuele machines onder hetzelfde abonnement te maken. Voor meer informatie over Managed Disks raadpleegt u [Managed Disks Overview](../articles/virtual-machines/windows/managed-disks-overview.md) (Overzicht van Managed Disks).

We raden u aan Azure Managed Disks te gebruiken voor nieuwe virtuele machines en uw vorige niet-beheerde schijven te converteren naar beheerde schijven, zodat u kunt profiteren van de functies in Managed Disks.

### <a name="disk-comparison"></a>Vergelijking van schijven

De volgende tabel bevat een vergelijking van de standaard harde schijven, Standard-SSD en Premium SSD voor niet-beheerde en beheerde schijven om te bepalen wat u wilt gebruiken. Grootte die is gemarkeerd met een sterretje zijn momenteel in preview.

|    | Azure Premium Disk |Azure Standard-SSD-schijf | Azure Standard harde schijf
|--- | ------------------ | ------------------------------- | -----------------------
| Schijftype | Solid-state drives (SSD) | Solid-state drives (SSD) | Hardeschijfstation (HDD)  
| Overzicht  | Hoogwaardige schijfondersteuning met lage latentie op basis van SSD's voor virtuele machines die IO-intensieve workloads uitvoeren of een bedrijfskritieke productieomgeving hosten |Consistente prestaties en betrouwbaarheid is dan de harde schijf. Geoptimaliseerd voor werkbelastingen met lage IOPS| Goedkoper zijn met schijf op basis van HDD voor incidentele toegang
| Scenario  | Productie- en prestatiegevoelige workloads |Webservers, bedrijfstoepassingen weinig wordt gebruikt en Dev/Test| Back-up, niet-kritieke, incidentele toegang
| Schijfgrootte | P4: 32 GiB (alleen voor beheerde schijven)<br>P6: 64 GiB (alleen voor beheerde schijven)<br>P10: 128 GiB<br>P15: 256 GB (alleen voor beheerde schijven)<br>P20: 512 GiB<br>P30: 1024 GiB<br>P40: 2048 GiB<br>P50: 4095 GiB<br>P60: 8192 GiB * (8 TiB)<br>P70: 16.384 GiB * (16 TiB)<br>P80: 32.767 GiB * (32 TiB) |Beheerde schijven alleen:<br>E10: 128 GiB<br>E15: 256 GB<br>E20: 512 GiB<br>E30: 1024 GiB<br>E40: 2048 GiB<br>E50: 4095 GiB<br>E60: 8192 GiB * (8 TiB)<br>E70: 16.384 GiB * (16 TiB)<br> E80: 32.767 GiB * (32 TiB) | Niet-beheerde schijven: 1 GiB – 4 TiB (4095 GiB) <br><br>Beheerde schijven:<br> S4: 32 GiB <br>S6: 64 GiB <br>S10: 128 GiB <br>S15: 256 GB <br>S20: 512 GiB <br>S30: 1024 GiB <br>S40: 2048 GiB<br>S50: 4095 GiB<br>S60: 8192 GiB * (8 TiB)<br>S70: 16.384 GiB * (16 TiB)<br>S80: 32.767 GiB * (32 TiB)
| Max. doorvoer per schijf | P4: 25 MiB/s<br> P6: 50 MiB/s<br> P10: 100 MiB/s<br> P15: 125 MiB/s<br> P20: 150 MiB/s<br> P30: 200 MiB/s<br> P40-P50: 250 MiB/s<br> P60: 480 MiB/s *<br> P70-P80: 750 MiB/s * | E10-E50: tot 60 MiB/s<br> E60: maximaal 300 MiB/s *<br> E70-E80: 500 MiB/s *| S4 - S50: Upt o 60 MiB/s<br> S60: maximaal 300 MiB/s *<br> S70-S80: tot 500 MiB/s *
| Max. IOP's per schijf | P4: 120 IOPS<br> P6: 240 IOPS<br> P10: 500 IOP 'S<br> P15: 1100 IOPS<br> P20: 2300 IOPS<br> P30: 5000 IOP 'S<br> P40-P50: 7500 IOPS<br> P60: 12.500 IOP 'S *<br> P70: 15.000 IOPS *<br> P80: 20.000 IOP 'S * | E10-E50: maximaal 500 IOP 's<br> E60: maximaal 1300 IOP's *<br> E70-E80: maximaal 2000 IOP's * | S4-S50: maximaal 500 IOP 's<br> S60: maximaal 1300 IOP's *<br> S70-S80: maximaal 2000 IOP's *
