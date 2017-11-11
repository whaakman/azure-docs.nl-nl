---
title: Capaciteitsplanning voor Azure Search | Microsoft Docs
description: Partitie en replica computerbronnen in Azure Search, waarbij een prijs van elke resource in factureerbare search-eenheden aanpassen.
services: search
documentationcenter: 
author: HeidiSteen
manager: jhubbard
editor: 
tags: azure-portal
ms.assetid: 1dc16afe-56f9-439d-8874-1733ae1a2b74
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 11/09/2017
ms.author: heidist
ms.openlocfilehash: 47dcd5366ef8ba3d4598e6d418b11997c61bddea
ms.sourcegitcommit: bc8d39fa83b3c4a66457fba007d215bccd8be985
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/10/2017
---
# <a name="scale-resource-levels-for-query-and-indexing-workloads-in-azure-search"></a>Resource schaalniveaus voor query's en workloads in Azure Search indexeren
Nadat u [Kies een prijscategorie](search-sku-tier.md) en [inrichten van een zoekservice](search-create-service-portal.md), de volgende stap is het eventueel verhogen van het aantal replica's of partities die worden gebruikt door uw service. Elke laag biedt een vast aantal facturering eenheden. Dit artikel wordt uitgelegd hoe deze eenheden om te zorgen voor een optimale configuratie die door een compromis uw vereisten voor het uitvoeren van de query tussen, indexeren en de opslag toewijzen.

Resourceconfiguratie is beschikbaar bij het instellen van een service op de [basisstaffel](http://aka.ms/azuresearchbasic) of een van de [standaard lagen](search-limits-quotas-capacity.md). Voor services op deze lagen, capaciteit is aangeschaft per *zoekeenheden* (SUs) waar elke partitie en replica telt als een SU. 

Gebruik minder SUs resulteert in een proportioneel lagere factuur. Facturering geldt voor als de service is ingesteld. Als u een service tijdelijk niet gebruikt, wordt de enige manier om te voorkomen dat facturering door de service verwijdert en vervolgens opnieuw maken wanneer dat nodig is.

> [!Note]
> Verwijderen van een service, worden alle gegevens verwijderd. Er is geen faciliteit in Azure Search voor back-up en herstellen van zoekgegevens persistent. Als u wilt implementeren op een bestaande index op een nieuwe service, moet u het programma dat wordt gebruikt voor het maken en oorspronkelijk laden uitvoeren. 

## <a name="terminology-partitions-and-replicas"></a>Terminologie: partities en replica 's
Partities en replica's zijn de primaire resources die back-search-service.

| Resource | Definitie |
|----------|------------|
|*Partities* | Voorziet index opslag en i/o-lees-/ schrijfbewerkingen (bijvoorbeeld wanneer opnieuw samenstellen of een index te vernieuwen).|
|*Replica 's* | Exemplaren van de search-service gebruikt voor het saldo querybewerkingen te laden. Elke replica altijd fungeert als host voor één exemplaar van een index. Als u 12 replica's hebt, hebt u 12 kopieën van elke index op de service geladen.|

> [!NOTE]
> Er is geen manier voor het rechtstreeks bewerken of beheren welke indexen worden uitgevoerd op een replica. Een exemplaar van elke index op elke replica maakt deel uit van de service-architectuur.
>

## <a name="how-to-allocate-partitions-and-replicas"></a>Hoe partities en replica's toewijzen
Een service in Azure Search is in eerste instantie een minimumniveau aan resources die bestaan uit één partitie en één replica toegewezen. Voor de lagen die dit ondersteunen, kunt u stapsgewijs rekenbronnen aanpassen door partities verhogen als u meer opslagruimte en i/o-nodig, of Voeg meer replica's van grotere volumes van de query of betere prestaties. Een enkele service moet voldoende bronnen zijn voor het verwerken van alle werkbelastingen (indexeren en query's) hebben. U kunt werkbelastingen tussen meerdere services kan geen onderverdelen.

Om te vergroten of wijzig de toewijzing van replica's en partities, wordt u aangeraden de Azure-portal. De portal wordt afgedwongen beperkingen met betrekking tot toegestane combinaties die onder bovengrenzen:

