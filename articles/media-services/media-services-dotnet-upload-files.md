---
title: Bestanden uploaden naar een Media Services-account met .NET | Microsoft Docs
description: Informatie over het ophalen van media-inhoud in Media Services door het maken en uploaden van activa.
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: c9c86380-9395-4db8-acea-507c52066f73
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/12/2017
ms.author: juliako
ms.openlocfilehash: ec8c1da633374ba684f6a0a895c542ee76ef73b8
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="upload-files-into-a-media-services-account-using-net"></a>Bestanden uploaden naar een Media Services-account met .NET
> [!div class="op_single_selector"]
> * [.NET](media-services-dotnet-upload-files.md)
> * [REST](media-services-rest-upload-files.md)
> * [Portal](media-services-portal-upload-files.md)
> 
> 

In Media Services moet u uw digitale bestanden uploaden naar (of opnemen in) een asset. De **Asset** entiteit kan bevatten, video, audio, afbeeldingen, verzamelingen miniaturen, tekst tekstsporen en ondertitelingsbestanden bestanden (en de metagegevens over deze bestanden.)  Zodra de bestanden zijn geüpload, wordt uw inhoud veilig opgeslagen in de cloud voor verdere verwerking en streaming.

De bestanden in de asset worden **assetbestanden** genoemd. De **AssetFile** exemplaar en de werkelijke mediabestand zijn twee verschillende objecten. Het exemplaar AssetFile bevat metagegevens over het mediabestand terwijl het mediabestand de werkelijke media-inhoud bevat.

