---
title: Algemene gebruiksvoorbeelden en scenario's voor Azure Cosmos DB | Microsoft Docs
description: 'Meer informatie over de bovenste vijf gebruiksvoorbeelden voor Azure Cosmos DB: gebruiker gegenereerde inhoud, logboekregistratie van gebeurtenissen, gegevens in de catalogus, gebruikersgegevens voorkeuren en Internet der dingen (IoT).'
services: cosmos-db
author: mimig1
manager: jhubbard
editor: 
documentationcenter: 
ms.assetid: eca68a58-1a8c-4851-8cf8-6e4d2b889905
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2017
ms.author: mimig
ms.openlocfilehash: edbed5654a4df8a28b43f03ffd0ac204e0d7f8b1
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="common-azure-cosmos-db-use-cases"></a>Algemene gebruiksvoorbeelden voor Azure Cosmos-DB
In dit artikel biedt een overzicht van enkele algemene gebruiksvoorbeelden voor Azure Cosmos DB.  De aanbevelingen in dit artikel fungeren als een beginpunt tijdens het ontwikkelen van uw toepassing met Cosmos-DB.   

Na het lezen van dit artikel, hebt u mogelijk de volgende vragen beantwoorden: 

* Wat zijn de algemene gebruiksvoorbeelden voor Azure Cosmos DB?
* Wat zijn de voordelen van het gebruik van Azure DB die Cosmos voor retail toepassingen?
* Wat zijn de voordelen van het gebruik van Azure DB die Cosmos als gegevensopslag voor Internet der dingen (IoT) systemen?
* Wat zijn de voordelen van het gebruik van Azure DB die Cosmos voor webtoepassingen en mobiele toepassingen?

## <a name="introduction"></a>Inleiding
[Azure Cosmos DB](../cosmos-db/introduction.md) globaal gedistribueerde database-service van Microsoft is. De service is zo ontworpen dat klanten elastisch (en onafhankelijk) schaal doorvoer en opslag voor een willekeurig aantal geografische regio's. Azure Cosmos-database is de eerste globaal gedistribueerde database-service op de markt vandaag te bieden uitgebreide [serviceovereenkomsten](https://azure.microsoft.com/support/legal/sla/cosmos-db/) die omvat doorvoer, latentie, beschikbaarheid en consistentie. 

