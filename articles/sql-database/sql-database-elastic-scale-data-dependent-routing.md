---
title: Gegevens afhankelijke routering met Azure SQL Database | Microsoft Docs
description: Het gebruik van de klasse ShardMapManager in .NET-toepassingen voor gegevensafhankelijke routering, een functie van shard-databases in Azure SQL Database
services: sql-database
documentationcenter: 
manager: jhubbard
author: torsteng
editor: 
ms.assetid: cad09e15-5561-4448-aa18-b38f54cda004
ms.service: sql-database
ms.custom: scale out apps
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/27/2017
ms.author: ddove
ms.openlocfilehash: 6b68bbb0133afd1493acdb58f79f3eeaf6a8d7cd
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="data-dependent-routing"></a>Gegevensafhankelijke routering
**Gegevensafhankelijke routering** is de mogelijkheid om de gegevens in een query voor het routeren van de aanvraag met een juiste database gebruiken. Dit is een fundamenteel patroon bij het werken met shard-databases. De aanvraagcontext kan ook worden gebruikt voor het routeren van de aanvraag, met name als de sleutel sharding geen deel uit van de query maakt. Elke specifieke query of een transactie in een toepassing met gegevensafhankelijke routering is beperkt tot toegang tot een individuele database per aanvraag. Voor de Azure SQL Database elastische hulpprogramma's voor deze routering wordt uitgevoerd met de  **[ShardMapManager klasse](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.aspx)**  in ADO.NET-toepassingen.

De toepassing hoeft niet te volgen verschillende verbindingsreeksen of DB locaties die zijn gekoppeld aan andere delen van gegevens in de shard-omgeving. In plaats daarvan de [Shard kaart Manager](sql-database-elastic-scale-shard-map-management.md) wordt geopend verbindingen met de juiste databases indien nodig, op basis van de gegevens in de shard-toewijzing en de waarde van de sharding-sleutel die het doel van de aanvraag van de toepassing. De sleutel is meestal de *customer_id*, *tenant_id*, *date_key*, of enige andere specifieke id die is een fundamenteel parameter van het verzoek voor database). 

