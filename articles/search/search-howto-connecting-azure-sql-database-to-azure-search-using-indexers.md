---
title: Verbinding maken met Azure SQL-Database naar Azure Search met behulp van indexeerfuncties | Microsoft Docs
description: Informatie over het ophalen van gegevens uit Azure SQL Database naar een Azure Search-index met de indexeerfuncties.
services: search
documentationcenter: 
author: chaosrealm
manager: pablocas
editor: 
ms.assetid: e9bbf352-dfff-4872-9b17-b1351aae519f
ms.service: search
ms.devlang: rest-api
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 07/13/2017
ms.author: eugenesh
ms.openlocfilehash: 49f614fdf3ba84de238139387ea97ee62077b072
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="connecting-azure-sql-database-to-azure-search-using-indexers"></a>Verbinding maken met Azure SQL Database in Azure Search met indexeerfuncties

Voordat u kunt een query een [Azure Search-index](search-what-is-an-index.md), moet u deze vullen met uw gegevens. Als de gegevens zich in een Azure SQL database, een **Azure Search-indexeerfunctie voor Azure SQL Database** (of **Azure SQL-indexeerfunctie** kortweg) het indexing proces, wat betekent minder code dat te schrijven en minder infrastructuur interesseren kunt automatiseren.

In dit artikel bevat informatie over het mechanisme voor het gebruik van [indexeerfuncties](search-indexer-overview.md), maar ook functies die alleen beschikbaar in Azure SQL-databases (bijvoorbeeld geïntegreerd wijzigingen bijhouden) beschreven. 

