---
title: Hoe u aan de slag met de tabelopslag en Visual Studio verbonden services (ASP.NET Core) | Microsoft Docs
description: Hoe u aan de slag met Azure Table storage in een ASP.NET Core-project in Visual Studio nadat u verbinding met een opslagaccount met Visual Studio hebt verbonden services
services: storage
documentationcenter: 
author: kraigb
manager: ghogen
editor: 
ms.assetid: c3c451d1-71ff-4222-a348-c41c98a02b85
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: vs-getting-started
ms.devlang: na
ms.topic: article
ms.date: 11/14/2017
ms.author: kraigb
ms.openlocfilehash: 4693c637597346e5c72bef6ddca9369e67b740f3
ms.sourcegitcommit: c4cc4d76932b059f8c2657081577412e8f405478
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/11/2018
---
# <a name="how-to-get-started-with-azure-table-storage-and-visual-studio-connected-services"></a>Hoe u aan de slag met Azure Table storage en Visual Studio verbonden services

[!INCLUDE [storage-try-azure-tools-tables](../../includes/storage-try-azure-tools-tables.md)]

Dit artikel wordt beschreven hoe u aan de slag met Azure Table storage in Visual Studio nadat u hebt gemaakt of een Azure storage-account in een ASP.NET Core-project waarnaar wordt verwezen door het gebruik van de Visual Studio **verbonden Services** functie. De **verbonden Services** bewerking installeert de juiste NuGet-pakketten voor toegang tot Azure-opslag in uw project en voegt u de verbindingsreeks voor de storage-account toe aan uw project configuratiebestanden. (Zie [documentatie Storage](https://azure.microsoft.com/documentation/services/storage/) voor algemene informatie over Azure Storage.)

De service Azure Table storage kunt u voor het opslaan van grote hoeveelheden gestructureerde gegevens. De service is een NoSQL-gegevensarchief die geverifieerde aanroepen vanuit binnen en buiten de Azure-cloud accepteert. Azure-tabellen zijn ideaal voor het opslaan van gestructureerde, niet-relationele gegevens. Zie voor meer algemene informatie over het gebruik van Azure Table storage [aan de slag met Azure Table storage met .NET](../storage/storage-dotnet-how-to-use-tables.md).

Om te beginnen, moet u eerst een tabel maken in uw opslagaccount. In dit artikel geeft vervolgens het maken van een tabel in C# en hoe basistabel bewerkingen zoals het toevoegen, wijzigen, lezen en verwijderen van tabelvermeldingen uit te voeren.  De code gebruikt de Azure Storage-clientbibliotheek voor .NET. Zie voor meer informatie over ASP.NET [ASP.NET](http://www.asp.net).

Sommige van de Azure Storage-API's zijn asynchroon en de code in dit artikel wordt ervan uitgegaan dat async-methoden worden gebruikt. Zie [asynchrone programmering](https://docs.microsoft.com/dotnet/csharp/async) voor meer informatie.

## <a name="access-tables-in-code"></a>Toegang tot tabellen in code

Voor toegang tot tabellen in ASP.NET Core projecten, moet u de volgende items naar de C# bronbestanden die toegang hebben tot Azure-tabelopslag bevatten.

1. Voeg de vereiste `using` instructies:

    ```cs
    using Microsoft.Framework.Configuration;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Table;
    using System.Threading.Tasks;
    using LogLevel = Microsoft.Framework.Logging.LogLevel;
    ```

1. Ophalen van een `CloudStorageAccount` -object met gegevens over uw storage-account. Gebruik de volgende code om de verbindingsreeks voor opslag en de gegevens van de storage-account van de configuratie van de Azure-service:

    ```cs
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```

1. Ophalen van een `CloudTableClient` object om te verwijzen naar de tabelobjecten in uw opslagaccount:

    ```cs
    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
    ```

1. Ophalen van een `CloudTable` reference-object om te verwijzen naar een specifieke tabel en entiteiten:

    ```cs
    // Get a reference to a table named "peopleTable"
    CloudTable peopleTable = tableClient.GetTableReference("peopleTable");
    ```

## <a name="create-a-table-in-code"></a>Een tabel in code maken

Aanroepen voor het maken van de Azure-tabel '' CreateIfNotExistsAsync()':

```cs
// Create the CloudTable if it does not exist
await peopleTable.CreateIfNotExistsAsync();
```

## <a name="add-an-entity-to-a-table"></a>Een entiteit toevoegen aan een tabel

Als u wilt een entiteit toevoegen aan een tabel, moet u een klasse die de eigenschappen van uw entiteit definieert maken. De volgende code definieert een entiteitsklasse aangeroepen `CustomerEntity` die de voornaam van de klant gebruikt als de rijsleutel en de achternaam als de partitiesleutel.

```cs
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

Bewerkingen met betrekking tot entiteiten gebruik tabel de `CloudTable` object u eerder hebt gemaakt in [tabellen in code toegang](#access-tables-in-code). De `TableOperation` object vertegenwoordigt de bewerking worden uitgevoerd. Het volgende codevoorbeeld toont het maken van een `CloudTable` object en een `CustomerEntity` object. Voorbereiden van de bewerking een `TableOperation` voor het invoegen van de klantentiteit in de tabel is gemaakt. Ten slotte de bewerking wordt uitgevoerd door het aanroepen van `CloudTable.ExecuteAsync`.

```cs
// Create a new customer entity.
CustomerEntity customer1 = new CustomerEntity("Harp", "Walter");
customer1.Email = "Walter@contoso.com";
customer1.PhoneNumber = "425-555-0101";

// Create the TableOperation that inserts the customer entity.
TableOperation insertOperation = TableOperation.Insert(customer1);

// Execute the insert operation.
await peopleTable.ExecuteAsync(insertOperation);
```

## <a name="insert-a-batch-of-entities"></a>Een batch entiteiten invoegen

U kunt meerdere entiteiten invoegen in een tabel in een enkel schrijfbewerking. Het volgende codevoorbeeld maakt twee entiteitsobjecten ('Jeff Smith' en 'Ben Smith'), worden toegevoegd aan een `TableBatchOperation` object met de `Insert` methode, en start vervolgens de bewerking door het aanroepen van `CloudTable.ExecuteBatchAsync`.

```cs
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
await peopleTable.ExecuteBatchAsync(batchOperation);
```

## <a name="get-all-of-the-entities-in-a-partition"></a>Alle entiteiten in een partitie ophalen

Gebruiken om te vragen een tabel voor alle entiteiten in een partitie, een `TableQuery` object. Het volgende codevoorbeeld geeft een filter voor entiteiten waarbij 'Smith' de partitiesleutel is. In dit voorbeeld worden de velden van elke entiteit in de queryresultaten naar de console afgedrukt.

```cs
// Construct the query operation for all customer entities where PartitionKey="Smith".
TableQuery<CustomerEntity> query = new TableQuery<CustomerEntity>().Where(TableQuery.GenerateFilterCondition("PartitionKey", QueryComparisons.Equal, "Smith"));

// Print the fields for each customer.
TableContinuationToken token = null;
do
{
    TableQuerySegment<CustomerEntity> resultSegment = await peopleTable.ExecuteQuerySegmentedAsync(query, token);
    token = resultSegment.ContinuationToken;

    foreach (CustomerEntity entity in resultSegment.Results)
    {
        Console.WriteLine("{0}, {1}\t{2}\t{3}", entity.PartitionKey, entity.RowKey,
        entity.Email, entity.PhoneNumber);
    }
} while (token != null);
```

## <a name="get-a-single-entity"></a>Één entiteit ophalen

U kunt een query als u één specifieke entiteit schrijven. De volgende code wordt een `TableOperation` object opgeven met de naam 'Ben Smith' van de klant. De methode retourneert één entiteit in plaats van een verzameling en de geretourneerde waarde in `TableResult.Result` is een `CustomerEntity` object. Het opgeven van de partitie-als rijsleutels in een query is de snelste manier om op te halen van één entiteit van de `Table` service.

```cs
// Create a retrieve operation that takes a customer entity.
TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

// Execute the retrieve operation.
TableResult retrievedResult = await peopleTable.ExecuteAsync(retrieveOperation);

// Print the phone number of the result.
if (retrievedResult.Result != null)
   Console.WriteLine(((CustomerEntity)retrievedResult.Result).PhoneNumber);
else
   Console.WriteLine("The phone number could not be retrieved.");
```

## <a name="delete-an-entity"></a>Een entiteit verwijderen

Nadat u deze hebt gevonden, kunt u een entiteit verwijderen. De volgende code zoekt en verwijdert een klantentiteit met de naam 'Ben Smith':

```cs
// Create a retrieve operation that expects a customer entity.
TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

// Execute the operation.
TableResult retrievedResult = peopleTable.Execute(retrieveOperation);

// Assign the result to a CustomerEntity object.
CustomerEntity deleteEntity = (CustomerEntity)retrievedResult.Result;

// Create the Delete TableOperation and then execute it.
if (deleteEntity != null)
{
   TableOperation deleteOperation = TableOperation.Delete(deleteEntity);

   // Execute the operation.
   await peopleTable.ExecuteAsync(deleteOperation);

   Console.WriteLine("Entity deleted.");
}

else
   Console.WriteLine("Couldn't delete the entity.");
```

## <a name="next-steps"></a>Volgende stappen
[!INCLUDE [vs-storage-dotnet-tables-next-steps](../../includes/vs-storage-dotnet-tables-next-steps.md)]
