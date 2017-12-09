---
title: Aan de slag met de hulpmiddelen voor elastische databases - Azure | Microsoft Docs
description: Basic uitleg van de hulpmiddelen voor elastische databases-functie van Azure SQL Database, met inbegrip van een eenvoudig run voorbeeld-app.
services: sql-database
documentationcenter: 
manager: jstrauss
author: anumjs
editor: CarlRabeler
ms.assetid: b6911f8d-2bae-4d04-9fa8-f79a3db7129d
ms.service: sql-database
ms.custom: scale out apps
ms.workload: On Demand
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/16/2017
ms.author: anjangsh
ms.openlocfilehash: 615e5b0bf299a844ea2d37476fc704c48e17b363
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2017
---
# <a name="get-started-with-elastic-database-tools"></a>Aan de slag met de hulpmiddelen voor elastische databases
Dit document vindt u de functionaliteit voor ontwikkelaars voor de [clientbibliotheek voor elastische database](sql-database-elastic-database-client-library.md) doordat u een voorbeeld-app uitvoeren. De voorbeeld-app maakt een eenvoudige shard-toepassing en behandelt de belangrijkste mogelijkheden van de functie voor elastische Database's van Azure SQL Database. Dit artikel gaat over gebruiksvoorbeelden voor [shard kaart management](sql-database-elastic-scale-shard-map-management.md), [gegevensafhankelijke routering](sql-database-elastic-scale-data-dependent-routing.md), en [opvragen van meerdere shard](sql-database-elastic-scale-multishard-querying.md). De clientbibliotheek is beschikbaar voor .NET en Java. 

