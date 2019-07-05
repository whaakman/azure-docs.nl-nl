---
title: bestand opnemen
description: bestand opnemen
services: virtual-machines
author: jonbeck7
ms.service: virtual-machines
ms.topic: include
ms.date: 05/16/2019
ms.author: azcspmt;jonbeck;cynthn
ms.custom: include file
ms.openlocfilehash: 9e145bc3a6824100409a0f6215152cdf70ec6777
ms.sourcegitcommit: ac1cfe497341429cf62eb934e87f3b5f3c79948e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/01/2019
ms.locfileid: "67501226"
---
Geoptimaliseerd voor geheugen VM-grootten aanbieding een hoge geheugen-naar-CPU-snelheid voor relationele-databaseservers, middelgrote tot grote caches en analysefuncties in het geheugen die. In dit artikel bevat informatie over het aantal vcpu's, gegevensschijven en NIC's, evenals de doorvoer en netwerkbandbreedte opslag voor elke grootte in deze groepering. 

* De Mv2-serie biedt het hoogste aantal vCPU's (maximaal 208 vcpu's) en het grootste geheugen (maximaal 5.7 TiB) van alle virtuele machines in de cloud. Dit is ideaal voor zeer grote databases of andere toepassingen die zo kunnen profiteren van een groot aantal vCPU’s en grote hoeveelheden geheugen.
 
