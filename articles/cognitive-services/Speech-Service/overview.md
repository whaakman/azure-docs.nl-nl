---
title: Wat is de Speech-service?
description: "De Speech-service, onderdeel van Microsoft's Cognitive Services, verenigt verschillende Azure-spraakservices die voorheen afzonderlijk beschikbaar waren: Bing Speech (bestaande uit spraakherkenning en tekst-naar-spraak), Custom Speech en Spraakomzetting."
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: v-jerkin
ms.service: cognitive-services
ms.component: speech-service
ms.topic: overview
ms.date: 09/24/2018
ms.author: v-jerkin
ms.openlocfilehash: b014144dd819aa354596175068bfb2c5905a4377
ms.sourcegitcommit: 67abaa44871ab98770b22b29d899ff2f396bdae3
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/08/2018
ms.locfileid: "48857989"
---
# <a name="what-is-the-speech-service"></a>Wat is de Speech-service?


Net zoals de andere Azure-spraakservices wordt de Speech-service mogelijk gemaakt door de spraaktechnologieën die worden gebruikt in producten zoals Cortana en Microsoft Office.

De Speech-service verenigt de Azure-spraakfuncties die voorheen beschikbaar waren via de services [Bing Speech-API](https://docs.microsoft.com/azure/cognitive-services/speech/home), [Translator Speech](https://docs.microsoft.com/azure/cognitive-services/translator-speech/), [Custom Speech](https://docs.microsoft.com/azure/cognitive-services/custom-speech-service/cognitive-services-custom-speech-home) en [Custom Voice](http://customvoice.ai/). Eén abonnement biedt nu toegang tot al deze mogelijkheden.

## <a name="main-speech-service-functions"></a>Belangrijkste functies van de Speech-service

De primaire functies van de Speech-service zijn Spraak naar tekst (ook wel spraakherkenning of transcriptie genoemd), Tekst naar spraak (spraaksynthese) en Spraakomzetting.

|Functie|Functies|
|-|-|
|[Spraak naar tekst](speech-to-text.md)| <ul><li>Transcribeert continue realtime spraak naar tekst.<li>Kan spraak uit geluidsopnamen in batches transcriberen. <li>Ondersteunt tussenliggende resultaten, detectie van het einde van de spraak, automatische tekstopmaak en maskering van grof taalgebruik. <li>Kan [Language Understanding](https://docs.microsoft.com/azure/cognitive-services/luis/) (LUIS) gebruiken om de bedoeling van gebruikers af te leiden van getranscribeerde spraak.\*|
|[Tekst naar spraak](text-to-speech.md)| <ul><li>Converteert tekst naar natuurlijk klinkende spraak. <li>Biedt meerdere geslachten en/of dialecten voor een groot aantal ondersteunde talen. <li>Ondersteunt de invoer van tekst zonder opmaak en Speech Synthesis Markup Language (SSML). |
|[Spraakomzetting](speech-translation.md)| <ul><li>Vertaalt streaming audio vrijwel in realtime.<li> Kan ook opgenomen spraak verwerken.<li>Levert resultaten als tekst of gesynthetiseerde spraak. |

\* *Herkenning van bedoeling vereist een LUIS-abonnement.*

## <a name="customize-speech-features"></a>Spraakfuncties aanpassen

U kunt uw eigen gegevens gebruiken om de onderliggende modellen van de functies Spraak naar tekst en Tekst naar spraak van de Speech-service te trainen.

|Functie|Model|Doel|
|-|-|-|
|Spraak naar tekst|[Akoestisch model](how-to-customize-acoustic-models.md)|Helpt bepaalde sprekers en omgevingen zoals auto’s of fabrieken te transcriberen.|
||[Taalmodel](how-to-customize-language-model.md)|Helpt veldspecifiek vocabulaire en grammatica zoals medisch of IT-jargon te transcriberen.|
||[Uitspraakmodel](how-to-customize-pronunciation.md)|Helpt afkortingen en acroniemen zoals ‘IOU’ voor ‘I owe you’ te transcriberen. |
|Tekst naar spraak|[Spraakstijl](how-to-customize-voice-font.md)|Geeft uw app zijn eigen stem door het model te trainen in voorbeelden van menselijke spraak.|

U kunt uw aangepaste modellen overal gebruiken waar u de standaardmodellen gebruikt in de functie Spraak naar tekst of Tekst naar spraak van uw app.

## <a name="use-the-speech-service"></a>De Speech-service gebruiken

Om de ontwikkeling van apps met spraakmogelijkheden te vereenvoudigen, biedt Microsoft de [Speech-SDK](speech-sdk.md) voor gebruik met de Speech-service. De Speech-SDK biedt consistente, systeemeigen API’s voor Spraak naar tekst en Spraakomzetting voor C#, C++ en Java. Als u met een van deze talen ontwikkelt, maakt de Speech-SDK het ontwikkelen gemakkelijker door de netwerkgegevens voor u af te handelen.

De Speech-service heeft ook een [REST-API](rest-apis.md) die werkt met elke programmeertaal die HTTP-verzoeken kan indienen. De REST-interface biedt niet de streaming, realtime functionaliteit van de SDK.

|<br>Methode|Speech<br>naar tekst|Tekst naar<br>Speech|Speech<br>Omzetting|<br>Beschrijving|
|-|-|-|-|-|
|[Speech-SDK](speech-sdk.md)|Ja|Nee|Ja|Systeemeigen API’s voor C#, C++ en Java om ontwikkeling te vereenvoudigen.|
|[REST](rest-apis.md)|Ja|Ja|Nee|Een eenvoudige HTTP-gebaseerde API die het gemakkelijk maakt spraak aan uw apps toe te voegen.|

### <a name="websockets"></a>WebSockets

De Speech-service heeft ook WebSockets-protocollen om Spraak naar tekst en Spraakomzetting te streamen. De Speech-SDK’s gebruiken deze protocollen om met de Speech-service te communiceren. Gebruik de Speech-SDK in plaats van te proberen uw eigen WebSockets-communicatie te implementeren met de Speech-service.

Als u echter al code hebt die Bing Speech of Translator Speech via WebSockets gebruikt, kunt u deze bijwerken om de Speech-service te gebruiken. De WebSockets-protocollen zijn compatibel, alleen de eindpunten zijn anders.

### <a name="speech-devices-sdk"></a>Speech Devices SDK

De [Speech Devices SDK](speech-devices-sdk.md) is een geïntegreerd hardware- en softwareplatform voor ontwikkelaars van apparaten met spraakmogelijkheden. Onze hardwarepartner biedt referentieontwerpen en ontwikkelingseenheden. Microsoft biedt een apparaatgeoptimaliseerde SDK die de mogelijkheden van de hardware volledig benut.

## <a name="why-move-to-the-speech-service"></a>Waarom overstappen op de Speech-service?

De Speech-service biedt alle functionaliteit en meer van de Bing Speech-API en drie andere Azure Speech-services: Custom Speech, Custom Voice en Translator Speech. We raden gebruikers van deze services aan om te migreren naar de Speech-service.

De Speech-service omvat veel upgrades ten opzichte van deze andere services, waaronder:

* Hogere nauwkeurigheid van de spraakherkenning. We verbeteren regelmatig de modellen die worden gebruikt in de service.

* Meer schaalbaarheid. De service kan beter meerdere gelijktijdige aanvragen verwerken, waardoor de latentie wordt verminderd.

* De Speech-service maakt gebruik van een prijsmodel op basis van tijd. Zie [Prijzen van de Speech-service](https://azure.microsoft.com/pricing/details/cognitive-services/speech-services/) voor meer informatie.

* De Speech-service is beschikbaar in [meerdere regio’s](regions.md) om in te kunnen spelen op de behoeften van klanten over de hele wereld. U moet een [Azure-abonnement](https://docs.microsoft.com/azure/cognitive-services/welcome) hebben voor elke regio die wordt gebruikt door uw toepassing.

* Een enkele abonnementssleutel voor de Speech-service verleent toegang tot de volgende functies. Elk wordt afzonderlijk gemeten, zodat u alleen betaalt voor de functies die u gebruikt.

    * [Spraak naar tekst](speech-to-text.md)
    * [Aangepaste spraak naar tekst ](https://cris.ai/CustomSpeech)
    * [Tekst naar spraak](text-to-speech.md)
    * [Aangepaste tekst naar spraak](https://cris.ai/CustomVoice)
    * [Spraakomzetting](speech-translation.md) (bevat geen [tekstomzetting](https://docs.microsoft.com/azure/cognitive-services/translator/translator-info-overview))

* De functie spraak-naar-tekst van de Speech-service kan worden geïntegreerd met de [Language Understanding Service](https://docs.microsoft.com/azure/cognitive-services/luis/) (LUIS) voor het herkennen van de intentie van sprekers. U kunt ook een LUIS-eindpuntsleutel gebruiken met de Speech-service. Zie de zelfstudie over [intentieherkenning](how-to-recognize-intents-from-speech-csharp.md) voor meer informatie.

* Bij spraak-naar-tekst hoeft u geen opnamemodus meer op te geven.

* De Speech-service ondersteunt 24KHz-stemmen voor tekst-naar-spraak, waardoor de audiokwaliteit wordt verbeterd. Op dit moment zijn er twee dergelijke stemmen (alleen Amerikaans Engels): `Jessa24kRUS` en `Guy24kRUS`.

* Met [batch-transcriptie](batch-transcription.md) van de Speech-service kunnen grote hoeveelheden opgenomen spraak, zoals opnamen van callcenters, efficiënt worden getranscribeerd, zodat ze eenvoudig kunnen worden geanalyseerd en doorzocht.

* Wanneer u de Speech-SDK gebruikt, bestaat er geen tijdslimiet voor het streamen van de transcriptie van spraak-naar-tekst.

* De [Speech-SDK](speech-sdk.md) biedt een consistente API voor de Speech-service voor meerdere programmeertalen en uitvoeringsomgevingen (inclusief Windows 10, UWP en .NET Core), waardoor de ontwikkeling eenvoudiger is, met name op meerdere platforms.

* De Speech-service is compatibel met de REST-API's en het WebSockets-protocol die worden gebruikt door andere spraakservices van Azure, zodat u bestaande clienttoepassingen eenvoudig kunt migreren naar de Speech-service.

## <a name="speech-scenarios"></a>Spraakscenario's

Gebruiksscenario’s voor de Speech-service omvatten:

> [!div class="checklist"]
> * Ontwikkeling van apps die reageren op stemmen
> * Transcriptie van callcenteropnamen
> * Implementatie van spraakbots

### <a name="voice-user-interface"></a>Gebruikersinterface voor spraak

Spraakinvoer is een geweldige manier om uw app flexibel, handsfree en snel te gebruiken te maken. In een app met spraakmogelijkheden kunnen gebruikers gewoon om de gewenste informatie vragen.

Als uw app is bedoeld voor gebruik door het algemene publiek, kunt u de standaard spraakherkenningsmodellen gebruiken. Die zijn goed in het herkennen van vele verschillende sprekers in algemene omgevingen.

Als uw app in een specifiek domein wordt gebruikt, zoals medisch of IT, kunt u een [taalmodel](how-to-customize-language-model.md) maken. U kunt dit model gebruiken om de Speech-service te leren over de speciale terminologie die wordt gebruikt door uw app.

Als uw app in een lawaaierige omgeving wordt gebruikt, zoals een fabriek, kunt u een aangepast [akoestisch model](how-to-customize-acoustic-models.md) maken. Dit model helpt de Speech-service om spraak te onderscheiden van ruis.

U kunt snel aan de slag met de service. Download de [Speech-SDK](speech-sdk.md) en volg de instructies in het relevante [snelstartartikel](quickstart-csharp-dotnet-windows.md).

### <a name="call-center-transcription"></a>Callcentertranscriptie

Opnamen in een callcenter worden vaak alleen geraadpleegd als er tijdens het gesprek een probleem optreedt. Met de Speech-service kan elke opname gemakkelijk worden getranscribeerd naar tekst. U kunt de tekst eenvoudig indexeren voor [zoekopdrachten in volledige tekst](https://docs.microsoft.com/azure/search/search-what-is-azure-search) of [tekstanalyse](https://docs.microsoft.com/azure/cognitive-services/Text-Analytics/) toepassen om gevoel, taal en belangrijke woordgroepen te detecteren.

Als uw callcenteropnamen speciale terminologie, zoals productnamen of IT-jargon bevatten, kunt u een [taalmodel](how-to-customize-language-model.md) maken om de Speech-service dat vocabulaire aan te leren. Een aangepast [akoestisch model](how-to-customize-acoustic-models.md) kan de Speech-service helpen niet zo goede telefoonverbindingen te begrijpen.

Voor meer informatie over dit scenario leest u meer over [batchtranscriptie](batch-transcription.md) met de Speech-service.

### <a name="voice-bots"></a>Spraakbots

[Bots](https://dev.botframework.com/) zijn een populaire manier om gebruikers te verbinden met de informatie die ze graag willen, en klanten met de bedrijven die ze graag gebruiken. Als u een conversationele gebruikersinterface aan uw website of app toevoegt, is de functionaliteit gemakkelijker te vinden en sneller te gebruiken. Met de Speech-service wordt deze conversatie nóg vloeiender door te reageren op gesproken vragen.

U kunt een unieke personaliteit aan uw bot met spraakmogelijkheden toevoegen door deze een eigen stem te geven. U kunt in twee stappen een aangepaste stem maken. Eerst [maakt u opnamen](record-custom-voice-samples.md) van de stem die u wilt gebruiken. Vervolgens [verzendt u die opnamen](how-to-customize-voice-font.md) samen met een transcriptie naar de [stemaanpassingsportal](https://cris.ai/Home/CustomVoice) van de Speech-service, die de rest doet. Nadat u uw aangepaste stem hebt gemaakt, kunt u deze gemakkelijk in uw app gebruiken.

## <a name="next-steps"></a>Volgende stappen

Verkrijg een abonnementssleutel voor de Speech-service.

> [!div class="nextstepaction"]
> [Probeer de Speech-service gratis uit](get-started.md)
