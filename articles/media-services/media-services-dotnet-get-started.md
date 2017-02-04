---
title: Aan de slag met het leveren van inhoud on demand met .NET | Microsoft Docs
description: In deze zelfstudie leert u een eenvoudige toepassing voor de levering van on demand inhoud te implementeren met Azure Media Services door gebruik te maken van .NET.
services: media-services
documentationcenter: 
author: Juliako
manager: erikre
editor: 
ms.assetid: 388b8928-9aa9-46b1-b60a-a918da75bd7b
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 12/11/2016
ms.author: juliako
translationtype: Human Translation
ms.sourcegitcommit: 24d324a724792051eb6d86026da7b41ee9ff87b1
ms.openlocfilehash: 26720340d72c31016e51cc33589388780a2f4a8a


---

# <a name="get-started-with-delivering-content-on-demand-using-net-sdk"></a>Aan de slag met het leveren van inhoud on demand met .NET SDK
[!INCLUDE [media-services-selector-get-started](../../includes/media-services-selector-get-started.md)]

> [!NOTE]
> U hebt een Azure-account nodig om deze zelfstudie te voltooien. Zie [Gratis proefversie van Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) voor meer informatie. 
> 
> 

## <a name="overview"></a>Overzicht
In deze zelfstudie leert u een eenvoudige toepassing voor de levering van VoD-inhoud (Video-on-Demand) te implementeren met Azure Media Services door gebruik te maken van de Azure Media Services (AMS) SDK voor .NET.

In deze zelfstudie maakt u kennis met de algemene werkstroom voor Media Services en de meest algemene programmeerobjecten en -taken die zijn vereist voor het ontwikkelen van Media Services. Wanneer u de zelfstudie hebt voltooid, kunt u een voorbeeldmediabestand streamen of progressief downloaden dat u hebt eerder hebt geüpload, gecodeerd of gedownload.

## <a name="what-youll-learn"></a>Wat u leert
De zelfstudie laat zien hoe u de volgende taken uitvoert:

1. Een Media Services-account (met Azure Portal).
2. Een streaming-eindpunt configureren (met Azure Portal).
3. Een Visual Studio-project maken en configureren.
4. Verbinding maken met het Azure Media Services-account.
5. Een nieuwe asset maken en een videobestand uploaden.
6. Het bronbestand coderen in een set Adaptive Bitrate MP4-bestanden.
7. De asset publiceren en URL's voor streamen en progressief downloaden ophalen.
8. Testen door uw inhoud af te spelen.

## <a name="prerequisites"></a>Vereisten
Hieronder wordt aangegeven wat de vereisten zijn om de zelfstudie te voltooien.

* U hebt een Azure-account nodig om deze zelfstudie te voltooien. 
  
    Als u geen account hebt, kunt u binnen een paar minuten een account voor de gratis proefversie maken. Zie [Gratis proefversie van Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) voor meer informatie. U ontvangt tegoed dat kan worden gebruikt om betaalde Azure-services te proberen. Zelfs nadat het tegoed is gebruikt, kunt u het account houden en de gratis Azure-services en -functies gebruiken, zoals de functie Web-apps in Azure App Service.
* Besturingssystemen: Windows 8 of hoger, Windows 2008 R2, Windows 7.
* .NET framework 4.0 of hoger
* Visual Studio 2010 SP1 (Professional, Premium, Ultimate of Express) of hoger.

