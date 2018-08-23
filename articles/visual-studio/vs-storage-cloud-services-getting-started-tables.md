---
title: Aan de slag met table storage en Visual Studio verbonden services (cloudservices) | Microsoft Docs
description: Aan de slag met Azure Table storage in een cloud service-project in Visual Studio nadat u verbinding met een opslagaccount met behulp van Visual Studio verbonden services
services: storage
author: ghogen
manager: douge
ms.assetid: a3a11ed8-ba7f-4193-912b-e555f5b72184
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.custom: vs-azure
ms.workload: azure-vs
ms.topic: conceptual
ms.date: 12/02/2016
ms.author: ghogen
ms.openlocfilehash: f1e663463ff91da887a4afaebde4b2257347d4f4
ms.sourcegitcommit: 30c7f9994cf6fcdfb580616ea8d6d251364c0cd1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2018
ms.locfileid: "42054309"
---
# <a name="getting-started-with-azure-table-storage-and-visual-studio-connected-services-cloud-services-projects"></a>Aan de slag met Azure table storage en Visual Studio verbonden services (cloudserviceprojecten)
[!INCLUDE [storage-try-azure-tools-tables](../../includes/storage-try-azure-tools-tables.md)]

## <a name="overview"></a>Overzicht
In dit artikel wordt beschreven hoe u aan de slag met Azure-tabelopslag in Visual Studio nadat u hebt gemaakt of een Azure storage-account in een cloud services-project waarnaar wordt verwezen door het gebruik van de Visual Studio **Connected Services toevoegen** dialoogvenster. De **Connected Services toevoegen** bewerking installeert de juiste NuGet-pakketten voor toegang tot Azure storage in uw project en de verbindingsreeks voor het opslagaccount toegevoegd aan uw project-configuratiebestanden.

De Azure Table storage-service kunt u voor het opslaan van grote hoeveelheden gestructureerde gegevens. De service is een NoSQL-gegevensarchief die geverifieerde aanroepen uit binnen en buiten de Azure-cloud accepteert. Azure-tabellen zijn ideaal voor het opslaan van gestructureerde, niet-relationele gegevens.

Als u wilt beginnen, moet u eerst een tabel maken in uw storage-account. We laten u over het maken van een Azure-tabel in de code, en ook eenvoudige tabel en bewerkingen van de entiteit, zoals het toevoegen, wijzigen, lezen en lezen van de tabelentiteiten uitvoeren. De voorbeelden zijn geschreven in C\# code en gebruik de [Microsoft Azure Storage-clientbibliotheek voor .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx).

**Opmerking:** enkele van de API's die het uitvoeren van aanroepen uit naar Azure storage zijn asynchroon. Zie [asynchrone programmering met Async en Await](http://msdn.microsoft.com/library/hh191443.aspx) voor meer informatie. De onderstaande code wordt ervan uitgegaan dat asynchrone programmering methoden worden gebruikt.

