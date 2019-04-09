---
title: Databasetransacties en optimistische gelijktijdigheid beheren in Azure Cosmos DB
description: Dit artikel wordt beschreven databasetransacties en optimistische gelijktijdigheid beheren in Azure Cosmos DB
author: rimman
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 04/08/2019
ms.author: rimman
ms.reviewer: sngun
ms.openlocfilehash: 568f47aacf39793d4c2da46798682abc002ca33b
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/08/2019
ms.locfileid: "59279499"
---
# <a name="transactions-and-optimistic-concurrency-control"></a>Transacties en optimistisch beheer van gelijktijdigheid

Databasetransacties bieden een veilige en voorspelbare programmeermodel gelijktijdige wijzigingen in de gegevens zijn getroffen. Traditionele relationele databases, zoals SQL Server, kunt u het schrijven van de bedrijfslogica met behulp van opgeslagen procedures en/of triggers, verzenden naar de server voor de uitvoering van rechtstreeks in de database-engine. Met traditionele relationele databases, bent u vereist voor het omgaan met twee verschillende programmeertalen de (niet-transactionele) toepassing programmeertaal, zoals JavaScript, Python, C#, Java, enz. en de transactionele programmeer-taal ( zoals T-SQL) is die systeemeigen uitgevoerd door de database.

De database-engine in Azure Cosmos DB ondersteunt volledige ACID (atomisch, consistent, geïsoleerd, duurzaamheid) compatibele transacties met snapshot-isolatie. Alle databasebewerkingen binnen het bereik van een container [logische partitie](partition-data.md) transactioneel worden uitgevoerd in de database-engine die wordt gehost door de replica van de partitie. Deze bewerkingen zijn beide bewerkingen lezen en schrijven (bijwerken van een of meer items in de logische partitie). De volgende tabel ziet u verschillende bewerkingen en transactie-typen:

| **Bewerking**  | **Type bewerking** | **Één of meerdere Item transactie** |
|---------|---------|---------|
| Invoegen (zonder een vooraf/post-trigger) | Schrijven | Één item transactie |
| Plaats (met een vooraf/post-trigger) | Lezen en schrijven | Meerdere item transactie |
| Vervangen (zonder een vooraf/post-trigger) | Schrijven | Één item transactie |
| Vervang (met een vooraf/post-trigger) | Lezen en schrijven | Meerdere item transactie |
| Upsert (zonder een vooraf/post-trigger) | Schrijven | Één item transactie |
| Upsert (met een vooraf/post-trigger) | Lezen en schrijven | Meerdere item transactie |
| Verwijderen (zonder een vooraf/post-trigger) | Schrijven | Één item transactie |
| Verwijderen (met een vooraf/post-trigger) | Lezen en schrijven | Meerdere item transactie |
| Opgeslagen procedures uitvoeren | Lezen en schrijven | Meerdere item transactie |
| Systeem gestart uitvoering van een procedure samenvoegen | Schrijven | Meerdere item transactie |
| Systeem gestart uitvoering van de items op basis van verlopen (TTL) van een item verwijderen | Schrijven | Meerdere item transactie |
| Lezen | Lezen | Één item transactie |
| Feed wijzigen | Lezen | Meerdere item transactie |
| Gepagineerde lezen | Lezen | Meerdere item transactie |
| Gepagineerde Query | Lezen | Meerdere item transactie |
| UDF als onderdeel van de gepagineerde query uitvoeren | Lezen | Meerdere item transactie |

## <a name="multi-item-transactions"></a>Meerdere item transacties

