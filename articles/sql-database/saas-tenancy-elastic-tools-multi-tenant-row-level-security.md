---
title: Apps met beveiliging op Rijniveau en hulpmiddelen voor elastic database met meerdere tenants | Microsoft Docs
description: Gebruik hulpprogramma's voor elastische databases met beveiliging op rijniveau om een toepassing met een zeer schaalbare gegevenslaag.
services: sql-database
ms.service: sql-database
ms.subservice: scenario
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: VanMSFT
ms.author: vanto
ms.reviewer: ''
manager: craigg
ms.date: 04/01/2018
ms.openlocfilehash: 6d701878886cb1d5cc20a57614a474537f06a728
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51242905"
---
# <a name="multi-tenant-applications-with-elastic-database-tools-and-row-level-security"></a>Multitenant-toepassingen met elastische Databasehulpprogramma's en beveiliging op rijniveau

[Hulpmiddelen voor elastic database](sql-database-elastic-scale-get-started.md) en [beveiliging op rijniveau (RLS)] [ rls] werken samen om het inschakelen van de gegevenslaag van een toepassing met meerdere tenants met Azure SQL Database schalen. Deze technologieën helpen samen u een toepassing bouwt die een zeer schaalbare gegevenslaag heeft. De gegevenslaag biedt ondersteuning voor multitenant shards en maakt gebruik van **ADO.NET SqlClient** of **Entity Framework**. Zie voor meer informatie, [ontwerppatronen voor multitenant SaaS-toepassingen met Azure SQL Database](saas-tenancy-app-design-patterns.md).

- **Hulpmiddelen voor elastic database** kunnen ontwikkelaars de gegevenslaag standard sharding-praktijken, uitschalen met behulp van .NET-bibliotheken en sjablonen voor Azure-service. Shards beheren met behulp van de [elastische Database-clientbibliotheek] [ s-d-elastic-database-client-library] helpt automatiseren en stroomlijnen veel van de infrastructurele taken die meestal gepaard gaat met sharding.
- **Beveiliging op rijniveau** biedt ontwikkelaars de mogelijkheid voor het veilig opslaan van gegevens voor meerdere tenants in dezelfde database. Beveiligingsbeleid voor beveiliging op Rijniveau filteren rijen die geen deel uitmaken van de tenant die een query uitvoert. Het filter de bedrijfslogica in de database centraliseren vereenvoudigt het onderhoud en vermindert het risico van een beveiligingsfout. De alternatieve van afhankelijk zijn van alle clientcode om af te dwingen beveiliging is riskant.

Door deze functies samen gebruikt, kan een toepassing gegevens opslaan voor meerdere tenants in dezelfde shard-database. Het kost minder per tenant als de tenants een database deelt. Nog kunt dezelfde toepassing bieden ook de premium-tenants de mogelijkheid om betalen voor hun eigen toegewezen shard met één tenant. Een voordeel van isolatie van één tenant is prestatiegaranties verhogen. In een database met één tenant is er geen andere tenant, en dingen om de resources.

Het doel is om de elastische database-clientbibliotheek gebruiken [gegevensafhankelijke routering](sql-database-elastic-scale-data-dependent-routing.md) API's voor elke opgegeven tenant wordt automatisch verbinding maken met de juiste shard-database. Slechts één shard bevat bepaalde tenant-id-waarde voor de opgegeven tenant. De tenant-id is de *sharding-sleutel*. Nadat de verbinding tot stand is gebracht, wordt een beveiligingsbeleid voor beveiliging op Rijniveau in de database zorgt ervoor dat de opgegeven tenant toegang heeft tot alleen rijen met gegevens die de tenant-id bevatten.

> [!NOTE]
> De tenant-id kan bestaan uit meer dan één kolom. Voor gebruiksgemak is van deze discussie, veronderstellen we eens een TenantId één kolom.

![Blog app-architectuur][1]

## <a name="download-the-sample-project"></a>Download het voorbeeldproject

### <a name="prerequisites"></a>Vereisten

