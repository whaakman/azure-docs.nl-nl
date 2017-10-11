---
title: 'Indexbewerkingen (Azure Search Service REST-API: 2015-02-28-Preview) | Microsoft Docs'
description: 'Indexbewerkingen (Azure Search Service REST-API: 2015-02-28-Preview)'
services: search
documentationcenter: 
author: chaosrealm
manager: pablocas
editor: 
ms.assetid: 42b5f85c-8304-4bc7-8e1e-a9c76b8ca25a
ms.service: search
ms.devlang: rest-api
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 05/01/2017
ms.author: eugenesh
ms.openlocfilehash: 356ceb98106d080d8c24dedc3547bee33750156e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="indexer-operations-azure-search-service-rest-api-2015-02-28-preview"></a>Indexbewerkingen (Azure Search Service REST-API: 2015-02-28-Preview)
> [!NOTE]
> Dit artikel wordt beschreven indexeerfuncties in de [2015-02-28-Preview REST-API](search-api-2015-02-28-preview.md). Deze API-versie wordt preview-versies van Azure Blob Storage een indexeerfunctie met document uitpakken en Azure Table Storage indexeerfunctie, plus andere verbeteringen toegevoegd. De API ondersteunt ook algemeen beschikbaar indexeerfuncties (GA), met inbegrip van indexeerfuncties voor Azure SQL Database, SQL Server op Azure VM's en Azure Cosmos DB.
> 
> 

## <a name="overview"></a>Overzicht
Azure Search kunt integreren rechtstreeks met algemene gegevensbronnen opheffen van de noodzaak code schrijven om uw gegevens te indexeren. Als u deze up instelt, kunt u de Azure Search API voor het maken en beheren van aanroepen **indexeerfuncties** en **gegevensbronnen**. 

Een **indexeerfunctie** is een resource die gegevensbronnen met de zoekindexen doel verbindt. Een indexeerfunctie wordt gebruikt in de volgende manieren: 

* Uitvoeren van een momentopname van de gegevens naar een index te vullen.
* Een index met wijzigingen in de gegevensbron volgens een schema worden gesynchroniseerd. De planning maakt deel uit van de indexeerfunctie-definitie.
* Op aanvraag voor het bijwerken van een index, indien nodig worden aangeroepen. 

