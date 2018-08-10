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
ms.openlocfilehash: 1606b5e38b79cb535108ca747783a05de1fbc982
ms.sourcegitcommit: d0ea925701e72755d0b62a903d4334a3980f2149
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/09/2018
ms.locfileid: "40026420"
---
VM-grootten voor algemeen gebruik bieden evenwichtige CPU-geheugenverhouding. Ideaal voor testen en ontwikkelen, kleine tot middelgrote databases en webservers met weinig of gemiddeld verkeer. In dit artikel bevat informatie over het aantal vcpu's, gegevensschijven en NIC's, evenals opslagdoorvoer voor grootten die in deze groepering. 

- De Av2-serie VM's kunnen worden geïmplementeerd op diverse hardwaretypen en processors. De grootte is afhankelijk van de hardware, zodat er consistente processorprestaties voor het actieve exemplaar kunnen worden geboden, ongeacht de hardware waarop deze is geïmplementeerd. Om de fysieke hardware te bepalen waarop deze grootte is geïmplementeerd, vraagt u vanuit de virtuele machine gegevens over de virtuele hardware op.

- De Dv2-serie, de opvolger van de oorspronkelijke D-serie, heeft een krachtigere CPU. De CPU van de Dv2-serie is ongeveer 35% sneller dan de CPU van de D-serie. Deze is gebaseerd op de nieuwste generatie Intel Xeon® E5-2673 v3 2,4 GHz (Haswell)- of E5-2673 v4 2,3 GHz (Broadwell)-processors, en met Intel Turbo Boost Technology 2.0 kunnen liefst 3,1 GHz bereiken. De Dv2-serie heeft dezelfde geheugen- en schijfconfiguraties als de D-serie.

- De Dv3-serie biedt de 2,4 GHz Intel Xeon® E5-2673 v3 (Haswell)-processor of de meest recente 2,3 GHz Intel XEON® E5-2673 v4 (Broadwell)-processor in een configuratie met hyper-threaded, bieden een betere toegevoegde waarde voor de meest algemene doeleinden-werkbelastingen.  Geheugen is (van ~3.5 GiB/vCPU naar 4 GiB/vCPU) uitgevouwen terwijl de schijf en netwerk limieten zijn aangepast op basis van per core om uit te lijnen met de overgang naar hyperthreading is.  De Dv3 heeft niet langer de hoge geheugen-VM-grootten van de D/Dv2-families, die zijn verplaatst naar de nieuwe Ev3-serie.


## <a name="b-series"></a>B-serie

Premium Storage: ondersteund

Premium Storage Caching: Niet ondersteund

De VM's met burstfunctie B-serie zijn ideaal voor workloads die niet continu de volledige prestaties van de CPU nodig hebt, zoals webservers, kleine databases en de ontwikkeling en testomgevingen. Deze werkbelastingen hebben meestal ' burstable ' prestatie-eisen. De B-serie biedt deze klanten de mogelijkheid om aan te schaffen van een VM-grootte met een basislijn waarmee het VM-exemplaar om op te bouwen tegoed bij minder dan de algemene prestatiegegevens gebruik van de virtuele machine zich bewust van prijs. Wanneer de virtuele machine zijn tegoed verzameld, kan de virtuele machine burst boven van de VM-basislijn met tot 100% van de CPU wanneer uw toepassing hogere CPU-prestaties vereist.


| Grootte             | vCPU  | Geheugen: GiB | Lokale SSD: GiB | Basis voor prestaties van een kern | Tegoed gestort / uur | Max gestort tegoed | Max. aantal gegevensschijven | Lokale schijf voor maximale prestaties: IOPS / MBps | Max zonder caching voor prestaties schijf: IOPS / MBps | Max. aantal NIC's |          
|---------------|-------------|----------------|----------------------------|-----------------------|--------------------|----------------|----------------------------------------|-------------------------------------------|-------------------------------------------|----------|
| Standard_B1s  | 1           | 1              | 4                          | 10%                   | 6                  | 144            | 2                                      | 400 / 10                                  | 320 / 10                                  | 2  |
| Standard_B1ms | 1           | 2              | 4                          | 20%                   | 12                 | 288            | 2                                      | 800 / 10                                  | 640 / 10                                  | 2  |
| Standard_B2s  | 2           | 4              | 8                          | 40%                   | 24                 | 576            | 4                                      | 1600 / 15                                 | 1280 / 15                                 | 3  |
| Standard_B2ms | 2           | 8              | 16                         | 60%                   | 36                 | 864            | 4                                      | 2400 / 22.5                               | 1920 / 22.5                               | 3  |
| Standard_B4ms | 4           | 16             | 32                         | 90%                   | 54                 | 1296           | 8                                      | 3600 / 35                                 | 2880 / 35                                 | 4  |
| Standard_B8ms | 8           | 32             | 64                         | 135%                  | 81                 | 1944           | 16                                     | 4320 / 50                                 | 4320 / 50                                 | 4  |