Azure Cosmos DB kunt u schrijven [opgeslagen procedures, triggers vooraf/post, gebruiker-gedefinieerde-functies (UDF's)](stored-procedures-triggers-udfs.md) en procedures in JavaScript samen te voegen. Azure Cosmos DB biedt systeemeigen ondersteuning voor de JavaScript-uitvoering in de database-engine. Kunt u zich registreren met opgeslagen procedures, vooraf/post triggers, gebruiker-gedefinieerde-functies (UDF's) en procedures samenvoegen in een container en hoger voert ze transactioneel binnen de Azure Cosmos-database-engine. Toepassingslogica schrijven in JavaScript kunt natuurlijke vorm van Controlestroom, variabele scoping, toewijzing en integratie van uitzonderingsverwerking primitieven in de databasetransacties rechtstreeks in de JavaScript-taal.

De JavaScript-gebaseerde opgeslagen procedures, triggers, UDF's en procedures voor samenvoegen zijn ingepakt binnen een ambient ACID-transactie met snapshot-isolatie voor alle items in de logische partitie. In de loop van de uitvoering ervan, als de JavaScript-programma genereert een uitzondering, de hele transactie is afgebroken en Service-update-back. De resulterende programmeermodel is eenvoudige maar krachtige. JavaScript-ontwikkelaars ophalen een duurzame programmeermodel terwijl nog steeds met behulp van hun eigen bekende taal constructs en bibliotheek primitieven.

De mogelijkheid voor het uitvoeren van JavaScript direct in de database-engine biedt prestaties en transactionele uitvoering van databasebewerkingen op de items van een container. Bovendien, omdat Azure Cosmos-database-engine systeemeigen ondersteuning voor JSON en JavaScript, is er geen wat betreft impedantie tussen het type van een toepassing en de database.

## <a name="optimistic-concurrency-control"></a>Optimistische gelijktijdigheid beheren 

Optimistisch gelijktijdigheidbeheer kunt u voorkomen dat updates verloren gaan en wordt verwijderd. Gelijktijdige, conflicterende bewerkingen worden onderworpen aan de reguliere pessimistische vergrendeling van de database-engine die wordt gehost door de logische partitie die eigenaar is van het item. Wanneer twee gelijktijdige bewerkingen proberen bij te werken van de meest recente versie van een item in een logische partitie, een van deze wordt win en de andere mislukken. Echter, als een of twee bewerkingen bij het gelijktijdig bijwerken van hetzelfde artikel eerder een oudere waarde van het item lezen heeft, de database niet weten als de eerder leeswaarden door een of beide de conflicterende bewerkingen inderdaad de laatste waarde van het item. Gelukkig deze situatie kan worden gedetecteerd met de **optimistische gelijktijdigheid besturingselement (OCC)** voordat waarin de twee bewerkingen opgeven de transactiegrens van de in de database-engine. OCC beschermt uw gegevens tegen per ongeluk worden overschreven wijzigingen die zijn aangebracht door anderen. Dit voorkomt ook dat anderen uw eigen wijzigingen per ongeluk wordt overschreven.

De gelijktijdige updates van een item zijn onderworpen aan de OCC door Azure Cosmos DB-communicatielaag-protocol. Azure Cosmos-database zorgt ervoor dat de client-side-versie van het item dat u bijwerken (of verwijderen) gelijk zijn aan de versie van het item in de Azure Cosmos-container. Dit zorgt ervoor dat uw schrijfbewerkingen zijn beschermd tegen het per ongeluk worden overschreven door het schrijven van anderen en vice versa. In een omgeving met meerdere gebruikers voorkomt de Optimistisch gelijktijdigheidbeheer dat u per ongeluk verwijderen of bijwerken van de verkeerde versie van een item. Items zijn zo beschermd tegen de roemruchte "verloren update" of 'verloren verwijderen' problemen.

Elk item dat is opgeslagen in een Azure Cosmos-container is een systeem gedefinieerd `_etag` eigenschap. De waarde van de `_etag` automatisch wordt gegenereerd en bijgewerkt door de server telkens als het item wordt bijgewerkt. `_etag` kan worden gebruikt met de opgegeven client `if-match` aanvraagheader om toe te staan van de server om te bepalen of een item voorwaardelijk kan worden bijgewerkt. De waarde van de `if-match` header komt overeen met de waarde van de `_etag` op de server, klikt u vervolgens het item wordt bijgewerkt. Als de waarde van de `if-match` aanvraagheader is niet meer actueel, worden geweigerd door de server de bewerking opnieuw uit met een ' HTTP 412 Precondition failure ' antwoordbericht. De client vervolgens kunt opnieuw ophalen van het item om het verkrijgen van de huidige versie van het item op de server of de versie van het item op de server met een eigen overschrijven `_etag` waarde voor het item. Bovendien `_etag` kan worden gebruikt met de `if-none-match` header om te bepalen of een opnieuw ophalen van een resource nodig is. 

Van het item `_etag` wijzigingen waarde telkens als het item wordt bijgewerkt. Voor item vervangen, `if-match` expliciet moet worden uitgedrukt als een onderdeel van de Aanvraagopties. Zie voor een voorbeeld: de voorbeeldcode in [GitHub](https://github.com/Azure/azure-documentdb-dotnet/blob/master/samples/code-samples/DocumentManagement/Program.cs#L398-L446). `_etag` waarden zijn impliciet gecontroleerd op alle geschreven artikelen die door de opgeslagen procedure. Als er een conflict wordt gedetecteerd, wordt de opgeslagen procedure Draai de transactie terug en een uitzondering. Met deze methode worden alle of geen schrijfbewerkingen binnen de opgeslagen procedure atomisch toegepast. Dit is een signaal aan de toepassing om de updates opnieuw en probeer de aanvraag van de oorspronkelijke client.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over databasetransacties en functionaliteit voor optimistische gelijktijdigheid beheren in de volgende artikelen:

- [Werken met Azure Cosmos-databases, containers en objecten](databases-containers-items.md)
- [Consistentieniveaus](consistency-levels.md)
- [Conflicttypen en oplossingsbeleid](conflict-resolution-policies.md)
- [Opgeslagen procedures, triggers en gebruikersgedefinieerde functies](stored-procedures-triggers-udfs.md)