> [!NOTE]
> Het volgende letten:
> 
> * Media Services gebruikt de waarde van de eigenschap IAssetFile.Name tijdens het bouwen van URL's voor de streaming-inhoud (bijvoorbeeld http://{AMSAccount}.origin.mediaservices.windows.net/{GUID}/{IAssetFile.Name}/streamingParameters.) Om deze reden is procent codering niet toegestaan. De waarde van de **naam** eigenschap kan niet een van de volgende hebben [procent-encoding-gereserveerde tekens](http://en.wikipedia.org/wiki/Percent-encoding#Percent-encoding_reserved_characters):! *' ();: @& = + $, /? % # [] '. Bovendien kunnen alleen er een '.' voor de bestandsnaamextensie.
> * De lengte van de naam mag niet groter zijn dan 260 tekens zijn.
> * Er is een limiet voor de maximale bestandsgrootte die wordt ondersteund voor verwerking in Media Services. Raadpleeg [dit onderwerp](media-services-quotas-and-limitations.md) voor meer informatie over de maximale bestandsgrootte.
> * Er geldt een limiet van 1.000.000 beleidsregels voor verschillende AMS-beleidsitems (bijvoorbeeld voor Locator-beleid of ContentKeyAuthorizationPolicy). U moet dezelfde beleids-id gebruiken als u altijd dezelfde dagen/toegangsmachtigingen gebruikt, bijvoorbeeld beleidsregels voor locators die zijn bedoeld om gedurende een lange periode gehandhaafd te blijven (niet-upload-beleidsregels). Raadpleeg [dit](media-services-dotnet-manage-entities.md#limit-access-policies) onderwerp voor meer informatie.
> 

Wanneer u activa maakt, kunt u de volgende versleutelingsopties. 

* **Geen**: er wordt geen versleuteling gebruikt. Dit is de standaardwaarde. Houd er rekening mee dat wanneer u deze optie uw inhoud wordt niet beveiligd tijdens de overdracht of in rust in de opslag.
  Als u een MP4-bestand wilt leveren via progressief downloaden, gebruikt u deze optie. 
* **CommonEncryption** -Gebruik deze optie als u inhoud uploadt die al is versleuteld en beveiligd met Common Encryption of PlayReady DRM (bijvoorbeeld Smooth Streaming beveiligd met PlayReady DRM).
* **EnvelopeEncrypted** – Gebruik deze optie als u HLS versleuteld met AES uploaden wilt. Houd er rekening mee dat de bestanden moeten zijn gecodeerd en versleuteld door Transform Manager.
* **StorageEncrypted** - versleutelt uw versleutelde inhoud lokaal met AES-256-bitsversleuteling, en vervolgens geüpload naar Azure Storage wordt bewaard in rust versleuteld. De versleuteling van assets die zijn beveiligd met Storage Encryption, wordt automatisch ongedaan gemaakt en de assets worden automatisch in een versleuteld bestandssysteem geplaatst voordat ze worden gecodeerd. Eventueel kunnen ze opnieuw worden versleuteld voordat ze opnieuw worden geüpload als een nieuwe uitvoerasset. Het primaire gebruiksvoorbeeld Storage Encryption is wanneer u wilt beveiligen van uw invoer media van hoge kwaliteit bestanden met een sterke codering in rust op schijf.
  
    Media Services biedt versleuteling van opslag op schijf voor de activa, niet over-the-wire zoals Manager van digitale rechten (DRM).
  
    Als uw asset opslag versleuteld is, moet u het leveringsbeleid voor Assets configureren. Zie voor meer informatie [leveringsbeleid voor Assets configureren](media-services-dotnet-configure-asset-delivery-policy.md).

Als u voor uw asset worden versleuteld opgeeft met een **CommonEncrypted** optie, of een **EnvelopeEncypted** optie, moet u uw asset met koppelen een **ContentKey**. Zie voor meer informatie [het maken van een ContentKey](media-services-dotnet-create-contentkey.md). 

Als u voor uw asset worden versleuteld opgeeft met een **StorageEncrypted** optie, de Media Services SDK voor .NET maakt een **StorateEncrypted** **ContentKey** voor uw asset.

Dit onderwerp leest hoe u met Media Services .NET SDK, evenals een Media Services .NET SDK extensions bestanden uploaden naar een Media Services-asset.

## <a name="upload-a-single-file-with-media-services-net-sdk"></a>Uploaden van één bestand met Media Services .NET SDK
De voorbeeldcode hieronder gebruikt .NET SDK om één bestand uploaden. De AccessPolicy en Locator zijn gemaakt en vernietigd door de functie uploaden. 


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


## <a name="upload-multiple-files-with-media-services-net-sdk"></a>Uploaden van meerdere bestanden met Media Services .NET SDK
De volgende code laat zien hoe een asset maken en meerdere bestanden uploaden.

De code doet het volgende:

* Maakt een lege asset via de methode CreateEmptyAsset is gedefinieerd in de vorige stap.
* Maakt een **AccessPolicy** exemplaar waarin de machtigingen en de duur van toegang voor de activa.
* Maakt een **Locator** exemplaar dat toegang tot de asset biedt.
* Maakt een **BlobTransferClient** exemplaar. Dit type vertegenwoordigt een client die wordt toegepast op de Azure blobs. In dit voorbeeld gebruiken we de client de voortgang van uploaden. 
* Inventariseren door middel van bestanden in de opgegeven map en maakt een **AssetFile** exemplaar voor elk bestand.
* De bestanden geüpload naar Media Services met behulp de **UploadAsync** methode. 

> [!NOTE]
> Gebruik de methode UploadAsync om ervoor te zorgen dat de aanroepen niet worden geblokkeerd en de bestanden zijn geüpload parallel.
> 
> 

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

                // It is recommended to validate AccestFiles before upload. 
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



Houd rekening met de volgende tijdens het uploaden van een groot aantal activa.

* Maak een nieuwe **CloudMediaContext** object per thread. De **CloudMediaContext** klasse is niet thread-veilig.
* Verhoog NumberOfConcurrentTransfers vanuit de standaardwaarde van 2 tot een hogere waarde zoals 5. Als u deze eigenschap is van invloed op alle exemplaren van **CloudMediaContext**. 
* Houd ParallelTransferThreadCount op de standaardwaarde van 10.

## <a id="ingest_in_bulk"></a>Het opnemen van de activa in Bulk met Media Services .NET SDK
Grote assetbestanden uploaden, kan een knelpunt zijn tijdens het maken van asset. Omvat het opnemen van activa in Bulk of "Opnemen van grote hoeveelheden" asset maken via het uploadproces ontkoppeling. Maak een manifest (IngestManifest) die worden beschreven de asset en de bijbehorende bestanden wilt gebruiken een bulksgewijs benadering opnemen. Gebruik de uploadmethode van uw keuze vervolgens de bijbehorende bestanden uploaden naar blob-container van het manifest. Microsoft Azure Media Services bewaakt de blob-container die is gekoppeld aan het manifest. Wanneer een bestand is geüpload naar de blob-container, is het asset maken op basis van de configuratie van de activa in het manifest (IngestManifestAsset) in Microsoft Azure Media Services voltooid.

Een nieuwe IngestManifest aanroepen voor het maken van de methode Create door de verzameling IngestManifests op de CloudMediaContext beschikbaar gesteld. Deze methode maakt een nieuwe IngestManifest met de manifest die u opgeeft.

    IIngestManifest manifest = context.IngestManifests.Create(name);

De assets die gekoppeld aan het bulksgewijs IngestManifest zijn maken. Configureer de gewenste versleutelingsopties op de asset voor het opnemen van grote hoeveelheden.

    // Create the assets that will be associated with this bulk ingest manifest
    IAsset destAsset1 = _context.Assets.Create(name + "_asset_1", AssetCreationOptions.None);
    IAsset destAsset2 = _context.Assets.Create(name + "_asset_2", AssetCreationOptions.None);

Een Asset koppelt een IngestManifestAsset aan een bulksgewijs IngestManifest voor het opnemen van grote hoeveelheden. Ook wordt de AssetFiles waaruit alle activa gekoppeld. Voor het maken van een IngestManifestAsset gebruikt u de methode Create in de servercontext.

Het volgende voorbeeld toont toe te voegen twee nieuwe IngestManifestAssets die de twee activa eerder hebt gemaakt voor de bulkinschrijving koppelen manifest opnemen. Elke IngestManifestAsset koppelt ook een set van bestanden die worden geüpload voor elk actief tijdens het opnemen van grote hoeveelheden.  

    string filename1 = _singleInputMp4Path;
    string filename2 = _primaryFilePath;
    string filename3 = _singleInputFilePath;

    IIngestManifestAsset bulkAsset1 =  manifest.IngestManifestAssets.Create(destAsset1, new[] { filename1 });
    IIngestManifestAsset bulkAsset2 =  manifest.IngestManifestAssets.Create(destAsset2, new[] { filename2, filename3 });

U kunt een clienttoepassing hoge snelheid kunnen de assetbestanden uploaden naar blob storage-container URI dat is opgegeven door de **IIngestManifest.BlobStorageUriForUpload** eigenschap van de IngestManifest. Een opmerkelijke hoge snelheid uploaden service [Aspera On Demand voor Azure-toepassing](https://datamarket.azure.com/application/2cdbc511-cb12-4715-9871-c7e7fbbb82a6). U kunt ook de code voor het uploaden van de bestanden van de activa zoals weergegeven in het volgende codevoorbeeld schrijven.

    static void UploadBlobFile(string destBlobURI, string filename)
    {
        Task copytask = new Task(() =>
        {
            var storageaccount = new CloudStorageAccount(new StorageCredentials(_storageAccountName, _storageAccountKey), true);
            CloudBlobClient blobClient = storageaccount.CreateCloudBlobClient();
            CloudBlobContainer blobContainer = blobClient.GetContainerReference(destBlobURI);

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

De code voor het uploaden van de assetbestanden voor het voorbeeld in dit onderwerp gebruikt, wordt weergegeven in het volgende codevoorbeeld.

    UploadBlobFile(manifest.BlobStorageUriForUpload, filename1);
    UploadBlobFile(manifest.BlobStorageUriForUpload, filename2);
    UploadBlobFile(manifest.BlobStorageUriForUpload, filename3);


U kunt bepalen de voortgang van het opnemen van grote hoeveelheden voor alle activa die zijn gekoppeld aan een **IngestManifest** door de eigenschap statistieken van de **IngestManifest**. Om bij te werken voortgangsinformatie, moet u een nieuwe **CloudMediaContext** telkens wanneer u de eigenschap statistieken opvragen.

Het volgende voorbeeld toont een IngestManifest door polling de **Id**.

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



## <a name="upload-files-using-net-sdk-extensions"></a>Uploaden van bestanden met behulp van de .NET SDK Extensions
Het volgende voorbeeld laat zien hoe één bestand met .NET SDK Extensions uploaden. In dit geval de **CreateFromFile** methode wordt gebruikt, maar ook de asynchrone versie beschikbaar is (**CreateFromFileAsync**). De **CreateFromFile** methode kunt u de bestandsnaam, versleutelingsoptie en een retouraanroep zijn om de uploadvoortgang van het bestand te rapporteren.

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

Het volgende voorbeeld UploadFile functie aanroept en versleuteling van opslag geeft als optie voor het maken van asset.  

    var asset = UploadFile(@"C:\VideoFiles\BigBuckBunny.mp4", AssetCreationOptions.StorageEncrypted);

## <a name="next-steps"></a>Volgende stappen

U kunt nu de geüploade assets coderen. Zie [Assets coderen](media-services-portal-encode.md) voor meer informatie.

U kunt ook Azure Functions gebruiken om een coderingstaak te activeren op basis van een bestand dat binnenkomt in de geconfigureerde container. Zie [dit voorbeeld](https://azure.microsoft.com/resources/samples/media-services-dotnet-functions-integration/ ) voor meer informatie.

## <a name="media-services-learning-paths"></a>Media Services-leertrajecten
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="next-step"></a>Volgende stap
Nu dat u hebt een actief geüpload met Media Services maken, gaat u naar de [krijgen een Processor Media] [ How to Get a Media Processor] onderwerp.

[How to Get a Media Processor]: media-services-get-media-processor.md

