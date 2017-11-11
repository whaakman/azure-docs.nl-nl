---
title: Kies een SKU of prijscategorie voor Azure Search | Microsoft Docs
description: 'Azure Search kan worden ingericht op deze SKU''s: gratis, basis en standaard, waarbij standaard is beschikbaar in verschillende configuraties van resources en capaciteit niveaus.'
services: search
documentationcenter: 
author: HeidiSteen
manager: jhubbard
editor: 
tags: azure-portal
ms.assetid: 8d4b7bca-02a5-43ee-b3f8-03551dfb32fd
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 10/24/2016
ms.author: heidist
ms.openlocfilehash: 781683f27c943e25d5629dd846da357f51c9d4f9
ms.sourcegitcommit: bc8d39fa83b3c4a66457fba007d215bccd8be985
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/10/2017
---
# <a name="choose-a-sku-or-pricing-tier-for-azure-search"></a>Kies een SKU of prijscategorie voor Azure Search
In Azure Search een [service is ingericht](search-create-service-portal.md) op een specifieke prijscategorie of SKU. Opties zijn onder andere **vrije**, **Basic**, of **standaard**, waarbij **standaard** is beschikbaar in meerdere configuraties en capaciteit.

Het doel van dit artikel is bij het kiezen van een laag. Als een laag capaciteit blijkt te laag, moet u een nieuwe service op de hogere lagen in te richten en vervolgens de indexen opnieuw te laden. Er is geen in-place upgrade van dezelfde service van een SKU naar een andere.

