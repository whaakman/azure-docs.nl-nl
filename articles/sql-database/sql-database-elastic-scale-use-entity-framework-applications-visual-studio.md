---
title: Met behulp van elastische database-clientbibliotheek met Entity Framework | Microsoft Docs
description: Clientbibliotheek voor Elastic Database- en Entity Framework gebruiken voor het coderen van databases
services: sql-database
ms.service: sql-database
ms.subservice: elastic-scale
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: ''
manager: craigg
ms.date: 04/01/2018
ms.openlocfilehash: 695da176d2bc86fd67608cc28d14cf15a7728980
ms.sourcegitcommit: 51a1476c85ca518a6d8b4cc35aed7a76b33e130f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/25/2018
ms.locfileid: "47161485"
---
# <a name="elastic-database-client-library-with-entity-framework"></a>Elastische Database-clientbibliotheek met Entity Framework
Dit document bevat de wijzigingen in een Entity Framework-toepassing die nodig zijn om te integreren met de [hulpmiddelen voor Elastic Database](sql-database-elastic-scale-introduction.md). De focus ligt op het samenstellen van [shard-Toewijzingsbeheer](sql-database-elastic-scale-shard-map-management.md) en [gegevensafhankelijke routering](sql-database-elastic-scale-data-dependent-routing.md) met het Entity Framework **Code First** benadering. De [Code-: nieuwe Database](http://msdn.microsoft.com/data/jj193542.aspx) zelfstudie voor EF fungeert als de actieve voorbeeld in dit document. De voorbeeldcode bij dit document is onderdeel van de hulpmiddelen voor elastic database stellen van de voorbeelden in de Visual Studio-codevoorbeelden.

## <a name="downloading-and-running-the-sample-code"></a>Downloaden en uitvoeren van de voorbeeldcode
Voor het downloaden van de code voor dit artikel:

* Visual Studio 2012 of hoger is vereist. 
* Download de [elastische DB-hulpprogramma's voor Azure SQL - integratie met Entity Framework voorbeeld](https://code.msdn.microsoft.com/windowsapps/Elastic-Scale-with-Azure-bae904ba) van MSDN. Pak deze uit het voorbeeld naar een locatie van uw keuze.
* Start Visual Studio. 
* In Visual Studio, selecteer Bestand -> Open Project/oplossing. 
* In de **Open Project** dialoogvenster, navigeer naar het voorbeeld dat u hebt gedownload en selecteer **EntityFrameworkCodeFirst.sln** te openen in het voorbeeld. 

De als voorbeeld wilt uitvoeren, moet u drie lege databases maken in Azure SQL Database:

* Database voor shard-Toewijzingsbeheer
* Database voor shard 1
* Database voor shard 2

Als u deze databases hebt gemaakt, vult u de tijdelijke aanduidingen in **Program.cs** met de naam van uw Azure SQL-database-server, de databasenamen van de en uw referenties verbinding maken met de databases. Maak de oplossing in Visual Studio. Visual Studio downloadt de vereiste NuGet-pakketten voor de elastische database-clientbibliotheek, Entity Framework en tijdelijke fouten afhandelen als onderdeel van het bouwproces. Zorg ervoor dat het herstellen van NuGet-pakketten is ingeschakeld voor uw oplossing. U kunt deze instelling inschakelen met de rechtermuisknop op het oplossingsbestand in Visual Studio Solution Explorer. 

## <a name="entity-framework-workflows"></a>Entity Framework-werkstromen
Entity Framework ontwikkelaars afhankelijk zijn van een van de volgende vier werkstromen om toepassingen te bouwen en om ervoor te zorgen persistentie van de objecten voor toepassingen: 

* **Code First (nieuwe Database)**: EF de ontwikkelaar heeft het model gemaakt in de toepassingscode en EF genereert vervolgens de database uit. 
* **Code First (bestaande Database)**: de ontwikkelaar kunt EF de toepassingscode voor het model genereren vanuit een bestaande database.
* **Eerste model**: de ontwikkelaar maakt het model in de ontwerpfunctie voor EF en EF maakt vervolgens de database uit het model.
* **Eerste database**: EF hulpprogramma's voor het afleiden van het model van een bestaande database maakt gebruik van de ontwikkelaar. 

Alle deze methoden zijn afhankelijk van de DbContext-klasse voor het beheren van databaseverbindingen en database-schema voor een toepassing met transparant. Verschillende constructors op de basis DbContext-klasse kunnen verschillende niveaus van controle over verbinding maken, uitvoeren van de database bootstrap en schema's. Uitdagingen ontstaan voornamelijk uit het feit dat de database verbinding management via EF met de mogelijkheden voor verbinding van de gegevensafhankelijke routering interfaces opgegeven samen door de client-bibliotheek elastische database. 

## <a name="elastic-database-tools-assumptions"></a>Veronderstellingen voor elastische database-hulpprogramma 's
Zie voor definities van de termijn [woordenlijst voor hulpprogramma's elastische Database](sql-database-elastic-scale-glossary.md).

Met de clientbibliotheek voor elastic database definieert u partities van de gegevens van uw toepassing met de naam shardlets. Shardlets worden aangeduid met een sharding-sleutel en zijn toegewezen aan specifieke databases. Een toepassing kan zoveel databases als nodig hebt en distribueren van de shardlets voor voldoende capaciteit of prestaties gegeven van de huidige zakelijke vereisten. De toewijzing van sharding-sleutelwaarden op de databases wordt opgeslagen in een shard-toewijzing geleverd door de client-API's voor de elastische database. Deze mogelijkheid wordt genoemd **Shard-Toewijzingsbeheer**, of SMM kortweg. De shard-toewijzing fungeert ook als de broker van databaseverbindingen voor aanvragen die een sharding-sleutel bevatten. Deze mogelijkheid wordt ook wel **gegevensafhankelijke routering**. 

De shard-Toewijzingsbeheer beveiligt gebruikers tegen inconsistent weergaven in shardlet-gegevens die optreden kunnen wanneer gelijktijdige shardlet management-bewerkingen (zoals het verplaatsen van gegevens uit één shard naar een andere) plaatsvinden. Om dit te doen, beheerd de shard-toewijzingen door de client-bibliotheek broker de databaseverbindingen voor een toepassing. Hiermee wordt de functionaliteit van de kaart shard automatisch afsluiten van een databaseverbinding wanneer shard management-bewerkingen kunnen invloed hebben op de shardlet die voor de verbinding is gemaakt. Deze aanpak nodig heeft om te integreren met een aantal van de EF-functionaliteit, zoals het maken van nieuwe verbindingen van een bestaande om te controleren op aanwezigheid database. In het algemeen is onze waarneming dat de standaard alleen werken op betrouwbare wijze voor gesloten databaseverbindingen die veilig kunnen worden gekloond voor EF DbContext-constructors werken. In plaats daarvan is het ontwerpprincipe van elastische database alleen broker geopende verbindingen. Een denkt dat het sluiten van een verbinding met de clientbibliotheek voor overdracht naar de DbContext EF brokered dit probleem kan oplossen. Door de verbinding wordt gesloten en vertrouwen op EF te openen, foregoes een echter de validatie en consistentie controles uitgevoerd door de bibliotheek. De functionaliteit voor migraties in EF, gebruikt echter deze verbindingen voor het beheren van het schema van de onderliggende database op een manier die is transparant voor de toepassing. In het ideale geval u behouden en alle deze mogelijkheden van de clientbibliotheek voor elastic database en de EF in dezelfde toepassing combineren. De volgende sectie worden deze eigenschappen en de vereisten in meer detail beschreven. 

## <a name="requirements"></a>Vereisten
Als u werkt met de clientbibliotheek voor elastic database- en Entity Framework-API's, die u wilt behouden van de volgende eigenschappen: 

* **Scale-out**: toevoegen of verwijderen van databases van de gegevenslaag van de shard-toepassing die nodig zijn voor de behoeften van de capaciteit van de toepassing. Dit betekent dat de controle over het maken en verwijderen van databases en het gebruik van de elastische database shard-Toewijzingsbeheer API's voor het beheren van databases en -toewijzingen van shardlets. 
* **Consistentie**: de toepassing de veiligheidsmaatregelen voor sharding en maakt gebruik van de gegevensafhankelijke routering mogelijkheden van de clientbibliotheek. Om te voorkomen beschadigd of verkeerd queryresultaten, worden verbindingen geleverd door de shard-toewijzing. Dit houdt ook validatie en consistentie.
* **Code First**: het gemak van de EF code eerste paradigma behouden. Klassen in de toepassing worden in Code First transparant toegewezen aan de onderliggende databasestructuren. De code van de toepassing communiceert met DbSets die de meeste aspecten die betrokken zijn bij het verwerken van de onderliggende database maskeren.
* **Schema**: Entity Framework verwerkt de initiële database-schema wordt gemaakt en de ontwikkeling van de volgende schema via migraties. Met behoud van deze mogelijkheden, is aanpassing van uw app eenvoudig als de gegevens zich verder ontwikkelt. 

De volgende richtlijnen geïnstrueerd hoe om te voldoen aan deze vereisten voor Code First-toepassingen met hulpmiddelen voor elastic database. 

## <a name="data-dependent-routing-using-ef-dbcontext"></a>Gegevensafhankelijke routering EF DbContext gebruiken
Databaseverbindingen met Entity Framework worden meestal beheerd via subklassen van **DbContext**. Deze subklassen maken die is afgeleid van **DbContext**. Dit is waar u definieert uw **DbSets** die de verzamelingen database-ondersteuning met CLR-objecten voor uw toepassing implementeren. In de context van gegevensafhankelijke routering, kunt u verschillende nuttige eigenschappen die niet noodzakelijkerwijs voor andere eerste toepassingsscenario's EF code bewaart identificeren: 

* De database bestaat al en is geregistreerd in de shard-toewijzing voor elastische database. 
* Het schema van de toepassing is al geïmplementeerd voor de database (Zie hieronder). 
* Gegevensafhankelijke routering verbindingen met de database zijn brokered door de shard-toewijzing. 

Om te integreren **DbContexts** met gegevensafhankelijke routering voor scale-out:

1. Verbindingen van de fysieke database via de elastic database client-interfaces van de shard-Toewijzingsbeheer maken 
2. Teruglopen van de verbinding met de **DbContext** subklasse
3. De verbinding naar beneden doorgeven aan de **DbContext** klassen om te controleren of de verwerking aan de EF gebeurt ook baseren. 

Het volgende codevoorbeeld ziet u deze aanpak. (Deze code is ook in de bijbehorende Visual Studio-project)

    public class ElasticScaleContext<T> : DbContext
    {
    public DbSet<Blog> Blogs { get; set; }
    …

        // C'tor for data-dependent routing. This call opens a validated connection 
        // routed to the proper shard by the shard map manager. 
        // Note that the base class c'tor call fails for an open connection
        // if migrations need to be done and SQL credentials are used. This is the reason for the 
        // separation of c'tors into the data-dependent routing case (this c'tor) and the internal c'tor for new shards.
        public ElasticScaleContext(ShardMap shardMap, T shardingKey, string connectionStr)
            : base(CreateDDRConnection(shardMap, shardingKey, connectionStr), 
            true /* contextOwnsConnection */)
        {
        }

        // Only static methods are allowed in calls into base class c'tors.
        private static DbConnection CreateDDRConnection(
        ShardMap shardMap, 
        T shardingKey, 
        string connectionStr)
        {
            // No initialization
            Database.SetInitializer<ElasticScaleContext<T>>(null);

            // Ask shard map to broker a validated connection for the given key
            SqlConnection conn = shardMap.OpenConnectionForKey<T>
                                (shardingKey, connectionStr, ConnectionOptions.Validate);
            return conn;
        }    

## <a name="main-points"></a>Belangrijkste punten
* Een nieuwe constructor vervangt de standaardconstructor in de subklasse DbContext 
* De nieuwe constructor neemt de argumenten die vereist zijn voor de gegevensafhankelijke routering via de clientbibliotheek voor elastic database:
  
  * de shard-toewijzing voor toegang tot de interfaces gegevensafhankelijke routering
  * de sharding-sleutel voor het identificeren van de shardlet
  * een verbindingsreeks met de referenties voor de gegevensafhankelijke routering verbinding met de shard. 
* De aanroep naar de basisklassenconstructor duurt een detour in een statische methode waarmee u alle stappen die nodig zijn voor gegevensafhankelijke routering. 
  
  * Hierbij de OpenConnectionForKey aanroep van de client-interfaces voor elastische database in de shard-toewijzing een open verbinding tot stand brengen.
  * De shard-toewijzing wordt gemaakt van de open verbinding met de shard die de shardlet voor de opgegeven sharding-sleutel bevat.
  * Deze open verbinding wordt doorgegeven aan de constructor van de basisklasse van DbContext om aan te geven dat deze verbinding moet worden gebruikt door EF in plaats van laten EF automatisch een nieuwe verbinding maken. Op deze manier de verbinding is gemarkeerd door de databaseclient-API van elastic, zodat deze consistentie onder beheerbewerkingen voor shard-toewijzing kunt garanderen.

Gebruik de nieuwe constructor voor uw subklasse DbContext in plaats van de standaardconstructor in uw code. Hier volgt een voorbeeld: 

    // Create and save a new blog.

    Console.Write("Enter a name for a new blog: "); 
    var name = Console.ReadLine(); 

    using (var db = new ElasticScaleContext<int>( 
                            sharding.ShardMap,  
                            tenantId1,  
                            connStrBldr.ConnectionString)) 
    { 
        var blog = new Blog { Name = name }; 
        db.Blogs.Add(blog); 
        db.SaveChanges(); 

        // Display all Blogs for tenant 1 
        var query = from b in db.Blogs 
                    orderby b.Name 
                    select b; 
     … 
    }

De nieuwe constructor opent de verbinding met de shard waarin de gegevens voor de shardlet geïdentificeerd door de waarde van **tenantid1**. De code in de **met behulp van** blok blijft ongewijzigd voor toegang tot de **DbSet** voor blogs EF met in de shard voor **tenantid1**. Hiermee wordt semantiek gewijzigd voor de code in het gebruik blokkeren zodanig dat alle databasebewerkingen nu zijn gericht op één shard waar **tenantid1** wordt bewaard. Bijvoorbeeld, een LINQ-query op de blogs **DbSet** retourneerde alleen opgeslagen op de huidige shard-blogs, maar niet de bestanden die zijn opgeslagen op andere shards.  

#### <a name="transient-faults-handling"></a>Tijdelijke fouten afhandelen
Het team Microsoft Patterns & Practices gepubliceerd de [de afhandeling van Toepassingsblok fouten](https://msdn.microsoft.com/library/dn440719.aspx). De bibliotheek wordt met elastische schaal-clientbibliotheek in combinatie met EF gebruikt. Echter voor zorgen dat er een tijdelijke uitzondering op een plaats waar u ervoor zorgen retourneert kunt dat de nieuwe constructor wordt gebruikt nadat een tijdelijke fout zodat elke nieuwe verbindingspoging is gemaakt met behulp van de constructors die u toegepast. Anders wordt een verbinding met de juiste shard kan niet worden gegarandeerd, en er zijn geen garanties die wordt bijgehouden als er wijzigingen aan de shard-toewijzing optreden. 

Het volgende codevoorbeeld laat zien hoe een SQL-beleid voor opnieuw proberen kan worden gebruikt om de nieuwe **DbContext** subklasse constructors: 

    SqlDatabaseUtils.SqlRetryPolicy.ExecuteAction(() => 
    { 
        using (var db = new ElasticScaleContext<int>( 
                                sharding.ShardMap,  
                                tenantId1,  
                                connStrBldr.ConnectionString)) 
            { 
                    var blog = new Blog { Name = name }; 
                    db.Blogs.Add(blog); 
                    db.SaveChanges(); 
            … 
            } 
        }); 

**SqlDatabaseUtils.SqlRetryPolicy** in de bovenstaande code wordt gedefinieerd als een **SqlDatabaseTransientErrorDetectionStrategy** met een aantal nieuwe pogingen van 10, en op 5 seconden wachttijd tussen nieuwe pogingen. Deze aanpak is vergelijkbaar met de richtlijnen voor EF en de gebruiker geïnitieerde transacties (Zie [beperkingen bij uitvoeringsstrategieën voor opnieuw proberen (EF6 en hoger)](http://msdn.microsoft.com/data/dn307226). Beide situaties is vereist dat de toepassing Hiermee bepaalt u het bereik waarvoor de tijdelijke uitzondering geretourneerd: aan de transactie opnieuw of maak opnieuw de context van de juiste constructor (zoals) die gebruikmaakt van de clientbibliotheek van elastische database.

De noodzaak om te bepalen waar tijdelijke uitzonderingen worden we weer binnen het bereik ook het gebruik van de ingebouwde uitsluit **SqlAzureExecutionStrategy** die wordt geleverd met EF. **SqlAzureExecutionStrategy** zou opnieuw een verbinding openen, maar niet **OpenConnectionForKey** en daarom bypass alle validatietests die wordt uitgevoerd als onderdeel van de **OpenConnectionForKey**aanroepen. In plaats daarvan de voorbeeldcode maakt gebruik van de ingebouwde **DefaultExecutionStrategy** die wordt geleverd met EF. Plaats **SqlAzureExecutionStrategy**, deze correct werkt in combinatie met het beleid voor opnieuw proberen van de afhandeling van tijdelijke fouten. Het uitvoeringsbeleid is ingesteld in de **ElasticScaleDbConfiguration** klasse. Houd er rekening mee dat we niet te gebruiken besloten **DefaultSqlExecutionStrategy** omdat dit kan erop met behulp van wijzen **SqlAzureExecutionStrategy** als er tijdelijke uitzonderingen optreden - wat zou leiden tot onjuist gedrag, zoals beschreven. Zie voor meer informatie over de verschillende retry-beleid en de EF [Verbindingstolerantie in EF](http://msdn.microsoft.com/data/dn456835.aspx).     

#### <a name="constructor-rewrites"></a>Constructor regeneraties
De bovenstaande codevoorbeelden ziet u de standaard-constructor herschrijft vereist zijn voor uw toepassing om te kunnen gebruiken gegevensafhankelijke routering met het Entity Framework. De volgende tabel generaliseert deze aanpak voor het andere constructors. 

| Huidige Constructor | Herschreven Constructor voor gegevens | Basis-Constructor | Opmerkingen |
| --- | --- | --- | --- |
| MyContext() |ElasticScaleContext (ShardMap, TKey) |DbContext (DbConnection, bool) |De verbinding moet een functie van de shard-toewijzing en de gegevensafhankelijke routering sleutel. U moet omzeilen automatisch verbinding maken met EF en in plaats daarvan de shard-toewijzing te gebruiken als Broker optreden voor de verbinding. |
| MyContext(string) |ElasticScaleContext (ShardMap, TKey) |DbContext (DbConnection, bool) |De verbinding is een functie van de shard-toewijzing en de gegevensafhankelijke routering sleutel. Een vaste databaserol naam of de verbindingsreeks werkt niet als ze validatie omzeilen door de shard-toewijzing. |
| MyContext(DbCompiledModel) |ElasticScaleContext (ShardMap, TKey, DbCompiledModel) |DbContext (DbConnection, DbCompiledModel, bool) |De verbinding wordt gemaakt voor de opgegeven shard-toewijzing en sharding-sleutel met het model dat is opgegeven. Het gecompileerde model wordt doorgegeven aan de basis c'tor. |
| MyContext (DbConnection, bool) |ElasticScaleContext (ShardMap, TKey, bool) |DbContext (DbConnection, bool) |De verbinding moet worden afgeleid van de shard-toewijzing en de sleutel. Het kan niet worden opgegeven als invoer (tenzij deze invoer is al met behulp van de shard-toewijzing en de sleutel). De Booleaanse waarde wordt doorgegeven. |
| MyContext (string, DbCompiledModel) |ElasticScaleContext (ShardMap, TKey, DbCompiledModel) |DbContext (DbConnection, DbCompiledModel, bool) |De verbinding moet worden afgeleid van de shard-toewijzing en de sleutel. Het kan niet worden opgegeven als invoer (tenzij deze invoer is met behulp van de shard-toewijzing en de sleutel). Het gecompileerde model wordt doorgegeven. |
| MyContext (ObjectContext, bool) |ElasticScaleContext (ShardMap, TKey, ObjectContext, bool) |DbContext (ObjectContext, bool) |De nieuwe constructor nodig om ervoor te zorgen dat elke verbinding in de ObjectContext doorgegeven als invoer omgeleid naar een verbinding die wordt beheerd door elastisch schalen wordt. Een gedetailleerde bespreking van ObjectContexts valt buiten het bereik van dit document. |
| MyContext (DbConnection, DbCompiledModel, bool) |ElasticScaleContext (ShardMap, TKey, DbCompiledModel, bool) |DbContext (DbConnection, DbCompiledModel, bool); |De verbinding moet worden afgeleid van de shard-toewijzing en de sleutel. De verbinding kan niet worden opgegeven als invoer (tenzij deze invoer is al met behulp van de shard-toewijzing en de sleutel). Model en Booleaanse waarde zijn doorgegeven aan de constructor basisklasse. |

## <a name="shard-schema-deployment-through-ef-migrations"></a>Shard-schema-implementatie via EF-migraties
Automatische Schemabeheer is voor uw gemak geleverd door de Entity Framework. In de context van toepassingen die gebruikmaken van hulpprogramma's voor elastische databases, die u wilt bewaren deze mogelijkheid voor het automatisch inrichten van het schema naar de zojuist gemaakte shards wanneer databases worden toegevoegd aan de shard-toepassing. De primaire use-case is om de capaciteit in de gegevenslaag voor shard-toepassingen met EF te vergroten. Afhankelijk van de EF-mogelijkheden voor schemabeheer, vermindert u de database-beheer zonder veel moeite met een shard-toepassing die is gebouwd op EF. 

Schema-implementatie via EF-migraties werkt het beste op **nog niet gelezen verbindingen**. Dit is in tegenstelling tot het scenario voor het gegevensafhankelijke routering die is gebaseerd op de geopende verbinding geleverd door de databaseclient-API van elastic. Een ander verschil is de vereiste consistentie: tijdens het wenselijk om ervoor te zorgen consistent voor alle gegevensafhankelijke routering verbindingen om te beveiligen tegen gelijktijdige shard-kaart manipuleren, het is niet een probleem met de initiële distributie van het schema aan een nieuwe database heeft nog niet zijn geregistreerd in de shard-toewijzing en nog niet is toegewezen aan shardlets bevatten. Daarom kunt u vertrouwen op reguliere databaseverbindingen voor dit scenario, in plaats van gegevensafhankelijke routering.  

Dit leidt tot een benadering waarbij schema-implementatie via EF-migraties is nauw gekoppeld aan de registratie van de nieuwe database als een shard in de shard-toewijzing van de toepassing. Dit is afhankelijk van de volgende vereisten: 

* De database is al gemaakt. 
* De database is leeg: deze bevat geen gebruikersschema en geen gebruikersgegevens.
* De database kan niet nog worden geopend via de client-API's voor de elastische databases voor gegevensafhankelijke routering. 

Met deze voorwaarden is voldaan, kunt u een regelmatige niet geopend **SqlConnection** naar een vliegende start EF-migraties voor distributie van het schema. Het volgende codevoorbeeld ziet u deze aanpak. 

        // Enter a new shard - i.e. an empty database - to the shard map, allocate a first tenant to it  
        // and kick off EF intialization of the database to deploy schema 

        public void RegisterNewShard(string server, string database, string connStr, int key) 
        { 

            Shard shard = this.ShardMap.CreateShard(new ShardLocation(server, database)); 

            SqlConnectionStringBuilder connStrBldr = new SqlConnectionStringBuilder(connStr); 
            connStrBldr.DataSource = server; 
            connStrBldr.InitialCatalog = database; 

            // Go into a DbContext to trigger migrations and schema deployment for the new shard. 
            // This requires an un-opened connection. 
            using (var db = new ElasticScaleContext<int>(connStrBldr.ConnectionString)) 
            { 
                // Run a query to engage EF migrations 
                (from b in db.Blogs 
                    select b).Count(); 
            } 

            // Register the mapping of the tenant to the shard in the shard map. 
            // After this step, data-dependent routing on the shard map can be used 

            this.ShardMap.CreatePointMapping(key, shard); 
        } 


In dit voorbeeld toont de methode **RegisterNewShard** die de shard registreert in de shard-toewijzing, implementeert u het schema via EF-migraties en een toewijzing van een sharding-sleutel voor de shard wordt opgeslagen. Er wordt gebruikgemaakt van een constructor met de **DbContext** subklasse (**ElasticScaleContext** in het voorbeeld) waarmee een SQL-verbindingsreeks als invoer. De code van deze constructor is eenvoudig, zoals in het volgende voorbeeld wordt weergegeven: 

        // C'tor to deploy schema and migrations to a new shard 
        protected internal ElasticScaleContext(string connectionString) 
            : base(SetInitializerForConnection(connectionString)) 
        { 
        } 

        // Only static methods are allowed in calls into base class c'tors 
        private static string SetInitializerForConnection(string connnectionString) 
        { 
            // You want existence checks so that the schema can get deployed 
            Database.SetInitializer<ElasticScaleContext<T>>( 
        new CreateDatabaseIfNotExists<ElasticScaleContext<T>>()); 

            return connnectionString; 
        } 

Een mogelijk gebruikt de versie van de constructor overgenomen van de basisklasse. Maar de code nodig om ervoor te zorgen dat de standaard-initialisatiefunctie voor EF wordt gebruikt wanneer verbinding wordt gemaakt. De korte detour daarom in de statische methode voordat u aan de constructor basisklasse door de verbindingsreeks. Houd er rekening mee dat de registratie van shards moet worden uitgevoerd in een ander toepassingsdomein of om ervoor te zorgen dat de initialisatiefunctie-instellingen voor EF geen conflict veroorzaken. 

## <a name="limitations"></a>Beperkingen
De methoden die worden beschreven in dit document leidt tot een aantal beperkingen: 

* EF-toepassingen die gebruikmaken van **LocalDb** moet u eerst om te migreren naar een normale SQL Server-database voordat u met behulp van de clientbibliotheek voor elastic database. Een toepassing via sharding uitbreiden met Elastic Scale is niet mogelijk is met **LocalDb**. Houd er rekening mee dat er nog steeds ontwikkeling kunt gebruiken **LocalDb**. 
* Eventuele wijzigingen in de toepassing die schemawijzigingen database impliceren moeten gaan via EF-migraties op alle shards. De voorbeeldcode voor dit document wordt niet laten zien hoe u dit doet. Overweeg het gebruik van de Database bijwerken met een parameter ConnectionString om te herhalen alle shards; of ophalen van de T-SQL-script voor de in behandeling migratie met behulp van de Database bijwerken met een Script - optie en de T-SQL-script toepassen op uw shards.  
* Een aanvraag opgegeven, wordt ervan uitgegaan dat alle van de verwerking van de database zich in een enkele shard aangeduid met de sharding-sleutel die is opgegeven door de aanvraag. Echter deze aanname niet altijd over de waarde true. Bijvoorbeeld, wanneer deze is niet mogelijk om een sharding-sleutel beschikbaar te maken. Om dit op te lossen, de clientbibliotheek biedt de **MultiShardQuery** klasse die een abstractie van de verbinding voor het uitvoeren van query's over verschillende shards. Leren gebruiken de **MultiShardQuery** in combinatie met EF valt buiten het bereik van dit document

## <a name="conclusion"></a>Conclusie
Via de stappen die worden beschreven in dit document, EF-toepassingen de mogelijkheid de elastische database-clientbibliotheek kunnen gebruiken voor het gegevensafhankelijke routering door herstructurering constructors van de **DbContext** subklassen gebruikt in de EF-toepassing. Dit beperkt de wijzigingen die zijn vereist voor deze locaties waar **DbContext** klassen bestaan al. Bovendien kunnen EF-toepassingen blijven profiteren van het schema voor automatische implementatie door een combinatie van de stappen waarmee u de benodigde EF-migraties met de registratie van de nieuwe shards en toewijzingen in de shard-toewijzing kunt aanroepen. 

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-scale-use-entity-framework-applications-visual-studio/sample.png
