---
title: Gegevenspijplijnen maken met behulp van Azure .NET SDK | Microsoft Docs
description: Informatie over het programmatisch maken, bewaken en beheren van Azure data factory's met behulp van de Data Factory-SDK.
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: b0a357be-3040-4789-831e-0d0a32a0bda5
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/22/2018
ms.author: spelluru
robots: noindex
ms.openlocfilehash: 35041e148e52e5c567601c53dffac05c88d45ed5
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2018
---
# <a name="create-monitor-and-manage-azure-data-factories-using-azure-data-factory-net-sdk"></a>Maken, bewaken en beheren van Azure data Factory met Azure Data Factory .NET SDK
> [!NOTE]
> Dit artikel is van toepassing op versie 1 van Data Factory, die algemeen beschikbaar is. Als u versie 2 van de Data Factory-service gebruikt, welke nog een preview is, raadpleegt u de [zelfstudie kopieeractiviteit in de documentatie voor versie 2](../quickstart-create-data-factory-dot-net.md). 

## <a name="overview"></a>Overzicht
U kunt maken, bewaken en beheren van Azure data factory's programmatisch met behulp van Data Factory .NET SDK. Dit artikel bevat een overzicht die u volgen kunt om een .NET-console voorbeeldtoepassing die u maakt en bewaakt een gegevensfactory maken. 

> [!NOTE]
> Dit artikel behandelt niet de volledige Data Factory .NET API. Zie [Data Factory .NET API Reference](/dotnet/api/index?view=azuremgmtdatafactories-4.12.1) voor uitgebreide documentatie over .NET API voor Data Factory. 

