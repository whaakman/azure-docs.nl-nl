---
title: Uploaden van grote hoeveelheden willekeurige gegevens parallel naar Azure Storage | Microsoft Docs
description: Informatie over het gebruik van de Azure SDK voor het uploaden van grote hoeveelheden willekeurige gegevens parallel met een Azure Storage-account
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
ms.openlocfilehash: 98f3f69c6025d61caac20e13b573651854952432
ms.sourcegitcommit: 4256ebfe683b08fedd1a63937328931a5d35b157
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/23/2017
---
# <a name="upload-large-amounts-of-random-data-in-parallel-to-azure-storage"></a>Uploaden van grote hoeveelheden willekeurige gegevens parallel naar Azure-opslag

Deze zelfstudie maakt deel uit twee van een serie. Deze zelfstudie ziet dat u een toepassing implementeren die grote hoeveelheden willekeurige gegevens naar een Azure storage-account uploadt.

Deel 2 van de reeks, leert u hoe:

> [!div class="checklist"]
> * De verbindingsreeks configureren
> * De toepassing bouwen
> * De toepassing uitvoeren
> * Valideren van het aantal verbindingen

Azure blob-opslag biedt een schaalbare service voor het opslaan van gegevens. Om te controleren of uw toepassing als zodat mogelijk een goed begrip van hoe de blob-opslag werkt wordt aanbevolen. Kennis van de limieten voor Azure blobs is belangrijk, voor meer informatie over deze limieten gaat u naar: [blob-opslag-schaalbaarheidsdoelen](../common/storage-scalability-targets.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json#azure-blob-storage-scale-targets).

[Partitie naming](../common/storage-performance-checklist.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json#subheading47) is een andere belangrijke factor bij het ontwerpen van een maximaal presterende toepassing met behulp van blobs. Azure-opslag maakt gebruik van een bereik gebaseerde partitieschema op schaal en load balancing. Deze configuratie betekent dat bestanden met vergelijkbare naamconventies of voorvoegsels gaat u naar dezelfde partitie. Deze logica bevat de naam van de container waarin de bestanden zijn om te worden geüpload. In deze zelfstudie gebruikt u de bestanden die geen GUID's voor namen als goed als willekeurig gegenereerde inhoud. Ze worden vervolgens naar de vijf verschillende containers met willekeurige namen geüpload.

## <a name="prerequisites"></a>Vereisten

Voor het voltooien van deze zelfstudie, u moet de zelfstudie hebt voltooid vorige opslag: [maken van een virtuele machine en storage-account voor een schaalbare toepassing][previous-tutorial].

## <a name="remote-into-your-virtual-machine"></a>Extern verbinding met uw virtuele machine

Gebruik de volgende opdracht op uw lokale machine extern bureaublad-sessiehost maken met de virtuele machine. Het IP-adres vervangen door de publicIPAddress van uw virtuele machine. Wanneer u wordt gevraagd, typt u de referenties die u hebt gebruikt bij het maken van de virtuele machine.

```
mstsc /v:<publicIpAddress>
```

## <a name="configure-the-connection-string"></a>De verbindingsreeks configureren

Ga naar uw opslagaccount in de Azure-portal. Selecteer **toegangssleutels** onder **instellingen** in uw opslagaccount. Kopieer de **verbindingsreeks** uit de primaire of secundaire sleutel. Aanmelden bij de virtuele machine die u in de vorige zelfstudie hebt gemaakt. Open een **opdrachtprompt** als beheerder en voer de `setx` opdracht met de `/m` switch, met deze opdracht slaat de omgevingsvariabele van een machine-instelling. De omgevingsvariabele is niet beschikbaar totdat u opnieuw laden de **opdrachtprompt**. Vervang  **\<storageConnectionString\>**  in het volgende voorbeeld:

```
setx storageconnectionstring "<storageConnectionString>" /m
```

Wanneer u klaar bent, opent u een andere **opdrachtprompt**, gaat u naar `D:\git\storage-dotnet-perf-scale-app` en het type `dotnet build` de toepassing te bouwen.

## <a name="run-the-application"></a>De toepassing uitvoeren

Navigeer naar `D:\git\storage-dotnet-perf-scale-app`.

Type `dotnet run` de toepassing uit te voeren. De eerste keer dat u uitvoert `dotnet` wordt gevuld met uw cache lokale pakket sneller herstellen en offline toegang inschakelen. Deze opdracht om uit te voeren op een minuut in beslag en alleen eenmaal wordt uitgevoerd.

```
dotnet run
```

De toepassing wordt gemaakt van vijf willekeurige naam containers en begint met het uploaden van de bestanden in de staging-map op het opslagaccount. De toepassing wordt de minimale threads ingesteld op 100 en de [DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit(v=vs.110).aspx) tot 100 om ervoor te zorgen dat een groot aantal gelijktijdige verbindingen zijn toegestaan wanneer de toepassing wordt uitgevoerd.

Naast het instellen van de limietinstellingen threading en verbinding met de [BlobRequestOptions](/dotnet/api/microsoft.windowsazure.storage.blob.blobrequestoptions?view=azure-dotnet) voor de [UploadFromStreamAsync](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblockblob.uploadfromstreamasync?view=azure-dotnet) methode zijn geconfigureerd voor gebruik van parallelle uitvoering en validatie van de MD5-hash uitschakelen. De bestanden zijn geüpload in blokken van 100 mb, deze configuratie biedt betere prestaties maar kostbaar als met een slecht uitvoeren netwerk alsof er een fout het hele 100 mb blok is wordt opnieuw geprobeerd.

|Eigenschap|Waarde|Beschrijving|
|---|---|---|
|[ParallelOperationThreadCount](/dotnet/api/microsoft.windowsazure.storage.blob.blobrequestoptions.paralleloperationthreadcount?view=azure-dotnet)| 8| De instelling breekt de blob in blokken, wanneer u uploadt. Deze waarde moet 8 keer het aantal kernen voor de beste prestaties. |
|[DisableContentMD5Validation](/dotnet/api/microsoft.windowsazure.storage.blob.blobrequestoptions.disablecontentmd5validation?view=azure-dotnet)| waar| Deze eigenschap de controle van de MD5-hash van de inhoud die geüpload wordt uitgeschakeld. Het uitschakelen van de MD5-validatie produceert een snellere overdracht. Maar niet bevestigt de geldigheid of de integriteit van de bestanden worden overgebracht.   |
|[StorBlobContentMD5](/dotnet/api/microsoft.windowsazure.storage.blob.blobrequestoptions.storeblobcontentmd5?view=azure-dotnet#Microsoft_WindowsAzure_Storage_Blob_BlobRequestOptions_StoreBlobContentMD5)| onwaar| Deze eigenschap bepaalt als een MD5-hash wordt berekend en met het bestand opgeslagen.   |
| [Het RetryPolicy](/dotnet/api/microsoft.windowsazure.storage.blob.blobrequestoptions.retrypolicy?view=azure-dotnet#Microsoft_WindowsAzure_Storage_Blob_BlobRequestOptions_RetryPolicy)| 2 seconden backoff bij 10 max nieuwe poging |Hiermee bepaalt u het beleid voor opnieuw proberen van aanvragen. Verbindingsfouten in dit voorbeeld worden geprobeerd een [ExponentialRetry](/dotnet/api/microsoft.windowsazure.storage.retrypolicies.exponentialretry?view=azure-dotnet) beleid wordt geconfigureerd met een backoff 2 seconden en een maximale aantal nieuwe pogingen van 10. Deze instelling is belangrijk bij het ophalen van uw toepassing dicht bij roept de [blob-opslag-schaalbaarheidsdoelen](../common/storage-scalability-targets.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json#azure-blob-storage-scale-targets).  |

De `UploadFilesAsync` taak in het volgende voorbeeld wordt weergegeven:

```csharp
private static async Task UploadFilesAsync()
{
    // Create random 5 characters containers to upload files to.
    CloudBlobContainer[] containers = await GetRandomContainersAsync();
    var currentdir = System.IO.Directory.GetCurrentDirectory();

    // path to the directory to upload
    string uploadPath = currentdir + "\\upload";
    Stopwatch time = Stopwatch.StartNew();
    try
    {
        Console.WriteLine("Iterating in directory: {0}", uploadPath);
        int count = 0;
        int max_outstanding = 100;
        int completed_count = 0;

        // Define the BlobRequestionOptions on the upload.
        // This includes defining an exponential retry policy to ensure that failed connections are retried with a backoff policy. As multiple large files are being uploaded
        // large block sizes this can cause an issue if an exponential retry policy is not defined.  Additionally parallel operations are enabled with a thread count of 8
        // This could be should be multiple of the number of cores that the machine has. Lastly MD5 hash validation is disabled for this example, this improves the upload speed.
        BlobRequestOptions options = new BlobRequestOptions
        {
            ParallelOperationThreadCount = 8,
            DisableContentMD5Validation = true,
            StoreBlobContentMD5 = false
        };
        // Create a new instance of the SemaphoreSlim class to define the number of threads to use in the application.
        SemaphoreSlim sem = new SemaphoreSlim(max_outstanding, max_outstanding);

        List<Task> tasks = new List<Task>();
        Console.WriteLine("Found {0} file(s)", Directory.GetFiles(uploadPath).Count());

        // Iterate through the files
        foreach (string path in Directory.GetFiles(uploadPath))
        {
            // Create random file names and set the block size that is used for the upload.
            var container = containers[count % 5];
            string fileName = Path.GetFileName(path);
            Console.WriteLine("Uploading {0} to container {1}.", path, container.Name);
            CloudBlockBlob blockBlob = container.GetBlockBlobReference(fileName);

            // Set block size to 100MB.
            blockBlob.StreamWriteSizeInBytes = 100 * 1024 * 1024;
            await sem.WaitAsync();

            // Create tasks for each file that is uploaded. This is added to a collection that executes them all asyncronously.  
            tasks.Add(blockBlob.UploadFromFileAsync(path, null, options, null).ContinueWith((t) =>
            {
                sem.Release();
                Interlocked.Increment(ref completed_count);
            }));
            count++;
        }

        // Creates an asynchronous task that completes when all the uploads complete.
        await Task.WhenAll(tasks);

        time.Stop();

        Console.WriteLine("Upload has been completed in {0} seconds. Press any key to continue", time.Elapsed.TotalSeconds.ToString());

        Console.ReadLine();
    }
    catch (DirectoryNotFoundException ex)
    {
        Console.WriteLine("Error parsing files in the directory: {0}", ex.Message);
    }
    catch (Exception ex)
    {
        Console.WriteLine(ex.Message);
    }
}
```

Het volgende voorbeeld wordt de uitvoer van een afgekapte toepassing uitgevoerd op een Windows-systeem.

```
Created container https://mystorageaccount.blob.core.windows.net/9efa7ecb-2b24-49ff-8e5b-1d25e5481076
Created container https://mystorageaccount.blob.core.windows.net/bbe5f0c8-be9e-4fc3-bcbd-2092433dbf6b
Created container https://mystorageaccount.blob.core.windows.net/9ac2f71c-6b44-40e7-b7be-8519d3ba4e8f
Created container https://mystorageaccount.blob.core.windows.net/47646f1a-c498-40cd-9dae-840f46072180
Created container https://mystorageaccount.blob.core.windows.net/38b2cdab-45fa-4cf9-94e7-d533837365aa
Iterating in directory: D:\git\storage-dotnet-perf-scale-app\upload
Found 50 file(s)
Starting upload of D:\git\storage-dotnet-perf-scale-app\upload\1d596d16-f6de-4c4c-8058-50ebd8141e4d.txt to container 9efa7ecb-2b24-49ff-8e5b-1d25e5481076.
Starting upload of D:\git\storage-dotnet-perf-scale-app\upload\242ff392-78be-41fb-b9d4-aee8152a6279.txt to container bbe5f0c8-be9e-4fc3-bcbd-2092433dbf6b.
Starting upload of D:\git\storage-dotnet-perf-scale-app\upload\38d4d7e2-acb4-4efc-ba39-f9611d0d55ef.txt to container 9ac2f71c-6b44-40e7-b7be-8519d3ba4e8f.
Starting upload of D:\git\storage-dotnet-perf-scale-app\upload\45930d63-b0d0-425f-a766-cda27ff00d32.txt to container 47646f1a-c498-40cd-9dae-840f46072180.
Starting upload of D:\git\storage-dotnet-perf-scale-app\upload\5129b385-5781-43be-8bac-e2fbb7d2bd82.txt to container 38b2cdab-45fa-4cf9-94e7-d533837365aa.
...
Upload has been completed in 142.0429536 seconds. Press any key to continue
```

### <a name="validate-the-connections"></a>Valideren van de verbindingen

Terwijl de bestanden zijn geüpload, kunt u controleren of het aantal gelijktijdige verbindingen naar uw opslagaccount. Open een **opdrachtprompt** en het type `netstat -a | find /c "blob:https"`. Deze opdracht geeft u het aantal verbindingen die momenteel zijn geopend met behulp van `netstat`. Het volgende voorbeeld ziet een vergelijkbare uitvoer naar wat u ziet bij het uitvoeren van de zelfstudie. Als u in het voorbeeld zien kunt, wordt met 800 verbindingen geopend waren toen de willekeurige bestanden uploaden naar het opslagaccount. Deze waarde wijzigt tijdens het uploaden wordt uitgevoerd. Door te uploaden in segmenten parallel blok, wordt de hoeveelheid tijd die nodig is om over te dragen van de inhoud aanzienlijk verminderd.

```
C:\>netstat -a | find /c "blob:https"
800

C:\>
```

## <a name="next-steps"></a>Volgende stappen

In deel twee van de reeks, hebt u geleerd over het uploaden van grote hoeveelheden willekeurige gegevens naar een opslagaccount in combinatie, zoals het:

> [!div class="checklist"]
> * De verbindingsreeks configureren
> * De toepassing bouwen
> * De toepassing uitvoeren
> * Valideren van het aantal verbindingen

Ga naar deel 2 van de reeks te downloaden van grote hoeveelheden gegevens van een opslagaccount.

> [!div class="nextstepaction"]
> [Uploaden van grote hoeveelheden grote bestanden naar een opslagaccount parallel](storage-blob-scalable-app-download-files.md)

[previous-tutorial]: storage-blob-scalable-app-create-vm.md
