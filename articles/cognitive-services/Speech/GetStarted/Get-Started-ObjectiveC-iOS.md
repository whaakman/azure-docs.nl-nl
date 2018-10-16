---
title: Aan de slag met de Bing Speech-API in Objective-C in iOS | Microsoft Docs
titlesuffix: Azure Cognitive Services
description: De Bing Speech-API gebruiken voor het ontwikkelen van iOS-toepassingen die Converteer gesproken audio naar tekst.
services: cognitive-services
author: zhouwangzw
manager: wolfma
ms.service: cognitive-services
ms.component: bing-speech
ms.topic: article
ms.date: 09/18/2018
ms.author: zhouwang
ms.openlocfilehash: 7c4a5029208854528afdfdbfcdc63434a2a94e24
ms.sourcegitcommit: 1aacea6bf8e31128c6d489fa6e614856cf89af19
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/16/2018
ms.locfileid: "49338693"
---
# <a name="quickstart-use-the-bing-speech-recognition-api-in-objective-c-on-ios"></a>Snelstartgids: Gebruik de Bing Speech-API in Objective-C in iOS

[!INCLUDE [Deprecation note](../../../../includes/cognitive-services-bing-speech-api-deprecation-note.md)]

Met de spraak-API, kunt u iOS-toepassingen die gebruikmaken van cloud-gebaseerde Service van spraak naar gesproken audio naar tekst converteren ontwikkelen. De API ondersteunt realtime streaming, zodat uw toepassing kunt tegelijkertijd en asynchroon gedeeltelijke herkenningsresultaten ontvangen op hetzelfde moment het verzendt audio naar de service.

