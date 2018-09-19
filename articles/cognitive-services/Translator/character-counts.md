---
title: Translator Text-API teken geteld
titlesuffix: Azure Cognitive Services
description: De tekst-API van Translator telt hoe tekens.
services: cognitive-services
author: Jann-Skotdal
manager: cgronlun
ms.service: cognitive-services
ms.component: translator-text
ms.topic: conceptual
ms.date: 12/20/2017
ms.author: v-jansko
ms.openlocfilehash: c6234a46ae55d73739dcc23110c5e0f6375c3f96
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/18/2018
ms.locfileid: "46128737"
---
# <a name="how-the-translator-text-api-counts-characters"></a>Hoe de tekst-API van Translator telt tekens

De tekst-API van Translator telt elke teken van de invoer. De tekens in de zin Unicode, niet in bytes. Unicode vervangende aantal tekens als twee tekens. Witruimte en markeringen tellen als tekens. De lengte van het antwoord niet van belang.

Aanroepen van de analyse en BreakSentence methoden worden niet meegeteld in het verbruik van het teken. Echter, we verwachten dat de aanroepen van de analyse en BreakSentence methoden zijn in een redelijke verhouding tot het gebruik van andere functies die worden geteld. Microsoft behoudt zich het recht om te detecteren en BreakSentence wordt geteld. 

Meer informatie over het aantal tekens is in de [Veelgestelde vragen over Microsoft Translator](https://www.microsoft.com/en-us/translator/faq.aspx).
