---
title: Wat is de spraakservice (preview)?
description: 'De spraak-service, onderdeel van de Microsoft Cognitive Services, koppelt zijn verschillende Azure speech services die eerder beschikbaar afzonderlijk waren netwerk: Bing Speech (die bestaat uit spraakherkenning en tekst naar spraak), aangepaste spraak en Spraakomzetting.'
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: v-jerkin
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 05/07/2018
ms.author: v-jerkin
ms.openlocfilehash: 60ff2f71766a14af17ebb1cb9d20825976471296
ms.sourcegitcommit: 1aedb52f221fb2a6e7ad0b0930b4c74db354a569
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/17/2018
ms.locfileid: "41987576"
---
# <a name="what-is-the-speech-service-preview"></a>Wat is de spraakservice (preview)?

De spraak-service biedt ontwikkelaars een eenvoudige manier om te krachtige functies voor spraak ingeschakelde toevoegen aan hun toepassingen met één abonnement. Uw apps kunnen nu voorzien van gesproken opdrachten, transcriptie, dicteren, spraaksynthese en spraakomzetting.

De spraak-service wordt mogelijk gemaakt door de technologieën die worden gebruikt in andere Microsoft-producten, met inbegrip van Cortana en Microsoft Office.

> [!NOTE]
> De spraak-service is momenteel in openbare preview. Ga terug hier regelmatig op updates naar documentatie en voorbeelden van aanvullende code.

## <a name="speech-service-features"></a>Spraak-service-functies

