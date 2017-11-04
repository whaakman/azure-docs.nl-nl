---
title: Met behulp van de clientbibliotheek voor elastische database met Entity Framework | Microsoft Docs
description: Gebruik van de clientbibliotheek voor elastische Database en Entity Framework voor het coderen van databases
services: sql-database
documentationcenter: 
manager: jhubbard
author: torsteng
editor: 
ms.assetid: b9c3065b-cb92-41be-aa7f-deba23e7e159
ms.service: sql-database
ms.custom: scale out apps
ms.workload: Inactive
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/06/2017
ms.author: torsteng
ms.openlocfilehash: 1fc61657419f1f4581c5c67639d7bc2e4b0d509f
ms.sourcegitcommit: dfd49613fce4ce917e844d205c85359ff093bb9c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/31/2017
---
# <a name="elastic-database-client-library-with-entity-framework"></a>Elastische Database-clientbibliotheek met Entity Framework
Dit document bevat de wijzigingen in een Entity Framework-toepassing die nodig zijn om te integreren met de [hulpmiddelen voor elastische databases](sql-database-elastic-scale-introduction.md). De focus is het samenstellen van [shard kaart management](sql-database-elastic-scale-shard-map-management.md) en [gegevensafhankelijke routering](sql-database-elastic-scale-data-dependent-routing.md) met de Entity Framework **Code First** benadering. De [Code eerst - nieuwe Database](http://msdn.microsoft.com/data/jj193542.aspx) zelfstudie voor EF fungeert als het actieve voorbeeld in dit hele document. De voorbeeldcode die bij dit document is onderdeel van de hulpmiddelen voor elastische databases ingesteld van de voorbeelden in de Visual Studio-codevoorbeelden.

## <a name="downloading-and-running-the-sample-code"></a>Downloaden en uitvoeren van de voorbeeldcode
Voor het downloaden van de code voor dit artikel:

* Visual Studio 2012 of hoger is vereist. 
* Download de [elastische DB-hulpprogramma's voor Azure SQL - Entity Framework integratie voorbeeld](https://code.msdn.microsoft.com/windowsapps/Elastic-Scale-with-Azure-bae904ba) van MSDN. Pak het voorbeeld naar een locatie van uw keuze.
* Start Visual Studio. 
* In Visual Studio-Selecteer-Bestand > Open Project/oplossing. 
* In de **Open Project** dialoogvenster, navigeer naar het voorbeeld dat u hebt gedownload en selecteer **EntityFrameworkCodeFirst.sln** openen van het voorbeeld. 

Als u wilt het voorbeeld uitvoert, moet u drie lege databases maken in Azure SQL Database:

* Shard-toewijzing Manager-database
* 1 shard-database
* 2 shard-database

Nadat u deze databases hebt gemaakt, vult u de houders plaats in **Program.cs** met de naam van uw Azure SQL DB-server, de databasenamen van de en uw referenties voor verbinding met de databases. Bouw de oplossing in Visual Studio. Visual Studio downloadt de vereiste NuGet-pakketten voor de clientbibliotheek elastische database Entity Framework en een tijdelijke fout verwerken als onderdeel van het buildproces. Zorg ervoor dat het herstellen van NuGet-pakketten is ingeschakeld voor uw oplossing. U kunt deze instelling inschakelen door met de rechtermuisknop op het oplossingsbestand in Visual Studio Solution Explorer. 

## <a name="entity-framework-workflows"></a>Entity Framework-werkstromen
Entity Framework ontwikkelaars zijn afhankelijk van een van de volgende vier werkstromen om toepassingen te bouwen en om ervoor te zorgen persistentie voor toepassingsobjecten: 

* **Eerste (nieuwe Database) code**: de EF-ontwikkelaar wordt gemaakt van het model in de toepassingscode en EF genereert vervolgens de database uit. 
* **Eerste (bestaande Database) code**: de ontwikkelaar kunt EF genereren de toepassingscode voor het model van een bestaande database.
* **Eerste model**: de ontwikkelaar maakt het model in de ontwerpfunctie EF en EF maakt vervolgens de database uit het model.
* **Eerste database**: EF tooling voor het afleiden van het model van een bestaande database maakt gebruik van de ontwikkelaar. 

Alle deze methoden zijn afhankelijk van de DbContext-klasse transparant databaseverbindingen en -databaseschema voor een toepassing beheren. Verschillende constructors in de basisklasse DbContext kunnen verschillende niveaus van controle over het maken van verbinding, database uitvoeren van de bootstrap en schema's. Uitdagingen ontstaan voornamelijk uit het feit dat de database verbinding management via EF in de polygoonring met mogelijkheden voor de verbinding van de gegevens-afhankelijke routeringsinterfaces opgegeven door de clientbibliotheek voor elastische database. 

## <a name="elastic-database-tools-assumptions"></a>Veronderstellingen voor elastische database-hulpprogramma 's
Zie voor definities van de termijn [verklarende woordenlijst hulpprogramma's van elastische Database](sql-database-elastic-scale-glossary.md).

Met de clientbibliotheek voor elastische databases definiëren u partities van de toepassingsgegevens aangeroepen shardlets. Shardlets worden geïdentificeerd door een sharding-sleutel en zijn toegewezen aan specifieke databases. Een toepassing kan zo veel databases zo nodig hebt en distribueren van de shardlets om voldoende capaciteit en prestaties gegeven van de huidige zakelijke vereisten te bieden. De toewijzing van sharding waarden van de databases is door een shard-toewijzing die is geleverd door de client-API's van elastische database opgeslagen. Deze mogelijkheid wordt aangeroepen **Shard kaart Management**, of SMM kortweg. De shard-kaart fungeert ook als de broker databaseverbindingen voor aanvragen die een sharding-sleutel bevatten. Deze mogelijkheid staat bekend als **gegevensafhankelijke routering**. 

De shard-toewijzing manager beveiligt gebruikers tegen inconsistente weergaven in shardlet gegevens die optreden kunnen wanneer de beheerbewerkingen gelijktijdige shardlet (zoals het verplaatsen van gegevens uit één shard naar een andere) plaatsvinden. Om dit te doen beheerd de shard-toewijzingen door de client-bibliotheek broker de databaseverbindingen voor een toepassing. Hiermee wordt de functionaliteit van de kaart shard een databaseverbinding automatisch afsluiten wanneer shard-beheerbewerkingen kunnen invloed hebben op de shardlet die voor de verbinding is gemaakt. Deze aanpak moet integreren met een aantal van de EF-functionaliteit, zoals het maken van nieuwe verbindingen van een bestaande om te controleren op database bestaan. In het algemeen is onze observatie dat de standaard DbContext-constructors alleen werk betrouwbaar voor gesloten databaseverbindingen die veilig kunnen worden gekloond voor EF werken. Het ontwerpprincipe van de elastische database wordt in plaats daarvan is het alleen broker geopende verbindingen. Een misschien denkt dat het sluiten van een verbinding die wordt geleverd door de clientbibliotheek voordat het overdragen van het naar de DbContext EF probleem mogelijk oplossen. Door de verbinding wordt gesloten en vertrouwen op EF te openen, foregoes een echter de validatie en consistentie controles uitgevoerd door de bibliotheek. De functionaliteit van migraties in EF, gebruikt echter deze verbindingen voor het beheren van het schema van de onderliggende database op een manier die is transparant voor de toepassing. In het ideale geval u behouden en combineer alle deze mogelijkheden van de clientbibliotheek voor elastische database en de EF in dezelfde toepassing. De volgende sectie wordt deze eigenschappen en de vereisten in meer detail beschreven. 

## <a name="requirements"></a>Vereisten
Als u werkt met de clientbibliotheek voor elastische database en de Entity Framework-API's, die u wilt behouden van de volgende eigenschappen: 

* **Scale-out**: toevoegen of verwijderen van databases van de gegevenslaag van de shard-toepassing die nodig zijn voor de vereisten van de capaciteit van de toepassing. Dit betekent dat de controle over het maken en verwijderen van databases en elastische database-shard-toewijzing manager API's gebruiken voor het beheren van databases en toewijzingen van shardlets. 
* **Consistentie**: de toepassing de veiligheidsmaatregelen voor sharding en gebruikt de gegevensafhankelijke routering van de clientbibliotheek. Om te voorkomen of beschadiging of onjuiste queryresultaten, worden de verbindingen geleverd door de shard-toewijzing manager. Dit houdt ook validatie en consistentie.
* **Eerste code**: het gemak van de EF code eerste paradigma behouden. In de Code First klassen in de toepassing transparant toegewezen aan de onderliggende structuur van de database. De toepassingscode communiceert met DbSets die de meeste aspecten die zijn betrokken bij de verwerking van de onderliggende database maskeren.
* **Schema**: Entity Framework initiële database schema maken en latere schema evolutie via migraties worden verwerkt. Als deze mogelijkheden aanhoudt, is aanpassing van uw app eenvoudig naarmate de gegevens zich verder ontwikkelen. 

De volgende richtlijnen geïnstrueerd hoe om te voldoen aan deze vereisten voor Code First-toepassingen met hulpprogramma's voor elastische database. 

## <a name="data-dependent-routing-using-ef-dbcontext"></a>Gegevensafhankelijke routering EF DbContext gebruiken
Databaseverbindingen met Entity Framework worden meestal beheerd via subklassen van **DbContext**. Deze subklassen maken die zijn afgeleid van **DbContext**. U definieert uw **DbSets** die de database-back-verzamelingen van CLR-objecten voor uw toepassing implementeren. In de context van het gegevensafhankelijke routering, kunt u verschillende nuttige eigenschappen die niet noodzakelijkerwijs voor andere EF code eerste toepassingsscenario's bewaart identificeren: 

* De database bestaat al en is geregistreerd in de shard-toewijzing van elastische database. 
* Het schema van de toepassing is al geïmplementeerd naar de database (Zie hieronder). 
* Gegevensafhankelijke routering verbindingen met de database zijn geleverd door de shard-toewijzing. 

Om te integreren **DbContexts** met gegevensafhankelijke routering voor scale-out:

1. De fysieke databaseverbindingen via de elastische database client-interfaces van de manager van de kaart shard maken 
2. Inpakken van de verbinding met de **DbContext** subklasse
3. De verbinding omlaag doorgeven aan de **DbContext** klassen om te controleren of de verwerking aan de kant EF gebeurt ook baseren. 

Het volgende codevoorbeeld ziet u deze methode. (Deze code is ook in het bijbehorende Visual Studio-project)

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
* Een nieuwe constructor vervangt de standaardconstructor in de DbContext-subklasse 
* De nieuwe constructor heeft de argumenten die vereist zijn voor het gegevensafhankelijke doorsturen via clientbibliotheek voor elastische database:
  
  * de shard-toewijzing voor toegang tot de gegevens-afhankelijke routeringsinterfaces
  * de sharding-sleutel voor het identificeren van de shardlet
  * een verbindingsreeks met de referenties voor de gegevensafhankelijke routering verbinding met de shard. 
* De aanroep naar de constructor basisklasse duurt een detour in een statische methode waarmee u alle stappen nodig voor het doorsturen van afhankelijk zijn van gegevens. 
  
  * Het maakt gebruik van de OpenConnectionForKey aanroep van de interfaces van de client elastische database op de shard-kaart een open verbinding tot stand brengen.
  * De shard-toewijzing wordt gemaakt van de open verbinding met de shard die de shardlet voor de opgegeven sharding-sleutel bevat.
  * Deze verbinding openen wordt doorgegeven aan de constructor basisklasse van DbContext om aan te geven dat deze verbinding moet worden gebruikt door EF in plaats van laten EF automatisch een nieuwe verbinding maken. De verbinding van deze manier zijn gelabeld door de client-API van de elastische database zodat zij consistentie onder beheerbewerkingen shard-toewijzing kunt waarborgen.

Gebruik de nieuwe constructor voor uw DbContext-subklasse in plaats van de standaardconstructor in uw code. Hier volgt een voorbeeld: 

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

De constructor new Hiermee opent u de verbinding met de shard waarin de gegevens voor de shardlet geïdentificeerd door de waarde van **tenantid1**. De code in de **met** blok blijft ongewijzigd voor toegang tot de **DbSet** voor blogs met EF op de shard voor **tenantid1**. Hiermee wijzigt u semantiek voor de code in het met blokkeren zoals dat alle databasebewerkingen nu zijn gericht op de ene shard waar **tenantid1** wordt bewaard. Bijvoorbeeld, een LINQ-query via de blogs **DbSet** retourneert alleen opgeslagen op de huidige shard-blogs, maar niet de bestanden die zijn opgeslagen op andere shards.  

#### <a name="transient-faults-handling"></a>Tijdelijke fouten verwerken
Het team van Microsoft Patterns & procedures gepubliceerd de [de tijdelijke fout afhandeling van Application Block](https://msdn.microsoft.com/library/dn440719.aspx). De tapewisselaar wordt gebruikt met de clientbibliotheek elastisch schalen in combinatie met EF. Er echter voor zorgen dat een tijdelijke uitzondering naar een locatie waar u ervoor zorgen retourneert kunt dat de nieuwe constructor na een tijdelijke fout gebruikt zodat elke nieuwe verbindingspoging wordt gemaakt met behulp van de constructors die u toegepast. Anders wordt een verbinding met de juiste shard kan niet worden gegarandeerd, en er zijn geen garantie die wordt bijgehouden als wijzigingen in de shard-toewijzing optreden. 

Het volgende codevoorbeeld ziet u hoe een SQL-beleid voor opnieuw proberen kan worden gebruikt om de nieuwe **DbContext** subklasse constructors: 

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

**SqlDatabaseUtils.SqlRetryPolicy** in de bovenstaande code wordt gedefinieerd als een **SqlDatabaseTransientErrorDetectionStrategy** met een aantal nieuwe pogingen van 10 en 5 seconden wachttijd tussen nieuwe pogingen. Deze aanpak is vergelijkbaar met de richtlijnen voor EF en transacties gebruiker gestart (Zie [beperkingen in de uitvoering van strategieën opnieuw uit te voeren (EF6 en hoger)](http://msdn.microsoft.com/data/dn307226). Beide situaties is vereist dat de toepassing kan bepalen het bereik waarvoor de tijdelijke uitzondering geretourneerd: aan de transactie opnieuw of maak de context van de juiste constructor (zoals) die gebruikmaakt van de clientbibliotheek voor elastische database.

De noodzaak om te bepalen waar tijdelijke uitzonderingen in beslag terug in bereik sluit ook het gebruik van de ingebouwde **SqlAzureExecutionStrategy** die wordt geleverd met EF. **SqlAzureExecutionStrategy** zou opnieuw een verbinding openen maar niet **OpenConnectionForKey** en daarom omzeilen alle validatietests die wordt uitgevoerd als onderdeel van de **OpenConnectionForKey** aanroepen. In plaats daarvan de voorbeeldcode maakt gebruik van de ingebouwde **DefaultExecutionStrategy** die wordt geleverd met EF. Niet **SqlAzureExecutionStrategy**, deze correct werkt in combinatie met het beleid voor opnieuw proberen van afhandeling van tijdelijke fout. Het uitvoeringsbeleid is ingesteld in de **ElasticScaleDbConfiguration** klasse. Let op: we besloten geen gebruik te **DefaultSqlExecutionStrategy** omdat er wordt verwezen met behulp van **SqlAzureExecutionStrategy** als tijdelijke uitzonderingen optreden - die zou leiden tot onjuiste gedrag zoals besproken. Zie voor meer informatie over de verschillende retry-beleid en de EF [Verbindingstolerantie in EF](http://msdn.microsoft.com/data/dn456835.aspx).     

#### <a name="constructor-rewrites"></a>Constructor regeneraties
De bovenstaande codevoorbeelden ziet u de standaard-constructor herschrijft vereist is voor uw toepassing om te kunnen gebruiken gegevensafhankelijke routering met de Entity Framework. De volgende tabel generaliseert deze benadering van andere constructors. 

| Huidige Constructor | Herschreven Constructor voor gegevens | Base-Constructor | Opmerkingen |
| --- | --- | --- | --- |
| MyContext() |ElasticScaleContext (ShardMap, TKey) |DbContext (DbConnection, bool) |De verbinding moet een functie van de shard-toewijzing en het gegevensafhankelijke routering sleutel. U moet maken van de automatische verbinding omzeilen door EF en in plaats daarvan broker van de verbinding met de shard-toewijzing. |
| MyContext(string) |ElasticScaleContext (ShardMap, TKey) |DbContext (DbConnection, bool) |De verbinding is een functie van de shard-toewijzing en het gegevensafhankelijke routering sleutel. Een vaste databaserol of verbindingstekenreeks werkt niet als ze validatie omzeilen door de shard-toewijzing. |
| MyContext(DbCompiledModel) |ElasticScaleContext (ShardMap, TKey, DbCompiledModel) |DbContext (DbConnection, DbCompiledModel, bool) |De verbinding wordt gemaakt voor de opgegeven shard-toewijzing en sharding-sleutel met het model. Het gecompileerde model wordt doorgegeven aan de basis c'tor. |
| MyContext (DbConnection, bool) |ElasticScaleContext (ShardMap, TKey, bool) |DbContext (DbConnection, bool) |De verbinding moet worden afgeleid van de shard-toewijzing en de sleutel. Deze kan niet worden opgegeven als invoer (tenzij deze invoer al voor de shard-toewijzing en de sleutel gebruikt is). De Booleaanse waarde is doorgegeven. |
| MyContext (string, DbCompiledModel) |ElasticScaleContext (ShardMap, TKey, DbCompiledModel) |DbContext (DbConnection, DbCompiledModel, bool) |De verbinding moet worden afgeleid van de shard-toewijzing en de sleutel. Deze kan niet worden opgegeven als invoer (tenzij deze invoer is de shard-toewijzing en de sleutel). Het gecompileerde model is doorgegeven. |
| MyContext (ObjectContext, bool) |ElasticScaleContext (ShardMap TKey, ObjectContext, bool) |DbContext (ObjectContext, bool) |De nieuwe constructor moet om ervoor te zorgen dat een verbinding in de ObjectContext is doorgegeven als invoer omgeleid naar een verbinding die wordt beheerd door elastisch schalen wordt. Een gedetailleerde discussie over ObjectContexts valt buiten het bereik van dit document. |
| MyContext (DbConnection, DbCompiledModel, bool) |ElasticScaleContext (ShardMap TKey, DbCompiledModel, bool) |DbContext (DbConnection, DbCompiledModel, bool); |De verbinding moet worden afgeleid van de shard-toewijzing en de sleutel. De verbinding kan niet worden opgegeven als invoer (tenzij deze invoer al voor de shard-toewijzing en de sleutel gebruikt is). Model en Booleaanse waarde worden doorgegeven aan de constructor basisklasse. |

## <a name="shard-schema-deployment-through-ef-migrations"></a>Implementatie van de shard-schema via EF-migraties
Automatische Schemabeheer is voor uw gemak geleverd door de Entity Framework. In de context van toepassingen met elastische database-hulpprogramma's die u wilt bewaren van deze mogelijkheid voor het automatisch inrichten van het schema voor de zojuist gemaakte shards wanneer databases worden toegevoegd aan de shard-toepassing. Er is een voornamelijk gebruikt om te verhogen voor shard-toepassingen met EF-capaciteit op de gegevenslaag. De database beheer inspanning vertrouwen op de EF mogelijkheden voor het Schemabeheer van het met een shard toepassing die is gebouwd op EF worden beperkt. 

Schema-implementatie via EF migraties werkt het beste op **nog niet gelezen verbindingen**. Dit is in tegenstelling tot het scenario voor het doorsturen van afhankelijk zijn van gegevens die is gebaseerd op de geopende verbinding geleverd door de client-API van de elastische database. Een ander verschil is de vereiste consistentie: tijdens het wenselijk om consistentie voor alle gegevensafhankelijke routering verbindingen te beschermen tegen gelijktijdige shard kaart manipulatie te garanderen, het is niet een probleem met het initiële schema implementatie naar een nieuwe database die nog niet is geregistreerd in de shard-toewijzing en nog niet is toegewezen aan de shardlets bevatten. U kunt daarom afgaan op reguliere databaseverbindingen voor dit scenario wordt in plaats van gegevensafhankelijke routering.  

Dit leidt tot een benadering waarbij schema implementatie via EF migraties is nauw samen met de registratie van de nieuwe database als een shard in shard-toewijzing van de toepassing. Dit is afhankelijk van de volgende vereisten: 

* De database is al gemaakt. 
* De database is leeg: Er is geen gebruikersschema en geen gebruikersgegevens bevat.
* De database kan niet nog toegankelijk via de client-API's van elastische database voor het doorsturen van afhankelijk zijn van gegevens. 

Met deze vereisten is voldaan, kunt u een gewone niet is geopend **SqlConnection** Activeer EF migraties voor schema-implementatie. Het volgende codevoorbeeld ziet u deze methode. 

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


Dit voorbeeld wordt de methode **RegisterNewShard** die de shard registreert in de shard-kaart, implementeert u het schema via EF migraties en slaat u een toewijzing van een sharding-sleutel voor de shard. Is afhankelijk van een constructor met de **DbContext** subklasse (**ElasticScaleContext** in de steekproef) waarmee een SQL-verbindingsreeks als invoer. De code van deze constructor is ongecompliceerde, zoals het volgende voorbeeld: 

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

Een mogelijk de versie van de constructor overgenomen van de basisklasse gebruikt. Maar de code nodig om ervoor te zorgen dat de standaard-initialisatiefunctie voor EF wordt gebruikt om verbinding te maken. De korte detour daarom in de statische methode aan voordat u aan de constructor basisklasse met de verbindingsreeks. Houd er rekening mee dat de registratie van shards moet worden uitgevoerd in een ander toepassingsdomein of het proces om ervoor te zorgen dat de instellingen van de initialisatiefunctie voor EF geen conflict veroorzaken. 

## <a name="limitations"></a>Beperkingen
De methoden die worden beschreven in dit document leidt tot een aantal beperkingen: 

* EF-toepassingen die gebruikmaken van **LocalDb** moet u eerst om te migreren naar een normale SQL Server-database voordat u de clientbibliotheek voor elastische database. Een toepassing via sharding uitbreiden met elastisch schalen is niet mogelijk met **LocalDb**. Houd er rekening mee dat ontwikkeling nog steeds kunt gebruiken **LocalDb**. 
* Eventuele wijzigingen in de toepassing die schemawijzigingen database impliceren moeten doorlopen EF migraties op alle shards. De voorbeeldcode voor dit document biedt niet laten zien hoe u dit doet. Overweeg het gebruik van de Database bijwerken met een parameter ConnectionString worden herhaald voor alle shards; of de T-SQL-script voor de in behandeling migratie met behulp van de Update-Database op te halen met een Script - optie en de T-SQL-script toepassen op uw shards.  
* Een aanvraag opgegeven, wordt ervan uitgegaan dat alle van de verwerking van de database zich in een enkel shard aangeduid met de sleutel sharding van de aanvraag. Echter, deze aanname niet altijd bijhoudt true. Bijvoorbeeld, wanneer het is niet mogelijk om een sharding-sleutel beschikbaar te maken. Daarom de clientbibliotheek biedt de **MultiShardQuery** klasse die een abstractie van de verbinding voor het uitvoeren van query's via meerdere shards implementeert. Leren gebruiken de **MultiShardQuery** in combinatie met EF valt buiten het bereik van dit document

## <a name="conclusion"></a>Conclusie
Via de stappen in dit document EF toepassingen kunnen gebruikmaken van de elastische database-clientbibliotheek mogelijkheid voor het doorsturen van afhankelijk zijn van gegevens door refactoring constructors van de **DbContext** subklassen gebruikt in de EF-toepassing. Dit beperkt de wijzigingen die zijn vereist voor deze locaties waar **DbContext** klassen bestaan al. Bovendien kunnen EF-toepassingen blijven profiteren van het schema voor automatische implementatie door een combinatie van de stappen die gebruikmaken van de benodigde EF migraties met de registratie van nieuwe shards en toewijzingen in de shard-toewijzing. 

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-scale-use-entity-framework-applications-visual-studio/sample.png
