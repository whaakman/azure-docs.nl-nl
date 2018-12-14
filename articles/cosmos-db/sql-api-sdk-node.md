---
title: 'Azure Cosmos DB: SQL Node.js-API, SDK en resources'
description: Meer informatie over de Node.js-SQL-API en SDK, inclusief release datums, buiten gebruik stellen datums en wijzigingen die zijn aangebracht tussen elke versie van de Azure Cosmos DB Node.js-SDK.
services: cosmos-db
author: deborahc
ms.service: cosmos-db
ms.component: cosmosdb-sql
ms.devlang: nodejs
ms.topic: reference
ms.date: 09/24/2018
ms.author: rnagpal
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 08e928143e8e54797869a06f4f7b99fd89cdcacc
ms.sourcegitcommit: edacc2024b78d9c7450aaf7c50095807acf25fb6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/13/2018
ms.locfileid: "53343986"
---
# <a name="azure-cosmos-db-nodejs-sdk-for-sql-api-release-notes-and-resources"></a>Azure Cosmos DB Node.js-SDK voor SQL-API: Opmerkingen bij de release en resources
> [!div class="op_single_selector"]
> * [.NET](sql-api-sdk-dotnet.md)
> * [.NET-Wijzigingenfeed](sql-api-sdk-dotnet-changefeed.md)
> * [.NET Core](sql-api-sdk-dotnet-core.md)
> * [Node.js](sql-api-sdk-node.md)
> * [Async Java](sql-api-sdk-async-java.md)
> * [Java](sql-api-sdk-java.md)
> * [Python](sql-api-sdk-python.md)
> * [REST](https://docs.microsoft.com/rest/api/cosmos-db/)
> * [REST-resourceprovider](https://docs.microsoft.com/rest/api/cosmos-db-resource-provider/)
> * [SQL](sql-api-query-reference.md)
> * [BulkExecutor - .NET](sql-api-sdk-bulk-executor-dot-net.md)
> * [BulkExecutor - Java](sql-api-sdk-bulk-executor-java.md)

|Resource  |Koppeling  |
|---------|---------|
|SDK downloaden  |   [NPM](https://www.npmjs.com/package/@azure/cosmos) 
|Documentatie voor API  |  [JavaScript SDK-referentiedocumentatie](https://docs.microsoft.com/javascript/api/%40azure/cosmos/?view=azure-node-latest)
|SDK-installatie-instructies  |  [Installatie-instructies](https://github.com/Azure/azure-cosmos-js#installation)
|Bijdragen aan de SDK | [GitHub](https://github.com/Azure/azure-cosmos-js/tree/master)
| Voorbeelden | [Node.js-codevoorbeelden](sql-api-nodejs-samples.md)
| Zelfstudie aan de slag | [Aan de slag met de JavaScript-SDK](sql-api-nodejs-get-started.md)
| Zelfstudie-web-app | [Een Node.js-webtoepassing met Azure Cosmos DB](sql-api-nodejs-application.md)
| Huidige ondersteund platform | [Node.js versie 6.x](https://nodejs.org/en/blog/release/v6.10.3/) - vereist voor SDK-versie 2.0.0 en hoger.<br/>[Node.js v4.2.0](https://nodejs.org/en/blog/release/v4.2.0/)<br/> [Node.js v0.12](https://nodejs.org/en/blog/release/v0.12.0/)<br/> [Node.js v0.10](https://nodejs.org/en/blog/release/v0.10.0/) 

## <a name="release-notes"></a>Releaseopmerkingen

### <a name="2.0.5"/>2.0.5</a>
* Interface voor knooppunt agenttype toevoegen. Typescript gebruikers hoeft niet langer voor het installeren van @types/node als een afhankelijkheid
* Voorkeurslocaties worden nu goed herkend
* Verbeteringen in de documentatie voor ontwikkelaars die bijdragen aan
* Verschillende typefout oplossingen

### <a name="2.0.4"/>2.0.4</a>
* Oplossingen Typ definitie probleem die is geïntroduceerd in 2.0.3

### <a name="2.0.3"/>2.0.3</a>
* Verwijder `big-integer` afhankelijkheid
* Schakel over naar de richtlijnen voor AsyncIterable type verwijzing. Typescript-gebruikers hebben niet langer voor het aanpassen van de instelling 'lib'.
* Typefout oplossingen

### <a name="2.0.2"/>2.0.2</a>
* Leesmij-bestand koppelingen herstellen

### <a name="2.0.1"/>2.0.1</a>
* Herstellen van de implementatie van de interface opnieuw proberen

### <a name="2.0.0"/>2.0.0</a>
* De algemene beschikbaarheid van versie 2.0.0 van de JavaScript-SDK
* Ondersteuning toegevoegd voor schrijfbewerkingen in meerdere regio's.

### <a name="2.0.0-3"/>2.0.0-3</a>
* Als u RC1 van versie 2.0.0 van de JavaScript-SDK voor de openbare preview.
* Nieuw objectmodel, met methoden en op het hoogste niveau CosmosClient verdeeld over de relevante klassen van de Database, -Container en Item. 
* Ondersteuning voor [belooft](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Using_promises). 
* SDK geconverteerd naar TypeScript.

### <a name="1.14.4"/>1.14.4</a>
* npm documentatie wordt opgelost.

### <a name="1.14.3"/>1.14.3</a>
* Ondersteuning toegevoegd voor nieuwe pogingen standaard op problemen met verbindingen.
* Ondersteuning toegevoegd voor het lezen van de verzameling wijzigen-feed.
* Vaste sessie consistentie bug die niet continu veroorzaakt 'lezen sessie niet beschikbaar'.
* Er is ondersteuning toegevoegd voor query metrische gegevens.
* Http-Agent kunt u het maximum aantal verbindingen is gewijzigd.

### <a name="1.14.2"/>1.14.2</a>
* Bijgewerkte documentatie om te verwijzen naar Azure Cosmos DB in plaats van Azure DocumentDB.
* Er is ondersteuning toegevoegd voor de instelling proxyUrl in ConnectionPolicy.

### <a name="1.14.1"/>1.14.1</a>
* Secundaire fix voor hoofdlettergevoelig bestandssystemen.

### <a name="1.14.0"/>1.14.0</a>
* Voegt ondersteuning toe voor de consistentie van de sessie.
* Deze SDK-versie vereist de meest recente versie van Azure Cosmos DB Emulator beschikbaar voor downloaden van https://aka.ms/cosmosdb-emulator.

### <a name="1.13.0"/>1.13.0</a>
* Gesplitste identificatienummers cross-partitie query's.
* Biedt ondersteuning voor resourcekoppeling met voorloop- en volgspaties slashes (en de bijbehorende tests) toegevoegd.

### <a name="1.12.2"/>1.12.2</a>
*   npm documentatie wordt opgelost.

### <a name="1.12.1"/>1.12.1</a>
* Vaste een bug in executeStoredProcedure documenten die betrokken zijn waar speciale Unicode-tekens (LS, PS).
* Er is een fout in verwerking van documenten met Unicode-tekens in de partitiesleutel opgelost.
* Vaste ondersteuning voor het maken van verzamelingen met de naam van media. GitHub-probleem #114.
* Vaste ondersteuning voor de machtiging van de verificatietoken. GitHub-probleem #178.

### <a name="1.12.0"/>1.12.0</a>
* Ondersteuning toegevoegd voor een nieuwe [consistentieniveau](consistency-levels.md) ConsistentPrefix genoemd.
* Ondersteuning toegevoegd voor UriFactory.
* Probleem opgelost een Unicode-ondersteuning. GitHub-probleem #171.

### <a name="1.11.0"/>1.11.0</a>
* De ondersteuning voor aggregatie-query's (aantal, MIN, MAX, som en gemiddelde) toegevoegd.
* De optie voor het beheren van de graad van parallelle uitvoering voor cross-partitie query's toegevoegd.
* De optie voor het uitschakelen van SSL-verificatie met het uitvoeren in Azure Cosmos DB-Emulator toegevoegd.
* Het minimale doorvoer op gepartitioneerde verzamelingen van 10,100 RU/s 2500 RU/s verlaagd.
* Het probleem opgelost voortzetting van token voor de verzameling met één partitie. GitHub-probleem #107.
* Het probleem opgelost executeStoredProcedure bij het verwerken van 0 als één parameter. GitHub-probleem #155.

### <a name="1.10.2"/>1.10.2</a>
* Vaste gebruikersagent header om op te nemen van de SDK-versie.
* Opruimen van de secundaire code.

### <a name="1.10.1"/>1.10.1</a>
* SSL-verificatie uitschakelen wanneer met de SDK voor de emulator(hostname=localhost).
* Er is ondersteuning toegevoegd voor het inschakelen van logboekregistratie voor scripts tijdens het uitvoeren van de opgeslagen procedure.

### <a name="1.10.0"/>1.10.0</a>
* Ondersteuning toegevoegd voor cross-partitie parallelle query's.
* Ondersteuning toegevoegd voor bovenste/ORDER BY-query's voor gepartitioneerde verzamelingen.

### <a name="1.9.0"/>1.9.0</a>
* Beleid-ondersteuning voor extra nieuwe pogingen voor beperkte aanvragen. (Beperkte aanvragen ontvangen een aanvraag snelheid te groot uitzondering, foutcode 429.) Standaard Azure Cosmos DB pogingen negen keer voor elke aanvraag wanneer foutcode 429 is aangetroffen, naleven van de tijd retryAfter in de antwoordkop. Het interval voor een vaste opnieuw proberen kan nu worden ingesteld als onderdeel van de eigenschap RetryOptions op het object ConnectionPolicy als u wilt de retryAfter tijd geretourneerd door server tussen de pogingen negeren. Azure Cosmos DB is nu moet wachten tot een maximum van 30 seconden voor elke aanvraag die wordt beperkt (ongeacht het aantal nieuwe pogingen) en wordt het antwoord met foutcode 429 geretourneerd. Deze tijd kan ook worden overschreven in de eigenschap RetryOptions op ConnectionPolicy-object.
* Cosmosdb retourneert nu x-ms-throttle--het aantal nieuwe pogingen en x-ms-throttle-retry-wait-time-ms als de antwoordheaders in elke aanvraag om aan te geven van de vertraging opnieuw proberen het aantal en de cumulatieve tijd die de aanvraag heeft gewacht tussen de nieuwe pogingen.
* De klasse RetryOptions is toegevoegd, de eigenschap RetryOptions op de ConnectionPolicy-klasse die kan worden gebruikt voor de onderdrukking van enkele van de standaardopties voor opnieuw proberen om vrij te geven.

### <a name="1.8.0"/>1.8.0</a>
* De ondersteuning voor databaseaccounts voor meerdere regio's toegevoegd.

### <a name="1.7.0"/>1.7.0</a>
* De ondersteuning toegevoegd voor tijd-Live(TTL)-functie voor documenten.

### <a name="1.6.0"/>1.6.0</a>
* Geïmplementeerd [gepartitioneerde verzamelingen](partition-data.md) en [niveau van de gebruiker gedefinieerde prestaties](performance-levels.md).

### <a name="1.5.6"/>1.5.6</a>
* Fout verholpen RangePartitionResolver.resolveForRead waar is het niet koppelingen vanwege een ongeldige concat van resultaten retourneren.

### <a name="1.5.5"/>1.5.5</a>
* Vaste hashPartitionResolver resolveForRead(): Wanneer is geen opgegeven partitiesleutel die uitzondering, in plaats van een lijst met alle geregistreerde koppelingen retourneren veroorzaakt.

### <a name="1.5.4"/>1.5.4</a>
* Probleem opgelost [#100](https://github.com/Azure/azure-documentdb-node/issues/100) -HTTPS-Agent toegewezen: Vermijd het wijzigen van de globale agent voor Azure Cosmos DB-doeleinden. Gebruik een toegewezen agent voor alle aanvragen van de lib.

### <a name="1.5.3"/>1.5.3</a>
* Probleem opgelost [#81](https://github.com/Azure/azure-documentdb-node/issues/81) - correct streepjes in de media-ID's worden verwerkt.

### <a name="1.5.2"/>1.5.2</a>
* Probleem opgelost [#95](https://github.com/Azure/azure-documentdb-node/issues/95) -EventEmitter listener lekken waarschuwing.

### <a name="1.5.1"/>1.5.1</a>
* Probleem opgelost [#92](https://github.com/Azure/azure-documentdb-node/issues/90) -Wijzig de naam van map Hash op hash voor hoofdlettergevoelig systemen.

### <a name="1.5.0"/>1.5.0</a>
* Sharding-ondersteuning voor het implementeren door hash & bereik partitie resolvers toe te voegen.

### <a name="1.4.0"/>1.4.0</a>
* Upsert implementeren. Nieuwe upsertXXX methoden op documentClient.

### <a name="1.3.0"/>1.3.0</a>
* Overgeslagen zodat de versienummers in overeenstemming met andere SDK's.

### <a name="1.2.2"/>1.2.2</a>
* Gesplitste Q belooft wrapper naar nieuwe opslagplaats.
* Bijwerken naar een pakketbestand voor npm-register.

### <a name="1.2.1"/>1.2.1</a>
* Implementeert-ID op basis van routering.
* Probleem opgelost [#49](https://github.com/Azure/azure-documentdb-node/issues/49) -huidige eigenschap strijdig is met de methode current().

### <a name="1.2.0"/>1.2.0</a>
* Er is ondersteuning toegevoegd voor georuimtelijke index.
* Hiermee valideert u de eigenschap id voor alle resources. Kunnen geen id's voor resources bevatten?, /, #, &#47; &#47;, tekens bevatten of eindigen met een spatie.
* Nieuwe header 'index transformatie uitgevoerd' toevoegen aan ResourceResponse.

### <a name="1.1.0"/>1.1.0</a>
* Indexeringsbeleid V2 implementeert.

### <a name="1.0.3"/>1.0.3</a>
* Probleem [#40](https://github.com/Azure/azure-documentdb-node/issues/40) - eslint geïmplementeerd en configuraties in de kern knorvis en beloven SDK.

### <a name="1.0.2"/>1.0.2</a>
* Probleem [#45](https://github.com/Azure/azure-documentdb-node/issues/45) -wrapper beloften bevat geen header met de fout.

### <a name="1.0.1"/>1.0.1</a>
* De mogelijkheid om query op conflicten door toe te voegen readConflicts readConflictAsync en queryConflicts is geïmplementeerd.
* Bijgewerkte API-documentatie.
* Probleem [#41](https://github.com/Azure/azure-documentdb-node/issues/41) -client.createDocumentAsync fout.

### <a name="1.0.0"/>1.0.0</a>
* SDK VOOR ALGEMENE BESCHIKBAARHEID.

## <a name="release--retirement-dates"></a>Release & datums buiten gebruik stellen
Microsoft biedt melding ten minste **12 maanden** voorafgaand aan buiten gebruik stellen van een SDK soepel te verwerken de overgang naar een nieuwere/ondersteunde versie.

Nieuwe functies en functionaliteit en -optimalisatie worden alleen toegevoegd aan de huidige SDK, daarom is het raadzaam dat u altijd een upgrade uitvoert naar de nieuwste SDK versie zo vroeg mogelijk.

Elke aanvraag voor het gebruik van Cosmos DB dat een buiten gebruik gestelde SDK is geweigerd door de service.

<br/>

| Versie | Releasedatum | Vervaldatum |
| --- | --- | --- |
| [2.0.0-3 (RC)](#2.0.0-3) |2 augustus 2018 |--- |
| [1.14.4](#1.14.4) |03 mei 2018 |--- |
| [1.14.3](#1.14.3) |03 mei 2018 |--- |
| [1.14.2](#1.14.2) |21 december 2017 |--- |
| [1.14.1](#1.14.1) |10 november 2017 |--- |
| [1.14.0](#1.14.0) |9 november 2017 |--- |
| [1.13.0](#1.13.0) |11 oktober 2017 |--- |
| [1.12.2](#1.12.2) |10 augustus 2017 |--- |
| [1.12.1](#1.12.1) |10 augustus 2017 |--- |
| [1.12.0](#1.12.0) |10 mei 2017 |--- |
| [1.11.0](#1.11.0) |16 maart 2017 |--- |
| [1.10.2](#1.10.2) |27 januari 2017 |--- |
| [1.10.1](#1.10.1) |22 december 2016 |--- |
| [1.10.0](#1.10.0) |03 oktober 2016 |--- |
| [1.9.0](#1.9.0) |07 juli 2016 |--- |
| [1.8.0](#1.8.0) |14 juni 2016 |--- |
| [1.7.0](#1.7.0) |26 april 2016 |--- |
| [1.6.0](#1.6.0) |29 maart 2016 |--- |
| [1.5.6](#1.5.6) |08 maart 2016 |--- |
| [1.5.5](#1.5.5) |02 februari 2016 |--- |
| [1.5.4](#1.5.4) |01 februari 2016 |--- |
| [1.5.2](#1.5.2) |26 januari 2016 |--- |
| [1.5.2](#1.5.2) |22 januari 2016 |--- |
| [1.5.1](#1.5.1) |4 januari 2016 |--- |
| [1.5.0](#1.5.0) |En met 31 december 2015 |--- |
| [1.4.0](#1.4.0) |06 oktober 2015 |--- |
| [1.3.0](#1.3.0) |06 oktober 2015 |--- |
| [1.2.2](#1.2.2) |Op 10 september 2015 |--- |
| [1.2.1](#1.2.1) |15 augustus 2015 |--- |
| [1.2.0](#1.2.0) |05 augustus 2015 |--- |
| [1.1.0](#1.1.0) |09 juli 2015 |--- |
| [1.0.3](#1.0.3) |04 juni 2015 |--- |
| [1.0.2](#1.0.2) |23 mei 2015 |--- |
| [1.0.1](#1.0.1) |15 mei 2015 |--- |
| [1.0.0](#1.0.0) |08 april 2015 |--- |

## <a name="faq"></a>Veelgestelde vragen
[!INCLUDE [cosmos-db-sdk-faq](../../includes/cosmos-db-sdk-faq.md)]

## <a name="see-also"></a>Zie ook
Zie voor meer informatie over Cosmos DB, [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) servicepagina.

