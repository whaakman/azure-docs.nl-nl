---
title: Gegevensafhankelijke routering met Azure SQL Database | Microsoft Docs
description: Over het gebruik van de klasse ShardMapManager in .NET-apps voor gegevensafhankelijke routering, een functie van de shard-databases in Azure SQL Database
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
ms.openlocfilehash: 46febbeb2675c38bf68c6ba0b911f799b268e208
ms.sourcegitcommit: d4f728095cf52b109b3117be9059809c12b69e32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/10/2019
ms.locfileid: "54201105"
---
# <a name="use-data-dependent-routing-to-route-a-query-to-appropriate-database"></a>Gegevensafhankelijke routering om te routeren van een query naar de juiste database gebruiken

**Gegevensafhankelijke routering** is de mogelijkheid om de gegevens in een query gebruiken voor het routeren van de aanvraag met een juiste database. Gegevens-afhankelijke routering is een fundamenteel patroon bij het werken met shard-databases. De context van de aanvraag kan ook worden gebruikt voor het routeren van de aanvraag, met name als de sharding-sleutel geen deel uit van de query maakt. Elke specifieke query of een transactie in een toepassing met behulp van gegevensafhankelijke routering is beperkt tot het openen van een individuele database per aanvraag. Voor de Azure SQL Database Elastic-hulpprogramma's, deze routering wordt uitgevoerd met de **ShardMapManager** ([Java](/java/api/com.microsoft.azure.elasticdb.shard.mapmanager.shardmapmanager), [.NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager)) klasse.

De toepassing hoeft niet te volgen verschillende tekenreeksen voor databaseverbindingen of DB-locaties die zijn gekoppeld aan verschillende delen van gegevens in de shard-omgeving. In plaats daarvan de [Shard-Toewijzingsbeheer](sql-database-elastic-scale-shard-map-management.md) verbindingen naar de juiste databases wanneer dat nodig is, wordt geopend op basis van de gegevens in de shard-toewijzing en de waarde van de sharding-sleutel die het doel van de aanvraag van de toepassing. De sleutel is doorgaans de *customer_id*, *tenant_id*, *date_key*, of enige andere specifieke id die is een fundamenteel parameter van de aanvraag van de database.

