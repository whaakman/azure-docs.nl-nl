---
title: Use custom activities in an Azure Data Factory pipeline (Aangepaste activiteiten gebruiken in een Azure Data Factory-pijplijn)
description: Leer hoe u aangepaste activiteiten te maken en deze gebruiken in een Azure Data Factory-pijplijn.
services: data-factory
documentationcenter: ''
author: douglaslMS
manager: craigg
ms.assetid: 8dd7ba14-15d2-4fd9-9ada-0b2c684327e9
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 01/10/2018
ms.author: douglasl
robots: noindex
ms.openlocfilehash: b7a2f9350633be5ec0cb8d5a7c6e7cc5048f956a
ms.sourcegitcommit: 8d88a025090e5087b9d0ab390b1207977ef4ff7c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/21/2018
ms.locfileid: "52275999"
---
# <a name="use-custom-activities-in-an-azure-data-factory-pipeline"></a>Use custom activities in an Azure Data Factory pipeline (Aangepaste activiteiten gebruiken in een Azure Data Factory-pijplijn)
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versie 1:](data-factory-use-custom-activities.md)
> * [Versie 2 (huidige versie)](../transform-data-using-dotnet-custom-activity.md)

> [!NOTE]
> Dit artikel is van toepassing op versie 1 van Data Factory. Als u de huidige versie van de Data Factory-service gebruikt, raadpleegt u [aangepaste activiteiten in V2](../transform-data-using-dotnet-custom-activity.md).

Er zijn twee soorten activiteiten die u in een Azure Data Factory-pijplijn gebruiken kunt.

