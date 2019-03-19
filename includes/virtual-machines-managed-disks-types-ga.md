---
title: bestand opnemen
description: bestand opnemen
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 03/13/2019
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 0201776914610ddaca50a670fc500156a65cd734
ms.sourcegitcommit: f596d88d776a3699f8c8cf98415eb874187e2a48
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/16/2019
ms.locfileid: "58124682"
---
## <a name="premium-ssd"></a>Premium SSD

Azure premium SSD's bieden ondersteuning voor hoge prestaties en lage latentie schijven voor virtuele machines (VM's) met invoer/uitvoer (I/O)-intensieve workloads. Als u wilt profiteren van de snelheid en prestaties van premium storage-schijven, kunt u bestaande VM-schijven naar Premium SSD's migreren. Premium SSD's zijn geschikt voor essentiële productietoepassingen.

### <a name="disk-size"></a>Schijfgrootte

Grootten gemarkeerd met een sterretje zijn momenteel in preview.

| Premium-SSD-grootten | P4 | P6 | P10 | P15 | P20 | P30 | P40 | P50 | P60* | P70* | P80* |
|-------------------|----|----|-----|-----|-----|-----|-----|-----|------|------|------|
| Schijfgrootte in GiB | 32 | 64 | 128 | 256 | 512 | 1,024 | 2,048 | 4,095 | 8.192 | 16,384 | 32,767 |
| IOP's per schijf | Maximaal kunnen 120 | Tot 240 | Maximaal 500 | Maximaal 1100 | Maximaal 2300 | Maximaal 5.000 | Maximaal 7.500 | Maximaal 7.500 | Maximaal 12.500 | Maximaal 15.000 | Maximaal 20.000 |
| Doorvoer per schijf | Maximaal 25 MiB/sec | Maximaal 50 MiB/sec | Maximaal 100 MiB/sec | Maximaal 125 MiB/sec | Maximaal 150 MiB/sec | Maximaal 200 MiB/sec | Maximaal 250 MiB/sec | Maximaal 250 MiB/sec| Maximaal 480 MiB/sec | Maximaal 750 MiB/sec | Maximaal 750 MiB/sec |

## <a name="standard-ssd"></a>Standard - SSD

Azure standard-SSD's zijn een optie voor rendabele opslag geoptimaliseerd voor workloads die consistente prestaties op lagere niveaus van IOP's nodig. Standard-SSD biedt een goede op instapniveau voor diegenen die u verplaatsen naar de cloud, wilt met name als u problemen ondervindt met de variantie van workloads die on-premises worden uitgevoerd op uw harde schijf-oplossingen. Standard-SSD's voorzien in betere beschikbaarheid, consistentie, betrouwbaarheid en latentie in vergelijking met harde schijven. Standard-SSD's zijn geschikt voor webservers, toepassingsservers met lage IOPS, weinig wordt gebruikt zakelijke toepassingen en werkbelastingen voor ontwikkelen/testen.

### <a name="disk-size"></a>Schijfgrootte

Grootten gemarkeerd met een sterretje zijn momenteel in preview.

| Standard-SSD-grootten | E4 | E6 | E10 | E15 | E20 | E30 | E40 | E50 | E60* | E70* | E80* |
|--------------------|----|----|-----|-----|-----|-----|-----|-----|------|------|------|
| Schijfgrootte in GiB | 32 | 64 | 128 | 256 | 512 | 1,024 | 2,048 | 4,095 | 8.192 | 16,384 | 32,767 |
| IOP's per schijf | Maximaal kunnen 120 | Tot 240 | Maximaal 500 | Maximaal 500 | Maximaal 500 | Maximaal 500 | Maximaal 500 | Maximaal 500 | Maximaal 1.300 | Maximaal 2000 | Maximaal 2000 |
| Doorvoer per schijf |  Maximaal 25 MiB/sec |  Maximaal 50 MiB/sec  |  Maximaal 60 MiB/sec | Maximaal 60 MiB/sec | Maximaal 60 MiB/sec | Maximaal 60 MiB/sec | Maximaal 60 MiB/sec | Maximaal 60 MiB/sec| Maximaal 300 MiB/sec |  Maximaal 500 MiB/sec | Maximaal 500 MiB/sec |

## <a name="standard-hdd"></a>Standard HDD

Azure standard HDD's leveren betrouwbare, voordelige schijfondersteuning voor virtuele machines waarop niet gevoelig werkbelastingen worden uitgevoerd. Het ondersteunt ook blobs, tabellen, wachtrijen en bestanden. Met de standard-opslag, worden de gegevens worden opgeslagen op hardeschijfstations (HDD's). Als u werkt met virtuele machines, kunt u standaard SSD en HDD-schijven kunt gebruiken voor scenario's voor ontwikkelen/testen en minder kritieke werkbelastingen. Standard-opslag is beschikbaar in alle Azure-regio's.

### <a name="disk-size"></a>Schijfgrootte

Grootten gemarkeerd met een sterretje zijn momenteel in preview.

| Standard-schijftype | S4 | S6 | S10 | S15 | S20 | S30 | S40 | S50 | S60* | S70* | S80 * |
|--------------------|----|----|-----|-----|-----|-----|-----|-----|------|------|------|
| Schijfgrootte in GiB | 32 | 64 | 128 | 256 | 512 | 1,024 | 2,048 | 4,095 | 8.192 | 16,384 | 32,767 |
| IOP's per schijf | Maximaal 500 | Maximaal 500 | Maximaal 500 | Maximaal 500 | Maximaal 500 | Maximaal 500 | Maximaal 500 | Maximaal 500 | Maximaal 1.300 | Maximaal 2000 | Maximaal 2000 |
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