## <a name="dsv3-series-sup1sup"></a>Dsv3-serie <sup>1</sup>

ACU: 160-190

Premium Storage: ondersteund

Caching van Premium Storage: ondersteund

Dsv3-serie zijn gebaseerd op de 2,4 GHz Intel Xeon® E5-2673 v3 (Haswell)-processor of de meest recente 2,3 GHz Intel XEON® E5-2673 v4 (Broadwell)-processor die kan maar liefst 3,5 GHz bereiken door Intel Turbo Boost Technology 2.0 en gebruiken premiumopslag. De Dsv3-serie biedt een combinatie van vCPU, geheugen en tijdelijke opslag voor de meeste productieworkloads.


| Grootte             | vCPU | Geheugen: GiB | Tijdelijke opslag (SSD) GiB | Max. aantal gegevensschijven | Maximale doorvoer voor schijven met caching en tijdelijke opslag: IOPS / MBps (cachegrootte in GiB) | Max. doorvoer voor schijf zonder caching: IOPS/MBps | Max. aantal NIC's / verwachte netwerkbandbreedte (Mbps) |
|------------------|--------|-------------|----------------|----------------|-----------------------------------------------------------------------|-------------------------------------------|------------------------------------------------|
| Standard_D2s_v3  | 2      | 8           | 16             | 4              | 4,000 / 32 (50)                                                       | 3200 / 48                                | 2 / 1,000                                   |
| Standard_D4s_v3  | 4      | 16          | 32             | 8              | 8,000 / 64 (100)                                                      | 6400 / 96                                | 2 / 2,000                                   |
| Standard_D8s_v3  | 8      | 32          | 64             | 16             | 16,000 / 128 (200)                                                    | 12.800 / 192                              | 4 / 4,000                                      |
| Standard_D16s_v3 | 16     | 64          | 128            | 32             | 32,000 / 256 (400)                                                    | 25.600 / 384                              | 8 / 8,000                                      |
| Standard_D32s_v3 | 32     | 128          | 256            | 32             | 64,000 / 512 (800)                                                    | 51.200 / 768                              | 8 / 16,000                                               |
| Standard_D64s_v3 | 64     | 256          | 512            | 32             | 128,000 / 1024 (1600)                                                    | 80,000 / 1200                              | 8 / 30,000                                               |

<sup>1</sup> Dsv3-serie VM's zijn uitgerust met Intel® Hyper-Threading-technologie

## <a name="dv3-series-sup1sup"></a>Dv3-serie <sup>1</sup>

ACU: 160-190

Premium-opslag: Niet ondersteund

Premium Storage Caching: Niet ondersteund

Dv3-serie zijn gebaseerd op de 2,4 GHz Intel Xeon® E5-2673 v3 (Haswell)-processor of 2,3 GHz Intel XEON® E5-2673 v4 (Broadwell)-processor die maar 3,5 GHz bereiken door Intel Turbo Boost Technology 2.0 liefst kan. De Dv3-serie biedt een combinatie van vCPU, geheugen en tijdelijke opslag voor de meeste productieworkloads.

Gegevensschijfopslag wordt apart van virtuele machines in rekening gebracht. Als u Premium Storage-schijven wilt gebruiken, gebruik dan de Dsv3-grootten. De prijs- en factureringsmeters voor de Dsv3-grootten zijn gelijk aan die van de Dv3-serie. 


| Grootte            | vCPU | Geheugen: GiB | Tijdelijke opslag (SSD) GiB | Max. aantal gegevensschijven | Maximale tijdelijke opslagdoorvoer: IOPS / MBps lezen / MBps schrijven | Max. aantal NIC's/netwerkbandbreedte |
|-----------------|-----------|-------------|----------------|----------------|----------------------------------------------------------|------------------------------|
| Standard_D2_v3  | 2         | 8           | 50             | 4              | 3000/46/23                                               | 2 / 1,000                    |
| Standard_D4_v3  | 4         | 16          | 100            | 8              | 6000/93/46                                               | 2 / 2,000                    |
| Standard_D8_v3  | 8         | 32          | 200            | 16             | 12000/187/93                                             | 4 / 4,000                    |
| Standard_D16_v3 | 16        | 64          | 400            | 32             | 24000/375/187                                            | 8 / 8,000                    |
| Standard_D32_v3 | 32        | 128          | 800            | 32             | 48000/750/375                                            | 8 / 16,000                             |
| Standard_D64_v3 | 64        | 256          | 1600            | 32             | 96000/1000/500                                            | 8 / 30,000                             |