## <a name="prerequisites"></a>Vereisten
* Visual Studio 2012 of 2013 of 2015
* Download en installeer [Azure .NET SDK](http://azure.microsoft.com/downloads/).
* Azure PowerShell. Volg de instructies in [Azure PowerShell installeren en configureren](/powershell/azure/overview) om Azure PowerShell te installeren op uw computer. Azure PowerShell wordt gebruikt om een Azure Active Directory-toepassing te maken.

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
    $azureAdApplication = New-AzureRmADApplication -DisplayName "ADFDotNetWalkthroughApp" -HomePage "https://www.contoso.org" -IdentifierUris "https://www.adfdotnetwalkthroughapp.org/example" -Password "Pass@word1"
    ```

    Als u de volgende fout ziet, geeft u een andere URL op en voert u de opdracht opnieuw uit.
    
    ```PowerShell
    Another object with the same value for property identifierUris already exists.
    ```
7. Maak de AD-service-principal.

    ```PowerShell
    New-AzureRmADServicePrincipal -ApplicationId $azureAdApplication.ApplicationId
    ```
8. Voeg de service-principal toe aan de rol **Inzender Data Factory**.

    ```PowerShell
    New-AzureRmRoleAssignment -RoleDefinitionName "Data Factory Contributor" -ServicePrincipalName $azureAdApplication.ApplicationId.Guid
    ```
9. Haal de toepassings-id op.

    ```PowerShell
    $azureAdApplication 
    ```
    Noteer de toepassings-id (applicationID in de uitvoer).

U moet na deze stappen beschikken over de volgende vier waarden:

* Tenant-id
* Abonnements-id
* Toepassings-id
* Wachtwoord (opgegeven in de eerste opdracht)

## <a name="walkthrough"></a>Walkthrough
In dit overzicht kunt u een gegevensfactory maken met een pijplijn met een kopieeractiviteit. De kopieeractiviteit kopieert gegevens van een map in de Azure blob-opslag naar een andere map in dezelfde blob storage. 

Met de kopieeractiviteit wordt de gegevensverplaatsing in Azure Data Factory uitgevoerd. De activiteit wordt mogelijk gemaakt door een wereldwijd beschikbare service waarmee gegevens veilig, betrouwbaar en schaalbaar kunnen worden gekopieerd tussen verschillende gegevensarchieven. Zie [Activiteiten voor gegevensverplaatsing](data-factory-data-movement-activities.md) voor meer informatie over de kopieeractiviteit.

1. Maak met behulp van Visual Studio 2012/2013/2015 een C# .NET-consoletoepassing.
   1. Open **Visual Studio** 2012/2013/2015.
   2. Klik op **File**, houd de muisaanwijzer op **New** en klik op **Project**.
   3. Vouw **Templates** uit en selecteer **Visual C#**. Tijdens deze walkthrough gebruikt u C#, maar u kunt een willekeurige .NET-taal gebruiken.
   4. Selecteer **Console Application** uit de lijst met projecttypen aan de rechterkant.
   5. Voer **DataFactoryAPITestApp** in als de naam.
   6. Selecteer **C:\ADFGetStarted** als de locatie.
   7. Klik op **OK** om het project aan te maken.
2. Klik op **Tools**, wijs **NuGet Package Manager** aan en klik op **Package Manager Console**.
3. Voer de volgende stappen uit in de **Package Manager Console**:
   1. Voer de volgende opdracht uit om het Data Factory-pakket te installeren: `Install-Package Microsoft.Azure.Management.DataFactories`
   2. Voer de volgende opdracht uit om het Azure Active Directory-pakket te installeren (u gebruikt de Active Directory API in de code): `Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -Version 2.19.208020213`
4. Vervang de inhoud van **App.config** bestand in het project met de volgende inhoud: 
    
    ```xml
    <?xml version="1.0" encoding="utf-8" ?>
    <configuration>
        <appSettings>
            <add key="ActiveDirectoryEndpoint" value="https://login.microsoftonline.com/" />
            <add key="ResourceManagerEndpoint" value="https://management.azure.com/" />
            <add key="WindowsManagementUri" value="https://management.core.windows.net/" />

            <add key="ApplicationId" value="your application ID" />
            <add key="Password" value="Password you used while creating the AAD application" />
            <add key="SubscriptionId" value= "Subscription ID" />
            <add key="ActiveDirectoryTenantId" value="Tenant ID" />
        </appSettings>
    </configuration>
    ```
5. Waarden voor bijwerken in het bestand App.Config  **&lt;toepassings-ID&gt;**,  **&lt;wachtwoord&gt;**,  **&lt;abonnement ID&gt;**, en  **&lt;tenant-ID&gt;**  met uw eigen waarden.
6. Voeg de volgende **met** instructies voor het **Program.cs** bestand in het project.

    ```csharp
    using System.Configuration;
    using System.Collections.ObjectModel;
    using System.Threading;
    using System.Threading.Tasks;

    using Microsoft.Azure;
    using Microsoft.Azure.Management.DataFactories;
    using Microsoft.Azure.Management.DataFactories.Models;
    using Microsoft.Azure.Management.DataFactories.Common.Models;

    using Microsoft.IdentityModel.Clients.ActiveDirectory;

    ```
6. Voeg de volgende code toe aan de methode **Main** om een instantie van de klasse **DataPipelineManagementClient** te maken.
 U gebruikt dit object om een gegevensfactory, een gekoppelde service, gegevenssets voor invoer en uitvoer, en een pijplijn te maken. U gebruikt dit object ook om segmenten van een gegevensset te bewaken tijdens runtime.

    ```csharp
    // create data factory management client

    //IMPORTANT: specify the name of Azure resource group here
    string resourceGroupName = "ADFTutorialResourceGroup";

    //IMPORTANT: the name of the data factory must be globally unique.
    // Therefore, update this value. For example:APITutorialFactory05122017
    string dataFactoryName = "APITutorialFactory";

    TokenCloudCredentials aadTokenCredentials = new TokenCloudCredentials(
            ConfigurationManager.AppSettings["SubscriptionId"],
            GetAuthorizationHeader().Result);

    Uri resourceManagerUri = new Uri(ConfigurationManager.AppSettings["ResourceManagerEndpoint"]);

    DataFactoryManagementClient client = new DataFactoryManagementClient(aadTokenCredentials, resourceManagerUri);
    ```

   > [!IMPORTANT]
   > Vervang de waarde van **resourceGroupName** door de naam van uw Azure-resourcegroep. U kunt maken met een resource-groep met de [New-AzureResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) cmdlet.
   >
   > Werk de naam van de data factory (dataFactoryName) zodanig bij dat deze uniek is. De naam van de gegevensfactory moet wereldwijd uniek zijn. Raadpleeg het onderwerp [Data Factory - Naamgevingsregels](data-factory-naming-rules.md) voor meer informatie over naamgevingsregels voor Data Factory-artefacten.
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
                Properties = new DataFactoryProperties()
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
9. Voeg de volgende code die **gegevenssets voor invoer en uitvoer** maakt toe aan de methode **Main**.

    De **FolderPath** voor de blob-invoerbron is ingesteld op **adftutorial /** waar **adftutorial** is de naam van de container in de blob-opslag. Als deze container niet in de Azure blob-opslag bestaat, een container maken met deze naam: **adftutorial** en een tekstbestand te uploaden naar de container.

    Het mappad voor de uitvoer-blob is ingesteld op: **adftutorial/apifactoryoutput / {segmenteren}** waar **segment** dynamisch wordt berekend op basis van de waarde van **SliceStart** () datum / tijd van elk segment starten).

    ```csharp
    // create input and output datasets
    Console.WriteLine("Creating input and output datasets");
    string Dataset_Source = "DatasetBlobSource";
    string Dataset_Destination = "DatasetBlobDestination";
    
    client.Datasets.CreateOrUpdate(resourceGroupName, dataFactoryName,
    new DatasetCreateOrUpdateParameters()
    {
        Dataset = new Dataset()
        {
            Name = Dataset_Source,
            Properties = new DatasetProperties()
            {
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
    
    client.Datasets.CreateOrUpdate(resourceGroupName, dataFactoryName,
    new DatasetCreateOrUpdateParameters()
    {
        Dataset = new Dataset()
        {
            Name = Dataset_Destination,
            Properties = new DatasetProperties()
            {
    
                LinkedServiceName = "AzureStorageLinkedService",
                TypeProperties = new AzureBlobDataset()
                {
                    FolderPath = "adftutorial/apifactoryoutput/{Slice}",
                    PartitionedBy = new Collection<Partition>()
                    {
                        new Partition()
                        {
                            Name = "Slice",
                            Value = new DateTimePartitionValue()
                            {
                                Date = "SliceStart",
                                Format = "yyyyMMdd-HH"
                            }
                        }
                    }
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
10. Voeg de volgende code die **een pijplijn maakt en activeert** toe aan de methode **Main**. Deze pijplijn heeft een **CopyActivity** die **BlobSource** als een bron neemt en **BlobSink** als een sink.

    Met de kopieeractiviteit wordt de gegevensverplaatsing in Azure Data Factory uitgevoerd. De activiteit wordt mogelijk gemaakt door een wereldwijd beschikbare service waarmee gegevens veilig, betrouwbaar en schaalbaar kunnen worden gekopieerd tussen verschillende gegevensarchieven. Zie [Activiteiten voor gegevensverplaatsing](data-factory-data-movement-activities.md) voor meer informatie over de kopieeractiviteit.

    ```csharp
    // create a pipeline
    Console.WriteLine("Creating a pipeline");
    DateTime PipelineActivePeriodStartTime = new DateTime(2014, 8, 9, 0, 0, 0, 0, DateTimeKind.Utc);
    DateTime PipelineActivePeriodEndTime = PipelineActivePeriodStartTime.AddMinutes(60);
    string PipelineName = "PipelineBlobSample";
    
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
                        Name = "BlobToBlob",
                        Inputs = new List<ActivityInput>()
                        {
                            new ActivityInput()
                {
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
12. Voeg de volgende code toe aan de methode **Main** om de status van een gegevenssegment van de uitvoergegevensset te achterhalen. Er is slechts één segment verwacht in dit voorbeeld.

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
13. **(optioneel)**  Voeg de volgende code als u wilt ophalen uitvoeren details voor een gegevenssegment naar de **Main** methode.

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
        });
    
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
14. Voeg de volgende Help-methode toe die door de methode **Main** wordt gebruikt voor de klasse **Program**. Deze methode verschijnt een dialoogvenster waarmee u bieden **gebruikersnaam** en **wachtwoord** die u gebruikt voor aanmelding bij Azure-portal.

    ```csharp
    public static async Task<string> GetAuthorizationHeader()
    {
        AuthenticationContext context = new AuthenticationContext(ConfigurationManager.AppSettings["ActiveDirectoryEndpoint"] + ConfigurationManager.AppSettings["ActiveDirectoryTenantId"]);
        ClientCredential credential = new ClientCredential(
            ConfigurationManager.AppSettings["ApplicationId"],
            ConfigurationManager.AppSettings["Password"]);
        AuthenticationResult result = await context.AcquireTokenAsync(
            resource: ConfigurationManager.AppSettings["WindowsManagementUri"],
            clientCredential: credential);

        if (result != null)
            return result.AccessToken;

        throw new InvalidOperationException("Failed to acquire token");
    }
    ```

15. Vouw het project in Solution Explorer: **DataFactoryAPITestApp**, met de rechtermuisknop op **verwijzingen**, en klik op **verwijzing toevoegen**. Schakel dit selectievakje in voor `System.Configuration` assembly en klik op **OK**.
15. Bouw de consoletoepassing. Klik op **Build** in het menu en klik op **Build Solution**.
16. Controleer of er ten minste één bestand in de container adftutorial in uw Azure-blobopslag. Als dit niet het geval is, Emp.txt-bestand in Kladblok met de volgende inhoud maken en uploaden naar de container adftutorial.

    ```
    John, Doe
    Jane, Doe
    ```
17. Voer het voorbeeld uit door op **Debug** -> **Start Debugging** te klikken in het menu. Als u **Getting run details of a data slice** ziet, wacht u een paar minuten en drukt u op **ENTER**.
18. Gebruik Azure Portal om te controleren of de gegevensfactory **APITutorialFactory** wordt gemaakt met de volgende artefacten:
    * Gekoppelde service: **AzureStorageLinkedService**
    * Gegevensset: **DatasetBlobSource** en **DatasetBlobDestination**.
    * Pijplijn: **PipelineBlobSample**
19. Controleren of een bestand voor uitvoer is gemaakt de **apifactoryoutput** map in de **adftutorial** container.

## <a name="get-a-list-of-failed-data-slices"></a>Een lijst met mislukte gegevenssegmenten ophalen 

```csharp
// Parse the resource path
var ResourceGroupName = "ADFTutorialResourceGroup";
var DataFactoryName = "DataFactoryAPITestApp";

var parameters = new ActivityWindowsByDataFactoryListParameters(ResourceGroupName, DataFactoryName);
parameters.WindowState = "Failed";
var response = dataFactoryManagementClient.ActivityWindows.List(parameters);
do
{
    foreach (var activityWindow in response.ActivityWindowListResponseValue.ActivityWindows)
    {
        var row = string.Join(
            "\t",
            activityWindow.WindowStart.ToString(),
            activityWindow.WindowEnd.ToString(),
            activityWindow.RunStart.ToString(),
            activityWindow.RunEnd.ToString(),
            activityWindow.DataFactoryName,
            activityWindow.PipelineName,
            activityWindow.ActivityName,
            string.Join(",", activityWindow.OutputDatasets));
        Console.WriteLine(row);
    }

    if (response.NextLink != null)
    {
        response = dataFactoryManagementClient.ActivityWindows.ListNext(response.NextLink, parameters);
    }
    else
    {
        response = null;
    }
}
while (response != null);
```

## <a name="next-steps"></a>Volgende stappen
Zie het volgende voorbeeld voor het maken van een pijplijn met .NET SDK waarmee gegevens worden gekopieerd van een Azure blob-opslag met een Azure SQL database: 

- [Een pijplijn om gegevens te kopiëren van Blob-opslag met SQL-Database maken](data-factory-copy-activity-tutorial-using-dotnet-api.md)
