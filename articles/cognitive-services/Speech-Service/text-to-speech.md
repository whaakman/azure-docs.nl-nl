---
title: Over Text to Speech | Microsoft Docs
description: Een overzicht van de mogelijkheden van tekst-naar-spraak.
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: v-jerkin
manager: noellelacharite
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 05/07/2018
ms.author: v-jerkin
ms.openlocfilehash: 84baf03c83bb63883b80982056cdf6e1e25b3fb7
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345824"
---
# <a name="about-the-text-to-speech-api"></a>Over Text to Speech API

De **Text to Speech** (TTS)-API van de service spraak invoertekst converteert naar natuurlijke klinkt speech (ook wel *spraak synthese*).

Voor het genereren van spraak, verzendt uw toepassing HTTP POST-aanvragen naar de service spraak. Daar tekst in human klinkt spraak is gemaakt en wordt geretourneerd als een audio-bestand. Een aantal stemmen en talen worden ondersteund.

In welke spraak synthese worden vastgesteld scenario's omvatten:

* *Verbetering van de toegankelijkheid:* **Text to Speech** technologie kunnen eigenaren van inhoud en uitgevers te reageren op de personen die verschillende manieren communiceren met de inhoud ervan. Mensen met gezichtsvermogen of problemen bij het lezen waarderen kunnen inhoud aurally gebruiken. Voice uitvoer ook gemakkelijker voor gebruikers om te profiteren van tekstinhoud, zoals kranten of blogs op mobiele apparaten tijdens het werk of uitoefenen.

* *Reageert multitasking scenario's:* **Text to Speech** kunnen personen belangrijke informatie kunnen snel en probleemloos tijdens besturen of anders buiten een handige lezen van de omgeving. Navigatie is een algemene toepassing op dit gebied. 

* *Verbeteren van learning met meerdere modi:* verschillende mensen meer informatie over beste op verschillende manieren. Onlinecursus experts is gebleken dat de stem en tekst samen te bieden kan ervoor zorgen dat informatie gemakkelijker te leren en te bewaren.

* *Het leveren van intuïtieve bots of assistenten:* te communiceren een integraal onderdeel van een bot intelligent chat of een virtuele assistent kan zijn. Steeds meer bedrijven ontwikkelt chat bots om aantrekkelijke customer service ervaringen voor hun klanten. Voice voegt een andere dimensie doordat de bot antwoorden aurally wordt ontvangen (bijvoorbeeld door telefoon).

## <a name="voice-support"></a>Voice-ondersteuning

De Microsoft **spraak** service biedt meer dan 75 stemmen in meer dan 45 talen en landinstellingen. Voor het gebruik van deze standaard 'stem lettertypen', moet u alleen de naam van de stem met enkele andere parameters opgeven wanneer u de service REST-API aanroept. Zie de details van de stemmen ondersteund [ondersteunde talen](supported-languages.md). 

Als u een unieke stem voor uw toepassing wilt, kunt u [aangepaste gesproken lettertypen](how-to-customize-voice-font.md) van uw eigen voorbeelden spraak.

## <a name="next-steps"></a>Volgende stappen

* [Uw proefabonnement spraak ophalen](https://azure.microsoft.com/try/cognitive-services/)
* [Zie het herkennen van spraak in C#](quickstart-csharp-windows.md)
