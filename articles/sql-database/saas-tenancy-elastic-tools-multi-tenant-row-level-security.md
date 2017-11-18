---
title: Multitenant-apps met RLS en hulpmiddelen voor elastische databases | Microsoft Docs
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
ms.date: 11/16/2017
ms.author: thmullan;torsteng
ms.openlocfilehash: f874800e8647eac6ed94945d79e904df86207b0f
ms.sourcegitcommit: a036a565bca3e47187eefcaf3cc54e3b5af5b369
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/17/2017
---
# <a name="multi-tenant-applications-with-elastic-database-tools-and-row-level-security"></a>Multitenant-toepassingen met elastische database-hulpprogramma's en beveiliging

[Hulpprogramma's voor elastische database](sql-database-elastic-scale-get-started.md) en [rijniveau beveiliging (RLS)] [ rls] werken samen om het inschakelen van de gegevenslaag van een toepassing met meerdere tenants met Azure SQL Database schalen. Samen helpen deze technologieën voor het ontwikkelen van een toepassing met een hoge mate schaalbaar gegevenslaag. De gegevenslaag biedt ondersteuning voor meerdere tenants shards en maakt gebruik van **ADO.NET SqlClient** of **Entity Framework**. Zie voor meer informatie [ontwerppatronen voor multitenant SaaS-toepassingen met Azure SQL Database](saas-tenancy-app-design-patterns.md).

- **Hulpprogramma's voor elastische database** kunnen ontwikkelaars de gegevenslaag standaard sharding praktijken, uitbreiden met behulp van .NET-bibliotheken en sjablonen voor Azure-service. Shards beheren met behulp van de [clientbibliotheek voor elastische Database] [ s-d-elastic-database-client-library] kunt automatiseren en veel van de infrastructurele taken meestal gekoppeld aan sharding stroomlijnen.
- **Beveiliging op gebruikersniveau rij** kunnen ontwikkelaars veilig opslaan van gegevens voor meerdere tenants in dezelfde database. Beveiligingsbeleid RLS filter rijen die geen deel uitmaken van de tenant een query uit te voeren. De filter logica in de database te centraliseren, vereenvoudigt u het onderhoud en vermindert het risico van een beveiligingsfout. Het alternatief van vertrouwen op alle clientcode enfore beveiliging is riskant.

Door deze functies samen gebruikt, kunt een toepassing gegevens opslaan voor meerdere tenants in dezelfde shard-database. Kost het minder per tenant als de tenants een database deelt. Nog kunt dezelfde toepassing bieden ook de premium-tenants de mogelijkheid om betaalt voor hun eigen toegewezen shard voor één tenant. Een voordeel van één tenantisolatie is verhogen prestaties garanties. In een één-tenant-database is er geen andere tenant concurrentie voor resources.

Het doel is het gebruik van de clientbibliotheek voor elastische database [gegevensafhankelijke routering](sql-database-elastic-scale-data-dependent-routing.md) API's voor elke opgegeven tenant wordt automatisch verbinding maken met de juiste shard-database. Slechts één shard bevat bepaalde TenantId waarde voor de opgegeven tenant. De tenant-id is de *sharding sleutel*. Nadat de verbinding tot stand is gebracht, wordt een beveiligingsbeleid RLS binnen de database zorgt ervoor dat de opgegeven tenant toegang heeft tot alleen die rijen met gegevens die de tenant-id bevatten.

> [!NOTE]
> De tenant-id kan bestaan uit meer dan een kolom. Voor uw gemak deze discussie is, aannemende informeel een TenantId één kolom.

![Blog app-architectuur][1]

## <a name="download-the-sample-project"></a>Download het voorbeeldproject

### <a name="prerequisites"></a>Vereisten

- Gebruik Visual Studio (2012 of hoger) 
- Drie Azure SQL-Databases maken 
- Download voorbeeldproject: [elastische DB-hulpprogramma's voor Azure SQL - Multitenant-Shards](http://go.microsoft.com/?linkid=9888163)
  - Vul de gegevens van uw databases aan het begin van **Program.cs** 

Dit project wordt uitgebreid beschreven in een [elastische DB-hulpprogramma's voor Azure SQL - Entity Framework integratie](sql-database-elastic-scale-use-entity-framework-applications-visual-studio.md) door ondersteuning voor meerdere tenants shard databases toe te voegen. Het project bouwt een eenvoudige consoletoepassing voor het maken van blogs en advertenties. Het project bevat vier tenants, plus twee multitenant shard-databases. Deze configuratie wordt weergegeven in het voorgaande diagram. 

Ontwikkel en voer de toepassing. Dit wordt uitgevoerd de elastische database tools shard kaart manager bootstraps en voert de volgende tests: 

1. Met behulp van Entity Framework en LINQ, een nieuw blog maken en vervolgens alle blogs voor elke tenant weergeven
2. Alle blogs voor een tenant met behulp van ADO.NET SqlClient weergeven
3. Probeer in te voegen een blog voor de verkeerde tenant om te controleren of er een fout wordt gegenereerd  

U ziet dat omdat RLS nog niet is ingeschakeld in de shard-databases, elk van deze tests een probleem opgetreden blijkt: tenants kunnen zien blogs die geen deel van deze uitmaken zijn, en de toepassing niet wordt verhinderd invoegen van een blog voor de verkeerde tenant. De rest van dit artikel wordt beschreven hoe u kunt deze problemen oplossen door het afdwingen van tenantisolatie voor beveiliging op Rijniveau. Er zijn twee stappen: 

1. **Toepassingslaag**: Wijzig de toepassingscode om altijd de huidige TenantId ingesteld in de sessie\_CONTEXT na het openen van een verbinding. Het voorbeeldproject stelt de TenantId al op deze manier. 
2. **Gegevenslaag**: maken van een beveiligingsbeleid RLS in elke database shard rijen filteren op basis van de tenant-id opgeslagen in de sessie\_CONTEXT. Maak een beleid voor elk van de shard-databases, anders rijen in een multitenant shards worden niet gefilterd. 

## <a name="1-application-tier-set-tenantid-in-the-sessioncontext"></a>1. Toepassingslaag: Set TenantId in de sessie\_CONTEXT

Eerst verbinding u met een shard-database met de routering API's afhankelijk zijn van gegevens van de clientbibliotheek voor elastische database. De toepassing nog steeds moet op de hoogte de database welke TenantId wordt de verbinding gebruikt. De TenantId Hiermee geeft u het beveiligingsbeleid RLS welke rijen moeten worden gefilterd als onderdeel van andere tenants. Opslaan van de huidige TenantId in de [sessie\_CONTEXT](https://docs.microsoft.com/sql/t-sql/functions/session-context-transact-sql) van de verbinding.

Een alternatief voor sessie\_CONTEXT is het gebruik van [CONTEXT\_INFO](https://docs.microsoft.com/sql/t-sql/functions/context-info-transact-sql). Maar sessie\_CONTEXT is een betere optie. SESSIE\_CONTEXT is eenvoudiger te gebruiken, wordt standaard NULL en het sleutel-waardeparen ondersteunt.

### <a name="entity-framework"></a>Entity Framework

Voor toepassingen met behulp van Entity Framework, wordt de beste aanpak is om in te stellen van de sessie\_CONTEXT binnen de ElasticScaleContext onderdrukking wordt beschreven in [gegevensafhankelijke routering met EF DbContext](sql-database-elastic-scale-use-entity-framework-applications-visual-studio.md#data-dependent-routing-using-ef-dbcontext). Maken en uitvoeren van een SqlCommand die TenantId ingesteld in de sessie\_CONTEXT voor de shardingKey opgegeven voor de verbinding. Retourneert de verbinding die wordt geleverd door het gegevensafhankelijke routering. Op deze manier hoeft u alleen te eenmaal code schrijven om in te stellen van de sessie\_CONTEXT. 

```csharp
// ElasticScaleContext.cs 
// Constructor for data-dependent routing.
// This call opens a validated connection that is routed to the
// proper shard by the shard map manager.
// Note that the base class constructor call fails for an open connection 
// if migrations need to be done and SQL credentials are used.
// This is the reason for the separation of constructors.
// ...
public ElasticScaleContext(ShardMap shardMap, T shardingKey, string connectionStr)
    : base(
        OpenDDRConnection(shardMap, shardingKey, connectionStr),
        true)  // contextOwnsConnection
{
}

public static SqlConnection OpenDDRConnection(
    ShardMap shardMap,
    T shardingKey,
    string connectionStr)
{
    // No initialization.
    Database.SetInitializer<ElasticScaleContext<T>>(null);

    // Ask shard map to broker a validated connection for the given key.
    SqlConnection conn = null;
    try
    {
        conn = shardMap.OpenConnectionForKey(
            shardingKey,
            connectionStr,
            ConnectionOptions.Validate);

        // Set TenantId in SESSION_CONTEXT to shardingKey
        // to enable Row-Level Security filtering.
        SqlCommand cmd = conn.CreateCommand();
        cmd.CommandText =
            @"exec sp_set_session_context
                @key=N'TenantId', @value=@shardingKey";
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

Nu de sessie\_CONTEXT wordt automatisch ingesteld met de opgegeven TenantId wanneer ElasticScaleContext wordt opgeroepen: 

```csharp
// Program.cs 
SqlDatabaseUtils.SqlRetryPolicy.ExecuteAction(() => 
{   
    using (var db = new ElasticScaleContext<int>(
        sharding.ShardMap, tenantId, connStrBldr.ConnectionString))   
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

Maak een functie wrapper rond methode ShardMap.OpenConnectionForKey voor toepassingen met behulp van ADO.NET SqlClient. De wrapper TenantId automatisch ingesteld in de sessie hebt\_CONTEXT voor de huidige TenantId voordat een verbinding wordt geretourneerd. Om ervoor te zorgen die sessie\_CONTEXT is altijd ingesteld, moet u alleen verbindingen met deze functie wrapper openen.

```csharp
// Program.cs
// Wrapper function for ShardMap.OpenConnectionForKey() that
// automatically sets SESSION_CONTEXT with the correct
// tenantId before returning a connection.
// As a best practice, you should only open connections using this method
// to ensure that SESSION_CONTEXT is always set before executing a query.
// ...
public static SqlConnection OpenConnectionForTenant(
    ShardMap shardMap, int tenantId, string connectionStr)
{
    SqlConnection conn = null;
    try
    {
        // Ask shard map to broker a validated connection for the given key.
        conn = shardMap.OpenConnectionForKey(
            tenantId, connectionStr, ConnectionOptions.Validate);

        // Set TenantId in SESSION_CONTEXT to shardingKey
        // to enable Row-Level Security filtering.
        SqlCommand cmd = conn.CreateCommand();
        cmd.CommandText =
            @"exec sp_set_session_context
                @key=N'TenantId', @value=@shardingKey";
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

// Example query via ADO.NET SqlClient.
// If row-level security is enabled, only Tenant 4's blogs are listed.
SqlDatabaseUtils.SqlRetryPolicy.ExecuteAction(() =>
{
    using (SqlConnection conn = OpenConnectionForTenant(
        sharding.ShardMap, tenantId4, connStrBldr.ConnectionString))
    {
        SqlCommand cmd = conn.CreateCommand();
        cmd.CommandText = @"SELECT * FROM Blogs";

        Console.WriteLine(@"--
All blogs for TenantId {0} (using ADO.NET SqlClient):", tenantId4);

        SqlDataReader reader = cmd.ExecuteReader();
        while (reader.Read())
        {
            Console.WriteLine("{0}", reader["Name"]);
        }
    }
});

```

## <a name="2-data-tier-create-row-level-security-policy"></a>2. Gegevenslaag: rijniveau beveiligingsbeleid maken

### <a name="create-a-security-policy-to-filter-the-rows-each-tenant-can-access"></a>Een beveiligingsbeleid voor het filteren van de rijen die elke tenant toegang heeft tot maken

Nu dat de toepassing sessie tot stand\_CONTEXT met de huidige TenantId voordat een query naar een beveiligingsbeleid RLS kunt query's en uitsluiten rijen met een andere TenantId filteren.  

RLS is geïmplementeerd in de Transact-SQL. Een door de gebruiker gedefinieerde functie definieert de logica toegang en een beveiligingsbeleid voor deze functie koppelt aan een onbeperkt aantal tabellen. Voor dit project:

1. De functie wordt gecontroleerd dat de toepassing is verbonden met de database en dat de TenantId opgeslagen in de sessie\_CONTEXT komt overeen met de TenantId van een bepaalde rij.
    - De toepassing is verbonden, in plaats van een andere SQL-gebruiker.

2. Een filterpredicaat kan rijen die voldoen aan het filter TenantId doorgeven voor SELECT-, UPDATE en query's verwijderd.
    - Een BLOCK-predicaat voorkomt u dat de rijen die niet voldoen aan het filter niet kan worden ingevoegd of bijgewerkt.
    - Als sessie\_CONTEXT niet is ingesteld, de functie retourneert NULL en geen rijen zijn zichtbaar is of kan worden ingevoegd. 

Schakel RLS op alle shards door de volgende T-SQL met behulp van Visual Studio (SSDT), SSMS of het PowerShell-script dat is opgenomen in het project worden uitgevoerd. Of als u [elastische Database taken](sql-database-elastic-jobs-overview.md), kunt u de uitvoering van deze T-SQL op alle shards automatiseren.

```sql
CREATE SCHEMA rls; -- Separate schema to organize RLS objects.
GO

CREATE FUNCTION rls.fn_tenantAccessPredicate(@TenantId int)     
    RETURNS TABLE     
    WITH SCHEMABINDING
AS
    RETURN SELECT 1 AS fn_accessResult
        -- Use the user in your application’s connection string.
        -- Here we use 'dbo' only for demo purposes!
        WHERE DATABASE_PRINCIPAL_ID() = DATABASE_PRINCIPAL_ID('dbo')
        AND CAST(SESSION_CONTEXT(N'TenantId') AS int) = @TenantId;
GO

CREATE SECURITY POLICY rls.tenantAccessPolicy
    ADD FILTER PREDICATE rls.fn_tenantAccessPredicate(TenantId) ON dbo.Blogs,
    ADD BLOCK  PREDICATE rls.fn_tenantAccessPredicate(TenantId) ON dbo.Blogs,
    ADD FILTER PREDICATE rls.fn_tenantAccessPredicate(TenantId) ON dbo.Posts,
    ADD BLOCK  PREDICATE rls.fn_tenantAccessPredicate(TenantId) ON dbo.Posts;
GO 
```

> [!TIP]
> In een complex project moet u mogelijk het predicaat op honderden tabellen toevoegen, kan dat omslachtig zijn. Er is een helper opgeslagen procedure die automatisch genereert een beveiligingsbeleid en voegt een predicaat voor alle tabellen in een schema. Zie voor meer informatie het blogbericht op [rijniveau beveiliging toepassen op alle tabellen - helper-script (blog)](http://blogs.msdn.com/b/sqlsecurity/archive/2015/03/31/apply-row-level-security-to-all-tables-helper-script).

Als u de voorbeeldtoepassing opnieuw uitvoert, Zie tenants nu alleen rijen die deel uitmaken van deze. Bovendien kan de toepassing niet rijen die deel uitmaken van tenants, andere dan de momenteel verbonden met de shard-database invoegen. De app kan de TenantId in rijen zichtbaar wordt ook niet bijwerken. Als de app probeert te doen, wordt een DbUpdateException gegeven.

Als u later een nieuwe tabel toevoegen, moet u het beveiligingsbeleid als u wilt FILTEREN en BLOCK-predicaten toevoegen op de nieuwe tabel wijzigen.

```sql
ALTER SECURITY POLICY rls.tenantAccessPolicy     
    ADD FILTER PREDICATE rls.fn_tenantAccessPredicate(TenantId) ON dbo.MyNewTable,
    ADD BLOCK  PREDICATE rls.fn_tenantAccessPredicate(TenantId) ON dbo.MyNewTable;
GO 
```

### <a name="add-default-constraints-to-automatically-populate-tenantid-for-inserts"></a>Default-beperkingen om automatisch te voorzien TenantId voor voegt toevoegen

U kunt een default-beperking voor elke tabel de TenantId automatisch gevuld met de waarde die momenteel zijn opgeslagen in de sessie plaatsen\_CONTEXT bij het invoegen van rijen. Hier volgt een voorbeeld. 

```sql
-- Create default constraints to auto-populate TenantId with the
-- value of SESSION_CONTEXT for inserts.
ALTER TABLE Blogs     
    ADD CONSTRAINT df_TenantId_Blogs      
    DEFAULT CAST(SESSION_CONTEXT(N'TenantId') AS int) FOR TenantId;
GO

ALTER TABLE Posts     
    ADD CONSTRAINT df_TenantId_Posts      
    DEFAULT CAST(SESSION_CONTEXT(N'TenantId') AS int) FOR TenantId;
GO 
```

Nu hoeft de toepassing niet op te geven van een TenantId bij het invoegen van rijen: 

```csharp
SqlDatabaseUtils.SqlRetryPolicy.ExecuteAction(() => 
{   
    using (var db = new ElasticScaleContext<int>(
        sharding.ShardMap, tenantId, connStrBldr.ConnectionString))
    {
        // The default constraint sets TenantId automatically!
        var blog = new Blog { Name = name };
        db.Blogs.Add(blog);     
        db.SaveChanges();   
    } 
}); 
```

> [!NOTE]
> Als u een default-beperkingen voor een Entity Framework-project gebruiken, is het raadzaam dat u *niet* de tenant-id-kolom is opgenomen in uw EF-gegevensmodel. Deze aanbeveling is omdat de Entity Framework query automatisch standaardwaarden levering dat voorrang boven de standaardbeperkingen gemaakt in T-SQL met sessie\_CONTEXT.
> Als u de default-beperkingen in het voorbeeldproject, bijvoorbeeld: moet u TenantId verwijderen uit DataClasses.cs (en voer Add-migratie in de Package Manager-Console) en T-SQL om ervoor te zorgen dat het veld alleen in de databasetabellen bestaat. Op deze manier EF automatisch levert onjuiste standaardwaarden bij het invoegen van gegevens.

### <a name="optional-enable-a-superuser-to-access-all-rows"></a>(Optioneel) Schakel een *supergebruiker* voor toegang tot alle rijen

Sommige toepassingen mogelijk wilt maken een *supergebruiker* wie toegang heeft tot alle rijen. Een supergebruiker kan worden ingeschakeld voor alle tenants in alle shards reporting. Of een beheerder kan gesplitste merge bewerkingen uitvoeren op shards die betrekking hebben op tenant rijen verplaatsen tussen databases.

Maak een nieuwe SQL-gebruiker zodat een beheerder (`superuser` in dit voorbeeld) in elke shard-database. Het beveiligingsbeleid met een nieuwe predicaatfunctie waarmee deze gebruiker toegang tot alle rijen vervolgens wijzigen. Naast deze functie krijgt.

```sql
-- New predicate function that adds superuser logic.
CREATE FUNCTION rls.fn_tenantAccessPredicateWithSuperUser(@TenantId int)
    RETURNS TABLE
    WITH SCHEMABINDING
AS
    RETURN SELECT 1 AS fn_accessResult 
        WHERE 
        (
            DATABASE_PRINCIPAL_ID() = DATABASE_PRINCIPAL_ID('dbo') -- Replace 'dbo'.
            AND CAST(SESSION_CONTEXT(N'TenantId') AS int) = @TenantId
        ) 
        OR
        (
            DATABASE_PRINCIPAL_ID() = DATABASE_PRINCIPAL_ID('superuser')
        );
GO

-- Atomically swap in the new predicate function on each table.
ALTER SECURITY POLICY rls.tenantAccessPolicy
    ALTER FILTER PREDICATE rls.fn_tenantAccessPredicateWithSuperUser(TenantId) ON dbo.Blogs,
    ALTER BLOCK  PREDICATE rls.fn_tenantAccessPredicateWithSuperUser(TenantId) ON dbo.Blogs,
    ALTER FILTER PREDICATE rls.fn_tenantAccessPredicateWithSuperUser(TenantId) ON dbo.Posts,
    ALTER BLOCK  PREDICATE rls.fn_tenantAccessPredicateWithSuperUser(TenantId) ON dbo.Posts;
GO
```


### <a name="maintenance"></a>Onderhoud

- **Toevoegen van nieuwe shards**: de T-SQL-script waarmee u RLS op alle nieuwe shards uitvoeren, anders query's in deze shards worden niet gefilterd.
- **Toevoegen van nieuwe tabellen**: een FILTEREN en BLOCK-predicaat toevoegen aan het beveiligingsbeleid op alle shards wanneer een nieuwe tabel is gemaakt. Anders worden query's op de nieuwe tabel niet worden gefilterd. Deze toevoeging kan worden geautomatiseerd met behulp van een DDL-trigger, zoals beschreven in [toepassen beveiliging automatisch op nieuwe tabellen gemaakt (blog)](http://blogs.msdn.com/b/sqlsecurity/archive/2015/05/22/apply-row-level-security-automatically-to-newly-created-tables.aspx).

## <a name="summary"></a>Samenvatting

Elastische database-hulpprogramma's en de beveiliging kunnen worden gebruikt samen voor scale-out van de toepassing gegevenslaag met ondersteuning voor beide multitenant en één tenant shards. Multitenant shards kunnen worden gebruikt voor het opslaan van gegevens efficiënter. Deze efficiëntie wordt verergerd waarbij een groot aantal tenants slechts een paar rijen van de gegevens hebben. Single-tenant shards biedt ondersteuning voor premium-tenants die strenger prestaties en vereisten voor netwerkisolatie.  Zie voor meer informatie [beveiliging verwijzing][rls].

## <a name="additional-resources"></a>Aanvullende bronnen

- [Wat is een Azure elastische groep?](sql-database-elastic-pool.md)
- [Uitbreiden met Azure SQL Database](sql-database-elastic-scale-introduction.md)
- [Ontwerppatronen voor SaaS-toepassingen met meerdere tenants met behulp van Azure SQL Database](saas-tenancy-app-design-patterns.md)
- [Verificatie in multitenant-apps met Azure AD en OpenID Connect](../guidance/guidance-multitenant-identity-authenticate.md)
- [De toepassing Tailspin Surveys](../guidance/guidance-multitenant-identity-tailspin.md)

## <a name="questions-and-feature-requests"></a>Vragen en Functieaanvragen

Voor vragen contact met ons op de [SQL Database-forum](http://social.msdn.microsoft.com/forums/azure/home?forum=ssdsgetstarted). En voeg de functieaanvragen die naar de [forum met feedback van SQL-Database](https://feedback.azure.com/forums/217321-sql-database/).


<!--Image references-->
[1]: ./media/saas-tenancy-elastic-tools-multi-tenant-row-level-security/blogging-app.png
<!--anchors-->
[rls]: https://docs.microsoft.com/sql/relational-databases/security/row-level-security
[s-d-elastic-database-client-library]: sql-database-elastic-database-client-library.md

