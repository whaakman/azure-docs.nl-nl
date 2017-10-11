---
title: 'Azure Cosmos DB: DocumentDB Java API, SDK en Resources | Microsoft Docs'
description: Meer informatie over de Java-API en de SDK, inclusief release datums, buiten gebruik stellen datums en wijzigingen die zijn aangebracht tussen elke versie van de Azure Cosmos DB DocumentDB Java SDK.
services: cosmos-db
documentationcenter: java
author: rnagpal
manager: jhubbard
editor: cgronlun
ms.assetid: 7861cadf-2a05-471a-9925-0fec0599351b
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: article
ms.date: 07/11/2017
ms.author: khdang
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 15e3f7ef3bfd6b1f61fe6081a378bdb29e0a1aa2
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2017
---
# <a name="azure-cosmos-db-documentdb-java-sdk-release-notes-and-resources"></a>Azure Cosmos DB: DocumentDB Java SDK-releaseopmerkingen en resources
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

<tr><td>**SDK downloaden**</td><td>[Maven](http://search.maven.org/#search%7Cgav%7C1%7Cg%3A%22com.microsoft.azure%22%20AND%20a%3A%22azure-documentdb%22)</td></tr>

<tr><td>**API-documentatie**</td><td>[Java-API-naslagdocumentatie](/java/api/com.microsoft.azure.documentdb)</td></tr>

<tr><td>**Bijdragen aan de SDK**</td><td>[GitHub](https://github.com/Azure/azure-documentdb-java/)</td></tr>

<tr><td>**Aan de slag**</td><td>[Aan de slag met de SDK voor Java](documentdb-java-get-started.md)</td></tr>

<tr><td>**Zelfstudie voor web-app**</td><td>[Ontwikkeling van webtoepassing met Azure Cosmos-DB](documentdb-java-application.md)</td></tr>

<tr><td>**Huidige ondersteunde runtime**</td><td>[JDK 7](http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html)</td></tr>
</table></br>

## <a name="release-notes"></a>Releaseopmerkingen

### <a name="a-name11201120"></a><a name="1.12.0"/>1.12.0
* Kritieke problemen opgelost voor aanvraagverwerking tijdens splitsingen partitie.
* Een probleem met de sterke en BoundedStaleness consistentieniveaus opgelost.

### <a name="a-name11101110"></a><a name="1.11.0"/>1.11.0
* Ondersteuning toegevoegd voor een nieuwe consistentieniveau ConsistentPrefix genoemd.
* Een fout bij het lezen van de verzameling in sessiemodus vast.

### <a name="a-name11001100"></a><a name="1.10.0"/>1.10.0
* Ingeschakelde ondersteuning voor gepartitioneerde verzameling met als geringe als 2500 RU per seconde en schalen in intervallen van 100 RU per seconde.
* Een fout in de native assembly waardoor NullRef uitzondering in sommige query's opgelost.

### <a name="a-name196196"></a><a name="1.9.6"/>1.9.6
* De configuratie van de query-engine dat leiden uitzonderingen voor query's in de modus van de Gateway tot kan, een bug vast.
* Enkele fouten in de sessie-container die ertoe leiden een 'Eigenaar bron is niet gevonden'-uitzondering voor aanvragen onmiddellijk na het maken van een siteverzameling dat kan opgelost.

### <a name="a-name195195"></a><a name="1.9.5"/>1.9.5
* Ondersteuning toegevoegd voor aggregatie van query's (COUNT, MIN, MAX, SUM en Gem). Zie [aggregatie ondersteuning](documentdb-sql-query.md#Aggregates).
* Ondersteuning toegevoegd voor de feed wijzigen.
* Ondersteuning toegevoegd voor de verzameling quotum informatie via RequestOptions.setPopulateQuotaInfo.
* Ondersteuning toegevoegd voor de opgeslagen procedure script logboekregistratie via RequestOptions.setScriptLoggingEnabled.
* Een bug vast waar query in de modus DirectHttps loopt vast wanneer zich voordoen tijdens versnelling fouten.
* Heeft een fout in de sessiemodus consistentie.
* Vaste een bug wat leiden NullReferenceException in HttpContext tot kan wanneer aanvraagsnelheid hoog is.
* Verbeterde prestaties van DirectHttps-modus.

### <a name="a-name194194"></a><a name="1.9.4"/>1.9.4
* Toegevoegde eenvoudige exemplaar, op basis van proxy clientondersteuning met ConnectionPolicy.setProxy()-API.
* Toegevoegde DocumentClient.close() API met correct afsluiten DocumentClient exemplaar.
* Verbeterde prestaties van query's in de modus voor directe verbinding met het queryplan die zijn afgeleid van de systeemeigen assembly in plaats van de Gateway.
* Stel FAIL_ON_UNKNOWN_PROPERTIES = false zodat gebruikers hoeven niet te definiëren JsonIgnoreProperties in hun POJO.
* Logboekregistratie geherstructureerd SLF4J gebruiken.
* Enkele andere fouten opgelost in de lezer van de consistentie.

### <a name="a-name193193"></a><a name="1.9.3"/>1.9.3
* Vaste een bug in het Verbindingsbeheer om te voorkomen dat connection lekken in directe verbinding-modus.
* Vaste een fout in de bovenste query waar deze NullReferenece uitzondering kan genereren.
* Verbeterde prestaties door het aantal netwerkaanroep voor het interne te verminderen.
* Toegevoegde statuscode ActivityID en aanvraag-URI in DocumentClientException als hulpmiddel bij probleemoplossing.

### <a name="a-name192192"></a><a name="1.9.2"/>1.9.2
* Een probleem opgelost in het Verbindingsbeheer van de voor de stabiliteit.

### <a name="a-name191191"></a><a name="1.9.1"/>1.9.1
* Ondersteuning toegevoegd voor BoundedStaleness consistentieniveau.
* Ondersteuning toegevoegd voor directe verbinding voor CRUD-bewerkingen voor gepartitioneerde verzamelingen.
* Heeft een fout in de query voor de database met SQL.
* Vaste een fout in de sessiecache waar sessietoken misschien onjuist ingesteld.

### <a name="a-name190190"></a><a name="1.9.0"/>1.9.0
* Ondersteuning toegevoegd voor cross-partitie parallelle query's.
* Ondersteuning toegevoegd voor TOP/ORDER BY-query's voor gepartitioneerde verzamelingen.
* Toegevoegde ondersteuning voor sterke consistentie.
* Ondersteuning toegevoegd voor aanvragen van een naam op basis van wanneer u directe verbinding.
* Vaste aanbrengen ActivityId blijven consistent op alle nieuwe aanvraagpogingen.
* Vaste een bug die betrekking hebben op de sessiecache bij het opnieuw genereren van een verzameling met dezelfde naam.
* Toegevoegde veelhoek en LineString DataTypes tijdens het opgeven van de verzameling beleid voor geofencing ruimtelijke query's te indexeren.
* Opgeloste problemen met Java Doc voor Java 1.8.

### <a name="a-name181181"></a><a name="1.8.1"/>1.8.1
* Heeft een fout in de PartitionKeyDefinitionMap naar verzamelingen met één partitie in de cache en extra ophalen die sleutels aanvragen partitie te maken.
* Een fout bij het niet opnieuw uitgevoerd als een waarde voor de onjuiste partitiesleutel beschikbaar is opgelost.

### <a name="a-name180180"></a><a name="1.8.0"/>1.8.0
* De ondersteuning voor meerdere landen/regio database accounts toegevoegd.
* Ondersteuning toegevoegd voor automatische nieuwe pogingen voor beperkte verzoeken met opties voor het maximale aantal pogingen en de maximale wachttijd aanpassen.  Zie RetryOptions en ConnectionPolicy.getRetryOptions().
* Afgeschafte IPartitionResolver op basis van aangepaste partitionering code. Gebruik gepartitioneerde verzamelingen voor hogere opslag en doorvoer.

### <a name="a-name171171"></a><a name="1.7.1"/>1.7.1
* Toegevoegde opnieuw Beleidsondersteuning voor beperking.  

### <a name="a-name170170"></a><a name="1.7.0"/>1.7.0
* Toegevoegde tijd live (TTL) ondersteuning voor documenten.

### <a name="a-name160160"></a><a name="1.6.0"/>1.6.0
* Geïmplementeerd [gepartitioneerde verzamelingen](partition-data.md) en [prestatieniveaus die door de gebruiker gedefinieerde](performance-levels.md).

### <a name="a-name151151"></a><a name="1.5.1"/>1.5.1
* Heeft een fout in de HashPartitionResolver voor het genereren van hash-waarden in little endian consistent met andere SDK's.

### <a name="a-name150150"></a><a name="1.5.0"/>1.5.0
* Toevoegen van hash- & bereik partitie-resolvers om te helpen bij sharding-toepassingen op verschillende partities.

### <a name="a-name140140"></a><a name="1.4.0"/>1.4.0
* Upsert implementeren. Nieuwe upsertXXX methoden toegevoegd ter ondersteuning van Upsert-functie.
* ID gebaseerde routering wordt geïmplementeerd. Er zijn geen openbare API-wijzigingen, worden alle wijzigingen interne.

### <a name="a-name130130"></a><a name="1.3.0"/>1.3.0
* Release overgeslagen op te zetten versienummer in uitlijning met andere SDK

### <a name="a-name120120"></a><a name="1.2.0"/>1.2.0
* Ondersteunt georuimtelijke Index
* De eigenschap id voor alle resources valideert. Kunnen geen id's voor bronnen bevatten?, /, #, \, tekens of eindigen met een spatie.
* Voegt nieuwe header 'index transformatie uitgevoerd' aan ResourceResponse.

### <a name="a-name110110"></a><a name="1.1.0"/>1.1.0
* V2-indexeringsbeleid implementeert

### <a name="a-name100100"></a><a name="1.0.0"/>1.0.0
* GA SDK

## <a name="release--retirement-dates"></a>Release & buiten gebruik stellen datums
Microsoft biedt melding ten minste **12 maanden** voordat het buiten gebruik stellen van een SDK om de overgang naar een nieuwere/ondersteunde versie vloeiend.

Nieuwe functies en functionaliteit en optimalisaties alleen zijn toegevoegd aan de huidige SDK, als zodanig wordt aanbevolen dat u altijd een upgrade uitvoert naar de nieuwste SDK versie zo snel mogelijk.

Elk verzoek aan de Cosmos-database met behulp van een buiten gebruik gestelde SDK worden geweigerd door de service.

> [!WARNING]
> Alle versies van de DocumentDB SDK voor Java voorafgaand aan versie **1.0.0** wordt buiten gebruik worden gesteld op **29 februari 2016**.
> 
> 

<br/>

| Versie | Releasedatum | Vervaldatum |
| --- | --- | --- |
| [1.12.0](#1.12.0) |11 juli 2017 |--- |
| [1.11.0](#1.11.0) |10 mei 2017 |--- |
| [1.10.0](#1.10.0) |11 maart 2017 |--- |
| [1.9.6](#1.9.6) |21 februari 2017 |--- |
| [1.9.5](#1.9.5) |31 januari 2017 |--- |
| [1.9.4](#1.9.4) |24 november 2016 |--- |
| [1.9.3](#1.9.3) |30 oktober 2016 |--- |
| [1.9.2](#1.9.2) |28 oktober 2016 |--- |
| [1.9.1](#1.9.1) |26 oktober 2016 |--- |
| [1.9.0](#1.9.0) |03 oktober 2016 |--- |
| [1.8.1](#1.8.1) |30 juni 2016 |--- |
| [1.8.0](#1.8.0) |14 juni 2016 |--- |
| [1.7.1](#1.7.1) |30 april 2016 |--- |
| [1.7.0](#1.7.0) |27 april 2016 |--- |
| [1.6.0](#1.6.0) |29 maart 2016 |--- |
| [1.5.1](#1.5.1) |31 december 2015 |--- |
| [1.5.0](#1.5.0) |04 december 2015 |--- |
| [1.4.0](#1.4.0) |05 oktober 2015 |--- |
| [1.3.0](#1.3.0) |05 oktober 2015 |--- |
| [1.2.0](#1.2.0) |05 augustus 2015 |--- |
| [1.1.0](#1.1.0) |09 juli 2015 |--- |
| [1.0.1](#1.0.1) |12 mei 2015 |--- |
| [1.0.0](#1.0.0) |07 april 2015 |--- |
| 0.9.5-prelease |09 mrt 2015 |29 februari 2016 |
| 0.9.4-prelease |17 februari 2015 |29 februari 2016 |
| 0.9.3-prelease |13 januari 2015 |29 februari 2016 |
| 0.9.2-prelease |19 december 2014 |29 februari 2016 |
| 0.9.1-prelease |19 december 2014 |29 februari 2016 |
| 0.9.0-prelease |10 december 2014 |29 februari 2016 |

## <a name="faq"></a>Veelgestelde vragen
[!INCLUDE [cosmos-db-sdk-faq](../../includes/cosmos-db-sdk-faq.md)]

## <a name="see-also"></a>Zie ook
Zie voor meer informatie over Cosmos DB, [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) pagina van de service.

