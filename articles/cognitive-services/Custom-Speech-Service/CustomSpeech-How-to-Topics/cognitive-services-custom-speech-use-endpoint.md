---
title: Gebruik een aangepaste spraak-eindpunt met Custom Speech Service op Azure | Microsoft Docs
description: Informatie over het gebruik van een aangepaste spraak-naar-tekst-eindpunt met de Custom Speech Service in Cognitive Services.
services: cognitive-services
author: PanosPeriorellis
manager: onano
ms.service: cognitive-services
ms.subservice: custom-speech
ms.topic: article
ms.date: 02/08/2017
ms.author: panosper
ms.openlocfilehash: 3f99172f486c374cdbd0b6ebd50fdf7bfec98ec3
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55227145"
---
# <a name="use-a-custom-speech-to-text-endpoint"></a>Een aangepast spraak naar tekst-eindpunt gebruiken

[!INCLUDE [Deprecation note](../../../../includes/cognitive-services-custom-speech-deprecation-note.md)]

U kunt aanvragen verzenden naar een eindpunt van de spraak-naar-tekst Azure Custom Speech Service op een soortgelijke manier als aan het standaardeindpunt voor Cognitive Services spraak. Deze eindpunten zijn identiek aan de Standaardeindpunten van de spraak-API. Dus is dezelfde functionaliteit die beschikbaar is via de clientbibliotheek of de REST-API voor de spraak-API ook beschikbaar voor uw aangepast eindpunt.

De eindpunten die u maakt met behulp van deze service kunnen verschillende aantallen gelijktijdige aanvragen verwerken. Het volume, is afhankelijk van de prijscategorie die zijn gekoppeld aan uw abonnement. Als er te veel aanvragen zijn ontvangen, wordt er een fout optreedt. De laag gratis geldt een maandelijkse limiet van aanvragen.

De service wordt ervan uitgegaan dat de gegevens in realtime wordt verzonden. Als het sneller wordt verzonden, kan de aanvraag wordt beschouwd als wordt uitgevoerd totdat de duur van de audio in real-time is verstreken.

