---
title: Verwerken van grote gegevenssets met behulp van Data Factory en Batch | Microsoft Docs
description: Beschrijft hoe het verwerken van grote hoeveelheden gegevens in een Azure Data Factory-pijplijn met behulp van parallelle verwerking mogelijkheden van Azure Batch.
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: 688b964b-51d0-4faa-91a7-26c7e3150868
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/15/2017
ms.author: spelluru
robots: noindex
ms.openlocfilehash: 2ceef65eaf195b605fada2f8dfe511fe33a5daa0
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/03/2017
---
# <a name="process-large-scale-datasets-using-data-factory-and-batch"></a>Grootschalige gegevenssets verwerken met Data Factory en Batch
> [!NOTE]
> Dit artikel is van toepassing op versie 1 van Data Factory, die algemeen beschikbaar is. Als u versie 2 van de Data Factory-service, die zich in de preview, Zie [aangepaste activiteiten in de Data Factory versie 2](../transform-data-using-dotnet-custom-activity.md).

In dit artikel beschrijft een architectuur van een Voorbeeldoplossing die wordt verplaatst en grootschalige gegevenssets in een automatische en geplande wijze verwerkt. Het bevat ook een end-to-end-stappen voor het implementeren van de oplossing met behulp van Azure Data Factory en Azure Batch.

