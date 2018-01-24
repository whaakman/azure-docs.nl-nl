---
title: Grote gegevenssets verwerken met behulp van Data Factory en Batch | Microsoft Docs
description: Beschrijft hoe het verwerken van grote hoeveelheden gegevens in een Azure Data Factory-pijplijn met behulp van de parallelle verwerking van de mogelijkheden van Azure Batch.
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
ms.date: 01/10/2018
ms.author: spelluru
robots: noindex
ms.openlocfilehash: af2c12cac5846ae1c4bc693bacaf72ab327fb87f
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2018
---
# <a name="process-large-scale-datasets-by-using-data-factory-and-batch"></a>Grote gegevenssets proces met behulp van Data Factory en Batch
> [!NOTE]
> In dit artikel is van toepassing op versie 1 van Azure Data Factory, die in het algemeen beschikbaar is. Als u versie 2 van de Data Factory-service, die zich in de preview, Zie [aangepaste activiteiten in de Data Factory versie 2](../transform-data-using-dotnet-custom-activity.md).

In dit artikel beschrijft een architectuur van een Voorbeeldoplossing die wordt verplaatst en grootschalige gegevenssets in een automatische en geplande wijze verwerkt. Het bevat ook een end-to-end-stappen voor de oplossing implementeert met behulp van Data Factory en Azure Batch.

