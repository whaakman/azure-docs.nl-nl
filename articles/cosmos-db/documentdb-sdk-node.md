---
title: Azure Cosmos DB Node.js-API, SDK en Resources | Microsoft Docs
description: Meer informatie over de Node.js-API en de SDK, inclusief release datums, buiten gebruik stellen datums en wijzigingen die zijn aangebracht tussen elke versie van de Azure-SDK voor Node.js Cosmos DB.
services: cosmos-db
documentationcenter: nodejs
author: rnagpal
manager: jhubbard
editor: cgronlun
ms.assetid: 9d5621fa-0e11-4619-a28b-a19d872bcf37
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 08/14/2017
ms.author: rnagpal
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 4376a5c07b5f00311ce0fe3c0056efdf79c273f9
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2017
---
# <a name="azure-cosmos-db-nodejs-sdk-release-notes-and-resources"></a>Azure DB Cosmos Node.js SDK: Releaseopmerkingen en resources
> [!div class="op_single_selector"]
> * [.NET](documentdb-sdk-dotnet.md)
> * [.NET-Feed van wijzigen](documentdb-sdk-dotnet-changefeed.md)
> * [.NET Core](documentdb-sdk-dotnet-core.md)
> * [Node.js](documentdb-sdk-node.md)
> * [Java](documentdb-sdk-java.md)
> * [Python](documentdb-sdk-python.md)
> * [REST](https://docs.microsoft.com/rest/api/documentdb/)
> * [REST-resourceprovider](https://docs.microsoft.com/rest/api/documentdbresourceprovider/)
> * [SQL](https://msdn.microsoft.com/library/azure/dn782250.aspx)
> 
> 

<table>

<tr><td>**SDK downloaden**</td><td>[NPM](https://www.npmjs.com/package/documentdb)</td></tr>

<tr><td>**API-documentatie**</td><td>[Node.js-API-naslagdocumentatie](http://azure.github.io/azure-documentdb-node/DocumentClient.html)</td></tr>

<tr><td>**SDK-installatie-instructies**</td><td>[Installatie-instructies](http://azure.github.io/azure-documentdb-node/)</td></tr>

<tr><td>**Bijdragen aan de SDK**</td><td>[GitHub](https://github.com/Azure/azure-documentdb-node/tree/master/source)</td></tr>

<tr><td>**Voorbeelden**</td><td>[Node.js-codevoorbeelden](documentdb-nodejs-samples.md)</td></tr>

<tr><td>**Zelfstudie Aan de slag**</td><td>[Aan de slag met de Node.js-SDK](documentdb-nodejs-get-started.md)</td></tr>

<tr><td>**Zelfstudie voor web-app**</td><td>[Een Node.js-webtoepassing met behulp van Azure DB die Cosmos bouwen](documentdb-nodejs-application.md)</td></tr>

<tr><td>**Huidige ondersteund platform**</td><td> 
[Node.js versie 6.x](https://nodejs.org/en/blog/release/v6.10.3/)<br/> 
[Node.js v4.2.0](https://nodejs.org/en/blog/release/v4.2.0/)<br/> 
[Node.js v0.12](https://nodejs.org/en/blog/release/v0.12.0/)<br/> 
[Node.js v0.10](https://nodejs.org/en/blog/release/v0.10.0/) 
</td></tr>
</table></br>

## <a name="release-notes"></a>Releaseopmerkingen

### <a name="1.12.2"/>1.12.2</a>
*   npm documentatie is opgelost.

### <a name="1.12.1"/>1.12.1</a>
* Heeft een fout in de executeStoredProcedure documenten die betrokken zijn waar speciale Unicode-tekens (LS, PS).
* Een fout bij het verwerken van documenten met Unicode-tekens in de partitiesleutel vast.
* Vaste ondersteuning voor het maken van verzamelingen met de naam van media. Github probleem #114.
* Vaste ondersteuning voor machtiging verificatietoken. Github probleem #178.

### <a name="1.12.0"/>1.12.0</a>
* Ondersteuning toegevoegd voor een nieuwe [consistentieniveau](consistency-levels.md) ConsistentPrefix aangeroepen.
* Ondersteuning toegevoegd voor UriFactory.
* Vaste een bug Unicode-ondersteuning. GitHub probleem #171.

### <a name="1.11.0"/>1.11.0</a>
* Het is ondersteuning toegevoegd voor aggregatie van query's (COUNT, MIN, MAX, SUM en Gem).
* De optie voor het beheren van de mate van parallelle uitvoering voor cross-partitie query's toegevoegd.
* De optie voor het uitschakelen van SSL-verificatie bij het uitvoeren in Azure Cosmos DB Emulator toegevoegd.
* Minimaal doorvoer voor gepartitioneerde verzamelingen uit 10,100 RU/s tot 2500 RU/s verlaagd.
* De voortzetting token oplossingen voor de verzameling van één partitie vast. Github probleem #107.
* De executeStoredProcedure-fout bij het verwerken van 0 als één param vast. Github probleem #155.

### <a name="1.10.2"/>1.10.2</a>
* Vaste gebruikersagent header om op te nemen van de SDK-versie.
* Het opruimen van de secundaire code.

### <a name="1.10.1"/>1.10.1</a>
* SSL-verificatie wordt uitgeschakeld wanneer u de SDK toe te passen de emulator(hostname=localhost).
* Ondersteuning toegevoegd voor het inschakelen van logboekregistratie script tijdens het uitvoeren van de opgeslagen procedure.

### <a name="1.10.0"/>1.10.0</a>
* Ondersteuning toegevoegd voor cross-partitie parallelle query's.
* Ondersteuning toegevoegd voor TOP/ORDER BY-query's voor gepartitioneerde verzamelingen.

### <a name="1.9.0"/>1.9.0</a>
* Toegevoegde opnieuw Beleidsondersteuning voor beperkte aanvragen. (Beperkte aanvragen ontvangen van een aanvraag snelheid te groot uitzondering, foutcode 429.) Standaard Azure Cosmos DB pogingen negen keer voor elke aanvraag wanneer foutcode 429 is opgetreden, de tijd retryAfter in de antwoordheader naleven. Een vaste opnieuw tijdsinterval kan nu worden ingesteld als onderdeel van de eigenschap RetryOptions op het object ConnectionPolicy als u wilt de retryAfter tijd geretourneerd door server tussen de pogingen negeren. Azure Cosmos-DB wordt nu gewacht op maximaal 30 seconden voor elke aanvraag die wordt beperkt (ongeacht het aantal pogingen) en het antwoord met foutcode 429 retourneert. Deze tijd kan ook worden overschreven in de eigenschap RetryOptions in ConnectionPolicy-object.
* Cosmos DB retourneert nu x-ms-versnelling-aantal nieuwe pogingen en x-ms-throttle-retry-wait-time-ms de antwoordheaders in elke aanvraag om aan te geven van de beperking probeer aantal en de cumulatieve tijd dat de aanvraag worden gewacht tussen de nieuwe pogingen.
* De klasse RetryOptions is toegevoegd, blootstellen van de eigenschap RetryOptions in de klasse ConnectionPolicy die kan worden gebruikt voor het onderdrukken van enkele van de standaardopties voor opnieuw proberen.

### <a name="1.8.0"/>1.8.0</a>
* De ondersteuning voor meerdere landen/regio database accounts toegevoegd.

### <a name="1.7.0"/>1.7.0</a>
* De ondersteuning voor de functie tijd-Live(TTL) voor documenten toegevoegd.

### <a name="1.6.0"/>1.6.0</a>
* Geïmplementeerd [gepartitioneerde verzamelingen](partition-data.md) en [prestatieniveaus die door de gebruiker gedefinieerde](performance-levels.md).

### <a name="1.5.6"/>1.5.6</a>
* Vaste RangePartitionResolver.resolveForRead bug waar koppelingen vanwege een onjuiste concat resultaten is niet geretourneerd.

### <a name="1.5.5"/>1.5.5</a>
* Vaste hashParitionResolver resolveForRead(): wanneer geen partitiesleutel opgegeven is er uitzondering is opgetreden, in plaats van een lijst met alle geregistreerde koppelingen retourneren.

### <a name="1.5.4"/>1.5.4</a>
* Probleem opgelost [#100](https://github.com/Azure/azure-documentdb-node/issues/100) -Agent voor HTTPS-specifieke: voorkomen wijzigen van de globale agent voor Azure Cosmos DB doeleinden. Een speciale agent gebruiken voor alle aanvragen van de lib.

### <a name="1.5.3"/>1.5.3</a>
* Probleem opgelost [#81](https://github.com/Azure/azure-documentdb-node/issues/81) - goed streepjes in de media-ID's verwerkt.

### <a name="1.5.2"/>1.5.2</a>
* Probleem opgelost [#95](https://github.com/Azure/azure-documentdb-node/issues/95) -listener EventEmitter lekken waarschuwing.

### <a name="1.5.1"/>1.5.1</a>
* Probleem opgelost [#92](https://github.com/Azure/azure-documentdb-node/issues/90) -naam wijzigen van de map Hash op hash voor systemen die hoofdlettergevoelig.

### <a name="1.5.0"/>1.5.0</a>
* Implementeer sharding ondersteuning door hash & bereik partitie resolvers toe te voegen.

### <a name="1.4.0"/>1.4.0</a>
* Upsert implementeren. Nieuwe upsertXXX-methoden op documentClient.

### <a name="1.3.0"/>1.3.0</a>
* Overgeslagen zodat versienummers realistisch met andere SDK's.

### <a name="1.2.2"/>1.2.2</a>
* Gesplitste Q belooft wrapper naar nieuwe opslagplaats.
* Bijwerken naar pakketbestand voor serverregister npm.

### <a name="1.2.1"/>1.2.1</a>
* Implements-ID gebaseerde routering.
* Probleem opgelost [#49](https://github.com/Azure/azure-documentdb-node/issues/49) -eigenschap current veroorzaakt een conflict met de methode current().

### <a name="1.2.0"/>1.2.0</a>
* Ondersteuning toegevoegd voor georuimtelijke index.
* De eigenschap id voor alle resources valideert. Kunnen geen id's voor bronnen bevatten?, /, #, &#47; &#47; tekens of eindigen met een spatie.
* Voegt nieuwe header 'index transformatie uitgevoerd' aan ResourceResponse.

### <a name="1.1.0"/>1.1.0</a>
* V2-indexeringsbeleid implementeert.

### <a name="1.0.3"/>1.0.3</a>
* Probleem [#40](https://github.com/Azure/azure-documentdb-node/issues/40) - eslint geïmplementeerd en configuraties in de kern knorvis en beloven SDK.

### <a name="1.0.2"/>1.0.2</a>
* Probleem [#45](https://github.com/Azure/azure-documentdb-node/issues/45) -pas wrapper bevat geen header met de fout.

### <a name="1.0.1"/>1.0.1</a>
* Geïmplementeerde mogelijkheid om de query voor conflicten door toe te voegen readConflicts readConflictAsync en queryConflicts.
* Bijgewerkte API-documentatie.
* Probleem [#41](https://github.com/Azure/azure-documentdb-node/issues/41) -client.createDocumentAsync-fout.

### <a name="1.0.0"/>1.0.0</a>
* GA SDK.

## <a name="release--retirement-dates"></a>Release & buiten gebruik stellen datums
Microsoft biedt melding ten minste **12 maanden** voordat het buiten gebruik stellen van een SDK om de overgang naar een nieuwere/ondersteunde versie vloeiend.

Nieuwe functies en functionaliteit en optimalisaties alleen zijn toegevoegd aan de huidige SDK, als zodanig wordt aanbevolen dat u altijd een upgrade uitvoert naar de nieuwste SDK versie zo snel mogelijk.

Een aanvraag voor het gebruik van de Cosmos-DB dat een buiten gebruik gestelde SDK is geweigerd door de service.

<br/>

| Versie | Releasedatum | Vervaldatum |
| --- | --- | --- |
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
| [1.5.0](#1.5.0) |31 december 2015 |--- |
| [1.4.0](#1.4.0) |06 oktober 2015 |--- |
| [1.3.0](#1.3.0) |06 oktober 2015 |--- |
| [1.2.2](#1.2.2) |10 september 2015 |--- |
| [1.2.1](#1.2.1) |15 augustus 2015 |--- |
| [1.2.0](#1.2.0) |05 augustus 2015 |--- |
| [1.1.0](#1.1.0) |09 juli 2015 |--- |
| [1.0.3](#1.0.3) |04 juni 2015 |--- |
| [1.0.2](#1.0.2) |23 mei 2015 |--- |
| [1.0.1](#1.0.1) |Op 15 mei 2015 |--- |
| [1.0.0](#1.0.0) |08 april 2015 |--- |

## <a name="faq"></a>Veelgestelde vragen
[!INCLUDE [cosmos-db-sdk-faq](../../includes/cosmos-db-sdk-faq.md)]

## <a name="see-also"></a>Zie ook
Zie voor meer informatie over Cosmos DB, [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) pagina van de service.