Het project Azure Cosmos DB begonnen in 2011 als 'Project Florence' adres developer-knelpunten worden managementoverhead voor grote Internet-toepassingen binnen Microsoft. We besloten inachtneming van deze problemen zijn niet uniek is voor toepassingen van Microsoft Azure Cosmos DB in het algemeen beschikbaar maken voor externe ontwikkelaars in 2015 is in de vorm van [Azure DocumentDB](https://azure.microsoft.com/blog/documentdb-moving-to-general-availability/). De service wordt ubiquitously intern gebruikt binnen Microsoft en is een van de services snelst groeiende door Azure ontwikkelaars extern gebruikt. 

Azure Cosmos-database is een globale gedistribueerde, meerdere model-database die wordt gebruikt in een breed scala aan toepassingen en gebruiksvoorbeelden. Er is een goede keuze voor een [zonder server](http://azure.com/serverless) toepassing die u moet beschikken over lage reactietijden van de volgorde van milliseconde en moet snel en globaal worden geschaald. Deze ondersteuning biedt voor meerdere gegevensmodellen (sleutel / waarde-, documenten, grafieken en kolommen) en veel API's voor toegang tot inclusief [MongoDB](mongodb-introduction.md), [DocumentDB SQL](documentdb-introduction.md), [Gremlin](graph-introduction.md), en [Azure-tabellen](table-introduction.md) systeemeigen en in een uitbreidbare manier. 

Hier volgen enkele kenmerken die geschikt zijn voor krachtige toepassingen met globale streven van Azure Cosmos-database.

* Azure Cosmos DB partities systeemeigen van uw gegevens voor hoge beschikbaarheid en schaalbaarheid. Azure Cosmos DB biedt 99,99% garanties met betrekking tot beschikbaarheid, doorvoer, lage latentie en consistentie.
* Azure Cosmos DB heeft back SSD-opslag met lage latentie reactietijden van de volgorde van milliseconde.
* Azure DB van Cosmos-ondersteuning voor consistentieniveaus zoals uiteindelijke, consistente voorvoegsel, sessie en gebonden-verouderd kan volledige flexibiliteit en lage kosten voor prestaties verhouding. Er is geen databaseservice biedt zoveel flexibiliteit als Azure Cosmos DB in niveaus consistentie. 
* Azure Cosmos-database heeft een flexibele data-vriendelijk-prijsmodel die meters opslag en doorvoer onafhankelijk.
* Azure Cosmos-DB gereserveerde doorvoer model kunt u ook denken in termen van het aantal leest/schrijft in plaats van de CPU/geheugen/IOPs van de onderliggende hardware.
* Azure Cosmos-DB ontwerp kunt die u kan worden uitgebreid naar grote aanvraag volumes in de volgorde van trillions van aanvragen per dag.

Deze kenmerken zijn nuttig in webtoepassingen, mobiele games en IoT-toepassingen die lage reactietijden nodig en moeten verwerken enorme hoeveelheden lees- en schrijfbewerkingen.

## <a name="iot-and-telematics"></a>IoT en telematica
Gebruiksvoorbeelden IoT vaak sommige patronen in hoe ze, verwerken opnemen, delen en opslaan van gegevens.  Deze systemen moeten eerst bursts van gegevens van het apparaat sensoren van verschillende landinstellingen opnemen. Vervolgens deze systemen verwerken en streaming gegevens analyseren om te worden afgeleid realtime-inzichten verkrijgen. De gegevens wordt vervolgens naar de opslag van koude voor batch analytics gearchiveerd. Microsoft Azure biedt uitgebreide services die kunnen worden toegepast voor IoT gebruiksvoorbeelden, waaronder Azure Cosmos DB, Azure Event Hubs, Azure Stream Analytics, Azure Notification Hub, Azure Machine Learning, Azure HDInsight en Power BI. 

![Azure IoT voor Cosmos-DB-referentiearchitectuur](./media/use-cases/iot.png)

Bursts van gegevens kunnen door Azure Event Hubs worden ingenomen zoals gegevensopname hoge doorvoersnelheden met een lage latentie biedt. Gegevens ingenomen die moet worden verwerkt voor realtime inzicht kunnen met Azure Stream Analytics voor realtime analyses worden softwareproducten. Gegevens kunnen worden geladen naar Cosmos-DB Azure ad-hoc query's. Nadat de gegevens in Azure DB die Cosmos is geladen, is de gegevens gereed om te worden opgevraagd. Bovendien, kunnen nieuwe gegevens en wijzigingen in bestaande gegevens worden gelezen op wijziging feed. Wijziging feed is een permanente, alleen logboek die wijzigingen in verzamelingen Cosmos DB in sequentiële volgorde opslaat toevoegen. De alle gegevens of alleen wijzigingen aan gegevens in Azure Cosmos DB kunnen worden gebruikt als referentiegegevens als onderdeel van realtime-analyses. Bovendien worden gegevens verder verfijnd en verwerkt door de verbinding van Azure DB die Cosmos-gegevens met HDInsight voor Pig, Hive of kaart/verminderen taken.  Verfijnd gegevens worden vervolgens terug naar Azure Cosmos DB geladen voor rapportage.   

Zie voor een voorbeeld IoT-oplossing met Azure Cosmos DB, EventHubs en Storm, de [hdinsight-storm-voorbeelden opslagplaats op GitHub](https://github.com/hdinsight/hdinsight-storm-examples/).

Zie voor meer informatie over Azure IoT-producten [maken van het Internet van uw dingen](http://www.microsoft.com/server-cloud/internet-of-things.aspx). 

## <a name="retail-and-marketing"></a>Detailhandel en marketing
Azure Cosmos DB wordt uitgebreid in van het Microsoft commerciële platforms, met de Windows Store en de XBox Live gebruikt. Het wordt ook gebruikt in de detailhandel voor het opslaan van gegevens in de catalogus en voor gebeurtenis bron in volgorde pijplijnen verwerken.

Gebruiksscenario's voor data Catalog hebben betrekking op opslaat en gegevensquery's van een set kenmerken voor entiteiten zoals mensen, plaatsen en producten. Enkele voorbeelden van gegevens in de catalogus zijn gebruikersaccounts, catalogi, IoT-apparaat registers en factuur van materialen systemen. Kenmerken voor deze gegevens kunnen variëren en na verloop van tijd om te voldoen aan de toepassingsvereisten kunnen wijzigen.

Bekijk een voorbeeld van een productcatalogus voor de leverancier van een auto-onderdelen. Elk onderdeel mogelijk een eigen kenmerken naast de gangbare kenmerken die delen van alle onderdelen. Kenmerken voor een bepaald deel kunnen bovendien het volgende jaar wanneer een nieuw model wordt vrijgegeven wijzigen. Azure Cosmos DB ondersteunt flexibele schema's en hiërarchische gegevens en het is dus geschikt voor het opslaan van gegevens in de productcatalogus.

![Azure DB Cosmos catalogus retail referentiearchitectuur](./media/use-cases/product-catalog.png)

Azure Cosmos DB wordt vaak gebruikt voor gebeurtenis bron naar de stroom gebeurtenissen architecturen met behulp van de [wijzigen van de feed](change-feed.md) functionaliteit. De feed wijzigen kunt u downstream microservices betrouwbaar en incrementeel lezen INSERT en wijzigingen (bijvoorbeeld volgorde gebeurtenissen) in een Cosmos Azure DB. Deze functionaliteit kan worden gebruikt om te voorzien in een archief persistente gebeurtenis als broker bericht gebeurtenissen status wijzigen en station verwerking werkstroom tussen veel microservices (die kunnen worden geïmplementeerd als [zonder server Azure Functions](http://azure.com/serverless)).

![Azure Cosmos DB ordening referentiearchitectuur pijplijn](./media/use-cases/event-sourcing.png)

Bovendien kunnen gegevens die zijn opgeslagen in Azure Cosmos DB worden geïntegreerd met HDInsight voor big data-analyses via Apache Spark-taken. Zie voor meer informatie over de Spark-Connector voor Azure Cosmos DB [een Spark-taak uitvoert met Cosmos DB en HDInsight](spark-connector.md).

## <a name="gaming"></a>Gaming
De databaselaag is een belangrijk onderdeel van games. Moderne games grafische verwerking op de clients mobile-console uitvoeren, maar zijn afhankelijk van de cloud voor aangepaste en gepersonaliseerde inhoud zoals in het spel statistieken, sociale media-integratie en hoge-score scoreborden. Games vereisen vaak één milliseconde latenties voor leesbewerkingen en schrijfbewerkingen naar bieden een bezighouden spel ervaring. Een game database moet worden snel en kunnen verwerken van grote pieken in tarieven voor aanvraag tijdens nieuwe spel wordt gestart en functies van updates.

Azure Cosmos-database wordt gebruikt door games zoals [de roulatie van inactieve: Nee Man van Land](https://azure.microsoft.com/blog/the-walking-dead-no-mans-land-game-soars-to-1-with-azure-documentdb/) door [volgende Games](http://www.nextgames.com/), en [Halo 5: voogden](https://azure.microsoft.com/blog/how-halo-5-guardians-implemented-social-gameplay-using-azure-documentdb/). Azure Cosmos DB biedt de volgende voordelen voor ontwikkelaars van games:

* Voorziet in Azure Cosmos DB prestaties worden geschaald naar boven of beneden elastisch. Hierdoor games bijwerken profiel en statistieken van tientallen miljoenen gelijktijdige gamers doordat één API-aanroep verwerken.
* Azure Cosmos DB ondersteunt milliseconde leest en schrijft om te voorkomen dat een lag tijdens het spel.
* Azure Cosmos-DB automatisch indexeren kunt u filteren op meerdere verschillende eigenschappen in realtime, bijvoorbeeld vinden spelers door hun interne player-id's of hun GameCenter, Facebook, Google-id's of query op basis van lidmaatschap in een vereniging player. Dit is mogelijk zonder het bouwen van complexe te indexeren of sharding-infrastructuur.
* Sociale functies zoals in het spel chatberichten, player vereniging lidmaatschappen uitdagingen voltooid, hoge-score scoreborden en sociale grafieken zijn gemakkelijker te implementeren met een flexibele schema.
* Azure Cosmos-database als een beheerde platform-as-a-service (PaaS) vereist minimale installatie en beheer werken om toe te staan voor snelle iteratie en problemen sneller op de markt.

![Azure DB Cosmos gaming-referentiearchitectuur](./media/use-cases/gaming.png)

## <a name="web-and-mobile-applications"></a>Web- en mobiele toepassingen
Azure Cosmos DB wordt meestal gebruikt in de web- en mobiele toepassingen en is geschikt voor het modelleren van sociale interacties integreren met de services van derden en voor het bouwen van uitgebreide persoonlijke ervaring. De Cosmos-DB SDK's kan worden gebruikt build uitgebreide iOS en Android-toepassingen met behulp van de populaire [Xamarin framework](mobile-apps-with-xamarin.md).  

### <a name="social-applications"></a>Sociale toepassingen
Een algemene gebruiksvoorbeeld voor Azure DB die Cosmos is opslaan en opvragen van de gebruiker gegenereerde inhoud (UGC) voor webtoepassingen, mobiele en sociale media-toepassingen. Enkele voorbeelden van UGC zijn chat-sessies, tweets blogberichten, classificaties en opmerkingen. De UGC in toepassingen met sociale media is vaak een combinatie van vrije tekst, eigenschappen, labels en relaties die niet beperkt rigid structuur tot zijn. Inhoud zoals Chat, opmerkingen en berichten kunnen worden opgeslagen in Cosmos DB zonder dat transformaties of complexe object aan relationele gegevens lagen.  Data-eigenschappen kunnen worden toegevoegd of gewijzigd eenvoudig om te voldoen aan vereisten zoals ontwikkelaars doorlopen de toepassingscode dus promoveren snelle ontwikkeling.  

Toepassingen die zijn geïntegreerd met sociale netwerken van derden moeten reageren op het wijzigen van schema's van deze netwerken. Als de gegevens wordt in Cosmos DB standaard automatisch geïndexeerd, is gegevens gereed om te worden opgevraagd op elk gewenst moment. Deze toepassingen hebben daarom de flexibiliteit om op te halen projecties volgens hun respectieve behoeften.

Veel van de sociale toepassingen uitvoeren op globale schaal en onvoorspelbare gebruikspatronen kunnen vertonen. Flexibiliteit toe voor het schalen van het gegevensarchief is essentieel tijdens de toepassingslaag gebruiksvereisten overeen te schalen.  U kunt uitbreiden door aanvullende gegevenspartities onder een Cosmos-DB-account toe te voegen.  Bovendien kunt u ook aanvullende Cosmos-DB-accounts maken in meerdere regio's. Zie voor de beschikbaarheid van een Cosmos-DB service regio [Azure-gebieden](https://azure.microsoft.com/regions/#services).

![Azure DB Cosmos web-app-referentiearchitectuur](./media/use-cases/apps-with-global-reach.png)

### <a name="personalization"></a>Personalisatie
Tegenwoordig moderne toepassingen worden geleverd met complexe weergaven en ervaringen. Dit zijn doorgaans dynamisch is, catering gebruikersvoorkeuren of stemming en huisstijl behoeften. Toepassingen moeten daarom kunnen persoonlijke instellingen effectief als u wilt renderen UI-elementen en ervaringen snel op te halen. 

JSON, een indeling die wordt ondersteund door Cosmos DB, is een effectieve indeling UI lay-outgegevens vertegenwoordigt, zoals het is niet alleen lichtgewicht, maar ook kan worden eenvoudig geïnterpreteerd door JavaScript. Cosmos DB biedt instelbare consistentieniveaus waarmee snelle Lees met lage latentie voor schrijfbewerkingen. Opslaan van de gebruikersinterface lay-outgegevens met inbegrip van persoonlijke instellingen als JSON-documenten in Cosmos-database is daarom een effectieve methode voor het ophalen van deze gegevens via de kabel.

![Azure DB Cosmos web-app-referentiearchitectuur](./media/use-cases/personalization.png)

## <a name="next-steps"></a>Volgende stappen
Om aan de slag met Azure Cosmos DB, volg ons [snel aan de slag](create-documentdb-dotnet.md), die helpt u bij het maken van een account en aan de slag met Cosmos-DB. 

Of, als u meer informatie over klanten met behulp van de Cosmos-database, de volgende ervaringen van klanten zijn beschikbaar:

* [Jet.com](https://jet.com). E-commerce uitdager eyes de bovenste positie, wordt uitgevoerd op de Microsoft cloud, maakt gebruik van de Cosmos-database op een wereldwijde schaal.
* [Asos.com](http://www.asos.com/). Asos.com is een Brits wijze en voordeel onlinewinkel. Asos is voornamelijk bedoeld voor jonge volwassenen, verkoopt meer dan 850 merken, evenals een eigen reeks kleding en accessoires.
* [Toyota](https://www.toyota.com/). Toyota Motor Corporation is een Japanse auto fabrikant. Cosmos DB Toyota gebruikt voor een algemene IoT-app.
* [Citrix](https://customers.microsoft.com/story/citrix). Citrix ontwikkelt eenmalige aanmelding oplossing met behulp van Azure Service Fabric en Azure Cosmos-DB
* [TEXA](https://customers.microsoft.com/story/texaspa) van TEXA revolutionaire IoT-oplossing voor eigenaren van vehicle bespaart u tijd, geld, gas, en mogelijk.
* [De Domino Pizza](https://www.dominos.com). De Domino Pizza Inc. is een restaurant American pizza keten.
* [Hiermee bepaalt u Johnson](http://www.johnsoncontrols.com). Johnson Controls is een globale gediversifieerde technologie en meerdere industriële opvulteken voor een breed scala aan klanten in meer dan 150 landen.
* [Microsoft Windows, Universal Store Azure IoT Hub, Xbox Live en andere services Internet scale](https://azure.microsoft.com/blog/how-azure-documentdb-planet-scale-nosql-helps-run-microsoft-s-own-businesses/). Hoe Microsoft sterk schaalbare services met behulp van Azure DB die Cosmos maakt.
* [Microsoft Data en analyses team](https://customers.microsoft.com/story/microsoftdataandanalytics). Microsoft team van gegevens en Analytics realiseert wereld scale big data-verzameling met Azure Cosmos-DB
* [Sulekha.com](https://customers.microsoft.com/story/sulekha-uses-azure-documentdb-to-connect-customers-and-businesses-across-india). Sulekha maakt gebruik van Azure DB die Cosmos klanten en bedrijven op India verbinding maken.
* [NewOrbit](https://customers.microsoft.com/story/neworbit-takes-flight-with-azure-documentdb). NewOrbit duurt vlucht met Azure Cosmos DB.
* [Affinio](https://customers.microsoft.com/doclink/affinio-switches-from-aws-to-azure-documentdb-to-harness-social-data-at-scale). Affinio overgeschakeld van AWS naar Azure Cosmos DB aan te wenden sociale gegevens op grote schaal.
* [Naast Games](https://azure.microsoft.com//blog/the-walking-dead-no-mans-land-game-soars-to-1-with-azure-documentdb/). De onbestelbare wandel: Man van Land game soars tot #1 wordt ondersteund door Azure Cosmos DB.
* [Halo](https://azure.microsoft.com/blog/how-halo-5-guardians-implemented-social-gameplay-using-azure-documentdb/). Hoe Halo 5 sociale spelen met behulp van Azure DB die Cosmos geïmplementeerd.
* [Cortana-Analytics galerie](https://azure.microsoft.com/blog/cortana-analytics-gallery-a-scalable-community-site-built-on-azure-documentdb/). Cortana Analytics Gallery - een communitysite schaalbare is gebouwd op Azure Cosmos DB.
* [Eenvoudig](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=18602). Voorloopspaties Integrator geeft meertalige ondernemingen globale inzicht in minuten met flexibele Cloudtechnologieën.
* [Nieuws Republiek](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=18639). Intelligence wordt toegevoegd aan het nieuws om informatie te geven met als doel voor de betrokken burgers. 
* [BG's International](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=18653). Belangrijke merken inschakelen voor consistente kleur overal ter wereld BG's. En schakelt u BG's naar Azure.
* [Telenor](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=18608). Globale opvulteken Telenor maakt gebruik van de cloud te verplaatsen van de snelheid van een opstarten. 
* [XOMNI](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=18667). Het archief van de toekomst wordt uitgevoerd op snelle zoekopdracht en de eenvoudige gegevensoverdracht.
* [Nucleo](https://customers.microsoft.com/story/azure-based-software-platform-breaks-down-barriers-bet). Op basis van een Azure-softwareplatform uitvalt barrières tussen bedrijven en klanten
* [Weka](https://customers.microsoft.com/story/weka-smart-fridge-improves-vaccine-management-so-more-people-can-be-protected-against-diseases). Smart koelkast weka verbetert vaccin beheer zodat meer gebruikers kunnen worden beveiligd tegen ziekten
* [Oranje Tribes](https://customers.microsoft.com/story/theres-more-to-that-food-app-than-meets-the-eye-or-the-mouth). Er is meer aan die app voeding dan voldoet aan de mond of de ogen.
* [Echte Madrid](https://customers.microsoft.com/story/real-madrid-brings-the-stadium-closer-to-450-million-f). Echte Madrid brengt het stadium dichter 450 miljoen ventilatoren over de hele wereld, met de Microsoft Cloud.
* [Tuku](https://customers.microsoft.com/story/tuku-makes-car-buying-fun-with-help-from-azure-services). TUKU maakt auto kopen plezier met behulp van Azure-services
