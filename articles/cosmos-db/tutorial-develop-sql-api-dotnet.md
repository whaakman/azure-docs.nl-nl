---
title: 'Azure Cosmos DB: ontwikkelen met de SQL-API in .NET | Microsoft Docs'
description: Meer informatie over het ontwikkelen met de SQL-API van Azure Cosmos DB met .NET
services: cosmos-db
documentationcenter: ''
author: rafats
manager: kfile
editor: ''
tags: ''
ms.assetid: ''
ms.service: cosmos-db
ms.devlang: dotnet
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: ''
ms.date: 05/10/2017
ms.author: rafats
ms.custom: mvc
ms.openlocfilehash: 528832473d68fa90e6383873b1e0491f5abe09c7
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2018
---
# <a name="azure-cosmos-db-develop-with-the-sql-api-in-net"></a>Azure Cosmos DB: ontwikkelen met de SQL-API in .NET

Azure Cosmos DB is de globaal gedistribueerde multimodel-databaseservice van Microsoft. U kunt snel databases maken van documenten, sleutel/waarde-paren en grafieken en hier query’s op uitvoeren. Deze databases genieten allemaal het voordeel van de globale distributie en horizontale schaalmogelijkheden die ten grondslag liggen aan Azure Cosmos DB.

Deze zelfstudie laat zien hoe u een Azure DB die Cosmos-account maakt behulp van Azure Portal en vervolgens een documentdatabase en verzameling maakt met een [partitiesleutel](sql-api-partition-data.md#partition-keys) met behulp van de [SQL .NET API](sql-api-introduction.md). Door een partitiesleutel te definiëren wanneer u een verzameling maakt, kan uw toepassing in de toekomst moeiteloos worden uitgebreid als uw gegevens groeien.

In deze zelfstudie komen de volgende taken die worden uitgevoerd met [SQL .NET API](sql-api-sdk-dotnet.md) aan de orde:

> [!div class="checklist"]
> * Maak een Azure Cosmos DB-account
> * Een database en verzameling maken met een partitiesleutel
> * JSON-documenten maken
> * Een document bijwerken
> * Gepartitioneerde verzamelingen opvragen
> * Opgeslagen procedures uitvoeren
> * Een document verwijderen
> * Een database verwijderen

## <a name="prerequisites"></a>Vereisten
Zorg ervoor dat u het volgende hebt voordat u begint:

* Toegang tot een Azure Cosmos DB-account

  [!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]

  U kunt ook uw eigen Azure-abonnement gebruiken door u te registreren voor een [gratis Azure-account](https://azure.microsoft.com/free/). Vanaf dat moment kunt u een [Azure Cosmos DB-account maken](create-sql-api-dotnet.md#create-a-database-account).

* Als u Visual Studio 2017 nog niet hebt geïnstalleerd, kunt u het downloaden en de **gratis** [Community Edition van Visual Studio 2017](https://www.visualstudio.com/downloads/) gebruiken. Zorg ervoor dat u **Azure-ontwikkeling** inschakelt tijdens de installatie van Visual Studio.


> [!TIP]
> * Als u ervoor kiest de Azure Cosmos DB-emulator te gebruiken, volgt u de stappen in [Azure Cosmos DB-emulator](local-emulator.md) om de emulator in te stellen
>
>


## <a id="SetupVS"></a>Uw Visual Studio-oplossing instellen
1. Open **Visual Studio** op uw computer.
2. Selecteer in het menu **Bestand** de optie **Nieuw** en kies vervolgens **Project**.
3. Selecteer in het dialoogvenster **Nieuw project** achtereenvolgens **Sjablonen** / **Visual C#** / **Consoletoepassing (.NET Framework)**, geef een naam op voor uw op en klik vervolgens op **OK**.
   ![Schermopname van het venster Nieuw project](./media/tutorial-develop-sql-api-dotnet/nosql-tutorial-new-project-2.png)

4. Klik in **Solution Explorer** met de rechtermuisknop op uw nieuwe consoletoepassing. Deze bevindt zich onder uw Visual Studio-oplossing. Klik vervolgens op **Manage NuGet Packages...**

    ![Schermopname van het rechtsklikmenu voor het project](./media/tutorial-develop-sql-api-dotnet/nosql-tutorial-manage-nuget-pacakges.png)
5. Klik op het tabblad **NuGet** op **Bladeren** en typ **documentdb** in het zoekvak.
<!---stopped here--->
6. Zoek **Microsoft.Azure.DocumentDB** in de resultaten en klik op **Installeren**.
   De pakket-id voor de Azure Cosmos DB-clientbibliotheek is [Microsoft.Azure.DocumentDB](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB).
   ![Schermopname van het NuGet-menu voor het zoeken naar de Azure Cosmos DB-client-SDK](./media/tutorial-develop-sql-api-dotnet/nosql-tutorial-manage-nuget-pacakges-2.png)

    Als u een bericht ontvangt over het controleren van wijzigingen in de oplossing, klikt u op **OK**. Als u een bericht ontvangt over het accepteren van de licentie, klikt u op **Accepteren**.

## <a id="Connect"></a>Verwijzingen naar uw project toevoegen
De overige stappen in deze zelfstudie bevatten de SQL-API-codefragmenten die vereist zijn voor het maken en bijwerken van Azure DB Cosmos-resources in uw project.

Voeg eerst deze verwijzingen toe aan uw toepassing.
<!---These aren't added by default when you install the pkg?--->

```csharp
using System.Net;
using Microsoft.Azure.Documents;
using Microsoft.Azure.Documents.Client;
using Newtonsoft.Json;
```

## <a id="add-references"></a>Uw app verbinden

U voegt nu deze twee constanten en uw *client*variabele toe aan uw toepassing.

```csharp
private const string EndpointUrl = "<your endpoint URL>";
private const string PrimaryKey = "<your primary key>";
private DocumentClient client;
```

Ga vervolgens terug naar [Azure Portal](https://portal.azure.com) om uw eindpunt-URL en primaire sleutel op te halen. Uw toepassing heeft de eindpunt-URL en de primaire sleutel nodig om te weten waarmee verbinding moet worden gemaakt en om ervoor te zorgen dat Azure Cosmos DB de verbinding van uw toepassing vertrouwt.

Navigeer in de Azure-portal naar uw Azure Cosmos DB-account. Selecteer **Sleutels** in het menu links, en selecteer vervolgens **Sleutels voor lezen/schrijven**.

Kopieer in de portal de URI en plak deze over `<your endpoint URL>` in het bestand program.cs. Kopieer vervolgens de PRIMAIRE SLEUTEL van de portal en plak deze over `<your primary key>`. Verwijder de `<` en `>` uit uw waarden.

![Schermopname van Azure Portal die voor de NoSQL-zelfstudie wordt gebruikt om een C#-consoletoepassing te maken. Schermopname van Azure Portal waarin SLEUTELS is gemarkeerd in de sectie Azure Cosmos DB-account en de waarden URI en PRIMAIRE SLEUTEL zijn gemarkeerd in de sectie Sleutels](./media/tutorial-develop-sql-api-dotnet/nosql-tutorial-keys.png)

## <a id="instantiate"></a>De DocumentClient maken

Maak nu een nieuw exemplaar van de **DocumentClient**.

```csharp
DocumentClient client = new DocumentClient(new Uri(EndpointUrl), PrimaryKey);
```

## <a id="create-database"></a>Een database maken

Maak nu een Cosmos Azure DB-[database](sql-api-resources.md#databases) met behulp van de methode [CreateDatabaseAsync](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.createdatabaseasync.aspx) of de methode [CreateDatabaseIfNotExistsAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdatabaseifnotexistsasync.aspx) van de  **DocumentClient**-klasse van de [SQL .NET SDK](sql-api-sdk-dotnet.md). Een database is een logische container voor een JSON-documentopslag, gepartitioneerd in verzamelingen.

```csharp
await client.CreateDatabaseAsync(new Database { Id = "db" });
```
## <a name="decide-on-a-partition-key"></a>Een partitiesleutel bepalen

Verzamelingen zijn containers voor het opslaan van documenten. Het zijn logische resources en ze kunnen een of meer [fysieke partities omvatten](partition-data.md). Een [partitiesleutel](sql-api-partition-data.md) is een eigenschap (of pad) in uw documenten die wordt gebruikt voor het distribueren van uw gegevens tussen de servers of partities. Alle documenten met dezelfde partitiesleutel worden opgeslagen in dezelfde partitie.

Het bepalen van een partitiesleutel is een belangrijke beslissing die u moet maken voordat u een verzameling maakt. Partitiesleutels zijn een eigenschap (of pad) in uw documenten die door Azure Cosmos DB kan worden wordt gebruikt om uw gegevens tussen de servers of partities te verdelen. Cosmos DB plaatst de waarde van de partitiesleutel in een hash en gebruikt het hash-resultaat om de partitie te bepalen waarin het document moet worden opgeslagen. Alle documenten met dezelfde partitiesleutel worden opgeslagen in dezelfde partitie en partitiesleutels kunnen niet worden gewijzigd zodra een verzameling is gemaakt.

Voor deze zelfstudie moet u de partitiesleutel instellen op `/deviceId`, zodat alle gegevens voor één apparaat in één partitie zijn opgeslagen. U wilt een partitiesleutel kiezen die een groot aantal waarden heeft, die alle op ongeveer dezelfde frequentie worden gebruikt om ervoor te zorgen dat Cosmos DB lasten kan verdelen als uw gegevens toenemen en de volledige doorvoer van de verzameling benut.

Zie [Partitioneren en schalen in Azure Cosmos DB](partition-data.md) voor meer informatie over partitionering.

## <a id="CreateColl"></a>Een verzameling maken

Met de partitiesleutel, `/deviceId`, kunt u een [verzameling](sql-api-resources.md#collections) maken met behulp van de methode [CreateDocumentCollectionAsync](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.createdocumentcollectionasync.aspx) of de methode [CreateDocumentCollectionIfNotExistsAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdocumentcollectionifnotexistsasync.aspx) van de **DocumentClient**-klasse. Een verzameling is een container van JSON-documenten en de bijbehorende JavaScript-toepassingslogica.

> [!WARNING]
> Het maken van een verzameling heeft gevolgen voor de prijs, omdat u doorvoer reserveert voor de toepassing om te communiceren met Azure Cosmos DB. Zie de [pagina met prijzen](https://azure.microsoft.com/pricing/details/cosmos-db/) voor meer informatie
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

Met deze methode wordt een REST-API gemaakt om Azure Cosmos DB aan te roepen, en de service voorziet in een aantal partities op basis van de aangevraagde doorvoer. U kunt de doorvoer van een verzameling of een set verzamelingen wijzigen met behulp van de SDK of [Azure Portal](set-throughput.md) als uw prestatiebehoeften veranderen.

## <a id="CreateDoc"></a>JSON-documenten maken
We gaan enkele JSON-documenten in Azure Cosmos DB invoegen. U kunt een [document](sql-api-resources.md#documents) maken met de methode [CreateDocumentAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdocumentasync.aspx) van de klasse **DocumentClient**. Documenten bestaan uit door gebruikers gedefinieerde (willekeurige) JSON-inhoud. Deze voorbeeldklasse bevat te lezen apparaatgegevens en een aanroep naar CreateDocumentAsync waarmee nieuwe te lezen apparaatgegevens in een verzameling wordt ingevoegd.

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

We lezen het document op basis van de partitiesleutel en -id met behulp van de methode ReadDocumentAsync. U ziet dat te lezen gegevens een PartitionKey-waarde bevatten (die overeenkomt met de `x-ms-documentdb-partitionkey`-aanvraagheader in de REST-API).

```csharp
// Read document. Needs the partition key and the Id to be specified
Document result = await client.ReadDocumentAsync(
  UriFactory.CreateDocumentUri("db", "coll", "XMS-001-FE24C"),
  new RequestOptions { PartitionKey = new PartitionKey("XMS-0001") });

DeviceReading reading = (DeviceReading)(dynamic)result;
```

## <a name="update-data"></a>Gegevens bijwerken

We gaan nu enkele gegevens via de methode ReplaceDocumentAsync bijwerken.

```csharp
// Update the document. Partition key is not required, again extracted from the document
reading.MetricValue = 104;
reading.ReadingTime = DateTime.UtcNow;

await client.ReplaceDocumentAsync(
  UriFactory.CreateDocumentUri("db", "coll", "XMS-001-FE24C"),
  reading);
```

## <a name="delete-data"></a>Gegevens verwijderen

We gaan nu een document verwijderen op partitiesleutel en -id met behulp van de methode DeleteDocumentAsync.

```csharp
// Delete a document. The partition key is required.
await client.DeleteDocumentAsync(
  UriFactory.CreateDocumentUri("db", "coll", "XMS-001-FE24C"),
  new RequestOptions { PartitionKey = new PartitionKey("XMS-0001") });
```
## <a name="query-partitioned-collections"></a>Gepartitioneerde verzamelingen opvragen

Wanneer u gegevens in gepartitioneerde verzamelingen opvraagt, stuurt Azure Cosmos DB de query automatisch naar de partities die overeenkomen met de partitiesleutelwaarden die zijn opgegeven in het filter (indien aanwezig). Deze query wordt bijvoorbeeld doorgestuurd naar de partitie met de partitiesleutel 'XMS-0001'.

```csharp
// Query using partition key
IQueryable<DeviceReading> query = client.CreateDocumentQuery<DeviceReading>(
    UriFactory.CreateDocumentCollectionUri("db", "coll"))
    .Where(m => m.MetricType == "Temperature" && m.DeviceId == "XMS-0001");
```

De volgende query heeft geen een filter voor de partitiesleutel (DeviceId) en wordt verspreid over alle partities waarop deze wordt uitgevoerd op basis van de index van de partitie. U moet de EnableCrossPartitionQuery (`x-ms-documentdb-query-enablecrosspartition` in de REST-API) opgeven om de SDK een query te laten uitvoeren op meerdere partities.

```csharp
// Query across partition keys
IQueryable<DeviceReading> crossPartitionQuery = client.CreateDocumentQuery<DeviceReading>(
    UriFactory.CreateDocumentCollectionUri("db", "coll"),
    new FeedOptions { EnableCrossPartitionQuery = true })
    .Where(m => m.MetricType == "Temperature" && m.MetricValue > 100);
```

## <a name="parallel-query-execution"></a>Parallelle queryuitvoering
De Azure Cosmos DB SQL SDK's 1.9.0 en hoger ondersteunen opties voor parallelle uitvoering van query’s, waarmee u query's met een lage latentie kunt uitvoeren op gepartitioneerde verzamelingen, zelfs als ze een groot aantal partities betreffen. De volgende query is bijvoorbeeld geconfigureerd voor parallelle uitvoering in meerdere partities.

```csharp
// Cross-partition Order By queries
IQueryable<DeviceReading> crossPartitionQuery = client.CreateDocumentQuery<DeviceReading>(
    UriFactory.CreateDocumentCollectionUri("db", "coll"),
    new FeedOptions { EnableCrossPartitionQuery = true, MaxDegreeOfParallelism = 10, MaxBufferedItemCount = 100})
    .Where(m => m.MetricType == "Temperature" && m.MetricValue > 100)
    .OrderBy(m => m.MetricValue);
```

U kunt parallelle queryuitvoering beheren door de volgende parameters af te stemmen:

* Door `MaxDegreeOfParallelism` in te stellen, kunt u de mate van parallelle uitvoering, dat wil zeggen het maximum aantal gelijktijdige netwerkverbindingen met de verzameling partities, beheren. Als u deze parameter instelt op -1, wordt de mate van parallelle uitvoering beheerd door de SDK. Als de `MaxDegreeOfParallelism` niet is opgegeven of is ingesteld op 0, wat de standaardwaarde is, is er één netwerkverbinding met de partities van de verzameling.
* Door `MaxBufferedItemCount` in te stellen, kunt u de querylatentie en het geheugengebruik aan clientzijde uitruilen. Als u deze parameter weglaat of op -1 instelt, wordt het aantal items dat in de buffer wordt opgeslagen tijdens parallelle queryuitvoering beheerd door de SDK.

Gezien dezelfde status van de verzameling, retourneert een parallelle query resultaten in dezelfde volgorde als bij seriële uitvoering. Wanneer u een partitieoverkoepelende query uitvoert met sortering (SORTEREN OP en/of BOVENAAN), wordt de query door de SQL-SDK parallel in meerdere partities uitgegeven en worden gedeeltelijk gesorteerde resultaten samengevoegd aan de clientzijde om globaal geordende resultaten te produceren.

## <a name="execute-stored-procedures"></a>Opgeslagen procedures uitvoeren
Ten slotte kunt u atomische transacties uitvoeren voor documenten met dezelfde apparaat-id, bijvoorbeeld als u statistische functies of de meest recente status van een apparaat in één document beheert door de volgende code toe te voegen aan uw project.

```csharp
await client.ExecuteStoredProcedureAsync<DeviceReading>(
    UriFactory.CreateStoredProcedureUri("db", "coll", "SetLatestStateAcrossReadings"),
    new RequestOptions { PartitionKey = new PartitionKey("XMS-001") },
    "XMS-001-FE24C");
```

Dat is alles. Dit zijn de belangrijkste onderdelen van een Azure Cosmos DB-toepassing die gebruikmaakt van een partitiesleutel om gegevensdistributie efficiënt te schalen over meerdere partities.  

## <a name="clean-up-resources"></a>Resources opschonen

Als u deze app niet verder gaat gebruiken, kunt u alle resources verwijderen die door deze zelfstudie zijn aangemaakt door de onderstaande stappen te volgen in Azure Portal:

1. Klik in het menu aan de linkerkant in Azure Portal op **Resourcegroepen** en klik vervolgens op de unieke naam van de resource die u hebt gemaakt.
2. Klik op de pagina van uw resourcegroep op **Verwijderen**, typ de naam van de resource die u wilt verwijderen in het tekstvak en klik vervolgens op **Verwijderen**.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u het volgende gedaan:

> [!div class="checklist"]
> * Een Azure Cosmos DB-account gemaakt
> * Een database en verzameling met een partitiesleutel gemaakt
> * JSON-documenten gemaakt
> * Een document bijgewerkt
> * Gepartitioneerde verzamelingen opgevraagd
> * Een opgeslagen procedure uitgevoerd
> * Een document verwijderd
> * Een database verwijderd

U kunt nu verdergaan met de volgende zelfstudie en aanvullende gegevens in uw Cosmos DB-account importeren.

> [!div class="nextstepaction"]
> [Gegevens importeren in Azure Cosmos DB](import-data.md)
