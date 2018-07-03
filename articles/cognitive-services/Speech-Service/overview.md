---
title: Wat is de spraakservice (preview)? | Microsoft Docs
description: 'De spraak-service, onderdeel van de Microsoft Cognitive Services, koppelt zijn verschillende Azure speech services die eerder beschikbaar afzonderlijk waren netwerk: Bing Speech (die bestaat uit spraakherkenning en tekst naar spraak), aangepaste spraak en Spraakomzetting.'
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: v-jerkin
manager: noellelacharite
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 05/07/2018
ms.author: v-jerkin
ms.openlocfilehash: ccdcdeeaf4ac8730be4f9e3ee648dc41c2a02641
ms.sourcegitcommit: 756f866be058a8223332d91c86139eb7edea80cc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/02/2018
ms.locfileid: "37345155"
---
# <a name="what-is-the-speech-service-preview"></a>Wat is de spraakservice (preview)?

De spraak-service, onderdeel van de Microsoft Cognitive Services, koppelt zijn verschillende Azure speech services die eerder beschikbaar afzonderlijk waren netwerk: Bing Speech (die bestaat uit spraakherkenning en tekst naar spraak), aangepaste spraak en Spraakomzetting. De Speech-service is, zoals de precursoren, mogelijk gemaakt door de technologieën die worden gebruikt in andere Microsoft-producten, met inbegrip van Cortana en Microsoft Office.

> [!NOTE]
> De spraak-service is momenteel in openbare preview. Ga terug hier regelmatig op updates naar documentatie en voorbeelden van aanvullende code.

De geïntegreerde Speech-service biedt ontwikkelaars een eenvoudige manier om aan te geven van hun toepassingen krachtige spraak ingeschakelde functies met één abonnement. Uw apps kunnen nu voorzien van gesproken opdrachten, transcriptie, dicteren, spraaksynthese en vertaling.

