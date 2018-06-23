---
title: Microsoft Translator API teken telt | Microsoft Docs
description: De API van Microsoft Translator tekst telt hoe tekens.
services: cognitive-services
author: Jann-Skotdal
manager: chriswendt1
ms.service: cognitive-services
ms.component: translator-text
ms.topic: article
ms.date: 12/20/2017
ms.author: v-jansko
ms.openlocfilehash: ebe3e3606a0413730e1fbfd704a6403f77275f89
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344808"
---
# <a name="how-the-microsoft-translator-text-api-counts-characters"></a>Hoe de tekst-API van Microsoft Translator telt tekens

Microsoft Translator telt elke teken van de invoer. De tekens in de zin Unicode, niet bytes. Unicode vervangende aantal tekens als twee tekens. Witruimte en opmaak tellen als tekens. De lengte van het antwoord niet van belang.

Aanroepen van de analyse- en BreakSentence methoden worden niet meegeteld in het verbruik van het teken. Echter, we verwachten dat het aanroepen van de analyse- en BreakSentence methoden zijn in een redelijke verhouding tot het gebruik van andere functies die worden geteld. Microsoft behoudt zich het recht om te detecteren en BreakSentence wordt geteld. 

Automatische detectie van aanbiedingen vertalen als u de parameter From taal weglaat. 

Meer informatie over het aantal tekens is in de [Veelgestelde vragen over Microsoft Translator](https://www.microsoft.com/en-us/translator/faq.aspx).
