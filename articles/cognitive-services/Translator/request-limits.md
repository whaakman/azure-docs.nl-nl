---
title: Aanvraaglimieten - Translator Text-API
titleSuffix: Azure Cognitive Services
description: Dit artikel worden de aanvraaglimieten voor de Translator Text-API. Kosten worden berekend op basis van aantal tekens, geen aanvraag frequentie met een limiet van 5000 tekens per aanvraag. Teken limieten zijn abonnement op basis van met F0 beperkt tot 2 miljoen tekens per uur.
services: cognitive-services
author: erhopf
manager: cgronlun
ms.service: cognitive-services
ms.component: translator-text
ms.topic: conceptual
ms.date: 11/15/2018
ms.author: erhopf
ms.openlocfilehash: 29ef4cc594a3335d37bd813c0b682b248f96cf22
ms.sourcegitcommit: 7804131dbe9599f7f7afa59cacc2babd19e1e4b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/17/2018
ms.locfileid: "51858344"
---
# <a name="request-limits-for-translator-text"></a>Aanvraaglimieten voor Translator tekst

In dit artikel bevat bandbreedtebeperking limieten voor de Translator Text-API. Services omvatten vertaling, vele, detectie van de lengte van de zin, taaldetectie en alternatieve vertalingen.

## <a name="character-limits-per-request"></a>Tekens per aanvraag

Elke aanvraag is beperkt tot maximaal 5000 tekens. U betaalt per teken, niet door het aantal aanvragen. Het is raadzaam om kortere aanvragen te verzenden, en sommige op een bepaald moment openstaande aanvragen.

Er is geen limiet voor het aantal openstaande aanvragen naar de Translator Text-API.

## <a name="character-limits-per-hour"></a>Teken limieten per uur

Het aantal tekens per uur is gebaseerd op uw abonnement op Translator Text-laag. Als u bereiken of groter zijn dan deze limieten, ontvangt u waarschijnlijk een out-of quotum reactie:

| Laag | Maximum aantal tekens |
|------|-----------------|
| F0 | 2 miljoen tekens per uur |
| S1 | 40 miljoen tekens per uur |
| S2 | 40 miljoen tekens per uur |
| S3 | 120 miljoen tekens per uur |
| S4 | 200 miljoen tekens per uur |

Deze limieten zijn beperkt tot de algemene systemen van Microsoft. Aangepaste vertaalsystemen die van Microsoft Translator Hub gebruikmaken zijn beperkt tot 1.800 tekens per seconde.

## <a name="latency"></a>Latentie

Translator tekst heeft een maximale latentie van 13 seconden. Op dit moment hebt u hebt ontvangen een resultaat of een time-antwoord. Normaal gesproken worden antwoorden geretourneerd in 150 milliseconden op 300 milliseconden. Reactietijden variëren op basis van de grootte of het paar aanvraag- en taalinstellingen.

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
| Thais | e | 258 |

> [!NOTE]
> Deze limiet niet van toepassing op vertalingen.

## <a name="next-steps"></a>Volgende stappen

* [Prijzen](https://azure.microsoft.com/pricing/details/cognitive-services/translator-text-api/)
* [Regionale beschikbaarheid](https://azure.microsoft.com/global-infrastructure/services/?products=cognitive-services)
* [V3 Translator Text-API-referentie](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference)