1. Aanmelden bij de [Azure-portal](https://portal.azure.com/) en selecteert u de search-service.
2. In **instellingen**, open de **Scale** blade en gebruik de schuifregelaars om vergroten of verkleinen het aantal partities en replica's.

Als u een script of code gebaseerde inrichting benadering, vereist de [Management REST API](https://docs.microsoft.com/rest/api/searchmanagement/services) vormt een alternatief voor de portal.

Meestal moeten zoektoepassingen meer replica's dan partities, met name wanneer de servicebewerkingen zijn gericht op de query-werkbelastingen. Het gedeelte [hoge beschikbaarheid](#HA) wordt uitgelegd waarom.

> [!NOTE]
> Nadat u een service is geconfigureerd, kan deze worden bijgewerkt naar een hogere SKU. U moet een search-service maken op de nieuwe laag en de indexen opnieuw laden. Zie [maken van een Azure Search-service in de portal](search-create-service-portal.md) voor hulp bij het inrichten van de service.
>
>

<a id="HA"></a>

## <a name="high-availability"></a>Hoge beschikbaarheid
Omdat het opschalen relatief snel en gemakkelijk, in het algemeen wordt aangeraden dat u met één partitie en één begint of twee replica's en schaal omhoog als volumes query bouwen. Query-workloads uitvoeren voornamelijk op de replica's. Als u meer doorvoer of hoge beschikbaarheid nodig hebt, moet u waarschijnlijk meer replica's.

Algemene aanbevelingen voor hoge beschikbaarheid zijn:

* Twee replica's voor hoge beschikbaarheid van alleen-lezen werkbelastingen (query's)
* Drie of meer replica's voor hoge beschikbaarheid van lezen/schrijven-werkbelastingen (query's plus indexeren als afzonderlijke documenten worden toegevoegd, bijgewerkt of verwijderd)

Serviceovereenkomsten (SLA) voor Azure Search is gericht op querybewerkingen en op index-updates die bestaan uit toevoegen, bijwerken of verwijderen van documenten.

Basic-laag boven uit op één partitie en drie replica's. Als u de flexibiliteit om onmiddellijk reageren op fluctuaties in vraag voor doorvoer voor indexering en query wilt, kunt u een van de lagen Standard.

### <a name="index-availability-during-a-rebuild"></a>Index beschikbaarheid tijdens het opnieuw opbouwen

Hoge beschikbaarheid voor Azure Search geldt voor query's en index-updates die niet opnieuw opbouwen van een index. Als u een veld verwijderen, wijzigen van een type of wijzig de naam van een veld, moet u de index opnieuw maken. Als u wilt de index opnieuw worden opgebouwd, moet u de index verwijderen, maak de index opnieuw en de gegevens opnieuw laden.

> [!NOTE]
> U kunt nieuwe velden toevoegen aan een Azure Search-index zonder het opnieuw opbouwen van de index. De waarde van het nieuwe veld is null voor alle documenten al in de index.

Om te blijven index beschikbaarheid tijdens het opnieuw opbouwen, moet u een kopie van de index met een andere naam op dezelfde service of een kopie van de index met dezelfde naam op een andere service hebben, en geef vervolgens omleiding of failoverlogica in uw code.

## <a name="disaster-recovery"></a>Herstel na noodgeval
Er is momenteel geen ingebouwde methode voor herstel na noodgevallen. Toe te voegen partities of replica's is de verkeerde strategie voor disaster recovery doelstellingen. De meest voorkomende aanpak is het toevoegen van redundantie op het serviceniveau van de door het instellen van een tweede service voor zoeken in een andere regio. Net als bij beschikbaarheid tijdens het opnieuw opbouwen van een index, de omleiding of failoverlogica moet afkomstig zijn van uw code.

## <a name="increase-query-performance-with-replicas"></a>Prestaties van query's met replica's vergroten
Latentie van query is een indicatie dat de replica's meer nodig zijn. In het algemeen is de eerste stap bij het verbeteren van de prestaties van query's meer van deze resource toevoegen. Als u replica's toevoegt, extra exemplaren van de index online gezet ter ondersteuning van grotere query werkbelastingen en laden van een balans vinden tussen de aanvragen via de meerdere replica's.