|Functie|Beschrijving|
|-|-|
|[Spraak-naar-tekst](speech-to-text.md)| Transcribes audiostreams naar tekst die uw toepassing als invoer accepteren kunt. Ook worden geïntegreerd met de [Language Understanding service](https://docs.microsoft.com/azure/cognitive-services/luis/) (LUIS) worden afgeleid van de bedoeling van gebruikers van uitingen.|
|[Tekst naar spraak](text-to-speech.md)| Converteert tekst zonder opmaak naar natuurlijk klinkende spraak, die worden geleverd aan uw toepassing in een geluidsbestand. Meerdere stemmen, variëren in geslacht of accent, zijn beschikbaar voor vele ondersteunde talen. |
|[Spraakomzetting](speech-translation.md)| Kan worden gebruikt voor de omzetting van streaming audio in near-real-time of opgenomen spraak te verwerken. |
|Aangepaste spraak-naar-tekst|U kunt de spraak-naar-tekst aanpassen door het maken van uw eigen [akoestische](how-to-customize-acoustic-models.md) en [taal](how-to-customize-language-model.md) -modellen en geven aangepaste [uitspraak](how-to-customize-pronunciation.md) regels. |
|[Aangepaste tekst naar spraak](how-to-customize-voice-font.md)|U kunt uw eigen stemmen voor tekst naar spraak maken.|
|[Spraak apparaten SDK](speech-devices-sdk.md)| Met de introductie van de geïntegreerde Speech-service bieden Microsoft en haar partners een geïntegreerde hardware/software-platform dat is geoptimaliseerd voor het ontwikkelen spraak ingeschakelde apparaten |

## <a name="access-to-the-speech-service"></a>Toegang tot de spraak-service

De spraak-service is beschikbaar op twee manieren. [De SDK](speech-sdk.md) de details van de netwerkprotocollen voor vereenvoudigde ontwikkeling op ondersteunde platforms volledig weggewerkt. De [REST-API](rest-apis.md) werkt met elke programmeertaal, maar biedt niet alle functies die worden aangeboden door de SDK.

|<br>Methode|Speech<br>naar tekst|Tekst die moet worden<br>Speech|Speech<br>Omzetting|<br>Beschrijving|
|-|-|-|-|-|
|[SDK 's](speech-sdk.md)|Ja|Nee|Ja|Websocket-procotol die eenvoudiger maken gebruik van bibliotheken voor specifieke programmeertalen.|
|[REST](rest-apis.md)|Ja|Ja|Nee|Een eenvoudige, door de HTTP-gebaseerde API waarmee u eenvoudig spraak toevoegen aan uw toepassing.|

## <a name="speech-scenarios"></a>Spraak-scenario 's

Enkele veelvoorkomende toepassingen van technologie voor spraak worden kort hieronder besproken. De [spraak SDK](speech-sdk.md) centraal staat in de meeste van deze scenario's.

> [!div class="checklist"]
> * Maak apps stem wordt geactiveerd
> * Transcriberen call center-opnamen
> * Stem bots implementeren

### <a name="voice-triggered-apps"></a>Apps stem wordt geactiveerd

Stem de invoer is een uitstekende manier om uw app maken voor flexibele, Handsfree en snel te gebruiken. Gebruikers kunnen alleen in een stem-app stellen voor de gegevens die ze willen in plaats van dat u hoeft te navigeren door te klikken of tikken.

Als uw app is bedoeld voor gebruik door het grote publiek, kunt u de basislijn spraakherkenning model geleverd door de Speech-service. Hiervoor wordt een goed in een groot aantal sprekers in de standaardomgevingen herkennen.

Als uw app wordt gebruikt in een specifiek domein (bijvoorbeeld arts of IT), kunt u een [taalmodel](how-to-customize-language-model.md) om te leren van de spraakservice over de speciale terminologie die wordt gebruikt door uw app.

Als uw app wordt gebruikt in een ruis omgeving, zoals een fabriek, kunt u een aangepaste [akoestisch model](how-to-customize-acoustic-models.md) beter zodat de spraak-service voor spraak onderscheiden van de ruis.

Aan de slag is net zo gemakkelijk als het downloaden van de [spraak SDK](speech-sdk.md) en relevante [snelstartgids](quickstart-csharp-dotnet-windows.md) artikel.

### <a name="transcribe-call-center-recordings"></a>Transcriberen call center-opnamen

Call center-opnamen zijn vaak alleen als er zich een probleem met een aanroep voordoet geraadpleegd. Met de Speech-service is het eenvoudig elke opnemen om tekst te transcriberen. Zodra ze tekst, kunt u deze eenvoudig indexeren [zoeken in volledige tekst](https://docs.microsoft.com/azure/search/search-what-is-azure-search) of toe te passen [Tekstanalyse](https://docs.microsoft.com/azure/cognitive-services/Text-Analytics/) aan het sentiment, taal en sleuteltermen detecteren.

Als uw call center-opnamen bevatten vaak gespecialiseerde terminologie (zoals productnamen of vaktermen IT), kunt u een [taalmodel](how-to-customize-language-model.md) om te leren van de spraakservice die vocabulaire. Een aangepaste [akoestisch model](how-to-customize-acoustic-models.md) help de spraakservice inzicht krijgt in minder dan optimale phone verbindingen.

Voor meer informatie over dit scenario, leest u meer over [batch transcriptie](batch-transcription.md) met de Speech-service.

### <a name="voice-bots"></a>Stem bots

[Bots](https://dev.botframework.com/) een steeds populairder manier om verbinding te maken van gebruikers met de informatie die ze willen, en klanten met de bedrijven die hun keuze. Een conversatie gebruikersinterface aan uw website of app toe te voegen, kunt u de functionaliteit ervan gemakkelijker te vinden en sneller toegang tot. Met de Speech-service duurt dit gesprek op een nieuwe dimensie van fluency door daadwerkelijk te reageren op gesproken query's met kunstmatige spraak.

Als u wilt een unieke persoonlijkheid toevoegen aan uw bot spraak ingeschakeld (en Versterk uw merk), krijgt het een stem van een eigen. Het maken van een aangepaste gesproken is een proces in twee stappen. Eerste, u [maken van opnamen](record-custom-voice-samples.md) van de stem die u wilt gebruiken. U [verzenden die opnamen](how-to-customize-voice-font.md) (samen met een tekstversie) met de spraak-service [stem aanpassing portal](https://cris.ai/Home/CustomVoice), dat doet de rest. Als u uw aangepaste gesproken hebt gemaakt, is het eenvoudig te gebruiken in uw app.

## <a name="next-steps"></a>Volgende stappen

Een abonnementssleutel voor de service voor spraak ophalen.

> [!div class="nextstepaction"]
> [De spraak-service gratis uitproberen](get-started.md)
