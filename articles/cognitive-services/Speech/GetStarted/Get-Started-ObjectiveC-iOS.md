---
title: Aan de slag met de Microsoft Speech Recognition API in Objective-C op iOS | Microsoft Docs
description: Gebruik de Microsoft Speech Recognition API voor het ontwikkelen van iOS-toepassingen die gesproken audio naar tekst wilt converteren.
services: cognitive-services
author: zhouwangzw
manager: wolfma
ms.service: cognitive-services
ms.component: bing-speech
ms.topic: article
ms.date: 09/29/2017
ms.author: zhouwang
ms.openlocfilehash: bbb8d3975cdab537135b97ca9bbf6e845aa3fa0e
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344693"
---
# <a name="get-started-with-the-speech-recognition-api-in-objective-c-on-ios"></a>Aan de slag met de erkenning Speech-API in Objective-C op iOS

U kunt iOS-toepassingen die gebruikmaken van cloud-gebaseerde spraak Service gesproken audio converteren naar tekst ontwikkelen met de API van de erkenning spraak. De API ondersteunt realtime streaming, zodat uw toepassing kunnen tegelijkertijd en asynchroon gedeeltelijke resultaten krijgt op hetzelfde moment het audio naar de service wordt verzonden.

Dit artikel wordt een voorbeeld van een toepassing gebruikt voor het demonstreren van de basisbeginselen van het aan de slag met de erkenning Speech-API voor het ontwikkelen van een iOS-toepassing. Zie voor een volledig overzicht van de API de [spraak SDK-client verwijzing naar de bibliotheek](https://cdn.rawgit.com/Microsoft/Cognitive-Speech-STT-iOS/master/com.Microsoft.SpeechSDK-1_0-for-iOS.docset/Contents/Resources/Documents/index.html).

## <a name="prerequisites"></a>Vereisten

### <a name="platform-requirements"></a>Platformvereisten

Zorg ervoor dat de Mac XCode IDE is geïnstalleerd.

### <a name="get-the-client-library-and-examples"></a>Ophalen van de client-bibliotheek en voorbeelden

De clientbibliotheek spraak en voorbeelden voor iOS zijn beschikbaar op de [spraak client-SDK voor iOS](https://github.com/microsoft/cognitive-speech-stt-ios).

### <a name="subscribe-to-the-speech-recognition-api-and-get-a-free-trial-subscription-key"></a>Abonneer u op de erkenning Speech-API en de sleutel voor een gratis proefabonnement ophalen

De Speech-API maakt deel uit van cognitieve Services (eerder Project Oxford). Krijgt u gratis proefabonnement sleutels van de [abonnement cognitieve Services](https://azure.microsoft.com/try/cognitive-services/) pagina. Nadat u de Speech-API selecteert, selecteert u **API-sleutel ophalen** ophalen van de sleutel. Het resultaat een primaire en secundaire sleutel. Beide sleutels zijn gekoppeld aan hetzelfde contingent, zodat u van sleutel kunt gebruiken.

Als u wilt gebruiken *herkenning met opzet*, moet u ook aanmelden voor de [Language Understanding Intelligent Service (LUIS)](https://azure.microsoft.com/services/cognitive-services/language-understanding-intelligent-service/).

> [!IMPORTANT]
> * De abonnementssleutel voor een ophalen. Voordat u spraak clientbibliotheken gebruiken kunt, moet u beschikken over een [abonnementssleutel](https://azure.microsoft.com/try/cognitive-services/).
>
> * De abonnementssleutel van uw worden gebruikt. Met de opgegeven iOS-voorbeeldtoepassing moet u het bestand Samples/SpeechRecognitionServerExample/settings.plist bijwerken met de abonnementssleutel van uw. Zie voor meer informatie [bouwen en uitvoeren van steekproeven](#build-and-run-samples).

## <a name="use-the-speech-client-library"></a>Gebruik de clientbibliotheek spraak

Als u wilt toevoegen de clientbibliotheek in XCode-project, volgt u deze [instructies](https://github.com/Azure-Samples/Cognitive-Speech-STT-iOS#the-client-library).

De client verwijzing naar de bibliotheek voor iOS vindt dit [webpagina](https://cdn.rawgit.com/Microsoft/Cognitive-Speech-STT-iOS/master/com.Microsoft.SpeechSDK-1_0-for-iOS.docset/Contents/Resources/Documents/index.html).

## <a name="build-and-run-samples"></a>Bouwen en uitvoeren van voorbeelden

Zie voor informatie over het bouwen en uitvoeren voorbeelden [Leesmij pagina](https://github.com/Azure-Samples/Cognitive-Speech-STT-iOS#the-sample).

## <a name="samples-explained"></a>Voorbeelden uitgelegd

### <a name="create-recognition-clients"></a>Maken voor clients

De volgende code in het voorbeeld ziet u hoe erkenning clientklassen op basis van gebruikersscenario maken:

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

De clientbibliotheek biedt vooraf geïmplementeerde erkenning clientklassen voor typische scenario's bij spraakherkenning kan:

* `DataRecognitionClient`: Spraakherkenning met PCM-gegevens (bijvoorbeeld van een bestand of de audio-bron). De gegevens wordt opgedeeld in buffers en elke buffer naar-spraak-Service is verzonden. Er is geen wijziging wordt gedaan aan de buffers, zodat gebruikers hun eigen Stiltedetectie toepassen kunnen, indien gewenst. Als de gegevens wordt geleverd door WAV-bestanden, kunt u gegevens vanaf de rechterkant van het bestand met de server verzenden. Als u hebt de onbewerkte gegevens, bijvoorbeeld audio binnenkomt via Bluetooth, u eerst een indeling-header verzenden naar de server die wordt gevolgd door de gegevens.
* `MicrophoneRecognitionClient`: Spraakherkenning met audio afkomstig is van de microfoon. Zorg ervoor dat de microfoon is ingeschakeld en dat de gegevens van de microfoon naar de erkenning spraak service wordt verzonden. Een ingebouwde 'stilte detectie' wordt toegepast op de microfoon gegevens voordat deze wordt verzonden naar de opname-service.
* `DataRecognitionClientWithIntent` en `MicrophoneRecognitionClientWithIntent`: naast de tekst voor herkenning, deze clients gestructureerde gegevens over het doel van de spreker dat uw toepassingen gebruiken kunnen om verdere acties retourneren. Als u wilt gebruiken 'Bedoeling', moet u eerst een model te trainen via [LUIS](https://azure.microsoft.com/services/cognitive-services/language-understanding-intelligent-service/).

### <a name="recognition-language"></a>Taal

Als u werkt met `SpeechRecognitionServiceFactory` voor het maken van de client, moet u een taal selecteren. Zie voor de volledige lijst met talen die worden ondersteund door de Service-spraak, [ondersteunde talen](../API-Reference-REST/supportedlanguages.md).

### <a name="speechrecognitionmode"></a>SpeechRecognitionMode

U moet ook opgeven `SpeechRecognitionMode` bij het maken van de client met `SpeechRecognitionServiceFactory`:

* `SpeechRecognitionMode_ShortPhrase`: Er is een utterance maximaal 15 seconden lang zijn. Wanneer gegevens worden verzonden naar de service, ontvangt de client meerdere gedeeltelijke resultaten en één eindresultaat meerdere n beste keuze.
* `SpeechRecognitionMode_LongDictation`: Er is een utterance tot twee minuten lang. Wanneer gegevens worden verzonden naar de service, ontvangt de client meerdere gedeeltelijke resultaten en meerdere laatste resultaten op basis van waar de server zin gepauzeerd identificeert.

### <a name="attach-event-handlers"></a>Gebeurtenis-handlers koppelen

U kunt diverse gebeurtenis-handlers koppelen aan de client hebt gemaakt:

* **Gedeeltelijke resultaten gebeurtenissen**: deze gebeurtenis wordt aangeroepen telkens wanneer die Service spraak wat u mogelijk worden gezegd voorspelt, voordat u klaar bent met het spreken (als u `MicrophoneRecognitionClient`) of eindigen met het verzenden van gegevens (als u `DataRecognitionClient`).
* **Foutgebeurtenissen**: wordt aangeroepen wanneer de service een fout wordt gedetecteerd.
* **Opzet gebeurtenissen**: aangeroepen op clients 'WithIntent' (alleen in de modus ShortPhrase) na de laatste opname resultaat in een gestructureerde JSON wordt intentie wordt geparseerd.
* **Gebeurtenissen leiden**:
  * In `SpeechRecognitionMode_ShortPhrase` modus, deze gebeurtenis wordt aangeroepen en n beste resultaten retourneert nadat u klaar bent met het spreken.
  * In `SpeechRecognitionMode_LongDictation` modus wordt de gebeurtenis-handler is meermaals aangeroepen, op basis van waar de service zin onderbroken identificeert.
  * **Voor elk van de keuzes n beste**, een waarde vertrouwen en een aantal verschillende vormen van de herkende tekst worden geretourneerd. Zie voor meer informatie [uitvoerindeling](../Concepts.md#output-format).

## <a name="related-topics"></a>Verwante onderwerpen

* [De verwijzing naar de client bibliotheek voor iOS](https://cdn.rawgit.com/Microsoft/Cognitive-Speech-STT-iOS/master/com.Microsoft.SpeechSDK-1_0-for-iOS.docset/Contents/Resources/Documents/index.html)
* [Aan de slag met Microsoft-spraakherkenning en/of de bedoeling in Java voor Android](GetStartedJavaAndroid.md)
* [Aan de slag met de Microsoft Speech-API in JavaScript](GetStartedJSWebsockets.md)
* [Aan de slag met de Microsoft Speech-API via REST](GetStartedREST.md)