In dit artikel wordt een voorbeeldtoepassing ter illustratie van de basisprincipes van het aan de slag met de spraak-API voor het ontwikkelen van een iOS-toepassing. Zie voor een volledige API-verwijzing de [naslaginformatie over spraakherkenning SDK-clientbibliotheek](https://cdn.rawgit.com/Microsoft/Cognitive-Speech-STT-iOS/master/com.Microsoft.SpeechSDK-1_0-for-iOS.docset/Contents/Resources/Documents/index.html).

## <a name="prerequisites"></a>Vereisten

### <a name="platform-requirements"></a>Platformvereisten

Zorg ervoor dat de Mac XCode IDE is geïnstalleerd.

### <a name="get-the-client-library-and-examples"></a>Ophalen van de client-bibliotheek en voorbeelden

De Speech client-bibliotheek en voorbeelden voor iOS zijn beschikbaar op de [Speech client-SDK voor iOS](https://github.com/microsoft/cognitive-speech-stt-ios).

### <a name="subscribe-to-the-speech-recognition-api-and-get-a-free-trial-subscription-key"></a>Abonneer u op de spraak-API en een gratis proefabonnement-sleutel ophalen

De spraak-API maakt deel uit van Cognitive Services (eerder Project Oxford). U krijgt een gratis proefabonnement sleutels uit de [Cognitive Services-abonnement](https://azure.microsoft.com/try/cognitive-services/) pagina. Nadat u de spraak-API selecteert, selecteert u **API-sleutel ophalen** om op te halen van de sleutel. Het resultaat een primaire en secundaire sleutel. Beide sleutels zijn gekoppeld aan dezelfde quota, zodat u beide sleutels kunt gebruiken.

Als u wilt gebruiken *erkenning met opzet*, moet u ook voor het aanmelden bij de [Language Understanding Intelligent Service (LUIS)](https://azure.microsoft.com/services/cognitive-services/language-understanding-intelligent-service/).

> [!IMPORTANT]
> * De abonnementssleutel van een ophalen. Voordat u de spraak-clientbibliotheken gebruiken kunt, moet u beschikken over een [abonnementssleutel](https://azure.microsoft.com/try/cognitive-services/).
>
> * Gebruik uw abonnement. Met de opgegeven iOS-voorbeeldtoepassing moet u het bestand Samples/SpeechRecognitionServerExample/settings.plist bijwerken met de abonnementssleutel van uw. Zie voor meer informatie, [bouwen en uitvoeren van voorbeelden](#build-and-run-samples).

## <a name="use-the-speech-client-library"></a>Gebruik de Speech client-bibliotheek

Als u wilt toevoegen de clientbibliotheek in een XCode-project, volgt u deze [instructies](https://github.com/Azure-Samples/Cognitive-Speech-STT-iOS#the-client-library).

Als u zoekt de client naslaginformatie over de clientbibliotheek voor iOS, ziet deze [webpagina](https://cdn.rawgit.com/Microsoft/Cognitive-Speech-STT-iOS/master/com.Microsoft.SpeechSDK-1_0-for-iOS.docset/Contents/Resources/Documents/index.html).

## <a name="build-and-run-samples"></a>Bouwen en uitvoeren van voorbeelden

Zie voor informatie over het bouwen en uitvoeren voorbeelden [Leesmij-bestand pagina](https://github.com/Azure-Samples/Cognitive-Speech-STT-iOS#the-sample).

## <a name="samples-explained"></a>Voorbeelden uitgelegd

### <a name="create-recognition-clients"></a>Opname-clients maken

De volgende code in het voorbeeld laat zien hoe spraakherkenning clientklassen op basis van gebruiker scenario's maken:

```
{
    NSString* language = @"en-us";

    NSString* path = [[NSBundle mainBundle] pathForResource:@"settings" ofType:@"plist"];
    NSDictionary* settings = [[NSDictionary alloc] initWithContentsOfFile:path];

    NSString* primaryOrSecondaryKey = [settings objectForKey:(@"primaryKey")];
    NSString* luisAppID = [settings objectForKey:(@"luisAppID")];
    NSString* luisSubscriptionID = [settings objectForKey:(@"luisSubscriptionID")];

    if (isMicrophoneReco) {
        if (!isIntent) {
            micClient = [SpeechRecognitionServiceFactory createMicrophoneClient:(recoMode)
                                                                   withLanguage:(language)
                                                                        withKey:(primaryOrSecondaryKey)
                                                                   withProtocol:(self)];
        }
        else {
            MicrophoneRecognitionClientWithIntent* micIntentClient;
            micIntentClient = [SpeechRecognitionServiceFactory createMicrophoneClientWithIntent:(language)
                                                                                        withKey:(primaryOrSecondaryKey)
                                                                                  withLUISAppID:(luisAppID)
                                                                                 withLUISSecret:(luisSubscriptionID)
                                                                                   withProtocol:(self)];
            micClient = micIntentClient;
        }
    }
    else {
        if (!isIntent) {
            dataClient = [SpeechRecognitionServiceFactory createDataClient:(recoMode)
                                                              withLanguage:(language)
                                                                   withKey:(primaryOrSecondaryKey)
                                                              withProtocol:(self)];
        }
        else {
            DataRecognitionClientWithIntent* dataIntentClient;
            dataIntentClient = [SpeechRecognitionServiceFactory createDataClientWithIntent:(language)
                                                                                   withKey:(primaryOrSecondaryKey)
                                                                             withLUISAppID:(luisAppID)
                                                                            withLUISSecret:(luisSubscriptionID)
                                                                              withProtocol:(self)];
            dataClient = dataIntentClient;
        }
    }
}

```

De clientbibliotheek biedt vooraf geïmplementeerde erkenning clientklassen voor gebruikelijke scenario's van spraakherkenning:

* `DataRecognitionClient`: Spraakherkenning met PCM-gegevens (bijvoorbeeld van de bron van een bestand of audio). De gegevens wordt opgedeeld in buffers en elke buffer naar spraak-Service is verzonden. Geen aanpassing is gereed om terug te de buffers, zodat gebruikers hun eigen Stiltedetectie toepassen kunnen, indien gewenst. Als de gegevens wordt geleverd door WAV-bestanden, kunt u gegevens verzenden vanaf de rechterkant van het bestand met de server. Als u hebt de onbewerkte gegevens, bijvoorbeeld audio die afkomstig zijn via Bluetooth, u eerst een indeling-header verzenden naar de server die wordt gevolgd door de gegevens.
* `MicrophoneRecognitionClient`: Spraakherkenning met audio die afkomstig zijn van de microfoon. Zorg ervoor dat de microfoon is ingeschakeld en dat de gegevens van de microfoon naar de spraakherkenningsservice wordt verzonden. Een ingebouwde "stiltedetector' wordt toegepast op de microfoon gegevens voordat deze wordt verzonden naar de opname-service.
* `DataRecognitionClientWithIntent` en `MicrophoneRecognitionClientWithIntent`: naast de tekst van de spraakherkenning, retourneert deze clients gestructureerde informatie over het doel van de spreker, die uw toepassingen gebruiken kunnen om verdere acties te stimuleren. Voor het gebruik van 'Doel', moet u eerst een model met behulp van de trein [LUIS](https://azure.microsoft.com/services/cognitive-services/language-understanding-intelligent-service/).

### <a name="recognition-language"></a>OCR-taal

Bij het gebruik `SpeechRecognitionServiceFactory` voor het maken van de client, moet u een taal selecteren. Zie voor de volledige lijst met talen die worden ondersteund door de Service voor spraak, [ondersteunde talen](../API-Reference-REST/supportedlanguages.md).

### <a name="speechrecognitionmode"></a>SpeechRecognitionMode

U moet ook opgeven `SpeechRecognitionMode` bij het maken van de client met `SpeechRecognitionServiceFactory`:

* `SpeechRecognitionMode_ShortPhrase`: Een utterance tot maximaal 15 seconden lang. Gegevens worden verzonden naar de service, ontvangt de client meerdere gedeeltelijke resultaten en één eindresultaat met meerdere n-beste keuzen.
* `SpeechRecognitionMode_LongDictation`: Een utterance tot twee minuten lang. Gegevens worden verzonden naar de service, ontvangt de client meerdere gedeeltelijke resultaten en meerdere eindresultaten, op basis van waar de server pauzes in zinnen aangeeft.

### <a name="attach-event-handlers"></a>Gebeurtenis-handlers koppelen

U kunt verschillende gebeurtenis-handlers koppelen aan de client u hebt gemaakt:

* **Gedeeltelijke resultaten gebeurtenissen**: deze gebeurtenis wordt aangeroepen telkens wanneer die Speech-Service wordt voorspeld wat u mogelijk worden zeggen, voordat u klaar bent met het spreken (als u `MicrophoneRecognitionClient`) of u klaar bent met het verzenden van gegevens (als u `DataRecognitionClient`).
* **Gebeurtenissen op foutniveau**: wordt aangeroepen wanneer de service een fout wordt gedetecteerd.
* **Intentie gebeurtenissen**: met de naam op de clients 'WithIntent' (alleen in de modus ShortPhrase) na de laatste opname resultaat in een gestructureerde JSON-doel wordt geparseerd.
* **Gebeurtenissen resulteren**:
  * In `SpeechRecognitionMode_ShortPhrase` modus, deze gebeurtenis wordt genoemd en n-beste resultaten retourneert nadat u klaar bent met het spreken.
  * In `SpeechRecognitionMode_LongDictation` modus wordt de gebeurtenis-handler heet meerdere keren op basis van waar de service pauzes in zinnen aangeeft.
  * **Voor elk van de n-beste keuzen**, een waarde vertrouwen en enkele andere vormen van de herkende tekst worden geretourneerd. Zie voor meer informatie, [uitvoerindeling](../Concepts.md#output-format).

## <a name="related-topics"></a>Verwante onderwerpen

* [Naslaginformatie over-clientbibliotheek voor iOS](https://cdn.rawgit.com/Microsoft/Cognitive-Speech-STT-iOS/master/com.Microsoft.SpeechSDK-1_0-for-iOS.docset/Contents/Resources/Documents/index.html)
* [Aan de slag met Microsoft-spraakherkenning en/of kunt u lezen wat in Java op Android](GetStartedJavaAndroid.md)
* [Aan de slag met de Microsoft Speech-API in JavaScript](GetStartedJSWebsockets.md)
* [Aan de slag met de Microsoft Speech-API via REST](GetStartedREST.md)
