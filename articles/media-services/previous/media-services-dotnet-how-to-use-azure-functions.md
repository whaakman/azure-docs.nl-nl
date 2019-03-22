---
title: Azure Functions met mediaservices ontwikkelen
description: In dit onderwerp laat zien hoe om te beginnen met het ontwikkelen van Azure Functions met Media Services met behulp van de Azure portal.
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.assetid: 51bdcb01-1846-4e1f-bd90-70020ab471b0
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 02/10/2019
ms.author: juliako
ms.openlocfilehash: 69099fe1369223871142f74d8f6d1c6a8bd66dab
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57851758"
---
# <a name="develop-azure-functions-with-media-services"></a>Azure Functions met mediaservices ontwikkelen

In dit artikel wordt beschreven hoe u aan de slag met het maken van Azure Functions met Media Services. De Azure-functie die is gedefinieerd in dit artikel een container van het opslagaccount met de naam bewaakt **invoer** voor nieuwe MP4-bestanden. Wanneer een bestand in de opslagcontainer is verwijderd, wordt de functie uitgevoerd in de blobtrigger. Azure functions, Zie [overzicht](../../azure-functions/functions-overview.md) en andere onderwerpen in de **Azure functions** sectie.

Als u wilt om te verkennen en implementeren van bestaande Azure-functies die gebruikmaken van Azure Media Services, Bekijk [Media Services, Azure Functions](https://github.com/Azure-Samples/media-services-dotnet-functions-integration). Deze opslagplaats bevat voorbeelden van Media Services gebruiken om werkstromen met betrekking tot het opnemen inhoud rechtstreeks van blob-opslag, codering, en inhoud schrijven terug naar blob-opslag weer te geven. Dit omvat ook voorbeelden over het bewaken van taakmeldingen via WebHooks en Azure-wachtrijen. U kunt ook ontwikkelen van uw functies op basis van de voorbeelden in de [Media Services, Azure Functions](https://github.com/Azure-Samples/media-services-dotnet-functions-integration) opslagplaats. Voor het implementeren van de functies, drukt u op de **implementeren in Azure** knop.

## <a name="prerequisites"></a>Vereisten

- Voordat u uw eerste functie kunt maken, moet u een actief Azure-account hebben. Als u nog geen Azure-account hebt, zijn er [gratis accounts beschikbaar](https://azure.microsoft.com/free/).
- Als u maken van Azure Functions die acties uitvoeren op uw account voor Azure Media Services (AMS) of luisteren naar gebeurtenissen die worden verzonden door Media Services wilt, moet u een AMS-account maken zoals wordt beschreven [hier](media-services-portal-create-account.md).
    
## <a name="create-a-function-app"></a>Een functie-app maken

1. Ga naar de [Azure-portal](https://portal.azure.com) en meld u aan met uw Azure-account.
2. Een functie-app maken zoals wordt beschreven [hier](../../azure-functions/functions-create-function-app-portal.md).

>[!NOTE]
> Een opslagaccount dat u opgeeft in de **StorageConnection** omgevingsvariabele (Zie de volgende stap) moet zich in dezelfde regio als uw app.

## <a name="configure-function-app-settings"></a>Instellingen voor functie-app configureren

Bij het ontwikkelen van Media Services-functies, is het handig om toe te voegen omgevingsvariabelen die worden gebruikt in uw functies. Klik op de koppeling van de App-instellingen configureren voor het configureren van app-instellingen. Zie voor meer informatie, [Azure-functie-app-instellingen configureren](../../azure-functions/functions-how-to-use-azure-function-app-settings.md). 

De functie, gedefinieerd in dit artikel wordt ervan uitgegaan dat u hebt de volgende omgevingsvariabelen in uw app-instellingen:

**AMSAADTenantDomain**: Azure AD-tenant-eindpunt. Zie voor meer informatie over het verbinden met de AMS-API, [dit](media-services-use-aad-auth-to-access-ams-api.md) artikel.

**AMSRESTAPIEndpoint**:  De URI die staat voor de REST API-eindpunt. 

**AMSClientId**: Azure AD-toepassing client-ID.

**AMSClientSecret**: Azure AD-toepassing-clientgeheim.

**StorageConnection**: opslag-verbinding van het account dat is gekoppeld aan de Media Services-account. Deze waarde wordt gebruikt de **function.json** bestand en **run.csx** bestand (Zie hieronder).

## <a name="create-a-function"></a>Een functie maken

Wanneer uw functie-app is geïmplementeerd, kunt u het vinden van **App Services** Azure Functions.

1. Selecteer uw functie-app en klik op **nieuwe functie**.
2. Kies de **C#** taal en **gegevensverwerking** scenario.
3. Kies **BlobTrigger** sjabloon. Deze functie wordt geactiveerd wanneer een blob is geüpload naar de **invoer** container. De **invoer** naam is opgegeven in de **pad**, in de volgende stap.

    ![bestanden weergeven](./media/media-services-azure-functions/media-services-azure-functions004.png)

4. Wanneer u selecteert **BlobTrigger**, sommige meer besturingselementen worden weergegeven op de pagina.

    ![bestanden weergeven](./media/media-services-azure-functions/media-services-azure-functions005.png)

4. Klik op **Create**. 

## <a name="files"></a>Bestanden

Uw Azure-functie is gekoppeld aan de codebestanden en andere bestanden die in deze sectie worden beschreven. Wanneer u de Azure portal gebruiken voor het maken van een functie **function.json** en **run.csx** worden voor u gemaakt. U wilt toevoegen of upload een **project.json** bestand. De rest van deze sectie geeft een korte uitleg van elk bestand en bevat de definities.

![bestanden weergeven](./media/media-services-azure-functions/media-services-azure-functions003.png)

### <a name="functionjson"></a>function.json

Het bestand function.json definieert de functiebindingen en andere configuratie-instellingen. De runtime maakt gebruik van dit bestand om te bepalen welke gebeurtenissen u wilt controleren en het doorgeven van gegevens in en als resultaat de gegevens van een functie wordt uitgevoerd. Zie voor meer informatie, [Azure functions-HTTP- en webhook-bindingen](../../azure-functions/functions-reference.md#function-code).

>[!NOTE]
>Stel de **uitgeschakeld** eigenschap **waar** om te voorkomen dat de functie wordt uitgevoerd. 

Vervang de inhoud van het bestaande function.json-bestand met de volgende code:

```json
{
  "bindings": [
    {
      "name": "myBlob",
      "type": "blobTrigger",
      "direction": "in",
      "path": "input/{filename}.mp4",
      "connection": "ConnectionString"
    }
  ],
  "disabled": false
}
```

### <a name="projectjson"></a>project.json

Het bestand project.json bevat afhankelijkheden. Hier volgt een voorbeeld van **project.json** bestand met de vereiste .NET Azure Media Services-pakketten van Nuget. Houd er rekening mee dat de versienummers met de meest recente updates op de pakketten, wijzigen zodat de meest recente versies dient u te bevestigen. 

De volgende definitie aan project.json toevoegen. 

```json
{
  "frameworks": {
    "net46":{
      "dependencies": {
        "windowsazure.mediaservices": "4.0.0.4",
        "windowsazure.mediaservices.extensions": "4.0.0.4",
        "Microsoft.IdentityModel.Clients.ActiveDirectory": "3.13.1",
        "Microsoft.IdentityModel.Protocol.Extensions": "1.0.2.206221351"
      }
    }
   }
}

```
    
### <a name="runcsx"></a>run.csx

Dit is de C# code voor uw functie.  De functie onder monitors gedefinieerd met een container van het opslagaccount met de naam **invoer** (dat is wat is opgegeven in het pad) voor nieuwe MP4-bestanden. Wanneer een bestand in de opslagcontainer is verwijderd, wordt de functie uitgevoerd in de blobtrigger.
    
Het voorbeeld dat is gedefinieerd in deze sectie ziet u 

1. het opnemen van een asset naar een Media Services-account (door een blob kopiëren naar een AMS-asset) en 
2. hoe naar een codeertaak verzendt die Media Encoder Standard van gebruikt vooraf "Adaptief streamen".

In het scenario voor echte leven wilt u waarschijnlijk taakvoortgang bijhouden en publiceert u de gecodeerde asset. Zie voor meer informatie, [gebruik Azure WebHooks voor het bewaken van taakmeldingen mediaservices](media-services-dotnet-check-job-progress-with-webhooks.md). Zie voor meer voorbeelden van [Media Services, Azure Functions](https://github.com/Azure-Samples/media-services-dotnet-functions-integration).  

Vervang de inhoud van het bestaande bestand run.csx door de volgende code: Wanneer u klaar bent voor het definiëren van uw functie klikt u op **opslaan en uitvoeren**.

```csharp
#r "Microsoft.WindowsAzure.Storage"
#r "Newtonsoft.Json"
#r "System.Web"

using System;
using System.Net;
using System.Net.Http;
using Newtonsoft.Json;
using Microsoft.WindowsAzure.MediaServices.Client;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading;
using System.Threading.Tasks;
using System.IO;
using System.Web;
using Microsoft.Azure;
using Microsoft.WindowsAzure.Storage;
using Microsoft.WindowsAzure.Storage.Blob;
using Microsoft.WindowsAzure.Storage.Auth;
using Microsoft.Azure.WebJobs;
using Microsoft.IdentityModel.Clients.ActiveDirectory;
  
// Read values from the App.config file.

static readonly string _AADTenantDomain = Environment.GetEnvironmentVariable("AMSAADTenantDomain");
static readonly string _RESTAPIEndpoint = Environment.GetEnvironmentVariable("AMSRESTAPIEndpoint");
 
static readonly string _mediaservicesClientId = Environment.GetEnvironmentVariable("AMSClientId");
static readonly string _mediaservicesClientSecret = Environment.GetEnvironmentVariable("AMSClientSecret");

static readonly string _connectionString = Environment.GetEnvironmentVariable("ConnectionString");  

private static CloudMediaContext _context = null;
private static CloudStorageAccount _destinationStorageAccount = null;

public static void Run(CloudBlockBlob myBlob, string fileName, TraceWriter log)
{
    // NOTE that the variables {fileName} here come from the path setting in function.json
    // and are passed into the  Run method signature above. We can use this to make decisions on what type of file
    // was dropped into the input container for the function. 

    // No need to do any Retry strategy in this function, By default, the SDK calls a function up to 5 times for a 
    // given blob. If the fifth try fails, the SDK adds a message to a queue named webjobs-blobtrigger-poison.

    log.Info($"C# Blob trigger function processed: {fileName}.mp4");
    log.Info($"Media Services REST endpoint : {_RESTAPIEndpoint}");

    try
    {
        AzureAdTokenCredentials tokenCredentials = new AzureAdTokenCredentials(_AADTenantDomain,
                            new AzureAdClientSymmetricKey(_mediaservicesClientId, _mediaservicesClientSecret),
                            AzureEnvironments.AzureCloudEnvironment);
 
        AzureAdTokenProvider tokenProvider = new AzureAdTokenProvider(tokenCredentials);
 
        _context = new CloudMediaContext(new Uri(_RESTAPIEndpoint), tokenProvider);

        IAsset newAsset = CreateAssetFromBlob(myBlob, fileName, log).GetAwaiter().GetResult();

        // Step 2: Create an Encoding Job

        // Declare a new encoding job with the Standard encoder
        IJob job = _context.Jobs.Create("Azure Function - MES Job");

        // Get a media processor reference, and pass to it the name of the 
        // processor to use for the specific task.
        IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Standard");

        // Create a task with the encoding details, using a custom preset
        ITask task = job.Tasks.AddNew("Encode with Adaptive Streaming",
            processor,
            "Adaptive Streaming",
            TaskOptions.None); 

        // Specify the input asset to be encoded.
        task.InputAssets.Add(newAsset);

        // Add an output asset to contain the results of the job. 
        // This output is specified as AssetCreationOptions.None, which 
        // means the output asset is not encrypted. 
        task.OutputAssets.AddNew(fileName, AssetCreationOptions.None);

        job.Submit();
        log.Info("Job Submitted");

    }
    catch (Exception ex)
    {
        log.Error("ERROR: failed.");
        log.Info($"StackTrace : {ex.StackTrace}");
        throw ex;
    }
}

private static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
{
    var processor = _context.MediaProcessors.Where(p => p.Name == mediaProcessorName).
    ToList().OrderBy(p => new Version(p.Version)).LastOrDefault();

    if (processor == null)
    throw new ArgumentException(string.Format("Unknown media processor", mediaProcessorName));

    return processor;
}

public static async Task<IAsset> CreateAssetFromBlob(CloudBlockBlob blob, string assetName, TraceWriter log){
    IAsset newAsset = null;

    try{
        Task<IAsset> copyAssetTask = CreateAssetFromBlobAsync(blob, assetName, log);
        newAsset = await copyAssetTask;
        log.Info($"Asset Copied : {newAsset.Id}");
    }
    catch(Exception ex){
        log.Info("Copy Failed");
        log.Info($"ERROR : {ex.Message}");
        throw ex;
    }

    return newAsset;
}

/// <summary>
/// Creates a new asset and copies blobs from the specifed storage account.
/// </summary>
/// <param name="blob">The specified blob.</param>
/// <returns>The new asset.</returns>
public static async Task<IAsset> CreateAssetFromBlobAsync(CloudBlockBlob blob, string assetName, TraceWriter log)
{
     //Get a reference to the storage account that is associated with the Media Services account. 
    _destinationStorageAccount = CloudStorageAccount.Parse(_connectionString);

    // Create a new asset. 
    var asset = _context.Assets.Create(blob.Name, AssetCreationOptions.None);
    log.Info($"Created new asset {asset.Name}");

    IAccessPolicy writePolicy = _context.AccessPolicies.Create("writePolicy",
    TimeSpan.FromHours(4), AccessPermissions.Write);
    ILocator destinationLocator = _context.Locators.CreateLocator(LocatorType.Sas, asset, writePolicy);
    CloudBlobClient destBlobStorage = _destinationStorageAccount.CreateCloudBlobClient();

    // Get the destination asset container reference
    string destinationContainerName = (new Uri(destinationLocator.Path)).Segments[1];
    CloudBlobContainer assetContainer = destBlobStorage.GetContainerReference(destinationContainerName);

    try{
    assetContainer.CreateIfNotExists();
    }
    catch (Exception ex)
    {
    log.Error ("ERROR:" + ex.Message);
    }

    log.Info("Created asset.");

    // Get hold of the destination blob
    CloudBlockBlob destinationBlob = assetContainer.GetBlockBlobReference(blob.Name);

    // Copy Blob
    try
    {
    using (var stream = await blob.OpenReadAsync()) 
    {            
        await destinationBlob.UploadFromStreamAsync(stream);          
    }

    log.Info("Copy Complete.");

    var assetFile = asset.AssetFiles.Create(blob.Name);
    assetFile.ContentFileSize = blob.Properties.Length;
    assetFile.IsPrimary = true;
    assetFile.Update();
    asset.Update();
    }
    catch (Exception ex)
    {
    log.Error(ex.Message);
    log.Info (ex.StackTrace);
    log.Info ("Copy Failed.");
    throw;
    }

    destinationLocator.Delete();
    writePolicy.Delete();

    return asset;
}
```

## <a name="test-your-function"></a>Uw functie testen

Om de functie testen, moet u voor het uploaden van een MP4-bestand in de **invoer** container van het opslagaccount dat u hebt opgegeven in de verbindingsreeks.  

1. Selecteer het opslagaccount dat u hebt opgegeven in de **StorageConnection** omgevingsvariabele.
2. Klik op **Blobs**.
3. Klik op **+ Container**. Naam van de container **invoer**.
4. Druk op **uploaden** en blader naar een MP4-bestand dat u wilt uploaden.

>[!NOTE]
> Wanneer u een blobtrigger in een verbruiksabonnement gebruikt, kunnen er maximaal 10 minuten duren in de verwerking van nieuwe blobs nadat een functie-app niet actief is geworden. Nadat de functie-app wordt uitgevoerd, worden onmiddellijk blobs verwerkt. Zie voor meer informatie, [Blob opslagtriggers en bindingen](https://docs.microsoft.com/azure/azure-functions/functions-bindings-storage-blob).

## <a name="next-steps"></a>Volgende stappen

Op dit moment bent u klaar om te beginnen met het ontwikkelen van een Media Services-toepassing. 
 
Zie voor meer informatie en volledige samples/oplossingen van het gebruik van Azure Functions en Logic Apps met Azure Media Services voor het maken van aangepaste inhoud maken werkstromen, de [Media Services .NET-functies integratie voorbeeld op GitHub](https://github.com/Azure-Samples/media-services-dotnet-functions-integration)

Zie ook [gebruik Azure WebHooks voor het bewaken van taakmeldingen Media Services met .NET](media-services-dotnet-check-job-progress-with-webhooks.md). 

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

