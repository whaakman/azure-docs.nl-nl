---
title: Kies een prijscategorie laag of SKU voor Azure Search-service - Azure Search
description: "Azure Search kan worden ingericht in deze SKU's: Gratis, Basic en Standard- en Standard is beschikbaar in verschillende resourceconfiguraties en capaciteit niveaus."
services: search
author: HeidiSteen
manager: cgronlun
tags: azure-portal
ms.service: search
ms.topic: conceptual
ms.date: 05/08/2019
ms.author: heidist
ms.custom: seodec2018
ms.openlocfilehash: 00422209302bbcc2139be4f6b490f0bb2816c051
ms.sourcegitcommit: f6c85922b9e70bb83879e52c2aec6307c99a0cac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/11/2019
ms.locfileid: "65539251"
---
# <a name="choose-a-pricing-tier-for-azure-search"></a>Kies een prijscategorie voor Azure Search

Wanneer u een Azure Search-service maakt een [resource is gemaakt](search-create-service-portal.md) op een prijzen laag of SKU die voor de levensduur van de service opgelost. Lagen zijn gratis, Basic, Standard en met geoptimaliseerde opslag. Standard en met geoptimaliseerde opslag zijn beschikbaar met verschillende configuraties en capaciteit.

De meeste klanten beginnen met de gratis laag, zodat ze de service kunnen evalueren. Ze vervolgens een upgrade naar een van de hogere lagen voor ontwikkeling en productie-implementaties. U kunt alle snelstartgidsen en zelfstudies uitvoeren met behulp van de laag gratis, met inbegrip van die voor het resource-intensieve cognitief zoeken.

> [!NOTE]
> Microsoft biedt momenteel de Servicelagen met geoptimaliseerde opslag in een preview kortingsprijzen voor het testen en experimenteren met het doel van het verzamelen van feedback. Laatste prijzen worden later aangekondigd wanneer deze lagen algemeen beschikbaar zijn. We raden u aan die deze lagen gebruiken voor productie-Apps.

Lagen weerspiegelen de eigenschappen van de hardware die als host fungeert voor de service (in plaats functies) en door elkaar worden onderscheiden:

+ Het aantal indexen die u kunt maken.
+ De grootte en de snelheid van partities (fysieke opslag).

Hoewel alle lagen, inclusief de laag gratis, in het algemeen functiepariteit bieden, kunnen grotere workloads nodig voor hogere lagen dicteren. Bijvoorbeeld, [AI indexeren met cognitieve Services](cognitive-search-concept-intro.md) heeft langlopende vaardigheden die time-out voor een gratis service, tenzij de gegevensset klein is.

> [!NOTE] 
> De uitzondering functiepariteit is [indexeerfuncties](search-indexer-overview.md), die zijn niet beschikbaar op S3 HD.
>

Binnen een laag, kunt u [replica en partitie resources aanpassen](search-capacity-planning.md) vergroten of verkleinen van de schaal. U kunt beginnen met een of twee van elk en de rekenkracht voor de workload van een intensief indexering tijdelijk verhogen. De mogelijkheid om af te stemmen resource niveaus binnen een laag voegt u flexibiliteit toe, maar ook iets ingewikkelder, uw analyse. Mogelijk hebt om te experimenteren om te zien of een lagere laag met meer resources/replica's betere waarde en prestaties dan een hogere laag met minder bronnen biedt. Zie voor meer informatie over wanneer en waarom zou u capaciteit aanpassen, [aandachtspunten voor prestaties en optimalisatie](search-performance-optimization.md).

## <a name="tiers-for-azure-search"></a>Lagen voor Azure Search

