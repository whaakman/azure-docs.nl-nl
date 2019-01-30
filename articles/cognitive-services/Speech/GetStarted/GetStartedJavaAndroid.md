---
title: Aan de slag met de Microsoft Speech-API in Java op Android | Microsoft Docs
titlesuffix: Azure Cognitive Services
description: Gebruik de Microsoft Speech-API voor het ontwikkelen van Android-toepassingen die Converteer gesproken audio naar tekst.
services: cognitive-services
author: zhouwangzw
manager: wolfma
ms.service: cognitive-services
ms.subservice: bing-speech
ms.topic: article
ms.date: 09/18/2018
ms.author: zhouwang
ms.openlocfilehash: 3d7bbdb25815027625b6f56b25e64c4a07b3728f
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55222487"
---
[!INCLUDE [Deprecation note](../../../../includes/cognitive-services-bing-speech-api-deprecation-note.md)]

# <a name="quickstart-use-the-bing-speech-recognition-api-in-java-on-android"></a>Snelstart: De opname van de Bing Speech-API in Java op Android gebruiken

Met de Bing Speech-API, kunt u Android-toepassingen die gebruikmaken van de cloud gebaseerde Bing Speech-Service naar gesproken audio naar tekst converteren ontwikkelen. De API ondersteunt realtime streaming, zodat uw toepassing kunt tegelijkertijd en asynchroon gedeeltelijke herkenningsresultaten ontvangen op hetzelfde moment het verzendt audio naar de service.

In dit artikel wordt een voorbeeldtoepassing te laten zien hoe de Speech client-bibliotheek voor Android gebruiken voor het ontwikkelen van toepassingen van de spraak-naar-tekst in Java voor Android-apparaten.

## <a name="prerequisites"></a>Vereisten

### <a name="platform-requirements"></a>Platformvereisten

