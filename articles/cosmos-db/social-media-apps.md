---
title: 'Ontwerppatroon voor een Azure Cosmos DB: socialemedia-apps'
description: Meer informatie over een ontwerppatroon voor sociale netwerken door gebruik te maken van de flexibiliteit van de opslag van Azure Cosmos DB en andere Azure-services.
keywords: socialemedia-apps
services: cosmos-db
author: ealsur
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 11/14/2018
ms.author: maquaran
ms.openlocfilehash: 669cfdc59fc0b2f509db704afa4867d8f55d86f8
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2018
ms.locfileid: "53083968"
---
# <a name="going-social-with-azure-cosmos-db"></a>Socialiseren met Azure Cosmos DB

Opgenomen in een zeer onderling verbonden society betekent dat een bepaald moment in het leven u deel van uitmaken een **sociaal netwerk**. U sociale netwerken gebruiken om te Blijf op de hoogte vrienden, collega's, familie en soms om uw passie delen met mensen met een gemeenschappelijk belang.

Als de technici en ontwikkelaars, u mogelijk hebt zich afgevraagd hoe deze netwerken opslaan en interconnect van uw gegevens. Of u mogelijk hebt zelfs zijn binnen de perken blijven om te maken of een nieuwe sociaal netwerk voor een specifieke specialistische markt ontwerpen. Als de grote vraag daartoe zich voordoet: hoe worden al deze gegevens opgeslagen?

Stel dat u maakt een nieuwe en glanzend sociaal netwerk waar uw gebruikers artikelen met gerelateerde media, zoals afbeeldingen, video's of zelfs muziek kunnen plaatsen. Gebruikers kunnen opmerkingen over berichten en punten geven voor de classificaties. Er is een feed met berichten die gebruikers zien en ermee op de startpagina van de belangrijkste website. Deze methode niet geluid complexe op eerst, maar om het eenvoudig te, gaan we daar niet. (U kunt dieper ingaan op aangepaste kanalen op basis van relaties worden beïnvloed, maar dit gaat verder dan het doel van dit artikel.)

Dus hoe slaat u deze gegevens en waar?

