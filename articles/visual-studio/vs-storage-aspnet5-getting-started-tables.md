---
title: Aan de slag met table storage en Visual Studio verbonden services (ASP.NET Core) | Microsoft Docs
description: Aan de slag met Azure Table storage in een ASP.NET Core-project in Visual Studio nadat u verbinding met een opslagaccount met behulp van Visual Studio verbonden services
services: storage
author: ghogen
manager: douge
ms.assetid: c3c451d1-71ff-4222-a348-c41c98a02b85
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.custom: vs-azure
ms.workload: azure-vs
ms.topic: conceptual
ms.date: 11/14/2017
ms.author: ghogen
ms.openlocfilehash: 1f90ce71084ba3acbf5a0aec5c7b8e9683323766
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58011619"
---
# <a name="how-to-get-started-with-azure-table-storage-and-visual-studio-connected-services"></a>Aan de slag met Azure Table storage en Visual Studio verbonden services

[!INCLUDE [storage-try-azure-tools-tables](../../includes/storage-try-azure-tools-tables.md)]

In dit artikel wordt beschreven hoe u aan de slag met Azure Table storage in Visual Studio nadat u hebt gemaakt of een Azure storage-account in een ASP.NET Core-project waarnaar wordt verwezen door het gebruik van de Visual Studio **Connected Services** functie. De **Connected Services** bewerking installeert de juiste NuGet-pakketten voor toegang tot Azure storage in uw project en de verbindingsreeks voor het opslagaccount toegevoegd aan uw project-configuratiebestanden. (Zie [documentatie voor Storage](https://azure.microsoft.com/documentation/services/storage/) voor algemene informatie over Azure Storage.)

De Azure Table storage-service kunt u voor het opslaan van grote hoeveelheden gestructureerde gegevens. De service is een NoSQL-gegevensarchief die geverifieerde aanroepen uit binnen en buiten de Azure-cloud accepteert. Azure-tabellen zijn ideaal voor het opslaan van gestructureerde, niet-relationele gegevens. Zie voor meer algemene informatie over het gebruik van Azure Table storage [aan de slag met Azure Table storage met .NET](../storage/storage-dotnet-how-to-use-tables.md).

Als u wilt beginnen, moet u eerst een tabel maken in uw storage-account. In dit artikel wordt weergegeven over het maken van een tabel in C# en hoe u eenvoudige tabelbewerkingen zoals het toevoegen, wijzigen, lezen en verwijderen van vermeldingen uit te voeren.  De code gebruikt de Azure Storage-clientbibliotheek voor .NET. Zie voor meer informatie over ASP.NET [ASP.NET](https://www.asp.net).

Enkele van de Azure Storage-API's zijn asynchroon en de code in dit artikel wordt ervan uitgegaan dat async-methoden worden gebruikt. Zie [asynchrone programmering](https://docs.microsoft.com/dotnet/csharp/async) voor meer informatie.

## <a name="access-tables-in-code"></a>Toegang tot tabellen in code

Voor toegang tot tabellen in ASP.NET Core-projecten, moet u de volgende items naar de C# bronbestanden die toegang hebben tot Azure-tabelopslag opnemen.

1. Toevoegen van de benodigde `using` instructies:

    ```csharp
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Table;
    using System.Threading.Tasks;
    ```

1. Krijgen een `CloudStorageAccount` -object met gegevens van uw opslagaccount. Gebruik de volgende code, met de naam van uw opslagaccount en de accountsleutel, die u in de verbindingsreeks voor opslag in appSettings.json vinden kunt:

    ```csharp
        CloudStorageAccount storageAccount = new CloudStorageAccount(
            new Microsoft.WindowsAzure.Storage.Auth.StorageCredentials(
                "<name>", "<account-key>"), true);
    ```

1. Krijgen een `CloudTableClient` object om te verwijzen naar de tabelobjecten in uw storage-account:

    ```csharp
    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
    ```

1. Krijgen een `CloudTable` reference-object om te verwijzen naar een specifieke tabel- en entiteiten:

    ```csharp
    // Get a reference to a table named "peopleTable"
    CloudTable peopleTable = tableClient.GetTableReference("peopleTable");
    ```

## <a name="create-a-table-in-code"></a>Een tabel in code maken

Voor het maken van de Azure-tabel maken van een asynchrone methode en roep locatiemogelijkheid `CreateIfNotExistsAsync()`:

```csharp
async void CreatePeopleTableAsync()
{
    // Create the CloudTable if it does not exist
    await peopleTable.CreateIfNotExistsAsync();
}
```
    
## <a name="add-an-entity-to-a-table"></a>Een entiteit toevoegen aan een tabel

Als u wilt een entiteit toevoegen aan een tabel, maakt u een klasse die de eigenschappen van uw entiteit definieert. De volgende code definieert een entiteitsklasse die met de naam `CustomerEntity` die de voornaam van de klant als de rijsleutel en de achternaam als de partitiesleutel gebruikt.

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

Tabelbewerkingen entiteiten gebruiken met betrekking tot de `CloudTable` object u eerder hebt gemaakt in [toegang tot tabellen in de code](#access-tables-in-code). De `TableOperation` object vertegenwoordigt de bewerking moet worden uitgevoerd. Het volgende codevoorbeeld toont het maken van een `CloudTable` object en een `CustomerEntity` object. Voor het voorbereiden van de bewerking wordt een `TableOperation` voor het invoegen van de klantentiteit in de tabel is gemaakt. Ten slotte wordt de bewerking is uitgevoerd door het aanroepen van `CloudTable.ExecuteAsync`.

```csharp
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

U kunt meerdere entiteiten invoegen in een tabel in een enkele schrijfbewerking. Het volgende codevoorbeeld maakt twee entiteitsobjecten ('Jeff Smith' en 'Ben Smith'), voegt deze toe aan een `TableBatchOperation` object met de `Insert` methode, en start de bewerking door het aanroepen van `CloudTable.ExecuteBatchAsync`.

```csharp
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

Gebruiken om een tabel voor alle entiteiten in een partitie, een `TableQuery` object. Het volgende codevoorbeeld geeft een filter voor entiteiten waarbij 'Smith' de partitiesleutel is. In dit voorbeeld worden de velden van elke entiteit in de queryresultaten naar de console afgedrukt.

```csharp
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

U kunt een query voor het ophalen van één specifieke entiteit schrijven. De volgende code gebruikt een `TableOperation` object om een klant 'Ben Smith' met de naam te geven. De methode retourneert één entiteit in plaats van een verzameling en de geretourneerde waarde in `TableResult.Result` is een `CustomerEntity` object. Partitie-en recordsleutels op te geven in een query is de snelste manier om op te halen van een enkele entiteit op basis van de `Table` service.

```csharp
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

U kunt een entiteit verwijderen nadat u deze vinden. De volgende code zoekt en verwijdert een klantentiteit met de naam 'Ben Smith':

```csharp
// Create a retrieve operation that expects a customer entity.
TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

// Execute the operation.
TableResult retrievedResult = await peopleTable.ExecuteAsync(retrieveOperation);

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
