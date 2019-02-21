---
title: bestand opnemen
description: bestand opnemen
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 01/22/2019
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 8a067474fb172d4ff7a7fdf7eb6d24536bd2d017
ms.sourcegitcommit: 9aa9552c4ae8635e97bdec78fccbb989b1587548
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/20/2019
ms.locfileid: "56443353"
---
# <a name="what-disk-types-are-available-in-azure"></a>Welke schijftypen zijn beschikbaar in Azure?

Azure-beheerde schijven biedt momenteel vier schijftypen, drie die algemeen beschikbaar (GA) en één die momenteel in preview zijn. Deze vier schijftypen elke hebben hun eigen juiste doel klantscenario's.

## <a name="disk-comparison"></a>Vergelijking van schijven

De volgende tabel bevat een vergelijking van ultra solid-state-stations (SSD) (preview), premium SSD, standard-SSD en standaard harde schijven (HDD) voor beheerde schijven om te bepalen wat u wilt gebruiken.

|   | Ultra SSD (preview)   | Premium SSD   | Standard - SSD   | Standard HDD   |
|---------|---------|---------|---------|---------|
|Schijftype   |SSD   |SSD   |SSD   |HDD   |
|Scenario   |I/o-intensieve workloads zoals SAP HANA, databases van de bovenste laag (bijvoorbeeld SQL, Oracle) en andere transactie zware workloads.   |Productie- en prestatiegevoelige workloads   |Webservers, bedrijfstoepassingen weinig wordt gebruikt en ontwikkelen en testen   |Back-up, niet-kritieke, incidentele toegang   |
|Schijfgrootte   |65.536 gibibyte (GiB) (Preview)   |4095 giB (GA), 32.767 GiB (Preview)    |4095 (GA) GiB, 32.767 GiB (Preview)   |4095 giB (GA), 32.767 GiB (Preview)   |
|Max. doorvoer   |2000 MiB/s (Preview)   |250 (GA) MiB/s, 750 MiB/s (Preview)   |60 MiB/s (GA), 500 MiB/s (Preview)   |60 Mib/s (GA), 500 MiB/s (Preview)   |
|Max. IOP's   |160.000 (preview)   |(GA), 7500 20.000 (Preview)   |500 (GA), 2000 (Preview)   |500 (GA), 2000 (Preview)   |

## <a name="ultra-ssd-preview"></a>Ultra SSD (preview)

Azure ultra SSD (preview) bieden hoge doorvoer, hoge IOPS en consistente lage latentie schijfopslag voor Azure IaaS VM's. Enkele aanvullende voordelen van ultra SSD zijn de mogelijkheid om de prestaties van de schijf, samen met uw workloads, zonder dat uw virtuele machines opnieuw moet worden dynamisch te wijzigen. Ultra SSD zijn geschikt voor gegevensintensieve workloads, zoals SAP HANA, databases van de bovenste laag en transactie-zware workloads. Ultra SSD kan alleen worden gebruikt als gegevensschijven. We adviseren premium SSD's gebruiken als de OS-schijven.

### <a name="performance"></a>Prestaties

Als u een ultra schijf inricht, kunt u de capaciteit en de prestaties van de schijf afzonderlijk configureren. Ultra SSD zijn verkrijgbaar in verschillende vaste grootte, variërend van 4 GiB maximaal 64 TiB en voorzien van een flexibele prestaties configuratiemodel waarmee u IOPS en doorvoer onafhankelijk van elkaar te configureren.

Er zijn enkele kernfuncties van Ultra SSD:

- Capaciteit van de schijf: Ultra bereiken van SSD-capaciteit van 4 GiB maximaal 64 TiB.
- Schijf-IOPS: Ultra SSD ondersteuning voor IOPS-limieten van 300 IOPS/GiB, tot een maximum van 160 kB IOPS per schijf. Zorg ervoor dat de geselecteerde schijf-IOPS kleiner dan de IOPS VM zijn voor het bereiken van de IOPS-waarde die u hebt ingericht. De minimale IOPS-schijf zijn 100 IOPS.
- Schijfdoorvoer: Met de ultra SSD, de doorvoerlimiet van één schijf is 256 KiB/s voor elk IOP's, tot een maximum van 2000 MBps per schijf ingericht (waarbij MBps = 10 ^ 6 Bytes per seconde). De schijfdoorvoer, met minimale is 1 MiB.

### <a name="disk-size"></a>Schijfgrootte

