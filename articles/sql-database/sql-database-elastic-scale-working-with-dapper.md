---
title: Clientbibliotheek voor elastische database gebruiken met Dapper | Microsoft Docs
description: Gebruik de clientbibliotheek voor elastische database met Dapper.
services: sql-database
documentationcenter: 
manager: jhubbard
author: torsteng
ms.assetid: 463d2676-3b19-47c2-83df-f8c50492c9d2
ms.service: sql-database
ms.custom: scale out apps
ms.workload: Inactive
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/27/2016
ms.author: torsteng
ms.openlocfilehash: c258b1859e14d9783a3dfa75431b69bef4d640fd
ms.sourcegitcommit: dfd49613fce4ce917e844d205c85359ff093bb9c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/31/2017
---
# <a name="using-elastic-database-client-library-with-dapper"></a>Clientbibliotheek voor elastische database gebruiken met Dapper
Dit document is bedoeld voor ontwikkelaars die afhankelijk zijn van Dapper om toepassingen te bouwen, maar ook wilt gaan [elastische database tooling](sql-database-elastic-scale-introduction.md) om toepassingen te maken die sharding implementeren voor de gegevenslaag uitschalen.  Dit document ziet u de wijzigingen in Dapper gebaseerde toepassingen die nodig zijn om te integreren met hulpprogramma's voor elastische database. Onze focus ligt op het samenstellen van de elastische database shard beheer- en gegevensafhankelijke routering met Dapper. 

