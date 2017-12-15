---
title: 'Azure Cosmos DB: Ontwikkelen met de SQL-API in .NET | Microsoft Docs'
description: Meer informatie over het ontwikkelen met Azure Cosmos DB SQL-API met .NET
services: cosmos-db
documentationcenter: 
author: rafats
manager: jhubbard
editor: 
tags: 
ms.assetid: 
ms.service: cosmos-db
ms.devlang: dotnet
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: 
ms.date: 05/10/2017
ms.author: rafats
ms.custom: mvc
ms.openlocfilehash: e37a0993567b6cec7ed6a91e6dad1f2e2c097198
ms.sourcegitcommit: 0e4491b7fdd9ca4408d5f2d41be42a09164db775
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/14/2017
---
# <a name="azure-cosmosdb-develop-with-the-sql-api-in-net"></a>Azure CosmosDB: Ontwikkelen met de SQL-API in .NET

[!INCLUDE [cosmos-db-sql-api](../../includes/cosmos-db-sql-api.md)]

Azure Cosmos DB is de wereldwijd gedistribueerde multimodel-databaseservice van Microsoft. U kunt snel databases maken van documenten, sleutel/waarde-paren en grafen en hier query’s op uitvoeren. Deze databases genieten allemaal het voordeel van de wereldwijde distributie en horizontale schaalmogelijkheden die ten grondslag liggen aan Azure Cosmos DB. 

