---
title: Wat is Azure Search | Microsoft Docs
description: Azure Search is een volledig beheerde gehoste cloud search-service. Meer informatie in het Functieoverzicht van deze.
services: search
manager: jhubbard
author: HeidiSteen
documentationcenter: 
ms.assetid: 50bed849-b716-4cc9-bbbc-b5b34e2c6153
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 11/10/2017
ms.author: heidist
ms.openlocfilehash: 2fa637b2119e55f0ad8a0aec3926df11871e7a2a
ms.sourcegitcommit: 176c575aea7602682afd6214880aad0be6167c52
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/09/2018
---
# <a name="what-is-azure-search"></a>Wat is Azure Search?
Azure Search is een search-as-a-service cloudoplossing die biedt ontwikkelaars API's en hulpprogramma's voor het toevoegen van een uitgebreide zoekervaring via uw inhoud in toepassingen met web, mobiel en enterprise.

Functionaliteit is beschikbaar gemaakt via een eenvoudige [REST-API](/rest/api/searchservice/) of [.NET SDK](search-howto-dotnet-sdk.md) die maskeert de intrinsieke complexiteit ophalen van informatie. Naast API's biedt de Azure portal beheer en inhoudbeheer ondersteunen, met hulpprogramma's voor het maken van een prototype en opvragen van de indexen. Omdat de service wordt uitgevoerd in de cloud, worden infrastructuur en beschikbaarheid beheerd door Microsoft.

<a name="feature-drilldown"></a>

## <a name="feature-summary"></a>Functieoverzicht

