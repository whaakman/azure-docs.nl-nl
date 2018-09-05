---
title: Wat is de Speech-service (preview-versie)?
description: "De Speech-service, onderdeel van Microsoft's Cognitive Services, verenigt verschillende Azure-spraakservices die voorheen afzonderlijk beschikbaar waren: Bing Speech (bestaande uit spraakherkenning en tekst-naar-spraak), Custom Speech en Spraakomzetting."
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: v-jerkin
ms.service: cognitive-services
ms.component: speech-service
ms.topic: overview
ms.date: 05/07/2018
ms.author: v-jerkin
ms.openlocfilehash: 922320bb0b880e933b27025257e6a533fe257680
ms.sourcegitcommit: f6e2a03076679d53b550a24828141c4fb978dcf9
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/27/2018
ms.locfileid: "43091469"
---
# <a name="what-is-the-speech-service"></a>Wat is de Speech-service?

De Speech-service verenigt de Azure-spraakfuncties die voorheen beschikbaar waren via de services [Bing Speech-API](https://docs.microsoft.com/azure/cognitive-services/speech/home), [Translator Speech](https://docs.microsoft.com/azure/cognitive-services/translator-speech/), [Custom Speech](https://docs.microsoft.com/azure/cognitive-services/custom-speech-service/cognitive-services-custom-speech-home) en [Custom Voice](http://customvoice.ai/). Eén abonnement biedt nu toegang tot al deze mogelijkheden.

Net zoals de andere Azure-spraakservices wordt de Speech-service mogelijk gemaakt door de bewezen spraaktechnologieën die worden gebruikt in producten zoals Cortana en Microsoft Office. U kunt rekenen op de kwaliteit van de resultaten en de betrouwbaarheid van de Azure-cloud.

> [!NOTE]
> De Speech-service is momenteel beschikbaar als openbare preview-versie. Kom hier regelmatig terug voor documentatie-updates, nieuwe codevoorbeelden en meer.

## <a name="main-speech-service-functions"></a>Belangrijkste functies van de Speech-service

De primaire functies van de Speech-service zijn Spraak naar tekst (ook wel spraakherkenning of transcriptie genoemd), Tekst naar spraak (spraaksynthese) en Spraakomzetting.

|Functie|Functies|
|-|-|
|[Spraak naar tekst](speech-to-text.md)| <ul><li>Transcribeert continue realtime spraak naar tekst.<li>Kan spraak uit geluidsopnamen in batches transcriberen. <li>Biedt herkenningsmodi voor interactieve, conversationele en dicterende gebruiksscenario’s.<li>Ondersteunt tussenliggende resultaten, detectie van het einde van de spraak, automatische tekstopmaak en maskering van grof taalgebruik. <li>Kan [Language Understanding](https://docs.microsoft.com/azure/cognitive-services/luis/) (LUIS) gebruiken om de bedoeling van gebruikers af te leiden van getranscribeerde spraak.\*|
|[Tekst naar spraak](text-to-speech.md)| <ul><li>Converteert tekst naar natuurlijk klinkende spraak. <li>Biedt meerdere geslachten en/of dialecten voor vele ondersteunde talen. <li>Ondersteunt de invoer van tekst zonder opmaak en Speech Synthesis Markup Language (SSML). |
|[Spraakomzetting](speech-translation.md)| <ul><li>Vertaalt streaming audio vrijwel in realtime.<li> Kan ook opgenomen spraak verwerken.<li>Levert resultaten als tekst of gesynthetiseerde spraak. |

\* *Herkenning van bedoeling vereist een LUIS-abonnement.*


## <a name="customizing-speech-features"></a>Spraakfuncties aanpassen

Met de Speech-service kunt u uw eigen gegevens gebruiken om de onderliggende modellen van de functies Spraak naar tekst en Tekst naar spraak van de Speech-service te trainen. 

|Functie|Model|Doel|
|-|-|-|
|Spraak naar tekst|[Akoestisch model](how-to-customize-acoustic-models.md)|Helpt bepaalde sprekers en omgevingen zoals auto’s of fabrieken te transcriberen|
||[Taalmodel](how-to-customize-language-model.md)|Helpt veldspecifiek vocabulaire en grammatica zoals medisch of IT-jargon te transcriberen|
||[Uitspraakmodel](how-to-customize-pronunciation.md)|Helpt afkortingen en acroniemen zoals ‘IOU’ voor ‘I owe you’ te transcriberen |
|Tekst naar spraak|[Spraakstijl](how-to-customize-voice-font.md)|Geeft uw app zijn eigen stem door het model te trainen in voorbeelden van menselijke spraak.|

Zodra uw aangepaste modellen zijn gemaakt, kunnen ze overal worden gebruikt waar u de standaardmodellen zou gebruiken in de functie Spraak naar tekst of Tekst naar spraak van uw app.


## <a name="using-the-speech-service"></a>De Speech-service gebruiken

Om de ontwikkeling van apps met spraakmogelijkheden te vereenvoudigen, biedt Microsoft de [Speech-SDK](speech-sdk.md) voor gebruik met de nieuwe Speech-service. De Speech-SDK biedt consistente systeemeigen API’s voor Spraak naar tekst en Spraakomzetting voor C#, C++ en Java. Als u met een van deze talen ontwikkelt, maakt de Speech-SDK het ontwikkelen gemakkelijker door de netwerkgegevens voor u te verwerken.

De Speech-service heeft ook een [REST-API](rest-apis.md) die werkt met elke programmeertaal die HTTP-verzoeken kan indienen. De REST-interface biedt echter niet de streaming, realtime functionaliteit die de SDK biedt.

|<br>Methode|Speech<br>naar tekst|Tekst naar<br>Speech|Speech<br>Omzetting|<br>Beschrijving|
|-|-|-|-|-|
|[Speech-SDK](speech-sdk.md)|Ja|Nee|Ja|Systeemeigen API’s voor C#, C++ en Java om ontwikkeling te vereenvoudigen.|
|[REST](rest-apis.md)|Ja|Ja|Nee|Een eenvoudige HTTP-gebaseerde API die het gemakkelijk maakt spraak aan uw apps toe te voegen.|

### <a name="websockets"></a>WebSockets

De Speech-service heeft ook WebSockets-protocollen om Spraak naar tekst en Spraakomzetting te streamen. De Speech-SDK’s gebruiken deze protocollen om met de Speech-service te communiceren. U moet de Speech-SDK gebruiken in plaats van uw eigen WebSockets-communicatie proberen te implementeren met de Speech-service.

Als u echter al code hebt die Bing Speech of Translator Speech via WebSockets gebruikt, kan deze gemakkelijk worden bijgewerkt om de Speech-service te gebruiken. De WebSockets-protocollen zijn compatibel; alleen de eindpunten zijn anders.

### <a name="speech-devices-sdk"></a>Speech Devices SDK

De [Speech Devices SDK](speech-devices-sdk.md) is een geïntegreerd hardware- en softwareplatform voor ontwikkelaars van apparaten met spraakmogelijkheden. Onze hardwarepartner biedt referentieontwerpen en ontwikkelingseenheden. Microsoft biedt een apparaatgeoptimaliseerde SDK die de mogelijkheden van de hardware volledig benut.


## <a name="speech-scenarios"></a>Spraakscenario's

Gebruiksscenario’s voor de Speech-service omvatten:

> [!div class="checklist"]
> * Ontwikkeling van apps die reageren op stemmen
> * Transcriptie van callcenteropnamen
> * Implementatie van spraakbots

### <a name="voice-user-interface"></a>Gebruikersinterface voor spraak

Spraakinvoer is een geweldige manier om uw app flexibel, handsfree en snel te gebruiken te maken. In een app met spraakmogelijkheden kunnen gebruikers gewoon om de gewenste informatie vragen in plaats van ernaar te moeten navigeren.

Als uw app is bedoeld voor gebruik door het algemene publiek, kunt u de standaard spraakherkenningsmodellen gebruiken. Die zijn goed in het herkennen van vele verschillende sprekers in algemene omgevingen.

Als uw app in een specifiek domein (bijvoorbeeld medisch of IT) zal worden gebruikt, kunt u een [taalmodel](how-to-customize-language-model.md) maken om de Speech-service de speciale terminologie aan te leren die door uw app wordt gebruikt.

Als uw app in een lawaaierige omgeving (bijvoorbeeld een fabriek) zal worden gebruikt, kunt u een aangepast [akoestisch model](how-to-customize-acoustic-models.md) maken om de Speech-service spraak en lawaai beter te kunnen laten onderscheiden.

Om aan de slag te gaan, downloadt u gewoon de [Speech-SDK](speech-sdk.md) en leest u een relevant [snelstartartikel](quickstart-csharp-dotnet-windows.md).

### <a name="call-center-transcription"></a>Callcentertranscriptie

Callcenteropnamen worden vaak alleen geraadpleegd als er tijdens het gesprek een probleem optreedt. Met de Speech-service kan elke opname gemakkelijk worden getranscribeerd naar tekst. Zodra opnamen in tekstvorm zijn, kunt u ze gemakkelijk indexeren voor [zoekopdrachten in volledige tekst](https://docs.microsoft.com/azure/search/search-what-is-azure-search) of [tekstanalyse](https://docs.microsoft.com/azure/cognitive-services/Text-Analytics/) toepassen om gevoel, taal en belangrijke woordgroepen te detecteren.

Als uw callcenteropnamen speciale terminologie (zoals productnamen of IT-jargon) bevatten, kunt u een [taalmodel](how-to-customize-language-model.md) maken om de Speech-service dat vocabulaire aan te leren. Een aangepast [akoestisch model](how-to-customize-acoustic-models.md) kan de Speech-service helpen niet zo goede telefoonverbindingen te begrijpen.

Voor meer informatie over dit scenario leest u meer over [batchtranscriptie](batch-transcription.md) met de Speech-service.

### <a name="voice-bots"></a>Spraakbots

[Bots](https://dev.botframework.com/) zijn een steeds populairdere manier om gebruikers te verbinden met de informatie die ze graag willen, en klanten met de bedrijven die ze graag gebruiken. Als u een conversationele gebruikersinterface aan uw website of app toevoegt, is de functionaliteit ervan gemakkelijker te vinden en sneller te gebruiken. Met de Speech-service wordt deze conversatie nóg vloeiender door te reageren op gesproken vragen.

Door een unieke personaliteit aan uw bot met spraakmogelijkheden toe te voegen (en uw merk te versterken), kunt u deze een eigen stem geven. U kunt in twee stappen een aangepaste stem maken. Eerst [maakt u opnamen](record-custom-voice-samples.md) van de stem die u wilt gebruiken. Vervolgens [verzendt u die opnamen](how-to-customize-voice-font.md) (samen met een transcriptie) naar de [stemaanpassingsportal](https://cris.ai/Home/CustomVoice) van de Speech-service, die de rest doet. Zodra u uw aangepaste stem hebt gemaakt, kunt u deze gemakkelijk in uw app gebruiken.

## <a name="next-steps"></a>Volgende stappen

Verkrijg een abonnementssleutel voor de Speech-service.

> [!div class="nextstepaction"]
> [Probeer de Speech-service gratis uit](get-started.md)
