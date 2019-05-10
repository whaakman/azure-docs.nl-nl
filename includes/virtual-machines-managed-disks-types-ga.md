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
ms.openlocfilehash: 885bc1c627626ee7ba4f391be31131b18fa1ab39
ms.sourcegitcommit: 0568c7aefd67185fd8e1400aed84c5af4f1597f9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65211951"
---
## <a name="premium-ssd"></a>Premium SSD

Azure premium SSD's bieden ondersteuning voor hoge prestaties en lage latentie schijven voor virtuele machines (VM's) met invoer/uitvoer (I/O)-intensieve workloads. Als u wilt profiteren van de snelheid en prestaties van premium storage-schijven, kunt u bestaande VM-schijven naar Premium SSD's migreren. Premium SSD's zijn geschikt voor essentiële productietoepassingen.

### <a name="disk-size"></a>Schijfgrootte
[!INCLUDE [disk-storage-premium-ssd-sizes](disk-storage-premium-ssd-sizes.md)]

Wanneer u een premium-opslagschijf, in tegenstelling tot standaard opslag inrichten kunt u de capaciteit, IOPS en doorvoer van die schijf worden gegarandeerd. Als u een P50 schijf maakt, richt Azure bijvoorbeeld 4.095 GB opslagcapaciteit, 7.500 IOPS en doorvoer van 250 MB/s voor de schijf. Uw toepassing kunt gebruiken of een deel van de capaciteit en prestaties. Premium-SSD-schijven zijn ontworpen om het doel prestaties 99,9% van de tijd.

## <a name="standard-ssd"></a>Standard - SSD

Azure standard-SSD's zijn een optie voor rendabele opslag geoptimaliseerd voor workloads die consistente prestaties op lagere niveaus van IOP's nodig. Standard-SSD biedt een goede op instapniveau voor diegenen die u verplaatsen naar de cloud, wilt met name als u problemen ondervindt met de variantie van workloads die on-premises worden uitgevoerd op uw harde schijf-oplossingen. Standard-SSD's voorzien in betere beschikbaarheid, consistentie, betrouwbaarheid en latentie in vergelijking met harde schijven. Standard-SSD's zijn geschikt voor webservers, toepassingsservers met lage IOPS, weinig wordt gebruikt zakelijke toepassingen en werkbelastingen voor ontwikkelen/testen.

### <a name="disk-size"></a>Schijfgrootte
[!INCLUDE [disk-storage-standard-ssd-sizes](disk-storage-standard-ssd-sizes.md)]

Standard-SSD's zijn ontworpen om latentie in milliseconden voor de meeste i/o-bewerkingen, en om u te bieden de IOPS en doorvoer van maximaal de limieten die worden beschreven in de voorgaande tabel 99% van de tijd. Werkelijke IOPS en doorvoer kunnen soms variëren, afhankelijk van de patronen in het netwerkverkeer. Standard-SSD's biedt consistente prestaties dan de HDD-schijven met een lagere latentie.

## <a name="standard-hdd"></a>Standard - HDD

Azure standard HDD's leveren betrouwbare, voordelige schijfondersteuning voor virtuele machines waarop niet gevoelig werkbelastingen worden uitgevoerd. Het ondersteunt ook blobs, tabellen, wachtrijen en bestanden. Met de standard-opslag, worden de gegevens worden opgeslagen op hardeschijfstations (HDD's). Als u werkt met virtuele machines, kunt u standaard SSD en HDD-schijven kunt gebruiken voor scenario's voor ontwikkelen/testen en minder kritieke werkbelastingen. Standard-opslag is beschikbaar in alle Azure-regio's.

### <a name="disk-size"></a>Schijfgrootte
[!INCLUDE [disk-storage-standard-hdd-sizes](disk-storage-standard-hdd-sizes.md)]

## <a name="billing"></a>Facturering

Als u beheerde schijven gebruikt, zijn de volgende factureringsvoorwaarden van toepassing:

- Schijftype
- Grootte van de beheerde schijf
- Momentopnamen
- Uitgaande gegevensoverdracht
- Aantal transacties

**Beheerde schijfgrootte**: beheerde schijven worden in rekening gebracht voor de ingerichte grootte. Azure wijst de ingerichte grootte (afgerond) naar de dichtstbijzijnde aangeboden schijfgrootte. Voor meer informatie over de grootte van de schijven die worden aangeboden, Zie de vorige tabellen. Elke schijf wordt toegewezen aan een aanbieding van de grootte van ondersteunde ingerichte schijf en dienovereenkomstig wordt gefactureerd. Bijvoorbeeld, als u een 200 GiB Standard-SSD ingericht, het wordt toegewezen aan de aanbieding van de grootte van schijf van E15 (256 GB). Facturering voor ingerichte schijven is per uur Pro rata met behulp van de maandelijkse prijs voor de Premium-opslag-aanbieding. Bijvoorbeeld, als u een schijf E10 ingericht en deze na 20 uur verwijderd, wordt u gefactureerd voor de aanbieding E10 20 uur Pro rata berekend. Dit is, ongeacht de hoeveelheid feitelijke gegevens geschreven naar de schijf.

**Momentopnamen**: Momentopnamen worden in rekening gebracht op basis van de gebruikte grootte. Als u een momentopname van een beheerde schijf met ingerichte capaciteit van 64 GiB en de werkelijke gebruikte gegevensgrootte van 10 GiB maakt, kunt u voor de momentopname, wordt gefactureerd alleen voor de gebruikte gegevensgrootte van 10 GiB.