## <a name="download-sample"></a>Voorbeeld downloaden
U kunt [hier](https://azure.microsoft.com/documentation/samples/media-services-dotnet-on-demand-encoding-with-media-encoder-standard/) een voorbeeld ophalen en uitvoeren.

## <a name="create-an-azure-media-services-account-using-the-azure-portal"></a>Een Azure Media Services-account maken met Azure Portal
In de stappen in deze sectie wordt uitgelegd hoe u een AMS-account maakt.

1. Meld u aan bij de [Azure Portal](https://portal.azure.com/).
2. Klik op **+Nieuw** > **Media + CDN** > **Mediaservices**.
   
    ![Media Services-account maken](./media/media-services-portal-vod-get-started/media-services-new1.png)
3. Voer bij **MEDIA SERVICES-ACCOUNT MAKEN** de vereiste waarden in.
   
    ![Media Services-account maken](./media/media-services-portal-vod-get-started/media-services-new3.png)
   
   1. Voer in **Accountnaam** de naam van het nieuwe AMS-account in. Voor de naam van een Media Services-account mogen alleen cijfers of kleine letters zonder spaties worden gebruikt. De naam mag 3 tot 24 tekens lang zijn.
   2. Selecteer in Abonnement een van de verschillende Azure-abonnementen waartoe u toegang hebt.
   3. Selecteer in **Resourcegroep** de nieuwe of bestaande resource.  Een resourcegroep is een verzameling resources met dezelfde levenscyclus, dezelfde machtigingen en hetzelfde beleid. Klik [hier](../azure-resource-manager/resource-group-overview.md#resource-groups) voor meer informatie.
   4. Selecteer bij **Locatie** de geografische regio die wordt gebruikt om de media en metagegevensrecords voor uw Media Services-account op te slaan. Deze regio wordt gebruikt om uw media te verwerken en te streamen. Alleen de beschikbare Media Services-regio's worden in de vervolgkeuzelijst weergegeven. 
   5. Selecteer bij **Opslagaccount** een opslagaccount om Blob Storage van de media-inhoud vanaf uw Media Services-account te leveren. U kunt een bestaand opslagaccount selecteren in dezelfde geografische regio als uw Media Services-account of u kunt een opslagaccount maken. Een nieuw opslagaccount wordt in dezelfde regio gemaakt. De regels voor opslagaccountnamen zijn hetzelfde als voor Media Services-accounts.
      
       Klik [hier](../storage/storage-introduction.md) voor meer informatie over opslag.
   6. Selecteer **Vastmaken aan dashboard** om de voortgang van de implementatie van het account te bekijken.
4. Klik op **Maken** onder in het formulier.
   
    Als het account is gemaakt, wordt de status gewijzigd in **Actief**. 
   
    ![Media Services-instellingen](./media/media-services-portal-vod-get-started/media-services-settings.png)
   
    Als u uw AMS-account wilt beheren (bijvoorbeeld video's uploaden, assets coderen, de voortgang van een taak bewaken), gebruikt u het venster **Instellingen**.

## <a name="configure-streaming-endpoints-using-the-azure-portal"></a>Streaming-eindpunten configureren met Azure Portal
Bij het werken met Azure Media Services wordt video meestal via Adaptive Bitrate Streaming aan de clients geleverd. Media Services ondersteunt de volgende Adaptive Bitrate Streaming-technologieën: HLS (HTTP Live Streaming), Smooth Streaming en MPEG DASH.

Media Services biedt dynamische pakketten waarmee u uw Adaptive Bitrate MP4-inhoud 'just in time' kunt leveren in de streaming-indelingen die door Media Services worden ondersteund (MPEG DASH, HLS, Smooth Streaming), zonder dat u vooraf verpakte versies van elk van deze streaming-indelingen hoeft op te slaan.

Als u dynamische pakketten wilt gebruiken, moet u het volgende doen:

* Codeer uw tussentijds (bron)bestand in een set Adaptive Bitrate MP4-bestanden (de coderingsstappen worden verderop in deze zelfstudie uitgelegd).  
* Maak ten minste één streaming-eenheid voor het *streaming-eindpunt* van waaruit u uw inhoud wilt leveren. De volgende stappen laten zien hoe u het aantal streaming-eenheden wijzigt.

Voor dynamische pakketten hoeft u voor slechts één opslagindeling de bestanden op te slaan en hiervoor te betalen. Media Services bouwt en levert de juiste reactie op basis van aanvragen van een client.

Ga als volgt te werk als u het aantal eenheden wilt maken en wijzigen dat voor streaming is gereserveerd:

1. Klik in het venster **Instellingen** op **Streaming-eindpunten**. 
2. Klik op het standaardstreaming-eindpunt. 
   
    Het venster **DEFAULT STREAMING ENDPOINT DETAILS** (DETAILS VAN STANDAARDSTREAMING-EINDPUNT) wordt weergegeven.
3. Geef het aantal streaming-eenheden op door de schuifregelaar **Streaming-eenheden** te verplaatsen.
   
    ![Streaming-eenheden](./media/media-services-portal-vod-get-started/media-services-streaming-units.png)
4. Klik op de knop **Opslaan** om uw wijzigingen op te slaan.
   
   > [!NOTE]
   > Het kan tot twintig minuten duren tot de toewijzing van nieuwe eenheden is voltooid.
   > 
   > 

## <a name="create-and-configure-a-visual-studio-project"></a>Maak en configureer een Visual Studio-project.

1. Maak een nieuwe C#-consoletoepassing in Visual Studio 2013, Visual Studio 2012 of Visual Studio 2010 SP1. Geef de **naam**, **locatie** en **naam van de oplossing** op en klik vervolgens op **OK**.
2. Gebruik het [windowsazure.mediaservices.extensions](https://www.nuget.org/packages/windowsazure.mediaservices.extensions) NuGet-pakket om **Azure Media Services .NET SDK Extensions** te installeren.  Media Services .NET SDK Extensions bevat een set uitbreidingsmethoden en Help-functies die uw code vereenvoudigen en het u gemakkelijker maken om met Media Services toepassingen te ontwikkelen. Als u dit pakket installeert, wordt ook de **Media Services .NET SDK** geïnstalleerd en worden alle andere vereiste afhankelijkheden toegevoegd.

    Verwijzingen toevoegen met behulp van NuGet: klik in Solution Explorer met de rechtermuisknop op de projectnaam en selecteer **NuGet-pakketten beheren**. Zoek vervolgens naar **windowsazure.mediaservices.extensions** en klik op **Installeren**.

3. Voeg een verwijzing naar de System.Configuration-assembly toe. Deze assembly bevat de klasse **System.Configuration.ConfigurationManager** die wordt gebruikt voor toegang tot de configuratiebestanden, bijvoorbeeld App.config.

    Verwijzingen toevoegen met behulp van NuGet: klik in Solution Explorer met de rechtermuisknop op de projectnaam en selecteer **Toevoegen** > **Verwijzing...** en het type configuratie in het zoekvak. 

4. Open het bestand App.config (voeg het bestand toe aan uw project als dit niet standaard wordt toegevoegd) en voeg de sectie *appSettings* aan het bestand toe. Stel de waarden voor de naam van uw Azure Media Services-account en de accountsleutel in, zoals wordt weergegeven in het volgende voorbeeld. Voor het verkrijgen van de accountnaam en sleutelinformatie gaat u naar de [Azure Portal](https://portal.azure.com/) en selecteert u uw AMS-account. Selecteer vervolgens **Instellingen** > **Sleutels**. In het venster Sleutels beheren worden de accountnaam en de primaire en secundaire sleutel weergegeven. Kopieer de waarden van de accountnaam en de primaire sleutel.
   
        <configuration>
        ...
          <appSettings>
            <add key="MediaServicesAccountName" value="Media-Services-Account-Name" />
            <add key="MediaServicesAccountKey" value="Media-Services-Account-Key" />
          </appSettings>
   
        </configuration>
5. Overschrijf de bestaande instructies aan het begin van het bestand Program.cs **met** de volgende code.
   
        using System;
        using System.Collections.Generic;
        using System.Linq;
        using System.Text;
        using System.Threading.Tasks;
        using System.Configuration;
        using System.Threading;
        using System.IO;
        using Microsoft.WindowsAzure.MediaServices.Client;
6. Maak een nieuwe map in de projectdirectory en kopieer het MP4- of WMV-bestand dat u wilt coderen en streamen of progressief wilt downloaden. In dit voorbeeld wordt het pad C:\VideoFiles gebruikt.

## <a name="connect-to-the-media-services-account"></a>Verbinding met het Azure Media Services-account maken

Als u Media Services gebruikt met .NET, moet u voor de meeste Media Services-programmeertaken de klasse **CloudMediaContext** gebruiken: verbinding maken met het Media Services-account; maken, bijwerken, gebruiken en verwijderen van de volgende objecten: assets, assetbestanden, taken, toegangsbeleid, locators enzovoort.

Overschrijf de standaardklasse Program met de volgende code. De code laat zien u hoe de verbindingswaarden in het bestand App.config kunt lezen en hoe u het object **CloudMediaContext** maakt om verbinding met Media Services te maken. Zie [Verbinding met Media Services maken via de Media Services SDK voor .NET](http://msdn.microsoft.com/library/azure/jj129571.aspx) voor meer informatie over het maken van verbinding met Media Services.


Met de functie **Main** worden methoden aangeroepen die later in deze sectie verder worden gedefinieerd.

> [!NOTE]
> Er worden compilatiefouten geretourneerd totdat u definities hebt toegevoegd voor alle functies.

    class Program
    {
        // Read values from the App.config file.
        private static readonly string _mediaServicesAccountName =
            ConfigurationManager.AppSettings["MediaServicesAccountName"];
        private static readonly string _mediaServicesAccountKey =
            ConfigurationManager.AppSettings["MediaServicesAccountKey"];

        // Field for service context.
        private static CloudMediaContext _context = null;
        private static MediaServicesCredentials _cachedCredentials = null;

        static void Main(string[] args)
        {
            try
            {
                // Create and cache the Media Services credentials in a static class variable.
                _cachedCredentials = new MediaServicesCredentials(
                                _mediaServicesAccountName,
                                _mediaServicesAccountKey);
                // Used the chached credentials to create CloudMediaContext.
                _context = new CloudMediaContext(_cachedCredentials);

                // Add calls to methods defined in this section.

                IAsset inputAsset =
                    UploadFile(@"C:\VideoFiles\BigBuckBunny.mp4", AssetCreationOptions.None);

                IAsset encodedAsset =
                    EncodeToAdaptiveBitrateMP4s(inputAsset, AssetCreationOptions.None);

                PublishAssetGetURLs(encodedAsset);
            }
            catch (Exception exception)
            {
                // Parse the XML error message in the Media Services response and create a new
                // exception with its content.
                exception = MediaServicesExceptionParser.Parse(exception);

                Console.Error.WriteLine(exception.Message);
            }
            finally
            {
                Console.ReadLine();
            }
        }
    }

## <a name="create-a-new-asset-and-upload-a-video-file"></a>Een nieuwe asset maken en een videobestand uploaden

In Media Services moet u uw digitale bestanden uploaden naar (of opnemen in) een asset. De entiteit **Asset** kan video, audio, afbeeldingen, verzamelingen miniaturen, tekstsporen en ondertitelingsbestanden (en de metagegevens over deze bestanden) bevatten.  Zodra de bestanden zijn geüpload, wordt uw inhoud veilig opgeslagen in de cloud voor verdere verwerking en streaming. De bestanden in de asset worden **assetbestanden** genoemd.

Met de methode **UploadFile**, zoals hieronder gedefinieerd, wordt **CreateFromFile** (gedefinieerd in .NET SDK Extensions) aangeroepen. Met **CreateFromFile** wordt een nieuwe asset gemaakt waarnaar het opgegeven bestand wordt geüpload.

De methode **CreateFromFile** maakt gebruik van **AssetCreationOptions**, waarmee u een van de volgende opties voor het maken van assets kunt opgeven:

* **Geen**: er wordt geen versleuteling gebruikt. Dit is de standaardwaarde. Houd er rekening mee dat bij gebruik van deze optie de inhoud tijdens de overdracht of in de opslag niet is beveiligd.
  Als u een MP4-bestand wilt leveren via progressief downloaden, gebruikt u deze optie.
* **StorageEncrypted**: gebruik deze optie om uw niet-versleutelde inhoud lokaal te versleutelen met Advanced Encryption Standard (AES)&256;-bitsversleuteling, waarna de inhoud wordt geüpload naar en versleuteld wordt bewaard in Azure Storage. De versleuteling van assets die zijn beveiligd met Storage Encryption, wordt automatisch ongedaan gemaakt en de assets worden automatisch in een versleuteld bestandssysteem geplaatst voordat ze worden gecodeerd. Eventueel kunnen ze opnieuw worden versleuteld voordat ze opnieuw worden geüpload als een nieuwe uitvoerasset. Storage Encryption wordt voornamelijk gebruikt om uw invoerbestanden met media van hoge kwaliteit die zijn opgeslagen op de schijf, te beveiligen met een sterke versleuteling.
* **CommonEncryptionProtected**: gebruik deze optie als u inhoud uploadt die al is versleuteld en beveiligd met Common Encryption of PlayReady DRM (bijvoorbeeld Smooth Streaming beveiligd met PlayReady DRM).
* **EnvelopeEncryptionProtected**: gebruik deze optie als u een HLS-stream uploadt die is versleuteld met AES. Houd er rekening mee dat de bestanden moeten zijn gecodeerd en versleuteld door Transform Manager.

Met de methode **CreateFromFile** kunt u ook een retouraanroep opgeven om de uploadvoortgang van het bestand te rapporteren.

In het volgende voorbeeld is voor de assetopties de waarde **Geen** opgegeven.

Voeg de volgende methode toe aan de klasse Program.

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


## <a name="encode-the-source-file-into-a-set-of-adaptive-bitrate-mp4-files"></a>Het bronbestand coderen in een set Adaptive Bitrate MP4-bestanden
Nadat assets zijn opgenomen in Media Services, kan de media worden gecodeerd, transmuxed, van een watermerk worden voorzien enzovoort, voordat deze aan clients wordt geleverd. Deze activiteiten worden gepland en uitgevoerd op meerdere achtergrondrolinstanties om hoge prestaties en een hoge beschikbaarheid te garanderen. Deze activiteiten worden taken genoemd. Elke taak bestaat uit atomische taken die daadwerkelijk werken op het assetbestand.

Zoals eerder al is aangegeven, wordt bij het werken met Azure Media Services meestal Adaptive Bitrate Streaming aan de clients geleverd. Met Media Services kunt u een dynamisch pakket met een van de volgende indelingen van MP4-bestanden met een adaptieve bitsnelheid maken: HTTP Live Streaming (HLS), Smooth Streaming en MPEG DASH.

Als u dynamische pakketten wilt gebruiken, moet u het volgende doen:

* Codeer of transcodeer uw tussentijds (bron)bestand naar een set MP4-bestanden met een adaptieve bitsnelheid of naar Smooth Streaming-bestanden met een adaptieve bitsnelheid.  
* Haal ten minste één streaming-eenheid op voor het streaming-eindpunt van waaruit u uw inhoud wilt leveren.

De volgende code toont u hoe u een codeertaak verzendt. De taak bevat één taak die aangeeft dat het tussentijdse bestand met **Media Encoder Standard** moet worden getranscodeerd in een set MP4-bestanden met een adaptieve bitsnelheid. De code verzendt de taak en wacht totdat de taak is voltooid.

Zodra de taak is voltooid, kunt u uw asset streamen of MP4-bestanden die zijn gemaakt naar aanleiding van een transcodering progressief downloaden.
U hoeft niet meer dan nul streaming-eenheden te hebben om MP4-bestanden progressief te downloaden.

Voeg de volgende methode toe aan de klasse Program.

    static public IAsset EncodeToAdaptiveBitrateMP4s(IAsset asset, AssetCreationOptions options)
    {

        // Prepare a job with a single task to transcode the specified asset
        // into a multi-bitrate asset.

        IJob job = _context.Jobs.CreateWithSingleTask(
            "Media Encoder Standard",
            "H264 Multiple Bitrate 720p",
            asset,
            "Adaptive Bitrate MP4",
            options);

        Console.WriteLine("Submitting transcoding job...");


        // Submit the job and wait until it is completed.
        job.Submit();

        job = job.StartExecutionProgressTask(
            j =>
            {
                Console.WriteLine("Job state: {0}", j.State);
                Console.WriteLine("Job progress: {0:0.##}%", j.GetOverallProgress());
            },
            CancellationToken.None).Result;

        Console.WriteLine("Transcoding job finished.");

        IAsset outputAsset = job.OutputMediaAssets[0];

        return outputAsset;
    }

## <a name="publish-the-asset-and-get-urls-for-streaming-and-progressive-download"></a>De asset publiceren en URL's ophalen voor streamen en progressief downloaden

Als u een asset wilt streamen of downloaden, moet u deze eerste publiceren door een locator te maken. Locators bieden toegang tot bestanden in de asset. Media Services ondersteunt twee typen locators: OnDemandOrigin-locators, voor het streamen van media (bijvoorbeeld MPEG DASH, HLS, of Smooth Streaming), en SAS-locators (Shared Access Signature), voor het downloaden van mediabestanden. (Ga naar [dit](http://southworks.com/blog/2015/05/27/reusing-azure-media-services-locators-to-avoid-facing-the-5-shared-access-policy-limitation/) blog voor meer informatie over SAS-locators.)

Nadat u de locators hebt gemaakt, kunt u de URL's maken die worden gebruikt om uw bestanden te streamen of te downloaden.

Een streaming-URL voor Smooth Streaming heeft de volgende indeling:

     {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest

Een streaming-URL voor HLS heeft de volgende indeling:

     {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)

Een streaming-URL voor MPEG DASH heeft de volgende indeling:

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)

Een SAS-URL die wordt gebruikt om bestanden te downloaden, heeft de volgende indeling:

    {blob container name}/{asset name}/{file name}/{SAS signature}

Media Services .NET SDK Extensions bieden handige Help-methoden die ingedeelde URL's voor de gepubliceerde asset retourneren.

De volgende code gebruikt .NET SDK Extensions om locators te maken en URL's op te halen die u kunt gebruiken om te streamen of progressief te downloaden. De code toont u ook hoe u bestanden downloadt naar een lokale map.

Voeg de volgende methode toe aan de klasse Program.

    static public void PublishAssetGetURLs(IAsset asset)
    {
        // Publish the output asset by creating an Origin locator for adaptive streaming,
        // and a SAS locator for progressive download.

        _context.Locators.Create(
            LocatorType.OnDemandOrigin,
            asset,
            AccessPermissions.Read,
            TimeSpan.FromDays(30));

        _context.Locators.Create(
            LocatorType.Sas,
            asset,
            AccessPermissions.Read,
            TimeSpan.FromDays(30));


        IEnumerable<IAssetFile> mp4AssetFiles = asset
                .AssetFiles
                .ToList()
                .Where(af => af.Name.EndsWith(".mp4", StringComparison.OrdinalIgnoreCase));

        // Get the Smooth Streaming, HLS and MPEG-DASH URLs for adaptive streaming,
        // and the Progressive Download URL.
        Uri smoothStreamingUri = asset.GetSmoothStreamingUri();
        Uri hlsUri = asset.GetHlsUri();
        Uri mpegDashUri = asset.GetMpegDashUri();

        // Get the URls for progressive download for each MP4 file that was generated as a result
        // of encoding.
        List<Uri> mp4ProgressiveDownloadUris = mp4AssetFiles.Select(af => af.GetSasUri()).ToList();


        // Display  the streaming URLs.
        Console.WriteLine("Use the following URLs for adaptive streaming: ");
        Console.WriteLine(smoothStreamingUri);
        Console.WriteLine(hlsUri);
        Console.WriteLine(mpegDashUri);
        Console.WriteLine();

        // Display the URLs for progressive download.
        Console.WriteLine("Use the following URLs for progressive download.");
        mp4ProgressiveDownloadUris.ForEach(uri => Console.WriteLine(uri + "\n"));
        Console.WriteLine();

        // Download the output asset to a local folder.
        string outputFolder = "job-output";
        if (!Directory.Exists(outputFolder))
        {
            Directory.CreateDirectory(outputFolder);
        }

        Console.WriteLine();
        Console.WriteLine("Downloading output asset files to a local folder...");
        asset.DownloadToFolder(
            outputFolder,
            (af, p) =>
            {
                Console.WriteLine("Downloading '{0}' - Progress: {1:0.##}%", af.Name, p.Progress);
            });

        Console.WriteLine("Output asset files available at '{0}'.", Path.GetFullPath(outputFolder));
    }

## <a name="test-by-playing-your-content"></a>Testen door uw inhoud af te spelen
Zodra u het programma uitvoert dat in de vorige sectie is gedefinieerd, worden er URL's in het consolevenster weergegeven die vergelijkbaar zijn met de volgende URL's.

URL's voor adaptief streamen:

Smooth Streaming

    http://amstestaccount001.streaming.mediaservices.windows.net/ebf733c4-3e2e-4a68-b67b-cc5159d1d7f2/BigBuckBunny.ism/manifest

HLS

    http://amstestaccount001.streaming.mediaservices.windows.net/ebf733c4-3e2e-4a68-b67b-cc5159d1d7f2/BigBuckBunny.ism/manifest(format=m3u8-aapl)

MPEG DASH

    http://amstestaccount001.streaming.mediaservices.windows.net/ebf733c4-3e2e-4a68-b67b-cc5159d1d7f2/BigBuckBunny.ism/manifest(format=mpd-time-csf)

URL's voor progressief downloaden (audio en video).

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_650kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_400kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_3400kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_2250kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_1500kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_1000kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_AAC_und_ch2_56kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z


Gebruik [Azure Media Services Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html) om uw video te streamen.

Als u het progressief downloaden wilt testen, plakt u een URL in een browser (bijvoorbeeld Internet Explorer, Chrome of Safari).

## <a name="next-steps-media-services-learning-paths"></a>Volgende stappen: Media Services-leertrajecten
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

### <a name="looking-for-something-else"></a>Zoekt u iets anders?
Als dit onderwerp niet de informatie bevat die u verwacht, er iets ontbreekt of het onderwerp op een andere manier niet aan uw behoeften voldoet, kunt u ons via de onderstaande Disqus-thread feedback geven.

<!-- Anchors. -->


<!-- URLs. -->
[Web Platform Installer]: http://go.microsoft.com/fwlink/?linkid=255386
[Portal]: http://manage.windowsazure.com/



<!--HONumber=Dec16_HO2-->


