---
title: Spraak naar tekst | Microsoft Docs
description: Een overzicht van de mogelijkheden van spraak naar tekst-API.
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: v-jerkin
manager: noellelacharite
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 05/07/2018
ms.author: v-jerkin
ms.openlocfilehash: 42552fa09f0a433cf12b92e11531303a0d1bd6bb
ms.sourcegitcommit: 756f866be058a8223332d91c86139eb7edea80cc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/02/2018
ms.locfileid: "37346508"
---
# <a name="about-the-speech-to-text-api"></a>Informatie over de Speech to Text-API

De **spraak naar tekst** API *transcribes* audiostreams naar tekst die uw toepassing kan worden weergegeven voor de gebruiker of bij het fungeren als invoer opdracht. De API's kunnen worden gebruikt met een SDK-clientbibliotheek (voor ondersteunde platforms en talen) of een REST-API.

De **spraak naar tekst** API biedt de volgende functies:

- Geavanceerde spraakherkenningstechnologie van Microsoft, hetzelfde zijn als voor Cortana, Office en andere Microsoft-producten.

- Realtime continue herkenning. **Spraak naar tekst** kunnen gebruikers audio naar tekst in realtime te transcriberen. Het ondersteunt ook tussenliggende resultaten van de woorden die tot nu toe zijn herkend. De service automatisch worden herkend als het einde van spraak. Gebruikers kunnen ook aanvullende opmaakopties, met inbegrip van hoofdletters en interpunctie, grof taalgebruik maskeren en inverse tekst normalisering kiezen.

- Geoptimaliseerde **spraak naar tekst** resultaten voor interactieve, conversatie en dicteren scenario's. 

- Ondersteuning van verschillende talen en dialecten. Zie voor de volledige lijst van ondersteunde talen in beide modi erkenning, [ondersteunde talen](supported-languages.md#speech-to-text).

- Aangepaste taal en akoestische modellen, zodat u uw toepassing op het vocabulaire van uw gebruikers gespecialiseerde domein, spreekstijl omgeving en manier aanpassen kunt van spreken.

- Natuurlijke taal begrijpen. Dankzij de integratie met [Language Understanding](https://docs.microsoft.com/azure/cognitive-services/luis/) (LUIS), u kunt afleiden intenties en entiteiten van spraak. Gebruikers hoeft te weten vocabulaire van uw app, maar wordt beschreven wat ze willen in hun eigen woorden.

## <a name="api-capabilities"></a>API-functies

Bepaalde functies van de **spraak naar tekst** API zijn niet beschikbaar via REST. De volgende tabel geeft een overzicht van de mogelijkheden van elke methode van de toegang tot de API.

| Use-case | REST | SDK's |
|-----|-----|-----|----|
| Een korte utterance, zoals een opdracht (lengte < 15 s); transcriberen Er zijn geen tijdelijke resultaten | Ja | Ja |
| Transcriberen een langer utterance (> 15 s) | Nee | Ja |
| Streaming audio met optionele tussentijdse resultaten transcriberen | Nee | Ja |
| Sprekerherkenning intents via LUIS begrijpen | Nee\* | Ja |

\* *LUIS intenties en entiteiten kunnen worden afgeleid met behulp van een afzonderlijk LUIS-abonnement. Met dit abonnement, de SDK LUIS-aanroep voor u en bieden entiteit en voor welk doel resultaten, evenals transcriptie van spraak. Met de REST-API, roept u LUIS zelf voor het afleiden van intenties en entiteiten met uw LUIS-abonnement.*

## <a name="next-steps"></a>Volgende stappen

* [Uw proefabonnement voor spraak ophalen](https://azure.microsoft.com/try/cognitive-services/)
* [Zie voor het herkennen van gesproken tekst in C#](quickstart-csharp-windows.md)