Het voorbeeld is ontwikkeld door [Android Studio](http://developer.android.com/sdk/index.html) voor Windows in Java.

### <a name="get-the-client-library-and-sample-application"></a>De client-bibliotheek en voorbeeld-toepassing ophalen

De Speech client-bibliotheek en de voorbeelden voor Android zijn beschikbaar in de [Speech client-SDK voor Android](https://github.com/microsoft/cognitive-speech-stt-android). U vindt het buildable voorbeeld in de map samples/SpeechRecoExample. U vindt de twee bibliotheken die u wilt gebruiken in uw eigen apps in SpeechSDK/bibliotheken onder de armeabi en de x86 map. De grootte van het bestand libandroid_platform.so 22 MB is, maar deze wordt teruggebracht tot 4 MB tijdens de implementatie.

#### <a name="subscribe-to-the-speech-api-and-get-a-free-trial-subscription-key"></a>Abonneer u op de spraak-API en een gratis proefabonnement-sleutel ophalen

De spraak-API maakt deel uit van Cognitive Services (eerder Project Oxford). U krijgt een gratis proefabonnement sleutels uit de [Cognitive Services-abonnement](https://azure.microsoft.com/try/cognitive-services/) pagina. Nadat u de spraak-API selecteert, selecteert u **API-sleutel ophalen** om op te halen van de sleutel. Het resultaat een primaire en secundaire sleutel. Beide sleutels zijn gekoppeld aan dezelfde quota, zodat u beide sleutels kunt gebruiken.

Als u wilt gebruiken *erkenning met opzet*, moet u ook voor het aanmelden bij de [Language Understanding Intelligent Service (LUIS)](https://azure.microsoft.com/services/cognitive-services/language-understanding-intelligent-service/).

> [!IMPORTANT]
>* De abonnementssleutel van een ophalen. Voordat u de spraak-clientbibliotheken gebruiken kunt, moet u beschikken over een [abonnementssleutel](https://azure.microsoft.com/try/cognitive-services/).
>
>* Gebruik uw abonnement. Bijwerken met de opgegeven Android voorbeeldtoepassing, de samples/SpeechRecoExample/res/values/strings.xml bestand met de abonnementssleutels van uw. Zie voor meer informatie, [bouwen en uitvoeren van voorbeelden](#build-and-run-samples).

## <a name="use-the-speech-client-library"></a>Gebruik de Speech client-bibliotheek

Voor het gebruik van de clientbibliotheek in uw toepassing, gaat u als volgt de [instructies](https://github.com/microsoft/cognitive-speech-stt-android#the-client-library).

U vindt de client naslaginformatie over de clientbibliotheek voor Android in de docs-map van de [Speech client-SDK voor Android](https://github.com/microsoft/cognitive-speech-stt-android).

## <a name="build-and-run-samples"></a>Bouwen en uitvoeren van voorbeelden

Zie voor meer informatie over het bouwen en uitvoeren van voorbeelden, dit [Leesmij-bestand pagina](https://github.com/microsoft/cognitive-speech-stt-android#the-sample).

## <a name="samples-explained"></a>Voorbeelden uitgelegd

### <a name="create-recognition-clients"></a>Opname-clients maken

De code in het volgende voorbeeld laat zien hoe spraakherkenning clientklassen op basis van gebruiker scenario's maken:

```java
void initializeRecoClient()
    {
        String language = "en-us";

        String subscriptionKey = this.getString(R.string.subscription_key);
        String luisAppID = this.getString(R.string.luisAppID);
        String luisSubscriptionID = this.getString(R.string.luisSubscriptionID);

        if (m_isMicrophoneReco && null == m_micClient) {
            if (!m_isIntent) {
                m_micClient = SpeechRecognitionServiceFactory.createMicrophoneClient(this,
                                                                                     m_recoMode,
                                                                                     language,
                                                                                     this,
                                                                                     subscriptionKey);
            }
            else {
                MicrophoneRecognitionClientWithIntent intentMicClient;
                intentMicClient = SpeechRecognitionServiceFactory.createMicrophoneClientWithIntent(this,
                                                                                                   language,
                                                                                                   this,
                                                                                                   subscriptionKey,
                                                                                                   luisAppID,
                                                                                                   luisSubscriptionID);
                m_micClient = intentMicClient;

            }
        }
        else if (!m_isMicrophoneReco && null == m_dataClient) {
            if (!m_isIntent) {
                m_dataClient = SpeechRecognitionServiceFactory.createDataClient(this,
                                                                                m_recoMode,
                                                                                language,
                                                                                this,
                                                                                subscriptionKey);
            }
            else {
                DataRecognitionClientWithIntent intentDataClient;
                intentDataClient = SpeechRecognitionServiceFactory.createDataClientWithIntent(this,
                                                                                              language,
                                                                                              this,
                                                                                              subscriptionKey,
                                                                                              luisAppID,
                                                                                              luisSubscriptionID);
                m_dataClient = intentDataClient;
            }
        }
    }

```

De clientbibliotheek biedt vooraf geïmplementeerde erkenning clientklassen voor gebruikelijke scenario's van spraakherkenning:

* `DataRecognitionClient`: Spraakherkenning met PCM-gegevens (bijvoorbeeld van de bron van een bestand of audio). De gegevens wordt opgedeeld in buffers en elke buffer naar spraak-Service is verzonden. Geen aanpassing is gereed om terug te de buffers, zodat de gebruiker de Stiltedetectie van hun eigen toepassen kunt, indien gewenst. Als de gegevens wordt geleverd door WAV-bestanden, kunt u gegevens vanaf de rechterkant van het bestand naar spraak-Service verzenden. Als u hebt de onbewerkte gegevens, bijvoorbeeld audio die afkomstig zijn via Bluetooth, u eerst een indeling-header verzenden naar Spraakservice gevolgd door de gegevens.
* `MicrophoneRecognitionClient`: Spraakherkenning met audio die afkomstig zijn van de microfoon. Zorg ervoor dat de microfoon is ingeschakeld en de gegevens van de microfoon is verzonden naar de spraakherkenningsservice. Een ingebouwde "stiltedetector' wordt toegepast op de microfoon gegevens voordat deze wordt verzonden naar de opname-service.
* `DataRecognitionClientWithIntent` en `MicrophoneRecognitionClientWithIntent`: Deze clients retourneren naast herkenning van tekst, gestructureerde informatie over het doel van de spreker, die kan worden gebruikt om verdere acties te stimuleren door uw toepassingen. Voor het gebruik van 'Doel', moet u eerst een model met behulp van de trein [LUIS](https://azure.microsoft.com/services/cognitive-services/language-understanding-intelligent-service/).

### <a name="recognition-language"></a>OCR-taal

Bij het gebruik `SpeechRecognitionServiceFactory` voor het maken van de client, moet u een taal selecteren. Zie voor de volledige lijst met talen die worden ondersteund door de Service voor spraak, [ondersteunde talen](../API-Reference-REST/supportedlanguages.md).

### `SpeechRecognitionMode`

U moet ook opgeven `SpeechRecognitionMode` bij het maken van de client met `SpeechRecognitionServiceFactory`:

* `ShortPhrase`: Een utterance tot maximaal 15 seconden lang. Gegevens worden verzonden naar de service, ontvangt de client meerdere gedeeltelijke resultaten en één eindresultaat met meerdere n-beste keuzen.
* `LongDictation`: Een utterance tot twee minuten lang. Gegevens worden verzonden naar de service, ontvangt de client meerdere gedeeltelijke resultaten en meerdere eindresultaten, op basis van waar de service pauzes in zinnen aangeeft.

### <a name="attach-event-handlers"></a>Gebeurtenis-handlers koppelen

U kunt verschillende gebeurtenis-handlers koppelen aan de client u hebt gemaakt:

* **Gedeeltelijke resultaten gebeurtenissen**: Deze gebeurtenis wordt aangeroepen wanneer Spraakservice wat u mogelijk worden zeggen voorspelt, voordat u klaar bent met het spreken (als u `MicrophoneRecognitionClient`) of u klaar bent met het verzenden van gegevens (als u `DataRecognitionClient`).
* **Gebeurtenissen op foutniveau**: Wordt aangeroepen wanneer de service een fout wordt gedetecteerd.
* **Intentie gebeurtenissen**: Met de naam op "WithIntent" clients (alleen in `ShortPhrase` modus) nadat de laatste herkenningsresultaat wordt geparseerd in een gestructureerde JSON-doel.
* **Gebeurtenissen resulteren**:
  * In `ShortPhrase` modus, deze gebeurtenis wordt genoemd en n-beste resultaten retourneert nadat u klaar bent met het spreken.
  * In `LongDictation` modus wordt de gebeurtenis-handler heet meerdere keren op basis van waar de service pauzes in zinnen aangeeft.
  * **Voor elk van de n-beste keuzen**, een waarde vertrouwen en enkele andere vormen van de herkende tekst worden geretourneerd. Zie voor meer informatie, [uitvoerindeling](../Concepts.md#output-format).

## <a name="related-topics"></a>Verwante onderwerpen

* [Naslaginformatie over-clientbibliotheek voor Android](https://github.com/Azure-Samples/Cognitive-Speech-STT-Android/tree/master/docs)
* [Aan de slag met de Microsoft Speech-API in C# voor Windows in .NET](GetStartedCSharpDesktop.md)
* [Aan de slag met de Microsoft Speech-API in Objective-C in iOS](Get-Started-ObjectiveC-iOS.md)
* [Aan de slag met de Microsoft Speech-API in JavaScript](GetStartedJSWebsockets.md)
* [Aan de slag met de Microsoft Speech-API via REST](GetStartedREST.md)
