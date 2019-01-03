---
title: Aan de slag met elastische Databasehulpprogramma's - Azure | Microsoft Docs
description: Basic uitleg van de functie Elastic Database-hulpprogramma's van Azure SQL Database, met inbegrip van een eenvoudig-en-klaar voorbeeld-app.
services: sql-database
ms.service: sql-database
ms.subservice: scale-out
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: anumjs
ms.author: anjangsh
ms.reviewer: sstein
manager: craigg
ms.date: 08/27/2018
ms.openlocfilehash: b3bdcc81776067f279c1f95458a0a79a8824f51c
ms.sourcegitcommit: 4eeeb520acf8b2419bcc73d8fcc81a075b81663a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/19/2018
ms.locfileid: "53603046"
---
# <a name="get-started-with-elastic-database-tools"></a>Aan de slag met elastische Databasehulpprogramma 's
Dit document bevat een inleiding tot de ervaring voor ontwikkelaars voor het [clientbibliotheek voor elastic database](sql-database-elastic-database-client-library.md) doordat u een voorbeeld-app uitvoeren. De voorbeeld-app een eenvoudige sharded toepassing maakt en gaat in op de belangrijkste mogelijkheden van de functie Elastic Database-hulpprogramma's van Azure SQL Database. Dit artikel gaat over gebruiksvoorbeelden voor [shard-Toewijzingsbeheer](sql-database-elastic-scale-shard-map-management.md), [gegevensafhankelijke routering](sql-database-elastic-scale-data-dependent-routing.md), en [meerdere shards uitvoeren van query's](sql-database-elastic-scale-multishard-querying.md). De clientbibliotheek is beschikbaar voor zowel .NET als Java. 

