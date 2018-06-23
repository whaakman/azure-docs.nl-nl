---
title: Gebruik een aangepaste spraak-eindpunt met aangepaste spraak-Service op Azure | Microsoft Docs
description: Informatie over het gebruik van een aangepaste spraak naar tekst-eindpunt met de aangepaste spraak-Service in cognitieve Services.
services: cognitive-services
author: PanosPeriorellis
manager: onano
ms.service: cognitive-services
ms.component: custom-speech
ms.topic: article
ms.date: 02/08/2017
ms.author: panosper
ms.openlocfilehash: d28065d7962ee660cafd4b3321abdd6a8f94abcb
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344551"
---
# <a name="use-a-custom-speech-to-text-endpoint"></a>Gebruik een aangepaste spraak naar tekst-eindpunt
U kunt aanvragen verzenden naar een eindpunt van de spraak naar tekst Azure aangepaste spraak Service op soortgelijke wijze als aan het standaardeindpunt van cognitieve Services spraak. Deze eindpunten zijn identiek aan de Standaardeindpunten van de Speech-API. Dus is dezelfde functionaliteit die beschikbaar is via de clientbibliotheek of de REST-API voor de Speech-API ook beschikbaar voor uw aangepaste eindpunt.

De eindpunten die u maakt met behulp van deze service kunnen verschillende aantallen gelijktijdige aanvragen worden verwerkt. Het volume is afhankelijk van de prijscategorie die is gekoppeld aan uw abonnement. Als er te veel aanvragen worden ontvangen, wordt er een fout optreedt. Gratis laag heeft een maandelijkse limiet van aanvragen.

De service wordt ervan uitgegaan dat de gegevens in realtime worden verzonden. Als het sneller wordt verzonden, kan de aanvraag wordt beschouwd als uitgevoerd totdat de audio duur in realtime is verstreken.