In dit artikel is langer dan het typische artikel omdat deze een overzicht van een Voorbeeldoplossing voor het volledige bevat. Als u niet bekend met Batch- en Data Factory bent, kunt u lezen over deze services en hoe ze samen werken. Als u over de services weet en zijn ontwerpen/veranderd een oplossing, u kunt zich richten alleen op de [architectuur sectie](#architecture-of-sample-solution) van het artikel en als u een prototype of een oplossing ontwikkelt, kunt u ook uitproberen Stapsgewijze instructies in de [scenario](#implementation-of-sample-solution). Wij willen graag uw opmerkingen over deze inhoud en hoe u deze gebruiken.

Eerst gaan we kijken hoe Data Factory en Batch services helpen kunnen bij het verwerken van grote gegevenssets in de cloud.     

## <a name="why-azure-batch"></a>Waarom Azure Batch?
In Azure Batch kunt u grootschalige parallelle en High Performance Computing (HPC)-toepassingen efficiënt in de cloud uitvoeren. Dit is een platformservice die de uitvoering van rekenintensief werk op een beheerde verzameling van virtuele machines plant en automatisch het aantal rekenresources kan aanpassen aan de behoeften van uw jobs.

Met de Batch-service definieert u Azure-rekenresources om uw toepassingen parallel en op de juiste schaal uit te voeren. U kunt taken op aanvraag uitvoeren of geplande taken uitvoeren en u hoeft niet handmatig een HPC-cluster, individuele virtuele machines, virtuele netwerken of een complexe job- en taakplanningsinfrastructuur te maken, configureren en beheren.

Zie de volgende artikelen als u niet bekend met Azure Batch bent als het helpt bij het begrijpen van de architectuur/implementatie van de oplossing wordt beschreven in dit artikel.   

* [Basisbeginselen van Azure Batch](../../batch/batch-technical-overview.md)
* [Overzicht van de functies van Batch](../../batch/batch-api-basics.md)

(optioneel) Zie voor meer informatie over Azure Batch, de [leertraject voor Azure Batch](https://azure.microsoft.com/documentation/learning-paths/batch/).

## <a name="why-azure-data-factory"></a>Waarom Azure Data Factory?
Een Data Factory is een cloudgebaseerde gegevensintegratieservice waarmee de verplaatsing en transformatie van gegevens wordt beheerd en geautomatiseerd. Met de Data Factory-service, kunt u beheerde gegevenspijplijnen die gegevens verplaatsen van on-premises en in de cloud van de opgeslagen gegevens naar een gecentraliseerde gegevensopslag maken (bijvoorbeeld: Azure Blob-opslag), en gegevens met behulp van services zoals Azure HDInsight en Azure Machine Learning proces/transformatie. U kunt ook plannen gegevenspijplijnen uitvoeren in een geplande manier (elk uur, dagelijks, wekelijks, enz.) en een monitor en ze te beheren in één oogopslag problemen identificeren en actie ondernemen.

Zie de volgende artikelen als u niet bekend met Azure Data Factory bent als het helpt bij het begrijpen van de architectuur/implementatie van de oplossing wordt beschreven in dit artikel.  

* [Introductie van Azure Data Factory](data-factory-introduction.md)
* [Uw eerste pijplijn voor gegevens](data-factory-build-your-first-pipeline.md)   

(optioneel) Zie voor meer informatie over Azure Data Factory, de [leertraject voor Azure Data Factory](https://azure.microsoft.com/documentation/learning-paths/data-factory/).

## <a name="data-factory-and-batch-together"></a>Data Factory en in Batch samen
Data Factory bevat ingebouwde activiteiten zoals Kopieeractiviteit gegevens kopiëren of verplaatsen van een brongegevensarchief naar een doelgegevensopslagplaats en Hive-activiteit om gegevens te verwerken met behulp van Hadoop-clusters (HDInsight) in Azure. Zie [activiteiten voor gegevenstransformatie](data-factory-data-transformation-activities.md) voor een lijst van activiteiten voor gegevenstransformatie ondersteund.

Ook kunt u aangepaste .NET-activiteiten om te verplaatsen of gegevens met uw eigen logica verwerken en deze activiteiten worden uitgevoerd op een Azure HDInsight-cluster of op een Azure Batch-pool van virtuele machines maken. Wanneer u Azure Batch gebruikt, kunt u de toepassingen automatisch schalen configureren (toevoegen of verwijderen van virtuele machines op basis van de werkbelasting) op basis van een formule die u opgeeft.     

## <a name="architecture-of-sample-solution"></a>Architectuur van Voorbeeldoplossing
Hoewel de architectuur die wordt beschreven in dit artikel is bedoeld voor een eenvoudige oplossing, is het relevant zijn voor complexe scenario's zoals risico modellering van financiële diensten, verwerking van afbeeldingen en rendering en Toepassingsgeoriënteerde analyse.

Het diagram illustreert 1) hoe Data Factory ingedeeld verwerking en de verplaatsing van gegevens en 2) hoe de gegevens op een parallelle manier Azure Batch verwerkt. Downloaden en afdrukken van het diagram ter referentie (11 x 17. of A3-grootte): [HPC en gegevens orchestration met behulp van Azure Batch- en Data Factory](http://go.microsoft.com/fwlink/?LinkId=717686).

[![Diagram van grootschalige gegevensverwerking](./media/data-factory-data-processing-using-batch/image1.png)](http://go.microsoft.com/fwlink/?LinkId=717686)

De volgende lijst bevat de basisstappen van het proces. De oplossing omvat code en uitleg om de end-to-end-oplossing te bouwen.

1. **Azure Batch configureren met een pool van rekenknooppunten (virtuele machines)**. U kunt het aantal knooppunten en de grootte van elk knooppunt opgeven.
2. **Maak een Azure Data Factory-instantie** die is geconfigureerd met de entiteiten die Azure blob-opslag, Azure Batch compute-service, invoer-en uitvoergegevens en een werkstroom/pijplijn met activiteiten die verplaatsen en gegevens transformeren vertegenwoordigen.
3. **Maken van een aangepaste .NET-activiteit in de Data Factory-pijplijn**. De activiteit is uw gebruikerscode die wordt uitgevoerd op de Azure Batch-pool.
4. **Opslaan van grote hoeveelheden gegevens als blobs in Azure storage**. Gegevens worden in logische segmenten verdeeld (meestal door time).
5. **Data Factory kopieert de gegevens die wordt verwerkt parallel** naar de secundaire locatie.
6. **Data Factory wordt uitgevoerd voor de aangepaste activiteit die gebruikmaken van de toegewezen door Batch**. Data Factory kunt activiteiten tegelijkertijd worden uitgevoerd. Elke activiteit verwerkt een segment met gegevens. De resultaten worden opgeslagen in Azure storage.
7. **Data Factory het eindresultaat verplaatst naar een derde locatie**, voor distributie via een app of voor verdere verwerking door andere hulpprogramma's.

## <a name="implementation-of-sample-solution"></a>Implementatie van Voorbeeldoplossing
De Voorbeeldoplossing is opzettelijk eenvoudig en wordt aangegeven, zodat u het gebruik van Data Factory en Batch gezamenlijk gegevenssets verwerken. De oplossing telt het aantal exemplaren van een zoekterm ('Microsoft') gewoon in invoerbestanden georganiseerd in een tijdreeks. Het levert de uitvoerbestanden telt.

**Tijd**: als u bekend met de basisprincipes van Azure Data Factory en Batch bent en de hieronder vermelde vereisten hebt voltooid, schatten we deze oplossing 1 tot 2 uur in beslag neemt.

### <a name="prerequisites"></a>Vereisten
#### <a name="azure-subscription"></a>Azure-abonnement
Als u geen Azure-abonnement hebt, kunt u een gratis proefaccount maken binnen een paar minuten. Zie [gratis proefversie](https://azure.microsoft.com/pricing/free-trial/).

#### <a name="azure-storage-account"></a>Azure Storage-account
U kunt een Azure storage-account gebruiken voor het opslaan van de gegevens in deze zelfstudie. Als u geen Azure storage-account hebt, raadpleegt u [een opslagaccount maken](../../storage/common/storage-create-storage-account.md#create-a-storage-account). De oplossing maakt gebruik van blob-opslag.

#### <a name="azure-batch-account"></a>Azure Batch-account
Een Azure Batch account maken met de [Azure-portal](http://portal.azure.com/). Zie [maken en beheren van Azure Batch-account](../../batch/batch-account-create-portal.md). Noteer de sleutel Azure Batch-account en -account. U kunt ook [New-AzureRmBatchAccount](https://msdn.microsoft.com/library/mt603749.aspx) cmdlet om een Azure Batch-account te maken. Zie [aan de slag met Azure Batch PowerShell-cmdlets](../../batch/batch-powershell-cmdlets-get-started.md) voor gedetailleerde instructies over het gebruik van deze cmdlet.

De oplossing maakt gebruik van Azure Batch (indirect via een Azure Data Factory-pijplijn) om gegevens te verwerken in een parallelle manier op een pool van rekenknooppunten (een beheerde verzameling van virtuele machines).

#### <a name="azure-batch-pool-of-virtual-machines-vms"></a>Azure Batch-pool van virtuele machines (VM's)
Maak een **Azure Batch-pool** met ten minste 2 rekenknooppunten.

1. In de [Azure-portal](https://portal.azure.com), klikt u op **Bladeren** in het menu aan de linkerkant en klik op **Batch-Accounts**.
2. Selecteer uw Azure Batch-account openen de **Batch-Account** blade.
3. Klik op **Pools** tegel.
4. In de **Pools** blade, klik op de knop toevoegen op de werkbalk om een groep toevoegen.
   1. Geef een ID voor de groep (**Pool-ID**). Opmerking de **ID van de pool**; u deze nodig hebt bij het maken van de Data Factory-oplossing.
   2. Geef **Windows Server 2012 R2** voor de besturingssysteemgroep-instelling.
   3. Selecteer een **knooppunt prijscategorie**.
   4. Voer **2** als waarde voor de **doel toegewezen** instelling.
   5. Voer **2** als waarde voor de **maximum aantal taken per knooppunt** instelling.
   6. Klik op **OK** om de groep te maken.

#### <a name="azure-storage-explorer"></a>Azure Opslagverkenner
[Azure Storage Explorer 6 (hulpprogramma)](https://azurestorageexplorer.codeplex.com/) of [CloudXplorer](http://clumsyleaf.com/products/cloudxplorer) (van ClumsyLeaf Software). U gebruikt deze hulpprogramma's voor te bekijken en wijzigen van de gegevens in uw Azure Storage-projecten met inbegrip van de logboeken van uw cloud-gebaseerde toepassingen.

1. Maken van een container met de naam **mycontainer** met persoonlijke toegang (geen anonieme toegang)
2. Als u **CloudXplorer**, mappen en submappen maken met de volgende structuur:

   ![](./media/data-factory-data-processing-using-batch/image3.png)

   `Inputfolder`en `outputfolder` zijn op het hoogste niveau van de mappen in `mycontainer`. De `inputfolder` submappen met de datum / tijd stempels (jjjj-MM-DD-UU).

   Als u **Azure Opslagverkenner**, in de volgende stap moet u voor het uploaden van bestanden met namen: `inputfolder/2015-11-16-00/file.txt`, `inputfolder/2015-11-16-01/file.txt` enzovoort. Deze stap maakt automatisch de mappen.
3. Maak een tekstbestand **bestand.txt** op uw computer met inhoud met het sleutelwoord **Microsoft**. Bijvoorbeeld: 'aangepaste activiteit Microsoft test aangepaste activiteit Microsoft test'.
4. Upload het bestand naar de volgende invoer mappen in Azure blob-opslag.

   ![](./media/data-factory-data-processing-using-batch/image4.png)

   Als u **Azure Opslagverkenner**, upload het bestand **bestand.txt** naar **mycontainer**. Klik op **kopie** op de werkbalk om een kopie maken van de blob. In de **Blob kopiëren** wijziging van het dialoogvenster de **blob doelnaam** naar `inputfolder/2015-11-16-00/file.txt`. Herhaal deze stap maken `inputfolder/2015-11-16-01/file.txt`, `inputfolder/2015-11-16-02/file.txt`, `inputfolder/2015-11-16-03/file.txt`, `inputfolder/2015-11-16-04/file.txt` enzovoort. Deze actie wordt automatisch gemaakt voor de mappen.
5. Maken van een andere container met de naam: `customactivitycontainer`. U uploaden het aangepaste activiteit zip-bestand naar deze container.

#### <a name="visual-studio"></a>Visual Studio
Installeer Microsoft Visual Studio 2012 of later om de aangepaste Batch-activiteit moet worden gebruikt in de Data Factory-oplossing te maken.

### <a name="high-level-steps-to-create-the-solution"></a>De stappen op hoog niveau om de oplossing te maken
1. Maak een aangepaste activiteit die de gegevensverwerking logica bevat.
2. Maak een Azure data factory die gebruikmaakt van de aangepaste activiteit:

### <a name="create-the-custom-activity"></a>De aangepaste activiteit maken
De Data Factory aangepaste activiteit vormt de kern van de Voorbeeldoplossing van dit. De Voorbeeldoplossing gebruikt Azure Batch voor het uitvoeren van de aangepaste activiteit. Zie [aangepaste activiteiten gebruiken in een Azure Data Factory-pijplijn](data-factory-use-custom-activities.md) voor de algemene informatie aan de aangepaste activiteiten te ontwikkelen en gebruiken in Azure Data Factory-pijplijnen.

Voor het maken van een aangepaste .NET-activiteit die u kunt gebruiken in een Azure Data Factory-pijplijn, moet u maken een **.NET Class Library** project met een klasse die die implementeert **IDotNetActivity** interface. Deze interface heeft slechts één methode: **Execute**. Dit is de handtekening van de methode:

```csharp
public IDictionary<string, string> Execute(
            IEnumerable<LinkedService> linkedServices,
            IEnumerable<Dataset> datasets,
            Activity activity,
            IActivityLogger logger)
```

De methode heeft enkele belangrijke onderdelen die u nodig hebt om te begrijpen.

* De methode bevat vier parameters:

  1. **linkedServices**. Een inventariseerbare lijst met gekoppelde services die een koppeling i/o-gegevensbronnen (bijvoorbeeld: Azure Blob Storage) aan de gegevensfactory. In dit voorbeeld is er slechts één van de gekoppelde service van het type Azure Storage gebruikt voor invoer en uitvoer.
  2. **gegevenssets**. Dit is een inventariseerbare lijst van gegevenssets. Deze parameter kunt u de locaties en schema's die zijn gedefinieerd door de invoer- en uitvoergegevenssets ophalen.
  3. **activiteit**. Deze parameter geeft de huidige compute entiteit - in dit geval wordt een Azure Batch-service.
  4. **Berichtenlogboek**. Het logboek kunt u foutopsporing opmerkingen die surface schrijven als het logboek 'Gebruiker' voor de pijplijn.
* De methode retourneert een woordenlijst die kan worden gebruikt om een keten van aangepaste activiteiten in de toekomst. Deze functie is nog niet geïmplementeerd, kunt u dus een woordenboek leeg retourneren van de methode.

#### <a name="procedure-create-the-custom-activity"></a>Procedure: De aangepaste activiteit maken
1. Maak een .NET Class Library-project in Visual Studio.

   1. Start **Visual Studio 2012**/**2013-2015**.
   2. Klik op **File**, houd de muisaanwijzer op **New** en klik op **Project**.
   3. Vouw **sjablonen**, en selecteer **Visual C\#**. In dit scenario gebruikt u C\#, maar u kunt een andere taal .NET gebruiken voor het ontwikkelen van de aangepaste activiteit.
   4. Selecteer **Class Library** uit de lijst met projecttypen aan de rechterkant.
   5. Voer **MyDotNetActivity** voor de **naam**.
   6. Selecteer **C:\\ADF** voor de **locatie**. Maak de map **ADF** als deze niet bestaat.
   7. Klik op **OK** om het project te maken.
2. Klik op **Tools**, wijs **NuGet Package Manager** aan en klik op **Package Manager Console**.
3. In de **Package Manager Console**, de volgende opdracht om te importeren **Microsoft.Azure.Management.DataFactories**.

    ```powershell
    Install-Package Microsoft.Azure.Management.DataFactories
    ```
4. Importeer de **Azure Storage** NuGet-pakket in het project. Omdat u de Blob storage API in dit voorbeeld gebruiken moet u dit pakket.

    ```powershell
    Install-Package Azure.Storage
    ```
5. Voeg de volgende **met** richtlijnen naar het bronbestand in het project.

    ```csharp
    using System.IO;
    using System.Globalization;
    using System.Diagnostics;
    using System.Linq;
    
    using Microsoft.Azure.Management.DataFactories.Models;
    using Microsoft.Azure.Management.DataFactories.Runtime;
    
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Blob;
    ```
6. Wijzig de naam van de **naamruimte** naar **MyDotNetActivityNS**.

    ```csharp
    namespace MyDotNetActivityNS
    ```
7. Wijzig de naam van de klasse **MyDotNetActivity** en die afkomstig zijn uit de **IDotNetActivity** interface zoals hieronder wordt weergegeven.

    ```csharp
    public class MyDotNetActivity : IDotNetActivity
    ```
8. Implementeer (toevoegen) de **Execute** methode van de **IDotNetActivity** interface met de **MyDotNetActivity** klasse en kopieer de volgende voorbeeldcode naar de methode. Zie de [methode Execute](#execute-method) sectie voor uitleg van de logica in deze methode gebruikt.

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
    
       // declare types for input and output data stores
       AzureStorageLinkedService inputLinkedService;
    
       Dataset inputDataset = datasets.Single(dataset => dataset.Name == activity.Inputs.Single().Name);
    
       foreach (LinkedService ls in linkedServices)
           logger.Write("linkedService.Name {0}", ls.Name);
    
       // using First method instead of Single since we are using the same
       // Azure Storage linked service for input and output.
       inputLinkedService = linkedServices.First(
           linkedService =>
           linkedService.Name ==
           inputDataset.Properties.LinkedServiceName).Properties.TypeProperties
           as AzureStorageLinkedService;
    
       string connectionString = inputLinkedService.ConnectionString; // To create an input storage client.
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
           // with the data slice.
           //
           // definition of the method is shown in the next step.
           output = Calculate(blobList, logger, folderPath, ref continuationToken, "Microsoft");
    
       } while (continuationToken != null);
    
       // get the output dataset using the name of the dataset matched to a name in the Activity output collection.
       Dataset outputDataset = datasets.Single(dataset => dataset.Name == activity.Outputs.Single().Name);
    
       folderPath = GetFolderPath(outputDataset);
    
       logger.Write("Writing blob to the folder: {0}", folderPath);
    
       // create a storage object for the output blob.
       CloudStorageAccount outputStorageAccount = CloudStorageAccount.Parse(connectionString);
       // write the name of the file.
       Uri outputBlobUri = new Uri(outputStorageAccount.BlobEndpoint, folderPath + "/" + GetFileName(outputDataset));
    
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
9. De volgende methoden toevoegen aan de klasse. Deze methoden worden aangeroepen door de **Execute** methode. Het belangrijkste is dat de **Calculate** methode isoleert de code die elke blob doorlopen.

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
    
       AzureBlobDataset blobDataset = dataArtifact.Properties.TypeProperties as AzureBlobDataset;
       if (blobDataset == null)
       {
           return null;
       }
    
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
    
       AzureBlobDataset blobDataset = dataArtifact.Properties.TypeProperties as AzureBlobDataset;
       if (blobDataset == null)
       {
           return null;
       }
    
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
    De **GetFolderPath** methode retourneert het pad naar de map die de gegevensset naar verwijst en de **GetFileName** methode retourneert de naam van de blob of het bestand die de gegevensset naar verwijst.

    ```csharp

    "name": "InputDataset",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
            "fileName": "file.txt",
            "folderPath": "mycontainer/inputfolder/{Year}-{Month}-{Day}-{Hour}",
    ```

    De **Calculate** methode berekent het aantal exemplaren van het sleutelwoord **Microsoft** in de invoerbestanden (BLOB's in de map). De zoekterm ('Microsoft') is vastgelegd in de code.

1. Het project gecompileerd. Klik op **bouwen** in het menu en klik op **Build Solution**.
2. Start **Windows Verkenner**, en navigeer naar **bin\\debug** of **bin\\release** map afhankelijk van het type build.
3. Maak een zipbestand **MyDotNetActivity.zip** die bevat alle binaire bestanden in de  **\\bin\\Debug** map. U kunt de MyDotNetActivity bevatten. **pdb** bestand zodat u aanvullende informatie zoals regelnummer in de broncode die het probleem wordt veroorzaakt ophalen wanneer er een fout optreedt.

   ![](./media/data-factory-data-processing-using-batch/image5.png)
4. Uploaden **MyDotNetActivity.zip** als blob naar de blob-container: `customactivitycontainer` in de Azure-blobopslag die de **StorageLinkedService** gekoppelde service in de **ADFTutorialDataFactory** gebruikt. Maken van de blob-container `customactivitycontainer` als deze niet al bestaat.

#### <a name="execute-method"></a>De methode Execute
Deze sectie bevat meer informatie en opmerkingen over de code in de Execute-methode.

1. De leden voor doorloopt de invoerverzameling vindt u in de [Microsoft.WindowsAzure.Storage.Blob](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.aspx) naamruimte. De blob-verzameling doorloopt, moet u de **BlobContinuationToken** klasse. In wezen, moet u een do-tijdens lus aan het token als het mechanisme voor de lus wordt afgesloten. Zie voor meer informatie [hoe Blob storage gebruiken met .NET](../../storage/blobs/storage-dotnet-how-to-use-blobs.md). Hier ziet u een eenvoudige lus:

    ```csharp
    // Initialize the continuation token.
    BlobContinuationToken continuationToken = null;
    do
    {
    // Get the list of input blobs from the input storage client object.
    BlobResultSegment blobList = inputClient.ListBlobsSegmented(folderPath,
    
                         true,
                                   BlobListingDetails.Metadata,
                                   null,
                                   continuationToken,
                                   null,
                                   null);
    // Return a string derived from parsing each blob.
    
     output = Calculate(blobList, logger, folderPath, ref continuationToken, "Microsoft");
    
    } while (continuationToken != null);

    ```
   Raadpleeg de documentatie voor de [ListBlobsSegmented](https://msdn.microsoft.com/library/jj717596.aspx) methode voor meer informatie.
2. De code voor het werken met de reeks blobs logisch gaat binnen de do-tijdens lus. In de **Execute** methode, de komen-terwijl lus de lijst met BLOB's aan een methode met de naam geeft **Calculate**. De methode retourneert een string-variabele met de naam **uitvoer** die het resultaat van het via de blobs in het segment dat herhaald.

   Deze retourneert het aantal exemplaren van de zoekterm (**Microsoft**) in de blob die is doorgegeven aan de **Calculate** methode.

    ```csharp
    output += string.Format("{0} occurrences of the search term \"{1}\" were found in the file {2}.\r\n", wordCount, searchTerm, inputBlob.Name);
    ```
3. Eenmaal de **Calculate** methode dat doet, deze moet worden geschreven naar een nieuwe blob. Dus voor elke set van BLOB's verwerkt, kan een nieuwe blob met de resultaten worden geschreven. Voor het schrijven naar een nieuwe blob, moet u eerst de uitvoergegevensset vinden.

    ```csharp
    // Get the output dataset using the name of the dataset matched to a name in the Activity output collection.
    Dataset outputDataset = datasets.Single(dataset => dataset.Name == activity.Outputs.Single().Name);
    ```
4. De code ook een Help-methode aanroept: **GetFolderPath** voor het ophalen van het mappad (de containernaam voor opslag).

    ```csharp
    folderPath = GetFolderPath(outputDataset);
    ```
   De **GetFolderPath** werpt u het object gegevensset naar een AzureBlobDataSet met een eigenschap genaamd FolderPath.

    ```csharp
    AzureBlobDataset blobDataset = dataArtifact.Properties.TypeProperties as AzureBlobDataset;
    
    return blobDataset.FolderPath;
    ```
5. Het aanroepen van de code de **GetFileName** methode voor het ophalen van de bestandsnaam (blob-naam). De code is vergelijkbaar met de bovenstaande code voor het ophalen van pad naar de map.

    ```csharp
    AzureBlobDataset blobDataset = dataArtifact.Properties.TypeProperties as AzureBlobDataset;
    
    return blobDataset.FileName;
    ```
6. De naam van het bestand is geschreven door het maken van een URI-object. De URI-constructor gebruikt de **BlobEndpoint** eigenschap om de containernaam te retourneren. De naam van pad en bestandsnaam in de map worden toegevoegd aan het samenstellen van de blob-URI van de uitvoer.  

    ```csharp
    // Write the name of the file.
    Uri outputBlobUri = new Uri(outputStorageAccount.BlobEndpoint, folderPath + "/" + GetFileName(outputDataset));
    ```
7. De naam van het bestand is geschreven en nu kunt u de uitvoertekenreeks van de **Calculate** methode naar een nieuwe blob:

    ```csharp
    // Create a blob and upload the output text.
    CloudBlockBlob outputBlob = new CloudBlockBlob(outputBlobUri, outputStorageAccount.Credentials);
    logger.Write("Writing {0} to the output blob", output);
    outputBlob.UploadText(output);
    ```

### <a name="create-the-data-factory"></a>De gegevensfactory maken
In de [maken van de aangepaste activiteit](#create-the-custom-activity) sectie maakt u een aangepaste activiteit gemaakt en het zip-bestand met binaire bestanden en het PDB-bestand is geüpload naar een Azure blob-container. In deze sectie maakt u een Azure **gegevensfactory** met een **pijplijn** die gebruikmaakt van de **aangepaste activiteit**.

De invoergegevensset voor de aangepaste activiteit vertegenwoordigt de blobs (bestanden) in de invoermap (`mycontainer\\inputfolder`) in de blob-opslag. De uitvoergegevensset voor de activiteit vertegenwoordigt de uitvoer blobs in de map voor uitvoer (`mycontainer\\outputfolder`) in de blob-opslag.

Een of meer bestanden neerzetten in de invoer-mappen:

```
mycontainer -\> inputfolder
    2015-11-16-00
    2015-11-16-01
    2015-11-16-02
    2015-11-16-03
    2015-11-16-04
```

Neerzetten bijvoorbeeld één bestand (bestand.txt) met de volgende inhoud in elk van de mappen.

```
test custom activity Microsoft test custom activity Microsoft
```

Elke invoermap komt overeen met een segment in Azure Data Factory, zelfs als de map 2 of meer bestanden bevat. Wanneer elk segment wordt verwerkt door de pijplijn, wordt de aangepaste activiteit doorlopen alle blobs in de invoermap voor dit segment.

Er zijn vijf uitvoerbestanden met dezelfde inhoud. Het uitvoerbestand van het verwerken van het bestand in de map 2015-11-16-00 heeft bijvoorbeeld de volgende inhoud:

```
2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-00/file.txt.
```

Als u meerdere bestanden (bestand.txt, bestand2.txt samengevoegd, file3.txt) met dezelfde inhoud naar de invoermap verwijderen, ziet u de volgende inhoud in het uitvoerbestand. Elke map (2015-11-16-00, enz.) komt overeen met een segment in dit voorbeeld, ondanks dat de map meerdere invoerbestanden heeft.

```csharp
2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-00/file.txt.
2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-00/file2.txt.
2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-00/file3.txt.
```

Het uitvoerbestand heeft drie regels nu, één voor elk bestand voor invoer (blob) in de map die is gekoppeld aan het segment (2015-11-16-00).

Een taak is gemaakt voor elke activiteit die wordt uitgevoerd. In dit voorbeeld moet u er slechts één activiteit is in de pijplijn. Wanneer een segment wordt verwerkt door de pijplijn, wordt de aangepaste activiteit wordt uitgevoerd op Azure Batch voor het verwerken van het segment. Aangezien er zijn vijf segmenten (elk segment kan hebben meerdere blobs of -bestand), moet u er vijf taken die zijn gemaakt in Azure Batch zijn. Wanneer een taak wordt uitgevoerd op de Batch, is het daadwerkelijk de aangepaste activiteit die wordt uitgevoerd.

Het volgende scenario vindt u meer informatie.

#### <a name="step-1-create-the-data-factory"></a>Stap 1: De gegevensfactory maken
1. Na het aanmelden bij de [Azure-portal](https://portal.azure.com/), moet u de volgende stappen uitvoeren:

   1. Klik in het menu links op **NIEUW**.
   2. Klik op **gegevens en analyse** in de **nieuw** blade.
   3. Klik op de blade **Gegevensanalyse** op **Gegevensfactory**.
2. In de **nieuwe gegevensfactory** blade Voer **CustomActivityFactory** voor de naam. De naam van de Azure-gegevensfactory moet wereldwijd uniek zijn. Als u de foutmelding: **Data factory-naam 'CustomActivityFactory' is niet beschikbaar**, wijzig de naam van de gegevensfactory (bijvoorbeeld **yournameCustomActivityFactory**) en probeert u het opnieuw.
3. Klik op **RESOURCEGROEPNAAM**, en selecteer een bestaande resourcegroep of maak een resourcegroep.
4. Controleer of dat u gebruikmaakt van het juiste abonnement en de regio waar u de gegevensfactory wilt maken.
5. Klik op de blade **Nieuwe gegevensfactory** op **Maken**.
6. U ziet de gegevensfactory wordt gemaakt in de **Dashboard** van de Azure portal.
7. Nadat de gegevensfactory is gemaakt, ziet u de gegevensfactorypagina. Hierop wordt de inhoud van de gegevensfactory weergegeven.

   ![](./media/data-factory-data-processing-using-batch/image6.png)

#### <a name="step-2-create-linked-services"></a>Stap 2: Gekoppelde services maken
Met gekoppelde services worden gegevensarchieven of compute-services gekoppeld aan een Azure Data Factory. In deze stap koppelt u uw **Azure Storage** account en **Azure Batch** account toe aan uw gegevensfactory.

#### <a name="create-azure-storage-linked-service"></a>Een gekoppelde Azure Storage-service maken
1. Klik op de **auteur en implementeren van** tegel op de **DATA FACTORY** blade voor **CustomActivityFactory**. U ziet de Data Factory-Editor.
2. Klik op **nieuwe gegevensopslag** op de opdracht en kies **Azure-opslag.** U ziet het JSON-script voor het maken van een gekoppelde Azure Storage-service in de editor.

   ![](./media/data-factory-data-processing-using-batch/image7.png)

3. Vervang de **accountnaam** door de naam van uw Azure-opslagaccount en vervang de **accountsleutel** door de toegangssleutel van het Azure-opslagaccount. Zie [Toegangssleutels voor opslag weergeven, kopiëren en opnieuw genereren](../../storage/common/storage-create-storage-account.md#manage-your-storage-account) voor meer informatie over het verkrijgen van uw toegangssleutel voor opslag.

4. Klik op de opdrachtbalk op **Implementeren** om de gekoppelde service te implementeren.

   ![](./media/data-factory-data-processing-using-batch/image8.png)

#### <a name="create-azure-batch-linked-service"></a>Azure Batch gekoppelde service maken
In deze stap maakt u een gekoppelde service maken voor uw **Azure Batch** account dat wordt gebruikt voor het uitvoeren van de Data Factory aangepaste activiteit.

1. Klik op **nieuwe berekening** op de opdracht en kies **Azure Batch.** U ziet het JSON-script voor het maken van een gekoppelde Azure-Batch-service in de editor.
2. In het JSON-script:

   1. Vervang **accountnaam** met de naam van uw Azure Batch-account.
   2. Vervang **toegangssleutel** door de toegangssleutel van het Azure Batch-account.
   3. Voer de ID van de groep die de **poolName** eigenschap**.** Voor deze eigenschap kunt u de naam van de toepassingen opgeven of id-groep
   4. Voer de batch-URI voor de **batchUri** JSON-eigenschap.

      > [!IMPORTANT]
      > De **URL** van de **blade van Azure Batch-account** is in de volgende indeling: \<accountname\>.\< regio\>. batch.azure.com. Voor de **batchUri** eigenschap in de JSON, moet u **verwijderen "accountname."** van de URL. Voorbeeld: `"batchUri": "https://eastus.batch.azure.com"`.
      >
      >

      ![](./media/data-factory-data-processing-using-batch/image9.png)

      Voor de **poolName** eigenschap, kunt u ook de ID van de groep in plaats van de naam van de groep opgeven.

      > [!NOTE]
      > De Data Factory-service biedt een optie op aanvraag geen ondersteuning voor Azure Batch als voor HDInsight. U kunt uw eigen Azure Batch-pool alleen gebruiken in een Azure data factory.
      >
      >
   5. Geef **StorageLinkedService** voor de **linkedServiceName** eigenschap. U kunt deze gekoppelde service gemaakt in de vorige stap. Deze opslag wordt gebruikt als een faseringsgebied voor bestanden en de logboeken.
3. Klik op de opdrachtbalk op **Implementeren** om de gekoppelde service te implementeren.

#### <a name="step-3-create-datasets"></a>Stap 3: Gegevenssets maken
In deze stap maakt u gegevenssets vertegenwoordigen de invoer-en uitvoergegevens.

#### <a name="create-input-dataset"></a>Invoergegevensset maken
1. In de **Editor** voor de Gegevensfactory klikt u op **nieuwe gegevensset** op de werkbalk en klikt u op **Azure Blob storage** uit de vervolgkeuzelijst.
2. Vervang de JSON in het rechterdeelvenster met de volgende JSON-fragment:

    ```json
    {
       "name": "InputDataset",
       "properties": {
           "type": "AzureBlob",
           "linkedServiceName": "AzureStorageLinkedService",
           "typeProperties": {
               "folderPath": "mycontainer/inputfolder/{Year}-{Month}-{Day}-{Hour}",
               "format": {
                   "type": "TextFormat"
               },
               "partitionedBy": [
                   {
                       "name": "Year",
                       "value": {
                           "type": "DateTime",
                           "date": "SliceStart",
                           "format": "yyyy"
                       }
                   },
                   {
                       "name": "Month",
                       "value": {
                           "type": "DateTime",
                           "date": "SliceStart",
                           "format": "MM"
                       }
                   },
                   {
                       "name": "Day",
                       "value": {
                           "type": "DateTime",
                           "date": "SliceStart",
                           "format": "dd"
                       }
                   },
                   {
                       "name": "Hour",
                       "value": {
                           "type": "DateTime",
                           "date": "SliceStart",
                           "format": "HH"
                       }
                   }
               ]
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

    U maakt een pijplijn verderop in dit scenario met begintijd: 2015-11-16T00:00:00Z-en eindtijd: 2015-11-16T05:00:00Z. Deze is gepland voor het produceren van gegevens **per uur**, zodat er 5 segmenten van de invoer/uitvoer (tussen **00**: 00:00 -\> **05**: 00:00).

    De **frequentie** en **interval** voor invoergegevensset is ingesteld op **uur** en **1**, wat betekent dat de invoersegment per uur beschikbaar is.

    Hier volgen de begintijden voor elk segment, die wordt vertegenwoordigd door **SliceStart** systeemvariabele in de bovenstaande JSON-codefragment.

    | **Segment** | **Begintijd**          |
    |-----------|-------------------------|
    | 1         | 2015-11-16T**00**: 00:00 |
    | 2         | 2015-11-16T**01**: 00:00 |
    | 3         | 2015-11-16T**02**: 00:00 |
    | 4         | 2015-11-16T**03**: 00:00 |
    | 5         | 2015-11-16T**04**: 00:00 |

    De **folderPath** wordt berekend met behulp van het jaar, maand, dag en uur deel van de begintijd van het segment (**SliceStart**). Hier is daarom hoe een invoermap is toegewezen aan een segment.

    | **Segment** | **Begintijd**          | **Invoermap**  |
    |-----------|-------------------------|-------------------|
    | 1         | 2015-11-16T**00**: 00:00 | 2015-11-16-**00** |
    | 2         | 2015-11-16T**01**: 00:00 | 2015-11-16-**01** |
    | 3         | 2015-11-16T**02**: 00:00 | 2015-11-16-**02** |
    | 4         | 2015-11-16T**03**: 00:00 | 2015-11-16-**03** |
    | 5         | 2015-11-16T**04**: 00:00 | 2015-11-16-**04** |

1. Klik op **implementeren** op de werkbalk om te maken en implementeren van de **InputDataset** tabel.

#### <a name="create-output-dataset"></a>Uitvoergegevensset maken
In deze stap maakt u een andere dataset van het type AzureBlob vertegenwoordigt de uitvoergegevens.

1. In de **Editor** voor de Gegevensfactory klikt u op **nieuwe gegevensset** op de werkbalk en klikt u op **Azure Blob storage** uit de vervolgkeuzelijst.
2. Vervang de JSON in het rechterdeelvenster met de volgende JSON-fragment:

    ```json
    {
       "name": "OutputDataset",
       "properties": {
           "type": "AzureBlob",
           "linkedServiceName": "AzureStorageLinkedService",
           "typeProperties": {
               "fileName": "{slice}.txt",
               "folderPath": "mycontainer/outputfolder",
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

    Een uitvoer-blob/bestand wordt gegenereerd voor elk segment invoer. Hier ziet u hoe een bestand voor uitvoer met de naam van elk segment. De uitvoerbestanden worden gegenereerd in een uitvoermap: `mycontainer\\outputfolder`.

    | **Segment** | **Begintijd**          | **Bestand voor uitvoer**       |
    |-----------|-------------------------|-----------------------|
    | 1         | 2015-11-16T**00**: 00:00 | 2015-11-16 -**00. txt** |
    | 2         | 2015-11-16T**01**: 00:00 | 2015-11-16 -**01. txt** |
    | 3         | 2015-11-16T**02**: 00:00 | 2015-11-16 -**02. txt** |
    | 4         | 2015-11-16T**03**: 00:00 | 2015-11-16 -**03. txt** |
    | 5         | 2015-11-16T**04**: 00:00 | 2015-11-16 -**04. txt** |

    Houd er rekening mee dat alle bestanden in een invoermap (bijvoorbeeld: 2015-11-16-00) deel uitmaken van een segment met de begintijd: 2015-11-16-00. Als dit segment is verwerkt, wordt de aangepaste activiteit gescand door elk bestand en resulteert in een regel in het uitvoerbestand met het aantal exemplaren van zoekterm ('Microsoft'). Als er drie bestanden in de map 2015-11-16-00, er zijn drie regels in het uitvoerbestand: 2015-11-16-00.txt.

1. Klik op **implementeren** op de werkbalk om te maken en implementeren van de **OutputDataset**.

#### <a name="step-4-create-and-run-the-pipeline-with-custom-activity"></a>Stap 4: Maken en uitvoeren van de pijplijn met een aangepaste activiteit
In deze stap maakt maken u een pijplijn met één activiteit, de aangepaste activiteit die u eerder hebt gemaakt.

> [!IMPORTANT]
> Als u dit nog niet hebt geüpload de **bestand.txt** invoer mappen in de blob-container, doen voordat u de pijplijn. De **isPaused** eigenschap is ingesteld op false in de pijplijn-JSON, zodat de pijplijn wordt onmiddellijk uitgevoerd als de **start** datum ligt in het verleden.
>
>

1. In de Data Factory-Editor, klikt u op **nieuwe pijplijn** op de opdrachtbalk. Als u de opdracht niet ziet, klikt u op **... (Ellips)**  om het te bekijken.
2. Vervang de JSON in het rechterdeelvenster met de volgende JSON-script:

    ```json
    {
       "name": "PipelineCustom",
       "properties": {
           "description": "Use custom activity",
           "activities": [
               {
                   "type": "DotNetActivity",
                   "typeProperties": {
                       "assemblyName": "MyDotNetActivity.dll",
                       "entryPoint": "MyDotNetActivityNS.MyDotNetActivity",
                       "packageLinkedService": "AzureStorageLinkedService",
                       "packageFile": "customactivitycontainer/MyDotNetActivity.zip"
                   },
                   "inputs": [
                       {
                           "name": "InputDataset"
                       }
                   ],
                   "outputs": [
                       {
                           "name": "OutputDataset"
                       }
                   ],
                   "policy": {
                       "timeout": "00:30:00",
                       "concurrency": 5,
                       "retry": 3
                   },
                   "scheduler": {
                       "frequency": "Hour",
                       "interval": 1
                   },
                   "name": "MyDotNetActivity",
                   "linkedServiceName": "AzureBatchLinkedService"
               }
           ],
           "start": "2015-11-16T00:00:00Z",
           "end": "2015-11-16T05:00:00Z",
           "isPaused": false
      }
    }
    ```
   Houd rekening met de volgende punten:

   * Er is slechts één activiteit in de pijplijn en die is van het type: **DotNetActivity**.
   * **AssemblyName** is ingesteld op de naam van het DLL-bestand: **MyDotNetActivity.dll**.
   * **EntryPoint** is ingesteld op **MyDotNetActivityNS.MyDotNetActivity**. Het is in feite \<naamruimte\>.\< ClassName\> in uw code.
   * **PackageLinkedService** is ingesteld op **StorageLinkedService** die verwijst naar de blobopslag dat het zip-bestand voor aangepaste activiteit bevat. Als u van andere Azure Storage-accounts voor i/o-bestanden en het zip-bestand voor aangepaste activiteit gebruikmaakt, hebt u een andere gekoppelde Azure Storage-service te maken. In dit artikel wordt ervan uitgegaan dat u hetzelfde Azure-opslagaccount.
   * **PackageFile** is ingesteld op **customactivitycontainer/MyDotNetActivity.zip**. Het is in de indeling: \<containerforthezip\>/\<nameofthezip.zip\>.
   * De aangepaste activiteit duurt **InputDataset** als invoer en **OutputDataset** als uitvoer.
   * De **linkedServiceName** eigenschap van de aangepaste activiteit verwijst naar de **AzureBatchLinkedService**, waarin u Azure Data Factory die de aangepaste activiteit moet worden uitgevoerd op Azure Batch bepaald.
   * De **gelijktijdigheid** instelling is belangrijk. Als u de standaardwaarde 1, is zelfs als er 2 of meer knooppunten in de Azure Batch-pool berekenen, gebruikt de segmenten worden verwerkt achter elkaar. U onderneemt daarom niet profiteren van de mogelijkheid parallelle verwerking van Azure Batch. Als u instelt **gelijktijdigheid** op een hogere waarde spreek 2, betekent dit dat twee segmenten (komt overeen met twee taken in Azure Batch) kunnen worden verwerkt op hetzelfde moment, in welk geval zowel de virtuele machines in de Azure Batch pool worden benut. U kunt de eigenschap gelijktijdigheid daarom op de juiste wijze instellen.
   * Slechts één taak (segment) wordt standaard uitgevoerd op een virtuele machine op elk gewenst moment. De reden is dat standaard de **maximum aantal taken per VM** is ingesteld op 1 voor een Azure Batch-pool. Als onderdeel van de vereisten, kunt u een groep gemaakt met deze eigenschap ingesteld op 2, dus twee segmenten van de Data Factory op een virtuele machine op hetzelfde moment uitvoeren kunnen.

    -   **isPaused** eigenschap is standaard ingesteld op false. De pijplijn wordt onmiddellijk uitgevoerd in dit voorbeeld omdat de segmenten te in het verleden starten. U kunt deze eigenschap instellen op waar de pijplijn onderbreken en opnieuw ingesteld op false om opnieuw te starten.

    -   De **start** tijd en **end** tijden zijn vijf uur uit elkaar en segmenten per uur uitgevoerd, worden geproduceerd zodat vijf segmenten worden geproduceerd door de pijplijn.

1. Klik op de opdrachtbalk op **Implementeren** om de pijplijn te implementeren.

#### <a name="step-5-test-the-pipeline"></a>Stap 5: De pijplijn testen
In deze stap kunt u de pijplijn testen door het verwijderen van bestanden in de invoer-mappen. U begint met het testen van de pijplijn met één bestand per één invoer map.

1. Klik in de Data Factory-blade in de Azure portal op **Diagram**.

   ![](./media/data-factory-data-processing-using-batch/image10.png)
2. Dubbelklik in de diagramweergave op invoergegevensset: **InputDataset**.

   ![](./media/data-factory-data-processing-using-batch/image11.png)
3. U ziet de **InputDataset** blade met alle vijf segmenten gereed. U ziet de **segment BEGINTIJD** en **segment EINDTIJD** voor elk segment.

   ![](./media/data-factory-data-processing-using-batch/image12.png)
4. In de **diagramweergave**, klik op nu **OutputDataset**.
5. U ziet dat de vijf uitvoer segmenten in de status gereed zijn als ze zijn al gemaakt.

   ![](./media/data-factory-data-processing-using-batch/image13.png)
6. Azure portal gebruiken om weer te geven de **taken** die zijn gekoppeld aan de **segmenten** en zien welke VM die is uitgevoerd op elk segment. Zie [Data Factory en Batch-integratie](#data-factory-and-batch-integration) sectie voor meer informatie.
7. U ziet de uitvoerbestanden in de `outputfolder` van `mycontainer` in uw Azure-blobopslag.

   ![](./media/data-factory-data-processing-using-batch/image15.png)

   Hier ziet u vijf uitvoerbestanden, één voor elke invoersegment. Van het uitvoerbestand moeten hebben inhoud vergelijkbaar met de volgende uitvoer:

    ```
    2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-00/file.txt.
    ```
   Het volgende diagram illustreert hoe de Data Factory-segmenten worden toegewezen aan taken in Azure Batch. In dit voorbeeld heeft een segment slechts één uitvoeren.

   ![](./media/data-factory-data-processing-using-batch/image16.png)
8. Nu gaan we proberen met meerdere bestanden in een map. Bestanden maken: **bestand2.txt samengevoegd**, **file3.txt**, **file4.txt**, en **file5.txt** met dezelfde inhoud zoals in bestand.txt in de map: **2015-11-06-01**.
9. In de uitvoermap **verwijderen** het uitvoerbestand: **2015-11-16-01.txt**.
10. Nu in de **OutputDataset** blade met de rechtermuisknop op het segment met **segment BEGINTIJD** ingesteld op **16-11-2015 01:00:00 AM**, en klik op **uitvoeren** voor opnieuw uitvoeren/opnieuw-proceskleuren het segment. Het segment is nu vijf bestanden in plaats van een bestand.

    ![](./media/data-factory-data-processing-using-batch/image17.png)
11. Nadat het segment wordt uitgevoerd en de status is **gereed**, Controleer of de inhoud van het uitvoerbestand voor dit segment (**2015-11-16-01.txt**) in de `outputfolder` van `mycontainer` in de blob-opslag. Er moet een regel voor elk bestand van het segment.

    ```
    2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-01/file.txt.
    2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-01/file2.txt.
    2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-01/file3.txt.
    2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-01/file4.txt.
    2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-01/file5.txt.
    ```

> [!NOTE]
> Als u de uitvoer bestand 2015-11-16-01.txt niet verwijdert hebt voordat u met vijf invoerbestanden, ziet u één regel van de vorige segment uitvoeren en vijf de regels van het huidige segment uitvoeren. Standaard wordt de inhoud toegevoegd om het uitvoerbestand als deze al bestaat.
>
>

#### <a name="data-factory-and-batch-integration"></a>Data Factory en Batch-integratie
De Data Factory-service maakt een taak in Azure Batch met de naam: `adf-poolname:job-xxx`.

![Azure Data Factory - Batch-taken](media/data-factory-data-processing-using-batch/data-factory-batch-jobs.png)

Een taak in de taak is gemaakt voor elke activiteit uitvoering van een segment. Als er 10 segmenten klaar om te worden verwerkt, wordt 10 taken worden gemaakt in de taak. U kunt meer dan één segment parallel worden uitgevoerd als er meerdere rekenknooppunten in de groep hebben. Als het maximum aantal taken per rekenknooppunt is ingesteld op 1 >, kunnen er meer dan één segment uitgevoerd in de berekening die dezelfde.

In dit voorbeeld zijn er vijf segmenten, dus vijf taken in Azure Batch. Met de **gelijktijdigheid** ingesteld op **5** in de JSON-code in Azure Data Factory-pijplijn en **maximum aantal taken per VM** ingesteld op **2** in Azure Batch-pool met **2** virtuele machines, de taken uitgevoerd snelle (Raadpleeg de begin- en eindtijden voor taken).

De portal gebruiken voor het weergeven van de Batch-job en de bijbehorende taken die zijn gekoppeld aan de **segmenten** en zien welke VM die is uitgevoerd op elk segment.

![Azure Data Factory - taken voor Batch-job](media/data-factory-data-processing-using-batch/data-factory-batch-job-tasks.png)

### <a name="debug-the-pipeline"></a>Fouten opsporen in de pijplijn
De foutopsporing bestaat uit enkele basistechnieken:

1. Als de invoersegment niet is ingesteld op **gereed**, Controleer of de structuur van de invoer juist is en bestand.txt in de invoer-mappen bestaat.

   ![](./media/data-factory-data-processing-using-batch/image3.png)
2. In de **Execute** methode van uw aangepaste activiteit, gebruikt de **IActivityLogger** object aan te melden informatie die helpt bij het oplossen van problemen. De geregistreerde berichten weergegeven in de gebruiker\_0-logboekbestand.

   In de **OutputDataset** blade, klikt u op het segment om te zien de **GEGEVENSSEGMENT** blade voor die segment. U ziet **activiteiten bij uitvoering** voor dit segment. U ziet één activiteit is uitgevoerd voor het segment. Als u op **uitvoeren** in de opdrachtbalk begint u een andere activiteit die wordt uitgevoerd voor hetzelfde segment.

   Wanneer u klikt op de activiteit die wordt uitgevoerd, ziet u de **DETAILS uitvoering van activiteit** blade met een lijst met logboekbestanden. U ziet geregistreerde berichten in de **gebruiker\_0 logboek** bestand. Wanneer een fout optreedt, ziet u drie activiteit wordt uitgevoerd, omdat het maximale aantal pogingen is ingesteld op 3 in de pipeline/JSON van de activiteit. Als u op de activiteit uitvoeren klikt, ziet u de logboekbestanden die u bekijken kunt voor het oplossen van de fout.

   ![](./media/data-factory-data-processing-using-batch/image18.png)

   Klik in de lijst van logboekbestanden op de **gebruiker 0.log**. In het rechterdeelvenster worden de resultaten van het gebruik van de **IActivityLogger.Write** methode.

   ![](./media/data-factory-data-processing-using-batch/image19.png)

   Controleer de systeem-0.log voor elk systeem foutberichten en uitzonderingen.

    ```
    Trace\_T\_D\_12/6/2015 1:43:35 AM\_T\_D\_\_T\_D\_Verbose\_T\_D\_0\_T\_D\_Loading assembly file MyDotNetActivity...
    
    Trace\_T\_D\_12/6/2015 1:43:35 AM\_T\_D\_\_T\_D\_Verbose\_T\_D\_0\_T\_D\_Creating an instance of MyDotNetActivityNS.MyDotNetActivity from assembly file MyDotNetActivity...
    
    Trace\_T\_D\_12/6/2015 1:43:35 AM\_T\_D\_\_T\_D\_Verbose\_T\_D\_0\_T\_D\_Executing Module
    
    Trace\_T\_D\_12/6/2015 1:43:38 AM\_T\_D\_\_T\_D\_Information\_T\_D\_0\_T\_D\_Activity e3817da0-d843-4c5c-85c6-40ba7424dce2 finished successfully
    ```
3. Bevatten de **PDB** bestand in het zip-bestand, zodat de foutdetails beschikt over informatie zoals **aanroepstack** wanneer er een fout optreedt.
4. Alle bestanden in het zip-bestand voor de aangepaste activiteit moet op de **bovenste niveau** zonder submappen.

   ![](./media/data-factory-data-processing-using-batch/image20.png)
5. Zorg ervoor dat de **assemblyName** (MyDotNetActivity.dll) **entryPoint** (MyDotNetActivityNS.MyDotNetActivity) **packageFile** (customactivitycontainer/MyDotNetActivity.zip) en **packageLinkedService** (moet verwijzen naar de Azure blob storage met het zipbestand) zijn ingesteld op de juiste waarden.
6. Als u een fout hebt hersteld en het segment opnieuw wilt verwerken, klikt u met de rechtermuisknop op het segment op de blade **OutputDataset** en klikt u op **Uitvoeren**.

   ![](./media/data-factory-data-processing-using-batch/image21.png)

   > [!NOTE]
   > U ziet een **container** in uw Azure Blob-opslag met de naam: `adfjobs`. Deze container wordt niet automatisch verwijderd, maar u het veilig kunt verwijderen nadat u klaar bent met testen van de oplossing. Op dezelfde manier de Data Factory-oplossing maakt u een Azure-Batch **taak** met de naam: `adf-\<pool ID/name\>:job-0000000001`. Nadat u de oplossing als u dit wilt testen, kunt u deze taak verwijderen.
   >
   >
7. De aangepaste activiteit maakt geen gebruik van de **app.config** bestand van het pakket. Dus als uw code wordt een verbindingsreeksen uit het configuratiebestand gelezen, werkt het niet tijdens runtime. Het is het beste wanneer het gebruik van Azure Batch is voor het opslaan van alle geheimen in een **Azure KeyVault**, u een certificaat gebaseerde service-principal voor het beveiligen van de keyvault en distribueren van het certificaat naar de Azure Batch-pool. De aangepaste .NET-activiteit heeft dan in runtime toegang tot geheimen in de KeyVault. Deze oplossing is een algemene en kan worden geschaald met elk type geheim, niet alleen de verbindingsreeks.

    Er is een eenvoudiger tijdelijke oplossing (maar niet aanbevolen): kunt u een **Azure SQL gekoppelde service** met verbindingsinstellingen tekenreeks voor een gegevensset die gebruikmaakt van de gekoppelde service maken en koppelen van de gegevensset als een dummy invoergegevensset aan de aangepaste .NET-activiteit. U kunt de verbindingsreeks van de gekoppelde service in de code van de aangepaste activiteit bekijken en deze moet werken tijdens runtime.  

#### <a name="extend-the-sample"></a>Het voorbeeld uitbreiden
U kunt dit voorbeeld voor meer informatie over Azure Data Factory en Azure Batch-functies kunt uitbreiden. Voer bijvoorbeeld de volgende stappen uit voor het verwerken van segmenten in een ander tijdsbereik:

1. Voeg de volgende submappen in de `inputfolder`: 2015-11-16-05 2015-11-16-06 201-11-16-07, 2011-11-16-08, 2015-11-16-09 en plaats invoerbestanden in deze mappen. Wijzigen van de eindtijd voor de pijplijn van `2015-11-16T05:00:00Z` naar `2015-11-16T10:00:00Z`. In de **diagramweergave**, dubbelklikt u op de **InputDataset**, en te bevestigen dat de invoersegmenten één keer gereed zijn. Dubbelklik op **OuptutDataset** om te zien van de status van de uitvoer-segmenten. Als ze zich in de status Ready heeft, controleert u de map voor uitvoer voor de uitvoerbestanden.
2. Vergroten of verkleinen de **gelijktijdigheid** instelling om te begrijpen hoe het is van invloed op de prestaties van uw oplossing, met name de verwerking die op Azure Batch plaatsvindt. (Zie stap 4: maken en uitvoeren van de pijplijn voor meer informatie over de **gelijktijdigheid** instelling.)
3. Een pool maken met de hogere en kleine **maximum aantal taken per VM**. Werk de gekoppelde Azure-Batch-service in de Data Factory-oplossing voor het gebruik van de nieuwe toepassingen die u hebt gemaakt. (Zie stap 4: maken en uitvoeren van de pijplijn voor meer informatie over de **maximum aantal taken per VM** instelling.)
4. Maken van een Azure Batch-pool met **automatisch schalen** functie. Automatisch vergroten/verkleinen rekenknooppunten in een Azure Batch-pool is de dynamische aanpassing van de verwerkingskracht die worden gebruikt door de toepassing. 

    De voorbeeldformule bereikt het volgende gedrag: wanneer de groep in eerste instantie is gemaakt, wordt 1 VM gestart. $PendingTasks metriek definieert het aantal taken in uitvoering + actief (in de wachtrij) staat.  De formule wordt het gemiddelde aantal in behandeling zijnde taken gevonden in de afgelopen 180 seconden en dienovereenkomstig TargetDedicated ingesteld. Hiermee zorgt u ervoor dat TargetDedicated nooit zich verder uitstrekt dan 25 virtuele machines. Dus als nieuwe taken worden verzonden, pool automatisch groeit en als de taken zijn voltooid, virtuele machines worden gratis één voor één en verkleint u het automatisch schalen die virtuele machines. startingNumberOfVMs en maxNumberofVMs kan worden aangepast aan uw behoeften.
 
    De formule voor automatisch schalen:

    ``` 
    startingNumberOfVMs = 1;
    maxNumberofVMs = 25;
    pendingTaskSamplePercent = $PendingTasks.GetSamplePercent(180 * TimeInterval_Second);
    pendingTaskSamples = pendingTaskSamplePercent < 70 ? startingNumberOfVMs : avg($PendingTasks.GetSample(180 * TimeInterval_Second));
    $TargetDedicated=min(maxNumberofVMs,pendingTaskSamples);
    ```

   Zie [automatisch schalen rekenknooppunten in een Azure Batch-pool](../../batch/batch-automatic-scaling.md) voor meer informatie.

   Als de groep met behulp van de standaard [autoScaleEvaluationInterval](https://msdn.microsoft.com/library/azure/dn820173.aspx), de Batch-service kan enige tijd duren 15-30 minuten voor het voorbereiden van de virtuele machine voordat u de aangepaste activiteit.  Als de groep een andere autoScaleEvaluationInterval gebruikt is, kan de Batch-service autoScaleEvaluationInterval + 10 minuten duren.
5. In de Voorbeeldoplossing de **Execute** methode roept de **Calculate** methode waarmee een segment invoergegevens om te produceren van een gegevenssegment uitvoer wordt verwerkt. U kunt uw eigen methode voor het verwerken van invoergegevens en vervang de methodeaanroep berekenen in de methode Execute met een aanroep naar uw methode kunt schrijven.

### <a name="next-steps-consume-the-data"></a>Volgende stappen: de gegevens gebruiken
Nadat u gegevens verwerken, kunt u het verbruiken met online hulpmiddelen zoals **Microsoft Power BI**. Hier vindt u koppelingen naar informatie waarmee u een overzicht van Power BI en het gebruik ervan in Azure:

* [Een gegevensset in Power BI verkennen](https://powerbi.microsoft.com/documentation/powerbi-service-get-data/)
* [Aan de slag met Power BI Desktop](https://powerbi.microsoft.com/documentation/powerbi-desktop-getting-started/)
* [Vernieuwen van gegevens in Power BI](https://powerbi.microsoft.com/documentation/powerbi-refresh-data/)
* [Azure en Power BI - basisoverzicht](https://powerbi.microsoft.com/documentation/powerbi-azure-and-power-bi/)

## <a name="references"></a>Verwijzingen
* [Azure Data Factory](https://azure.microsoft.com/documentation/services/data-factory/)

  * [Inleiding tot Azure Data Factory-service](data-factory-introduction.md)
  * [Aan de slag met Azure Data Factory](data-factory-build-your-first-pipeline.md)
  * [Aangepaste activiteiten gebruiken in een Azure Data Factory-pijplijn](data-factory-use-custom-activities.md)
* [Azure Batch](https://azure.microsoft.com/documentation/services/batch/)

  * [Basisbeginselen van Azure Batch](../../batch/batch-technical-overview.md)
  * [Overzicht van Azure Batch-functies](../../batch/batch-api-basics.md)
  * [Azure Batch-account in de Azure portal maken en beheren](../../batch/batch-account-create-portal.md)
  * [Aan de slag met Azure Batch-bibliotheek .NET](../../batch/batch-dotnet-get-started.md)

[batch-explorer]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchExplorer
[batch-explorer-walkthrough]: http://blogs.technet.com/b/windowshpc/archive/2015/01/20/azure-batch-explorer-sample-walkthrough.aspx
