---
title: Schaal partities en replica's voor query's en indexering - Azure zoeken
description: Partitie en de replica computerbronnen in Azure Search, waarbij elke resource prijs is afhankelijk van in factureerbare search-eenheden aanpassen.
author: HeidiSteen
manager: cgronlun
services: search
ms.service: search
ms.topic: conceptual
ms.date: 03/22/2019
ms.author: heidist
ms.custom: seodec2018
ms.openlocfilehash: 6879dd975f97ba2746165e87a135e5d90e8b229f
ms.sourcegitcommit: f8c592ebaad4a5fc45710dadc0e5c4480d122d6f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58620629"
---
# <a name="scale-partitions-and-replicas-for-query-and-indexing-workloads-in-azure-search"></a>De schaal van partities en replica's voor query's en indexering van workloads in Azure Search
Nadat u [Kies een prijscategorie](search-sku-tier.md) en [inrichten van een service voor zoeken](search-create-service-portal.md), de volgende stap is het eventueel verhogen van het aantal replica's of partities die worden gebruikt door uw service. Elke laag biedt een vast aantal factureringseenheden. In dit artikel wordt uitgelegd hoe u de eenheden voor het bereiken van een optimale configuratie die een van uw vereisten voor het uitvoeren van query's, indexering en opslag toewijzen.