Kan u ervaring hebt op SQL-databases of een notatie van [relationele gegevens modelleren](https://en.wikipedia.org/wiki/Relational_model). U kunt starten tekenen er ongeveer als volgt:

![Diagram waarin u ziet een relatieve relationele model](./media/social-media-apps/social-media-apps-sql.png)

Een perfect genormaliseerde en mooi gegevensstructuur... die niet worden geschaald.

Geen, krijgt u me onjuist is, dat ik heb gewerkt om alle mijn leven met SQL-databases. Ze zijn goed, maar net als elk patroon, praktijk en software-platform, is het niet ideaal voor elk scenario.

Waarom SQL is niet om de beste keuze in dit scenario? Bekijk de structuur van een enkel bericht in. Als ik wil het bericht weergeven in een website of toepassing, moet ik kan een query met... lid wordt van acht tables(!) alleen als u wilt weergeven van een enkel bericht. Afbeelding nu een stream van berichten die dynamisch worden geladen en worden weergegeven op het scherm, en u ziet waar ik ben nu.

U kunt een enorme SQL-exemplaar gebruiken met voldoende stroom voor het oplossen van duizenden query's met veel koppelingen voor het bieden van uw inhoud. Maar waarom zou u, wanneer er een eenvoudigere oplossing bestaat?

## <a name="the-nosql-road"></a>De weg NoSQL

In dit artikel begeleidt u bij het modelleren van gegevens van uw sociale platform met NoSQL-database van Azure [Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) voordelig. Deze ook wordt uitgelegd hoe u het gebruik van andere Azure Cosmos DB-functies, zoals de [Gremlin-API](../cosmos-db/graph-introduction.md). Met behulp van een [NoSQL](https://en.wikipedia.org/wiki/NoSQL) benadering voor het opslaan van gegevens, JSON-indeling en toe te passen [denormalisatie](https://en.wikipedia.org/wiki/Denormalization), het eerder gecompliceerd bericht kan worden omgezet naar één [Document](https://en.wikipedia.org/wiki/Document-oriented_database):

    {
        "id":"ew12-res2-234e-544f",
        "title":"post title",
        "date":"2016-01-01",
        "body":"this is an awesome post stored on NoSQL",
        "createdBy":User,
        "images":["https://myfirstimage.png","https://mysecondimage.png"],
        "videos":[
            {"url":"https://myfirstvideo.mp4", "title":"The first video"},
            {"url":"https://mysecondvideo.mp4", "title":"The second video"}
        ],
        "audios":[
            {"url":"https://myfirstaudio.mp3", "title":"The first audio"},
            {"url":"https://mysecondaudio.mp3", "title":"The second audio"}
        ]
    }

En kan worden verkregen met een eenvoudige query uitvoeren, en er zijn geen joins. Deze query is veel eenvoudig en snel en budget-wise, deze scan vereist minder resources om een beter resultaat te verkrijgen.

Azure Cosmos DB zorgt ervoor dat alle eigenschappen met de automatische indexering worden geïndexeerd. De automatische indexering kan ook worden [aangepaste](index-policy.md). De aanpak van schema's kan we het opslaan van documenten met verschillende en dynamische structuren. Misschien morgen u wilt dat berichten naar een lijst met categorieën of hashtags die zijn gekoppeld aan deze? Cosmos DB wordt de nieuwe documenten met de toegevoegde kenmerken zonder extra werk vereist is door ons verwerkt.

Opmerkingen over een item kunnen worden behandeld als andere berichten met een bovenliggende eigenschap. (Deze procedure kunt u uw objecttoewijzing eenvoudiger.)

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

Het maken van feeds is alleen een kwestie van het maken van documenten die een lijst met boeken id's met een bepaalde relevantie order kunnen bevatten:

    [
        {"relevance":9, "post":"ew12-res2-234e-544f"},
        {"relevance":8, "post":"fer7-mnb6-fgh9-2344"},
        {"relevance":7, "post":"w34r-qeg6-ref6-8565"}
    ]

U kunt een 'laatste' stream hebben met berichten die zijn geordend op datum gemaakt. Of u kunt een 'meest belaste' stream met deze berichten met meer likes in de afgelopen 24 uur hebben. U kunt zelfs een aangepaste stroom voor elke gebruiker op basis van logica, zoals Volgers en interesses implementeren. Het is nog steeds een lijst met berichten. Er is een kwestie van het bouwen van deze lijsten, maar de prestaties lezen concurrerende blijft. Wanneer u een van deze lijsten aanschaft, u één query verlenen aan het Cosmos DB met behulp van de [IN de operator](how-to-sql-query.md#WhereClause) om op te halen van pagina's van berichten op een tijdstip.

De feed stromen kunnen worden gebouwd met behulp van [Azure App Services](https://azure.microsoft.com/services/app-service/) processen op de achtergrond: [Webjobs](../app-service/web-sites-create-web-jobs.md). Zodra een bericht wordt gemaakt, verwerking op de achtergrond kan worden geactiveerd met behulp van [Azure Storage](https://azure.microsoft.com/services/storage/) [wachtrijen](../storage/queues/storage-dotnet-how-to-use-queues.md) en Webjobs geactiveerd met behulp van de [Azure Webjobs SDK](https://github.com/Azure/azure-webjobs-sdk/wiki), uitvoering de boeken doorgifte in stromen op basis van uw eigen aangepaste logica.

Punten en likes via een bericht kunnen worden verwerkt op een uitgestelde manier met behulp van dezelfde methode om een uiteindelijke consistente omgeving te maken.

Volgers zijn lastiger. Cosmos DB heeft een limiet van het document en grote documenten lezen/schrijven kan invloed hebben op de schaalbaarheid van uw toepassing. U vindt dit Volgers opslaan als een document met deze structuur:

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

Deze structuur kan werken voor een gebruiker met een paar duizend Volgers. Als sommige beroemdheden lid wordt van de posities, echter deze benadering zal leiden tot een grote documentgrootte, en het uiteindelijk de limiet van de grootte van document mogelijk bereikt.

U lost dit probleem, kunt u een gemengde benadering. U kunt het aantal Volgers opslaan als onderdeel van het document gebruikersstatistieken:

    {
        "id":"234d-sd23-rrf2-552d",
        "user": "dse4-qwe2-ert4-aad2",
        "followers":55230,
        "totalPosts":452,
        "totalPoints":11342
    }

U kunt de werkelijke grafiek van Volgers met Azure Cosmos DB opslaan [Gremlin-API](../cosmos-db/graph-introduction.md) maken [hoekpunten](http://mathworld.wolfram.com/GraphVertex.html) voor elke gebruiker en [randen](http://mathworld.wolfram.com/GraphEdge.html) die de "A-volgt-B' onderhouden relaties. Met de Gremlin-API, kunt u de Volgers van een bepaalde gebruiker ophalen en maken van meer complexe query's uit om mensen in algemene voorstellen. Als u aan de grafiek de categorieën van de inhoud die mensen zoals of geniet toevoegt, kun u weven ervaringen met slimme inhoudsdetectie, stellen inhoud dat deze personen die u als volgt of zoeken naar personen die u mogelijk veel met hetzelfde.

Het document gebruikersstatistieken kan nog steeds worden gebruikt om te maken van kaarten in de gebruikersinterface of snelle profiel previews.

## <a name="the-ladder-pattern-and-data-duplication"></a>De 'Ladder'-patroon en gegevens duplicatie

Als u mogelijk al hebt gezien in de JSON-document dat verwijst naar een bericht, zijn er veel exemplaren van een gebruiker. En u zou hebben geraden rechts, dat deze duplicaten betekenen dat de gegevens die worden beschreven van een gebruiker, krijgt deze denormalisatie mogelijk aanwezig zijn op meer dan één locatie.

Als u wilt toestaan voor snellere query's, in rekening worden gebracht gegevensontdubbeling. Het probleem met dit neveneffect is dat als door sommige wijzigingen van de gegevens van een gebruiker, moet u alle activiteiten vinden en de gebruiker ooit heeft ze alle bijwerken. Geen geluid praktische, rechts?

U gaat met het vaststellen van de belangrijkste kenmerken van een gebruiker die u in uw toepassing voor elke activiteit weergeeft op te lossen. Als u visueel weergeven van een bericht in uw toepassing en alleen de maker van de naam en afbeelding wilt weergeven, waarom u alle gegevens van de gebruiker opgeslagen in het kenmerk 'createdBy'? Als u voor elke opmerking dat u afbeelding van de gebruiker te laten zien, hoeft u niet zeker de rest van de gegevens van de gebruiker. Dat is waar iets Roep ik het patroon"Ladder' wordt betrokken.

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

De kleinste stap een UserChunk, de minimale stukje informatie waarmee een gebruiker wordt genoemd en deze wordt gebruikt voor het kopiëren van gegevens. De gedupliceerde gegevens door grootte te beperken tot alleen de informatie die u zult 'show', moet u de mogelijkheid om enorme updates verminderen.

De middelste stap wordt de gebruiker genoemd. Het is de volledige gegevens die zullen worden gebruikt op de meeste afhankelijk van de prestaties van query's op Cosmos DB, de meest gebruikte en kritieke. Deze bevat de informatie die wordt vertegenwoordigd door een UserChunk.

De grootste waarde is van de gebruiker uitgebreid. Het bevat de essentiële informatie over en andere gegevens die niet hoeven te snel worden gelezen of heeft uiteindelijke gebruik, zoals de aanmeldprocedure. Deze gegevens kan worden opgeslagen buiten Cosmos DB in Azure SQL Database of Azure Storage-tabellen.

Waarom zou u de gebruiker splitsen en ook deze informatie op te slaan op verschillende plaatsen? Omdat vanuit een oogpunt van prestaties, hoe groter de documenten, het costlier query's. Houd documenten dun, met de juiste informatie naar alle uw afhankelijk van de prestaties van query's voor uw sociale netwerk. Store de extra informatie voor uiteindelijke scenario's zoals volledig profiel bewerkt, aanmeldingen en gegevensanalyse voor gebruiksanalyse en Big Data-initiatieven. U echt niet zo belangrijk als het verzamelen van gegevens voor gegevensanalyse langzamer, is omdat deze wordt uitgevoerd op Azure SQL Database. U hebben betrekking op echter of uw gebruikers een snelle en slanke ervaring hebben. Een gebruiker die zijn opgeslagen op Cosmos DB eruit als de volgende code:

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

Als een bewerking zich wanneer een kenmerk van het segment wordt beïnvloed voordoet, kunt u de betreffende documenten eenvoudig vinden. Gebruik alleen query's die naar de geïndexeerde kenmerken, zoals verwijzen `SELECT * FROM posts p WHERE p.createdBy.id == "edited_user_id"`, en werk vervolgens de segmenten.

## <a name="the-search-box"></a>Het zoekvak

Gebruikers wordt gelukkig veel inhoud is gegenereerd. En u moet kunnen bieden de mogelijkheid om te zoeken en te zoeken naar inhoud die mogelijk niet rechtstreeks in hun inhoud stromen, misschien omdat u niet de makers volgen, of misschien u zojuist hebt probeert te vinden dat oude post u zes maanden geleden hebt.

Omdat u van Azure Cosmos DB gebruikmaakt, kunt u eenvoudig implementeren een search-engine met [Azure Search](https://azure.microsoft.com/services/search/) binnen een paar minuten zonder te hoeven typen van code, dan de search-proces en de gebruikersinterface.

Waarom is dit proces zo gemakkelijk?

Azure Search worden geïmplementeerd wat ze bellen [indexeerfuncties](https://msdn.microsoft.com/library/azure/dn946891.aspx), achtergrondprocessen die haakje in uw gegevensopslagplaatsen en automagically toevoegen, bijwerken of verwijderen van uw objecten in de indexen. Deze ondersteuning bieden voor een [indexeerfuncties van Azure SQL Database](https://blogs.msdn.microsoft.com/kaevans/2015/03/06/indexing-azure-sql-database-with-azure-search/), [Azure Blobs indexeerfuncties](../search/search-howto-indexing-azure-blob-storage.md) en gelukkig [indexeerfuncties van Azure Cosmos DB](../search/search-howto-index-documentdb.md). De overgang van informatie van Cosmos DB in Azure Search is vrij eenvoudig. Beide technologieën gegevens opslaan in JSON-indeling, zodat u alleen wilt [maken van uw Index](../search/search-create-index-portal.md) en toewijzen van de kenmerken van uw geïndexeerde gewenste documenten. Dat is alles. Afhankelijk van de grootte van uw gegevens is alle inhoud beschikbaar voor bij worden gezocht binnen enkele minuten met de beste oplossing voor Search-as-a-Service in de cloudinfrastructuur.

Voor meer informatie over Azure Search, vindt u de [Hitchhiker van handleiding voor het zoeken](https://blogs.msdn.microsoft.com/mvpawardprogram/2016/02/02/a-hitchhikers-guide-to-search/).

## <a name="the-underlying-knowledge"></a>De onderliggende kennis

Na deze inhoud die meegroeit en wordt elke dag op te slaan, is het wellicht denken: wat kan ik doen met deze stroom van gegevens van mijn gebruikers?

Het antwoord is vrij eenvoudig: plaats om te werken en leer van deze.

Maar wat kunt u leren? Enkele eenvoudige voorbeelden zijn onder meer [sentimentanalyse](https://en.wikipedia.org/wiki/Sentiment_analysis), inhoud aanbevelingen op basis van gebruikersvoorkeuren of zelfs een geautomatiseerde content moderator die ervoor dat de inhoud die is gepubliceerd door uw sociale netwerk zorgt veilig is voor de familie.

Nu dat ik kreeg u aangesloten, denkt u waarschijnlijk na moet u enkele PhD in de wetenschap math om op te halen van deze patronen en informatie uit de eenvoudige databases en -bestanden, maar u zou zijn gegaan.

[Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/), onderdeel van de [Cortana Intelligence Suite](https://social.technet.microsoft.com/wiki/contents/articles/36688.introduction-to-cortana-intelligence-suite.aspx), is een volledig beheerde cloudservice waarmee u werkstromen maken middels algoritmen in een eenvoudige interface met slepen en neerzetten, uw eigen algoritmen in code[ R](https://en.wikipedia.org/wiki/R_\(programming_language\)), of u sommige van de al zijn gemaakt en klaar voor gebruik van API's, zoals: [Tekstanalyse](https://gallery.cortanaanalytics.com/MachineLearningAPI/Text-Analytics-2), [Content Moderator of [aanbevelingen](https://gallery.azure.ai/Solution/Recommendations-Solution).

U kunt gebruiken voor het bereiken van een van deze scenario's voor Machine Learning, [Azure Data Lake](https://azure.microsoft.com/services/data-lake-store/) om op te nemen van de gegevens uit verschillende bronnen. U kunt ook [U-SQL](https://azure.microsoft.com/documentation/videos/data-lake-u-sql-query-execution/) voor de verwerking van de gegevens en het genereren van een uitvoer die kan worden verwerkt door Azure Machine Learning.

Een andere optie beschikbaar is met [Azure Cognitive Services](https://www.microsoft.com/cognitive-services) voor het analyseren van uw gebruikers inhoud; niet alleen kunt u ze beter begrijpen (via het analyseren van schrijven met [Tekstanalyse-API](https://www.microsoft.com/cognitive-services/en-us/text-analytics-api)), maar u ook kan ongewenste of volwassen inhoud detecteren en dienovereenkomstig kan reageren met [Computer Vision-API](https://www.microsoft.com/cognitive-services/en-us/computer-vision-api). Cognitive Services bevat veel out-of-the-box-oplossingen waarvoor niet alle soorten Machine Learning kennis wilt gebruiken.

## <a name="a-planet-scale-social-experience"></a>Een sociale ervaring op wereldwijde schaal

Er is een laatste, maar niet in het minst belangrijk artikel ik moet rekening houden: **schaalbaarheid**. Bij het ontwerpen van een architectuur, moet elk onderdeel op een eigen schalen. Uiteindelijk moet u meer gegevens verwerken of wilt u een grotere geografische dekking hebt. Gelukkig zijn beide taken te bereiken is een **kant en klare ervaring** met Cosmos DB.

Cosmos DB biedt ondersteuning voor [dynamische partitioneren](https://azure.microsoft.com/blog/10-things-to-know-about-documentdb-partitioned-collections/) out-of-the-box. Maakt automatisch partities op basis van een bepaalde **partitiesleutel**, die is gedefinieerd als een kenmerk in uw documenten. De juiste partitiesleutel te definiëren moet worden uitgevoerd tijdens de ontwerpfase. Zie voor meer informatie de [kiest u de juiste partitiesleutel](partitioning-overview.md#choose-partitionkey) artikel.

Voor een sociale-ervaring, moet u uw strategie voor partitioneren met de manier waarop u query's uitvoeren en u schrijft uitlijnen. (Bijvoorbeeld leesbewerkingen binnen dezelfde partitie wenselijk zijn, en dat 'hotspots' door te spreiden schrijfbewerkingen in meerdere partities te voorkomen.) Sommige opties zijn: partities op basis van een tijdelijke sleutel (dag/maand/week), op inhoud categorie, per geografische regio of door de gebruiker. Alles echt afhankelijk van hoe u de gegevens opvragen en weergeven van de gegevens in uw sociale-ervaring.

Uw query's worden uitgevoerd door cosmos DB (met inbegrip van [statistische functies](https://azure.microsoft.com/blog/planet-scale-aggregates-with-azure-documentdb/)) voor alle partities in uw transparant, zodat u niet wilt toevoegen een logica als uw gegevens groeien.

Met de tijd, u zult uiteindelijk toenemen in verkeer en het gebruik van resources (gemeten in [ru's](request-units.md), of Aanvraageenheden) wordt verhoogd. U wordt lezen en schrijven meer vaak als uw gebruikers toeneemt. De gebruikersgroep wordt gestart, maken en meer inhoud lezen. Dus de mogelijkheden van **schaal de doorvoer van uw** is van vitaal belang. Vergroot uw ru's is eenvoudig. U kunt dit doen met een paar klikken op de Azure-portal of door [uitgeven van opdrachten via de API](https://docs.microsoft.com/rest/api/cosmos-db/replace-an-offer).

![Omhoog schalen en het definiëren van een partitiesleutel](./media/social-media-apps/social-media-apps-scaling.png)

Wat gebeurt er als dingen steeds beter? Stel dat gebruikers van een andere regio, land/regio of continent ziet u uw platform en gaan gebruiken. Wat een geweldige verrassen!

Maar wachten! U kunt binnenkort realiseren dat hun ervaring met uw platform niet optimaal. Ze tot nu toe van uw operationele regio dat de latentie verschrikkelijke is. U natuurlijk wilt niet dat ze om af te sluiten. Als er slechts er een eenvoudige manier van is **uitbreiden van uw wereldwijde bereik**? Er is.

Cosmos DB kunt u [uw gegevens globaal repliceren](../cosmos-db/tutorial-global-distribution-sql-api.md) en transparant met een paar muisklikken en automatisch kiezen uit de beschikbare regio's van uw [clientcode](../cosmos-db/tutorial-global-distribution-sql-api.md). Dit proces betekent ook dat u kunt hebben [meerdere regio's voor failover-](high-availability.md).

Wanneer u uw gegevens globaal repliceren, moet u om ervoor te zorgen dat uw klanten van deze profiteren kunnen. Als u een web-front- of toegang tot API's van mobiele clients, kunt u implementeren [Azure Traffic Manager](https://azure.microsoft.com/services/traffic-manager/) en kloon van uw Azure App Service op de gewenste regio's met behulp van een prestatieconfiguratie voor de ondersteuning van uw uitgebreide globale dekking. Wanneer uw clients toegang krijgen de front-end- of -API's tot, moeten deze worden doorgestuurd naar de dichtstbijzijnde App Service, die op zijn beurt maakt verbinding met de lokale replica van de Cosmos DB.

![Wereldwijde dekking toe te voegen aan uw sociale platform](./media/social-media-apps/social-media-apps-global-replicate.png)

## <a name="conclusion"></a>Conclusie

In dit artikel wordt enkele licht geworpen in de alternatieven voor het maken van sociale netwerken volledig op Azure met services met lage kosten. het biedt een resultaten door het bevorderen van het gebruik van een meerlaagse storage-oplossing en gegevens distributie 'Ladder' genoemd.

![Diagram van de interactie tussen Azure-services voor sociale netwerken](./media/social-media-apps/social-media-apps-azure-solution.png)

De waarheid is dat er geen silver opsommingsteken voor dit soort scenario's. Het is van de synergie die zijn gemaakt door de combinatie van de geweldige services die we kunnen het bouwen van geweldige ervaringen: de snelheid en de vrijheid van Azure Cosmos DB voor een geweldige sociale toepassing, de intelligence achter een eersteklas search-oplossing, zoals Azure Search, de flexibiliteit van Azure App Services voor het hosten van toepassingen niet zelfs taal-neutraal, maar krachtige achtergrondprocessen en de uitbreidbare Azure Storage en Azure SQL Database voor het opslaan van grote hoeveelheden gegevens en de analytische kracht van Azure Machine Learning voor kennis en intelligentie die kan feedback geven op uw processen en Help ons de juiste inhoud leveren aan de juiste gebruikers te maken.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over gebruiksvoorbeelden voor Cosmos DB, [algemene Cosmos DB use-cases](use-cases.md).
