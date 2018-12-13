---
title: Partities en replica's toewijzen voor de query en de indexing - Azure Search
description: Partitie en de replica computerbronnen in Azure Search, waarbij elke resource prijs is afhankelijk van in factureerbare search-eenheden aanpassen.
author: HeidiSteen
manager: cgronlun
services: search
ms.service: search
ms.topic: conceptual
ms.date: 11/09/2017
ms.author: heidist
ms.custom: seodec2018
ms.openlocfilehash: e2eff6c854dae48961700341a6db19dc7113901c
ms.sourcegitcommit: eb9dd01614b8e95ebc06139c72fa563b25dc6d13
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/12/2018
ms.locfileid: "53316111"
---
# <a name="allocate-partitions-and-replicas-for-query-and-indexing-workloads-in-azure-search"></a>Partities en replica's toewijzen voor query's en indexering van workloads in Azure Search
Nadat u [Kies een prijscategorie](search-sku-tier.md) en [inrichten van een service voor zoeken](search-create-service-portal.md), de volgende stap is het eventueel verhogen van het aantal replica's of partities die worden gebruikt door uw service. Elke laag biedt een vast aantal factureringseenheden. In dit artikel wordt uitgelegd hoe u de eenheden voor het bereiken van een optimale configuratie die een van uw vereisten voor het uitvoeren van query's, indexering en opslag toewijzen.

