---
title: Visual Search-zelfstudie voor mobiele app
description: Open Source C#-toepassing implementeren visual zoeken op basis van het kader van de platformoverschrijdende cognitieve Services Computer Vision-API, Web zoeken-API van Bing en Xamarin.Forms.
services: bing-web-search, computer-vision
author: Aristoddle
manager: bking
ms.service: cognitive-services
ms.devlang: csharp
ms.topic: article
ms.date: 06/22/2017
ms.author: t-jolan
ms.openlocfilehash: 54dabaeaad2e49ba7cc138636054bc3dfa4b8379
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344785"
---
# <a name="visual-search-mobile-app-tutorial"></a>Visual Search-zelfstudie voor mobiele app

## <a name="introduction"></a>Inleiding  
Deze zelfstudie behandelt de [Computer Vision-API](https://azure.microsoft.com/services/cognitive-services/computer-vision/) en [Bing Web Search API](https://azure.microsoft.com/services/cognitive-services/bing-web-search-api/) eindpunten en toont hoe ze kunnen worden gebruikt voor het bouwen van een visual basic zoektoepassing met [Xamarin.Forms](https://developer.xamarin.com/guides/xamarin-forms/).  Deze zelfstudie over het algemeen bevat informatie over de volgende onderwerpen: 

> [!div class="checklist"]
> * Het instellen van Visual Studio voor Xamarin.Forms-toepassingen te ontwikkelen
> * Met behulp van de [Xamarin Media invoegtoepassing](https://github.com/jamesmontemagno/MediaPlugin) vastleggen en image-gegevens importeren
> * Parseren van tekst van een installatiekopie met de Computer Vision-API 's
> * Search-aanvragen verzenden naar de Web zoeken-API van Bing
> * Bij het parseren van JSON-antwoorden van beide API's met [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) (met LINQ en dit model object deserialisatie)
> * Maken van een platformoverschrijdende Xamarin.Forms-gebruikersinterface voor visual zoeken 

## <a name="prerequisites"></a>Vereisten

In dit voorbeeld is ontwikkeld met behulp van Xamarin.Forms met [Visual Studio 2017](https://www.visualstudio.com/downloads/). De Community Edition van Visual Studio 2017 kan worden gebruikt. Zie voor meer informatie over het gebruik van Xamarin met Visual Studio de [Xamarin documentatie](https://developer.xamarin.com/guides/cross-platform/getting_started/).

Dit voorbeeld wordt gebruikgemaakt van de volgende NuGet-pakketten:

> [!div class="checklist"]
> * [Invoegtoepassing Xamarin-Media](https://github.com/jamesmontemagno/MediaPlugin)
> * [Json.NET](https://github.com/JamesNK/Newtonsoft.Json)

De toepassing de volgende cognitieve Services API's gebruikt:

> [!div class="checklist"]
> * [Zoeken op het Web Bing API](https://azure.microsoft.com/services/cognitive-services/bing-web-search-api/) 
> * [Computer Vision-API](https://azure.microsoft.com/services/cognitive-services/computer-vision/)

U kunt 30-daagse evaluatieversie sleutels voor deze API's, Zie [cognitieve Services probeer](https://azure.microsoft.com/try/cognitive-services/). Zie voor meer informatie over het verkrijgen van sleutels voor commerciële doeleinden [prijzen](https://azure.microsoft.com/pricing/calculator/).

## <a name="setup-for-development"></a>Setup voor ontwikkeling  

### <a name="installing-xamarin-on-windows"></a>Xamarin in Windows installeren
Met een versie van Visual Studio 2017 geïnstalleerd, opent u de Visual Studio Installer, selecteert u het menu hamburger is gekoppeld aan uw installatie van Visual Studio en kies **wijzigen**.  

![Visual Studio-installatieprogramma](./media/computer-vision-web-search-tutorial/visual-studio-installer.png) 

Blader omlaag naar de mobiele & games en inschakelen **Mobile ontwikkeling met .NET**, als deze niet is ingeschakeld.

![Visual Studio-installatieprogramma met Xamarin.Forms geselecteerd](./media/computer-vision-web-search-tutorial/xamarin-forms-is-enabled.png)

Tot slot op **wijzigen** in de rechterbenedenhoek van het venster en wacht Xamarin te installeren.

### <a name="installing-xamarin-on-macos"></a>Xamarin installeren op Mac OS
Xamarin wordt vooraf geleverd met Visual Studio voor Mac. Er worden geen installatie moet nodig.

## <a name="building-and-running-the-app"></a>Bouwen en uitvoeren van de app.

Een Visual Studio solution bestand *(.sln)* voor de zoekopdracht Visual toepassing kan worden gedownload vanaf [Visual zoeken App met cognitieve Services](https://azure.microsoft.com/resources/samples/cognitive-services-xamarin-forms-computer-vision-search/). U kunt downloaden van het ZIP-archief met een webbrowser, dit naar uw werkstation klonen vanuit GitHub of downloaden met behulp van Visual Studio.

Als u wilt werken met het voorbeeld, open `VisualSearchApp.sln` in Visual Studio.  Tijdens de initialisatie van de vereiste onderdelen kan even duren.

De toepassing vereist twee derde bibliotheken: **Json.NET** en de **Xamarin Media invoegtoepassing**. U kunt deze bibliotheken rechts in Visual Studio met de NuGet Package Manager installeren. Kies **Extra > NuGet Package Manager > beheren NuGet-pakketten voor oplossing**, of met de rechtermuisknop op de oplossing in Solution Explorer en kiest u **NuGet-pakketten beheren** in het contextmenu.

Zoek in het venster NuGet en installeer **invoegtoepassing Xamarin Media** (Xam.Plugin.Media) versie 2.6.2 en **Json.NET** (Newtonsoft.Json) 10.0.3. Zorg ervoor dat alle projecten selecteren wanneer u installeert.

Als u wilt maken van de toepassing voor alle beschikbare platformen, drukt u op **Ctrl + Shift + B**, of klik op **bouwen** in het lintmenu en kies **Build Solution**.  Zie voor het samenstellen en testen van code voor iOS uit de Windows-systeem, [in deze handleiding](https://developer.xamarin.com/guides/ios/getting_started/installation/windows/) voor hulp.

Voordat u de toepassing uitvoert, moet u een doel configuratie, het Platform en het Project.  Voor Windows-, Android en iOS worden gecompileerd naar systeemeigen code Xamarin.Forms-toepassingen. Deze zelfstudie bevat schermopnamen van de Windows-versie van het voorbeeld, maar alle versies functioneel equivalent zijn. De implementatie-instellingen gebruikt in deze handleiding worden hier weergegeven.  

![Visual Studio die zijn geconfigureerd voor het compileren voor een Android-telefoon](./media/computer-vision-web-search-tutorial/configuration-selection.png)

Zodra het buildproces is voltooid en u een doelplatform hebt geselecteerd, klikt u op de **Start** knop in de werkbalk of druk op **F5**. Visual Studio uw oplossing implementeert in uw doelplatform en deze wordt gestart.

De pagina sleutels toevoegen wordt weergegeven. (Deze pagina is gedefinieerd in `AddKeysPage.xaml`.)  

![Afbeelding van de toevoegen sleutels pagina](./media/computer-vision-web-search-tutorial/add-keys-page.png)  

Hier kunt plakken in uw Computer Vision en Bing Web zoeken-API-sleutels. De Computer Vision-API moet u de server waarop het eindpunt wordt gehost.

> [!TIP]
> Om over te slaan op deze pagina, voert u deze gegevens in de juiste locaties in `App.xaml.cs`. 

De toepassing uw sleutels gevalideerd door het uitvoeren van een testquery, klikt u vervolgens vindt die u naar het Selecteer OCR pagina (gedefinieerd in `OcrSelectPage.xaml`).
   
![Afbeelding van de OCR Selecteer pagina](./media/computer-vision-web-search-tutorial/ocr-select-page.png)  

Klik boven aan dit scherm kunt u kiezen of de tekst die u wilt kunnen herkennen is afgedrukt of geschreven.

> [!TIP]
> Het item van waaruit u probeert te herkennen als niveau mogelijk tekst en zorg ervoor dat het gelijkmatig licht met geen weerspiegeling houdt. We hebben gevonden dat geschreven OCR soms beter voor script lettertypen of andere "fraai" tekst werkt.

Klik vervolgens op **nemen Photo** of **importeren Photo** aan een foto met behulp van de camera van uw apparaat neemt of kies een foto op uw apparaat opgeslagen.

Nadat een foto is genomen of gekozen, wordt de afbeelding wordt doorgegeven aan de Computer Vision-API. De pagina woorden gevonden (gedefinieerd in `OcrResultsPage.xaml`) woorden herkend in de afbeelding wordt weergegeven.

![Afbeelding van de resultatenpagina OCR](./media/computer-vision-web-search-tutorial/ocr-results-page.png)  

> [!NOTE]
> De installatiekopie die wordt gebruikt voor deze resultaten is op de bron-opslagplaats als `SamplePhotos\TestImage.jpg`.

Wanneer u een item op de woorden gevonden op pagina, de resultaten van de Web-pagina (`WebResultsPage.xaml`) wordt weergegeven, waarin u de bovenkant Bing-voor die zoekopdracht resultaten.

![Afbeelding van de webpagina van de resultaten](./media/computer-vision-web-search-tutorial/web-results-page.png)  
Kies ten slotte een item op de pagina webresultaten voor de koppeling in een ingesloten webweergave openen. (Of Ga terug om te selecteren die een ander resultaat.)

![Afbeelding van de weergave webpagina](./media/computer-vision-web-search-tutorial/web-view-page.png)  

U kunt met de pagina werken zoals u zou in elke webbrowser of Ga terug naar de pagina met webonderdelen. 

## <a name="review-and-learn"></a>Bekijk en meer informatie

Nu dat u de app Visual zoeken voor een spin hebt gemaakt, gaan we verkennen precies hoe we maken gebruik van de twee Microsoft cognitieve Services-API's.  Of u dit voorbeeld als een beginpunt voor uw eigen toepassing of als een instructies voor de API's, is het nuttigst voor het doorlopen van de toepassing scherm scherm om te onderzoeken precies hoe het werkt.

### <a name="add-keys-page"></a>Sleutels pagina toevoegen
De sleutels toevoegen pagina gebruikersinterface is gedefinieerd in `AddKeysPage.xaml`, en de primaire logica wordt gedefinieerd in `AddKeysPage.xaml.cs`. Aangezien de sleutels worden gevalideerd door een testaanvraag, is de tijd is gereed om het gebruik van cognitieve Services-eindpunten in C# stand te brengen.  

De basisstructuur van deze communicatie is: 

1. Instantiëren *HttpResponseMessage* en *HttpClient* objecten uit *System.Net.Http*
2. Gewenste headers (gedefinieerd in de API-referentiemateriaal voor elk eindpunt) koppelen aan de *HttpClient* object
3. Een GET of POST-aanvraag met uw gegevens, alle benodigde parameters toe te voegen aan uw eindpunt URI verzenden
4. Controleer of het antwoord is geslaagd
5. Het antwoord op voor verdere verwerking doorgeven

De functie die de geldigheid van de Bing zoeken-API-sleutel controleert is `CheckBingSearchKey()`, hier weergegeven.

```csharp
async Task CheckBingSearchKey(object sender = null, EventArgs e = null)
{
    HttpResponseMessage response;
    HttpClient SearchApiClient = new HttpClient();

    SearchApiClient.DefaultRequestHeaders.Add(AppConstants.OcpApimSubscriptionKey, BingSearchKeyEntry.Text);

    try
    {
        response = await SearchApiClient.GetAsync(AppConstants.BingWebSearchApiUrl + "q=test");

        if (response.StatusCode != System.Net.HttpStatusCode.Unauthorized)
        {
            BingSearchKeyEntry.BackgroundColor = Color.Green;
            AppConstants.BingWebSearchApiKey = BingSearchKeyEntry.Text;
            bingSearchKeyWorks = true;
        }
        else
        {
            BingSearchKeyEntry.BackgroundColor = Color.Red;
            bingSearchKeyWorks = false;
        }
    }
    catch( Exception exception )
    {
        BingSearchKeyEntry.BackgroundColor = Color.Red;
        Console.WriteLine($"ERROR: {exception.Message}");
    }
}
```

Een vergelijkbare functie `CheckComputerVisionKey()`, voor het valideren van de Computer Vision-sleutel wordt gebruikt.

### <a name="ocr-select-page"></a>Pagina OCR selecteren

De pagina OCR selecteren (`OcrSelectPage.xaml`) heeft twee belangrijke functies. Eerst bepaalt wat voor soort OCR wordt uitgevoerd op de doel-foto. Ten tweede wordt de gebruiker vastleggen of open de afbeelding die zij willen verwerken.

De tweede taak is traditioneel omslachtige in een toepassing platformoverschrijdende omdat voor elk platform andere code is vereist. De Xamarin-invoegtoepassing voor Media afhandelt met een paar regels code.

De volgende functie bevat een voorbeeld van het gebruik van de invoegtoepassing Xamarin Media voor photo vastleggen.  In dit we:

1. Zorg ervoor dat een camera beschikbaar op het huidige apparaat
2. Exemplaar maken van een `StoreCameraMediaOptions` object opgeven waar de vastgelegde kopie opslaan
3. Een installatiekopie en het verkrijgen van een `MediaFile` object met de afbeeldingsgegevens
4. Pak de `MediaFile` in een bytematrix
5. De bytematrix voor verdere verwerking retourneren

Hier volgt `TakePhoto()`, de functie die gebruikmaakt van de invoegtoepassing Xamarin Media voor photo vastleggen.  

```csharp
async Task<byte[]> TakePhoto()
{
    MediaFile photoMediaFile = null;
    byte[] photoByteArray = null;

    if (CrossMedia.Current.IsCameraAvailable)
    {
        var mediaOptions = new StoreCameraMediaOptions
        {
            PhotoSize = PhotoSize.Medium,
            AllowCropping = true,
            SaveToAlbum = true,
            Name = $"{DateTime.UtcNow}.jpg"
        };
        photoMediaFile = await CrossMedia.Current.TakePhotoAsync(mediaOptions);
        photoByteArray = MediaFileToByteArray(photoMediaFile);
    }
    else
    {
        await DisplayAlert("Error", "No camera found", "OK");
        Console.WriteLine($"ERROR: No camera found");
    }
    return photoByteArray;
}
```
De foto importeren hulpprogramma werkt op een vergelijkbare manier. Beide soorten functionaliteit kunnen u vinden in `OcrSelectPage.xaml.cs`. 
 
> [!NOTE]
> Foto's die kleiner dan 4 MB zijn zijn alleen geschikt voor het eindpunt OCR geschreven. Bestand grootte om problemen te voorkomen, beperken we de foto tot 50% van de oorspronkelijke grootte door de optie `PhotoSize = PhotoSize.Medium` op de `StoreCameraMediaOptions` object.  Als uw apparaat uitzonderlijk hoogwaardige foto's neemt en uw fouten, kunt u proberen `PhotoSize = PhotoSize.Small` in plaats daarvan.

Dit is de hulpprogrammafunctie is gebruikt om te converteren een *MediaFile* in een byte-matrix.

```csharp
byte[] MediaFileToByteArray(MediaFile photoMediaFile)
{
    using (var memStream = new MemoryStream())
    {
        photoMediaFile.GetStream().CopyTo(memStream);
        return memStream.ToArray();
    }
}
```

### <a name="ocr-results-page"></a>OCR resultatenpagina

De pagina OCR resultaten worden weergegeven tekst opgehaald uit de installatiekopie met de **Json.NET** [SelectToken methode](http://www.newtonsoft.com/json/help/html/SelectToken.htm).  De twee OCR eindpunten werken enigszins anders, dus is het nuttigst voor beide bespreken.  

Omdat de Computer Vision-API wordt alleen op enkele serverlocaties worden gehost, moet het eindpunt dynamisch worden samengesteld tijdens runtime. De functie `SetOcrLocation` (onderdeel van de statische *AppConstants* klasse gevonden `AppConstants.cs`) stelt de locatie van het URI-eindpunt. Dit komt overeen met de selectie van de gebruiker op de pagina sleutels toevoegen of gebruikt de waarde ingesteld `App.xaml.cs`. 

```csharp
public static void SetOcrLocation(string location)
{
    ComputerVisionApiOcrUrl = $"https://{location}.api.cognitive.microsoft.com/vision/v1.0/ocr?language=en&detectOrientation=true";
    ComputerVisionApiHandwritingUrl = $"https://{location}.api.cognitive.microsoft.com/vision/v1.0/recognizeText?handwriting=true";
}
```

U gaat nu een nader bekeken deze API-aanvragen. De Computer Vision OCR API is geschikt is voor het parseren van tekst van een onbepaalde taal, maar we ook om te zoeken naar Engelse tekst voor het verbeteren van de resultaten. We kunnen ook de API voor het detecteren van de richting van tekst voor ons. Met behulp van een vaste richting mogelijk verbeteren van onze parseren resultaten, maar afdrukstand detectie is handig in een mobiele toepassing.

U kunt meer informatie over de parameters die beschikbaar zijn met dit eindpunt van de [afdrukken OCR API Reference](https://westus.dev.cognitive.microsoft.com/docs/services/56f91f2d778daf23d8ec6739/operations/56f91f2e778daf14a499e1fc).  

De OCR geschreven API is nog steeds in preview en momenteel werkt alleen met tekst in het Engels.  Daarom is de enige parameter momenteel een vlag die aangeeft of u hand geschreven op alle parseren. Beide kunnen worden geparseerd door de geschreven OCR API machine gedrukte en geschreven tekst, maar `handwriting=false` levert betere resultaten op afgedrukte tekst. 

Omdat deze toepassing in het Engels, we kan alleen geschreven OCR gebruikt voor dit voorbeeld en ingesteld de `handwriting` vlag afhankelijk van wat de gebruiker vertellen worden herkend.  We beide eindpunten gebruikt ter illustratie.  

U kunt meer informatie over de parameters die beschikbaar zijn met dit eindpunt van de [geschreven OCR API Reference](https://westus.dev.cognitive.microsoft.com/docs/services/56f91f2d778daf23d8ec6739/operations/587f2c6a154055056008f200).

Nu we bekijken de functies die deze API's aanroepen.

`FetchPrintedWordList()` maakt gebruik van het eindpunt afdrukken OCR parseren afgedrukte tekst van afbeeldingen. De HTTP-aanvraag volgt een structuur die vergelijkbaar is met de aanroep die in de pagina sleutels toevoegen gebruikt voor het valideren van de sleutel, maar we moeten foto's verzenden. Een foto is te groot om door te geven in een queryreeks zodat we een HTTP POST-aanvraag in plaats van een GET-aanvraag gebruiken moet. Moeten we onze photo (die bestaat in het geheugen als een bytematrix) coderen in een `ByteArrayContent` object en een koptekst kunt toevoegen aan dit object definieert het type gegevens ontvangt. 

U kunt meer informatie over de aanvaardbare inhoudstypen die in de [Computer Vision-API-referentiemateriaal](https://westus.dev.cognitive.microsoft.com/docs/services/56f91f2d778daf23d8ec6739/operations/587f2c6a154055056008f200).  

```csharp
async Task<ObservableCollection<string>> FetchPrintedWordList()
{
    ObservableCollection<string> wordList = new ObservableCollection<string>();
    if (photo != null)
    {
        HttpResponseMessage response = null;
        using (var content = new ByteArrayContent(photo))
        {
            // The media type of the body sent to the API. 
            // "application/octet-stream" defines an image represented 
            // as a byte array
            content.Headers.ContentType = new MediaTypeHeaderValue("application/octet-stream");
            response = await visionApiClient.PostAsync(AppConstants.ComputerVisionApiOcrUrl, content);
        }

        string ResponseString = await response.Content.ReadAsStringAsync();
        JObject json = JObject.Parse(ResponseString);

        // Here, we pull down each "line" of text and then join it to 
        // make a string representing the entirety of each line.  
        // In the Handwritten endpoint, you are able to extract the 
        // "line" without any further processing.  If you would like 
        // to simply get a list of all extracted words,* you can do 
        // this with:
        // json.SelectTokens("$.regions[*].lines[*].words[*].text) 
        IEnumerable<JToken> lines = json.SelectTokens("$.regions[*].lines[*]");
        if (lines != null)
        {
            foreach (JToken line in lines)
            {
                IEnumerable<JToken> words = line.SelectTokens("$.words[*].text");
                if (words != null)
                {
                    wordList.Add(string.Join(" ", words.Select(x => x.ToString())));
                }
            }
        }
    }
    return wordList;
}
```
> [!TIP]
> Houd er rekening mee hoe we maken gebruik van de **Json.NET** [SelectToken methode](http://www.newtonsoft.com/json/help/html/SelectToken.htm) tekst ophalen uit het antwoordobject. `SelectToken` is ideaal als we alleen een bepaald deel van de JSON-antwoord dat we vervolgens aan de volgende functie doorgeven kunnen nodig. We in andere onderdelen van de code in modelobjecten die zijn gedefinieerd in JSON-antwoorden deserialiseren `ModelObjects.cs`.

Het belangrijkste verschil tussen de afdrukken OCR en geschreven OCR-aanvraag is de service afdrukken OCR retourneert de herkende tekst als onderdeel van het HTTP-antwoord, terwijl de service geschreven OCR een extra aanvraag vereist voor het ophalen van de gegevens. De eerste aanvraag voor OCR geschreven retourneert een 202 HTTP-status, ten teken dat alleen dat de verwerking is gestart. Dit antwoord bevat een eindpunt dat de client moet worden gecontroleerd voltooid antwoord verkrijgen wanneer deze beschikbaar is. Zie `FetchHandwrittenWordList()` om te zien hoe alles werkt.

```csharp
async Task<ObservableCollection<string>> FetchHandwrittenWordList()
{
    ObservableCollection<string> wordList = new ObservableCollection<string>();
    if (photo != null)
    {
        // Make the POST request to the handwriting recognition URL
        HttpResponseMessage response = null;
        using (var content = new ByteArrayContent(photo))
        {
            // The media type of the body sent to the API. 
            // "application/octet-stream" defines an image represented 
            // as a byte array
            content.Headers.ContentType = new MediaTypeHeaderValue("application/octet-stream");
            response = await visionApiClient.PostAsync(AppConstants.ComputerVisionApiHandwritingUrl, content);
        }

        // Fetch results
        IEnumerable<string> operationLocationValues;
        string statusUri = string.Empty;
        if (response.Headers.TryGetValues("Operation-Location", out operationLocationValues))
        {
            statusUri = operationLocationValues.FirstOrDefault();

            // Ping status URL, wait for processing to finish 
            JObject obj = await FetchResultFromStatusUri(statusUri);
            IEnumerable<JToken> strings = obj.SelectTokens("$.recognitionResult.lines[*].text");
            foreach (string s in strings)
            {
                wordList.Add((string)s);
            }
        }
    }
    return wordList;
}
```

`FetchResultFromStatusUri()` het tweede gedeelte van het proces handschriftherkenning OCR is. Pingt deze de URI die is opgehaald uit de eerste reactie metagegevens ofwel totdat een resultaat wordt verkregen of de functie een optreedt time-out.  Het is belangrijk dat deze functie op de eigen thread asynchroon wordt aangeroepen. Deze methode zou anders de gebruikersinterface vergrendelen totdat verwerking voltooid is.

```csharp
// Takes in the url to check for handwritten text parsing results, and pings it per second until processing is finished
// Returns the JObject holding data for a successful parse
async Task<JObject> FetchResultFromStatusUri(string statusUri)
{
    JObject obj = null;
    int timeoutcounter = 0;
    HttpResponseMessage response = await visionApiClient.GetAsync(statusUri);
    string responseString = await response.Content.ReadAsStringAsync();
    obj = JObject.Parse(responseString);
    while ((!((string)obj.SelectToken("status")).Equals("Succeeded")) && (timeoutcounter++ < 60))
    {
        await Task.Delay(1000);
        response = await visionApiClient.GetAsync(statusUri);
        responseString = await response.Content.ReadAsStringAsync();
        obj = JObject.Parse(responseString);
    } 
    return obj;
}
```

### <a name="web-results-page"></a>Resultaten van webpagina
Nadat de gebruiker een zoekterm op de pagina OCR resultaten weergegeven selecteert, wordt er naar de pagina webresultaten gaan langs.  Hier maken we een [Bing Web Search API](https://azure.microsoft.com/services/cognitive-services/bing-web-search-api/) aangevraagd, verzenden naar de service-eindpunt en deserialiseren van de JSON-antwoord via de **Json.NET** [DeserializeObject](http://www.newtonsoft.com/json/help/html/DeserializeObject.htm) methode.  

```csharp
async Task<WebResultsList> GetQueryResults()
{
    // URL-encode the query term
    var queryString = System.Net.WebUtility.UrlEncode(queryTerm);

    // Here we encode the URL that will be used for the GET request to 
    // find query results.  Its arguments are as follows:
    // 
    // - [count=20] This sets the number of webpage objects returned at 
    //   "$.webpages" in the JSON response.  Currently, the API asks for 
    //   20 webpages in the response
    //
    // - [mkt=en-US] This sets the market where the results come from.
    //   Currently, the API looks for english results based in the 
    //   United States.
    //
    // - [q=queryString] This sets the string queried using the Search 
    //   API.   
    //
    // - [responseFilter=Webpages] This filters the response to only 
    //   include Webpage results.  This tag can take a comma seperated 
    //   list of response types that you are looking for.  If left 
    //   blank, all responses (webPages, News, Videos, etc) are 
    //   returned.
    //
    // - [setLang=en] This sets the languge for user interface strings. 
    //   To learn more about UI strings, check the Web Search API 
    //   reference.
    //
    // - [API Reference] https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference
    string uri = AppConstants.BingWebSearchApiUrl + $"count=20&mkt=en-US&q={queryString}&responseFilter=Webpages&setLang=en";

    // Make the HTTP Request
    WebResultsList webResults = null;
    HttpResponseMessage httpResponseMessage = await searchApiClient.GetAsync(uri);
    var responseContentString = await httpResponseMessage.Content.ReadAsStringAsync();
    JObject json = JObject.Parse(responseContentString);
    JToken resultBlock = json.SelectToken("$.webPages");
    if (resultBlock != null)
    {
        webResults = JsonConvert.DeserializeObject<WebResultsList>(resultBlock.ToString());
    }
    return webResults;
}
```

De Bing Web Search API werkt het beste als u zoveel mogelijk gegevens over wat de gebruiker mogelijk wilt opgeven. In het bijzonder moet bijna altijd u de `mkt` en `setLang` parameters (hier ingesteld voor Amerikaans Engels) om de locatie en de voorkeurstaal van uw gebruikers te identificeren.

> [!NOTE]
> We onze zoekquery Bing Web om ervoor te zorgen dat de broncode gemakkelijker te begrijpen is eenvoudig gehouden.  In een echte toepassing, moet u de volgende headers toevoegen aan uw HTTP-aanvraag voor betere resultaten. 
> * Gebruikersagent  
> * X-MSEdge-ClientID  
> * X-Search-client-IP  
> * X-Search-locatie  
>
> U kunt meer informatie over deze headerwaarden in de [Bing Web zoeken-API-referentiemateriaal](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#headers)

## <a name="next-steps"></a>Volgende stappen
Cognitieve Services van Microsoft bieden veel extra services die eenvoudig in deze toepassing kan worden geïntegreerd.  U kunt bijvoorbeeld het volgende doen:

* Voeg [Bing entiteit Search](https://azure.microsoft.com/services/cognitive-services/bing-entity-search-api/) naar uw zoekresultaten verbeteren
* Wisselen [Bing aangepaste zoekactie](https://azure.microsoft.com/services/cognitive-services/bing-custom-search/) in plaats van Bing webpagina's zoeken
* Gebruik de [zoeken naar Bing-afbeelding](https://azure.microsoft.com/services/cognitive-services/bing-image-search-api/) insights mogelijkheid voor meer informatie over uw installatiekopie en soortgelijke afbeeldingen vinden op de webserver, een installatiekopie
* Alvast [spellingcontrole van Bing](https://azure.microsoft.com/services/cognitive-services/spell-check/) voor het verder verbeteren van de kwaliteit van de geparseerde tekst
* Integreert de [Microsoft Translator](https://azure.microsoft.com/services/cognitive-services/translator-text-api/) om te zien van uw uitgepakte tekst in verschillende talen
* Meng en overeenkomen met de andere services van de [cognitieve Services-Portal](https://azure.microsoft.com/services/cognitive-services/); de lucht de limiet voor de!
