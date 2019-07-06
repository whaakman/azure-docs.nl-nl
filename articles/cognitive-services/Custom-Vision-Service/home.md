---
title: Wat is Azure Custom Vision?
titlesuffix: Azure Cognitive Services
description: Leer hoe u de Custom Vision-service kunt gebruiken om aangepaste classificaties voor afbeeldingen te bouwen in de Azure-cloud.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: overview
ms.date: 07/03/2019
ms.author: pafarley
ms.openlocfilehash: 8a5dd16948724e3a79863450212702aa8aeb2347
ms.sourcegitcommit: f10ae7078e477531af5b61a7fe64ab0e389830e8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/05/2019
ms.locfileid: "67605145"
---
# <a name="what-is-azure-custom-vision"></a>Wat is Azure Custom Vision?

Azure Custom Vision is een cognitive service waarmee u bouwen, implementeren en uw eigen beeldclassificeerders te verbeteren. Een afbeeldingsclassificatie is een AI-service die op basis van de visuele kenmerken van een afbeelding labels (die _klassen_ vertegenwoordigen) op afbeeldingen toepast. Anders dan bij de [Computer Vision](https://docs.microsoft.com/azure/cognitive-services/computer-vision/home)-service kunt u met Custom Vision bepalen welke labels u wilt toepassen.

## <a name="what-it-does"></a>Wat het doet

De Custom Vision-service gebruikt een machine learning-algoritme om labels toe te passen op afbeeldingen. U, als ontwikkelaar, moet groepen afbeeldingen indienen die de betreffende kenmerken al dan niet hebben. U voorziet de afbeeldingen op het moment van inzending zelf van een label. Vervolgens wordt het algoritme getraind op basis van deze gegevens en berekent het algoritme de eigen nauwkeurigheid aan de hand van deze zelfde gegevens. Wanneer het algoritme is getraind, kunt u het model testen, opnieuw trainen en uiteindelijk gebruiken om nieuwe afbeeldingen te classificeren op basis van de behoeften van de app. U kunt het model zelf ook exporteren voor offlinegebruik.

### <a name="classification-and-object-detection"></a>Classificatie- en objectdetectie

Custom Vision-functionaliteit kan worden onderverdeeld in twee functies. Met **afbeeldingsclassificatie** worden een of meer labels toegepast op een afbeelding. **Objectdetectie** is vergelijkbaar, maar hiermee worden ook de coördinaten van de afbeelding geretourneerd waar de toegepaste labels kunnen worden gevonden.

### <a name="optimization"></a>Optimalisatie

De Custom Vision service is geoptimaliseerd voor het snel herkent belangrijke verschillen tussen de afbeeldingen, zodat u maken van een prototype uw model met een kleine hoeveelheid gegevens beginnen kunt. 50 afbeeldingen per label is meestal al een goed begin. De service is echter niet optimaal zijn voor het detecteren van subtiele verschillen in afbeeldingen (bijvoorbeeld als u kleine meer tussendoor of deuken detecteren in kwaliteit assurance scenario's).

Daarnaast kunt u kiezen uit verschillende Custom Vision-algoritmen die zijn geoptimaliseerd voor afbeeldingen met bepaalde onderwerpen&mdash;bijvoorbeeld oriëntatiepunten of detailhandelsartikelen. Zie voor meer informatie de [een classificatie maken](getting-started-build-a-classifier.md) handleiding.

## <a name="what-it-includes"></a>Samenstelling van Content Moderator

De Custom Vision Service is beschikbaar als een set met systeemeigen SDK’s of via een webinterface op de [Custom Vision-startpagina](https://customvision.ai/). U kunt een model maken, testen en trainen via een van de interfaces of beide samen gebruiken.

![Custom Vision-startpagina in een Chrome-browservenster](media/browser-home.png)

## <a name="data-privacy-and-security"></a>Gegevensprivacy en -beveiliging

Zoals geldt voor alle Cognitive Services, dienen ontwikkelaars die de Custom Vision-service gebruiken op de hoogte te zijn van het beleid van Microsoft inzake klantgegevens. Zie de [pagina Cognitive Services](https://www.microsoft.com/trustcenter/cloudservices/cognitiveservices) (Engelstalig) in het Microsoft Trust Center voor meer informatie.

## <a name="next-steps"></a>Volgende stappen

Volg de handleiding [Een classificatie bouwen](getting-started-build-a-classifier.md) om aan de slag te gaan met Custom Vision op het web, of voltooi een [zelfstudie voor afbeeldingsclassificatie](csharp-tutorial.md) om een basisscenario in code te implementeren.
