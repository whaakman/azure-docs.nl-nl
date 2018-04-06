---
title: bestand opnemen
description: bestand opnemen
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 03/26/2018
ms.author: jeking
ms.custom: include file
ms.openlocfilehash: f6d437e4ad0e05d55c408ad8f9defe5385b52050
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/05/2018
---
Zone-redundante opslag (ZRS) wordt synchroon uw gegevens repliceert over drie opslagclusters in één regio. Elke opslagcluster fysiek wordt gescheiden van de andere en bevindt zich in een eigen zone beschikbaarheid (AZ). Elke zone beschikbaarheid en het cluster ZRS erin is autonome met afzonderlijke hulpprogramma's en netwerkmogelijkheden.

Opslaan van gegevens in een account ZRS zorgt ervoor dat u toegang tot en beheer van uw gegevens in het geval dat een zone niet meer beschikbaar is. ZRS biedt uitstekende prestaties en zeer lage latentie.

Zie voor meer informatie over beschikbaarheid zones [beschikbaarheid Zones overzicht](https://docs.microsoft.com/azure/availability-zones/az-overview).

## <a name="zrs-for-high-availability"></a>ZRS voor hoge beschikbaarheid 

U kunt ZRS voor scenario's waarvoor sterke consistentie, sterk duurzaamheid en maximale beschikbaarheid, zelfs als er een storing of een natuurramp renders een datacenter is niet beschikbaar. ZRS biedt duurzaamheid voor opslagobjecten van ten minste 99.9999999999% (12 van 9) gedurende een bepaald jaar.

ZRS ondersteunt momenteel standaard, algemene v2 accounttypen (GPv2). ZRS is beschikbaar voor blok-blobs, niet-disk-pagina-blobs, bestanden, tabellen en wachtrijen. 

ZRS is algemeen beschikbaar op de volgende gebieden:

- VS Oost 2
- VS Centraal
- Noord-Europa
- West-Europa
- Frankrijk - centraal
- Zuidoost-Azië

Microsoft blijft ZRS in andere Azure-regio's inschakelen.

### <a name="what-happens-when-a-zone-becomes-unavailable"></a>Wat gebeurt er wanneer een zone niet meer beschikbaar is?

Uw gegevens blijft robuuste als een zone niet beschikbaar is. Microsoft raadt aan dat u kunt volgen procedures voor tijdelijke fout verwerken blijven, zoals het implementeren van beleid voor opnieuw proberen met exponentieel back uitschakelen. Wanneer een zone niet beschikbaar is, zal de Azure VPN-updates, zoals DNS-repointing. Deze updates kunnen invloed hebben op uw toepassing als u toegang uw gegevens tot voordat ze hebt voltooid.

ZRS mag niet uw gegevens beschermen tegen een regionale ramp optreedt waarbij meerdere zones permanent worden beïnvloed. ZRS biedt in plaats daarvan tolerantie voor uw gegevens in het geval van een tijdelijk niet beschikbaar zijn. Voor beveiliging tegen rampen regionale Microsoft raadt u [geografisch redundante opslag (GRS): Cross-regionale replicatie voor de Azure Storage](../articles/storage/common/storage-redundancy-grs.md).

## <a name="zrs-classic-a-legacy-option-for-block-blobs-redundancy"></a>ZRS klassiek: Een verouderde optie voor de redundantie van blok-blobs
> [!NOTE]
> Klassieke ZRS-accounts zijn gepland voor afschaffing en vereiste migratie op 31 maart 2021. Microsoft verzendt meer informatie naar ZRS klassieke klanten vóór de afschaffing. Microsoft-abonnementen bieden een geautomatiseerde migratieproces van klassieke ZRS aan ZRS in de toekomst.

Klassieke ZRS is alleen beschikbaar voor blok-blobs in algemene V1 storage-accounts (GPv1). ZRS klassieke worden gegevens asynchroon gerepliceerd via datacenters binnen een tot twee regio's. Een replica is mogelijk niet beschikbaar tenzij Microsoft failover naar de secundaire initieert. Een klassiek ZRS-account kan niet worden geconverteerd naar of van LRS of GRS en heeft geen metrische gegevens of logboek-functionaliteit.

ZRS klassieke accounts kunnen niet worden geconverteerd naar of van LRS, GRS of RA-GRS. ZRS klassieke accounts ook ondersteunen geen metrische gegevens of de registratie.

Zodra de ZRS is algemeen beschikbaar in een regio, worden niet langer een klassieke ZRS-account maken vanuit de portal in deze regio, maar kunt u een andere wijze zoals PowerShell, CLI enzovoort.

Gebruik handmatig ZRS-account om gegevens te migreren naar of van een LRS, ZRS klassieke, GRS of RA-GRS-account, AzCopy, Azure Storage Explorer, Azure PowerShell of Azure CLI. U kunt ook uw eigen migratieoplossing met een van de clientbibliotheken van Azure Storage opbouwen.