**Voorbeeldcode**: [hulpmiddelen voor elastische databases voor Azure SQL Database - Dapper integratie](https://code.msdn.microsoft.com/Elastic-Scale-with-Azure-e19fc77f).

Integratie van **Dapper** en **DapperExtensions** met de elastische database-clientbibliotheek voor Azure SQL Database is eenvoudig. Uw toepassingen kunnen gebruikmaken van gegevensafhankelijke routering door het wijzigen van het maken en openen van nieuwe [SqlConnection](http://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.aspx) objecten te gebruiken de [OpenConnectionForKey](http://msdn.microsoft.com/library/azure/dn807226.aspx) aanroepen vanuit de [clientbibliotheek ](http://msdn.microsoft.com/library/azure/dn765902.aspx). Dit beperkt de wijzigingen in uw toepassing alleen wanneer nieuwe verbindingen zijn gemaakt en geopend. 

## <a name="dapper-overview"></a>Overzicht van dapper
**Dapper** is een object-relationele toewijzen. Het .NET-objecten van uw toepassing naar een relationele database (en omgekeerd) worden toegewezen. Het eerste deel van de voorbeeldcode ziet u hoe u de clientbibliotheek voor elastische database kunt integreren met Dapper gebaseerde toepassingen. Het tweede gedeelte van de voorbeeldcode laat zien hoe bij gebruik van zowel Dapper en DapperExtensions integreren.  

De functionaliteit toewijzen in Dapper biedt uitbreidingsmethoden op databaseverbindingen die verzenden T-SQL-instructies voor het uitvoeren en het opvragen van de database te vereenvoudigen. Bijvoorbeeld: Dapper kunt u gemakkelijk om een koppeling tussen uw .NET-objecten en de parameters van de SQL-instructies voor **Execute** -aanroepen, of de resultaten van uw SQL-query's gebruiken in .NET-objecten met behulp van **Query** aanroepen van Dapper. 

Wanneer u DapperExtensions gebruikt, moet u niet langer bieden de SQL-instructies. Extensies methoden zoals **GetList** of **invoegen** via verbinding met de database maken van de SQL-instructies achter de schermen.

Een ander voordeel van Dapper en ook DapperExtensions is dat de toepassing het maken van verbinding met de database bepaalt. Dit helpt communiceren met de clientbibliotheek voor elastische database die beleggingsmakelaars-databaseverbindingen op basis van de toewijzing van shardlets naar databases.

Als u de Dapper assembly's, Zie [Dapper punt net](http://www.nuget.org/packages/Dapper/). Zie voor de Dapper extensies [DapperExtensions](http://www.nuget.org/packages/DapperExtensions).

## <a name="a-quick-look-at-the-elastic-database-client-library"></a>Een kort overzicht van de clientbibliotheek voor elastische database
Met de clientbibliotheek elastische database die u definieert partities van de toepassingsgegevens aangeroepen *shardlets*, toe te wijzen aan de databases en deze door herkennen *sharding sleutels*. U kunt zoveel databases als u uw shardlets verdelen over deze databases nodig hebben. De toewijzing van sharding waarden van de databases is door een shard-toewijzing geleverd door de API's van de bibliotheek opgeslagen. Deze mogelijkheid wordt aangeroepen **shard kaart management**. De shard-kaart fungeert ook als de broker databaseverbindingen voor aanvragen die een sharding-sleutel bevatten. Deze mogelijkheid wordt aangeduid als **gegevensafhankelijke routering**.

![Shard-kaarten en gegevensafhankelijke routering][1]

De shard-toewijzing manager beveiligt gebruikers tegen inconsistente weergaven in shardlet gegevens die optreden kunnen wanneer de gelijktijdige shardlet bewerkingen plaatsvinden op de databases. Om dit te doen broker de shard-maps de databaseverbindingen voor een toepassing met de bibliotheek gemaakt. Als de shard-beheerbewerkingen kunnen invloed hebben op de shardlet, kan de functionaliteit van de kaart shard automatisch afsluiten van een databaseverbinding. 

In plaats van de traditionele manier om verbindingen voor Dapper te maken, moet u de [OpenConnectionForKey methode](http://msdn.microsoft.com/library/azure/dn824099.aspx). Dit zorgt ervoor dat de validatie plaats vindt en verbindingen correct worden beheerd als er gegevens worden verplaatst tussen shards.

### <a name="requirements-for-dapper-integration"></a>Vereisten voor Dapper-integratie
Als u werkt met de clientbibliotheek voor elastische database en de Dapper API's, die u wilt behouden van de volgende eigenschappen:

* **Uitschalen**: We wilt toevoegen of verwijderen van de databases uit de gegevenslaag van de shard-toepassing die nodig zijn voor de vereisten van de capaciteit van de toepassing. 
* **Consistentie**: omdat de toepassing is uitgebreid met behulp van sharding, u moet uitvoeren gegevensafhankelijke routering. We willen de gegevensafhankelijke routering mogelijkheden van de bibliotheek gebruiken om dit te doen. In het bijzonder u wilt behouden de validatie en consistentie wordt geleverd door verbindingen die worden geleverd door de shard-toewijzing manager om te voorkomen dat beschadiging of onjuiste queryresultaten. Dit zorgt ervoor dat de verbindingen met een bepaalde shardlet afgewezen of gestopt als (bijvoorbeeld) de shardlet momenteel wordt verplaatst naar een andere shard met gesplitste/Merge-API's.
* **Objecttoewijzing**: We willen het gemak van de toewijzingen geleverd door Dapper voor de omzetting tussen klassen in de toepassing en de onderliggende structuur van de database behouden. 

De volgende sectie bevat hulp voor deze vereisten voor toepassingen op basis van **Dapper** en **DapperExtensions**.

## <a name="technical-guidance"></a>Technische richtlijnen
### <a name="data-dependent-routing-with-dapper"></a>Gegevensafhankelijke routering met Dapper
De toepassing is met Dapper, gewoonlijk verantwoordelijk is voor het maken en openen van de verbindingen met de onderliggende database. Een type T gezien door de toepassing, retourneert Dapper queryresultaten zoals .NET verzamelingen van het type T. Dapper voert de toewijzing van de T-SQL-Resultatenrijen aan de objecten van het type T. Op deze manier maps Dapper .NET-objecten in SQL-waarden of de parameters voor instructies voor data manipulatie language (DML). Dapper biedt deze functionaliteit via uitbreidingsmethoden op de normale [SqlConnection](http://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.aspx) object uit de ADO .NET SQL-clientbibliotheken. De SQL-verbinding die wordt geretourneerd door de Elastic Scale API's voor DDR zijn ook reguliere [SqlConnection](http://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.aspx) objecten. Deze manier kunnen wij rechtstreeks Dapper om uitbreidingen te gebruiken via het type dat wordt geretourneerd door de clientbibliotheek DDR-API, omdat het ook een eenvoudige SQL-clientverbinding.

Deze opmerkingen kunnen u eenvoudig door de clientbibliotheek voor elastische databases voor Dapper brokered verbindingen te gebruiken.

Dit codevoorbeeld (van de bijbehorende voorbeeld) ziet u de methode waarbij de sharding-sleutel wordt geleverd door de toepassing aan de bibliotheek op de verbinding met de juiste shard broker.   

    using (SqlConnection sqlconn = shardingLayer.ShardMap.OpenConnectionForKey(
                     key: tenantId1, 
                     connectionString: connStrBldr.ConnectionString, 
                     options: ConnectionOptions.Validate))
    {
        var blog = new Blog { Name = name };
        sqlconn.Execute(@"
                      INSERT INTO
                            Blog (Name)
                            VALUES (@name)", new { name = blog.Name }
                        );
    }

De aanroep van de [OpenConnectionForKey](http://msdn.microsoft.com/library/azure/dn807226.aspx) API vervangt het standaard maken en openen van een SQL-clientverbinding. De [OpenConnectionForKey](http://msdn.microsoft.com/library/azure/dn807226.aspx) aanroep worden de argumenten die vereist voor het doorsturen van afhankelijk zijn van gegevens zijn: 

* De shard-toewijzing voor toegang tot de gegevens-afhankelijke routeringsinterfaces
* De sharding-sleutel voor het identificeren van de shardlet
* De referenties (gebruikersnaam en wachtwoord) verbinding maken met de shard

Het object shard-toewijzing maakt een verbinding met de shard die de shardlet voor de opgegeven sharding-sleutel bevat. De client-API's van elastische database ook de verbinding met het implementeren van de consistentie wordt gegarandeerd labelen. Sinds de aanroep [OpenConnectionForKey](http://msdn.microsoft.com/library/azure/dn807226.aspx) retourneert reguliere SQL object clientverbinding, de volgende aanroep naar de **Execute** uitbreidingsmethode van Dapper volgt de standaard Dapper praktijk.

Query's werken bijna hetzelfde - u opent eerst de verbinding met [OpenConnectionForKey](http://msdn.microsoft.com/library/azure/dn807226.aspx) van de client-API. U de gewone Dapper uitbreidingsmethoden de resultaten van uw SQL-query toewijzen aan .NET-objecten gebruiken:

    using (SqlConnection sqlconn = shardingLayer.ShardMap.OpenConnectionForKey(
                    key: tenantId1, 
                    connectionString: connStrBldr.ConnectionString, 
                    options: ConnectionOptions.Validate ))
    {    
           // Display all Blogs for tenant 1
           IEnumerable<Blog> result = sqlconn.Query<Blog>(@"
                                SELECT * 
                                FROM Blog
                                ORDER BY Name");

           Console.WriteLine("All blogs for tenant id {0}:", tenantId1);
           foreach (var item in result)
           {
                Console.WriteLine(item.Name);
            }
    }

Houd er rekening mee dat de **met** blokkeren met de DDR verbinding scopes alle databasebewerkingen in het blok op de ene shard waar tenantId1 wordt gehouden. De query retourneert alleen opgeslagen op de huidige shard-blogs, maar niet de bestanden die zijn opgeslagen op een andere shards. 

## <a name="data-dependent-routing-with-dapper-and-dapperextensions"></a>Gegevensafhankelijke routering met Dapper en DapperExtensions
Dapper wordt geleverd met een ecosysteem van de aanvullende extensies die verdere gemak en abstractie uit de database bieden kunnen bij het ontwikkelen van databasetoepassingen. DapperExtensions is een voorbeeld. 

DapperExtensions gebruiken in uw toepassing wordt niet gewijzigd hoe databaseverbindingen gemaakt en beheerd. Nog steeds de verantwoordelijkheid van de toepassing te openen van verbindingen en reguliere SQL Client connection-objecten door de uitbreidingsmethoden worden verwacht. We kunnen vertrouwen op de [OpenConnectionForKey](http://msdn.microsoft.com/library/azure/dn807226.aspx) hierboven. Zoals de volgende codevoorbeelden weergeven, is de enige wijziging dat u niet langer de T-SQL-instructies schrijven:

    using (SqlConnection sqlconn = shardingLayer.ShardMap.OpenConnectionForKey(
                    key: tenantId2, 
                    connectionString: connStrBldr.ConnectionString, 
                    options: ConnectionOptions.Validate))
    {
           var blog = new Blog { Name = name2 };
           sqlconn.Insert(blog);
    }

En dit is de voorbeeldcode voor de query: 

    using (SqlConnection sqlconn = shardingLayer.ShardMap.OpenConnectionForKey(
                    key: tenantId2, 
                    connectionString: connStrBldr.ConnectionString, 
                    options: ConnectionOptions.Validate))
    {
           // Display all Blogs for tenant 2
           IEnumerable<Blog> result = sqlconn.GetList<Blog>();
           Console.WriteLine("All blogs for tenant id {0}:", tenantId2);
           foreach (var item in result)
           {
               Console.WriteLine(item.Name);
           }
    }

### <a name="handling-transient-faults"></a>Afhandeling van tijdelijke fouten
Het team van Microsoft Patterns & procedures gepubliceerd de [tijdelijke fout afhandeling van Application Block](http://msdn.microsoft.com/library/hh680934.aspx) voor ontwikkelaars van toepassingen beperken algemene tijdelijke storingen opgetreden tijdens het uitvoeren in de cloud. Zie voor meer informatie [Perseverance, geheim van alle successen: met behulp van de tijdelijke fout afhandeling van Application Block](http://msdn.microsoft.com/library/dn440719.aspx).

In het voorbeeld is afhankelijk van de bibliotheek tijdelijke fout bij de bescherming tegen tijdelijke fouten. 

    SqlDatabaseUtils.SqlRetryPolicy.ExecuteAction(() =>
    {
       using (SqlConnection sqlconn = 
          shardingLayer.ShardMap.OpenConnectionForKey(tenantId2, connStrBldr.ConnectionString, ConnectionOptions.Validate))
          {
              var blog = new Blog { Name = name2 };
              sqlconn.Insert(blog);
          }
    });

**SqlDatabaseUtils.SqlRetryPolicy** in de bovenstaande code wordt gedefinieerd als een **SqlDatabaseTransientErrorDetectionStrategy** met een aantal nieuwe pogingen van 10 en 5 seconden wachttijd tussen nieuwe pogingen. Als u van transacties gebruikmaakt, ervoor zorgen dat uw bereik opnieuw teruggaan naar het begin van de transactie in het geval van een tijdelijke fout.

## <a name="limitations"></a>Beperkingen
De methoden die worden beschreven in dit document leidt tot een aantal beperkingen:

* De voorbeeldcode voor dit document biedt niet laten zien hoe schema beheren via shards.
* Een aanvraag opgegeven, gaan we ervan uit dat alle bijbehorende verwerken van de database zich in een enkel shard aangeduid met de sleutel sharding van de aanvraag. Echter, deze aanname niet altijd bijhoudt, bijvoorbeeld wanneer het is niet mogelijk om een sharding-sleutel beschikbaar te maken. Daarom de clientbibliotheek voor elastische database bevat de [MultiShardQuery klasse](http://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.multishardexception.aspx). De klasse implementeert een abstractie van de verbinding voor het uitvoeren van query's via meerdere shards. MultiShardQuery gebruiken in combinatie met Dapper valt buiten het bereik van dit document.

## <a name="conclusion"></a>Conclusie
Toepassingen met Dapper en DapperExtensions kunnen eenvoudig profiteren van de hulpmiddelen voor elastische databases voor Azure SQL Database. Via de stappen in dit document die toepassingen de mogelijkheid van het hulpprogramma kunnen gebruiken voor het doorsturen van gegevens afhankelijk zijn van het door het wijzigen van het maken en openen van nieuwe [SqlConnection](http://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.aspx) objecten te gebruiken de [ OpenConnectionForKey](http://msdn.microsoft.com/library/azure/dn807226.aspx) aanroep van de clientbibliotheek voor elastische database. Dit beperkt de wijzigingen in de toepassing vereist voor deze locaties waar de nieuwe verbindingen zijn gemaakt en geopend. 

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-scale-working-with-dapper/dapperimage1.png
