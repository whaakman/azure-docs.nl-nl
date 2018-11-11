---
title: Azure SQL Database verbinding te maken met Azure Search met de indexeerfuncties | Microsoft Docs
description: Informatie over het ophalen van gegevens uit Azure SQL Database naar een Azure Search-index met de indexeerfuncties.
ms.date: 10/17/2018
author: mgottein
manager: cgronlun
ms.author: magottei
services: search
ms.service: search
ms.devlang: rest-api
ms.topic: conceptual
ms.openlocfilehash: ba2ce12fcfad14b0910144b1a95efd44be54811f
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51245644"
---
# <a name="connecting-azure-sql-database-to-azure-search-using-indexers"></a>Azure SQL Database verbinding te maken met Azure Search met behulp van indexeerfuncties

Voordat u kunt een query een [Azure Search-index](search-what-is-an-index.md), moet u deze vullen met uw gegevens. Als de gegevens zich in een Azure SQL-database bevinden, een **Azure Search-indexeerfunctie voor Azure SQL Database** (of **indexeerfunctie voor Azure SQL** kortweg) het indexeringsproces worden geautomatiseerd, wat betekent dat minder code te schrijven en minder kunt automatiseren infrastructuur over.

In dit artikel bevat informatie over het mechanisme voor het gebruik van [indexeerfuncties](search-indexer-overview.md), maar ook functies die alleen beschikbaar in Azure SQL-databases (bijvoorbeeld geïntegreerd wijzigingen bijhouden) beschreven. 