> [!NOTE]
> We bieden ondersteuning voor de [nieuwe websockets](https://docs.microsoft.com/azure/cognitive-services/speech/api-reference-rest/websocketprotocol) nog. Als u van plan bent het gebruik van websockets met uw aangepaste spraak-eindpunt, volgt u de instructies hier.
>
> De nieuwe [REST-API](https://docs.microsoft.com/azure/cognitive-services/speech/getstarted/getstartedrest) ondersteuning is binnenkort beschikbaar. Als u van plan bent om aan te roepen van het eindpunt van uw aangepaste spraak via HTTP, volgt u de instructies hier.
>

## <a name="send-requests-by-using-the-speech-client-library"></a>Verzenden van aanvragen met behulp van de speech client-bibliotheek

Voor het verzenden van aanvragen naar uw aangepast eindpunt met behulp van de speech client-bibliotheek, start u de opname-client. Gebruik de Speech Client SDK van [NuGet](http://nuget.org/). Zoeken naar *spraakherkenning*, en selecteer het pakket met spraakherkenning van Microsoft voor uw platform. Een voorbeeld van code kan worden gevonden op [GitHub](https://github.com/Microsoft/Cognitive-Speech-STT-Windows). De Speech Client-SDK biedt een klasse factory **SpeechRecognitionServiceFactory**, die biedt de volgende methoden:

  *   ```CreateDataClient(...)```: Een client opname van gegevens.
  *   ```CreateDataClientWithIntent(...)```: Een opname-client voor gegevens met de bedoeling.
  *   ```CreateMicrophoneClient(...)```: Een microfoon opname-client.
  *   ```CreateMicrophoneClientWithIntent(...)```: Een microfoon opname-client met de bedoeling.

Zie voor gedetailleerde documentatie, de [Bing Speech-API](https://docs.microsoft.com/azure/cognitive-services/speech/home). De Custom Speech Service-eindpunten ondersteunen dezelfde SDK.

De gegevens opname-client is geschikt voor spraakherkenning van gegevens, zoals een bestand of andere audiobron. De microfoon opname-client is geschikt voor spraakherkenning van de microfoon. Het gebruik van het doel in een client kan retourneren gestructureerde intentie resultaten van de [Language Understanding Intelligent Service](https://www.luis.ai/) (LUIS), als u een LUIS-toepassing hebt gebouwd voor uw scenario.

Alle vier typen clients kunnen op twee manieren worden gemaakt. De standaard Cognitive Services Speech-API maakt gebruik van de eerste manier. De tweede manier kunt u een URL die overeenkomt met uw aangepaste eindpunt dat is gemaakt met de Custom Speech Service op te geven.

Bijvoorbeeld, kunt u een **DataRecognitionClient** die aanvragen verzendt naar een aangepast eindpunt met behulp van de volgende methode:

```csharp
public static DataRecognitionClient CreateDataClient(SpeeechRecognitionMode speechRecognitionMode, string language, string primaryOrSecondaryKey, **string url**);
```

De **your_subscriptionId** en **endpointURL** verwijzen naar de abonnementssleutel en de URL van de web-sockets, respectievelijk op de **informatie over de implementatie** pagina.

De **AuthenticationUri** wordt gebruikt voor het ontvangen van een token van de service voor verificatie. Deze URI moet worden afzonderlijk instellen, zoals wordt weergegeven in de volgende voorbeeldcode.

Deze voorbeeldcode ziet u hoe u de client-SDK:

```csharp
var dataClient = SpeechRecognitionServiceFactory.CreateDataClient(
  SpeechRecognitionMode.LongDictation,
  "en-us",
  "your_subscriptionId",
  "your_subscriptionId",
  "endpointURL");
// set the authorization Uri
dataClient.AuthenticationUri = "https://westus.api.cognitive.microsoft.com/sts/v1.0/issueToken";
```

> [!NOTE]
> Bij het gebruik **maken** methoden in de SDK, moet u de abonnements-ID opgeven tweemaal vanwege overbelasting van de **maken** methoden.
>

De Custom Speech Service maakt gebruik van twee verschillende URL's voor herkenning kort formulier en lange vorm. Beide worden weergegeven op de **implementaties** pagina. Gebruik de juiste eindpunt-URL voor de specifieke vorm die u wilt gebruiken.

Zie voor meer informatie over het aanroepen van de verschillende herkenning van clients met uw aangepast eindpunt, de [SpeechRecognitionServiceFactory](https://www.microsoft.com/cognitive-services/Speech-api/documentation/GetStarted/GetStartedCSharpDesktop) klasse. De documentatie op deze pagina verwijst naar akoestisch modelaanpassing, maar het is van toepassing op alle eindpunten die zijn gemaakt met behulp van de Custom Speech Service.

## <a name="send-requests-by-using-the-speech-protocol"></a>Verzenden van aanvragen met behulp van de spraak-Protocol

De eindpunten die worden weergegeven voor de [spraak Protocol](https://docs.microsoft.com/azure/cognitive-services/speech/api-reference-rest/websocketprotocol) zijn eindpunten voor de Open Source Web Socket spraak-Protocol.

De clientimplementatie alleen officiële is momenteel voor [JavaScript](https://github.com/Azure-Samples/SpeechToText-WebSockets-Javascript). Als u wilt beginnen met het voorbeeld dat is opgegeven, de volgende wijzigingen aanbrengen in de code en het voorbeeld opnieuw maken:

1. In _src\sdk\speech.browser\SpeechConnectionFactory.ts_, de host naam "wss://speech.platform.bing.com" vervangen door de naam van de host die wordt weergegeven op de pagina met details van uw implementatie. Voeg geen hier de volledige URI, maar alleen de *wss* protocol-schema en de hostnaam. Bijvoorbeeld:

    ```JavaScript
    private get Host(): string {
        return Storage.Local.GetOrAdd("Host", "wss://<your_key_goes_here>.api.cris.ai");
    }
    ```

2. Stel de _recognitionMode_ parameter in _samples\browser\Samples.html_ volgens uw vereisten:
    * _RecognitionMode.Interactive_ ondersteunt maximaal 15 seconden aanvragen.
    * _RecognitionMode.Conversation_ en _RecognitionMode.Dictation_ (beide zijn equivalent daarvan in de Custom Speech Service) ondersteuningsaanvragen maximaal tien minuten.

3. Bouw het voorbeeld met behulp van 'build gulp' voordat u deze kunt gebruiken.

> [!NOTE]
> Zorg ervoor dat u de juiste URI voor dit protocol gebruiken. De vereiste schema is *wss* (niet *http* zoals in het clientprotocol). 

Zie voor meer informatie de [Bing Speech-API](https://docs.microsoft.com/azure/cognitive-services/speech/getstarted/getstartedclientlibraries) documentatie.

## <a name="send-requests-by-using-http"></a>Verzenden van aanvragen met behulp van HTTP

Een aanvraag verzenden naar uw aangepast eindpunt met behulp van een HTTP post is vergelijkbaar met het verzenden van een aanvraag via HTTP naar de Bing Speech-API voor Cognitive Services. Wijzig de URL om het adres van uw aangepaste implementatie weer te geven.

Er zijn enkele beperkingen voor aanvragen die worden verzonden via HTTP voor het eindpunt van de Cognitive Services spraak en de aangepaste eindpunten die zijn gemaakt met deze service. De HTTP-aanvraag kan geen gedeeltelijke resultaten retourneren tijdens de opname. Daarnaast is de duur van de aanvragen beperkt tot tien seconden totdat de audio-inhoud en 14 algehele seconden.

Volg de hetzelfde proces dat u voor de Cognitive Services Speech-API gebruiken voor het maken van een post-aanvraag.

1. Een toegangstoken verkrijgen met behulp van uw abonnements-ID. Dit token is vereist voor toegang tot het opname-eindpunt. Deze kan worden hergebruikt voor 10 minuten.

    ```
    curl -X POST --header "Ocp-Apim-Subscription-Key:<subscriptionId>" --data "" "https://westus.api.cognitive.microsoft.com/sts/v1.0/issueToken"
    ```
      De **subscriptionId** moet worden ingesteld op de abonnements-ID die u voor deze implementatie gebruiken. Het antwoord is het gewoon token die u nodig hebt voor de volgende aanvraag.

2. Audio naar het eindpunt opnieuw via POST plaatsen.

    ```
    curl -X POST --data-binary @example.wav -H "Authorization: Bearer <token>" -H "Content-Type: application/octet-stream" "<https_endpoint>"
    ```

    De **token** is het toegangstoken dat u hebt ontvangen bij de vorige aanroep. De **https_endpoint** is het volledige adres van uw aangepaste spraak-naar-tekst-eindpunt, die wordt weergegeven op de **informatie over de implementatie** pagina.

Zie voor meer informatie over HTTP post-parameters en de indeling van het antwoord het [Microsoft Cognitive Services Bing Speech HTTP API](https://www.microsoft.com/cognitive-services/speech-api/documentation/API-Reference-REST/BingVoiceRecognition#SampleImplementation).

## <a name="send-requests-by-using-the-service-library"></a>Verzenden van aanvragen met behulp van de Service-bibliotheek
De Service-Servicebibliotheek kunt u uw service te gebruiken van de cloud van Microsoft Speech transcriptie gesproken taal converteren naar tekst in realtime, zodat uw client-app kan audio verzenden en ontvangen gedeeltelijke herkenningsresultaten back-tegelijkertijd en asynchroon uitgevoerd. Details van de SDK-Service kan worden gevonden [hier](https://docs.microsoft.com/azure/cognitive-services/speech/getstarted/getstartedcsharpservicelibrary)

> [!NOTE]
> Wanneer met behulp van de Service-bibliotheek hebt om te wijzigen van de URI van de autorisatieprovider in de implementatie van **IAuthorizationProvider** naar https://westus.api.cognitive.microsoft.com/sts/v1.0/issueToken.

## <a name="next-steps"></a>Volgende stappen
* De nauwkeurigheid vergroten met uw [aangepast akoestisch model](cognitive-services-custom-speech-create-acoustic-model.md).
* De nauwkeurigheid vergroten met een [aangepast taalmodel](cognitive-services-custom-speech-create-language-model.md).
