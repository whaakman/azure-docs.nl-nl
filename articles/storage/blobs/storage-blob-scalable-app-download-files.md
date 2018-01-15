---
title: Downloaden van grote hoeveelheden willekeurige gegevens uit Azure Storage | Microsoft Docs
description: Informatie over het gebruik van de Azure SDK downloaden van grote hoeveelheden willekeurige gegevens van een Azure Storage-account
services: storage
documentationcenter: 
author: georgewallace
manager: jeconnoc
editor: 
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: csharp
ms.topic: tutorial
ms.date: 12/12/2017
ms.author: gwallace
ms.custom: mvc
ms.openlocfilehash: 3842860acb1c0fdd9e07f6d2f678ac5d5304003b
ms.sourcegitcommit: 48fce90a4ec357d2fb89183141610789003993d2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/12/2018
---
# <a name="download-large-amounts-of-random-data-from-azure-storage"></a>Downloaden van grote hoeveelheden willekeurige gegevens uit Azure storage

Deze zelfstudie maakt deel uit drie van een serie. Deze zelfstudie laat zien hoe u voor het downloaden van grote hoeveelheden gegevens uit Azure storage.

Deel 3 van de reeks, leert u hoe:

> [!div class="checklist"]
> * De toepassing bijwerken
> * De toepassing uitvoeren
> * Valideren van het aantal verbindingen

## <a name="prerequisites"></a>Vereisten

Voor het voltooien van deze zelfstudie, u moet de zelfstudie hebt voltooid vorige opslag: [uploaden van grote hoeveelheden willekeurige gegevens parallel naar Azure storage][previous-tutorial].

## <a name="remote-into-your-virtual-machine"></a>Extern verbinding met uw virtuele machine

 Als een extern bureaublad-sessiehost maken met de virtuele machine, gebruik de volgende opdracht op uw lokale machine. Het IP-adres vervangen door de publicIPAddress van uw virtuele machine. Wanneer u hierom wordt gevraagd, typt u de referenties die zijn gebruikt bij het maken van de virtuele machine.

```
mstsc /v:<publicIpAddress>
```

## <a name="update-the-application"></a>De toepassing bijwerken

In de vorige zelfstudie geüpload u alleen bestanden met de opslagaccount. Open `D:\git\storage-dotnet-perf-scale-app\Program.cs` in een teksteditor. Vervang de `Main` methode met het volgende voorbeeld. Deze opmerkingen voorbeeld buiten de taak uploaden en uncomments de taken downloaden en het verwijderen van de inhoud van het storage-account als u klaar.

```csharp
public static void Main(string[] args)
{
    Console.WriteLine("Azure Blob storage performance and scalability sample");
    // Set threading and default connection limit to 100 to ensure multiple threads and connections can be opened.
    // This is in addition to parallelism with the storage client library that is defined in the functions below.
    ThreadPool.SetMinThreads(100, 4);
    ServicePointManager.DefaultConnectionLimit = 100; // (Or More)

    bool exception = false;
    try
    {
        // Call the UploadFilesAsync function.
        UploadFilesAsync().GetAwaiter().GetResult();

        // Uncomment the following line to enable downloading of files from the storage account.  This is commented out
        // initially to support the tutorial at https://docs.microsoft.com/azure/storage/blobs/storage-blob-scaleable-app-download-files.
        // DownloadFilesAsync().GetAwaiter().GetResult();
    }
    catch (Exception ex)
    {
        Console.WriteLine(ex.Message);
        exception = true;
    }
    finally
    {
        // The following function will delete the container and all files contained in them.  This is commented out initialy
        // As the tutorial at https://docs.microsoft.com/azure/storage/blobs/storage-blob-scaleable-app-download-files has you upload only for one tutorial and download for the other. 
        if (!exception)
        {
            // DeleteExistingContainersAsync().GetAwaiter().GetResult();
        }
        Console.WriteLine("Press any key to exit the application");
        Console.ReadKey();
    }
}
```

Nadat de toepassing is bijgewerkt, moet u de toepassing opnieuw te bouwen. Open een `Command Prompt` en navigeer naar `D:\git\storage-dotnet-perf-scale-app`. De toepassing opnieuw door het uitvoeren van `dotnet build` zoals te zien is in het volgende voorbeeld:

```
dotnet build
```

## <a name="run-the-application"></a>De toepassing uitvoeren

Nu dat de toepassing is opnieuw opgebouwd is het tijd om de toepassing met de bijgewerkte code uitvoeren. Als deze niet al is geopend, opent u een `Command Prompt` en navigeer naar `D:\git\storage-dotnet-perf-scale-app`.

Type `dotnet run` de toepassing uit te voeren.

```
dotnet run
```

