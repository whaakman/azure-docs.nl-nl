---
title: Aan de slag met de Microsoft Speech Recognition API in Java op Android | Microsoft Docs
description: Gebruik de Microsoft Speech-API voor het ontwikkelen van Android-toepassingen die gesproken audio naar tekst wilt converteren.
services: cognitive-services
author: zhouwangzw
manager: wolfma
ms.service: cognitive-services
ms.component: bing-speech
ms.topic: article
ms.date: 09/29/2017
ms.author: zhouwang
ms.openlocfilehash: a10f7be1c36fb431016a9867f606e26be858069e
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344696"
---
# <a name="get-started-with-speech-recognition-in-java-on-android"></a>Aan de slag met spraakherkenning in Java voor Android

U kunt Android-toepassingen die gebruikmaken van cloud-gebaseerde spraak Service gesproken audio converteren naar tekst ontwikkelen met de API van de erkenning spraak. De API ondersteunt realtime streaming, zodat uw toepassing kunnen tegelijkertijd en asynchroon gedeeltelijke resultaten krijgt op hetzelfde moment het audio naar de service wordt verzonden.

Dit artikel wordt een voorbeeld van een toepassing te laten zien hoe de spraak-clientbibliotheek voor Android gebruiken voor het ontwikkelen van toepassingen van spraak naar tekst in Java voor Android-apparaten.

## <a name="prerequisites"></a>Vereisten

### <a name="platform-requirements"></a>Platformvereisten

