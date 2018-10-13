---
title: Over tekst naar spraak
description: Een overzicht van de mogelijkheden van tekst naar spraak.
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: erhopf
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 05/07/2018
ms.author: erhopf
ms.openlocfilehash: bc60eed63fb40c42fc4331edb01e15f850bf6ecb
ms.sourcegitcommit: c282021dbc3815aac9f46b6b89c7131659461e49
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/12/2018
ms.locfileid: "49166080"
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

De Microsoft **tekst naar spraak** service biedt meer dan 75 stemmen in meer dan 45 talen en landen. Voor het gebruik van deze standaard 'spraakstijlen', moet u alleen de naam van de gesproken met een aantal andere parameters opgeven wanneer u een van de service REST-API aanroept. Zie voor de details van de stemmen wordt ondersteund, [ondersteunde talen](language-support.md#text-to-speech). 

Als u een unieke stem voor uw toepassing wilt, kunt u [aangepaste spraakstijlen](how-to-customize-voice-font.md) uit uw eigen voorbeelden van spraak.

## <a name="api-capabilities"></a>API-functies

Veel van de mogelijkheden van de **tekst naar spraak** API, met name wat betreft de aanpassing, zijn beschikbaar via REST. De volgende tabel geeft een overzicht van de mogelijkheden van elke methode van de toegang tot de API. Raadpleeg voor een volledige lijst van de mogelijkheden en API-details, [Swagger](https://swagger/service/11ed9226-335e-4d08-a623-4547014ba2cc#/),

| Use-case | REST | SDK's |
|-----|-----|-----|----|
| Gegevenssets voor de toon aanpassing uploaden | Ja | Nee |
| Maken en beheren van modellen voor spraak-lettertype | Ja | Nee |
| Maken en beheren van implementaties van spraak lettertype | Ja | Nee |
| Maken en beheren van spraak lettertype tests| Ja | Nee |
| Abonnementen beheren | Ja | Nee |

> [!NOTE]
> De API implementeert die de API-aanvragen voor 25 per vijf seconden beperkingslimieten. Bericht hearders wordt op de hoogte van de limieten.

## <a name="next-steps"></a>Volgende stappen

* [Uw proefabonnement voor Speech ophalen](https://azure.microsoft.com/try/cognitive-services/)
* [Zie how to bootsen spraak via de REST-API](how-to-text-to-speech.md)
