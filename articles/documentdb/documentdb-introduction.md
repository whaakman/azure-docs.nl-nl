---
title: Inleiding tot DocumentDB, een JSON-database | Microsoft Docs
description: Meer informatie over Azure DocumentDB, een NoSQL JSON-database. Deze documentdatabase is gebouwd voor big data, een elastische schaalbaarheid en een hoge beschikbaarheid.
keywords: json-database, documentdatabase
services: documentdb
author: mimig1
manager: jhubbard
editor: monicar
documentationcenter: 
ms.assetid: 686cdd2b-704a-4488-921e-8eefb70d5c63
ms.service: documentdb
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/14/2017
ms.author: mimig
translationtype: Human Translation
ms.sourcegitcommit: a087df444c5c88ee1dbcf8eb18abf883549a9024
ms.openlocfilehash: 4f6ae0c3c40a10f75b46c6e44b0aa9e8ce440b4d
ms.lasthandoff: 03/15/2017


---
# <a name="introduction-to-documentdb-a-nosql-json-database"></a>Inleiding tot DocumentDB: een NoSQL JSON-database
## <a name="what-is-documentdb"></a>Wat is DocumentDB?
DocumentDB is een volledig beheerde NoSQL-databaseservice die is gebouwd voor snelle en voorspelbare prestaties, hoge beschikbaarheid, elastisch schalen, globale distributie en een gebruiksvriendelijke manier van ontwikkelen. DocumentDB is een NoSQl-database zonder schema en biedt rijke en vertrouwde mogelijkheden voor SQL-query‘s met consistente lage latenties voor JSON-gegevens. Hierdoor wordt 99% van de leesbewerkingen in minder dan 10 milliseconden verwerkt en 99% van de schrijfbewerkingen in minder dan 15 milliseconden. Door deze unieke voordelen is DocumentDB zeer geschikt voor internet, mobiel, games en IoT, en voor veel andere toepassingen die naadloze schaling en globale replicatie nodig hebben.

## <a name="how-can-i-learn-about-documentdb"></a>Hoe kan ik meer informatie krijgen over DocumentDB?
Als u snel meer te weten wilt komen over DocumentDB en een demonstratie wilt bekijken, volgt u de volgende drie stappen: 

