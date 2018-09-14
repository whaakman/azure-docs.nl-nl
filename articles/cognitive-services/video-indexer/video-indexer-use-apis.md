---
title: Gebruik Azure Video Indexer-API | Microsoft Docs
description: Dit artikel leest hoe u aan de slag met behulp van de Video Indexer-API.
services: cognitive services
documentationcenter: ''
author: juliako
manager: erikre
ms.service: cognitive-services
ms.topic: article
ms.date: 09/09/2018
ms.author: juliako
ms.openlocfilehash: bd0f9e01257bec2d39ef0c8e13b68c4a7a13637d
ms.sourcegitcommit: f983187566d165bc8540fdec5650edcc51a6350a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/13/2018
ms.locfileid: "45542919"
---
# <a name="use-azure-video-indexer-api"></a>Gebruik Azure Video Indexer-API

> [!Note]
> De Video Indexer-API V1 is op 1 augustus 2018 afgeschaft. U moet nu de v2-API voor Video Indexer gebruiken. <br/>Als u wilt ontwikkelen met Video Indexer v2-API's, Zie de instructies [hier](https://api-portal.videoindexer.ai/). 

Video Indexer consolideert verschillende audio en video kunstmatige intelligentie (AI)-technologieën die worden aangeboden door Microsoft in één geïntegreerde service, wat het ontwikkelen eenvoudiger. De API's zijn ontworpen voor ontwikkelaars zich richten op de Media AI-technologieën gebruiken zonder u zorgen te maken over schalen, globale bereiken inschakelen, beschikbaarheid en betrouwbaarheid van cloud-platform. De API kunt u uw bestanden uploaden, ophalen, gedetailleerde inzichten in video's, URL's van inzicht en de ingesloten speler widgets ophalen als u wilt deze insluiten in uw toepassing en andere taken.

Als u een Video Indexer-account maakt, kunt u een gratis proefaccount (waar u aan een bepaald aantal gratis indexering minuten) of een betaalde optie (waarbij u bent niet beperkt door het quotum). Met gratis proefversie Video Indexer biedt maximaal 600 minuten gratis worden geïndexeerd voor gebruikers van de website en maximaal 2400 minuten gratis indexing voor een API-gebruikers. Betaalde optie, maakt u een Video Indexer-account dat is [verbonden met uw Azure-abonnement en een Azure Media Services-account](connect-to-azure.md). U betaalt voor de minuten geïndexeerd, evenals de Azure Media Services account gerelateerde kosten in rekening gebracht. 

