---
title: Zelfstudie voor het indexeren van Azure SQL-databases in Azure Search | Microsoft Docs
description: Een Azure SQL-database verkennen om doorzoekbare gegevens op te halen en een Azure Search-index te vullen.
services: search
documentationcenter: 
author: HeidiSteen
manager: jhubbard
editor: 
tags: 
ms.assetid: 
ms.service: search
ms.devlang: na
ms.workload: search
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.date: 11/10/2017
ms.author: heidist
ms.openlocfilehash: bebfdfdf72014019a49a6da0e512e72932b096ac
ms.sourcegitcommit: 176c575aea7602682afd6214880aad0be6167c52
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/09/2018
---
# <a name="how-to-crawl-an-azure-sql-database-using-azure-search-indexers"></a>Een Azure SQL-database verkennen met de indexeerfuncties van Azure Search

In deze zelfstudie ziet u hoe u een indexeerfunctie configureert die doorzoekbare gegevens uit een Azure SQL-voorbeelddatabase ophaalt. [Indexeerfuncties](search-indexer-overview.md) zijn onderdelen van Azure Search die externe gegevensbronnen verkennen en een [zoekindex](search-what-is-an-index.md) vullen met inhoud. Van alle indexeerfuncties is de indexeerfunctie voor Azure SQL Database de meest gebruikte. 

Een goede vaardigheid in het configureren van indexeerfuncties is nuttig omdat dit de hoeveelheid code die u moet schrijven en onderhouden vereenvoudigt. In plaats van een schemacompatibele JSON-gegevensset voor te bereiden en te pushen, kunt u een indexeerfunctie koppelen aan een gegevensbron en de indexeerfunctie gegevens laten ophalen en in een index plaatsen. U kunt de indexeerfunctie eventueel uitvoeren volgens een terugkerend schema om zo wijzigingen in de onderliggende gegevensbron op te halen.