|Functie|Beschrijving|
|-|-|
|Spraak naar tekst|Converteert continue menselijke spraak naar tekst die kan worden gebruikt als invoer voor uw toepassing. Kan worden geïntegreerd met de [Language Understanding service](https://docs.microsoft.com/azure/cognitive-services/luis/) (LUIS) worden afgeleid van de bedoeling van gebruikers van uitingen.|
|Tekst naar spraak|Converteert tekst naar audio-bestanden van de kunstmatige spraak natuurlijke klinkende.|
|Spraak&nbsp;vertaling|Vertalingen van spraak naar andere talen met tekst of spraak uitvoer opgeven.|

## <a name="using-the-speech-service"></a>Met behulp van de Speech-service

De spraak-service is beschikbaar op twee manieren. [De SDK](speech-sdk.md) de details van de netwerkprotocollen volledig weggewerkt. De [REST-API](rest-apis.md) werkt met elke programmeertaal, maar biedt niet alle functies die worden aangeboden door de SDK.

|<br>Methode|Speech<br>naar tekst|Tekst die moet worden<br>Speech|Speech<br>Omzetting|<br>Beschrijving|
|-|-|-|-|-|
|[SDK 's](speech-sdk.md)|Ja|Nee|Ja|-Bibliotheken voor specifieke programmeertalen die eenvoudiger.|
|[REST](rest-apis.md)|Ja|Ja|Nee|Een eenvoudige, door de HTTP-gebaseerde API waarmee u eenvoudig spraak toevoegen aan uw toepassing.|

## <a name="speech-to-text"></a>Spraak naar tekst

De [spraak naar tekst](speech-to-text.md) (STT), of spraakherkenning, API transcribes audiostreams naar tekst die uw toepassing als invoer accepteren kunt. Uw toepassing kan de tekst dan bijvoorbeeld in een document invoeren of als een opdracht interpreteren.

Spraak naar tekst afzonderlijk is geoptimaliseerd voor interactieve, conversatie en dicteren scenario's. Hieronder vindt u algemene scenario's voor de Speech to Text-API. 

* Een korte utterance, zoals een opdracht, zonder tussentijdse resultaten herkennen
* Een lange, eerder opgenomen utterance, zoals een voicemailbericht transcriberen
* Transcriberen streaming gesproken tekst in realtime met gedeeltelijke resultaten, voor spraakherkenning
* Bepalen wat gebruikers doen op basis van een aanvraag voor spraak natuurlijke taal

De spraak-naar-tekst-API biedt ondersteuning voor interactieve spraaktranscriptie met realtime continue erkenning en tijdelijke resultaten. De SDK ondersteunt ook de detectie van het einde van de spraak, optioneel automatisch hoofdlettergebruik, maskering van grof taalgebruik, en tekstnormalisatie.

U kunt spraak naar tekst akoestische en taalmodellen om te voldoen aan gespecialiseerde vocabulaire ruis omgevingen en spreken op verschillende manieren aanpassen.

## <a name="text-to-speech"></a>Tekst naar spraak

De [tekst naar spraak](text-to-speech.md) (Text-to-Speech), of spraaksynthese, API converteert natuurlijk klinkende spraak, die worden geleverd aan uw toepassing in een geluidsbestand tekst zonder opmaak. Meerdere stemmen, variëren in geslacht of accent, zijn beschikbaar voor vele ondersteunde talen.

De API ondersteunt tags van spraak synthese Markup Language (SSML), zodat u kunt opgeven aan welke exacte fonetische uitspraak voor lastige woorden. Met SSML kunt u direct vanuit de tekst ook spraakkenmerken aangeven, waaronder nadruk, tempo, volume, geslacht en toonhoogte.

Hieronder vindt u algemene scenario's voor de Text to Speech-API.

* Spraakuitvoer als een alternatieve schermuitvoer voor slechtzienden gebruikers
* Stem vragen om in de auto-toepassingen, zoals navigatie
* Conversatie gebruikersinterfaces in combinatie met de Speech to Text-API

Als u een niet-ondersteunde dialect moet of alleen een unieke stem voor uw toepassing wilt, de Text to Speech-API biedt ondersteuning voor aangepaste gesproken modellen.

## <a name="speech-translation"></a>Spraakomzetting

De [Spraakomzetting](speech-translation.md) API kan worden gebruikt voor de omzetting van streaming audio in near-real-time of opgenomen spraak te verwerken. In de streaming-vertaling, retourneert de service tussentijdse resultaten die kunnen worden weergegeven aan de gebruiker om aan te geven van de voortgang van de vertaling. De resultaten kunnen worden geretourneerd als tekst of als spraak.

Use cases voor Spraakomzetting zijn de volgende.

* Een 'beschrijvende' vertaling mobiele app of een apparaat voor reizigers implementeren 
* Automatische vertalingen voor subtitling van audio en video-opnamen opgeven

## <a name="speech-devices-sdk"></a>Speech Devices SDK

Met de introductie van de geïntegreerde Speech-service, Microsoft en haar partners bieden een geïntegreerde hardware/software-platform dat is geoptimaliseerd voor het ontwikkelen van spraak ingeschakelde apparaten: de [spraak Devices SDK](speech-devices-sdk.md). Deze SDK is geschikt voor het ontwikkelen van slimme apparaten met spraakmogelijkheden voor alle toepassingstypen.

De SDK van de apparaten spraakherkenning kunt u bouw uw eigen omgeving apparaten met een aangepaste wake, zodat de hint die wordt geactiveerd vastleggen systeemaudio uniek is voor uw merk is. De SDK bevat ook audioverwerking van superieure kwaliteit van audio uit meerkanaals bronnen, voor nauwkeurigere spraakherkenning, waaronder ruisonderdrukking, 'far-field' spraak en golfvorming.

De SDK is gebaseerd op websockets met behulp van poort 443.

## <a name="next-steps"></a>Volgende stappen

Een gratis proefabonnement-sleutel voor de service voor spraak ophalen.

> [!div class="nextstepaction"]
> [De spraak-service gratis uitproberen](get-started.md)