Naast Azure SQL-databases, biedt Azure Search indexeerfuncties voor [Azure Cosmos DB](search-howto-index-cosmosdb.md), [Azure Blob-opslag](search-howto-indexing-azure-blob-storage.md), en [Azure-tabelopslag](search-howto-indexing-azure-tables.md). Ondersteuning aanvragen voor andere gegevensbronnen, uw feedback geven over de [forum met feedback van Azure Search](https://feedback.azure.com/forums/263029-azure-search/).

## <a name="indexers-and-data-sources"></a>Indexeerfuncties en gegevensbronnen

Een **gegevensbron** Hiermee geeft u de gegevens die u wilt de index, referenties voor toegang tot gegevens en beleidsregels die efficiënt om wijzigingen in de gegevens (nieuwe, gewijzigde of verwijderde rijen). Het gedefinieerd als een onafhankelijke resource zodat deze kan worden gebruikt door meerdere indexeerfuncties.

Een **indexeerfunctie** is een resource die een enkele gegevensbron met een gerichte search-index verbindt. Een indexeerfunctie wordt gebruikt in de volgende manieren:

* Uitvoeren van een momentopname van de gegevens naar een index te vullen.
* Bijwerken van een index met wijzigingen in de gegevensbron volgens een schema.
* On-demand om het bijwerken van een index, indien nodig worden uitgevoerd.

Een indexeerfunctie één kan alleen worden gebruikt een tabel of weergave, maar u kunt meerdere indexeerfuncties maken als u wilt meerdere search-index te vullen. Zie voor meer informatie over concepten [bewerkingen voor indexeerfunctie: standaardwerkstroom](https://docs.microsoft.com/rest/api/searchservice/Indexer-operations#typical-workflow).

U kunt instellen en configureren van een Azure SQL-indexer gebruiken:

* De wizard gegevens importeren in de [Azure-portal](https://portal.azure.com)
* Azure Search [.NET SDK](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.indexer?view=azure-dotnet)
* Azure Search [REST-API](https://docs.microsoft.com/rest/api/searchservice/indexer-operations)

In dit artikel gebruiken we de REST-API maken **indexeerfuncties** en **gegevensbronnen**.

## <a name="when-to-use-azure-sql-indexer"></a>Wanneer u Azure SQL-Indexer
Afhankelijk van diverse factoren met betrekking tot uw gegevens, het gebruik van Azure SQL-indexer kan of mogelijk niet altijd geschikt. Als uw gegevens de volgende vereisten voldoet past, kunt u de indexeerfunctie voor Azure SQL kunt gebruiken.

| Criteria | Details |
|----------|---------|
| Data is afkomstig uit één tabel of weergave | Als de gegevens is verspreid over meerdere tabellen, kunt u een weergave van de gegevens maken. Echter, als u een weergave gebruikt, kunt u zich niet SQL Server geïntegreerd wijziging detection gebruikt om te vernieuwen van een index met incrementele wijzigingen. Zie voor meer informatie, [vastleggen gewijzigd en de rijen verwijderd](#CaptureChangedRows) hieronder. |
| Gegevenstypen zijn compatibel | De meeste, maar niet alle de SQL-typen worden ondersteund in een Azure Search-index. Zie voor een lijst [toewijzing gegevenstypen](#TypeMapping). |
| Realtime gegevenssynchronisatie is niet vereist | Een indexeerfunctie kan uw tabel indexeren maximaal vijf minuten. Als uw gegevens regelmatig worden gewijzigd en de wijzigingen worden weergegeven in de index binnen enkele seconden of enkele minuten moeten, wordt u aangeraden de [REST-API](https://docs.microsoft.com/rest/api/searchservice/AddUpdate-or-Delete-Documents) of [.NET SDK](search-import-data-dotnet.md) bijgewerkte rijen rechtstreeks te pushen. |
| Incrementele indexeren is mogelijk | Als u een grote gegevensset en een plan voor de indexeerfunctie uitvoeren volgens een planning hebt, is Azure Search moet kunnen nieuwe, gewijzigde of verwijderde rijen efficiënt te identificeren. Indexeren van niet-incrementele is alleen toegestaan als u bent op aanvraag (niet op schema) te indexeren of minder dan 100.000 rijen te indexeren. Zie voor meer informatie, [vastleggen gewijzigd en de rijen verwijderd](#CaptureChangedRows) hieronder. |

> [!NOTE] 
> Azure Search biedt ondersteuning voor SQL Server-verificatie. Als u ondersteuning voor Azure Active Directory-wachtwoordverificatie nodig hebt, kunt stemmen voor deze [UserVoice suggestie](https://feedback.azure.com/forums/263029-azure-search/suggestions/33595465-support-azure-active-directory-password-authentica).

## <a name="create-an-azure-sql-indexer"></a>Een Azure SQL-indexeerfunctie maken

1. De gegevensbron maken:

   ```
    POST https://myservice.search.windows.net/datasources?api-version=2017-11-11
    Content-Type: application/json
    api-key: admin-key

    {
        "name" : "myazuresqldatasource",
        "type" : "azuresql",
        "credentials" : { "connectionString" : "Server=tcp:<your server>.database.windows.net,1433;Database=<your database>;User ID=<your user name>;Password=<your password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30;" },
        "container" : { "name" : "name of the table or view that you want to index" }
    }
   ```

   Krijgt u de verbindingsreeks uit de [Azure-portal](https://portal.azure.com); gebruik de `ADO.NET connection string` optie.

2. Maak de doel-Azure Search-index als u er nog geen hebt. U kunt maken met een index met behulp van de [portal](https://portal.azure.com) of de [API ' Create Index '](https://docs.microsoft.com/rest/api/searchservice/Create-Index). Zorg ervoor dat het schema van de doelindex compatibel met het schema van de brontabel is-Zie [toewijzing tussen SQL en Azure search gegevenstypen](#TypeMapping).

3. De indexeerfunctie maken door een naam geven en verwijzen naar de bron en doel-index van gegevens:

    ```
    POST https://myservice.search.windows.net/indexers?api-version=2017-11-11
    Content-Type: application/json
    api-key: admin-key

    {
        "name" : "myindexer",
        "dataSourceName" : "myazuresqldatasource",
        "targetIndexName" : "target index name"
    }
    ```

Een indexeerfunctie gemaakt op deze manier beschikt niet over een schema. Deze wordt automatisch uitgevoerd zodra wanneer deze gemaakt. U kunt het opnieuw uitvoeren op elk gewenst moment een **indexeerfunctie uitvoeren** aanvraag:

    POST https://myservice.search.windows.net/indexers/myindexer/run?api-version=2017-11-11
    api-key: admin-key

U kunt verschillende aspecten van indexeerfunctie gedrag, zoals batchgrootte en het aantal documenten worden overgeslagen voordat de uitvoering van een indexeerfunctie is mislukt. Zie voor meer informatie, [Indexer-API maken](https://docs.microsoft.com/rest/api/searchservice/Create-Indexer).

Mogelijk moet u verbinding maakt met uw database Azure-services toestaan. Zie [verbinding te maken van Azure](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure) voor instructies over hoe u dat doet.

Voor het controleren van de indexeerfunctie status en de uitvoering geschiedenis (aantal items dat is geïndexeerd, fouten, enzovoort) gebruikt een **indexeerfunctie status** aanvraag:

    GET https://myservice.search.windows.net/indexers/myindexer/status?api-version=2017-11-11
    api-key: admin-key

Het antwoord moet er ongeveer als volgt uitzien:

    {
        "\@odata.context":"https://myservice.search.windows.net/$metadata#Microsoft.Azure.Search.V2015_02_28.IndexerExecutionInfo",
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

Uitvoeringsgeschiedenis bevat maximaal 50 van de meest recentelijk uitgevoerde uitvoeringen, die in de omgekeerde volgorde worden gesorteerd (zodat de meest recente uitvoering in het antwoord eerste komt).
Meer informatie over het antwoord kunt vinden [indexeerfunctie Status ophalen](https://go.microsoft.com/fwlink/p/?LinkId=528198)

## <a name="run-indexers-on-a-schedule"></a>Indexeerfuncties volgens een planning wordt uitgevoerd
Ook kunt u instellen dat de indexeerfunctie periodiek wordt uitgevoerd volgens een schema. Om dit te doen, voeg de **planning** eigenschap bij het maken of bijwerken van de indexeerfunctie. In het volgende voorbeeld toont een PUT-aanvraag voor het bijwerken van de indexeerfunctie:

    PUT https://myservice.search.windows.net/indexers/myindexer?api-version=2017-11-11
    Content-Type: application/json
    api-key: admin-key

    {
        "dataSourceName" : "myazuresqldatasource",
        "targetIndexName" : "target index name",
        "schedule" : { "interval" : "PT10M", "startTime" : "2015-01-01T00:00:00Z" }
    }

De **interval** parameter is vereist. Het interval verwijst naar de tijd tussen het begin van de twee opeenvolgende indexeerfunctie uitvoeringen. De minimaal toegestane interval is 5 minuten. het langste is één dag. Deze moet zijn opgemaakt als een "dayTimeDuration" XSD-waarde (een beperkte subset van een [duur van de ISO 8601](http://www.w3.org/TR/xmlschema11-2/#dayTimeDuration) waarde). Het patroon hiervoor is: `P(nD)(T(nH)(nM))`. Voorbeelden: `PT15M` voor elke 15 minuten, `PT2H` voor elke 2 uur.

De optionele **startTime** geeft aan wanneer de geplande uitvoeringen moeten beginnen. Als deze wordt weggelaten, wordt de huidige UTC-tijd gebruikt. Deze tijd kan worden in het verleden – waarin geval de eerste uitvoering is gepland als de indexeerfunctie continu sinds de startTime actief is geweest.  

Slechts één uitvoering van een indexeerfunctie kunt uitvoeren op een tijdstip. Als een indexeerfunctie wordt uitgevoerd wanneer de uitvoering ervan is gepland, wordt de uitvoering is uitgesteld totdat de volgende tijdstip geplande.

Laten we eens een voorbeeld waarmee dit meer concrete. Stel dat we de volgende planning per uur geconfigureerd:

    "schedule" : { "interval" : "PT1H", "startTime" : "2015-03-01T00:00:00Z" }

Dit is wat er gebeurt:

1. De eerste uitvoering van de indexeerfunctie wordt gestart op of omstreeks vanaf 1 maart 2015 12:00 uur UTC.
2. Wordt ervan uitgegaan dat deze uitvoering duurt 20 minuten (of elk gewenst moment minder dan 1 uur).
3. De tweede uitvoering gestart op of omstreeks vanaf 1 maart 2015 1:00 uur
4. Stel nu dat deze tot uitvoering van meer dan een uur – bijvoorbeeld 70 minuten – duurt zodat deze ongeveer 2:10 uur is voltooid
5. Het is nu 2:00 uur, tijd voor het derde uitvoeren om te starten. Echter, omdat de tweede uitvoering van 1 uur nog steeds uitgevoerd, is de derde uitvoering is overgeslagen. De derde uitvoering begint bij 3 uur.

U kunt toevoegen, wijzigen of verwijderen van een schema voor een bestaande indexeerfunctie met behulp van een **PUT indexeerfunctie** aanvraag.

<a name="CaptureChangedRows"></a>

## <a name="capture-new-changed-and-deleted-rows"></a>Het vastleggen van nieuwe, gewijzigd en rijen verwijderd

Maakt gebruik van Azure Search **incrementele indexeren** om te voorkomen dat indexeren van de hele tabel of weergave telkens wanneer een indexeerfunctie wordt uitgevoerd. Azure Search biedt dat twee beleidsregels ter ondersteuning van incrementele indexeren wijzigen. 

### <a name="sql-integrated-change-tracking-policy"></a>In SQL geïntegreerde wijzigingen bijhouden van beleid
Als uw SQL-database ondersteunt [wijzigingen bijhouden](https://docs.microsoft.com/sql/relational-databases/track-changes/about-change-tracking-sql-server), wordt u aangeraden **SQL geïntegreerde wijzigen Traceringsbeleid**. Dit is de meest efficiënte beleid. Daarnaast kan Azure Search om te identificeren van verwijderde rijen zonder dat u hoeft een expliciete "voorlopig verwijderen' kolom toevoegen aan uw tabel.

#### <a name="requirements"></a>Vereisten 

+ Vereisten voor de database-versie:
  * SQL Server 2012 SP3 en later, als u van SQL Server op Azure Virtual machines gebruikmaakt.
  * Azure SQL Database V12, als u Azure SQL Database.
+ Alleen tabellen (Er zijn geen weergaven). 
+ Op de database, [bijhouden van wijzigingen inschakelen](https://docs.microsoft.com/sql/relational-databases/track-changes/enable-and-disable-change-tracking-sql-server) voor de tabel. 
+ Er is geen samengestelde primaire sleutel (een primaire sleutel die meer dan één kolom) op de tabel.  

#### <a name="usage"></a>Gebruik

Voor het gebruik van dit beleid niet maken of bijwerken van uw gegevensbron als volgt:

    {
        "name" : "myazuresqldatasource",
        "type" : "azuresql",
        "credentials" : { "connectionString" : "connection string" },
        "container" : { "name" : "table or view name" },
        "dataChangeDetectionPolicy" : {
           "@odata.type" : "#Microsoft.Azure.Search.SqlIntegratedChangeTrackingPolicy"
      }
    }

Wanneer SQL geïntegreerde wijzigingen bijhouden-beleid gebruiken een afzonderlijke detectie verwijderingsbeleid niet opgeeft: dit beleid heeft ingebouwde ondersteuning voor het identificeren van de rijen verwijderd. Voor het verwijderen moet gedetecteerde 'automagically', moet de documentsleutel in uw search-index wel gelijk zijn aan de primaire sleutel in de SQL-tabel. 

<a name="HighWaterMarkPolicy"></a>

### <a name="high-water-mark-change-detection-policy"></a>Beleid voor detectie van high-Water Mark wijzigen

Dit detectiebeleid voor wijzigingen, is afhankelijk van een 'hoge water mark' kolom vastleggen van de versie of wanneer een rij voor het laatst is bijgewerkt. Als u een weergave, moet u een beleid voor high-water mark gebruiken. De kolom watermerk hoge moet voldoen aan de volgende vereisten.

#### <a name="requirements"></a>Vereisten 

* Alle voegt opgeven een waarde voor de kolom.
* Alle updates aan een item wordt ook de waarde van de kolom wijzigen.
* De waarde van deze kolom verhoogd met elke invoegen of bijwerken.
* Query's met het volgende waar en ORDER BY-componenten efficiënt kunnen worden uitgevoerd: `WHERE [High Water Mark Column] > [Current High Water Mark Value] ORDER BY [High Water Mark Column]`

> [!IMPORTANT] 
> Wordt aangeraden met behulp van de [rowversion](https://docs.microsoft.com/sql/t-sql/data-types/rowversion-transact-sql) gegevenstype voor de kolom watermerk hoog. Als een ander gegevenstype wordt gebruikt, is niet bijhouden om vast te leggen van alle wijzigingen met transacties die worden uitgevoerd als een query indexeerfunctie gegarandeerd. Bij het gebruik van **rowversion** in een configuratie met alleen-lezen replica's, moet u de indexeerfunctie verwijzen naar de primaire replica. Alleen een primaire replica kan worden gebruikt voor scenario's voor het synchroniseren van gegevens.

#### <a name="usage"></a>Gebruik

Voor het gebruik van een high-water mark-beleid maken of bijwerken van uw gegevensbron als volgt:

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
> Als de brontabel heeft geen een index voor de kolom watermerk hoge, kunnen query's die worden gebruikt door de SQL-indexeerfunctie time-out. In het bijzonder de `ORDER BY [High Water Mark Column]` component moet een index voor het efficiënt uitvoeren wanneer de tabel veel rijen bevat.
>
>

Als er time-outfouten optreden, kunt u de `queryTimeout` indexeerfunctie configuratie-instelling in te stellen time-out van de query op een waarde die hoger is dan de time-out voor de standaard 5 minuten. Bijvoorbeeld, om te stellen de time-out voor 10 minuten, maken of bijwerken van de indexeerfunctie met de volgende configuratie:

    {
      ... other indexer definition properties
     "parameters" : {
            "configuration" : { "queryTimeout" : "00:10:00" } }
    }

U kunt ook uitschakelen de `ORDER BY [High Water Mark Column]` component. Dit wordt echter niet aanbevolen omdat als de uitvoering van de indexeerfunctie wordt onderbroken door een fout, wordt de indexeerfunctie is voor het opnieuw verwerken van alle rijen als deze hoger - wordt uitgevoerd, zelfs als de indexeerfunctie al bijna alle rijen verwerkt op het moment dat deze is onderbroken. Om uit te schakelen de `ORDER BY` component, gebruik de `disableOrderByHighWaterMarkColumn` instellen in de definitie van de indexeerfunctie:  

    {
     ... other indexer definition properties
     "parameters" : {
            "configuration" : { "disableOrderByHighWaterMarkColumn" : true } }
    }

### <a name="soft-delete-column-deletion-detection-policy"></a>Beleid voor voorlopig verwijderen kolom verwijderen detectie
Wanneer rijen zijn verwijderd uit de brontabel, wilt u waarschijnlijk de rijen verwijderen uit de search-index. Als u de geïntegreerde SQL-beleid voor bijhouden, wordt dit afgehandeld voor u. Echter, het beleid voor bijhouden van high-water mark niet helpt u met de verwijderde rijen. Hoe pakt u dit aan?

Als de rijen worden fysiek verwijderd uit de tabel, heeft Azure Search afleiden van de aanwezigheid van de records die niet meer aanwezig zijn op geen enkele manier.  U kunt echter de techniek 'voorlopig verwijderen' logisch om rijen te verwijderen zonder te verwijderen uit de tabel. Een kolom toevoegen aan de rijen van de tabel of weergave en markeren als verwijderd met behulp van die kolom.

Wanneer u de functie voor voorlopig verwijderen techniek, kunt u het beleid voor voorlopige verwijdering als volgt bij het maken of bijwerken van de gegevensbron kunt opgeven:

    {
        …,
        "dataDeletionDetectionPolicy" : {
           "@odata.type" : "#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy",
           "softDeleteColumnName" : "[a column name]",
           "softDeleteMarkerValue" : "[the value that indicates that a row is deleted]"
        }
    }

De **softDeleteMarkerValue** moet een tekenreeks is: Gebruik de tekenreeksweergave van uw werkelijke waarde. Bijvoorbeeld, hebt u een kolom met gehele getallen waar verwijderde rijen zijn gemarkeerd met de waarde 1, gebruik `"1"`. Als u een bits-kolom waar verwijderde rijen zijn gemarkeerd met de Booleaanse waarde true hebt, gebruikt u de letterlijke tekenreeks `True` of `true`, de aanvraag is niet van belang.

<a name="TypeMapping"></a>

## <a name="mapping-between-sql-and-azure-search-data-types"></a>Toewijzing tussen gegevenstypen SQL en Azure Search
| SQL-gegevenstype | Doelindex veldtypen toegestaan | Opmerkingen |
| --- | --- | --- |
| bits |Edm.Boolean, Edm.String | |
| int en smallint, tinyint |Edm.Int32, Edm.Int64, Edm.String | |
| bigint |Edm.Int64, Edm.String | |
| echte, drijvende komma |Edm.Double, Edm.String | |
| smallmoney, geld decimaal getal |Edm.String |Azure Search biedt geen ondersteuning voor het decimale type converteren naar Edm.Double omdat dit precisie kwijtraken |
| CHAR, nchar, varchar, nvarchar |Edm.String<br/>Collection(EDM.String) |Een SQL-tekenreeks kan worden gebruikt voor het vullen van een veld Collection(Edm.String) als de tekenreeks een JSON-matrix van tekenreeksen vertegenwoordigt: `["red", "white", "blue"]` |
| smalldatetime, datum/tijd, datetime2, date, datetimeoffset |Edm.DateTimeOffset, Edm.String | |
| uniqueidentifer |Edm.String | |
| Geografie |Edm.GeographyPoint |Alleen Geografie exemplaren van het type punt met SRID 4326 (dit is de standaardinstelling) worden ondersteund |
| ROWVERSION |N/A |Kolommen van de rij-versie kunnen niet worden opgeslagen in de search-index, maar ze kunnen worden gebruikt voor het bijhouden |
| tijd, timespan, binary, varbinary, image, xml, geometrie, CLR-typen |N/A |Niet ondersteund |

## <a name="configuration-settings"></a>Configuratie-instellingen
Indexeerfunctie voor SQL wordt aangegeven dat verschillende configuratie-instellingen:

| Instelling | Gegevenstype | Doel | Standaardwaarde |
| --- | --- | --- | --- |
| queryTimeout |tekenreeks |Hiermee stelt u de time-out voor de uitvoering van de SQL-query |5 minuten ("00: 05:00") |
| disableOrderByHighWaterMarkColumn |BOOL |Zorgt ervoor dat de SQL-query die wordt gebruikt door het beleid voor high-water mark weglaten de component ORDER BY. Zie [High-Water Mark beleid](#HighWaterMarkPolicy) |false |

Deze instellingen worden gebruikt de `parameters.configuration` object in de definitie van de indexeerfunctie. Bijvoorbeeld, als u wilt de querytime-out ingesteld op 10 minuten, maken of bijwerken van de indexeerfunctie met de volgende configuratie:

    {
      ... other indexer definition properties
     "parameters" : {
            "configuration" : { "queryTimeout" : "00:10:00" } }
    }

## <a name="faq"></a>Veelgestelde vragen

**V: kan ik Azure SQL-indexer gebruiken met SQL-databases die worden uitgevoerd op virtuele IaaS-machines in Azure?**

Ja. U moet echter om toe te staan uw search-service verbinding maakt met uw database. Zie voor meer informatie, [configureren van een verbinding tussen een Azure Search-indexeerfunctie en SQL Server op een Azure VM](search-howto-connecting-azure-sql-iaas-to-azure-search-using-indexers.md).

**V: kan ik Azure SQL-indexer gebruiken met SQL-databases die on-premises uitgevoerd?**

Niet rechtstreeks. We raden aan of niet een rechtstreekse verbinding ondersteuning als dit dus moet u uw databases om internetverkeer te openen. Klanten zijn met dit scenario met bridge-technologieën, zoals Azure Data Factory geslaagd. Zie voor meer informatie, [gegevens pushen naar een Azure Search-index met behulp van Azure Data Factory](https://docs.microsoft.com/azure/data-factory/data-factory-azure-search-connector).

**V: kan ik indexeerfunctie voor Azure SQL-databases dan SQL Server uitvoeren in IaaS op Azure gebruiken?**

Nee. Dit scenario wordt ondersteund niet omdat de indexeerfunctie met databases dan SQL Server niet getest.  

**V: kan ik meerdere indexeerfuncties uitgevoerd volgens een planning maken?**

Ja. Echter kan alleen een indexeerfunctie worden uitgevoerd op één knooppunt tegelijk. Als u meerdere indexeerfuncties gelijktijdig worden uitgevoerd, kunt u omhoog schalen van uw zoekservice op meer dan één zoekeenheid.

**V: bevat een indexeerfunctie hebben invloed op mijn werkbelasting van query's uitgevoerd?**

Ja. Indexeerfunctie wordt uitgevoerd op een van de knooppunten in uw zoekservice en de resources van het knooppunt worden gedeeld tussen indexeren ten behoeve van query's en andere API-aanvragen. Als u intensieve werkbelastingen voor indexeren en query uitvoeren en een groot aantal 503-fouten of toenemende reactietijden optreden, kunt u overwegen [omhoog schalen van uw zoekservice](search-capacity-planning.md).

**V: kan ik gebruik van een secundaire replica in een [failovercluster](https://docs.microsoft.com/azure/sql-database/sql-database-geo-replication-overview) als een gegevensbron?**

Dat hangt ervan af. Voor volledige indexering van een tabel of weergave, kunt u een secundaire replica. 

Voor incrementele indexeren, Azure Search ondersteunt twee beleidsregels wijzigen: geïntegreerde SQL wijzigen bij te houden en een hoge watermerk.

Op alleen-lezen replica's ondersteunt SQL database geen geïntegreerde wijzigingen bijhouden. Daarom moet u het beleid voor High-Water Mark gebruiken. 

De standaard aanbeveling is het gebruik van het gegevenstype rowversion voor de kolom watermerk hoog. Echter met behulp van rowversion is afhankelijk van SQL-Database `MIN_ACTIVE_ROWVERSION` functie, wat niet wordt ondersteund op alleen-lezen replica's. Daarom moet u de indexeerfunctie verwijzen naar een primaire replica als u rowversion.

Als u probeert te rowversion gebruiken op een alleen-lezen replica, ziet u de volgende fout: 

    "Using a rowversion column for change tracking is not supported on secondary (read-only) availability replicas. Please update the datasource and specify a connection to the primary availability replica.Current database 'Updateability' property is 'READ_ONLY'".

**V: kan ik een alternatieve, niet-tijdstempelkolom gebruiken voor het bijhouden van high-water mark?**

Het wordt niet aanbevolen. Alleen **rowversion** kunt voor het synchroniseren van betrouwbare gegevens. Echter, afhankelijk van uw toepassingslogica kan het zijn veilig als:

+ U kunt ervoor zorgen dat wanneer de indexeerfunctie wordt uitgevoerd, er zijn geen openstaande transacties in de tabel die wordt geïndexeerd (bijvoorbeeld alle updates van de tabel zich voordoen als een batch volgens een schema en de planning van de indexeerfunctie Azure Search is ingesteld om te voorkomen dat overlapt met de tabelupdate Schedule).  

+ U kunt een volledige reindex om op te halen gemiste rijen periodiek doen. 
