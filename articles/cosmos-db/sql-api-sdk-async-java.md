---
title: 'Azure Cosmos DB: SQL Async Java API, SDK en resources | Microsoft Docs'
description: Meer informatie over de SQL Async Java API en de SDK, inclusief release datums, buiten gebruik stellen datums en wijzigingen die zijn aangebracht tussen elke versie van de Azure Cosmos DB SQL Async Java SDK.
services: cosmos-db
author: SnehaGunda
manager: kfile
ms.service: cosmos-db
ms.component: cosmosdb-sql
ms.devlang: java
ms.topic: reference
ms.date: 06/20/2018
ms.author: sngun
ms.openlocfilehash: e4a3b3a482f56065c54525a4d9cd7971f50f5b2a
ms.sourcegitcommit: ea5193f0729e85e2ddb11bb6d4516958510fd14c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/21/2018
ms.locfileid: "36300676"
---
# <a name="azure-cosmos-db-async-java-sdk-for-sql-api-release-notes-and-resources"></a>Azure Cosmos DB Async Java SDK voor SQL-API: releaseopmerkingen en resources
> [!div class="op_single_selector"]
> * [.NET](sql-api-sdk-dotnet.md)
> * [.NET-Feed van wijzigen](sql-api-sdk-dotnet-changefeed.md)
> * [.NET Core](sql-api-sdk-dotnet-core.md)
> * [Node.js](sql-api-sdk-node.md)
> * [Async Java](sql-api-sdk-async-java.md)
> * [Java](sql-api-sdk-java.md)
> * [Python](sql-api-sdk-python.md)
> * [REST](https://docs.microsoft.com/rest/api/cosmos-db/)
> * [REST-resourceprovider](https://docs.microsoft.com/rest/api/cosmos-db-resource-provider/)
> * [SQL](https://msdn.microsoft.com/library/azure/dn782250.aspx)
> * [BulkExecutor - .NET](sql-api-sdk-bulk-executor-dot-net.md)
> * [BulkExecutor - Java](sql-api-sdk-bulk-executor-java.md)

De SQL-API asynchrone Java SDK verschilt van de SQL-API Java SDK dankzij de asynchrone bewerkingen met ondersteuning van de [Netty bibliotheek](http://netty.io/). De vooraf bestaande [SQL API Java SDK](sql-api-sdk-java.md) biedt geen ondersteuning voor asynchrone bewerkingen. 

<table>

<tr><td>**SDK downloaden**</td><td>[Maven](https://mvnrepository.com/artifact/com.microsoft.azure/azure-cosmosdb)</td></tr>

<tr><td>**API-documentatie**</td><td>[Java-API-naslagdocumentatie](https://azure.github.io/azure-cosmosdb-java/)</td></tr>

<tr><td>**Bijdragen aan de SDK**</td><td>[GitHub](https://github.com/Azure/azure-cosmosdb-java)</td></tr>

<tr><td>**Aan de slag**</td><td>[Aan de slag met de asynchrone Java SDK](https://github.com/Azure-Samples/azure-cosmos-db-sql-api-async-java-getting-started)</td></tr>

<tr><td>**Voorbeeld van code**</td><td>[GitHub](https://github.com/Azure/azure-cosmosdb-java#usage-code-sample)</td></tr>

<tr><td>**Tips voor prestaties**</td><td>[Github Leesmij-bestand](https://github.com/Azure/azure-cosmosdb-java#guide-for-prod)</td></tr>

<tr><td>**Minimale ondersteunde runtime**</td><td>[JDK 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)</td></tr>
</table></br>

## <a name="release-notes"></a>Releaseopmerkingen

### <a name="a-name200200"></a><a name="2.0.0"/>2.0.0
* Org.json afhankelijkheid vervangen door jackson vanwege Prestatieoverwegingen en licentieverlening ([github #29](https://github.com/Azure/azure-cosmosdb-java/issues/29)).
* Afgeschafte OfferV2 klasse verwijderd.
* Toegevoegde accessor-methode aan klasse van de aanbieding voor doorvoer inhoud.
* Elke methode in een Document of systeembronnen org.json typen gewijzigd om te retourneren van een jackson objecttype retourneren.
* methode getObject(.) van klassen uitbreiden JsonSerializable gewijzigd om te retourneren van een ObjectNode jackson typt.
* de methode getCollection(.) gewijzigd om te retourneren van de verzameling van ObjectNode.
* Verwijderde JsonSerializable subklassen constructors met org.json.JSONObject arg.
* JsonSerializable.toJson (SerializationFormattingPolicy.Indented) gebruikt nu twee spaties voor inspringing.
  
### <a name="a-name102102"></a><a name="1.0.2"/>1.0.2
* Ondersteuning toegevoegd voor unieke Index beleid.
* Ondersteuning toegevoegd voor het beperken van de reactie voortzetting token grootte in de feed opties.
* Ondersteuning toegevoegd voor de partitie splitsen in Partitiequery Cross.
* Een bug in Json-serialisatie voor tijdstempel vaste ([github #32](https://github.com/Azure/azure-cosmosdb-java/issues/32)).
* Heeft een fout in de Json-serialisatie voor enum.
* Een fout bij het beheren van documenten met een grootte van 2MB vaste ([github #33](https://github.com/Azure/azure-cosmosdb-java/issues/33)).
* Afhankelijkheid com.fasterxml.jackson.core:jackson-databind bijgewerkt naar 2.9.5 vanwege een fout ([jackson databind: github #1599](https://github.com/FasterXML/jackson-databind/issues/1599))
* Afhankelijkheid van een upgrade uitgevoerd naar 0.8.0.17 vanwege een fout rxjava-extra's ([rxjava-extra's: github #30](https://github.com/davidmoten/rxjava-extras/issues/30)).
* De beschrijving van de metagegevens in pom bestand bijgewerkt om te worden inline met de rest van de documentatie.
* Syntaxis van de gebruikerservaring ([github #41](https://github.com/Azure/azure-cosmosdb-java/issues/41)), ([github #40](https://github.com/Azure/azure-cosmosdb-java/issues/40)).

### <a name="a-name101101"></a><a name="1.0.1"/>1.0.1
* Toegevoegde ondersteuning voor back-druk in de query.
* Ondersteuning toegevoegd voor de partitie-id sleutel bereik in de query.
* Oplossing voor het toestaan van grotere vervolgtoken in de aanvraagheader (bugfix github #24).
* netty afhankelijkheid bijgewerkt naar 4.1.22.Final om ervoor te zorgen JVM afgesloten nadat de hoofdthread is voltooid.
* Los om te voorkomen dat sessietoken doorgeven bij het lezen van de master-resources.
* Meer voorbeelden toegevoegd.
* Meer benchmarking scenario's toegevoegd.
* Header-bestanden van de vaste Java voor het genereren van de juiste java-document.

### <a name="a-name100100"></a><a name="1.0.0"/>1.0.0
* GA-SDK met end-to-end-ondersteuning voor niet-blokkerende i/o-gebruik de [Netty bibliotheek](http://netty.io/) in de modus van de gateway. 

## <a name="release-and-retirement-dates"></a>Release-en buiten gebruik stellen
Microsoft biedt melding ten minste **12 maanden** voordat het buiten gebruik stellen van een SDK om de overgang naar een nieuwere/ondersteunde versie vloeiend.

Nieuwe functies en functionaliteit en optimalisaties worden alleen toegevoegd aan de huidige SDK. Daarom wordt het aanbevolen dat u altijd een upgrade naar de nieuwste versie van de SDK zo spoedig mogelijk uitvoert.

Elk verzoek aan de Cosmos-database met behulp van een buiten gebruik gestelde SDK worden geweigerd door de service.

<br/>

| Versie | Releasedatum | Vervaldatum |
| --- | --- | --- |
| [2.0.0](#2.0.0) |20 juni 2018|--- |
| [1.0.2](#1.0.2) |18 mei 2018|--- |
| [1.0.1](#1.0.1) |20 april 2018|--- |
| [1.0.0](#1.0.0) |27 februari, 2018|--- |

## <a name="faq"></a>Veelgestelde vragen
[!INCLUDE [cosmos-db-sdk-faq](../../includes/cosmos-db-sdk-faq.md)]

## <a name="see-also"></a>Zie ook
Zie voor meer informatie over Cosmos DB, [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) pagina van de service.

