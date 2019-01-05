---
title: 'Azure Cosmos DB: SQL Python-API, SDK en resources'
description: Meer informatie over de SQL Python-API en SDK, inclusief release datums, buiten gebruik stellen datums en wijzigingen die zijn aangebracht tussen elke versie van de Python-SDK van Azure Cosmos DB.
author: SnehaGunda
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.devlang: python
ms.topic: reference
ms.date: 11/29/2018
ms.author: sngun
ms.openlocfilehash: 01f6fe3ef309f19896befe61512d524a8543745f
ms.sourcegitcommit: 8330a262abaddaafd4acb04016b68486fba5835b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2019
ms.locfileid: "54041455"
---
# <a name="azure-cosmos-db-python-sdk-for-sql-api-release-notes-and-resources"></a>Azure Cosmos DB Python-SDK voor SQL-API: Opmerkingen bij de release en resources
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

<table>

<tr><td>**SDK downloaden**</td><td>[PyPI](https://pypi.org/project/azure-cosmos)</td></tr>

<tr><td>**API-documentatie**</td><td>[Python-API-referentiedocumentatie](https://docs.microsoft.com/python/api/overview/azure/cosmosdb?view=azure-python)</td></tr>

<tr><td>**SDK-installatie-instructies**</td><td>[Python-SDK-installatie-instructies](https://github.com/Azure/azure-cosmos-python)</td></tr>

<tr><td>**Bijdragen aan de SDK**</td><td>[GitHub](https://github.com/Azure/azure-cosmos-python)</td></tr>

<tr><td>**Aan de slag**</td><td>[Aan de slag met de Python-SDK](sql-api-python-application.md)</td></tr>

<tr><td>**Huidige ondersteund platform**</td><td>[Python 2.7](https://www.python.org/downloads/) en [Python 3.5](https://www.python.org/downloads/)</td></tr>
</table></br>

## <a name="release-notes"></a>Releaseopmerkingen

### <a name="a-name302302"></a><a name="3.0.2"/>3.0.2
* Er is ondersteuning toegevoegd voor MultiPolygon gegevenstype
* Opgelost probleem in de sessie lezen beleid voor opnieuw proberen
* Opgelost probleem voor problemen bij het decoderen van tekenreeksen met base 64 onjuist opvulling

### <a name="a-name301301"></a><a name="3.0.1"/>3.0.1
* Opgelost probleem in LocationCache
* Pogingslogica voor foutoplossing eindpunt
* Vaste documentatie

### <a name="a-name300300"></a><a name="3.0.0"/>3.0.0
* Ondersteuning voor schrijfbewerkingen in meerdere regio's.
* Namespace gewijzigd in azure.cosmos.
* Verzameling en het document concepten gewijzigd in de container en object, document_client gewijzigd in cosmos_client. 

### <a name="a-name233233"></a><a name="2.3.3"/>2.3.3
* Er is ondersteuning toegevoegd voor proxy
* Er is ondersteuning toegevoegd voor het lezen van wijzigingenfeed
* Er is ondersteuning toegevoegd voor verzameling quotum kopteksten
* Bugfix voor grote sessie tokens probleem
* Bugfix voor ReadMedia API
* Bugfix in de partitie sleutelbereik cache

### <a name="a-name232232"></a><a name="2.3.2"/>2.3.2
* Ondersteuning toegevoegd voor nieuwe pogingen standaard op problemen met verbindingen.

### <a name="a-name231231"></a><a name="2.3.1"/>2.3.1
* Bijgewerkte documentatie om te verwijzen naar Azure Cosmos DB in plaats van Azure DocumentDB.

### <a name="a-name230230"></a><a name="2.3.0"/>2.3.0
* Deze SDK-versie vereist de meest recente versie van Azure Cosmos DB Emulator beschikbaar voor downloaden van https://aka.ms/cosmosdb-emulator.

### <a name="a-name221221"></a><a name="2.2.1"/>2.2.1
* Opgelost probleem voor statistische woordenlijst.
* Opgelost probleem voor bijsnijden slashes in de resourcekoppeling.
* Toegevoegde tests voor Unicode-codering.

### <a name="a-name220220"></a><a name="2.2.0"/>2.2.0
* Er is ondersteuning toegevoegd voor een nieuwe consistentieniveau ConsistentPrefix genoemd.


### <a name="a-name210210"></a><a name="2.1.0"/>2.1.0
* Er is ondersteuning toegevoegd voor aggregatie-query's (aantal, MIN, MAX, som en gemiddelde).
* Een optie voor het uitschakelen van SSL-verificatie met het uitvoeren in Cosmos DB-Emulator toegevoegd.
* De beperking van afhankelijke aanvragen module moet exact 2.10.0 verwijderd.
* Het minimale doorvoer op gepartitioneerde verzamelingen van 10,100 RU/s 2500 RU/s verlaagd.
* Er is ondersteuning toegevoegd voor het inschakelen van logboekregistratie voor scripts tijdens het uitvoeren van de opgeslagen procedure.
* REST-API-versie tegenaan op ' 2017-01-19' in deze versie.

### <a name="a-name201201"></a><a name="2.0.1"/>2.0.1
* Redactionele wijzigingen aangebracht in de documentatie bij opmerkingen.

### <a name="a-name200200"></a><a name="2.0.0"/>2.0.0
* Ondersteuning toegevoegd voor Python 3.5.
* Ondersteuning toegevoegd voor groepsgewijze verbinding met een module aanvragen.
* Er is ondersteuning toegevoegd voor de sessieconsistentie.
* Er is ondersteuning toegevoegd voor bovenste/ORDERBY-query's voor gepartitioneerde verzamelingen.

### <a name="a-name190190"></a><a name="1.9.0"/>1.9.0
* Beleid-ondersteuning voor extra nieuwe pogingen voor beperkte aanvragen. (Beperkte aanvragen ontvangen een aanvraag snelheid te groot uitzondering, foutcode 429.) Standaard Azure Cosmos DB pogingen negen keer voor elke aanvraag wanneer foutcode 429 is aangetroffen, naleven van de tijd retryAfter in de antwoordkop. Het interval voor een vaste opnieuw proberen kan nu worden ingesteld als onderdeel van de eigenschap RetryOptions op het object ConnectionPolicy als u wilt de retryAfter tijd geretourneerd door server tussen de pogingen negeren. Azure Cosmos DB is nu moet wachten tot een maximum van 30 seconden voor elke aanvraag die wordt beperkt (ongeacht het aantal nieuwe pogingen) en wordt het antwoord met foutcode 429 geretourneerd. Deze tijd kan ook worden overschreven in de eigenschap RetryOptions op ConnectionPolicy-object.
* Cosmosdb retourneert nu x-ms-throttle--het aantal nieuwe pogingen en x-ms-throttle-retry-wait-time-ms als de antwoordheaders in elke aanvraag om aan te geven van de vertraging opnieuw proberen het aantal en de cumulatieve tijd die de aanvraag heeft gewacht tussen de nieuwe pogingen.
* Het RetryPolicy-klasse en de bijbehorende eigenschap (retry_policy) weergegeven op de klasse document_client verwijderd en wordt geïntroduceerd in plaats daarvan een RetryOptions klasse blootstellen van de eigenschap RetryOptions op ConnectionPolicy-klasse die kan worden gebruikt voor de onderdrukking van enkele van de standaardopties voor opnieuw proberen.

### <a name="a-name180180"></a><a name="1.8.0"/>1.8.0
* De ondersteuning voor databaseaccounts voor meerdere regio's toegevoegd.

### <a name="a-name170170"></a><a name="1.7.0"/>1.7.0
* De ondersteuning toegevoegd voor tijd-Live(TTL)-functie voor documenten.

### <a name="a-name161161"></a><a name="1.6.1"/>1.6.1
* Oplossingen voor problemen die betrekking hebben op de server-side partitioneren om toe te staan van speciale tekens in het sleutelpad partitie.

### <a name="a-name160160"></a><a name="1.6.0"/>1.6.0
* Geïmplementeerd [gepartitioneerde verzamelingen](partition-data.md) en [niveau van de gebruiker gedefinieerde prestaties](performance-levels.md). 

### <a name="a-name150150"></a><a name="1.5.0"/>1.5.0
* Toevoegen van hash- & bereik partitie-resolvers om te helpen met sharding-toepassingen voor meerdere partities.

### <a name="a-name142142"></a><a name="1.4.2"/>1.4.2
* Upsert implementeren. Nieuwe UpsertXXX methoden toegevoegd ter ondersteuning van Upsert-functie.
* Implementatie-ID op basis van routering. Er zijn geen openbare API-wijzigingen, worden alle interne wijzigingen.

### <a name="a-name120120"></a><a name="1.2.0"/>1.2.0
* Ondersteunt georuimtelijke index.
* Hiermee valideert u de eigenschap id voor alle resources. Kunnen geen id's voor resources bevatten?, /, #, \, tekens bevatten of eindigen met een spatie.
* Nieuwe header 'index transformatie uitgevoerd' toevoegen aan ResourceResponse.

### <a name="a-name110110"></a><a name="1.1.0"/>1.1.0
* Indexeringsbeleid V2 implementeert.

### <a name="a-name101101"></a><a name="1.0.1"/>1.0.1
* Biedt ondersteuning voor proxy-verbinding.

### <a name="a-name100100"></a><a name="1.0.0"/>1.0.0
* SDK VOOR ALGEMENE BESCHIKBAARHEID.

## <a name="release--retirement-dates"></a>Datums release & buiten gebruik stellen
Microsoft biedt melding ten minste **12 maanden** voorafgaand aan buiten gebruik stellen van een SDK soepel te verwerken de overgang naar een nieuwere/ondersteunde versie.

Nieuwe functies en functionaliteit en -optimalisatie worden alleen toegevoegd aan de huidige SDK, daarom wordt aanbevolen dat u altijd een upgrade uitvoert naar de nieuwste SDK versie zo vroeg mogelijk. 

Een aanvraag voor het Cosmos DB met behulp van een buiten gebruik gestelde SDK worden geweigerd door de service.

> [!WARNING]
> Alle versies van de Azure SQL-SDK voor Python voorafgaand aan versie **1.0.0** zijn buiten gebruik gesteld op **en met 29 februari 2016**. 
> 
> 

<br/>

| Versie | Releasedatum | Vervaldatum |
| --- | --- | --- |
| [3.0.2](#3.0.2) |15 november 2018 |--- |
| [3.0.1](#3.0.1) |04 oktober 2018 |--- |
| [2.3.3](#2.3.3) |08 september 2018 |--- |
| [2.3.2](#2.3.2) |08 mei 2018 |--- |
| [2.3.1](#2.3.1) |21 december 2017 |--- |
| [2.3.0](#2.3.0) |10 november 2017 |--- |
| [2.2.1](#2.2.1) |Sep 29, 2017 |--- |
| [2.2.0](#2.2.0) |10 mei 2017 |--- |
| [2.1.0](#2.1.0) |Mei 01, 2017 |--- |
| [2.0.1](#2.0.1) |30 oktober 2016 |--- |
| [2.0.0](#2.0.0) |29 september 2016 |--- |
| [1.9.0](#1.9.0) |07 juli 2016 |--- |
| [1.8.0](#1.8.0) |14 juni 2016 |--- |
| [1.7.0](#1.7.0) |26 april 2016 |--- |
| [1.6.1](#1.6.1) |08 april 2016 |--- |
| [1.6.0](#1.6.0) |29 maart 2016 |--- |
| [1.5.0](#1.5.0) |03 januari 2016 |--- |
| [1.4.2](#1.4.2) |06 oktober 2015 |--- |
| [1.4.1](#1.4.1) |06 oktober 2015 |--- |
| [1.2.0](#1.2.0) |06 augustus 2015 |--- |
| [1.1.0](#1.1.0) |09 juli 2015 |--- |
| [1.0.1](#1.0.1) |25 mei 2015 |--- |
| [1.0.0](#1.0.0) |07 april 2015 |--- |
| 0.9.4-prelease |14 januari 2015 |En met 29 februari 2016 |
| 0.9.3-prelease |09 december 2014 |En met 29 februari 2016 |
| 0.9.2-prelease |25 november 2014 |En met 29 februari 2016 |
| 0.9.1-prelease |23 september 2014 |En met 29 februari 2016 |
| 0.9.0-prelease |21 augustus 2014 |En met 29 februari 2016 |

## <a name="faq"></a>Veelgestelde vragen
[!INCLUDE [cosmos-db-sdk-faq](../../includes/cosmos-db-sdk-faq.md)]

## <a name="see-also"></a>Zie ook
Zie voor meer informatie over Cosmos DB, [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) servicepagina. 

