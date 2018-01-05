---
title: 'Azure Cosmos DB: SQL Python-API, SDK en resources | Microsoft Docs'
description: Meer informatie over de SQL-API voor Python en SDK, inclusief release datums, buiten gebruik stellen datums en wijzigingen die zijn aangebracht tussen elke versie van de Azure-SDK voor Python Cosmos DB.
services: cosmos-db
documentationcenter: python
author: rnagpal
manager: jhubbard
editor: cgronlun
ms.assetid: 3ac344a9-b2fa-4a3f-a4cc-02d287e05469
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 1/4/2018
ms.author: rnagpal
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 6801c5b62be08e4dcb32ad342b15e9ad3f3e20a8
ms.sourcegitcommit: 3cdc82a5561abe564c318bd12986df63fc980a5a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/05/2018
---
# <a name="azure-cosmos-db-python-sdk-for-sql-api-release-notes-and-resources"></a>Azure Cosmos DB Python SDK voor SQL-API: releaseopmerkingen en resources
> [!div class="op_single_selector"]
> * [.NET](sql-api-sdk-dotnet.md)
> * [.NET-Feed van wijzigen](sql-api-sdk-dotnet-changefeed.md)
> * [.NET Core](sql-api-sdk-dotnet-core.md)
> * [Node.js](sql-api-sdk-node.md)
> * [Java](sql-api-sdk-java.md)
> * [Python](sql-api-sdk-python.md)
> * [REST](https://docs.microsoft.com/rest/api/documentdb/)
> * [REST-resourceprovider](https://docs.microsoft.com/rest/api/documentdbresourceprovider/)
> * [SQL](https://msdn.microsoft.com/library/azure/dn782250.aspx)
> 
> 

[!INCLUDE [cosmos-db-sql-api](../../includes/cosmos-db-sql-api.md)]

<table>

<tr><td>**SDK downloaden**</td><td>[PyPI](https://pypi.python.org/pypi/pydocumentdb)</td></tr>

<tr><td>**API-documentatie**</td><td>[Python-API-naslagdocumentatie](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.html)</td></tr>

<tr><td>**SDK-installatie-instructies**</td><td>[Python SDK installatie-instructies](http://azure.github.io/azure-documentdb-python/)</td></tr>

<tr><td>**Bijdragen aan de SDK**</td><td>[GitHub](https://github.com/Azure/azure-documentdb-python)</td></tr>

<tr><td>**Aan de slag**</td><td>[Aan de slag met de SDK voor Python](sql-api-python-application.md)</td></tr>

<tr><td>**Huidige ondersteund platform**</td><td>[Python 2.7](https://www.python.org/downloads/) en [Python 3.5](https://www.python.org/downloads/)</td></tr>
</table></br>

## <a name="release-notes"></a>Releaseopmerkingen
### <a name="a-name231231"></a><a name="2.3.1"/>2.3.1
* Bijgewerkte documentatie om te verwijzen naar Azure Cosmos DB in plaats van Azure DocumentDB.

### <a name="a-name230230"></a><a name="2.3.0"/>2.3.0
* Deze SDK-versie moet de meest recente versie van Azure Cosmos DB Emulator die beschikbaar is voor downloaden van https://aka.ms/cosmosdb-emulator.

### <a name="a-name221221"></a><a name="2.2.1"/>2.2.1
* Fout te herstellen voor cumulatieve woordenlijst.
* Fout te herstellen voor het bijsnijden slashes in de resourcekoppeling.
* Toegevoegde tests voor Unicode-codering.

### <a name="a-name220220"></a><a name="2.2.0"/>2.2.0
* Ondersteuning toegevoegd voor een nieuwe consistentieniveau ConsistentPrefix genoemd.


### <a name="a-name210210"></a><a name="2.1.0"/>2.1.0
* Ondersteuning toegevoegd voor aggregatie van query's (COUNT, MIN, MAX, SUM en Gem).
* Een optie voor het uitschakelen van SSL-verificatie bij het uitvoeren in Cosmos DB Emulator toegevoegd.
* De beperking van de afhankelijke aanvragen module exact 2.10.0 worden verwijderd.
* Minimaal doorvoer voor gepartitioneerde verzamelingen uit 10,100 RU/s tot 2500 RU/s verlaagd.
* Ondersteuning toegevoegd voor het inschakelen van logboekregistratie script tijdens het uitvoeren van de opgeslagen procedure.
* REST-API-versie tegenaan 2017 '-01-19' in deze versie.

### <a name="a-name201201"></a><a name="2.0.1"/>2.0.1
* Redactionele wijzigingen aangebracht in documentopmerkingen.

### <a name="a-name200200"></a><a name="2.0.0"/>2.0.0
* Ondersteuning toegevoegd voor Python 3.5.
* Ondersteuning toegevoegd voor verbindingsgroepering met een module aanvragen.
* Ondersteuning toegevoegd voor sessieconsistentie.
* Ondersteuning toegevoegd voor TOP/ORDERBY-query's voor gepartitioneerde verzamelingen.

### <a name="a-name190190"></a><a name="1.9.0"/>1.9.0
* Toegevoegde opnieuw Beleidsondersteuning voor beperkte aanvragen. (Beperkte aanvragen ontvangen van een aanvraag snelheid te groot uitzondering, foutcode 429.) Standaard Azure Cosmos DB pogingen negen keer voor elke aanvraag wanneer foutcode 429 is opgetreden, de tijd retryAfter in de antwoordheader naleven. Een vaste opnieuw tijdsinterval kan nu worden ingesteld als onderdeel van de eigenschap RetryOptions op het object ConnectionPolicy als u wilt de retryAfter tijd geretourneerd door server tussen de pogingen negeren. Azure Cosmos-DB wordt nu gewacht op maximaal 30 seconden voor elke aanvraag die wordt beperkt (ongeacht het aantal pogingen) en het antwoord met foutcode 429 retourneert. Deze tijd kan ook worden overschreven in de eigenschap RetryOptions in ConnectionPolicy-object.
* Cosmos DB retourneert nu x-ms-versnelling-aantal nieuwe pogingen en x-ms-throttle-retry-wait-time-ms de antwoordheaders in elke aanvraag om aan te geven van de beperking probeer aantal en de cumulatieve tijd dat de aanvraag worden gewacht tussen de nieuwe pogingen.
* Het RetryPolicy-klasse en de bijbehorende eigenschap (retry_policy) weergegeven op de klasse document_client verwijderd en wordt geïntroduceerd in plaats daarvan een RetryOptions klasse blootstellen van de eigenschap RetryOptions voor ConnectionPolicy-klasse die kan worden gebruikt voor het onderdrukken van enkele van de standaardopties voor opnieuw proberen.

### <a name="a-name180180"></a><a name="1.8.0"/>1.8.0
* De ondersteuning voor meerdere landen/regio database accounts toegevoegd.

### <a name="a-name170170"></a><a name="1.7.0"/>1.7.0
* De ondersteuning voor de functie tijd-Live(TTL) voor documenten toegevoegd.

### <a name="a-name161161"></a><a name="1.6.1"/>1.6.1
* Oplossingen voor problemen met betrekking tot serverzijde partitioneren zodat speciale tekens in sleutelpad partitie.

### <a name="a-name160160"></a><a name="1.6.0"/>1.6.0
* Geïmplementeerd [gepartitioneerde verzamelingen](partition-data.md) en [prestatieniveaus die door de gebruiker gedefinieerde](performance-levels.md). 

### <a name="a-name150150"></a><a name="1.5.0"/>1.5.0
* Toevoegen van hash- & bereik partitie-resolvers om te helpen bij sharding-toepassingen op verschillende partities.

### <a name="a-name142142"></a><a name="1.4.2"/>1.4.2
* Upsert implementeren. Nieuwe UpsertXXX methoden toegevoegd ter ondersteuning van Upsert-functie.
* ID gebaseerde routering wordt geïmplementeerd. Er zijn geen openbare API-wijzigingen, worden alle wijzigingen interne.

### <a name="a-name120120"></a><a name="1.2.0"/>1.2.0
* Ondersteunt georuimtelijke index.
* De eigenschap id voor alle resources valideert. Kunnen geen id's voor bronnen bevatten?, /, #, \, tekens of eindigen met een spatie.
* Voegt nieuwe header 'index transformatie uitgevoerd' aan ResourceResponse.

### <a name="a-name110110"></a><a name="1.1.0"/>1.1.0
* V2-indexeringsbeleid implementeert.

### <a name="a-name101101"></a><a name="1.0.1"/>1.0.1
* Ondersteunt proxyverbinding.

### <a name="a-name100100"></a><a name="1.0.0"/>1.0.0
* GA SDK.

## <a name="release--retirement-dates"></a>Release & buiten gebruik stellen datums
Microsoft biedt melding ten minste **12 maanden** voordat het buiten gebruik stellen van een SDK om de overgang naar een nieuwere/ondersteunde versie vloeiend.

Nieuwe functies en functionaliteit en optimalisaties alleen zijn toegevoegd aan de huidige SDK, als zodanig wordt aanbevolen dat u altijd een upgrade uitvoert naar de nieuwste SDK versie zo snel mogelijk. 

Elk verzoek aan de Cosmos-database met behulp van een buiten gebruik gestelde SDK worden geweigerd door de service.

> [!WARNING]
> Alle versies van de SQL Azure SDK voor Python voorafgaand aan versie **1.0.0** buiten gebruik zijn gesteld op **29 februari 2016**. 
> 
> 

<br/>

| Versie | Releasedatum | Vervaldatum |
| --- | --- | --- |
| [2.3.1](#2.3.1) |21 december 2017 |--- |
| [2.3.0](#2.3.0) |10 november 2017 |--- |
| [2.2.1](#2.2.1) |Sep 29, 2017 |--- |
| [2.2.0](#2.2.0) |10 mei 2017 |--- |
| [2.1.0](#2.1.0) |01 mei 2017 |--- |
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
| 0.9.4-prelease |14 januari 2015 |29 februari 2016 |
| 0.9.3-prelease |09 december 2014 |29 februari 2016 |
| 0.9.2-prelease |25 november 2014 |29 februari 2016 |
| 0.9.1-prelease |23 september 2014 |29 februari 2016 |
| 0.9.0-prelease |21 augustus 2014 |29 februari 2016 |

## <a name="faq"></a>Veelgestelde vragen
[!INCLUDE [cosmos-db-sdk-faq](../../includes/cosmos-db-sdk-faq.md)]

## <a name="see-also"></a>Zie ook
Zie voor meer informatie over Cosmos DB, [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) pagina van de service. 

