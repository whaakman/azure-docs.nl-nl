---
title: Over Text to Speech | Microsoft Docs
description: Een overzicht van de mogelijkheden van tekst naar spraak.
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: v-jerkin
manager: noellelacharite
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 05/07/2018
ms.author: v-jerkin
ms.openlocfilehash: d7ec8648a8428558264c9bfd4d923523b90cce07
ms.sourcegitcommit: 0b4da003fc0063c6232f795d6b67fa8101695b61
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37855937"
---
# <a name="about-the-text-to-speech-api"></a>Over de Text to Speech-API

De **tekst naar spraak** (Text-to-Speech)-API van de spraak-service zet invoertekst in natuurlijk klinkende spraak (ook wel genoemd *spraaksynthese*).

Voor het genereren van spraak, verzendt uw toepassing HTTP POST-aanvragen naar de Speech-service. Daar tekst is spraaksynthese human klinkende en geretourneerd als een geluidsbestand. Een aantal stemmen en talen worden ondersteund.

In welke spraak synthese wordt toegepast scenario's omvatten:

* *Verbetering van de toegankelijkheid:* **tekst naar spraak** technologie kunnen eigenaren van inhoud en uitgevers om te reageren op de verschillende manieren waarop mensen werken met hun eigen inhoud. Mensen met gezichtsvermogen of problemen bij het lezen waarderen kunnen inhoud aurally gebruiken. Stem ook uitvoer maakt het gemakkelijker voor gebruikers om te profiteren van tekstuele inhoud, zoals newspapers of blogs, op mobiele apparaten bij het nakomen van of als.

* *Reageert in scenario's multitasking:* **tekst naar spraak** kunnen mensen kunnen worden opgevangen belangrijke informatie snel en probleemloos tijdens het verkeer of anders buiten een handige lezen van de omgeving. Navigatie is een algemene toepassing op dit gebied. 

* *Verbetering van leren met meerdere modi:* verschillende mensen meer beste op verschillende manieren. Online learning experts heeft aangetoond dat de spraak- en samen bieden gegevens gemakkelijker helpen kan te leren en te behouden.

* *Leveren van intuïtieve bots of assistenten:* de mogelijkheid om te communiceren een integraal onderdeel van een intelligente chatbot of een virtuele assistent kan zijn. Steeds meer bedrijven zijn chat bots om aantrekkelijke klant een service ervaringen voor hun klanten te ontwikkelen. Stem wordt een andere dimensie toegevoegd doordat de antwoorden van de bot wilt aurally ontvangen (bijvoorbeeld via de telefoon).

## <a name="voice-support"></a>Voice-ondersteuning

De Microsoft **tekst naar spraak** service biedt meer dan 75 stemmen in meer dan 45 talen en landen. Voor het gebruik van deze standaard 'spraakstijlen', moet u alleen de naam van de gesproken met een aantal andere parameters opgeven wanneer u een van de service REST-API aanroept. Zie voor de details van de stemmen wordt ondersteund, [ondersteunde talen](https://docs.microsoft.com/en-us/azure/cognitive-services/speech-service/supported-languages#text-to-speech). 

Als u een unieke stem voor uw toepassing wilt, kunt u [aangepaste spraakstijlen](how-to-customize-voice-font.md) uit uw eigen voorbeelden van spraak.

## <a name="next-steps"></a>Volgende stappen

* [Uw proefabonnement voor spraak ophalen](https://azure.microsoft.com/try/cognitive-services/)
* [Zie voor het herkennen van gesproken tekst in C#](quickstart-csharp-windows.md)
