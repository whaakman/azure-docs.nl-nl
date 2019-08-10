---
title: Inhoud voor volwassenen en ongepaste-Computer Vision
titleSuffix: Azure Cognitive Services
description: Concepten met betrekking tot het detecteren van inhoud voor volwassenen en ongepaste in afbeeldingen met behulp van de Computer Vision-APi.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 02/08/2019
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: ca5f35ab47822d74de556671c38886942d23d9ff
ms.sourcegitcommit: 124c3112b94c951535e0be20a751150b79289594
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/10/2019
ms.locfileid: "68946285"
---
# <a name="detect-adult-and-racy-content"></a>Inhoud voor volwassenen en ongepaste detecteren

Computer Vision kunnen materiaal voor volwassenen in afbeeldingen detecteren zodat ontwikkel aars de weer gave van dergelijke installatie kopieën in hun software kunnen beperken. Inhouds vlaggen worden toegepast met een score tussen nul en één zodat ontwikkel aars de resultaten kunnen interpreteren op basis van hun eigen voor keuren. 

> [!NOTE]
> Deze functie wordt ook aangeboden door de [Azure content moderator](https://docs.microsoft.com/azure/cognitive-services/content-moderator/overview) -service. Bekijk dit alternatief voor oplossingen voor meer strenge toezicht scenario's voor inhoud, zoals tekst toezicht en Human Review-werk stromen.

## <a name="content-flag-definitions"></a>Definities van inhouds markeringen

Installatie kopieën voor **volwassenen** worden gedefinieerd als die voor een porno grafie en worden vaak genaakte en seksuele handelingen weer gegeven. 

**Ongepaste** -installatie kopieën worden gedefinieerd als installatie kopieën die expliciet worden voorgesteld en bevatten vaak minder expliciete inhoud dan afbeeldingen die als volwasseneworden gelabeld. 

## <a name="identify-adult-and-racy-content"></a>Inhoud voor volwassenen en ongepaste identificeren

De [analyse](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa) -API.

De methode voor het analyseren van afbeeldingen retourneert twee `isAdultContent` Booleaanse `isRacyContent`eigenschappen, en in het JSON-antwoord van de methode om respectievelijk inhoud van volwassene en ongepaste aan te geven. De methode retourneert ook twee eigenschappen `adultScore` en `racyScore`, die de betrouwbaarheids scores vertegenwoordigen voor respectievelijk het identificeren van volwassene en ongepaste-inhoud.

## <a name="next-steps"></a>Volgende stappen

Leer concepten over [het detecteren van serverspecifieke inhoud](concept-detecting-domain-content.md) en het [detecteren van gezichten](concept-detecting-faces.md).
