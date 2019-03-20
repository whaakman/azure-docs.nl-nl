---
title: Aan de slag met Azure Cosmos DB Table-API met behulp van .NET Standard-SDK
description: Store gestructureerde gegevens in de cloud met behulp van de Azure Cosmos DB Table-API.
author: wmengmsft
ms.author: wmeng
ms.service: cosmos-db
ms.subservice: cosmosdb-table
ms.devlang: dotnet
ms.topic: sample
ms.date: 03/11/2019
ms.openlocfilehash: 0f324d39db38b17d436583277d60d87b2878d131
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57880786"
---
# <a name="get-started-with-azure-cosmos-db-table-api-and-azure-table-storage-using-the-net-sdk"></a>Aan de slag met Azure Cosmos DB Table-API en Azure Table storage met behulp van de .NET SDK

[!INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]

[!INCLUDE [storage-table-applies-to-storagetable-and-cosmos](../../includes/storage-table-applies-to-storagetable-and-cosmos.md)]

U kunt de Azure Cosmos DB Table-API of Azure Table storage gebruiken voor het opslaan van gestructureerde op NoSQL gegevens in de cloud, zodat een sleutelkenmerk opslaan met een schema minder ontwerp. Omdat Azure Cosmos DB Table-API-en tabelopslag schema kleiner zijn, is het uw kunt gegevens eenvoudig aanpassen naarmate de behoeften van uw toepassing veranderen. U kunt Azure Cosmos DB Table-API of de Table storage gebruiken voor het opslaan van flexibele gegevenssets, zoals gebruikersgegevens voor webtoepassingen, adresboeken, apparaatgegevens of andere soorten metagegevens die uw service nodig heeft. 

