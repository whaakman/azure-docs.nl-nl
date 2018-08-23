---
title: Microsoft Translator Text-API teken geteld | Microsoft Docs
description: De Microsoft Translator Text-API telt hoe tekens.
services: cognitive-services
author: Jann-Skotdal
manager: chriswendt1
ms.service: cognitive-services
ms.component: translator-text
ms.topic: article
ms.date: 12/20/2017
ms.author: v-jansko
ms.openlocfilehash: 1b4987509c17e4064d7c54608395e272efa8de3b
ms.sourcegitcommit: 744747d828e1ab937b0d6df358127fcf6965f8c8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41987575"
---
# <a name="how-the-microsoft-translator-text-api-counts-characters"></a>Hoe tekens in de Microsoft Translator Text-API wordt geteld

Microsoft Translator telt elke teken van de invoer. De tekens in de zin Unicode, niet in bytes. Unicode vervangende aantal tekens als twee tekens. Witruimte en markeringen tellen als tekens. De lengte van het antwoord niet van belang.

Aanroepen van de analyse en BreakSentence methoden worden niet meegeteld in het verbruik van het teken. Echter, we verwachten dat de aanroepen van de analyse en BreakSentence methoden zijn in een redelijke verhouding tot het gebruik van andere functies die worden geteld. Microsoft behoudt zich het recht om te detecteren en BreakSentence wordt geteld. 

Meer informatie over het aantal tekens is in de [Veelgestelde vragen over Microsoft Translator](https://www.microsoft.com/en-us/translator/faq.aspx).
