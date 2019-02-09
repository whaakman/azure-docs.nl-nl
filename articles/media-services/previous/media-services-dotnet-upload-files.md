---
title: Bestanden uploaden naar een Media Services-account met behulp van .NET | Microsoft Docs
description: Informatie over het verkrijgen van media-inhoud in Media Services door te maken en uploaden van activa.
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.assetid: c9c86380-9395-4db8-acea-507c52066f73
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/08/2019
ms.author: juliako
ms.openlocfilehash: a07c4f285b2bb2d74b580648a3aad6a550766828
ms.sourcegitcommit: 943af92555ba640288464c11d84e01da948db5c0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/09/2019
ms.locfileid: "55984219"
---
# <a name="upload-files-into-a-media-services-account-using-net-legacy"></a>Bestanden uploaden naar een Media Services-account met behulp van .NET (verouderd)
> [!div class="op_single_selector"]
> * [.NET](media-services-dotnet-upload-files.md)
> * [REST](media-services-rest-upload-files.md)
> * [Portal](media-services-portal-upload-files.md)
> 
> 

In Media Services moet u uw digitale bestanden uploaden naar (of opnemen in) een asset. De **Asset** entiteit kan bevatten, video, audio, afbeeldingen, verzamelingen van miniaturen, tekst tekstsporen en ondertitelingsbestanden bestanden (en de metagegevens over deze bestanden.)  Zodra de bestanden zijn geüpload, wordt uw inhoud veilig opgeslagen in de cloud voor verdere verwerking en streaming.

De bestanden in de asset worden **assetbestanden** genoemd. De **AssetFile** instantie en de werkelijke mediabestand zijn twee verschillende objecten. Het exemplaar AssetFile bevat metagegevens over de mediabestand, terwijl het bestand de werkelijke media-inhoud bevat.