* Zie [aan de slag met Azure Table storage met .NET](../storage/storage-dotnet-how-to-use-tables.md) voor meer informatie over het bewerken van programmatisch tabellen.
* Zie [documentatie voor Storage](https://azure.microsoft.com/documentation/services/storage/) voor algemene informatie over Azure Storage.
* Zie [documentatie voor Cloud Services](https://azure.microsoft.com/documentation/services/cloud-services/) voor algemene informatie over Azure-cloudservices.
* Zie [ASP.NET](http://www.asp.net) voor meer informatie over het programmeren van ASP.NET-toepassingen.

## <a name="access-tables-in-code"></a>Toegang tot tabellen in code
Voor toegang tot tabellen in de cloud service-projecten, moet u de volgende items naar de C# bronbestanden die toegang hebben tot Azure-tabelopslag opnemen.

1. Zorg ervoor dat de naamruimtedeclaraties boven aan het C#-bestand bevatten deze **met behulp van** instructies.
   
        using Microsoft.Framework.Configuration;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Table;
        using System.Threading.Tasks;
        using LogLevel = Microsoft.Framework.Logging.LogLevel;
2. Krijgen een **CloudStorageAccount** -object met gegevens van uw opslagaccount. Gebruik de volgende code om op te halen van de verbindingsreeks voor opslag en gegevens over het opslagaccount van de configuratie van de Azure-service.
   
         CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
           CloudConfigurationManager.GetSetting("<storage account name>
         _AzureStorageConnectionString"));
   > [!NOTE]
   > Alle bovenstaande code voor de code in de volgende voorbeelden gebruiken.
   > 
   > 
3. Krijgen een **CloudTableClient** object om te verwijzen naar de tabelobjecten in uw storage-account.
   
         // Create the table client.
         CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
4. Krijgen een **CloudTable** reference-object om te verwijzen naar een specifieke tabel- en entiteiten.
   
        // Get a reference to a table named "peopleTable".
        CloudTable peopleTable = tableClient.GetTableReference("peopleTable");

## <a name="create-a-table-in-code"></a>Een tabel in code maken
Uitbreiden voor het maken van de Azure-tabel, een aanroep van **CreateIfNotExistsAsync** naar de nadat u een **CloudTable** zoals beschreven in de sectie "Toegang tot tabellen in de code"-object.

    // Create the CloudTable if it does not exist.
    await peopleTable.CreateIfNotExistsAsync();

## <a name="add-an-entity-to-a-table"></a>Een entiteit toevoegen aan een tabel
Als u een entiteit wilt toevoegen aan een tabel, maakt u een klasse die de eigenschappen van uw entiteit definieert. De volgende code definieert een entiteitsklasse die met de naam **CustomerEntity** die de voornaam van de klant gebruikt als de rijsleutel en de achternaam als de partitiesleutel.

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

Table-bewerkingen met betrekking tot entiteiten worden gedaan met behulp van de **CloudTable** -object dat u eerder in 'Toegang tot tabellen in de code.' gemaakt De **TableOperation** object vertegenwoordigt de bewerking moet worden uitgevoerd. Het volgende codevoorbeeld toont het maken van een **CloudTable** object en een **CustomerEntity** object. Voor het voorbereiden van de bewerking wordt een **TableOperation** voor het invoegen van de klantentiteit in de tabel is gemaakt. Ten slotte wordt de bewerking is uitgevoerd door het aanroepen van **CloudTable.ExecuteAsync**.

    // Create a new customer entity.
    CustomerEntity customer1 = new CustomerEntity("Harp", "Walter");
    customer1.Email = "Walter@contoso.com";
    customer1.PhoneNumber = "425-555-0101";

    // Create the TableOperation that inserts the customer entity.
    TableOperation insertOperation = TableOperation.Insert(customer1);

    // Execute the insert operation.
    await peopleTable.ExecuteAsync(insertOperation);


## <a name="insert-a-batch-of-entities"></a>Een batch entiteiten invoegen
U kunt meerdere entiteiten invoegen in een tabel in een enkele schrijfbewerking. Het volgende codevoorbeeld maakt twee entiteitsobjecten ('Jeff Smith' en 'Ben Smith'), voegt deze toe aan een **TableBatchOperation** -object met behulp van de methode invoegen en start vervolgens de bewerking door het aanroepen van  **CloudTable.ExecuteBatchAsync**.

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
Gebruiken om een tabel voor alle entiteiten in een partitie, een **TableQuery** object. Het volgende codevoorbeeld geeft een filter voor entiteiten waarbij 'Smith' de partitiesleutel is. In dit voorbeeld worden de velden van elke entiteit in de queryresultaten naar de console afgedrukt.

    // Construct the query operation for all customer entities where PartitionKey="Smith".
    TableQuery<CustomerEntity> query = new TableQuery<CustomerEntity>()
        .Where(TableQuery.GenerateFilterCondition("PartitionKey", QueryComparisons.Equal, "Smith"));

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

    return View();


## <a name="get-a-single-entity"></a>Één entiteit ophalen
U kunt een query voor het ophalen van één specifieke entiteit schrijven. De volgende code gebruikt een **TableOperation** object om een klant 'Ben Smith' met de naam te geven. Deze methode retourneert één entiteit in plaats van een verzameling en de geretourneerde waarde in **TableResult.Result** is een **CustomerEntity** object. Partitie-en recordsleutels op te geven in een query is de snelste manier om op te halen van een enkele entiteit op basis van de **tabel** service.

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
U kunt een entiteit verwijderen nadat u deze vinden. De volgende code ziet er uit voor een klantentiteit met de naam 'Ben Smith' en als deze wordt gevonden, wordt deze verwijderd.

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