> [!NOTE]
> We bieden ondersteuning voor de [nieuwe websockets](https://docs.microsoft.com/azure/cognitive-services/speech/api-reference-rest/websocketprotocol) nog. Als u van plan bent een websockets gebruiken met het eindpunt van uw aangepaste spraak, volgt u de volgende instructies.
>
> De nieuwe [REST-API](https://docs.microsoft.com/azure/cognitive-services/speech/getstarted/getstartedrest) ondersteuning is binnenkort beschikbaar. Als u van plan bent om aan te roepen van uw aangepaste spraak eindpunt via HTTP, volgt u de volgende instructies.
>

## <a name="send-requests-by-using-the-speech-client-library"></a>Verzenden van aanvragen met behulp van de clientbibliotheek spraak

Als u wilt aanvragen verzenden naar uw aangepaste eindpunt met behulp van de clientbibliotheek spraak, start u de opname-client. Gebruik de spraak Client SDK uit [NuGet](http://nuget.org/). Zoeken naar *spraakherkenning*, en selecteer het pakket van de erkenning spraak van Microsoft voor uw platform. Sommige voorbeeldcode vindt u op [GitHub](https://github.com/Microsoft/Cognitive-Speech-STT-Windows). De Client spraak SDK biedt een klasse factory **SpeechRecognitionServiceFactory**, die biedt de volgende methoden:

  *   ```CreateDataClient(...)```: Een client die gegevens herkenning.
  *   ```CreateDataClientWithIntent(...)```: Een gegevens voor client met opzet.
  *   ```CreateMicrophoneClient(...)```: Een microfoon opname-client.
  *   ```CreateMicrophoneClientWithIntent(...)```: Een microfoon erkenning client met opzet.

Zie voor gedetailleerde documentatie, de [Bing Speech-API](https://docs.microsoft.com/azure/cognitive-services/speech/home). De aangepaste spraak Service-eindpunten ondersteuning voor de dezelfde SDK.

De gegevens voor client is geschikt voor spraakherkenning van gegevens, zoals een bestand of een andere bron audio. De microfoon opname-client is geschikt voor spraakherkenning van de microfoon. Het gebruik van het doel in een client van kunt terugkeren gestructureerde opzet resultaten van de [Language Understanding Intelligent Service](https://www.luis.ai/) (LUIS), als u een toepassing LUIS hebt gemaakt voor uw scenario.

Alle vier typen clients kunnen op twee manieren worden gemaakt. De eerste manier gebruikt de API standaard cognitieve Services spraak. De tweede manier kunt u een URL opgeven die overeenkomt met uw aangepaste gemaakt met de aangepaste spraak-Service-eindpunt.

Bijvoorbeeld, kunt u een **DataRecognitionClient** die verzendt aanvragen naar een aangepaste eindpunt met behulp van de volgende methode:

```csharp
public static DataRecognitionClient CreateDataClient(SpeeechRecognitionMode speechRecognitionMode, string language, string primaryOrSecondaryKey, **string url**);
```

De **your_subscriptionId** en **endpointURL** verwijzen naar de abonnementssleutel en de URL sockets, respectievelijk op de **implementatiegegevens** pagina.

De **AuthenticationUri** wordt gebruikt voor het ontvangen van een token van de verificatieservice. Deze URI moet afzonderlijk weergegeven in de volgende voorbeeldcode worden ingesteld.

Deze voorbeeldcode laat zien hoe de SDK-client gebruiken:

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
> Als u werkt met **maken** methoden in de SDK, moet u de abonnements-ID opgeven tweemaal vanwege overbelasting van de **maken** methoden.
>

De aangepaste spraak-Service gebruikt twee verschillende URL's voor opname in verkorte vorm en lange vorm. Beide worden vermeld op de **implementaties** pagina. Gebruik de juiste eindpunt-URL voor het formulier dat u wilt gebruiken.

Zie voor meer informatie over de verschillende clients erkenning met uw aangepaste eindpunt wordt aangeroepen, de [SpeechRecognitionServiceFactory](https://www.microsoft.com/cognitive-services/Speech-api/documentation/GetStarted/GetStartedCSharpDesktop) klasse. De documentatie op deze pagina verwijst naar akoestisch model aanpassing, maar dit van toepassing op alle eindpunten die zijn gemaakt met behulp van de aangepaste spraak-Service.

## <a name="send-requests-by-using-the-speech-protocol"></a>Aanvragen verzenden via het Protocol spraak

De eindpunten weergegeven voor de [spraak Protocol](https://docs.microsoft.com/azure/cognitive-services/speech/api-reference-rest/websocketprotocol) eindpunten zijn voor de Open Source Web Socket spraak-Protocol.

De clientimplementatie van de alleen officiële is momenteel voor [JavaScript](https://github.com/Azure-Samples/SpeechToText-WebSockets-Javascript). Als u wilt beginnen met het voorbeeld dat is opgegeven, de volgende wijzigingen aanbrengen in de code en opnieuw opbouwen van de steekproef:

1. In _src\sdk\speech.browser\SpeechConnectionFactory.ts_, de host-naam 'wss://speech.platform.bing.com"vervangen door de hostnaam die wordt weergegeven op de detailpagina van uw implementatie. Voeg hier de volledige URI maar alleen de geen *wss* protocol schema en de hostnaam. Bijvoorbeeld:

    ```JavaScript
    private get Host(): string {
        return Storage.Local.GetOrAdd("Host", "wss://<your_key_goes_here>.api.cris.ai");
    }
    ```

2. Stel de _recognitionMode_ parameter in _samples\browser\Samples.html_ volgens uw vereisten:
    * _RecognitionMode.Interactive_ ondersteunt maximaal 15 seconden aanvragen.
    * _RecognitionMode.Conversation_ en _RecognitionMode.Dictation_ (zowel vergelijkbaar zijn met het aangepaste spraak Service) ondersteuning voor maximaal 10 minuten aanvragen.

3. Het voorbeeld bouwen met behulp van 'build gulp' voordat u deze kunt gebruiken.

> [!NOTE]
> Zorg ervoor dat u de juiste URI voor dit protocol. Het vereiste schema is *wss* (geen *http* zoals in het clientprotocol). 

Zie voor meer informatie de [Bing Speech-API](https://docs.microsoft.com/azure/cognitive-services/speech/getstarted/getstartedclientlibraries) documentatie.

## <a name="send-requests-by-using-http"></a>Verzenden van aanvragen met behulp van HTTP

Een aanvraag verzonden naar uw aangepaste eindpunt met behulp van een HTTP post is vergelijkbaar met het verzenden van een aanvraag via HTTP naar de cognitieve Services Bing Speech-API. Wijzig de URL, zodat het adres van uw aangepaste implementatie.

Er zijn enkele beperkingen voor aanvragen die worden verzonden via HTTP voor het eindpunt cognitieve Services spraak en de aangepaste eindpunten is gemaakt met deze service. De HTTP-aanvraag kan niet gedeeltelijke resultaten geretourneerd tijdens het herkenningsproces. Bovendien is de duur van de aanvragen beperkt tot tien seconden totdat de audio-inhoud en 14 algehele seconden.

Volg dezelfde procedure die u voor de cognitieve Services Speech-API gebruikt voor het maken van een post-aanvraag.

1. Een toegangstoken verkrijgen via uw abonnements-ID. Dit token is vereist voor toegang tot de opname-eindpunt. De opnieuw kan worden gebruikt voor 10 minuten.

    ```
    curl -X POST --header "Ocp-Apim-Subscription-Key:<subscriptionId>" --data "" "https://westus.api.cognitive.microsoft.com/sts/v1.0/issueToken"
    ```
      De **subscriptionId** moet worden ingesteld op de abonnements-ID die u voor deze implementatie gebruikt. Het antwoord is het normaal token die u nodig hebt voor de volgende aanvraag.

2. Boekt audio naar het eindpunt opnieuw via POST.

    ```
    curl -X POST --data-binary @example.wav -H "Authorization: Bearer <token>" -H "Content-Type: application/octet-stream" "<https_endpoint>"
    ```

    De **token** is het toegangstoken dat u hebt ontvangen bij de vorige aanroep. De **https_endpoint** is het volledig adres van uw aangepaste spraak naar tekst-eindpunt worden weergegeven op de **implementatiegegevens** pagina.

Zie voor meer informatie over HTTP post-parameters en indeling van het antwoord, de [Microsoft cognitieve Services Bing Speech HTTP API](https://www.microsoft.com/cognitive-services/speech-api/documentation/API-Reference-REST/BingVoiceRecognition#SampleImplementation).

## <a name="send-requests-by-using-the-service-library"></a>Verzenden van aanvragen via de Service-bibliotheek
De Service-bibliotheek biedt uw service om te kunnen gebruiken van de cloud van Microsoft Speech schrijffouten gesproken converteren naar tekst in realtime, zodat uw client-app kunt audio verzenden en ontvangen herkenningsresultaten gedeeltelijke back-tegelijk en asynchroon. Details van de SDK-Service vindt [hier](https://docs.microsoft.com/azure/cognitive-services/speech/getstarted/getstartedcsharpservicelibrary)

> [!NOTE]
> Wanneer met behulp van de Service-bibliotheek hebt u de URI van de autorisatieprovider in de implementatie van wijzigen **IAuthorizationProvider** naar https://westus.api.cognitive.microsoft.com/sts/v1.0/issueToken.

## <a name="next-steps"></a>Volgende stappen
* De nauwkeurigheid vergroten met uw [aangepaste akoestisch model](cognitive-services-custom-speech-create-acoustic-model.md).
* De nauwkeurigheid vergroten met een [aangepaste taalmodel](cognitive-services-custom-speech-create-language-model.md).