In dit artikel is langer dan een typische artikel omdat deze een overzicht van een Voorbeeldoplossing voor het volledige bevat. Als u geen ervaring met de Batch- en Data Factory, kunt u lezen over deze services en hoe ze samen werken. Als u over de services weet en zijn ontwerpen/veranderd een oplossing, kunt u zich richten op de [architectuur sectie](#architecture-of-sample-solution) van het artikel. Als u een prototype of een oplossing ontwikkelt, wilt u mogelijk uitproberen Stapsgewijze instructies in de [scenario](#implementation-of-sample-solution). Wij willen graag uw opmerkingen over deze inhoud en hoe u deze gebruiken.

Eerst gaan we kijken hoe Data Factory en Batch services kunnen u helpen proces grote gegevenssets in de cloud.     

## <a name="why-azure-batch"></a>Waarom Azure Batch?
 U kunt Batch efficiënt grootschalige parallelle en high-performance computing (HPC)-toepassingen uitvoeren in de cloud. Dit is een platformservice die gepland rekenintensief werk uit te voeren op een beheerde verzameling van virtuele machines (VM's). Deze kan rekenresources om te voldoen aan de behoeften van uw taken automatisch schalen.

Met de Batch-service definieert u Azure-rekenresources om uw toepassingen parallel en op de juiste schaal uit te voeren. U kunt op aanvraag uitvoeren of geplande taken. U hoeft niet te handmatig maken, configureren en beheren van een HPC-cluster, individuele virtuele machines, virtuele netwerken, of een complexe job en infrastructuur plannen van taken.

 Als u niet bekend bent met Batch, kunnen u inzicht in de architectuur/implementatie van de oplossing wordt beschreven in dit artikel met de volgende artikelen:   

* [Basisprincipes van Batch](../../batch/batch-technical-overview.md)
* [Overzicht van de functies van Batch](../../batch/batch-api-basics.md)

Optioneel, voor meer informatie over Batch, Zie [leertraject voor Batch](https://azure.microsoft.com/documentation/learning-paths/batch/).

## <a name="why-azure-data-factory"></a>Waarom Azure Data Factory?
Een Data Factory is een cloudgebaseerde gegevensintegratieservice waarmee de verplaatsing en transformatie van gegevens wordt beheerd en geautomatiseerd. Data Factory kunt u beheerde gegevenspijplijnen die gegevens verplaatsen van on-premises en in de cloud van de opgeslagen gegevens naar een gecentraliseerde gegevensopslag maken. Een voorbeeld is de Azure-blobopslag. U kunt Data Factory transformatie/proces gegevens met behulp van services zoals Azure HDInsight en Azure Machine Learning. U kunt ook gegevenspijplijnen om uit te voeren op een geplande manier (bijvoorbeeld elk uur, dagelijks en wekelijks) plannen. U kunt bewaken en beheren van de pijplijnen in één oogopslag problemen identificeren en actie ondernemen.

  Als u niet bekend bent met Data Factory, kunnen u inzicht in de architectuur/implementatie van de oplossing wordt beschreven in dit artikel met de volgende artikelen:  

* [Inleiding tot Data Factory](data-factory-introduction.md)
* [Uw eerste pijplijn voor gegevens](data-factory-build-your-first-pipeline.md)   

Optioneel, voor meer informatie over Data Factory, Zie [leertraject voor Data Factory](https://azure.microsoft.com/documentation/learning-paths/data-factory/).

## <a name="data-factory-and-batch-together"></a>Data Factory en in Batch samen
Data Factory bevat ingebouwde activiteiten. Bijvoorbeeld met de kopieerbewerking wordt gebruikt om te kopiëren of verplaatsen gegevens uit een gegevensopslag bron naar een doelgegevensopslagplaats. Het Hive-activiteit wordt gebruikt voor het verwerken van gegevens met behulp van Hadoop-clusters (HDInsight) in Azure. Zie voor een lijst van activiteiten voor gegevenstransformatie ondersteunde [activiteiten voor gegevenstransformatie](data-factory-data-transformation-activities.md).

U kunt ook aangepaste .NET-activiteiten om te verplaatsen of verwerken van gegevens met uw eigen logica maken. U kunt deze activiteiten uitvoeren op een HDInsight-cluster of op een Batch-pool van virtuele machines. Wanneer u Batch gebruikt, kunt u de toepassingen automatisch schalen configureren (toevoegen of verwijderen van virtuele machines op basis van de werkbelasting) op basis van een formule die u opgeeft.     

## <a name="architecture-of-a-sample-solution"></a>Architectuur van een Voorbeeldoplossing
  De architectuur die wordt beschreven in dit artikel is bedoeld voor een eenvoudige oplossing. Het is ook relevant zijn voor complexe scenario's, zoals risico modellering van financiële diensten, verwerking van afbeeldingen en rendering en Toepassingsgeoriënteerde analyse.

Het diagram illustreert hoe Data Factory ingedeeld verwerking en de verplaatsing van gegevens. Ook ziet u hoe de gegevens op een parallelle manier Batch verwerkt. Downloaden en afdrukken van het diagram ter referentie (11 x 17 inch of A3-grootte). Voor toegang tot het diagram zodat u het kunt afdrukken, Zie [orchestration HPC en gegevens met behulp van Batch- en Data Factory](http://go.microsoft.com/fwlink/?LinkId=717686).

[![Diagram van grootschalige gegevensverwerking](./media/data-factory-data-processing-using-batch/image1.png)](http://go.microsoft.com/fwlink/?LinkId=717686)

De volgende lijst bevat de basisstappen van het proces. De oplossing omvat code en uitleg om de end-to-end-oplossing te bouwen.

* **Batch configureren met een pool van rekenknooppunten (virtuele machines).** U kunt het aantal knooppunten en de grootte van elk knooppunt opgeven.

* **Maak een exemplaar van de Data Factory** die is geconfigureerd met de entiteiten die blob-opslag, de Batch compute-service, invoer-en uitvoergegevens en een werkstroom/pijplijn met activiteiten die verplaatsen en gegevens transformeren vertegenwoordigen.

* **Maak een aangepaste .NET-activiteit in de Data Factory-pijplijn.** De activiteit is uw gebruikerscode die wordt uitgevoerd op de Batch-pool.

* **Opslaan van grote hoeveelheden gegevens als blobs in Azure Storage.** Gegevens worden in logische segmenten verdeeld (meestal door time).

* **Data Factory kopieert de gegevens die wordt verwerkt parallel** naar de secundaire locatie.

* **De aangepaste activiteit Data Factory uitgevoerd met behulp van de groep toegewezen door de Batch.** Data Factory kunt activiteiten tegelijkertijd worden uitgevoerd. Elke activiteit verwerkt een segment met gegevens. De resultaten worden opgeslagen in de opslag.

* **Data Factory het eindresultaat verplaatst naar een derde locatie** voor distributie via een app of voor verdere verwerking door andere hulpprogramma's.

## <a name="implementation-of-the-sample-solution"></a>Implementatie van de Voorbeeldoplossing
De Voorbeeldoplossing is opzettelijk eenvoudig. Deze is ontworpen voor hoe u Data Factory en Batch naar proces gegevenssets samen gebruiken. De oplossing telt het aantal exemplaren van de zoekterm 'Microsoft' in de invoer-bestanden die zijn ingedeeld in een tijdreeks. Het levert vervolgens de uitvoerbestanden telt.

**Tijd:** als u bekend met de basisprincipes van Azure Data Factory en Batch bent en de volgende vereisten hebt voltooid, deze oplossing wordt een tot twee uur om te voltooien.

### <a name="prerequisites"></a>Vereisten
#### <a name="azure-subscription"></a>Azure-abonnement
Als u geen Azure-abonnement hebt, kunt u snel een gratis proefaccount maken. Zie voor meer informatie [gratis proefversie](https://azure.microsoft.com/pricing/free-trial/).

#### <a name="azure-storage-account"></a>Azure Storage-account
U kunt een opslagaccount gebruiken voor het opslaan van de gegevens in deze zelfstudie. Als u geen storage-account hebt, raadpleegt u [een opslagaccount maken](../../storage/common/storage-create-storage-account.md#create-a-storage-account). De oplossing maakt gebruik van blob-opslag.

#### <a name="azure-batch-account"></a>Azure Batch-account
Een Batch-account maken met behulp van de [Azure-portal](http://portal.azure.com/). Zie voor meer informatie [maken en beheren van een Batch-account](../../batch/batch-account-create-portal.md). Noteer de sleutel Batch-account en -account. Ook kunt u de [New-AzureRmBatchAccount](https://msdn.microsoft.com/library/mt603749.aspx) cmdlet voor het maken van een Batch-account. Zie voor instructies over het gebruik van deze cmdlet [aan de slag met de Batch PowerShell-cmdlets](../../batch/batch-powershell-cmdlets-get-started.md).

De oplossing maakt gebruik van Batch (indirect via een data factory-pijplijn) om gegevens te verwerken in een parallelle manier op een pool van rekenknooppunten (een beheerde verzameling van virtuele machines).

#### <a name="azure-batch-pool-of-virtual-machines"></a>Azure Batch-pool van virtuele machines
Maak een Batch-pool met ten minste twee rekenknooppunten.

1. In de [Azure-portal](https://portal.azure.com), selecteer **Bladeren** in het menu aan de linkerkant en selecteer **Batch-Accounts**.

2. Selecteer uw Batch-account openen de **Batch-Account** blade.

3. Selecteer de **Pools** tegel.

4. Op de **Pools** blade, selecteer de **toevoegen** op de werkbalk een groep toevoegen.

   a. Geef een ID voor de groep (**Pool-ID**). Noteer de ID van de pool. U moet bij het maken van de data factory-oplossing.

   b. Geef **Windows Server 2012 R2** voor de **besturingssysteemgroep** instelling.

   c. Selecteer een **knooppunt prijscategorie**.

   d. Voer **2** als de waarde voor de **doel toegewezen** instelling.

   e. Voer **2** als de waarde voor de **maximum aantal taken per knooppunt** instelling.

   f. Selecteer **OK** om de pool te maken.

#### <a name="azure-storage-explorer"></a>Azure Opslagverkenner
U gebruikt [Azure Storage Explorer 6](https://azurestorageexplorer.codeplex.com/) of [CloudXplorer](http://clumsyleaf.com/products/cloudxplorer) (van ClumsyLeaf Software) om te controleren en wijzigen van de gegevens in uw Storage-projecten. U kunt ook controleren en wijzigen van de gegevens in de logboeken van uw cloud-gebaseerde toepassingen.

1. Maken van een container met de naam **mycontainer** met persoonlijke toegang (geen anonieme toegang).

2. Als u CloudXplorer gebruikt, maakt u mappen en submappen met de volgende structuur:

   ![Structuur van mappen en submappen](./media/data-factory-data-processing-using-batch/image3.png)

   `Inputfolder`en `outputfolder` zijn op het hoogste niveau van de mappen in `mycontainer`. De `inputfolder` map bevat submappen met de datum / tijd stempels (jjjj-MM-DD-UU).

   Als u Opslagverkenner in de volgende stap gebruikt, uploadt u bestanden met de volgende namen: `inputfolder/2015-11-16-00/file.txt`, `inputfolder/2015-11-16-01/file.txt`, enzovoort. Deze stap maakt automatisch de mappen.

3. Maak een tekstbestand **bestand.txt** op uw computer met inhoud met het sleutelwoord **Microsoft**. Een voorbeeld is 'aangepaste activiteit Microsoft test aangepaste activiteit Microsoft test'.

4. Upload het bestand naar de volgende invoer mappen in de blob-opslag:

   ![Invoer mappen](./media/data-factory-data-processing-using-batch/image4.png)

   Als u Opslagverkenner gebruikt, uploadt u de **bestand.txt** van het bestand in **mycontainer**. Selecteer **kopie** op de werkbalk om een kopie maken van de blob. In de **Blob kopiëren** wijziging van het dialoogvenster de **blob doelnaam** naar `inputfolder/2015-11-16-00/file.txt`. Herhaal deze stap maken `inputfolder/2015-11-16-01/file.txt`, `inputfolder/2015-11-16-02/file.txt`, `inputfolder/2015-11-16-03/file.txt`, `inputfolder/2015-11-16-04/file.txt`, enzovoort. Deze actie wordt automatisch gemaakt voor de mappen.

5. Maken van een andere container met de naam `customactivitycontainer`. Upload het zip-bestand voor aangepaste activiteit aan deze container.

#### <a name="visual-studio"></a>Visual Studio
Installeer Visual Studio 2012 of later om de aangepaste Batch-activiteit moet worden gebruikt in de data factory-oplossing te maken.

### <a name="high-level-steps-to-create-the-solution"></a>De stappen op hoog niveau om de oplossing te maken
1. Maak een aangepaste activiteit die de gegevensverwerking logica bevat.

2. Maak een gegevensfactory die gebruikmaakt van de aangepaste activiteit.

### <a name="create-the-custom-activity"></a>De aangepaste activiteit maken
De data factory aangepaste activiteit vormt de kern van de Voorbeeldoplossing van dit. De oplossing maakt gebruik van Batch uitvoeren van de aangepaste activiteit. Zie voor meer informatie over het ontwikkelen van aangepaste activiteiten en deze gebruiken in de data factory-pijplijnen [aangepaste activiteiten gebruiken in een data factory-pijplijn](data-factory-use-custom-activities.md).

Voor het maken van een aangepaste .NET-activiteit die u kunt gebruiken in een data factory-pijplijn, kunt u een .NET-klasse bibliotheek-project maakt met een klasse die de IDotNetActivity-interface implementeert. Deze interface heeft slechts één methode: uitvoeren. Dit is de handtekening van de methode:

```csharp
public IDictionary<string, string> Execute(
            IEnumerable<LinkedService> linkedServices,
            IEnumerable<Dataset> datasets,
            Activity activity,
            IActivityLogger logger)
```

De methode heeft enkele belangrijke onderdelen die u nodig hebt om te begrijpen:

* De methode bevat vier parameters:

  * **linkedServices**. Deze parameter is een inventariseerbare lijst met gekoppelde services die gegevensbronnen van invoer/uitvoer (bijvoorbeeld blobopslag) aan de gegevensfactory koppelen. In dit voorbeeld is er slechts één van de gekoppelde service van Azure Storage gebruikt voor invoer en uitvoer van het type.
  * **datasets**. Deze parameter is een inventariseerbare lijst met gegevenssets. Deze parameter kunt u de locaties en schema's die zijn gedefinieerd door de invoer- en uitvoergegevenssets ophalen.
  * **activiteit**. Deze parameter geeft de huidige compute-entiteit. In dit geval is het een batchservice.
  * **logger**. Het logboek kunt u foutopsporing opmerkingen die surface schrijven als het logboek 'Gebruiker' voor de pijplijn.
* De methode retourneert een woordenlijst die kan worden gebruikt om een keten van aangepaste activiteiten in de toekomst. Deze functie nog niet is geïmplementeerd, dus u hoeft alleen een woordenboek leeg retourneren van de methode.

#### <a name="procedure-create-the-custom-activity"></a>Procedure: De aangepaste activiteit maken
1. Maak een .NET-klasse bibliotheek-project in Visual Studio.

   a. Start Visual Studio 2012/2013/2015.

   b. Selecteer **Bestand** > **Nieuw** > **Project**.

   c. Vouw **sjablonen**, en selecteer **Visual C\#**. In dit scenario gebruikt u C\#, maar u kunt een andere taal .NET gebruiken voor het ontwikkelen van de aangepaste activiteit.

   d. Selecteer **Class Library** uit de lijst met projecttypen aan de rechterkant.

   e. Voer **MyDotNetActivity** voor de **naam**.

   f. Selecteer **C:\\ADF** voor de **locatie**. Maak de map **ADF** als deze niet bestaat.

   g. Selecteer **OK** om het project te maken.

2. Selecteer **extra** > **NuGet-Pakketbeheer** > **Package Manager Console**.

3. Voer de volgende opdracht Microsoft.Azure.Management.DataFactories importeren in de Package Manager-Console:

    ```powershell
    Install-Package Microsoft.Azure.Management.DataFactories
    ```
4. Importeer de **Azure Storage** NuGet-pakket in het project. Omdat u de API van Blob Storage in dit voorbeeld gebruiken moet u dit pakket:

    ```powershell
    Install-Package Azure.Storage
    ```
5. Voeg het volgende toe met behulp van instructies aan het bronbestand in het project:

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
6. Wijzig de naam van de naamruimte naar **MyDotNetActivityNS**.

    ```csharp
    namespace MyDotNetActivityNS
    ```
7. Wijzig de naam van de klasse **MyDotNetActivity**, en die afkomstig zijn uit de **IDotNetActivity** interface zoals wordt weergegeven:

    ```csharp
    public class MyDotNetActivity : IDotNetActivity
    ```
8. Implementeer (toevoegen) de **Execute** methode van de **IDotNetActivity** interface met de **MyDotNetActivity** klasse. Kopieer de volgende voorbeeldcode naar de methode. Zie voor een uitleg van de logica in deze methode gebruikt, de [methode Execute](#execute-method) sectie.

    ```csharp
    /// <summary>
    /// The Execute method is the only method of IDotNetActivity interface you must implement.
    /// In this sample, the method invokes the Calculate method to perform the core logic.  
    /// </summary>
    public IDictionary<string, string> Execute(
       IEnumerable<LinkedService> linkedServices,
       IEnumerable<Dataset> datasets,
       Activity activity,
       IActivityLogger logger)
    {
    
       // Declare types for the input and output data stores.
       AzureStorageLinkedService inputLinkedService;
    
       Dataset inputDataset = datasets.Single(dataset => dataset.Name == activity.Inputs.Single().Name);
    
       foreach (LinkedService ls in linkedServices)
           logger.Write("linkedService.Name {0}", ls.Name);
    
       // Use the First method instead of Single because we are using the same
       // Azure Storage linked service for input and output.
       inputLinkedService = linkedServices.First(
           linkedService =>
           linkedService.Name ==
           inputDataset.Properties.LinkedServiceName).Properties.TypeProperties
           as AzureStorageLinkedService;
    
       string connectionString = inputLinkedService.ConnectionString; // To create an input storage client.
       string folderPath = GetFolderPath(inputDataset);
       string output = string.Empty; // for use later.
    
       // Create the storage client for input. Pass the connection string.
       CloudStorageAccount inputStorageAccount = CloudStorageAccount.Parse(connectionString);
       CloudBlobClient inputClient = inputStorageAccount.CreateCloudBlobClient();
    
       // Initialize the continuation token before using it in the do-while loop.
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
    
           // The Calculate method returns the number of occurrences of
           // the search term "Microsoft" in each blob associated
           // with the data slice.
           //
           // The definition of the method is shown in the next step.
           output = Calculate(blobList, logger, folderPath, ref continuationToken, "Microsoft");
    
       } while (continuationToken != null);
    
       // Get the output dataset by using the name of the dataset matched to a name in the Activity output collection.
       Dataset outputDataset = datasets.Single(dataset => dataset.Name == activity.Outputs.Single().Name);
    
       folderPath = GetFolderPath(outputDataset);
    
       logger.Write("Writing blob to the folder: {0}", folderPath);
    
       // Create a storage object for the output blob.
       CloudStorageAccount outputStorageAccount = CloudStorageAccount.Parse(connectionString);
       // Write the name of the file.
       Uri outputBlobUri = new Uri(outputStorageAccount.BlobEndpoint, folderPath + "/" + GetFileName(outputDataset));
    
       logger.Write("output blob URI: {0}", outputBlobUri.ToString());
       // Create a blob and upload the output text.
       CloudBlockBlob outputBlob = new CloudBlockBlob(outputBlobUri, outputStorageAccount.Credentials);
       logger.Write("Writing {0} to the output blob", output);
       outputBlob.UploadText(output);
    
       // The dictionary can be used to chain custom activities together in the future.
       // This feature is not implemented yet, so just return an empty dictionary.
       return new Dictionary<string, string>();
    }
    ```
9. De volgende methoden toevoegen aan de klasse. Deze methoden worden aangeroepen door de **Execute** methode. Belangrijkste, de **Calculate** methode isoleert de code die elke blob doorlopen.

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
    /// Iterates through each blob (file) in the folder, counts the number of instances of the search term in the file,
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
    De methode GetFolderPath retourneert het pad naar de map die de gegevensset naar verwijst en de getfilename,-methode retourneert de naam van de blob of het bestand die de gegevensset naar verwijst.

    ```csharp

    "name": "InputDataset",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
            "fileName": "file.txt",
            "folderPath": "mycontainer/inputfolder/{Year}-{Month}-{Day}-{Hour}",
    ```

    De methode Calculate berekent het aantal exemplaren van het sleutelwoord 'Microsoft' in de invoerbestanden (BLOB's in de map). De zoekterm is 'Microsoft' vastgelegd in de code.

10. Het project gecompileerd. Selecteer **bouwen** in het menu en selecteer vervolgens **Build Solution**.

11. Start Windows Verkenner en Ga naar de **bin\\debug** of **bin\\release** map. De map keuze is afhankelijk van het type build.

12. Maak een zipbestand **MyDotNetActivity.zip** die bevat alle binaire bestanden in de  **\\bin\\Debug** map. Mogelijk wilt de MyDotNetActivity bevatten. **pdb** bestand zodat u aanvullende informatie zoals het regelnummer in de broncode die het probleem wordt veroorzaakt ophalen wanneer er een fout optreedt.

   ![De lijst van de map bin\Debug](./media/data-factory-data-processing-using-batch/image5.png)

13. Uploaden **MyDotNetActivity.zip** als blob naar de blob-container `customactivitycontainer` in de blobopslag dat het de gekoppelde StorageLinkedService-service in ADFTutorialDataFactory gebruikt. Maken van de blob-container `customactivitycontainer` als deze niet al bestaat.

#### <a name="execute-method"></a>De methode Execute
Deze sectie bevat meer informatie over de code in de Execute-methode.

1. De leden voor doorloopt de invoerverzameling vindt u in de [Microsoft.WindowsAzure.Storage.Blob](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.aspx) naamruimte. Als u wilt de blob-collectie doorlopen, wordt vereist voor het gebruik van de **BlobContinuationToken** klasse. In wezen, moet u een do-tijdens lus aan het token als het mechanisme voor de lus wordt afgesloten. Zie voor meer informatie [gebruik Blob storage uit het .NET](../../storage/blobs/storage-dotnet-how-to-use-blobs.md). Hier ziet u een eenvoudige lus:

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
   Zie voor meer informatie de documentatie voor de [ListBlobsSegmented](https://msdn.microsoft.com/library/jj717596.aspx) methode.

2. De code voor het werken met de reeks blobs logisch gaat binnen de do-tijdens lus. In de **Execute** methode, de komen-terwijl lus de lijst met BLOB's aan een methode met de naam geeft **Calculate**. De methode retourneert een string-variabele met de naam **uitvoer** die het resultaat van het via de blobs in het segment dat herhaald.

   Deze retourneert het aantal exemplaren van de zoekterm 'Microsoft' in de blob wordt doorgegeven aan de **Calculate** methode.

    ```csharp
    output += string.Format("{0} occurrences of the search term \"{1}\" were found in the file {2}.\r\n", wordCount, searchTerm, inputBlob.Name);
    ```
3. Na de **Calculate** methode is voltooid, er moet worden geschreven naar een nieuwe blob. Voor elke set van BLOB's verwerkt, kan een nieuwe blob met de resultaten worden geschreven. Voor het schrijven naar een nieuwe blob, moet u eerst de uitvoergegevensset vinden.

    ```csharp
    // Get the output dataset by using the name of the dataset matched to a name in the Activity output collection.
    Dataset outputDataset = datasets.Single(dataset => dataset.Name == activity.Outputs.Single().Name);
    ```
4. De code ook de Help-methode aanroept **GetFolderPath** voor het ophalen van het mappad (de containernaam voor opslag).

    ```csharp
    folderPath = GetFolderPath(outputDataset);
    ```
   De methode GetFolderPath cast de DataSet-object aan een AzureBlobDataSet met een eigenschap genaamd FolderPath.

    ```csharp
    AzureBlobDataset blobDataset = dataArtifact.Properties.TypeProperties as AzureBlobDataset;
    
    return blobDataset.FolderPath;
    ```
5. Het aanroepen van de code de **GetFileName** methode voor het ophalen van de bestandsnaam (blob-naam). De code is vergelijkbaar met de vorige code die is gebruikt voor het ophalen van pad naar de map.

    ```csharp
    AzureBlobDataset blobDataset = dataArtifact.Properties.TypeProperties as AzureBlobDataset;
    
    return blobDataset.FileName;
    ```
6. De naam van het bestand is geschreven door het maken van een URI-object. De URI-constructor gebruikt de **BlobEndpoint** eigenschap om de containernaam te retourneren. De naam van pad en bestandsnaam in de map worden toegevoegd aan het samenstellen van de blob-URI van de uitvoer.  

    ```csharp
    // Write the name of the file.
    Uri outputBlobUri = new Uri(outputStorageAccount.BlobEndpoint, folderPath + "/" + GetFileName(outputDataset));
    ```
7. Nadat de naam van het bestand is geschreven, kunt u de uitvoertekenreeks van de **Calculate** methode naar een nieuwe blob:

    ```csharp
    // Create a blob and upload the output text.
    CloudBlockBlob outputBlob = new CloudBlockBlob(outputBlobUri, outputStorageAccount.Credentials);
    logger.Write("Writing {0} to the output blob", output);
    outputBlob.UploadText(output);
    ```

### <a name="create-the-data-factory"></a>De gegevensfactory maken
In de [maken van de aangepaste activiteit](#create-the-custom-activity) sectie maakt u een aangepaste activiteit gemaakt en het zip-bestand met binaire bestanden en het PDB-bestand is geüpload naar een blob-container. In deze sectie maakt u een gegevensfactory met een pipeline die gebruikmaakt van de aangepaste activiteit.

De invoergegevensset voor de aangepaste activiteit vertegenwoordigt de blobs (bestanden) in de invoermap (`mycontainer\\inputfolder`) in de blob-opslag. De uitvoergegevensset voor de activiteit vertegenwoordigt de uitvoer blobs in de map voor uitvoer (`mycontainer\\outputfolder`) in de blob-opslag.

Een of meer bestanden neerzetten in de invoer mappen:

```
mycontainer -\> inputfolder
    2015-11-16-00
    2015-11-16-01
    2015-11-16-02
    2015-11-16-03
    2015-11-16-04
```

Neerzetten bijvoorbeeld één bestand (bestand.txt) met de volgende inhoud in elk van de mappen:

```
test custom activity Microsoft test custom activity Microsoft
```

Elke invoermap komt overeen met een segment in de gegevensfactory, zelfs als de map twee of meer bestanden heeft. Wanneer elk segment wordt verwerkt door de pijplijn, wordt de aangepaste activiteit doorlopen alle blobs in de invoermap voor dit segment.

Er zijn vijf uitvoerbestanden met dezelfde inhoud. Het uitvoerbestand van het verwerken van het bestand in de map 2015-11-16-00 heeft bijvoorbeeld de volgende inhoud:

```
2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-00/file.txt.
```

Als u meerdere bestanden (bestand.txt, bestand2.txt samengevoegd, file3.txt) met dezelfde inhoud in de invoermap neerzetten, ziet u de volgende inhoud in het uitvoerbestand. Elke map (2015-11-16-00, enz.) komt overeen met een segment in dit voorbeeld, ondanks dat de map meerdere invoerbestanden heeft.

```csharp
2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-00/file.txt.
2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-00/file2.txt.
2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-00/file3.txt.
```

Het uitvoerbestand heeft drie regels nu, één voor elk bestand voor invoer (blob) in de map die is gekoppeld aan het segment (2015-11-16-00).

Een taak is gemaakt voor elke activiteit die wordt uitgevoerd. In dit voorbeeld moet u er slechts één activiteit is in de pijplijn. Wanneer een segment wordt verwerkt door de pijplijn, wordt de aangepaste activiteit wordt uitgevoerd op Batch voor het verwerken van het segment. Omdat er vijf segmenten (elk segment kan hebben meerdere blobs of -bestand), worden de vijf taken in Batch gemaakt. Wanneer een taak wordt uitgevoerd op de Batch, is de aangepaste activiteit die wordt uitgevoerd.

Het volgende scenario vindt u meer informatie.

#### <a name="step-1-create-the-data-factory"></a>Stap 1: De gegevensfactory maken
1. Nadat u zich aanmeldt bij de [Azure-portal](https://portal.azure.com/), voert de volgende stappen uit:

   a. Selecteer **nieuw** in het menu links.

   b. Selecteer **gegevens en analyse** op de **nieuw** blade.

   c. Selecteer **Data Factory** op de **gegevensanalyse** blade.

2. Op de **nieuwe gegevensfactory** blade Voer **CustomActivityFactory** voor de naam. De naam van de data factory moet een Globally Unique Identifier zijn. Als u de foutmelding 'naam gegevensfactory CustomActivityFactory is niet beschikbaar', moet u de naam van de gegevensfactory wijzigen. Bijvoorbeeld, gebruik yournameCustomActivityFactory en opnieuw maken van de gegevensfactory.

3. Selecteer **RESOURCEGROEPNAAM**, en selecteer een bestaande resourcegroep of maak een resourcegroep.

4. Controleer of het abonnement en de regio waar u de gegevensfactory gemaakt juist zijn.

5. Selecteer **maken** op de **nieuwe gegevensfactory** blade.

6. De gegevensfactory wordt gemaakt in het dashboard van de portal.

7. Nadat de gegevensfactory is gemaakt, ziet u de **gegevensfactory** pagina, waarin u de inhoud van de gegevensfactory.

   ![Data factory-pagina](./media/data-factory-data-processing-using-batch/image6.png)

#### <a name="step-2-create-linked-services"></a>Stap 2: Gekoppelde services maken
Gekoppelde services worden gegevensarchieven of compute-services aan een gegevensfactory. In deze stap koppelt u uw opslagaccount en de Batch-account aan uw gegevensfactory.

#### <a name="create-an-azure-storage-linked-service"></a>Een gekoppelde Azure Storage-service maken
1. Selecteer de **auteur en implementeren van** tegel op de **gegevensfactory** blade voor **CustomActivityFactory**. De Data Factory-Editor wordt weergegeven.

2. Selecteer **nieuwe gegevensopslag** op de opdrachtbalk en kies **Azure-opslag.** De JSON-script maakt u een opslag gekoppelde service in de editor wordt weergegeven.

   ![Nieuwe gegevensopslag](./media/data-factory-data-processing-using-batch/image7.png)

3. Vervang **accountnaam** met de naam van uw opslagaccount. Vervang **accountsleutel** door de toegangssleutel van het opslagaccount. Zie voor meer informatie over het ophalen van uw toegangssleutel voor opslag, [weergeven, kopiëren en opnieuw genereren opslag toegangssleutels](../../storage/common/storage-create-storage-account.md#manage-your-storage-account).

4. Selecteer **implementeren** op de opdrachtbalk om de gekoppelde service te implementeren.

   ![Implementeren](./media/data-factory-data-processing-using-batch/image8.png)

#### <a name="create-an-azure-batch-linked-service"></a>Maak een gekoppelde Azure-Batch-service
In deze stap maakt u een gekoppelde service voor uw Batch-account dat wordt gebruikt voor het uitvoeren van de data factory aangepaste activiteit.

1. Selecteer **nieuwe berekening** op de opdrachtbalk en kies **Azure Batch.** De JSON-script maakt u een Batch gekoppelde service in de editor wordt weergegeven.

2. In het JSON-script:

   a. Vervang **accountnaam** met de naam van uw Batch-account.

   b. Vervang **toegangssleutel** door de toegangssleutel van Batch-account.

   c. Voer de ID van de groep die de **poolName** eigenschap. Voor deze eigenschap kunt u de naam of id van de groep van toepassingen.

   d. Voer de batch-URI voor de **batchUri** JSON-eigenschap.

      > [!IMPORTANT]
      > De URL van de **Batch-Account** blade is in de volgende indeling: \<accountname\>.\< regio\>. batch.azure.com. Voor de **batchUri** eigenschap in het JSON-script, moet u verwijderen a88 'accountname." ** van de URL. Een voorbeeld is `"batchUri": "https://eastus.batch.azure.com"`.
      >
      >

      ![Blade voor batch-Account](./media/data-factory-data-processing-using-batch/image9.png)

      Voor de **poolName** eigenschap, kunt u ook de ID van de groep in plaats van de naam van de groep opgeven.

      > [!NOTE]
      > De Data Factory-service biedt geen een-op-verzoek-optie ondersteuning voor Batch als voor HDInsight. U kunt alleen uw eigen Batch-pool gebruiken in een gegevensfactory.
      >
      >
   
   e. Geef **StorageLinkedService** voor de **linkedServiceName** eigenschap. U kunt deze gekoppelde service gemaakt in de vorige stap. Deze opslag wordt gebruikt als een faseringsgebied voor bestanden en de logboeken.

3. Selecteer **implementeren** op de opdrachtbalk om de gekoppelde service te implementeren.

#### <a name="step-3-create-datasets"></a>Stap 3: Gegevenssets maken
In deze stap maakt u gegevenssets vertegenwoordigen de invoer-en uitvoergegevens.

#### <a name="create-the-input-dataset"></a>Invoergegevensset maken
1. In de Data Factory-Editor, selecteer de **nieuwe gegevensset** op de werkbalk. Selecteer **Azure Blob storage** uit de vervolgkeuzelijst.

2. Vervang de JSON-script in het rechterdeelvenster met de volgende JSON-fragment:

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

    U maakt een pijplijn verderop in dit overzicht met de begintijd 2015-11-16T00:00:00Z en het einde keer 2015-11-16T05:00:00Z. Deze is gepland voor het produceren van gegevens per uur uitgevoerd, dus er vijf segmenten van de invoer/uitvoer zijn (tussen **00**: 00:00 -\> **05**: 00:00).

    De **frequentie** en **interval** voor invoergegevensset zijn ingesteld op **uur** en **1**, wat betekent dat de invoersegment per uur beschikbaar is.

    De begintijd voor elk segment wordt vertegenwoordigd door de **SliceStart** systeemvariabele in het vorige JSON-codefragment. Hier volgen de begintijden voor elk segment.

    | **Slice** | **Begintijd**          |
    |-----------|-------------------------|
    | 1         | 2015-11-16T**00**:00:00 |
    | 2         | 2015-11-16T**01**:00:00 |
    | 3         | 2015-11-16T**02**:00:00 |
    | 4         | 2015-11-16T**03**:00:00 |
    | 5         | 2015-11-16T**04**:00:00 |

    De **folderPath** wordt berekend met behulp van het jaar, maand, dag en uur deel van de begintijd van het segment (**SliceStart**). Hier ziet u hoe een invoermap is toegewezen aan een segment.

    | **Slice** | **Begintijd**          | **Invoermap**  |
    |-----------|-------------------------|-------------------|
    | 1         | 2015-11-16T**00**:00:00 | 2015-11-16-**00** |
    | 2         | 2015-11-16T**01**:00:00 | 2015-11-16-**01** |
    | 3         | 2015-11-16T**02**:00:00 | 2015-11-16-**02** |
    | 4         | 2015-11-16T**03**:00:00 | 2015-11-16-**03** |
    | 5         | 2015-11-16T**04**:00:00 | 2015-11-16-**04** |

3. Selecteer **implementeren** op de werkbalk om te maken en implementeren van de **InputDataset** tabel.

#### <a name="create-the-output-dataset"></a>De uitvoergegevensset maken
In deze stap maakt u een andere dataset van het type AzureBlob vertegenwoordigt de uitvoergegevens.

1. In de Data Factory-Editor, selecteer de **nieuwe gegevensset** op de werkbalk. Selecteer **Azure Blob storage** uit de vervolgkeuzelijst.

2. Vervang de JSON-script in het rechterdeelvenster met de volgende JSON-fragment:

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

    Een uitvoer-blob/bestand wordt gegenereerd voor elk segment invoer. Hier ziet u hoe een bestand voor uitvoer met de naam van elk segment. De uitvoerbestanden worden gegenereerd in een map voor uitvoer, `mycontainer\\outputfolder`.

    | **Slice** | **Begintijd**          | **Bestand voor uitvoer**       |
    |-----------|-------------------------|-----------------------|
    | 1         | 2015-11-16T**00**:00:00 | 2015-11-16-**00.txt** |
    | 2         | 2015-11-16T**01**:00:00 | 2015-11-16-**01.txt** |
    | 3         | 2015-11-16T**02**:00:00 | 2015-11-16-**02.txt** |
    | 4         | 2015-11-16T**03**:00:00 | 2015-11-16-**03.txt** |
    | 5         | 2015-11-16T**04**:00:00 | 2015-11-16-**04.txt** |

    Houd er rekening mee dat alle bestanden in een invoermap (bijvoorbeeld 2015-11-16-00) deel van een segment met de begintijd 2015-11-16-00 uitmaken. Als dit segment is verwerkt, de aangepaste activiteit gescand door elk bestand en resulteert in een regel in het uitvoerbestand met het aantal exemplaren van de zoekterm 'Microsoft'. Als er drie bestanden in de map 2015-11-16-00, zijn er drie regels in de uitvoer bestand 2015-11-16-00.txt.

3. Selecteer **implementeren** op de werkbalk om te maken en implementeren van de **OutputDataset**.

#### <a name="step-4-create-and-run-the-pipeline-with-a-custom-activity"></a>Stap 4: Maken en uitvoeren van de pijplijn met een aangepaste activiteit
In deze stap maakt maken u een pijplijn met één activiteit, de aangepaste activiteit die u eerder hebt gemaakt.

> [!IMPORTANT]
> Als u dit nog niet hebt geüpload **bestand.txt** invoer mappen in de blob-container, doen voordat u de pijplijn maken. De **isPaused** eigenschap is ingesteld op false in de pijplijn-JSON, zodat de pijplijn wordt onmiddellijk uitgevoerd, omdat de **start** datum ligt in het verleden.
>
>

1. Selecteer in de Data Factory-Editor, **nieuwe pijplijn** op de opdrachtbalk. Als u de opdracht niet ziet, selecteert u het weglatingsteken weer te geven.

2. Vervang de JSON-script in het rechterdeelvenster met de volgende JSON-fragment:

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

   * Slechts één activiteit is in de pijplijn en is van het type **DotNetActivity**.
   * **AssemblyName** is ingesteld op de naam van het DLL-bestand **MyDotNetActivity.dll**.
   * **EntryPoint** is ingesteld op **MyDotNetActivityNS.MyDotNetActivity**. Het is in feite \<naamruimte\>.\< ClassName\> in uw code.
   * **PackageLinkedService** is ingesteld op **StorageLinkedService**, die verwijzen naar de blobopslag dat het zip-bestand voor aangepaste activiteit bevat. Als u verschillende opslagaccounts voor i/o-bestanden en het zip-bestand voor aangepaste activiteit gebruikt, hebt u een andere opslag gekoppelde service maken. In dit artikel wordt ervan uitgegaan dat u hetzelfde opslagaccount.
   * **PackageFile** is ingesteld op **customactivitycontainer/MyDotNetActivity.zip**. Het is in de notatie \<containerforthezip\>/\<nameofthezip.zip\>.
   * De aangepaste activiteit duurt **InputDataset** als invoer en **OutputDataset** als uitvoer.
   * De **linkedServiceName** eigenschap van de aangepaste activiteit verwijst naar **AzureBatchLinkedService**, waarin u Data Factory die de aangepaste activiteit moet worden uitgevoerd op Batch bepaald.
   * De **gelijktijdigheid** instelling is belangrijk. Als u de standaardwaarde 1, is zelfs als u twee of meer knooppunten in de Batch-pool berekenen, gebruikt de segmenten worden verwerkt achter elkaar. Daarom zijn niet u profiteren van de parallelle verwerking van de mogelijkheid van Batch. Als u instelt **gelijktijdigheid** op een hogere waarde spreek 2, betekent dit dat twee segmenten (komt overeen met twee taken in Batch) op hetzelfde moment kunnen worden verwerkt. In dit geval worden zowel de virtuele machines in de Batch-pool worden benut. De eigenschap gelijktijdigheid juist ingesteld.
   * Slechts één taak (segment) wordt standaard uitgevoerd op een virtuele machine op elk gewenst moment. Standaard **maximum aantal taken per VM** is ingesteld op 1 voor een Batch-pool. Als onderdeel van de vereisten, kunt u een groep gemaakt met deze eigenschap ingesteld op 2. Daarom kunnen segmenten van twee data factory uitvoeren op een virtuele machine op hetzelfde moment.
    - De **isPaused** eigenschap is standaard ingesteld op false. De pijplijn wordt onmiddellijk uitgevoerd in dit voorbeeld omdat de segmenten te in het verleden starten. U kunt deze eigenschap instellen op **true** onderbreken van de pijplijn en weer in set **false** om opnieuw te starten.
    -   De **start** en **end** tijden zijn vijf uur uit elkaar liggen. Segmenten worden elk uur, geproduceerd zodat vijf segmenten worden geproduceerd door de pijplijn.

3. Selecteer **implementeren** op de opdrachtbalk om de pijplijn te implementeren.

#### <a name="step-5-test-the-pipeline"></a>Stap 5: De pijplijn testen
In deze stap kunt u de pijplijn testen door het verwijderen van bestanden in de invoer-mappen. Begin met het testen van de pijplijn met een bestand voor elke map invoer.

1. Op de **gegevensfactory** blade in de Azure portal, selecteer **Diagram**.

   ![Diagram](./media/data-factory-data-processing-using-batch/image10.png)

2. In de **Diagram** weergeven, dubbelklikt u op de invoergegevensset **InputDataset**.

   ![InputDataset](./media/data-factory-data-processing-using-batch/image11.png)

3. De **InputDataset** blade wordt weergegeven met alle vijf segmenten gereed. U ziet de **segment BEGINTIJD** en **segment EINDTIJD** voor elk segment.

   ![Invoer segment begin- en eindtijd](./media/data-factory-data-processing-using-batch/image12.png)

4. In de **Diagram** weergave, selecteer **OutputDataset**.

5. De vijf uitvoer segmenten worden weergegeven in de **gereed** status als ze zijn geproduceerd.

   ![Uitvoer segment begin- en eindtijd](./media/data-factory-data-processing-using-batch/image13.png)

6. De portal gebruiken om de taken die zijn gekoppeld aan de segmenten weer te geven en zien welke VM die is uitgevoerd op elk segment. Zie voor meer informatie de [Data Factory en Batch-integratie](#data-factory-and-batch-integration) sectie.

7. De uitvoerbestanden worden weergegeven onder `mycontainer` in `outputfolder` in de blob-opslag.

   ![Output-bestanden in de opslag](./media/data-factory-data-processing-using-batch/image15.png)

   Vijf uitvoerbestanden worden vermeld, één voor elk invoersegment. Elk van de uitvoerbestanden heeft de inhoud is vergelijkbaar met de volgende uitvoer:

    ```
    2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-00/file.txt.
    ```
   Het volgende diagram illustreert hoe de data factory-segmenten worden toegewezen aan taken in Batch. In dit voorbeeld heeft een segment slechts één uitvoeren.

   ![Diagram van segment toewijzing](./media/data-factory-data-processing-using-batch/image16.png)

8. Probeer het nu met meerdere bestanden in een map. Maken van de bestanden **bestand2.txt samengevoegd**, **file3.txt**, **file4.txt**, en **file5.txt** met dezelfde inhoud zoals bestand.txt in de map in**2015-11-06-01**.

9. Verwijder het bestand voor uitvoer in de uitvoermap **2015-11-16-01.txt**.

10. Op de **OutputDataset** blade met de rechtermuisknop op het segment met **segment BEGINTIJD** ingesteld op **16-11-2015 01:00:00 AM**. Selecteer **uitvoeren** voor verwerking opnieuw apparaat uitvoeren of het segment opnieuw uitvoeren. Het segment heeft nu vijf bestanden in plaats van een bestand.

    ![Voer](./media/data-factory-data-processing-using-batch/image17.png)

11. Nadat het segment wordt uitgevoerd en de status is **gereed**, Controleer of de inhoud van het uitvoerbestand voor dit segment (**2015-11-16-01.txt**). Het bestand voor uitvoer wordt weergegeven onder `mycontainer` in `outputfolder` in de blob-opslag. Er moet een regel voor elk bestand van het segment.

    ```
    2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-01/file.txt.
    2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-01/file2.txt.
    2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-01/file3.txt.
    2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-01/file4.txt.
    2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-01/file5.txt.
    ```

> [!NOTE]
> Als u de uitvoer bestand 2015-11-16-01.txt voordat u probeerde met vijf invoerbestanden niet verwijdert, ziet u één regel van de vorige segment uitvoeren en vijf de regels van het huidige segment uitvoeren. Standaard wordt de inhoud naar het uitvoerbestand toegevoegd, als deze al bestaat.
>
>

#### <a name="data-factory-and-batch-integration"></a>Data Factory en Batch-integratie
De Data Factory-service maakt een taak in een Batch met de naam `adf-poolname:job-xxx`.

![Batch-taken](media/data-factory-data-processing-using-batch/data-factory-batch-jobs.png)

Een taak in de taak is gemaakt voor elke activiteit uitvoering van een segment. Als 10 segmenten gereed om te worden verwerkt zijn, worden de 10 taken in de taak gemaakt. U kunt meer dan één segment parallel worden uitgevoerd als er meerdere rekenknooppunten in de groep hebben. Als het maximum aantal taken per rekenknooppunt is ingesteld op meer dan één, wordt meer dan één segment kunt uitvoeren in de berekening die dezelfde.

In dit voorbeeld zijn er vijf segmenten dus er vijf taken in Batch zijn. Met **gelijktijdigheid** ingesteld op **5** in de pijplijn-JSON in de gegevensfactory en **maximum aantal taken per VM** ingesteld op **2** in de Batch-pool met **2** virtuele machines, de taken worden snel uitgevoerd. (Raadpleeg de begin- en eindtijden voor taken.)

De portal gebruiken om de Batch-job en de bijbehorende taken die gekoppeld aan de segmenten zijn weer te geven en zien welke VM die is uitgevoerd op elk segment.

![Batch-taken](media/data-factory-data-processing-using-batch/data-factory-batch-job-tasks.png)

### <a name="debug-the-pipeline"></a>Fouten opsporen in de pijplijn
Foutopsporing bestaat uit een paar eenvoudige technieken.

1. Als de invoersegment niet is ingesteld op **gereed**, Controleer of de structuur van de invoer juist is en dat bestand.txt in de invoer-mappen bestaat.

   ![Structuur van invoer](./media/data-factory-data-processing-using-batch/image3.png)

2. In de **Execute** methode van uw aangepaste activiteit, gebruikt de **IActivityLogger** object aan te melden informatie die helpt bij het oplossen van problemen. De geregistreerde berichten weergegeven in de gebruiker\_0-logboekbestand.

   Op de **OutputDataset** blade, selecteer het segment om te zien de **gegevenssegment** blade voor die segment. Onder **activiteit wordt uitgevoerd**, Zie van een activiteit die wordt uitgevoerd voor het segment. Als u selecteert **uitvoeren** in de opdrachtbalk begint u een andere activiteit die wordt uitgevoerd voor hetzelfde segment.

   Wanneer u de activiteit die wordt uitgevoerd selecteert, ziet u de **details uitvoering van activiteit** blade met een lijst met logboekbestanden. U ziet geregistreerde berichten in de gebruiker\_0-logboekbestand. Wanneer een fout optreedt, ziet u drie activiteit wordt uitgevoerd, omdat het maximale aantal pogingen is ingesteld op 3 in de pipeline/JSON van de activiteit. Als u de activiteit uitvoeren selecteert, ziet u de logboekbestanden die u bekijken kunt voor het oplossen van de fout.

   ![OutputDataset en gegevens segment blades](./media/data-factory-data-processing-using-batch/image18.png)

   Selecteer in de lijst van logboekbestanden **gebruiker 0.log**. In het rechterdeelvenster de resultaten van het gebruik van de **IActivityLogger.Write** methode worden weergegeven.

   ![Activiteit blade toewijzingdetails uitvoeren](./media/data-factory-data-processing-using-batch/image19.png)

   Controleer de systeem-0.log voor elk systeem foutberichten en uitzonderingen.

    ```
    Trace\_T\_D\_12/6/2015 1:43:35 AM\_T\_D\_\_T\_D\_Verbose\_T\_D\_0\_T\_D\_Loading assembly file MyDotNetActivity...
    
    Trace\_T\_D\_12/6/2015 1:43:35 AM\_T\_D\_\_T\_D\_Verbose\_T\_D\_0\_T\_D\_Creating an instance of MyDotNetActivityNS.MyDotNetActivity from assembly file MyDotNetActivity...
    
    Trace\_T\_D\_12/6/2015 1:43:35 AM\_T\_D\_\_T\_D\_Verbose\_T\_D\_0\_T\_D\_Executing Module
    
    Trace\_T\_D\_12/6/2015 1:43:38 AM\_T\_D\_\_T\_D\_Information\_T\_D\_0\_T\_D\_Activity e3817da0-d843-4c5c-85c6-40ba7424dce2 finished successfully
    ```
3. Bevatten de **PDB** bestand in het zip-bestand, zodat de foutdetails informatie zoals de aanroepstack hebben wanneer er een fout optreedt.

4. Alle bestanden in het zip-bestand voor de aangepaste activiteit moet op het hoogste niveau zonder submappen.

   ![Lijst met aangepaste activiteit zip-bestand](./media/data-factory-data-processing-using-batch/image20.png)

5. Zorg ervoor dat **assemblyName** (MyDotNetActivity.dll) **entryPoint** (MyDotNetActivityNS.MyDotNetActivity) **packageFile** (customactivitycontainer / MyDotNetActivity.zip) en **packageLinkedService** (moet verwijzen naar de blob-opslag met het zipbestand) zijn ingesteld op de juiste waarden.

6. Als u een fout en wilt u het segment opnieuw verwerken hebt opgelost, met de rechtermuisknop op het segment de **OutputDataset** blade en selecteer **uitvoeren**.

   ![Blade OutputDataset optie uitvoeren](./media/data-factory-data-processing-using-batch/image21.png)

   > [!NOTE]
   > Een container bevindt zich in de blob-opslag met de naam `adfjobs`. Deze container wordt niet automatisch verwijderd, maar u het veilig kunt verwijderen nadat u klaar bent met het testen van de oplossing. Op deze manier maakt u de data factory-oplossing een Batch-job met de naam `adf-\<pool ID/name\>:job-0000000001`. Nadat u de oplossing als u dit wilt testen, kunt u deze taak verwijderen.
   >
   >
7. De aangepaste activiteit maakt geen gebruik van de **app.config** bestand van het pakket. Dus als uw code wordt een verbindingsreeksen uit het configuratiebestand gelezen, werkt niet tijdens runtime. Het is het beste wanneer u Batch is ingedrukt houden geen geheimen in Azure Sleutelkluis. Gebruik vervolgens een certificaat gebaseerde service-principal beveiligen van de sleutelkluis en distribueren van het certificaat naar de Batch-pool. De aangepaste activiteit .NET kan toegang krijgen tot geheimen van de sleutelkluis tijdens runtime. Deze algemene oplossing kunt met elk type geheim, niet alleen een verbindingsreeks schalen.

    Er is een oplossing eenvoudiger, maar het is niet aanbevolen. U kunt een service van SQL database gekoppeld tekenreeks instellingen met verbinding maken. Vervolgens kunt u een gegevensset die gebruikmaakt van de gekoppelde service maken en koppelen van de gegevensset als een dummy invoergegevensset aan de aangepaste .NET-activiteit. Vervolgens kunt u de verbindingsreeks van de gekoppelde service in de code van de aangepaste activiteit openen. Deze moet werken tijdens runtime.  

#### <a name="extend-the-sample"></a>Het voorbeeld uitbreiden
U kunt dit voorbeeld voor meer informatie over Data Factory en Batch-functies kunt uitbreiden. Bijvoorbeeld: voor het verwerken van segmenten in een ander tijdsbereik, de volgende stappen uitvoeren:

1. Voeg de volgende submappen in `inputfolder`: 2015-11-16-05, 2015-11-16-06 201-11-16-07-2011-11-16-08 en 2015-11-16-09. Invoerbestanden in deze mappen plaatsen. Wijzigen van de eindtijd voor de pijplijn van `2015-11-16T05:00:00Z` naar `2015-11-16T10:00:00Z`. In de **Diagram** weergeven, dubbelklikt u op **InputDataset** en Bevestig dat de invoersegmenten één keer gereed zijn. Dubbelklik op **OutputDataset** om te zien van de status van de uitvoer-segmenten. Als ze zich in de **gereed** staat, controleert u de map voor uitvoer voor de uitvoerbestanden.

2. Vergroten of verkleinen de **gelijktijdigheid** instelling om te begrijpen hoe het is van invloed op de prestaties van uw oplossing, met name de verwerking die op Batch plaatsvindt. Voor meer informatie over de **gelijktijdigheid** instelling, Zie ' stap 4: maken en uitvoeren van de pijplijn met een aangepaste activiteit. "

3. Een pool maken met de hogere en kleine **maximum aantal taken per VM**. Gekoppelde service in de data factory-oplossing voor het gebruik van de nieuwe toepassingen die u hebt gemaakt, het bijwerken van de Batch. Voor meer informatie over de **maximum aantal taken per VM** instelling, Zie ' stap 4: maken en uitvoeren van de pijplijn met een aangepaste activiteit. "

4. Maak een Batch-pool met de **automatisch schalen** functie. Automatisch vergroten/verkleinen rekenknooppunten in een Batch-pool is de dynamische aanpassing van de verwerkingskracht die worden gebruikt door de toepassing. 

    De formule hier bereikt het volgende gedrag. Wanneer de groep in eerste instantie is gemaakt, wordt deze begint met één virtuele machine. De metriek $PendingTasks definieert het aantal taken actief en actief is (in de wachtrij) aangegeven. De formule wordt het gemiddelde aantal in behandeling zijnde taken gevonden in de afgelopen 180 seconden en dienovereenkomstig TargetDedicated ingesteld. Hiermee zorgt u ervoor dat TargetDedicated nooit zich verder uitstrekt dan 25 virtuele machines. Als nieuwe taken worden verzonden, wordt de groep automatisch groeit. Als de taken zijn voltooid, virtuele machines worden gratis één voor één en verkleint u het automatisch schalen die virtuele machines. U kunt startingNumberOfVMs en maxNumberofVMs aanpassen aan uw behoeften.
 
    De formule voor automatisch schalen:

    ``` 
    startingNumberOfVMs = 1;
    maxNumberofVMs = 25;
    pendingTaskSamplePercent = $PendingTasks.GetSamplePercent(180 * TimeInterval_Second);
    pendingTaskSamples = pendingTaskSamplePercent < 70 ? startingNumberOfVMs : avg($PendingTasks.GetSample(180 * TimeInterval_Second));
    $TargetDedicated=min(maxNumberofVMs,pendingTaskSamples);
    ```

   Zie voor meer informatie [automatisch schalen rekenknooppunten in een Batch-pool](../../batch/batch-automatic-scaling.md).

   Als de groep wordt gebruikt voor [autoScaleEvaluationInterval](https://msdn.microsoft.com/library/azure/dn820173.aspx), 15 tot 30 minuten voor het voorbereiden van de virtuele machine voordat de aangepaste activiteit wordt uitgevoerd door de Batch-service kan duren. Als de groep gebruikmaakt van een andere autoScaleEvaluationInterval, kan de Batch-service autoScaleEvaluationInterval plus 10 minuten duren.

5. In de Voorbeeldoplossing de **Execute** methode roept de **Calculate** methode waarmee een segment invoergegevens om te produceren van een gegevenssegment uitvoer wordt verwerkt. Kunt u uw eigen methode voor invoergegevens verwerkt en vervang de **Calculate** methode-aanroep in de **Execute** methode met een aanroep naar uw methode.

### <a name="next-steps-consume-the-data"></a>Volgende stappen: de gegevens gebruiken
Nadat u gegevens verwerken, kunt u het verbruiken met online hulpprogramma's, zoals Power BI. Hier vindt u koppelingen naar informatie waarmee u een overzicht van Power BI en het gebruik ervan in Azure:

* [Een gegevensset in Power BI verkennen](https://powerbi.microsoft.com/documentation/powerbi-service-get-data/)
* [Aan de slag met Power BI Desktop](https://powerbi.microsoft.com/documentation/powerbi-desktop-getting-started/)
* [Vernieuwen van gegevens in Power BI](https://powerbi.microsoft.com/documentation/powerbi-refresh-data/)
* [Azure en Power BI: basisoverzicht](https://powerbi.microsoft.com/documentation/powerbi-azure-and-power-bi/)

## <a name="references"></a>Verwijzingen
* [Azure Data Factory](https://azure.microsoft.com/documentation/services/data-factory/)

  * [Inleiding tot de Data Factory-service](data-factory-introduction.md)
  * [Aan de slag met Data Factory](data-factory-build-your-first-pipeline.md)
  * [Aangepaste activiteiten gebruiken in een Data Factory-pijplijn](data-factory-use-custom-activities.md)
* [Azure Batch](https://azure.microsoft.com/documentation/services/batch/)

  * [Basisprincipes van Batch](../../batch/batch-technical-overview.md)
  * [Overzicht van Batch-functies](../../batch/batch-api-basics.md)
  * [Een batchaccount in de Azure portal maken en beheren](../../batch/batch-account-create-portal.md)
  * [Aan de slag met de Batch-clientbibliotheek voor .NET](../../batch/batch-dotnet-get-started.md)

[batch-explorer]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchExplorer
[batch-explorer-walkthrough]: http://blogs.technet.com/b/windowshpc/archive/2015/01/20/azure-batch-explorer-sample-walkthrough.aspx
