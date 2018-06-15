---
title: Wat is de spraak-service (preview)? | Microsoft Docs
description: 'De service spraak, onderdeel van de cognitieve Services van Microsoft, koppelt zijn verschillende Azure-spraakservices die eerder beschikbaar afzonderlijk waren netwerk: aangepaste spraak, Bing Speech (die bestaat uit spraakherkenning en tekst-naar-spraak) en de vertaling van spraak.'
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: v-jerkin
manager: noellelacharite
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 05/07/2018
ms.author: v-jerkin
ms.openlocfilehash: e7c09eee1634c52e78a523a7cc65641ea99f23e6
ms.sourcegitcommit: b7290b2cede85db346bb88fe3a5b3b316620808d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/05/2018
ms.locfileid: "35345983"
---
# <a name="what-is-the-speech-service-preview"></a>Wat is de spraak-service (preview)?

De service spraak, onderdeel van de cognitieve Services van Microsoft, koppelt zijn verschillende Azure-spraakservices die eerder beschikbaar afzonderlijk waren netwerk: aangepaste spraak, Bing Speech (die bestaat uit spraakherkenning en tekst-naar-spraak) en de vertaling van spraak. Zoals de precursoren, wordt de service spraak aangedreven door technologieën in andere Microsoft-producten, waaronder Cortana en Microsoft Office.

> [!NOTE]
> De spraak-service is momenteel in de openbare preview. Retourneren hier regelmatig updates voor documentatie en aanvullende codevoorbeelden.

De uniforme spraak-service biedt ontwikkelaars een eenvoudige manier om te bieden hun toepassingen krachtige spraak ingeschakelde functies met één abonnement. Uw apps kunnen nu functie gesproken opdracht, schrijffouten dicteren, spraak synthese en vertaling.

|Functie|Beschrijving|
|-|-|
|Spraak naar tekst|Continue menselijke stem converteert naar tekst die kan worden gebruikt als invoer voor uw toepassing. Kan worden geïntegreerd met de [Language Understanding service](https://docs.microsoft.com/azure/cognitive-services/luis/) (LUIS) worden afgeleid van de gebruiker van utterances.|
|Tekst naar spraak|Tekst wordt geconverteerd naar audio-bestanden van natuurlijke klinkt kunstmatige spraak.|
|Spraak&nbsp;vertaling|Tekst of gesproken uitvoer bieden vertalingen van spraak naar andere talen.|

## <a name="using-the-speech-service"></a>Met behulp van de service spraak

De spraak-service wordt uitgevoerd op twee manieren beschikbaar zijn. [De SDK](speech-sdk.md) isoleert weg de details van de netwerkprotocollen. De [REST-API](rest-apis.md) werkt met elke programmeertaal maar biedt niet alle functies die worden aangeboden door de SDK.

|<br>Methode|Speech<br>naar tekst|Tekst<br>Speech|Speech<br>De vertaling|<br>Beschrijving|
|-|-|-|-|-|
|[SDK 's](speech-sdk.md)|Ja|Nee|Ja|Bibliotheken voor specifieke programmeertalen die vereenvoudigen.|
|[REST](rest-apis.md)|Ja|Ja|Nee|Een eenvoudige HTTP-gebaseerde-API waarmee u gemakkelijk spraak toevoegen aan uw toepassing.|

## <a name="speech-to-text"></a>Spraak naar tekst

De [spraak naar tekst](speech-to-text.md) (STT) of spraakherkenning, API transcribes audio stromen naar tekst die uw toepassing kan worden geaccepteerd als invoer. Uw toepassing kan vervolgens bijvoorbeeld de tekst in een document invoeren of bij het fungeren als een opdracht.

Spraak naar tekst afzonderlijk is geoptimaliseerd voor interactieve, conversatie en dicteren scenario's. Hieronder vindt u algemene gebruiksvoorbeelden voor de spraak tekst API. 

* Herkent korte een utterance, zoals een opdracht, zonder tussentijdse resultaten
* Transcriberen met gebruikmaking van een lange, eerder opgenomen utterance, zoals een voicemailbericht
* Transcriberen met gebruikmaking van streaming spraak in realtime met gedeeltelijke resultaten, voor dicteren
* Bepalen wat gebruikers wilt doen op basis van een aanvraag voor gesproken natuurlijke taal

De spraak tekst API biedt ondersteuning voor interactieve spraak schrijffouten met realtime continue opname en tussentijdse resultaten. Het ondersteunt ook detectie van het einde van spraak, optioneel automatisch hoofdlettergebruik en leestekens taalgebruik maskeren en tekst normalisatie.

U kunt aanpassen spraak naar tekst akoestisch en taal-modellen voor gespecialiseerde vocabulaire, omgevingen met veel ruis veroorzaken en verschillende manieren van spreken.

## <a name="text-to-speech"></a>Tekst naar spraak

De [Text to Speech](text-to-speech.md) (TTS) of spraak synthese, API converteert natuurlijke klinkt spraak, geleverd aan uw toepassing in een audiobestand tekst zonder opmaak. Meerdere stemmen, variëren in geslacht of accent, zijn beschikbaar voor veel van de ondersteunde talen.

De API ondersteunt tags van spraak synthese Markup Language (SSML), zodat u kunt exacte fonetische uitspraak voor lastige woorden opgeven. SSML kan ook duiden op spraak kenmerken (inclusief de nadruk, snelheid, volume, geslacht en presentatie) rechts in de tekst.

Hieronder vindt u algemene gebruiksvoorbeelden voor Text to Speech API.

* Spraakuitvoer als een alternatieve schermuitvoer voor een visuele gebruikers
* Stem te vragen voor de toepassingen in de auto zoals navigatie
* Eigen gebruikersinterfaces in overleg met de spraak tekst API

Als u een niet-ondersteunde dialect moet of gewoon een unieke stem voor uw toepassing wilt, Text to Speech API biedt ondersteuning voor aangepaste gesproken modellen.

## <a name="speech-translation"></a>Spraakomzetting

De [spraak vertaling](speech-translation.md) API kan worden gebruikt om te vertalen streaming audio in near-realtime of opgenomen spraak te verwerken. In de streaming vertaling, stuurt de service tussentijdse resultaten die kunnen worden weergegeven voor de gebruiker om de voortgang vertaling te geven. De resultaten kunnen worden geretourneerd als tekst of als een stem.

Gebruiksvoorbeelden voor spraak vertaling biedt de volgende.

* Een 'eigen' vertaling mobiele app of apparaat implementeren voor reizigers 
* Bieden van automatische vertalingen voor subtitling van audio en video-opnamen

## <a name="speech-devices-sdk"></a>Speech Devices-SDK

Met de introductie van de geïntegreerde spraak service Microsoft en haar partners bieden een geïntegreerde hardware/software-platform geoptimaliseerd voor het ontwikkelen van spraak-apparaten: de [spraak apparaten SDK](speech-devices-sdk.md). Deze SDK is geschikt voor smart spraak apparaten voor alle typen toepassingen ontwikkelen.

De apparaten spraak SDK kunt u voor het bouwen van uw eigen ambient apparaten met een aangepaste wake-woord, zodat de hint die audio vastleggen activeert uniek voor uw merk is. Het bevat ook bovenliggende audio van meerkanaals bronnen voor nauwkeuriger spraakherkenning, met inbegrip van ruis onderdrukking, helemaal veld spraak en beamforming verwerken.

## <a name="next-steps"></a>Volgende stappen

De sleutel voor een gratis proefabonnement voor de service spraak ophalen.

> [!div class="nextstepaction"]
> [De service spraak gratis uitproberen](get-started.md)
