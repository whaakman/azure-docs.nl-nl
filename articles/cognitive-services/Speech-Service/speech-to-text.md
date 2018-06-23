---
title: Over spraak naar tekst | Microsoft Docs
description: Een overzicht van de mogelijkheden van Speech Text-API.
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: v-jerkin
manager: noellelacharite
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 05/07/2018
ms.author: v-jerkin
ms.openlocfilehash: 2eee1c6f9158f128ed5ffe575f8f498f1d3eb5e9
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345843"
---
# <a name="about-the-speech-to-text-api"></a>Spraak naar tekst API

De **spraak naar tekst** API *transcribes* audio stromen naar tekst die uw toepassing kan worden weergegeven voor de gebruiker of bij het fungeren als invoer opdracht. De API's kunnen worden gebruikt met een client SDK-bibliotheek (voor ondersteunde platforms en talen) of een REST-API.

De **spraak naar tekst** API biedt de volgende functies:

- Geavanceerde speech recognition technologie van Microsoft: dezelfde gebruikt door Cortana, Office en andere Microsoft-producten.

- Realtime continue herkenning. **Spraak naar tekst** kunnen gebruikers audio transcriberen naar tekst in realtime. Het ondersteunt ook het ontvangen van tussenliggende resultaten van de woorden die tot nu toe is herkend. De service automatisch worden herkend als het einde van spraak. Gebruikers kunnen ook kiezen voor extra opmaakopties, inclusief hoofdlettergebruik en leestekens, taalgebruik dat maskering en tekst normalisatie.

- Voor optimale **spraak naar tekst** resultaten voor interactieve, conversatie en dicteren scenario's. 

- Ondersteuning voor veel talen in meerdere dialecten. Zie voor een volledige lijst van ondersteunde talen in beide modi erkenning [ondersteunde talen](supported-languages.md#speech-to-text).

- Aangepaste taal en acoustic modellen, zodat u uw toepassing naar uw gebruikers manier van spreken, spreken omgeving en gespecialiseerde woordenlijst kunt aanpassen.

- Kennis van natuurlijke taal. Dankzij de integratie met [Language Understanding](https://docs.microsoft.com/azure/cognitive-services/luis/) (LUIS), u kunt afleiden intents en entiteiten van spraak. Gebruikers niet al op de hoogte van uw app vocabulaire, maar wordt beschreven wat ze willen in hun eigen woorden.

## <a name="api-capabilities"></a>API-mogelijkheden

Bepaalde functies van de **spraak naar tekst** API zijn niet beschikbaar via REST. De volgende tabel geeft een overzicht van de mogelijkheden van elke methode van de toegang tot de API.

| Gebruiksvoorbeeld | REST | SDK's |
|-----|-----|-----|----|
| Transcriberen met gebruikmaking van een korte utterance, zoals een opdracht (lengte < 15 s); Er zijn geen tussentijdse resultaten | Ja | Ja |
| Transcriberen met gebruikmaking van een langere utterance (> 15 s) | Nee | Ja |
| Transcriberen met gebruikmaking van streaming audio met optionele tussentijdse resultaten | Nee | Ja |
| Spreker intents via LUIS begrijpen | Er is geen\* | Ja |

\* *LUIS intents en entiteiten kunnen worden afgeleid met behulp van een afzonderlijke LUIS-abonnement. Bij dit abonnement, met de SDK LUIS-aanroep voor u en bieden entiteit en doel van de resultaten zoals spraak transcriptie. Met de REST API, kunt u LUIS aanroepen zelf intents en entiteiten met uw abonnement LUIS worden afgeleid.*

## <a name="next-steps"></a>Volgende stappen

* [Uw proefabonnement spraak ophalen](https://azure.microsoft.com/try/cognitive-services/)
* [Zie het herkennen van spraak in C#](quickstart-csharp-windows.md)