Dit artikel wordt beschreven hoe de ontwikkelaars kunnen profiteren van de [Video Indexer-API](https://api-portal.videoindexer.ai/).

## <a name="subscribe-to-the-api"></a>Abonneer u op de API

1. Aanmelden bij [Video Indexer-Ontwikkelaarsportal](https://api-portal.videoindexer.ai/).
    
    ![Aanmelden](./media/video-indexer-use-apis/video-indexer-api01.png)

    > [!Important]
    > * U moet de dezelfde provider die u hebt gebruikt toen u zich hebt geregistreerd voor Video Indexer gebruiken.
    > * Persoonlijke Google en Microsoft (outlook/live) accounts kunnen alleen worden gebruikt voor proefaccounts. Accounts die zijn verbonden met Azure vereist Azure AD.
    > * Er mag slechts één actieve account per E-Mail. Als een gebruiker probeert om aan te melden met user@gmail.com voor LinkedIn en daarna met user@gmail.com voor Google de later een foutpagina weergeven, dat de gebruiker al bestaat.

2. Zich abonneren.

    Selecteer de [producten](https://api-portal.videoindexer.ai/products) tabblad. Vervolgens selecteert u autorisatie en abonneren. 
    
    ![Aanmelden](./media/video-indexer-use-apis/video-indexer-api02.png)

    > [!NOTE]
    > Nieuwe gebruikers worden automatisch geabonneerd op autorisatie.
    
    Zodra u zich hebt geabonneerd, kunt u zich om uw abonnement en de primaire en secundaire sleutels te bekijken. De sleutels moeten worden beveiligd. De sleutels mag alleen worden gebruikt door de servercode. Deze mag niet zijn beschikbaar op de client (.js, .html, enz.).

    ![Aanmelden](./media/video-indexer-use-apis/video-indexer-api03.png)

## <a name="obtain-access-token-using-the-authorization-api"></a>Met behulp van de autorisatie-API-toegangstoken verkrijgen

Zodra u geabonneerd op de autorisatie-API, kunt u zich aan toegangstokens verkrijgen. Deze toegangstokens worden gebruikt voor verificatie op basis van de Operations-API. 

Elke aanroep naar de Operations-API moet worden gekoppeld aan een toegangstoken die overeenkomt met het bereik van de autorisatie van de aanroep.

- Het gebruikersniveau van de - toegangsniveau gebruikerstokens kunnen u bewerkingen uitvoeren op de **gebruiker** niveau. Bijvoorbeeld: get-gekoppelde accounts.
- Accountniveau – account toegangsniveau van de tokens kunnen u bewerkingen uitvoeren op de **account** niveau of de **video** niveau. Bijvoorbeeld video uploaden, lijst met alle video's, krijg inzichten in video's, enzovoort.
- Videoniveau – toegangsniveau van de video-tokens kunnen u bewerkingen uitvoeren op een specifieke **video**. Bijvoorbeeld: get inzichten in video's, ondertiteling downloaden, krijg widgets, enzovoort. 

U kunt bepalen of deze tokens alleen-lezen zijn of ze bewerken door op te geven toestaan **bewerken = true/false**.

Voor de meeste scenario's voor server-naar-server, wordt u waarschijnlijk dezelfde gebruiken **account** token omdat hierin beide **account** operations en **video** bewerkingen. Echter, als u van plan bent om te maken van de client-side-aanroepen met Video Indexer (bijvoorbeeld van javascript), u wilt gebruiken een **video** toegangstoken om te voorkomen dat clients de toegang tot het gehele account. Dat is ook de reden dat wanneer VideoIndexer clientcode insluiten in uw client (bijvoorbeeld met behulp van **ophalen Insights Widget** of **Player Widget ophalen**) moet u een **video**toegangstoken.

Dingen om eenvoudiger te maken, kunt u de **autorisatie** API > **GetAccounts** ophalen van uw accounts zonder het verkrijgen van een gebruiker token eerst. U kunt ook vragen om op te halen van de accounts met behulp van geldige tokens, zodat u kunt het overslaan van een extra aanroep van een account-token ophalen.

Toegangstokens verlopen na 1 uur. Zorg ervoor dat uw token geldig is voordat u met behulp van de Operations-API. Als is verlopen, de autorisatie-API voor het ophalen van een nieuw toegangstoken aanroepen.
 
U bent klaar om te integreren met de API. Zoek [de gedetailleerde beschrijving van elke Video Indexer REST-API](http://api-portal.videoindexer.ai/).

## <a name="location"></a>Locatie

Alle bewerking API's vereist een locatieparameter, waarmee wordt aangegeven van de regio die de aanroep moet worden gestuurd en in het account is gemaakt.

De waarden die worden beschreven in de volgende tabel zijn van toepassing. De **Param waarde** is de waarde die u als doorgeven met behulp van de API.

|**Naam**|**Waarde van parameter**|**Beschrijving**|
|---|---|---|
|Proefversie|proefversie|Voor proefaccounts gebruikt.|
|US - west|westus2|Gebruikt voor de regio van Azure VS-West 2.|
|Europa - noord |northeurope|Voor de regio Noord-Europa Azure gebruikt.|
|Azië - oost|eastasia|Gebruikt voor de regio Oost-Azië van Azure.|

## <a name="account-id"></a>Account-id 

De Account-ID-parameter is vereist in alle operationele API-aanroepen. Account-ID is een GUID die kan worden verkregen in een van de volgende manieren:

* Gebruik de **Video Indexer-website** om op te halen van de Account-ID:

    1. Blader naar de [Video Indexer](https://www.videoindexer.ai/) website en meld u aan.
    2. Blader naar de **instellingen** pagina.
    3. Kopieer de account-ID.

        ![Account-id](./media/video-indexer-use-apis/account-id.png)

* Gebruik **Video Indexer-Ontwikkelaarsportal** om op te halen via een programma de Account-ID.

    Gebruik de [accounts ophalen](https://api-portal.videoindexer.ai/docs/services/authorization/operations/Get-Accounts?) API.
    
    > [!TIP]
    > U kunt de toegangstokens voor de accounts genereren door te definiëren `generateAccessTokens=true`.
    
* Haal de account-ID van de URL van een pagina van de speler in uw account.

    Wanneer u een video bekijkt, de ID wordt weergegeven na de `accounts` sectie en voordat de `videos` sectie.

    ```
    https://www.videoindexer.ai/accounts/00000000-f324-4385-b142-f77dacb0a368/videos/d45bf160b5/
    ```

## <a name="recommendations"></a>Aanbevelingen

Deze sectie vindt u enkele aanbevelingen bij het gebruik van Video Indexer-API.

- Als u van plan bent een video kunt uploaden, is het raadzaam om het bestand in een openbaar netwerklocatie (bijvoorbeeld OneDrive). Haal de koppeling naar de video en geef de URL als de parameter van de bestand uploaden. 

    De URL die is opgegeven met Video Indexer moet verwijzen naar een media (audio of video)-bestand. Sommige van de koppelingen die worden gegenereerd door OneDrive zijn voor een HTML-pagina die het bestand bevat. Een eenvoudige verificatie voor de URL worden kan te plakken in een browser – als het bestand begonnen met het downloaden, is het waarschijnlijk een goede URL. Als de browser wordt gerenderd enkele visualisatie, is het waarschijnlijk niet een koppeling naar een bestand, maar een HTML-pagina.
    
- Als u de API die inzichten in video's opgehaald voor de opgegeven video aanroepen, krijgt u een gedetailleerde JSON-uitvoer als de inhoud van de reactie. [Informatie over de geretourneerde JSON in dit onderwerp](video-indexer-output-json.md).

## <a name="code-sample"></a>Codevoorbeeld

De volgende C#-codefragment toont het gebruik van alle de Video Indexer-API's samen.

```csharp
var apiUrl = "https://api.videoindexer.ai";
var accountId = "..."; 
var location = "westus2";
var apiKey = "..."; 

System.Net.ServicePointManager.SecurityProtocol = System.Net.ServicePointManager.SecurityProtocol | System.Net.SecurityProtocolType.Tls12;

// create the http client
var handler = new HttpClientHandler(); 
handler.AllowAutoRedirect = false; 
var client = new HttpClient(handler);
client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", apiKey); 

// obtain account access token
var accountAccessTokenRequestResult = client.GetAsync($"{apiUrl}/auth/{location}/Accounts/{accountId}/AccessToken?allowEdit=true").Result;
var accountAccessToken = accountAccessTokenRequestResult.Content.ReadAsStringAsync().Result.Replace("\"", "");

client.DefaultRequestHeaders.Remove("Ocp-Apim-Subscription-Key");

// upload a video
var content = new MultipartFormDataContent();
Debug.WriteLine("Uploading...");
// get the video from URL
var videoUrl = "VIDEO_URL"; // replace with the video URL

// as an alternative to specifying video URL, you can upload a file.
// remove the videoUrl parameter from the query string below and add the following lines:
  //FileStream video =File.OpenRead(Globals.VIDEOFILE_PATH);
  //byte[] buffer =newbyte[video.Length];
  //video.Read(buffer, 0, buffer.Length);
  //content.Add(newByteArrayContent(buffer));

var uploadRequestResult = client.PostAsync($"{apiUrl}/{location}/Accounts/{accountId}/Videos?accessToken={accountAccessToken}&name=some_name&description=some_description&privacy=private&partition=some_partition&videoUrl={videoUrl}", content).Result;
var uploadResult = uploadRequestResult.Content.ReadAsStringAsync().Result;

// get the video id from the upload result
var videoId = JsonConvert.DeserializeObject<dynamic>(uploadResult)["id"];
Debug.WriteLine("Uploaded");
Debug.WriteLine("Video ID: " + videoId);           

// obtain video access token            
client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", apiKey);
var videoTokenRequestResult = client.GetAsync($"{apiUrl}/auth/{location}/Accounts/{accountId}/Videos/{videoId}/AccessToken?allowEdit=true").Result;
var videoAccessToken = videoTokenRequestResult.Content.ReadAsStringAsync().Result.Replace("\"", "");

client.DefaultRequestHeaders.Remove("Ocp-Apim-Subscription-Key");

// wait for the video index to finish
while (true)
{
  Thread.Sleep(10000);

  var videoGetIndexRequestResult = client.GetAsync($"{apiUrl}/{location}/Accounts/{accountId}/Videos/{videoId}/Index?accessToken={videoAccessToken}&language=English").Result;
  var videoGetIndexResult = videoGetIndexRequestResult.Content.ReadAsStringAsync().Result;

  var processingState = JsonConvert.DeserializeObject<dynamic>(videoGetIndexResult)["state"];

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
var searchRequestResult = client.GetAsync($"{apiUrl}/{location}/Accounts/{accountId}/Videos/Search?accessToken={accountAccessToken}&id={videoId}").Result;
var searchResult = searchRequestResult.Content.ReadAsStringAsync().Result;
Debug.WriteLine("");
Debug.WriteLine("Search:");
Debug.WriteLine(searchResult);

// get insights widget url
var insightsWidgetRequestResult = client.GetAsync($"{apiUrl}/{location}/Accounts/{accountId}/Videos/{videoId}/InsightsWidget?accessToken={videoAccessToken}&widgetType=Keywords&allowEdit=true").Result;
var insightsWidgetLink = insightsWidgetRequestResult.Headers.Location;
Debug.WriteLine("Insights Widget url:");
Debug.WriteLine(insightsWidgetLink);

// get player widget url
var playerWidgetRequestResult = client.GetAsync($"{apiUrl}/{location}/Accounts/{accountId}/Videos/{videoId}/PlayerWidget?accessToken={videoAccessToken}").Result;
var playerWidgetLink = playerWidgetRequestResult.Headers.Location;
Debug.WriteLine("");
Debug.WriteLine("Player Widget url:");
Debug.WriteLine(playerWidgetLink);

```

## <a name="next-steps"></a>Volgende stappen

[Details van de JSON-uitvoer onderzoeken](video-indexer-output-json.md).

[Video Indexer-overzicht](video-indexer-overview.md)
