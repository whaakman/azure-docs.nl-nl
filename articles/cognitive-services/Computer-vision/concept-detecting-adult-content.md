---
title: Met een beschrijving van erotische en ongepaste inhoud - Computer Vision
titleSuffix: Azure Cognitive Services
description: Concepten met betrekking tot het detecteren van erotische en ongepaste inhoud in afbeeldingen met behulp van de Computer Vision-APi.
services: cognitive-services
author: PatrickFarley
manager: cgronlun
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: conceptual
ms.date: 08/29/2018
ms.author: pafarley
ms.openlocfilehash: 71866149e3d2dca4b39585ce8da73aae658a4d59
ms.sourcegitcommit: 1aacea6bf8e31128c6d489fa6e614856cf89af19
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/16/2018
ms.locfileid: "49344911"
---
# <a name="detecting-adult-and-racy-content"></a>Erotische en ongepaste inhoud detecteren

Is de groep erotische en ongepaste, schakelt u het detecteren van volwassenen materialen en Hiermee beperkt u de weergave van afbeeldingen die seksuele inhoud bevat over de verschillende categorieën van de visual. Het filter voor de detectie van erotische en ongepaste inhoud kan worden ingesteld op een schaal van de schuifregelaar om voorkeur van de gebruiker mogelijk te maken.

## <a name="defining-adult-and-racy-content"></a>Erotische en ongepaste inhoud definiëren

Een van de verschillende visual functies wordt gedekt door de [analyseren installatiekopie methode](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa), de functie voor volwassenen visual schakelt u het detecteren van erotische en ongepaste afbeeldingen. "Volwassene" installatiekopieën worden gedefinieerd als die zijn pornografische van aard en vaak weer bloot en seksuele handelingen. 'Op' installatiekopieën worden gedefinieerd als de installatiekopieën die zijn seksueel suggestieve van aard en bevatten vaak minder seksueel getint inhoud dan installatiekopieën die zijn gemarkeerd als "Volwassene". Het type volwassenen visualfunctie wordt meestal gebruikt om te beperken van de weergave van afbeeldingen met seksueel suggestieve en expliciet seksuele inhoud.

## <a name="identifying-adult-and-racy-content"></a>Erotische en ongepaste inhoud identificeren

De afbeelding analyseren-methode retourneert twee eigenschappen, `isAdultContent` en `isRacyContent`, in de JSON-antwoord van de methode om aan te geven, respectievelijk erotische en ongepaste inhoud. Beide eigenschappen retourneren een Booleaanse waarde waar of ONWAAR. De methode retourneert ook twee eigenschappen, `adultScore` en `racyScore`, die staan voor, respectievelijk de scores vertrouwen voor het identificeren van erotische en ongepaste inhoud. Een filter vertrouwen voor de detectie van erotische en ongepaste inhoud kan worden ingesteld op een schaal van de schuifregelaar voor uw voorkeur op basis van uw specifieke scenario.

## <a name="next-steps"></a>Volgende stappen

Kennis met concepten over [domeinspecifieke inhoud detecteren](concept-detecting-domain-content.md) en [gezichten detecteren](concept-detecting-faces.md).