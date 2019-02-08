---
title: Teken aantallen - Translator Text-API
titlesuffix: Azure Cognitive Services
description: De tekst-API van Translator telt hoe tekens.
services: cognitive-services
author: Jann-Skotdal
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: conceptual
ms.date: 02/01/2019
ms.author: v-jansko
ms.openlocfilehash: 2164070c8f0efe2898e3780cac30a80c9f1ca986
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55874410"
---
# <a name="how-the-translator-text-api-counts-characters"></a>Hoe de tekst-API van Translator telt tekens

De tekst-API van Translator telt elke codepunt voor Unicode van invoertekst als een teken. Elke omzetting van een tekst naar een andere taal telt als een afzonderlijke vertaling, zelfs als de aanvraag is gedaan in een enkele API aanroep vertalen naar meerdere talen. De lengte van het antwoord niet van belang.

Wat telt is:

* Tekst die wordt doorgegeven naar de Translator Text-API in de hoofdtekst van de aanvraag
   * `Text` bij het gebruik van de methoden vertalen, Transliterate en woordenboek
   * `Text` en `Translation` bij het gebruik van de methode Dictionary-voorbeelden
* Alle aantekeningen: HTML, XML-tags, enz. in het tekstveld van de aanvraagtekst. JSON-notatie gebruikt voor het bouwen van de aanvraag (bijvoorbeeld ' tekst: ') wordt niet meegerekend.
* Een afzonderlijke letter
* Leestekens
* Een spatie, tab, aantekeningen en elk soort spatie
* Elk codepunt is gedefinieerd in Unicode
* Een herhaalde vertaling, zelfs als u dezelfde tekst eerder zijn vertaald

Voor scripts op basis van weer te geven ideogrammen zoals Chinees en Japans Kanji, telt de Translator Text-API nog steeds het aantal codepunten Unicode, één teken per ideogram. Uitzondering: Unicode vervangende aantal tekens als twee tekens.

Het aantal aanvragen, woorden, bytes of zinnen is niet van belang in het aantal tekens. 

Aanroepen van de analyse en BreakSentence methoden worden niet meegeteld in het verbruik van het teken. Echter, we verwachten dat de aanroepen van de analyse en BreakSentence methoden zijn in een redelijke verhouding tot het gebruik van andere functies die worden geteld. Als het aantal detecteren of BreakSentence aanroepen die u aanbrengt groter is dan het aantal getelde methoden door 100 keer Microsoft behoudt zich het recht om uw gebruik van de analyse en BreakSentence methoden te beperken.


Meer informatie over het aantal tekens is in de [Veelgestelde vragen over Microsoft Translator](https://www.microsoft.com/en-us/translator/faq.aspx).
