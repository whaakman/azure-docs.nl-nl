---
title: 'Zelfstudie: een pijplijn maken met Copy Activity in .NET API | Microsoft Docs'
description: In deze zelfstudie maakt u een Azure Data Factory-pijplijn met Copy Activity. Hiervoor gebruikt u .NET API.
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: 58fc4007-b46d-4c8e-a279-cb9e479b3e2b
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 10/27/2016
ms.author: spelluru
translationtype: Human Translation
ms.sourcegitcommit: d175d3d4d7d7a58f071dab0f32e3fdd3cb3146ce
ms.openlocfilehash: 8c26d8ef39827cff87b7fc7f17fab78c627a5035


---
# <a name="tutorial-create-a-pipeline-with-copy-activity-using-net-api"></a>Zelfstudie: een pijplijn maken met de kopieeractiviteit in .NET API
> [!div class="op_single_selector"]
> * [Overzicht en vereisten](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
> * [De wizard Kopiëren](data-factory-copy-data-wizard-tutorial.md)
> * [Azure Portal](data-factory-copy-activity-tutorial-using-azure-portal.md)
> * [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md)
> * [PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)
> * [Azure Resource Manager-sjabloon](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
> * [REST API](data-factory-copy-activity-tutorial-using-rest-api.md)
> * [.NET API](data-factory-copy-activity-tutorial-using-dotnet-api.md)

In deze zelfstudie ziet u hoe u een Azure-gegevensfactory maakt en bewaakt met een .NET API. In de pijplijn in de gegevensfactory wordt gebruikgemaakt van Copy Activity om gegevens van Azure-blobopslag te kopiëren naar Azure SQL Database.

Met Copy Activity wordt de gegevensverplaatsing in Azure Data Factory uitgevoerd. De activiteit wordt mogelijk gemaakt door een wereldwijd beschikbare service waarmee gegevens veilig, betrouwbaar en schaalbaar kunnen worden gekopieerd tussen verschillende gegevensarchieven. Zie [Activiteiten voor gegevensverplaatsing](data-factory-data-movement-activities.md) voor meer informatie over Copy Activity.

> [!NOTE]
> Dit artikel behandelt niet de volledige Data Factory .NET API. Zie [Data Factory .NET API-referentie](https://msdn.microsoft.com/library/mt415893.aspx) voor meer informatie over de Data Factory .NET SDK.

## <a name="prerequisites"></a>Vereisten
* Neem [Overzicht van de zelfstudie en vereisten](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) door voor een overzicht van de zelfstudie en voer de **vereiste** stappen uit.
* Visual Studio 2012 of 2013 of 2015
* Download en installeer [Azure .NET SDK](http://azure.microsoft.com/downloads/)
* Azure PowerShell. Volg de instructies in [Azure PowerShell installeren en configureren](/powershell/azureps-cmdlets-docs) om Azure PowerShell te installeren op uw computer. Azure PowerShell wordt gebruikt om een Azure Active Directory-toepassing te maken.

### <a name="create-an-application-in-azure-active-directory"></a>Een toepassing maken in Azure Active Directory
Maak een Azure Active Directory-toepassing, maak een service-principal voor de toepassing en wijs deze toe aan de rol **Inzender Data Factory**.

1. Start **PowerShell**.
2. Voer de volgende opdracht uit en geef de gebruikersnaam en het wachtwoord op waarmee u zich aanmeldt bij Azure Portal.

    ```PowerShell
    Login-AzureRmAccount
    ```
3. Voer de volgende opdracht uit om alle abonnementen voor dit account weer te geven.

    ```PowerShell
    Get-AzureRmSubscription
    ```
4. Voer de volgende opdracht uit om het abonnement te selecteren waarmee u wilt werken. Vervang **&lt;NameOfAzureSubscription**&gt; door de naam van uw Azure-abonnement.

    ```PowerShell
    Get-AzureRmSubscription -SubscriptionName <NameOfAzureSubscription> | Set-AzureRmContext
    ```

   > [!IMPORTANT]
   > Noteer de **SubscriptionId** en de **TenantId** uit de uitvoer van deze opdracht.

5. Maak een Azure-resourcegroep met de naam **ADFTutorialResourceGroup** door de volgende opdracht uit te voeren in PowerShell.

    ```PowerShell
    New-AzureRmResourceGroup -Name ADFTutorialResourceGroup  -Location "West US"
    ```

    Als de resourcegroep al bestaat, geeft u aan of u deze wilt bijwerken (Y) of ongewijzigd wilt laten (N).

    Als u een andere resourcegroep gebruikt, moet u voor deze zelfstudie de naam van uw resourcegroep gebruiken in plaats van ADFTutorialResourceGroup.
6. Maak een Azure Active Directory-toepassing.

    ```PowerShell
    $azureAdApplication = New-AzureRmADApplication -DisplayName "ADFCopyTutotiralApp" -HomePage "https://www.contoso.org" -IdentifierUris "https://www.adfcopytutorialapp.org/example" -Password "Pass@word1"
    ```

    Als u de volgende fout ziet, geeft u een andere URL op en voert u de opdracht opnieuw uit.
    
    ```PowerShell
    Another object with the same value for property identifierUris already exists.
    ```
7. Maak de AD-service-principal.

    ```PowerShell
    New-AzureRmADServicePrincipal -ApplicationId $azureAdApplication.ApplicationId
    ```
8. Voeg service-principal toe aan de rol **Inzender Data Factory**.

    ```PowerShell
    New-AzureRmRoleAssignment -RoleDefinitionName "Data Factory Contributor" -ServicePrincipalName $azureAdApplication.ApplicationId.Guid
    ```
9. Haal de toepassings-id op.

    ```PowerShell
    $azureAdApplication 
    ```
    Noteer de toepassings-id (**applicationID** uit de uitvoer).

U moet na deze stappen beschikken over de volgende vier waarden:

* Tenant-id
* Abonnements-id
* Toepassings-id
* Wachtwoord (opgegeven in de eerste opdracht)

## <a name="walkthrough"></a>Walkthrough
1. Maak met behulp van Visual Studio 2012/2013/2015 een C# .NET-consoletoepassing.
   1. Open **Visual Studio** 2012/2013/2015.
   2. Klik op **File**, houd de muisaanwijzer op **New** en klik op **Project**.
   3. Vouw **Templates** uit en selecteer **Visual C#**. Tijdens deze walkthrough gebruikt u C#, maar u kunt een willekeurige .NET-taal gebruiken.
   4. Selecteer **Console Application** uit de lijst met projecttypen aan de rechterkant.
   5. Voer **DataFactoryAPITestApp** in als de naam.
   6. Selecteer **C:\ADFGetStarted** als de locatie.
   7. Klik op **OK** om het project te maken.
2. Klik achtereenvolgens op **Tools**, houd de muisaanwijzer op **Nuget Package Manager** en klik op **Package Manager Console**.
3. Voer de volgende stappen uit in de **Package Manager Console**:
   1. Voer de volgende opdracht uit om het Data Factory-pakket te installeren: `Install-Package Microsoft.Azure.Management.DataFactories`
   2. Voer de volgende opdracht uit om het Azure Active Directory-pakket te installeren (u gebruikt de Active Directory API in de code): `Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -Version 2.19.208020213`
4. Voeg de volgende sectie **appSetttings** toe aan het bestand **App.config**. Deze instellingen worden gebruikt door de Help-methode: **GetAuthorizationHeader**.

    Vervang de waarden voor **&lt;Application ID&gt;**, **&lt;Password&gt;**, **&lt;Subscription ID&gt;** en **&lt;Tenant ID&gt;** door uw eigen waarden.

    ```xml
    <?xml version="1.0" encoding="utf-8" ?>
    <configuration>
        <startup>
            <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.5.2" />
        </startup>
        <appSettings>
            <add key="ActiveDirectoryEndpoint" value="https://login.windows.net/" />
            <add key="ResourceManagerEndpoint" value="https://management.azure.com/" />
            <add key="WindowsManagementUri" value="https://management.core.windows.net/" />

            <add key="ApplicationId" value="your application ID" />
            <add key="Password" value="Password you used while creating the AAD application" />
            <add key="SubscriptionId" value= "Subscription ID" />
            <add key="ActiveDirectoryTenantId" value="Tenant ID" />
        </appSettings>
    </configuration>
    ```

5. Voeg de volgende **using**-instructies toe aan het bronbestand (Program.cs) in het project.

    ```csharp
    using System.Threading;
    using System.Configuration;
    using System.Collections.ObjectModel;

    using Microsoft.Azure.Management.DataFactories;
    using Microsoft.Azure.Management.DataFactories.Models;
    using Microsoft.Azure.Management.DataFactories.Common.Models;

    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using Microsoft.Azure;
    ```

6. Voeg de volgende code toe aan de methode **Main** om een instantie van de klasse **DataPipelineManagementClient** te maken.
 U gebruikt dit object om een gegevensfactory, een gekoppelde service, gegevenssets voor invoer en uitvoer, en een pijplijn te maken. U gebruikt dit object ook om segmenten van een gegevensset te bewaken tijdens runtime.

    ```csharp
    // create data factory management client
    string resourceGroupName = "ADFTutorialResourceGroup";
    string dataFactoryName = "APITutorialFactory";

    TokenCloudCredentials aadTokenCredentials =
        new TokenCloudCredentials(
            ConfigurationManager.AppSettings["SubscriptionId"],
            GetAuthorizationHeader());

    Uri resourceManagerUri = new Uri(ConfigurationManager.AppSettings["ResourceManagerEndpoint"]);

    DataFactoryManagementClient client = new DataFactoryManagementClient(aadTokenCredentials, resourceManagerUri);
    ```

   > [!IMPORTANT]
   > Vervang de waarde van **resourceGroupName** door de naam van uw Azure-resourcegroep.
   >
   > Werk de naam van de gegevensfactory (**dataFactoryName**) bij zodat deze uniek is. De naam van de gegevensfactory moet wereldwijd uniek zijn. Raadpleeg het onderwerp [Data Factory - Naamgevingsregels](data-factory-naming-rules.md) voor meer informatie over naamgevingsregels voor Data Factory-artefacten.

7. Voeg de volgende code die een **gegevensfactory** maakt toe aan de methode **Main**.

    ```csharp
    // create a data factory
    Console.WriteLine("Creating a data factory");
    client.DataFactories.CreateOrUpdate(resourceGroupName,
        new DataFactoryCreateOrUpdateParameters()
        {
            DataFactory = new DataFactory()
            {
                Name = dataFactoryName,
                Location = "westus",
                Properties = new DataFactoryProperties() { }
            }
        }
    );
    ```

8. Voeg de volgende code die een **gekoppelde Azure Storage-service** maakt toe aan de methode **Main**.

   > [!IMPORTANT]
   > Vervang **storageaccountname** en **accountkey** door de naam en sleutel van uw Azure Storage-account.

    ```csharp
    // create a linked service for input data store: Azure Storage
    Console.WriteLine("Creating Azure Storage linked service");
    client.LinkedServices.CreateOrUpdate(resourceGroupName, dataFactoryName,
        new LinkedServiceCreateOrUpdateParameters()
        {
            LinkedService = new LinkedService()
            {
                Name = "AzureStorageLinkedService",
                Properties = new LinkedServiceProperties
                (
                    new AzureStorageLinkedService("DefaultEndpointsProtocol=https;AccountName=<storageaccountname>;AccountKey=<accountkey>")
                )
            }
        }
    );
    ```

9. Voeg de volgende code die een **gekoppelde Azure SLQ-service** maakt toe aan de methode **Main**.

   > [!IMPORTANT]
   > Vervang **servername**, **databasename**, **username** en **password** door de namen van uw Azure SQL-server, database, gebruiker en wachtwoord.

    ```csharp
    // create a linked service for output data store: Azure SQL Database
    Console.WriteLine("Creating Azure SQL Database linked service");
    client.LinkedServices.CreateOrUpdate(resourceGroupName, dataFactoryName,
        new LinkedServiceCreateOrUpdateParameters()
        {
            LinkedService = new LinkedService()
            {
                Name = "AzureSqlLinkedService",
                Properties = new LinkedServiceProperties
                (
                    new AzureSqlDatabaseLinkedService("Data Source=tcp:<servername>.database.windows.net,1433;Initial Catalog=<databasename>;User ID=<username>;Password=<password>;Integrated Security=False;Encrypt=True;Connect Timeout=30")
                )
            }
        }
    );
    ```

10. Voeg de volgende code die **gegevenssets voor invoer en uitvoer** maakt toe aan de methode **Main**.

    ```csharp
    // create input and output datasets
    Console.WriteLine("Creating input and output datasets");
    string Dataset_Source = "DatasetBlobSource";
    string Dataset_Destination = "DatasetAzureSqlDestination";

    Console.WriteLine("Creating input dataset of type: Azure Blob");
    client.Datasets.CreateOrUpdate(resourceGroupName, dataFactoryName,

    new DatasetCreateOrUpdateParameters()
    {
        Dataset = new Dataset()
        {
            Name = Dataset_Source,
            Properties = new DatasetProperties()
            {
                Structure = new List<DataElement>()
                {
                    new DataElement() { Name = "FirstName", Type = "String" },
                    new DataElement() { Name = "LastName", Type = "String" }
                },
                LinkedServiceName = "AzureStorageLinkedService",
                TypeProperties = new AzureBlobDataset()
                {
                    FolderPath = "adftutorial/",
                    FileName = "emp.txt"
                },
                External = true,
                Availability = new Availability()
                {
                    Frequency = SchedulePeriod.Hour,
                    Interval = 1,
                },

                Policy = new Policy()
                {
                    Validation = new ValidationPolicy()
                    {
                        MinimumRows = 1
                    }
                }
            }
        }
    });

    Console.WriteLine("Creating output dataset of type: Azure SQL");
    client.Datasets.CreateOrUpdate(resourceGroupName, dataFactoryName,
        new DatasetCreateOrUpdateParameters()
        {
            Dataset = new Dataset()
            {
                Name = Dataset_Destination,
                Properties = new DatasetProperties()
                {
                    Structure = new List<DataElement>()
                    {
                        new DataElement() { Name = "FirstName", Type = "String" },
                        new DataElement() { Name = "LastName", Type = "String" }
                    },
                    LinkedServiceName = "AzureSqlLinkedService",
                    TypeProperties = new AzureSqlTableDataset()
                    {
                        TableName = "emp"
                    },

                    Availability = new Availability()
                    {
                        Frequency = SchedulePeriod.Hour,
                        Interval = 1,
                    },
                }
            }
        });
    ```

11. Voeg de volgende code die **een pijplijn maakt en activeert** toe aan de methode **Main**. Deze pijplijn heeft een **CopyActivity** die **BlobSource** als een bron neemt en **BlobSink** als een sink.

    ```csharp
    // create a pipeline
    Console.WriteLine("Creating a pipeline");
    DateTime PipelineActivePeriodStartTime = new DateTime(2016, 8, 9, 0, 0, 0, 0, DateTimeKind.Utc);
    DateTime PipelineActivePeriodEndTime = PipelineActivePeriodStartTime.AddMinutes(60);
    string PipelineName = "ADFTutorialPipeline";

    client.Pipelines.CreateOrUpdate(resourceGroupName, dataFactoryName,
        new PipelineCreateOrUpdateParameters()
        {
            Pipeline = new Pipeline()
            {
                Name = PipelineName,
                Properties = new PipelineProperties()
                {
                    Description = "Demo Pipeline for data transfer between blobs",

            // Initial value for pipeline's active period. With this, you won't need to set slice status
            Start = PipelineActivePeriodStartTime,
                    End = PipelineActivePeriodEndTime,

                    Activities = new List<Activity>()
                    {
                        new Activity()
                        {
                            Name = "BlobToAzureSql",
                            Inputs = new List<ActivityInput>()
                            {
                                new ActivityInput() {
                                    Name = Dataset_Source
                                }
                            },
                            Outputs = new List<ActivityOutput>()
                            {
                                new ActivityOutput()
                                {
                                    Name = Dataset_Destination
                                }
                            },
                            TypeProperties = new CopyActivity()
                            {
                                Source = new BlobSource(),
                                Sink = new BlobSink()
                                {
                                    WriteBatchSize = 10000,
                                    WriteBatchTimeout = TimeSpan.FromMinutes(10)
                                }
                            }
                        }
                    },
                }
            }
        });
    ```

12. Voeg de volgende code toe aan de methode **Main** om de status van een gegevenssegment van de uitvoergegevensset te achterhalen. In dit voorbeeld wordt alleen een segment verwacht.

    ```csharp
    // Pulling status within a timeout threshold
    DateTime start = DateTime.Now;
    bool done = false;

    while (DateTime.Now - start < TimeSpan.FromMinutes(5) && !done)
    {
        Console.WriteLine("Pulling the slice status");
        // wait before the next status check
        Thread.Sleep(1000 * 12);

        var datalistResponse = client.DataSlices.List(resourceGroupName, dataFactoryName, Dataset_Destination,
            new DataSliceListParameters()
            {
                DataSliceRangeStartTime = PipelineActivePeriodStartTime.ConvertToISO8601DateTimeString(),
                DataSliceRangeEndTime = PipelineActivePeriodEndTime.ConvertToISO8601DateTimeString()
            });

        foreach (DataSlice slice in datalistResponse.DataSlices)
        {
            if (slice.State == DataSliceState.Failed || slice.State == DataSliceState.Ready)
            {
                Console.WriteLine("Slice execution is done with status: {0}", slice.State);
                done = true;
                break;
            }
            else
            {
                Console.WriteLine("Slice status is: {0}", slice.State);
            }
        }
    }
    ```

13. Voeg de volgende code toe om details van de uitvoering van een gegevenssegment naar de methode **Main** te achterhalen.

    ```csharp
    Console.WriteLine("Getting run details of a data slice");

    // give it a few minutes for the output slice to be ready
    Console.WriteLine("\nGive it a few minutes for the output slice to be ready and press any key.");
    Console.ReadKey();

    var datasliceRunListResponse = client.DataSliceRuns.List(
            resourceGroupName,
            dataFactoryName,
            Dataset_Destination,
            new DataSliceRunListParameters()
            {
                DataSliceStartTime = PipelineActivePeriodStartTime.ConvertToISO8601DateTimeString()
            }
        );

    foreach (DataSliceRun run in datasliceRunListResponse.DataSliceRuns)
    {
        Console.WriteLine("Status: \t\t{0}", run.Status);
        Console.WriteLine("DataSliceStart: \t{0}", run.DataSliceStart);
        Console.WriteLine("DataSliceEnd: \t\t{0}", run.DataSliceEnd);
        Console.WriteLine("ActivityId: \t\t{0}", run.ActivityName);
        Console.WriteLine("ProcessingStartTime: \t{0}", run.ProcessingStartTime);
        Console.WriteLine("ProcessingEndTime: \t{0}", run.ProcessingEndTime);
        Console.WriteLine("ErrorMessage: \t{0}", run.ErrorMessage);
    }

    Console.WriteLine("\nPress any key to exit.");
    Console.ReadKey();
    ```

14. Voeg de volgende Help-methode toe die door de methode **Main** wordt gebruikt voor de klasse **Program**.

    ```csharp
    public static string GetAuthorizationHeader()
    {
        AuthenticationResult result = null;
        var thread = new Thread(() =>
        {
            try
            {
                var context = new AuthenticationContext(ConfigurationManager.AppSettings["ActiveDirectoryEndpoint"] + ConfigurationManager.AppSettings["ActiveDirectoryTenantId"]);

                ClientCredential credential = new ClientCredential(ConfigurationManager.AppSettings["ApplicationId"], ConfigurationManager.AppSettings["Password"]);
                result = context.AcquireToken(resource: ConfigurationManager.AppSettings["WindowsManagementUri"], clientCredential: credential);
            }
            catch (Exception threadEx)
            {
                Console.WriteLine(threadEx.Message);
            }
        });

        thread.SetApartmentState(ApartmentState.STA);
        thread.Name = "AcquireTokenThread";
        thread.Start();
        thread.Join();

        if (result != null)
        {
            return result.AccessToken;
        }

        throw new InvalidOperationException("Failed to acquire token");
    }
    ```

15. Vouw in Solution Explorer het project (**DataFactoryAPITestApp**) uit, klik met de rechtermuisknop op **References** en klik vervolgens op **Add Reference**. Schakel het selectievakje voor '**System.Configuration**'-assembly in en klik op **OK**.
16. Bouw de consoletoepassing. Klik op **Build** in het menu en klik op **Build Solution**.
17. Controleer of er ten minste één bestand in de **adftutorial**-container in uw Azure Blob Storage staat. Als dit niet het geval is, maakt u in Kladblok het **Emp.txt**-bestand met de volgende inhoud en uploadt u het bestand naar de adftutorial-container.

    ```
    John, Doe
    Jane, Doe
    ```
18. Voer het voorbeeld uit door op **Debug** -> **Start Debugging** te klikken in het menu. Als u **Getting run details of a data slice** ziet, wacht u een paar minuten en drukt u op **ENTER**.
19. Gebruik Azure Portal om te controleren of de gegevensfactory **APITutorialFactory** wordt gemaakt met de volgende artefacten:
   * Gekoppelde service: **LinkedService_AzureStorage**
   * Gegevensset: **DatasetBlobSource** en **DatasetBlobDestination**.
   * Pijplijn: **PipelineBlobSample**
20. Controleer of de twee werknemersrecords zijn gemaakt in de tabel "**emp**" in de opgegeven Azure SQL-database.

## <a name="next-steps"></a>Volgende stappen
* Lees het artikel [Activiteiten voor gegevensverplaatsing](data-factory-data-movement-activities.md). Dit artikel biedt gedetailleerde informatie over Copy Activity die u tijdens deze zelfstudie hebt gemaakt.
* Zie [Data Factory .NET API-referentie](https://msdn.microsoft.com/library/mt415893.aspx) voor meer informatie over de Data Factory .NET SDK. Dit artikel behandelt niet de volledige Data Factory .NET API.




<!--HONumber=Jan17_HO1-->


