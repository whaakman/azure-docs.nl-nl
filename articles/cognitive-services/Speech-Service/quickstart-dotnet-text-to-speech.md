---
title: 'Quickstart: Converteren van tekst naar spraak, .NET Core - spraakservices'
titleSuffix: Azure Cognitive Services
description: In deze snelstartgids leert u hoe u kunt converteren van tekst naar spraak met de REST-API van tekst naar spraak. De voorbeeldtekst opgenomen in deze handleiding is opgebouwd als spraak synthese Markup Language (SSML). Hiermee kunt u de spraak en taal van het antwoord spraak te kiezen.
services: cognitive-services
author: erhopf
manager: cgronlun
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 12/13/2018
ms.author: erhopf
ms.openlocfilehash: 9e0b71acc4faa97bf1aa82e56a3784c349663dc0
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55207102"
---
# <a name="quickstart-convert-text-to-speech-using-net-core"></a>Snelstart: Converteert tekst naar spraak met behulp van .NET Core

In deze snelstartgids leert u hoe u om te converteren tekst naar spraak met behulp van .NET Core en de tekst naar spraak REST-API. De voorbeeldtekst opgenomen in deze handleiding is opgebouwd als [spraak synthese Markup Language (SSML)](speech-synthesis-markup.md), waarmee u de spraak en taal van het antwoord te kiezen.

