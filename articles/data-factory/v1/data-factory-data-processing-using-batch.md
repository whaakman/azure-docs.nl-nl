---
title: Grootschalige gegevenssets verwerken met behulp van Data Factory en Batch | Microsoft Docs
description: Beschrijft hoe u voor het verwerken van grote hoeveelheden gegevens in een Azure Data Factory-pijplijn met behulp van de parallelle verwerking van de mogelijkheden van Azure Batch.
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
ms.assetid: 688b964b-51d0-4faa-91a7-26c7e3150868
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 01/10/2018
ms.author: shlo
robots: noindex
ms.openlocfilehash: 77bb891dd8e7d6a3cb6363b06eaf361d09db594c
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/02/2018
ms.locfileid: "39422413"
---
# <a name="process-large-scale-datasets-by-using-data-factory-and-batch"></a>Grootschalige gegevenssets verwerken met behulp van Data Factory en Batch
> [!NOTE]
> Dit artikel is van toepassing op versie 1 van Azure Data Factory, die algemeen beschikbaar is. Als u de huidige versie van de Data Factory-service gebruikt, raadpleegt u [aangepaste activiteiten in Data Factory](../transform-data-using-dotnet-custom-activity.md).

In dit artikel beschrijft een architectuur van een Voorbeeldoplossing die wordt verplaatst en grootschalige gegevenssets in een automatische en geplande wijze verwerkt. Het biedt ook een end-to-end-scenario voor het implementeren van de oplossing met behulp van Data Factory en Azure Batch.

