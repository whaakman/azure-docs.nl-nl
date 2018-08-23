---
title: Aan de slag met blob storage en Visual Studio verbonden services (ASP.NET Core) | Microsoft Docs
description: Aan de slag met Azure Blob-opslag in een Visual Studio ASP.NET Core-project nadat u een opslagaccount met behulp van Visual Studio verbonden services hebt gemaakt
services: storage
author: ghogen
manager: douge
ms.assetid: 094b596a-c92c-40c4-a0f5-86407ae79672
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.custom: vs-azure
ms.workload: azure-vs
ms.topic: conceptual
ms.date: 11/14/2017
ms.author: ghogen
ms.openlocfilehash: b2b707585df4a7ec26f689b4213be74bdaab680d
ms.sourcegitcommit: 30c7f9994cf6fcdfb580616ea8d6d251364c0cd1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2018
ms.locfileid: "42054316"
---
# <a name="get-started-with-azure-blob-storage-and-visual-studio-connected-services-aspnet-core"></a>Aan de slag met Azure Blob storage en Visual Studio verbonden services (ASP.NET Core)

[!INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

In dit artikel wordt beschreven hoe u aan de slag met Azure Blob-opslag in Visual Studio nadat u hebt gemaakt of een Azure storage-account in een ASP.NET Core-project waarnaar wordt verwezen door het gebruik van de Visual Studio **Connected Services** functie. De **Connected Services** bewerking installeert de juiste NuGet-pakketten voor toegang tot Azure storage in uw project en de verbindingsreeks voor het opslagaccount toegevoegd aan uw project-configuratiebestanden. (Zie [documentatie voor Storage](https://azure.microsoft.com/documentation/services/storage/) voor algemene informatie over Azure Storage.)

Azure Blob-opslag is een service voor het opslaan van grote hoeveelheden ongestructureerde gegevens die kunnen worden benaderd vanaf elke locatie ter wereld via HTTP of HTTPS. Één blob kan elke grootte zijn. BLOBs kunnen zijn items zoals afbeeldingen, audio en video-bestanden, onbewerkte gegevens en bestanden. In dit artikel wordt beschreven hoe u aan de slag met blob-opslag, nadat u een Azure storage-account hebt gemaakt met behulp van de Visual Studio **Connected Services** in een ASP.NET Core-project.

Net zoals u bestanden in mappen woont, live storage-blobs in containers. Nadat u een blob hebt gemaakt, maakt u een of meer containers in die blob. Bijvoorbeeld in een blob met de naam 'Plakboek', kunt u containers met de naam 'installatiekopieën' voor het opslaan van foto's maken en een andere naam "audio" audio-bestanden wilt opslaan. Nadat u de containers hebt gemaakt, kunt u afzonderlijke bestanden uploaden naar deze. Zie [Quickstart: blobs uploaden, downloaden, en lijst met behulp van .NET](../storage/blobs/storage-quickstart-blobs-dotnet.md) voor meer informatie over het bewerken van blobs via een programma.

Enkele van de Azure Storage-API's zijn asynchroon en de code in dit artikel wordt ervan uitgegaan dat async-methoden worden gebruikt. Zie [asynchrone programmering](https://docs.microsoft.com/dotnet/csharp/async) voor meer informatie.

## <a name="access-blob-containers-in-code"></a>Toegang tot blob-containers in code

Om programmatisch toegang verkrijgen tot de blobs in ASP.NET Core-projecten, moet u de volgende code toevoegen als dit nog niet geïnstalleerd:

1. Toevoegen van de benodigde `using` instructies:

    ```cs
    using Microsoft.Extensions.Configuration;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Blob;
    using System.Threading.Tasks;
    using LogLevel = Microsoft.Extensions.Logging.LogLevel;
    ```

1. Krijgen een `CloudStorageAccount` -object met gegevens van uw opslagaccount. Gebruik de volgende code om op te halen van de verbindingsreeks voor opslag en gegevens over het opslagaccount van de configuratie van de Azure-service:

    ```cs
     CloudStorageAccount storageAccount = new CloudStorageAccount(
        new Microsoft.WindowsAzure.Storage.Auth.StorageCredentials(
        "<storage-account-name>",
        "<access-key>"), true);
    ```

1. Gebruik een `CloudBlobClient` object aan een `CloudBlobContainer` verwijzing naar een bestaande container in uw storage-account:

    ```cs
    // Create a blob client.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Get a reference to a container named "mycontainer."
    CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");
    ```

## <a name="create-a-container-in-code"></a>Een container in code maken

U kunt ook de `CloudBlobClient` naar een container in uw storage-account maken door het aanroepen van `CreateIfNotExistsAsync`:

```cs
// Create a blob client.
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

// Get a reference to a container named "my-new-container."
CloudBlobContainer container = blobClient.GetContainerReference("my-new-container");

// If "mycontainer" doesn't exist, create it.
await container.CreateIfNotExistsAsync();
```

Als u wilt de bestanden in de container beschikbaar maken voor iedereen, stelt u de container openbaar:

```cs
await container.SetPermissionsAsync(new BlobContainerPermissions
{
    PublicAccess = BlobContainerPublicAccessType.Blob
});
```

## <a name="upload-a-blob-into-a-container"></a>Een blob uploaden naar een container

Als u wilt een blob-bestand uploaden naar een container, een containerverwijzing ophalen en deze gebruiken om te een blobverwijzing ophalen. Vervolgens elke gewenste gegevensstroom gegevens uploaden naar die verwijzen naar door het aanroepen van de `UploadFromStreamAsync` methode. Met deze bewerking wordt de blob gemaakt als deze nog niet is ingevuld, of een bestaande blob wordt overschreven. 

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

Als u wilt de blobs in een container te vermelden, eerste get een containerverwijzing, roept u vervolgens de `ListBlobsSegmentedAsync` methode voor het ophalen van de blobs en/of de mappen hierin. Voor toegang tot de uitgebreide set eigenschappen en methoden voor een geretourneerde `IListBlobItem`, dit casten naar een `CloudBlockBlob`, `CloudPageBlob`, of `CloudBlobDirectory` object. Als u niet weet het blobtype wat, kunt u typecontrole gebruiken om te bepalen welke dit casten naar.

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

Zie [Quickstart: blobs uploaden, downloaden, en lijst met behulp van .NET](../storage/blobs/storage-quickstart-blobs-dotnet.md#list-the-blobs-in-a-container) voor andere manieren om de inhoud van een blob-container weer te geven.

## <a name="download-a-blob"></a>Een blob downloaden

Voor het downloaden van een blob, het eerste get een verwijzing naar de blob, roept u vervolgens de `DownloadToStreamAsync` methode. Het volgende voorbeeld wordt de `DownloadToStreamAsync` methode voor het overdragen van de blob-inhoud naar een stroomobject, dat u kunt vervolgens opslaan als een lokaal bestand.

```cs
// Get a reference to a blob named "photo1.jpg".
CloudBlockBlob blockBlob = container.GetBlockBlobReference("photo1.jpg");

// Save the blob contents to a file named "myfile".
using (var fileStream = System.IO.File.OpenWrite(@"path\myfile"))
{
    await blockBlob.DownloadToStreamAsync(fileStream);
}
```

Zie [Quickstart: blobs uploaden, downloaden, en lijst met behulp van .NET](../storage/blobs/storage-quickstart-blobs-dotnet.md#download-blobs) voor andere manieren om het opslaan van blobs als bestanden.

## <a name="delete-a-blob"></a>Een blob verwijderen

Als u wilt verwijderen van een blob, het eerste get een verwijzing naar de blob, roept u vervolgens de `DeleteAsync` methode:

```cs
// Get a reference to a blob named "myblob.txt".
CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob.txt");

// Delete the blob.
await blockBlob.DeleteAsync();
```

## <a name="next-steps"></a>Volgende stappen

[!INCLUDE [vs-storage-dotnet-blobs-next-steps](../../includes/vs-storage-dotnet-blobs-next-steps.md)]