1. Bekijk de video [What is DocumentDB?](https://azure.microsoft.com/documentation/videos/what-is-azure-documentdb/) (Wat is DocumentDB?) waarin u in twee minuten kennismaakt met de voordelen van het gebruik van DocumentDB.
2. Bekijk de drie minuten durende video [Create DocumentDB on Azure](https://azure.microsoft.com/documentation/videos/create-documentdb-on-azure/) (DocumentDB op Azure maken), waarin wordt uitgelegd hoe u aan de slag kunt met DocumentDB door gebruik te maken van Azure Portal.
3. Bezoek de [Query Playground](http://www.documentdb.com/sql/demo)(Queryspeelplaats) waar u verschillende activiteiten kunt doorlopen om meer te leren over de geavanceerde queryfunctionaliteit die beschikbaar is in DocumentDB. Ga vervolgens naar het tabblad Sandbox en voer uw eigen SQL-query's uit en experimenteer met DocumentDB.

Kom vervolgens terug naar dit artikel waar dieper op de zaken wordt ingegaan.  

## <a name="what-capabilities-and-key-features-does-documentdb-offer"></a>Welke mogelijkheden en belangrijke functies biedt DocumentDB?
Azure DocumentDB biedt de volgende belangrijke mogelijkheden en voordelen:

* **Elastische schaalbare doorvoer en opslag:** U kunt uw DocumentDB JSON-database eenvoudig omhoog of omlaag schalen om te voldoen aan de behoeften van uw toepassing. Uw gegevens worden opgeslagen op SSD-schijven (Solid State Disks) voor lage voorspelbare latenties. DocumentDB ondersteunt containers voor het opslaan van de JSON-gegevens die verzamelingen worden genoemd, waarvan de opslaggrootte en ingerichte doorvoer vrijwel onbeperkt kan worden geschaald. Wanneer uw toepassing groeit, kunt u DocumentDB probleemloos schalen met voorspelbare prestaties. 
* **Replicatie voor meerdere regio's:** DocumentDB repliceert uw gegevens transparant naar alle regio's die u aan uw DocumentDB-account hebt gekoppeld, zodat u toepassingen kunt ontwikkelen waarvoor globale toegang tot gegevens is vereist, en die tegelijkertijd zorgen voor een balans tussen consistentie, beschikbaarheid en prestaties, allemaal met overeenkomstige garanties. DocumentDB biedt transparante regionale failover met multihoming-API's en de mogelijkheid om doorvoer en opslag wereldwijd elastisch te schalen. Zie [Distribute data globally with DocumentDB](documentdb-distribute-data-globally.md) (Gegevens globaal distribueren met DocumentDB) voor meer informatie.
* **Ad-hocquery's met bekende SQL-syntaxis:** U kunt heterogene JSON-documenten in DocumentDB opslaan en query's op deze documenten uitvoeren via een bekende SQL-syntaxis. DocumentDB maakt gebruik van een uiterst gelijktijdige, vergrendelingsvrije, indexeringstechnologie op basis van een logboekenstructuur om alle documentinhoud automatisch te indexeren. Zodoende kunt u realtime query's uitvoeren zonder dat u schemahints, secundaire indexen of weergaven hoeft op te geven. Zie [Query's in DocumentDB uitvoeren](documentdb-sql-query.md) voor meer informatie. 
* **Uitvoeren van JavaScript in de database:** Express-toepassingslogica als opgeslagen procedures en triggers en door de gebruiker gedefinieerde functies (UDF's) met standaard-JavaScript. Zodoende kan uw toepassingslogica worden toegepast op gegevens zonder dat u zich zorgen hoeft te maken over onjuiste overeenkomsten tussen de toepassing en het databaseschema. Met DocumentDB kan de JavaScript-toepassingslogica volledig transactioneel en rechtstreeks worden uitgevoerd in de database-engine. Dankzij de diepe integratie van JavaScript kunnen de bewerkingen INSERT, REPLACE, DELETE en SELECT als geïsoleerde transactie worden uitgevoerd vanuit een JavaScript-programma. Zie [Programmeren op de DocumentDB-server](documentdb-programming.md) voor meer informatie.
* **Instelbare consistentieniveaus:** Selecteer een van de vier goed gedefinieerde consistentieniveaus voor een optimale balans tussen de consistentie en prestaties. Voor query's en leesbewerkingen biedt DocumentDB vier verschillende consistentieniveaus: sterk, gebonden-verouderd, sessie en mogelijk. Op basis van deze gedetailleerde, goed gedefinieerde consistentieniveaus kunt u een goede balans vinden tussen de consistentie, beschikbaarheid en latentie. Zie [Consistentieniveaus gebruiken om de beschikbaarheid en prestaties in DocumentDB te maximaliseren](documentdb-consistency-levels.md) voor meer informatie.
* **Volledig beheerd:** Elimineer de noodzaak om database- en machineresources te beheren. U hoeft voor een volledig beheerde Microsoft Azure-service geen virtuele machines te beheren, software te implementeren en te configureren, schaalwijzigingen te beheren of complexe upgrades voor gegevenslagen uit te voeren. Er wordt automatisch een back-up van elke database gemaakt en de databases worden automatisch beveiligd tegen regionale fouten. U kunt gemakkelijk een DocumentDB-account toevoegen en capaciteit inrichten als u dit nodig hebt, waardoor u zich kunt richten op uw toepassing in plaats van het besturingssysteem en het beheren van uw database. 
* **Open ontwerp:** U kunt snel aan de slag door gebruik te maken van bestaande vaardigheden en hulpprogramma's. U kunt op een eenvoudige en gebruiksvriendelijke manier programmeren voor DocumentDB. Bovendien hebt u geen nieuwe hulpprogramma's nodig en hoeft u geen aangepaste uitbreidingen voor JSON of JavaScript te gebruiken. De volledige databasefunctionaliteit, inclusief CRUD-, query- en JavaScript-verwerking, is toegankelijk via een eenvoudige RESTful HTTP-interface. DocumentDB is geschikt voor bestaande indelingen, talen en standaarden, terwijl daarnaast waardevolle databasemogelijkheden worden geboden.
* **Automatisch indexeren:** alle documenten in de database worden standaard automatisch geïndexeerd in DocumentDB. Er wordt geen schema verwacht of vereist en er hoeven geen secundaire indexen te worden gemaakt. Wilt u niet alles indexeren? Maakt u zich geen zorgen. U kunt ook [paden uitschakelen in uw JSON-bestand](documentdb-indexing-policies.md).
* **Compatibiliteit met MongoDB-apps:** met DocumentDB: API voor MongoDB kunt u DocumentDB-databases gebruiken als het gegevensarchief voor apps die zijn geschreven voor MongoDB. Door bestaande stuurprogramma's voor MongDB-databases te gebruiken, kan uw toepassing die is geschreven voor MongoDB, nu dus communiceren met DocumentDB en DocumentDB-databases kan gebruiken in plaats van MongoDB-databases. In veel gevallen hoeft u alleen een verbindingsreeks te wijzigen als u wilt overschakelen van het gebruik van MongoDB naar DocumentDB . Meer informatie vindt u in [Wat is DocumentDB: API voor MongoDB?](documentdb-protocol-mongodb.md)

## <a name="data-management"></a>Hoe worden gegevens beheerd in DocumentDB?
JSON-gegevens in Azure DocumentDB worden beheerd via goed gedefinieerde databaseresources. Deze resources worden gerepliceerd voor hoge beschikbaarheid en zijn uniek adresseerbaar op basis van hun logische URI. DocumentDB biedt voor alle resources een eenvoudig RESTful-programmeermodel op basis van HTTP. 

Het DocumentDB-databaseaccount is een unieke naamruimte die u toegang biedt tot Azure DocumentDB. Voordat u een databaseaccount kunt maken, moet u een Azure-abonnement hebben. Dit abonnement geeft u toegang tot diverse Azure-services. 

Alle resources binnen DocumentDB worden gemodelleerd en opgeslagen als JSON-documenten. Resources worden beheerd als items, oftewel JSON-documenten met metagegevens, en als feeds, oftewel verzamelingen items. Sets met items bevinden zich in hun respectieve feeds.

De onderstaande afbeelding toont u relaties tussen de DocumentDB-resources:

![De hiërarchische relatie tussen resources in DocumentDB, een NoSQL JSON-database][1] 

Een databaseaccount bestaat uit een set databases die elk meerdere verzamelingen bevatten. Elk verzameling kan opgeslagen procedures, triggers, UDF's, documenten en verwante bijlagen bevatten. Er zijn ook gebruikers aan een database gekoppeld. Elke gebruiker beschikt over een set machtigingen voor toegang tot verschillende andere verzamelingen, opgeslagen procedures, triggers, UDF's, documenten of bijlagen. Databases, gebruikers, machtigingen en verzamelingen zijn door het systeem gedefinieerde resources met bekende schema's. Documenten, opgeslagen procedures, triggers, UDF's en bijlagen bevatten echter willekeurige, door de gebruiker gedefinieerde JSON-inhoud.  

## <a name="develop"></a> Hoe kan ik apps ontwikkelen met DocumentDB?
Azure DocumentDB ontsluit resources via een REST API die kan worden aangeroepen via elke taal waarmee HTTP-/HTTPS-aanvragen kunnen worden gemaakt. Daarnaast biedt DocumentDB programmeringsbibliotheken voor verschillende veelgebruikte talen en is het compatibel met MongoDB API's. Dankzij de clientbibliotheken kunt u in vele opzichten eenvoudiger met Azure DocumentDB werken, doordat ze de gegevens voor adrescaching, uitzonderingsbeheer, automatische nieuwe pogingen enzovoort verwerken. Bibliotheken zijn momenteel beschikbaar voor de volgende talen en platformen:  

| Downloaden | Documentatie |
| --- | --- |
| [.NET SDK](http://go.microsoft.com/fwlink/?LinkID=402989) |[.NET-bibliotheek](https://msdn.microsoft.com/library/azure/dn948556.aspx) |
| [Node.js SDK](http://go.microsoft.com/fwlink/?LinkID=402990) |[Node.js-bibliotheek](http://azure.github.io/azure-documentdb-node/) |
| [Java SDK](http://go.microsoft.com/fwlink/?LinkID=402380) |[Java-bibliotheek](http://azure.github.io/azure-documentdb-java/) |
| [JavaScript SDK](http://go.microsoft.com/fwlink/?LinkID=402991) |[JavaScript-bibliotheek](http://azure.github.io/azure-documentdb-js/) |
| N.v.t. |[JavaScript SDK op de server](http://azure.github.io/azure-documentdb-js-server/) |
| [Python SDK](https://pypi.python.org/pypi/pydocumentdb) |[Python-bibliotheek](http://azure.github.io/azure-documentdb-python/) |
| N.v.t. | [API voor MongoDB](documentdb-protocol-mongodb.md)

Met behulp van de [Azure DocumentDB-emulator](documentdb-nosql-local-emulator.md) kunt u uw toepassing lokaal ontwikkelen en testen, kosteloos en zonder een Azure-abonnement te maken. Als u tevreden bent over hoe uw toepassing werkt in de DocumentDB-emulator, kunt u overstappen naar een Azure DocumentDB-account in de cloud.

DocumentDB biedt naast de eenvoudige maak-, lees-, bijwerk- en verwijderbewerkingen een geavanceerde SQL-queryinterface voor het ophalen van JSON-documenten en ondersteuning aan de serverzijde voor de transactionele uitvoering van JavaScript-toepassingslogica. De interfaces voor het uitvoeren van query's en scripts zijn beschikbaar via alle platformbibliotheken en REST API's. 

### <a name="sql-query"></a>SQL-query
Azure DocumentDB biedt ondersteuning voor documentquery's die gebruikmaken van een SQL-taal, die verankerd ligt in het JavaScript-typesysteem, en voor expressies met ondersteuning voor relationele, hiërarchische en ruimtelijke query's. De quertytaal van DocumentDB is een eenvoudige maar krachtige interface om query's op JSON-documenten uit te voeren. De taal ondersteunt een subset van de ANSI SQL-grammatica en zorgt voor een diepe integratie van JavaScript-object-, -matrix-, -objectconstructie- en functieaanroepen. DocumentDB levert het querymodel zonder een expliciet schema of indexeringshints van de ontwikkelaar.

Door de gebruiker gedefinieerde functies (UDF's) kunnen worden geregistreerd met DocumentDB en er kan als onderdeel van een SQL-query naar worden verwezen, waarmee de grammatica ter ondersteuning van aangepaste toepassingslogica wordt uitgebreid. Deze UDF zijn geschreven als JavaScript-programma's en worden uitgevoerd binnen de database. 

Voor .NET-ontwikkelaars bevat DocumentDB een LINQ-queryprovider als onderdeel van de [.NET SDK](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.linq.aspx). 

### <a name="transactions-and-javascript-execution"></a>Transacties en JavaScript uitvoeren
Met DocumentDB kunt u toepassingslogica schrijven als benoemde programma's die volledig zijn geschreven in JavaScript. Deze programma's worden geregistreerd voor een verzameling en kunnen databasebewerkingen uitvoeren op de documenten binnen een bepaalde verzameling. JavaScript kan worden geregistreerd voor uitvoering als een trigger, opgeslagen procedure of als een door de gebruiker gedefinieerde functie. Met triggers en opgeslagen procedures kunt u documenten maken, lezen, bijwerken en verwijderen, terwijl de functies die door de gebruiker zijn gedefinieerd, worden uitgevoerd als onderdeel van de logica voor het uitvoeren van een query, zonder schrijftoegang tot de verzameling.

De JavaScript-uitvoering in DocumentDB is gemodelleerd naar de concepten die worden ondersteund door relationele databasesystemen, met JavaScript als een moderne vervanger van Transact-SQL. Alle JavaScript-logica wordt uitgevoerd binnen een ambient ACID-transactie met het isolatieniveau Snapshot. Als er tijdens de uitvoering een JavaScript-uitzondering optreedt, wordt de volledige transactie afgebroken.

## <a name="are-there-any-online-courses-on-documentdb"></a>Zijn er online cursussen over DocumentDB?

Ja, er is een [Microsoft Virtual Academy](https://mva.microsoft.com/en-US/training-courses/azure-documentdb-planetscale-nosql-16847)-cursus over Azure DocumentDB. 

>[!VIDEO https://mva.microsoft.com/en-US/training-courses-embed/azure-documentdb-planetscale-nosql-16847]
>
>

## <a name="next-steps"></a>Volgende stappen
Hebt u al een Azure-account? Dan kunt u aan de slag met DocumentDB in [Azure Portal](https://portal.azure.com/#gallery/Microsoft.DocumentDB) door [een DocumentDB-databaseaccount te maken](documentdb-create-account.md).

Hebt u geen Azure-account? U kunt:

* Meld u aan voor een [gratis proefversie van Azure](https://azure.microsoft.com/free/). U hebt dan 30 dagen en €170 om alle Azure-services te proberen. 
* Als u een MSDN-abonnement hebt, komt u in aanmerking voor [$&150; aan gratis Azure-tegoed per maand](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/). Dit tegoed kunt u gebruiken voor elke willekeurige Azure-service. 
* Download de [Azure DocumentDB-emulator](documentdb-nosql-local-emulator.md) om uw toepassing lokaal te ontwikkelen.

Als u vervolgens meer wilt leren, kunt u ons [leertraject](https://azure.microsoft.com/documentation/learning-paths/documentdb/) bezoeken om door alle beschikbare leerresources te bladeren. 

[1]: ./media/documentdb-introduction/json-database-resources1.png


