---
title: Aanvraaglimieten - Translator Text-API
titleSuffix: Azure Cognitive Services
description: Dit artikel worden de aanvraaglimieten voor de Translator Text-API. Kosten worden berekend op basis van aantal tekens, geen aanvraag frequentie met een limiet van 5000 tekens per aanvraag. Teken limieten zijn abonnement op basis van met F0 beperkt tot 2 miljoen tekens per uur.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: conceptual
ms.date: 06/04/2019
ms.author: erhopf
ms.openlocfilehash: c19e39918aa64730a35a27fcdadd70800f47f4fa
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66514643"
---
# <a name="request-limits-for-translator-text"></a>Aanvraaglimieten voor Translator tekst

In dit artikel bevat bandbreedtebeperking limieten voor de Translator Text-API. Services omvatten vertaling, vele, detectie van de lengte van de zin, taaldetectie en alternatieve vertalingen.

## <a name="character-and-array-limits-per-request"></a>Teken en matrix limieten per aanvraag

Elk verzoek vertalen is beperkt tot maximaal 5000 tekens. U betaalt per teken, niet door het aantal aanvragen. Het is raadzaam om kortere aanvragen te verzenden.

De volgende tabel lijsten matrix-element en teken limieten voor elke bewerking van de Translator Text-API.

| Bewerking | Maximale grootte van het matrixelement |   Maximum aantal matrixelementen |  Maximale grootte aanvragen (tekens) |
|:----|:----|:----|:----|
| Translate | 5,000 | 100   | 5,000 |
| Transliterate | 5,000 | 10    | 5,000 |
| Detect | 10\.000 | 100 |   50,000 |
| BreakSentence | 10\.000    | 100 | 5,0000 |
| Opzoeken in woordenlijst| 100 |  10  | 1000 |
| Voorbeelden in woordenlijst | 100 voor tekst en 100 voor vertaling (200 totaal)| 10|   2,000 |

## <a name="character-limits-per-hour"></a>Teken limieten per uur

Het aantal tekens per uur is gebaseerd op uw abonnement op Translator Text-laag. De uurquotum moet gelijkmatig worden gebruikt tijdens het uur. Als u bereiken of groter zijn dan deze limieten, of een gedeelte van het quotum te grote in een korte periode verzenden, ontvangt u waarschijnlijk een out-of quotum antwoord.

| Laag | Maximum aantal tekens |
|------|-----------------|
| F0 | 2 miljoen tekens per uur |
| S1 | 40 miljoen tekens per uur |
| S2 / C2 | 40 miljoen tekens per uur |
| S3 / C3 | 120 miljoen tekens per uur |
| S4 / C4 | 200 miljoen tekens per uur |

Limieten voor [meerdere services abonnementen](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference#authentication) zijn hetzelfde als de S1-laag.

Deze limieten zijn beperkt tot de standaard omzettingsmodellen van Microsoft. Aangepaste omzettingsmodellen die gebruikmaken van aangepaste Translator zijn beperkt tot 1.800 tekens per seconde.

## <a name="latency"></a>Latentie

De Translator Text-API heeft een maximale latentie van 15 seconden met behulp van standaard-modellen. Met behulp van aangepaste modellen vertaling heeft een maximale latentie van 25 seconden. Op dit moment hebt u hebt ontvangen een resultaat of een time-antwoord. Normaal gesproken worden antwoorden geretourneerd in 150 milliseconden op 300 milliseconden. Reactietijden variëren op basis van de grootte van het paar aanvraag- en taalinstellingen. Als u een vertaling niet ontvangt of een [foutbericht](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference#errors) in die periode, moet u Controleer uw netwerkverbinding en probeer het opnieuw.

## <a name="sentence-length-limits"></a>Lengtebeperkingen zin

Wanneer u de [BreakSentence](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-break-sentence) functie zinlengte is beperkt tot 275 tekens. Er zijn uitzonderingen voor de volgende talen:

| Taal | Code | Maximum aantal tekens |
|----------|------|-----------------|
| Chinees | zh | 132 |
| Duits | de | 290 |
| Italiaans | it | 280 |
| Japans | ja | 150 |
| Portugees | pt | 290 |
| Spaans | es | 280 |
| Italiaans | it | 280 |
| Thais | do | 258 |

> [!NOTE]
> Deze limiet niet van toepassing op vertalingen.

## <a name="next-steps"></a>Volgende stappen

* [Prijzen](https://azure.microsoft.com/pricing/details/cognitive-services/translator-text-api/)
* [Regionale beschikbaarheid](https://azure.microsoft.com/global-infrastructure/services/?products=cognitive-services)
* [V3 Translator Text-API-referentie](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference)