## <a name="elastic-database-tools-for-java"></a>Hulpprogramma's voor elastische databases voor Java
### <a name="prerequisites"></a>Vereisten
* Een Java Developer Kit (JDK), versie 1.8 of hoger
* [Maven](http://maven.apache.org/download.cgi)
* Een logische server in Azure of een lokale SQL Server-exemplaar

### <a name="download-and-run-the-sample-app"></a>De voorbeeldapp downloaden en uitvoeren
Voor het bouwen van de JAR-bestanden en aan de slag met het voorbeeldproject, het volgende doen: 
1. Kloon de [GitHub-opslagplaats](https://github.com/Microsoft/elastic-db-tools-for-java) met de clientbibliotheek, samen met de voorbeeld-app. 

2. Bewerk de _./sample/src/main/resources/resource.properties_ -bestand naar het volgende instellen:
    * TEST_CONN_USER
    * TEST_CONN_PASSWORD
    * TEST_CONN_SERVER_NAME

3. Het voorbeeldproject in bouwen de _. / voorbeeld_ directory, voert u de volgende opdracht uit:

    ```
    mvn install
    ```
    
4. Om te beginnen het voorbeeldproject met de _. / voorbeeld_ directory, voert u de volgende opdracht uit: 
    
    ```
    mvn -q exec:java "-Dexec.mainClass=com.microsoft.azure.elasticdb.samples.elasticscalestarterkit.Program"
    ```
    
5. Experimenteer met de verschillende opties voor meer informatie over de mogelijkheden voor de bibliotheek. U kunt de code voor meer informatie over de voorbeeld-app-implementatie verkennen.

    ![Progress-java][5]
    
Gefeliciteerd! U hebt gemaakt en uitvoeren van uw eerste shard-toepassing met behulp van hulpprogramma's voor elastische databases op Azure SQL Database. Gebruik Visual Studio of SQL Server Management Studio verbinding maken met uw SQL-database en een beknopt overzicht van de shards die het voorbeeld hebt gemaakt. U zult de nieuwe voorbeeld shard-databases en een shard map manager-database die het voorbeeld is gemaakt. 

De clientbibliotheek toevoegen aan uw eigen Maven-project, toevoegen de volgende afhankelijkheden in het POM-bestand:

```xml
<dependency> 
    <groupId>com.microsoft.azure</groupId> 
    <artifactId>elastic-db-tools</artifactId> 
    <version>1.0.0</version> 
</dependency> 
```

## <a name="elastic-database-tools-for-net"></a>Hulpprogramma's voor elastische databases voor .NET 
### <a name="prerequisites"></a>Vereisten
* Visual Studio 2012 of hoger met C#. Download een gratis versie op [Visual Studio-Downloads](https://www.visualstudio.com/downloads/download-visual-studio-vs.aspx).
* NuGet 2.7 of later. Als u de nieuwste versie, raadpleegt u [NuGet installeren](http://docs.nuget.org/docs/start-here/installing-nuget).

### <a name="download-and-run-the-sample-app"></a>De voorbeeldapp downloaden en uitvoeren
Voor het installeren van de bibliotheek, gaat u naar [Microsoft.Azure.SqlDatabase.ElasticScale.Client](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/). De bibliotheek is geïnstalleerd met de voorbeeld-app die wordt beschreven in de volgende sectie.

Als u wilt downloaden en uitvoeren van het voorbeeld, de volgende stappen uit: 

1. Download de [elastische DB-hulpprogramma's voor Azure SQL - aan de slag-voorbeeld](https://code.msdn.microsoft.com/windowsapps/Elastic-Scale-with-Azure-a80d8dc6) van MSDN. Pak deze uit het voorbeeld naar een locatie die u kiest.

2. Als u wilt een project maakt, opent u de *ElasticScaleStarterKit.sln* oplossing uit de *C#* directory.

3. Open in de oplossing voor het voorbeeldproject, de *app.config* bestand. Volg de instructies in het bestand de naam van uw Azure SQL Database-server en uw gegevens aanmelden (gebruikersnaam en wachtwoord) toe te voegen.

4. Ontwikkel en voer de toepassing. Wanneer u wordt gevraagd, zodat u Visual Studio de NuGet-pakketten van de oplossing te herstellen. Deze actie downloadt de nieuwste versie van de elastische database-clientbibliotheek vanuit NuGet.

5. Experimenteer met de verschillende opties voor meer informatie over de mogelijkheden voor de bibliotheek. Houd er rekening mee de stappen die de toepassing in de console wordt uitgevoerd en u kunt de code achter de schermen worden besproken.
   
    ![Voortgang][4]

Gefeliciteerd! U hebt gemaakt en u hebt uw eerste shard-toepassing uitvoeren met behulp van hulpprogramma's voor elastische databases in SQL-Database. Gebruik Visual Studio of SQL Server Management Studio verbinding maken met uw SQL-database en een beknopt overzicht van de shards die het voorbeeld hebt gemaakt. U zult de nieuwe voorbeeld shard-databases en een shard map manager-database die het voorbeeld is gemaakt.

> [!IMPORTANT]
> Raden wij aan dat u altijd de meest recente versie van Management Studio zodat u gesynchroniseerd met updates voor Azure en SQL-Database blijven. [SQL Server Management Studio bijwerken](https://msdn.microsoft.com/library/mt238290.aspx).
> 
> 

## <a name="key-pieces-of-the-code-sample"></a>Belangrijke onderdelen van het codevoorbeeld van
* **Beheer van shards en shard-kaarten**: De code laat zien hoe u werkt met shards, bereiken en -toewijzingen in de *ShardManagementUtils.cs* bestand. Zie voor meer informatie, [Scale-out databases met de shard-Toewijzingsbeheer](https://go.microsoft.com/?linkid=9862595).  

* **Gegevensafhankelijke routering**: Routering van transacties naar de juiste shard wordt weergegeven in de *DataDependentRoutingSample.cs* bestand. Zie voor meer informatie, [gegevensafhankelijke routering](https://go.microsoft.com/?linkid=9862596). 

* **Uitvoeren van query's over meerdere shards**: Het opvragen van meerdere shards wordt weergegeven in de *MultiShardQuerySample.cs* bestand. Zie voor meer informatie, [meerdere shards uitvoeren van query's](https://go.microsoft.com/?linkid=9862597).

* **Lege shards toe te voegen**: Het toevoegen van nieuwe lege shards iteratieve wordt uitgevoerd door de code in de *CreateShardSample.cs* bestand. Zie voor meer informatie, [Scale-out databases met de shard-Toewijzingsbeheer](https://go.microsoft.com/?linkid=9862595).

## <a name="other-elastic-scale-operations"></a>Andere bewerkingen elastisch schalen
* **Het splitsen van een bestaande shard**: De mogelijkheid om op te splitsen shards wordt geleverd door het hulpprogramma voor splitsen en samenvoegen. Zie voor meer informatie, [om gegevens te verplaatsen tussen uitgeschaalde clouddatabases](sql-database-elastic-scale-overview-split-and-merge.md).

* **Samenvoegen van bestaande shards**: Shard-samenvoegingen worden ook uitgevoerd met behulp van het hulpprogramma voor splitsen en samenvoegen. Zie voor meer informatie, [om gegevens te verplaatsen tussen uitgeschaalde clouddatabases](sql-database-elastic-scale-overview-split-and-merge.md).   

## <a name="cost"></a>Kosten
De hulpprogramma's voor elastische Database-bibliotheek is gratis. Wanneer u Elastic Database-hulpprogramma's gebruikt, worden er geen extra kosten berekend boven de kosten van uw gebruik van Azure. 

De voorbeeldtoepassing maakt bijvoorbeeld nieuwe databases. De kosten van deze mogelijkheid is afhankelijk van de versie van de SQL-Database die u kiest en het gebruik van Azure van uw toepassing.

Zie voor informatie over de prijzen [SQL-Database prijsinformatie](https://azure.microsoft.com/pricing/details/sql-database/).

## <a name="next-steps"></a>Volgende stappen
Zie de volgende artikelen voor meer informatie over hulpprogramma's voor elastische databases:

* Codevoorbeelden: 
  * Hulpmiddelen voor elastic Database ([.NET](https://code.msdn.microsoft.com/Elastic-Scale-with-Azure-a80d8dc6?SRC=VSIDE), [Java](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-elasticdb-tools%22))
  * [Hulpmiddelen voor elastic Database voor Azure SQL - integratie met Entity Framework](https://code.msdn.microsoft.com/Elastic-Scale-with-Azure-bae904ba?SRC=VSIDE)
  * [Shardflexibiliteit op Script Center](https://gallery.technet.microsoft.com/scriptcenter/Elastic-Scale-Shard-c9530cbe)
* Blog: [Aankondiging van elastische schaal](https://azure.microsoft.com/blog/2014/10/02/introducing-elastic-scale-preview-for-azure-sql-database/)
* Channel 9: [Video elastische schaal-overzicht](http://channel9.msdn.com/Shows/Data-Exposed/Azure-SQL-Database-Elastic-Scale)
* Discussieforum: [Azure SQL Database-forum](https://social.msdn.microsoft.com/forums/azure/home?forum=ssdsgetstarted)
* Voor het meten van prestaties: [Prestatiemeteritems voor shard-toewijzingsbeheer](sql-database-elastic-database-client-library.md)

<!--Anchors-->
[The Elastic Scale Sample Application]: #The-Elastic-Scale-Sample-Application
[Download and Run the Sample App]: #Download-and-Run-the-Sample-App
[Cost]: #Cost
[Next steps]: #next-steps

<!--Image references-->
[1]: ./media/sql-database-elastic-scale-get-started/newProject.png
[2]: ./media/sql-database-elastic-scale-get-started/click-online.png
[3]: ./media/sql-database-elastic-scale-get-started/click-CSharp.png
[4]: ./media/sql-database-elastic-scale-get-started/output2.png
[5]: ./media/sql-database-elastic-scale-get-started/java-client-library.PNG

