---
title: Een Azure data factory maken met behulp van .NET | Microsoft Docs
description: "Maak een Azure data factory om gegevens te kopiëren van een locatie in een Azure Blob Storage naar een andere locatie in dezelfde Blob Storage."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: 
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 01/22/2018
ms.author: jingwang
ms.openlocfilehash: b430f5932bcd54d5e2ab787fb6c4491b48dbc416
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2018
---
# <a name="create-a-data-factory-and-pipeline-using-net-sdk"></a>Een data factory en pijplijn maken met behulp van .NET SDK
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versie 1 - Algemene beschikbaarheid](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
> * [Versie 2 - Preview](quickstart-create-data-factory-dot-net.md)

In deze snelstartgids wordt beschreven hoe u .NET SDK kunt gebruiken om een Azure data factory te maken. Met de pijplijn die u in deze data factory maakt, worden gegevens **gekopieerd** van één map naar een andere map in een Azure Blob Storage. Zie [Zelfstudie: Gegevens transformeren met Spark](transform-data-using-spark.md) voor meer informatie over het **transformeren** van gegevens met Azure Data Factory. 

> [!NOTE]
> Dit artikel is van toepassing op versie 2 van Data Factory, dat zich momenteel in de previewfase bevindt. Als u versie 1 van de Data Factory-service gebruikt, die algemeen beschikbaar is (GA), raadpleegt u [Aan de slag met versie 1 van Data Factory](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).
>
> Dit artikel is geen gedetailleerde introductie tot de Data Factory-service. Zie [Inleiding tot Azure Data Factory](introduction.md) voor een inleiding tot Azure Data Factory-service.

Als u nog geen Azure-abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

### <a name="azure-subscription"></a>Azure-abonnement
Als u nog geen Azure-abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

### <a name="azure-roles"></a>Azure-rollen
Als u Data Factory-exemplaren wilt maken, moet het gebruikersaccount waarmee u zich bij Azure aanmeldt, lid zijn van de rollen **Inzender** of **Eigenaar**, of moet dit een **beheerder** van het Azure-abonnement zijn. Klik in Azure Portal op uw **gebruikersnaam** in de rechterbovenhoek en selecteer **Machtigingen** om de machtigingen weer te geven die u in het abonnement hebt. Als u toegang tot meerdere abonnementen hebt, moet u het juiste abonnement selecteren. Zie het artikel [Rollen toevoegen](../billing/billing-add-change-azure-subscription-administrator.md) voor voorbeelden van instructies voor het toevoegen van een gebruiker aan een rol.

### <a name="azure-storage-account"></a>Azure Storage-account
In deze QuickStart gaat u een algemeen Azure Storage-account (en dan met name voor Blob Storage) gebruiken als zowel **bron-** als **doel**gegevensarchieven. Zie het artikel [Een opslagaccount maken](../storage/common/storage-create-storage-account.md#create-a-storage-account) als u geen Azure Storage-account hebt voor algemene doeleinden en er een wilt maken. 

#### <a name="get-storage-account-name-and-account-key"></a>De naam en sleutel van een opslagaccount ophalen
In deze QuickStart gaat u de naam en sleutel van uw Azure Storage-account gebruiken. De volgende procedure bevat stappen waarmee u de naam en sleutel van uw opslagaccount kunt ophalen. 

1. Open een browser en navigeer naar [Azure Portal](https://portal.azure.com). Meld u aan met uw Azure-gebruikersnaam en -wachtwoord. 
2. Klik op **Meer services >** in het menu links, filter op het trefwoord **Opslag** en selecteer **Opslagaccounts**.

    ![Zoeken naar een opslagaccount](media/quickstart-create-data-factory-dot-net/search-storage-account.png)
3. Filter in de lijst met opslagaccounts op uw opslagaccount (indien nodig) en selecteer vervolgens **uw opslagaccount**. 
4. Selecteer op de pagina **Opslagaccount** de optie **Toegangssleutels** in het menu.

    ![De naam en sleutel van het opslagaccount ophalen](media/quickstart-create-data-factory-dot-net/storage-account-name-key.png)
5. Kopieer de waarden voor de velden **Opslagaccountnaam** en **key1** naar het klembord. Plak deze in Kladblok of een andere editor en sla ze op.  

#### <a name="create-input-folder-and-files"></a>Invoermap en bestanden maken
In deze sectie maakt u in uw Azure Blob Storage een blobcontainer met de naam **adftutorial**. Vervolgens maakt u in de container een map met de naam **Invoer** en uploadt u een voorbeeldbestand naar de map Invoer. 

1. Schakel op de pagina **Opslagaccount** over naar **Overzicht** en klik vervolgens op **Blobs**. 

    ![De optie Blobs selecteren](media/quickstart-create-data-factory-dot-net/select-blobs.png)
2. Klik op de pagina **Blob service** in de werkbalk op **+ Container**. 

    ![Knop Container toevoegen](media/quickstart-create-data-factory-dot-net/add-container-button.png)    
3. Voer in het dialoogvenster **Nieuwe container** als naam **adftutorial** in en klik op **OK**. 

    ![Naam van container invoeren](media/quickstart-create-data-factory-dot-net/new-container-dialog.png)
4. Klik op **adftutorial** in de lijst met containers. 

    ![De container selecteren](media/quickstart-create-data-factory-dot-net/select-adftutorial-container.png)
1. Klik op de pagina **Container** in de werkbalk op **Uploaden**.  

    ![Knop Uploaden](media/quickstart-create-data-factory-dot-net/upload-toolbar-button.png)
6. Klik op de pagina **Blob uploaden** op **Geavanceerd**.

    ![Op de koppeling Geavanceerd klikken](media/quickstart-create-data-factory-dot-net/upload-blob-advanced.png)
7. Start **Kladblok** en maak een bestand met de naam **emp.txt** met de volgende inhoud. Sla dit bestand op in de map **c:\ADFv2QuickStartPSH**. Maak de map **ADFv2QuickStartPSH** als deze nog niet bestaat.
    
    ```
    John, Doe
    Jane, Doe
    ```    
8. Blader in Azure Portal op de pagina **Blob uploaden** naar het bestand **emp.txt** en selecteer dit voor het veld **Bestanden**. 
9. Typ **Invoer** als waarde in het veld **Uploaden naar map**. 

    ![Blob-instellingen uploaden](media/quickstart-create-data-factory-dot-net/upload-blob-settings.png)    
10. Controleer of de map **Invoer** en het bestand **emp.txt** is en klik op **Uploaden**.
11. Als het goed is, ziet u in de lijst nu het bestand **emp.txt**, evenals de uploadstatus hiervan. 
12. Sluit de pagina **Blob uploaden** door op de **X** in de hoek te klikken. 

    ![De pagina Blob uploaden sluiten](media/quickstart-create-data-factory-dot-net/close-upload-blob.png)
1. Houd de pagina **Container** geopend. U gaat hiermee aan het einde van deze QuickStart de uitvoer controleren.

### <a name="visual-studio"></a>Visual Studio
De procedures in dit artikel zijn gebaseerd op Visual Studio 2017. U kunt ook Visual Studio 2013 of 2015 gebruiken.

### <a name="azure-net-sdk"></a>Azure .NET SDK
Download en installeer [Azure .NET SDK](http://azure.microsoft.com/downloads/) op uw computer.

### <a name="create-an-application-in-azure-active-directory"></a>Een toepassing maken in Azure Active Directory
Volg de instructies in [dit artikel](../azure-resource-manager/resource-group-create-service-principal-portal.md#create-an-azure-active-directory-application) om het volgende te doen: 

1. **Een Azure Active Directory-toepassing maken**. Maak een toepassing in Azure Active Directory die staat voor de .NET-toepassing die u in deze zelfstudie maakt. Voor de aanmeldings-URL kunt u een dummy-URL opgeven, zoals wordt getoond in het artikel (`https://contoso.org/exampleapp`).
2. Haal de **toepassings-id** en **verificatiesleutel*** op met behulp van de instructies in de sectie **De toepassings-id en verificatiesleutel ophalen** in het artikel. Noteer deze waarden, zodat u ze later in deze zelfstudie kunt gebruiken. 
3. Haal de **tenant-id** op met behulp van de instructies in de sectie **Tenant-id ophalen** in het artikel. Noteer deze waarde. 
4. Wijs de toepassing toe aan de rol **Inzender** op het niveau van het abonnement, zodat met de toepassing gegevensfactory's in het abonnement kunnen worden gemaakt. Volg de instructies in de sectie **Toepassing toewijzen aan rol** in het artikel. 

## <a name="create-a-visual-studio-project"></a>Een Visual Studio-project maken

Maak met behulp van Visual Studio 2013/2015/2017 een C# .NET-consoletoepassing.

1. Start **Visual Studio**.
2. Klik op **File**, houd de muisaanwijzer op **New** en klik op **Project**.
3. Selecteer **Visual C#** -> **Console App (.NET Framework)** in de lijst met projecttypen aan de rechterkant. .NET versie 4.5.2 of hoger is vereist.
4. Voer **ADFv2QuickStart** in als de naam.
5. Klik op **OK** om het project aan te maken.

## <a name="install-nuget-packages"></a>NuGet-pakketten installeren

1. Klik op **Hulpprogramma's** -> **NuGet Package Manager** -> **Package Manager-console**.
2. Voer in **Package Manager Console** de volgende opdrachten uit om pakketten te installeren:

    ```
    Install-Package Microsoft.Azure.Management.DataFactory -Prerelease
    Install-Package Microsoft.Azure.Management.ResourceManager -Prerelease
    Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory

    ```

## <a name="create-a-data-factory-client"></a>Een data factory-client maken

1. Open **Program.cs** en neem de volgende instructies op om verwijzingen naar naamruimten toe te voegen.

    ```csharp
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using Microsoft.Rest;
    using Microsoft.Azure.Management.ResourceManager;
    using Microsoft.Azure.Management.DataFactory;
    using Microsoft.Azure.Management.DataFactory.Models;
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    ```

2. Voeg de volgende code toe aan de methode **Main** waarmee de variabelen worden ingesteld. Vervang de plaatsaanduidingen door uw eigen waarden. Momenteel kunt u in Data Factory V2 alleen data factory's maken in de regio's VS - oost, VS - oost 2 en West-Europa. De gegevensopslagexemplaren (Azure Storage, Azure SQL Database, enzovoort) en berekeningen (HDInsight, enzovoort) die worden gebruikt in Data Factory, kunnen zich in andere regio's bevinden.

    ```csharp
    // Set variables
    string tenantID = "<your tenant ID>";
    string applicationId = "<your application ID>";
    string authenticationKey = "<your authentication key for the application>";
    string subscriptionId = "<your subscription ID where the data factory resides>";
    string resourceGroup = "<your resource group where the data factory resides>";
    string region = "East US 2";
    string dataFactoryName = "<specify the name of data factory to create. It must be globally unique.>";
    string storageAccount = "<your storage account name to copy data>";
    string storageKey = "<your storage account key>";
    // specify the container and input folder from which all files need to be copied to the output folder. 
    string inputBlobPath = "<the path to existing blob(s) to copy data from, e.g. containername/foldername>";
    //specify the contains and output folder where the files are copied
    string outputBlobPath = "<the blob path to copy data to, e.g. containername/foldername>";

    string storageLinkedServiceName = "AzureStorageLinkedService";  // name of the Azure Storage linked service
    string blobDatasetName = "BlobDataset";             // name of the blob dataset
    string pipelineName = "Adfv2QuickStartPipeline";    // name of the pipeline
    ```

3. Voeg de volgende code toe aan de methode **Main** om een instantie van de klasse **DataFactoryManagementClient** te maken. U gebruikt dit object om een data factory, een gekoppelde service, gegevenssets en een pijplijn te maken. U kunt dit object ook gebruiken om de details van de pijplijnuitvoering te controleren.

    ```csharp
    // Authenticate and create a data factory management client
    var context = new AuthenticationContext("https://login.windows.net/" + tenantID);
    ClientCredential cc = new ClientCredential(applicationId, authenticationKey);
    AuthenticationResult result = context.AcquireTokenAsync("https://management.azure.com/", cc).Result;
    ServiceClientCredentials cred = new TokenCredentials(result.AccessToken);
    var client = new DataFactoryManagementClient(cred) { SubscriptionId = subscriptionId };
    ```

## <a name="create-a-data-factory"></a>Een gegevensfactory maken

Voeg de volgende code toe aan de methode **Main** om een **data factory** te maken. 

```csharp
// Create a data factory
Console.WriteLine("Creating data factory " + dataFactoryName + "...");
Factory dataFactory = new Factory
{
    Location = region,
    Identity = new FactoryIdentity()
};
client.Factories.CreateOrUpdate(resourceGroup, dataFactoryName, dataFactory);
Console.WriteLine(SafeJsonConvert.SerializeObject(dataFactory, client.SerializationSettings));

while (client.Factories.Get(resourceGroup, dataFactoryName).ProvisioningState == "PendingCreation")
{
    System.Threading.Thread.Sleep(1000);
}
```

## <a name="create-a-linked-service"></a>Een gekoppelde service maken

Voeg de volgende code toe aan de methode **Main** om een **gekoppelde Azure Storage-service** te maken.

U maakt gekoppelde services in een gegevensfactory om uw gegevensarchieven en compute-services aan de gegevensfactory te koppelen. In deze snelstartgids hoeft u maar één gekoppelde Azure Storage-service te maken, die wordt gebruikt als de bron voor het kopiëren en als de sinkopslag, met de naam 'AzureStorageLinkedService'.

```csharp
// Create an Azure Storage linked service
Console.WriteLine("Creating linked service " + storageLinkedServiceName + "...");

LinkedServiceResource storageLinkedService = new LinkedServiceResource(
    new AzureStorageLinkedService
    {
        ConnectionString = new SecureString("DefaultEndpointsProtocol=https;AccountName=" + storageAccount + ";AccountKey=" + storageKey)
    }
);
client.LinkedServices.CreateOrUpdate(resourceGroup, dataFactoryName, storageLinkedServiceName, storageLinkedService);
Console.WriteLine(SafeJsonConvert.SerializeObject(storageLinkedService, client.SerializationSettings));
```

## <a name="create-a-dataset"></a>Een gegevensset maken

Voeg de volgende code toe aan de methode **Main** om een **Azure Blob Storage-gegevensset** te maken.

U definieert een gegevensset die de gegevens vertegenwoordigt die van een bron naar een sink moeten worden gekopieerd. In dit voorbeeld verwijst deze Blob-gegevensset naar de gekoppelde Azure Storage-service die u in de vorige stap gemaakt hebt. De gegevensset heeft een parameter nodig waarvan de waarde wordt ingesteld in een activiteit die de gegevensset verbruikt. De parameter wordt gebruikt om het 'folderPath' te maken dat verwijst naar de plaats waar de gegevens zich bevinden/zijn opgeslagen.

```csharp
// Create a Azure Blob dataset
Console.WriteLine("Creating dataset " + blobDatasetName + "...");
DatasetResource blobDataset = new DatasetResource(
    new AzureBlobDataset
    {
        LinkedServiceName = new LinkedServiceReference
        {
            ReferenceName = storageLinkedServiceName
        },
        FolderPath = new Expression { Value = "@{dataset().path}" },
        Parameters = new Dictionary<string, ParameterSpecification>
        {
            { "path", new ParameterSpecification { Type = ParameterType.String } }

        }
    }
);
client.Datasets.CreateOrUpdate(resourceGroup, dataFactoryName, blobDatasetName, blobDataset);
Console.WriteLine(SafeJsonConvert.SerializeObject(blobDataset, client.SerializationSettings));
```

## <a name="create-a-pipeline"></a>Een pijplijn maken

Voeg de volgende code toe aan de methode **Main** om **een pijplijn met een kopieeractiviteit te maken**.

In dit voorbeeld bevat deze pijplijn één activiteit en heeft deze twee parameters: het pad van de invoerblob en het pad van de uitvoerblob. De waarden voor deze parameters worden ingesteld wanneer de pijplijn wordt geactiveerd of uitgevoerd. De kopieeractiviteit verwijst naar dezelfde blobgegevensset die u in de vorige stap heb gemaakt als invoer en uitvoer. Wanneer de dataset wordt gebruikt als invoergegevensset, wordt het invoerpad opgegeven. En wanneer de dataset wordt gebruikt als uitvoergegevensset, wordt het uitvoerpad opgegeven. 

```csharp
// Create a pipeline with a copy activity
Console.WriteLine("Creating pipeline " + pipelineName + "...");
PipelineResource pipeline = new PipelineResource
{
    Parameters = new Dictionary<string, ParameterSpecification>
    {
        { "inputPath", new ParameterSpecification { Type = ParameterType.String } },
        { "outputPath", new ParameterSpecification { Type = ParameterType.String } }
    },
    Activities = new List<Activity>
    {
        new CopyActivity
        {
            Name = "CopyFromBlobToBlob",
            Inputs = new List<DatasetReference>
            {
                new DatasetReference()
                {
                    ReferenceName = blobDatasetName,
                    Parameters = new Dictionary<string, object>
                    {
                        { "path", "@pipeline().parameters.inputPath" }
                    }
                }
            },
            Outputs = new List<DatasetReference>
            {
                new DatasetReference
                {
                    ReferenceName = blobDatasetName,
                    Parameters = new Dictionary<string, object>
                    {
                        { "path", "@pipeline().parameters.outputPath" }
                    }
                }
            },
            Source = new BlobSource { },
            Sink = new BlobSink { }
        }
    }
};
client.Pipelines.CreateOrUpdate(resourceGroup, dataFactoryName, pipelineName, pipeline);
Console.WriteLine(SafeJsonConvert.SerializeObject(pipeline, client.SerializationSettings));
```

## <a name="create-a-pipeline-run"></a>Een pijplijnuitvoering maken

Voeg de volgende code toe aan de methode **Main** om een **pijplijnuitvoering te activeren**.

Met deze code worden ook de waarden van de parameters **inputPath** en **outputPath** die in de pijplijn worden opgegeven, ingesteld op de werkelijke waarden van de bron- en sinkblobpaden.

```csharp
// Create a pipeline run
Console.WriteLine("Creating pipeline run...");
Dictionary<string, object> parameters = new Dictionary<string, object>
{
    { "inputPath", inputBlobPath },
    { "outputPath", outputBlobPath }
};
CreateRunResponse runResponse = client.Pipelines.CreateRunWithHttpMessagesAsync(resourceGroup, dataFactoryName, pipelineName, parameters).Result.Body;
Console.WriteLine("Pipeline run ID: " + runResponse.RunId);
```

## <a name="monitor-a-pipeline-run"></a>Een pijplijnuitvoering controleren

1. Voeg de volgende code toe aan de methode **Main** om continu de status te controleren totdat het kopiëren van de gegevens is voltooid.

    ```csharp
    // Monitor the pipeline run
    Console.WriteLine("Checking pipeline run status...");
    PipelineRun pipelineRun;
    while (true)
    {
        pipelineRun = client.PipelineRuns.Get(resourceGroup, dataFactoryName, runResponse.RunId);
        Console.WriteLine("Status: " + pipelineRun.Status);
        if (pipelineRun.Status == "InProgress")
            System.Threading.Thread.Sleep(15000);
        else
            break;
    }
    ```

2. Voeg de volgende code toe aan de methode **Main** om details van de kopieeractiviteit weer te geven, zoals de omvang van de gelezen of weggeschreven gegevens.

    ```csharp
    // Check the copy activity run details
    Console.WriteLine("Checking copy activity run details...");
   
    List<ActivityRun> activityRuns = client.ActivityRuns.ListByPipelineRun(
    resourceGroup, dataFactoryName, runResponse.RunId, DateTime.UtcNow.AddMinutes(-10), DateTime.UtcNow.AddMinutes(10)).ToList(); 
    if (pipelineRun.Status == "Succeeded")
        Console.WriteLine(activityRuns.First().Output);
    else
        Console.WriteLine(activityRuns.First().Error);
    Console.WriteLine("\nPress any key to exit...");
    Console.ReadKey();
    ```

## <a name="run-the-code"></a>De code uitvoeren

Bouw en start de toepassing en controleer vervolgens de uitvoering van de pijplijn.

In de console wordt de voortgang weergegeven van het maken van een data factory, een gekoppelde service, gegevenssets, pijplijn en pijplijnuitvoering. Vervolgens wordt de uitvoeringsstatus van de pijplijn gecontroleerd. Wacht totdat u details ziet van de uitvoering van de kopieeractiviteit, waaronder de omvang van de gelezen/weggeschreven gegevens. Gebruik vervolgens hulpprogramma's als [Azure Storage Explorer](https://azure.microsoft.com/features/storage-explorer/) om te controleren of de blob(s) is/zijn gekopieerd van het 'inputBlobPath' naar het 'outputBlobPath' zoals u hebt opgegeven in de variabelen.

### <a name="sample-output"></a>Voorbeelduitvoer: 
```json
Creating data factory SPv2Factory0907...
{
  "identity": {
    "type": "SystemAssigned"
  },
  "location": "East US"
}
Creating linked service AzureStorageLinkedService...
{
  "properties": {
    "type": "AzureStorage",
    "typeProperties": {
      "connectionString": {
        "value": "DefaultEndpointsProtocol=https;AccountName=<storageAccountName>;AccountKey=<storageAccountKey>",
        "type": "SecureString"
      }
    }
  }
}
Creating dataset BlobDataset...
{
  "properties": {
    "type": "AzureBlob",
    "typeProperties": {
      "folderPath": {
        "value": "@{dataset().path}",
        "type": "Expression"
      }
    },
    "linkedServiceName": {
      "referenceName": "AzureStorageLinkedService",
      "type": "LinkedServiceReference"
    },
    "parameters": {
      "path": {
        "type": "String"
      }
    }
  }
}
Creating pipeline Adfv2QuickStartPipeline...
{
  "properties": {
    "activities": [
      {
        "type": "Copy",
        "typeProperties": {
          "source": {
            "type": "BlobSource"
          },
          "sink": {
            "type": "BlobSink"
          }
        },
        "inputs": [
          {
            "referenceName": "BlobDataset",
            "parameters": {
              "path": "@pipeline().parameters.inputPath"
            },
            "type": "DatasetReference"
          }
        ],
        "outputs": [
          {
            "referenceName": "BlobDataset",
            "parameters": {
              "path": "@pipeline().parameters.outputPath"
            },
            "type": "DatasetReference"
          }
        ],
        "name": "CopyFromBlobToBlob"
      }
    ],
    "parameters": {
      "inputPath": {
        "type": "String"
      },
      "outputPath": {
        "type": "String"
      }
    }
  }
}
Creating pipeline run...
Pipeline run ID: 308d222d-3858-48b1-9e66-acd921feaa09
Checking pipeline run status...
Status: InProgress
Status: InProgress
Checking copy activity run details...
{
    "dataRead": 331452208,
    "dataWritten": 331452208,
    "copyDuration": 23,
    "throughput": 14073.209,
    "errors": [],
    "effectiveIntegrationRuntime": "DefaultIntegrationRuntime (West US)",
    "usedCloudDataMovementUnits": 2,
    "billedDuration": 23
}

Press any key to exit...
```

## <a name="verify-the-output"></a>De uitvoer controleren
De uitvoermap wordt automatisch door de pijplijn gemaakt in de blobcontainer adftutorial. Vervolgens wordt het bestand emp.txt gekopieerd van de invoermap naar de uitvoermap. 

1. Klik in Azure Portal op de pagina met de **adftutorial**-container op **Vernieuwen** om de uitvoermap weer te geven. 
    
    ![Vernieuwen](media/quickstart-create-data-factory-dot-net/output-refresh.png)
2. Klik in de lijst met mappen op **Uitvoer**. 
2. Controleer of het bestand **emp.txt** naar de uitvoermap is gekopieerd. 

    ![Vernieuwen](media/quickstart-create-data-factory-dot-net/output-file.png)

## <a name="clean-up-resources"></a>Resources opschonen
Als u de data factory programmatisch wilt verwijderen, voegt u de volgende regels code toe aan het programma: 

```csharp
            Console.WriteLine("Deleting the data factory");
            client.Factories.Delete(resourceGroup, dataFactoryName);
```

## <a name="next-steps"></a>Volgende stappen
Met de pijplijn in dit voorbeeld worden gegevens gekopieerd van de ene locatie naar een andere locatie in een Azure Blob-opslag. Doorloop de [zelfstudies](tutorial-copy-data-dot-net.md) voor meer informatie over het gebruiken van Data Factory in andere scenario's. 
