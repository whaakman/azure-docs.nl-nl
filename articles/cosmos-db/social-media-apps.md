---
title: 'Azure DB Cosmos-ontwerppatroon: sociale media-apps | Microsoft Docs'
description: Meer informatie over een ontwerppatroon voor sociale netwerken dankzij het gebruik van de flexibiliteit van de opslag van Azure DB die Cosmos en andere Azure-services.
keywords: sociale media-apps
services: cosmos-db
author: ealsur
manager: kfile
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 05/29/2017
ms.author: maquaran
ms.openlocfilehash: 977ca57fdb2bcc0c9eaaa33eee06c1d8ae8e39ab
ms.sourcegitcommit: 6eb14a2c7ffb1afa4d502f5162f7283d4aceb9e2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/25/2018
ms.locfileid: "36754483"
---
# <a name="going-social-with-azure-cosmos-db"></a>Met Azure Cosmos DB sociale gaan
Die in een samenleving massively onderling verbonden betekent dat op een bepaald moment in leven u deel van uitmaken een **sociale netwerken**. We gebruiken sociale netwerken om te communiceren vrienden, collega's, familie of soms onze passie delen met mensen met een gemeenschappelijke interesses.

Als engineers of ontwikkelaars kunnen we mogelijk hebben zich wel eens afgevraagd hoe deze netwerken opslaan en onze gegevens onderling verbinding maken of mogelijk hebt zelfs zijn te beheren voor het maken of een nieuw sociale netwerk voor een specifieke gespecialiseerde markt uzelf shards. Als de grote vraag zich voordoet: hoe worden al deze gegevens opgeslagen?

Stel dat we een nieuwe en glanzend sociale netwerken, waar onze gebruikers artikelen met gerelateerde medium, zoals foto's, video's of zelfs muziek kunnen boeken maakt. Gebruikers kunnen opmerkingen over berichten en punten geven voor de classificaties. Er is een feed van berichten die worden weergegeven voor gebruikers en kunnen werken met op de startpagina van de belangrijkste website. Dit erg complex niet begrijpt (aanvankelijk), maar om het eenvoudig, te laten we er (we kan aangepaste gebruiker feeds van invloed op een relaties dieper, maar deze groter is dan het doel van dit artikel).

Ja, hoe kan we dit opslaan en waar?