Een **indexeerfunctie** is handig als u regelmatig updates naar een index wilt. U kunt een inline-planning instellen als onderdeel van de definitie van een indexeerfunctie of uitvoeren op aanvraag met behulp van [uitvoeren indexeerfunctie](#RunIndexer). 

Een **gegevensbron** bepaalt welke gegevens moeten worden geïndexeerd, referenties voor toegang tot de gegevens en het beleid voor het inschakelen van Azure Search, wijzigingen in de gegevens (zoals gewijzigd of verwijderde rijen in een databasetabel) efficiënt te identificeren. Het gedefinieerd als een onafhankelijke resource zodat deze kan worden gebruikt door meerdere indexeerfuncties.

De volgende gegevensbronnen worden momenteel ondersteund:

* **Azure SQL Database** en **SQL Server op Azure Virtual machines**. Zie voor een gerichte procedure [in dit artikel](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md). 
* **Azure Cosmos DB**. Zie voor een gerichte procedure [in dit artikel](search-howto-index-documentdb.md). 
* **Azure Blob Storage**, waaronder de volgende indelingen document: PDF-, Microsoft Office (DOCX/DOC, XSLX/XLS, PPTX/PPT, bericht) HTML, XML, POSTCODE en tekst zonder opmaak bestanden (inclusief JSON). Zie voor een gerichte procedure [in dit artikel](search-howto-indexing-azure-blob-storage.md).
* **Azure-tabelopslag**. Zie voor een gerichte procedure [in dit artikel](search-howto-indexing-azure-tables.md).

We overweegt voortaan ondersteuning voor extra gegevensbronnen toe te voegen. Om deze beslissingen bepalen, Geef uw feedback op de [forum met feedback van Azure Search](http://feedback.azure.com/forums/263029-azure-search).

Zie [Servicelimieten](search-limits-quotas-capacity.md) voor maximaal limieten over de indexeerfunctie en data source-resources.

## <a name="typical-usage-flow"></a>Typische gebruiksscenario stroom
U kunt maken en beheren van Indexeerfuncties en gegevensbronnen via eenvoudige HTTP-aanvragen (POST, GET, PUT, DELETE) op basis van een bepaalde `data source` of `indexer` resource.

Het instellen van automatische indexering is doorgaans een proces vier stappen:

1. Identificeer de gegevensbron die de gegevens bevat die moet worden geïndexeerd. Houd er rekening mee dat Azure Search mogelijk geen ondersteuning voor alle gegevenstypen aanwezig is in de gegevensbron. Zie [ondersteunde gegevenstypen](https://msdn.microsoft.com/library/azure/dn798938.aspx) voor de lijst.
2. Een Azure Search-index waarvan het schema compatibel met uw gegevensbron is maken.
3. Maak een Azure Search-gegevensbron, zoals beschreven in [gegevensbron maken](#CreateDataSource).
4. Maak een Azure Search-indexeerfunctie zoals beschreven [indexeerfunctie maken](#CreateIndexer).

U moet over het maken van een indexeerfunctie voor elke combinatie target index en gegevensbron plannen. U kunt meerdere indexeerfuncties geschreven naar dezelfde index hebben en u kunt dezelfde gegevensbron voor meerdere indexeerfuncties hergebruiken. Echter een indexeerfunctie kan slechts één gegevensbron gebruiken op een tijdstip en kan alleen schrijven naar een enkele index. 

Na het maken van een indexeerfunctie kunt u ophalen de uitvoering status met het [indexeerfunctie Status ophalen](#GetIndexerStatus) bewerking. U kunt ook een indexeerfunctie uitvoeren op elke tijd (in plaats van of naast regelmatig uitgevoerd volgens een schema) die de [uitvoeren indexeerfunctie](#RunIndexer) bewerking.

<!-- MSDN has 2 art files plus a API topic link list -->


## <a name="create-data-source"></a>Gegevensbron maken
Een gegevensbron wordt in Azure Search met indexeerfuncties, bieden de verbindingsgegevens voor de ad-hoctaak of geplande gegevensvernieuwing van een doelindex gebruikt. U kunt een nieuwe gegevensbron binnen een Azure Search-service met behulp van een HTTP POST-aanvraag maken.

    POST https://[service name].search.windows.net/datasources?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin key]

U kunt ook gebruik van opslag en geef de naam van de gegevensbron op de URI. Als de gegevensbron niet bestaat nog, wordt deze gemaakt.

    PUT https://[service name].search.windows.net/datasources/[datasource name]?api-version=[api-version]

> [!NOTE]
> Het maximum aantal gegevensbronnen dat is toegestaan, hangt af van de prijscategorie. De gratis service kunnen maximaal 3-gegevensbronnen. Standard-service kan 50 gegevensbronnen. Zie [Servicelimieten](search-limits-quotas-capacity.md) voor meer informatie.
> 
> 

**Aanvraag**

HTTPS is vereist voor alle aanvragen van de service. De **gegevensbron maken** aanvraag kan worden geconstrueerd met een methode POST of PUT. Wanneer u POST, moet u de naam van een gegevensbron in de aanvraagtekst samen met de definitie van de gegevensbron opgeven. Met opslag is de naam onderdeel van de URL. Als de gegevensbron niet bestaat, wordt deze gemaakt. Als deze al bestaat, wordt deze bijgewerkt naar de nieuwe definitie. 

Naam van de gegevensbron moet in kleine letters worden, beginnen met een letter of cijfer, hebben geen slashes of punten en minder dan 128 tekens bevatten. De rest van de naam kan na het starten van de naam van gegevensbron met een letter of cijfer bevatten een letter, cijfer en streepjes, zolang de streepjes niet opeenvolgende zijn. Zie [naamgevingsregels](https://msdn.microsoft.com/library/azure/dn857353.aspx) voor meer informatie.

De `api-version` is vereist. De huidige versie is `2015-02-28`.

**Aanvraagheaders**

De volgende lijst beschrijft de vereiste en optionele aanvraagheaders. 

* `Content-Type`: Vereist. Stel dit in op`application/json`
* `api-key`: Vereist. De `api-key` wordt gebruikt voor verificatie van de aanvraag voor uw zoekservice. Het is een tekenreekswaarde die uniek is voor uw service. De **gegevensbron maken** aanvraag moet bevatten een `api-key` header ingesteld op de administratorsleutel (in plaats van een querysleutel). 

U moet ook de naam van de service om de aanvraag-URL samen te stellen. U krijgt de servicenaam en `api-key` van uw servicedashboard in de [Azure Portal](https://portal.azure.com/). Zie [maken van een service voor zoeken in de portal](search-create-service-portal.md) voor pagina navigatie hulp.

<a name="CreateDataSourceRequestSyntax"></a>
**Syntaxis van de aanvraag hoofdtekst**

De hoofdtekst van de aanvraag bevat een definitie van de gegevensbron, waaronder het type van de gegevensbron referenties voor het lezen van de gegevens, evenals een optionele gegevens detectie te wijzigen en verwijderen van een detectie beleid voor gegevens die worden gebruikt voor het efficiënt identificeren gewijzigd of verwijderd gegevens in de gegevensbron gebruikt in combinatie met een regelmatig geplande indexeerfunctie. 

De syntaxis voor het structureren nettolading van de aanvraag is als volgt. Een voorbeeld van een aanvraag wordt aangeboden op verder in dit onderwerp.

    { 
        "name" : "Required for POST, optional for PUT. The name of the data source",
        "description" : "Optional. Anything you want, or nothing at all",
        "type" : "Required. Must be one of 'azuresql', 'documentdb', 'azureblob', or 'azuretable'",
        "credentials" : { "connectionString" : "Required. Connection string for your data source" },
        "container" : { "name" : "Required. The name of the table, collection, or blob container you wish to index" },
        "dataChangeDetectionPolicy" : { Optional. See below for details }, 
        "dataDeletionDetectionPolicy" : { Optional. See below for details }
    }

Aanvraag bevat de volgende eigenschappen: 

* `name`: Vereist. De naam van de gegevensbron. Naam van een gegevensbron moet alleen kleine letters, cijfers of afbreekstreepjes bevatten, mag niet beginnen of eindigen met streepjes en is beperkt tot 128 tekens.
* `description`: Een optionele beschrijving. 
* `type`: Vereist. Moet een van de ondersteunde gegevensbrontypen:
  * `azuresql`-Azure SQL Database of SQL Server op Azure Virtual machines
  * `documentdb`-Cosmos azure DB
  * `azureblob`-Azure Blob-opslag
  * `azuretable`-Azure-tabelopslag
* `credentials`:
  * De vereiste `connectionString` eigenschap geeft u de verbindingsreeks voor de gegevensbron. De indeling van de verbindingsreeks is afhankelijk van het gegevensbrontype: 
    * Dit is de gebruikelijke SQL Server-verbindingsreeks voor Azure SQL. Als u de verbindingsreeks ophalen in de Azure-portal, gebruikt de `ADO.NET connection string` optie.
    * Azure Cosmos DB, moet de verbindingsreeks in de volgende indeling: `"AccountEndpoint=https://[your account name].documents.azure.com;AccountKey=[your account key];Database=[your database id]"`. Alle waarden zijn vereist. U vindt deze in de [Azure-portal](https://portal.azure.com/).  
    * Dit is de verbindingsreeks voor opslag-account voor Azure-Blob en Table Storage. Beschrijving van de notatie [hier](https://azure.microsoft.com/documentation/articles/storage-configure-connection-string/). HTTPS-eindpunt-protocol is vereist.  
* `container`, vereist: Hiermee geeft u de gegevens naar de index met behulp van de `name` en `query` eigenschappen: 
  * `name`Vereist:
    * Azure SQL: Hiermee geeft u de tabel of weergave. U kunt schema gekwalificeerde namen, zoals `[dbo].[mytable]`.
    * DocumentDB: Hiermee geeft u de verzameling. 
    * Azure Blob Storage: Hiermee geeft u de storage-container.
    * Azure Table Storage: Hiermee geeft u de naam van de tabel. 
  * `query`, optioneel:
    * DocumentDB: Hiermee kunt u een query die wordt samengevoegd een willekeurige indeling van de JSON-document in een platte schema dat u kunt de Azure Search index opgeven.  
    * Azure Blob Storage: kunt u een virtuele map in de blob-container opgeven. Bijvoorbeeld: voor blobpad `mycontainer/documents/blob.pdf`, `documents` kan worden gebruikt als de virtuele map.
    * Azure Table Storage: Hiermee kunt u een query waarmee de verzameling rijen moeten worden geïmporteerd gefilterd opgeven.
    * Azure SQL: query wordt niet ondersteund. Als u deze functionaliteit nodig, neem stemmen voor [deze suggestie](https://feedback.azure.com/forums/263029-azure-search/suggestions/9893490-support-user-provided-query-in-sql-indexer)
* De optionele `dataChangeDetectionPolicy` en `dataDeletionDetectionPolicy` eigenschappen worden hieronder beschreven.

<a name="DataChangeDetectionPolicies"></a>
**Beleid voor de detectie van gegevens wijzigen**

Het doel van een beleid gegevens wijzigen detectie is efficiënt gewijzigde gegevens om items te identificeren. Ondersteunde beleidsregels variëren, afhankelijk van het gegevensbrontype. De volgende secties beschrijven elk beleid. 

***Wijziging detectie Bovengrensbeleid*** 

Gebruik dit beleid wanneer de gegevensbron bevat een kolom of een eigenschap die voldoet aan de volgende criteria:

* Alle voegt een waarde opgeven voor de kolom. 
* Alle updates aan een item wordt ook de waarde van de kolom wijzigen. 
* De waarde van deze kolom wordt verhoogd met elke wijziging.
* Query's die gebruikmaken van een filter-component die vergelijkbaar zijn met de volgende `WHERE [High Water Mark Column] > [Current High Water Mark Value]` efficiënt kunnen worden uitgevoerd.

Bijvoorbeeld: bij gebruik van Azure SQL-gegevensbronnen, een geïndexeerde `rowversion` kolom is de ideale kandidaat voor gebruik met aan het beleid high-water mark. 

Dit beleid kan als volgt worden opgegeven:

    { 
        "@odata.type" : "#Microsoft.Azure.Search.HighWaterMarkChangeDetectionPolicy",
        "highWaterMarkColumnName" : "[a row version or last_updated column name]" 
    } 

Wanneer u Azure Cosmos DB-gegevensbronnen, moet u de `_ts` eigenschap verstrekt door Azure Cosmos DB. 

Bij gebruik van Azure Blob-gegevensbronnen, Azure Search automatisch maakt gebruik van een bovengrens wijzigingsbeleid detectie op basis van een blob het laatst is gewijzigd van het tijdstempel; u hoeft niet te dergelijk beleid zelf opgeeft.   

***In SQL geïntegreerde wijzigingsbeleid***

Als uw SQL database ondersteunt [bijhouden](https://msdn.microsoft.com/library/bb933875.aspx), wordt aangeraden SQL geïntegreerde wijzigen Traceringsbeleid. Dit beleid kunt de meest efficiënte bijhouden en kunt Azure Search, verwijderde rijen identificeren zonder dat u hoeft te hebben van een kolom met expliciete 'voorlopig verwijderen' in uw schema.

Geïntegreerd wijzigingen bijhouden wordt ondersteund met de volgende versies van SQL Server-database wordt gestart: 

* SQL Server 2008 R2, als u SQL Server op Azure Virtual machines.
* Azure SQL Database V12, als u Azure SQL Database.  

Wanneer met behulp van SQL geïntegreerde wijzigingen bijhouden beleid, geef geen een afzonderlijke gegevens detectie verwijderingsbeleid - dit beleid bevat ingebouwde ondersteuning voor het identificeren van verwijderde rijen. 

Dit beleid kan alleen worden gebruikt met tabellen. Deze kan niet worden gebruikt met weergaven. U moet wijzigingen bijhouden in de tabel die u voordat u dit beleid kunt inschakelen. Zie [inschakelen en uitschakelen van bijhouden](https://msdn.microsoft.com/library/bb964713.aspx) voor instructies.    

Structureren de **gegevensbron maken** aanvraagt, SQL geïntegreerde wijzigingen bijhouden van beleid kan als volgt worden opgegeven:

    { 
        "@odata.type" : "#Microsoft.Azure.Search.SqlIntegratedChangeTrackingPolicy" 
    }

<a name="DataDeletionDetectionPolicies"></a>
**Beleid voor de detectie van gegevens verwijderen**

Het doel van een gegevens-verwijderingsbeleid detectie is efficiënt verwijderde gegevens om items te identificeren. De enige ondersteunde beleid is momenteel de `Soft Delete` beleid, waardoor het identificeren van verwijderde items op basis van de waarde van een `soft delete` kolom of de eigenschap in de gegevensbron. Dit beleid kan als volgt worden opgegeven:

    { 
        "@odata.type" : "#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy",
        "softDeleteColumnName" : "the column that specifies whether a row was deleted", 
        "softDeleteMarkerValue" : "the value that identifies a row as deleted" 
    }

> [!NOTE]
> Alleen kolommen met een tekenreeks, geheel getal of Boole-waarden worden ondersteund. De waarde die wordt gebruikt als `softDeleteMarkerValue` moet een tekenreeks, zelfs als de bijbehorende kolom gehele getallen of Booleaanse waarden bevat. Bijvoorbeeld, als de waarde die wordt weergegeven in de gegevensbron 1 is, gebruikt u `"1"` als de `softDeleteMarkerValue`.    
> 
> 

<a name="CreateDataSourceRequestExamples"></a>
**Aanvraag hoofdtekst voorbeelden**

Als u van plan bent gebruik van de gegevensbron met een indexeerfunctie die volgens een planning wordt uitgevoerd, ziet u in dit voorbeeld wijzigen en verwijderen van een beleid voor detectie opgeven: 

    { 
        "name" : "asqldatasource",
        "description" : "a description",
        "type" : "azuresql",
        "credentials" : { "connectionString" : "Server=tcp:....database.windows.net,1433;Database=...;User ID=...;Password=...;Trusted_Connection=False;Encrypt=True;Connection Timeout=30;" },
        "container" : { "name" : "sometable" },
        "dataChangeDetectionPolicy" : { "@odata.type" : "#Microsoft.Azure.Search.HighWaterMarkChangeDetectionPolicy", "highWaterMarkColumnName" : "RowVersion" }, 
        "dataDeletionDetectionPolicy" : { "@odata.type" : "#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy", "softDeleteColumnName" : "IsDeleted", "softDeleteMarkerValue" : "true" }
    }

Als u alleen gebruiken van de gegevensbron voor eenmalige kopie van de gegevens wilt, kan het beleid kunnen worden weggelaten:

    { 
        "name" : "asqldatasource",
        "description" : "anything you want, or nothing at all",
        "type" : "azuresql",
        "credentials" : { "connectionString" : "Server=tcp:....database.windows.net,1433;Database=...;User ID=...;Password=...;Trusted_Connection=False;Encrypt=True;Connection Timeout=30;" },
        "container" : { "name" : "sometable" }
    } 

**Antwoord**

Aanvraag is gelukt: 201 gemaakt. 

<a name="UpdateDataSource"></a>

## <a name="update-data-source"></a>Gegevensbron bijwerken
U kunt een bestaande gegevensbron met behulp van een HTTP PUT-aanvraag bijwerken. U opgeven de naam van de gegevensbron bij te werken op de aanvraag-URI:

    PUT https://[service name].search.windows.net/datasources/[datasource name]?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin key]

De `api-version` is vereist. De huidige versie is `2015-02-28`. [Azure Search API-versies](https://msdn.microsoft.com/library/azure/dn864560.aspx) details en meer informatie over alternatieve versies heeft.

De `api-key` moet een beheersleutel (in plaats van een querysleutel). Raadpleeg het gedeelte verificatie in [REST-API van zoekservice](https://msdn.microsoft.com/library/azure/dn798935.aspx) voor meer informatie over sleutels. [Maken van een service voor zoeken in de portal](search-create-service-portal.md) wordt uitgelegd hoe u de service-URL en de key-eigenschappen die in de aanvraag.

**Aanvraag**

De syntaxis van de aanvraag hoofdtekst is hetzelfde als voor [gegevensbron maken aanvragen](#CreateDataSourceRequestSyntax).

Sommige eigenschappen kunnen niet worden bijgewerkt op een bestaande gegevensbron. Bijvoorbeeld, wijzigen u het type van een bestaande gegevensbron niet.  

Als u niet wilt wijzigen van de verbindingsreeks voor een bestaande gegevensbron, kunt u de letterlijke waarde `<unchanged>` voor de verbindingsreeks. Dit is handig in situaties waarin u wilt bijwerken van een bron, maar geen snel toegang tot de verbindingsreeks omdat het vertrouwelijke gegevens.

**Antwoord**

Aanvraag is gelukt: 201 gemaakt als een nieuwe gegevensbron is gemaakt en 204 geen inhoud als een bestaande gegevensbron is bijgewerkt.

<a name="ListDataSource"></a>

## <a name="list-data-sources"></a>Lijst met gegevensbronnen
De **lijst gegevensbronnen** bewerking retourneert een lijst van de gegevensbronnen in uw Azure Search-service. 

    GET https://[service name].search.windows.net/datasources?api-version=[api-version]
    api-key: [admin key]

De `api-version` is vereist. De huidige versie is `2015-02-28`. 

De `api-key` moet een beheersleutel (in plaats van een querysleutel). Raadpleeg het gedeelte verificatie in [REST-API van zoekservice](https://msdn.microsoft.com/library/azure/dn798935.aspx) voor meer informatie over sleutels. [Maken van een service voor zoeken in de portal](search-create-service-portal.md) wordt uitgelegd hoe u de service-URL en de key-eigenschappen die in de aanvraag.

**Antwoord**

Aanvraag is gelukt: 200 OK.

Hier volgt een voorbeeld-antwoordtekst:

    {
      "value" : [
        {
          "name": "datasource1",
          "type": "azuresql",
          ... other data source properties
        }]
    }

Houd er rekening mee dat u het antwoord naar beneden eigenschappen die u geïnteresseerd bent in kunt filteren. Bijvoorbeeld: als u wilt dat alleen een lijst met namen van gegevensbronnen, gebruiken de OData `$select` query-optie:

    GET /datasources?api-version=205-02-28&$select=name

In dit geval wordt zou het antwoord van het bovenstaande voorbeeld als volgt uitzien: 

    {
      "value" : [ { "name": "datasource1" }, ... ]
    }

Dit is een nuttig techniek bandbreedte opslaan als er een groot aantal gegevensbronnen in uw zoekservice.

<a name="GetDataSource"></a>

## <a name="get-data-source"></a>Gegevensbron ophalen
De **gegevensbron ophalen** bewerking definitie van de gegevensbron opgehaald uit Azure Search.

    GET https://[service name].search.windows.net/datasources/[datasource name]?api-version=[api-version]
    api-key: [admin key]

De `api-version` is vereist. De huidige versie is `2015-02-28`. 

De `api-key` moet een beheersleutel (in plaats van een querysleutel). Raadpleeg het gedeelte verificatie in [REST-API van zoekservice](https://msdn.microsoft.com/library/azure/dn798935.aspx) voor meer informatie over sleutels. [Maken van een service voor zoeken in de portal](search-create-service-portal.md) wordt uitgelegd hoe u de service-URL en de key-eigenschappen die in de aanvraag.

**Antwoord**

Statuscode: 200 OK wordt geretourneerd voor een geslaagde reactie.

Het antwoord is vergelijkbaar met de voorbeelden in [gegevensbron maken voorbeeld aanvragen](#CreateDataSourceRequestExamples): 

    { 
        "name" : "asqldatasource",
        "description" : "a description",
        "type" : "azuresql",
        "credentials" : { "connectionString" : "Server=tcp:....database.windows.net,1433;Database=...;User ID=...;Password=...;Trusted_Connection=False;Encrypt=True;Connection Timeout=30;" },
        "container" : { "name" : "sometable" },
        "dataChangeDetectionPolicy" : { 
            "@odata.type" : "#Microsoft.Azure.Search.HighWaterMarkChangeDetectionPolicy",
            "highWaterMarkColumnName" : "RowVersion" }, 
        "dataDeletionDetectionPolicy" : { 
            "@odata.type" : "#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy",
            "softDeleteColumnName" : "IsDeleted", 
            "softDeleteMarkerValue" : "true" }
    }

> [!NOTE]
> Stel de `Accept` aanvraagheader naar `application/json;odata.metadata=none` wanneer deze API als in dat geval wordt aangeroepen, wordt `@odata.type` kenmerk moeten worden weggelaten uit het antwoord en u niet mogelijk onderscheid maken tussen de gegevens wijzigen en verwijderen van een detectie beleid van verschillende typen. 
> 
> 

<a name="DeleteDataSource"></a>

## <a name="delete-data-source"></a>Gegevensbron verwijderen
De **gegevensbron verwijderen** bewerking verwijdert u een gegevensbron van uw Azure Search-service.

    DELETE https://[service name].search.windows.net/datasources/[datasource name]?api-version=[api-version]
    api-key: [admin key]

> [!NOTE]
> Als alle indexeerfuncties verwijst naar de gegevensbron die u wilt verwijderen, worden de delete-bewerking wordt nog voortgezet. Deze indexeerfuncties echter verandert in een foutstatus van de volgende keer wordt uitgevoerd.  
> 
> 

De `api-version` is vereist. De huidige versie is `2015-02-28`. 

De `api-key` moet een beheersleutel (in plaats van een querysleutel). Raadpleeg het gedeelte verificatie in [REST-API van zoekservice](https://msdn.microsoft.com/library/azure/dn798935.aspx) voor meer informatie over sleutels. [Maken van een service voor zoeken in de portal](search-create-service-portal.md) wordt uitgelegd hoe u de service-URL en de key-eigenschappen die in de aanvraag.

**Antwoord**

Statuscode: 204 geen inhoud wordt geretourneerd voor een geslaagde reactie.

<a name="CreateIndexer"></a>

## <a name="create-indexer"></a>Maak indexeerfunctie
U kunt een nieuwe indexeerfunctie binnen een Azure Search-service met behulp van een HTTP POST-aanvraag maken.

    POST https://[service name].search.windows.net/indexers?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin key]

U kunt ook gebruik van opslag en geef de naam van de gegevensbron op de URI. Als de gegevensbron niet bestaat nog, wordt deze gemaakt.

    PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=[api-version]

> [!NOTE]
> Het maximum aantal toegestane indexeerfuncties varieert per prijscategorie. De gratis service kunnen maximaal 3 indexeerfuncties. Standard-service kan 50 indexeerfuncties. Zie [Servicelimieten](search-limits-quotas-capacity.md) voor meer informatie.
> 
> 

De `api-version` is vereist. De huidige versie is `2015-02-28`. 

De `api-key` moet een beheersleutel (in plaats van een querysleutel). Raadpleeg het gedeelte verificatie in [REST-API van zoekservice](https://msdn.microsoft.com/library/azure/dn798935.aspx) voor meer informatie over sleutels. [Maken van een service voor zoeken in de portal](search-create-service-portal.md) wordt uitgelegd hoe u de service-URL en de key-eigenschappen die in de aanvraag.

<a name="CreateIndexerRequestSyntax"></a>
**Syntaxis van de aanvraag hoofdtekst**

De hoofdtekst van de aanvraag bevat een definitie van een indexeerfunctie, waarmee de gegevensbron en de doelindex voor indexeren, evenals optionele indexering planning en parameters. 

De syntaxis voor het structureren nettolading van de aanvraag is als volgt. Een voorbeeld van een aanvraag wordt aangeboden op verder in dit onderwerp.

    { 
        "name" : "Required for POST, optional for PUT. The name of the indexer",
        "description" : "Optional. Anything you want, or null",
        "dataSourceName" : "Required. The name of an existing data source",
        "targetIndexName" : "Required. The name of an existing index",
        "schedule" : { Optional. See Indexing Schedule below. },
        "parameters" : { Optional. See Indexing Parameters below. },
        "fieldMappings" : { Optional. See Field Mappings below. },
        "disabled" : Optional boolean value indicating whether the indexer is disabled. False by default.  
    }

**Indexeerfunctie planning**

Een indexeerfunctie kunt optioneel een planning opgeven. Als een planning aanwezig is, wordt de indexeerfunctie regelmatig uitgevoerd volgens de planning. Schema heeft de volgende kenmerken:

* `interval`: Vereist. Een duration-waarde die een interval of de periode voor de indexeerfunctie aangeeft wordt uitgevoerd. De minimaal toegestane interval is 5 minuten. het langste is één dag. Moet zijn geformatteerd als de waarde van een XSD-'dayTimeDuration' (een beperkte subset van een [duur van de ISO 8601](http://www.w3.org/TR/xmlschema11-2/#dayTimeDuration) waarde). Het patroon hiervoor is: `"P[nD][T[nH][nM]]"`. Voorbeelden: `PT15M` voor elke 15 minuten `PT2H` voor elke 2 uur. 
* `startTime`: Vereist. Een UTC datetime wanneer de indexeerfunctie moet zijn gestart. 

**Indexeerfunctie Parameters**

Een indexeerfunctie kunt eventueel verschillende parameters die invloed hebben op het gedrag opgeven. Alle parameters zijn optioneel.  

* `maxFailedItems`: Het aantal items dat niet kan worden geïndexeerd voordat de uitvoering van een indexeerfunctie wordt beschouwd als een fout. De standaardwaarde is 0. Informatie over mislukte items wordt geretourneerd door de [indexeerfunctie Status ophalen](#GetIndexerStatus) bewerking. 
* `maxFailedItemsPerBatch`: Het aantal items dat niet kan worden geïndexeerd in elke batch voordat de uitvoering van een indexeerfunctie wordt beschouwd als een fout. De standaardwaarde is 0.
* `base64EncodeKeys`: Geeft aan of document sleutels base 64-codering. Azure Search gelden beperkingen voor tekens die gebruikt in een documentsleutel worden kunnen. De waarden in de brongegevens bevatten echter tekens die ongeldig zijn. Als het indexeren van dergelijke waarden als document sleutels nodig is, deze vlag worden ingesteld op true. Standaard is `false`.
* `batchSize`: Hiermee geeft u het aantal items die zijn gelezen uit de gegevensbron en geïndexeerd als één batch om de prestaties verbeteren. De standaardwaarde is afhankelijk van het gegevensbrontype: is 1000 voor Azure SQL en Azure Cosmos DB en 10 voor Azure Blob Storage.

**Veld-verwijzingen**

Veldtoewijzingen kunt u de naam van een veld in de gegevensbron worden toegewezen aan een andere veldnaam in de doelindex. Neem bijvoorbeeld een brontabel met een veld `_id`. Azure Search toegestaan niet in de naam van een veld met een onderstrepingsteken beginnen, zodat de naam van het veld moet worden gewijzigd. U kunt dit doen met behulp van de `fieldMappings` eigenschap van de indexeerfunctie als volgt: 

    "fieldMappings" : [ { "sourceFieldName" : "_id", "targetFieldName" : "id" } ] 

U kunt meerdere veldtoewijzingen opgeven: 

    "fieldMappings" : [ 
        { "sourceFieldName" : "_id", "targetFieldName" : "id" },
        { "sourceFieldName" : "_timestamp", "targetFieldName" : "timestamp" },
     ]

Bron- en doelserver veldnamen zijn hoofdlettergevoelig.

<a name="FieldMappingFunctions"></a>
***Veld toewijzing functies***

Veldtoewijzingen kunnen ook worden gebruikt voor het transformeren van bron veldwaarden met *toewijzing functies*.

Slechts één van deze functie wordt momenteel ondersteund: `jsonArrayToStringCollection`. Een veld met een tekenreeks die is opgemaakt als een JSON-matrix in een verzameling (EDM.String) veld in de doelindex worden geparseerd. Het is bedoeld voor gebruik met Azure SQL-indexeerfunctie in het bijzonder omdat SQL beschikt niet over een eigen verzamelingsgegevenstype. Worden kan gebruikt als volgt: 

    "fieldMappings" : [ { "sourceFieldName" : "tags", "mappingFunction" : { "name" : "jsonArrayToStringCollection" } } ] 

Bijvoorbeeld, als het bronveld bevat de tekenreeks `["red", "white", "blue"]`, klikt u vervolgens het doelveld van het type `Collection(Edm.String)` wordt gevuld met de drie waarden `"red"`, `"white"` en `"blue"`.

Houd er rekening mee dat de `targetFieldName` eigenschap is optioneel; als weggelaten, de `sourceFieldName` waarde wordt gebruikt. 

<a name="CreateIndexerRequestExamples"></a>
**Aanvraag hoofdtekst voorbeelden**

Het volgende voorbeeld wordt een indexeerfunctie waarmee gegevens worden gekopieerd uit de tabel waarnaar wordt verwezen door de `ordersds` gegevensbron naar de `orders` index volgens een schema dat begint op 1 januari 2015 UTC en wordt elk uur wordt uitgevoerd. Elke aanroep van indexeerfunctie zijn geslaagd als niet meer dan 5 items niet worden geïndexeerd in elke batch en niet meer dan 10 items niet worden geïndexeerd in totaal. 

    {
        "name" : "myindexer",
        "description" : "a cool indexer",
        "dataSourceName" : "ordersds",
        "targetIndexName" : "orders",
        "schedule" : { "interval" : "PT1H", "startTime" : "2015-01-01T00:00:00Z" },
        "parameters" : { "maxFailedItems" : 10, "maxFailedItemsPerBatch" : 5, "base64EncodeKeys": false }
    }

**Antwoord**

201 gemaakt voor de aanvraag is gelukt.

<a name="UpdateIndexer"></a>

## <a name="update-indexer"></a>Indexeerfunctie bijwerken
U kunt een bestaande indexeerfunctie met behulp van een HTTP PUT-aanvraag bijwerken. U opgeven de naam van de indexeerfunctie om bij te werken op de aanvraag-URI:

    PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin key]

De `api-version` is vereist. De huidige versie is `2015-02-28`. 

De `api-key` moet een beheersleutel (in plaats van een querysleutel). Raadpleeg het gedeelte verificatie in [REST-API van zoekservice](https://msdn.microsoft.com/library/azure/dn798935.aspx) voor meer informatie over sleutels. [Maken van een service voor zoeken in de portal](search-create-service-portal.md) wordt uitgelegd hoe u de service-URL en de key-eigenschappen die in de aanvraag.

**Aanvraag**

De syntaxis van de aanvraag hoofdtekst is hetzelfde als voor [maken indexeerfunctie aanvragen](#CreateIndexerRequestSyntax).

**Antwoord**

Aanvraag is gelukt: 201 gemaakt als een nieuwe indexeerfunctie is gemaakt en 204 geen inhoud als een bestaande indexeerfunctie is bijgewerkt.

<a name="ListIndexers"></a>

## <a name="list-indexers"></a>Lijst met indexeerfuncties
De **lijst indexeerfuncties** bewerking retourneert de lijst met indexeerfuncties in uw Azure Search-service. 

    GET https://[service name].search.windows.net/indexers?api-version=[api-version]
    api-key: [admin key]


De `api-version` is vereist. De preview-versie is `2015-02-28-Preview`. [Azure Search versioning](https://msdn.microsoft.com/library/azure/dn864560.aspx) details en meer informatie over alternatieve versies heeft.

De `api-key` moet een beheersleutel (in plaats van een querysleutel). Raadpleeg het gedeelte verificatie in [REST-API van zoekservice](https://msdn.microsoft.com/library/azure/dn798935.aspx) voor meer informatie over sleutels. [Maken van een service voor zoeken in de portal](search-create-service-portal.md) wordt uitgelegd hoe u de service-URL en de key-eigenschappen die in de aanvraag.

**Antwoord**

Aanvraag is gelukt: 200 OK.

Hier volgt een voorbeeld-antwoordtekst:

    {
      "value" : [
      {
        "name" : "myindexer",
        "description" : "a cool indexer",
        "dataSourceName" : "ordersds",
        "targetIndexName" : "orders",
        ... other indexer properties
      }]
    }

Houd er rekening mee dat u het antwoord naar beneden eigenschappen die u geïnteresseerd bent in kunt filteren. Bijvoorbeeld: als u alleen een lijst met namen van de indexeerfunctie wilt, gebruiken de OData `$select` query-optie:

    GET /indexers?api-version=2014-10-20-Preview&$select=name

In dit geval wordt zou het antwoord van het bovenstaande voorbeeld als volgt uitzien: 

    {
      "value" : [ { "name": "myindexer" } ]
    }

Dit is een nuttig techniek bandbreedte opslaan als er een groot aantal indexeerfuncties in uw zoekservice.

<a name="GetIndexer"></a>

## <a name="get-indexer"></a>Indexeerfunctie ophalen
De **ophalen indexeerfunctie** bewerking krijgt de definitie van de indexeerfunctie van Azure Search.

    GET https://[service name].search.windows.net/indexers/[indexer name]?api-version=[api-version]
    api-key: [admin key]

De `api-version` is vereist. De preview-versie is `2015-02-28-Preview`. 

De `api-key` moet een beheersleutel (in plaats van een querysleutel). Raadpleeg het gedeelte verificatie in [REST-API van zoekservice](https://msdn.microsoft.com/library/azure/dn798935.aspx) voor meer informatie over sleutels. [Maken van een service voor zoeken in de portal](search-create-service-portal.md) wordt uitgelegd hoe u de service-URL en de key-eigenschappen die in de aanvraag.

**Antwoord**

Statuscode: 200 OK wordt geretourneerd voor een geslaagde reactie.

Het antwoord is vergelijkbaar met de voorbeelden in [indexeerfunctie maken voorbeeld aanvragen](#CreateIndexerRequestExamples): 

    {
        "name" : "myindexer",
        "description" : "a cool indexer",
        "dataSourceName" : "ordersds",
        "targetIndexName" : "orders",
        "schedule" : { "interval" : "PT1H", "startTime" : "2015-01-01T00:00:00Z" },
        "parameters" : { "maxFailedItems" : 10, "maxFailedItemsPerBatch" : 5, "base64EncodeKeys": false }
    }


<a name="DeleteIndexer"></a>

## <a name="delete-indexer"></a>Indexeerfunctie verwijderen
De **indexeerfunctie verwijderen** bewerking verwijdert een indexeerfunctie van uw Azure Search-service.

    DELETE https://[service name].search.windows.net/indexers/[indexer name]?api-version=[api-version]
    api-key: [admin key]

Wanneer een indexeerfunctie wordt verwijderd, de indexeerfunctie uitvoeringen uitgevoerd op dat moment wordt uitgevoerd in uitvoering, maar er is geen verdere uitvoeringen wordt gepland. Gebruik een niet-bestaande indexeerfunctie zal resulteren in HTTP-statuscode 404 niet gevonden. 

De `api-version` is vereist. De preview-versie is `2015-02-28-Preview`. 

De `api-key` moet een beheersleutel (in plaats van een querysleutel). Raadpleeg het gedeelte verificatie in [REST-API van zoekservice](https://msdn.microsoft.com/library/azure/dn798935.aspx) voor meer informatie over sleutels. [Maken van een service voor zoeken in de portal](search-create-service-portal.md) wordt uitgelegd hoe u de service-URL en de key-eigenschappen die in de aanvraag.

**Antwoord**

Statuscode: 204 geen inhoud wordt geretourneerd voor een geslaagde reactie.

<a name="RunIndexer"></a>

## <a name="run-indexer"></a>Indexeerfunctie uitvoeren
Naast het periodiek wordt uitgevoerd op een planning, kan een indexeerfunctie ook worden aangeroepen op aanvraag via de **uitvoeren indexeerfunctie** bewerking: 

    POST https://[service name].search.windows.net/indexers/[indexer name]/run?api-version=[api-version]
    api-key: [admin key]

De `api-version` is vereist. De preview-versie is `2015-02-28-Preview`. 

De `api-key` moet een beheersleutel (in plaats van een querysleutel). Raadpleeg het gedeelte verificatie in [REST-API van zoekservice](https://msdn.microsoft.com/library/azure/dn798935.aspx) voor meer informatie over sleutels. [Maken van een service voor zoeken in de portal](search-create-service-portal.md) wordt uitgelegd hoe u de service-URL en de key-eigenschappen die in de aanvraag.

**Antwoord**

Statuscode: 202 geaccepteerde wordt geretourneerd voor een geslaagde reactie.

<a name="GetIndexerStatus"></a>

## <a name="get-indexer-status"></a>Status van de indexeerfunctie ophalen
De **indexeerfunctie Status ophalen** bewerking wordt de huidige status en de uitvoering van de geschiedenis van een indexeerfunctie opgehaald: 

    GET https://[service name].search.windows.net/indexers/[indexer name]/status?api-version=[api-version]
    api-key: [admin key]


De `api-version` is vereist. De preview-versie is `2015-02-28-Preview`. 

De `api-key` moet een beheersleutel (in plaats van een querysleutel). Raadpleeg het gedeelte verificatie in [REST-API van zoekservice](https://msdn.microsoft.com/library/azure/dn798935.aspx) voor meer informatie over sleutels. [Maken van een service voor zoeken in de portal](search-create-service-portal.md) wordt uitgelegd hoe u de service-URL en de key-eigenschappen die in de aanvraag.

**Antwoord**

Statuscode: 200 OK voor een geslaagde reactie.

Hoofdtekst van de reactie bevat informatie over de algehele status van de indexeerfunctie, de laatste aanroep van indexeerfunctie, evenals de geschiedenis van recente indexeerfunctie aanroepen (indien aanwezig). 

Een voorbeeld-antwoordtekst ziet er als volgt: 

    {
        "status":"running",
        "lastResult": {
            "status":"success",
            "errorMessage":null,
            "startTime":"2014-11-26T03:37:18.853Z",
            "endTime":"2014-11-26T03:37:19.012Z",
            "errors":[],
            "itemsProcessed":11,
            "itemsFailed":0,
            "initialTrackingState":null,
            "finalTrackingState":null
         },
        "executionHistory":[ {
            "status":"success",
             "errorMessage":null,
            "startTime":"2014-11-26T03:37:18.853Z",
            "endTime":"2014-11-26T03:37:19.012Z",
            "errors":[],
            "itemsProcessed":11,
            "itemsFailed":0,
            "initialTrackingState":null,
            "finalTrackingState":null
        }]
    }

**Status van de indexeerfunctie**

Status van de indexeerfunctie kan een van de volgende waarden zijn:

* `running`Hiermee wordt aangegeven dat de indexeerfunctie normaal wordt uitgevoerd. Houd er rekening mee dat sommige van de indexeerfunctie uitvoeringen nog steeds is beschadigd, dus is het een goed idee om de `lastResult` ook de eigenschap. 
* `error`Hiermee wordt aangegeven dat de indexeerfunctie is een fout die niet kan worden hersteld zonder menselijke tussenkomst opgetreden. Bijvoorbeeld, de referenties van de gegevensbron is mogelijk verlopen of het schema van de gegevensbron of de doelindex is gewijzigd in een recente manier. 

**Resultaat van uitvoering indexeerfunctie**

Het resultaat van een indexeerfunctie-uitvoering bevat informatie over een enkel indexeerfunctie worden uitgevoerd. Het laatste resultaat wordt opgehaald als de `lastResult` eigenschap van de status van de indexeerfunctie. Andere recente resultaten, indien aanwezig, worden geretourneerd als de `executionHistory` eigenschap van de status van de indexeerfunctie. 

Resultaat van uitvoering indexeerfunctie bevat de volgende eigenschappen: 

* `status`: de status van de uitvoering van een. Zie [indexeerfunctie Execution Status](#IndexerExecutionStatus) hieronder voor meer informatie. 
* `errorMessage`: foutbericht voor een mislukte uitvoering. 
* `startTime`: de tijd in UTC wanneer deze-uitvoering wordt gestart.
* `endTime`: de tijd in UTC wanneer deze-uitvoering is beëindigd. Deze waarde is niet ingesteld als de uitvoering nog uitgevoerd wordt.
* `errors`: een matrix van itemniveau fouten, indien van toepassing. Elk item bevat een documentsleutel (`key` eigenschap) en een foutbericht (`errorMessage` eigenschap). 
* `itemsProcessed`: het aantal items (bijvoorbeeld tabelrijen) dat de indexeerfunctie wordt geprobeerd om te indexeren tijdens het uitvoeren van deze gegevensbron. 
* `itemsFailed`: het aantal items dat is mislukt tijdens het uitvoeren van deze.  
* `initialTrackingState`: altijd `null` voor de eerste uitvoering van de indexeerfunctie of als de gegevens bijhouden beleid wijzigt, is niet ingeschakeld op de gegevensbron die wordt gebruikt. Als dit beleid is ingeschakeld, in een volgende uitvoering deze waarde geeft de eerste (laagste) bijhouden waarde verwerkt door deze kan worden uitgevoerd. 
* `finalTrackingState`: altijd `null` als de gegevens bijhouden beleid wijzigt, is niet ingeschakeld op de gegevensbron die wordt gebruikt. Anders geeft de meest recente (hoogste) bijhouden waarde is verwerkt door deze kan worden uitgevoerd. 

<a name="IndexerExecutionStatus"></a>
**Uitvoeringsstatus van indexeerfunctie**

Uitvoeringsstatus van indexeerfunctie bevat de status van een enkele indexeerfunctie worden uitgevoerd. Dit kan de volgende waarden hebben:

* `success`Hiermee wordt aangegeven dat de uitvoering van de indexeerfunctie is voltooid.
* `inProgress`Hiermee wordt aangegeven dat de uitvoering van de indexeerfunctie uitgevoerd wordt. 
* `transientFailure`Hiermee wordt aangegeven dat de uitvoering van een indexeerfunctie is mislukt. Zie `errorMessage` eigenschap voor meer informatie. De fout kan of kan geen menselijke tussenkomst om op te lossen vereist - bijvoorbeeld een incompatibel schema tussen de gegevensbron en de doelindex is hersteld vereist in te grijpen, maar niet door een tijdelijke gegevensbron uitvaltijd. Aanroepen van de indexeerfunctie wordt voortgezet per planning, indien aanwezig. 
* `persistentFailure`Hiermee wordt aangegeven dat de indexeerfunctie is mislukt op een manier die menselijke tussenkomst vereist. Geplande indexeerfunctie uitvoeringen gestopt. Gebruik nadat het probleem wordt opgelost, indexeerfunctie API opnieuw instellen om opnieuw te starten van de geplande uitvoeringen. 
* `reset`Hiermee wordt aangegeven dat de indexeerfunctie is opnieuw ingesteld door een aanroep van indexeerfunctie API opnieuw instellen (Zie hieronder). 

<a name="ResetIndexer"></a>

## <a name="reset-indexer"></a>Indexeerfunctie opnieuw instellen
De **indexeerfunctie opnieuw** bewerking stelt de status is gekoppeld aan de indexeerfunctie voor bijhouden. Hiermee kunt u voor het activeren van het begin opnieuw indexeren (bijvoorbeeld als het schema van de gegevensbron is gewijzigd) of om de gegevens wijzigen detectie beleid voor een gegevensbron die is gekoppeld aan de indexeerfunctie te wijzigen.   

    POST https://[service name].search.windows.net/indexers/[indexer name]/reset?api-version=[api-version]
    api-key: [admin key]

De `api-version` is vereist. De preview-versie is `2015-02-28-Preview`. 

De `api-key` moet een beheersleutel (in plaats van een querysleutel). Raadpleeg het gedeelte verificatie in [REST-API van zoekservice](https://msdn.microsoft.com/library/azure/dn798935.aspx) voor meer informatie over sleutels. [Maken van een service voor zoeken in de portal](search-create-service-portal.md) wordt uitgelegd hoe u de service-URL en de key-eigenschappen die in de aanvraag.

**Antwoord**

Statuscode: 204 geen inhoud voor een geslaagde reactie.

## <a name="mapping-between-sql-data-types-and-azure-search-data-types"></a>Toewijzing tussen de SQL-gegevenstypen en Azure Search-gegevenstypen
<table style="font-size:12">
<tr>
<td>SQL-gegevenstype</td>    
<td>Toegestaan doelindex veldtypen</td>
<td>Opmerkingen</td>
</tr>
<tr>
<td>bits</td>
<td>Edm.Boolean, Edm.String</td>
<td></td>
</tr>
<tr>
<td>int en smallint, tinyint</td>
<td>Edm.Int32, Edm.Int64, Edm.String</td>
<td></td>
</tr>
<tr>
<td>bigint</td>
<td>Edm.Int64, Edm.String</td>
<td></td>
</tr>
<tr>
<td>reële float</td>
<td>Edm.Double, Edm.String</td>
<td></td>
</tr>
<tr>
<td>smallmoney, geld<br>Decimale<br>numerieke
</td>
<td>Edm.String</td>
<td>Azure Search biedt geen ondersteuning voor het decimale typen converteren naar Edm.Double omdat dit precisie verliezen
</td>
</tr>
<tr>
<td>CHAR, nchar, varchar, nvarchar</td>
<td>Edm.String<br/>Collection(EDM.String)</td>
<td>Zie [veld toewijzing functies](#FieldMappingFunctions) in dit document voor meer informatie over het voor een kolom met tekenreeksen omzetten naar een verzameling (EDM.String)</td>
</tr>
<tr>
<td>smalldatetime, datetime, datetime2, date, datetimeoffset</td>
<td>Edm.DateTimeOffset, Edm.String</td>
<td></td>
</tr>
<tr>
<td>uniqueidentifer</td>
<td>Edm.String</td>
<td></td>
</tr>
<tr>
<td>Geografie</td>
<td>Edm.GeographyPoint</td>
<td>Alleen Geografie exemplaren van het type punt met SRID 4326 (dit is de standaardinstelling) worden ondersteund</td>
</tr>
<tr>
<td>ROWVERSION</td>
<td>N.v.t.</td>
<td>Kolommen van de rij-versie kunnen niet worden opgeslagen in de search-index, maar ze kunnen worden gebruikt voor het bijhouden</td>
</tr>
<tr>
<td>tijd, timespan<br>Binary, varbinary, afbeelding,<br>XML, geometry, CLR-typen</td>
<td>N.v.t.</td>
<td>Niet ondersteund</td>
</tr>
</table>

## <a name="mapping-between-json-data-types-and-azure-search-data-types"></a>Toewijzing tussen JSON-gegevenstypen en Azure Search-gegevenstypen
<table style="font-size:12">
<tr>
<td>JSON-gegevenstype</td>    
<td>Toegestaan doelindex veldtypen</td>
<td>Opmerkingen</td>
</tr>
<tr>
<td>BOOL</td>
<td>Edm.Boolean, Edm.String</td>
<td></td>
</tr>
<tr>
<td>Integraal cijfers</td>
<td>Edm.Int32, Edm.Int64, Edm.String</td>
<td></td>
</tr>
<tr>
<td>Getallen met drijvende komma</td>
<td>Edm.Double, Edm.String</td>
<td></td>
</tr>
<tr>
<td>Tekenreeks</td>
<td>Edm.String</td>
<td></td>
</tr>
<tr>
<td>matrices met primitieve typen, bijvoorbeeld ["a", "b", "c"]</td>
<td>Collection(EDM.String)</td>
<td></td>
</tr>
<tr>
<td>Tekenreeksen die lijken op datums</td>
<td>Edm.DateTimeOffset, Edm.String</td>
<td></td>
</tr>
<tr>
<td>GeoJSON point-objecten</td>
<td>Edm.GeographyPoint</td>
<td>GeoJSON punten zijn JSON-objecten in de volgende indeling: {'type': 'Point', 'coordinates': [lang, lat]} </td>
</tr>
<tr>
<td>Andere JSON-objecten</td>
<td>N.v.t.</td>
<td>Niet ondersteund. Azure Search ondersteunt momenteel alleen primitieve typen en tekenreeks verzamelingen</td>
</tr>
</table>
