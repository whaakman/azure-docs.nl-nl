---
title: bestand opnemen
description: bestand opnemen
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 01/11/2019
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 04b6ad25c1ecd10a9480dcbf3e2b4f75e114a6f9
ms.sourcegitcommit: d2329d88f5ecabbe3e6da8a820faba9b26cb8a02
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/16/2019
ms.locfileid: "56331202"
---
*Opwarmen van de Cache*  
De schijf met alleen-lezentoegang opslaan in cache zijn kunnen bieden hogere IOP's dan de limiet van de schijf. Als u dit maximum leesprestaties uit de cache van de host, moet eerst u opgewarmd de cache van deze schijf. Dit zorgt ervoor dat de lezen IOs dat het hulpprogramma voor benchmarking wordt station op CacheReads volume, daadwerkelijk treffers in de cache en niet op de schijf rechtstreeks. Het resultaat van de cache treffers in extra IOP's uit de cache van één schijf ingeschakeld.

> [!IMPORTANT]
> U moet de cache opgewarmd voordat benchmarking, wordt uitgevoerd telkens wanneer de virtuele machine opnieuw is opgestart.

## <a name="tools"></a>Hulpprogramma's

### <a name="iometer"></a>Iometer

[Download het hulpprogramma Iometer](http://sourceforge.net/projects/iometer/files/iometer-stable/2006-07-27/iometer-2006.07.27.win32.i386-setup.exe/download) op de virtuele machine.

#### <a name="test-file"></a>Testbestand

Iometer maakt gebruik van een testbestand dat is opgeslagen op het volume waarop u de benchmarking test uitvoert. Deze stations leest en schrijft voor dit testbestand voor het meten van de schijf IOPS en doorvoer. Iometer wordt deze testbestand gemaakt als u deze niet hebt opgegeven. Maak een bestand voor het testen van 200 GB iobw.tst aangeroepen op de volumes CacheReads en NoCacheWrites.

#### <a name="access-specifications"></a>Access-specificaties

De specificaties aanvragen i/o-grootte % lezen/schrijven, willekeurige/opeenvolgende % zijn geconfigureerd op het tabblad 'Specificaties Access' in Iometer. Maak een specificatie van de toegang voor elk van de scenario's die hieronder worden beschreven. De toegangsspecificaties maken en 'Opslaan' met een geschikte naam zoals – RandomWrites\_8 kB, RandomReads\_8 kB. Selecteer de bijbehorende specificatie bij het uitvoeren van de Testscenario.

Hieronder, ziet u een voorbeeld van toegang tot de specificaties voor maximale schrijven IOP's-scenario  
    ![Voorbeeld van toegang tot de specificaties voor de maximale IOPS voor schrijven](../articles/virtual-machines/linux/media/premium-storage-performance/image8.png)

#### <a name="maximum-iops-test-specifications"></a>Maximale IOPS testen specificaties

Gebruiken om te demonstreren maximale IOPs, kleinere aanvraaggrootte. Aanvraaggrootte van 8 kB gebruiken en maken van de specificaties voor willekeurige schrijfbewerkingen en leesbewerkingen.

| Access-specificatie | Aanvraaggrootte | Willekeurige % | De % lezen |
| --- | --- | --- | --- |
| RandomWrites\_8 kB |8 KB |100 |0 |
| RandomReads\_8 kB |8 KB |100 |100 |

#### <a name="maximum-throughput-test-specifications"></a>Maximale doorvoer test specificaties

Gebruiken om te demonstreren maximale doorvoer, grotere grootte van de aanvraag. Aanvraag voor de grootte van 64 kB gebruiken en maken van de specificaties voor willekeurige schrijfbewerkingen en leesbewerkingen.

| Access-specificatie | Aanvraaggrootte | Willekeurige % | De % lezen |
| --- | --- | --- | --- |
| RandomWrites\_64 kB |64 KB |100 |0 |
| RandomReads\_64 kB |64 KB |100 |100 |

#### <a name="run-the-iometer-test"></a>Voer de test Iometer

Voer de stappen hieronder om te oefenen cache uit

1. Twee toegangsspecificaties maken met waarden die hieronder worden weergegeven

   | Name | Aanvraaggrootte | Willekeurige % | De % lezen |
   | --- | --- | --- | --- |
   | RandomWrites\_1 MB |1 MB |100 |0 |
   | RandomReads\_1 MB |1 MB |100 |100 |
1. Voer de test Iometer voor het initialiseren van cacheschijf met de volgende parameters. Drie werkthreads gebruiken voor het doelvolume en een wachtrijdiepte van 128. De duur 'Uitvoeringstijd' van de test ingesteld op 2 uur op het tabblad 'Testen instellen'.

   | Scenario | Doelvolume | Name | Duur |
   | --- | --- | --- | --- |
   | Cacheschijf initialiseren |CacheReads |RandomWrites\_1 MB |2 uur |
1. Voer de test Iometer voor het opwarmen van cacheschijf met de volgende parameters. Drie werkthreads gebruiken voor het doelvolume en een wachtrijdiepte van 128. De duur 'Uitvoeringstijd' van de test ingesteld op 2 uur op het tabblad 'Testen instellen'.

   | Scenario | Doelvolume | Name | Duur |
   | --- | --- | --- | --- |
   | Warme van Cache-schijf |CacheReads |RandomReads\_1 MB |2 uur |

Nadat de cacheschijf is opgewarmd, gaat u verder met de test-scenario's die hieronder worden vermeld. Als u wilt de Iometer test uitvoert, gebruikt u ten minste drie werkthreads voor **elke** volume als doel. Voor elke werkthread, selecteert u het doelvolume, wachtrijdiepte ingesteld en selecteert u een van de specificaties opgeslagen test, zoals wordt weergegeven in de onderstaande tabel, om uit te voeren van het bijbehorende Testscenario. De tabel toont ook verwachte resultaten voor IOPS en doorvoer bij het uitvoeren van deze tests. Voor alle scenario's, wordt een kleine i/o-grootte van 8 KB en een hoge wachtrijdiepte van 128 gebruikt.

| Testscenario | Doelvolume | Name | Resultaat |
| --- | --- | --- | --- |
| Met maximaal Gelezen IOP 's |CacheReads |RandomWrites\_8 kB |MAAR LIEFST 50.000 IOPS |
| Met maximaal IOPS-schrijfbewerkingen |NoCacheWrites |RandomReads\_8 kB |MAAR LIEFST 64.000 IOPS |
| Met maximaal Gecombineerde IOPS |CacheReads |RandomWrites\_8 kB |100.000 IOPS |
| NoCacheWrites |RandomReads\_8 kB | &nbsp; | &nbsp; |
| Met maximaal Read MB/sec |CacheReads |RandomWrites\_64 kB |524 MB/sec |
| Met maximaal MB per seconde schrijven |NoCacheWrites |RandomReads\_64 kB |524 MB/sec |
| Gecombineerde MB per seconde |CacheReads |RandomWrites\_64 kB |1000 MB per seconde |
| NoCacheWrites |RandomReads\_64 kB | &nbsp; | &nbsp; |

Hieronder ziet u schermafbeeldingen van de Iometer testresultaten voor gecombineerde scenario's voor IOPS en doorvoer.

#### <a name="combined-reads-and-writes-maximum-iops"></a>Gecombineerd leest en schrijft de maximale IOPS

![Gecombineerde lees- en schrijfbewerkingen maximale IOPS](../articles/virtual-machines/linux/media/premium-storage-performance/image9.png)

#### <a name="combined-reads-and-writes-maximum-throughput"></a>Gecombineerd leest en schrijft de maximale doorvoer

![Gecombineerde lees- en schrijfbewerkingen maximale doorvoer](../articles/virtual-machines/linux/media/premium-storage-performance/image10.png)

### <a name="fio"></a>FIO

FIO is een populair hulpprogramma naar een benchmark-opslag op de virtuele Linux-machines. Deze heeft de flexibiliteit om te selecteren van verschillende i/o-grootte, sequentiële of willekeurige leest en schrijft. Deze gestart werkthreads of processen de opgegeven i/o-bewerkingen uit te voeren. U kunt het type van de i/o-bewerkingen die elke werkthread moet uitvoeren met behulp van de taakbestanden opgeven. We één taakbestand per scenario is geïllustreerd in de onderstaande voorbeelden hebt gemaakt. U kunt de specificaties van deze taakbestanden benchmark-test uitvoeren verschillende werkbelastingen die worden uitgevoerd op Premium Storage kunt wijzigen. In de voorbeelden gebruiken we een Standard DS-14 VM die wordt uitgevoerd **Ubuntu**. Gebruik dezelfde instellingen die worden beschreven in het begin van de [Benchmarking sectie](#Benchmarking) en warme van de cache voordat u de benchmarking tests uitvoert.

Voordat u begint, [FIO downloaden](https://github.com/axboe/fio) en installeer deze op uw virtuele machine.

Voer de volgende opdracht uit voor Ubuntu,

```
apt-get install fio
```

We gebruiken de vier werkthreads voor het aansturen van schrijfbewerkingen en vier werkthreads voor sparen leesbewerkingen op de schijven. De werknemers schrijven verantwoordelijk zijn voor verkeer op het volume "nocache", die 10-schijven met cache ingesteld op 'None'. De werknemers lezen verantwoordelijk zijn voor verkeer op het volume "readcache", die beschikt over een schijf met de cache is ingesteld op 'Alleen-lezen'.

#### <a name="maximum-write-iops"></a>Maximale IOPS voor schrijven

De taakbestand maken met de volgende specificaties om op te halen van de maximale IOPS voor schrijven. Geef het de naam 'fiowrite.ini'.

```ini
[global]
size=30g
direct=1
iodepth=256
ioengine=libaio
bs=8k

[writer1]
rw=randwrite
directory=/mnt/nocache
[writer2]
rw=randwrite
directory=/mnt/nocache
[writer3]
rw=randwrite
directory=/mnt/nocache
[writer4]
rw=randwrite
directory=/mnt/nocache
```

Houd er rekening mee de volgen belangrijke dingen die in overeenstemming met de richtlijnen voor het ontwerpen in de vorige secties besproken. Deze specificaties zijn essentieel om het maximum aantal IOP's, te stimuleren  

* Een hoge wachtrijdiepte van 256.  
* Een kleine blokgrootte van 8 KB.  
* Meerdere threads uitvoeren van willekeurige schrijfbewerkingen.

De volgende opdracht om te worden gehouden, trappen af de FIO test gedurende 30 seconden  

```
sudo fio --runtime 30 fiowrite.ini
```

Tijdens de test wordt uitgevoerd, bent u kan het aantal schrijven IOP's van de virtuele machine en het leveren van Premium-schijven. Zoals u in het onderstaande voorbeeld, levert de DS14-virtuele machine de maximale IOPS-limiet van maar liefst 50.000 IOPS-schrijven.  
    ![Aantal IOPS VM- en Premium-schijven leveren](../articles/virtual-machines/linux/media/premium-storage-performance/image11.png)

#### <a name="maximum-read-iops"></a>Maximum aantal IOP's lezen

De taakbestand maken met de volgende specificaties om op te halen van de maximale IOPS voor lezen. Geef het de naam 'fioread.ini'.

```ini
[global]
size=30g
direct=1
iodepth=256
ioengine=libaio
bs=8k

[reader1]
rw=randread
directory=/mnt/readcache
[reader2]
rw=randread
directory=/mnt/readcache
[reader3]
rw=randread
directory=/mnt/readcache
[reader4]
rw=randread
directory=/mnt/readcache
```

Houd er rekening mee de volgen belangrijke dingen die in overeenstemming met de richtlijnen voor het ontwerpen in de vorige secties besproken. Deze specificaties zijn essentieel om het maximum aantal IOP's, te stimuleren

* Een hoge wachtrijdiepte van 256.  
* Een kleine blokgrootte van 8 KB.  
* Meerdere threads uitvoeren van willekeurige schrijfbewerkingen.

De volgende opdracht om te worden gehouden, trappen af de FIO test gedurende 30 seconden

```
sudo fio --runtime 30 fioread.ini
```

Tijdens de test wordt uitgevoerd, bent u kan het aantal gelezen IOP's van de virtuele machine en het leveren van Premium-schijven. Zoals weergegeven in het voorbeeld hieronder ziet, is de DS14-virtuele machine het leveren van meer dan 64.000 IOPS voor lezen. Dit is een combinatie van de schijf en de prestaties van de cache.  
    ![](../articles/virtual-machines/linux/media/premium-storage-performance/image12.png)

#### <a name="maximum-read-and-write-iops"></a>Maximale lezen en schrijven IOP 's

De taakbestand maken met de volgende specificaties om op te halen maximale gecombineerde lezen en schrijven IOP's. Geef het de naam 'fioreadwrite.ini'.

```ini
[global]
size=30g
direct=1
iodepth=128
ioengine=libaio
bs=4k

[reader1]
rw=randread
directory=/mnt/readcache
[reader2]
rw=randread
directory=/mnt/readcache
[reader3]
rw=randread
directory=/mnt/readcache
[reader4]
rw=randread
directory=/mnt/readcache

[writer1]
rw=randwrite
directory=/mnt/nocache
rate_iops=12500
[writer2]
rw=randwrite
directory=/mnt/nocache
rate_iops=12500
[writer3]
rw=randwrite
directory=/mnt/nocache
rate_iops=12500
[writer4]
rw=randwrite
directory=/mnt/nocache
rate_iops=12500
```

Houd er rekening mee de volgen belangrijke dingen die in overeenstemming met de richtlijnen voor het ontwerpen in de vorige secties besproken. Deze specificaties zijn essentieel om het maximum aantal IOP's, te stimuleren

* Een hoge wachtrijdiepte van 128.  
* Een kleine blokgrootte van 4 KB.  
* Meerdere threads uitvoeren van willekeurige leest en schrijft.

De volgende opdracht om te worden gehouden, trappen af de FIO test gedurende 30 seconden

```
sudo fio --runtime 30 fioreadwrite.ini
```

Tijdens de test wordt uitgevoerd, u kunt zien hoeveel gecombineerde lezen en schrijven IOP's van de virtuele machine en leveren van Premium-schijven. Zoals wordt weergegeven in het onderstaande voorbeeld, de DS14-virtuele machine is het leveren van meer dan 100.000 gecombineerde lezen en schrijven IOP's. Dit is een combinatie van de schijf en de prestaties van de cache.  
    ![Gecombineerde lezen en schrijven IOP 's](../articles/virtual-machines/linux/media/premium-storage-performance/image13.png)

#### <a name="maximum-combined-throughput"></a>Maximale gecombineerde doorvoer

Als u de maximale gecombineerde lezen en schrijven doorvoer, gebruikt u een groter blok en grote wachtrijdiepte met meerdere threads uitvoeren van lees- en schrijfbewerkingen. U kunt een blokgrootte van 64 KB en wachtrijdiepte van 128.
