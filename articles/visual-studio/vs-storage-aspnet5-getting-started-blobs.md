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

In dit artikel wordt beschreven hoe u aan de slag kunt gaan met Azure Blob Storage in Visual Studio nadat u een Azure-opslagaccount hebt gemaakt of ernaar hebt verwezen in een ASP.NET Core-project met de functie Visual Studio **Connected Services**. De **Connected Services** bewerking installeert de juiste NuGet-pakketten voor toegang tot Azure storage in uw project en voegt de connectiestring voor het opslagaccount toe aan uw project-configuratiebestanden. (Zie [documentatie voor Storage](https://azure.microsoft.com/documentation/services/storage/) voor algemene informatie over Azure Storage.)

Azure Blob-opslag is een service voor het opslaan van grote hoeveelheden ongestructureerde gegevens die kunnen worden benaderd vanaf elke locatie ter wereld via HTTP of HTTPS. Één blob kan elke grootte zijn. BLOBs kunnen zijn items zoals afbeeldingen, audio en video-bestanden, onbewerkte gegevens en bestanden. In dit artikel wordt beschreven hoe u aan de slag met blob-opslag, nadat u een Azure storage-account hebt gemaakt met behulp van de Visual Studio **Connected Services** in een ASP.NET Core-project.

Net zoals bestanden in mappen staan, staan opslag-blobs in containers. Nadat u een blob hebt gemaakt, maakt u een of meer containers in die blob. Bijvoorbeeld in een blob met de naam 'Plakboek', kunt u containers met de naam 'afbeeldingen' voor het opslaan van foto's maken en een andere met de naam "audio" waar u audio-bestanden wilt opslaan. Nadat u de containers hebt gemaakt, kunt u afzonderlijke bestanden er naar toe uploaden. Zie [Quickstart: blobs uploaden, downloaden, en lijst met behulp van .NET](../storage/blobs/storage-quickstart-blobs-dotnet.md) voor meer informatie over het bewerken van blobs via een programma.

Enkele van de Azure Storage-API's zijn asynchroon en de code in dit artikel gaat ervan uit dat async-methoden worden gebruikt. Zie [asynchrone programmering](https://docs.microsoft.com/dotnet/csharp/async) voor meer informatie.

## <a name="access-blob-containers-in-code"></a>Toegang tot blob-containers in code

Om programmatisch toegang verkrijgen tot de blobs in ASP.NET Core-projecten, moet u de volgende code toevoegen als dit er nog niet staat:

1. Toevoegen van de benodigde `using` instructies:

    ```cs
    using Microsoft.Extensions.Configuration;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Blob;
    using System.Threading.Tasks;
    using LogLevel = Microsoft.Extensions.Logging.LogLevel;
    ```

1. Maak een `CloudStorageAccount` -object met gegevens van uw opslagaccount. Gebruik de volgende code om de connectiestring van de opslag en gegevens over het opslagaccount op te halen uit de configuratie van de Azure-service:

    ```cs
     CloudStorageAccount storageAccount = new CloudStorageAccount(
        new Microsoft.WindowsAzure.Storage.Auth.StorageCredentials(
        "<storage-account-name>",
        "<access-key>"), true);
    ```

1. Gebruik een `CloudBlobClient` object om een `CloudBlobContainer` verwijzing te krijgen naar een bestaande container in uw storage-account:

    ```cs
    // Create a blob client.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Get a reference to a container named "mycontainer."
    CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");
    ```

## <a name="create-a-container-in-code"></a>Een container in code maken

U kunt ook de `CloudBlobClient` gebruiken om een container in uw storage-account te maken door het aanroepen van `CreateIfNotExistsAsync`:

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

Als u een blob-bestand wilt uploaden in een container, haal dan een containerverwijzing op en gebruik deze om te een blobverwijzing te krijgen. Upload vervolgens elke gegevensstroom naar die referentie door de methode `UploadFromStreamAsync` aan te roepen. Met deze bewerking wordt de blob gemaakt als deze nog niet is ingevuld, of een bestaande blob wordt overschreven. 

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

Als u de blobs in een container in een lijst wilt plaatsen, haal dan eerst een containerverwijzing op, vervolgens roept u de methode `ListBlobsSegmentedAsync` aan voor het ophalen van de blobs en/of de mappen hierin. Voor toegang tot de uitgebreide set eigenschappen en methoden van een geretourneerd `IListBlobItem`, zet deze om naar een `CloudBlockBlob`, `CloudPageBlob`, of `CloudBlobDirectory` object. Als u het blobtype niet weet, kunt u een typecontrole gebruiken om te bepalen waar u deze naar kunt omzetten.

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

Als u een blob wilt downloaden, moet u eerst een verwijzing naar de blob krijgen en vervolgens de methode `DownloadToStreamAsync` gebruiken. In het volgende voorbeeld wordt de methode `DownloadToStreamAsync` gebruikt om de blob-inhoud over te zetten naar een stroomobject dat u vervolgens kunt opslaan als een lokaal bestand.

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

Als u een blob wilt verwijderen, moet u eerst een verwijzing naar de blob krijgen en vervolgens de methode `DeleteAsync` aanroepen:

```cs
// Get a reference to a blob named "myblob.txt".
CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob.txt");

// Delete the blob.
await blockBlob.DeleteAsync();
```

## <a name="next-steps"></a>Volgende stappen

[!INCLUDE [vs-storage-dotnet-blobs-next-steps](../../includes/vs-storage-dotnet-blobs-next-steps.md)]
