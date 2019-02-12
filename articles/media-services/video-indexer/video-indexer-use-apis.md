---
title: Gebruik de Video Indexer-API - Azure
titlesuffix: Azure Media Services
description: Met dit artikel leert u hoe u aan de slag gaat met de Video Indexer-API.
services: media-services
author: Juliako
manager: femila
ms.service: media-services
ms.topic: article
ms.date: 02/10/2019
ms.author: juliako
ms.openlocfilehash: dc3e654e741fd1a326c83e51c72284f41ad74798
ms.sourcegitcommit: e69fc381852ce8615ee318b5f77ae7c6123a744c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/11/2019
ms.locfileid: "55988048"
---
# <a name="tutorial-use-the-video-indexer-api"></a>Zelfstudie: De Video Indexer-API gebruiken

> [!Note]
> De Video Indexer V1-API is afgeschaft op 1 augustus 2018. U moet nu de Video Indexer v2-API gebruiken. <br/>Bekijk [hier](https://api-portal.videoindexer.ai/) de instructies voor ontwikkeling met de Video Indexer v2-API's. 

Video Indexer integreert een aantal audiovisuele technologieën voor kunstmatige intelligentie (AI) die Microsoft in een enkele geïntegreerde service aanbiedt, wat de ontwikkeling eenvoudiger maakt. De API's zijn ontworpen om het ontwikkelaars mogelijk te maken zich te concentreren op het verbruik van Media AI-technologieën, zonder zich druk te hoeven maken over schaal, wereldwijd bereik, beschikbaarheid en betrouwbaarheid van cloud-platforms. U kunt de API gebruiken om uw bestanden te uploaden, gedetailleerde video-inzichten te krijgen, URL's van inzicht- en spelerwidgets te krijgen om deze in te sluiten in uw toepassing of voor andere taken.

Wanneer u een Video Indexer-account maakt, kunt u kiezen uit een gratis proefversie (waarmee u een bepaald aantal gratis minuten indexering krijgt) of een betaalde optie (zonder quotumlimiet). Bij de gratis proefversie biedt Video Indexer websitegebruikers maximaal 600 minuten aan gratis indexering en API-gebruikers maximaal 2400 minuten gratis indexering. Met de betaalde versie maakt u een Video Indexer-account dat is [gekoppeld aan uw Azure-abonnement en een Azure Media Services-account](connect-to-azure.md). U betaalt zowel voor de geïndexeerde minuten als voor kosten verbonden aan het Azure Media Services-account. 

In dit artikel wordt uitgelegd hoe ontwikkelaars kunnen profiteren van de [Video Indexer-API](https://api-portal.videoindexer.ai/).

## <a name="subscribe-to-the-api"></a>Abonneren op de API

1. Meld u aan bij de [Video Indexer-ontwikkelaarsportal](https://api-portal.videoindexer.ai/).
    
    ![Aanmelden](./media/video-indexer-use-apis/video-indexer-api01.png)

    > [!Important]
    > * U moet dezelfde provider gebruiken als voor het aanmelden bij Video Indexer.
    > * Persoonlijke Google- en Microsoft-accounts (Outlook/Live) kunnen alleen voor proefaccounts worden gebruikt. Accounts die gekoppeld zijn met Azure vereisen Azure Active Directory.
    > * U kunt slechts één actief account per e-mailadres gebruiken. Als een gebruiker zich met user@gmail.com voor LinkedIn probeert aan te melden en daarna met user@gmail.com voor Google, zal de laatste een foutpagina weergeven, met de tekst dat de gebruiker al bestaat.

2. Abonneer u.

    Selecteer het tabblad [Producten](https://api-portal.videoindexer.ai/products). Selecteer daarna Autorisatie en abonneren. 
    
    ![Aanmelden](./media/video-indexer-use-apis/video-indexer-api02.png)

    > [!NOTE]
    > Nieuwe gebruikers worden automatisch geabonneerd op Autorisatie.
    
    Wanneer u zich abonneert, kunt u uw abonnement en uw primaire en secundaire sleutels zien. De sleutels moeten beveiligd zijn. De sleutels mogen alleen door uw servercode gebruikt worden. Ze mogen niet aan clientzijde beschikbaar zijn (.js, .html, enzovoort).

    ![Aanmelden](./media/video-indexer-use-apis/video-indexer-api03.png)

> [!TIP]
> Een gebruiker van Video Indexer kan met één abonnementssleutel verbinding maken met meerdere Video Indexer-accounts. U kunt deze Video Indexer-accounts vervolgens koppelen aan verschillende Media Services-accounts.

## <a name="obtain-access-token-using-the-authorization-api"></a>Tokens verkrijgen met de Authorization-API

Zodra u geabonneerd bent op de Authorization-API, kunt u toegangstokens verkrijgen. Deze toegangstokens worden gebruikt voor verificatie op basis van de Operations-API. 

Elke aanroep naar de Operations-API moet worden gekoppeld aan een toegangstoken dat overeenkomt met het autorisatiebereik van de aanroep.

- Gebruikersniveau: met toegangstokens op gebruikersniveau kunt u bewerkingen uitvoeren op het **gebruikersniveau**. Hiermee kunt u bijvoorbeeld gekoppelde accounts ophalen.
- Accountniveau: met toegangstokens op accountniveau kunt u bewerkingen uitvoeren op het **accountniveau** of het **videoniveau**. Hiermee kunt u bijvoorbeeld een video uploaden, een lijst maken met alle video's, inzichten krijgen in video's, enzovoort.
- Videoniveau: met toegangstokens op videoniveau kunt u bewerkingen uitvoeren op een specifieke **video**. Hiermee kunt u bijvoorbeeld inzichten in een video krijgen, ondertiteling downloaden, widgets ophalen, enzovoort. 

U kunt bepalen of deze tokens alleen-lezen zijn of dat ze bewerkt kunnen worden door **allowEdit=true/false** op te geven.

Voor de meeste scenario's van server naar server, moet u waarschijnlijk hetzelfde **accounttoken** gebruiken omdat hierin zowel **accountbewerkingen** als **videobewerkingen** zijn opgenomen. Als u echter van plan bent om aanroepen van de clientzijde te doen met Video Indexer (bijvoorbeeld vanuit JavaScript), kunt u het best een **videotoegangstoken** gebruiken om te voorkomen dat clients toegang krijgen tot het gehele account. Dat is ook de reden dat wanneer u Video Indexer-clientcode insluit in uw client (bijvoorbeeld met behulp van **Inzichtenwidget ophalen** of **Spelerwidget ophalen**), u een **videotoegangstoken** moet opgeven.

Om het eenvoudiger te maken, kunt u **Authorization-API** > **GetAccounts** gebruiken om uw accounts op te halen zonder eerst een gebruikerstoken te verkrijgen. U kunt verzoeken om de accounts op te halen met behulp van geldige tokens, zodat u een extra aanroep om een accounttoken op te halen kunt overslaan.

Toegangstokens verlopen na één uur. Zorg ervoor dat uw token geldig is voordat u de Operations-API gebruikt. Als uw toegangstoken is verlopen, gebruikt u de Authorization-API om een nieuwe op te halen.
 
U bent nu klaar om te integreren met de API. Zoek [de gedetailleerde beschrijving van elke Video Indexer-REST API](http://api-portal.videoindexer.ai/).

## <a name="location"></a>Locatie

Alle API's voor bewerking vereisen een locatieparameter, waarmee de regio wordt aangegeven waar de aanroep naar moet worden gestuurd en waarin het account is gemaakt.

De waarden die worden beschreven in de volgende tabel zijn van toepassing. De **Parameterwaarde** is de waarde die u doorgeeft met behulp van de API.

|**Naam**|**Parameterwaarde**|**Beschrijving**|
|---|---|---|
|Proefversie|audittrail|Wordt gebruikt voor proefaccounts.|
|US - west|westus2|Wordt gebruikt voor de Azure-regio US - west 2.|
|Europa - noord |northeurope|Wordt gebruikt voor de Azure-regio Europa - noord.|
|Azië - oost|eastasia|Wordt gebruikt voor de Azure-regio Azië - oost.|

## <a name="account-id"></a>Account-id 

De parameter Account-id is vereist in alle operationele API-aanroepen. Account-id is een GUID die kan worden verkregen op een van de volgende manieren:

* Gebruik de **Video Indexer-website** om de account-id op te halen:

    1. Ga naar de [Video Indexer](https://www.videoindexer.ai/)-website en meld u aan.
    2. Ga naar de pagina **Instellingen**.
    3. Kopieer de account-id.

        ![Account-id](./media/video-indexer-use-apis/account-id.png)

* Gebruik de **Video Indexer-ontwikkelaarsportal** om de account-id programmatisch op te halen.

    Gebruik de API [Accounts ophalen](https://api-portal.videoindexer.ai/docs/services/authorization/operations/Get-Accounts?).
    
    > [!TIP]
    > U kunt de toegangstokens voor de accounts genereren door `generateAccessTokens=true` te definiëren.
    
* Haal de account-id op uit de URL van een pagina van de speler in uw account.

    Wanneer u een video bekijkt, wordt de id weergegeven na de sectie `accounts` en voor de sectie `videos`.

    ```
    https://www.videoindexer.ai/accounts/00000000-f324-4385-b142-f77dacb0a368/videos/d45bf160b5/
    ```

## <a name="recommendations"></a>Aanbevelingen

In deze sectie vindt u enkele aanbevelingen bij het gebruik van de Video Indexer-API.

- Als u van plan bent een video te uploaden, is het raadzaam om het bestand in een openbare netwerklocatie te plaatsen (bijvoorbeeld OneDrive). Haal de link naar de video op en geef de URL op als de parameter van het uploadbestand. 

    De URL die is opgegeven met Video Indexer moet verwijzen naar een mediabestand (audio of video). Sommige links die worden gegenereerd door OneDrive zijn voor een HTML-pagina die het bestand bevat. U kunt de URL eenvoudig verifiëren door deze in een browser te plakken. Als het bestand wordt gedownload, is het waarschijnlijk een goede URL. Als de browser een visualisatie weergeeft, is het waarschijnlijk niet een link naar een bestand, maar naar een HTML-pagina.
    
- Als u de API die video-inzichten ophaalt voor de opgegeven video aanroept, krijgt u gedetailleerde JSON-uitvoer als resultaat. [Informatie over de geretourneerde JSON in dit onderwerp](video-indexer-output-json-v2.md).

## <a name="code-sample"></a>Codevoorbeeld

Het volgende C#-codefragment toont het gebruik van alle Video Indexer-API's samen.

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

[Details van de JSON-uitvoer bekijken](video-indexer-output-json-v2.md)

[Overzicht van Video Indexer](video-indexer-overview.md)