Resourceconfiguratie is beschikbaar bij het instellen van een service op de [Basic-laag](https://aka.ms/azuresearchbasic) of een van de [Standard- of met geoptimaliseerde opslag lagen](search-limits-quotas-capacity.md). Voor services op deze lagen, capaciteit is aangeschaft, in stappen van *eenheden zoeken* (SUs) waar elke partitie en de replica telt als één SU. 

Gebruik minder SUs resulteert in een proportioneel lagere factuur. Facturering is in feite voor zolang de service is ingesteld. Als u een service tijdelijk niet gebruikt, is de enige manier om facturering te voorkomen dat door de service verwijderen en vervolgens opnieuw te maken wanneer u ze nodig hebt.

> [!Note]
> Verwijderen van een service, worden alle gegevens verwijderd. Er is geen functie in Azure Search voor back-ups maken en herstellen van opgeslagen zoekgegevens. Als u wilt implementeren op een bestaande index op een nieuwe service, moet u het programma dat wordt gebruikt voor het maken en deze oorspronkelijk laden uitvoeren. 

## <a name="terminology-replicas-and-partitions"></a>Terminologie: de replica's en partities
Replica's en partities worden de primaire bronnen die back-ups een search-service maken.

| Resource | Definitie |
|----------|------------|
|*Partities* | Biedt indexopslag en i/o voor bewerkingen voor lezen/schrijven (bijvoorbeeld bij het opnieuw samenstellen of vernieuwen van een index).|
|*Replicas* | Exemplaren van de search-service gebruikt voor het laden saldo querybewerkingen. Elke replica altijd fungeert als host voor één exemplaar van een index. Als u 12 replica's hebt, beschikt u over 12 kopieën van elke index geladen op de service.|

> [!NOTE]
> Er is geen manier om rechtstreeks bewerken of te beheren welke indexen worden uitgevoerd op een replica. Een exemplaar van elke index op elke replica maakt deel uit van de servicearchitectuur.
>


## <a name="how-to-allocate-replicas-and-partitions"></a>Toewijzen van replica's en partities
Een service in Azure Search is in eerste instantie een minimumniveau aan resources die bestaat uit één partitie en een van de replica toegewezen. Voor de lagen die ondersteuning bieden voor deze kunt u stapsgewijs rekenbronnen aanpassen door partities verhogen als u meer opslag en i/o nodig, of meer replica's voor grotere volumes van query's of betere prestaties toevoegen. Een enkele service moet voldoende bronnen zijn voor het verwerken van alle werkbelastingen (indexeren en query's) hebben. U kunt workloads tussen meerdere services kan niet onderverdelen.

Als u wilt vergroten of wijzigen van de toewijzing van replica's en partities, wordt u aangeraden de Azure-portal. De portal wordt afgedwongen beperkingen met betrekking tot de toegestane combinaties die onder maximumlimieten blijven. Als u een op basis van een script of op code gebaseerde inrichting benadering, vereist de [Azure PowerShell](search-manage-powershell.md) of de [Management REST API](https://docs.microsoft.com/rest/api/searchmanagement/services) alternatieve oplossingen zijn.

Zoektoepassingen over het algemeen moeten meer replica's dan partities bevatten, met name wanneer de service-bewerkingen zijn gericht op de querywerkbelastingen. De sectie over [hoge beschikbaarheid](#HA) wordt uitgelegd waarom.

1. Aanmelden bij de [Azure-portal](https://portal.azure.com/) en selecteert u de search-service.
2. In **instellingen**, open de **schaal** pagina om replica's en partities te wijzigen. 

   De volgende schermafbeelding ziet u een standard-service voorzien van een replica en partitie. De formule aan de onderkant geeft aan hoeveel search-eenheden worden gebruikt (1). Als de prijs per eenheid is $100 (niet een echte prijs), zou de maandelijkse kosten van het uitvoeren van deze service gemiddeld $100 zijn.

   ![Pagina voor schalen van de huidige waarden](media/search-capacity-planning/1-initial-values.png "pagina voor schalen van de huidige waarden")

3. Gebruik de schuifregelaar om te vergroten of verkleinen van het aantal partities. De formule aan de onderkant geeft aan hoeveel search-eenheden worden gebruikt.

   In dit voorbeeld verdubbeld capaciteit, met twee replica's en alle partities. U ziet het aantal eenheden van search; het is nu vier omdat de facturering formule replica's vermenigvuldigd met partities (2 x 2). Verdubbeling van de capaciteit van verdubbeld de kosten van het uitvoeren van de service meer dan. Als de kostprijs zoeken $100 is, is de nieuwe maandelijkse factuur zou nu $400.

   Voor de huidige per eenheid kosten van elke laag, gaat u naar de [pagina met prijzen](https://azure.microsoft.com/pricing/details/search/).

   ![Toevoegen van replica's en partities](media/search-capacity-planning/2-add-2-each.png "replica's en partities toevoegen")

3. Klik op **opslaan** om de wijzigingen te bevestigen.

   ![Controleer of wijzigingen in de schaal en facturering](media/search-capacity-planning/3-save-confirm.png "wijzigingen te schalen en facturering bevestigen")

   Wijzigingen in de capaciteit van duren enkele uren. U annuleren niet nadat het proces is gestart en er is geen realtime-controle voor de replica en partitie aanpassingen. Het volgende bericht blijft zichtbaar wanneer wijzigingen uitgevoerd worden.

   ![Statusbericht in de portal](media/search-capacity-planning/4-updating.png "statusbericht in de portal")


> [!NOTE]
> Nadat een service is ingericht, kan deze worden bijgewerkt naar een hogere SKU. U moet een search-service op de nieuwe laag maken en uw indexen opnieuw laden. Zie [maken van een Azure Search-service in de portal](search-create-service-portal.md) voor hulp bij het inrichten van de service.
>
>

<a id="chart"></a>

## <a name="partition-and-replica-combinations"></a>Combinaties van partitie en replica

Een eenvoudige service kan exact één partitie en maximaal drie replica's, voor een maximale beperken van drie su's. De alleen aanpasbare resource heeft replica's. U moet er minimaal twee replica's voor hoge beschikbaarheid op query's.

Alle Standard- en opslag geoptimaliseerd search-services kunnen ervan uitgaan dat de volgende combinaties van replica's en partities, afhankelijk van de 36 SU-limiet. 

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


<a id="HA"></a>

## <a name="high-availability"></a>Hoge beschikbaarheid
Omdat het is eenvoudig en relatief snel om omhoog te schalen, in het algemeen wordt aangeraden dat u met één partitie en één begint of twee replica's, en klikt u vervolgens omhoog schalen als query volumes maken. Querywerkbelastingen hoofdzakelijk wordt uitgevoerd op replica's. Als u meer doorvoer of hoge beschikbaarheid nodig hebt, moet u waarschijnlijk extra replica's.

Algemene aanbevelingen voor hoge beschikbaarheid zijn:

* Twee replica's voor hoge beschikbaarheid van workloads voor alleen-lezen (query's)
* Drie of meer replica's voor hoge beschikbaarheid voor lezen/schrijven-werkbelastingen (query's plus indexeren als afzonderlijke documenten worden toegevoegd, bijgewerkt of verwijderd)

Service level agreements (SLA) voor Azure Search is gericht op querybewerkingen en op index-updates die bestaan uit het toevoegen, bijwerken of verwijderen van documenten.

Basic-laag boven uit op één partitie en drie replica's. Als u wilt dat de flexibiliteit om te kunnen onmiddellijk reageren op fluctuaties in de vraag voor indexeren en query-doorvoer, kunt u overwegen een van de standaardlagen.  Als u uw vereisten groeien veel sneller dan de querydoorvoer van uw hebt gevonden, kunt u overwegen een van de lagen met geoptimaliseerde opslag.

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


## <a name="next-steps"></a>Volgende stappen

[Kies een prijscategorie voor Azure Search](search-sku-tier.md)