Kan geen wij harde maakt een schatting op query's per seconde (QPS): query prestaties, is afhankelijk van de complexiteit van de query en concurrerende werkbelastingen. Hoewel het toevoegen van replica's duidelijk resulteert in betere prestaties, het resultaat is niet strikt lineaire: drie doorvoer wordt niet gegarandeerd drie replica's toe te voegen.

Zie voor instructies in het schatten van QPS voor uw werkbelastingen [aandachtspunten voor prestaties en optimalisatie van Azure Search](search-performance-optimization.md).

## <a name="increase-indexing-performance-with-partitions"></a>De prestaties van indexering met partities
Zoektoepassingen waarvoor near realtime gegevens vernieuwen moet proportioneel meer partities dan replica's. Lees-/ schrijfbewerkingen toe te voegen partities worden verspreid over een groter aantal rekenresources. Dit ook biedt u meer schijfruimte voor het opslaan van aanvullende indexen en documenten.

Grotere indexen duren aan query langer. Als zodanig kan gebeuren dat elke incrementele stijging van de partities een kleinere maar evenredig toename van replica's vereist. De complexiteit van uw query's en het volume van de query wordt factor in hoe snel de queryuitvoering rond is ingeschakeld.

## <a name="basic-tier-partition-and-replica-combinations"></a>Basic-laag: combinaties van partitie en replica
Een Basic service kunt precies één partitie hebben en maximaal drie replica's voor een maximum beperken van drie SUs. De resource alleen aanpasbare is replica's. U moet minimaal twee replica's voor hoge beschikbaarheid op query's.

<a id="chart"></a>

## <a name="standard-tiers-partition-and-replica-combinations"></a>Standaard lagen: combinaties van partitie en replica
Deze tabel ziet de SUs vereist ter ondersteuning van combinaties van replica's en partities onderworpen aan de limiet 36 SU voor alle standaard lagen.

|   | **1 partitie** | **2 partities** | **3 partities** | **4 partities** | **6 partities** | **12 partities** |
| --- | --- | --- | --- | --- | --- | --- |
| **1 replica** |1 SU |2 SU |3 SU |4 SU |6 SU |12 SU |
| **2 replica 's** |2 SU |4 SU |6 SU |8 SU |12 SU |24 SU |
| **3 replica 's** |3 SU |6 SU |9 SU |12 SU |18 SU |36 SU |
| **4 replica 's** |4 SU |8 SU |12 SU |16 SU |24 SU |N.v.t. |
| **5 replica 's** |5 SU |10 SU |15 SU |20 SU |30 SU |N.v.t. |
| **6 replica 's** |6 SU |12 SU |18 SU |24 SU |36 SU |N.v.t. |
| **12 replica 's** |12 SU |24 SU |36 SU |N.v.t. |N.v.t. |N.v.t. |

SUs-prijzen en capaciteit zijn in detail uitgelegd op de Azure-website. Zie voor meer informatie [prijsinformatie](https://azure.microsoft.com/pricing/details/search/).

> [!NOTE]
> Het aantal replica's en partities gelijkmatig wordt verdeeld in 12 (specifiek, 1, 2, 3, 4, 6, 12). Dit is omdat elke index Azure Search vooraf in 12 shards verdeelt zodat deze in gedeelten gelijk voor alle partities kan worden verspreid. Als uw service drie partities heeft en u een index maakt, wordt elke partitie vier shards van de index bevatten. Hoe Azure Search shards een index is een implementatiedetail, wordt kan worden gewijzigd in toekomstige releases. Hoewel het getal 12 vandaag de dag is, kunt u dat dit aantal altijd in de toekomst worden 12 mag niet verwacht.
>
>

## <a name="billing-formula-for-replica-and-partition-resources"></a>De formule facturering voor replica- en partitie bronnen
De formule voor het berekenen van hoeveel SUs worden gebruikt voor specifieke combinaties is het product van replica's en -partities bevat, of (R X P = SU). Bijvoorbeeld drie replica's vermenigvuldigd met drie partities wordt in rekening gebracht als negen SUs.

Kosten per SU wordt bepaald door de laag, met een lagere per eenheid facturerings frequentie voor Basic dan voor standaard. Voor elke laag kan worden gevonden op classificeert [prijsinformatie](https://azure.microsoft.com/pricing/details/search/).