Deze zelfstudie laat zien hoe u een Azure DB die Cosmos-account maken met de Azure-portal en maak vervolgens een documentdatabase en verzameling met een [partitiesleutel](sql-api-partition-data.md#partition-keys) met behulp van de [SQL .NET API](sql-api-introduction.md). Een partitiesleutel definiëren wanneer u een verzameling maakt, wordt uw toepassing voorbereid moeiteloos schalen als uw gegevens groeit. 

Deze zelfstudie bevat de volgende taken met behulp van de [SQL .NET API](sql-api-sdk-dotnet.md):

> [!div class="checklist"]
> * Maak een Azure Cosmos DB-account
> * Een database en verzameling maken met een partitiesleutel
> * JSON-documenten maken
> * Een document bijwerken
> * Gepartitioneerde verzamelingen opvragen
> * Opgeslagen procedures worden uitgevoerd
> * Een document verwijderen
> * Een database verwijderen

## <a name="prerequisites"></a>Vereisten
Zorg ervoor dat u over de volgende zaken beschikt:

* Een actief Azure-account. Als u nog geen account hebt, kunt u zich aanmelden voor een [gratis account](https://azure.microsoft.com/free/). 

  [!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]

* Als u Visual Studio 2017 nog niet hebt geïnstalleerd, kunt u het downloaden en de **gratis** [Community Edition van Visual Studio 2017](https://www.visualstudio.com/downloads/) gebruiken. Zorg ervoor dat u **Azure-ontwikkeling** inschakelt tijdens de installatie van Visual Studio.

## <a name="create-an-azure-cosmos-db-account"></a>Maak een Azure Cosmos DB-account

Begint met het maken van een Azure DB die Cosmos-account in de Azure portal.

> [!TIP]
> * Hebt u al een Azure DB die Cosmos-account? Als dit het geval is, gaat u verder met [uw Visual Studio-oplossing instellen](#SetupVS)
> * Als u de Emulator Azure Cosmos DB, volgt u de stappen in [Azure Cosmos DB Emulator](local-emulator.md) instellen van de emulator en gaat u verder met [instellen van uw Visual Studio-oplossing](#SetupVS). 
>
>

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <a id="SetupVS"></a>Uw Visual Studio-oplossing instellen
1. Open **Visual Studio** op uw computer.
2. Selecteer in het menu **Bestand** de optie **Nieuw** en kies vervolgens **Project**.
3. In de **nieuw Project** dialoogvenster Selecteer **sjablonen** / **Visual C#** / **Console-App (.NET Framework)**, Geef uw project en klik vervolgens op **OK**.
   ![Schermopname van het venster Nieuw project](./media/tutorial-develop-sql-api-dotnet/nosql-tutorial-new-project-2.png)

4. Klik in **Solution Explorer** met de rechtermuisknop op uw nieuwe consoletoepassing. Deze bevindt zich onder uw Visual Studio-oplossing. Klik vervolgens op **Manage NuGet Packages...**
    
    ![Schermopname van het rechtsklikmenu voor het project](./media/tutorial-develop-sql-api-dotnet/nosql-tutorial-manage-nuget-pacakges.png)
5. In de **NuGet** tabblad **Bladeren**, en het type **documentdb** in het zoekvak.
<!---stopped here--->
6. Zoek **Microsoft.Azure.DocumentDB** in de resultaten en klik op **Installeren**.
   De pakket-id voor de clientbibliotheek van Azure Cosmos DB [Microsoft.Azure.DocumentDB](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB).
   ![Schermopname van het NuGet-Menu voor het vinden van Client-SDK van Azure Cosmos DB](./media/tutorial-develop-sql-api-dotnet/nosql-tutorial-manage-nuget-pacakges-2.png)

    Als u een bericht ontvangt over het controleren van wijzigingen in de oplossing, klikt u op **OK**. Als u een bericht ontvangt over het accepteren van de licentie, klikt u op **Accepteren**.

## <a id="Connect"></a>Verwijzingen naar uw project toevoegen
De overige stappen in deze zelfstudie bieden de SQL-API-codefragmenten die is vereist voor het maken en bijwerken van Azure DB die Cosmos-resources in uw project.

Voeg eerst deze verwijzingen naar uw toepassing.
<!---These aren't added by default when you install the pkg?--->

```csharp
using System.Net;
using Microsoft.Azure.Documents;
using Microsoft.Azure.Documents.Client;
using Newtonsoft.Json;
```

## <a id="add-references"></a>Verbinding maken met uw app

Vervolgens voegt u deze twee constanten en uw *client* variabele in uw toepassing.

```csharp
private const string EndpointUrl = "<your endpoint URL>";
private const string PrimaryKey = "<your primary key>";
private DocumentClient client;
```

Vervolgens, head weer terug naar de [Azure-portal](https://portal.azure.com) voor het ophalen van uw eindpunt-URL en de primaire sleutel. Uw toepassing heeft de eindpunt-URL en de primaire sleutel nodig om te weten waarmee verbinding moet worden gemaakt en om ervoor te zorgen dat Azure Cosmos DB de verbinding van uw toepassing vertrouwt.

Navigeer in de Azure-portal naar uw Azure DB die Cosmos-account, klikt u op **sleutels**, en klik vervolgens op **lezen-schrijven sleutels**.

De URI van de portal kopiëren en plakken via `<your endpoint URL>` in het bestand program.cs. Vervolgens kopieert u de primaire sleutel van de portal en plakt u deze via `<your primary key>`. Verwijder de `<` en `>` van uw waarden.

![Schermopname van de Azure portal gebruikt door de NoSQL-zelfstudie om een C#-consoletoepassing te maken. Toont een Cosmos-DB Azure-account met de sleutels gemarkeerd op de blade van Azure DB die Cosmos-account en de waarden URI en primaire sleutel gemarkeerd op de blade sleutels](./media/tutorial-develop-sql-api-dotnet/nosql-tutorial-keys.png)

## <a id="instantiate"></a>De DocumentClient instantiëren

Maak nu een nieuw exemplaar van de **DocumentClient**.

```csharp
DocumentClient client = new DocumentClient(new Uri(EndpointUrl), PrimaryKey);
```

## <a id="create-database"></a>Een database maken

Maak vervolgens een Cosmos Azure DB [database](sql-api-resources.md#databases) met behulp van de [CreateDatabaseAsync](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.createdatabaseasync.aspx) methode of [CreateDatabaseIfNotExistsAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdatabaseifnotexistsasync.aspx) methode van de  **DocumentClient** klasse van de [SQL .NET SDK](sql-api-sdk-dotnet.md). Een database is een logische container voor een JSON-documentopslag, gepartitioneerd in verzamelingen.

```csharp
await client.CreateDatabaseAsync(new Database { Id = "db" });
```
## <a name="decide-on-a-partition-key"></a>Bepaal een partitiesleutel 

Verzamelingen zijn containers voor het opslaan van documenten. Ze zijn logische resources en kunnen [omvatten een of meer fysieke partities](partition-data.md). Een [partitiesleutel](sql-api-partition-data.md) is een eigenschap (of pad) in uw documenten die wordt gebruikt voor het distribueren van uw gegevens tussen de servers of partities. Alle documenten met dezelfde partitiesleutel worden opgeslagen in dezelfde partitie. 

Bepalen van een partitiesleutel is een belangrijke beslissing te maken voordat u een verzameling maken. Partitiesleutels zijn een eigenschap (of pad) binnen uw documenten die door Azure Cosmos DB kunnen worden gebruikt voor het distribueren van uw gegevens tussen meerdere servers of partities. Cosmos DB de waarde voor de partitiesleutel-hashes en gebruikt het hash-resultaat om te bepalen van de partitie in om op te slaan van het document. Alle documenten met dezelfde partitiesleutel worden opgeslagen in dezelfde partitie en partitiesleutels kunnen niet worden gewijzigd zodra een verzameling is gemaakt. 

Voor deze zelfstudie gaan we de partitiesleutel ingesteld op `/deviceId` zodat alle gegevens voor één apparaat is opgeslagen in één partitie. Wilt u een partitiesleutel die een groot aantal waarden, op die elk over dezelfde frequentie worden gebruikt om ervoor te zorgen dat cosmos DB kunt verdelen als uw gegevens groeit en de bereiken van de volledige doorvoer van de verzameling te kiezen. 

Zie voor meer informatie over partitioneren [partitioneren en schaal in Azure Cosmos DB?](partition-data.md) 

## <a id="CreateColl"></a>Een verzameling maken 

Nu dat we onze partitiesleutel weten `/deviceId`, kunt maken van een [verzameling](sql-api-resources.md#collections) met behulp van de [CreateDocumentCollectionAsync](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.createdocumentcollectionasync.aspx) methode of [CreateDocumentCollectionIfNotExistsAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdocumentcollectionifnotexistsasync.aspx) methode van de **DocumentClient** klasse. Een verzameling is een container van JSON-documenten en eventuele bijbehorende JavaScript-toepassingslogica. 

> [!WARNING]
> Maken van een verzameling heeft gevolgen, zoals u bij het reserveren van doorvoer voor de toepassing om te communiceren met Azure Cosmos DB. Ga voor meer informatie naar onze [pagina met prijzen](https://azure.microsoft.com/pricing/details/cosmos-db/)
> 
> 

```csharp
// Collection for device telemetry. Here the JSON property deviceId is used  
// as the partition key to spread across partitions. Configured for 2500 RU/s  
// throughput and an indexing policy that supports sorting against any  
// number or string property. .
DocumentCollection myCollection = new DocumentCollection();
myCollection.Id = "coll";
myCollection.PartitionKey.Paths.Add("/deviceId");

await client.CreateDocumentCollectionAsync(
    UriFactory.CreateDatabaseUri("db"),
    myCollection,
    new RequestOptions { OfferThroughput = 2500 });
```

Deze methode maakt een REST-API-aanroep om Azure Cosmos DB en de bepalingen van de service een aantal partities op basis van de aangevraagde doorvoer. U kunt de doorvoer van een verzameling wijzigen als de prestaties van uw moet ontwikkelen met behulp van de SDK of de [Azure-portal](set-throughput.md).

## <a id="CreateDoc"></a>JSON-documenten maken
Laten we enkele JSON-documenten in Azure Cosmos DB invoegen. U kunt een [document](sql-api-resources.md#documents) maken met de methode [CreateDocumentAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdocumentasync.aspx) van de klasse **DocumentClient**. Documenten bestaan uit door gebruikers gedefinieerde (willekeurige) JSON-inhoud. Deze voorbeeldklasse bevat een apparaat lezen en een aanroep naar CreateDocumentAsync waarmee het invoegen van een nieuw apparaat lezen in een verzameling.

```csharp
public class DeviceReading
{
    [JsonProperty("id")]
    public string Id;

    [JsonProperty("deviceId")]
    public string DeviceId;

    [JsonConverter(typeof(IsoDateTimeConverter))]
    [JsonProperty("readingTime")]
    public DateTime ReadingTime;

    [JsonProperty("metricType")]
    public string MetricType;

    [JsonProperty("unit")]
    public string Unit;

    [JsonProperty("metricValue")]
    public double MetricValue;
  }

// Create a document. Here the partition key is extracted 
// as "XMS-0001" based on the collection definition
await client.CreateDocumentAsync(
    UriFactory.CreateDocumentCollectionUri("db", "coll"),
    new DeviceReading
    {
        Id = "XMS-001-FE24C",
        DeviceId = "XMS-0001",
        MetricType = "Temperature",
        MetricValue = 105.00,
        Unit = "Fahrenheit",
        ReadingTime = DateTime.UtcNow
    });
```
## <a name="read-data"></a>Gegevens lezen

We lezen van het document door de partitiesleutel en Id via de methode ReadDocumentAsync. Opmerking dat gelezen gegevens een PartitionKey-waarde bevatten (die overeenkomt met de `x-ms-documentdb-partitionkey` aanvraag-header in de REST-API).

```csharp
// Read document. Needs the partition key and the Id to be specified
Document result = await client.ReadDocumentAsync(
  UriFactory.CreateDocumentUri("db", "coll", "XMS-001-FE24C"), 
  new RequestOptions { PartitionKey = new PartitionKey("XMS-0001") });

DeviceReading reading = (DeviceReading)(dynamic)result;
```

## <a name="update-data"></a>Gegevens bijwerken

Nu gaan we enkele gegevens via de methode ReplaceDocumentAsync bijwerken.

```csharp
// Update the document. Partition key is not required, again extracted from the document
reading.MetricValue = 104;
reading.ReadingTime = DateTime.UtcNow;

await client.ReplaceDocumentAsync(
  UriFactory.CreateDocumentUri("db", "coll", "XMS-001-FE24C"), 
  reading);
```

## <a name="delete-data"></a>Gegevens verwijderen

U kunt nu een document door partitiesleutel en id via de methode DeleteDocumentAsync verwijderen.

```csharp
// Delete a document. The partition key is required.
await client.DeleteDocumentAsync(
  UriFactory.CreateDocumentUri("db", "coll", "XMS-001-FE24C"), 
  new RequestOptions { PartitionKey = new PartitionKey("XMS-0001") });
```
## <a name="query-partitioned-collections"></a>Gepartitioneerde verzamelingen opvragen

Wanneer u gegevens in gepartitioneerde verzamelingen opvragen, stuurt Azure Cosmos DB automatisch de query naar de partities die overeenkomt met de partitie sleutelwaarden die zijn opgegeven in het filter (wanneer aanwezig). Deze query wordt bijvoorbeeld doorgestuurd naar partitie met de partitiesleutel 'XMS-0001'.

```csharp
// Query using partition key
IQueryable<DeviceReading> query = client.CreateDocumentQuery<DeviceReading>(
    UriFactory.CreateDocumentCollectionUri("db", "coll"))
    .Where(m => m.MetricType == "Temperature" && m.DeviceId == "XMS-0001");
```
    
De volgende query heeft geen een filter voor de partitiesleutel (DeviceId) en is fanned uit op alle partities waarop deze wordt uitgevoerd op basis van de index van de partitie. Opmerking die u hebt voor het opgeven van de EnableCrossPartitionQuery (`x-ms-documentdb-query-enablecrosspartition` in de REST-API) om de SDK voor het uitvoeren van een query meerdere partities.

```csharp
// Query across partition keys
IQueryable<DeviceReading> crossPartitionQuery = client.CreateDocumentQuery<DeviceReading>(
    UriFactory.CreateDocumentCollectionUri("db", "coll"), 
    new FeedOptions { EnableCrossPartitionQuery = true })
    .Where(m => m.MetricType == "Temperature" && m.MetricValue > 100);
```

## <a name="parallel-query-execution"></a>Parallelle queryuitvoering
De Azure Cosmos DB SQL SDK's 1.9.0 en hoger ondersteuning parallelle uitvoering queryopties, waarmee u kunt uitvoeren lage latentie query's voor gepartitioneerde verzamelingen, zelfs als ze nodig hebben om een groot aantal partities touch. De volgende query is bijvoorbeeld geconfigureerd voor meerdere partities parallel worden uitgevoerd.

```csharp
// Cross-partition Order By queries
IQueryable<DeviceReading> crossPartitionQuery = client.CreateDocumentQuery<DeviceReading>(
    UriFactory.CreateDocumentCollectionUri("db", "coll"), 
    new FeedOptions { EnableCrossPartitionQuery = true, MaxDegreeOfParallelism = 10, MaxBufferedItemCount = 100})
    .Where(m => m.MetricType == "Temperature" && m.MetricValue > 100)
    .OrderBy(m => m.MetricValue);
```
    
U kunt parallelle queryuitvoering beheren door het afstemmen van de volgende parameters:

* Door in te stellen `MaxDegreeOfParallelism`, u kunt de mate van parallelle uitvoering, dat wil zeggen, het maximum aantal gelijktijdige netwerkverbindingen op de verzameling partities beheren. Als u deze instelt op-1, de mate van parallelle uitvoering wordt beheerd door de SDK. Als de `MaxDegreeOfParallelism` is niet opgegeven of is ingesteld op 0, wat de standaardwaarde is, zal er een netwerk met één verbinding met de verzameling partities.
* Door in te stellen `MaxBufferedItemCount`, kunt u handelt uit query latentie en client-side geheugengebruik. Als u deze parameter of stel deze optie in op-1, het aantal items in de buffer opgeslagen tijdens parallelle queryuitvoering wordt beheerd door de SDK.

Gezien de dezelfde status van de verzameling, retourneert een parallelle query resultaten in dezelfde volgorde als in seriële uitvoering. Bij het uitvoeren van een query voor cross-partitie met sorteren (ORDER BY en/of boven), wordt de SQL-SDK problemen met de query parallel meerdere partities en gedeeltelijk gesorteerde resulteert in de clientzijde levert geen resultaten globaal geordende worden samengevoegd.

## <a name="execute-stored-procedures"></a>Opgeslagen procedures worden uitgevoerd
Ten slotte kunt u atomische transacties tegen documenten met dezelfde apparaat-ID, zoals uitvoeren als u onderhoudt statistische functies of de meest recente toestand van een apparaat in één document door de volgende code toe te voegen aan uw project.

```csharp
await client.ExecuteStoredProcedureAsync<DeviceReading>(
    UriFactory.CreateStoredProcedureUri("db", "coll", "SetLatestStateAcrossReadings"),
    new RequestOptions { PartitionKey = new PartitionKey("XMS-001") }, 
    "XMS-001-FE24C");
```

En dat is alles! Dit zijn de belangrijkste onderdelen van een Azure DB die Cosmos-toepassing die gebruikmaakt van een partitiesleutel efficiënt schalen gegevensdistributie meerdere partities.  

## <a name="clean-up-resources"></a>Resources opschonen

Als u niet gaat u deze app blijven gebruiken, verwijdert u alle resources die zijn gemaakt met deze zelfstudie in de Azure portal met de volgende stappen:

1. Klik in het menu links in de Azure portal op **resourcegroepen** en klik vervolgens op de unieke naam van de resource die u hebt gemaakt. 
2. Klik op de pagina van uw resourcegroep op **Verwijderen**, typ de naam van de resource die u wilt verwijderen in het tekstvak en klik vervolgens op **Verwijderen**.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u het volgende gedaan: 

> [!div class="checklist"]
> * Een Azure DB die Cosmos-account gemaakt
> * Een database en verzameling gemaakt met een partitiesleutel
> * Gemaakte JSON-documenten
> * Een document wordt bijgewerkt
> * De query gepartitioneerde verzamelingen
> * Een opgeslagen procedure is uitgevoerd
> * Een document verwijderd
> * Een database wordt verwijderd

U kunt nu doorgaan met de volgende zelfstudie en aanvullende gegevens importeren in uw account Cosmos DB. 

> [!div class="nextstepaction"]
> [Gegevens importeren in Azure Cosmos DB](import-data.md)