<sup>1</sup> Dv3-serie VM's zijn uitgerust met Intel® Hyper-Threading-technologie


## <a name="dsv2-series"></a>DSv2-serie

ACU: 210-250

Premium Storage: ondersteund

Caching van Premium Storage: ondersteund

| Grootte | vCPU | Geheugen: GiB | Tijdelijke opslag (SSD) GiB | Max. aantal gegevensschijven | Maximale doorvoer voor schijven met caching en tijdelijke opslag: IOPS / MBps (cachegrootte in GiB) | Max. doorvoer voor schijf zonder caching: IOPS/MBps | Max. aantal NIC's / verwachte netwerkbandbreedte (Mbps) |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_DS1_v2 |1 |3,5 |7 |4 |4000 / 32 (43) |3200 / 48 |2 / 750 |
| Standard_DS2_v2 |2 |7 |14 |8 |8000 / 64 (86) |6400 / 96 |2 / 1500 |
| Standard_DS3_v2 |4 |14 |28 |16 |16.000 / 128 (172) |12.800 / 192 |4 / 3000 |
| Standard_DS4_v2 |8 |28 |56 |32 |32.000 / 256 (344) |25.600 / 384 |8 / 6000 |
| Standard_DS5_v2 |16 |56 |112 |64 |64.000 / 512 (688) |51.200 / 768 |8 / 12000 |



## <a name="dv2-series"></a>Dv2-serie

ACU: 210-250

Premium-opslag: Niet ondersteund

Premium Storage Caching: Niet ondersteund

| Grootte           | vCPU | Geheugen: GiB | Tijdelijke opslag (SSD) GiB | Maximale tijdelijke opslagdoorvoer: IOPS / MBps lezen / MBps schrijven | Max. aantal gegevensschijven | Doorvoer: IOPS | Max. aantal NIC's / verwachte netwerkbandbreedte (Mbps) |
|----------------|------|-------------|------------------------|------------------------------------------------------------|----------------|------------------|----------------------------------------------|
| Standard_D1_v2 | 1    | 3,5         | 50                     | 3000 / 46 / 23                                             | 4              | 4 x 500            | 2 / 750                                      |
| Standard_D2_v2 | 2    | 7           | 100                    | 6000 / 93 / 46                                             | 8              | 8 x 500            | 2 / 1500                                     |
| Standard_D3_v2 | 4    | 14          | 200                    | 12.000 / 187 / 93                                           | 16             | 16 x 500           | 4 / 3000                                       |
| Standard_D4_v2 | 8    | 28          | 400                    | 24.000 / 375 / 187                                          | 32             | 32 x 500           | 8 / 6000                                       |
| Standard_D5_v2 | 16   | 56          | 800                    | 48.000 / 750 / 375                                          | 64             | 64 x 500           | 8 / 12000                                    |


## <a name="av2-series"></a>Av2-serie

ACU: 100

Premium-opslag: Niet ondersteund

Premium Storage Caching: Niet ondersteund

| Grootte            | vCPU | Geheugen: GiB | Tijdelijke opslag (SSD) GiB | Maximale tijdelijke opslagdoorvoer: IOPS / MBps lezen / MBps schrijven | Maximumaantal gegevensschijven / doorvoer: IOPS | Max. aantal NIC's / verwachte netwerkbandbreedte (Mbps) | 
|-----------------|-----------|-------------|----------------|----------------------------------------------------------|-----------------------------------|------------------------------|
| Standard_A1_v2  | 1         | 2           | 10             | 1000 / 20 / 10                                           | 2 / 2 x 500               | 2 / 250                 |
| Standard_A2_v2  | 2         | 4           | 20             | 2000 / 40 / 20                                           | 4 / 4 x 500               | 2 / 500                 |
| Standard_A4_v2  | 4         | 8           | 40             | 4000 / 80 / 40                                           | 8 / 8 x 500               | 4 / 1000                     |
| Standard_A8_v2  | 8         | 16          | 80             | 8000 / 160 / 80                                          | 16 / 16 x 500             | 8 / 2000                     |
| Standard_A2m_v2 | 2         | 16          | 20             | 2000 / 40 / 20                                           | 4 / 4 x 500               | 2 / 500                 |
| Standard_A4m_v2 | 4         | 32          | 40             | 4000 / 80 / 40                                           | 8 / 8 x 500               | 4 / 1000                     |
| Standard_A8m_v2 | 8         | 64          | 80             | 8000 / 160 / 80                                          | 16 / 16 x 500             | 8 / 2000                     |

<br>





