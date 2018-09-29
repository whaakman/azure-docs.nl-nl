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
ms.openlocfilehash: 961f82cd4970abfdd11a30b2847a14f8ff1880b0
ms.sourcegitcommit: f31bfb398430ed7d66a85c7ca1f1cc9943656678
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/28/2018
ms.locfileid: "47454408"
---
Opslag geoptimaliseerde VM-grootten bieden een snelle doorvoer van schijfgegevens en IO- en zijn ideaal voor Big Data, SQL en NoSQL-databases. In dit artikel bevat informatie over het aantal vcpu's, gegevensschijven en NIC's, evenals de doorvoer en netwerkbandbreedte opslag voor elke grootte in deze groepering. 

De Ls-serie biedt maximaal 32 vCPU's en maakt gebruik van een [Intel Xeon®-processor uit de E5 v3-familie](http://www.intel.com/content/www/us/en/processors/xeon/xeon-e5-solutions.html). De Ls-serie heeft dezelfde CPU-prestaties als de G/GS-serie en wordt geleverd met 8 GiB geheugen per vCPU.  Virtuele machines uit de ls-serie zijn ideaal voor toepassingen die een lage latentie, hoge doorvoer en grote lokale schijfopslag vereisen. 

Voorbeelden van use cases zijn NoSQL-databases zoals Cassandra, MongoDB, Cloudera en Redis, gegevensopslag en grote transactionele databases.

> [!NOTE]
> De Ls-serie is geoptimaliseerd voor het gebruik van de tijdelijke schijf die is gekoppeld aan de VM-machine in plaats van duurzame gegevensschijven. De hoge doorvoer en IOPS van de tijdelijke schijf maakt de Ls-serie ideaal voor NoSQL-archieven, zoals Apache Cassandra en MongoDB waarmee gegevens worden gerepliceerd op meerdere virtuele machines voor persistentie in het geval van het uitvallen van één virtuele machine. De Ls-serie biedt geen ondersteuning voor het maken van een lokale cache voor het verhogen van de IOPS haalbare door duurzame gegevensschijven.

## <a name="ls-series"></a>Ls-serie

ACU: 180-240

Premium Storage: ondersteund

Premium Storage Caching: Niet ondersteund
 
| Grootte          | vCPU | Geheugen: GiB | Tijdelijke opslag (SSD) GiB | Max. aantal gegevensschijven | Maximale tijdelijke opslagdoorvoer: IOPS / MBps | Max. doorvoer voor schijf zonder caching: IOPS/MBps | Max. aantal NIC's / verwachte netwerkbandbreedte (Mbps) | 
|---------------|-----------|-------------|--------------------------|----------------|-------------------------------------------------------------|-------------------------------------------|------------------------------| 
| Standard_L4s   | 4    | 32   | 678   | 16    | 20,000 / 200   | 5000 / 125        | 2 / 4,000  | 
| Standard_L8s   | 8    | 64   | 1388 | 32   | 40,000 / 400   | 10.000 / 250       | 4 / 8,000  | 
| Standard_L16s  | 16   | 128  | 2807 | 64   | 80,000 / 800   | 20.000 / 500       | 8 / 16,000 | 
| Standard_L32s <sup>1</sup> | 32   | 256  | 5630 | 64   | 160,000 / 1,600   | 40.000 / 1,000     | 8 / 20,000 | 
 

De maximale schijfdoorvoer die mogelijk is met de Ls-serie VM's kan worden beperkt door het aantal, de grootte en de striping van de gekoppelde schijven. Zie [Premium Storage: High-performance storage for Azure virtual machine workloads](../articles/virtual-machines/windows/premium-storage.md) (Premium Storage: opslag met hoge prestaties voor Azure VM-workloads) voor meer informatie.

<sup>1</sup> exemplaar is geïsoleerd voor hardware toegewezen aan één klant.

## <a name="size-table-definitions"></a>De grootte van tabeldefinities wijzigen

- De opslagcapaciteit wordt weergegeven in GiB-eenheden of 1024^3 bytes. Wanneer u schijven die zijn gemeten in GB (1000^3 bytes), vergelijkt met schijven die zijn gemeten in GiB (1024^3), moet u voor ogen houden dat de capaciteit in GiB kleiner lijkt te zijn. 1023 GiB is bijvoorbeeld gelijk aan 1098,4 GB
- De schijfdoorvoer wordt gemeten in I/O-bewerkingen per seconde (IOPS) en MBps, waarbij MBps = 10^6 bytes per seconde.
- Als u de beste prestaties voor uw VM's wilt, moet u het aantal gegevensschijven aan 2 schijven per vCPU beperken.
- **Netwerkbandbreedte verwacht** is de maximale geaggregeerde [bandbreedte die is toegewezen per VM-type](../articles/virtual-network/virtual-machine-network-throughput.md) voor alle NIC's, voor alle bestemmingen. Bovengrenzen worden niet gegarandeerd, maar zijn bedoeld als richtlijn voor het selecteren van het juiste VM-type voor de bedoelde toepassing. De werkelijke netwerkprestaties zijn afhankelijk van verschillende factoren, waaronder netwerkcongestie, toepassingsbelastingen en netwerkinstellingen. Zie [Netwerkdoorvoer optimaliseren voor Windows en Linux](../articles/virtual-network/virtual-network-optimize-network-bandwidth.md) voor meer informatie over het optimaliseren van de netwerkdoorvoer. Om de verwachte netwerkprestaties op Linux en Windows te bereiken, kan het noodzakelijk zijn een specifieke versie te selecteren of de VM te optimaliseren. Zie [How to reliably test for virtual machine throughput](../articles/virtual-network/virtual-network-bandwidth-testing.md) (Betrouwbaar testen van de doorvoer van virtuele machines) voor meer informatie.
