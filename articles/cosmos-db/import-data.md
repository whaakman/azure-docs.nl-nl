---
title: Hulpprogramma voor migratie van database voor Azure Cosmos DB | Microsoft Docs
description: Informatie over het gebruik van de open-source Azure Cosmos DB hulpprogramma's voor gegevensmigratie voor het importeren van gegevens naar Azure Cosmos DB uit diverse bronnen, met inbegrip van MongoDB, SQL Server, Table storage, Amazon DynamoDB, CSV en JSON-bestanden. CSV naar JSON converteren.
keywords: CSV in json, 's voor migratie van databases, csv niet converteren naar json
services: cosmos-db
author: andrewhoh
manager: jhubbard
editor: monicar
documentationcenter: 
ms.assetid: d173581d-782a-445c-98d9-5e3c49b00e25
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/15/2017
ms.author: anhoh
ms.custom: mvc
ms.openlocfilehash: 103f4200ea24c34c066a11c7b49676f51f252589
ms.sourcegitcommit: 0e4491b7fdd9ca4408d5f2d41be42a09164db775
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/14/2017
---
# <a name="azure-cosmos-db-data-migration-tool"></a>Azure Cosmos DB: Hulpprogramma voor gegevensmigratie

[!INCLUDE [cosmos-db-sql-api](../../includes/cosmos-db-sql-api.md)]

Deze zelfstudie bevat instructies over het gebruik van het hulpprogramma Azure Cosmos DB Data Migration, waarmee gegevens uit diverse bronnen in Azure Cosmos DB verzamelingen en tabellen importeren kunt. U kunt importeren uit JSON-bestanden, CSV-bestanden, SQL, MongoDB, Azure Table storage, Amazon DynamoDB en zelfs Azure Cosmos DB SQL-API verzamelingen en u migreert die gegevens naar verzamelingen en tabellen voor met Azure Cosmos DB gebruiken. Het hulpprogramma voor migratie van gegevens kan ook worden gebruikt bij het migreren van een verzameling van één partitie in een verzameling met meerdere partitie voor de SQL-API.

Welke API u gaat gebruiken met Azure Cosmos DB? 
* **[SQL-API](documentdb-introduction.md)**  -kunt u een van de bronopties in het hulpprogramma voor migratie van gegevens om gegevens te importeren.
* **[Tabel API](table-introduction.md)**  -u kunt het hulpprogramma voor gegevensmigratie of AzCopy gebruiken om gegevens te importeren. Zie [importeren van gegevens voor gebruik met de Azure-API voor tabel Cosmos DB](table-import.md) voor meer informatie.
* **[MongoDB API](mongodb-introduction.md)**  -hulpprogramma voor migratie van de gegevens van ondersteunt momenteel geen Azure Cosmos DB MongoDB-API als een bron of als een doelserver. Als u migreren van de gegevens in of buiten het MongoDB-API-verzamelingen in Azure Cosmos DB wilt, raadpleegt u [Azure Cosmos DB: het migreren van gegevens voor de MongoDB-API](mongodb-migrate.md) voor instructies. U kunt het hulpprogramma voor migratie van gegevens nog steeds gegevens van MongoDB exporteren naar Azure Cosmos DB SQL-API-verzamelingen voor gebruik met de SQL-API. 
* **[Graph API](graph-introduction.md)**  -hulpprogramma voor migratie van de gegevens is niet een ondersteunde import-hulpprogramma voor Graph API accounts op dit moment. 

Deze zelfstudie bevat de volgende taken:

> [!div class="checklist"]
> * Het hulpprogramma voor gegevensmigratie installeren
> * Het importeren van gegevens uit verschillende gegevensbronnen
> * Exporteren van Azure Cosmos DB naar JSON

## <a id="Prerequisites"></a>Vereisten
Voordat u de instructies in dit artikel uitvoert, zorg ervoor dat u het volgende zijn geïnstalleerd hebt:

* [Microsoft .NET Framework 4,51](https://www.microsoft.com/download/developer-tools.aspx) of hoger.

## <a id="Overviewl"></a>Overzicht
Het hulpprogramma voor migratie van gegevens is een open-source-oplossing waarmee gegevens worden geïmporteerd met Azure Cosmos DB uit diverse bronnen, met inbegrip van:

* JSON-bestanden
* MongoDB
* SQL Server
* CSV-bestanden
* Azure Table Storage
* Amazon DynamoDB
* HBase
* Azure DB Cosmos-verzamelingen

Terwijl het hulpprogramma voor importeren, bevat een grafische gebruikersinterface (dtui.exe), kan het ook aangestuurd vanaf de opdrachtregel (dt.exe). Er is in feite een optie voor uitvoer van de bijbehorende opdracht na het instellen van een import via de gebruikersinterface. In tabelvorm brongegevens (bijvoorbeeld SQL Server- of CSV-bestanden) kan worden getransformeerd zodat hiërarchische relaties (subdocumenten) kunnen worden gemaakt tijdens het importeren. Houd lezen voor meer informatie over opties voor gegevensbron steekproef van opdrachtregels voor het importeren uit elke bron, doel-opties en importeren van de weer te geven resultaten.

## <a id="Install"></a>Installatie
De broncode van de migratie-hulpprogramma is beschikbaar op GitHub in [deze opslagplaats](https://github.com/azure/azure-documentdb-datamigrationtool) en een gecompileerde versie is beschikbaar vanuit [Microsoft Download Center](http://www.microsoft.com/downloads/details.aspx?FamilyID=cda7703a-2774-4c07-adcc-ad02ddc1a44d). U kunt compileren van de oplossing of gewoon downloaden en uitpakken van de gecompileerde versie naar een map van uw keuze. Voer vervolgens een:

* **Dtui.exe**: versie van de grafische interface van het hulpprogramma
* **DT.exe**: opdrachtregelprogramma versie van het hulpprogramma

## <a name="select-data-source"></a>Selecteer gegevensbron

Nadat u het hulpprogramma hebt geïnstalleerd, is het tijd om uw gegevens te importeren. Welk type gegevens wilt u gebruiken om te importeren?

* [JSON-bestanden](#JSON)
* [MongoDB](#MongoDB)
* [MongoDB exportbestanden](#MongoDBExport)
* [SQL Server](#SQL)
* [CSV-bestanden](#CSV)
* [Azure Table storage](#AzureTableSource)
* [Amazon DynamoDB](#DynamoDBSource)
* [BLOB](#BlobImport)
* [Azure DB Cosmos-verzamelingen](#SQLSource)
* [HBase](#HBaseSource)
* [Azure DB Cosmos bulkimport](#SQLBulkImport)
* [Azure DB Cosmos sequentiële record importeren](#DocumentDSeqTarget)


## <a id="JSON"></a>JSON-bestanden importeren
De optie voor de gegevensbron importfunctie van JSON-bestand kunt u wilt importeren een of meer JSON-bestanden voor één document of JSON-bestanden dat elk een matrix met JSON-documenten bevatten. Bij het toevoegen van mappen met JSON-bestanden te importeren, hebt u de mogelijkheid van recursief zoeken naar bestanden in submappen.

![Schermopname van JSON-bestand bronopties - hulpprogramma's voor migratie](./media/import-data/jsonsource.png)

Hier volgen enkele voorbeelden vanaf de opdrachtregel voor het importeren van JSON-bestanden:

    #Import a single JSON file
    dt.exe /s:JsonFile /s.Files:.\Sessions.json /t:CosmosDBBulk /t.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:Sessions /t.CollectionThroughput:2500

    #Import a directory of JSON files
    dt.exe /s:JsonFile /s.Files:C:\TESessions\*.json /t:CosmosDBBulk /t.ConnectionString:" AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:Sessions /t.CollectionThroughput:2500

    #Import a directory (including sub-directories) of JSON files
    dt.exe /s:JsonFile /s.Files:C:\LastFMMusic\**\*.json /t:CosmosDBBulk /t.ConnectionString:" AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:Music /t.CollectionThroughput:2500

    #Import a directory (single), directory (recursive), and individual JSON files
    dt.exe /s:JsonFile /s.Files:C:\Tweets\*.*;C:\LargeDocs\**\*.*;C:\TESessions\Session48172.json;C:\TESessions\Session48173.json;C:\TESessions\Session48174.json;C:\TESessions\Session48175.json;C:\TESessions\Session48177.json /t:CosmosDBBulk /t.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:subs /t.CollectionThroughput:2500

    #Import a single JSON file and partition the data across 4 collections
    dt.exe /s:JsonFile /s.Files:D:\\CompanyData\\Companies.json /t:CosmosDBBulk /t.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:comp[1-4] /t.PartitionKey:name /t.CollectionThroughput:2500

## <a id="MongoDB"></a>Importeren van MongoDB

> [!IMPORTANT]
> Als u naar een Cosmos-DB Azure-account met ondersteuning voor MongoDB importeren wilt, volgt u deze [instructies](mongodb-migrate.md).
> 
> 

De optie voor MongoDB importfunctie kunt u importeren uit een afzonderlijke MongoDB-verzameling en eventueel documenten met behulp van een query filteren en/of de documentstructuur wijzigen met behulp van een projectie.  

![Schermopname van MongoDB-opties voor gegevensbron](./media/import-data/mongodbsource.png)

De verbindingsreeks heeft de standaard MongoDB-indeling:

    mongodb://<dbuser>:<dbpassword>@<host>:<port>/<database>

> [!NOTE]
> Gebruik de opdracht controleren om ervoor te zorgen dat het MongoDB-exemplaar dat is opgegeven in het veld connection string kan worden geopend.
> 
> 

Voer de naam van de verzameling van waaruit de gegevens worden geïmporteerd. U kunt eventueel opgeven of een bestand voor een query opgeven (bijvoorbeeld {pop: {$gt: 5000}}) en/of een projectie (bijvoorbeeld {loc:0}) voor zowel filteren en vorm van de gegevens moeten worden geïmporteerd.

Hier volgen enkele voorbeelden vanaf de opdrachtregel om te importeren uit MongoDB:

    #Import all documents from a MongoDB collection
    dt.exe /s:MongoDB /s.ConnectionString:mongodb://<dbuser>:<dbpassword>@<host>:<port>/<database> /s.Collection:zips /t:CosmosDBBulk /t.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:BulkZips /t.IdField:_id /t.CollectionThroughput:2500

    #Import documents from a MongoDB collection which match the query and exclude the loc field
    dt.exe /s:MongoDB /s.ConnectionString:mongodb://<dbuser>:<dbpassword>@<host>:<port>/<database> /s.Collection:zips /s.Query:{pop:{$gt:50000}} /s.Projection:{loc:0} /t:CosmosDBBulk /t.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:BulkZipsTransform /t.IdField:_id/t.CollectionThroughput:2500

## <a id="MongoDBExport"></a>MongoDB-exportbestanden importeren

> [!IMPORTANT]
> Als u naar een Cosmos-DB Azure-account met ondersteuning voor MongoDB importeren wilt, volgt u deze [instructies](mongodb-migrate.md).
> 
> 

De MongoDB JSON-bestand bron importfunctie exportoptie kunt u voor het importeren van een of meer JSON-bestanden van het hulpprogramma mongoexport geproduceerd.  

![Schermopname van MongoDB-exportopties bron](./media/import-data/mongodbexportsource.png)

Als u mappen met MongoDB export JSON-bestanden voor het importeren van toevoegt, hebt u de mogelijkheid van recursief zoeken naar bestanden in submappen.

Hier volgt een voorbeeld van de opdrachtregel om te importeren uit JSON-bestanden van MongoDB-export:

    dt.exe /s:MongoDBExport /s.Files:D:\mongoemployees.json /t:CosmosDBBulk /t.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:employees /t.IdField:_id /t.Dates:Epoch /t.CollectionThroughput:2500

## <a id="SQL"></a>Importeren van SQL Server
De SQL-bron importfunctie optie kunt u om te importeren uit een afzonderlijke SQL Server-database en optioneel de records moeten worden geïmporteerd met behulp van een query filteren. U kunt bovendien de documentstructuur wijzigen door op te geven van een geneste scheidingsteken (meer informatie over die later).  

![Schermafbeelding van de SQL - opties voor hulpprogramma's voor migratie](./media/import-data/sqlexportsource.png)

De indeling van de verbindingsreeks is standaard SQL indeling van de verbindingsreeks.

> [!NOTE]
> Gebruik de opdracht controleren om ervoor te zorgen dat de SQL Server-exemplaar dat is opgegeven in het veld connection string kan worden geopend.
> 
> 

De geneste scheidingsteken-eigenschap gebruikt voor het maken van hiërarchische relaties (onderliggende documenten) tijdens het importeren. Houd rekening met de volgende SQL-query:

*CAST (BusinessEntityID AS varchar) als de Id, naam, adrestype als [Address.AddressType], AddressLine1 als [Address.AddressLine1], plaats als [Address.Location.City], StateProvinceName als [Address.Location.StateProvinceName], postcode als [selecteren Address.PostalCode] CountryRegionName als [Address.CountryRegionName] uit Sales.vStoreWithAddresses waar adrestype = 'Hoofdkantoor'*

Dit retourneert de volgende resultaten voor (gedeeltelijk):

![Schermafbeelding van de SQL-queryresultaten](./media/import-data/sqlqueryresults.png)

Noteer de aliassen zoals Address.AddressType en Address.Location.StateProvinceName. Door op te geven van een geneste scheidingsteken van '.', het hulpprogramma voor importeren adres en Address.Location subdocumenten maakt tijdens het importeren. Hier volgt een voorbeeld van een resulterende document in Azure Cosmos DB:

*{'id': '956', 'Name': 'Fijner verkoop en Service', 'Adres': {'Adrestype': 'Hoofdkantoor', 'AddressLine1': '#500 75 O'Connor straat', 'Locatie': {'Stad': 'Canada Ottawa', 'StateProvinceName': "Ontario"}, 'Postcode': 'K4B 1S2', 'CountryRegionName': ' Canada'}}*

Hier volgen enkele voorbeelden vanaf de opdrachtregel om te importeren uit SQL Server:

    #Import records from SQL which match a query
    dt.exe /s:SQL /s.ConnectionString:"Data Source=<server>;Initial Catalog=AdventureWorks;User Id=advworks;Password=<password>;" /s.Query:"select CAST(BusinessEntityID AS varchar) as Id, * from Sales.vStoreWithAddresses WHERE AddressType='Main Office'" /t:CosmosDBBulk /t.ConnectionString:" AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:Stores /t.IdField:Id /t.CollectionThroughput:2500

    #Import records from sql which match a query and create hierarchical relationships
    dt.exe /s:SQL /s.ConnectionString:"Data Source=<server>;Initial Catalog=AdventureWorks;User Id=advworks;Password=<password>;" /s.Query:"select CAST(BusinessEntityID AS varchar) as Id, Name, AddressType as [Address.AddressType], AddressLine1 as [Address.AddressLine1], City as [Address.Location.City], StateProvinceName as [Address.Location.StateProvinceName], PostalCode as [Address.PostalCode], CountryRegionName as [Address.CountryRegionName] from Sales.vStoreWithAddresses WHERE AddressType='Main Office'" /s.NestingSeparator:. /t:CosmosDBBulk /t.ConnectionString:" AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:StoresSub /t.IdField:Id /t.CollectionThroughput:2500

## <a id="CSV"></a>CSV-bestanden importeren en CSV niet converteren naar JSON
De optie voor de gegevensbron importfunctie van CSV-bestand kunt u een of meer CSV-bestanden te importeren. Bij het toevoegen van mappen met CSV-bestanden voor importeren, hebt u de mogelijkheid van recursief zoeken naar bestanden in submappen.

![Schermopname van CSV - opties voor CSV in JSON](media/import-data/csvsource.png)

Vergelijkbaar met de SQL-bron, het aantal geneste scheidingsteken-eigenschap kan hiërarchische relaties (onderliggende documenten) maken tijdens het importeren worden gebruikt. Houd rekening met de volgende header voor de CSV-rij en gegevensrijen:

![Schermopname van CSV-voorbeeld registreert - CSV in JSON](./media/import-data/csvsample.png)

Noteer de aliassen zoals DomainInfo.Domain_Name en RedirectInfo.Redirecting. Door op te geven van een geneste scheidingsteken van '.', maakt het hulpprogramma voor importeren DomainInfo en RedirectInfo subdocumenten tijdens het importeren. Hier volgt een voorbeeld van een resulterende document in Azure Cosmos DB:

*{"DomainInfo": {'Domeinnaam': 'ACUS.GOV', 'Domain_Name_Address': 'http://www.ACUS.GOV'}, 'Federale instantie': ' beheerdersrechten Conferentie van de Verenigde Staten ', 'RedirectInfo': {'Omleiden': '0', 'Redirect_Destination': ' "}, 'id': '9cc565c5-ebcd-1c03-ebd3-cc3e2ecd814d'}*

Het hulpprogramma voor importeren probeert het afleiden van type-informatie voor zonder aanhalingstekens waarden in CSV-bestanden (tussen aanhalingstekens waarden worden altijd behandeld als tekenreeksen).  Typen worden geïdentificeerd in de volgende volgorde: getal, datum/tijd, Booleaanse waarde.  

Er zijn twee dingen te weten over de CSV-import:

1. Standaard zonder aanhalingstekens waarden zijn altijd bijgesneden voor tabs en spaties, terwijl tussen aanhalingstekens waarden behouden als blijven-is. Dit gedrag kan worden genegeerd met het selectievakje Trim tussen aanhalingstekens waarden of de /s.TrimQuoted-opdrachtregeloptie.
2. Standaard wordt een zonder aanhalingstekens null worden beschouwd als een null-waarde. Dit gedrag kan worden genegeerd (dat wil zeggen een zonder aanhalingstekens null behandeld als een tekenreeks 'null') met het behandelen zonder aanhalingstekens NULL als tekenreeks selectievakje of de /s.NoUnquotedNulls-opdrachtregeloptie.

Hier volgt een voorbeeld van een opdrachtregelprogramma voor CSV-import:

    dt.exe /s:CsvFile /s.Files:.\Employees.csv /t:CosmosDBBulk /t.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:Employees /t.IdField:EntityID /t.CollectionThroughput:2500

## <a id="AzureTableSource"></a>Importeren uit Azure Table storage
De optie voor de gegevensbron importfunctie van Azure Table storage kunt u om te importeren uit een afzonderlijke tabel voor Azure Table-opslag. Eventueel kunt u filteren in de tabelentiteiten worden geïmporteerd. 

Gegevens die zijn geïmporteerd uit Azure Table Storage kunnen worden uitgevoerd op Azure DB die Cosmos-tabellen en entiteiten, voor gebruik met de API van de tabel, of naar verzamelingen en documenten, voor gebruik met de SQL-API. Echter; Tabel-API is alleen beschikbaar als een doel in het opdrachtregelprogramma, u niet exporteren naar tabel API met behulp van de gebruikersinterface van het hulpprogramma voor gegevensmigratie. Zie voor meer informatie [importeren van gegevens voor gebruik met de Azure-API voor tabel Cosmos DB](table-import.md). 

![Schermopname van Azure Table storage bronopties](./media/import-data/azuretablesource.png)

De indeling van de verbindingsreeks voor Azure Table storage is:

    DefaultEndpointsProtocol=<protocol>;AccountName=<Account Name>;AccountKey=<Account Key>;

> [!NOTE]
> Gebruik de opdracht controleren om ervoor te zorgen dat het opgegeven in het veld connection string Azure Table storage-exemplaar kan worden geopend.
> 
> 

Voer de naam van de Azure-tabel van waaruit u wilt importeren. U kunt eventueel opgeven een [filter](https://msdn.microsoft.com/library/azure/ff683669.aspx).

De optie Azure Table storage importfunctie heeft de volgende aanvullende opties:

1. Interne velden bevatten
   1. All - omvatten alle interne velden (PartitionKey, RowKey en tijdstempel)
   2. Geen - uitsluiten alle interne velden
   3. RowKey - alleen bestaan uit het veld RowKey
2. Kolommen selecteren
   1. Azure Table storage filters bieden geen ondersteuning voor projecties. Als u importeren alleen specifieke Azure Table-entiteitseigenschappen wilt, deze toevoegen aan de lijst met kolommen selecteren. Alle eigenschappen van andere entiteiten worden genegeerd.

Hier volgt een voorbeeld van de opdrachtregel om te importeren uit Azure Table storage:

    dt.exe /s:AzureTable /s.ConnectionString:"DefaultEndpointsProtocol=https;AccountName=<Account Name>;AccountKey=<Account Key>" /s.Table:metrics /s.InternalFields:All /s.Filter:"PartitionKey eq 'Partition1' and RowKey gt '00001'" /s.Projection:ObjectCount;ObjectSize  /t:CosmosDBBulk /t.ConnectionString:" AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:metrics /t.CollectionThroughput:2500

## <a id="DynamoDBSource"></a>Importeren uit Amazon DynamoDB
De Amazon DynamoDB bron importfunctie optie kunt u om te importeren uit een tabel met afzonderlijke Amazon DynamoDB en filter optioneel de entiteiten worden geïmporteerd. Verschillende sjablonen zijn bedoeld om het instellen van een import zo eenvoudig mogelijk is.

![Schermopname van Amazon DynamoDB bronopties - hulpprogramma's voor migratie](./media/import-data/dynamodbsource1.png)

![Schermopname van Amazon DynamoDB bronopties - hulpprogramma's voor migratie](./media/import-data/dynamodbsource2.png)

De indeling van de verbindingsreeks Amazon DynamoDB is:

    ServiceURL=<Service Address>;AccessKey=<Access Key>;SecretKey=<Secret Key>;

> [!NOTE]
> Gebruik de opdracht controleren om ervoor te zorgen dat het opgegeven in het veld connection string Amazon DynamoDB exemplaar kan worden geopend.
> 
> 

Hier volgt een voorbeeld van de opdrachtregel om te importeren uit Amazon DynamoDB:

    dt.exe /s:DynamoDB /s.ConnectionString:ServiceURL=https://dynamodb.us-east-1.amazonaws.com;AccessKey=<accessKey>;SecretKey=<secretKey> /s.Request:"{   """TableName""": """ProductCatalog""" }" /t:DocumentDBBulk /t.ConnectionString:"AccountEndpoint=<Azure Cosmos DB Endpoint>;AccountKey=<Azure Cosmos DB Key>;Database=<Azure Cosmos DB Database>;" /t.Collection:catalogCollection /t.CollectionThroughput:2500

## <a id="BlobImport"></a>Importeren uit Azure Blob-opslag
De JSON-bestand, MongoDB-exportbestand en CSV-bestand bron importfunctie opties kunnen u een of meer bestanden importeren uit Azure Blob-opslag. Nadat u een URL van de Blob-container en de Accountsleutel, bieden een reguliere expressie om te selecteren van de bestanden te importeren.

![Schermopname van Blob-bestand bronopties](./media/import-data/blobsource.png)

Dit is een opdrachtregelprogramma voorbeeld JSON-bestanden importeren uit Azure Blob-opslag:

    dt.exe /s:JsonFile /s.Files:"blobs://<account key>@account.blob.core.windows.net:443/importcontainer/.*" /t:CosmosDBBulk /t.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:doctest

## <a id="SQLSource"></a>Importeren uit een verzameling SQL-API
De optie Azure Cosmos DB importfunctie kunt u gegevens importeren uit een of meer Azure Cosmos DB verzamelingen en filter optioneel documenten met behulp van een query.  

![Opties voor schermopname van Azure Cosmos DB-gegevensbron](./media/import-data/documentdbsource.png)

De indeling van de Azure DB die Cosmos-verbindingsreeks is:

    AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;

De verbindingsreeks voor Azure DB die Cosmos-account kan worden opgehaald uit de pagina sleutels van de Azure portal, zoals beschreven in [het beheren van een account voor Azure Cosmos DB](manage-account.md), maar de naam van de database moet worden toegevoegd aan de verbindingsreeks in de volgende indeling:

    Database=<CosmosDB Database>;

> [!NOTE]
> Gebruik de opdracht controleren om ervoor te zorgen dat het Azure DB die Cosmos-exemplaar dat is opgegeven in het veld connection string kan worden geopend.
> 
> 

Als u wilt importeren uit een verzameling met één Azure Cosmos DB, voer de naam van de verzameling voor het importeren van gegevens uit. Om te importeren uit meerdere Azure Cosmos DB verzamelingen, bieden een reguliere expressie zodat deze overeenkomt met een of meer verzamelingsnamen (bijvoorbeeld collection01 | collection02 | collection03). U kunt eventueel, of geef een bestand voor een query op filter en de vorm van de gegevens moeten worden geïmporteerd.

> [!NOTE]
> Omdat het veld voor de verzameling reguliere expressies, accepteert als u wilt importeren uit één verzameling waarvan de naam tekens van de reguliere expressie bevat, moeten vervolgens die tekens worden voorafgegaan dienovereenkomstig.
> 
> 

De optie Azure Cosmos DB importfunctie heeft de volgende geavanceerde opties:

1. Interne velden bevatten: Hiermee geeft u wel of niet Azure Cosmos DB document Systeemeigenschappen opnemen in de uitvoer (bijvoorbeeld _rid, _ts).
2. Aantal nieuwe pogingen bij fouten: Hiermee geeft u het aantal keren opnieuw de verbinding met Azure Cosmos DB in geval van een tijdelijke fouten (bijvoorbeeld connectiviteit onderbreking op het netwerk).
3. Interval voor opnieuw proberen: Geeft aan hoe lang moet worden gewacht tussen opnieuw verbinding met Azure Cosmos-database in geval van een tijdelijke fouten (bijvoorbeeld connectiviteit onderbreking op het netwerk).
4. Verbindingsmodus: Hiermee geeft u de verbindingsmodus gebruiken met Azure Cosmos DB. De beschikbare opties zijn DirectTcp, DirectHttps en Gateway. De modi rechtstreekse verbinding zijn sneller, terwijl de gateway-modus meer firewall beschrijvende is aangezien hierbij alleen poort 443.

![Schermopname van Azure Cosmos DB bron geavanceerde opties](./media/import-data/documentdbsourceoptions.png)

> [!TIP]
> Het hulpprogramma voor importeren standaard verbindingsmodus DirectTcp. Als u de firewall-problemen ondervindt, overschakelen naar verbindingsmodus Gateway, omdat hiervoor alleen poort 443.
> 
> 

Hier volgen enkele opdrachtregel voorbeelden om te importeren uit Azure Cosmos DB:

    #Migrate data from one Azure Cosmos DB collection to another Azure Cosmos DB collections
    dt.exe /s:CosmosDB /s.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /s.Collection:TEColl /t:CosmosDBBulk /t.ConnectionString:" AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:TESessions /t.CollectionThroughput:2500

    #Migrate data from multiple Azure Cosmos DB collections to a single Azure Cosmos DB collection
    dt.exe /s:CosmosDB /s.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /s.Collection:comp1|comp2|comp3|comp4 /t:CosmosDBBulk /t.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:singleCollection /t.CollectionThroughput:2500

    #Export an Azure Cosmos DB collection to a JSON file
    dt.exe /s:CosmosDB /s.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /s.Collection:StoresSub /t:JsonFile /t.File:StoresExport.json /t.Overwrite /t.CollectionThroughput:2500

> [!TIP]
> Het Azure Cosmos DB Data Import-hulpprogramma ondersteunt ook het importeren van gegevens uit de [Azure Cosmos DB Emulator](local-emulator.md). Bij het importeren van een lokale emulator, het eindpunt instellen op `https://localhost:<port>`. 
> 
> 

## <a id="HBaseSource"></a>Importeren van HBase
De optie voor HBase importfunctie kunt u gegevens importeren uit een HBase-tabel en optioneel filter de gegevens. Verschillende sjablonen zijn bedoeld om het instellen van een import zo eenvoudig mogelijk is.

![Schermopname van HBase-opties voor gegevensbron](./media/import-data/hbasesource1.png)

![Schermopname van HBase-opties voor gegevensbron](./media/import-data/hbasesource2.png)

De indeling van de verbindingsreeks HBase Stargate is:

    ServiceURL=<server-address>;Username=<username>;Password=<password>

> [!NOTE]
> Gebruik de opdracht controleren om ervoor te zorgen dat het HBase-exemplaar dat is opgegeven in het veld connection string kan worden geopend.
> 
> 

Hier volgt een voorbeeld van de opdrachtregel om te importeren uit HBase:

    dt.exe /s:HBase /s.ConnectionString:ServiceURL=<server-address>;Username=<username>;Password=<password> /s.Table:Contacts /t:CosmosDBBulk /t.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:hbaseimport

## <a id="SQLBulkTarget"></a>Importeren in de SQL-API (bulkimport)
De importfunctie Azure Cosmos DB bulksgewijs kunt u om te importeren uit een van de bronopties die beschikbaar, met behulp van een Azure Cosmos DB opgeslagen procedure voor efficiëntie. Het hulpprogramma ondersteunt importeren naar één enkel gepartitioneerd Azure Cosmos DB verzameling, evenals shard importeren, waarbij de gegevens in meerdere één gepartitioneerd Azure Cosmos DB verzamelingen zijn gepartitioneerd. Zie voor meer informatie over het partitioneren van gegevens, [partitionering en schalen in Azure Cosmos DB](partition-data.md). Het hulpprogramma maakt, wordt uitgevoerd en wordt de opgeslagen procedure verwijderd uit de verzameling(en) doel.  

![Schermopname van Azure Cosmos DB bulksgewijs opties](./media/import-data/documentdbbulk.png)

De indeling van de Azure DB die Cosmos-verbindingsreeks is:

    AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;

De verbindingsreeks voor Azure DB die Cosmos-account kan worden opgehaald uit de pagina sleutels van de Azure portal, zoals beschreven in [het beheren van een account voor Azure Cosmos DB](manage-account.md), maar de naam van de database moet worden toegevoegd aan de verbindingsreeks in de volgende indeling:

    Database=<CosmosDB Database>;

> [!NOTE]
> Gebruik de opdracht controleren om ervoor te zorgen dat het Azure DB die Cosmos-exemplaar dat is opgegeven in het veld connection string kan worden geopend.
> 
> 

Als u wilt importeren naar één verzameling, voer de naam van de verzameling gegevens importeren uit en klik op de knop toevoegen. Als u wilt importeren tot meerdere verzamelingen, voer de verzamelingsnaam van elke afzonderlijk of gebruik de volgende syntaxis om op te geven van meerdere verzamelingen: *collection_prefix*[begin index - end-index]. Houd rekening met de volgende richtlijnen bij het opgeven van meerdere verzamelingen via de hiervoor genoemde syntaxis:

1. Alleen geheel getal bereik bestandsnaampatronen worden ondersteund. Het opgeven van de verzameling [0-3] wordt bijvoorbeeld de volgende verzamelingen: collection0, collection1, collection2, collection3.
2. U kunt een verkorte syntaxis gebruiken: [3] verzameling maakt dezelfde set van verzamelingen die zijn vermeld in stap 1.
3. Meer dan een vervanging kan worden opgegeven. Verzameling [0-1] [0-9] genereert bijvoorbeeld 20 verzamelingsnamen met toonaangevende nullen (collection01,... 02... 03).

Wanneer de verzameling namen zijn opgegeven, kiest u de gewenste doorvoer van de verzameling(en) (400 RUs naar 10.000 RUs). Kies een hogere doorvoer voor de beste prestaties importeren. Zie voor meer informatie over prestatieniveaus [prestatieniveaus in Azure Cosmos DB](performance-levels.md).

> [!NOTE]
> De instelling van de doorvoer prestaties is alleen van toepassing op het maken van de verzameling. Als de opgegeven verzameling al bestaat, wordt de doorvoer niet worden gewijzigd.
> 
> 

Bij het importeren van meerdere verzamelingen, ondersteunt het hulpprogramma voor importeren op basis van het hash-sharding. Geef in dit scenario wordt de documenteigenschap dat u wilt gebruiken als de partitiesleutel (als partitiesleutel leeg is, documenten zijn shard willekeurig in de doel-verzamelingen).

U kunt eventueel opgeven welk veld in de bron voor het importeren moet worden gebruikt als de eigenschap id van Azure DB die Cosmos-document tijdens het importeren (Let erop dat als deze eigenschap documenten niet bevatten, klikt u vervolgens het hulpprogramma voor importeren een GUID als de waarde van de id-eigenschap genereert).

Er zijn tijdens het importeren van een aantal geavanceerde opties beschikbaar. Terwijl het hulpprogramma omvat eerst een standaard bulkimport opgeslagen procedure (BulkInsert.js), kunt u uw eigen importeren opgeslagen procedure opgeven:

 ![Schermopname van Azure Cosmos DB bulksgewijs invoegen sproc optie](./media/import-data/bulkinsertsp.png)

Bij het importeren van datum-typen (bijvoorbeeld van SQL Server of MongoDB), kunt u kunt daarnaast voor kiezen tussen de drie opties voor importeren:

 ![Schermopname van Azure Cosmos DB datum tijdopties importeren](./media/import-data/datetimeoptions.png)

* Tekenreeks: Behouden als een string-waarde
* Epoche: Behouden als een getalwaarde epoche
* Beide: Zowel tekenreeks als numerieke waarden epoche bewaard. Deze optie maakt u een subdocument, bijvoorbeeld: "date_joined": {'' waarde '': ' 2013-10-21T21:17:25.2410000Z ', ' epoche ': 1382390245}

De Azure Cosmos DB Bulk-importprogramma heeft de volgende aanvullende geavanceerde opties:

1. Batchgrootte: Het hulpprogramma wordt standaard ingesteld op een batchgrootte van 50.  Als de documenten die moeten worden geïmporteerd groot zijn, kunt u overwegen de batchgrootte te verlagen. Als u daarentegen als de documenten die moeten worden geïmporteerd klein zijn, kunt u de batchgrootte verhogen.
2. Maximum aantal Script-grootte (bytes): het hulpprogramma wordt standaard ingesteld op een script maximale grootte van 512 KB.
3. Schakel automatische van generatie-Id: Als elk document dat moet worden geïmporteerd, een id-veld bevat, deze optie kan de prestaties verbeteren. Een unieke id-veld ontbreekt documenten zijn niet geïmporteerd.
4. Update bestaande documenten: Het hulpprogramma wordt standaard ingesteld op niet vervangen door een bestaand document-id's. Als u deze optie selecteert, kunt bestaande documenten met overeenkomende id overschrijven. Deze functie is handig voor geplande gegevens migraties die bijwerken van bestaande documenten.
5. Aantal nieuwe pogingen bij fouten: Hiermee geeft u het aantal keren opnieuw de verbinding met Azure Cosmos DB in geval van een tijdelijke fouten (bijvoorbeeld connectiviteit onderbreking op het netwerk).
6. Interval voor opnieuw proberen: Geeft aan hoe lang moet worden gewacht tussen opnieuw verbinding met Azure Cosmos-database in geval van een tijdelijke fouten (bijvoorbeeld connectiviteit onderbreking op het netwerk).
7. Verbindingsmodus: Hiermee geeft u de verbindingsmodus gebruiken met Azure Cosmos DB. De beschikbare opties zijn DirectTcp, DirectHttps en Gateway. De modi rechtstreekse verbinding zijn sneller, terwijl de gateway-modus meer firewall beschrijvende is aangezien hierbij alleen poort 443.

![Schermopname van Azure Cosmos DB bulkimport geavanceerde opties](./media/import-data/docdbbulkoptions.png)

> [!TIP]
> Het hulpprogramma voor importeren standaard verbindingsmodus DirectTcp. Als u de firewall-problemen ondervindt, overschakelen naar verbindingsmodus Gateway, omdat hiervoor alleen poort 443.
> 
> 

## <a id="SQLSeqTarget"></a>Importeren in de SQL-API (sequentiële Record importeren)
De importfunctie van Azure DB die Cosmos sequentiële record kunt u om te importeren uit een van de beschikbare opties op basis van door een andere record. U kunt deze optie selecteren als u wilt importeren naar een bestaande collectie heeft het quotum van opgeslagen procedures bereikt. Het hulpprogramma ondersteunt importeren naar een verzameling van de Azure Cosmos DB één (één partitie en meerdere partitie), evenals shard importeren, waarbij de gegevens in Azure Cosmos DB meerdere verzamelingen voor één partitie en/of meerdere partitie zijn gepartitioneerd. Zie voor meer informatie over het partitioneren van gegevens, [partitionering en schalen in Azure Cosmos DB](partition-data.md).

![Schermopname van Azure Cosmos DB-opties voor opeenvolgende record importeren](./media/import-data/documentdbsequential.png)

De indeling van de Azure DB die Cosmos-verbindingsreeks is:

    AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;

De verbindingsreeks voor Azure DB die Cosmos-account kan worden opgehaald uit de pagina sleutels van de Azure portal, zoals beschreven in [het beheren van een account voor Azure Cosmos DB](manage-account.md), maar de naam van de database moet worden toegevoegd aan de verbindingsreeks in de volgende indeling:

    Database=<Azure Cosmos DB Database>;

> [!NOTE]
> Gebruik de opdracht controleren om ervoor te zorgen dat het Azure DB die Cosmos-exemplaar dat is opgegeven in het veld connection string kan worden geopend.
> 
> 

Als u wilt importeren naar één verzameling, voer de naam van de verzameling waarop gegevens worden geïmporteerd en klik op de knop toevoegen. Als u wilt importeren tot meerdere verzamelingen, voer de verzamelingsnaam van elke afzonderlijk of gebruik de volgende syntaxis om op te geven van meerdere verzamelingen: *collection_prefix*[begin index - end-index]. Houd rekening met de volgende richtlijnen bij het opgeven van meerdere verzamelingen via de hiervoor genoemde syntaxis:

1. Alleen geheel getal bereik bestandsnaampatronen worden ondersteund. Het opgeven van de verzameling [0-3] wordt bijvoorbeeld de volgende verzamelingen: collection0, collection1, collection2, collection3.
2. U kunt een verkorte syntaxis gebruiken: [3] verzameling maakt dezelfde set van verzamelingen die zijn vermeld in stap 1.
3. Meer dan een vervanging kan worden opgegeven. Verzameling [0-1] [0-9] maakt bijvoorbeeld 20 verzamelingsnamen met toonaangevende nullen (collection01,... 02... 03).

Wanneer de verzameling namen zijn opgegeven, kiest u de gewenste doorvoer van de verzameling(en) (400 RUs naar 250.000 RUs). Kies een hogere doorvoer voor de beste prestaties importeren. Zie voor meer informatie over prestatieniveaus [prestatieniveaus in Azure Cosmos DB](performance-levels.md). Invoer voor verzamelingen met doorvoer > 10.000 RUs vereisen een partitiesleutel. Als u ervoor kiest om meer dan 250.000 RUs, moet u het bestand is een aanvraag in de portal voor uw account wordt verhoogd.

> [!NOTE]
> De doorvoer-instelling is alleen van toepassing op het maken van de verzameling. Als de opgegeven verzameling al bestaat, wordt de doorvoer niet gewijzigd.
> 
> 

Bij het importeren van meerdere verzamelingen, ondersteunt het hulpprogramma voor importeren op basis van het hash-sharding. Geef in dit scenario wordt de documenteigenschap dat u wilt gebruiken als de partitiesleutel (als partitiesleutel leeg is, documenten zijn shard willekeurig in de doel-verzamelingen).

U kunt eventueel opgeven welk veld in de bron voor het importeren moet worden gebruikt als de eigenschap id van Azure DB die Cosmos-document tijdens het importeren (Let erop dat als deze eigenschap documenten niet bevatten, klikt u vervolgens het hulpprogramma voor importeren een GUID als de waarde van de id-eigenschap genereert).

Er zijn tijdens het importeren van een aantal geavanceerde opties beschikbaar. Eerst bij het importeren van datum-typen (bijvoorbeeld van SQL Server of MongoDB), kunt u tussen de drie opties voor importeren:

 ![Schermopname van Azure Cosmos DB datum tijdopties importeren](./media/import-data/datetimeoptions.png)

* Tekenreeks: Behouden als een string-waarde
* Epoche: Behouden als een getalwaarde epoche
* Beide: Zowel tekenreeks als numerieke waarden epoche bewaard. Deze optie maakt u een subdocument, bijvoorbeeld: "date_joined": {'' waarde '': ' 2013-10-21T21:17:25.2410000Z ', ' epoche ': 1382390245}

De Azure Cosmos DB - sequentiële record importfunctie heeft de volgende aanvullende geavanceerde opties:

1. Aantal parallelle aanvragen: het hulpprogramma wordt standaard ingesteld op twee parallelle aanvragen. Als de documenten die moeten worden geïmporteerd klein zijn, kunt u overwegen het verhogen van het aantal parallelle aanvragen. Houd er rekening mee dat als dit nummer te veel wordt gegenereerd, de import beperking ondervinden.
2. Schakel automatische van generatie-Id: Als elk document dat moet worden geïmporteerd, een id-veld bevat, deze optie kan de prestaties verbeteren. Een unieke id-veld ontbreekt documenten zijn niet geïmporteerd.
3. Update bestaande documenten: Het hulpprogramma wordt standaard ingesteld op niet vervangen door een bestaand document-id's. Als u deze optie selecteert, kunt bestaande documenten met overeenkomende id overschrijven. Deze functie is handig voor geplande gegevens migraties die bijwerken van bestaande documenten.
4. Aantal nieuwe pogingen bij fouten: Hiermee geeft u het aantal keren opnieuw de verbinding met Azure Cosmos DB in geval van een tijdelijke fouten (bijvoorbeeld connectiviteit onderbreking op het netwerk).
5. Interval voor opnieuw proberen: Geeft aan hoe lang moet worden gewacht tussen opnieuw verbinding met Azure Cosmos-database in geval van een tijdelijke fouten (bijvoorbeeld connectiviteit onderbreking op het netwerk).
6. Verbindingsmodus: Hiermee geeft u de verbindingsmodus gebruiken met Azure Cosmos DB. De beschikbare opties zijn DirectTcp, DirectHttps en Gateway. De modi rechtstreekse verbinding zijn sneller, terwijl de gateway-modus meer firewall beschrijvende is aangezien hierbij alleen poort 443.

![Schermopname van Azure Cosmos DB sequentiële record importeren geavanceerde opties](./media/import-data/documentdbsequentialoptions.png)

> [!TIP]
> Het hulpprogramma voor importeren standaard verbindingsmodus DirectTcp. Als u de firewall-problemen ondervindt, overschakelen naar verbindingsmodus Gateway, omdat hiervoor alleen poort 443.
> 
> 

## <a id="IndexingPolicy"></a>Geef een indexeringsbeleid
Wanneer u het hulpprogramma voor migratie van Azure Cosmos DB SQL-API om verzamelingen te maken tijdens het importeren van toestaat, kunt u het indexeringsbeleid van de verzamelingen. In de sectie Geavanceerde opties van de Azure Cosmos DB bulkimport en Azure Cosmos DB sequentiële record opties, navigeer naar de sectie beleid voor indexering.

![Schermopname van Azure Cosmos DB indexeren beleid geavanceerde opties](./media/import-data/indexingpolicy1.png)

Het indexeren beleid geavanceerde optie gebruikt, kunt u selecteren een bestand met beleidsregel van indexering, handmatig een indexeringsbeleid invoeren, of uit een reeks standaardsjablonen (door met de rechtermuisknop in het tekstvak voor indexering beleid).

De beleidssjablonen van die het hulpprogramma geeft zijn:

* De standaardinstelling. Dit beleid wordt aanbevolen wanneer u het uitvoeren van de gelijkheid query uitgevoerd naar tekenreeksen en ORDER BY, bereik en gelijkheid query's gebruiken voor getallen. Dit beleid heeft een lagere opslagoverhead voor indexering dan bereik.
* Het bereik. Dit beleid wordt aanbevolen u ORDER BY, bereik en gelijkheid query's op de cijfers en tekenreeksen. Dit beleid heeft een hogere opslagoverhead voor indexering dan standaard of hash-bewerking.

![Schermopname van Azure Cosmos DB indexeren beleid geavanceerde opties](./media/import-data/indexingpolicy2.png)

> [!NOTE]
> Als u een indexeringsbeleid niet opgeeft, wordt het standaardbeleid toegepast. Zie voor meer informatie over indexering beleid [Azure Cosmos DB indexeren beleid](indexing-policies.md).
> 
> 

## <a name="export-to-json-file"></a>Exporteren naar JSON-bestand
De exportfunctie Azure Cosmos DB JSON kunt u een van de beschikbare opties voor exporteren naar een JSON-bestand met een matrix met JSON-documenten. Het hulpprogramma omgaat met de uitvoer voor u of u kunt kiezen om de resulterende migratie-opdracht weer en voer de opdracht zelf. Het resulterende JSON-bestand kan worden opgeslagen, lokaal of in Azure Blob-opslag.

![Schermopname van Azure Cosmos DB JSON-optie voor het exporteren van lokaal bestand](./media/import-data/jsontarget.png)

![Schermopname van Azure Cosmos DB JSON Azure Blob storage exportoptie](./media/import-data/jsontarget2.png)

U kunt eventueel de resulterende JSON die de grootte van het resulterende document vergroot tijdens het maken van de inhoud meer prettify leesbaar.

    Standard JSON export
    [{"id":"Sample","Title":"About Paris","Language":{"Name":"English"},"Author":{"Name":"Don","Location":{"City":"Paris","Country":"France"}},"Content":"Don's document in Azure Cosmos DB is a valid JSON document as defined by the JSON spec.","PageViews":10000,"Topics":[{"Title":"History of Paris"},{"Title":"Places to see in Paris"}]}]

    Prettified JSON export
    [
     {
    "id": "Sample",
    "Title": "About Paris",
    "Language": {
      "Name": "English"
    },
    "Author": {
      "Name": "Don",
      "Location": {
        "City": "Paris",
        "Country": "France"
      }
    },
    "Content": "Don's document in Azure Cosmos DB is a valid JSON document as defined by the JSON spec.",
    "PageViews": 10000,
    "Topics": [
      {
        "Title": "History of Paris"
      },
      {
        "Title": "Places to see in Paris"
      }
    ]
    }]

## <a name="advanced-configuration"></a>Geavanceerde configuratie
Geef de locatie van het logboekbestand waarnaar u eventuele fouten die zijn geschreven dat wilt in het scherm Geavanceerde configuratie. De volgende regels zijn van toepassing op deze pagina:

1. Als een bestandsnaam niet opgegeven is, worden op de pagina met alle fouten geretourneerd.
2. Als een bestandsnaam is opgegeven zonder een map, klikt u vervolgens het bestand is gemaakt (of overschreven) in de huidige map in de omgeving.
3. Als u een bestaand bestand en vervolgens het bestand wordt overschreven, er is geen optie toevoegen.

Kies of u aan te melden, kritiek, of er geen foutberichten. Ten slotte bepalen hoe vaak de op het scherm overdracht bericht is bijgewerkt met de voortgang ervan.

    ![Screenshot of Advanced configuration screen](./media/import-data/AdvancedConfiguration.png)

## <a name="confirm-import-settings-and-view-command-line"></a>Bevestig de instellingen importeren en weergeven vanaf de opdrachtregel
1. Na het opgeven van informatie van gegevensbron, doelinformatie en geavanceerde configuratie controleren van de migratie samenvatting en, desgewenst weergeven/kopiëren de resulterende migratie-opdracht (kopiëren van de opdracht is nuttig voor het automatiseren van importbewerkingen):
   
    ![Schermopname van het scherm Samenvatting](./media/import-data/summary.png)
   
    ![Schermopname van het scherm Samenvatting](./media/import-data/summarycommand.png)
2. Wanneer u tevreden met de bron en doel-opties bent, klikt u op **importeren**. De verstreken tijd, aantal verzonden en foutgegevens (als u een bestandsnaam in de geavanceerde configuratie niet opgeven) bijwerken op het importeren wordt verwerkt. Als u klaar is, kunt u de resultaten (bijvoorbeeld om te gaan met eventuele fouten importeren) exporteren.
   
    ![Schermopname van Azure Cosmos DB JSON exportoptie](./media/import-data/viewresults.png)
3. U kunt ook een nieuwe import starten te behouden de bestaande instellingen (bijvoorbeeld verbinding tekenreeks informatie, de bron en doel keuze, etc.) of opnieuw instellen van alle waarden.
   
    ![Schermopname van Azure Cosmos DB JSON exportoptie](./media/import-data/newimport.png)

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie maakt hebt u de volgende taken uitgevoerd:

> [!div class="checklist"]
> * Het hulpprogramma voor gegevensmigratie is geïnstalleerd
> * Geïmporteerde gegevens uit verschillende gegevensbronnen
> * Uit Azure Cosmos DB geëxporteerd naar JSON

U kunt nu doorgaan met de volgende zelfstudie en informatie over het opvragen van gegevens met behulp van Azure Cosmos DB. 

> [!div class="nextstepaction"]
>[Hoe kan ik een query over gegevens?](../cosmos-db/tutorial-query-sql-api.md)