De toepassing leest de containers die zich in de storage-account opgegeven in de **storageconnectionstring**. Doorlopen van de blobs 10 op een tijdstip met de [ListBlobsSegmented](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblobcontainer.listblobssegmented?view=azure-dotnet#Microsoft_WindowsAzure_Storage_Blob_CloudBlobContainer_ListBlobsSegmented_System_String_System_Boolean_Microsoft_WindowsAzure_Storage_Blob_BlobListingDetails_System_Nullable_System_Int32__Microsoft_WindowsAzure_Storage_Blob_BlobContinuationToken_Microsoft_WindowsAzure_Storage_Blob_BlobRequestOptions_Microsoft_WindowsAzure_Storage_OperationContext_) methode in de containers en downloads ze aan de lokale machine met behulp van de [DownloadToFileAsync](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblob.downloadtofileasync?view=azure-dotnet#Microsoft_WindowsAzure_Storage_Blob_CloudBlob_DownloadToFileAsync_System_String_System_IO_FileMode_Microsoft_WindowsAzure_Storage_AccessCondition_Microsoft_WindowsAzure_Storage_Blob_BlobRequestOptions_Microsoft_WindowsAzure_Storage_OperationContext_) methode.
De volgende tabel toont de [BlobRequestOptions](/dotnet/api/microsoft.windowsazure.storage.blob.blobrequestoptions?view=azure-dotnet) die voor elke blob zijn gedefinieerd zoals deze is gedownload.

|Eigenschap|Waarde|Beschrijving|
|---|---|---|
|[DisableContentMD5Validation](/dotnet/api/microsoft.windowsazure.storage.blob.blobrequestoptions.disablecontentmd5validation?view=azure-dotnet)| waar| Deze eigenschap de controle van de MD5-hash van de inhoud die geüpload wordt uitgeschakeld. Het uitschakelen van de MD5-validatie produceert een snellere overdracht. Maar niet bevestigt de geldigheid of de integriteit van de bestanden worden overgebracht. |
|[StorBlobContentMD5](/dotnet/api/microsoft.windowsazure.storage.blob.blobrequestoptions.storeblobcontentmd5?view=azure-dotnet#Microsoft_WindowsAzure_Storage_Blob_BlobRequestOptions_StoreBlobContentMD5)| onwaar| Deze eigenschap bepaalt als een MD5-hash wordt berekend en opgeslagen.   |

De `DownloadFilesAsync` taak in het volgende voorbeeld wordt weergegeven:

```csharp
private static async Task DownloadFilesAsync()
{
    CloudBlobClient blobClient = GetCloudBlobClient();

    // Define the BlobRequestionOptions on the download, including disabling MD5 hash validation for this example, this improves the download speed.
    BlobRequestOptions options = new BlobRequestOptions
    {
        DisableContentMD5Validation = true,
        StoreBlobContentMD5 = false
    };

    // Retrieve the list of containers in the storage account.  Create a directory and configure variables for use later.
    BlobContinuationToken continuationToken = null;
    List<CloudBlobContainer> containers = new List<CloudBlobContainer>();
    do
    {
        var listingResult = await blobClient.ListContainersSegmentedAsync(continuationToken);
        continuationToken = listingResult.ContinuationToken;
        containers.AddRange(listingResult.Results);
    }
    while (continuationToken != null);

    var directory = Directory.CreateDirectory("download");
    BlobResultSegment resultSegment = null;
    Stopwatch time = Stopwatch.StartNew();

    // Download the blobs
    try
    {
        List<Task> tasks = new List<Task>();
        int max_outstanding = 100;
        int completed_count = 0;

        // Create a new instance of the SemaphoreSlim class to define the number of threads to use in the application.
        SemaphoreSlim sem = new SemaphoreSlim(max_outstanding, max_outstanding);

        // Iterate through the containers
        foreach (CloudBlobContainer container in containers)
        {
            do
            {
                // Return the blobs from the container lazily 10 at a time.
                resultSegment = await container.ListBlobsSegmentedAsync(null, true, BlobListingDetails.All, 10, continuationToken, null, null);
                continuationToken = resultSegment.ContinuationToken;
                {
                    foreach (var blobItem in resultSegment.Results)
                    {

                        if (((CloudBlob)blobItem).Properties.BlobType == BlobType.BlockBlob)
                        {
                            // Get the blob and add a task to download the blob asynchronously from the storage account.
                            CloudBlockBlob blockBlob = container.GetBlockBlobReference(((CloudBlockBlob)blobItem).Name);
                            Console.WriteLine("Downloading {0} from container {1}", blockBlob.Name, container.Name);
                            await sem.WaitAsync();
                            tasks.Add(blockBlob.DownloadToFileAsync(directory.FullName + "\\" + blockBlob.Name, FileMode.Create, null, options, null).ContinueWith((t) =>
                            {
                                sem.Release();
                                Interlocked.Increment(ref completed_count);
                            }));

                        }
                    }
                }
            }
            while (continuationToken != null);
        }

        // Creates an asynchronous task that completes when all the downloads complete.
        await Task.WhenAll(tasks);
    }
    catch (Exception e)
    {
        Console.WriteLine("\nError encountered during transfer: {0}", e.Message);
    }

    time.Stop();
    Console.WriteLine("Download has been completed in {0} seconds. Press any key to continue", time.Elapsed.TotalSeconds.ToString());
    Console.ReadLine();
}
```

### <a name="validate-the-connections"></a>Valideren van de verbindingen

Terwijl de bestanden worden gedownload, kunt u controleren of het aantal gelijktijdige verbindingen naar uw opslagaccount. Open een `Command Prompt` en het type `netstat -a | find /c "blob:https"`. Deze opdracht geeft u het aantal verbindingen die momenteel zijn geopend met behulp van `netstat`. Het volgende voorbeeld ziet een vergelijkbare uitvoer naar wat u ziet bij het uitvoeren van de zelfstudie. Als u in het voorbeeld zien kunt, wordt met meer dan 280 verbindingen geopend waren toen de willekeurige bestanden downloaden van het opslagaccount.

```
C:\>netstat -a | find /c "blob:https"
289

C:\>
```

## <a name="next-steps"></a>Volgende stappen

In deel 2 van de reeks, hebt u geleerd over het downloaden van grote hoeveelheden willekeurige gegevens van een opslagaccount, zoals het:

> [!div class="checklist"]
> * De toepassing uitvoeren
> * Valideren van het aantal verbindingen

Ga naar de vierde deel van de reeks om te controleren of de doorvoer en latentie metrische gegevens in de portal.

> [!div class="nextstepaction"]
> [Controleer of de doorvoer en latentie metrische gegevens in de portal](storage-blob-scalable-app-verify-metrics.md)

[previous-tutorial]: storage-blob-scalable-app-upload-files.md