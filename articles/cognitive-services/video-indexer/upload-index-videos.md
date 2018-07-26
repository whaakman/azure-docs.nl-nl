---
title: Indexeren van uw video's met Azure Video Indexer | Microsoft Docs
description: In dit onderwerp ziet u hoe u API's gebruiken om te uploaden en uw video's met Azure Video Indexer indexeren
services: cognitive services
documentationcenter: ''
author: juliako
manager: erikre
ms.service: cognitive-services
ms.topic: article
ms.date: 05/30/2018
ms.author: juliako
ms.openlocfilehash: 64c4c8e4df0179f1644f23f0ae489015222a4ffd
ms.sourcegitcommit: c2c64fc9c24a1f7bd7c6c91be4ba9d64b1543231
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/26/2018
ms.locfileid: "39258361"
---
# <a name="upload-and-index-your-videos"></a>Indexeren van uw video 's  

In dit artikel ziet u hoe u de [video uploaden](https://api-portal.videoindexer.ai/docs/services/operations/operations/Upload-video?) API voor het uploaden en indexeren van uw video's met Video Indexer van Azure. Hierin worden ook enkele van de parameters die u op de API instellen kunt te wijzigen van het proces en de uitvoer van de API.

## <a name="configurations-and-params"></a>Configuraties en parameters

Deze sectie beschrijft een aantal van de volgende optionele parameters en wanneer wilt u deze instelt.

### <a name="externalid"></a>externalID 

Deze parameter kunt u opgeven van een ID die gekoppeld aan de video is. De ID kan worden toegepast op externe "Video Content Management' (VCM) systeemintegratie. De video's die zich in de Video Indexer-portal kunnen worden doorzocht met behulp van de opgegeven externe ID.

### <a name="indexingpreset"></a>indexingPreset

Gebruik deze parameter als onbewerkte of externe-opnamen achtergrondgeluiden bevatten. Deze parameter wordt gebruikt voor het configureren van het indexeringsproces worden geautomatiseerd. U kunt de volgende waarden opgeven:

- `Default` – Te indexeren en extraheren van inzichten met behulp van zowel audio en video
- `AudioOnly` – Te indexeren en extraheren van inzichten met behulp van audio alleen (wordt genegeerd video)
- `DefaultWithNoiseReduction` – Index en het inzicht van zowel audio en video, tijdens het toepassen van ruis vermindering algoritmen op audiostream

Prijs is afhankelijk van de geselecteerde optie voor indexering.  

### <a name="callbackurl"></a>callbackUrl

Een POST-URL om te waarschuwen wanneer indexeren is voltooid. Video Indexer voegt twee Reeksparameters naar deze query:-id en status. Bijvoorbeeld, als de callback-url is 'https://test.com/notifyme?projectName=MyProject', de melding wordt verzonden met extra parameters voor'https://test.com/notifyme?projectName=MyProject&id=1234abcd&state=Processed'.

U kunt ook meer parameters toevoegen aan de URL voordat u de aanroep van Video Indexer en deze parameters worden opgenomen in de callback. Later in uw code kunt u ophalen en parseren van de query-tekenreeks back alle van de opgegeven parameters in de queryreeks (gegevens die u oorspronkelijk had toegevoegd aan de URL plus de gegevens van de Video Indexer opgegeven.) 

### <a name="streamingpereset"></a>streamingPereset

Zodra uw video is geüpload, codeert Video Indexer, eventueel de video. Vervolgens gaat u door naar het indexeren en analyseren van de video. Wanneer Video Indexer is klaar analyseren, ontvangt u een melding met de video-ID.  

Wanneer u de [video uploaden](https://api-portal.videoindexer.ai/docs/services/operations/operations/Upload-video?) of [Video opnieuw indexeren](https://api-portal.videoindexer.ai/docs/services/operations/operations/Re-index-video?) API, een van de volgende optionele parameters is `streamingPreset`. Als u instelt `streamingPereset` naar `Default`, `SingleBitrate`, of `AdaptiveBitrate`, het coderingsproces wordt geactiveerd. Wanneer het indexeren en coderingstaken klaar bent, wordt de video is gepubliceerd, zodat u kunt ook uw video streamen. De Streaming-eindpunt van waaruit u wilt de video te streamen, moet zich in de **met** staat.

Om uit te voeren van het indexeren en coderingstaken, de [Azure Media Services-account verbonden met uw Video Indexer-account](connect-to-azure.md), gereserveerde eenheden is vereist. Zie voor meer informatie, [Media-verwerking schalen](https://docs.microsoft.com/azure/media-services/previous/media-services-scale-media-processing-overview). Aangezien deze intensieve rekentaken, wordt S3-eenheidstype aanbevolen. Het aantal ru's definieert het maximale aantal taken dat parallel kan worden uitgevoerd. De aanbeveling van de basislijn is 10 S3-ru's. 

Als u alleen wilt uw video kunt indexeren, maar deze niet te coderen, stelt u `streamingPereset`naar `NoStreaming`.

## <a name="code-sample"></a>Codevoorbeeld

De volgende C#-codefragment toont het gebruik van alle de Video Indexer-API's samen.

```csharp
public async Task Sample()
{
    var apiUrl = "https://api.videoindexer.ai";
    var location = "westus2";
    var apiKey = "...";

    System.Net.ServicePointManager.SecurityProtocol =
        System.Net.ServicePointManager.SecurityProtocol | System.Net.SecurityProtocolType.Tls12;

    // create the http client
    var handler = new HttpClientHandler();
    handler.AllowAutoRedirect = false;
    var client = new HttpClient(handler);
    client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", apiKey);

    // obtain account information and access token
    string queryParams = CreateQueryString(
        new Dictionary<string, string>()
        {
            {"generateAccessTokens", "true"},
            {"allowEdit", "true"},
        });
    HttpResponseMessage result = await client.GetAsync($"{apiUrl}/auth/trial/Accounts?{queryParams}");
    var json = await result.Content.ReadAsStringAsync();
    var accounts = JsonConvert.DeserializeObject<AccountContractSlim[]>(json);
    // take the relevant account, here we simply take the first
    var accountInfo = accounts.First();

    // we will use the access token from here on, no need for the apim key
    client.DefaultRequestHeaders.Remove("Ocp-Apim-Subscription-Key");

    // upload a video
    var content = new MultipartFormDataContent();
    Debug.WriteLine("Uploading...");
    // get the video from URL
    var videoUrl = "VIDEO_URL"; // replace with the video URL

    // as an alternative to specifying video URL, you can upload a file.
    // remove the videoUrl parameter from the query params below and add the following lines:
    //FileStream video =File.OpenRead(Globals.VIDEOFILE_PATH);
    //byte[] buffer =newbyte[video.Length];
    //video.Read(buffer, 0, buffer.Length);
    //content.Add(newByteArrayContent(buffer));

    queryParams = CreateQueryString(
        new Dictionary<string, string>()
        {
            {"accessToken", accountInfo.AccessToken},
            {"name", "video_name"},
            {"description", "video_description"},
            {"privacy", "private"},
            {"partition", "partition"},
            {"videoUrl", videoUrl},
        });
    var uploadRequestResult = await client.PostAsync($"{apiUrl}/{accountInfo.Location}/Accounts/{accountInfo.Id}/Videos?{queryParams}", content);
    var uploadResult = await uploadRequestResult.Content.ReadAsStringAsync();

    // get the video id from the upload result
    string videoId = JsonConvert.DeserializeObject<dynamic>(uploadResult)["id"];
    Debug.WriteLine("Uploaded");
    Debug.WriteLine("Video ID:");
    Debug.WriteLine(videoId);

    // wait for the video index to finish
    while (true)
    {
        await Task.Delay(10000);

        queryParams = CreateQueryString(
            new Dictionary<string, string>()
            {
                {"accessToken", accountInfo.AccessToken},
                {"language", "English"},
            });

        var videoGetIndexRequestResult = await client.GetAsync($"{apiUrl}/{accountInfo.Location}/Accounts/{accountInfo.Id}/Videos/{videoId}/Index?{queryParams}");
        var videoGetIndexResult = await videoGetIndexRequestResult.Content.ReadAsStringAsync();

        string processingState = JsonConvert.DeserializeObject<dynamic>(videoGetIndexResult)["state"];

        Debug.WriteLine("");
        Debug.WriteLine("State:");
        Debug.WriteLine(processingState);

        // job is finished
        if (processingState != "Uploaded" && processingState != "Processing")
        {
            Debug.WriteLine("");
            Debug.WriteLine("Full JSON:");
            Debug.WriteLine(videoGetIndexResult);
            break;
        }
    }

    // search for the video
    queryParams = CreateQueryString(
        new Dictionary<string, string>()
        {
            {"accessToken", accountInfo.AccessToken},
            {"id", videoId},
        });

    var searchRequestResult = await client.GetAsync($"{apiUrl}/{accountInfo.Location}/Accounts/{accountInfo.Id}/Videos/Search?{queryParams}");
    var searchResult = await searchRequestResult.Content.ReadAsStringAsync();
    Debug.WriteLine("");
    Debug.WriteLine("Search:");
    Debug.WriteLine(searchResult);

    // Generate video access token (used for get widget calls)
    client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", apiKey);
    var videoTokenRequestResult = await client.GetAsync($"{apiUrl}/auth/{accountInfo.Location}/Accounts/{accountInfo.Id}/Videos/{videoId}/AccessToken?allowEdit=true");
    var videoAccessToken = (await videoTokenRequestResult.Content.ReadAsStringAsync()).Replace("\"", "");
    client.DefaultRequestHeaders.Remove("Ocp-Apim-Subscription-Key");

    // get insights widget url
    queryParams = CreateQueryString(
        new Dictionary<string, string>()
        {
            {"accessToken", videoAccessToken},
            {"widgetType", "Keywords"},
            {"allowEdit", "true"},
        });
    var insightsWidgetRequestResult = await client.GetAsync($"{apiUrl}/{accountInfo.Location}/Accounts/{accountInfo.Id}/Videos/{videoId}/InsightsWidget?{queryParams}");
    var insightsWidgetLink = insightsWidgetRequestResult.Headers.Location;
    Debug.WriteLine("Insights Widget url:");
    Debug.WriteLine(insightsWidgetLink);

    // get player widget url
    queryParams = CreateQueryString(
        new Dictionary<string, string>()
        {
            {"accessToken", videoAccessToken},
        });
    var playerWidgetRequestResult = await client.GetAsync($"{apiUrl}/{accountInfo.Location}/Accounts/{accountInfo.Id}/Videos/{videoId}/PlayerWidget?{queryParams}");
    var playerWidgetLink = playerWidgetRequestResult.Headers.Location;
    Debug.WriteLine("");
    Debug.WriteLine("Player Widget url:");
    Debug.WriteLine(playerWidgetLink);
}

private string CreateQueryString(IDictionary<string, string> parameters)
{
    var queryParameters = HttpUtility.ParseQueryString(string.Empty);
    foreach (var parameter in parameters)
    {
        queryParameters[parameter.Key] = parameter.Value;
    }

    return queryParameters.ToString();
}

public class AccountContractSlim
{
    public Guid Id { get; set; }
    public string Name { get; set; }
    public string Location { get; set; }
    public string AccountType { get; set; }
    public string Url { get; set; }
    public string AccessToken { get; set; }
}
```



## <a name="next-steps"></a>Volgende stappen

[Bekijk de Azure-Video Indexer-uitvoer geproduceerd door de v2-API](video-indexer-output-json-v2.md)