Naast Azure SQL-databases, biedt Azure Search indexeerfuncties voor [Azure Cosmos DB](search-howto-index-documentdb.md), [Azure Blob storage](search-howto-indexing-azure-blob-storage.md), en [Azure-tabelopslag](search-howto-indexing-azure-tables.md). Vraag ondersteuning voor andere gegevensbronnen, uw feedback geven over de [forum met feedback van Azure Search](https://feedback.azure.com/forums/263029-azure-search/).

## <a name="indexers-and-data-sources"></a>Indexeerfuncties en gegevensbronnen

Een **gegevensbron** geeft aan welke gegevens worden index, referenties voor toegang tot gegevens en beleidsregels die wijzigingen in de gegevens (nieuwe, gewijzigde of verwijderde rijen) efficiënt te identificeren. Het gedefinieerd als een onafhankelijke resource zodat deze kan worden gebruikt door meerdere indexeerfuncties.

Een **indexeerfunctie** is een bron die een enkele gegevensbron is verbonden met een gerichte search-index. Een indexeerfunctie wordt gebruikt in de volgende manieren:

* Uitvoeren van een momentopname van de gegevens naar een index te vullen.
* Bijwerken van een index met wijzigingen in de gegevensbron volgens een schema.
* Voer op aanvraag voor het bijwerken van een index, indien nodig.

Een enkele indexeerfunctie kan alleen gebruiken één tabel of weergave, maar u kunt meerdere indexeerfuncties maken als u wilt meerdere zoekindexen vullen. Zie voor meer informatie over concepten [indexbewerkingen: gangbare werkstroom](https://docs.microsoft.com/rest/api/searchservice/Indexer-operations#typical-workflow).

U kunt instellen en configureren van een Azure SQL-indexeerfunctie met:

* De wizard gegevens importeren in de [Azure-portal](https://portal.azure.com)
* Azure Search [.NET SDK](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.indexer?view=azure-dotnet)
* Azure Search [REST-API](https://docs.microsoft.com/en-us/rest/api/searchservice/indexer-operations)

In dit artikel, kun je de REST-API maken **indexeerfuncties** en **gegevensbronnen**.

## <a name="when-to-use-azure-sql-indexer"></a>Het gebruik van Azure SQL indexeerfunctie
Afhankelijk van verschillende factoren, met betrekking tot uw gegevens, het gebruik van Azure SQL-indexeerfunctie mogelijk of niet. Als uw gegevens aan de volgende vereisten, kunt u Azure SQL-indexeerfunctie.

| Criteria | Details |
|----------|---------|
| Gegevens zijn afkomstig van één tabel of weergave | Als de gegevens is verspreid over meerdere tabellen, kunt u één enkele weergave van de gegevens maken. Echter, als u een weergave gebruikt, u niet mogelijk gebruik van SQL Server geïntegreerd opsporing vernieuwen van een index met incrementele wijzigingen. Zie voor meer informatie [vastleggen gewijzigd en verwijderd rijen](#CaptureChangedRows) hieronder. |
| Gegevenstypen zijn compatibel | De meeste, maar niet alle de SQL-typen worden ondersteund in een Azure Search-index. Zie voor een lijst [toewijzing gegevenstypen](#TypeMapping). |
| Synchronisatie van realtime gegevens is niet vereist | Een indexeerfunctie kan de tabel opnieuw indexeren van maximaal vijf minuten. Als uw gegevens vaak worden gewijzigd en de wijzigingen worden weergegeven in de index binnen enkele seconden of minuten op één moeten, wordt u aangeraden de [REST-API](https://docs.microsoft.com/rest/api/searchservice/AddUpdate-or-Delete-Documents) of [.NET SDK](search-import-data-dotnet.md) voor de bijgewerkte rijen direct push. |
| Incrementele indexeren is mogelijk | Als u een grote gegevensset en plannen voor de indexeerfunctie uitvoeren op een planning hebt, is Azure Search moet kunnen nieuwe, gewijzigde of verwijderde rijen efficiënt te identificeren. Niet-incrementele indexeren is alleen toegestaan als u indexeren op aanvraag (niet op schema), of minder dan 100.000 rijen te indexeren. Zie voor meer informatie [vastleggen gewijzigd en verwijderd rijen](#CaptureChangedRows) hieronder. |

## <a name="create-an-azure-sql-indexer"></a>Maak een Azure SQL-indexeerfunctie

1. De gegevensbron maken:

   ```
    POST https://myservice.search.windows.net/datasources?api-version=2016-09-01
    Content-Type: application/json
    api-key: admin-key

    {
        "name" : "myazuresqldatasource",
        "type" : "azuresql",
        "credentials" : { "connectionString" : "Server=tcp:<your server>.database.windows.net,1433;Database=<your database>;User ID=<your user name>;Password=<your password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30;" },
        "container" : { "name" : "name of the table or view that you want to index" }
    }
   ```

   U krijgt de verbindingsreeks van de [Azure-portal](https://portal.azure.com); gebruik de `ADO.NET connection string` optie.

2. De doel-Azure Search-index maken als u nog niet hebt. U kunt maken met een index met behulp van de [portal](https://portal.azure.com) of de [Index-API maken](https://docs.microsoft.com/rest/api/searchservice/Create-Index). Zorg ervoor dat het schema van de doelindex compatibel met het schema van de brontabel - Zie [toewijzing tussen SQL en Azure search-gegevenstypen](#TypeMapping).

3. De indexeerfunctie maken door een naam geven en verwijst naar de index van het bron- en doel:

    ```
    POST https://myservice.search.windows.net/indexers?api-version=2016-09-01
    Content-Type: application/json
    api-key: admin-key

    {
        "name" : "myindexer",
        "dataSourceName" : "myazuresqldatasource",
        "targetIndexName" : "target index name"
    }
    ```

Een indexeerfunctie gemaakt op deze manier beschikt niet over een planning. Deze wordt automatisch uitgevoerd zodra wanneer deze gemaakt. U kunt deze opnieuw uitvoeren op elke tijd die een **indexeerfunctie uitvoeren** aanvraag:

    POST https://myservice.search.windows.net/indexers/myindexer/run?api-version=2016-09-01
    api-key: admin-key

U kunt verschillende aspecten van de indexeerfunctie gedrag, zoals batchgrootte en het aantal documenten worden overgeslagen voordat de uitvoering van een indexeerfunctie is mislukt. Zie voor meer informatie [indexeerfunctie-API maken](https://docs.microsoft.com/rest/api/searchservice/Create-Indexer).

Mogelijk moet u verbinding maken met uw database Azure-services toestaan. Zie [verbinding te maken van Azure](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure) voor instructies over hoe u dat doet.

Gebruik voor het bewaken van de indexeerfunctie status en uitvoering geschiedenis (aantal items dat is geïndexeerd, fouten, enz.), een **indexeerfunctie status** aanvraag:

    GET https://myservice.search.windows.net/indexers/myindexer/status?api-version=2016-09-01
    api-key: admin-key

Het antwoord moet er ongeveer als volgt uitzien:

    {
        "@odata.context":"https://myservice.search.windows.net/$metadata#Microsoft.Azure.Search.V2015_02_28.IndexerExecutionInfo",
        "status":"running",
        "lastResult": {
            "status":"success",
            "errorMessage":null,
            "startTime":"2015-02-21T00:23:24.957Z",
            "endTime":"2015-02-21T00:36:47.752Z",
            "errors":[],
            "itemsProcessed":1599501,
            "itemsFailed":0,
            "initialTrackingState":null,
            "finalTrackingState":null
        },
        "executionHistory":
        [
            {
                "status":"success",
                "errorMessage":null,
                "startTime":"2015-02-21T00:23:24.957Z",
                "endTime":"2015-02-21T00:36:47.752Z",
                "errors":[],
                "itemsProcessed":1599501,
                "itemsFailed":0,
                "initialTrackingState":null,
                "finalTrackingState":null
            },
            ... earlier history items
        ]
    }

Uitvoergeschiedenis van bevat maximaal 50 van de meest recentelijk uitgevoerde uitvoeringen die in de omgekeerde volgorde worden gesorteerd (zodat de meest recente uitvoering bovenaan in het antwoord).
Meer informatie over het antwoord vindt u in [indexeerfunctie Status ophalen](http://go.microsoft.com/fwlink/p/?LinkId=528198)

## <a name="run-indexers-on-a-schedule"></a>Indexeerfuncties volgens een planning wordt uitgevoerd
U kunt ook de indexeerfunctie periodiek wordt uitgevoerd volgens een schema rangschikken. Voeg hiervoor de **planning** eigenschap tijdens het maken of bijwerken van de indexeerfunctie. Het volgende voorbeeld ziet een PUT-aanvraag voor het bijwerken van de indexeerfunctie:

    PUT https://myservice.search.windows.net/indexers/myindexer?api-version=2016-09-01
    Content-Type: application/json
    api-key: admin-key

    {
        "dataSourceName" : "myazuresqldatasource",
        "targetIndexName" : "target index name",
        "schedule" : { "interval" : "PT10M", "startTime" : "2015-01-01T00:00:00Z" }
    }

De **interval** parameter is vereist. Het interval verwijst naar de tijd tussen het begin van de twee opeenvolgende indexeerfunctie uitvoeringen. De minimaal toegestane interval is 5 minuten. het langste is één dag. Moet zijn geformatteerd als de waarde van een XSD-'dayTimeDuration' (een beperkte subset van een [duur van de ISO 8601](http://www.w3.org/TR/xmlschema11-2/#dayTimeDuration) waarde). Het patroon hiervoor is: `P(nD)(T(nH)(nM))`. Voorbeelden: `PT15M` voor elke 15 minuten `PT2H` voor elke 2 uur.

De optionele **startTime** geeft aan wanneer de geplande uitvoeringen moeten beginnen. Als u dit weglaat, wordt de huidige UTC-tijd gebruikt. Deze tijd kan worden in het verleden – waarin geval de eerste uitvoering is gepland als de indexeerfunctie wordt uitgevoerd continu sinds de starttijd.  

Slechts één van de uitvoering van een indexeerfunctie kunt uitvoeren op een tijdstip. Als een indexeerfunctie wordt uitgevoerd wanneer de uitvoering is gepland, wordt de uitvoering wordt uitgesteld totdat de volgende tijdstip geplande.

Een voorbeeld om dit te concrete laten we eens. Stel dat we de volgende per uur schema dat is geconfigureerd:

    "schedule" : { "interval" : "PT1H", "startTime" : "2015-03-01T00:00:00Z" }

Dit is wat er gebeurt:

1. De eerste uitvoering van de indexeerfunctie wordt gestart op of rond 1 maart 2015 12:00 uur DE UTC.
2. Stel dat deze uitvoering duurt 20 minuten (of elk gewenst moment minder dan 1 uur).
3. De tweede uitvoering wordt gestart op of rond 1 maart 2015 1:00 uur
4. Stel nu dat deze uitvoering meer dan een uur – bijvoorbeeld 70 minuten – neemt zodat ongeveer 2:10 uur is voltooid
5. Het is nu 2:00 uur, tijd voor de derde uitvoering te starten. Echter, omdat de tweede uitvoering van 1 uur nog steeds actief is, het derde uitvoeren wordt overgeslagen. De uitvoering van de derde begint bij 3 uur.

U kunt toevoegen, wijzigen of verwijderen van een planning voor een bestaande indexeerfunctie met behulp van een **PUT indexeerfunctie** aanvraag.

<a name="CaptureChangedRows"></a>

## <a name="capture-new-changed-and-deleted-rows"></a>Nieuwe, gewijzigde en verwijderde rijen vastleggen

Maakt gebruik van Azure Search **incrementele indexeren** om te voorkomen dat opnieuw indexeren de gehele tabel of weergave telkens wanneer een indexeerfunctie wordt uitgevoerd. Azure Search biedt dat twee detectie beleidsregels ter ondersteuning van incrementele indexeren wijzigen. 

### <a name="sql-integrated-change-tracking-policy"></a>In SQL geïntegreerde wijzigingen bijhouden van beleid
Als uw SQL database ondersteunt [bijhouden](https://docs.microsoft.com/sql/relational-databases/track-changes/about-change-tracking-sql-server), wordt u aangeraden **SQL geïntegreerde wijzigen Traceringsbeleid**. Dit is het meest efficiënt beleid. Bovendien kunt u Azure Search, verwijderde rijen identificeren zonder dat u hoeft een expliciete 'soft delete'-kolom toevoegen aan uw tabel.

#### <a name="requirements"></a>Vereisten 

+ Database-versievereisten:
  * SQL Server 2012 SP3 en later, als u SQL Server op Azure Virtual machines.
  * Azure SQL Database V12, als u Azure SQL Database.
+ Alleen tabellen (Er zijn geen weergaven). 
+ In de database [inschakelen voor het bijhouden](https://docs.microsoft.com/sql/relational-databases/track-changes/enable-and-disable-change-tracking-sql-server) voor de tabel. 
+ Er is geen samengestelde primaire sleutel (een primaire sleutel die meer dan één kolom) voor de tabel.  

#### <a name="usage"></a>Gebruik

Gebruik dit beleid, maken of bijwerken van de gegevensbron die u als volgt:

    {
        "name" : "myazuresqldatasource",
        "type" : "azuresql",
        "credentials" : { "connectionString" : "connection string" },
        "container" : { "name" : "table or view name" },
        "dataChangeDetectionPolicy" : {
           "@odata.type" : "#Microsoft.Azure.Search.SqlIntegratedChangeTrackingPolicy"
      }
    }

Wanneer met behulp van SQL geïntegreerd bijhouden van beleid, geef geen een afzonderlijke gegevens detectie verwijderingsbeleid - dit beleid bevat ingebouwde ondersteuning voor het identificeren van verwijderde rijen. Echter voor de verwijderingen gedetecteerde 'automagically' moet moet de documentsleutel in uw search-index hetzelfde zijn als de primaire sleutel in de SQL-tabel. 

<a name="HighWaterMarkPolicy"></a>

### <a name="high-water-mark-change-detection-policy"></a>Beleid voor High-Water Mark wijzigen

Beleid voor deze wijziging is afhankelijk van een 'high-water mark' kolom voor het vastleggen van de versie of de tijd waarop een rij voor het laatst is bijgewerkt. Als u een weergave, moet u een high-water mark-beleid. De kolom high-water mark moet voldoen aan de volgende vereisten.

#### <a name="requirements"></a>Vereisten 

* Alle voegt een waarde opgeven voor de kolom.
* Alle updates aan een item wordt ook de waarde van de kolom wijzigen.
* De waarde van deze kolom wordt verhoogd met elke invoegen of bijwerken.
* Query's met het volgende waar en ORDER BY-componenten efficiënt kunnen worden uitgevoerd:`WHERE [High Water Mark Column] > [Current High Water Mark Value] ORDER BY [High Water Mark Column]`

> [!IMPORTANT] 
> Wij raden met behulp van de [rowversion](https://docs.microsoft.com/sql/t-sql/data-types/rowversion-transact-sql) gegevenstype voor de kolom high-water mark. Als een ander gegevenstype wordt gebruikt, wordt tracering van wijzigingen niet gegarandeerd voor het vastleggen van alle wijzigingen in de aanwezigheid van transacties dat gelijktijdig loopt met een indexeerfunctie-query wordt uitgevoerd. Wanneer u **rowversion** in een configuratie met alleen-lezen-replica's, moet u de indexeerfunctie aanwijst op de primaire replica. Alleen een primaire replica kan worden gebruikt voor scenario's voor het synchroniseren van gegevens.

#### <a name="usage"></a>Gebruik

Voor het gebruik van een high-water mark-beleid maken of bijwerken van de gegevensbron die u als volgt:

    {
        "name" : "myazuresqldatasource",
        "type" : "azuresql",
        "credentials" : { "connectionString" : "connection string" },
        "container" : { "name" : "table or view name" },
        "dataChangeDetectionPolicy" : {
           "@odata.type" : "#Microsoft.Azure.Search.HighWaterMarkChangeDetectionPolicy",
           "highWaterMarkColumnName" : "[a rowversion or last_updated column name]"
      }
    }

> [!WARNING]
> Als de brontabel geen een index voor de kolom high-water mark heeft, kunnen er query's die worden gebruikt door de SQL-indexeerfunctie time-out. In het bijzonder de `ORDER BY [High Water Mark Column]` component vereist een index worden efficiënt wordt uitgevoerd wanneer u deze tabel bevat veel rijen.
>
>

Als u de time-outfouten ondervindt, kunt u de `queryTimeout` indexeerfunctie configuratie-instelling voor de querytime-out ingesteld op een waarde die hoger is dan de time-out van standaard 5 minuten. Bijvoorbeeld, om te stellen de time-out voor 10 minuten, maken of bijwerken van de indexeerfunctie met de volgende configuratie:

    {
      ... other indexer definition properties
     "parameters" : {
            "configuration" : { "queryTimeout" : "00:10:00" } }
    }

U kunt ook uitschakelen de `ORDER BY [High Water Mark Column]` component. Dit wordt echter niet aanbevolen omdat als de uitvoering van de indexeerfunctie wordt onderbroken door een fout, wordt de indexeerfunctie is opnieuw verwerkt alle rijen als deze later - wordt uitgevoerd zelfs als de indexeerfunctie bijna alle rijen al verwerkt door de tijd dat deze is onderbroken. Uitschakelen van de `ORDER BY` component, gebruik de `disableOrderByHighWaterMarkColumn` instellen in de definitie van de indexeerfunctie:  

    {
     ... other indexer definition properties
     "parameters" : {
            "configuration" : { "disableOrderByHighWaterMarkColumn" : true } }
    }

### <a name="soft-delete-column-deletion-detection-policy"></a>Beleid voor voorlopig verwijderen kolom verwijdering detectie
Wanneer rijen zijn verwijderd uit de brontabel, wilt u waarschijnlijk de rijen verwijderen uit de search-index. Als u beleid voor de SQL geïntegreerd bijhouden, is dit afgehandeld voor u. Echter, de high-water mark bijhouden beleid niet helpt u met verwijderde rijen. Hoe pakt u dit aan?

Als de rijen worden fysiek verwijderd uit de tabel, er Azure Search geen manier voor het afleiden van de aanwezigheid van records die niet meer bestaat.  U kunt echter de techniek 'soft-delete' logisch om rijen te verwijderen zonder dat ze worden verwijderd uit de tabel. Een kolom toevoegen aan de rijen van de tabel of weergave en markeren als verwijderd met behulp van die kolom.

Wanneer u de soft-delete-methode gebruikt, kunt u het beleid voor voorlopige verwijdering als volgt bij het maken of bijwerken van de gegevensbron kunt opgeven:

    {
        …,
        "dataDeletionDetectionPolicy" : {
           "@odata.type" : "#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy",
           "softDeleteColumnName" : "[a column name]",
           "softDeleteMarkerValue" : "[the value that indicates that a row is deleted]"
        }
    }

De **softDeleteMarkerValue** moet een tekenreeks-de tekenreeksweergave van uw werkelijke waarde gebruiken. Bijvoorbeeld, als u een kolom met gehele getallen waar verwijderde rijen zijn gemarkeerd met de waarde 1 hebt, gebruik `"1"`. Als u hebt een bits-kolom waar verwijderde rijen zijn gemarkeerd met de waarde voor Boole-waarde ' True ', gebruikt u `"True"`.

<a name="TypeMapping"></a>

## <a name="mapping-between-sql-and-azure-search-data-types"></a>Toewijzing tussen SQL en Azure Search-gegevenstypen
| SQL-gegevenstype | Toegestaan doelindex veldtypen | Opmerkingen |
| --- | --- | --- |
| bits |Edm.Boolean, Edm.String | |
| int en smallint, tinyint |Edm.Int32, Edm.Int64, Edm.String | |
| bigint |Edm.Int64, Edm.String | |
| reële float |Edm.Double, Edm.String | |
| smallmoney, geld decimale numerieke |Edm.String |Azure Search biedt geen ondersteuning voor het decimale typen converteren naar Edm.Double omdat dit precisie verliezen |
| CHAR, nchar, varchar, nvarchar |Edm.String<br/>Collection(EDM.String) |Een SQL-tekenreeks kan worden gebruikt voor het vullen van een veld verzameling (EDM.String) als de tekenreeks een JSON-matrix van tekenreeksen vertegenwoordigt:`["red", "white", "blue"]` |
| smalldatetime, datetime, datetime2, date, datetimeoffset |Edm.DateTimeOffset, Edm.String | |
| uniqueidentifer |Edm.String | |
| Geografie |Edm.GeographyPoint |Alleen Geografie exemplaren van het type punt met SRID 4326 (dit is de standaardinstelling) worden ondersteund |
| ROWVERSION |N.v.t. |Kolommen van de rij-versie kunnen niet worden opgeslagen in de search-index, maar ze kunnen worden gebruikt voor het bijhouden |
| tijd, timespan, binary, varbinary, image, xml, geometry, CLR-typen |N.v.t. |Niet ondersteund |

## <a name="configuration-settings"></a>Configuratie-instellingen
SQL-indexeerfunctie beschrijft de verschillende configuratie-instellingen:

| Instelling | Gegevenstype | Doel | Standaardwaarde |
| --- | --- | --- | --- |
| queryTimeout |Tekenreeks |Hiermee stelt u de time-out voor de uitvoering van de SQL-query |5 minuten (' 00: 05:00 ") |
| disableOrderByHighWaterMarkColumn |BOOL |Zorgt ervoor dat de SQL-query die wordt gebruikt door het beleid high-water mark weglaten de component ORDER BY. Zie [High-Water Mark beleid](#HighWaterMarkPolicy) |ONWAAR |

Deze instellingen worden gebruikt de `parameters.configuration` -object in de definitie van de indexeerfunctie. Bijvoorbeeld, als u wilt de querytime-out ingesteld op 10 minuten, maken of bijwerken van de indexeerfunctie met de volgende configuratie:

    {
      ... other indexer definition properties
     "parameters" : {
            "configuration" : { "queryTimeout" : "00:10:00" } }
    }

## <a name="faq"></a>Veelgestelde vragen

**V: kan ik Azure SQL-indexeerfunctie gebruiken met SQL-databases die zijn uitgevoerd op IaaS virtuele machines in Azure**

Ja. U moet echter toestaan uw search-service verbinding maken met uw database. Zie voor meer informatie [configureren van een verbinding van een Azure Search-indexeerfunctie naar SQL Server op een Azure VM](search-howto-connecting-azure-sql-iaas-to-azure-search-using-indexers.md).

**V: kan ik Azure SQL-indexeerfunctie met SQL-databases met lokale gebruiken?**

Niet rechtstreeks. Er wordt niet aanbevolen noch een rechtstreekse verbinding ondersteunen zoals doen dus moet u uw databases om internetverkeer te openen. Klanten zijn met dit scenario met brug technologieën zoals Azure Data Factory geslaagd. Zie voor meer informatie [Push-gegevens naar een Azure Search-index met behulp van Azure Data Factory](https://docs.microsoft.com/azure/data-factory/data-factory-azure-search-connector).

**V: kan ik Azure SQL-indexeerfunctie gebruiken dan SQL Server op Azure wordt uitgevoerd in een IaaS-databases**

Nee. Dit scenario wordt ondersteund niet omdat de indexeerfunctie met alle databases dan SQL Server is niet getest.  

**V: kan ik meerdere indexeerfuncties uitgevoerd volgens een planning maken?**

Ja. Echter kan slechts één indexeerfunctie worden uitgevoerd op één knooppunt tegelijk. Als u meerdere indexeerfuncties gelijktijdig uitgevoerd moet, kunt u uw zoekservice aan meer dan één zoekeenheid schaalt.

**V: bevat een indexeerfunctie invloed zijn op de workload van mijn query's uitgevoerd?**

Ja. Indexeerfunctie wordt uitgevoerd op een van de knooppunten in uw zoekservice en resources voor dat knooppunt worden gedeeld tussen indexeren ten behoeve van de queryverkeer en andere API-aanvragen. Als u werkbelastingen met een intensieve indexeren en de query uitvoert en een hoge frequentie van 503-fouten of toenemende reactietijden optreden, kunt u overwegen [uw search-service schaalt](search-capacity-planning.md).

**V: kan ik gebruik van een secundaire replica in een [failovercluster](https://docs.microsoft.com/azure/sql-database/sql-database-geo-replication-overview) als een gegevensbron?**

Dat hangt ervan af. U kunt een secundaire replica gebruiken voor volledige indexering van een tabel of weergave. 

Voor incrementele indexeren Azure Search ondersteunt twee beleidsregels wijzigen: SQL geïntegreerd bijgehouden en High-Water Mark wijzigen.

Op alleen-lezen-replica's ondersteunt SQL database geen geïntegreerde wijzigingen bijhouden. Daarom moet u een High-Water Mark beleid gebruiken. 

We bevelen aan standaard is het gebruik van het gegevenstype rowversion voor de kolom high-water mark. Echter, is met behulp van rowversion afhankelijk van de SQL-Database `MIN_ACTIVE_ROWVERSION` functie, wat niet wordt ondersteund op alleen-lezen-replica's. Daarom moet u de indexeerfunctie verwijzen naar een primaire replica als u van rowversion gebruikmaakt.

Als u probeert te gebruiken rowversion op een alleen-lezen-replica, ziet u de volgende fout: 

    "Using a rowversion column for change tracking is not supported on secondary (read-only) availability replicas. Please update the datasource and specify a connection to the primary availability replica.Current database 'Updateability' property is 'READ_ONLY'".

**V: kan ik een alternatieve, niet-tijdstempelkolom gebruiken voor het bijhouden van een high-water mark?**

Het wordt niet aangeraden. Alleen **rowversion** voor betrouwbare gegevenssynchronisatie kunt. Echter, afhankelijk van uw toepassingslogica mogelijk veilige als:

+ U kunt ervoor zorgen dat wanneer de indexeerfunctie wordt uitgevoerd, er geen openstaande transacties in de tabel die wordt geïndexeerd (bijvoorbeeld alle tabel updates geval een batch op een planning en de Azure Search-indexeerfunctie planning is ingesteld zijn om te voorkomen dat overlapt met de tabel update plannen).  

+ U doen een volledige opnieuw indexeren om op te halen rijen gemiste regelmatig. 
