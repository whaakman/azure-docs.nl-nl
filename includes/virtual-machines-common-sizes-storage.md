---
title: bestand opnemen
description: bestand opnemen
services: virtual-machines
author: jonbeck7
ms.service: virtual-machines
ms.topic: include
ms.date: 04/17/2019
ms.author: azcspmt;jonbeck;cynthn
ms.custom: include file
ms.openlocfilehash: 5123ee3f65744f3d0c255712efe990b01be58e26
ms.sourcegitcommit: c05618a257787af6f9a2751c549c9a3634832c90
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/30/2019
ms.locfileid: "66420706"
---
Opslag geoptimaliseerde VM-grootten bieden een snelle doorvoer van schijfgegevens en IO- en zijn ideaal voor Big Data, SQL, NoSQL-databases, gegevensopslag en grote transactionele databases.  Voorbeelden zijn onder meer Cassandra, MongoDB, Cloudera en Redis. In dit artikel bevat informatie over het aantal vcpu's, gegevensschijven en NIC's, evenals lokale opslag en doorvoer netwerkbandbreedte voor elke grootte geoptimaliseerde.

De Lsv2-serie functies hoge doorvoer en lage latentie en rechtstreeks toegewezen lokale NVMe-opslag die wordt uitgevoerd op de [AMD EPYC &trade; 7551 processor](https://www.amd.com/en/products/epyc-7000-series) met een alle core versterking van 2.55 GHz en een maximale versterking van 3.0 GHz. De VM's uit de Lsv2-serie zijn verkrijgbaar in groottes van 8 tot 80 vCPU in een simultane multithreading configuratie.  Er is 8 GiB geheugen per vCPU en één 1,92 TB NVMe SSD M.2-apparaat per 8 vCPU's, met maximaal 19,2 TB (10 x 1,92 TB) beschikbaar op de L80s v2.

> [!NOTE]
> De Lsv2-serie VM's zijn geoptimaliseerd voor het gebruik van de lokale schijf op het knooppunt rechtstreeks gekoppeld aan de virtuele machine, in plaats van met behulp van duurzame gegevensschijven. Hiermee kunt u meer IOPs / doorvoer voor uw workloads. De Lsv2 en Ls-serie bieden geen ondersteuning voor het maken van een lokale cache voor het verhogen van de IOPs haalbare door duurzame gegevensschijven.
>
> De hoge doorvoer en IOPs van de lokale schijf maakt de Lsv2 en Ls-serie VM's ideaal voor NoSQL-archieven, zoals Apache Cassandra en MongoDB waarmee gegevens worden gerepliceerd op meerdere virtuele machines voor persistentie in het geval van het uitvallen van één virtuele machine.
>
> Zie voor meer informatie, [optimaliseren op de virtuele machines van de Lsv2-serie](../articles/virtual-machines/linux/storage-performance.md).  


## <a name="lsv2-series"></a>Lsv2-serie

ACU: 150-175

Premium-opslag: Ondersteund

Premium Storage opslaan in cache: Niet ondersteund

| Grootte          | vCPU | Geheugen (GiB) | Tijdelijke schijf<sup>1</sup> (GiB) | NVMe-schijven<sup>2</sup> | NVMe schijfdoorvoer<sup>3</sup> (IOPS voor lezen / MBps) | Maximum aantal schijfbewerkingen zonder gegevens schijfdoorvoer (IOPs/MBps)<sup>4</sup> | Maximum aantal gegevensschijven | Max. aantal NIC's / verwachte netwerkbandbreedte (Mbps) |
|---------------|-----------|-------------|--------------------------|----------------|---------------------------------------------------|-------------------------------------------|------------------------------|------------------------------| 
| Standard_L8s_v2   |  8 |  64 |  80 |  1x1.92 TB  | 400000 / 2000 | 8000/160 | 16 | 2 / 3200  |
| Standard_L16s_v2  | 16 | 128 | 160 |  2x1.92 TB  | 800000 / 4000 | 16000/320 | 32 | 4 / 6400  |
| Standard_L32s_v2  | 32 | 256 | 320 |  4x1.92 TB  | 1,5 MILJOEN / 8000    | 32000/640 | 32 | 8 / 12800 |
| Standard_L64s_v2  | 64 | 512 | 640 |  8x1.92 TB  | 2.9 M / 16000   | 64000/1280 | 32 | 8 / 16000+ |
| Standard_L80s_v2<sup>5</sup> | 80 | 640 | 800 | 10x1.92TB   | 3.8 M / 20000   | 80000/1400 | 32 | 8 / 16000+ |

<sup>1</sup> Lsv2-serie VM's hebben een standaardschijf van de basis van tijdelijke resource SCSI voor gebruik van een wisselbestand/wisselen bestand besturingssystemen (D: op Windows, /dev/sdb op Linux). Deze schijf biedt 80 GiB van opslag, 4000 IOPS en 80 MBps overdrachtssnelheid voor elke 8 vcpu's (bijvoorbeeld Standard_L80s_v2 biedt 800 GiB 40.000 IOPS en 800 MBPS). Dit zorgt ervoor dat de NVMe-stations kunnen volledig worden toegewezen aan het gebruik. Deze schijf is een tijdelijke en alle gegevens gaan verloren op stoppen/toewijzing ongedaan maken.

<sup>2</sup> lokale NVMe-schijven zijn kortstondige, gegevens op deze schijven gaan verloren als u stoppen/toewijzing ongedaan maken uw virtuele machine.

<sup>3</sup> Hyper-V NVMe Direct-technologie biedt met toegang tot lokale NVMe-stations toegewezen veilig in de Gast-VM-ruimte.  Maximale prestaties te bereiken, moet met behulp van de meest recente WS2019 build of Ubuntu 18.04 of 16.04 vanuit Azure Marketplace.  Schrijfprestaties is afhankelijk van i/o-grootte, station laden en gebruik van capaciteit.

<sup>4</sup> Lsv2-serie VM's bieden geen hostcache voor gegevensschijf zoals de Lsv2-werkbelastingen niet profiteren.  Echter aankan Lsv2 virtuele machines van Azure-schijfoptie kortstondige besturingssysteem van de virtuele machine (maximaal 30 GiB).

<sup>5</sup> virtuele machines met meer dan 64 vcpu's een van deze ondersteunde gastbesturingssystemen vereist:
- WindowsServer 2016 of hoger
- Ubuntu 16.04 LTS of hoger, met Azure is afgestemd op kernel (4.15 kernel of hoger)
- SLES 12 SP2 of hoger
- RHEL of CentOS-versie 6.7 t/m 6.10, met Microsoft beschikbaar LIS pakket 4.3.1 (of hoger) geïnstalleerd
- RHEL of CentOS-versie 7.3, met een pakket van Microsoft beschikbaar LIS 4.2.1 (of hoger) geïnstalleerd
- RHEL of CentOS-versie 7.4 of hoger
- Oracle Linux met UEK4 of hoger
- Debian 9 met de kernel backports, Debian, 10 of hoger
- CoreOS met een 4.14 kernel of hoger


## <a name="size-table-definitions"></a>De grootte van tabeldefinities wijzigen

- De opslagcapaciteit wordt weergegeven in GiB-eenheden of 1024^3 bytes. Wanneer u schijven die zijn gemeten in GB (1000^3 bytes), vergelijkt met schijven die zijn gemeten in GiB (1024^3), moet u voor ogen houden dat de capaciteit in GiB kleiner lijkt te zijn. 1023 GiB is bijvoorbeeld gelijk aan 1098,4 GB
- De schijfdoorvoer wordt gemeten in I/O-bewerkingen per seconde (IOPS) en MBps, waarbij MBps = 10^6 bytes per seconde.
- Als u de beste prestaties voor uw VM's wilt, moet u het aantal gegevensschijven aan 2 schijven per vCPU beperken.
- **Netwerkbandbreedte verwacht** is de maximale geaggregeerde [bandbreedte die is toegewezen per VM-type](../articles/virtual-network/virtual-machine-network-throughput.md) voor alle NIC's, voor alle bestemmingen. Bovengrenzen worden niet gegarandeerd, maar zijn bedoeld als richtlijn voor het selecteren van het juiste VM-type voor de bedoelde toepassing. De werkelijke netwerkprestaties zijn afhankelijk van verschillende factoren, waaronder netwerkcongestie, toepassingsbelastingen en netwerkinstellingen. Zie [Netwerkdoorvoer optimaliseren voor Windows en Linux](../articles/virtual-network/virtual-network-optimize-network-bandwidth.md) voor meer informatie over het optimaliseren van de netwerkdoorvoer. Om de verwachte netwerkprestaties op Linux en Windows te bereiken, kan het noodzakelijk zijn een specifieke versie te selecteren of de VM te optimaliseren. Zie [How to reliably test for virtual machine throughput](../articles/virtual-network/virtual-network-bandwidth-testing.md) (Betrouwbaar testen van de doorvoer van virtuele machines) voor meer informatie.