- Gebruik Visual Studio (2012 of hoger)
- Drie Azure SQL-databases maken
- Download voorbeeldproject: [elastische DB-hulpprogramma's voor Azure SQL - Multitenant Shards](https://go.microsoft.com/?linkid=9888163)
  - Vul de gegevens voor uw databases aan het begin van **Program.cs** 

Dit project wordt uitgebreid beschreven in een [elastische DB-hulpprogramma's voor Azure SQL - integratie met Entity Framework](sql-database-elastic-scale-use-entity-framework-applications-visual-studio.md) door het toevoegen van ondersteuning voor multitenant shard-databases. Het project bouwt een eenvoudige consoletoepassing voor het maken van blogs en berichten. Het project omvat vier tenants, plus twee multitenant shard-databases. Deze configuratie wordt weergegeven in het voorgaande diagram. 

Ontwikkel en voer de toepassing. Deze uitvoering bootstrapt de hulpmiddelen voor elastic database shard-Toewijzingsbeheer en voert de volgende tests: 

1. Met behulp van Entity Framework en LINQ, een nieuw blog maken en vervolgens alle blogs voor elke tenant weergeven
2. Alle blogs voor een tenant met behulp van ADO.NET SqlClient weergeven
3. Probeer in te voegen een blog voor de verkeerde tenant om te controleren of dat er een fout is gegenereerd  

U ziet dat omdat beveiliging op Rijniveau is nog niet ingeschakeld in de shard-databases, elk van deze tests een probleem opgetreden blijkt: tenants zijn blogs die niet tot deze behoren zien en de toepassing wordt niet voorkomen dat een blog invoegen voor de verkeerde tenant. De rest van dit artikel wordt beschreven hoe u deze problemen oplossen door het afdwingen van tenantisolatie beveiliging op rijniveau. Er zijn twee stappen: 

1. **Toepassingslaag**: wijzigen van de toepassingscode om in te stellen altijd de huidige tenant-id in de sessie\_CONTEXT na het openen van een verbinding. Het voorbeeldproject wordt de tenant-id al ingesteld op deze manier. 
2. **Gegevenslaag**: maken van een beveiligingsbeleid voor beveiliging op Rijniveau in elke shard-database rijen filteren op basis van de tenant-id die zijn opgeslagen in de sessie\_CONTEXT. Maak een beleid voor elk van de shard-databases, anders rijen in multitenant shards worden niet gefilterd. 

## <a name="1-application-tier-set-tenantid-in-the-sessioncontext"></a>1. Toepassingslaag: Set-tenant-id in de sessie\_CONTEXT

Eerst verbinding u met een shard-database met behulp van de API's gegevensafhankelijke routering van de clientbibliotheek van elastische database. De toepassing nog steeds moet op de hoogte de database die tenant-id maakt gebruik van de verbinding. De tenant-id wordt het beveiligingsbeleid voor beveiliging op Rijniveau aangegeven welke rijen moeten worden gefilterd als behorend tot andere tenants. De huidige tenant-id in Store de [sessie\_CONTEXT](https://docs.microsoft.com/sql/t-sql/functions/session-context-transact-sql) van de verbinding.

Een alternatief voor sessie\_CONTEXT is het gebruik van [CONTEXT\_INFO](https://docs.microsoft.com/sql/t-sql/functions/context-info-transact-sql). Maar sessie\_CONTEXT is een betere optie. SESSIE\_CONTEXT is eenvoudiger te gebruiken, wordt standaard NULL en het sleutel / waarde-paren ondersteunt.

### <a name="entity-framework"></a>Entity Framework

Voor toepassingen met behulp van Entity Framework, de beste aanpak is om in te stellen van de sessie\_CONTEXT binnen de ElasticScaleContext onderdrukking wordt beschreven in [gegevensafhankelijke routering met EF DbContext](sql-database-elastic-scale-use-entity-framework-applications-visual-studio.md#data-dependent-routing-using-ef-dbcontext). Maken en uitvoeren van een SqlCommand die tenant-id ingesteld in de sessie\_CONTEXT voor de shardingKey opgegeven voor de verbinding. Retourneert de verbinding die wordt geleverd door gegevensafhankelijke routering. Op deze manier hoeft u alleen te één keer code schrijven om in te stellen van de sessie\_CONTEXT. 

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

Nu de sessie\_CONTEXT wordt automatisch ingesteld met de opgegeven tenant-id wanneer ElasticScaleContext wordt aangeroepen: 

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

Maak een functie wrapper rond methode ShardMap.OpenConnectionForKey voor toepassingen met behulp van ADO.NET SqlClient. De tenant-ID automatisch ingesteld in de sessie-wrapper hebt\_CONTEXT voor de huidige TenantId alvorens een verbinding. Om ervoor te zorgen die sessie\_CONTEXT is altijd ingesteld, moet u alleen verbindingen met behulp van deze functie wrapper openen.

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

## <a name="2-data-tier-create-row-level-security-policy"></a>2. Gegevenslaag: Maak beleid voor beveiliging op rijniveau

### <a name="create-a-security-policy-to-filter-the-rows-each-tenant-can-access"></a>Een beveiligingsbeleid om te filteren van de rijen die toegang elke tenant tot hebben maken

Nu dat de toepassing is bezig met het sessie\_CONTEXT met de huidige tenant-id voor het uitvoeren van query's, een beveiligingsbeleid voor beveiliging op Rijniveau kunt filteren, query's en uitsluiten rijen met een andere tenant-id.  

Beveiliging op Rijniveau is geïmplementeerd in de Transact-SQL. Een door de gebruiker gedefinieerde functie definieert de logica en een beveiligingsbeleid voor deze functie koppelt aan een onbeperkt aantal tabellen. Voor dit project:

1. De functie wordt gecontroleerd dat de toepassing is verbonden met de database, en dat de tenant-id opgeslagen in de sessie\_CONTEXT overeenkomt met de tenant-id van een bepaalde rij.
    - De toepassing is verbonden, in plaats van een andere SQL-gebruiker.

2. Een filterpredicaat kunt rijen die voldoen aan het filter van de tenant-id doorgeven voor de SELECT-, UPDATE en query's verwijderen.
    - Een BLOCK-predicaat voorkomt u dat rijen die niet voldoen aan het filter wordt ingevoegd of bijgewerkt.
    - Als sessie\_CONTEXT niet is ingesteld, wordt er NULL geretourneerd in de functie en geen rijen zijn zichtbaar of kunnen worden ingevoegd. 

Om in te schakelen RLS op alle shards, uitvoeren van de volgende T-SQL met behulp van Visual Studio (SSDT), SSMS of het PowerShell-script dat is opgenomen in het project. Of als u gebruikmaakt van [elastische databasetaken](sql-database-elastic-jobs-overview.md), kunt u de uitvoering van deze T-SQL in alle shards automatiseren.

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
> In een complexe project moet u mogelijk het predicaat op honderden tabellen toevoegen, die mogelijk moeite. Er is een helper opgeslagen procedure die automatisch genereert een beveiligingsbeleid en voegt een predicaat toe op alle tabellen in een schema. Zie voor meer informatie het blogbericht op [Row-Level Security toepassen op alle tabellen - helper-script (blog)](https://blogs.msdn.com/b/sqlsecurity/archive/2015/03/31/apply-row-level-security-to-all-tables-helper-script).

Als u opnieuw de voorbeeldtoepassing uitvoert, Zie tenants nu alleen rijen die deel uitmaken van deze. De toepassing kan niet verder, invoegen van rijen die deel uitmaken van tenants, andere zijn dan de momenteel verbonden met de database voor shard. De app kan de tenant-id in alle rijen zichtbaar wordt bovendien niet bijwerken. Als de app probeert te doen, wordt een DbUpdateException gegeven.

Als u later een nieuwe tabel toevoegen, wijzigt u het beveiligingsbeleid om te FILTEREN en BLOCK-predicaten op de nieuwe tabel toevoegen.

```sql
ALTER SECURITY POLICY rls.tenantAccessPolicy     
    ADD FILTER PREDICATE rls.fn_tenantAccessPredicate(TenantId) ON dbo.MyNewTable,
    ADD BLOCK  PREDICATE rls.fn_tenantAccessPredicate(TenantId) ON dbo.MyNewTable;
GO 
```

### <a name="add-default-constraints-to-automatically-populate-tenantid-for-inserts"></a>Default-beperkingen om automatisch te TenantId voor voegt toevoegen

U kunt een default-beperking voor elke tabel voor het automatisch vullen van de tenant-id met de waarde die momenteel zijn opgeslagen in de sessie plaatsen\_CONTEXT bij het invoegen van rijen. Hier volgt een voorbeeld. 

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

De toepassing heeft nu niet nodig om op te geven van een tenant-id bij het invoegen van rijen: 

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
> Als u default-beperkingen voor een project Entity Framework gebruikt, is het raadzaam dat u *niet* de tenant-id-kolom is opgenomen in uw EF-gegevensmodel. Deze aanbeveling is omdat Entity Framework query's automatisch aanbod standaardwaarden die overschrijven de standaardbeperkingen gemaakt in T-SQL met sessie\_CONTEXT.
> Voor het gebruik van default-beperkingen in het voorbeeldproject, bijvoorbeeld: moet u TenantId verwijderen uit DataClasses.cs (en voer toevoegen-migratie in de Package Manager Console) en T-SQL gebruiken om ervoor te zorgen dat het veld alleen in de databasetabellen bestaat. Op deze manier EF automatisch levert onjuist standaardwaarden bij het invoegen van gegevens.

### <a name="optional-enable-a-superuser-to-access-all-rows"></a>(Optioneel) Schakel een *superuser* voor toegang tot alle rijen

Sommige toepassingen mogelijk wilt maken een *superuser* wie toegang heeft tot alle rijen. Een supergebruiker kan melden voor alle tenants op alle shards inschakelen. Of een supergebruiker kan splitsen en samenvoegen bewerkingen uitvoeren op shards die betrekking hebben op tenant rijen verplaatsen tussen databases.

Als u wilt een supergebruiker inschakelen, maakt u een nieuwe SQL-gebruiker (`superuser` in dit voorbeeld) in elke shard-database. Vervolgens het beleid met een nieuwe predikaatfunctie die deze gebruiker toegang tot alle rijen wijzigen. Vervolgens krijgt een dergelijke functie.

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

- **Toevoegen van nieuwe shards**: de T-SQL-script waarmee u beveiliging op Rijniveau op alle nieuwe shards uitvoeren, anders query's in deze shards worden niet gefilterd.
- **Nieuwe tabellen toe te voegen**: een FILTEREN en BLOCK-predicaat toevoegen aan het beveiligingsbeleid op alle shards worden gewijzigd wanneer er een nieuwe tabel wordt gemaakt. Query's op de nieuwe tabel zijn anders niet gefilterd. Deze toevoeging kan worden geautomatiseerd met behulp van een DDL-trigger, zoals beschreven in [toepassen Row-Level Security automatisch naar de zojuist gemaakte tabellen (blog)](https://blogs.msdn.com/b/sqlsecurity/archive/2015/05/22/apply-row-level-security-automatically-to-newly-created-tables.aspx).

## <a name="summary"></a>Samenvatting

Hulpmiddelen voor elastic database en beveiliging op rijniveau kunnen worden gebruikt samen voor het opschalen van de gegevenslaag van een toepassing met ondersteuning voor beide meerdere tenants en één tenant shards. Multitenant shards kunnen worden gebruikt om gegevens efficiënter opslaan. Deze efficiëntie wordt uitgesproken waarbij een groot aantal tenants slechts een paar rijen met gegevens hebben. Shards van één tenant biedt ondersteuning voor premium-tenants waarvoor strengere prestaties en vereisten voor netwerkisolatie.  Zie voor meer informatie, [Row-Level Security verwijzing][rls].

## <a name="additional-resources"></a>Aanvullende resources

- [Wat is een elastische groep van Azure?](sql-database-elastic-pool.md)
- [Uitbreiden met Azure SQL Database](sql-database-elastic-scale-introduction.md)
- [Ontwerppatronen voor SaaS-toepassingen met meerdere tenants met behulp van Azure SQL Database](saas-tenancy-app-design-patterns.md)
- [Verificatie in multitenant-apps met Azure AD en OpenID Connect](../guidance/guidance-multitenant-identity-authenticate.md)
- [De toepassing Tailspin Surveys](../guidance/guidance-multitenant-identity-tailspin.md)

## <a name="questions-and-feature-requests"></a>Vragen en Functieaanvragen

Voor vragen contact met ons op de [forum van SQL-Database](https://social.msdn.microsoft.com/forums/azure/home?forum=ssdsgetstarted). En voeg de functieaanvragen die naar de [forum met feedback van SQL-Database](https://feedback.azure.com/forums/217321-sql-database/).


<!--Image references-->
[1]: ./media/saas-tenancy-elastic-tools-multi-tenant-row-level-security/blogging-app.png
<!--anchors-->
[rls]: https://docs.microsoft.com/sql/relational-databases/security/row-level-security
[s-d-elastic-database-client-library]: sql-database-elastic-database-client-library.md

