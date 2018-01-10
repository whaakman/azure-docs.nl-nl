---
title: 'Azure Cosmos DB: inleiding tot SQL API | Microsoft Docs'
description: Leer hoe u Azure Cosmos DB gebruikt om query's uit te voeren voor grote aantallen JSON-documenten en hoe u deze met een lage latentie opslaat met behulp van SQL en JavaScript).
keywords: json-database, documentdatabase
services: cosmos-db
author: rafats
manager: jhubbard
editor: monicar
documentationcenter: 
ms.assetid: 686cdd2b-704a-4488-921e-8eefb70d5c63
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/22/2017
ms.author: rafats
ms.openlocfilehash: 28c18be81d184766f7b64199c09620238e47cde4
ms.sourcegitcommit: 0e4491b7fdd9ca4408d5f2d41be42a09164db775
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/14/2017
---
# <a name="introduction-to-azure-cosmos-db-sql-api"></a>Inleiding tot Azure Cosmos DB: SQL API

[!INCLUDE [cosmos-db-sql-api](../../includes/cosmos-db-sql-api.md)]

[Azure Cosmos DB](introduction.md) is de wereldwijd gedistribueerde databaseservice met meerdere modellen van Microsoft voor essentiële toepassingen. Azure Cosmos DB biedt [gebruiksklare wereldwijde distributie](distribute-data-globally.md), [elastisch schalen van doorvoer en opslag](partition-data.md), latentie die wereldwijd in het 99e percentiel onder de 10 milliseconden blijft, [vijf goed gedefinieerde consistentieniveaus](consistency-levels.md) en een gegarandeerd hoge beschikbaarheid, allemaal op basis van [toonaangevende serviceovereenkomsten](https://azure.microsoft.com/support/legal/sla/cosmos-db/). Met Azure Cosmos DB worden [gegevens automatisch geïndexeerd](http://www.vldb.org/pvldb/vol8/p1668-shukla.pdf), zonder dat u te maken krijgt met schema- en indexbeheer. Azure Cosmos DB beschikt over meerdere modellen en ondersteunt modellen voor document-, sleutelwaarde-, grafiek- en kolomgegevens.

![Azure SQL API](./media/sql-api-introduction/cosmosdb-sql-api.png) 

Met de SQL API biedt Azure Cosmos DB rijke en vertrouwde [SQL-querymogelijkheden](sql-api-sql-query.md) met consistente lage latenties voor JSON-gegevens zonder schema. In dit artikel krijgt u een overzicht van de Azure Cosmos DB SQL API en leert u hoe u deze kunt gebruiken om grote hoeveelheden JSON-gegevens op te slaan, hoe u er query's voor uitvoert met een latentie van milliseconden en hoe u het schema eenvoudig kunt veranderen. 

## <a name="what-capabilities-and-key-features-does-azure-cosmos-db-offer"></a>Welke mogelijkheden en belangrijke functies biedt Azure Cosmos DB?
Azure Cosmos DB biedt via de SQL API de volgende belangrijke mogelijkheden en voordelen:

* **Elastisch schaalbare doorvoer en opslag:** u kunt uw JSON-database eenvoudig omhoog of omlaag schalen om te voldoen aan de behoeften van uw toepassing. Uw gegevens worden opgeslagen op SSD-schijven (Solid State Disks) voor lage voorspelbare latenties. Azure Cosmos DB ondersteunt containers voor het opslaan van JSON-gegevens (zogenaamde 'verzamelingen'), waarvan de opslaggrootte en ingerichte doorvoer vrijwel onbeperkt kan worden geschaald. Naarmate uw toepassing groeit, kunt u Azure Cosmos DB probleemloos elastisch schalen met voorspelbare prestaties. 


* **Replicatie voor meerdere regio's:** Azure Cosmos DB repliceert uw gegevens transparant naar alle regio's die u aan uw Azure Cosmos DB-account hebt gekoppeld, zodat u toepassingen kunt ontwikkelen waarvoor wereldwijde toegang tot gegevens is vereist, en die tegelijkertijd zorgen voor een balans tussen consistentie, beschikbaarheid en prestaties, allemaal met overeenkomstige garanties. Azure Cosmos DB biedt transparante regionale failover met multihoming-API's en de mogelijkheid om doorvoer en opslag wereldwijd elastisch te schalen. Zie [Distribute data globally with Azure Cosmos DB](distribute-data-globally.md) (Gegevens wereldwijd distribueren met Azure Cosmos DB) voor meer informatie.

* **Ad-hocquery's met bekende SQL-syntaxis:** u kunt heterogene JSON-documenten opslaan en query's op deze documenten uitvoeren via een bekende SQL-syntaxis. Azure Cosmos DB maakt gebruik van een vrijwel gelijktijdige en vergrendelingsvrije indexeringstechnologie op basis van een logboekenstructuur om alle documentinhoud automatisch te indexeren. Zodoende kunt u realtime query's uitvoeren zonder dat u schemahints, secundaire indexen of weergaven hoeft op te geven. Zie [Query Azure Cosmos DB](sql-api-sql-query.md) (Query's uitvoeren voor Azure Cosmos DB) voor meer informatie. 
* **Uitvoeren van JavaScript in de database:** Express-toepassingslogica als opgeslagen procedures en triggers en door de gebruiker gedefinieerde functies (UDF's) met standaard-JavaScript. Zodoende kan uw toepassingslogica worden toegepast op gegevens zonder dat u zich zorgen hoeft te maken over onjuiste overeenkomsten tussen de toepassing en het databaseschema. Met de SQL API kan de JavaScript-toepassingslogica volledig transactioneel en rechtstreeks worden uitgevoerd in de database-engine. Dankzij de diepe integratie van JavaScript kunnen de bewerkingen INSERT, REPLACE, DELETE en SELECT als geïsoleerde transactie worden uitgevoerd vanuit een JavaScript-programma. Zie [Programmeren op de SQL-server](programming.md) voor meer informatie.

* **Instelbare consistentieniveaus:** selecteer een van de vijf goed gedefinieerde consistentieniveaus voor een optimale balans tussen consistentie en prestaties. Voor query's en leesbewerkingen biedt Azure Cosmos DB vijf verschillende consistentieniveaus: sterk, gebonden-verouderd, sessie, consistent voorvoegsel en mogelijk. Op basis van deze gedetailleerde, goed gedefinieerde consistentieniveaus kunt u een goede balans vinden tussen de consistentie, beschikbaarheid en latentie. Zie [Consistentieniveaus gebruiken om de beschikbaarheid en prestaties te maximaliseren](consistency-levels.md) voor meer informatie.

* **Volledig beheerd:** Elimineer de noodzaak om database- en machineresources te beheren. U hoeft voor een volledig beheerde Microsoft Azure-service geen virtuele machines te beheren, software te implementeren en te configureren, schaalwijzigingen te beheren of complexe upgrades voor gegevenslagen uit te voeren. Er wordt automatisch een back-up van elke database gemaakt en de databases worden automatisch beveiligd tegen regionale fouten. U kunt gemakkelijk een Azure Cosmos DB-account toevoegen en capaciteit inrichten als u dit nodig hebt, waardoor u zich kunt richten op uw toepassing in plaats van op beheer en uitvoer van uw database. 

* **Open ontwerp:** U kunt snel aan de slag door gebruik te maken van bestaande vaardigheden en hulpprogramma's. U kunt op een eenvoudige en gebruiksvriendelijke manier programmeren voor de SQL API. Bovendien hebt u geen nieuwe hulpprogramma's nodig en hoeft u geen aangepaste uitbreidingen voor JSON of JavaScript te gebruiken. De volledige databasefunctionaliteit, inclusief CRUD-, query- en JavaScript-verwerking, is toegankelijk via een eenvoudige RESTful HTTP-interface. De SQL API is geschikt voor bestaande indelingen, talen en standaarden, terwijl daarnaast waardevolle databasemogelijkheden worden geboden.

* **Automatisch indexeren:** alle documenten in de database worden standaard automatisch geïndexeerd in Azure Cosmos DB. Er wordt geen schema verwacht of vereist en er hoeven geen secundaire indexen te worden gemaakt. Wilt u niet alles indexeren? Maakt u zich geen zorgen. U kunt ook [paden uitschakelen in uw JSON-bestand](indexing-policies.md).

* **Ondersteuning wijzigingenfeed:** De wijzigingenfeed bevat een gesorteerde lijst met documenten binnen een verzameling van Azure Cosmos DB, in de volgorde waarin ze zijn gewijzigd. Deze feed kan worden gebruikt om te luisteren naar wijzigingen in gegevens om vervolgens gegevens te repliceren, API-aanroepen te activeren of gegevensstroomverwerking uit te voeren op updates. De wijzigingenfeed is automatisch ingeschakeld en eenvoudig te gebruiken: [meer informatie over de wijzigingenfeed](https://docs.microsoft.com/azure/cosmos-db/change-feed). 

## <a name="data-management"></a>Hoe beheer ik gegevens met de SQL-API?
Met de SQL-API kunt u JSON-gegevens beheren via goed gedefinieerde databaseresources. Deze resources worden gerepliceerd voor hoge beschikbaarheid en zijn uniek adresseerbaar op basis van hun logische URI. De SQL-API biedt voor alle resources een eenvoudig RESTful-programmeermodel op basis van HTTP. 


Het Azure Cosmos DB-databaseaccount is een unieke naamruimte die u toegang biedt tot Azure Cosmos DB. Voordat u een databaseaccount kunt maken, moet u een Azure-abonnement hebben. Dit abonnement geeft u toegang tot diverse Azure-services. 

Alle resources binnen Azure Cosmos DB worden gemodelleerd en opgeslagen als JSON-documenten. Resources worden beheerd als items, oftewel JSON-documenten met metagegevens, en als feeds, oftewel verzamelingen items. Sets met items bevinden zich in hun respectieve feeds.

De onderstaande afbeelding toont u de relaties tussen de Azure Cosmos DB-resources:

![De hiërarchische relatie tussen de resources in Azure Cosmos DB][1] 

Een databaseaccount bestaat uit een set databases die elk meerdere verzamelingen bevatten. Elk verzameling kan opgeslagen procedures, triggers, UDF's, documenten en verwante bijlagen bevatten. Er zijn ook gebruikers aan een database gekoppeld. Elke gebruiker beschikt over een set machtigingen voor toegang tot verschillende andere verzamelingen, opgeslagen procedures, triggers, UDF's, documenten of bijlagen. Databases, gebruikers, machtigingen en verzamelingen zijn door het systeem gedefinieerde resources met bekende schema's. Documenten, opgeslagen procedures, triggers, UDF's en bijlagen bevatten echter willekeurige, door de gebruiker gedefinieerde JSON-inhoud.  

## <a name="develop"></a> Hoe kan ik apps ontwikkelen met de SQL-API?

Azure Cosmos DB ontsluit resources via de REST-API's die kunnen worden aangeroepen in elke taal waarmee HTTP-/HTTPS-aanvragen kunnen worden gedaan. Daarnaast worden er voor de SQL-API programmeringsbibliotheken geboden voor verschillende veelgebruikte talen. Dankzij de clientbibliotheken kunt u in vele opzichten eenvoudiger met de API werken, doordat ze de gegevens voor adrescaching, uitzonderingsbeheer, automatische nieuwe pogingen enzovoort verwerken. Bibliotheken zijn momenteel beschikbaar voor de volgende talen en platformen:  

| Downloaden | Documentatie |
| --- | --- |
| [.NET SDK](http://go.microsoft.com/fwlink/?LinkID=402989) |[.NET-bibliotheek](/dotnet/api/overview/azure/cosmosdb?view=azure-dotnet) |
| [Node.js SDK](http://go.microsoft.com/fwlink/?LinkID=402990) |[Node.js-bibliotheek](http://azure.github.io/azure-documentdb-node/) |
| [Java SDK](http://go.microsoft.com/fwlink/?LinkID=402380) |[Java-bibliotheek](/java/api/com.microsoft.azure.documentdb) |
| [JavaScript SDK](http://go.microsoft.com/fwlink/?LinkID=402991) |[JavaScript-bibliotheek](http://azure.github.io/azure-documentdb-js/) |
| N.v.t. |[JavaScript SDK op de server](http://azure.github.io/azure-documentdb-js-server/) |
| [Python SDK](https://pypi.python.org/pypi/pydocumentdb) |[Python-bibliotheek](http://azure.github.io/azure-documentdb-python/) |
| N.v.t. | [API voor MongoDB](mongodb-introduction.md)


Met behulp van de [Azure Cosmos DB-emulator](local-emulator.md) kunt u uw toepassing lokaal ontwikkelen en testen met de SQL-API, kosteloos en zonder dat u een Azure-abonnement hoeft te nemen. Als u tevreden bent over hoe uw toepassing in de emulator werkt, kunt u overstappen naar een Azure Cosmos DB-account in de cloud.

De SQL-API biedt naast de eenvoudige maak-, lees-, bijwerk- en verwijderbewerkingen een geavanceerde SQL-queryinterface voor het ophalen van JSON-documenten en ondersteuning aan de serverzijde voor de transactionele uitvoering van JavaScript-toepassingslogica. De interfaces voor het uitvoeren van query's en scripts zijn beschikbaar via alle platformbibliotheken en REST API's. 

### <a name="sql-query"></a>SQL-query
Azure Cosmos DB biedt ondersteuning voor documentquery's die gebruikmaken van een SQL-taal, die verankerd ligt in het JavaScript-typesysteem, en voor expressies met ondersteuning voor relationele, hiërarchische en ruimtelijke query's. De quertytaal van Azure Cosmos DB is een eenvoudige maar krachtige interface om query's op JSON-documenten uit te voeren. De taal ondersteunt een subset van de ANSI SQL-grammatica en zorgt voor een diepe integratie van JavaScript-object-, -matrix-, -objectconstructie- en functieaanroepen. De SQL-API levert het querymodel zonder een expliciet schema of indexeringshints van de ontwikkelaar.

Door de gebruiker gedefinieerde functies (UDF's) kunnen worden geregistreerd met de SQL-API en er kan als onderdeel van een SQL-query naar worden verwezen, waarmee de grammatica ter ondersteuning van aangepaste toepassingslogica wordt uitgebreid. Deze UDF zijn geschreven als JavaScript-programma's en worden uitgevoerd binnen de database. 

Voor .NET-ontwikkelaars bevat de [.NET SDK](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.linq.aspx) van de SQL-API een LINQ-queryprovider. 

### <a name="transactions-and-javascript-execution"></a>Transacties en JavaScript uitvoeren
Met de SQL-API kunt u toepassingslogica schrijven als benoemde programma's die volledig zijn geschreven in JavaScript. Deze programma's worden geregistreerd voor een verzameling en kunnen databasebewerkingen uitvoeren op de documenten binnen een bepaalde verzameling. JavaScript kan worden geregistreerd voor uitvoering als een trigger, opgeslagen procedure of als een door de gebruiker gedefinieerde functie. Met triggers en opgeslagen procedures kunt u documenten maken, lezen, bijwerken en verwijderen, terwijl de functies die door de gebruiker zijn gedefinieerd, worden uitgevoerd als onderdeel van de logica voor het uitvoeren van een query, zonder schrijftoegang tot de verzameling.

De JavaScript-uitvoering binnen de Cosmos DB is gemodelleerd naar de concepten die worden ondersteund door relationele databasesystemen, met JavaScript als een moderne vervanger van Transact-SQL. Alle JavaScript-logica wordt uitgevoerd binnen een ambient ACID-transactie met het isolatieniveau Snapshot. Als er tijdens de uitvoering een JavaScript-uitzondering optreedt, wordt de volledige transactie afgebroken.

## <a name="next-steps"></a>Volgende stappen
Hebt u al een Azure-account? Vervolgens kunt u aan de slag met Azure Cosmos DB door onze [Quick Starts](../cosmos-db/create-sql-api-dotnet.md) te volgen. Deze bieden informatie over hoe u een account maakt en aan de slag gaat met Cosmos DB.

[1]: ./media/sql-api-introduction/json-database-resources1.png

