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
ms.date: 12/02/2016
ms.author: kraigb
ms.openlocfilehash: 8d05fe3ed9a5c66f186a930d4107162c1f322c05
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-get-started-with-azure-table-storage-and-visual-studio-connected-services"></a>Hoe u aan de slag met Azure Table storage en Visual Studio verbonden services
[!INCLUDE [storage-try-azure-tools-tables](../../includes/storage-try-azure-tools-tables.md)]

## <a name="overview"></a>Overzicht
Dit artikel wordt beschreven hoe aan de slag met Azure Table storage in Visual Studio nadat u hebt gemaakt of een Azure storage-account in een ASP.NET Core-project waarnaar wordt verwezen door het gebruik van de Visual Studio **verbonden Services toevoegen** dialoogvenster.

De service Azure Table storage kunt u voor het opslaan van grote hoeveelheden gestructureerde gegevens. De service is een NoSQL-gegevensarchief die geverifieerde aanroepen vanuit binnen en buiten de Azure-cloud accepteert. Azure-tabellen zijn ideaal voor het opslaan van gestructureerde, niet-relationele gegevens.

De **verbonden Services toevoegen** bewerking installeert de juiste NuGet-pakketten voor toegang tot Azure-opslag in uw project en voegt u de verbindingsreeks voor de storage-account toe aan uw project configuratiebestanden.

Zie voor meer algemene informatie over het gebruik van Azure Table storage [aan de slag met Azure Table storage met .NET](../storage/storage-dotnet-how-to-use-tables.md).

Om te beginnen, moet u eerst een tabel maken in uw opslagaccount. Leert u hoe u een Azure-tabel in code maken. Ook leert u hoe basistabel en entiteit bewerkingen, zoals het toevoegen, wijzigen, lezen en lezen van tabelentiteiten uit te voeren. De voorbeelden zijn geschreven in C\# code en het gebruik van de Azure Storage-clientbibliotheek voor .NET.

**Opmerking** -sommige van de API's die het uitvoeren van aanroepen uit Azure storage in ASP.NET Core zijn asynchroon. Zie [asynchrone programmering met Async en Await](http://msdn.microsoft.com/library/hh191443.aspx) voor meer informatie. De volgende code wordt ervan uitgegaan programming Async-methoden worden gebruikt.

## <a name="access-tables-in-code"></a>Toegang tot tabellen in code
Voor toegang tot tabellen in ASP.NET Core projecten, moet u de volgende items naar de C# bronbestanden die toegang hebben tot Azure-tabelopslag bevatten.

1. Zorg ervoor dat de naamruimtedeclaraties boven aan het C#-bestand zijn deze **met** instructies.
   
        using Microsoft.Framework.Configuration;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Table;
        using System.Threading.Tasks;
        using LogLevel = Microsoft.Framework.Logging.LogLevel;
2. Ophalen van een **CloudStorageAccount** -object met gegevens over uw storage-account. De volgende code gebruiken om op te halen de uw verbindingsreeks voor opslag en opslag accountgegevens van de configuratie van Azure service.
   
        CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
            CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
   
    **Opmerking** -alle bovenstaande code voor de code in de volgende voorbeelden gebruiken.
3. Ophalen van een **CloudTableClient** object om te verwijzen naar de tabelobjecten in uw opslagaccount.  
   
        // Create the table client.
        CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
4. Ophalen van een **CloudTable** reference-object om te verwijzen naar een specifieke tabel en entiteiten.
   
        // Get a reference to a table named "peopleTable"
        CloudTable table = tableClient.GetTableReference("peopleTable");

## <a name="create-a-table-in-code"></a>Een tabel in code maken
Voor het maken van de Azure-tabel net Voeg een aanroep naar **CreateIfNotExistsAsync()**.

    // Create the CloudTable if it does not exist
    await table.CreateIfNotExistsAsync();

## <a name="add-an-entity-to-a-table"></a>Een entiteit toevoegen aan een tabel
Als u wilt een entiteit toevoegen aan een tabel kunt u een klasse die de eigenschappen van uw entiteit definieert maken. De volgende code definieert een entiteitsklasse aangeroepen **CustomerEntity** die de voornaam van de klant gebruikt als de rijsleutel en de achternaam als de partitiesleutel.

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

Tabelbewerkingen met betrekking tot entiteiten worden gedaan met behulp van de **CloudTable** object u eerder in 'Toegang tot tabellen in de code' gemaakt De **TableOperation** object vertegenwoordigt de bewerking worden uitgevoerd. Het volgende codevoorbeeld toont het maken van een **CloudTable** object en een **CustomerEntity** object. Voorbereiden van de bewerking een **TableOperation** voor het invoegen van de klantentiteit in de tabel is gemaakt. Ten slotte wordt de bewerking uitgevoerd door het aanroepen van CloudTable.ExecuteAsync.

    // Create a new customer entity.
    CustomerEntity customer1 = new CustomerEntity("Harp", "Walter");
    customer1.Email = "Walter@contoso.com";
    customer1.PhoneNumber = "425-555-0101";

    // Create the TableOperation that inserts the customer entity.
    TableOperation insertOperation = TableOperation.Insert(customer1);

    // Execute the insert operation.
    await peopleTable.ExecuteAsync(insertOperation);

## <a name="insert-a-batch-of-entities"></a>Een batch entiteiten invoegen
U kunt meerdere entiteiten invoegen in een tabel in een enkel schrijfbewerking. Het volgende codevoorbeeld maakt twee entiteitsobjecten ('Jeff Smith' en 'Ben Smith'), worden toegevoegd aan een **TableBatchOperation** object met de **invoegen** methode, en start vervolgens de bewerking door het aanroepen van CloudTable.ExecuteBatchAsync.

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

## <a name="get-all-of-the-entities-in-a-partition"></a>Alle entiteiten in een partitie ophalen
Gebruiken om te vragen een tabel voor alle entiteiten in een partitie, een **TableQuery** object. Het volgende codevoorbeeld geeft een filter voor entiteiten waarbij 'Smith' de partitiesleutel is. In dit voorbeeld worden de velden van elke entiteit in de queryresultaten naar de console afgedrukt.

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

## <a name="get-a-single-entity"></a>Één entiteit ophalen
U kunt een query als u één specifieke entiteit schrijven. De volgende code wordt een **TableOperation** object opgeven met de naam 'Ben Smith' van de klant. Deze methode retourneert één entiteit in plaats van een verzameling en de geretourneerde waarde in **TableResult.Result** is een **CustomerEntity** object. Het opgeven van de partitie-als rijsleutels in een query is de snelste manier om op te halen van één entiteit van de **tabel** service.

    // Create a retrieve operation that takes a customer entity.
    TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

    // Execute the retrieve operation.
    TableResult retrievedResult = await peopleTable.ExecuteAsync(retrieveOperation);

    // Print the phone number of the result.
    if (retrievedResult.Result != null)
       Console.WriteLine(((CustomerEntity)retrievedResult.Result).PhoneNumber);
    else
       Console.WriteLine("The phone number could not be retrieved.");

## <a name="delete-an-entity"></a>Een entiteit verwijderen
Nadat u deze hebt gevonden, kunt u een entiteit verwijderen. De volgende code wordt gezocht naar een klantentiteit met de naam 'Ben Smith' en als het wordt gevonden, wordt de snelkoppeling verwijderd.

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

## <a name="next-steps"></a>Volgende stappen
[!INCLUDE [vs-storage-dotnet-tables-next-steps](../../includes/vs-storage-dotnet-tables-next-steps.md)]

