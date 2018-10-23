---
title: Teken aantallen - Translator Text-API
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
ms.openlocfilehash: 8e04158d34765b8a077fc56f2108ea6d7b4b03a6
ms.sourcegitcommit: ccdea744097d1ad196b605ffae2d09141d9c0bd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/23/2018
ms.locfileid: "49649191"
---
# <a name="how-the-translator-text-api-counts-characters"></a>Hoe de tekst-API van Translator telt tekens

De tekst-API van Translator telt elke teken van de invoer. De tekens in de zin Unicode, niet in bytes. Unicode vervangende aantal tekens als twee tekens. Witruimte en markeringen tellen als tekens. De lengte van het antwoord niet van belang.

Aanroepen van de analyse en BreakSentence methoden worden niet meegeteld in het verbruik van het teken. Echter, we verwachten dat de aanroepen van de analyse en BreakSentence methoden zijn in een redelijke verhouding tot het gebruik van andere functies die worden geteld. Microsoft behoudt zich het recht om te detecteren en BreakSentence wordt geteld.

Meer informatie over het aantal tekens is in de [Veelgestelde vragen over Microsoft Translator](https://www.microsoft.com/en-us/translator/faq.aspx).
