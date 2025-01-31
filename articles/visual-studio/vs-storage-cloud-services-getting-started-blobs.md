---
title: Aan de slag met blob storage en Visual Studio verbonden services (cloudservices) | Microsoft Docs
description: Aan de slag met Azure Blob-opslag in een cloud service-project in Visual Studio nadat u verbinding met een opslagaccount met behulp van Visual Studio verbonden services
services: storage
author: ghogen
manager: douge
ms.assetid: 1144a958-f75a-4466-bb21-320b7ae8f304
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.custom: vs-azure
ms.workload: azure-vs
ms.topic: conceptual
ms.date: 12/02/2016
ms.author: ghogen
ms.openlocfilehash: 9f1ef06e0275954343c548d0f6937b9c6fbcfd18
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "62122937"
---
# <a name="get-started-with-azure-blob-storage-and-visual-studio-connected-services-cloud-services-projects"></a>Aan de slag met Azure Blob Storage en Visual Studio verbonden services (cloudserviceprojecten)
[!INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>Overzicht
In dit artikel wordt beschreven hoe u aan de slag met Azure Blob Storage, nadat u hebt gemaakt of een Azure Storage-account waarnaar wordt verwezen door het gebruik van de Visual Studio **Connected Services toevoegen** dialoogvenster in een Visual Studio-cloud services-project. We laten u hoe u toegang tot en blob-containers maken en hoe u veelvoorkomende taken uitvoeren zoals uploaden, weergeven en downloaden van blobs. De voorbeelden zijn geschreven in C\# en gebruik de [Microsoft Azure Storage-clientbibliotheek voor .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx).

Azure Blob-opslag is een service voor het opslaan van grote hoeveelheden ongestructureerde gegevens die kunnen worden benaderd vanaf elke locatie ter wereld via HTTP of HTTPS. Een enkele blob kan elke grootte zijn. Blobs kunnen items zijn zoals afbeeldingen, audio en video-bestanden, onbewerkte gegevens en bestanden.

Net zoals bestanden in mappen staan, staan opslag-blobs in containers. Nadat u een opslagaccount hebt gemaakt, maakt u een of meer containers in de opslag. Bijvoorbeeld in een opslag met de naam 'Plakboek', kunt u containers maken in de opslag met de naam 'installatiekopieën' voor het opslaan van afbeeldingen en andere genaamd "audio" audio-bestanden wilt opslaan. Nadat u de containers hebt gemaakt, kunt u afzonderlijke blob-bestanden uploaden naar deze.

* Zie voor meer informatie over het bewerken van programmatisch blobs [aan de slag met Azure Blob storage met .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md).
* Raadpleeg voor algemene informatie over Azure Storage [documentatie voor Storage](https://azure.microsoft.com/documentation/services/storage/).
* Raadpleeg voor algemene informatie over Azure Cloud Services, [documentatie voor Cloud Services](https://azure.microsoft.com/documentation/services/cloud-services/).
* Zie voor meer informatie over het programmeren van ASP.NET-toepassingen [ASP.NET](https://www.asp.net).

## <a name="access-blob-containers-in-code"></a>Toegang tot blob-containers in code
Om programmatisch toegang verkrijgen tot de blobs in de cloud service-projecten, moet u de volgende items toevoegen als ze nog niet aanwezig zijn.

1. De volgende code naamruimtedeclaraties toevoegen aan het begin van een C#-bestand waarin u wilt programmatisch toegang verkrijgen tot Azure Storage.
   
        using Microsoft.Framework.Configuration;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Blob;
        using System.Threading.Tasks;
        using LogLevel = Microsoft.Framework.Logging.LogLevel;
2. Krijgen een **CloudStorageAccount** -object met gegevens van uw opslagaccount. Gebruik de volgende code om op te halen de uw verbindingsreeks voor opslag en gegevens over het opslagaccount van de configuratie van de Azure-service.
   
        CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("<storage account name>_AzureStorageConnectionString"));
3. Krijgen een **CloudBlobClient** object om te verwijzen naar een bestaande container in uw opslagaccount.
   
        // Create a blob client.
        CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
4. Krijgen een **CloudBlobContainer** object om te verwijzen naar een specifieke blob-container.
   
        // Get a reference to a container named "mycontainer."
        CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

> [!NOTE]
> Alle van de code die wordt weergegeven in de vorige procedure voor de code die wordt weergegeven in de volgende secties gebruiken.
> 
> 

## <a name="create-a-container-in-code"></a>Een container in code maken
> [!NOTE]
> Aantal API's die het uitvoeren van aanroepen uit naar Azure Storage in ASP.NET zijn asynchroon. Zie [asynchrone programmering met Async en Await](https://msdn.microsoft.com/library/hh191443.aspx) voor meer informatie. De code in het volgende voorbeeld wordt ervan uitgegaan dat u van asynchrone programmering methoden gebruikmaakt.
> 
> 

Voor het maken van een container in uw opslagaccount, hoeft u alleen maar is Voeg een aanroep naar **CreateIfNotExistsAsync** zoals in de volgende code:

    // If "mycontainer" doesn't exist, create it.
    await container.CreateIfNotExistsAsync();


Als u wilt de bestanden in de container beschikbaar maken voor iedereen, kunt u de container openbaar zijn met behulp van de volgende code instellen.

    await container.SetPermissionsAsync(new BlobContainerPermissions
    {
        PublicAccess = BlobContainerPublicAccessType.Blob
    });


Iedereen op Internet kan blobs in een openbare container zien, maar u kunt wijzigen of ze alleen verwijderen als u de juiste toegangssleutel hebben.

## <a name="upload-a-blob-into-a-container"></a>Een blob uploaden naar een container
Azure Storage ondersteunt blok-blobs en pagina-blobs. In de meeste gevallen is een blok-blob het aangewezen type om te gebruiken.

Om een bestand naar een blok-blob te uploaden, haalt u een containerverwijzing op en gebruikt u deze om een blok-blobverwijzing op te halen. Zodra u een blobverwijzing hebt, kunt u er elke gewenste gegevensstroom naar uploaden door de methode **UploadFromStream** aan te roepen. Met deze bewerking wordt de blob gemaakt als deze nog niet bestaat, of overschreven als deze wel al bestaat. Het volgende voorbeeld laat zien hoe u een blob uploadt naar een container. Hierbij wordt ervan uitgegaan dat de container al is gemaakt.

    // Retrieve a reference to a blob named "myblob".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob");

    // Create or overwrite the "myblob" blob with contents from a local file.
    using (var fileStream = System.IO.File.OpenRead(@"path\myfile"))
    {
        blockBlob.UploadFromStream(fileStream);
    }

## <a name="list-the-blobs-in-a-container"></a>De blobs in een container in een lijst weergeven
Als u een lijst van de blobs in een container wilt weergeven, moet u eerst een containerverwijzing ophalen. Vervolgens kunt u de methode **ListBlobs** van de container gebruiken voor het ophalen van de blobs en/of de mappen hierin. Voor toegang tot de uitgebreide set eigenschappen en methoden voor een geretourneerde **IListBlobItem**, u moet dit casten naar een **CloudBlockBlob**, **CloudPageBlob**, of  **CloudBlobDirectory** object. Als het type onbekend is, kunt u typecontrole gebruiken om te bepalen waarnaar het moet worden gecast. De volgende code toont hoe de URI van elk item in de **photos**-container wordt opgehaald en uitgevoerd:

    // Loop over items within the container and output the length and URI.
    foreach (IListBlobItem item in container.ListBlobs(null, false))
    {
        if (item.GetType() == typeof(CloudBlockBlob))
        {
            CloudBlockBlob blob = (CloudBlockBlob)item;

            Console.WriteLine("Block blob of length {0}: {1}", blob.Properties.Length, blob.Uri);

        }
        else if (item.GetType() == typeof(CloudPageBlob))
        {
            CloudPageBlob pageBlob = (CloudPageBlob)item;

            Console.WriteLine("Page blob of length {0}: {1}", pageBlob.Properties.Length, pageBlob.Uri);

        }
        else if (item.GetType() == typeof(CloudBlobDirectory))
        {
            CloudBlobDirectory directory = (CloudBlobDirectory)item;

            Console.WriteLine("Directory: {0}", directory.Uri);
        }
    }

Zoals weergegeven in het vorige codevoorbeeld, heeft de blob-service het concept van mappen binnen containers, evenals. Dit is zodat u de blobs in een map vergelijkbare structuur kunt indelen. Bekijk bijvoorbeeld de volgende set blok-blobs in een container met de naam **photos**:

    photo1.jpg
    2010/architecture/description.txt
    2010/architecture/photo3.jpg
    2010/architecture/photo4.jpg
    2011/architecture/photo5.jpg
    2011/architecture/photo6.jpg
    2011/architecture/description.txt
    2011/photo7.jpg

Als u aanroept **ListBlobs** op de container (zoals in het vorige voorbeeld), de verzameling geretourneerd bevat **CloudBlobDirectory** en **CloudBlockBlob** objecten die de mappen en blobs die zijn opgenomen op het hoogste niveau. Dit is de resulterende uitvoer:

    Directory: https://<accountname>.blob.core.windows.net/photos/2010/
    Directory: https://<accountname>.blob.core.windows.net/photos/2011/
    Block blob of length 505623: https://<accountname>.blob.core.windows.net/photos/photo1.jpg


U kunt desgewenst de parameter **UseFlatBlobListing** van de methode **ListBlobs** instellen op **true**. Dit resulteert in elke blob die wordt geretourneerd als een **CloudBlockBlob**, onafhankelijk van de directory. Dit is de aanroep van **ListBlobs**:

    // Loop over items within the container and output the length and URI.
    foreach (IListBlobItem item in container.ListBlobs(null, true))
    {
       ...
    }

en hier vindt u de resultaten:

    Block blob of length 4: https://<accountname>.blob.core.windows.net/photos/2010/architecture/description.txt
    Block blob of length 314618: https://<accountname>.blob.core.windows.net/photos/2010/architecture/photo3.jpg
    Block blob of length 522713: https://<accountname>.blob.core.windows.net/photos/2010/architecture/photo4.jpg
    Block blob of length 4: https://<accountname>.blob.core.windows.net/photos/2011/architecture/description.txt
    Block blob of length 419048: https://<accountname>.blob.core.windows.net/photos/2011/architecture/photo5.jpg
    Block blob of length 506388: https://<accountname>.blob.core.windows.net/photos/2011/architecture/photo6.jpg
    Block blob of length 399751: https://<accountname>.blob.core.windows.net/photos/2011/photo7.jpg
    Block blob of length 505623: https://<accountname>.blob.core.windows.net/photos/photo1.jpg

Zie voor meer informatie, [CloudBlobContainer.ListBlobs](https://msdn.microsoft.com/library/azure/dd135734.aspx).

## <a name="download-blobs"></a>Blobs downloaden
Om blobs te downloaden, moet u eerst een blobverwijzing ophalen en vervolgens de methode **DownloadToStream** aanroepen. In het volgende voorbeeld wordt de methode **DownloadToStream** gebruikt om de blobinhoud over te dragen naar een stroomobject, dat u vervolgens persistent kunt maken in een lokaal bestand.

    // Get a reference to a blob named "photo1.jpg".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("photo1.jpg");

    // Save blob contents to a file.
    using (var fileStream = System.IO.File.OpenWrite(@"path\myfile"))
    {
        blockBlob.DownloadToStream(fileStream);
    }

U kunt ook de methode **DownloadToStream** gebruiken om de inhoud van een blob te downloaden als een tekenreeks.

    // Get a reference to a blob named "myblob.txt"
    CloudBlockBlob blockBlob2 = container.GetBlockBlobReference("myblob.txt");

    string text;
    using (var memoryStream = new MemoryStream())
    {
        blockBlob2.DownloadToStream(memoryStream);
        text = System.Text.Encoding.UTF8.GetString(memoryStream.ToArray());
    }

## <a name="delete-blobs"></a>Blobs verwijderen
Als u wilt verwijderen van een blob, eerst een blobverwijzing ophalen en vervolgens roept de **verwijderen** methode.

    // Get a reference to a blob named "myblob.txt".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob.txt");

    // Delete the blob.
    blockBlob.Delete();


## <a name="list-blobs-in-pages-asynchronously"></a>Blobs asynchroon op pagina's weergeven
Als u een groot aantal blobs in een lijst weergeeft of als u het aantal resultaten dat per lijst wordt geretourneerd, wilt bepalen, kunt u blobs weergeven op pagina's met resultaten. Dit voorbeeld laat zien hoe resultaten op pagina's asynchroon worden geretourneerd, zodat de uitvoering niet wordt geblokkeerd tijdens het wachten op het retourneren van een groot aantal resultaten.

In dit voorbeeld gebruiken we een platte bloblijst, maar u kunt ook een hiërarchische lijst uitvoeren door de parameter **useFlatBlobListing** van de methode **ListBlobsSegmentedAsync** in te stellen op **false**.

Omdat de voorbeeldmethode een asynchrone bewerking aanroept, moet deze worden voorafgegaan door het sleutelwoord **async** en een **Task**-object retourneren. Het sleutelwoord 'await' dat is opgegeven voor de methode **ListBlobsSegmentedAsync**, onderbreekt de uitvoering van de voorbeeldmethode totdat de taak in de lijst is voltooid.

    async public static Task ListBlobsSegmentedInFlatListing(CloudBlobContainer container)
    {
        // List blobs to the console window, with paging.
        Console.WriteLine("List blobs in pages:");

        int i = 0;
        BlobContinuationToken continuationToken = null;
        BlobResultSegment resultSegment = null;

        // Call ListBlobsSegmentedAsync and enumerate the result segment returned, while the continuation token is non-null.
        // When the continuation token is null, the last page has been returned and execution can exit the loop.
        do
        {
            // This overload allows control of the page size. You can return all remaining results by passing null for the maxResults parameter,
            // or by calling a different overload.
            resultSegment = await container.ListBlobsSegmentedAsync("", true, BlobListingDetails.All, 10, continuationToken, null, null);
            if (resultSegment.Results.Count<IListBlobItem>() > 0) { Console.WriteLine("Page {0}:", ++i); }
            foreach (var blobItem in resultSegment.Results)
            {
                Console.WriteLine("\t{0}", blobItem.StorageUri.PrimaryUri);
            }
            Console.WriteLine();

            //Get the continuation token.
            continuationToken = resultSegment.ContinuationToken;
        }
        while (continuationToken != null);
    }

## <a name="next-steps"></a>Volgende stappen
[!INCLUDE [vs-storage-dotnet-blobs-next-steps](../../includes/vs-storage-dotnet-blobs-next-steps.md)]