> [!NOTE]
> Nadat u een laag kiezen en [inrichten van een zoekservice](search-create-service-portal.md), kunt u de replica verhogen en partitie telt in de service. Zie voor instructies [schalen niveaus van de bron voor query's en workloads indexering](search-capacity-planning.md).
>
>

## <a name="how-to-approach-a-pricing-tier-decision"></a>Een prijscategorie laag beslissing benadering
In Azure Search bepaalt de laag niet functiebeschikbaarheid capaciteit. In het algemeen zijn functies beschikbaar op elke laag, met inbegrip van de preview-functies. De enige uitzondering hierop is geen ondersteuning voor indexeerfuncties in S3 HD.

> [!TIP]
> Het is raadzaam dat u altijd inrichten een **vrije** service (één per abonnement zonder vervaldatum) zodat de direct beschikbaar voor lichte projecten. Gebruik de **vrije** service voor testen en evalueren; te maken van een tweede factureerbare service op de **Basic** of **standaard** -laag voor de productie of groter testwerkbelastingen.
>
>

Ga hand voorhanden capaciteit en de kosten van het uitvoeren van de service. De informatie in dit artikel kunt u bepalen welke SKU levert de juiste balans, maar een van deze nuttig zijn, moet u ten minste ruwe schattingen op de volgende:

* Aantal en de grootte van indexen die u wilt maken
* Aantal en de grootte van documenten te uploaden
* Een idee van de query-volume, in termen van query's Per tweede (QPS). Zie voor instructies [Azure Search-prestaties en optimalisatie](search-performance-optimization.md).

Aantal en de grootte zijn belangrijk omdat de maximale limiet wordt bereikt door middel van een vaste limiet op het aantal indexen per service of op resources (opslag of replica's) dat wordt gebruikt door de service. De werkelijke limiet voor uw service is afhankelijk van wat eerst wordt gebruikt: bronnen of objecten.

Schattingen in hand moeten de volgende stappen uit het proces vereenvoudigen:

* **Stap 1** beschrijvingen van de SKU hieronder voor meer informatie over de beschikbare opties bekijken.
* **Stap 2** de onderstaande moet worden uitgevoerd op een voorlopige beslissing vragen beantwoorden.
* **Stap 3** uw beslissing voltooien door de vaste limieten op opslag controleren en prijzen.

## <a name="sku-descriptions"></a>Beschrijvingen van de SKU
De volgende tabel bevat beschrijvingen van elke laag.

| Laag | Primaire scenario's |
| --- | --- |
| **Gratis** |Een gedeelde service, zonder kosten, gebruikt voor de evaluatie, onderzoek of kleine werkbelastingen. Omdat deze wordt gedeeld met andere abonnees, varieert query-doorvoer en indexeren op basis van wie de service wordt gebruikt. Capaciteit is klein (50 MB of 3 indexen met van 10.000 documenten elk). |
| **Basic** |Kleine productieworkloads op specifieke hardware. Maximaal beschikbaar. De capaciteit is maximaal 3 replica's en 1 partitie (2 GB). |
| **S1** |Standaard 1 ondersteunt flexibele combinaties van partities (12) en replica's (12) gebruikt voor gemiddeld productieworkloads op specifieke hardware. U kunt partities en replica's in de combinaties die worden ondersteund door het maximale aantal 36 factureerbare search-eenheden toewijzen. Partities zijn op dit niveau 25 GB. |
| **S2** |Standaard 2 wordt uitgevoerd met behulp van de dezelfde 36 search-eenheden als S1, maar met grotere grootte partities en replica's groter productie-workloads. Partities zijn op dit niveau 100 GB. |
| **S3** |Standaard 3 wordt uitgevoerd proportioneel groter productieworkloads op hogere end-systemen in de configuratie van maximaal 12 partities of 12 replica's onder 36 search-eenheden. Partities zijn op dit niveau 200 GB. |
| **HD S3** |Standaard 3 high-density is ontworpen voor een groot aantal kleinere indexen. U kunt maximaal 3 partities hebben van 200 GB.|

> [!NOTE]
> Replica- en partitie maximumwaarden wordt gefactureerd uit als de search-eenheden (36 maximale per service eenheid), die legt een effectieve ondergrens dan de maximale impliceert op face-waarde. Bijvoorbeeld: voor het gebruik van het maximum van 12 replica's, kan er maximaal 3 partities (12 * 3 = 36 units). Verminder op dezelfde manier voor het gebruik van de maximale partities replica's op 3. Zie [schalen niveaus van de bron voor query's en workloads in Azure Search indexeren](search-capacity-planning.md) voor een grafiek van toegestane combinaties.
>
>

## <a name="review-limits-per-tier"></a>Limieten per laag controleren
De volgende tabel is een subset van de limieten van [Servicelimieten in Azure Search](search-limits-quotas-capacity.md). Hiermee geeft u de meest waarschijnlijke invloed zijn op een beslissing SKU factoren. U kunt verwijzen naar deze grafiek bij het controleren van de onderstaande vragen.

| Resource | Gratis | Basic | S1 | S2 | S3 | S3 HD |
| --- | --- | --- | --- | --- | --- | --- |
| Service Level Agreement (SLA) |Nee <sup>1</sup> |Ja |Ja |Ja |Ja |Ja |
| Limieten voor index |3 |5 |50 |200 |200 |1000 <sup>2</sup> |
| Document limieten |10.000 in totaal |1 miljoen per service |15 miljoen per partitie |60 miljoen per partitie |120 miljoen per partitie |1 miljoen per index |
| Maximum aantal partities |N.v.t. |1 |12 |12 |12 |3 <sup>2</sup> |
| Partitiegrootte |Totaal aantal 50 MB |2 GB per service |25 GB per partitie |100 GB per partitie (maximaal 1,2 TB per service) |200 GB per partitie (tot maximum van 2,4 TB per service) |200 GB (maximaal 600 GB per service) |
| Maximum aantal replica 's |N.v.t. |3 |12 |12 |12 |12 |

<sup>1</sup> gratis laag en de preview-functies niet bij service level agreements (Sla's) worden geleverd. Voor alle factureerbare lagen, sla's van kracht als u voldoende redundantie voor uw service inricht. Twee of meer replica's zijn vereist voor de SLA voor query (gelezen). Drie of meer replica's zijn vereist voor query's en indexering SLA (lezen / schrijven). Het aantal partities is niet een SLA-overweging. 

<sup>2</sup> S3 en S3 HD worden ondersteund door de infrastructuur van identieke hoge capaciteit, maar elk ervan de maximale limiet bereikt die op verschillende manieren. S3 gericht op een kleiner aantal zeer grote indexen. Als zodanig de resource afhankelijk is van de maximumlimiet (2,4 TB voor elke service). S3 HD is bedoeld voor een groot aantal kleine indexen. S3 HD bereikt 1000 indexen, de grenzen in de vorm van indexbeperkingen. Als u een klant S3 HD die meer dan 1000 indexen nodig heeft, moet u contact op met Microsoft Support voor meer informatie over om door te gaan.

## <a name="eliminate-skus-that-dont-meet-requirements"></a>Elimineren SKU's die niet voldoen aan vereisten
De volgende vragen kunt u op de juiste beslissing SKU voor uw workload binnenkomen.

1. Hebt u **Service Level Agreement (SLA)** vereisten? U kunt elke categorie factureerbare (basis op up), maar u moet de service voor de redundantie configureren. Twee of meer replica's zijn vereist voor de SLA voor query (gelezen). Drie of meer replica's zijn vereist voor query's en indexering SLA (lezen / schrijven). Het aantal partities is niet een SLA-overweging.
2. **Hoeveel indexeert** hebben die u nodig? Een van de grootste variabelen waarbij in een beslissing SKU is het aantal indexen die worden ondersteund door elke SKU. Ondersteuning van de index is op veel verschillende niveaus in de onderste Prijscategorieën. Vereisten voor het aantal indexen kunnen een primaire factor van een beslissing SKU worden.
3. **Het aantal documenten** worden geladen in elke index? Het aantal en de grootte van documenten wordt de uiteindelijke grootte van de index bepaald. U kunt dit nummer op basis van de grootte van de partitie per SKU, uitgebreid door het aantal partities vereist voor het opslaan van een index van deze grootte ervan uitgaande dat u de geschatte grootte van de index kunt schatten, vergelijken.
4. **Wat is de verwachte querybelasting**? U kunt query werkbelastingen zodra opslagvereisten worden begrepen. S2 en beide S3-SKU's bieden bijna-equivalente doorvoer, maar SLA vereisten zal sprake is van een preview-SKU's.
5. Als u de categorie S2 of S3 overweegt, moet u bepalen of u nodig [indexeerfuncties](search-indexer-overview.md). Indexeerfuncties zijn nog niet beschikbaar voor de laag S3 HD. Alternatieve methode is het gebruik van een pushmodel voor index-updates, waar het schrijven van code van de toepassing naar een gegevensset naar een index te pushen.

De meeste klanten kunnen een specifieke SKU in- of regel op basis van hun antwoorden op de bovenstaande vragen. Als u niet zeker weet welke SKU te gaan met, kunt u vragen stellen op MSDN of StackOverflow forums, of neem contact op met de Azure-ondersteuning voor verdere informatie.

## <a name="decision-validation-does-the-sku-offer-sufficient-storage-and-qps"></a>Besluit validatie: de SKU biedt voldoende opslagruimte en QPS?
Als laatste stap terugkeren naar de [pagina met prijzen](https://azure.microsoft.com/pricing/details/search/) en de [per-service en per index secties in Servicelimieten](search-limits-quotas-capacity.md) om te controleren van uw maakt een schatting op basis van limieten voor het abonnement en de service.

Als de prijs of de opslag vereisten buiten het bereik zijn, wilt u mogelijk opsplitsen van de werkbelasting tussen meerdere kleinere services (bijvoorbeeld). U kunt op een meer gedetailleerd niveau herontwerp indexen kleiner of filters gebruiken om te efficiënter query's.

> [!NOTE]
> Vereisten voor opslag kunnen worden te veel hoge als documenten overbodige gegevens bevatten. In het ideale geval bevatten documenten alleen doorzoekbare gegevens of metagegevens. Binaire gegevens is niet-doorzoekbare en moet worden opgeslagen afzonderlijk (mogelijk in een Azure table of blob-opslag) met een veld in de index van een URL-verwijzing naar de externe gegevens bevatten. De maximale grootte van een afzonderlijk document 16 MB is (of minder als u meerdere documenten in één aanvraag uploaden bulksgewijs zijn). Zie [Servicelimieten in Azure Search](search-limits-quotas-capacity.md) voor meer informatie.
>
>

## <a name="next-step"></a>Volgende stap
Als u welk SKU is de juiste keuze te maken weet, kunt u verder met deze stappen:

* [Maken van een service voor zoeken in de portal](search-create-service-portal.md)
* [Wijzig de toewijzing van partities en replica's om uw service te schalen](search-capacity-planning.md)