Het voorbeeld is ontwikkeld door [Android Studio](http://developer.android.com/sdk/index.html) voor Windows in Java.

### <a name="get-the-client-library-and-sample-application"></a>Ophalen van de client-bibliotheek en voorbeeld-toepassing

De clientbibliotheek spraak en voorbeelden voor Android zijn beschikbaar in de [spraak client SDK voor Android](https://github.com/microsoft/cognitive-speech-stt-android). U vindt het buildable voorbeeld in de map met voorbeelden/SpeechRecoExample. U vindt de twee bibliotheken die u wilt gebruiken in uw eigen SpeechSDK/bibliotheken onder de armeabi-apps en de x86 map. De grootte van het bestand libandroid_platform.so 22 MB is, maar deze wordt verkleind tot 4 MB tijdens de implementatie.

#### <a name="subscribe-to-the-speech-api-and-get-a-free-trial-subscription-key"></a>Abonneer u op de Speech-API en de sleutel voor een gratis proefabonnement ophalen

De Speech-API maakt deel uit van cognitieve Services (eerder Project Oxford). Krijgt u gratis proefabonnement sleutels van de [abonnement cognitieve Services](https://azure.microsoft.com/try/cognitive-services/) pagina. Nadat u de Speech-API selecteert, selecteert u **API-sleutel ophalen** ophalen van de sleutel. Het resultaat een primaire en secundaire sleutel. Beide sleutels zijn gekoppeld aan hetzelfde contingent, zodat u van sleutel kunt gebruiken.

Als u wilt gebruiken *herkenning met opzet*, moet u ook aanmelden voor de [Language Understanding Intelligent Service (LUIS)](https://azure.microsoft.com/services/cognitive-services/language-understanding-intelligent-service/).

> [!IMPORTANT]
>* De abonnementssleutel voor een ophalen. Voordat u spraak clientbibliotheken gebruiken kunt, moet u beschikken over een [abonnementssleutel](https://azure.microsoft.com/try/cognitive-services/).
>
>* De abonnementssleutel van uw worden gebruikt. Bijwerken van de opgegeven Android voorbeeldtoepassing de samples/SpeechRecoExample/res/values/strings.xml bestand met de sleutels van uw abonnement. Zie voor meer informatie [bouwen en uitvoeren van steekproeven](#build-and-run-samples).

## <a name="use-the-speech-client-library"></a>Gebruik de clientbibliotheek spraak

Ga als volgt met de clientbibliotheek in uw toepassing, de [instructies](https://github.com/microsoft/cognitive-speech-stt-android#the-client-library).

U vindt de client verwijzing naar de bibliotheek voor Android in de map documenten van de [spraak client SDK voor Android](https://github.com/microsoft/cognitive-speech-stt-android).

## <a name="build-and-run-samples"></a>Bouwen en uitvoeren van voorbeelden

Zie voor informatie over het bouwen en uitvoeren van voorbeelden, dit [Leesmij pagina](https://github.com/microsoft/cognitive-speech-stt-android#the-sample).

## <a name="samples-explained"></a>Voorbeelden uitgelegd

### <a name="create-recognition-clients"></a>Maken voor clients

De code in het volgende voorbeeld ziet u hoe erkenning clientklassen op basis van gebruikersscenario maken:

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

De clientbibliotheek biedt vooraf geïmplementeerde erkenning clientklassen voor typische scenario's bij spraakherkenning kan:

* `DataRecognitionClient`: Spraakherkenning met PCM-gegevens (bijvoorbeeld van een bestand of de audio-bron). De gegevens wordt opgedeeld in buffers en elke buffer naar-spraak-Service is verzonden. Er is geen wijziging wordt gedaan aan de buffers, zodat de gebruiker de Stiltedetectie van hun eigen toepassen kunt, indien gewenst. Als de gegevens wordt geleverd door WAV-bestanden, kunt u gegevens vanaf de rechterkant van het bestand naar-spraak Service verzenden. Als u hebt de onbewerkte gegevens, bijvoorbeeld audio binnenkomt via Bluetooth, u eerst een indeling-header verzenden naar-spraak Service gevolgd door de gegevens.
* `MicrophoneRecognitionClient`: Spraakherkenning met audio afkomstig is van de microfoon. Zorg ervoor dat de microfoon is ingeschakeld en de gegevens van de microfoon naar de spraak opname-service is verzonden. Een ingebouwde 'stilte detectie' wordt toegepast op de microfoon gegevens voordat deze wordt verzonden naar de opname-service.
* `DataRecognitionClientWithIntent` en `MicrophoneRecognitionClientWithIntent`: deze clients retourneren, naast de tekst voor herkenning, gestructureerde informatie over het doel van de spreker die kan worden gebruikt om verdere acties door uw toepassingen. Als u wilt gebruiken 'Bedoeling', moet u eerst een model te trainen via [LUIS](https://azure.microsoft.com/services/cognitive-services/language-understanding-intelligent-service/).

### <a name="recognition-language"></a>Taal

Als u werkt met `SpeechRecognitionServiceFactory` voor het maken van de client, moet u een taal selecteren. Zie voor de volledige lijst met talen die worden ondersteund door de Service-spraak, [ondersteunde talen](../API-Reference-REST/supportedlanguages.md).

### `SpeechRecognitionMode`

U moet ook opgeven `SpeechRecognitionMode` bij het maken van de client met `SpeechRecognitionServiceFactory`:

* `ShortPhrase`: Er is een utterance maximaal 15 seconden lang zijn. Wanneer gegevens worden verzonden naar de service, ontvangt de client meerdere gedeeltelijke resultaten en één eindresultaat meerdere n beste keuze.
* `LongDictation`: Er is een utterance tot twee minuten lang. Wanneer gegevens worden verzonden naar de service, ontvangt de client meerdere gedeeltelijke resultaten en meerdere laatste resultaten op basis van waar de service zin wordt onderbroken identificeert.

### <a name="attach-event-handlers"></a>Gebeurtenis-handlers koppelen

U kunt diverse gebeurtenis-handlers koppelen aan de client hebt gemaakt:

* **Gedeeltelijke resultaten gebeurtenissen**: deze gebeurtenis wordt aangeroepen telkens spraak Service wat u mogelijk worden gezegd voorspelt, voordat u klaar bent met het spreken (als u `MicrophoneRecognitionClient`) of eindigen met het verzenden van gegevens (als u `DataRecognitionClient`).
* **Foutgebeurtenissen**: wordt aangeroepen wanneer de service een fout wordt gedetecteerd.
* **Opzet gebeurtenissen**: aangeroepen op 'WithIntent' clients (alleen `ShortPhrase` modus) nadat het resultaat van de laatste opname in een gestructureerde JSON wordt intentie wordt geparseerd.
* **Gebeurtenissen leiden**:
  * In `ShortPhrase` modus, deze gebeurtenis wordt aangeroepen en n beste resultaten retourneert nadat u klaar bent met het spreken.
  * In `LongDictation` modus wordt de gebeurtenis-handler is meermaals aangeroepen, op basis van waar de service zin onderbroken identificeert.
  * **Voor elk van de keuzes n beste**, een waarde vertrouwen en een aantal verschillende vormen van de herkende tekst worden geretourneerd. Zie voor meer informatie [uitvoerindeling](../Concepts.md#output-format).

## <a name="related-topics"></a>Verwante onderwerpen

* [De verwijzing naar de client bibliotheek voor Android](https://github.com/Azure-Samples/Cognitive-Speech-STT-Android/tree/master/docs)
* [Aan de slag met de Microsoft Speech-API in C# voor Windows in .NET](GetStartedCSharpDesktop.md)
* [Aan de slag met de Microsoft Speech-API in Objective-C op iOS](Get-Started-ObjectiveC-iOS.md)
* [Aan de slag met de Microsoft Speech-API in JavaScript](GetStartedJSWebsockets.md)
* [Aan de slag met de Microsoft Speech-API via REST](GetStartedREST.md)
