---
title: bestand opnemen
description: bestand opnemen
services: storage
author: rogara
ms.service: storage
ms.topic: include
ms.date: 04/09/2018
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: f64645db782b055e1c544f257149411f29fc99d7
ms.sourcegitcommit: 3c3488fb16a3c3287c3e1cd11435174711e92126
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/08/2018
ms.locfileid: "34806312"
---
## <a name="about-vhds"></a>Over VHD's
De VHD's die worden gebruikt in Azure zijn .vhd-bestanden opgeslagen als pagina-blobs in een Standard Storage- of Premium Storage-account in Azure. Zie [Understanding block blobs and page blobs](/rest/api/storageservices/Understanding-Block-Blobs--Append-Blobs--and-Page-Blobs/) (Inzicht in blok-blobs en pagina-blobs) voor meer informatie over pagina-blobs. Zie [High-performance premium storage and Azure VMs](../articles/virtual-machines/windows/premium-storage.md) (Hoogwaardige Premium Storage en virtuele Azure-machines) voor meer informatie over Premium Storage.

Azure biedt ondersteuning voor de VHD-indeling met vaste schijf. Bij een vaste indeling wordt de logische schijf lineair ingedeeld binnen het bestand, zodat de schijf-offset X wordt opgeslagen op blob-offset X. Een kleine voettekst aan het einde van de blob beschrijft de eigenschappen van de VHD. De vaste indeling afval ruimte vaak, omdat de meeste schijven grote ongebruikte bereiken bevat hebben. Azure slaat VHD-bestanden echter in een sparse-indeling op, zodat u profiteert van de voordelen van zowel vaste als dynamische schijven. Zie voor meer informatie [aan de slag met virtuele harde schijven](https://technet.microsoft.com/library/dd979539.aspx).

Alle VHD-bestanden in Azure die u gebruiken als bron wilt voor het maken van schijven of installatiekopieën zijn alleen-lezen, met uitzondering van de VHD-bestanden geüpload of gekopieerd naar Azure-opslag door de gebruiker (die kan worden alleen-lezen of alleen-lezen). Wanneer u een schijf of installatiekopie maakt, maakt Azure kopieën van de bron-VHD-bestanden. Deze kopieën kunnen alleen-lezen of lezen/schrijven zijn, afhankelijk van hoe u de VHD gebruikt.

Wanneer u een virtuele machine van een installatiekopie maakt, maakt Azure een schijf voor de virtuele machine die een kopie is van het VHD-bronbestand. Als bescherming tegen onbedoeld verwijderen, plaatst Azure een lease op elk VHD-bronbestand dat wordt gebruikt voor het maken van een installatiekopie, een besturingssysteemschijf of een gegevensschijf.

Voordat u een VHD-bronbestand kunt verwijderen, dient u de lease te verwijderen door de schijf of installatiekopie te verwijderen. Als u een VHD-bestand wilt verwijderen dat door een virtuele machine wordt gebruikt als besturingssysteemschijf, kunt u de virtuele machine, de besturingssysteemschijf en het VHD-bronbestand in één keer verwijderen door de virtuele machine en alle gekoppelde schijven te verwijderen. Het verwijderen van een VHD-bronbestand voor een gegevensschijf vereist echter een aantal stappen in een vaste volgorde. Koppel eerst de schijf los van de virtuele machine, verwijder daarna de schijf en verwijder vervolgens het VHD-bestand.

> [!WARNING]
> Als u een VHD-bronbestand uit de opslag verwijdert of uw opslagaccount verwijdert, kan Microsoft die gegevens niet voor u herstellen.

## <a name="types-of-disks"></a>Typen schijven 
Azure-schijven zijn ontworpen om 99,999% van de tijd beschikbaar te zijn. Azure-schijven hebben dan bedrijfsniveau duurzaamheid met een toonaangevende nul % Annualized Faalpercentage consistent geleverd.

Er zijn drie prestatielagen voor opslag die u kiezen kunt uit bij het maken van uw schijven--Premium SSD-schijven, standaard SSD (Preview) en Standard HDD-opslag. Er zijn ook twee soorten schijven--zonder begeleiding en beheerd.

### <a name="standard-hdd-disks"></a>Standaard harde schijven
Standaard harde schijven worden ondersteund door HDD's en rendabele opslag bieden. Standard HDD-opslag kan lokaal worden gerepliceerd in een datacenter of worden geografisch redundante met primaire en secundaire datacenters. Zie voor meer informatie over de storage-replicatie, [Azure Storage-replicatie](../articles/storage/common/storage-redundancy.md). 

Zie voor meer informatie over het gebruik van standaard harde schijven [Standard-opslag en schijven](../articles/virtual-machines/windows/standard-storage.md).

### <a name="standard-ssd-disks-preview"></a>Standaard SSD-schijven (preview)
Standaard SSD-schijven zijn ontworpen om het adres van de werkbelasting van hetzelfde type als standaard harde schijven, maar bieden consistente prestaties en betrouwbaarheid is dan de harde schijf. Standaard SSD-schijven combineren elementen van Premium-SSD-schijven en standaard harde schijven aan formulier een rendabele oplossing het beste geschikt voor toepassingen, zoals webservers die hoge IOPS op schijven niet meer nodig. Indien beschikbaar, zijn standaard SSD-schijven in de aanbevolen implementatiemethode voor de meeste belastingen. Standaard SSD-schijven zijn alleen beschikbaar als schijven worden beheerd en tijdens het Preview-versie zijn alleen beschikbaar in [regio's selecteren](../articles/virtual-machines/windows/faq-for-disks.md) en met het tolerantietype lokaal redundante opslag (LRS).

### <a name="premium-ssd-disks"></a>Premium-SSD-schijven 
Premium-SSD-schijven zijn SSD's, en biedt ondersteuning voor hoge prestaties, lage latentie schijven voor virtuele machines waarop I/O-intensieve werkbelastingen worden uitgevoerd. Doorgaans kunt u Premium-SSD-schijven met grootten die een "s" in de naam van de gegevensreeks opnemen. Bijvoorbeeld: Er is de Dv3-reeks en de Dsv3-serie, de Dsv3-serie kunnen worden gebruikt met Premium-SSD-schijven.  Ga voor meer informatie naar [Premium Storage](../articles/virtual-machines/windows/premium-storage.md).

### <a name="unmanaged-disks"></a>Niet-beheerde schijven
Oorspronkelijk werden niet-beheerde schijven gebruikt door virtuele machines. Met deze schijven die u kunt uw eigen storage-account maken en dit opslagaccount opgeven wanneer u de schijf maken. Zorg ervoor dat u niet te veel schijven in hetzelfde opslagaccount plaatsen omdat u kan overschrijdt de [schaalbaarheidsdoelen](../articles/storage/common/storage-scalability-targets.md) van het opslagaccount (20.000 IOPS, bijvoorbeeld), wat resulteert in de virtuele machines wordt beperkt. Met niet-beheerde schijven moet u bepalen hoe u het beste gebruik kunt maken van een of meer opslagaccounts om de beste prestaties voor uw virtuele machines te realiseren.

### <a name="managed-disks"></a>Managed Disks 
Managed Disks beheert het maken/beheren van het opslagaccount op de achtergrond en zorgt ervoor dat u zich geen zorgen hoeft te maken over de schaalbaarheidslimieten van het opslagaccount. U hoeft alleen maar de schijfgrootte en prestatielaag (Standard/Premium) op te geven en Azure maakt en beheert de schijf voor u. Ook als u schijven toevoegt of de virtuele machine omhoog of omlaag schaalt, hoeft u zich geen zorgen te maken over de gebruikte opslag. 

U kunt ook uw aangepaste installatiekopieën in één opslagaccount per Azure-regio beheren en deze gebruiken om honderden virtuele machines onder hetzelfde abonnement te maken. Voor meer informatie over Managed Disks raadpleegt u [Managed Disks Overview](../articles/virtual-machines/windows/managed-disks-overview.md) (Overzicht van Managed Disks).

We raden u aan Azure Managed Disks te gebruiken voor nieuwe virtuele machines en uw vorige niet-beheerde schijven te converteren naar beheerde schijven, zodat u kunt profiteren van de functies in Managed Disks.

### <a name="disk-comparison"></a>Vergelijking van schijven
De volgende tabel bevat een vergelijking van standaard harde schijven, SSD standaard en Premium-SSD voor niet-beheerde en beheerde schijven om te bepalen wat moet worden gebruikt.

|    | Azure Premium Disk |Azure Standard SSD-schijven (preview)| Azure Standard harde schijf 
|--- | ------------------ | ------------------------------- | ----------------------- 
| Schijftype | Solid-state drives (SSD) | Solid-state drives (SSD) | Hardeschijfstation (HDD)  
| Overzicht  | Hoogwaardige schijfondersteuning met lage latentie op basis van SSD's voor virtuele machines die IO-intensieve workloads uitvoeren of een bedrijfskritieke productieomgeving hosten |Consistente prestaties en betrouwbaarheid is dan de harde schijf. Geoptimaliseerd voor lage IOPS werkbelastingen| Rendabel schijf voor incidentele toegang op basis van een harde schijf
| Scenario  | Productie- en prestatiegevoelige workloads |Webservers licht gebruikte bedrijfstoepassingen en ontwikkelen en testen| Back-up, niet-kritieke, incidentele toegang 
| Schijfgrootte | P4: 32 GiB (alleen beheerde schijven)<br>P6: 64 GiB (alleen beheerde schijven)<br>P10: 128 GiB<br>P15: 256 GiB (alleen beheerde schijven)<br>P20: 512 GiB<br>P30: 1024 GiB<br>P 40: 2048 GiB<br>P50: 4095 GiB |Schijven alleen beheerd:<br>E10: 128 GiB<br>E15: 256 GiB<br>E20: 512 GiB<br>E30: 1024 GiB<br>E40: 2048 GiB<br>E50: 4095 GiB | Niet-beheerde schijven: 1 GiB – 4 TiB (4095 GiB) <br><br>Beheerde schijven:<br> S4: 32 GiB <br>S6: 64 GiB <br>S10: 128 GiB <br>S15: 256 GiB <br>S20: 512 GiB <br>S30: 1024 GiB <br>S40: 2048 GiB<br>Aanbeveling S50: 4095 GiB
| Max. doorvoer per schijf | 250 MiB/s | Maximaal 60 MiB/s | Maximaal 60 MiB/s 
| Max. IOP's per schijf | 7500 IOP 'S | Maximaal 500 IOP's | Maximaal 500 IOP's 