|Schijfgrootte (GiB)  |IOPS Caps  |Bovengrens voor doorvoer (MBps)  |
|---------|---------|---------|
|4     |1,200         |300         |
|8     |2,400         |600         |
|16     |4,800         |1,200         |
|32     |9,600         |2,000         |
|64     |19,200         |2,000         |
|128     |38,400         |2,000         |
|256     |76,800         |2,000         |
|512     |80,000         |2,000         |
|1024-65.536 (grootten die in dit bereik verhogen in stappen van 1 TiB)     |160,000         |2,000         |

### <a name="preview-scope-and-limitations"></a>Bereik van de Preview-versie en beperkingen

Tijdens de preview, ultra SSD:

- In VS-Oost 2 in één beschikbaarheidszone worden ondersteund  
- Kan alleen worden gebruikt met beschikbaarheidszones (beschikbaarheidssets en één VM-implementaties buiten zones worden niet hebben de mogelijkheid om te koppelen van een ultra-schijf)
- Worden alleen ondersteund op ES/DS v3-VM 's
- Zijn alleen beschikbaar als gegevensschijven en alleen ondersteuning voor 4k die fysieke sectorgrootte  
- Kunnen alleen worden gemaakt als de lege schijven  
- Momenteel kan alleen worden geïmplementeerd met behulp van Azure Resource Manager-sjablonen, CLI en de python-SDK.
- Ondersteunt nog geen momentopnamen van de schijf, VM-installatiekopieën, beschikbaarheidssets, virtuele-machineschaalsets en Azure disk encryption.
- Integratie met Azure Backup of Azure Site Recovery biedt geen nog ondersteuning.
- Net als bij [meeste previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/), deze functie mag niet worden gebruikt voor werkbelastingen voor productie tot algemene beschikbaarheid (GA).

## <a name="premium-ssd"></a>Premium SSD

Azure premium SSD's bieden ondersteuning voor hoge prestaties en lage latentie schijven voor virtuele machines (VM's) met invoer/uitvoer (I/O)-intensieve workloads. Als u wilt profiteren van de snelheid en prestaties van premium storage-schijven, kunt u bestaande VM-schijven naar Premium SSD's migreren. Premium SSD's zijn geschikt voor essentiële productietoepassingen.

### <a name="disk-size"></a>Schijfgrootte

Grootten gemarkeerd met een sterretje zijn momenteel in preview.

| Premium-SSD-grootten  | P4               | P6               | P10             | P15 | P20              | P30              | P40              | P50              | P60*              | P70*              | P80*              |
|---------------------|---------------------|---------------------|------------------|------------------|------------------|------------------|------------------|------------------|------------------|------------------|------------------|
| Schijfgrootte in GiB           | 32             | 64             | 128            | 256  | 512            | 1,024    | 2,048     | 4,095    | 8.192     | 16,384     | 32,767     |
| IOP's per schijf       | Maximaal kunnen 120 | Tot 240              | Maximaal 500              | Maximaal 1100 | Maximaal 2300              | Maximaal 5.000              | Maximaal 7.500             | Maximaal 7.500              | Maximaal 12.500              | Maximaal 15.000              | Maximaal 20.000              |
| Doorvoer per schijf | Maximaal 25 MiB/sec | Maximaal 50 MiB/sec | Maximaal 100 MiB/sec | Maximaal 125 MiB/sec | Maximaal 150 MiB/sec | Maximaal 200 MiB/sec | Maximaal 250 MiB/sec | Maximaal 250 MiB/sec| Maximaal 480 MiB/sec | Maximaal 750 MiB/sec | Maximaal 750 MiB/sec |

## <a name="standard-ssd"></a>Standard - SSD

Azure standard-SSD's zijn een optie voor rendabele opslag geoptimaliseerd voor workloads die consistente prestaties op lagere niveaus van IOP's nodig. Standard-SSD biedt een goede op instapniveau voor diegenen die u verplaatsen naar de cloud, wilt met name als u problemen ondervindt met de variantie van workloads die on-premises worden uitgevoerd op uw harde schijf-oplossingen. Standard-SSD's voorzien in betere beschikbaarheid, consistentie, betrouwbaarheid en latentie in vergelijking met harde schijven. Standard-SSD's zijn geschikt voor webservers, toepassingsservers met lage IOPS, weinig wordt gebruikt zakelijke toepassingen en werkbelastingen voor ontwikkelen/testen.

### <a name="disk-size"></a>Schijfgrootte

Grootten gemarkeerd met een sterretje zijn momenteel in preview.

