---
title: Aan de slag met Azure Table Storage met .NET | Microsoft Docs
description: Sla gestructureerde gegevens op in de cloud met Azure Table Storage, een oplossing voor NoSQL-gegevensopslag.
services: cosmos-db
documentationcenter: .net
author: mimig1
manager: jhubbard
editor: tysonn
ms.assetid: fe46d883-7bed-49dd-980e-5c71df36adb3
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 11/03/2017
ms.author: mimig
ms.openlocfilehash: 0019858e1142c1f7e7b6fedea5c2ec97518548c9
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/18/2018
---
# <a name="get-started-with-azure-table-storage-using-net"></a>Aan de slag met Azure Table Storage met .NET
[!INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
[!INCLUDE [storage-table-cosmos-db-tip-include](../../includes/storage-table-cosmos-db-tip-include.md)]

Azure Table Storage is een service die gestructureerde NoSQL-gegevens opslaat in de cloud en zo een sleutel/kenmerkarchiefontwerp zonder schema biedt. Omdat Table Storage schemaloos is, kunt u uw gegevens eenvoudig aanpassen naarmate de behoeften van uw toepassing veranderen. Toegang tot Table Storage-gegevens is snel en kostenefficiënt voor veel soorten toepassingen en doorgaans goedkoper dan traditionele SQL voor vergelijkbare gegevensvolumes.

U kunt Table Storage gebruiken voor de opslag van flexibele gegevenssets, zoals gebruikersgegevens voor webtoepassingen, adresboeken, apparaatgegevens of andere soorten metagegevens die uw service nodig heeft. In elke tabel kunt u een willekeurig aantal entiteiten opslaan. Een opslagaccount kan een onbeperkt aantal tabellen bevatten, tot de maximale capaciteit van het opslagaccount.

### <a name="about-this-tutorial"></a>Over deze zelfstudie
In deze zelfstudie leert u hoe u de [Azure Storage-clientbibliotheek voor .NET](https://www.nuget.org/packages/WindowsAzure.Storage/) in een aantal veelvoorkomende scenario's voor Azure Table Storage gebruikt. Deze scenario's worden geïllustreerd met C#-voorbeelden voor het maken en verwijderen van een tabel en het invoegen, bijwerken, verwijderen en opvragen van tabelgegevens.

## <a name="prerequisites"></a>Vereisten

U hebt het volgende nodig om deze zelfstudie te voltooien:

* [Microsoft Visual Studio](https://www.visualstudio.com/downloads/)
* [Azure Storage-clientbibliotheek voor .NET](https://www.nuget.org/packages/WindowsAzure.Storage/)
* [Azure Configuration Manager voor .NET](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager/)
* [Azure Storage-account](../storage/common/storage-create-storage-account.md#create-a-storage-account)

[!INCLUDE [storage-dotnet-client-library-version-include](../../includes/storage-dotnet-client-library-version-include.md)]

### <a name="more-samples"></a>Meer voorbeelden
Zie [Getting Started with Azure Table Storage in .NET](https://azure.microsoft.com/documentation/samples/storage-table-dotnet-getting-started/) (Aan de slag met Azure Table Storage in .NET) voor meer voorbeelden van het gebruik van Table Storage. U kunt de voorbeeldtoepassing downloaden en uitvoeren, of de code bekijken op GitHub.

[!INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

[!INCLUDE [storage-development-environment-include](../../includes/storage-development-environment-include.md)]

### <a name="add-using-directives"></a>Using-instructies toevoegen
Voeg de volgende **using**-instructies aan het begin van het bestand `Program.cs` toe:

```csharp
using Microsoft.Azure; // Namespace for CloudConfigurationManager
using Microsoft.WindowsAzure.Storage; // Namespace for CloudStorageAccount
using Microsoft.WindowsAzure.Storage.Table; // Namespace for Table storage types
```

### <a name="parse-the-connection-string"></a>De verbindingsreeks parseren
[!INCLUDE [storage-cloud-configuration-manager-include](../../includes/storage-cloud-configuration-manager-include.md)]

### <a name="create-the-table-service-client"></a>De Tabelserviceclient maken
Met behulp van de [CloudTableClient][dotnet_CloudTableClient]-klasse kunt u tabellen en entiteiten ophalen die zijn opgeslagen in Table Storage. Hier volgt één manier om de client voor de Table-service te maken:

```csharp
// Create the table client.
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
```

U bent nu klaar om code te schrijven die gegevens leest uit en schrijft naar Table Storage.

## <a name="create-a-table"></a>Een tabel maken
In dit voorbeeld ziet u hoe u een tabel kunt maken als deze nog niet bestaat:

```csharp
// Retrieve the storage account from the connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the table client.
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

// Retrieve a reference to the table.
CloudTable table = tableClient.GetTableReference("people");

// Create the table if it doesn't exist.
table.CreateIfNotExists();
```

## <a name="add-an-entity-to-a-table"></a>Een entiteit toevoegen aan een tabel
Entiteiten worden toegewezen aan C#-objecten met behulp van een aangepaste klasse die is afgeleid van [TableEntity][dotnet_TableEntity]. Als u een entiteit wilt toevoegen aan een tabel, maakt u een klasse die de eigenschappen van uw entiteit definieert. De volgende code definieert een entiteitsklasse die de voornaam van de klant als de rijsleutel en de achternaam als de partitiesleutel gebruikt. De entiteit wordt in de tabel op unieke wijze geïdentificeerd door diens partitie- en rijsleutel. Entiteiten met dezelfde partitiesleutel kunnen sneller worden opgevraagd dan entiteiten met verschillende partitiesleutels, maar het gebruik van verschillende partitiesleutels maakt een grotere schaalbaarheid van parallelle bewerkingen mogelijk. De entiteiten die u in tabellen wilt opslaan, moeten van een ondersteund type zijn. Ze moeten bijvoorbeeld zijn afgeleid van de [TableEntity][dotnet_TableEntity]-klasse. De entiteitseigenschappen die u in een tabel wilt opslaan, moeten openbare eigenschappen van het type zijn. Bovendien moeten ze zowel het ophalen als het instellen van waarden ondersteunen. Ook *moet* uw entiteitstype een parameterloze constructor bevatten.

```csharp
public class CustomerEntity : TableEntity
{
    public CustomerEntity(string lastName, string firstName)
    {
        this.PartitionKey = lastName;
        this.RowKey = firstName;
    }

    public CustomerEntity() { }

    public string Email { get; set; }

    public string PhoneNumber { get; set; }
}
```

Tabelbewerkingen die betrekking hebben op entiteiten, worden uitgevoerd via het [CloudTable][dotnet_CloudTable]-object dat u eerder hebt gemaakt in de sectie Een tabel maken. De bewerking die moet worden uitgevoerd, wordt vertegenwoordigd door een [TableOperation][dotnet_TableOperation]-object. Het volgende codevoorbeeld toont het maken van het [CloudTable][dotnet_CloudTable]-object, gevolgd door een **CustomerEntity**-object. Voor het voorbereiden van de bewerking wordt een [TableOperation][dotnet_TableOperation]-object gemaakt voor het invoegen van de klantentiteit in de tabel. Ten slotte wordt de bewerking uitgevoerd door het aanroepen van [CloudTable][dotnet_CloudTable].[Execute][dotnet_CloudTable_Execute].

```csharp
// Retrieve the storage account from the connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the table client.
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

// Create the CloudTable object that represents the "people" table.
CloudTable table = tableClient.GetTableReference("people");

// Create a new customer entity.
CustomerEntity customer1 = new CustomerEntity("Harp", "Walter");
customer1.Email = "Walter@contoso.com";
customer1.PhoneNumber = "425-555-0101";

// Create the TableOperation object that inserts the customer entity.
TableOperation insertOperation = TableOperation.Insert(customer1);

// Execute the insert operation.
table.Execute(insertOperation);
```

## <a name="insert-a-batch-of-entities"></a>Een batch entiteiten invoegen
U kunt in één schrijfbewerking een batch entiteiten invoegen in een tabel. Enkele aanvullende opmerkingen over batchbewerkingen:

* U kunt in één en dezelfde batchbewerking updates, verwijderingen en invoegingen uitvoeren.
* Een batchbewerking kan maximaal 100 entiteiten bevatten.
* Alle entiteiten in een batchbewerking moeten dezelfde partitiesleutel hebben.
* Het is mogelijk een query als batchbewerking uit te voeren, maar deze moet dan wel de enige bewerking in de batch zijn.

Het volgende codevoorbeeld maakt twee entiteitsobjecten en voegt elk daarvan toe aan [TableBatchOperation][dotnet_TableBatchOperation] met behulp van de methode [Insert][dotnet_TableBatchOperation_Insert]. Vervolgens wordt [CloudTable][dotnet_CloudTable].[ExecuteBatch][dotnet_CloudTable_ExecuteBatch] aangeroepen om de bewerking uit te voeren.

```csharp
// Retrieve the storage account from the connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the table client.
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

// Create the CloudTable object that represents the "people" table.
CloudTable table = tableClient.GetTableReference("people");

// Create the batch operation.
TableBatchOperation batchOperation = new TableBatchOperation();

// Create a customer entity and add it to the table.
CustomerEntity customer1 = new CustomerEntity("Smith", "Jeff");
customer1.Email = "Jeff@contoso.com";
customer1.PhoneNumber = "425-555-0104";

// Create another customer entity and add it to the table.
CustomerEntity customer2 = new CustomerEntity("Smith", "Ben");
customer2.Email = "Ben@contoso.com";
customer2.PhoneNumber = "425-555-0102";

// Add both customer entities to the batch insert operation.
batchOperation.Insert(customer1);
batchOperation.Insert(customer2);

// Execute the batch operation.
table.ExecuteBatch(batchOperation);
```

## <a name="retrieve-all-entities-in-a-partition"></a>Alle entiteiten in een partitie ophalen
Gebruik een [TableQuery][dotnet_TableQuery]-object om een tabel met alle entiteiten in een partitie op te vragen. Het volgende codevoorbeeld geeft een filter voor entiteiten waarbij 'Smith' de partitiesleutel is. In dit voorbeeld worden de velden van elke entiteit in de queryresultaten naar de console afgedrukt.

```csharp
// Retrieve the storage account from the connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the table client.
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

// Create the CloudTable object that represents the "people" table.
CloudTable table = tableClient.GetTableReference("people");

// Construct the query operation for all customer entities where PartitionKey="Smith".
TableQuery<CustomerEntity> query = new TableQuery<CustomerEntity>().Where(TableQuery.GenerateFilterCondition("PartitionKey", QueryComparisons.Equal, "Smith"));

// Print the fields for each customer.
foreach (CustomerEntity entity in table.ExecuteQuery(query))
{
    Console.WriteLine("{0}, {1}\t{2}\t{3}", entity.PartitionKey, entity.RowKey,
        entity.Email, entity.PhoneNumber);
}
```

## <a name="retrieve-a-range-of-entities-in-a-partition"></a>Een bereik van entiteiten in een partitie ophalen
Als u een query niet op alle entiteiten in een partitie wilt uitvoeren, kunt u een bereik opgeven door het partitiesleutelfilter te combineren met een rijsleutelfilter. Het volgende codevoorbeeld maakt gebruik van twee filters om alle entiteiten met de partitie 'Smith' op te halen waarbij de rijsleutel (voornaam) begint met een letter die in het alfabet vóór de 'E' komt. Vervolgens worden de resultaten van de query afgedrukt.

```csharp
// Retrieve the storage account from the connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the table client.
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

// Create the CloudTable object that represents the "people" table.
CloudTable table = tableClient.GetTableReference("people");

// Create the table query.
TableQuery<CustomerEntity> rangeQuery = new TableQuery<CustomerEntity>().Where(
    TableQuery.CombineFilters(
        TableQuery.GenerateFilterCondition("PartitionKey", QueryComparisons.Equal, "Smith"),
        TableOperators.And,
        TableQuery.GenerateFilterCondition("RowKey", QueryComparisons.LessThan, "E")));

// Loop through the results, displaying information about the entity.
foreach (CustomerEntity entity in table.ExecuteQuery(rangeQuery))
{
    Console.WriteLine("{0}, {1}\t{2}\t{3}", entity.PartitionKey, entity.RowKey,
        entity.Email, entity.PhoneNumber);
}
```

## <a name="retrieve-a-single-entity"></a>Eén entiteit ophalen
U kunt een query schrijven om één specifieke entiteit op te halen. De volgende code gebruikt [TableOperation][dotnet_TableOperation] om de klant 'Ben Smith' op te geven. Deze methode retourneert één entiteit in plaats van een verzameling. De geretourneerde waarde in [TableResult][dotnet_TableResult].[Result][dotnet_TableResult_Result] is een **CustomerEntity**-object. Het opgeven van zowel partitie- als rijsleutels in een query is de snelste manier om één entiteit op te halen uit de Tabelservice.

```csharp
// Retrieve the storage account from the connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the table client.
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

// Create the CloudTable object that represents the "people" table.
CloudTable table = tableClient.GetTableReference("people");

// Create a retrieve operation that takes a customer entity.
TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

// Execute the retrieve operation.
TableResult retrievedResult = table.Execute(retrieveOperation);

// Print the phone number of the result.
if (retrievedResult.Result != null)
{
    Console.WriteLine(((CustomerEntity)retrievedResult.Result).PhoneNumber);
}
else
{
    Console.WriteLine("The phone number could not be retrieved.");
}
```

## <a name="replace-an-entity"></a>Een entiteit vervangen
Als u een entiteit wilt bijwerken, haalt u deze op uit de Tabelservice, wijzigt u het entiteitsobject en slaat u de wijzigingen weer op in de Tabelservice. De volgende code wijzigt het telefoonnummer van een bestaande klant. In plaats van [Insert][dotnet_TableOperation_Insert] aan te roepen, gebruikt deze code [Replace][dotnet_TableOperation_Replace]. Met [Replace][dotnet_TableOperation_Replace] wordt de entiteit op de server volledig vervangen, tenzij de entiteit op de server sinds het ophalen is gewijzigd. In dat geval mislukt de bewerking. Hiermee wordt voorkomen dat de toepassing per ongeluk een wijziging overschrijft die door een ander onderdeel van uw toepassing is aangebracht tussen het moment van ophalen en het moment van bijwerken. U kunt deze fout het best als volgt afhandelen: de entiteit opnieuw ophalen, de gewenste wijzigingen (indien nog geldig) aanbrengen en vervolgens een andere [Replace][dotnet_TableOperation_Replace]-bewerking uitvoeren. In de volgende sectie wordt beschreven hoe u dit gedrag kunt wijzigen.

```csharp
// Retrieve the storage account from the connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the table client.
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

// Create the CloudTable object that represents the "people" table.
CloudTable table = tableClient.GetTableReference("people");

// Create a retrieve operation that takes a customer entity.
TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

// Execute the operation.
TableResult retrievedResult = table.Execute(retrieveOperation);

// Assign the result to a CustomerEntity object.
CustomerEntity updateEntity = (CustomerEntity)retrievedResult.Result;

if (updateEntity != null)
{
    // Change the phone number.
    updateEntity.PhoneNumber = "425-555-0105";

    // Create the Replace TableOperation.
    TableOperation updateOperation = TableOperation.Replace(updateEntity);

    // Execute the operation.
    table.Execute(updateOperation);

    Console.WriteLine("Entity updated.");
}
else
{
    Console.WriteLine("Entity could not be retrieved.");
}
```

## <a name="insert-or-replace-an-entity"></a>Een entiteit invoegen of vervangen
[Replace][dotnet_TableOperation_Replace]-bewerkingen mislukken als de entiteit is gewijzigd nadat deze is opgehaald van de server. Voor een succesvolle [Replace][dotnet_TableOperation_Replace]-bewerking moet u de entiteit bovendien eerst ophalen van de server. Soms weet u echter niet of de entiteit op de server aanwezig is en zijn de huidige waarden die erin zijn opgeslagen, niet relevant. Met uw update worden deze allemaal overschreven. Hiervoor gebruikt u een [InsertOrReplace][dotnet_TableOperation_InsertOrReplace]-bewerking. Met deze bewerking wordt de entiteit ingevoegd als deze niet bestaat of vervangen als deze wel bestaat, ongeacht wanneer de laatste update is uitgevoerd.

In het volgende codevoorbeeld wordt een klantentiteit voor 'Fred Jones' gemaakt en ingevoegd in de tabel 'people'. Vervolgens gebruiken we de bewerking [InsertOrReplace][dotnet_TableOperation_InsertOrReplace] om een entiteit met dezelfde partitiesleutel (Jones) en rijsleutel (Fred) op te slaan op de server, dit keer met een andere waarde voor de eigenschap PhoneNumber. Omdat we [InsertOrReplace][dotnet_TableOperation_InsertOrReplace] hebben gebruikt, worden alle bijbehorende eigenschapswaarden vervangen. Als de entiteit 'Fred Jones' echter niet in de tabel had bestaan, zou deze worden ingevoegd.

```csharp
// Retrieve the storage account from the connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the table client.
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

// Create the CloudTable object that represents the "people" table.
CloudTable table = tableClient.GetTableReference("people");

// Create a customer entity.
CustomerEntity customer3 = new CustomerEntity("Jones", "Fred");
customer3.Email = "Fred@contoso.com";
customer3.PhoneNumber = "425-555-0106";

// Create the TableOperation object that inserts the customer entity.
TableOperation insertOperation = TableOperation.Insert(customer3);

// Execute the operation.
table.Execute(insertOperation);

// Create another customer entity with the same partition key and row key.
// We've already created a 'Fred Jones' entity and saved it to the
// 'people' table, but here we're specifying a different value for the
// PhoneNumber property.
CustomerEntity customer4 = new CustomerEntity("Jones", "Fred");
customer4.Email = "Fred@contoso.com";
customer4.PhoneNumber = "425-555-0107";

// Create the InsertOrReplace TableOperation.
TableOperation insertOrReplaceOperation = TableOperation.InsertOrReplace(customer4);

// Execute the operation. Because a 'Fred Jones' entity already exists in the
// 'people' table, its property values will be overwritten by those in this
// CustomerEntity. If 'Fred Jones' didn't already exist, the entity would be
// added to the table.
table.Execute(insertOrReplaceOperation);
```

## <a name="query-a-subset-of-entity-properties"></a>Een query uitvoeren op een subset van entiteitseigenschappen
Met een tabelquery haalt u slechts enkele eigenschappen van een entiteit op in plaats van alle eigenschappen hiervan. Deze methode, projectie genoemd, verbruikt minder bandbreedte en kan de queryprestaties verbeteren, vooral bij grote entiteiten. De query in de volgende code retourneert alleen de e-mailadressen van de entiteiten in de tabel. Dit wordt gedaan via een [DynamicTableEntity][dotnet_DynamicTableEntity]- en een [EntityResolver][dotnet_EntityResolver]-query. Meer informatie over projectie vindt u in het blogbericht [Introducing Upsert and Query Projection][blog_post_upsert] (Introductie tot upsert en queryprojectie). Projectie wordt niet ondersteund door de opslagemulator. Deze code wordt dus alleen uitgevoerd als u een account gebruikt in de Tabelservice.

```csharp
// Retrieve the storage account from the connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the table client.
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

// Create the CloudTable that represents the "people" table.
CloudTable table = tableClient.GetTableReference("people");

// Define the query, and select only the Email property.
TableQuery<DynamicTableEntity> projectionQuery = new TableQuery<DynamicTableEntity>().Select(new string[] { "Email" });

// Define an entity resolver to work with the entity after retrieval.
EntityResolver<string> resolver = (pk, rk, ts, props, etag) => props.ContainsKey("Email") ? props["Email"].StringValue : null;

foreach (string projectedEmail in table.ExecuteQuery(projectionQuery, resolver, null, null))
{
    Console.WriteLine(projectedEmail);
}
```

## <a name="delete-an-entity"></a>Een entiteit verwijderen
U kunt een entiteit gemakkelijk verwijderen nadat u deze hebt opgehaald. Hiervoor gebruikt u hetzelfde patroon dat is getoond voor het bijwerken van een entiteit. Met de volgende code wordt een klantentiteit opgehaald en verwijderd.

```csharp
// Retrieve the storage account from the connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the table client.
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

// Create the CloudTable that represents the "people" table.
CloudTable table = tableClient.GetTableReference("people");

// Create a retrieve operation that expects a customer entity.
TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

// Execute the operation.
TableResult retrievedResult = table.Execute(retrieveOperation);

// Assign the result to a CustomerEntity.
CustomerEntity deleteEntity = (CustomerEntity)retrievedResult.Result;

// Create the Delete TableOperation.
if (deleteEntity != null)
{
    TableOperation deleteOperation = TableOperation.Delete(deleteEntity);

    // Execute the operation.
    table.Execute(deleteOperation);

    Console.WriteLine("Entity deleted.");
}
else
{
    Console.WriteLine("Could not retrieve the entity.");
}
```

## <a name="delete-a-table"></a>Een tabel verwijderen
Ten slotte wordt met het volgende codevoorbeeld een tabel uit een opslagaccount verwijderd. Een verwijderde tabel kan gedurende een bepaalde tijd na de verwijdering niet opnieuw worden gemaakt.

```csharp
// Retrieve the storage account from the connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the table client.
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

// Create the CloudTable that represents the "people" table.
CloudTable table = tableClient.GetTableReference("people");

// Delete the table it if exists.
table.DeleteIfExists();
```

## <a name="retrieve-entities-in-pages-asynchronously"></a>Entiteiten asynchroon ophalen op pagina's
Als u een groot aantal entiteiten leest en u deze wilt verwerken/weergeven terwijl ze worden opgehaald in plaats van te wachten tot ze allemaal zijn geretourneerd, kunt u entiteiten ophalen met behulp van een gesegmenteerde query. Dit voorbeeld laat zien hoe resultaten op pagina's worden geretourneerd met behulp van het Async-Await-patroon, zodat de uitvoering niet wordt geblokkeerd tijdens het wachten op het retourneren van een groot aantal resultaten. Zie [Asynchrone programmering met Async en Await-patroon in .NET (C# en Visual Basic)](https://msdn.microsoft.com/library/hh191443.aspx) voor meer informatie over het gebruik van het Async-Await-patroon in .NET.

```csharp
// Initialize a default TableQuery to retrieve all the entities in the table.
TableQuery<CustomerEntity> tableQuery = new TableQuery<CustomerEntity>();

// Initialize the continuation token to null to start from the beginning of the table.
TableContinuationToken continuationToken = null;

do
{
    // Retrieve a segment (up to 1,000 entities).
    TableQuerySegment<CustomerEntity> tableQueryResult =
        await table.ExecuteQuerySegmentedAsync(tableQuery, continuationToken);

    // Assign the new continuation token to tell the service where to
    // continue on the next iteration (or null if it has reached the end).
    continuationToken = tableQueryResult.ContinuationToken;

    // Print the number of rows retrieved.
    Console.WriteLine("Rows retrieved {0}", tableQueryResult.Results.Count);

// Loop until a null continuation token is received, indicating the end of the table.
} while(continuationToken != null);
```

## <a name="next-steps"></a>Volgende stappen
Nu u de basisprincipes van Table Storage hebt geleerd, volgt u deze koppelingen voor meer informatie over complexere opslagtaken:

* [Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md) is een gratis, zelfstandige app van Microsoft waarmee u visueel met Azure Storage-gegevens kunt werken in Windows, macOS en Linux.
* Zie [Getting Started with Azure Table Storage in .NET](https://azure.microsoft.com/documentation/samples/storage-table-dotnet-getting-started/) (Aan de slag met Azure Table Storage in .NET) voor meer voorbeelden van Table Storage.
* Bekijk de naslagdocumentatie over de Tabelservice voor meer informatie over beschikbare API's:
* [Naslaginformatie over de Storage-clientbibliotheek voor .NET](http://go.microsoft.com/fwlink/?LinkID=390731&clcid=0x409)
* [Naslaginformatie over REST API](http://msdn.microsoft.com/library/azure/dd179355)
* Leer hoe u de code die u schrijft om te werken met Azure Storage, kunt vereenvoudigen met behulp van de [Azure WebJobs SDK](https://github.com/Azure/azure-webjobs-sdk/wiki).
* Bekijk meer functiehandleidingen voor informatie over aanvullende mogelijkheden voor het opslaan van gegevens in Azure.
* [Aan de slag met Azure Blob Storage met .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md) voor het opslaan van niet-gestructureerde gegevens.
* [Verbinding maken met SQL Database met behulp van .NET (C#)](../sql-database/sql-database-develop-dotnet-simple.md) voor het opslaan van relationele gegevens.

[Download and install the Azure SDK for .NET]: /develop/net/
[Creating an Azure Project in Visual Studio]: http://msdn.microsoft.com/library/azure/ee405487.aspx

[blog_post_upsert]: http://blogs.msdn.com/b/windowsazurestorage/archive/2011/09/15/windows-azure-tables-introducing-upsert-and-query-projection.aspx

[dotnet_api_ref]: https://msdn.microsoft.com/library/azure/mt347887.aspx
[dotnet_CloudTableClient]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.table.cloudtableclient.aspx
[dotnet_CloudTable]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.table.cloudtable.aspx
[dotnet_CloudTable_Execute]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.table.cloudtable.execute.aspx
[dotnet_CloudTable_ExecuteBatch]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.table.cloudtable.executebatch.aspx
[dotnet_DynamicTableEntity]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.table.dynamictableentity.aspx
[dotnet_EntityResolver]: https://msdn.microsoft.com/library/jj733144.aspx
[dotnet_TableBatchOperation]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.table.tablebatchoperation.aspx
[dotnet_TableBatchOperation_Insert]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.table.tablebatchoperation.insert.aspx
[dotnet_TableEntity]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.table.tableentity.aspx
[dotnet_TableOperation]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.table.tableoperation.aspx
[dotnet_TableOperation_Insert]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.table.tableoperation.insert.aspx
[dotnet_TableOperation_InsertOrReplace]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.table.tableoperation.insertorreplace.aspx
[dotnet_TableOperation_Replace]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.table.tableoperation.replace.aspx
[dotnet_TableQuery]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.table.tablequery.aspx
[dotnet_TableResult]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.table.tableresult.aspx
[dotnet_TableResult_Result]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.table.tableresult.result.aspx
