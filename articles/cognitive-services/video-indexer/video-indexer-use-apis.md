---
title: Gebruik Azure Video indexeerfunctie API | Microsoft Docs
description: Dit artikel laat zien hoe u aan de slag met de API van de indexeerfunctie Video.
services: cognitive services
documentationcenter: ''
author: juliako
manager: erikre
ms.service: cognitive-services
ms.topic: article
ms.date: 06/04/2018
ms.author: juliako
ms.openlocfilehash: d378934a0c085910475c366f4bdb538f09efc12b
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/11/2018
ms.locfileid: "35350105"
---
# <a name="use-azure-video-indexer-api"></a>Gebruik Azure Video indexeerfunctie API

Video indexeerfunctie samengevoegd verschillende audio en video kunstmatige intelligence (AI)-technologieën die worden aangeboden door Microsoft in één geïntegreerde service, ontwikkeling eenvoudiger. De API's zijn ontworpen om ontwikkelaars zich te concentreren op de Media AI technologieën gebruiken zonder dat u over schalen, globale bereiken inschakelen, beschikbaarheid en betrouwbaarheid van cloud-platform. De API kunt u uw bestanden uploaden, gedetailleerde video inzichten verkrijgen, URL's van inzicht en player widgets ophalen om ze te insluiten in uw toepassing en andere taken.

> [!Note]
> De gratis proefversie heeft een dagelijkse uploadlimiet van 100 bestanden. U kunt het foutbericht geretourneerd voor meer informatie kunt controleren. Houd er rekening mee dat de dagelijkse limiet kan worden gewijzigd.

