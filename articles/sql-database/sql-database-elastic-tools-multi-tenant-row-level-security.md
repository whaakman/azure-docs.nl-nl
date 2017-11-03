---
title: Multitenant-toepassingen met elastische database-hulpprogramma's en beveiliging
description: Gebruik hulpprogramma's voor elastische database met beveiliging op rijniveau voor het bouwen van een toepassing met een hoge mate schaalbaar gegevenslaag.
metakeywords: azure sql database elastic tools multi tenant row level security rls
services: sql-database
documentationcenter: 
manager: jhubbard
author: tmullaney
ms.assetid: e72d3cfe-e9be-4326-b776-9c6d96c0a18e
ms.service: sql-database
ms.custom: scale out apps
ms.workload: On Demand
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/27/2016
ms.author: thmullan;torsteng
ms.openlocfilehash: 4b4ec29bb53bdce413dadf7cb0751433b9ca686c
ms.sourcegitcommit: dfd49613fce4ce917e844d205c85359ff093bb9c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/31/2017
---
# <a name="multi-tenant-applications-with-elastic-database-tools-and-row-level-security"></a>Multitenant-toepassingen met elastische database-hulpprogramma's en beveiliging
[Hulpprogramma's voor elastische database](sql-database-elastic-scale-get-started.md) en [rijniveau beveiliging (RLS)](https://msdn.microsoft.com/library/dn765131) bieden een set krachtige mogelijkheden voor het flexibel en efficiënt schalen van de gegevenslaag van een toepassing met meerdere tenants met Azure SQL Database. Zie voor meer informatie [ontwerppatronen voor multitenant SaaS-toepassingen met Azure SQL Database](sql-database-design-patterns-multi-tenancy-saas-applications.md) voor meer informatie. 

In dit artikel laat zien hoe deze technologieën samen gebruiken voor het bouwen van een toepassing met een hoge mate schaalbaar gegevenslaag die ondersteuning biedt voor meerdere tenants shards, met behulp van **ADO.NET SqlClient** en/of **Entity Framework**.  

* **Hulpprogramma's voor elastische database** kunnen ontwikkelaars de gegevenslaag van een toepassing via industriestandaard sharding procedures met behulp van een set van .NET-bibliotheken en Azure servicesjablonen worden uitgebreid. Het beheren van shards met behulp van de elastische Database-clientbibliotheek kunt automatiseren en veel van de infrastructurele taken meestal gekoppeld aan sharding stroomlijnen. 
* **Beveiliging op gebruikersniveau rij** kunnen ontwikkelaars voor het opslaan van gegevens voor meerdere tenants in dezelfde database met behulp van beveiligingsbeleid voor het filteren van rijen die geen deel uitmaken van de tenant een query uit te voeren. Logica van de toegang voor beveiliging op Rijniveau in de database, centraliseren groeit in plaats van in de toepassing, vereenvoudigt u het onderhoud en vermindert het risico van fouten als een toepassing de codebasis. 

Een toepassing kan deze functies samen gebruikt, profiteren van kosten besparen en efficiëntie toeneemt door het opslaan van gegevens voor meerdere tenants in dezelfde shard-database. Een toepassing is op hetzelfde moment nog steeds de flexibiliteit te bieden geïsoleerd, één tenant shards voor 'premium' tenants die strenger prestaties garanties nodig omdat multitenant shards komen niet gegarandeerd dat gelijk resource verdeling van tenants.  

Kort gezegd: de elastische database van de clientbibliotheek [gegevensafhankelijke routering](sql-database-elastic-scale-data-dependent-routing.md) API's automatisch tenants verbinding maken met de juiste shard-database met hun sharding-sleutel (doorgaans een ' TenantId'). Eenmaal zijn verbonden, wordt een beveiligingsbeleid RLS binnen de database zorgt ervoor dat tenants alleen toegang tot rijen die hun TenantId bevatten. Ervan wordt uitgegaan dat alle tabellen bevatten een kolom TenantId om aan te geven welke rijen aan elke tenant behoren. 

![Blog app-architectuur][1]

## <a name="download-the-sample-project"></a>Download het voorbeeldproject
### <a name="prerequisites"></a>Vereisten
* Gebruik Visual Studio (2012 of hoger) 
* Drie Azure SQL-Databases maken 
* Download voorbeeldproject: [elastische DB-hulpprogramma's voor Azure SQL - Multitenant-Shards](http://go.microsoft.com/?linkid=9888163)
  * Vul de gegevens van uw databases aan het begin van **Program.cs** 

Dit project wordt uitgebreid beschreven in een [elastische DB-hulpprogramma's voor Azure SQL - Entity Framework integratie](sql-database-elastic-scale-use-entity-framework-applications-visual-studio.md) door ondersteuning voor meerdere tenants shard databases toe te voegen. Dit is een eenvoudige consoletoepassing voor het maken van blogs en berichten, met vier tenants en twee multitenant shard-databases zoals geïllustreerd in het voorgaande diagram gebaseerd. 

Ontwikkel en voer de toepassing. Dit is de elastische database tools shard kaart manager bootstraps en voer de volgende tests uit: 

1. Met behulp van Entity Framework en LINQ, een nieuw blog maken en vervolgens alle blogs voor elke tenant weergeven
2. Alle blogs voor een tenant met behulp van ADO.NET SqlClient weergeven
3. Probeer in te voegen een blog voor de verkeerde tenant om te controleren of er een fout wordt gegenereerd  

U ziet dat omdat RLS nog niet is ingeschakeld in de shard-databases, elk van deze tests een probleem opgetreden blijkt: tenants kunnen zien blogs die geen deel van deze uitmaken zijn, en de toepassing niet wordt verhinderd invoegen van een blog voor de verkeerde tenant. De rest van dit artikel wordt beschreven hoe u kunt deze problemen oplossen door het afdwingen van tenantisolatie voor beveiliging op Rijniveau. Er zijn twee stappen: 

1. **Toepassingslaag**: de toepassingscode altijd de huidige TenantId om in te stellen de SESSION_CONTEXT na het openen van een verbinding te wijzigen. Het voorbeeldproject heeft dit al gedaan. 
2. **Gegevenslaag**: maken van een beveiligingsbeleid RLS in elke database shard rijen filteren op basis van de opgeslagen in SESSION_CONTEXT TenantId. U moet dit doen voor elk van de shard-databases, anders rijen in een multitenant shards worden niet gefilterd. 

## <a name="step-1-application-tier-set-tenantid-in-the-sessioncontext"></a>Stap 1) toepassingslaag: TenantId ingesteld in de SESSION_CONTEXT
Nadat u verbinding maakt met een shard-database met behulp van de elastische database-clientbibliotheek gegevensafhankelijke routering API's, de toepassing nog steeds moet de database zien welke TenantId die verbinding wordt gebruikt zodat een beveiligingsbeleid RLS rijen die horen bij kunt uitfilteren andere tenants. De aanbevolen manier om door te geven deze informatie is voor het opslaan van de huidige TenantId voor die verbinding in de [SESSION_CONTEXT](https://msdn.microsoft.com/library/mt590806.aspx). (Opmerking: U kunt ook [CONTEXT_INFO](https://msdn.microsoft.com/library/ms180125.aspx), maar SESSION_CONTEXT is een betere optie omdat dit eenvoudiger te gebruiken, retourneert NULL standaard en sleutel-waardeparen ondersteunt.)

### <a name="entity-framework"></a>Entity Framework
Voor toepassingen met behulp van Entity Framework, wordt de beste aanpak is om in te stellen de SESSION_CONTEXT binnen de ElasticScaleContext onderdrukking wordt beschreven in [gegevensafhankelijke routering met EF DbContext](sql-database-elastic-scale-use-entity-framework-applications-visual-studio.md#data-dependent-routing-using-ef-dbcontext). Voordat de verbinding die wordt geleverd door het gegevensafhankelijke routering wordt geretourneerd, maken en uitvoeren van een SqlCommand die 'TenantId' ingesteld in de SESSION_CONTEXT naar de shardingKey opgegeven voor die verbinding. Op deze manier alleen moet u één keer code schrijven om in te stellen de SESSION_CONTEXT. 

```
// ElasticScaleContext.cs 
// ... 
// C'tor for data-dependent routing. This call opens a validated connection routed to the proper 
// shard by the shard map manager. Note that the base class c'tor call fails for an open connection 
// if migrations need to be done and SQL credentials are used. This is the reason for the  
// separation of c'tors into the DDR case (this c'tor) and the internal c'tor for new shards. 
public ElasticScaleContext(ShardMap shardMap, T shardingKey, string connectionStr)
    : base(OpenDDRConnection(shardMap, shardingKey, connectionStr), true /* contextOwnsConnection */)
{
}

public static SqlConnection OpenDDRConnection(ShardMap shardMap, T shardingKey, string connectionStr)
{
    // No initialization
    Database.SetInitializer<ElasticScaleContext<T>>(null);

    // Ask shard map to broker a validated connection for the given key
    SqlConnection conn = null;
    try
    {
        conn = shardMap.OpenConnectionForKey(shardingKey, connectionStr, ConnectionOptions.Validate);

        // Set TenantId in SESSION_CONTEXT to shardingKey to enable Row-Level Security filtering
        SqlCommand cmd = conn.CreateCommand();
        cmd.CommandText = @"exec sp_set_session_context @key=N'TenantId', @value=@shardingKey";
        cmd.Parameters.AddWithValue("@shardingKey", shardingKey);
        cmd.ExecuteNonQuery();

        return conn;
    }
    catch (Exception)
    {
        if (conn != null)
        {
            conn.Dispose();
        }

        throw;
    }
} 
// ... 
```

De SESSION_CONTEXT wordt nu automatisch ingesteld met de opgegeven TenantId wanneer ElasticScaleContext wordt opgeroepen: 

```
// Program.cs 
SqlDatabaseUtils.SqlRetryPolicy.ExecuteAction(() => 
{   
    using (var db = new ElasticScaleContext<int>(sharding.ShardMap, tenantId, connStrBldr.ConnectionString))   
    {     
        var query = from b in db.Blogs
                    orderby b.Name
                    select b;

        Console.WriteLine("All blogs for TenantId {0}:", tenantId);     
        foreach (var item in query)     
        {       
            Console.WriteLine(item.Name);     
        }   
    } 
}); 
```

### <a name="adonet-sqlclient"></a>ADO.NET SqlClient
Voor toepassingen met behulp van ADO.NET SqlClient, is de aanbevolen aanpak voor het maken van een functie wrapper rond ShardMap.OpenConnectionForKey() die automatisch wordt ingesteld 'TenantId' in de SESSION_CONTEXT naar de juiste TenantId voordat een verbinding wordt geretourneerd. Om ervoor te zorgen dat er altijd SESSION_CONTEXT is ingesteld, moet u alleen verbindingen met deze functie wrapper openen.

```
// Program.cs
// ...

// Wrapper function for ShardMap.OpenConnectionForKey() that automatically sets SESSION_CONTEXT with the correct
// tenantId before returning a connection. As a best practice, you should only open connections using this 
// method to ensure that SESSION_CONTEXT is always set before executing a query.
public static SqlConnection OpenConnectionForTenant(ShardMap shardMap, int tenantId, string connectionStr)
{
    SqlConnection conn = null;
    try
    {
        // Ask shard map to broker a validated connection for the given key
        conn = shardMap.OpenConnectionForKey(tenantId, connectionStr, ConnectionOptions.Validate);

        // Set TenantId in SESSION_CONTEXT to shardingKey to enable Row-Level Security filtering
        SqlCommand cmd = conn.CreateCommand();
        cmd.CommandText = @"exec sp_set_session_context @key=N'TenantId', @value=@shardingKey";
        cmd.Parameters.AddWithValue("@shardingKey", tenantId);
        cmd.ExecuteNonQuery();

        return conn;
    }
    catch (Exception)
    {
        if (conn != null)
        {
            conn.Dispose();
        }

        throw;
    }
}

// ...

// Example query via ADO.NET SqlClient
// If row-level security is enabled, only Tenant 4's blogs are listed
SqlDatabaseUtils.SqlRetryPolicy.ExecuteAction(() =>
{
    using (SqlConnection conn = OpenConnectionForTenant(sharding.ShardMap, tenantId4, connStrBldr.ConnectionString))
    {
        SqlCommand cmd = conn.CreateCommand();
        cmd.CommandText = @"SELECT * FROM Blogs";

        Console.WriteLine("--\nAll blogs for TenantId {0} (using ADO.NET SqlClient):", tenantId4);
        SqlDataReader reader = cmd.ExecuteReader();
        while (reader.Read())
        {
            Console.WriteLine("{0}", reader["Name"]);
        }
    }
});

```

## <a name="step-2-data-tier-create-row-level-security-policy"></a>Stap 2) gegevens laag: rijniveau beveiligingsbeleid maken
### <a name="create-a-security-policy-to-filter-the-rows-each-tenant-can-access"></a>Een beveiligingsbeleid voor het filteren van de rijen die elke tenant toegang heeft tot maken
Nu dat de toepassing SESSION_CONTEXT met de huidige TenantId instellen is voordat het uitvoeren van query's, kan een beveiligingsbeleid RLS query's filteren en rijen met een andere TenantId uitsluiten.  

RLS is geïmplementeerd in T-SQL: een door de gebruiker gedefinieerde functie definieert de logica toegang en een beveiligingsbeleid voor deze functie koppelt aan een onbeperkt aantal tabellen. Voor dit project, controleert de functie of dat de toepassing (in plaats van een andere SQL-gebruiker) is verbonden met de database en dat de 'TenantId' in de SESSION_CONTEXT opgeslagen overeenkomt met de TenantId van een bepaalde rij. Een filterpredicaat kunt rijen die voldoen aan deze voorwaarden doorgeven aan het filter voor SELECT-, UPDATE- en DELETE-query's. en een block-predicaat voorkomt dat de rijen die strijdig zijn met deze voorwaarden niet kan worden ingevoegd of bijgewerkt. Als SESSION_CONTEXT is niet ingesteld, wordt het resultaat dat null en geen rijen zijn zichtbaar of kunnen worden ingevoegd. 

Schakel RLS door de volgende T-SQL niet uitvoeren op alle shards met Visual Studio (SSDT), SSMS of het PowerShell-script dat is opgenomen in het project (of als u [elastische Database taken](sql-database-elastic-jobs-overview.md), kunt u het automatiseren van de uitvoering van deze T-SQL op alle shards): 

```
CREATE SCHEMA rls -- separate schema to organize RLS objects 
GO

CREATE FUNCTION rls.fn_tenantAccessPredicate(@TenantId int)     
    RETURNS TABLE     
    WITH SCHEMABINDING
AS
    RETURN SELECT 1 AS fn_accessResult          
        WHERE DATABASE_PRINCIPAL_ID() = DATABASE_PRINCIPAL_ID('dbo') -- the user in your application’s connection string (dbo is only for demo purposes!)         
        AND CAST(SESSION_CONTEXT(N'TenantId') AS int) = @TenantId
GO

CREATE SECURITY POLICY rls.tenantAccessPolicy
    ADD FILTER PREDICATE rls.fn_tenantAccessPredicate(TenantId) ON dbo.Blogs,
    ADD BLOCK PREDICATE rls.fn_tenantAccessPredicate(TenantId) ON dbo.Blogs,
    ADD FILTER PREDICATE rls.fn_tenantAccessPredicate(TenantId) ON dbo.Posts,
    ADD BLOCK PREDICATE rls.fn_tenantAccessPredicate(TenantId) ON dbo.Posts
GO 
```

> [!TIP]
> Complexere projecten moet het predicaat op honderden tabellen toevoegen, kunt u een helper opgeslagen procedure die genereert automatisch een beveiligingsbeleid toe te voegen een predicaat voor alle tabellen in een schema. Zie voor meer informatie [rijniveau beveiliging toepassen op alle tabellen - helper-script (blog)](http://blogs.msdn.com/b/sqlsecurity/archive/2015/03/31/apply-row-level-security-to-all-tables-helper-script).  
> 
> 

Als u de voorbeeldtoepassing opnieuw uitvoert, Zie tenants nu alleen rijen die deel uitmaken van deze. Bovendien kan de toepassing niet invoegen rijen die deel uitmaken van tenants andere dan de momenteel met de shard-database verbonden en deze zichtbare rijen als u wilt dat een andere tenant-id kan niet worden bijgewerkt. Als de toepassing probeert te doen, wordt een DbUpdateException gegeven.

Als u later op een nieuwe tabel toevoegen, wijzigen van het beveiligingsbeleid en filter toevoegen en blokkeren predicaten voor de nieuwe tabel: 

```
ALTER SECURITY POLICY rls.tenantAccessPolicy     
    ADD FILTER PREDICATE rls.fn_tenantAccessPredicate(TenantId) ON dbo.MyNewTable,
    ADD BLOCK PREDICATE rls.fn_tenantAccessPredicate(TenantId) ON dbo.MyNewTable
GO 
```

### <a name="add-default-constraints-to-automatically-populate-tenantid-for-inserts"></a>Default-beperkingen om automatisch te voorzien TenantId voor voegt toevoegen
U kunt een default-beperking voor elke tabel de TenantId automatisch gevuld met de waarde die momenteel zijn opgeslagen in SESSION_CONTEXT bij het invoegen van rijen plaatsen. Bijvoorbeeld: 

```
-- Create default constraints to auto-populate TenantId with the value of SESSION_CONTEXT for inserts 
ALTER TABLE Blogs     
    ADD CONSTRAINT df_TenantId_Blogs      
    DEFAULT CAST(SESSION_CONTEXT(N'TenantId') AS int) FOR TenantId 
GO

ALTER TABLE Posts     
    ADD CONSTRAINT df_TenantId_Posts      
    DEFAULT CAST(SESSION_CONTEXT(N'TenantId') AS int) FOR TenantId 
GO 
```

Nu hoeft de toepassing niet op te geven van een TenantId bij het invoegen van rijen: 

```
SqlDatabaseUtils.SqlRetryPolicy.ExecuteAction(() => 
{   
    using (var db = new ElasticScaleContext<int>(sharding.ShardMap, tenantId, connStrBldr.ConnectionString))
    {
        var blog = new Blog { Name = name }; // default constraint sets TenantId automatically     
        db.Blogs.Add(blog);     
        db.SaveChanges();   
    } 
}); 
```

> [!NOTE]
> Als u een default-beperkingen voor een Entity Framework-project gebruiken, is het raadzaam dat u de kolom TenantId niet in uw EF-gegevensmodel opnemen. Dit komt doordat de Entity Framework-query's worden automatisch standaardwaarden die de standaardbeperkingen gemaakt in T-SQL met SESSION_CONTEXT overschrijven opgeeft. Als u de default-beperkingen in het voorbeeldproject, bijvoorbeeld: moet u TenantId verwijderen uit DataClasses.cs (en voer Add-migratie in de Package Manager-Console) en T-SQL om ervoor te zorgen dat het veld alleen in de databasetabellen bestaat. Op deze manier EF automatisch levert onjuiste standaardwaarden bij het invoegen van gegevens. 
> 
> 

### <a name="optional-enable-a-superuser-to-access-all-rows"></a>(Optioneel) Een 'beheerder' voor toegang tot alle rijen inschakelen
Sommige toepassingen kunt maken van een 'beheerder' wie toegang alle rijen, bijvoorbeeld tot om aan te melden voor alle tenants in alle shards inschakelen of gesplitste/Merge-bewerkingen op shards die betrekking hebben op tenant rijen verplaatsen tussen databases uit te voeren. U kunt deze inschakelen, moet u een nieuwe SQL-gebruiker ('superuser' in dit voorbeeld) in elke shard-database maken. Het beveiligingsbeleid met een nieuwe predicaatfunctie waarmee deze gebruiker toegang tot alle rijen wijzigt:

```
-- New predicate function that adds superuser logic
CREATE FUNCTION rls.fn_tenantAccessPredicateWithSuperUser(@TenantId int)
    RETURNS TABLE
    WITH SCHEMABINDING
AS
    RETURN SELECT 1 AS fn_accessResult 
        WHERE 
        (
            DATABASE_PRINCIPAL_ID() = DATABASE_PRINCIPAL_ID('dbo') -- note, should not be dbo!
            AND CAST(SESSION_CONTEXT(N'TenantId') AS int) = @TenantId
        ) 
        OR
        (
            DATABASE_PRINCIPAL_ID() = DATABASE_PRINCIPAL_ID('superuser')
        )
GO

-- Atomically swap in the new predicate function on each table
ALTER SECURITY POLICY rls.tenantAccessPolicy
    ALTER FILTER PREDICATE rls.fn_tenantAccessPredicateWithSuperUser(TenantId) ON dbo.Blogs,
    ALTER BLOCK PREDICATE rls.fn_tenantAccessPredicateWithSuperUser(TenantId) ON dbo.Blogs,
    ALTER FILTER PREDICATE rls.fn_tenantAccessPredicateWithSuperUser(TenantId) ON dbo.Posts,
    ALTER BLOCK PREDICATE rls.fn_tenantAccessPredicateWithSuperUser(TenantId) ON dbo.Posts
GO
```


### <a name="maintenance"></a>Onderhoud
* **Toevoegen van nieuwe shards**: de T-SQL-script waarmee u RLS op alle nieuwe shards uitvoeren, anders query's in deze shards worden niet gefilterd.
* **Toevoegen van nieuwe tabellen**: Voeg een filter en blok predikaat aan het beveiligingsbeleid op alle shards wanneer een nieuwe tabel is gemaakt, anders query's op de nieuwe tabel worden niet gefilterd. Dit kan worden geautomatiseerd met behulp van een DDL-trigger, zoals beschreven in [toepassen beveiliging automatisch op nieuwe tabellen gemaakt (blog)](http://blogs.msdn.com/b/sqlsecurity/archive/2015/05/22/apply-row-level-security-automatically-to-newly-created-tables.aspx).

## <a name="summary"></a>Samenvatting
Elastische database-hulpprogramma's en de beveiliging kunnen worden gebruikt samen voor scale-out van de toepassing gegevenslaag met ondersteuning voor beide multitenant en één tenant shards. Multitenant shards kunnen worden gebruikt voor het opslaan van gegevens efficiënter (met name in gevallen waarin een groot aantal tenants slechts een paar rijen met gegevens hebben), tijdens één tenant shards kunnen worden gebruikt ter ondersteuning van tenants met strengere vereisten voor prestaties en isolatie premium.  Zie voor meer informatie [beveiliging verwijzing](https://msdn.microsoft.com/library/dn765131). 

## <a name="additional-resources"></a>Aanvullende bronnen
* [Wat is een Azure elastische groep?](sql-database-elastic-pool.md)
* [Uitbreiden met Azure SQL Database](sql-database-elastic-scale-introduction.md)
* [Ontwerppatronen voor SaaS-toepassingen met meerdere tenants met behulp van Azure SQL Database](sql-database-design-patterns-multi-tenancy-saas-applications.md)
* [Verificatie in multitenant-apps met Azure AD en OpenID Connect](../guidance/guidance-multitenant-identity-authenticate.md)
* [De toepassing Tailspin Surveys](../guidance/guidance-multitenant-identity-tailspin.md)

## <a name="questions-and-feature-requests"></a>Vragen en Functieaanvragen
Voor vragen bereiken ons op de [SQL Database-forum](http://social.msdn.microsoft.com/forums/azure/home?forum=ssdsgetstarted) en voor functieaanvragen, toe te voegen aan de [forum met feedback van SQL-Database](https://feedback.azure.com/forums/217321-sql-database/).

<!--Image references-->
[1]: ./media/sql-database-elastic-tools-multi-tenant-row-level-security/blogging-app.png
<!--anchors-->


