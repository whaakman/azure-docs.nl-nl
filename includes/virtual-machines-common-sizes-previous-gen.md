---
title: bestand opnemen
description: bestand opnemen
services: virtual-machines-windows, virtual-machines-linux
author: cynthn
ms.service: multiple
ms.topic: include
ms.date: 04/11/2019
ms.author: cynthn;azcspmt;jonbeck
ms.custom: include file
ms.openlocfilehash: d0802cfcf05874044b6e116ba194c16a79f9d309
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60541596"
---
In deze sectie bevat informatie over eerdere generaties groottes van virtuele machines. Met deze grootten zijn nog steeds kunnen worden gebruikt, maar er zijn nieuwere generaties beschikbaar. 

## <a name="f-series"></a>F-serie

De F-serie is gebaseerd op de 2,4 GHz Intel Xeon® E5-2673 v3 (Haswell)-processor, die met Intel Turbo Boost Technology 2.0 een kloksnelheid kan halen van 3,1 GHz. Dit zijn dezelfde CPU-prestaties als de virtuele machines uit de Dv2-serie.  

Virtuele machines uit de F-serie vormen een uitstekende keuze voor workloads die snellere CPU's nodig hebben, maar niet zo veel geheugen of tijdelijke opslag per vCPU.  Voor workloads voor analysen, gameservers, webservers en batchverwerking is de F-serie wellicht een betere keuze.

ACU: 210 - 250

Premium-opslag:  Niet ondersteund

Premium Storage Caching:  Niet ondersteund

| Grootte         | vCPU | Geheugen: GiB | Tijdelijke opslag (SSD) GiB | Maximale tijdelijke opslagdoorvoer: IOPS / MBps lezen / MBps schrijven | Max. aantal gegevensschijven / doorvoer: IOPS | Max. aantal NIC's / verwachte netwerkbandbreedte (Mbps) |
|--------------|-----------|-------------|----------------|----------------------------------------------------------|-----------------------------------|------------------------------|
| Standard_F1  | 1         | 2           | 16             | 3000 / 46 / 23                                           | 4 / 4 x 500                         | 2 / 750                 |
| Standard_F2  | 2         | 4           | 32             | 6000 / 93 / 46                                           | 8 / 8 x 500                         | 2 / 1500                     |
| Standard_F4  | 4         | 8           | 64             | 12.000 / 187 / 93                                         | 16 / 16 x 500                         | 4 / 3000                     |
| Standard_F8  | 8         | 16          | 128            | 24.000 / 375 / 187                                        | 32 / 32 x 500                       | 8 / 6000                     |
| Standard_F16 | 16        | 32          | 256            | 48.000 / 750 / 375                                        | 64 / 64 x 500                       | 8 / 12000           |

## <a name="fs-series-sup1sup"></a>Fs-series <sup>1</sup>

De Fs-serie biedt alle voordelen van de F-serie, plus Premium-opslag.

ACU: 210 - 250

Premium-opslag:  Ondersteund

Premium Storage Caching:  Ondersteund

| Grootte | vCPU | Geheugen: GiB | Tijdelijke opslag (SSD) GiB | Max. aantal gegevensschijven | Maximale doorvoer voor opslag in cache en tijdelijke opslag: IOPS / MBps (cachegrootte in GiB) | Maximale doorvoer voor schijf zonder caching: IOPS / MBps | Max. aantal NIC's / verwachte netwerkbandbreedte (Mbps) |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_F1s |1 |2 |4 |4 |4000 / 32 (12) |3200 / 48 |2 / 750 |
| Standard_F2s |2 |4 |8 |8 |8000 / 64 (24) |6400 / 96 |2 / 1500 |
| Standard_F4s |4 |8 |16 |16 |16.000 / 128 (48) |12.800 / 192 |4 / 3000 |
| Standard_F8s |8 |16 |32 |32 |32.000 / 256 (96) |25.600 / 384 |8 / 6000 |
| Standard_F16s |16 |32 |64 |64 |64.000 / 512 (192) |51.200 / 768 |8 / 12000 |

MBps = 10^6 bytes per seconde en GiB = 1024^3 bytes.

<sup>1</sup> de maximale schijfdoorvoer (IOPS of MBps) die mogelijk is met een virtuele machine uit de Fs-serie kan worden beperkt door het aantal, grootte en de striping van de gekoppelde schijven.  Zie voor meer informatie, [ontwerpen voor hoge prestaties](../articles/virtual-machines/windows/premium-storage-performance.md).  

## <a name="ls-series"></a>Ls-serie

De Ls-serie biedt maximaal 32 vCPU's en maakt gebruik van een [Intel Xeon®-processor uit de E5 v3-familie](http://www.intel.com/content/www/us/en/processors/xeon/xeon-e5-solutions.html). De Ls-serie heeft dezelfde CPU-prestaties als de G/GS-serie en wordt geleverd met 8 GiB geheugen per vCPU.

De Ls-serie biedt geen ondersteuning voor het maken van een lokale cache voor het verhogen van de IOPS haalbare door duurzame gegevensschijven. De hoge doorvoer en IOPS van de lokale schijf kunt uit de Ls-serie VM's ideaal voor NoSQL-archieven, zoals Apache Cassandra en MongoDB waarmee gegevens worden gerepliceerd op meerdere virtuele machines voor persistentie in het geval van het uitvallen van één virtuele machine.

ACU: 180-240

Premium-opslag:  Ondersteund

Premium Storage Caching:  Niet ondersteund
 
| Grootte          | vCPU | Geheugen (GiB) | Tijdelijke opslag (GiB) | Max. aantal gegevensschijven | Maximale tijdelijke opslagdoorvoer (IOPS / MBps) | Belastingstests maximale schijfdoorvoer (IOPS / MBps) | Max. aantal NIC's / verwachte netwerkbandbreedte (Mbps) | 
|----------------|-----------|-------------|--------------------------|----------------|-------------------------------------------------------------|-------------------------------------------|------------------------------| 
| Standard_L4s   | 4  | 32  | 678   | 16 | 20,000 / 200 | 5000 / 125  | 2 / 4,000  | 
| Standard_L8s   | 8  | 64  | 1388 | 32 | 40,000 / 400 | 10.000 / 250 | 4 / 8,000  | 
| Standard_L16s  | 16 | 128 | 2807 | 64 | 80,000 / 800 | 20.000 / 500 | 8 / 16,000 | 
| Standard_L32s&nbsp;<sup>1</sup> | 32   | 256  | 5630 | 64   | 160,000 / 1,600   | 40.000 / 1,000     | 8 / 20,000 | 

De maximale schijfdoorvoer die mogelijk is met de Ls-serie VM's kan worden beperkt door het aantal, de grootte en de striping van de gekoppelde schijven. Zie voor meer informatie, [ontwerpen voor hoge prestaties](../articles/virtual-machines/windows/premium-storage-performance.md).

<sup>1</sup> exemplaar is geïsoleerd voor hardware toegewezen aan één klant.

### <a name="standard-a0---a4-using-cli-and-powershell"></a>Standard A0 - A4 met CLI en PowerShell

In het klassieke implementatiemodel verschillen sommige namen van VM-grootten enigszins in CLI en in PowerShell:

* Standard_A0 is ExtraSmall
* Standard_A1 is Small
* Standard_A2 is Medium
* Standard_A3 is Large
* Standard_A4 is ExtraLarge
