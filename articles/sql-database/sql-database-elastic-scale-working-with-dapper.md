---
title: Clientbibliotheek voor elastic database gebruiken met Dapper | Microsoft Docs
description: Clientbibliotheek voor elastic database gebruiken met Dapper.
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
ms.date: 12/04/2018
ms.openlocfilehash: 8de155eb0c53a07c88d996e2545be9da3159653f
ms.sourcegitcommit: ba035bfe9fab85dd1e6134a98af1ad7cf6891033
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/01/2019
ms.locfileid: "55565578"
---
# <a name="using-elastic-database-client-library-with-dapper"></a>Clientbibliotheek voor elastic database gebruiken met Dapper
Dit document is bedoeld voor ontwikkelaars die afhankelijk zijn van Dapper om toepassingen te bouwen, maar ook wilt Profiteer [elastische database tooling](sql-database-elastic-scale-introduction.md) om toepassingen te maken die sharding implementeren om uit de gegevenslaag te schalen.  Dit document ziet u de wijzigingen in Dapper gebaseerde toepassingen die nodig zijn om te integreren met hulpmiddelen voor elastic database. Onze focus ligt op het samenstellen van de elastische database shard management en gegevensafhankelijke routering met Dapper. 

**Voorbeeldcode**: [Hulpmiddelen voor elastic database voor Azure SQL Database - Dapper integratie](https://code.msdn.microsoft.com/Elastic-Scale-with-Azure-e19fc77f).

Integratie van **Dapper** en **DapperExtensions** met het elastic database-clientbibliotheek voor Azure SQL Database is eenvoudig. Uw toepassingen kunnen gebruikmaken van gegevensafhankelijke routering door het wijzigen van het maken en openen van nieuwe [SqlConnection](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.aspx) objecten die u wilt gebruiken de [OpenConnectionForKey](https://msdn.microsoft.com/library/azure/dn807226.aspx) aanroepen vanuit de [-clientbibliotheek ](https://msdn.microsoft.com/library/azure/dn765902.aspx). Dit beperkt de wijzigingen in uw toepassing alleen wanneer nieuwe verbindingen worden gemaakt en geopend. 

## <a name="dapper-overview"></a>Overzicht van dapper
**Dapper** is een object-relationele mapper. Het .NET-objecten van uw toepassing in een relationele database (en omgekeerd) wordt toegewezen. Het eerste deel van de voorbeeldcode laat zien hoe u de elastische database-clientbibliotheek kunt integreren met Dapper gebaseerde toepassingen. Het tweede gedeelte van de voorbeeldcode ziet u hoe u bij het gebruik van zowel Dapper en DapperExtensions integreren.  

De mapper-functionaliteit in Dapper biedt uitbreidingsmethoden voor databaseverbindingen dat verzenden T-SQL-instructies voor de uitvoering en het opvragen van de database te vereenvoudigen. Bijvoorbeeld, Dapper kunt u eenvoudig om een koppeling tussen uw .NET-objecten en de parameters van de SQL-instructies voor **Execute** -aanroepen, of de resultaten van uw SQL-query's gebruiken in .NET-objecten met behulp van **Query** aanroepen van Dapper. 

Wanneer u DapperExtensions, moet u niet meer voor de SQL-instructies. Extensies-methoden zoals **GetList** of **invoegen** via verbinding met de database maken van de SQL-instructies achter de schermen.

Een ander voordeel van Dapper en ook DapperExtensions is dat de toepassing het maken van verbinding met de database bepaalt. Dit helpt te communiceren met de clientbibliotheek van elastische database die brokers op basis van de toewijzing van shardlets met databases databaseverbindingen.

Als u de Dapper assembly's, Zie [Dapper dot net](http://www.nuget.org/packages/Dapper/). Zie voor de extensies Dapper [DapperExtensions](http://www.nuget.org/packages/DapperExtensions).

## <a name="a-quick-look-at-the-elastic-database-client-library"></a>Een beknopt overzicht van de clientbibliotheek van elastische database
Met de clientbibliotheek van elastische database, definieert u de partities van de gegevens van uw toepassing met de naam *shardlets*, toe te wijzen aan databases en herkennen door *sharding sleutels*. U kunt zoveel databases als u uw shardlets verdelen over deze databases nodig hebben. De toewijzing van sharding-sleutelwaarden op de databases is door een shard-toewijzing geleverd door de API's van de bibliotheek opgeslagen. Deze mogelijkheid wordt genoemd **shard-Toewijzingsbeheer**. De shard-toewijzing fungeert ook als de broker van databaseverbindingen voor aanvragen die een sharding-sleutel bevatten. Deze mogelijkheid wordt aangeduid als **gegevensafhankelijke routering**.

![Shard-toewijzingen en gegevensafhankelijke routering][1]

De shard-Toewijzingsbeheer beveiligt gebruikers tegen inconsistent weergaven in shardlet-gegevens die optreden kunnen wanneer gelijktijdige shardlet management-bewerkingen op de databases plaatsvinden. De shard-toewijzingen broker om dit te doen, de databaseverbindingen voor een toepassing die zijn gebouwd met de bibliotheek. Wanneer u beheerbewerkingen voor shard kunnen van invloed zijn op de shardlet, Hiermee wordt de functionaliteit van de shard-kaart automatisch afsluiten van de verbinding met een database. 

In plaats van de traditionele manier om te maken van verbindingen voor Dapper, die u wilt gebruiken de [OpenConnectionForKey methode](https://msdn.microsoft.com/library/azure/dn824099.aspx). Dit zorgt ervoor dat alle validatietests plaatsvindt en verbindingen goed worden beheerd wanneer alle gegevens worden verplaatst tussen shards.

### <a name="requirements-for-dapper-integration"></a>Vereisten voor integratie met Dapper
Als u werkt met zowel de elastische database-clientbibliotheek en Dapper-API's, die u wilt behouden van de volgende eigenschappen:

* **Uitschalen**: We willen toevoegen of verwijderen van databases uit de gegevenslaag van de shard-toepassing die nodig zijn voor de behoeften van de capaciteit van de toepassing. 
* **Consistentie**: Omdat de toepassing geschaald met behulp van sharding, moet u uitvoeren gegevensafhankelijke routering. We willen de gegevensafhankelijke routering mogelijkheden van de bibliotheek gebruiken om dit te doen. In het bijzonder, u wilt behouden van de validatie en consistentie gegarandeerd geleverd door de verbindingen die worden geleverd door de shard-Toewijzingsbeheer om te voorkomen, beschadigd of verkeerd queryresultaten. Dit zorgt ervoor dat de verbindingen met een bepaalde shardlet geweigerd of gestopt als (bijvoorbeeld) de shardlet op dit moment wordt verplaatst naar een andere shard met behulp van splitsen en samenvoegen/API's.
* **Mapping-object**: We willen het gemak van de toewijzingen geleverd door Dapper voor de omzetting tussen klassen in de toepassing en de onderliggende databasestructuren behouden. 

De volgende sectie bevat richtlijnen voor deze vereisten voor toepassingen op basis van **Dapper** en **DapperExtensions**.

## <a name="technical-guidance"></a>Technische richtlijnen
### <a name="data-dependent-routing-with-dapper"></a>Gegevensafhankelijke routering met Dapper
De toepassing is met Dapper, gewoonlijk verantwoordelijk is voor het maken en openen van de verbindingen met de onderliggende database. Basis van een type T door de toepassing, retourneert Dapper de resultaten van query als .NET-collecties van het type T. Dapper voert de toewijzing van de T-SQL-Resultatenrijen aan de objecten van het type T. Op deze manier toegewezen Dapper .NET-objecten in de SQL-waarden of parameters op gegevens manipuleren taal (DML)-instructies. Dapper biedt deze functionaliteit via uitbreidingsmethoden op de normale [SqlConnection](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.aspx) object op basis van de ADO .NET SQL-clientbibliotheken. De SQL-verbinding die is geretourneerd door de Elastic Scale API's voor DDR zijn ook reguliere [SqlConnection](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.aspx) objecten. Dit kan we rechtstreeks Dapper-extensies gebruiken via het type dat wordt geretourneerd door de clientbibliotheek DDR-API, omdat het ook een eenvoudige SQL-clientverbinding.

Deze opmerkingen maken het eenvoudig door de elastische database-clientbibliotheek voor Dapper brokered verbindingen te gebruiken.

Dit codevoorbeeld (van het bijbehorende voorbeeld) ziet u de aanpak waarbij de sharding-sleutel wordt geleverd door de toepassing aan de bibliotheek als Broker optreden voor de verbinding met de juiste shard.   

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

De aanroep van de [OpenConnectionForKey](https://msdn.microsoft.com/library/azure/dn807226.aspx) API vervangt het standaard maken en openen van een SQL-clientverbinding. De [OpenConnectionForKey](https://msdn.microsoft.com/library/azure/dn807226.aspx) aanroep neemt de argumenten die vereist voor gegevensafhankelijke routering zijn: 

* Interfaces van de shard-toewijzing voor toegang tot de gegevensafhankelijke routering
* De sharding-sleutel voor het identificeren van de shardlet
* De referenties (gebruikersnaam en wachtwoord) verbinding maken met de shard

Het object shard-kaart maakt een verbinding met de shard die de shardlet voor de opgegeven sharding-sleutel bevat. De client-API's voor de elastische database worden ook de verbinding met het implementeren van de garanties voor consistentie labelen. Sinds de aanroep [OpenConnectionForKey](https://msdn.microsoft.com/library/azure/dn807226.aspx) resulteert in een normale SQL-Client verbindingsobject, de volgende aanroep naar de **Execute** uitbreidingsmethode van Dapper volgt de Dapper standaardprocedure.

Query's bijna dezelfde manier werken - opent u eerst de verbinding met [OpenConnectionForKey](https://msdn.microsoft.com/library/azure/dn807226.aspx) van de client-API. Vervolgens u de gewone Dapper uitbreidingsmethoden gebruikt om toe te wijzen de resultaten van uw SQL-query naar .NET-objecten:

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

Houd er rekening mee dat de **met behulp van** blokkeren met de DDR verbinding bereiken alle databasebewerkingen in het blok op de ene shard waarop tenantId1 wordt bewaard. De query retourneert alleen opgeslagen op de huidige shard-blogs, maar niet de bestanden die zijn opgeslagen op een andere shards. 

## <a name="data-dependent-routing-with-dapper-and-dapperextensions"></a>Gegevensafhankelijke routering met Dapper en DapperExtensions
Dapper wordt geleverd met een ecosysteem van de aanvullende extensies die verdere gebruiksgemak en de abstractie van de database bieden kan bij het ontwikkelen van databasetoepassingen. DapperExtensions is een voorbeeld. 

DapperExtensions gebruiken in uw toepassing verandert niet hoe databaseverbindingen worden gemaakt en beheerd. Het is nog steeds de verantwoordelijkheid van de toepassing verbindingen openen en normale SQL-Client verbindingsobjecten zijn door de uitbreidingsmethoden wordt verwacht. We kunnen afhankelijk zijn van de [OpenConnectionForKey](https://msdn.microsoft.com/library/azure/dn807226.aspx) als hierboven beschreven. Zoals de volgende codevoorbeelden laten zien, is de enige wijziging die u niet meer hebt voor de T-SQL-instructies schrijven:

    using (SqlConnection sqlconn = shardingLayer.ShardMap.OpenConnectionForKey(
                    key: tenantId2, 
                    connectionString: connStrBldr.ConnectionString, 
                    options: ConnectionOptions.Validate))
    {
           var blog = new Blog { Name = name2 };
           sqlconn.Insert(blog);
    }

En het codevoorbeeld voor de query als volgt: 

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
Het team Microsoft Patterns & Practices gepubliceerd de [verwerken Toepassingsblok fouten](https://msdn.microsoft.com/library/hh680934.aspx) om toepassingsontwikkelaars die verhelpen algemene tijdelijke storingen opgetreden bij het uitvoeren van in de cloud te helpen. Zie voor meer informatie, [Perseverance, geheim van alle successen: Met behulp van het Toepassingsblok voor afhandeling van tijdelijke fouten](https://msdn.microsoft.com/library/dn440719.aspx).

De voorbeeldcode is afhankelijk van de bibliotheek tijdelijke fouten om te beveiligen tegen tijdelijke fouten. 

    SqlDatabaseUtils.SqlRetryPolicy.ExecuteAction(() =>
    {
       using (SqlConnection sqlconn = 
          shardingLayer.ShardMap.OpenConnectionForKey(tenantId2, connStrBldr.ConnectionString, ConnectionOptions.Validate))
          {
              var blog = new Blog { Name = name2 };
              sqlconn.Insert(blog);
          }
    });

**SqlDatabaseUtils.SqlRetryPolicy** in de bovenstaande code wordt gedefinieerd als een **SqlDatabaseTransientErrorDetectionStrategy** met een aantal nieuwe pogingen van 10, en op 5 seconden wachttijd tussen nieuwe pogingen. Als u van transacties gebruikmaakt, controleert u uw bereik voor nieuwe pogingen gaat terug naar het begin van de transactie in het geval van een tijdelijke fout.

## <a name="limitations"></a>Beperkingen
De methoden die worden beschreven in dit document leidt tot een aantal beperkingen:

* De voorbeeldcode voor dit document wordt niet laten zien hoe u het schema beheren in verschillende shards.
* Een aanvraag opgegeven, gaan we ervan uit dat alle de verwerking van de database zich in een enkele shard aangeduid met de sharding-sleutel die is opgegeven door de aanvraag. Maar bevat deze aanname geen altijd, bijvoorbeeld wanneer het is niet mogelijk om een sharding-sleutel beschikbaar te maken. Om dit op te lossen, de clientbibliotheek van elastische database bevat de [MultiShardQuery klasse](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.multishardexception.aspx). De klasse implementeert een abstractie van de verbinding voor het uitvoeren van query's over verschillende shards. MultiShardQuery gebruiken in combinatie met Dapper valt buiten het bereik van dit document.

## <a name="conclusion"></a>Conclusie
Toepassingen met Dapper en DapperExtensions kunnen eenvoudig profiteren van de hulpprogramma's voor elastische databases voor Azure SQL Database. Via de stappen die worden beschreven in dit document, die toepassingen de mogelijkheid van het hulpprogramma kunnen gebruiken voor het gegevensafhankelijke routering door het wijzigen van het maken en openen van nieuwe [SqlConnection](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.aspx) objecten die u wilt gebruiken de [ OpenConnectionForKey](https://msdn.microsoft.com/library/azure/dn807226.aspx) aanroep van de clientbibliotheek van elastische database. Dit beperkt de wijzigingen in de toepassing vereist voor deze locaties waar nieuwe verbindingen worden gemaakt en geopend. 

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-scale-working-with-dapper/dapperimage1.png
