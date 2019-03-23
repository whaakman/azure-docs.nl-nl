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
ms.openlocfilehash: 88a4110d68dc8aa921d647f90de654d2ebb4e17d
ms.sourcegitcommit: 49c8204824c4f7b067cd35dbd0d44352f7e1f95e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/22/2019
ms.locfileid: "58395526"
---
## <a name="premium-ssd"></a>Premium SSD

Azure premium SSD's bieden ondersteuning voor hoge prestaties en lage latentie schijven voor virtuele machines (VM's) met invoer/uitvoer (I/O)-intensieve workloads. Als u wilt profiteren van de snelheid en prestaties van premium storage-schijven, kunt u bestaande VM-schijven naar Premium SSD's migreren. Premium SSD's zijn geschikt voor essentiële productietoepassingen.

### <a name="disk-size"></a>Schijfgrootte
[!INCLUDE [disk-storage-premium-ssd-sizes](disk-storage-premium-ssd-sizes.md)]

## <a name="standard-ssd"></a>Standard - SSD

Azure standard-SSD's zijn een optie voor rendabele opslag geoptimaliseerd voor workloads die consistente prestaties op lagere niveaus van IOP's nodig. Standard-SSD biedt een goede op instapniveau voor diegenen die u verplaatsen naar de cloud, wilt met name als u problemen ondervindt met de variantie van workloads die on-premises worden uitgevoerd op uw harde schijf-oplossingen. Standard-SSD's voorzien in betere beschikbaarheid, consistentie, betrouwbaarheid en latentie in vergelijking met harde schijven. Standard-SSD's zijn geschikt voor webservers, toepassingsservers met lage IOPS, weinig wordt gebruikt zakelijke toepassingen en werkbelastingen voor ontwikkelen/testen.

### <a name="disk-size"></a>Schijfgrootte
[!INCLUDE [disk-storage-standard-ssd-sizes](disk-storage-standard-ssd-sizes.md)]

## <a name="standard-hdd"></a>Standard HDD

Azure standard HDD's leveren betrouwbare, voordelige schijfondersteuning voor virtuele machines waarop niet gevoelig werkbelastingen worden uitgevoerd. Het ondersteunt ook blobs, tabellen, wachtrijen en bestanden. Met de standard-opslag, worden de gegevens worden opgeslagen op hardeschijfstations (HDD's). Als u werkt met virtuele machines, kunt u standaard SSD en HDD-schijven kunt gebruiken voor scenario's voor ontwikkelen/testen en minder kritieke werkbelastingen. Standard-opslag is beschikbaar in alle Azure-regio's.

### <a name="disk-size"></a>Schijfgrootte
[!INCLUDE [disk-storage-standard-hdd-sizes](disk-storage-standard-hdd-sizes.md)]

## <a name="billing"></a>Billing

Als u beheerde schijven gebruikt, zijn de volgende factureringsvoorwaarden van toepassing:

- Schijftype
- Grootte van de beheerde schijf
- Momentopnamen
- Uitgaande gegevensoverdracht
- Aantal transacties

**Beheerde schijfgrootte**: beheerde schijven worden in rekening gebracht voor de ingerichte grootte. Azure wijst de ingerichte grootte (afgerond) naar de dichtstbijzijnde aangeboden schijfgrootte. Voor meer informatie over de grootte van de schijven die worden aangeboden, Zie de vorige tabellen. Elke schijf wordt toegewezen aan een aanbieding van de grootte van ondersteunde ingerichte schijf en dienovereenkomstig wordt gefactureerd. Bijvoorbeeld, als u een 200 GiB Standard-SSD ingericht, het wordt toegewezen aan de aanbieding van de grootte van schijf van E15 (256 GB). Facturering voor ingerichte schijven is per uur Pro rata met behulp van de maandelijkse prijs voor de Premium-opslag-aanbieding. Bijvoorbeeld, als u een schijf E10 ingericht en deze na 20 uur verwijderd, wordt u gefactureerd voor de aanbieding E10 20 uur Pro rata berekend. Dit is, ongeacht de hoeveelheid feitelijke gegevens geschreven naar de schijf.

**Momentopnamen**: Momentopnamen worden in rekening gebracht op basis van de gebruikte grootte. Als u een momentopname van een beheerde schijf met ingerichte capaciteit van 64 GiB en de werkelijke gebruikte gegevensgrootte van 10 GiB maakt, kunt u voor de momentopname, wordt gefactureerd alleen voor de gebruikte gegevensgrootte van 10 GiB.