---
title: Kies een prijscategorie laag of SKU voor Azure Search-service | Microsoft Docs
description: "Azure Search kan worden ingericht op deze SKU's: gratis, basis en standaard, waarbij standaard is beschikbaar in verschillende configuraties van resources en capaciteit niveaus."
services: search
author: HeidiSteen
manager: cgronlun
tags: azure-portal
ms.service: search
ms.topic: conceptual
ms.date: 05/12/2018
ms.author: heidist
ms.openlocfilehash: bbf535c5b446fd654331374d29c106b6e43d55f5
ms.sourcegitcommit: 96089449d17548263691d40e4f1e8f9557561197
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/17/2018
ms.locfileid: "34266877"
---
# <a name="choose-a-pricing-tier-for-azure-search"></a>Kies een prijscategorie voor Azure Search

In Azure Search een [service is ingericht](search-create-service-portal.md) op een specifieke prijscategorie of SKU. Opties zijn onder andere **vrije**, **Basic**, of **standaard**, waarbij **standaard** is beschikbaar in meerdere configuraties en capaciteit. 

Het doel van dit artikel is bij het kiezen van een laag. Er is een aanvulling op de [pagina met prijzen](https://azure.microsoft.com/pricing/details/search/) en [Servicelimieten](search-limits-quotas-capacity.md) pagina met een overzicht van facturering concepten en verbruik patronen die zijn gekoppeld aan verschillende lagen. Ook raadt u aan een herhaalbare aanpak om te begrijpen welke laag beste aan uw behoeften voldoet. 

Lagen bepalen capaciteit, niet-functies. Als een laag capaciteit blijkt te laag, moet u voor het inrichten van een nieuwe service op de hogere lagen en vervolgens [opnieuw laden van de indexen](search-howto-reindex.md). Er is geen in-place upgrade van dezelfde service van een SKU naar een andere.

Beschikbaarheid van functies is geen overweging primaire laag. Alle lagen, waaronder de **vrije** servicetier, functie pariteit, met uitzondering van de indexeerfunctie ondersteuning voor S3HD bieden. Echter beperkingen voor indexering en resource effectief kunnen worden beperkt de omvang van het gebruik van functies. Bijvoorbeeld: [cognitieve zoeken](cognitive-search-concept-intro.md) indexeren heeft langlopende vaardigheden die time-out voor een gratis service tenzij de gegevensset gebeurt erg klein.

> [!TIP]
> De meeste klanten beginnen met de **vrije** servicetier voor evaluatie en vervolgens naar studeren **standaard** voor ontwikkeling. Nadat u een laag kiezen en [inrichten van een zoekservice](search-create-service-portal.md), kunt u [verhogen van de replica en partitie aantallen](search-capacity-planning.md) voor prestaties afstemmen. Zie voor meer informatie over wanneer en waarom zou u capaciteit aanpassen [aandachtspunten voor prestaties en optimalisatie](search-performance-optimization.md).
>

## <a name="billing-concepts"></a>Concepten van facturering

Concepten waarover die u moet begrijpen voor het selecteren van een bevatten definities van capaciteit, Servicelimieten en service-eenheden. 

### <a name="capacity"></a>Capaciteit

Capaciteit is gestructureerd als *replica's* en *partities*. Replica's zijn exemplaren van de search-service, waarbij elke replica fungeert als host voor één taakverdeling exemplaar van een index. Zo heeft een service met 6 replica's 6 kopieën van elke index geladen in de service. Partities opslaan indexen en automatisch doorzoekbare gegevens splitsen: twee partities uw index in de helft, ziet u drie partities splitsen in derde, enzovoort. In termen van capaciteit, *partitie grootte* is voor de primaire functie van de verschillende alle lagen.

> [!NOTE]
> Alle **standaard** lagen ondersteuning [flexibele combinaties replica en partities](search-capacity-planning.md#chart) zodat u kunt [gewicht van uw systeem voor snelheid of opslag](search-performance-optimization.md) door het wijzigen van het saldo. **Basic** biedt tot drie replica's voor hoge beschikbaarheid maar alleen partitie heeft. **Gratis** lagen bieden geen toegewijde bronnen: computing resources worden gedeeld door meerdere gratis services.

### <a name="limits"></a>Limieten

Services hostresources, zoals indexen, indexeerfuncties, enzovoort. Elke laag legt [Servicelimieten](search-limits-quotas-capacity.md) op de hoeveelheid resources die u kunt maken. Hierdoor is een limiet van het aantal indexen (en andere objecten) de tweede verschillende functie tussen lagen. Houd er rekening mee de grenzen van het aantal indexen tijdens het bekijken van elke optie in de portal. Andere resources, zoals Indexeerfuncties en gegevensbronnen vaardigheden, worden index-limieten vastgelegd.

### <a name="search-units"></a>Zoekeenheden

Het belangrijkste facturering concept om te begrijpen is een *zoekeenheid* (SU) Dit is de facturering eenheid voor Azure Search. Omdat Azure Search, hangt af van de replica's en partities functie, niet dat u door een of andere gefactureerd. In plaats daarvan is facturering gebaseerd op een samenstelling van beide. Formulaically, is van het product van replica- en partities die worden gebruikt door een service in een SU: (R X P = SU). Ten minste elke service wordt gestart met 1 SU (één replica vermenigvuldigd met één partitie), maar een meer realistische model is mogelijk een 3-replica, 3-partitie service gefactureerd als 9 SUs. 

Hoewel elke laag geleidelijk hogere capaciteit biedt, kunt u brengt een deel van de totale capaciteit online, de rest in reserve houden. In termen van facturering is het aantal partities en replica's u brengt online, berekend met behulp van de formule SU, die bepaalt wat u daadwerkelijk betaalt.

Facturerings frequentie is per uur per SU, met elke laag met een ander tarief. Voor elke laag kan worden gevonden op classificeert [prijsinformatie](https://azure.microsoft.com/pricing/details/search/).

## <a name="consumption-patterns"></a>Patronen voor verbruik

De meeste klanten beginnen met de **vrije** service, zodat ze voor onbepaalde tijd worden behouden en kies vervolgens een van de **standaard** lagen voor ernstige ontwikkeling of productie werkbelastingen. 

![Azure search lagen](./media/search-sku-tier/tiers.png "Azure search Prijscategorieën")

Op beide uiteinden **Basic** en **S3 HD** voor belangrijke maar ongebruikelijk verbruik patronen bestaan. **Basic** is bedoeld voor kleine productieworkloads: SLA, biedt toegewezen resources, hoge beschikbaarheid, maar matige opslag, toegenomen uit op de totaal 2 GB. Deze laag is ontworpen voor klanten die consistent onder gebruikte beschikbare capaciteit. Aan het einde, **S3 HD** is voor werkbelastingen doorgaans ISV's, partners, [multitenant oplossingen](search-modeling-multitenant-saas-applications.md), of een willekeurige configuratie aanroepen voor een groot aantal kleine indexen. Dit heeft vaak voor de hand liggende wanneer **Basic** of **S3 HD** laag is de juiste keuze te maken, maar als u wilt bevestigen dat u kunt boeken [StackOverflow](https://stackoverflow.com/questions/tagged/azure-search) of [Neem contact op met Azure Ondersteuning voor](https://azure.microsoft.com/support/options/) voor verdere informatie.

Focus verschuiven naar de meest gangbare standaard lagen **S1 S3** zijn een voortgang van de capaciteit met infecties punten op de grootte van partitie- en maximumwaarden op aantal indexen, Indexeerfuncties en corollary resources te verhogen:

|  | S1 | S2 | S3 |  |  |  |  |
|--|----|----|----|--|--|--|--|
| Partitiegrootte|  25 GB | 100 GB | 250 GB |  |  |  |  |
| index en indexeerfunctie limieten| 50 | 200 | 200 |  |  |  |  |

**S1** is een algemene keuze wanneer toegewijde bronnen en meerdere partities niet meer noodzakelijk. Met partities van 25 GB voor maximaal 12 partities de per-service beperken op **S1** is 300 GB totaal als u partities op replica's maximaliseren (Zie [partities en replica's toe te wijzen](search-capacity-planning.md#chart) voor meer met gelijke taakverdeling de samenstelling.)

Pagina's Portal en prijscategorie zet de focus op partitiegrootte en opslag, maar voor elke laag alle mogelijkheden (schijfcapaciteit, snelheid, CPU's) berekenen lineair groeien met prijs. Een **S2** replica is sneller dan **S1**, en **S3** sneller dan **S2**. **S3** lagen opsplitsen het doorgaans lineaire compute prijzen patroon niet goed sneller i/o's. Als u een i/o's als de knelpunt voorziet een **S3** biedt u veel meer IOPS dan lagere niveaus.

**S3** en **S3 HD** worden ondersteund door identieke hoge capaciteit infrastructuur, maar elk een de maximumlimiet is bereikt op verschillende manieren. **S3** gericht is op een kleiner aantal zeer grote indexen. Als zodanig de resource afhankelijk is van de maximumlimiet (2,4 TB voor elke service). **S3 HD** gericht is op een groot aantal kleine indexen. Op 1000 indexen **S3 HD** de grenzen in de vorm van indexbeperkingen is bereikt. Als u een **S3 HD** klant die meer dan 1000 indexen nodig contact op met Microsoft Support voor meer informatie over om door te gaan.

> [!NOTE]
> Voorheen document limieten overweging zijn maar die niet meer van toepassing voor de meeste Azure Search-services na januari 2018 ingericht. Zie voor meer informatie over voorwaarden waarvoor limieten document nog steeds gelden [Servicelimieten: document limieten](search-limits-quotas-capacity.md#document-limits).
>

## <a name="evaluate-capacity"></a>Capaciteit evalueren

Ga hand voorhanden capaciteit en de kosten van het uitvoeren van de service. Lagen leggen beperkingen met betrekking tot twee niveaus (opslag en resources), dus u beide bedenken moet omdat ongeacht een scherm eerst de effectieve limiet is. 

Zakelijke vereisten, het aantal indexen, u moet doorgaans bepalen. Bijvoorbeeld, een globale index voor een grote opslagplaats van documenten of mogelijk meerdere indexen op basis van regio, toepassing of zakelijke gespecialiseerde.

Om te bepalen van de grootte van een index, u moet [een bouwen](search-create-index-portal.md). De structuur van de gegevens in Azure Search is voornamelijk een [omgekeerde index](https://en.wikipedia.org/wiki/Inverted_index), die andere kenmerken dan brongegevens heeft. Omvang en complexiteit voor een omgekeerde index worden bepaald door de inhoud, niet per se de hoeveelheid gegevens die u in het kanaal. Een bron grote hoeveelheden gegevens met grote redundantie kan resulteren in een index kleiner is dan een kleinere gegevensset die maximaal variabele inhoud bevat.  Hierdoor is het zelden kunt afleiden van de indexgrootte van de op basis van de grootte van de oorspronkelijke gegevensset.

### <a name="preliminary-estimates-using-the-free-tier"></a>Voorlopige maakt een schatting met behulp van de laag gratis

Een aanpak voor het schatten van de capaciteit is beginnen met de **vrije** laag. Intrekken die de **vrije** service biedt maximaal 3 indexen, 50 MB aan opslagruimte en 2 minuten van tijd te indexeren. Het kan lastig zijn om in te schatten van de indexgrootte van een verwachte met deze beperkingen, maar het volgende voorbeeld wordt een benadering:

+ [Een gratis service maken](search-create-service-portal.md)
+ Voorbereiden van een kleine, representatieve gegevensset (wordt ervan uitgegaan dat 5000 documenten en samplegrootte van tien procent)
+ [Een initiële index bouwen](search-create-index-portal.md) en noteer de grootte in de portal (wordt ervan uitgegaan dat 30 MB)

Ervan uitgaande dat het voorbeeld is zowel vertegenwoordiger en tien procent van de gehele gegevensbron, wordt een index 30 MB ongeveer 300 MB als alle documenten worden geïndexeerd. . Met dit nummer voorlopig worden mogelijk dubbel bedrag naar budget voor twee indexen (ontwikkeling en productie) voor een totaal van 600 MB in opslagvereisten. Dit eenvoudig door wordt voldaan aan de **Basic** servicetier, zodat u er zou gaan.

### <a name="advanced-estimates-using-a-billable-tier"></a>Geavanceerde maakt een schatting met behulp van een factureerbare laag

Bepaalde klanten graag starten met toegewijde bronnen die kunnen geschikt voor grotere steekproeven en verwerkingstijden en vervolgens tijdens het ontwikkelen van ontwikkelen realistische maakt een schatting van de index aantal, de grootte en de volumes van de query. In eerste instantie is een service is ingericht op basis van een schatting van de beste schatting en vervolgens naarmate het project voor de ontwikkeling meer vormt krijgt, teams meestal weten of de bestaande service boven of onder de capaciteit voor geprojecteerde productieworkloads is. 

1. [Bekijk Servicelimieten op elke laag](https://docs.microsoft.com/en-us/azure/search/search-limits-quotas-capacity#index-limits) om te bepalen of het aantal indexen, u moet kunnen ondersteuning voor lagere niveaus. Via de **Basic**-**S1**- **S2** lagen, index-limieten zijn 15 50 200, respectievelijk.

1. [Een service maken op een factureerbare laag](search-create-service-portal.md):

    + Start lage op **Basic** of **S1** als u aan het begin van uw leerproces.
    + Start hoge op **S2** of zelfs **S3**als voor de hand liggende grootschalige indexeren en de query laadt.

1. [Een initiële index bouwen](search-create-index-portal.md) om te bepalen hoe de brongegevens worden omgezet naar een index. Dit is de enige manier om een schatting maken van de indexgrootte van de.

1. [Bewaken van opslag, Servicelimieten query volume en latentie](search-monitor-usage.md) in de portal. De portal ziet u query's per tweede, beperkte query's en zoeken latentie; alle kunt u bepalen of u zich op de juiste laag. Behalve portal metrische gegevens, kunt u configureren diepe bewaking, zoals clickthrough-analyse, doordat [zoeken traffic analytics](search-traffic-analytics.md). 

Aantal van de index en de grootte evenveel relevant zijn voor uw analyse omdat de maximale limiet wordt bereikt door middel van volledig gebruik van opslag (partities) of door het maximum aantal beperkingen met betrekking tot resources (indexen, indexeerfuncties, enzovoort), afhankelijk van wat zich het eerste voordoet. De portal kunt u het bijhouden van beide, weergegeven met huidige gebruik en maximaal limieten naast elkaar op de pagina overzicht.

> [!NOTE]
> Vereisten voor opslag kunnen worden te veel hoge als documenten overbodige gegevens bevatten. In het ideale geval bevatten documenten alleen de gegevens die u nodig hebt voor de zoekfunctie. Binaire gegevens is niet-doorzoekbare en moet worden opgeslagen afzonderlijk (mogelijk in een Azure table of blob-opslag) met een veld in de index van een URL-verwijzing naar de externe gegevens bevatten. De maximale grootte van een afzonderlijk document 16 MB is (of minder als u meerdere documenten in één aanvraag uploaden bulksgewijs zijn). [Servicelimieten in Azure Search](search-limits-quotas-capacity.md) bevat meer informatie.
>

**Overwegingen voor query-volume**

Query's per seconde (QPS) is een metriek die uitzien tijdens prestatieafstemming krijgt, maar is in het algemeen geen laag overweging tenzij u zeer hoge query volume in het begin verwacht.

Alle van de lagen standard bieden een evenwicht van replica's op partities, snellere reactietijd ook een query door middel van replica's meer ondersteuning voor het laden van taakverdeling en extra partities voor parallelle verwerking. U kunt voor prestaties afstemmen nadat de service is ingericht.

Klant die verwacht strong aanhoudend query volumes vanaf het begin rekening moeten houden voor hogere lagen, ondersteund door krachtigere hardware. U kunt vervolgens partities en replica's offline te halen of zelfs overschakelen naar een lagere service van de laag als u deze query volumes niet kunt geven aan. Zie voor meer informatie over het berekenen van de query doorvoer [Azure Search-prestaties en optimalisatie](search-performance-optimization.md).


**Serviceovereenkomsten**

De **vrije** laag en de preview-functies niet afkomstig zijn met [service level agreements (Sla)](https://azure.microsoft.com/support/legal/sla/search/v1_0/). Voor alle factureerbare lagen, sla's van kracht als u voldoende redundantie voor uw service inricht. Twee of meer replica's zijn vereist voor de SLA voor query (gelezen). Drie of meer replica's zijn vereist voor query's en indexering SLA (lezen / schrijven). Het aantal partities is niet een SLA-overweging. 

## <a name="tips-for-tier-evaluation"></a>Tips voor evaluatie van de laag

+ Informatie over het bouwen van indexen op efficiënte en welke methoden vernieuwen zijn de minimale impactful. Het is raadzaam [zoeken traffic analytics](search-traffic-analytics.md) om de ervaring voor de queryactiviteit inzicht te krijgen.

+ Toestaan van metrische gegevens te bouwen om query's en verzamelen van gegevens om gebruikspatronen (query's tijdens de kantooruren, tijdens de daluren te indexeren) en deze gegevens gebruiken om te informeren over toekomstige service beslissingen voor de inrichting. Tijdens het niet praktisch op een niveau Uurlijkse of dagelijkse kunt u partities en resources voor geplande wijzigingen in de query-volumes, of niet-geplande maar volgehouden wijzigingen als niveaus lang genoeg is houdt voor de actie te ondernemen garandeert dynamisch aanpassen.

+ Houd er rekening mee dat het enige nadeel van onder-inrichting is dat u een service moeten verbreken wellicht als de daadwerkelijke vereisten groter dan u geschatte zijn. Als u wilt voorkomen dat de service wordt onderbroken, zou u een nieuwe service maken in hetzelfde abonnement op een hogere categorie en voer dit gelijktijdige totdat alle apps en aanvragen gericht op het nieuwe eindpunt.

## <a name="next-steps"></a>Volgende stappen

Beginnen met een **vrije** servicetier en bouwen van een initiële index met behulp van een subset van uw gegevens om te begrijpen van de kenmerken ervan. De structuur van de gegevens in Azure Search is een omgekeerde index, waarbij de grootte en complexiteit van een omgekeerde index wordt bepaald door de inhoud. Houd er rekening mee dat maximaal redundante inhoud leidt doorgaans tot een kleinere index dan maximaal onregelmatige inhoud. Hierdoor is het kenmerken van inhoud in plaats van de grootte van de gegevensset die opslagvereisten index bepaalt.

Zodra er een initiële idee van de indexgrootte, [inrichten van een factureerbare service](search-create-service-portal.md) op een van de lagen die wordt besproken in dit artikel, ofwel **Basic** of een **standaard** laag. Kunstmatige beperkingen op gegevenssubsets versoepelen en [uw index opnieuw worden opgebouwd](search-howto-reindex.md) alle van de gegevens die u wilt worden doorzocht.

[Partities en replica's toe te wijzen](search-capacity-planning.md) zo nodig om op te halen van de prestaties en schalen die u nodig hebt.

Als de prestaties en capaciteit fijn zijn, bent u klaar. Anders maakt opnieuw een search-service op een andere tier die meer nauw afgestemd op uw behoeften.

> [!NOTE]
> Voor meer hulp bij uw vragen naar boeken [StackOverflow](https://stackoverflow.com/questions/tagged/azure-search) of [Neem contact op met ondersteuning van Azure](https://azure.microsoft.com/support/options/).