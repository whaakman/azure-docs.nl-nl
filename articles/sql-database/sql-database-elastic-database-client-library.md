---
title: Schaalbare clouddatabases bouwen | Microsoft Docs
description: Bouw schaalbare apps van .NET-database met de clientbibliotheek van elastische database
services: sql-database
ms.service: sql-database
ms.subservice: scale-out
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: ''
manager: craigg
ms.date: 04/01/2018
ms.openlocfilehash: 15a16201f83cc9e7244eb0be9d4e185d5e1e3922
ms.sourcegitcommit: 51a1476c85ca518a6d8b4cc35aed7a76b33e130f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/25/2018
ms.locfileid: "47165156"
---
# <a name="building-scalable-cloud-databases"></a>Schaalbare clouddatabases bouwen
Uitschalen databases kan gemakkelijk worden gedaan met behulp van schaalbare hulpmiddelen en functies voor Azure SQL Database. In het bijzonder, kunt u de **Elastic Database-clientbibliotheek** naar uitgeschaalde databases maken en beheren. Deze functie kunt u eenvoudig ontwikkelen shard-toepassingen met honderden, of zelfs duizenden — van Azure SQL-databases. [Elastische taken](sql-database-elastic-jobs-powershell.md) kan vervolgens worden gebruikt om u te helpen bij het vereenvoudigen van deze databases.