In dit artikel is langer dan een typische artikel omdat deze een overzicht van een volledige oplossing bevat. Als u geen ervaring met Batch en Data Factory, vindt u informatie over deze services en hoe ze samenwerken. Als u over de services weet en zijn ontwerpen/architectuur met een oplossing, kunt u zich richten op de [architectuur sectie](#architecture-of-sample-solution) van het artikel. Als u een prototype of een oplossing ontwikkelt, kunt u voor het uitproberen van de stapsgewijze instructies in de [scenario](#implementation-of-sample-solution). Wij willen graag uw opmerkingen over deze inhoud en hoe u deze gebruikt.

Eerst laten we kijken hoe Data Factory en Batch-services u kunt grote gegevenssets proces in de cloud.     

## <a name="why-azure-batch"></a>Waarom Azure Batch?
 U kunt Batch gebruiken voor het uitvoeren van grootschalige parallelle en high performance computing (HPC) toepassingen efficiënt in de cloud. Het is een platformservice die rekenintensief werk inplant voor uitvoering op een beheerde verzameling virtuele machines (VM's). Deze compute-resources om te voldoen aan de behoeften van uw taken kan automatisch worden geschaald.

Met de Batch-service definieert u Azure-rekenresources om uw toepassingen parallel en op de juiste schaal uit te voeren. U kunt op aanvraag uitvoeren of geplande taken. U hoeft niet te handmatig maken, configureren en beheren van een HPC-cluster, individuele virtuele machines, virtuele netwerken of een complexe taak en taakplanning infrastructuur.

 Als u niet bekend bent met Batch, de volgende artikelen helpen u te begrijpen van de architectuur/implementatie van de oplossing in dit artikel beschreven:   

* [Basisprincipes van Batch](../../batch/batch-technical-overview.md)
* [Overzicht van de functies van Batch](../../batch/batch-api-basics.md)

(Optioneel) voor meer informatie over Batch, Zie [leertraject voor Batch](https://azure.microsoft.com/documentation/learning-paths/batch/).

## <a name="why-azure-data-factory"></a>Waarom Azure Data Factory?
Een Data Factory is een cloudgebaseerde gegevensintegratieservice waarmee de verplaatsing en transformatie van gegevens wordt beheerd en geautomatiseerd. Data Factory kunt u beheerde gegevenspijplijnen maken die gegevens verplaatsen van on-premises en gegevensarchieven naar een centrale gegevensopslagplaats in de cloud. Een voorbeeld is de Azure Blob-opslag. U kunt Data Factory gebruiken om te verwerken/transformeren gegevens met behulp van services zoals Azure HDInsight en Azure Machine Learning. U kunt ook gegevenspijplijnen om uit te voeren op een geplande wijze (bijvoorbeeld elk uur, dagelijks en wekelijks) plannen. U kunt controleren en beheren van de pijplijnen in een oogwenk om problemen te identificeren en actie ondernemen.

  Als u niet bekend bent met Data Factory, de volgende artikelen helpen u te begrijpen van de architectuur/implementatie van de oplossing in dit artikel beschreven:  

* [Inleiding tot Data Factory](data-factory-introduction.md)
* [Uw eerste pijplijn bouwen](data-factory-build-your-first-pipeline.md)   

(Optioneel) voor meer informatie over Data Factory, Zie [leertraject voor Data Factory](https://azure.microsoft.com/documentation/learning-paths/data-factory/).

## <a name="data-factory-and-batch-together"></a>Data Factory en Batch samen
Data Factory bevat ingebouwde activiteiten. Bijvoorbeeld, de Copy-activiteit wordt gebruikt om te kopiëren en verplaatsen van gegevens van een brongegevensarchief naar een doelgegevensarchief. De Hive-activiteit wordt gebruikt om gegevens te verwerken met behulp van Hadoop-clusters (HDInsight) op Azure. Zie voor een lijst van ondersteunde transformatieactiviteiten, [activiteiten voor gegevenstransformatie](data-factory-data-transformation-activities.md).

U kunt ook aangepaste .NET-activiteiten wilt verplaatsen of gegevens te verwerken met uw eigen logica maken. U kunt deze activiteiten uitvoeren op een HDInsight-cluster of in een Batch-pool van virtuele machines. Wanneer u Batch gebruikt, kunt u de groep van automatisch schalen configureren (toevoegen of verwijderen van virtuele machines op basis van de werkbelasting) op basis van een formule die u opgeeft.     

## <a name="architecture-of-a-sample-solution"></a>Architectuur van een Voorbeeldoplossing
  De architectuur die worden beschreven in dit artikel is bedoeld voor een eenvoudige oplossing. Het is ook relevant zijn voor complexe scenario's, zoals de modellering van financiële services, verwerking van afbeeldingen en rendering en genetische analysis risico.

Het diagram illustreert hoe Data Factory regelt de verwerking en gegevensverplaatsing. U leert ook hoe Batch verwerkt de gegevens in een parallelle manier. Downloaden en afdrukken van het diagram voor eenvoudige verwijzing (11 x 17 inches of A3-grootte). Voor toegang tot het diagram zodat u het kunt afdrukken, Zie [HPC en gegevensindeling met behulp van Batch en Data Factory](http://go.microsoft.com/fwlink/?LinkId=717686).

[![Grootschalige gegevensverwerking diagram](./media/data-factory-data-processing-using-batch/image1.png)](http://go.microsoft.com/fwlink/?LinkId=717686)

De volgende lijst beschrijft de basisstappen van het proces. De oplossing bevat code en uitleg om de end-to-end oplossing te bouwen.

* **Batch configureren met een pool van rekenknooppunten (virtuele machines).** U kunt het aantal knooppunten en de grootte van elk knooppunt opgeven.

* **Maak een Data Factory-exemplaar** die is geconfigureerd met entiteiten die staan voor blob-opslag, het Batch-compute-service, i/o-gegevens en een werkstroom/pipeline met activiteiten die gegevens verplaatsen en transformeren.

* **Maak een aangepaste .NET-activiteit in de Data Factory-pijplijn.** De activiteit is uw eigen code die wordt uitgevoerd op de Batch-pool.

* **Store grote hoeveelheden gegevens als blobs in Azure Storage.** Gegevens is onderverdeeld in logische segmenten (meestal door time).

* **Data Factory worden gegevens die wordt verwerkt parallel** naar de secundaire locatie.

* **Data Factory wordt de aangepaste activiteit uitgevoerd met behulp van de groep toegewezen door de Batch.** Data Factory kunt activiteiten tegelijk worden uitgevoerd. Elke activiteit een segment van de gegevens worden verwerkt. De resultaten worden opgeslagen in de opslag.

* **Data Factory de eindresultaten verplaatst naar een derde locatie** om te worden gedistribueerd via een app of voor verdere verwerking door andere hulpprogramma's.

## <a name="implementation-of-the-sample-solution"></a>Implementatie van de Voorbeeldoplossing
De Voorbeeldoplossing is opzettelijk eenvoudig. Het is ontworpen om u te laten zien u hoe u Data Factory en Batch samen met proces-gegevenssets. De oplossing telt het aantal exemplaren van de zoekterm "Microsoft" in invoerbestanden die zijn ingedeeld in een tijdreeks. Deze vervolgens weergeeft het aantal dat moet worden uitvoerbestanden.

**Tijd:** als u bekend met de basisprincipes van Azure Data Factory en Batch bent en de volgende vereisten hebt voltooid, deze oplossing een tot twee uur duurt.

### <a name="prerequisites"></a>Vereisten
#### <a name="azure-subscription"></a>Azure-abonnement
Als u geen Azure-abonnement hebt, kunt u snel een gratis proefaccount maken. Zie voor meer informatie, [gratis proefversie](https://azure.microsoft.com/pricing/free-trial/).

#### <a name="azure-storage-account"></a>Azure Storage-account
U kunt een storage-account gebruiken voor het opslaan van de gegevens in deze zelfstudie. Als u geen een storage-account hebt, raadpleegt u [een opslagaccount maken](../../storage/common/storage-create-storage-account.md#create-a-storage-account). De Voorbeeldoplossing maakt gebruik van blob-opslag.

#### <a name="azure-batch-account"></a>Azure Batch-account
Een Batch-account maken met behulp van de [Azure-portal](http://portal.azure.com/). Zie voor meer informatie, [maken en beheren van een Batch-account](../../batch/batch-account-create-portal.md). Houd er rekening mee de Batch-account naam en de accountsleutel. Ook kunt u de [New-AzureRmBatchAccount](https://msdn.microsoft.com/library/mt603749.aspx) cmdlet voor het maken van een Batch-account. Zie voor instructies over het gebruik van deze cmdlet [aan de slag met Batch PowerShell-cmdlets](../../batch/batch-powershell-cmdlets-get-started.md).

De Voorbeeldoplossing maakt gebruik van Batch (indirect via een data factory-pijplijn) om gegevens te verwerken in een parallelle manier op een pool van rekenknooppunten (een beheerde verzameling virtuele machines).

#### <a name="azure-batch-pool-of-virtual-machines"></a>Azure Batch-pool van virtuele machines
Een Batch-pool maken met ten minste twee rekenknooppunten.

1. In de [Azure-portal](https://portal.azure.com), selecteer **Bladeren** in het menu aan de linkerkant en selecteer **Batch-Accounts**.

1. Selecteer uw Batch-account openen de **Batch-Account** blade.

1. Selecteer de **Pools** tegel.

1. Op de **Pools** blade, selecteer de **toevoegen** knop op de werkbalk om een groep toevoegen.

   a. Geef een ID voor de pool (**groeps-ID**). Houd er rekening mee de ID van de pool. U hebt deze nodig wanneer u de data factory-oplossing.

   b. Geef **Windows Server 2012 R2** voor de **Besturingssysteemfamilie** instelling.

   c. Selecteer een **knooppunt prijscategorie**.

   d. Voer **2** als de waarde voor de **doel toegewezen** instelling.

   e. Voer **2** als de waarde voor de **maximum aantal taken per knooppunt** instelling.

   f. Selecteer **OK** om de pool te maken.

#### <a name="azure-storage-explorer"></a>Azure Opslagverkenner
U gebruikt [Azure Storage Explorer 6](https://azurestorageexplorer.codeplex.com/) of [CloudXplorer](http://clumsyleaf.com/products/cloudxplorer) (van ClumsyLeaf Software) om te controleren en wijzigen van de gegevens in uw Storage-projecten. U kunt ook controleren en wijzigen van de gegevens in de logboeken van uw cloudtoepassingen.

1. Maak een container met de naam **mycontainer** met persoonlijke toegang (geen anonieme toegang).

1. Als u CloudXplorer gebruikt, maakt u mappen en submappen met de volgende structuur:

   ![Structuur van map en submappen](./media/data-factory-data-processing-using-batch/image3.png)

   `Inputfolder` en `outputfolder` zijn op het hoogste niveau mappen in `mycontainer`. De `inputfolder` map bevat submappen met de datum / tijd-stempels (jjjj-MM-DD: HH).

   Als u Storage Explorer, in de volgende stap, uploadt u de bestanden met de volgende namen: `inputfolder/2015-11-16-00/file.txt`, `inputfolder/2015-11-16-01/file.txt`, enzovoort. Deze stap maakt automatisch de mappen.

1. Maak een tekstbestand **bestand.txt** op uw computer met inhoud die het sleutelwoord is **Microsoft**. Een voorbeeld is 'aangepaste activiteit Microsoft test aangepaste activiteit Microsoft test'.

1. Upload het bestand naar de volgende invoer mappen in blob-opslag:

   ![Invoer mappen](./media/data-factory-data-processing-using-batch/image4.png)

   Als u Storage Explorer gebruikt, uploadt u de **bestand.txt** van het bestand in **mycontainer**. Selecteer **kopie** op de werkbalk om een kopie van de blob te maken. In de **kopiëren van de Blob** wijzigen van het dialoogvenster de **blob doelnaam** naar `inputfolder/2015-11-16-00/file.txt`. Herhaal deze stap maken `inputfolder/2015-11-16-01/file.txt`, `inputfolder/2015-11-16-02/file.txt`, `inputfolder/2015-11-16-03/file.txt`, `inputfolder/2015-11-16-04/file.txt`, enzovoort. Deze actie wordt automatisch gemaakt voor de mappen.

1. Maken van een andere container met de naam `customactivitycontainer`. De aangepaste activiteit zip-bestand uploaden naar deze container.

#### <a name="visual-studio"></a>Visual Studio
Installeer Visual Studio 2012 of later om de aangepaste activiteit Batch moet worden gebruikt in de data factory-oplossing te maken.

### <a name="high-level-steps-to-create-the-solution"></a>Op hoog niveau van de stappen voor het maken van de oplossing
1. Maak een aangepaste activiteit die de gegevensverwerking logica bevat.

1. Maak een data factory die gebruikmaakt van de aangepaste activiteit.

### <a name="create-the-custom-activity"></a>De aangepaste activiteit maken
De data factory aangepaste activiteit vormt de kern van de Voorbeeldoplossing van dit. De Voorbeeldoplossing maakt gebruik van Batch om uit te voeren van de aangepaste activiteit. Zie voor meer informatie over het ontwikkelen van aangepaste activiteiten en deze gebruiken in data factory-pijplijnen [aangepaste activiteiten gebruiken in een data factory-pijplijn](data-factory-use-custom-activities.md).

Voor het maken van een aangepaste .NET-activiteit die u kunt gebruiken in een data factory-pijplijn, maakt u een .NET-klassebibliotheekproject met een klasse die de IDotNetActivity-interface implementeert. Deze interface is slechts één methode: uitvoeren. Dit is de handtekening van de methode:

```csharp
public IDictionary<string, string> Execute(
            IEnumerable<LinkedService> linkedServices,
            IEnumerable<Dataset> datasets,
            Activity activity,
            IActivityLogger logger)
```

De methode heeft enkele belangrijke onderdelen die u nodig hebt om te begrijpen:

* De methode nodig vier parameters:

  * **linkedServices**. Deze parameter is een overzichtslijst met gekoppelde services die i/o-gegevensbronnen (bijvoorbeeld blob storage) aan de data factory koppelen. In dit voorbeeld is er slechts één van de gekoppelde service van het type Azure-opslag die wordt gebruikt voor zowel invoer en uitvoer.
  * **gegevenssets**. Deze parameter is een overzichtslijst met gegevenssets. U kunt deze parameter gebruiken om de locaties en schema's die zijn gedefinieerd door gegevenssets voor invoer en uitvoer te verkrijgen.
  * **activiteit**. Deze parameter geeft de huidige compute-entiteit. In dit geval is het een batchservice.
  * **logger**. U kunt de logger foutopsporing opmerkingen die surface schrijven als het logboek 'Gebruiker' voor de pijplijn.
* De methode retourneert een woordenlijst die kan worden gebruikt om aangepaste activiteiten samen in de toekomst koppelen. Deze functie nog niet is geïmplementeerd, kunt u dus een lege woordenlijst retourneren van de methode.

#### <a name="procedure-create-the-custom-activity"></a>Procedure: De aangepaste activiteit maken
1. Maak een .NET-klassebibliotheekproject in Visual Studio.

   a. Start Visual Studio 2012/2013/2015.

   b. Selecteer **Bestand** > **Nieuw** > **Project**.

   c. Vouw **sjablonen**, en selecteer **Visual C\#**. In dit scenario gebruikt u C\#, maar u kunt een .NET-taal gebruiken voor het ontwikkelen van de aangepaste activiteit.

   d. Selecteer **Class Library** uit de lijst met projecttypen aan de rechterkant.

   e. Voer **MyDotNetActivity** voor de **naam**.

   f. Selecteer **C:\\ADF** voor de **locatie**. Maak de map **ADF** als deze niet bestaat.

   g. Selecteer **OK** om het project te maken.

1. Selecteer **extra** > **NuGet-Pakketbeheer** > **Package Manager Console**.

1. In de Package Manager-Console, dan de volgende opdracht om te importeren Microsoft.Azure.Management.DataFactories:

    ```powershell
    Install-Package Microsoft.Azure.Management.DataFactories
    ```
1. Importeren van de **Azure Storage** NuGet-pakket in het project. Omdat u de API voor Blob-opslag in dit voorbeeld gebruiken moet u dit pakket:

    ```powershell
    Install-Package Azure.Storage
    ```
1. Voeg de volgende using-instructies aan het bronbestand in het project:

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
1. Wijzig de naam van de naamruimte **MyDotNetActivityNS**.

    ```csharp
    namespace MyDotNetActivityNS
    ```
1. Wijzig de naam van de klasse **MyDotNetActivity**, en die afkomstig zijn uit de **IDotNetActivity** interface zoals wordt weergegeven:

    ```csharp
    public class MyDotNetActivity : IDotNetActivity
    ```
1. Implementeren (toevoegen) de **Execute** -methode van de **IDotNetActivity** interface met de **MyDotNetActivity** klasse. Kopieer de volgende code toe aan de methode. Zie voor een uitleg van de logica in deze methode gebruikt, de [uitvoermethode](#execute-method) sectie.

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
1. Voeg de volgende methoden toe aan de klasse. Deze methoden worden aangeroepen door de **Execute** methode. Belangrijkste, de **berekenen** methode worden geïsoleerd van de code die door elke blob gegevensbrontabellen loopt.

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
    De methode GetFolderPath retourneert het pad naar de map die de gegevensset naar wijst en de getfilename,-methode retourneert de naam van de blob /-bestand dat de gegevensset naar wijst.

    ```csharp

    "name": "InputDataset",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
            "fileName": "file.txt",
            "folderPath": "mycontainer/inputfolder/{Year}-{Month}-{Day}-{Hour}",
    ```

    De methode Calculate berekent het aantal exemplaren van het sleutelwoord 'Microsoft' in de invoerbestanden (blobs in de map). De zoekterm "Microsoft" is vastgelegd in de code.

1. Het project compileren. Selecteer **bouwen** in het menu en selecteer vervolgens **Build Solution**.

1. Start Windows Verkenner en Ga naar de **bin\\debug** of **bin\\release** map. De keuze van de map is afhankelijk van het type van de build.

1. Maak een zip-bestand **MyDotNetActivity.zip** die bevat alle binaire bestanden in de  **\\bin\\Debug** map. Het is raadzaam om op te nemen van de MyDotNetActivity. **pdb** bestand, zodat u aanvullende informatie zoals het regelnummer krijgen in de broncode waardoor het probleem een fout optreedt.

   ![De lijst van de map bin\Debug](./media/data-factory-data-processing-using-batch/image5.png)

1. Uploaden **MyDotNetActivity.zip** als blob naar de blob-container `customactivitycontainer` in de blob-opslag dat met de StorageLinkedService-service in ADFTutorialDataFactory wordt gekoppeld. Maken van de blob-container `customactivitycontainer` als deze nog niet bestaat.

#### <a name="execute-method"></a>De methode Execute
Deze sectie bevat meer informatie over de code in de Execute-methode.

1. De leden voor de invoerverzameling doorlopen vindt u in de [Microsoft.WindowsAzure.Storage.Blob](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.aspx) naamruimte. Als u wilt de blob-collectie doorlopen, bent u vereist om te gebruiken de **BlobContinuationToken** klasse. In wezen, moet u een do-while-lus met het token als het mechanisme voor het afsluiten van de lus. Zie voor meer informatie, [gebruik Blob storage vanuit .NET](../../storage/blobs/storage-dotnet-how-to-use-blobs.md). Een basic-lus wordt hier weergegeven:

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

1. De code voor het werken met de reeks blobs logisch gaat binnen de do-while-lus. In de **Execute** methode, de-terwijl de lus wordt de lijst met blobs doorgegeven aan een methode met de naam **berekenen**. De methode retourneert een string-variabele met de naam **uitvoer** dat wil zeggen het resultaat van die via alle blobs in het segment herhaald.

   Deze retourneert het aantal exemplaren van de zoekterm "Microsoft" in de blob die is doorgegeven aan de **berekenen** methode.

    ```csharp
    output += string.Format("{0} occurrences of the search term \"{1}\" were found in the file {2}.\r\n", wordCount, searchTerm, inputBlob.Name);
    ```
1. Na de **berekenen** methode is voltooid, moet zijn geschreven naar een nieuwe blob. Voor elke set blobs verwerkt, kan een nieuwe blob worden geschreven met de resultaten. Voor het schrijven naar een nieuwe blob, moet u eerst de uitvoergegevensset vinden.

    ```csharp
    // Get the output dataset by using the name of the dataset matched to a name in the Activity output collection.
    Dataset outputDataset = datasets.Single(dataset => dataset.Name == activity.Outputs.Single().Name);
    ```
1. De code wordt ook de Help-methode **GetFolderPath** om op te halen van het mappad (de containernaam van opslag).

    ```csharp
    folderPath = GetFolderPath(outputDataset);
    ```
   De methode GetFolderPath cast-object voor de gegevensset naar een AzureBlobDataSet, met een eigenschap met de naam FolderPath.

    ```csharp
    AzureBlobDataset blobDataset = dataArtifact.Properties.TypeProperties as AzureBlobDataset;
    
    return blobDataset.FolderPath;
    ```
1. De code roept de **GetFileName** methode voor het ophalen van de naam van het bestand (blobnaam). De code is vergelijkbaar met de vorige code die is gebruikt voor het ophalen van pad naar de map.

    ```csharp
    AzureBlobDataset blobDataset = dataArtifact.Properties.TypeProperties as AzureBlobDataset;
    
    return blobDataset.FileName;
    ```
1. De naam van het bestand is gemaakt met het maken van een URI-object. De URI-constructor gebruikt de **BlobEndpoint** eigenschap om de containernaam te retourneren. De naam van pad en bestandsnaam in de map worden toegevoegd aan het samenstellen van de blob-URI van de uitvoer.  

    ```csharp
    // Write the name of the file.
    Uri outputBlobUri = new Uri(outputStorageAccount.BlobEndpoint, folderPath + "/" + GetFileName(outputDataset));
    ```
1. Nadat de naam van het bestand is geschreven, kunt u de uitvoertekenreeks van de **berekenen** methode naar een nieuwe blob:

    ```csharp
    // Create a blob and upload the output text.
    CloudBlockBlob outputBlob = new CloudBlockBlob(outputBlobUri, outputStorageAccount.Credentials);
    logger.Write("Writing {0} to the output blob", output);
    outputBlob.UploadText(output);
    ```

### <a name="create-the-data-factory"></a>De data factory maken
In de [maken van de aangepaste activiteit](#create-the-custom-activity) sectie maakt u een aangepaste activiteit gemaakt en het zip-bestand met de binaire bestanden en het PDB-bestand geüpload naar een blob-container. In deze sectie maakt maken u een data factory met een pijplijn met behulp van de aangepaste activiteit.

De invoergegevensset voor de aangepaste activiteit vertegenwoordigt de blobs (bestanden) in de invoermap (`mycontainer\\inputfolder`) in blob-opslag. De uitvoergegevensset voor de activiteit vertegenwoordigt de gegevens in blobs in de map voor uitvoer (`mycontainer\\outputfolder`) in blob-opslag.

Een of meer bestanden in de invoermappen plaatsen:

```
mycontainer -\> inputfolder
    2015-11-16-00
    2015-11-16-01
    2015-11-16-02
    2015-11-16-03
    2015-11-16-04
```

Verwijder bijvoorbeeld één bestand (bestand.txt) met de volgende inhoud in elk van de mappen:

```
test custom activity Microsoft test custom activity Microsoft
```

Elke invoermap komt overeen met een segment in de data factory, zelfs als de map twee of meer bestanden heeft. Wanneer elk segment wordt verwerkt door de pijplijn, wordt de aangepaste activiteit doorloopt de blobs in de map invoer voor dat segment.

U ziet vijf uitvoerbestanden met dezelfde inhoud. Het bestand voor uitvoer van het verwerken van het bestand in de map 2015-11-16: 00 heeft bijvoorbeeld de volgende inhoud:

```
2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-00/file.txt.
```

Als u meerdere bestanden (bestand.txt, bestand2.txt samengevoegd, file3.txt) met dezelfde inhoud in de map invoer neerzetten, ziet u de volgende inhoud in het uitvoerbestand. Elke map (2015-11-16: 00, enzovoort) komt overeen met een segment in dit voorbeeld, zelfs als de map meerdere invoerbestanden heeft.

```csharp
2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-00/file.txt.
2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-00/file2.txt.
2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-00/file3.txt.
```

Het uitvoerbestand heeft drie regels nu, één voor elk invoerbestand (blob) in de map die is gekoppeld aan het segment (2015-11-16: 00).

Een taak wordt gemaakt voor elke activiteit die wordt uitgevoerd. In dit voorbeeld moet u er slechts één activiteit is in de pijplijn. Wanneer een segment wordt verwerkt door de pijplijn, wordt de aangepaste activiteit wordt uitgevoerd op Batch voor het verwerken van het segment. Omdat er vijf segmenten (elk segment kan hebben meerdere blobs of -bestand), worden de vijf taken in Batch gemaakt. Wanneer een taak wordt uitgevoerd op Batch, is de aangepaste activiteit die wordt uitgevoerd.

De volgende procedure bevat aanvullende informatie.

#### <a name="step-1-create-the-data-factory"></a>Stap 1: Maak de data factory
1. Nadat u zich aanmeldt bij de [Azure-portal](https://portal.azure.com/), de volgende stappen uit:

   a. Selecteer **nieuw** in het menu links.

   b. Selecteer **gegevens en analyses** op de **nieuw** blade.

   c. Selecteer **Data Factory** op de **gegevensanalyse** blade.

1. Op de **nieuwe data factory** blade Voer **CustomActivityFactory** voor de naam. De naam van de data factory moet een Globally Unique Identifier zijn. Als u de foutmelding 'de naam Data factory CustomActivityFactory is niet beschikbaar', wijzigt u de naam van de data factory. Bijvoorbeeld, gebruik yournameCustomActivityFactory en opnieuw maken van de data factory.

1. Selecteer **GROEPSNAAM voor ACCOUNTRESOURCES**, en selecteer een bestaande resourcegroep of maak een resourcegroep.

1. Controleer of het abonnement en de regio waar u de data factory moet worden gemaakt juist zijn.

1. Selecteer **maken** op de **nieuwe data factory** blade.

1. De data factory is gemaakt in het dashboard van de portal.

1. Nadat de gegevensfactory is gemaakt, ziet u de **Data factory** pagina, waarin u de inhoud van de data factory.

   ![Pagina Data factory](./media/data-factory-data-processing-using-batch/image6.png)

#### <a name="step-2-create-linked-services"></a>Stap 2: Maak gekoppelde services
Gekoppelde services worden gegevensarchieven of compute-services aan een data factory. In deze stap koppelt u uw opslagaccount en de Batch-account aan uw data factory.

#### <a name="create-an-azure-storage-linked-service"></a>Een gekoppelde Azure Storage-service maken
1. Selecteer de **auteur en implementeer** tegel op de **Data factory** blade voor **CustomActivityFactory**. De Data Factory-Editor wordt weergegeven.

1. Selecteer **nieuw gegevensarchief** op de opdrachtbalk en kies **Azure storage.** De JSON-script gebruikt u voor het maken van een gekoppelde service in de editor wordt weergegeven.

   ![Nieuwe gegevensopslag](./media/data-factory-data-processing-using-batch/image7.png)

1. Vervang de **accountnaam** door de naam van uw opslagaccount. Vervang de **accountsleutel** met de toegangssleutel van uw opslagaccount. Zie voor meer informatie over het verkrijgen van uw toegangssleutel voor opslag, [weergeven, kopiëren en opnieuw genereren opslag toegangssleutels](../../storage/common/storage-create-storage-account.md#manage-your-storage-account).

1. Selecteer in de opdrachtbalk **Implementeren** om de gekoppelde service te implementeren.

   ![Implementeren](./media/data-factory-data-processing-using-batch/image8.png)

#### <a name="create-an-azure-batch-linked-service"></a>Een gekoppelde Azure-Batch-service maken
In deze stap maakt u een gekoppelde service voor uw Batch-account dat wordt gebruikt voor het uitvoeren van de aangepaste activiteit van data factory.

1. Selecteer **nieuwe berekening** op de opdrachtbalk en kies **Azure Batch.** De JSON-script maakt u een Batch gekoppelde service in de editor wordt weergegeven.

1. In de JSON-script:

   a. Vervang **accountnaam** met de naam van uw Batch-account.

   b. Vervang **toegangssleutel** door de toegangssleutel van het Batch-account.

   c. Voer de ID van de groep voor de **poolName** eigenschap. Voor deze eigenschap, kunt u opgeven de naam van de groep van toepassingen of de id van de groep van toepassingen.

   d. Voer de batch-URI voor de **batchUri** JSON-eigenschap.

      > [!IMPORTANT]
      > De URL van de **Batch-Account** blade is in de volgende indeling: \<accountname\>.\< regio\>. batch.azure.com. Voor de **batchUri** eigenschap in de JSON-script dat u wilt verwijderen van a88 "accountname." ** uit de URL. Een voorbeeld is `"batchUri": "https://eastus.batch.azure.com"`.
      >
      >

      ![Blade batch-Account](./media/data-factory-data-processing-using-batch/image9.png)

      Voor de **poolName** eigenschap, kunt u ook de ID van de pool in plaats van de naam van de pool opgeven.

      > [!NOTE]
      > De Data Factory-service biedt geen op aanvraag optie ondersteuning voor Batch als voor HDInsight. U kunt alleen uw eigen Batch-pool gebruiken in een data factory.
      >
      >
   
   e. Geef **StorageLinkedService** voor de **linkedServiceName** eigenschap. U hebt deze gekoppelde service gemaakt in de vorige stap. Deze opslag wordt gebruikt als een faseringsgebied voor bestanden en Logboeken.

1. Selecteer in de opdrachtbalk **Implementeren** om de gekoppelde service te implementeren.

#### <a name="step-3-create-datasets"></a>Stap 3: Maak gegevenssets
In deze stap maakt u gegevenssets voor invoer-en uitvoergegevens.

#### <a name="create-the-input-dataset"></a>De invoergegevensset maken
1. Selecteer in de Data Factory Editor de **nieuwe gegevensset** op de werkbalk. Selecteer **Azure Blob-opslag** uit de vervolgkeuzelijst.

1. Vervang de JSON-script in het rechterdeelvenster met de volgende JSON-fragment:

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

    Maakt u een pijplijn verderop in dit scenario met de begintijd 2015-11-16T00:00:00Z en aan het eind tijd 2015-11-16T05:00:00Z. Deze is ingesteld voor het produceren van gegevens per uur, dus er vijf i/o-segmenten zijn (tussen **00**: 00:00 -\> **05**: 00:00).

    De **frequentie** en **interval** voor de invoergegevensset zijn ingesteld op **uur** en **1**, wat betekent dat het invoersegment beschikbaar per uur is.

    De begintijd voor elk segment wordt vertegenwoordigd door de **SliceStart** systeemvariabele in de vorige JSON-codefragment. Hier volgen de begintijden voor elk segment.

    | **Segment** | **Begintijd**          |
    |-----------|-------------------------|
    | 1         | 2015-11-16T**00**:00:00 |
    | 2         | 2015-11-16T**01**:00:00 |
    | 3         | 2015-11-16T**02**:00:00 |
    | 4         | 2015-11-16T**03**:00:00 |
    | 5         | 2015-11-16T**04**:00:00 |

    De **folderPath** wordt berekend met behulp van het jaar, maand, dag en uur deel van de starttijd van segment (**SliceStart**). Hier ziet u hoe een invoermap is toegewezen aan een segment.

    | **Segment** | **Begintijd**          | **Invoermap**  |
    |-----------|-------------------------|-------------------|
    | 1         | 2015-11-16T**00**:00:00 | 2015-11-16-**00** |
    | 2         | 2015-11-16T**01**:00:00 | 2015-11-16-**01** |
    | 3         | 2015-11-16T**02**:00:00 | 2015-11-16-**02** |
    | 4         | 2015-11-16T**03**:00:00 | 2015-11-16-**03** |
    | 5         | 2015-11-16T**04**:00:00 | 2015-11-16-**04** |

1. Selecteer **implementeren** op de werkbalk om te maken en implementeren van de **InputDataset** tabel.

#### <a name="create-the-output-dataset"></a>De uitvoergegevensset maken
In deze stap maakt u een andere gegevensset van het type AzureBlob om weer te geven van de uitvoergegevens.

1. Selecteer in de Data Factory Editor de **nieuwe gegevensset** op de werkbalk. Selecteer **Azure Blob-opslag** uit de vervolgkeuzelijst.

1. Vervang de JSON-script in het rechterdeelvenster met de volgende JSON-fragment:

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

    Een uitvoer-blob/bestand is gegenereerd voor elke invoersegment. Hier ziet u hoe een uitvoerbestand voor elk segment wordt genoemd. De uitvoerbestanden worden gegenereerd in een map voor uitvoer, `mycontainer\\outputfolder`.

    | **Segment** | **Begintijd**          | **Uitvoerbestand**       |
    |-----------|-------------------------|-----------------------|
    | 1         | 2015-11-16T**00**:00:00 | 2015-11-16-**00.txt** |
    | 2         | 2015-11-16T**01**:00:00 | 2015-11-16-**01.txt** |
    | 3         | 2015-11-16T**02**:00:00 | 2015-11-16-**02.txt** |
    | 4         | 2015-11-16T**03**:00:00 | 2015-11-16-**03.txt** |
    | 5         | 2015-11-16T**04**:00:00 | 2015-11-16-**04.txt** |

    Houd er rekening mee dat alle bestanden in een map invoer (bijvoorbeeld 2015-11-16: 00) deel van een segment met de begintijd 2015-11-16: 00 uitmaken. Wanneer dit segment wordt verwerkt, de aangepaste activiteit via elk bestand wordt gescand en produceert een regel in het uitvoerbestand met het aantal exemplaren van de zoekterm 'Microsoft'. Als er drie bestanden in de map 2015-11-16: 00, zijn er drie regels in de uitvoer bestand 2015-11-16-00.txt.

1. Selecteer **implementeren** op de werkbalk om te maken en implementeren van de **OutputDataset**.

#### <a name="step-4-create-and-run-the-pipeline-with-a-custom-activity"></a>Stap 4: Maken en uitvoeren van de pijplijn met een aangepaste activiteit
In deze stap maakt u een pijplijn met één activiteit, de aangepaste activiteit die u eerder hebt gemaakt.

> [!IMPORTANT]
> Als u dit nog niet hebt geüpload **bestand.txt** invoer mappen in de blob-container, doen voordat u de pijplijn maken. De **isPaused** eigenschap is ingesteld op false in de pijplijn-JSON, zodat de pijplijn wordt onmiddellijk uitgevoerd omdat de **start** datum ligt in het verleden.
>
>

1. Selecteer in de Data Factory Editor **nieuwe pijplijn** op de opdrachtbalk. Als u de opdracht niet ziet, selecteert u het weglatingsteken weer te geven.

1. Vervang de JSON-script in het rechterdeelvenster met de volgende JSON-fragment:

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

   * Slechts één activiteit in de pijplijn is en is van het type **DotNetActivity**.
   * **AssemblyName** is ingesteld op de naam van het DLL-bestand **MyDotNetActivity.dll**.
   * **EntryPoint** is ingesteld op **MyDotNetActivityNS.MyDotNetActivity**. Het is in feite \<naamruimte\>.\< ClassName\> in uw code.
   * **PackageLinkedService** is ingesteld op **StorageLinkedService**, die verwijzen naar de blob-opslag met de aangepaste activiteit zip-bestand. Als u verschillende opslagaccounts voor i/o-bestanden en de aangepaste activiteit zip-bestand gebruikt, hebt u een andere gekoppelde Storage-service maken. In dit artikel wordt ervan uitgegaan dat u hetzelfde opslagaccount.
   * **PackageFile** is ingesteld op **customactivitycontainer/MyDotNetActivity.zip**. Het is in de indeling \<containerforthezip\>/\<nameofthezip.zip\>.
   * De aangepaste activiteit neemt **InputDataset** als invoer en **OutputDataset** als uitvoer.
   * De **linkedServiceName** eigenschap van de aangepaste activiteit wijst naar **AzureBatchLinkedService**, waarin staat dat Data Factory die de aangepaste activiteit moet worden uitgevoerd op Batch.
   * De **gelijktijdigheid** instelling is belangrijk. Als u de standaardwaarde 1, is zelfs als u twee of meer in de Batch-pool rekenknooppunten, de segmenten worden verwerkt na elkaar. Daarom worden niet u u profiteert van de parallelle verwerking van de mogelijkheid van Batch. Als u instelt **gelijktijdigheid** naar een hogere waarde, bijvoorbeeld 2 is, betekent dit dat twee segmenten (komt overeen met twee taken in Batch) op hetzelfde moment kunnen worden verwerkt. In dit geval wordt worden de virtuele machines in de Batch-pool gebruikt. De eigenschap gelijktijdigheid van taken op de juiste wijze ingesteld.
   * Slechts één taak (segment) wordt uitgevoerd op een virtuele machine op elk gewenst moment standaard. Standaard **maximum aantal taken per VM** is ingesteld op 1 voor een Batch-pool. Als onderdeel van de vereisten, kunt u een groep gemaakt met deze eigenschap is ingesteld op 2. Daarom kunnen twee gegevenssegmenten voor factory uitvoeren op een virtuele machine op hetzelfde moment.
    - De **isPaused** eigenschap is standaard ingesteld op false. De pijplijn wordt onmiddellijk uitgevoerd in dit voorbeeld omdat de segmenten in het verleden. U kunt deze eigenschap instellen op **waar** onderbreken van de pijplijn en weer in **false** opnieuw te starten.
    -   De **start** en **end** tijden zijn vijf uur uit elkaar liggen. Segmenten worden per uur geproduceerd, zodat de vijf segmenten worden geproduceerd door de pijplijn.

1. Selecteer in de opdrachtbalk **Implementeren** om de pijplijn te implementeren.

#### <a name="step-5-test-the-pipeline"></a>Stap 5: De pijplijn testen
In deze stap maakt testen u de pijplijn door het verwijderen van bestanden in de invoer-mappen. Begin met het testen van de pijplijn met één bestand voor elke map invoer.

1. Op de **Data factory** -blade in Azure portal, selecteer **Diagram**.

   ![Diagram](./media/data-factory-data-processing-using-batch/image10.png)

1. In de **Diagram** weergeven, dubbelklikt u op de invoergegevensset **InputDataset**.

   ![InputDataset](./media/data-factory-data-processing-using-batch/image11.png)

1. De **InputDataset** blade wordt weergegeven met alle vijf segmenten gereed. U ziet dat de **segment BEGINTIJD** en **EINDTIJD van segment** voor elk segment.

   ![Segment start invoer- en eindtijd](./media/data-factory-data-processing-using-batch/image12.png)

1. In de **Diagram** weergave, selecteer **OutputDataset**.

1. De vijf uitvoersegmenten worden weergegeven in de **gereed** status als ze zijn geproduceerd.

   ![Uitvoer segment begin- en eindtijd](./media/data-factory-data-processing-using-batch/image13.png)

1. De portal gebruiken om de taken die zijn gekoppeld aan de segmenten weer te geven en kijk wat elk segment worden uitgevoerd op de VM. Zie voor meer informatie de [integratie van Data Factory en Batch](#data-factory-and-batch-integration) sectie.

1. De uitvoerbestanden worden weergegeven onder `mycontainer` in `outputfolder` in uw blob storage.

   ![Uitvoerbestanden in de opslag](./media/data-factory-data-processing-using-batch/image15.png)

   Vijf uitvoerbestanden worden vermeld, één voor elk segment invoer. Elk van de uitvoerbestanden bevat inhoud die vergelijkbaar is met de volgende uitvoer:

    ```
    2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-00/file.txt.
    ```
   Het volgende diagram illustreert hoe de data factory-segmenten worden toegewezen aan taken in Batch. In dit voorbeeld heeft een segment slechts één uitvoeren.

   ![Diagram van de toewijzing van segment](./media/data-factory-data-processing-using-batch/image16.png)

1. Probeer nu met meerdere bestanden in een map. Maken van de bestanden **bestand2.txt samengevoegd**, **file3.txt**, **file4.txt**, en **file5.txt** met dezelfde inhoud zoals in bestand.txt in de map **2015-11-06-01**.

1. In de map voor uitvoer, verwijdert u het uitvoerbestand **2015-11-16-01.txt**.

1. Op de **OutputDataset** blade met de rechtermuisknop op het segment met **segment BEGINTIJD** ingesteld op **11/16/2015 01:00:00 AM**. Selecteer **uitvoeren** voor verwerking opnieuw apparaat uitvoeren of het segment opnieuw uitvoeren. Het segment heeft nu de vijf bestanden in plaats van een bestand.

    ![Voer](./media/data-factory-data-processing-using-batch/image17.png)

1. Nadat het segment de status wordt uitgevoerd en de status ervan **gereed**, Controleer of de inhoud van het uitvoerbestand voor dit segment (**2015-11-16-01.txt**). Het uitvoerbestand wordt weergegeven onder `mycontainer` in `outputfolder` in uw blob storage. Er moet een regel voor elk bestand van het segment.

    ```
    2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-01/file.txt.
    2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-01/file2.txt.
    2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-01/file3.txt.
    2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-01/file4.txt.
    2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-01/file5.txt.
    ```

> [!NOTE]
> Als u de uitvoer bestand 2015-11-16-01.txt voordat u probeert met vijf invoerbestanden niet verwijdert, ziet u een regel uit de vorige segment uitvoering en vijf regels van het huidige segment uitvoeren. De inhoud wordt toegevoegd aan het uitvoerbestand standaard, als deze al bestaat.
>
>

#### <a name="data-factory-and-batch-integration"></a>Integratie van Data Factory en Batch
De Data Factory-service maakt een taak in Batch met de naam `adf-poolname:job-xxx`.

![Batch-taken](media/data-factory-data-processing-using-batch/data-factory-batch-jobs.png)

Een taak in de taak wordt gemaakt voor elke uitvoering van de activiteit van een segment. Als 10 segmenten worden verwerkt, wordt 10 taken worden gemaakt in de taak. U kunt meer dan één segment parallel worden uitgevoerd als er meerdere rekenknooppunten in de groep hebben. Als het maximum aantal taken per knooppunt is ingesteld op meer dan één, kunt meer dan één segment in de dezelfde berekening uitvoeren.

In dit voorbeeld zijn het vijf segmenten, dus er vijf taken in Batch zijn. Met **gelijktijdigheid** ingesteld op **5** in de pijplijn-JSON in de data factory en **maximum aantal taken per VM** ingesteld op **2** in de Batch-pool met **2** virtuele machines, de taken snel worden uitgevoerd. (De begin- en eindtijden voor taken controleren).

De portal gebruiken om de Batch-taak en de bijbehorende taken die gekoppeld aan de segmenten zijn weer te geven en kijk wat elk segment worden uitgevoerd op de VM.

![Batch-taken](media/data-factory-data-processing-using-batch/data-factory-batch-job-tasks.png)

### <a name="debug-the-pipeline"></a>Fouten opsporen in de pijplijn
De foutopsporing bestaat uit enkele basistechnieken.

1. Als het invoersegment niet is ingesteld op **gereed**, Controleer of de invoermapstructuur juist is en dat bestand.txt bestaat in de invoermappen.

   ![Invoermapstructuur](./media/data-factory-data-processing-using-batch/image3.png)

1. In de **Execute** methode van de aangepaste activiteit, gebruik de **IActivityLogger** object om informatie die helpt u bij het oplossen van problemen te registreren. De geregistreerde berichten weergegeven in de gebruiker\_0-logboekbestand.

   Op de **OutputDataset** blade, selecteert u het segment voor de **gegevenssegment** blade voor dat segment. Onder **uitvoeringen van activiteit**, ziet u een activiteit die wordt uitgevoerd voor het segment. Als u selecteert **uitvoeren** in de opdrachtbalk, kun u een andere activiteit die wordt uitgevoerd voor hetzelfde segment.

   Wanneer u de activiteit die wordt uitgevoerd selecteert, ziet u de **details uitvoering van activiteit** blade met een lijst van logboekbestanden. U ziet dat geregistreerde berichten in de gebruiker\_0-logboekbestand. Wanneer een fout optreedt, ziet u drie uitvoeringen van activiteit omdat het maximale aantal pogingen is ingesteld op 3 in de pijplijn/JSON van de activiteit. Wanneer u de uitvoering van activiteit selecteert, ziet u de logboekbestanden die u bekijken kunt voor het oplossen van de fout.

   ![Gegevens en OutputDataset segment blades](./media/data-factory-data-processing-using-batch/image18.png)

   Selecteer in de lijst met logboekbestanden, **user-0.log**. In het rechterdeelvenster de resultaten van het gebruik van de **IActivityLogger.Write** methode worden weergegeven.

   ![Blade met details uitvoering van activiteit](./media/data-factory-data-processing-using-batch/image19.png)

   Controleer de systeem-0.log voor elk systeem foutberichten en uitzonderingen.

    ```
    Trace\_T\_D\_12/6/2015 1:43:35 AM\_T\_D\_\_T\_D\_Verbose\_T\_D\_0\_T\_D\_Loading assembly file MyDotNetActivity...
    
    Trace\_T\_D\_12/6/2015 1:43:35 AM\_T\_D\_\_T\_D\_Verbose\_T\_D\_0\_T\_D\_Creating an instance of MyDotNetActivityNS.MyDotNetActivity from assembly file MyDotNetActivity...
    
    Trace\_T\_D\_12/6/2015 1:43:35 AM\_T\_D\_\_T\_D\_Verbose\_T\_D\_0\_T\_D\_Executing Module
    
    Trace\_T\_D\_12/6/2015 1:43:38 AM\_T\_D\_\_T\_D\_Information\_T\_D\_0\_T\_D\_Activity e3817da0-d843-4c5c-85c6-40ba7424dce2 finished successfully
    ```
1. Bevatten de **PDB** bestand in het zip-bestand, zodat de foutdetails informatie zoals de aanroepstack hebben wanneer er een fout optreedt.

1. Alle bestanden in het zip-bestand voor de aangepaste activiteit moeten zich op het hoogste niveau zonder submappen.

   ![Lijst met aangepaste activiteit zip-bestanden](./media/data-factory-data-processing-using-batch/image20.png)

1. Zorg ervoor dat **assemblyName** (MyDotNetActivity.dll), **entryPoint** (MyDotNetActivityNS.MyDotNetActivity), **packageFile** (customactivitycontainer / MyDotNetActivity.zip), en **packageLinkedService** (moet verwijzen naar de blob-opslag die het zip-bestand bevat) zijn ingesteld op de juiste waarden.

1. Als u een fout en opnieuw wilt verwerken van het segment de status opgelost, met de rechtermuisknop op het segment in de **OutputDataset** blade en selecteer **uitvoeren**.

   ![Blade OutputDataset optie uitvoeren](./media/data-factory-data-processing-using-batch/image21.png)

   > [!NOTE]
   > Een container bevindt zich in de blob-opslag met de naam `adfjobs`. Deze container wordt niet automatisch verwijderd, maar u kunt deze veilig verwijderen nadat u klaar bent met testen van de oplossing. Op dezelfde manier de data factory-oplossing maakt u een batchtaak met de naam `adf-\<pool ID/name\>:job-0000000001`. Nadat u de oplossing als u dit wilt testen, kunt u deze taak verwijderen.
   >
   >
1. De aangepaste activiteit maakt geen gebruik van de **app.config** -bestand van het pakket. Dus als uw code verbindingstekenreeksen van het configuratiebestand leest, werkt niet tijdens runtime. Het is het beste wanneer u Batch gebruikt is voor het opslaan van alle geheimen in Azure Key Vault. Vervolgens gebruiken een certificaat op basis van service-principal voor de key vault beveiligen en distribueren van het certificaat naar de Batch-pool. De aangepaste .NET-activiteit hebben toegang tot geheimen van de key vault tijdens runtime. Deze algemene oplossing kan worden geschaald naar elk type geheim, niet alleen een verbindingsreeks.

    Er is een eenvoudigere oplossing, maar het is niet een best practice. U kunt een service van SQL database gekoppeld tekenreeks instellingen met verbinding maken. Vervolgens kunt u een gegevensset die gebruikmaakt van de gekoppelde service maken en koppelen van de gegevensset als een dummy-invoergegevensset aan de aangepaste .NET-activiteit. U kunt vervolgens toegang tot de verbindingsreeks van de gekoppelde service in de code van de aangepaste activiteit. Dit moet werkt prima tijdens runtime.  

#### <a name="extend-the-sample"></a>Het voorbeeld uitbreiden
U kunt dit voorbeeld voor meer informatie over Data Factory en Batch-functies kunt uitbreiden. Neem bijvoorbeeld de volgende stappen uit voor het verwerken van segmenten in een ander tijdsbereik:

1. Voeg de volgende submappen in `inputfolder`: 2015-11-16-05, 2015-11-16-06 201-11-16-07-2011-11-16-08 en 2015-11-16-09. Plaats de invoerbestanden in deze mappen. Wijzigen van de eindtijd voor de pijplijn uit `2015-11-16T05:00:00Z` naar `2015-11-16T10:00:00Z`. In de **Diagram** weergeven, dubbelklikt u op **InputDataset** en bevestigt u dat de invoersegmenten gereed zijn. Dubbelklik op **OutputDataset** om te zien van de status van de uitvoersegmenten. Als deze zich in de **gereed** staat, controleert u de map voor uitvoer voor de uitvoerbestanden.

1. Vergroten of verkleinen van de **gelijktijdigheid** instelling om te begrijpen hoe dit van invloed is op de prestaties van uw oplossing, met name de verwerking die wordt uitgevoerd op Batch. Voor meer informatie over de **gelijktijdigheid** stellen, Zie ' stap 4: maken en uitvoeren van de pijplijn met een aangepaste activiteit. "

1. Een pool maken met nieuwere/oudere **maximum aantal taken per VM**. Gekoppelde gebruikt u de nieuwe groep die u hebt gemaakt, het bijwerken van de Batch-service in de data factory-oplossing. Voor meer informatie over de **maximum aantal taken per VM** stellen, Zie ' stap 4: maken en uitvoeren van de pijplijn met een aangepaste activiteit. "

1. Maak een Batch-pool met de **voor automatisch schalen** functie. Automatisch schalen rekenknooppunten in een Batch-pool is de dynamische aanpassing van de verwerkingscapaciteit die worden gebruikt door uw toepassing. 

    De voorbeeldformule hier bereikt het volgende gedrag. Wanneer de pool in eerste instantie wordt gemaakt, wordt deze begint met één virtuele machine. De metriek $PendingTasks definieert het aantal taken in de die wordt uitgevoerd en actieve (in de wachtrij) staat. De formule wordt het gemiddelde aantal in behandeling zijnde taken gevonden in de afgelopen 180 seconden en TargetDedicated dienovereenkomstig ingesteld. Het zorgt ervoor dat TargetDedicated nooit meer dan 25 VM's gaat. Als nieuwe taken worden verzonden, wordt de pool automatisch vergroot. Als de taken zijn voltooid, virtuele machines gratis één voor één en worden deze virtuele machines Hiermee verkleint u de automatische schaalaanpassing. U kunt startingNumberOfVMs en maxNumberofVMs aanpassen aan uw behoeften.
 
    Formule voor automatisch schalen:

    ``` 
    startingNumberOfVMs = 1;
    maxNumberofVMs = 25;
    pendingTaskSamplePercent = $PendingTasks.GetSamplePercent(180 * TimeInterval_Second);
    pendingTaskSamples = pendingTaskSamplePercent < 70 ? startingNumberOfVMs : avg($PendingTasks.GetSample(180 * TimeInterval_Second));
    $TargetDedicated=min(maxNumberofVMs,pendingTaskSamples);
    ```

   Zie voor meer informatie, [automatisch schalen rekenknooppunten in een Batch-pool](../../batch/batch-automatic-scaling.md).

   Als de groep gebruikmaakt van de [autoScaleEvaluationInterval](https://msdn.microsoft.com/library/azure/dn820173.aspx), 15 tot 30 minuten aan de virtuele machine voorbereiden voordat u de aangepaste activiteit kan worden uitgevoerd door de Batch-service. Als de groep gebruikmaakt van een andere autoScaleEvaluationInterval, kan de Batch-service autoScaleEvaluationInterval plus 10 minuten duren.

1. In de Voorbeeldoplossing is de **Execute** methode roept de **berekenen** methode waarmee een segment invoergegevens om te produceren van een uitvoergegevenssegment wordt verwerkt. Kunt u uw eigen methode voor het verwerken van invoergegevens en vervang de **berekenen** methode aanroepen in de **Execute** methode met een aanroep naar de methode.

### <a name="next-steps-consume-the-data"></a>Volgende stappen: de gegevens gebruiken
Nadat u gegevens verwerken, kunt u deze gebruiken met onlineprogramma's zoals Power BI. Hier vindt u koppelingen naar informatie waarmee u inzicht in de Power BI en het gebruik ervan in Azure:

* [Een gegevensset in Power BI verkennen](https://powerbi.microsoft.com/documentation/powerbi-service-get-data/)
* [Aan de slag met Power BI Desktop](https://powerbi.microsoft.com/documentation/powerbi-desktop-getting-started/)
* [Gegevens vernieuwen in Power BI](https://powerbi.microsoft.com/documentation/powerbi-refresh-data/)
* [Azure en Power BI: basisoverzicht](https://powerbi.microsoft.com/documentation/powerbi-azure-and-power-bi/)

## <a name="references"></a>Verwijzingen
* [Azure Data Factory](https://azure.microsoft.com/documentation/services/data-factory/)

  * [Inleiding tot de Data Factory-service](data-factory-introduction.md)
  * [Aan de slag met Data Factory](data-factory-build-your-first-pipeline.md)
  * [Aangepaste activiteiten gebruiken in een Data Factory-pijplijn](data-factory-use-custom-activities.md)
* [Azure Batch](https://azure.microsoft.com/documentation/services/batch/)

  * [Basisprincipes van Batch](../../batch/batch-technical-overview.md)
  * [Overzicht van Batch-functies](../../batch/batch-api-basics.md)
  * [Een batchaccount in Azure portal maken en beheren](../../batch/batch-account-create-portal.md)
  * [Aan de slag met de Batch-clientbibliotheek voor .NET](../../batch/quick-run-dotnet.md)

[batch-explorer]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchExplorer
[batch-explorer-walkthrough]: http://blogs.technet.com/b/windowshpc/archive/2015/01/20/azure-batch-explorer-sample-walkthrough.aspx