Zie voor meer informatie, [schalen van SQL Server met gegevensafhankelijke routering](https://technet.microsoft.com/library/cc966448.aspx).

## <a name="download-the-client-library"></a>Downloaden van de clientbibliotheek

Downloaden:

* De Java-versie van de bibliotheek, Zie [Maven Central Repository](https://search.maven.org/#search%7Cga%7C1%7Celastic-db-tools).
* De .NET-versie van de bibliotheek, Zie [NuGet](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/).

## <a name="using-a-shardmapmanager-in-a-data-dependent-routing-application"></a>Met behulp van een ShardMapManager in een toepassing voor gegevensafhankelijke routering

Toepassingen moeten exemplaar maken van de **ShardMapManager** tijdens de initialisatie met behulp van de aanroep factory **GetSQLShardMapManager** ([Java](/java/api/com.microsoft.azure.elasticdb.shard.mapmanager.shardmapmanagerfactory.getsqlshardmapmanager), [.NET ](https://docs.microsoft.com/dotnet/api/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.getsqlshardmapmanager)). In dit voorbeeld wordt zowel een **ShardMapManager** en een specifieke **ShardMap** die deze bevat worden geïnitialiseerd. In dit voorbeeld ziet u de GetSqlShardMapManager en GetRangeShardMap ([Java](/java/api/com.microsoft.azure.elasticdb.shard.mapmanager.shardmapmanager.getrangeshardmap), [.NET](https://docs.microsoft.com/previous-versions/azure/dn824173(v=azure.100))) methoden.

```Java
ShardMapManager smm = ShardMapManagerFactory.getSqlShardMapManager(connectionString, ShardMapManagerLoadPolicy.Lazy);
RangeShardMap<int> rangeShardMap = smm.getRangeShardMap(Configuration.getRangeShardMapName(), ShardKeyType.Int32);
```

```csharp
ShardMapManager smm = ShardMapManagerFactory.GetSqlShardMapManager(smmConnectionString, ShardMapManagerLoadPolicy.Lazy);
RangeShardMap<int> customerShardMap = smm.GetRangeShardMap<int>("customerMap"); 
```

### <a name="use-lowest-privilege-credentials-possible-for-getting-the-shard-map"></a>Laagste bevoegdheden referenties kan gebruiken voor het ophalen van de shard-toewijzing

Als een toepassing niet de shard-toewijzing zelf bewerken is, de referenties die worden gebruikt in de fabrieksmethode alleen-lezen machtigingen moeten hebben op de **globale Shard-toewijzing** database. Deze referenties zijn doorgaans verschillend van de referenties die worden gebruikt voor het openen van verbindingen met de shard-Toewijzingsbeheer van. Zie ook [referenties gebruikt voor toegang tot de clientbibliotheek voor Elastic Database](sql-database-elastic-scale-manage-credentials.md).

## <a name="call-the-openconnectionforkey-method"></a>Roep de methode OpenConnectionForKey

De **ShardMap.OpenConnectionForKey methode** ([Java](/java/api/com.microsoft.azure.elasticdb.shard.mapper.listshardmapper.openconnectionforkey), [.NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.openconnectionforkey)) retourneert een verbinding gereed is voor het uitgeven van opdrachten met de juiste database op basis van de waarde van de **sleutel** parameter. Shard-gegevens opgeslagen in de cache in de toepassing door de **ShardMapManager**, zodat deze aanvragen gaat het meestal niet zoeken in de database op basis van doen om de **globale Shard-toewijzing** database.

```Java
// Syntax:
public Connection openConnectionForKey(Object key, String connectionString, ConnectionOptions options)
```

```csharp
// Syntax:
public SqlConnection OpenConnectionForKey<TKey>(TKey key, string connectionString, ConnectionOptions options)
```

* De **sleutel** parameter wordt gebruikt als een lookup-sleutel in de shard-toewijzing te bepalen van de juiste database voor de aanvraag.
* De **connectionString** wordt gebruikt om door te geven alleen de gebruikersreferenties voor de gewenste verbinding. Er is geen databasenaam of de servernaam is opgenomen in deze *connectionString* omdat de methode de database en de server met bepaalt de **ShardMap**.
* De **connectionOptions** ([Java](/java/api/com.microsoft.azure.elasticdb.shard.mapper.connectionoptions), [.NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.sqldatabase.elasticscale.shardmanagement.connectionoptions)) moet worden ingesteld op **ConnectionOptions.Validate** als een omgeving waar de mei voor shard-kaarten wijzigings- en rijen kunnen verplaatsen naar andere databases als gevolg van bewerkingen voor splitsen of samenvoegen. Deze validatie betreft een korte query naar de lokale shard-toewijzing op de doel-database (en niet naar de globale shard-toewijzing) voordat de verbinding aan de toepassing wordt geleverd.

Als de validatie op basis van de lokale shard-toewijzing is mislukt (waarmee wordt aangegeven dat de cache onjuist is), de globale shard-toewijzing voor het verkrijgen van de nieuwe juiste waarde voor de zoekopdracht, bijwerken van de cache en verkrijgen en retourneren van verbinding met de juiste database query uitgevoerd op de Shard-Toewijzingsbeheer .

Gebruik **ConnectionOptions.None** alleen wanneer wijzigingen voor shard-toewijzing wordt niet verwacht terwijl een toepassing online is. In dat geval de waarden in de cache altijd alleen correct kunnen worden aangenomen en de extra traject validatie-aanroep naar de doeldatabase veilig kan worden overgeslagen. Dat vermindert databaseverkeer. De **connectionOptions** kan ook worden ingesteld via een waarde in een configuratiebestand om aan te geven of sharding wijzigingen worden verwacht of niet tijdens een bepaalde periode.  

In dit voorbeeld wordt de waarde van een geheel getal met sleutel **CustomerID**, met een **ShardMap** object met de naam **customerShardMap**.  

```Java
int customerId = 12345;
int productId = 4321;
// Looks up the key in the shard map and opens a connection to the shard
try (Connection conn = shardMap.openConnectionForKey(customerId, Configuration.getCredentialsConnectionString())) {
    // Create a simple command that will insert or update the customer information
    PreparedStatement ps = conn.prepareStatement("UPDATE Sales.Customer SET PersonID = ? WHERE CustomerID = ?");

    ps.setInt(1, productId);
    ps.setInt(2, customerId);
    ps.executeUpdate();
} catch (SQLException e) {
    e.printStackTrace();
}
```

```csharp
int customerId = 12345;
int newPersonId = 4321;

// Connect to the shard for that customer ID. No need to call a SqlConnection
// constructor followed by the Open method.
using (SqlConnection conn = customerShardMap.OpenConnectionForKey(customerId, Configuration.GetCredentialsConnectionString(), ConnectionOptions.Validate))
{
    // Execute a simple command.
    SqlCommand cmd = conn.CreateCommand();
    cmd.CommandText = @"UPDATE Sales.Customer
                        SET PersonID = @newPersonID WHERE CustomerID = @customerID";

    cmd.Parameters.AddWithValue("@customerID", customerId);cmd.Parameters.AddWithValue("@newPersonID", newPersonId);
    cmd.ExecuteNonQuery();
}  
```

De **OpenConnectionForKey** methode retourneert een nieuwe al open verbinding met de juiste database. Verbindingen gebruikt op deze manier kunnen nog steeds profiteren van groepsgewijze verbinding.

De **OpenConnectionForKeyAsync methode** ([Java](/java/api/com.microsoft.azure.elasticdb.shard.mapper.listshardmapper.openconnectionforkeyasync), [.NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.openconnectionforkeyasync)) is ook beschikbaar als uw toepassing gebruik asynchrone programmering.

## <a name="integrating-with-transient-fault-handling"></a>Integreren met afhandeling van tijdelijke fouten

Er is een best practice bij het ontwikkelen van toepassingen voor toegang in de cloud om ervoor te zorgen dat tijdelijke fouten zijn opgepikt door de app en dat de bewerkingen worden niet opnieuw geprobeerd meerdere keren voordat er een fout. Tijdelijke fouten afhandelen voor cloud-toepassingen is besproken in de afhandeling van tijdelijke fouten ([Java](/java/api/com.microsoft.azure.elasticdb.core.commons.transientfaulthandling), [.NET](https://docs.microsoft.com/previous-versions/msp-n-p/dn440719(v=pandp.60))).

Afhandeling van tijdelijke fouten kan op een natuurlijke manier worden gecombineerd met het patroon gegevensafhankelijke routering. De belangrijkste vereiste is om opnieuw te proberen de volledige gegevens toegang aanvraag met inbegrip van de **met behulp van** blokkeren die de gegevensafhankelijke routering verbinding hebt verkregen. Het vorige voorbeeld kan als volgt worden herschreven.

### <a name="example---data-dependent-routing-with-transient-fault-handling"></a>Voorbeeld: gegevensafhankelijke routering met afhandeling van tijdelijke fouten

```Java
int customerId = 12345;
int productId = 4321;
try {
    SqlDatabaseUtils.getSqlRetryPolicy().executeAction(() -> {
        // Looks up the key in the shard map and opens a connection to the shard
        try (Connection conn = shardMap.openConnectionForKey(customerId, Configuration.getCredentialsConnectionString())) {
            // Create a simple command that will insert or update the customer information
            PreparedStatement ps = conn.prepareStatement("UPDATE Sales.Customer SET PersonID = ? WHERE CustomerID = ?");

            ps.setInt(1, productId);
            ps.setInt(2, customerId);
            ps.executeUpdate();
        } catch (SQLException e) {
            e.printStackTrace();
        }
    });
} catch (Exception e) {
    throw new StoreException(e.getMessage(), e);
}
```

```csharp
int customerId = 12345;
int newPersonId = 4321;

Configuration.SqlRetryPolicy.ExecuteAction(() =&gt;
{
    // Connect to the shard for a customer ID.
    using (SqlConnection conn = customerShardMap.OpenConnectionForKey(customerId, Configuration.GetCredentialsConnectionString(), ConnectionOptions.Validate))
    {
        // Execute a simple command
        SqlCommand cmd = conn.CreateCommand();

        cmd.CommandText = @"UPDATE Sales.Customer
                            SET PersonID = @newPersonID
                            WHERE CustomerID = @customerID";

        cmd.Parameters.AddWithValue("@customerID", customerId);
        cmd.Parameters.AddWithValue("@newPersonID", newPersonId);
        cmd.ExecuteNonQuery();

        Console.WriteLine("Update completed");
    }
});
```

Pakketten die nodig zijn voor het implementeren van de afhandeling van tijdelijke fouten worden automatisch gedownload als u de voorbeeldtoepassing voor elastische database maken.

## <a name="transactional-consistency"></a>Transactionele consistentie

Transactionele eigenschappen worden gegarandeerd voor alle bewerkingen die lokaal op een shard. Bijvoorbeeld, uitvoeren van transacties die zijn verzonden via gegevensafhankelijke routering binnen het bereik van de shard doel voor de verbinding. Op dit moment geen functionaliteit voor het opnemen van meerdere verbindingen in een transactie zijn en er zijn daarom geen transactionele garanties met betrekking tot bewerkingen die worden uitgevoerd in verschillende shards.

## <a name="next-steps"></a>Volgende stappen

Een shard los te koppelen of te koppelen van een shard, Zie [problemen van shardtoewijzingen met de RecoveryManager-klasse](sql-database-elastic-database-recovery-manager.md)

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]