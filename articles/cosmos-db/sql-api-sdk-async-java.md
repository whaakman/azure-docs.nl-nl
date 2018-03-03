---
title: 'Azure Cosmos DB: SQL Async Java API, SDK en resources | Microsoft Docs'
description: Meer informatie over de SQL Async Java API en de SDK, inclusief release datums, buiten gebruik stellen datums en wijzigingen die zijn aangebracht tussen elke versie van de Azure Cosmos DB SQL Async Java SDK.
services: cosmos-db
documentationcenter: java
author: mimig1
manager: jhubbard
editor: cgronlun
ms.assetid: a452ffa2-c15d-4b0a-a8c1-ec9b750ce52b
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: article
ms.date: 02/28/2018
ms.author: mimig
ms.openlocfilehash: 408080fd7d9788d13b13e2769e59946a81e6742c
ms.sourcegitcommit: 782d5955e1bec50a17d9366a8e2bf583559dca9e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/02/2018
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
> * [REST](https://docs.microsoft.com/rest/api/documentdb/)
> * [REST-resourceprovider](https://docs.microsoft.com/rest/api/documentdbresourceprovider/)
> * [SQL](https://msdn.microsoft.com/library/azure/dn782250.aspx)
> 
> 

[!INCLUDE [cosmos-db-sql-api](../../includes/cosmos-db-sql-api.md)]

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

### <a name="a-name100100"></a><a name="1.0.0"/>1.0.0
* GA-SDK met end-to-end-ondersteuning voor niet-blokkerende i/o-gebruik de [Netty bibliotheek](http://netty.io/) in de modus van de gateway. 

## <a name="release-and-retirement-dates"></a>Release-en buiten gebruik stellen
Microsoft biedt melding ten minste **12 maanden** voordat het buiten gebruik stellen van een SDK om de overgang naar een nieuwere/ondersteunde versie vloeiend.

Nieuwe functies en functionaliteit en optimalisaties alleen zijn toegevoegd aan de huidige SDK, als zodanig wordt aanbevolen dat u altijd een upgrade uitvoert naar de nieuwste SDK versie zo snel mogelijk.

Elk verzoek aan de Cosmos-database met behulp van een buiten gebruik gestelde SDK worden geweigerd door de service.

<br/>

| Versie | Releasedatum | Vervaldatum |
| --- | --- | --- |
| [1.0.0](#1.0.0) |27 februari 2018|--- |

## <a name="faq"></a>Veelgestelde vragen
[!INCLUDE [cosmos-db-sdk-faq](../../includes/cosmos-db-sdk-faq.md)]

## <a name="see-also"></a>Zie ook
Zie voor meer informatie over Cosmos DB, [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) pagina van de service.