| Standard-SSD-grootten  | E10               | E15               | E20             | E30 | E40              | E50              | E60*              | E70*              | E80*              |
|---------------------|---------------------|---------------------|------------------|------------------|------------------|------------------|------------------|------------------|------------------|
| Schijfgrootte in GiB           | 128             | 256             | 512            | 1,024  | 2,048            | 4,095     | 8.192     | 16,384     | 32,767    |
| IOP's per schijf       | Maximaal 500              | Maximaal 500              | Maximaal 500              | Maximaal 500 | Maximaal 500              | Maximaal 500              | Maximaal 500             | Maximaal 500              | Maximaal 1.300              | Maximaal 2000              | Maximaal 2000              |
| Doorvoer per schijf | Maximaal 60 MiB/sec | Maximaal 60 MiB/sec | Maximaal 60 MiB/sec | Maximaal 60 MiB/sec | Maximaal 60 MiB/sec | Maximaal 60 MiB/sec | Maximaal 60 MiB/sec | Maximaal 60 MiB/sec| Maximaal 300 MiB/sec |  Maximaal 500 MiB/sec | Maximaal 500 MiB/sec |

## <a name="standard-hdd"></a>Standard HDD

Azure standard HDD's leveren betrouwbare, voordelige schijfondersteuning voor virtuele machines waarop niet gevoelig werkbelastingen worden uitgevoerd. Het ondersteunt ook blobs, tabellen, wachtrijen en bestanden. Met de standard-opslag, worden de gegevens worden opgeslagen op hardeschijfstations (HDD's). Als u werkt met virtuele machines, kunt u standaard SSD en HDD-schijven kunt gebruiken voor scenario's voor ontwikkelen/testen en minder kritieke werkbelastingen. Standard-opslag is beschikbaar in alle Azure-regio's.

### <a name="disk-size"></a>Schijfgrootte

Grootten gemarkeerd met een sterretje zijn momenteel in preview.

| Standard-schijftype  | S4               | S6               | S10             | S15 | S20              | S30              | S40              | S50              | S60*              | S70*              | S80 *              |
|---------------------|---------------------|---------------------|------------------|------------------|------------------|------------------|------------------|------------------|------------------|------------------|------------------|
| Schijfgrootte in GiB          | 32             | 64             | 128            | 256  | 512            | 1,024    | 2,048     | 4,095    | 8.192     | 16,384     | 32,767     |
| IOP's per schijf       | Maximaal 500              | Maximaal 500              | Maximaal 500              | Maximaal 500 | Maximaal 500              | Maximaal 500              | Maximaal 500             | Maximaal 500              | Maximaal 1.300              | Maximaal 2000              | Maximaal 2000              |
| Doorvoer per schijf | Maximaal 60 MiB/sec | Maximaal 60 MiB/sec | Maximaal 60 MiB/sec | Maximaal 60 MiB/sec | Maximaal 60 MiB/sec | Maximaal 60 MiB/sec | Maximaal 60 MiB/sec | Maximaal 60 MiB/sec| Maximaal 300 MiB/sec | Maximaal 500 MiB/sec | Maximaal 500 MiB/sec |

## <a name="billing"></a>Billing

Als u beheerde schijven gebruikt, zijn de volgende factureringsvoorwaarden van toepassing:

- Schijftype
- Grootte van de beheerde schijf
- Momentopnamen
- Uitgaande gegevensoverdracht
- Aantal transacties

**Beheerde schijfgrootte**: beheerde schijven worden in rekening gebracht voor de ingerichte grootte. Azure wijst de ingerichte grootte (afgerond) naar de dichtstbijzijnde aangeboden schijfgrootte. Voor meer informatie over de grootte van de schijven die worden aangeboden, Zie de vorige tabellen. Elke schijf wordt toegewezen aan een aanbieding van de grootte van ondersteunde ingerichte schijf en dienovereenkomstig wordt gefactureerd. Bijvoorbeeld, als u een 200 GiB Standard-SSD ingericht, het wordt toegewezen aan de aanbieding van de grootte van schijf van E15 (256 GB). Facturering voor ingerichte schijven is per uur Pro rata met behulp van de maandelijkse prijs voor de Premium-opslag-aanbieding. Bijvoorbeeld, als u een schijf E10 ingericht en deze na 20 uur verwijderd, wordt u gefactureerd voor de aanbieding E10 20 uur Pro rata berekend. Dit is, ongeacht de hoeveelheid feitelijke gegevens geschreven naar de schijf.

**Momentopnamen**: Momentopnamen worden in rekening gebracht op basis van de gebruikte grootte. Als u een momentopname van een beheerde schijf met ingerichte capaciteit van 64 GiB en de werkelijke gebruikte gegevensgrootte van 10 GiB maakt, kunt u voor de momentopname, wordt gefactureerd alleen voor de gebruikte gegevensgrootte van 10 GiB.