Deze zelfstudie wordt beschreven voor een voorbeeld waarin wordt uitgelegd hoe u de [Microsoft Azure Cosmos DB tabel-bibliotheek voor .NET](https://www.nuget.org/packages/Microsoft.Azure.Cosmos.Table) met Azure Cosmo DB Table-API en Azure Table storage-scenario's. U moet de verbinding die specifiek zijn voor de Azure-service. Deze scenario's worden verkend met behulp van C# voorbeelden die laten zien hoe u het maken van tabellen, invoegen / bijwerken van gegevens, gegevens op te vragen en het verwijderen van tabellen.

## <a name="prerequisites"></a>Vereisten

U hebt het volgende nodig voor dit voorbeeld:

* [Microsoft Visual Studio](https://www.visualstudio.com/downloads/)

* [Microsoft Azure cosmos DB tabel-bibliotheek voor .NET](https://www.nuget.org/packages/Microsoft.Azure.Cosmos.Table) -deze bibliotheek is momenteel beschikbaar voor .NET Standard- en .NET framework. 

* [Azure Cosmos DB Table-API-account](create-table-dotnet.md#create-a-database-account).

## <a name="create-an-azure-cosmos-db-table-api-account"></a>Een Azure Cosmos DB Table-API-account maken

[!INCLUDE [cosmos-db-create-dbaccount-table](../../includes/cosmos-db-create-dbaccount-table.md)]

## <a name="create-a-net-console-project"></a>Een .NET-console-project maken

Maak een nieuwe .NET-consoletoepassing in Visual Studio. In de volgende stappen ziet u hoe u een consoletoepassing maakt in Visual Studio 2017. De stappen zijn nagenoeg gelijk in andere versies van Visual Studio. U kunt de Azure Cosmos DB tabel-bibliotheek in elk type .NET-toepassing, met inbegrip van een Azure-cloud-service of web-app, en desktop- en mobiele toepassingen gebruiken. In deze gids gebruiken we een consoletoepassing voor de eenvoud.

1. Selecteer **Bestand** > **Nieuw** > **Project**.

1. Selecteer **geïnstalleerd** > **Visual C#**   >  **consoletoepassing (.NET Core)**.

1. In de **naam** veld, voer een naam voor uw toepassing, bijvoorbeeld: **CosmosTableSamples** (u kunt een andere naam opgeven indien nodig).

1. Selecteer **OK**.

Alle codevoorbeelden in dit voorbeeld kunnen worden toegevoegd aan de methode Main() van uw consoletoepassing **Program.cs** bestand.

## <a name="install-the-required-nuget-package"></a>Installeer de vereiste NuGet-pakket

Als u het NuGet-pakket, de volgende stappen uit:

1. Klik met de rechtermuisknop op het project in **Solution Explorer** en kies **NuGet-pakketten beheren**.

1. Zoek online naar `Microsoft.Azure.Cosmos.Table`, `Microsoft.Extensions.Configuration`, `Microsoft.Extensions.Configuration.Json`, `Microsoft.Extensions.Configuration.Binder` en selecteer **installeren** voor het installeren van de Microsoft Azure Cosmos DB tabel-bibliotheek.

## <a name="configure-your-storage-connection-string"></a>De opslagverbindingsreeks configureren

1. Uit de [Azure-portal](https://portal.azure.com/), klikt u op **Connection String**. Gebruik de kopieerknop aan de rechterkant van het venster om de **PRIMARY CONNECTION STRING** te kopiëren.

   ![De PRIMARY CONNECTION STRING in het deelvenster Verbindingsreeks weergeven en kopiëren](./media/create-table-dotnet/connection-string.png)
   
1. Voor het configureren van de verbindingsreeks vanuit visual studio Klik met de rechtermuisknop op uw project **CosmosTableSamples**.

1. Selecteer **toevoegen** en vervolgens **Nieuw Item**. Maak een nieuw bestand **Settings.json** met bestandstype als **TypeScript JSON configuratie** bestand. 

1. Vervang de code in het bestand Settings.json door de volgende code en toewijzen van de primaire verbindingsreeks:

   ```csharp
   {
   "StorageConnectionString": <Primary connection string of your Azure Cosmos DB account>
   }
   ```

1. Klik met de rechtermuisknop op uw project **CosmosTableSamples**. Selecteer **toevoegen**, **Nieuw Item** en toevoegen van een klasse met de naam **AppSettings.cs**.

1. Voeg de volgende code naar het bestand AppSettings.cs. Dit bestand wordt de verbindingsreeks uit het bestand Settings.json gelezen en toegewezen aan de configuratieparameter:

   ```csharp
   namespace CosmosTableSamples
   {
    using Microsoft.Extensions.Configuration;
    public class AppSettings
    {
        public string StorageConnectionString { get; set; }
        public static AppSettings LoadAppSettings()
        {
            IConfigurationRoot configRoot = new ConfigurationBuilder()
                .AddJsonFile("Settings.json")
                .Build();
            AppSettings appSettings = configRoot.Get<AppSettings>();
            return appSettings;
        }
    }
   }
   ```

## <a name="parse-and-validate-the-connection-details"></a>Parseren en de gegevens van de verbinding valideren 

1. Klik met de rechtermuisknop op uw project **CosmosTableSamples**. Selecteer **toevoegen**, **Nieuw Item** en toevoegen van een klasse met de naam **Common.cs**. Schrijft u code te valideren van gegevens van de verbinding en een tabel in deze klasse te maken.

1. Een methode definiëren `CreateStorageAccountFromConnectionString` zoals hieronder wordt weergegeven. Deze methode worden de details van de verbindingsreeks te parseren en valideren of de accountnaam en het account belangrijke informatie in het bestand "Settings.json" geldig zijn. 

   ```csharp
   public static CloudStorageAccount CreateStorageAccountFromConnectionString(string storageConnectionString)
    {
            CloudStorageAccount storageAccount;
            try
            {
                storageAccount = CloudStorageAccount.Parse(storageConnectionString);
            }
            catch (FormatException)
            {
                Console.WriteLine("Invalid storage account information provided. Please confirm the AccountName and AccountKey are valid in the app.config file - then restart the application.");
                throw;
            }
            catch (ArgumentException)
            {
                Console.WriteLine("Invalid storage account information provided. Please confirm the AccountName and AccountKey are valid in the app.config file - then restart the sample.");
                Console.ReadLine();
                throw;
            }

            return storageAccount;
        }
   ```


## <a name="create-a-table"></a>Een tabel maken 

Met behulp van de [CloudTableClient](https://docs.microsoft.com/dotnet/api/microsoft.windowsazure.storage.table.cloudtableclient?redirectedfrom=MSDN&view=azure-dotnet)-klasse kunt u tabellen en entiteiten ophalen die zijn opgeslagen in de Table Storage. Omdat we niet alle tabellen in de Cosmos DB Table-API-account hebt, gaan we toevoegen de `CreateTableAsync` methode de **Common.cs** klasse om een tabel te maken:

```csharp
public static async Task<CloudTable> CreateTableAsync(string tableName)
  {
    string storageConnectionString = AppSettings.LoadAppSettings().StorageConnectionString;

    // Retrieve storage account information from connection string.
    CloudStorageAccount storageAccount = CreateStorageAccountFromConnectionString(storageConnectionString);

    // Create a table client for interacting with the table service
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient(new TableClientConfiguration());

    Console.WriteLine("Create a Table for the demo");

    // Create a table client for interacting with the table service 
    CloudTable table = tableClient.GetTableReference(tableName);
    if (await table.CreateIfNotExistsAsync())
    {
      Console.WriteLine("Created Table named: {0}", tableName);
    }
    else
    {
      Console.WriteLine("Table {0} already exists", tableName);
    }

    Console.WriteLine();
    return table;
}
```

## <a name="define-the-entity"></a>De entiteit definiëren 

Entiteiten worden toegewezen aan C# objecten met behulp van een aangepaste klasse die is afgeleid van [TableEntity](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.table.tableentity.aspx). Als u een entiteit wilt toevoegen aan een tabel, maakt u een klasse die de eigenschappen van uw entiteit definieert.

Klik met de rechtermuisknop op uw project **CosmosTableSamples**. Selecteer **toevoegen**, **nieuwe map** en noem deze **Model**. Voeg een klasse met de naam in de map Model **CustimerEntity.cs** en voeg de volgende code eraan toe.

```csharp
namespace CosmosTableSamples.Model
{
    using Microsoft.Azure.Cosmos.Table;
    public class CustomerEntity : TableEntity
    {
        public CustomerEntity()
        {
        }

        public CustomerEntity(string lastName, string firstName)
        {
            PartitionKey = lastName;
            RowKey = firstName;
        }

        public string Email { get; set; }
        public string PhoneNumber { get; set; }
    }
}
```

Deze code definieert een entiteitsklasse die gebruikmaakt van de voornaam van de klant als de rijsleutel en de achternaam als de partitiesleutel. De entiteit wordt in de tabel op unieke wijze geïdentificeerd door diens partitie- en rijsleutel. Entiteiten met dezelfde partitiesleutel kunnen sneller worden opgevraagd dan entiteiten met verschillende partitiesleutels, maar gebruik van verschillende partitiesleutels maakt een grotere schaalbaarheid van parallelle bewerkingen. Entiteiten worden opgeslagen in tabellen moeten zijn van een ondersteund type, bijvoorbeeld zijn afgeleid van de [TableEntity](https://docs.microsoft.com/dotnet/api/microsoft.windowsazure.storage.table.tableentity?redirectedfrom=MSDN&view=azure-dotnet) klasse. De entiteitseigenschappen die u in een tabel wilt opslaan, moeten openbare eigenschappen van het type zijn. Bovendien moeten ze zowel het ophalen als het instellen van waarden ondersteunen. Bovendien moet uw entiteitstype beschikbaar maken een parameterloze constructor bevatten.

## <a name="insert-or-merge-an-entity"></a>Invoegen of samenvoegen van een entiteit

Het volgende codevoorbeeld maakt een entiteitsobject en toegevoegd aan de tabel. De methode InsertOrMerge binnen de [TableOperation](https://docs.microsoft.com/dotnet/api/microsoft.windowsazure.storage.table.tableoperation?redirectedfrom=MSDN&view=azure-dotnet) klasse wordt gebruikt voor het invoegen of een entiteit samenvoegt. De [CloudTable.ExecuteAsync](https://docs.microsoft.com/dotnet/api/microsoft.windowsazure.storage.table.cloudtable.executeasync?view=azure-dotnet) methode wordt aangeroepen voor het uitvoeren van de bewerking. 

Klik met de rechtermuisknop op uw project **CosmosTableSamples**. Selecteer **toevoegen**, **Nieuw Item** en toevoegen van een klasse met de naam **SamplesUtils.cs**. Deze klasse bevat de code die zijn vereist om uit te voeren CRUD-bewerkingen op de entiteiten. 

```csharp
public static async Task<CustomerEntity> InsertOrMergeEntityAsync(CloudTable table, CustomerEntity entity)
    {
      if (entity == null)
    {
       throw new ArgumentNullException("entity");
    }
    try
    {
       // Create the InsertOrReplace table operation
       TableOperation insertOrMergeOperation = TableOperation.InsertOrMerge(entity);

       // Execute the operation.
       TableResult result = await table.ExecuteAsync(insertOrMergeOperation);
       CustomerEntity insertedCustomer = result.Result as CustomerEntity;
        
        // Get the request units consumed by the current operation. RequestCharge of a TableResult is only applied to Azure CosmoS DB 
        if (result.RequestCharge.HasValue)
          {
            Console.WriteLine("Request Charge of InsertOrMerge Operation: " + result.RequestCharge);
          }

        return insertedCustomer;
        }
        catch (StorageException e)
        {
          Console.WriteLine(e.Message);
          Console.ReadLine();
          throw;
        }
    }
```

### <a name="get-an-entity-from-a-partition"></a>Een entiteit uit een partitie ophalen

U kunt verkrijgen van de entiteit van een partitie met behulp van de methode ophalen onder de [TableOperation](https://docs.microsoft.com/dotnet/api/microsoft.windowsazure.storage.table.tableoperation?redirectedfrom=MSDN&view=azure-dotnet) klasse. Het volgende codevoorbeeld haalt de partitiesleutel sleutelrij, e-mailadres en telefoonnummer van een klantentiteit. In dit voorbeeld worden ook de basis van aanvraageenheden gebruikt om op te vragen de entiteit afgedrukt. Om te vragen voor een entiteit, toevoegen de volgende code om **SamplesUtils.cs** bestand: 

```csharp
public static async Task<CustomerEntity> RetrieveEntityUsingPointQueryAsync(CloudTable table, string partitionKey, string rowKey)
    {
      try
      {
        TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>(partitionKey, rowKey);
        TableResult result = await table.ExecuteAsync(retrieveOperation);
        CustomerEntity customer = result.Result as CustomerEntity;
        if (customer != null)
        {
          Console.WriteLine("\t{0}\t{1}\t{2}\t{3}", customer.PartitionKey, customer.RowKey, customer.Email, customer.PhoneNumber);
        }

        // Get the request units consumed by the current operation. RequestCharge of a TableResult is only applied to Azure CosmoS DB 
        if (result.RequestCharge.HasValue)
        {
           Console.WriteLine("Request Charge of Retrieve Operation: " + result.RequestCharge);
        }

        return customer;
        }
        catch (StorageException e)
        {
           Console.WriteLine(e.Message);
           Console.ReadLine();
           throw;
        }
    }
```

## <a name="delete-an-entity"></a>Een entiteit verwijderen

U kunt een entiteit gemakkelijk verwijderen nadat u deze hebt opgehaald. Hiervoor gebruikt u hetzelfde patroon dat is getoond voor het bijwerken van een entiteit. Met de volgende code wordt een klantentiteit opgehaald en verwijderd. Als u wilt verwijderen van een entiteit, toevoegen de volgende code om **SamplesUtils.cs** bestand: 

```csharp
public static async Task DeleteEntityAsync(CloudTable table, CustomerEntity deleteEntity)
   {
     try
     {
        if (deleteEntity == null)
     {
        throw new ArgumentNullException("deleteEntity");
     }

    TableOperation deleteOperation = TableOperation.Delete(deleteEntity);
    TableResult result = await table.ExecuteAsync(deleteOperation);

    // Get the request units consumed by the current operation. RequestCharge of a TableResult is only applied to Azure CosmoS DB 
    if (result.RequestCharge.HasValue)
    {
       Console.WriteLine("Request Charge of Delete Operation: " + result.RequestCharge);
    }

    }
    catch (StorageException e)
    {
        Console.WriteLine(e.Message);
        Console.ReadLine();
        throw;
    }
}
```

## <a name="execute-the-crud-operations-on-sample-data"></a>Uitvoeren van de CRUD-bewerkingen op voorbeeldgegevens

Nadat u de methoden voor het maken van de tabel, insert nebo samenvoegen entiteiten definiëren, voert u deze methoden op de voorbeeldgegevens. Om dit te doen, klik met de rechtermuisknop op uw project **CosmosTableSamples**. Selecteer **toevoegen**, **Nieuw Item** en toevoegen van een klasse met de naam **BasicSamples.cs** en voeg de volgende code eraan toe. Deze code maakt een tabel, entiteiten toevoegt aan het. Als u wilt verwijderen van de entiteit en de tabel aan het einde van het project verwijdert u eventuele opmerkingen van `table.DeleteIfExistsAsync()` en `SamplesUtils.DeleteEntityAsync(table, customer)` methoden van de volgende code:

```csharp
using System;
namespace CosmosTableSamples
{
    using System.Threading.Tasks;
    using Microsoft.Azure.Cosmos.Table;
    using Model;

    class BasicSamples
    {
        public async Task RunSamples()
        {
            Console.WriteLine("Azure Cosmos DB Table - Basic Samples\n");
            Console.WriteLine();

            string tableName = "demo" + Guid.NewGuid().ToString().Substring(0, 5);

            // Create or reference an existing table
            CloudTable table = await Common.CreateTableAsync(tableName);

            try
            {
                // Demonstrate basic CRUD functionality 
                await BasicDataOperationsAsync(table);
            }
            finally
            {
                // Delete the table
                // await table.DeleteIfExistsAsync();
            }
        }

        private static async Task BasicDataOperationsAsync(CloudTable table)
        {
            // Create an instance of a customer entity. See the Model\CustomerEntity.cs for a description of the entity.
            CustomerEntity customer = new CustomerEntity("Harp", "Walter")
            {
                Email = "Walter@contoso.com",
                PhoneNumber = "425-555-0101"
            };

            // Demonstrate how to insert the entity
            Console.WriteLine("Insert an Entity.");
            customer = await SamplesUtils.InsertOrMergeEntityAsync(table, customer);

            // Demonstrate how to Update the entity by changing the phone number
            Console.WriteLine("Update an existing Entity using the InsertOrMerge Upsert Operation.");
            customer.PhoneNumber = "425-555-0105";
            await SamplesUtils.InsertOrMergeEntityAsync(table, customer);
            Console.WriteLine();

            // Demonstrate how to Read the updated entity using a point query 
            Console.WriteLine("Reading the updated Entity.");
            customer = await SamplesUtils.RetrieveEntityUsingPointQueryAsync(table, "Harp", "Walter");
            Console.WriteLine();

            // Demonstrate how to Delete an entity
            //Console.WriteLine("Delete the entity. ");
            //await SamplesUtils.DeleteEntityAsync(table, customer);
            //Console.WriteLine();
        }
    }
}
```

De vorige code maakt een tabel die met 'demo begint' en de gegenereerde GUID wordt toegevoegd aan de naam van de tabel. Er wordt een klantentiteit met eerste en laatste naam als 'Walter Harp' toegevoegd en later werkt het telefoonnummer van deze gebruiker. 

In deze zelfstudie hebt u code voor het uitvoeren van eenvoudige CRUD-bewerkingen op de gegevens die zijn opgeslagen in Table-API-account gemaakt. U kunt ook geavanceerde bewerkingen zoals uitvoeren – invoegen batchgegevens, query een scala aan gegevens binnen een partitie, een lijst met tabellen in het account waarvan de namen met het opgegeven voorvoegsel beginnen query uitvoeren op de gegevens binnen een partitie. U kunt het formulier volledig voorbeeld downloaden [azure-cosmos-table-dotnet-core-getting-started](https://github.com/Azure-Samples/azure-cosmos-table-dotnet-core-getting-started) GitHub-opslagplaats. De [AdvancedSamples.cs](https://github.com/Azure-Samples/azure-cosmos-table-dotnet-core-getting-started/blob/master/CosmosTableSamples/AdvancedSamples.cs) klasse heeft andere bewerkingen die u op de gegevens uitvoeren kunt.  

## <a name="run-the-project"></a>Het project uitvoeren

Nu de oplossing te bouwen en druk op F5 om uit te voeren van het project. Wanneer het project wordt uitgevoerd, ziet u de volgende uitvoer in de opdrachtprompt:

![Uitvoer van de opdrachtprompt](./media/tutorial-develop-table-standard/output-from-sample.png)

Als u een foutbericht dat bestand Settings.json kan niet worden gevonden ontvangt bij het uitvoeren van het project, kunt u deze kunt oplossen door de volgende XML-vermelding toe te voegen aan de instellingen van het project. Klik met de rechtermuisknop op CosmosTableSamples, CosmosTableSamples.csproj bewerken selecteren en toevoegen van de volgende itemGroup: 

```csharp
  <ItemGroup>
    <None Update="Settings.json">
      <CopyToOutputDirectory>PreserveNewest</CopyToOutputDirectory>
    </None>
  </ItemGroup>
```
U kunt nu Meld u aan bij de Azure-portal en controleer of de gegevens in de tabel bestaat. 

![Resultaten in de portal](./media/tutorial-develop-table-standard/results-in-portal.png)

## <a name="next-steps"></a>Volgende stappen

U kunt nu doorgaan met de volgende zelfstudie en meer informatie over het migreren van gegevens naar Azure Cosmos DB Table-API-account. 

> [!div class="nextstepaction"]
>[Hoe u gegevens op te vragen](../cosmos-db/table-import.md)