Zie voor meer informatie [schalen van SQL Server met gegevensafhankelijke routering](https://technet.microsoft.com/library/cc966448.aspx).

## <a name="download-the-client-library"></a>Downloaden van de clientbibliotheek
Als u de klasse, installeert u de [clientbibliotheek voor elastische Database](http://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/). 

## <a name="using-a-shardmapmanager-in-a-data-dependent-routing-application"></a>Met behulp van een ShardMapManager in een gegevenstoepassing afhankelijke routering
Het instantiëren van toepassingen moeten de **ShardMapManager** tijdens de initialisatie met behulp van de factory-aanroep  **[GetSQLShardMapManager](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.getsqlshardmapmanager.aspx)**. In dit voorbeeld wordt zowel een **ShardMapManager** en een specifieke **ShardMap** die deze bevat zijn geïnitialiseerd. Dit voorbeeld ziet u de GetSqlShardMapManager en [GetRangeShardMap](https://msdn.microsoft.com/library/azure/dn824173.aspx) methoden.

    ShardMapManager smm = ShardMapManagerFactory.GetSqlShardMapManager(smmConnnectionString, 
                      ShardMapManagerLoadPolicy.Lazy);
    RangeShardMap<int> customerShardMap = smm.GetRangeShardMap<int>("customerMap"); 

### <a name="use-lowest-privilege-credentials-possible-for-getting-the-shard-map"></a>Laagste bevoegdheid referenties mogelijk gebruiken voor het ophalen van de shard-kaart
Als een toepassing de shard-toewijzing zelf niet bewerken is, de referenties in de fabrieksmethode slechts alleen-lezen toegang moeten hebben op de **globale Shard-toewijzing** database. Deze referenties zijn meestal verschillen van referenties die worden gebruikt voor het openen van verbindingen met de shard-toewijzing manager. Zie ook [referenties gebruikt voor toegang tot de clientbibliotheek voor elastische Database](sql-database-elastic-scale-manage-credentials.md). 

## <a name="call-the-openconnectionforkey-method"></a>Roep de methode OpenConnectionForKey
De  **[ShardMap.OpenConnectionForKey methode](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.openconnectionforkey.aspx)**  retourneert een ADO.Net-verbinding gereed is voor het uitgeven van opdrachten op de juiste database op basis van de waarde van de **sleutel** parameter. Shard-gegevens in cache wordt opgeslagen in de toepassing door de **ShardMapManager**, zodat deze aanvragen zoeken in de database op basis van geen doorgaans worden de **globale Shard-toewijzing** database. 

    // Syntax: 
    public SqlConnection OpenConnectionForKey<TKey>(
        TKey key,
        string connectionString,
        ConnectionOptions options
    )


* De **sleutel** parameter wordt gebruikt als een lookup-sleutel in de shard-toewijzing om te bepalen van de juiste database voor de aanvraag. 
* De **connectionString** wordt gebruikt voor het doorgeven van de gebruikersreferenties voor de gewenste verbinding. Er is geen databasenaam of servernaam zijn opgenomen in deze *connectionString* omdat de methode de database en de server met bepaalt de **ShardMap**. 
* De  **[connectionOptions](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.connectionoptions.aspx)**  moet worden ingesteld op **ConnectionOptions.Validate** als een omgeving waar shard toegewezen kan worden gewijzigd en rijen naar andere databases als gevolg van gesplitste of merge-bewerkingen verplaatsen kunnen. Hiervoor moet een korte query naar de lokale shard-toewijzing op de doel-database (en niet naar de globale shard-toewijzing) voordat de verbinding naar de toepassing wordt geleverd. 

Als de validatie op basis van de lokale shard-toewijzing is mislukt (waarmee wordt aangegeven dat de cache onjuist is), query de Shard-toewijzing Manager de globale shard-toewijzing voor het verkrijgen van de nieuwe juiste waarde voor de zoekactie, de cache wordt bijgewerkt en opvragen en verbinding met de juiste database geretourneerd. 

Gebruik **ConnectionOptions.None** alleen wanneer shard-toewijzing wijzigingen wordt niet verwacht terwijl een toepassing online is. In dat geval waarden in het cachegeheugen altijd alleen correct kunnen worden aangenomen en de aanroep extra round trip validatie van de doeldatabase veilig worden overgeslagen. Dat vermindert databaseverkeer. De **connectionOptions** kan ook worden ingesteld via een waarde in een configuratiebestand om aan te geven dat sharding wijzigingen worden verwacht of tijdens een bepaalde tijd niet.  

In dit voorbeeld wordt de waarde van de sleutel van een geheel getal **CustomerID**met een **ShardMap** object met de naam **customerShardMap**.  

    int customerId = 12345; 
    int newPersonId = 4321; 

    // Connect to the shard for that customer ID. No need to call a SqlConnection 
    // constructor followed by the Open method.
    using (SqlConnection conn = customerShardMap.OpenConnectionForKey(customerId, 
        Configuration.GetCredentialsConnectionString(), ConnectionOptions.Validate)) 
    { 
        // Execute a simple command. 
        SqlCommand cmd = conn.CreateCommand(); 
        cmd.CommandText = @"UPDATE Sales.Customer 
                            SET PersonID = @newPersonID 
                            WHERE CustomerID = @customerID"; 

        cmd.Parameters.AddWithValue("@customerID", customerId); 
        cmd.Parameters.AddWithValue("@newPersonID", newPersonId); 
        cmd.ExecuteNonQuery(); 
    }  

De **OpenConnectionForKey** methode retourneert een nieuwe al open verbinding met de juiste database. Verbindingen gebruikt op deze manier kunnen nog steeds profiteren van de ADO.Net verbindingsgroepering. Zolang transacties en aanvragen kunnen worden voldaan door één shard tegelijk, moet dit de enige aanpassing die in een toepassing die al met ADO.Net nodig. 

De  **[OpenConnectionForKeyAsync methode](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.openconnectionforkeyasync.aspx)**  is ook beschikbaar als uw toepassing gebruik van asynchrone programmering met ADO.Net maakt. Het gedrag is de afhankelijke routering equivalent met ADO gegevens. De NET  **[Connection.OpenAsync](https://msdn.microsoft.com/library/hh223688\(v=vs.110\).aspx)**  methode.

## <a name="integrating-with-transient-fault-handling"></a>Integratie met afhandeling van tijdelijke fout
Er is een best practice bij het ontwikkelen van de data access-toepassingen in de cloud om ervoor te zorgen dat de tijdelijke fouten zijn opgepikt door de app en dat de bewerkingen zijn niet opnieuw geprobeerd meerdere keren voordat er wordt een fout opgetreden. Tijdelijke fout verwerking voor cloud-toepassingen is besproken in [afhandeling van tijdelijke fout](https://msdn.microsoft.com/library/dn440719\(v=pandp.60\).aspx). 

Afhandeling van tijdelijke fout kan natuurlijk worden gecombineerd met het patroon gegevensafhankelijke routering. De belangrijkste vereiste is om opnieuw te proberen de gehele toegang aanvraag, waaronder de **met** blok die de gegevensafhankelijke routering verbinding hebt verkregen. Het bovenstaande voorbeeld kan worden herschreven als volgt (Opmerking gemarkeerd wijzigen). 

### <a name="example---data-dependent-routing-with-transient-fault-handling"></a>Voorbeeld - afhankelijke routering met tijdelijke fout afhandeling van gegevens
<pre><code>int customerId = 12345; 
int newPersonId = 4321; 

<span style="background-color:  #FFFF00">Configuration.SqlRetryPolicy.ExecuteAction(() =&gt; </span> 
<span style="background-color:  #FFFF00">    { </span>
        // Connect to the shard for a customer ID. 
        using (SqlConnection conn = customerShardMap.OpenConnectionForKey(customerId,  
        Configuration.GetCredentialsConnectionString(), ConnectionOptions.Validate)) 
        { 
            // Execute a simple command 
            SqlCommand cmd = conn.CreateCommand(); 

            cmd.CommandText = @&quot;UPDATE Sales.Customer 
                            SET PersonID = @newPersonID 
                            WHERE CustomerID = @customerID&quot;; 

            cmd.Parameters.AddWithValue(&quot;@customerID&quot;, customerId); 
            cmd.Parameters.AddWithValue(&quot;@newPersonID&quot;, newPersonId); 
            cmd.ExecuteNonQuery(); 

            Console.WriteLine(&quot;Update completed&quot;); 
        } 
<span style="background-color:  #FFFF00">    }); </span> 
</code></pre>


Pakketten die nodig zijn voor het implementeren van afhandeling van tijdelijke fout worden automatisch gedownload als u de voorbeeldtoepassing elastische database. Pakketten zijn ook verkrijgbaar afzonderlijk via [Enterprise Library - tijdelijke fout afhandeling van Application Block](http://www.nuget.org/packages/EnterpriseLibrary.TransientFaultHandling/). Versie 6.0 of hoger gebruiken. 

## <a name="transactional-consistency"></a>Transactionele consistentie
Transactionele eigenschappen zijn gegarandeerd voor alle bewerkingen die lokaal op een shard. Bijvoorbeeld, uitvoeren transacties die zijn verzonden via het gegevensafhankelijke routering binnen het bereik van de shard doel voor de verbinding. Op dit moment zijn er geen mogelijkheden voor het opnemen van meerdere verbindingen in een transactie en er zijn daarom geen transactionele garanties met betrekking tot de bewerkingen die worden uitgevoerd via shards.

## <a name="next-steps"></a>Volgende stappen
Zie een shard loskoppelen of opnieuw koppelen van een shard [met behulp van de klasse RecoveryManager shard kaart problemen op te lossen](sql-database-elastic-database-recovery-manager.md)

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

