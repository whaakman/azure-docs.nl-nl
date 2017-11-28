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
ms.date: 11/14/2017
ms.author: kraigb
ms.openlocfilehash: afd73bd0fd041a53fbe31aa3a5c23b3e27d7a9ec
ms.sourcegitcommit: f847fcbf7f89405c1e2d327702cbd3f2399c4bc2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/28/2017
---
# <a name="get-started-with-azure-blob-storage-and-visual-studio-connected-services-aspnet-core"></a>Aan de slag met Azure Blob storage en Visual Studio verbonden services (ASP.NET Core)

[!INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

Dit artikel wordt beschreven hoe u aan de slag met Azure Blob storage in Visual Studio nadat u hebt gemaakt of een Azure storage-account in een ASP.NET Core-project waarnaar wordt verwezen door het gebruik van de Visual Studio **verbonden Services** functie. De **verbonden Services** bewerking installeert de juiste NuGet-pakketten voor toegang tot Azure-opslag in uw project en voegt u de verbindingsreeks voor de storage-account toe aan uw project configuratiebestanden. (Zie [documentatie Storage](https://azure.microsoft.com/documentation/services/storage/) voor algemene informatie over Azure Storage.)

Azure Blob storage is een service voor het opslaan van grote hoeveelheden ongestructureerde gegevens die toegankelijk zijn vanuit overal ter wereld via HTTP of HTTPS. Één blob kan elke grootte zijn. BLOBs kunnen worden items zoals afbeeldingen, audio en video-bestanden, onbewerkte gegevens en bestanden. Dit artikel wordt beschreven hoe u aan de slag met blob storage nadat u een Azure storage-account hebt gemaakt met behulp van de Visual Studio **verbonden Services** in een project ASP.NET Core.

Net als bestanden bevinden zich in mappen, live storage-blobs in containers. Nadat u een blob hebt gemaakt, maakt u een of meer containers in blob. Bijvoorbeeld in een blob 'Plakboek' genoemd, kunt u containers, genaamd 'afbeeldingen' voor het opslaan van afbeeldingen en andere aangeroepen 'audio' audio-bestanden op te slaan. Nadat u de containers hebt gemaakt, kunt u afzonderlijke bestanden om ze te uploaden. Zie [aan de slag met Azure Blob storage met .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md) voor meer informatie over het bewerken van programmatisch blobs.

Sommige van de Azure Storage-API's zijn asynchroon en de code in dit artikel wordt ervan uitgegaan dat async-methoden worden gebruikt. Zie [asynchrone programmering](https://docs.microsoft.com/dotnet/csharp/async) voor meer informatie.

## <a name="access-blob-containers-in-code"></a>Toegang tot blob-containers in code

Om programmatisch toegang biedt tot de blobs in ASP.NET Core projecten, moet u de volgende code toevoegen als dat niet al aanwezig zijn:

1. Voeg de vereiste `using` instructies:

    ```cs
    using Microsoft.Extensions.Configuration;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Blob;
    using System.Threading.Tasks;
    using LogLevel = Microsoft.Extensions.Logging.LogLevel;
    ```

1. Ophalen van een `CloudStorageAccount` -object met gegevens over uw storage-account. Gebruik de volgende code om uw verbindingsreeks voor opslag en de gegevens van de storage-account van de configuratie van de Azure-service:

    ```cs
     CloudStorageAccount storageAccount = new CloudStorageAccount(
        new Microsoft.WindowsAzure.Storage.Auth.StorageCredentials(
        "<storage-account-name>",
        "<access-key>"), true);
    ```

1. Gebruik een `CloudBlobClient` object ophalen van een `CloudBlobContainer` verwijzing naar een bestaande container in uw opslagaccount:

    ```cs
    // Create a blob client.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Get a reference to a container named "mycontainer."
    CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");
    ```

## <a name="create-a-container-in-code"></a>Een container in code maken

U kunt ook de `CloudBlobClient` een container maken in uw opslagaccount door het aanroepen van `CreateIfNotExistsAsync`:

```cs
// Create a blob client.
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

// Get a reference to a container named "my-new-container."
CloudBlobContainer container = blobClient.GetContainerReference("my-new-container");

// If "mycontainer" doesn't exist, create it.
await container.CreateIfNotExistsAsync();
```

Als u de bestanden in de container beschikbaar wilt maken voor iedereen, stelt u de container openbaar:

```cs
await container.SetPermissionsAsync(new BlobContainerPermissions
{
    PublicAccess = BlobContainerPublicAccessType.Blob
});
```

## <a name="upload-a-blob-into-a-container"></a>Een blob uploaden naar een container

Als u wilt een blob-bestand uploaden naar een container, een containerverwijzing ophalen en deze gebruiken een blobverwijzing ophalen. Elke andere stroom van gegevens naar die verwijzen naar uploaden door het aanroepen van de `UploadFromStreamAsync` methode. Deze bewerking wordt de blob gemaakt als deze nog niet is gebeurd, en een bestaande blob worden overschreven. 

```cs
// Get a reference to a blob named "myblob".
CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob");

// Create or overwrite the "myblob" blob with the contents of a local file
// named "myfile".
using (var fileStream = System.IO.File.OpenRead(@"path\myfile"))
{
    await blockBlob.UploadFromStreamAsync(fileStream);
}
```

## <a name="list-the-blobs-in-a-container"></a>De blobs in een container in een lijst weergeven

Als de blobs in een container wilt weergeven, eerste get een containerverwijzing, roept u vervolgens de `ListBlobsSegmentedAsync` methode voor het ophalen van de blobs en/of de mappen hierin. Voor toegang tot de uitgebreide set eigenschappen en methoden voor een geretourneerde `IListBlobItem`, dit casten naar een `CloudBlockBlob`, `CloudPageBlob`, of `CloudBlobDirectory` object. Als u het blobtype niet weet, kunt u typecontrole gebruiken om te bepalen welke gecast.

```cs
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
```

Zie [aan de slag met Azure Blob storage met .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md#list-the-blobs-in-a-container) voor andere manieren om de inhoud van een blob-container.

## <a name="download-a-blob"></a>Een blob downloaden

Voor het downloaden van een blob, het eerste get een verwijzing naar de blob, roept u vervolgens de `DownloadToStreamAsync` methode. Het volgende voorbeeld wordt de `DownloadToStreamAsync` methode voor het overdragen van de blobinhoud naar een stroomobject dat u kunt vervolgens opslaan als een lokaal bestand.

```cs
// Get a reference to a blob named "photo1.jpg".
CloudBlockBlob blockBlob = container.GetBlockBlobReference("photo1.jpg");

// Save the blob contents to a file named "myfile".
using (var fileStream = System.IO.File.OpenWrite(@"path\myfile"))
{
    await blockBlob.DownloadToStreamAsync(fileStream);
}
```

Zie [aan de slag met Azure Blob storage met .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md#download-blobs) voor andere manieren om het opslaan van blobs als bestanden.

## <a name="delete-a-blob"></a>Een blob verwijderen

Als u wilt verwijderen een blob, het eerste get een verwijzing naar de blob, roept u vervolgens de `DeleteAsync` methode:

```cs
// Get a reference to a blob named "myblob.txt".
CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob.txt");

// Delete the blob.
await blockBlob.DeleteAsync();
```

## <a name="next-steps"></a>Volgende stappen

[!INCLUDE [vs-storage-dotnet-blobs-next-steps](../../includes/vs-storage-dotnet-blobs-next-steps.md)]
