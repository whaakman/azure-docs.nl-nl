---
title: bestand opnemen
description: bestand opnemen
services: virtual-machines
author: jonbeck7
ms.service: virtual-machines
ms.topic: include
ms.date: 07/06/2018
ms.author: azcspmt;jonbeck;cynthn
ms.custom: include file
ms.openlocfilehash: dbbfea183454b1068558111bf62b45f5fa6415cc
ms.sourcegitcommit: d2329d88f5ecabbe3e6da8a820faba9b26cb8a02
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/16/2019
ms.locfileid: "56333802"
---
Opslag geoptimaliseerde VM-grootten bieden een snelle doorvoer van schijfgegevens en IO- en zijn ideaal voor Big Data, SQL, NoSQL-databases, gegevensopslag en grote transactionele databases.  Voorbeelden zijn onder meer Cassandra, MongoDB, Cloudera en Redis. In dit artikel bevat informatie over het aantal vcpu's, gegevensschijven en NIC's, evenals lokale opslag en doorvoer netwerkbandbreedte voor elke grootte geoptimaliseerde.

De Lsv2-serie functies hoge doorvoer en lage latentie en rechtstreeks toegewezen lokale NVMe-opslag die wordt uitgevoerd op de [AMD EPYC &trade; 7551 processor](https://www.amd.com/en/products/epyc-7000-series) met een alle core versterking van 2.55 GHz en een maximale versterking van 3.0 GHz. De VM's uit de Lsv2-serie zijn verkrijgbaar in groottes van 8 tot 80 vCPU in een simultane multithreading configuratie.  Er is 8 GiB geheugen per vCPU en één 1,92 TB NVMe SSD M.2-apparaat per 8 vCPU's, met maximaal 19,2 TB (10 x 1,92 TB) beschikbaar op de L80s v2.

De Ls-serie biedt maximaal 32 vCPU's en maakt gebruik van een [Intel Xeon®-processor uit de E5 v3-familie](http://www.intel.com/content/www/us/en/processors/xeon/xeon-e5-solutions.html). De Ls-serie heeft dezelfde CPU-prestaties als de G/GS-serie en wordt geleverd met 8 GiB geheugen per vCPU.

> [!NOTE]
> De Lsv2-serie VM's zijn geoptimaliseerd voor het gebruik van de lokale schijf op het knooppunt rechtstreeks gekoppeld aan de virtuele machine, in plaats van met behulp van duurzame gegevensschijven.  Hiermee kunt u meer IOPs / doorvoer voor uw workloads.  De Lsv2 en Ls-serie bieden geen ondersteuning voor het maken van een lokale cache voor het verhogen van de IOPS haalbare door duurzame gegevensschijven. De hoge doorvoer en IOPS van de lokale schijf maakt de Lsv2 en Ls-serie VM's ideaal voor NoSQL-archieven, zoals Apache Cassandra en MongoDB waarmee gegevens worden gerepliceerd op meerdere virtuele machines voor persistentie in het geval van het uitvallen van één virtuele machine. 

## <a name="lsv2-series"></a>Lsv2-serie
ACU: 150-175

Premium-opslag: Ondersteund

Premium Storage Caching: Niet ondersteund

| Grootte          | vCPU | Geheugen (GiB) | Tijdelijke schijf<sup>1</sup> (GiB) | NVMe-schijven<sup>2</sup> | NVMe schijfdoorvoer<sup>3</sup> (IOPS voor lezen / MBps) | Cachegrootte hosten<sup>4</sup> | Maximum aantal gegevensschijven | Max. aantal NIC's / verwachte netwerkbandbreedte (Mbps) | 
|---------------|-----------|-------------|--------------------------|----------------|---------------------------------------------------|-------------------------------------------|------------------------------|------------------------------| 
| Standard_L8s_v2   |  8 |  64 |  80 |  1x1.92 TB  | 340,000 / 2,000 | N/A | 16 | 2 / 3,200  | 
| Standard_L16s_v2  | 16 | 128 | 160 |  2x1.92 TB  | 680,000 / 4,500 | N/A | 32 | 4 / 6,400  | 
| Standard_L32s_v2  | 32 | 256 | 320 |  4x1.92 TB  | 1.4 M / 9000    | N/A | 32 | 8 / 12,800 | 
| Standard_L64s_v2  | 64 | 512 | 640 |  8x1.92 TB  | 2.7 M / 18.000   | N/A | 32 | 8 / 25,600 |
| Standard_L80s_v2  | 80 | 640 | 800 | 10x1.92TB   | 3.4 M / 22,000   | N/A | 32 | 8 / 32,000 |
 
<sup>1</sup> Lsv2-serie VM's hebben een standaardschijf van de basis van tijdelijke resource SCSI voor gebruik van een wisselbestand/wisselen bestand besturingssystemen (D: op Windows, /dev/sdb op Linux). Deze schijf biedt 80 GiB van opslag, 4000 IOPS en 80 MBps overdrachtssnelheid voor elke 8 vcpu's (bijvoorbeeld Standard_L80s_v2 biedt 800 GiB 40.000 IOPS en 800 MBPS). Dit zorgt ervoor dat de NVMe-stations kunnen volledig worden toegewezen aan het gebruik. Deze schijf is een tijdelijke en alle gegevens gaan verloren op stoppen/toewijzing ongedaan maken.

<sup>2</sup> lokale NVMe-schijven zijn kortstondige, gegevens op deze schijven gaan verloren als u stoppen/toewijzing ongedaan maken uw virtuele machine.

<sup>3</sup> Hyper-V NVMe Direct-technologie biedt met toegang tot lokale NVMe-stations toegewezen veilig in de Gast-VM-ruimte.  Maximale prestaties te bereiken, moet met behulp van de meest recente WS2019 build of Ubuntu 18.04 of 16.04 vanuit Azure Marketplace.  Schrijfprestaties is afhankelijk van i/o-grootte, station laden en gebruik van capaciteit.

<sup>4</sup> Lsv2-serie VM's bieden geen hostcache voor gegevensschijf zoals de Lsv2-werkbelastingen niet profiteren.  Echter aankan Lsv2 virtuele machines van Azure-schijfoptie kortstondige besturingssysteem van de virtuele machine (maximaal 30 GiB). 



## <a name="ls-series"></a>Ls-serie
ACU: 180-240

Premium-opslag:  Ondersteund

Premium Storage Caching:  Niet ondersteund
 
| Grootte          | vCPU | Geheugen (GiB) | Tijdelijke opslag (GiB) | Max. aantal gegevensschijven | Maximale tijdelijke opslagdoorvoer (IOPS / MBps) | Belastingstests maximale schijfdoorvoer (IOPS / MBps) | Max. aantal NIC's / verwachte netwerkbandbreedte (Mbps) | 
|----------------|-----------|-------------|--------------------------|----------------|-------------------------------------------------------------|-------------------------------------------|------------------------------| 
| Standard_L4s   | 4  | 32  | 678   | 16 | 20,000 / 200 | 5000 / 125  | 2 / 4,000  | 
| Standard_L8s   | 8  | 64  | 1388 | 32 | 40,000 / 400 | 10.000 / 250 | 4 / 8,000  | 
| Standard_L16s  | 16 | 128 | 2807 | 64 | 80,000 / 800 | 20.000 / 500 | 8 / 16,000 | 
| Standard_L32s <sup>1</sup> | 32   | 256  | 5630 | 64   | 160,000 / 1,600   | 40.000 / 1,000     | 8 / 20,000 | 
 

De maximale schijfdoorvoer die mogelijk is met de Ls-serie VM's kan worden beperkt door het aantal, de grootte en de striping van de gekoppelde schijven. Zie voor meer informatie, [ontwerpen voor hoge prestaties](../articles/virtual-machines/windows/premium-storage-performance.md).

<sup>1</sup> exemplaar is geïsoleerd voor hardware toegewezen aan één klant.

## <a name="size-table-definitions"></a>De grootte van tabeldefinities wijzigen

- De opslagcapaciteit wordt weergegeven in GiB-eenheden of 1024^3 bytes. Wanneer u schijven die zijn gemeten in GB (1000^3 bytes), vergelijkt met schijven die zijn gemeten in GiB (1024^3), moet u voor ogen houden dat de capaciteit in GiB kleiner lijkt te zijn. 1023 GiB is bijvoorbeeld gelijk aan 1098,4 GB
- De schijfdoorvoer wordt gemeten in I/O-bewerkingen per seconde (IOPS) en MBps, waarbij MBps = 10^6 bytes per seconde.
- Als u de beste prestaties voor uw VM's wilt, moet u het aantal gegevensschijven aan 2 schijven per vCPU beperken.
- **Netwerkbandbreedte verwacht** is de maximale geaggregeerde [bandbreedte die is toegewezen per VM-type](../articles/virtual-network/virtual-machine-network-throughput.md) voor alle NIC's, voor alle bestemmingen. Bovengrenzen worden niet gegarandeerd, maar zijn bedoeld als richtlijn voor het selecteren van het juiste VM-type voor de bedoelde toepassing. De werkelijke netwerkprestaties zijn afhankelijk van verschillende factoren, waaronder netwerkcongestie, toepassingsbelastingen en netwerkinstellingen. Zie [Netwerkdoorvoer optimaliseren voor Windows en Linux](../articles/virtual-network/virtual-network-optimize-network-bandwidth.md) voor meer informatie over het optimaliseren van de netwerkdoorvoer. Om de verwachte netwerkprestaties op Linux en Windows te bereiken, kan het noodzakelijk zijn een specifieke versie te selecteren of de VM te optimaliseren. Zie [How to reliably test for virtual machine throughput](../articles/virtual-network/virtual-network-bandwidth-testing.md) (Betrouwbaar testen van de doorvoer van virtuele machines) voor meer informatie.