> [!NOTE]
> De volgende overwegingen zijn van toepassing:
> 
> * Media Services wordt de waarde van de eigenschap IAssetFile.Name bij het bouwen van URL's voor de streaming-inhoud (bijvoorbeeld http://{AMSAccount}.origin.mediaservices.windows.net/{GUID}/{IAssetFile.Name}/streamingParameters.) Om deze reden is procent codering niet toegestaan. De waarde van de **naam** eigenschap kan niet een van de volgende hebben [procent-encoding-gereserveerde tekens](http://en.wikipedia.org/wiki/Percent-encoding#Percent-encoding_reserved_characters):! *' ();: @& = + $, /? % # [] '. Bovendien kunnen alleen er een '.' voor de bestandsnaamextensie.
> * De lengte van de naam mag niet groter zijn dan 260 tekens lang zijn.
> * Er is een limiet voor de maximale bestandsgrootte die wordt ondersteund voor verwerking in Media Services. Raadpleeg [dit](media-services-quotas-and-limitations.md) artikel voor meer informatie over de maximale bestandsgrootte.
> * Er geldt een limiet van 1.000.000 beleidsregels voor verschillende AMS-beleidsitems (bijvoorbeeld voor Locator-beleid of ContentKeyAuthorizationPolicy). U moet dezelfde beleids-id gebruiken als u altijd dezelfde dagen/toegangsmachtigingen gebruikt, bijvoorbeeld beleidsregels voor locators die zijn bedoeld om gedurende een lange periode gehandhaafd te blijven (niet-upload-beleidsregels). Raadpleeg [dit artikel](media-services-dotnet-manage-entities.md#limit-access-policies) voor meer informatie.
> 

Wanneer u activa maakt, kunt u de volgende versleutelingsopties:

* **Geen**: er wordt geen versleuteling gebruikt. Dit is de standaardwaarde. Wanneer u deze optie wordt uw inhoud is niet beveiligd tijdens de overdracht of in rust in de opslag.
  Als u van plan bent een MP4-bestand leveren via progressief downloaden, gebruikt u deze optie: 
* **CommonEncryption** -Gebruik deze optie als u inhoud uploadt die al is versleuteld en beveiligd met Common Encryption of PlayReady DRM (bijvoorbeeld Smooth Streaming beveiligd met PlayReady DRM).
* **EnvelopeEncrypted** – Gebruik deze optie als u met AES versleutelde HLS uploadt. Houd er rekening mee dat de bestanden moeten zijn gecodeerd en versleuteld door Transform Manager.
* **StorageEncrypted** - versleutelt uw niet-versleutelde inhoud lokaal via AES-256-bits codering en vervolgens geüpload naar Azure Storage waar deze zijn opgeslagen in rust versleuteld. De versleuteling van assets die zijn beveiligd met Storage Encryption, wordt automatisch ongedaan gemaakt en de assets worden automatisch in een versleuteld bestandssysteem geplaatst voordat ze worden gecodeerd. Eventueel kunnen ze opnieuw worden versleuteld voordat ze opnieuw worden geüpload als een nieuwe uitvoerasset. Storage Encryption wordt voornamelijk gebruikt om uw invoerbestanden met media van hoge kwaliteit die zijn opgeslagen op de schijf, te beveiligen met een sterke versleuteling.
  
    Media Services biedt versleuteling op disk-opslag voor uw middelen, niet over-the-wire, zoals beheer van digitale rechten (DRM).
  
    Als uw asset versleuteld opslag is, moet u het leveringsbeleid voor Assets configureren. Zie voor meer informatie, [leveringsbeleid voor Assets configureren](media-services-dotnet-configure-asset-delivery-policy.md).

Als u voor uw asset opgeeft moeten worden versleuteld met een **CommonEncrypted** optie, of een **EnvelopeEncrypted** optie, moet u uw asset met koppelen een **ContentKey**. Zie voor meer informatie, [over het maken van een ContentKey](media-services-dotnet-create-contentkey.md). 

Als u voor uw asset opgeeft moeten worden versleuteld met een **StorageEncrypted** optie, de Media Services SDK voor .NET-maakt een **StorageEncrypted** **ContentKey** voor uw asset.

Dit artikel leest hoe u met Media Services .NET SDK, evenals de Media Services .NET SDK extensions bestanden uploaden naar een Media Services-asset.

## <a name="upload-a-single-file-with-media-services-net-sdk"></a>Uploaden van één bestand met Media Services .NET SDK
De volgende code gebruikt .NET voor het uploaden van een enkel bestand. De AccessPolicy en Locator worden gemaakt en vernietigd door de functie uploaden. 

```csharp
        static public IAsset CreateAssetAndUploadSingleFile(AssetCreationOptions assetCreationOptions, string singleFilePath)
        {
            if (!File.Exists(singleFilePath))
            {
                Console.WriteLine("File does not exist.");
                return null;
            }

            var assetName = Path.GetFileNameWithoutExtension(singleFilePath);
            IAsset inputAsset = _context.Assets.Create(assetName, assetCreationOptions);

            var assetFile = inputAsset.AssetFiles.Create(Path.GetFileName(singleFilePath));

            Console.WriteLine("Upload {0}", assetFile.Name);

            assetFile.Upload(singleFilePath);
            Console.WriteLine("Done uploading {0}", assetFile.Name);

            return inputAsset;
        }
```


## <a name="upload-multiple-files-with-media-services-net-sdk"></a>Meerdere bestanden uploaden met Media Services .NET SDK
De volgende code toont hoe u een asset maken en meerdere bestanden te uploaden.

De code doet het volgende:

* Hiermee maakt u een lege asset met de methode CreateEmptyAsset is gedefinieerd in de vorige stap.
* Hiermee maakt u een **AccessPolicy** exemplaar waarin de machtigingen en de duur van toegang aan de asset.
* Hiermee maakt u een **Locator** -exemplaar dat toegang tot de asset biedt.
* Hiermee maakt u een **BlobTransferClient** exemplaar. Dit type vertegenwoordigt een client die van invloed op de Azure-blobs. In dit voorbeeld controleert de client de uploadvoortgang van de. 
* Inventariseren door middel van bestanden in de opgegeven map en maakt een **AssetFile** -exemplaar voor elk bestand.
* Deze bestanden worden geüpload in Media Services met behulp van de **UploadAsync** methode. 

> [!NOTE]
> Gebruik de methode UploadAsync om ervoor te zorgen dat de aanroepen niet worden geblokkeerd en de bestanden zijn geüpload parallel.
> 
> 

```csharp
        static public IAsset CreateAssetAndUploadMultipleFiles(AssetCreationOptions assetCreationOptions, string folderPath)
        {
            var assetName = "UploadMultipleFiles_" + DateTime.UtcNow.ToString();

            IAsset asset = _context.Assets.Create(assetName, assetCreationOptions);

            var accessPolicy = _context.AccessPolicies.Create(assetName, TimeSpan.FromDays(30),
                                                                AccessPermissions.Write | AccessPermissions.List);

            var locator = _context.Locators.CreateLocator(LocatorType.Sas, asset, accessPolicy);

            var blobTransferClient = new BlobTransferClient();
            blobTransferClient.NumberOfConcurrentTransfers = 20;
            blobTransferClient.ParallelTransferThreadCount = 20;

            blobTransferClient.TransferProgressChanged += blobTransferClient_TransferProgressChanged;

            var filePaths = Directory.EnumerateFiles(folderPath);

            Console.WriteLine("There are {0} files in {1}", filePaths.Count(), folderPath);

            if (!filePaths.Any())
            {
                throw new FileNotFoundException(String.Format("No files in directory, check folderPath: {0}", folderPath));
            }

            var uploadTasks = new List<Task>();
            foreach (var filePath in filePaths)
            {
                var assetFile = asset.AssetFiles.Create(Path.GetFileName(filePath));
                Console.WriteLine("Created assetFile {0}", assetFile.Name);

                // It is recommended to validate AssetFiles before upload. 
                Console.WriteLine("Start uploading of {0}", assetFile.Name);
                uploadTasks.Add(assetFile.UploadAsync(filePath, blobTransferClient, locator, CancellationToken.None));
            }

            Task.WaitAll(uploadTasks.ToArray());
            Console.WriteLine("Done uploading the files");

            blobTransferClient.TransferProgressChanged -= blobTransferClient_TransferProgressChanged;

            locator.Delete();
            accessPolicy.Delete();

            return asset;
        }

    static void  blobTransferClient_TransferProgressChanged(object sender, BlobTransferProgressChangedEventArgs e)
    {
        if (e.ProgressPercentage > 4) // Avoid startup jitter, as the upload tasks are added.
        {
            Console.WriteLine("{0}% upload competed for {1}.", e.ProgressPercentage, e.LocalFile);
        }
    }
```


Tijdens het uploaden van een groot aantal activa, houd rekening met het volgende:

* Maak een nieuwe **CloudMediaContext** object per thread. De **CloudMediaContext** klasse is niet thread-veilig.
* Verhoog NumberOfConcurrentTransfers van de standaardwaarde van 2 tot een hogere waarde zoals 5. Als u deze eigenschap is van invloed op alle exemplaren van **CloudMediaContext**. 
* Houd ParallelTransferThreadCount op de standaardwaarde van 10.

## <a id="ingest_in_bulk"></a>Assets in één bulkbewerking met behulp van Media Services .NET SDK opnemen
Grote assetbestanden worden geüpload, kan een knelpunt zijn tijdens het maken van asset. Assets in Bulk of 'Bulksgewijs opnemen' opnemen, omvat het asset het maken van het uploadproces ontkoppeling. Voor het gebruik van een bulksgewijze benadering opnemen, moet u een manifest (IngestManifest) die worden beschreven van de asset en de bijbehorende bestanden maken. Gebruik vervolgens de uploadmethode van uw keuze de bijbehorende bestanden uploaden naar blob-container van het manifest. Microsoft Azure Media Services controleert de blob-container die zijn gekoppeld aan het manifest. Wanneer een bestand wordt geüpload naar de blob-container, is het asset maken op basis van de configuratie van de activa in het manifest (IngestManifestAsset) in Microsoft Azure Media Services voltooid.

Voor het maken van een nieuwe IngestManifest aanroept die worden weergegeven door de verzameling IngestManifests op de CloudMediaContext maken. Deze methode maakt u een nieuwe IngestManifest met de manifestnaam die u opgeeft.

```csharp
    IIngestManifest manifest = context.IngestManifests.Create(name);
```

De activa die gekoppeld aan het grootste deel IngestManifest zijn maken. Configureer de gewenste versleutelingsopties voor de asset voor het bulksgewijs opnemen.

```csharp
    // Create the assets that will be associated with this bulk ingest manifest
    IAsset destAsset1 = _context.Assets.Create(name + "_asset_1", AssetCreationOptions.None);
    IAsset destAsset2 = _context.Assets.Create(name + "_asset_2", AssetCreationOptions.None);
```

Een Asset koppelt een IngestManifestAsset aan een bulksgewijs IngestManifest voor het bulksgewijs opnemen. Het is ook wordt gekoppeld aan de AssetFiles waaruit alle activa. Voor het maken van een IngestManifestAsset gebruikt u de methode voor maken van de servercontext.

Het volgende voorbeeld ziet u toe te voegen twee nieuwe IngestManifestAssets die de twee activa eerder hebt gemaakt voor de bulkinschrijving koppelen manifest opnemen. Elke IngestManifestAsset koppelt ook een set van bestanden die worden geüpload voor alle activa tijdens bulksgewijs opnemen.  

```csharp
    string filename1 = _singleInputMp4Path;
    string filename2 = _primaryFilePath;
    string filename3 = _singleInputFilePath;

    IIngestManifestAsset bulkAsset1 =  manifest.IngestManifestAssets.Create(destAsset1, new[] { filename1 });
    IIngestManifestAsset bulkAsset2 =  manifest.IngestManifestAssets.Create(destAsset2, new[] { filename2, filename3 });
```

U kunt elke snelle clienttoepassing kan de assetbestanden uploaden naar blob storage-container URI geleverd door de **IIngestManifest.BlobStorageUriForUpload** eigenschap van de IngestManifest. 

De volgende code toont hoe u .NET SDK gebruiken om de activa-bestanden te uploaden.

```csharp
    static void UploadBlobFile(string containerName, string filename)
    {
        Task copytask = new Task(() =>
        {
            var storageaccount = new CloudStorageAccount(new StorageCredentials(_storageAccountName, _storageAccountKey), true);
            CloudBlobClient blobClient = storageaccount.CreateCloudBlobClient();
            CloudBlobContainer blobContainer = blobClient.GetContainerReference(containerName);

            string[] splitfilename = filename.Split('\\');
            var blob = blobContainer.GetBlockBlobReference(splitfilename[splitfilename.Length - 1]);

            using (var stream = System.IO.File.OpenRead(filename))
                blob.UploadFromStream(stream);

            lock (consoleWriteLock)
            {
                Console.WriteLine("Upload for {0} completed.", filename);
            }
        });

        copytask.Start();
    }
```

De code voor het uploaden van de assetbestanden voor het voorbeeld dat wordt gebruikt in dit artikel wordt weergegeven in het volgende codevoorbeeld:

```csharp
    UploadBlobFile(manifest.BlobStorageUriForUpload, filename1);
    UploadBlobFile(manifest.BlobStorageUriForUpload, filename2);
    UploadBlobFile(manifest.BlobStorageUriForUpload, filename3);
```

U kunt bepalen de voortgang van de bulksgewijs opnemen voor alle activa die zijn gekoppeld aan een **IngestManifest** door het opvragen van configuratiegegevens bij de eigenschap statistieken van de **IngestManifest**. Als u wilt bijwerken wordt uitgevoerd, moet u een nieuwe **CloudMediaContext** telkens wanneer u de eigenschap statistieken opvragen.

Het volgende voorbeeld ziet u een IngestManifest door polling de **Id**.

```csharp
    static void MonitorBulkManifest(string manifestID)
    {
       bool bContinue = true;
       while (bContinue)
       {
          CloudMediaContext context = GetContext();
          IIngestManifest manifest = context.IngestManifests.Where(m => m.Id == manifestID).FirstOrDefault();

          if (manifest != null)
          {
             lock(consoleWriteLock)
             {
                Console.WriteLine("\nWaiting on all file uploads.");
                Console.WriteLine("PendingFilesCount  : {0}", manifest.Statistics.PendingFilesCount);
                Console.WriteLine("FinishedFilesCount : {0}", manifest.Statistics.FinishedFilesCount);
                Console.WriteLine("{0}% complete.\n", (float)manifest.Statistics.FinishedFilesCount / (float)(manifest.Statistics.FinishedFilesCount + manifest.Statistics.PendingFilesCount) * 100);

                if (manifest.Statistics.PendingFilesCount == 0)
                {
                   Console.WriteLine("Completed\n");
                   bContinue = false;
                }
             }

             if (manifest.Statistics.FinishedFilesCount < manifest.Statistics.PendingFilesCount)
                Thread.Sleep(60000);
          }
          else // Manifest is null
             bContinue = false;
       }
    }
```


## <a name="upload-files-using-net-sdk-extensions"></a>Bestanden uploaden met .NET SDK Extensions
Het volgende voorbeeld ziet hoe u met behulp van .NET SDK Extensions één bestand uploaden. In dit geval de **CreateFromFile** methode wordt gebruikt, maar ook de asynchrone versie beschikbaar is (**CreateFromFileAsync**). De **CreateFromFile** methode kunt u de bestandsnaam, versleutelingsoptie en een callback om te rapporteren van de uploadvoortgang van het bestand opgeven.

```csharp
    static public IAsset UploadFile(string fileName, AssetCreationOptions options)
    {
        IAsset inputAsset = _context.Assets.CreateFromFile(
            fileName,
            options,
            (af, p) =>
            {
                Console.WriteLine("Uploading '{0}' - Progress: {1:0.##}%", af.Name, p.Progress);
            });

        Console.WriteLine("Asset {0} created.", inputAsset.Id);

        return inputAsset;
    }
```

Het volgende voorbeeld wordt UploadFile-functie aangeroepen en geeft versleuteling van opslag als de optie voor het maken van asset.  

```csharp
    var asset = UploadFile(@"C:\VideoFiles\BigBuckBunny.mp4", AssetCreationOptions.StorageEncrypted);
```

## <a name="next-steps"></a>Volgende stappen

U kunt nu de geüploade assets coderen. Zie [Assets coderen](media-services-portal-encode.md) voor meer informatie.

U kunt ook Azure Functions gebruiken om een coderingstaak te activeren op basis van een bestand dat binnenkomt in de geconfigureerde container. Zie [dit voorbeeld](https://azure.microsoft.com/resources/samples/media-services-dotnet-functions-integration/ ) voor meer informatie.

## <a name="media-services-learning-paths"></a>Media Services-leertrajecten
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="next-step"></a>Volgende stap
Nu dat u een asset naar Media Services hebt geüpload, gaat u naar de [over het verkrijgen van een Mediaprocessor] [ How to Get a Media Processor] artikel.

[How to Get a Media Processor]: media-services-get-media-processor.md

