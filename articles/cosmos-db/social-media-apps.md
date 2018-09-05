---
title: 'Ontwerppatroon voor een Azure Cosmos DB: socialemedia-apps | Microsoft Docs'
description: Meer informatie over een ontwerppatroon voor sociale netwerken door gebruik te maken van de flexibiliteit van de opslag van Azure Cosmos DB en andere Azure-services.
keywords: socialemedia-apps
services: cosmos-db
author: ealsur
manager: kfile
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 06/27/2018
ms.author: maquaran
ms.openlocfilehash: 7925ef15dc7b3ce25ae919810a5ed2220184fe6e
ms.sourcegitcommit: cb61439cf0ae2a3f4b07a98da4df258bfb479845
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/05/2018
ms.locfileid: "43700840"
---
# <a name="going-social-with-azure-cosmos-db"></a>Socialiseren met Azure Cosmos DB
Opgenomen in een zeer onderling verbonden society betekent dat een bepaald moment in het leven u deel van uitmaken een **sociaal netwerk**. U sociale netwerken gebruiken om te Blijf op de hoogte vrienden, collega's, familie en soms om uw passie delen met mensen met een gemeenschappelijk belang.

Als de technici en ontwikkelaars, u mogelijk hebt zich afgevraagd hoe deze netwerken opslaan en uw gegevens, interconnect of mogelijk hebt zelfs belast om te maken of een nieuwe sociaal netwerk voor een specifieke specialistische markt uzelf ontwerpen. Als de grote vraag daartoe zich voordoet: hoe worden al deze gegevens opgeslagen?

Stel dat u een nieuwe en glanzend sociaal netwerk maakt, waar uw gebruikers artikelen met gerelateerde media, zoals afbeeldingen, video's of zelfs muziek kunnen plaatsen. Gebruikers kunnen opmerkingen over berichten en punten geven voor de classificaties. Er is een feed met berichten die gebruikers zien en kunnen werken met op de startpagina van de belangrijkste website. Dit complexe niet geluid (op het eerste), maar om het eenvoudig, te laten stoppen er (u kunt dieper ingaan op aangepaste kanalen op basis van relaties worden beïnvloed, maar deze groter is dan het doel van dit artikel).

Dus hoe slaat u deze en waar?