In deze zelfstudie gebruikt u de [clientbibliotheken van Azure Search .NET](https://aka.ms/search-sdk) en een .NET Core-consoletoepassing om de volgende taken uit te voeren:

> [!div class="checklist"]
> * De oplossing downloaden en configureren
> * Informatie over de zoekservice toevoegen aan toepassingsinstellingen
> * Een externe gegevensset voorbereiden in Azure SQL Database 
> * De definities van de index en indexeerfunctie in de voorbeeldcode controleren
> * De code van de indexeerfunctie uitvoeren om gegevens te importeren
> * Zoeken in de index
> * De configuratie van de indexeerfunctie in de portal bekijken

## <a name="prerequisites"></a>Vereisten

* Een actief Azure-account. Als u nog geen abonnement hebt, kunt u zich aanmelden voor een [gratis proefabonnement](https://azure.microsoft.com/free/). 

* Een Azure Search-service. Zie [Een zoekservice maken](search-create-service-portal.md) voor meer informatie over het instellen hiervan.

* Een Azure SQL-database met de externe gegevensbron die wordt gebruikt door een indexeerfunctie. De voorbeeldoplossing biedt een SQL-gegevensbestand om de tabel te maken.

* Visual Studio 2017. U kunt de gratis [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/) gebruiken. 

> [!Note]
> Als u de gratis Azure Search-service gebruikt, bent u beperkt tot drie indexen, drie indexeerfuncties en drie gegevensbronnen. In deze zelfstudie wordt één exemplaar van elk onderdeel gemaakt. Zorg ervoor dat uw service voldoende ruimte heeft voor de nieuwe resources.

## <a name="download-the-solution"></a>De oplossing downloaden

De indexeeroplossing in deze zelfstudie is afkomstig uit een verzameling Azure Search-voorbeelden die in één grote download wordt aangeboden. De oplossing die voor deze zelfstudie wordt gebruikt, is *DotNetHowToIndexers*.

1. Ga naar [**Azure-Samples/search-dotnet-getting-started**](https://github.com/Azure-Samples/search-dotnet-getting-started) in de GitHub-opslagplaats met Azure-voorbeelden.

2. Klik op **Klonen of downloaden** > **ZIP downloaden**. Standaard wordt het bestand opgeslagen in de map Downloads.

3. Klik in **Verkenner** > **Downloads** met de rechtermuisknop op het bestand en kies **Alles uitpakken**.

4. Schakel alleen-lezen machtigingen uit. Klik met de rechtermuisknop op de naam van de map > **Eigenschappen** > **Algemeen** en schakel het kenmerk **Alleen-lezen** voor de huidige map, submappen en bestanden uit.

5. Open de oplossing *DotNetHowToIndexers.sln* in **Visual Studio 2017**.

6. Klik in **Solution Explorer** met de rechtermuisknop op de bovenste bovenliggende oplossing > **Restore Nuget Packages**.

## <a name="set-up-connections"></a>Verbindingen instellen
Verbindingsgegevens voor benodigde services worden opgegeven in het bestand **appsettings.json** in de oplossing. 

Open **appsettings.json** in Solution Explorer, zodat u elke instelling kunt invoeren op basis van de instructies in deze zelfstudie.  

```json
{
  "SearchServiceName": "Put your search service name here",
  "SearchServiceAdminApiKey": "Put your primary or secondary API key here",
  "AzureSqlConnectionString": "Put your Azure SQL database connection string here",
}
```

### <a name="get-the-search-service-name-and-admin-api-key"></a>De naam van de zoekservice en de api-sleutel voor beheer ophalen

U vindt het eindpunt van de zoekservice en de sleutel in de portal. Een sleutel biedt toegang tot servicebewerkingen. Beheersleutels staan schrijftoegang toe, wat nodig is om objecten zoals indexen en indexeerfuncties in uw service te maken en te verwijderen.

1. Meld u aan bij de [Azure Portal](https://portal.azure.com/) en zoek de [zoekservices voor uw abonnement](https://portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices).

2. Open de servicepagina.

3. Zoek bovenaan op de hoofdpagina de servicenaam. In de volgende schermafbeelding is dit *azs-tutorial*.

   ![Servicenaam](./media/search-indexer-tutorial/service-name.png)

4. Kopieer en plak dit als eerste vermelding in **appsettings.json** in Visual Studio.

  > [!Note]
  > De servicenaam maakt deel uit van het eindpunt dat search.windows.net bevat. Desgewenst kunt u de volledige URL bekijken via **Essentials** op de overzichtspagina. De URL ziet eruit als in dit voorbeeld: https://naam-van-uw-service.search.windows.net

5. Kopieer aan de linkerkant, onder **Instellingen** > **Sleutels**, een van de beheersleutels en plak deze als tweede vermelding in **appsettings.json**. Sleutels zijn alfanumerieke tekenreeksen die tijdens de inrichting voor uw service worden gegenereerd en zijn vereist voor geautoriseerde toegang tot servicebewerkingen. 

  Nadat u beide instellingen hebt toegevoegd, ziet uw bestand er ongeveer uit als in dit voorbeeld:

  ```json
  {
    "SearchServiceName": "azs-tutorial",
    "SearchServiceAdminApiKey": "A1B2C3D4E5F6G7H8I9J10K11L12M13N14",
    . . .
  }
  ```

## <a name="prepare-an-external-data-source"></a>Een externe gegevensbron voorbereiden

In deze stap maakt u een externe gegevensbron die een indexeerfunctie kan verkennen. Het gegevensbestand voor deze zelfstudie is *hotels.sql*, in de oplossingsmap \DotNetHowToIndexers. 

### <a name="azure-sql-database"></a>Azure SQL Database

U kunt de Azure Portal en het bestand *hotels.sql* uit het voorbeeld gebruiken om de gegevensset in Azure SQL Database te maken. Azure Search gebruikt platte rijensets, zoals de sets die worden gegenereerd op basis van een weergave of query. Het SQL-bestand in de voorbeeldoplossing maakt en vult één tabel.

In de volgende oefening wordt ervan uitgegaan dat er geen bestaande server of database is en u maakt beide in stap 2. Als u een bestaande resource hebt, kunt u de tabel hotels er desgewenst aan toevoegen vanaf stap 4.

1. Meld u aan bij [Azure Portal](https://portal.azure.com/). 

2. Klik op **Nieuw** > **SQL-database** om een database, server en resourcegroep te maken. U kunt de standaardinstellingen en de laagste prijscategorie gebruiken. Eén voordeel van het maken van een server is dat u de gebruikersnaam en het wachtwoord van een beheerder kunt opgeven die in een latere stap nodig zijn om tabellen te maken en te laden.

   ![De pagina Nieuwe database](./media/search-indexer-tutorial/indexer-new-sqldb.png)

3. Klik op **Maken** om de nieuwe server en database te implementeren. Wacht tot de server en database zijn geïmplementeerd.

4. Open de pagina SQL Database voor de nieuwe database als deze nog niet is geopend. De resourcenaam moet *SQL-database* zijn en niet *SQL-server*.

  ![De pagina SQL-database](./media/search-indexer-tutorial/hotels-db.png)

4. Klik op de opdrachtbalk op **Extra** > **Query-editor**.

5. Klik op **Aanmelden** en voer de gebruikersnaam en het wachtwoord van de serverbeheerder in.

6. Klik op **Query openen** en navigeer naar de locatie van *hotels.sql*. 

7. Selecteer het bestand en klik op **Openen**. Het script moet er ongeveer uitzien als in de volgende schermafbeelding:

  ![SQL-script](./media/search-indexer-tutorial/sql-script.png)

8. Klik op **Uitvoeren** om de query uit te voeren. In het resultatenvenster zou u het bericht moeten zien dat de query is gelukt voor 3 rijen.

9. Als u een rijenset uit deze tabel wilt retourneren, kunt u de volgende query uitvoeren als verificatiestap:

   ```sql
   SELECT HotelId, HotelName, Tags FROM Hotels
   ```
   De prototypequery, `SELECT * FROM Hotels`, werkt niet in de query-editor. De voorbeeldgegevens bevatten geografische coördinaten in het veld Locatie dat momenteel niet door de editor wordt verwerkt. Voor een lijst met andere kolommen waarvoor u een query kunt uitvoeren, kunt u deze instructie uitvoeren: `SELECT * FROM sys.columns WHERE object_id = OBJECT_ID('dbo.Hotels')`

10. Nu u een externe gegevensset hebt, kopieert u de ADO.NET-verbindingsreeks voor de database. Ga op de pagina SQL Database van uw database naar **Instellingen** > **Verbindingsreeksen** en kopieer de ADO.NET-verbindingsreeks.
 
  Een ADO.NET-verbindingsreeks ziet eruit als in het volgende voorbeeld, dat is gewijzigd zodat een geldige databasenaam, gebruikersnaam en wachtwoord worden gebruikt.

  ```sql
  Server=tcp:hotels-db.database.windows.net,1433;Initial Catalog=hotels-db;Persist Security Info=False;User ID={your_username};Password={your_password};MultipleActiveResultSets=False;Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;
  ```
11. Plak de verbindingsreeks in 'AzureSqlConnectionString' als derde vermelding in het bestand **appsettings.json** in Visual Studio.

    ```json
    {
      "SearchServiceName": "azs-tutorial",
      "SearchServiceAdminApiKey": "A1B2C3D4E5F6G7H8I9J10K11L12M13N14",
      "AzureSqlConnectionString": "Server=tcp:hotels-db.database.windows.net,1433;Initial Catalog=hotels-db;Persist Security  Info=False;User ID={your_username};Password={your_password};MultipleActiveResultSets=False;Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;",
    }
    ```

## <a name="understand-index-and-indexer-code"></a>Code voor index en indexeerfuncties begrijpen

Uw code is nu gereed om te worden samengesteld en uitgevoerd. Voordat u dat doet, moet u even de tijd nemen om de definities van de index en de indexeerfuncties voor dit voorbeeld te bestuderen. De relevante code staat in twee bestanden:

  + **hotel.cs**, dat het schema bevat dat de index definieert
  + **Program.cs**, dat de functies bevat die de structuren in uw service maken en beheren

### <a name="in-hotelcs"></a>In hotel.cs

Het indexschema definieert de verzameling met velden, met inbegrip van kenmerken die toegestane bewerkingen opgeven, bijvoorbeeld of de volledige tekst van een veld kan worden doorzocht, gefilterd of gesorteerd zoals wordt weergegeven in de volgende velddefinitie voor HotelName. 

```csharp
. . . 
[IsSearchable, IsFilterable, IsSortable]
public string HotelName { get; set; }
. . .
```

Een schema kan ook andere elementen bevatten, zoals scoreprofielen om een zoekscore te verbeteren, aangepaste analysefuncties en andere constructies. In dit geval is het schema echter beperkt gedefinieerd en bestaat het alleen uit velden in de voorbeeldgegevenssets.

In deze zelfstudie haalt de indexeerfunctie gegevens op uit één gegevensbron. In de praktijk kunt u meerdere indexeerfuncties koppelen aan de dezelfde index en zo een geconsolideerde doorzoekbare index van meerdere gegevensbronnen en indexeerfuncties maken. U kunt dezelfde combinatie van index en indexeerfunctie gebruiken, waarbij alleen de gegevensbronnen verschillen, of één index gebruiken met verschillende combinaties van indexeerfunctie en gegevensbron, afhankelijk van waar u flexibiliteit nodig hebt.

### <a name="in-programcs"></a>In Program.cs

Het hoofdprogramma bevat functies voor de drie representatieve gegevensbronnen. Als we ons alleen richten op Azure SQL Database, vallen de volgende objecten op:

  ```csharp
  private const string IndexName = "hotels";
  private const string AzureSqlHighWaterMarkColumnName = "RowVersion";
  private const string AzureSqlDataSourceName = "azure-sql";
  private const string AzureSqlIndexerName = "azure-sql-indexer";
  ```

In Azure Search zijn de objecten die u onafhankelijk kunt weergeven, configureren of verwijderen indexen, indexeerfuncties en gegevensbronnen (respectievelijk *hotels*, *azure-sql-indexer* en *azure-sql*). 

De kolom *AzureSqlHighWaterMarkColumnName* verdient een speciale vermelding omdat deze gegevens voor detectie van wijzigingen bevat die door de indexeerfunctie worden gebruikt om te bepalen of een rij is gewijzigd sinds de laatste indexering. [Beleid voor detectie van wijzigingen](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md) wordt alleen ondersteund in indexeerfuncties en verschilt per gegevensbron. Voor Azure SQL Database kunt u kiezen uit twee soorten beleid, afhankelijk van de vereisten voor de database.

De volgende code toont de methoden in Program.cs die worden gebruikt om een gegevensbron en indexeerfunctie te maken. De code controleert op en verwijdert bestaande resources met dezelfde naam, waarbij ervan wordt uitgegaan dat u dit programma meerdere keren uitvoert.

  ```csharp
  private static string SetupAzureSqlIndexer(SearchServiceClient serviceClient, IConfigurationRoot configuration)
  {
    Console.WriteLine("Deleting Azure SQL data source if it exists...");
    DeleteDataSourceIfExists(serviceClient, AzureSqlDataSourceName);

    Console.WriteLine("Creating Azure SQL data source...");
    DataSource azureSqlDataSource = CreateAzureSqlDataSource(serviceClient, configuration);

    Console.WriteLine("Deleting Azure SQL indexer if it exists...");
    DeleteIndexerIfExists(serviceClient, AzureSqlIndexerName);

    Console.WriteLine("Creating Azure SQL indexer...");
    Indexer azureSqlIndexer = CreateIndexer(serviceClient, AzureSqlDataSourceName, AzureSqlIndexerName);

    return azureSqlIndexer.Name;
  }
  ```

Merk op dat API-aanroepen van de indexeerfunctie platformneutraal zijn, behalve in het geval van [DataSourceType](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.datasourcetype?view=azure-dotnet), dat aangeeft welke type verkenner moet worden aangeroepen.

## <a name="run-the-indexer"></a>De indexeerfunctie uitvoeren

In deze stap wordt het programma gecompileerd en uitgevoerd. 

1. Klik in Solution Explorer met de rechtermuisknop op **DotNetHowToIndexers** en selecteer **Build**.
2. Klik nogmaals met de rechtermuisknop op **DotNetHowToIndexers**, gevolgd door **Debug** > **Start new instance**.

Het programma wordt uitgevoerd in de foutopsporingsmodus. De status van elke bewerking wordt weergegeven in een consolevenster.

  ![SQL-script](./media/search-indexer-tutorial/console-output.png)

Uw code wordt lokaal uitgevoerd in Visual Studio en maakt verbinding met uw zoekservice in Azure, die op zijn beurt de verbindingstekenreeks gebruikt om verbinding te maken met Azure SQL Database en de gegevensset op te halen. Met zo veel verschillende bewerkingen zijn er veel punten waar een fout kan optreden, maar als er een fout optreedt, controleert u eerst het volgende:

+ De verbindingsgegevens van de zoekservice die u opgeeft, zijn in deze zelfstudie beperkt tot de servicenaam. Als u de volledige URL hebt ingevoerd, stopt de verwerking bij het maken van de index, met het foutbericht dat er geen verbinding kan worden gemaakt.

+ Gegevens over de databaseverbinding in **appsettings.json**. Dit moet de ADO.NET-verbindingsreeks zijn die u hebt verkregen via de portal en die is gewijzigd, zodat deze een gebruikersnaam en wachtwoord bevat die geldig zijn voor uw database. Het gebruikersaccount moet machtigingen hebben om gegevens op te halen.

+ Bronlimieten. Onthoud dat voor de gedeelde (gratis) service een limiet van 3 indexen, indexeerfuncties en gegevensbronnen geldt. Een service met de maximale limiet kan geen nieuwe objecten maken.

## <a name="search-the-index"></a>Zoeken in de index 

Klik in de Azure Portal op de overzichtspagina van de zoekservice op **Search Explorer** bovenaan om enkele query's voor de nieuwe index te verzenden.

1. Klik op **Index wijzigen** bovenaan om de index *hotels* te selecteren.

2. Klik op de knop **Zoeken** om een lege zoekopdracht te geven. 

  De drie vermeldingen in de index worden geretourneerd als JSON-documenten. Search Explorer retourneert documenten in JSON, zodat u de volledige structuur kunt bekijken.

3. Geef vervolgens een zoekreeks op: `search=river&$count=true`. 

  Deze query roept een zoekopdracht aan die in de volledige tekst zoekt naar de term `river` en het resultaat bevat het aantal overeenkomende documenten. Het aantal overeenkomende documenten retourneren kan handig zijn in testscenario's wanneer u een grote index met duizenden of miljoenen documenten hebt. In dit geval komt slechts één document overeen met de query.

4. Voer ten slotte een zoektekenreeks in die de JSON-uitvoer beperkt tot de gewenste velden: `search=river&$count=true&$select=hotelId, baseRate, description`. 

  De respons van de query wordt beperkt tot geselecteerde velden, wat resulteert in een beknoptere uitvoer.

## <a name="view-indexer-configuration"></a>De configuratie van de indexeerfunctie bekijken

Alle indexeerfuncties, inclusief de functie die u zojuist via programmacode hebt gemaakt, worden vermeld in de portal. U kunt de definitie van een indexeerfunctie openen en de gegevensbron ervan weergeven of een vernieuwingsschema configureren om nieuwe en gewijzigde rijen op te halen.

1. Open de overzichtspagina van uw Azure Search-service.
2. Schuif omlaag naar de tegels met **indexeerfuncties** en **gegevensbronnen**.
3. Klik op een tegel om een lijst met resources te openen. U kunt afzonderlijke indexeerfuncties of gegevensbronnen selecteren om configuratie-instellingen weer te geven of te wijzigen.

  ![Tegels met indexeerfuncties en gegevensbronnen](./media/search-indexer-tutorial/tiles-portal.png)

## <a name="clean-up-resources"></a>Resources opschonen

Als u deze services verder niet gaat gebruiken, volgt u deze stappen om alle resources te verwijderen die met deze zelfstudie in de Azure Portal zijn gemaakt. 

1. Klik in het menu aan de linkerkant in Azure Portal op **Resourcegroepen** en klik vervolgens op de resource die u hebt gemaakt. 
2. Klik op de pagina van uw resourcegroep op **Resourcegroep verwijderen**, typ de naam van de resource die u wilt verwijderen in het tekstvak en klik vervolgens op **Verwijderen**.

## <a name="next-steps"></a>Volgende stappen

Raadpleeg de volgende artikelen voor meer informatie en taken die specifiek zijn voor andere ondersteunde gegevensbronnen:

* [Azure SQL Database of SQL Server op een virtuele Azure-machine](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md)
* [Azure Cosmos DB](search-howto-index-cosmosdb.md)
* [Azure Table Storage](search-howto-indexing-azure-tables.md)
* [Azure Blob Storage](search-howto-indexing-azure-blob-storage.md)
* [Indexeren van CSV-blobs met de indexeerfunctie Azure Search Blob](search-howto-index-csv-blobs.md)
* [Indexeren van JSON-blobs met de indexeerfunctie Azure Search Blob](search-howto-index-json-blobs.md)

