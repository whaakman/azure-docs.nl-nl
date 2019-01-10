---
title: Beheer van referenties in de bibliotheek met elastische-client | Microsoft Docs
description: Over het instellen van het juiste niveau van de referenties van beheerder zijn om alleen-lezen, voor elastische database-apps
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
ms.date: 01/03/2019
ms.openlocfilehash: e6c7d682d9b16266208baeeff14168b3da157251
ms.sourcegitcommit: 63b996e9dc7cade181e83e13046a5006b275638d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/10/2019
ms.locfileid: "54186616"
---
# <a name="credentials-used-to-access-the-elastic-database-client-library"></a>Referenties voor toegang tot de clientbibliotheek voor Elastic Database

De [Elastic Database-clientbibliotheek](sql-database-elastic-database-client-library.md) drie verschillende soorten referenties gebruikt voor toegang tot de [shard-Toewijzingsbeheer](sql-database-elastic-scale-shard-map-management.md). Afhankelijk van de noodzaak de referentie op die met het laagste niveau van toegang mogelijk te gebruiken.

* **Beheerreferenties**: voor het maken of bewerken van een shard-toewijzing. (Zie de [verklarende woordenlijst](sql-database-elastic-scale-glossary.md).)
* **Toegang tot referenties**: voor toegang tot een bestaande shard-Toewijzingsbeheer voor informatie over de shards.
* **Verbindingsreferenties**: verbinding maken met shards.

Zie ook [databases en aanmeldingen beheren in Azure SQL Database](sql-database-manage-logins.md).

## <a name="about-management-credentials"></a>Over het van beheerreferenties

Van beheerreferenties worden gebruikt voor het maken van een **ShardMapManager** ([Java](/java/api/com.microsoft.azure.elasticdb.shard.mapmanager.shardmapmanager), [.NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager))-object voor toepassingen die wijzigingen shard-toewijzingen aanbrengen. (Zie bijvoorbeeld [toe te voegen een shard met behulp van hulpprogramma's voor elastische databases](sql-database-elastic-scale-add-a-shard.md) en [gegevensafhankelijke routering](sql-database-elastic-scale-data-dependent-routing.md)). De gebruiker van de clientbibliotheek van elastische schaal wordt gemaakt van de SQL-gebruikers en de SQL-aanmeldingen en zorgt ervoor dat elk krijgt de machtigingen voor lezen/schrijven voor de globale database voor shard en ook alle shard-databases. Deze referenties worden gebruikt voor het onderhouden van de globale shard-toewijzing en het lokale shard-toewijzingen wanneer wijzigingen in de shard-toewijzing worden uitgevoerd. Gebruik bijvoorbeeld de beheerreferenties te maken van het object shard map manager (met behulp van **GetSqlShardMapManager** ([Java](/java/api/com.microsoft.azure.elasticdb.shard.mapmanager.shardmapmanagerfactory.getsqlshardmapmanager), [.NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.getsqlshardmapmanager)):

```java
// Obtain a shard map manager.
ShardMapManager shardMapManager = ShardMapManagerFactory.GetSqlShardMapManager(smmAdminConnectionString,ShardMapManagerLoadPolicy.Lazy);
```

De variabele **smmAdminConnectionString** een verbindingsreeks met de beheerreferenties. De gebruikers-ID en het wachtwoord voor lezen/schrijven toegang bieden tot zowel database voor shard- en afzonderlijke shards. De verbindingsreeks management bevat ook de servernaam en databasenaam voor het identificeren van de globale database voor shard. Hier volgt een gebruikelijke verbindingsreeks voor dat doel:

```java
"Server=<yourserver>.database.windows.net;Database=<yourdatabase>;User ID=<yourmgmtusername>;Password=<yourmgmtpassword>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30;”
```

Gebruik niet de waarden in de vorm van 'username@server', in plaats daarvan de waarde 'gebruikersnaam' gebruiken.  Dit is omdat de referenties moeten worden toegepast op de database voor shard-Toewijzingsbeheer en de afzonderlijke shards, die mogelijk op verschillende servers.

## <a name="access-credentials"></a>Toegangsreferenties

Bij het maken van een shard-Toewijzingsbeheer in een toepassing waarin de shard-toewijzingen niet wordt beheerd, gebruikt u de referenties die alleen-lezen-machtigingen op de globale shard-toewijzing hebben. De gegevens opgehaald uit de globale shard-toewijzing onder deze referenties worden gebruikt voor [gegevensafhankelijke routering](sql-database-elastic-scale-data-dependent-routing.md) en voor het vullen van de cache van de shard-kaart op de client. De referenties worden geleverd via hetzelfde patroon aanroep naar **GetSqlShardMapManager**:

```java
// Obtain shard map manager.
ShardMapManager shardMapManager = ShardMapManagerFactory.GetSqlShardMapManager(smmReadOnlyConnectionString, ShardMapManagerLoadPolicy.Lazy);  
```

Let op het gebruik van de **smmReadOnlyConnectionString** in overeenstemming met het gebruik van andere referenties voor deze toegang namens **niet-beheerders** gebruikers: deze referenties moeten geen schrijfmachtigingen op de globale shard-toewijzing.

## <a name="connection-credentials"></a>Verbindingsreferenties

Aanvullende referenties nodig zijn bij het gebruik van de **OpenConnectionForKey** ([Java](/java/api/com.microsoft.azure.elasticdb.shard.mapper.listshardmapper.openconnectionforkey), [.NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.openconnectionforkey)) methode voor toegang tot een shard die zijn gekoppeld aan een sharding-sleutel. Deze referenties moeten opgeven van machtigingen voor alleen-lezen toegang tot de lokale shard map tabellen op de shard. Dit is nodig voor het uitvoeren van verbindingsvalidatie voor gegevensafhankelijke routering op de shard. Dit codefragment kunt toegang tot gegevens in de context van gegevensafhankelijke routering:

```csharp
using (SqlConnection conn = rangeMap.OpenConnectionForKey<int>(targetWarehouse, smmUserConnectionString, ConnectionOptions.Validate))
```

In dit voorbeeld **smmUserConnectionString** bevat de verbindingsreeks voor referenties van de gebruiker. Hier volgt een gebruikelijke verbindingsreeks voor referenties van gebruiker voor Azure SQL DB:

```java
"User ID=<yourusername>; Password=<youruserpassword>; Trusted_Connection=False; Encrypt=True; Connection Timeout=30;”  
```

Als met de referenties van de beheerder, gebruik geen waarden in de vorm van 'username@server'. In plaats daarvan gebruikt u 'gebruikersnaam'.  Houd er ook rekening mee dat de verbindingsreeks een servernaam en databasenaam bevat. Dat komt doordat de **OpenConnectionForKey** aanroep automatisch de verbinding met de juiste shard op basis van de sleutel om te leiden. Daarom kan zijn de databasenaam en de servernaam niet opgegeven.

## <a name="see-also"></a>Zie ook

[Databases en aanmeldingen beheren in Azure SQL Database](sql-database-manage-logins.md)

[Uw SQL-database beveiligen](sql-database-security-overview.md)

[Aan de slag met taken voor Elastic Database](sql-database-elastic-jobs-getting-started.md)

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]