De volgende tabel bevat de beschikbare categorieën. U vindt meer informatie over de verschillende lagen op het [pagina met prijzen](https://azure.microsoft.com/pricing/details/search/), in de [Servicelimieten in Azure Search](search-limits-quotas-capacity.md) artikel en pagina in de portal wanneer u bij het inrichten van een service.

|Laag | Capaciteit |
|-----|-------------|
|Gratis | Gedeeld met andere abonnees. Niet schaalbaar. Beperkt tot drie indexen en 50 MB aan opslagruimte. |
|Basis | Toegewezen computerbronnen voor productieworkloads op kleinere schaal. Een partitie van 2 GB en maximaal drie replica's. |
|Standard 1 (S1) | Voor S1 en hoger, toegewezen machines met meer capaciteit voor de opslag en verwerking op ieder niveau. Voor S1 wordt de grootte van partitie 25 GB per partitie (met een maximum van 300 GB per service). |
|Standard 2 (S2) | Vergelijkbaar naar S1 uitvoert, maar met partities van 100 GB (en een maximum van 1,2 TB per service). |
|Standard 3 (S3) | 200 GB-partities (met een maximum van 2,4 TB per service). |
|Standard 3 hoge dichtheid (S3 HD) | High-densitysampling is een *hostmodus* voor S3. De onderliggende hardware is geoptimaliseerd voor een groot aantal kleinere indexen en is bedoeld voor scenario's voor multitenancy. S3 HD heeft de dezelfde per eenheid kosten in rekening gebracht zoals S3, maar de hardware is geoptimaliseerd voor het snel bestanden lezen op een groot aantal kleinere indexen.|
|1 (L1) met geoptimaliseerde opslag | 1 TB partities (met een maximum van 12 TB per service). |
|2 (L2) met geoptimaliseerde opslag | 2 TB-partities (met een maximum van 24 TB per service). |

> [!NOTE] 
> De lagen met geoptimaliseerde opslag bieden grotere opslagcapaciteit tegen een lagere prijs per TB dan de standaard-laag. Primaire afweging is hogere latentie van query, die u voor uw specifieke toepassingsvereisten controleren moet.  Zie voor meer informatie over de Prestatieoverwegingen van deze laag [aandachtspunten voor prestaties en optimalisatie](search-performance-optimization.md).
>

## <a name="how-billing-works"></a>Werking van facturering

Er zijn drie manieren u kosten maken in Azure Search en zijn er vaste en variabele onderdelen. In deze sectie beschrijft de drie facturering onderdelen: core servicekosten, kosten voor uitgaande gegevens en AI-verrijkt indexeren.

### <a name="core-service-costs-fixed-and-variable"></a>Servicekosten voor Core (vaste en variabele)

Voor de service zelf is de minimale kosten in rekening gebracht de eerste zoekeenheid (1 replica x 1 partitie). Omdat de service op iets minder dan deze configuratie kan niet worden uitgevoerd, is dit minimum opgelost voor de levensduur van de service.

Meer dan het minimum, kunt u toevoegen replica's en partities onafhankelijk van elkaar. U kunt bijvoorbeeld alleen replica's of alleen de partities toevoegen. Incrementele verhogingen in capaciteit via de replica's en partities maken om de variabele kosten-component.

Facturering is gebaseerd op een [formule (replica's x partities x-tarief)](#search-units). Het tarief dat u betaalt, is afhankelijk van de prijscategorie die u selecteert.

Voor gratis, basis en S1 wordt per eenheid prijzen in de volgende schermafbeelding aangegeven. (S2, S3, L1 en L2 worden niet weergegeven.) Als u een eenvoudige service maakt, uw maandelijkse kosten voor de gemiddelde van de waarde die wordt weergegeven voor *prijs 1*. Voor een Standard-service, wordt uw maandelijkse kosten voor de waarde die wordt weergegeven voor gemiddelde van *prijs 2*. Kosten per eenheid verhogen voor elke laag, omdat de rekenkundige power- en opslagcapaciteit op elke opeenvolgende laag valt. De tarieven voor Azure Search zijn beschikbaar op de [Azure Search-pagina met prijzen](https://azure.microsoft.com/pricing/details/search/).

![Prijs per eenheid](./media/search-sku-tier/per-unit-pricing.png "prijzen Per eenheid")

Wanneer u van de kosten van een search-oplossing schatten bent, houd er rekening mee dat prijzen en capaciteit niet lineair. (Verdubbeling van de capaciteit van de kosten meer dan verdubbeld.) Zie voor een voorbeeld van hoe u van de formule werkt [toewijzen van replica's en partities](search-capacity-planning.md#how-to-allocate-replicas-and-partitions).

#### <a name="billing-based-on-search-units"></a>Facturering op basis van de search-eenheden

Het belangrijkste facturering concept voor meer informatie over voor Azure Search-bewerkingen is de *zoekeenheid* (SU). Omdat Azure Search, hangt af van de replica's en partities om te indexeren en query's, verstandig niet het om aan te brengen door slechts in één van de andere kosten in rekening. In plaats daarvan is facturering gebaseerd op een samenstelling van beide.

SU is het product van de *replica's* en *partities* die worden gebruikt door een service: **(R x P = SU)** .

Elke service wordt gestart met één SU (één replica wordt vermenigvuldigd met één partitie) als het minimum. Het maximum voor elke service is 36 su's. Dit maximum kan op verschillende manieren worden bereikt: 6 partities x 6 replica's, of 3 partities x 12 replica's, bijvoorbeeld. Het is gebruikelijk is minder dan de totale capaciteit (bijvoorbeeld een 3-replica, 3-partition service in rekening gebracht als 9 su's) gebruiken. Zie de [combinaties van partitie en replica](search-capacity-planning.md#chart) grafiek voor geldige combinaties.

Het tarief is per uur per SU. Elke categorie heeft een steeds hogere snelheid. Hogere lagen komen met grotere en sneller partities, en dit draagt bij aan een totale hogere uurtarief voor die laag. U kunt de tarieven voor elke laag weergeven op de [prijsinformatie](https://azure.microsoft.com/pricing/details/search/) pagina.

De meeste klanten Breng slechts een deel van de totale capaciteit online, met de rest in reserveren. Voor facturering, het aantal partities en replica's die u online, brengt bepaalt berekend door het SU-formule, die wat u betaalt op uurbasis.

### <a name="data-egress-charges-during-indexing"></a>Kosten voor uitgaande gegevens tijdens het indexeren

Met behulp van [Azure Search-indexeerfuncties](search-indexer-overview.md) mogelijk van invloed zijn op de facturering, afhankelijk van de locatie van uw services. Volledig als u de Azure Search-service in dezelfde regio als uw gegevens maakt, kunt u kosten voor uitgaande gegevens voorkomen. Hier volgt informatie uit de [bandbreedte pagina met prijzen](https://azure.microsoft.com/pricing/details/bandwidth/):

+ Microsoft niet in rekening gebracht voor binnenkomende gegevens voor elke service op Azure, of voor alle uitgaande gegevens van Azure Search.

+ Er zijn geen kosten voor gegevens die via de kabel binnenkomen bij alle services zich in dezelfde regio in MST oplossingen.

In rekening gebracht voor uitgaande gegevens als services zich in verschillende regio's. Deze kosten zijn niet deel uit van uw Azure Search-factuur. Ze worden hier vermeld omdat als u gegevens of AI verrijkt indexeerfuncties gegevens wilt halen uit verschillende regio's, u de kosten die worden weergegeven in uw algehele factuur ziet.

### <a name="ai-enriched-indexing-with-cognitive-services"></a>AI-verrijkt indexeren met cognitieve Services

Voor [AI indexeren met cognitieve Services](cognitive-search-concept-intro.md), moet u van plan bent om te koppelen van een factureerbare Azure Cognitive Services-resource, in dezelfde regio als de Azure Search, op de S0-prijscategorie voor de verwerking van betalen per gebruik. Er is geen vaste kosten die gepaard gaan met het koppelen van Cognitive Services. U betaalt alleen voor de verwerking die u nodig hebt.

Afbeelding extractie tijdens documenten kraken is een Azure Search-kosten in rekening gebracht. Dit wordt in rekening gebracht op basis van het aantal afbeeldingen uit uw documenten hebt uitgepakt. Extractie van tekst is momenteel gratis.

Andere enrichments, zoals de verwerking van natuurlijke taal, zijn gebaseerd op [ingebouwde cognitieve vaardigheden](cognitive-search-predefined-skills.md) kosten in rekening gebracht tegen een Cognitive Services-resource. Deze wordt hetzelfde tarief gefactureerd als wanneer u de taak heeft uitgevoerd met behulp van Cognitive Services rechtstreeks. Zie voor meer informatie, [koppelen van een Cognitive Services-resource met een set vaardigheden](cognitive-search-attach-cognitive-services.md).

<a name="search-units"></a>

#### <a name="billing-for-image-extraction-in-cognitive-search"></a>Facturering voor het ophalen van de afbeelding in cognitief zoeken

Als u afbeeldingen niet van de bestanden in een cognitief zoeken pijplijn indexeren extraheren, u in rekening gebracht voor de bewerking op uw factuur voor Azure Search. In een [configuratie van de indexeerfunctie](https://docs.microsoft.com/rest/api/searchservice/create-indexer#indexer-parameters), **imageAction** is de parameter die wordt geactiveerd extractie van de installatiekopie. Als **imageAction** is ingesteld op 'none' (de standaardinstelling), u niet in rekening gebracht voor het ophalen van de installatiekopie.

Prijzen zijn onderhevig aan wijzigingen. Dit wordt beschreven op de [prijsinformatie](https://azure.microsoft.com/pricing/details/search/) pagina voor Azure Search.

#### <a name="billing-for-built-in-skills-in-cognitive-search"></a>Facturering voor ingebouwde vaardigheden in cognitief zoeken

Bij het instellen van een pijplijn verrijking eventuele [ingebouwde vaardigheden](cognitive-search-predefined-skills.md) gebruikt in de pijplijn zijn gebaseerd op machine learning-modellen. Deze modellen worden geleverd door de Cognitive Services. Als u deze modellen gebruikt tijdens het indexeren, wordt u gefactureerd hetzelfde tarief als u zijn zou als de resource direct is aangevraagd.

Stel dat u hebt een pijplijn met behulp van optische tekenherkenning (OCR) voor de gescande JPEG-bestanden en de resulterende tekst in een Azure Search-index wordt gepusht voor vrije zoekquery's. Uw pijplijn voor indexering omvat een indexeerfunctie met de [OCR-kwalificatie](cognitive-search-skill-ocr.md), en dat de kwalificatie zou zijn [die is gekoppeld aan een resource voor Cognitive Services](cognitive-search-attach-cognitive-services.md). Wanneer u de indexeerfunctie uitvoert, worden kosten in rekening gebracht voor de uitvoering van OCR wordt weergegeven op uw factuur cognitieve Resources.

## <a name="tips-for-reducing-costs"></a>Tips voor het verminderen van kosten

U kan niet uw kosten verlagen van de service afsluiten. Toegewezen resources worden altijd operationele, toegewezen voor exclusief gebruik gedurende de levensduur van uw service. De enige manier om uw factuur verlagen is om te beperken van replica's en partities op een niveau waarmee u nog steeds aanvaardbare prestaties en [SLA-naleving](https://azure.microsoft.com/support/legal/sla/search/v1_0/).

Een manier om kosten te verlagen is een laag met een lagere uurtarief kiezen. S1 uurtarieven zijn lager dan S2 of S3-tarieven. Ervan uitgaande dat u uw service met de laagste waarde van de load-projecties inrichten, als uw bedrijf de service te groot, kunt u een tweede grotere lagen service maken, het samenstellen van de tweede service, en verwijder vervolgens het eerste item.

Als u de capaciteitsplanning voor on-premises servers hebt gedaan, weet u is het gebruikelijk dat 'kopen van', zodat u de verwachte groei kunt verwerken. Met een cloudservice, kunt u verdere kosten te besparen meer doelbewust omdat u niet bent gekoppeld aan een specifieke aankoop. U kunt altijd overschakelen naar een hogere lagen service als de huidige bewerking is niet voldoende.

### <a name="capacity"></a>Capaciteit

In Azure Search capaciteit is gestructureerd als *replica's* en *partities*.

+ Replica's zijn exemplaren van de search-service. Elke replica als host fungeert voor een gelijke kopie van een index. Een service met zes replica's heeft bijvoorbeeld zes kopieën van elke index geladen in de service.

+ Partities indexen opslaan en doorzoekbare gegevens automatisch te splitsen. Twee partities splitsen uw index in de helft, drie partities deze opsplitsen in derde, enzovoort. Wat betreft de capaciteit, *partitie grootte* is de primaire onderscheidende functie tussen lagen.

> [!NOTE]
> Ondersteuning voor alle Standard- en opslag geoptimaliseerd lagen [flexibele combinaties van replica's en partities](search-capacity-planning.md#chart) zodat u deze kunt [optimaliseren van uw systeem voor snelheid of opslag](search-performance-optimization.md) door het veranderen van de balans. De Basic-laag biedt maximaal drie replica's voor hoge beschikbaarheid, maar slechts één partitie is. Gratis lagen toegewezen resources niet bevatten: computing resources worden gedeeld door meerdere abonnees.

### <a name="more-about-service-limits"></a>Meer informatie over Servicelimieten

Hostresources, zoals indexen en indexeerfuncties-Services. Elke laag legt [Servicelimieten](search-limits-quotas-capacity.md) op het aantal resources die u kunt maken. Het maximum aantal indexen (en andere objecten) is dus de tweede onderscheidende functie tussen lagen. Houd er rekening mee de limieten voor het aantal indexen tijdens het bekijken van elke optie in de portal. Andere resources, zoals Indexeerfuncties en gegevensbronnen kennis en vaardigheden, zijn index-limieten aangebracht.

## <a name="consumption-patterns"></a>Gebruikspatronen voor databasebronnen

De meeste klanten beginnen met de gratis service, die ze voor onbepaalde tijd houden, en kies vervolgens een van de lagen Standard- of opslag geoptimaliseerd voor ernstige ontwikkeling of productie workloads.

![Azure Search Prijscategorieën](./media/search-sku-tier/tiers.png "Azure Search Prijscategorieën")

Op de hoge en lage-ends zijn Basic en S3 HD belangrijke, maar ongewone verbruik patronen. Basic is bedoeld voor kleine productieworkloads. Het biedt een SLA's, toegewezen resources en hoge beschikbaarheid, maar het gemiddelde hoeveelheden opslag, toegenomen van 2 GB totaal biedt. Deze laag is ontworpen voor klanten die consistent beschikbare capaciteit ondergebruiken. Aan de hoge einde S3 HD is bedoeld voor workloads karakteristiek voor ISV's, partners, [multitenant oplossingen](search-modeling-multitenant-saas-applications.md), of een configuratie die voor een groot aantal kleine indexen worden aangeroepen. Het is vaak duidelijk als basis of S3 HD is de juiste laag. Als u bevestigen wilt, kunt u boeken aan [StackOverflow](https://stackoverflow.com/questions/tagged/azure-search) of [Neem contact op met ondersteuning van Azure](https://azure.microsoft.com/support/options/) voor hulp.

De veelgebruikte standard lagen S1 tot S3, vormen een voortgang van het niveau van de capaciteit te vergroten. Er zijn belangrijk punten op de grootte van partitie en beperkingen met betrekking tot aantallen indexen, Indexeerfuncties en corollary resources:

|  | S1 | S2 | S3 |  |  |  |  |
|--|----|----|----|--|--|--|--|
| Partitiegrootte|  25 GB | 100 GB | 200 GB |  |  |  |  |
| limieten voor index en indexeerfunctie| 50 | 200 | 200 |  |  |  |  |

S1 is een algemene keuze voor klanten die behoefte toegewezen resources en meerdere partities hebben. S1 biedt partities van 25 GB en maximaal 12 partities, een per-service-limiet van 300 GB bieden als u partities via de replica's te maximaliseren. (Zie [partities en replica's toewijzen](search-capacity-planning.md#chart) verdeeld toewijzingen voor meer informatie.)

De portal en prijzen pagina zet de focus op de grootte van partitie en opslag, maar voor elke laag alle compute-mogelijkheden (schijfcapaciteit, de snelheid van CPU's) in het algemeen lineair vergroten met de prijs. De replica van een S2 is sneller dan S1 en S3 is sneller dan S2. S3 lagen onderbreking van de lineaire compute-prijzen-patroon met onevenredig sneller i/o's. Als u i/o het knelpunt worden verwacht, houd er rekening mee dat u veel meer IOPS met S3 dan u met lagere lagen krijgt kunt krijgen.

S3 en S3 HD worden ondersteund door identieke hoge capaciteit infrastructuur, maar ze hun maximale limieten bereiken op verschillende manieren. S3 is gericht op een kleiner aantal zeer grote indexen, zodat de maximumlimiet afhankelijk van de resource is (2,4 TB voor elke service). S3 HD is bedoeld voor een groot aantal kleine indexen. S3 HD bereikt 1000 indexen in de vorm van indexbeperkingen uiterste te belasten. Als u een S3 HD-klant bent en u meer dan 1000 indexen moet, neem dan contact op met Microsoft Support voor meer informatie over om door te gaan.

> [!NOTE]
> Document limieten in één keer een betrokken zijn, maar ze zijn niet meer van toepassing op nieuwe services. Zie voor meer informatie over voorwaarden waarin document limieten nog steeds van toepassing [Document limieten](search-limits-quotas-capacity.md#document-limits).
>

Geoptimaliseerde opslaglagen, L1 en L2, zijn ideaal voor toepassingen met vereisten voor grote hoeveelheden gegevens maar een relatief klein aantal gebruikers, wanneer de hoogste prioriteit voor het minimaliseren van de latentie van query niet.  

|  | L1 | L2 |  |  |  |  |  |
|--|----|----|--|--|--|--|--|
| Partitiegrootte|  1 TB | 2 TB |  |  |  |  |  |
| limieten voor index en indexeerfunctie| 10 | 10 |  |  |  |  |  |

L2 biedt tweemaal de totale opslagcapaciteit van L1.  De laag op basis van de maximale hoeveelheid gegevens die u vindt dat de index moet kiezen. De partities van de laag L1 omhoog schalen in stappen van 1 TB tot een maximum van 12 TB. De L2-partities verhogen met 2 TB per partitie tot een maximum van 24 TB.

## <a name="evaluating-capacity"></a>Evaluatie van capaciteit

Capaciteit en de kosten van het uitvoeren van de service zijn direct gerelateerd. Lagen leggen beperkingen met betrekking tot twee niveaus: opslag- en -resources. U moet doen met beide omdat ongeacht welke limiet u eerst bereikt, is de effectieve limiet.

Bedrijfsvereisten voorschrijven doorgaans het aantal indexen u moet. Bijvoorbeeld, moet u mogelijk een globale index voor een grote opslagruimte voor documenten. Of u moet mogelijk meerdere indexen op basis van regio, toepassing of nichemarkten business.

Om te bepalen van de grootte van een index, die u hebt tot [bouwen een](search-create-index-portal.md). De gegevensstructuur van de in Azure Search is voornamelijk een [omgekeerde index](https://en.wikipedia.org/wiki/Inverted_index) structuur, met andere eigenschappen dan de brongegevens. Voor een omgekeerde index, worden grootte en complexiteit bepaald door inhoud, niet per se op basis van de hoeveelheid gegevens die u in het kanaal. Een bron van grote hoeveelheden gegevens met hoge redundantie kan leiden tot een kleinere-index dan een kleinere gegevensset die zeer variabel inhoud bevat. Het is dus zelden mogelijk afleiden van de indexgrootte van de is gebaseerd op de grootte van de oorspronkelijke gegevensset.

> [!NOTE] 
> Hoewel het schatten van de toekomstige behoeften voor indexen en opslag giswerk lijkt kunt, is het waard is om dit. Als de capaciteit van een laag blijkt te laag zijn, moet u voor het inrichten van een nieuwe service met een hogere laag en vervolgens [uw indexen opnieuw laden](search-howto-reindex.md). Er is geen in-place upgrade van een service van een SKU naar een andere.
>

### <a name="step-1-develop-rough-estimates-by-using-the-free-tier"></a>Stap 1: Maakt een ruwe schatting ontwikkelen met behulp van de gratis laag

Een aanpak voor het schatten van de capaciteit is om te beginnen met de gratis laag. Houd er rekening mee dat de gratis service biedt tot drie indexen, 50 MB opslag, en 2 minuten tijd worden geïndexeerd. Het kan lastig zijn om in te schatten van de indexgrootte van een verwachte met deze beperkingen. Dit is een methode die u kunt uitvoeren:

+ [Maak een gratis service](search-create-service-portal.md).
+ Bereid een kleine, representatieve gegevensset (bijvoorbeeld 5000 documenten en de grootte van 10 procent steekproef).
+ [Bouw een eerste index](search-create-index-portal.md) en noteer de grootte in de portal (bijvoorbeeld: 30 MB).

Als het voorbeeld vertegenwoordiger en 10 procent van de gehele gegevensbron is, wordt een index 30 MB ongeveer 300 MB als alle documenten die zijn geïndexeerd. Met dit voorlopige nummer hebt, u mogelijk dubbel bedrag voor het budget voor twee indexen (ontwikkeling en productie). Dit biedt u een totaal van 600 MB in de opslagvereisten voor. Deze vereiste eenvoudig door wordt voldaan aan de laag Basic, zodat u er rekening gebracht.

### <a name="step-2-develop-refined-estimates-by-using-a-billable-tier"></a>Stap 2: Verfijnd schattingen ontwikkelen met behulp van een factureerbare laag

Sommige klanten de voorkeur geeft aan om te beginnen met toegewezen resources die kunnen groter steekproeven en verwerkingstijden en kan vervolgens ontwikkelen realistische maakt een schatting van de index aantal, de grootte en de query volumes tijdens de ontwikkeling. Een service wordt in eerste instantie wordt ingericht op basis van een inschatting schatting. Klik, als het project voor de ontwikkeling zich verder ontwikkelt, teams meestal weten of de bestaande service boven of onder de capaciteit voor geplande productieworkloads.

1. [Bekijk Servicelimieten op elke laag](https://docs.microsoft.com/azure/search/search-limits-quotas-capacity#index-limits) om te bepalen of het aantal indexen, u moet kunnen ondersteuning voor lagere niveaus. Voor de lagen Basic, S1 en S2 index limieten zijn 15, 50 en 200, respectievelijk. De laag geoptimaliseerd voor opslag heeft een limiet van 10 indexen, omdat het is ontworpen ter ondersteuning van een klein aantal grote indexen.

1. [Een service maken op een factureerbare laag](search-create-service-portal.md):

    + Start laag, basis-of S1, bent u aan het begin van de leercurve.
    + Start hoog, S2 of zelfs S3, als u weet dat u gaat grootschalige indexeren en query-belastingen.
    + Starten met geoptimaliseerde opslag op L1 of L2, als u een grote hoeveelheid gegevens indexeren wilt en querybelasting relatief klein, als met een interne bedrijfstoepassing is.

1. [Bouw een eerste index](search-create-index-portal.md) om te bepalen hoe gegevens worden omgezet naar een index. Dit is de enige manier om te schatten van de indexgrootte van de.

1. [Opslag, Servicelimieten, queryvolume en latentie controleren](search-monitor-usage.md) in de portal. De portal ziet u query's per seconde, beperkte query's en zoeklatentie. Al deze waarden kunt u bepalen als u de juiste laag hebt geselecteerd. Ook kunt u configureren diepe bewaking van waarden zoals clickthrough-analyses door in te schakelen [zoekverkeer](search-traffic-analytics.md).

Index aantal en de grootte zijn net zo belangrijk is voor uw analyse. Dit is omdat de maximale limieten zijn bereikt via volledig benut opslag (partities) of door de maximale limieten voor resources (indexen, indexeerfuncties, enzovoort), afhankelijk van wat het eerste komt. De portal kunt u bijhouden van beide, van huidige gebruiks- en maximumlimieten naast elkaar op de pagina overzicht.

> [!NOTE]
> Vereisten voor opslag kunnen worden schaalfactor als documenten overbodige gegevens bevatten. In het ideale geval bevatten documenten alleen de gegevens die u nodig hebt voor de zoekfunctie. Binaire gegevens is niet doorzoekbaar en moet afzonderlijk worden opgeslagen (mogelijk in een Azure-tabel of blob storage). Een veld moet vervolgens worden toegevoegd in de index voor het opslaan van een URL-verwijzing naar de externe gegevens. De maximale grootte van een afzonderlijke document is 16 MB (of minder als u bulksgewijs uploaden van meerdere documenten in één aanvraag). Zie voor meer informatie, [Servicelimieten in Azure Search](search-limits-quotas-capacity.md).
>

**Overwegingen voor het volume van query**

Query's per seconde (QPS) is een belangrijke metrische tijdens het afstemmen van prestaties, maar het is doorgaans alleen laag overweging als u verwacht hoge queryvolume in het begin dat.

De standaard-laag kunnen bieden een evenwicht van replica's en partities. U kunt query doorlooptijd verhogen door replica's voor de taakverdeling toe te voegen of toevoegen van partities voor parallelle verwerking. U kunt vervolgens af te stemmen voor prestaties nadat de service is ingericht.

Als u verwacht hoge langdurige query volumes vanaf het begin dat, moet u rekening houden met de hogere standaard-laag, ondersteund door krachtigere hardware. U kunt vervolgens partities en replica's offline te halen, of zelfs overschakelen naar een lager niveau-service, als deze volumes query niet wordt uitgevoerd. Zie voor meer informatie over het berekenen van de query-doorvoer [Azure Search-prestaties en optimalisatie](search-performance-optimization.md).

De lagen met geoptimaliseerde opslag zijn handig voor workloads met veel gegevens, ondersteuning van meer opslag voor algemeen beschikbare index voor wanneer query latentievereisten minder belangrijk zijn. U moet wel extra replica's gebruiken voor load balancing en aanvullende partities voor parallelle verwerking. U kunt vervolgens af te stemmen voor prestaties nadat de service is ingericht.

**Service level agreements**

De gratis laag en preview-functies niet bieden [service level agreements (Sla's)](https://azure.microsoft.com/support/legal/sla/search/v1_0/). Voor alle factureerbare lagen, sla's van kracht wanneer u voldoende redundantie voor uw service inricht. U moet twee of meer replica's hebben voor de query (lezen) Sla's. U moet drie of meer replica's voor query's en indexering van SLA's (lezen-schrijven). Het aantal partities heeft geen invloed op de SLA's.

## <a name="tips-for-tier-evaluation"></a>Tips voor evaluatie van de laag

+ Informatie over het bouwen van efficiënte indexen en informatie over welke methoden vernieuwen weinig mogelijk impact hebben. Gebruik [zoekverkeer](search-traffic-analytics.md) Queryactiviteit inzichten kunnen krijgen.

+ Metrische gegevens om te bouwen om query's toestaan en verzamelen van gegevens over gebruikspatronen (query's tijdens kantooruren, indexering tijdens daluren). Deze gegevens gebruiken om te informeren over inrichting beslissingen voor de service. Hoewel het niet praktisch is op een per uur of dagelijks uitgebracht, kunt u partities en resources voor geplande wijzigingen in query-volumes dynamisch aanpassen. U aankan ook niet-geplande maar langdurige wijzigingen als niveaus lang genoeg bevatten voor actie te ondernemen.

+ Houd er rekening mee dat het enige nadeel van underprovisioning is dat u wellicht te moeten verbreken van een service als de daadwerkelijke vereisten groter dan de voorspellingen zijn. Als u wilt voorkomen dat de service wordt onderbroken, zou u een nieuwe service maken in hetzelfde abonnement op een hogere laag en deze naast elkaar uitvoeren totdat alle apps en aanvragen gericht op het nieuwe eindpunt.

## <a name="next-steps"></a>Volgende stappen

Beginnen met een gratis laag en een eerste index maken met behulp van een subset van uw gegevens om te begrijpen van de kenmerken ervan. De gegevensstructuur in Azure Search is een structuur omgekeerde index. De grootte en complexiteit van een omgekeerde index wordt bepaald door de inhoud. Houd er rekening mee dat maximaal redundante inhoud leidt doorgaans tot een kleinere-index dan maximaal onregelmatige inhoud. Zodat inhoud kenmerken in plaats van de grootte van de gegevensset index opslagvereisten bepalen.

Nadat u een eerste schatting van de indexgrootte van uw hebt [een factureerbare service inrichten](search-create-service-portal.md) op een van de lagen die in dit artikel worden besproken: Basic, Standard- of geoptimaliseerd voor opslag. Een kunstmatig opgelegde beperkingen gelden voor het formaat van de gegevens te versoepelen en [opnieuw opbouwen van uw index](search-howto-reindex.md) om op te nemen van de gegevens die u wilt worden doorzocht.

[Partities en replica's toewijzen](search-capacity-planning.md) indien nodig om op te halen van de prestaties en schaal die u nodig hebt.

Als prestaties en capaciteit fijn, bent u klaar. Anders, een search-service op een andere laag die meer nauw aansluit bij de behoeften van uw opnieuw maken.

> [!NOTE]
> Als u vragen hebt, naar boeken [StackOverflow](https://stackoverflow.com/questions/tagged/azure-search) of [Neem contact op met ondersteuning van Azure](https://azure.microsoft.com/support/options/).