## <a name="elastic-database-tools-for-java"></a>Elastische Database-hulpprogramma's voor Java
### <a name="prerequisites"></a>Vereisten
* Een Java Developer Kit (JDK), versie 1.8 of hoger
* [Maven](http://maven.apache.org/download.cgi)
* Een logische server in Azure of een lokaal exemplaar van SQL Server

### <a name="download-and-run-the-sample-app"></a>De voorbeeldapp downloaden en uitvoeren
Voor het bouwen van de JAR-bestanden en aan de slag met het voorbeeldproject, het volgende doen: 
1. Kloon de [GitHub-opslagplaats](https://github.com/Microsoft/elastic-db-tools-for-java) met de clientbibliotheek, samen met de voorbeeld-app. 

2. Bewerk de _./sample/src/main/resources/resource.properties_ bestand naar het volgende instellen:
    * TEST_CONN_USER
    * TEST_CONN_PASSWORD
    * TEST_CONN_SERVER_NAME

3. Om het voorbeeldproject in samen te stellen de _. / steekproef_ map en voer de volgende opdracht:

    ```
    mvn install
    ```
    
4. Starten van het voorbeeldproject in de _. / steekproef_ map en voer de volgende opdracht: 
    
    ```
    mvn -q exec:java "-Dexec.mainClass=com.microsoft.azure.elasticdb.samples.elasticscalestarterkit.Program"
    ```
    
5. Experimenteer met de verschillende opties voor meer informatie over de mogelijkheden van de client-bibliotheek. U kunt de code voor meer informatie over de voorbeeld-app-implementatie te verkennen.

    ![Voortgang van java-][5]
    
Gefeliciteerd. U hebt gebouwd en uitvoeren van uw eerste shard-toepassing met behulp van de hulpmiddelen voor elastische databases op Azure SQL Database. Visual Studio of SQL Server Management Studio gebruiken om te verbinden met uw SQL-database en bekijk snel de shards die het voorbeeld is gemaakt. U zult nieuwe voorbeeld shard-databases en een shard kaart manager-database die is gemaakt met het voorbeeld. 

Toevoegen als u wilt de clientbibliotheek toevoegen aan uw eigen Maven-project, de volgende afhankelijkheden in uw bestand POM:

```xml
<dependency> 
    <groupId>com.microsoft.azure</groupId> 
    <artifactId>elastic-db-tools</artifactId> 
    <version>1.0.0</version> 
</dependency> 
```

## <a name="elastic-database-tools-for-net"></a>Elastische Database-hulpprogramma's voor .NET 
### <a name="prerequisites"></a>Vereisten
* Visual Studio 2012 of hoger met C#. Download een gratis versie op [Visual Studio-Downloads](http://www.visualstudio.com/downloads/download-visual-studio-vs.aspx).
* NuGet 2.7 of hoger. Als u de nieuwste versie, Zie [NuGet installeren](http://docs.nuget.org/docs/start-here/installing-nuget).

### <a name="download-and-run-the-sample-app"></a>De voorbeeldapp downloaden en uitvoeren
Als u wilt installeren in de bibliotheek, gaat u naar [Microsoft.Azure.SqlDatabase.ElasticScale.Client](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/). De bibliotheek is geïnstalleerd met de voorbeeld-app in de volgende sectie wordt beschreven.

Als u wilt downloaden en uitvoeren van de steekproef, de volgende stappen uit: 

1. Download de [elastische DB-hulpprogramma's voor Azure SQL - voorbeeld aan de slag](https://code.msdn.microsoft.com/windowsapps/Elastic-Scale-with-Azure-a80d8dc6) van MSDN. Pak het voorbeeld naar een locatie die u kiest.

2. Als u wilt een project maakt, opent u de *ElasticScaleStarterKit.sln* oplossing van de *C#* directory.

3. Open in de oplossing voor het voorbeeldproject de *app.config* bestand. Volg de instructies in het bestand de naam van uw Azure SQL Database-server en uw gegevens aanmelden (gebruikersnaam en wachtwoord) toe te voegen.

4. Ontwikkel en voer de toepassing. Wanneer u wordt gevraagd, schakel u Visual Studio de NuGet-pakketten van de oplossing te herstellen. Hierdoor downloadt de nieuwste versie van de clientbibliotheek voor elastische database van NuGet.

5. Experimenteer met de verschillende opties voor meer informatie over de mogelijkheden van de client-bibliotheek. Noteer de stappen die nodig is de toepassing in de console uitvoer en Verken de code achter de schermen gerust.
   
    ![Voortgang][4]

Gefeliciteerd. U hebt gebouwd en uitvoeren van uw eerste shard-toepassing met behulp van de hulpmiddelen voor elastische databases op SQL-Database. Visual Studio of SQL Server Management Studio gebruiken om te verbinden met uw SQL-database en bekijk snel de shards die het voorbeeld is gemaakt. U zult nieuwe voorbeeld shard-databases en een shard kaart manager-database die is gemaakt met het voorbeeld.

> [!IMPORTANT]
> Het is raadzaam dat u de nieuwste versie van Management Studio altijd gebruiken zodat u gesynchroniseerd met updates voor Azure en SQL-Database blijven. [SQL Server Management Studio bijwerken](https://msdn.microsoft.com/library/mt238290.aspx).
> 
> 

## <a name="key-pieces-of-the-code-sample"></a>Belangrijke onderdelen van de codevoorbeeld
* **Toewijzingen beheren shards en shard**: de code laat zien hoe u werkt met shards, bereiken en toewijzingen in de *ShardManagementUtils.cs* bestand. Zie voor meer informatie [Scale-out databases met de shard-toewijzing manager](http://go.microsoft.com/?linkid=9862595).  

* **Gegevensafhankelijke routering**: routering van transacties naar de juiste shard wordt weergegeven in de *DataDependentRoutingSample.cs* bestand. Zie voor meer informatie [gegevensafhankelijke routering](http://go.microsoft.com/?linkid=9862596). 

* **Uitvoeren van query's via meerdere shards**: uitvoeren van query's over shards wordt weergegeven in de *MultiShardQuerySample.cs* bestand. Zie voor meer informatie [opvragen van meerdere shard](http://go.microsoft.com/?linkid=9862597).

* **Toe te voegen leeg shards**: het herhalende toevoegen van nieuwe leeg shards wordt uitgevoerd door de code in de *CreateShardSample.cs* bestand. Zie voor meer informatie [Scale-out databases met de shard-toewijzing manager](http://go.microsoft.com/?linkid=9862595).

## <a name="other-elastic-scale-operations"></a>Andere bewerkingen elastisch schalen
* **Het splitsen van een bestaande shard**: de mogelijkheid om op te splitsen shards wordt geleverd door het hulpprogramma gesplitste samenvoegen. Zie voor meer informatie [verplaatsen van gegevens tussen cloud uitgebreide databases](sql-database-elastic-scale-overview-split-and-merge.md).

* **Samenvoegen van bestaande shards**: Shard samenvoegingen worden ook uitgevoerd met het hulpprogramma gesplitste samenvoegen. Zie voor meer informatie [verplaatsen van gegevens tussen cloud uitgebreide databases](sql-database-elastic-scale-overview-split-and-merge.md).   

## <a name="cost"></a>Kosten
De bibliotheek hulpmiddelen voor elastische databases is gratis. Wanneer u de hulpmiddelen voor elastische databases gebruikt, kosten u die geen extra buiten de kosten van het gebruik van uw Azure. 

De voorbeeldtoepassing maakt bijvoorbeeld nieuwe databases. De kosten van deze mogelijkheid is afhankelijk van de versie van de SQL-Database die u kiest en de Azure gebruik van uw toepassing.

Zie voor informatie over prijzen, [SQL-Database prijsinformatie](https://azure.microsoft.com/pricing/details/sql-database/).

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over hulpmiddelen voor elastische databases, de volgende artikelen:

* Codevoorbeelden: 
  * Hulpprogramma's voor elastische Database ([.NET](http://code.msdn.microsoft.com/Elastic-Scale-with-Azure-a80d8dc6?SRC=VSIDE), [Java](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-elasticdb-tools%22))
  * [Elastische Database-hulpprogramma's voor Azure SQL - Entity Framework-integratie](http://code.msdn.microsoft.com/Elastic-Scale-with-Azure-bae904ba?SRC=VSIDE)
  * [Shard-elasticiteit op Script Center](https://gallery.technet.microsoft.com/scriptcenter/Elastic-Scale-Shard-c9530cbe)
* Blog: [aankondiging elastisch schalen](https://azure.microsoft.com/blog/2014/10/02/introducing-elastic-scale-preview-for-azure-sql-database/)
* Microsoft Virtual Academy: [Sharding met behulp van Scale-Out met de elastische Database Client bibliotheek Video implementeren](https://mva.microsoft.com/training-courses/elastic-database-capabilities-with-azure-sql-db-16554?l=lWyQhF1fC_6306218965) 
* Channel 9: [elastische Schaalfunctionaliteit van video-overzicht](http://channel9.msdn.com/Shows/Data-Exposed/Azure-SQL-Database-Elastic-Scale)
* Discussieforum: [-forum Azure SQL Database](http://social.msdn.microsoft.com/forums/azure/home?forum=ssdsgetstarted)
* Om prestaties te meten: [prestatiemeteritems voor het beheer van shard-kaart](sql-database-elastic-database-client-library.md)

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