Veel van u mogelijk ervaring op SQL-databases of ten minste begrip [relationele gegevens modelleren](https://en.wikipedia.org/wiki/Relational_model) en u misschien geneigd om te beginnen met het tekenen er ongeveer als volgt:

![Diagram waarin u ziet een relatieve relationele model](./media/social-media-apps/social-media-apps-sql.png) 

Een perfect genormaliseerde en mooi gegevensstructuur... die niet schalen. 

Geen, krijgt u me gegaan, ik heb gewerkt om alle mijn leven met SQL-databases, ze goed zijn, maar de net als elk patroon, praktijk en software-platform, is het niet ideaal voor elk scenario.

Waarom SQL is niet om de beste keuze in dit scenario? We bekijken de structuur van een enkel bericht als ik wil dit item weergeven in een website of toepassing, moet ik een query uitvoeren met... Acht tabelsamenvoegingen (!) alleen om één enkel bericht, nu, afbeelding een stream van berichten die dynamisch worden geladen en worden weergegeven op het scherm en u ziet waar ben ik weer te geven.

U kunt, natuurlijk, een enorme SQL-exemplaar gebruiken met voldoende stroom voor het oplossen van duizenden query's met te veel koppelingen voor het bieden van uw inhoud, maar echt, waarom zou u, wanneer er bestaat een eenvoudigere oplossing?

## <a name="the-nosql-road"></a>De weg NoSQL
In dit artikel begeleidt u bij het modelleren van gegevens van uw sociale platform met NoSQL-database van Azure [Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) een rendabele methode terwijl u gebruikmaakt van andere Azure Cosmos DB functies zoals de [Gremlin Gremlin-API ](../cosmos-db/graph-introduction.md). Met behulp van een [NoSQL](https://en.wikipedia.org/wiki/NoSQL) benadering voor het opslaan van gegevens, JSON-indeling en toe te passen [denormalisatie](https://en.wikipedia.org/wiki/Denormalization), het eerder gecompliceerd bericht kan worden omgezet naar één [Document](https://en.wikipedia.org/wiki/Document-oriented_database):


    {
        "id":"ew12-res2-234e-544f",
        "title":"post title",
        "date":"2016-01-01",
        "body":"this is an awesome post stored on NoSQL",
        "createdBy":User,
        "images":["http://myfirstimage.png","http://mysecondimage.png"],
        "videos":[
            {"url":"http://myfirstvideo.mp4", "title":"The first video"},
            {"url":"http://mysecondvideo.mp4", "title":"The second video"}
        ],
        "audios":[
            {"url":"http://myfirstaudio.mp3", "title":"The first audio"},
            {"url":"http://mysecondaudio.mp3", "title":"The second audio"}
        ]
    }

En met een eenvoudige query uitvoeren, en er zijn geen joins kunnen worden verkregen. Dit is nog veel meer eenvoudig en snel en budget-wise, deze scan vereist minder resources om een beter resultaat te verkrijgen.

Azure Cosmos DB zorgt ervoor dat alle eigenschappen worden geïndexeerd met de automatische indexering die kunnen ook worden [aangepaste](indexing-policies.md). De aanpak van schema's kan we het opslaan van documenten met verschillende en dynamische structuren, misschien morgen die u wilt dat berichten naar een lijst met categorieën of die zijn gekoppeld aan deze hashtags, Cosmos DB zorgt de nieuwe documenten met de toegevoegde kenmerken zonder extra werk vereist door ons.

Opmerkingen over een item kunnen worden behandeld als andere berichten met een bovenliggende eigenschap (Dit vereenvoudigt de objecttoewijzing). 

    {
        "id":"1234-asd3-54ts-199a",
        "title":"Awesome post!",
        "date":"2016-01-02",
        "createdBy":User2,
        "parent":"ew12-res2-234e-544f"
    }

    {
        "id":"asd2-fee4-23gc-jh67",
        "title":"Ditto!",
        "date":"2016-01-03",
        "createdBy":User3,
        "parent":"ew12-res2-234e-544f"
    }

En alle sociale interacties kunnen worden opgeslagen op een afzonderlijk object als items:

    {
        "id":"dfe3-thf5-232s-dse4",
        "post":"ew12-res2-234e-544f",
        "comments":2,
        "likes":10,
        "points":200
    }

Het maken van feeds is alleen een kwestie van het maken van documenten die een lijst van de bericht-id's met een bepaalde relevantie order kunnen bevatten:

    [
        {"relevance":9, "post":"ew12-res2-234e-544f"},
        {"relevance":8, "post":"fer7-mnb6-fgh9-2344"},
        {"relevance":7, "post":"w34r-qeg6-ref6-8565"}
    ]

Kan er een 'laatste' stream met berichten die zijn geordend op datum gemaakt, een 'gegevensbeheer' stream met deze berichten met meer leuk vinden in de afgelopen 24 uur, u ook een aangepaste stroom voor elke gebruiker op basis van logica, zoals Volgers en interesses kan implementeren en het is nog steeds een lijst met  berichten. Er is een kwestie van het bouwen van deze lijsten, maar de prestaties lezen concurrerende blijft. Wanneer u een van deze lijsten aanschaft, u één query verlenen aan het Cosmos DB met behulp van de [IN de operator](sql-api-sql-query.md#WhereClause) om op te halen van pagina's van berichten op een tijdstip.

De feed stromen kunnen worden gebouwd met behulp van [Azure App Services](https://azure.microsoft.com/services/app-service/) processen op de achtergrond: [Webjobs](../app-service/web-sites-create-web-jobs.md). Zodra een bericht wordt gemaakt, verwerking op de achtergrond kan worden geactiveerd met behulp van [Azure Storage](https://azure.microsoft.com/services/storage/) [wachtrijen](../storage/queues/storage-dotnet-how-to-use-queues.md) en Webjobs geactiveerd met behulp van de [Azure Webjobs SDK](https://github.com/Azure/azure-webjobs-sdk/wiki), uitvoering de boeken doorgifte in stromen op basis van uw eigen aangepaste logica. 

Punten en likes via een bericht kunnen worden verwerkt op een uitgestelde manier met behulp van dezelfde methode om een uiteindelijke consistente omgeving te maken.

Volgers zijn lastiger. Cosmos DB heeft een limiet van maximale document en grote documenten lezen/schrijven kan invloed hebben op de schaalbaarheid van uw toepassing. U vindt dit Volgers opslaan als een document met deze structuur:

    {
        "id":"234d-sd23-rrf2-552d",
        "followersOf": "dse4-qwe2-ert4-aad2",
        "followers":[
            "ewr5-232d-tyrg-iuo2",
            "qejh-2345-sdf1-ytg5",
            //...
            "uie0-4tyg-3456-rwjh"
        ]
    }

Dit werkt mogelijk voor een gebruiker met een paar duizend Volgers, maar als de posities, deze benadering wordt lid wordt van sommige beroemdheden leiden tot een grote documentgrootte en uiteindelijk de limiet van de grootte van document mogelijk bereikt.

U kunt een gemengde benadering gebruiken om op te lossen dit. U kunt het aantal Volgers opslaan als onderdeel van het document gebruikersstatistieken:

    {
        "id":"234d-sd23-rrf2-552d",
        "user": "dse4-qwe2-ert4-aad2",
        "followers":55230,
        "totalPosts":452,
        "totalPoints":11342
    }

En de werkelijke grafiek van Volgers kan worden opgeslagen met Azure Cosmos DB [Gremlin-API](../cosmos-db/graph-introduction.md), maken [hoekpunten](http://mathworld.wolfram.com/GraphVertex.html) voor elke gebruiker en [randen](http://mathworld.wolfram.com/GraphEdge.html) die de "A-volgt-B' onderhouden relaties. De Gremlin-API laten we u niet alleen de Volgers van een bepaalde gebruiker ophalen maar meer complexe query's om voor te stellen, zelfs de mensen in algemene maken. Als u aan de grafiek de categorieën van de inhoud die mensen zoals of geniet toevoegt, kun u weven ervaringen met slimme vinden van inhoud, geen inhoud mag voorstellen die u als volgt of zoeken naar personen met wie u mogelijk veel gemeen hebben.

Het document gebruikersstatistieken kan nog steeds worden gebruikt om te maken van kaarten in de gebruikersinterface of snelle profiel previews.

## <a name="the-ladder-pattern-and-data-duplication"></a>De 'Ladder'-patroon en gegevens duplicatie
Als u mogelijk al hebt gezien in de JSON-document dat verwijst naar een bericht, zijn er meerdere exemplaren van een gebruiker. En u zou hebben geraden rechts, betekent dit dat de informatie die staat voor een gebruiker, krijgt deze denormalisatie aanwezig zijn op meer dan één locatie kan zijn.

Als u wilt toestaan voor snellere query's, in rekening worden gebracht gegevensontdubbeling. Het probleem met dit neveneffect is dat als door sommige wijzigingen van de gegevens van een gebruiker, moet u alle activiteiten vinden hij ooit hebt en ze alle bijwerken. Geen geluid praktische, rechts?

U gaat op te lossen door de id-kenmerken van de sleutel van een gebruiker die u in uw toepassing voor elke activiteit weergeven. Als u visueel weergeven van een bericht in uw toepassing en alleen de maker van de naam en afbeelding wilt weergeven, waarom u alle gegevens van de gebruiker opgeslagen in het kenmerk 'createdBy'? Voor elke opmerking u te laten zien van de gebruiker foto, hoeft u nog niet precies de rest van deze informatie. Dat is waar iets Roep ik het patroon"Ladder' wordt geleverd aan de orde.

We gaan gebruikersgegevens als een voorbeeld:

    {
        "id":"dse4-qwe2-ert4-aad2",
        "name":"John",
        "surname":"Doe",
        "address":"742 Evergreen Terrace",
        "birthday":"1983-05-07",
        "email":"john@doe.com",
        "twitterHandle":"\@john",
        "username":"johndoe",
        "password":"some_encrypted_phrase",
        "totalPoints":100,
        "totalPosts":24
    }

Door deze informatie bekijkt, kunt u snel die essentiële informatie en die niet is, waardoor het maken van een 'Ladder' detecteren:

![Diagram van een patroon ladder](./media/social-media-apps/social-media-apps-ladder.png)

De kleinste stap een UserChunk, de minimale stukje informatie waarmee een gebruiker wordt genoemd en deze wordt gebruikt voor het kopiëren van gegevens. Door de grootte van de gedupliceerde gegevens beperken tot alleen de informatie die u zal 'show', moet u de mogelijkheid om enorme updates verminderen.

De middelste stap wordt de gebruiker genoemd, is de volledige gegevens die zullen worden gebruikt op de meeste afhankelijk van de prestaties van query's op Cosmos DB, de meest gebruikte en kritieke. Deze bevat de informatie die wordt vertegenwoordigd door een UserChunk.

De grootste waarde is van de gebruiker uitgebreid. Dit omvat alle kritieke gebruikersgegevens plus andere gegevens die niet echt nodig hebt om snel worden gelezen of gebruik de uiteindelijke (zoals de aanmeldingsprocedure) is. Deze gegevens kan worden opgeslagen buiten Cosmos DB in Azure SQL Database of Azure Storage-tabellen.

Waarom zou u de gebruiker splitsen en ook deze informatie op te slaan op verschillende plaatsen? Omdat vanuit een oogpunt van prestaties, hoe groter de documenten, het costlier query's. Documenten dun, met de juiste informatie naar alle uw afhankelijk van de prestaties van query's voor uw sociale netwerken en opslaan van de extra informatie voor uiteindelijke scenario's zoals, volledig profiel bewerkt, aanmeldingen, gegevensanalyse voor gebruiksanalyse en Big Data, zelfs bewaren initiatieven. U echt niet zo belangrijk als het verzamelen van gegevens voor gegevensanalyse is langzamer, omdat deze wordt uitgevoerd op Azure SQL Database, u hebben betrekking op echter dat uw gebruikers een snelle en slanke ervaring hebben. Een gebruiker, die zijn opgeslagen op Cosmos DB, zou er als volgt:

    {
        "id":"dse4-qwe2-ert4-aad2",
        "name":"John",
        "surname":"Doe",
        "username":"johndoe"
        "email":"john@doe.com",
        "twitterHandle":"\@john"
    }

En een bericht zou er als volgt uitzien:

    {
        "id":"1234-asd3-54ts-199a",
        "title":"Awesome post!",
        "date":"2016-01-02",
        "createdBy":{
            "id":"dse4-qwe2-ert4-aad2",
            "username":"johndoe"
        }
    }

En wanneer een bewerking zich waar een van de kenmerken van het segment wordt beïnvloed voordoet, is het gemakkelijk te vinden van de betreffende documenten met behulp van query's die naar de geïndexeerde kenmerken verwijzen (Selecteer * FROM berichten p waar p.createdBy.id == "edited_user_id") en vervolgens de segmenten bij te werken.

## <a name="the-search-box"></a>Het zoekvak
Gebruikers wordt gelukkig veel inhoud is gegenereerd. En u moet kunnen bieden de mogelijkheid om te zoeken en te zoeken naar inhoud die mogelijk niet rechtstreeks in hun inhoud stromen, misschien omdat u niet de makers volgen, of misschien u alleen wilt zoeken dat oude post u zes maanden geleden hebt.

Gelukkig heeft de fout en omdat u van Azure Cosmos DB gebruikmaakt, kunt u eenvoudig implementeren een search-engine met [Azure Search](https://azure.microsoft.com/services/search/) in enkele minuten en zonder één regel code (andere dan natuurlijk, de zoekactie en gebruikersinterface) te typen.

Waarom is dit dus eenvoudig?

Azure Search worden geïmplementeerd wat ze bellen [indexeerfuncties](https://msdn.microsoft.com/library/azure/dn946891.aspx), achtergrondprocessen die haakje in uw gegevensopslagplaatsen en automagically toevoegen, bijwerken of verwijderen van uw objecten in de indexen. Deze ondersteuning bieden voor een [indexeerfuncties van Azure SQL Database](https://blogs.msdn.microsoft.com/kaevans/2015/03/06/indexing-azure-sql-database-with-azure-search/), [Azure Blobs indexeerfuncties](../search/search-howto-indexing-azure-blob-storage.md) en gelukkig [indexeerfuncties van Azure Cosmos DB](../search/search-howto-index-documentdb.md). De overgang van informatie van Cosmos DB in Azure Search is vrij eenvoudig, als beide gegevens opslaan in JSON-indeling, hoeft u alleen te [maken van uw Index](../search/search-create-index-portal.md) en welke kenmerken uit uw documenten die u indexeren wilt, toewijzen en dat is alles, in een paar minuten (afhankelijk van de grootte van uw gegevens), alle inhoud kan worden doorzocht, door de beste oplossing voor Search-as-a-Service in de cloudinfrastructuur. 

Voor meer informatie over Azure Search, vindt u de [Hitchhiker van handleiding voor het zoeken](https://blogs.msdn.microsoft.com/mvpawardprogram/2016/02/02/a-hitchhikers-guide-to-search/).

## <a name="the-underlying-knowledge"></a>De onderliggende kennis
Na deze inhoud die meegroeit en wordt elke dag op te slaan, is het wellicht denken: wat kan ik doen met deze stroom van gegevens van mijn gebruikers?

Het antwoord is vrij eenvoudig: plaats om te werken en leer van deze.

Maar wat kunt u leren? Enkele eenvoudige voorbeelden zijn onder meer [sentimentanalyse](https://en.wikipedia.org/wiki/Sentiment_analysis), inhoud aanbevelingen op basis van gebruikersvoorkeuren of zelfs een geautomatiseerde content moderator die ervoor zorgt dat alle inhoud die zijn gepubliceerd door uw sociale netwerk veilig voor de familie is.

Nu dat ik kreeg u aangesloten, denkt u waarschijnlijk na moet u enkele PhD in de wetenschap math om op te halen van deze patronen en informatie uit de eenvoudige databases en -bestanden, maar u zou zijn gegaan.

[Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/), onderdeel van de [Cortana Intelligence Suite](https://social.technet.microsoft.com/wiki/contents/articles/36688.introduction-to-cortana-intelligence-suite.aspx), is een volledig beheerde cloudservice waarmee u werkstromen maken middels algoritmen in een eenvoudige interface met slepen en neerzetten, uw eigen algoritmen in code[ R](https://en.wikipedia.org/wiki/R_\(programming_language\)) of u sommige van de al zijn gemaakt en klaar voor gebruik van API's, zoals: [Tekstanalyse](https://gallery.cortanaanalytics.com/MachineLearningAPI/Text-Analytics-2), [Content Moderator of [aanbevelingen](https://gallery.azure.ai/Solution/Recommendations-Solution).

Voor het bereiken van een van deze scenario's voor Machine Learning, kunt u [Azure Data Lake](https://azure.microsoft.com/services/data-lake-store/) het opnemen van de gegevens uit verschillende bronnen en gebruiken van [U-SQL](https://azure.microsoft.com/documentation/videos/data-lake-u-sql-query-execution/) voor het verwerken van de informatie en een uitvoer die kan worden gegenereerd door Azure Machine Learning worden verwerkt.

Een andere optie beschikbaar is met [Microsoft Cognitive Services](https://www.microsoft.com/cognitive-services) voor het analyseren van uw gebruikers inhoud; niet alleen kunt u ze beter begrijpen (via het analyseren van schrijven met [Tekstanalyse-API](https://www.microsoft.com/cognitive-services/en-us/text-analytics-api)), maar u kunt ook ongewenste of volwassen inhoud detecteren en dienovereenkomstig kan reageren met [Computer Vision-API](https://www.microsoft.com/cognitive-services/en-us/computer-vision-api). Cognitive Services bevat veel out-of-the-box-oplossingen waarvoor niet alle soorten Machine Learning kennis wilt gebruiken.

## <a name="a-planet-scale-social-experience"></a>Een sociale ervaring op wereldwijde schaal
Er is een laatste, maar niet in het minst belangrijk artikel ik moet rekening houden: **schaalbaarheid**. Bij het ontwerpen van een architectuur is het essentieel dat elk onderdeel in een eigen, ofwel schalen kunt omdat u nodig hebt om meer gegevens te verwerken of omdat u wilt een grotere geografische dekking (of beide!). Gelukkig heeft de fout te bereiken een complexe taak is een **kant en klare ervaring** met Cosmos DB.

Cosmos DB biedt ondersteuning voor [dynamische partitioneren](https://azure.microsoft.com/blog/10-things-to-know-about-documentdb-partitioned-collections/) out-of-the-box door het automatisch maken van partities op basis van een bepaalde **partitiesleutel** (gedefinieerd als een van de kenmerken van uw documenten). De juiste partitiesleutel moet worden uitgevoerd tijdens de ontwerpfase te definiëren en waarmee u rekening moet houden de [aanbevolen procedures](../cosmos-db/partition-data.md#designing-for-partitioning) beschikbaar; in het geval van een sociale-ervaring, uw strategie voor partitioneren moet worden uitgelijnd met de manier waarop u query's (leesbewerkingen binnen hetzelfde uitvoeren partitie wenselijk zijn) en schrijven ("hotspots" voorkomen door te spreiden schrijfbewerkingen in meerdere partities). Sommige opties zijn: partities op basis van een tijdelijke sleutel (dag/maand/week), op inhoud categorie, per geografische regio, door gebruiker. Alles echt afhankelijk van hoe u de gegevens op te vragen en geef deze weer in uw sociale-ervaring. 

Een interessante opgemerkt is dat uw query's worden uitgevoerd door Cosmos DB (met inbegrip van [statistische functies](https://azure.microsoft.com/blog/planet-scale-aggregates-with-azure-documentdb/)) voor alle partities in uw transparant, u hoeft niet alle logica toevoegen als uw gegevens groeien.

Met de tijd, wordt uiteindelijk groter wordt in het verkeer en het gebruik van resources (gemeten in [ru's](request-units.md), of Aanvraageenheden) wordt verhoogd. U leest en schrijft vaker naarmate uw gebruikersgroep zich uitbreidt en ze maken en lezen meer inhoud; wordt gestart de mogelijkheid van **schaal de doorvoer van uw** is van vitaal belang. Verhoging van uw ru's is eenvoudig, u kunt dit doen met een paar klikken op de Azure-portal of door [uitgeven van opdrachten via de API](https://docs.microsoft.com/rest/api/cosmos-db/replace-an-offer).

![Omhoog schalen en het definiëren van een partitiesleutel](./media/social-media-apps/social-media-apps-scaling.png)

Wat gebeurt er als dingen steeds beter en gebruikers van een andere regio, land/regio of continent, ziet u uw platform en gaan gebruiken, wat een geweldige verrassing!

Maar geduld... u binnenkort profiteren van hun ervaring met uw platform is niet optimaal; ze zijn tot nu toe van uw operationele regio dat de latentie verschrikkelijke, en u natuurlijk niet wilt dat ze om af te sluiten. Als er slechts er een eenvoudige manier van is **uitbreiden van uw wereldwijde bereik**..., maar er is!

Cosmos DB kunt u [uw gegevens globaal repliceren](../cosmos-db/tutorial-global-distribution-sql-api.md) en transparant met een paar muisklikken en automatisch kiezen uit de beschikbare regio's van uw [clientcode](../cosmos-db/tutorial-global-distribution-sql-api.md). Dit betekent ook dat u kunt hebben [meerdere regio's voor failover-](regional-failover.md). 

Wanneer u uw gegevens globaal repliceren, moet u om ervoor te zorgen dat uw klanten van deze profiteren kunnen. Als u van een web-front gebruikmaakt of toegang tot API's van mobiele clients, kunt u implementeren [Azure Traffic Manager](https://azure.microsoft.com/services/traffic-manager/) en kloon van uw Azure App Service op de gewenste regio's met behulp van een prestatieconfiguratie voor de ondersteuning van uw uitgebreide globale dekking. Wanneer uw clients toegang krijgen de front-end- of -API's tot, worden ze worden doorgestuurd naar de dichtstbijzijnde App Service, die op zijn beurt maakt verbinding met de lokale replica van de Cosmos DB.

![Wereldwijde dekking toe te voegen aan uw sociale platform](./media/social-media-apps/social-media-apps-global-replicate.png)

## <a name="conclusion"></a>Conclusie
In dit artikel wordt geprobeerd om helderheid te krijgen sommige in de alternatieven voor sociale netwerken volledig op Azure maken met services van de lage kosten en prachtige resultaten door het bevorderen van het gebruik van een meerlaagse storage-oplossing en gegevens distributie 'Ladder' genoemd.

![Diagram van de interactie tussen Azure-services voor sociale netwerken](./media/social-media-apps/social-media-apps-azure-solution.png)

Het gaat erom dat er geen silver opsommingsteken voor dit soort scenario's, is de synergie die zijn gemaakt door de combinatie van de geweldige services die we kunnen het bouwen van geweldige ervaringen: de snelheid en de vrijheid van Azure Cosmos DB voor een geweldige sociale, toepassing, de Intelligence achter een eersteklas search-oplossing, zoals Azure Search, de flexibiliteit van Azure App Services voor het hosten van toepassingen niet zelfs taal-neutraal, maar krachtige achtergrondprocessen en de uitbreidbare Azure Storage en Azure SQL Database voor het opslaan van enorme hoeveelheden gegevens en de analytische kracht van Azure Machine Learning te maken van de kennis en intelligentie die kan feedback geven op uw processen en Help ons te bieden de juiste inhoud naar de juiste gebruikers.

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over gebruiksvoorbeelden voor Cosmos DB, [algemene Cosmos DB use-cases](use-cases.md).
