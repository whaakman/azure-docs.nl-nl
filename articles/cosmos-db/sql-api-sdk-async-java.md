---
title: 'Azure Cosmos DB: SQL Async Java-API, SDK en resources | Microsoft Docs'
description: Meer informatie over de SQL Async Java-API en SDK, inclusief release datums, buiten gebruik stellen datums en wijzigingen die zijn aangebracht tussen elke versie van de Azure Cosmos DB SQL Async Java SDK.
services: cosmos-db
author: SnehaGunda
manager: kfile
ms.service: cosmos-db
ms.component: cosmosdb-sql
ms.devlang: java
ms.topic: reference
ms.date: 11/02/2018
ms.author: moderakh
ms.openlocfilehash: c8b8397e5d51b67895a6a95ed6d1611813a9aaf6
ms.sourcegitcommit: d372d75558fc7be78b1a4b42b4245f40f213018c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/09/2018
ms.locfileid: "51300843"
---
# <a name="azure-cosmos-db-async-java-sdk-for-sql-api-release-notes-and-resources"></a>Azure Cosmos DB Async Java SDK voor SQL-API: releaseopmerkingen en resources
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
> * [SQL](https://msdn.microsoft.com/library/azure/dn782250.aspx)
> * [BulkExecutor - .NET](sql-api-sdk-bulk-executor-dot-net.md)
> * [BulkExecutor - Java](sql-api-sdk-bulk-executor-java.md)

De SQL-SDK voor Java-API asynchrone wijkt af van de Java-SDK van de SQL-API door te geven van asynchrone bewerkingen met ondersteuning van de [Netty bibliotheek](http://netty.io/). De vooraf bestaande [SQL API Java SDK](sql-api-sdk-java.md) biedt geen ondersteuning voor asynchrone bewerkingen. 

<table>

<tr><td>**SDK downloaden**</td><td>[Maven](https://mvnrepository.com/artifact/com.microsoft.azure/azure-cosmosdb)</td></tr>

<tr><td>**API-documentatie**</td><td>[Java API-referentiedocumentatie](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb.rx._async_document_client?view=azure-java-stable)</td></tr>

<tr><td>**Bijdragen aan de SDK**</td><td>[GitHub](https://github.com/Azure/azure-cosmosdb-java)</td></tr>

<tr><td>**Aan de slag**</td><td>[Aan de slag met de Async Java-SDK](https://github.com/Azure-Samples/azure-cosmos-db-sql-api-async-java-getting-started)</td></tr>

<tr><td>**Voorbeeld van code**</td><td>[GitHub](https://github.com/Azure/azure-cosmosdb-java#usage-code-sample)</td></tr>

<tr><td>**Tips voor prestaties**</td><td>[Github Leesmij-bestand](https://github.com/Azure/azure-cosmosdb-java#guide-for-prod)</td></tr>

<tr><td>**Minimaal ondersteunde runtime**</td><td>[JDK 8](https://aka.ms/azure-jdks)</td></tr>
</table></br>

## <a name="release-notes"></a>Releaseopmerkingen

### <a name="a-name222222"></a><a name="2.2.2"/>2.2.2
* Een bug pakketten verholpen.

### <a name="a-name221221"></a><a name="2.2.1"/>2.2.1
* Een NFE opgelost in het pad voor schrijven probeer het opnieuw.
* Eindpunt management, een bug NFE vast.
* Kwetsbaar afhankelijkheden bijgewerkt ([github #68](https://github.com/Azure/azure-cosmosdb-java/issues/68)).
* Ondersteuning toegevoegd voor Netty netwerk logboekregistratie voor het oplossen van problemen.

### <a name="a-name220220"></a><a name="2.2.0"/>2.2.0
* Er is ondersteuning toegevoegd voor meerdere regio's schrijven.

### <a name="a-name210210"></a><a name="2.1.0"/>2.1.0
* Er is ondersteuning toegevoegd voor de Proxy.
* Ondersteuning toegevoegd voor bronautorisatie token.
* Een probleem opgelost in grote partitiesleutels verwerken ([github #63](https://github.com/Azure/azure-cosmosdb-java/issues/63)).
* Documentatie is verbeterd.
* De SDK is geherstructureerd in gedetailleerdere modules.

### <a name="a-name201201"></a><a name="2.0.1"/>2.0.1
* Een probleem opgelost voor niet-Engelse landinstellingen ([github #51](https://github.com/Azure/azure-cosmosdb-java/issues/51)).
* Toegevoegde helpermethoden in Conflict Resource.

### <a name="a-name200200"></a><a name="2.0.0"/>2.0.0
* Org.json afhankelijkheid vervangen door jackson vanwege prestatieredenen en licentieverlening ([github #29](https://github.com/Azure/azure-cosmosdb-java/issues/29)).
* Afgeschafte OfferV2 klasse is verwijderd.
* Toegevoegde accessor-methode voor het aanbod klasse voor de inhoud van de doorvoer.
* Een methode in Document/Resource org.json typen gewijzigd om terug te keren een jackson objecttype retourneren.
* methode getObject(.) van klassen uitbreiden JsonSerializable gewijzigd om terug te keren een ObjectNode jackson typt.
* getCollection(.) methode om te retourneren van de verzameling van ObjectNode gewijzigd.
* Verwijderde JsonSerializable subklassen constructors met org.json.JSONObject arg.
* JsonSerializable.toJson (SerializationFormattingPolicy.Indented) maakt nu gebruik van twee spaties voor inspringen.
  
### <a name="a-name102102"></a><a name="1.0.2"/>1.0.2
* Er is ondersteuning toegevoegd voor de unieke Index beleid.
* Ondersteuning toegevoegd voor het beperken van de reactie voortzetting van token grootte in de opties voor invoer.
* Ondersteuning toegevoegd voor partitie splitsen in meerdere Partitiequery.
* Een probleem opgelost in Json-serialisatie voor tijdstempel ([github #32](https://github.com/Azure/azure-cosmosdb-java/issues/32)).
* Een bug opgelost in Json-serialisatie voor enum.
* Een probleem opgelost bij het beheren van documenten van de grootte van 2MB ([github #33](https://github.com/Azure/azure-cosmosdb-java/issues/33)).
* Afhankelijkheid com.fasterxml.jackson.core:jackson-databind bijgewerkt naar 2.9.5 vanwege een fout ([jackson databind: github #1599](https://github.com/FasterXML/jackson-databind/issues/1599))
* Afhankelijkheid van een upgrade uitgevoerd naar 0.8.0.17 vanwege een bug rxjava-extra's ([rxjava extra: github #30](https://github.com/davidmoten/rxjava-extras/issues/30)).
* De beschrijving van de metagegevens in het pom-bestand bijgewerkt in verband met inline met de rest van documentatie zijn.
* Syntaxis van de gebruikerservaring ([github #41](https://github.com/Azure/azure-cosmosdb-java/issues/41)), ([github #40](https://github.com/Azure/azure-cosmosdb-java/issues/40)).

### <a name="a-name101101"></a><a name="1.0.1"/>1.0.1
* Toegevoegde ondersteuning voor back-druk in de query.
* Ondersteuning toegevoegd voor partitie-id sleutelbereik in de query.
* Oplossing voor het toestaan van grotere vervolgtoken in de aanvraagheader (bugfix github #24).
* Een upgrade uitgevoerd naar 4.1.22.Final om ervoor te zorgen JVM-netty afhankelijkheid afgesloten nadat de hoofdthread is voltooid.
* Om te voorkomen dat sessietoken wordt doorgegeven bij het lezen van de master-resources oplossen.
* Meer voorbeelden toegevoegd.
* Meer scenario's voor benchmarking toegevoegd.
* Header-bestanden van de vaste Java voor het genereren van de juiste java doc-bestand.

### <a name="a-name100100"></a><a name="1.0.0"/>1.0.0
* GA-SDK met end-to-end-ondersteuning voor het gebruik van i/o-niet-blokkerende de [Netty bibliotheek](http://netty.io/) in de modus van de gateway. 

## <a name="release-and-retirement-dates"></a>Release-en buiten gebruik stellen
Microsoft biedt melding ten minste **12 maanden** voorafgaand aan buiten gebruik stellen van een SDK soepel te verwerken de overgang naar een nieuwere/ondersteunde versie.

Nieuwe functies en functionaliteit en -optimalisatie worden alleen toegevoegd aan de huidige SDK. Daarom wordt het aanbevolen dat u altijd een upgrade naar de nieuwste versie van de SDK zo vroeg mogelijk uitvoert.

Een aanvraag voor het Cosmos DB met behulp van een buiten gebruik gestelde SDK worden geweigerd door de service.

<br/>

| Versie | Releasedatum | Vervaldatum |
| --- | --- | --- |
| [2.2.2](#2.2.2) |8 november 2018|--- |
| [2.2.1](#2.2.1) |2 november 2018|--- |
| [2.2.0](#2.2.0) |22 september 2018|--- |
| [2.1.0](#2.1.0) |5 september 2018|--- |
| [2.0.1](#2.0.1) |16 augustus 2018|--- |
| [2.0.0](#2.0.0) |20 juni 2018|--- |
| [1.0.2](#1.0.2) |18 mei 2018|--- |
| [1.0.1](#1.0.1) |20 april 2018|--- |
| [1.0.0](#1.0.0) |27 februari, 2018|--- |

## <a name="faq"></a>Veelgestelde vragen
[!INCLUDE [cosmos-db-sdk-faq](../../includes/cosmos-db-sdk-faq.md)]

## <a name="see-also"></a>Zie ook
Zie voor meer informatie over Cosmos DB, [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) servicepagina.

