---
title: Teken aantallen - Translator Text-API
titlesuffix: Azure Cognitive Services
description: De tekst-API van Translator telt hoe tekens.
services: cognitive-services
author: Jann-Skotdal
manager: cgronlun
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: conceptual
ms.date: 12/20/2017
ms.author: v-jansko
ms.openlocfilehash: c56622ee5e25fc422d02cec6ecfaa1f847e9e769
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55226431"
---
# <a name="how-the-translator-text-api-counts-characters"></a>Hoe de tekst-API van Translator telt tekens

De tekst-API van Translator telt elke teken van de invoer. De tekens in de zin Unicode, niet in bytes. Unicode vervangende aantal tekens als twee tekens. Witruimte en markeringen tellen als tekens. De lengte van het antwoord niet van belang.

Aanroepen van de analyse en BreakSentence methoden worden niet meegeteld in het verbruik van het teken. Echter, we verwachten dat de aanroepen van de analyse en BreakSentence methoden zijn in een redelijke verhouding tot het gebruik van andere functies die worden geteld. Microsoft behoudt zich het recht om te detecteren en BreakSentence wordt geteld.

Meer informatie over het aantal tekens is in de [Veelgestelde vragen over Microsoft Translator](https://www.microsoft.com/en-us/translator/faq.aspx).