Deze snelstartgids moet een [Azure Cognitive Services-account](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) met een resource Speech-Service. Als u geen account hebt, kunt u de [gratis proefversie](https://azure.microsoft.com/try/cognitive-services/) gebruiken om een abonnementssleutel op te halen.

## <a name="prerequisites"></a>Vereisten

Voor deze snelstart zijn de volgende zaken vereist:

* [.NET Core-SDK](https://dotnet.microsoft.com/download)
* [Visual Studio](https://visualstudio.microsoft.com/downloads/), [Visual Studio Code](https://code.visualstudio.com/download) of uw favoriete teksteditor
* Een Azure-abonnementssleutel voor de Speech Service

## <a name="create-a-net-core-project"></a>Een .NET Core-project maken

Open een nieuwe opdrachtprompt (of een terminalsessie) en voer deze opdrachten uit:

```console
dotnet new console -o tts-sample
cd tts-sample
```

De eerste opdracht doet twee dingen. Hiermee wordt een nieuwe .NET-consoletoepassing gemaakt en een map met de naam `tts-sample`. Met de tweede opdracht gaat u naar de map voor het project.

## <a name="select-the-c-language-version"></a>Selecteer de C# taalversie

Deze snelstartgids moet C# 7.1 of hoger. Er zijn een aantal manieren om te wijzigen de C# versie voor uw project. In deze handleiding leert u hoe u aan te passen de `tts-sample.csproj` bestand. Voor alle beschikbare opties, zoals het wijzigen van de taal van Visual Studio, Zie [selecteert u de C# taalversie](https://docs.microsoft.com/dotnet/csharp/language-reference/configure-language-version).

Open het project en open vervolgens `tts-sample.csproj`. Zorg ervoor dat `LangVersion` is ingesteld op 7.1 of hoger. Als er geen een groep met eigenschappen voor de taalversie, voegt u deze regels toe:

```csharp
<PropertyGroup>
   <LangVersion>7.1</LangVersion>
</PropertyGroup>
```

Zorg ervoor dat uw wijzigingen op te slaan.

## <a name="add-required-namespaces-to-your-project"></a>Vereiste naamruimten toevoegen aan het project

Met de opdracht `dotnet new console` die u eerder hebt uitgevoerd, is een project gemaakt, met daarin `Program.cs`. In dit bestand plaatst u de toepassingscode. Open `Program.cs` en vervang de bestaande using-instructies. Deze instructies zorgen ervoor dat u toegang hebt tot alle typen die zijn vereist voor het bouwen en uitvoeren van de voorbeeld-app.

```csharp
using System;
using System.Net.Http;
using System.Text;
using System.IO;
using System.Threading.Tasks;
```

## <a name="create-a-class-for-token-exchange"></a>Maak een klasse voor token uitwisseling

De Text to Speech REST-API is een toegangstoken voor verificatie vereist. Als u een toegangstoken, is een exchange vereist. In dit voorbeeld uitwisselt uw abonnementssleutel Spraakservice voor een access token met de `issueToken` eindpunt.

In dit voorbeeld wordt ervan uitgegaan dat uw spraak-Service-abonnement in de regio VS-West. Als u een andere regio, werk de waarde voor `FetchTokenUri`. Zie voor een volledige lijst [regio's](https://docs.microsoft.com/azure/cognitive-services/speech-service/regions#rest-apis).

```csharp
public class Authentication
{
    private string subscriptionKey;
    private string tokenFetchUri;

    public Authentication(string tokenFetchUri, string subscriptionKey)
    {
        if (string.IsNullOrWhiteSpace(tokenFetchUri))
        {
            throw new ArgumentNullException(nameof(tokenFetchUri));
        }
        if (string.IsNullOrWhiteSpace(subscriptionKey))
        {
            throw new ArgumentNullException(nameof(subscriptionKey));
        }
        this.tokenFetchUri = tokenFetchUri;
        this.subscriptionKey = subscriptionKey;
    }

    public async Task<string> FetchTokenAsync()
    {
        using (var client = new HttpClient())
        {
            client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", this.subscriptionKey);
            UriBuilder uriBuilder = new UriBuilder(this.tokenFetchUri);

            var result = await client.PostAsync(uriBuilder.Uri.AbsoluteUri, null).ConfigureAwait(false);
            return await result.Content.ReadAsStringAsync().ConfigureAwait(false);
        }
    }
}
```

> [!NOTE]
> Zie voor meer informatie over verificatie [verifiëren met een toegangstoken](https://docs.microsoft.com/azure/cognitive-services/authentication#authenticate-with-an-authentication-token).

## <a name="get-an-access-token-and-set-the-host-url"></a>Een toegangstoken ophalen en instellen van de host-URL

Zoek `static void Main(string[] args)` en vervang deze door `static async Task Main(string[] args)`.

Vervolgens kopieert u deze code in de belangrijkste methode. Het biedt een aantal dingen, maar belangrijker nog, die tekst nodig als invoer en aanroepen van de `Authentication` functie voor het uitwisselen van uw abonnementssleutel voor een toegangstoken. Als er iets misgaat, wordt de fout weergegeven in de console.

Zorg ervoor dat u uw abonnementssleutel toevoegen voordat u de app uitvoert.

```csharp
// Prompts the user to input text for TTS conversion
Console.Write("What would you like to convert to speech? ");
string text = Console.ReadLine();

// Gets an access token
string accessToken;
Console.WriteLine("Attempting token exchange. Please wait...\n");

// Add your subscription key here
// If your resource isn't in WEST US, change the endpoint
Authentication auth = new Authentication("https://westus.api.cognitive.microsoft.com/sts/v1.0/issueToken", "YOUR_SUBSCRIPTION_KEY");
try
{
    accessToken = await auth.FetchTokenAsync().ConfigureAwait(false);
    Console.WriteLine("Successfully obtained an access token. \n");
}
catch (Exception ex)
{
    Console.WriteLine("Failed to obtain an access token.");
    Console.WriteLine(ex.ToString());
    Console.WriteLine(ex.Message);
    return;
}
```

Stel vervolgens de host en de route voor tekst naar spraak:

```csharp
string host = "https://westus.tts.speech.microsoft.com/cognitiveservices/v1";
```

## <a name="build-the-ssml-request"></a>De aanvraag SSML bouwen

Tekst wordt verzonden als de instantie van een `POST` aanvraag. Met SSML, kunt u de spraak- en taal. In deze snelstart gebruiken we SSML met de taal die is ingesteld op `en-US` en de stem ingesteld als `ZiraRUS`. We maken de SSML voor uw aanvraag:

```csharp
string body = @"<speak version='1.0' xmlns='http://www.w3.org/2001/10/synthesis' xml:lang='en-US'>
              <voice name='Microsoft Server Speech Text to Speech Voice (en-US, ZiraRUS)'>" +
              text + "</voice></speak>";
```

> [!NOTE]
> In dit voorbeeld wordt de `ZiraRUS` spraakstijl. Voor een volledige lijst van door Microsoft geleverd stemmen/talen, Zie [taalondersteuning](language-support.md). Als u geïnteresseerd bent in het maken van een unieke, herkenbare stem voor uw merk, Zie [het maken van aangepaste spraakstijlen](how-to-customize-voice-font.md).

## <a name="instantiate-the-client-make-a-request-and-save-synthesized-audio-to-a-file"></a>Exemplaar maken van de client, een aanvraag indienen en kunstmatige audio naar een bestand opslaan

Er is een veel gebeurt in dit codevoorbeeld. We gaan snel bekijken wat er gebeurt:

* De client en de aanvraag worden geïnstantieerd.
* De HTTP-methode is ingesteld als `POST`.
* Vereiste headers zijn toegevoegd aan de aanvraag.
* De aanvraag wordt verzonden en de statuscode is ingeschakeld.
* Het antwoord is asynchroon gelezen en geschreven naar een bestand met de naam sample.wav.

Kopieer deze code in uw project. Vervang de waarde van de `User-Agent` -header met de naam van uw resource vanuit Azure portal.

```csharp
using (var client = new HttpClient())
{
    using (var request = new HttpRequestMessage())
    {
        // Set the HTTP method
        request.Method = HttpMethod.Post;
        // Construct the URI
        request.RequestUri = new Uri(host);
        // Set the content type header
        request.Content = new StringContent(body, Encoding.UTF8, "application/ssml+xml");
        // Set additional header, such as Authorization and User-Agent
        request.Headers.Add("Authorization", "Bearer " + accessToken);
        request.Headers.Add("Connection", "Keep-Alive");
        // Update your resource name
        request.Headers.Add("User-Agent", "YOUR_RESOURCE_NAME");
        request.Headers.Add("X-Microsoft-OutputFormat", "riff-24khz-16bit-mono-pcm");
        // Create a request
        Console.WriteLine("Calling the TTS service. Please wait... \n");
        using (var response = await client.SendAsync(request).ConfigureAwait(false))
        {
            response.EnsureSuccessStatusCode();
            // Asynchronously read the response
            using (var dataStream = await response.Content.ReadAsStreamAsync().ConfigureAwait(false))
            {
                Console.WriteLine("Your speech file is being written to file...");
                using (var fileStream = new FileStream(@"sample.wav", FileMode.Create, FileAccess.Write, FileShare.Write))
                {
                    await dataStream.CopyToAsync(fileStream).ConfigureAwait(false);
                    fileStream.Close();
                }
                Console.WriteLine("\nYour file is ready. Press any key to exit.");
                Console.ReadLine();
            }
        }
    }
}
```

## <a name="run-the-sample-app"></a>De voorbeeld-app uitvoeren

Dat is alles, u kunt nu uw Text to Speech app uit te voeren. Ga vanaf de opdrachtregel (of terminalsessie) naar de projectmap en voer het volgende uit:

```console
dotnet run
```

Als dit lukt, wordt de spraak-bestand wordt opgeslagen in de projectmap. Spelen met behulp van uw favoriete MediaPlayer.

## <a name="clean-up-resources"></a>Resources opschonen

Als u uw abonnementssleutel hebt vastgelegd in het programma, verwijdert u deze sleutel wanneer u klaar bent met de snelstart.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [.NET-voorbeelden op GitHub bekijken](https://github.com/Azure-Samples/Cognitive-Speech-TTS/tree/master/Samples-Http/NETCore)

## <a name="see-also"></a>Zie ook

* [Naslaginformatie voor de Text to Speech-API](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis#text-to-speech-api)
* [Het maken van aangepaste spraakstijlen](how-to-customize-voice-font.md)
* [Record stem voorbeelden voor het maken van een aangepaste spraak](record-custom-voice-samples.md)