- [Activiteiten voor gegevensverplaatsing](data-factory-data-movement-activities.md) verplaatsen van gegevens tussen [ondersteunde bron- en sinkblobpaden gegevensarchieven](data-factory-data-movement-activities.md#supported-data-stores-and-formats).
- [Activiteiten voor gegevenstransformatie](data-factory-data-transformation-activities.md) om gegevens te transformeren met behulp van rekenservices zoals Azure HDInsight, Azure Batch en Azure Machine Learning. 

Maken om gegevens te verplaatsen naar/van een gegevensarchief die geen ondersteuning biedt voor Data Factory, een **aangepaste activiteit** met uw eigen logica voor het verplaatsen van gegevens en gebruik de activiteit in een pijplijn. Op dezelfde manier als u wilt transformeren en verwerken gegevens op een manier die niet wordt ondersteund door Data Factory, een aangepaste activiteit maken met uw eigen logica van de transformatie van gegevens en het gebruik van de activiteit in een pijplijn. 

U kunt een aangepaste activiteit uit te voeren op een **Azure Batch** pool van virtuele machines. Wanneer u Azure Batch gebruikt, kunt u alleen een bestaande Azure Batch-pool.

De volgende procedure bevat stapsgewijze instructies voor het maken van een aangepaste .NET-activiteit en het gebruik van de aangepaste activiteit in een pijplijn. Deze stapsgewijze Kennismaking gebruikt een **Azure Batch** gekoppelde service. 

> [!IMPORTANT]
> - Het is niet mogelijk om te gebruiken van een Data Management Gateway van een aangepaste activiteit voor toegang tot on-premises gegevensbronnen. Op dit moment [Data Management Gateway](data-factory-data-management-gateway.md) ondersteunt alleen de kopieeractiviteit en de opgeslagen procedure-activiteit in Data Factory.   

## <a name="walkthrough-create-a-custom-activity"></a>Walkthrough: een aangepaste activiteit maken
### <a name="prerequisites"></a>Vereisten
* Visual Studio 2012/2013/2015
* Download en installeer [Azure .NET SDK](https://azure.microsoft.com/downloads/)

### <a name="azure-batch-prerequisites"></a>Vereisten voor Azure Batch
In het scenario, moet u uw aangepaste .NET-activiteiten met behulp van Azure Batch als een compute-resource uitvoeren. **Azure Batch** is een platform service voor het uitvoeren van grootschalige parallelle en high performance computing (HPC)-toepassingen efficiënt in de cloud. Azure Batch plant rekenintensief werk inplant voor uitvoering op een beheerde **verzameling van virtuele machines**, en kan automatisch de schaal van rekenkracht resources om te voldoen aan de behoeften van uw taken. Zie [basisbeginselen van Azure Batch] [ batch-technical-overview] artikel voor een gedetailleerd overzicht van de Azure Batch-service.

Voor deze zelfstudie maakt u een Azure Batch-account aan een pool van virtuele machines. Dit zijn de stappen:

1. Maak een **Azure Batch-account** met behulp van de [Azure-portal](http://portal.azure.com). Zie [maken en beheren van Azure Batch-account] [ batch-create-account] artikel voor meer informatie.
2. Noteer de Azure Batch-accountnaam, accountsleutel, URI en naam van groep. U moet ze om een gekoppelde Azure-Batch-service te maken.
    1. Op de startpagina van Azure Batch-account, ziet u een **URL** in de volgende indeling: `https://myaccount.westus.batch.azure.com`. In dit voorbeeld **myaccount** is de naam van de Azure Batch-account. U in de definitie van de gekoppelde service gebruikt-URI is de URL zonder de naam van het account. Bijvoorbeeld: `https://<region>.batch.azure.com`.
    2. Klik op **sleutels** op het menu links en kopieer de **primaire TOEGANGSSLEUTEL**.
    3. Voor het gebruik van een bestaande pool, klikt u op **Pools** in het menu en noteert u de **ID** van de pool. Als u geen een bestaande pool, verplaatst naar de volgende stap.     
2. Maak een **Azure Batch-pool**.

   1. In de [Azure-portal](https://portal.azure.com), klikt u op **Bladeren** in het menu links en klik op **Batch-Accounts**.
   2. Selecteer uw Azure Batch-account openen de **Batch-Account** blade.
   3. Klik op **Pools** tegel.
   4. In de **Pools** blade, klik op de knop toevoegen op de werkbalk om een groep toevoegen.
      1. Geef een ID voor de pool (groep-ID). Houd er rekening mee de **ID van de pool**; u moet deze bij het maken van de Data Factory-oplossing.
      2. Geef **Windows Server 2012 R2** voor de besturingssysteemgroep-instelling.
      3. Selecteer een **knooppunt prijscategorie**.
      4. Voer **2** als waarde voor de **doel toegewezen** instelling.
      5. Voer **2** als waarde voor de **maximum aantal taken per knooppunt** instelling.
   5. Klik op **OK** om de groep te maken.
   6. Noteer de **ID** van de pool. 



### <a name="high-level-steps"></a>Stappen op hoog niveau
Hier volgen de twee hoofdstappen die u als onderdeel van dit scenario uitvoert: 

1. Maak een aangepaste activiteit die eenvoudige transformatie/verwerkingslogica bevat.
2. Maak een Azure-gegevensfactory met een pijplijn met behulp van de aangepaste activiteit.

### <a name="create-a-custom-activity"></a>Een aangepaste activiteit maken
Als u wilt maken van een aangepaste .NET-activiteit, een **.NET-klassebibliotheek** project met een klasse die wordt geïmplementeerd die **IDotNetActivity** interface. Deze interface is slechts één methode: [Execute](https://msdn.microsoft.com/library/azure/mt603945.aspx) en de handtekening is:

```csharp
public IDictionary<string, string> Execute(
        IEnumerable<LinkedService> linkedServices,
        IEnumerable<Dataset> datasets,
        Activity activity,
        IActivityLogger logger)
```


De methode nodig vier parameters:

- **linkedServices**. Deze eigenschap is een overzichtslijst met Data Store gekoppelde services waarnaar wordt verwezen door de i/o-gegevenssets voor de activiteit.   
- **gegevenssets**. Deze eigenschap is een overzichtslijst met gegevenssets voor invoer/uitvoer voor de activiteit. U kunt deze parameter gebruiken om de locaties en schema's die zijn gedefinieerd door gegevenssets voor invoer en uitvoer te verkrijgen.
- **activiteit**. Deze eigenschap vertegenwoordigt de huidige activiteit. Het kan worden gebruikt voor toegang tot uitgebreide eigenschappen die zijn gekoppeld aan de aangepaste activiteit. Zie [toegang tot uitgebreide eigenschappen](#access-extended-properties) voor meer informatie.
- **logger**. Dit object kunt u het schrijven van de foutopsporing opmerkingen die aanvallen in het gebruikerslogboek voor de pijplijn.

De methode retourneert een woordenlijst die kan worden gebruikt om aangepaste activiteiten samen in de toekomst koppelen. Deze functie is nog niet geïmplementeerd, kunt u dus een lege woordenlijst retourneren van de methode.  

### <a name="procedure"></a>Procedure
1. Maak een **.NET-klassebibliotheek** project.
   <ol type="a">
     <li>Start <b>Visual Studio 2017</b> of <b>Visual Studio 2015</b> of <b>Visual Studio 2013</b> of <b>Visual Studio 2012</b>.</li>
     <li>Klik op <b>File</b>, houd de muisaanwijzer op <b>New</b> en klik op <b>Project</b>.</li>
     <li>Vouw <b>Templates</b> uit en selecteer <b>Visual C#</b>. In dit scenario maakt u C#, maar u kunt een .NET-taal gebruiken voor het ontwikkelen van de aangepaste activiteit.</li>
     <li>Selecteer <b>Class Library</b> uit de lijst met projecttypen aan de rechterkant. Kies in Visual Studio 2017, <b>Class Library (.NET Framework)</b> </li>
     <li>Voer <b>MyDotNetActivity</b> voor de <b>naam</b>.</li>
     <li>Selecteer <b>C:\ADFGetStarted</b> voor de <b>locatie</b>.</li>
     <li>Klik op <b>OK</b> om het project aan te maken.</li>
   </ol>
   
2. Klik op **Tools**, wijs **NuGet Package Manager** aan en klik op **Package Manager Console**.

3. Voer de volgende opdracht uit om te importeren in de Package Manager Console **Microsoft.Azure.Management.DataFactories**.

    ```PowerShell
    Install-Package Microsoft.Azure.Management.DataFactories
    ```
4. Importeren van de **Azure Storage** NuGet-pakket in het project.

    ```PowerShell
    Install-Package WindowsAzure.Storage -Version 4.3.0
    ```

    > [!IMPORTANT]
    > Data Factory-service starten is de versie 4.3 van WindowsAzure.Storage vereist. Als u een verwijzing naar een nieuwere versie van Azure Storage-assembly in uw project aangepaste activiteit toevoegt, ziet u een fout opgetreden bij het uitvoeren van de activiteit. U kunt de fout oplossen, Zie [Appdomain isolatie](#appdomain-isolation) sectie. 
5. Voeg de volgende **met behulp van** instructies toe aan het bronbestand in het project.

    ```csharp

    // Comment these lines if using VS 2017
    using System.IO;
    using System.Globalization;
    using System.Diagnostics;
    using System.Linq;
    // --------------------

    // Comment these lines if using <= VS 2015
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Text;
    using System.Threading.Tasks;
    // ---------------------

    using Microsoft.Azure.Management.DataFactories.Models;
    using Microsoft.Azure.Management.DataFactories.Runtime;

    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Blob;
    ```
6. Wijzig de naam van de **naamruimte** naar **MyDotNetActivityNS**.

    ```csharp
    namespace MyDotNetActivityNS
    ```
7. Wijzig de naam van de klasse **MyDotNetActivity** en die afkomstig zijn uit de **IDotNetActivity** interface zoals wordt weergegeven in het volgende codefragment:

    ```csharp
    public class MyDotNetActivity : IDotNetActivity
    ```
8. Implementeren (toevoegen) de **Execute** -methode van de **IDotNetActivity** interface met de **MyDotNetActivity** klasse en kopieer de volgende code toe aan de methode.

    Het volgende voorbeeld wordt het aantal exemplaren van de zoekterm ('Microsoft') in elke blob die is gekoppeld aan een gegevenssegment geteld.

    ```csharp
    /// <summary>
    /// Execute method is the only method of IDotNetActivity interface you must implement.
    /// In this sample, the method invokes the Calculate method to perform the core logic.  
    /// </summary>
    
    public IDictionary<string, string> Execute(
        IEnumerable<LinkedService> linkedServices,
        IEnumerable<Dataset> datasets,
        Activity activity,
        IActivityLogger logger)
    {
        // get extended properties defined in activity JSON definition
        // (for example: SliceStart)
        DotNetActivity dotNetActivity = (DotNetActivity)activity.TypeProperties;
        string sliceStartString = dotNetActivity.ExtendedProperties["SliceStart"];
    
        // to log information, use the logger object
        // log all extended properties            
        IDictionary<string, string> extendedProperties = dotNetActivity.ExtendedProperties;
        logger.Write("Logging extended properties if any...");
        foreach (KeyValuePair<string, string> entry in extendedProperties)
        {
            logger.Write("<key:{0}> <value:{1}>", entry.Key, entry.Value);
        }
    
        // linked service for input and output data stores
        // in this example, same storage is used for both input/output
        AzureStorageLinkedService inputLinkedService;

        // get the input dataset
        Dataset inputDataset = datasets.Single(dataset => dataset.Name == activity.Inputs.Single().Name);
    
        // declare variables to hold type properties of input/output datasets
        AzureBlobDataset inputTypeProperties, outputTypeProperties;
        
        // get type properties from the dataset object
        inputTypeProperties = inputDataset.Properties.TypeProperties as AzureBlobDataset;
    
        // log linked services passed in linkedServices parameter
        // you will see two linked services of type: AzureStorage
        // one for input dataset and the other for output dataset 
        foreach (LinkedService ls in linkedServices)
            logger.Write("linkedService.Name {0}", ls.Name);
    
        // get the first Azure Storage linked service from linkedServices object
        // using First method instead of Single since we are using the same
        // Azure Storage linked service for input and output.
        inputLinkedService = linkedServices.First(
            linkedService =>
            linkedService.Name ==
            inputDataset.Properties.LinkedServiceName).Properties.TypeProperties
            as AzureStorageLinkedService;
    
        // get the connection string in the linked service
        string connectionString = inputLinkedService.ConnectionString;
    
        // get the folder path from the input dataset definition
        string folderPath = GetFolderPath(inputDataset);
        string output = string.Empty; // for use later.
    
        // create storage client for input. Pass the connection string.
        CloudStorageAccount inputStorageAccount = CloudStorageAccount.Parse(connectionString);
        CloudBlobClient inputClient = inputStorageAccount.CreateCloudBlobClient();
    
        // initialize the continuation token before using it in the do-while loop.
        BlobContinuationToken continuationToken = null;
        do
        {   // get the list of input blobs from the input storage client object.
            BlobResultSegment blobList = inputClient.ListBlobsSegmented(folderPath,
                                     true,
                                     BlobListingDetails.Metadata,
                                     null,
                                     continuationToken,
                                     null,
                                     null);
    
            // Calculate method returns the number of occurrences of
            // the search term (“Microsoft”) in each blob associated
               // with the data slice. definition of the method is shown in the next step.
    
            output = Calculate(blobList, logger, folderPath, ref continuationToken, "Microsoft");
    
        } while (continuationToken != null);
    
        // get the output dataset using the name of the dataset matched to a name in the Activity output collection.
        Dataset outputDataset = datasets.Single(dataset => dataset.Name == activity.Outputs.Single().Name);

        // get type properties for the output dataset
        outputTypeProperties = outputDataset.Properties.TypeProperties as AzureBlobDataset;
    
        // get the folder path from the output dataset definition
        folderPath = GetFolderPath(outputDataset);

        // log the output folder path   
        logger.Write("Writing blob to the folder: {0}", folderPath);
    
        // create a storage object for the output blob.
        CloudStorageAccount outputStorageAccount = CloudStorageAccount.Parse(connectionString);
        // write the name of the file.
        Uri outputBlobUri = new Uri(outputStorageAccount.BlobEndpoint, folderPath + "/" + GetFileName(outputDataset));
    
        // log the output file name
        logger.Write("output blob URI: {0}", outputBlobUri.ToString());

        // create a blob and upload the output text.
        CloudBlockBlob outputBlob = new CloudBlockBlob(outputBlobUri, outputStorageAccount.Credentials);
        logger.Write("Writing {0} to the output blob", output);
        outputBlob.UploadText(output);
    
        // The dictionary can be used to chain custom activities together in the future.
        // This feature is not implemented yet, so just return an empty dictionary.  
    
        return new Dictionary<string, string>();
    }
    ```
9. Voeg de volgende methoden: 

    ```csharp
    /// <summary>
    /// Gets the folderPath value from the input/output dataset.
    /// </summary>
    
    private static string GetFolderPath(Dataset dataArtifact)
    {
        if (dataArtifact == null || dataArtifact.Properties == null)
        {
            return null;
        }

        // get type properties of the dataset   
        AzureBlobDataset blobDataset = dataArtifact.Properties.TypeProperties as AzureBlobDataset;
        if (blobDataset == null)
        {
            return null;
        }
    
        // return the folder path found in the type properties
        return blobDataset.FolderPath;
    }
    
    /// <summary>
    /// Gets the fileName value from the input/output dataset.   
    /// </summary>
    
    private static string GetFileName(Dataset dataArtifact)
    {
        if (dataArtifact == null || dataArtifact.Properties == null)
        {
            return null;
        }
    
        // get type properties of the dataset
        AzureBlobDataset blobDataset = dataArtifact.Properties.TypeProperties as AzureBlobDataset;
        if (blobDataset == null)
        {
            return null;
        }
    
        // return the blob/file name in the type properties
        return blobDataset.FileName;
    }
    
    /// <summary>
    /// Iterates through each blob (file) in the folder, counts the number of instances of search term in the file,
    /// and prepares the output text that is written to the output blob.
    /// </summary>
    
    public static string Calculate(BlobResultSegment Bresult, IActivityLogger logger, string folderPath, ref BlobContinuationToken token, string searchTerm)
    {
        string output = string.Empty;
        logger.Write("number of blobs found: {0}", Bresult.Results.Count<IListBlobItem>());
        foreach (IListBlobItem listBlobItem in Bresult.Results)
        {
            CloudBlockBlob inputBlob = listBlobItem as CloudBlockBlob;
            if ((inputBlob != null) && (inputBlob.Name.IndexOf("$$$.$$$") == -1))
            {
                string blobText = inputBlob.DownloadText(Encoding.ASCII, null, null, null);
                logger.Write("input blob text: {0}", blobText);
                string[] source = blobText.Split(new char[] { '.', '?', '!', ' ', ';', ':', ',' }, StringSplitOptions.RemoveEmptyEntries);
                var matchQuery = from word in source
                                 where word.ToLowerInvariant() == searchTerm.ToLowerInvariant()
                                 select word;
                int wordCount = matchQuery.Count();
                output += string.Format("{0} occurrences(s) of the search term \"{1}\" were found in the file {2}.\r\n", wordCount, searchTerm, inputBlob.Name);
            }
        }
        return output;
    }
    ```

    De methode GetFolderPath retourneert het pad naar de map die de gegevensset naar wijst en de getfilename,-methode retourneert de naam van de blob /-bestand dat de gegevensset naar wijst. Als u folderPath hebt gedefinieerd met behulp van de variabelen zoals {Year}, {Month}, {Day} enz., de methode retourneert de tekenreeks als deze is zonder deze te vervangen door waarden van de runtime. Zie [toegang tot uitgebreide eigenschappen](#access-extended-properties) sectie voor meer informatie over het openen van de slicestart-waarde, SliceEnd, enzovoort.    

    ```JSON
    "name": "InputDataset",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "fileName": "file.txt",
            "folderPath": "adftutorial/inputfolder/",
    ```

    De methode Calculate berekent het aantal exemplaren van het sleutelwoord Microsoft in de invoerbestanden (blobs in de map). De zoekterm ('Microsoft') is vastgelegd in de code.
10. Het project compileren. Klik op **bouwen** in het menu en klik op **Build Solution**.

    > [!IMPORTANT]
    > Versie van .NET Framework als doelframework voor uw project instellen 4.5.2: met de rechtermuisknop op het project en klik op **eigenschappen** om in te stellen van de doelframework. Data Factory biedt geen ondersteuning voor aangepaste activiteiten die zijn samengesteld op basis van .NET Framework-versies hoger is dan 4.5.2.

11. Start **Windows Explorer**, en navigeer naar **bin\debug** of **bin\release** map, afhankelijk van het type van de build.
12. Maak een zip-bestand **MyDotNetActivity.zip** die bevat alle binaire bestanden in de <project folder>\bin\Debug map. Bevatten de **MyDotNetActivity.pdb** bestand, zodat u aanvullende informatie zoals regelnummer in de broncode waardoor het probleem krijgen als er een fout opgetreden is. 

    > [!IMPORTANT]
    > Alle bestanden in het zip-bestand voor de aangepaste activiteit moeten zich op het **hoogste niveau** bevinden, zonder submappen.

    ![Binaire uitvoerbestanden](./media/data-factory-use-custom-activities/Binaries.png)
14. Maak een blobcontainer met de naam **customactivitycontainer** als deze niet al bestaat. 
15. MyDotNetActivity.zip uploaden als een blob naar de customactivitycontainer in een **voor algemeen gebruik** Azure blob-opslag (niet warme/koude Blob storage) waarnaar wordt verwezen door AzureStorageLinkedService.  

> [!IMPORTANT]
> Als u deze activiteit .NET project toevoegen aan een oplossing in Visual Studio met een Data Factory-project en voeg een verwijzing naar .NET-activiteit-project van de Data Factory-toepassingsproject, hoeft u niet om uit te voeren van de laatste twee stappen voor het handmatig maken van het ZIP-bestand bestands- en uploaden naar de algemene Azure blob-opslag. Wanneer u met behulp van Visual Studio Data Factory-entiteiten publiceert, worden deze stappen automatisch uitgevoerd door het publicatieproces. Zie voor meer informatie, [Data Factory-project in Visual Studio](#data-factory-project-in-visual-studio) sectie.

## <a name="create-a-pipeline-with-custom-activity"></a>Een pijplijn maken met aangepaste activiteit
U hebt gemaakt van een aangepaste activiteit en het zip-bestand met de binaire bestanden geüpload naar een blobcontainer in een **voor algemeen gebruik** Azure Storage-Account. In deze sectie maakt maken u een Azure data factory met een pijplijn met behulp van de aangepaste activiteit.

De invoergegevensset voor de aangepaste activiteit vertegenwoordigt blobs (bestanden) in de map customactivityinput van de container adftutorial in de blob-opslag. De uitvoergegevensset voor de activiteit vertegenwoordigt gegevens in blobs in de map customactivityoutput van de container adftutorial in de blob-opslag.

Maak **bestand.txt** -bestand met de volgende inhoud en upload het naar **customactivityinput** map van de **adftutorial** container. De container adftutorial maken als deze niet al bestaat. 

```
test custom activity Microsoft test custom activity Microsoft
```

De map invoer komt overeen met een segment in Azure Data Factory, zelfs als de map twee of meer bestanden heeft. Wanneer elk segment wordt verwerkt door de pijplijn, wordt de aangepaste activiteit doorloopt de blobs in de map invoer voor dat segment.

Ziet u een bestand met in de map adftutorial\customactivityoutput uitvoer met een of meer regels (hetzelfde als het aantal blobs in de map invoer):

```
2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2016-11-16-00/file.txt.
```


Hier volgen de stappen die u in deze sectie uitvoeren:

1. Maak een **gegevensfactory**.
2. Maak **gekoppelde services** voor de Azure Batch-pool van VM's op die de aangepaste activiteit wordt uitgevoerd en de Azure-opslag waarin de invoer/uitvoer-blobs.
3. Maken van de invoer en uitvoer **gegevenssets** die staan voor invoer en uitvoer van de aangepaste activiteit.
4. Maak een **pijplijn** die gebruikmaakt van de aangepaste activiteit.

> [!NOTE]
> Maak de **bestand.txt** en dit uploaden naar een blob-container, als u dat nog niet hebt gedaan. Zie de instructies in de voorgaande sectie.   

### <a name="step-1-create-the-data-factory"></a>Stap 1: Maak de data factory
1. Na het aanmelden bij Azure portal, voer de volgende stappen uit:
   1. Klik op **een resource maken** in het menu links.
   2. Klik op **gegevens en analyses** in de **nieuw** blade.
   3. Klik op de blade **Gegevensanalyse** op **Gegevensfactory**.
   
    ![Menu nieuwe Azure Data Factory](media/data-factory-use-custom-activities/new-azure-data-factory-menu.png)
2. In de **nieuwe data factory** blade Voer **CustomActivityFactory** voor de naam. De naam van de Azure-gegevensfactory moet wereldwijd uniek zijn. Als u de foutmelding: **Data factory name 'CustomActivityFactory' is niet beschikbaar**, wijzigt u de naam van de data factory (bijvoorbeeld **yournameCustomActivityFactory**) en probeer het opnieuw.

    ![Nieuwe Azure Data Factory-blade](media/data-factory-use-custom-activities/new-azure-data-factory-blade.png)
3. Klik op **GROEPSNAAM voor ACCOUNTRESOURCES**, en selecteer een bestaande resourcegroep of maak een resourcegroep.
4. Controleer of u de juiste **abonnement** en **regio** waar u de data factory moet worden gemaakt.
5. Klik op de blade **Nieuwe gegevensfactory** op **Maken**.
6. U ziet de data factory die worden gemaakt in de **Dashboard** van de Azure-portal.
7. Nadat de gegevensfactory is gemaakt, ziet u de Data Factory-blade, waarin u de inhoud van de data factory.
    
    ![Blade Gegevensfactory](media/data-factory-use-custom-activities/data-factory-blade.png)

### <a name="step-2-create-linked-services"></a>Stap 2: Maak gekoppelde services
Met gekoppelde services worden gegevensarchieven of compute-services gekoppeld aan een Azure Data Factory. In deze stap koppelt u uw Azure Storage-account en de Azure Batch-account aan uw data factory.

#### <a name="create-azure-storage-linked-service"></a>Een gekoppelde Azure Storage-service maken
1. Klik op de **auteur en implementeer** tegel op de **DATA FACTORY** blade voor **CustomActivityFactory**. U ziet de Data Factory Editor.
2. Klik op **nieuw gegevensarchief** op de opdracht en kies **Azure storage**. U ziet het JSON-script voor het maken van een gekoppelde Azure Storage-service in de editor.
    
    ![Nieuwe gegevensopslag - Azure Storage](media/data-factory-use-custom-activities/new-data-store-menu.png)
3. Vervang `<accountname>` met de naam van uw Azure storage-account en `<accountkey>` door de toegangssleutel van het Azure storage-account. Zie [Toegangssleutels voor opslag weergeven, kopiëren en opnieuw genereren](../../storage/common/storage-account-manage.md#access-keys) voor meer informatie over het verkrijgen van uw toegangssleutel voor opslag.

    ![Azure Storage service leuk vinden](media/data-factory-use-custom-activities/azure-storage-linked-service.png)
4. Klik op de opdrachtbalk op **Implementeren** om de gekoppelde service te implementeren.

#### <a name="create-azure-batch-linked-service"></a>Gekoppelde Azure-Batch-service maken
1. In de Data Factory-Editor, klikt u op **... Meer** Klik op de opdrachtbalk op **nieuwe berekening**, en selecteer vervolgens **Azure Batch** in het menu.

    ![Nieuwe berekening - Azure Batch](media/data-factory-use-custom-activities/new-azure-compute-batch.png)
2. De volgende wijzigingen aanbrengen in de JSON-script:

   1. Geef de naam van de Azure Batch-account voor de **accountName** eigenschap. De **URL** uit de **blade Azure Batch-account** is in de volgende indeling: `http://accountname.region.batch.azure.com`. Voor de **batchUri** eigenschap in de JSON die u wilt verwijderen `accountname.` door de URL en gebruik de `accountname` voor de `accountName` JSON-eigenschap.
   2. Geef de sleutel van de Azure Batch-account voor de **accessKey** eigenschap.
   3. Geef de naam van de groep die u hebt gemaakt als onderdeel van de vereisten voor de **poolName** eigenschap. U kunt ook de ID van de pool in plaats van de naam van de pool opgeven.
   4. Geef de Azure Batch-URI voor de **batchUri** eigenschap. Voorbeeld: `https://westus.batch.azure.com`.  
   5. Geef de **AzureStorageLinkedService** voor de **linkedServiceName** eigenschap.

        ```json
        {
         "name": "AzureBatchLinkedService",
         "properties": {
           "type": "AzureBatch",
           "typeProperties": {
             "accountName": "myazurebatchaccount",
             "batchUri": "https://westus.batch.azure.com",
             "accessKey": "<yourbatchaccountkey>",
             "poolName": "myazurebatchpool",
             "linkedServiceName": "AzureStorageLinkedService"
           }
         }
        }
        ```

       Voor de **poolName** eigenschap, kunt u ook de ID van de pool in plaats van de naam van de pool opgeven.

    

### <a name="step-3-create-datasets"></a>Stap 3: Maak gegevenssets
In deze stap maakt u gegevenssets voor invoer-en uitvoergegevens.

#### <a name="create-input-dataset"></a>Invoergegevensset maken
1. In de **Editor** voor de Data Factory, klikt u op **... Meer** Klik op de opdrachtbalk op **nieuwe gegevensset**, en selecteer vervolgens **Azure Blob-opslag** uit de vervolgkeuzelijst.
2. Vervang de JSON in het rechterdeelvenster met de volgende JSON-fragment:

    ```json
    {
     "name": "InputDataset",
     "properties": {
         "type": "AzureBlob",
         "linkedServiceName": "AzureStorageLinkedService",
         "typeProperties": {
             "folderPath": "adftutorial/customactivityinput/",
             "format": {
                 "type": "TextFormat"
             }
         },
         "availability": {
             "frequency": "Hour",
             "interval": 1
         },
         "external": true,
         "policy": {}
     }
    }
    ```

   Maakt u een pijplijn verderop in dit scenario met begintijd: 2016-11-16T00:00:00Z-en eindtijd: 2016-11-16T05:00:00Z. Deze is gepland voor het produceren van gegevens per uur, dus er vijf i/o-segmenten zijn (tussen **00**: 00:00 -> **05**: 00:00).

   De **frequentie** en **interval** voor de invoergegevensset is ingesteld op **uur** en **1**, wat betekent dat het invoersegment beschikbaar per uur is. In dit voorbeeld is hetzelfde bestand (bestand.txt) in de intputfolder.

   Hier volgen de begintijden voor elk segment, die wordt vertegenwoordigd door de systeemvariabele slicestart-waarde in de bovenstaande JSON-codefragment.
3. Klik op **implementeren** op de werkbalk om te maken en implementeren van de **InputDataset**. Controleer of u **TABEL IS GEMAAKT** ziet op de titelbalk van de editor.

#### <a name="create-an-output-dataset"></a>Een uitvoergegevensset maken
1. In de **Data Factory-editor**, klikt u op **... Meer** Klik op de opdrachtbalk op **nieuwe gegevensset**, en selecteer vervolgens **Azure Blob-opslag**.
2. Vervang de JSON-script in het rechterdeelvenster met de volgende JSON-script:

    ```JSON
    {
        "name": "OutputDataset",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "fileName": "{slice}.txt",
                "folderPath": "adftutorial/customactivityoutput/",
                "partitionedBy": [
                    {
                        "name": "slice",
                        "value": {
                            "type": "DateTime",
                            "date": "SliceStart",
                            "format": "yyyy-MM-dd-HH"
                        }
                    }
                ]
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            }
        }
    }
    ```

     Uitvoerlocatie van de is **adftutorial/customactivityoutput/** en naam van het uitvoerbestand is jjjj-MM-dd-HH.txt waarbij JJJJ-MM-dd HH is van het jaar, de maand, de datum en het uur van het segment wordt geproduceerd. Zie [referentie voor ontwikkelaars] [ adf-developer-reference] voor meer informatie.

    Een uitvoer-blob/bestand is gegenereerd voor elke invoersegment. Hier ziet u hoe een uitvoerbestand voor elk segment wordt genoemd. De uitvoerbestanden worden gegenereerd in een map voor uitvoer: **adftutorial\customactivityoutput**.

   | Segment | Begintijd | Uitvoerbestand |
   |:--- |:--- |:--- |
   | 1 |2016-11-16T00:00:00 |2016-11-16-00.txt |
   | 2 |2016-11-16T01:00:00 |2016-11-16-01.txt |
   | 3 |2016-11-16T02:00:00 |2016-11-16-02.txt |
   | 4 |2016-11-16T03:00:00 |2016-11-16-03.txt |
   | 5 |2016-11-16T04:00:00 |2016-11-16-04.txt |

    Houd er rekening mee dat alle bestanden in een map invoer deel van een segment met de uitmaken hierboven genoemde uitvoertijd. Wanneer dit segment wordt verwerkt, wordt de aangepaste activiteit scant via elk bestand en maakt een regel in het uitvoerbestand met het aantal exemplaren van zoekterm ('Microsoft'). Als er drie bestanden in de inputfolder, er zijn drie regels in het uitvoerbestand voor elk segment per uur: 2016-11-16-00.txt 2016-11-16:01:00:00.txt, enzovoort.
3. Implementatie van de **OutputDataset**, klikt u op **implementeren** op de opdrachtbalk.

### <a name="create-and-run-a-pipeline-that-uses-the-custom-activity"></a>Een pijplijn die gebruikmaakt van de aangepaste activiteit maken en uitvoeren
1. In de Data Factory-Editor, klikt u op **... Meer**, en selecteer vervolgens **nieuwe pijplijn** op de opdrachtbalk. 
2. Vervang de JSON in het rechterdeelvenster met de volgende JSON-script:

    ```JSON
    {
      "name": "ADFTutorialPipelineCustom",
      "properties": {
        "description": "Use custom activity",
        "activities": [
          {
            "Name": "MyDotNetActivity",
            "Type": "DotNetActivity",
            "Inputs": [
              {
                "Name": "InputDataset"
              }
            ],
            "Outputs": [
              {
                "Name": "OutputDataset"
              }
            ],
            "LinkedServiceName": "AzureBatchLinkedService",
            "typeProperties": {
              "AssemblyName": "MyDotNetActivity.dll",
              "EntryPoint": "MyDotNetActivityNS.MyDotNetActivity",
              "PackageLinkedService": "AzureStorageLinkedService",
              "PackageFile": "customactivitycontainer/MyDotNetActivity.zip",
              "extendedProperties": {
                "SliceStart": "$$Text.Format('{0:yyyyMMddHH-mm}', Time.AddMinutes(SliceStart, 0))"
              }
            },
            "Policy": {
              "Concurrency": 2,
              "ExecutionPriorityOrder": "OldestFirst",
              "Retry": 3,
              "Timeout": "00:30:00",
              "Delay": "00:00:00"
            }
          }
        ],
        "start": "2016-11-16T00:00:00Z",
        "end": "2016-11-16T05:00:00Z",
        "isPaused": false
      }
    }
    ```

    Houd rekening met de volgende punten:

   * **Gelijktijdigheid** is ingesteld op **2** zodat twee segmenten parallel door 2 virtuele machines in de Azure Batch-pool verwerkt worden.
   * Er is één activiteit in het gedeelte activiteiten en is van het type: **DotNetActivity**.
   * **AssemblyName** is ingesteld op de naam van het DLL-bestand: **MyDotnetActivity.dll**.
   * **EntryPoint** is ingesteld op **MyDotNetActivityNS.MyDotNetActivity**.
   * **PackageLinkedService** is ingesteld op **AzureStorageLinkedService** die verwijst naar de blob-opslag met de aangepaste activiteit zip-bestand. Als u van andere Azure Storage-accounts voor i/o-bestanden en de aangepaste activiteit zip-bestand gebruikmaakt, maakt u een andere gekoppelde Azure Storage-service. In dit artikel wordt ervan uitgegaan dat u van hetzelfde Azure Storage-account gebruikmaakt.
   * **PackageFile** is ingesteld op **customactivitycontainer/MyDotNetActivity.zip**. Het is in de indeling: containerforthezip/nameofthezip.zip.
   * De aangepaste activiteit neemt **InputDataset** als invoer en **OutputDataset** als uitvoer.
   * De eigenschap linkedServiceName van de aangepaste activiteit verwijst naar de **AzureBatchLinkedService**, waarin staat dat Azure Data Factory die de aangepaste activiteit moet worden uitgevoerd op virtuele machines in Azure Batch.
   * **isPaused** eigenschap is ingesteld op **false** standaard. De pijplijn wordt onmiddellijk uitgevoerd in dit voorbeeld omdat de segmenten in het verleden. U kunt deze eigenschap instellen op true als u wilt onderbreken van de pijplijn en opnieuw ingesteld op false om op te starten.
   * De **start** tijd en **end** tijden zijn **vijf** uur uit elkaar liggen en de segmenten worden per uur, geproduceerd, zodat de vijf segmenten worden geproduceerd door de pijplijn.
3. Voor het implementeren van de pijplijn, klikt u op **implementeren** op de opdrachtbalk.

### <a name="monitor-the-pipeline"></a>De pijplijn bewaken
1. Klik in de Data Factory-blade in Azure portal op **Diagram**.

    ![Tegel Diagram](./media/data-factory-use-custom-activities/DataFactoryBlade.png)
2. Klik in de diagramweergave op de OutputDataset.

    ![Diagramweergave](./media/data-factory-use-custom-activities/diagram.png)
3. U ziet dat de vijf uitvoersegmenten in de status gereed heeft worden. Als ze zich niet in de status gereed heeft, nog niet hebt ze nog zijn geproduceerd. 

   ![Uitvoersegmenten](./media/data-factory-use-custom-activities/OutputSlices.png)
4. Controleer of dat de uitvoerbestanden worden gegenereerd in de blob-opslag in de **adftutorial** container.

   ![uitvoer van de aangepaste activiteit][image-data-factory-output-from-custom-activity]
5. Als u het uitvoerbestand opent, ziet u de uitvoer is vergelijkbaar met de volgende uitvoer:

    ```
    2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2016-11-16-00/file.txt.
    ```
6. Gebruik de [Azure-portal] [ azure-preview-portal] of Azure PowerShell-cmdlets voor het bewaken van uw data factory, pijplijnen en gegevenssets. U kunt zien dat berichten van de **ActivityLogger** in de code voor de aangepaste activiteit in de logboeken (specifiek user-0.log) die u kunt downloaden vanuit de portal of met behulp van cmdlets.

   ![Logboeken downloaden die van de aangepaste activiteit][image-data-factory-download-logs-from-custom-activity]

Zie [pijplijnen bewaken en beheren](data-factory-monitor-manage-pipelines.md) voor gedetailleerde stappen voor het bewaken van pijplijnen en gegevenssets.      

## <a name="data-factory-project-in-visual-studio"></a>Data Factory-project in Visual Studio  
U kunt maken en publiceren van Data Factory-entiteiten met behulp van Visual Studio in plaats van Azure portal. Voor informatie over het maken gedetailleerde en publiceren van Data Factory-entiteiten met behulp van Visual Studio, Zie [bouw uw eerste pijplijn met Visual Studio](data-factory-build-your-first-pipeline-using-vs.md) en [gegevens kopiëren van Azure-Blob naar Azure SQL](data-factory-copy-activity-tutorial-using-visual-studio.md) de artikelen.

Voer de volgende aanvullende stappen als u Data Factory-project in Visual Studio maakt:
 
1. De Data Factory-project toevoegen aan de Visual Studio-oplossing die de aangepaste activiteit-project bevat. 
2. Voeg een verwijzing naar de .NET-activiteit-project van de Data Factory-project. Met de rechtermuisknop op Data Factory-project, wijs **toevoegen**, en klik vervolgens op **verwijzing**. 
3. In de **verwijzing toevoegen** in het dialoogvenster, selecteer de **MyDotNetActivity** project en klik op **OK**.
4. Bouw en publiceren van de oplossing.

    > [!IMPORTANT]
    > Wanneer u Data Factory-entiteiten publiceert, een zip-bestand automatisch voor u is gemaakt en geüpload naar de blob-container: customactivitycontainer. Als de blob-container niet bestaat, wordt deze automatisch gemaakt te.  


## <a name="data-factory-and-batch-integration"></a>Integratie van Data Factory en Batch
De Data Factory-service maakt een taak in Azure Batch met de naam: **adf-poolname: taak-xxx**. Klik op **taken** in het menu links. 

![Azure Data Factory - Batch-taken](media/data-factory-use-custom-activities/data-factory-batch-jobs.png)

Een taak wordt gemaakt voor elke uitvoering van de activiteit van een segment. Als er vijf segmenten worden verwerkt zijn, worden de vijf taken gemaakt in deze taak. Als er meerdere rekenknooppunten in de Batch-pool, kunnen twee of meer segmenten parallel worden uitgevoerd. Als het maximum aantal taken per knooppunt is ingesteld op > 1, kunt u ook meer dan één segment die worden uitgevoerd op de dezelfde compute hebben.

![Azure Data Factory - Batch-taken](media/data-factory-use-custom-activities/data-factory-batch-job-tasks.png)

Het volgende diagram illustreert de relatie tussen een Azure Data Factory en Batch-taken.

![Data Factory en Batch](./media/data-factory-use-custom-activities/DataFactoryAndBatch.png)

## <a name="troubleshoot-failures"></a>Problemen oplossen met mislukte
Het oplossen van bestaat uit enkele basistechnieken:

1. Als u de volgende fout ziet, kunt u een warme/koude blob-opslag gebruiken in plaats van een algemeen Azure blob-opslag. Upload het zipbestand naar een **algemeen Azure Storage-Account**. 
 
    ```
    Error in Activity: Job encountered scheduling error. Code: BlobDownloadMiscError Category: ServerError Message: Miscellaneous error encountered while downloading one of the specified Azure Blob(s).
    ``` 
2. Als u de volgende fout ziet, controleert u of de naam van de klasse in het bestand CS komt overeen met de naam die u hebt opgegeven voor de **EntryPoint** eigenschap in de pijplijn-JSON. In het scenario, de naam van de klasse is: MyDotNetActivity en het ingangspunt in de JSON is: MyDotNetActivityNS. **MyDotNetActivity**.

    ```
    MyDotNetActivity assembly does not exist or doesn't implement the type Microsoft.DataFactories.Runtime.IDotNetActivity properly
    ```

   Als de namen overeen komen, controleert u of dat de binaire bestanden zich in de **hoofdmap** van het zip-bestand. Dat wil zeggen, wanneer u het zip-bestand opent, ziet u alle bestanden in de hoofdmap, niet in alle submappen.   
3. Als het invoersegment niet is ingesteld op **gereed**, Controleer of de invoermapstructuur juist en **bestand.txt** bestaat in de invoermappen.
3. In de **Execute** methode van de aangepaste activiteit, gebruik de **IActivityLogger** object om informatie die helpt u bij het oplossen van problemen te registreren. De geregistreerde berichten weergegeven in de logboekbestanden van de gebruiker (een of meer bestanden met de naam: user-0.log, user-1.log, user-2.log, enzovoort.).

   In de **OutputDataset** blade, klikt u op het segment voor de **GEGEVENSSEGMENT** blade voor dat segment. U ziet **uitvoeringen van activiteit** voor dat segment. Hier ziet u een activiteit die wordt uitgevoerd voor het segment. Als u in de opdrachtbalk op uitvoeren klikt, kunt u een andere activiteit die wordt uitgevoerd voor hetzelfde segment starten.

   Wanneer u klikt op de activiteit die wordt uitgevoerd, ziet u de **DETAILS uitvoering van activiteit** blade met een lijst van logboekbestanden. Ziet u geregistreerde berichten in het bestand user_0.log. Wanneer een fout optreedt, ziet u drie uitvoeringen van activiteit omdat het maximale aantal pogingen is ingesteld op 3 in de pijplijn/JSON van de activiteit. Wanneer u de uitvoering van activiteit klikt, ziet u de logboekbestanden die u bekijken kunt voor het oplossen van de fout.

   Klik in de lijst van de logboekbestanden van de **user-0.log**. In het rechterdeelvenster worden de resultaten van het gebruik van de **IActivityLogger.Write** methode. Als u alle berichten niet ziet, controleert u of er meer logboekbestanden met de naam: user_1.log, user_2.log enzovoort. Anders wordt de code is mogelijk niet na de laatste bericht in het logboek geregistreerd.

   Controleer daarnaast **system-0.log** voor elk systeem foutberichten en uitzonderingen.
4. Bevatten de **PDB** bestand in het zip-bestand, zodat de foutdetails beschikt over informatie zoals **aanroepstack** wanneer er een fout optreedt.
5. Alle bestanden in het zip-bestand voor de aangepaste activiteit moeten zich op het **hoogste niveau** bevinden, zonder submappen.
6. Zorg ervoor dat de **assemblyName** (MyDotNetActivity.dll), **entryPoint**(MyDotNetActivityNS.MyDotNetActivity), **packageFile** (customactivitycontainer / MyDotNetActivity.zip), en **packageLinkedService** (moet verwijzen naar de **voor algemeen gebruik**Azure blob-opslag met het zip-bestand) op de juiste waarden zijn ingesteld.
7. Als u een fout hebt hersteld en het segment opnieuw wilt verwerken, klikt u met de rechtermuisknop op het segment op de blade **OutputDataset** en klikt u op **Uitvoeren**.
8. Als u de volgende fout ziet, gebruikt u de Azure Storage-package van versie > 4.3.0. Data Factory-service starten is de versie 4.3 van WindowsAzure.Storage vereist. Zie [Appdomain isolatie](#appdomain-isolation) sectie voor een tijdelijke oplossing als u de nieuwere versie van Azure Storage-assembly moet gebruiken. 

    ```
    Error in Activity: Unknown error in module: System.Reflection.TargetInvocationException: Exception has been thrown by the target of an invocation. ---> System.TypeLoadException: Could not load type 'Microsoft.WindowsAzure.Storage.Blob.CloudBlob' from assembly 'Microsoft.WindowsAzure.Storage, Version=4.3.0.0, Culture=neutral, 
    ```

    Als u de 4.3.0 kunt versie van Azure Storage-package, verwijder de bestaande verwijzing naar Azure Storage-package van versie > 4.3.0. Voer de volgende opdracht uit vanuit NuGet Package Manager-Console. 

    ```PowerShell
    Install-Package WindowsAzure.Storage -Version 4.3.0
    ```

    Maak het project. Assembly van versie > 4.3.0 Azure.Storage uit de map bin\Debug verwijderen. Maak een zip-bestand met de binaire bestanden en het PDB-bestand. Het oude zip-bestand vervangen door deze gegevensset in de blob-container (customactivitycontainer). Opnieuw uitvoeren van de segmenten die niet zijn (met de rechtermuisknop op het segment, en klik op uitvoeren).   
8. De aangepaste activiteit maakt geen gebruik van de **app.config** -bestand van het pakket. Dus als uw code verbindingstekenreeksen van het configuratiebestand leest, werkt het niet tijdens runtime. De aanbevolen procedure als voor het opslaan van alle geheimen in met behulp van Azure Batch is een **Azure Key Vault**, een certificaat op basis van service-principal gebruiken om te beveiligen de **keyvault**, en distribueren van het certificaat aan Azure Batch groep van toepassingen. De aangepaste .NET-activiteit heeft dan in runtime toegang tot geheimen in de KeyVault. Deze oplossing is een algemene oplossing en kan worden geschaald naar elk type geheim, niet alleen een verbindingsreeks.

   Er is een eenvoudigere oplossing (maar niet aanbevolen): u kunt maken een **gekoppelde Azure SQL-service** met instellingen voor connection strings, maakt u een gegevensset die gebruikmaakt van de gekoppelde service en keten van de gegevensset als een dummy-invoergegevensset naar de aangepaste .NET-activiteit. U kunt vervolgens toegang tot de verbindingsreeks van de gekoppelde service in de code van de aangepaste activiteit.  

## <a name="update-custom-activity"></a>Aangepaste activiteit bijwerken
Als u de code voor de aangepaste activiteit bijwerkt, bouw het en upload het zipbestand met nieuwe binaire bestanden naar de blob-opslag.

## <a name="appdomain-isolation"></a>AppDomain isolatie
Zie [Cross AppDomain voorbeeld](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/CrossAppDomainDotNetActivitySample) die laat zien hoe u een aangepaste activiteit die niet tot assembly-versies die worden gebruikt door de Data Factory startprogramma voor apps beperkt is maken (voorbeeld: WindowsAzure.Storage verze 4.3.0, Newtonsoft.Json v6.0.x, enz.).

## <a name="access-extended-properties"></a>Toegang tot uitgebreide eigenschappen
U kunt aangeven dat uitgebreide eigenschappen in de activiteits-JSON zoals wordt weergegeven in het volgende voorbeeld:

```JSON
"typeProperties": {
  "AssemblyName": "MyDotNetActivity.dll",
  "EntryPoint": "MyDotNetActivityNS.MyDotNetActivity",
  "PackageLinkedService": "AzureStorageLinkedService",
  "PackageFile": "customactivitycontainer/MyDotNetActivity.zip",
  "extendedProperties": {
    "SliceStart": "$$Text.Format('{0:yyyyMMddHH-mm}', Time.AddMinutes(SliceStart, 0))",
    "DataFactoryName": "CustomActivityFactory"
  }
},
```


In het voorbeeld zijn er twee uitgebreide eigenschappen: **SliceStart** en **DataFactoryName**. De waarde van de slicestart-waarde is gebaseerd op de systeemvariabele slicestart-waarde. Zie [systeemvariabelen](data-factory-functions-variables.md) voor een lijst met ondersteunde systeemvariabelen. De waarde voor DataFactoryName is vastgelegd CustomActivityFactory.

Voor toegang tot deze uitgebreide eigenschappen in de **Execute** methode, code gebruiken die vergelijkbaar is met de volgende code:

```csharp
// to get extended properties (for example: SliceStart)
DotNetActivity dotNetActivity = (DotNetActivity)activity.TypeProperties;
string sliceStartString = dotNetActivity.ExtendedProperties["SliceStart"];

// to log all extended properties                               
IDictionary<string, string> extendedProperties = dotNetActivity.ExtendedProperties;
logger.Write("Logging extended properties if any...");
foreach (KeyValuePair<string, string> entry in extendedProperties)
{
    logger.Write("<key:{0}> <value:{1}>", entry.Key, entry.Value);
}
```

## <a name="auto-scaling-of-azure-batch"></a>Automatische schaalaanpassing van Azure Batch
U kunt ook maken met een Azure Batch-pool met **voor automatisch schalen** functie. U kunt bijvoorbeeld een azure batch-pool maken met 0 toegewezen virtuele machines en een formule voor automatisch schalen op basis van het aantal in behandeling zijnde taken. 

De voorbeeldformule hier bereikt het volgende gedrag: wanneer de pool in eerste instantie wordt gemaakt, wordt 1 VM gestart. Metrische gegevens $PendingTasks definieert het aantal taken in die wordt uitgevoerd en actieve (in de wachtrij) staat.  De formule wordt het gemiddelde aantal in behandeling zijnde taken gevonden in de afgelopen 180 seconden en TargetDedicated dienovereenkomstig ingesteld. Het zorgt ervoor dat TargetDedicated nooit meer dan 25 VM's gaat. Dus als nieuwe taken worden verzonden en toepassingen automatisch meeschaalt en als de taken zijn voltooid, virtuele machines worden gratis één voor één en deze virtuele machines Hiermee verkleint u de automatische schaalaanpassing. startingNumberOfVMs en maxNumberofVMs kan worden aangepast aan uw behoeften.

Formule voor automatisch schalen:

``` 
startingNumberOfVMs = 1;
maxNumberofVMs = 25;
pendingTaskSamplePercent = $PendingTasks.GetSamplePercent(180 * TimeInterval_Second);
pendingTaskSamples = pendingTaskSamplePercent < 70 ? startingNumberOfVMs : avg($PendingTasks.GetSample(180 * TimeInterval_Second));
$TargetDedicated=min(maxNumberofVMs,pendingTaskSamples);
```

Zie [automatisch schalen rekenknooppunten in een Azure Batch-pool](../../batch/batch-automatic-scaling.md) voor meer informatie.

Als de groep met behulp van de standaard [autoScaleEvaluationInterval](https://msdn.microsoft.com/library/azure/dn820173.aspx), 15 tot 30 minuten aan de virtuele machine voorbereiden voordat u de aangepaste activiteit kan worden uitgevoerd door de Batch-service.  Als de groep van een andere autoScaleEvaluationInterval gebruikmaakt, kan de Batch-service autoScaleEvaluationInterval + 10 minuten duren.


## <a name="create-a-custom-activity-by-using-net-sdk"></a>Een aangepaste activiteit maken met behulp van .NET SDK
In de procedures in dit artikel, kunt u een data factory maken met een pijplijn met behulp van de aangepaste activiteit met behulp van de Azure-portal. De volgende code toont u hoe u de data factory maken met behulp van .NET SDK in plaats daarvan. U vindt meer informatie over het gebruik van de SDK om te maken via een programma pijplijnen in de [maken van een pijplijn met copy activity met behulp van .NET API](data-factory-copy-activity-tutorial-using-dotnet-api.md) artikel. 

```csharp
using System;
using System.Configuration;
using System.Collections.ObjectModel;
using System.Threading;
using System.Threading.Tasks;

using Microsoft.Azure;
using Microsoft.Azure.Management.DataFactories;
using Microsoft.Azure.Management.DataFactories.Models;
using Microsoft.Azure.Management.DataFactories.Common.Models;

using Microsoft.IdentityModel.Clients.ActiveDirectory;
using System.Collections.Generic;

namespace DataFactoryAPITestApp
{
    class Program
    {
        static void Main(string[] args)
        {
            // create data factory management client

            // TODO: replace ADFTutorialResourceGroup with the name of your resource group.
            string resourceGroupName = "ADFTutorialResourceGroup";

            // TODO: replace APITutorialFactory with a name that is globally unique. For example: APITutorialFactory04212017
            string dataFactoryName = "APITutorialFactory";

            TokenCloudCredentials aadTokenCredentials = new TokenCloudCredentials(
                ConfigurationManager.AppSettings["SubscriptionId"],
                GetAuthorizationHeader().Result);

            Uri resourceManagerUri = new Uri(ConfigurationManager.AppSettings["ResourceManagerEndpoint"]);

            DataFactoryManagementClient client = new DataFactoryManagementClient(aadTokenCredentials, resourceManagerUri);

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
                            // TODO: Replace <accountname> and <accountkey> with name and key of your Azure Storage account.
                            new AzureStorageLinkedService("DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>")
                        )
                    }
                }
            );

            // create a linked service for output data store: Azure SQL Database
            Console.WriteLine("Creating Azure Batch linked service");
            client.LinkedServices.CreateOrUpdate(resourceGroupName, dataFactoryName,
                new LinkedServiceCreateOrUpdateParameters()
                {
                    LinkedService = new LinkedService()
                    {
                        Name = "AzureBatchLinkedService",
                        Properties = new LinkedServiceProperties
                        (
                            // TODO: replace <batchaccountname> and <yourbatchaccountkey> with name and key of your Azure Batch account
                            new AzureBatchLinkedService("<batchaccountname>", "https://westus.batch.azure.com", "<yourbatchaccountkey>", "myazurebatchpool", "AzureStorageLinkedService")
                        )
                    }
                }
            );

            // create input and output datasets
            Console.WriteLine("Creating input and output datasets");
            string Dataset_Source = "InputDataset";
            string Dataset_Destination = "OutputDataset";

            Console.WriteLine("Creating input dataset of type: Azure Blob");
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
                                FolderPath = "adftutorial/customactivityinput/",
                                Format = new TextFormat()
                            },
                            External = true,
                            Availability = new Availability()
                            {
                                Frequency = SchedulePeriod.Hour,
                                Interval = 1,
                            },

                            Policy = new Policy() { }
                        }
                    }
                });

            Console.WriteLine("Creating output dataset of type: Azure Blob");
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
                                FileName = "{slice}.txt",
                                FolderPath = "adftutorial/customactivityoutput/",
                                PartitionedBy = new List<Partition>()
                                {
                                    new Partition()
                                    {
                                        Name = "slice",
                                        Value = new DateTimePartitionValue()
                                        {
                                            Date = "SliceStart",
                                            Format = "yyyy-MM-dd-HH"
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

            Console.WriteLine("Creating a custom activity pipeline");
            DateTime PipelineActivePeriodStartTime = new DateTime(2017, 3, 9, 0, 0, 0, 0, DateTimeKind.Utc);
            DateTime PipelineActivePeriodEndTime = PipelineActivePeriodStartTime.AddMinutes(60);
            string PipelineName = "ADFTutorialPipelineCustom";

            client.Pipelines.CreateOrUpdate(resourceGroupName, dataFactoryName,
                new PipelineCreateOrUpdateParameters()
                {
                    Pipeline = new Pipeline()
                    {
                        Name = PipelineName,
                        Properties = new PipelineProperties()
                        {
                            Description = "Use custom activity",

                            // Initial value for pipeline's active period. With this, you won't need to set slice status
                            Start = PipelineActivePeriodStartTime,
                            End = PipelineActivePeriodEndTime,
                            IsPaused = false,

                            Activities = new List<Activity>()
                            {
                                new Activity()
                                {
                                    Name = "MyDotNetActivity",
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
                                    LinkedServiceName = "AzureBatchLinkedService",
                                    TypeProperties = new DotNetActivity()
                                    {
                                        AssemblyName = "MyDotNetActivity.dll",
                                        EntryPoint = "MyDotNetActivityNS.MyDotNetActivity",
                                        PackageLinkedService = "AzureStorageLinkedService",
                                        PackageFile = "customactivitycontainer/MyDotNetActivity.zip",
                                        ExtendedProperties = new Dictionary<string, string>()
                                        {
                                            { "SliceStart", "$$Text.Format('{0:yyyyMMddHH-mm}', Time.AddMinutes(SliceStart, 0))"}
                                        }
                                    },
                                    Policy = new ActivityPolicy()
                                    {
                                        Concurrency = 2,
                                        ExecutionPriorityOrder = "OldestFirst",
                                        Retry = 3,
                                        Timeout = new TimeSpan(0,0,30,0),
                                        Delay = new TimeSpan()
                                    }
                                }
                            }
                        }
                    }
                });
        }

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
    }
}
```

## <a name="debug-custom-activity-in-visual-studio"></a>Fouten opsporen in aangepaste activiteit in Visual Studio
De [Azure Data Factory - lokale omgeving](https://github.com/gbrueckl/Azure.DataFactory.LocalEnvironment) voorbeeld op GitHub omvat een hulpprogramma waarmee u fouten opsporen in aangepaste .NET-activiteiten binnen Visual Studio.  


## <a name="sample-custom-activities-on-github"></a>Aangepaste activiteiten voorbeeld op GitHub
| Voorbeeld | Welke aangepaste activiteit doet |
| --- | --- |
| [Gegevens van HTTP-Downloader](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/HttpDataDownloaderSample). |Gegevens downloadt van een HTTP-eindpunt naar Azure Blob Storage met behulp van aangepaste C#-activiteit in Data Factory. |
| [Voorbeeld van Twitter-Sentimentanalyse](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/TwitterAnalysisSample-CustomC%23Activity) |Hiermee wordt een Azure ML-model en sentimentanalyse doen, score, voorspelling enzovoort. |
| [R-Script uitvoeren](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample). |R-script aanroept uit te voeren RScript.exe op uw HDInsight-cluster dat al R is geïnstalleerd op deze is. |
| [Cross-AppDomain .NET-activiteit](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/CrossAppDomainDotNetActivitySample) |Maakt gebruik van verschillende assembly-versies van resources die worden gebruikt door de Data Factory starten |
| [Een model in Azure Analysis Services opnieuw verwerken](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/AzureAnalysisServicesProcessSample) |  Een model in Azure Analysis Services opnieuw verwerken. |

[batch-net-library]: ../../batch/batch-dotnet-get-started.md
[batch-create-account]: ../../batch/batch-account-create-portal.md
[batch-technical-overview]: ../../batch/batch-technical-overview.md
[batch-get-started]: ../../batch/batch-dotnet-get-started.md
[use-custom-activities]: data-factory-use-custom-activities.md
[troubleshoot]: data-factory-troubleshoot.md
[data-factory-introduction]: data-factory-introduction.md
[azure-powershell-install]: https://github.com/Azure/azure-sdk-tools/releases


[developer-reference]: http://go.microsoft.com/fwlink/?LinkId=516908
[cmdlet-reference]: http://go.microsoft.com/fwlink/?LinkId=517456

[new-azure-batch-account]: https://msdn.microsoft.com/library/mt125880.aspx
[new-azure-batch-pool]: https://msdn.microsoft.com/library/mt125936.aspx
[azure-batch-blog]: http://blogs.technet.com/b/windowshpc/archive/2014/10/28/using-azure-powershell-to-manage-azure-batch-account.aspx

[nuget-package]: http://go.microsoft.com/fwlink/?LinkId=517478
[adf-developer-reference]: http://go.microsoft.com/fwlink/?LinkId=516908
[azure-preview-portal]: https://portal.azure.com/

[adfgetstarted]: data-factory-copy-data-from-azure-blob-storage-to-sql-database.md
[hivewalkthrough]: data-factory-data-transformation-activities.md

[image-data-factory-output-from-custom-activity]: ./media/data-factory-use-custom-activities/OutputFilesFromCustomActivity.png

[image-data-factory-download-logs-from-custom-activity]: ./media/data-factory-use-custom-activities/DownloadLogsFromCustomActivity.png