* De M-serie biedt een aantal hoge vCPU (maximaal 128 vcpu's) en een grote hoeveelheid geheugen (wel 3,8 TiB). Het is ook ideaal voor zeer grote databases of andere toepassingen die van een groot aantal vCPU's en grote hoeveelheden geheugen profiteren.

* Dv2-serie, G-serie en de tegenhangers DSv2/GS zijn ideaal voor toepassingen die snellere CPU's, betere prestaties voor tijdelijke opslag, of hogere geheugen-eisen. Ze bieden een krachtige combinatie voor vele toepassingen op bedrijfsniveau.

* De Dv2-serie, de opvolger van de oorspronkelijke D-serie, heeft een krachtigere CPU. De CPU van de Dv2-serie is ongeveer 35% sneller dan de CPU van de D-serie. Deze is gebaseerd op de nieuwste generatie 2,4 GHz Intel Xeon® E5-2673 v3 2,4 GHz (Haswell)- of E5-2673 v4 2,3 GHz (Broadwell)-processors, en met Intel Turbo Boost Technology 2.0 kunnen liefst 3,1 GHz bereiken. De Dv2-serie heeft dezelfde geheugen- en schijfconfiguraties als de D-serie.

* De Ev3-serie functies het E5-2673 v4 2,3 GHz (Broadwell)-processor in een hyper-threaded configuratie, bieden een betere toegevoegde waarde voor de meest algemene doeleinden-workloads, en hoe u de Ev3 in overeenstemming met de algemeen gebruik virtuele machines van de meeste andere clouds brengt.  Geheugen is (vanaf 7 GiB/vCPU op 8 GiB/vCPU) uitgevouwen terwijl de schijf en netwerk limieten zijn aangepast op basis van per core om uit te lijnen met de overgang naar hyperthreading is.  De Ev3 is het volgen tot de hoge geheugen-VM-grootten van de D/Dv2-families.

* Azure Compute biedt formaten virtuele machines die zijn geïsoleerd voor een specifiek hardwaretype en bedoeld voor een enkele klant.  Deze formaten virtuele machines zijn ideaal voor workloads waarvoor een hoge mate van isolatie van andere klanten vereist is en voor workloads waarbij elementen als naleving en wettelijke vereisten een rol spelen.  Klanten kunnen ook kiezen voor het verder onderverdelen de resources van deze geïsoleerde virtuele machines met behulp van [Azure-ondersteuning voor geneste virtuele machines](https://azure.microsoft.com/blog/nested-virtualization-in-azure/).  Raadpleeg de tabellen van families met virtuele machines hieronder voor de geïsoleerde VM-opties.

## <a name="esv3-series"></a>Esv3-serie 

ACU: 160-190 <sup>1</sup>

Premium-opslag:  Ondersteund

Premium Storage opslaan in cache:  Ondersteund

Exemplaren uit de ESv3-serie zijn gebaseerd op Intel XEON ® E5-2673 v4-processors van 2,3 GHz (Broadwell) en kunnen maar liefst 3,5 GHz bereiken door de Intel Turbo Boost Technology 2.0 en maken gebruik van Premium Storage. Ev3-exemplaren zijn ideaal voor geheugenintensieve bedrijfstoepassingen.


| Size             | vCPU | Geheugen: GiB | Tijdelijke opslag (SSD) GiB | Max. aantal gegevensschijven | Maximale doorvoer voor opslag in cache en tijdelijke opslag: IOPS / MBps (cachegrootte in GiB) | Maximale doorvoer voor schijf zonder caching: IOPS / MBps | Max. aantal NIC's / verwachte netwerkbandbreedte (Mbps) |
|------------------|--------|-------------|----------------|----------------|-----------------------------------------------------------------------|-------------------------------------------|------------------------------------------------|
| Standard_E2s_v3 | 2      | 16          | 32             | 4              | 4000 / 32 (50)                                                       | 3200 / 48                                | 2 / 1000                                   |
| Standard_E4s_v3&nbsp;<sup>2</sup> | 4      | 32          | 64             | 8              | 8000 / 64 (100)                                                      | 6400 / 96                                | 2 / 2000                                   |
| Standard_E8s_v3&nbsp;<sup>2</sup> | 8      | 64          | 128            | 16             | 16000 / 128 (200)                                                    | 12800 / 192                              | 4 / 4000                                       |
| Standard_E16s_v3&nbsp;<sup>2</sup> | 16     | 128         | 256            | 32             | 32000 / 256 (400)                                                    | 25600 / 384                              | 8 / 8000                                       |
| Standard_E20s_v3                   | 20     | 160         | 320            | 32             | 40000 / 320 (400)                                                    | 32000 / 480                              | 8 / 10000                                       |
| Standard_E32s_v3&nbsp;<sup>2</sup> | 32     | 256         | 512            | 32             | 64000 / 512 (800)                                                    | 51200 / 768                              | 8 / 16000                             |
| Standard_E48s_v3&nbsp;<sup>2</sup> | 48     | 384         | 768            | 32             | 96000/768 (1200)                                                   | 76800 / 1152                             | 8 / 24000                             |
| Standard_E64s_v3&nbsp;<sup>2</sup> | 64     | 432         | 864            | 32             | 128000 / 1024 (1600)                                                   | 80000 / 1200                             | 8 / 30000                             |
| Standard_E64is_v3&nbsp;<sup>3</sup> | 64     | 432         | 864            | 32             | 128000 / 1024 (1600)                                                   | 80000 / 1200                             | 8 / 30000                             |


<sup>1</sup> Esv3-serie VM's zijn uitgerust met Intel® Hyper-Threading-technologie.

<sup>2</sup> constrained core grootten beschikbaar.

<sup>3</sup> exemplaar is geïsoleerd voor hardware toegewezen aan één klant.


## <a name="ev3-series"></a>Ev3-serie 

ACU: 160 - 190 <sup>1</sup>

Premium-opslag:  Niet ondersteund

Premium Storage opslaan in cache:  Niet ondersteund

Exemplaren uit de Ev3-serie zijn gebaseerd op Intel XEON ® E5-2673 v4-processors van 2,3 GHz (Broadwell) en kunnen maar liefst 3,5 GHz bereiken door de Intel Turbo Boost Technology 2.0. Ev3-exemplaren zijn ideaal voor geheugenintensieve bedrijfstoepassingen.

Gegevensschijfopslag wordt apart van virtuele machines in rekening gebracht. Als u Premium Storage-schijven wilt gebruiken, gebruik dan de ESv3-grootten. De prijs- en factureringsmeters voor de ESv3-grootten zijn gelijk aan die van de Ev3-serie. 


| Size            | vCPU | Geheugen: GiB | Tijdelijke opslag (SSD) GiB | Max. aantal gegevensschijven | Maximale tijdelijke opslagdoorvoer: IOPS / MBps lezen / MBps schrijven | Max. aantal NIC's/netwerkbandbreedte |
|-----------------|-----------|-------------|----------------|----------------|----------------------------------------------------------|------------------------------|
| Standard_E2_v3  | 2         | 16          | 50             | 4              | 3000/46/23                                               | 2 / 1000                 |
| Standard_E4_v3  | 4         | 32          | 100            | 8              | 6000/93/46                                               | 2 / 2000                 |
| Standard_E8_v3  | 8         | 64          | 200            | 16             | 12000/187/93                                             | 4 / 4000                     |
| Standard_E16_v3 | 16        | 128         | 400            | 32             | 24000/375/187                                            | 8 / 8000                     |
| Standard_E20_v3 | 20        | 160         | 500            | 32             | 30000/469/234                                            | 8 / 10000                     |
| Standard_E32_v3 | 32        | 256         | 800            | 32             | 48000/750/375                                            | 8 / 16000                 |
| Standard_E48_v3 | 48        | 384         | 1200            | 32             | 96000/1000/500                                            | 8 / 24000                 |
| Standard_E64_v3 | 64        | 432         | 1600           | 32             | 96000/1000/500                                           | 8 / 30000           |
| Standard_E64i_v3&nbsp;<sup>2,&nbsp;3</sup> | 64        | 432         | 1600           | 32             | 96000/1000/500                                           | 8 / 30000           |

<sup>1</sup> van de VM uit de Ev3-serie zijn uitgerust met Intel® Hyper-Threading-technologie.

<sup>2</sup> constrained core grootten beschikbaar.

<sup>3</sup> exemplaar is geïsoleerd voor hardware toegewezen aan één klant.


## <a name="mv2-series"></a>Mv2-serie

Premium-opslag: Ondersteund

Premium Storage opslaan in cache: Ondersteund

Write Accelerator: [Ondersteund](https://docs.microsoft.com/azure/virtual-machines/windows/how-to-enable-write-accelerator)

De Mv2-reeks functies hoge doorvoer en lage latentie en rechtstreeks toegewezen lokale NVMe opslag uitgevoerd op een hyper-threaded Intel® Xeon® Platinum 8180 M 2,5 GHz (Skylake)-processor met een alle core base frequentie van 2,5 GHz en een maximale turbo frequentie van 3,8 GHz. Alle Mv2-serie VM-grootten kunnen permanente schijven van zowel standard als premium gebruiken. Mv2-series-instanties zijn geoptimaliseerd voor geheugen VM-grootten ongeëvenaarde rekenkracht ter ondersteuning van grote in-memory-databases en workloads, met een hoge geheugen-naar-CPU-snelheid die ideaal is voor relationele-databaseservers, grote cachegeheugens en in het geheugen Analytics. 

|Size | vCPU | Geheugen: GiB | Tijdelijke opslag (SSD) GiB | Max. aantal gegevensschijven | Maximale doorvoer voor opslag in cache en tijdelijke opslag: IOPS / MBps (cachegrootte in GiB) | Maximale doorvoer voor schijf zonder caching: IOPS / MBps | Max. aantal NIC's / verwachte netwerkbandbreedte (Mbps) |
|-----------------|------|-------------|----------------|----------------|-----------------------------------------------------------------------|-------------------------------------------|------------------------------|
| Standard_M208ms_v2<sup>1, 2</sup> | 208 | 5700 | 4096 | 64 | 80000 / 800 (7040) | 40000 / 1000 | 8 / 16000 |
| Standard_M208s_v2<sup>1, 2</sup> | 208 | 2850 | 4096 | 64 | 80000 / 800 (7040) | 40000 / 1000 | 8 / 16000 |

Mv2-serie VM's zijn uitgerust met Intel® Hyper-Threading-technologie  

<sup>1</sup> deze grote VM's is een van deze ondersteunde gastbesturingssystemen vereist: Windows Server 2016, Windows Server 2019, SLES 12 SP4, SLES 15.

<sup>2</sup> Mv2-serie VM's zijn van de 2e generatie alleen. Als u Linux gebruikt, raadpleegt u de volgende sectie voor informatie over het zoeken en selecteer een SUSE Linux-installatiekopie.

#### <a name="find-a-suse-image"></a>Een installatiekopie SUSE vinden

Selecteer een geschikte SUSE Linux-installatiekopie in Azure portal: 

1. Selecteer in de Azure portal, **een resource maken** 
1. Zoek naar "SUSE SAP" 
1. SLES voor SAP generatie 2-installatiekopieën zijn beschikbaar als een van beide betalen per gebruik, of breng uw eigen abonnement (BYOS). Vouw de categorie van de gewenste installatiekopie in de lijst met zoekresultaten:

    * SUSE Linux Enterprise Server (SLES) voor SAP
    * SUSE Linux Enterprise Server (SLES) voor SAP (BYOS)
    
1. Installatiekopieën van SUSE compatibel is met de Mv2-serie worden voorafgegaan door de naam van de `GEN2:`. De volgende SUSE-installatiekopieën zijn beschikbaar voor Mv2-serie VM's:

    * GEN2: SUSE Linux Enterprise Server (SLES) 12 SP4 voor SAP-toepassingen
    * GEN2: SUSE Linux Enterprise Server (SLES) 15 voor SAP-toepassingen
    * GEN2: SUSE Linux Enterprise Server (SLES) 12 SP4 voor SAP-toepassingen (BYOS)
    * GEN2: SUSE Linux Enterprise Server (SLES) 15 voor SAP-toepassingen (BYOS)

#### <a name="select-a-suse-image-via-azure-cli"></a>Selecteer de installatiekopie van een SUSE via Azure CLI

Voor een overzicht van de momenteel beschikbare SLES voor SAP-installatiekopie voor Mv2-serie VM's, gebruikt u de volgende [ `az vm image list` ](https://docs.microsoft.com/cli/azure/vm/image?view=azure-cli-latest#az-vm-image-list) opdracht:

```azurecli
az vm image list --output table --publisher SUSE --sku gen2 --all
```

De opdracht levert de momenteel beschikbare generatie 2 VM's beschikbaar van SUSE voor Mv2-serie VM's. 

Voorbeelduitvoer:

```
Offer          Publisher  Sku          Urn                                        Version
-------------  ---------  -----------  -----------------------------------------  ----------
SLES-SAP       SUSE       gen2-12-sp4  SUSE:SLES-SAP:gen2-12-sp4:2019.05.13       2019.05.13
SLES-SAP       SUSE       gen2-15      SUSE:SLES-SAP:gen2-15:2019.05.13           2019.05.13
SLES-SAP-BYOS  SUSE       gen2-12-sp4  SUSE:SLES-SAP-BYOS:gen2-12-sp4:2019.05.13  2019.05.13
SLES-SAP-BYOS  SUSE       gen2-15      SUSE:SLES-SAP-BYOS:gen2-15:2019.05.13      2019.05.13
```

## <a name="m-series"></a>M-serie 

ACU: 160-180 <sup>1</sup>

Premium-opslag:  Ondersteund

Premium Storage opslaan in cache:  Ondersteund

Write Accelerator:  [Ondersteund](https://docs.microsoft.com/azure/virtual-machines/windows/how-to-enable-write-accelerator)

| Size            | vCPU | Geheugen: GiB | Tijdelijke opslag (SSD) GiB | Max. aantal gegevensschijven | Maximale doorvoer voor opslag in cache en tijdelijke opslag: IOPS / MBps (cachegrootte in GiB) | Maximale doorvoer voor schijf zonder caching: IOPS / MBps | Max. aantal NIC's / verwachte netwerkbandbreedte (Mbps) |
|-----------------|------|-------------|----------------|----------------|-----------------------------------------------------------------------|-------------------------------------------|------------------------------|
| Standard_M8ms&nbsp;<sup>3</sup>    | 8  | 218.75 | 256  | 8  | 10000 / 100 (793)  | 5000  / 125 | 4 / 2000 |
| Standard_M16ms&nbsp;<sup>3</sup>   | 16 | 437.5  | 512  | 16 | 20000 / 200 (1587) | 10000 / 250 | 8 / 4000 |
| Standard_M32ts | 32 | 192    | 1024 | 32 | 40000 / 400 (3174) | 20000 / 500 | 8 / 8000 |
| Standard_M32ls | 32 | 256    | 1024 | 32 | 40000 / 400 (3174) | 20000 / 500 | 8 / 8000 |
| Standard_M32ms&nbsp;<sup>3</sup>   | 32 | 875    | 1024 | 32 | 40000 / 400 (3174) | 20000 / 500 | 8 / 8000 |
| Standard_M64s  | 64 | 1024   | 2048 | 64 | 80000 / 800 (6348)| 40000 / 1000 | 8 / 16000          |
| Standard_M64ls  | 64 | 512    | 2048 | 64 | 80000 / 800 (6348) | 40000 / 1000 | 8 / 16000 |
| Standard_M64ms&nbsp;<sup>3</sup>  | 64   | 1792 | 2048 | 64 | 80000 / 800 (6348)| 40000 / 1000 | 8 / 16000          |
| Standard_M128s&nbsp;<sup>2</sup> | 128  | 2048        | 4096  | 64 | 160000 / 1600 (12696) | 80000 / 2000                            | 8 / 30000          |
| Standard_M128ms&nbsp;<sup>2,&nbsp;3,&nbsp;4</sup> | 128  | 3892  | 4096 | 64 | 160000 / 1600 (12696) | 80000 / 2000                            | 8 / 30000          |
| Standard_M64   | 64  | 1024 | 7168  | 64 | 80000  / 800  (1228) | 40000 / 1000 | 8 / 16000 |
| Standard_M64m  | 64  | 1792 | 7168  | 64 | 80000  / 800  (1228) | 40000 / 1000 | 8 / 16000 |
| Standard_M128&nbsp;<sup>2  | 128 | 2048 | 14336 | 64 | 250000 / 1600 (2456) | 80000 / 2000 | 8 / 32000 |
| Standard_M128m&nbsp;<sup>2 | 128 | 3892 | 14336 | 64 | 250000 / 1600 (2456) | 80000 / 2000 | 8 / 32000 |



<sup>1</sup> M-serie VM's functie Intel® Hyper-Threading-technologie

<sup>2</sup> meer dan 64 vCPU's is een van deze ondersteunde gastbesturingssystemen vereist: WindowsServer 2016, Ubuntu 16.04 TNS, SLES 12 SP2 en Red Hat Enterprise Linux, CentOS 7.3 of Oracle Linux 7.3 met LIS 4.2.1.

<sup>3</sup> constrained core grootten beschikbaar.

<sup>4</sup> exemplaar is geïsoleerd voor hardware toegewezen aan één klant.
<br>


## <a name="dsv2-series-11-15"></a>11-15, DSv2-serie

ACU: 210 - 250 <sup>1</sup>

Premium-opslag:  Ondersteund

Premium Storage opslaan in cache:  Ondersteund

| Size | vCPU | Geheugen: GiB | Tijdelijke opslag (SSD) GiB | Max. aantal gegevensschijven | Maximale doorvoer voor opslag in cache en tijdelijke opslag: IOPS / MBps (cachegrootte in GiB) | Maximale doorvoer voor schijf zonder caching: IOPS / MBps | Max. aantal NIC's / verwachte netwerkbandbreedte (Mbps) |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_DS11_v2&nbsp;<sup>3</sup> |2 |14 |28 |8 |8000 / 64 (72) |6400 / 96 |2 / 1500 |
| Standard_DS12_v2&nbsp;<sup>3</sup> |4 |28 |56 |16 |16000 / 128 (144) |12800 / 192 |4 / 3000 |
| Standard_DS13_v2&nbsp;<sup>3</sup> |8 |56 |112 |32 |32000 / 256 (288) |25600 / 384 |8 / 6000 |
| Standard_DS14_v2&nbsp;<sup>3</sup>|16 |112 |224 |64 |64000 / 512 (576) |51200 / 768 |8 / 12000 |
| Standard_DS15_v2&nbsp;<sup>2</sup> |20 |140 |280 |64 |80000 / 640 (720) |64000 / 960 |8 / 25000&nbsp;<sup>4</sup>

<sup>1</sup> de maximale schijfdoorvoer (IOPS of MBps) die mogelijk is met een virtuele machine uit de DSv2-serie kan worden beperkt door het aantal, grootte en de striping van de gekoppelde schijven.  Zie voor meer informatie, [ontwerpen voor hoge prestaties](../articles/virtual-machines/windows/premium-storage-performance.md).  
<sup>2</sup> exemplaar is geïsoleerd voor hardware toegewezen aan één klant.  
<sup>3</sup> constrained core grootten beschikbaar.  
<sup>4</sup> 25000 Mbps met versneld netwerken. 

<br>

## <a name="dv2-series-11-15"></a>Dv2-series 11-15

ACU: 210 - 250

Premium-opslag:  Niet ondersteund

Premium Storage opslaan in cache:  Niet ondersteund

| Size              | vCPU | Geheugen: GiB | Tijdelijke opslag (SSD) GiB | Maximale tijdelijke opslagdoorvoer: IOPS / MBps lezen / MBps schrijven | Max. aantal gegevensschijven / doorvoer: IOPS | Max. aantal NIC's / verwachte netwerkbandbreedte (Mbps) |
|-------------------|-----------|-------------|----------------|----------------------------------------------------------|-----------------------------------|------------------------------|
| Standard_D11_v2   | 2         | 14          | 100            | 6000 / 93 / 46                                           | 8 / 8 x 500                         | 2 / 1500                     |
| Standard_D12_v2   | 4         | 28          | 200            | 12.000 / 187 / 93                                         | 16 / 16 x 500                         | 4 / 3000                     |
| Standard_D13_v2   | 8         | 56          | 400            | 24.000 / 375 / 187                                        | 32 / 32 x 500                       | 8 / 6000                     |
| Standard_D14_v2   | 16        | 112         | 800            | 48.000 / 750 / 375                                        | 64 / 64 x 500                       | 8 / 12000          |
| Standard_D15_v2&nbsp;<sup>1</sup> | 20        | 140         | 1000          | 60.000 / 937 / 468                                        | 64 / 64 x 500                       | 8 / 25000&nbsp;<sup>2</sup> |

<sup>1</sup> exemplaar is geïsoleerd voor hardware toegewezen aan één klant.  
<sup>2</sup> 25000 Mbps met versneld netwerken. 
