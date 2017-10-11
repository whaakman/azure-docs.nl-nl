---
title: Aan de slag met blob storage en Visual Studio verbonden services (ASP.NET Core) | Microsoft Docs
description: Hoe u aan de slag met Azure Blob-opslag in een project voor Visual Studio ASP.NET Core nadat u een opslagaccount met behulp van Visual Studio verbonden services hebt gemaakt
services: storage
documentationcenter: 
author: kraigb
manager: ghogen
editor: 
ms.assetid: 094b596a-c92c-40c4-a0f5-86407ae79672
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: vs-getting-started
ms.devlang: na
ms.topic: article
ms.date: 12/02/2016
ms.author: kraigb
ms.openlocfilehash: 2e8060b44c395ad7c24e7778c0ef65148a3a45de
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="get-started-with-azure-blob-storage-and-visual-studio-connected-services-aspnet-core"></a>Aan de slag met Azure Blob storage en Visual Studio verbonden services (ASP.NET Core)
[!INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>Overzicht
Dit artikel wordt beschreven hoe u aan de slag met Azure Blob storage in Visual Studio nadat u hebt gemaakt of een Azure storage-account in een ASP.NET Core-project waarnaar wordt verwezen in het dialoogvenster Visual Studio verbonden Services toevoegen.

Azure Blob storage is een service voor het opslaan van grote hoeveelheden ongestructureerde gegevens die toegankelijk zijn vanuit overal ter wereld via HTTP of HTTPS. Één blob kan elke grootte zijn. BLOBs kunnen worden items zoals afbeeldingen, audio en video-bestanden, onbewerkte gegevens en bestanden. Dit artikel wordt beschreven hoe u aan de slag met blob storage nadat u een Azure storage-account hebt gemaakt met behulp van de Visual Studio **verbonden Services toevoegen** dialoogvenster in een project ASP.NET Core.

Net als bestanden bevinden zich in mappen, live storage-blobs in containers. Nadat u een opslagruimte hebt gemaakt, kunt u een of meer containers maken in de opslag. Bijvoorbeeld in een opslag 'Plakboek' genoemd, kunt u containers maken in de opslag, naam 'images' voor het opslaan van afbeeldingen en andere aangeroepen 'audio' audio-bestanden op te slaan. Nadat u de containers hebt gemaakt, kunt u afzonderlijke blob bestanden om ze te uploaden. Zie [aan de slag met Azure Blob storage met .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md) voor meer informatie over het bewerken van programmatisch blobs.

## <a name="access-blob-containers-in-code"></a>Toegang tot blob-containers in code
Om programmatisch toegang biedt tot de blobs in ASP.NET Core projecten, moet u de volgende items toevoegen als ze nog niet aanwezig is.

1. De volgende code naamruimtedeclaraties toevoegen aan het begin van een C#-bestand waarin u wilt programmatisch toegang biedt tot Azure-opslag.
   
        using Microsoft.Extensions.Configuration;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Blob;
        using System.Threading.Tasks;
        using LogLevel = Microsoft.Extensions.Logging.LogLevel;
2. Ophalen van een **CloudStorageAccount** -object met gegevens over uw storage-account. Gebruik de volgende code naar uw verbindingsreeks voor opslag en storage-account-gegevens ophalen uit de configuratie van de Azure-service.
   
         CloudStorageAccount storageAccount = new CloudStorageAccount(
            new Microsoft.WindowsAzure.Storage.Auth.StorageCredentials(
            "<storage-account-name>",
            "<access-key>"), true);
   
    **Opmerking:** alle bovenstaande code voor de code in de volgende secties gebruiken.
3. Gebruik een **CloudBlobClient** object ophalen van een **CloudBlobContainer** verwijzing naar een bestaande container in uw opslagaccount.
   
        // Create a blob client.
        CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
   
        // Get a reference to a container named "mycontainer."
        CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

## <a name="create-a-container-in-code"></a>Een container in code maken
U kunt ook de **CloudBlobClient** een container maken in uw opslagaccount. U hoeft te doen is het toevoegen van een aanroep van **CreateIfNotExistsAsync** zoals in de volgende code:

    // Create a blob client.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Get a reference to a container named "my-new-container."
    CloudBlobContainer container = blobClient.GetContainerReference("my-new-container");

    // If "mycontainer" doesn't exist, create it.
    await container.CreateIfNotExistsAsync();


**Opmerking:** de API's die het uitvoeren van aanroepen naar Azure storage in ASP.NET Core zijn asynchroon. Zie [asynchrone programmering met Async en Await](http://msdn.microsoft.com/library/hh191443.aspx) voor meer informatie. De volgende code wordt ervan uitgegaan programming async-methoden worden gebruikt.

Als u de bestanden in de container beschikbaar wilt maken voor iedereen, kunt u de container als public met behulp van de volgende code instellen.

    await container.SetPermissionsAsync(new BlobContainerPermissions
    {
        PublicAccess = BlobContainerPublicAccessType.Blob
    });

## <a name="upload-a-blob-into-a-container"></a>Een blob uploaden naar een container
Als u wilt een blob-bestand uploaden naar een container, een containerverwijzing ophalen en deze gebruiken een blobverwijzing ophalen. Nadat u een blobverwijzing hebt, kunt u een stream met gegevens uploaden naar het door het aanroepen van de **UploadFromStreamAsync** methode. Deze bewerking wordt de blob gemaakt als deze nog niet is gebeurd, of deze wordt overschreven als deze bestaat. Het volgende voorbeeld laat zien hoe u een blob uploadt naar een container. Hierbij wordt ervan uitgegaan dat de container al is gemaakt.

    // Get a reference to a blob named "myblob".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob");

    // Create or overwrite the "myblob" blob with the contents of a local file
    // named "myfile".
    using (var fileStream = System.IO.File.OpenRead(@"path\myfile"))
    {
        await blockBlob.UploadFromStreamAsync(fileStream);
    }

## <a name="list-the-blobs-in-a-container"></a>De blobs in een container in een lijst weergeven
Als u een lijst van de blobs in een container wilt weergeven, moet u eerst een containerverwijzing ophalen. Vervolgens kunt u de container aanroepen **ListBlobsSegmentedAsync** methode voor het ophalen van de blobs en/of de mappen hierin. Voor toegang tot de uitgebreide set eigenschappen en methoden voor een geretourneerde **IListBlobItem** moet u dit casten naar een object van het type **CloudBlockBlob**, **CloudPageBlob** of **CloudBlobDirectory**. Als u het blobtype niet weet, kunt u typecontrole gebruiken om te bepalen welke dit casten naar. De volgende code laat zien hoe ophalen en de uitvoer van de URI van elk item in een container.

    BlobContinuationToken token = null;
    do
    {
        BlobResultSegment resultSegment = await container.ListBlobsSegmentedAsync(token);
        token = resultSegment.ContinuationToken;

        foreach (IListBlobItem item in resultSegment.Results)
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
    } while (token != null);

Er zijn andere manieren om de inhoud van een blob-container. Zie [aan de slag met Azure Blob storage met .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md#list-the-blobs-in-a-container) voor meer informatie.

## <a name="download-a-blob"></a>Een blob downloaden
Voor het downloaden van een blob, eerst een verwijzing naar de blob ophalen en roept u vervolgens de **DownloadToStreamAsync** methode. Het volgende voorbeeld wordt de **DownloadToStreamAsync** methode voor het overdragen van de blobinhoud naar een stroomobject dat u kunt vervolgens opslaan als een lokaal bestand.

    // Get a reference to a blob named "photo1.jpg".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("photo1.jpg");

    // Save the blob contents to a file named "myfile".
    using (var fileStream = System.IO.File.OpenWrite(@"path\myfile"))
    {
        await blockBlob.DownloadToStreamAsync(fileStream);
    }

Er zijn andere manieren om het opslaan van blobs als bestanden. Zie [aan de slag met Azure Blob storage met .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md#download-blobs) voor meer informatie.

## <a name="delete-a-blob"></a>Een blob verwijderen
Als u wilt verwijderen van een blob, eerst een verwijzing naar de blob ophalen en roept u vervolgens de **DeleteAsync** methode erop.

    // Get a reference to a blob named "myblob.txt".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob.txt");

    // Delete the blob.
    await blockBlob.DeleteAsync();

## <a name="next-steps"></a>Volgende stappen
[!INCLUDE [vs-storage-dotnet-blobs-next-steps](../../includes/vs-storage-dotnet-blobs-next-steps.md)]

