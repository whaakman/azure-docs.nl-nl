---
title: Spraak naar tekst
description: Een overzicht van de mogelijkheden van spraak naar tekst-API.
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: v-jerkin
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 05/07/2018
ms.author: v-jerkin
ms.openlocfilehash: ba6710c8b5b8de1c63fa6778ea3853ab52365254
ms.sourcegitcommit: 7ad9db3d5f5fd35cfaa9f0735e8c0187b9c32ab1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/27/2018
ms.locfileid: "39325333"
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

* [Uw proefabonnement voor Speech ophalen](https://azure.microsoft.com/try/cognitive-services/)
* [Snelstartgids: herkennen gesproken tekst in C#](quickstart-csharp-dotnet-windows.md)
* [Zie voor het herkennen van intenties van gesproken tekst in C#](how-to-recognize-intents-from-speech-csharp.md)