Dit artikel laat zien hoe de ontwikkelaars kunnen profiteren van de [Video indexeerfunctie API](https://api-portal.videoindexer.ai/). Vindt u een meer gedetailleerd overzicht van de indexeerfunctie Video-service de [overzicht](video-indexer-overview.md) artikel.

## <a name="subscribe-to-the-api"></a>Abonneren op de API

1. Aanmelden.

    Als u wilt ontwikkelen met Video indexeerfunctie, moet u eerst aanmelden bij de [Video indexeerfunctie](https://api-portal.videoindexer.ai/) portal. 
    
    ![Aanmelden](./media/video-indexer-use-apis/video-indexer-api01.png)

    > [!Important]
    > 1. U moet dezelfde provider die u hebt gebruikt toen u zich registreerde voor Video indexeerfunctie gebruiken.
    > 2. Voordat u Azure AD-gebruikers van een domein kunnen aanmelden, moet de domeinbeheerder AAD dat domeinregistratie inschakelen [hier](https://api-portal.videoindexer.ai/aadadminconsent).
    > 3. Persoonlijke Google en Microsoft (outlook/live) accounts kunnen alleen worden gebruikt voor proefaccounts. Accounts die zijn verbonden met Azure vereisen AAD.

2. Zich abonneren.

    Selecteer de [producten](https://api-portal.videoindexer.ai/products) tabblad. Vervolgens selecteert u autorisatie en zich abonneren. 
    
    ![Aanmelden](./media/video-indexer-use-apis/video-indexer-api02.png)
    
    Zodra u zich hebt geabonneerd, kunt u zich kunt zien van uw abonnement en de primaire en secundaire sleutels. De sleutels moeten worden beveiligd. De sleutels mag alleen worden gebruikt door uw servercode. Zij mag niet aan de clientzijde (.js, .html, enzovoort) beschikbaar zijn.

    ![Aanmelden](./media/video-indexer-use-apis/video-indexer-api03.png)

## <a name="obtain-access-token-using-the-authorization-api"></a>Met de autorisatie-API toegangstoken verkrijgen

Zodra u de autorisatie-API hebt geabonneerd, kunt u zich kunnen toegangstokens te verkrijgen. Deze toegangstokens worden gebruikt voor verificatie op basis van de Operations-API. 

Elke aanroep van de Operations-API moet worden gekoppeld aan een toegangstoken die overeenkomt met het bereik van de autorisatie van de aanroep.

- Gebruikersniveau - gebruikerstokens toegangsniveau kunnen u bewerkingen uitvoeren op de **gebruiker** niveau. Bijvoorbeeld get gekoppelde accounts.
- Niveau – account toegangsniveau tokens kunnen u bewerkingen uitvoeren op de **account** niveau of de **video** niveau. Bijvoorbeeld video uploaden, lijst met alle video's, ophalen video inzichten, enzovoort.
- Video niveau – toegangsniveau van de video-tokens kunnen u bewerkingen uitvoeren op een specifieke **video**. Bijvoorbeeld video inzichten verkrijgen, bijschriften downloaden, ophalen widgets, enzovoort. 

U kunt bepalen of deze tokens alleen-lezen zijn of ze bewerken door op te geven toestaan **allowEdit = true/false**.

Voor de meeste scenario's met server-naar-server wordt waarschijnlijk u dezelfde **account** token omdat deze beide heeft **account** operations en **video** bewerkingen. Echter, als u van plan bent om ervoor te clientzijde aanroepen naar Video indexeerfunctie (bijvoorbeeld via javascript) u wilt gebruiken een **video** toegangstoken om te voorkomen dat clients wordt de toegang tot het hele account. Is ook de reden dat wanneer VideoIndexer clientcode insluiten in uw client (bijvoorbeeld **ophalen Insights Widget** of **Player Widget ophalen**) moet u opgeven een **video**toegangstoken.

Eenvoudiger om, kunt u de **autorisatie** API > **GetAccounts** uw accounts zonder het verkrijgen van een gebruiker-token ophalen eerst. U kunt ook vragen om op te halen van de accounts met geldige-tokens, zodat u kunt het overslaan van een extra aanroep voor het ophalen van een account-token.

Toegangstokens verlopen na 1 uur. Zorg ervoor dat uw toegangstoken geldig is voordat u met de Operations-API. Als is verlopen, roept u de API autorisatie opnieuw als u een nieuw toegangstoken.
 
U bent klaar om te integreren met de API. Zoeken naar [de gedetailleerde beschrijving van elke Video indexeerfunctie REST-API](http://api-portal.videoindexer.ai/).

## <a name="location"></a>Locatie

Alle bewerking API's vereisen een locatieparameter, waarmee wordt aangegeven van de regio voor de aanroep moet worden gestuurd en waarin het account is gemaakt.

De waarden in de volgende tabel beschreven toepassing. De **Param waarde** is de waarde die u wanneer doorgeeft met behulp van de API.

|**Naam**|**Waarde van parameter**|**Beschrijving**|
|---|---|---|
|Proefversie|proefversie|Gebruikt voor proefaccounts.|
|VS - west|westus2|Gebruikt voor de regio Azure West ons 2.|
|Noord-Europa |northeurope|Gebruikt voor de regio Noord-Europa Azure.|
|Oost-Azië|eastasia|Gebruikt voor de regio Azure Oost-Azië.|

## <a name="account-id"></a>Account-id 

De Account-ID-parameter is vereist voor alle operationele API-aanroepen. Account-ID is een GUID die kan worden verkregen in een van de volgende manieren:

* Gebruik de portal Video indexeerfunctie om de Account-ID:

    1. Aanmelden bij [videoindexer](https://www.videoindexer.ai/).
    2. Blader naar de **instellingen** pagina.
    3. Kopieer de account-ID.

        ![Account-id](./media/video-indexer-use-apis/account-id.png)

* Gebruik de API om via een programma de Account-ID.

    Gebruik de [accounts ophalen](https://api-portal.videoindexer.ai/docs/services/authorization/operations/Get-Accounts?) API.
    
    > [!TIP]
    > U kunt toegangstokens voor de accounts genereren door het definiëren van `generateAccessTokens=true`.
    
* De account-ID ophalen van de URL van een pagina player in uw account.

    Als u een video bekijkt, de ID wordt weergegeven na de `accounts` sectie en vóór de `videos` sectie.

    ```
    https://www.videoindexer.ai/accounts/00000000-f324-4385-b142-f77dacb0a368/videos/d45bf160b5/
    ```

## <a name="recommendations"></a>Aanbevelingen

Deze sectie vindt enkele aanbevelingen wanneer Video indexeerfunctie API.

- Als u van plan bent een video uploaden, wordt het aanbevolen om het bestand in de locatie van een openbaar netwerk (bijvoorbeeld OneDrive). Haal de koppeling naar de video en geef de URL als het bestand uploaden param. 

    De URL die is geleverd aan Indexer Video moet verwijzen naar een mediabestand (audio of video). Sommige van de koppelingen die worden gegenereerd door OneDrive zijn voor een HTML-pagina met het bestand. Een eenvoudige verificatie voor de URL te plakken in een browser – als het bestand wordt gestart met het downloaden van zijn zou, is het waarschijnlijk een goede URL. De browser, is enkele visualisatie rendering, is dit waarschijnlijk niet als een koppeling naar een bestand, maar een HTML-pagina.
    
- Als u de API die video insights opgehaald voor de opgegeven video aanroepen, krijgt u een gedetailleerde JSON-uitvoer als de antwoordinhoud. [Zie voor meer informatie over de geretourneerde JSON in dit onderwerp](video-indexer-output-json.md).

## <a name="code-sample"></a>Codevoorbeeld

De volgende C#-codefragment ziet samen u het gebruik van de API voor de indexeerfunctie Video.

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

[Bekijk de details van de JSON-uitvoer](video-indexer-output-json.md).

## <a name="see-also"></a>Zie ook

[Video indexeerfunctie-overzicht](video-indexer-overview.md)