Resourceconfiguratie is beschikbaar bij het instellen van een service op de [Basic-laag](https://aka.ms/azuresearchbasic) of een van de [standaardlagen](search-limits-quotas-capacity.md). Voor services op deze lagen, capaciteit is aangeschaft, in stappen van *eenheden zoeken* (SUs) waar elke partitie en de replica telt als één SU. 

Gebruik minder SUs resulteert in een proportioneel lagere factuur. Facturering is in feite voor zolang de service is ingesteld. Als u een service tijdelijk niet gebruikt, is de enige manier om facturering te voorkomen dat door de service verwijderen en vervolgens opnieuw te maken wanneer u ze nodig hebt.

> [!Note]
> Verwijderen van een service, worden alle gegevens verwijderd. Er is geen functie in Azure Search voor back-ups maken en herstellen van opgeslagen zoekgegevens. Als u wilt implementeren op een bestaande index op een nieuwe service, moet u het programma dat wordt gebruikt voor het maken en deze oorspronkelijk laden uitvoeren. 

## <a name="terminology-partitions-and-replicas"></a>Terminologie: partities en replica 's
Partities en replica's zijn de primaire bronnen die back-ups een search-service maken.

| Resource | Definitie |
|----------|------------|
|*Partities* | Biedt indexopslag en i/o voor bewerkingen voor lezen/schrijven (bijvoorbeeld bij het opnieuw samenstellen of vernieuwen van een index).|
|*Replica 's* | Exemplaren van de search-service gebruikt voor het laden saldo querybewerkingen. Elke replica altijd fungeert als host voor één exemplaar van een index. Als u 12 replica's hebt, beschikt u over 12 kopieën van elke index geladen op de service.|

> [!NOTE]
> Er is geen manier om rechtstreeks bewerken of te beheren welke indexen worden uitgevoerd op een replica. Een exemplaar van elke index op elke replica maakt deel uit van de servicearchitectuur.
>

## <a name="how-to-allocate-partitions-and-replicas"></a>Partities en replica's toewijzen
Een service in Azure Search is in eerste instantie een minimumniveau aan resources die bestaat uit één partitie en een van de replica toegewezen. Voor de lagen die ondersteuning bieden voor deze kunt u stapsgewijs rekenbronnen aanpassen door partities verhogen als u meer opslag en i/o nodig, of meer replica's voor grotere volumes van query's of betere prestaties toevoegen. Een enkele service moet voldoende bronnen zijn voor het verwerken van alle werkbelastingen (indexeren en query's) hebben. U kunt workloads tussen meerdere services kan niet onderverdelen.

Als u wilt vergroten of wijzigen van de toewijzing van replica's en partities, wordt u aangeraden de Azure-portal. De portal wordt afgedwongen beperkingen met betrekking tot de toegestane combinaties die onder maximumlimieten blijven:

1. Aanmelden bij de [Azure-portal](https://portal.azure.com/) en selecteert u de search-service.
2. In **instellingen**, open de **schaal** blade en gebruik de schuifregelaars om vergroten of verkleinen van het aantal partities en replica's.

Als u een op basis van een script of op code gebaseerde inrichting benadering, vereist de [Management REST API](https://docs.microsoft.com/rest/api/searchmanagement/services) is een alternatief voor de portal.

Zoektoepassingen over het algemeen moeten meer replica's dan partities bevatten, met name wanneer de service-bewerkingen zijn gericht op de querywerkbelastingen. De sectie over [hoge beschikbaarheid](#HA) wordt uitgelegd waarom.

> [!NOTE]
> Nadat een service is ingericht, kan deze worden bijgewerkt naar een hogere SKU. U moet een search-service op de nieuwe laag maken en uw indexen opnieuw laden. Zie [maken van een Azure Search-service in de portal](search-create-service-portal.md) voor hulp bij het inrichten van de service.
>
>

<a id="HA"></a>

## <a name="high-availability"></a>Hoge beschikbaarheid
Omdat het is eenvoudig en relatief snel om omhoog te schalen, in het algemeen wordt aangeraden dat u met één partitie en één begint of twee replica's, en klikt u vervolgens omhoog schalen als query volumes maken. Querywerkbelastingen hoofdzakelijk wordt uitgevoerd op replica's. Als u meer doorvoer of hoge beschikbaarheid nodig hebt, moet u waarschijnlijk extra replica's.

Algemene aanbevelingen voor hoge beschikbaarheid zijn:

* Twee replica's voor hoge beschikbaarheid van workloads voor alleen-lezen (query's)
* Drie of meer replica's voor hoge beschikbaarheid voor lezen/schrijven-werkbelastingen (query's plus indexeren als afzonderlijke documenten worden toegevoegd, bijgewerkt of verwijderd)

Service level agreements (SLA) voor Azure Search is gericht op querybewerkingen en op index-updates die bestaan uit het toevoegen, bijwerken of verwijderen van documenten.

Basic-laag boven uit op één partitie en drie replica's. Als u wilt dat de flexibiliteit om te kunnen onmiddellijk reageren op fluctuaties in de vraag voor indexeren en query-doorvoer, kunt u overwegen een van de standaardlagen.

### <a name="index-availability-during-a-rebuild"></a>Beschikbaarheid van indexen tijdens het opnieuw opbouwen

Hoge beschikbaarheid voor Azure Search geldt voor query's en index-updates die niet opnieuw opbouwen van een index. Als u een veld verwijderen, een gegevenstype wijzigen of wijzig de naam van een veld, moet u de index opnieuw opbouwen. De index opnieuw kunt bouwen, moet u de index verwijderen, maak de index en de gegevens opnieuw laden.

> [!NOTE]
> U kunt nieuwe velden toevoegen aan een Azure Search-index zonder het opnieuw opbouwen van de index. De waarde van het nieuwe veld wordt niet null zijn voor alle documenten die al in de index.

Als u wilt behouden index beschikbaarheid tijdens het opbouwen, moet u een kopie van de index met een andere naam in dezelfde service of een kopie van de index met dezelfde naam op een andere service en geef vervolgens omleiding of failover-logica in uw code.

## <a name="disaster-recovery"></a>Herstel na noodgeval
Er is momenteel geen ingebouwd mechanisme voor herstel na noodgevallen. Toe te voegen partities of replica's is de verkeerde strategie voor disaster recovery doelstellingen. De meest voorkomende aanpak is het toevoegen van redundantie op het serviceniveau van de door het instellen van een tweede service voor zoeken in een andere regio. Net als bij beschikbaarheid tijdens het opnieuw opbouwen van een index, de omleiding of failoverlogica moet afkomstig zijn van uw code.

## <a name="increase-query-performance-with-replicas"></a>Prestaties van query's met replica's vergroten
Latentie van query is een indicator dat extra replica's nodig zijn. Over het algemeen is een eerste stap bij het verbeteren van de prestaties van query's om toe te voegen meer van deze resource. Als u replica's toevoegt, extra exemplaren van de index online worden gebracht om grotere query-workloads te ondersteunen en te laden saldo van de aanvragen via het meerdere replica's.

We kunnen niet harde schattingen op te leveren op query's per seconde (QPS): query prestaties, is afhankelijk van de complexiteit van de query en concurrerende workloads. Hoewel het toevoegen van replica's duidelijk resulteert in betere prestaties, het resultaat is niet strikt linear: drie replica's toe te voegen is geen garantie voor drie opeenvolgende doorvoer.

Zie voor richtlijnen bij het schatten van de QPS voor uw workloads, [overwegingen voor de prestaties en optimalisatie van de Azure Search](search-performance-optimization.md).

## <a name="increase-indexing-performance-with-partitions"></a>Indexering verbeteren met partities
Zoektoepassingen waarvoor in de buurt van real-time gegevens vernieuwen moet proportioneel meer partities dan replica's. Bewerkingen voor lezen/schrijven toe te voegen partities worden verspreidt over een groter aantal compute-resources. Het biedt ook meer schijfruimte voor het opslaan van aanvullende indexen en documenten.

Grotere indexen langer duren voordat de query. Daarom kan het gebeuren dat een kleiner, maar in verhouding toename van de replica's voor elke incrementele toename in partities is vereist. In hoe snel uitvoeren van query's is ingeschakeld om rekening met de complexiteit van uw query's en queryvolume.

## <a name="basic-tier-partition-and-replica-combinations"></a>Basic-laag: Combinaties van partitie en replica
Een eenvoudige service kan exact één partitie en maximaal drie replica's, voor een maximale beperken van drie su's. De alleen aanpasbare resource heeft replica's. U moet er minimaal twee replica's voor hoge beschikbaarheid op query's.

<a id="chart"></a>

## <a name="standard-tiers-partition-and-replica-combinations"></a>Standaard-laag: Combinaties van partitie en replica
Deze tabel bevat de su's vereist zijn ter ondersteuning van combinaties van replica's en partities, afhankelijk van de limiet 36 SU voor alle Standard-lagen.

|   | **1 partitie** | **2 partities** | **3 partities** | **4-partities** | **6-partities** | **12 partities** |
| --- | --- | --- | --- | --- | --- | --- |
| **1 replica** |1 SU |2 SU |3 SU |4 SU |6 SU |12 SU |
| **2 replica 's** |2 SU |4 SU |6 SU |8 SU |12 SU |24 SU |
| **3 replica 's** |3 SU |6 SU |9 SU |12 SU |18 SU |36 SU |
| **4 replica 's** |4 SU |8 SU |12 SU |16 SU |24 SU |N/A |
| **5 replica 's** |5 SU |10 SU |15 SU |20 SU |30 SU |N/A |
| **6 replica 's** |6 SU |12 SU |18 SU |24 SU |36 SU |N/A |
| **12 replica 's** |12 SU |24 SU |36 SU |N/A |N/A |N/A |

SUs, prijzen en -capaciteit worden in detail uitgelegd in de Azure-website. Zie voor meer informatie, [prijsinformatie](https://azure.microsoft.com/pricing/details/search/).

> [!NOTE]
> Het aantal replica's en partities gelijkmatig verdeelt in 12 (met name 1, 2, 3, 4, 6, 12). Dit komt doordat Azure Search vooraf elke index in 12 shards verdeelt zodat deze kan in gelijke delen worden verdeeld over alle partities. Bijvoorbeeld, als uw service drie partities heeft en u een index maakt, bevat elke partitie vier shards van de index. Hoe Azure Search shards een index is een implementatiedetail, wordt kan worden gewijzigd in toekomstige releases. Hoewel het getal 12 vandaag is, kunt u dit getal altijd in de toekomst worden 12 mag niet verwacht.
>
>

## <a name="billing-formula-for-replica-and-partition-resources"></a>Facturering-formule voor de replica en partitie resources
De formule voor het berekenen van hoeveel su's worden gebruikt voor een specifieke combinatie ervan is het product van replica's en partities, of (X-P R = SU). Bijvoorbeeld drie replica's vermenigvuldigd met drie partities wordt gefactureerd als negen su's.

Kosten per SU wordt bepaald door de categorie, met een lagere per eenheid-tarief voor Basic dan voor Standard. Tarieven voor elke laag kan worden gevonden op [prijsinformatie](https://azure.microsoft.com/pricing/details/search/).