Veel van u mogelijk ervaring op SQL-databases of hebt u ten minste begrip [relationele gegevens modelleren](https://en.wikipedia.org/wiki/Relational_model) en u misschien geneigd om te beginnen met het tekenen van ongeveer het volgende:

![Diagram ter illustratie van een relatieve relationele model](./media/social-media-apps/social-media-apps-sql.png) 

Een perfect genormaliseerde en gegevensstructuur... die niet wordt geschaald. 

Geen krijgt mij verkeerde, ik heb gewerkt om alle mijn leven met SQL-databases, ze zijn ideaal, maar zoals elke patroon, practice en software-platform, is het niet ideaal voor elk scenario.

Waarom SQL is niet om de beste keuze in dit scenario? Bekijk de structuur van een enkele post als ik wil dat bericht weergeven in een website of toepassing, dat zou er een query uitvoeren met... 8 tabelkoppelingen (!) alleen wilt weergeven van één enkele post, nu afbeelding is een stream van berichten die dynamisch worden geladen en worden weergegeven op het scherm en u kunt tegenkomen wanneer ik ga.

We kan natuurlijk een internationale SQL-exemplaar gebruiken met voldoende stroom voor het oplossen van query's met deze veel joins te bedienen onze content maar waarom zou er bestaat een eenvoudigere oplossing echt duizenden?

## <a name="the-nosql-road"></a>De NoSQL-weg
In dit artikel begeleidt u bij uw sociale platform-gegevens met Azure NoSQL-database te modelleren [Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) op een rendabele manier tijdens het gebruik van andere Cosmos Azure DB functies, zoals de [Gremlin Graph API](../cosmos-db/graph-introduction.md). Met behulp van een [NoSQL](https://en.wikipedia.org/wiki/NoSQL) benadering voor het opslaan van gegevens in JSON-indeling en toepassen van [denormalization](https://en.wikipedia.org/wiki/Denormalization), onze eerder ingewikkeld post kan worden omgezet in één [Document](https://en.wikipedia.org/wiki/Document-oriented_database):


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

En kan worden verkregen met één query op, en er is geen joins. Dit is nog veel meer eenvoudig en snel en budget-wise, deze scan vereist minder resources om een beter resultaat te bereiken.

Azure Cosmos DB zorgt ervoor dat alle eigenschappen worden geïndexeerd met de automatische indexeren, wat kan zelfs zijn [aangepaste](indexing-policies.md). De methode zonder schema kan we documenten opslaan met verschillende en dynamische structuren, mogelijk morgen we berichten willen voor een lijst van categorieën of -hashtags ermee hebben gekoppeld, Cosmos DB wordt afgehandeld tijdens de nieuwe documenten met de toegevoegde kenmerken zonder extra werk vereist door ons.

Opmerkingen over een post kunnen worden behandeld als alleen andere advertenties met een bovenliggende eigenschap (vereenvoudigt de objecttoewijzing van onze). 

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

En alle sociale interacties kunnen worden opgeslagen op een afzonderlijke object als items:

    {
        "id":"dfe3-thf5-232s-dse4",
        "post":"ew12-res2-234e-544f",
        "comments":2,
        "likes":10,
        "points":200
    }

Maken van feeds is zojuist een kwestie van het maken van documenten die een lijst met post-id's met een bepaalde relevantie volgorde kunnen bevatten:

    [
        {"relevance":9, "post":"ew12-res2-234e-544f"},
        {"relevance":8, "post":"fer7-mnb6-fgh9-2344"},
        {"relevance":7, "post":"w34r-qeg6-ref6-8565"}
    ]

We een 'laatste' stream met berichten die zijn geordend op aanmaakdatum kan hebben, die berichten met 'gegevensbeheer' stream graag meer in de afgelopen 24 uur kan zelfs implementeren van een aangepaste stroom voor elke gebruiker op basis van logica zoals pc-gebruikers en interesses en is nog steeds een lijst met p osts. Er is een kwestie van het bouwen van deze lijsten, maar de prestaties lezen concurrerende blijft. Zodra er een van deze lijsten aanschaft, wordt één query verlenen aan Cosmos-database met behulp van de [IN de operator](sql-api-sql-query.md#WhereClause) verkrijgen van pagina's van berichten op een tijdstip.

De feed stromen kunnen worden gebouwd met behulp van [Azure App Services](https://azure.microsoft.com/services/app-service/) processen op de achtergrond: [Webjobs](../app-service/web-sites-create-web-jobs.md). Nadat een advertentie is gemaakt, achtergrondverwerking kan worden geactiveerd met behulp van [Azure Storage](https://azure.microsoft.com/services/storage/) [wachtrijen](../storage/queues/storage-dotnet-how-to-use-queues.md) en Webjobs geactiveerd met behulp van de [Azure Webjobs SDK](https://github.com/Azure/azure-webjobs-sdk/wiki), uitvoering de na de doorgifte in streams op basis van ons eigen aangepaste regels. 

Punten en positieve via een post kunnen worden verwerkt in een uitgestelde manier dezelfde techniek gebruiken voor het maken van een omgeving uiteindelijk consistent is.

Pc-gebruikers zijn lastiger. Cosmos DB heeft een maximale grootte van maximaal document en grote documenten lezen/schrijven kunnen invloed hebben op de schaalbaarheid van uw toepassing. Zo kunt u denkt dat over het opslaan van pc-gebruikers als een document met deze structuur:

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

Dit werkt mogelijk voor een gebruiker met enkele duizenden pc-gebruikers, maar als onze posities, dit wordt benadering lid wordt van sommige beroemdheden leiden tot een grootte van een grote document en kan het document grootte kapje uiteindelijk bereikt.

We kunnen u verhelpt dit door een gemengde benadering gebruiken. Als onderdeel van het document gebruikersstatistieken bewaren wij het nummer van pc-gebruikers:

    {
        "id":"234d-sd23-rrf2-552d",
        "user": "dse4-qwe2-ert4-aad2",
        "followers":55230,
        "totalPosts":452,
        "totalPoints":11342
    }

En de werkelijke grafiek van pc-gebruikers kan worden opgeslagen met behulp van Azure DB die Cosmos [Gremlin Graph API](../cosmos-db/graph-introduction.md), maken [hoekpunten](http://mathworld.wolfram.com/GraphVertex.html) voor elke gebruiker en [randen](http://mathworld.wolfram.com/GraphEdge.html) die de 'A-volgt-B' onderhouden relaties. De Graph API we u niet alleen verkrijgen van de pc-gebruikers van een bepaalde gebruiker maar complexe query's voor het voorstellen mensen zelfs gemeenschappelijk maken. Als we aan de grafiek de inhoud categorieën die mensen toevoegen, zoals of profiteer, kunt we starten weven ervaringen met slim inhoud content voorstellen die we volgen zoals of zoeken naar personen met wie er mogelijk veel gemeen hebben.

Het document gebruikersstatistieken kan nog steeds worden gebruikt voor het maken van kaarten in de gebruikersinterface of snelle profiel previews.

## <a name="the-ladder-pattern-and-data-duplication"></a>De 'Ladder' patroon en gegevens worden gedupliceerd
Als u dat in de JSON-document dat verwijst naar een post opgevallen mogelijk, zijn er meerdere exemplaren van een gebruiker. En u zou hebben geraden rechts, betekent dit dat de informatie die staat voor een gebruiker, krijgt deze denormalization aanwezig zijn op meer dan één plek mogelijk zijn.

Als u wilt toestaan voor snellere query's, maken we gegevens worden gedupliceerd. Het probleem met dit neveneffect is dat als door een bepaalde actie, wijzigingen van de gegevens van een gebruiker, moeten we alle activiteiten zoeken hij ooit hebt en deze alle bijwerken. Geen geluid zeer praktische, rechts?

We gaan met het vaststellen van de kenmerken van de sleutel van een gebruiker die we in onze toepassing voor elke activiteit zien op te lossen. Als we visueel een bericht met onze toepassing weergeven en alleen de maker naam en afbeelding wilt weergeven, waarom u alle gegevens van de gebruiker opgeslagen in het kenmerk 'controleactiviteit gemaakt door'? Voor elke opmerking we zojuist beeld van de gebruiker weergeven, hoeft er niet zeker de rest van de informatie. Dat is waar iets dat ik het patroon"Ladder' aanroepen in het spel worden opgehaald.

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

Door deze informatie bekijkt, kunnen we snel die belangrijke informatie en die niet is, waardoor een 'Ladder' detecteren:

![Diagram van een patroon ladder](./media/social-media-apps/social-media-apps-ladder.png)

De kleinste stap heet een UserChunk, de minimale stukje informatie waarmee een gebruiker en het wordt gebruikt voor gegevens worden gedupliceerd. Door de grootte van de gedupliceerde gegevens beperken tot alleen de informatie die we 'tonen', beperken we de mogelijkheid van grote updates.

De middelste stap wordt de gebruiker genoemd, is de volledige gegevens die worden gebruikt op de meeste prestaties-afhankelijke query's voor Cosmos-database, de meest gebruikte en kritieke. Deze bevat de informatie die wordt vertegenwoordigd door een UserChunk.

De grootste waarde is de gebruiker uitgebreid. Dit omvat alle kritieke gebruikersgegevens plus andere gegevens die niet echt nodig hebt om snel te lezen of gebruik de uiteindelijke (zoals het aanmeldingsproces) is. Deze gegevens kan worden opgeslagen buiten Cosmos-database in Azure SQL Database- of Azure Storage-tabellen.

Waarom zouden we splitsen van de gebruiker en zelfs deze informatie op te slaan op verschillende plaatsen? Omdat vanuit een oogpunt van prestaties, hoe groter de documenten, de costlier query's. Documenten dun, met de juiste informatie om dat te doen alle prestaties-afhankelijke query voor het sociaal netwerk en de extra gegevens voor uiteindelijke scenario's zoals volledig profiel bewerkingen, aanmeldingen opgeslagen zelfs gegevensanalyse voor gebruiksanalyse en Big Data bewaren initiatieven. We zeker niet van belang als het verzamelen van gegevens voor gegevensanalyse is langzamer, omdat deze wordt uitgevoerd op Azure SQL Database, we hebben betrekking op echter dat onze gebruikers met een snelle en dunne ervaring hebben. Een gebruiker wordt opgeslagen op een Cosmos-DB eruit als volgt:

    {
        "id":"dse4-qwe2-ert4-aad2",
        "name":"John",
        "surname":"Doe",
        "username":"johndoe"
        "email":"john@doe.com",
        "twitterHandle":"\@john"
    }

En een Post eruit als:

    {
        "id":"1234-asd3-54ts-199a",
        "title":"Awesome post!",
        "date":"2016-01-02",
        "createdBy":{
            "id":"dse4-qwe2-ert4-aad2",
            "username":"johndoe"
        }
    }

En wanneer een bewerking waarbij een van de kenmerken van het segment wordt beïnvloed ontstaat, is het gemakkelijk te vinden van de betrokken documenten met behulp van query's die naar de geïndexeerde kenmerken verwijzen (Selecteer * FROM boekt p waar p.createdBy.id == 'edited_user_id') en vervolgens de segmenten bij te werken.

## <a name="the-search-box"></a>Het zoekvak
Gebruikers zullen gelukkig veel inhoud genereren. En we moeten kunnen bieden de mogelijkheid om te zoeken en te zoeken naar inhoud die mogelijk niet direct in hun inhoud gegevensstromen, mogelijk omdat we de beveiligingsbeheerder niet volgen, of misschien we zojuist probeert te vinden dat oude post we zes maanden geleden heeft.

Gelukkig en omdat we Azure Cosmos DB gebruiken, kunnen we eenvoudig implementeren een zoekactie engine met [Azure Search](https://azure.microsoft.com/services/search/) binnen een paar minuten en zonder één regel code (anders dan natuurlijk de zoekactie en gebruikersinterface) te hoeven typen.

Waarom is dit dus eenvoudig?

Azure Search implementeert wat ze aanroepen [indexeerfuncties](https://msdn.microsoft.com/library/azure/dn946891.aspx), achtergrondprocessen die haakje in de opslagplaatsen van uw gegevens en automagically toevoegen, bijwerken of verwijderen van uw objecten in de indexen. Deze ondersteuning bieden voor een [indexeerfuncties in Azure SQL Database](https://blogs.msdn.microsoft.com/kaevans/2015/03/06/indexing-azure-sql-database-with-azure-search/), [Azure Blobs indexeerfuncties](../search/search-howto-indexing-azure-blob-storage.md) en gelukkig [Azure Cosmos DB indexeerfuncties](../search/search-howto-index-documentdb.md). De overgang van de informatie van de Cosmos-database naar Azure Search is eenvoudig, als beide archiefgegevens in JSON-indeling, moeten wij ons NET [onze Index maken](../search/search-create-index-portal.md) en toewijzen van welke kenmerken van onze documenten willen we geïndexeerde en dit is alles in een binnen enkele minuten (afhankelijk van de grootte van onze gegevens), alle onze inhoud kan worden gezocht, door de beste oplossing Search-as-a-Service in de cloudinfrastructuur. 

Voor meer informatie over Azure Search vindt u de [Hitchhiker van handleiding voor het zoeken](https://blogs.msdn.microsoft.com/mvpawardprogram/2016/02/02/a-hitchhikers-guide-to-search/).

## <a name="the-underlying-knowledge"></a>De onderliggende kennis
Na deze inhoud die moet worden uitgebreid en wordt elke dag op te slaan, kunnen we vinden onszelf gegevensclassificatie: wat kan ik doen met deze stroom van gegevens van mijn gebruikers?

Het antwoord is eenvoudig: plaatsen om te werken en te leren van het.

Maar wat we meer? Een paar eenvoudige voorbeelden zijn [gevoel analysis](https://en.wikipedia.org/wiki/Sentiment_analysis), inhoud aanbevelingen op basis van gebruikersvoorkeuren of zelfs een geautomatiseerde inhoud beheerder dat ervoor zorgt dat de inhoud die zijn gepubliceerd door onze sociale netwerken voor de familie veilig is.

Nu dat ik heb u aangesloten, zult u waarschijnlijk moet u enkele PhD in math wetenschap deze patronen en de informatie uit de eenvoudige databases en bestanden uitpakken, maar u zou verkeerde zien.

[Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/), deel uit van de [Cortana Intelligence Suite](https://social.technet.microsoft.com/wiki/contents/articles/36688.introduction-to-cortana-intelligence-suite.aspx), is een volledig beheerde cloudservice waarmee u het maken van werkstromen met algoritmen in een eenvoudige interface voor slepen en neerzetten, code van uw eigen algoritmen in [ R](https://en.wikipedia.org/wiki/R_\(programming_language\)) of u een aantal van de al zijn gemaakt en gereed voor gebruik van API's, zoals: [Tekstanalyse](https://gallery.cortanaanalytics.com/MachineLearningAPI/Text-Analytics-2), [inhoud beheerder](https://www.microsoft.com/moderator) of [aanbevelingen](https://gallery.cortanaanalytics.com/MachineLearningAPI/Recommendations-2).

Als u wilt bereiken van deze Machine Learning-scenario's, gebruiken we [Azure Data Lake](https://azure.microsoft.com/services/data-lake-store/) voor het opnemen van de gegevens uit verschillende bronnen en gebruiken [U-SQL](https://azure.microsoft.com/documentation/videos/data-lake-u-sql-query-execution/) voor het verwerken van de informatie en een uitvoer die kan worden gegenereerd door Azure Machine Learning worden verwerkt.

Een andere optie beschikbaar is met [cognitieve Microsoft-Services](https://www.microsoft.com/cognitive-services) voor het analyseren van onze gebruikers inhoud; niet alleen kunnen we ze beter begrijpen (via het analyseren van schrijven met [Text Analytics API](https://www.microsoft.com/cognitive-services/en-us/text-analytics-api)), maar we ook kan ongewenste of volwassen inhoud detecteren en dienovereenkomstig reageren met [Computer Vision-API](https://www.microsoft.com/cognitive-services/en-us/computer-vision-api). Cognitieve Services bevatten een groot aantal out-of-the-box-oplossingen die geen enkele vorm van Machine Learning kennis te gebruiken is vereist.

## <a name="a-planet-scale-social-experience"></a>Een sociaal wereld scale-ervaring
Er is een laatste, maar niet in het minst belangrijk onderwerp die ik moet houden: **schaalbaarheid**. Bij het ontwerpen van een architectuur cruciaal is dat elk onderdeel kan worden geschaald op een eigen, ofwel omdat we moeten meer gegevens verwerken of omdat we willen dat een grotere geografische dekking (of beide!). Gelukkig bereiken van een complexe taak is een **klare ervaring** met Cosmos-DB.

Biedt ondersteuning voor cosmos DB [dynamische partitionering](https://azure.microsoft.com/blog/10-things-to-know-about-documentdb-partitioned-collections/) out-of-the-box door het automatisch maken van partities op basis van een bepaalde **partitiesleutel** (gedefinieerd als een van de kenmerken in uw documenten). Het definiëren van de juiste partitiesleutel moet worden uitgevoerd tijdens het ontwerpen en houd in gedachten de [aanbevolen procedures](../cosmos-db/partition-data.md#designing-for-partitioning) beschikbaar; in het geval van een sociale ervaring moet uw strategie voor partitionering worden uitgelijnd met de manier waarop u een query (leesbewerkingen in hetzelfde partitie wenselijk zijn) en schrijven ("hotspots" voorkomen door te spreiden schrijfbewerkingen op meerdere partities). Sommige opties zijn: partities op basis van een tijdelijke sleutel (dag/maand/week) door inhoud categorie, per geografische regio, door gebruiker. Alles echt afhankelijk van hoe u gegevens opvragen en weergeven in uw sociale ervaring. 

Een interessant punt opgemerkt Cosmos-database wordt uitgevoerd is met uw query's (inclusief [statistische functies](https://azure.microsoft.com/blog/planet-scale-aggregates-with-azure-documentdb/)) voor alle partities in uw transparant, u hoeft niet alle logica toevoegen wanneer uw gegevens groeit.

Met de tijd, wordt uiteindelijk groeien in verkeer en het verbruik van uw (gemeten in [RUs](request-units.md), of Aanvraageenheden) wordt verhoogd. U leest en schrijft vaker als uw userbase groeit en ze gaat maken en lezen meer inhoud; de mogelijkheid van **schalen van uw doorvoer** is het essentieel. Onze RUs verhoging is heel eenvoudig, we kunt dit doen met een paar muisklikken in de Azure Portal of door [uitgeven van opdrachten via de API](https://docs.microsoft.com/rest/api/cosmos-db/replace-an-offer).

![Omhoog schalen en het definiëren van een partitiesleutel](./media/social-media-apps/social-media-apps-scaling.png)

Wat gebeurt er als dingen wordt steeds betere en gebruikers van een andere regio, land of continent, ziet uw platform en gaan gebruiken, wat een geweldige verrassing!

Maar geduld … ontdekt u snel hun ervaring met uw platform is niet optimaal; ze zijn tot nu toe weg van uw operationele regio dat de latentie verschrikkelijke is en u natuurlijk niet wilt dat ze om af te sluiten. Als er slechts er een eenvoudige manier van is **uitbreiden van uw globale bereik**..., maar er is!

Cosmos DB kunt u [globaal uw gegevens repliceren](../cosmos-db/tutorial-global-distribution-sql-api.md) en transparant met een paar muisklikken en automatisch kiezen uit de beschikbare regio's van uw [clientcode](../cosmos-db/tutorial-global-distribution-sql-api.md). Dit betekent ook dat u hebt [meerdere regio's voor failover-](regional-failover.md). 

Wanneer u uw gegevens globaal repliceren, moet u ervoor zorgen dat uw clients van deze profiteren kunnen. Als u een web-front- of toegang tot API's vanaf mobiele clients gebruikt, kunt u implementeren [Azure Traffic Manager](https://azure.microsoft.com/services/traffic-manager/) en klonen van uw Azure App Service op de gewenste regio's met behulp van een configuratie van de prestaties voor de ondersteuning van de uitgebreide algemene dekking. Wanneer uw clients toegang krijgen uw frontend of API's tot, worden ze doorgestuurd naar de dichtstbijzijnde App Service, die op zijn beurt maakt verbinding met de lokale replica van de Cosmos-DB.

![Globale dekking toe te voegen aan uw sociale platform](./media/social-media-apps/social-media-apps-global-replicate.png)

## <a name="conclusion"></a>Conclusie
In dit artikel probeert te verderhelpen in de alternatieven sociale netwerken volledig op Azure maken met services voor lage kosten en goede resultaten door look wordt het gebruik van een oplossing en gegevens verdeling van opslag met meerdere lagen 'Ladder' genoemd.

![Diagram van interactie tussen Azure-services voor sociale netwerken](./media/social-media-apps/social-media-apps-azure-solution.png)

De waarheid is dat er is geen zilver opsommingsteken voor dit soort scenario's, is de synergie gemaakt door de combinatie van goede services die kunnen we grote ervaringen bouwen: de snelheid en vrijheid van Azure DB die Cosmos voor een goede sociale toepassing de Intelligence achter een uitstekende zoekoplossing zoals Azure Search, de flexibiliteit van Azure App Services voor het hosten van toepassingen niet zelfs taal-networkdirect maar krachtige achtergrondprocessen en de uitbreidbare Azure Storage en Azure SQL Database voor het opslaan van enorme hoeveelheden gegevens en de analytische kracht van Azure Machine Learning voor het maken van de kennis en intelligentie die u kunt uw feedback op onze processen en help ons bieden de juiste inhoud naar de juiste gebruikers.

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over de toepassingsmogelijkheden voor Cosmos DB, [algemene Cosmos DB gebruiksvoorbeelden](use-cases.md).