Downloaden:
* De Java-versie van de bibliotheek, Zie [Maven Central Repository](https://search.maven.org/#search%7Cga%7C1%7Celastic-db-tools).
* De .NET-versie van de bibliotheek, Zie [NuGet](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/).

## <a name="documentation"></a>Documentatie
1. [Aan de slag met tools voor Elastic Database](sql-database-elastic-scale-get-started.md)
2. [Functies voor elastische Database](sql-database-elastic-scale-introduction.md)
3. [Shard-toewijzingsbeheer](sql-database-elastic-scale-shard-map-management.md)
4. [Bestaande databases migreren voor uitschalen](sql-database-elastic-convert-to-use-elastic-tools.md)
5. [Gegevensafhankelijke routering](sql-database-elastic-scale-data-dependent-routing.md)
6. [Multi-shard query 's](sql-database-elastic-scale-multishard-querying.md)
7. [Een shard met behulp van hulpprogramma's voor elastische databases toevoegen](sql-database-elastic-scale-add-a-shard.md)
8. [Multitenant-toepassingen met elastische Databasehulpprogramma's en beveiliging op rijniveau](sql-database-elastic-tools-multi-tenant-row-level-security.md)
9. [Upgrade uitvoeren voor client-bibliotheek apps](sql-database-elastic-scale-upgrade-client-library.md) 
10. [Elastische query's-overzicht](sql-database-elastic-query-overview.md)
11. [Woordenlijst voor hulpprogramma's voor Elastic Database](sql-database-elastic-scale-glossary.md)
12. [Elastische Database-clientbibliotheek met Entity Framework](sql-database-elastic-scale-use-entity-framework-applications-visual-studio.md)
13. [Clientbibliotheek voor elastic database met Dapper](sql-database-elastic-scale-working-with-dapper.md)
14. [Hulpprogramma voor splitsen en samenvoegen](sql-database-elastic-scale-overview-split-and-merge.md)
15. [Prestatiemeteritems voor shard-toewijzingsbeheer](sql-database-elastic-database-client-library.md) 
16. [Veelgestelde vragen over de hulpmiddelen voor Elastic database](sql-database-elastic-scale-faq.md)

## <a name="client-capabilities"></a>Client-mogelijkheden
Toepassingen die gebruikmaken van uitschalen *sharding* geeft uitdagingen voor zowel de ontwikkelaar als de beheerder. De clientbibliotheek vereenvoudigt de beheertaken dankzij de hulpprogramma's waarmee zowel ontwikkelaars en beheerders uitgeschaalde databases beheren. In een typisch voorbeeld zijn er veel databases, bekend als "shards," om te beheren. Klanten in dezelfde database zich bevindt en er is één database per klant (een schema voor één tenant). De clientbibliotheek omvat de volgende functies:

- **Shard-Toewijzingsbeheer**: een speciale database met de naam van de 'shard-Toewijzingsbeheer' wordt gemaakt. Shard-Toewijzingsbeheer is de mogelijkheid voor een toepassing voor het beheren van metagegevens over de shards. Ontwikkelaars kunnen deze functionaliteit gebruiken voor het registreren van databases als shards, beschrijven toewijzingen van afzonderlijke sharding sleutels of sleutelbereiken aan die databases en onderhouden van deze metagegevens als het getal en samenstelling van de databases als gevolg van wijzigingen van de capaciteit zich verder ontwikkelt. Zonder de clientbibliotheek van elastische database moet u besteden veel tijd aan de beheer-code te schrijven bij het implementeren van sharding. Zie voor meer informatie, [Shard-Toewijzingsbeheer](sql-database-elastic-scale-shard-map-management.md).

- **Gegevensafhankelijke routering**: Stel dat een aanvraag die afkomstig zijn in de toepassing. Op basis van het waarde van de sharding-sleutel van de aanvraag, moet de toepassing om te bepalen van de juiste database op basis van de sleutelwaarde. Deze opent vervolgens een verbinding met de database om de aanvraag te verwerken. Gegevensafhankelijke routering biedt de mogelijkheid om te openen van verbindingen met één eenvoudig aanroep in de shard-toewijzing van de toepassing. Gegevensafhankelijke routering is een ander aspect van infrastructuurcode die nu wordt gedekt door functionaliteit in de clientbibliotheek van elastische database. Zie voor meer informatie, [gegevensafhankelijke routering](sql-database-elastic-scale-data-dependent-routing.md).
- **Multi-shard query's (MSQ)**: meerdere shards uitvoeren van query's werkt wanneer een aanvraag verschillende (of alle) shards omvat. De dezelfde T-SQL-code een multi-shard query uitgevoerd op alle shards of een set met shards. De resultaten van de deelnemende shards worden samengevoegd in een algemene resultatenset via semantiek voor UNION ALL. De functionaliteit, die toegankelijk is via de clientbibliotheek handelt vele taken, met inbegrip van: beheer, thread management afhandeling van fouten en tussenliggende resultaten verwerken. MSQ kunnen honderden shards opvragen. Zie voor meer informatie, [meerdere shards uitvoeren van query's](sql-database-elastic-scale-multishard-querying.md).

Klanten die gebruikmaken van hulpprogramma's voor elastische databases kunnen in het algemeen verwachten volledige T-SQL-functionaliteit ophalen bij het indienen van shard-lokale bewerkingen in plaats van over meerdere shards bewerkingen die hun eigen heeft.



## <a name="next-steps"></a>Volgende stappen

- Clientbibliotheek voor elastic Database ([Java](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-elasticdb-tools%22), [.NET](http://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/)) - voor **downloaden** de bibliotheek.

- [Aan de slag met elastische Databasehulpprogramma's](sql-database-elastic-scale-get-started.md) - om te proberen de **voorbeeldapp** die clientfuncties laat zien.

- GitHub ([Java](https://github.com/Microsoft/elastic-db-tools-for-java/blob/master/README.md), [.NET](https://github.com/Azure/elastic-db-tools)): als u wilt bijdragen te leveren aan de code.
- [Overzicht van Azure SQL Database elastische query](sql-database-elastic-query-overview.md) : gebruik elastische query's.

- [Gegevens verplaatsen tussen uitgeschaalde clouddatabases](sql-database-elastic-scale-overview-split-and-merge.md) - voor instructies over het gebruik van de **hulpprogramma voor splitsen en samenvoegen**.



<!-- Additional resources H2 -->

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]


<!--Anchors-->
<!--Image references-->

[1]: ./media/sql-database-elastic-database-client-library/glossary.png

