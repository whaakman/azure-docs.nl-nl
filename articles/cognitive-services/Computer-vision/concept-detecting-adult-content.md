---
title: Beschrijf erotische en ongepaste inhoud - Computer Vision
titleSuffix: Azure Cognitive Services
description: Concepten met betrekking tot het detecteren van erotische en ongepaste inhoud in afbeeldingen met behulp van de Computer Vision-APi.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 02/08/2019
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: 69a4c136e9c210dd40e004b8d5e1c1a2a8fceaa7
ms.sourcegitcommit: f7be3cff2cca149e57aa967e5310eeb0b51f7c77
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/15/2019
ms.locfileid: "56312524"
---
# <a name="detect-adult-and-racy-content"></a>Erotische en ongepaste inhoud detecteren

Computer Vision kunt pornografisch materiaal in afbeeldingen detecteren, zodat ontwikkelaars de weergave van dergelijke beelden in hun software kunnen beperken. Inhoud vlaggen worden toegepast met een score tussen 0 en 1, zodat ontwikkelaars kunnen de resultaten op basis van hun eigen voorkeuren interpreteren. 

> [!NOTE]
> Deze functie wordt ook aangeboden door de [Azure Content Moderator](https://docs.microsoft.com/azure/cognitive-services/content-moderator/overview) service. Zie dit alternatief voor oplossingen voor strengere inhoudstoezicht-scenario's, zoals beheer van tekst en werkstromen voor menselijke beoordeling.

## <a name="content-flag-definitions"></a>Inhoud vlag definities

**Volwassenen** installatiekopieën zijn gedefinieerd als die welke pornografische van aard en vaak bloot en seksuele besluiten weer. 

**Ongepaste** installatiekopieën zijn gedefinieerd zoals installatiekopieën die seksueel suggestieve van aard en vaak zijn minder seksueel getint inhoud dan installatiekopieën die zijn gemarkeerd als bevatten **volwassenen**. 

## <a name="identify-adult-and-racy-content"></a>Erotische en ongepaste inhoud identificeren

De [analyseren](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa) API.

De afbeelding analyseren-methode retourneert twee Booleaanse eigenschappen, `isAdultContent` en `isRacyContent`, in de JSON-antwoord van de methode om aan te geven erotische en ongepaste inhoud respectievelijk. De methode retourneert ook twee eigenschappen, `adultScore` en `racyScore`, die staan voor de scores vertrouwen voor het identificeren van erotische en ongepaste inhoud respectievelijk.

## <a name="next-steps"></a>Volgende stappen

Kennis met concepten over [domeinspecifieke inhoud detecteren](concept-detecting-domain-content.md) en [gezichten detecteren](concept-detecting-faces.md).