| Category | Functies |
|----------|----------|
|Zoekopdracht in volledige tekst en analyse van tekst | [Zoekopdracht in volledige tekst](search-lucene-query-architecture.md) is een primaire gebruiksvoorbeeld voor de meeste zoekopdrachten gebaseerde apps. Query's kunnen worden geformuleerd met een ondersteunde syntaxis. <br/><br/>[**Vereenvoudigde querysyntaxis** ](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search) biedt een logische operators, Zoekoperators woordgroep, achtervoegsel operators, operators voorrang.<br/><br/>[**Lucene-querysyntaxis** ](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search) alle bewerkingen in de eenvoudige syntaxis met uitbreidingen voor fuzzy zoeken, zoeken bij benadering, term versterking en reguliere expressies bevat.| 
| Gegevensintegratie | Azure Search-index accepteren gegevens uit een bron, mits dit als een JSON-gegevensstructuur is ingediend. <br/><br/> U kunt desgewenst voor ondersteunde gegevensbronnen in Azure, kunt u [ **indexeerfuncties** ](search-indexer-overview.md) automatisch verkennen [Azure SQL Database](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md), [Azure Cosmos DB](search-howto-index-cosmosdb.md), of [Azure Blob storage](search-howto-indexing-azure-blob-storage.md) de inhoud met uw primaire gegevensopslag van uw search-index te synchroniseren. Azure Blob-Indexeerfuncties kunnen uitvoeren *document kraken* voor [belangrijke bestandsindelingen indexeren](search-howto-indexing-azure-blob-storage.md), met inbegrip van Microsoft Office-, PDF- en HTML-documenten. |
| Taalkundige analyse | Analyzers zijn onderdelen die worden gebruikt voor verwerking tijdens het indexeren en de zoekcriteria operations tekst. Er zijn twee typen. <br/><br/>[**Aangepaste lexicale analyzers** ](https://docs.microsoft.com/rest/api/searchservice/custom-analyzers-in-azure-search) voor complexe zoekopdrachten met fonetische overeenkomende en reguliere expressies worden gebruikt. <br/><br/>[**Taalanalyse** ](https://docs.microsoft.com/rest/api/searchservice/language-support) van Lucene of Microsoft worden gebruikt voor het afhandelen van op intelligente wijze taalspecifieke linguistics werkwoordsvormen, geslacht, onregelmatige meervoud woorden (bijvoorbeeld ' muis' versus 'muizen'), word ongedaan wordt samengesteld, inclusief Woordafbreking (voor talen zonder spaties), en meer. |
| Geo-zoeken | Azure Search verwerkt, filters en geografische locaties weergegeven. Deze kan gebruikers op basis van de nabijheid van een fysieke locatie van een zoekresultaat gegevens verkennen. [Bekijk deze video](https://channel9.msdn.com/Shows/Data-Exposed/Azure-Search-and-Geospatial-Data) of [controleren van dit voorbeeld](https://github.com/Azure-Samples/search-dotnet-asp-net-mvc-jobs) voor meer informatie. |
| Functies voor de ervaring | [**Zoeksuggesties** ](https://docs.microsoft.com/rest/api/searchservice/suggesters) kan worden ingeschakeld voor automatisch aangevulde query's in een zoekbalk. Werkelijke documenten in uw index worden voorgesteld als gebruikers deelzoekopdrachten invoer invoeren. <br/><br/>[**Meervoudige navigatie** ](https://docs.microsoft.com/azure/search/search-faceted-navigation) wordt ingeschakeld via een enkele queryparameter. Azure Search retourneert een meervoudige navigatie-structuur die u als de code achter een categorieënlijst gebruiken kunt voor te filteren (bijvoorbeeld catalog om items te filteren op prijs bereik of merk). <br/><br/> [**Filters** ](https://docs.microsoft.com/rest/api/searchservice/odata-expression-syntax-for-azure-search) kan worden gebruikt voor het opnemen van meervoudige navigatie in de gebruikersinterface van uw toepassing, query formulering verbeteren en filteren op basis van gebruiker of ontwikkelaar opgegeven criteria. Maak filters met behulp van de OData-syntaxis.<br/><br/> [**Markeren** ](https://docs.microsoft.com/rest/api/searchservice/Search-Documents) tekstopmaak aan een overeenkomende trefwoord in de zoekresultaten van toepassing is. U kunt kiezen welke velden gemarkeerde codefragmenten retourneren.<br/><br/>[**Sorteren** ](https://docs.microsoft.com/rest/api/searchservice/Search-Documents) is voor meerdere velden via het indexschema dat wordt aangeboden en vervolgens tijdens de query met een enkele zoekparameter is uitgeschakeld.<br/><br/> [**Wisselgeheugengebruik** ](search-pagination-page-layout.md) en uw zoekresultaten beperking is eenvoudig met het nauwkeurig afgestemde besturingselement die Azure Search dan de zoekresultaten biedt.  
| Relevantie | [**Score berekenen voor eenvoudige** ](/rest/api/searchservice/add-scoring-profiles-to-a-search-index) is een belangrijk voordeel van Azure Search. Scoreprofiel profielen worden gebruikt als model voor relevantie als functie van de waarden in de documenten zelf. Bijvoorbeeld wellicht nieuwere producten of korting producten hoger in de zoekresultaten worden weergegeven. U kunt ook scoreprofiel profielen met de labels voor persoonlijke score berekenen op basis van de klant Zoekvoorkeuren u hebt gevolgd en opgeslagen afzonderlijk maken. |
| Controle en rapportage | [**Zoek traffic analytics** ](search-traffic-analytics.md) worden verzameld en geanalyseerd om te ontgrendelen inzicht in wat gebruikers in het zoekvak typt. <br/><br/>Metrische gegevens over query's per seconde, latentie en beperking worden vastgelegd en worden gerapporteerd in de portal-pagina's zonder aanvullende configuratie vereist. U kunt ook eenvoudig monitor index en document telt zodat u capaciteit naar wens kunt aanpassen. Zie voor meer informatie [Service-beheer](search-manage.md) |
| Hulpprogramma's voor het maken van een prototype en controle | In de portal kunt u de [ **wizard gegevens importeren** ](search-import-data-portal.md) indexeerfuncties, index-designer tegen een index, configureren en [ **Search explorer** ](search-explorer.md)scoreprofiel profielen verfijnen en testen van query's. U kunt ook een index om het schema weer te openen. |
| Infrastructuur | De **maximaal beschikbare platform** zorgt ervoor dat een zeer betrouwbare service zoekfunctie. Wanneer geschaald naar behoren [Azure Search biedt een SLA met 99,9%](https://azure.microsoft.com/support/legal/sla/search/v1_0/).<br/><br/> **Volledig beheerde en schaalbare** als een end-to-end-oplossing vereist Azure Search helemaal geen infrastructuurbeheer. Uw service kan worden aangepast aan uw behoeften door te schalen in twee dimensies voor het afhandelen van meer documentopslag, hogere querybelasting of beide.

## <a name="how-to-use-azure-search"></a>Het gebruik van Azure Search
### <a name="step-1-provision-service"></a>Stap 1: Inrichting service
U kunt draaien van een Azure Search-service in de [Azure-portal](https://portal.azure.com/) of via de [Azure Resource Management-API](/rest/api/searchmanagement/). U kunt de gratis service gedeeld met andere abonnees of een [betaald laag](https://azure.microsoft.com/pricing/details/search/) die dedicates bronnen alleen gebruikt door uw service. Voor betaalde lagen, kunt u een service in twee dimensies schalen: 

- Replica's voor het vergroten van de capaciteit voor het afhandelen van zware querybelasting toevoegen.   
- Partities groeien opslag voor meer documenten toevoegen. 

Door de opslag- en doorvoer document afzonderlijk verwerkt, kunt u kalibreren resourcing op basis van behoeften voor productie.

### <a name="step-2-create-index"></a>Stap 2: De index maken
Voordat u inhoud uploaden kunt, moet u eerst een Azure Search-index definiëren. Een index is vergelijkbaar met een databasetabel die uw gegevens bevat en zoekquery's kan accepteren. Definieert u het schema van de index om toe te wijzen om de structuur van de documenten die u zoeken wilt, vergelijkbaar met de velden in een database te weerspiegelen.

Een schema kan worden gemaakt in de Azure-portal of programmatisch met de [.NET SDK](search-howto-dotnet-sdk.md) of [REST-API](/rest/api/searchservice/).

### <a name="step-3-index-data"></a>Stap 3: Indexgegevens
Wanneer u een index definiëren, bent u klaar voor het uploaden van inhoud. U kunt een push of pull model gebruiken.

Het pull-model haalt gegevens uit externe gegevensbronnen. Het wordt ondersteund door *indexeerfuncties* die stroomlijnen en automatiseren aspecten van gegevensopname zoals verbinding maken met, lezen en gegevens serialiseren. [Indexeerfuncties](/rest/api/searchservice/Indexer-operations) beschikbaar zijn voor Azure Cosmos DB, Azure SQL Database, Azure Blob Storage en SQL Server die wordt gehost in een Azure VM. U kunt een indexeerfunctie voor configureren op de vraag of de geplande gegevensvernieuwing.

Het pushmodel is beschikbaar via de SDK of de REST-API's gebruikt voor het bijgewerkte documenten verzenden naar een index. U kunt pushen van gegevens vanaf vrijwel elke gegevensset met behulp van de JSON-indeling. Zie [toevoegen, bijwerken of verwijderen en documenten](/rest/api/searchservice/addupdate-or-delete-documents) of [het gebruik van de .NET SDK)](search-howto-dotnet-sdk.md) voor hulp bij het laden van gegevens.

### <a name="step-4-search"></a>Stap 4: zoeken
Na een index te vullen, kunt u [zoekquery's uitgeven](/rest/api/searchservice/Search-Documents) met uw service-eindpunt met behulp van eenvoudige HTTP-aanvragen met de REST-API of de .NET SDK.

## <a name="how-azure-search-compares"></a>Hoe Azure Search vergelijkt

Klanten wordt vaak vragen hoe Azure Search worden vergeleken met andere oplossingen zoekproblemen. De volgende tabel geeft een overzicht van belangrijke verschillen.

| Vergeleken met | Belangrijke verschillen |
|--|--|
|Bing | [Bing Web Search API](https://docs.microsoft.com/azure/cognitive-services/bing-web-search/) de indexen op Bing.com gezocht naar overeenkomende voorwaarden die u verzendt. Indexen zijn van HTML-, XML en andere webinhoud op openbare sites gebouwd. [Bing aangepaste zoekactie](https://docs.microsoft.com/azure/cognitive-services/bing-custom-search/) biedt dezelfde crawler technologie voor web inhoudstypen binnen het bereik van individuele websites.<br/><br/>Azure Search zoekt een index die u definieert, ingevuld met gegevens en documenten die u eigenaar bent, vaak uit verschillende bronnen. Azure Search heeft crawler mogelijkheden voor sommige gegevensbronnen via [indexeerfuncties](search-indexer-overview.md), maar u kunt elk JSON-document dat voldoet aan uw indexschema push in een enkele, geconsolideerde doorzoekbare resource. |
|Zoeken in database | [SQL Server zoekopdracht in volledige tekst](https://docs.microsoft.com/sql/relational-databases/search/full-text-search) is voor intern gebruik binnen het DBMS in SQL-tabellen inhoud. <br/><br/>Azure Search slaat de inhoud van heterogene bronnen en biedt speciale tekst verwerken functies zoals taalkundige en aangepaste analyse. De [zoekmachine voor volledige tekst](search-lucene-query-architecture.md) in Azure Search is gebouwd op Apache Lucene, een industriestandaard in informatie ophalen. <br/><br/>Resourcegebruik is een ander infecties punt. Zoeken in natuurlijke taal is vaak rekenkracht. Offloading zoeken om een oplossing voor toegewezen bewaart bronnen voor transactieverwerking. Door externalizing zoeken, kunt u eenvoudig scale zodat deze overeenkomen met de query volume aanpassen.|
|Zoekoplossing voor speciale | On premises of cloud zijn serviceoplossingen toegewezen zoekoplossingen met volledige spectrum functionaliteit. Zoeken technologieën doorgaans controle over het indexeren en de query pijplijnen bieden, en toegang tot uitgebreidere query positie en relevantie en functies voor zoeken in gericht en intelligente filteren syntaxis, kunt beheren. <br/><br/>U kunt specifieke zoekoplossingen wordt aangeboden als een cloud service of als een zelfstandige server gehost on-premises of op een virtuele machine vinden. Een cloudservice de juiste keuze is als u wilt een [directe oplossing met minimale overhead en onderhoud en aanpasbare schaal](#cloud-service-advantage). <br/><br/>Binnen het paradigma cloud bieden verschillende providers vergelijkbare basislijn-functies met zoeken in volledige tekst, geo-search en de mogelijkheid voor het afhandelen van een bepaalde mate van dubbelzinnigheid in invoer zoeken. Normaal gesproken is dit een [gespecialiseerde functie](#feature-drilldown), of het gemak en de algehele eenvoud van API's, hulpprogramma's en beheer waarmee wordt bepaald van het meest geschikt is. |

Tussen cloudproviders is de Azure Search sterkste voor volledige tekst zoeken werkbelastingen via inhoud archieven en databases op Azure, voor apps die afhankelijk voornamelijk van zoeken naar ophalen van gegevens en inhoud navigatie zijn. Belangrijkste voordelen zijn:

+ Azure gegevensintegratie (crawlers) op de indexing-laag
+ Azure-portal voor Centraal beheer
+ Azure schaal, betrouwbaarheid en beschikbaarheid van hoogwaardige
+ Taalkundige en aangepaste analyse, met analyzers voor zoekopdrachten in volledige tekst effen in 56 talen
+ [Core functies gemeen zoeken gericht apps](#feature-drilldown): score berekenen, facetten, suggesties, synoniemen, geo-search en meer.

> [!Note]
> Niet-Azure-gegevensbronnen worden volledig ondersteund, maar afhankelijk zijn van een meer code-intensieve push-methode in plaats van indexeerfuncties. U kunt een verzameling van de JSON-document naar een Azure Search-index met behulp van API's, overbrengen.

Tussen onze klanten zijn die gebruikmaken van een groot aantal functies in Azure Search online catalogussen line-of-business-programma's en toepassingen voor detectie van documenten.

## <a name="rest-api--net-sdk"></a>REST-API | .net SDK

Hoewel veel taken kunnen worden uitgevoerd in de portal, Azure Search bedoeld is voor ontwikkelaars die willen zoekfunctionaliteit integreren met bestaande toepassingen. De volgende API's zijn beschikbaar.

|Platform |Beschrijving |
|-----|------------|
|[REST](/rest/api/searchservice/) | HTTP-opdrachten die door alle programming platform en taal, met inbegrip van Xamarin, Java en JavaScript ondersteund|
|[.NET SDK](search-howto-dotnet-sdk.md) | .NET-wrapper voor de REST-API biedt efficiënte codering in C# en andere talen voor beheerde code die gericht is op .NET Framework |

## <a name="free-trial"></a>Gratis proefversie
Azure-abonnees kunnen [inrichten van een service in de laag gratis](search-create-service-portal.md).

Als u niet een abonnee, kunt u [gratis een Azure-account openen](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F). U ontvangt tegoed voor het uitproberen van betaalde Azure-services. Nadat ze zijn gebruikt, kunt u het account houden en gebruiken [gratis Azure-services](https://azure.microsoft.com/free/). Uw creditcard is nooit in rekening gebracht tenzij u expliciet de instellingen wijzigen en vragen kunnen worden gebracht.

U kunt ook [voordelen als MSDN-abonnee activeren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F): uw MSDN-abonnement ontvangt u elke maand tegoeden die u voor betaalde Azure-services kunt gebruiken. 

## <a name="how-to-get-started"></a>Hoe gaat u aan de slag

1. Maken van een service in de [gratis laag](search-create-service-portal.md).

2. Een of meer van de volgende zelfstudies doorlopen. 

  + [Het gebruik van de .NET SDK](search-howto-dotnet-sdk.md) ziet u de belangrijkste stappen in beheerde code.  
  + [Aan de slag met de REST-API](https://github.com/Azure-Samples/search-rest-api-getting-started) toont dezelfde stappen met de REST API.  
  + [Uw eerste index te maken in de portal](search-get-started-portal.md) met behulp van de ingebouwde functies voor indexering en prototype.   

Zoekmachines worden de algemene stuurprogramma's ophalen van informatie in mobiele apps, op het web en in bedrijf gegevensarchieven. Azure Search biedt u hulpprogramma's voor het maken van een zoekfunctie vergelijkbaar met die op grote commerciële websites.

In deze 9 minuten durende video van programmamanager Liam Cavanagh meer informatie over hoe het integreren van een zoekmachine uw app kunt profiteren. Korte demonstraties betrekking hebben op belangrijke functies in Azure Search en hoe een gangbare werkstroom eruit ziet. 

>[!VIDEO https://channel9.msdn.com/Events/Connect/2016/138/player]
 
+ 0-3 minuten dekt belangrijke functies en use cases.
+ 3 en 4 minuten bevat informatie over het inrichten van de service. 
+ 4-6 minuten bevat informatie over de wizard gegevens importeren naar een index maken met de ingebouwde vastgoed gegevensset gebruikt.
+ 6-9 minuten behandelt Search Explorer en verschillende query's.


